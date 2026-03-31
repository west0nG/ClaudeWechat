<p align="right"><a href="README.md">中文</a></p>

# ClaudeWechat — Let Claude Operate WeChat for You

> Tell Claude what to send, and it opens WeChat, finds the contact, and sends the message — automatically.

A Claude plugin that automates **WeChat Desktop** through Computer Use (screenshot + click + type). Ships with optimized action flows so Claude doesn't waste time on exploratory screenshots.

> **Current status:** Requires Computer Use capability. Both **Claude Desktop** and **Claude Code CLI** now support Computer Use.

## What It Can Do

### Core Operations

**Send messages** — Search any contact or group chat, navigate to it, and send your message. Skips search if already in the target chat.

**Read messages** — Capture chat history via screenshots, scroll up for older messages.

### Advanced Operations

**Broadcast** — Send the same message to multiple contacts in a loop.

**Unread summary** — Scan the chat list for unread badges, click into each, and produce a structured summary.

**Quote reply** — Right-click to quote a specific message and reply to it.

**Forward messages** — Right-click forward to any contact, with Recent Forwards shortcut.

**Send files** — Send files and images via copy-paste (bypasses the native file picker).

### Efficiency Gains

Without this plugin, Claude typically needs 12-15 tool calls to send a single message. This plugin uses `computer_batch` to combine multiple actions into single calls, dramatically reducing round-trips:

| Operation | Tool Calls | Screenshots |
|-----------|-----------|-------------|
| Send message to contact | 5 | 3 |
| Broadcast to N contacts | ~4N+2 | N+1 |
| Quote reply | 4 | 2 |
| Forward a message | 5-7 | 3 |

## Project Structure

```
skills/wechat-desktop/
├── SKILL.md                        # Entry point: setup + core rules + workflow index (~40 lines)
├── workflows/                      # Operation workflows (loaded on demand)
│   ├── send-message.md             # Send message
│   ├── broadcast.md                # Broadcast
│   ├── unread-summary.md           # Unread summary
│   ├── quote-reply.md              # Quote reply
│   ├── forward.md                  # Forward
│   ├── send-file.md                # Send file
│   └── scheduled-monitoring.md     # Scheduled monitoring
└── reference/                      # Reference materials (loaded on demand)
    ├── shortcuts.md                # Keyboard shortcuts
    └── troubleshooting.md          # Troubleshooting
```

Multi-file architecture: SKILL.md loads only core information (~40 lines), specific workflows are read on demand.

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

## Requirements

- **WeChat Desktop** installed and logged in (macOS or Windows)
- Claude with **Computer Use** capability (screenshot + click + type)

## Limitations

- Cannot interact with native OS file pickers (use copy-paste workaround)
- Cannot operate mini-programs — these are embedded WebViews with their own UI
- Will not and should not automate WeChat Pay
- Cannot handle CAPTCHAs or security verification prompts
- Overlay apps (Grammarly, Typeless, etc.) may intercept clicks — add to allowed apps or close them

## License

MIT
