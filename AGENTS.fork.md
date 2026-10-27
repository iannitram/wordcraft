# Rules for our pipeline in this fork

Upstream's `AGENTS.md` and `CLAUDE.md` are upstream's: follow them for how the code is written and checked. This file adds our pipeline. The full pipeline and rules live in the pipeline repo (private): `PIPELINE.md`, `RULES.md`, `RUBRIC.md`, `roles/`; this is the short copy.

1. **Find the stage** in `PIPELINE.md`. The spec is `SPEC.md` (upstream's roadmap plus our delta), the code map `docs/code-map.md`.
2. **Work from an issue.** Take one that is not `in-progress` and not blocked; add `in-progress` before you start. Its body is your brief; an Easy issue has a "Qwen brief" section with the expected results and the code map snippet.
3. **Check upstream first.** Before building, look for an open upstream pull request or a fix on upstream `main` for the same thing (`gh pr list -R storytold/wordcraft --search <words>`). If upstream has it, comment and close ours.
4. **Your own platform's issues first.** Mac, Windows and Linux all matter: say which you built on.
5. **Check that it builds**, then open the PR into our `main`. CI and a review by another satellite with another model do the rest.
6. **Label the PR** `by:<machine>` and `model:<model>`, and write `Closes #N`.
7. **A failed job** gets a comment on the issue saying what happened, and the issue moves up one size (Easy -> Medium -> Hard).
8. **Never edit upstream's files for our pipeline** (`ROADMAP.md`, `AGENTS.md`, `CLAUDE.md`, `docs/gaps.md`): ours are `PIPELINE.md`, `SPEC.md`, `CHANGELOG.md`, `AGENTS.fork.md` and `docs/code-map.md`. Code changes are fine; "send upstream" work goes upstream as a pull request (R36).
9. **No private details** in code, commits or PR text: no names, e-mail addresses, home paths or machine names. Commit as the no-reply identity.
