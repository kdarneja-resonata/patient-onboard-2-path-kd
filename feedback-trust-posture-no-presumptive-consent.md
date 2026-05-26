---
name: trust-posture-no-presumptive-consent
description: Never assert that Resonata took an action on the patient's behalf (saved, sent, scheduled, subscribed) before they have explicitly consented to that action. Audit every "we did X for you" statement against consent AND patient interest.
type: feedback
date_captured: 2026-05-13
captured_by: Claude (cowork session, patient onboarding v2 prototype)
target_memory_location: ~/Developer/resonata/context-library/_memory/
---

# Trust posture: never assert action without consent

## The rule

Before any UI element says "we [verb] this for you" — saved, sent, emailed, subscribed, scheduled, scheduled, retrieved, registered — both of these must be true:

1. **Explicit consent.** The patient took an action that clearly authorized this. "I gave you my email" is NOT consent to be emailed; it's consent for the email to be used as identifier. Authorization needs to be specific to the action.
2. **Action serves the patient.** Not just the funnel. Sending matches via email might "feel friendly" but it defeats the come-back-to-convert loop AND signals we're optimizing against ourselves.

If either fails, redesign as opt-in or as a magic-link-to-return pattern.

## Why this matters

**The triggering incident (2026-05-13):** Patient onboarding v2 prototype had a banner on the preview screen saying "We saved these matches to your inbox" before the patient had:
- Explicitly consented to receive matching emails
- Even completed sign-up (account creation came later in the flow)

Lauran caught it: "this design is playing fast and loose with trust." She was right. Two failures in one element:

1. **Consent failure.** The landing-page email field said "Your email" with help text noting we'd "save your matches and send updates." That's an announcement, not consent. A reasonable patient reads "we saved these to your inbox" and asks "wait, what did I sign up for?"

2. **Logic failure.** If we email patients their matches, the come-back motive is gone. The deck's actual model is the opposite: email is the **magic link** that brings them back to the matches *living on Resonata*. Email = key. Web = vault.

## Why this is especially load-bearing for Resonata

- **HIPAA context.** Resonata handles ePHI. Trust posture is non-negotiable.
- **Brand voice.** "We are healthcare people, not silicon-valley tech bros" (brand-voice-guide-resonata.md). Presumptive "we did X for you" copy is exactly the growth-hack pattern Resonata explicitly avoids.
- **Audience expectation.** Patients with serious chronic conditions are wary by default. They've been over-marketed-to by drug ads and over-promised by other healthtech. Any moment of presumption confirms their suspicion.

## How to apply

**During design review:**
- Read every UI string that contains "we [verb]" or "[verb]ed for you" or "your [thing] is [past-tense action]"
- For each: name the moment of consent that authorized it. If you can't, redesign.

**Patterns that work:**
- *Magic link pattern:* "Want to come back to these later? [Email me a link]" (opt-in, web stays the home of the data)
- *Conditional promise:* "When you finish sign-up, your matches will be saved here." (clear about what triggers the action)
- *Earned promise:* Move the "we saved this" banner to *after* the consent moment that authorizes saving.

**Patterns that fail:**
- Any past-tense "we did X" before the user took an action authorizing X
- Any "we'll email you when..." without a paired opt-in checkbox/toggle
- Any "we set up your..." that wasn't preceded by an explicit setup step

## Convergent evidence from the persona panel (same date)

All four synthetic patient personas independently flagged trust-posture issues in the same prototype. Pattern across reviews:

- **Marcus (34, technical):** Flagged DNAvisit appearing as an unintroduced third-party records partner with no privacy policy linked. Won't connect records until he can verify the partner.
- **Dorothy (68, refractory):** Flagged blurred match cards on the preview screen as feeling like "a magic trick" — withholding things she might need before deciding to share more.
- **Sarah (52, collaborative):** Same DNAvisit concern. Plus would not connect records before consulting her doctor — wants the option to share/print the matches for the appointment.
- **Jasmine (28, digital native):** Took Path B explicitly because trust wasn't established yet. Generic visual identity didn't help.

These aren't four separate issues — they're one pattern: **the prototype takes trust faster than it earns it.** The "saved to your inbox" banner is the most explicit instance, but it lives in a family.

## Standing question to revisit

When and how do we introduce DNAvisit? Right now they appear unannounced inside the records authorization screen. Consider:
- Naming them earlier in the flow (e.g., on the consent split screen) with a one-line credibility pitch
- Linking to their security/privacy posture inline
- Treating "third-party records partner" as a trust event that needs its own micro-onboarding moment, not a footnote in a consent form

---

*This file lives in the project folder pending lift to `~/Developer/resonata/context-library/_memory/` via Claude Code.*
