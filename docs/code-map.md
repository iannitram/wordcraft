# WordCraft code map (our fork)

Ours, not upstream's. Audited 2026-10-28 against `main`: each row below was checked against the Cargo workspace (`crates/*`, `apps/*`, `xtask`) and that crate's `Cargo.toml` (description and internal dependencies, dev-dependencies included). Sizes are Rust lines in tracked `.rs` files at that commit. The 16 crates, 3 apps and `xtask` below are the whole workspace. Upstream's own architecture notes: [`docs/architecture.md`](architecture.md) where present, and `AGENTS.md`. When a crate is added or split, update this table.

Describes commit: 1d8f900a (2026-10-28)

## Crates

| Path | What it owns | Uses (internal) | Platforms | Size |
|---|---|---|---|---|
| `crates/control-key` | WordCraft control-port key file: where it lives and how to read it (shared by the app and the MCP bridge) | — | 1 file(s) with per-OS or wasm code | ~87 |
| `crates/doc` | WordCraft document model: stories, paragraphs, runs, tables, sections, styles, numbering, fields, notes, comments, revisions; edit primitives | `geom` | portable | ~23,531 |
| `crates/docbin` | WordCraft reader for the Word 97-2003 binary file format (.doc, [MS-DOC]) | `doc`, `geom` | portable | ~8,815 |
| `crates/docx` | WordCraft DOCX (OOXML WordprocessingML) reader and writer | `doc`, `geom` | portable | ~36,907 |
| `crates/engine` | WordCraft engine: session, selection, undo, the command registry (every action is a command), Word's command catalog, file I/O dispatch | `doc`, `docbin`, `docx`, `fonts`, `formats`, `geom`, `layout`, `pdf`, `proof`, `render` | 24 file(s) with per-OS or wasm code | ~72,295 |
| `crates/fonts` | WordCraft fonts: font database (bundled OFL + system), metrics, glyph outlines, shaping | — | 6 file(s) with per-OS or wasm code | ~6,884 |
| `crates/formats` | WordCraft import/export: plain text, Markdown, HTML, RTF, OpenDocument Text and LaTeX | `doc`, `geom` | 1 file(s) with per-OS or wasm code | ~22,730 |
| `crates/geom` | WordCraft geometry and units: points, twips, EMU, inches, centimetres; rectangles; measurement parsing | — | portable | ~1,271 |
| `crates/layout` | WordCraft layout: shaping, line breaking, tabs, lists, tables, pagination, headers/footers, hit testing | `doc`, `fonts`, `geom`, `metafile`, `proof` | 1 file(s) with per-OS or wasm code | ~34,172 |
| `crates/mcp` | WordCraft MCP server: every command as a tool, headless or bridged to the running app | `control-key`, `doc`, `engine`, `render` | 1 file(s) with per-OS or wasm code | ~1,824 |
| `crates/metafile` | WordCraft metafiles: parses Windows WMF and EMF records into a neutral vector model (paths and bitmaps) | — | portable | ~3,614 |
| `crates/pdf` | WordCraft PDF export (craft-pdf / Krilla): embedded font subsets with selectable text, links, outline, tagging | `doc`, `fonts`, `geom`, `layout`, `metafile` | 1 file(s) with per-OS or wasm code | ~4,411 |
| `crates/proof` | WordCraft proofing per language: spelling, suggestions, grammar checks and hyphenation for English (public-domain word list) and Polish (installed Hunspell dictionary, TeX patterns) | — | 1 file(s) with per-OS or wasm code | ~16,296 |
| `crates/render` | WordCraft rasteriser: pages and thumbnails from the layout display list (vello_cpu), PNG/JPEG export | `doc`, `fonts`, `geom`, `layout`, `metafile` | 2 file(s) with per-OS or wasm code | ~4,433 |
| `crates/ui-egui` | WordCraft egui front end (Word-style ribbon UI); swappable: nothing below depends on it | `doc`, `engine`, `fonts`, `geom`, `layout`, `render`, `zotero` | 28 file(s) with per-OS or wasm code | ~80,056 |
| `crates/zotero` | WordCraft Zotero integration: Zotero's word-processor wire protocol, answered against an engine Session | `doc`, `docx`, `engine`, `formats` | 1 file(s) with per-OS or wasm code | ~2,372 |
| `apps/wordcraft` | WordCraft desktop app | `control-key`, `doc`, `engine`, `fonts`, `proof`, `ui-egui` | 11 file(s) with per-OS or wasm code | ~3,743 |
| `apps/wordcraft-cli` | WordCraft command line: convert, inspect, render, run commands, MCP server | `doc`, `engine`, `mcp`, `render`, `zotero` | 2 file(s) with per-OS or wasm code | ~1,904 |
| `apps/wordcraft-web` | WordCraft in the browser (eframe web runner, WebGPU with WebGL2 fallback) | `doc`, `engine`, `fonts`, `ui-egui` | 2 file(s) with per-OS or wasm code | ~1,159 |
| `xtask` | Build and check tasks (`cargo xtask ci`, parity reports) | — | portable | — |

## How they interlink

The engine owns the session, undo and the command registry; every user action is a command. The `ui-egui` front end sits on top and is swappable: nothing below it depends on it. The desktop, CLI and web apps are thin shells over the engine and the front end. The MCP crate drives the engine headless or through the running app.

- `control-key` is used by `mcp`, `wordcraft`.
- `doc` is used by `docbin`, `docx`, `engine`, `formats`, `layout`, `mcp`, `pdf`, `render`, `ui-egui`, `wordcraft`, `wordcraft-cli`, `wordcraft-web`, `zotero`.
- `docbin` is used by `engine`.
- `docx` is used by `engine`, `zotero`.
- `engine` is used by `mcp`, `ui-egui`, `wordcraft`, `wordcraft-cli`, `wordcraft-web`, `zotero`.
- `fonts` is used by `engine`, `layout`, `pdf`, `render`, `ui-egui`, `wordcraft`, `wordcraft-web`.
- `formats` is used by `engine`, `zotero`.
- `geom` is used by `doc`, `docbin`, `docx`, `engine`, `formats`, `layout`, `pdf`, `render`, `ui-egui`.
- `layout` is used by `engine`, `pdf`, `render`, `ui-egui`.
- `mcp` is used by `wordcraft-cli`.
- `metafile` is used by `layout`, `pdf`, `render`.
- `pdf` is used by `engine`.
- `proof` is used by `engine`, `layout`, `wordcraft`.
- `render` is used by `engine`, `mcp`, `ui-egui`, `wordcraft-cli`.
- `ui-egui` is used by `wordcraft`, `wordcraft-web`.
- `zotero` is used by `ui-egui`, `wordcraft-cli`.

## Upstream and our delta

Every crate, app and the `xtask` here is upstream's (`storytold/wordcraft`). Our delta on top is documentation only: `PIPELINE.md`, `SPEC.md`, `CHANGELOG.md`, `AGENTS.fork.md`, this map, `docs/agents/`, and ten lines in `CLAUDE.md` (agent-skills config); `git diff --stat upstream/main...main` shows no source file. Upstream commits daily, so a day or two of its work is not on `main` until the daily sync merges it (on 2026-10-28 the crate set was identical on both).

## Outside the workspace

In this repo but outside the Cargo workspace: `assets/` (app icons, fonts, hyphenation and spelling data), `packaging/` (per-OS release packaging: `linux`, `macos`, `windows`, `web`; no `freebsd` directory), `craft-i18n.rev` (the commit of the `craft-i18n` interface translations that releases embed), `ATTRIBUTION.md`, `contributors/`.

Not here at all: shared code that comes in as git dependencies pinned by revision, so a change to it is a change in another repository: the `craft-*` crates (`craft-pdf`, `craft-update` and `krilla`) from `storytold/craft-libs`, and forks of egui, eframe and winit (`storytold/egui`, `storytold/winit`).

## Mac, PC and Linux (R04)

- Packaging for macOS, Windows, Linux and the web is in `packaging/`; FreeBSD has a CI workflow but no packaging directory here. `release.yml` (push to `release`) calls the shared release workflow in `storytold/craft-common-actions`.
- Per-OS code sits mostly in the engine, the `ui-egui` front end and the desktop app (the "Platforms" column counts files with `cfg(target_os…)`, `cfg(windows|unix)`, `cfg(target_family…)` or wasm code). Everything else is portable Rust.
- CI: `ci.yml` (every pull request and push to main) calls `storytold/craft-common-actions` `gate.yml` with its default `os` input, `["ubuntu-24.04"]`: the full gate runs on Linux only. The one workflow that runs tests on Windows and macOS is `printing.yml`: the print host and the UI tests on `ubuntu-24.04`, `windows-latest` and `macos-latest`, for pull requests and pushes to main that touch `Cargo*`, `apps/wordcraft*`, `crates/ui-egui` or the workflow. The others: `freebsd.yml`, `nix.yml`, `packaging-lint.yml`, `windows-arm64.yml` (builds the ARM64 package and smoke-tests it; no ARM `cargo test`), `sonarcloud.yml` and `release.yml`.
- A build issue that touches per-OS code carries a `platform:*` label and says which OS it was built and checked on.
