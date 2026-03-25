<p align="right"><a href="README.md">中文</a></p>

# ClaudeWechat — Let Claude Operate WeChat for You

> Tell Claude what to send, and it opens WeChat, finds the contact, and sends the message — automatically.

A Claude plugin that automates **WeChat Desktop** through Computer Use (screenshot + click + type). Ships with a complete UI layout map and optimized action flows so Claude doesn't waste time on exploratory screenshots.

> **Current status:** Requires Computer Use capability. Currently only **Claude Desktop** supports Computer Use — Claude Code CLI does not yet. CLI users can install now and it'll activate once Computer Use lands in the CLI.

## What It Can Do

### Core Operations

**Send messages** — Search any contact or group chat, navigate to it, and send your message.

**Quick reply** — Type and send directly in the current conversation.

**Read messages** — Capture chat history via screenshots, scroll up for older messages.

### Advanced Operations

**Broadcast** — Send the same message to multiple contacts in a loop.

**Unread summary** — Scan the chat list for unread badges, click into each, and produce a structured summary.

**Quote reply** — Right-click to quote a specific message and reply to it.

**Forward messages** — Right-click forward to any contact.

**Send files** — Send files and images via copy-paste (bypasses the native file picker).

### Automation

**Scheduled monitoring** — Combine with `scheduled-tasks` to periodically check for unread messages and alert you.

### Efficiency Gains

Without this plugin, Claude typically needs 12-15 tool calls to send a single message — most of the time spent in "take screenshot → figure out what to click → take another screenshot" loops.

This plugin ships a complete UI layout map for WeChat and uses `computer_batch` to combine multiple actions into single calls, dramatically reducing round-trips:

| Operation | Tool Calls | Screenshots | Savings |
|-----------|-----------|-------------|---------|
| Send message to contact | 5 | 3 | ~55% |
| Reply in current chat | 2 | 1 | ~70% |
| Read messages | 2-3 | 1-2 | ~50% |
| Broadcast to N contacts | ~4N+2 | N+1 | ~65% |
| Unread summary | 8-12 | 5-8 | ~40% |
| Quote reply | 5 | 2 | ~50% |
| Forward a message | 5-7 | 3 | ~40% |

## Installation

### Claude Desktop

Claude Desktop doesn't support installing third-party plugins from GitHub directly. Add it manually:

1. Open [`skills/wechat-desktop/SKILL.md`](skills/wechat-desktop/SKILL.md)
2. Copy everything **below** the `---` frontmatter
3. Paste into your project's **Custom Instructions**

### Claude Code

```bash
claude plugin marketplace add https://github.com/west0nG/ClaudeWechat
claude plugin install wechat-desktop
```

> Claude Code CLI doesn't support Computer Use yet — the plugin will activate once it does.

## Requirements

- **WeChat Desktop** installed and logged in (macOS or Windows)
- Claude with **Computer Use** capability (screenshot + click + type)

## Limitations

- Cannot interact with native OS file pickers (use copy-paste workaround)
- Cannot operate mini-programs (小程序) — these are embedded WebViews with their own UI
- Will not and should not automate WeChat Pay
- Cannot handle CAPTCHAs or security verification prompts

## License

MIT
