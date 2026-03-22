# Changelog

All notable changes to **bv** (beads_viewer) are documented here. bv is a graph-aware triage engine for [Beads](https://github.com/Dicklesworthstone/beads_rust) projects, providing TUI-based visualization, robot-mode JSON commands for agents, and static HTML export.

Versions marked with a release link have published [GitHub Releases](https://github.com/Dicklesworthstone/beads_viewer/releases) with pre-built binaries. Plain tag links denote source-only tags.

---

## [Unreleased](https://github.com/Dicklesworthstone/beads_viewer/compare/v0.15.2...HEAD)

### Editor dispatch and stability

- **Smart terminal editor dispatch** via `O` key -- opens the selected bead in `$EDITOR` with YAML frontmatter for inline editing ([550f3bd](https://github.com/Dicklesworthstone/beads_viewer/commit/550f3bd)).
- Fix YAML frontmatter escaping: all fields are now YAML-escaped, not just title ([90aa46e](https://github.com/Dicklesworthstone/beads_viewer/commit/90aa46e)).
- Fix three editor dispatch bugs: body whitespace handling, missing issue guard, labels field ([c0f670b](https://github.com/Dicklesworthstone/beads_viewer/commit/c0f670b)).
- Guard against negative `strings.Repeat` and nil `Process` check in truncation helpers ([a0a35ee](https://github.com/Dicklesworthstone/beads_viewer/commit/a0a35ee), [816f9c3](https://github.com/Dicklesworthstone/beads_viewer/commit/816f9c3)).
- Preserve issue deep-links during cold load filter sync ([81a1983](https://github.com/Dicklesworthstone/beads_viewer/commit/81a1983)).

---

## [v0.15.2](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.15.2) -- 2026-03-09

### Cloudflare Pages auth fix

- Check all wrangler config paths and handle refresh tokens to fix deployment on headless servers ([cf001ba](https://github.com/Dicklesworthstone/beads_viewer/commit/cf001ba)).

---

## [v0.15.1](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.15.1) -- 2026-03-09

### Wrangler headless hang fix

- Fix wrangler auth check hanging on headless servers during `--pages` export ([4cc8635](https://github.com/Dicklesworthstone/beads_viewer/commit/4cc8635)).

---

## [v0.15.0](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.15.0) -- 2026-03-08

### Major release: deployment, bd-to-br migration, and terminal compatibility

**Deployment and export:**
- GitHub Pages and Cloudflare Pages deployment support via `--export-pages` ([e60384b](https://github.com/Dicklesworthstone/beads_viewer/commit/e60384b)).
- Recipe filtering now applies before robot modes, fixing `--recipe actionable --robot-plan` combos ([dc6bfab](https://github.com/Dicklesworthstone/beads_viewer/commit/dc6bfab)).

**bd-to-br migration:**
- Complete migration from legacy `bd` command references to `br` across the entire codebase ([f9ba482](https://github.com/Dicklesworthstone/beads_viewer/commit/f9ba482), [6bce598](https://github.com/Dicklesworthstone/beads_viewer/commit/6bce598)).
- Normalize robot output envelope across all commands ([23172a1](https://github.com/Dicklesworthstone/beads_viewer/commit/23172a1)).
- Error messages now reference `br` instead of deprecated `bd` ([ca5da4e](https://github.com/Dicklesworthstone/beads_viewer/commit/ca5da4e)).

**Terminal compatibility:**
- Color-profile-aware styling for Solarized and 16-color terminals ([cbbcb1f](https://github.com/Dicklesworthstone/beads_viewer/commit/cbbcb1f)).
- Use terminal default background to prevent ANSI color mismap ([2599cce](https://github.com/Dicklesworthstone/beads_viewer/commit/2599cce)).
- Improve footer text contrast across terminal themes ([271cb10](https://github.com/Dicklesworthstone/beads_viewer/commit/271cb10)).

**Version and configuration:**
- Multi-source version detection with graceful fallback (ldflags, `pkg/version`, git describe) ([ede65f2](https://github.com/Dicklesworthstone/beads_viewer/commit/ede65f2)).
- `--db` flag and `BEADS_DB` env var for configuring database path ([b56ddae](https://github.com/Dicklesworthstone/beads_viewer/commit/b56ddae)).
- Migrate from Go `flag` to `pflag` for POSIX double-dash options ([064b3d0](https://github.com/Dicklesworthstone/beads_viewer/commit/064b3d0)).

**Agent integration:**
- `--agents-*` CLI flags for managing AGENTS.md blurb injection ([8e9c656](https://github.com/Dicklesworthstone/beads_viewer/commit/8e9c656)).
- Draft/review statuses and upgraded agent blurb to v2 ([ce542b3](https://github.com/Dicklesworthstone/beads_viewer/commit/ce542b3)).
- Status color mappings for deferred, draft, pinned, hooked, review, and tombstone ([42d69f7](https://github.com/Dicklesworthstone/beads_viewer/commit/42d69f7)).

**Board view fixes:**
- Column width calculation fix to prevent line rendering glitch ([08eb523](https://github.com/Dicklesworthstone/beads_viewer/commit/08eb523)).
- Allow board columns to shrink below 12 chars on very narrow terminals ([e50be8a](https://github.com/Dicklesworthstone/beads_viewer/commit/e50be8a)).
- Correct board detail panel box drawing ([2ff6cab](https://github.com/Dicklesworthstone/beads_viewer/commit/2ff6cab)).

**Security:**
- Scope GitHub token to `github.com` domains to prevent credential leaking on redirects ([ccd23d0](https://github.com/Dicklesworthstone/beads_viewer/commit/ccd23d0)).
- Trim whitespace from GitHub token env vars to prevent 401 errors ([a148823](https://github.com/Dicklesworthstone/beads_viewer/commit/a148823)).
- `GITHUB_TOKEN` support for self-update ([2ff6cab](https://github.com/Dicklesworthstone/beads_viewer/commit/2ff6cab)).

**Other:**
- Transitive parent-blocked check in `GetActionableIssues` ([b14e9c4](https://github.com/Dicklesworthstone/beads_viewer/commit/b14e9c4)).
- Read labels from separate labels table for `br`/beads-rs compatibility ([19437c4](https://github.com/Dicklesworthstone/beads_viewer/commit/19437c4)).
- Invalidate robot triage disk cache when `.beads/` directory changes ([9464db4](https://github.com/Dicklesworthstone/beads_viewer/commit/9464db4)).
- License updated to MIT with OpenAI/Anthropic Rider ([81c2b94](https://github.com/Dicklesworthstone/beads_viewer/commit/81c2b94)).

---

## [v0.14.4](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.14.4) -- 2026-02-03

### Expanded robot-mode commands

- Expand robot-mode commands with additional outputs and enhanced wizard support ([ae2e2e7](https://github.com/Dicklesworthstone/beads_viewer/commit/ae2e2e7), [15e72df](https://github.com/Dicklesworthstone/beads_viewer/commit/15e72df)).

---

## [v0.14.3](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.14.3) -- 2026-02-03

### XSS and cache fixes

- XSS-escape title in HTML export, wrap errors properly, improve OPFS cache cleanup ([005a220](https://github.com/Dicklesworthstone/beads_viewer/commit/005a220)).
- Ensure SHA-256 hash is always computed for OPFS cache invalidation ([d263a78](https://github.com/Dicklesworthstone/beads_viewer/commit/d263a78)).

---

## [v0.14.2](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.14.2) -- 2026-02-03

### Cache-busting for GitHub Pages

- Add cache-busting to HTML script tags to prevent stale data after GitHub Pages updates ([fcf1b7f](https://github.com/Dicklesworthstone/beads_viewer/commit/fcf1b7f)).

---

## [v0.14.1](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.14.1) -- 2026-02-03

### Resizable split view and robot-docs

- Resizable split view panes in the TUI ([acf7568](https://github.com/Dicklesworthstone/beads_viewer/commit/acf7568)).
- `--robot-docs` command for machine-readable documentation export ([0c9f6a3](https://github.com/Dicklesworthstone/beads_viewer/commit/0c9f6a3)).
- Cache-busting to prevent stale data on GitHub Pages updates ([5cfd94a](https://github.com/Dicklesworthstone/beads_viewer/commit/5cfd94a)).

---

## [v0.14.0](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.14.0) -- 2026-02-02

### Major release: live-reload, TOON format, smart data sources, and performance

**Live-reload and preview:**
- Live-reload via Server-Sent Events for instant browser refresh during `--preview-pages` ([4b6a095](https://github.com/Dicklesworthstone/beads_viewer/commit/4b6a095)).
- Fix SSE script injection with Content-Length deletion and buffered content flushing ([9a72ba7](https://github.com/Dicklesworthstone/beads_viewer/commit/9a72ba7), [2b46353](https://github.com/Dicklesworthstone/beads_viewer/commit/2b46353)).

**TOON format support:**
- `--format json|toon` and `--stats` flags for robot output in TOON encoding ([4f5f032](https://github.com/Dicklesworthstone/beads_viewer/commit/4f5f032), [ebfeb6f](https://github.com/Dicklesworthstone/beads_viewer/commit/ebfeb6f)).
- `--robot-schema` for JSON Schema output ([9213bc5](https://github.com/Dicklesworthstone/beads_viewer/commit/9213bc5)).
- `RobotEnvelope` for consistent robot output across all commands ([e6609dd](https://github.com/Dicklesworthstone/beads_viewer/commit/e6609dd)).

**Smart data sources:**
- Smart multi-source data detection with automatic fallback between SQLite, JSONL, and git sources ([2016b25](https://github.com/Dicklesworthstone/beads_viewer/commit/2016b25), [af5499b](https://github.com/Dicklesworthstone/beads_viewer/commit/af5499b)).

**Performance and triage:**
- Comprehensive Round 2 performance optimizations for robot-triage latency ([378c4c6](https://github.com/Dicklesworthstone/beads_viewer/commit/378c4c6)).
- Buffer pooling for Brandes' algorithm ([44c10c2](https://github.com/Dicklesworthstone/beads_viewer/commit/44c10c2)).
- Memoize `GetActionableIssues` via `TriageContext` ([127176b](https://github.com/Dicklesworthstone/beads_viewer/commit/127176b)).
- O(n^2) to O(n log n) sorts, package-level allocations, determinism fixes ([ac57875](https://github.com/Dicklesworthstone/beads_viewer/commit/ac57875)).

**Workspace and TUI:**
- Watch all repos in workspace mode ([ac11e35](https://github.com/Dicklesworthstone/beads_viewer/commit/ac11e35)).
- `y` shortcut to copy bead ID in list view ([a9ff252](https://github.com/Dicklesworthstone/beads_viewer/commit/a9ff252)).
- Fix phantom detail pane from hiding graph nodes in HTML export ([84ae00b](https://github.com/Dicklesworthstone/beads_viewer/commit/84ae00b)).
- Fix HTTP 408 timeout on large GitHub Pages pushes ([a9594bf](https://github.com/Dicklesworthstone/beads_viewer/commit/a9594bf)).

**Other:**
- Support `review` status in data model ([5fc1a70](https://github.com/Dicklesworthstone/beads_viewer/commit/5fc1a70)).
- Support git worktrees when finding beads directory ([6414b4b](https://github.com/Dicklesworthstone/beads_viewer/commit/6414b4b)).
- Default install to `~/.local/bin` to avoid requiring root ([32785d4](https://github.com/Dicklesworthstone/beads_viewer/commit/32785d4)).

---

## [v0.13.0](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.13.0) -- 2026-01-14

### Major release: Homebrew/Scoop, performance metrics, vector search, and all 8 statuses

**Distribution:**
- GoReleaser auto-publishing to Homebrew and Scoop ([09db3df](https://github.com/Dicklesworthstone/beads_viewer/commit/09db3df)).
- Claude Code SKILL.md for automatic capability discovery ([3d393e0](https://github.com/Dicklesworthstone/beads_viewer/commit/3d393e0)).

**Performance:**
- `--robot-metrics` command for performance profiling ([af868af](https://github.com/Dicklesworthstone/beads_viewer/commit/af868af)).
- Comprehensive Round 2 optimizations: topk package, TriageContext memoization ([1cbdb9c](https://github.com/Dicklesworthstone/beads_viewer/commit/1cbdb9c), [47adaeb](https://github.com/Dicklesworthstone/beads_viewer/commit/47adaeb)).

**Data model:**
- Support all 8 beads status types; filter blocked items from `--robot-next` ([c1c5c40](https://github.com/Dicklesworthstone/beads_viewer/commit/c1c5c40)).
- Unify tombstone and closed status handling across codebase ([a80a75c](https://github.com/Dicklesworthstone/beads_viewer/commit/a80a75c)).
- Orphan detection, O(1) cycle lookup, and case-insensitive labels ([cba2f3f](https://github.com/Dicklesworthstone/beads_viewer/commit/cba2f3f)).

**Export:**
- SQLite export with comments and watch mode ([2324e61](https://github.com/Dicklesworthstone/beads_viewer/commit/2324e61)).
- XSS prevention and JSON encoding fixes in HTML export ([a1e4ca3](https://github.com/Dicklesworthstone/beads_viewer/commit/a1e4ca3)).

**Agent safety:**
- TTY guard to prevent control sequence leakage in robot mode ([c7e2cfe](https://github.com/Dicklesworthstone/beads_viewer/commit/c7e2cfe)).
- Windows compatibility for atomic file operations ([fe79efa](https://github.com/Dicklesworthstone/beads_viewer/commit/fe79efa)).

---

## [v0.12.1](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.12.1) -- 2026-01-07

### Multi-instance coordination and background analysis

**Instance coordination:**
- Multi-instance awareness with stale lock takeover and TOCTOU race fix ([8bf8118](https://github.com/Dicklesworthstone/beads_viewer/commit/8bf8118), [b544a31](https://github.com/Dicklesworthstone/beads_viewer/commit/b544a31)).

**Background analysis pipeline:**
- Async Phase 2 analysis notification ([bb5c4bc](https://github.com/Dicklesworthstone/beads_viewer/commit/bb5c4bc)).
- Background worker infrastructure with error handling and panic recovery ([cd31623](https://github.com/Dicklesworthstone/beads_viewer/commit/cd31623), [5ff1d90](https://github.com/Dicklesworthstone/beads_viewer/commit/5ff1d90)).
- `buildSnapshot()` with dedup and error handling ([0b67acd](https://github.com/Dicklesworthstone/beads_viewer/commit/0b67acd)).

**Other:**
- Prevent shell injection in editor file path ([9cd383b](https://github.com/Dicklesworthstone/beads_viewer/commit/9cd383b)).
- Add missing mutex to `InDegreeRank`/`OutDegreeRank` ([8c9d759](https://github.com/Dicklesworthstone/beads_viewer/commit/8c9d759)).
- JSONL parser fuzz testing ([3b0ed00](https://github.com/Dicklesworthstone/beads_viewer/commit/3b0ed00)).
- Windows installation support via PowerShell script ([3d48d7a](https://github.com/Dicklesworthstone/beads_viewer/commit/3d48d7a)).
- Accept any non-empty `IssueType` for Gastown compatibility ([f3cba6e](https://github.com/Dicklesworthstone/beads_viewer/commit/f3cba6e)).
- Convert static colors to `AdaptiveColor` for light terminal support ([761f4fb](https://github.com/Dicklesworthstone/beads_viewer/commit/761f4fb)).

---

## [v0.12.0](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.12.0) -- 2026-01-06

### Tree view and file-system navigation

- Tree view with expand/collapse, cursor-follows-viewport scrolling, and state persistence ([7cae563](https://github.com/Dicklesworthstone/beads_viewer/commit/7cae563), [f54da8b](https://github.com/Dicklesworthstone/beads_viewer/commit/f54da8b), [e3ad293](https://github.com/Dicklesworthstone/beads_viewer/commit/e3ad293)).
- Windowed viewport rendering with scroll position indicator ([a1b5e7c](https://github.com/Dicklesworthstone/beads_viewer/commit/a1b5e7c)).
- Handle `EDITOR` with arguments (e.g., `"cursor -w"`) ([bdae62a](https://github.com/Dicklesworthstone/beads_viewer/commit/bdae62a)).
- Use visual width in `padRight` for proper emoji/CJK alignment ([786cbc3](https://github.com/Dicklesworthstone/beads_viewer/commit/786cbc3)).

---

## [v0.11.3](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.11.3) -- 2026-01-03

### Nix flake and TUI polish

- Nix flake for reproducible builds and development (`nix profile install github:Dicklesworthstone/beads_viewer`) ([ceb993a](https://github.com/Dicklesworthstone/beads_viewer/commit/ceb993a)).
- TUI update modal with version comparison and false-positive dev build fix ([8b8bc01](https://github.com/Dicklesworthstone/beads_viewer/commit/8b8bc01)).
- Restore focus to label picker and time-travel input after help overlay ([1769970](https://github.com/Dicklesworthstone/beads_viewer/commit/1769970)).
- Fix Board view context help and keyboard shortcut inconsistencies ([122a08a](https://github.com/Dicklesworthstone/beads_viewer/commit/122a08a), [c125906](https://github.com/Dicklesworthstone/beads_viewer/commit/c125906)).

---

## [v0.11.2](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.11.2) -- 2025-12-21

### Binary asset embedding

- Embed viewer assets in binary for `--pages` export so the export works without network access ([d537ba5](https://github.com/Dicklesworthstone/beads_viewer/commit/d537ba5)).
- Fix `normalizedFiles` length in `ImpactAnalysis` correlation ([1c4c0fb](https://github.com/Dicklesworthstone/beads_viewer/commit/1c4c0fb)).

---

## [v0.11.1](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.11.1) -- 2025-12-19

### Mobile and heatmap enhancements

- Arrow key navigation, enhanced heatmap controls, and mobile help modal in the HTML viewer ([5a8c94c](https://github.com/Dicklesworthstone/beads_viewer/commit/5a8c94c)).

---

## [v0.11.0](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.11.0) -- 2025-12-19

### Hybrid search engine

- Hybrid search with graph-aware ranking combining FTS5 text search, vector similarity, and graph centrality ([7956d47](https://github.com/Dicklesworthstone/beads_viewer/commit/7956d47), [87981a0](https://github.com/Dicklesworthstone/beads_viewer/commit/87981a0)).
- Search configuration and CLI hybrid search integration ([87981a0](https://github.com/Dicklesworthstone/beads_viewer/commit/87981a0)).
- Metrics cache and query-adaptive weight adjustment ([4cbd453](https://github.com/Dicklesworthstone/beads_viewer/commit/4cbd453)).
- Graph metrics added to SQLite schema with optional WASM scorer ([61a93f7](https://github.com/Dicklesworthstone/beads_viewer/commit/61a93f7)).

---

## [v0.10.6](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.10.6) -- 2025-12-18

### E2E test fix

- Support Linux `script` command syntax for TUI E2E tests ([ebfdcf0](https://github.com/Dicklesworthstone/beads_viewer/commit/ebfdcf0)).

---

## [v0.10.5](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.10.5) -- 2025-12-18

### Escape key fix

- Escape key now properly closes label picker before triggering quit confirmation ([bff5876](https://github.com/Dicklesworthstone/beads_viewer/commit/bff5876)).

---

## [v0.10.4](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.10.4) -- 2025-12-18

### Mobile export and tutorial redesign

- Mobile-responsive UI and advanced graph metrics in HTML export ([53da0ca](https://github.com/Dicklesworthstone/beads_viewer/commit/53da0ca)).
- Replace ASCII art with native lipgloss component system in tutorial ([6fe88ed](https://github.com/Dicklesworthstone/beads_viewer/commit/6fe88ed)).

---

## [v0.10.3](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.10.3) -- 2025-12-18

### Massive feature release: graph heatmap, causality, board enhancements, tutorial system, and more

This release represents one of the largest feature drops in the project's history, spanning dozens of capabilities across the TUI and HTML export.

**Graph and visualization:**
- Heatmap mode with gold glow hover highlighting ([63d43dd](https://github.com/Dicklesworthstone/beads_viewer/commit/63d43dd)).
- Pre-computed graph layout and detail pane for pages export ([a0d7169](https://github.com/Dicklesworthstone/beads_viewer/commit/a0d7169)).
- Temporal causality analysis for bead history ([a941fbd](https://github.com/Dicklesworthstone/beads_viewer/commit/a941fbd)).
- Impact network graph for bead correlation ([7729029](https://github.com/Dicklesworthstone/beads_viewer/commit/7729029)).
- Timeline visualization panel in history view ([b51608c](https://github.com/Dicklesworthstone/beads_viewer/commit/b51608c)).
- Blocker chain visualization and `--robot-blocker-chain` command ([b18a1cf](https://github.com/Dicklesworthstone/beads_viewer/commit/b18a1cf)).
- File co-change pattern detection ([9b58387](https://github.com/Dicklesworthstone/beads_viewer/commit/9b58387)).
- Orphan commit detection with smart heuristics ([8a01220](https://github.com/Dicklesworthstone/beads_viewer/commit/8a01220)).
- View mode toggle animation in History view ([db435cf](https://github.com/Dicklesworthstone/beads_viewer/commit/db435cf)).

**Board view:**
- Inline card expansion ([966f513](https://github.com/Dicklesworthstone/beads_viewer/commit/966f513)).
- Swimlane grouping modes ([0c23321](https://github.com/Dicklesworthstone/beads_viewer/commit/0c23321)).
- Column statistics in board headers ([053999d](https://github.com/Dicklesworthstone/beads_viewer/commit/053999d)).
- Smart empty column handling ([b827f64](https://github.com/Dicklesworthstone/beads_viewer/commit/b827f64)).
- Filter keys (o/c/r) in board view ([60dabe2](https://github.com/Dicklesworthstone/beads_viewer/commit/60dabe2)).
- Rich card content with improved info density ([6753569](https://github.com/Dicklesworthstone/beads_viewer/commit/6753569)).
- Visual dependency indicators with color-coded borders ([f9c754f](https://github.com/Dicklesworthstone/beads_viewer/commit/f9c754f)).
- Detail panel with Tab toggle and full dependency info ([22e55ae](https://github.com/Dicklesworthstone/beads_viewer/commit/22e55ae), [35a49ab](https://github.com/Dicklesworthstone/beads_viewer/commit/35a49ab)).

**Tutorial system:**
- Multi-page tutorial with Glamour markdown rendering ([187c022](https://github.com/Dicklesworthstone/beads_viewer/commit/187c022), [091a597](https://github.com/Dicklesworthstone/beads_viewer/commit/091a597)).
- Tutorial progress persistence ([9f5e51b](https://github.com/Dicklesworthstone/beads_viewer/commit/9f5e51b)).
- Space key tutorial entry point in help modal ([49fc7a9](https://github.com/Dicklesworthstone/beads_viewer/commit/49fc7a9)).
- Content sections: Introduction, Core Concepts, Views & Navigation, Advanced Features, Real-World Workflows.

**History view:**
- File-centric drill-down ([f975c29](https://github.com/Dicklesworthstone/beads_viewer/commit/f975c29)).
- Lifecycle events display in detail pane ([9ba1348](https://github.com/Dicklesworthstone/beads_viewer/commit/9ba1348)).
- Statistics header bar with badges ([7c56a35](https://github.com/Dicklesworthstone/beads_viewer/commit/7c56a35)).
- Commit detail pane with rich display ([358280e](https://github.com/Dicklesworthstone/beads_viewer/commit/358280e)).
- Adaptive three-pane layout ([f374d48](https://github.com/Dicklesworthstone/beads_viewer/commit/f374d48)).
- Keyboard navigation improvements ([37bf83e](https://github.com/Dicklesworthstone/beads_viewer/commit/37bf83e)).
- Search and filter infrastructure ([6d2f89d](https://github.com/Dicklesworthstone/beads_viewer/commit/6d2f89d)).

**Agent integration:**
- AGENTS.md detection, blurb content, atomic file operations, and prompt modal ([35e5298](https://github.com/Dicklesworthstone/beads_viewer/commit/35e5298), [738eebf](https://github.com/Dicklesworthstone/beads_viewer/commit/738eebf), [61e1068](https://github.com/Dicklesworthstone/beads_viewer/commit/61e1068)).
- Legacy blurb detection and migration support ([f68d307](https://github.com/Dicklesworthstone/beads_viewer/commit/f68d307)).
- Change impact analysis for agents via `--robot-impact` ([25fe4e5](https://github.com/Dicklesworthstone/beads_viewer/commit/25fe4e5)).
- Session Preview Modal (`V` key) ([04c640a](https://github.com/Dicklesworthstone/beads_viewer/commit/04c640a)).
- Status bar session indicator ([8c18b66](https://github.com/Dicklesworthstone/beads_viewer/commit/8c18b66)).

**cass integration:**
- Detection and health checking for `cass` (coding_agent_session_search) ([beca14b](https://github.com/Dicklesworthstone/beads_viewer/commit/beca14b)).
- Search interface with safety wrappers and LRU caching ([24b4594](https://github.com/Dicklesworthstone/beads_viewer/commit/24b4594), [5fddade](https://github.com/Dicklesworthstone/beads_viewer/commit/5fddade)).

**SQLite and search:**
- Pure-Go SQLite with built-in FTS5 support, replacing CGO dependency ([eda80f0](https://github.com/Dicklesworthstone/beads_viewer/commit/eda80f0)).
- Correlation confidence audit ([f279882](https://github.com/Dicklesworthstone/beads_viewer/commit/f279882)).
- File-bead reverse index for history view ([382a90f](https://github.com/Dicklesworthstone/beads_viewer/commit/382a90f)).

**Other:**
- Context-specific help system ([c0e83e1](https://github.com/Dicklesworthstone/beads_viewer/commit/c0e83e1)).
- Label picker with count-based sorting ([9983300](https://github.com/Dicklesworthstone/beads_viewer/commit/9983300)).
- Tiered escape behavior -- clear filters first, then quit ([51a7a38](https://github.com/Dicklesworthstone/beads_viewer/commit/51a7a38)).
- Redesigned flow matrix as interactive dashboard ([3381324](https://github.com/Dicklesworthstone/beads_viewer/commit/3381324)).
- Semantic search with non-blocking keyboard handling ([a2ecd1d](https://github.com/Dicklesworthstone/beads_viewer/commit/a2ecd1d)).
- Auto-add `.bv` to `.gitignore` ([6921e11](https://github.com/Dicklesworthstone/beads_viewer/commit/6921e11)).
- Fix deadlock in `FeedbackData` weight retrieval methods ([13c3c6a](https://github.com/Dicklesworthstone/beads_viewer/commit/13c3c6a)).

---

## [v0.10.2](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.10.2) -- 2025-11-30

### Test coverage and CI hardening

- Coverage gates and Codecov wiring ([e9f795e](https://github.com/Dicklesworthstone/beads_viewer/commit/e9f795e)).
- Broadened test coverage across UI, hooks, git loader, analysis, and robot CLI ([8e5efd4](https://github.com/Dicklesworthstone/beads_viewer/commit/8e5efd4), [70bd854](https://github.com/Dicklesworthstone/beads_viewer/commit/70bd854), [5783560](https://github.com/Dicklesworthstone/beads_viewer/commit/5783560)).
- Recipe filter/sort and cycle formatting tests ([90bbf61](https://github.com/Dicklesworthstone/beads_viewer/commit/90bbf61)).
- Refresh insights panel when toggled ([f531f4e](https://github.com/Dicklesworthstone/beads_viewer/commit/f531f4e)).
- Support unbounded track IDs in execution plan ([443162e](https://github.com/Dicklesworthstone/beads_viewer/commit/443162e)).

---

## [v0.10.1-build.2](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.10.1-build.2) -- 2025-11-30

Incremental build with additional test coverage for hooks, UI edge cases, and benchmark improvements. No user-facing feature changes beyond v0.10.2.

---

## [v0.10.1](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.10.1) -- 2025-11-30

### Module path fix

- Fix module path and correct syntax error in diff.go ([ad84b1b](https://github.com/Dicklesworthstone/beads_viewer/commit/ad84b1b), [eca1623](https://github.com/Dicklesworthstone/beads_viewer/commit/eca1623)).

---

## [v0.10.0](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.10.0) -- 2025-11-30

### Major release: drift detection, workspace mode, and graph engine overhaul

**Drift detection:**
- New drift detection package with configurable staleness thresholds and alerts ([b275251](https://github.com/Dicklesworthstone/beads_viewer/commit/b275251)).
- Comprehensive drift E2E tests ([8631705](https://github.com/Dicklesworthstone/beads_viewer/commit/8631705), [88b6029](https://github.com/Dicklesworthstone/beads_viewer/commit/88b6029)).

**Graph engine:**
- Async Phase 2 with caching and configuration for graph analysis ([4757dde](https://github.com/Dicklesworthstone/beads_viewer/commit/4757dde)).
- Improved data loading and model validation robustness ([3567434](https://github.com/Dicklesworthstone/beads_viewer/commit/3567434)).

**Workspace mode:**
- Workspace mode with file watching and visual enhancements ([6f6c069](https://github.com/Dicklesworthstone/beads_viewer/commit/6f6c069)).

**Export:**
- Improved markdown reporting with expanded integration tests ([fd86957](https://github.com/Dicklesworthstone/beads_viewer/commit/fd86957)).

**Other:**
- Handle UTF-8 BOM in JSONL files ([e985dff](https://github.com/Dicklesworthstone/beads_viewer/commit/e985dff)).
- Robust error handling and validation in loader and updater packages ([9191c59](https://github.com/Dicklesworthstone/beads_viewer/commit/9191c59)).
- Install script improvements: prefer release binaries, guard `BASH_SOURCE` for piped installs ([5426d73](https://github.com/Dicklesworthstone/beads_viewer/commit/5426d73), [c31f605](https://github.com/Dicklesworthstone/beads_viewer/commit/c31f605)).

---

## [v0.9.3](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.9.3) -- 2025-11-30

### Pre-v0.10 checkpoint

- Checkpoint of accumulated code and beads data before the v0.10 series.

---

## [v0.9.2](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.9.2) -- 2025-11-27

### TUI interaction features

- Time-travel input for exploring historical states of beads data ([c28d145](https://github.com/Dicklesworthstone/beads_viewer/commit/c28d145)).
- Clipboard copy support ([c28d145](https://github.com/Dicklesworthstone/beads_viewer/commit/c28d145)).
- `E` keybinding to export Markdown report from TUI ([51f1b72](https://github.com/Dicklesworthstone/beads_viewer/commit/51f1b72)).
- Editor launch from TUI ([c28d145](https://github.com/Dicklesworthstone/beads_viewer/commit/c28d145)).
- Allow Ctrl+C to quit during time-travel input ([21f4f38](https://github.com/Dicklesworthstone/beads_viewer/commit/21f4f38)).

---

## [v0.9.1](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.9.1) -- 2025-11-27

### Analysis hang fix and docs

- Prevent graph analysis hang with HITS/cycle timeouts ([1e83209](https://github.com/Dicklesworthstone/beads_viewer/commit/1e83209)).
- Redesigned TUI architecture diagram ([eddabb6](https://github.com/Dicklesworthstone/beads_viewer/commit/eddabb6)).

---

## [v0.9.0](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.9.0) -- 2025-11-27

### UI design system and smart install

- Smart install script with binary-first strategy ([8c3ad56](https://github.com/Dicklesworthstone/beads_viewer/commit/8c3ad56)).
- Comprehensive UI design system with badge components ([f821019](https://github.com/Dicklesworthstone/beads_viewer/commit/f821019)).
- Stripe-level visual polish for UI components ([4ebb573](https://github.com/Dicklesworthstone/beads_viewer/commit/4ebb573)).
- Redesigned footer with keyboard hints and status indicators ([aec03fd](https://github.com/Dicklesworthstone/beads_viewer/commit/aec03fd)).
- Enhanced git loader with date parsing and scanner error handling ([3037beb](https://github.com/Dicklesworthstone/beads_viewer/commit/3037beb)).
- Optimized graph analysis with fixed edge direction semantics ([04e96e7](https://github.com/Dicklesworthstone/beads_viewer/commit/04e96e7)).
- MIT license and screenshot assets ([1b0bc8d](https://github.com/Dicklesworthstone/beads_viewer/commit/1b0bc8d)).

---

## [v0.8.2](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.8.2) -- 2025-11-27

### Graph visualization rewrite

- Visual ASCII graph with comprehensive metrics ([0dc4c5d](https://github.com/Dicklesworthstone/beads_viewer/commit/0dc4c5d)).
- Fix critical bugs in graph visualization ([ce1cc83](https://github.com/Dicklesworthstone/beads_viewer/commit/ce1cc83)).
- Ego-centric neighborhood display for graph view ([100638c](https://github.com/Dicklesworthstone/beads_viewer/commit/100638c)).
- Fix header cutoff bug and add mouse wheel scrolling ([6f32017](https://github.com/Dicklesworthstone/beads_viewer/commit/6f32017)).

*Note: v0.8.0 and v0.8.1 exist as draft releases only and were never published.*

---

## [v0.7.0](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.7.0) -- 2025-11-27

### Time-travel, planning, recipes, and AI agent interface

- Time-travel analysis: loader and UI components for viewing beads at any point in git history ([9ee882e](https://github.com/Dicklesworthstone/beads_viewer/commit/9ee882e)).
- Recipe system for pre-built filter/sort combinations ([9ee882e](https://github.com/Dicklesworthstone/beads_viewer/commit/9ee882e)).
- AI agent CLI interface for programmatic graph analysis ([fe6646a](https://github.com/Dicklesworthstone/beads_viewer/commit/fe6646a)).
- Interactive dependency graph visualization ([c6ea4f7](https://github.com/Dicklesworthstone/beads_viewer/commit/c6ea4f7)).
- Interactive insights dashboard with calculation proofs ([b8106a5](https://github.com/Dicklesworthstone/beads_viewer/commit/b8106a5)).
- Paging, fixed header row, F1 help, and quit confirmation ([8910c0c](https://github.com/Dicklesworthstone/beads_viewer/commit/8910c0c)).
- Board view refactored with adaptive column navigation ([562beb7](https://github.com/Dicklesworthstone/beads_viewer/commit/562beb7)).
- Smart JSONL file discovery with fallback ([aee943b](https://github.com/Dicklesworthstone/beads_viewer/commit/aee943b)).
- Comprehensive unit tests for analysis, planning, and UI logic ([dbf2c06](https://github.com/Dicklesworthstone/beads_viewer/commit/dbf2c06)).

---

## [v0.6.2](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.6.2) -- 2025-11-26

CI-only change: remove master branch trigger from workflows.

---

## [v0.6.1](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.6.1) -- 2025-11-26

Branch sync: finalize master-to-main migration.

---

## [v0.6.0](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.6.0) -- 2025-11-26

Documentation update with main branch install URL and feature overview.

---

## [v0.5.3](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.5.3) -- 2025-11-26

### Performance

- Optimize graph analysis and fix UBS (Ultimate Bug Scanner) findings ([55152e5](https://github.com/Dicklesworthstone/beads_viewer/commit/55152e5)).

---

## [v0.5.2](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.5.2) -- 2025-11-26

Documentation polish and install URL fix.

---

## [v0.5.1](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.5.1) -- 2025-11-26

Documentation update with all features and install branch reference fix.

---

## [v0.5.0](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.5.0) -- 2025-11-26

### Insights dashboard and sparklines

- Insights dashboard with sparklines and advanced analytics ([294dd8e](https://github.com/Dicklesworthstone/beads_viewer/commit/294dd8e)).

---

## [v0.4.1](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.4.1) -- 2025-11-26

### Impact score fix

- Fix impact score logic and UI integration ([5b8a8fc](https://github.com/Dicklesworthstone/beads_viewer/commit/5b8a8fc)).

---

## [v0.4.0](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.4.0) -- 2025-11-26

### Graph theory analytics

- Graph theory analytics and impact scoring using PageRank, betweenness centrality, and HITS ([a0de64b](https://github.com/Dicklesworthstone/beads_viewer/commit/a0de64b)).

---

## [v0.3.0](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.3.0) -- 2025-11-26

### Board filtering fix

- Fix critical board filtering bug, enhanced layouts, and tests ([5294241](https://github.com/Dicklesworthstone/beads_viewer/commit/5294241)).

---

## [v0.2.0](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.2.0) -- 2025-11-26

### Kanban board

- Kanban board view, Mermaid diagram export, and visual polish ([d18b489](https://github.com/Dicklesworthstone/beads_viewer/commit/d18b489)).

---

## [v0.1.1](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.1.1) -- 2025-11-26

### Markdown export

- Markdown export, ultra-wide view, and real-data tests ([bed7a9b](https://github.com/Dicklesworthstone/beads_viewer/commit/bed7a9b)).

---

## [v0.1.0](https://github.com/Dicklesworthstone/beads_viewer/releases/tag/v0.1.0) -- 2025-11-26

### Initial release

- Split view TUI with list and detail panes.
- Issue statistics and summaries.
- Self-updater.
- CI/CD with GoReleaser.
- Initial commit ([61ff39d](https://github.com/Dicklesworthstone/beads_viewer/commit/61ff39d)).
