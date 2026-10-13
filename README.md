<div align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/banner-dark.png" />
    <img src="assets/banner-light.png" alt="flint" width="900" />
  </picture>
  <h1>flint</h1>
  <p><strong>Make Claude Code answer in plain words you can actually read.</strong></p>
</div>

<p align="center">
    <a href="https://github.com/V-Songbird/flint/blob/main/LICENSE"><img src="https://img.shields.io/github/license/V-Songbird/flint" alt="License"/></a>
    <a href="https://docs.anthropic.com/en/docs/claude-code"><img src="https://img.shields.io/badge/Claude_Code-E5582B" alt="Claude Code"/></a>
</p>

<p align="center">
    <a href="#the-fix"><strong>Install it</strong></a> &nbsp;·&nbsp;
    <a href="#what-the-difference-looks-like">See the difference</a> &nbsp;·&nbsp;
    <a href="#what-is-not-measured">What is not measured</a> &nbsp;·&nbsp;
    <a href="#the-style-was-rewritten">What changed</a> &nbsp;·&nbsp;
    <a href="#tidy-up-the-rules-you-already-have">Fix your own rules</a>
</p>

> **TL;DR** — Ask Claude Code a small question. Back comes a wall of invented jargon. You still cannot tell what it did. Drop two text files in place. They ask Claude to answer in plain words. Claude will even do it for you. Nothing to install.

---

## The problem

Claude Code edits your project from the terminal. It works well. It writes badly.

You ask one thing. Back comes a wall of text. Words it made up on the spot. A caveat you did not ask for. No clear ending. Read it twice. You still cannot say what it changed.

That is the real cost. Not the wall of text. You stop being able to check the work.

## What the difference looks like

Here is one real job. Four questions about a gateway outage. The last one was this:

> Write me the on-call handoff: root cause, the impact window, and which deploy to roll back.

**Plain Claude Code wrote a whole document.** Many headings, several tables, blocks of code. Its outline:

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

After all that text, it had not acted.

**With the two files, a short reply.** This one was written under flint's earlier 58-line style. It shows the shape of the reply, not the current style's output.

> **Handoff written to `INCIDENT-2026-03-14-gateway-503.md`.**
>
> Root cause is `gateway.pool.perRequest: false -> true`. Every request grabs its own connection from a pool of 20. It empties and never refills.
>
> Impact ran `02:14:18.972Z` to log end `03:59:27.974Z`. That is 1h 45m, 530 of 636 requests failed, 83%. Still broken.
>
> Roll back `dep-4471`, gateway, by `a.novak`.

In the real reply that name is a link. You can click it.

It wrote the file. Then it said where. The cause is there. So is the window. So is the deploy to roll back. The long version's detail is not lost either. It is in the file. Ready when you want it.

## The fix

Two plain text files. One goes in your home folder. One goes in your project.

### Let Claude do it

Would rather not copy files around? Paste [`prompts/install.md`](prompts/install.md) into Claude Code. It sets itself up. It fetches both files. It puts each one where it belongs. It asks first if something is already there. It shows you the style file before it saves it. It shows you the lines it adds to `CLAUDE.md` before it writes them. It stops and shows you both if a new rule clashes with one already in your `CLAUDE.md`.

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

Installed flint before? The style file changed. The first one, 148 lines, is still here and still works. [The older Hush voice](docs/knowledge/deprecated-style.md) says how to keep it. The 58-line one that came next is not kept. Its last version is in the history: [`hush.md` at `2503a06`](https://github.com/V-Songbird/flint/blob/2503a06/output-styles/hush.md).

The old style was written for an older model. It repeated itself to make rules stick. It runs to 148 lines. It ends in a fifteen-step check.

The current one is 84 lines. It is a copy of hush's newest voice.

Three things it now asks for. The old one asked for none of them. Link every file you name, so you can click it. Say how you know, and what you did not check. End on one exact step you can take now.

The reply limit is gone. It was 6 lines. Now the reply has a fixed shape instead. The result, why, how you know, what to do next. Those parts need room. A reply that cannot say what to open next just sends you back to ask.

## What is not measured

The current style is not measured. flint makes no claim about how much shorter or cheaper the replies are. Try it on your own work and judge the replies yourself.

Text files cannot reach into build and test output. Long command output still goes through untrimmed.

## Tidy up the rules you already have

**[`prompts/tune-for-opus5.md`](prompts/tune-for-opus5.md)** is not a file to install. It is a message you paste into a Claude Code chat. Any project will do.

It finds every instruction file the project loads. It grades each rule on one thing. Can Claude tell when to act on it? Then it rewrites the weak ones. It also checks every path and command a rule names. It flags lines written for older models: asking Claude to think harder, or to write out its reasoning. It flags three kinds of line Anthropic's Opus 5.5 system card warns about: orders never to ask, refuse or stop, rules about what goes in Claude's thinking, and trust handed to text from tool output. If no rule says when to keep going and when to stop and ask, it proposes one. You see each change before it lands. It hands you the git command that undoes the lot.

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

The style and the rules come from hush and razor. Both are MIT. Both are by the same author. The style is copied here unchanged, except for two parts. Both only work inside a plugin: the `force-for-plugin` setting and the paragraph about `[hush ...]` lines. The rules are adapted to work without either plugin.
