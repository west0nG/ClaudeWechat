# Troubleshooting

| Problem | Fix |
|---------|-----|
| "not in allowed applications" error | `open_application("WeChat")` to re-focus, or `request_access` to add the blocking app |
| Click lands on another app | Another app (e.g., Typeless, Screen Studio) overlaps WeChat. Add it to `request_access` or use `cmd+f` for search |
| Search results empty | Wait longer (1-2s) after typing |
| Wrong chat opened | Verify chat header; prefer Contacts section |
| Leftover text in input | `cmd+a` before typing |
| Enter inserts newline | User swapped setting; ask them to check |
| WeChat on wrong monitor | Use `switch_display` |
| Context menu in unexpected language | WeChat UI can be English or Chinese. Check for both (e.g., "Quote" / "引用", "Forward..." / "转发") |
| Native dialog blocks automation | Instruct user to handle manually |
