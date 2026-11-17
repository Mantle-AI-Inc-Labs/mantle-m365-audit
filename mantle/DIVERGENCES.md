# Mantle Divergences from Upstream

Every Mantle-specific change to upstream code requires an entry here, created in the same PR as the change.

Entries are reverse-chronological. Use H2 for each divergence.

## Entry schema

- **File(s):** path(s) touched
- **Upstream version last reviewed:** pinned tag / SHA
- **Rationale:** why Mantle needs this deviation (what client-facing or operator-facing problem does it solve)
- **Cost of carrying:** merge conflicts expected on upstream sync; test burden; who maintains it
- **Upstream issue / PR:** link if we've tried to push the change upstream so this entry can one day be deleted
- **Introduced:** date and fork PR #
- **Review date:** when to next re-evaluate whether this can be dropped (quarterly default)

---

## Invoke-M365Assessment -AdditionalGraphScopes and -KeepGraphConnection

- **Files:**
  - `src/M365-Assess/Invoke-M365Assessment.ps1` (two parameters, help text, scope merge, conditional Graph disconnect)
  - `tests/Mantle/Invoke-M365Assessment.MantleEnrichments.Tests.ps1` (new)
- **Upstream version last reviewed:** tag `v2.14.0` (`e891bcb69b6b5d99e60a799651581c975840b2b4`)
- **Rationale:** The Mantle engagement orchestrator (`mantle-engagement` in mantle-m365-render) runs this assessment and then mantle-m365-docs against the same tenant with one sign-in. `-AdditionalGraphScopes` adds the docs fork's read scopes to the consent this assessment already requests, and `-KeepGraphConnection` leaves the Graph session open at the end so the orchestrator can pass its token to `Connect-M365Doc -AccessToken`. Without these, the operator signs in twice and the docs run lacks consent for Cloud PC, print, entitlement-management, PIM-eligibility, and information-protection scopes.
- **Cost of carrying:** Low. Both parameters are additive and default to upstream behavior. Conflicts are possible if upstream reworks the scope-assembly block or the disconnect loop near the end of the script. Pester tests cover parameter declarations and, through `-DryRun`, the scope merge; `-KeepGraphConnection` is verified by the orchestrator's live run.
- **Upstream issue / PR:** None opened yet. Both parameters are generic enough to propose upstream.
- **Introduced:** 2026-11-17, fork PR TBD (`mantle/graph-session-reuse` branch)
- **Review date:** 2027-02-17
