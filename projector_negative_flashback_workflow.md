# Projector Negative Flashback Transition in DaVinci Resolve

## Core idea

This is not a normal flashback dissolve and not a digital glitch. The language is:

> a memory as a physical photographic object being pushed into a projector.

The feeling should be mechanical, imperfect, tactile, and slightly unstable.

The transition has three parts:

1. **Mechanical trigger** — projector click / slide lock sound.
2. **Physical insertion** — film/negative edge slides into the frame, briefly misaligned.
3. **Projected memory** — the image blooms, flickers, jitters, and settles.

The sound should begin slightly before the image changes. That makes the memory feel like it is being loaded into the mind, not just cut into the edit.

---

## What not to do

Avoid treating this like:

- VHS damage
- cyber glitch
- bad TV signal
- generic old-film overlay
- heavy scratch/dust effect
- fake sepia nostalgia

Those are different aesthetics. Your effect should feel closer to:

- slide projector
- film strip
- photo negative
- lens change
- mechanical gate
- imperfect projected light

A digital glitch says:

> the mind is broken.

A projector says:

> the mind is searching.

That is the key difference.

---

## Suggested timeline structure

Example:

```text
V4  Dust / scratches / optional overlay
V3  Adjustment Clip: projector flicker + Film Look Creator
V2  Flashback / memory snippets
V1  Present-day scene
A3  Film rustle
A2  Projector click / mechanical lock
A1  Main dialogue / scene audio
```

For each flashback snippet, aim for **1–2 seconds**. Shorter can work, but if everything is too short, the audience only feels “trailer montage.” For memory, give a few images enough time to breathe.

---

## Basic edit-page implementation

### 1. Put the memory footage above the present footage

Place the present-day scene on **V1**.

Place the memory/flashback clip on **V2**.

The memory should overlap the present-day scene by about **12–24 frames** if you want a double-exposure bleed-in.

---

### 2. Use composite mode for double exposure

Select the memory clip.

Go to:

```text
Inspector → Video → Composite
```

Try these composite modes:

- **Screen** — usually the safest for memory/projection.
- **Lighten** — useful if only highlights should appear.
- **Add** — stronger, more blown-out, more projector-like, but easier to overdo.

Start with:

```text
Composite Mode: Screen
Opacity: 40–70%
```

Then keyframe opacity:

```text
0% → 60% → 100%
```

or, for a ghostier effect:

```text
0% → 45% → 70% → 45% → 100%
```

That gives the feeling of the memory struggling into place.

---

## The projector-loading transition

The transition should feel like this:

```text
present scene
↓
projector click
↓
brief dark/bright interruption
↓
negative edge slides into view
↓
image is misaligned
↓
exposure blooms
↓
frame settles
↓
flashback plays
```

### Recommended timing

At 24 fps:

```text
Frames 01–02: projector click begins
Frames 03–05: quick blackout or white flicker
Frames 06–12: film/negative slides into frame
Frames 13–18: frame overshoots and settles
Frames 19 onward: memory stabilizes but keeps slight flicker
```

At 25 fps or 30 fps, the same idea holds. Do not make it too slow unless the film deliberately pauses on the ritual of remembering.

---

## Color treatment

You were right to focus on color. The projector feeling is not just grain. It is instability.

Modern digital footage is too perfect:

- exposure is fixed
- color temperature is fixed
- frame position is fixed
- focus is fixed
- geometry is fixed

Projected images are alive because they are unstable.

### What should pulsate?

#### 1. Brightness

This is the most important.

Projector light should gently fluctuate:

```text
100%
103%
98%
101%
99%
102%
```

Do not make it obvious. The viewer should feel it more than see it.

#### 2. Color temperature

Add small warmth/coolness drift:

```text
neutral → slightly warm → slightly cool → neutral
```

This can be done with keyframed temperature/tint, or with a Fusion color corrector.

#### 3. Gate weave

The frame should wander slightly:

```text
X: -1 to +1 pixels
Y: -1 to +1 pixels
Rotation: -0.2° to +0.2°
```

This is tiny, but very powerful.

#### 4. Focus softness

The memory can appear slightly soft, then sharpen over 6–10 frames.

That gives the feeling of a lens/projector finding focus.

---

## Using Film Look Creator

Yes, use **Film Look Creator**, but do not expect it to do the whole projector effect.

Film Look Creator is mostly for:

- film color response
- bloom
- halation
- grain
- print-style softness

It is not mainly for:

- film gate movement
- mechanical insertion
- projector click rhythm
- frame misalignment
- lamp pulsation

So treat Film Look Creator as the **film stock / print layer**, not the entire transition.

### Suggested Film Look Creator settings

Use these as starting points, not laws:

```text
Film Look Creator
Look Strength: low to medium
Grain: low to medium
Halation: subtle
Bloom: moderate
Color shift: subtle
```

Avoid heavy Film Damage unless the story really demands old archival footage. Too much dust and scratch becomes fake very fast.

For your effect:

```text
90% exposure instability
5% gate weave
5% dust/scratches
```

That will feel more real than dumping scratches everywhere.

---

## Sound design

The sound is half the transition.

Your preferred sequence is correct:

```text
projector click → film rustle → projected memory
```

The click should come first because it tells the viewer that something mechanical has engaged.

Then the film rustle implies handling, insertion, friction, physical material.

### Sound layers

Use 2–4 layers:

1. **Projector click / slide lock**
   - short, dry, mechanical
   - should happen 2–4 frames before the visual transition

2. **Film rustle / negative handling**
   - soft, papery, plasticky
   - starts immediately after the click

3. **Low thump or muted pressure hit**
   - optional
   - useful if the memory is emotionally heavy

4. **Projector hum / room tone**
   - optional, very low
   - can continue under the flashback snippets

### Timing

```text
Frame -04: faint pre-click room tension / silence dip
Frame  00: mechanical click
Frame +02: film rustle starts
Frame +04: image enters
Frame +08: brightness blooms
Frame +12: frame settles
```

The sound should slightly lead the picture. That makes the transition feel intentional and physical.

---

## Fusion transition template

I prepared a Fusion `.setting` template along with this document.

It creates a reusable node setup for:

- memory image sliding into frame
- slight rotation/misalignment
- brightness flicker
- negative-like color inversion/tint option
- gate weave
- soft blur/focus settle
- film-frame matte/border

### How to import it

1. In Resolve, create or select a Fusion clip / adjustment clip.
2. Go to the **Fusion** page.
3. Right-click in the node graph.
4. Choose:

```text
Paste Settings
```

or drag/drop the `.setting` file into the Fusion node area if your Resolve build supports it.

If Paste Settings does not appear, open the `.setting` file in a text editor, select all, copy, then paste directly into the Fusion node graph.

### How to use it

The template expects:

- **MediaIn1** = present / base image
- **MediaIn2** = flashback / memory image

If you only see one MediaIn, add the second clip to the Fusion composition or build this on a Fusion Clip containing both layers.

Then adjust the marked nodes:

```text
Projector_Insert_Transform
Projector_Flicker_CC
Negative_Tint_CC
Gate_Weave_Transform
Frame_Border_Rectangle
```

---

## Fusion node explanation

### MediaIn1: Present

The normal timeline image.

### MediaIn2: Memory

The flashback image or snippet.

### Projector_Insert_Transform

This animates the memory entering the frame like a film strip or negative being pushed into the projector gate.

Recommended movement:

```text
starts off-frame or partly off-frame
slides in
overshoots slightly
settles
```

Do not make the motion perfectly smooth. Mechanical movement should feel slightly abrupt.

### Projector_Flicker_CC

This creates brightness pulsation.

Animate gain/lift/gamma very subtly.

Good values:

```text
Gain: 0.95–1.08
Gamma: 0.98–1.03
```

### Negative_Tint_CC

This gives the memory a photographic negative / projected film color character.

You can either:

- keep it subtle and warm/orange
- push it toward cyan/blue negative tones
- reduce saturation for a ghost image

Avoid making it cartoonishly inverted unless you specifically want the viewer to think “negative image.”

### Gate_Weave_Transform

This adds the tiny frame movement.

Keep it tiny:

```text
X/Y movement: 0.001–0.004 in Fusion normalized coordinates
Rotation: 0.1–0.3 degrees
```

### Frame_Border_Rectangle

This creates the visible projector/film frame.

You can make it:

- black border
- orange negative border
- dirty white projection border
- thin frame-line only

For your taste, I would use a warm orange/brown negative-frame edge very briefly during insertion, then fade most of it out.

---

## Practical recipe for one flashback transition

### Edit page

1. Put present clip on V1.
2. Put flashback clip on V2.
3. Turn the flashback into a Fusion Clip if needed.
4. Add the Fusion transition template.
5. Add an adjustment clip above the flashback if you want Film Look Creator.

### Color page

On the flashback or adjustment clip:

1. Add Film Look Creator.
2. Add moderate bloom.
3. Add subtle halation.
4. Add low/medium grain.
5. Avoid heavy damage.

### Fusion page

1. Animate the memory sliding into position.
2. Add flicker.
3. Add gate weave.
4. Add border/negative edge.
5. Let the image settle, but not become perfectly stable.

### Fairlight page

Layer:

```text
projector click
film rustle
low emotional thump, if needed
faint projector hum, if useful
```

Keep it dry and tactile. Do not make it too cinematic unless the film’s tone allows it.

---

## Strong recommendation for Piraikottu

For this film, I would avoid loud “effect” language.

Use the projector idea almost like a ritual:

```text
click
rustle
image slips in
light breathes
memory appears
```

Let the imperfection carry the emotion.

This should not scream:

> flashback transition!

It should quietly say:

> a memory has been loaded.

That is much more beautiful.
