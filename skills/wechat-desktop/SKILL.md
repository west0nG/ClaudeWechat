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

## Operations

### Send Message to a Contact

```
📸 1. screenshot → locate search box
   2. computer_batch: [click search box, type contact name, wait 1s]
📸 3. screenshot → verify correct contact in results
   4. click correct search result (prefer Contacts section)
   5. computer_batch: [click input box, key "cmd+a", type message, key "Return", screenshot]
```
**5 calls, 3 screenshots.** (Setup calls excluded.)

Tips: `cmd+a` clears leftover drafts. `type` handles Chinese directly. Ask user for full name/WeChat ID if multiple similar contacts appear.

### Reply in Current Chat

```
📸 1. screenshot → confirm which chat is open
   2. computer_batch: [click input box, key "cmd+a", type reply, key "Return"]
```
**2 calls, 1 screenshot.**

### Read Recent Messages

```
📸 1. screenshot → capture visible messages
   2. (optional) scroll up + screenshot for older messages
```
**2-3 calls.** Use `zoom` if text is small.

### Broadcast to Multiple Contacts (群发)

Loop over "Send Message" for each contact:
```
📸 1. screenshot → locate search box (once)
Per contact:
   2. computer_batch: [click search box, type name, wait 1s]
📸 3. screenshot → verify contact
   4. click result
   5. computer_batch: [click input box, key "cmd+a", type message, key "Return"]
   6. clear search (click X button or cmd+a + Backspace in search box)
```
**~4 calls per contact.** Skip screenshot after send unless it fails. Report progress to user after each send.

### Read and Summarize Unread Messages

```
📸 1. screenshot → capture chat list
📸 2. zoom on chat list column → identify ALL chats with red badges or red dots (easy to miss at full resolution)
Per unread chat:
   3. click the chat
📸 4. screenshot → read messages (zoom if needed)
After all:
   5. present structured summary to user
```

**Critical: always zoom the chat list first.** Red badges (number) and muted red dots are small and easy to miss in a full-resolution screenshot. Zoom the left column (~170-400px x range) to catch all unreads before clicking into any chat.

**Beware of list reordering:** after clicking into a chat, the chat list re-sorts (most recent on top). Always re-read chat positions from the latest screenshot before clicking the next one — never reuse old coordinates.

Red badges = unread count. Small red dot = muted unread. For groups, summarize only the most recent visible messages. Format:
```
- [Contact A] (3 unread): "latest message..."
- [Group B] (12 unread): [person] discussing [topic]
```

### Quote Reply (引用回复)

```
📸 1. screenshot → find target message
   2. right-click on the message bubble
📸 3. screenshot → find "引用" (NOT "转发") in context menu
   4. click "引用"
   5. computer_batch: [type reply, key "Return"]
```
**5 calls, 2 screenshots.** Cursor auto-focuses in input after clicking "引用".

### Forward a Message

```
📸 1. screenshot → find target message
   2. right-click on message
📸 3. screenshot → click "转发" in context menu
📸 4. screenshot → search and select target contact in picker dialog
   5. click "发送"
```
**5-7 calls, 3 screenshots.** More screenshots needed due to context menus and dialogs.

### Send a File or Image

**Cannot automate** — native file picker is inaccessible. Workaround: user copies file in Finder/Explorer, then Claude pastes with `cmd+v` in the input box.

## Scheduled Monitoring (定时检查)

Use `create_scheduled_task` or `CronCreate` for periodic unread checks:
```
Prompt: "Open WeChat, screenshot chat list, check for red badges.
         If unread, click into each and summarize. Otherwise report no new messages."
Cron: "*/5 * * * *"  (every 5 minutes, adjust as needed)
```
Note: each scheduled run needs its own `request_access`. Will briefly foreground WeChat.

## Keyboard Shortcuts

| Action | macOS | Windows |
|--------|-------|---------|
| Send message | Enter | Enter |
| New line | Shift+Enter | Shift+Enter |
| Select all | Cmd+A | Ctrl+A |
| Paste | Cmd+V | Ctrl+V |
| Search | Cmd+F | Ctrl+F |

Enter/Shift+Enter can be swapped in WeChat settings (设置 → 通用 → 快捷键).

## Troubleshooting

| Problem | Fix |
|---------|-----|
| "not in allowed applications" error | `open_application("WeChat")` to re-focus |
| Search results empty | Wait longer (1-2s) after typing |
| Wrong chat opened | Verify chat header; prefer Contacts section |
| Leftover text in input | `cmd+a` before typing |
| Enter inserts newline | User swapped setting; ask them to check |
| WeChat on wrong monitor | Use `switch_display` |
| Native dialog blocks automation | Instruct user to handle manually |

## Limitations

- Cannot interact with native OS dialogs (file pickers, alerts)
- Cannot operate WeChat mini-programs (小程序) or WeChat Pay (微信支付)
- Cannot handle CAPTCHAs or security verification
- Coordinates are approximate — always calibrate from initial screenshot
