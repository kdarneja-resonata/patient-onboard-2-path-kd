# Marcus Williams — Patient Onboarding v2 Usability Review
**Date:** 2026-05-13
**Prototype:** patient-onboarding-v2-prototype.html (semi-conversion pivot)
**Synthetic User:** Marcus Williams (34, Austin TX, ocular MG 6mo, AChR+, high tech)

---

## 1. First impressions

Five seconds in. I'm looking at the landing page and doing what I always do — sizing up whether this is real or whether somebody who's never built a real product paid a contractor in Manila to slap a stock template on a Stripe-style hero.

Verdict: it's not embarrassing. The typography is Inter, the spacing has rhythm, the color palette is one decision (clinical green) instead of seven, and there's a HIPAA badge sitting above the H1 instead of buried in a footer disclaimer. That tells me somebody on the team has actually shipped consumer-grade software before, and that's already more than I can say for the last three patient portals I've had to log into.

But — and this is where I get suspicious — the headline reads "Your personalized path to better care." That is a phrase I have read on roughly forty SaaS landing pages this quarter. It is the linguistic equivalent of beige. It means nothing. The subhead recovers a little: "matched to treatments and clinical trials that fit your specific medical profile — not generic options for everyone with your condition." Okay, you've got a thesis. Now prove it.

The "How it works" sidebar on the right ("Tell us a little / See your matches / Go deeper when you're ready") is doing something I respect — it's pre-committing to a short path. As a guy who has been jerked through six-screen consent flows just to get a refill request through MyChart, the implicit promise "you will not have to fight us" matters.

So: cautiously not unimpressed. Bar is on the floor in this category. Resonata is at least standing.

## 2. Step-by-step walkthrough

**Screen 1 — Landing.** Email + condition dropdown. Friction is appropriately minimal. The microcopy under the email field — "We'll save your matches here and send updates only when something new fits your profile" — is a real promise that I'll be checking later (Marcus side note: I will absolutely sniff-test this with a burner address and see if I get retargeted on Facebook within a week). The condition dropdown lumps "Another neuromuscular condition," "An autoimmune condition," and "A cancer diagnosis" into the same flat list, which is fine for now but won't scale. I'd want a search/typeahead the second this hits twenty indications.

**Screen 2 — Indication check loading.** 1.8 seconds of spinner. Theatrical. There's no actual lookup happening here that needs that long — you have my condition string, you know in microseconds if you cover it. This is performative reassurance ("look how hard we're thinking about you") and that's exactly the kind of thing that erodes my trust in the data backbone. If the front-end fakes urgency, what else is faked? Either do the real lookup and show me the latency, or just transition. Don't pretend.

**Screens 3–5 — Quiz.** Three questions: MG type, antibody status, treatment stage. The progress bar is honest (25 / 50 / 75%). I like that "I'm not sure" / "I don't know" is a first-class option on every question and doesn't punish me. The microcopy on Q2 — "If you don't know, that's fine — we'll show broader matches" — is the right voice. The clinical detail in the choice descriptions ("about 85% of generalized MG") is appropriate for somebody like me but might be over-clinical for, say, my mom. Whatever — that's their segmentation problem, not mine. For me, it lands.

Where's the methodology hidden? Already, here. You're about to compute a match for me off three categorical variables. Three. I want to know what your model is doing with those three answers, what features you're holding out for the records pull, and what the confidence interval is on the "approximate match" you're about to show me. Nothing on this screen tells me.

**Screen 6 — Preview matches.** This is the make-or-break screen and it's the most interesting screen in the prototype. The "Approximate matches based on what you told us" banner is the right caveat in the right place — and I respect the design decision to put "Not a fit" as its own bucket. Showing me what you ruled OUT is a real trust signal. That's better than 90% of patient-facing products which just show you the green checks.

But — three things bother me immediately.

First: "4 strong matches, 6 potential matches, 3 options we know aren't a fit." That's 13 total. Why these 13? Where's the denominator? Are these the only 13 things you have in your database for generalized AChR+ MG? Or did your algorithm pick 13 out of 80? I have no idea, and that single number gap is the difference between "curated by experts" and "this is everything we have."

Second: no filter, no sort, no facets. I can't say "show me trials only," "show me oral therapies only," "show me Phase 3 only," "exclude placebo-controlled," "exclude weekly infusions." I'm looking at four cards in the Full bucket and I can already see I want to dimension them against each other. There's a "Why we matched you" line on each card but I can't compare them side by side. Where is the spreadsheet view? I will literally export this to my own spreadsheet if you don't give me one.

Third: the "Matches: generalized MG · AChR-positive · on treatment looking for better options" line under each option. That's the same line for every card in the Full bucket. If everything matches on the same three criteria, what's actually differentiating their rank? Is this an alphabetical list pretending to be a ranking? Show me the score.

**Screen 7 — Complete sign-up.** Name, state, DOB. Fine. Slim. No password requirement, which I assume means magic link, which I'd prefer over yet another password. Note: as somebody who has had to give my DOB to 400 healthcare entities this year, the field-help "Required by some trials and treatment eligibility" is the right justification. Don't ask me for something you can't justify.

**Screen 8 — Consent split.** This is the actual pivot. Two cards: "Connect your records now" (recommended, primary button, "verified matches in about 8 hours") vs "Connect records later" (secondary button, "Keep browsing. Upgrade anytime"). The recommended badge on Path A is shaded just enough that I notice the nudge without feeling shoved. The footer reassurance — "We never share anything that identifies you with the companies behind treatments or trials" — is exactly the right sentence at exactly the right moment.

One real complaint: I want a third tile. "What's the actual difference?" I want a feature comparison row by row — application access, eligibility precision, notification frequency, data retained. The two cards make me read prose to compare, which is the wrong information architecture for a side-by-side decision.

**Screen 9 — Path A authorizations.** Two consents. HIPAA + records retrieval via "DNAvisit." The block layout is clean. "Read the full authorization →" links are present, which is the legal minimum and the UX maximum I expect at this stage. The DNAvisit name is doing a lot of unannounced work here — I have never heard of these people. Who are they? What's their SOC 2 status? Why am I meeting them in checkbox #2 of a healthcare onboarding without so much as a one-liner? If I tap "Read the full authorization" and I get a 4,000-word legal document, this is going to be the moment I bail.

**Screen 10a — Path A confirmation.** "You're set. We're getting your records." Clean. The "What happens next" numbered list is the right thing to show after a high-friction step.

**Screen 10b — Path B confirmation.** "You're in. Your matches are saved." Also clean. The promise "Connect your records whenever you're ready — one click, no re-signup" is the entire reason Path B exists, and saying it in plain text is good.

**Screen 11 — Uncovered indication.** Honest. "We don't have matching data for your condition yet." No marketing pivot, no soft-bullshit fallback. I respect it.

**Screen 12 — Education explorer.** This is where the brand voice slips a little. "The treatment landscape" with four cards (Approved treatments, Clinical trials in progress, How care decisions get made, Insurance and access). Each one reads like the table of contents to a content marketing strategy that hasn't shipped yet. The two anonymized patient stories are unsourced quotes from "JM, 47, Midwest" and "RT, 32, Northeast" — which is fine if real, but reads like a copywriter wrote them in twenty minutes. Where did these come from? Internal data? Reddit scraped? Made up?

**Screen 13 — App home (semi-converted).** This is where Path B users live. The warning banner ("These are approximate matches. Based on your quiz answers, not your medical records.") is the right persistent reminder. The inline upgrade CTA — "2 trials in your area are still accepting applications. Connect your records to apply directly" — is the right level of nudge. It's specific (2 trials, your area, accepting now). I'm a software guy. I respect specific over general.

**Screen 14 — Care option detail (gated).** Two gated blocks. "Site-specific eligibility details" and "Apply to this trial." Both blurred. Both with the same lock + CTA. The site-level detail is exactly the thing I want as a power user — "minimum disease duration 6 months, specific labs required" is the texture I need to evaluate fit. Gating it behind records is fair. The blur effect is honest about what's there without revealing it. Smart pattern. Bookmark + notify + read protocol are sensible "what you can do now without records" alternatives.

## 3. What works

- **The "Not a fit" bucket on the preview screen.** Showing me what you ruled out is a transparency move I will reward.
- **The "approximate matches" disclaimer persisting through the semi-converted app home.** You are not pretending you have certainty you don't have. That's grown-up product design.
- **The consent split itself.** The pivot is correct. Forcing full HIPAA + records consent on visit #1 is a conversion-killer in any healthcare flow, and the choice to let me browse and upgrade later is the same logic Plaid figured out for banking five years ago.
- **The "I'm not sure" option in the quiz.** Frictionless escape hatches. Good.
- **The detail-page "Why we matched you" checklist.** Three explicit criteria, each tied to a specific quiz answer. That's a real explainability surface, even if it's thin.
- **Microcopy promising no spam.** "We'll email you only if a new option fits your profile." If true, this is a competitive moat against literally every other healthtech mailing list I've been on.

## 4. What doesn't work

- **No methodology link anywhere.** I cannot find a "how matching works" link. No methods page, no algorithm description, no model card. For a product whose value proposition is "precise matching," the precision is a black box. Unacceptable for me.
- **No denominator.** "4 full matches." Out of what? 13? 87? 400? Without the denominator I can't tell if you're curating intelligently or just showing me everything you have for AChR+ generalized MG.
- **No filter or sort on the matches list.** No way to say "trials only," "in Texas only," "no placebo arms," "oral therapies only." For somebody with my needs, the absence of facets is the absence of a product.
- **No comparison view.** I want a table. Columns: option, type, phase, mechanism, site distance, time commitment, placebo chance, current enrollment status. Rows: each match. This does not exist. I will rebuild it in Notion within an hour of leaving this flow.
- **The 1.8-second fake loader on indication check.** Cut it. If you can't do the lookup in real time, fix the backend; don't theater it.
- **DNAvisit appears with no introduction.** I'm consenting to a third party I've never heard of. There should be a one-line "Who is DNAvisit?" with a "learn more" link inline next to the consent.
- **Hero headline is generic.** "Your personalized path to better care" could be on a Medicare Advantage ad.

## 5. Trust and safety assessment

This is where I spent the most time, because this is what matters.

What earns trust: the HIPAA-protected badge above the fold, the explicit "we never share identifying data with the companies behind treatments or trials" line on the consent split, the persistent "approximate matches" banner that doesn't let me forget you're not done analyzing me yet, and the gated blocks on the detail page that make data-share-required actions visibly unavailable rather than failing silently.

What does NOT earn trust: I cannot see a privacy policy link in the chrome of any screen I walked through. No cookies banner. No data-processing addendum. No SOC 2 attestation visible anywhere. No bug bounty / responsible disclosure mention. No "delete my account and all data" promise. No data-export promise. As an engineer who's read the breach notifications from 23andMe, MyHeritage, Change Healthcare, and four others I'm forgetting, the absence of explicit deletion + export commitments is conspicuous. Patients in 2026 should be able to expect both, on the landing page, with one click. Resonata is silent.

The DNAvisit third-party dependency is the single biggest unaddressed trust risk in this flow. You're asking me to authorize a vendor I've never heard of to retrieve my entire clinical record. The auth screen treats them as a footnote. They should be a profile: who they are, who funds them, where data lives, retention windows, deletion rights, whether they sub-process to anyone else. None of that is here.

## 6. Language review

**Earned:** "Approximate matches based on what you told us." "We're showing you these so you know we considered them — and ruled them out for you." "We saved these matches to your inbox." "Standard HIPAA consent only. Nothing retrieved yet." "We're focused on Myasthenia Gravis right now." These are concrete sentences with measurable claims. They're the voice the brand should be writing in everywhere.

**Patronizing or marketing-y:** "Your personalized path to better care." "Built by healthcare experts." (Which experts? Show me a clinical advisory board page.) "Precise care matching" as a tagline — fine, but only if you actually expose the precision. "You're in. Your matches are saved." has a vibe-y ring to it that feels just shy of a Headspace push notification. I'd kill the contractions and the chirp on confirmation screens. I'm an adult who just gave you my date of birth and antibody status. Talk to me like one.

**Imprecise:** "Strong fit," "may fit," "approximate matches." Pick numbers. Even bands — "match score 90%+", "match score 60–89%", "match score <60%". The verbal hedging reads as a designer not wanting to commit. Commit.

## 7. What's missing

In order of how much I care:

1. **Algorithm transparency / methods page.** What model? What inputs? What's the policy on negative results — do you show me trials that excluded me and why?
2. **Eligibility specifics with confidence intervals.** Every match should have a "fit confidence" with the criteria that contributed to it.
3. **Filter and sort on the matches list.** Trial vs approved, phase, site distance, mechanism class, dosing frequency, enrollment window. Minimum.
4. **Side-by-side comparison view.** I want to pin 3–4 options and see them in a table.
5. **Source citations on every match.** ClinicalTrials.gov NCT number, FDA label link, sponsor page. One-click out to primary sources.
6. **Data export.** Give me my full profile, my matches, and my notes as JSON or CSV. If you can't export it, I don't really own it.
7. **A "what changed since last visit" view.** I want to know if a new trial opened, an enrollment window narrowed, or a phase changed since my last login.
8. **DNAvisit transparency.** Who, where, retention, sub-processors.
9. **Specialist finder.** As a guy with a 4-month wait for an MG specialist, even an "MG Centers of Excellence" directory inside the app would be valuable.
10. **A subreddit/community signal.** Even one line of "X people with your profile are also looking at this option" — anonymized, aggregate. It would tell me you're a network, not just a database.

## 8. Overall verdict

Would I finish the flow? Yes. The friction is low enough that I'd push through the three quiz questions and see the preview screen, partly because three questions is below my abandon threshold and partly because I'm curious what they have on AChR+ ocular MG. (Side note: the quiz doesn't ask whether I'm ocular or generalized in a way that gates trial display correctly for me — the example data shows generalized matches for what appears to be an ocular profile. That's a real bug, not just a nit.)

Would I connect records? Path B, for now. The Path A promise of "verified matches in 8 hours" is appealing, but I'm not handing over my entire clinical record to a brand-new vendor and a never-heard-of-them records partner until I've (a) read their privacy policy, (b) looked up DNAvisit's security posture, and (c) verified the matching methodology with at least one external source. Path B lets me browse, scope the data quality, sniff-test the trial list against ClinicalTrials.gov directly, and upgrade only when I've decided they're not bullshit. The fact that Path B exists is the reason I'd stay in the funnel at all. Without it, I'd have closed the tab at the consent screen.

Would I post it on r/myastheniagravis? Soft yes, with caveats. Something like: *"Resonata launched a new onboarding. Three-question quiz, shows approximate matches up front, lets you browse before connecting records. Better than ClinicalTrials.gov for usability, but no filter/sort yet and the algorithm is a black box. Worth a look if you want a curated view, but cross-check everything against the primary source."*

Net: this is the best patient onboarding I've seen in this category in a year, and it's still about 40% of where it needs to be for somebody like me. The pivot to Path A / Path B is correct. The interaction design is competent. The data exposure is too thin and the trust scaffolding is too quiet. Fix the methodology surface and add filter/sort/compare, and I'll convert to Path A within a week.

Until then — I'm semi-converted. Which I think is exactly the state your PM wanted me in. Touché.
