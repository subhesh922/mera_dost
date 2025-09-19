# Personal Assistant Architecture

This document outlines the updated system design for the **Professor + Agent Assistant**.

---

## High-Level Overview

```mermaid
flowchart TD

    subgraph Uploads[Upload Options]
        KBUpload[Upload to Knowledge Base] --> KBStore[Qdrant Collection: knowledge_base]
        AgenticUpload[Upload to Agentic System] --> AgenticStore[Qdrant Collection: agentic_store]
    end

    subgraph ProfessorMode[Professor Mode (Knowledge Base)]
        KBStore -->|Domains| PythonKB[Python]
        KBStore -->|Domains| MSKB[Multiple Sclerosis]
        KBStore -->|Domains| EnglishKB[English & Vocabulary]
    end

    subgraph AgenticRAG[Agentic RAG]
        AgenticStore --> FileTypes[Docs/Repos/Scans]

        FileTypes --> PDF[PDF/DOCX/CSV/XLSX Parser]
        FileTypes --> Repo[GitHub Repo via Repomix]
        FileTypes --> OCRFlow[PaddleOCR → scanned docs/tables]

        AgenticStore --> RetrievalModes[Retrieval Mode Selection]
        RetrievalModes --> Qdrant[Qdrant Only]
        RetrievalModes --> WebSearch[Web Search Only]
        RetrievalModes --> Hybrid[Hybrid (Qdrant + Web Search)]

        Qdrant --> ReasoningModels[Reasoning Model Selection]
        WebSearch --> ReasoningModels
        Hybrid --> ReasoningModels

        ReasoningModels --> GPTOSS[gpt-oss:20b → ThinkLarger]
        ReasoningModels --> DeepSeek[deepseek-r1:8b → ThinkMini]

        GPTOSS --> ThinkingModes[Thinking Mode Selection]
        DeepSeek --> ThinkingModes

        ThinkingModes --> QuickThink[⚡ QuickThink → Chain of Thought]
        ThinkingModes --> DeepThink[🌳 DeepThink → Tree of Thought]

        AgenticStore --> Opik[Opik Evaluation + Dashboards]
    end

    subgraph Memory[Memory System]
        STM[Short-Term Memory]
        LTM[Long-Term Memory]
        Episodic[Episodic Memory → Logs, OCR choices, Opik evals]
        Semantic[Semantic Memory → Structured KB]
        Procedural[Procedural Memory → Workflows]
    end

    subgraph EnglishCoach[English Coach (Optional Wrapper)]
        ECSession[Start/Stop Session → Timestamp]
        ECSession --> ECRecord[Monitor all chats/dictations]
        ECRecord --> ECAnalysis[Post-Session Analysis: vocab, grammar, readability]
        ECAnalysis --> ECDashboard[Separate English Progress Dashboard]
    end

    subgraph Interaction[Interaction Layer]
        ChatUI[Chat Interface (friend/professor tone)]
        STT[Speech-to-Text (Dictation)]
    end

    %% Connections
    KBStore --> Semantic
    AgenticStore --> Episodic
    Memory --> ProfessorMode
    Memory --> AgenticRAG
    Interaction --> ProfessorMode
    Interaction --> AgenticRAG
    Interaction --> EnglishCoach
```

---

## Key Updates
1. **OCR** → Simplified to PaddleOCR only.  
2. **Retrieval Modes** → User can select between Qdrant-only, Web Search-only, or Hybrid.  
3. **Reasoning Models** → User can pick between:  
   - `gpt-oss:20b` (*ThinkLarger*) → deeper reasoning, coding tasks.  
   - `deepseek-r1:8b` (*ThinkMini*) → lighter, faster, generic chats.  
4. **Thinking Styles** → After reasoning model selection, user chooses:  
   - ⚡ QuickThink → Chain of Thought (fast, single reasoning path).  
   - 🌳 DeepThink → Tree of Thought (slower, multi-branch exploration).  
