# Setup guide — the one blessed path

This is the *only* path you need. No options, no decision paralysis. Follow it top to bottom. **Windows-first** (Mac/Linux notes inline). Total time: ~30–45 minutes the first time.

If anything breaks, don't improvise — jump to [`troubleshooting.md`](troubleshooting.md). Every common wall is in there with the exact fix.

---

## Before you start — the 5 accounts

You'll create these as you go. All have free tiers. **You make the accounts; the kit wires them together.** We never touch your keys — you paste them into your own `.env` and Netlify.

| # | Account | What it's for | Sign up |
|---|---|---|---|
| 1 | **GitHub** | Stores + versions your code | github.com |
| 2 | **Anthropic Console** | Powers Claude Code (⚠️ *separate* from claude.ai) | console.anthropic.com |
| 3 | **Netlify** | Puts your site on the internet | netlify.com |
| 4 | **Notion** | Planning + memory | notion.so |
| 5 | **Pinecone** *(optional)* | Long-term AI memory | pinecone.io |

---

## Step 1 — Tools on your machine

You need two things installed: **Node.js** and **Claude Code**.

```powershell
# Check if you already have Node (need v18+; this kit pins v20):
node --version

# If missing or too old, install from nodejs.org (LTS), then reopen your terminal.

# Install Claude Code:
npm install -g @anthropic-ai/claude-code
```

> **Windows trap:** if `claude` "is not recognized" after install, close and reopen your terminal (PATH didn't refresh). Still broken? See troubleshooting → *"command not recognized"*. **Do not run `npm` with `sudo`** — it causes permission errors later.

## Step 2 — Get your Anthropic API key (the #1 confusion)

1. Go to **console.anthropic.com** — this is a **different account** from the claude.ai chat you may already use.
2. Create an API key. **It's shown only once.** Copy it now (starts with `sk-ant-`).
3. You'll paste it in Step 5. Don't lose it — if you do, just make a new one.

## Step 3 — GitHub: clone your kit

1. On the kit's GitHub page, click the green **"Use this template" → Create a new repository**. Name it your project.
2. Clone it:
   ```powershell
   git clone https://github.com/YOUR-USERNAME/YOUR-REPO.git
   cd YOUR-REPO
   ```
3. **Auth:** the first push will ask you to sign in. The painless way:
   ```powershell
   npm install -g gh    # or install GitHub CLI from cli.github.com
   gh auth login        # choose HTTPS, follow the browser prompt
   ```
   > **The #1 beginner wall is GitHub auth.** Password login was removed in 2021 — if you see *"invalid username or password"*, you're hitting it. The `gh auth login` flow above sidesteps it entirely. Cached-credential weirdness? See troubleshooting → *GitHub auth*.

## Step 4 — Notion: duplicate the template and connect it

1. Open the [Notion template](#) link and click **Duplicate** (top right) to copy it into your workspace.
2. Create an integration at **notion.so/my-integrations** → **New integration** → copy the token (starts with `ntn_`).
3. **Share the page with the integration** — open your duplicated workspace page → **•••** menu → **Connections** → add your integration.
   > ⚠️ **This sharing step is the one everyone forgets.** The token alone does nothing. If you later see *"Could not find object"*, it's because a page wasn't shared. (troubleshooting → Notion)

## Step 5 — Fill in your secrets

```powershell
copy .env.example .env     # Mac/Linux: cp .env.example .env
```
Open `.env` and paste in the keys from Steps 2–4. **`.env` is gitignored — it will never be committed.** Never paste a key into any other file.

## Step 6 — Let Claude Code wire the rest

```powershell
claude
```
Then paste the entire contents of [`templates/START-HERE.md`](../templates/START-HERE.md) as your first message. It will check what's connected, set up your memory, connect Netlify, and run a real deploy test. Follow its prompts.

## Step 7 — Smoke test = you're done

The START-HERE instruction finishes by deploying `starter-site/` to Netlify. **When the live URL loads and says "If you can read this, it all works" — your entire chain is connected.** Record the URL in your Notion deploy-tracker (it'll offer to).

---

**Next:** open [`tools-explained.md`](tools-explained.md) if any of the *concepts* (terminal, API key, version control, deploy) still feel fuzzy. It's the plain-language primer.
