# LLM gateway pattern

If every team calls model providers directly, you end up with keys in a dozen places and no idea what data leaves the company. I prefer one path for all model traffic, through a gateway the security team can see and control. I've run this with Kong in front of commercial model APIs; LiteLLM's proxy does a similar job.

## What the gateway does

| Control | Why |
| --- | --- |
| Authentication per application | Know who is calling; revoke one app without breaking the rest |
| Provider keys stay in the gateway | Teams never hold the real API keys; rotation happens in one place |
| Model allowlist | Only approved models and regions, per use case |
| Rate limits and token budgets | Stops runaway loops and surprise invoices |
| Request and response logging | Metadata always; full prompts only where policy allows |
| Data checks | Optional PII or secret detection on the way out |
| Egress control | Direct calls to provider domains blocked from the network |

## Things that bite later

- **Logging prompts is logging personal data.** Agree retention and access with data protection before you turn on full logging.
- **Streaming responses** need support in the gateway's plugins, or limits and logging get skipped.
- **Budgets per team** are easier to agree up front than to introduce after the first big bill.
- **The gateway is now critical.** Run it highly available and alert on errors, latency and spend.

## Minimum setup

1. One gateway route per provider, provider keys stored in a vault.
2. A consumer per application, with its own key and rate limit.
3. Network policy that blocks direct egress to model providers.
4. A dashboard with calls, tokens and errors per consumer.
