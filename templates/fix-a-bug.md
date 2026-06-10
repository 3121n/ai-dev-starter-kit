# Task template — Fix a bug

Paste this into Claude Code, filling in the blanks.

---

Something's broken. Help me fix it, following `CLAUDE.md` and the recovery playbook in `docs/troubleshooting.md`.

**What I expected to happen:**
> _..._

**What actually happens (exact error / screenshot description):**
> _paste the full error text, not just the first line_

**When it started / what I changed last:**
> _..._

Please:
1. First check the error against `docs/troubleshooting.md` — if it matches a known pattern, tell me which and apply that fix.
2. If it's new, find the root cause (read the actual error, not the scary first line) before changing anything.
3. Make the smallest fix that works. Explain what was wrong in one sentence.
4. Verify the fix locally, then commit, push, and confirm the deploy.
5. If this was a non-obvious gotcha, write it to memory so it's a 2-minute lookup next time.
