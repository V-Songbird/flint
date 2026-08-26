<div align="center">
  <h1>flint</h1>
  <p><strong>Two files you drop into a project so a fresh Claude session starts sharp — no plugins, no setup.</strong></p>
</div>

<p align="center">
    <a href="https://github.com/V-Songbird/flint/blob/main/LICENSE"><img src="https://img.shields.io/github/license/V-Songbird/flint" alt="License"/></a>
    <a href="https://docs.anthropic.com/en/docs/claude-code"><img src="https://img.shields.io/badge/Claude_Code-E5582B" alt="Claude Code"/></a>
</p>

> **TL;DR** — Claude writes more code than you asked for, and narrates while it works. Copy one file into your project and it stops doing both. The second file is a prompt that tidies up the instructions you already have. Nothing to install.

---

## What is this?

A fresh Claude Code session on a new machine has no plugins. It adds libraries you did not need, and it talks you through every step on the way.

flint is two plain text files that fix that with nothing but a copy and paste.

| File | What it does |
|---|---|
| [`fragments/razor-hush.md`](fragments/razor-hush.md) | Paste into your `CLAUDE.md`. Claude cuts before it adds, and it reports once at the end instead of narrating. |
| [`prompts/tune-for-opus5.md`](prompts/tune-for-opus5.md) | Paste into a chat. Claude audits the instruction files you already have and rewrites the weak ones. |

## Install

Copy the fragment into your project's `CLAUDE.md`. That is the whole install.

```bash
cat fragments/razor-hush.md >> /path/to/your/project/CLAUDE.md
```

If the project has no `CLAUDE.md`, the file becomes it.

## The second half is optional

The fragment says to report once, at the end, in short plain sentences. It does not say what those sentences sound like.

For that you want an output style. Copy [hush](https://github.com/V-Songbird/hush)'s style file into `.claude/output-styles/hush.md` in your project, then set it in `.claude/settings.json`:

```json
{
  "outputStyle": "Hush"
}
```

Without it the fragment still works. Claude just writes in its own voice.

## What it measured

24 headless sessions on Claude Opus 5, high effort. Four jobs, two runs each, three setups.

| Setup | Cost per job | Words written | Chatter while working |
|---|---|---|---|
| Plain Claude | $0.66 | 8,798 | 47 words |
| The hush plugin | $0.64 | 6,528 | 6 words |
| This fragment | $0.45 | 3,626 | 4 words |

Every one of the 24 got the right answer.

One catch. The fragment is text only, so nothing trims long build and test output. That traffic went up 10 to 19% on the noisy jobs. The plugins have a hook for that; a file cannot.

One batch, two runs per job. Promising, not settled.

## Tuning the instructions you already have

[`prompts/tune-for-opus5.md`](prompts/tune-for-opus5.md) is a prompt, not a file to install. Paste it into a session in any repository.

It grades every rule and every skill description in the project, rewrites the ones that name no clear moment to act, repoints the ones naming files that are gone, and builds a real check for the duties a script can do better than a sentence. Every change is previewed first, and every change can be undone.

It needs the [assay](https://github.com/V-Songbird/assay) plugin installed to do the grading.

## Why "flint"

It is what you strike to start a fire. These files are what you strike to start a session.

## License

MIT. See [LICENSE](LICENSE).

The fragment's rules come from [razor](https://github.com/V-Songbird/razor) and [hush](https://github.com/V-Songbird/hush), both MIT, both by the same author.
