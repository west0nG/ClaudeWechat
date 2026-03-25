# WeChat Desktop Automation Skill

Automate WeChat Desktop (微信桌面版) on macOS or Windows via Computer Use. Send messages, read chats, search contacts, forward messages, and more — all through Claude.

## Versions

### Claude Code Plugin (Marketplace)

Install as a Claude Code plugin:

```
skills/wechat-desktop/SKILL.md
```

### Claude Desktop (Project Instructions)

Copy the contents of `claude-desktop/instructions.md` into your Claude Desktop project's custom instructions.

## Features

- **Send messages** to any contact or group chat (9 steps, 2 screenshots)
- **Reply** in current conversation (4 steps, 1 screenshot)
- **Read messages** from any chat
- **Forward messages** to other contacts
- **Search** contacts and group chats
- Pre-mapped UI layout eliminates exploratory screenshots (~40-50% fewer steps)

## Requirements

- WeChat Desktop installed and logged in
- Claude with Computer Use capability (screenshot + click + type)
- macOS or Windows

## Limitations

- Cannot interact with native OS file pickers
- Cannot operate WeChat mini-programs (小程序)
- Cannot automate WeChat Pay (微信支付)
- Cannot handle CAPTCHAs or security verification

## License

MIT
