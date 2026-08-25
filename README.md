# 📈 Voice Agent — AI Stock Analysis (n8n)

> Voice-enabled AI agent that fetches live stock market data, runs LLM-powered analysis, and delivers spoken summaries — real-time financial intelligence via voice, fully automated in n8n.

![n8n](https://img.shields.io/badge/n8n-Workflow-orange?style=flat-square&logo=n8n)
![OpenAI](https://img.shields.io/badge/OpenAI-Whisper%20%2B%20GPT--4o%20%2B%20TTS-412991?style=flat-square&logo=openai)
![Voice](https://img.shields.io/badge/Voice-STT%20%2F%20TTS-blue?style=flat-square)
![Finance](https://img.shields.io/badge/Data-Live%20Stock%20API-red?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-blue?style=flat-square)

---

## 📌 Overview

A fully automated **voice agent pipeline** built in **n8n** that combines live financial data, LLM reasoning, and voice synthesis in one workflow:

1. Accepts a voice or text query (e.g. *"Analyze TSLA for today"*)
2. Converts speech to text via **OpenAI Whisper**
3. Fetches **live stock market data** — price, volume, OHLC, news sentiment
4. Runs **GPT-4o powered analysis** — trend direction, key metrics, news sentiment
5. Converts the analysis to **natural spoken audio** via OpenAI TTS
6. Delivers the voice response back to the user

**Financial API → LLM Reasoning → Voice Synthesis — end-to-end, no manual steps.**

---

## 🏗️ Architecture

```
Voice Input (Audio file / Webhook)
           │
           ▼
  OpenAI Whisper (STT)
  — transcribes query to text
           │
           ▼
  Intent Parser
  — extracts ticker symbol + query type
  — e.g. "TSLA", "daily summary"
           │
           ▼
  Live Stock API
  (Alpha Vantage / Polygon.io)
  — Price, Volume, OHLC, 52W range, News
           │
           ▼
  GPT-4o Analysis
  — Trend direction (bullish / bearish / neutral)
  — Key metrics summary
  — News sentiment
  — Plain-English briefing
           │
           ▼
  OpenAI TTS (tts-1-hd)
  — converts analysis to audio
           │
           ▼
  Voice Output (.mp3) / Webhook Response
```

---

## 🛠️ Tech Stack

| Component | Tool |
|---|---|
| Orchestration | n8n (self-hosted or cloud) |
| Speech-to-Text | OpenAI Whisper (`whisper-1`) |
| Stock Market Data | Alpha Vantage API (free tier available) |
| LLM Analysis | OpenAI GPT-4o |
| Text-to-Speech | OpenAI TTS (`tts-1-hd`) |
| Trigger | Webhook (voice upload or text POST) |
| Output | `.mp3` audio file / JSON + audio |

---

## ⚙️ Setup & Installation

### Prerequisites

- **n8n** instance (self-hosted via Docker or n8n Cloud)
- **OpenAI** API key (covers Whisper + GPT-4o + TTS) → [platform.openai.com](https://platform.openai.com)
- **Alpha Vantage** API key (free tier: 25 requests/day) → [alphavantage.co](https://www.alphavantage.co/support/#api-key)

### Step 1 — Clone the Repository

```bash
git clone https://github.com/omaralzabir/voice-stock-analysis-agent-n8n.git
cd voice-stock-analysis-agent-n8n
```

### Step 2 — Import Workflow into n8n

1. Open your n8n instance
2. Go to **Workflows** → **Import from File**
3. Select `workflow.json`
4. Click **Import**

### Step 3 — Connect Credentials

| Credential | Where to get it |
|---|---|
| OpenAI API | [platform.openai.com/api-keys](https://platform.openai.com/api-keys) |
| Alpha Vantage API | [alphavantage.co/support/#api-key](https://www.alphavantage.co/support/#api-key) |

### Step 4 — Test the Agent

**Text query (simple test):**
```bash
curl -X POST https://your-n8n-instance/webhook/stock-agent \
  -H "Content-Type: application/json" \
  -d '{
    "query": "Give me a summary of Apple stock today",
    "ticker": "AAPL"
  }'
```

**Voice query:**
```bash
curl -X POST https://your-n8n-instance/webhook/stock-agent \
  -F "audio=@your_query.mp3"
```

---

## 🔄 How It Works

### 1. Input

Send a text or voice query:

```json
{
  "query": "How is Tesla performing today?",
  "ticker": "TSLA"
}
```

### 2. Stock Data Fetched

The workflow calls Alpha Vantage and retrieves:

| Data Point | Example |
|---|---|
| Current price | $248.50 |
| Daily change | +2.3% |
| Volume | 98.4M (above avg) |
| 52-week range | $138.80 – $278.98 |
| News headlines | Latest 3 articles |

### 3. GPT-4o Analysis

Structured data is passed to GPT-4o with a strict analysis prompt:

```
Given this stock data for {ticker}, produce a concise voice briefing covering:
1. Price and daily movement
2. Volume context (above/below average)
3. News sentiment (positive / negative / neutral)
4. Short-term momentum direction
Keep it under 150 words, plain English, no jargon.
```

### 4. Voice Output

```
"Tesla is trading at $248.50, up 2.3% today with above-average volume.
Recent news sentiment is moderately positive, driven by Cybertruck
delivery updates. The stock is trading near its 30-day moving average.
Short-term momentum appears bullish."
```

The text is converted to `.mp3` audio via OpenAI TTS (`tts-1-hd`, voice: `nova`) and returned.

---

## 📁 Repository Structure

```
voice-stock-analysis-agent-n8n/
├── workflow.json      # n8n workflow export (import this)
├── README.md
└── docs/
    └── architecture.png
```

---

## 🔧 Customisation

| Parameter | Where to change | Default |
|---|---|---|
| TTS voice style | OpenAI TTS node | `nova` |
| Stock data source | HTTP Request node | Alpha Vantage |
| Analysis length | System prompt | 150 words |
| LLM model | OpenAI node | `gpt-4o` |
| Output format | Respond to Webhook node | `.mp3` |

---

## ⚠️ Disclaimer

This tool is for **educational and informational purposes only**.
It does not constitute financial advice. Always conduct your own research before making investment decisions.

---

## 💡 Use Cases

- **Morning market briefing** — automated daily stock audio summary
- **Portfolio monitoring** — voice alerts on price movements
- **Financial research assistant** — quick spoken overviews before meetings
- **Trading preparation** — pre-market sentiment check via voice

---

## 👤 Author

**Omar Al Zabir** — n8n Automation Engineer & AI Workflow Developer
📧 omaralzabir7@gmail.com
🔗 [LinkedIn](https://www.linkedin.com/in/omaralzabir) · [GitHub](https://github.com/omaralzabir)
📍 Chittagong, Bangladesh

---

> Built with n8n · OpenAI (Whisper + GPT-4o + TTS) · Alpha Vantage | 🇧🇩 Chittagong, Bangladesh
