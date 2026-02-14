# Claude Agent SDK Skills

Expert guidance for building production AI agents with the [Claude Agent SDK](https://platform.claude.com/docs/en/agent-sdk/overview) — split into **Python** and **TypeScript** skills so you only load what you need.

## Skills Included

| Skill | Language | Covers |
|-------|----------|--------|
| `claude-agent-sdk-python` | Python | `query()`, `ClaudeSDKClient`, `ClaudeAgentOptions`, `@tool`, subagents, hooks, MCP, sessions |
| `claude-agent-sdk-typescript` | TypeScript | `query()`, `ClaudeSDKClient`, `allowedTools`, `createSdkMcpServer`, subagents, hooks, MCP, sessions |

## Installation

### Option 1 — `npx skills add` (works with Claude Code, Cursor, OpenCode, Kiro)

```bash
# Install both skills
npx skills add waltersumbon/claude-agent-sdk-skills

# Install only the Python skill
npx skills add waltersumbon/claude-agent-sdk-skills --skill claude-agent-sdk-python

# Install only the TypeScript skill
npx skills add waltersumbon/claude-agent-sdk-skills --skill claude-agent-sdk-typescript
```

### Option 2 — Claude Code Plugin Marketplace

```bash
# Install both
/plugin marketplace add waltersumbon/claude-agent-sdk-skills

# List available skills
/plugin marketplace add waltersumbon/claude-agent-sdk-skills --list
```

### Option 3 — Manual (any Claude tool)

Copy the desired skill folder into your skills directory:

```bash
# Python skill — project-level
cp -r skills/claude-agent-sdk-python .claude/skills/

# TypeScript skill — user-level (available in all projects)
cp -r skills/claude-agent-sdk-typescript ~/.claude/skills/
```

### Option 4 — Upload to claude.ai

Download the individual skill ZIP from the [Releases](../../releases) page and upload it via **Settings → Skills** in claude.ai.

## Repository Structure

```
claude-agent-sdk-skills/
├── .claude-plugin/
│   └── marketplace.json              # Registers both skills for marketplace
├── skills/
│   ├── claude-agent-sdk-python/      # Python SDK skill
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── troubleshooting.md
│   └── claude-agent-sdk-typescript/  # TypeScript SDK skill
│       ├── SKILL.md
│       └── references/
│           └── troubleshooting.md
├── LICENSE
└── README.md
```

## What's Covered

Each skill provides complete, production-ready guidance for its language:

- **Two interaction modes**: `query()` (stateless) vs `ClaudeSDKClient` (stateful)
- **Full options reference**: Every `ClaudeAgentOptions` field with types and descriptions
- **Custom tools**: SDK MCP server (`@tool` / `createSdkMcpServer`) for in-process tools
- **Subagents**: `AgentDefinition` with factory patterns
- **Hooks**: `PreToolUse` / `PostToolUse` lifecycle callbacks for safety and auditing
- **MCP integration**: External stdio servers + mixing with SDK servers
- **Skills in SDK**: `setting_sources` / `settingSources` configuration
- **Sessions**: Capturing and resuming `session_id`
- **Authentication**: API key, Bedrock, Vertex AI, Azure
- **Common patterns**: Batch processing, structured output, error handling
- **Migration guide**: v0.0.x → v0.1.0 breaking changes
- **Troubleshooting**: Top pitfalls with WRONG/CORRECT code pairs

## License

[Apache-2.0](LICENSE)
