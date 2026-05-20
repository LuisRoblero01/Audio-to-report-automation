# Audio Processing Strategy

This document describes how the system handles different types of audio content, from short voice notes to multi-hour recordings.

---

## Processing Routes

The system classifies audio into three categories based on duration:

### Route A: Direct Processing (< 20 minutes)

**Use case:** Voice notes, short class recordings, quick memos

**Process:**
1. Audio is sent directly to the Speech-to-Text API
2. Full transcript is sent to the AI model in a single request
3. AI generates the structured output according to the selected format
4. Document is generated and delivered

**Advantages:** Fastest route, lowest API cost, simplest pipeline

---

### Route B: Chunked Processing (20–90 minutes)

**Use case:** Lectures, meetings, interviews, podcasts

**Process:**
1. Audio is split into overlapping segments
2. Each segment is transcribed independently
3. Transcripts are merged with context preservation
4. The merged transcript is processed by the AI model
5. Document is generated with section markers corresponding to original audio segments

**Advantages:** Handles longer content without API token limits, preserves context across segments

**Technical considerations:**
- Segment overlap ensures no content is lost at boundaries
- Segment size is optimized for the specific AI model's context window
- Parallel processing where possible to reduce total time

---

### Route C: Specialized Processing (> 90 minutes)

**Use case:** Full conferences, workshops, all-day events

**Process:**
1. Audio is split into chapters or natural breakpoints
2. Each chapter is processed independently
3. Results are consolidated into a master document
4. An executive summary is generated separately
5. The final document includes:
   - Executive summary (overview)
   - Chapter-by-chapter detailed sections
   - Cross-references between related segments
   - Index of key topics discussed

**Advantages:** Handles very large content volumes, maintains readability

---

## Cost Estimation

Before processing begins, the system:
1. Measures the exact audio duration
2. Applies the credit calculation formula
3. Shows the user the estimated cost
4. Deducts credits only after successful document generation

If processing fails at any stage, credits are not consumed (the user can retry).

---

## Failure Handling

| Stage | Failure Response |
|---|---|
| Download | Re-attempts download; asks user to re-send if persistent |
| Transcription | Retries with alternative API if configured |
| AI Processing | Retries with reduced context window |
| Document Generation | Falls back to plain text output |
| Delivery | Retries with file compression |

Users are notified at each retry step via status messages.

---

## Quality Assurance

- Transcriptions are validated for minimum word count before formatting
- AI outputs are checked for structure compliance
- Generated documents are validated for file integrity
- Processing time is logged for performance monitoring
