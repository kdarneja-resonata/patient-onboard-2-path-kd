# Patient App Pivot Sign-Off — Meeting Notes

**Date:** 2026-05-15 (Friday)
**Duration:** 1h13m
**Attendees:** Lauran, Martin, Vishal, KD (joined ~39:30)
**Invited but absent:** Megan, Rajni, Kristen
**Source transcript:** Gemini auto-notes (file id `1XjREdfTkazVi0Agg8PlaY6_PUD6bnEtJnpTA9FwfcJQ`)
**Scope:** Sign-off on the new onboarding (v2 / "v3"), prototype `patient-onboarding-v2-prototype.html`, PRD `prd-patient-onboarding-v3-2026-05-13.md`.
**Note:** Gemini transcribes "Vishal" as "Michelle" throughout. Same person.

---

## TL;DR

Pivot direction is signed off. Ship the current structured flow with a long list of fixes; Vishal's chat-first concept is "directionally correct" for the next iteration, not this one. Two reversals from the v3 PRD: "Not a fit" comes out of the preview (Pfizer-liability + competitive-intel), and snowflake is shelved as a brand element. HIPAA posture, single-signature consolidation, and scroll-to-bottom requirements all park behind a Kate Black consult that Martin will set up.

---

## Decisions

1. **Ship the current structured flow as v.next.** Chat-first onboarding is the iteration after.
2. **Remove "Not a fit" from the preview matches.** Drives competitive-intel exposure (negative + positive = full library) and Pfizer-class liability. Synthetic-patient signal that it was a differentiator is overruled.
3. **Remove the mismatch / count bar at the top of matches.** Falls out of #2.
4. **Distance is calculated, not filtered.** Show "57 mi · 32 mi · 4 mi" on cards. No 100-mile cutoff.
5. **List ordering will not be influenced by paid sponsors.** Logos/borders allowed as window dressing. Default ordering TBD (alphabetical by sponsor name, or approved-vs-trial, candidates).
6. **Architectural rule: same UX pre- and post-records.** Only the data feeding the algorithm differs. Incomplete-record state surfaces as an indicator (border or top band) with a path to complete. Lauran reserved the right to suppress features on first-encounter screens for new-user information management; Martin accepted.
7. **Quiz Q3 is broken.** Regenerate by giving Claude the 22-care-option matrix + eligibility criteria, then Megan sanity-checks for what patients can realistically answer.
8. **"I don't know" is the first option on every quiz question.** Currently miscoded.
9. **Data minimization on the save-and-come-back path.** "Only take what you need." First name + email at minimum. DOB likely replaced with "I'm over 18" (or 13, FTC). Lauran owns the final field list.
10. **Reorder signup screens.** "Connect records now" leads. Basic name/DOB only appears on the save-for-later path.
11. **Single signature for both consents.** Scroll-to-bottom on both documents, check both boxes, sign once. Validate legality with Kate.
12. **Scroll-to-bottom-before-sign required on consent documents.** Verify whether already implemented; build if missing.
13. **HIPAA consent + medical record authorization language explicit under the Connect Records button.** Per KD.
14. **Copy reframe: signup gate gives users "full access" to Resonata, not just "more precise matches."** Lauran landed on "full access" framing.
15. **Snowflake shelved as the brand element.** Possibly an ad concept later. Logo decision in the PRD is reversed.
16. **Side-effect / treatment-burden data layer deferred.** Feature creep for this version even though package inserts are available.
17. **Versioning convention: decimals.** No more integer bumps.
18. **Path A "share with doctor" PDF is functional.** Confirmed live, "gorgeous." Treat as shipped, not deferred.
19. **Patient-supplied data front end is built.** Back end remains. Not committed to this release but closer than Martin assumed ("months away").
20. **Library walker is a future top-level path,** not this version. Hosts the "see all care options with mismatch reasons" experience and user overrides on criteria.

---

## Prototype change list

References below use the screen IDs from [`patient-onboarding-v2-prototype.html:735-756`](../../patient-onboarding-v2-prototype.html). P0 = must be in the next iteration. P1 = before broader launch. P2 = nice to have.

### Screen 1 — Landing (`landing`)

| # | Priority | Change |
|---|---|---|
| 1 | P0 | Replace snowflake mark with neutral wordmark or temporary placeholder. Snowflake is shelved as brand element ([`patient-onboarding-v2-prototype.html:1436-1441`](../../patient-onboarding-v2-prototype.html)). |
| 2 | P0 | Condition dropdown content must reflect production behavior: all supported conditions, "coming soon" tagged conditions, and an "Other" free-text option. |
| 3 | P0 | Unsupported-condition handler copy: "This isn't in our matching yet. We're adding conditions all the time. We'll email you as soon as there's news." |
| 4 | P1 | Tagline "Precision care options for **you**" stays. Verify with Megan whether "you" emphasis still reads as intended once snowflake is gone. |

### Screens 3–6 — Quiz (`quiz-1` through `quiz-4`)

| # | Priority | Change |
|---|---|---|
| 5 | P0 | Replace or remove **Q3** ("Treatment stage"). Was carry-over from sample-patient flow; not a useful funnel-halving question for current 22-option matrix. Source replacement by running Claude over the care-option matrix + eligibility criteria, then Megan sanity-check. |
| 6 | P0 | Move "I don't know / not sure" to the **top** of the option list on every quiz question, not buried at the bottom ([`patient-onboarding-v2-prototype.html:897-1108`](../../patient-onboarding-v2-prototype.html) — verify each quiz block). |
| 7 | P1 | After each answer, consider showing a running counter: "Based on this answer, X options remain." Vishal's idea, lifted from his chat-first proposal; small enough to bring forward without doing the full chat redesign. |

### Screen 7 — Preview matches (`preview`)

| # | Priority | Change |
|---|---|---|
| 8 | P0 | **Remove "Not a fit" bucket entirely.** Drop the third chart row ([`patient-onboarding-v2-prototype.html:1158-1168`](../../patient-onboarding-v2-prototype.html)) and all `data-bucket="not"` cards. |
| 9 | P0 | **Remove the mismatch / count bar at the top of matches.** Falls out of #8. The chart-as-filter pattern can stay (Full / Maybe only) or simplify further; Lauran's call. |
| 10 | P0 | Card meta: replace any 100-mile-radius framing with a literal distance from home ("57 mi", "32 mi"). Update `option-meta` lines ([`patient-onboarding-v2-prototype.html:1194,1211,1228`](../../patient-onboarding-v2-prototype.html) etc.). |
| 11 | P0 | Trial cards: ensure bookmark icon is **always pinned top corner**, never sharing position with Apply ([`patient-onboarding-v2-prototype.html:1183+`](../../patient-onboarding-v2-prototype.html) card structure). Apply is a CTA; bookmark is a persistent affordance. |
| 12 | P1 | Bookmark behavior on preview: deactivated/greyed-out at this stage with prompt "Save your spot to bookmark." Martin's framing: bookmarking is a lower bar than chat, so it's a reasonable carrot for email capture. Pair with KD's compliance language. |
| 13 | P1 | Replace traffic-light coloring on Full / Maybe categories. Color-blindness risk (Megan's existing styling, not in the prototype yet but flagged). |
| 14 | P0 | Verify trial-site distance sorting in the live product. Martin says Avan built this 4–6 months ago and it works in production; if it's not in the prototype card detail it's a regression. Lauran to confirm with Akash. |
| 15 | P0 | Update the save banner copy ([`patient-onboarding-v2-prototype.html:1415`](../../patient-onboarding-v2-prototype.html)) to match the new account model decided on this call (no longer a "send yourself a link, no signup needed" pitch unless Kate confirms minimal-state HIPAA posture; see Pending Kate). |
| 16 | P0 | CTA copy on `:1423`: change "Ready to make these matches precise?" to "Ready to open up Resonata?" / "Get full access." Lauran's framing is "full access." Megan to review final. |

### Screen 8 — Signup (`signup`)

| # | Priority | Change |
|---|---|---|
| 17 | P0 | **Reorder screens.** Lead with "Connect records now" (currently `consent-split` `:1489`). Make the basic-fields screen the lead-in only for the save-for-later branch. |
| 18 | P0 | Apply data minimization to the save-for-later fields ([`patient-onboarding-v2-prototype.html:1450-1477`](../../patient-onboarding-v2-prototype.html)): keep first name + email; drop last name; replace DOB with an over-18 (or over-13) attestation; state may stay if jurisdictional rules require it, else optional. Lauran owns the final list. |
| 19 | P0 | Fix the A/B path layout on `consent-split` `:1489-1553`. Currently reads as a recommended-vs-secondary toggle; Lauran flagged the layout as confusing. The "Connect records now" path is now the lead; the secondary save-for-later card should be visually subordinate but still clearly available. |
| 20 | P0 | Under the "Connect records now" button: spell out the two consents as **HIPAA authorization** and **medical record authorization** (currently generic "Two consents to sign" on `:1527`). Per KD. |
| 21 | P0 | Update `:1517` "Get verified matches in about 8 hours" and the bullets `:1518-1524` against the new "full access" framing. Bullets currently emphasize precision; add the access/functionality framing Martin asked for. |
| 22 | P0 | Bullet `:1521` references the comprehensive doctor report. Confirmed live and "gorgeous" per Martin. Wording can stay; cross-link to whichever version Akash has live. |

### Screens 9 — Path A Authorizations (`path-a-auth`)

| # | Priority | Change |
|---|---|---|
| 23 | P0 | Consolidate to **single signature** flow: present both documents, require scroll-to-bottom on each, check both boxes, sign once. Validate with Kate before build (see Pending Kate). |
| 24 | P0 | Enforce scroll-to-bottom-before-sign. Sign button disabled until scrolled. Verify with Akash whether this exists in production; build if not. |
| 25 | P1 | DNAvisit explainer page: still an open issue from [`prd-issues-running.md`](../../prd-issues-running.md). Not changed by this meeting but reaffirmed as a launch dependency. |

### Screen 11a — Path A confirmation (`path-a-confirm`)

| # | Priority | Change |
|---|---|---|
| 26 | P0 | Rewrite "You're set. We're getting your records." headline ([`patient-onboarding-v2-prototype.html:1630`](../../patient-onboarding-v2-prototype.html)). Martin: "You're not set. You're still sick. We didn't actually solve your problems… it's like the waiter taking your drink order and saying 'I hope you liked your meal.'" Megan to draft replacement. |

### Screen 13 — App home (semi-converted) (`app-home`)

| # | Priority | Change |
|---|---|---|
| 27 | P0 | Update the "Connect records to make them precise" link copy ([`patient-onboarding-v2-prototype.html:1837`](../../patient-onboarding-v2-prototype.html)) to the new "full access" framing. |
| 28 | P1 | Add the incomplete-medical-record indicator pattern (border or top band) for users in the save-for-later state. Architectural ground rule: same UX as logged-in-with-records, just with the indicator + a path to complete. Lauran retains UX-suppression rights on first-encounter screens. |

### Screen 14 — Care option detail (`app-detail`)

| # | Priority | Change |
|---|---|---|
| 29 | P1 | Remove or rephrase any "Why this is ruled out" language on individual cards ([`patient-onboarding-v2-prototype.html:1369`](../../patient-onboarding-v2-prototype.html) and similar). If retained for the future library walker, swap to "Mismatch reason" (Martin's preferred framing: "You're not saying this isn't good for you. You're saying this is why we don't see it as a match for you."). For the current preview/app surface, the line goes away entirely once "Not a fit" is removed. |

### Cross-cutting (all screens)

| # | Priority | Change |
|---|---|---|
| 30 | P0 | Strip snowflake mark + tagline tagline tagline (`:1437`, `:1440`, etc.) everywhere it appears as the brand element. Replace with neutral wordmark short-term. |
| 31 | P0 | Patient-vocabulary dictionary pass with Megan on top-level card density. Existing agent + Megan feedback already flagged this. |
| 32 | P1 | UI regression spotted live on the staging website ("that wasn't there yesterday"). Saba reportedly deployed via Maya. Investigate before next demo. |
| 33 | P1 | Condition-specific copy on website needs modification (Lauran flagged mid-demo; specifics not captured). |

---

## PRD change list

References below use [`prd-patient-onboarding-v3-2026-05-13.md`](../../prd-patient-onboarding-v3-2026-05-13.md) line ranges.

### Reversals (existing language is now wrong)

| # | Section | Current | Change |
|---|---|---|---|
| P1 | `:130-141` "Decisions captured during prototype iteration" | `"Not a fit" bucket as a brand-level differentiator (kept visible and explained, not hidden).` | Replace with: `"Not a fit" is removed from the preview match surface. Reasons: (a) Pfizer-class liability if we appear to disqualify a sponsor's treatment, (b) competitive intelligence — negative set + positive set = full library, exposing our catalog. Synthetic-patient signal was that "Not a fit" reads as a differentiator; this is acknowledged but overruled.` |
| P2 | `:130-141` "Decisions captured during prototype iteration" | `Logo: botanical snowflake (uniqueness metaphor — no two snowflakes alike, ties to "for you" framing).` | Remove. Snowflake is shelved as a brand element. Possibly an ad concept later. |
| P3 | `:139` | `Tagline: "Precision care options for **you**."` | Stays for now but flag for Megan + brand pass once logo decision is settled. |
| P4 | `:154` Stakeholders | `Legal | Henry | HIPAA posture on magic-link, semi-converted state` | Replace with: `Legal | Kate Black (pro bono, via Devon McGraw) | HIPAA posture on minimum onboarding, document signing, single-signature consolidation.` Keep Henry as a secondary if still in the loop. |
| P5 | `:6` Target launch | `TBD` | No change requested but consider populating once the Kate consult lands. |
| P6 | `:38` Non-goals | `Side-effect / treatment-burden data layer (on roadmap — see issue #3).` | No change. Confirmed deferred for this version. |
| P7 | `:6` Status | `Draft v1` | Bump to `Draft v1.1` (decimal convention). Date stamp 2026-05-18. |

### Additions to "Hard requirements (V1)" (`:75-84`)

Add the following bullets:

| # | New requirement |
|---|---|
| P8 | **Same UX pre- and post-records.** Once a user has supplied an email, the app surface is identical regardless of medical record status. The only difference is the data feeding the algorithm. Incomplete-record state is signaled with a non-blocking indicator (border, top band) and a path to complete. UX execution may suppress features on first-encounter screens for new-user information management, but feature availability does not change post-records. |
| P9 | **Data minimization on the save-for-later path.** Collect only what is operationally required. First name + email at minimum; DOB replaced with age attestation (over-18 or over-13 per FTC); state only if jurisdictional rules require it. |
| P10 | **Sponsor neutrality in list ordering.** Paid sponsors do not influence match ordering. Window dressing (sponsor logos on cards, card borders) is permitted; ranking is not. Default ordering is alphabetical by sponsor name or grouped approved-vs-trial; final default TBD. |
| P11 | **Trial distance is presented, not filtered.** Cards display the distance from the user's home (e.g., "57 mi"). No mileage cutoff filters the list. |
| P12 | **Single-signature consolidation for consents** (subject to Kate validation). Both documents scrollable to the bottom, both boxes checkable, one signature event. |
| P13 | **Scroll-to-bottom-before-sign enforcement** on every consent document. Sign action disabled until scroll completes. |
| P14 | **HIPAA consent + medical record authorization language explicit under the Connect Records call to action.** Generic "two consents to sign" copy is not sufficient. |

### Changes to Flow (`:49-72`)

| # | Section | Change |
|---|---|---|
| P15 | `:53-58` Quiz | Replace Q3 ("Treatment stage") with a new question sourced from the 22-care-option matrix. Methodology: feed care options + eligibility criteria to Claude, ask for three questions that move users halfway down the funnel, then have Megan sanity-check for patient comprehension. Document this methodology as the source for any future quiz revisions. Specify: "I don't know" is the first option on every question. |
| P16 | `:59` Preview matches | Remove the "Full / Maybe / Not a fit" three-bar viz language. Replace with two categories (Full / Maybe) and no Not-a-fit bucket. Update the data viz description accordingly. Adjust the chart-as-filter narrative — it still exists, but filters two buckets only. |
| P17 | `:60` Sign-up | Update to reflect data minimization on save-for-later. The full-account branch keeps its richer collection. |
| P18 | `:61-62` Consent split | Reframe so the Connect-records-now path is the lead, not a peer-presented option. Save-for-later remains available but visually subordinate. Update language from "Path A vs Path B equal options" to a primary-with-fallback framing. |
| P19 | `:62-63` Path A | Add the single-signature + scroll-to-bottom requirements. Add the explicit HIPAA + record-auth callouts under the button. |

### Changes to Phasing (`:117-126`)

| # | Section | Change |
|---|---|---|
| P20 | `:121` V2 | Add: "Vishal's chat-first onboarding pattern. After each quiz answer, show running options-remaining count. Chat surface as the gate for email capture. Profile builds incrementally as the user clicks and chats." |
| P21 | `:121` V2 | Add: "Library walker — top-level path that walks users through the full care-options catalog with mismatch reasons per option and a way to override criteria. Patient-supplied data propagates across options." |
| P22 | `:123-124` V3 | Move "Doctor-share / print-for-appointment" out of V3 and into V1 confirmed-shipped status. The PDF is functional per Martin. |

### Changes to "Top open issues" (`:109-114`)

| # | Issue | Change |
|---|---|---|
| P23 | Issue 1: Magic-link escape vs. minimal sign-up | Re-scope around Kate Black's input on whether minimum-state persistence requires HIPAA authorization. Martin's view: it may not. Pending decision. |
| P24 | Issue 2: DNAvisit | No change. Still open. |
| P25 | Issue 3: Match logic dynamism | No change. Still open. |

### New PRD section: Out of scope (mentioned, deferred)

Append to the existing out-of-scope list in [`prd-issues-running.md`](../../prd-issues-running.md):

- **Library walker** (deferred to V2+). Future home for full-catalog browse + mismatch reasons + criteria overrides.
- **Patient-supplied data back end** (front end built, back end remaining; not committed to this release).
- **Vishal's chat-first onboarding** (directionally correct, next iteration).
- **Side-effect / treatment burden** (feature creep for this version; package inserts available on server, can be Claude-processed later).
- **Snowflake brand element** (shelved; possible ad concept later).

### New PRD section: Principles to call out

Add a "Principles" or "Architectural rules" section before Phasing. Capture:

1. Records are an upgrade, not a gate.
2. Same UX pre- and post-records.
3. Data minimization ("only take what you need").
4. Sponsor neutrality in ordering.
5. Distance is calculated, not filtered.
6. No presumptive consent.

---

## Action items

| # | Owner | Action | Blocking? |
|---|---|---|---|
| A1 | Martin | Introduce Lauran and KD to Kate Black | Blocks 8 below items |
| A2 | Lauran | Replace Quiz Q3 using care-option-matrix methodology | Blocks ship |
| A3 | Megan | Sanity-check new Q3 question for patient comprehension | Blocks ship |
| A4 | Lauran | Patient vocabulary dictionary pass on top-level cards | Blocks ship |
| A5 | Lauran + Akash | Verify trial-site distance sorting in live product; treat as regression if missing | |
| A6 | Lauran + Akash | Verify scroll-to-bottom-before-sign on consents; build if not present | Blocks ship |
| A7 | Lauran | Strip "Not a fit" + mismatch bar from preview; refactor chart-as-filter to two buckets | Blocks ship |
| A8 | Lauran | Update preview card meta to show calculated distance per card | Blocks ship |
| A9 | Lauran | Reorder signup screens (Connect-records leads, basic name/DOB only for save-for-later) | Blocks ship |
| A10 | Lauran | Apply data-minimization to save-for-later fields | Blocks ship |
| A11 | Lauran | Strip snowflake brand element from prototype; placeholder wordmark | Blocks ship |
| A12 | Lauran | Update CTA copy: "full access" / "open up Resonata" | Blocks ship |
| A13 | Lauran + Megan | Rewrite "You're set" headline on Path A confirmation | Blocks ship |
| A14 | Lauran | Spell out HIPAA + medical record authorization under Connect Records button | Blocks ship |
| A15 | Lauran | Fix A/B path layout on consent-split screen | Blocks ship |
| A16 | Lauran | PRD edits per change list above; bump to Draft v1.1 | |
| A17 | Lauran | Investigate UI regression on staging website (Saba/Maya deploy) | |
| A18 | Vishal + team | Land GitHub adoption to unblock v1.1 production release with auto-matching | |
| A19 | Lauran | Patient-supplied data: confirm back-end remaining scope with Akash | |
| A20 | Lauran | Roadmap addition: website facelift with human imagery (use Martin's iStock; avoid pharma-stereotype shots) | |

No explicit due dates in the meeting. Lauran's stated intent: "get this next iteration out fast. We can change it every day if we want."

---

## Pending Kate Black consult

These items move only after the consult Martin is setting up. Block them out of the next iteration if Kate doesn't land in time, or build with placeholders and ship the final state after.

1. **HIPAA authorization for the save-for-later minimum.** Can we hold first name + email + quiz answers + matches without a HIPAA release? Martin's prior: probably yes. Henry's prior: probably no. Kate is the tiebreaker.
2. **Single-signature consolidation legality.** One signature covering both HIPAA + record-auth documents after scroll + checkbox confirmation, vs. the current two-signature flow.
3. **Scroll-to-bottom enforcement standard.** Required for pharma diligence? If yes, the bar is "user must reach end of doc before sign enabled."

---

## Open questions / disagreements

1. **List ordering default** when a user has many matches. Lauran flagged "what do I do next?" as unsolved. Alphabetical by sponsor name, approved-vs-trial grouping, or some other heuristic. No final call. Sponsor influence is explicitly off the table.
2. **Pre-records bookmark functionality.** Martin wants parity (deactivated bookmark icon with "save your spot to bookmark" prompt). Lauran wants UX-suppression rights on first-encounter screens. Resolved at principle level (parity applies post-email-capture); execution discretion remains with Lauran.
3. **Traffic-light coloring on match categories.** Color-blindness risk on Megan's styling. No resolution. Lauran to brief Megan.
4. **Exact final field list on save-for-later signup.** Martin gave the principle ("only take what you need") and explicitly declined to dictate. Lauran owns.
5. **MuSK+ content depth.** Not raised in this meeting but still in [`prd-issues-running.md`](../../prd-issues-running.md#8-musk-content-investment) as an open issue.

---

## Quotes worth preserving

> **Martin (on legal risk of "Not a fit"):** "If you disqualify a treatment for somebody — let's say it's a Pfizer treatment — you could have Pfizer sue us to say you are prematurely disqualifying users from our product and that causes us damages. Positive proposals of treatments is only good for them. Negative proposals of treatments is potentially a liability for us. Pfizer has deeper pockets than we do."

> **Martin (on preview screen purpose):** "You only need the full and maybe at this level because the purpose of this level is to deliver value, which encourages the user to go deeper. You've accomplished that with just the first two."

> **Martin (on distance):** "It's not filtering, it's calculating. 100 miles is arbitrary. People won't give a sh*t if it's 101 miles."

> **Martin (on quiz methodology):** "Here's our 22 care options with their eligibility criteria. Tell Claude: give me three questions that would enable us to move patients at least halfway down the funnel. Then sanity check them with Megan because she'll know what patients are likely to know. Claude might propose questions that are the highest yield but nobody would be able to answer them."

> **Martin (on preview-vs-full UX parity):** "There are no display UX differences between preview mode and full mode. The only difference is the data that has gone into the algorithm is more limited."

> **Martin (on HIPAA over-engineering):** "Just be aware of imaginary burdens. You may not need some sort of HIPAA release from a patient to hold this level of information."

> **Martin (on Kate):** "Kate Black is our privacy counsel. She came to us by way of Devon McGraw, who basically is the author of HIPAA and first enforcer of HIPAA years ago. They're forward thinkers who look at this from the lens of how do you do it instead of what do you not do."

> **Martin (on data minimization):** "Only take what you need."

> **Lauran (on sponsor ordering):** "I can imagine a sponsor saying 'well if I'm paying you mine goes at the top.'"
> **Martin:** "And just for what it's worth, we're not going to do that."
> **Lauran:** "That's what I fully expected you to say and I think it's the right decision."

> **Martin (on "You're set"):** "You're not set. You're still sick. We didn't actually solve your problems. It's saying that we accomplished more than we did. It's like the waiter taking your drink order and saying 'I hope you liked your meal.'"

> **Martin (on stock imagery):** "I have a very passionate dislike for pharma-like imagery. The happy grandpa dancing on the beach. No. It's just so New Jersey."

> **Lauran (on first-encounter UX):** "Adding things to an app the first time someone sees it does not always have the effect that you think it will have. We have a real information management situation with this type of an app. This needs to feel like a train on tracks when I first get started, not 'here's five paths, now pick one.'"

> **Lauran (calling convergence):** "What I'm trying to do is get this next iteration with this forked signup capability out fast. We can change it every day if we want. Are we on the same page that that's the next thing, to get it out?"
> **Vishal:** "Lauran, this is directionally correct. We can go ahead with this basically for the initial launch."

> **Martin (on GitHub unlock):** "It also means v1.1 can finally get released into production. So I don't have to match the patients. They just get matched."

---

## Timeline / capacity check

No explicit dates committed. Lauran's stated tempo is "fast, iterate daily." A few capacity flags worth tracking:

- The change list above is large. Most items are mechanical prototype edits, but Q3 replacement (A2 + A3) and the Kate consult (A1 → 8 dependent items) are blockers with external dependencies.
- The Kate consult timeline is unknown. If it doesn't happen in week 1, the single-signature consolidation and the minimum-onboarding HIPAA posture will hang.
- GitHub adoption (A18) is on Vishal's side. Not blocking this iteration but is the unlock for v1.1 production.

---

## Next steps

1. Lauran spins through the Prototype change list (A2 – A17). Sequence: mechanical edits first, then the layout reorder, then the consent flow rewrites pending Kate.
2. Lauran PRD edits (A16). Bump to Draft v1.1.
3. Martin sends Kate intro (A1).
4. Lauran + Megan schedule the patient-vocabulary pass + Q3 sanity-check + copy review.
5. Next sign-off / demo: TBD. Suggest re-syncing once the P0 prototype edits land + Kate consult is on the calendar.

---

*Saved 2026-05-18. Source: Gemini auto-notes from Patient App Pivot Sign-Off meeting on 2026-05-15.*
