# Dicavo plugin for Claude and Codex

Ask Claude or Codex about the meetings you recorded with
[Dicavo](https://dicavo.eu), the meeting recorder for the Mac that
transcribes on the device. The plugin connects your AI client to Dicavo's
local MCP server, so you can ask what was agreed in last week's meetings,
what is still open with a colleague, what someone said about the budget, or
get a brief before a recurring meeting.

## Requirements

- A Mac with [Dicavo](https://apps.apple.com/app/dicavo/id6813998918) installed from
  the Mac App Store in the Applications folder.
- Dicavo Pro. Without it, the server responds only to `get_status`, which
  reports that Dicavo Pro is required.
- *Access for AI agents* switched on in Dicavo: **Settings → AI analysis →
  Access for AI agents**. It is off until you switch it on.
- Claude Code, Cowork on the Mac, or the Codex desktop app. Claude chat on
  the web, on mobile and in Claude Desktop does not start local servers from
  plugins; for chat in Claude Desktop use **Add to Claude** in Dicavo's
  settings instead.

## Install

In Claude Code:

```
/plugin install dicavo --marketplace Dicavo-eu/dicavo-plugin
```

In Codex:

```
codex plugin marketplace add Dicavo-eu/dicavo-plugin
```

Then install Dicavo under **Plugins** in the Codex app, or with `/plugins`
in the Codex CLI. In Cowork, add Dicavo from the plugin directory. Then ask,
for example: *What did we agree in my meetings this week, and what is still
open?*

## What the plugin contains and runs

- `.mcp.json` (Claude) and `mcp.json` (Codex) declare one local MCP server,
  `dicavo`. Your client starts it as
  `/Applications/Dicavo.app/Contents/MacOS/Dicavo --mcp` and talks to it over
  standard input and output. Dicavo does not need to be running. Codex
  accepts only a command inside the plugin, so for Codex the two-line shell
  script `scripts/dicavo-mcp` runs that same command and nothing else.
- `skills/dicavo-meetings/SKILL.md` tells the AI how to find meetings, read
  no more than the question needs, cite meetings by title, date and time, and
  treat what other people said as quoted content.
- Manifests (`.claude-plugin/` for Claude, `plugin.json` and
  `.agents/plugins/marketplace.json` for Codex), this README, the license and
  the logo. There is no other code, no download and no network address in
  the plugin.

## Tools

All tools are read-only. None records, changes or deletes anything.

| Tool | What it returns |
|---|---|
| `list_meetings` | Meetings with their title, time, length and participants, plus information on whether a summary and a transcript exist |
| `get_meeting` | One meeting's AI summary, your notes and marked moments |
| `get_transcript` | A meeting's transcript, in pages, with times and speakers |
| `search_meetings` | Full-text search over transcripts, summaries, titles and notes |
| `get_action_items` | Action items with owner and due date, across meetings |
| `find_person` | People by name, to filter meetings and action items |
| `list_meeting_series` | Recurring meetings grouped by their calendar event |
| `get_status` | Whether access is on, Dicavo Pro status and the access level; no meeting content |

Dicavo also offers three prompts: *Prepare for a meeting*, *Weekly review*
and *Brief from the last meeting of a series*.

## Privacy and data handling

- The server reads only Dicavo's own files on your Mac. It opens no network
  connection, listens on no port and sends nothing to the developer of
  Dicavo, who receives none of your meeting content.
- What the server returns goes to your AI client, and your client sends it
  to its own AI provider (Anthropic for Claude, OpenAI for Codex) under your
  account and that provider's terms. Meeting content is personal data of the
  people in your meetings; connect only clients you are allowed to use for
  it.
- You decide in Dicavo what agents may read: summaries and transcripts or
  summaries only, how far back, and, per recording, *Summary only* or *Hide
  from AI agents*. Organisations can enforce these settings with a
  configuration profile.
- Dicavo keeps a log of every call for 30 days with the time, the tool, the
  client and the outcome, never the content. You see it under **Settings →
  AI analysis → Access log**.

The full policy is at [dicavo.eu/en/privacy.html](https://dicavo.eu/en/privacy.html),
and the server's behaviour is described at
[dicavo.eu/en/mcp.html](https://dicavo.eu/en/mcp.html).

## Support

[dicavo.eu/en/support.html](https://dicavo.eu/en/support.html) or
support@dicavo.eu.

## License

Apache License 2.0. Dicavo itself is a separate app from the Mac App Store
and is not covered by this license.
