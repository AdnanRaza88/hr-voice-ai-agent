# Agent Registry

| ID            | Name                  | Responsibility                                      | Status  |
|---------------|-----------------------|-----------------------------------------------------|---------|
| orchestrator  | Orchestrator          | Route intents, manage shared state, decide next node| planned |
| stt           | SpeechToTextAgent     | Inbound audio → text                                | planned |
| intent        | IntentClassifierAgent | Classify user utterance                             | planned |
| screening     | ScreeningAgent        | Ask structured questions, extract answers           | planned |
| rag_policy    | RAGPolicyAgent        | Answer company policy questions from knowledge base | planned |
| tts           | TextToSpeechAgent     | Reply text → audio                                  | planned |
| persistence   | PersistenceAgent      | Save collected_data at end of call                  | planned |
| custom_builder| CustomAgentBuilder    | Scaffold + register a new specialized agent         | planned |

Each agent has a contract at `agents/<id>/agent.md`.
