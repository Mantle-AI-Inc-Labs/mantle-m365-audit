# Upstream Sync Checklist — mantle-m365-safeguards-audit

Walk this for every upstream release before merging it into `main`. The weekly `upstream-watch` workflow opens an issue in this repo when Galvnyz/M365-Assess publishes a release newer than the tag pinned in `mantle/UPSTREAM_PIN.md`.

## 1. Read what changed

- [ ] Read the release notes and the compare link in the watcher issue.
- [ ] Check whether upstream touched any file listed in `mantle/DIVERGENCES.md`. Expect merge conflicts there, and decide per entry: keep it, adapt it, or drop it because upstream now covers it (then delete the entry).

## 2. Engagement timing

- [ ] If a client engagement is mid-collection, finish it on the current pin first. A sync can change checks, severities, and the bridge output that deliverables are built from.

## 3. Merge on a branch

```bash
git switch -c upstream-sync/<vX.Y.Z> main
git fetch upstream --tags
git merge <vX.Y.Z>
```

The Mantle patches are already on `main`, so the merge carries them forward; resolve any conflicts so that every `DIVERGENCES.md` entry still works.

## 4. Verify

- [ ] Mantle tests: `pwsh -NoProfile -Command "Invoke-Pester -Path ./tests/Mantle"`.
- [ ] Upstream suite: `pwsh -NoProfile -Command "Invoke-Pester -Path ./tests"` (CI also runs it on the PR).
- [ ] In mantle-m365-assessment, `uv run pytest`. `tests/test_themes.py` fails if the release adds check families without a report theme (extend `src/mantle_m365_assessment/data/report_themes.json`); bridge-loading tests fail if the bridge schema changed (update the vendored `data/assessment-bridge.schema.json` and `bridge.py`).
- [ ] In mantle-m365-assessment, `uv run mantle-engagement doctor` reports every Mantle patch present.
- [ ] Live run against the test tenant: `uv run mantle-engagement run test-tenant --tenant <test tenant>`, then review the delivery package. CI uses synthetic fixtures; the live run is the real check.

## 5. Record and merge

- [ ] Update `mantle/UPSTREAM_PIN.md` (tag, commit SHA, published date, review date, reviewer).
- [ ] Update "Upstream version last reviewed" on each `DIVERGENCES.md` entry.
- [ ] Open a PR to `main` with this checklist completed in the body, merge it, and close the watcher issue.
- [ ] Do not push `v*` tags in this fork: the release workflow would publish to the PowerShell Gallery.
