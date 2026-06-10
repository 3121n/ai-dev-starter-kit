# CLAUDE.md — AI Dev Starter Kit

This file is your project's persistent memory and rulebook. Claude Code reads it automatically at the start of every session. Edit it to match *your* project — the version below is the starter that ships with the kit.

> **Why this file matters:** A pre-written `CLAUDE.md` is the single highest-leverage setup step, and it's the one beginners skip. It's the difference between an AI that guesses about your project every session and one that already knows your stack, conventions, and where things live.

---

## Project

- **Name:** _your project name_
- **What it is:** _one sentence — what you're building and for whom_
- **Stack:** Static site (HTML/CSS/JS) → Netlify. Swap in your framework here when you add one.
- **Status:** _e.g. "MVP, pre-launch"_

## The connected workflow (how this project is wired)

This project follows the AI Dev Starter Kit pattern. The tools and their jobs:

| Tool | Job in this project |
|---|---|
| **Notion** | Planning + memory of record. Projects, session log, decisions, learnings, deploy status. |
| **GitHub** | Source of truth for code. Every change is a commit. `main` is always deployable. |
| **Netlify** | Where the site lives. Auto-deploys on push to `main` via `.github/workflows/deploy.yml`. |
| **Claude Code** | The agent that does the work, in the terminal, with this file as its memory. |
| **Memory (Pinecone/Mem0 MCP)** | Long-term recall across sessions. See "Memory" below. |

## Conventions (rules for Claude Code)

- **Match the surrounding code** — naming, style, structure. Read a file before editing it.
- **Secrets never get committed.** API keys live in `.env` (gitignored) and in Netlify's env-var settings — never in code, never in this file. If you spot a secret in a diff, stop and flag it.
- **Small commits, clear messages.** One logical change per commit. Present tense ("Add deploy workflow", not "Added").
- **`main` stays deployable.** If a change is risky, branch first.
- **Update the session log.** At the end of meaningful work, summarize what was done and what's next (in Notion, or in `SESSION-LOG.md` if you prefer files).
- **Windows-first.** This kit assumes Windows by default. MCP servers that shell out to `npx` need the `cmd /c` wrapper — see `docs/troubleshooting.md`.

## Memory

This project uses a memory MCP server for recall across sessions. The pattern mirrors a `MEMORY.md` index:

- One fact per memory, with a short description.
- Write a memory when you learn something non-obvious that future-you would waste time rediscovering (a fix, a decision, a gotcha).
- Don't store what the code or git history already records.

When memory is wired (the START-HERE instruction does this), Claude Code can recall and append to it. Until then, keep a plain `MEMORY.md` in the repo root.

## Environment

- **Node:** pinned in `.nvmrc` / `netlify.toml` — keep local and Netlify on the same major version (this is the #1 cause of "works locally, fails on deploy").
- **Required keys:** see `.env.example`. Each one is also documented in the Notion "API Keys" page (names only).

## What "done" looks like for a task

1. Code changed and matches conventions.
2. Runs locally.
3. Committed with a clear message.
4. Pushed → Netlify deploy is green.
5. Session log + any new memory updated.

---

*This file ships intentionally short. As your project grows, add the specifics Claude keeps getting wrong — that's exactly what `CLAUDE.md` is for.*
