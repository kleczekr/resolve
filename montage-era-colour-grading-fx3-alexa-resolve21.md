# MONTAGE ERA COLOUR GRADING WORKFLOW

## Resolve 21 Studio / FX3 Cine EI / ALEXA / 1960s, 1970s, 1980s Montage Sequences

This document is for three montage sequences that must feel like different
historical/emotional periods without becoming fake period filters.

The sources are mixed:

| Montage | Camera   | Likely Capture                              |
| ------- | -------- | ------------------------------------------- |
| 1960s   | Sony FX3 | Cine EI, S-Log3 / S-Gamut3.Cine             |
| 1970s   | Sony FX3 | Cine EI, S-Log3 / S-Gamut3.Cine             |
| 1980s   | ALEXA    | LogC / ALEXA Wide Gamut or ARRIRAW pipeline |

The job has two separate layers:

1. **Camera reconciliation:** make FX3 and ALEXA sit inside the same film world.
2. **Era interpretation:** bend each montage toward a decade-specific colour
   memory.

Do not skip the first step. If the FX3 is not disciplined before the look, the
1960s and 1970s sequences will feel like modern Sony footage wearing a vintage
preset.

---

## INTRO

Montage is already a machine for memory. The viewer is not only asking what
happened; they are asking when it belongs, how distant it feels, what kind of
emotional weather surrounds it, and whether the image seems remembered,
discovered, archived, imagined, or lived. Colour can make time legible without
turning the frame into a costume. It can suggest a decade through limits,
texture, softness, density, and restraint rather than through obvious period
tricks.

The philosophy here is to reconcile cameras first, then let each era have its
own relationship to colour. The FX3 and ALEXA do not need to become identical,
but they must stop arguing about what kind of film they are in. Once they share
a world, the 1960s, 1970s, and 1980s looks can become emotional interpretations
rather than presets. The grade should make each montage feel historically and
psychologically specific while still belonging to the same larger film.

---

## CORE PRINCIPLE

**Era colour is not a LUT. Era colour is a set of limits.**

Each decade should have a different relationship to:

- Saturation.
- Black density.
- Highlight softness.
- Skin warmth.
- Colour separation.
- Grain.
- Halation.
- Lens/print texture.
- How much the image feels chemically unstable.

The grade should suggest period through restraint, not announce it.

---

## RESOLVE 21 STUDIO TOOLS TO USE

Resolve 21 Studio gives you enough built-in tools to do this without third-party
film emulation.

Useful tools:

| Tool                                  | Use                                                      |
| ------------------------------------- | -------------------------------------------------------- |
| Color Space Transform                 | Camera normalization                                     |
| DaVinci Wide Gamut Intermediate       | Common working space                                     |
| HDR wheels                            | Controlled exposure and tonal range                      |
| Custom Curves                         | Print-style contrast shaping                             |
| Color Warper                          | Era-specific palette steering                            |
| Hue vs Sat / Hue vs Hue / Luma vs Sat | Controlled colour limits                                 |
| Film Look Creator                     | Grain, halation, bloom, gate weave, print-style shaping  |
| Glow / Halation-style effects         | Motivated highlight behavior                             |
| Depth Map                             | Separate foreground/background period treatment if clean |
| Magic Mask                            | Isolate people or key props when needed                  |
| Face Refinement                       | Only for subtle cleanup, not beauty grading              |
| Texture Pop / Midtone Detail          | Control digital sharpness                                |
| Grain                                 | Era texture and compression protection                   |

Use Film Look Creator as a controlled component, not as the entire look. Build
the image first, then use it for film characteristics.

---

## PROJECT COLOR MANAGEMENT

Use a common working space for all three montages.

Recommended:

1. Project Settings -> Color Management.
2. Color Science: `DaVinci YRGB Color Managed`.
3. Timeline Color Space: `DaVinci Wide Gamut Intermediate`.
4. Output Color Space: `Rec.709 Gamma 2.4` for normal grading monitor.
5. FX3 Input: `Sony S-Gamut3.Cine / S-Log3`.
6. ALEXA Input: `ARRI Wide Gamut / LogC3` unless the metadata/pipeline indicates
   otherwise.

Alternative node-based setup:

```
FX3 CST:
Input Color Space: Sony S-Gamut3.Cine
Input Gamma: Sony S-Log3
Output Color Space: DaVinci Wide Gamut
Output Gamma: DaVinci Intermediate

ALEXA CST:
Input Color Space: ARRI Wide Gamut
Input Gamma: ARRI LogC3
Output Color Space: DaVinci Wide Gamut
Output Gamma: DaVinci Intermediate
```

Use one approach only. Do not apply both color-managed input and CST input
transforms.

---

## CAMERA RECONCILIATION BEFORE ERA LOOKS

The FX3 and ALEXA should not be forced to look identical, but they must belong
to the same film.

### FX3 Tendency

FX3 S-Log3 can feel:

- Cleaner and more digital.
- Sharper at edges.
- More video-like in saturated colour.
- Less graceful in clipped highlights.
- More chroma-noisy in underexposed shadows.

### ALEXA Tendency

ALEXA tends to feel:

- Softer in highlight rolloff.
- More organic in skin.
- More natural in colour separation.
- More forgiving in mixed light.
- Denser without becoming brittle.

### FX3 Reconciliation Moves

Before applying the 1960s or 1970s era look:

1. Normalize S-Log3 correctly.
2. Reduce digital sharpness if needed.
3. Protect highlights with soft rolloff.
4. Lower saturation in the deepest shadows.
5. Use a gentle print-style curve instead of hard contrast.
6. Add subtle grain after tonal shaping.
7. Avoid pushing global saturation.

Starting corrections:

| Problem                | Move                                                             |
| ---------------------- | ---------------------------------------------------------------- |
| FX3 too sharp          | Midtone Detail -5 to -15, or Texture Pop down subtly             |
| Highlights feel video  | Custom curve shoulder, Film Look Creator halation/bloom very low |
| Shadows noisy          | Luma vs Sat down in low luma, temporal NR if necessary           |
| Colours too clean      | Reduce global saturation, rebuild selected colours               |
| Skin too magenta/green | Balance with vectorscope, do not over-warm                       |

---

## GLOBAL MONTAGE NODE TREE

Use the same skeleton for all three sequences.

```
01 INPUT TRANSFORM / CAMERA RAW
02 CAMERA RECONCILIATION
03 ERA BASE CONTRAST
04 ERA PALETTE
05 SKIN / HUMAN HOLD
06 SOURCE-SPECIFIC FIXES
07 FILM TEXTURE / GRAIN / HALATION
08 MONTAGE RHYTHM TRIM
09 OUTPUT TRANSFORM / SAFETY
```

For FX3 montages, node `02 CAMERA RECONCILIATION` matters heavily.

For ALEXA 1980s montage, node `02` may be lighter, but still use it to align
with the rest of the film.

---

## 1960s MONTAGE

### Concept

The 1960s sequence should feel older than the camera, but not like a fake
home-movie filter unless that is the explicit intention.

Think:

- Eastmancolor / older colour print memory, not a modern vintage filter.
- Slightly restrained palette.
- Warm skin and practicals.
- Softer contrast than modern digital.
- Blacks present but not aggressively crushed.
- Gentle colour instability.
- A little less precision.

The 1960s can have optimism or fragility depending on the scene, but it should
not feel slick.

The practical target is a soft colour print feeling: creamy highlights, fine
grain, held blacks, and colours that feel slightly rounded off. If the image
looks like a clean digital shot with a warm LUT, it is not there yet.

### Tonal Shape

Use:

- Softer toe than the main film.
- Softer shoulder.
- Moderate contrast.
- Slightly lifted near-blacks if the footage feels too modern.

Avoid:

- Deep modern black density.
- Harsh microcontrast.
- Extremely clean whites.

Suggested curve:

```
Shadows: held, not crushed
Lower mids: gently full
Faces: soft and readable
Highlights: rounded, never sharp
```

### Palette

Good directions:

- Creamy warm highlights.
- Muted greens.
- Slightly cyan/soft blue shadows if appropriate.
- Reds controlled, not electric.
- Yellows warm but not neon.

Reduce:

- Modern saturated blues.
- Digital greens.
- Magenta skin contamination.
- Extremely clean white balance.

### Practical Node Moves

1. In `ERA BASE CONTRAST`, use Custom Curves to make a gentle S-curve. Pull the
   lower toe down only a little, then lift the very bottom slightly if the blacks
   become too modern. Round the upper shoulder so white fabric, windows, and skin
   sheen do not snap to video white.
2. In `ERA PALETTE`, use Color Warper or Hue vs Sat to reduce clean Sony blues
   and bright greens. Keep blues softer and slightly cyan. Push greens toward
   muted green/olive, but stop before they become yellow mud.
3. Use HDR wheels or Log wheels to add a tiny warm bias to highlights only. Do
   not warm the whole image with Offset unless the shot is truly too cold.
4. Use Hue vs Sat to reduce red saturation before it becomes lipstick-red.
   1960s reds should feel printed, not electric.
5. Use Luma vs Sat to reduce saturation slightly in the deepest shadows. This
   helps the image feel like older colour negative/print rather than clean
   digital darkness.
6. If the shot feels too sharp, reduce Midtone Detail slightly or add a tiny
   amount of diffusion before grain. Do this before Film Look Creator.
7. Add subtle grain, finer than 1970s. The grain should be felt in motion, not
   seen as a special effect on a paused frame.
8. Use Film Look Creator lightly. It should provide film behavior, not the whole
   1960s look.

### 1960s Film Look Creator Starting Point

Use Film Look Creator after the contrast and palette nodes.

Starting point:

```
Overall amount / blend: low, roughly 20-35%
Grain: fine, low to low-medium
Halation: very low
Bloom: very low, only for motivated highlights
Gate weave: off, or the smallest usable amount
Vignette: very subtle, only if it supports the shot
Saturation/print effect: restrained rather than faded
```

How to judge it:

- Toggle the Film Look Creator node on and off. The shot should gain texture and
  softness, not suddenly become a different grade.
- If faces change hue when the node turns on, reduce the effect or protect skin
  before it.
- If the shot becomes "old footage" instead of "1960s memory," pull back gate
  weave, bloom, and grain first.

### 1960s Fast Recipe

For a normal FX3 1960s montage shot:

1. Normalize S-Log3/S-Gamut3.Cine cleanly.
2. Build a soft print curve: gentle toe, rounded shoulder.
3. Warm highlights slightly; keep shadows neutral or faintly cool.
4. Mute greens and strong blues.
5. Reduce red purity.
6. Protect skin with a qualifier or gentle Hue vs Hue/Hue vs Sat correction.
7. Add fine grain and very low halation in Film Look Creator.
8. Watch in motion and remove anything that reads as a preset.

### 1960s Danger Signs

- Looks like Instagram vintage.
- Skin turns peach/orange.
- Greens become yellow mud.
- Blacks are lifted so much the image feels washed out.
- Grain is too visible for the emotional tone.

---

## 1970s MONTAGE

### Concept

The 1970s sequence can be earthier, denser, and more unstable than the 1960s. It
can tolerate more dirt, more brown/green, more tobacco warmth, and more shadow
weight.

Think:

- Earth tones.
- Lower chroma.
- Heavier mids.
- Slightly dirtier whites.
- More texture.
- More environmental colour contamination.
- Less polished skin.

This is not just "make it brown." It is a narrower, more grounded colour world.
The practical target is a heavier Eastman/Kodak-style print memory: denser lower
mids, dirtier whites, ochre/olive/tobacco bias, faded reds, and more texture
than the 1960s.

### Tonal Shape

Use:

- Denser lower mids.
- Deeper blacks than the 1960s.
- Less brilliant highlights.
- Slightly compressed upper range.

Avoid:

- Clean high-key contrast.
- Sparkling whites.
- Modern colour separation.

Suggested curve:

```
Shadows: denser than 1960s
Lower mids: heavy and textured
Faces: warm but not beautified
Highlights: restrained and slightly dirty
```

### Palette

Good directions:

- Ochre.
- Tobacco.
- Olive.
- Faded red.
- Warm brown.
- Dirty cream.
- Muted cyan only if motivated.

Reduce:

- Pure blue.
- Pure green.
- Clean magenta.
- Bright digital red.
- Cold white.

### Practical Node Moves

1. In `ERA BASE CONTRAST`, add density in lower mids without crushing faces.
   Use Log/HDR midtone controls or a curve point around the lower-mid range. The
   image should feel heavier, not simply darker.
2. Pull the very top of the highlights down slightly or round the shoulder. The
   1970s look should not have sparkling digital whites.
3. In `ERA PALETTE`, use Hue vs Sat to reduce blues and clean greens. Pure blue
   is usually the first colour that makes the shot feel modern.
4. Use Hue vs Hue to push some greens toward olive, not yellow. If foliage,
   clothing, or walls become mustard, you pushed too far.
5. Move some reds toward faded red/brick. Do not let red objects stay clean and
   synthetic.
6. Warm highlights slightly, but keep whites dirty rather than golden. A dirty
   cream white is better than a yellow white.
7. Use Luma vs Sat to reduce saturation in shadows. Let environmental colour
   contamination live in the mids, not as noisy chroma in the blacks.
8. Reduce Midtone Detail or Texture Pop slightly if the FX3 image is too crisp.
   This is especially important on skin, hair, fabric, and wall texture.
9. Add more grain than 1960s, but keep the grain scale believable. The grain
   should unify the montage; it should not hide bad matching.
10. Add very low halation around practicals if present.
11. Consider a tiny amount of gate weave in Film Look Creator only if it supports
    montage memory and does not distract.

### 1970s Film Look Creator Starting Point

Starting point:

```
Overall amount / blend: low-medium, roughly 30-45%
Grain: medium, more visible than 1960s
Halation: low, only around motivated practicals or bright edges
Bloom: low or off
Gate weave: off to tiny
Vignette: subtle to moderate if the shot benefits from heavier edges
Saturation/print effect: muted, earthy, lower chroma
```

How to judge it:

- Toggle the node while watching skin. Skin can be warmer and less polished, but
  it must not become muddy.
- If whites become urine-yellow, reduce warmth and rebuild the dirty-white feel
  with lower saturation and softer contrast instead.
- If the image becomes a brown wash, restore separation with controlled skin
  warmth, a little coolness in shadows, or selective hue separation in Color
  Warper.

### 1970s Fast Recipe

For a normal FX3 1970s montage shot:

1. Normalize the camera image.
2. Add lower-mid density.
3. Restrain highlights and dirty the whites.
4. Push greens toward olive and reds toward faded brick.
5. Reduce pure blues.
6. Keep skin warm but imperfect.
7. Reduce digital crispness.
8. Add medium grain and low halation in Film Look Creator.
9. Compare against the 1960s look: this should feel heavier, earthier, and less
   delicate.

### 1970s Danger Signs

- Everything becomes brown.
- Faces become muddy.
- Whites become urine-yellow.
- Grain overwhelms the shot.
- The image loses separation between actor and background.

---

## 1980s MONTAGE

### Concept

The 1980s montage is on ALEXA, so it will naturally have a richer, more filmic
base than the FX3 material. The era look can be cleaner, bolder, and more
graphic than the 1960s/1970s, but it should still belong to the main art film.

There are many possible 1980s looks. Choose one direction and stay consistent:

1. **Clean commercial 1980s:** clearer colours, stronger contrast, controlled
   saturation.
2. **Moody late-1980s:** deeper shadows, cyan/blue contamination, practical
   warmth.
3. **Analog domestic 1980s:** softer, grainier, less polished, slightly faded
   colour.

Do not mix all three unless the montage is intentionally fractured.

### Tonal Shape

Compared to the 1960s and 1970s:

- Blacks can be cleaner and deeper.
- Highlights can be slightly brighter.
- Contrast can be more decisive.
- Colour separation can be stronger.

But because this is still part of a dim art film, avoid a glossy music-video
look unless that is the point.

### Palette Options

#### Clean Commercial 1980s

- Stronger reds.
- Cleaner blues.
- Neutral-to-cool shadows.
- Controlled warm skin.
- Crisp but not digital contrast.
- Lower dirt than 1970s.
- Highlights can be brighter, but still controlled.

#### Moody Late-1980s

- Cyan/blue shadows.
- Warm practicals.
- Deeper blacks.
- Lower saturation in backgrounds.
- More pronounced separation between warm faces and cool space.
- Stronger contrast than the 1960s/1970s, but not music-video gloss unless that
  is the point.

#### Analog Domestic 1980s

- Softer contrast.
- Faded primaries.
- Slightly warm whites.
- Visible grain.
- Lower sharpness.
- More imperfect and intimate than the clean commercial version.

### Practical Node Moves

1. Use the ALEXA material's natural rolloff. Do not over-process it to match
   FX3. Let the 1980s montage have a cleaner image base.
2. In `ERA BASE CONTRAST`, allow more contrast than 1960s/1970s. Keep blacks
   cleaner and more decisive, but do not crush detail that carries the scene.
3. In `ERA PALETTE`, choose one palette lane and stay there for the sequence.
   Do not mix clean commercial, moody late-1980s, and analog domestic cues in
   the same montage unless the story requires that fracture.
4. For clean commercial 1980s, use Color Warper or Hue vs Sat to let selected
   reds/blues hold more saturation than the 1970s, while keeping skin controlled.
5. For moody late-1980s, use Log/HDR wheels or Color Warper to cool shadows
   toward cyan/blue and keep practicals/skin warm. This is the most useful lane
   if the montage still needs to belong to the dark art-film world.
6. For analog domestic 1980s, soften contrast, reduce sharpness, fade primaries
   slightly, and allow more grain.
7. Add grain, but less dirty than 1970s unless the footage asks for it.
8. Use halation carefully around practicals. ALEXA already gives you a graceful
   highlight base, so heavy halation can become decorative fast.
9. If using Film Look Creator, keep the film effects lower than the grade
   itself. The ALEXA image already carries softness and highlight grace.

### 1980s Film Look Creator Starting Point

Choose the lane first, then set Film Look Creator.

Clean commercial 1980s:

```
Overall amount / blend: low, roughly 15-30%
Grain: low to low-medium
Halation: very low to low
Bloom: low to medium only on motivated highlights
Gate weave: off
Vignette: off or very subtle
Saturation/print effect: cleaner, stronger, but controlled
```

Moody late-1980s:

```
Overall amount / blend: low to low-medium, roughly 20-35%
Grain: low-medium
Halation: low around practicals
Bloom: low, mostly for warm light sources
Gate weave: off
Vignette: subtle if it helps deepen the frame
Saturation/print effect: cooler shadows, warm practicals, controlled skin
```

Analog domestic 1980s:

```
Overall amount / blend: low-medium, roughly 25-40%
Grain: medium
Halation: low
Bloom: low
Gate weave: off or tiny
Vignette: subtle
Saturation/print effect: slightly faded, less polished
```

How to judge it:

- If it looks like synthwave by default, you have overdone saturation, cyan
  shadows, or bloom.
- If it looks too clean next to the FX3 montages, add texture and reduce digital
  sharpness before adding more colour effect.
- If it no longer belongs to the main film, reduce the palette separation before
  reducing grain.

### 1980s Fast Recipe

For the likely best fit on ALEXA, start with moody late-1980s:

1. Normalize LogC / ALEXA Wide Gamut.
2. Build cleaner, deeper blacks than the 1960s/1970s.
3. Add controlled contrast.
4. Cool shadows slightly toward cyan/blue.
5. Keep faces and practicals warm.
6. Lower background saturation if it competes with faces.
7. Add low-medium grain and low halation.
8. Pull back anything that turns into teal-orange styling.

### 1980s Danger Signs

- Looks like neon synthwave by default.
- Too much teal-orange.
- ALEXA footage becomes over-clean compared to the rest of the film.
- Grain/halation style does not match the 1960s/1970s montage logic.
- The image separates from the main film too aggressively.

---

## FILM LOOK CREATOR STRATEGY

Use Film Look Creator after the era contrast and palette nodes.

Recommended position:

```
01 Input
02 Camera Reconciliation
03 Era Contrast
04 Era Palette
05 Skin/Human Hold
06 Shot Fixes
07 Film Look Creator / Grain / Halation
08 Trim
09 Output
```

### Suggested Relative Intensity

| Era   | Overall amount | Grain         | Halation | Bloom                      | Gate Weave     | Saturation Effect               |
| ----- | -------------- | ------------- | -------- | -------------------------- | -------------- | ------------------------------- |
| 1960s | Low            | Low to medium | Very low | Very low                   | None or tiny   | Slightly restrained             |
| 1970s | Low-medium     | Medium        | Low      | Low or off                 | Tiny if useful | Muted/earthy                    |
| 1980s | Low to medium  | Low to medium | Low      | Low to medium if motivated | None           | Cleaner or selectively stronger |

Treat these as starting intensities, not numeric truth. Resolve's Film Look
Creator response depends on the shot's exposure, colour management, and where
the node sits in the tree.

### Film Look Creator Rules

Use Film Look Creator for physical film behavior:

- grain
- halation
- bloom
- diffusion/softness
- vignette
- tiny instability if motivated

Do not use it as the only era grade. The decade should already be readable when
the Film Look Creator node is bypassed. If the era disappears when that node is
off, rebuild the contrast and palette nodes first.

When adjusting:

1. Set grain first, because grain changes perceived sharpness and contrast.
2. Add halation only after the highlights are already controlled.
3. Add bloom last, and only where motivated by bright sources.
4. Add gate weave only after watching the montage in motion.
5. Toggle the node often. The effect should integrate the shot, not announce
   itself.

Do not use gate weave just because it is available. Movement artifacts can
quickly make the montage feel like parody.

---

## FX3-SPECIFIC NOTES FOR 1960s AND 1970s

### Exposure

Cine EI rewards disciplined exposure, but underexposed S-Log3 can become
chroma-noisy. In the grade:

- Do not lift noisy shadows aggressively.
- Use Luma vs Sat to reduce colour noise in low luma.
- Use temporal noise reduction before grain if necessary.
- Add grain after noise reduction so the texture feels intentional.

### Sharpness

FX3 footage may feel too crisp for period material.

Use one or more:

- Midtone Detail down slightly.
- Texture Pop down subtly.
- Small diffusion through Film Look Creator or Resolve FX.
- Avoid global sharpening.

### Highlight Behavior

FX3 highlights can feel less graceful than ALEXA.

Use:

- Custom curve shoulder.
- HDR Specular/Highlight controls.
- Very low halation/bloom around motivated highlights.

Avoid:

- Pulling Gain down until the image goes gray.
- Letting clipped highlights become hard white shapes.

---

## MATCHING FX3 MONTAGES TO ALEXA 1980S

Do not make FX3 look exactly like ALEXA. Make it feel equally intentional.

Match these:

1. Black floor discipline.
2. Skin hue credibility.
3. Highlight softness.
4. Grain scale.
5. Overall density.
6. Emotional contrast.

Let these differ:

1. Era palette.
2. Saturation limits.
3. Texture amount.
4. Colour contamination.
5. Cleanliness of whites.

The audience should feel time has changed, not cameras.

---

## ERA LOOK SUMMARY

| Era   | Contrast         | Saturation           | Blacks         | Highlights              | Texture      | Palette                                                        |
| ----- | ---------------- | -------------------- | -------------- | ----------------------- | ------------ | -------------------------------------------------------------- |
| 1960s | Soft/moderate    | Restrained           | Slightly held  | Soft/creamy             | Fine/subtle  | Warm cream, muted green, soft blue                             |
| 1970s | Denser           | Muted                | Heavier        | Restrained/dirty        | More visible | Ochre, olive, tobacco, faded red                               |
| 1980s | Cleaner/stronger | Selectively stronger | Cleaner/deeper | Brighter but controlled | Controlled   | Depends: clean commercial, moody cyan/warm, or analog domestic |

---

## SHOT-BY-SHOT WORKFLOW

For each montage:

1. Choose 3 anchor shots.
2. Normalize camera input.
3. Build the era contrast.
4. Build the era palette.
5. Protect faces/hands/key objects.
6. Add film texture.
7. Match all shots horizontally.
8. Watch the montage in motion.
9. Pull back anything that reads as an effect.

Anchor shots:

- Most representative shot.
- Darkest shot.
- Brightest or most colourful shot.

Do not grade the most extreme shot first. It will distort the whole sequence.

---

## MONTAGE RHYTHM TRIM

Montage colour is felt through cuts. A shot that works alone may be too strong
in sequence.

On the final pass, watch only the montage:

- Does the colour pulse unintentionally?
- Does one shot look modern?
- Does one shot look like stock footage?
- Do the darkest shots still read?
- Does grain jump between shots?
- Does skin change hue from cut to cut?
- Does the era feel consistent without becoming monotonous?

Use a final trim node for tiny shot-level corrections after the era look is
established.

---

## WHAT NOT TO DO

Do not:

- Put a 1960s/1970s/1980s LUT on top and call it done.
- Add heavy gate weave to every montage.
- Make 1960s yellow, 1970s brown, 1980s neon by default.
- Use grain to hide bad matching.
- Over-clean the FX3 until it looks plastic.
- Over-process the ALEXA until it loses its natural advantage.
- Match scopes instead of matching memory and emotional continuity.

---

## FINAL TEST

Watch the three montages back to back.

Ask:

- Can I feel a different decade without being told?
- Does the FX3 footage feel intentionally period, not technically inferior?
- Does the ALEXA 1980s material still belong to the same film?
- Are the looks different in colour logic, not just in filter intensity?
- Does the montage remain emotionally connected to the main art-film darkness?

If the answer is yes, the grade is doing its job.
