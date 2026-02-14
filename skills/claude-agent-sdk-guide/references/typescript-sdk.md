# TypeScript SDK Reference

TypeScript-specific patterns for the Claude Agent SDK.

## Installation

```bash
npm install @anthropic-ai/claude-agent-sdk
```

## query() — One-Shot

```typescript
import { query } from "@anthropic-ai/claude-agent-sdk";

for await (const message of query({
  prompt: "Find and fix the bug in auth.ts",
  options: {
    allowedTools: ["Read", "Edit", "Bash"],
    permissionMode: "acceptEdits",
  },
})) {
  if (message.type === "assistant") {
    for (const block of message.message.content) {
      if ("text" in block) console.log(block.text);
      else if ("name" in block) console.log(`Tool: ${block.name}`);
    }
  }
  if (message.type === "result") {
    console.log(`Done: ${message.subtype}`);
  }
}
```

## ClaudeAgentOptions (TypeScript)

Key differences from Python:
- `allowedTools` (camelCase, not `allowed_tools`)
- `disallowedTools` (camelCase)
- `permissionMode` (camelCase)
- `systemPrompt` (camelCase)
- `maxTurns` (camelCase)
- `settingSources` (camelCase, not `setting_sources`)
- `mcpServers` (camelCase)

## Custom Tools

```typescript
import { query, tool, createSdkMcpServer } from "@anthropic-ai/claude-agent-sdk";

const searchOrders = tool(
  "search_orders",
  "Search customer orders",
  { customer_id: "string", status: "string" },
  async (args) => ({
    content: [{ type: "text", text: JSON.stringify(await db.query(args.customer_id)) }],
  })
);

const server = createSdkMcpServer({
  name: "business",
  tools: [searchOrders],
});

for await (const msg of query({
  prompt: "Find recent orders for customer C-123",
  options: {
    mcpServers: { biz: server },
    allowedTools: ["mcp__biz__search_orders"],
  },
})) {
  console.log(msg);
}
```

## Subagents

```typescript
import { query } from "@anthropic-ai/claude-agent-sdk";

for await (const message of query({
  prompt: "Review auth module for security issues",
  options: {
    allowedTools: ["Read", "Glob", "Grep", "Task"],
    agents: {
      "code-reviewer": {
        description: "Expert code reviewer for quality and security.",
        prompt: "Analyze code quality and suggest improvements.",
        tools: ["Read", "Glob", "Grep"],
        model: "opus",
      },
    },
  },
})) {
  if ("result" in message) console.log(message.result);
}
```

## Hooks

```typescript
import { query, HookCallback } from "@anthropic-ai/claude-agent-sdk";
import { appendFileSync } from "fs";

const auditLog: HookCallback = async (input) => {
  const toolName = (input as any).tool_name ?? "unknown";
  appendFileSync("audit.log", `${new Date().toISOString()}: ${toolName}\n`);
  return {};
};

for await (const msg of query({
  prompt: "Refactor utils.ts",
  options: {
    permissionMode: "acceptEdits",
    hooks: {
      PostToolUse: [{ matcher: "Edit|Write", hooks: [auditLog] }],
    },
  },
})) {
  if ("result" in msg) console.log(msg.result);
}
```

## Skills in TypeScript SDK

```typescript
for await (const msg of query({
  prompt: "Help me process this PDF",
  options: {
    cwd: "/path/to/project",
    settingSources: ["user", "project"],  // Required for Skills
    allowedTools: ["Skill", "Read", "Write", "Bash"],
  },
})) {
  console.log(msg);
}
```

## Session Management

```typescript
import { query } from "@anthropic-ai/claude-agent-sdk";

let sessionId: string | undefined;

for await (const msg of query({
  prompt: "Review the codebase",
  options: { allowedTools: ["Read", "Glob", "Grep"] },
})) {
  if (msg.type === "system" && "session_id" in msg) {
    sessionId = msg.session_id;
  }
}

// Resume
for await (const msg of query({
  prompt: "Fix the first issue you found",
  options: {
    sessionId,
    allowedTools: ["Read", "Edit"],
    permissionMode: "acceptEdits",
  },
})) {
  console.log(msg);
}
```
