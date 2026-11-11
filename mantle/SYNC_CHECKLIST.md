# Upstream Sync Impact Assessment — mantle-m365-audit

Walk this checklist for every upstream release before merging into `main`.

## 1. Engagement impact

- [ ] Which clients are currently engaged?
- [ ] Do any shipped deliverables depend on checks in the changed set?
- [ ] If yes: hold the sync until the active engagement ships, OR scope the sync to non-affected checks only.

## 2. Severity-overlay conflicts

- [ ] Does the upstream release change severity ratings on any check that Mantle has locally overridden in `controls/risk-severity.json`?
- [ ] If yes: decide per check whether to accept upstream's new rating (remove the local override) or keep Mantle's.
- [ ] Record each decision in `DIVERGENCES.md`.

## 3. Context-weighting changes

- [ ] Does the upstream release introduce new check categories that `mantle-m365-render` needs to handle in context-based weighting (HIPAA overlay, life-sciences overlay, industry overlays)?
- [ ] If yes: open a parallel PR in `mantle-m365-render` and reference it from this sync PR.

## 4. Adapter rendering

- [ ] Does the upstream release change the `assessment-bridge.schema.json` schema or introduce new finding section types that `mantle-m365-render` doesn't render?
- [ ] If yes: bump the adapter schema version and update the Findings-Appendix renderer.

## 5. Regression test

- [ ] Run the sync branch against the reference-tenant snapshot corpus in `mantle-m365-render/tests/fixtures/`.
- [ ] Zero regressions required before merge.

## Merge workflow

```bash
git checkout -b upstream-sync/<vX.Y.Z>
git fetch upstream
git merge upstream/<ref>
# re-apply patches from mantle/DIVERGENCES.md
# run tests + regression against fixtures
# PR to main of fork with this checklist completed in the PR body
```
