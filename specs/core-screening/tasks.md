# Tasks: Core Screening Call Flow

**Status**: Ready for review  
**Depends on**: Approved spec.md + design.md

## Milestone 0 — Project Foundation
- [ ] T000 — Create repo structure, constitution.md, AGENTS.md, specs index
  Depends: none
  Files: constitution.md, AGENTS.md, specs/, agents/
  Acceptance: Files exist and match the design file map

- [ ] T001 — Write agents/registry.md and agent contracts for the 7 core agents + template
  Depends: T000
  Files: agents/registry.md, agents/*/agent.md, agents/_template/
  Acceptance: Every agent listed in design has a contract file

## Milestone 1 — Skeleton & Config
- [ ] T010 — Create FastAPI skeleton with /health and empty /voice/webhook
  Depends: T000
  Files: src/main.py, src/config.py, requirements.txt, .env.example
  Acceptance: `uvicorn src.main:app` starts; GET /health → 200

- [ ] T011 — Define AgentState and empty LangGraph in graph.py
  Depends: T010
  Files: src/state.py, src/graph.py
  Acceptance: Graph compiles with placeholder nodes

## Milestone 2 — RAG
- [ ] T020 — Implement knowledge loader + ChromaDB + search_company_policy tool
  Depends: T010
  Files: src/rag/loader.py, src/rag/chain.py, knowledge/ (sample FAQ)
  Acceptance: Query “working hours” returns relevant chunk

- [ ] T021 — Implement RAG Policy Agent node
  Depends: T020, T001
  Files: src/agents/rag_policy.py
  Acceptance: Node returns ≤ 2-sentence answer grounded in context

## Milestone 3 — Screening Logic
- [ ] T030 — Implement Screening Agent (question list + extraction)
  Depends: T011, T001
  Files: src/agents/screening.py, src/prompts.py
  Acceptance: Given a transcript, correctly updates collected_data and advances index

- [ ] T031 — Implement Intent Classifier Agent
  Depends: T011, T001
  Files: src/agents/intent.py
  Acceptance: Distinguishes policy_question vs screening_answer vs end/other

## Milestone 4 — Voice Pipeline (mocked first)
- [ ] T040 — STT and TTS agent stubs + real implementations behind flags
  Depends: T011
  Files: src/agents/stt.py, src/agents/tts.py
  Acceptance: Unit tests pass with mocks; real path works when credentials present

- [ ] T041 — Wire full graph: STT → Intent → (RAG|Screening) → TTS → conditional Save
  Depends: T021, T030, T031, T040
  Files: src/graph.py
  Acceptance: Simulated conversation completes all 5 questions and sets is_complete

## Milestone 5 — Persistence & Twilio
- [ ] T050 — Persistence Agent (Sheets + local SQLite fallback)
  Depends: T041
  Files: src/agents/persistence.py
  Acceptance: After a completed run a row appears in the target Sheet / DB

- [ ] T051 — Twilio webhook handler producing TwiML / media stream hooks
  Depends: T041
  Files: src/twilio_handler.py, src/main.py
  Acceptance: Twilio can hit the webhook (via ngrok) and receive a valid response

## Milestone 6 — End-to-End & Polish
- [ ] T060 — Happy-path integration test (mocked Twilio + real LLM if key present)
  Depends: T050, T051
  Files: tests/
  Acceptance: Test passes and asserts data was saved

- [ ] T061 — Documentation: README with setup, free-tier keys, ngrok instructions
  Depends: T060
  Files: README.md
  Acceptance: A new developer can follow the README and place a test call

## Custom Agent Path (can be parallel)
- [ ] T070 — Implement agents/_template and a small CLI or script to scaffold a new agent
  Depends: T001
  Files: agents/_template/, scripts/new_agent.py (optional)
  Acceptance: Running the scaffold produces a valid contract + stub node that can be registered
