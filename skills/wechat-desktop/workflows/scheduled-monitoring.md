# Scheduled Monitoring (定时检查)

Use `create_scheduled_task` or `CronCreate` for periodic unread checks:

```
Prompt: "Open WeChat, screenshot chat list, check for red badges.
         If unread, click into each and summarize. Otherwise report no new messages."
Cron: "*/5 * * * *"  (every 5 minutes, adjust as needed)
```

Note: each scheduled run needs its own `request_access`. Will briefly foreground WeChat.
