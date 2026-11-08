# AI and LLM security

These are my working notes on securing LLM applications and AI agents. They come from two places: building agents on an internal GenAI platform, and reviewing AI systems as a security engineer before that.

Same rules as the rest of this site. The notes are for me, they stay short, and I update them when something changes.

| Page | What's in it |
| --- | --- |
| [OWASP Top 10 for LLM applications](owasp_llm_top_10.md) | The 2025 list, with how I test each item and what actually fixes it |
| [Prompt injection](prompt_injection.md) | Where untrusted text gets in, test cases I start with, and defences that hold up |
| [Agents and MCP](agents_and_mcp.md) | Threat model for tool-using agents, MCP and A2A specifics, a review checklist |
| [LLM gateway pattern](llm_gateway.md) | How I'd route all model traffic through one controlled path |
| [AI governance for security people](ai_governance.md) | EU AI Act, NIST AI RMF and ISO/IEC 42001, mapped to security work |

!!! warning "Authorised testing only"
    Only test systems you own or have written permission to test.
