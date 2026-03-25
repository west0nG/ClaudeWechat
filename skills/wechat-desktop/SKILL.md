---
name: wechat-desktop
description: "Automate WeChat Desktop (微信桌面版) on macOS or Windows via Computer Use (screenshot + click + type). Use this skill whenever the user wants to send messages, read chats, search contacts, forward messages, or perform any operation inside the WeChat desktop application. Triggers include: '微信', 'WeChat', '发消息', '发微信', 'send a WeChat message', 'reply on WeChat', '转发', '群发', or any reference to operating the WeChat desktop app. Also trigger when the user mentions a contact name and implies messaging them via WeChat, even if they don't say 'WeChat' explicitly. Do NOT use for WeChat web version (web.wechat.com) or WeChat mini-programs."
---

# WeChat Desktop Automation

Operate WeChat Desktop via Computer Use tools. This skill provides a pre-mapped UI layout and optimized action sequences so Claude can act on WeChat with minimal exploratory screenshots.

## Core Philosophy: Act First, Verify at Checkpoints

The biggest performance drain is unnecessary screenshots. WeChat's layout is highly consistent — once you've confirmed the window is open, you can act on known positions without re-scanning the screen every step.

**Rules:**
1. Take ONE screenshot at the start to confirm WeChat is visible and capture window dimensions.
2. After that, only screenshot at **checkpoints** (marked with 📸 below).
3. Prefer `find` tool over raw coordinates — it's faster when it works. Fall back to coordinates from the UI map when `find` returns nothing (common with native desktop apps).
4. Always send messages with the **Enter key**, not by clicking the Send button.
5. After typing in search, **always wait 800ms** before interacting with results.

## Platform Detection

From the initial screenshot, determine:
- **macOS**: Red/yellow/green traffic light buttons top-left. Native Cocoa app.
- **Windows**: Standard minimize/maximize/close top-right. "微信" in title bar.

This matters for keyboard shortcuts (see reference table below).

## UI Layout Map

WeChat Desktop uses a fixed three-column layout:

```
┌──────┬───────────────┬─────────────────────────┐
│ Nav  │ Chat List     │ Chat Window             │
│ ~48px│ ~260px        │ remaining width         │
│      │               │                         │
│  💬  │ [Search Box]  │ [Chat Header / Name]    │
│  📒  │               │                         │
│  ⭐  │  Contact 1    │  Message history        │
│  📁  │  Contact 2    │  (scrollable)           │
│      │  Contact 3    │                         │
│      │  ...          │ [Toolbar: emoji/file/…] │
│      │               │ [Message Input Box]     │
│      │               │           [Send Button] │
└──────┴───────────────┴─────────────────────────┘
```

### Coordinate Estimation

Coordinates depend on window size. After the initial screenshot, calculate positions using these **relative rules** rather than hardcoded pixel values:

| Element | Position Rule |
|---------|--------------|
| Search box | x = ~185px from left edge, y = ~35px from top |
| First search result | Same x, y = ~80-100px from top |
| Chat list items | x = ~185px, starting y = ~90px, each item ~60px tall |
| Message input box | x = center of chat window area, y = window_height - 80px |
| Send button | x = right_edge - 80px, y = window_height - 25px (but use Enter instead) |
| Nav: Chats | x = 24px, y = ~80px |
| Nav: Contacts | x = 24px, y = ~130px |

**Adapt these based on actual window dimensions from your initial screenshot.** The ratios stay consistent even when window size varies.

## Operation Flows

Each operation below lists the exact steps. Steps marked 📸 require a screenshot; all other steps are executed blind using the UI map.

### Send Message to a Contact (most common)

```
📸 1. screenshot → confirm WeChat is open, note window size
   2. click search box (try `find "search"` first, fall back to coordinates)
   3. type the contact name
   4. wait 800ms
📸 5. screenshot → verify correct contact in search results
   6. click the correct search result
   7. click the message input area (bottom of chat window)
   8. type the message content
   9. press Enter to send
```

**Total: 9 steps, only 2 screenshots.** Without this skill, Claude typically takes 12-15 steps with 5-7 screenshots.

**Tips:**
- After step 6, the input box often auto-focuses. You can try skipping step 7 and going straight to typing. If the text doesn't appear in the input, add step 7 back.
- For Chinese names, type the full Chinese characters. WeChat search also accepts pinyin.
- If multiple contacts share similar names, ask the user for a distinguishing detail (full name, remark name, or WeChat ID).

### Reply in Current Chat (already in the right conversation)

```
📸 1. screenshot → confirm which chat is open
   2. click the message input area
   3. type the reply
   4. press Enter
```

**Total: 4 steps, 1 screenshot.**

### Read Recent Messages

```
📸 1. screenshot → capture visible messages in chat window
   2. (if more needed) scroll up in the chat area
📸 3. screenshot → capture older messages
```

**Total: 2-3 steps.** Use the `zoom` action on the chat area if text is too small to read.

### Search and Open a Group Chat

Same as "Send Message to a Contact" — group names work in search. If the group name is long, use a distinctive substring. After clicking the result:

```
📸 verify the chat header shows the correct group name before typing
```

### Forward a Message

```
📸 1. screenshot → identify the target message
   2. right-click on the message
📸 3. screenshot → find "转发" in context menu
   4. click "转发"
📸 5. screenshot → contact picker dialog appears
   6. search and select the target contact
   7. click "发送" in the dialog
```

**Total: 7 steps, 3 screenshots.** This flow is inherently more complex due to context menus and dialogs.

### Send a File or Image

**Known limitation:** Clicking the file/image button opens a native OS file picker that Computer Use cannot interact with.

**Workaround:** Instruct the user to:
1. Copy the file in Finder (Cmd+C) or Explorer (Ctrl+C)
2. Then tell Claude to click the input box and paste (Cmd+V / Ctrl+V)

Or have the user drag-and-drop the file into the chat window manually.

## Keyboard Shortcuts

| Action | macOS | Windows |
|--------|-------|---------|
| Send message | Enter | Enter |
| New line | Shift+Enter | Shift+Enter |
| Global search | Cmd+F | Ctrl+F |
| Paste | Cmd+V | Ctrl+V |
| Select all in input | Cmd+A | Ctrl+A |
| Screenshot (WeChat) | Ctrl+Cmd+A | Alt+A |

**Enter behavior can be swapped** in WeChat settings (设置 → 通用 → 快捷键). If Enter inserts a newline instead of sending, the user has changed this setting. Ask them to confirm or switch it back.

## Troubleshooting

These are the most common failure modes. The skill is designed to prevent them, but if they occur:

| Problem | Cause | Fix |
|---------|-------|-----|
| Search results empty | Typed too fast, didn't wait | Add `wait 1-2s` after typing in search |
| Wrong chat opened | Similar contact names | Verify chat header with screenshot before typing |
| Message not sent | Input box didn't have focus | Click input box explicitly, then type |
| Message became newline | Enter/Shift+Enter swapped | Ask user to check WeChat settings |
| Can't find WeChat window | App minimized or behind others | macOS: Cmd+Tab to WeChat. Windows: Alt+Tab or click taskbar icon |
| Native dialog appeared | File picker, alert, permission | Cannot automate — instruct user to handle manually |

## Performance Expectations

| Operation | Without Skill | With Skill | Savings |
|-----------|--------------|------------|---------|
| Send message to contact | 12-15 steps, 5-7 screenshots | 9 steps, 2 screenshots | ~40% fewer steps |
| Reply in current chat | 6-8 steps, 3-4 screenshots | 4 steps, 1 screenshot | ~50% fewer steps |
| Read messages | 4-6 steps | 2-3 steps | ~40% fewer steps |
| Forward a message | 10-14 steps | 7 steps, 3 screenshots | ~40% fewer steps |

Per-step latency (2-4 seconds per tool call) is unchanged — that's the API/model inference overhead, not something this skill can optimize. The gains come entirely from fewer steps and fewer screenshots.

## What This Skill Cannot Do

Be upfront with the user about these limitations:
- **Cannot interact with native OS dialogs** (file pickers, system alerts)
- **Cannot read WeChat mini-programs** (小程序) — these are embedded webviews with their own UI
- **Cannot automate WeChat Pay** (微信支付) — and should never attempt to
- **Cannot handle CAPTCHAs or verification** if WeChat triggers security checks
- **Cannot guarantee coordinates are pixel-perfect** — window size, display scaling, and OS version all affect layout. The initial calibration screenshot is essential.
