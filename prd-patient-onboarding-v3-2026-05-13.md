# PRD: Patient Onboarding v2 — Semi-Conversion Pivot

**Author:** Lauran Hazan
**Date:** 2026-05-13
**Status:** Draft v1
**Target launch:** TBD

---

## TL;DR

Rewrite of patient onboarding around **records as upgrade, not gate**. Patients answer a 4-question quiz, see approximate matches up front, then choose Path A (connect records now → verified precision matches in ~8 hours) or Path B (semi-converted state with persistent approximate matches and one-click upgrade later). The pivot's purpose is to capture the ~92% of demand we currently lose at the records-consent cliff and turn it into a measurable, growable cohort.

Validated by synthetic patient panel ([cross-patient synthesis](research-synthesis/patient-onboarding-v2-usability-synthesis-2026-05-13.md)) and reviewed with the patient advisor cohort. Working prototype: [`patient-onboarding-v2-prototype.html`](../patient-onboarding-v2-prototype.html). Open issues running list: [`prd-issues-running.md`](../prd-issues-running.md).

---

## Problem

Today, patient onboarding requires HIPAA + medical records consent before the patient sees anything useful. ~35% of visitors start consent. ~8% complete it. ~4% apply to anything. The other ~92% leave with no relationship, no data, no return path.

Three structural reasons:
- Patients with serious chronic conditions are wary by default. They've been over-marketed-to and over-promised by other healthtech.
- "Records or nothing" forces a high-trust decision before any value is shown.
- We measure only what closes. The 92% who don't is invisible to the product, the team, and sponsors.

## Goals

1. **Increase Path A (records-now) conversion** above the current ~8% baseline.
2. **Capture Path B (semi-converted) as a measurable cohort**, currently at 0%.
3. **Surface demand signal per condition**, not just per converted user — feeds multi-condition expansion sequencing.
4. **Maintain trust posture** — no presumptive consent, no growth-hack tactics. The product earns trust in deposits.

## Non-goals (V1)

- Caregiver mode (Phase 2 — see [issue #4](../prd-issues-running.md)).
- Doctor-portal integration (separate workstream).
- Side-effect / treatment-burden data layer (on roadmap — see [issue #3](../prd-issues-running.md)).
- Filter / sort / compare UI on the matches list (V2).

## Audience

MG patients, all stages — newly-diagnosed (Marcus archetype), mid-journey collaborative deciders (Sarah archetype), refractory veterans (Dorothy archetype), young professionals planning life around their condition (Jasmine archetype). Tech proficiency varies wildly. Mobile is the dominant device. Caregivers (especially adult children of older patients) often co-pilot the experience.

---

## Solution

### Flow

1. **Landing** — email + condition selector. Email is a magic-link key, not a marketing list. Headline: "Find care options for **you**." Mobile-first; form is the dominant element above the fold.
2. **Indication check** — Covered branch (V1: MG only) or Uncovered branch.
3. **Quiz — 4 questions** (regenerated 2026-05-18 per Martin's "halfway down the funnel" target — see [`outputs/quiz-question-regeneration-2026-05-18.md`](outputs/quiz-question-regeneration-2026-05-18.md); updated 2026-05-19 war room — see [`outputs/Meeting-Notes/2026-05-19-patient-conversion-war-room.md`](outputs/Meeting-Notes/2026-05-19-patient-conversion-war-room.md)):
   - Q1: MG form (generalized / ocular / juvenile / I'm not sure). Refractory captured separately as a follow-up (see Refractory-status capture below).
   - Q2: Antibody status (AChR+ / MuSK+ / LRP4+ / seronegative / I don't know). LRP4+ is a first-class option per 2026-05-19 war room, not folded into "seronegative" or "I don't know."
   - Q3: Day-to-day severity, MG-ADL proxy (barely noticeable / manageable / severe / I don't know). Scoring bands: severe → MG-ADL ≥6 (qualifies for severity-gated options); barely noticeable → MG-ADL <6 (excludes severity-gated options); manageable and I don't know both → unknown (downgrade severity-gated options to Maybe, do not exclude). *Note: the 2026-05-19 war room transcript captured these bands inverted relative to clinical convention (higher MG-ADL = worse symptoms). The mapping above follows clinical convention. Verify with Megan before backend wires this up.*
   - Q4: Zip code (optional — for trials within 100 miles filter)
   - **Refractory-status capture:** A follow-up question after Q1 (or after Q3, TBD) captures whether the patient has tried multiple MG treatments without success. Refractory generalized MG is a major eligibility branch in the current trial set; cannot be inferred from Q1 form alone. Patient-readable wording pending Lauran's pass — the clinical term "refractory generalized myasthenia gravis" needs translation to something like "generalized MG that hasn't responded to first-line treatment."
   - Each question has an "I don't know / not sure" first-class option.
4. **Preview matches** — Treatment landscape data viz at top (2 stacked bars: Full / Maybe, each split into approved drugs + trials, tap to filter); equal-weight 2-column card grid below; "These matches are yours" magic-link banner at bottom. Options that don't fit the profile are silently omitted from the patient view (see "No 'Not a fit' surface" under Hard requirements).
5. **Sign-up** — name, state, DOB. Explicit account-creation moment.
6. **Consent split** — Path A (most precise) vs Path B (browse later). Both presented as equal options with full benefit lists.
7. **Path A** — HIPAA + DNAvisit retrieval auth → "we're getting your records" confirmation → verified matches in ~8 hours.
8. **Path B** — HIPAA-only consent → semi-converted app entrance.

### Uncovered branch

If the patient selects a condition Resonata doesn't yet match (per the supported-conditions list sourced from Martin's roadmap), the app does NOT enter the prototype experience. The patient is redirected to the marketing site's "Explore Resonata" page, which holds them with content and captures demand. **Reversed 2026-05-19 war room** — supersedes the prior in-prototype `uncovered` + `explorer` screens.

V1 implication: no in-app explorer pathway. The in-app explorer moves to V2+ ([issue #7b](../prd-issues-running.md)). Demand-pool capture happens at the marketing redirect, not inside the app.

### Semi-converted app surface

Persistent matches across sessions. Browse, bookmark, get notified on new options. Soft "connect records to apply" upgrade prompts. Care option detail pages show everything — no gating; Apply is presented as a flow step ("connect records to start your application").

---

## Hard requirements (V1)

- **Match logic must be dynamic to all four quiz answers.** Synthetic testing surfaced a real bug: ocular MG profile returned generalized matches; MuSK+ returned AChR-coded options. Patients see only Full + Maybe buckets, so a wrong-bucket assignment is a wrong-fit recommendation — the trust cost is direct.
- **No "Not a fit" surface — patient-facing.** Per 2026-05-15 pivot sign-off, the "Not a fit" / mismatch bucket is removed entirely from V1. Patients see Full + Maybe only. Internally the system still knows which options don't fit a profile; those are silently omitted from the patient view. Sponsor-facing reporting may aggregate "did not match" counts (no individual patients), but that is a separate B2B workstream. Rationale: sponsor liability (showing patients "this drug isn't for you" is a legal exposure sponsors block) and competitive intelligence exposure (revealed mismatches expose matching logic to competitors).
- **DNAvisit must have explanatory introduction before consent.** All four synthetic patients flagged DNAvisit as the highest-friction trust moment. Resonata-built explainer page minimum; ideally DNAvisit ships their own ([issue #1](../prd-issues-running.md)).
- **No presumptive consent.** Every "we did X for you" UI string must be backed by an explicit user action. See [feedback-trust-posture-no-presumptive-consent.md](../feedback-trust-posture-no-presumptive-consent.md).
- **Mobile-first.** Single column on phones. Form is the dominant above-the-fold element on landing. No image stealing form real estate. See [feedback-mobile-first-design.md](../feedback-mobile-first-design.md).
- **Equal-weight match presentation.** Cards in a 2-column grid (1-col mobile), no "+ X more" truncation, explicit microcopy that ordering is not ranking. The chart drives filtering; the cards present without bias.
- **HIPAA authorization is granular.** Separate from records-retrieval consent. Patients can authorize use of submitted data without authorizing record retrieval (Path B).
- **Two distinct email systems, managed separately.**
  - *App-initiated transactional emails* (magic-link auth, password reset, account confirmations, in-flow notifications driven by the patient's own actions on the app) are sent by Resonata's own infrastructure. Out of scope for marketing automation.
  - *Lifecycle and cohort comms* are sent by HubSpot via opaque-ID cohort tags and event triggers Resonata emits. HubSpot stores no PHI. Cadence, copy, and journey design are defined by the marketing team; baseline journey maps for that workshop live in `outputs/journey-maps/`.
  - Brand-voice constraints (no presumptive consent, no "we miss you" patterns, no urgency theater) apply to both systems.
- **Quiz design rule — "I don't know" is always first.** Every quiz question presents "I don't know" / "I'm not sure" as the first option (above all specific answers). The matching logic treats it as a graceful fallback — patient gets broader approximate matches across the missing dimension, never excluded. Rationale: the population we most want to capture (newly diagnosed, low health literacy, caregiver-on-behalf) is the population least likely to know specific clinical answers. Burying the escape hatch creates abandonment and pressures wrong answers.
- **No PHI in HubSpot.** Backend is the sole system of record for any data linked to a patient identifier. HubSpot (and any other marketing automation surface) receives non-PHI fields only (likely first name + email + condition string for marketing automation; boundary to be documented per action item, 2026-05-19 war room). Live precedent: a recent $5M fine on a healthtech startup cited in war room.
- **Two-value data model — self-reported and records-verified.** Patient-supplied values and records-verified values are stored separately, each with its own timestamp. Same field can hold a self-reported value and a records-verified value side by side. The UI surfaces provenance wherever both can exist for the same field (e.g., "Self-reported: AChR negative" / "From medical records: AChR+ confirmed 2025-08-14"). Per 2026-05-19 war room.
- **Match list sort order — FDA-approved before trials; trials descending by phase (3 → 2 → 1).** Default order applies absent any user filter. Phase is surfaced explicitly on trial cards via a dedicated badge so patients can see what phase they're looking at without tapping into detail. Per 2026-05-19 war room.
- **Conditions list reflects production matching support.** The condition selector on the landing page is sourced from the roadmap (owned by Martin), not generated. Each entry tagged `supported` or `coming-soon`. Coming-soon entries route to the marketing redirect, not into the matching flow. Per 2026-05-19 war room.

---

## Success metrics

### Primary

| Metric | Why it matters |
|---|---|
| Quiz completion rate | New top of funnel; replaces "started consent" |
| Path A conversion rate | Apples-to-apples with current ~8% baseline |
| Path B (semi-conversion) rate | Currently 0%; the pivot's main capture mechanism |
| Semi → Full upgrade rate, by week | The pivot's value depends on this curve |
| Time-to-records for upgraders | Tells us whether semi-converted is a holding pattern or a step |

### Secondary

- First-email open rate, per cohort (Path A vs Path B)
- Magic-link return rate (semi-converted patients clicking back)
- Per-condition demand pool from uncovered branch (drives V2/V3 condition rollout)
- Quiz drop-off rate per question (catches friction)
- Mobile vs desktop completion split

---

## Top open issues (full list: [`prd-issues-running.md`](../prd-issues-running.md))

1. **Magic-link escape vs. mandatory minimal sign-up.** Current preview screen offers an "Email me a link" CTA that promises persistent access without sign-up. Operationally we may need at least name + HIPAA-only consent. Decide before launch. 
2. **DNAvisit has no patient-facing website.** Universal trust friction in synthetic testing. Mitigation: Resonata-built explainer page (faster), pushing DNAvisit to ship their own (longer ask). 
3. **Match logic must be dynamic to quiz answers.** Real bug surfaced in synthetic testing. Hard requirement, not a UX issue.
4. **Matching logic strategy.** Does the existing matching algorithm absorb the new questionnaire rules (refractory, LRP4, MG-ADL proxy) directly, or does a new business-rule layer sit in front of it? Decision needed before the new questionnaire is wired to the matching pipeline. Owner: Avinash + Aakash + Lauran. Per 2026-05-19 war room. 

---

## Phasing

**V1 (this PRD).** Onboarding flow. Quiz (with LRP4, MG-ADL proxy, refractory capture per 2026-05-19 war room). Preview matches with data viz and sort order (FDA-approved before trials; trials desc by phase). Consent split. Path A authorizations. Semi-converted app surface (basic — persistent matches, browse, bookmark, upgrade). Uncovered branch redirects to marketing site's "Explore Resonata" page (no in-app explorer in V1). **Dev-env deploy target: 2026-05-22 (Friday).**

**V2.** Revised patient app — simplified categories, patient-facing language dictionary. In-app explorer pathway for uncovered conditions (moved from V1 per 2026-05-19 war room).

**V3.** Side-effect / treatment-burden data layer. Filter / sort / compare on matches list. Doctor-share / print-for-appointment feature (addresses Sarah's biggest gap). First-email design pass. DNAvisit explainer microsite. Cost / insurance transparency on cards. Algorithm transparency / methods page. **Administration method (oral / infusion / IV) as card attribute and filter** (pending Aakash verification that the data is in care-option records today, per 2026-05-19 war room). **Per-question running match counter on the quiz** (Vishal's idea from 2026-05-15 pivot sign-off — shows patients a live count of matches narrowing per question to anchor commitment; deferred from V1 because it requires real-time match-counting backend that we don't want on the critical path for initial release).

**V4.** Caregiver mode. Multi-condition expansion driven by V1 demand-pool data. Patient stories with opt-in real-name ambassadors. Life-context quiz dimensions (pregnancy, career, lifestyle).


---

## Decisions captured during prototype iteration

- Mobile-first design across all screens.
- "Records as upgrade, not gate" framing across all consent surfaces.
- ~~"Not a fit" bucket as a brand-level differentiator (kept visible and explained, not hidden).~~ **Reversed 2026-05-15 at pivot sign-off.** Removed entirely from V1. Two reasons: (1) Pfizer-class sponsor liability — showing patients "your drug isn't a fit" is a legal exposure sponsors will block; (2) competitive intelligence exposure — competitors infer our matching logic from which profiles get "not a fit" tagged against their products. Trade-off accepted: we lose the "we tell you the truth" trust signal in exchange for sponsor viability. The "Maybe" bucket carries the soft-trust posture without the cost.
- Equal-weight card grid with chart-as-filter to remove implicit ranking.
- **Color palette softened on Maybe bucket (2026-05-18 stopgap, pending Megan brand review).** Vivid amber `--partial-*` tokens dialed to muted bronze (`#997950` / `#F5EFE5` / `#D4BA89`) so the Maybe bucket reads as "additional info" rather than "caution." Per 2026-05-15 pivot sign-off, the traffic-light frame is off-brand for a patient population that has been over-coded as patients-at-risk by other healthtech. Megan to sign off on the full token-by-token rework.
- All gating removed from care option detail page — patients see everything; Apply is presented as a flow step requiring records, not a visual lock.
- Treatment landscape data viz placed above personal results, with universe stat as context below.
- Tagline: "Precision care options for **you**." Headline: "Find care options for **you**."
- Logo: botanical snowflake (uniqueness metaphor — no two snowflakes alike, ties to "for you" framing).

---

## Stakeholders

| Role | Person | What they care about |
|---|---|---|
| Product lead (this work) | Lauran | Whole spec |
| Product lead (B2B) | KD | Sponsor visibility into semi-converted demand |
| Clinical advisor | Megan | Clinical accuracy, terminology, patient advisory |
| CEO | Martin | Strategic priority, metrics, condition expansion |
| Eng lead | Avinash | Architecture feasibility, DNAvisit integration |
| Lead engineer | Aakash | F/E Implementation, Analytics implementation |
| Data / integration | Anupam | Backend data model (self-reported vs records-verified), HubSpot boundary |
| Commercial advisor | Kristen | Sponsor-facing implications |
| Legal | Henry | HIPAA posture on magic-link, semi-converted state |

---

*Living document. Updates as prototype iterates and open issues resolve.*
