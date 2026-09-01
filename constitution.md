# Project Constitution — HR Voice AI Agent Platform

This document is the non-negotiable source of truth for every agent and human working on the system. Read it on every session.

## 1. Product Vision (One Sentence)

Build a local-first, open-source multi-agent platform that lets HR teams run intelligent voice screening, high-quality meeting intelligence (MOM), knowledge retrieval, and on-demand custom agents — all with excellent transcript quality, strong memory, and a clean professional interface.

## 2. Non-Negotiables

### Stack Preferences
- Language: Python 3.11+
- Orchestration: LangGraph (primary) + LangChain
- LLM: Prefer fast free/local options first (Groq, local models via Ollama/LM Studio when possible). Cloud only when necessary.
- Embeddings: Local lightweight models (sentence-transformers family)
- Vector stores: ChromaDB and/or FAISS (open-source, local)
- Voice: Twilio for telephony + high-quality STT/TTS (prefer free tiers / local where quality allows)
- Backend: FastAPI
- Frontend: Vanilla or lightweight modern stack with strict design rules (see UI section)
- Memory: LangGraph checkpointers + explicit long-term memory patterns. No compromise.
- Observability: LangSmith when useful, otherwise structured local logging
- Forbidden as primary path: Heavy paid-only SaaS, proprietary lock-in vector DBs, MongoDB for core data

### Code Quality
- Clean, human-written appearance
- No AI-style filler comments
- No emojis in code, commits, or UI
- No AI icons or "AI-looking" visual elements
- Strict type hints (TypedDict / Pydantic)
- No unexplained `any`
- Every specialized agent must have a written contract in `agents/<id>/agent.md`

### Design & UI Quality
- Glassmorphism + liquid morph aesthetic, mixed carefully
- Professional, calm, high-end feel
- Zero tolerance for generic AI-generated looking interfaces
- Accessibility and clarity over decoration
- Mobile-friendly where relevant

### Agent System Rules
- Spec is the source of truth. Update the relevant spec before changing behavior.
- Never invent requirements not present in an approved specification.
- One agent = one clear responsibility.
- Every agent is registered in `agents/registry.md` and has an explicit input/output contract.
- Custom Agent Builder is a first-class capability: users describe an agent in natural language; the system designs, scaffolds, and registers it.
- Orchestrator routes and coordinates. Business logic lives inside specialized agents.
- Memory is mandatory for agents that maintain context across turns or sessions.

### Performance & Cost
- Voice turn latency target remains aggressive (aim < 2s p95 where possible)
- Prefer local and free-tier components
- Document any paid dependency clearly

### Security & Privacy
- Secrets only via environment variables
- Minimize logging of raw PII and full transcripts in production
- Candidate and employee data treated as sensitive
- Local-first architecture supports air-gapped or private deployments

### Local-First & Deployment
- Must run fully on a local machine for development and small teams
- Must also support deployment on a VPS / VM for 24/7 operation
- Open-source tools preferred so users are not locked in

## 3. Core Capabilities (Must Be Planned)

1. High-quality Agentic RAG pipeline (upload → load → intelligent chunking → embed → best retrieval)
2. Voice call handling with concurrent support and excellent transcript cleaning
3. Meeting / MOM intelligence (transcript → structured notes → action items → speaker awareness)
4. Persistent memory across conversations and agents
5. Custom Agent Builder (natural language → working specialized agent)
6. Clean professional frontend
7. Records layer that can feed back into the knowledge base

## 4. Planning Discipline

- This project follows Spec-Driven Development.
- No significant implementation work begins until the relevant specification and design are written and reviewed.
- Small details matter. Missing edge cases in planning become expensive later.
- Prefer depth in planning documents over rushing to code.

## 5. Change Control

When requirements change:
1. Update the relevant specification first
2. Update design and tasks if needed
3. Then implement

Never let code become the source of truth.
