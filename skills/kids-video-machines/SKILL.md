---
name: "kids-video-machines"
description: "Topic reference for kids-science-video when the book is about trucks, construction or rescue machines, vehicles, trains, planes, boats or how machines work: rigs, attachments, explainers, labels."
---

# Machines, vehicles & construction: reference for kids-science-video

Read this when the book is about trucks, construction machines, rescue vehicles, trains, planes, boats, tools or how a machine works. The core skill's workflow, TTS, rendering and delivery rules still apply.

**Learned on 8 films:**
- Construction Zone: Backhoes, Bulldozers, Concrete Mixers, Cranes, Diggers, Rollers.
- Machines to the Rescue: Snowplows, Tow Trucks.

## 1. Start from the library
- Check `CATALOG.md` before drawing anything. Reuse what exists:
  - backhoe, dozer, mixer, crane, digger, rollers, paver, plow trucks, plane, flatbed, wrecker, crane truck, car carrier, lowboy;
  - pickup, car, seated driver, standing worker, `drawPerson` (walk / climb / tools / children), `drawSeated` (any cab) and ladder;
  - site, town, winter and sea backdrops.
- Change colours and poses through `P`, not by editing the shared file.
- **A new machine is drawn from the book's own photos first.** The p.22 parts photo and the side-view photos give proportions and part placement. Use web photos only when the book has no side view.
  - Trace proportions point by point into local units (ground y = 0, front to +x).
  - Write the mapping in a comment.
- **Put the machine into the library afterwards.** Use one file per machine, with a `drawX(g, P)` entry, a `xGeom(P)` that returns attachment points, and a `xW(P, p)` local→world mapper. Then the next book in the series starts cheaper.

## 2. Rig rules
- **One rig, one local frame.**
  - `P = {x, gy, s, face, t, roll, …}`.
  - Wheels turn with `roll = distance / (R · s)`, so they never skid.
  - Draw far wheels and tracks first (offset, darker), then the body, then near wheels with arches.
- **Moving parts are angles, not redrawn shapes.**
  - Booms, arms, buckets and decks rotate about real pins.
  - Hydraulic cylinders span pin to pin with `cylinder(g, a, b, w)`.
  - Expose the geometry (`fbGeom`, `wrGeom`, `crGeom2`, `exGeom`) so scenes can attach loads, cables and labels to real points.
- **Drivers and workers must be detailed:**
  - **Seated driver** (`drawDriver`, clipped to the cab window): hard hat with brim, face, hi-vis vest, hands on the wheel.
  - **Standing worker** (`drawTowWorker`): jeans, vest bands, cap, gloves, and poses `reach` (hand to a lever or strap), `crouch`, `wave` and `walk`.
  - A blank silhouette in a cab was rejected.
- **People move like people** (`template/src/09_people.js`, part of every project):
  - **Hands on the controls.** The seated driver's hands sit ON the wheel rim (or ON the lever knobs when the seat faces back); the shoulder is placed so the arm reaches with a bent elbow. A driver whose hand stops short of a far-away wheel was rejected.
  - **Walking: feet stay planted.** Use `drawPerson` with `ph: walkPhase(distanceWalked, s)`; never `ph: t * k`, which makes the legs march in place while the body glides.
  - **Getting in or out: climb, never fly.** Mount a cab by its steps and grab rail with `climbPose` (one foot up a step, then the body pushes up; hands re-grip one at a time), then go in through the door. Leave a trench by a ladder, one person at a time. A worker lerped diagonally into the cab or out of a hole was rejected.
  - **Nobody pops in, fades in, grows or teleports.** People walk into the shot and walk out of it; a person who must be there later is there from the start. Use `drawSeated` for every cab (face, hat, hands on the wheel or levers), never a dark head-and-shoulders silhouette. A worker who moves while working (raking, shovelling) alternates strokes in place with real steps; never slide a working pose across the ground. At a control box only the hand moves from lever to lever, not the whole body.
- **Scale.** A worker is about 1.75 m; a car is about 4.5 m long. Keep trucks, cranes and machines in real proportion to people and cars in the same shot.
- **No crude parts.** A blocky paver and a black box for a crawler counterweight were rejected. Draw real structure: hopper, screed, auger, stacked counterweight slabs, chrome grille, step, mirror on its arm, light bar.

## 3. Physics and attachment: everything must touch what it acts on
These were all real complaints:
- **Cables and hooks end on a real attachment point,** computed from the pose of the other object including its rotation. Example: a tow loop under the rear bumper, from `pickupPose().tow`. A hook in mid-air ("pulling the car by magic") is the worst error.
- **Parts of one body move together.** If the car shakes, its hood, door, steam and driver get the **same** offset. An overlay (an open hood or door) is drawn only while it is open; never leave a static copy over a moving body.
- **Vehicles face the direction they travel.** Check the drawn silhouette itself: the long hood and headlight are at the front, the short trunk and red tail light at the back. A car drawn with its hood on the wrong end looked like it was reversing.
- **Real procedure in the right order:**
  - A rollback backs up to the car (beep, beep): the bed tilts and slides, then the winch cable pulls the car up by its front hook.
  - Straps go over the tyres to deck anchors with a ratchet, and the bed levels before strapping.
  - A wheel-lift cradles one axle; the other wheels roll on the road.
  - A crane lifts with straps under the wheels; outriggers go down first.
  - A plow's curved mouldboard rolls snow up and casts it sideways.
- **Ground contact.** Wheels and tracks sit on the ground line, or on the terrain function (a ditch slope). Compute both contact points and rotate the body to match. Nothing floats, and nothing sinks into the bed or deck.
- **Effort reads as effort.** A stuck car lurches forward and slides back, the body rocks, the wheels spin, snow flies backwards and the exhaust puffs. A static car with a "?" was not enough.

## 4. Explaining how it works (the extension sentence)
- **Make the mechanism visible in a dedicated shot,** not a tiny abstract icon:
  - **"The blade is curved, so snow rolls up and slides to the side":** two animated panels.
    - A side cut-away: snow rides up the curved blade and curls over.
    - A top view: the angled blade sends snow along its face into a windrow.
  - **"Flags show where the ends of the wide blade are":** a front view. The blade is wider than the truck, the flags glow, dashed lines drop from the flags to the blade ends, and a double arrow is labelled "wide blade".
  - **"Each lever moves a different part":** colour rings link each lever to its part (bed tilt, bed slide, winch) as each part moves.
  - **"Its engine stopped working":** a close-up of the open engine bay (block, valve cover, belts, battery, radiator, steam), joined by a dashed line to the car, plus the real dashboard check-engine light. A grey box with a red X was "too abstract".
  - **"Like a fishing reel":** a small reel winding its line, next to the spinning winch drum.
- **Use the real dashboard or real-world symbol** when one exists: the check-engine light, a hazard beacon, a ratchet click.

## 5. Labels and parts pages
- Use the book's own label words and colours (`bookLabel`).
  - Story pages and the parts page often use different colours: sample both.
  - A label points at the real part, outside the caption bar, and never covers the part.
- **Reproduce the parts page or the "types of" page in the book's layout:**
  - same title colour;
  - the machine drawn large and centred, every label inside the frame;
  - each label lit when its word is spoken.

## 6. Camera and framing
- Keep the road low on screen (`gy ≈ 860`, zoom 1.3–1.5 for `s ≈ .5`). Wide shots below zoom 1 made the vehicles tiny.
- Push in (zoom 2–2.4) on the named part when its word is spoken (controls, pulley, strap), then pull back for the action.
- When the camera follows a moving truck, centre it ahead of the truck so the truck stays whole. Check the first and last frame of every follow move.

## 7. Mistakes this file exists to prevent
- Drivers drawn as blank shapes; a paver drawn crudely; a black box for a counterweight.
- A driver far from the steering wheel; a driver floating up into the cab; workers "moonwalking" (legs cycling on a timer, feet sliding).
- Silhouette drivers with no face; a worker fading in beside the blade; children growing out of the ground; a shovelling worker sliding sideways; a worker jumping sideways at each lever change; a stick-figure girl gliding with her dog.
- Following cars drawn under the truck; cars driving "backwards" (the silhouette reversed).
- A hook hanging in the air instead of on the car; a cable ending short because the car's tilt was ignored.
- An open-hood overlay that stayed still while the car shook, and a static closed-hood patch before it opened.
- An abstract engine icon; an abstract top-view icon for blade width; a mechanism explained in a tiny bubble.
- A towed pickup overlapping the wrecker body. Compute the cradle position, then place the pickup there.
- A worker standing in front of the part being explained. Move them aside during the close-up.
- The plane landing through the plow; labels covering the flags they name; scenes framed so wide the machine was a speck.