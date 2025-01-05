# Hospital RAG Chatbot

A **Retrieval-Augmented Generation (RAG)** chatbot for answering questions about a **fake hospital system**, leveraging:
- **Neo4j AuraDB** for both structured data (patients, visits, physicians, payers, etc.) and vector embeddings (patient reviews).
- **LangChain** to build modular chains, an agent, and tools for retrieving both structured and unstructured data.
- **FastAPI** for serving the agent as a REST endpoint.
- **Streamlit** for a user-friendly web interface.

This repository demonstrates:
1. **End-to-end AI chatbot design** with data loading (ETL), vector search for text data, and dynamic query generation for structured data.
2. **Docker Compose orchestration** for deploying the project’s three main services.
3. **Version control** best practices with Git.

---

## Table of Contents

1. [Architecture Overview](#architecture-overview)  
2. [Project Structure](#project-structure)  
3. [Prerequisites](#prerequisites)  
4. [Environment Setup](#environment-setup)  
5. [How to Run](#how-to-run)  
6. [Testing](#testing)  
7. [Usage Examples](#usage-examples)  
8. [Project Roadmap](#project-roadmap)  
9. [Contributing](#contributing)  
10. [License](#license)

---

## Architecture Overview

This project implements a **Graph RAG Chatbot**. The flow of data is:

1. **ETL to Neo4j (hospital_neo4j_etl)**:
   - Loads CSV data into Neo4j (e.g., `hospitals.csv`, `visits.csv`, etc.).
   - Sets up graph nodes and relationships.

2. **LangChain Agent (chatbot_api)**:
   - **Cypher Chain** for structured queries over Neo4j.
   - **Vector Chain** for searching patient reviews stored as embeddings in Neo4j.
   - **Wait Times** functions for real-time hospital wait times.
   - A single **LangChain Agent** decides which chain or function to call based on the user’s query.
   - Exposed via **FastAPI** at `http://localhost:8000/hospital-rag-agent`.

3. **Streamlit Frontend (chatbot_frontend)**:
   - Chat-like UI for end-users at `http://localhost:8501/`.
   - Sends POST requests to the FastAPI backend.

Everything is orchestrated via **Docker Compose**.

---

## Project Structure

```bash
hospital_rag_chatbot/
│
├── chatbot_api/
│   ├── src/
│   │   ├── agents/
│   │   │   └── hospital_rag_agent.py
│   │   ├── chains/
│   │   │   ├── hospital_cypher_chain.py
│   │   │   └── hospital_review_chain.py
│   │   ├── models/
│   │   │   └── hospital_rag_query.py
│   │   ├── tools/
│   │   │   └── wait_times.py
│   │   ├── utils/
│   │   │   └── async_utils.py
│   │   ├── entrypoint.sh
│   │   └── main.py
│   ├── Dockerfile
│   └── pyproject.toml
│
├── chatbot_frontend/
│   ├── src/
│   │   ├── entrypoint.sh
│   │   └── main.py
│   ├── Dockerfile
│   └── pyproject.toml
│
├── hospital_neo4j_etl/
│   ├── src/
│   │   ├── entrypoint.sh
│   │   └── hospital_bulk_csv_write.py
│   ├── Dockerfile
│   └── pyproject.toml
│
├── tests/
│   ├── async_agent_requests.py
│   └── sync_agent_requests.py
│
├── .env
├── docker-compose.yml
└── README.md
```

## Prerequisites
...