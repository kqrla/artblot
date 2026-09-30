# browser sim -- live html canvas plotter simulation

a browser-based simulation of the plotter drawing stroke by stroke, in real time. this is the first thing you can actually see and interact with in inkblotter.

this is its own separate orphan branch from the engine branch. read the distinction below before assuming this is where planning logic lives.

---

## browser sim branch versus engine branch

these are two different branches. the engine branch is the actual analysis and planning logic, headless, no ui, takes an image in and produces a stroke plan out. the browser sim branch is purely a visualization and testing tool: it takes a stroke plan the engine already produced and animates it being drawn on an html canvas, so a human can see it without any hardware.

concretely: if you are writing code that decides where lines go based on image darkness, that belongs in the engine branch. if you are writing code that draws a line on a canvas element and steps to the next one on a timer, that belongs here.

see engine.md for the planning logic side of this.

---

## what it is

an html canvas app that:

1. takes a stroke plan, produced by the engine branch, as input
2. animates the drawing process live, one stroke at a time, in order
3. looks like watching a real pen plotter work

it is not a photo filter or an instant render. you watch it draw. it does not run the planning logic itself, it visualizes plans the engine already computed.

## this is not a generative model, and never will be

the browser sim does not do anything resembling what a diffusion model does. there is no sampling, no denoising, no hallucinated pixels, nothing probabilistic happening on the canvas at all. every line drawn is a deterministic coordinate pair that the engine branch already computed ahead of time. the canvas is just replaying a fixed, ordered list, exactly like a pen plotter physically replays a fixed toolpath. if you fed it the same stroke plan twice, it draws the exact same thing twice, pixel for pixel, every time. keep this distinction explicit in any code or documentation added to this branch, since the visual result of watching an image "emerge" on screen can superficially look similar to watching a diffusion model denoise an image, but the underlying mechanism has nothing in common.

---

## ui sketch

```
+------------------------------------------+
|  inkblotter sim                           |
|                                          |
|  [upload image]   density: ####.. 60%   |
|  angle: 45  crosshatch: on              |
|  speed: ..###.. medium                  |
|                                          |
|  +--------------------------------------+|
|  |                                      ||
|  |        (canvas drawing here)         ||
|  |                                      ||
|  |  plotter head: *                     ||
|  +--------------------------------------+|
|                                          |
|  stroke 847 / 2304     [pause] [reset]  |
+------------------------------------------+
```

---

## technical spec

### stack
- vanilla html + javascript, no framework needed for v1
- html5 canvas api for drawing
- web workers optionally for planning computation (keeps ui responsive)

### animation loop
- generate all strokes upfront as an ordered array
- use requestanimationframe to draw one stroke per frame (or multiple at higher speeds)
- each stroke: move "pen" from point a to point b, draw the line
- optional: animate the plotter head dot moving across the canvas

### stroke representation (internal)
```json
{
  "id": 847,
  "x1": 120, "y1": 340,
  "x2": 145, "y2": 365,
  "angle": 45,
  "weight": 1.2,
  "pen_up_after": false
}
```

### controls
- upload image: any jpg or png
- density: how dark = how many lines (slider)
- angle: primary hatch angle in degrees
- crosshatch: toggle second angle on/off
- speed: animation playback speed (slow to watch detail, fast to see result)
- pause / resume: stop mid-draw
- reset: clear and start over

---

## visual style of the sim

- white or off-white canvas background
- thin dark lines (like a 0.3mm technical pen)
- small dot or crosshair showing current plotter position
- stroke counter visible (so you can see how many marks it takes)
- no color in the crosshatch beta, value only

### future direction: real material swatches instead of flat color

worth studying colorwork.studio for this. it is a browser tool for charting knit colorwork, and instead of rendering flat idealized color blocks for each yarn, it renders every swatch from an actual photograph of that yarn, shot under consistent studio lighting, so you see the real texture, sheen, and natural color variation before committing to a physical piece.

flat rendered color is fine for the crosshatch beta, since it is just thin technical-pen-style lines with no material to represent. but once the sim needs to preview embroidery thread or tufting yarn, flat color will look noticeably fake next to the real material. the same approach colorwork.studio uses, photographing actual thread or yarn under controlled lighting and rendering swatches from those photographs instead of flat fills, is the right direction for those later sims. not needed for crosshatch beta v1 or v2, but should be the plan once embroidery or tufting previews are built.

colorwork.studio's own rendering code is not public, their published open source license list only covers minor third party pieces like fonts, not the actual swatch renderer. per their own faq, their approach is to photograph each yarn by hand under consistent studio lighting, then render knit swatches from those photographs using traditional computer graphics techniques, running live in the browser via webgl2. since their implementation itself is not available to look at, build this with a clean room approach: study and document the observable result, what the rendered swatches look like and how they read, without referencing or copying any actual implementation, then build inkblotter's version independently from that spec.

### comparable projects, by tier

industrial / commercial tools:
- shima seiki's sds-one apex fabric simulation platform. scans real yarns and uses that scanned data to render fuzz and texture in woven fabric, circular knitting, towel, and embroidery simulations. built for professional textile manufacturing, closest industrial comparable to what colorwork.studio does for hobbyists.
- weavecad and nedgraphics, professional textile and weave design software, worth a look for how they structure the design-to-visualization pipeline, not for direct technique borrowing.

open source / hobbyist projects on github:
- yunniko/crochet-simulator -- renders yarn live as genuine three dimensional loop-through-loop stitch tubes, computed from real stitch topology in the core logic itself, not a flat rendering-layer approximation. closest open reference for how to represent real stitch geometry rather than faking it visually.
- hannah144/crochet-simulator -- a simpler framework for generating basic crochet structures from yarn, hook, and stitch type.
- tylervick/graphghan (see issue 51 in that repo) -- an active, detailed discussion of exactly the hard problem this kind of sim runs into: knit and crochet stitches need genuinely different yarn geometry and cross-sections, not just different textures, citing real academic research on representing crochet as stitch meshes. worth reading in full before building inkblotter's stitch renderer.

academic research:
- kuiwuchn.github.io/rtstitch -- real-time knit deformation and rendering, fiber-level yarn simulation fast enough for real-time use, with a published approach.
- kuiwuchn.github.io/stitchmodeling -- stitch mesh modeling, going from a 3d garment shape to a knittable stitch structure.
- cornell (kavita bala et al), "fitting procedural yarn models for realistic cloth rendering" -- captures real photographed thread and fits a procedural yarn model to it for rendering. closest academic match to what colorwork.studio is likely doing internally.
- "rapid simulation of yarn-level realism in fuzzy yarn knitted fabrics" (fibers and polymers journal) -- a newer approach using consumer-grade imaging equipment rather than specialized scanning hardware, notable since it lowers the cost of capturing real yarn data.
- dragonbook/awesome-cloth on github -- a curated running list of cloth, yarn, and fabric research, useful as an ongoing reference rather than a single source.

all of this is reference material for technique and approach only. per the clean room note above, do not copy implementation from any project whose source is not explicitly open and licensed for reuse.

### comparable projects, specifically for tufting

tuftnano.com -- an ai powered tufting design studio, closest direct comparable to inkblotter's tufting pipeline. it uses segment anything model to automatically isolate distinct color regions and extract the subject from a background image, then uses depth anything v2 to generate a grayscale depth map that controls pile height, white pixels map to maximum pile height around 40mm, black pixels map to flatweave around 10mm. this is a strong direct reference for the engine's color region extraction job on the tufting branch specifically. likely closed source, same clean room caution applies, study the approach and observable output, do not reference or copy any implementation.

nedgraphics tuft premium -- industrial tuft cad software, newly includes a proper 3d viewer (built on their u3m file format) that previews tufted fabric fully unrolled with realistic, customizable yarn parameters from a 3d yarn library. good reference for how a mature, production grade tufting preview pipeline is structured, aimed at professional carpet manufacturers rather than hobbyists.

tuftlab.app and thetuftingcalculator.com -- two free browser tools that take an uploaded image and break it into a yarn color and yardage list per color, used for cost and material estimation. useful small reference for the image-to-per-region-yarn-requirements step, a simpler adjacent problem to full visual rendering.

pointcarre.com's tuft cad tools -- professional carpet design software compatible with real industrial tufting machines (cmc, tuftco, vandewiele, modra), with 3d rug simulation and ai assisted design tools. another industrial-grade reference point, similar tier to nedgraphics.

---

## why this matters for the wall arm branch

the browser sim produces the same stroke sequence that the wall arm branch's hardware would physically execute. the json export from the sim is the proto version of the stroke plan format. once the wall arm branch exists, you can feed it the same file the sim used and the arm should draw the same image.

---

## sim is done when

- [ ] image upload works
- [ ] crosshatch lines are generated correctly from image value
- [ ] animation draws stroke by stroke in real time
- [ ] plotter head position is visible
- [ ] pause / reset controls work
- [ ] speed control works
- [ ] stroke count is displayed
- [ ] can export stroke list as json
