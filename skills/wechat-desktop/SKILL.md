---
name: wechat-desktop
description: "Automate WeChat Desktop (微信桌面版) on macOS or Windows via Computer Use (screenshot + click + type). Use this skill whenever the user wants to send messages, read chats, search contacts, forward messages, or perform any operation inside the WeChat desktop application. Triggers include: '微信', 'WeChat', '发消息', '发微信', 'send a WeChat message', 'reply on WeChat', '转发', '群发', or any reference to operating the WeChat desktop app. Also trigger when the user mentions a contact name and implies messaging them via WeChat, even if they don't say 'WeChat' explicitly. Do NOT use for WeChat web version (web.wechat.com) or WeChat mini-programs."
---

# WeChat Desktop Automation

Minimal screenshots, maximum batching.

## Setup

1. **`request_access(["WeChat"])`** — once per session. Add overlay apps (Typeless, etc.) if clicks are blocked.
2. **`open_application("WeChat")`** — bring to foreground. Re-call if another app steals focus.

## Core Rules

1. ONE screenshot at start to locate UI elements.
2. **`computer_batch`** to combine predictable sequences.
3. Send with **Enter**, not Send button.
4. Wait **1s** after typing in search before screenshotting.
5. **Always `cmd+a` before typing** to clear leftover text.
6. Search results: prefer **Contacts** over Group Chats or Chat History.
7. Right-click near **center of message bubble**, not at window edge (avoids overlay app conflicts).
8. Context menus can be English or Chinese — check for both (e.g., "Quote" / "引用").

## Workflows

**Read the relevant file before executing.**

| Operation | File |
|-----------|------|
| Send message | `workflows/send-message.md` |
| Broadcast (群发) | `workflows/broadcast.md` |
| Unread summary | `workflows/unread-summary.md` |
| Quote reply (引用回复) | `workflows/quote-reply.md` |
| Forward (转发) | `workflows/forward.md` |
| Send file/image | `workflows/send-file.md` |

## Read Messages

Just screenshot. Scroll up + screenshot again for older messages. Use `zoom` if text is small.

## Reference

- **Troubleshooting** → `reference/troubleshooting.md`
- **Keyboard shortcuts** → `reference/shortcuts.md`
