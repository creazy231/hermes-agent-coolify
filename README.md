<div align="center">

# ⚡ Hermes Agent on Coolify

### Deploy a powerful AI agent to Telegram, Discord, Slack & more — in minutes.

[![Coolify](https://img.shields.io/badge/Coolify-v4+-6C47FF?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCIgZmlsbD0id2hpdGUiPjxwYXRoIGQ9Ik0xMiAyQzYuNDggMiAyIDYuNDggMiAxMnM0LjQ4IDEwIDEwIDEwIDEwLTQuNDggMTAtMTBTMTcuNTIgMiAxMiAyem0wIDE4Yy00LjQyIDAtOC0zLjU4LTgtOHMzLjU4LTggOC04IDggMy41OCA4IDgtMy41OCA4LTggOHoiLz48L3N2Zz4=)](https://coolify.io)
[![Hermes Agent](https://img.shields.io/badge/Hermes_Agent-NousResearch-FF6B35?style=for-the-badge&logo=github)](https://github.com/NousResearch/hermes-agent)
[![Docker](https://img.shields.io/badge/Docker_Compose-Ready-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://docs.docker.com/compose/)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](https://github.com/NousResearch/hermes-agent/blob/main/LICENSE)

---

**One `docker-compose.yml`. No cloning. No forking. Just deploy.**

The image builds itself by pulling the source from GitHub at build time.

</div>

## 📋 Table of Contents

- [Prerequisites](#-prerequisites)
- [Quick Start](#-quick-start)
- [Environment Variables](#-environment-variables)
  - [LLM Provider](#1--llm-provider)
  - [Messaging Platforms](#2--messaging-platforms)
  - [Tool API Keys](#3--tool-api-keys)
  - [Other Settings](#4--other-settings)
- [Data Persistence](#-data-persistence)
- [Pinning a Version](#-pinning-a-version)
- [Updating](#-updating)
- [Architecture](#-architecture)
- [Troubleshooting](#-troubleshooting)
- [License](#-license)

---

## ✅ Prerequisites

| Requirement | Details |
|:--|:--|
| **Coolify** | A running [Coolify](https://coolify.io) instance (v4+) |
| **LLM API Key** | OpenRouter, OpenAI-compatible, or a built-in provider |
| **Bot Token** | At least one messaging platform (Telegram, Discord, Slack, etc.) |

---

## 🚀 Quick Start

```
1.  Coolify  →  Projects  →  Pick a project  →  + New  →  Docker Compose
2.  Select "Empty Compose File" (or point to this repo)
3.  Paste the contents of docker-compose.yml
4.  Go to Environment Variables tab and set your keys
5.  Click Deploy
```

> **That's it.** First build takes a few minutes (clones repo, installs deps). Subsequent rebuilds use Docker layer cache.

---

## 🔧 Environment Variables

### 1. 🧠 LLM Provider

You need **one** of the following:

<details>
<summary><b>Option A — OpenRouter</b> (easiest — access to many models)</summary>

| Variable | Value |
|:--|:--|
| `OPENROUTER_API_KEY` | Your key from [openrouter.ai/keys](https://openrouter.ai/keys) |
| `LLM_MODEL` | e.g. `anthropic/claude-opus-4.6`, `openai/gpt-4o`, `google/gemini-3-flash-preview` |

</details>

<details>
<summary><b>Option B — Custom OpenAI-compatible provider</b></summary>

Works with **vLLM**, **Ollama**, **LiteLLM proxy**, **Together**, **Groq**, local **llama.cpp**, or any OpenAI-compatible API.

| Variable | Value |
|:--|:--|
| `OPENAI_BASE_URL` | Your provider's base URL (e.g. `https://api.together.xyz/v1`) |
| `OPENAI_API_KEY` | API key for that provider |
| `LLM_MODEL` | Model name your provider expects (e.g. `meta-llama/Llama-3-70b-chat-hf`) |

</details>

<details>
<summary><b>Option C — Built-in providers</b></summary>

| Provider | Key Variable | Base URL Variable |
|:--|:--|:--|
| z.ai / ZhipuAI | `GLM_API_KEY` | `GLM_BASE_URL` *(optional)* |
| Kimi / Moonshot | `KIMI_API_KEY` | `KIMI_BASE_URL` *(optional)* |
| MiniMax | `MINIMAX_API_KEY` | `MINIMAX_BASE_URL` *(optional)* |

</details>

---

### 2. 💬 Messaging Platforms

Configure **at least one**:

<details>
<summary><b>Telegram</b></summary>

| Variable | Description |
|:--|:--|
| `TELEGRAM_BOT_TOKEN` | From [@BotFather](https://t.me/BotFather) |
| `TELEGRAM_ALLOWED_USERS` | Comma-separated Telegram user IDs |
| `TELEGRAM_HOME_CHANNEL` | *(Optional)* Channel ID for the bot's "home" |

</details>

<details>
<summary><b>Discord</b></summary>

| Variable | Description |
|:--|:--|
| `DISCORD_BOT_TOKEN` | From the [Discord Developer Portal](https://discord.com/developers/applications) |
| `DISCORD_ALLOWED_USERS` | Comma-separated Discord user IDs |
| `DISCORD_HOME_CHANNEL` | *(Optional)* Channel ID for the bot's "home" |

</details>

<details>
<summary><b>Slack</b></summary>

| Variable | Description |
|:--|:--|
| `SLACK_BOT_TOKEN` | `xoxb-...` from [Slack App settings](https://api.slack.com/apps) |
| `SLACK_APP_TOKEN` | `xapp-...` for Socket Mode |
| `SLACK_ALLOWED_USERS` | Comma-separated Slack user IDs |

</details>

<details>
<summary><b>WhatsApp</b></summary>

| Variable | Description |
|:--|:--|
| `WHATSAPP_ENABLED` | `true` to enable |
| `WHATSAPP_ALLOWED_USERS` | Comma-separated phone numbers (e.g. `15551234567`) |

> **Note:** Requires a one-time QR code pairing. After deploying, exec into the container and run `hermes whatsapp` to pair your device.

</details>

<details>
<summary><b>Signal</b></summary>

| Variable | Description |
|:--|:--|
| `SIGNAL_HTTP_URL` | URL of your [signal-cli-rest-api](https://github.com/bbernhard/signal-cli-rest-api) instance |
| `SIGNAL_ACCOUNT` | Your Signal phone number |
| `SIGNAL_ALLOWED_USERS` | Comma-separated phone numbers |

</details>

<details>
<summary><b>Home Assistant</b></summary>

| Variable | Description |
|:--|:--|
| `HASS_URL` | e.g. `http://homeassistant.local:8123` |
| `HASS_TOKEN` | Long-lived access token from HA |

</details>

---

### 3. 🛠 Tool API Keys

*All optional — enable the capabilities you need:*

| Variable | Service | Get a key |
|:--|:--|:--|
| `FIRECRAWL_API_KEY` | Web search & extraction | [firecrawl.dev](https://firecrawl.dev) |
| `FAL_KEY` | Image generation (FLUX) | [fal.ai](https://fal.ai) |
| `HONCHO_API_KEY` | Cross-session user modeling | [honcho.dev](https://app.honcho.dev) |
| `VOICE_TOOLS_OPENAI_KEY` | Whisper transcription & TTS | [platform.openai.com](https://platform.openai.com/api-keys) |
| `BROWSERBASE_API_KEY` | Cloud browser automation | [browserbase.com](https://browserbase.com) |
| `BROWSERBASE_PROJECT_ID` | Browserbase project | *(from your Browserbase dashboard)* |

---

### 4. ⚙️ Other Settings

| Variable | Default | Description |
|:--|:--|:--|
| `HERMES_REF` | `main` | Git branch/tag to build from *(build argument)* |
| `TERMINAL_ENV` | `local` | Terminal backend (`local`, `docker`, `ssh`) |
| `TERMINAL_TIMEOUT` | `60` | Command timeout in seconds |
| `HERMES_MAX_ITERATIONS` | `90` | Max tool-calling iterations per conversation |
| `GATEWAY_ALLOW_ALL_USERS` | `false` | Allow all users without allowlist |
| `HERMES_HUMAN_DELAY_MODE` | `off` | Response pacing (`off`, `natural`, `custom`) |
| `CONTEXT_COMPRESSION_ENABLED` | `true` | Auto-compress long conversations |

---

## 💾 Data Persistence

The `hermes-data` volume is mounted at `/root/.hermes` inside the container and persists across redeployments:

| Data | Description |
|:--|:--|
| **Memory** | The agent's learned knowledge across sessions |
| **Skills** | Autonomously created and improved skills |
| **Config** | `config.yaml` and `auth.json` |
| **Logs** | Session trajectories |

---

## 📌 Pinning a Version

By default the image builds from the `main` branch. To pin to a specific release:

1. In Coolify, set the build argument `HERMES_REF` to a tag or commit hash (e.g. `v1.0.0`)
2. Redeploy

---

## 🔄 Updating

1. In Coolify, click **Rebuild** on the service
2. The Docker build will `git clone` the latest `main` (or your pinned `HERMES_REF`)

> **Tip:** Disable Docker build cache in Coolify if the rebuild serves a cached old image.

---

## 🏗 Architecture

```
┌──────────────────────────────────────────────────┐
│  Coolify Server                                  │
│                                                  │
│  ┌────────────────────────────────────────────┐  │
│  │  hermes-gateway container                  │  │
│  │                                            │  │
│  │  python -m gateway.run                     │  │
│  │    ├── Telegram adapter  ──► Telegram API  │  │
│  │    ├── Discord adapter   ──► Discord API   │  │
│  │    ├── Slack adapter     ──► Slack API     │  │
│  │    ├── WhatsApp adapter  ──► WhatsApp      │  │
│  │    ├── Signal adapter    ──► Signal API    │  │
│  │    └── Home Assistant    ──► HA API        │  │
│  │                                            │  │
│  │  /root/.hermes  ◄──► hermes-data volume    │  │
│  └────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────┘
```

> The gateway connects **outbound** to messaging APIs — no inbound ports, domains, or SSL certificates needed.

---

## 🔍 Troubleshooting

<details>
<summary><b>Build fails with <code>uv: not found</code></b></summary>

This was a known issue with `ENV PATH` inside `dockerfile_inline`. The compose file uses absolute paths (`/root/.local/bin/uv`) to avoid this. Make sure you're using the latest `docker-compose.yml` from this repo.

</details>

<details>
<summary><b>Bot requires interactive authentication (Telegram, WhatsApp, etc.)</b></summary>

Some platforms require a one-time interactive approval. After deploying, open a terminal to the container via the Coolify web UI:

1. Click **Terminal** in the left sidebar of your Coolify dashboard
2. Select the hermes-gateway container from the resource list
3. Follow the prompts to complete authentication

> If the terminal option is not available, go to **Servers** → select your server → **Configuration** → **Advanced** and enable terminal access. Ports `6001` and `6002` must be open on your Coolify server.

</details>

<details>
<summary><b>Container starts but bot doesn't respond</b></summary>

Check the logs in Coolify. Common causes:
- Missing or invalid bot token
- Your user ID is not in the `*_ALLOWED_USERS` list
- No messaging platform configured at all

</details>

<details>
<summary><b>Want to use Docker-in-Docker for the terminal backend</b></summary>

Set `TERMINAL_ENV=docker` and mount the Docker socket:

```yaml
volumes:
  - hermes-data:/root/.hermes
  - /var/run/docker.sock:/var/run/docker.sock
```

</details>

<details>
<summary><b>Want to exec into the container</b></summary>

In Coolify, click **Terminal** in the left sidebar and select the hermes-gateway container. This gives you a full interactive shell in your browser — no SSH or `docker exec` needed. From there you can run:

```bash
hermes status
```

</details>

---

## 📄 License

The upstream [Hermes Agent](https://github.com/NousResearch/hermes-agent) project is MIT licensed. This deployment configuration is provided as-is.

---

<div align="center">

**Built with** ❤️ **for the self-hosting community**

[Report an Issue](https://github.com/NousResearch/hermes-agent/issues) · [Hermes Agent Docs](https://github.com/NousResearch/hermes-agent)

</div>
