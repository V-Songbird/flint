Set up flint for me in this project. Do the work yourself, but stop twice for my yes: before you save the style file and before you write to `CLAUDE.md`. Tell me at the end what changed.

Fetch these two files:

- https://raw.githubusercontent.com/V-Songbird/flint/main/output-styles/hush.md
- https://raw.githubusercontent.com/V-Songbird/flint/main/fragments/razor-hush.md

Then do four things.

Save the first file, unchanged, to `.claude/output-styles/hush.md` inside my home folder. Create the folder if it is not there. If no file is at that path yet, show me the whole file exactly as it will land and save it only after I say yes. If a file is already at that path, show me both versions and ask before replacing it.

Add the second file's contents to the end of `CLAUDE.md` in this project. If there is no `CLAUDE.md`, create one holding just that. If any rule in it contradicts a rule already in my `CLAUDE.md`, stop and show me the pair rather than stacking both. Before you write to `CLAUDE.md`, show me the change as a diff, every added line exactly as it will land, and write it only after I say yes.

Check that `.claude/settings.json` in this project does not already pin a different output style. If it does, say which one and leave it alone.

Then tell me, in one short message: which files you wrote, whether anything was already there, and that the last step is mine — I run `/output-style` and pick **Hush**.

Change nothing else. No new folders, no config beyond what is named above, and no edits to my source code.
