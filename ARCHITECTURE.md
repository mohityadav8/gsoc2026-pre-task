

Webiu-master.zip
zip

Ceylon Computer Science Institute (C2SI) Project Proposal Template for GSoC 2026         

`# Tips to make good GSoC proposal

Start as early as possible
Get involved in C2SI projects
Discuss your project idea with your mentor within C2SI Community
Prepare your proposal early, leaving time to discuss your proposal within C2SI Community
C2SI GSoC Proposal Template

Project Introduction

In this section,
* The student should first define the problem.
* Current state of the problem.
* What issue does the student wish to solve?

Project Goals

In this section,

The student should propose a clear list of deliverables, explaining exactly what the student promises to do and what he/she does not plan to do.
Implementation

In this section,

The student should describe what he/she plans to do as a solution for the problem he/she defined earlier. Timeline
In this section,

The student should define the project milestones in accordance with the GSoC timeline.
C2SI Contributions

In this section,

The student should include his/her past contributions to the C2SI Community.
Contribution Examples: Bug Reporting, Patchers (Github Pull Requests), Mailing List Activities
Personal Information

Contact information (E-mail, IM, IRC)
Few sentences about the student and why he/she thinks as the best person to do this project.

Reference

Project reference

Questions

Are you a C2SI contributor/ Have you contributed to C2SI before?

How can we reach you (eg: email) if we have questions about your application?

What is your github username(s):

Project Specific Questions

Which C2SI GSoC project are you applying for (please submit separate applications for each project):
What do you plan to accomplish over this summer for this project? (Please tell us       a. What project you want to work on,       b. How you will approach that project portion (with your milestones))
You may want to ask for help from our mentors through __c2si.slack.com__ if you have not created project milestones before, or if you are unsure what can be accomplished.

If your project proposal is accepted, you will have a chance to work with your mentors to revise these milestones.

If you have your own project to propose, please describe it here:
Projects related details. (Have you tried that project you selected from the C2SI project list? What problems, if any, were presented? What prevented you from getting the entire system up and running?)

List down any plans you have during this summer( over the time period of GSoC, such as classes, job, vacation plans, thesis, etc.)

Education:

What year are you in school?
What programming courses have you taken?
What is your major?
Have you done group projects (programming or otherwise)?
What was your primary contribution to/role in the group?
Do you have work experience in programming? Tell us about it.

Do you have previous open source experience? Briefly describe what you have done.

Tell one interesting fact about yourself.

 according to data create a project proposal strictly follow the steps they have provided

Show more
Mar 21

Examined project files to construct GSoC proposal framework
Examined project files to construct GSoC proposal framework
Now I have a thorough understanding of the Webiu project. Let me read the DOCX skill and create a polished proposal.


Ran 4 commands, edited 2 files, viewed a file
Ran 4 commands, edited 2 files, viewed a file
Here's your complete GSoC 2026 proposal for the WebiU 2.0 project, strictly following every section of the C2SI template. Here's what's inside:

Structure (exactly per the template):

Project Introduction — Defines the problem (MongoDB commented out, GraphQL incomplete, no E2E tests, no PWA, no real-time updates, no analytics), documents the current state, and explains what will be solved.
Project Goals — Seven clearly scoped deliverables: MongoDB persistence, full GraphQL API, Playwright E2E testing, Angular PWA, WebSocket real-time updates, Chart.js analytics, and CI/CD maturity — plus explicit "Out of Scope" items.
Implementation — Technical deep-dive for each goal, rooted in the actual codebase (references app.module.ts, src/graphql/, CacheService, GithubService, etc.).
Timeline — 10-row milestone table mapped to the GSoC 2026 calendar (Community Bonding → Final Eval), with two-week sprints and a midterm deliverable.
C2SI Contributions — Three PRs (#47, #51, #55), two bug reports (#38, #43), and Slack/mailing list activity.
Personal Information — Contact details and a compelling candidate statement.
Reference — All relevant project and tool links.
All 12 Questions — Every general and project-specific question answered in full, including the education table (Q9).
Note: Personalise the name, email, GitHub username, and contribution PR/issue numbers with your real details before submitting.

Gsoc 2026 webiu proposal
Document · DOCX 

Pre-GSoC Task 2026 #541 Open Task Open Pre-GSoC Task 2026 #541 Task @Maahi10001 Description Maahi10001 opened 2 weeks ago · edited by Maahi10001 Collaborator Discussed in #540 Originally posted by Maahi10001 March 10, 2026 NOTE Strictly adhere to the requirement. A few addition to t

pasted

how to do these task like i have to pre gsoc task

1:38 AM

Deconstructed pre-GSoC tasks and sought clarification on assistance needed
Deconstructed pre-GSoC tasks and sought clarification on assistance needed
Great that you're working on these! Let me break down exactly what each task requires, then I can help you build them.

While you answer those, here's a quick overview of exactly what the evaluators expect from each task:

Task 1 — Architecture Design requires 4 deliverables:

An architecture diagram (can be a clean image or diagram tool export)
A 1–2 page written explanation covering all 10 points (rate limiting, webhooks, caching, scalability to 10k repos, failure handling, etc.)
An API flow description
Technology justification
Task 2 — Development requires you to actually build and deploy a working tool that:

Takes GitHub repo URLs as input
Calls the GitHub API to collect data (stars, forks, commits, languages, contributors)
Runs your custom scoring formulas for Activity Score, Complexity Score, and Learning Difficulty (Beginner/Intermediate/Advanced)
Shows a structured report
Is deployed with a live URL
Has clear documentation of your formulas
The key note from the task: strictly adhere to requirements — don't add random extra features, focus on doing the required things well. Looking at other submissions, the ones that stand out have clearly documented formulas and clean UIs.

Share your answers above and I'll help you build both from scratch!

tel me what to do next

1:51 AM

Synthesized context and devised actionable next steps
Synthesized context and devised actionable next steps
Let me ask a couple of quick things so I can give you the right steps:

Once you answer these two questions I can give you the exact next steps — whether that's writing your architecture document, building the tool code, or walking you through deployment. Just tap your answers above!

Q: Which task do you want to tackle first?
A: Task 1 — Architecture Design (diagram + document)

Q: What's your coding comfort level?
A: Beginner — need step-by-step help

1:51 AM

Architected layered system diagram with detailed SVG positioning
Architected layered system diagram with detailed SVG positioning
Perfect! Let me break this down into simple steps. Task 1 has 4 deliverables — I'll help you create all of them one by one.

Here's the plan:

First — understand the architecture (I'll draw it for you)
Second — write the design document (I'll generate it)
Third — put it all in a GitHub repo
Let's start with the diagram so you understand the system visually:

Here's the architecture you need to understand and submit. Let me show you the full system diagram first, then I'll tell you exactly what to write for each section.



You can click any layer in the diagram above to learn more about it. Now here's exactly what to do next, step by step:

Step 1 — Create a GitHub repo

Go to github.com → New repository → Name it gsoc2026-pre-task → Make it Public → Add a README.

Step 2 — Create ARCHITECTURE.md

This is your Task 1 submission file. It needs to cover all 10 requirements. I'll generate the full content for you — just say "write my ARCHITECTURE.md" and I'll produce the entire file ready to copy-paste.

It will cover all 10 points the task asks for: rate limit handling, webhooks vs cron, MongoDB vs dynamic data, scaling from 300 to 10,000 repos, failure handling, caching strategy, API flow, and technology justification.

Step 3 — Add the architecture diagram

You can either embed the diagram as an image or describe it in the markdown. I can help you create a clean image version too.

Step 4 — Start Task 2 (the Python tool)

Once Task 1 is done, we move to building the actual GitHub analyzer tool in Python with Streamlit (easy to deploy for free).

Want me to write your full ARCHITECTURE.md file now? Just say yes and I'll generate everything.

write my ARCHITECTURE.md

1:58 AM

Orchestrated comprehensive architecture documentation framework
Orchestrated comprehensive architecture documentation framework
Your ARCHITECTURE.md is ready. It covers all 10 required points from the task. Here's what's inside:

Section	What it covers
System Overview	The problem and the 4-layer solution
Architecture Diagram	ASCII diagram showing all layers
Core Components	All 5 required components explained
Rate Limit Handling	6 strategies (conditional requests, queuing, token rotation, GraphQL)
Update Mechanism	Webhooks + cron + daily full resync
Data Storage Strategy	What goes in MongoDB vs fetched live, with a table
Scalability Plan	300 → 10,000 repos with concrete numbers
Performance Optimization	Redis, pre-computed aggregations, indexes, pagination
Failure Handling	GitHub down, rate limit exhaustion, Redis failure, MongoDB failure
API Flow	3 sequence diagrams + endpoint table
Technology Choices	NestJS, MongoDB, Redis, Bull — all justified
Now do this to submit it:

Go to your GitHub repo (gsoc2026-pre-task)
Click Add file → Create new file
Name it ARCHITECTURE.md
Paste the entire content from this file
Click Commit changes
Once that's done, say "now help me build Task 2" and I'll walk you through building the GitHub analyzer tool in Python step by step.

Architecture
Document · MD 





Claude is AI and can make mistakes. Please double-check responses.
Architecture · MD
Copy

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
 

