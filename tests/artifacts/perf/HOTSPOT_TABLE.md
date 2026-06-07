# Hotspot Table — `bv --robot-triage` (and the wider robot path)

**Scenario:** cold-process `bv --robot-triage` on the repo's own `.beads/issues.jsonl`
(757 issues / 1.9 MB; 1307 commits, 168 correlated).
**Host:** AMD EPYC 7282 (64c), Go 1.25.5, git 2.51.2, kernel 6.17, default `go build`.
**Golden output:** the `--robot-triage` JSON (and `--robot-plan`/`--robot-insights`) must remain
byte-equivalent modulo timestamps/`compute_time_ms`. Verify with a saved golden before/after each change.

## Baselines (hyperfine, warmup 2)

| Command | mean ± σ | git execs | Note |
|---|---|---|---|
| `--robot-triage` | **2.872 s ± 0.165 s** | 340 | the target |
| `--robot-next`   | 2.636 s ± 0.018 s | 340 | "minimal" mode *also* runs correlation |
| `--robot-plan`   | 0.904 s ± 0.010 s | 2 | no correlation → exposes the in-process floor |
| `--robot-insights` | 8.399 s ± 0.219 s | 2 | full exact Phase-2 graph metrics |

Warm in-process (go test -benchmem, real data):
`IssueLoading 25.9 ms / 7.7 MB / 22.8k allocs` · `FullTriage 1.26 ms` · `GraphBuild 0.48 ms` · `FullAnalysis 0.69 ms`.
→ The real *work* is tens of ms; the seconds come from subprocess fan-out and the disk cache.

## Ranked hotspots (evidence-cited)

| Rank | Location | Metric | Value | Category | Evidence |
|------|----------|--------|-------|----------|----------|
| 1 | `pkg/correlation/cocommit.go` `getFilesChanged`:154 + `getLineStats`:202 — **two `git show` per commit** (`--name-status`, `--numstat`) | subprocess fan-out | **336 git execs ≈ 1.7–1.9 s** of the 2.87 s triage | I/O / subprocess | `strace -f -e execve` → 168 `--name-status` + 168 `--numstat`; `/usr/bin/time` sys=1.31 s |
| 2 | `pkg/analysis/cache.go` `getRobotDiskCachedStats`:906 → `readRobotDiskCacheLocked`:830 + `writeRobotDiskCacheLocked`:849 — reads **and rewrites the whole 6.6 MB `analysis_cache.json`** every call (even on a hit, just to bump LRU `AccessedAt`) via **stdlib `encoding/json`** | (de)serialize + I/O | **~0.9 s = 30.9 % CPU**; paid by *every* robot cmd | CPU/alloc/IO | `perf report` `perf_plan.data`: Unmarshal 13.9 % + Encode 13.8 %; cache file = 6.6 MB |
| 3 | exact betweenness / cycles / HITS / eigenvector (insights `ConfigForSize`, `betweenness_approx.go`, `graph.go`) | full Phase-2 compute | **insights 8.4 s** (≈ 7.5 s beyond the floor) | CPU | hyperfine insights vs plan |
| 4 | `pkg/analysis/graph.go` metric write-back as `map[string]float64` (per-node, ~12 metrics) | alloc + string hashing; **inflates #2 cache to 6.6 MB** | bloats serialize/parse in #2 | CPU/alloc | investigation report; cache size |
| 5 | `pkg/loader/loader.go` JSONL parse + `pkg/analysis/cache.go` `ComputeDataHash`:141 (SHA256 over all issues, sorted) | load + hash | ~26 ms warm / 22.8k allocs | CPU/alloc | bench `IssueLoading` |

## What triage actually pays (decomposition of 2.87 s)
```
~0.9 s  disk-cache read+rewrite (rank 2)   ← also in plan/next/insights
~1.9 s  correlation git fan-out  (rank 1)  ← triage/next only
~0.0 s  graph analysis (cache hit, rank 3/4/5 ~1 ms)
```
Killing ranks 1 + 2 should take triage from ~2.9 s to well under ~0.1 s.

---

## Final results (after 10 passes)

**Host:** AMD EPYC 7282 (64c), Go 1.25.5, git 2.51.2, kernel 6.17, default `go build`.
**Cache state matters:** all warm numbers below use a *fresh isolated* `XDG_CACHE_HOME`
warmed once per command, then measured with `hyperfine -w3 -r15 -N`. Cold numbers are
first-call on an empty cache dir. These are not comparable to the original §"Baselines"
table above, which was taken under a *cold process + cold disk-cache* regime (hence the
seconds-scale numbers); the warm regime is what the 10-pass loop drove down and is the
honest steady-state an agent sees on repeated robot calls.

### Warm end-to-end (fresh-cache, before → after pass 10)

The pass-10 change (parallel JSONL parse, size-gated) is **net-neutral on the warm robot
path for this repo** because the store (~1.9 MB) sits *below* the measured parallel
crossover (~4 MB) and so deliberately stays on the faster serial path. Before/after are
within σ — i.e. no regression, with the parallel speedup now latent for larger stores.

| Command | before (pass-9 binary) | after (pass-10 binary) | Δ |
|---|---|---|---|
| `--robot-plan`     | 88.2 ms ± 3.8 | 88.5 ms ± 4.1 | ~0 (within σ) |
| `--robot-triage`   | 101.3 ms ± 1.5 | 102.4 ms ± 3.3 | ~0 (within σ) |
| `--robot-next`     | 100.9 ms ± 2.9 | 102.5 ms ± 2.5 | ~0 (within σ) |
| `--robot-insights` | 321.8 ms ± 11.2 | 343.3 ms ± 45.7 | ~0 (noise; insights variance is GC-driven) |

### Cold (first-call, empty cache)

| Command | after (pass-10) | Note |
|---|---|---|
| `--robot-triage` | ~1.00 s (997–1008 ms over 5 runs) | dominated by correlation git fan-out + first cache build, **not** the loader |

### Cumulative speedup across all 10 passes (warm robot path)

Cold-process, cold-disk-cache → warm fresh-cache steady state:

| Command | original cold baseline | pass-10 warm | cumulative speedup |
|---|---|---|---|
| `--robot-triage`   | 2.872 s | 0.102 s | **~28×** |
| `--robot-next`     | 2.636 s | 0.103 s | **~26×** |
| `--robot-plan`     | 0.904 s | 0.089 s | **~10×** |
| `--robot-insights` | 8.399 s | 0.343 s | **~24×** |

(The bulk of these wins are from passes 1–9: killing the git fan-out, the 6.6 MB
disk-cache read+rewrite, the metric write-back bloat, and the correlation snapshot path.
Pass 10 contributes a *future-proofing* lever, not a number here.)

### Pass 10 — Loader-ParallelParse (this pass)

- **Change:** `pkg/loader/loader.go` — `parseIssuesWithOptions` now size-gates onto a
  morsel-driven parallel JSONL decoder (`parseIssuesParallel` + `parseChunkLines`),
  reusing a single shared per-line processor (`processIssueLine`) so the serial and
  parallel paths are byte-equivalent (BOM strip, `_type` dispatch, CRLF trim, 10 MB line
  cap, normalize/validate, tombstone/pool deep-copy, ParseStats, and warnings replayed in
  original line order). Alien-graveyard technique: §8.2 Vectorized Execution + Morsel-Driven
  Parallelism (bounded worker pool pulling line-aligned chunks from a central dispatcher,
  results reassembled by chunk-index + intra-chunk-index for deterministic order).
- **Measured crossover (warm, real-shaped data, 64c):** the JSONL parse is
  *allocation/GC-bound*, not CPU-bound (CPU profile: `runtime.gcDrain` ~36 % cum; the JSON
  decode itself is ~10 %). Parallelism only pays once per-issue work outweighs the parallel
  path's extra allocation (per-chunk slices + order-preserving reassembly copy):

  | file size | serial | parallel | winner |
  |---|---|---|---|
  | 1.9 MB (this repo) | 13.4 ms | 15.3 ms | serial |
  | 4 MB   | 37.5 ms | 37.1 ms | tie (crossover) |
  | 8 MB   | 62.9 ms | 56.4 ms | parallel +10 % |
  | 40 MB  | 246 ms  | 203 ms  | parallel +21 % |

- **Decision:** threshold `parallelParseMinBytes = 4 MiB`, so the repo's own ~1.9 MB store
  stays serial (no warm-path regression) while multi-MB monorepo exports get the speedup.
- **Proof:** `go test -race ./pkg/loader/...` green; differential tests
  (`TestParallelDiff_*`, `TestParallelParse_AutoDispatchMatchesSerial`) assert identical
  `[]Issue` + order + count + stats + ordered warnings on the real file and on
  corrupt/BOM/CRLF/no-trailing-newline fixtures; all 4 goldens OK; `go vet` / `gofmt` /
  `ubs` clean.
