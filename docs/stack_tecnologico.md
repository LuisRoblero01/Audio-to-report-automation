# Technology Stack

## Core Platform

| Layer | Technology | Role |
|---|---|---|
| **Orchestration** | n8n | Workflow automation engine — all business logic flows |
| **Database** | PostgreSQL | User data, transactions, processing history, templates |
| **Cache/Queue** | Redis | Job queuing and temporary state management |

## AI & Processing

| Service | Purpose |
|---|---|
| **Large Language Models** | Text structuring, summarization, formatting |
| **Speech-to-Text API** | Audio transcription |
| **Document Analysis** | Format reference extraction from uploaded templates |

Multiple AI providers are integrated to allow routing based on cost, speed, or capability requirements.

## Messaging & Delivery

| Service | Purpose |
|---|---|
| **Telegram Bot API** | User interface — message handling, file transfer, inline menus |

## Payments

| Service | Purpose |
|---|---|
| **Third-party Payment Gateway** | Credit package purchases, payment webhooks |

## Observability

| Service | Purpose |
|---|---|
| **Langfuse** | Tracing, cost monitoring, latency tracking, debugging |

## Infrastructure

| Component | Technology |
|---|---|
| **Containerization** | Docker / Docker Compose |
| **Reverse Proxy** | Nginx Proxy Manager |
| **SSL/TLS** | Let's Encrypt (automated renewal) |
| **Hosting** | Cloud VPS |

---

## Design Decisions & Rationale

### Why n8n?
- Visual workflow builder speeds up iteration
- Native integrations for Telegram, databases, and APIs
- Self-hosted — full control over data and execution
- Webhook-based triggers for real-time processing

### Why PostgreSQL?
- ACID compliance for financial transactions
- JSONB support for flexible document templates
- Triggers and functions for automated balance calculations
- Reliable for multi-step payment flows

### Why credit-based pricing?
- Aligns costs with actual API usage
- No recurring commitments — lower barrier to entry
- Transparent: users see exactly what they spend
- Scalable: costs scale with usage

### Why Telegram?
- No additional app installation required
- Rich inline keyboard support for complex menus
- File upload/download API for documents
- Large user base in target markets
