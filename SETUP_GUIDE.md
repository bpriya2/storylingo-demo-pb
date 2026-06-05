# StoryLingo Demo: Complete Setup Guide

Deploying an app to the internet can be intimidating. There are so many moving parts — accounts, API keys, servers, environment variables — and if you've never done it before, the whole thing feels like it was designed for someone else. Someone more technical. Someone who *already knows* how this works.

I get it. I've been there. And here's the truth — it's way less scary than it looks.

By the end of this guide, you'll have a **working voice AI app** — one that tells kids fairy tales in real-time using OpenAI's voice models — **live on the internet**, deployed to your own backend. You configured it. You deployed it. That counts.

Here are the questions this guide will answer:

- What accounts do I need, and how do I set them up?
- How do I get my own copy of the code?
- How do I connect the **frontend** to the **backend**?
- How do I actually get this thing **live on the internet**?
- What do I do when things break? (*Because they will.*)

Don't worry if you've never opened a terminal before. We'll go step by step.

Here's what we'll cover:

1. Set up your accounts (Claude Code, OpenAI, GitHub, Railway)
2. Fork the repo and clone the code from GitHub
3. Preview the app UI locally
4. Set up the backend for your app
5. Get your OpenAI API key
6. Create a Prompt ID for your voice agent
7. Create a backend on Railway
8. Connect your environments together
9. Deploy and vibe code any issues
10. (Advanced) Add a feature and create a Pull Request

We have a lot to talk about, let's get into it.

---

## Before You Start: The Full Picture

Here's everything you're going to set up. It looks like a lot, but each one takes **5-10 minutes**.

| Tool | What it does | Cost |
|------|-------------|------|
| **Claude Code** | Your AI coding assistant — it'll help you **run commands** and **edit files** | Free tier available |
| **GitHub** | Where the app's code lives — think of it as **Google Drive for code** | Free |
| **OpenAI** | Powers the **voice AI storytelling** — the "brain" behind the app | Pay-as-you-go (~$5 credit) |
| **Railway** | Hosts your app on the internet — it's like **renting a computer in the cloud** | Free tier available ($5/month after) |

Total setup time: **30-60 minutes**.

---

## Step 1: Set Up Your Claude Code Account, OpenAI API, and Railway (Backend)

Before we touch any code, you need to create accounts on the platforms we'll be using.

We've put together a **step-by-step checklist** that walks you through creating each account. Follow it, then come back here:

**[StoryLingo Demo — Accounts Setup Checklist](https://www.notion.so/productacademy/StoryLingo-Demo-Accounts-Setup-Checklist-993e23fb1fac453396921afa9f8502b3)**

That checklist covers:

- **Claude Code** — your AI coding assistant
- **GitHub** — where the code lives
- **OpenAI** — the voice AI brain
- **Railway** — your cloud backend

Once all four accounts are created, come back here and continue to Step 2.

> **Tip:** You don't need to configure anything yet — just create the accounts. We'll set everything up in the steps that follow.

---

## Step 2: Fork the Repo and Clone the Code from GitHub

Before you can work on the code, you need **your own copy**.

In GitHub, that's called a **fork**. It creates a copy of the project under your own account that you can change freely. This is important — without a fork, you won't be able to **push changes** or **auto-deploy** later.

### Fork the repo

1. Go to [github.com/deewang/storylingo-demo](https://github.com/deewang/storylingo-demo)
2. Click the **Fork** button in the top-right corner
3. Select your GitHub account as the destination
4. Wait a few seconds — you'll be taken to your fork (it'll be at `github.com/YOUR-USERNAME/storylingo-demo`)

> **Why fork?** Forking gives you your own copy of the code that you can **push changes** to. You'll need this for Railway to **auto-deploy** when you make changes, and for creating **Pull Requests** later.

### Clone your fork

Now download your fork to your computer.

**Using Claude Code (recommended):**

Open Claude Code and type:

> "Clone the repo from https://github.com/YOUR-USERNAME/storylingo-demo"

Replace `YOUR-USERNAME` with your actual GitHub username.

**Using the terminal manually:**

```bash
git clone https://github.com/YOUR-USERNAME/storylingo-demo.git
cd storylingo-demo
```

The first command downloads the code. The second command moves you into the project folder.

**How do I know it worked?** You should see a new folder called `storylingo-demo` with files like `package.json`, `README.md`, and a `server/` folder inside it.

> **What's a repo?** Short for "repository." It's just a folder of code that's tracked by Git. Every time someone makes a change, Git remembers it — like **version history in Google Docs**.

---

## Step 3: Preview the App UI Locally

Before setting up any API keys or backends, let's see the app actually running on your machine. This gives you a feel for what you're deploying — and it's a great way to check that the code downloaded correctly.

### Ask Claude to run it

Open Claude Code inside the `storylingo-demo` folder and type:

> "Can you run a preview of this app"

Claude will **install the dependencies** and **start the development server** for you. After a minute or so you'll see the app open in your browser (or Claude will give you a link like `http://localhost:8081`).

### What you should see

You'll land on the **StoryLingo home screen** — an owl mascot, the app name, and a **Start** button. Tap Start and you'll reach the **Story Selection screen**, where you can browse the available stories and language options.

> **Note:** The voice AI features won't work yet — those need your **OpenAI keys** from the next steps. But the whole UI will be there and navigable.

### How to stop the server

When you're done previewing, go back to Claude Code and type:

> "Stop the dev server"

Or just close the terminal window.

---

## Step 4: Set Up the Backend for Your App

Now it's time to set up the pieces your app needs to actually work — the **API keys** and **configuration** that power the voice AI and connect your frontend to the backend.

Your app needs **three pieces of information** to run. These are stored in a file called `.env` (short for "environment variables") — think of it as a **settings file that holds your secrets**.

### Create your .env file

Open Claude Code in the `storylingo-demo` folder and type:

> "Create a .env file from the .env.example template"

Or run this in your terminal:

```bash
cp .env.example .env
```

### What's inside

Open the `.env` file and you'll see three variables you need to fill in:

```
OPENAI_API_KEY=sk-your-openai-api-key-here
OPENAI_PROMPT_ID=pmpt_your-prompt-id-here
EXPO_PUBLIC_DOMAIN=your-app.up.railway.app
```

| Variable | What it is | Where you'll get it |
|----------|-----------|---------------------|
| `OPENAI_API_KEY` | Your OpenAI API key | Step 5 |
| `OPENAI_PROMPT_ID` | Your stored prompt ID | Step 6 |
| `EXPO_PUBLIC_DOMAIN` | Your Railway app's URL | Step 7 |

Don't worry about filling these in yet — the next three steps will walk you through getting each value.

> **What's a .env file?** It's a simple text file that holds your **environment variables** locally. It's already in `.gitignore`, which means Git will never upload it to GitHub. Your secrets stay on your machine.

---

## Step 5: Get Your OpenAI API Key

Your **API key** is like a password that lets your app talk to OpenAI's voice AI. Here's how to get it:

1. Go to [platform.openai.com/api-keys](https://platform.openai.com/api-keys)
2. Click **Create new secret key**
3. Give it a name like "StoryLingo Demo"
4. **Copy the key immediately** — you won't be able to see it again after you close the dialog
5. Save it somewhere safe (a notes app, password manager, etc.)

Your key will look something like: `sk-proj-abc123xyz...`

> **Important:** This key is like a password. Don't share it, don't paste it in public chats, and don't commit it to GitHub. Anyone with your key can use your **OpenAI credits**.

### Verify Realtime API access

The app uses OpenAI's **Realtime API** for voice conversations. This is a newer feature and your account needs access to it.

1. Go to [platform.openai.com/settings/organization/limits](https://platform.openai.com/settings/organization/limits)
2. Look for `gpt-realtime` or `gpt-4o-realtime` in the model list
3. If you see it, you're good. If not, you may need to **add more credit** or wait for access to be enabled on your account

Save your API key — you'll add it to your `.env` file and Railway in Step 8.

---

## Step 6: Create a Prompt ID for Your Voice Agent

The **stored prompt** is a set of instructions saved in your OpenAI account that tells the AI how to behave as a storyteller. Your app references this prompt by its **ID**.

Here is what to do:

1. Go to [platform.openai.com/prompts](https://platform.openai.com/prompts)
2. Click **Create prompt**
3. Open the file `prompts/storyteller.md` in the repo you cloned — you can find it in your code editor or [view it on GitHub](https://github.com/deewang/storylingo-demo/blob/main/prompts/storyteller.md)
4. Copy everything in the **Prompt Template** section (the part inside the code block)
5. Paste it into the OpenAI prompt editor
6. Make sure the editor recognises the three **template variables**:
   - `{{story_title}}`
   - `{{story_context}}`
   - `{{story_beats}}`
7. Click **Save**
8. Copy the **Prompt ID** — it starts with `pmpt_` (e.g., `pmpt_abc123def456...`)

Save this Prompt ID alongside your API key — you'll need both in Step 8.

---

## Step 7: Create a Backend for Your App (e.g. Railway)

Now it's time to set up your backend.

The way you do this is you go to **Railway**, which is what we selected for the app. If you're creating a custom app, you can ask Claude or your coding agents to figure out what is the best backend for your service. But in this case, we will be using Railway.

### Create a new project in Railway

1. Go to [railway.app](https://railway.app) and sign in
2. Click **New** on the top right hand corner
3. Connect your GitHub repository that you have **forked** from our code
4. Once you have connected the GitHub repository, you will be able to **automatically deploy via GitHub** into Railway any time you make a change

### Generate your domain

5. Then go to **Settings** and scroll down to **Networking** > **Create a Custom Domain**
6. Railway will create a URL like `storylingo-demo-production.up.railway.app`
7. **Copy and paste that domain** into your notepad or somewhere safe — we'll need this later

> Don't worry if the first deploy fails — we haven't set up the environment variables yet. That's the next step.

---

## Step 8: Connect Your Environments Together

Now you have everything. It's time to **connect all the pieces**.

### Add environment variables to Railway

Go to Railway, and add these items in your **Environment Variables**:

1. Your **OpenAI API Key** (`OPENAI_API_KEY`)
2. Your **Voice Agent's Prompt ID** (`OPENAI_PROMPT_ID`)
3. The **Domain** where your app is hosted (`EXPO_PUBLIC_DOMAIN`)

In your Railway service, go to the **Variables** tab and click **New Variable** for each one:

```
OPENAI_API_KEY = sk-proj-your-actual-key-here
OPENAI_PROMPT_ID = pmpt_your-actual-prompt-id-here
EXPO_PUBLIC_DOMAIN = storylingo-demo-production.up.railway.app
```

> **Why does EXPO_PUBLIC_DOMAIN matter?** This tells the frontend where to find the backend API. It gets **baked into the app when it builds** — which is why you need to set it *before* the app builds. If you set it after, you'll need to trigger a redeploy.

### Also update your local .env file

Go back to your `.env` file and add in the same information. This is needed if you want to **test locally without waiting for a deploy** every time (faster development).

Open the `.env` file in your project and fill in the real values:

```
OPENAI_API_KEY=sk-proj-your-actual-key-here
OPENAI_PROMPT_ID=pmpt_your-actual-prompt-id-here
EXPO_PUBLIC_DOMAIN=localhost:5000
```

> **Note:** For local development, set `EXPO_PUBLIC_DOMAIN` to `localhost:5000` instead of your Railway domain.

---

## Step 9: Deploy the Changes and Vibe Code Any Issues in Claude or VS Code

Now the real **vibe coding** really begins.

You have now set up the **front end** and **backend** for your services, and the rest is about **iterating on issues** and asking the agent to fix it for you.

From my experience, here's what to expect:

1. **Your deployment will always fail the first time.** There are always settings missed, configurations that need to be done, packages that need to be downscaled, etc. This is completely normal — don't freak out.

2. **Your job now is to vibe with the coding agent** and troubleshoot all the issues until you have deployed your app in Railway using a production URL.

3. Open Claude Code inside your `storylingo-demo` folder and describe what you're seeing — it can read the error logs and help you debug.

### Common things that go wrong

| What you see | What's probably wrong | How to fix it |
|-------------|----------------------|--------------|
| Build fails on Railway | `EXPO_PUBLIC_DOMAIN` wasn't set before the build ran | Set the variable and trigger a **redeploy** |
| App loads but voice doesn't work | Missing prompt ID or API key | Check that both `OPENAI_API_KEY` and `OPENAI_PROMPT_ID` are set in Railway **Variables** |
| "OPENAI_PROMPT_ID not set" error | Prompt ID missing | Go back to **Step 6** and create the stored prompt |
| 401 error from OpenAI | Invalid API key | Double-check your API key — it may have expired or been revoked |
| Microphone not detected | Browser permissions | Click the **lock icon** in your browser's address bar and allow microphone access |
| App loads but shows URL errors | `EXPO_PUBLIC_DOMAIN` mismatch | Make sure the domain matches your Railway URL exactly (no `https://`, no trailing slash). Redeploy after fixing |

### How do I know it's working?

1. Open your Railway URL in a browser (e.g., `https://storylingo-demo-production.up.railway.app`)
2. You should see the **StoryLingo app** load
3. Pick a story — try "Snow White" to start
4. Select a language (English is the default)
5. Start the session and **grant microphone access** when your browser asks
6. Talk to the storyteller — it should respond with a warm voice telling you the story

> **Use Chrome.** The voice features work best in Google Chrome. Safari and Firefox may have issues with the WebRTC audio.

---

## Advanced Step 10: Adding a Feature to the Product as a Pull Request

Once you have the app working, you can now start **adding features to the product**.

Here's an example. Go to Claude Code and ask it to add in a new story for **"The Little Mermaid"** for StoryLingo.

### Use Plan Mode

Go to Claude Code and select **Plan Mode**. In Plan Mode, that's where you can **plan for your feature developments without building code**.

1. Go to Plan Mode and ask it to plan out how **Little Mermaid** would work as a new story for StoryLingo
2. Give it a description of what you want the feature to be
3. Ask it to **come up with a plan first** before it codes
4. **Test the feature locally** first before you deploy the changes
5. Afterwards, let it vibe code the feature and create a **Pull Request** for the main repo

> **What's a Pull Request?** A PR is how you propose changes to someone else's codebase. It lets the repo owner **review your changes** before merging them in. When you fork a repo and make improvements, a PR is how you contribute those improvements back.

---

## Wrapping Up

Look at what you just did:

1. **Created accounts** on Claude Code, GitHub, OpenAI, and Railway
2. **Forked and cloned** a real codebase to your own GitHub
3. **Previewed the app locally** before touching any credentials
4. **Set up your backend** with environment variables
5. **Got an OpenAI API key** and verified Realtime API access
6. **Created a stored voice prompt** to power the storytelling AI
7. **Created a Railway project** and connected it to your GitHub fork
8. **Connected everything together** — env vars in Railway and locally
9. **Deployed and debugged** a full-stack app to a live URL
10. **Tested a voice AI feature** end-to-end

That's a real deployment workflow. The same pattern — **fork code, configure secrets, deploy to a cloud provider, iterate** — is how production apps ship at real companies every day.

The tools change (Railway vs. AWS vs. Vercel), the apps change (StoryLingo vs. your future product), but the pattern is the same. And now you've done it yourself.

Hopefully, this guide has been useful for you. If you do the above well, you'll be deploying your own AI-powered products in no time.

---

## Quick Reference

When you need to remember the key pieces:

| What | Where |
|------|-------|
| Account setup checklist | [Notion checklist](https://www.notion.so/productacademy/StoryLingo-Demo-Accounts-Setup-Checklist-993e23fb1fac453396921afa9f8502b3) |
| Your app URL | Railway dashboard > Settings > Networking |
| Your API key | [platform.openai.com/api-keys](https://platform.openai.com/api-keys) |
| Your prompt ID | [platform.openai.com/prompts](https://platform.openai.com/prompts) |
| Env vars on Railway | Railway dashboard > Variables tab |
| Env vars locally | `.env` file in your project |
| Prompt template | `prompts/storyteller.md` in this repo |
| Env var template | `.env.example` in this repo |
| Deployment logs | Railway dashboard > Deployments tab |
