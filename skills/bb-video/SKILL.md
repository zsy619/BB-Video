---
name: bb-video
description: 'Code-made videos (mp4; 16:9, 9:16, 1:1): narrated explainers and beat-synced promos for websites / apps, drawn in HTML/SVG/canvas, rendered in Chrome, sound synthesized in code, 33 styles (whiteboard, ink-wash, risograph, pixel, manga, trailer, map, blueprint…; full list in styles/INDEX.md). Use when asked for an "explainer video", "a promo for this site / app", a launch video, or in Chinese "做个科普视频" "白板动画" "讲个历史故事" "给这个网站 / App 做个宣传片" "产品发布视频", or when a video style is named (水墨, 像素, 日漫, 皮影戏, 剪报解说…). Outputs mp4 + cover + captions after automatic checks.'
---

# BB Video

Videos drawn in code: HTML + CSS / SVG / canvas frames, rendered frame by frame in Chrome, sound synthesized in code, assembled with ffmpeg. Every frame is the pure function `renderFrame(t)`, so any frame can be rendered on its own and every render is identical.

Relative paths in this file are relative to this skill's folder.

## One choice: the style

The style is the **only** thing the user picks. It decides how the video looks, moves and sounds — never what it says. If the user names a style, use it; otherwise recommend one from the topic or product and say why in the storyboard. When the user says "try another style", redo the same content in the new style.

The full style table (look, best for, explainer / promo support) is in `styles/INDEX.md`; read it before recommending a style. Styles: `whiteboard` · `riso` · `ink` · `launch` · `origami` · `pop` · `op` · `cutout` · `ascii` · `shadow` · `bauhaus` · `pixel` · `chat` · `toy3d` · `newsreel` · `filmroom` · `mystery` · `isometric` · `trailer` · `dossier` · `manga` · `map` · `swiss` · `lumen` · `blueprint` · `collage` · `grain` · `storybook` · `tapestry` · `parable` · `pencil` · `drawnworld` · `goldenage`.

**Stories with named people** follow `styles/CHARACTERS.md` in every style (fixed cast, people told apart at a glance, face-to-face scenes, relays); only whiteboard, ink, shadow, pixel, chat, manga, lumen and tapestry have characters — recommend one of them for character-driven stories (storybook has a cast too, but of original creatures and objects, never people; parable's stick figures are anonymous roles told apart by their labels; pencil's gesture figures are faceless and can stand for a real person only from behind or in silhouette; drawnworld's townspeople are small invented extras, never named people; goldenage's cast is an invented worker and an invented moon: slapstick is for invented characters only, never real people).

Each style lives in `styles/<name>/`: its `STYLE.md` (same outline for every style: `styles/TEMPLATE.md`, Chinese `styles/TEMPLATE.zh.md`) and its starter files. **Once a style is chosen, read its STYLE.md before writing any drawing code.**

Every style makes both kinds of video. Each style was born in one workflow and reaches the other through the common interface (`promo/runtime/kit.js`: the same verbs — big word, sentence, card, real screenshot, signature move, transition, end card — drawn in every style's own material):

| | Explainer (with a voiceover) | Promo (on the beat, no voiceover) |
|---|---|---|
| `whiteboard` `riso` `ink` | native: `node explainer/scripts/new.mjs <dir> --style …` — full board API, characters, formulas | `node promo/scripts/new.mjs <dir> --style … [--quick]` — the board runs inside a promo film: scenes are board regions / pages / scroll sections, transitions are pans / page changes / scroll moves |
| `launch` `origami` `pop` `op` `cutout` `ascii` `shadow` `bauhaus` `pixel` `chat` `toy3d` `newsreel` `filmroom` `mystery` `isometric` `trailer` `dossier` `manga` `map` `swiss` `lumen` `blueprint` `collage` `grain` `storybook` `tapestry` `parable` `pencil` `drawnworld` `goldenage` | `node explainer/scripts/new.mjs <dir> --style …` — creates a narration-paced project (voiceover + captions; cues timed to the narration, `promo/references/kit.md`, "Following the voiceover" | native: `node promo/scripts/new.mjs <dir> --style …` — the style's full component set |

The native side has the richest tools (whiteboard formulas and characters; origami folds; op lenses…); the other side covers what most videos need. When a video needs something only the native side has (a formula in a cut-out explainer, a folding animation in a riso promo), say so and offer the native style for that part.

## What kind of video: read it from the request

The user does not pick this; it follows from what they ask for.

| The user wants… | Workflow | Guide |
|---|---|---|
| To explain something: a phenomenon, a problem, a word, a concept, a story, a tutorial, the news — with a voiceover | **Explainer** (narration-driven: voiceover → timeline → visuals timed to each line; captions; background music) | `explainer/GUIDE.md` |
| To promote a product: a website, an app, software, a launch — no voiceover | **Promo** (beat-driven: research and fact sheet → visuals on the beat; synthesized score and sound effects; 16:9 + 9:16) | `promo/GUIDE.md` |

Any video can also be rendered square: `--aspect 1:1` (1080×1080, `out/video-1x1.mp4`), next to the default 16:9 and `--aspect 9:16`.

Both workflows run on one engine (`core/`): the same clock and runtime core, one renderer and one sound pipeline — voiceover, music and sound effects mixed together, music ducking under the voice, loudness normalized. So a promo can have a voiceover with captions (`promo/GUIDE.md`, step 5) and an explainer can cut on the beat (`EV.beats`). A project made with a style from the other side follows the workflow of the kind of video (explainer steps for an explainer), using the renderer of the engine that draws it (`promo/scripts/render.mjs` for the promo-born styles).

Before step 1, tell the user in one message: the workflow (explainer / promo), the style you recommend and why, the steps you will take and the files you will deliver; wait for their OK (skip only when they say "just make it"). Then read the matching guide and follow it from step 1. Inside each guide, how the story is told is also chosen for the user, not by them: explainer content modes in `explainer/references/modes.md`, promo tellings in `promo/references/structure.md`.

Paths inside a guide are relative to that guide's folder: run `node explainer/scripts/…` / `python3 explainer/scripts/tts.py …` for explainers and `node promo/scripts/…` for promos (from this skill's folder, or with the full path).

## Rules for every video

- **Facts first.** Verify facts, numbers and quotes before drawing; write them into the project's `brief.md`. Every piece of on-screen text must be traceable there.
- **Animation libraries are allowed** (GSAP, Three.js, Matter.js, opentype.js…), but time always comes from our clock: libraries are seeked to `t`, never run on their own clock or callbacks (`promo/references/motion.md`, "Animation libraries").
- **Self-review before delivering**: the automatic checks, the contact sheet / stills, and the frame check of the rendered video, as each guide describes.
- **Language**: unless told otherwise, the video is in the language the user writes in.
- **Scripts are black boxes**: run `scripts/*` with the documented flags (or `--help`); do not read or edit their source unless the task is changing the skill itself.
- **Never log in, never enter passwords** on sites you research; research is read-only.

## Adding a style

Write `styles/<name>/STYLE.md` from the template, its runtime, starter files and sound, in the workflow it runs on. For promo styles, also register the style's common interface in `promo/runtime/kit.js` (`promo/references/kit.md`). Add the style to `styles/INDEX.md` and to the style list above.
