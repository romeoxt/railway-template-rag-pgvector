# Railway Template Composer Setup

## Marketplace listing

- **Title:** Deploy and Host RAG API with Railway
- **Short description:** Document Q&A with PostgreSQL pgvector — ingest text, ask questions grounded in your data.
- **Category:** AI
- **Overview:** paste `README.md`

## Services

| Service | Source | Volume | Public HTTP |
| --- | --- | --- | --- |
| RAG API | GitHub repo (this folder) | — | Yes |
| Postgres | Railway PostgreSQL plugin | `/var/lib/postgresql/data` | No |

## Variables — RAG API

| Variable | Value | Secret | Description |
| --- | --- | --- | --- |
| `DATABASE_URL` | `${{Postgres.DATABASE_URL}}` | Yes | Postgres with pgvector |
| `OPENAI_API_KEY` | (user supplied) | Yes | Embeddings + chat |
| `OPENAI_EMBEDDING_MODEL` | `text-embedding-3-small` | No | Embedding model |
| `OPENAI_CHAT_MODEL` | `gpt-4o-mini` | No | Answer model |
| `API_KEY` | `${{secret(32)}}` | Yes | `X-API-Key` for ingest/ask |

## Settings — RAG API

- Healthcheck: `/health`
- App runs `CREATE EXTENSION vector` on startup
- Attach volume to PostgreSQL
