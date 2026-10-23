---
name: "kids-science-video"
description: "Make a narrated children's science or read-along video from a picture book: fact-checked script, accurate code animation, TTS voiceover, MP4 delivery. Any topic; loads a topic reference on demand."
---

# Children's companion video from a picture book (core)

This skill makes a 1080p MP4 that goes with a children's nonfiction book, plus SRT captions, a source zip, a photo-vs-drawing comparison and a README.
- Every picture is drawn by code.
- Narration comes from the user's TTS MCP.
- Captions highlight word by word.

**The bar:** scientifically correct, matched to the book, readable by a 3–6 year old, natural-looking, free of technical defects.
- Beautiful but wrong is a failure.
- Correct but stiff or "code-like" (代码化) is also a failure.
- This applies to everything on screen: props, backdrops and motion are judged as hard as the hero.

## 0. Standing defaults (don't ask again)
- **Book text.** The user owns the right to use the text of the books they supply. Read every book sentence **verbatim**, plus ONE extension sentence per scene: simple, concrete and sourced, 5–8 words for the earliest readers and at most about 12. Match the book's reading level (words per page, F&P level), and make every word visible on screen.
- **Don't stop for sign-off.** Script, voice and style are settled; just build and deliver. Ask only if something is truly ambiguous and expensive to redo.
- **TTS:** relay `gemini-tts-relay`, model `gemini-3.1-flash-tts-preview`, voice **Sulafat**, natural pace. If the user names another route, model or voice, use theirs. Never mix voices or models within one film.
- **Mix:** voice only. No ambience bed: it was heard as hiss.
- **Style:** the house style (clean picture-book vector shapes, real form and light, soft gradients, contact shadows, restrained texture). Don't switch styles unprompted. If asked for a new look, show stills of one scene first.
- **Delivery folder:** `Documents\TTS-Output\<Book>\` on the user's computer, and send the files in chat.

## 1. Get the kit, pick the topic reference (do this first)
- **The video kit** lives on the user's computer at `Documents\TTS-Output\_video-kit\video-kit.zip` (always the latest version; older `video-kit_vN.zip` files are rollback points). Stage and unzip it.
  - `template/`: a working project. Copy it per book.
  - `library/`: finished rigs and backdrops.
  - `CATALOG.md`: every function signature.
  - `README.md`: the 8-step pipeline.
  - `tools/`: kit maintenance (see §9).
  - Never rebuild the engine, audio or render scripts from scratch.
  - If the zip is missing, ask the user to attach it.
- **Load exactly one topic reference** (Skill tool) before drawing:

| Book topic | Load |
|---|---|
| animals, sea life, plants, habitats, deserts, weather, geography | `kids-video-animals` |
| trucks, construction or rescue machines, vehicles, trains, planes, boats, tools, how machines work | `kids-video-machines` |
| anything else (human body, space, everyday life, history…) | neither; follow §3 strictly, and propose a new topic reference at wrap-up (§9) |

- **One book per session.** Long sessions re-read their history every step and cost far more. Start the next book in a new session with a 5-line handoff: kit location, which library files to reuse, the user's standing defaults.

## 2. Read the book, script, word-to-picture map
- **Read every page yourself** (spread images or PDF pages, batched into a few contact images).
  - Record the exact sentences, the key word per page, what each photo shows, and the book's label words and colours.
  - Note the parts or "types of" page.
- **Scenes.** One knowledge point per scene. Write `script.json` (clip id → sentence) and `scenes.json`.
  - Split question-and-answer lines into separate clips.
- **Fact-check every extension sentence** with a web search against authoritative sources (encyclopedias, agencies, universities, manufacturers).
  - Record the URLs for the README.
  - Prefer careful wording ("many", "often") to sweeping claims. Give numbers only with a source.
  - No jargon for early readers: show it instead.
- **Check the book's photos against its text.** If a stock photo contradicts the sentence, draw what is true and tell the user.
- **Word-to-picture map (图文对应, required).**
  - For each sentence, the concrete thing that appears or changes **as each content word is spoken**.
  - Abstract words become visible: a thermometer, an arrow, a counter, a calendar, a question bubble, a cut-away.
  - Scenes read word times via `S.at(clip,'word')`. Never hard-code seconds.
- **Motion that carries the science** (breathing, swimming, a winch pulling, a blade rolling snow): write the phases in order from a source before animating: what moves, where the pivot is, which way things flow. Slow it for children and say so.
- **"Why/how" sentences get a dedicated explainer shot** (a cut-away, a front or top view, an animated panel), not a tiny icon.

## 3. Drawing rules (all topics)
- **No reference, no drawing.** Every hero object and large prop is traced from a photo (the book's own photo first) or reused from the library. Drawing from memory is the main cause of code-like pictures.
- **Finding photos.** Use the built-in browser on the user's computer; the workspace can't reach image sites.
  1. Open `https://www.bing.com/robots.txt`.
  2. In `javascript_tool`, `fetch('/images/search?q=…&qft=+filterui:imagesize-large')` and parse with `DOMParser`.
  3. Read `a.iusc[m]`; its JSON holds `turl` (thumbnail) and `murl` (full image).
  4. Grid about 20 thumbnails in one screenshot.
  5. View one large with `turl+'&w=1400&h=1400&c=7&rs=1'`.
  6. Crop it, save it to `ref/`, draw a grid overlay on it, and trace from that.

  Photos are study references only; never put photo pixels in the film.
- **Parts grow, never paste.** One outline per body; parts join with no seam lines. One consistent light: lit side, shade, rim light, contact shadow.
- **Texture is quiet.** You notice the object, not the strokes.
- **"Code-like" checklist.** Fix any of these before rendering:
  - ruled lines or perfect circles as organic parts;
  - evenly spaced repeats;
  - flat fills with hard outlines everywhere;
  - floating objects (no contact shadow);
  - seams where parts meet;
  - texture that ignores the form;
  - crude block shapes for machines.
- **Physics and attachment.**
  - Things that act on each other touch: cables end on a real hook point, loads sit on the deck, wheels sit on the ground.
  - Parts of one body move together (the same shake or offset for every attached piece).
  - Vehicles and animals face the direction they travel.
- **People:** detailed (face, hair or hat, clothes, hands), in real scale to the objects.
- **On-screen text:**
  - the caption bar (exactly one word lit at a time);
  - the keyword chip;
  - the book's own labels, in the book's colours, never under the caption bar;
  - short exclamations taken from the narration ("Oh, no!", "Cool!").

  Named-part highlights use one warm yellow glow.
- **Framing.**
  - Keep the hero large: no wide shots that make it a speck.
  - Camera pushes and follows must not crop it; check the first and last frame of every camera move.

## 4. Narration (TTS MCP on the user's computer)
- **Generating.** `tts_batch` with 4 items per call. Run several calls in parallel.
  - Use one fixed `character` persona for the whole film, e.g. "A warm, friendly teacher reading a picture book aloud to young children."
  - Keep each `direction` short ("Warm and clear, natural pace.", "Excited.").
  - Never put directions in the line text.
- **Timeouts.** If a call times out, **do not resend it**: `device_list_dir` the project folder and re-call only for missing WAVs (existing ones return cached). On HTTP 429, wait the stated time.
- **Staging.** Stage WAVs with `device_stage_files`, at most about 21 files per call. After a desktop-app restart, re-request folder access first.
- **Route B (`gemini-tts`, official 3.8).** Only if the user asks.
  - The server must be v1.2 or later: the text is sent verbatim, notes go in annotations, the RIFF/C2PA chunk is stripped, and it writes `_gemini_tts.log`.
  - If a raw clip starts with `RIFF` or contains `C2PA`, fix the server; don't patch the clips.
- **Audio QA.** `audio_clean.py` does integrity, trim, normalise (−18 dBFS speech RMS, −1 dBFS peak) and word timing.
  - Read its printout: flag `LONG` clips (extra speech) and any `lastpk` spike, and regenerate those lines.
  - You can't listen: tell the user to listen once.
- **Numbers.** Digits are read out in full ("1,500" takes about 1.9 s). Keep the digits in the caption and give that token a large syllable weight (about 6) in `audio_clean.py`.
- **3.8 sound tags.** Route B 3.8 accepts inline `<short pause>`, `<long pause>`, `<breath>`, `<laugh>` and `<sigh>` when wanted.
- **Sound effects** (if any) stay at least 12 dB under the speech peak; a chime louder than the narration was rejected.
- **Picture-only changes never need new narration.**

## 5. Pacing (built into `timeline.py`)
- Hold each scene about 2.7 s after its last word (title about 3.4 s, last scene about 3.6 s).
- Crossfade 1.3 s, then settle 1.8 s before the first word: about 3.3 s of silence between knowledge points.
- Within a scene, 0.8 s between sentences, plus 0.3–1.0 s before a new fact or an answer (`extra` in `scenes.json`).
- A 10–12 scene film runs about 3:00–3:15.

## 6. Build, preview, render (cheap loop)
- **After every edit:** `./build.sh` (prints `BUILD_OK`).
- **Preview.** `timeout 115 node multi.js out/sheet.jpg 0:4 1:3.5 …` renders 4–10 frames in ONE browser, then `Read` the sheet.
  - Never chain several preview calls in one command: they hit the 2-minute timeout.
- **Before the full render, preview every scene** at its key words (each mapped word, each camera end-point). Fix there, not after rendering.
- **Render.** `nohup ./render_all.sh > out/render.log 2>&1 &`. Poll the log for `RENDER_ALL_DONE`.
- **After a fix,** re-render only the affected 500-frame chunks: frame = seconds × 30, chunk = frame ÷ 500.
- **Finish.** `python3 mix.py`, then `./encode.sh` (two-pass, the same `-vf tpad` in both passes, auto bitrate under 20 MB).
- **Pitfalls.**
  - Clamp radii (an easing overshoot gives a negative arc and kills the render).
  - Change a sprite's cache key after editing it.
  - Colour helpers return `#rrggbb`.
  - Backdrops extend beyond the frame on pans.
  - For drawing work, use a temporary test scene that shows the object alone at 2–3x on a plain background. Delete it before the final build.
  - Don't wait on a job with `pgrep -f <name>` from a command that contains that name: it matches itself. Poll the log instead.
  - Fonts are embedded in `src/font.css`; Google Fonts and npm are usually unreachable.

## 7. QA before delivery
- **Side-by-side.** Book photo vs drawing for every new hero object, saved as `out/*_compare.jpg`.
- **Contact sheet.** One frame per scene, plus frames pulled from the final MP4 at the changed scenes and the camera moves. Check:
  - right species or machine and its parts; nothing reads as a different thing;
  - attachments touch, nothing floats, parts move together, facing direction is right;
  - mechanism shots are clear to a 4-year-old;
  - labels don't overlap the caption or each other; one caption word lit;
  - nothing cropped.
- **Joins and small things at full resolution.** Crop every join (tail or ear roots, boom pins, hitches, cable ends) at two or three animation phases. Thumbnails hide seams and floating hooks.
- **Science in motion.** Render a strip of 6–9 frames across one cycle and check the phase order.
- **Independent critic (optional).** For high-stakes visuals, give a separate agent only the photo and the frame, and ask what looks wrong or code-made.

## 8. Delivery
- **MP4.** Send it with `SendUserFile`, and commit it to `Documents\TTS-Output\<Book>\` (`device_commit_files`, **one MP4 per call**, `force: true` when overwriting).
- **Also deliver:**
  - the SRT;
  - the compare image;
  - a README (extension sentences + sources, labels used, what was drawn from which photo, where the picture departs from the book);
  - a source zip (`src`, scripts, JSON, cleaned WAVs, `ref`).
- **Tell the user, one line each:**
  - the TTS route, model and voice;
  - what you could not verify (you cannot listen);
  - which photo each new drawing was traced from.
- **On a revision,** say which time range changed and re-send the MP4.
- **If the user rejects a commit,** don't retry it unless asked.

## 9. Wrap-up: grow the kit and the skill (after the user accepts the film)
Do this once per book, only after the user is happy with the final film. Never do it mid-revision.
1. **Library.** For each new reusable piece (a machine, an animal, a person, a backdrop, an explainer panel), make one self-contained file `library/<name>.js`.
   - Its first line is `// ===== <what it is> (<book>, p.<pages>) =====`.
   - It has a `drawX(g, P)` entry and geometry or attachment helpers.
   - Don't copy one-off scene code. Prefix any helper name that could clash (`rlTyre`, not `tyre`).
2. **Publish a new kit version** from the unzipped kit: `tools/publish_kit.sh "<book>: added <names>"`. It refuses to publish on any error. In order, it:
   - checks for duplicate names;
   - builds and loads a page with every library file;
   - regenerates `CATALOG.md`;
   - bumps `VERSION`;
   - appends `CHANGELOG.md`;
   - writes `video-kit_vN.zip` and `video-kit.zip`.

   Fix any error and run it again. Then commit both zips to `Documents\TTS-Output\_video-kit\` (one file per call). Old `_vN` zips stay as rollback points.
3. **Lessons.** List every correction the user asked for on this book, one line each (what was wrong → the rule). Then call `propose_skills` to update the matching topic reference's "mistakes" list.
   - If a book topic had no reference, propose a new reference skill instead (`kids-video-<topic>`).
   - The user approves the card; never claim the skill changed until they have.
4. **Tell the user in two lines:** the new kit version and what was added, and that a skill-update card is waiting.

## 10. Cost rules
- One book per session, using the kit, with only the topic reference you need.
- Preview stills before rendering; re-render chunks, not the film.
- Batch tool calls (TTS 4 per call in parallel; stage ≤21 files; one large file per commit).
- Every piece of user feedback becomes one line in the topic reference's "mistakes" list, so it never needs saying twice.