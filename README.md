# voice-agent-sdk


# Personal Voice Agent SDK

A production-grade real-time AI voice agent that handles phone calls end-to-end — 
from telephony to AI response — using Twilio, OpenAI Realtime API, FastAPI, and Redis.

---

## Architecture

┌─────────────────────────────────────────────────────────┐
│                      CALLER                             │
└─────────────────────┬───────────────────────────────────┘
                      │ Phone Call
                      ▼
┌─────────────────────────────────────────────────────────┐
│                     TWILIO                              │
│              (Telephony Layer)                          │
└─────────────────────┬───────────────────────────────────┘
                      │ WebSocket Audio Stream
                      ▼
┌─────────────────────────────────────────────────────────┐
│                 FASTAPI BACKEND                         │
│             (Event-Driven Core)                         │
└──────┬───────────────┬──────────────────────────────────┘
       │               │
       ▼               ▼
┌────────────┐  ┌──────────────────┐
│ Semantic   │  │   µ-law Encoder  │
│    VAD     │  │   (G.711 8kHz)   │
└────────────┘  └────────┬─────────┘
                         │
                         ▼
            ┌────────────────────────┐
            │  OpenAI Realtime API   │
            │       (GPT-4o)         │
            └────────────┬───────────┘
                         │ AI Audio Response
                         ▼
            ┌────────────────────────┐
            │    FASTAPI BACKEND     │
            └──────┬─────────────────┘
                   │              │
                   ▼              ▼
            ┌──────────┐    ┌──────────┐
            │  TWILIO  │    │  REDIS   │
            └──────┬───┘    └──────────┘
                   │      (Transcript + Session)
                   ▼
            ┌──────────────┐
            │    CALLER    │
            │ hears reply  │
            └──────────────┘

---

## Overview

This project is a personal recreation of a production voice agent system. It demonstrates 
how modern AI and telephony technologies can be combined to build a fully automated, 
real-time voice assistant capable of handling live phone conversations with sub-2s response latency.

The system is designed around an event-driven, streaming architecture — meaning audio flows 
continuously between the caller, the backend, and the AI model without any buffering delays.

---

## How It Works

### Call Flow

1. **Incoming Call** — Caller dials in via Twilio. Twilio initiates a WebSocket connection to the FastAPI backend.
2. **Audio Streaming** — Raw audio from the caller is streamed in real-time over WebSocket to the backend.
3. **Voice Activity Detection** — Semantic VAD continuously monitors the audio stream and detects when the user has finished speaking.
4. **Audio Encoding** — Detected speech is µ-law encoded (G.711 standard — the format used by telephone networks).
5. **AI Processing** — Encoded audio is forwarded to OpenAI Realtime API (GPT-4o), which processes speech and generates a response in real-time.
6. **Response Streaming** — AI-generated audio response is streamed back to FastAPI, then forwarded to Twilio.
7. **Caller Hears Response** — Twilio plays the AI response to the caller in real-time.
8. **Transcript Storage** — Every utterance (caller + AI) is timestamped and saved to Redis for session memory and logging.

---

## Key Concepts

### Semantic VAD (Voice Activity Detection)
Unlike basic energy-based VAD that simply detects loudness, semantic VAD understands 
the context of speech — detecting natural pauses, sentence endings, and conversational 
turn-taking. This ensures the AI responds at the right moment without cutting off the caller.

### µ-law Encoding
Telephone networks transmit audio using G.711 µ-law encoding at 8kHz sampling rate. 
Raw PCM audio from the microphone must be converted to µ-law format before it can be 
sent over standard telephony infrastructure. This project handles that conversion internally.

### OpenAI Realtime API
The Realtime API allows direct audio-to-audio communication with GPT-4o — meaning 
speech goes in and speech comes out, without separate STT and TTS steps. This dramatically 
reduces latency compared to traditional pipelines.

### Redis Transcript Storage
Every message in the conversation — both from the caller and the AI — is stored in Redis 
with timestamps. This enables session continuity, call logging, and post-call analytics.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Telephony | Twilio |
| AI Model | OpenAI Realtime API (GPT-4o) |
| Backend | FastAPI, Python |
| Audio Processing | µ-law Encoding (G.711), WebSocket Streaming |
| Voice Detection | Semantic VAD |
| Session Storage | Redis |
| Protocol | WebSockets, RTP/UDP |

---

## Features

- Real-time audio streaming with sub-2s end-to-end response latency
- Semantic Voice Activity Detection for natural conversation turn-taking
- µ-law audio encoding — compatible with standard telephone networks (G.711)
- Automated transcript storage with timestamps in Redis
- Stateless backend — horizontally scalable
- Modular component design — each layer (VAD, encoding, AI, storage) is independent
- Production-ready architecture patterns — built from real production experience

---

## Performance

| Metric | Value |
|---|---|
| End-to-end response latency | < 2 seconds |
| Audio encoding format | G.711 µ-law @ 8kHz |
| Transcript storage | Real-time, per utterance |
| Scalability | Horizontal (stateless backend) |

---

## Use Cases

- Outbound sales call automation
- Inbound customer support voice bots
- Insurance query handling via voice
- Appointment scheduling via phone
- Any domain requiring real-time AI voice interaction

---

## Author

**Lokesh Chandra Roy**  
AI/ML Engineer — Specializing in Voice AI, LLM Orchestration, and RAG Systems  
[LinkedIn](https://www.linkedin.com/in/lokesh-roy02) · [GitHub](https://github.com/Lokeshroy2) · lokeshroy523@gmail.com
