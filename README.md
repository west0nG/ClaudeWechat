# WeChat Desktop Plugin for Claude

Automate WeChat Desktop (微信桌面版) via Computer Use. Send messages, read chats, search contacts, forward messages — with pre-mapped UI layout that reduces steps by ~40-50%.

[中文说明](#中文说明)

## Installation

### Claude Code

```bash
claude plugin add /path/to/claudewechat
```

Or add this repo as a plugin source.

### Claude Desktop

Claude Desktop doesn't support plugins yet. Copy the contents of [`skills/wechat-desktop/SKILL.md`](skills/wechat-desktop/SKILL.md) (everything after the `---` frontmatter) into your project's **Custom Instructions**.

## Requirements

- WeChat Desktop installed and logged in
- Claude with **Computer Use** capability (screenshot + click + type)
- macOS or Windows

## What It Does

| Operation | Steps | Screenshots |
|-----------|-------|-------------|
| Send message to contact | 9 | 2 |
| Reply in current chat | 4 | 1 |
| Read messages | 2-3 | 1-2 |
| Forward a message | 7 | 3 |

Without this plugin, Claude typically needs 12-15 steps and 5-7 screenshots for a single message — this plugin cuts that by ~40-50% through pre-mapped UI coordinates and checkpoint-based screenshotting.

## Limitations

- Cannot interact with native OS file pickers (use copy-paste workaround)
- Cannot operate WeChat mini-programs (小程序)
- Cannot automate WeChat Pay (微信支付)
- Cannot handle CAPTCHAs or security verification

## License

MIT

---

## 中文说明

通过 Computer Use 自动操作微信桌面版。发消息、读聊天记录、搜索联系人、转发消息——内置 UI 布局映射，减少约 40-50% 的操作步骤。

### 安装方法

**Claude Code：**

```bash
claude plugin add /path/to/claudewechat
```

**Claude Desktop：**

Claude Desktop 暂不支持插件安装。请复制 [`skills/wechat-desktop/SKILL.md`](skills/wechat-desktop/SKILL.md) 中 `---` 分隔线以下的内容，粘贴到项目的 **Custom Instructions** 中。

### 系统要求

- 微信桌面版已安装并登录
- Claude 需支持 **Computer Use**（截图 + 点击 + 输入）
- macOS 或 Windows

### 已知限制

- 无法操作系统原生文件选择器（可用复制粘贴替代）
- 无法操作微信小程序
- 无法操作微信支付（也不应该尝试）
- 无法处理验证码或安全验证
