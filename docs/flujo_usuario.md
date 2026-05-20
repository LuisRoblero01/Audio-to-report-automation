# User Journey — Complete Flow

This document describes every step a user goes through when interacting with the bot, from first contact to receiving a processed document.

---

## Phase 1 — First Contact

### Step 1: /start
The user sends `/start` to the Telegram bot.

**Bot response:**
- Welcome message with bot description
- Main menu with inline buttons:
  - Send Audio (main action)
  - My Profile (balance, history)
  - Buy Credits (purchase packages)
  - Custom Formats (manage templates)
  - Support / Help

---

## Phase 2 — Sending Audio

### Step 2: Upload audio
The user sends a voice message or audio file (.ogg, .mp3, .wav, etc.)

**Bot response:**
- Confirms receipt of the audio
- Shows metadata:
  - File name
  - Duration (in minutes)
  - Estimated credit cost
- Displays available credit balance
- Presents format selection menu

### Step 3: Select output format
The user chooses a format from inline buttons:

| Format | Best for |
|---|---|
| Quick Summary | Short voice notes, quick captures |
| Detailed Summary | Lengthy discussions, structured output |
| Meeting Minutes | Business meetings, agreements |
| Conference Analysis | Talks, presentations, panels |
| Study Notes | Lectures, classes, educational content |

If the user has custom formats defined, those also appear as options.

### Step 4: Credit validation
- If user has enough credits → processing begins
- If user doesn't have enough → bot suggests purchasing a package
  - Clicking "Buy Credits" opens the purchase flow
  - After payment confirmation, processing begins automatically

---

## Phase 3 — Processing

### Step 5: Processing status
The bot sends status updates:
1. "Downloading audio..."
2. "Transcribing audio..." (Speech-to-Text)
3. "Analyzing transcript..." (AI structuring)
4. "Formatting document..." (Applying template)
5. "Generating PDF..." (Document creation)

Each status is sent as an edited message to avoid notification spam.

### Step 6: Document delivery
Once complete, the bot sends:
- The final document (PDF or Word file)
- Processing summary (duration, credits consumed)
- Updated balance
- Option to process another audio

---

## Phase 4 — Additional Actions

### Buying Credits
Users can purchase credit packages at any time:
1. Select "Buy Credits" from the main menu
2. Choose a package (60 / 120 / 180 / 300 minutes)
3. Receive a payment link
4. Complete payment through the payment gateway
5. Credits are credited automatically

### Creating Custom Formats
Users can define their own output templates:
1. Select "Custom Formats" from the main menu
2. Upload an HTML/CSS template
3. Preview the template with sample content
4. Name and save the format
5. It appears in the format selection menu

### Profile & History
Users can check:
- Current credit balance
- Processing history (date, audio duration, format used, credits consumed)
- Active custom formats

---

## Error Handling

| Scenario | Bot Response |
|---|---|
| Invalid audio file | Informs user of supported formats |
| File too large | Prompts user to send smaller file |
| API timeout | Retries automatically, notifies user if retry fails |
| Payment timeout | Cancels and prompts user to try again |
| Insufficient credits | Leads to purchase flow |
