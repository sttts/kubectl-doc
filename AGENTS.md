# Agent Instructions

This project uses **bd** (beads) for issue tracking. Run `bd onboard` to get started.

## Quick Reference

```bash
bd ready              # Find available work
bd show <id>          # View issue details
bd update <id> --status in_progress  # Claim work
bd close <id>         # Complete work
bd sync               # Sync with git
```

## Releases

- A pushed tag is not a completed release by itself. Every tag meant as a release must result in a GitHub Release for the same tag.
- Before tagging, write or verify release notes for the exact version being released. Do not rely on GitHub `--generate-notes` alone for direct hotfix tags, because it may produce only a full-changelog link.
- Release notes must summarize the user-visible change, important bug fixes, and any relevant compatibility or integration impact.
- After the release workflow finishes, always run `gh release view <tag> --repo sttts/kubectl-doc` and verify the release body is meaningful, not just `Full Changelog`.
- Also verify the release workflow is green and the expected archives plus `.sha256` assets are attached before saying the release is done.
- If release notes are missing or too thin, edit the GitHub release before handing off.

### Release Notes Procedure

1. Prepare a notes file for the tag, usually under `/private/tmp/kubectl-doc-<tag>-release-notes.md`, with this shape:
   ```markdown
   ## Changes

   - Summarize the user-visible fix or feature.
   - Mention important regression coverage or generated artifact updates.

   ## Verification

   - `make lint`
   - `go test ./...`
   - GitHub release workflow completed successfully for `<tag>`.

   **Full Changelog**: https://github.com/sttts/kubectl-doc/compare/<previous-tag>...<tag>
   ```
2. After the release workflow publishes the release, inspect the body:
   ```bash
   gh release view <tag> --repo sttts/kubectl-doc
   ```
3. If the body is missing, thin, or only a full-changelog link, replace it:
   ```bash
   gh release edit <tag> --repo sttts/kubectl-doc --notes-file /private/tmp/kubectl-doc-<tag>-release-notes.md
   ```
4. Re-run `gh release view <tag> --repo sttts/kubectl-doc` and do not report the release as done until the notes are meaningful.

## Landing the Plane (Session Completion)

**When ending a work session**, you MUST complete ALL steps below. Work is NOT complete until `git push` succeeds.

**MANDATORY WORKFLOW:**

1. **File issues for remaining work** - Create issues for anything that needs follow-up
2. **Run quality gates** (if code changed) - Tests, linters, builds
3. **Update issue status** - Close finished work, update in-progress items
4. **PUSH TO REMOTE** - This is MANDATORY:
   ```bash
   git pull --rebase
   bd sync
   git push
   git status  # MUST show "up to date with origin"
   ```
5. **Clean up** - Clear stashes, prune remote branches
6. **Verify** - All changes committed AND pushed
7. **Hand off** - Provide context for next session

**CRITICAL RULES:**
- Work is NOT complete until `git push` succeeds
- NEVER stop before pushing - that leaves work stranded locally
- NEVER say "ready to push when you are" - YOU must push
- If push fails, resolve and retry until it succeeds


<!-- BEGIN BEADS INTEGRATION v:1 profile:minimal hash:7510c1e2 -->
## Beads Issue Tracker

This project uses **bd (beads)** for issue tracking. Run `bd prime` to see full workflow context and commands.

### Quick Reference

```bash
bd ready              # Find available work
bd show <id>          # View issue details
bd update <id> --claim  # Claim work
bd close <id>         # Complete work
```

### Rules

- Use `bd` for ALL task tracking — do NOT use TodoWrite, TaskCreate, or markdown TODO lists
- Run `bd prime` for detailed command reference and session close protocol
- Use `bd remember` for persistent knowledge — do NOT use MEMORY.md files

**Architecture in one line:** issues live in a local Dolt DB; sync uses `refs/dolt/data` on your git remote; `.beads/issues.jsonl` is a passive export. See https://github.com/gastownhall/beads/blob/main/docs/SYNC_CONCEPTS.md for details and anti-patterns.

## Session Completion

**When ending a work session**, you MUST complete ALL steps below. Work is NOT complete until `git push` succeeds.

**MANDATORY WORKFLOW:**

1. **File issues for remaining work** - Create issues for anything that needs follow-up
2. **Run quality gates** (if code changed) - Tests, linters, builds
3. **Update issue status** - Close finished work, update in-progress items
4. **PUSH TO REMOTE** - This is MANDATORY:
   ```bash
   git pull --rebase
   git push
   git status  # MUST show "up to date with origin"
   ```
5. **Clean up** - Clear stashes, prune remote branches
6. **Verify** - All changes committed AND pushed
7. **Hand off** - Provide context for next session

**CRITICAL RULES:**
- Work is NOT complete until `git push` succeeds
- NEVER stop before pushing - that leaves work stranded locally
- NEVER say "ready to push when you are" - YOU must push
- If push fails, resolve and retry until it succeeds
<!-- END BEADS INTEGRATION -->

## UI/CSS Conventions

- For text/icon alignment fixes, prefer semantic CSS alignment such as `align-items: baseline`, flex/grid alignment, or `vertical-align`.
- Do not use pixel-exact line boxes, manual offsets, or hard-coded positioning solely to make text appear aligned. Use relative units and inherited line-height where possible so embedding pages can scale the UI predictably.
