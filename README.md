# 🦞 Clawdboss Lite

**Minimal, security-hardened OpenClaw setup by NanoFlow.**

Same security, same WAL Protocol, same optional tools as [Clawdboss](https://github.com/NanoFlow-io/clawdboss) — but without the personality onboarding. For advanced users who want to customize their agent's identity manually.

## Clawdboss vs Clawdboss Lite

| | **Clawdboss** | **Clawdboss Lite** |
|---|---|---|
| User questionnaire | Role, description, use case, extras | Name + timezone only |
| Agent personality | 6 vibes, mission, expertise | Generic defaults |
| Security hardening | ✅ | ✅ |
| WAL Protocol | ✅ | ✅ |
| 3-Layer Memory | ✅ | ✅ |
| Optional tools (13) | ✅ | ✅ |
| Built-in skills wizard | ✅ | ✅ |
| Multi-agent tiers | ✅ | ✅ |
| Best for | First-time users, non-technical | Developers, power users |

**Use Clawdboss Lite when** you want the infrastructure without the hand-holding. You'll customize `SOUL.md`, `USER.md`, and other workspace files yourself after install.

## Quick Start

### Ubuntu VPS (Fresh Install)

```bash
apt-get update && apt-get install -y git
git clone https://github.com/NanoFlow-io/clawdboss-lite.git
cd clawdboss-lite && ./setup.sh
```

The wizard auto-installs Node.js 22, Python, build tools, and OpenClaw. It asks for:

1. Your name and timezone
2. Agent name, pronouns, emoji
3. Agent tier (Solo / Team / Squad)
4. Interface (Discord / NanoFlow Console / Both)
5. LLM provider and API keys
6. Optional tools (13 available, each Y/n)
7. Built-in skills activation

Then customize your agent by editing the workspace files directly:

```bash
# Edit your agent's personality
nano ~/.openclaw/workspace/SOUL.md

# Edit what the agent knows about you
nano ~/.openclaw/workspace/USER.md

# Edit operating rules
nano ~/.openclaw/workspace/AGENTS.md
```

## What You Get

- **Multi-agent architecture** — Main agent + optional specialist agents (Comms, Research, Security)
- **Security-first** — Prompt injection defense, anti-loop rules, content tagging, credential isolation
- **WAL Protocol** — Write-Ahead Log for corrections and decisions that survive context loss
- **3-Layer Memory** — L1 (Brain) → L2 (Memory) → L3 (Reference) with file budgets
- **Working Buffer** — Danger zone logging to survive context compaction
- **13 optional tools** — Knowledge graphs, web scraping, browser automation, marketing skills, and more
- **Env-based secrets** — All API keys in `.env`, never in config files

## Post-Install Customization

After running the wizard, personalize these files:

| File | Purpose | What to change |
|------|---------|----------------|
| `SOUL.md` | Agent personality | Voice, values, mission, expertise |
| `USER.md` | About you | Your background, preferences, work context |
| `IDENTITY.md` | Quick reference | Name, pronouns, emoji |
| `AGENTS.md` | Operating rules | Add custom rules, adjust protocols |
| `TOOLS.md` | Machine-specific notes | SSH hosts, camera names, voice preferences |
| `HEARTBEAT.md` | Periodic tasks | Add recurring checks |

See the [Clawdboss docs](https://github.com/NanoFlow-io/clawdboss/tree/main/docs) for detailed customization guides.

## Requirements

- Ubuntu 22.04+ (or any Linux with bash)
- Node.js 22+ (auto-installed by wizard)
- 2GB+ RAM recommended (1GB works for Solo tier)
- An LLM provider (GitHub Copilot, OpenAI, Anthropic, or others)
- A Discord bot token (if using Discord interface)

## License

MIT — see [LICENSE](LICENSE) for details.
