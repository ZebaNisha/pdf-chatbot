

1. Project Overview
PDF BOT

AI PDF Chatbot (Scalable RAG Platform)

Vision

Build a scalable AI-powered document intelligence platform where users can upload PDFs and interact with them using natural language through Retrieval-Augmented Generation (RAG).

The system should support:

Multi-document chat
Context-aware responses
Citation-based answers
Fast retrieval
Scalable architecture
Future enterprise expansion
Goal

Enable users to:

Upload PDFs
Extract and index document knowledge
Ask questions conversationally
Receive accurate answers with sources


TECH STACK

| Layer          | Technology             |
| -------------- | ---------------------- |
| Frontend       | React / Next.js        |
| Styling        | Tailwind CSS           |
| Backend        | FastAPI                |
| Authentication | JWT / Clerk / Auth0    |
| Database       | PostgreSQL             |
| Vector DB      | Qdrant                 |
| Queue          | Celery + Redis         |
| Embeddings     | text-embedding-3-small |
| LLM            | GPT-4.1 / Claude       |
| PDF Parsing    | PyMuPDF                |
| OCR            | Tesseract (future)     |
| Object Storage | AWS S3                 |
| Deployment     | Docker + Kubernetes    |
| Monitoring     | Prometheus + Grafana   |
| Logging        | ELK Stack              |


4. System Architecture
High-Level Architecture
                    ┌─────────────────────┐
                    │     Frontend        │
                    │ React / Next.js     │
                    └─────────┬───────────┘
                              │ HTTPS
                              ▼
                    ┌─────────────────────┐
                    │    API Gateway      │
                    │ FastAPI Backend     │
                    └─────────┬───────────┘
                              │
        ┌─────────────────────┼─────────────────────┐
        ▼                     ▼                     ▼

┌──────────────┐     ┌────────────────┐    ┌────────────────┐
│ Auth Service │     │ Document Queue │    │ Chat Service   │
└──────────────┘     └────────────────┘    └────────────────┘
                              │
                              ▼
                    ┌─────────────────────┐
                    │ PDF Processing      │
                    │ Chunking Pipeline   │
                    └─────────┬───────────┘
                              │
                              ▼
                    ┌─────────────────────┐
                    │ Embedding Service   │
                    └─────────┬───────────┘
                              │
             ┌────────────────┼────────────────┐
             ▼                                 ▼

   ┌───────────────────┐           ┌───────────────────┐
   │ Vector Database   │           │ PostgreSQL        │
   │ Chroma / Qdrant   │           │ Metadata Storage  │
   └───────────────────┘           └───────────────────┘
                              │
                              ▼
                    ┌─────────────────────┐
                    │ LLM Provider        │
                    │ OpenAI / Claude     │
                    └─────────────────────┘


FOLDER STRUCTURE

project/
│
├── frontend/
│
├── backend/
│   ├── api/
│   ├── services/
│   ├── workers/
│   ├── rag/
│   ├── models/
│   ├── db/
│   └── utils/
│
├── infrastructure/
│   ├── docker/
│   ├── kubernetes/
│   └── terraform/
│
└── docs/



5. Scalable Architecture Strategy
Why Scalability Matters

The system must support:

Thousands of PDFs
Concurrent users
Large document collections
Long conversations
Enterprise workloads
Scalability Decisions
Microservice-Oriented Design

Separate services for:

Auth
Upload
Processing
Retrieval
Chat

This prevents bottlenecks.

Asynchronous Processing

PDF processing should NEVER happen synchronously.

Use:

Celery
Redis Queue
RabbitMQ

Flow:

Upload PDF
    ↓
Add job to queue
    ↓
Worker processes PDF
    ↓
Embeddings stored
    ↓
Document marked ready
Vector Database Strategy
Development
ChromaDB
Production
Qdrant
Weaviate
Pinecone

Reason:

Better indexing
Horizontal scaling
Distributed retrieval
Storage Strategy
PDFs

Store in:

AWS S3
Cloudflare R2
Azure Blob

NOT inside database.

Horizontal Scaling

Containerize using:

Docker

Deploy using:

Kubernetes
ECS
Railway (initial)
Caching Layer

Use Redis for:

Chat cache
Session cache
Query caching
Rate limiting


9. API Design
Authentication
POST /auth/register
{
  "name": "John",
  "email": "john@example.com",
  "password": "123456"
}
POST /auth/login
Document APIs
POST /documents/upload

Upload PDF.

GET /documents

List user documents.

DELETE /documents/{id}

Delete document.

Chat APIs
POST /chat
{
  "message": "Summarize chapter 2",
  "document_ids": ["doc_123"]
}

Response:

{
  "answer": "Chapter 2 discusses...",
  "sources": [
    {
      "page": 12,
      "text": "..."
    }
  ]
}
GET /chat/history/{chat_id}

Retrieve conversation.

10. Database Schema
Users
Field	Type
id	UUID
name	String
email	String
password_hash	String
created_at	Timestamp
Documents
Field	Type
id	UUID
user_id	UUID
filename	String
storage_url	String
status	Enum
created_at	Timestamp
ChunksMetadata
Field	Type
id	UUID
document_id	UUID
chunk_index	Integer
page_number	Integer
text	Text
ChatHistory
Field	Type
id	UUID
user_id	UUID
message	Text
response	Text
created_at	Timestamp
