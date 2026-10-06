# Anthropic kit: cf0 Zero

Two submissions in the developer portal at claude.ai/directory/manage, paired once both exist:

1. **MCP connector** for `https://api.cf0.ai/mcp`, slug `cf0-zero`.
2. **Plugin bundle** from the public GitHub repository `cf0-enterprise/cf0-zero` (this repository), plugin path empty (the plugin is at the root).

Credentials never go in this file. They are entered only in the portal's **Test & launch** step.

## Policy 4.B: Claude stays text-only

The Anthropic Software Directory Policy, section 4.B, excludes software that uses AI models to generate audio, and section 4 opens "Unless otherwise expressly permitted by us in writing". Zero's product includes AI voice calls on cf0's own phone lines, though nothing in this plugin generates audio.

On 2026-10-02 Luca emailed `mcp-review@anthropic.com` from luca@cf0.ai to ask whether that is permitted (founder decision L3). Until Anthropic answers in writing, the Claude door stays text-only: the three tools return text and structured data, and no tool starts, places or returns a call or any audio. Mention the open request in the submission notes so the reviewer has the context.

## Connector listing fields

| Field | Value |
|---|---|
| Server name (max 100) | cf0 Zero |
| One-liner (max 200) | Ask your property management firm's AI assistant, Zero, what it is handling, hand it one task about one person on your records, and answer what it is waiting on. |
| Categories | Productivity; Business (check the portal's list) |
| Documentation URL | https://cf0.ai/support#assistants |
| Privacy policy URL | https://cf0.ai/privacy |
| Support contact | support@cf0.ai |
| Icon | `assets/logo.png` (434 by 434 PNG) |
| Slug (permanent) | `cf0-zero` |
| Company | cf0, Corp., https://cf0.ai |
| Primary contact | Luca Tabone, luca@cf0.ai |

**Description (max 2,000):**

> cf0 Zero connects your firm's Zero, the AI assistant in cf0, to Claude. It is for staff at property management, lettings and short-term rental firms that work in cf0.
>
> Ask Zero what it is handling, what is waiting on you, and where a task stands. Asking changes nothing and contacts no one.
>
> Hand Zero one task about one person already on your firm's records, such as arranging access with a tenant for a repair visit, confirming a check-out time with a guest, or telling an owner about booked work. Zero drafts one email from your firm's own Zero mailbox and sends it only after someone at your firm approves it.
>
> Answer what Zero is waiting on: approve or decline an ordinary email after a preview of the person's role, the property, the send time in your firm's time zone and the full message; answer a question Zero asked about your task; or cancel a task.
>
> People on your firm's records appear as reference codes with their role, and contact details and access codes are hidden. A name that is not on your firm's records, such as one typed into the chat, is passed back as written. Zero never uses a phone number or email address typed into the chat, and never takes leasing, leads, cold outreach, marketing, tenant screening, rent or arrears, payments, legal notices or emergencies. Messages to 10 or more people, documents and sensitive content are approved only on cf0.ai.

**Use cases:** checking what Zero is doing and what waits on you; handing Zero maintenance and access, move-out and turnover, owner update and stay operation tasks; approving Zero's ordinary emails; answering Zero's questions; cancelling tasks.

**What users need before they connect:** a cf0 account as a member of a firm cf0 has approved. Giving Zero tasks also needs an admin to set up billing and Zero's mailbox on cf0.ai.

**Reads or writes:** both. `ask_agent` reads; `give_agent_task` and `answer_agent` write.

**Authentication:** OAuth with client ID metadata documents (CIMD). The authorization server is Clerk at `https://clerk.cf0.ai`, named in the protected-resource metadata at `https://api.cf0.ai/.well-known/oauth-protected-resource/mcp`. Scopes: `openid email profile user:org:read offline_access`.

**Data handling:** the API is cf0's own, first party. The connector does not handle personal health data and carries no sponsored content.

## Example prompts (Policy 3.E)

1. "What is waiting on me from Zero today?"
2. "Ask Zero to arrange access with the tenant at Harbour View Flat 3 for the boiler service on Thursday morning."
3. "Show me the email Zero wants to send the guest at Seafront Studio 2." Then: "Approve it."
4. "Zero has a question about the owner update for Elm Court 5. Tell it Friday afternoon works for us."
5. "Cancel the gutter cleaning task at Riverside 7."

The expected result of each is in `openai-review-kit.md` (P1 to P5), and three requests Zero refuses are there as N1 to N3. Every prompt works against the demo firm's seeded data.

## Test account notes (Policy 3.D)

- **Firm:** cf0 Demo Lettings, a standard test account with synthetic sample data (the records and seeded items listed in `openai-review-kit.md`). The Claude reviewer has its own recipient set of cf0-owned inboxes, separate from the ChatGPT reviewer's, so no review run reaches a real person.
- **User:** a Clerk user on cf0 production who is an admin of cf0 Demo Lettings and one of its named approvers, because approving an action needs an admin.
- **Sign-in:** https://cf0.ai/login with email and password. No MFA, no email or SMS code, no magic link.
- **Steps for the reviewer:** in Claude, open Customize, then Connectors, choose Add custom connector and paste `https://api.cf0.ai/mcp` (or add cf0 Zero from the directory once it is listed). Sign in with the test account, choose cf0 Demo Lettings on the consent screen if asked, then try the example prompts.
- **Where credentials live:** only in the Test & launch step. Louis rotates the password after each review.
- **Tools run before submitting:** confirm each of the three tools ran from a Claude conversation against the demo firm, as the Test & launch step asks.

## Connector compliance: the seven acknowledgements

| Acknowledgement | How cf0 Zero meets it |
|---|---|
| Directory guidelines | Follows the Software Directory Terms and Policy; every tool has a title and `readOnlyHint` and `destructiveHint`; read and write tools are separate; tool names are under 64 characters. |
| First-party API usage | The server calls only cf0's own API and data. It is not a pass-through to a property management system or any third-party service. |
| Financial transactions | Zero moves no money and executes no financial transactions. It refuses rent, arrears and payment tasks. No price or checkout appears in any tool. |
| AI media generation | No tool generates images, video or audio. The Claude door stays text-only until Anthropic answers the Policy 4.B request sent on 2026-10-02. |
| Prompt injection | Tool descriptions never tell Claude to call other tools or fetch instructions from elsewhere. Text quoted from tenants, guests or owners is returned as information, and the server's instructions say to treat it as data. |
| Conversation data collection | Tools take only the user's question or task and the ids Zero returned. They never ask for chat history, memory, summaries or uploaded files. |
| Public documentation | Setup and usage are at https://cf0.ai/support#assistants; privacy at https://cf0.ai/privacy; terms at https://cf0.ai/terms. |

## Plugin bundle steps

- **Source:** repository `cf0-enterprise/cf0-zero`, plugin path empty, branch `main`.
- **Validate:** run `claude plugin validate .` and `scripts/check-no-price.sh` locally first, then the portal's Validate. Fix anything marked Blocking.
- **Data handling answers:**
  - Reads or stores personal data: yes. Zero reads the firm's records in cf0 and returns people as reference codes with their role. Tasks the user gives are stored on the firm's board in cf0.
  - Sends data to services other than its declared connector: no. The only connection is `https://api.cf0.ai/mcp`.
  - Retention: as the cf0 privacy policy states for the firm's workspace.
  - Intended for people under 18: no. It is for staff of property management firms.
- **Compliance:** confirm the contact email (luca@cf0.ai) and select all four acknowledgements.
- **Updates:** keep GitHub push webhook; Louis, as repository admin, sets it up from the submitted page.
- **Pairing:** once both listings exist, pair the plugin with the `cf0-zero` connector in the portal.
