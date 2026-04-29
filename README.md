# compute_vault

Pod AI Assistant (Working Title)

Privacy-preserving AI assistant that connects to a Solid Pod (Personal Online Datastore), reads personal data with user consent, and returns useful insights while ensuring full user ownership and control.

Problem

Most AI assistants require users to upload private data (notes, documents, health logs, etc.) to centralized servers, leading to privacy risks, vendor lock-in, and potential misuse.

Solid Pods address data ownership, but there is currently no standard architecture for enabling powerful LLMs to use Pod data without compromising privacy.

Core Idea
User data remains in their Solid Pod, not in application databases.
The AI assistant receives scoped, temporary permissions to access specific Pod resources (e.g., /notes/, /health/).
A backend AI service (local, remote, or hybrid) processes this data.
Results (summaries, insights, extracted tasks) are written back to the user’s Pod.

This reverses the traditional model: users own their data, and AI services operate on it with permission.

High-Level Architecture
Solid Pod

Stores user data (notes, files, logs, chat history) using Solid access control and WebID identity.

Client Application (Web/Desktop)
User authenticates via Solid-OIDC
Grants fine-grained read/write permissions to the AI assistant
AI Backend Service
Fetches only permitted data using access tokens
Runs LLM pipelines (summarization, Q&A, retrieval-augmented generation)
Stores outputs (summaries, embeddings metadata, chat logs) back into the Pod
Privacy Modes (Research Focus)

The central research question:

How can AI use personal data in Pods without violating privacy?

1. Edge / On-Device Mode
Data is processed locally (e.g., using llama.cpp or Ollama)

Pros

Data never leaves the device
Maximum privacy

Cons

Limited compute
Smaller models only
2. Trusted Backend Mode
Server processes Pod data and calls a powerful LLM

Pros

High-quality model outputs
Faster development and deployment

Cons

Requires trust in backend
Risk of data exposure
3. Hybrid Mode (Primary Research Target)
Sensitive processing (e.g., PII filtering, embeddings, pre-summarization) runs locally
Only minimized or redacted data is sent to the backend LLM

Goal: balance privacy (edge) and performance (cloud)

Future Extension

Trusted Execution Environments (TEEs)
Confidential computing for encrypted data processing
Example Use Cases
Personal Knowledge Assistant

Summarize notes, track todos, and extract insights from user data stored in the Pod.

Document Chat (RAG over Pod Data)

Chat with documents stored in /docs/, with chat history stored back in the Pod.

Health / Activity Insights

Analyze personal logs (steps, sleep, journals) and generate trends without centralizing raw data.

Research Contribution

This project serves as both a prototype and a research artifact:

Compare Edge vs Backend vs Hybrid approaches in terms of:
Privacy (data exposure)
Utility (quality of results)
UX (latency and responsiveness)
Demonstrate how Solid WebID and access control can act as an identity and authorization layer for AI systems
Extend ideas from prior work (e.g., SocialGenPod) toward practical, privacy-preserving AI systems
Tech Stack

Pods & Identity

Solid Pod provider (Community or Node Solid Server)
Solid-OIDC and access control

Backend

Node.js (Express) or Python (FastAPI)

AI

Local: llama.cpp, Ollama
Cloud: Claude, OpenAI, or self-hosted models

Security

TLS
OAuth / OIDC
Scoped access tokens
Optional encryption for sensitive data
Project Status
Basic prototype:
Solid WebID login
Read notes from Pod
Generate summaries using LLM
Write results back to Pod
Planned:
Edge mode implementation
Trusted backend mode
Hybrid mode with evaluation
