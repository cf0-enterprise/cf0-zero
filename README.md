# cf0 Zero

cf0 Zero lets staff at a property management firm talk to Zero, their firm's AI assistant in [cf0](https://cf0.ai), from their AI assistant. Zero's work stays on the firm's task board in cf0, with the same approvals and the same record of what it sent and when.

## What it does

- **Ask Zero** (`ask_agent`, read-only): what Zero is handling, what is waiting on you, and where a task, person or property stands. It changes nothing and contacts no one.
- **Give Zero a task** (`give_agent_task`): hand Zero one task about one person already on your firm's records, such as arranging access with a tenant for a repair visit, confirming a check-out time with a guest or telling an owner about booked work. Zero drafts one email from your firm's own Zero mailbox and sends it only after someone at your firm approves it.
- **Answer Zero** (`answer_agent`): approve or decline an ordinary email after a preview, answer a question Zero asked about your task, or cancel a task.

People appear as reference codes such as `p_k7mq2xrb4a` with their role, never with names or contact details. Zero never takes leads, cold outreach, marketing, tenant screening, rent or arrears, payments or emergencies. Messages to 10 or more people, documents and sensitive content can only be approved on cf0.ai.

## What is in this package

| Path | Purpose |
|---|---|
| `plugin.json`, `mcp.json` | Agent Plugins manifest and MCP server for ChatGPT and Codex |
| `.claude-plugin/plugin.json`, `.mcp.json` | Plugin manifest and MCP server for Claude |
| `skills/working-with-zero/SKILL.md` | Guidance for using the three tools |
| `assets/` | cf0 logos, light and dark |
| `listing/` | Directory submission material; not loaded by the plugin |
| `scripts/` | Repository checks; not loaded by the plugin |

The plugin runs no local code, hooks or commands. Its one connection is the remote MCP server at `https://api.cf0.ai/mcp`, operated by cf0. When you connect, your assistant asks you to sign in with your cf0 account through OAuth. Each request then carries your question or task, and Zero answers from your firm's workspace in cf0. Nothing is sent anywhere else.

## Before you connect

You need a cf0 account as a member of a firm that cf0 has approved. Until then, Zero answers every request by saying cf0 is still reviewing your firm. Giving Zero tasks also needs an admin to set up billing and Zero's mailbox on cf0.ai.

Setup steps for each assistant are at [cf0.ai/support](https://cf0.ai/support#assistants).

## Privacy and terms

- Privacy policy: [cf0.ai/privacy](https://cf0.ai/privacy)
- Terms: [cf0.ai/terms](https://cf0.ai/terms)
- Support: [support@cf0.ai](mailto:support@cf0.ai)

## License

MIT. See [LICENSE](LICENSE).
