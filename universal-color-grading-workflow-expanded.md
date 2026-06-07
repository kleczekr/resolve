# UNIVERSAL COLOR GRADING WORKFLOW

## DaVinci Resolve Node-Based Scaffolding (Expanded Edition)

A repeatable, professional workflow for narrative, commercial, and documentary
projects.

---

## INTRO

Camera records more than the audience can meaningfully receive. The grade
decides how that record becomes experience: where the eye rests, how skin feels,
how heavy the shadows are, whether a room feels safe or hostile, whether a scene
belongs to daylight, memory, fear, comedy, grief, or desire. Colour is not
decoration after the edit. It is one of the ways a film thinks.

The purpose of a workflow is to keep that thinking deliberate. Technical
correction gives the image a stable world; creative grading gives that world a
point of view. If the order is wrong, taste becomes damage: LUTs hide exposure
problems, saturation hides weak contrast, and style fights continuity. If the
order is right, the grade becomes quiet authority. It preserves the photographed
truth where the film needs truth, and bends it where the film needs feeling.

This document is built around that discipline: normalize before stylizing,
separate responsibilities by node, protect faces and highlights early, and make
decisions that can be repeated across shots. The goal is not to make every image
pretty. The goal is to make every image belong to the same emotional argument.

---

## TOOL LOCATION CHEAT SHEET

| Tool                         | Where to Find It                                                                                                   |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| Color Space Transform        | Effects Library → OpenFX → ResolveFX Color → Color Space Transform (drag onto node)                                |
| Lift / Gamma / Gain / Offset | Color page → Bottom left panel → Color Wheels (icon looks like 4 circles)                                          |
| Temp / Tint                  | Color page → Bottom left panel → Color Wheels → Below the Offset wheel, or in Primaries Bars                       |
| Custom Curves                | Color page → Bottom left panel → Curves (icon looks like an S-curve)                                               |
| Contrast / Pivot             | Color page → Bottom left panel → Primaries Bars → Contrast and Pivot sliders                                       |
| Luma vs Sat                  | Color page → Bottom left panel → Curves → Dropdown menu at top → Lum vs Sat                                        |
| Hue vs Hue                   | Color page → Bottom left panel → Curves → Dropdown menu at top → Hue vs Hue                                        |
| Hue vs Sat                   | Color page → Bottom left panel → Curves → Dropdown menu at top → Hue vs Sat                                        |
| Hue vs Lum                   | Color page → Bottom left panel → Curves → Dropdown menu at top → Hue vs Lum                                        |
| Saturation (global)          | Color page → Bottom left panel → Primaries Bars → Sat slider at bottom                                             |
| Color Boost                  | Color page → Bottom left panel → Primaries Bars → Below Saturation slider                                          |
| Midtone Detail               | Color page → Primaries Bars (next to Contrast/Pivot) OR Log Wheels bottom bar (next to Color Boost) - same control |
| Sharpen                      | Color page → Bottom left panel → Blur (water drop icon) → Sharpen section (H/V sliders)                            |
| Power Window (circle)        | Color page → Bottom center panel → Window tab → Circle icon                                                        |
| Qualifier                    | Color page → Bottom center panel → Qualifier tab → HSL controls                                                    |
| Video Limiter                | Effects Library → OpenFX → ResolveFX Refinement → Video Limiter (drag onto node)                                   |
| Color Warper                 | Color page → Bottom left panel → Color Warper (grid icon)                                                          |
| Magic Mask                   | Color page → Bottom center panel → Magic Mask icon (between Blur and Tracker)                                      |
| Face Refinement              | Effects Library → OpenFX → ResolveFX Refine → Face Refinement (drag onto node)                                     |
| Tracker                      | Color page → Bottom center panel → Tracker tab                                                                     |

---

## CORE PHILOSOPHY

**World first, style last.** Normalize reality before adding taste.

**One node = one responsibility.** If a node does two jobs, it will fail at
both.

**Protect highlights and skin early.** Everything else depends on that.

**Copy structure, adjust values.** Never reinvent node trees per shot.

---

## KEYBOARD SHORTCUTS

| Action                          | Shortcut                                                 |
| ------------------------------- | -------------------------------------------------------- |
| Go to Color page                | Shift + 6                                                |
| Add Serial Node (after current) | Alt/Option + S                                           |
| Add Node Before                 | Alt/Option + B                                           |
| Add Parallel Node               | Alt/Option + P                                           |
| Add Layer Node                  | Alt/Option + L                                           |
| Reset Node (all adjustments)    | Cmd/Ctrl + Home                                          |
| Bypass Node (toggle)            | Cmd/Ctrl + D                                             |
| Bypass All Grades (toggle)      | Shift + D                                                |
| Open Scopes                     | Cmd/Ctrl + Shift + W                                     |
| Grab Still                      | Cmd/Ctrl + Alt + G                                       |
| Play/Stop                       | Spacebar                                                 |
| Next Clip                       | Down Arrow                                               |
| Previous Clip                   | Up Arrow                                                 |
| Ripple Grade to All Clips       | Right-click node → Ripple Node Changes to Selected Clips |
| Image Wipe (compare)            | Cmd/Ctrl + W                                             |
| Load clip as reference          | Middle-click (or Cmd/Ctrl + click) on any thumbnail      |

---

## PRIMARIES VS LOG WHEELS (QUICK EXPLANATION)

You'll see two main wheel modes in Resolve. Here's the difference:

```
┌─────────────────────────────────────────────────────────────────┐
│ TO SWITCH: Click dropdown at top-left of the wheels panel       │
│            Options: "Primaries Wheels" / "Primaries Bars" /     │
│                     "Log" / "HDR" etc.                          │
└─────────────────────────────────────────────────────────────────┘
```

**PRIMARIES (Lift / Gamma / Gain / Offset):**

- The classic, default wheels
- Adjustments OVERLAP - Lift affects Gamma a little, Gamma affects Gain a little
- More natural, organic feel to adjustments
- Good for general correction work

**LOG (Shadow / Midtone / Highlight / Offset):**

- More ISOLATED adjustments - each wheel affects only its range
- You can customize the ranges (Low Range, High Range sliders)
- Designed for log-encoded footage
- Good for targeted, surgical adjustments

**Which to use?**

| Situation                        | Recommended                        |
| -------------------------------- | ---------------------------------- |
| General correction, starting out | Primaries                          |
| Working with LOG footage         | Log wheels often feel more natural |
| Need to affect ONLY shadows      | Log (more isolated)                |
| Want organic, film-like response | Primaries                          |
| Need precise control over ranges | Log (adjustable ranges)            |

**Important:** Both Primaries and Log adjustments work INDEPENDENTLY. If you
adjust Primaries, then switch to Log and adjust, BOTH are applied. Reset one
doesn't reset the other.

**The sliders around the wheels (Contrast, Mid/Detail, Color Boost, etc.) are
shared** - they're the same controls regardless of which wheel mode you're in.

---

## PROJECT-LEVEL COLOR MANAGEMENT (ALTERNATIVE TO N1)

You can handle input color space at the project/timeline level instead of using
a node. This is often cleaner for single-camera projects.

```
┌─────────────────────────────────────────────────────────────────┐
│ LOCATION: File → Project Settings → Color Management            │
│ Or: Click the gear icon (bottom-right of screen)                │
└─────────────────────────────────────────────────────────────────┘
```

**Setup for color-managed workflow:**

1. **Color Science:** Set to "DaVinci YRGB Color Managed"
2. **Input Color Space:** Set your camera's color space (e.g., S-Gamut3.Cine /
   S-Log3)
3. **Timeline Color Space:** Set to DaVinci Wide Gamut / Intermediate
4. **Output Color Space:** Set to your delivery (usually Rec.709 for web)

This applies the transform to everything automatically - you can skip N1
entirely.

**For mixed cameras on the same timeline:**

1. Right-click a clip in the Media Pool
2. Select **Input Color Space** → Choose that camera's specific space
3. Resolve handles the conversion per-clip automatically

**When to use which approach:**

| Approach                        | Use When                                                     |
| ------------------------------- | ------------------------------------------------------------ |
| Project-level color management  | Single camera, consistent footage                            |
| Per-clip Input Color Space      | Mixed cameras, B-roll from different sources                 |
| N1 node (Color Space Transform) | Need per-shot control, want to see before/after, or learning |

---

## HOW TO READ SCOPES (CHECKING LUMA VALUES)

When the document says "skin should be at 40-55%," here's how to actually check
that.

```
┌─────────────────────────────────────────────────────────────────┐
│ OPEN SCOPES: Cmd/Ctrl + Shift + W                               │
│ Or: Workspace menu → Video Scopes                               │
│ CHANGE SCOPE TYPE: Right-click on scope → Select type           │
└─────────────────────────────────────────────────────────────────┘
```

### Method 1: Waveform + Visual Correlation (Everyday Method)

The waveform displays your image laid out left-to-right, with brightness on the
vertical axis:

- **Bottom (0%)** = Pure black
- **Top (100%)** = Pure white
- **Middle (50%)** = Midtones

**How to read it:**

1. Look at where something is positioned horizontally in your image (e.g., face
   on the left side)
2. Look at the same horizontal position on the waveform
3. See where the "blob" of pixels sits vertically - that's your brightness
   percentage

```
Waveform visualization:

100% ┬─────────────────────── Clipping danger zone
     │          ░░
 80% │         ░░░░           Highlights
     │        ░░░░░░
 60% │       ░░░░░░░░
     │      ░░░░░░░░░░        ← Bright skin should be here
 50% │─────░░░░░░░░░░░─────── Midpoint
     │    ░░░░░░░░░░░░░░
 40% │   ░░░░░░░░░░░░░░░      ← Average skin should be here
     │  ░░░░░░░░░░░░░░░░░
 20% │ ░░░░░░░░░░░░░░░░░░     Shadows
     │░░░░░░░░░░░░░░░░░░░░
  0% ┴─────────────────────── Black point
     Left                Right
     (corresponds to left/right of image)
```

### Method 2: Qualifier Picker

1. Go to **Qualifier tab** (bottom center panel)
2. Click the **eyedropper** tool
3. Click on the area you want to measure (e.g., someone's cheek)
4. The selected area highlights on the waveform, showing you exactly where it
   falls

### Method 3: Data Burn-In (Exact Numbers)

For precise values when you need actual numbers:

1. **Workspace** → **Data Burn-In**
2. Enable **"Cursor Position RGB"** or similar option
3. Now when you hover over the image, exact values display on screen
4. Hover over skin → read the Y (luma) value directly

**Note:** This is overkill for normal grading. Most colorists eyeball it using
the waveform. You'll develop an intuition for "that blob is around 45%" after a
while.

### Quick Visual Reference

| What You're Checking | Where It Should Sit on Waveform    |
| -------------------- | ---------------------------------- |
| Darkest shadows      | Just touching the bottom (0-5%)    |
| Dark skin tones      | Lower-middle area (35-45%)         |
| Medium skin tones    | Middle area (45-55%)               |
| Light skin tones     | Upper-middle area (55-65%)         |
| White objects        | Upper area (80-90%)                |
| Brightest highlights | Near top but not clipping (90-98%) |

---

## SHOT COMPARISON AND MATCHING

When matching shots, you need to compare your current clip against a reference.
Here's how.

```
┌─────────────────────────────────────────────────────────────────┐
│ GRAB STILL: Right-click thumbnail → Grab Still                  │
│             Or: Cmd/Ctrl + Alt + G                              │
│                                                                 │
│ COMPARE: Double-click a still in Gallery to load it             │
│          Then press Cmd/Ctrl + W for wipe view                  │
└─────────────────────────────────────────────────────────────────┘
```

### Setting Up a Reference Still

1. Grade your **hero shot** (the shot you want everything else to match)
2. **Grab a still:** Right-click the thumbnail → Grab Still (or Cmd/Ctrl + Alt +
   G)
3. The still appears in the **Gallery** (top-left panel in Color page)

### Comparing Against Your Reference

1. Move to the next shot you want to grade
2. In the Gallery, **double-click your reference still** - this loads it for
   comparison
3. Press **Cmd/Ctrl + W** to enable the wipe/split view
4. Drag the wipe line across to compare the two images side by side

### Wipe Modes

Click the dropdown next to the wipe button in the viewer to change modes:

| Mode         | What It Does                                | Use For                                   |
| ------------ | ------------------------------------------- | ----------------------------------------- |
| Wipe         | Diagonal/straight line split you can drag   | General comparison, seeing both at once   |
| Split Screen | Side by side, 50/50                         | Direct comparison without overlap         |
| Blend        | Opacity mix between the two                 | Checking if exposures match               |
| Difference   | Shows pixel differences (black = identical) | Technical matching, finding discrepancies |

### Comparing Against Another Clip (Not a Still)

If you want to compare against a clip you haven't grabbed as a still:

1. **Hover over any clip** in the thumbnail timeline at the bottom of the Color
   page
2. **Middle-click** (scroll wheel click) or **Cmd/Ctrl + click** to load it as
   the reference
3. Press **Cmd/Ctrl + W** to wipe/compare

### Matching Workflow Tips

- **Match skin tones first** - If faces match, most other things will fall into
  place
- **Look at the waveform while comparing** - Both images should have similar
  waveform shapes
- **Match brightness before color** - Get exposure right, then worry about color
  balance
- **Use the same area of frame** - Compare face-to-face, not face-to-sky

---

## HORIZONTAL WORKFLOW (HOW TO WORK THROUGH SHOTS)

**Critical concept:** Work HORIZONTALLY across shots, not VERTICALLY through
nodes.

### What This Means

**CORRECT - Horizontal workflow:**

```
N2 on shot 1 → N2 on shot 2 → N2 on shot 3 → ... → N2 DONE on all shots
N3 on shot 1 → N3 on shot 2 → N3 on shot 3 → ... → N3 DONE on all shots
N4 on shot 1 → N4 on shot 2 → N4 on shot 3 → ... → N4 DONE on all shots
...continue through N10
```

**WRONG - Vertical workflow:**

```
Shot 1: N2 → N3 → N4 → N5 → N6 → N7 → N8 → N9 → N10 (fully finished)
Shot 2: N2 → N3 → N4 → N5 → N6 → N7 → N8 → N9 → N10 (start from scratch)
Shot 3: ... ← This is a nightmare, don't do this
```

### Why Horizontal Works Better

| Reason                    | Explanation                                                                                         |
| ------------------------- | --------------------------------------------------------------------------------------------------- |
| **Easier matching**       | When you're doing N2 on shot 5, shots 1-4 are at the same stage - you're comparing apples to apples |
| **Consistent eye**        | You're making the same type of decision repeatedly, so you stay calibrated                          |
| **Catch mistakes early**  | If N2 is wrong on shot 3, you'll notice immediately when you do shot 4                              |
| **Look stays consistent** | The creative look (N9) gets applied to already-matched footage                                      |

### The Full Process

**Phase 0: Setup**

1. Build your 10-node structure on the hero shot
2. Save as PowerGrade (see below)
3. Apply the PowerGrade to all clips in the scene

**Phase 1: Physics (N1-N4) - All shots** 4. Complete N2 (balance) on ALL shots -
use wipe to compare each to your hero 5. Complete N3 (contrast) on ALL shots 6.
Complete N4 (highlight safety) on ALL shots

**Phase 2: Perception (N5-N7) - All shots** 7. Complete N5 (saturation) on ALL
shots 8. Complete N6 (hue sculpt) on ALL shots 9. Complete N7 (texture) on ALL
shots

**Phase 3: Taste (N8-N10) - All shots** 10. Complete N8 (vignette) on shots that
need it 11. Complete N9 (look) on ALL shots 12. Verify N10 (output) on ALL shots

### Working in Scene Batches

For longer projects, work scene by scene:

1. Scene 1: N2 all shots → N3 all shots → ... → N10 all shots
2. Scene 2: N2 all shots → N3 all shots → ... → N10 all shots
3. And so on...

This keeps you focused and ensures each scene is internally consistent before
moving on.

---

## POWERGRADE: SAVING AND REUSING NODE STRUCTURES

A PowerGrade is a saved node structure that works across ALL projects (unlike
regular stills which are project-specific).

### PowerGrade vs Regular Still

| Still                              | PowerGrade                                 |
| ---------------------------------- | ------------------------------------------ |
| Saved in current project's Gallery | Saved to disk as a file                    |
| Only accessible in this project    | Accessible from ANY project                |
| Lost if project corrupts           | Survives independently                     |
| Good for shot references           | Good for templates and reusable structures |

### Creating a PowerGrade

1. Build your node tree (the 10-node structure, with or without values)
2. **Grab a still:** Right-click thumbnail → Grab Still (or Cmd/Ctrl + Alt + G)
3. Go to **Gallery** (top-left panel in Color page)
4. Find your still → **Right-click** → **Export** → **Export as PowerGrade**
5. Save the .dpx file somewhere you'll remember (create a "LUTs and Grades"
   folder)

### Using a PowerGrade

1. Open any project
2. In the Gallery → **Right-click** → **Import** → **Import PowerGrade**
3. Navigate to your saved .dpx file
4. Select the clips you want to apply it to
5. **Right-click the PowerGrade** → **Apply Grade**

### PowerGrade Tips

- **Create a "template" PowerGrade** with all 10 nodes labeled but minimal
  adjustments
- **Create camera-specific PowerGrades** with N1 already set for each camera you
  use
- **Create look PowerGrades** with your favorite N9 looks for quick application
- **Keep PowerGrades organized** in folders by project type or camera

---

## MASTER NODE STRUCTURE

**How to create the 10-node tree:**

1. Select your clip in the Color page
2. Press Alt/Option + S to add a serial node (repeat 9 times for 10 total)
3. Right-click each node → "Node Label" → Name them N1, N2, etc.
4. Or: Right-click in the node graph → Add Node → Serial

```
N1  Input Transform / Camera Normalization
N2  Primary Balance (Exposure + White Balance)
N3  Contrast Shaping
N4  Highlight Safety
N5  Saturation & Color Density
N6  Hue Sculpt (Selective Color Control)
N7  Texture Control
N8  (Optional) Vignette / Eye Guidance
N9  Creative Look
N10 Output Trim / Legal Safe
```

**Mental model:**

- N1–N4 = Physics (making the image technically correct)
- N5–N7 = Perception (making it feel right to human eyes)
- N8–N9 = Taste (artistic choices)
- N10 = Insurance (delivery safety)

---

# NODE-BY-NODE DETAILED BREAKDOWN

---

## N1 — INPUT TRANSFORM / NORMALIZATION

**Purpose:** Bring footage into a predictable working color space so every
adjustment you make afterward behaves consistently.

```
┌─────────────────────────────────────────────────────────────────┐
│ TOOL: Color Space Transform                                     │
│ LOCATION: Effects Library → OpenFX → ResolveFX Color →          │
│           Color Space Transform                                 │
│ HOW: Drag and drop onto the node                                │
└─────────────────────────────────────────────────────────────────┘
```

### What This Node Actually Does

When you shoot in LOG (like S-Log3, C-Log, V-Log), your camera captures a "flat"
image with lots of latitude for grading. But LOG footage looks washed out and
gray - that's intentional. The camera is storing maximum dynamic range, not a
pretty picture.

This node translates that LOG data into a standardized working space (DaVinci
Wide Gamut / Intermediate) so that when you push the color wheels, the image
responds predictably.

**Think of it like this:** Different cameras speak different languages. This
node is the translator that converts everything to one common language before
you start working.

### When You Need This Node

**USE IT when:**

- You shot LOG footage (S-Log, C-Log, V-Log, BRAW, ProRes RAW, etc.)
- You're mixing different cameras on the same timeline
- Phone footage is behaving weirdly (too contrasty, colors shifting
  unexpectedly)
- You're working in a color-managed workflow (ACES, DaVinci Color Management)

**SKIP IT when:**

- You shot in Rec.709 (standard video mode)
- Your footage already looks "normal" and responds well to grading
- You're doing a quick grade on social media content shot on Auto

### Step-by-Step Setup

1. **Select the N1 node** (click on it in the node graph)
2. **Open Effects Library** (top left of screen, or Workspace → Effects Library)
3. **Navigate:** OpenFX → ResolveFX Color → Color Space Transform
4. **Drag the effect onto your N1 node**
5. **In the Inspector panel (top right), set:**
   - Input Color Space: Your camera's color space
   - Input Gamma: Your camera's LOG curve
   - Output Color Space: DaVinci Wide Gamut
   - Output Gamma: DaVinci Intermediate

### Common Camera Settings

| Camera                  | Input Color Space | Input Gamma           |
| ----------------------- | ----------------- | --------------------- |
| Sony FX3/FX6/A7S III    | S-Gamut3.Cine     | S-Log3                |
| Sony older (A7S II)     | S-Gamut3          | S-Log2                |
| Blackmagic (Gen 5)      | Blackmagic Design | Blackmagic Film Gen 5 |
| Blackmagic (older)      | Blackmagic Design | Blackmagic Film Gen 4 |
| Canon C70/C300 III      | Cinema Gamut      | Canon Log 3           |
| Canon older             | Cinema Gamut      | Canon Log 2           |
| Panasonic S1H/GH6       | V-Gamut           | V-Log                 |
| ARRI (modern)           | ARRI Wide Gamut 4 | LogC4                 |
| ARRI (older)            | ARRI Wide Gamut 3 | LogC3                 |
| RED (IPP2 - modern)     | REDWideGamutRGB   | Log3G10               |
| RED (pre-IPP2 / legacy) | DRAGONcolor2      | REDlogFilm            |
| iPhone 15 Pro (Log)     | Apple Log         | Apple Log             |
| DJI (D-Log M)           | DJI D-Gamut       | D-Log                 |

### How to Know If It's Working

**Before transform:** Image looks flat, gray, washed out (if LOG) **After
transform:** Image has more contrast and color, but may look slightly weird or
oversaturated

**Don't worry if it looks a bit off** - you're just converting the data. The
next nodes will make it look good.

### Common Mistakes

❌ **Adding contrast or saturation here** - This node is ONLY for color space
conversion ❌ **Using the wrong input settings** - Check your camera's recording
settings ❌ **Applying to Rec.709 footage** - This will make normal footage look
terrible ❌ **Forgetting to set BOTH input AND output** - Leaving either on
"Bypass" defeats the purpose

### Extra Nodes to Consider (N1 variants)

**N1b - Noise Reduction (before everything else):** If your footage is noisy,
add a Temporal Noise Reduction node BEFORE or immediately after the color space
transform. Noise reduction works best on the original data before any contrast
adjustments amplify the noise.

```
LOCATION: Effects Library → OpenFX → ResolveFX Refinement →
          Noise Reduction (drag onto a new node before N2)
SETTINGS: Start with Temporal NR, Threshold 10-25, keep Spatial low
```

**N1c - Lens Correction:** If you have significant lens distortion or chromatic
aberration, fix it early.

```
LOCATION: Color page → OpenFX → ResolveFX Refinement →
          Chromatic Aberration or Dead Pixel Fixer
```

### End State Checklist

✓ Footage is in a consistent color space ✓ No creative changes have been made ✓
Image responds predictably when you test the color wheels ✓ You haven't touched
contrast or saturation

---

## N2 — PRIMARY BALANCE

**Purpose:** Establish physical reality - make the image look like what your
eyes would have seen if you were standing there. This is the most important node
in your entire grade.

```
┌─────────────────────────────────────────────────────────────────┐
│ TOOLS: Lift / Gamma / Gain / Offset + Temp / Tint               │
│ LOCATION: Color page → Bottom left panel → Color Wheels         │
│           (icon looks like 4 circles)                           │
│ Temp/Tint: Below the Offset wheel, or switch to Primaries Bars  │
└─────────────────────────────────────────────────────────────────┘
```

### What This Node Actually Does

This is where you make the image look "correct" - proper exposure, accurate
colors, realistic black levels. You're not making it pretty yet, you're making
it true.

**The goal is a "boring" image** - one that looks completely natural, like
you're looking through a clean window at the real world.

### Understanding the Controls

**OFFSET (the big wheel in the center or the fourth wheel):**

- Moves the ENTIRE image brighter or darker
- Like adjusting the exposure dial on your camera
- Use this FIRST to get overall brightness roughly right

**LIFT (shadows wheel - usually bottom left):**

- Controls the darkest parts of your image
- Dragging UP = brighter shadows (more milky/washed out)
- Dragging DOWN = darker shadows (more crushed/contrasty)
- Pushing toward a color tints your shadows that color

**GAMMA (midtones wheel - usually top):**

- Controls the middle brightness values
- This is where skin tones live
- Dragging UP = brighter midtones
- Dragging DOWN = darker midtones

**GAIN (highlights wheel - usually bottom right):**

- Controls the brightest parts of your image
- Dragging UP = brighter highlights (can clip/blow out)
- Dragging DOWN = darker highlights

**TEMP (color temperature):**

- LEFT (negative) = Cooler/bluer
- RIGHT (positive) = Warmer/more orange
- Corrects for wrong white balance in camera

**TINT:**

- LEFT (negative) = More green
- RIGHT (positive) = More magenta
- Corrects for fluorescent lights or mixed lighting

### Step-by-Step Workflow

**Step 1: Fix White Balance First**

Look at something in your image that should be neutral - a white shirt, gray
concrete, a piece of paper.

If it looks:

- **Too orange/yellow:** Drag Temp LEFT (cooler)
- **Too blue:** Drag Temp RIGHT (warmer)
- **Too green:** Drag Tint RIGHT (add magenta)
- **Too magenta/pink:** Drag Tint LEFT (add green)

**Eyedropper trick:** In the Color Wheels panel, there's an eyedropper icon.
Click it, then click on something that should be neutral gray in your image.
Resolve will auto-correct the white balance. This is a starting point - adjust
by eye afterward.

**Step 2: Set Overall Exposure with Offset**

Look at your Waveform scope (Cmd/Ctrl + Shift + W to open scopes).

The waveform shows brightness from bottom (0% = pure black) to top (100% = pure
white).

- If the whole waveform is too low, drag Offset UP
- If the whole waveform is too high, drag Offset DOWN

**Step 3: Set Your Black Point with Lift**

Look at the bottom of your waveform. Your darkest shadows should:

- Touch or nearly touch 0% (true black)
- NOT be crushed flat against 0% (you'd lose shadow detail)
- NOT be floating above 0% (image will look washed out)

**What "crushing" looks like:** When you drag Lift too far down, watch the
bottom of your waveform. If it flattens out and a bunch of different dark values
all become the same pure black, you've "crushed" your shadows. You've lost
detail. Back off.

**Visual test:** Look at the darkest areas of your image - a person's black
hair, shadows under furniture, the inside of a doorway. Can you still see detail
and texture? If it's all just a blob of black, you've crushed.

**What "milky" looks like:** If your Lift is too high, your blacks won't be
black - they'll be dark gray. The image will look faded, low-contrast, like a
print that's been left in the sun. The bottom of your waveform will float above
0%.

**Step 4: Set Faces with Gamma**

Skin tones should fall in specific brightness ranges:

| Lighting Scenario | Skin Luma Target |
| ----------------- | ---------------- |
| Bright daylight   | 50-60%           |
| Normal indoor     | 40-55%           |
| Moody/night       | 30-45%           |
| Dramatic shadows  | 20-35%           |

**How to check:** Hover over face in the viewer while looking at the waveform.
Or use the Qualifier to isolate skin and see where it sits on the scope.

If faces are too dark, push Gamma UP. If too bright, pull Gamma DOWN.

**Step 5: Control Highlights with Gain**

Look at the top of your waveform. Your brightest areas should:

- Stay below 100% (not clipping)
- Typically peak around 85-95%
- Only hit 100% for actual light sources (sun, lamps)

**What "clipping" looks like:** When highlights hit 100% and flatten against the
top of the waveform, you've lost detail. A white shirt becomes a blob with no
fabric texture. The sky becomes a featureless white void. This is bad and often
unfixable.

If your highlights are clipping, drag Gain DOWN until they drop below 100%.

### Target Values Reference

| Element             | Waveform Target | What to Look For                     |
| ------------------- | --------------- | ------------------------------------ |
| Pure black in scene | 0-2%            | Deepest shadows just touching bottom |
| Dark shadows        | 5-15%           | Detail still visible                 |
| Dark skin tones     | 35-45%          | Rich, not muddy                      |
| Medium skin tones   | 45-55%          | Natural, not blown                   |
| Light skin tones    | 55-65%          | Not overexposed                      |
| White objects       | 80-90%          | Texture visible                      |
| Specular highlights | 90-100%         | Only small points                    |
| Light sources       | 100%            | OK to clip                           |

### Reading the Vectorscope for White Balance

The vectorscope shows color, not brightness. For a properly white-balanced
image:

- The "blob" of your image data should be roughly centered
- If it's shifted toward orange/yellow, image is too warm
- If it's shifted toward blue/cyan, image is too cool
- If it's shifted toward green, you have green tint
- If it's shifted toward magenta, you have magenta tint

### Common Mistakes

❌ **Making it "look good" instead of "look correct"** - Save pretty for later
nodes ❌ **Crushing shadows for "cinematic" look** - You'll regret losing that
detail ❌ **Over-correcting white balance** - A slight warm or cool cast can be
intentional ❌ **Ignoring the scopes** - Your eyes adapt and lie; scopes tell
the truth ❌ **Adjusting one shot in isolation** - Always compare to surrounding
shots

### How to Know If You're Done

Toggle the node bypass (Cmd/Ctrl + D). The image should go from "correctly
exposed, proper colors" to "whatever the camera recorded."

The "after" should look:

- Properly exposed (not too dark, not too bright)
- Correct colors (whites are white, not orange or blue)
- Full tonal range (blacks are black, whites are white, with detail in between)
- Boring (no style, no mood, just reality)

### Extra Nodes to Consider (N2 variants)

**N2b - Secondary Exposure Fix (for specific areas):** If part of your image
needs different exposure than the rest (like a bright window or dark corner),
create a parallel node with a Power Window to fix just that area.

```
HOW: Alt/Option + P for parallel node, add Window, adjust exposure
USE: Bright windows, dark backgrounds, blown-out skies
```

**N2c - Skin Tone Isolation:** If skin tones need separate treatment from the
rest of the image:

```
HOW: Add serial node after N2, use Qualifier to select skin
LOCATION: Bottom center panel → Qualifier tab → HSL
TIP: Select skin, add slight warmth or adjust exposure independently
```

### End State Checklist

✓ White/neutral objects look neutral (not orange, blue, green, or magenta) ✓
Darkest shadows are near 0% on waveform with detail retained ✓ Brightest
highlights are below 100% on waveform (except light sources) ✓ Faces are at
appropriate brightness for the scene ✓ Image looks "correct" even if it's boring
✓ No style or mood has been added yet

---

## N3 — CONTRAST SHAPING

**Purpose:** Add depth and dimension to the flat, "correct" image from N2. Make
it feel cinematic without making it look "graded."

```
┌─────────────────────────────────────────────────────────────────┐
│ TOOL: Custom Curves (preferred)                                 │
│ LOCATION: Color page → Bottom left panel → Curves               │
│           (icon looks like an S-curve)                          │
│ Make sure dropdown says "Custom" (not Hue vs Hue etc.)          │
│                                                                 │
│ ALTERNATIVE: Contrast + Pivot sliders                           │
│ LOCATION: Color page → Primaries Bars → Contrast / Pivot        │
└─────────────────────────────────────────────────────────────────┘
```

### What This Node Actually Does

After N2, your image is technically correct but often feels flat - like a news
broadcast rather than a movie. Contrast shaping adds the subtle tonal
compression and expansion that makes images feel three-dimensional and
cinematic.

**The key insight:** Film doesn't capture light linearly. It compresses
highlights and lifts shadows slightly. Digital cameras capture light more
literally, which can look harsh. This node adds back that organic feeling.

### Understanding Curves

The curve graph shows input values (original brightness) on the X-axis and
output values (resulting brightness) on the Y-axis.

- **Straight diagonal line:** No change (output = input)
- **Line above the diagonal:** That brightness range gets brighter
- **Line below the diagonal:** That brightness range gets darker
- **Steeper curve:** More contrast in that range
- **Flatter curve:** Less contrast in that range

**The "S-Curve":** An S-shaped curve increases contrast in midtones while
compressing shadows and highlights. This is the classic cinematic contrast
shape.

```
           ___-------- Highlights: rolled off (compressed)
          /
         /
        /    Midtones: steep (more contrast)
       |
      |
     /
____/           Shadows: lifted toe (compressed)
```

### Step-by-Step Workflow

**Step 1: Open Custom Curves**

1. Click on your N3 node
2. Go to Curves panel (bottom left)
3. Make sure dropdown at top says "Custom" (not Hue vs Hue, etc.)
4. You should see a diagonal line in a square graph

**Step 2: Lift the Toe (Shadow Compression)**

Click on the curve near the bottom-left (around 10-15% on the X-axis). Drag this
point UP slightly.

**What this does:** The very darkest parts of your image won't go to pure black
anymore. They'll land at a slightly lifted dark gray. This mimics how film
handles shadows and prevents harsh, digital-looking blacks.

**How much:** Start subtle. Lift the shadows so they land around 3-8% instead of
0%. You can always add more.

**Visual check:** Look at the darkest areas of your image. Do they feel rich and
detailed, or flat and digital? They should feel "dense" - dark but with depth,
not like a black hole.

**Step 3: Roll Off the Highlights (Highlight Compression)**

Click on the curve near the top-right (around 85-90% on the X-axis). Drag this
point DOWN slightly.

**What this does:** The brightest parts of your image won't go to pure white
anymore. They'll compress into a softer, creamier highlight. This prevents harsh
clipping and mimics how film handles bright areas.

**How much:** Roll off gently so highlights peak around 95-98% instead of 100%.
You want them soft, not dull.

**Visual check:** Look at bright areas - windows, sky, white clothing. Do they
feel soft and natural, or harsh and blown? They should feel luminous but not
glaring.

**Step 4: Shape the Midtones (Optional S-Curve)**

If you want more punch, add a slight S-shape through the midtones:

1. Add a point around 30-35% (lower midtones) - drag slightly DOWN
2. Add a point around 65-70% (upper midtones) - drag slightly UP

This increases contrast in the midtones where faces live, making the image pop.

**How much:** Very subtle. If you can obviously see the effect, you've gone too
far.

### Understanding "Contrast" and "Pivot" Sliders

If curves feel too complex, you can use the simpler Contrast and Pivot sliders:

**Contrast slider:**

- Increases = more difference between darks and lights
- Decreases = flatter, less difference

**Pivot slider:**

- Sets the "center point" around which contrast is applied
- Lower pivot = contrast affects shadows more
- Higher pivot = contrast affects highlights more

For a cinematic look, try: Contrast +5 to +15, Pivot around 0.35-0.45

### What Is "Crushing" and How to Avoid It

**Crushing = losing shadow detail by pushing darks to pure black**

When you add too much contrast or push your curve too far, the bottom of the
waveform flattens. Multiple different dark gray values all become the same pure
black. You've lost information permanently.

**How to spot crushing:**

- Waveform: Bottom flattens into a thick line at 0%
- Image: Shadow areas become featureless black blobs
- Details like hair texture, fabric in shadows, background details disappear

**How to avoid it:**

- Always lift the toe slightly (don't let curve touch bottom-left corner)
- Watch the waveform as you adjust
- Look at actual shadow detail in the image, not just overall contrast

**Fix if you've crushed:** Back off your adjustment. Pull that lower curve point
back up until shadow detail returns.

### What Is "Banding" and How to Avoid It

**Banding = visible steps in smooth gradients instead of smooth transitions**

Common in skies, out-of-focus backgrounds, and any smooth gradient area. Instead
of a smooth blue-to-lighter-blue sky, you see distinct bands like steps.

**How to spot banding:**

- Look at skies and smooth gradients
- Visible "steps" or "rings" in areas that should be smooth
- Most obvious in 8-bit footage or heavily graded images

**How to avoid it:**

- Use gentle curves, not extreme adjustments
- Don't add excessive contrast to already-compressed footage
- Work in a higher bit-depth project if possible (10-bit or higher)

**Fix if you have banding:** Reduce the intensity of your contrast curve. Add a
touch of grain/noise in N7 (grain hides banding).

### Common Mistakes

❌ **Too much S-curve** - Image looks crunchy and over-processed ❌ **Crushing
shadows for "mood"** - You're just losing information ❌ **Clipping
highlights** - Watch the top of your waveform ❌ **Making faces too
contrasty** - Faces should be lifted gently, not punchy ❌ **Adjusting contrast
without looking at scopes** - Your eyes lie after a few minutes

### How to Know If You're Done

Toggle the node bypass (Cmd/Ctrl + D).

The difference should be:

- Subtle but noticeable
- Image has more "depth" and dimension
- Shadows feel rich, not crushed
- Highlights feel soft, not blown
- It should NOT look "filtered" or "graded"

**The "90% rule":** If someone who doesn't do color grading notices the change,
you've probably gone too far.

### Extra Nodes to Consider (N3 variants)

**N3b - RGB Contrast Separation:** Instead of one contrast curve for all
channels, you can use separate curves for Red, Green, and Blue. This creates
subtle color separation between shadows and highlights.

```
HOW: In Curves panel, click "Y" and change to "R", "G", or "B"
TIP: Slightly different curves per channel = organic color separation
```

**N3c - Zone-Based Contrast:** For more control, use separate nodes for shadow
contrast and highlight contrast.

```
HOW: Create two serial nodes
NODE 1: Only affect shadows (use Luminance Qualifier or low-end curve)
NODE 2: Only affect highlights (use high-end curve)
```

**N3d - Local Contrast (use sparingly):** If you want more "pop" without
affecting overall contrast:

```
TOOL: Midtone Detail (positive values)
LOCATION: Blur panel → Mid Detail slider
WARNING: Easy to overdo. Keep it subtle (+5 to +15)
```

### End State Checklist

✓ Image has depth and dimension (not flat) ✓ Shadows are rich and detailed (not
crushed to black) ✓ Highlights are soft and natural (not clipped to white) ✓ No
visible banding in gradients ✓ Faces look natural (not over-contrasty) ✓ Change
is subtle - wouldn't look "filtered" to a normal viewer

---

## N4 — HIGHLIGHT SAFETY

**Purpose:** Protect highlights from getting weird colors when you add
saturation later. This is preventive medicine for common digital artifacts.

```
┌─────────────────────────────────────────────────────────────────┐
│ TOOL: Luma vs Sat curve                                         │
│ LOCATION: Color page → Bottom left panel → Curves →             │
│           Click dropdown at top → Select "Lum vs Sat"           │
│                                                                 │
│ The X-axis is luma (brightness), Y-axis is saturation           │
│ Pull down the RIGHT side (highlights) to desaturate brights     │
└─────────────────────────────────────────────────────────────────┘
```

### What This Node Actually Does

When you add saturation to an image (in N5), the brightest areas can get weird.
Skies turn electric blue. White shirts get yellow or magenta casts. Skin
highlights go orange.

This node preemptively reduces saturation in the highlights so that when you
boost overall saturation later, the bright areas stay clean and neutral.

**Think of it like sunscreen:** You apply it before you go outside, not after
you're burned.

### The Problem This Solves

**Why highlights get weird with saturation:**

Digital sensors capture color differently than film. When a pixel gets very
bright, it can lose accurate color information. The "color" you see in those
highlights is often just noise or sensor artifacts, not real color data.

When you boost saturation, you're amplifying everything - including those
artifacts. A slightly yellow-ish highlight becomes VERY yellow. A slightly blue
sky goes nuclear.

**Common highlight problems:**

- **Sky turning electric/neon blue** - Most common, looks terrible
- **White clothing getting yellow or magenta tint**
- **Skin highlights going orange** - Makes people look fake
- **Specular reflections picking up weird colors**
- **Light sources (windows, practicals) getting color casts**

### Understanding the Luma vs Sat Curve

This curve lets you control saturation based on brightness:

- **X-axis (left to right):** Brightness from 0% (black) to 100% (white)
- **Y-axis (up and down):** Saturation adjustment
- **Center line:** No change
- **Above center:** More saturation
- **Below center:** Less saturation

**Default state:** Flat horizontal line in the middle = no change to any
brightness range

### Step-by-Step Workflow

**Step 1: Open Luma vs Sat**

1. Click on your N4 node
2. Go to Curves panel (bottom left)
3. Click the dropdown at the top (probably says "Custom")
4. Select "Lum vs Sat"

**Step 2: Anchor Your Midtones**

Before touching anything, add anchor points to protect the areas you DON'T want
to change:

1. Click on the curve around 40-50% (midtones) to add a point
2. Click around 60-70% (upper midtones) to add another point
3. Don't move these points - they're anchors

**Why anchor:** Without anchors, when you pull down the highlights, the change
affects more of the image than you want. Anchors keep the midtones (where skin
lives) untouched.

**Step 3: Pull Down the Highlights**

1. Click on the curve at the far right (around 90-95%)
2. Drag this point DOWN
3. The curve should now dip down on the right side only

**How much:** Start subtle. Pull down until the very brightest areas lose maybe
20-30% of their saturation. You can always do more if needed.

**Step 4: Optional - Protect the Extreme Shadows**

Sometimes very dark shadows also get noisy/weird saturation. You can pull down
the left side slightly too:

1. Click on the curve at the far left (around 5-10%)
2. Drag this point DOWN slightly
3. Add an anchor around 20% to protect lower midtones

### How to Check If It's Working

**The Before/After Test:**

1. Go to N5 and temporarily boost saturation to an extreme amount (like +30)
2. Look at your highlights - are they going crazy?
3. Now toggle N4 on and off (Cmd/Ctrl + D)
4. With N4 ON: Highlights should stay relatively neutral
5. With N4 OFF: Highlights will get colorful/weird
6. Reset N5 saturation when done testing

**What to Look At:**

| Element         | Without N4 (bad)       | With N4 (good)           |
| --------------- | ---------------------- | ------------------------ |
| Sky             | Electric/neon blue     | Clean, natural blue      |
| White clothing  | Yellow or magenta tint | Clean white with texture |
| Skin highlights | Orange, unnatural      | Soft, natural glow       |
| Windows/lights  | Color cast             | Clean, white-ish         |

### Common Mistakes

❌ **Pulling down too much** - Highlights become gray and lifeless ❌
**Forgetting to anchor midtones** - Affects skin and other important areas ❌
**Skipping this node** - "I'll fix it if there's a problem" - by then it's
harder ❌ **Not testing with saturation boost** - You won't see the effect until
you saturate

### How Extreme Is Too Extreme?

If your highlights look:

- **Gray and dead:** You've pulled down too much. Back off.
- **Clean but slightly less colorful than the rest of the image:** Perfect.
- **Still getting neon/weird colors when you add saturation:** Pull down more.

The goal is "invisible protection" - you shouldn't be able to see what this node
does until you stress-test it with saturation.

### Extra Nodes to Consider (N4 variants)

**N4b - Highlight Color Correction:** If highlights have a specific color cast
(like yellow), you might want to correct the color, not just desaturate.

```
HOW: Add serial node, use Qualifier or Curves to select highlights
THEN: Adjust Temp/Tint or color wheels to neutralize the cast
```

**N4c - Highlight Recovery (for clipped footage):** If your highlights are
already blown out from shooting:

```
TOOL: Highlight (HDR) recovery in Camera RAW panel (for RAW footage)
OR: Use the Gain or Highlight slider to pull down brightness
LIMITATION: Can't recover what wasn't captured
```

**N4d - Sky Replacement/Fix Node:** If sky is particularly problematic, create a
dedicated sky node:

```
HOW: Add node, use Qualifier to select sky (HSL: select blues/cyans)
THEN: Adjust saturation, hue, and luminance just for sky
TIP: Feather the selection to avoid harsh edges
```

### End State Checklist

✓ Midtones are anchored and unchanged ✓ Highlights have reduced saturation (you
may not be able to see it yet) ✓ When you boost saturation in N5, highlights
stay clean ✓ Sky doesn't go nuclear blue when saturated ✓ White objects stay
white, not tinted ✓ The node appears to "do nothing" under normal viewing

---

## N5 — SATURATION & COLOR DENSITY

**Purpose:** Bring life and energy back into the image after all the technical
corrections. Make the world feel vibrant and alive without looking "processed."

```
┌─────────────────────────────────────────────────────────────────┐
│ TOOLS: Saturation slider + Color Boost slider                   │
│ LOCATION: Color page → Bottom left panel → Primaries Bars       │
│           (click the sliders icon, not the wheels)              │
│                                                                 │
│ Saturation: Main "Sat" slider at the bottom of the panel        │
│ Color Boost: Right below/beside Saturation slider               │
└─────────────────────────────────────────────────────────────────┘
```

### What This Node Actually Does

After color space conversion and balancing, images often feel a bit flat or
desaturated. This node adds back color richness - the feeling of lush greens,
deep blues, warm skin tones.

**The goal:** The world should feel alive and present, like you could reach into
the screen and touch things. But it should NOT feel like an Instagram filter.

### Understanding the Two Tools

**SATURATION (main slider):**

- Increases/decreases ALL colors equally
- +10 = everything gets 10% more saturated
- Affects already-saturated colors as much as desaturated ones
- Can push skin and vibrant colors too far quickly

**COLOR BOOST (secondary slider):**

- Smart saturation that protects already-saturated areas
- Boosts undersaturated colors more than saturated ones
- Gentler on skin tones
- Good for bringing up weak colors without over-cooking strong ones

**When to use which:**

| Situation                   | Use               | Why                                      |
| --------------------------- | ----------------- | ---------------------------------------- |
| Image is evenly desaturated | Saturation        | Everything needs equal boost             |
| Some colors weak, some OK   | Color Boost       | Brings up weak without over-doing strong |
| Skin tones present          | Color Boost first | More forgiving on skin                   |
| Want vibrant, punchy look   | Saturation        | More aggressive effect                   |
| Subtle, natural look        | Color Boost       | More controlled                          |

### Step-by-Step Workflow

**Step 1: Open Primaries Bars**

1. Click on your N5 node
2. Go to the bottom-left panel
3. Click the "bars" icon (looks like horizontal sliders) instead of the wheels

**Step 2: Start with Color Boost**

1. Find the Color Boost slider (usually labeled "Color Boost" or near
   Saturation)
2. Start at 0
3. Slowly push up to +10 or +15
4. Watch the image, especially skin and already-colorful areas

**Step 3: Add Saturation if Needed**

1. If Color Boost isn't enough, add some Saturation
2. Start at 50 (default)
3. Push up to 52-58
4. Watch for skin going orange and shadows getting noisy

**Step 4: Check Your Danger Zones**

Look at these areas specifically:

| Danger Zone | What Can Go Wrong           | What to Watch For                  |
| ----------- | --------------------------- | ---------------------------------- |
| Skin tones  | Goes orange/red, looks fake | Should look human, not tanned      |
| Shadows     | Chroma noise appears        | Colored speckles in dark areas     |
| Sky         | Goes electric blue          | Should be natural blue             |
| Foliage     | Goes neon green             | Should be organic, not radioactive |
| Reds        | Blows out first             | Red clothing, lips, etc.           |

### Understanding Chroma Noise

**What is chroma noise?** Random colored speckles (usually
red/blue/magenta/green) that appear in dark areas of the image. Looks like
confetti in your shadows.

**Why saturation causes it:** Shadow areas have weak color information from the
sensor. When you boost saturation, you amplify this weak signal, making the
noise visible.

**How to spot it:**

1. Look at the darkest areas of your image at 100% zoom
2. Look for random colored pixels that shouldn't be there
3. Most visible in: dark clothing, night scenes, underexposed areas

**How to avoid it:**

- Don't over-saturate
- Use Color Boost instead of raw Saturation
- If you see it, back off Saturation or add noise reduction
- You can also use Lum vs Sat to reduce saturation in shadows (like you did for
  highlights in N4)

### Typical Settings

| Look                 | Saturation | Color Boost | Notes                   |
| -------------------- | ---------- | ----------- | ----------------------- |
| Natural/documentary  | 50         | 5-10        | Minimal boost           |
| Standard narrative   | 52-55      | 10-15       | Healthy but not pushed  |
| Vibrant commercial   | 55-60      | 15-20       | Punchy but not garish   |
| Music video/stylized | 60-70      | 10-15       | Intentionally saturated |
| Desaturated/gritty   | 40-48      | 0           | Intentionally muted     |

### Reading the Vectorscope

The vectorscope shows color saturation and hue:

- **Distance from center = saturation** (more saturated = further out)
- **Angle around circle = hue** (colors are labeled around the edge)
- **Skin tone line** = the line pointing toward "red/yellow" area (about 11
  o'clock)

As you add saturation, the "blob" of your image data should expand outward from
the center.

**Watch for:**

- Any color hitting the edge of the scope (over-saturated)
- Skin tones drifting off the skin tone line (going too orange or too pink)
- Overall shape becoming lopsided (one color much more saturated than others)

### Common Mistakes

❌ **Eyeballing without scopes** - Your eyes adapt and you'll over-saturate ❌
**Boosting saturation to compensate for flat contrast** - Fix contrast in N3
instead ❌ **Ignoring shadow noise** - Zoom in and look at the dark areas ❌
**Making skin look "healthy" by adding saturation** - Skin should be human, not
tanned ❌ **Trying to fix hue problems with saturation** - Hue fixes belong in
N6

### How to Know If You're Done

**The "Is this a photo or real life?" test:** Look away from the screen for 30
seconds (rest your eyes), then look back. Does the image look:

- Natural and believable? ✓
- Like it has a filter? ✗ (back off saturation)
- Still a bit flat? (add a bit more, carefully)

**The bypass test (Cmd/Ctrl + D):** The difference should be:

- Noticeable but not dramatic
- Makes the image feel more alive
- Doesn't make you go "whoa, that's colorful"

### Extra Nodes to Consider (N5 variants)

**N5b - Shadow Saturation Control:** If shadows are getting noisy but you want
more saturation in midtones:

```
HOW: Add another Lum vs Sat curve node
CURVE: Pull down the LEFT side (shadows) to desaturate darks
KEEP: Midtones and highlights at normal
```

**N5c - Saturation by Color (Hue vs Sat):** If you want different saturation
amounts for different colors:

```
TOOL: Hue vs Sat curve
LOCATION: Curves panel → dropdown → Hue vs Sat
HOW: Boost saturation for weak colors, reduce for strong colors
```

**N5d - Vibrance vs Saturation:** Resolve doesn't have a "Vibrance" slider like
Lightroom, but Color Boost is similar. If you want more control:

```
HOW: Use a Parallel Node structure
NODE A: High saturation on midtones only (use qualifier)
NODE B: Low saturation on already-saturated areas
BLEND: Mix to taste
```

### End State Checklist

✓ Image feels alive and present (not flat) ✓ Skin tones look human (not orange
or fake-tanned) ✓ No visible chroma noise in shadows ✓ Sky and foliage look
natural (not neon) ✓ No colors hitting the edge of the vectorscope ✓ Looks like
reality, not Instagram

---

## N6 — HUE SCULPT (SELECTIVE COLOR CONTROL)

**Purpose:** Fine-tune individual colors to make the world feel lush,
intentional, and subtly controlled. This is where you decide which colors matter
and shape them precisely.

```
┌─────────────────────────────────────────────────────────────────┐
│ TOOLS: Hue vs Hue / Hue vs Sat / Hue vs Lum curves              │
│ LOCATION: Color page → Bottom left panel → Curves →             │
│           Click dropdown at top → Select curve type             │
│                                                                 │
│ Hue vs Hue: Shifts one hue toward another (e.g., cyan → blue)   │
│ Hue vs Sat: Boosts/reduces saturation of specific hues          │
│ Hue vs Lum: Brightens/darkens specific hues                     │
│                                                                 │
│ HOW TO USE: Click on the curve to add a point, drag to adjust   │
│ TIP: Ctrl/Cmd + click on the image to auto-select that hue      │
└─────────────────────────────────────────────────────────────────┘
```

### What This Node Actually Does

N5 was about "how much color" overall. N6 is about "which specific colors" and
where they point.

Common uses:

- Making sky a cleaner, more pleasant blue
- Making foliage a healthier green
- Removing ugly color pollution (like magenta in shadows)
- Unifying the color palette

**The goal:** A world that feels cohesive and intentional, where no color
distracts or feels "off."

### Understanding the Three Curve Types

**HUE vs HUE (Steering Colors)**

- X-axis: Input hue (the color wheel, left to right)
- Y-axis: Output hue offset (how much to shift)
- Use: Changing one hue toward another (cyan → blue, yellow-green → green)

**HUE vs SAT (Selective Saturation)**

- X-axis: Hue (the color wheel)
- Y-axis: Saturation adjustment for that hue
- Use: Boosting or reducing saturation of specific colors only

**HUE vs LUM (Selective Brightness)**

- X-axis: Hue (the color wheel)
- Y-axis: Luminance adjustment for that hue
- Use: Making specific colors brighter or darker (use sparingly)

### The Color Wheel Layout

The hue axis follows the color wheel:

```
Left side → Middle → Right side
Red → Orange → Yellow → Green → Cyan → Blue → Magenta → Red

Approximate positions:
0° / 360°: Red
30°: Orange
60°: Yellow
120°: Green
180°: Cyan
240°: Blue
300°: Magenta
```

### Step-by-Step Workflow

**Step 1: Identify Problem Colors**

Before touching anything, look at your image and ask:

- Is any color ugly or distracting?
- Is the sky a weird cyan instead of blue?
- Is foliage too yellow or too blue-green?
- Are there magenta or green casts in shadows?

**Step 2: Use Hue vs Hue for Color Steering**

**Most common fix - Cleaning up sky:**

Skies often record as cyan (blue-green) instead of pure blue. To fix:

1. Select "Hue vs Hue" from dropdown
2. Ctrl/Cmd + click on the sky in your image - this auto-selects that hue
3. Three points appear on the curve
4. Drag the CENTER point DOWN to push cyan toward blue
5. The outer two points are "boundaries" - they limit the effect

**How much to move:** Very small amounts. Moving 5-10 degrees is a lot. If you
move 20°, you've gone way too far.

**Visual check:** Does the sky look like a sky you'd see in real life? Not gray,
not electric, just... sky blue?

**Step 3: Use Hue vs Sat for Selective Energy**

After steering colors, you might want to boost or reduce specific ones:

**Boosting weak colors:**

1. Select "Hue vs Sat"
2. Ctrl/Cmd + click on a color you want to boost
3. Drag the center point UP to add saturation to just that hue

**Reducing distracting colors:**

1. Ctrl/Cmd + click on the distracting color
2. Drag the center point DOWN to reduce its saturation

**Common Hue vs Sat moves:**

| Color            | Direction    | Why                          |
| ---------------- | ------------ | ---------------------------- |
| Sky blues        | Slight UP    | Make sky feel present        |
| Foliage greens   | Slight UP    | Lush, healthy vegetation     |
| Skin             | DO NOT TOUCH | Seriously, don't             |
| Magentas         | DOWN         | Remove pollution/artifacting |
| Cyans in shadows | DOWN         | Reduce color cast            |

**Step 4: Protect Skin Tones (CRITICAL)**

Skin tones live in a specific hue range (roughly orange/red, around 15-40° on
the hue circle).

**NEVER adjust this region in Hue vs Hue.** Even small shifts make skin look
sick or fake.

**How to protect skin:**

- Look at where your adjustment points are
- Make sure your curve is FLAT through the skin tone region
- Add anchor points on either side of the skin region if needed
- Test by looking at faces - any shift should be invisible

**Skin protection visualization:**

```
        DO NOT TOUCH THIS REGION
                ↓
    ┌──────────────────────┐
____│                      │____
    └──────────────────────┘
    ~15°                  ~40°
   (Red)                (Orange)
```

### Common Color Problems and Fixes

| Problem                 | Tool               | Fix                                     |
| ----------------------- | ------------------ | --------------------------------------- |
| Sky is too cyan         | Hue vs Hue         | Pull cyan region toward blue (down)     |
| Sky is dull/gray        | Hue vs Sat         | Boost saturation in blue/cyan region    |
| Grass is too yellow     | Hue vs Hue         | Pull yellow-green toward green          |
| Grass is neon green     | Hue vs Sat         | Reduce saturation in green region       |
| Skin is too orange      | N2 (white balance) | Fix in primary balance, not here        |
| Magenta in shadows      | Hue vs Sat         | Reduce saturation in magenta region     |
| Everything feels same-y | Hue vs Hue         | Create subtle separation between colors |

### The "It Looks Graded" Warning

**If you can tell colors have been pushed, you've gone too far.**

This node should be invisible to normal viewers. The sky should just look like a
nice sky, not a "color graded sky."

**Signs you've overdone it:**

- Colors look "selected" or isolated
- The image feels artificial or "digital"
- Someone watching would say "that sky is really blue"
- Faces look strange (even slightly)

**Fix:** Reduce all your adjustments by 50% and see if it still works.

### Common Mistakes

❌ **Moving hues too far** - 5-10° is usually plenty, 20°+ is almost always
wrong ❌ **Touching skin tones** - This is not where skin issues get fixed ❌
**Making every color "better"** - The goal is cohesion, not perfection ❌
**Using Hue vs Lum aggressively** - Changes brightness weirdly, use sparingly ❌
**Forgetting to create boundaries** - Without outer anchor points, changes bleed
into other colors

### How to Know If You're Done

**The "turn it off" test:** Toggle N6 bypass. The difference should be:

- Barely noticeable to a casual viewer
- You notice the sky is slightly better/cleaner
- You notice foliage feels slightly more natural
- Skin looks exactly the same

**The "someone else" test:** Show someone the image without telling them you
graded it. Ask "does anything look weird or off?" If they say no, you're good.

### Extra Nodes to Consider (N6 variants)

**N6b - Qualifier-Based Color Isolation:** For more precise control than curves
allow:

```
TOOL: Qualifier (HSL)
LOCATION: Bottom center panel → Qualifier tab
HOW: Use eyedropper to select specific color, adjust only that selection
USE: When curves affect too broad a range
```

**N6c - Color Warper:** Alternative to Hue vs Hue curves with a visual grid
interface:

```
TOOL: Color Warper
LOCATION: Color page → Bottom left panel → Color Warper tab (grid icon)
HOW: Drag color points on the grid to shift them
USE: When you want to visualize color relationships spatially
```

**N6d - RGB Mixer (for extreme color control):** For technical color separation:

```
TOOL: RGB Mixer
LOCATION: Color page → Primaries panel → RGB Mixer tab
USE: Advanced color channel mathematics - use only if you know what you're doing
```

**N6e - Skin Isolation Node:** If skin needs its own treatment:

```
HOW: Add parallel node, use Qualifier to select skin only
THEN: Adjust hue, saturation, luminance for skin independently
TIP: Use very soft selection and check edges carefully
```

### End State Checklist

✓ Sky looks like a pleasant, natural sky ✓ Foliage looks healthy, not neon ✓ No
distracting color casts in shadows ✓ Skin tones are UNCHANGED from N5 ✓ A normal
viewer wouldn't notice colors have been adjusted ✓ The world feels cohesive and
intentional

---

## N7 — TEXTURE CONTROL

**Purpose:** Control the perceived "feel" of the image - smooth vs. detailed,
digital vs. filmic. This is subtle but makes a significant difference in how
professional the image looks.

```
┌─────────────────────────────────────────────────────────────────┐
│ TOOL: Midtone Detail (aka Mid/Detail or MD)                     │
│                                                                 │
│ LOCATION 1: Color page → Primaries Wheels/Bars →                │
│             Look for "Mid/Detail" slider in the adjustment      │
│             controls (same row as Contrast, Pivot, etc.)        │
│                                                                 │
│ LOCATION 2: Color page → Log Wheels →                           │
│             Bottom bar → "Mid Detail" or "MD" slider            │
│             (next to Color Boost, Shad, Hi/Light)               │
│                                                                 │
│ NOTE: Both locations control the SAME parameter - use whichever │
│       panel you're already working in. They are not independent.│
│                                                                 │
│ SHARPEN (different tool, use sparingly):                        │
│ LOCATION: Color page → Blur panel (water drop icon) →           │
│           Sharpen section → H/V sliders                         │
└─────────────────────────────────────────────────────────────────┘
```

### What This Node Actually Does

Modern digital cameras and phones apply aggressive processing that can make
images look "digital" - overly sharp, crunchy, with weird halos around edges.
This node fixes that.

**Midtone Detail** adjusts local contrast in the midtones:

- **Negative values:** Soften the image, reduce harsh digital texture
- **Positive values:** Add perceived detail and "pop"

**Think of it like this:** You're not blurring or sharpening the whole image -
you're adjusting how "crunchy" vs. "smooth" the texture feels.

### Understanding Midtone Detail

**What "midtone detail" actually controls:** It's a local contrast adjustment
that affects texture perception. When you reduce it, fine details become
smoother. When you increase it, fine details pop more.

**Negative Midtone Detail (-5 to -20):**

- Smooths skin texture (without blurring features)
- Removes harsh digital processing look
- Makes the image feel more "filmic" or "organic"
- Good for: beauty work, skin, soft natural scenes

**Positive Midtone Detail (+5 to +20):**

- Enhances perceived detail and texture
- Makes surfaces pop (brick, metal, fabric)
- Adds "crunch" and definition
- Good for: landscapes, architecture, product shots, gritty scenes

### How to Find Midtone Detail

This confuses people because it's not where you'd expect:

1. Go to Color page
2. Look at the bottom-left panel (where Color Wheels, Curves, etc. are)
3. Find the icon that looks like a water drop (Blur panel)
4. Click it
5. Look for "Mid Detail" slider (might be abbreviated "MD")

**If you can't find it:**

- Try different panel layouts (drop-down arrow at top of panel)
- It's in the same area as Blur, Sharpen, and Mist
- It's NOT in Effects Library - it's built into the Color page

### Step-by-Step Workflow

**Step 1: Assess Your Image**

Look at your image at 100% zoom and ask:

- Does skin look harsh or pore-y? → Reduce MD
- Do textures look flat or mushy? → Increase MD
- Does the image feel "digital"? → Probably reduce MD
- Is it from a phone that over-processed? → Reduce MD

**Step 2: Make Your Adjustment**

For most footage:

| Source                    | Starting Point | Why                                    |
| ------------------------- | -------------- | -------------------------------------- |
| Cinema camera (ARRI, RED) | 0 to +5        | Already looks good, maybe slight boost |
| Sony/Blackmagic           | 0 to +5        | Generally clean, might want subtle pop |
| Panasonic                 | -5 to 0        | Can be slightly harsh                  |
| Canon DSLR                | -10 to 0       | Often over-sharpened in camera         |
| iPhone/phone              | -15 to -5      | Heavy processing to undo               |
| GoPro                     | -10 to -5      | Aggressive sharpening                  |
| Drone (DJI)               | -5 to +5       | Varies by model                        |

**Step 3: Check Specific Areas**

Zoom to 100% and check:

| Area       | What to Look For                 | Fix                          |
| ---------- | -------------------------------- | ---------------------------- |
| Skin       | Pores visible but not emphasized | Reduce MD if pores are harsh |
| Hair       | Natural texture, no halo         | Reduce if haloing appears    |
| Fabric     | Texture visible, not crunchy     | Adjust to taste              |
| Eyes       | Sharp but not weird              | Should be naturally detailed |
| Background | Smooth gradients                 | Check for artifacts          |

### What Are "Halos" and How to Avoid Them

**Halos = bright or dark edges around objects, especially against contrasting
backgrounds**

You'll see them as:

- Light line around a dark object against a bright background
- Dark line around a bright object against a dark background
- "Glow" around hair or tree branches against sky

**What causes halos:**

- Over-sharpening
- Too much positive Midtone Detail
- Aggressive in-camera processing

**How to fix:**

- Reduce Midtone Detail
- Don't use Sharpen (or use very little)
- Accept that some detail is less important than avoiding artifacts

### Understanding the Blur Panel Controls

The Blur panel has several controls:

**Radius (Blur):**

- Actual blur amount
- Usually leave at 0 unless you want intentional softening
- Use with windows for selective blur

**H/V Sharpen:**

- Horizontal and Vertical sharpening
- Very aggressive - use sparingly (2-8 max)
- Creates halos easily
- Only use if image is genuinely soft

**Mist:**

- Adds halation/glow to highlights
- Creates a dreamy, filmic look
- Usually 0-10% if used at all

**Mid Detail:**

- What we've been discussing
- -50 to +50 range
- Most useful control for texture

**Scaling:**

- Controls the "size" of the detail adjustment
- Usually leave at default
- Lower = finer detail, Higher = broader detail

### Common Mistakes

❌ **Using Sharpen instead of Mid Detail** - Sharpen creates halos, Mid Detail
is more organic ❌ **Going too extreme** - ±20 is usually maximum, ±10 is
typical ❌ **Not checking at 100% zoom** - You can't see texture issues zoomed
out ❌ **Applying same value to all shots** - Different sources need different
treatment ❌ **Using Clarity** - If your version of Resolve has it, avoid -
creates unnatural local contrast

### How to Know If You're Done

**The zoom test:**

1. View at 100% zoom
2. Look at skin - does it look like real skin? Not waxy, not harsh?
3. Look at fabric - can you see the texture without it being distracting?
4. Look at hair - natural edges, no halos?
5. Look at sky/gradients - smooth, no artifacts?

**The bypass test (Cmd/Ctrl + D):** The change should be:

- Subtle but real
- Makes the image feel more "organic" or "film-like" (if reducing)
- Makes texture more present (if increasing)
- Not dramatically different

### Extra Nodes to Consider (N7 variants)

**N7b - Skin Softening (selective):** If skin needs more softening than the rest
of the image:

```
HOW: Add serial node, use Qualifier to select skin tones
THEN: Add blur (2-4 radius) or more negative Mid Detail
TIP: Keep it subtle - waxy skin looks worse than textured skin
```

**N7c - Detail Enhancement (selective):** If you want to boost detail in
specific areas only:

```
HOW: Add serial node, use Power Window to select area
THEN: Boost Mid Detail or add slight Sharpen (2-4)
USE: Eyes, product details, architectural elements
```

**N7d - Grain Addition:** Film grain can hide digital artifacts and add organic
feel:

```
TOOL: Film Grain (OpenFX)
LOCATION: Effects Library → OpenFX → ResolveFX Texture → Film Grain
SETTINGS: Start subtle - Grain Amount 5-15, Size 1-2
TIP: Grain also helps hide banding in gradients
```

**N7e - Noise Reduction (if not done in N1):** If you still have noise issues:

```
TOOL: Noise Reduction
LOCATION: Effects Library → OpenFX → ResolveFX Refinement → Noise Reduction
SETTINGS: Temporal NR first (Threshold 10-25), Spatial NR sparingly
WARNING: Too much NR = waxy, artificial look
```

### End State Checklist

✓ Skin looks natural (not waxy, not harsh) ✓ No halos around edges (especially
hair against sky) ✓ Textures feel organic, not digital ✓ Image has appropriate
"feel" for its content ✓ Change is subtle - wouldn't be noticed by casual viewer

---

## N8 — VIGNETTE / EYE GUIDANCE (OPTIONAL)

**Purpose:** Subtly guide the viewer's eye toward the subject without them
noticing. This is invisible compositional control.

```
┌─────────────────────────────────────────────────────────────────┐
│ TOOL: Power Window (Circle)                                     │
│ LOCATION: Color page → Bottom CENTER panel → Window tab         │
│           → Click the Circle icon to add a circular window      │
│                                                                 │
│ THEN: Use the Lift/Gamma/Gain wheels to darken the area         │
│       OR use the Master Wheel (large wheel) in the center       │
│                                                                 │
│ SOFTNESS: In the Window panel, use "Soft" sliders (1-4)         │
│           to feather the edge                                   │
│                                                                 │
│ INVERT: Click the "Invert" button to affect OUTSIDE the window  │
└─────────────────────────────────────────────────────────────────┘
```

### What This Node Actually Does

A vignette subtly darkens the edges of the frame, drawing attention toward the
center (where your subject usually is). When done right, viewers never notice
it - they just naturally look where you want them to look.

**The key word is "invisible."** If anyone notices the vignette, you've done it
wrong.

### When to Use This Node

**USE IT when:**

- Your composition has a clear subject you want to emphasize
- The edges of frame are distracting
- You want a slightly more "finished" feel
- The subject doesn't move significantly

**SKIP IT when:**

- Wide shots with important edge content
- Fast-moving subjects
- Documentary/natural style
- The shot already has natural vignetting from the lens

### Step-by-Step Workflow

**Step 1: Add a Circle Window**

1. Click on your N8 node
2. Go to the bottom-CENTER panel (not bottom-left)
3. Click "Window" tab
4. Click the Circle icon

A circle overlay appears on your image.

**Step 2: Position and Size the Window**

1. Drag the center point to position (usually near subject's face)
2. Drag the corner handles to resize
3. Make it large enough to cover the subject and important surroundings
4. Usually covers 60-80% of the frame

**Step 3: Invert the Selection**

By default, the window affects INSIDE the circle. You want to darken OUTSIDE.

1. Look for "Invert" button in the Window panel
2. Click it - now your adjustments affect only the edges of frame

**Step 4: Add Softness (CRITICAL)**

This is the most important step. A hard-edged vignette looks terrible.

1. Find the "Soft" sliders (usually labeled 1-4)
2. Crank them UP - try 70-100 to start
3. You want the transition to be extremely gradual
4. No visible edge should exist

**Step 5: Darken the Edges**

Now use the color wheels to darken the selected (outside) area:

**Method 1 - Master Wheel:**

- Find the large wheel labeled "Master" or the center Offset-like control
- Drag it down slightly (darker)
- Amount: 0.15-0.25 stops (very subtle)

**Method 2 - Gamma:**

- Drag Gamma wheel down slightly
- Affects midtones more than shadows or highlights
- Often looks more natural than pure exposure reduction

**Step 6: Check Your Work**

1. Press Cmd/Ctrl + Shift + H to hide the window overlay
2. Look at the image - can you see the vignette?
3. If yes, either add more softness or reduce the darkening
4. Toggle node bypass - the change should be barely perceptible

### How Dark Is Too Dark?

**Good vignette:**

- You can't see where it starts or ends
- Edges feel naturally darker (like peripheral vision)
- Subject seems naturally more prominent
- Takes 5-10 seconds for someone to notice if you point it out

**Bad vignette:**

- Visible oval or circle shape
- Obviously darker corners
- Looks like an Instagram filter
- Anyone watching immediately thinks "vignette"

**Typical amounts:**

- Light vignette: 0.1-0.15 stops darker
- Standard vignette: 0.15-0.25 stops darker
- Heavy vignette (rarely appropriate): 0.25-0.4 stops darker

### Using Tracking for Moving Subjects

If your subject moves significantly, the vignette should follow them:

**Step 1: Enable tracking**

1. In the Window panel, find "Tracker" section
2. Select tracking type (usually "Cloud Tracker" or point tracker)
3. Click "Track Forward" and/or "Track Reverse"

**Step 2: Verify**

- Play through the clip
- Make sure the center stays roughly on your subject
- Adjust tracking if it drifts

### Using Shapes Other Than Circles

Power Windows come in several shapes:

| Shape       | Use Case                                     |
| ----------- | -------------------------------------------- |
| Circle/Oval | Most common, general vignette                |
| Rectangle   | Widescreen compositions                      |
| Polygon     | Irregular shapes, avoiding specific elements |
| Gradient    | Directional darkening (top-down, side)       |
| Bezier      | Complex, custom shapes                       |

For vignettes, Circle/Oval is almost always correct.

### Common Mistakes

❌ **Not enough softness** - Hard edges are immediately visible ❌ **Too much
darkening** - Looks like a filter, not subtle guidance ❌ **Centered
incorrectly** - Should focus on subject, not geometric center ❌ **Using on
every shot** - Some shots don't need it ❌ **Visible corners** - If you can see
the corners are dark, it's too much ❌ **Forgetting to invert** - You darken the
subject instead of the edges

### How to Know If You're Done

**The "stare at it" test:** Look at your image for 30 seconds. Does your eye
naturally go to the subject? Do the edges feel like they're just... there,
nothing special?

**The "show someone" test:** Show someone the image and ask where their eye
goes. Don't mention vignette. If they look at the subject and don't mention the
edges being dark, you've succeeded.

**The bypass test (Cmd/Ctrl + D):** The change should be:

- Almost imperceptible
- Makes the subject feel slightly more present
- Doesn't change the mood or feel dramatically

### Extra Nodes to Consider (N8 variants)

**N8b - Directional Falloff:** Instead of circular vignette, darken from a
direction (like simulating window light):

```
HOW: Use Gradient window instead of Circle
SHAPE: Linear gradient from one side
USE: Simulating directional light, drawing eye in specific direction
```

**N8c - Subject Highlight:** Instead of darkening edges, slightly brighten the
subject:

```
HOW: Circle window (not inverted), slight Gamma lift
AMOUNT: Very subtle (+0.1 to +0.15 stops)
TIP: Even more invisible than darkening edges
```

**N8d - Multiple Vignettes:** For complex compositions:

```
HOW: Create multiple Circle windows in same node
OR: Use multiple N8 nodes with different windows
USE: Multiple subjects, avoiding bright elements at different edges
```

### End State Checklist

✓ Window is inverted (affecting outside, not inside) ✓ Softness is very high (no
visible edge) ✓ Darkening is subtle (0.15-0.25 stops) ✓ Center is positioned on
subject (not frame center) ✓ Change is invisible unless pointed out ✓ Eye
naturally goes to subject

---

## N9 — CREATIVE LOOK

**Purpose:** Apply artistic intent - the "style" or "mood" that makes this
project feel like itself. This is where you finally get to be creative.

```
┌─────────────────────────────────────────────────────────────────┐
│ MULTIPLE TOOLS - CHOOSE YOUR APPROACH:                          │
│                                                                 │
│ LUT APPLICATION:                                                │
│ → Right-click on node → LUT → Browse for your .cube file        │
│ → Or: Color page → LUTs tab (top right) → drag onto node        │
│                                                                 │
│ COLOR WHEELS (for manual look):                                 │
│ → Color page → Bottom left → Color Wheels                       │
│ → Push Gain toward warm (orange), Lift toward cool (blue)       │
│                                                                 │
│ PER-CHANNEL CURVES (for advanced look):                         │
│ → Color page → Curves → Click "Custom" dropdown                 │
│ → Select individual channels: Y (luma), R, G, or B              │
│ → Edit Blue channel: raise shadows, lower highlights = warm     │
└─────────────────────────────────────────────────────────────────┘
```

### What This Node Actually Does

Everything before N9 was about making the image technically correct and
perceptually pleasing. N9 is about making it emotionally correct - adding the
mood, style, and feeling that serves the story.

**The key insight:** Style only works when applied to a solid foundation. If
your N1-N8 are good, almost any look will work. If they're bad, no look will
save you.

### Approaches to Creating a Look

**Method 1: Using a LUT (Fastest)**

A LUT (Look-Up Table) is a preset color transformation. Someone else already did
the work.

**To apply a LUT:**

1. Right-click on N9 node
2. Select "LUT"
3. Browse to your .cube file
4. Click to apply

**Important:** LUTs are designed for specific input. Most expect:

- Rec.709 input (standard video)
- Properly balanced footage
- Correct exposure

If your footage isn't properly balanced, the LUT will look wrong.

**Adjusting LUT intensity:** LUTs at 100% are often too strong. To reduce:

1. Right-click node → Add Node → Parallel Layer
2. One path: original (no LUT)
3. Other path: LUT applied
4. Adjust Key Output (node opacity) to blend

Or simply:

- Apply LUT to a Layer Mixer node
- Adjust layer opacity to taste (try 30-50%)

**Method 2: Manual Color Wheel Look (Most Control)**

Using the color wheels to create color separation between shadows/highlights:

**Classic "Orange and Teal" (Hollywood standard):**

- Lift (shadows): Push toward TEAL/BLUE
- Gain (highlights): Push toward ORANGE/WARM
- Gamma (midtones): Keep neutral or very slight warm
- Amount: Very subtle - barely visible individually

**Warm/Nostalgic:**

- Lift: Warm (toward yellow/orange)
- Gain: Warm, slightly more than lift
- Gamma: Slight warm
- Overall saturation: Might reduce slightly

**Cold/Clinical:**

- Lift: Cool blue
- Gain: Cool, slightly cyan
- Gamma: Neutral
- Overall: Desaturated feeling

**High Contrast Drama:**

- Lift: Deep, cool blue
- Gain: Hot, slightly yellow
- Contrast: Increased
- Saturation: Often reduced in shadows, boosted in highlights

**Method 3: Per-Channel Curves (Advanced)**

Each RGB channel can be adjusted separately:

**Blue Channel (most common for looks):**

- Raise shadows = Blue shadows
- Lower highlights = Warm/yellow highlights
- This is the basis of most "cinematic" looks

**Red Channel:**

- Raise shadows = Magenta/warm shadows
- Lower highlights = Cyan highlights

**Green Channel:**

- Raise shadows = Green shadows (rarely wanted)
- Lower highlights = Magenta highlights

**Classic Blue Shadows / Warm Highlights:**

1. Go to Curves → dropdown → select "B" (Blue channel)
2. Add point in shadows (around 20-30%), drag UP slightly
3. Add point in highlights (around 70-80%), drag DOWN slightly
4. Result: Blue-ish shadows, warm-ish highlights

### Common Look Recipes

**Clean/Natural (Documentary, Corporate)**

- Lift: Neutral
- Gamma: Neutral
- Gain: Neutral to very slight warm
- Saturation: Default to +5
- Intent: Just enhance reality, don't stylize

**Warm/Inviting (Commercials, Rom-coms)**

- Lift: Slight warm
- Gamma: Slight warm
- Gain: Warm (toward yellow-orange)
- Saturation: +5 to +10
- Intent: Everything feels sunny and welcoming

**Cool/Tense (Thriller, Drama)**

- Lift: Blue/teal
- Gamma: Neutral to slight cool
- Gain: Neutral to slight warm (for skin)
- Saturation: -5 to default
- Intent: Tension, unease

**High Fashion/Beauty**

- Lift: Neutral to slight cool
- Gamma: Bright, open
- Gain: Clean white
- Saturation: Controlled, skin neutral
- Intent: Clean, aspirational

**Gritty/Desaturated (War films, Gritty drama)**

- Lift: Raised (not black)
- Gamma: Flat
- Gain: Rolled off (not white)
- Saturation: -20 to -40
- Intent: Raw, documentary feel

**Orange and Teal (Action, Blockbuster)**

- Lift: Teal (cyan-blue)
- Gamma: Neutral
- Gain: Orange-warm
- Saturation: Boosted
- Intent: Punchy, exciting

### The "Toggle Test"

This is the most important test for any creative look:

1. Toggle N9 off (Cmd/Ctrl + D)
2. Look at the image without the look
3. Toggle N9 back on
4. Ask: Does this look ADD to the story, or is it just "different"?

**If the look is good:**

- The image feels more emotionally appropriate with it on
- The mood matches the content
- It enhances without distracting

**If the look is bad:**

- The image is just "more blue" or "more orange" without purpose
- The mood doesn't match the content
- You notice the look more than the image

### Applying Looks Consistently

Once you have a look you like:

**Method 1: Group Clips**

1. Select all clips in the scene
2. Right-click → Add Clips to Group
3. Apply look to Group Pre-Clip or Group Post-Clip grade
4. Individual clips can still have shot-specific adjustments

**Method 2: Use Shared Nodes**

1. In the node graph, right-click → Add Node → Shared Node
2. Apply your look to this shared node
3. Shared node syncs across all clips using it
4. Change once, changes everywhere

**Method 3: PowerGrade**

1. Create your look on one clip
2. Right-click thumbnail → Grab Still
3. Gallery → Right-click → Export → PowerGrade
4. Apply to other clips, adjust as needed

### Common Mistakes

❌ **Adding a look before N1-N8 are done** - Style on bad foundation = garbage
❌ **Going too extreme** - If anyone says "wow, that's really teal," it's too
much ❌ **Using a look that doesn't match content** - Cool look on a warm beach
scene = wrong ❌ **Same look for everything** - Different scenes might need
different intensities ❌ **Fighting the footage** - Work with what you shot, not
against it ❌ **Forgetting about skin** - Skin should still look human under any
look

### Extra Nodes to Consider (N9 variants)

**N9b - LUT Blend Node:** To control LUT intensity properly:

```
HOW: Add Parallel structure before LUT
NODE A: Original (bypass)
NODE B: LUT applied
OUTPUT: Adjust Key Output to blend (30-50% common)
```

**N9c - Film Print Emulation:** For the most authentic film look:

```
TOOL: Film print emulation LUT + subtle grain
WHERE: N9 for color, N7 for grain
TIP: Real film looks are subtle - 10-30% LUT strength
```

**N9d - Shot-Specific Look Adjustments:** When some shots need different
intensity:

```
HOW: Add node after N9 for shot-specific tweaks
USE: Day-to-night adjustment, interior vs exterior intensity
KEEP: Base look consistent, only adjust intensity
```

### End State Checklist

✓ Look matches the emotional intent of the content ✓ Skin tones still look human
✓ Toggle test passes (image is better with look, not just different) ✓ Look
intensity is appropriate (not obviously graded) ✓ Consistency across shots in
the scene ✓ Enhances the story, doesn't distract from it

---

## N10 — OUTPUT TRIM / LEGAL SAFE

**Purpose:** Ensure your final output meets technical specifications and doesn't
contain any illegal values. This is your insurance policy.

```
┌─────────────────────────────────────────────────────────────────┐
│ TOOL: Video Limiter (for broadcast) or Soft Clip                │
│                                                                 │
│ VIDEO LIMITER:                                                  │
│ → Effects Library → OpenFX → ResolveFX Refinement →             │
│   Video Limiter (drag onto node)                                │
│ → Set to "Soft Clip" mode, levels depend on delivery spec       │
│                                                                 │
│ SOFT CLIP (built-in):                                           │
│ → Color page → Bottom left → Primaries Bars →                   │
│   High Soft / Low Soft sliders (clip highlights/shadows)        │
│                                                                 │
│ CHECKING LEVELS:                                                │
│ → View your Waveform scope - nothing should exceed 100%         │
│   or go below 0% (for web) or outside 16-235 (for broadcast)    │
└─────────────────────────────────────────────────────────────────┘
```

### What This Node Actually Does

This node catches any values that might have crept out of legal range during
your grading. It's a safety net, not a creative tool.

**If your grade is good, this node does almost nothing.** That's the point.

### Understanding Legal Levels

Different delivery destinations have different requirements:

**Web/YouTube/Vimeo (Full Range):**

- Black: 0%
- White: 100%
- No illegal values exist - anything is technically "legal"
- But clipped highlights (100%) have no detail

**Broadcast (Legal Range / Limited Range):**

- Black: 16 (8-bit) / 64 (10-bit) = ~6%
- White: 235 (8-bit) / 940 (10-bit) = ~92%
- Values outside this range can cause problems
- Broadcast equipment may clip or behave unexpectedly

**HDR (Depends on Standard):**

- Different rules entirely
- Consult specific HDR specifications

### Step-by-Step Workflow

**Step 1: Check Your Delivery Specs**

Before doing anything, know your destination:

- YouTube/Web: Full range (0-100%)
- TV broadcast: Legal range (16-235)
- Streaming (Netflix, etc.): Check their specific requirements
- Film festival: Usually full range

**Step 2: Check Your Current Levels**

Open your Waveform scope:

1. Look at the very top - anything hitting or exceeding 100%?
2. Look at the very bottom - anything at or below 0%?
3. Any significant amount of clipping?

**Step 3: If Needed, Add Limiters**

**For web (minimal intervention):** Usually just verify no severe clipping. If
highlights are barely touching 100%, it's probably fine.

**For broadcast (Video Limiter):**

1. Effects Library → OpenFX → ResolveFX Refinement → Video Limiter
2. Drag onto N10 node
3. In Inspector, set:
   - Output Range: Legal (or specific values: 64-940 for 10-bit)
   - Soft Clip: ON (more natural than hard clipping)

**Step 4: Final Check**

After adding limiters:

1. View waveform again
2. Verify all values are now within legal range
3. Play through the timeline looking for any spikes

### Soft Clip vs. Hard Clip

**Hard Clip:**

- Everything above the limit becomes exactly the limit value
- Abrupt, can look harsh
- Use only when specs are absolute

**Soft Clip:**

- Gradually compresses values approaching the limit
- More natural rolloff
- Better for maintaining perceived detail

Always prefer Soft Clip unless told otherwise.

### Manual Soft Clipping (Built-in)

If you don't want to use the Video Limiter OFX:

1. Go to Primaries Bars panel
2. Find "High Soft" slider (affects highlights)
3. Pull down slightly to compress peaks
4. Find "Low Soft" slider (affects shadows)
5. Pull up slightly if needed

### Common Mistakes

❌ **Relying on this node to fix bad grades** - Fix problems in earlier nodes ❌
**Using hard limits on web content** - No benefit, just clips your detail ❌
**Forgetting to check specs** - Different destinations need different limits ❌
**Not checking the final output** - Always verify after this node

### Extra Nodes to Consider (N10 variants)

**N10b - Color Space Conversion Out:** If you need to convert to a specific
delivery color space:

```
TOOL: Color Space Transform
USE: Converting from working space to delivery space
EXAMPLE: DaVinci Wide Gamut → Rec.709 for web
WHERE: Can be here or in Deliver page settings
```

**N10c - Dithering (for 8-bit delivery):** If delivering to 8-bit and seeing
banding:

```
TOOL: Add subtle grain or noise
PURPOSE: Dithering hides banding in 8-bit outputs
AMOUNT: Very subtle - just enough to mask banding
```

### End State Checklist

✓ You know your delivery specifications ✓ Waveform shows all values within legal
range ✓ Soft clip is used (not hard clip) where appropriate ✓ No severe clipping
of important detail ✓ Node does minimal work (grade is already good)

---

## WORKFLOW ORDER (SHOT MATCHING) - DETAILED

**Remember: Work HORIZONTALLY, not vertically. Complete each node across ALL
shots before moving to the next node.**

### Phase 0: Setup

1. Grade your **hero shot** first (the best/most representative shot in the
   scene)
2. Build the complete 10-node structure on this shot
3. Save as **PowerGrade**
4. Apply the PowerGrade to all clips in the scene
5. **Grab a still** of your hero shot for reference

### Phase 1: Physical Correction (N1–N4)

**N2 - Primary Balance (ALL SHOTS):**

- Use wipe comparison (Cmd/Ctrl + W) against your hero still
- Match exposure and white balance across all shots
- Check waveform - skin tones should land at similar heights
- This is the most important matching phase

**N3 - Contrast (ALL SHOTS):**

- Apply similar contrast curves across the scene
- Check that shadow and highlight roll-off matches
- Scene should feel cohesive in terms of "density"

**N4 - Highlight Safety (ALL SHOTS):**

- Usually similar settings across all shots
- Verify no highlight blow-out when you boost saturation later

### Phase 2: Perceptual Enhancement (N5–N7)

**N5 - Saturation (ALL SHOTS):**

- Similar saturation levels create visual consistency
- Watch for shots that need less (already punchy) or more (flat)

**N6 - Hue Sculpt (ALL SHOTS):**

- Keep hue adjustments consistent within a scene
- Sky should be the same blue, grass the same green

**N7 - Texture (ALL SHOTS):**

- Match the "feel" across shots
- Same camera = same settings usually

### Phase 3: Finishing (N8–N10)

**N8 - Vignette (SHOTS THAT NEED IT):**

- Not every shot needs vignette
- Apply where it helps guide the eye

**N9 - Creative Look (ALL SHOTS):**

- Same look applied to all = instant consistency
- Fine-tune intensity if needed per shot

**N10 - Output (ALL SHOTS):**

- Verify legal levels
- Final safety check

### Why This Order Works

```
When you do N2 on shot 5:
- Shots 1-4 are already balanced
- You can wipe-compare against them
- Easy to match

When you do N9 on shot 5:
- Shots 1-4 already have the same look
- N9 just needs to be applied, minimal tweaking
- Everything matches automatically
```

If you finished shot 1 completely before starting shot 2, you'd be trying to
match a fully-graded image to raw footage - much harder.

---

## GOLDEN RULES

**Where does this fix belong?**

- Affects all shots → earlier node (N2–N4)
- Affects specific colors → N6
- Affects mood/feel → N9
- Affects one shot only → create a new node after N10

**Before you "just tweak saturation":**

- Stop
- Ask: What am I actually trying to fix?
- Find the right node for that job

**The bypass test:**

- Toggle each node off and on
- If the change is invisible, the node might be unnecessary
- If the image breaks, the node is doing too much

---

## QUICK REFERENCE: SCOPES

```
┌─────────────────────────────────────────────────────────────────┐
│ HOW TO OPEN SCOPES:                                             │
│ → Color page → Top right corner → Click "Scopes" button         │
│ → Or: Workspace menu → Video Scopes                             │
│ → Or: Shortcut Cmd/Ctrl + Shift + W                             │
│                                                                 │
│ TO CHANGE SCOPE TYPE: Right-click on the scope → Select type    │
└─────────────────────────────────────────────────────────────────┘
```

| Scope                 | Use For                         | Watch                      |
| --------------------- | ------------------------------- | -------------------------- |
| Waveform (Luma)       | Exposure, contrast              | Clipping at 0 or 100       |
| Waveform (RGB Parade) | Color balance, channel clipping | Matched tops/bottoms       |
| Vectorscope           | Skin tones, saturation          | Skin on the skin tone line |
| Histogram             | Overall distribution            | Gaps, clipping             |

**Skin tone line:** Runs from center toward 11 o'clock on the vectorscope. All
natural skin tones (any ethnicity) should fall on or very near this line.

---

## COMMON PROBLEMS & FIXES

| Problem               | Likely Cause                | Fix Location                   |
| --------------------- | --------------------------- | ------------------------------ |
| Image feels flat      | Insufficient contrast       | N3                             |
| Skin looks orange     | Over-saturated or wrong WB  | N2 or N5                       |
| Sky is breaking up    | Highlight saturation        | N4                             |
| Shadows are noisy     | Too much saturation or lift | N5 or N2                       |
| Image looks "graded"  | Over-styled                 | N9 (reduce)                    |
| Colors are unnatural  | Hue shifts too aggressive   | N6                             |
| Harsh/digital texture | Needs midtone detail        | N7                             |
| Shot doesn't match    | Primary balance off         | N2                             |
| Halos around objects  | Over-sharpened              | N7 (reduce Mid Detail)         |
| Banding in gradients  | Too much contrast or 8-bit  | N3 (reduce) or add grain in N7 |

---

## POWERGRADE TEMPLATE SETUP

**Getting to the Color page:**

- Press Shift + 6, or click "Color" in the bottom toolbar

**To save this workflow as a template:**

1. Build the complete 10-node structure
2. Label each node clearly (N1, N2, etc. or full names)
3. Right-click the thumbnail → Grab Still
4. In the Gallery, right-click → Export → PowerGrade

**To apply:**

1. Right-click in Gallery → Import → PowerGrade
2. Right-click the grade → Apply Grade
3. Adjust values per shot (structure stays)

---

# PODCAST-SPECIFIC WORKFLOW

For talking-head podcast content, you'll likely need additional techniques
beyond the standard 10-node workflow. This section covers three common podcast
needs:

1. Separating subject from background (brighten face, darken background)
2. Cosmetic enhancement (reducing wrinkles, under-eye circles)
3. Grading full source clips before cutting (so tracking works across the entire
   recording)

---

## PODCAST NODE STRUCTURE

For podcast work, insert these additional nodes after your standard workflow:

```
N1-N10: Standard workflow (as documented above)
N11: Subject/Background Separation (Magic Mask)
N12: Face Enhancement (Face Refinement or Beauty)
N13: Under-Eye Fix (if needed)
N14: Eye Enhancement (if needed)
```

---

## P1 — SEPARATING SUBJECT FROM BACKGROUND

**Purpose:** Brighten the face/subject while darkening or desaturating the
background to make the speaker pop.

```
┌─────────────────────────────────────────────────────────────────┐
│ TOOL: Magic Mask (AI-powered subject isolation)                 │
│ LOCATION: Color page → Bottom center panel → Magic Mask icon    │
│           (between Blur and Tracker icons)                      │
│                                                                 │
│ ALTERNATIVE: Power Window with tracking                         │
│ LOCATION: Color page → Window tab → Circle/Polygon              │
└─────────────────────────────────────────────────────────────────┘
```

### Method 1: Magic Mask (Easiest, Studio Version Only)

Magic Mask uses AI to automatically detect and track people in your footage.

**Step-by-Step:**

1. **Create a new serial node** after your main grade (Alt/Option + S)
2. **Click Magic Mask icon** in the center toolbar
3. **Select "Person"** tab (or "Features" if you only want the face)
4. **Click "Add" tool** (plus icon)
5. **Draw a stroke** through your subject's torso/head - just a quick line, not
   an outline
6. The AI will automatically detect and mask the person (shown in red overlay)
7. **Toggle Mask Overlay** (icon looks like a square) to see what's selected
8. **Track the mask:** Click the forward/backward tracking buttons to track
   through the clip
9. Now any adjustments on this node only affect the person

**To affect BACKGROUND instead:**

- Click **"Invert"** button in the Magic Mask panel
- Now adjustments affect everything EXCEPT the person

**Typical Background Adjustments (on inverted mask):**

- Lower exposure (Offset down -0.15 to -0.30)
- Reduce saturation slightly
- Optional: Add slight blur for depth-of-field effect

**Typical Subject Adjustments (on non-inverted mask):**

- Slight lift to exposure if needed
- Gentle contrast boost
- Keep saturation natural

### Method 2: Power Window with Face Tracking (Works in Free Version)

If you don't have Studio, or Magic Mask isn't working well:

1. **Create a new serial node**
2. **Go to Window tab** (bottom center panel)
3. **Select Circle or Polygon** shape
4. **Draw around your subject's face/upper body**
5. **Add heavy softness** (Soft 1-4 sliders, crank them up)
6. **Go to Tracker tab**
7. **Select "Window"** tracking type
8. **Click Track Forward/Backward** to track the window
9. Apply adjustments to the subject

**To affect background:** Click "Invert" in the Window panel.

### Podcast-Specific Tips

**For stationary talking heads:**

- Tracking is usually simple since the subject doesn't move much
- You can often get away with a static oval window with heavy feathering
- Check beginning, middle, and end of clip for drift

**For multiple people in frame:**

- Use Magic Mask → Features → Face to isolate just faces
- Or create separate nodes with separate masks for each person

**Performance note:** Magic Mask can be slow on long clips. Consider:

- Rendering cache (right-click clip → Render Cache → User)
- Or work with proxies during editing

---

## P2 — COSMETIC ENHANCEMENT (FACE REFINEMENT)

**Purpose:** Subtly improve skin appearance - reduce wrinkles, smooth skin,
brighten eyes - without looking "filtered."

```
┌─────────────────────────────────────────────────────────────────┐
│ TOOL: Face Refinement (OpenFX plugin - Studio only)             │
│ LOCATION: Effects Library → OpenFX → ResolveFX Refine →         │
│           Face Refinement (drag onto a node)                    │
│                                                                 │
│ ALTERNATIVE: Beauty / Midtone Detail + Power Windows            │
│ LOCATION: Primaries Bars → Mid/Detail slider (negative values)  │
│           Combined with Window to isolate face                  │
└─────────────────────────────────────────────────────────────────┘
```

### Method 1: Face Refinement Plugin (Recommended for Podcasts)

Face Refinement automatically tracks faces and gives you per-feature controls.

**Step-by-Step:**

1. **Create a new serial node** after subject separation (if used)
2. **Open Effects Library** (top left, or Workspace → Effects Library)
3. **Search "Face Refinement"** or navigate: OpenFX → ResolveFX Refine → Face
   Refinement
4. **Drag onto your node**
5. **In the Inspector (top right), click "Analyze"** - wait for it to track the
   face
6. You should see tracking markers appear on the face in the viewer
7. If not, enable **OpenFX Overlay** in the viewer dropdown

**Recommended Settings for Podcasts:**

| Section            | Setting        | Typical Value    | Notes                                                     |
| ------------------ | -------------- | ---------------- | --------------------------------------------------------- |
| **Texture**        | Smoothing mode | Beauty Automatic | Start here, switch to "Smoothing" for more control        |
| **Texture**        | Texture        | 0.2 - 0.4        | Higher = smoother. Don't go above 0.5 or it looks plastic |
| **Eye Retouching** | Sharpen        | 0.1 - 0.2        | Very subtle, keeps eyes crisp                             |
| **Eye Retouching** | Brighten       | 0.1 - 0.15       | Subtle lift to eye area                                   |
| **Color Grading**  | Contrast       | 0.05 - 0.1       | Slight face contrast boost                                |
| **Forehead**       | Smooth         | 0.1 - 0.3        | Reduces forehead lines                                    |
| **Cheek**          | Smooth         | 0.1 - 0.2        | Reduces cheek lines                                       |

**Critical: Less is more.** If someone can tell you've applied face refinement,
you've gone too far. The goal is "they look good on camera," not "they've been
retouched."

### Method 2: Midtone Detail + Window (Works in Free Version)

If you don't have Face Refinement:

1. **Create a new serial node**
2. **Add a Power Window** (oval) around the face
3. **Track it** using the Tracker panel
4. **Add heavy softness** to the window edges
5. **Go to Primaries Bars**
6. **Set Mid/Detail to -15 to -25** (negative = softer)

This softens skin texture without a dedicated face tool. It's less precise but
works.

### What Each Face Refinement Section Does

**Texture tab:**

- Smoothes skin while (theoretically) preserving detail
- "Beauty Automatic" is the easiest mode
- "Smoothing" mode gives you more control with Detail/Smoothing sliders

**Eye Retouching tab:**

- Sharpen: Increases perceived detail in eyes
- Brighten: Lifts the eye area slightly
- Important: Keep eyes sharp while softening skin

**Lip Retouching tab:**

- Can add color/saturation to lips
- Usually leave alone for podcast/natural look

**Forehead/Cheek/Chin tabs:**

- Per-region smoothing control
- Useful if forehead has lines but cheeks are fine

**Blush tab:**

- Adds artificial blush color to cheeks
- Usually not needed for podcasts (too "made up" looking)

**Color Grading tab:**

- Affects just the face
- Contrast, Tint, Saturation just for the isolated face

---

## P3 — FIXING UNDER-EYE CIRCLES (PANDA EYES)

**Purpose:** Reduce dark circles under eyes without looking unnatural.

```
┌─────────────────────────────────────────────────────────────────┐
│ TOOL: Custom Power Window + Contrast/Exposure adjustment        │
│ LOCATION: Color page → Window tab → Pen tool (custom shape)     │
│           or Circle tool positioned under each eye              │
│                                                                 │
│ CONCEPT: Emulate fill light by reducing contrast in dark areas  │
└─────────────────────────────────────────────────────────────────┘
```

### The Technique (Manual but Effective)

Under-eye circles are essentially shadows. The fix is to emulate what a fill
light would do - lift the shadows without affecting highlights.

**Step-by-Step:**

1. **Create a new serial node** after Face Refinement
2. **Go to Window tab**
3. **Select the Pen tool** (for kidney-bean shapes) or use two small **ovals**
4. **Draw a shape under one eye** - kidney-bean or crescent shape following the
   dark area
5. **Add heavy softness** (Soft sliders to 70-100%) - NO visible edges
6. **Track the window** (Tracker tab → Track Forward/Backward)
7. **Repeat for the other eye** (or use one larger shape covering both)

**Now make the adjustment:**

8. **Reduce Contrast** (pull down to -10 to -25) - This lifts shadows while
   pulling down highlights, reducing the darkness
9. **Slight Gamma lift** if needed (push up +0.05 to +0.10) - Don't overdo it
10. **Check color:** Reduced contrast can make the area look gray/desaturated
11. **Add slight warmth or saturation** to match surrounding skin if needed

**Why Reduce Contrast Instead of Just Lifting?**

- Lifting exposure alone brightens everything including the highlights, looking
  unnatural
- Reducing contrast compresses the tonal range, bringing shadows up AND
  highlights down
- This emulates what fill light actually does - it doesn't brighten, it reduces
  shadow depth

### Common Mistakes

❌ **Hard-edged windows** - Creates obvious patches under eyes ❌ **Lifting too
much** - Under-eye becomes brighter than surrounding skin ❌ **Forgetting to
color match** - Fixed area looks gray compared to skin ❌ **Over-smoothing with
blur** - Looks like concealer applied by a child

### Alternative: Use Face Refinement's Eye Section

Face Refinement has an "Eye Retouching → Brighten" parameter that can help with
minor dark circles, but it's less targeted than a custom window.

---

## P4 — GRADING FULL SOURCE CLIPS (BEFORE CUTTING)

**Purpose:** When you have a pre-cut podcast edit, and you need face tracking or
grades to apply to the FULL recording (not just the cut portions).

```
┌─────────────────────────────────────────────────────────────────┐
│ THE PROBLEM:                                                    │
│ You've edited a 1-hour podcast into 20 cuts from the same clip. │
│ Face tracking only tracks within each cut segment.              │
│ If you track on the cut timeline, each segment tracks           │
│ independently and may not match.                                │
│                                                                 │
│ THE SOLUTION:                                                   │
│ Grade the FULL source clip first, THEN edit (or re-conform).   │
└─────────────────────────────────────────────────────────────────┘
```

### Method 1: Grade First, Edit Second (Best Workflow)

If you're starting a new podcast project:

1. **Import your full recording** into Media Pool
2. **Create a "grading timeline"** with the full, uncut clip
3. **Do all your grading, face tracking, face refinement** on this full clip
4. The tracking runs across the entire recording seamlessly
5. **Grab a still** of your grade and save as PowerGrade
6. **Now create your edited timeline** and do your cuts
7. The grade from the source clip carries over to all cuts

**Why this works:** When you grade a source clip, the grade is attached to the
clip itself. All instances of that clip in any timeline inherit the grade.

### Method 2: Remote Grades (For Already-Edited Timelines)

If you've already edited and need to grade:

1. **In Color page,** right-click on your clip
2. **Enable "Use Remote Grades"** (or find it in the Color menu)
3. Grade one instance of the clip
4. **All other instances of the same source clip** automatically get the same
   grade

**Limitation:** Remote grades share the SAME grade. If you need different grades
for different sections, this won't work.

### Method 3: Create a Full-Clip Timeline for Tracking

If you need face tracking across the full source:

1. **Create a NEW timeline** (File → New Timeline)
2. **Drag the FULL source clip** (uncut) into this timeline
3. **Apply your grade with face tracking/refinement** to this full clip
4. Track the entire duration - Magic Mask or Face Refinement will track
   continuously
5. **Grab a still** of the grade
6. **Go back to your edited timeline**
7. **Apply the grade** from the still to your cut clips

**The tracking data is baked into the grade** from the full clip, so when you
apply it to cuts, the tracking should align (since they're from the same source
at the same timecodes).

### Method 4: Group Pre-Clip for Consistent Base Grade

If your podcast uses the same source clip cut multiple times:

1. **In Color page, select all clips** from that source (use thumbnail view)
2. **Right-click → Add to New Group**
3. **Change Node Editor mode** from "Clip" to "Group Pre-Clip"
4. **Apply your base grade here** (no tracking - just color/exposure)
5. This grade affects ALL clips in the group before individual clip grades

**For tracking:** You'll still need to track each segment individually, but at
least the base grade is unified.

### The Harsh Reality of Podcast Tracking

**Truth:** If you've already cut your timeline into many small pieces, and you
need face tracking that spans the cuts, there's no perfect solution in Resolve.

**Best practices:**

- Grade and track the FULL source BEFORE editing whenever possible
- For already-cut timelines, you may need to track each segment individually
- Use Groups for consistent base grades
- Accept that tracking may need touchup at cut points

---

## PODCAST QUICK REFERENCE

| Task                           | Tool                          | Location                                             |
| ------------------------------ | ----------------------------- | ---------------------------------------------------- |
| Isolate person from background | Magic Mask                    | Color page → Magic Mask icon                         |
| Darken background only         | Magic Mask (inverted)         | Magic Mask → Invert button                           |
| Smooth skin                    | Face Refinement or Mid/Detail | Effects → Face Refinement, or Primaries → Mid/Detail |
| Brighten eyes                  | Face Refinement               | Face Refinement → Eye Retouching → Brighten          |
| Sharpen eyes                   | Face Refinement               | Face Refinement → Eye Retouching → Sharpen           |
| Fix under-eye circles          | Power Window + Contrast       | Window → Pen tool, then reduce Contrast              |
| Grade full source before cuts  | Create grading timeline       | New Timeline with full source clip                   |
| Apply same grade to all cuts   | Remote Grades or Groups       | Right-click → Use Remote Grades, or Group Pre-Clip   |

---

## PODCAST NODE TREE EXAMPLE

```
N1:  Input Transform (if shooting LOG)
N2:  Primary Balance (exposure, white balance)
N3:  Contrast Shaping
N4:  Highlight Safety
N5:  Saturation
N6:  Hue Sculpt
N7:  Texture (midtone detail)
N8:  (skip for podcasts - vignette not usually needed)
N9:  Creative Look (optional)
N10: Output Safety
─────────────────────────────
N11: Background Darken (Magic Mask inverted, reduce exposure)
N12: Subject Enhance (Magic Mask normal, slight lift/contrast)
N13: Face Refinement (smooth skin, brighten eyes)
N14: Under-Eye Fix (Power Window, reduce contrast)
```

**Order matters:** Do cosmetic work AFTER color grading, so your face refinement
is working on correctly balanced footage.

---

## RENDERING, CACHING, AND PERFORMANCE

This section explains the different ways to pre-render footage in Resolve, when
to use each, and how to protect expensive work like Magic Mask tracking.

---

## UNDERSTANDING THE THREE OPTIONS

Render Cache, Render in Place, and Final Render are three different operations
with different purposes.

| Option          | What It Does                        | Files Created                              | Portability | Use Case                          |
| --------------- | ----------------------------------- | ------------------------------------------ | ----------- | --------------------------------- |
| Render Cache    | Pre-renders for smooth playback     | `.dvcc` files in cache folder              | Local only  | Day-to-day editing                |
| Render in Place | Bakes effects into a new media file | Actual video files such as ProRes or DNxHR | Portable    | Protecting work and collaboration |
| Final Render    | Exports from the Deliver page       | Final deliverable                          | Portable    | Client delivery                   |

---

## RENDER CACHE

**What it is:** Resolve pre-renders clips in the background so playback is
smooth.

**Location:** Playback -> Render Cache -> Smart or User

```
SMART MODE:
- RED line = needs caching
- BLUE line = cached, will play smoothly

USER MODE:
- Right-click clip -> Render Cache Color Output -> On
```

**Key points:**

- Cache files are `.dvcc` files, usable only by Resolve and only on your
  machine.
- If you clear cache, move the project, or lose the cache drive, you lose the
  cache.
- Cache is not used in final export by default.
- Good for day-to-day smooth editing.
- Bad for protecting expensive work such as hour-long Magic Mask tracks.

**Cache format setting:** Project Settings -> Master Settings -> Optimized Media
and Render Cache -> Render Cache Format

| Format                  | When to Use                              |
| ----------------------- | ---------------------------------------- |
| DNxHR SQ                | General work, good quality/size balance  |
| DNxHR HQX (10-bit)      | Better quality, larger files             |
| ProRes 422 HQ           | Mac users, good quality                  |
| ProRes 4444 / DNxHR 444 | When you need to preserve alpha channels |

**Warning:** Render Cache can be fragile. Magic Mask data is stored in cache. If
cache is cleared, corrupted, or the project is moved, Magic Mask may need to
re-track. This is why Render in Place or an external matte is safer.

---

## RENDER IN PLACE

**What it is:** Creates actual video files with effects baked in and replaces
the timeline clips with those files.

**Location:** Edit page -> Select clip(s) -> Right-click -> Render in Place

Render in Place creates real video files you can move to another computer, share
with collaborators, and keep as a backup. It is reversible: right-click the
rendered clip and choose **Decompose to Original** to restore the original clip
with effects still present.

| Option                        | Recommendation                 |
| ----------------------------- | ------------------------------ |
| Include Video Effects         | Yes, bakes Edit page effects   |
| Include Fusion Composition    | Yes, bakes Fusion work         |
| Include Color Grading Effects | Careful, see below             |
| Add Handles                   | Yes, 10 frames for flexibility |
| Use Source Resolution         | Yes, preserves quality         |

### The Include Color Grading Decision

- Check **Include Color Grading Effects** if you want to permanently bake your
  grade.
- Uncheck it if you want to grade on top of the rendered file and keep
  flexibility.
- For podcast Magic Mask work, usually uncheck color grading if you want to keep
  grading flexibility after baking edit/Fusion work.

### Codec Choice for Render in Place

| Platform      | Standard Work      | With Alpha Channel |
| ------------- | ------------------ | ------------------ |
| Mac           | ProRes 422 HQ      | ProRes 4444        |
| Windows/Linux | DNxHR HQX (10-bit) | DNxHR 444 (12-bit) |

---

## THE MAGIC MASK PROTECTION PROBLEM

The problem: you spend an hour tracking a Magic Mask, and the tracking data
lives in cache. If anything goes wrong with that cache, you lose the tracking
work.

| Tool            | Where Tracking Is Stored | Survives Project Close/Reopen? | Survives Cache Clear? |
| --------------- | ------------------------ | ------------------------------ | --------------------- |
| Face Refinement | Project database         | Yes                            | Yes                   |
| Magic Mask      | Cache folder             | Yes, if cache is intact        | No                    |

Face Refinement tracking is stored in the project database. Magic Mask tracking
is stored in the cache folder. If you clear render cache, move the project to
another computer, corrupt the cache, or lose the cache drive, Magic Mask may
need to re-track from scratch.

**Conclusion:** Face Refinement is relatively safe. Magic Mask needs protection
via Render in Place or an external matte export.

---

## PROTECTING MAGIC MASK: SIMPLE METHOD

Use **Render in Place** if you just want to bake everything and move on, and you
do not need independent person/background adjustment later.

Current node setup:

```
[Source] -> [Node 1: Magic Mask - person adjustments]
              |
              v
           [Node 2: background adjustments - inverted key]
```

### Steps

1. Go to the Edit page with `Shift + 4`.
2. Select the full source clip with Magic Mask applied.
3. Right-click -> Render in Place.
4. Use these settings:

```
Format: QuickTime
Codec: ProRes 422 or DNxHR HQX

Include Video Effects: checked
Include Fusion Composition: checked
Include Color Grading Effects: checked
Add Handles: 0 frames, or 10 for safety
Use Source Resolution: checked
```

5. Render to a clearly named file such as `Source_Graded_Baked.mov`.
6. When done, Resolve replaces the timeline clip with the rendered file.

The Magic Mask never needs to run again for that baked clip. You can clear cache
or move the project and the grade remains permanent.

If you need to change something later:

1. Right-click the clip -> Decompose to Original.
2. Make changes in the original node tree.
3. Render in Place again.

---

## PROTECTING MAGIC MASK: FLEXIBLE METHOD

Use an **External Matte** if you want to adjust person and background
brightness/color later without re-tracking.

Overview:

1. Export only the mask as a black-and-white video.
2. Delete or disable the Magic Mask node.
3. Use the black-and-white video as a stencil for subject/background nodes.

### Step 1: Make the Mask Visible as Black and White

Your Magic Mask creates an invisible alpha channel. Convert it into visible
black and white:

1. Select the Magic Mask node.
2. Add a new Serial Node with `Alt/Option + S`.
3. Drag the Magic Mask node's blue alpha output to the new node's green RGB
   input.
4. The viewer should show white for the person and black for the background.

If you do not see black and white, select the new node, open the Key tab, and
set **Key Output Gain** to `1.0`.

### Step 2: Disable All Color Grading

Bypass every grading node except the Magic Mask node and the new black-and-white
matte node. The viewer should show only the black-and-white mask.

### Step 3: Render the Matte Only

1. Go to the Edit page.
2. Select the clip.
3. Right-click -> Render in Place.
4. Use QuickTime with ProRes 422, DNxHR SQ, or even H.264 if the matte is only
   for black-and-white shape data.
5. Check **Include Color Grading Effects**.
6. Save as something clear, such as `Matte_Person.mov`.

### Step 4: Restore the Original Clip

1. Right-click the clip -> Decompose to Original.
2. Re-enable your original grading nodes.
3. Delete the temporary black-and-white matte output node.
4. Delete or disable the Magic Mask node if the external matte is replacing it.

### Step 5: Link the Matte to the Source Clip

1. In the Media Pool, find the original source clip.
2. Right-click -> Clip Attributes.
3. Go to the Mattes tab.
4. Click **Add Matte** or `+`.
5. Select the rendered matte file.
6. Click OK.

### Step 6: Use the Matte in Nodes

For person adjustments:

1. Create or select a subject node.
2. Open the Key tab.
3. Choose the linked matte from the Matte dropdown.
4. Adjustments now affect only the white areas.

For background adjustments:

1. Create a background node.
2. Select the same matte.
3. Click **Invert**.
4. Adjustments now affect only the black areas.

Final structure:

```
[Source] -> [Person Node + External Matte] -> [Background Node + Inverted Matte] -> [Output]
```

---

## WHICH MAGIC MASK PROTECTION METHOD SHOULD YOU USE?

| Situation                                                | Use This Method |
| -------------------------------------------------------- | --------------- |
| Just want it done                                        | Render in Place |
| Do not need separate subject/background changes later    | Render in Place |
| Deadline tomorrow                                        | Render in Place |
| Client might request separate subject/background changes | External Matte  |
| Need long-term flexibility                               | External Matte  |

For most podcast work, Render in Place is usually enough. If the client asks for
changes, decompose to original, adjust, and re-render. Use the external matte
workflow when independent subject/background control must remain available.

---

## TROUBLESHOOTING: RENDER STUCK OR SLOW

### Quick Fix: Render in Place

Use this when a render is stuck and you need a reliable result.

1. Cancel the stuck render.
2. Go to the Edit page.
3. Make sure you are on the full source timeline, not a cut timeline with many
   edits.
4. Select the clip with Magic Mask.
5. Right-click -> Render in Place.
6. Use QuickTime with ProRes 422 or DNxHR HQX.
7. Check Video Effects, Fusion Composition, Color Grading Effects, and Use
   Source Resolution.
8. Render.

After Render in Place, playback and final export should be fast because Magic
Mask computation is baked into the rendered file.

### If Render in Place Also Re-Tracks

Your cache is probably lost. Options:

1. Let it re-track, then Render in Place immediately after tracking completes.
2. If tracking is corrupted, re-track manually, then Render in Place
   immediately.

Prevention: Render in Place right after a successful Magic Mask track, before
clearing cache or moving the project.

### GPU Acceleration Check

If renders are extremely slow, check:

**DaVinci Resolve -> Preferences -> System -> Memory and GPU**

| Setting             | What to Check                                                                 |
| ------------------- | ----------------------------------------------------------------------------- |
| GPU Processing Mode | CUDA for NVIDIA, OpenCL for AMD, Metal for Mac; avoid Auto if troubleshooting |
| GPU Selection       | Make sure the dedicated GPU is selected                                       |

### Magic Mask Re-Computation During Deliver Render

Deliver page may recompute Magic Mask even if it was already tracked.

Symptoms:

- Render starts but shows no progress for 10+ minutes.
- Render is absurdly slow compared to similar projects.
- GPU usage is maxed out.

Solutions:

1. In Deliver page -> Video tab -> Advanced, check **Use render cached images**.
2. Pre-cache the timeline: Playback -> Render Cache -> Smart, then wait for the
   timeline to turn blue before rendering.
3. Render in Place first, then final export from the baked file.

### Render Stuck Diagnostic

1. Set In/Out points around 10 seconds of footage.
2. Try to render only that section.
3. If 10 seconds takes forever, the problem is likely Magic Mask computation.
4. If 10 seconds renders quickly, the problem may be a specific section of the
   timeline.

### Slow ProRes Renders

ProRes itself encodes quickly. If ProRes renders are slow:

- Magic Mask is probably being recomputed.
- GPU acceleration may not be enabled correctly.
- Cache may be on a slow drive.
- Try Playback -> Delete Render Cache -> All, then re-cache and render again.

---

## WHICH TIMELINE TO RENDER?

If you have a one-hour podcast cut into many clips, and Magic Mask was tracked
on a separate full-source timeline, render the **full source timeline**, not the
cut timeline.

Why:

1. Continuous tracking stays intact.
2. One master file is cleaner than many rendered fragments.
3. The rendered full clip can preserve source timecode, so cuts still reference
   the correct frames.

Workflow:

1. Full Source Timeline: apply Magic Mask, track, grade or export matte.
2. Cut Timeline: use the rendered source or linked external matte.

---

## THE BAKING TRAP

If you Render in Place with all effects and color grading, subject brightness
and background brightness become one video layer. You can no longer adjust them
independently.

To keep flexibility:

- Render only the matte.
- Render with alpha.
- Render in Place without color grading, then grade the rendered file.

---

## CODEC SPEED COMPARISON

H.264 and H.265 are delivery codecs: highly compressed, small, and slow to
encode. ProRes and DNxHR are intermediate/editing codecs: larger, faster, and
easier to work with.

| Codec         | Encode Speed | File Size  | Use Case             |
| ------------- | ------------ | ---------- | -------------------- |
| H.264         | Slow         | Small      | Final delivery only  |
| H.265/HEVC    | Very slow    | Smaller    | Final delivery only  |
| ProRes 422 HQ | Fast         | Large      | Intermediate/editing |
| DNxHR HQX     | Fast         | Large      | Intermediate/editing |
| ProRes 4444   | Fast         | Very large | When you need alpha  |

**Rule:** Use ProRes or DNxHR for intermediate work. Use H.264/H.265 only for
final client delivery.

---

## RENDER SETTINGS QUICK REFERENCE

### Render Cache

```
Format: DNxHR SQ, or ProRes 422 on Mac
Location: Fast SSD, not your system drive
```

### Render in Place to Protect Magic Mask

```
Format: QuickTime
Codec: DNxHR HQX, or ProRes 422 HQ on Mac
Include Video Effects: yes
Include Fusion: yes
Include Color Grading: no, unless you want to bake it
Add Handles: 10 frames
Use Source Resolution: yes
```

### Matte Export

```
Format: QuickTime or MP4
Codec: DNxHR SQ, ProRes 422, or H.264
Resolution: match source/timeline when possible
```

### Final Delivery

```
Format: MP4
Codec: H.264 or H.265
Quality: 20-35 Mbps for web, higher for broadcast
```

---

## CLOUD GPU RENDERING

For one-off projects, cloud GPU rendering is usually not worth the setup time.

It makes sense for regular high-volume work, extremely long renders, or severely
underpowered local hardware.

Simpler alternatives:

1. Render overnight.
2. Use proxies while editing, then render full resolution at the end.
3. Render an intermediate ProRes/DNxHR file, then transcode to H.264 separately.
4. Use Playback -> Timeline Proxy Mode -> Half or Quarter resolution while
   editing.

---

## FINAL TRUTH

A good grade is not noticed. A bad grade is unforgettable.

Build reality first. Then bend it gently.
