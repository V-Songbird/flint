# Contributing

Thanks for looking. flint is small on purpose: two text files you copy into place, no code, no install step. The two files are the output style `output-styles/hush.md` and the `CLAUDE.md` fragment `fragments/razor-hush.md`.

## What belongs here

A change that makes the fragment work better on a real project. That is it.

The output style is a copy of hush's `output-styles/hush.md`. Propose a change to the style in hush; flint's copy changes only to follow hush. It differs from hush in three plugin-only parts: it has no `force-for-plugin` frontmatter key, no `[hush ...]` paragraph and no paragraph about `/hush:config` and "hush settings".

The older style `output-styles/hush-deprecated.md` still works and is explained in [the older Hush voice](docs/knowledge/deprecated-style.md). The two messages in `prompts/` are what users paste into Claude Code: `install.md` installs the two files, and `tune-for-opus5.md` tidies the rules a user already has. Change `install.md` when you rename or move either file.

A new file has a high bar. Every line in `fragments/razor-hush.md` is loaded into every session in every project that uses it, so a line has to earn its place against that cost. If a rule only matters to one language or one framework, it does not belong in the shared fragment.

## What does not belong here

- Anything that needs installing. flint is copy and paste.
- Rules that fire on a moment Claude cannot recognize. "Keep things tidy" is not a rule.
- Numbers in the README that came from anywhere but a real measured run.

## Changing the fragment

If your change makes the fragment longer or changes what a rule asks for, say in the pull request how you know it helps.

A change that only shortens wording, fixes a typo, or repairs a dead link needs no measurement.

## Reporting a problem

Open an issue. Say which file, what you expected, and what happened instead. If Claude ignored a rule, paste the part of the session where it did.

## Style

Short sentences. Everyday words. Say what a thing is before naming it.

Rules are written for a model reading them cold: name the moment the rule fires, then name the action. Never a rule that only says what not to do — pair it with what to do instead, or a session can stall on it.

## Code of conduct

By taking part you agree to the [code of conduct](CODE_OF_CONDUCT.md).
