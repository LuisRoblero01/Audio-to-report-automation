# System Architecture

## Overview

The system is built as a modular automation pipeline orchestrated through n8n workflows, connected to a PostgreSQL database for state management and user data.

## Architecture Diagram (Functional)

```
[Telegram User]
      |
[Telegram Bot API]
      |
[Message Router] -----> [Command Handler]
      |                     |--- /start -> Welcome menu
      |                     |--- /perfil -> Balance & stats
      |                     L--- /credits -> Purchase flow
      |
[Audio Processor]
      |--- Metadata extraction
      |--- Duration estimation
      |--- Credit validation
      L--- Cost calculation
            |
[Transcription Module] -----> AI Speech-to-Text API
            |
[Formatting Engine]
      |--- Quick summary
      |--- Detailed notes
      |--- Meeting minutes
      |--- Conference analysis
      |--- Study notes
      L--- Custom formats (user-defined templates)
            |
[Document Generator] -----> PDF / Word output
            |
[Delivery Module] -----> Telegram file upload
```

## Data Flow

1. **Ingestion Layer** — Receives and validates incoming messages via Telegram webhook
2. **User Context Layer** — Manages user profiles, credit balances, and preferences
3. **Processing Pipeline** — Orchestrates transcription, formatting, and document generation
4. **Payment Integration** — Handles credit purchases through external payment gateway
5. **Storage Layer** — PostgreSQL for users, transactions, audio metadata, and documents
6. **Observability** — Langfuse for tracing and performance monitoring

## Key Design Decisions

- **Event-driven:** Each processing stage triggers the next upon completion
- **Stateful:** User sessions and credit balances persist across interactions
- **Modular:** Each functional module is independent and replaceable
- **Template-based:** Document outputs use configurable templates to reduce API costs
- **Chunked processing:** Long audio files are split into manageable segments

## External Integrations

- **Telegram Bot API** — User interface and file transfer
- **AI APIs** — Speech-to-text and text structuring
- **Payment API** — Credit purchase processing
- **Langfuse** — Observability and tracing

## Database Schema (Simplified)

- **users** — Profiles, balances, preferences
- **audio_transactions** — Metadata for each processed audio
- **credit_movements** — Credit purchases, consumption, and adjustments
- **payments** — Payment records linked to credit purchases
- **format_templates** — Predefined and custom document templates
- **tokens** — API token usage tracking