---
config:
  layout: elk
---
flowchart TD
 subgraph Uploads["Upload Options"]
        KBStore["Qdrant Collection: knowledge_base"]
        KBUpload["Upload to Knowledge Base"]
        AgenticStore["Qdrant Collection: agentic_store"]
        AgenticUpload["Upload to Agentic System"]
  end
 subgraph ProfessorMode["Professor Mode - Knowledge Base"]
        PythonKB["Python"]
        MSKB["Multiple Sclerosis"]
        EnglishKB["English & Vocabulary"]
  end
 subgraph AgenticRAG["Agentic RAG"]
        FileTypes["Docs/Repos/Scans"]
        PDF["PDF/DOCX/CSV/XLSX Parser"]
        Repo["GitHub Repo via Repomix"]
        OCRFlow["PaddleOCR → scanned docs/tables"]
        RetrievalModes["Retrieval Mode Selection"]
        Qdrant["Qdrant Only"]
        WebSearch["Web Search Only"]
        Hybrid["Hybrid - Qdrant + Web Search"]
        ReasoningModels["Reasoning Model Selection"]
        GPTOSS["gpt-oss:20b → ThinkLarger"]
        DeepSeek["deepseek-r1:8b → ThinkMini"]
        ThinkingModes["Thinking Mode Selection"]
        QuickThink["⚡ QuickThink → Chain of Thought"]
        DeepThink["🌳 DeepThink → Tree of Thought"]
        Opik["Opik Evaluation + Dashboards"]
  end
 subgraph Memory["Memory System"]
        STM["Short-Term Memory"]
        LTM["Long-Term Memory"]
        Episodic["Episodic Memory → Logs, OCR choices, Opik evals"]
        Semantic["Semantic Memory → Structured KB"]
        Procedural["Procedural Memory → Workflows"]
  end
 subgraph EnglishCoach["English Coach - Optional Wrapper"]
        ECSession["Start/Stop Session → Timestamp"]
        ECRecord["Monitor all chats/dictations"]
        ECAnalysis["Post-Session Analysis: vocab, grammar, readability"]
        ECDashboard["Separate English Progress Dashboard"]
  end
 subgraph Interaction["Interaction Layer"]
        ChatUI["Chat Interface - friend or professor tone"]
        STT["Speech-to-Text - Dictation"]
  end
    KBUpload --> KBStore
    AgenticUpload --> AgenticStore
    KBStore -- Domains --> PythonKB & MSKB & EnglishKB
    AgenticStore --> FileTypes & RetrievalModes & Opik & Episodic
    FileTypes --> PDF & Repo & OCRFlow
    RetrievalModes --> Qdrant & WebSearch & Hybrid
    Qdrant --> ReasoningModels
    WebSearch --> ReasoningModels
    Hybrid --> ReasoningModels
    ReasoningModels --> GPTOSS & DeepSeek
    GPTOSS --> ThinkingModes
    DeepSeek --> ThinkingModes
    ThinkingModes --> QuickThink & DeepThink
    ECSession --> ECRecord
    ECRecord --> ECAnalysis
    ECAnalysis --> ECDashboard
    KBStore --> Semantic
    Memory --> ProfessorMode & AgenticRAG
    Interaction --> ProfessorMode & AgenticRAG & EnglishCoach
