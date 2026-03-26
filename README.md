# Za Pizza Virtual Agent — Conversational AI Solution Design

A full-stack conversational AI solution designed for a fictional US restaurant chain, built as part of a take-home assignment for a Conversational AI role. The brief simulated a real pre-sales scoping engagement: I was given a client scenario and asked to produce deliverables a solutions engineer would actually bring to a stakeholder meeting.

---

## The Brief

Za Pizza is a large US restaurant chain operating an in-house call centre with 150 live agents. Their existing system is a DTMF-only IVR — no NLU, no natural language handling. The goal: design and pitch a Virtual Agent that reduces average handling time, cuts call queues, and decreases human-handled call volume, without hurting CSAT scores. English and Spanish support required.

---

## What I Built

### 1. Stakeholder Presentation
A 19-slide deck designed for an executive audience (CEO, CFO, CMO, VP of CS). Rather than leading with technology, I structured the narrative around business impact first — current cost of the status quo, projected ROI, phased rollout — and put the architecture in the middle as supporting evidence.

Key slides:
- **The Cost of Standing Still** — baseline metrics (AHT, abandonment rate, CSAT, cost per contact) against projected VA targets at months 1–2 and 5–6
- **Proven in Pizza** — real industry benchmarks anchoring the projections (Jet's Pizza, Domino's, Taco Bell/Yum Brands)
- **Clear ROI** — cost model showing ~$3.4M net Year 1 benefit, with a cumulative cost vs. savings curve
- **Architecture Overview** — full system map with tier logic, language detection, global fallback rules
- **The Growth Roadmap** — 5 Phase 2 capabilities with quantified revenue/savings estimates (proactive outreach, autonomous error resolution, personalization & loyalty, live intelligence, platform consolidation)

### 2. Architecture — 3-Tier Intent Framework

I designed an intent classification system that groups all 11 intents by how much human involvement they require:

| Tier | Logic | Intents |
|------|-------|---------|
| **Tier 1 — Full Containment** | No agent needed | Opening Hours, Chain Location, Menu, Reservations |
| **Tier 2 — API-Assisted Containment** | Resolved via API call, agent rarely needed | Delivery Status, Reset Password, App Malfunction |
| **Tier 3 — Collect, Then Hand Off** | VA collects context, warm transfers with full whisper | Order Delivery, Delivery Error, Feature Request, Leave a Message |

This framing was a deliberate choice: it lets a client see at a glance what percentage of their call volume can be fully deflected vs. enriched before reaching an agent, which is the number CFOs and VPs of CS actually care about.

### 3. Conversation Flow Design

I designed individual flows for all 11 intents plus 7 sub-flows. Each flow specifies:
- Parameters collected (slot-filling)
- Integration points (OMS API, Knowledge Bases, CRM, SMS Gateway, App Auth API)
- Decision branches and fallback logic
- VA script at each node

**Global rules applied across all flows:**
- Max 2 collection attempts → fallback
- 2× NLU failure → DTMF fallback
- All API errors → polite redirect to app/website
- Global human override → immediate warm transfer
- SMS requires consent; barge-in enabled throughout

> In real delivery, the individual intent flows would live in a technical spec document rather than the stakeholder deck. They're included here to demonstrate flow design depth.

### 4. NLU Training Utterances

I produced 12 training utterances per intent (132 total) covering:
- Clean/direct queries
- Colloquial and fragmented speech patterns
- Parameterised variants (`{order_id}`, `{city}`, `{time}`)
- Frustrated or emotionally charged inputs (important for voice — callers don't speak to IVRs the way they type into search bars)

---

## Key Design Decisions

**Why full VA replacement over a hybrid IVR?**
Running two systems in parallel (legacy DTMF + VA) means two vendor contracts, two maintenance tracks, and a degraded caller experience at the transition seam. Full replacement is cleaner and cheaper at Za Pizza's scale.

**Why native language detection over prompting?**
Asking callers to "press 1 for English" replicates the worst of DTMF. Vonage AI Studio supports automatic language detection — so the VA detects Spanish from the first utterance and routes to the parallel Spanish flow without the caller having to do anything.

**Why a warm transfer with whisper rather than a cold transfer?**
Callers shouldn't have to repeat themselves. For Tier 3 intents, the VA collects full context before handoff and passes it to the agent as a whisper — the agent hears `[caller_name], [error_type], [items_affected]` before the call connects. This protects CSAT even for escalated calls.

**Why include an upsell node in Order Delivery?**
The brief was about cost reduction, but a VA that only deflects misses the revenue opportunity. After order confirmation but before warm transfer, I added a lightweight upsell attempt (e.g., "Want to add garlic bread or a drink?"). One node, zero friction, meaningful upside.

---

## Deliverables

| File | Description |
|------|-------------|
| [`deck/stakeholder-presentation.pdf`](./deck/stakeholder-presentation.pdf) | Full executive presentation (19 slides) |
| [`nlu/training-utterances.md`](./nlu/training-utterances.md) | 132 utterances across 11 intents |
| [`assets/flowchart-overview.png`](./assets/flowchart-overview.png) | Architecture overview — full system map |
| [`assets/flowchart-intents.png`](./assets/flowchart-intents.png) | Individual flows for all 11 intents |
| [`assets/flowchart-subflows.png`](./assets/flowchart-subflows.png) | 7 sub-flows (Warm Transfer, CSAT, After Hours, CRM Ticket, API Error, Contact Info Confirmation, Anything Else) |

---

## What I'd Do Differently

- **Payment handling in Order Delivery** — I routed to a warm transfer for payment, but a real implementation would push toward in-app payment or a PCI-compliant voice payment module to reduce that handoff entirely
- **Spanish flow parity** — the architecture flags a parallel Spanish flow but doesn't fully spec it; in a real engagement, that's a separate design sprint
- **Confidence score thresholds** — I specified 2× NLU failure as the DTMF fallback trigger, but a production build would need empirically tested confidence thresholds per intent, not a fixed count

---

## About

I'm a solutions engineer and AI automation builder. This project sits at the intersection of both — it's conversation design work, but the underlying logic is the same systems thinking I apply when building agentic workflows and customer-facing AI products.

[LinkedIn](https://linkedin.com/in/jai-goldberg) · [GitHub](https://github.com/jai-goldberg)
