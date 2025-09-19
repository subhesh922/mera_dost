# Stage 1 – Agentic Core Architecture

This diagram shows the Stage 1 architecture where we build the **Agentic AI core** (documents, repos, OCR, chat).

---

## Architecture Diagram

```mermaid
flowchart TD

    subgraph Uploads
        PDF[PDF/DOCX/CSV/XLSX Parser]
        Repo[GitHub Repo via Repomix]
        OCR[OCR Engine]
    end

    subgraph OCR_Hybrid
        dots[dots.ocr]
        paddle[PaddleOCR]
    end

    OCR --> dots
    OCR --> paddle

    Uploads --> Ingestion[Chunk + Embed (Ollama)]
    Ingestion --> AgenticStore[Qdrant Collection: agentic_store]

    AgenticStore --> Retrieval[Retrieve Top-k Chunks]
    Retrieval --> LLM[Ollama LLM → Response]

    subgraph Chat_Interface
        Streamlit[Streamlit Chat UI]
        STT[Speech-to-Text Input]
    end

    LLM --> Streamlit
    STT --> Streamlit
