# Jasmine Patel — Patient Onboarding v2 Usability Review
**Date:** 2026-05-13
**Prototype:** patient-onboarding-v2-prototype.html (semi-conversion pivot)
**Synthetic User:** Jasmine Patel (28, NYC, generalized MG 2yr, Anti-MuSK+, digital native)

---

## First Impressions (the 5-second test)

Okay, so. I opened this on my phone, mid-commute, between the F and the 6, the way I open every health thing someone DMs me. First five seconds.

The green is fine. Inoffensive. Exactly the green every healthcare brand has used since 2019 — Hims green, Headspace green, Calm-but-make-it-medical green. It doesn't repel me, but it also doesn't make me screenshot anything. The logo is a little wave inside a circle which… sure. I get the "resonance" thing intellectually. Visually it reads as a generic wellness mark. It would not survive a brand audit at my company.

The headline — "Your personalized path to better care." — is grammatically correct and emotionally inert. The little badge that says "Built by healthcare experts · HIPAA-protected" is good. That actually does work on me. It signals "this is not a TikTok wellness grift," which is the bar a lot of health apps don't clear. The "Free for patients" trust pill is also smart — I clocked that in like a second.

But here's the thing: this homepage is talking to no one in particular. It is not talking to *me*. There's no patient in it — no face, no quote, no "I'm 31 and I have MG" energy. It's a clean white page with form fields. I'd put it in the bucket of "looks competent, doesn't feel like it knows I exist." Which is the bucket where most of healthcare lives. Sigh.

Would I screenshot? No. Would I close? Not yet — the email + condition form is a low enough commitment that I'd at least poke it. So we make it past the first cut. Barely.

---

## The Walkthrough — Screen by Screen

**Landing.** Two fields. Email + condition dropdown. Fast. Mobile-friendly enough. The fact that I can pick "Myasthenia Gravis" right there from the dropdown and not have to type it (and not auto-correct it to "anesthesia" three times) is genuinely a small gift. The little "How it works" sidebar with the three numbered steps is fine — it does the job of "I won't have to do anything terrifying in the next 30 seconds." The framing of step 3 — "Connect your medical records for verified, precise matches. Or browse what we have and connect later." — is the first time I actually relax. Because that's the deal. You're not going to make me upload my entire EMR before I see anything. Good. That earns you the next click.

**Indication check (loading).** A spinner with "Checking your condition coverage…" Fine. Two seconds. I won't bounce on a 2-second spinner. Honestly the copy underneath — "We're confirming we have matching data for your profile" — is tight. I'd notice if it said something cute and I'd judge you. It doesn't. Good restraint.

**Quiz Q1 — MG type.** Big radio cards, tap targets are big enough for thumbs. Generalized MG, Ocular MG, Juvenile MG, I'm not sure. I tap Generalized. The selected state with the green border + halo is satisfying — feels like an iOS toggle, not a 2014 Bootstrap button. Good. The "I'm not sure" copy ("No problem — we can still show approximate matches based on what you do know") is the right tone. Not condescending. Not chirpy. Quietly competent. I'd keep going.

**Quiz Q2 — Antibody status.** Here's where I sit up. AChR-positive, MuSK-positive, Seronegative, I don't know. Hey — *I'm in there.* MuSK-positive is a real option, not an afterthought. I'm tapping it. And the description — "Anti-muscle-specific kinase antibody. Different treatment options apply." — is correct, clinical without being patronizing, and acknowledges that my treatment universe is different. This is the first moment where the product feels like it might know I exist. Small but real.

I do want to flag, though — the percentages in the AChR description ("Most common form, about 85% of generalized MG") subtly position MuSK as the small minority. Which factually it is. But I notice you don't give MuSK a parallel framing. That's a missed warmth opportunity.

**Quiz Q3 — Treatment stage.** "Where are you in your treatment?" Newly diagnosed, On treatment looking for better options, Tried multiple without success, Stable exploring what's new. I tap "On treatment, looking for better options." Good options. But I'm noting: nothing here asks anything about *my life*. Not whether I work. Not whether I want kids. Not whether I'm planning a pregnancy in the next two years which is, currently, the single most important thing I'd want a matching tool to know about me. I'll come back to this.

**Preview matches.** This is the screen that decides whether I become a user or not. And — okay. The structure is genuinely good. Three buckets: Full matches, Potential matches, Not a fit. Counts in a circle. Color-coded. The "Not a fit" bucket — showing me what you ruled out and why — is the move. That's a trust signal that almost nobody else does. It tells me you're not just funneling me into whatever pays you. I might screenshot the "Not a fit" idea and send it to my friend who does product at a fintech. That's a compliment.

But the actual content of my "Full matches" is all… AChR. Vyvgart, Rystiggo (yes, this one is for both AChR and MuSK, props), Nipocalimab AChR+ trial, Telitacicept. The trial names literally have "AChR+" in them. If I'd actually picked MuSK in Q2, this preview should look completely different. Either the prototype isn't dynamic to my answers (which, fine, it's a prototype), or the copy on the cards reveals that MuSK content barely exists in the system. Either way the bottom line for me as a MuSK+ patient would be: this list is mostly not for me. The "Save your matches to your inbox" banner is a nice touch but I'd be left wondering whether the email I get next week is going to be relevant or just AChR newsletter spam.

**Complete sign-up.** First name, last name, state, DOB. Standard. Fast. The line "Your matches are already saved. This step creates your account so you can come back to them and apply when you're ready" is a really good piece of copy. It reframes the form as a save-your-progress step rather than a wall. I'd fill this out. Three minutes max.

The state dropdown defaults to Illinois which is weird (I'm in NYC and you've already taken my email — couldn't you guess?) but it's a prototype, fine.

**Consent split.** This is the screen I think the team is most proud of and they should be. Two cards side by side. "Connect your records now" with a "Most precise" tag. "Connect records later." The "later" option doesn't shame me. The bullets are clear. The reassurance line at the bottom — "Your data stays protected either way. We never share anything that identifies you with the companies behind treatments or trials" — is exactly what I'd want to read. The "Most precise" tag tells me which choice you want me to make without screaming at me. This is the most thoughtful screen in the whole flow.

But — and this is real — I would tap "Connect later." Eight hours and signing two consents to give a healthcare startup access to my entire medical record so they can refine some matches that I haven't yet decided I trust? On the F train? With my partner not consulted? No. The "connect later" choice exists because of people like me. Thank you for not blocking my exit.

**Path A authorizations.** Two big white blocks. HIPAA Authorization. Records Retrieval Authorization (DNAvisit). Checkboxes. "Read the full authorization →" links. This is pretty good legal-UI for a hard moment. I appreciate that you named the records partner (DNAvisit) up front instead of burying it. I would absolutely click "Read the full authorization" and skim it for any "we may share with marketing partners" language. The page doesn't show me what's behind that link in this prototype, so I can't grade you on it. But the willingness to name the third party = trust deposit.

**Path A confirmation.** "You're set. We're getting your records." Clean. Tells me what happens next in 4 steps. Fine. The CTA is "See my current matches" which is the right thing to put there — you're not leaving me on a thank-you page.

**Path B confirmation.** "You're in. Your matches are saved." Also clean. The line "When you're ready for precision matches, connecting records takes about 3 minutes" is a good little re-marketing seed. Not pushy. Just there. CTA: "Enter Resonata," which is on-brand and inoffensive.

**Uncovered indication branch.** Logged it. Decent fallback. The "We're expanding to more conditions" badge and "stay on the list" framing is the right move. The Education Explorer screen has stories from "Patient · 47, Midwest" and "Patient · 32, Northeast" — anonymous, themed. Smart for HIPAA. Boring for me. I want a real face, real first name, real lifestyle context. (More on this in What's Missing.)

**App home (semi-converted).** This is where I'd actually live. The yellow banner at the top — "These are approximate matches. Based on your quiz answers, not your medical records. Connect records to make them precise →" — is good. Persistent reminder that I have an upgrade path, without it being a popup that I have to dismiss. The "2 trials in your area are still accepting applications. Connect your records to apply directly" inline CTA is well-tuned. It's specific, scarce, and actionable. It's the kind of thing I'd actually click on day three when curiosity got the better of me.

**Care option detail (gated).** Blurred-out site list with a lock icon and "Connect records to see details." This is *exactly* the right gate. You let me see the trial. You let me read what it's studying. You let me bookmark it. The gated thing is the operational stuff (sites, eligibility specifics, the apply button) — which is genuinely the part that needs verified records. The gate is justified, not arbitrary. I respect a justified gate. The "What you can do now without records" box at the bottom (Bookmark, Get notified, Read protocol) is smart — it tells me the unconverted state is still useful, not punitive.

---

## What Works

1. **The "records as upgrade, not gate" architecture itself.** It is the single biggest reason I'd make it past the consent screen. Every other healthcare matching thing I've tried wants my whole life before showing me anything. This shows me the thing first. That alone earns the team a coffee.
2. **The "Not a fit" bucket on the preview screen.** Showing me what you ruled out is the most credibility-building thing on the entire page. It signals "we're matching, not selling."
3. **The justified gate on the detail screen.** Blurring the site-specific stuff but letting me see the trial + bookmark it = correct gate philosophy. I don't feel paywalled. I feel respected.
4. **Naming the records partner (DNAvisit) up front.** Most apps would hide this in a 30-page ToS. You named it. Trust deposit.
5. **The persistent "approximate matches" banner in the app home.** It's honest about the limits of what I'm seeing without panicking me into upgrading. That's a hard tone to land. They landed it.
6. **Tap targets, type sizing, mobile feel.** This actually works on a phone. That is a low bar that healthcare almost always trips over. Pass.

---

## What Doesn't Work

1. **The visual design is professionally generic.** It looks like every other green healthcare brand from 2021. Inter at default weights, the Hims-and-Hers green palette, soft drop shadows, rounded rectangles. Competent. Forgettable. I would not screenshot a single screen of this for aesthetic reasons. If you want me to share Resonata in my chronic illness DM circle, the screens have to look like *something* — a typographic point of view, a quieter color palette, a real illustration system, *anything* with a personality. Right now this looks like the design system from a Series A health tech I've already churned out of.
2. **MuSK-positive content is structurally underweight.** Everything in the "Full matches" bucket on the preview screen is AChR. The trial names literally read "for generalized AChR+ MG." If I'd selected MuSK on Q2, the matches don't update — which I get is a prototype limitation, but the deeper signal is that the *content library* this is being built from is AChR-centric. As a MuSK+ patient I'd assume from this preview that you don't have much for me, and I'd quietly stop opening the emails.
3. **The quiz is medically scoped, not life-scoped.** Three questions: type, antibody, treatment stage. Nothing about whether I work. Nothing about whether I want kids. Nothing about whether I want oral vs infusion. Nothing about pregnancy planning. For me — and I think for most patients in their 20s and 30s — those are the actual matching variables that matter. The quiz feels like it was designed by a clinical team, not a patient-experience team.
4. **The patient stories are anonymous.** "Patient · 47, Midwest." HIPAA, I know, I know. But you can absolutely use opt-in patient ambassadors with first names, real photos, and real bios. As-is, the anonymized stories read like a stock-photo wellness brochure. They reduce trust, not build it.
5. **No community surface anywhere.** I can save matches. I can bookmark. I cannot find another person like me. For a 28-year-old with a rare-ish autoimmune disease, that is the whole reason I am on the internet. If I can't find one other Anti-MuSK+ woman in her 20s on this platform, I am leaving for a Facebook group within a week.
6. **The state dropdown defaults to Illinois.** Why. You have my email. You have my IP. Make a guess. This is a small thing that signals "this team has not actually used this product on a phone."

---

## Trust & Safety Assessment

I read privacy policies sometimes. I'm the friend everyone forwards a sketchy app to because I'll go find the data sharing clause and screenshot it back.

This product reads as legitimately serious about HIPAA. The trust signals are placed where they should be — under the form, on the consent split, on the "Your data stays protected" reassurance bar. The choice to break consent into two distinct authorizations (HIPAA + DNAvisit retrieval) is the right architecture; lots of companies bundle these into one click and it's actually shadier than it looks. Naming the third-party records partner is the move that genuinely converts me from "skeptical" to "okay, I'll keep reading." The "we never share anything that identifies you with the companies behind treatments" line is the line I needed to see. If that's actually true and survives a deeper read of the privacy policy, this product is in the small minority of patient apps I'd trust with a record.

The thing that *doesn't* feel serious yet is the marketing framing. The hero headline is generic enough that it dilutes the seriousness underneath. If your product is actually a serious matching engine — and the architecture suggests it is — the marketing surface should feel less like every other Series A health brand and more like the thing it is: a precision matching platform for patients with serious conditions.

---

## Language Review

**What worked:**
- "Approximate matches based on what you told us." Honest, specific, doesn't oversell.
- "We saved these matches to your inbox." Concrete promise.
- "We can come back to them and apply when you're ready." Removes urgency without removing motivation.
- "Most precise" as the recommendation tag. One word. Specific. Not "Recommended."
- "Your matches are saved either way." Calms the panic.
- "We're showing you these so you know we considered them — and ruled them out for you." This is the line I'd quote in a tweet.

**What missed:**
- "Your personalized path to better care." Generic. Could be any healthcare brand. Not Resonata-specific.
- "Get matched to treatments and clinical trials that fit your specific medical profile." Functional, not memorable.
- The "Welcome, Jane" placeholder in the app header. (Known prototype thing, but worth saying — the second I see "Jane" my brain goes "this is a demo for an investor.")
- "Tell us a little." → "Tell us a bit" reads slightly more current. "A little" is the kind of phrasing that tips me into "designed for someone older."
- The quiz helper texts skew slightly clinical ("Your antibody profile changes which treatments and trials are likely to work"). True. Could be warmer with a human framing.

The tone overall is competent and grown-up. Not cute. Not chirpy. Not patronizing. That is honestly the right zone for a serious health product, and a lot of the small copy choices land. The brand voice guide says "we are healthcare people, not silicon-valley tech bros," and the copy actually delivers on that.

---

## What's Missing

For me, specifically:

1. **Anything about pregnancy or fertility.** This is the #1 thing I would want a matching tool to know. There is not a single touchpoint in the entire 14-screen flow that asks me, surfaces options for me, or even acknowledges that this is a question patients have. For a product positioning itself as precision matching, missing the fertility-relevant filter is a structural gap.
2. **Anti-MuSK+ specific options as first-class content.** Not just Rystiggo getting a cameo. A real "MuSK+ care options" view, with the trials and approved therapies that work for that antibody profile.
3. **Community.** A way to find other patients with my profile. Even a "12 other people with your match profile have looked at this trial" social signal would be huge.
4. **Lifestyle/career filters.** Oral vs infusion. Weekend vs weekday infusion availability. Treatments that minimize visible side effects (no moon face, please). Treatments that don't tank fertility.
5. **Patient stories with faces and first names.** Opt-in. With real lifestyle context. "Maya, 29, marketing, MuSK+, here's how she navigated infusion scheduling around her job." That's the thing I'd send to my best friend.
6. **Something shareable.** A clean shareable card I could send to my partner ("here's what I found"). A copy-link for a saved match. Anything that acknowledges this is a thing I'd want to talk about with someone.
7. **A "what would change" preview before consent.** Before I commit to records retrieval, show me what *specifically* sharpens — "your matches will go from 4 strong → 3 verified strong + 2 you didn't know qualified for," that kind of thing. Make the upgrade tangible.

---

## Overall Verdict

Would I complete this flow? **Yes**, through Path B. The friction is low enough, the consent split is humane enough, and the "browse first, decide later" architecture is genuinely the right read on what I need.

Would I open the email when matches arrive? **Once.** If the first one is generic AChR content that doesn't apply to me as a MuSK+ patient, that's the last one I open.

Would I connect records (Path A)? **Not on day one.** Maybe day 14, after I've poked around the app, seen one or two genuinely relevant things, and built up enough trust to give you the keys. The persistent upgrade banner + the gated detail screen are the right pressure mechanisms — soft, present, justified.

Would I DM someone about it? **Honestly, not yet.** The architecture is smart and the consent UX is humane and I respect the team for shipping the "records as upgrade" pivot. But the surface — the visual identity, the absence of MuSK content, the missing fertility/community/lifestyle layer — isn't there yet. If they fix the content gaps and give the brand a real point of view, I'd send it to every chronically-ill 20-something I know. Right now I'd send it to my mom, who would say "oh that looks nice, dear." Which is exactly the wrong audience.

Make the surface as serious and specific as the architecture. Then we talk.
