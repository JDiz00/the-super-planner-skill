# The Super Planner Skill for Claude Code

**A drop-in Claude Code skill that turns "build this" into a real plan — then guides Claude Code through a verify/repair loop.** One command. It sizes the job itself, researches from the right source, locks a checkable goal, plans, builds after you approve, and is told not to report "done" without walking every success criterion for a passing check.

> Built: 2026-07-01 · Free · MIT

Most planning prompts are a giant wall of text that treats a one-line copy fix the same as a new payment integration. This skill doesn't. It classifies the task into **Quick / Standard / Deep** first, then runs *only* the ceremony that tier needs — so a small change gets a one-screen plan and a real build gets the full research → refine → goal-contract → plan → execute → verify loop.

This is the planner I actually run on my own projects. It's opt-in only — it fires only when you type `/jd-super-plan`, never on a casual question.

---

## What it does

- **Sizes the task before anything else.** Tier 1 (Quick) skips straight to a mini-plan and one approval. Tier 2 (Standard) runs the light path with a single combined approval gate. Tier 3 (Deep) runs the full pipeline for new projects, unfamiliar APIs, or anything touching auth / payments / third-party data.
- **Routes research by claim type.** Technical/pricing/security facts → official docs (never a social post). Market/pain-point signal → a recent-activity search, treated as trend signal not truth. A citation-grade question → a deep-research pass. And it checks your dependency files first so it verifies against *your* versions, not the latest.
- **Locks a Goal Contract** with numbered, individually checkable success criteria and a runnable definition of done — then makes every implementation slice trace back to one of those criteria.
- **Won't wave "done" through.** The final review walks every success criterion and requires a passing check for each. A criterion with no evidence is not done. (This is instruction, not enforcement — it tells Claude to prove each one; the enforced version is below.)
- **Never invents.** No API, command, path, price, model id, or flag from memory — it verifies or says it can't. A stated gap beats a confident guess.

## Install

Copy the skill folder into your Claude Code skills directory:

```bash
mkdir -p ~/.claude/skills/jd-super-plan && cp -R skill/jd-super-plan/. ~/.claude/skills/jd-super-plan/
```

That's it — no dependencies, nothing to build. Claude Code picks it up on the next session.

## Use

In Claude Code:

```text
/jd-super-plan build a rate limiter for my API
```

It states the tier it picked, then runs that tier. You approve at the gate; it builds and verifies. Run `/jd-super-plan` with no arguments and it'll ask for the idea in one line. (It's opt-in only, so plain "help me plan X" won't trigger it — use the `/jd-super-plan` command.)

## What's in here

```text
skill/jd-super-plan/SKILL.md   the skill (the whole thing is one file)
START-HERE.html                a friendly walkthrough — open in any browser
README.md / README.txt         docs (Markdown + plain text)
VERSION.txt                    version + contents manifest
LICENSE                        MIT
```

## Requirements

- Claude Code (the skill uses its `/command` + skill loader).
- Nothing else. The skill is a single Markdown file — no packages, no runtime.

Some phases mention optional tools (a docs-fetch tool, a web-search tool, a deep-research pass, a brainstorming skill). If a tool is unavailable, the skill uses a direct source when it can, or says it can't verify that part. It's written to confirm a command exists before using it, which heads off avoidable dead-ends — but a task can still block on missing credentials, permissions, or a failing test, and it'll tell you plainly when it does.

## A note on accuracy

These docs were checked against Anthropic's Claude Code docs on 2026-07-01. Claude Code commands and skill frontmatter change as Anthropic updates the tool — if a detail looks off against your `/help`, trust your installed version. Spot something stale? Open an issue.

## Want it enforced, not just asked?

This skill makes a *good* plan and tells Claude to verify each success criterion. If you want "done" to be a check that literally exits 0 — enforced by a real Stop hook that blocks a fake "done" before the turn can end — that's a separate, deeper kit: the **[Claude Code Setup Playbook](https://expressive446.gumroad.com/l/biaqgd)** (real hooks + skills you install once).

## License

MIT — use it, fork it, ship it. See [LICENSE](LICENSE).

---

*This project is not affiliated with, endorsed by, or sponsored by Anthropic PBC. Claude is a trademark of Anthropic PBC.*
