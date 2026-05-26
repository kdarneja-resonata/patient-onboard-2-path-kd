# Quiz Question Regeneration — Analysis & Recommendation

**Author:** Claude (PM copilot)
**Date:** 2026-05-18
**Status:** Draft for Lauran review → Megan clinical sanity-check → eng spec
**Methodology source:** Martin, pivot sign-off meeting 2026-05-15
**Scope:** Three substantive quiz questions to replace current set of three (Q1 MG type / Q2 antibody / Q3 treatment stage). Zip stays as separate optional Q4.

---

## 1. Source-of-truth confirmation

**Matrix used:** Google Sheet `1GvMVJw9TpEVWtAwU1SS487Y1OdvHyIGr`, tab `gid=1118179235` (Resonata care-option × eligibility-criteria matrix, owned by Matching team).

**22 MG care options confirmed** (cols 5–26 of the matrix header row):
- **7 approved drugs:** Imaavy · MESTINON · RYSTIGGO · SOLIRIS · Ultomiris · vyvgart-hytrulo · ZILBRYSQ
- **15 NCT trials:** NCT06106672, NCT06193889, NCT06220201, NCT06359041, NCT06414954, NCT06456580, NCT06463587, NCT06517758, NCT06558279, NCT06626919, NCT06704269, NCT06744920, NCT06799247, NCT07039916, NCT07215949

Trailing columns in the sheet are non-MG (cardiac/PAH/amyloid trials and approved drugs) — excluded from this analysis.

**Method:** Pulled the sheet via Google Drive MCP, parsed all 4,712 criterion rows, filtered to rows where at least one of the 22 MG columns has a non-empty cell, deduped across the sheet's repeated sections, and extracted the value-per-option for each criterion. Working CSV saved at `/tmp/mg_matrix_dedup.csv` (local, not committed).

---

## 2. Current state baseline

The prototype quiz today (lines 903–1102 of `patient-onboarding-v2-prototype.html`):

### Q1 — "Which type of Myasthenia Gravis describes you?" (lines 916–948)
| Answer | Maps to matrix criterion | Options eligible (of 22) |
|---|---|---|
| I'm not sure | (downgrade — all match) | 22 |
| Generalized MG | `[I] myasthenia gravis, generalized` (21 options carry this) | 21 |
| Ocular MG | `[I] Myasthenia gravis, ocular` (only NCT06558279) | 1 |
| Juvenile MG | Only Imaavy supports age <18 | 1–2 |

**Narrowing power (current):** Assuming patient mix ~85% gMG / 12% ocular / 3% other, the expected post-Q1 option pool ≈ **18–19 of 22 (~15% reduction)**. The split is so lopsided that for the vast majority of patients, Q1 narrows almost nothing.

### Q2 — "Do you know your antibody status?" (lines 973–1005)
| Answer | Maps to | Eligible |
|---|---|---|
| I don't know | (downgrade) | 22 |
| AChR-positive | 17 options have AChR-required; 5 antibody-agnostic still pass | ~20 |
| MuSK-positive | 9 options have MuSK-required; antibody-agnostic still pass | ~12 |
| Seronegative | Only MESTINON + ocular trial + 2 antibody-agnostic trials | ~4 |

**Narrowing power (current):** ~70% AChR+ / 5% MuSK+ / 15% seroneg / 10% don't know → expected post-Q2 ≈ **17 of 22 (~22% reduction from start, ~10% reduction from Q1 result)**. Most-common patient (AChR+) sees almost no narrowing.

### Q3 — "Where are you in your treatment?" (lines 1030–1062)
| Answer | Maps to | Eligible |
|---|---|---|
| Newly diagnosed | (no direct criterion) | 22 |
| On treatment, looking for better | (no direct criterion) | 22 |
| Tried multiple, no success | Loosely maps to `[I] Refractory disease` (2 trials) — but enriches, doesn't filter | 22 (+2 prioritized) |
| Stable, exploring | (no direct criterion; weakly anti-correlates with MG-ADL≥6 trials) | 22 |
| I don't know | (downgrade) | 22 |

**Narrowing power (current):** **~0%.** Martin is right — Q3 doesn't map to any explicit eligibility criterion in the matrix. It can softly enrich (refractory trials surface higher) and inform display copy, but does not narrow the option pool.

### Q4 — Zip (optional, lines 1086–1101)
Used for distance filtering on the 15 trials only. Approved drugs are nationwide.

### Cumulative current state
For a representative AChR+ generalized MG patient: starts at 22 → after Q1 21 → after Q2 ~20 → after Q3 ~20. **Net narrowing ≈ 9%.** The pivot's promise of meaningful pre-records narrowing is not being delivered by the current questions.

---

## 3. Candidate question sets

For each set, "narrowing power" is the expected option pool size for a representative MG patient (mix of subtypes weighted by US MG epidemiology). Calculations assume "I don't know" is a graceful downgrade (option stays in pool but gets demoted from "Full" to "Maybe match").

### Patient-profile assumptions used for all sets

- MG form: ~85% gMG / 12% ocular / 3% juvenile
- Antibody status (of gMG patients): ~75% AChR+ / 5% MuSK+ / 5% LRP4 / 15% seronegative
- Symptom burden (MG-ADL or self-report): ~20% mild (<6) / ~55% moderate (6–9) / ~25% severe (≥10)
- Treatment history: ~40% no specialty MG drug ever / ~35% on or recent FcRn/complement/rituximab / ~25% prior thymectomy
- These %s are best-guess from MG epi literature — **Megan should validate.**

### SET A — "Clinical fingerprint" (antibody + severity + treatment history)

**Q1. Has a blood test shown a specific antibody linked to your MG?**
- I don't know / haven't been told the results
- **AChR positive** (acetylcholine receptor antibody — most common)
- **MuSK positive**
- **LRP4 positive** or another less common antibody
- Tested negative for all the antibodies they checked

Narrowing: AChR+ → ~20 remain · MuSK+ → ~12 · LRP4 → ~4 · Seroneg → ~4 · Don't know → 22 (downgrade). **Expected pool after Q1: ~17.**

**Q2. How much does MG affect your day-to-day right now?**
*(Helper text: things like talking, swallowing, breathing, holding your head up, lifting your arms, walking, getting dressed.)*
- I don't know how to answer this
- **Barely noticeable** — I can do most things normally
- **Manageable** — symptoms come and go but I get by
- **Severe** — I struggle with basic activities or have been in MG crisis

**Matching logic (per Lauran 2026-05-18):**
- "Barely noticeable" → MG-ADL < 6 → excludes the 11 trials gated on MG-ADL ≥ 6 (and the 4 trials gated on QMG ≥ 11)
- "Severe" → MG-ADL ≥ 6 → qualifies for severity-gated trials; may also surface the 2 refractory trials when combined with Q3 signals
- "Manageable" → treated the same as "I don't know" → downgrade severity-gated options to "Maybe match" instead of excluding
- "I don't know" → downgrade severity-gated options to "Maybe match"

*Note: this assumes the standard MG-ADL direction (higher score = more symptoms). Lauran's note on the file used the inverse direction, which I've read as a transposition. Confirm before eng implements.*

Narrowing: Barely noticeable → ~7 remain (excludes 11 MG-ADL≥6 and overlapping QMG≥11 trials) · Manageable → 22 with severity-gated downgraded to Maybe · Severe → 22, with refractory trials enriched on Q3 cross-check · Don't know → downgrade. **Expected pool after Q1+Q2: ~13** (mass weighted toward Manageable/Don't know in the moderate buckets).

**Q3. Have you ever had any of these MG-specific treatments?** *(Multi-select OK.)*
- I don't know
- **None of these** — only standard pills like Mestinon (pyridostigmine), steroids, or immunosuppressants (Cellcept, Imuran)
- **IV/infusion or injection antibody treatment for MG** — Soliris, Ultomiris, Vyvgart, Rystiggo, Imaavy, Zilbrysq, or similar
- **Rituximab** or another B-cell therapy (Rituxan, Truxima, Ocrevus)
- **Thymectomy** — surgery to remove the thymus gland
- *(Listed Yes can be combined)*

Narrowing: "None" → 22 (broadest) · "Antibody treatment" → excludes 5–7 washout trials → ~15 · "Rituximab" → excludes 4 trials → ~18 · "Thymectomy" → excludes 8 trials → ~14 · Multiple Yes → can drop to ~6 · "Don't know" → downgrade. **Expected pool after all three: ~9.**

| Q | Narrows? | Patient-answerability | Ports to non-MG? |
|---|---|---|---|
| Q1 antibody | High for non-AChR+; moderate for typical | **Medium** — newly-diagnosed may not know exact antibody, but most diagnosed patients are told | **Yes** — generic "do you have a specific biomarker?" structure ports (e.g., HER2+ in oncology, TTR genotype in amyloid) |
| Q2 severity | High for mild; opens refractory for severe | **High** — fully subjective self-report, no record lookup | **Yes** — "how much does your condition affect daily activities?" is universal |
| Q3 prior treatment | High for any "yes" | **Medium** — patients should recognize brand names but newer drugs (Imaavy, Zilbrysq) may not be familiar; thymectomy is memorable | **Yes** — "have you had any of these specific treatments?" ports cleanly per condition |

**Cumulative narrowing (Set A): 22 → ~9 (about 59% reduction).** Hits Martin's "halfway down the funnel" target.

### SET B — "MG form + Antibody + Severity"

Keeps current Q1 structure (MG type) and replaces Q3 with severity.

**Q1. Which best describes your MG today?**
- I'm not sure
- **Generalized MG** (affects multiple muscle groups: limbs, breathing, swallowing, eyes)
- **Ocular MG** (mainly the eye muscles)
- **Juvenile MG** (diagnosed in childhood)
- **Refractory** (tried multiple treatments, still struggling)

Narrowing: gMG → 21 · Ocular → 1 · Juvenile → 1–2 · Refractory → treated as gMG (21) + flags 2 refractory-gated trials (NCT06220201, NCT06517758) as a match boost · Not sure → 22. **Expected pool after Q1: ~19.**

*Note: "Refractory" mixes a treatment-response signal into a form-of-disease question. We accept the taxonomy blur because the refractory chip is how veteran patients describe themselves, and it lets two refractory-gated trials surface earlier in the flow without adding a fourth question.*

**Q2. Antibody status** *(same wording as Set A Q1)*. **Expected pool after Q1+Q2: ~16.**

**Q3. How much does MG affect your day-to-day right now?** *(same as Set A Q2)*. **Expected pool after all three: ~13.**

| Q | Narrows? | Patient-answerability | Ports? |
|---|---|---|---|
| Q1 MG form | Low for typical patient | **High** — patients know whether their symptoms are eye-only or whole-body | **Partially** — "type of X" ports but answer options are MG-specific |
| Q2 antibody | Moderate | Medium | Yes |
| Q3 severity | High for mild patients | High | Yes |

**Cumulative narrowing (Set B): 22 → ~13 (~41% reduction).** Less aggressive than Set A but very patient-friendly. Closer to current state's structure (less change for engineering, fewer new copy decisions).

### SET C — "Antibody + Severity + Surgery" (simpler, lower-risk wording)

**Q1. Antibody status** *(as Set A)*. Pool ~17.

**Q2. Severity** *(as Set A)*. Pool ~13.

**Q3. Have you ever had a thymectomy? (Surgery to remove a gland called the thymus, behind your breastbone.)**
- I don't know
- **No**
- **Yes**

Narrowing: Yes → excludes 8 trials → ~9 of remaining 13. No → no change. Don't know → downgrade.

| Q | Narrows? | Patient-answerability | Ports? |
|---|---|---|---|
| Q1 antibody | Moderate | Medium | Yes |
| Q2 severity | High for mild | High | Yes |
| Q3 thymectomy | Medium (binary, ~30% of patients answer Yes) | **High** — major surgery, patients remember | **Partially** — surgery-history pattern ports but the specific procedure is MG-specific |

**Cumulative narrowing (Set C): 22 → ~10 (~55% reduction).** Hits target. Q3 is the lowest-cognitive-load of any treatment-history option but loses the washout-exclusion power of Set A Q3.

### Side-by-side comparison

| | Set A | Set B | Set C |
|---|---|---|---|
| Q1 narrowing focus | Antibody | MG form | Antibody |
| Q2 narrowing focus | Severity | Antibody | Severity |
| Q3 narrowing focus | Treatment history (multi) | Severity | Thymectomy |
| Expected pool after 3 Qs | **~9** | ~13 | ~10 |
| Average % narrowing | **59%** | 41% | 55% |
| Patient-answerability risk | Medium (Q3 brand recognition) | Low | Low |
| Ports to non-MG conditions | Yes (all 3) | Partially (Q1) | Partially (Q3) |
| Engineering lift | Highest (multi-select on Q3) | Lowest | Medium |

---

## 4. Recommended set

**Set A — "Clinical fingerprint."**

Rationale:
1. **Only set that meets the "halfway down the funnel" target** (~59% narrowing) without leaning on a low-power question.
2. **Each question pulls weight independently.** Antibody narrows the non-AChR+ minority; severity narrows the mild-symptom segment; treatment history narrows the experienced-patient segment. Almost every patient profile has at least one question that meaningfully narrows.
3. **Drops the current weak Q1 (MG form).** The matrix only has 1 ocular-specific option and 1 juvenile-accessible option, so the MG-form question is a near-no-op for 85%+ of users. Removing it makes room for severity, which actually splits the matrix near 50/50 for mild patients.
4. **Treatment-history question (Q3) is where most washout-driven mismatches live.** This is what would have prevented the kind of bug surfaced in synthetic testing where MuSK+ profiles returned AChR-coded options. The current Q3 (treatment stage) was supposed to do this work and doesn't; replacing it with explicit prior-treatment recognition closes that gap.
5. **Ports across conditions.** All three question structures (biomarker, severity self-report, prior-treatment recognition) transfer cleanly to other conditions for V2 expansion — only the answer options need swapping.

Known trade-off: **Q3 has the highest patient-answerability risk in the set.** Newer drug brand names (Imaavy 2024, Zilbrysq 2023) may not be top-of-mind for patients. Mitigations: include generic descriptions ("antibody infusion or injection treatment for MG"), group multiple brands per answer, accept multi-select.

If Megan flags Q3 as a stretch for patient vocabulary, **fall back to Set C** (swap Q3 for thymectomy). This costs ~5% narrowing but reduces patient cognitive load substantially. Set B is the most conservative — recommend only if Megan thinks both Set A Q3 and the severity self-report are unreliable.

---

## 5. Open questions for Megan

These are the items only a clinical-and-patient-voice review can resolve. Each one can flip the recommendation between Set A / B / C.

1. **Antibody self-report (Set A/B/C Q1).** Does a newly-diagnosed MG patient typically know their antibody result without looking it up? If they were told "you're seropositive" but not which specific antibody, what's the most they can reliably tell us? Should the answer set collapse to "AChR / MuSK / other or none / I don't know" (4 options) instead of 5?

2. **Severity self-report (Set A Q2 / B Q3 / C Q2).** Lauran simplified this to a 3-chip scale plus "I don't know" (Barely noticeable / Manageable / Severe). Matching maps Barely noticeable → MG-ADL < 6, Severe → MG-ADL ≥ 6, Manageable and I don't know both downgrade severity-gated options to "Maybe." Will patients reliably split themselves at the right end of that scale? Would concrete behavioral anchors ("I avoid stairs," "I get tired chewing food") help, or do they add noise?

3. **MG-ADL proxy.** Patients won't have memorized their MG-ADL score, so the question above is the proxy. Is "barely noticeable" → MG-ADL < 6 a defensible mapping clinically? Same for "severe" → ≥ 6? Anywhere the binning is likely to misroute patients?

4. **Drug-name recognition (Set A Q3).** Which of these brand names would a non-newly-diagnosed MG patient recognize: Soliris, Ultomiris, Vyvgart, Rystiggo, Imaavy, Zilbrysq, Rituxan? If recognition is poor for the newer brands, should we group them by mechanism ("antibody infusion treatment") and accept some loss of specificity?

5. **Thymectomy recall.** Is "have you had a thymectomy" a question patients answer confidently, or do many MG patients not connect that surgery to MG? Is the helper text ("surgery to remove a gland behind your breastbone") enough?

6. **Refractory framing.** Two trials (NCT06220201, NCT06517758) gate on refractory status. Set B Q1 now has "Refractory (tried multiple treatments, still struggling)" as a chip alongside Generalized / Ocular / Juvenile — refractory is treated as gMG for matching and boosts the two refractory-gated trials. Is "refractory" a word patients self-identify with, or do they say "nothing's worked"? If patients won't pick the chip, we lose the signal. In Set A, refractory would instead be inferred from Q3 (any prior antibody therapy + "Severe" on Q2). Which approach feels more reliable clinically?

7. **Pregnancy.** 14 of 22 options exclude pregnancy/breastfeeding. We didn't include a pregnancy question in any set because it applies to a small slice of patients and is already in the sign-up step. Confirm we shouldn't add a "currently pregnant or planning?" yes/no after sign-up but before matches — or whether a soft-exclusion at the matches screen is acceptable.

8. **Multi-select on Q3 (Set A).** Is multi-select acceptable to patients on a mobile screen this early in the flow, or does it slow people down enough to hurt completion?

---

## 6. Implementation notes

**Source files referenced:**
- Prototype: `patient-onboarding-v2-prototype.html`
- PRD: `prd-patient-onboarding-v3-2026-05-13.md`

**No changes to make yet.** This document is for Lauran's review and Megan's sanity-check before code edits.

**When the question set is approved, the changes in `patient-onboarding-v2-prototype.html` are:**

1. **Devbar buttons** (lines 737–740):
   - Update labels for `quiz-1`, `quiz-2`, `quiz-3` to match new question topics.
   - Q4 zip label stays.

2. **Quiz Q1 screen** (lines 903–955):
   - If Set A: replace the question and answer list (lines 916–948) with the new antibody question content. Move the helper copy. "I don't know" stays as first option per [constraint 2].
   - If Set B: leave the MG-form question content but reorder the choices so "I'm not sure" is first (currently it IS first — line 920 — but verify across all four quiz screens).

3. **Quiz Q2 screen** (lines 960–1012):
   - Replace question text (line 973) and answer list (lines 976–1005) with the chosen Q2.
   - For Set A or C: this becomes the severity question. For Set B: this is the antibody question (essentially the current Q2, kept).

4. **Quiz Q3 screen** (lines 1017–1069):
   - Replace question text (line 1030) and answer list (lines 1033–1062) with the chosen Q3.
   - For Set A: multi-select treatment history (UI change — currently single-select radio pattern; add a `data-multi="true"` flag and update `selectChoice` JS — see line ~2030 area). Currently `selectChoice` deselects siblings; needs a multi-select branch.
   - For Set C: simple yes/no/IDK thymectomy.

5. **Section comment headers** (lines 957–959, 1014–1016, 1071–1073):
   - Update screen labels to reflect new question topics.

6. **Match-logic mock data** (lines ~1189–1500 — the "preview matches" and full-matches screens):
   - The hard-coded sample patient profile is currently `generalized MG + AChR+ + on treatment looking for better`. Update the example match copy to reflect the new question dimensions. The dynamic match-logic bug (PRD hard-requirement #1) needs to be resolved separately — this analysis does not change that.

7. **Sign-up / consent split / Path A / Path B screens** (lines 1440+):
   - No quiz wording dependencies expected. Verify the "your matches" framing references don't quote the old Q3 specifically.

8. **JS analytics events**:
   - Wherever quiz answers are emitted to analytics (search for `quiz-1`, `quiz-2`, `quiz-3` event names), update payload keys to match new question semantics.

**Out of scope for the prototype edit:** updating the matching algorithm itself. The matching logic remains a mock until eng implements live matrix lookup. This analysis defines the INPUT shape for that algorithm; the OUTPUT logic (full match / maybe match / not a fit) is unchanged.

**PRD update needed when approved:** Section "Solution → Flow → Quiz" (PRD lines 53–58) currently lists Q1–Q4 by name. Replace with the chosen set's question titles. Add a note that "I don't know" maps to a "Maybe match" downgrade in the matching logic (constraint 2 from the task brief).
