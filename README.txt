The Super Planner Skill for Claude Code
=======================================
Built 2026-07-01 · Free · MIT

WHAT THIS IS
------------
A drop-in Claude Code skill that turns "build this" into a real plan, then
executes it with a verify/repair loop. One command. It sizes the job itself
(Quick / Standard / Deep), researches from the right source, locks a checkable
goal, plans, builds after you approve, and is told not to call anything "done"
without a passing check.

This is the planner used on real projects. It's opt-in only (/jd-super-plan) —
it never hijacks a casual question.

START HERE
----------
Open  START-HERE.html  in any browser. It shows you how to install the skill
and run it, with a copy button on everything you paste.

INSTALL (30 seconds)
--------------------
  mkdir -p ~/.claude/skills/jd-super-plan && cp -R skill/jd-super-plan/. ~/.claude/skills/jd-super-plan/

No dependencies, nothing to build. Claude Code picks it up next session.

USE
---
In Claude Code, type:
  /jd-super-plan build a rate limiter for my API
It states the tier it picked, runs that tier, pauses at the approval gate, then
guides Claude Code through the build and verification after you approve. Run
/jd-super-plan with no arguments and it asks for the idea in one line. (It's
opt-in only — plain "help me plan X" won't trigger it; use the command.)

WHAT'S IN HERE
--------------
  skill/jd-super-plan/SKILL.md   the skill (the whole thing is one file)
  START-HERE.html                friendly walkthrough (open in any browser)
  README.md / README.txt         docs (Markdown + plain text)
  VERSION.txt                    version + contents manifest
  LICENSE                        MIT

REQUIREMENTS
------------
Claude Code. Nothing else — the skill is a single Markdown file, no packages,
no runtime. Some phases mention optional tools (docs-fetch, web-search,
deep-research, brainstorming); if a tool is unavailable, the skill uses a
direct source when it can, or says it can't verify that part. It confirms a
command exists before using it (heads off avoidable dead-ends), but a task can
still block on missing credentials/permissions/a failing test — it says so
plainly when it does.

A NOTE ON ACCURACY
------------------
These docs were checked against Anthropic's Claude Code docs on 2026-07-01.
Claude Code commands and skill frontmatter change as Anthropic updates the
tool. If a detail looks off against your /help, trust your installed version.

WANT THE ENFORCED VERSION?
--------------------------
This skill makes a good plan and asks you to verify. If you want "done" to be a
check that literally exits 0 — enforced by a Stop hook that blocks a fake
"done" — that's a separate, deeper kit: the Claude Code Setup Playbook. See the
Gumroad listing this came from.

This product is not affiliated with, endorsed by, or sponsored by Anthropic PBC.
Claude is a trademark of Anthropic PBC.
