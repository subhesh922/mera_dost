flowchart TD
    User[User] --> UI[UI]
    UI --> CrewAI[CrewAI]
    CrewAI --> Qdrant[Qdrant]
    CrewAI --> WebSearch[Web Search]
    Qdrant --> GPT[gpt-oss:20b]
    WebSearch --> GPT
    CrewAI --> Deepseek[deepseek-r1:8b]
    GPT --> Thinking[QuickThink / DeepThink]
    Deepseek --> Thinking
    Thinking --> UI
