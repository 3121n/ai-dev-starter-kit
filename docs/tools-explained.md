# Tools explained (the concepts, in plain language)

Free tutorials assume you already know this stuff. That assumption is exactly where people get stuck. So here's the *why* behind each piece — no jargon. Read it once and the setup guide will make total sense.

---

## The terminal

A text window where you type commands instead of clicking buttons. Developers use it because it's faster and scriptable. On Windows, use **PowerShell** (search for it in the Start menu). When a guide says "run this," it means: type it into the terminal and press Enter.

You'll use maybe 6 commands ever. You don't need to "learn the terminal."

## Node.js

A program that lets your computer run JavaScript-based developer tools — including Claude Code. You install it once. That's it. The only thing to watch: the *version*. This kit pins **Node 20** everywhere so your machine and the cloud agree (mismatched versions are the #1 reason a site works locally but fails when deployed).

## API key

A long secret password that lets one program talk to another on your behalf. Claude Code uses an Anthropic API key to talk to Claude. Notion uses a token. **Treat every key like a credit-card number:**

- It goes in `.env` (which is gitignored — never uploaded) and in the host's settings panel.
- It **never** goes in your code, in `CLAUDE.md`, or in a chat message.
- If one ever leaks, *rotate* it (delete + make a new one). Bots scan public GitHub for keys within minutes — this is not paranoia, it's Tuesday.

> Two-account gotcha: **console.anthropic.com** (where API keys live) is a *different account* from **claude.ai** (the chat). Most "my key doesn't work" problems are really "I'm logged into the wrong one."

## Git & GitHub (version control)

**Git** is a time machine for your code: every time you "commit," it saves a snapshot you can return to. **GitHub** is the website that stores those snapshots online and lets you (and AI tools) collaborate.

- A **repo** (repository) is one project's folder + its full history.
- A **commit** is one saved change with a short message ("Add login page").
- **Push** = upload your commits to GitHub. **Pull** = download others'.
- `main` is the primary version. Keep it working — that's what gets deployed.

Why beginners stall here: GitHub removed password login in 2021, so the old "type your password" flow fails confusingly. The setup guide's `gh auth login` avoids it.

## Netlify (deploy / "going live")

**Deploying** means putting your site on the internet so anyone with the link can see it. Netlify watches your GitHub repo and, every time you push to `main`, automatically rebuilds and republishes your site. You get a public URL for free.

The thing to know: Netlify builds your site **in the cloud**, not on your machine. So it can't see your local `.env` — you have to add your environment variables in Netlify's own settings panel too. "Works locally, fails on deploy" is almost always a missing env var or a Node-version mismatch.

## Claude Code

An AI agent that lives in your terminal, inside your project. Unlike a chat tab, it can read and edit your actual files, run commands, and make commits. It reads `CLAUDE.md` automatically so it knows your project's rules and stack every session.

This is the leap from "ChatGPT in a browser tab" to a *connected* workflow: the AI is no longer guessing about your project from pasted snippets — it's working *in* it.

## MCP (Model Context Protocol)

The standard that lets Claude Code plug into other tools — GitHub, Notion, a memory store — as "servers" it can call. You don't need to understand the protocol. You need to know: **install 3–5 servers, not 30.** More than that bloats the AI's context and makes it *worse* at picking the right tool. This kit's blessed set: GitHub + your memory store + one docs lookup. The START-HERE instruction sets them up.

## Memory (Pinecone / Mem0)

By default, Claude Code forgets everything between sessions. A memory server fixes that: it stores facts ("we chose X because Y", "this bug's fix is Z") and lets the AI recall them later. The pattern in this kit mirrors a simple `MEMORY.md` index — one fact per entry, short description, written only when it's something future-you would waste time rediscovering.

One gotcha if you wire Pinecone yourself: the index's **dimension** must match your embedding model (often 1536). Wrong number = silent failures. The kit's default config gets this right.

---

That's the whole mental model. Five tools, each with one job, wired so the AI can move work between them. Back to [`setup-guide.md`](setup-guide.md).
