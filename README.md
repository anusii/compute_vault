# compute_vault

## Pod AI Assistant (Working Title)

Privacy-preserving AI assistant that connects to a Solid Pod (Personal Online Datastore), reads personal data with user consent, and returns useful insights while ensuring full user ownership and control.

---

## Problem

Most AI assistants require users to upload private data (notes, documents, health logs, etc.) to centralized servers, leading to privacy risks, vendor lock-in, and potential misuse.

Solid Pods address data ownership, but there is currently no standard architecture for enabling powerful LLMs to use Pod data without compromising privacy.

---

## Core Idea

- User data remains in their Solid Pod, not in application databases  
- The AI assistant receives scoped, temporary permissions to access specific Pod resources (e.g., `/notes/`, `/health/`)  
- A backend AI service (local, remote, or hybrid) processes this data  
- Results (summaries, insights, extracted tasks) are written back to the user’s Pod  

This reverses the traditional model: users own their data, and AI services operate on it with permission.

---

## High-Level Architecture

### Components

**Solid Pod**
- Stores user data (notes, files, logs, chat history)
- Uses Solid access control and WebID identity

**Client Application (Web/Desktop)**
- Authenticates via Solid-OIDC
- Grants fine-grained read/write permissions

**AI Backend Service**
- Fetches only permitted data using access tokens
- Runs LLM pipelines (summarization, Q&A, RAG)
- Writes outputs back into the Pod

---

## Architecture Diagram

```mermaid
flowchart TD
    A[User] --> B[Client App (Web/Desktop)]
    B -->|OIDC Login| C[Solid Pod]

    B -->|Request Access Token| C
    B -->|Scoped Permissions| C

    B --> D[AI Orchestrator Backend]

    D -->|Fetch Allowed Data| C
    D --> E[Privacy Layer]

    E -->|Edge Mode| F[Local LLM (Ollama / llama.cpp)]
    E -->|Trusted Mode| G[Cloud LLM (Claude / OpenAI)]
    E -->|Hybrid Mode| H[Local Preprocessing + Cloud LLM]

    F --> D
    G --> D
    H --> D

    D -->|Write Results| C
    D --> B
```
