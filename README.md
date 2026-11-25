# 🤖 AI Agent for Project Management (n8n)

An automation that turns meeting recordings into action. After a meeting ends, this workflow reads the transcript, lets an AI pick out the tasks, saves them in Airtable, emails the client, and schedules follow-up meetings, all without any manual work.

## ✨ What It Does

1. **Webhook** receives a signal when a meeting has been processed in Fireflies.
2. **Get Meeting Content** fetches the transcript, summary and action items from Fireflies.
3. **AI Agent** (powered by OpenAI) reads the meeting and decides what to do.
4. The AI Agent uses three tools:
   - 📋 **Create Tasks**: adds each action item to Airtable
   - 📧 **Notify Client About Tasks**: sends a summary email through Gmail
   - 📅 **Create Event**: books follow-up meetings in Google Calendar

```
Webhook → Get Meeting Content → AI Agent
                                   ├── OpenAI Chat Model
                                   ├── Create Tasks (Airtable)
                                   ├── Notify Client (Gmail)
                                   └── Create Event (Google Calendar)
```

## 🧰 What You Need (All Have Free Options)

| Tool | Why you need it |
|------|-----------------|
| [n8n](https://n8n.io) | Runs the workflow (cloud or self-hosted) |
| [Fireflies.ai](https://fireflies.ai) | Records and transcribes meetings |
| [OpenAI](https://platform.openai.com) | The AI brain (needs an API key) |
| [Airtable](https://airtable.com) | Stores the tasks |
| Gmail | Sends emails to the client |
| Google Calendar | Schedules events |

## 🚀 Setup Guide (Step by Step)

### Step 1: Import the workflow
1. Open n8n and create a new workflow.
2. Click the **⋯** menu (top right) → **Import from file**.
3. Choose `meeting-ai-project-manager.json`.

### Step 2: Prepare Airtable
1. Create a new Base with a table (for example, "Tasks").
2. Add these columns: **Name**, **Notes**, **Due Date**.
3. Copy your **Base ID** and **Table ID** (find them in the Airtable URL or API docs).
4. Open the **Create Tasks** node and paste both IDs.

### Step 3: Add your Fireflies API key
1. In Fireflies, go to **Settings → Developer Settings** and copy your API key.
2. Open the **Get Meeting Content** node.
3. Replace `YOUR_FIREFLIES_API_KEY` with your key.

### Step 4: Connect your accounts (credentials)
Click each node with a red warning triangle and connect your account:
- **OpenAI Chat Model** → OpenAI API key
- **Create Tasks** → Airtable token
- **Notify Client About Tasks** → Gmail login
- **Create Event** → Google Calendar login

### Step 5: Connect the Webhook
1. Open the **Webhook** node and copy the **Production URL**.
2. In Fireflies, go to **Settings → Developer Settings → Webhooks** and paste the URL.
3. Toggle the workflow to **Active** (top right).

## 🧪 How to Test

1. Click **Listen for test event** on the Webhook node.
2. Send a test POST request, for example with Postman or curl:
   ```bash
   curl -X POST YOUR_TEST_WEBHOOK_URL \
     -H "Content-Type: application/json" \
     -d '{"meetingId": "YOUR_FIREFLIES_MEETING_ID"}'
   ```
3. Check that:
   - New rows appear in Airtable ✅
   - The client email is sent ✅
   - A calendar event is created (only if a follow-up meeting was mentioned) ✅

## 🛠️ Troubleshooting

| Problem | Fix |
|---------|-----|
| Red triangle on a node | A credential is missing. Open the node and connect your account. |
| Fireflies returns an error | Check that your API key is correct and the `meetingId` exists. |
| Airtable error | Make sure the column names are exactly `Name`, `Notes` and `Due Date`. |
| Nothing happens after a meeting | Confirm the workflow is **Active** and the Production URL is set in Fireflies. |
| AI invents tasks | Edit the AI Agent's system message to make the instructions stricter. |

## 🎨 Customize It

- Change the AI behaviour by editing the **System Message** in the AI Agent node.
- Swap `gpt-4o-mini` for a different OpenAI model in the Chat Model node.
- Add a **Memory** node to the AI Agent if you want it to remember past meetings.

## 📚 What I Learned

- How AI Agents choose and call tools on their own
- Using webhooks to trigger event-driven automations
- Connecting several apps (Fireflies, Airtable, Gmail, Google Calendar) with n8n
- Writing clear prompts for reliable AI output

## 👤 Author

Built by a BS Artificial Intelligence student learning automation with n8n. Feel free to fork, improve and share! ⭐

## 📄 License

MIT: free to use and modify.
