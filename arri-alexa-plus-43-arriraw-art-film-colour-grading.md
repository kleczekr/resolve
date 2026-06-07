# ARRIRAW ART FILM COLOUR GRADING WORKFLOW

## ARRI ALEXA Plus 4:3 / Ultra Prime / Dim Candle-Lit Interiors

This is not a universal grading workflow. This is a project-specific approach
for an art film recorded in ARRIRAW on an ARRI ALEXA Plus 4:3 with Ultra Prime
lenses, built around dim light, deep shadows, candle-like irregularity, and the
need to make visible walls fall into deep, endless space.

The core idea:

**Do not make the image correct. Make the darkness believable.**

The grade should preserve the feeling that the characters are held by a small,
unstable pool of light, while the world behind them disappears.

---

## INTRODUCTORY

Darkness is never just the absence of light. In a dim interior, colour decides
whether black feels like cheap underexposure, theatrical mystery, spiritual
pressure, memory, exhaustion, or an actual room receding beyond sight. The ALEXA
negative may contain more information than the film should reveal, so the grade
must become an ethic of withholding: not every visible wall deserves to be seen,
not every shadow should be rescued, and not every face needs ordinary brightness
to remain human.

The philosophy of this workflow is to let colour behave like candlelight
behaves: local, unstable, partial, and alive. Warmth should come from sources,
not from a blanket orange wash. Faces and hands should be held just enough for
recognition, while backgrounds are allowed to lose their claim on the viewer.
The grade is not correcting the footage toward a neutral image; it is shaping a
believable threshold between presence and disappearance.

---

## WHAT THIS FOOTAGE WANTS

ARRIRAW from an ALEXA Plus 4:3 has enormous latitude, smooth highlight handling,
and very pliable shadows. That is a gift, but it is also a danger. The files may
let you see too much.

For this film, visible information is not automatically useful information.

You are not trying to:

- Recover every wall.
- Normalize every exposure.
- Make each shot evenly readable.
- Make blacks technically clean and open.
- Make candlelight neutral.

You are trying to:

- Hold faces and hands inside a small, fragile light field.
- Let backgrounds fall into dense, quiet black.
- Keep the light alive, uneven, and warm without turning orange.
- Preserve texture without revealing the set.
- Match the emotional darkness of close shots and wide shots.

---

## CAMERA AND COLOR ASSUMPTIONS

Use these as the starting assumptions unless the camera metadata says otherwise:

| Item                         | Likely Setting                                                     |
| ---------------------------- | ------------------------------------------------------------------ |
| Camera                       | ARRI ALEXA Plus 4:3                                                |
| Recording                    | ARRIRAW                                                            |
| Camera color science         | LogC3 / ALEXA Wide Gamut                                           |
| Lens family                  | ARRI/Zeiss Ultra Prime                                             |
| Working space recommendation | DaVinci Wide Gamut Intermediate                                    |
| Output                       | Rec.709 Gamma 2.4 for grading monitor, unless delivering otherwise |

Do not use LogC4 or ARRI Wide Gamut 4 for this camera. Those belong to newer
ARRI color pipelines. The ALEXA Plus 4:3 is a classic ALEXA-era LogC3/ALEXA Wide
Gamut camera.

---

## PROJECT SETUP IN DAVINCI RESOLVE

### Recommended Color Management

Use a color-managed workflow if possible:

1. Open **Project Settings -> Color Management**.
2. Set **Color Science** to `DaVinci YRGB Color Managed`.
3. Set **Timeline Color Space** to `DaVinci Wide Gamut Intermediate`.
4. Set **Output Color Space** to `Rec.709 Gamma 2.4` for a normal grading
   monitor.
5. Set ARRIRAW clips to `ARRI LogC3 / ALEXA Wide Gamut` as input, if Resolve
   does not detect this correctly.

This keeps the grade in a large working space and avoids making creative
decisions in cramped Rec.709 too early.

### Alternative Node-Based Input Transform

If you do not want project-level color management:

1. Keep project color science as DaVinci YRGB.
2. Use a Color Space Transform node near the start.
3. Input Color Space: `ARRI Wide Gamut`.
4. Input Gamma: `ARRI LogC3`.
5. Output Color Space: `DaVinci Wide Gamut`.
6. Output Gamma: `DaVinci Intermediate`.
7. Use another CST at the end for
   `DaVinci Wide Gamut Intermediate -> Rec.709 Gamma 2.4`.

Do not stack both approaches. Either use project-level color management or
node-based transforms.

---

## ARRIRAW DECODE SETTINGS

Before grading, check the raw decode settings.

Open the Camera RAW panel in Resolve:

```
Color page -> Camera RAW panel
Decode using: Clip
Color Space: ARRI Wide Gamut
Gamma: LogC
```

### Exposure Decode

Use ARRIRAW exposure controls carefully. They happen before the grade and can
shift the entire negative.

Use raw exposure when:

- A whole shot is globally too high or too low.
- The image is technically clipped or buried before grading.
- You need to normalize a scene before applying the common look.

Avoid raw exposure when:

- Only the face needs adjustment.
- Only the background is too visible.
- You are trying to make a moody shot brighter because the scopes feel low.

For this film, many shots should live low. Do not lift them just because the
waveform looks empty.

### White Balance Decode

Candle-like sources should not be neutralized. Keep warmth, but prevent the
grade from becoming muddy orange.

Use raw white balance only for large mistakes:

- A whole scene is accidentally too green.
- A shot clearly mismatches its neighbors because of camera setting drift.
- Skin cannot be recovered cleanly with normal color controls.

Otherwise, handle warmth and tint inside grading nodes.

---

## REFERENCE STRATEGY

Before touching the whole film, build references.

Choose:

1. One close shot where the face feels right.
2. One medium shot where candlelight shape feels right.
3. One wide shot where the background is too visible.
4. One darkest acceptable shot.
5. One brightest acceptable shot.

Grade those first. These are not final polish shots. They are anchors.

The close shot anchor tells you how faces should live. The wide shot anchor
tells you how much wall detail must disappear. The darkest shot tells you how
far the film is allowed to go.

---

## TARGET IMAGE PHILOSOPHY

### Blacks

The film needs deep blacks, but not video-crushed blacks.

Good black:

- Feels dense.
- Holds subtle texture near the subject.
- Lets the background disappear.
- Does not chatter with compression noise.
- Does not flatten the entire frame into a graphic silhouette unless intended.

Bad black:

- Clips into a dead digital floor.
- Makes hair, wardrobe, and background merge accidentally.
- Pulses between shots.
- Looks like contrast was simply dragged down.

### Faces

Faces do not need to be bright. They need to be locatable.

For dim candle-like interiors, faces may sit lower than normal:

| Face Type                             | Possible Luma Range |
| ------------------------------------- | ------------------- |
| Barely held in darkness               | 25-35 IRE           |
| Main candle-lit face                  | 35-50 IRE           |
| Moment of stronger emotional exposure | 50-60 IRE           |

These are not rules. They are guardrails. If the face feels alive at 30 IRE, do
not lift it to 50 just because a normal workflow says skin should live there.

### Highlights

There may not be many highlights. Protect the few that exist.

Candle flames, hot practicals, and small speculars can go high, but they should
feel soft and organic. ALEXA highlight rolloff is one of the reasons the image
can feel painterly. Do not hard-clip it with aggressive contrast.

### Color

Candle-like light is not simply orange. It contains:

- Warm amber/yellow near the source.
- Red/orange in skin and wood.
- Green contamination from walls, fabrics, or mixed fixtures.
- Cooler shadows when the eye adapts away from the flame.

The grade should keep warmth local. If the whole image becomes orange, the light
stops feeling like a source and starts feeling like a filter.

---

## CUSTOM NODE TREE

This is a better starting structure than the universal 10-node workflow.

```
01 RAW / INPUT NORMALIZATION
02 SCENE EXPOSURE ANCHOR
03 BLACK FLOOR / SHADOW SHAPE
04 FACE AND HAND HOLD
05 CANDLE COLOR FIELD
06 BACKGROUND SUPPRESSION
07 WALL REMOVAL / ENDLESS SPACE
08 LOCAL CONTRAST AND TEXTURE
09 FILM DENSITY / PRINT FEEL
10 SHOT MATCH TRIM
11 OUTPUT TRANSFORM / LEGAL SAFETY
```

For wide shots, `06` and `07` may become the most important nodes in the tree.

---

## 01 RAW / INPUT NORMALIZATION

Purpose: bring ARRIRAW into the working color space without imposing the look.

Do:

- Confirm LogC3/ALEXA Wide Gamut.
- Work in DaVinci Wide Gamut Intermediate or an equivalent scene-referred
  pipeline.
- Use raw exposure only for true global correction.
- Keep the image flatter than final.

Do not:

- Add contrast here.
- Force Rec.709 early.
- Neutralize candle warmth.
- Open shadows just to see what is there.

End state: a technically stable image with the ALEXA negative intact.

---

## 02 SCENE EXPOSURE ANCHOR

Purpose: decide where the scene lives emotionally.

Use Offset or HDR Global Exposure, not Lift/Gamma/Gain chaos.

Ask:

- Is this scene supposed to be seen, sensed, or almost lost?
- What must the viewer read?
- Where is the light source in the emotional logic of the shot?
- Is the face being revealed, concealed, or threatened by darkness?

For each scene, set one exposure anchor. Then match shots around that anchor.

Wide shots should not automatically be brighter because they were lit wider. If
they reveal too much wall, lower the background locally instead of pulling the
whole shot down and losing the actors.

---

## 03 BLACK FLOOR / SHADOW SHAPE

Purpose: create deep space without crude crushing.

Use a combination of:

- Custom Curves.
- HDR Shadows/Blacks.
- Log wheels with carefully set ranges.
- Luma vs Sat to calm noisy chroma in darkness.

### Practical Method

1. Open waveform.
2. Identify the darkest important subject detail: hair edge, shoulder, hand,
   cheek shadow.
3. Bring irrelevant background close to black.
4. Keep subject-adjacent shadow barely alive.
5. Do not let the entire lower waveform flatten into one hard line unless the
   shot is meant to become silhouette.

### Shadow Curve Shape

Use a soft toe:

```
Pure black      -> held low
Near black      -> compressed, not clipped
Subject shadow  -> still separable
Face midtone    -> protected from the toe
```

The toe should feel like darkness thickening, not like a digital guillotine.

---

## 04 FACE AND HAND HOLD

Purpose: keep human presence alive inside the darkness.

In this film, faces and hands may be the only stable readable objects. Treat
them as islands of attention.

Use:

- Soft Power Windows.
- Qualifier plus window, if needed.
- Magic Mask only if the shot requires it and the track is worth preserving.
- Very gentle Gamma/HDR Light adjustments.
- Very gentle contrast reduction if skin is too harsh.

Do not brighten faces globally. Shape them locally.

### Face Node Starting Moves

- Lift face exposure slightly if it is lost.
- Add tiny warmth if candle direction supports it.
- Reduce green contamination if skin is sickly.
- Avoid smoothing unless digital texture becomes distracting.
- Keep one side of the face allowed to fall away.

### Important

Do not make both eyes equally readable unless the shot asks for that.
Candle-like lighting often needs asymmetry. A face half-lost can be more
truthful than a face corrected into evenness.

---

## 05 CANDLE COLOR FIELD

Purpose: make the warmth feel sourced, irregular, and alive.

This node is not "make it orange." It is source behavior.

Use:

- Curves.
- HDR wheels.
- Hue vs Hue.
- Hue vs Sat.
- Color Warper if needed.
- Windows around practical source influence.

### Candle-Like Color Principles

Near source:

- Warmer.
- Slightly more saturated.
- More yellow/amber than red.

Away from source:

- Less saturated.
- Cooler or more neutral.
- Allowed to fall into brown/black, not orange-black.

Skin:

- Warm, but not uniformly orange.
- Preserve red/yellow variation.
- Remove green contamination carefully.

### Common Failure

If walls, wardrobe, skin, and shadows are all the same amber color, the grade
has become a wash. Reduce global warmth and rebuild warmth locally.

---

## 06 BACKGROUND SUPPRESSION

Purpose: reduce visible set/background information without harming actors.

Use this for shots where lighting needed to be wider and the walls became too
readable.

Tools:

- Large soft Power Windows.
- Luma qualifiers.
- Depth Map, only if it works cleanly.
- Magic Mask/external matte for actors if necessary.
- Layer Mixer or parallel nodes for subject/background split.

### Basic Background Suppression Node

1. Add a serial or parallel node after the face/subject hold.
2. Create a large window covering the visible walls/background.
3. Feather heavily.
4. Exclude actors with a separate window, qualifier, or matte if needed.
5. Lower Gamma or HDR Shadows/Midtones, not just Lift.
6. Reduce saturation slightly in the background.
7. Soften detail very slightly if the wall texture calls attention to itself.

Starting values:

```
Gamma / HDR Midtones: down slightly
Shadows: down slightly
Saturation: -5 to -15
Midtone Detail: -5 to -15 if texture is too visible
```

Never make the background look like a vignette pasted onto the frame. It should
feel like light naturally dies before reaching the wall.

---

## 07 WALL REMOVAL / ENDLESS SPACE

Purpose: create the illusion that the room falls into infinite darkness behind
the characters.

This is more specific than background suppression. Use it when visible walls
break the illusion of deep space.

### The Endless Space Method

1. Identify the wall planes that reveal the actual room.
2. Create broad, soft windows over those planes.
3. Track if the camera moves.
4. Lower the wall exposure until edges and texture recede.
5. Reduce wall saturation.
6. Reduce wall sharpness or midtone detail.
7. Preserve any motivated candle spill near characters.
8. Let darkness increase with distance from the actors.

### Do Not Simply Crush the Wall

Walls should disappear because light falls off, not because a black patch was
placed on them.

Bad wall removal:

- Obvious black blob.
- Window edge visible.
- Actor enters the darkened area and gets dimmed.
- Wall texture still visible but darker, which can look like dirty paint.

Good wall removal:

- The viewer stops reading the wall as a surface.
- Depth behind characters feels larger.
- Light still behaves plausibly near candles and faces.
- The subject remains separated.

### Useful Node Layout for Wide Shots

```
01 Input
02 Scene Exposure
03 Black Floor
04 Subject Hold
05 Candle Color
06 Background Suppression
07 Wall Plane A
08 Wall Plane B
09 Edge/Corner Falloff
10 Texture/Density
11 Output
```

Treat each visible wall plane separately if needed. One giant vignette rarely
works on wide shots.

---

## 08 LOCAL CONTRAST AND TEXTURE

Purpose: keep the image tactile without making darkness noisy or walls too
visible.

Ultra Primes are clean and controlled. ARRIRAW is detailed. Dim art-film
lighting often benefits from a small reduction in digital clarity, not extra
sharpness.

Use:

- Midtone Detail gently.
- Very subtle blur or mist if needed.
- Grain after the main grade, not before.
- Noise reduction only where it protects the image.

### Starting Points

| Problem                 | Move                                                            |
| ----------------------- | --------------------------------------------------------------- |
| Skin too crisp          | Midtone Detail -5 to -15 in a face/window node                  |
| Walls too readable      | Midtone Detail -10, slight blur, lower saturation               |
| Shadows noisy           | Luma vs Sat down in shadows, temporal NR if necessary           |
| Image too clean/digital | Add subtle grain after contrast is set                          |
| Image too soft          | Do not sharpen globally; add local contrast only to faces/hands |

Do not sharpen the whole image. It will bring back the walls you are trying to
lose.

---

## 09 FILM DENSITY / PRINT FEEL

Purpose: give the grade a coherent density after the shot-level work is done.

This should be subtle. The film already has strong photographic ingredients:
ARRIRAW, ALEXA rolloff, Ultra Primes, low light, and motivated warm sources.

Possible moves:

- Gentle print-style contrast curve.
- Slight highlight warmth retention.
- Slight shadow cool/neutral density.
- Low global saturation with localized warm source energy.
- Film grain for texture and compression resistance.

Avoid:

- Heavy teal-orange.
- LUT intensity at 100%.
- Milky lifted blacks unless that is the concept.
- Crushed music-video contrast.
- Uniform sepia.

The look should feel like a negative interpreted with restraint, not a plugin
laid over the image.

---

## 10 SHOT MATCH TRIM

Purpose: match emotional continuity, not mathematical continuity.

For this film, shot matching must prioritize:

1. Face readability.
2. Direction and strength of candle-like light.
3. Depth of background darkness.
4. Shadow density.
5. Color temperature relationship between shots.

Wide shots may have been lit differently from close shots. The grade should hide
that production reality.

### Matching Wide Shots to Close Shots

When a wide shot feels over-lit:

1. Do not immediately lower the whole shot.
2. Match the actor's face/hands to the close-shot anchor.
3. Darken background planes separately.
4. Reduce wall saturation and texture.
5. Add falloff so light feels motivated from the same candle/practical logic.
6. Compare with the close shot in image wipe.

The viewer should feel the same room and the same darkness, even if the wide
shot was physically lit more.

---

## 11 OUTPUT TRANSFORM / LEGAL SAFETY

Purpose: convert to delivery space and catch technical issues.

Use:

- CST or project-level output transform to Rec.709 Gamma 2.4.
- Soft clipping, not hard clipping.
- Video limiter only if delivery requires it.

Check:

- Candle highlights do not clip harshly.
- Shadows do not macroblock badly in export.
- Background darkness survives compression.
- Faces remain readable on a normal monitor.

For very dark films, always test an H.264/H.265 export. A grade that looks
beautiful in Resolve can fall apart when compressed if the shadows are noisy or
too close to the codec floor.

---

## WIDE SHOT SPECIAL WORKFLOW

Use this when the walls are visible but should feel like endless darkness.

### Step 1: Protect the Actors

Before darkening anything, isolate the actors enough that the background work
cannot damage them.

Options:

- Soft oval windows around actors.
- Magic Mask for people.
- External matte if Magic Mask tracking is expensive.
- Qualifier plus window for skin/hands.

### Step 2: Establish the Desired Darkness

Find a close or medium shot that has the correct darkness. Use it as reference.

Ask:

- How black is the space behind the actor?
- How fast does light fall off?
- Is there any readable wall texture?
- Does the background have color, or only density?

### Step 3: Treat the Background by Planes

Do not use one universal darkening move.

Separate:

- Back wall.
- Side wall.
- Corners.
- Floor if visible.
- Ceiling if visible.
- Doorways/openings.

Each plane may need a different falloff.

### Step 4: Remove Information in This Order

1. Lower exposure.
2. Reduce saturation.
3. Reduce midtone detail.
4. Blur very slightly if texture still calls attention.
5. Add grain after, so the dark area does not become plastic.

This order is important. If you blur first, the background may look like an
effect. If you crush first, it may look artificial.

### Step 5: Preserve Motivated Spill

If a candle or practical would light the wall near the actors, keep a small
amount of spill. The darkness should not contradict the light source.

The trick is not "no wall." The trick is "the wall exists only where light
plausibly reaches it."

### Step 6: Check Movement

Scrub the shot. Look for:

- Window edges.
- Actor crossing into darkened wall windows.
- Background pulsing.
- Track slipping.
- Sudden disappearance of wall texture.

If the window is visible in motion, it is too strong or too hard.

---

## SCOPES FOR THIS FILM

Scopes are useful, but they should not bully the grade into normality.

### Waveform

Expect a low waveform. That is fine.

Watch for:

- Important face detail not disappearing completely.
- Background black floor not clipping into an ugly flat line.
- Candle/practical highlights not hard-clipping.
- Shot-to-shot black level consistency.

### Vectorscope

Watch for:

- Skin drifting too green.
- Warmth becoming global orange.
- Shadows becoming saturated red/brown noise.
- Candle warmth retaining direction and locality.

### False Color, If Available

False color can help separate:

- Faces that are too low to read.
- Walls that are too high and visible.
- Candle highlights that are too hot.
- Background areas that need to fall below subject-adjacent shadows.

---

## COMMON PROBLEMS AND FIXES

| Problem                                   | Likely Cause                              | Fix                                                     |
| ----------------------------------------- | ----------------------------------------- | ------------------------------------------------------- |
| Wide shots feel theatrical                | Background/walls too visible              | Darken wall planes separately, reduce saturation/detail |
| Close shots feel rich but wides feel flat | Wides were lit for coverage               | Match faces first, then suppress background             |
| Candlelight looks orange                  | Warmth applied globally                   | Reduce global warmth, add local source warmth           |
| Shadows look muddy brown                  | Too much saturation in low luma           | Use Luma vs Sat to reduce shadow saturation             |
| Faces disappear                           | Whole image pulled down                   | Restore faces/hands locally, not globally               |
| Walls look like black patches             | Windows too hard or too strong            | Feather more, split planes, preserve spill              |
| Image feels digital                       | Too much sharpness/local contrast         | Reduce Midtone Detail, add subtle grain                 |
| Black is dead                             | Toe is too hard                           | Lift near-black slightly while keeping wall planes dark |
| Shot matching feels wrong                 | Matching scopes instead of dramatic light | Match face, source direction, and background depth      |

---

## PRACTICAL GRADING ORDER FOR THE FILM

1. Set up color management and ARRIRAW decode.
2. Pick five reference shots.
3. Build the look on the references only.
4. Create a PowerGrade from the custom node tree.
5. Apply it scene by scene.
6. Match faces and hands first.
7. Shape black floor and candle warmth.
8. Fix wide-shot backgrounds.
9. Add density/texture/grain.
10. Test export compression.
11. Do final shot-matching pass.

Do not grade every shot vertically from raw to finished. Work horizontally by
scene:

1. Exposure anchors for all shots.
2. Black floor for all shots.
3. Face/hand hold for all shots.
4. Background suppression for all shots.
5. Candle color field for all shots.
6. Texture and density for all shots.
7. Final trim.

---

## THE RULES FOR THIS PROJECT

1. Darkness is part of the image, not a defect.
2. The background does not deserve equal truth.
3. Faces can be dim if they are emotionally readable.
4. Candle warmth must feel local.
5. Wide shots must be bent toward the close-shot reality.
6. Wall detail is usually the enemy.
7. Do not recover information just because ARRIRAW allows it.
8. Preserve the ALEXA rolloff.
9. Keep the grade quiet.
10. The viewer should feel space behind the characters, not inspect the room.

---

## FINAL IMAGE TEST

After grading a scene, watch it without scopes.

Ask:

- Do I believe the darkness?
- Do the characters remain emotionally readable?
- Does the light feel like it comes from candles/practicals?
- Do the wide shots reveal the set?
- Does the background fall away without looking artificially masked?
- Does the film feel darker, or only underexposed?

The target is not a technically perfect image.

The target is a controlled darkness that feels inevitable.
