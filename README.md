# Deploy and Host RAG API with Railway

Document Q&A with PostgreSQL pgvector — ingest text, embed with OpenAI, ask questions grounded in your data.

## About RAG API

A FastAPI retrieval-augmented generation service. Upload documents via `/v1/ingest`, store embeddings in PostgreSQL with pgvector, then ask questions via `/v1/ask` and get answers based on your content.

## About Hosting RAG API

Self-hosting on Railway gives you a private Postgres instance with vector search, plus an API that keeps your OpenAI key server-side. Attach a volume so ingested documents survive redeploys.

## Environment Variables

| Variable | Description | Secret | Example/Notes |
| --- | --- | --- | --- |
| `DATABASE_URL` | PostgreSQL with pgvector | Yes | `${{Postgres.DATABASE_URL}}` |
| `OPENAI_API_KEY` | OpenAI key for embeddings + chat | Yes | User-supplied at deploy time |
| `OPENAI_EMBEDDING_MODEL` | Embedding model | No | `text-embedding-3-small` |
| `OPENAI_CHAT_MODEL` | Model for answers | No | `gpt-4o-mini` |
| `API_KEY` | Protects ingest/ask routes | Yes | `${{secret(32)}}` — send as `X-API-Key` |

## Deploy and Host

1. Deploy this repo from GitHub on Railway.
2. Add **PostgreSQL** and attach a **volume**.
3. The app runs `CREATE EXTENSION vector` on startup. If that fails, use a Postgres image with pgvector preinstalled.
4. Set all environment variables above on the API service.
5. Enable **public HTTP** and deploy.
6. Ingest a document, then ask a question:

```bash
curl -X POST https://YOUR-URL/v1/ingest \
  -H "Content-Type: application/json" \
  -H "X-API-Key: YOUR_KEY" \
  -d "{\"source\":\"faq\",\"content\":\"Returns allowed within 30 days.\"}"

curl -X POST https://YOUR-URL/v1/ask \
  -H "Content-Type: application/json" \
  -H "X-API-Key: YOUR_KEY" \
  -d "{\"question\":\"What is the refund policy?\"}"
```

## Common Use Cases

- FAQ and docs Q&A for products and internal wikis
- Support bots grounded in your knowledge base
- Prototypes exploring RAG before a full vector stack
- Small document collections without managed vector DB costs

## Dependencies for RAG API Hosting

The Railway template includes:

- **RAG API** — this GitHub repo (FastAPI + Uvicorn)
- **PostgreSQL** — Railway PostgreSQL plugin with pgvector extension

## Deployment Dependencies

- [pgvector documentation](https://github.com/pgvector/pgvector)
- [OpenAI embeddings docs](https://platform.openai.com/docs/guides/embeddings)
- [Railway PostgreSQL docs](https://docs.railway.com/databases/postgresql)

## Why Deploy RAG API on Railway?

Postgres + pgvector and your RAG API in one project — private networking, persistent storage, and a single HTTPS endpoint for ingest and ask.

## Template Content

| Service | Source |
| --- | --- |
| RAG API | GitHub repo (this template) |
| Postgres | Railway PostgreSQL plugin |

## Run locally

Requires local Postgres with pgvector.

```bash
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
copy .env.example .env
uvicorn app.main:app --reload --port 8000
```

## Author

romeoxt — herbylegall9@gmail.com

## License

MIT
