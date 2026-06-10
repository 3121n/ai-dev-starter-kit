# Task template — Deploy & log it

Paste this into Claude Code when you've made changes and want them live + recorded.

---

I want to deploy my current changes and record the deploy in my workflow.

Please:
1. Show me a one-line summary of what's changed since the last commit (`git status` / `git diff --stat`).
2. Commit anything uncommitted with a clear message (ask me if the intent isn't obvious).
3. Push to `main`.
4. Confirm the Netlify build succeeds and give me the live URL. If it fails, diagnose against `docs/troubleshooting.md` §3 (Node version, publish dir, env vars).
5. Update my Notion deploy-tracker (or `MEMORY.md` if Notion isn't wired) with: date, what shipped, the live URL, and status.
6. Tell me in one line: what's live now, and what the obvious next step is.
