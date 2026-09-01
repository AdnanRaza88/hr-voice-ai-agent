# System Architecture — HR Voice AI Agent Platform

**Status**: Planning  
**Last updated**: 2026-09-01

## High-Level Shape

The system is a **multi-agent platform** coordinated by LangGraph, exposed through FastAPI, with a clean web frontend.

```
                    ┌─────────────────────────────┐
                    │        Frontend (UI)        │
                    │  Glass + Liquid Morph UI    │
                    └─────────────┬───────────────┘
                                  │
                    ┌─────────────▼───────────────┐
                    │         FastAPI             │
                    │  REST + Webhooks + Streams   │
                    └─────────────┬───────────────┘
                                  │
                    ┌─────────────▼───────────────┐
                    │     LangGraph Orchestrator  │
                    │   (routing + shared state)  │
                    └─────────────┬───────────────┘
                                  │
        ┌─────────────────────────┼─────────────────────────┐
        │                         │                         │
        ▼                         ▼                         ▼
┌───────────────┐       ┌─────────────────┐       ┌─────────────────┐
│  Voice & Call │       │  Knowledge &    │       │  Meeting & MOM  │
│  Agents       │       │  Agentic RAG    │       │  Agents         │
└───────────────┘       └─────────────────┘       └─────────────────┘
        │                         │                         │
        └─────────────────────────┼─────────────────────────┘
                                  │
                    ┌─────────────▼───────────────┐
                    │   Memory + Records Layer    │
                    │  (Checkpointers, DB, Sheets)│
                    └─────────────────────────────┘
```

## Major Subsystems

### 1. Platform Core
- LangGraph orchestration
- Shared typed state
- Memory architecture (short-term conversation + longer-term entity/memory store)
- Agent registry and discovery
- Custom Agent Builder runtime
- Configuration and secrets management
- Local-first + VPS deployment story

### 2. Agentic RAG Pipeline
Focus: **retrieval quality**

Pipeline stages (each can be its own agent or sub-graph):
1. Document Ingestion (upload, type detection, loading)
2. Intelligent Chunking (quality-focused, possibly multi-pass or multi-agent)
3. Embedding (local lightweight model)
4. Indexing (ChromaDB / FAISS)
5. Retrieval (hybrid or advanced strategies)
6. Answer Generation / Synthesis (grounded, concise)

### 3. Voice & Transcript Intelligence
- Telephony integration (Twilio)
- Concurrent call handling strategy
- STT → multi-agent transcript refinement pipeline
- Intent understanding
- Structured data extraction (screening fields)
- High-quality TTS response generation
- Call state management

### 4. Meeting / MOM Intelligence
- Ingest meeting audio or live transcript
- Multi-agent processing for cleaning, speaker ID, key points, action items, MOM writing

### 5. Records & Data Layer
- Structured storage for candidates, calls, meetings, action items
- Google Sheets / Excel support
- Ability to index records back into the RAG system

### 6. Custom Agent Builder
- User describes desired agent in natural language
- System produces contract + scaffold + registration
- Human review gate before activation

### 7. Frontend
- Clean professional interface
- Glassmorphism + liquid morph language
- Strict visual and code quality rules from constitution

## Memory Architecture (Critical)

Two layers minimum:
1. **Conversation / Thread Memory** — LangGraph checkpointer
2. **Longer-term Memory** — Entity memory, summaries, persistent knowledge

Memory design must be explicit. No agent that needs context across turns is allowed to be memory-less.

## Agent Organization Principles

- Clear single responsibility
- Explicit input and output schemas
- Registered in `agents/registry.md`
- Contract file per agent
- Prefer composition of specialized agents over one giant agent

## Deployment Modes

1. Local development / small team
2. Private VPS / VM for 24/7 operation

## Risk Areas That Need Careful Planning

- Transcript quality on real accents and mixed Urdu/English
- Chunking strategies that actually improve retrieval
- Concurrent call state isolation
- Memory growth and retrieval relevance over time
- Making Custom Agent Builder safe and useful
- Keeping UI quality high without over-engineering
