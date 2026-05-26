# Legal sign-off items — patient onboarding v2

**Meeting:** Kate, 12:30 today (2026-05-19)
**Purpose:** Get directional sign-off (or flag blockers) on legal/HIPAA questions in the new patient onboarding flow before we build it.

---

## Context in 3 lines

We're rewriting patient onboarding to fix a 92% drop-off at the records-consent step. New flow: 3-question quiz → preview matches → sign-up → patient picks Path A (connect records now) or Path B (browse with HIPAA-only consent, no records pulled). Path B is the new thing legally — patients we hold data on without records access.

Working PRD: `prd-patient-onboarding-v3-2026-05-13.md`. Prototype available if you want to see screens.

---

## Highest priority — need a directional answer today

### 1. Quiz answers collected before HIPAA consent

Patient enters MG type, antibody status, severity, and (optional) zip in the quiz BEFORE seeing HIPAA. They've only given us email + condition at that point.

**Question for you:** Are quiz answers PHI? If yes, what disclosure do we need at the quiz entry? If we treat them as "interest data" (pre-identification), where's the line?

### 2. Path B — "semi-converted" state

Patient signs up (name, state, DOB), gives HIPAA-only consent (no records retrieval), browses persistent matches across sessions. We hold name + DOB + state + condition + quiz answers + matches indefinitely. We send magic-link emails ("new option fits your profile") periodically.

**Questions for you:**
- Is HIPAA authorization without records retrieval a legally clean state to hold a patient in?
- Retention policy if patient never upgrades to Path A? (1 year? 3? Indefinite with annual re-consent?)
- Anything in the consent language that needs to distinguish Path B from Path A explicitly?

### 3. Magic-link authentication

Patient returns to their matches via emailed magic link (no password). Same mechanism whether they're Path A or Path B.

**Questions for you:**
- Acceptable under HIPAA for a patient accessing their own data?
- Any additional verification needed on return (e.g., re-enter DOB)?
- Link expiration window?

### 4. Email capture at landing (before any consent)

Email is the first thing we ask for, before HIPAA, before quiz. It functions as a magic-link key, not a marketing list.

**Question for you:** Any disclosure required at the email field? Current copy says "Find care options for you" with no fine print at email entry.

---

## Medium priority — can punt to follow-up if needed

### 5. Email scope

Only 3 email types ever sent:
1. Magic link back to matches
2. "Your records are ready" (Path A only)
3. "A new option fits your profile"

No newsletters. No "we miss you." No marketing.

**Question:** Confirm this scope is treatable as transactional/treatment-related, not requiring marketing opt-in.

### 6. DNAvisit retrieval (Path A only)

DNAvisit pulls records on the patient's behalf. They're our records-retrieval vendor.

**Questions:**
- Is our consent language sufficient for DNAvisit to act as a records-retrieval agent and hand records to Resonata?
- Should DNAvisit have a separate explainer/consent page, or does our consent cover it?

### 7. Silent suppression of "not a fit" options

Internal system knows which care options don't fit a patient's profile. Patients only see Full match + Maybe match. The "not a fit" bucket is silently omitted from patient view.

**Question:** Any duty-to-disclose risk if a patient could theoretically have qualified for a trial we hid? (Our matching is conservative — we exclude based on documented eligibility, not preference.)

### 8. Uncovered-condition waitlist

If patient picks a condition we don't cover (everything except MG in V1), we capture their email and condition for a waitlist.

**Question:** Any HIPAA implications when we capture condition data but don't serve them? (They haven't given HIPAA consent yet at this point.)

---

## Low priority — nice to confirm but not blocking

### 9. DOB at sign-up

We collect DOB at sign-up for identity. Could we use year-of-birth only?

### 10. Sponsor-facing aggregates

For B2B reporting, we may share "did not match" counts (aggregated, no individual patients) with sponsors. Confirm this is fine as long as it's truly de-identified.

---

## What we need from you by EOD

If we can leave the meeting with a yes/no/needs-more-info on items 1–4, eng can keep moving. Items 5–10 can be follow-up.

If you need to loop in outside counsel on any of the Path B questions, flag that now so we can schedule.
