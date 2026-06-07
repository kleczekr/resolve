# Using an Exported Magic Mask Matte for Targeted Color Grading in DaVinci Resolve 20 Studio

## Introduction

Magic Mask is powerful for isolating a person or object, but it can be
GPU-intensive to use during every render. A practical workaround is to export
the Magic Mask result as a black-and-white matte clip, with the subject in white
and the background in black, then use that matte inside the Color page node
tree.

This lets you apply precise color corrections to the masked subject without
continuously re-tracking Magic Mask. The result is faster playback, faster final
rendering, and a more reliable workflow when cache is cleared or projects move
between systems.

---

## Intro

Cinema is not only recorded light; it is directed attention. Every adjustment
decides what the viewer is allowed to feel first: a face, a hand, a background
threat, a softened memory, a small change in emotional temperature. Colour can
make an image truthful without making it literal. It can protect ambiguity,
heighten intimacy, separate a body from its surroundings, or let a room swallow
someone when the story requires it.

This matte workflow is therefore not just a performance trick. It is a way of
making the act of selection durable. Once the subject is isolated cleanly, the
grade can treat person and world as separate emotional forces: skin can be held,
background can be cooled or deepened, distractions can be quieted, and the
viewer's eye can be guided without constantly rebuilding the mask. The technical
purpose is speed and reliability; the artistic purpose is control over what the
image asks us to care about.

---

## Why Replace Magic Mask with an External Matte

### Performance and Reliability

Magic Mask uses Resolve's Neural Engine to track the subject. By using a
pre-rendered matte file, you remove that AI tracking from the live grading
pipeline. The matte behaves like ordinary footage, so playback and final renders
are much faster.

An external matte is also a permanent file. It survives cache clears, project
moves, and project duplication.

### Flexibility

Using an external matte inside the color node tree gives you control similar to
Magic Mask, but without retracking:

- Reuse the matte on multiple nodes.
- Combine the matte with qualifiers or Power Windows.
- Grade the subject and background separately.
- Move or duplicate the project without losing the mask.
- Preserve the tracking work permanently.

### Quality

Resolve 20's Magic Mask 2 is more accurate than earlier versions, but complex
edges and occlusions can still be difficult. Once you have a good mask,
exporting it locks in the best version of that tracking. You can still refine
the matte inside Resolve using key controls such as blur, shrink/expand, Clip
Black, and Clip White.

---

## Exporting the Magic Mask as a Matte Clip

If you already have a white-on-black matte video, skip to the import section.
Otherwise:

1. Create the Magic Mask on your clip in the Color page.
2. Track the person or object through the clip.
3. Pipe the Magic Mask alpha output to a visible RGB output so the viewer shows
   a black-and-white matte.
4. Disable other grade nodes so the output is clean white subject on black
   background.
5. If the matte looks faint, set the node's **Key Output Gain** to `1.0`.
6. Render the matte clip using Render in Place or the Deliver page.
7. Use a high-quality intermediate format such as ProRes 422 or DNxHR when
   possible.
8. Match the matte's length, frame rate, resolution, and timecode to the
   original clip.

Tip: For edited projects, export the matte from the full source clip rather than
from a trimmed segment. If the matte carries matching source timecode, Resolve
can align it automatically with cut timeline instances.

---

## Importing the Matte Clip into Resolve

1. Import the matte video into the Media Pool.
2. Right-click the original source clip in the Media Pool.
3. Choose **Clip Attributes**.
4. Open the **Mattes** tab.
5. Click **Add Matte** or the `+` button.
6. Select the imported matte clip.
7. Click OK.

Resolve associates the matte with the source clip using timecode and frame
index. If the matte and source share matching start timecode or matching frame
ranges, they should line up automatically.

Alternative: On the Color page, open the Media Pool panel and drag the matte
clip directly into the node graph. Resolve adds it as an External Matte node.
You can then connect that matte node to a correction node's blue key input.

---

## Applying the External Matte in the Color Node Tree

There are two common workflows:

- Use the matte to mask a serial correction node.
- Use a Layer Mixer with separate subject and background branches.

---

## A. Serial Node Approach

Use this when you want the quickest setup.

### Steps

1. Go to the Color page and select the clip linked to the matte.
2. Keep your base grade first if you already have one.
3. Add a new Serial Node for the subject correction.
4. Label it clearly, such as `Subject Correction`.
5. Open the Key/Qualifier panel.
6. In the external matte dropdown, choose the linked matte.
7. Preview the mask with Highlight mode, usually `Shift + H`.
8. If the background is selected instead of the subject, invert the matte in the
   Key panel.
9. Grade the subject on this node.

To grade the background:

1. Add another serial node after the subject node.
2. Select the same external matte.
3. Invert it.
4. Grade the background on that node.

Example node order:

```
Node 1 - Primary grade
Node 2 - Subject grade with external matte
Node 3 - Background grade with inverted external matte
Output
```

This gives you separate subject and background control without any live Magic
Mask node.

---

## B. Layer Mixer Approach

Use this when you prefer a compositing-style layout or need multiple branches.

### Steps

1. Start with the source/base node.
2. Add a Layer Mixer.
3. Create one branch for the subject and one branch for the background.
4. Apply the external matte to the subject branch.
5. Apply the same matte inverted to the background branch.
6. Make sure the subject branch feeds the foreground/top input of the Layer
   Mixer.
7. Grade each branch independently.

Example:

```
             -> [Subject Node + Matte] ----
[Source] ---                              -> [Layer Mixer] -> [Output]
             -> [Background Node + Inverted Matte]
```

The final result should match the serial approach: the subject and background
are graded separately and composited back together.

---

## Resolve 20 Workflow Notes

Resolve 20 Studio introduced Magic Mask 2.0, which improves mask speed and
quality. The external matte workflow remains useful because:

- Magic Mask still does not become a permanent standalone matte automatically.
- Clearing cache can still force re-tracking.
- Moving projects can still break cache-dependent tracking.
- External matte nodes make projects lighter and more portable.
- Dragging a Media Pool clip into the Color node graph can add it as a matte.
- External matte nodes in newer Resolve versions may use the matte filename as
  the node label.

Once an external matte replaces the Magic Mask, you can delete or disable the
Magic Mask node. The matte file becomes the permanent mask source.

---

## Troubleshooting

### Matte Timing or Alignment Issues

Symptoms:

- Corrections spill outside the subject.
- The matte lags or leads the subject.
- The mask appears offset by a fixed number of frames.

Fixes:

1. Prefer Clip Attributes -> Mattes tab for linking.
2. Confirm the matte is attached to the correct source clip.
3. Check the matte file's start timecode in Clip Attributes -> General.
4. If needed, set the matte timecode to match the source clip or the source
   segment it represents.
5. If using an External Matte node, adjust the **External Matte Offset**
   parameter in frames.
6. Re-export the matte from the full source clip if trimmed matte alignment
   remains unreliable.

### Resolution or Aspect Ratio Mismatch

Symptoms:

- Matte is scaled incorrectly.
- Matte is shifted.
- Edges look soft or blocky.

Fixes:

1. Export the matte at the same resolution and aspect ratio as the source or
   timeline.
2. Check image scaling in Clip Attributes.
3. Use the matte node's Sizing controls only for minor corrections.
4. Re-render at full resolution for critical work.

### Improper Alpha or Matte Behavior

Symptoms:

- Subject appears transparent.
- Background and subject corrections overlap.
- Edges show dark fringes or halos.

Fixes:

- Check inversion: subject nodes usually use the normal matte; background nodes
  use the inverted matte.
- Check Layer Mixer order: the subject branch should act as the foreground/top
  layer.
- If using a file with an actual alpha channel, check Clip Attributes -> Alpha
  Mode and test Straight vs Premultiplied.
- If the matte is not fully opaque, raise Key Output Gain to `1.0`.
- Use Clip Black/White or In/Out controls to tighten the matte.
- Add a small blur to soften harsh edges.
- Expand or contract the matte slightly to hide edge halos.

### Multiple Mattes

Resolve can use multiple mattes per clip, usually listed as Matte 1, Matte 2,
and so on. Keep nodes named clearly. If a matte does not appear to work, verify
that the node is using the correct matte from the dropdown.

If you need to combine mattes, use a Key Mixer before feeding the result into
the correction node.

### Matte Not Updating

If you replace the matte file with a new version and Resolve still shows the old
result:

1. Clear cache for the clip.
2. Remove and re-add the matte.
3. Close and reopen the project if necessary.

---

## Practical Final Workflow

1. Track Magic Mask on the full source clip.
2. Export a white-on-black matte with matching resolution, frame rate, duration,
   and timecode.
3. Import the matte into Resolve.
4. Attach it to the original source clip through Clip Attributes -> Mattes.
5. Use the matte on subject nodes.
6. Use the inverted matte on background nodes.
7. Delete or disable the Magic Mask node.
8. Render normally without retracking.

This workflow preserves the Magic Mask result as a permanent matte file while
keeping subject and background grades fully editable.

---

## Sources Mentioned in the PDF

- Blackmagic Design Forum: Magic Mask to External Matte Workflow
- Creative Video Tips: Save Magic Mask 2 in Resolve 20
- DaVinci Resolve user guide material on external matte setup and usage
- Creative COW discussions on external matte sync
- Blackmagic Design DaVinci Resolve 20 release notes
