# Learning Plan: Claude Chat for Patients

**Owner:** Lauran
**Created:** 2026-05-14
**Purpose:** Single master source for everything you need to learn to ship a HIPAA-grade conversational AI product. Upload this file to NotebookLM as one source, then add the linked YouTube/web sources individually so NotebookLM can ground answers across all of them.

---

## How to Use This With NotebookLM

1. Create a new notebook in NotebookLM. Name it "Claude Chat for Patients - Course".
2. Upload this file as the first source (it's the spine).
3. Add the resources below as separate sources using their native types:
   - YouTube videos: paste the URL, NotebookLM ingests the transcript.
   - Web articles/blog posts: paste the URL.
   - PDFs (if you download Anthropic guides): upload the file.
   - Skilljar courses: these can't be ingested directly. Take notes as you go and upload your notes as sources later.
4. Once sources are loaded, use the "Studio" panel to generate a Study Guide, FAQ, and Audio Overview. The audio overview is genuinely good for commute/walk listening.
5. Use the questions at the bottom of this doc as starting prompts when chatting with the notebook.

---

## What You're Trying to Learn

You're a PM with 15+ years experience but you haven't shipped a conversational AI product before. You need three things:

1. **Concept fluency.** What an eval is, what an agent harness is, what tool use is, what RAG is, where each fits. Enough to have the right argument with engineering, not enough to write the code yourself.
2. **Judgment.** When to use which model, when to add a tool vs. add to the prompt, when an eval is "good enough" to ship.
3. **A vocabulary that works in healthcare context.** Where the patterns common in consumer AI break under HIPAA, where they hold.

You do NOT need: deep coding tutorials, LangChain framework knowledge, fine-tuning workflows, or future-of-work AI commentary.

---

## The 3-Week Sequence

### Week 1: Foundation (concepts and capabilities)

**Goal:** Be able to explain what a modern LLM application actually is and what its parts do.

| Day | Resource | What to extract |
|---|---|---|
| Mon | Anthropic Academy: **AI Fluency: Framework & Foundations** | Vocabulary baseline. How to think about LLMs as collaborators, not search. |
| Tue | Anthropic Academy: **AI Capabilities and Limitations** | What current models can and cannot do reliably. Hallucination, context window, drift. |
| Wed | YouTube: **Aman Khan, "2 hours to become an AI PM"** (https://youtu.be/Ej4pBDaHspk) | PM-specific framing of AI products. Built for your situation. |
| Thu | Anthropic blog: **Building Effective Agents** (https://www.anthropic.com/research/building-effective-agents) | The canonical Anthropic framing of workflows vs agents. Cite this in any architecture conversation with Vishal. |
| Fri | Ethan Mollick, **Co-Intelligence** (book, ~5 hrs total) | Big-picture context on how LLMs change work. Skim, don't study. |

### Week 2: Evals (the most important week)

**Goal:** Walk into the eval conversation with engineering and clinical knowing exactly what you want.

| Day | Resource | What to extract |
|---|---|---|
| Mon | YouTube: **Ankur Goyal, "Evals are the new PRD"** (https://youtu.be/71qvIkO9d_A) | The Distance Principle. The Data-Task-Scores framework. LLM-as-judge. Watch twice. |
| Tue | Hamel Husain blog: **"Your AI product needs evals"** (https://hamel.dev/blog/posts/evals/) | The operational view of evals. Failure modes. How to start small. |
| Wed | Hamel Husain blog: **"A Field Guide to Rapidly Improving AI Products"** (https://hamel.dev/blog/posts/field-guide/) | How to actually run the eval-improve loop. |
| Thu | Anthropic Academy: **Building with the Claude API** | Where evals plug into the API. Practical hooks. |
| Fri | Sit with Avinash for 60-90 min | Walk through how he'd actually instrument an eval suite for our chat. Bring questions from the videos. |

### Week 3: Architecture and integration

**Goal:** Be conversant on the AWS Bedrock path, MCP, and what tool use looks like in practice.

| Day | Resource | What to extract |
|---|---|---|
| Mon | Anthropic Academy: **Claude with Amazon Bedrock** | The HIPAA-compliant deployment path. AWS BAA. What Bedrock does and doesn't give you. |
| Tue | Anthropic Academy: **Introduction to MCP** | Tool boundaries. Why we want MCP-shaped tool calls. |
| Wed | Anthropic prompt engineering guide (https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview) | Prompt patterns that survive contact with reality. |
| Thu | Latent Space podcast: pick the most recent Anthropic episode (https://www.latent.space/) | Industry framing. Hear how operators talk about this stuff. |
| Fri | Re-watch Goyal eval video with everything you now know | Different things will land the second time. |

---

## The Core Library (linked resources to add to NotebookLM)

### Videos (add as sources)

1. **Ankur Goyal, "Evals are the new PRD"** - https://youtu.be/71qvIkO9d_A
   Watch first. Healthcare relevance is direct. Live MCP demo. Distance Principle is the single most useful eval framing.

2. **Aman Khan, "2 hours to become an AI PM"** - https://youtu.be/Ej4pBDaHspk
   Watch second. Built for PMs in your exact situation.

3. **Skip:** Jaclyn Konzelmann, Google AI Masterclass - https://youtu.be/rcz39Y0qFxw
   Consumer Google tool demos. Doesn't transfer to building a product.

### Anthropic Academy / Skilljar (note: not directly ingestible by NotebookLM)

Take notes as you go and upload your notes as a single doc per course.

1. **AI Fluency: Framework & Foundations** - https://anthropic.skilljar.com/ai-fluency-foundations
2. **AI Capabilities and Limitations** - https://anthropic.skilljar.com/
3. **Claude with Amazon Bedrock** - https://anthropic.skilljar.com/
4. **Introduction to MCP** - https://anthropic.skilljar.com/
5. **Building with the Claude API** - https://anthropic.skilljar.com/

(If specific course URLs differ, search the catalog at anthropic.skilljar.com.)

### Written sources (add as web links)

1. **Anthropic - Building Effective Agents** - https://www.anthropic.com/research/building-effective-agents
2. **Anthropic prompt engineering guide** - https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview
3. **Hamel Husain - Evals** - https://hamel.dev/blog/posts/evals/
4. **Hamel Husain - Field Guide to Rapidly Improving AI Products** - https://hamel.dev/blog/posts/field-guide/

### Books and ongoing reading

1. **Ethan Mollick, Co-Intelligence** (book) - read for context, don't study
2. **Latent Space podcast** - https://www.latent.space/
3. **Eric Topol, Ground Truths newsletter** - https://erictopol.substack.com/ (for healthcare-AI signal specifically)

---

## What to Skip

Don't waste time on these even if they show up in suggested-content algorithms:

- Hands-on Python tutorials beyond the Anthropic API course. Avinash writes the code.
- LangChain deep-dives. Not what we'll use.
- Fine-tuning content. We won't fine-tune Claude.
- "Future of work" AI commentary. Not your job to forecast.
- AutoGPT / agent-swarm content. Hype, not what we're building.
- Anything about training models from scratch.

---

## Questions to Prompt NotebookLM With

Once you've loaded the sources, here are starting questions. Each one should produce a synthesis across multiple sources.

### Concept questions
- What is the difference between an LLM workflow and an LLM agent, in plain terms? When should I pick which?
- What is "tool use" and how does it differ from RAG?
- What is MCP and why does it matter for a multi-tool agent?
- What is prompt caching and when does it save us money?

### Evals questions
- How would I build an eval suite for a patient chat that has to refuse to give medical advice, escalate suicide ideation, and stay on-topic?
- What's the Distance Principle and how do I apply it to ambiguous patient questions?
- When do I use LLM-as-judge vs. human-graded evals? What's the right ratio?
- What does Hamel mean by "looking at the data"? What does that look like in practice?

### Architecture questions
- Walk me through the Bedrock path for a HIPAA-compliant Claude deployment. What does AWS provide and what do we still need?
- What are the trade-offs between using Claude via Bedrock vs. directly via Anthropic API?
- How do I think about Opus 4.7 vs Haiku 4.5 in a routing strategy? What goes to which?

### Healthcare-specific questions
- What are the specific failure modes I should evals against, given that our users are patients with serious chronic conditions?
- How should I think about crisis escalation in a chat? When does the agent stop and hand off to a human?
- What are the audit-logging and reproducibility requirements that compliance will care about?

### PM judgment questions
- What should my PRD for this chat product include that a normal PRD wouldn't?
- What's the minimum eval coverage I should require before any external user touches the product?
- What does a phase-gate kill criterion actually look like, in numbers?

---

## How to Know You've Got It

You're ready when you can:

1. Sketch the architecture diagram on a whiteboard without notes: client, server harness, model router, tool layer, eval harness, audit log.
2. Explain to Martin in 90 seconds why we're going Bedrock not direct API.
3. Define the first 10 evals you want for the alpha, with rubric language for each.
4. Argue both sides of "should this be a workflow or an agent" for any given user flow.
5. Tell Avinash "no" to a technically interesting suggestion that doesn't pass an eval criterion.

---

## Tracking

When you finish a resource, append the date and one-line takeaway here. NotebookLM will pick this up on re-upload.

- [ ] AI Fluency Foundations -
- [ ] AI Capabilities and Limitations -
- [ ] Aman Khan video -
- [ ] Building Effective Agents -
- [ ] Co-Intelligence -
- [ ] Ankur Goyal video -
- [ ] Hamel "Evals" post -
- [ ] Hamel "Field Guide" post -
- [ ] Building with Claude API -
- [ ] Session with Avinash -
- [ ] Claude with Bedrock -
- [ ] Introduction to MCP -
- [ ] Prompt engineering guide -
- [ ] Latent Space episode -
- [ ] Re-watch Goyal -

---

*Living doc. Update as you find better resources or as the project shape changes.*
