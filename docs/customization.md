# Customization Guide

## Changing Your Agent's Personality

Edit `~/.openclaw/workspace/SOUL.md`. This is the agent's core identity file — it defines personality, tone, and values.

## Adding Skills

OpenClaw has a skill marketplace. Browse and install:

```bash
# Search for skills
clawhub search "image generation"

# Install a skill
clawhub install openai-image-gen

# Configure the skill
openclaw config set skills.entries.openai-image-gen.apiKey '${OPENAI_API_KEY}'
```

## Adding More Discord Channels

1. Create the channel in Discord
2. Add it to your config:
```bash
openclaw config set channels.discord.guilds.YOUR_GUILD_ID.channels.NEW_CHANNEL_ID.allow true
```
3. Restart: `openclaw gateway restart`

## Binding an Agent to a Channel

Add a binding in `openclaw.json`:
```json
{
  "bindings": [
    {
      "agentId": "your-agent-id",
      "match": {
        "channel": "discord",
        "peer": {
          "kind": "channel",
          "id": "CHANNEL_ID"
        }
      }
    }
  ]
}
```

## Changing Models

```bash
# Set default model
openclaw config set agents.defaults.model.primary "provider/model-name"

# Set model for a specific agent
openclaw config set agents.list.1.model.primary "provider/model-name"
```

## Memory & Context

Clawdboss uses a three-layer memory architecture:

### The Three Layers

| Layer | Location | Loaded | Purpose |
|-------|----------|--------|---------|
| **L1 (Brain)** | Root workspace files | Every turn (automatic) | Operating system — who the agent is, what's active |
| **L2 (Memory)** | `memory/` directory | Searched semantically | Long-term recall — daily notes + topic breadcrumbs |
| **L3 (Reference)** | `reference/` directory | Opened on demand | Deep context — SOPs, research, playbooks |

**Key rule:** Information flows down, never duplicated across layers. One home per fact.

### L1 Files

- **SOUL.md** — Personality, voice, values (not instructions)
- **AGENTS.md** — Role, rules, lane (not personality)
- **MEMORY.md** — What's active right now (one line per item, present tense)
- **USER.md** — How the user thinks and what they need
- **TOOLS.md** — Machine-specific commands and workarounds
- **IDENTITY.md** — Name, role, quick reference
- **HEARTBEAT.md** — Standing tasks for recurring checks
- **SESSION-STATE.md** — Active working memory (WAL target)

**L1 Budget:** Target 500-1,000 tokens per file, total under 7,000 tokens. Bloated files get skimmed — agents start missing instructions silently.

### L2: Daily Notes + Breadcrumbs

- **Daily notes** (`memory/YYYY-MM-DD.md`): Session history, decisions, completed work. What actually happened.
- **Breadcrumb files** (`memory/[topic].md`): Curated one-liners organized by topic, each pointing to deeper reference docs. Example:

```markdown
# memory/deals.md
- Active deal: 123 Main St, pending inspection → reference/deal-123-main.md
- Compliance check due March 15 → reference/compliance-sop.md
```

Breadcrumbs are the bridge — search finds the breadcrumb, the breadcrumb points to depth. Max 4KB per file.

### L3: Reference

Deep context that agents reach into on demand: SOPs, frameworks, research reports, playbooks. Not searched by `memory_search` by design — you don't want to burn context loading rarely-needed docs.

### WAL Protocol (Write-Ahead Log)

Your agents are pre-configured to use the WAL Protocol. When you tell an agent something important (a correction, a name, a decision), it writes that to `SESSION-STATE.md` before responding. This means the detail survives even if context compacts.

### Working Buffer

When context gets high (~60%), agents start logging every exchange to `memory/working-buffer.md`. After compaction, they read this buffer to recover. You never have to re-explain what you were working on.

### Maintenance Triggers

Two built-in maintenance protocols:

- **`trim`** — Weekly L1 cleanup. Measures all workspace files, moves excess to L2/L3, reports before/after token counts. Nothing gets deleted — everything is archived. Run when MEMORY.md reads like a journal or agents start missing instructions.

- **`recalibrate`** — Drift correction. Forces the agent to re-read all L1 files and compare its recent behavior against them. Reports specific drift examples and corrections. Run weekly or when the agent's personality/behavior feels off.

### Heartbeat Tasks

Add periodic checks to `HEARTBEAT.md`. Keep it lean — each heartbeat burns tokens.

## Queue Tuning

The default config uses `interrupt` mode which is best for Discord (responds to latest message, drops stale ones). If you want to process all messages in order, change to `collect`:

```bash
openclaw config set messages.queue.mode "collect"
```
