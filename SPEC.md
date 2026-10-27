# WordCraft: spec (our fork)

**Approval:** 2026-10-26, by the owner: the specs and roadmaps were developed upstream, so normalize them to our system (spec = upstream roadmap + our delta, R18).

## The spec is upstream's

WordCraft is a fork of [storytold/wordcraft](https://github.com/storytold/wordcraft). Upstream's plan is the spec:

- [`ROADMAP.md`](https://github.com/storytold/wordcraft/blob/main/ROADMAP.md): the summary, stage and headline numbers (parity with Microsoft Word).
- [`docs/gaps.md`](https://github.com/storytold/wordcraft/blob/main/docs/gaps.md): the ranked work list. The current stage is gaps #1–#7 (beta list).
- [`docs/roadmap.md`](https://github.com/storytold/wordcraft/blob/main/docs/roadmap.md): milestones and the current focus.

Our copies of those files come in with the daily upstream merge; never edit them here.

**1.0 for our fork:** upstream's beta, plus the delta below, used for real work on Mac, PC and Linux.

## Our delta

What our fork has or wants that upstream does not. "Send upstream" work is built here and offered upstream as a pull request (R36); "keep on our fork" stays here.

| Item | Where it goes |
|---|---|
| Agent-skills config: lines in upstream's `CLAUDE.md` and `docs/agents/` (commit ba84ba66) | keep on our fork |
| Pipeline v2 files: `PIPELINE.md`, `SPEC.md`, `CHANGELOG.md`, `AGENTS.fork.md`, `docs/code-map.md` | keep on our fork |
| The owner's 95% feature list for real work (#6) | keep on our fork |
| First real-use test on each platform and a golden task (#7) | keep on our fork |
| WordCraft as the default app for Word-type documents when the launcher installs it (#8; the work lives in craft-launcher) | keep on our fork |
| Sibling sweep bug class BC1, the ancestor-swap race in the automation read check (#4) | send upstream |
| Beta-list build issues filed 2026-10-26 (gaps #1–#7 not already covered by an open upstream PR) | send upstream |

Our issues and our code map cover the delta and the build issues we take from upstream's current stage. Anything else upstream builds itself: check upstream's open pull requests before starting an issue, and close ours if upstream has it.
