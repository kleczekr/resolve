# ARRIRAW Shot: 3D Blood Enhancement Workflow

## 1. Problem Summary

We have a film shot in **ARRIRAW**. In one shot, an actor has an existing artificial gunshot wound on the head, but the wound does not contain enough visible blood. The goal is to add **much more blood**, with enough dimensionality that it does not look like a flat 2D overlay.

The difficult part is that the actor’s head is not perfectly still. The actor breathes and the head moves subtly. Therefore, the blood cannot simply be painted on top of the image. The blood needs to appear attached to the actor’s head and move naturally with it.

The desired solution is a **hybrid Resolve/Fusion + Blender workflow**:

- **Resolve/Fusion** for plate prep, tracking, stabilization, roto/masks, final compositing, color matching, grain, and final integration.
- **Blender** for actual 3D blood geometry: clots, raised blood, drips, wet specular highlights, and depth.

The core VFX concept is:

```text
Track / stabilize the actor’s head
→ build 3D blood in Blender on a rough proxy head
→ render blood with alpha
→ composite blood back into Resolve/Fusion
→ restore original motion and integrate
```

---

## 2. Why This Cannot Be Solved With Only a Flat Blood Overlay

A simple 2D PNG or painted blood layer may work for a minor stain, but this shot needs **more 3D blood**. That means the blood should have at least some of the following:

- raised clots
- visible thickness
- curved contact with the skull/skin
- small hanging or running drips
- specular wet highlights
- shadows/contact darkening around the wound
- surface irregularity

A flat 2D element will usually fail because:

- it will look pasted on
- it will not catch light correctly
- it will not have depth
- it may slide if the head moves
- it will not wrap around the curved head shape
- it will not respond believably to focus, blur, grain, and final grade

The solution should therefore combine:

```text
2D projected/painted blood design
+ 3D geometry for volume
+ shader variation for wetness
+ tracked movement
+ compositing polish
```

---

## 3. Recommended High-Level Pipeline

The recommended workflow is **not** to do everything in Blender. Blender should create the 3D blood. Resolve/Fusion should handle the shot-level VFX integration.

```text
ARRIRAW source in Resolve
    ↓
Basic RAW/color-managed VFX plate
    ↓
Fusion tracking / head stabilization reference
    ↓
Export EXR image sequence plate
    ↓
Blender tracking / proxy head / 3D blood build
    ↓
Render blood pass with alpha
    ↓
Import blood render into Fusion
    ↓
Roto/mask/occlusion/color/blur/grain
    ↓
Restore motion if using stabilized workflow
    ↓
Final grade in Resolve
```

---

## 4. Recommended Tool Responsibilities

### Resolve / Fusion

Use Resolve/Fusion for:

- ARRIRAW plate handling
- timeline management
- color-managed VFX plate preparation
- local head tracking
- temporary head stabilization
- roto/masking
- occlusion masks
- final compositing
- color correction of the rendered blood
- matching blur, grain, and motion softness
- final grade

### Blender

Use Blender for:

- importing the VFX plate/image sequence
- creating a rough proxy head or wound-side skull/face patch
- object/head tracking if needed
- building 3D blood geometry
- creating clots, drips, thickness, and raised wound material
- lighting/shading blood
- rendering blood with transparent background
- exporting EXR/PNG sequence with alpha

---

## 5. Working Assumptions

This workflow assumes:

- The shot is live-action ARRIRAW.
- **The head is tilted ~90° (actor lying on their side); the wound is on the temple.** This matters for the blood physics: gravity pulls droplets **off** the temple and they **fall away** — they do not run down the face. This is a *dripping* shot, not a *run-down-a-surface* shot (see Sections 10.5–10.6).
- **Motion is minimal — a slow in-breath, mostly visible in the chest, with only tiny head movement.** The camera is essentially static.
- **Decision: we are NOT stabilizing.** The motion is too small to be worth the risk of making a living actor look artificial. We keep the natural micro-movement and **match-move** the blood to it instead (Approach A, Section 7). The footage may be **slightly slowed** (retime) as a creative choice. The minor wound-position drift can be handled with **a little Transform keyframing in comp at the end** if needed — not with a stabilize pass.
- The existing wound is already visible but **under-dressed**: there is only a small amount of practical blood and a few drops already dripping in the footage. We need to (a) make the wound read as more **extensive / open**, and (b) add a clearly **active blood flow** — fresh blood welling from the wound and dripping off the temple during the shot.
- Because active flow is a requirement, this shot **does** escalate to fluid simulation (see Section 10.5). The general "you rarely need a fluid sim" guidance further down does **not** apply here — active dripping is a story beat, not a stain.
- The final shot will be graded in Resolve after the VFX comp.

If the shot has extreme camera movement, heavy motion blur, fast head movement, or strong occlusion, the workflow becomes more complex. None of those apply here — the only real technical care item is getting **gravity orientation** right for the dripping (Section 10.6), which is straightforward now that we are *not* stabilizing.

---

## 6. Plate Preparation in Resolve

### 6.1 Duplicate the Timeline

Before doing VFX:

1. Duplicate the main edit timeline.
2. Rename it something like:

```text
PIRAI_VFX_BLOOD_WORKING
```

3. Isolate the shot needing blood enhancement.
4. Add handles if possible.

Recommended handles:

```text
8–16 frames before and after the used shot
```

Handles are useful if you later need to adjust edit timing.

---

### 6.2 Work Before Final Creative Grade

The VFX should ideally happen before the final aggressive grade.

Avoid doing the blood comp over a heavily graded image unless absolutely necessary.

Preferred order:

```text
ARRIRAW interpretation / color management
→ VFX comp
→ final look / creative grade
```

Do not bake in:

- heavy contrast
- strong film look
- heavy halation
- strong grain
- strong vignette
- final output LUT

Those should happen after the blood is integrated.

---

### 6.3 Suggested VFX Plate Format

Best option:

```text
OpenEXR image sequence
```

Acceptable option:

```text
ProRes 4444
```

Less ideal:

```text
H.264 / H.265 / MP4
```

Avoid H.264/H.265 for this if possible. Compression artifacts make tracking and comp integration worse.

Recommended export:

```text
Format: OpenEXR sequence
Bit depth: 16-bit half float if available
Color: scene-linear or consistent color-managed VFX space
Resolution: original timeline/source resolution
Frame rate: same as timeline/source
Filename pattern: shot_blood_plate_####.exr
```

Example folder structure:

```text
vfx/
  blood_head_shot/
    plates/
      shot_blood_plate_0001.exr
      shot_blood_plate_0002.exr
      ...
    blender/
      blood_head_shot.blend
    renders/
      blood_beauty_0001.exr
      blood_beauty_0002.exr
      ...
    comp/
      fusion_notes.md
```

---

### 6.4 If You Slow the Footage Down

You're considering a slight slow-down. That's a fine creative choice, but decide it **before** simulating, because it sets the timebase everything else must share.

```text
Lock the final speed first, then sim and track against the ALREADY-retimed plate.
```

Why: the blood sim, the head track, and the plate all have to live on one timeline. If you sim at normal speed and then slow the plate in comp, the blood drips at the wrong (too-fast) rate and the track no longer lines up.

Two clean ways to handle it:

- **Preferred — retime the plate first, export the retimed EXR sequence, then track and sim against that.** Everything is simply "real-time" relative to that plate. Simplest to keep in sync.
- **Alternative — sim and render at normal speed, then slow the rendered blood by the same factor as the plate.** Workable, but for a *dripping* sim, slow-motion liquid is unforgiving: if you stretch normal-speed drops, surface tension and the pinch-off will look wrong. If the final look is noticeably slowed, it's better to sim slow from the start (lower inflow rate, let the solver resolve the slow neck) than to retime fast drops down.

For a *slight* slow-down the difference is small; for anything dramatic, sim it slow natively.

---

## 7. Tracking and Stabilizing the Head in Fusion

> **Decision for this shot: NOT stabilizing.** The motion is just a slow in-breath (mostly chest, tiny head movement). Stabilizing a barely-moving living actor risks making them look artificial — the cure would be worse than the disease. So we use **Approach A (match-move)** below: keep the natural micro-movement, track the small head motion, and ride the blood on it. Any residual wound-position drift gets a touch of manual Transform keyframing in comp at the very end. **Sections 7.3–7.6 are kept for reference** (and in case the body-steadying question comes back), but they are not part of the plan for this shot.

The actor’s head is moving slightly, so the blood needs to follow the head.

There are two usable approaches:

---

### Approach A: Match-Move the Blood to the Moving Head

This is simpler.

```text
Track head movement
→ apply that tracking data to the blood render
→ merge blood over the original moving plate
```

This is usually enough if:

- the head movement is small
- there is no strong rotation
- there is no heavy breathing deformation
- the blood is confined to a small area

---

### Approach B: Stabilize the Head, Work on Stabilized Version, Then Restore Motion

This is cleaner for paint/design work.

```text
Track head movement
→ stabilize the head region
→ design/align blood on stable head
→ render/comp
→ reapply original motion
```

Use this if:

- you need to paint a reference blood design
- the head movement makes alignment annoying
- you need to inspect the wound area frame by frame
- the blood geometry must be lined up carefully

---

### 7.1 What to Track

Track stable areas near the wound, not the wound itself.

Good tracking features:

- temple texture
- eyebrow edge
- hairline
- ear contour
- cheekbone shadow
- forehead pores/texture
- high-contrast skin feature

Bad tracking features:

- the wound prosthetic itself
- shiny blood
- changing highlights
- motion-blurred patches
- soft low-contrast skin
- hair strands moving independently

The goal is to track the **head**, not the blood.

---

### 7.2 Which Fusion Tracker to Use

Use one of these:

#### Planar Tracker

Use if the head motion is mild and behaves roughly like a plane.

Good for:

- slight head translation
- slight rotation
- slight camera movement
- a small wound area

#### Surface Tracker

Use if available and if the skin/head surface deforms.

Good for:

- subtle breathing deformation
- cheek/forehead warping
- non-flat organic surface motion

#### Point Tracker / IntelliTrack

Use for a quick attempt if there are clear contrast features.

Good for:

- quick tracking
- stabilization
- simple match move

Not ideal for:

- curved surfaces
- soft skin with few contrast points
- strong rotation

---

### 7.3 Two Different Stabilization Goals — Keep Them Separate

There are two reasons you might stabilize, and conflating them causes most of the pain.

```text
Goal 1 — VFX alignment (the wound):
    Temporarily neutralize head motion so blood is easy to design,
    align, and simulate. Motion is RESTORED before final output.
    Nobody ever sees the stabilized version.

Goal 2 — Final look (the body):
    Deliberately make the actor steadier in the FINISHED shot
    because the footage is too shaky / too alive / too restless.
    The audience DOES see the result.
```

Goal 1 is covered by Approach B in Section 7 and by Section 8. It is safe, standard, and reversible.

Goal 2 — your "magic-mask the body, put it on a top layer, freeze it" idea — is a creative decision with real traps. Read 7.4 before committing to it.

---

### 7.4 The Magic-Mask Freeze-Frame Idea — and Its Traps

Your instinct:

```text
Magic Mask the body
→ promote it to a top layer
→ freeze-frame it (hold a single frame)
→ optionally scale it up slightly so motion stops mattering
→ comp it back over the (possibly still-moving) background
```

This *can* work, but understand what you are actually doing: you are turning a living person into a **photograph** and laying it over live footage. That is exactly where it tends to break:

- **Frozen grain / frozen noise.** A held frame holds its grain too. The body becomes a still texture while the background keeps shimmering. This is the single biggest giveaway. If you freeze, you **must** strip the baked grain and add fresh *moving* grain on top.
- **Edge / matte crawl mismatch.** Magic Mask edges wander frame to frame. Freeze the matte and the background keeps moving — so the seam between frozen subject and live plate slides and breathes. The silhouette edge is where the eye goes first.
- **Lost light interaction.** Passing shadows, flicker, bounce, atmosphere — none of it lands on a frozen subject anymore. If anything in the scene changes the light, the frozen body ignores it and reads as pasted-in.
- **The "embalmed" look.** A completely still living person is uncanny. Zero breath, zero weight shift, zero micro-tension reads as *dead*, not *calm* — which, for a gunshot-wound shot, may even be the wrong kind of unsettling.
- **Scale-up softness / parallax.** Enlarging the frozen body to outrun motion changes its perspective and softens it relative to the plate, and it no longer lines up with contact points (where the body meets ground, props, other actors).

So: a hard freeze is the **highest-risk** option, not the easy one. Reserve it for a genuine "frozen moment" creative beat — and even then, re-inject life (see 7.5).

---

### 7.5 Preferred: Stabilize, Don't Freeze — and Preserve Micro-Movement

You were right to suspect that micro-movements beat a static image. They almost always do. The better tools sit *between* "raw shaky footage" and "dead still":

**Option A — Smoothed / damped stabilization (recommended for the look).**
Track the body/head and remove the *gross, unwanted* motion (drift, sway, jitter) while **keeping** breathing and micro-tension. In Fusion this is a Tracker in *Match Move → Stabilize* mode with the stabilization **strength/smoothing dialed below 100%**, or a Planar Tracker stabilize feeding a Transform you partially back off. On Resolve's Color page the Stabilizer has a **Smooth** control and a **Strength** slider — lower strength keeps life, higher strength approaches lock-down. This gives "steadier, still alive."

**Option B — Stabilize-for-work, then restore (recommended for the blood).**
This is Approach B already in the doc. Neutralize motion only while you design/sim/align the blood, then **re-apply the exact inverse transform** so the final image has all its original motion back. The blood inherits the motion; the actor keeps every micro-movement because you never threw it away.

**Option C — Hard freeze, but faked back to life (only if you truly want a held moment).**
If you still want the frozen-body look:
- Strip baked grain; add fresh moving grain/noise over the frozen layer.
- Add a tiny animated breathing motion: a subtle, slow Transform or a gentle Warp/Dent oscillation on the chest/shoulders (a few pixels, ~0.2–0.4 Hz).
- Keep *something* live if you can roto it — eyes, a hair edge, a wisp — so the frame isn't 100% inert.
- Re-introduce any moving light/shadow over the frozen layer as an animated multiply.

In order of how natural they look: **A ≈ B  >  C**. Don't reach for C unless the still-moment is the point.

---

### 7.6 Where to Stabilize: Resolve Color Page vs Fusion

Both live inside Resolve; pick by task.

```text
Resolve Color page:
    - Magic Mask (DaVinci Neural Engine) to isolate the body/person — best place for the mask itself.
    - Stabilizer (Tracker palette: Perspective / Similarity / Translation,
      plus Smooth + Strength) for quick whole-frame or region steadying.
    - Magic Mask v2 / Depth Map can help generate occlusion mattes for the comp.
    - Good for: fast look-stabilization, generating the body matte, grade.

Fusion (recommended for the VFX-grade work):
    - Planar Tracker / Tracker → precise region stabilization and, crucially,
      the ability to RE-APPLY the inverse transform later (stabilize → work → restore).
    - MatteControl / roto / Delta keyer to refine the Magic Mask edge.
    - TimeStretcher / Hold-frame node if you genuinely freeze a layer.
    - Merge stack to recombine frozen/stabilized body over live background.
    - Good for: stabilize-and-restore, freeze-frame recombines, edge work.
```

Rule of thumb: **generate the body matte with Magic Mask (Color page), but do the actual stabilize / freeze / recombine in Fusion**, because only Fusion gives you clean control over re-applying motion and managing the seam between a treated subject and a live background.

If you go the freeze route, your Fusion stack is roughly:

```text
MediaIn plate
→ Magic Mask matte (imported from Color page or rebuilt with roto)
→ split: [body layer]  +  [background layer]
→ body layer: Hold-frame → strip grain → add fresh grain → subtle breathing warp
→ Merge body over background
→ re-grain whole frame lightly to marry the seam
→ MediaOut
```

---

## 8. Exporting a Stabilized Reference for Blender

If you choose the stabilize-first workflow, export a stabilized version of the head shot for Blender reference.

This does **not** have to be the final plate. It can be a working reference that helps you align the blood.

Recommended files:

```text
plates/
  original_plate_####.exr
  stabilized_head_ref_####.exr
```

The stabilized reference helps you:

- design the blood on a steady wound
- position the proxy head
- judge the blood silhouette
- avoid fighting subtle actor movement while modeling

Later, the final blood pass still needs to match the original moving shot.

---

## 9. Blender Workflow

## 9.1 Import the Plate

In Blender:

1. Open a new project.
2. Set the frame rate to match the film shot.
3. Set the frame range to match the plate.
4. Import the image sequence as a movie clip or background reference.
5. Set resolution to match the plate.

Recommended:

```text
Frame rate: same as Resolve timeline
Resolution: same as plate
Start frame: match exported sequence
```

If your Resolve/Fusion comp starts at frame 1001, keep that convention. Do not casually shift frame numbering unless you track it carefully.

---

## 9.2 Camera / Object Tracking Decision

### If Camera Is Static and Only the Head Moves

This is the easier case.

You can treat the head as the moving object.

Workflow:

```text
Import plate
→ track points on head
→ solve/animate object motion or manually keyframe proxy head
→ attach blood geometry to proxy head
```

If the head motion is very small, manual keyframing may be faster and more reliable than a full solve.

### If Camera Moves and Head Moves

More complex.

Workflow:

```text
Track background/camera features
→ solve camera
→ track head/object features
→ solve/approximate head object motion
→ attach proxy head and blood to head motion
```

This is harder and may require more manual cleanup.

### If Tracking Fails

Do not panic.

For a short shot, it may be faster to:

- manually keyframe the proxy head
- use 2D tracking from Fusion for comp alignment
- render several blood versions if needed
- fix final drift in Fusion

Do not worship the solver. The audience only sees the final shot.

---

## 9.3 Create a Rough Proxy Head

You do not need a perfect digital double.

You need enough geometry for the blood to:

- sit on a curved surface
- have thickness
- cast or receive small contact shadows
- wrap around the forehead/temple/head region
- move in plausible relation to the head

Options:

### Option 1: Simple Head Mesh

Use a generic head mesh and reshape it roughly.

Good if:

- side/forehead/temple area is visible
- you need curvature
- you want blood to wrap naturally

### Option 2: Partial Patch Mesh

Create only the visible wound-side patch.

Good if:

- the blood is localized
- the shot is close-up
- you only need temple/forehead curvature

### Option 3: Very Simple Curved Surface

Use a subdivided plane bent into head-like curvature.

Good if:

- the shot is short
- the head motion is small
- the blood is mostly visible from one angle

For a beginner, Option 2 or 3 may be enough.

---

## 9.4 Align Proxy Head to Plate

Use the plate as camera background.

Line up the proxy head/wound area against the footage:

- match head angle
- match scale
- match wound position
- match visible curvature
- match camera perspective

This does not need to be anatomically perfect. It needs to be correct enough that the blood appears locked to the actor.

Use a reference frame where:

- the wound is clearly visible
- the head is not motion blurred
- the actor is in a neutral part of the movement
- lighting is representative

Mark that frame as the **hero alignment frame**.

Example:

```text
Hero frame: frame 1034
```

---

## 10. Building the 3D Blood

Do not begin with a complex fluid simulation. Start with art-directed blood geometry.

The recommended blood build is:

```text
projected/painted blood stain
+ raised clot geometry
+ small drip geometry
+ wet highlight shader
+ contact darkening
```

---

### 10.1 Base Blood Projection

Create a 2D blood design first.

You can make this by:

- painting on a still frame
- drawing a mask/reference in Fusion
- creating a texture in Krita/Photoshop/GIMP
- using Blender texture painting

The base design should include:

- main wound blood mass
- stain spreading around wound
- darker clot center
- thinner reddish smear edges
- possible downward drip paths

Then project or map it onto the proxy head.

This gives you good placement and art direction.

---

### 10.2 Raised Clot Geometry

Create actual small meshes for the dimensional parts.

Possible geometry:

- flattened blobs
- irregular raised patches
- small lumpy clots
- torn blood ridges around wound
- pooled blood crescent
- small bead-like clots

Techniques:

- metaballs converted to mesh
- sculpted mesh blobs
- subdivided mesh with proportional editing
- geometry nodes if comfortable
- displacement map on a patch mesh

Keep the geometry irregular. Realistic gore should not look like clean red plastic.

---

### 10.3 Drips

Add a few controlled drips.

For a head wound, less is often better.

Good drip types:

- short thick drip from wound edge
- thin partially dried streak
- one hanging bead
- one curved run following face contour

Avoid:

- too many symmetrical drips
- perfectly vertical lines if the head is tilted
- bright red ketchup color
- overly smooth tubes

Drips should follow gravity in the shot. If the head is tilted, gravity does not necessarily align with the screen’s vertical axis.

---

### 10.4 Do You Need Fluid Simulation? — For This Shot, Yes

The general rule below still holds for *most* wound-enhancement work, but **this shot is the exception**: we specifically want fresh blood **actively welling and dripping** from an enlarged wound. Active liquid motion is a story beat here, so we escalate to a real fluid simulation (Section 10.5).

Use fluid sim when (all true for us):

- blood visibly flows during the shot ✔ (we want this)
- a fresh well/run develops over time ✔
- liquid motion is a story point ✔
- the camera is close enough for fluid motion to be seen ✔

For a *static* wound stain, hand-built geometry is still better (easier to direct, revise, match, render). So the smart approach is **hybrid**:

```text
Hand-built geometry  →  the static wound mass, clots, and pooled blood
Fluid simulation     →  the active welling + running drips on top
Existing practical drops in the footage  →  match to, don't fight
```

Do **not** simulate the whole thing. Simulate only the moving part. Keep the congealed wound bed as art-directed geometry (Sections 10.1–10.3) and let the sim ride on top of it.

Before committing to a full sim, it is still worth trying **faked motion** for any drips that don't need to interact:

- animate a bead moving down a curve (a path-following sphere with a stretch)
- animate drip length / reveal mask growing
- animate material wetness
- render 2–3 incremental blood states and dissolve between them

Use the sim for the hero flow that has to behave like liquid; fake the secondary dribbles. That keeps the heavy, fragile part of the job as small as possible.

---

### 10.5 Active Blood Flow — Fluid Simulation (Mantaflow Liquid)

Blender's built-in liquid solver is **Mantaflow** (Object → Quick Effects → Quick Liquid, or add a Fluid physics modifier). The whole job is: an **inflow** at the wound, a **domain** that contains the drip path, the **proxy head as a collision (effector)** so blood wells against the temple before it detaches, and a **mesh** output you can shade and render with alpha.

**This is a dripping shot, not a running shot.** Because the head is tilted ~90° and the wound is on the temple, blood doesn't run down the face — it **wells at the wound, forms a hanging (pendant) droplet, stretches, pinches off, and falls away** under gravity. That's a dripping-faucet simulation. The physics that matter most here are **surface tension** (holds the bead until its weight wins, then necks and pinches) and **enough resolution to resolve the thin neck** as the drop detaches. A low-resolution sim will drop fat round blobs with no neck and look fake.

**The droplets only fall a short distance and leave frame** — they don't land on anything in shot. This is the easy version: no landing surface, no splash, no secondary spatter to worry about. The hero moment is entirely at the wound — the well, the hang, and the pinch-off — plus the first few cm of fall before the drop exits frame. Keep your effort (and your resolution) concentrated there.

**Enlarging the wound first.** The wound is too small as shot, so before simulating, build the bigger source: expand the opening with projected texture / matte paint, add a little displacement to suggest a deeper cavity, and darken the interior. The fluid **inflow** then emits from inside this enlarged wound so the blood reads as coming *out of* the wound, not appearing on the skin.

**Core setup:**

```text
Domain (cube):
    - Just needs to contain the wound + the short fall path to the edge of frame.
      It can be small and tight — drops leave frame almost immediately, so don't
      waste a tall domain (and the voxels) on fall distance you'll never see.
    - Fluid → Type: Domain → Domain Type: Liquid
    - Enable Adaptive Domain (Resize) so the box follows the fluid and you don't waste voxels.

Inflow (small object inside the wound):
    - Fluid → Type: Flow → Flow Type: Liquid → Behavior: Inflow
    - Keep the flow rate LOW and sustained → blood wells slowly and drips at intervals,
      rather than pouring. A low inflow is what produces discrete droplets.
    - Keyframe it on/off for the bleeding window.

Effector (the proxy head):
    - Fluid → Type: Effector → Effector Type: Collision
    - Surface Thickness small but non-zero so thin blood doesn't leak through.
    - Here it makes blood pool/cling at the temple wound before a drop detaches and falls.

No landing surface needed:
    - Drops fall a short distance and leave frame. Let them simply exit the bottom
      of the domain. No collision plane, no splash, no spatter.

Gravity:
    - Scene gravity (true world-down) drives the drip. Orientation note in Section 10.6.
```

**Settings that actually matter for this drip:**

- **Resolution Divisions.** The detaching neck of a droplet is *thin*. Too-low resolution = fat blobby drops with no neck, no pinch, no realism. Push resolution up (main cost) and keep the domain tight around the wound + fall path. Block out at low res, then raise it for the final cache.
- **Surface Tension.** The single most important setting for dripping. It's what makes the bead hang, neck, and pinch off. Tune it so drops form and release at a believable size — too low and it sheets/strings, too high and everything balls into spheres.
- **Viscosity.** Real blood is only mildly viscous (~3–4× water), but a little extra sells the slow, heavy, clinging weep and slows the drip cadence. Start moderate; too high and it crawls like honey, too low and it splashes like water.
- **Solver.** Prefer **APIC** over FLIP — more stable, less random splash, cleaner droplet formation.
- **Mesh.** Enable the domain's **Mesh** panel for a renderable surface. Raise Upres if the meshed droplets look blobby.
- **Secondary Particles (optional, sparing).** A *couple* of fine flung droplets can read well if a drop hits something. Don't add foam — blood doesn't foam.
- **Scale.** Mantaflow is **scale-sensitive**. Set the domain's real-world size to head scale (a head is ~0.2 m, a droplet a few mm). Wrong scale = drops that float like a giant slow ocean or zip like mist, and the drip *cadence* (how often a drop falls) will be wrong. Get this right before tuning anything else.

**Cache it:**

```text
Domain → Cache:
    - Type: Modular or All
    - Format: OpenVDB (Files) so it survives restarts and renders deterministically
    - Set Frame Start/End to the shot range (+ a few pre-roll frames so flow has begun before the cut in)
    - Bake Data → (Bake Mesh) → bake secondary particles last
```

Pre-roll matters: if the wound should already be bleeding when the shot starts, begin the inflow several frames **before** your first comp frame so there's flow on frame one rather than a sudden start.

---

### 10.6 Gravity & the 90° Tilt

**Blood falls in world space, not head space.** With the head tilted ~90° on the temple, this is the whole ballgame: a droplet leaving the wound falls toward the **real ground**, which — relative to the head — is roughly *sideways/off the temple*, not "down the face." Get this backwards and the drops will look like they're defying gravity.

Good news: **because we decided not to stabilize (Section 7), this is the easy case.** You build the proxy head in its real tilted orientation, leave scene gravity at default true world-down, and the sim just does the right thing — droplets pinch off the temple and fall toward the ground exactly as they should.

```text
Setup:
    - Orient the proxy head to match the footage: ~90° tilt, wound on the temple, facing as shot.
    - Leave scene gravity at world-down (default -Z).
    - Frame the camera in Blender to match the plate, so "down the screen" in your render
      matches "down" in the shot.

Sanity check:
    - Play the sim and confirm droplets fall the same direction the existing practical
      drops fall in the footage. They must agree — that's your ground-truth reference.
```

> Note: the elaborate "counter-rotate gravity on a stabilized plate" warning that used to live here only applies if you stabilize. We aren't, so ignore it. The only thing to nail is matching the head's real-world tilt and the camera framing so screen-down equals world-down.

---

### 10.7 Wetting at the Wound (Minor Here)

Because our droplets **fall away** rather than run down the face, the long wet trail this section was built around mostly doesn't apply. There's no streak down the cheek to stain — the blood leaves the temple and drops.

What *does* still help, in a smaller way:

- **A wet, glistening pool right at the wound** where blood wells before each drop releases — keep that area lowest-roughness and darkest-wettest so the source reads as fresh and liquid.
- **A short residue** along the lip of the wound where blood briefly clings before pinching off — a small Dynamic Paint canvas patch (or just hand-painted wetness in the wound material) covers this. You do **not** need a full run-down-the-face trail.

If you find a drop *does* briefly track across a bit of skin before detaching, you can still use Dynamic Paint (Canvas = proxy head, Brush = fluid/particles) to stain just that short path — but treat it as a small detail, not the hero element it would be in a run-down-the-face shot.

---

## 11. Blood Material / Shader Design

Blood should not be pure red.

Use layered variation.

### 11.1 Color

Suggested color zones:

```text
Deep clot:      dark maroon / brown-red
Wet blood:      deep red, not neon
Thin smear:     reddish brown / translucent red
Fresh edge:     slightly brighter red
Dried edge:     darker, browner, less glossy
```

Avoid:

```text
pure RGB red
uniform red
plastic candy red
over-saturated theatrical red
```

---

### 11.2 Roughness / Specular

Blood realism depends heavily on gloss variation.

Material idea:

```text
Base clot: medium roughness
Wet layer: low roughness, high specular
Dried edge: higher roughness
Thin smear: mixed roughness
```

The wet highlights should be:

- small
- sharp
- tied to the scene light direction
- not everywhere

If the entire blood surface is equally glossy, it will look fake.

---

### 11.3 Bump / Displacement

Use subtle bump or displacement for:

- clot texture
- uneven coagulation
- skin-wound transition
- tiny ridges
- broken edges

Do not overdo it. Large displacement can look like alien coral.

---

### 11.4 Transparency / Subsurface

For most film comp work, do not overcomplicate it.

A little translucency can help on thin edges, but thick blood should mostly read as dark, wet mass.

---

### 11.5 Shading Dripping vs Static Blood Differently

Now that part of the blood is *actively dripping*, it should not share one uniform material with the dried wound bed. Fresh falling blood and old settled blood look different:

```text
Fresh / dripping (the sim mesh — welling bead + falling droplets):
    - brightest, wettest, lowest roughness, sharpest highlights
    - slightly more translucent at the thin neck as a drop pinches off
    - the droplet catches a clear, small specular glint

Wet pooled (the welling mass at the wound):
    - deep red, very glossy, dark in the mass — this is the freshest-reading spot

Coagulated / static (hand-built clots, old stain):
    - darker maroon/brown, higher roughness, duller
```

The key contrast for *live bleeding* is a **bright, wet, glossy welling pool and droplets** against the **darker, duller coagulated wound bed** around it. That difference — fresh-and-liquid sitting on old-and-set — is what sells it. (The Dynamic Paint wetness from 10.7 only matters for the small area right at the wound here, not a long trail.)

---

## 12. Lighting the Blood in Blender

Match the plate lighting.

Before rendering, identify:

- main light direction
- fill level
- highlight color
- shadow softness
- contrast level
- whether the scene is warm/cool
- whether the blood should catch a sharp or soft highlight

Use simple Blender lighting:

- one key light matching the shot
- optional soft fill
- optional small specular kicker if the shot has one

Do not create beautiful lighting that does not exist in the footage. The blood must belong to the plate.

---

## 13. Rendering the Blood Pass

Render only the blood over transparency.

Recommended render:

```text
Format: OpenEXR sequence
Background: transparent
Alpha: enabled
Resolution: same as plate
Frame range: same as shot
Motion blur: match plate if needed
```

Useful passes:

```text
Beauty blood pass
Alpha
Specular pass, optional
Shadow/contact pass, optional
Cryptomatte/object mask, optional
```

For the active-flow version, a few extra passes help in comp:

```text
Static wound/clot pass        (hand-built geometry — rarely changes)
Flowing blood pass            (the Mantaflow mesh — the moving part)
Wet-trail / wetmap pass       (Dynamic Paint stain on skin)
Drip-tip specular pass        (so you can push the live glint separately)
```

Splitting the static wound from the moving flow lets you retime, dim, or hold the flow in comp without re-rendering the whole wound — very useful when the director says "a touch less blood" or "start the drip two frames later."

If you are new to passes, start simple:

```text
blood_beauty_with_alpha_####.exr
```

Then do the integration in Fusion.

---

## 14. Compositing Back in Fusion

Import:

```text
original plate
blood render with alpha
tracking/stabilization data
occlusion masks
```

Basic comp stack:

```text
MediaIn original plate
Blood render EXR
→ color correct blood
→ blur/defocus match
→ grain/noise match
→ mask/roto occlusions
→ merge over original plate
→ final adjustments
→ MediaOut
```

---

### 14.1 Align Blood to Head Motion

If Blender rendered blood already moving correctly, you may only need small corrections.

If Blender rendered on a stabilized reference, then you need to reapply the original head/camera motion in Fusion.

Possible methods:

- use Fusion Tracker data to reintroduce motion
- apply Planar Transform data to the blood pass
- manually keyframe Transform corrections
- combine tracker motion with manual correction

For short shots, a small amount of manual keyframing is normal.

---

### 14.2 Occlusion Masks

This is critical.

Create rotos/masks for anything that should appear in front of blood:

- hair
- ear edge
- actor’s hand
- foreground objects
- wound prosthetic raised edge
- face contour

Without occlusion, the blood may look pasted over everything.

---

### 14.3 Color Match

Blood render will almost always need adjustment in Fusion.

Adjust:

- lift/gamma/gain
- saturation
- hue
- contrast
- black level
- highlight brightness
- specular intensity

Important rule:

```text
The blood black level must match the shot black level.
```

If the blood has deeper blacks or brighter highlights than the shot allows, it will separate from the image.

---

### 14.4 Blur / Defocus / Motion Blur

The CG blood cannot be sharper than the plate.

Match:

- lens softness
- focus falloff
- motion blur
- sensor/noise texture
- compression/grain

If the face is slightly soft and the CG blood is razor-sharp, the illusion breaks.

Add:

- tiny blur
- directional/motion blur if movement demands it
- grain/noise after blur

---

### 14.5 Grain Match

If the shot has grain/noise, the blood needs it too.

Typical mistake:

```text
grainy ARRIRAW plate + clean CG blood = fake
```

Add grain/noise to the blood or to the final comp. Prefer matching the plate texture before final grade.

---

### 14.6 Timing the Flow and Marrying It to the Practical Drops

The footage already has a few real drops. The CG flow must agree with them, not contend:

- **Match the existing drops' speed and direction** — sample how fast the practical blood moves and aim the sim's run to match. Mismatched drip speeds between real and CG blood is an instant tell.
- **Let the practical drops lead.** Where possible, have the new flow appear to originate the same drops the camera already caught, so they read as one event.
- **Time the build with the action.** If the wound is fresh in-shot, ramp the flow on; if it's an existing wound, it should already be running on frame one (use the pre-roll from Section 10.5).
- Because you rendered the flow as a separate pass (Section 13), you can slip its timing in comp with a TimeStretcher without touching the static wound.

### 14.7 If You Used the Freeze-Frame Body Treatment

If you took the Section 7.4/7.6 freeze route, the comp has one extra job: **marry the seam**. The blood now lives on a frozen (or stabilized) body over a live background.

- Add fresh moving grain over the frozen body so it doesn't sit dead against the shimmering plate.
- Watch the magic-mask edge against the moving background for crawl/sliding; hold or re-roto the matte if it breathes.
- Make sure the blood's added grain/blur matches the *treated* body layer, not the untreated plate — they may differ.
- Re-grain the whole frame lightly at the end to unify everything under one texture.

---

## 15. Final Grade

Once the comp is integrated, return to the Resolve Color page and grade the shot with the blood included.

Watch for:

- blood becoming too saturated after grade
- highlights clipping
- red hue shifting toward orange/magenta
- shadow crushing hiding the clot detail
- film grain/halation making blood too intense
- vignette changing perceived blood brightness

The blood should survive the grade but not scream “VFX element.”

---

## 16. Quality Control Checklist

Watch the shot at normal speed first.

Do not judge only frame-by-frame.

### Motion

- Does blood stick to the head?
- Does it drift?
- Does it wobble independently?
- Does it survive breathing motion?
- Does it slide at the wound edge?

### Drip (active blood)

- Do droplets fall toward the **real ground** (off the temple), matching the existing practical drops? (See 10.6.)
- Do drops **neck and pinch off**, or do they release as fat round blobs? (Resolution / surface tension.)
- Is the drip **cadence** believable for the wound size — not too fast, not too regular?
- Is the welling pool at the wound wet and glossy, and does the bleeding start/stop read with the action (no abrupt pop-on)?
- If slowed: does the slow-motion liquid still look like liquid, or like stretched goo? (See 6.4.)

### Motion / no-stabilize

- Did the natural in-breath survive? (We chose NOT to stabilize — confirm the shot still feels alive.)
- Does the blood track the small head motion without sliding or lagging?
- If you keyframed wound position in comp: is it correcting drift, or fighting the track?

### Integration

- Is blood too sharp?
- Is blood too clean?
- Is blood too red?
- Is the black level correct?
- Are highlights believable?
- Does grain match?
- Does blur match?

### Spatial Believability

- Does blood wrap around the head curvature?
- Do clots have visible thickness?
- Do drips follow gravity?
- Are occlusions correct?
- Does hair/skin edge interact properly?

### Story / Taste

- Is there too much blood?
- Is it distracting?
- Is the wound still readable?
- Does the enhancement serve the shot?
- Does it accidentally look comic or fake?

---

## 17. Common Failure Modes and Fixes

### Problem: Blood Looks Like a Sticker

Fix:

- add raised geometry
- add contact shadows
- add edge irregularity
- reduce saturation
- match blur/grain
- add occlusion masks

---

### Problem: Blood Slides on the Head

Fix:

- improve head track
- use better features around temple/hairline
- use Surface Tracker if planar track fails
- manually keyframe corrections
- reduce visible blood edge near high-motion areas

---

### Problem: Blender Track Is Too Hard

Fix:

- use Fusion for tracking/match move
- manually align proxy head in Blender on hero frames
- render blood on stabilized reference
- reapply motion in Fusion
- use manual keyframes for short shot

---

### Problem: Blood Is Too Red

Fix:

- darken base
- shift toward maroon/brown
- reduce saturation
- add darker clot areas
- use highlights for wetness, not red saturation

---

### Problem: Blood Is Too Sharp

Fix:

- add slight blur
- match plate focus
- add grain
- add motion blur if needed
- soften alpha edges

---

### Problem: Blood Does Not Look Wet

Fix:

- add small specular highlights
- lower roughness only in selected areas
- create roughness variation
- add tiny bright glints according to light direction
- keep most of the blood dark so highlights have contrast

---

### Problem: Blood Looks Like Plastic

Fix:

- increase color variation
- add bump/displacement
- break up specular
- reduce uniform smoothness
- darken thick areas
- roughen dried edges

---

## 18. Beginner-Friendly First Pass Plan

For a first working version, do not attempt the most complex version.

Do this:

```text
1. Export EXR plate from Resolve.
2. Track/stabilize head region in Fusion.
3. Export one stabilized reference frame or sequence.
4. In Blender, create a rough curved patch matching wound-side head.
5. Paint/project base blood texture onto the patch.
6. Add 3–5 raised clot meshes.
7. Add 1–3 controlled drips.
8. Create wet/dry blood material variation.
9. Render blood with transparent alpha.
10. Composite in Fusion over the original plate.
11. Use masks for hair/wound/skin occlusion.
12. Match blur, grain, and color.
13. Watch at full speed.
14. Manually keyframe corrections where needed.
```

This is the best balance of realism and sanity.

---

## 19. More Advanced Version

Only after the first pass works:

```text
1. Improve proxy head shape.
2. Add object/head motion solve in Blender.
3. Add animated drip movement.
4. Add separate specular render pass.
5. Add contact shadow pass.
6. Add displacement texture for clots.
7. Use Surface Tracker / mesh tracking if skin deformation is visible.
8. Render multiple blood layers separately for better comp control.
```

Separate passes could include:

```text
blood_base.exr
blood_clots.exr
blood_drips.exr
blood_specular.exr
blood_shadow.exr
```

This gives more control in Fusion.

---

## 20. Practical Decision Tree

### Is the blood mostly stationary after the wound?

Use:

```text
Projected texture + 3D clot geometry
```

### Is blood visibly flowing?

Use:

```text
Animated drip geometry first
Fluid simulation only if necessary
```

### Do we WANT active dripping? (this shot — yes)

Use:

```text
Hybrid: hand-built static wound bed
      + Mantaflow liquid sim for the active drip (10.5) — surface tension is king
      + wet pool only at the wound (10.7), no run-down-face trail
      + proxy head in its real 90° tilt, scene gravity = world-down (10.6)
```

### Head tilted 90°, wound on temple — which way does blood go?

Use:

```text
Droplets pinch off the temple and FALL toward the real ground.
They do NOT run down the face. Match the existing practical drops' direction.
```

### Do we want the body steadier in the final shot? (this shot — NO)

Use:

```text
Decided against. Motion is just a slow in-breath; stabilizing risks an artificial look.
Keep the micro-movement, match-move the blood (Approach A), and add a touch of
manual Transform keyframing in comp only if the wound drifts (7. decision note).
If a slight slow-down is wanted, lock that speed FIRST (6.4).
```

### Is the head barely moving?

Use:

```text
Fusion planar track + Blender render + Fusion comp
```

### Is the head breathing/warping visibly?

Use:

```text
Surface tracking / mesh tracking / manual correction
```

### Is the camera moving strongly?

Use:

```text
Camera solve + object/head solve
```

### Is the shot very short?

Use:

```text
Manual keyframes where tracking fails
```

Do not waste a whole day making a perfect solve for a one-second shot if manual keyframes solve it.

---

## 21. Final Recommendation

The best workflow for this shot is:

```text
Resolve/Fusion:
    ARRIRAW prep
    local head tracking
    temporary stabilization
    roto/masks
    final comp

Blender:
    rough proxy head
    projected blood design
    actual 3D clot/drip geometry
    wet blood shader
    transparent EXR render

Resolve/Fusion:
    composite blood render
    occlusion masks
    color/blur/grain match
    restore original motion
    final grade
```

The most important creative recommendation:

```text
Do not make the blood more believable by making it more complicated.
Make it more believable by making it more layered.
```

Use:

```text
2D blood stain for placement
+ 3D geometry for thickness
+ shader variation for wetness
+ careful comp integration
```

That is the most controllable and realistic route for enhancing an existing gunshot head wound in a live-action ARRIRAW shot.

---

## 22. References / Useful Documentation

- Blackmagic Design — DaVinci Resolve Fusion overview  
  https://www.blackmagicdesign.com/products/davinciresolve/fusion

- Blackmagic Design — DaVinci Resolve 20 Fusion Visual Effects Guide  
  https://documents.blackmagicdesign.com/UserManuals/DaVinci-Resolve-20-Fusion-Visual-Effects.pdf

- Blender Manual — Motion Tracking  
  https://docs.blender.org/manual/en/latest/movie_clip/tracking/index.html

- Blender Manual — Plane Track Deform Node  
  https://docs.blender.org/manual/en/latest/compositing/types/tracking/plane_track_deform.html

- Blender Manual — Render transparency / film settings  
  https://docs.blender.org/manual/en/latest/render/cycles/render_settings/film.html

- Blender Manual — Fluid simulation (Mantaflow) overview  
  https://docs.blender.org/manual/en/latest/physics/fluid/index.html

- Blender Manual — Liquid domain settings (resolution, viscosity, mesh, cache)  
  https://docs.blender.org/manual/en/latest/physics/fluid/type/domain/liquid/index.html

- Blender Manual — Fluid Effector / collision objects  
  https://docs.blender.org/manual/en/latest/physics/fluid/type/effector.html

- Blender Manual — Dynamic Paint (wetmap / stain trails)  
  https://docs.blender.org/manual/en/latest/physics/dynamic_paint/index.html

- Blackmagic Design — DaVinci Resolve Magic Mask  
  https://www.blackmagicdesign.com/products/davinciresolve/color

- Blackmagic Design — DaVinci Resolve Stabilizer (Color page tracker / Smooth + Strength)  
  https://documents.blackmagicdesign.com/UserManuals/DaVinci-Resolve-20-Reference-Manual.pdf
