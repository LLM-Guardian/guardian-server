# LLMGuardian Server

FastAPI server exposing [guardian-core](https://github.com/LLM-Guardian/guardian-core) detection as a REST API. Sits between your application and your LLM — every prompt and response passes through before either side acts on it.

## Endpoints

**POST /scan-prompt/** — scan user input before it reaches the model

**POST /scan-llm/** — scan model output before it reaches your application

Both return a `RiskModel`:

```json
{
  "query": "*",
  "markers": { "ExploitClassifier": "0.555079" },
  "score": 2.0,
  "passed": false,
  "risk": "high"
}
```

## Installation

```bash
pip install -r requirements.txt
```

## Run locally

```bash
uvicorn server.api:app --reload
```

## Deploy

Configured for Vercel via `vercel.json`. One-click deploy:

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/LLM-Guardian/guardian-server)

## Architecture

```
Your App → guardian-server → LLM
              ↑
         guardian-core
    (prompt injection, jailbreak,
     PII, hallucination detection)
```

`guardian-server` wraps [guardian-core](https://github.com/LLM-Guardian/guardian-core), a fork of [last_layer](https://github.com/arekusandr/last_layer) extended for deployment-level oversight verification.

## Part of LLMGuardian

- [guardian-core](https://github.com/LLM-Guardian/guardian-core) — detection engine
- [guardian-server](https://github.com/LLM-Guardian/guardian-server) — this repo, REST API wrapper
- [llm-guardian.surge.sh](https://llm-guardian.surge.sh) — product page, free trial
