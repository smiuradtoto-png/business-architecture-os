# Business Hypothesis Engine

A live fragment of a larger **Business Architecture OS** — an AI-assisted system for turning limited business inputs into structured, falsifiable hypotheses (one AI recommendation + three structurally distinct alternatives), each carrying its own confidence, expected benefit, and risk.

Built as part of my application for the AI Growth Marketing Manager role. Rather than a slide deck, this demonstrates how I design AI-assisted decision systems in practice: hypotheses instead of guesses, explicit confidence levels instead of false certainty, and a decision log instead of overwritten opinions.

**Live demo:** `https://smiuradtoto-png.github.io/business-architecture-os/`

---

## What this shows

- **Business Definition → Hypothesis flow**: the input fields (company, market, business type, budget, objective) map to the Business Definition module of the broader OS; the output maps to the Business Hypothesis module.
- **Facts / Assumptions / Unknowns are never mixed.** The engine is instructed to mark what it doesn't know rather than fill gaps with invented certainty.
- **Every hypothesis carries Reason, Confidence, Expected Benefit, and Risk** — never a bare recommendation.
- **Accept / Modify / Reject** on each hypothesis, logged as a new version rather than overwriting the previous one.
- **Localized UI (EN / JA / ES / PT), canonical English values underneath** — display language and the data sent to the model are deliberately decoupled.

## Architecture

```
GitHub Pages (this repo)          Cloudflare Worker (separate, not in this repo)
┌─────────────────────┐           ┌──────────────────────────────┐
│  index.html          │  ─POST─▶ │  hypothesis-proxy             │
│  (static, no secrets)│           │  - checks Origin              │
└─────────────────────┘           │  - per-IP daily rate limit    │
                                   │    (Cloudflare KV, JST date)  │
                                   │  - holds ANTHROPIC_API_KEY    │
                                   │  - forwards to Claude API     │
                                   └──────────────────────────────┘
```

The frontend never holds an API key. All model calls, rate limiting, and origin checks happen in a Cloudflare Worker proxy that isn't part of this public repo (the proxy source is kept in a private location since — while it holds no secret itself — there's no reason to publish infrastructure code that isn't part of the demo).

## Running this locally

This is a static file with no build step.

```bash
git clone https://github.com/smiuradtoto-png/business-architecture-os.git
cd business-architecture-os
# open index.html directly, or serve it:
python3 -m http.server 8000
```

Note: hypothesis generation will only work once `API_ENDPOINT` in `index.html` points to a deployed proxy Worker with a valid `ANTHROPIC_API_KEY`. Without that, the form still renders and validates input, but generation will fail against a placeholder endpoint.

## Part of a larger system

This module is one piece of a broader Business Architecture OS covering Business Definition → Hypothesis → Research → Strategy → Execution → Growth → AI Optimization. This repo intentionally scopes to the Hypothesis module to keep the demo focused and reviewable.
