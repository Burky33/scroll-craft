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

> **Status: private and pre-release (v0.1.0).** It works, and it has produced
> twelve finished sites. It is not yet portable to someone else's machine. See
> [Known gaps](#known-gaps) before handing it to anyone.

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

## Requirements

| Requirement | Why | Notes |
| --- | --- | --- |
| Node 18+ | every script | |
| **A full ffmpeg build** | encoding clips for scrubbing, extracting posters and seam frames | A stripped ffmpeg (some toolchains ship one with ~50 filters) fails in ways that read as syntax errors. Set `SCROLLCRAFT_FFMPEG` to override. |
| `playwright-core` + Chrome | the verification harness | `npm i playwright-core` in the build folder |
| `KIE_AI_API_KEY` | **only** if generating imagery | See `.env.example`. Bring-your-own-assets builds need no key and no spend. |

## What is in here

```
plugins/scrollcraft/
├── SKILL.md            the procedure: interview, grammar, score, build, verify
├── references/
│   ├── uniqueness.md   eight page grammars, the signature move, the fingerprint gate
│   ├── feel.md         the feeling curve, the engineered peak, the feel check
│   ├── devices.md      the nine scroll devices and the cue contract
│   ├── worldflight.md  continuous-world mode: one fixed stage, no seams
│   ├── worlds.md       art direction, and the style-preamble method
│   ├── taste.md        the taste floor: spacing, type, colour, depth, motion
│   ├── assets.md       generation, camera moves, encoding for scrubbing
│   ├── verify.md       the harness, and what it cannot tell you
│   └── template.html   a starting skeleton, not a layout
├── engine/             scrollcraft.js + .css. The mechanism. Never edited per project.
└── scripts/            kie.mjs, encode.sh, serve.mjs, shoot.mjs
```

`CHANGELOG.md` inside the plugin is worth reading on its own. It records what
broke on each build and the rule that came out of it, rather than a feature list.

`EXAMPLES.md` at the repo root is the fingerprint registry for the twelve builds
made so far: the grammar, nav, hero, act shape, close and signature move of
each. It is a record of shapes that are taken.

## The one rule that matters most

The engine is the mechanism and it is never edited per project. Theme it with
six colour tokens and two fonts, write your own semantic HTML, and drive
anything bespoke off the `--sc-p` custom property the engine publishes. A
runtime that builds the page from a config object is exactly why every site
built on one looks the same.

## Known gaps

Honest list of what stops this being handed to a student today.

1. **Paths assume the author's repo.** Nine references across `SKILL.md`,
   `uniqueness.md`, `feel.md` and `worldflight.md` point at
   `OtherWorlds/Ultimate Websites/`. The fingerprint gate and the worked
   worldflight rig both dead-end elsewhere.
2. **The fingerprint registry is personal.** `EXAMPLES.md` lists builds by one
   author. A new user should be gated against *their own* builds, starting from
   an empty registry, or they are blocked from grammars they have never used.
3. **No preflight.** There is no `doctor` script. Missing ffmpeg filters, an
   unset API key and a missing Chrome all currently fail deep inside a run with
   messages that point at the wrong cause.
4. **Cost is not surfaced early enough.** Generation spends real money. The
   bring-your-own-assets path works and should be documented as a first-class
   route rather than a footnote.
5. **No licence yet.** See below.

## Licence

**Not yet licensed. All rights reserved.**

This is deliberate, not an oversight. The engine is the reusable part and the
licence choice is a business decision: a permissive licence buys adoption, a
source-available one protects the asset. Pick one before this repo goes public.
