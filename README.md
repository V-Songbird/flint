<div align="center">
  <h1>flint</h1>
  <p><strong>Make Claude Code answer in plain words you can actually read.</strong></p>
</div>

<p align="center">
    <a href="https://github.com/V-Songbird/flint/blob/main/LICENSE"><img src="https://img.shields.io/github/license/V-Songbird/flint" alt="License"/></a>
    <a href="https://docs.anthropic.com/en/docs/claude-code"><img src="https://img.shields.io/badge/Claude_Code-E5582B" alt="Claude Code"/></a>
</p>

> **TL;DR** — Claude Code answers a small question with 400 words of invented jargon, and you still cannot tell what it did. Its own "Concise" setting barely dents that. Copy two text files into your project and the same answer comes back in 49 plain words. Nothing to install.

---

## The problem

Claude Code is the assistant that edits files in your project from the terminal. It works well and it writes badly.

You ask one thing. Back comes a wall of text: words it made up on the spot, a caveat you did not ask for, and no clear ending. Read it twice and you still cannot say what it changed.

That is the real cost. Not the wall of text — the fact that you stop being able to check the work.

## The fix

Two plain text files. One goes in your home folder, one in your project.

### Let Claude do it

If you would rather not copy files around, paste [`prompts/install.md`](prompts/install.md) into Claude Code and it sets itself up. It fetches both files, puts each where it belongs, and asks first if something is already there.

Prefer to do it yourself? The two steps are below.

### 1. Make it write like a person

Claude Code lets you hand it a file that says how to sound. **[`output-styles/hush.md`](output-styles/hush.md)** is one. Short sentences. Everyday words. One report at the end, not a running commentary.

**Put the file where Claude looks.** Inside your home folder there is a folder called `.claude`. Drop `hush.md` into `.claude/output-styles/`, making that folder if it is not there yet. Claude now finds it in every project.

**Turn it on.** Open Claude Code and type:

```
/output-style
```

Pick **Hush** from the list. That is it.

### 2. Make it write less code

Claude also reads a file called `CLAUDE.md` in your project and treats it as standing instructions. **[`fragments/razor-hush.md`](fragments/razor-hush.md)** tells it to check whether the code is needed at all, and to reuse what your project already has, before writing anything new.

```bash
cat fragments/razor-hush.md >> your-project/CLAUDE.md
```

If your project has no `CLAUDE.md` yet, that command makes one.

## Does it work?

24 sessions on Claude Opus 5, high effort. Four real jobs, two runs each, three setups, all run together so the numbers compare.

| Setup | Words in the reply | Chatter while working |
|---|---|---|
| Claude Code on its own | 411 | 25 words |
| Its own built-in `Concise` style | 386 | 23 words |
| **With both files** | **49** | **4 words** |

`Concise` cuts the reply by 6%. These two files cut it by 88%, on the same jobs, with the same answers.

23 of the 24 got the right answer. The one miss was flint: it put the detail in a file and left the summary too short for the marker, which reads the reply. Right work, wrong place.

Three honest caveats.

Cost did not move either way. One batch had flint 32% cheaper, a second had it 12% dearer. Treat it as a wash.

Text files cannot reach into build and test output, so long command output still goes through untrimmed.

Two batches, two runs per job. Promising, not settled.

## Tidy up the rules you already have

**[`prompts/tune-for-opus5.md`](prompts/tune-for-opus5.md)** is not a file to install. It is a message you paste into a Claude Code chat, in any project.

It reads every instruction file you have, finds the rules too vague to act on or pointing at files that no longer exist, and rewrites them. You see each change before it lands, and you can undo any of it.

The grading is done by a plugin called `assay`, so you need that one installed for this prompt to run.

## Go one level beyond this

flint is text. It cannot watch what Claude does or step in mid-task. Plugins can.

These three are the full versions of the ideas here. Add the collection once:

```
/plugin marketplace add V-Songbird/foundry
```

Then install whichever you want.

| Plugin | What it adds beyond the text |
|---|---|
| **[hush](https://github.com/V-Songbird/hush)** | Trims long command output and logs before they fill the session, and nudges the quiet back when a session slips. This is the part a text file genuinely cannot do. The style here comes from it. |
| **[razor](https://github.com/V-Songbird/razor)** | Stops the actual moment a package gets added and asks once, with your existing packages in the message. Counts what a session added. |
| **[foreman](https://github.com/V-Songbird/foreman)** | Keeps a to-do list in your repo, picks what to work on next, and says why that one came first. |

## Why "flint"

Flint is the stone you strike to start a fire. These files are what you strike to start a session.

## License

MIT. See [LICENSE](LICENSE).

The style and the rules come from hush and razor. Both are MIT, both by the same author, and both are copied here unchanged apart from one line that only applies inside a plugin.
