# inkblotter

> a robot that paints with real brushes and real paint, and a separate small tabletop machine that manufactures enamel-style pins on demand using uv resin. deterministic engine, no generation in the pipeline.

---

## what is this

inkblotter started because i wanted a robot that could actually paint.

not plot. not draw. not stamp. paint.

and when i started digging into why nobody had done it well, it became obvious pretty fast:

- pen plotters deposit ink uniformly no matter what. solved.
- cnc machines and axidraw are also solved.
- a paintbrush is not solved.

a loaded brush behaves completely differently from a pen. paint runs out as you stroke. bristles bend and spring back. wet paint bleeds into wet paint on the canvas. you have to know when to re-dip. you have to know when to switch brushes. none of that is a software problem you can skip.

that's the thing nobody had built yet. so that's what this is.

---

## what it turned into

what started as one idea is now four separate pieces of work.

they do not share code. they mostly do not share context with each other. they live as separate orphan branches in this repo because they are genuinely different problems with different goals and different audiences:

```
engine          -- the research brain. analyzes an image and produces physical instructions.
browser sim     -- visualizes what the engine produced, stroke by stroke, before any hardware exists.
wall arm        -- the big robot that physically paints with real brushes and real paint.
resin plotter   -- a small tabletop machine that manufactures enamel-style pins on demand.
```

they are not phases of one product. they are not nested inside each other. the engine does not own the browser sim. the wall arm and the resin plotter use completely different machines.

---

## the engine

the engine is the analysis and planning brain.

given a reference image, it figures out:

- what colors are in it
- where the regions are
- what the value structure looks like
- what kind of marks were used to make it
- what order those marks were probably applied in

and it turns that understanding into a set of physical instructions.

it does not have a ui. it does not touch hardware. it does not care which machine will eventually execute its output. it is purely analytical, headless, and hardware agnostic.

it belongs to neither hardware branch. it is its own thing.

see engine.md.

---

## the browser sim

a separate piece of software from the engine.

it takes a stroke plan the engine already produced and plays it back on an html canvas, stroke by stroke, live, so you can watch it happen and verify it looks right -- without any hardware built yet.

it makes no planning decisions. it does not figure out where strokes go. it just replays what the engine already decided.

watching it might look a bit like watching a diffusion model denoise an image. the underlying mechanism has nothing in common. the browser sim is not sampling or predicting anything. it is deterministically replaying a fixed ordered list of coordinates. feed it the same input twice and it produces the identical result every single time.

see browser-sim.md.

---

## the wall arm

the big, ambitious, vertical robot arm.

it physically holds real brushes, dips them in real paint, and paints on a wall-mounted canvas. this is where wet medium physics actually has to get solved in hardware:

- paint load decay -- a brush runs out of paint as you stroke
- bristle spring-back -- bristles bend and recover unpredictably
- wet-on-wet bleeding -- fresh paint bleeds into wet paint on the canvas
- re-dip timing -- the arm has to know when to go back to the palette
- brush switching -- physically swapping to a different brush mid-painting

none of these have the same answer as "pen up, swap pen, pen down."

this branch is not a commercial product. it is a hard engineering problem worth pursuing for its own sake. the right audience is engineering grants and technical credibility, not investors.

see wall-arm.md, wet-medium-physics.md, brush-and-paint-types.md.

---

## the resin plotter

the actual business.

a small, tabletop, regular pen-plotter-sized machine. instead of ink or paint, it dispenses uv resin, which stays liquid and workable until you deliberately hit it with uv light. then it cures completely, and you can go to the next layer.

it plugs into a creator's existing storefront -- shopify, squarespace, instagram shop -- as a backend fulfillment layer. a customer buys a pin or keychain through whatever the creator already sells on. payment goes through something like stripe connect. only then does the machine actually make that one item.

the thing this removes:

- no kickstarter with demand that hasn't happened yet
- no bulk minimum order (no "commit to 20 units before a single one sells")
- no speculative inventory
- no stale stock
- no clearance sales
- nothing manufactured that wasn't already sold

one real paid order. one item made. that's the whole loop.

uv resin specifically because:

- it doesn't begin curing on its own -- consistent viscosity all day, no stringing or trailing from the nozzle
- designs can be built up in cured layers, like working in procreate -- each layer is locked before the next one goes down, no wet-on-wet ambiguity at all
- enamel-style designs are naturally flat color regions with hard edges, so the engine's job for this branch is simpler -- mostly color region extraction and layer order, not brushstroke analysis

see resin-plotter.md.

---

## why they are kept apart

each branch has a different goal and a different audience.

- the engine is pure research. it should stay hardware agnostic and not carry paint physics assumptions or resin curing assumptions.
- the browser sim is a visualization tool. it should stay free of planning logic.
- the wall arm is an engineering pursuit, grant-shaped and credibility-shaped.
- the resin plotter is a commercial product, storefront-shaped and vc-shaped.

mixing them would force compromises none of them need to make.

---

## this is not ai art

it would be easy to hear "image analysis engine plus robot that makes marks" and file this under generic ai image generation. it is not that.

no prompts. no generation. no reimagining.

the engine studies an existing reference image to figure out how to physically recreate or manufacture it. the output is always a physical object -- real paint or real cured resin -- not a rendered image. whatever image goes in is what comes out the other end, as faithfully as the hardware allows.

diffusion models given a reference image still add their own stylistic drift. the output is a new generated image inspired by the input, not a reproduction of it. inkblotter does the exact opposite. whatever goes in is exactly what gets reproduced. zero creative interpretation anywhere in the pipeline.

---

## files in this repo

| file | what is in it |
|---|---|
| readme.md | this file |
| explain.md | full plain language walkthrough if you want the whole picture in one place |
| engine.md | the research branch: ingestion and stroke planning |
| browser-sim.md | the html canvas visualization, separate from the engine |
| wall-arm.md | the large articulated arm painting branch |
| resin-plotter.md | the tabletop uv resin print on demand branch |
| crosshatch-beta.md | the engine's first milestone: crosshatch detection and replay |
| stroke-plan-format.md | the wall arm's data contract, produced by the engine |
| wet-medium-physics.md | the wall arm's core unsolved problem |
| brush-and-paint-types.md | brush and paint reference for the wall arm branch |
| embroidery.md | small desktop scale embroidery branch, early idea stage |
| open-questions.md | things not decided yet |
| glossary.md | terms used across the project |
| for-agents.md | context file for any ai agent or collaborator joining the project |

---

## relationship to illustrace

illustrace is a separate project, a software research benchmark for measuring and transferring visual style in illustrations. the overlap with inkblotter is about 5%, limited to the possibility that illustrace might someday hand the engine an image to analyze. inkblotter does not use illustrace's architecture, parameters, or operators.
