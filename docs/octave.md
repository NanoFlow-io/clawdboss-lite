# OCTAVE Protocol — Structured AI Communication

## What Is OCTAVE?

OCTAVE (Olympian Common Text And Vocabulary Engine) is a structured document format for LLM communication. Think of it as a "semantic zip file" — it compresses complex information into token-efficient, machine-readable artifacts that survive multi-agent handoffs, compression, and auditing.

**Key benefits:**
- **3-20x token reduction** on large documents
- **Deterministic canonicalization** — same input, same output, always
- **Schema validation** with repair suggestions
- **Multi-agent safe** — documents carry their schema and transformation logs
- **Zero training required** — LLMs understand it out of the box

## Why Include It in Clawdboss?

For multi-agent setups (Team/Squad tiers), OCTAVE solves real problems:

1. **Agent handoffs** — When Cipher delegates to Trace, the brief stays structured and lossless
2. **Audit trails** — Decision logs and status updates in canonical form
3. **Token savings** — Compress large research docs, meeting notes, or reports before loading into context
4. **Knowledge artifacts** — Structured, queryable documents instead of messy prose

## How It's Installed

OCTAVE is installed as an MCP server accessible via `mcporter`:

```bash
# The setup wizard handles this, but manually:
uv venv ~/.octave-venv
~/.octave-venv/bin/pip install octave-mcp
mcporter config add octave --command "$HOME/.octave-venv/bin/octave-mcp-server" --transport stdio
```

## Available Tools

| Tool | Purpose |
|------|---------|
| `octave_validate` | Schema check + repair suggestions for OCTAVE content |
| `octave_write` | Write OCTAVE files through the validation pipeline |
| `octave_eject` | Project to different views (canonical, executive, developer, markdown) |
| `octave_compile_grammar` | Compile schema to GBNF grammar for constrained generation |

## Quick Example

### Write a project status document

```bash
mcporter call octave.octave_write target_path="./status.oct.md" --args '{
  "content": "===PROJECT_STATUS===\nMETA:\n  TYPE::PROJECT_BRIEF\n  AUTHOR::Cipher\n§1::STATUS\n  PHASE::\"Alpha[hardened]\"\n  AUDIT::\"Complete[12_findings→all_fixed]\"\n§2::NEXT\n  JOURNEY::ODYSSEAN\n  IMMEDIATE::[deploy_skills,test_integration]\n===END===",
  "lenient": true
}'
```

### Validate an existing document

```bash
mcporter call octave.octave_validate file_path="./status.oct.md" schema="META" profile="LENIENT"
```

### Export as markdown

```bash
mcporter call octave.octave_eject --args '{
  "content": "<octave content>",
  "schema": "META",
  "mode": "executive",
  "format": "markdown"
}'
```

## OCTAVE Syntax Cheat Sheet

```
===DOCUMENT_NAME===           # Document boundary
META:                         # Required metadata block
  TYPE::value                 # Key-value assignment (:: operator)
  VERSION::"1.0"              # Quoted strings for values with dots/special chars

§1::SECTION_NAME              # Numbered sections
  KEY::value                  # Simple assignment
  LIST::[item1,item2,item3]   # Lists
  A→B                         # Flow / causality
  A⊕B                         # Synthesis
  A⇌B                         # Tension / trade-off

# Mythological compression (semantic zip)
JOURNEY::ODYSSEAN             # Long, difficult, full of obstacles
CHALLENGE::SISYPHEAN          # Repetitive, frustrating, cyclical
STRATEGY::ATHENA              # Clever solution balancing constraints
SECURITY::ATHENA              # Precision, strategic protection
===END===                     # Document end boundary
```

## Further Reading

- **GitHub:** <https://github.com/elevanaltd/octave-mcp>
- **PyPI:** <https://pypi.org/project/octave-mcp/>
- **Origin:** [Reddit r/ClaudeAI post](https://www.reddit.com/r/ClaudeAI/comments/1lon0g8/)

## Requirements

- Python 3.12+
- `uv` (recommended) or `pip` with venv support
- `mcporter` (included with OpenClaw)
