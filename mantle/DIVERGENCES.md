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

## README notice pointing to mantle-engagement

- **Files:** `README.md` (notice block above the upstream content)
- **Upstream version last reviewed:** tag `v2.14.0` (`e891bcb69b6b5d99e60a799651581c975840b2b4`)
- **Rationale:** The fork's README is otherwise the upstream project's, with upstream run instructions. Mantle staff opening this repo need to know that client engagements run through `mantle-engagement` in mantle-m365-render, and that this repo is run directly only for patch development and upstream syncs.
- **Cost of carrying:** Minimal. The notice sits above the upstream content; on sync, keep it at the top if upstream rewrites the README's opening lines.
- **Upstream issue / PR:** Not applicable (Mantle-specific).
- **Introduced:** 2026-11-17, fork PR #3 (`mantle/readme-entry-point` branch)
- **Review date:** 2027-02-17

## Bridge JSON setting and recommendedValue fields

- **Files:**
  - `src/M365-Assess/Common/Export-AssessmentBridgeJson.ps1` (two fields per finding)
  - `src/M365-Assess/schemas/assessment-bridge.schema.json` (two optional nullable properties)
  - `tests/Mantle/Export-AssessmentBridgeJson.MantleEnrichments.Tests.ps1` (new)
- **Upstream version last reviewed:** tag `v2.14.0` (`e891bcb69b6b5d99e60a799651581c975840b2b4`)
- **Rationale:** mantle-m365-render's client appendix needs to say what each failed check found and what it expected. The bridge carried `currentValue` but not the collector's `Setting` or `RecommendedValue`, which the enriched rows already hold, so a row could only restate the control name ("Ensure ...") without showing that it failed or why.
- **Cost of carrying:** Low. Additive fields; `schemaVersion` stays `1.1` because consumers that ignore unknown properties are unaffected. Conflicts are possible if upstream reshapes the per-finding object in the exporter.
- **Upstream issue / PR:** None opened yet; generally useful to other bridge consumers.
- **Introduced:** 2026-11-17, fork PR #2 (`mantle/graph-session-reuse` branch)
- **Review date:** 2027-02-17

## Invoke-M365Assessment -AdditionalGraphScopes and -KeepGraphConnection

- **Files:**
  - `src/M365-Assess/Invoke-M365Assessment.ps1` (two parameters, help text, scope merge, conditional Graph disconnect)
  - `tests/Mantle/Invoke-M365Assessment.MantleEnrichments.Tests.ps1` (new)
- **Upstream version last reviewed:** tag `v2.14.0` (`e891bcb69b6b5d99e60a799651581c975840b2b4`)
- **Rationale:** The Mantle engagement orchestrator (`mantle-engagement` in mantle-m365-render) runs this assessment and then mantle-m365-docs against the same tenant with one sign-in. `-AdditionalGraphScopes` adds the docs fork's read scopes to the consent this assessment already requests, and `-KeepGraphConnection` leaves the Graph session open at the end so the orchestrator can pass its token to `Connect-M365Doc -AccessToken`. Without these, the operator signs in twice and the docs run lacks consent for Cloud PC, print, entitlement-management, PIM-eligibility, and information-protection scopes.
- **Cost of carrying:** Low. Both parameters are additive and default to upstream behavior. Conflicts are possible if upstream reworks the scope-assembly block or the disconnect loop near the end of the script. Pester tests cover parameter declarations and, through `-DryRun`, the scope merge; `-KeepGraphConnection` is verified by the orchestrator's live run.
- **Upstream issue / PR:** None opened yet. Both parameters are generic enough to propose upstream.
- **Introduced:** 2026-11-17, fork PR #2 (`mantle/graph-session-reuse` branch)
- **Review date:** 2027-02-17
