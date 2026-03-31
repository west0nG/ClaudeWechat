# Troubleshooting

| Problem | Fix |
|---------|-----|
| "not in allowed applications" error | `open_application("WeChat")` to re-focus, or `request_access` to add the blocking app |
| Click lands on overlay app | Add it to `request_access`. If bundle ID unknown, right-click closer to center of WeChat window, or ask user to close the overlay |
| Context menu blocked by overlay | Right-click near center of message bubble, not at window edge. Overlay apps (Grammarly, Typeless) tend to attach to edges |
| Search results empty | Wait longer (1-2s) after typing |
| Wrong chat opened | Verify chat header; prefer Contacts section in search results |
| Leftover text in input | `cmd+a` before typing |
| Enter inserts newline | User swapped setting; ask them to check (Settings → General → Shortcuts) |
| WeChat on wrong monitor | Use `switch_display` |
| Context menu in unexpected language | Check for both English and Chinese labels (e.g., "Quote" / "引用", "Forward..." / "转发", "Send" / "发送") |
| Native file picker dialog | Cannot automate. Ask user to copy file in Finder, then paste with `cmd+v` |
| Mini-programs or WeChat Pay | Cannot automate. Instruct user to handle manually |
