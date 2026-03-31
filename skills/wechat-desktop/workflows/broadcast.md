# Broadcast to Multiple Contacts (群发)

Loop over "Send Message" for each contact:

```
📸 1. screenshot → locate search box (once)
Per contact:
   2. computer_batch: [click search box, key "cmd+a", type name, wait 1s]
📸 3. screenshot → verify contact
   4. click result
   5. computer_batch: [click input box, key "cmd+a", type message, key "Return"]
   6. clear search (click X button or cmd+a + Backspace in search box)
```

**~4 calls per contact.** Skip screenshot after send unless it fails. Report progress to user after each send.
