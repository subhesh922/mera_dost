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
        FileTypes --> OCRFlow[OCR Engine]

        subgraph OCRHybrid[Hybrid OCR]
            dots[dots.ocr → layout-heavy scans]
            paddle[PaddleOCR → tables, structured docs]
        end

        OCRFlow --> dots
        OCRFlow --> paddle

        AgenticStore --> WebSearch[Web Search Toggle]
        AgenticStore --> Reasoning[Reasoning Styles]
        Reasoning --> CoT[Chain of Thought]
        Reasoning --> ToT[Tree of Thought]

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
