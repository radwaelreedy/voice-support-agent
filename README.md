# voice-support-agent

A simple customer support agent that works with **text and voice**. Built with free tools only.

```
Customer types ──────────────┐
                             ▼
Customer speaks → STT → Agent (LLM + 3 tools) → Reply → Chat / TTS → Customer hears
```

## 1. Use case

E-commerce support agent. It checks orders, issues small refunds, and hands off to a human when needed.

## 2. LLM and provider (free)

| | |
|---|---|
| **Provider** | Groq (Groq API) |
| **LLM** | Llama (open-source model) |
| **Cost** | Free tier |

Voice uses the browser's built-in speech APIs, also free:

| Part | Choice |
|---|---|
| Speech to text | Browser Web Speech API |
| Text to speech | Browser `speechSynthesis` |

Fully offline alternative: run a local model with [Ollama](https://ollama.com). Free limits can change, so check the provider's current terms.

## 3. Tools

| Tool | What it does |
|---|---|
| `get_order_status(order_id)` | Looks up an order |
| `issue_refund(order_id, amount)` | Refunds up to $100 |
| `escalate_to_human(summary)` | Hands off to a person |

The model outputs a JSON tool call and the code runs it:

```json
{ "name": "issue_refund", "input": { "order_id": "ORD-555", "amount": 60 } }
```

## 4. System prompt

```
You are Nora, a customer support agent for ShopEase, an online store.
Help customers with their orders and refunds quickly and politely.

Tools:
- get_order_status: use it whenever a customer mentions an order.
- issue_refund: only after checking the order and getting the customer's confirmation. Max $100.
- escalate_to_human: if the refund is above $100, the customer is angry, or you can't solve the issue.

Rules:
1. Ask for the order ID if it's missing.
2. Never make up information.
3. Only discuss ShopEase orders and support.

Tone: friendly, short, professional.
For voice: reply in 1-2 short sentences, no lists or emojis.
```

## Run

```bash
git clone https://github.com/radwaelreedy/voice-support-agent.git
cd voice-support-agent
pip install -r requirements.txt
cp .env.example .env    # add your free API key
python app.py
```
