flowchart TB
    User([🧑 User]) --> UI[💻 Streamlit / FastAPI UI (user selects options)]
    UI --> CrewAI[🤖 CrewAI Orchestrator]

    %% Retrieval choices
    CrewAI -->|Option 1| Qdrant[(🗄️ Qdrant - Memory)]
    CrewAI -->|Option 2| WebSearch[🌐 Web Search]
    CrewAI -->|Option 3| Hybrid[🔀 Hybrid Retrieval]

    %% Reasoning models
    Qdrant --> Reasoning1((gpt-oss:20b - ThinkLarger))
    Qdrant --> Reasoning2((deepseek-r1:8b - ThinkMini))
    WebSearch --> Reasoning1
    WebSearch --> Reasoning2
    Hybrid --> Reasoning1
    Hybrid --> Reasoning2

    %% Thinking modes
    Reasoning1 --> QuickThink[⚡ QuickThink - Chain of Thought]
    Reasoning1 --> DeepThink[🌳 DeepThink - Tree of Thought]
    Reasoning2 --> QuickThink
    Reasoning2 --> DeepThink

    %% Responses
    QuickThink --> UI
    DeepThink --> UI
