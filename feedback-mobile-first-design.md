---
name: mobile-first-design
description: Resonata's patient-facing design must be mobile-first — designed for narrow viewports first and progressively enhanced for larger screens, not desktop-first with mobile fallbacks. Especially: the hero/landing must be form-dominant on mobile, with no competing image stealing above-the-fold real estate.
type: feedback
date_captured: 2026-05-13
captured_by: Claude (cowork session, patient onboarding v2 prototype)
target_memory_location: ~/Developer/resonata/context-library/_memory/
---

# Resonata design is mobile-first

## The rule

Design for the narrowest viewport first. Every layout decision starts with "what does this look like on a 360px-wide phone?" Then progressively enhance for larger viewports if and only if the larger viewport unlocks new value.

**Default:** vertical, single-column, form-and-content-first.
**Not default:** desktop-first multi-column layouts that collapse on mobile.

## Specific applications

- **Landing hero:** the form (email + condition) must be the dominant above-the-fold element on mobile. No competing hero image, illustration, or sidebar should steal that real estate. Photos and brand visuals belong elsewhere — confirmation screens, marketing pages, or below the form on landing if they earn the space.
- **Multi-column layouts:** start as stacked single-column. Add `@media (min-width: ...)` to switch to columns only when the larger viewport meaningfully improves comprehension.
- **Type and spacing:** smaller by default on mobile, scale up at breakpoints — not the reverse.
- **Tap targets:** 44px minimum height for any interactive element. Buttons, choice cards, dropdowns.
- **Forms:** stack labels and inputs vertically. Don't put labels in line with inputs except in compact secondary forms.
- **Trust signals:** wrap and stack naturally on mobile. Don't try to fit four trust badges in a single row that breaks at 380px.

## Why

The patient audience often uses Resonata during symptom flares, fatigue periods, or moments of decision urgency. Many encounters happen on a phone — F train commute (Jasmine pattern), waiting room (Sarah pattern), tablet handed over by a caregiver (Dorothy pattern). A desktop-first design that "also works on mobile" is a design that doesn't actually work for the moment patients are in.

Resonata's brand voice emphasizes seriousness, respect, and meeting patients where they are. Where they are is on a phone.

## The triggering incident (2026-05-13)

The patient onboarding v2 prototype landing page had a hero with two columns at desktop — form on the left, hero photo on the right. On mobile, the photo stacked above the form and pushed it below the fold. Lauran caught it: "Also don't put the hero image of the patients there — design should be mobile-first."

The fix: removed the hero photo entirely from the landing, restructured the hero CSS to be single-column by default and only scale type/padding at `min-width: 720px`. The photo can live elsewhere if it earns the space (e.g., confirmation moments, marketing pages, semi-converted app home). It does not earn the hero.

## How to apply

**Before sketching any new screen:**
1. Open the design in a 360px viewport mentally first.
2. Ask: what's the single most important action above the fold? Make that thing dominant.
3. Anything that doesn't serve that action goes below the fold, in a separate screen, or gets deleted.

**Before adding any image to a patient-facing surface:**
1. Ask whether it adds value on mobile or just on desktop.
2. If only on desktop, ask whether it's worth the mobile-experience cost.
3. If it's a hero/landing image, the answer is almost always no — at least not above the form.

**Code patterns:**
- CSS: default to mobile styles in the unconditional block. Use `@media (min-width: X)` to add desktop enhancements, not `@media (max-width: X)` to remove desktop styles for mobile.
- Layouts: `display: flex; flex-direction: column;` by default. Switch to `flex-direction: row` or `display: grid; grid-template-columns: ...` only inside larger breakpoints.

---

*This file lives in the project folder pending lift to `~/Developer/resonata/context-library/_memory/` via Claude Code.*
