```mermaid
flowchart LR
    User([🧑 User]) --> UI[💻 Streamlit / FastAPI UI]
    UI --> CrewAI[🤖 CrewAI Orchestrator]

    CrewAI -->|Embed docs/queries| Embeddings[(bge-large:335m)]
    Embeddings -->|Store vectors| Qdrant[(🗄️ Qdrant - Persistent Volume)]
    CrewAI -->|Retrieve context| Qdrant
    CrewAI -->|Fallback if low recall| WebSearch[🌐 Web Search]

    Qdrant -->|Context| Reasoning((gpt-oss:20b))
    WebSearch -->|External context| Reasoning
    CrewAI -->|Tasks / Prompts| Reasoning
    CrewAI -->|Generic Q&A| Chat((deepseek-r1:8b))

    Reasoning -->|Responses| UI
    Chat -->|Responses| UI
```
