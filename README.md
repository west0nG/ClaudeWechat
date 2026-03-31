<p align="right"><a href="README.en.md">English</a></p>

# ClaudeWechat — 让 Claude 帮你操作微信

> 一句话告诉 Claude 你想发什么，它自动打开微信、找到联系人、发出消息。

一个 Claude 插件，通过 Computer Use（屏幕截图 + 鼠标点击 + 键盘输入）自动操作**微信桌面版**。内置优化操作流程，让 Claude 不用反复截图试探，直接高效完成任务。

> **当前状态：** 此插件依赖 Computer Use 能力。**Claude Desktop** 和 **Claude Code CLI** 均已支持 Computer Use。

## 能做什么

### 核心操作

**发送消息** — 搜索联系人或群聊，自动定位并发送消息。已在目标聊天中时跳过搜索。

**阅读消息** — 截图读取聊天记录，支持向上滚动查看历史

### 进阶操作

**群发消息** — 给多个联系人发送同一条消息，自动循环搜索和发送

**未读总结** — 扫描聊天列表中的未读标记，逐个点入阅读，输出结构化摘要

**引用回复** — 右键引用某条消息并回复

**消息转发** — 右键转发到指定联系人，支持 Recent Forwards 快捷选择

**文件发送** — 通过复制粘贴发送文件和图片（绕过系统文件选择器）

### 效率对比

没有这个插件，Claude 发一条消息通常需要 12-15 次工具调用。这个插件通过 `computer_batch` 将多个操作合并为单次调用，大幅减少来回次数：

| 操作 | 工具调用次数 | 截图次数 |
|------|-------------|----------|
| 给联系人发消息 | 5 | 3 |
| 群发 N 个联系人 | ~4N+2 | N+1 |
| 引用回复 | 4 | 2 |
| 转发消息 | 5-7 | 3 |

## 项目结构

```
skills/wechat-desktop/
├── SKILL.md                        # 入口文件：setup + 核心规则 + 操作索引（~40 行）
├── workflows/                      # 操作流程（按需加载）
│   ├── send-message.md             # 发送消息
│   ├── broadcast.md                # 群发
│   ├── unread-summary.md           # 未读总结
│   ├── quote-reply.md              # 引用回复
│   ├── forward.md                  # 转发
│   ├── send-file.md                # 发送文件
│   └── scheduled-monitoring.md     # 定时监控
└── reference/                      # 参考资料（按需加载）
    ├── shortcuts.md                # 快捷键
    └── troubleshooting.md          # 常见问题排障
```

多文件架构：SKILL.md 只加载核心信息（~40 行），具体操作流程按需读取。

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

## 系统要求

- **微信桌面版**已安装并登录（macOS 或 Windows）
- Claude 需具备 **Computer Use** 能力（截图 + 点击 + 输入）

## 已知限制

- 无法操作系统原生文件选择器（可通过复制粘贴绕过）
- 无法操作小程序（小程序是独立的内嵌 WebView）
- 不会也不应该操作微信支付
- 无法处理验证码或安全验证弹窗
- Overlay 应用（如 Grammarly、Typeless）可能拦截点击，需加入权限或关闭

## License

MIT
