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
- The actor’s head moves slightly.
- The camera may be static or gently moving.
- The existing wound is already visible.
- We need to enhance the wound with **more dimensional blood**, not create the entire wound from scratch.
- The blood is mostly attached to the head, not a large active splatter/gush event.
- The final shot will be graded in Resolve after the VFX comp.

If the shot has extreme camera movement, heavy motion blur, fast head movement, strong occlusion, or long flowing blood simulation, the workflow becomes more complex.

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

## 7. Tracking and Stabilizing the Head in Fusion

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

### 10.4 Do You Need Fluid Simulation?

Usually, no.

Use fluid sim only if:

- blood visibly flows during the shot
- a fresh gush happens
- a drip grows over time
- liquid motion is a story point
- the camera is close enough for fluid motion to be seen

For a wound enhancement shot, hand-built geometry is usually better:

- easier to direct
- easier to revise
- easier to match to the prosthetic
- faster to render
- less technically fragile

If you need motion, fake it first:

- animate drip scale
- animate a bead moving down a curve
- animate material wetness
- animate opacity/reveal mask
- render 2–3 incremental blood states

Only escalate to simulation if fake motion fails.

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
