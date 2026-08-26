<div align="center">
  <h1>flint</h1>
  <p><strong>Plain text files that make Claude Code write less and talk less. Nothing to install.</strong></p>
</div>

<p align="center">
    <a href="https://github.com/V-Songbird/flint/blob/main/LICENSE"><img src="https://img.shields.io/github/license/V-Songbird/flint" alt="License"/></a>
    <a href="https://docs.anthropic.com/en/docs/claude-code"><img src="https://img.shields.io/badge/Claude_Code-E5582B" alt="Claude Code"/></a>
</p>

> **TL;DR** — Ask Claude Code for one small thing and you often get a new library, extra files, and a running commentary you did not need. Paste one file into your project and it stops. Copy, paste, done.

---

## The problem

Claude Code is the assistant that edits files in your project from the terminal.

Left alone it adds. A helper you did not ask for, a package to do three lines of work, a folder for a future that never arrives. All of it is now yours to read and maintain.

It also narrates. Paragraphs of "now I'll check the config" between every step, and a wall of summary at the end.

## The fix

Claude reads a file called `CLAUDE.md` in your project, every session, and treats it as standing instructions.

flint gives you one to paste in.

```bash
cat fragments/razor-hush.md >> your-project/CLAUDE.md
```

If your project has no `CLAUDE.md` yet, that command makes one. That is the whole install.

**[`fragments/razor-hush.md`](fragments/razor-hush.md)** tells Claude two things. Check whether the code is needed at all before writing it, and reuse what the project already has. Then stay quiet while working and give you one short report at the end.

## Does it work?

24 sessions were run head to head on Claude Opus 5. Four real jobs, two runs each, three setups. Every single one got the right answer.

| Setup | Cost per job | Words written | Chatter while working |
|---|---|---|---|
| Claude Code on its own | $0.66 | 8,798 | 47 words |
| This file | $0.45 | 3,626 | 4 words |

One catch. A text file cannot reach into build and test output, so long command output still goes through untrimmed. On the noisiest jobs that traffic rose 10 to 19%.

One test batch, two runs per job. Promising, not settled.

## Make the reports nicer to read

The file above says report once, at the end. It does not say what that report sounds like.

For that, Claude Code has a setting called an output style: a file that says how Claude should sound. **[`output-styles/hush.md`](output-styles/hush.md)** is one, and it is the one used in the test above.

Copy it into your project and switch it on:

```bash
mkdir -p your-project/.claude/output-styles
cp output-styles/hush.md your-project/.claude/output-styles/
```

Then add this to `.claude/settings.json` in your project:

```json
{
  "outputStyle": "Hush"
}
```

Now the report comes back in short plain sentences. Skip this and everything still works — Claude just writes in its own voice.

## Tidy up the rules you already have

**[`prompts/tune-for-opus5.md`](prompts/tune-for-opus5.md)** is not a file to install. It is a message you paste into a Claude Code chat, in any project.

It reads every instruction file you have, finds the rules that are too vague to act on or point at files that no longer exist, and rewrites them. You see each change before it lands, and you can undo any of it.

The grading is done by a plugin called `assay`, so you need that one installed for this prompt to run.

## Go one level beyond this

flint is text. It cannot watch what Claude does or step in mid-task. Plugins can.

These three are the full versions of the ideas in this repo. Add the collection once:

```
/plugin marketplace add V-Songbird/foundry
```

Then install whichever you want.

| Plugin | What it adds beyond the text |
|---|---|
| **[razor](https://github.com/V-Songbird/razor)** | Stops the actual moment a package gets added and asks once, with your existing packages in the message. Counts what a session added. |
| **[hush](https://github.com/V-Songbird/hush)** | Trims long command output and logs before they fill the session. This is the part a text file genuinely cannot do. It is also where the output style here comes from. |
| **[foreman](https://github.com/V-Songbird/foreman)** | Keeps a to-do list in your repo, picks what to work on next, and says why that one came first. |

## Why "flint"

Flint is the stone you strike to start a fire. These files are what you strike to start a session.

## License

MIT. See [LICENSE](LICENSE).

The rules in the fragment, and the output style, come from razor and hush. Both are MIT, both by the same author, and both are copied here unchanged apart from one line that only applies inside a plugin.
