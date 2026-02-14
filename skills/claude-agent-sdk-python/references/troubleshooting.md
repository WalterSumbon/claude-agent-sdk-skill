# Troubleshooting — Python SDK

## Installation Issues

### CLINotFoundError

The SDK requires the Claude Code CLI bundled within the package. If you see this error:

```
CLINotFoundError: Claude Code CLI not found
```

**Fix**: The CLI is auto-bundled with `pip install claude-agent-sdk`. If using a custom path:

```python
options = ClaudeAgentOptions(cli_path="/path/to/claude")
```

Or install CLI separately: `curl -fsSL https://claude.ai/install.sh | bash`

### Node.js Required

The Claude Code CLI requires Node.js >= 18. Ensure it's installed:

```bash
node --version  # Should be >= 18
```

## Configuration Mistakes

### Skills Not Loading

**Symptom**: Claude doesn't discover or invoke Skills even though `"Skill"` is in `allowed_tools`.

**Cause**: Missing `setting_sources` configuration.

```python
# WRONG — Skills won't load
options = ClaudeAgentOptions(allowed_tools=["Skill"])

# CORRECT — Skills loaded from filesystem
options = ClaudeAgentOptions(
    setting_sources=["user", "project"],
    allowed_tools=["Skill"],
)
```

### System Prompt Surprise (v0.1.0+)

**Symptom**: Agent behaves differently after upgrading from v0.0.x.

**Cause**: v0.1.0 no longer loads Claude Code's system prompt by default.

```python
# Restore old behavior explicitly
options = ClaudeAgentOptions(
    system_prompt={"type": "preset", "preset": "claude_code"}
)
```

### Subagents Not Invoked

**Symptom**: Claude ignores defined subagents.

**Cause**: Missing `"Task"` in `allowed_tools`.

```python
# WRONG — Task tool not enabled
options = ClaudeAgentOptions(
    allowed_tools=["Read", "Grep"],
    agents={"reviewer": AgentDefinition(...)},
)

# CORRECT
options = ClaudeAgentOptions(
    allowed_tools=["Read", "Grep", "Task"],  # Task required
    agents={"reviewer": AgentDefinition(...)},
)
```

### MCP Tool Name Mismatch

**Symptom**: Claude can't find your MCP tools.

**Cause**: Tool names must follow `mcp__<server-name>__<tool-name>` convention.

```python
# Server named "biz" with tool "search_orders"
# Correct tool name: "mcp__biz__search_orders"

# Use wildcard to allow all tools from a server:
allowed_tools=["mcp__biz"]
```

## Runtime Issues

### Agent Runs Forever

**Fix**: Set `max_turns` to limit iterations:

```python
options = ClaudeAgentOptions(max_turns=50)
```

### Permission Denied on File Edits

**Fix**: Set appropriate permission mode:

```python
# For automation (auto-approve edits)
options = ClaudeAgentOptions(permission_mode="acceptEdits")

# For full automation (bypass all permissions — use with caution)
options = ClaudeAgentOptions(permission_mode="bypassPermissions")
```

### Context Window Exhaustion

The SDK has built-in compaction (automatic summarization when context limit approaches).
If you still hit limits in very long sessions, consider:

1. Breaking tasks into smaller `query()` calls
2. Using subagents to isolate context per task
3. Using `ClaudeSDKClient` and starting new conversations when appropriate

## Migration Checklist (v0.0.x → v0.1.0)

- [ ] Replace `ClaudeCodeOptions` with `ClaudeAgentOptions`
- [ ] Review system prompt behavior — add explicit `preset: "claude_code"` if needed
- [ ] Add `setting_sources` if you rely on Skills or CLAUDE.md
- [ ] Test all hooks and custom tools for compatibility
