# Patient Onboarding v2 — Cross-Patient Usability Synthesis
**Date:** 2026-05-13
**Prototype:** patient-onboarding-v2-prototype.html (semi-conversion pivot)
**Panel:** Sarah Chen (52, generalized MG 3yr, AChR+, moderate tech) · Marcus Williams (34, ocular MG 6mo, AChR+, high tech) · Dorothy Martinez (68, refractory generalized MG 18yr, low tech, daughter-dependent) · Jasmine Patel (28, generalized MG 2yr, Anti-MuSK+, digital native)

---

## Executive summary

**The pivot works.** All four patients completed the flow, and all four would have abandoned a records-first version. "Records as upgrade, not gate" is the right architectural call, validated independently by every patient on the panel — three of four would explicitly take Path B in this session and upgrade later, and the fourth (Dorothy) only completed Path A with her daughter doing the work.

**The visible architecture earns trust faster than the brand surface does.** The strongest signals — Path B existing, the "Not a fit" bucket on the preview screen, the "approximate matches" disclaimer that persists into the app, the justified gate on detail screens — are universal wins. Patients trust the *system* more than they trust the *brand* right now. Visual identity and copy moments need to catch up to what the architecture is doing.

**The biggest unaddressed risk is DNAvisit.** All four flagged the records partner as a moment of trust friction — Sarah and Marcus would freeze at the authorization screen, Dorothy would close the tablet without her daughter, and even Jasmine (who appreciated naming the partner) wants more upfront context.

**A real bug surfaced.** Marcus noticed that the preview matches don't dynamically reflect quiz answers — an ocular MG profile shows generalized-MG matches, and a MuSK+ profile shows AChR+ trial names. Jasmine independently flagged the MuSK content gap as a structural product gap, not a prototype quirk.

---

## Universal wins (3+ patients agreed)

**1. The "Not a fit" bucket on the preview screen.** All four patients named this as the strongest single trust signal in the prototype. Sarah called the framing — "We're showing you these so you know we considered them — and ruled them out for you" — "the single best sentence on the site." Marcus said it's "better than 90% of patient-facing products which just show you the green checks." Jasmine said she'd screenshot and share it. This pattern is unprecedented in patient-matching UX and should be elevated in brand positioning.

**2. The Path A / Path B consent split.** Every patient explicitly stayed in the funnel because Path B exists. Sarah: "letting me stop without losing my matches is the single most important design decision in this whole flow." Marcus: "the choice to let me browse and upgrade later is the same logic Plaid figured out for banking five years ago." Dorothy: "the fact that I could leave without giving them everything and still come back" was a major trust signal. Jasmine: "the 'connect later' choice exists because of people like me. Thank you for not blocking my exit."

**3. The "approximate matches" honest framing.** The yellow disclaimer banner that persists from preview into the semi-converted app home was named by Sarah, Marcus, and Jasmine as a credibility multiplier. Sarah: "I'd believe everything else this product says because of that one badge." Marcus: "You are not pretending you have certainty you don't have. That's grown-up product design."

**4. The 3-question quiz being actually short.** All four patients made it through. Marcus called three questions "below my abandon threshold." Dorothy explicitly noted the promised 2-minute time was honest. The "I'm not sure / I don't know" options on each question were called out as good design by Sarah, Marcus, and Jasmine.

**5. The treatment-stage question (Q3).** Sarah said seeing "On treatment, looking for better options" as a distinct option felt "validating in a way I don't always feel allowed to be." Dorothy similarly responded to "Tried more than 3 things that didn't work well enough" as the first form question that acknowledged her real situation. The middle phases of MG life (between newly-diagnosed and refractory) are typically erased on other tools. Don't lose this.

**6. The detail-page "Why we matched you" explainability.** Sarah, Marcus, and Jasmine all noted the per-card match explanation as a trust signal. Marcus called it "a real explainability surface, even if it's thin." Worth investing more here.

---

## Universal gaps (3+ patients agreed)

**1. DNAvisit appears unannounced.** Sarah ("the moment I would freeze"), Marcus ("I have never heard of these people. Who are they? What's their SOC 2 status?"), and Dorothy ("I'd want to see a name. A face. A hospital affiliation") all named the records partner as the highest-friction trust moment in the flow. Even Jasmine — who *liked* that DNAvisit was named at all rather than buried — wants more upfront context. **There is no version of this product that ships without a DNAvisit introduction moment.**

**2. The matches aren't actually differentiated by quiz answers (real bug + content gap).** Marcus caught it as an explicit bug: his ocular MG profile shows generalized-MG matches. Jasmine caught it as a structural content gap: all "Full matches" are AChR-coded even when MuSK+ is selected. This is the single thing that most undermines the "precision matching" value proposition. Both patients would lose trust in the email follow-ups if matches don't match their inputs.

**3. No doctor coordination anywhere.** Sarah named this as her biggest single gap — "any decision I make about MG treatment, I make *with* my doctor, not instead of him." Dorothy implicitly needs it (her specialist is the trusted authority). Marcus wants a specialist finder. Jasmine wants shareable cards. The "share with my doctor" / "print for my appointment" feature is missing and is required for collaborative-decision patients (which is the majority).

**4. The visual surface feels generic.** Sarah ("competent but a little cold... no warmth in the imagery, no human face"), Jasmine ("competent. forgettable. I would not screenshot a single screen of this for aesthetic reasons"), and Dorothy ("no human face for credibility") all said the visual identity doesn't yet match the seriousness of the architecture. Marcus didn't comment on visuals but said the headline copy "is the linguistic equivalent of beige." The new snowflake-and-hero-photo direction we just took addresses some of this; copy still needs work.

**5. Side-effect / treatment-burden information missing on match cards.** Sarah ("that's the information I need *before* I decide whether to connect records"), Dorothy (wants "how many appointments, how many infusions, how far would I have to drive"), and Marcus (wants confidence intervals and contributing criteria per card). The current "Why we matched you" line is a good start but underdelivers on what patients actually need to evaluate fit.

**6. No cost / insurance information.** Sarah, Dorothy, and Marcus (via the "free for patients" question — "so who's paying?") all flagged this. Eculizumab is famously expensive; Vyvgart has insurance access patterns patients want to know. For Dorothy, "this matters as much as efficacy." For Sarah, "knowing what insurance typically covers would change which match I'd actually pursue."

---

## Patient-specific insights (what each one caught that others missed)

**Sarah Chen — doctor as missing third-party stakeholder.** Sarah is the only patient who explicitly frames her decision-making as collaborative with her neurologist. She brings a binder. She forgets questions in the appointment. She wants to print her matches and bring them in. Every other tool she's used ignores this; the prototype does too. The "share / print / discuss with doctor" feature is her biggest unmet need — and she's representative of the largest patient segment in MG (mid-stage, collaborative deciders).

**Marcus Williams — algorithm black box and missing power-user surface.** Marcus is the only patient who looked for a methodology page, a denominator on "4 strong matches," a filter/sort, a comparison view, source citations to ClinicalTrials.gov, a privacy policy link in the chrome, a data-export commitment, and a deletion right. He's also the only one who'd burner-test the email-restraint promise and cross-check matches against primary sources before trusting the system. His specific request: a methods page + filter/sort/compare + denominator transparency. **Also caught a literal bug:** ocular profile showing generalized matches.

**Dorothy Martinez — caregiver mode is the biggest hole in the entire product.** Dorothy is the only patient with a caregiver actively doing the tapping. Her daughter Linda is filling out the form, reading the legal text, picking the password, deciding which hospitals to authorize. The system has no concept of this. They used Linda's email and Dorothy quietly noted "but then it's not really my account." Caregiver-as-first-class-user is a structural feature gap with HIPAA implications, not a small UX nit. Dorothy also caught: no phone number anywhere, the word "upgrade" feeling like a phone plan, no voice/audio option, no visible hospital affiliation or doctor name to credibility-check Resonata against.

**Jasmine Patel — the quiz captures clinical reality but misses life context.** Jasmine is the only patient who asked: where's pregnancy? Where's career compatibility? Where's "oral vs infusion"? The current quiz is medically scoped, not life-scoped. For Jasmine and patients like her (20s/30s women planning families, professionals managing disclosure, anyone with lifestyle-dependent treatment preferences), the missing filter dimensions are the ones that actually drive their care decisions. She also caught: MuSK+ content is structurally underweight as content, not just as quiz routing; patient stories are anonymous when they should be opt-in with real first names and photos; no community surface (she'll be on Facebook within a week if she can't find another MuSK+ woman her age here).

---

## Dealbreaker scenarios (per patient)

| Patient | Where they'd close the tab today | Where they'd churn long-term |
|---|---|---|
| **Sarah** | Forced records upfront (averted by Path B). DNAvisit authorization screen without explanation. | First follow-up email that is chirpy or marketing-y. The "we miss you" pattern. |
| **Marcus** | Forced records (averted by Path B). The flow as-is on a second visit if he can't find a methods page or filter/sort. | Matches that don't survive cross-check against ClinicalTrials.gov. Discovery that DNAvisit has any security incident in their history. |
| **Dorothy** | HIPAA authorization page alone, without Linda — small text + legal language + needing to know which hospital systems hold her records. | First email containing the word "upgrade." First email showing options she's already tried and failed. First email that doesn't include realistic refractory options. |
| **Jasmine** | Already past the highest-friction moment via Path B. | First match email that's generic AChR content for a MuSK+ patient. Anonymous patient stories where opt-in real names would have made her share it. |

**The pattern:** Path A's existence saved all four sessions. The next pivotal moment is the first follow-up email. Three of four patients said the email determines whether they ever come back.

---

## Prioritized recommendations

### P0 — Block the next round (universal pain, fundamental to the value proposition)

**1. Introduce DNAvisit properly.** Add a micro-onboarding moment that names them with: who they are (one sentence), what they do (one sentence), why Resonata uses them, where data lives, retention policy. Place this **before** the consent screen so patients meet the partner before they're asked to authorize them. Currently it appears as a footnote inside checkbox #2 of a legal screen.

**2. Make matches actually dynamic to quiz answers.** Marcus's bug (ocular profile showing generalized) and Jasmine's MuSK+ gap are the same underlying issue. Whether this is a prototype limitation or a content-library limitation, the production version cannot ship without this. The "Not a fit" bucket only earns trust if the matches in the other buckets are actually filtered correctly.

**3. Build doctor-coordination tools.** Share-with-doctor / print-for-appointment view as a first-class feature on the preview screen and on care option details. Sarah's biggest gap; Dorothy implicitly needs it; Marcus wants a specialist finder; Jasmine wants something shareable. Probable smallest viable form: a "print this page" affordance + a "send to email" affordance that produces a clean appointment-ready summary.

**4. Add treatment-burden / side-effect info to match cards.** Three patients explicitly want this BEFORE deciding to connect records. Currently it's gated, but it shouldn't be — the data exists in trial protocols and FDA labels. Showing the cost of a match (time, side effects, infusion frequency, travel) is what makes patients trust the upgrade ask.

### P1 — Should ship before the pivot launches to all patients

**5. Caregiver mode.** Dorothy's biggest catch but applies to a large segment of the population (older patients, refractory patients, anyone in crisis). Spec: account-with-designated-caregiver, caregiver sees same view, both can act on the account. HIPAA implications need legal review, but the concept is well-established in patient-portal design.

**6. A phone number on the marketing surface.** Dorothy specifically named this. For a serious healthtech handling ePHI, the absence of a human touchpoint is a credibility issue, not a contact-volume issue. Even an answering-service number that returns calls in 24 hours would close this gap.

**7. Algorithm transparency / methods page.** Marcus's biggest catch. A methods page that explains: inputs (current 3 quiz answers + future fields), what's computed approximately vs. precisely, why records improve the match, and a source-attribution commitment ("every match cites its FDA label or ClinicalTrials.gov NCT number"). This is the trust scaffolding under "precision matching."

**8. Filter / sort / compare on the matches list.** Marcus's biggest functional gap; Sarah wanted compare-two-options; Jasmine wanted oral-vs-infusion. Minimum viable set of facets: trial vs approved, phase, site distance, oral vs infusion, dosing frequency. Plus a "pin to compare" affordance that surfaces a 2-4 column comparison view.

### P2 — Brand surface / second-wave investments

**9. Visual identity earns its weight.** The snowflake-and-hero-photo direction we just adopted addresses three of four patients' visual criticisms; carry it through. Replace anonymized "Patient · 47, Midwest" stories with opt-in real-name patient ambassadors with photos. Invest in a typographic point of view that's neither generic-green-healthcare nor cute-wellness.

**10. Cost / insurance transparency on match cards.** Especially for FDA-approved drugs. "Typically covered by Medicare / commercial insurance with prior auth" is the kind of one-liner that changes which match a patient pursues.

**11. Life-context quiz dimensions.** Jasmine's pregnancy / career / lifestyle gap. Could be an optional "tell us more about your life" step after the medical quiz, or a filter on the matches list. Different patients want different additional dimensions — Dorothy wants travel distance and treatment burden; Jasmine wants fertility and oral-vs-infusion.

### P3 — Real but second-order

12. Community surface (Jasmine: "find me one other MuSK+ woman in her 20s")
13. Data export + deletion-right commitments visible in chrome (Marcus)
14. Voice / audio option for content (Dorothy)
15. Larger-text accessibility option (Dorothy, and Sarah on bad-eyelid days)
16. Source citations on every match — NCT numbers, FDA label links (Marcus)
17. "What would change" preview before records consent — show specifically what the precision upgrade unlocks (Jasmine)

---

## Convergent themes worth elevating

**1. Trust is earned in deposits, not given in bulk.** All four patients walked through a sequence of micro-trust-decisions — give email, answer quiz, complete sign-up, choose Path A vs B, authorize records. The pivot's genius is splitting this into smaller decisions; the remaining gaps are wherever the product takes trust without depositing first (DNAvisit appearing unannounced, "we saved this for you" presumptions, blurred match cards that feel like withholding). Audit every "we [verb]" statement in the product against an explicit consent or trust deposit.

**2. The "Not a fit" bucket is brand-level differentiating.** Every other patient-matching product in this category shows what fits. Showing what doesn't fit is a transparency move that earned trust faster than any marketing copy could. This pattern should be more central to brand positioning — "We tell you what isn't for you" is closer to Resonata's actual identity than "Your personalized path to better care."

**3. The first follow-up email is the second pivotal moment.** The prototype gets patients past the consent split. But three of four patients explicitly said their decision to come back hinges on what arrives in their inbox next. If it's spammy, chirpy, contains the word "upgrade," or shows options they've already tried — they churn. The email experience is downstream of this prototype but determines its conversion. Treat it as part of the same design problem.

**4. The semi-converted state is genuinely valuable, not a consolation prize.** All four patients respect Path B and would actually use the semi-converted app surface (persistent matches, browse, bookmark, soft upgrade prompts). The "Records later" path should not be apologized for or designed as a downgrade. It is its own real product. The current prototype mostly gets this right; protect it from internal pressure to make Path A look more attractive at Path B's expense.

**5. Different patients require different content depths, not different products.** Sarah wants doctor coordination; Marcus wants methodology; Dorothy wants simplicity and human contact; Jasmine wants lifestyle filters and community. These aren't four products — they're four lenses on the same matches. The architecture (single matching engine + multiple surfaces) is right. The investment is in surfacing the right information for each patient at the right moment.

---

## Open questions for further research

1. **DNAvisit introduction moment** — where does this best live? Pre-consent screen? Inline in the consent? A separate FAQ surface? Each option trades trust against flow speed.

2. **Caregiver model** — how do we model caregiver-patient relationships without violating HIPAA, without doubling the onboarding friction, and without confusing the matching engine about whose answers it's matching on?

3. **Algorithm transparency commitment** — what's the methodology surface we'd actually publish? Inputs (current + planned), match-scoring approach, confidence intervals, source-citation commitments. Feasibility check needed with engineering.

4. **Real patient stories program** — is there a path to an opt-in patient-ambassador program? Names, photos, real bios, real consent. Operational lift but potentially transformative for trust and shareability.

5. **First-email design** — three of four patients said the inaugural email determines whether they come back. This needs its own design pass with the same rigor as the onboarding flow. Specifically: when does it send? What's in it for a semi-converted patient vs. a fully converted patient? How do we re-engage Dorothy in language she trusts (not "upgrade") and engage Jasmine in language she'd share (not "Dear patient")?

6. **MuSK+ content investment** — separate from the prototype, this is a content/library decision. Are there enough MuSK-specific trials and approved therapies in our database to make a credible Full-matches bucket for a MuSK+ patient? If not, what's the timeline?

7. **"Free for patients" — show or hide the business model?** Sarah's concern: hiding who pays makes me suspicious. Marcus's concern: same. Worth testing whether a transparent "trial sponsors and biopharma companies pay for matched patient introductions; we never share your data with them" line on the landing page increases or decreases conversion. Hypothesis: it increases trust without decreasing conversion.

---

## What this synthesis is not

This is synthetic patient feedback, not real user research. The four personas covered four important patient archetypes but did not surface:
- Real abandonment patterns (we can only infer where patients said they'd quit)
- Email open / click behavior
- The actual completion rates for each quiz question
- How patients react to the prototype on mobile devices (we can only simulate)
- The reactions of MG patients we don't have personas for (caregivers, pediatric MG family decision-makers, veterans, low-English-proficiency patients)

Use this to harden the prototype before putting it in front of real patients, not instead of it. The P0 recommendations should be applied; the prototype should then be tested with 5-8 real MG patients across the same archetype dimensions before launch.

---

*Individual reviews:*
- [Sarah Chen review](../analyses/sarah-chen-onboard-v2-review-2026-05-13.md)
- [Marcus Williams review](../analyses/marcus-williams-onboard-v2-review-2026-05-13.md)
- [Dorothy Martinez review](../analyses/dorothy-martinez-onboard-v2-review-2026-05-13.md)
- [Jasmine Patel review](../analyses/jasmine-patel-onboard-v2-review-2026-05-13.md)
