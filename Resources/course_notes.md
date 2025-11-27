# Course Notes - Key Topics Covered

## Retrieval-Augmented Generation (RAG) Systems

### Core Concepts
- **RAG Architecture**: Combines retrieval systems with generative AI to provide grounded, citation-backed answers
- **Two-Stage Retrieval**: Broad vector search (top-20) followed by precise re-ranking (top-5) improves accuracy
- **Context-Only Generation**: Enforcing "answer only from context" rules prevents hallucinations

### Key Components
1. **Retrieval Layer**
   - Dense vector search using embeddings
   - Similarity-based document retrieval
   - Top-k search algorithms

2. **Generation Layer**
   - LLM API integration for answer synthesis
   - Prompt engineering for factual accuracy
   - Citation enforcement in responses

3. **Interface Layer**
   - User-friendly UI design
   - Real-time query processing
   - Error handling and graceful degradation

---

## Vector Databases & Embeddings

### FAISS (Facebook AI Similarity Search)
- **Purpose**: Efficient similarity search in high-dimensional vector spaces
- **Index Types**: IVF-Flat for our implementation
- **Performance**: Enables fast retrieval from 5,000-10,000 document chunks
- **Storage**: Precomputed embeddings stored as .npy files or FAISS index

### Sentence Transformers
- **Model Used**: all-MiniLM-L6-v2
- **Alternative**: OpenAI text-embedding models
- **Function**: Converts text queries and documents into dense vector representations
- **Advantage**: Captures semantic meaning beyond keyword matching

---

## Evaluation Metrics

### Retrieval Metrics
- **Precision@5**: Proportion of top-5 chunks that are relevant
- **Recall@10**: Proportion of relevant chunks found in top-10
- **MRR (Mean Reciprocal Rank)**: Average inverse rank of first relevant chunk

### Generation Metrics
- **ROUGE-L**: Lexical overlap with reference answers
- **BERTScore**: Semantic similarity measurement
- **Citation Accuracy**: Correct source attribution rate (target: >90%)

### Performance Benchmarks
- **Target Latency**: Under 5 seconds per query
- **Baseline Comparison**: 15-25% improvement over BM25 keyword search
- **User Satisfaction**: 4.2-4.5 out of 5.0 rating

---

## Hybrid Retrieval Strategy

### Combination Approach
- **70% Semantic Search**: Dense vector similarity (FAISS)
- **30% Keyword Search**: BM25 algorithm for exact term matching
- **Result**: Handles both conceptual queries and specific terminology

### Why Hybrid Works
- Pure semantic search misses exact technical terms
- Pure keyword search misses conceptual relationships
- Combination achieves best of both worlds

---

## Prompt Engineering Techniques

### System Prompt Design
- **Instruction**: "Answer only from provided context"
- **Citation Requirement**: Include source metadata in responses
- **Temperature Setting**: 0.2-0.3 for factual accuracy (vs. 0.7+ for creativity)
- **Token Limits**: 300-500 tokens for responses, 4,000-8,000 for context

### Impact
- 20% improvement in citation accuracy
- Significant reduction in hallucinations
- More reliable academic use

---

## Python Fundamentals Applied

### Asynchronous Programming
- **asyncio module**: Concurrent task execution
- **Use Case**: Improved efficiency when waiting on API calls or I/O operations
- **Benefit**: Non-blocking operations for better performance

### Data Processing
- **NumPy**: Array mathematics and numerical operations
- **Pandas**: DataFrame manipulation, filtering, CSV operations
- **Application**: Processing and analyzing retrieved document chunks

### Database Operations
- **SQLite**: In-memory database for metadata storage
- **CRUD Operations**: Create, Read, Update, Delete records
- **Use Case**: Managing document metadata and query logs

### API Integration
- **HTTP Requests**: GET/POST operations for external services
- **JSON Processing**: Parsing API responses
- **Implementation**: OpenAI GPT-4 API calls, embedding generation APIs

---

## LangChain Framework

### Purpose
- Manages retrieval, prompts, and LLM calls in unified pipeline
- Simplifies complex RAG workflows
- Provides modular components for experimentation

### Components Used
- Retrieval chains for document search
- Prompt templates for consistent formatting
- LLM wrappers for API abstraction

---

## Cross-Encoder Reranking

### Model: ms-marco-MiniLM
- **Function**: Scores query-document pairs for relevance
- **Stage**: After initial retrieval, before generation
- **Tradeoff**: Adds 1-2 seconds latency but improves precision significantly
- **Decision**: Acceptable for educational applications prioritizing accuracy

---

## Data Preprocessing Best Practices

### Chunking Strategy
- **Structure-Aware Chunking**: Better than fixed-token splitting
- **Chunk Overlap**: 50-100 tokens for context continuity
- **Optimal Size**: Balances context richness with retrieval precision

### Metadata Tagging
- Course week number
- Document type (syllabus, assignment, lecture notes)
- Source filename
- **Purpose**: Enables precise citations and filtering

---

## Deployment Considerations

### Environment
- **Primary**: Google Colab (GPU: T4/A100 optional)
- **Python Version**: 3.10.x
- **Compute**: CPU works for evaluation; GPU speeds up training

### Reproducibility
- **Seed**: 42 (fixed across NumPy and PyTorch)
- **Determinism**: Disabled non-deterministic CUDA operations
- **Documentation**: All experiments logged with configs and timestamps

---

## Error Analysis Framework

### Error Categories Identified
1. **Retrieval Failures**: Relevant docs not retrieved (solution: hybrid approach)
2. **Irrelevant Retrieval**: Non-relevant chunks included (solution: reranking)
3. **Generation Errors**: Hallucination or incorrect synthesis (solution: stricter prompts)
4. **Citation Failures**: Missing/incorrect sources (solution: metadata formatting)

### Debugging Techniques
- t-SNE/UMAP visualization of embeddings
- Comparison with BM25 baseline on problematic queries
- A/B testing of prompt variations

---

## Performance Optimization

### Caching Strategy
- Cache frequent queries to reduce latency
- Store precomputed embeddings (not recomputed per query)
- Warm cache improves p50/p95 latency metrics

### Context Compression
- Summarize retrieved chunks when context exceeds token limits
- Reduces noise while maintaining key information

### Query Expansion
- Automatically rephrase queries to improve retrieval coverage
- Handles variations in how users ask questions
