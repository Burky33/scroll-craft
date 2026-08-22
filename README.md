# scrollcraft

A Claude Code skill for building premium, scroll-driven interactive landing
pages. Scroll becomes the timeline: video scrubs frame by frame under the wheel,
sections pin and advance, rails pan sideways, headlines assemble line by line,
and the pointer moves things that are not scrolling.

It is not a template. The skill interviews you first, then picks a **page
grammar** and invents a **signature move**, and it runs a fingerprint gate to
prove the result is not a re-skin of something already built.

Every build ends by verifying itself: a headless browser walks the page at every
scroll position, waits for the playhead to settle, and reports dead scroll, cues
that never reach full opacity, and text contrast measured on the composited page
at the brightest frame under each line.

> **Status: private, v0.2.0.** It works, it has produced twelve finished sites,
> and as of this version it is portable: nothing assumes the author's machine.

---

## Install

```bash
/plugin marketplace add nateherkai/scroll-craft
```
```bash
/plugin install scrollcraft@nateherk
```

If the install summary says `Run /reload-plugins to activate.`, run that.

To work on the skill itself without installing:

```bash
claude --plugin-dir ./plugins/scrollcraft
```

## First run

```bash
node scripts/doctor.mjs            # preflight: tells you exactly what is missing
node scripts/workspace.mjs --ensure  # creates your workspace and an empty registry
```

`doctor` is worth running before anything else. The three most common setup
faults all surface later as misleading errors otherwise: a stripped ffmpeg
reports a missing filter as a syntax error in your command, a missing webp muxer
reports as a bad filename, and `playwright-core` resolves from the wrong
directory.

## The workspace

Your builds and your fingerprint registry live in one directory, resolved rather
than assumed. First hit wins:

1. `SCROLLCRAFT_HOME`
2. the nearest `.scrollcraft.json` walking up from the current directory:
   ```json
   { "workspace": "path/to/wherever/you/keep/builds" }
   ```
3. `<project root>/scrollcraft`, where the project root is the nearest ancestor
   holding a `.git`

So a build lands in `<workspace>/builds/<name>/` and your registry is
`<workspace>/FINGERPRINTS.md`. If you already keep builds somewhere, point a
`.scrollcraft.json` at it and nothing moves.

**Your registry starts empty, and that is correct.** The fingerprint gate exists
to stop you repeating *yourself*. Your first build has nothing to clear; every
build after it does. `EXAMPLES.md` in this repo is the author's twelve-row
table, included so you can see what a filled registry looks like and which
shapes tend to collide. It is illustration, not constraint.

## Requirements

| Requirement | Why | Notes |
| --- | --- | --- |
| Node 18+ | every script | |
| **A full ffmpeg build** | encoding clips for scrubbing, extracting posters and seam frames | A stripped ffmpeg (some toolchains ship one with ~50 filters) fails in ways that read as syntax errors. `doctor` finds a real build if one exists; `SCROLLCRAFT_FFMPEG` overrides. |
| `playwright-core` + Chrome | the verification harness | `npm i playwright-core` **in the build folder** |
| `KIE_AI_API_KEY` | **only** if generating imagery | See `.env.example`. A build from your own photos and footage needs no key and no spend, and it is a first-class route. |

## The one rule that matters most

The engine is the mechanism and it is never edited per project. Theme it with
six colour tokens and two fonts, write your own semantic HTML, and drive
anything bespoke off the `--sc-p` custom property the engine publishes. A
runtime that builds the page from a config object is exactly why every site
built on one looks the same.

## Known gaps

Fixed in v0.2.0: hardcoded author paths, the shared fingerprint registry, the
missing preflight, and the unshipped worldflight rig.

Still open:

1. **No licence yet.** See below.
2. **Windows-first path guesses.** `doctor` and `encode.sh` look for ffmpeg in
   WinGet, Homebrew and `/usr/local` locations. Other setups need
   `SCROLLCRAFT_FFMPEG`.
3. **Not tested on macOS or Linux.** Every build so far ran on Windows.

## Licence

**Not yet licensed. All rights reserved.**

This is deliberate, not an oversight. The engine is the reusable part and the
licence choice is a business decision: a permissive licence buys adoption, a
source-available one protects the asset. Pick one before this repo goes public.
