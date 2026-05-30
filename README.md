# Voice Agent SDK

A real-time AI voice agent built using Twilio, OpenAI Realtime API, FastAPI, and Redis.

This project demonstrates how modern AI and telephony technologies can be integrated to create a voice assistant capable of handling phone conversations through real-time audio streaming, voice activity detection, AI-powered responses, and session management.

---

## Architecture

```text
Caller
   │
   ▼
Twilio (Telephony Layer)
   │
   ▼
FastAPI Backend
   ├── Semantic VAD
   ├── Audio Processing (G.711 µ-law)
   └── Session Management
   │
   ▼
OpenAI Realtime API (GPT-4o)
   │
   ▼
FastAPI Backend
   │
   ▼
Twilio
   │
   ▼
Caller Receives Response

           Redis
     (Transcripts & Sessions)
```

---

## Overview

This project demonstrates a real-time voice AI architecture where callers can interact with an AI assistant through a phone call.

The system uses WebSocket-based audio streaming to transfer audio between Twilio, the FastAPI backend, and the OpenAI Realtime API. Conversation transcripts and session data are stored in Redis to support logging and session continuity.

---

## Call Flow

### 1. Incoming Call

A user places a phone call through Twilio.

### 2. Audio Streaming

Twilio streams audio to the FastAPI backend using WebSockets.

### 3. Voice Activity Detection

Semantic Voice Activity Detection (VAD) identifies when the caller has finished speaking.

### 4. Audio Processing

Audio is processed using the G.711 µ-law format commonly used in telephony systems.

### 5. AI Response Generation

The audio stream is sent to OpenAI Realtime API (GPT-4o), which generates a response.

### 6. Response Streaming

The generated audio response is streamed back through FastAPI and forwarded to Twilio.

### 7. Session Storage

Conversation transcripts and metadata are stored in Redis for tracking and future analysis.

---

## Key Concepts

### Semantic Voice Activity Detection (VAD)

Semantic VAD helps determine natural speech boundaries and conversational pauses, allowing the assistant to respond at appropriate moments.

### G.711 µ-law Encoding

Telephone systems commonly use G.711 µ-law encoding at 8kHz. This project processes audio in a format compatible with telephony infrastructure.

### OpenAI Realtime API

The OpenAI Realtime API enables real-time speech-to-speech interactions, reducing the need for separate Speech-to-Text (STT) and Text-to-Speech (TTS) pipelines.

### Redis Session Storage

Redis stores transcripts, conversation history, and session-related information for logging and analytics.

---

## Tech Stack

| Layer            | Technology                       |
| ---------------- | -------------------------------- |
| Telephony        | Twilio                           |
| AI Model         | OpenAI Realtime API (GPT-4o)     |
| Backend          | FastAPI, Python                  |
| Audio Processing | G.711 µ-law, WebSocket Streaming |
| Voice Detection  | Semantic VAD                     |
| Storage          | Redis                            |
| Communication    | WebSockets                       |

---

## Features

* Real-time voice interaction
* Twilio phone call integration
* OpenAI Realtime API integration
* Semantic Voice Activity Detection
* G.711 µ-law audio processing
* Redis-based transcript storage
* Session management
* Modular architecture for extensibility

---

## Use Cases

* AI-powered customer support
* Insurance assistance bots
* Appointment scheduling systems
* Voice-based information retrieval
* Outbound and inbound call automation

---

## Future Enhancements

* Multi-language support
* Call analytics dashboard
* Sentiment analysis
* CRM integration
* Call recording and playback
* Advanced conversation memory

---

## Learning Outcomes

This project demonstrates practical experience with:

* Voice AI systems
* Real-time audio streaming
* WebSocket communication
* Telephony integration
* OpenAI Realtime API
* FastAPI backend development
* Redis session management
* Event-driven architectures

---

## Author

**Lokesh Chandra Roy**

AI/ML Engineer | Voice AI | Generative AI | RAG Systems | LLM Applications

LinkedIn: https://www.linkedin.com/in/lokesh-roy02

GitHub: https://github.com/Lokeshroy2
