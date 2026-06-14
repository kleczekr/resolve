# ARRIRAW Blood VFX Stabilization Strategy

## Invocation

The goal is not to make the footage technically perfect. The goal is to make the
body read as dead, make the blood obey gravity, and keep the final shot from
feeling like a digital repair.

This note supersedes the earlier assumption that the blood shots should avoid
stabilization entirely. That was correct for a natural living micro-movement
look. It is not correct if the story need is a corpse-like body with no visible
breathing, no head wobble, and no hand sway.

The important distinction:

```text
Stabilize body parts only where the motion is narratively wrong.
Do not stabilize falling blood droplets unless you also preserve their world-space path.
```

## Shot Categories

There are two different problems here:

1. Head/chest shot: the actor's breathing and subtle head movement make the body
   look alive. Stabilization or freeze-style treatment may be needed.
2. Hanging-hand shot: the hand sways because of breathing, but blood droplets
   fall from the hand onto a surface below. Stabilizing the hand can break the
   physical truth of the falling droplets if handled carelessly.

These should not use one identical workflow.

## Core Principle

Separate the subject from the physics.

The body part can be stabilized, frozen, warped, or partially held. The falling
blood is a world-space event. Once a droplet leaves the body, it should continue
falling according to gravity, not inherit the body's stabilization.

That means:

```text
Attached blood/wetness: follows the stabilized body part.
Detached falling droplets: stay in original plate/world motion.
New CG droplets: must be rendered or transformed to match world gravity.
```

If this distinction is ignored, droplets will look like they are stuck to the
stabilized hand or sliding through space in the wrong direction.

## Shot 1: Head and Chest Need to Look Dead

### Problem

The head shot originally had minimal motion: slow in-breath, chest movement, and
tiny head movement. If the body is meant to be dead, even small breathing cues
can ruin the shot.

The chest and head also do not necessarily move the same way. Stabilizing the
whole frame from a single track can create bad artifacts:

- the chest may look fixed while the head still drifts
- the head may lock while the chest edge swims
- the background may warp or shimmer
- the wound/blood may slide if attached to the wrong motion basis

Treat the head and chest as separate regions.

### Recommended Approach

Use a layered Fusion comp:

```text
Original ARRIRAW plate
→ separate head matte
→ separate chest/body matte
→ stabilize or hold each region independently
→ rebuild edges over the live background
→ add/align blood
→ regrain and final comp
```

Do not stabilize the whole frame unless the camera itself is moving. If the
camera is essentially static, stabilize only the actor regions that betray life.

### Head Treatment

For the head, use one of these depending on how much motion remains:

#### Option A: Damped stabilization

Use a Fusion Tracker or Planar Tracker on rigid-ish facial/head features near
the wound. Apply stabilization, but do not necessarily force a perfect lock.

Good for:

- tiny head drift
- small tremor
- keeping the head photographic and alive as footage

Risk:

- if any breathing deformation is visible in the face/neck, a rigid stabilize
  may not fully remove it

#### Option B: Hold-frame head layer with edge repair

Use a still frame of the head region, masked over the live plate. This is more
aggressive but can sell death better if the shot is short.

Good for:

- corpse-like stillness
- removing head micro-shakes completely
- giving Blender a stable wound target

Risks:

- frozen grain looks dead in the wrong way
- mask edges can crawl against the live background
- lighting changes can reveal the patch

If using a hold-frame head:

- remove or soften the original grain on the held layer
- add fresh animated grain back on top
- feather the matte only where the edge is naturally soft
- keep hard anatomical edges precise
- avoid freezing visible falling droplets with the head layer

### Chest Treatment

The chest breathing is probably the bigger giveaway. Stabilizing the head alone
will not make the body read dead if the torso rises and falls.

For the chest:

```text
Magic Mask or roto chest/body region
→ Planar Tracker on torso fabric/skin area
→ stabilize or hold
→ patch/reveal original background around edges
```

The chest can often tolerate a stronger treatment than the head because it has
less expressive detail. If the shot is short, a held or partially held chest
layer may be acceptable.

Watch for:

- clothing folds that freeze unnaturally
- edge crawl along shoulder/neck
- background revealed by the original breathing motion
- contact shadows that no longer align

### Head and Chest Should Not Share One Track

Use separate tracks because breathing is non-rigid. The chest motion is the
source; the head may lag, rotate, or move less. A single stabilization transform
cannot describe both cleanly.

Recommended layer order:

```text
Background/live plate
→ stabilized or held chest/body layer
→ stabilized or held head layer
→ wound extension / static blood
→ moving blood/drip pass
→ foreground occlusion mattes
→ unified grain
```

### Blood in the Head Shot

Blood attached to the wound should follow the treated head layer.

Detached droplets should obey gravity. If droplets are already present in the
plate, decide whether they are:

- inside the held/stabilized head matte, in which case they may get frozen by
  mistake
- outside the matte, in which case they can remain live
- crossing the matte boundary, in which case they need manual roto or cleanup

For new Blender blood:

- build against the stabilized/held head reference if that is easier
- render static wound blood separately from active dripping blood
- composite attached wound blood with the head motion
- composite detached falling droplets in world motion

If the head layer is frozen but the new droplet is falling, the droplet should
not be frozen. The source point can be fixed on the wound, but after release the
drop follows gravity.

## Shot 2: Hanging Hand With Falling Blood Droplets

### Problem

The hand sways in frame because of breathing. You want the hand to look dead or
heavier, but there are blood droplets falling from the hand onto a surface
underneath.

This is more delicate than the head shot because the droplets are direct
evidence of gravity. If you stabilize the hand and accidentally stabilize the
falling droplets with it, the blood will look physically wrong.

### Rule

Stabilize the hand only while the blood is still attached to the hand.

Once blood detaches and becomes a falling droplet, it belongs to the world, not
the hand.

```text
Hand: can be stabilized.
Wet hand surface: follows hand.
Pendant droplet before release: follows hand.
Falling droplet after release: follows gravity/world plate.
Impact on surface: follows surface/world plate.
```

### Preferred Workflow

Use a hand-only treatment and protect the droplets.

```text
Original plate
→ hand matte from Magic Mask / roto
→ separate droplet mattes where needed
→ stabilize or damp hand
→ keep falling droplets from original plate, or recreate them
→ comp stabilized hand over live plate
→ comp droplets over/under correctly
```

Do not put the falling droplets inside the stabilized hand matte unless you
intend to replace them.

### Practical Fusion Strategy

Create at least three logical layers:

```text
Layer 1: live background / surface / original falling droplets
Layer 2: stabilized hand and attached wetness
Layer 3: repaired or added droplet elements
```

The live background layer preserves the real droplet fall. The stabilized hand
layer removes the breathing sway. The droplet layer lets you fix anything that
was damaged by the hand matte.

### Magic Mask Use

Magic Mask can help isolate the hand, but it will probably want to include
nearby blood because the color and motion are related. Do not trust it blindly.

After Magic Mask:

- inspect every frame where a droplet detaches
- remove falling droplets from the hand matte
- keep attached wetness and hanging pre-release droplets in the hand matte
- add manual roto for droplet frames if Magic Mask flickers

The matte boundary should be based on physical state, not just image color.

Ask of each red shape:

```text
Is it still attached to the hand?
    yes → hand layer
    no  → world/droplet layer
```

### How Much Stabilization Is Safe?

Use the minimum stabilization that removes the breathing read.

If the hand is gently swaying, a fully locked hand may look fake. Dead limbs can
still settle slightly, especially if the body or camera has tiny movement. The
goal is not "tripod-locked hand"; the goal is "not breathing-driven hand."

Start with:

- damped stabilization rather than hard lock
- lower strength / smoothed motion
- no perspective warp unless needed
- short hand matte with careful feathering

Escalate to a hold-frame only if the sway is too obvious.

### If Existing Droplets Must Be Preserved

If the real droplets look good, preserve them. They already have correct motion
blur, gravity, timing, and contact with the surface.

Workflow:

```text
1. Keep original plate as base.
2. Stabilize/hold only the hand region.
3. Roto holes or exclusions for falling droplets.
4. Let original droplets remain visible from the base plate.
5. Paint/patch hand edges where the stabilized hand covers or reveals artifacts.
```

This is usually better than recreating all droplets in Blender.

### If Stabilization Damages the Droplets

If the hand matte crosses over the falling blood and ruins the real drops, use
one of these:

#### Option A: Roto the real droplets back

Keep the original droplet pixels from the live plate and composite them over the
stabilized hand treatment.

Good when:

- the droplets are readable
- they do not pass directly behind the hand
- their motion blur is already good

#### Option B: Paint out damaged droplets and replace with CG/2D droplets

Use Blender or Fusion particles/painted elements for replacement droplets.

Good when:

- real drops are half-covered by the hand matte
- Magic Mask chewed the droplet edges
- the timing needs to change
- you need more blood anyway

If replacing droplets, match:

- fall direction
- acceleration
- motion blur length
- droplet size variation
- impact timing on the lower surface
- focus softness
- grain

### If Adding New Blender Droplets

For the hand shot, Blender should use the original plate orientation and world
gravity. Do not simulate droplets in a stabilized hand coordinate system unless
you later transform them correctly back to world motion.

The safest setup:

```text
Use original EXR plate as Blender background
→ roughly match camera/framing
→ animate hand source point if needed
→ simulate or keyframe droplets falling in world-down direction
→ render droplets over transparent alpha
→ composite droplets over the original/live plate
```

If the hand is stabilized in Fusion, the source point may be steadier in the
final comp than it was in the original plate. That means the new droplet's birth
point should visually attach to the stabilized hand, but after detachment the
drop should fall naturally.

In practice, this may mean manually keyframing the first few frames of a droplet:

```text
pre-release: follows hand
release frame: starts at hand/fingertip/wound source
post-release: follows gravity and stops inheriting hand stabilization
```

## Stabilize-Then-Restore vs Stabilize-For-Final

There are two different uses of stabilization:

### Stabilize-then-restore

Use this when stabilization is only a working convenience.

```text
stabilize plate → design/track/render blood → reapply original motion
```

Final image keeps original motion.

This is good for VFX alignment, but it does not solve the "body looks alive"
problem because the original motion comes back.

### Stabilize-for-final

Use this when the final shot itself must remove motion.

```text
isolate body part → stabilize/hold it → composite it over live plate
```

Final image changes the actor motion.

This is what the head/chest and hand shots may need.

Do not confuse these. For the corpse problem, the stabilization is not just a
Blender convenience. It is a final-image treatment.

## Recommended Per-Shot Plans

### Head/Chest Shot Plan

1. Export original EXR plate from Resolve before final grade.
2. In Fusion, create separate mattes for head and chest/body.
3. Stabilize or hold the chest enough to remove breathing.
4. Stabilize or hold the head separately.
5. Repair edges against the live background.
6. Build/align wound extension and blood to the treated head.
7. Keep active drips physically falling away from the temple.
8. Add grain/noise back over treated layers.
9. Composite and grade with the blood included.

### Hanging-Hand Shot Plan

1. Export original EXR plate from Resolve before final grade.
2. Create a hand matte with Magic Mask, then manually refine.
3. Exclude detached falling droplets from the hand matte.
4. Stabilize/dampen the hand, not the whole frame.
5. Preserve original droplets from the live plate where possible.
6. Roto real droplets back over the stabilized hand if needed.
7. Replace or augment damaged droplets only when necessary.
8. If adding CG droplets, make post-release motion world-gravity based.
9. Match blur, grain, focus, and impact timing.

## Common Failure Modes

### The Body Looks Like a Frozen Photograph

Cause:

- hold-frame layer without animated grain
- no lighting variation
- overly broad matte
- edge too smooth or too sharp

Fix:

- add fresh moving grain
- keep the hold region as small as possible
- preserve live edges where possible
- add subtle local warp only if needed

### Droplets Stick to the Stabilized Hand

Cause:

- falling droplets included in the hand matte
- hand stabilization applied after droplet composite
- CG droplets rendered in hand space for their entire fall

Fix:

- split attached and detached blood
- keep detached droplets in live/world layer
- composite droplets after hand stabilization

### Droplets Fall in the Wrong Direction

Cause:

- sim built on a stabilized or rotated reference
- Blender gravity not aligned to the real shot
- transform applied to droplets after release

Fix:

- use original plate orientation for droplet simulation
- confirm real screen-down/world-down direction from the footage
- manually keyframe post-release path if needed

### Stabilized Hand Reveals Background Gaps

Cause:

- original hand moved, stabilized hand no longer covers same pixels

Fix:

- use clean plate/paint patch underneath
- expand hand patch slightly where safe
- use live plate as base and only cover the necessary hand area

### Blood and Body Have Different Grain

Cause:

- EXR/CG blood is clean
- held body patch has frozen grain
- original plate has moving ARRIRAW texture

Fix:

- denoise or soften treated layer grain
- add matched moving grain to blood/body
- add final subtle grain over the whole comp

## Decision Table

| Situation | Best choice |
| --- | --- |
| Body should still feel alive | Damped stabilization or match-move only |
| Body must read dead | Stabilize-for-final or hold-frame body parts |
| Head and chest move differently | Separate head and chest tracks/mattes |
| Real falling droplets look good | Preserve them from the original plate |
| Droplets are attached to hand | Let them follow the hand |
| Droplets have detached | Keep them in world motion |
| Hand sway is small | Damped stabilization |
| Hand sway is obvious breathing | Stronger stabilization or hold-frame hand |
| Stabilization breaks blood trail | Split/roto droplets or replace them |

## Bottom Line

For the head/chest shot, it is reasonable to stabilize or hold the body parts
because the unwanted motion is the problem. Treat head and chest separately.

For the hanging-hand shot, stabilize the hand cautiously and protect the falling
blood. The hand can be steadied, but the detached droplets must keep their
gravity-driven path. The compositing problem is not simply "stabilize the shot";
it is "stabilize the dead limb while preserving world-space blood physics."
