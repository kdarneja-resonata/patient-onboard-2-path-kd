# Handoff: Patient Onboarding v2 UX Changes

**To:** KD
**From:** Lauran
**Date:** 2026-05-26
**Purpose:** Implement the V1 UX changes on the existing patient onboarding prototype and reflect them in the PRD.

You know the product and the pivot, so this skips the background and goes straight to what changes, where, and what still needs an input before you can build it.

---

## Artifacts

| Artifact | Path |
|---|---|
| Prototype (source of truth) | `patient-onboarding-v2-prototype.html` |
| PRD | `prd-patient-onboarding-v3-2026-05-13.md` |
| Running issues backlog | `prd-issues-running.md` |
| War room change list (origin of most items) | `outputs/Meeting-Notes/2026-05-19-patient-conversion-war-room.md` |
| Pivot sign-off (prior context) | `outputs/Meeting-Notes/2026-05-15-patient-app-pivot-signoff.md` |

Prototype screen IDs you'll touch: `landing`, `quiz-1` to `quiz-4`, `preview` (`landscape-chart`, `options-grid`), `uncovered`, `explorer`, `consent-split`, `path-a-auth`, `path-a-confirm`, `path-b-confirm`, `app-home`, `app-detail`.

---

## Resolve before building these

Several P0 items are gated on an input that isn't decided yet. Starting them before the input lands risks rework.

- **Refractory phrasing** (items 34, 37): patient-readable wording is Lauran's pass and not final. Hold copy until she lands it.
- **MGADL proxy bands** (item 36): the war room transcript captured the bands inverted vs clinical convention (higher MGADL = worse symptoms). The PRD has the corrected mapping. Confirm with Megan before wiring, and Megan also sanity-checks the question wording for patient comprehension.
- **Conditions list** (item 39): needs the official roadmap-derived list from Martin. Don't build against the current AI-generated list.
- **Flow reorder + redundant CTAs** (items 40, 41): specific reorder and specific buttons are Lauran's pass, not yet specified.
- **Two-value model UI** (item 45): depends on Avinash's backend separating self-reported from records-verified values. UI can be designed in parallel but can't be truthfully wired until the backend lands.
- **Path B HIPAA-only consent step** (issue 7f / 0): blocked on the Kate Black legal decision (whether consent is a standalone screen or embedded in sign-up). Both save-banner buttons currently route to a sign-up placeholder.

---

## Change list (V1)

All P0 unless noted. "Needs" names the blocking input where one exists.

### Quiz (`quiz-1` to `quiz-4`)

| # | Change | Where | Needs |
|---|---|---|---|
| 34 | Add refractory-status capture. Generalized MG splits into refractory vs non-refractory, a major eligibility branch. Likely a follow-up after Q1 or Q3. | quiz flow | Lauran's phrasing |
| 35 | Add LRP4+ as a first-class option in Q2 antibody status. Not folded into "seronegative" or "I don't know." | `quiz-2` | none |
| 36 | Replace Q3 (treatment stage) with an MGADL proxy question that produces a band: "barely noticeable" to one end, "severe" to the other, "manageable"/"I don't know" to unknown. | `quiz-3` | Megan (bands + wording) |
| 37 | Single patient-readable phrasing for refractory generalized MG everywhere it appears (preview cards, match descriptions, app-home, app-detail). | all surfaces | Lauran's phrasing |

Guardrail: every quiz question keeps "I don't know / I'm not sure" as the **first** option, above all specific answers.

### Landing / uncovered branch

| # | Change | Where | Needs |
|---|---|---|---|
| 38 | Replace the in-prototype uncovered + explorer screens with a redirect to the marketing site's "Explore Resonata" page. Patients on an unsupported condition do not enter the prototype experience. Drop the `explorer` screen from V1. | `uncovered`, `explorer` | none |
| 39 | Source the condition selector from Martin's roadmap list, not the generated list. Tag each entry supported / coming-soon. Coming-soon entries route to the same marketing redirect with a "we'll let you know" capture. | `landing` (`condition`) | Martin's list (via Lauran) |

### Onboarding flow

| # | Change | Where | Needs |
|---|---|---|---|
| 40 | Reorder onboarding flow steps. Likely overlaps the consent-split reorder already applied 2026-05-18 (Path A leads). Confirm whether more reordering is needed after the quiz updates land. | flow sequence | Lauran's spec |
| 41 | Remove redundant CTAs. Pass for duplicate Continue / Save matches / Next pairs across quiz and preview. | quiz, `preview` | Lauran's pass |
| 42 | Remove sample patients. Replace embedded demo patient names/profiles with the user's own profile built from quiz answers. Check `app-home`, `app-detail`, `preview`. | `app-home`, `app-detail`, `preview` | none |

### Match presentation

| # | Change | Where | Needs |
|---|---|---|---|
| 43 | Apply sort order: FDA-approved before trials; trials descending by phase (3, 2, 1). Default order absent any user filter. Mirror Avinash's backend Linear ticket. | `preview`, `app-home`, `app-detail` | none |
| 44 (P1) | Surface phase explicitly on trial cards via a small badge in the meta line, so phase is visible without tapping into detail. | trial cards | none |
| 45 | Visual treatment for the two-value model: small indicator on the detail page showing self-reported vs records-verified (e.g. "Self-reported: AChR negative" / "From medical records: AChR+ confirmed 2025-08-14"). Coordinate with the incomplete-record indicator (issue 7e). | `app-detail` | Avinash backend |

---

## Guardrails (do not regress these)

These are PRD hard requirements. They constrain how the changes above are implemented.

- **No presumptive consent.** Every "we did X for you" string must be backed by an explicit user action.
- **No "Not a fit" surface (patient-facing).** Patients see Full and Maybe only. Mismatches are silently omitted, never shown as "ruled out."
- **Mobile-first.** Single column on phones. Form is the dominant above-the-fold element on landing. No image stealing form real estate.
- **Equal-weight match presentation.** 2-column grid (1-col mobile), no "+X more" truncation, microcopy that ordering is not ranking. The chart drives filtering; cards present without bias.
- **Maybe bucket color.** Muted bronze tokens (`#997950` / `#F5EFE5` / `#D4BA89`) are the 2026-05-18 stopgap, pending Megan's brand review. Don't revert to the vivid amber traffic-light palette.
- **DNAvisit needs an explanatory intro before the consent ask.** Highest-friction trust moment in synthetic testing. Explainer minimum.
- **No PHI in HubSpot.** Backend is the sole system of record. Marketing automation receives non-PHI fields only.

---

## Not in V1 scope (so you don't build it)

From `prd-issues-running.md` and PRD phasing. Context for you, not a build list.

- In-app explorer pathway for uncovered conditions (moved to V2).
- Caregiver mode (V4).
- Doctor-share / print-for-appointment feature (V3). Note: the Path A consent card already promises "a one-page summary you can take to your doctor"; if the live product doesn't produce that artifact yet, the bullet copy needs softening (issue 7d, Aakash to verify).
- Side-effect / treatment-burden data layer, filter / sort / compare on matches, cost / insurance transparency, algorithm transparency page (V3).
- Voice / audio read-aloud, larger-text accessibility pass (roadmap; verify WCAG text-resize on small card meta lines is at least not broken).
- Per-question running match counter (V3).
- Life-context quiz dimensions, real-name patient ambassadors, community surface (V4).

---

## Open questions for you to confirm

- **Sign-up screen.** The PRD flow step 5 is a sign-up screen (name, state, DOB) and issue 0 references a `signup` screen the save-banner buttons route to. The current prototype has no `signup` screen id, only `first-name-landing` on the landing screen. Confirm whether the sign-up screen exists, needs building, or is intentionally folded into landing.
- **Flow order.** Final step sequence after the quiz updates land (item 40) is still open; align with Lauran before reordering.
- **Path B consent screen** depends on the Kate Black decision; confirm the model before touching the consent-split to path-b-confirm sequence.
