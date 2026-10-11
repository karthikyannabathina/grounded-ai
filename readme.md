# 🧠 Grounded AI

> A local-first RAG and document intelligence platform that answers **only from your documents**, and refuses when the documents don't support an answer.

[![Live Demo](https://img.shields.io/badge/Live%20Demo-grounded--ai--mu.vercel.app-brightgreen)](https://grounded-ai-mu.vercel.app)
![Stack](https://img.shields.io/badge/stack-Node.js%20%7C%20TypeScript%20%7C%20React%20%7C%20Ollama-blue)
![Status](https://img.shields.io/badge/status-active%20development-orange)

**🔗 Live Demo:** [grounded-ai-mu.vercel.app](https://grounded-ai-mu.vercel.app)

---

## 🎯 The Problem

LLMs sound confident even when they are wrong. For document Q&A (policies, manuals, reports), a made-up answer is worse than no answer.

Grounded AI treats this as a retrieval and verification problem, not a prompting problem. Every answer must be backed by retrieved source text, and questions the documents can't answer are **refused** instead of guessed.

---

## ✨ Highlights

- 📄 PDF and Markdown ingestion with intelligent chunking
- 🧠 Local Hugging Face embeddings (no paid API needed)
- 🔀 Hybrid retrieval: vector similarity + BM25 keyword search
- 🎯 Cross-encoder reranking
- 🛡️ Relevance gate that stops low-confidence queries before they reach the LLM
- ✅ Answer-grounding verification on every generated answer
- 🚫 Refusal for unsupported questions
- 📊 Built-in evaluation pipeline with measurable metrics
- 🏠 Local-first: runs on your machine with Ollama

---

## 📊 Evaluation

The project ships with an evaluation pipeline that measures retrieval, reranking, grounding and refusal behaviour.

| Metric | Result |
|---|---|
| Overall Accuracy | 90.0% |
| Grounding Accuracy | 86.7% |
| Refusal Accuracy | 100% |
| Answer Grounding | 100% |
| Unsupported Claim Rate | 0% |
| Recall@5 | 100% |
| MRR | 1.000 |
| Reranker Survival | 100% |

**Breakdown**
- Retrieval hits: 15 / 15
- Reranker hits: 15 / 15
- Grounded answers: 13 / 13
- Unsupported claims: 0 / 7

> 📝 **Note:** The evaluation set is small and curated. These numbers show the pipeline behaves correctly on known cases, not production-scale accuracy. Expanding the dataset is on the roadmap.

Run it yourself:

```bash
cd backend
npm run evaluate
```

---

## 🏗️ Architecture

### Ingestion

```
PDF / Markdown
      │
      ▼
 Parse & clean
      │
      ▼
 Chunking
      │
      ▼
 Local embeddings (Hugging Face)
      │
      ▼
 Vector index  +  BM25 index
```

### Query pipeline

```
User question
      │
      ▼
 Query normalization
      │
      ▼
 Hybrid retrieval  ──  Vector search + BM25
      │
      ▼
 Cross-encoder reranker
      │
      ▼
 Relevance gate ───── low confidence ───►  🚫 Refuse
      │
      ▼
 LLM answer generation (Ollama)
      │
      ▼
 Answer-grounding check ── unsupported ──►  🚫 Refuse
      │
      ▼
 ✅ Grounded answer
```

### Why each stage exists

| Stage | Why it's there |
|---|---|
| **Vector search** | Finds passages that mean the same thing, even with different wording |
| **BM25** | Catches exact terms, names and numbers that embeddings often miss |
| **Reranker** | Re-scores the combined candidates with a stronger model for better top results |
| **Relevance gate** | Stops the system from answering when nothing relevant was retrieved |
| **Grounding check** | Verifies the generated answer is supported by the retrieved text |

---

## 🛠️ Tech Stack

| Layer | Technologies |
|---|---|
| **Backend** | Node.js, TypeScript, Express.js |
| **LLM** | Ollama (local models) |
| **ML / Retrieval** | Hugging Face Transformers, LangChain, BM25, cross-encoder reranking |
| **Documents** | PDF parsing, Markdown ingestion |
| **Frontend** | React, Vite |
| **Tools** | Git, GitHub, npm |

---

## 📁 Project Structure

```
grounded-ai/
├── backend/
│   ├── src/
│   │   ├── api/          # Express routes
│   │   ├── embeddings/   # Local embedding generation
│   │   ├── evaluation/   # Evaluation pipeline
│   │   ├── ingestion/    # Parsing and chunking
│   │   └── retrieval/    # Hybrid retrieval, reranking, gating
│   ├── documents/        # Source documents
│   ├── data/
│   │   └── evaluation/   # Evaluation datasets
│   ├── uploads/
│   ├── package.json
│   └── .env.example
│
├── frontend/             # React + Vite UI
├── .gitignore
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites
- Node.js 18+
- npm
- [Ollama](https://ollama.com) installed and running

### 1. Clone

```bash
git clone https://github.com/karthikyannabathina/grounded-ai.git
cd grounded-ai
```

### 2. Pull an Ollama model

```bash
ollama pull <your-model-name>
```

### 3. Install and configure the backend

```bash
cd backend
npm install
cp .env.example .env
```

Edit `.env` and fill in the required values.

### 4. Build the index

```bash
npm run ingest   # replace with your actual indexing script name
```

### 5. Start the backend

```bash
npm run dev
```

### 6. Start the frontend

```bash
cd ../frontend
npm install
npm run dev
```

### 7. Run the evaluation

```bash
cd ../backend
npm run evaluate
```

---

## 🔮 Roadmap

- [ ] Persistent vector database
- [ ] Streaming answers
- [ ] Source citation UI (show the exact passages behind each answer)
- [ ] Document management dashboard
- [ ] Multi-document collections
- [ ] Conversation history
- [ ] Larger evaluation dataset
- [ ] Production deployment
- [ ] Authentication and authorization

---

## 📌 Status

Actively developed. The core pipeline is working: hybrid retrieval, reranking, relevance gating, answer-grounding verification and automated evaluation.

---

## 👨‍💻 Author

**Karthik Yannabathina**
Full Stack Developer (MERN), final-year B.Tech CSE student, building AI/RAG systems.

[![Portfolio](https://img.shields.io/badge/Portfolio-000000?style=for-the-badge&logo=vercel&logoColor=white)](https://karthikyannabathina-portfolio.vercel.app/)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/karthik-yannabathina/)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/karthikyannabathina)
[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:karthikyannabathina4444@gmail.com)

⭐ If you found Grounded AI useful, consider starring the repo.
