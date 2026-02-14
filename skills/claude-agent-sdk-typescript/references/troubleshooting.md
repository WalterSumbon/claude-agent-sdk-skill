# Troubleshooting — TypeScript SDK

## Installation Issues

### CLINotFoundError

The SDK requires the Claude Code CLI bundled within the package. If you see this error:

```
CLINotFoundError: Claude Code CLI not found
```

**Fix**: The CLI is auto-bundled with `npm install @anthropic-ai/claude-agent-sdk`. If using a custom path:

```typescript
const options = { cliPath: "/path/to/claude" };
```

Or install CLI separately: `curl -fsSL https://claude.ai/install.sh | bash`

### Node.js Required

The Claude Code CLI requires Node.js >= 18. Ensure it's installed:

```bash
node --version  # Should be >= 18
```

## Configuration Mistakes

### Skills Not Loading

**Symptom**: Claude doesn't discover or invoke Skills even though `"Skill"` is in `allowedTools`.

**Cause**: Missing `settingSources` configuration.

```typescript
// WRONG — Skills won't load
const options = { allowedTools: ["Skill"] };

// CORRECT — Skills loaded from filesystem
const options = {
  settingSources: ["user", "project"],
  allowedTools: ["Skill"],
};
```

### System Prompt Surprise (v0.1.0+)

**Symptom**: Agent behaves differently after upgrading from v0.0.x.

**Cause**: v0.1.0 no longer loads Claude Code's system prompt by default.

```typescript
// Restore old behavior explicitly
const options = {
  systemPrompt: { type: "preset", preset: "claude_code" },
};
```

### Subagents Not Invoked

**Symptom**: Claude ignores defined subagents.

**Cause**: Missing `"Task"` in `allowedTools`.

```typescript
// WRONG — Task tool not enabled
const options = {
  allowedTools: ["Read", "Grep"],
  agents: { reviewer: { /* ... */ } },
};

// CORRECT
const options = {
  allowedTools: ["Read", "Grep", "Task"], // Task required
  agents: { reviewer: { /* ... */ } },
};
```

### MCP Tool Name Mismatch

**Symptom**: Claude can't find your MCP tools.

**Cause**: Tool names must follow `mcp__<server-name>__<tool-name>` convention.

```typescript
// Server named "biz" with tool "search_orders"
// Correct tool name: "mcp__biz__search_orders"

// Use wildcard to allow all tools from a server:
allowedTools: ["mcp__biz"]
```

## Runtime Issues

### Agent Runs Forever

**Fix**: Set `maxTurns` to limit iterations:

```typescript
const options = { maxTurns: 50 };
```

### Permission Denied on File Edits

**Fix**: Set appropriate permission mode:

```typescript
// For automation (auto-approve edits)
const options = { permissionMode: "acceptEdits" };

// For full automation (bypass all permissions — use with caution)
const options = { permissionMode: "bypassPermissions" };
```

### Context Window Exhaustion

The SDK has built-in compaction (automatic summarization when context limit approaches).
If you still hit limits in very long sessions, consider:

1. Breaking tasks into smaller `query()` calls
2. Using subagents to isolate context per task
3. Using `ClaudeSDKClient` and starting new conversations when appropriate

## Migration Checklist (v0.0.x → v0.1.0)

- [ ] Update import: `@anthropic-ai/claude-code` → `@anthropic-ai/claude-agent-sdk`
- [ ] Update type name: `ClaudeCodeOptions` → `ClaudeAgentOptions`
- [ ] Review system prompt behavior — add explicit `preset: "claude_code"` if needed
- [ ] Add `settingSources` if you rely on Skills or CLAUDE.md
- [ ] Test all hooks and custom tools for compatibility
