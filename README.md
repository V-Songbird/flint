<div align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/logo-dark.svg" />
    <img src="assets/logo.svg" alt="flint" width="200" />
  </picture>
  <h1>flint</h1>
  <p><strong>Make Claude Code answer in plain words you can actually read.</strong></p>
</div>

<p align="center">
    <a href="https://github.com/V-Songbird/flint/blob/main/LICENSE"><img src="https://img.shields.io/github/license/V-Songbird/flint" alt="License"/></a>
    <a href="https://docs.anthropic.com/en/docs/claude-code"><img src="https://img.shields.io/badge/Claude_Code-E5582B" alt="Claude Code"/></a>
</p>

> **TL;DR** — Ask Claude Code a small question. Back come 500 words of invented jargon. You still cannot tell what it did. Its own `Concise` setting cuts that by a third. You are no wiser. Drop two text files in place. The same answer comes back in 75 plain words. Claude will even do it for you. Nothing to install.

---

## The problem

Claude Code edits your project from the terminal. It works well. It writes badly.

You ask one thing. Back comes a wall of text. Words it made up on the spot. A caveat you did not ask for. No clear ending. Read it twice. You still cannot say what it changed.

That is the real cost. Not the wall of text. You stop being able to check the work.

## What the difference looks like

Here is one real job. Four questions about a gateway outage. The last one was this:

> Write me the on-call handoff: root cause, the impact window, and which deploy to roll back.

**Plain Claude Code wrote 1155 words.** That is 115 lines. Ten headings, three tables, two blocks of code:

```
# On-Call Handoff — Gateway 503s
## TL;DR
## Root cause
## Impact window
## Roll back: dep-4471
## Timeline
## Ruled out
## Watch out — two red herrings
## Open items after rollback
## Data caveats
```

None of it is wrong. It is a document. Pasted into a chat window. At four in the morning. Somebody asked one question. And the last line was this:

> Want me to save this as a markdown file in the working directory?

After 1155 words, it had not acted.

**With the two files, 52 words.** Not one word of chatter while it worked.

> **Handoff written to `INCIDENT-2026-03-14-gateway-503.md`.**
>
> Root cause is `gateway.pool.perRequest: false -> true`. Every request grabs its own connection from a pool of 20. It empties and never refills.
>
> Impact ran `02:14:18.972Z` to log end `03:59:27.974Z`. That is 1h 45m, 530 of 636 requests failed, 83%. Still broken.
>
> Roll back `dep-4471`, gateway, by `a.novak`.

In the real reply that name is a link. You can click it.

It wrote the file. Then it said where. The cause is there. So is the window. So is the deploy to roll back. The long version's detail is not lost either. It is in the file. Ready when you want it.

Both runs got the right answer. A script checks that. Mangle the facts and the run fails.

## The fix

Two plain text files. One goes in your home folder. One goes in your project.

### Let Claude do it

Would rather not copy files around? Paste [`prompts/install.md`](prompts/install.md) into Claude Code. It sets itself up. It fetches both files. It puts each one where it belongs. It asks first if something is already there.

Prefer to do it yourself? The two steps are below.

### 1. Make it write like a person

Claude Code reads a file that sets its voice. **[`output-styles/hush.md`](output-styles/hush.md)** is one. Short sentences. Everyday words. One report at the end, not a running commentary.

**Put the file where Claude looks.** Your home folder holds a folder called `.claude`. Drop `hush.md` into `.claude/output-styles/`. Make that folder if it is not there yet. Claude now finds it in every project.

**Turn it on.** Open Claude Code and type:

```
/output-style
```

Pick **Hush** from the list. That is it.

### 2. Make it write less code

Claude reads `CLAUDE.md` at every start. It follows whatever is in it. **[`fragments/razor-hush.md`](fragments/razor-hush.md)** asks two things of it. Check whether the code is needed at all. Reuse what your project already has.

Open `CLAUDE.md` in your project. Paste the fragment at the bottom. No `CLAUDE.md` yet? Make one and paste it in there.

That file stays with the project. Anyone who works on it gets the same rules.

## The style was rewritten

Installed flint before? The style file changed. The old one is still here and still works. [DEPRECATED.md](DEPRECATED.md) says how to keep it.

The old style was written for an older model. It repeated itself to make rules stick. It runs to 148 lines. It ends in a fifteen-step check.

The current one is 58 lines. It was written from scratch for the newer models. In testing, those models followed the shorter file more closely.

Three things it now asks for. The old one asked for none of them. Name the file you changed. Make it a link you can click. Say where things stand, not just what you did. End on the next move. Or say none is needed.

The reply limit went up. It was 6 lines. It is now 8. Those three things need room. A reply that cannot say what to open next just sends you back to ask.

## Does it work?

24 sessions on Claude Opus 5, high effort. Four real jobs, two runs each, three setups. All of them ran together, so the numbers compare.

| Setup | Words in the reply | Chatter while working |
|---|---|---|
| Claude Code on its own | 530 | 37 words |
| Its own built-in `Concise` style | 344 | 9 words |
| **With both files** | **75** | **4 words** |

`Concise` cuts the reply by about a third. These two files cut it by 86%. Same jobs. All 24 sessions got the right answer.

Three honest caveats.

Cost did not move much. flint came out 23% under plain Claude here. Earlier runs went both ways by a similar margin. Treat it as a wash for now.

Text files cannot reach into build and test output. Long command output still goes through untrimmed.

Two runs per job, on one batch. Promising, not settled. An earlier batch on these jobs read very differently. It put `Concise` much closer to plain Claude. That is how far a few runs can move.

## Tidy up the rules you already have

**[`prompts/tune-for-opus5.md`](prompts/tune-for-opus5.md)** is not a file to install. It is a message you paste into a Claude Code chat. Any project will do.

It finds every instruction file the project loads. It grades each rule on one thing. Can Claude tell when to act on it? Then it rewrites the weak ones. It also checks every path and command a rule names. You see each change before it lands. It hands you the git command that undoes the lot.

Nothing to install for this either. Claude does it all with the tools it has.

## Go one level beyond this

flint is text. It cannot watch what Claude does. It cannot step in mid-task. Plugins can.

These two are the full versions of these ideas. Add the collection once:

```
/plugin marketplace add V-Songbird/foundry
```

Then install whichever you want.

| Plugin | What it adds beyond the text |
|---|---|
| **[hush](https://github.com/V-Songbird/hush)** | Trims long command output before it fills the session. Nudges the quiet back when a session slips. This is the part a text file cannot do. The style here comes from it. |
| **[razor](https://github.com/V-Songbird/razor)** | Stops the moment a package gets added. Asks once, with your existing packages in the message. Counts what a session added. |

## Why "flint"

Flint is the stone you strike for a fire. These files are what you strike for a session.

## License

MIT. See [LICENSE](LICENSE).

The style and the rules come from hush and razor. Both are MIT. Both are by the same author. Both are copied here unchanged. One line was dropped. It only applies inside a plugin.
