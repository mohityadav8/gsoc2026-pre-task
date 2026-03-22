# Scalable GitHub Data Aggregation System — Architecture Design

**Pre-GSoC Task 2026 | Task 1 — Architecture Design**
**Organization:** Ceylon Computer Science Institute (C2SI)
**Project:** WebiU 2.0

---

## Table of Contents

1. [System Overview](#1-system-overview)
2. [Architecture Diagram](#2-architecture-diagram)
3. [Core Components](#3-core-components)
4. [Rate Limit Handling](#4-rate-limit-handling)
5. [Update Mechanism](#5-update-mechanism)
6. [Data Storage Strategy](#6-data-storage-strategy)
7. [Scalability Plan](#7-scalability-plan)
8. [Performance Optimization](#8-performance-optimization)
9. [Failure Handling](#9-failure-handling)
10. [API Flow](#10-api-flow)
11. [Technology Choices](#11-technology-choices)

---

## 1. System Overview

The organization maintains **300+ GitHub repositories** whose data must be displayed on a public website. The core challenge is that GitHub's REST API imposes strict rate limits (5,000 requests/hour for authenticated tokens), making it impossible to fetch all repository data on every frontend request.

The solution is a **pull-and-cache architecture** with four layers:

- **Ingestion Layer** — collects data from GitHub via webhooks and scheduled jobs
- **Processing Layer** — normalizes, aggregates, and enriches the raw data
- **Storage Layer** — persists data in MongoDB and caches hot data in Redis
- **API Layer** — serves the frontend with fast, pre-computed responses

The frontend **never calls GitHub directly**. It only talks to our own API, which serves data from cache or database — not from GitHub in real time.

---

## 2. Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                        GITHUB API                               │
│              api.github.com  (300+ repositories)                │
└───────────────────────────┬─────────────────────────────────────┘
                            │
              ┌─────────────▼──────────────┐
              │      DATA INGESTION        │
              │  ┌──────────┐ ┌─────────┐  │
              │  │ Webhooks │ │  Cron   │  │
              │  │(real-time│ │Scheduler│  │
              │  │ push)    │ │(15 min) │  │
              │  └──────────┘ └─────────┘  │
              └─────────────┬──────────────┘
                            │
              ┌─────────────▼──────────────┐
              │     PROCESSING LAYER       │
              │  Aggregation · Normalize   │
              │  Enrich · Rate-limit queue │
              └──────┬──────────────┬──────┘
                     │              │
          ┌──────────▼───┐   ┌──────▼──────────┐
          │   MONGODB    │   │   REDIS CACHE   │
          │  (Persistent │   │  (Hot data,     │
          │   storage)   │   │   TTL: 5 min)   │
          └──────────────┘   └─────────────────┘
                     │              │
              ┌──────▼──────────────▼──────┐
              │         API LAYER          │
              │  NestJS REST / GraphQL     │
              │  Cache-Control headers     │
              └─────────────┬──────────────┘
                            │
              ┌─────────────▼──────────────┐
              │     WEBSITE / FRONTEND     │
              │      Angular 17+           │
              └────────────────────────────┘
```

---

## 3. Core Components

### 3.1 Data Ingestion

The ingestion layer is responsible for pulling data from GitHub and feeding it into the rest of the system. It has two sub-components that work together:

**GitHub Webhooks (event-driven)**
Webhooks are registered on the GitHub organization. When a repository event occurs — such as a new push, a new pull request, a star, or a fork — GitHub immediately sends an HTTP POST to our ingestion endpoint. This ensures near-real-time updates for active repositories without us needing to poll.

Webhook events handled:
- `push` — updates commit count and last-activity timestamp
- `pull_request` — updates open/closed PR count
- `issues` — updates issue count
- `star` / `watch` — updates star and watcher count
- `fork` — updates fork count
- `repository` — handles repository creation, deletion, or rename

**Cron Scheduler (scheduled polling)**
Not all data changes trigger a webhook. Repository metadata (description, language, topics, size), contributor statistics, and full commit history require proactive polling. A scheduled job runs every 15 minutes using `@nestjs/schedule` (`@Cron()` decorator) to fetch incremental updates.

The cron job uses **conditional requests** (`If-Modified-Since` header) to skip repositories that have not changed, which reduces API usage significantly.

### 3.2 Processing Layer

The processing layer receives raw GitHub API responses and transforms them before storing:

- **Aggregation** — combines data from multiple API calls per repository (e.g. merging repo metadata with contributor stats and language breakdown)
- **Normalization** — converts GitHub timestamps to ISO format, standardizes field names, removes redundant fields
- **Contributor ranking** — pre-computes a sorted contributor leaderboard across all repositories so the frontend never has to compute it at query time
- **Rate-limit queue** — wraps all outgoing GitHub API calls in a queue (using `bull` or `p-limit`) to avoid bursting past the rate limit

### 3.3 Storage Layer

The storage layer has two tiers:

**MongoDB (Primary Store)**
All processed repository and contributor data is written to MongoDB. This is the source of truth. MongoDB is used because the data is document-shaped (JSON from GitHub maps naturally to BSON documents), relationships are minimal, and horizontal sharding is well-supported for future scaling.

**Redis (Cache Layer)**
Frequently requested data — such as the full project list or the contributor leaderboard — is stored in Redis with a 5-minute TTL. The API layer reads from Redis first and falls back to MongoDB on a cache miss. This keeps average response times under 20ms for popular endpoints.

### 3.4 API Layer

The API layer exposes the stored and cached data to the frontend. It is built with NestJS and supports both REST and GraphQL. It never calls the GitHub API directly — it only reads from Redis or MongoDB.

### 3.5 Caching Mechanism

A two-tier caching strategy is used:

| Tier | Technology | TTL | What is cached |
|------|-----------|-----|----------------|
| Application cache | Redis | 5 minutes | Project list, contributor leaderboard, per-repo stats |
| HTTP cache | Cache-Control headers | 5 minutes | All GET responses sent to the browser |
| Frontend cache | Angular `shareReplay(1)` | Page session | Avoids duplicate API calls within the same browser session |

---

## 4. Rate Limit Handling

GitHub's REST API allows **5,000 requests per hour** per authenticated token. With 300+ repositories, naively fetching all data on every request would exhaust the limit almost immediately.

The following strategies are used to stay well within limits:

### 4.1 Background Sync (primary strategy)
Data is never fetched from GitHub at request time. All GitHub API calls happen in the background (via cron jobs or webhooks) and results are stored in MongoDB. The frontend always reads from our own API, not from GitHub.

### 4.2 Conditional Requests
Every cron-based poll uses `If-Modified-Since` or `ETag` headers. If GitHub returns `304 Not Modified`, it does **not** count against the rate limit. This can reduce effective API usage by 60–80% for mostly-static repositories.

### 4.3 Request Queuing with Batching
All outgoing GitHub API calls are queued using `p-limit` with a concurrency of 10. This prevents burst spikes. Repositories are processed in batches of 10 using `Promise.all()` per batch.

### 4.4 Multiple Authentication Tokens (token rotation)
If scaling to 10,000 repositories requires more throughput, multiple GitHub Personal Access Tokens can be used in round-robin rotation. With 5 tokens, the effective rate limit becomes 25,000 requests/hour.

### 4.5 GraphQL API (fewer calls per query)
GitHub's GraphQL API allows fetching multiple fields for multiple repositories in a single request, compared to REST which requires one call per endpoint. For bulk data collection cycles, GraphQL is preferred over REST.

### 4.6 Rate Limit Monitoring
The system checks the `X-RateLimit-Remaining` header on every response. When the remaining count drops below 500, new requests are paused and queued until the reset time indicated by `X-RateLimit-Reset`.

---

## 5. Update Mechanism

The system uses a hybrid approach — webhooks for real-time changes, cron jobs for full refreshes.

### 5.1 Webhooks (real-time)
GitHub webhooks are registered at the organization level, covering all repositories. When a repository event fires, GitHub sends an HTTP POST to `/webhook/github` on our ingestion service. The payload is validated using GitHub's `X-Hub-Signature-256` HMAC signature, then the affected repository's record is updated in MongoDB and the Redis cache entry for that repository is invalidated.

**Latency:** Updates reflect on the website within seconds of a GitHub event.

### 5.2 Cron Scheduler (incremental sync)
A background job runs every 15 minutes to:
- Fetch newly created repositories not yet seen by webhooks
- Refresh contributor statistics (not available via webhooks)
- Refresh language breakdown and repository size
- Re-compute the contributor leaderboard

The cron job uses **incremental updates**: it only fetches repositories modified since the last sync timestamp, reducing API calls significantly.

### 5.3 Full Resync (daily)
Once per day at midnight, a full resync job re-fetches all repository metadata from scratch. This catches any data drift and ensures accuracy even if webhooks were missed during an outage.

---

## 6. Data Storage Strategy

### 6.1 What is stored persistently in MongoDB

The following data is expensive to fetch from GitHub (requires multiple API calls or pagination) and changes infrequently, so it is always stored persistently:

| Data | MongoDB Collection | Update Trigger |
|------|--------------------|----------------|
| Repository metadata (name, description, stars, forks, language, topics, size, license) | `repositories` | Webhook + 15-min cron |
| Contributor list per repository (username, commit count, additions, deletions) | `contributors` | 15-min cron |
| Aggregated contributor leaderboard (total contributions across all repos) | `leaderboard` | 15-min cron |
| Language distribution per repository | `repositories.languages` | 15-min cron |
| Open issue and PR counts | `repositories` | Webhook |
| Repository creation/update timestamps | `repositories` | Webhook |

### 6.2 What is fetched dynamically (not stored)

Some data is cheap to fetch, changes rarely, or is only requested occasionally. This is fetched live from GitHub only when the user explicitly requests it:

| Data | Reason not stored |
|------|------------------|
| Individual commit details (message, diff) | High volume, rarely requested |
| Full issue/PR thread content | Large payload, not displayed on homepage |
| User profile avatars and bios | Fetched directly by the browser from GitHub's CDN |
| Release notes and changelogs | Rarely requested, large text |

---

## 7. Scalability Plan

### Current state: 300 repositories
The architecture handles 300 repositories comfortably within a single NestJS instance with one MongoDB instance and one Redis instance. The 15-minute cron job needs approximately 300–600 API calls per cycle (well within the 5,000/hour limit).

### Scaling to 10,000 repositories

At 10,000 repositories, three changes are required:

**1. Token rotation pool**
10,000 repositories require roughly 10,000–20,000 API calls per full sync cycle. With a single token at 5,000/hour, a full sync would take 2–4 hours. A pool of 4–5 tokens (each registered under a separate GitHub account or GitHub App) brings the effective limit to 20,000–25,000 requests/hour, enabling a full sync in under 1 hour.

**2. Message queue for ingestion**
At scale, webhooks arrive in high volume. Instead of processing them synchronously, webhook events are pushed to a **Redis-backed Bull queue**. Worker processes consume the queue in parallel. This decouples ingestion speed from processing speed.

**3. MongoDB horizontal sharding**
At 10,000 repositories × contributor data, MongoDB is sharded by `organization_name`. Each shard handles a subset of repositories. A MongoDB Atlas cluster with auto-scaling handles this without code changes.

**4. Cron job partitioning**
Instead of one cron job iterating all 10,000 repositories sequentially, repositories are partitioned into buckets (e.g. 10 workers × 1,000 repos each). Each worker runs in a separate Node.js process or container, reducing sync time proportionally.

**Infrastructure comparison:**

| Scale | Tokens needed | MongoDB | Redis | Worker processes |
|-------|--------------|---------|-------|-----------------|
| 300 repos | 1 | Single instance | Single instance | 1 |
| 1,000 repos | 1–2 | Single instance | Single instance | 2–3 |
| 10,000 repos | 4–5 | Sharded cluster | Redis Cluster | 10+ |

---

## 8. Performance Optimization

### 8.1 Redis caching for hot endpoints
The most-requested endpoints — the project list and contributor leaderboard — are cached in Redis with a 5-minute TTL. Average response time for cached endpoints is under 20ms regardless of the number of repositories.

### 8.2 Pre-computed aggregations
The contributor leaderboard is not computed at query time. It is pre-computed by the processing layer every 15 minutes and stored as a ready-to-serve document in MongoDB and Redis. The API layer simply returns this pre-computed result.

### 8.3 HTTP cache headers
All GET responses from the API include `Cache-Control: public, max-age=300`. This allows browser caches and CDN edges to serve responses without hitting the API server at all.

### 8.4 Frontend in-memory cache
The Angular frontend uses RxJS `shareReplay(1)` in service observables. Once a project list is fetched within a browser session, subsequent navigation back to the same page does not trigger a new API call.

### 8.5 Database indexing
MongoDB collections are indexed on the fields most commonly used in queries:
- `repositories`: indexed on `full_name`, `stars` (desc), `updated_at` (desc)
- `contributors`: indexed on `username` and `repository`
- `leaderboard`: indexed on `total_contributions` (desc)

### 8.6 Pagination
All list endpoints return paginated responses (default: 30 items per page). This keeps response payloads small and frontend rendering fast regardless of repository count.

---

## 9. Failure Handling

### 9.1 GitHub API unavailability
If the GitHub API returns a 5xx error or times out, the ingestion service applies **exponential backoff with jitter**: it retries after 1s, 2s, 4s, 8s (maximum 4 retries). If all retries fail, the job is pushed back to the Bull queue with a 30-minute delay. The last successfully synced data remains in MongoDB and continues to be served to the frontend — users see slightly stale data rather than an error page.

### 9.2 Rate limit exhaustion
If the `X-RateLimit-Remaining` header drops to zero, all outgoing GitHub API calls are immediately suspended. The system reads the `X-RateLimit-Reset` Unix timestamp from the header and schedules a resume exactly at that time. The frontend is unaffected because it reads from MongoDB/Redis, not from GitHub directly.

### 9.3 Redis failure
If Redis becomes unavailable, the API layer falls back to reading directly from MongoDB. Response times increase (typically 50–200ms instead of <20ms) but the service continues to function correctly. A health-check endpoint (`GET /health`) reports the degraded state.

### 9.4 MongoDB failure
If MongoDB becomes unavailable, the API layer serves data from Redis for as long as the TTL allows (up to 5 minutes). After TTL expiry, the API returns a 503 response with a `Retry-After` header. A PagerDuty/Slack alert is triggered immediately on MongoDB connection failure.

### 9.5 Webhook delivery failure
GitHub retries failed webhook deliveries up to 3 times with increasing delays. In addition, the 15-minute cron job acts as a safety net: any update missed by a failed webhook will be picked up by the next scheduled sync.

### 9.6 Missed webhook events during downtime
If the ingestion service is down during a period of repository activity, the daily full resync job (midnight) will re-fetch all data from scratch and restore consistency.

---

## 10. API Flow

### 10.1 Normal request flow (cache hit)

```
Frontend                 API Layer               Redis           MongoDB
   │                         │                     │                │
   │── GET /api/projects ───►│                     │                │
   │                         │── GET projects ────►│                │
   │                         │◄── cached data ─────│                │
   │◄── 200 JSON (< 20ms) ───│                     │                │
```

### 10.2 Cache miss flow

```
Frontend                 API Layer               Redis           MongoDB
   │                         │                     │                │
   │── GET /api/projects ───►│                     │                │
   │                         │── GET projects ────►│                │
   │                         │◄── cache miss ───────│                │
   │                         │────────────── GET projects ─────────►│
   │                         │◄───────────── full data ─────────────│
   │                         │── SET projects ────►│                │
   │◄── 200 JSON (< 100ms) ──│                     │                │
```

### 10.3 Webhook update flow

```
GitHub                Ingestion Service       Processing Layer   MongoDB  Redis
   │                         │                     │               │        │
   │── POST /webhook ────────►│                     │               │        │
   │                         │── validate HMAC     │               │        │
   │                         │── push to queue ───►│               │        │
   │                         │                     │── fetch delta─►│        │
   │                         │                     │── write ──────►│        │
   │                         │                     │── invalidate ──────────►│
```

### 10.4 API Endpoints

| Method | Endpoint | Description | Data source |
|--------|----------|-------------|-------------|
| `GET` | `/api/projects` | All repositories with stats | Redis → MongoDB |
| `GET` | `/api/projects/:repo` | Single repository detail | Redis → MongoDB |
| `GET` | `/api/contributors` | Aggregated leaderboard | Redis → MongoDB |
| `GET` | `/api/contributors/:username` | Per-user contribution stats | MongoDB |
| `GET` | `/api/analytics/languages` | Language distribution across all repos | Redis → MongoDB |
| `POST` | `/webhook/github` | Receive GitHub webhook events | — (write path) |
| `GET` | `/health` | System health check | Redis + MongoDB ping |

---

## 11. Technology Choices

### 11.1 Backend — NestJS (Node.js + TypeScript)

**Why:** NestJS provides a modular, dependency-injection architecture that maps directly to this system's layered design (ingestion module, processing module, storage module, API module). Its `@nestjs/schedule` package handles cron jobs natively, `@nestjs/bull` integrates with Redis queues, and `@nestjs/mongoose` integrates with MongoDB — all first-party packages with strong TypeScript support. The existing WebiU 2.0 codebase is already built on NestJS, so this maintains consistency.

**Alternative considered:** Express.js — rejected because it lacks built-in dependency injection and module organization, which would make this multi-layer system harder to maintain.

### 11.2 Database — MongoDB

**Why:** GitHub API responses are JSON documents. Repository data has a variable schema (not all repos have the same fields), and MongoDB's flexible document model handles this naturally without schema migrations. MongoDB also supports horizontal sharding natively for the 10,000-repo scale scenario.

**Alternative considered:** PostgreSQL — rejected because the tabular schema would require many joins for contributor-per-repository queries, and schema migrations would be needed whenever GitHub adds new fields to its API response.

### 11.3 Cache — Redis

**Why:** Redis is the industry standard for low-latency key-value caching. It supports TTL natively, runs in memory (sub-millisecond reads), and doubles as a message broker for the Bull job queue. Using Redis for both caching and queuing reduces infrastructure components.

**Alternative considered:** In-memory Node.js Map — rejected because it does not persist across process restarts and cannot be shared across multiple worker processes or containers.

### 11.4 Job Queue — Bull (Redis-backed)

**Why:** Bull is the most widely used NestJS-compatible job queue library. It supports delayed jobs, retries with exponential backoff, job prioritization, and a visual dashboard (Bull Board). It uses Redis as its backend, so no additional infrastructure is needed.

### 11.5 Update Mechanism — GitHub Webhooks + Cron (hybrid)

**Why:** Webhooks alone are unreliable (delivery can fail, and GitHub does not guarantee exactly-once delivery). Cron jobs alone are too slow for real-time updates and waste API calls on unchanged repositories. The hybrid approach gives the best of both: near-real-time updates via webhooks, and guaranteed eventual consistency via scheduled syncs.

### 11.6 Frontend — Angular 17+ (existing)

The frontend is already built with Angular 17+ in the WebiU 2.0 codebase. No change is proposed to the frontend technology stack.

### 11.7 Infrastructure Summary

| Component | Technology | Justification |
|-----------|-----------|---------------|
| Backend API | NestJS (Node.js + TypeScript) | Modular, existing codebase |
| Primary database | MongoDB | Flexible JSON schema, horizontal sharding |
| Cache + queue | Redis | Sub-ms reads, TTL, Bull backend |
| Job queue | Bull (@nestjs/bull) | Retries, delays, NestJS-native |
| Containerization | Docker + Docker Compose | Matches existing WebiU 2.0 setup |
| Webhook security | HMAC-SHA256 signature validation | GitHub-native security |
| API style | REST + GraphQL | REST for simplicity, GraphQL for bulk sync |

---

*Design document prepared for C2SI Pre-GSoC Task 2026 — Task 1.*
