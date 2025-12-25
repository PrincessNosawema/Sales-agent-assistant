# 📂 DocuFetch: Enterprise RAG Agent & ETL Pipeline

**DocuFetch** is a high-performance Retrieval-Augmented Generation (RAG) system built on **n8n**. It automates the entire lifecycle of company knowledge—from the moment a document is dropped into Google Drive to the moment an employee asks a complex question in Slack.

By leveraging **Google Gemini 2.5 Pro** for reasoning and **Pinecone** for vector storage, DocuFetch provides a "human-in-the-loop" feel with institutional memory.
[Process Flow](docufetch-system-architecture.png)

## 🎥 Demo Video

> *[https://www.loom.com/share/8a7bea8d60314eb3ae03c08232b96ad8]*

## 📸 Workflow

> *[DocuFetch Screenshot.png]*

## 🚀 Key Features

*   **Automated Knowledge Ingestion (ETL):** A specialized pipeline that polls Google Drive for new files, downloads them, and processes them for the vector database.
*   **Advanced Text Chunking:** Utilizes a Recursive Character Text Splitter with a 220-character overlap to ensure semantic context is preserved across chunks.
*   **High-Reasoning LLM:** Powered by Google Gemini 2.5 Pro, enabling the agent to handle nuanced internal queries with a warm, professional "team-member" persona.
*   **Conversational Memory:** Implements a Window Buffer Memory (last 3 interactions) so the bot understands follow-up questions and maintains context.
*   **Loop Prevention Logic:** A custom "Ignore Bot" gate ensures the system doesn't trigger itself in Slack, maintaining stability and reducing API costs.

## 🛠️ Tech Stack

| Component | Technology |
| :--- | :--- |
| **Orchestration** | n8n |
| **LLM (Reasoning)** | Google Gemini 2.5 Pro |
| **Vector Database** | Pinecone |
| **Embeddings** | Google Gemini Embedding-001 |
| **Data Sources** | Google Drive API |
| **Communication** | Slack API |

## 🏗️ Workflow Architecture

The system is divided into two primary loops:

### 1. The Ingestion Loop (ETL)
Every minute, the agent "watches" a specific Google Drive folder.
*   **Trigger:** New file detected in "Office Docs".
*   **Transform:** Text is extracted and split into optimized segments.
*   **Embed:** Gemini generates high-dimensional vectors for the text.
*   **Upsert:** Data is stored in the `documentknowledge` Pinecone index.

### 2. The Retrieval Loop (Query)
When a user sends a message in the `#random` (or designated) Slack channel:
*   **Filtering:** The "Ignore Bot" node validates the user to prevent loops.
*   **Reasoning:** The Gemini 2.5 Pro Agent analyzes the intent.
*   **Retrieval:** The agent calls the `vector_store_retriever` tool to pull relevant facts from Pinecone.
*   **Response:** A conversational, brand-aligned answer is sent back to Slack.

## 🧠 Prompt Engineering

The agent is configured with a sophisticated system prompt that enforces:
*   **Internal Awareness:** The bot speaks as a Princess Osas Limited employee (e.g., "We have..." instead of "The company has...").
*   **Source Attribution:** Naturally citing documents (e.g., "According to the Employee Handbook...").
*   **Strict Guardrails:** Prevention of hallucinations by strictly limiting answers to the provided knowledge base.

## 📥 Installation & Setup

1.  **Import to n8n:** Download the `DocuFetch_Company_RAG_Agent.json` and import it into your n8n instance.
2.  **Credentials:** Configure the following credentials:
    *   Google Drive OAuth2
    *   Google Gemini (PaLM) API
    *   Pinecone API
    *   Slack API
3.  **Environment Variables:** Update the `folderToWatch` ID and `pineconeIndex` name to match your environment.
4.  **Activate:** Toggle the workflow to 'Active'.

## 📈 Impact

*   **Zero-Touch Maintenance:** Documentation updates in real-time without manual database entries.
*   **Reduced Slack Noise:** Employees get instant answers to policy questions without tagging HR/Management.
*   **Scalable Knowledge:** Handles up to thousands of document chunks with sub-second retrieval times.
