# AI Agent & Knowledge System

A self-hosted conversational AI layer — chatbot, local LLM inference, and a retrieval-augmented knowledge service — built without relying on third-party AI APIs.

## The problem

Client enquiries needed a fast, always-on first response, and the business wanted answers grounded in its own knowledge (services, pricing, policies) rather than a generic model hallucinating on the business's behalf — while keeping all data in-house rather than sending client conversations to an external AI vendor.

## Architecture

- **Local LLM inference** — a self-hosted model server runs multiple model sizes side by side: a larger model for conversational quality, and smaller, faster models for lightweight classification and quick responses, with a cloud model available as an opt-in fallback for heavier reasoning tasks.
- **RAG / knowledge service** — a retrieval layer that embeds the business's own documents and pulls relevant context into a prompt before the model answers, so responses are grounded rather than invented.
- **Chatbot front-end** — a chat platform embedded on the public website, talking to the local LLM and RAG layer behind the scenes.
- **Intake agent** — a dedicated agent classifies incoming enquiries, extracts the useful details, and writes a structured lead record to the database — turning a free-text message into a usable CRM entry automatically.
- **Automation gateway (separate project, same core)** — the same local LLM also powers a chat-driven admin automation agent, with a hard requirement that it proposes an action and waits for explicit human confirmation before anything executes.

## Design decisions worth calling out

**Local-first, not API-first.** Running inference locally meant more setup work (model selection, hardware sizing, keeping models updated) but kept client data off third-party AI infrastructure entirely — a deliberate trade of convenience for data control.

**Model tiering.** Not every task needs the biggest model. Classification and quick lookups run on a small, fast model; only genuinely complex conversations route to the larger one — keeping response times low without sacrificing quality where it matters.

**Grounding over generation.** The RAG layer exists specifically so the chatbot answers from real business documents instead of the model's general knowledge — reducing the risk of confidently wrong answers reaching a client.

**Structured output from unstructured input.** The intake agent's whole job is turning a messy chat message into a clean, structured lead — the kind of "boring but valuable" automation that actually saves staff time daily.

**Guardrails on anything that acts.** Any agent capable of *doing* something (sending an email, triggering a job) is separated from any agent that only *answers* — and the acting agent requires human confirmation before every action, with a standing list of action types it will never execute unattended.

## Outcome

A chatbot that answers from the business's own knowledge instead of guessing, an intake pipeline that turns raw enquiries into usable leads without manual data entry, and an automation layer that can act on the business's behalf without ever doing so unsupervised.

---
