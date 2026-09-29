# for agents -- read this first

if you are an ai agent being given context on inkblotter, read this file before anything else. it will save you from making the same mistakes other agents have made.

---

## what inkblotter is not

- not one product. it is four separate branches of work.
- not a digital image filter or renderer
- not a single hardware platform, the wall arm and the resin plotter are genuinely different machines
- the engine and the browser sim are not the same branch either, even though both are software
- not a twin or fork of illustrace (see below)

---

## what inkblotter actually is

inkblotter is four separate orphan branches that mostly do not share code, hardware, or context with each other:

1. engine -- research branch. analyzes a reference image and figures out how to turn it into physical instructions, palette, regions, value structure, stroke direction, layer order. hardware agnostic. does not belong to either hardware branch. headless, no ui.

2. browser sim -- visualization branch. an html canvas app that takes a stroke plan the engine already produced and animates it being drawn live, so a human can verify it without hardware. this is a separate branch from the engine, not a subdirectory of it, even though both are software. do not put planning logic here, and do not put canvas rendering logic in the engine branch.

3. wall arm -- large, vertical, articulated arm hardware that paints with real brushes and real paint on wall mounted or large format canvas. the unsolved problem here is wet medium physics. not commercially viable on its own, positioned toward hardware grants and engineering credibility.

4. resin plotter -- small, tabletop, pen-plotter-form-factor hardware that dispenses uv resin for on demand manufacturing of pins, keychains, and similar small items. plugs into a creator's existing storefront as a backend fulfillment layer. this is the commercial, vc shaped branch.

do not treat these as phases of one roadmap. they have different goals and different audiences. and do not merge branch 1 and branch 2 just because they are both software, they solve different problems, one is analysis, the other is visualization.

---

## critical distinctions between the two hardware branches

wall arm:
- vertical, articulated, moves up down and sideways
- wall mounted or large format
- uses real brushes and real paint, acrylic, gouache, watercolor, oil
- wet medium physics is a central unsolved problem: paint load decay, bristle spring back, wet on wet bleeding, re-dip timing
- not commercially viable, grant and engineering shaped

resin plotter:
- small, tabletop, regular pen-plotter scale
- dispenses uv resin, which does not dry until deliberately cured with uv light
- almost entirely sidesteps wet medium physics, since resin does not evaporate or thicken the way paint does
- commercial product, plugs into shopify, squarespace, or instagram shop as backend fulfillment, uses something like stripe connect to trigger manufacturing only after a real paid order exists
- solves a real problem: artists currently need a kickstarter or a bulk minimum order (commonly around 20 units) before anything gets made, which creates speculative inventory and eventual waste through clearance sales. resin plotter removes that entirely.

do not apply wall arm's wet medium physics research to the resin plotter branch. do not apply resin plotter's commercial storefront framing to the wall arm branch. they solve different problems for different reasons.

---

## the engine branch and the browser sim branch

the engine is the research work behind analyzing artwork and producing stroke or fill level instructions. it is not owned by either hardware branch. it should stay hardware agnostic and headless, no ui code belongs here.

the browser sim is a separate branch. it is a visualization tool that consumes stroke plans the engine produces and animates them on an html canvas, live, stroke by stroke, so a human can verify the output without hardware. it borrows from open source pen plotter tools like vpype as a starting point for the rendering side. see browser-sim.md.

current milestone: the crosshatch beta, which spans both branches. it is staged into two versions, and this staging matters. version one takes an image that is already crosshatched and the engine only detects and vectorizes the existing linework, it does not decide where hatching goes, this keeps version one strictly deterministic and reinforces the no-reimagining principle above. version two, which comes later, takes a plain image and has the engine actually decide hatching placement and density. do not build version two before version one's detection and playback pipeline works end to end. see crosshatch-beta.md for both algorithms in detail.

do not assume the engine's stroke plan format (see stroke-plan-format.md) works for the resin plotter branch. that format was designed around brush strokes. resin fill regions and color pours are a different vocabulary and likely need their own format.

---

## illustrace, important note

illustrace is a separate project by the same author. it is a software research benchmark for measuring and transferring visual style in illustrations, relevant mainly to the engine branch. it has its own parameter registry, transfer operators, and stylebench evaluation framework.

illustrace and inkblotter overlap by approximately 5 percent. the only realistic connection is that illustrace might someday hand the engine an image to study.

do not:
- suggest using illustrace's parameter registry to drive the engine's stroke planning
- propose deep api or json integration between the two projects
- frame inkblotter as illustrace's physical twin or executor

the user has explicitly corrected this framing multiple times.

---

## reference painting

a reference painting was provided at project start: a gouache-style landscape with a pink sunset sky, navy mountain silhouette, green rolling hills, and pink foreground foliage. it is an aspirational style target, most relevant to the wall arm branch and the engine's ingestion research. it is not a direct input to the crosshatch beta.

key properties relevant to the wall arm branch:
- opaque flat color regions, dry on dry, no wet on wet blending
- limited palette, 6 to 8 colors
- hard edges between regions
- minimal visible brushstroke texture
- gouache or gouache-like medium

this style is friendly for the wall arm branch's early milestones.

---

## repo structure note

engine, wall arm, and resin plotter should live as separate orphan branches, not nested under one another, and not treated as sequential phases. if you are helping structure the actual git repo, keep that separation literal, not just conceptual.

---

## files in this repo

| file | what is in it |
|---|---|
| readme.md | big picture overview |
| explain.md | full plain language walkthrough of the whole project, read this first if you want the complete picture without piecing it together from other files |
| engine.md | the research branch, ingestion and stroke planning |
| wall-arm.md | the large articulated arm painting branch |
| resin-plotter.md | the tabletop uv resin print on demand branch |
| crosshatch-beta.md | engine's first beta milestone, crosshatch in browser |
| browser-sim.md | html canvas live simulation spec, part of the engine branch |
| stroke-plan-format.md | wall arm's data contract, produced by the engine |
| wet-medium-physics.md | wall arm's core unsolved problem |
| brush-and-paint-types.md | wall arm material reference |
| open-questions.md | undecided decisions, across all branches |
| glossary.md | term definitions |
| for-agents.md | this file |
