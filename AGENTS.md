# flint

flint is two text files for Claude Code, copied into place by hand or by a pasted prompt: an
output style and a `CLAUDE.md` fragment. There is no code, no install step and no test suite.
This repository is a Foundry submodule outside both marketplace catalogs.

## Start here

- Before changing either file, read [CONTRIBUTING.md](CONTRIBUTING.md): the fragment is measured, and a longer rule needs evidence.
- Before touching the older style, read [the older Hush voice](docs/knowledge/deprecated-style.md).

## Where things live

| Path | Content |
| --- | --- |
| `output-styles/hush.md` | The current writing voice; `hush-deprecated.md` beside it is the older one, still working |
| `fragments/razor-hush.md` | The `CLAUDE.md` fragment |
| `prompts/install.md`, `prompts/tune-for-opus5.md` | Messages a user pastes into Claude Code; the installer fetches the two files from this repository's `main` by raw URL |
| `docs/knowledge/` | The page about the older voice |
| `assets/` | Banner and logo artwork for the README |

## Conventions

Rules are written for a model reading them cold: name the moment the rule fires, then the
action, and pair every prohibition with what to do instead. Short sentences, everyday words.
A README number comes only from a measured run. `CLAUDE.md` imports this file.

## Pitfalls

- **The installer fetches from `main` by raw URL.** A rename under `output-styles/` or `fragments/` breaks `prompts/install.md` until that prompt is updated.
- **The style and the fragment are copies of hush's voice and razor's checklist.** Change them here only to keep the copies faithful; one plugin-only line is deliberately dropped.
