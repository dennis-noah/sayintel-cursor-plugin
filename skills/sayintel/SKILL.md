---
name: sayintel
description: Turn a conference speaker page into a researched outbound campaign with SayIntel. Use when starting a campaign from a conference URL, checking run progress, reviewing approved leads and stats, or asking about a conference.
---

# SayIntel

Use the SayIntel MCP tools when the user wants to:

- Start an outbound campaign from a conference or speaker-page URL
- Check how a campaign run is going
- Review approved leads and campaign stats
- Ask about a conference (brief, signals, or fit)

The hosted connector is at `https://sayintel.com/mcp`. Cursor signs the user in with OAuth (dynamic client registration and a consent screen). This connector exposes 100 tools: read and write only. Sending and pushing are never available through this connector.

## Campaign

The main object is a campaign:

```text
Campaign = { id, name, conference_url, status, latest_run, stats }
```

Use only values returned by the tools. Never invent campaign IDs, speakers, emails, stats, or run status.

## When to call which tools

1. **Orient.** Call `whoami` (and `get_credit_balance` when spend or remaining credits matter) before creating work. If `whoami` shows clients, `create_campaign` needs exactly one of `client_id` or `own_company: true`.
2. **Start a campaign from a conference URL.** Call `create_campaign` with the public `https` conference or speaker-page URL. Pass `who` (audience in plain words) and `conference_date` (`YYYY-MM-DD`) when the user provides them. Use `dry_run: true` first if the user only wants a cost preview.
3. **Check run progress.** Use `get_campaign`, and poll `get_run_status` while a run is in progress. Use `campaign_stats` for totals.
4. **Review approved leads.** Use `list_approved_leads` (and `list_campaigns` when the user has not named a campaign).
5. **Ask about a conference.** Use `get_conference_brief`, `list_signals`, and `ask_intel`.

Official references: [docs](https://sayintel.com/docs), [tools](https://sayintel.com/docs/tools), [authentication](https://sayintel.com/docs/authentication), and [API](https://sayintel.com/docs/api).

## Guardrails

- Never invent data. If a tool errors, is empty, or the user is not signed in, say so and stop. Do not fabricate speakers, copy, or results.
- Pushing to sending tools, bulk approve-and-push, inviting teammates and setting integration keys are not available on this connector. `approve_message` can mark a draft approved but never sends; on a campaign with auto-push armed it is refused. Do not claim that email was sent.
- Confirm with the user before calling `approve_message` or any tool that spends credits (for example `create_campaign` without `dry_run`).
- If tools return 401 or say the connection is revoked, tell the user SayIntel is not connected and they should reconnect from Cursor. Then stop; do not retry. A connection disconnected in SayIntel Settings stays revoked until the user consents again.
- Do not ask for API keys or create accounts yourself. The user connects in Cursor through SayIntel's sign-in screen.
