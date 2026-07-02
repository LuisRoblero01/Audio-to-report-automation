# Brudi Notas bot: how it works inside

## Quick intro

This document explains, in simple words, how the **Brudi Notas bot** flow works inside the **Audio to Report Automation** project.

The idea is simple: the user talks to the bot on Telegram, sends an audio or a voice note, the bot checks if the user has credits, turns that audio into text, shapes it with AI, and sends back a ready-to-read document. 🙂

The flow has two entry doors:

- **Telegram mensajes**: receives text, menu commands, and audios or voice notes.
- **Confirmación pago**: receives the Mercado Pago notice when a payment is completed, through a **webhook** (an automatic door where another system tells us that something happened).

## In a nutshell

Think of the bot as a small automatic office.

First it receives the audio. Then it checks if that audio was already processed before, confirms that the user has enough credits, and, if everything is fine, it sends it to be transcribed. After that, an AI arranges the content based on the chosen format, a document is created, and finally the bot sends it back through Telegram.

If the user has no credits, the bot takes them to the **Comprar paquetes de créditos** sub-flow so they can buy minutes with Mercado Pago.

## The journey of an audio, step by step

1. The bot receives something from Telegram: it can be text, a menu command, an audio, or a voice note.

2. The **Tipo entrada** node decides what kind of message arrived.

3. If what arrived is audio, the flow extracts the needed data, downloads the file from Telegram, and saves it temporarily on disk.

4. Then it calculates a **hash** of the audio (a unique fingerprint that helps avoid processing the same file twice).

5. After that, it saves the audio record in **PostgreSQL** (the database where the bot's information is stored) and looks up the user's credit balance.

6. The bot checks if the user has enough credits.

7. If the user does not have enough credits, it sends them to the **Comprar paquetes de créditos** sub-flow.

8. If the user does have credits, the flow picks the AI model and the matching format.

9. While it works, the bot keeps the user posted on Telegram with progress messages:

   - **Audio en proceso** (audio in progress)
   - **Transcribiendo audio** (transcribing audio)
   - **Analizando transcript** (analyzing transcript)
   - **Dando formato** (formatting)
   - **Convirtiendo PDF** (converting to PDF)
   - **Flujo realizado** (flow done)

10. To turn the audio into text, it uses **Deepgram** with the **nova-3** model in Spanish. It also uses diarization, which means separating who is speaking, and it adds punctuation and smart formatting.

11. The transcript is saved in the database.

12. Then the transcript goes to an AI model, which structures it based on the format chosen by the user.

13. The result is turned into a document, either from **HTML to PDF** or from **DOCX to PDF**.

14. In the end, the bot sends it through Telegram with the **Enviar documento** step.

## When the user pays for credits

Payment also has its own path.

1. Mercado Pago notifies the flow through the **Confirmación pago** webhook.

2. A node reads the payment data. There it identifies the user by their **chat_id** (the identifier of their Telegram conversation) and detects which package they bought using a reference like **chatid_paquete_1**.

3. The flow inserts the payment into the database.

4. Then it checks whether the payment was approved or declined.

5. If the payment was approved, it records the purchase movement, adds the credits or minutes, updates the balance, and tells the user on Telegram their new balance.

## The formats the user can choose

The bot can deliver the result in different formats. Some belong to the basic plan and others to the premium plan:

| Format | Plan |
| --- | --- |
| Raw transcript | basic |
| Clean transcript | basic |
| Quick summary | basic |
| Detailed summary | premium |
| Conference notes | basic |
| Conference analysis | premium |
| Essential notes | basic |
| Complete notes | premium |
| Standard minutes | basic |
| Complete minutes | premium |
| Interview / podcast premium | premium |

There are also **custom** formats. In that case, the user uploads a template or reference document, the bot analyzes it, and saves it to reuse later. From the menus, the user can add and remove their own formats.

## The credit packages

Credits are bought as processing minutes, paid through **Mercado Pago**:

| Package | Minutes | Price |
| --- | ---: | ---: |
| Package 1 | 60 min | 29 MXN |
| Package 2 | 120 min | 59 MXN |
| Package 3 | 180 min | 79 MXN |
| Package 4 | 300 min | 119 MXN |

## What technology it uses under the hood (and why)

- **n8n**: the automation that orchestrates everything, like the conductor of the flow.
- **Telegram Bot API**: the channel where the user talks to the bot.
- **Deepgram**: turns the audio into text.
- **AI models**: arrange and structure the text based on the chosen format.
- **PostgreSQL**: the database where everything is stored.
- **Mercado Pago**: processes the payments.
- **Langfuse**: used for observability, that is, to see and monitor how the AI behaves, its times, and its tokens.
- **Dynamic menus**: buttons inside Telegram to move around the main menu, profile, credits, formats, and support.

Depending on the format, the flow can use these AI models:

- **GPT-5.4-nano** for simple formats like the raw transcript.
- **GPT-5.4** and **GPT-4** for basic and premium OpenAI formats.
- **DeepSeek v4**, in flash and pro versions, for several formats.
- **Google Gemini 3.1 Pro** to analyze reference documents for custom formats.

## What data the bot stores

The bot stores information by purpose, not as a loose notebook, but in tables inside the database:

- Users and their credit balance.
- Received audios, including their hash to avoid duplicates.
- Transcripts.
- Payments and purchase movements.
- Credit usage per processing.
- Each user's custom formats.
- Used tokens and execution times for monitoring.

---

This is conceptual documentation to understand the flow in a clear and general way. Credentials, tokens, and sensitive internal details are handled separately.
