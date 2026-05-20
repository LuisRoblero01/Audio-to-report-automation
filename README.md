# Audio to Report Automation

AI-powered Telegram bot that transcribes voice messages and audio files into structured documents (PDF/Word).

## Purpose

Converts unstructured audio content — classes, meetings, conferences, voice notes — into professional, organized documents. The user sends an audio file via Telegram and receives a processed document in minutes.

## Core Modules

- **Audio Ingestion** — Accepts audio via Telegram, extracts metadata (duration, format, file size)
- **Pricing Engine** — Estimates processing cost based on audio duration
- **Payment Gateway** — Handles credit purchases through a payment provider
- **Transcription** — Converts speech to text using AI APIs
- **Formatting Engine** — Structures the transcript into user-selected output formats
- **Document Generation** — Produces PDF or Word files from templates
- **Delivery** — Returns the final document through Telegram

## Built With

- **Orchestration:** n8n workflow automation
- **AI/ML:** Large Language Models (text processing & structuring)
- **Storage:** PostgreSQL
- **Payments:** Third-party payment processor integration
- **Messaging:** Telegram Bot API
- **Observability:** Langfuse

## Project Structure

```
├── docs/
│   ├── vision_producto.md    # Product vision & business model
│   └── arquitectura.md       # System architecture
└── structure_flow           # High-level flow description
```

## Note

This repository contains the conceptual design and architecture of the system. Implementation details, credentials, and internal logic are maintained separately.