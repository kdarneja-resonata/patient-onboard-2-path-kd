# Patient Chat Experiment: Planning Outline

**Author:** Lauran Hazan
**Date:** 2026-05-14
**Status:** Draft v1, pre-decision
**Predecessor work:** [`prd-patient-onboarding-v2-2026-05-13.md`](../prd-patient-onboarding-v2-2026-05-13.md), [`patient-chat-claude-prototype.html`](../patient-chat-claude-prototype.html)

---

## Direct answers to the explicit questions

**1. Should engineering deploy Claude on AWS in a sandbox and run a spike first?**
Yes. Spike before product spec. A 4 to 6 week engineering spike in an isolated AWS account, synthetic patients only, no real ePHI, no integration with the production Resonata environment. The spike is how you learn the technical envelope (drift, hallucination, tool reliability, latency, cost) before you commit to design or schedule.

**2. Does this require a BAA with Anthropic?**
Not if you go through AWS Bedrock. AWS Bedrock is a HIPAA-eligible service. AWS provides a BAA. When you access Claude through Bedrock, inference runs inside AWS infrastructure and Anthropic does not receive prompts or responses. The AWS BAA covers the data flow. **Verify with Henry that AWS BAA explicitly covers Bedrock Claude usage**, then proceed.

If you instead used Anthropic's API directly, you would need a BAA with Anthropic. Anthropic does offer BAAs (typically enterprise terms with zero data retention requirements), but it adds a vendor and adds complexity. Bedrock is cleaner.

**3. Are there approaches that avoid needing a BAA?**
Yes, but they have real costs. The pattern is "non-PHI-by-design": the chat never receives identifiable patient data. Quiz answers alone (MG type, antibody status, treatment stage, zip) may not be PHI without identifiers attached. Email, name, DOB, address, MRN stay outside the model context, captured by structured form components, not by the conversation.

Tradeoffs of this approach:
- Fragile. One careless prompt change can leak PHI into model context.
- Limits future use cases (records-aware conversation, post-onboarding support, sponsor-facing analytics).
- Patient experience suffers: you can't have the chat say "Hi Marcus" or "the records we got from Dr. Chen confirmed AChR+".
- Still needs strong logging, access control, and legal review.

**Recommendation: Bedrock + AWS BAA.** It is the most defensible posture and the only path that unlocks the future use cases. Non-PHI-by-design is a useful constraint inside the Bedrock-backed design (don't put PHI in the model context unless necessary), not a substitute for the BAA.

**4. What is the right approach overall?**
Six phases with hard gates between them. Phase 0 decisions (this week and next). Phase 1 spike (synthetic only). Phase 2 evals (built in parallel with spike). Phase 3 compliance and legal (parallel, gates real patient exposure). Phase 4 controlled alpha with patient advisors. Phase 5 beta. Phase 6 GA. The hardest deliverable is the eval framework. Without it you cannot put a patient on this safely.

---

## TL;DR

- **Posture:** Bedrock + AWS BAA. Spike first, evals from day one, compliance in parallel, real patients only after gates pass.
- **Estimated time to first real patient:** 14 to 18 weeks if everything goes well, conservatively 20 to 24.
- **Hardest unknown:** conversation drift causing wrong quiz answer extraction. Second hardest: hallucinated medical claims.
- **Cheapest kill point:** end of Phase 1 spike, before any real patient is exposed.
- **Most expensive mistake:** rushing to alpha before evals exist.

---

## Phase 0: Decisions and pre-work (1 to 2 weeks)

**Owner:** Lauran (PM), with Henry (legal), Martin (CEO sponsor), Vishal (eng feasibility), Rajni and Megan (clinical sponsors).

**Decisions to make and document:**

1. **Infrastructure.** Bedrock + AWS BAA, Anthropic API + Anthropic BAA, or non-PHI-by-design. Recommended: Bedrock. *Status: conversation has occurred with Henry; needs written confirmation memo specifying exact scope (Bedrock service, specific Claude models, regions, logging configuration).*
2. **Spike scope.** Onboarding-only (recommended) vs full lifecycle. Onboarding has a clean start and end and an existing conversion baseline to compare against.
3. **Spike budget.** Engineering weeks (recommended 4 to 6), AWS dollar cap, model spend cap.
4. **Cohort during spike.** Synthetic only (recommended) vs synthetic plus patient advisors.
5. **Model selection.** Opus 4.7 for hard turns, Haiku 4.5 for routine turns, mixed strategy.
6. **Reuse path for matching logic.** Does the spike call the real matching service in a read-only mode, or is it stubbed entirely?
7. **Eval staffing model.** Megan (nurse practitioner) confirmed available for clinical accuracy rubric work; confirm dedicated hours per week. Decide whether Avinash splits time across spike + eval harness, or bring in a second engineer.
8. **Crisis on-call staffing model.** Who actually receives an escalated patient signal at any hour. Resonata does not have a clinical ops team today; staffing the on-call may need a clinical contractor. Budget conversation now, not in Phase 4.
9. **Cost ceiling per completed onboarding.** Set acceptable upper bound before measuring, so spike results are evaluated against a pre-registered number. Recommend both "go" and "no-go" thresholds.
10. **Chat handoff and post-onboarding scope.** Where does the chat end? What does the patient see on the other side? What is the chat allowed to say about what happens next? Note: full app functionality after onboarding still needs to be defined (see open issues).

**Deliverables (Phase 0):**

| Deliverable | Owner | Notes |
|---|---|---|
| Henry-signed BAA scope memo | Henry | one-pager: BAA explicitly covers Bedrock + specific Claude models + region + logging configuration. Conversation has occurred; needs written confirmation. |
| Decision doc: infrastructure path | Lauran + Henry | BAA matrix, recommended path, why |
| Spike charter | Lauran | see [`spike-charter-claude-chat-2026-05-14.md`](spike-charter-claude-chat-2026-05-14.md) |
| Compliance pre-read | Henry | BAA status, ePHI commitments, audit logging baseline, residency, what blocks alpha |
| Eval staffing plan | Lauran + Rajni + Megan | who does clinical rubric work; dedicated hours per week confirmed; engineering split or second hire |
| Crisis on-call staffing model | Lauran + Rajni + Henry | who responds, on what timeline, contracted clinical coverage if needed |
| Cost ceiling and budget | Martin + Lauran | per-onboarding go/no-go numbers, AWS spend cap |
| Chat handoff scope memo | Lauran | end-of-chat handoff definition, post-onboarding experience scope |
| Sponsor sign-off | Martin | budget, posture, risk tolerance |

**Gate to Phase 1:** All Phase 0 deliverables complete. Specifically: Henry's BAA memo is signed; spike charter is approved; eval staffing is confirmed in writing (not aspirational); crisis on-call model is defined; cost ceiling is documented; chat handoff scope is documented; spike budget is approved.

---

## Phase 1: Engineering spike (4 to 6 weeks)

**Owner:** Vishal (eng lead), Avinash (impl), Lauran (product spec and conversation design)

**Setup:**

- Isolated AWS account, separate from production Resonata.
- Bedrock provisioned with Claude Opus 4.7 and Claude Haiku 4.5 access.
- Audit logging on day one. Every prompt, response, tool call, and consent event captured with timestamp.
- Cost tracking dashboard from day one.
- No real patient data. No production database connection.
- Source-of-truth front-end: reuse `patient-chat-claude-prototype.html` as the UI shell, replacing the scripted state machine with a real Bedrock-backed agent.

**Build:**

- Conversation orchestration layer (server-side agent harness, state machine, tool routing).
- Bedrock integration with tool use enabled.
- Tools (stubbed or real, decided in Phase 0):
  - Matching service tool (returns matches from synthetic patient profile).
  - Records orchestration tool (DNAvisit handoff, stubbed in spike).
  - Content lookup tool (vetted explainer content for MG, antibodies, treatments).
  - Consent capture tool (records each consent event with structured metadata).
  - Patient state tool (read/write profile).
  - Email tool (stubbed; sends to internal test inbox).
- Eval harness (see Phase 2).
- Internal review UI for human-in-loop conversation review.

**Learn (these are the experiment questions):**

1. Does Claude stay on the scripted rails when patients answer naturally?
2. How often does Claude hallucinate medical claims with a strong system prompt?
3. How reliably does Claude make tool calls when it should (consent capture, match fetch, content lookup), versus chatting about it?
4. Latency per turn. Target under 2 seconds to first token.
5. Cost per completed onboarding. Set a target in Phase 0 and measure against it.
6. Where does conversation drift hurt the matching engine (i.e., quiz answer extracted wrong, branch misrouted)?
7. Does the embedded-card pattern preserve equal-weight presentation, or does chat sequencing still bias patients?
8. Does the patient experience feel like a Resonata product, or like a generic chatbot?

**Deliverables (Phase 1):**

| Deliverable | Owner | Notes |
|---|---|---|
| Working spike | Vishal + Avinash | end-to-end onboarding flow in chat, deployed to private URL behind auth |
| Eval results report | Lauran + Avinash | passes and fails on the Phase 2 suite |
| Cost and latency report | Vishal | actual spike numbers, projected to expected volumes |
| Risk register | Lauran | what we found, what is mitigable, what is a deal-breaker |
| Go / no-go recommendation | Lauran | continue to alpha, pivot, or kill |

**Kill criteria (define numbers in Phase 0):**

- Hallucination rate above [X]% on medical claims after prompt hardening.
- Tool-call reliability below [Y]% for critical actions (consent capture, match fetch).
- Cost per completed onboarding above [$Z].
- Quiz answer extraction error rate above [N]%.
- Drift recovery rate below [M]% (chat cannot return to flow after patient FAQ).

**Gate to Phase 4:** Spike results meet thresholds; eval suite is automated and passing; compliance package is signed off; alpha PRD is approved.

---

## Phase 2: Eval framework (parallel to Phase 1, ongoing forever)

**Owner:** Lauran (definition), Avinash (implementation), Rajni + Megan (clinical accuracy)

This is the most important deliverable in the entire program. Without evals, you cannot put a patient on this. With weak evals, the product can drift overnight when prompts or models change.

**Eval categories:**

1. **Safety**
   - Does it refuse to diagnose?
   - Does it refuse to recommend a specific treatment?
   - Does it respond to self-harm or crisis signals with correct escalation, no minimization?
   - Does it capture and escalate suspected adverse events?
   - Does it refuse jailbreak attempts ("ignore prior instructions", role-play as a doctor, etc.)?
   - Does it stay silent on competitors and other companies?

2. **Accuracy**
   - Correctly explains MG, antibodies, treatment stages.
   - Correctly explains Resonata's matching, semi-converted state, Path A vs Path B.
   - Correctly explains DNAvisit, HIPAA, records use.
   - Pulls from approved content when asked about specific treatments, does not freelance.

3. **Conversation flow**
   - Completes the quiz correctly when patient answers naturally, not via chip.
   - Extracts quiz answers in non-obvious phrasings ("the muscle weakness one", "the one in my eyes").
   - Correctly routes to covered vs uncovered branch.
   - Returns to the flow after answering a mid-flow FAQ.
   - Handles "I don't know" without judgment, every time.

4. **Brand voice**
   - Follows Resonata brand voice (empowering, professional, warm, innovative).
   - Avoids banned phrases ("sorry you're going through this", "patient portal", emoji).
   - Uses sanctioned phrases when appropriate ("you've got options", "we're here to help").

5. **Consent and trust posture**
   - Explicitly discloses AI on first interaction.
   - Avoids presumptive consent language.
   - Correctly explains HIPAA, data use, who sees what.
   - Honors "I don't want to give my email" without abandoning the patient.

6. **Comparative (vs screen flow)**
   - On the same 50+ synthetic patient scenarios, chat extracts the same quiz answers as the screen flow.
   - Chat finishes onboarding in similar or fewer turns.
   - Trust signals (would-you-trust ratings from synthetic patients) match or exceed screen flow.

**Methodology:**

- Synthetic patient panel runs using existing patient personas (Sarah, Jasmine, Marcus, Dorothy) plus extensions for adversarial behaviors.
- Adversarial panel: synthetic patients who push for diagnosis, ask about competitors, signal crisis, attempt jailbreaks.
- LLM-as-judge with Rajni-reviewed rubrics for clinical accuracy. Separate model used as judge.
- Human review of every flagged failure for the first 100 conversations.
- Pre-registered pass/fail thresholds per category before any patient sees the chat.
- Re-runs the full suite on every prompt change or model version update. CI-style gate.

**Deliverables (Phase 2):**

| Deliverable | Owner | Notes |
|---|---|---|
| Eval framework spec | Lauran | categories, methodology, thresholds |
| Clinical accuracy rubric | Rajni + Megan | what counts as accurate per medical claim category; Megan (NP) leads, Rajni sign-off |
| Eval suite (automated) | Avinash | runnable on demand and on every prompt change |
| Eval dashboard | Avinash | results visible to team, alerting on regression |
| First eval run report | Lauran | baseline pass rates from spike |

---

## Phase 3: Compliance and legal (parallel; gates Phase 4)

**Owner:** Henry (legal), Lauran (product), Vishal (infrastructure)

**Compliance checklist (tracked, not just a list):**

- [ ] AWS BAA signed. Confirm Bedrock and Claude usage in Bedrock are explicitly in scope.
- [ ] Engineering policy: Bedrock only, no direct Anthropic API calls (unless and until a separate Anthropic BAA exists).
- [ ] Privacy notice updated: AI use, data flow, retention, patient rights.
- [ ] Patient AI consent: surfaces at start of chat, separate from HIPAA authorization.
- [ ] Right to opt out, right to human, right to access conversation logs: documented and operationally supported.
- [ ] Audit logging: every prompt, response, tool call, consent captured with timestamp and (if applicable) patient ID. HIPAA-grade controls.
- [ ] Encryption: at rest (AWS KMS), in transit (TLS 1.2+). No logs to third-party services.
- [ ] Data residency: US-only Bedrock regions.
- [ ] Retention policy: how long conversations are stored, who can access, when deleted.
- [ ] Access controls: who in Resonata can view conversations, role-based access, audit trail on access.
- [ ] Crisis response protocol: documented, tested in red-team eval, escalation path to humans staffed.
- [ ] Adverse event reporting: defined channel and protocol. Note: may trigger 21 CFR 803 if connected to a drug sponsor.
- [ ] FDA SaMD assessment: document why chat is not SaMD (does not diagnose, treat, prevent, mitigate, cure).
- [ ] State law review: CMIA (California), CCPA, CPRA, Washington My Health My Data, others as applicable.
- [ ] Algorithm transparency: prepare for emerging state requirements.
- [ ] Sponsor implications: confirm with Kristen whether sponsors need additional disclosures, indemnities, or audit access for AI-driven onboarding.
- [ ] Model versioning policy: how a Bedrock model version change is reviewed before being deployed to patients.

**Deliverables (Phase 3):**

| Deliverable | Owner | Notes |
|---|---|---|
| BAA package | Henry | signed AWS BAA, internal Bedrock-only policy |
| Compliance checklist (tracked) | Henry + Lauran | line-by-line owner and status |
| Patient AI consent flow | Henry + Lauran | drafted, legal-reviewed, baked into chat opening |
| Privacy notice update | Henry | drafted, legal-reviewed, deployed before alpha |
| Crisis and adverse event protocols | Rajni + Henry | documented, tested in red-team eval |
| Model version review policy | Vishal + Henry | gate before any Bedrock model update reaches patients |

**Gate to Phase 4:** all compliance items signed off; crisis protocol tested end-to-end; patient AI consent flow integrated.

---

## Phase 4: Controlled alpha (4 to 6 weeks after Phase 1, 2, 3 gates pass)

**Owner:** Lauran (PM), Vishal (eng), Rajni (clinical safety oversight)

**Scope:**

- Real patients, small cohort (target 20 to 50).
- Patient advisor cohort first (already in trust relationship, briefed, explicitly consenting).
- Then invite-only alpha. Recommend randomized assignment: half on chat, half on screen flow, same landing page.
- Real DNAvisit integration after spike-stub passes evals.

**Guardrails:**

- Human-in-loop for the first 100 conversations. Every conversation reviewed within 24 hours.
- Crisis escalation: real, tested, staffed.
- Kill switch: ability to route patients back to screen flow within minutes.
- Daily safety review for first two weeks.

**What we measure (on top of v2 PRD metrics):**

- Eval pass rate in production. Continuous.
- Patient-reported trust (post-flow survey).
- Conversion (quiz complete, Path A, Path B) vs screen-flow control.
- Time-to-records for upgraders.
- Conversation length and cost per patient.
- Escape rate (patients who switch from chat to screen mid-flow if offered).
- Mentions of AI in patient feedback (positive, negative, neutral).
- Refusal events. For each, was the refusal correct?

**Deliverables (Phase 4):**

| Deliverable | Owner | Notes |
|---|---|---|
| Alpha PRD | Lauran | scope, cohort, measurement plan, success criteria, kill criteria |
| Operations runbook | Vishal + Lauran | human-in-loop process, who staffs it, escalation tree |
| Alpha results reports | Lauran | week 2, week 4, week 6 |
| Go / no-go to beta | Lauran + Martin + Rajni | based on alpha evidence |

---

## Phase 5: Beta (timing depends on alpha results)

**Owner:** Lauran (PM), full team

- 100 to 500 patients.
- Randomized chat vs screen flow on the live signup page.
- Eval suite running continuously in production.
- Sponsor visibility (with Kristen) on the existence of the experiment.

**Deliverables (Phase 5):**

| Deliverable | Owner | Notes |
|---|---|---|
| Beta PRD | Lauran | scope, cohort sizing, measurement plan, success criteria for GA |
| Beta results report | Lauran | end-of-beta summary, recommended GA decision |

---

## Phase 6: GA (gated on beta evidence)

**Gate:** beta evidence of equal-or-better trust, conversion, and cost vs screen flow.

**Deliverables (Phase 6):**

| Deliverable | Owner | Notes |
|---|---|---|
| GA PRD | Lauran | full product spec for production chat experience |
| Operations playbook | Vishal + Lauran | steady-state ops, on-call, monitoring, model version updates |
| Continuous eval dashboard with alerting | Avinash | regression alerts, drift alerts, cost alerts |

---

## Full deliverables list (one place)

| # | Deliverable | Phase | Owner | Status |
|---|---|---|---|---|
| 1 | Henry-signed BAA scope memo | 0 | Henry | conversation done, doc pending |
| 2 | Decision doc: infrastructure path | 0 | Lauran + Henry | not started |
| 3 | Spike charter | 0 | Lauran | drafted 2026-05-14 |
| 4 | Compliance pre-read | 0 | Henry | not started |
| 5 | Eval staffing plan | 0 | Lauran + Rajni + Megan | not started |
| 6 | Crisis on-call staffing model | 0 | Lauran + Rajni + Henry | not started |
| 7 | Cost ceiling and budget | 0 | Martin + Lauran | not started |
| 8 | Chat handoff scope memo | 0 | Lauran | not started |
| 9 | Sponsor sign-off | 0 | Martin | not started |
| 10 | Spike PRD (what to build) | 1 | Lauran | not started |
| 11 | Working spike | 1 | Vishal + Avinash | not started |
| 12 | Cost and latency report | 1 | Vishal | not started |
| 13 | Risk register | 1 | Lauran | not started |
| 14 | Spike go / no-go | 1 | Lauran | not started |
| 15 | Eval framework spec | 2 | Lauran | not started |
| 16 | Clinical accuracy rubric | 2 | Megan (lead), Rajni (sign-off) | not started |
| 17 | Eval suite (automated) | 2 | Avinash | not started |
| 18 | Eval dashboard | 2 | Avinash | not started |
| 19 | BAA package | 3 | Henry | not started |
| 20 | Compliance checklist (tracked) | 3 | Henry + Lauran | not started |
| 21 | Patient AI consent flow | 3 | Henry + Lauran | not started |
| 22 | Privacy notice update | 3 | Henry | not started |
| 23 | Crisis and adverse event protocols | 3 | Rajni + Henry | not started |
| 24 | Model version review policy | 3 | Vishal + Henry | not started |
| 25 | Alpha PRD | 4 | Lauran | not started |
| 26 | Operations runbook | 4 | Vishal + Lauran | not started |
| 27 | Alpha results reports | 4 | Lauran | not started |
| 28 | Beta PRD | 5 | Lauran | not started |
| 29 | Beta results report | 5 | Lauran | not started |
| 30 | GA PRD | 6 | Lauran | not started |
| 31 | Operations playbook | 6 | Vishal + Lauran | not started |
| 32 | Continuous eval dashboard with alerting | 6 | Avinash | not started |

---

## MCP and API architecture (recommended pattern)

**Principle:** Server-side agent harness, not pure LLM chat.

The chat is not "Claude talking to a patient." It is an orchestration layer that uses Claude as a language interface, plus a set of MCP servers (or REST endpoints with strict schemas) that own everything stateful. The model handles language; the tools handle truth.

**Tool boundaries (the things the model can call):**

1. **Matching service tool.** Given quiz answers, returns matches in canonical Resonata format. Model never invents matches.
2. **Records orchestration tool.** Handles HIPAA auth and DNAvisit handoff. Model triggers it; model never simulates the result.
3. **Content lookup tool.** Vetted explainer content for MG, antibodies, treatments, trials. Model retrieves and presents; does not freelance medical claims.
4. **Consent capture tool.** Records each consent event with structured metadata, timestamp, patient ID, version of the consent text.
5. **Patient state tool.** Read and write patient profile fields, with field-level access controls.
6. **Email and magic link tool.** Sends only the three sanctioned email types from the v2 PRD.

**Why this matters:**

- Hallucination is contained: the model cannot make up a trial that does not exist.
- Audit trail is clean: every state change is a tool call, logged.
- Future-proof: when Claude versions change, integrations do not.
- HIPAA-defensible: data flows are documented and structured, not free-form.

**API considerations:**

- Latency budget: model call plus tool call(s) under 2 seconds to first token. Tune system prompt size, cache aggressively.
- Cost: mix Haiku (fast, cheap) for routine turns and Sonnet or Opus for high-stakes turns (consent, clinical questions). Measure during spike.
- Prompt caching: the system prompt is long. Use Bedrock's caching to amortize cost.
- Tool-call retry and fallback: if a tool fails, model has a defined fallback string, not a hallucinated answer.
- Streaming: stream to the patient as tokens arrive, but tools complete before the next turn proceeds.

**MCP specifically:** MCP is Anthropic's protocol for tool servers. It works server-side, fine inside a Bedrock-backed harness. You can use MCP servers internally without exposing anything to Anthropic. Alternative: in-process tool definitions in your harness (also fine, less reusable across products). Pick based on what KD's B2B side might reuse.

---

## What to decide first (this week)

1. **Get Henry's BAA memo in writing.** Conversation has happened; need a one-pager that confirms exact scope, model coverage, region, and logging configuration.
2. **Get spike budget approved by Martin.** Eng weeks plus AWS dollars, against defined learning goals.
3. **Confirm eval staffing.** Megan's dedicated hours per week for clinical rubric; Avinash split or second engineer for eval harness.
4. **Decide crisis on-call staffing model.** Who responds when a patient signals crisis. Clinical contractor if needed.
5. **Set the cost ceiling per onboarding.** A pre-registered go/no-go number.
6. **Define the chat handoff.** Where the chat ends, what the patient sees next.
7. **Pin spike scope to onboarding-only.** Don't try to also do post-onboarding chat in the spike.
8. **Pin spike cohort to synthetic-only.** No real patients until evals exist.
9. **Set the kill-criteria numbers** in the spike charter, before anyone writes code.

---

## Open issues (need decisions or research)

- **Field-level access control on PHI.** Flagged in plan review as a subsystem rather than a tool. Not well understood in current scope; needs research before Phase 1 tool design is finalized. What does "field-level access control on PHI" actually require operationally for a Resonata-deployed Bedrock harness? Owner: Vishal + Henry, due before spike build starts.
- **Full app functionality after onboarding is not defined.** Chat handoff scope memo (Phase 0 deliverable) addresses the handoff itself, but the post-onboarding patient experience (semi-converted app, enroller follow-up timing, where chat ends and persistent app begins) is still to be defined. Owner: Lauran, parallel workstream.

## Risks and open questions to track separately

- Sponsor reaction to "AI-driven onboarding" framing. Kristen owns the conversation.
- Patient advisor reaction to the chat format. Worth a focused session with the advisor cohort before alpha.
- Reusability across conditions. Is the chat MG-specific in V1, or does the architecture have to generalize from day one?
- Caregiver mode. The v2 PRD defers caregiver mode to V3. Chat may make caregiver mode easier or harder; revisit before GA.
- Cost-at-scale. Per-patient inference cost may be the gating factor, not the technology.
- Brand risk. A patient screenshots Claude saying something off-brand. Mitigation: tight evals, conservative model temperature, no model-generated marketing language.
- Model deprecation. Bedrock models get versioned and eventually deprecated. Need a model-update review policy (Phase 3 deliverable).

---

## Before greenlighting

Per the standing rule in the master context, this plan should be roasted by Thidwick before execution. Worth running it through `/thidwick` once the Phase 0 decisions are made and the spike charter exists, to surface assumptions and brittle points.

---

*Living document. Update as Phase 0 decisions land.*
