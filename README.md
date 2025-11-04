# Automated RAG System with Google Drive, Gemini, Pinecone & ElevenLabs

![n8n](https://img.shields.io/badge/n8n-workflow-FF6B00?style=for-the-badge&logo=n8n&logoColor=white)
![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)

## Description
This n8n workflow implements a **fully automated Retrieval-Augmented Generation (RAG)** system designed to fetch, embed, store, and retrieve knowledge documents for intelligent question answering.  

It integrates **Google Drive**, **Google Gemini**, **Pinecone**, and **ElevenLabs** to form a seamless data ingestion and query-answering loop — automatically updating vector embeddings and returning spoken AI responses.

> [!NOTE]
> The workflow automates document ingestion, vectorization, retrieval, and voice-based AI responses without human intervention.

---

## Features
- **Automated document ingestion** from Google Drive on a schedule.
- **Vector embedding generation** using **Google Gemini embeddings**.
- **Contextual retrieval** from **Pinecone** vector database.
- **AI Agent-powered Q&A** using Gemini chat model.
- **Voice-based answer delivery** via **ElevenLabs** webhook integration.

---

## Prerequisites
- An active **n8n** instance (self-hosted or n8n.cloud).  
- **Google Drive API credentials** (OAuth2).  
- **Pinecone API key** with a configured index (e.g., `solar-panel-and-ev-charger-installers`).  
- **Google Gemini API key** (through the Google PaLM credentials in n8n).  
- **ElevenLabs API key** (for text-to-speech conversion).  

---

## Setup & Installation

### 1. Download or copy the workflow JSON
Download the file [`ElevenLabs Solar Panels RAG.json`](./ElevenLabs Solar Panels RAG.json) or copy its raw JSON content.

### 2. Import into n8n
1. Open your n8n dashboard.  
2. Click **Import → Paste JSON** or **Upload File**.  
3. Paste or upload the JSON and click **Import**.  
4. Once imported, check each node for credentials.

> [!WARNING]
> Make sure all API credentials (Google, Pinecone, ElevenLabs) are correctly configured before activating.

### 3. Activate the workflow

1. Open the workflow in n8n.  
2. Toggle **Active** to enable automatic runs.  
3. The **Schedule Trigger** will handle regular updates automatically.

---

## Configuration & Credentials

### Required Credentials
| Node | Credential | Description |
|------|-------------|-------------|
| Google Drive | `googleDriveOAuth2Api` | Access to files/folders for ingestion. |
| Pinecone Vector Store | `pineconeApi` | Vector database for embeddings. |
| Gemini Embeddings & Chat | `googlePalmApi` | Used by Gemini for embeddings and LLM queries. |
| ElevenLabs | Webhook endpoint | Used to deliver AI-generated speech responses. |

### Environment Variables (if applicable)
```bash
GOOGLE_API_KEY="your-google-api-key"
PINECONE_API_KEY="your-pinecone-api-key"
ELEVENLABS_API_KEY="your-elevenlabs-api-key"
GOOGLE_DRIVE_FOLDER_ID="1q2z0VGGVsDNMzpqpayXozFtKS2m8iiew"

Workflow Visual & Breakdown

### Diagram

![Workflow Diagram](./images/ElevenLabs Voice RAG Customer Support Agent.jpg)

### Data Flow

Input → Process → Output

1. Input
- The Schedule Trigger initiates periodic runs.
- Google Drive Node searches for files within the specified folder and downloads them.

2. Process
- Files are converted into embeddings using Google Gemini Embedding Node.
- Embeddings are inserted into Pinecone Vector Store (solarpanelsdocs namespace).
- On a separate execution path, a Webhook receives user questions.
- The AI Agent retrieves relevant context via Pinecone and uses Gemini Chat Model to craft a response.

3. Output
- The response is sent to ElevenLabs, which converts it into an audio file and delivers it as the final response.

