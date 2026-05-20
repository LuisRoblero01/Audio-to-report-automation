# Data Model — High-Level Overview

This document describes the conceptual data model used by the system. It does not include exact table schemas, column names, or implementation details.

---

## Core Entities

### User
- **Purpose:** Stores user profiles and preferences
- **Key attributes:** Identifier, name, registration date, tier/level
- **Relationships:** Links to credit balances, processing history, custom formats

### Credit Balance
- **Purpose:** Tracks available credits for each user
- **Sources of credits:** Purchased packages, promotional credits, adjustments
- **Consumption:** Credits deducted per audio processing session (proportional to duration)
- **Tiers:** Multiple balance tiers affect processing priority and available features

### Payment Transactions
- **Purpose:** Records every credit purchase
- **Key data:** Amount, package type, payment status, timestamp, payment method
- **States:** Pending → Confirmed → Credited

### Credit Movements
- **Purpose:** Full audit trail of all credit changes
- **Movement types:**
  - Purchase (credit addition)
  - Consumption (processing cost)
  - Adjustment (manual corrections)
  - Promotional (bonus credits)
- **Key data:** Type, amount, timestamp, reference to related entity

### Audio Transactions
- **Purpose:** Records every audio processing session
- **Key data:** File metadata, duration, processing cost, format used, status
- **States:** Received → Transcribing → Formatting → Generating → Complete / Error
- **Relationships:** Links to user, credit movement, and output document

### Document Outputs
- **Purpose:** Stores generated documents
- **Key data:** File reference, format type, template used, generation timestamp
- **Formats:** PDF, DOCX

### Format Templates
- **Purpose:** Defines output formats for document structuring
- **Types:**
  - System formats (predefined)
  - Custom formats (user-created via HTML/CSS upload)
- **Key data:** Name, structure definition, preview reference, owner

### API Token Tracking
- **Purpose:** Monitors and logs API usage for cost optimization
- **Key data:** API provider, tokens consumed, operation type, timestamp

---

## Entity Relationships (Conceptual)

```
User
 ├── Credit Balance (1:1)
 ├── Credit Movements (1:N)
 ├── Audio Transactions (1:N)
 │    └── Document Outputs (1:1)
 ├── Payment Transactions (1:N)
 ├── Format Templates (1:N)
 └── Custom Format Submissions (1:N)
```

---

## Design Principles

- **Eventual consistency:** Processing updates flow asynchronously
- **Idempotency:** Payment and processing operations handle duplicate requests safely
- **Auditability:** Every credit change has a traceable movement record
- **Separation of concerns:** Payment data is isolated from core processing data
