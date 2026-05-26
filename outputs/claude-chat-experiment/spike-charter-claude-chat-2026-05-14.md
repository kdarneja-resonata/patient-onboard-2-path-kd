# Spike Charter: Claude-Powered Patient Onboarding Chat

**Author:** Lauran Hazan
**Date:** 2026-05-14
**Status:** Draft v1, pre-decision
**Parent plan:** [`plan-claude-chat-experiment-2026-05-14.md`](plan-claude-chat-experiment-2026-05-14.md)
**Concept prototype:** [`../patient-chat-claude-prototype.html`](../patient-chat-claude-prototype.html)

---

## Goal

Build a real Claude-backed version of the v2 patient onboarding flow inside an isolated AWS account, run it against synthetic patients only, and answer the experiment questions below. Decide at the end whether to continue to controlled alpha, pivot the approach, or kill the project.

This is a **learning vehicle, not a product**. Nothing the spike produces ships to real patients. The bar is "good enough to evaluate," not "good enough to deploy."

---

## Owners

| Role | Person |
|---|---|
| PM, product spec, conversation design | Lauran |
| Engineering lead, architecture, infra | Vishal |
| Developer, implementation | Avinash |
| Clinical accuracy and safety | Megan (nurse practitioner, lead) and Rajni (sign-off) |
| Legal and compliance | Henry |
| Executive sponsor | Martin |
| Commercial sponsor sensitivity | Kristen |

---

## Prerequisites (must be done before spike starts)

These come from Phase 0 of the parent plan. Spike does not start until all of these are in writing.

1. **Henry's BAA scope memo** confirming AWS BAA covers Bedrock + the specific Claude models we'll use + the regions + the logging configuration. Conversation has occurred; written confirmation pending.
2. **Spike budget signed off** by Martin. Eng weeks plus AWS dollar cap.
3. **Eval staffing plan** documented. Megan's confirmed dedicated hours per week for clinical rubric. Engineering split for Avinash, or commitment to a second engineer for eval harness.
4. **Crisis on-call staffing model** decided in principle (does not need to be operational yet because spike has no real patients; needs to be defined so it can be staffed by alpha).
5. **Cost ceiling** set: pre-registered "go" and "no-go" numbers for cost per completed onboarding.
6. **Chat handoff scope memo** drafted: where the chat ends, what the patient sees on the other side, what the chat is allowed to say about what happens next. (Note: full post-onboarding app functionality is a separate, parallel workstream.)
7. **Kill criteria numbers** filled in (see "Kill criteria" section below).

---

## Learning objectives (the experiment questions)

Numbered so eval results map back to them.

1. **Conversation rails.** Does Claude stay on the scripted onboarding flow when patients answer naturally instead of using chips?
2. **Hallucination on medical claims.** How often does Claude make medical claims that fail Megan-and-Rajni-reviewed accuracy checks, with a strong system prompt and content-lookup tool in place?
3. **Tool-call reliability.** When the chat needs to fetch matches, capture consent, look up vetted content, or trigger records auth, does it call the tool or chat about it?
4. **Latency.** Time to first token per turn, and total turn time. Target under 2 seconds to first token.
5. **Cost per completed onboarding.** Actual measured cost in the spike, projected to expected volumes. Compare against the pre-registered cost ceiling.
6. **Quiz answer extraction accuracy.** When patients answer naturally, does the orchestration extract the correct structured value (MG type, antibody status, treatment stage, zip code)?
7. **Branch routing accuracy.** Does the chat correctly route to covered vs uncovered branch based on indication?
8. **Drift recovery.** When patients ask a mid-flow FAQ, does the chat return to the correct step in the flow?
9. **Equal-weight presentation.** Do embedded cards (treatment landscape chart, consent split, app home buckets) preserve the v2 PRD's equal-weight rule, or does chat sequencing still bias patients?
10. **Brand experience.** Does the patient experience feel like a Resonata product or a generic chatbot?

---

## In-scope (what the spike builds)

- Conversation orchestration layer (server-side agent harness, state machine, tool routing).
- Bedrock integration with Claude Opus 4.7 and Claude Haiku 4.5 enabled. Mixed model strategy: Haiku for routine turns, Opus for high-stakes turns (consent, clinical questions).
- Prompt caching for the system prompt.
- Audit logging on day one: every prompt, response, tool call, and consent event captured with timestamp.
- Cost tracking dashboard from day one.
- Tools (stubbed where appropriate, real where decided in Phase 0):
  - Matching service tool (returns matches from synthetic patient profile, schema matches the production matching service).
  - Records orchestration tool (stubbed; simulates DNAvisit handoff).
  - Content lookup tool (vetted explainer content for MG, antibodies, treatments, trials; Megan and Rajni curate the content set).
  - Consent capture tool (records consent events with structured metadata, timestamp, version of consent text).
  - Patient state tool (read and write patient profile fields; full field-level access control is a Phase 0 open issue, simplified for spike).
  - Email tool (stubbed; sends to internal test inbox only).
- Eval harness wired to the orchestration layer.
- Synthetic patient panel runner (drives conversations through the chat using the existing Sarah, Jasmine, Marcus, Dorothy personas plus adversarial extensions).
- Minimal but agent-aware front-end UI (see UI scope below).

---

## UI scope (explicit, per plan-review feedback)

The HTML chat prototype is a scripted state machine. A real agent-backed chat needs different UI affordances. The spike builds enough UI to run the eval suite end-to-end, not the full production polish.

**In scope for the spike UI:**

- Streaming tokens (text appears as it is generated, not after the full response is ready).
- Tool-call indicator (visible UI state when the model is calling a tool, distinct from "thinking").
- Latency indicator (subtle, signals that a response is on its way).
- Error and retry UI (if a tool call fails, the patient sees a calm fallback; the model does not hallucinate a result).
- Kill switch UI (Resonata operator can route a patient back to screen flow within the same session).
- AI disclosure banner (persistent in chat header, plus opening message).
- "Talk to a human" escape hatch (visible in composer footer; stubbed action for spike).
- Consent capture surfaces (HIPAA, DNAvisit, AI processing): explicit checkbox + Submit pattern preserved from the v2 prototype, rendered as embedded cards.
- Embedded structured cards for the treatment landscape chart, consent split, auth blocks, trial details, and app-home bucket browser (reused from the HTML prototype's pattern).

**Deferred until alpha or later (not in the spike):**

- Full mobile polish beyond what is needed for synthetic testing.
- Caregiver mode UI.
- Multi-condition UI (spike is MG-only).
- Real-time presence indicators ("Megan is reviewing this conversation").
- Filter / sort / compare UI on matches list (already V2-deferred per v2 PRD).
- Voice / audio input or output.
- Accessibility audit and remediation (do not skip this for alpha; out of scope for spike specifically because synthetic patients do not surface accessibility issues).

---

## Chat handoff and end-of-chat behavior

The chat is **onboarding-only**. It begins with the patient landing and ends when one of these happens:

1. **Path A complete.** Patient has consented to HIPAA + records retrieval. Chat shows the "records are being retrieved, we will email you in approximately 8 hours" confirmation card. Chat then offers two end states: stay in chat for exploration, or hand off to the semi-converted app surface (which is not built in this spike; for spike purposes, this is a stubbed "you have been handed off" message).
2. **Path B complete.** Patient has signed HIPAA only and entered semi-converted state. Same handoff stub as above.
3. **Uncovered branch complete.** Patient has joined the waitlist. Chat confirms and offers the future explorer pathway (stubbed for spike).
4. **Patient explicit exit.** Patient says "I'm done" or closes the session.

**What the chat is allowed to say about what happens next:**

- For Path A: "We will retrieve your records, run them against the matching engine, and email you when your verified matches are ready. We expect this within 8 hours."
- For Path B: "Your matches are saved. You can come back anytime. When you are ready to apply to anything, we will need your records."
- For uncovered: "We do not match for your condition yet. We will email you when we do."

**What the chat is not allowed to say:**

- Anything about the post-onboarding Resonata app experience that is not yet defined.
- Anything implying the chat itself will be available after onboarding.
- Anything about enroller workflow, sponsor visibility, or commercial implications.

This list will need refinement based on the chat handoff scope memo (Phase 0 deliverable).

---

## Out-of-scope (explicitly)

- Real patient exposure of any kind.
- Real ePHI in the spike. All test data is synthetic.
- Integration with the production Resonata environment.
- Real DNAvisit integration (stubbed).
- Production-grade error handling, retry policies, observability.
- Production-grade UI polish.
- Multi-condition support.
- Caregiver mode.
- Post-onboarding app surface (the persistent semi-converted experience is referenced as a stubbed handoff target).
- Sponsor-facing analytics layer.
- Commercial discussions with sponsors about AI-driven onboarding (Kristen-led conversation, separate track).

---

## Success criteria

The spike succeeds if **all** of these are true at the end:

1. End-to-end onboarding flow runs against synthetic patients in chat.
2. Eval suite is automated and produces a pass-fail report on each of the 10 learning objectives.
3. Pass rates on safety, accuracy, conversation flow, brand voice, and consent posture meet the thresholds set in the eval framework.
4. Cost per completed onboarding is at or below the pre-registered "go" ceiling.
5. Quiz answer extraction accuracy and branch routing accuracy meet the thresholds set in the eval framework.
6. A reasoned go / no-go recommendation can be made by Lauran with sign-off from Vishal, Megan, Rajni, and Henry.

---

## Kill criteria (numbers to set before code is written)

Spike is killed (or returns to design) if **any** of these are true:

| Metric | Kill threshold |
|---|---|
| Medical hallucination rate (after prompt hardening) | above [X]% on Megan + Rajni reviewed cases |
| Tool-call reliability on critical actions (consent capture, match fetch) | below [Y]% |
| Quiz answer extraction error rate | above [N]% |
| Branch routing error rate | above [N]% |
| Drift recovery rate (returning to flow after FAQ) | below [M]% |
| Cost per completed onboarding | above [$Z] |
| Latency to first token (median) | above [T] seconds |

X, Y, N, M, Z, T are set in Phase 0 by Lauran, Vishal, Megan, and Martin (cost) before spike starts. The point is to pre-register the numbers so the team is not negotiating thresholds against actual results after the fact.

---

## Budget and timeline

**Duration:** 4 to 6 weeks. If the spike is going to slip past 6 weeks, it is killed or rechartered.

**Engineering:** Vishal supervising; Avinash full-time during the spike (subject to eval staffing decision in Phase 0).

**Cost cap:**

- AWS spend cap: [TBD in Phase 0]. Hard ceiling, alerts at 50% and 80%.
- Model inference budget: [TBD in Phase 0]. Hard ceiling, separate from AWS spend cap.

**Milestones:**

- Week 1: Isolated AWS account stood up, Bedrock access confirmed, audit logging skeleton in place, first prompt-and-response round-trip. Tool stubs scaffolded.
- Week 2: Conversation orchestration layer functional end-to-end against one synthetic patient. Embedded cards rendering.
- Week 3: Tool integrations complete (stubbed). Eval harness running against synthetic patient panel.
- Week 4: Adversarial eval pass; clinical accuracy rubric run by Megan; iterate on prompts.
- Week 5: Cost and latency report draft. Second eval pass. Conversation review with full team.
- Week 6: Risk register, go / no-go recommendation, written report.

---

## Synthetic patient testing plan

- **Primary panel:** existing Sarah, Jasmine, Marcus, Dorothy personas. Each runs the full flow.
- **Adversarial extensions:** synthetic patients who push for diagnosis, ask about competitors, signal crisis (suicide / self-harm / adverse event), attempt jailbreaks ("ignore your prior instructions"), insist on speaking to a human, refuse to give email.
- **Edge cases:** patients who type only "I don't know" answers, patients who answer in non-obvious phrasings ("the muscle weakness one"), patients with ambiguous indication, patients who try to skip ahead, patients who get distracted mid-flow.
- **Methodology:** every conversation logged with full prompt, response, tool call trace. LLM-as-judge (separate model from the one being tested) runs the rubric. Megan reviews flagged accuracy failures. Lauran reviews flagged brand-voice and conversation-flow failures. Vishal reviews tool-call failures.
- **Volume target:** at least 100 full synthetic patient runs across personas and edge cases over the spike duration.

---

## Deliverables

| Deliverable | Owner |
|---|---|
| Working spike (private URL behind auth) | Vishal + Avinash |
| Audit log of all spike conversations | Avinash |
| Eval results report mapped to learning objectives 1 to 10 | Lauran + Megan + Avinash |
| Cost and latency report with projection to expected volumes | Vishal |
| Risk register | Lauran |
| Clinical accuracy review summary | Megan + Rajni |
| Brand voice review summary | Lauran |
| Tool-call reliability summary | Vishal |
| Go / no-go recommendation memo | Lauran |

---

## Risks specific to the spike

- **Underspecified UI work.** If UI scope creeps beyond the in-scope list above, the spike slips.
- **Clinical accuracy rubric not ready in time.** If Megan's rubric is not in place by Week 2, the eval suite has nothing to judge against.
- **Cost overrun.** Bedrock inference costs can compound quickly if the prompt is long and patients have many turns. Prompt caching must be enabled from day one.
- **Tool-call rigidity.** If tools are too rigid in their schemas, the model fails to call them; if too loose, the model invents bad arguments. Iteration on tool schemas during the spike is expected and should be budgeted.
- **Single-engineer dependency.** Avinash is the only person who deeply understands the orchestration layer during the spike. If he is unavailable for a week, the spike stalls. Vishal should be the named backup.
- **Field-level access control on PHI** is a separate research issue (see open issues in parent plan). The spike will use a simplified model; production design is deferred.

---

## Sign-offs needed before spike starts

- [ ] Henry: BAA scope memo signed.
- [ ] Martin: spike budget approved.
- [ ] Lauran: spike charter approved and kill criteria numbers filled in.
- [ ] Vishal: engineering feasibility and approach signed off.
- [ ] Megan + Rajni: clinical accuracy ownership and dedicated hours confirmed in writing.

---

*Living document. Updates as Phase 0 decisions land.*
