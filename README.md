# **Za Pizza — Enterprise Voice AI Virtual Agent | $3.4M Net ROI Architecture & Executive Deck**

A solutions engineer take-home assignment. The fictional client (Za Pizza) provided basic context about their business and call centre operation. I was tasked with scoping, designing, and presenting a full virtual agent POC as if pitching to their executive stakeholders.

---

## The Challenge

Modernize a legacy 150-agent DTMF IVR into a high-containment Voice AI experience using a fixed 11-intent schema — without degrading CSAT.

**Solution:** A 3-tier containment architecture delivering $3.4M net Year-1 value and a projected CSAT improvement to 4.4.  
**Validated Design:** Received explicit feedback during the Vonage Solutions Engineering process that the architecture was distinctive and the deck was highly effective for non-technical stakeholders. Advanced to final rounds.  
**Scope:** Full discovery, architecture, NLU training data, conversation flows, and ROI modeling.  
**Turnaround:** 48 hours.

---

## Architecture Overview

I grouped all 11 intents into a 3-tier framework based on how much human involvement each requires. This framing was a deliberate choice — it lets a client see immediately what percentage of their call volume can be fully deflected, partially contained, or enriched before reaching an agent.

![Architecture Overview](./assets/Overview.drawio.png)

> **Legend:** [View node type legend](./assets/Legend.drawio.png)

| Tier | Logic | Intents |
|------|-------|---------|
| **Tier 1 — Full Containment** | Resolved entirely by the VA, no agent needed | Opening Hours, Chain Location, Menu, Reservations |
| **Tier 2 — API-Assisted Containment** | Resolved via live API call, agent rarely needed | Delivery Status, Reset Password, App Malfunction |
| **Tier 3 — Collect, Then Hand Off** | VA collects full context, warm transfers with agent whisper | Order Delivery, Delivery Error, Feature Request, Leave a Message |

Global rules applied across all flows: max 2 collection attempts before fallback, 2× NLU failure triggers DTMF fallback, all API errors redirect politely to app or website, global human override routes to immediate warm transfer, barge-in enabled throughout.

---

## Key Design Decisions

**Full VA replacement over a hybrid IVR.**
Running the legacy DTMF system alongside a new VA means two vendor contracts, two maintenance tracks, and a broken caller experience at the handoff seam. At Za Pizza's scale, full replacement is cleaner and cheaper.

**Native language detection over prompting.**
Asking callers to "press 1 for English" replicates the worst of DTMF. Vonage AI Studio supports automatic language detection — the VA detects Spanish from the first utterance and routes to the parallel Spanish flow without the caller doing anything.

**Warm transfer with agent whisper, not cold transfer.**
For all Tier 3 intents, the VA collects complete context before handoff and passes it to the agent as a whisper. The agent hears the caller's name, issue type, and relevant details before the call connects. Callers never repeat themselves — which is the single biggest driver of CSAT drop on escalated calls.

**Upsell node in Order Delivery.**
The brief was about cost reduction. But a VA that only deflects misses the revenue side. After order confirmation and before warm transfer, I added a lightweight upsell attempt triggered by a Menu KB query for relevant add-ons. One node, zero friction.

**Allergy and dietary queries redirected to app/website.**
A VA answering allergen questions over voice is a liability. If it gets it wrong, it's a health and legal issue. The flow catches these queries and redirects to the app where accurate, up-to-date information lives.

---

## Deliverables

| File | Description |
|------|-------------|
| [Stakeholder Presentation Final.pdf](./deck/Stakeholder%20Presentation%20Final.pdf) | Full 19-slide executive presentation — business case, ROI model, architecture, growth roadmap |
| [Stakeholder Presentation Intents.pdf](./deck/Stakeholder%20Presentation%20Intents.pdf) | Individual slides for all 11 intents — flow summary, integrations, parameters per intent |
| [`nlu/training-utterances.md`](./nlu/training-utterances.md) | 132 NLU training utterances across all 11 intents |
| [`assets/intents/`](./assets/Intents/) | Conversation flow diagrams for all 11 intents |
| [`assets/subflows/`](./assets/subflows/) | 7 sub-flow diagrams (Warm Transfer, CSAT, After Hours, CRM Ticket Creation, API Error, Contact Info Confirmation, Anything Else) |

---

## About

I'm a solutions engineer and AI automation builder — I work across the full stack from pre-sales scoping and solution design through to building and deploying production-grade AI systems.

[LinkedIn](https://www.linkedin.com/in/jai-goldberg142/) · [GitHub](https://github.com/Jacing142)
