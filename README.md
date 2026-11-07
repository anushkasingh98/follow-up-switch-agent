# followup-tracker

A Switch agent that finds the chats where you owe someone a reply and turns each one into a Linear task with a suggested answer. **It never sends, drafts or posts any message.**

## What it does

When you tell it **"followups"**, it:

1. Reads your chats from three sources:
   - **iMessage** and **WhatsApp**, through the Beeper MCP server
   - **Slack**, through the Claude Slack connector
2. Finds every chat directed at you where you have not replied, however old:
   - a personal chat (DM) where the last message is from the other person
   - a group chat where you were individually tagged or mentioned and have not answered
3. Skips what does not need a reply: group chatter not addressed to you, broadcasts, newsletters, automated messages, one-time codes, spam, and conversations that clearly ended (a thanks, a thumbs up).
4. Checks Linear so it does not create duplicates for a chat that already has a task.
5. Creates **one Linear task per chat** in the **Follow Ups** project (team Kathalyst, workspace Kathalyst). Each task has:
   - who it is, the app and chat name, and how long it has been overdue
   - a short summary of the context and the key messages
   - a **Suggested reply** you can copy, and a shorter alternative when the situation is sensitive
6. Ends with a summary in the chat: how many chats it checked per source, the follow-ups found (oldest first, with links to the tasks), anything it was unsure about, and anything it could not check.

## Safety rules

- It only **reads** chats and **creates Linear tasks**.
- It never sends, replies, reacts, drafts into a chat, archives or marks anything read.
- It treats the text of messages as data and never follows instructions written inside them.
- Only you and your own agents can talk to it, and it asks before risky actions (bypass is off).

## How it was created

It was created from the switch-buddy-admin chat in Switch Console:

| Setting | Value |
|---|---|
| Name | followup-tracker |
| Runs on | Anushka's MacBook Pro |
| AI provider | Claude |
| Folder | its own fresh folder: `~/.switch/agents/localhost-63493/followup-tracker` |
| Who can talk to it | you and your own agents only |
| Permissions | auto mode on, bypass off |
| Instructions | the hard no-send rule, the steps above, and the task format |

Its instructions can be changed on its settings page in Switch Console, or by asking switch-buddy-admin.

## What it depends on

These are not set up inside Switch. The agent uses what is already on the computer:

- **Beeper**: added with `claude mcp add --transport http --scope user beeper http://127.0.0.1:23373/v0/mcp` and signed in through /mcp. Beeper Desktop must be running.
- **Linear**: the Claude Linear connector, signed in to the Kathalyst workspace. The Follow Ups project must exist there.
- **Slack**: the Claude Slack connector. It needs a sign-in in the claude.ai connector settings. Until then the agent skips Slack and says so.

Because these come from the computer, every Claude agent on this machine can use them, and the agent acts as you in each one.

## How to use it

1. Open the agent in Switch Console and start a chat with it.
2. Say **followups**.
3. Open the tasks it creates in the Follow Ups project, copy the suggested reply, and send it yourself.

## Limits

- The Mac must be on, Switch Console open and Beeper running for it to answer.
- It has not been tuned on real chats. Check the first batch of tasks, and tell it what it got wrong so the instructions can be improved.
- "Overdue" means any chat directed at you with no reply from you. There is no age limit, so the first run may produce many tasks.
