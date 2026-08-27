You are a senior engineer tuning this project's Claude Code instruction files so that Claude Opus 5 follows them reliably. You have no memory of any earlier conversation. Everything you need is below or in the repository you are sitting in. Use nothing but the tools you already have.

Before you touch a file, make sure my work is safe. If this is a git repository with uncommitted changes, say so and stop until I answer. If it is clean, note the current commit so I can get back to it, and give me that command at the end.

First, find what actually gets loaded. Look for all of these, and report which exist:

- `CLAUDE.md` in the project root, and any `CLAUDE.md` in subdirectories
- every file under `.claude/rules/`
- every `.claude/skills/*/SKILL.md`, specifically the `description:` line in its frontmatter
- every `.claude/agents/*.md`, same
- `.claude/settings.json` and any hooks it wires

Read each one in full. Then tell me in a few lines what you found: how many files, how many separate rules, and anything that surprised you — a file nothing loads, a rule written for a different model, a duty a wired hook already enforces.

Now grade every rule. A rule is one instruction: a sentence or bullet that asks for something. Judge each against three questions, and say which ones it fails.

Does it name a moment Claude can recognize? "When you change a file under `src/`" is a moment. "Keep things tidy" is not, and neither is "when possible". A rule with no moment is not followed late — it is not followed at all.

Does it name an action with an artifact? "Add a line to `CHANGELOG.md` under Unreleased" is an action. "Be careful about documentation" is not.

Does it only forbid? A rule that says never do X, with no alternative and no escape hatch, can stall a whole session when the task genuinely needs X. Pair it with what to do instead, or with "stop and ask me".

Now check where each rule lives, which matters as much as how it is written.

`CLAUDE.md` is loaded into every single session in this project, whether the rule applies or not. That is what it is for: things that are true of all work here. A rule that only applies to some of the code does not belong in it. "When editing TypeScript files, prefer named exports" is paying for itself in every Python session, every documentation session, every session that never opens a `.ts` file. Move that rule into its own file under `.claude/rules/`, scoped to the files it is about, and it loses the when-clause because the scope now says it: "Use named exports."

So for each rule, tell me which of these it is. It genuinely applies to all work here, so `CLAUDE.md` is right. Or it applies to one language, one folder, or one kind of file, so it belongs in a scoped rules file — and name the pattern it should be scoped to.

Two more placement problems to look for, and both are worse than any wording problem because the rule never reaches the model at all. A rules file scoped to a pattern that matches nothing in this repository. And a file shadowed by another one, or sitting past a read limit, so the host skips it.

Then check three mechanical things that have nothing to do with wording. A rule pointing at a file, function, or command that no longer exists — verify each path and each command for real, do not assume. Two rules asking for the same thing in different words. Two rules that contradict each other.

Report all of that before changing anything. Order it worst first, in this order: rules the host never loads, then rules pointing at things that are gone, then rules in the wrong file, then contradictions and duplicates, then wording. A rule that never loads is broken for every model. Wording strength is a matter of degree, and it is the part I am least sure transfers to Opus 5.

Then, for the wording problems only, show me each rewrite before you make it. Old line, new line, one sentence on what changed. Sharpen how a rule asks; never change what it asks for. Here is the shape:

Before: "Keep the changelog updated." No moment, no artifact, so it gets skipped entirely. After: "When you change any file under `src/`, add a line to `CHANGELOG.md` under Unreleased in the same commit."

Apply the ones I approve, one file at a time. When a rule needs to move to a scoped file, moving it is a change like any other — show me the new file, its scope pattern, and the line as it will read once the scope carries the when-clause.

Two kinds of finding you must report and must not fix yourself. Where two rules disagree, name both sides and leave the choice to me. Where a duty is genuinely mechanical — a command that must run, a path that must never be edited, a file that must stay in step with another — say so and describe the hook or script that would do it, but do not write it unless I ask. Prose a machine could enforce is worth flagging, not worth replacing on your own initiative.

Never delete or deactivate a rule. If you believe one is obsolete, that is a finding to report, not a change to make.

Skill and agent descriptions get the same treatment as rules, judged differently. A description is the only thing that decides whether Claude reaches for that skill at all. It must say when to use it, in the words someone would actually type, and when not to. A description that reads as a summary of what the skill does will never fire.

Stay inside these limits. Do not touch source code, tests, or CI configuration. Do not edit instruction files outside this repository. Do not add a dependency or install anything — everything here is reading and rewriting text.

When you are done, tell me: what you found, what you changed, what you deliberately left alone and why, which duties would be better as a hook, and the exact git command that puts everything back.
