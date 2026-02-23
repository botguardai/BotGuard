<p align="center">
  <img src="https://botguard.dev/logo.png" alt="BotGuard" width="80" />
</p>

<h1 align="center">BotGuard</h1>

<p align="center">
  <strong>Automated red-teaming & real-time firewall for AI agents</strong>
</p>

<p align="center">
  <a href="https://botguard.dev">Website</a> · <a href="https://pypi.org/project/botguard/">Python SDK</a> · <a href="https://www.npmjs.com/package/botguard">Node.js SDK</a> · <a href="https://botguard.dev/docs">Docs</a>
</p>

<p align="center">
  <a href="https://pypi.org/project/botguard/"><img src="https://img.shields.io/pypi/v/botguard.svg" alt="PyPI" /></a>
  <a href="https://www.npmjs.com/package/botguard"><img src="https://img.shields.io/npm/v/botguard.svg" alt="npm" /></a>
  <a href="https://opensource.org/licenses/MIT"><img src="https://img.shields.io/badge/License-MIT-blue.svg" alt="License: MIT" /></a>
</p>

---

Most AI agents ship without security testing. BotGuard fixes that.

Point BotGuard at your AI agent endpoint and it runs **70+ automated attack scenarios** aligned to the OWASP LLM Top 10 — prompt injection, jailbreaks, role-playing attacks, context overflow, indirect injection via tool/RAG responses, and more. You get a **scored security report** with reproduction steps for every vulnerability found.

Then deploy **Shield** — a real-time firewall that intercepts requests, detects adversarial inputs, and **blocks attacks before they reach your model**. Latency under 20ms.

> **BotGuard doesn't just find vulnerabilities — it fixes them.**  
> Get a security score, then use BotGuard's AI to generate a hardened system prompt that closes every vulnerability. Re-scan and watch your score go up.

---

## Features

| Feature | Description |
|---------|-------------|
| **Security Audit** | 70+ attack templates aligned to OWASP LLM Top 10 |
| **Real-time Shield** | Multi-tier firewall — regex → ML classifier → semantic analysis → AI judge |
| **MCP Tool Scanning** | Detect indirect prompt injection in tool responses |
| **RAG Pipeline Protection** | Scan document chunks for data poisoning before they reach your LLM |
| **PII Detection** | Automatically detect and redact sensitive data (SSN, emails, credit cards) |
| **Output Guardrails** | Validate bot responses to prevent data leaks |
| **Multi-Provider** | OpenAI, Anthropic Claude, Google Gemini, Azure OpenAI, Groq, and more |
| **CI/CD Integration** | REST API for automated testing on every deployment |
| **Prompt Hardening** | AI-generated secure system prompts based on audit results |
| **Security Certification** | Verifiable trust badges for your product |

---

## Quick Start

### Install

```bash
# Python
pip install botguard

# Node.js
npm install botguard
```

### Protect a Chatbot / AI Agent

**Python:**
```python
from botguard import BotGuard

guard = BotGuard(
    shield_id="sh_your_shield_id",   # from botguard.dev → Shield page
    api_key="sk-your-openai-key",
)

result = guard.chat.completions.create(
    model="gpt-4o",
    messages=[{"role": "user", "content": user_message}],
)

if result.blocked:
    print(result.shield.reason)  # "Attack detected: jailbreak_ignore"
else:
    print(result.content)        # Safe LLM response
```

**Node.js:**
```typescript
import { BotGuard } from 'botguard';

const guard = new BotGuard({
  shieldId: 'sh_your_shield_id',   // from botguard.dev → Shield page
  apiKey: 'sk-your-openai-key',
});

const result = await guard.chat.completions.create({
  model: 'gpt-4o',
  messages: [{ role: 'user', content: userMessage }],
});

if (result.blocked) {
  console.log(result.shield.reason); // "Attack detected: jailbreak_ignore"
} else {
  console.log(result.content);       // Safe LLM response
}
```

### Scan MCP Tool Responses

Detect indirect prompt injection hidden inside tool results — no LLM key needed.

```python
guard = BotGuard(shield_id="sh_your_shield_id")

tool_result = mcp_client.call_tool("web_search", {"query": user_query})
scan = guard.scan_tool_response(tool_result.text, tool_name="web_search")

if scan.blocked:
    raise ValueError(f"Injection detected: {scan.reason}")

return scan.safe_response
```

### Scan RAG Document Chunks

Remove poisoned documents before they reach your LLM.

```python
guard = BotGuard(shield_id="sh_your_shield_id")

chunks = vector_db.similarity_search(user_query, k=5)
result = guard.scan_chunks([c.page_content for c in chunks])

print(f"Blocked {result.blocked_count}/{result.total_count} poisoned chunks")
context = "\n\n".join(result.clean_chunks)  # safe chunks only
```

---

## How Shield Works

Shield uses a multi-tier analysis pipeline — each tier adds depth, and the request is blocked as soon as any tier detects a threat:

| Tier | Method | Latency |
|------|--------|---------|
| 1 | Regex pattern matching | ~1ms |
| 1.5 | ML classifier | ~5ms |
| 1.75 | Semantic similarity | ~50ms |
| 2.5 | HuggingFace DeBERTa | ~300ms |
| 3 | AI Judge (OpenAI / Groq) | ~500ms |

Most attacks are caught at Tier 1–1.5, keeping average latency **under 20ms**.

---

## Multi-Provider Support

BotGuard works as a drop-in proxy for any LLM provider:

```python
# OpenAI
guard.chat.completions.create(model="gpt-4o", messages=messages)

# Anthropic Claude
guard.chat.completions.create(model="claude-3-5-sonnet-20241022", messages=messages)

# Google Gemini
guard.chat.completions.create(model="gemini-1.5-pro", messages=messages)
```

---

## Plans & Pricing

|  | **Free** | **Starter** | **Pro** | **Business** |
|--|----------|-------------|---------|-------------|
| **Price** | $0/mo | $29/mo | $79/mo | $199/mo |
| **Security scans** | 25/mo | 50/mo | 200/mo | 1,000/mo |
| **Shield requests** | 5,000/mo | 10,000/mo | 50,000/mo | 200,000/mo |
| **Shield endpoints** | 1 | 3 | 10 | 50 |

**Start free at [botguard.dev](https://botguard.dev)** — no credit card required.

---

## Links

| | |
|---|---|
| **Website & Dashboard** | [botguard.dev](https://botguard.dev) |
| **Python SDK (PyPI)** | [pypi.org/project/botguard](https://pypi.org/project/botguard/) |
| **Node.js SDK (npm)** | [npmjs.com/package/botguard](https://www.npmjs.com/package/botguard) |
| **API Documentation** | [botguard.dev/docs](https://botguard.dev/docs) |
| **Cursor IDE Plugin** | [MCP Server for Cursor](https://github.com/user/botguard-cursor-plugin) |

---

## License

MIT
