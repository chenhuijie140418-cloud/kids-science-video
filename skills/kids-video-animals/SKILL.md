---
name: "kids-video-animals"
description: "Topic reference for kids-science-video when the book is about animals, sea life, plants, habitats, deserts, weather or geography: species accuracy, anatomy, texture, props, motion."
---

# Animals, plants & nature: reference for kids-science-video

Read this when the book is about animals, sea life, plants, habitats, deserts, weather or geography. The core skill's workflow, TTS, rendering and delivery rules still apply. This file adds the drawing and science rules for living things and landscapes.

## 1. Reference sheet: species, pose, partners, photo-vs-text conflicts
For each book page record:
- **The basics:** the exact sentence, the key word, and the animal/species or object shown.
- **Visible features that matter:** ear shape and position, nose, eye size, fin shape, colour pattern, posture.
- **Pose and behaviour shown:** e.g. curled asleep with the tail over the face, tail coiled, head down grazing. The pose is part of the science.
- **Scene mapping:** which video scene each page maps to.
  - Each scene's animal must be the same kind of animal as on its book page: the same species, or at least the same family with the same key features.
  - Never swap in a different kind because it is easier to draw or "fits the sentence". Example: the book's leaf-nosed bat must not become a long-eared bat.
- **Supporting organisms and habitat too.** Identify what else is in the scene and pick the real partner species and props.
  - A tomato clownfish's host is a bubble-tip anemone.
  - Anemonefish eggs are a dense, same-oriented clutch on rock beside the anemone.
  - A dead log on cracked mud is weathered, bleached wood.
  - Background props are part of the science.
- **Check the book's photos against its own sentences.** Stock photos are often wrong for the text. Read the sentence verbatim, draw what is true, and tell the user what you changed and why. Examples:
  - Remoras on a page about fish that "nibble a shark's skin": the real cleaner is a cleaner wrasse.
  - A freshwater aquarium plant on a coral-reef "plants" page: draw seagrass instead.
  - Seaweed for "algae give coral its colour": the colour comes from zooxanthellae inside the coral tissue.
  - Meerkats in front of American cactuses: meerkats live in southern Africa.
  - A tundra valley for "some deserts are cold": show a real cold desert instead.

## 2. Physiology in motion: write the sequence before animating
- **Physiology in motion: write the sequence down before animating.** For every body process the video shows (breathing, swimming, wingbeat, feeding, hatching), write down from a source:
  - the phases in order;
  - what moves, and where the hinge or pivot is;
  - which way air or water flows;
  - what is closed while something else is open.

  Example, bony-fish breathing (buccal–opercular pump):
  1. The mouth opens and the gill cover stays shut, so water is drawn in.
  2. The mouth shuts and the gill cover swings out from its front/top hinge, with the slit widest at the bottom. Water is pushed over the gills and out backwards along the flank.

  The two phases never overlap. Slow the motion for children and say so.

## 2b. Tracing and measuring from the photo
**Trace, don't guess:**
- **Trace key outlines from the grid photo.** For each animal or prop, trace point by point into the sprite code: body, head, ears, tail, legs, prop outline.
  - Write the mapping in a comment, e.g. `// traced from ref/bilby.jpg: local = (refx - 380, refy - 415)`. Mirror the photo when the animal must face the other way.
  - The trace gives the proportions and the silhouette. The house style (§3 here) decides the surface.
- **Measure landmarks in a body unit and write them down.**
  - **Faces:** use eye-spacing units (distance between the eyes = 1; eye-line centre = origin). Record eye radius, ear tips, ear bases (medial/lateral), tragus, nose leaf top/bottom, nostrils, mouth, chin, cheek width and shoulder width.
  - **Fish:** use standard length (snout = +0.5, tail base = −0.5). Record eye position and size, mouth end relative to the eye, gill-cover edge, body depth, and fin bases and heights.
- **Also study the material:** how light falls on it (bleached top, dark underside), which way the grain or fur runs, where the cracks are, how it meets the ground.
- **Keep references in the source.** Put them in `ref/` in the source zip. Tell the user which photo and species each drawing was based on. If something was drawn without a photo, say so.

## 3. Style for living things: texture, joins, pose
**House style: clean picture-book shapes, real form and light.** Smooth vector shapes, soft gradients, gentle rim light from the sun or moon, soft shadows, a bokeh background and a slow camera push. Realism comes from (in this order of importance):
1. **Silhouette and proportions** traced from a photo (§2b).
2. **Structure:** parts that grow out of each other with no seams (below).
3. **One consistent light:** a lit side and a shaded side, occlusion underneath, a rim light, and a contact shadow where it touches the ground.
4. **Surface texture.** Last, and kept restrained.

Every scene, including close-ups, must look like it belongs to the same film. Use 3D (see §9) for shots where motion or structure matters most: flight, the wing skeleton, orbiting a model.

**Surface texture: allowed, but never the main thing.** Fur strokes, scales and wood grain are fine. What the user rejected was a close-up where hair strokes dominated: thousands of long, high-contrast strands made it hairy, messy and semi-real. The fix is restraint, not a ban:
- **The viewer must notice the animal, not the hair.** At final display size, texture should read as a soft surface quality. If the first thing you notice is the strokes, there is too much.
- **Fur strokes:**
  - short;
  - low contrast (alpha about .2–.4, colour within about ±15% of the local base colour);
  - following the direction the fur grows;
  - denser along the silhouette and at colour boundaries, sparse on large flat areas.
- **Outline hairs:** short and soft. No long stray strands all round the outline (a halo or comb). Strands placed only at the sampled outline points line up like comb teeth: scatter them along the segments between points.
- **Fluffy parts** (a fox tail, a bushy cheek) are carried mainly by a soft, uneven outline and soft light. Only then add strokes.
- **Reptile skin:** small, low-contrast granules plus a few soft blotches and bands. Never a net of outlined rings: that looked fake.
- **Wood:** grain bands along the trunk that bend round knots, long dark splits with a pale lip, and weathered patches. Not a flat fill with a few arcs.
- **No random noise unrelated to structure.** Every texture follows the form: the fur flow, the spine, the trunk axis.

**Parts grow, never paste.** These are the most frequent complaints: ears that look stuck on, a tail like a stick out of the body, humps like caps, legs like blocks laid on top.
- **Body and tail are ONE outline.** The back line runs on, unbroken, into the tail. The underside of the tail blends back into the rump with a smooth curve, not a corner. Build the tail as a centre line plus a width function, and splice its upper and lower edges into the body outline.
- **Ears, horns and stubs** sit behind the head or body. Their roots then **fade into it**: draw the part again on top through a gradient mask that goes from transparent at the root to opaque. Then there is no seam line where it joins.
- **Legs:** upper legs go behind the body, and lower legs come out from under it.
- **Humps and swellings** are shaped by soft radial shading inside the body outline, not by a separate shape with an edge.
- **One outline per part.** A hard outline stroke drawn around every part makes each joint look like a seam.

**Pose follows behaviour (from the photo).** A sleeping fennec is curled, with its big ears lying back, the head resting and its bushy tail wrapped round and over the muzzle. A chameleon's tail tapers evenly and rolls into a tightening (logarithmic) spiral that coils under itself.

**Props from photos too.** Examples:
- **Dead log:** a cylinder shaded across its width (bleached silver-tan on top, warm brown below). It has grain and splits, a knot, a broken (not sawn) end with rings, broken branch stubs, and a contact shadow so it rests on the ground.
- **Dead desert tree:** a short, thick, twisted trunk splitting into crooked branches that reach upward and taper to sharp tips. The roots splay at the base. Farther trees are smaller and paler.

## 4. Close-up recipe for mammals
**Close-up recipe for mammals** (canvas 2D, units = eye spacing):
- **Draw order:** ears → body+head → face lighting → nose/mouth → eyes.
  - The ears go *behind* the head, so the head covers the ear bases. Then redraw the ear roots faded into the head.
  - Ears are separate sprites, so they can twitch around a pivot at the base.
- **Head and body are one silhouette:** the cheeks flow straight into broad shoulders. A round head sitting on a separate body reads as a ball on a bean, or as a rabbit or owl.
- **Separate the face from the body with light, not lines.** Use a warm radial highlight on the face, a very soft blurred jaw shadow (blur ≥ 9 px, alpha ≤ 0.25) and a small pale throat patch. Hard dark jaw lines read as wrinkles or a moustache.
- **Ears:** a translucent grey-brown membrane with a warm glow only near the base, ribs as faint thin lines, and a thin light rim on the moon side. Strong pink, a bright white tragus and radiator-regular ribs read as rabbit ears. Fennec ears are broad triangles with a pale inner side and soft white hairs.
- **Nose leaf:** a muted brownish pink close to the muzzle colour, with a faint midline highlight. Saturated pink with a hard highlight reads as a rhino horn.
- **Eyes:**
  - exactly the measured size ("small eyes" must stay small);
  - glossy dark, with one big and one tiny highlight;
  - blink every few seconds;
  - keep breathing and ear twitches subtle.
- **Eye patches** (meerkats etc.) are soft-edged, brown, teardrop-shaped and slant toward the nose. Black discs read as sunglasses.
- **Tools:** a Catmull-Rom helper for outlines, `ctx.filter='blur(npx)'` for soft shadows, and each part pre-rendered as a sprite at 2–3x.
- **Texture library:** keep these helpers in one file and reuse them across books:
  - `furShape(g, pts, {base, light, dark, len, edgeLen, w, flow, seed})`: a lit gradient, short strokes following the flow, occlusion and soft outline hairs;
  - `scaleShape(g, pts, {base, light, dark, scale, blotch, over})`;
  - `limbTube`;
  - a root-fade helper (offscreen canvas + `destination-in` gradient);
  - a contact-shadow helper.

  Tune `len`, the alpha and the stroke count down until the texture is quiet at display size.

## 5. Fish recipe
**Fish recipe** (parametric, reusable for every species):
- **Body.** A dorsal/ventral profile table in standard-length units.
- **Fins.** Base points on the body plus tip points, drawn with a gradient membrane and faint rays.
  - Fin height ramps up from zero at its origin: no boxy vertical leading edge.
  - Give the tail from the peduncle the species' real spread: forked, truncate, rounded, lyre or halfmoon; heterocercal for sharks.
  - Pelvic and pectoral fins must be wide enough to read as fins, not drips or thorns.
- **Body, caudal peduncle and tail are ONE continuous outline path.**
  - Never paste the tail on as a separate sprite: any scale or offset opens a visible seam, and the tail looks broken off (user complaint on a shark).
  - The side-view tail beat = foreshorten the tail's own outline points around the caudal-fin base inside the same path.
  - Belly and countershading patches taper to a point along the body edge. Never end them with a blunt cut.
- **Motion.**
  - Body undulation grows from a stiff head to the tail.
  - Pectorals flutter around their base.
  - Everything passes through the same deform, so the fins stay attached.
- **Patterns and scales.** Spots, bars and stripes are clipped to the body and deformed with it. Scales are overlapping arcs whose free edge points to the tail, kept very faint.
- **Head.** A gill-cover line, a real eye (iris colour per species), and a mouth whose gape matches the species (a largemouth bass jaw reaches past the eye).

## 6. Anatomy checklist per animal
**Anatomy checklist per animal,** written down before drawing:
- **Bats.**
  - Wing = hand: a thumb with a claw, free on the leading edge, plus 4 long fingers (digits 2–5) holding the skin.
  - The skin runs to the ankle, with a tail membrane between the legs.
  - Horseshoe bat: horseshoe nose leaf, pointed ridged ears, no tragus, wraps its wings when roosting.
  - Leaf-nosed bats (Phyllostomidae, e.g. the California leaf-nosed bat *Macrotus*): spear nose leaf, large ribbed ears joined low on the forehead, long tragus, small eyes.
  - Flying fox: dog-like face, big eyes, no nose leaf.
  - Long-eared bat: very long ears with a tragus.
  - Hanging: by the feet, head down, toe claws hooked on rock.
- **Fish.**
  - Fin count and position: spiny vs soft dorsal, adipose fin in tetras, thoracic pelvics in bass.
  - Tail shape, gill cover and gill slit, colour pattern.
  - Translucent species show the vertebrae, ribs, neural spines, the spinal cord above the vertebral bodies, and the swim bladder.
- **Sharks.** 5 gill slits; a larger upper tail lobe with a terminal lobe and notch; the species' fin markings (grey reef shark: plain first dorsal, broad black caudal margin).
- **Scale pairs use real sizes.** Diver ≈ 1.75 m vs whale shark ≈ 10 m; cleaner wrasse ≈ 10 cm vs reef shark ≈ 1.5–1.8 m.
- **Corals.**
  - A stony-coral polyp is a soft tube in a limestone cup with radiating septa, a mouth on the oral disc and a ring of tentacles (multiples of six).
  - View it slightly from above, so the ring reads as a ring.
  - Zooxanthellae are golden-brown cells inside the tissue.
- **Octopus.** 8 arms with suckers underneath; a horizontal slit pupil; the mantle behind the head (the day octopus has an eyespot).
- **Sea turtle.** Long front flippers that beat together; the carapace scutes drawn as plates.
- **Camels.** Bactrian = two humps, a shaggy winter coat on the humps, neck and upper legs. Camels pace (both legs on one side move together).
- **Chameleons.**
  - A tall, flattened body with a high arched back and a casque on the head.
  - Turret eyes that move independently; pincer feet.
  - A tail that continues the back line and coils into a spiral.
- **Lizards** (monitor, chuckwalla): the skin follows the spine as it bends. Chuckwalla: thick, banded tail; loose neck folds.

## 7. Props and habitat (coral, anemones, rock, logs, trees, eggs, small organisms, maps)
**Props and habitat** (same care as the hero):
- Never straight sticks, flat blobs or ruled lines. Coral, anemone, rock, logs and trees need structure and texture from a photo.
- **Build them once as cached sprites:**
  - **Brain coral:**
    - a dome with evenly spaced **meandering ridges**, made with a Gray-Scott reaction-diffusion maze (F≈.029, k≈.057);
    - clamp the values to 0..1, or it can blow up to NaN and give a black dome;
    - add a small emboss, light from above and occlusion at the base;
    - warped sine/noise stripes read as zebra or camouflage.
  - **Staghorn/branching coral:** tapered connected tubes, rounded joints, pale growing tips, small light corallite bumps.
  - **Soft coral (Dendronephthya):** a visible translucent stalk and branches with red sclerites. The polyp clusters are separate balls with gaps; one merged mass reads as a bush.
  - **Gorgonian sea fan:** fine branching, plus a mesh only inside the branch envelope.
  - **Bubble-tip / magnificent anemone:** many soft tapered tentacles in 3 depth rings fanning outward, swelling near the tip, with a gentle travelling sway.
  - **Reef rock:**
    - a smooth lumpy outline (no spiky peaks);
    - mottling, small pink coralline patches, green turf and pits;
    - light from the top.
  - **Logs and dead trees:** see above.
  - **Depth:** add a hazy far-reef or far-horizon silhouette layer.
- **Egg clutches:** hex-packed, all the same orientation, with embryo eyes visible when the camera pushes in.
- **Small organisms must be recognisable or not drawn.** This covers parasites, plankton, algae cells and eggs.
  - Draw their true shape at a readable size: e.g. a shark copepod = a shield body plus two trailing egg strings.
  - Use only a few.
  - Introduce them with the warm glow ring at the moment the narration is about them (e.g. ring each parasite just before the cleaner fish eats it).
  - Unexplained brown specks made the user ask "what are these dots?".
- **Maps.** The workspace cannot download geodata.
  - Load Natural Earth via the built-in browser (`https://cdn.jsdelivr.net/npm/world-atlas@2/land-110m.json`) and decode the TopoJSON in `javascript_tool`.
  - Simplify with RDP on closed rings. The first point equals the last point, so measure distance to the start point when the chord length is 0.
  - Round to 0.5° and return about 18 KB of JSON to paste into the source.
  - When drawing, unwrap longitudes per polygon and draw copies at ±360°, so no seam line crosses the map.
  - Skip Antarctica unless the book needs it.
  - Make overlay bands wider than the map, so zooms show no edges.

## 8. Motion
**Motion checklist:**
- **The head always leads the direction of travel.** A front-view flying sprite must not be translated sideways. Use a view (e.g. seen from below) and rotate the body to the path tangent, or use 3D.
- **Wingbeats.**
  - Extended on the downstroke, partly folded on the upstroke.
  - Real small bats beat roughly 8–12 times per second. Slow it to about 4–5 per second so children can see it, and say so if asked.
- **Physiology** follows the written sequence from §2 (e.g. the mouth and gill cover alternate; water goes in at the mouth and out behind the gill cover).
- **Camera pushes must not crop the hero.** Move other animals out of the way as the camera zooms (e.g. the parent fish swims up and away when the camera goes to the eggs).
- **Predator and prey.** The predator must not swim through the school. The prey turn away and flee; the predator passes above or behind.
- **Kid clarity.** Light the features the narration names (ears, eyes, wings, gills). Avoid dark silhouettes on the key shots.

## 9. 3D (for flight, wing skeletons, orbiting models)
- **3D.**
  - WebGL2 works headless with `--use-gl=angle --use-angle=swiftshader --enable-unsafe-swiftshader --ignore-gpu-blocklist` (software rendering, about 1–1.5 s per 1080p frame).
  - npm and CDN downloads are usually blocked in the workspace. Ask the user to put the library tarball (e.g. `three-0.170.0.tgz`) in their `TTS-Output` folder and stage it.
  - ES modules need a local server (`python3 -m http.server`), not `file://`.
  - Without a library, write raw WebGL2:
    - skeleton-driven membrane panels rebuilt each frame;
    - shell-textured fur with alpha-to-coverage;
    - sky and water in a full-screen shader;
    - ACES tone mapping.

## 10. Mistakes this file exists to prevent
**Content and science:**
- Content above the book's level (tendons, blood vessels, 600 insects an hour for a Level C reader).
- A wing drawn with 3 fingers instead of 4.
- A close-up drawn as a different species than the book page, with ears that looked like a rabbit's.
- Abstract words left abstract: the user wants each key word made concrete on screen when it is spoken (图文对应).

**Drawing from memory:**
- Animals drawn without reference photos looked "code-like". Every animal had to be redrawn from photos, and a wildebeest drawn from memory was wrong (mane, beard, horns, front-heavy build).
- Props drawn without photos: a dead log as a grey slab with an upright stick and arcs; dead horizon trees as matchstick lines.

**Texture:**
- Texture as the main event: a close-up dominated by thousands of long, high-contrast hair strokes looked hairy and messy. Texture must stay quiet; it is not banned.
- Fur outline hairs placed only at sample points (a comb); a net of outlined rings as reptile skin; saw-tooth splinters on a broken log.

**Joins and seams:**
- Parts pasted on: ears stuck on a ball head, a tail like a stick out of the middle of the body, hump caps, leg blocks laid over the body, a corner where the tail meets the rump.
- A shark tail pasted on as a separate sprite looked broken off, and the white belly ended in a blunt cut.

**Pose:**
- A sleeping fox whose tail lay under its chin instead of wrapping over its face.
- A chameleon tail that ran straight with a small hook instead of a tapering spiral.

**Faces:**
- A head drawn as a ball on a separate body; pink bunny ears; a white beard-like chin; a horn-like nose leaf; dark jaw lines; black eye discs that read as sunglasses.

**Motion and physiology:**
- A front-view bat translated sideways ("flying sideways").
- Gill breathing animated wrong: the mouth and gill cover open at the same time, the gill cover itself not moving (only a red band appearing), a red highlight that looked like a wound.

**Props and habitat:**
- A host anemone drawn as stiff straight sticks (the user saw it as ugly "coral").
- Flat-blob coral heads; brain coral as black scribbles or zebra stripes; staghorn branches not joined; sea-fan mesh spilling outside the fan.
- Eggs scattered randomly instead of a packed clutch.
- Small brown dots (parasites) with no recognisable shape and no introduction: the user could not tell what they were.
- Dark dots all over props from an `rgb()`/hex colour-format bug.

**Framing and display:**
- A hero fish cropped by a camera push; sand ending mid-frame during a pan.
- Two caption words highlighted at once.
- A world map with a seam line across it.