# START-HERE — paste this whole file into Claude Code

> **How to use:** Open your project in a terminal, run `claude`, and paste this *entire file* as your first message. Claude Code will run the wiring for you, asking for input only when it genuinely needs it. You do not need to understand every step — that's the point.

---

You are my setup agent for the **AI Dev Starter Kit**. Your job is to take me from "tools installed but nothing connected" to "a fully wired, tested AI dev workflow," doing the work yourself and only stopping to ask me for something when you truly cannot proceed without it (like pasting an API key into a file I own).

Work through these phases **in order**. After each phase, give me a one-line status (✅/⚠️/❌) before moving on. Be concise. If something fails, consult `docs/troubleshooting.md` in this repo, apply the matching fix, and tell me which pattern you used. Never print my secret values back to the screen.

## Phase 0 — Orient
- Read `CLAUDE.md`, `docs/setup-guide.md`, and `docs/troubleshooting.md` in this repo so you know the conventions and the known failure modes.
- Confirm the working directory is the cloned kit repo (you should see `netlify.toml`, `starter-site/`, `templates/`).

## Phase 1 — Check what's missing
Detect, and report a checklist of what's present vs missing:
- `node --version` (need ≥18; kit pins 20). 
- `git --version` and whether this repo has a GitHub remote (`git remote -v`).
- Whether `gh` is installed and authenticated (`gh auth status`).
- Whether `claude` (you) has a working Anthropic key (you're running, so yes).
- Whether `.env` exists and which **names** (not values) are filled in vs still placeholders. Never print values.
- Which MCP servers are configured.

For anything missing, tell me the *one* command or action to fix it, pulling exact steps from `docs/setup-guide.md`. Wait for me to confirm before continuing if a fix needs my action.

## Phase 2 — Wire the tools
- **Git/GitHub:** ensure `origin` points at my repo and a test `git push` works. If auth fails, walk me through `gh auth login` per troubleshooting §1.
- **MCP servers (the blessed 3–5, not more):** set up GitHub access, a memory server, and one docs-lookup server. On **Windows**, wrap `npx`-based servers with `cmd /c` (troubleshooting §4). Show me the final MCP config and confirm each server starts.
- **Notion:** verify my `NOTION_API_KEY` is set and that I've shared my workspace page with the integration (troubleshooting §5). Do a single read to confirm the connection; if it returns "Could not find object," tell me exactly which page to share.

## Phase 3 — Set up memory (the MEMORY.md pattern)
- If a memory MCP is wired, initialize it. Otherwise create a `MEMORY.md` at the repo root with this structure: a one-line index at top, then one fact per entry below (short description + the fact). 
- Seed it with 2–3 facts you've already learned about my setup during Phase 1 (e.g. my OS, my Node version, any gotcha we hit). One fact per entry. This shows me the pattern.
- Add a note in `CLAUDE.md`'s Memory section pointing at whichever store we ended up using.

## Phase 4 — End-to-end test (the proof)
- Make a trivial, visible edit to `starter-site/index.html` (e.g. add today's date in a comment or a small "set up by [me] on [date]" line).
- Commit it with a clear message and push to `main`.
- Deploy `starter-site/` to Netlify. Prefer connecting the repo in the Netlify UI (zero secrets) — if I haven't linked it, walk me through it; otherwise use the Netlify CLI. 
- Give me the **live URL** and confirm it loads the "If you can read this, it all works" page.
- If the deploy fails, diagnose against troubleshooting §3 (Node version, publish dir, env vars) and fix it.

## Phase 5 — "You're ready" report
Print a short report:
- ✅/❌ for each: Node, Git+GitHub, MCP servers (list them), Notion, Memory, Netlify deploy (with live URL).
- The 3 task templates available in `templates/` and how to use them.
- One suggested first real task to try, phrased as something I can paste back to you.
- Anything still on *me* to finish, as a short numbered list.

Then stop and wait for my next instruction. Do not start building my actual project until I tell you what it is.
