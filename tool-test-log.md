# Tool test log

Companion to [tools-roster.md](tools-roster.md) — a running record of tools we actually test-drive, one row per tool with a verdict.

**Verdict scale:** Keeper (clearly useful, will use again) · Solid (works as described) · Meh (works but limited value for us) · Skip (broken, dead, or wrong fit).

**Ruled out by environment** (CachyOS Linux — no macOS/iOS): Screen Studio, Screenize, Coucou, TetherKit, Tinycast. Noted here so we don't reconsider them later.

## Log

| Date | Tool | Category | What we tried | Verdict |
|---|---|---|---|---|
| 2026-11-03 | awwwards skill | UI | Full build of a launch site (fictional type foundry) using its whole method: concept system, art direction, engine, QA scripts, critic pass | **Keeper** |
| 2026-08 | World Labs Marble | 3D | Wedding-reference world generation | Tested pre-roster; favorite of the batch |
| 2026-08 | Mint | 3D | Wedding-reference 3D generation | Tested pre-roster |
| 2026-08 | Meshy | 3D | Wedding-reference 3D generation | Tested pre-roster |
| 2026-08 | SpAItial Echo 2 | 3D | Wedding-reference world generation | Tested pre-roster; judged weaker than Marble |

## Test notes

Detailed writeups go below, one section per test, newest last.

### Test 001 — awwwards skill (2026-11-03)

**Tool:** [juliobsz/skills](https://github.com/juliobsz/skills) `awwwards` (+ `jury` companion). Claim: builds
award-caliber websites from a one-line prompt, with a built-in QA loop and independent scoring.

**What we did:** gave it the one-line brief "launch page for Fumarole, a fictional independent type
foundry announcing its first typeface". Followed its full method: Phase 0–2 concept selection
(3 concepts from different territories, scored, 1 picked), art-direction system, DESIGN.md; Phase 3
build (Astro + GSAP + Lenis, its motion engine copied in, 3 pages: home, specimen, designed 404);
Phase 4 QA with its own scripts.

**How the QA loop performed (the tool's core promise):**
- `audit.mjs` caught a missing og:image (FAIL) → its `og.mjs` generated one → 0 FAIL / 0 WARN.
- `shoot.mjs --crawl --nav` (~90 screenshots + per-page reports: vitals, axe, keyboard, hover,
  reduced-motion) surfaced: header colliding with content, a logo link with no hover state,
  sub-24px tap targets, loader blowing the 2.6s budget on throttled mobile (4.8s), idle motion
  too subtle to register. All real bugs, all fixed across two passes.
- Independent critic subagent (its prescribed brief, evidence-only): read the concept correctly,
  scored Design 7 / Usability 7 / Creativity 7.5 / Content 7.5 = **7.15 weighted**
  ("Honorable-Mention territory") + a ranked 10-item fix list. All ten applied and verified in a
  final crawl: mobile blank-scroll posters, skinny intro columns, void hero, descender/caption
  collision, mid-word hero break, border clipping, header legibility, muted-ink contrast.
- Final state: throttled mobile LCP 0.9–2.3s (was 4.8), loader ≤2.3s, CLS ≤0.003, 58–60fps,
  0 axe violations, 0 console errors, skip-link first, no hover gaps.

**Verdict: Keeper.** This is not a component library, it is a full methodology that made the agent
build measurably better and catch real bugs a one-shot prompt would ship. Caveats: expensive in
tokens/time (critic pass alone was ~1.9M subagent tokens; each crawl ~8min); opinionated stack
(Astro/Next + GSAP + Lenis — fine for us); engine constants still need per-project tuning
(loader budget on throttled phones); `idleMotion` flag flip-flops between crawls depending on
where the detector samples (probe-verified the page breathes; the skill itself allows documenting
stillness as a decision). A second critic pass after the fix list would be its prescribed next
step if we wanted the site itself pushed toward 8+.

**Artifacts:** [../tool-tests/test-001-awwwards/site](../tool-tests/test-001-awwwards/site) —
`DESIGN.md` (concept + decisions + QA findings), `.shots/` (final screenshots + reports),
`.shots-audit.txt`, `dist/` (buildable). Reproduce locally: `npm run build`, serve `dist`, run the
scripts from a fresh clone of the repo.
