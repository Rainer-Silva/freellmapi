# INSTRUCTIONS.md — FreeLLMAPI (Rainer-Silva fork)

This file was added to the fork for Rainer Andrade Silva on 12 November 2026 (Europe/Stockholm). It is not part of the upstream project.

- **Upstream:** https://github.com/tashfeenahmed/freellmapi
- **This fork:** https://github.com/Rainer-Silva/freellmapi
- **Upstream site:** https://freellmapi.co
- **License:** MIT
- **Reviewed snapshot:** 50,879 stars, 7,069 forks, last upstream commit 11 Nov 2026, 18:58 CET (Europe/Stockholm), latest release v0.16.26. The Instagram reel claimed about 47k stars; the live count is in the same range.

## What it is

FreeLLMAPI is a self-hosted, OpenAI-compatible router. You add *your own* free-tier API keys from many providers (Google, Groq, Mistral, OpenRouter, NVIDIA, Hugging Face, and others). The router exposes one local `/v1` endpoint, picks a healthy key that is still under its free-tier cap, and fails over when a provider rate-limits you.

The “7.4 billion tokens per month / 34 providers / 635 endpoints” figure is the *sum of listed free-tier caps across the catalog*, not a pool of tokens you receive automatically. You only get capacity for the providers whose keys you add, and only within each provider’s own free-tier limits. The project itself says it is for **personal experimentation and learning, not production**.

The router can also run a **prompt-compression** pipeline (see upstream `docs/en/compression/`). That is the only part that can reduce *token count*. The rest mainly reduces *cost* by sending work to free models.

## Can it cut token usage / cost?

| Question | Honest answer |
|---|---|
| Cut Grok Bot / this assistant’s usage? | **No.** Grok Bot’s own traffic cannot be rerouted through this. |
| Cut cost in your own apps, scripts, or coding agents? | **Maybe, if you bring your own free-tier keys** and point those tools at `http://localhost:3001`. Free models are weaker and slower than paid frontier models, and quality drops as daily caps are hit. |
| Cut token count? | **Only if you turn on prompt compression**, and only for requests that go through this router. Routing alone does not shrink prompts. |
| Fit for production or legal / citizenship work? | **No.** Upstream is explicit: no SLA, variable quality, personal experimentation only. |

A $19/year or $49 lifetime **Premium** plan only unlocks a same-day model catalog feed. The router stays free and MIT-licensed. **Do not buy Premium, or any paid upstream API, without Rainer’s spend approval.**

## Setup (do not run until you have decided to try it)

Preferred path: read the install script *before* running it. Do not pipe an unread script into bash.

1. Read https://freellmapi.co/install.sh (or the copy in upstream `docs/en/install/01-install.md`).
2. Install Docker if it is not already present.
3. Only then run the installer, or follow the Docker Compose steps in the docs, which create `~/freellmapi`, generate a local encryption key, and start the container.
4. Open http://localhost:3001, add *your* provider keys on the Keys page, set the fallback chain, and copy the unified local API key.
5. Point an OpenAI-compatible client at `http://localhost:3001` with that unified key.

Desktop installers exist for macOS and Windows; there is also a Play Store / App Store app. Server installs use an email + password for the dashboard. Keys are encrypted at rest in a local SQLite database. Keep `.env` and `ENCRYPTION_KEY` off shared drives and out of git and chat.

To use the fork instead of upstream later, change clone/install URLs to `https://github.com/Rainer-Silva/freellmapi`. For catalog updates and releases, tracking upstream is usually better.

## Risks and caveats

- **Your keys stay your responsibility.** This is not a shared key pool from the project. Putting keys into a third-party *hosted* service would be a different, higher risk; prefer a local/self-hosted install.
- **Provider terms still apply.** Upstream’s disclaimer says you must follow each provider’s terms. Stacking free tiers through a proxy can still violate a provider’s ToS (for example if the free tier is limited to direct interactive use). Review `docs/en/architecture/00-high-level-index.md#terms-of-service-review` before adding a key.
- **curl \| bash install.** Always read `install.sh` first.
- **Do not expose the server to the internet.** It binds to `127.0.0.1` by default. `HOST_BIND=0.0.0.0` is a documented opt-in and has no multi-tenant auth.
- **Star counts are unusually high** for a repo created on 21 April 2026 (about 249 stars/day). Treat popularity as marketing, not a security review.
- **No passwords or API keys in chat.**

## Cost note

Running anything that needs paid APIs, Premium, cloud servers, or app-store purchases requires Rainer’s spend approval first. Forking and reading the code is free. A local Docker trial with only free-tier keys you already have should cost 0 kr, but check each provider’s terms and any usage that might flip a key to a paid plan.

## Recommendation

Worth a cautious local trial later, for personal scripts and coding agents only, after reading the install script and adding keys you already own. Do not use it for Grok Bot, legal work, or anything that needs a reliable model. Do not buy Premium unless approved.
