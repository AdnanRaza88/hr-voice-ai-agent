# Feature: Core Screening Call Flow

**Status**: Draft  
**Created**: 2026-08-31  
**Priority**: P0

## Summary
A candidate calls a Twilio number and is greeted by “Ayesha”, an HR voice assistant. The agent conducts a short structured screening, answers company-policy questions from a knowledge base when asked, collects key fields, and saves the results after the call ends.

## User Stories

### US-1 — Complete Screening (P0)
As a candidate, I want to answer a short set of screening questions over the phone so that my profile reaches the HR team quickly.

Acceptance:
- [ ] Agent greets the caller by name “Ayesha” and states the purpose in a friendly professional tone.
- [ ] Agent asks exactly one question at a time from the defined list.
- [ ] Agent extracts and stores answers for: full_name, experience_years, current_location, expected_salary, availability.
- [ ] After the last required answer the agent thanks the candidate and ends the call gracefully.
- [ ] Collected data appears in the configured Google Sheet (or local DB) within 30 seconds of call end.

### US-2 — Policy Question during Screening (P0)
As a candidate, I want to ask about company policies (working hours, leave, remote work, etc.) and receive an accurate short answer so that I can decide whether to continue.

Acceptance:
- [ ] When the user asks a policy-related question the agent uses the RAG tool instead of guessing.
- [ ] Answer is grounded in the loaded knowledge base documents.
- [ ] Answer is spoken in ≤ 2 sentences and then the agent returns to the next screening question (or continues if already finished).

### US-3 — Real-time Voice Loop (P0)
As a candidate, I want natural turn-taking with low latency so the conversation feels human.

Acceptance:
- [ ] End-to-end reply latency target < 2 s (p95) under normal network conditions.
- [ ] Agent handles short silences and re-prompts once if needed.
- [ ] Agent speaks in a natural Urdu + English mix appropriate for Pakistani candidates.

## Functional Requirements

- **FR-001**: WHEN a call is received on the Twilio number THE system SHALL start a new conversation session keyed by call_sid.
- **FR-002**: THE system SHALL greet the caller as “Ayesha” and explain that this is an initial screening.
- **FR-003**: THE system SHALL ask the following questions one at a time (order fixed for MVP):
  1. Full name
  2. Years of relevant experience
  3. Current city / location
  4. Expected monthly salary (PKR)
  5. Earliest availability / notice period
- **FR-004**: WHEN the candidate provides an answer THE system SHALL extract the value, validate basic format, and store it in the session state.
- **FR-005**: IF the answer is unclear or missing THE system SHALL re-ask the same question once, then mark the field as “unclear” and move on.
- **FR-006**: WHEN the candidate asks a question about company policy THE system SHALL invoke the RAG policy agent and speak the retrieved answer.
- **FR-007**: AFTER all required fields are collected (or the candidate ends the call) THE system SHALL persist the collected_data and call metadata.
- **FR-008**: THE system SHALL support a graceful end-of-call message.

## Non-Functional Requirements

- **NFR-001**: p95 latency from end of user speech to start of agent speech < 2 seconds.
- **NFR-002**: Use only free-tier or local components for the MVP (Groq, local embeddings, Chroma, Twilio trial, Google free credits).
- **NFR-003**: All secrets loaded from environment variables.
- **NFR-004**: Conversation state must survive the full call duration.

## Data Collected

| Field              | Type   | Required | Validation / Notes                  |
|--------------------|--------|----------|-------------------------------------|
| full_name          | str    | yes      | non-empty                           |
| experience_years   | float  | yes      | ≥ 0                                 |
| current_location   | str    | yes      | city or “remote”                    |
| expected_salary    | str    | yes      | free text, preferably PKR amount    |
| availability       | str    | yes      | free text (notice period / date)    |
| call_sid           | str    | yes      | Twilio call SID                     |
| transcript_summary | str    | no       | optional short summary              |

## Edge Cases & Error Handling

- IF user is silent > 8 seconds THEN re-prompt once; if still silent, end call politely.
- IF STT returns low-confidence or empty transcript THEN ask the user to repeat.
- IF RAG returns no relevant documents THEN say “I don’t have that information right now; the HR team can answer it later.”
- IF persistence fails THEN log the error and still end the call gracefully (data can be recovered from logs in MVP).
- IF user asks to speak to a human THEN collect a callback preference if possible and end the call.

## Out of Scope (MVP)

- Full interview scheduling or calendar integration
- Video or WhatsApp channels
- Multi-language detection beyond Urdu/English mix
- Authentication of the caller
- Advanced sentiment analysis or scoring
- Admin dashboard (Sheet is the MVP UI)
- Custom agent creation UI (CLI / code template only for now)

## Success Criteria

- **SC-001**: A real call to the Twilio number completes the 5-question flow and data appears in the Sheet.
- **SC-002**: Asking “What are the working hours?” (or similar) returns an answer grounded in the knowledge-base PDF/FAQ.
- **SC-003**: Measured latency on a typical call stays under the 2-second target for most turns.
- **SC-004**: The entire happy path can be demonstrated with free-tier credentials only.

## Glossary
- **Ayesha**: The persona name of the voice agent.
- **Screening questions**: The fixed list of 5 fields above.
- **RAG Policy Agent**: Specialized agent that answers from the company knowledge base.
