---
layout: docs.njk
title: Vector Store
description: Vector store implementation for document embeddings
section: technical
navGroup: Contributing
navOrder: 4
---

# Vector Store Implementation Guide

## Implementation Complete

This implementation provides pgvector support with multi-tenant vector storage and similarity search for your TypeORM + PostgreSQL application.

This document describes the pgvector and OpenAI SDK integration for embedding storage and similarity search.

## Setup

### 1. Environment Variables

Add the following to your `.env` file:

```env
# Required
OPENAI_API_KEY=your-openai-api-key
EMBEDDING_DIM=1536  # Default dimensions for text-embedding-3-small

# Database (should already be configured)
DB_HOST=your-db-host
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
- `page_content`: The text content (entity field: `pageContent`)
- `metadata`: JSONB for additional data
- `embedding`: vector(1536) for similarity search
- `created_at`: Timestamp, auto-set on creation (entity field: `createdAt`)

### Indexes

1. **B-tree index** on `organization_id` for filtering
2. **HNSW index** on `embedding` using cosine distance (m = 16, ef_construction = 64) for similarity search

### Performance Optimization

For large-scale deployments:

1. **Partial HNSW indexes** per large organization:

```sql
CREATE INDEX embeddings_embedding_hnsw_org_xyz
ON embeddings USING hnsw (embedding vector_cosine_ops)
WITH (m = 16, ef_construction = 64)
WHERE "organization_id" = 'specific-org-uuid';
```

2. **LIST partitioning** for massive scale (see migration comments)

## API Reference

### VectorStoreService Methods

#### `initialize()`

Initialize the vector store. Must be called after DataSource is initialized.

#### `addChunks(organizationId, docId, chunks)`

Add text chunks to the vector store.

- Returns: Array of embedding IDs

#### `search(organizationId, query, k)`

Search for similar content within an organization.

- Returns: Array of results with similarity scores

#### `searchDocuments(organizationId, query, k)`

Search for documents by vector similarity, grouped by document. Returns the best similarity score per document (deduplicates chunks). Internally retrieves a candidate pool of nearest chunks via the HNSW index, then aggregates by `document_id`.

- `organizationId` - Organization ID to filter results
- `query` - Search query text
- `k` - Number of documents to return (default: 10)
- Returns: Array of `{ documentId, similarity }` sorted by similarity descending

#### `deleteByDocumentId(organizationId, docId, manager?)`

Delete all embeddings for a document.

- `manager` - Optional `EntityManager` for transactional operations
- Returns: Number of deleted rows

#### `deleteByOrganizationId(orgId, manager?)`

Delete all embeddings for an organization. Use with caution.

- `manager` - Optional `EntityManager` for transactional operations
- Returns: Number of deleted rows

#### `deleteByConversationIds(orgId, conversationIds, manager?)`

Delete embeddings whose metadata contains any of the given conversation IDs (GDPR erasure).

- `orgId` - Organization ID for security filtering
- `conversationIds` - Array of conversation IDs
- `manager` - Optional `EntityManager` for transactional operations
- Returns: Number of deleted rows

#### `deleteByMessageIds(orgId, messageIds, manager?)`

Delete embeddings whose metadata contains any of the given message IDs (GDPR erasure).

- `orgId` - Organization ID for security filtering
- `messageIds` - Array of message IDs
- `manager` - Optional `EntityManager` for transactional operations
- Returns: Number of deleted rows

#### `findByConversationIds(orgId, conversationIds)`

Find embeddings by conversation IDs (GDPR export). Returns embeddings where `metadata->>'conversationId'` matches any of the given IDs.

- `orgId` - Organization ID for security filtering
- `conversationIds` - Array of conversation IDs
- Returns: Array of `{ id, pageContent, metadata, createdAt? }`

#### `findByMessageIds(orgId, messageIds)`

Find embeddings by message IDs (GDPR export). Returns embeddings where `metadata->>'messageId'` matches any of the given IDs.

- `orgId` - Organization ID for security filtering
- `messageIds` - Array of message IDs
- Returns: Array of `{ id, pageContent, metadata, createdAt? }`

#### `getStatistics(organizationId)`

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
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
```

### Performance issues

1. Check index usage: `EXPLAIN ANALYZE your_query`
2. Consider partial indexes for large orgs
3. Monitor `m` and `ef_construction` HNSW parameters

### Multi-tenancy concerns

- Always filter by `organization_id` first
- Consider partitioning for 1000+ organizations

## Testing

Run the integration tests:

```bash
npm test -- tests/integration/vector-store.test.ts
```

## Security Considerations

1. **Multi-tenancy**: All queries are scoped by `organization_id`
2. **API Keys**: Store OpenAI keys securely
3. **Data deletion**: Cascade delete with documents

## Resources

- [pgvector documentation](https://github.com/pgvector/pgvector)
- [OpenAI embeddings](https://platform.openai.com/docs/guides/embeddings)
