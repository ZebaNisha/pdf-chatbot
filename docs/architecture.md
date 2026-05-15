

Deployment Architecture
Development
Frontend → Vercel
Backend → Railway
DB → Supabase
Vector DB → Qdrant Cloud
Production
Frontend → CDN
Backend → Kubernetes Cluster
Queue → Redis Cluster
DB → Managed PostgreSQL
Vector DB → Distributed Qdrant
Storage → AWS S3


2. Core Features
MVP Features
Authentication
User signup/login
JWT authentication
Session management
PDF Upload
Upload single/multiple PDFs
Drag-and-drop UI
File validation
Upload progress
PDF Processing
Extract text from PDFs
Handle scanned PDFs (future OCR support)
Metadata extraction
AI Search & Chat
Ask natural language questions
Conversational memory
Multi-turn chat
RAG Pipeline
Chunk documents
Generate embeddings
Store vectors
Retrieve relevant chunks
Generate grounded answers
Citations
Show source paragraphs/pages
Highlight referenced sections
Multi-Document Support
Chat with:
Single document
Folder/workspace
Entire knowledge base
Chat History
Save conversations
Resume chats later
Dashboard
Uploaded documents
Processing status
Recent chats

Engineering Notes for Antigravity / IDE
Development Principles
MUST FOLLOW
Modular architecture
Service separation
Async processing
Environment-based configs
Clean code practices
API-first design
Code Standards
Type-safe APIs
Pydantic validation
Proper error handling
Structured logging
AI Engineering Standards
Retrieval before generation
Source-grounded answers
Prevent hallucinations
Scalable embedding pipeline