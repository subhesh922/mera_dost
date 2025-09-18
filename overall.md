# Personal Assistant Architecture

```mermaid
flowchart TD

    subgraph Uploads
        KBUpload[Upload to Knowledge Base] --> KBStore[Qdrant: knowledge_base]
        AgenticUpload[Upload to Agentic System] --> AgenticStore[Qdrant: agentic_store]
    end

    subgraph Professor_Mode
        KBStore --> PythonKB[Python Knowledge]
        KBStore --> MSKB[Multiple Sclerosis Knowledge]
        KBStore --> EnglishKB[English & Vocabulary]
    end

    subgraph Agentic_RAG
        AgenticStore --> Docs[PDF/DOCX/CSV/XLSX Parser]
        AgenticStore --> Repo[GitHub via Repomix]
        AgenticStore --> OCR[OCR Engine]

        OCR --> Dots[dots.ocr]
        OCR --> Paddle[PaddleOCR]

        AgenticStore --> WebSearch[Web Search]
        AgenticStore --> Reasoning[Reasoning Styles]
        Reasoning --> CoT[Chain of Thought]
        Reasoning --> ToT[Tree of Thought]

        AgenticStore --> Opik[Opik Evaluation]
    end

    subgraph Memory
        STM[Short-Term Memory]
        LTM[Long-Term Memory]
        Episodic[Episodic Memory]
        Semantic[Semantic Memory]
        Procedural[Procedural Memory]
    end

    subgraph English_Coach
        ECSession[Start/Stop Session]
        ECSession --> ECRecord[Record Conversations]
        ECRecord --> ECAnalysis[Analyze Vocabulary & Grammar]
        ECAnalysis --> ECDashboard[English Progress Dashboard]
    end

    subgraph Interaction
        ChatUI[Chat Interface]
        STT[Speech-to-Text Input]
    end

    KBStore --> Semantic
    AgenticStore --> Episodic
    Memory --> Professor_Mode
    Memory --> Agentic_RAG
    Interaction --> Professor_Mode
    Interaction --> Agentic_RAG
    Interaction --> English_Coach
