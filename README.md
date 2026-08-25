# 📈 Voice Agent — AI Stock Analysis

> Voice-enabled AI agent that fetches live stock market data, runs LLM-powered analysis, and delivers spoken summaries — real-time financial intelligence via voice.

![n8n](https://img.shields.io/badge/n8n-Workflow-orange?style=flat-square&logo=n8n)
![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4o-412991?style=flat-square&logo=openai)
![Voice](https://img.shields.io/badge/Voice-TTS%20%2F%20STT-blue?style=flat-square)
![Finance](https://img.shields.io/badge/Data-Live%20Stock%20API-red?style=flat-square)

---

## 📌 Overview

A fully automated voice agent pipeline built in **n8n** that:

1. Accepts a voice or text query (e.g. *"Analyze AAPL for today"*)
2. Fetches **live stock market data** via financial API
3. Runs **GPT-4o powered analysis** — trend, sentiment, key metrics
4. Converts the analysis to **spoken audio** via Text-to-Speech
5. Delivers the voice summary back to the user

Combines real-time financial data, LLM reasoning, and voice synthesis in one automated pipeline.

---

## 🏗️ Architecture

```
Voice Input / Webhook
        │
        ▼
Speech-to-Text (Whisper)
        │
        ▼
Intent Parser (Extract ticker + query type)
        │
        ▼
Live Stock API (Price, Volume, OHLC, News)
        │
        ▼
GPT-4o Analysis (Trend + Sentiment + Summary)
        │
        ▼
Text-to-Speech (OpenAI TTS / ElevenLabs)
        │
        ▼
Voice Output / Response
```

---

## 🛠️ Tech Stack

| Component | Tool |
|-----------|------|
| Orchestration | n8n (self-hosted) |
| Speech-to-Text | OpenAI Whisper |
| Stock Data | Alpha Vantage / Polygon.io |
| LLM Analysis | GPT-4o |
| Text-to-Speech | OpenAI TTS (`tts-1-hd`) |
| Trigger | Webhook / Voice API |

---

## ⚙️ Setup

### Prerequisites
- n8n instance
- OpenAI API key (Whisper + GPT-4o + TTS)
- Stock data API key (Alpha Vantage — free tier available)

### Credentials Required in n8n
```
- OpenAI API
- Alpha Vantage API (or Polygon.io)
- HTTP Header Auth (for voice delivery endpoint)
```

### Import Workflow
1. Download `workflow.json`
2. Open n8n → **Workflows** → **Import from file**
3. Connect credentials
4. Test with a stock ticker ✅

---

## 🔄 How It Works

### Trigger — Send a query
```json
{
  "query": "Give me a summary of Tesla stock today",
  "ticker": "TSLA"
}
```

### Analysis Output (GPT-4o)
The LLM receives structured stock data and produces:
- **Price trend** (bullish/bearish/neutral)
- **Key metrics** (P/E, volume, 52-week range)
- **News sentiment** summary
- **Plain-English recommendation** (not financial advice)

### Voice Output
The analysis is passed to OpenAI TTS and returned as an `.mp3` audio file or streamed directly.

---

## 📊 Sample Analysis Output

```
"Tesla is trading at $248.50, up 2.3% today with above-average volume.
Recent news sentiment is moderately positive, driven by Cybertruck 
delivery updates. The stock is trading near its 30-day moving average.
Short-term momentum appears bullish."
```

---

## ⚠️ Disclaimer

This tool is for **educational and informational purposes only**.  
It does not constitute financial advice. Always do your own research.

---

## 💡 Use Cases

- Personal AI financial briefing
- Morning market summary automation
- Portfolio monitoring voice alerts
- Financial research assistant

---

## 👤 Author

**Zabir** — Freelance Automation Developer  
🔗 [Upwork Profile](https://www.upwork.com) · 📧 Contact via GitHub

---

> Built with n8n + OpenAI + Live Stock API | Chittagong, Bangladesh 🇧🇩
