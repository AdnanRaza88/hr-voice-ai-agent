# Master Vision — HR Voice AI Agent Platform

**Status**: Planning  
**Last updated**: 2026-09-01

## Why This Exists

HR teams waste time on repetitive screening calls, poorly transcribed meetings, scattered knowledge, and lack of specialized automation. Existing tools are either too rigid, too expensive, cloud-locked, or produce mediocre transcripts and retrieval.

This platform aims to be different:
- Multi-agent by design
- Local-first and open-source friendly
- Obsessed with transcript quality and retrieval quality
- Extensible via a Custom Agent Builder
- Professional UI with no AI-slop aesthetics

## Primary Users

1. **HR / Talent teams** — run screening calls, answer policy questions, generate structured candidate data
2. **Managers / Team leads** — turn meetings into reliable MOM + action items
3. **Knowledge owners** — upload documents and get high-quality retrieval
4. **Power users / Admins** — create new specialized agents when a new need appears

## Core Value Propositions

| Capability                    | Why it matters                                      | Quality bar                          |
|------------------------------|-----------------------------------------------------|--------------------------------------|
| Agentic RAG                  | Knowledge must be trustworthy                       | Chunking + retrieval quality first   |
| Transcript Intelligence      | Raw STT is not enough                               | Multi-agent cleaning & structuring   |
| Voice Screening              | Fast, consistent, low-friction candidate intake     | Natural conversation + data capture  |
| Meeting / MOM                | Meetings should produce actionable output           | Speaker-aware, task-distributed      |
| Custom Agent Builder         | System must grow with the organization              | Natural language → working agent     |
| Strong Memory                | Context loss kills usefulness                       | No compromise                        |
| Local + VPS ready            | Privacy and cost control                            | Runs offline and on private servers  |

## System Character

- Calm, professional, reliable
- Feels like a capable internal tool, not a flashy AI demo
- Prefers correctness and clarity over cleverness
- Extensible without rewriting the core

## Success Picture (12–18 months)

- An HR person can upload company policies and immediately ask accurate questions via voice or text
- Screening calls produce clean structured data with minimal human cleanup
- Meetings automatically yield usable minutes and assigned action items
- When a new need appears (“I need an agent that summarizes candidate feedback weekly”), a user can describe it and get a working agent
- The entire system can run on a modest local machine or a single VPS

## Explicit Non-Goals (for now)

- Replacing full ATS / HRIS systems
- Consumer-facing product
- Heavy multi-tenant SaaS with complex billing
- Real-time video interviews
- Fully autonomous decision-making without human oversight on sensitive HR actions

## Planning Principle

We will invest heavily in planning documents (vision, architecture, agent contracts, specs, designs, tasks) before writing significant code. Good planning makes the rest of the system emerge cleanly. Missing small details early creates expensive problems later.
