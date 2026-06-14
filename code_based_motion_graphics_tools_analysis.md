# Code-Based Motion Graphics Systems: In-Depth Pros / Cons Analysis

**Date:** 2026-06-13  
**Audience:** programmer / data-viz person / Resolve-Fusion user / software-product video creator  
**Core question:** Which code-capable motion graphics systems are worth learning, and for what kind of work?

---

## 0. Executive verdict

If the goal is **professional, repeatable motion graphics for software-product videos, LinkedIn reels, vertical demos, explainer overlays, and data-driven variants**, the strongest practical stack is:

1. **Remotion** — main video-generation engine for React/JS/TS people.
2. **Motion Canvas** — best code-first system for clean technical explainers and diagram animation.
3. **Blender Python** — best for procedural 3D, cinematic 3D elements, VFX-ish assets, simulated worlds, particles, camera moves.
4. **D3 + SVG/Canvas**, often inside Remotion — best for custom animated data visualizations.
5. **GSAP**, often inside web capture or Remotion-ish workflows — excellent animation grammar for DOM/SVG/canvas, but not primarily a video renderer.
6. **Fusion scripting/macros** — useful inside DaVinci Resolve, but not the nicest primary code-generation environment.
7. **After Effects expressions/scripts** — powerful and industry-standard, but only worth adopting if client/workflow pressure demands Adobe.
8. **Manim** — excellent for mathematical/STEM precision; less ideal for modern product-marketing motion unless deliberately stylized.
9. **Three.js / React Three Fiber** — great for browser-native 3D and interactive visuals; video export is the awkward part.
10. **Lottie / dotLottie** — excellent for UI micro-animations; weak as a full video/motion-graphics production system.
11. **p5.js / Processing** — excellent creative-coding sketchbooks; weaker for polished, reusable client video pipelines.
12. **AI vector animation systems** — promising research, not production-core yet.

The blunt recommendation: **learn Remotion first**, then **Motion Canvas**, then **Blender Python**. Use **D3/GSAP/Three.js** as ingredients. Use **Fusion** when the deliverable must live inside Resolve. Avoid making After Effects your home unless there is money attached to that requirement.

---

## 1. Evaluation criteria

A code-based motion graphics system is not automatically good just because it uses code. The important axes are:

| Criterion | Why it matters |
|---|---|
| **Renderability** | Can it produce reliable MP4/ProRes/image-sequence output, or do you need screen recording hacks? |
| **Timeline control** | Can you precisely define frame, duration, easing, sequencing, and synchronization? |
| **Composability** | Can you build reusable components/templates? |
| **Design quality ceiling** | Can it make work that feels like professional motion design, not a coding demo? |
| **Iteration speed** | Can you preview quickly and adjust timing without pain? |
| **Data-driven generation** | Can you create 10–100 variants from data or JSON? |
| **Integration with existing workflow** | Does it play nicely with Resolve, Fusion, Blender, web assets, Figma/SVG, etc.? |
| **Learning curve** | Is the difficulty mostly useful, or mostly API misery? |
| **Stability / maintenance** | Is the tool actively maintained and documented? |
| **Expressive range** | 2D graphics? typography? data-viz? 3D? particles? UI? VFX? |

---

## 2. System categories

### Category A — Code-native video engines

These are the most relevant if you want to generate final video from code.

- Remotion
- Motion Canvas
- Manim

### Category B — Pro-app scripting systems

These extend large motion/VFX/compositing tools.

- Blender Python
- DaVinci Resolve / Fusion scripting
- After Effects scripting and expressions

### Category C — Browser / creative-coding systems

These are excellent for visuals, but often need another layer for video export.

- GSAP
- D3
- p5.js
- Processing
- Three.js
- React Three Fiber

### Category D — Vector animation / app animation formats

These are more for micro-animation and app/web delivery than rendered social video.

- Lottie / dotLottie
- Bodymovin / LottieFiles / lottie-web
- SVG animation tools and libraries

### Category E — AI/code-assisted emerging systems

Promising, but still immature for reliable client work.

- LottieGPT
- OmniLottie
- LiveSVG-style systems
- LLM-assisted Manim / SVG / Remotion code generation

---

# 3. Remotion

**Official positioning:** Remotion lets you create real MP4 videos with React, code, and data-driven parameterization.  
**Language:** JavaScript / TypeScript / React  
**Best for:** product videos, social variants, animated UI mockups, data-driven video, generated reels, marketing explainers, programmatic exports.

## 3.1 What it is

Remotion treats video as a React application. A video is composed of React components, CSS, SVG, Canvas, audio, images, browser-rendered layers, and timed sequences. Instead of exporting a website, you render frames into a video.

That sounds simple, but it is a big deal: you get the entire frontend ecosystem as a motion graphics system.

You can build:

- kinetic typography
- UI walkthrough videos
- animated overlays on top of screen recordings
- charts and dashboards
- dynamic product/social variants
- intro/outro packs
- batch-rendered videos from JSON/CSV/API data
- synthetic “software demo” scenes without recording a browser manually

## 3.2 Pros

### 1. Best fit for a JavaScript developer

If you already know JS, CSS, SVG, React-ish component thinking, Remotion is immediately more legible than Fusion scripting or After Effects scripting.

You can use:

- React components
- CSS layout
- SVG
- Canvas
- WebGL
- D3
- GSAP-ish ideas
- JSON data
- npm packages
- local assets
- font loading
- programmatic composition

This is extremely powerful for software-product motion graphics.

### 2. Real video output

Unlike normal browser animation, Remotion is designed to render video files. That removes the classic creative-coding problem: “Cool sketch, but now how do I get a clean MP4 at 1080x1920 and 30 fps without stutter?”

For actual deliverables, this matters a lot.

### 3. Data-driven rendering is first-class

Remotion is strong when a video is not one handcrafted object but a template:

- 20 LinkedIn variants
- one reel per feature
- one video per product
- one chart animation per dataset
- one case-study animation per client
- localized versions
- different aspect ratios

This is where code beats GUI motion design brutally.

### 4. Great for vertical software videos

For your likely use case — screen recording + animated titles + highlighted UI areas + clean captions + callouts — Remotion is probably the most efficient tool in the list.

A strong workflow:

1. Record clean vertical or cropped software footage.
2. Put the footage into Remotion.
3. Add timed overlays, text, arrows, zoom panels, captions, cursor highlights.
4. Render multiple versions.
5. Do final sound/grade/tiny tweaks in Resolve.

### 5. Version control is natural

Because everything is code, Git works well:

- template changes are reviewable
- old versions are recoverable
- variations are branchable
- reusable components are packageable
- generated videos can be recreated later

This is a huge advantage over manually keyframed motion projects where the “source of truth” is a binary project file.

### 6. Can absorb other web animation tools

D3 charts, SVG systems, CSS animation, Canvas drawings, Three.js/R3F, and custom JS math can all be part of a Remotion project.

Remotion can become the “rendering shell” around many other code-driven visual systems.

## 3.3 Cons

### 1. React is not a motion design language by itself

React is a UI composition model. Motion design still requires you to design timing, rhythm, easing, layout, hierarchy, and visual grammar.

Bad Remotion work can easily look like “website components sliding around.”

The tool will not save you from ugly animation.

### 2. Complex 3D and VFX are not its home

You can use WebGL/Three.js, but Remotion is not Blender. For physically rich 3D, particles, fluid, lighting, materials, tracking, or realistic compositing, Blender/Fusion/Resolve are better.

### 3. Browser rendering quirks can leak into the pipeline

Fonts, subpixel layout, GPU differences, media decoding, browser APIs, and package compatibility can become annoying. It is still web tech under the hood.

### 4. Long-form editorial is not the point

Remotion can render long videos, but it is not an NLE. Do not use it to edit a film, repair audio, grade footage, manage takes, or do complex editorial decisions.

Use Resolve/Avid for that.

### 5. Design tooling is weaker than After Effects

You do not get AE-style visual editing, graph editor culture, plugin ecosystem, or motion-designer muscle memory. You are writing code. That is a feature for some people and a deal-breaker for others.

## 3.4 Best use cases

- Product feature videos
- LinkedIn/Instagram vertical demos
- Automated video variants
- Programmatic lower thirds
- Animated dashboards
- “Launch announcement” videos
- SaaS explainer clips
- Generated social cards/videos
- Screen recording enhancement

## 3.5 Bad use cases

- Realistic 3D/VFX
- Fluid simulation
- hand-animated character animation
- complex cinematic compositing
- work where a motion designer expects AE
- timeline-heavy editorial work

## 3.6 Overall judgment

**Best first pick for a JS developer who wants code-based motion graphics that actually render as video.**

For software-product motion graphics, Remotion is the strongest practical answer.

---

# 4. Motion Canvas

**Official/project positioning:** TypeScript library plus editor for creating animated videos, especially informative vector animations and voice-over synchronization.  
**Language:** TypeScript  
**Best for:** technical explainers, diagrams, educational animations, clean vector motion, UI/architecture visualizations.

## 4.1 What it is

Motion Canvas is a code-first animation system built around TypeScript, generators, scenes, and a preview/editor environment. It is more explicitly built as an animation authoring tool than a general React video renderer.

If Remotion feels like “React app rendered as video,” Motion Canvas feels more like “programmatic motion graphics storyboard.”

## 4.2 Pros

### 1. Excellent mental model for explainers

Motion Canvas is very good when you need to animate:

- boxes
- arrows
- lines
- graphs
- callouts
- flow diagrams
- architecture diagrams
- code concepts
- UI abstractions
- voiceover-synced educational graphics

This makes it especially good for software explainers.

### 2. Better animation authoring feel than raw React

The generator-based sequencing model is nice for motion work. You can describe temporal flow in a readable way:

- show this
- wait
- move that
- fade this
- transform these together
- synchronize with narration

This can be more natural than wiring everything through frame numbers manually.

### 3. Real-time preview/editor is a big advantage

Code-only animation can become painful if every tweak requires rendering. Motion Canvas’s editor/preview environment helps bridge that gap.

### 4. Strong for precision

For crisp explainers, you often need exact spacing, predictable layout, and deterministic motion. Motion Canvas is strong here.

### 5. TypeScript ecosystem

You get type safety, modern JS tooling, npm, reusable abstractions, and editor help.

## 4.3 Cons

### 1. Smaller ecosystem than Remotion

Remotion has broader mindshare around video generation. Motion Canvas is more specialized and less likely to have every answer already available.

### 2. Less natural for media-heavy videos

If your project is mostly edited footage, screen recordings, music beds, voiceover, captions, and social-format video variants, Remotion may be the more natural wrapper.

Motion Canvas is excellent for generated visuals, but less obviously the center of a whole video production pipeline.

### 3. Can look “explainer-ish”

This is not inherently bad. But its natural aesthetic leans toward educational/technical/vector clarity. For glossy product marketing, you may need extra design work.

### 4. Not a full compositing/VFX tool

No substitute for Fusion/Blender when you need advanced compositing, tracking, 3D integration, masks, color pipelines, or realistic effects.

## 4.4 Best use cases

- Technical explainer videos
- Software architecture visuals
- Algorithm demos
- Animated diagrams
- Voiceover-synced educational clips
- Product “how it works” sequences
- Clean vector UI abstractions

## 4.5 Bad use cases

- cinematic VFX
- heavy footage editing
- fully designed ad films with organic 3D
- client pipelines expecting AE templates
- rich particle/physics work

## 4.6 Overall judgment

**Best code-first explainer animation tool.**

For a programmer making clean technical motion graphics, Motion Canvas is beautiful. I would learn it after or alongside Remotion.

---

# 5. Manim

**Official positioning:** Python library for creating mathematical animations; community edition focuses on programmatic precision.  
**Language:** Python  
**Best for:** mathematical, algorithmic, STEM, pedagogical, precise visual explanation.

## 5.1 What it is

Manim is the famous Python animation engine associated with 3Blue1Brown-style explanations. It excels at mathematically precise object transformations, graphs, formulas, coordinate systems, geometric relationships, and explanatory animation.

## 5.2 Pros

### 1. Exceptional for abstract precision

If the animation is about a concept rather than a product, Manim can be superb:

- algorithms
- graphs
- geometry
- equations
- vectors
- transformations
- computational concepts
- data structures
- mathematical metaphors

### 2. Python-native

For data analysis and algorithmic work, Python is a comfortable environment. You can generate animation directly from computed values.

### 3. Deterministic and scriptable

Manim is excellent for reproducible educational visuals. You define a scene, it renders exactly from code.

### 4. Strong community around explainers

There are many examples and a recognizable educational style. If you want to explain technical concepts, that community is valuable.

## 5.3 Cons

### 1. The house style is strong

Manim often looks like Manim. That can be good for math education, but it may not suit modern software-product marketing unless you heavily customize visual design.

### 2. Less suitable for UI/product promos

For a Shopify app promo, SaaS dashboard reel, or LinkedIn software demo, Manim is not the natural first choice. Remotion or Motion Canvas will feel more aligned.

### 3. Typography and design polish require effort

You can make beautiful things, but a lot of the beauty comes from disciplined design work. Out of the box, it is more about clarity than brand-polished motion graphics.

### 4. Media workflow is not its main strength

It is not a video editor, compositor, or rich media sequencer. It is a scene-rendering engine.

## 5.4 Best use cases

- Math videos
- DSA explanations
- graph algorithms
- technical education
- research explanation
- physics/geometry visuals
- programmatic diagrams derived from calculations

## 5.5 Bad use cases

- software product ads
- kinetic typography promos
- client brand motion kits
- footage-heavy reels
- Resolve/Fusion compositing workflows

## 5.6 Overall judgment

**Great for technical pedagogy, not the best general-purpose motion graphics system for product video.**

Learn it if you want to explain algorithms, data structures, math, or systems. Do not pick it as your primary LinkedIn-product-video engine.

---

# 6. Blender Python

**Official positioning:** Blender exposes a Python API for scripting and automation inside the full 3D creation suite.  
**Language:** Python  
**Best for:** procedural 3D, generated scenes, 3D typography, particles, camera motion, simulated elements, cinematic assets.

## 6.1 What it is

Blender is a complete 3D package. Blender Python lets you automate scene creation, animation, materials, lighting, cameras, geometry nodes-ish workflows, render settings, imports/exports, and batch generation.

It is not merely a motion graphics library. It is a full 3D production environment that happens to be scriptable.

## 6.2 Pros

### 1. Massive expressive ceiling

Blender can do things web/video libraries cannot touch properly:

- real 3D objects
- physically based materials
- volumetrics
- particles
- rigid/soft body sims
- procedural geometry
- lighting
- camera moves
- 3D typography
- realistic shadows/reflections
- abstract product worlds
- VFX elements

For “more 3D blood,” procedural objects, blood splats, tracked 3D marks, or cinematic generated elements, Blender is the serious answer.

### 2. Python automation is powerful

You can script:

- object creation
- animation curves
- camera paths
- material variants
- render batches
- procedural layouts
- asset generation
- import/export pipelines

This enables repeatable 3D motion systems, not just one-off manual Blender files.

### 3. Professional render outputs

You can render image sequences, transparent EXRs/PNGs, multilayer passes, depth, normals, masks, etc., then composite in Resolve/Fusion/Nuke/After Effects.

For serious post-production, image sequences are often better than direct MP4.

### 4. Works well with Resolve/Fusion

A practical pipeline:

1. Track/stabilize/matchmove in Resolve/Fusion/Blender as needed.
2. Build 3D asset/effect in Blender.
3. Render transparent image sequence or EXR passes.
4. Composite in Fusion/Resolve.
5. Grade with the shot.

### 5. Excellent for procedural visual identity

Blender can generate rich branded scenes:

- abstract glass objects
- Shopify/product cards in 3D
- animated packaging
- floating dashboards
- device mockups
- isometric worlds
- kinetic 3D text

## 6.3 Cons

### 1. Learning curve is brutal if you are new to 3D

Blender has many separate learning curves:

- interface
- modeling
- materials
- lighting
- animation
- cameras
- rendering
- compositing
- simulations
- Python API
- color management

Using Python does not remove the need to understand 3D. It can actually make beginner confusion worse.

### 2. Scripting API can be verbose and stateful

Blender scripting often involves manipulating scene state, object references, contexts, collections, modifiers, and data blocks. It is not as clean as a small animation library.

### 3. Slower iteration than 2D systems

Rendering 3D is heavier. You may wait on previews, render tests, lighting tweaks, and material changes.

### 4. Overkill for simple motion graphics

Do not use Blender Python for simple lower thirds, animated charts, or SaaS text overlays unless you specifically want 3D.

### 5. Compositing realism is hard

For VFX, the hard part is not “make an object.” The hard part is making it match the plate:

- lighting direction
- lens distortion
- motion blur
- grain
- color temperature
- focus/depth of field
- camera movement
- contact shadows
- occlusion
- tracking accuracy

Blender gives the tools, not the automatic result.

## 6.4 Best use cases

- 3D blood/effects/assets
- procedural 3D intros
- product/device mockups
- 3D typography
- simulated elements
- abstract cinematic background loops
- scene variants from data
- logo worlds

## 6.5 Bad use cases

- simple text overlays
- 2D social captions
- dashboards/charts
- most UI animation
- quick-turnaround product reels unless you already have assets/templates

## 6.6 Overall judgment

**Best code-capable 3D option by far.**

Do not use it for everything. Use it when the shot or animation truly needs 3D.

---

# 7. DaVinci Resolve / Fusion scripting and macros

**Official/API positioning:** Fusion scripting exposes FusionScript via Lua and Python.  
**Language:** Lua / Python  
**Best for:** Resolve-native compositing automation, generated Fusion comps, reusable macros, lower thirds, batch setup, pipeline glue.

## 7.1 What it is

Fusion is Resolve’s node-based compositing/motion graphics environment. It can be scripted and macro-ized. This means you can automate comp creation, tool parameters, render jobs, templates, text elements, and repetitive Fusion operations.

## 7.2 Pros

### 1. Native to your Resolve workflow

This is the big one. If the final edit, grade, sound, and delivery are in Resolve, Fusion is right there.

No roundtrip is needed for many overlays and compositing jobs.

### 2. Excellent for shot-based compositing

Fusion is much better than Remotion/Motion Canvas for:

- masks
- tracking
- planar-ish compositing workflows
- merges
- keying
- rotoscoping
- glow/blur/distortion
- particles
- grading-adjacent comp work
- shot-specific fixes

### 3. Macros can become reusable client tools

A well-built Fusion macro can become a reusable lower-third/callout/title generator. You can expose parameters in Resolve’s inspector and reuse them across edits.

This is valuable if you repeatedly make the same visual package.

### 4. Good for combining code and manual compositing

You can generate a starting comp via script, then tweak visually. That hybrid model is often practical.

## 7.3 Cons

### 1. Developer experience is rougher

Compared with Remotion or Motion Canvas, Fusion scripting is not pleasant. Documentation is thinner, examples are more scattered, and the API feels like production software internals rather than a friendly creative-coding environment.

### 2. Not ideal for large code-first projects

You can script Fusion, but building a whole motion graphics system there is harder than in a modern TypeScript environment.

### 3. Harder to version and review

Fusion comps/macros are more text-like than some binary formats, but the workflow is still not as clean as a normal code repository with reusable components.

### 4. Less web/data ecosystem

You do not get the npm universe, React components, D3, browser layout, or easy JSON-driven template rendering in the same way.

### 5. Fusion motion graphics can become node spaghetti

For complex generated animation, node graphs can become unpleasant to reason about. Great for compositing; less great as a general programming surface.

## 7.4 Best use cases

- Resolve-native lower thirds
- reusable title macros
- shot-specific comp fixes
- tracking-based overlays
- screen-recording callouts inside Resolve
- batch-generated Fusion comps
- templates for repeated edits

## 7.5 Bad use cases

- large data-driven video generation
- many social variants from JSON
- heavy UI-like layout
- modern web-style animation systems
- pleasant code-first animation authoring

## 7.6 Overall judgment

**Great as Resolve-native glue and compositing automation. Weak as your main code-first motion graphics environment.**

Use Fusion when the work belongs inside Resolve. Use Remotion/Motion Canvas when the work belongs in code.

---

# 8. After Effects scripting and expressions

**Official positioning:** After Effects supports scripts for automating operations and expressions using a JavaScript engine for property animation.  
**Language:** JavaScript engine for expressions; ExtendScript / scripting ecosystem for automation  
**Best for:** industry-standard motion graphics automation, template manipulation, AE-native pipelines, client handoff.

## 8.1 What it is

After Effects is still the dominant traditional motion graphics application. Code enters AE mainly through:

- **Expressions**: code attached to properties, often for procedural animation.
- **Scripts**: automation of project/layer/composition operations.
- **Extensions/panels/plugins**: deeper UI and workflow tools.

## 8.2 Pros

### 1. Industry standard

If a client, agency, or collaborator says “send the AE project,” that usually means After Effects. No code-native tool replaces that expectation.

### 2. Mature motion-design ecosystem

AE has:

- huge tutorial ecosystem
- plugin ecosystem
- templates
- presets
- designers who know it
- agency workflows
- expressions culture
- handoff norms

### 3. Expressions are powerful for procedural animation

Expressions are excellent for:

- looping motion
- linking properties
- responsive rigs
- automatic delays
- procedural wiggle/noise
- text animation setups
- parametric lower thirds
- data-ish controls

### 4. Good hybrid of manual design + code

AE’s strength is not pure code generation. It is manual art direction plus small pieces of code to make rigs flexible.

That is often how professional motion design actually works.

## 8.3 Cons

### 1. Adobe dependency

Subscription, platform assumptions, app weight, project-file management, plugin licensing, and general Adobe ecosystem lock-in.

If you are trying to stay Resolve/Linux-centric, AE is not a happy default home.

### 2. Scripting environment can feel archaic

AE scripting has history. Some parts feel old, inconsistent, or under-documented compared with modern web tooling.

### 3. Less natural for software/data generation than Remotion

For 100 generated videos from structured data, Remotion is usually cleaner. AE can do data-driven templates, but the developer ergonomics are worse.

### 4. Version control is ugly

AE project files are not pleasant Git artifacts. You can version scripts and expressions, but not the whole creative state cleanly.

### 5. Linux problem

AE is not a Linux-native workflow. For a Rocky Linux / Resolve user, that alone is a major practical strike.

## 8.4 Best use cases

- agency motion graphics
- client AE template delivery
- existing AE project automation
- expression-rigged typography
- template customization
- MOGRT-style ecosystems

## 8.5 Bad use cases

- Linux-native workflow
- programmer-first video generation
- scalable data-driven rendering
- Resolve-centered post pipeline unless roundtripping is required

## 8.6 Overall judgment

**Powerful, employable, and standard — but not the best personal home base for a code-first Resolve/Linux workflow.**

Learn enough to understand it if the market demands it. Do not choose it as your primary system unless clients pay for that choice.

---

# 9. GSAP

**Official positioning:** JavaScript animation platform that can animate anything JavaScript can touch.  
**Language:** JavaScript / TypeScript  
**Best for:** web motion, SVG/DOM animation, UI animation, scroll animation, kinetic web experiences.

## 9.1 What it is

GSAP is a mature animation library for JavaScript. It is not a video renderer. It animates values over time: DOM elements, SVG, Canvas-backed objects, WebGL properties, generic JS objects, React/Vue elements, and more.

## 9.2 Pros

### 1. Excellent animation grammar

GSAP timelines are very good. You can sequence complex motion clearly:

- start this at 0.2s
- overlap this by 0.1s
- stagger these elements
- ease this differently
- reverse / scrub / loop
- coordinate multiple objects

This is one of the best APIs for “motion as timed choreography.”

### 2. Strong SVG and DOM animation

For animated logos, UI callouts, product pages, animated diagrams, and web-based motion, GSAP is superb.

### 3. Mature and battle-tested

GSAP has been used in serious production web work for years. The docs, examples, and community are strong.

### 4. Can integrate with React, D3, Three.js, Canvas

GSAP does not care much what it animates. This makes it a flexible ingredient.

### 5. Great for interactive demos

If the output is a web page or interactive visual, GSAP is often better than a video-only system.

## 9.3 Cons

### 1. Not a video renderer

The big drawback: GSAP itself does not solve final video output. You usually need:

- screen recording
- headless browser capture
- Puppeteer frame capture
- Remotion integration
- custom export logic

That can be fine, but it is extra pipeline work.

### 2. Browser timing is not the same as frame-accurate video timing

For web animation, requestAnimationFrame is fine. For video rendering, you often want deterministic frame-by-frame output. Remotion is better at this.

### 3. Can become imperative animation spaghetti

GSAP timelines are clear until they are not. Large projects need disciplined structure.

### 4. Licensing/plugin considerations

GSAP has open and paid/plugin sides. For some professional workflows, licensing details matter. Check current license/plugin status before building commercial dependencies around specific plugins.

## 9.4 Best use cases

- animated websites
- product landing pages
- SVG motion
- UI transitions
- interactive charts
- scroll-based demos
- web-captured promo scenes
- animation ingredient inside Remotion-like workflows

## 9.5 Bad use cases

- standalone video pipeline
- cinematic VFX
- 3D production
- heavy editorial
- deterministic batch video rendering without another tool

## 9.6 Overall judgment

**Brilliant animation library, not a complete video production system.**

Use it as an ingredient, especially for SVG/UI/web motion. Pair it with Remotion or a capture pipeline when video output matters.

---

# 10. D3

**Official positioning:** JavaScript library for bespoke data visualization with high flexibility.  
**Language:** JavaScript / TypeScript  
**Best for:** custom data visualization, charts, data-driven visual narratives, animated graphics from datasets.

## 10.1 What it is

D3 is not “a chart library.” It is a low-level data-to-visual-elements toolkit. It binds data to SVG/Canvas/HTML and gives you scales, axes, layouts, transitions, shapes, geographic projections, hierarchy tools, force simulations, and more.

## 10.2 Pros

### 1. Best-in-class custom data visualization

If the graphic is data-driven and needs to be custom, D3 is still one of the strongest tools in existence.

It is ideal for:

- animated charts
- custom dashboards
- maps
- networks
- Sankey/alluvial diagrams
- force layouts
- small multiples
- transitions between chart states
- scroll/data narratives

### 2. Perfect fit for your existing skills

If you already have D3 history, this is low-hanging power. Combining D3 with Remotion is a very strong move.

### 3. Excellent for generated visuals

D3 lets the data drive layout, scale, shape, and animation. That is exactly where code-based motion beats manual keyframes.

### 4. Fine control

You can build graphics that generic chart libraries cannot express cleanly.

## 10.3 Cons

### 1. D3 is not a motion graphics system by itself

D3 can animate transitions, but it does not give you a full video timeline, audio sync, export pipeline, cinematic layout, or reusable social video structure.

### 2. Steep learning curve

D3’s power comes from being low-level. That means more decisions and more code.

### 3. Native D3 transitions may conflict with deterministic rendering

If you use D3’s internal transition timing inside a video renderer, you may fight frame determinism. In Remotion, it is often cleaner to use D3 for geometry/scales/layout and Remotion for time.

### 4. Design burden is on you

D3 makes accurate visuals possible. It does not automatically make them beautiful, legible, or emotionally effective.

## 10.4 Best use cases

- animated data stories
- dashboard/product metrics videos
- custom charts in Remotion
- technical analytics explainers
- maps/networks/hierarchies
- data-driven product demos

## 10.5 Bad use cases

- non-data motion graphics
- cinematic 3D
- typography-first promos
- app UI micro-interactions without data
- final video export by itself

## 10.6 Overall judgment

**Use D3 as the data-visualization engine inside a broader video system.**

For you, the killer combo is likely **D3 + Remotion**.

---

# 11. p5.js

**Official positioning:** Friendly open-source JavaScript library for creative coding and making art.  
**Language:** JavaScript  
**Best for:** generative art, sketches, abstract visuals, learning, procedural 2D animation.

## 11.1 What it is

p5.js is a browser-based creative-coding library inspired by Processing. It gives you a simple canvas drawing loop and approachable APIs for visual experiments.

## 11.2 Pros

### 1. Fast sketching

p5.js is excellent when you want to quickly make:

- particles
- generative patterns
- abstract backgrounds
- reactive visuals
- noise fields
- procedural textures
- playful animations

### 2. Friendly API

The mental model is simple:

- setup
- draw
- frame loop
- shapes
- colors
- input
- media

This makes it much less intimidating than raw Canvas/WebGL.

### 3. Great creative sandbox

If you want to explore visual ideas without a heavy production pipeline, p5.js is lovely.

### 4. Browser ecosystem

Easy to embed, share, and run anywhere a browser runs.

## 11.3 Cons

### 1. Not a professional video pipeline by itself

Like GSAP, p5.js does not automatically solve deterministic high-quality video export. You need capture or integration.

### 2. Can look sketchy

p5.js work often has a “creative coding sketch” feel. That is sometimes beautiful, sometimes amateurish. Polish requires discipline.

### 3. Weak for structured product motion

For UI overlays, software callouts, data-viz, and typography systems, Remotion/Motion Canvas/D3 are usually better.

### 4. Scaling large projects is awkward

p5.js is great for sketches. For componentized, maintainable motion systems, it is less ideal.

## 11.4 Best use cases

- generative backgrounds
- abstract loops
- art experiments
- procedural textures
- music-reactive visuals
- quick prototypes

## 11.5 Bad use cases

- repeatable client video templates
- UI-heavy product videos
- precise explainer diagrams
- serious 3D
- final export pipeline without extra tooling

## 11.6 Overall judgment

**Great sketchbook, weak production core.**

Use it to generate visual ingredients. Do not build your whole professional motion pipeline around it unless your style is deliberately generative/experimental.

---

# 12. Processing

**Official positioning:** Flexible software sketchbook and language for learning code in visual arts.  
**Language:** Processing/Java-style environment; related modes/ecosystem  
**Best for:** creative coding, education, installations, generative art, visual experiments.

## 12.1 What it is

Processing is the older, highly influential creative-coding environment that inspired p5.js. It is historically important and still useful, especially in art/education/installations.

## 12.2 Pros

### 1. Strong creative-coding heritage

Processing shaped a huge amount of modern creative coding. The conceptual model is excellent for artists learning code.

### 2. Good for installations and experiments

It can be useful for:

- generative art
- projections
- interactive installations
- sensor-driven visuals
- educational coding

### 3. Mature community and examples

There is a long archive of Processing examples and patterns.

## 12.3 Cons

### 1. Less aligned with your current stack

For a JS/Python/Resolve workflow, p5.js, Remotion, Motion Canvas, and Blender Python are more naturally aligned.

### 2. Not primarily a modern video-generation tool

Again: sketching and interactive visual output are not the same as clean social video rendering.

### 3. Ecosystem feels less central today

Processing is still meaningful, but for web-native work, p5.js/Canvas/WebGL/Three.js are often more directly useful.

## 12.4 Best use cases

- creative coding education
- installation art
- generative sketches
- art-school style visual systems
- legacy Processing projects

## 12.5 Bad use cases

- modern web/product video pipeline
- React/JS production work
- data-driven social video rendering
- Resolve-centered workflow

## 12.6 Overall judgment

**Historically important and still useful, but not where I would invest first for your practical motion-graphics needs.**

---

# 13. Three.js

**Official positioning:** JavaScript 3D library for the web.  
**Language:** JavaScript / TypeScript  
**Best for:** browser 3D, interactive 3D, lightweight product worlds, WebGL visuals, abstract 3D motion.

## 13.1 What it is

Three.js is the main high-level JavaScript library for WebGL 3D. It lets you create scenes, cameras, lights, materials, meshes, shaders, animations, and interactions in the browser.

## 13.2 Pros

### 1. Best web-native 3D ecosystem

For browser 3D, Three.js is the standard practical choice.

### 2. Strong for interactive product experiences

Three.js shines when the output is interactive:

- product configurators
- 3D landing pages
- scroll-driven 3D
- data worlds
- visual demos
- shader toys
- 3D web portfolios

### 3. Can produce beautiful abstract visuals

With good art direction, Three.js can make slick modern tech visuals:

- particles
- glassy objects
- morphing shapes
- floating UI panels
- 3D text
- procedural backgrounds

### 4. Pairs with Remotion in some workflows

You can use Three.js scenes as video ingredients. This is attractive if you want web-style 3D in generated videos.

## 13.3 Cons

### 1. Not a full 3D production package

Three.js is not Blender. It lacks the full asset creation, simulation, sculpting, rigging, rendering, and compositing environment.

### 2. Video export is not the core mission

Like GSAP and p5.js, Three.js is designed for runtime rendering in a browser, not necessarily deterministic offline video production.

### 3. 3D is hard

Even in JavaScript, you still need to understand:

- cameras
- lights
- materials
- geometry
- shaders
- performance
- coordinate systems
- animation loops

### 4. Performance constraints

Browser/GPU performance matters, especially for high-res rendering or complex scenes.

## 13.4 Best use cases

- interactive 3D websites
- lightweight product 3D
- abstract tech visuals
- shader/particle backgrounds
- browser-native demos
- 3D ingredients for Remotion-style videos

## 13.5 Bad use cases

- realistic cinematic VFX
- production 3D simulation
- full film compositing
- anything needing Blender-level asset work
- simple 2D text/caption overlays

## 13.6 Overall judgment

**Excellent for web-native 3D, less ideal as a standalone video-production system.**

Use it when the desired aesthetic is browser-tech 3D. Use Blender when the desired aesthetic is cinematic/physical 3D.

---

# 14. React Three Fiber

**Official positioning:** React renderer for Three.js; lets you build Three.js scenes declaratively using reusable React components.  
**Language:** JavaScript / TypeScript / React  
**Best for:** React-based 3D scenes, componentized 3D UI, interactive product visuals, Three.js inside React ecosystems.

## 14.1 What it is

React Three Fiber is not a replacement for Three.js. It is a React renderer for Three.js. Instead of imperative Three.js scene setup, you describe 3D scenes declaratively as React components.

## 14.2 Pros

### 1. Excellent if you think in React

If your mental model is components, props, state, hooks, and composition, R3F makes Three.js more natural.

### 2. Reusable 3D components

You can build componentized 3D systems:

- `<PhoneMockup />`
- `<FloatingCard />`
- `<MetricOrb />`
- `<AnimatedLogo />`
- `<ProductScene variant="dark" />`

That is powerful for product-video generation.

### 3. Works well with modern React tooling

It can integrate with routing, state management, design systems, controls, and other React libraries.

### 4. Strong companion libraries

The R3F ecosystem has useful helpers for controls, staging, postprocessing, Drei-style abstractions, etc.

## 14.3 Cons

### 1. Adds abstraction on top of already-hard 3D

If something breaks, you may need to understand React, R3F, Three.js, and WebGL concepts.

### 2. Version compatibility matters

R3F versions pair with React versions. This is manageable, but it is one more dependency constraint.

### 3. Still not Blender

R3F helps render 3D scenes in React. It does not replace Blender for asset creation, simulations, or cinematic rendering.

### 4. Video export still needs a shell

For final MP4, you still need Remotion/capture/custom render pipeline.

## 14.4 Best use cases

- React product sites with 3D
- componentized 3D motion systems
- 3D elements inside Remotion
- interactive demos
- reusable 3D dashboard/product visuals

## 14.5 Bad use cases

- beginner 3D learning without patience
- cinematic VFX
- shot compositing
- manual 3D asset creation
- simple 2D motion graphics

## 14.6 Overall judgment

**A strong bridge between React and Three.js.**

For your world, R3F is most interesting as a way to put reusable 3D components inside Remotion-like video systems or web demos.

---

# 15. Lottie / dotLottie / Bodymovin / lottie-web / LottieFiles

**Official positioning:** Lottie is a JSON-based animation format for shipping animations across platforms like static assets.  
**Language:** JSON format; usually produced from AE or design tools; rendered via libraries  
**Best for:** UI micro-animations, animated icons, app/web illustrations, lightweight vector motion.

## 15.1 What it is

Lottie is not exactly a motion graphics authoring system. It is more a **portable vector animation format**. A designer creates an animation, often in After Effects, exports it as Lottie JSON via Bodymovin/LottieFiles tooling, then developers render it in web/mobile/app contexts.

## 15.2 Pros

### 1. Lightweight scalable animation

Lottie’s big appeal:

- small files
- vector scaling
- works across platforms
- good for UI animations
- better than GIFs for many app/web uses

### 2. Great developer handoff format

For product teams, Lottie can be a nice bridge between motion designer and developer.

### 3. Excellent for micro-interactions

Best examples:

- loading indicators
- success checkmarks
- onboarding illustrations
- animated icons
- empty states
- button feedback
- app/web decorative motion

### 4. Can be controlled programmatically

Using lottie-web or platform players, you can trigger playback, scrub frames, loop segments, or connect animation to UI events.

## 15.3 Cons

### 1. Not ideal for full videos

Lottie is not where you make a full 60-second promo video with footage, music, voiceover, subtitles, and rich compositing.

### 2. Feature compatibility issues

Not every After Effects feature exports cleanly to Lottie. Expressions, effects, masks, gradients, blur, 3D layers, and raster elements can be limited or inconsistent depending on player/exporter.

### 3. JSON is not pleasant to author manually

Technically code-based? Yes. Practically hand-coding Lottie JSON is miserable. You usually generate it.

### 4. Debugging can be annoying

When an animation looks different in AE, browser, Android, iOS, or a specific player, debugging can be unpleasant.

### 5. Weak cinematic range

Lottie is great for vector UI motion. It is not for cinematic motion graphics, realistic lighting, or heavy visual effects.

## 15.4 Best use cases

- app/web micro-animations
- icons
- onboarding illustrations
- UI feedback
- tiny reusable vector animations
- animations embedded inside a product demo

## 15.5 Bad use cases

- full promo videos
- footage-heavy motion graphics
- cinematic VFX
- complex AE effects
- manual code-authored animation

## 15.6 Overall judgment

**Excellent delivery format for UI animation; poor primary system for video motion graphics.**

Use Lottie as an asset format, not as your main production environment.

---

# 16. SVG animation tools and code-driven SVG

This category includes raw SVG + CSS/JS, SVGator-like tools, Snap.svg/anime.js-style libraries, and generated SVG systems.

## 16.1 What it is

SVG is a vector graphics format that can be animated with:

- CSS transitions/keyframes
- SMIL animation, where supported
- JavaScript
- GSAP
- D3
- custom code
- exported tools like SVGator

## 16.2 Pros

### 1. Perfect for crisp vector motion

SVG is ideal for:

- logos
- icons
- diagrams
- arrows
- UI overlays
- line art
- simple illustrations

### 2. Programmatically manipulable

Because SVG is structured XML/DOM, code can target specific shapes, paths, groups, masks, and transforms.

### 3. Works well with D3 and GSAP

D3 can generate the SVG; GSAP can animate it. This is a very strong combination.

### 4. Scales cleanly

Vector graphics remain sharp at different resolutions.

## 16.3 Cons

### 1. Complex SVGs become a mess

Exported SVGs from design tools can contain strange group structures, inline styles, masks, paths, transforms, and random IDs.

Animating them cleanly may require manual cleanup.

### 2. Not good for raster/cinematic footage

SVG is for vector graphics. It is not a compositing or video-editing system.

### 3. Browser compatibility and export issues

Some SVG features render differently between environments.

### 4. Does not solve timeline/audio/video export alone

You still need Remotion, Motion Canvas, browser capture, or another render pipeline.

## 16.4 Best use cases

- logo animation
- technical diagrams
- vector callouts
- arrows and labels
- chart elements
- UI overlay graphics
- generated infographic animation

## 16.5 Bad use cases

- cinematic footage compositing
- 3D
- simulation
- large video projects by itself

## 16.6 Overall judgment

**SVG is a core ingredient, not a complete production system.**

For software motion graphics, SVG + Remotion/Motion Canvas/D3/GSAP is extremely useful.

---

# 17. AI vector animation and LLM-assisted animation

Examples/current research directions:

- LottieGPT-style native vector/Lottie generation
- OmniLottie-style parameterized Lottie token systems
- LiveSVG-style SVG animation via generated video fitting
- LLM-assisted Manim code generation
- LLM-generated Remotion/Motion Canvas/SVG/GSAP code

## 17.1 What it is

This is the emerging field of generating editable vector/code animation from text, images, prompts, or partial assets.

The important distinction:

- **Raster video generation** creates pixels.
- **Vector/code animation generation** creates editable structure: paths, layers, keyframes, objects, parameters.

For motion graphics, the second is far more interesting long-term.

## 17.2 Pros

### 1. Potentially huge future advantage

If these systems mature, they could generate editable animations instead of flat videos. That would be far more useful for production.

### 2. Good fit for structured motion

Motion graphics often has semantic structure:

- title enters
- icon bounces
- line draws
- chart grows
- card slides
- arrow points

That is more code/vector-friendly than cinematic live-action generation.

### 3. LLMs already help with code generation

Even today, LLMs can help write:

- Remotion components
- Motion Canvas scenes
- Manim scenes
- SVG markup
- GSAP timelines
- Blender Python scripts

This is already useful, with human correction.

## 17.3 Cons

### 1. Not reliable enough as production core

Research demos are not the same as a dependable client pipeline.

Generated animation often fails in boring but fatal ways:

- wrong timing
- broken layout
- bad typography
- weird object hierarchy
- messy code
- non-editable outputs
- hallucinated APIs
- poor brand consistency

### 2. Editable output is still hard

A generated animation is only useful if you can revise it. Motion graphics production is revision-heavy.

If an AI gives you something pretty but uneditable, it is often a trap.

### 3. Design taste remains unsolved

AI can produce motion, but good motion design requires rhythm, restraint, hierarchy, brand sense, and timing.

### 4. Tool fragmentation

There is no stable dominant AI-native vector animation production workflow yet.

## 17.4 Best use cases today

- generating first drafts of code
- ideating SVG/Manim/Remotion structures
- producing throwaway prototypes
- converting descriptions into animation scaffolds
- helping with boilerplate

## 17.5 Bad use cases today

- client-critical final animation without human review
- precise brand deliverables
- complex revision workflows
- anything with strict timing and typography unless manually controlled

## 17.6 Overall judgment

**Use AI as a coding assistant, not as the motion graphics engine.**

The practical 2026 workflow is: ask an LLM to help generate Remotion/Motion Canvas/Blender/Manim code, then you own the pipeline and fix the details.

---

# 18. Comparison matrix

Scores are subjective: 1 = weak, 5 = excellent.

| Tool | Video export | 2D MG | 3D | Data-driven | UI/software video | Explainers | VFX/compositing | Dev ergonomics | Production fit |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| Remotion | 5 | 4 | 2–3 | 5 | 5 | 4 | 1 | 5 | 5 |
| Motion Canvas | 4 | 5 | 1–2 | 4 | 4 | 5 | 1 | 4 | 4 |
| Manim | 4 | 3 | 1 | 4 | 2 | 5 | 1 | 3 | 3 |
| Blender Python | 4 | 3 | 5 | 4 | 2 | 3 | 4 | 2–3 | 5 for 3D |
| Fusion scripting | 4 | 4 | 3 | 2 | 3 | 3 | 5 | 2 | 4 inside Resolve |
| After Effects scripting | 5 | 5 | 2–3 | 3 | 4 | 4 | 4 | 2–3 | 5 in AE world |
| GSAP | 1–2 | 5 | 2 | 4 | 5 | 4 | 1 | 5 | 3 unless paired |
| D3 | 1–2 | 4 | 1 | 5 | 3 | 4 | 1 | 3 | 4 as ingredient |
| p5.js | 1–2 | 4 | 1–2 | 3 | 2 | 3 | 1 | 4 | 2–3 |
| Processing | 1–2 | 4 | 2 | 3 | 1–2 | 3 | 1 | 3 | 2 |
| Three.js | 1–2 | 2 | 4 | 3 | 4 | 3 | 1–2 | 3 | 3 as ingredient |
| React Three Fiber | 1–2 | 2 | 4 | 3 | 4 | 3 | 1–2 | 4 | 3 as ingredient |
| Lottie | 1 for full video / 5 for app delivery | 4 | 1 | 2 | 4 for UI micro | 2 | 1 | 2 | 5 for micro-animations |

---

# 19. Recommended workflows by job type

## 19.1 Vertical software promo / LinkedIn reel

**Best stack:** Remotion + screen recording + SVG/CSS + optional D3/GSAP  
**Finish:** Resolve

Workflow:

1. Capture clean screen recording.
2. Put recording into Remotion composition.
3. Add timed text, callouts, highlights, zoom frames, progress bars, captions.
4. Render MP4 or image sequence.
5. Finish audio, grade, and delivery in Resolve.

Why not Fusion first? Fusion can do this, but Remotion is more reusable and easier to template.

## 19.2 Technical explainer / architecture diagram

**Best stack:** Motion Canvas  
**Alternative:** Remotion + SVG/D3  
**Finish:** Resolve if needed

Use Motion Canvas when clarity and timing are the main point.

## 19.3 Data visualization animation

**Best stack:** D3 for layout + Remotion for timeline/render  
**Alternative:** Motion Canvas for simpler diagrammatic data visuals

Rule: use D3 to compute visual geometry; use Remotion to own video timing.

## 19.4 Abstract generative background

**Best stack:** p5.js or Three.js, then capture/render through Remotion or image sequence  
**Alternative:** Blender for richer 3D procedural backgrounds

Use p5.js for fast experiments. Use Blender when it needs depth, lighting, materials, and cinematic quality.

## 19.5 3D product/object motion

**Best stack:** Blender Python/manual Blender hybrid  
**Alternative:** Three.js/R3F if the asset is web-native or interactive

For real render quality, Blender wins.

## 19.6 VFX shot element, such as blood, wound enhancement, muzzle mark, object insertion

**Best stack:** Blender + Fusion/Resolve  
**Possible workflow:**

1. Stabilize/track shot/head/region.
2. Build 3D or 2D element in Blender/Fusion.
3. Render passes/mattes.
4. Composite in Fusion.
5. Match color, grain, blur, lens, and motion.

Remotion is not the tool for this.

## 19.7 Resolve-native lower-thirds package

**Best stack:** Fusion macro + manual Resolve workflow  
**Alternative:** Generate overlays externally in Remotion and import as transparent video/image sequence

If editors need to tweak text inside Resolve, Fusion macro is better. If you alone generate final overlays, Remotion may be cleaner.

## 19.8 Website/app micro-animations

**Best stack:** Lottie or GSAP  
**Alternative:** CSS/SVG animation

Do not render these as video unless the target is actually a video.

---

# 20. Suggested learning path

## Phase 1 — Remotion basics

Goal: make a 15-second vertical product promo from code.

Learn:

- compositions
- frame/time helpers
- sequences
- interpolate/easing
- text and images
- video assets
- audio
- vertical layouts
- rendering
- passing JSON props

First project:

- 1080x1920 vertical video
- screen recording in center
- animated headline
- 3 callouts
- captions
- CTA end card

## Phase 2 — D3 inside Remotion

Goal: make a data-driven animation.

Learn:

- D3 scales
- generated SVG paths/shapes
- animated chart states
- using Remotion frame as time source

First project:

- “before/after” chart animation for a Shopify/product metric.

## Phase 3 — Motion Canvas

Goal: make a clean technical explainer.

Learn:

- scenes
- generator-based timing
- shapes/text
- arrows/lines
- layout
- voiceover sync

First project:

- “How our Shopify app processes an order” architecture animation.

## Phase 4 — Blender Python

Goal: generate one reusable 3D asset animation.

Learn:

- create objects
- set materials
- animate transforms
- camera/lights
- render transparent PNG/EXR sequence
- import into Resolve

First project:

- floating 3D product cards or logo object with camera move.

## Phase 5 — Fusion macros/scripting

Goal: create a Resolve-native reusable title/callout.

Learn:

- Fusion text tools
- merge/transform/background
- publishing macro controls
- simple Lua/Python script to create a comp

First project:

- lower-third/callout macro with editable text, color, and position.

---

# 21. Strong opinions

## 21.1 Remotion is the practical winner for your software-video use case

It fits your JavaScript background, your product-video needs, and your likely desire to automate variants. It also complements Resolve: generate the motion layer in code, finish in Resolve.

## 21.2 Motion Canvas is probably more elegant than Remotion for pure explainers

If the whole video is diagrams and voiceover, Motion Canvas may feel cleaner. But for mixed-media product videos, Remotion is more general.

## 21.3 Fusion is valuable, but not a pleasant coding home

Fusion is a compositing environment first. Use it where its node/tracking/compositing strengths matter. Do not force it to become a web-style code animation framework.

## 21.4 Blender Python is a monster, not a convenience tool

It can do almost anything 3D. But the cost is real. Use it when the visual payoff justifies it.

## 21.5 After Effects is employable, but strategically annoying for a Resolve/Linux person

AE is standard. That matters. But it is not the tool I would voluntarily build a programmer-first video-generation workflow around in your setup.

## 21.6 Lottie is not “video motion graphics”

It is excellent for app/web micro-animation. Do not confuse that with a full video production pipeline.

## 21.7 AI should write drafts of animation code, not own the pipeline

The sane approach is: AI helps generate code; you use stable tools to render and revise.

---

# 22. Practical final ranking for you

## Learn first

1. **Remotion**
2. **Motion Canvas**
3. **D3 inside Remotion**

## Learn for 3D / VFX jobs

4. **Blender Python**
5. **Fusion compositing + macros**

## Keep as ingredient libraries

6. **GSAP**
7. **Three.js / React Three Fiber**
8. **p5.js**

## Learn only as needed

9. **After Effects expressions/scripts**
10. **Lottie tooling**
11. **Processing**

## Watch, but do not rely on yet

12. **AI vector/code animation systems**

---

# 23. Source notes

These notes are based on the current public positioning and documentation of the tools as checked on 2026-06-13:

- Remotion: https://www.remotion.dev/ and https://www.remotion.dev/docs/render
- Remotion parameterized rendering: https://www.remotion.dev/docs/parameterized-rendering
- Motion Canvas project/docs: https://github.com/motion-canvas/motion-canvas and https://motion-canvas-docs.vercel.app/
- Manim Community docs: https://docs.manim.community/en/stable/
- Blender Python API: https://docs.blender.org/api/current/index.html
- Fusion scripting guide: https://documents.blackmagicdesign.com/UserManuals/Fusion8_Scripting_Guide.pdf
- Adobe After Effects expression reference: https://helpx.adobe.com/in/after-effects/using/expression-language-reference.html
- Adobe scripts in After Effects: https://helpx.adobe.com/in/after-effects/using/scripts.html
- GSAP: https://gsap.com/ and https://gsap.com/docs/v3/
- D3: https://d3js.org/
- p5.js: https://p5js.org/
- Processing: https://processing.org/
- Three.js: https://threejs.org/ and https://threejs.org/docs/
- React Three Fiber: https://r3f.docs.pmnd.rs/getting-started/introduction
- LottieFiles: https://lottiefiles.com/what-is-lottie
- LottieGPT paper: https://arxiv.org/abs/2604.11792
- OmniLottie paper: https://arxiv.org/abs/2603.02138
- LiveSVG paper: https://arxiv.org/abs/2605.30174

---

# 24. One-sentence tool chooser

- Want **software promo video**? Use **Remotion**.
- Want **clean explainer diagrams**? Use **Motion Canvas**.
- Want **math/DSA/STEM explanation**? Use **Manim**.
- Want **3D/cinematic/procedural assets**? Use **Blender Python**.
- Want **Resolve-native compositing/title templates**? Use **Fusion macros/scripting**.
- Want **agency-standard motion design scripting**? Use **After Effects expressions/scripts**.
- Want **web UI/SVG animation**? Use **GSAP**.
- Want **animated data visualization**? Use **D3**, preferably inside Remotion.
- Want **generative art sketches**? Use **p5.js**.
- Want **browser 3D**? Use **Three.js or React Three Fiber**.
- Want **app micro-animations**? Use **Lottie**.
- Want **AI animation generation**? Use it for scaffolding, not final production.
