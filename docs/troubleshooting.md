# 🆘 The "stuck at 98%" recovery playbook

This is the part of the kit you'll come back to. Every wall below is one that beginners *predictably* hit when wiring these tools together. Each entry is: **the symptom you actually see → why it happens → the exact fix.** Treat it as a 2-minute lookup, not a 4-hour rabbit hole.

> 🔎 **How to use this:** Ctrl-F the error message you're seeing. If it's not here, paste the exact error into Claude Code and say "check this against the kit's troubleshooting patterns."

---

## 1. GitHub authentication (the #1 wall)

**Symptom:** `remote: Invalid username or password` / `Authentication failed` when you `git push`.
**Why:** GitHub removed plain password auth in August 2021. Typing your account password no longer works.
**Fix:**
```powershell
npm install -g gh           # or install GitHub CLI from cli.github.com
gh auth login               # choose GitHub.com → HTTPS → login with browser
```

**Symptom:** It *still* fails after `gh auth login`, or uses an old account.
**Why:** Windows cached a stale credential that survives reinstalls.
**Fix:** Open **Credential Manager** (Windows search) → **Windows Credentials** → delete anything under `git:https://github.com`, then push again. (Mac: Keychain Access → delete `github.com` internet password.)

**Symptom:** Push works but the Action/permissions fail.
**Why:** Token is missing a scope.
**Fix:** Re-run `gh auth login` and ensure `repo` and `workflow` scopes are granted, or regenerate a PAT at github.com/settings/tokens with those boxes checked.

---

## 2. Environment variables & secrets across the cloud boundary

**Symptom:** Site works locally, but the deployed version is blank / 500 / "missing API key."
**Why:** Netlify builds in the cloud and **cannot see your local `.env`**. Local-only env vars don't exist in the deploy.
**Fix:** Add each variable in **Netlify → Site configuration → Environment variables**, with scope **"Builds"** (and "Runtime" if your functions need it). Redeploy.

**Symptom:** You accidentally committed `.env` with real keys.
**Why:** `.gitignore` only ignores files that aren't *already* tracked — and it does **not** scrub history.
**Fix:** (1) **Rotate the keys immediately** (delete + regenerate at each provider — assume they're compromised; bots find committed keys within minutes). (2) `git rm --cached .env && git commit -m "Remove .env"`. (3) For full history scrubbing, use `git filter-repo` or GitHub's secret-scanning guidance.

**Symptom:** A Netlify env var named `SITE_NAME` (or similar) silently doesn't apply.
**Why:** Some names are **reserved** by Netlify and ignored on import.
**Fix:** Rename it (e.g. `APP_SITE_NAME`) and reference the new name.

---

## 3. Netlify "works locally, fails on deploy"

**Symptom:** Build fails in Netlify with errors you never saw locally.
**Causes & fixes:**
- **Node version mismatch** → the most common one. Pin it: `NODE_VERSION = "20"` in `netlify.toml` + `.nvmrc` (both shipped in this kit). Match your local `node --version`.
- **Warnings treated as fatal** → Netlify sets `CI=true`, which makes some toolchains fail on warnings. Fix the warning, or set the build to not treat warnings as errors.
- **Wrong publish directory** → `publish` in `netlify.toml` must point at your output folder (`starter-site` for the kit; `dist`/`build` once you add a framework).
- **Missing `netlify.toml`** → if Netlify can't find build settings it guesses wrong. The kit ships one; keep it at the repo root.

---

## 4. Claude Code / MCP on Windows

**Symptom:** An MCP server fails to start with `spawn npx ENOENT`.
**Why:** On Windows, Node can't directly spawn `npx`. This is the single most-reported MCP bug across Claude Code, Cursor, Cline, and Windsurf.
**Fix:** Wrap the command with `cmd /c` in your MCP config, e.g.:
```json
{ "command": "cmd", "args": ["/c", "npx", "-y", "@some/mcp-server"] }
```

**Symptom:** `claude` "is not recognized as a command."
**Why:** PATH didn't refresh after install, or npm's global bin isn't on PATH.
**Fix:** Close and reopen your terminal. Still broken? Run `npm config get prefix`, and make sure that folder's path is in your PATH environment variable.

**Symptom:** Permission errors (`EACCES`, root-owned files) during `npm install -g`.
**Why:** You ran npm with `sudo`/admin earlier and npm's directory is now root-owned.
**Fix:** Don't use `sudo` with npm. Reset ownership of `~/.npm` and the global prefix, or reinstall Node via a version manager.

**Symptom:** Claude Code stops mid-task / "context full."
**Why:** Too many MCP servers / large tool outputs saturate the context window.
**Fix:** Keep to 3–5 MCP servers. Start a fresh session for a new task. Ask Claude to checkpoint progress to memory before context fills.

---

## 5. Notion API

**Symptom:** `Could not find object` / `object_not_found` even though the page clearly exists.
**Why:** You created the integration token but never **shared the page** with the integration. The token and the sharing are independent.
**Fix:** Open the page → **•••** → **Connections** → add your integration. Do this on the top-level page; child pages inherit it.

**Symptom:** `API token is invalid`.
**Why:** Wrong token, or you copied the integration's *ID* instead of its *secret* (which starts with `ntn_`).
**Fix:** Regenerate the secret at notion.so/my-integrations and paste the `ntn_...` value.

---

## 6. Memory / Pinecone

**Symptom:** Memory writes "succeed" but recall returns nothing, or you get a dimension error.
**Why:** The index's **dimension** doesn't match your embedding model (e.g. index is 768 but the model outputs 1536).
**Fix:** Recreate the index with the dimension your embedding model produces (commonly **1536**). Confirm the metric (usually `cosine`) and region too.

**Symptom:** Memory MCP can't authenticate.
**Why:** `PINECONE_API_KEY` missing/typo, or the index name in your config doesn't match the one in the Pinecone console.
**Fix:** Re-check `.env` against app.pinecone.io. Names are case-sensitive.

---

## When all else fails

1. Read the **actual** error, not the first scary line — the real cause is usually 2–3 lines down.
2. Paste the full error into Claude Code: *"Here's the error. Check it against the kit's troubleshooting playbook and tell me which pattern it matches."*
3. Still stuck after 20 minutes? That's a real bug, not a you-problem. Email **hei@andersjohansen.no** with the error and what you tried — fixes that aren't here yet get added (and you get credited).
