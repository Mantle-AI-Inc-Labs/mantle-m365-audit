# Upstream Pin — mantle-m365-audit

## Primary upstream

| Item | Value |
|---|---|
| Upstream repo | [Galvnyz/M365-Assess](https://github.com/Galvnyz/M365-Assess) |
| Pinned ref | tag `v2.14.0` |
| Pinned commit SHA | `e891bcb69b6b5d99e60a799651581c975840b2b4` |
| Upstream published | 2026-10-19 |
| Mantle review date | 2026-11-11 |
| Reviewer | ascheer-mntl |

## Dependency: control registry

| Item | Value |
|---|---|
| Upstream repo | [Galvnyz/CheckID](https://github.com/Galvnyz/CheckID) |
| Pinned ref | tag `v3.4.1` |
| Upstream published | 2026-06-13 |

The CheckID pin is tracked in the upstream M365-Assess codebase via the `TAG` variable in `.github/workflows/ci.yml`; Mantle does not override this pin independently unless a divergence is needed (if so, record in `DIVERGENCES.md`).

## How to bump

1. The weekly upstream-watcher action opens a GitHub issue when a new upstream release is published.
2. Walk `mantle/SYNC_CHECKLIST.md` for the release.
3. Create branch `upstream-sync/<vX.Y.Z>`, merge the upstream tag, re-apply all patches from `mantle/DIVERGENCES.md`, run the regression suite.
4. Update this file + open a PR to `main`.
