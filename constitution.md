# Project Constitution — HR Voice AI Agent

## Non-negotiables

### Stack
- Language: Python 3.11+
- Agent orchestration: LangGraph + LangChain
- LLM: Groq API (`llama-3.1-70b-versatile` or current free fast model)
- Embeddings: sentence-transformers/all-MiniLM-L6-v2 (local via HuggingFace)
- Vector DB: ChromaDB (local)
- STT: Groq Whisper or Google Cloud Speech-to-Text (free tier preferred)
- TTS: Google Cloud Text-to-Speech (free tier) or equivalent free option
- Telephony: Twilio (trial credits)
- Backend: FastAPI + Uvicorn
- Persistence: Google Sheets via gspread (or SQLite for local dev)
- Forbidden: Paid-only APIs as primary path, MongoDB, heavy cloud vector DBs for MVP

### Quality Gates
- All public functions and state schemas must have type hints (TypedDict / Pydantic)
- No raw `any` without an inline justification
- Every agent node must have a clear input/output contract documented in agents/
- Unit tests required for intent classification, data extraction, and RAG retrieval
- Integration test for the happy-path call flow (mocked Twilio)

### Performance & Cost
- Target p95 end-to-end reply latency < 2 seconds (Groq helps)
- Prefer free/local components; document any paid usage
- Keep voice responses under 2 sentences unless the candidate asks for more detail

### Security & Privacy
- Never log full transcripts or PII in plain text in production logs
- All secrets via environment variables / .env (never committed)
- Candidate data stored only in the configured persistence layer
- Comply with local data protection expectations for HR data

### Code Style
- Clean, human-written appearance
- No AI-style filler comments
- No emojis in code or commit messages
- Prefer explicit names over clever ones

### Agent & Spec Rules
- Spec is the source of truth. Update the relevant spec.md before changing behavior.
- Never invent requirements not present in an approved spec.
- Every specialized agent must be registered in agents/registry.md and have an agent.md contract.
- Custom agents are added only via the Custom Agent Builder process.
- Orchestrator only routes; business logic lives inside specialized agents.
