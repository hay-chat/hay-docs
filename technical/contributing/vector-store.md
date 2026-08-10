---
layout: docs.njk
title: Vector Store
description: Vector store implementation for document embeddings
section: technical
navGroup: Contributing
navOrder: 4
---

# Vector Store Implementation Guide

## ✅ Implementation Complete

This implementation provides pgvector support with multi-tenant vector storage and similarity search for your TypeORM + PostgreSQL application.

This document describes the pgvector integration for embedding storage and similarity search, implemented via raw SQL through TypeORM's `AppDataSource.query` and a custom `llmProviderFactory` for generating embeddings.

## Setup

### 1. Environment Variables

Add the following to your `.env` file:

```env
# Required
OPENAI_API_KEY=your-openai-api-key

# Optional (standard config keys — defaults shown)
EMBEDDING_DIM=1536  # Default: 1536 (dimensions for text-embedding-3-small)
OPENAI_EMBEDDING_MODEL=text-embedding-3-small  # Default: text-embedding-3-small

# Database (should already be configured)
DB_HOST=your-postgres-host
DB_PORT=5432
DB_USERNAME=your-username
DB_PASSWORD=your-password
DB_NAME=your-database
```

### 2. Run Migrations

```bash
# Run the embeddings table migration
npm run migration:run
```

## Usage

### Basic Usage in Code

```typescript
import { vectorStoreService } from "./services/vector-store.service";

// Initialize (after DataSource is ready)
await vectorStoreService.initialize();

// Add embeddings
const chunks = [
  { content: "First chunk of text", metadata: { source: "doc1" } },
  { content: "Second chunk of text", metadata: { source: "doc1" } },
];

const ids = await vectorStoreService.addChunks(
  organizationId,
  documentId, // optional
  chunks,
);

// Search for similar content
const results = await vectorStoreService.search(
  organizationId,
  "search query",
  10, // top K results
);
```

## Architecture

### Database Schema

The `embeddings` table structure:

- `id`: UUID primary key
- `organization_id`: UUID for multi-tenancy
- `document_id`: Optional UUID linking to documents
- `page_content`: The text content (TypeScript: `pageContent`)
- `metadata`: JSONB for additional data
- `embedding`: vector(1536) for similarity search
- `created_at`: Timestamp, auto-generated

### Indexes

1. **B-tree index** on `organization_id` for filtering
2. **HNSW index** (`embeddings_embedding_hnsw_idx`) on `embedding` using cosine distance for similarity search (`m=16, ef_construction=64`)

### Performance Optimization

For large-scale deployments:

1. **Partial HNSW indexes** per large organization:

```sql
CREATE INDEX embeddings_embedding_hnsw_org_xyz
ON embeddings USING hnsw (embedding vector_cosine_ops)
WHERE "organization_id" = 'specific-org-uuid';
```

2. **LIST partitioning** for massive scale

## API Reference

### VectorStoreService Methods

#### `initialize()`

Initialize the vector store. Must be called after DataSource is initialized.

#### `addChunks(organizationId, docId, chunks)`

Add text chunks to the vector store.

- Returns: Array of embedding IDs

#### `searchDocuments(organizationId, query, k)`

Search for similar documents within an organization. Groups results by document and returns the best (highest) similarity score per document.

- Returns: `Array<{ documentId, similarity }>`

#### `search(organizationId, query, k)`

Search for similar content within an organization.

- Returns: Array of results with similarity scores

#### `deleteByDocumentId(orgId, docId, manager?: EntityManager)`

Delete all embeddings for a document.

- Returns: Number of deleted rows

#### `deleteByOrganizationId(orgId: string, manager?: EntityManager)`

Delete all embeddings for an organization (GDPR compliance).

#### `deleteByConversationIds(orgId: string, conversationIds: string[], manager?: EntityManager)`

Delete all embeddings associated with the given conversation IDs (GDPR compliance).

#### `deleteByMessageIds(orgId: string, messageIds: string[], manager?: EntityManager)`

Delete all embeddings associated with the given message IDs (GDPR compliance).

#### `findByConversationIds(orgId: string, conversationIds: string[])`

Find all embeddings associated with the given conversation IDs.

#### `findByMessageIds(orgId: string, messageIds: string[])`

Find all embeddings associated with the given message IDs.

#### `getStatistics(orgId)`

Get embedding statistics for an organization.

- Returns: Statistics object

## Switching Embedding Models

To use a different embedding model:

1. Update `.env`:

```env
OPENAI_EMBEDDING_MODEL=text-embedding-3-large
EMBEDDING_DIM=3072  # For large model
```

2. Create a new migration to update the vector dimension:

```sql
ALTER TABLE embeddings
ALTER COLUMN embedding TYPE vector(3072);
```

## Switching Distance Metrics

Currently using cosine distance (default). To switch:

### For L2 (Euclidean) distance:

```sql
-- Drop old index
DROP INDEX embeddings_embedding_hnsw_idx;

-- Create new index with L2
CREATE INDEX embeddings_embedding_hnsw_idx
ON embeddings USING hnsw (embedding vector_l2_ops)
WITH (m = 16, ef_construction = 64);

-- Update queries to use <-> operator
```

### For Inner Product:

```sql
-- Create index with inner product
CREATE INDEX embeddings_embedding_hnsw_idx
ON embeddings USING hnsw (embedding vector_ip_ops)
WITH (m = 16, ef_construction = 64);

-- Update queries to use <#> operator
```

## Troubleshooting

### Extension not found

```sql
CREATE EXTENSION IF NOT EXISTS vector;
CREATE EXTENSION IF NOT EXISTS pgcrypto;
```

### Performance issues

1. Check index usage: `EXPLAIN ANALYZE your_query`
2. Consider partial indexes for large orgs
3. Monitor `lists` and `ef_construction` HNSW parameters

### Multi-tenancy concerns

- Always filter by `organizationId` first
- Consider partitioning for 1000+ organizations

## Testing

Run the integration tests:

```bash
cd server && npm test -- tests/integration/vector-store.test.ts
```

There is also `tests/services/vector-store-embedding.test.ts` which covers embedding-specific unit tests.

## Security Considerations

1. **Multi-tenancy**: All queries are scoped by `organizationId`
2. **API Keys**: Store OpenAI keys securely
3. **Data deletion**: Cascade delete with documents

## Resources

- [pgvector documentation](https://github.com/pgvector/pgvector)
- [OpenAI embeddings](https://platform.openai.com/docs/guides/embeddings)
