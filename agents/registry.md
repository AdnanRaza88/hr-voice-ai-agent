# Agent Registry

This is the single source of truth for every specialized agent in the platform.  
Every agent listed here must have a corresponding contract at `agents/<id>/agent.md`.

**Legend**  
- Status: planned | in-design | implemented | deprecated  
- Domain: platform | rag | voice | meeting | builder | records

## Platform & Orchestration

| ID                | Name                     | Responsibility                                      | Domain    | Status   |
|-------------------|--------------------------|-----------------------------------------------------|-----------|----------|
| orchestrator      | Orchestrator             | Route intents, manage shared state, select next agent | platform | planned |
| memory_manager    | Memory Manager           | Read/write short-term and long-term memory          | platform | planned |
| session_manager   | Session Manager          | Create/isolate sessions for calls, chats, meetings  | platform | planned |

## Agentic RAG Domain

| ID                | Name                     | Responsibility                                      | Domain | Status   |
|-------------------|--------------------------|-----------------------------------------------------|--------|----------|
| doc_loader        | Document Loader          | Ingest uploaded files, detect type, extract text    | rag    | planned |
| chunk_planner     | Chunk Planner            | Decide chunking strategy for a document             | rag    | planned |
| chunker           | Chunker                  | Perform high-quality splitting                      | rag    | planned |
| chunk_critic      | Chunk Quality Critic     | Evaluate and improve chunk quality                  | rag    | planned |
| embedder          | Embedder                 | Generate embeddings with local model                | rag    | planned |
| indexer           | Indexer                  | Write vectors + metadata to Chroma/FAISS            | rag    | planned |
| retriever         | Retriever                | Execute retrieval strategies                        | rag    | planned |
| retrieval_critic  | Retrieval Critic         | Judge relevance of retrieved chunks                 | rag    | planned |
| rag_synthesizer   | RAG Synthesizer          | Produce grounded, concise answers                   | rag    | planned |
| knowledge_curator | Knowledge Curator        | Maintain knowledge base health and metadata         | rag    | planned |

## Voice & Call Domain

| ID                | Name                     | Responsibility                                      | Domain | Status   |
|-------------------|--------------------------|-----------------------------------------------------|--------|----------|
| stt               | Speech-to-Text           | Convert audio to raw transcript                     | voice  | planned |
| transcript_cleaner| Transcript Cleaner       | Fix errors, improve fluency and meaning             | voice  | planned |
| transcript_structurer | Transcript Structurer | Add structure, speakers, timestamps where possible | voice  | planned |
| intent_classifier | Intent Classifier        | Classify user utterance                             | voice  | planned |
| screening_agent   | Screening Agent          | Ask structured questions and extract fields         | voice  | planned |
| tts               | Text-to-Speech           | Convert reply text to natural speech                | voice  | planned |
| call_manager      | Call Manager             | Handle call lifecycle and concurrency               | voice  | planned |

## Meeting & MOM Domain

| ID                | Name                     | Responsibility                                      | Domain  | Status   |
|-------------------|--------------------------|-----------------------------------------------------|---------|----------|
| meeting_ingester  | Meeting Ingester         | Accept meeting audio or live transcript             | meeting | planned |
| speaker_identifier| Speaker Identifier       | Attribute speech segments to participants           | meeting | planned |
| key_point_extractor | Key Point Extractor    | Extract important discussion points                 | meeting | planned |
| action_item_agent | Action Item Agent        | Identify tasks, owners, and deadlines               | meeting | planned |
| mom_writer        | MOM Writer               | Produce clean Minutes of Meeting                    | meeting | planned |

## Records & Persistence

| ID                | Name                     | Responsibility                                      | Domain  | Status   |
|-------------------|--------------------------|-----------------------------------------------------|---------|----------|
| persistence       | Persistence Agent        | Write structured records (Sheets, DB, etc.)         | records | planned |
| records_indexer   | Records Indexer          | Optionally index records back into knowledge base   | records | planned |

## Custom Agent Builder

| ID                | Name                     | Responsibility                                      | Domain  | Status   |
|-------------------|--------------------------|-----------------------------------------------------|---------|----------|
| agent_analyst     | Agent Request Analyst    | Understand natural language agent request           | builder | planned |
| agent_designer    | Agent Designer           | Produce contract, tools, and integration plan       | builder | planned |
| agent_scaffolder  | Agent Scaffolder         | Generate files and register the new agent           | builder | planned |
| custom_builder    | Custom Agent Builder     | Orchestrates the above three for end-to-end creation| builder | planned |

## Notes

- Not every agent needs to be a full LLM agent. Some can be deterministic tools wrapped in the agent interface.
- Start with a smaller active set and grow.
- When a new agent is added via Custom Agent Builder, it must appear here and receive a proper contract.
