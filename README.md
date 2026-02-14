# Claude Agent SDK Skills

Expert guidance for building production AI agents with the **Claude Agent SDK** (formerly Claude Code SDK).

This skill teaches Claude how to write correct SDK code — covering `query()`, `ClaudeSDKClient`, `ClaudeAgentOptions`, custom tools, subagents, hooks, MCP integration, sessions, permissions, and production best practices.

> **All code examples are sourced from official Anthropic documentation and verified against the latest SDK version.**

## Skills Included

| Skill | Description |
|-------|-------------|
| **claude-agent-sdk-guide** | Complete reference for building agents with the Claude Agent SDK. Python & TypeScript. |

## Installation

### Option 1: `npx skills add` (Recommended — works with Claude Code, Cursor, OpenCode, etc.)

```bash
# Install the skill
npx skills add waltersumbon/claude-agent-sdk-skills

# Install to a specific agent
npx skills add waltersumbon/claude-agent-sdk-skills -a claude-code

# List available skills before installing
npx skills add waltersumbon/claude-agent-sdk-skills --list
```

### Option 2: Claude Code Plugin Marketplace

```bash
# Step 1: Add the marketplace
/plugin marketplace add waltersumbon/claude-agent-sdk-skills

# Step 2: Install the plugin
/plugin install claude-agent-sdk-guide@claude-agent-sdk-skills
```

### Option 3: Manual Installation

```bash
# Clone to your personal skills directory (available across all projects)
git clone https://github.com/waltersumbon/claude-agent-sdk-skills.git
cp -r claude-agent-sdk-skills/skills/claude-agent-sdk-guide ~/.claude/skills/

# Or install at project level (shared with team via git)
mkdir -p .claude/skills
cp -r claude-agent-sdk-skills/skills/claude-agent-sdk-guide .claude/skills/
```

### Option 4: Upload to Claude.ai

1. Download the ZIP from [Releases](../../releases)
2. Go to [claude.ai/settings/capabilities](https://claude.ai/settings/capabilities)
3. Find the "Skills" section → "Upload skill"
4. Select the downloaded ZIP

## Repository Structure

```
claude-agent-sdk-skills/
├── .claude-plugin/
│   └── marketplace.json              # Claude Code marketplace catalog
├── skills/
│   └── claude-agent-sdk-guide/       # The skill directory
│       ├── SKILL.md                  # Main instructions (412 lines)
│       └── references/
│           ├── typescript-sdk.md     # TypeScript-specific patterns
│           └── troubleshooting.md    # Common pitfalls & migration guide
├── LICENSE                           # Apache-2.0
└── README.md
```

## What's Covered

- **Two interaction modes**: `query()` (stateless) vs `ClaudeSDKClient` (multi-turn)
- **ClaudeAgentOptions**: Complete configuration reference
- **Custom tools**: In-process SDK MCP servers with `@tool` decorator
- **Subagents**: `AgentDefinition`, factory patterns, model selection per task
- **Hooks**: `PreToolUse` / `PostToolUse` safety guards and audit logging
- **MCP integration**: External servers + SDK servers, mixed configurations
- **Skills in SDK**: `setting_sources` configuration, common mistakes
- **Sessions**: Capturing and resuming `session_id`
- **System prompts**: Three approaches (custom / preset / minimal)
- **Auth**: API key, Bedrock, Vertex AI, Azure AI Foundry
- **Patterns**: Batch processing, structured output, error handling
- **Migration**: v0.0.x → v0.1.0 breaking changes checklist

## Official Resources

| Resource | URL |
|----------|-----|
| SDK Overview | https://platform.claude.com/docs/en/agent-sdk/overview |
| Python Reference | https://platform.claude.com/docs/en/agent-sdk/python |
| TypeScript Reference | https://platform.claude.com/docs/en/agent-sdk/typescript |
| Quickstart | https://platform.claude.com/docs/en/agent-sdk/quickstart |
| Demo Agents | https://github.com/anthropics/claude-agent-sdk-demos |
| Engineering Blog | https://www.anthropic.com/engineering/building-agents-with-the-claude-agent-sdk |

## Contributing

Contributions welcome! Please:

1. Fork the repository
2. Make changes to the skill files
3. Test by installing locally: `cp -r skills/claude-agent-sdk-guide ~/.claude/skills/`
4. Verify the skill triggers correctly in Claude Code
5. Submit a PR with a clear description

## License

Apache-2.0 — see [LICENSE](LICENSE).
