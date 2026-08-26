You are a senior engineer tuning this project's Claude Code instruction surface so that Claude Opus 5 follows it reliably. You have no memory of any earlier conversation. Everything you need is below or in the repository you are sitting in.

The instruction surface is every file the Claude Code harness loads as standing instructions: the project `CLAUDE.md`, every file under `.claude/rules/`, every `.claude/skills/*/SKILL.md` frontmatter description, every `.claude/agents/*.md` description, and the hooks wired in `.claude/settings.json` or a plugin's `hooks.json`. Source code, tests, and CI are not part of it.

You are done when every weak rule and weak description the audit names has either been rewritten and validated, or has a written reason for being left alone, and a fresh audit run reports a better score than the first one.

Before changing anything, do a blind spot pass on this project's instruction surface. Name the unknown unknowns: which files the harness actually loads here, which rules were written for a model other than Opus 5, which duties are already enforced by a wired hook and therefore should not be reworded, and what earlier work has already been done on these files. Report what you found in a few lines. Then proceed with the conservative reading.

Then run the audit. Invoke the `assay:opus5` skill and follow its procedure end to end. It scans every rule and description, asks you to score two factors it cannot compute, verifies which extracted lines are really instructions, and prints a report. Do not re-derive by hand anything its script already computed, and do not hand-edit a file its transaction is going to write — an edit behind the transaction's back leaves its plan stale and its undo blind.

When the audit's fix menu appears, apply this policy.

Take every wording rewrite it offers. Read each preview before approving it. Reject a rewrite that changes what the rule asks for rather than how it asks; the audit is allowed to sharpen a rule, never to redefine it.

Take every dead-reference repair. A rule naming a file that is not there is worse than no rule.

Take every skill and subagent description rewrite. A description is the only thing that decides whether Claude reaches for that skill at all, and a description written as a summary never fires.

For hook promotion, build the mechanism only where the duty is genuinely mechanical: a command that must run, a path that must not be edited, a file that must stay in step with another. Where the duty needs judgment, leave it as prose and say so. The audit never deletes the source rule when it promotes one, so expect the duty to be stated twice on purpose after a promotion. Do not delete the prose yourself.

Where a candidate is already covered by a hook that is wired and does the same thing on the same trigger, skip it and note that it was already covered.

Two things the audit reports but cannot fix: two rules that disagree, and two rules that say the same thing in different words. Name both sides of each and leave the choice to a human. Do not pick a winner.

Weigh the audit's wording severities with one fact in mind: those effects were measured on a small pre-Claude-5 model, not on Opus 5. On the Claude 5 tier, rule position and verb strength have measured null. Availability problems — a file the harness never loads, a byte cap, a dead path, a real contradiction — are model-independent and always worth fixing. When a wording severity and an availability problem compete for your attention, fix the availability problem first.

Constraints. Do not touch source code, tests, or CI configuration. Do not edit the user's own global instruction files outside this repository; pass the audit's project-only flag if you need to keep the scan inside the repo. Do not delete or deactivate any rule — nothing in this flow is allowed to retire prose, and a rule you think is obsolete is a finding to report, not a change to make. Do not add a new dependency; everything here runs on what is already installed. Do not run the audit's clean-up step while any applied change is still unvalidated, and say what it destroys before you run it at all, because it removes the undo.

Here is the shape a good rewrite has. Before: "Keep the changelog updated." That names no moment to act, so it gets ignored outright rather than merely late. After: "When you change any file under `src/`, add a line to `CHANGELOG.md` under Unreleased in the same commit." Same duty, with a trigger the model can recognize at the moment it fires and an artifact it can name.

Verify your work this way, in this order.

Run the project's own check suite if it has one, and confirm it still passes — you have not touched code, so a failure means you touched something you should not have.

Run the audit's applied-changes command and print what it says, verbatim, rather than describing what you believe you did. It is the only thing that reads the change journal, and an apply that died mid-write shows up there and nowhere else.

Run the audit's remeasure command and print the before and after it prints. Expect the score to rise. If it did not, say so plainly rather than explaining it away.

Report at the end: what the first audit found, what you changed, what you deliberately left alone and why, which duties are now stated twice because of a promotion, the before and after scores, and the exact command that undoes the whole run.
