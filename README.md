# Wapio Autonomous AI WhatsApp Community Manager (n8n + Native Polls)

An autonomous, editorial-grade WhatsApp community manager built with **n8n**, **Wapio WhatsApp API**, and **Google Gemini 3.5 Flash**.

This bot automatically posts high-signal technical content, rotates daily editorial archetypes, maintains persistent anti-repetition memory, and sends **native interactive WhatsApp polls** directly into your WhatsApp groups or channels.

---

## ⚡ Overview & Architecture

```
[Schedule Trigger (9 AM & 6 PM)] / [Manual Test Trigger]
                     │
         ⚙️ Community Bot Settings
 (Brand, Audience, Archetype, Depth, Priority Topics)
                     │
       🧠 Memory & Day Cadence Engine
(Anti-Repetition Tracking + Daily Slot Determination)
                     │
        🤖 Gemini Autonomous Editor
         (Google Gemini 3.5 Flash)
                     │
      🛡️ Sanitize WhatsApp Text & Poll
  (Mobile Formatting & Payload Sanitization)
                     │
      📲 Wapio - Send Text to WhatsApp
(Delivers Branded Formatted Post to Group JID)
                     │
          Has Interactive Poll? (If)
                     │ [true]
    📊 Wapio - Send Interactive Poll
(Delivers Native WhatsApp Poll with One-Tap Voting)
```

---

## 🌟 Key Features

- **Automated Morning & Evening Cadence:**
  - **Morning Kickstart (9 AM):** High-signal, actionable briefing to start the day.
  - **Evening Roundtable (6 PM):** Relaxed, thought-provoking discussion ending with an interactive native WhatsApp poll.
- **Native WhatsApp Interactive Polls:**
  - Uses the official `n8n-nodes-wapio` node with `operation: "sendPoll"`.
  - Group members tap poll options directly within WhatsApp to vote in real time.
- **Anti-Repetition Persistent Memory:**
  - Tracks a sliding window of the last 30 covered topics using n8n workflow static data so the bot never repeats itself.
- **7 High-Signal Day Archetypes:**
  - **Monday:** `TACTICAL_PLAYBOOK` (rules of thumb, practical code smells, heuristics)
  - **Tuesday:** `ARCHITECTURE_TEARDOWN` (stack debates: Postgres vs specialized DBs, Monolith vs Microservices)
  - **Wednesday:** `PRODUCTION_WAR_STORY` (realistic outage, migration regret, tech debt)
  - **Thursday:** `HIDDEN_TRADEOFFS` (cloud billing surprises, cold starts, vendor lock-in)
  - **Friday:** `POLARIZING_DEBATE` (contrarian takes that challenge common wisdom)
  - **Weekend:** `FUTURE_HORIZONS` & `DEEP_CRAFT_REFLECT` (local AI tooling, open-source gems, engineering craft)
- **Zero Corporate Fluff:**
  - Strictly formatted for WhatsApp with bold hooks, quote bars (`>`), and monospace (`` `code` ``).
  - No generic templates or boring "Yes / No / Maybe" polls.

---

## 📦 Files in this Folder

| File | Description |
| :--- | :--- |
| `wapio_ai_community_manager.json` | Complete n8n workflow template ready for direct copy-paste import into n8n. |
| `README.md` | Complete setup, configuration, and group JID discovery guide. |

---

## 🛠️ Prerequisites

1. **n8n Instance:** Self-hosted or n8n Cloud (v1.0.0+) with community node support.
2. **Wapio Account & WhatsApp Session:** An active connected WhatsApp session from [wapio.io](https://wapio.io).
3. **Wapio Session Key:** Copied from your connected session in the Wapio dashboard.
4. **Google AI Studio API Key:** Free Gemini API key from [Google AI Studio](https://aistudio.google.com/).

---

## 🚀 Quick Setup (6 Steps)

### Step 1: Install the Wapio Community Node in n8n

1. In n8n, open **Settings** (gear icon in the bottom-left corner).
2. Click **Community nodes** $\rightarrow$ **Install a community node**.
3. Type `n8n-nodes-wapio` and click **Install**.

---

### Step 2: Import the Workflow into n8n

1. Open `wapio_ai_community_manager.json`.
2. Copy the entire raw JSON content (`Ctrl + C` or `Cmd + C`).
3. In your n8n workspace, open an empty workflow canvas, click into the canvas, and press `Ctrl + V` (or `Cmd + V`).
4. The complete workflow will appear immediately.

---

### Step 3: Customize Bot Settings & Add Your Group JID

Double-click the **⚙️ Community Bot Settings** node to customize your bot parameters:

```javascript
return [{
  json: {
    brandName: "TECH PULSE",
    recipient: "YOUR_GROUP_OR_CHANNEL_JID@g.us",
    niche: "Modern Software Engineering, AI Coding Tools, and System Architecture",
    audience: "Senior Developers, Tech Leads, Founders, and Systems Builders",
    persona: "Opinionated Staff Engineer with 15 years in production. Pragmatic, allergic to corporate hype, values simplicity & rock-solid reliability, speaks like an authentic peer in a dev Discord.",
    contentArchetype: "AUTO_ROTATE",
    contentDepth: "DEEP_INSIGHT",
    priorityTopics: [
      "AI code generation vs architecture decay: why seniors spend 4x longer reviewing PRs",
      "The $30,000 cloud bill horror story: serverless cold starts & API loops vs a $40 Hetzner VPS",
      "Postgres is almost always enough: why teams regret adopting distributed databases too early",
      "Local LLMs with Ollama/vLLM vs OpenAI cloud dependency in production systems",
      "The microservices trap: how premature splitting kills team velocity",
      "Dead code & dependency bloat: the hidden vulnerability nobody talks about",
      "Async engineering culture vs the disease of back-to-back calendar meetings"
    ],
    customRules: "Avoid generic LinkedIn-style motivational fluff. No forced labels like 'The Real Story:' or 'Takeaway:'. Speak with technical authority, include specific tools/numbers/trade-offs, and make the debate feel like a lively team standup debate.",
    enablePolls: true,
    pollMultiSelect: false
  }
}];
```

#### 🔍 How to Find Your WhatsApp Group JID:
1. Open the [Wapio Documentation](https://docs.wapio.net) and navigate to **Endpoints** $\rightarrow$ **List Groups** (`GET /api/groups`).
2. Choose your preferred language (e.g. cURL, JavaScript, or Python) and copy the code snippet.
3. Open your editor or terminal, paste the code in, add your session key, and run it.
4. The API response will list all WhatsApp groups your session is in:
   ```json
   {
     "status": "success",
     "data": [
       {
         "id": "12036302839281928@g.us",
         "subject": "Tech Pulse Community"
       }
     ]
   }
   ```
5. Copy the `id` ending in `@g.us` and paste it into the `recipient` field.

---

### Step 4: Configure Google Gemini Credentials

1. Open the **Google Gemini Chat Model** node (connected beneath the AI Editor).
2. Under **Credential for Google Gemini(PaLM) Api**, click **Create New Credential**.
3. Paste your free Gemini API key from [Google AI Studio](https://aistudio.google.com/).
4. Ensure the model is set to `models/gemini-3.5-flash`.

---

### Step 5: Connect Your Wapio Session

1. Open the **📲 Wapio - Send Text to WhatsApp** node.
2. Under **Credential for Wapio API**, click **Create New Credential** and paste your **Session Key** from your Wapio dashboard.
3. In the **Session** dropdown, select your connected WhatsApp session.
4. Verify that the **📊 Wapio - Send Interactive Poll** node is also connected to the same session credential.

---

### Step 6: Test & Activate

1. Click **Test step** on the **Manual Trigger (Test Now)** node.
2. Observe execution flowing through: Settings $\rightarrow$ Cadence $\rightarrow$ Gemini $\rightarrow$ Wapio Text $\rightarrow$ Wapio Poll.
3. Check your WhatsApp group: your formatted insight post will arrive, immediately followed by the native WhatsApp interactive poll!
4. Once verified, toggle the workflow switch in the top right to **Active** to run on schedule automatically every day.

---

## ⚙️ Parameters Reference

| Parameter | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `brandName` | string | `"TECH PULSE"` | Name displayed in the top header tag of every post. |
| `recipient` | string | `"YOUR_GROUP@g.us"` | WhatsApp Group JID (`@g.us`), Channel (`@newsletter`), or phone number. |
| `niche` | string | `"..."` | Primary topic and industry vertical for the bot. |
| `audience` | string | `"..."` | Target group member profile (developers, founders, crypto traders, etc.). |
| `persona` | string | `"..."` | Tone, perspective, and voice of the AI community manager. |
| `contentArchetype` | string | `"AUTO_ROTATE"` | `'AUTO_ROTATE'` for daily rotation, or force: `'TACTICAL_PLAYBOOK'`, `'ARCHITECTURE_TEARDOWN'`, `'PRODUCTION_WAR_STORY'`, `'POLARIZING_DEBATE'`. |
| `contentDepth` | string | `"DEEP_INSIGHT"` | `'DEEP_INSIGHT'` (120–160 words with real depth) or `'SNACKABLE'` (75–100 words). |
| `priorityTopics` | array | `[...]` | Seed topics and discussion themes for the bot to prioritize. |
| `customRules` | string | `"..."` | Specific instructions for formatting, vocabulary, or taboo topics. |
| `enablePolls` | boolean | `true` | Set to `true` to deliver native interactive WhatsApp polls with evening posts. |
| `pollMultiSelect`| boolean | `false` | Set to `true` to allow group members to select multiple poll options. |

---

## 📄 License & Credits

Built with ❤️ for the **Wapio** developer community. Free to use, adapt, and deploy in any private or public WhatsApp community.
- Website: [wapio.io](https://wapio.io)
- Documentation: [docs.wapio.net](https://docs.wapio.net)
