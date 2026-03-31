# Read and Summarize Unread Messages

```
📸 1. screenshot → capture chat list
📸 2. zoom on chat list column → identify ALL chats with red badges or red dots (easy to miss at full resolution)
Per unread chat:
   3. click the chat
📸 4. screenshot → read messages (zoom if needed)
After all:
   5. present structured summary to user
```

## Critical: Always zoom the chat list first

Red badges (number) and muted red dots are small and easy to miss in a full-resolution screenshot. Zoom the left column (~170-400px x range) to catch all unreads before clicking into any chat.

## Beware of list reordering

After clicking into a chat, the chat list re-sorts (most recent on top). Always re-read chat positions from the latest screenshot before clicking the next one — never reuse old coordinates.

## Output format

Red badges = unread count. Small red dot = muted unread. For groups, summarize only the most recent visible messages.

```
- [Contact A] (3 unread): "latest message..."
- [Group B] (12 unread): [person] discussing [topic]
```
