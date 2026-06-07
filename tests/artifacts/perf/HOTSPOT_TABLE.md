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
