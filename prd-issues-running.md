# PRD Issues — Patient Onboarding v2

Running list of issues, risks, and known gaps that need to be addressed in the PRD when we write it. Captured during prototype iteration. Each item should land in the PRD's "Open issues" / "Risks" / "Roadmap" / "Out of scope" sections as appropriate.

---

## P0 — Block launch / require explicit decision

### 0. "Email me a link" escape hatch vs. mandatory minimal sign-up

**Status update (2026-05-18):** Lauran decision — both actions on the save banner ("Save my matches" and "Email me a link to come back") route to a short sign-up form. Path B is **not** a passwordless / magic-link persistence model. The "email me a link" framing stays as a softer trust-frame for patients who don't want to commit to records-sharing, but operationally it still requires sign-up. Banner copy and CTAs updated 2026-05-18 to remove the "no signup needed" and "no other emails" claims.

**Still open — Kate Black sync (2026-05-19):**
- Does the "Save my matches" button route through a T&C acknowledgement step before sign-up?
- Does it route through a HIPAA authorization step before sign-up?
- Or does it land directly on the sign-up form, with T&C / HIPAA consent embedded as part of the sign-up?

**Why it still matters:** The interaction model between the save-banner button and the consent flow needs Kate's legal posture before we cement the screen sequence. Until then, both banner buttons route to the existing `signup` screen as a placeholder.

**Owner:** Lauran (decision after Kate sync); Aakash (implementation update once decided).

---

### 1. DNAvisit has no patient-facing website

**The issue:** DNAvisit is the third-party records-retrieval partner that pulls patient records from Health Information Exchanges. Patients are asked to authorize DNAvisit during the records consent step. DNAvisit has no patient-facing website — patients who try to look them up to verify legitimacy will find nothing.

**Why it matters:** All four synthetic patient personas independently flagged DNAvisit as the highest-friction trust moment in the flow. Real patients will Google DNAvisit before authorizing them. Finding nothing patient-facing will cause drop-off at the consent step at minimum, and may damage trust in Resonata by association.

**Options to mitigate:**
- A. Resonata builds an explainer page (or in-product micro-onboarding) about DNAvisit — who they are, what HIE means, security posture, retention, sub-processors. Push for DNAvisit to provide source material.
- B. Push DNAvisit to build a patient-facing landing page that we can link to from the consent screen.
- C. Evaluate alternative records-retrieval partners with stronger patient-facing presence.

**Recommendation:** Combine A and B. Resonata can ship an explainer page faster; DNAvisit landing page is a longer ask but worth pushing for. Document the partner risk in the PRD's risks section.

**Owner:** Product (Lauran) for the explainer page; Vishal for the partner conversation; Henry for legal posture on third-party identification in consent.

### 2. Match logic must be dynamic to quiz answers

**The issue:** In the prototype, the preview matches don't actually reflect quiz answers. Marcus (synthetic, ocular MG) flagged that selecting Ocular MG returned generalized-MG trial matches. Jasmine (synthetic, MuSK+) flagged that selecting MuSK returned AChR-coded matches.

**Why it matters:** The "precision matching" value proposition collapses if the buckets aren't filtered correctly. Patients now see only Full + Maybe (the "Not a fit" bucket was removed at the 2026-05-15 pivot sign-off), so a wrong-bucket assignment surfaces as a wrong-fit recommendation directly to the patient — there is no "ruled out" surface to absorb the error.

**Action for PRD:** Specify the matching logic per quiz answer combination. AChR / MuSK / Seronegative / Unknown × Generalized / Ocular / Juvenile / Unsure × Newly diagnosed / On treatment / Refractory / Stable. Some of these combinations may have very few matches (a real signal worth surfacing), but the buckets must be honest.

**Owner:** Vishal + Aakash for the matching logic; product spec needed.

---

## P1 — Should be addressed before broader launch

### 3. Side-effect profiles missing from care option detail (on roadmap)

**The issue:** Patients (Sarah, Dorothy, Marcus all flagged this) want to see side-effect profiles, treatment burden (visit count, infusion frequency, distance), and trade-offs *before* deciding to share records. Currently this data is not in the system. Detail pages now show a "coming soon" note instead of fake data, which is honest but a gap.

**Why it matters:** This is the information Sarah said she'd want before consenting to records retrieval, not after. Without it, patients can browse but can't make decisions, which limits the value of the semi-converted state.

**Action for PRD:** Specify the side-effect / burden data model. Where does the data come from? FDA labels, trial protocols, sponsor pages? How do we keep it current? What's the editorial standard? V1 can be partial (FDA-approved drugs only, expanded to trials in V2) but the data architecture should support both.

**Owner:** Product + clinical (Rajni). Possibly content/editorial role to define.

### 4. Caregiver mode — first-class user, not a workaround

**The issue:** Dorothy's synthetic review surfaced this: her daughter Linda is doing all the tapping, but the system has no concept of caregiver vs patient. They had to use Linda's email and pretend it's Dorothy's account. This applies broadly to elderly patients, refractory patients, anyone in crisis — probably 30–40% of MG patients.

**Why it matters:** Without caregiver mode, the product systematically excludes a large patient segment from honest engagement. HIPAA implications are non-trivial — caregivers acting on behalf of patients have specific consent requirements.

**Action for PRD:** Define the caregiver model. Patient + designated caregiver(s)? Both can act on the account? What does each see? How is consent captured? Henry needed for legal posture.

**Owner:** Product + legal (Henry).

### 5. Doctor coordination — share / print / discuss with provider

**The issue:** Sarah's biggest gap — and a universal need for collaborative-decision patients. She can't make MG treatment decisions without involving her neurologist. Currently the prototype has no share-with-doctor or print-for-appointment feature. We've added a "comprehensive match report for your doctor" benefit to the Path A consent card, but the actual feature isn't designed.

**Why it matters:** Most MG patients make care decisions with their specialist. A precision matching tool that doesn't support that workflow is incomplete.

**Action for PRD:** Spec the "share with my doctor" feature. Email a summary? Print-friendly view? Branded PDF? Provider portal for receiving these? Path A unlocks this as a benefit; Path B should also have a usable version (even if less detailed).

**Owner:** Product + design.

### 6. Algorithm transparency / methods page

**The issue:** Marcus's biggest catch. No methodology page explains how matches are computed. No methods, no model card, no source citation commitment. For a "precision matching" product, the precision is currently a black box.

**Action for PRD:** Decide what we'll publish. Inputs (quiz + records), match scoring approach, confidence intervals (or bands), source citation commitment per match (NCT for trials, FDA label for approved). May need engineering feasibility check.

**Owner:** Product + engineering.

### 7. First-email design

**The issue:** Three of four synthetic patients explicitly said the *first follow-up email* determines whether they come back. The email pattern is downstream of the prototype but is part of the same conversion problem. Currently undesigned.

**Why it matters:** Dorothy will churn on the word "upgrade." Jasmine will churn on generic AChR content for a MuSK+ patient. Marcus will burner-test the spam restraint. Sarah will close anything chirpy. The email is high-stakes.

**Action for PRD:** Specify first-email design with the same rigor as the onboarding flow. Trigger conditions, content per patient state (semi vs full converted), tone guardrails (no "upgrade", no "we miss you"), what happens if matches don't change.

**Owner:** Product + lifecycle marketing if/when that exists.

### 7f. Path B has no HIPAA-only consent step in the flow

**The issue:** The consent-split screen's "Connect later" button now reads "You'll sign a HIPAA authorization only. No records retrieved" — but the next screen in the flow is `path-b-confirm` ("Your matches are saved. Take your time."). There's no screen between them where the HIPAA-only authorization is actually presented and signed. The prototype skips the consent.

**Why it matters:** If the helper text promises a HIPAA authorization step but the patient never sees one, we're either (a) lying about the consent model or (b) burying the consent into the sign-up form earlier in the flow without making that visible. Either breaks the "no presumptive consent" hard requirement.

**What to design:**
- Insert a `path-b-auth` screen between consent-split and path-b-confirm that presents a single HIPAA authorization block, uses the same scroll-to-bottom enforcement pattern as path-a-auth, and routes to path-b-confirm on submission.
- OR — if the HIPAA authorization is embedded in the sign-up form (Item 0 / Kate Black decision), redesign the sign-up screen to show that block explicitly, and update the consent-split helper text to match.

**Owner:** Product (decision on which model); Aakash (implementation). Blocked on Kate Black sync (2026-05-19) for the same reason Issue #0 is blocked.

### 7e. Incomplete-record indicator pattern on match cards

**The issue:** After Path A records retrieval, some matches will be fully verified against the patient's records while others can't be — the patient's records may be missing a lab value, an antibody test, a recent imaging study, or another data point the match logic needs. Today the prototype has no design pattern for this state: a card either says "approximate" (semi-converted state) or shows full verified info (post-Path A state, not yet designed). There's no in-between visual that says "verified against most of your records, but we couldn't confirm X."

**Why it matters:** Trust posture. If we silently treat partial-records matches as "verified," patients will eventually catch the lie when they hit a site and find out they don't meet a criterion we never had data on. If we lump partial-records matches in with "approximate," we waste the value of the records they did share. The honest middle state needs its own visual + microcopy.

**What to design:**
- Card-level indicator (badge, icon, sub-label) for "verified with gaps"
- Detail-page expansion: which specific criteria were verified vs. which couldn't be checked, with a clear note about what would close the gap (e.g., "We don't have a recent anti-AChR titer in your records — ask your neurologist to share this and we can confirm.")
- Aggregate state on the chart: if a high % of matches in a bucket are "verified with gaps," consider surfacing that at the bucket level

**Owner:** Product + design. Defer to V1.5 / V2 once Path A post-retrieval surface is designed.

### 7d. Verify doctor-report bullet aligns with live product

**The issue:** Path A consent card now promises "A one-page summary you can take to your doctor" as a benefit. The live product needs to actually produce that artifact in the form the bullet implies (one page, doctor-facing, take-with-you format). If the live product delivers a different artifact (multi-page match list, in-app share link, branded PDF, etc.), the bullet copy needs to match.

**Why it matters:** Sarah's biggest gap (Issue #5) was doctor coordination. A bullet that overpromises in scope or format will undercut the trust posture we're building elsewhere.

**Action:** Aakash to confirm what the doctor-share artifact currently is. If the one-page summary doesn't exist yet, either soften the bullet copy or schedule the artifact for V1.

**Owner:** Aakash (verification); Product (copy or feature decision).

### 7c. Verify trial-site distance sort in live product

**The issue:** KD flagged in the 2026-05-15 pivot sign-off that the live patient app is supposed to sort trial sites by distance from the patient's zip, but he wasn't 100% sure the sort applies consistently across all surfaces (preview cards, app-home bucket views, app-detail page site listings). The PRD now commits to "distance to nearest site" as the trial card locator pattern (see Item 10 in the prototype change list and the trial card meta updates in `patient-onboarding-v2-prototype.html`). If the live sort doesn't actually surface the nearest site first, the copy will lie.

**Why it matters:** "47 mi to nearest site" only works if the system actually picks the nearest site. If the live product is sorting alphabetically, by enrollment status, by sponsor priority, or anything else, the meta line is misleading at best, false at worst. Patients will notice when they tap through and find a closer site that wasn't the one cited.

**Action:** Aakash to verify the current sort logic across preview / app-home / app-detail. Confirm:
- Trial sites are sorted by distance from patient zip ascending.
- The "nearest site" string on the card references the actual top-of-list site.
- If patient zip is not provided (Q4 optional), the card falls back to "N sites nationwide" with no distance.
- If verified working, no action. If broken or inconsistent, file a bug and tag P0.

**Owner:** Aakash (verification); Product (confirm fallback behavior for missing zip).


### 7b. Uncovered-indication content path is designed but not built (Screens 11 + 12)

**The issue:** The prototype shows the right pathway for patients whose condition we don't yet match — screen 11 ("We haven't built matching for this condition yet") offers a "Browse what's known about my condition" CTA that leads to screen 12, an education explorer with treatment landscape cards, "stories from patients like you," and links to deeper content. **This functionality does not exist yet.** We have no condition-agnostic education content library, no patient-story repository, no editorial pipeline.

**Why the pathway is in the prototype anyway:** It's the right design direction. Patients who land in the uncovered branch shouldn't get a dead-end "thanks, bye." The "while you wait, here's what we know" framing is a real value offer — it's what keeps the email subscription warm and turns the eventual coverage announcement into a welcome moment instead of a cold pitch.

**What needs to happen:**
- Decide whether the V1 uncovered branch ships with the explorer pathway visible (with a "coming soon" treatment) or hidden until the content exists.
- Plan the content production: who writes the per-condition treatment landscape pages, the patient stories, the "how care decisions get made" content. Editorial standard, source attribution, frequency of updates.
- Build the backend to serve condition-specific content. Probably a CMS layer.

**Owner:** Product + content/editorial role (TBD).

---

### 8. MuSK+ content investment

**The issue:** Jasmine flagged that MuSK-positive content is structurally underweight in the matches library. Whether that's a prototype quirk or a real content gap, V1 needs MuSK-specific matches that aren't a token cameo.

**Owner:** Product + clinical.

### 9. Cost / insurance transparency on match cards

**The issue:** Sarah and Dorothy both flagged this. Especially for FDA-approved drugs (Vyvgart, Soliris, Ultomiris are notoriously expensive). "Typically covered by Medicare / commercial with prior auth" is the kind of one-liner that changes which match a patient pursues.

**Owner:** Product + content.

### 10. Voice / audio option for content

**The issue:** Dorothy's accessibility ask. On a bad day she can't focus on text. A read-aloud option for key content (matches, consent screens, education explorer) would help.

### 11. Larger-text accessibility option

**The issue:** Dorothy and Sarah both flagged. Body text is fine; meta lines on cards ("Open enrollment · Sites in 14 states") are small. WCAG-compliant text-resize handling needs verification.

### 12. Phone number / human contact on the marketing surface

**The issue:** Dorothy specifically. For a serious healthtech handling ePHI, the absence of a human touchpoint is a credibility issue, not a contact-volume issue.

### 13. Life-context quiz dimensions

**The issue:** Jasmine — the quiz captures clinical reality but not life context. Pregnancy, career, lifestyle, oral-vs-infusion preferences. These are the matching variables that matter most for patients in their 20s/30s.

**Action for PRD:** Decide whether life-context lives in the quiz, in a separate "tell us more" step, or as filters on the matches list.

### 14. Real patient stories with opt-in identification

**The issue:** Jasmine flagged anonymous "Patient · 47, Midwest" stories as reading like stock-photo wellness content. Opt-in patient ambassadors with first names and photos would build trust dramatically. Operational lift.

### 15. Community / peer surface

**The issue:** Jasmine — "find me one other MuSK+ woman in her 20s." Not in scope for V1 but a known competitive moat against Facebook groups.

### 16. UI regression check on Saba / Maya deploy

**The issue:** During the 2026-05-15 pivot sign-off, Vishal mentioned that a recent deploy from Saba and Maya may have re-introduced UI regressions on the live patient app — color shifts in the match cards, alignment drift on the consent screens, possible missing micro-interactions. Not yet verified on the live product.

**Action:** Walk the live patient app screen-by-screen against the prototype source-of-truth. Capture any visual regressions, file bugs, prioritize as P0/P1 based on whether they affect trust posture (color/copy) vs. cosmetic (alignment).

**Owner:** Aakash (verification); Saba / Maya (fixes if regressions confirmed).

### 17. Condition-specific copy on marketing site

**The issue:** The website's marketing copy is currently generic ("precision care options"). For the conditions Resonata actually covers (V1: MG), the marketing copy should speak the condition's language — MG-specific terminology, treatment landscape callouts, the patient-archetype framing (newly diagnosed / refractory / etc.). Patients searching for "myasthenia gravis trial" should land somewhere that visibly understands MG.

**Why it matters:** Generic copy underperforms for SEO and trust. A MuSK+ patient who lands on a page that doesn't even mention MuSK leaves immediately.

**Action:** Decide which conditions get dedicated landing pages on the marketing site, what content goes on each, and who owns content production. V1 minimum: MG landing page that mirrors the in-product condition framing.

**Owner:** Product + marketing (TBD owner for condition-specific copy).

---

## Engineering / setup notes

### 16. Project folder location is wrong

**The issue:** The Resonata project folder is at `~/Documents/Claude/Projects/patient-onboard-2-path/` instead of inside `~/Developer/resonata/`. This is contrary to the standing CLAUDE.md instruction. Side effect: cowork sessions started here can't reach the resonata `_memory/` folder, so feedback files have to be staged here for later lift.

**Action:** Move the project directory to `~/Developer/resonata/patient-onboard-2-path/` (or wherever it's meant to live within the resonata folder structure). Re-open the cowork session from there.

**Owner:** Lauran, or have Claude Code move it.

---

## Out of scope (mentioned, not pursued for V1)

- Specialist finder / MG Centers of Excellence directory (Marcus suggested)
- Data export (JSON / CSV of patient profile + matches) — Marcus
- Account deletion + privacy policy in chrome — Marcus (privacy policy yes, deletion mechanic yes, just not in the chrome of every screen)
- A "what changed since last visit" view — Marcus
- Side-by-side comparison view — Marcus, Sarah (P1 candidate, deferred for now)
- Sortable/filterable matches list — Marcus (P1 candidate; partially addressed by chart-as-filter on preview)

---

*Updated 2026-05-13 during prototype iteration round 2.*
