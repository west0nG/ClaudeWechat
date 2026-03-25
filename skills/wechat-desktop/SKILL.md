---
name: wechat-desktop
description: "Automate WeChat Desktop (微信桌面版) on macOS or Windows via Computer Use (screenshot + click + type). Use this skill whenever the user wants to send messages, read chats, search contacts, forward messages, or perform any operation inside the WeChat desktop application. Triggers include: '微信', 'WeChat', '发消息', '发微信', 'send a WeChat message', 'reply on WeChat', '转发', '群发', or any reference to operating the WeChat desktop app. Also trigger when the user mentions a contact name and implies messaging them via WeChat, even if they don't say 'WeChat' explicitly. Do NOT use for WeChat web version (web.wechat.com) or WeChat mini-programs."
---

# WeChat Desktop Automation

Operate WeChat Desktop via Computer Use tools. This skill provides a pre-mapped UI layout and optimized action sequences so Claude can act on WeChat with minimal exploratory screenshots.

## Prerequisites: Permission & App Focus

Before ANY interaction with WeChat, you MUST complete these two steps:

1. **`request_access`** — Call `request_access` with `["WeChat"]` to get permission. Without this, every click/type will fail.
2. **`open_application`** — Call `open_application("WeChat")` to bring WeChat to the foreground. Do NOT use Cmd+Tab — `open_application` is more reliable and handles minimized/hidden windows.

**If another app steals focus mid-operation** (e.g., a terminal or notification), call `open_application("WeChat")` again. Do NOT try to request_access for the blocking app just to dismiss it — simply re-focus WeChat.

## Core Philosophy: Batch Actions, Verify at Checkpoints

The biggest performance drains are (1) unnecessary screenshots and (2) individual tool calls for predictable sequences. WeChat's layout is highly consistent — once you've confirmed the window is open, you can batch multiple actions together.

**Rules:**
1. Take ONE screenshot at the start to confirm WeChat is visible and capture window dimensions.
2. After that, only screenshot at **checkpoints** (marked with 📸 below).
3. **Use `computer_batch`** to combine predictable action sequences (click → type → key) into a single tool call. This cuts round-trips dramatically.
4. **Skip the `find` tool** — it rarely works on native desktop apps like WeChat. Go directly to coordinate-based clicking from the initial screenshot.
5. Always send messages with the **Enter key**, not by clicking the Send button.
6. After typing in search, **always wait 800ms–1s** before screenshotting results.
7. **Always clear the input box** before typing — use `cmd+a` (macOS) or `ctrl+a` (Windows) to select all existing content, then type your new text to replace it.

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

**Do NOT use hardcoded coordinates.** WeChat window size varies. After the initial screenshot, visually locate elements and read their coordinates from the screenshot. Key landmarks to identify:

| Element | How to Find It |
|---------|---------------|
| Search box | Gray rounded rectangle at top of the middle column, with magnifying glass icon and placeholder text "Search" or "搜索" |
| First search result | First item below the "Contacts" section header in search results — prioritize exact matches under "Contacts" over "Group Chats" or "Chat History" |
| Message input box | Large white/dark text area at the bottom of the right column, above the toolbar with emoji/file icons |
| Chat header | Text at the very top of the right column showing the contact or group name |

**The relative layout is always the same** — three columns, search at top of middle column, input at bottom of right column. Use the screenshot to get actual pixel positions.

## Operation Flows

Each operation below lists the exact steps. Steps marked 📸 require a screenshot; all other steps are executed blind or batched.

### Send Message to a Contact (most common)

```
   1. request_access(["WeChat"])
   2. open_application("WeChat")
📸 3. screenshot → confirm WeChat is open, note window size, locate search box position
   4. computer_batch: [click search box, type contact name, wait 1s]
📸 5. screenshot → verify correct contact in search results
   6. click the correct search result (prefer "Contacts" section over "Group Chats" or "Chat History")
   7. computer_batch: [click input box, key "cmd+a", type message, key "Return", screenshot]
```

**Total: 7 tool calls, 3 screenshots.** Steps 1-2 are one-time setup. The actual operation is 5 calls.

**Tips:**
- Step 7 uses `cmd+a` before typing to clear any leftover text in the input box. This prevents accidentally prepending to old drafts.
- For Chinese messages, the `type` action handles Chinese characters directly — no IME interaction needed from Computer Use.
- If the contact name yields multiple results, the search results are grouped: **Contacts** (individual), **Group Chats**, **Chat History**. Always click the match under "Contacts" unless the user specifically asked for a group.
- If multiple contacts share similar names, ask the user for a distinguishing detail (full name, remark name, or WeChat ID).

### Reply in Current Chat (already in the right conversation)

```
📸 1. screenshot → confirm which chat is open
   2. computer_batch: [click input box, key "cmd+a", type reply, key "Return"]
```

**Total: 2 tool calls, 1 screenshot.**

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

| Problem | Cause | Fix |
|---------|-------|-----|
| Tool call fails with "not in allowed applications" | Another app stole focus | Call `open_application("WeChat")` to re-focus, then retry |
| Search results empty | Typed too fast, didn't wait | Add `wait 1-2s` after typing in search |
| Wrong chat opened | Similar contact names | Verify chat header with screenshot; prefer "Contacts" section results |
| Input box has leftover text | Previous draft not cleared | Always use `cmd+a` before typing to select and replace |
| Message not sent | Input box didn't have focus | Click input box explicitly, then type |
| Message became newline | Enter/Shift+Enter swapped | Ask user to check WeChat settings |
| WeChat not visible after open_application | App on different monitor | Check screenshot note for monitor info; use `switch_display` if needed |
| Native dialog appeared | File picker, alert, permission | Cannot automate — instruct user to handle manually |
| Chinese text garbled or wrong | IME interference | The `type` action sends characters directly; if IME intercepts, try clicking input box first to ensure it has focus |

## Performance Expectations

| Operation | Without Skill | With Skill | Savings |
|-----------|--------------|------------|---------|
| Send message to contact | 12-15 calls | 7 calls | ~50% fewer calls |
| Reply in current chat | 6-8 calls | 2 calls | ~70% fewer calls |
| Read messages | 4-6 calls | 2-3 calls | ~50% fewer calls |
| Forward a message | 10-14 calls | 7 calls | ~40% fewer calls |

The savings come from:
1. Using `computer_batch` to combine click → type → key sequences
2. Skipping `find` tool (doesn't work on native apps)
3. Knowing to clear input with `cmd+a` before typing
4. Using `open_application` instead of manual app switching

## What This Skill Cannot Do

Be upfront with the user about these limitations:
- **Cannot interact with native OS dialogs** (file pickers, system alerts)
- **Cannot read WeChat mini-programs** (小程序) — these are embedded webviews with their own UI
- **Cannot automate WeChat Pay** (微信支付) — and should never attempt to
- **Cannot handle CAPTCHAs or verification** if WeChat triggers security checks
- **Cannot guarantee coordinates are pixel-perfect** — always read positions from the initial screenshot rather than using hardcoded values
