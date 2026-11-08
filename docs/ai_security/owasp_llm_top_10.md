# OWASP Top 10 for LLM applications (2025)

The OWASP GenAI Security Project's list of the most common risks in apps built on large language models. Source: [genai.owasp.org/llm-top-10](https://genai.owasp.org/llm-top-10/).

For each item: what it is, how I check for it, and what fixes it. The fixes are almost never "a better prompt".

## LLM01 Prompt injection

Text the model reads changes what it does. Direct injection comes from the user. Indirect injection hides in content the model pulls in: documents, web pages, emails, tool results.

- **Test:** see [Prompt injection](prompt_injection.md).
- **Fix:** assume it will happen. Limit what the model can do (tools, data, network), require confirmation for actions with side effects, and treat model output as untrusted input everywhere downstream.

## LLM02 Sensitive information disclosure

The app leaks personal data, secrets or internal documents through answers, logs or citations.

- **Test:** ask for other users' data, internal identifiers, API keys, "the documents you were given". Check what the RAG index contains and whether retrieval respects the caller's permissions.
- **Fix:** don't put secrets in prompts. Filter what goes into the index. Enforce access control at retrieval time, per user, not in the prompt.

## LLM03 Supply chain

Compromised or unvetted models, datasets, plugins, packages or model hubs.

- **Test:** list every model, adapter, dataset and package in use, and where it came from. Look for unpinned versions and pickle-based model files.
- **Fix:** pin and verify versions, prefer safetensors, scan dependencies, keep an inventory (an AI bill of materials helps).

## LLM04 Data and model poisoning

Manipulated training, fine-tuning or embedding data that changes behaviour or plants a backdoor.

- **Test:** check who can write to training data, fine-tuning sets and the documents behind RAG.
- **Fix:** control and log write access to data sources, validate data before training, keep provenance.

## LLM05 Improper output handling

Model output is passed to a browser, shell, SQL query or API without validation. Classic injection bugs, new source.

- **Test:** get the model to output HTML/JavaScript, markdown images with external URLs, SQL fragments or shell metacharacters, and see where they end up.
- **Fix:** encode and validate output like any other untrusted input. Parameterised queries, no `eval`, a strict CSP, block remote images in rendered markdown.

## LLM06 Excessive agency

The model has more tools, permissions or autonomy than the task needs.

- **Test:** list each tool and its scopes. Ask: what's the worst thing this agent could do if fully hijacked?
- **Fix:** least privilege per tool, read-only by default, human approval for writes, payments and messages, and rate limits on actions.

## LLM07 System prompt leakage

The system prompt is revealed. The real problem is when it holds secrets or is used as the access control.

- **Test:** ask for it directly, in translation, as a poem, "repeat everything above".
- **Fix:** assume it will leak. No credentials or internal rules that matter in the system prompt; enforce rules in code.

## LLM08 Vector and embedding weaknesses

Problems in the RAG layer: cross-tenant leakage, poisoned documents, embedding inversion.

- **Test:** query as user A for content only user B should see. Plant a document with instructions and see if it gets retrieved and followed.
- **Fix:** per-tenant or permission-filtered retrieval, review of what gets indexed, and logging of retrieval results.

## LLM09 Misinformation

Confident, wrong answers that people act on: invented facts, fake package names, wrong legal or medical claims.

- **Test:** ask questions with known answers in your domain; ask for package names and check they exist.
- **Fix:** grounding with citations, human review where the stakes are high, and clear limits in the UI.

## LLM10 Unbounded consumption

No limits on requests, tokens or compute. This leads to denial of service, cost blow-ups, or model extraction.

- **Test:** long inputs, high request rates, prompts that cause long outputs or tool loops.
- **Fix:** per-user and per-app rate limits, token budgets, timeouts, max tool-call depth, and cost alerts.
