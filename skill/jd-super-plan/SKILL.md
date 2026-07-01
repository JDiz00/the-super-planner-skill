---
name: jd-super-plan
description: >
  A reusable end-to-end planner for Claude Code. Invoke with /jd-super-plan <your idea>. Opt-in
  only — disable-model-invocation prevents automatic loading, so it never fires on a casual
  question; you call it by command. Runs research → refine → goal-contract → plan → execute →
  verify, and self-sizes: a tiny task gets a one-screen plan, a real build gets the full loop.
argument-hint: '<your idea in plain English>'
category: Prompting
tags: [Prompting, Planning, Research, Local]
user-invocable: true
disable-model-invocation: true
---

# Super Plan

Turn an idea into the strongest possible plan, then execute it with a verify/repair loop. This
is an orchestration skill, not a one-shot coding request. **Do not write code until the plan is
approved.**

The idea to plan: **$ARGUMENTS** (if empty, ask for it in one line before anything else).

---

## Rule 0 — Size the task FIRST (this decides everything else)

Before any phase, silently classify the task, state the tier in one line, then run only that
tier. When unsure, go one tier heavier — but say so.

- **Tier 1 · Quick** — a small, well-understood change (add a button, fix copy, a known one-file
  edit, a config tweak). No unknowns, no external service, no new dependency.
  → Skip to **Mini-Plan** (below). One approval, then build + verify. No research, no goal
  contract, no routing map.

- **Tier 2 · Standard** — a real feature or a multi-file change inside a known codebase.
  → Run **Phases 1–10 on the light path**: research is docs-verify only (a social-signal pass
  only if market/UX signal matters); Goal Contract + Plan collapse into **one** combined
  approval gate.

- **Tier 3 · Deep** — a new project, an unfamiliar domain, a **new or unfamiliar external API, a
  new auth/payment/security boundary, third-party data, or a high-blast-radius change**, or you
  say "do this right" / "be thorough" / "from scratch".
  → Run the full pipeline, all phases. **Max two pre-build approval gates** (combined
  spec+contract, then plan) unless you ask for the deep-refine pass.

State it like: `Tier 2 (Standard) — a feature inside an existing codebase. Running the light path.`

### Mini-Plan (Tier 1 only)
Skip the ceremony. In one short message: one-line goal · files you'll touch · the exact change ·
the verify command and expected result. Get a yes, build it, verify it, report with evidence. Done.

---

## Operating rules (all tiers)

- **Never invent.** No API, package, command, file path, price, model id, or flag from memory.
  Verify against the repo or current official docs. Can't verify → say so. A stated gap beats a
  confident guess.
- **One question at a time.** Multiple-choice when you can. Don't batch a wall of questions.
- **State assumptions and continue** when a detail is safely inferable — don't stall.
- **Recommendation first.** At every fork, lead with your genuine best pick, labeled
  `(Recommended)`, one line of why.
- **Approval gates are real stops.** When a phase says "pause for approval," stop and wait.
- **Check for prior work.** Before researching or building, see if this was already done (your
  own notes, saved memory, a prior branch). Same work → say so in one line, confirm reuse/extend
  vs rebuild.
- **Sensitive-data egress is a hard stop.** Before recommending OR performing any copy / upload /
  sync / push of data off-box: is it already backed up (a derived artifact needs no second copy)?
  Is it the smallest footprint? Whose data is it? **Third-party PII / a client's financial data =
  HARD STOP** needing explicit consent for that exact action. A casual "do it" is not that consent.

---

## Phase 1 · Fresh research (Tier 2 light · Tier 3 full)

Get current signal before refining. **For an existing repo, first glance at dependency/env files
(package.json, requirements, lockfiles) to know the actual versions — then verify docs against
those versions, not the latest.** Route by claim type:

**Technical / API / pricing / security / legal → official docs (do this for every tier that
researches).** Verify against the source: the library/framework/SDK/CLI's own docs (even
well-known ones), the vendor's docs + live pricing for models/APIs. Never a social post for a
technical fact. If you have a docs-fetch tool (Context7 or similar), use it; otherwise fetch the
official docs page directly.

**Social / market / pain-point signal → a recent-activity search.** Run it for: what people
actually say about this problem now, current tools/workflows, complaints, real good/failed
examples, competitor buzz, feature expectations. Treat it as **trend signal, not truth.** Tier 2
uses it only when market/UX signal actually matters. Use whatever web-search tool you have; if
thin, say so and fall back to a plain web search.

**Deep, citation-grade question → a multi-source research pass** (if you have a deep-research
tool). Reach for it when the plan hinges on a verified multi-source answer, not just a pulse.

**Synthesize into:** key findings · ideas worth adding · risks/warnings · current best options ·
what this means for the plan · sources. Don't proceed to refinement until this exists.
*(Tier 1 skips this entirely.)*

---

## Phase 2 · Refine the idea

If you have a brainstorming skill available, invoke it now and feed it the Phase 1 findings.
Else run its shape manually: ask one question at a time until the idea is clear — target user,
exact problem, outcome, must-haves vs nice-to-haves, platform/env, data sources, integrations,
security/privacy, constraints, budget, deadline, and what "done" means. Challenge weak
assumptions. Propose 2–3 approaches with trade-offs + a recommended pick. Produce a refined
design/spec.

*(Tier 2 & 3: don't stop here — carry this straight into the combined gate at Phase 4.)*

---

## Phase 3 · Inspect project context

Read the ground truth so the plan fits reality: CLAUDE.md · AGENTS.md · README/docs · package/
dependency files · scripts · tests · conventions · available skills/tools/MCPs · env · recent
commits. Follow existing patterns; don't propose unrelated refactors. Re-confirm: no invented
paths, deps, or commands.

---

## Phase 4 · Goal Contract  ← combined approval gate

Lock the target: primary goal · user outcome · **success criteria (numbered — each must be
individually checkable)** · non-goals · constraints · assumptions · risks · deliverables ·
**definition of done (runnable/checkable where possible)** · blocked items. For money / security
/ client-data work, add the **sensitive-data egress line**: what leaves the box, whose it is, and
that consent is confirmed (or the task stays on-box).

**Gate:** present Phase 2's refined spec + this Goal Contract + (for Tier 2) the Phase 6 plan
together as **one** approval. Wait for your yes.

---

## Phase 5 · Tool / skill routing map  *(Tier 3 only)*

A short table, **required tools only, ≤5 rows**: tool/skill · why · when in the build · fallback.
Verify each named command actually exists on this box before listing it. Skip this for Tier 2.

---

## Phase 6 · Implementation plan

Plain English in the body; put all file paths / line numbers / function names in a technical
appendix below a `---`, never in the body.

Sections: Discovery · Architecture · Data model · Files to create/edit · Dependencies ·
Implementation slices · Verification plan · Security review · Maintainability review · Repair
strategy · Handoff checklist.

**Each slice carries:** goal · **which success criterion / DoD item it satisfies** · files
affected · exact work · verify command/check · expected result · rollback/repair note. Small
slices beat big ones. Every success criterion from Phase 4 must be covered by at least one slice —
if one isn't, the plan is incomplete.

Tier 2: this is folded into the Phase 4 gate. Tier 3: **pause** for approval (revise, or hand to
the optional deep-refine).

---

## Phase 7 · Optional deep-refine  *(only if you ask)*

Use whatever plan-refinement tool is actually installed (verify first): a plan-loop skill that
writes a machine-checkable Definition of Done, an auto-plan reviewer, or a writing-plans skill.
**Never call a command you haven't confirmed exists on this box.** Refine for: architecture
correctness, simplicity, security, testing, failure points, missing requirements, execution
order, complexity, smaller slices, better verification. Summarize what changed. Pause for final
approval.

---

## Phase 8 · Execute

Only after approval. Build in small slices, verifying after each. Fix failures before continuing.
Reuse existing patterns, avoid unnecessary dependencies (smallest change that works), use current
docs for external services, preserve working behavior unless the plan says otherwise.

---

## Phase 9 · Verify / repair loop (per slice)

1. Run the check tied to this slice's success criterion: test · build · lint · typecheck · API
   call · UI check. For UI, verify FUNCTION yourself with a scripted browser check — loads,
   console clean, element rendered *and visible*, real action fires. Ask only the aesthetic
   question.
2. Read the failure carefully. Fix the **root cause**, not the symptom.
3. Re-run until green, or until genuinely blocked (missing access/credential, broken external
   dep, unclear requirement, tool limit). If blocked, stop and report the blocker plainly.

---

## Phase 10 · Final review & report

Before claiming done: review the diff · **walk every Phase-4 success criterion and confirm each
has a passing check** (a criterion with no evidence is not done) · security pass · egress pass for
any off-box data · maintainability · no unnecessary complexity · docs/comments sane.

**Final report:** what was built · files changed · commands/checks run · pass/fail evidence per
criterion · remaining risks · blocked items · recommended next step. Don't exaggerate. If
something wasn't tested, say so. "Done" means verified.

---

## Why this beats a giant paste
- Self-sizes, so a small task doesn't drown in a 10-gate ceremony (one combined gate on Tier 2).
- Research is routed (official docs for truth · a recent-activity search for pulse · a
  deep-research pass for depth) and version-aware, instead of one blurry pass.
- Calls only commands you've confirmed exist on your box, so it never dead-ends on a command
  that isn't installed.
- Every slice traces to a success criterion, and "done" requires a passing check for each one.

---

*This skill is not affiliated with, endorsed by, or sponsored by Anthropic PBC. Claude is a
trademark of Anthropic PBC.*
