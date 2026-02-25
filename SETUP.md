# n8n Local Setup Guide

> Everything you need installed and running before the workshop starts.

## What You'll Need

| Component | Why |
|-----------|-----|
| **n8n** | The workflow automation platform we'll build in all day |
| **Gemini API key** | Free AI model we'll use in Lesson 2 and the workshop |
| **Browser** | Chrome or Firefox (latest version) |
| **Internet access** | For API calls, downloading templates, and MCP connections |

## Step 1: Install n8n

Pick **one** of the two options below. Docker is recommended if you already have Docker installed. npx is the fastest if you don't.

### Option A: npx (no install needed)

You need Node.js 18+ installed. Check with:

```bash
node --version
```

If you have Node.js, run n8n directly:

```bash
npx n8n
```

That's it. n8n will download and start. Open your browser to **http://localhost:5678**.

### Option B: Docker

```bash
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v n8n_data:/home/node/.n8n \
  n8nio/n8n
```

Open your browser to **http://localhost:5678**.

> The `-v n8n_data:/home/node/.n8n` flag keeps your data between restarts. If you stop and restart the container, your workflows and credentials will still be there.

### First Launch

When you open n8n for the first time, it will ask you to create an owner account. Pick any email and password — this is local only, nothing is sent anywhere.

## Step 2: Verify n8n is Running

Open **http://localhost:5678** in your browser. You should see the n8n editor with an empty canvas.

From the terminal, you can also verify with:

```bash
curl -s http://localhost:5678/healthz
```

You should get back `{ "status": "ok" }`.

## Step 3: Get a Gemini API Key (Free)

We use Google's Gemini model for the AI exercises. It has a generous free tier.

1. Go to **https://aistudio.google.com/apikey**
2. Sign in with your Google account
3. Click **"Create API Key"**
4. Copy the key somewhere safe — you'll need it in the next step

> If you already have an xAI (Grok) API key, that works too. Any model that n8n supports is fine.

## Step 4: Add the API Key to n8n

1. In n8n, click the **three dots menu** (top right) or go to **Settings** (gear icon in the left sidebar)
2. Click **Credentials**
3. Click **Add Credential**
4. Search for **"Google Gemini"** (or "Google PaLM" depending on your n8n version)
5. Paste your API key
6. Click **Save**

To verify the credential works:

1. Create a new workflow
2. Add a **Basic LLM Chain** node (or **AI Agent** node)
3. In the model dropdown, select your Gemini credential
4. Set the prompt to something simple like `Say hello`
5. Click **Test step** — you should get a response back

## Step 5: Download the Exercise Files

The workshop includes pre-built workflow files that you'll import into n8n.

Download these three files from the `workflows/` folder:

- [`exercise_1_data_flow.json`](workflows/exercise_1_data_flow.json)
- [`exercise_2_code_debugging.json`](workflows/exercise_2_code_debugging.json)
- [`exercise_4_llm_debugging.json`](workflows/exercise_4_llm_debugging.json)

To import a workflow file into n8n:

1. Open n8n and create a new workflow
2. Click the **three dots menu** (top right of the canvas)
3. Click **Import from file** and select the JSON file
4. The workflow appears on your canvas

## Troubleshooting

### n8n won't start (port already in use)

Something else is using port 5678. Either stop that process or run n8n on a different port:

```bash
# npx
N8N_PORT=5679 npx n8n

# Docker
docker run -it --rm --name n8n -p 5679:5678 -v n8n_data:/home/node/.n8n n8nio/n8n
```

Then open **http://localhost:5679** instead.

### npx n8n hangs or fails

Try installing n8n globally instead:

```bash
npm install -g n8n
n8n start
```

### Docker container exits immediately

Make sure Docker Desktop is running. On Mac, check the Docker icon in the menu bar. On Windows, make sure Docker Desktop is open.

### "Cannot find module" errors

Your Node.js version might be too old. n8n requires Node.js 18 or newer:

```bash
node --version
# Should be v18.x.x or higher
```

### Gemini API key doesn't work

Verify the key directly:

```bash
curl -s "https://generativelanguage.googleapis.com/v1/models?key=YOUR_KEY_HERE" | head -5
```

If you get an error, the key might not be activated yet. Go back to https://aistudio.google.com/apikey and make sure the key is listed as active.

### AI nodes don't show up in n8n

You might be on an older n8n version. The AI/LangChain nodes require n8n 1.19+. Check your version:

```bash
npx n8n --version
```

If it's older than 1.19, update:

```bash
npm install -g n8n@latest
```

## Quick Verification Checklist

Before the workshop, make sure all of these pass:

- [ ] n8n opens at http://localhost:5678
- [ ] You can create a new workflow and add nodes
- [ ] Your Gemini (or Grok) API key is saved as a credential in n8n
- [ ] You can run a Basic LLM Chain node and get a response
- [ ] You have the 3 exercise JSON files downloaded
