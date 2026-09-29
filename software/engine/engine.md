# engine -- ingestion and stroke planning research (standalone branch)

this is its own orphan branch: a standalone research engine for studying and decomposing artwork into stroke-level or fill-level instructions.

this is software, but it is not the browser sim. they are two different branches. see the distinction section below before touching either one.

---

## why this is separate from everything else

the ingestion and planning problem, given an image, figure out how it was likely constructed (palette, regions, stroke direction, value structure, layer order) and turn that into a physical instruction set, is genuinely its own research field. it does not belong to the wall arm hardware and it does not belong to the resin plotter hardware.

both hardware worlds are potential consumers of this engine's output eventually, but neither owns it, and it should not be developed inside either hardware branch's context.

this branch is research heavy. progress here is measured in understanding, not in shipped hardware, and not in a visible demo.

---

## engine branch versus browser sim branch, these are different things

both are software, and it is easy to mentally merge them, but they are two separate orphan branches with separate goals:

the engine branch:
- the actual analysis and planning logic
- takes an image in, produces a stroke plan or fill plan out
- has no user interface, no visualization, no animation
- this is where the real research happens: palette extraction, region segmentation, value structure, stroke direction, texture analysis, layer ordering
- can be developed, tested, and validated entirely headless, just running against images and inspecting the json output

the browser sim branch:
- a visualization and testing tool, not the planning logic itself
- an html canvas app that takes a stroke plan (produced by the engine) and animates it being drawn, stroke by stroke, live
- exists so a human can visually verify the engine's output looks right, without needing any hardware
- see browser-sim.md for the full spec

the engine can be worked on and validated without the browser sim existing at all. the browser sim is a consumer of the engine's output, not a part of the engine itself. do not build planning logic inside the browser sim branch, and do not build canvas rendering or animation logic inside the engine branch.

---

## what lives in the engine branch specifically

- image analysis: palette extraction, region segmentation, value structure, stroke direction, texture
- the stroke plan format and its variants (see stroke-plan-format.md)
- the crosshatch beta's actual hatching algorithm, value extraction, region tiling, density mapping, direction assignment, path generation, stroke ordering (see crosshatch-beta.md for the algorithm, but note the algorithm itself belongs here, while the live animated demo of it belongs to the browser sim branch)
- future stroke language expansion: contour lines, fill strokes, stippling, painterly stroke planning
- the ingestion / study phase concept, where the system analyzes a reference artwork before planning any marks
- for the resin plotter branch specifically: flattening a design into ordered, curable color layers, which is a simpler ingestion problem than painterly stroke analysis

---

## relationship to the hardware branches

the engine produces plans. it does not know or care what physically executes them, and it does not know or care how those plans get visualized for a human either, that is the browser sim's job.

- the wall arm branch consumes stroke-plan-style output for brush and paint execution
- the resin plotter branch most likely needs its own output vocabulary, closer to fill regions than brush strokes, since resin behaves nothing like a loaded brush, but may share underlying analysis work (palette extraction, region segmentation) with this engine

this branch stays hardware agnostic. do not let wall arm brush physics or resin curing constraints leak into this engine's core design.

---

## structure note for the repo

engine and browser sim should be two separate top level branches or directories, not nested under either hardware branch, and not nested under each other. treat the engine the way you would treat a shared library that multiple products and tools might import from. the browser sim is one of those consumers, not a subdirectory of the engine.
