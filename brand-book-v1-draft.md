# Resonata Brand Book — v1 Draft

**Status:** Interim reference, Phase 1. Comprehensive enough to evaluate for brand rules; not yet finalized.
**Date:** 2026-05-04
**Owner:** Lauran (Product). Final approval on cull/tier decisions: Martin (CEO).
**Audience for this doc:** Resonata team, future designers/copywriters, and Claude's web-artifacts-builder when generating prototypes.

---

## How to Use This Document

1. **For copy and prototypes:** treat sections 2–6 as binding once Martin signs off.
2. **For Claude's web-artifacts-builder:** the CSS token block in section 8 is the source of truth. Tokens take precedence over hex values mentioned in prose elsewhere in this doc.
3. **For open questions:** every unresolved decision is captured in section 9 (Rules to Validate). Do not silently invent a position on those — flag and ask.

---

## 1. Brand at a Glance

**What Resonata does:** Precision patient matching. We match patients to clinical trials and treatments based on their specific medical profile and history. The platform also includes an enroller workbench that delivers eligible, interested patients to designated clinical staff.

**Core stance:** Patient-first. Care options come to the patient based on who they are, not the other way around.

**Five audiences (do not cross messaging lines):**
1. Patients with serious or chronic conditions
2. Biopharma sponsors (clinical research / marketing teams)
3. Sponsor agencies
4. Enrollers (clinical staff at sites)
5. Investors

**Anti-positioning:** We are not "tech bros." We are not selling a gimmick. We are not mass-marketing a treatment. We are a sophisticated matching service that solves a real patient problem.

**Values (carry through everything):**
- **Seriousness** — we know healthcare; we are not playful about it.
- **Empowerment** — patients are people, not conditions.
- **Compassion** — getting the right care option fast matters.
- **Respect** — patients are capable of informed decisions when given real information.

**Regulatory frame:** HIPAA. Resonata handles ePHI. Visual and copy decisions must never expose, imply, or invite the storage of identifying information.

---

## 2. Logo System

### 2.1 Wordmark

- **Type:** Poppins SemiBold
- **Size in primary lockup:** 48px
- **Color:** Charcoal `#393939` (Charcoal 15)
- **Spacing:** Maintain clear space equal to the cap-height of the wordmark on all sides.

### 2.2 Tagline (in lockup only)

- **Phrase:** "On Target Care Options"
- **Type:** Poppins Regular
- **Size:** 22px
- **Placement:** Below wordmark, baseline-aligned to the wordmark's left edge.
- **Do not** use the website hero phrase ("Care Options for People Who Need Them") in a logo lockup.
- **Do not** use "Finds that fit." anywhere — that copy is retired.

### 2.3 Website hero (separate from logo)

- **Phrase:** "Care Options for People Who Need Them"
- **Surface:** resonata.health home page only.
- Treat as headline copy, not a tagline. Do not pair with the logo.

### 2.4 Color variants — proposal

The marketing drive currently has **24 logo files in 7 colors** (black, white, blue, red, purple, gold, green). The 7 colors map 1:1 to the 6 brand hues + white. This is intentional but excessive for a healthcare brand. **Proposed cull (pending Martin's approval):**

| Variant | Status | Use |
|---|---|---|
| Charcoal `#393939` | **Keep** — primary | Default lockup on light backgrounds |
| White `#FFFFFF` | **Keep** | On dark/photographic backgrounds |
| Brand Green `#319218` | **Keep** | Marketing surfaces, single-color emphasis |
| Black | **Retire** | Use Charcoal instead |
| Blue | **Retire** | Repurpose Blue palette for UI states only (see §3) |
| Red | **Retire** | Repurpose Red palette for error states only |
| Purple | **Retire** | No clear use; dilutes the system |
| Gold | **Retire** | Repurpose for warning states only |

**Rationale:** healthcare brands earn trust through restraint. A 7-color logo system reads as "we haven't decided yet" and undermines the seriousness pillar.

### 2.5 Misuse — never do these

- Do not stretch, skew, or rotate the wordmark.
- Do not place the Charcoal lockup on backgrounds darker than the Charcoal 5 shade (low contrast).
- Do not place the wordmark on busy photography without a solid surface behind it.
- Do not recolor the wordmark to any hue not approved in §2.4.
- Do not add effects (shadow, glow, gradient) to the wordmark.

---

## 3. Color Palette

The Figma design kit defines a **6-hue × 17-shade ramp** (Blue, Green, Red, Gold, Purple, Charcoal). Shades are dual-named (numeric 1–18 and Tailwind-style 50–900). This is the raw system; below is the proposed role-based tiering.

### 3.1 Tier 1 — Brand (use freely)

| Token | Hex | Source | Use |
|---|---|---|---|
| `--brand-green` | `#319218` | Green 13 | Primary brand color. **All CTAs.** Accent surfaces, success emphasis. |
| `--white` | `#FFFFFF` | — | Default surface, reversed wordmark on photography |

**Charcoal is NOT a brand surface color.** It is **text/ink only** — used for the wordmark, body copy, and outlines. Do not use Charcoal as a background fill for sections, sign-up zones, banners, hero blocks, or large surfaces. CTAs are never Charcoal; they are always Brand Green.

| Token | Hex | Source | Use |
|---|---|---|---|
| `--charcoal` | `#393939` | Charcoal 15 | **Text and ink only** — wordmark, body copy, outlines, secondary outline buttons |

### 3.2 Tier 2 — Semantic (use only for state)

| Token | Hex | Source | Use |
|---|---|---|---|
| `--state-error` | `#ED431D` | Red 10 | Errors, destructive confirmations |
| `--state-warning` | `#F9C461` | Gold 9 | Warnings, attention-needed states |
| `--state-info` | `#EBF3F9` | Blue 1 (surface) | Informational surfaces |

### 3.3 Tier 3 — Neutrals (UI scaffolding)

Use the Charcoal ramp (Charcoal 1–17) for body text, borders, dividers, surfaces. Sample: Charcoal 9 = `#BFBFBF`.

### 3.4 Tier 4 — Retired / hold

- **Purple ramp** — proposed retire. No active role. Hold in Figma but do not use in product or marketing surfaces without re-approval.

### 3.5 Background rules — non-negotiable

- **No pure black (`#000`) backgrounds.** Healthcare context, low-vision accessibility, and our seriousness pillar all argue against it.
- **No Charcoal slab backgrounds.** Charcoal 15 is text/ink, not a section surface. Sign-up sections, hero zones, privacy sections, and footers are NOT Charcoal slabs. Use light surfaces (White, Charcoal 1) or Brand Green soft tint instead.
- **Brand Green soft tint** (`#E8F4E4`) is the preferred "branded" surface for sign-up zones, demo zones, and other moments that need to feel distinct from default content. Saturated full-bleed Brand Green is reserved for accent moments (CTA buttons, single-color emphasis); it is not a canvas for body content.
- **Default surface:** White (`#FFFFFF`) or Charcoal 1 (`#F8F8F8`).
- **Patient-facing screens default to light mode.** Dark mode is a Rule to Validate (§9).

### 3.6 Contrast minimum

All text must meet **WCAG AA** (4.5:1 for body, 3:1 for large text). Brand Green on white is borderline for small text — verify before using for body copy.

---

## 4. Typography

### 4.1 Type family

- **Primary:** Poppins (already used for wordmark and tagline).
- **Weights in use:** SemiBold (600), Regular (400). Consider Medium (500) for UI labels. **Do not use** Black (900) or Thin (100/200) — both undercut the brand's seriousness.

### 4.2 Type scale (proposed — to validate)

| Token | Size | Weight | Use |
|---|---|---|---|
| `--type-display` | 48px | SemiBold | Wordmark only |
| `--type-h1` | 36px | SemiBold | Hero / page title |
| `--type-h2` | 28px | SemiBold | Section title |
| `--type-h3` | 22px | SemiBold | Subsection / lockup tagline |
| `--type-body-lg` | 18px | Regular | Lead paragraph |
| `--type-body` | 16px | Regular | Default body |
| `--type-body-sm` | 14px | Regular | Secondary / metadata |
| `--type-caption` | 12px | Regular | Labels, footnotes |

### 4.3 Type rules

- Line height: 1.5 for body, 1.2 for headings.
- Tracking: default. Do not letter-space body copy.
- **Do not** use all caps for body. Sentence case for everything except short labels.
- **Do not** italicize medical terms by default. Italics only for emphasis where unavoidable.

---

## 5. Imagery and Illustration

### 5.1 Direction

- **Vector-first.** The website leans on illustration over photography. Continue that direction — illustration is more controllable for HIPAA and avoids implying we have specific real patients in our system.
- **Photography:** if used, must be respectful, not stocky, and never imply a specific condition or identifiable patient. Avoid hospital-bed/medical-distress tropes.

### 5.2 Illustration rules

- Use the brand palette only. No off-system colors.
- Human figures should be inclusive and ambiguous regarding specific conditions.
- Backgrounds: white, Charcoal 1, or a single brand-tinted surface. No busy textures.

### 5.3 Iconography

- Use platform-native icon sets where the connected libraries already provide them (Material Symbols on Android/web, SF Symbols on iOS). Do not invent custom icons unless brand-critical.

### 5.4 Patient privacy in visuals

- **Never** show real patient names, IDs, DOBs, or identifying details in screenshots, demos, or marketing.
- Use synthetic personas (Sarah Chen, Jasmine Patel, Marcus Williams, Dorothy Martinez) for any patient-shaped UI.

---

## 6. Voice

### 6.1 Locked voice attributes (4)

| Attribute | Means | Does not mean |
|---|---|---|
| **Empowering** | Supportive, enabling, collaborative. "You've got options." | Cheerleading, hype, exclamatory. |
| **Professional** | Expert, credible, authoritative. We know healthcare. | Stiff, jargon-heavy, gatekeeping. |
| **Grounded** | Calm, plain, real. We meet patients where they are. | Cold or clinical. Saccharine sympathy. |
| **Precise** | Accurate, specific, measured. "We tell you how you matched on every element." | Pedantic, robotic, technical for its own sake. |

**Why these (not Warm, not Innovative):** "Warm" conflicts with our explicit avoid-list (sympathize/empathize/sorry). "Innovative" conflicts with the anti-tech-bro stance. "Grounded" and "Precise" do the work of those attributes without the failure modes.

### 6.2 Tone by context

- **Onboarding new patients:** Empowering, optimistic, hopeful, reassuring. Confidence we understand what they need and respect their privacy.
- **Explaining medical information:** Accessible without dumbing down. Professional and serious; not patronizing; not gatekeeping with medical jargon.
- **Errors / technical issues:** Short, calm, actionable. Not cute, not exclamatory.
  - Good: "Your matches will be ready soon. We'll email you to let you know."
  - Bad: "No matches! Sorry 🙂"
- **Success confirmations:** Short, reassuring.
  - Good: "Thank you. Consent form completed."
  - Bad: "Thanks!! You're the best!" or "Done"
- **Reminders:** Brief, calm, friendly without being personal.
  - Good: "Reminder: There are still 2 steps to do to complete enrollment. Click here to start."
  - Bad: "Hey! Just reminding you your enrollment isn't complete yet!"
- **Billing / payments:** Brief, calm, friendly without being personal.

### 6.3 Sentence style

Short and clear. Active voice but collaborative language ("Let's", "You've got", "We'll").

### 6.4 Words/phrases we use

- "Let's go"
- "We're here to help"
- "You've got options"
- "Precise matching"
- "Recommendations based on your actual medical history"
- "No more irrelevant drug ads or dead-end searches"
- "Where you already are" (re: ad placement)
- "Care options when you need them — that's all we do"
- "We check the eligibility criteria, so you don't have to"
- "We tell you how you matched on every matching element"
- "Your data stays protected"
- "No identifiable information shared with companies behind the treatment options"
- "You decide if you want to disclose your information"

### 6.5 Words/phrases we avoid

- "Sympathize," "empathize," "sorry," "this is tough," "we're sorry you're going through this"
- "Patient Portal" (we are not a portal)
- Emoji of any kind in product copy
- Em dashes (per house style)
- Tech-bro language: "disrupt," "revolutionize," "AI-powered" (when "matching" suffices), "next-gen"

### 6.6 Voice by audience (high level — to expand in Phase 2)

Voice attributes are constant. Tone shifts by audience. **Never reuse copy across audiences without checking it.** The 5-audience framework is canonical (see `context-library/product/product-overview.md`).

---

## 7. Application Rules (Product UI)

### 7.1 Platform-native deference

The Figma file connects four design system libraries: **Material 3, iOS 18, iOS 26, and a Simple Design System.** Resonata's strategy is to lean on platform-native components and layer brand on top, not to build a bespoke component system.

**Rule:** Default to platform-native for component behavior (buttons, sheets, navigation, form controls). Override only for:
- Color (apply the palette in §3)
- Typography (Poppins, scale in §4)
- Voice (copy per §6)

### 7.2 Mobile first

98% of patient traffic is mobile. Design and review at mobile breakpoints first; desktop is a stretch case. Prototypes that don't work at 375px wide are not done.

### 7.3 Brand presence

- Logo wordmark in the top-left of patient-facing screens, Charcoal on white surface.
- **All CTAs are Brand Green** (filled). Secondary actions are Charcoal outline (text + border only, transparent fill). No Charcoal-filled CTAs.
- Saturated Brand Green is for buttons and accents, not large body-content backgrounds. Use Brand Green soft tint (`#E8F4E4`) when a section needs a branded surface.
- No Charcoal slab backgrounds anywhere.

### 7.4 Density

Patient-facing screens favor lower density (more whitespace, larger tap targets). Enroller workbench may run denser but should still respect the 4px grid.

---

## 8. CSS Tokens (for web-artifacts-builder)

This block is binding for any prototype Claude generates. Tokens take precedence over prose hex values elsewhere in this doc.

```css
:root {
  /* Brand */
  --brand-green: #319218;          /* Tier 1: all CTAs, primary accent */
  --brand-green-dark: #26701A;     /* CTA hover / pressed (darker green, NOT charcoal) */
  --brand-green-soft: #E8F4E4;     /* preferred "branded" section surface */
  --brand-green-mid: #C7E5BC;      /* outline / secondary surface accent */
  --white: #FFFFFF;
  --charcoal: #393939;             /* TEXT/INK ONLY — never a section surface or CTA fill */

  /* Neutrals (Charcoal ramp — sample; full ramp to be filled in Phase 2) */
  --charcoal-1: #F8F8F8;   /* surface */
  --charcoal-3: #E5E5E5;   /* dividers */
  --charcoal-9: #BFBFBF;   /* placeholder text */
  --charcoal-13: #6B6B6B;  /* secondary text */
  --charcoal-15: #393939;  /* primary text */

  /* Semantic state */
  --state-error: #ED431D;     /* Red 10 */
  --state-warning: #F9C461;   /* Gold 9 */
  --state-info-bg: #EBF3F9;   /* Blue 1 */

  /* Typography */
  --font-family: "Poppins", system-ui, -apple-system, "Segoe UI", Roboto, sans-serif;
  --font-weight-regular: 400;
  --font-weight-medium: 500;
  --font-weight-semibold: 600;

  --type-h1: 36px;
  --type-h2: 28px;
  --type-h3: 22px;
  --type-body-lg: 18px;
  --type-body: 16px;
  --type-body-sm: 14px;
  --type-caption: 12px;

  --line-height-tight: 1.2;
  --line-height-body: 1.5;

  /* Spacing (4px grid) */
  --space-1: 4px;
  --space-2: 8px;
  --space-3: 12px;
  --space-4: 16px;
  --space-6: 24px;
  --space-8: 32px;
  --space-12: 48px;

  /* Radius */
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 16px;

  /* Surfaces */
  --surface-default: var(--white);
  --surface-muted: var(--charcoal-1);
  --surface-info: var(--state-info-bg);

  /* Text */
  --text-primary: var(--charcoal-15);
  --text-secondary: var(--charcoal-13);
  --text-on-brand: var(--white);
  --text-link: var(--brand-green);
}

/* Hard prohibitions for prototypes */
body { background: var(--surface-default); color: var(--text-primary); }
/* Do NOT use: background: #000; — pure black is out of brand. */
/* Do NOT use: var(--charcoal) as a section background or button fill — Charcoal is text/ink only. */
/* Do NOT use: var(--brand-green) as a full-bleed body-content background — saturated green is for buttons and accents. Use --brand-green-soft for branded surfaces. */
/* All CTAs use background: var(--brand-green); hover: var(--brand-green-dark). */
```

---

## 9. Rules to Validate

Open decisions. Each needs a clear ruling before Phase 2. Items marked **(Martin)** need CEO approval; others are team calls.

1. **Logo color cull (Martin).** Approve retiring Black, Blue, Red, Purple, Gold logo variants. Keep Charcoal, White, Brand Green only.
2. **Brand-strength shade ambiguity.** Production uses Green 13 (`#319218`); design kit highlights shade 11 (550). Confirm canonical brand green.
3. **"Finds that fit." removal.** Stale tagline still in Figma file `65qAJnr1ExnGP9KzmwmNOn`. Remove from all artifacts.
4. **Tagline placement rule.** Confirm "On Target Care Options" is logo-only and "Care Options for People Who Need Them" is hero-only. (Already locked verbally; lock in writing.)
5. **Black backgrounds banned (Martin).** No `#000` backgrounds anywhere in product, marketing, or prototypes. Charcoal 15 is the floor.
6. **Palette role split (Martin).** Confirm 3-tier proposal in §3 (Brand / Semantic / Neutrals) and the retirement of Purple.
7. **Dark mode policy.** Decision: do we ship a dark mode for patient-facing surfaces? Default proposal: no, light only.
8. **Photography vs. illustration.** Confirm vector-first as the standing direction. If photography is allowed, set guardrails.
9. **Type scale.** Confirm scale in §4.2. Verify against the Figma design kit's actual text styles.
10. **Platform-native deference rule.** Confirm strategy of leaning on Material 3 / iOS components rather than custom components.
11. **Contrast minimum.** Lock WCAG AA as the floor. Decide whether AAA is aspirational or required for any surface.
12. **Patient privacy in visuals.** Confirm rule: synthetic personas only in any visual artifact (mocks, demos, marketing).
13. **Voice by audience.** Phase 2 needs per-audience tone refinement (patients vs. sponsors vs. investors). Out of scope for v1, flagged here.
14. **Wordmark clear-space and minimum size.** Spec'd loosely above; needs a visual diagram in Phase 2.
15. **CSS token completeness.** §8 covers the essentials but not the full Charcoal ramp or hover/focus state tokens. Flesh out before web-artifacts-builder relies on it for production-quality prototypes.

---

## 10. Phase 2 Scope (not in this doc)

- Per-audience voice playbooks (patients, sponsors, sponsor agencies, enrollers, investors)
- Component-level UI specs (buttons, forms, cards) layered over platform-native libraries
- Full Charcoal ramp + hover/focus/disabled state tokens
- Visual examples (correct vs. incorrect logo use, color use, photography)
- Email and Slack template guardrails
- Investor-deck visual rules

---

*End of v1 draft. Next step: Lauran reviews → resolve §9 items with Martin → convert to Google Doc as the canonical reference.*
