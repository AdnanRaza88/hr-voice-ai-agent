# Design: Core Screening Call Flow

## Architecture Overview

```
Twilio Call
    │
    ▼
FastAPI /voice/webhook  ──────────────────────────────────────┐
    │                                                         │
    ▼                                                         │
LangGraph Orchestrator                                        │
    │                                                         │
    ├──► STT Agent  (audio → text)                            │
    ├──► Intent Classifier Agent                              │
    │         │                                               │
    │         ├── policy_question ──► RAG Policy Agent        │
    │         └── screening_answer ─► Screening Agent         │
    │                                                         │
    ├──► TTS Agent  (text → audio) ──► Twilio stream          │
    └──► Persistence Agent  (end of call) ──► Google Sheets   │
```

High-level flow:
START → STT → Intent → (RAG | Screening) → TTS → (loop or Save → END)

## State Schema

```python
from typing import TypedDict, List, Dict, Optional, Literal
from langchain_core.messages import BaseMessage

class AgentState(TypedDict):
    call_sid: str
    transcript: str                    # latest user utterance
    user_input: str                    # cleaned
    conversation_history: List[BaseMessage]
    current_question_index: int
    collected_data: Dict[str, str]
    rag_answer: Optional[str]
    next_action: Literal["ask", "rag", "save", "end", "reprompt"]
    is_complete: bool
    error: Optional[str]
```

## Specialized Agents (see also agents/registry.md)

| Agent ID     | Responsibility                                      | Tools                  |
|--------------|-----------------------------------------------------|------------------------|
| orchestrator | Route based on intent and state                     | none                   |
| stt          | Convert inbound audio to text                       | Groq Whisper / Google  |
| intent       | Classify utterance: answer / policy_q / other / end | LLM                    |
| screening    | Ask next question, extract & validate field         | LLM                    |
| rag_policy   | Retrieve + answer from company knowledge base       | search_company_policy  |
| tts          | Convert reply text to speech audio                  | Google TTS             |
| persistence  | Write collected_data + metadata to Sheets / DB      | gspread / sqlite       |
| custom_builder | Scaffold a new agent from template                | file system            |

## Component Responsibilities

### FastAPI Layer
- `POST /voice/webhook` — Twilio voice webhook (TwiML or media stream)
- `POST /voice/stream` — real-time audio if using Media Streams
- `GET /health` — liveness

### LangGraph Graph
Nodes correspond 1:1 to specialized agents above.  
Edges are conditional on `next_action` and `is_complete`.

### RAG Subsystem
- Documents: PDF + FAQ.txt loaded at startup into local ChromaDB
- Embeddings: all-MiniLM-L6-v2
- Retriever exposed as LangChain tool `search_company_policy`
- RAG Policy Agent calls the tool then summarizes in ≤ 2 spoken sentences

### Persistence
- Primary: Google Sheets (one row per completed call)
- Fallback for local dev: SQLite table `screenings`

## File / Module Map (planned)

```
src/
├── main.py                 # FastAPI app + lifespan
├── graph.py                # LangGraph definition
├── state.py                # AgentState
├── agents/
│   ├── stt.py
│   ├── intent.py
│   ├── screening.py
│   ├── rag_policy.py
│   ├── tts.py
│   └── persistence.py
├── rag/
│   ├── loader.py
│   └── chain.py
├── twilio_handler.py
├── prompts.py
└── config.py
agents/                     # human-readable contracts
├── registry.md
├── _template/
└── <agent-id>/agent.md
knowledge/                  # source docs for RAG
specs/
constitution.md
AGENTS.md
```

## Integration Points
- Twilio: webhook + optional Media Streams
- Groq: LLM + optional Whisper
- Google Cloud: STT / TTS (service account JSON via env)
- ChromaDB: persistent local directory
- gspread: service account or OAuth

## Risks & Mitigations
| Risk                          | Mitigation                                      |
|-------------------------------|-------------------------------------------------|
| Latency spikes                | Groq + short prompts + streaming where possible |
| STT errors on accented speech | Re-prompt once; allow English/Urdu mix          |
| RAG hallucination             | Strict “only use retrieved context” instruction |
| Twilio trial limits           | Document credit usage; local mock mode          |
| Spec drift                    | Constitution rule: update spec before code      |

## Custom Agent Builder
A documented template + registration process lives under `agents/_template/`.  
New agents are added by:
1. Copying the template
2. Filling the contract
3. Implementing the node
4. Registering in registry.md and the graph routing table
5. Adding corresponding tasks to the active feature’s tasks.md
