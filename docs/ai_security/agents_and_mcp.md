# Agents and MCP

An agent is an LLM that can call tools in a loop. The security question changes from "what can it say?" to "what can it do, and with whose permissions?"

## Threat model in one table

| Part | What can go wrong |
| --- | --- |
| Model | Follows injected instructions, invents tool arguments |
| Tools | Too many, too broad, no confirmation for writes |
| Credentials | Shared service account, long-lived tokens, tokens passed through to other APIs |
| Context sources | RAG, web, email and tool output carry injected instructions |
| Memory | Poisoned memory persists across sessions and users |
| Other agents | A remote agent's output is treated as trusted |

## Model Context Protocol (MCP)

MCP standardises how apps expose tools and data to models. It makes integration easy, which also makes it easy to connect too much.

- **Tool descriptions are prompts.** A malicious or compromised server can hide instructions in a tool's description or results (tool poisoning). Review descriptions like code.
- **Definitions can change after approval.** Pin server versions and re-review when a tool's definition changes.
- **Local servers run as you.** A local MCP server has your user's file system and network access. Run untrusted ones in a container or sandbox.
- **Don't pass tokens through.** A server should use credentials issued for it, not forward the client's token to downstream APIs. Use the MCP authorisation spec (OAuth) for remote servers.
- **Scope per tool.** Read-only tools and write tools should not share one broad credential.
- **Allowlist servers** in managed environments, and log every tool call with its arguments.

## Agent-to-agent (A2A)

When agents call other agents, treat each remote agent like an external API:

- authenticate both sides and check the remote agent's identity before sending data;
- validate and sanitise what comes back, since it can carry injected instructions;
- don't let a remote agent trigger side effects without your own policy checks.

## Review checklist

- [ ] Every tool listed with its scope, owner and whether it writes
- [ ] Writes, sends and deletes need human confirmation
- [ ] The agent acts with the user's permissions, not a shared admin identity
- [ ] Short-lived, narrowly scoped tokens; nothing in prompts or tool descriptions
- [ ] Untrusted content is marked as data; outputs are validated before rendering or execution
- [ ] Limits on loop depth, tool calls per task, tokens and cost
- [ ] MCP servers pinned, reviewed and allowlisted; local ones sandboxed
- [ ] Prompts, retrieved context and tool calls logged and reviewable
- [ ] A kill switch to disable a tool or the whole agent quickly
