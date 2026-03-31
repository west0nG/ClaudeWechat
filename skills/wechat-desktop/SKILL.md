---
name: wechat-desktop
description: "Automate WeChat Desktop (微信桌面版) on macOS or Windows via Computer Use (screenshot + click + type). Use this skill whenever the user wants to send messages, read chats, search contacts, forward messages, or perform any operation inside the WeChat desktop application. Triggers include: '微信', 'WeChat', '发消息', '发微信', 'send a WeChat message', 'reply on WeChat', '转发', '群发', or any reference to operating the WeChat desktop app. Also trigger when the user mentions a contact name and implies messaging them via WeChat, even if they don't say 'WeChat' explicitly. Do NOT use for WeChat web version (web.wechat.com) or WeChat mini-programs."
---

# WeChat Desktop Automation

Operate WeChat Desktop via Computer Use tools with minimal screenshots and maximum batching.

## Setup (required before EVERY operation)

1. **`request_access(["WeChat"])`** — get permission (once per session)
2. **`open_application("WeChat")`** — bring to foreground. Use this instead of Cmd+Tab. Re-call if another app steals focus.

## Core Rules

1. Take ONE screenshot at the start to confirm WeChat is visible and locate UI elements.
2. **Use `computer_batch`** to combine predictable sequences into one call.
3. **Skip `find` tool** — doesn't work on native apps. Use coordinates from screenshot.
4. Send with **Enter key**, not Send button.
5. Wait **1s** after typing in search before screenshotting results.
6. **Always `cmd+a` before typing** in input box to clear leftover text.
7. **macOS**: Cmd shortcuts. **Windows**: Ctrl shortcuts. Detect from traffic lights vs title bar.

## UI Layout

```
┌──────┬───────────────┬─────────────────────────┐
│ Nav  │ Chat List     │ Chat Window             │
│~48px │ ~260px        │ remaining width         │
│  💬  │ [Search Box]  │ [Chat Header / Name]    │
│  📒  │  Contact 1    │  Message history        │
│  ⭐  │  Contact 2    │  (scrollable)           │
│  📁  │  ...          │ [Toolbar: emoji/file/…] │
│      │               │ [Message Input Box]     │
└──────┴───────────────┴─────────────────────────┘
```

Read actual coordinates from the initial screenshot. Don't use hardcoded values. Search results group into **Contacts** > **Group Chats** > **Chat History** — always prefer Contacts matches.

## Workflows

Each operation has its own file. **Read the relevant file before executing.**

| Operation | File | Calls | Screenshots |
|-----------|------|-------|-------------|
| Send message | `workflows/send-message.md` | 5 | 3 |
| Reply in current chat | `workflows/reply.md` | 2 | 1 |
| Read messages | `workflows/read-messages.md` | 2-3 | 1-2 |
| Broadcast (群发) | `workflows/broadcast.md` | ~4N+2 | N+1 |
| Unread summary | `workflows/unread-summary.md` | 8-12 | 5-8 |
| Quote reply (引用回复) | `workflows/quote-reply.md` | 5 | 2 |
| Forward message (转发) | `workflows/forward.md` | 5-7 | 3 |
| Send file/image | `workflows/send-file.md` | 2 | 1 |
| Scheduled monitoring | `workflows/scheduled-monitoring.md` | varies | varies |

## Reference

- **Keyboard shortcuts** → `reference/shortcuts.md`
- **Troubleshooting** → `reference/troubleshooting.md`

## Limitations

- Cannot interact with native OS dialogs (file pickers, alerts)
- Cannot operate WeChat mini-programs (小程序) or WeChat Pay (微信支付)
- Cannot handle CAPTCHAs or security verification
- Coordinates are approximate — always calibrate from initial screenshot
