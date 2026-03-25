<p align="right"><a href="README.en.md">English</a></p>

# ClaudeWechat — 让 Claude 帮你操作微信

> 一句话告诉 Claude 你想发什么，它自动打开微信、找到联系人、发出消息。

这是一个 Claude 插件，通过 Computer Use（屏幕截图 + 鼠标点击 + 键盘输入）自动操作**微信桌面版**。内置完整的 UI 布局映射和优化操作流程，让 Claude 不用反复截图试探，直接高效完成任务。

> **当前状态：** 此插件依赖 Computer Use 能力。目前仅 **Claude Desktop** 支持 Computer Use，Claude Code CLI 暂不支持。CLI 用户可先安装，等支持后即可使用。

## 能做什么

**对话发送** — 搜索联系人或群聊，自动定位并发送消息

**快速回复** — 在当前聊天窗口直接输入并发送

**消息阅读** — 截图读取聊天记录，支持向上滚动查看历史

**消息转发** — 右键转发到指定联系人

**文件发送** — 通过复制粘贴发送文件和图片（绕过系统文件选择器）

### 效率对比

没有这个插件，Claude 发一条消息通常需要 12-15 次工具调用——大量时间花在「截图→找位置→再截图」的循环上。

这个插件内置了微信的完整 UI 布局图，并通过 `computer_batch` 将多个操作合并为单次调用，大幅减少来回次数：

| 操作 | 工具调用次数 | 截图次数 | 节省 |
|------|-------------|----------|------|
| 给联系人发消息 | 7 | 3 | ~50% |
| 回复当前聊天 | 2 | 1 | ~70% |
| 阅读聊天记录 | 2-3 | 1-2 | ~50% |
| 转发消息 | 7 | 3 | ~40% |

## 安装

### Claude Desktop

Claude Desktop 暂不支持从 GitHub 直接安装第三方插件。请手动添加：

1. 打开 [`skills/wechat-desktop/SKILL.md`](skills/wechat-desktop/SKILL.md)
2. 复制 `---` 分隔线以下的**全部内容**
3. 粘贴到你的项目 **Custom Instructions** 中

### Claude Code

```bash
claude plugin marketplace add https://github.com/west0nG/ClaudeWechat
claude plugin install wechat-desktop
```

> Claude Code CLI 暂不支持 Computer Use，安装后需等 CLI 支持才能使用。

## 系统要求

- **微信桌面版**已安装并登录（macOS 或 Windows）
- Claude 需具备 **Computer Use** 能力（截图 + 点击 + 输入）

## 已知限制

- 无法操作系统原生文件选择器（可通过复制粘贴绕过）
- 无法操作小程序（小程序是独立的内嵌 WebView）
- 不会也不应该操作微信支付
- 无法处理验证码或安全验证弹窗

## License

MIT
