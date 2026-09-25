---
type: knowledge
summary: "Why flint's writing style was rewritten, what changed between the older and the current voice, the numbers the older one earned and how to keep using it; read before touching output-styles/hush-deprecated.md."
related_files:
  - output-styles/hush.md
  - output-styles/hush-deprecated.md
  - prompts/install.md
---

# The older Hush voice

flint ships two versions of the writing style. The current one is
[`output-styles/hush.md`](../../output-styles/hush.md), and it is the one the
[README](../../README.md) walks you through. This page is about the other one,
[`output-styles/hush-deprecated.md`](../../output-styles/hush-deprecated.md).

It still works. Nothing about it broke. If you had it and you liked it, you can
keep it, and this page tells you how.

## Why there is a newer one

The older style was written before Claude's newest models. It was built for a
model that needed to be told the same thing several ways, so it says a lot: 148
lines, six sections, and a fifteen-step checklist the model walks before it
sends anything.

The current style is 84 lines, a copy of the newest voice in the
[hush](https://github.com/V-Songbird/hush) plugin. It is not shorter for its own
sake: in testing, the newer models followed the 58-line style below more closely
than this 148-line one, and the older one had grown long by repeating itself. The
84-line style has not been tested that way yet.

Between the two, flint shipped a 58-line style. It is not kept as a file. Its last version is [`hush.md` at `2503a06`](https://github.com/V-Songbird/flint/blob/2503a06/output-styles/hush.md).

The 148-line and 58-line styles score about the same on the jobs we run. The
58-line one writes a little more than the 148-line one did and says more with it.
The 84-line style is not measured yet.

## What actually changed

| | Older | Current |
|---|---|---|
| Length of the style file | 148 lines | 84 lines |
| Reply limit | 6 lines, 60 words | None; a fixed shape instead |
| Sentence limit | 10 words | 12 words |
| Final check before sending | 15 steps | 2 steps |

Four things the current one asks for that the older one did not:

1. Name the file you changed or found, as a link, so the reader can click it.
2. Say how you know: what you ran or read, and what you did not check.
3. End on one exact next step the reader can take now. There is always one.
4. Write the reply in the language the user writes in.

The reply limit is **gone**. The older style capped a reply at 6 lines. The
current one gives it a fixed shape instead: the result, the idea behind it, how
it was checked, and what to do next. That is deliberate. The four
items above need room, and a reply that is too short to say what to open next
sends the reader back to ask.

## How the older numbers were measured

These are the numbers this style earned, kept here because the README now
carries the 58-line style's. Neither table is for the current 84-line style,
which is not measured yet.

24 sessions on Claude Opus 5, high effort. Four jobs, two runs each, three
setups, all run together so the numbers compare.

| Setup | Words in the reply | Chatter while working |
|---|---|---|
| Claude Code on its own | 411 | 25 words |
| Its own built-in `Concise` style | 386 | 23 words |
| **With both older files** | **49** | **4 words** |

Two batches, two runs per job. Promising, not settled. Cost did not move either
way.

## How to use it

Same as the current one, with a different filename.

Save [`output-styles/hush-deprecated.md`](../../output-styles/hush-deprecated.md) into
the `.claude/output-styles/` folder inside your home folder, making that folder
if it is not there yet. Then open Claude Code and type:

```
/output-style
```

Pick **Hush (deprecated)** from the list.

You can keep both files side by side. They show up as two entries and you switch
between them whenever you want.

The paste-in installer at [`prompts/install.md`](../../prompts/install.md) only fetches
the current style. Getting this one is the copy above.
