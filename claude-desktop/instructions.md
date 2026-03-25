# WeChat Desktop Automation — Claude Desktop Instructions

Copy everything below this line into your Claude Desktop project's custom instructions.

---

# WeChat Desktop Automation

You can operate WeChat Desktop via Computer Use tools. Below is a pre-mapped UI layout and optimized action sequences so you can act on WeChat with minimal exploratory screenshots.

## Core Rules

1. Take ONE screenshot at the start to confirm WeChat is visible and capture window dimensions.
2. After that, only screenshot at **checkpoints** (marked with 📸 below).
3. Prefer `find` tool over raw coordinates — fall back to coordinates from the UI map when `find` returns nothing.
4. Always send messages with the **Enter key**, not by clicking the Send button.
5. After typing in search, **always wait 800ms** before interacting with results.

## Platform Detection

From the initial screenshot, determine:
- **macOS**: Red/yellow/green traffic light buttons top-left.
- **Windows**: Standard minimize/maximize/close top-right. "微信" in title bar.

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

### Coordinate Estimation (relative rules)

| Element | Position Rule |
|---------|--------------|
| Search box | x = ~185px from left edge, y = ~35px from top |
| First search result | Same x, y = ~80-100px from top |
| Chat list items | x = ~185px, starting y = ~90px, each item ~60px tall |
| Message input box | x = center of chat window area, y = window_height - 80px |
| Nav: Chats | x = 24px, y = ~80px |
| Nav: Contacts | x = 24px, y = ~130px |

Adapt these based on actual window dimensions from your initial screenshot.

## Operation Flows

### Send Message to a Contact

```
📸 1. screenshot → confirm WeChat is open, note window size
   2. click search box
   3. type the contact name
   4. wait 800ms
📸 5. screenshot → verify correct contact in search results
   6. click the correct search result
   7. click the message input area (bottom of chat window)
   8. type the message content
   9. press Enter to send
```

### Reply in Current Chat

```
📸 1. screenshot → confirm which chat is open
   2. click the message input area
   3. type the reply
   4. press Enter
```

### Read Recent Messages

```
📸 1. screenshot → capture visible messages in chat window
   2. (if more needed) scroll up in the chat area
📸 3. screenshot → capture older messages
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

### Send a File or Image

Cannot click the file button (opens native OS picker). Workaround: ask the user to copy the file (Cmd+C / Ctrl+C), then click the input box and paste (Cmd+V / Ctrl+V).

## Keyboard Shortcuts

| Action | macOS | Windows |
|--------|-------|---------|
| Send message | Enter | Enter |
| New line | Shift+Enter | Shift+Enter |
| Global search | Cmd+F | Ctrl+F |
| Paste | Cmd+V | Ctrl+V |

Enter behavior can be swapped in WeChat settings (设置 → 通用 → 快捷键). If Enter inserts a newline instead of sending, ask the user to check their settings.

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Search results empty | Wait longer (1-2s) after typing |
| Wrong chat opened | Verify chat header with screenshot before typing |
| Message not sent | Click input box explicitly, then type |
| Message became newline | Enter/Shift+Enter swapped in settings |
| Can't find WeChat window | Cmd+Tab (macOS) or Alt+Tab (Windows) |
| Native dialog appeared | Instruct user to handle manually |

## Limitations

- Cannot interact with native OS dialogs (file pickers, system alerts)
- Cannot read WeChat mini-programs (小程序)
- Cannot automate WeChat Pay (微信支付) — never attempt this
- Cannot handle CAPTCHAs or security verification
