# The older Hush voice

flint ships two versions of the writing style. The current one is
[`output-styles/hush.md`](output-styles/hush.md), and it is the one the
[README](README.md) walks you through. This page is about the other one,
[`output-styles/hush-deprecated.md`](output-styles/hush-deprecated.md).

It still works. Nothing about it broke. If you had it and you liked it, you can
keep it, and this page tells you how.

## Why there is a newer one

The older style was written before Claude's newest models. It was built for a
model that needed to be told the same thing several ways, so it says a lot: 148
lines, six sections, and a fifteen-step checklist the model walks before it
sends anything.

The current style is 58 lines, written from scratch for the newer models. It is
not shorter for its own sake: the newer models followed the shorter file more
closely in testing, and the older one had grown long by repeating itself.

Both voices score about the same on the jobs we run. The current one writes a
little more than the older one did and says more with it, which is the trade
described below.

## What actually changed

| | Older | Current |
|---|---|---|
| Length of the style file | 148 lines | 58 lines |
| Reply limit | 6 lines, 60 words | 8 lines, 90 words |
| Sentence limit | 10 words | 8 words |
| Final check before sending | 15 steps | 1 step |

Four things the current one asks for that the older one did not:

1. Name the file you changed or found, as a link, so the reader can click it.
2. Say where things stand now, not only what you just did.
3. End on the next move, or say plainly that none is needed.
4. When the answer landed in a file, put the findings in the reply anyway, not a
   pointer to the file.

The reply limit went **up**, from 6 lines to 8. That is deliberate. The four
items above need room, and a reply that is too short to say what to open next
sends the reader back to ask.

## How the older numbers were measured

These are the numbers this style earned, kept here because the README now
carries the current one's.

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

Save [`output-styles/hush-deprecated.md`](output-styles/hush-deprecated.md) into
the `.claude/output-styles/` folder inside your home folder, making that folder
if it is not there yet. Then open Claude Code and type:

```
/output-style
```

Pick **Hush (deprecated)** from the list.

You can keep both files side by side. They show up as two entries and you switch
between them whenever you want.

The paste-in installer at [`prompts/install.md`](prompts/install.md) only fetches
the current style. Getting this one is the copy above.
