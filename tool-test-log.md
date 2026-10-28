# Tool test log

Companion to [tools-roster.md](tools-roster.md) — a running record of tools we actually test-drive, one row per tool with a verdict.

**Verdict scale:** Keeper (clearly useful, will use again) · Solid (works as described) · Meh (works but limited value for us) · Skip (broken, dead, or wrong fit).

**Ruled out by environment** (CachyOS Linux — no macOS/iOS): Screen Studio, Screenize, Coucou, TetherKit, Tinycast. Noted here so we don't reconsider them later.

## Log

| Date | Tool | Category | What we tried | Verdict |
|---|---|---|---|---|
| 2026-08 | World Labs Marble | 3D | Wedding-reference world generation | Tested pre-roster; favorite of the batch |
| 2026-08 | Mint | 3D | Wedding-reference 3D generation | Tested pre-roster |
| 2026-08 | Meshy | 3D | Wedding-reference 3D generation | Tested pre-roster |
| 2026-08 | SpAItial Echo 2 | 3D | Wedding-reference world generation | Tested pre-roster; judged weaker than Marble |

## Test notes

Detailed writeups go below, one section per test, newest last.

### Test 001 — awwwards skill (paused mid-test, 2026-10-24)

**Tool:** awwwards skill — `juliobsz/skills` (repo also contains a `jury` scoring skill).
**Claim:** builds award-caliber websites from a one-line prompt, with a QA loop of its own
scripts (screenshots, vitals, axe, keyboard/hover checks) and a fresh "critic" subagent pass.
**Status: in progress — built and building-clean, QA loop not yet run. Paused at Asim's request.**

- Brief given: launch page for Fumarole, a fictional type foundry announcing its first
  typeface ("Vent Grotesk"). Picked concept: "the site is the typeface's design space" —
  scroll drives the wght axis, pointer drives wdth, live mono readout reports coordinates.
- Project: [../tool-tests/test-001-awwwards/site](../tool-tests/test-001-awwwards/site) — Astro +
  GSAP + Lenis, engine copied from the skill's references (GL module stripped), DESIGN.md
  written per the skill's Phase 0–2. Pages: home, /specimen, designed 404 (.notdef tofu box).
- `npm run build` passes: 3 pages + sitemap. Production server + `shoot.mjs --crawl --nav`
  was about to run when we stopped. No screenshots taken yet; no verdict yet.

**To resume:**
1. `git clone --depth 1 https://github.com/juliobsz/skills /tmp/juliobsz-skills`
2. `cd ~/Documents/tool-tests/test-001-awwwards/site`
3. `node /tmp/juliobsz-skills/awwwards/scripts/serve.mjs dist --port 4400 &`
4. `node /tmp/juliobsz-skills/awwwards/scripts/shoot.mjs http://127.0.0.1:4400/ --crawl --nav`
5. `node /tmp/juliobsz-skills/awwwards/scripts/audit.mjs http://127.0.0.1:4400/ > .shots/audit.txt`
6. Read `.shots/` screenshots + reports, run the critic-brief subagent pass, fix, log verdict.

If we decide to keep the tool after the verdict, install it permanently (e.g. copy the
`awwwards/` + `jury/` dirs into `~/.agents/skills/`).
