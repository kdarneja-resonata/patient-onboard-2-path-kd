# Patient Conversion War Room (Daily) — Meeting Notes

**Date:** 2026-05-19 (Tuesday)
**Attendees:** Lauran, Avinash, Aakash, Maya, KD, Anupam
**Source transcript:** Gemini auto-notes (pasted by Lauran)
**Scope:** Engineering sync on the patient onboarding pivot. Data backend, HIPAA posture, questionnaire updates, sort logic, dev-env deploy target for end of week.

---

## TL;DR

Engineering is moving the matching pipeline off Google Sheets and into the backend. Self-reported vs medical-record data become two distinct, timestamped value sets. The questionnaire gains refractory-status, LRP4 antibody, and an MGADL proxy; sample patients come out; unsupported conditions route to the marketing site (`Explore Resonata`), not the in-app explorer. FDA approvals sort before trials, trials sort by phase descending. Aakash + Avinash committed to a dev-env deploy by Friday so Lauran can test over the weekend. Open: whether the existing matching algorithm absorbs the new questionnaire rules or a new business-rule layer sits in front of it. Kate consult on HIPAA still pending (today's call, separate session).

---

## Decisions

### Aligned

1. **Unsupported conditions redirect to "Explore Resonata" on the marketing website.** Replaces the in-prototype `uncovered` + `explorer` screens for V1. Patients selecting a condition we don't yet match land on a marketing page that holds them with content while we build coverage.
2. **Remove sample patients from onboarding.** The onboarding flow uses the user's own profile to display care options; demo patient data comes out.
3. **Backend separates patient-supplied data from medical-record data.** Each set has its own values and its own timestamps. Same field can hold a self-reported value and a records-verified value side by side, with provenance and recency visible.
4. **Sort order on matches: FDA-approved before trials; trials by phase descending (3 → 2 → 1).** Avinash noted an existing Linear ticket for this sort logic.
5. **HubSpot must not store PHI.** Internal backend is the sole system of record. Lauran cited a recent $5M fine on a healthtech startup as the live precedent.
6. **MGADL proxy scoring logic.** "Manageable" and "I don't know" map to `unknown`; "barely noticeable" maps to MGADL ≥ 6; "severe" maps to MGADL < 6.
7. **"Get full access" stays as the primary CTA framing.** Carries over from the 2026-05-15 sign-off; reaffirmed today.

### Open / needs further discussion

8. **Matching logic implementation strategy.** Does the current matching algorithm absorb the new questionnaire (refractory, LRP4, MGADL proxy) directly, or does a new business-rule layer sit in front of it? Decision needed before the matching pipeline is wired to the new questionnaire.

---

## Action items

| Owner | Task | Due |
|---|---|---|
| Lauran + Anupam | Review match discrepancy: why full matches are missing for new patient profiles in v2.43 of the Google sheet Martin shared. Avinash already tagged Martin in the engineering channel with screenshots. | This week |
| Lauran | Update prototype: refractory generalized MG phrasing, remove redundant CTAs, reorder onboarding flow steps. | Before Friday deploy |
| Lauran | Provide official conditions list from Martin (derived from roadmap) for the questionnaire. Current AI-generated list does not match production support. | This week |
| Lauran | Consult Kate Black on HIPAA authorization requirements and validate the prototype interface. | 2026-05-19 (today, separate session) |
| Aakash | Verify whether administration method (oral vs infusion vs IV) is currently supported in care-option records. | This week |
| Avinash | Update backend schema: separate self-reported values from medical-record values, each with entry timestamps. | Before Friday deploy |
| Aakash + Avinash | Build updated onboarding flow + questionnaire functionality. Ready for dev-env deploy by Friday so Lauran can test over the weekend. | 2026-05-22 (Friday) |

---

## Prototype change list

References below use [`patient-onboarding-v2-prototype.html`](../../patient-onboarding-v2-prototype.html). All P0 unless flagged.

### Quiz (`quiz-1` through `quiz-4`)

| # | Priority | Change |
|---|---|---|
| 34 | P0 | **Add refractory-status identification to the questionnaire.** Generalized MG patients are split into refractory vs non-refractory; refractory is a major eligibility branch in the current trial set. Currently no quiz question captures this. Likely belongs in Q3 (treatment stage already lists "tried multiple, none worked"; needs explicit refractory wording) or as a follow-up. Lauran owns the phrasing pass. |
| 35 | P0 | **Add LRP4 antibody option to Q2.** Q2 currently offers AChR+ / MuSK+ / seronegative / I don't know. LRP4 needs to be a first-class option, not folded into "seronegative" or "I don't know." |
| 36 | P0 | **Replace Q3 ("Treatment stage") with an MGADL proxy question.** Builds on the Q3 regeneration work from the 2026-05-15 sign-off. New question text and answer set should produce an MGADL band: "barely noticeable" → ≥ 6; "severe" → < 6; "manageable" or "I don't know" → unknown. Lauran owns wording; Megan sanity-checks for patient comprehension. |
| 37 | P0 | **Refractory generalized MG phrasing.** Wherever the term appears in copy (preview cards, match descriptions, app-home, app-detail), align to a single patient-readable phrasing. "Refractory generalized myasthenia gravis" reads clinical; consider "generalized MG that hasn't responded to first-line treatment" or similar. Lauran's call. |

### Landing / uncovered branch

| # | Priority | Change |
|---|---|---|
| 38 | P0 | **Replace the in-prototype uncovered flow with a redirect to the marketing site's "Explore Resonata" page.** Today the `uncovered` and `explorer` screens live inside the prototype ([`patient-onboarding-v2-prototype.html` uncovered + explorer screens](../../patient-onboarding-v2-prototype.html)). The new model: if the patient selects a condition not in the supported list, the app does not enter the prototype experience; it redirects to the marketing site. Decision implications: drop the explorer screen from V1 scope, drop the issue 7b dependency on a content production pipeline inside the app. |
| 39 | P0 | **Conditions list must match production.** The dropdown currently uses an AI-generated list that does not match what the matching system actually supports. Pull the official list from Martin (roadmap-derived). Each entry tagged supported / coming-soon. Coming-soon entries route to the same marketing redirect with a "we'll let you know" capture. |

### Onboarding flow ordering

| # | Priority | Change |
|---|---|---|
| 40 | P0 | **Reorder onboarding flow steps.** Lauran's action item; specific reorder TBD. Likely overlaps with the consent-split reordering applied 2026-05-18 (Path A leads). Confirm whether additional reordering is required after the questionnaire updates land. |
| 41 | P0 | **Remove redundant CTA buttons.** Lauran's action item; specific buttons TBD. Likely candidates: duplicate "Continue" / "Save matches" / "Next" pairs across the quiz and preview screens. Pass for redundancy across the flow. |
| 42 | P0 | **Remove sample patients.** Wherever the prototype shows example patient data (names, profiles, demo matches), replace with the user's own profile built from quiz answers. Check the app-home, app-detail, and preview screens for embedded sample patient names. |

### Match presentation

| # | Priority | Change |
|---|---|---|
| 43 | P0 | **Apply the sort order: FDA-approved before trials; trials by phase descending.** Updates the match card sequence on `preview`, `app-home`, and `app-detail`. Card meta lines should already display phase for trials; verify the order matches the new sort. Avinash has the Linear ticket for the backend; prototype display needs to mirror it. |
| 44 | P1 | **Surface phase explicitly on trial cards.** If sorting is phase-descending, the patient needs to be able to see what phase they're looking at without tapping into detail. Likely a small badge in the trial card meta line. |

### Backend-driven UI (post-records state)

| # | Priority | Change |
|---|---|---|
| 45 | P0 | **Visual treatment for the two-value model (self-reported + records-verified).** Once backend separates the two sets, the UI needs to show which is which. Likely a small indicator on the card detail page: "Self-reported: AChR negative" / "From medical records: AChR+ confirmed 2025-08-14." Coordinate with the incomplete-record indicator pattern flagged as Issue 7e. |

### Compliance

| # | Priority | Change |
|---|---|---|
| 46 | P0 | **HubSpot data flow audit.** PHI cannot land in HubSpot. Confirm with Aakash + Avinash exactly what crosses the HubSpot boundary today (likely first name + email + condition string for marketing automation). Document the boundary in the PRD as an explicit architectural rule. |

---

## PRD change list

References below use [`prd-patient-onboarding-v3-2026-05-13.md`](../../prd-patient-onboarding-v3-2026-05-13.md).

### Reversals / updates

| # | Section | Change |
|---|---|---|
| P26 | Flow `:53-58` (Quiz) | Update Q2 to include LRP4. Replace Q3 description with MGADL proxy. Document the MGADL scoring bands ("barely noticeable" → ≥ 6; "severe" → < 6; rest → unknown) as a hard requirement so the backend doesn't reinterpret. Add refractory-status capture. |
| P27 | Flow `:65-67` (Uncovered branch) | Reverse: uncovered branch redirects to the marketing site's "Explore Resonata" page; no in-app explorer in V1. Update phasing to reflect that the in-app explorer pathway moves to V2+. Update Issue 7b: explorer pathway is no longer V1 scope; demand-pool capture happens at the marketing redirect, not inside the app. |
| P28 | Hard requirements | Add: **No PHI in HubSpot.** Backend is the sole system of record for any data linked to a patient identifier. HubSpot (and any other marketing automation surface) receives non-PHI fields only. |
| P29 | Hard requirements | Add: **Two-value data model.** Patient-supplied values and records-verified values are stored separately, each with its own timestamp. The UI surfaces provenance wherever both can exist for the same field. |
| P30 | Hard requirements | Add: **Match list sort order: FDA-approved before trials; trials descending by phase (3, 2, 1).** Default order applies absent any user filter. |
| P31 | Hard requirements | Add: **Conditions list reflects production matching support.** The dropdown is sourced from the roadmap, not generated. Each entry tagged `supported` or `coming-soon`. Coming-soon entries route to the marketing redirect, not into the matching flow. |
| P32 | Open issues | Add: **Matching logic strategy.** Pending decision on whether the existing matching algorithm absorbs the new questionnaire rules or a new business-rule layer sits in front of it. Owner: Avinash + Aakash + Lauran. |
| P33 | Stakeholders | Add Anupam as a contributor (data/integration). |

### New / appended

| # | Section | Change |
|---|---|---|
| P34 | Phasing V1 | Add: dev-env deploy target 2026-05-22 (Friday). |
| P35 | Phasing V2 | Add: in-app explorer pathway for uncovered conditions (moved from V1 per P27). |
| P36 | Phasing V3 | Add: administration method (oral / infusion / IV) as a card attribute and a filter, pending Aakash's verification that the data is in the care-option records today. |

---

## Open questions

- **Matching logic strategy** (see decision #8). Existing algorithm vs new rule layer.
- **Refractory phrasing** for patient consumption. Clinical term vs patient-readable.
- **Exact ordering** of onboarding flow steps after the questionnaire updates land (Lauran's action item #40).
- **Which CTAs are redundant** (action item #41) — needs Lauran's pass.
- **Admin method support in care-option records** (action item, Aakash). If not supported, P36 slips.
- **HubSpot boundary documentation** (action item #46). Exact fields crossing the line.

---

## Cross-references

- Today's separate session: Kate Black HIPAA consult ([`kate-legal-meeting-2026-05-19.md`](../kate-legal-meeting-2026-05-19.md)).
- Prior pivot sign-off: [`2026-05-15-patient-app-pivot-signoff.md`](2026-05-15-patient-app-pivot-signoff.md).
- Running open issues: [`prd-issues-running.md`](../../prd-issues-running.md).
- Prototype: [`patient-onboarding-v2-prototype.html`](../../patient-onboarding-v2-prototype.html).
- PRD: [`prd-patient-onboarding-v3-2026-05-13.md`](../../prd-patient-onboarding-v3-2026-05-13.md).

---

*Notes processed from Gemini auto-notes. Verify owner names and attribution against the source before circulating.*
