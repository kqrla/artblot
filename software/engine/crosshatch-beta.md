# crosshatch beta -- first milestone for the engine branch

the simplest possible version of the stroke planning engine. no paint, no color, no brush physics. just crosshatching: parallel lines at varying densities and angles that create the illusion of shading and value.

this milestone is staged into two versions. version one is deliberately simpler than you might expect, and that is intentional. read the staging section before assuming this generates hatching from any photo.

---

## why crosshatch first

crosshatching is the minimum viable stroke language because:

- output is just line segments, the simplest possible geometry
- a pen plotter can already execute it physically
- value (light/dark) is encoded purely in line density and angle, no color needed
- it is visually verifiable instantly -- does it read as a face or landscape from across the room?
- it lets the whole engine branch pipeline be tested end-to-end before introducing paint complexity

---

## staged approach: version one versus version two

### version one -- input is already crosshatched

version one deliberately takes an image that is already crosshatched artwork as its input, not a plain photo or painting. the engine's job in version one is to detect and vectorize the existing hatching linework that is already present in the image, turning it into an ordered stroke plan, not to decide where hatching should go.

this is a deliberate scoping choice, not a shortcut or a limitation to apologize for. by removing the "where should hatching go" decision from version one entirely, both the engine's vectorization logic and the browser sim's physical playback can be built and validated on their own, without also having to solve image understanding at the same time. this keeps version one strictly deterministic and strictly a tracing and replaying problem, with zero interpretation happening anywhere in the pipeline.

version one is not a reduced or simplified version of crosshatch generation. it does not do any of the value extraction, density mapping, or angle assignment steps described below. those belong to version two.

### version two -- input is a plain image, engine decides the hatching

version two is a different weight class of problem from version one, not just a harder variant of it. version one is pure execution: the hatching already exists, the engine just has to find it and trace it faithfully. version two requires the engine to make the actual artistic decision of where hatching should go and how dense it should be, which is a research problem, not an execution problem.

the algorithm sketch below, value extraction, density mapping, angle assignment, is a reasonable naive starting point, but getting it to actually read as convincing shading is genuinely hard. simply filling darker regions with more lines mechanically does not automatically look like shading a human would draw. real hatching decisions account for things like where form actually changes (not just where an area happens to be dark), which directions imply volume versus flat surface, how line spacing needs to vary smoothly rather than in a blocky grid, and where a human hatcher would deliberately break or curve lines to follow a form's contour rather than a fixed angle.

this means version two likely needs its own research phase before implementation, closer in spirit to the engine branch's broader ingestion and study work, understanding palette, regions, and value structure, than to a quick algorithm. expect version two to require iterating on what "reads as good hatching" even means before writing the code that produces it, not just tuning parameters on the naive algorithm below.

do not build version two's logic before version one's tracing and playback pipeline is working end to end. they are sequential, not parallel, and they are not the same size of problem.

---

## browser simulation

before any hardware, the crosshatch engine's output runs live in the browser on an html canvas, for both versions.

key behavior:
- it draws in front of you, stroke by stroke, in real time
- not an instant image render, you watch the plotter head move
- same path order and timing as the physical arm would follow
- you can see the image emerge line by line, just like watching a real plotter
- deterministic in both versions, same input always produces the same output, see browser-sim.md for why this is fundamentally unlike a diffusion model

see [browser-sim.md](browser-sim.md) for the full simulation spec.

---

## version one: detecting and vectorizing existing hatching (algorithm sketch)

### step 1 -- line detection
find the existing hatching lines already present in the input image, using edge detection or line detection methods, since the hatching pattern is already drawn, not being invented.

### step 2 -- vectorization
convert detected raster lines into precise vector line segments, start point and end point.

### step 3 -- stroke ordering
order the detected strokes for efficient plotter travel, minimizing pen up travel distance between segments.

### step 4 -- export
output the ordered line segments as an svg path or json stroke list, ready for the browser sim to animate or for stroke-plan-format.md's v0 format.

---

## version two: generating hatching from a plain image (algorithm sketch)

this only applies once version one is solid.

### step 1 -- value extraction
convert the input image to grayscale. this gives a value map where 0 = black and 255 = white.

### step 2 -- region tiling
divide the canvas into a grid of small cells (e.g. 20x20px each).

### step 3 -- density mapping
for each cell, measure the average value:
- dark cell: high line density (lines close together)
- light cell: low line density (lines far apart or none)
- white cell: no lines

### step 4 -- direction assignment
assign line angles:
- single hatch: all lines at one angle (e.g. 45 degrees)
- crosshatch: two angles (45 and 135 degrees)
- multi-directional: angle follows the gradient direction of the image (more expressive)

### step 5 -- path generation
generate line segments for each cell based on density and angle. output as svg paths or a json stroke list.

### step 6 -- stroke ordering
order strokes for efficient plotter travel (minimize pen-up travel distance). standard tsp-adjacent pen plotter optimization.

---

## open source starting points

| tool | what it does |
|---|---|
| vpype | pen plotter path processing pipeline |
| axidraw python api | controls axidraw hardware |
| linedraw | image to pen plotter crosshatch, relevant mainly to version two |
| stipplegen | stippling (dots not lines, but similar concept) |

linedraw is closer to version two's problem. version one's line detection and vectorization is closer to general computer vision line/edge detection tooling, not a crosshatch-specific tool.

---

## version one is done when

- [ ] input: an image that already contains crosshatched artwork
- [ ] output: live stroke-by-stroke animation on html canvas
- [ ] existing hatching lines are correctly detected and vectorized, not regenerated or reinterpreted
- [ ] stroke order is logical (not jumping all over the canvas)
- [ ] can export the stroke list as json (proto-stroke-plan format)
- [ ] the animated redraw is a faithful match to the input hatching, same lines, same positions

## version two is done when

- [ ] input: any plain image (jpg/png), no existing hatching required
- [ ] output: live stroke-by-stroke animation on html canvas
- [ ] value to line density mapping works and looks correct
- [ ] at least two hatch angles work (single and cross)
- [ ] stroke order is logical (not jumping all over the canvas)
- [ ] can export the stroke list as json (proto-stroke-plan format)
- [ ] looks like a recognizable version of the input from arm's length
