# explain

this file exists so you never have to go back through a chat history to understand what inkblotter is or why it works the way it does. if you read one file, read this one.

---

## where this started

i wanted a robot that could actually paint.

not a pen plotter. not a cnc machine. not axidraw. those all exist and are fine at what they do. they deposit ink or drag a cutting head. consistent. predictable. completely solved territory.

a paintbrush is not consistent. a paintbrush is not solved.

when you load a real brush with real paint and make a stroke, the paint runs out as you go. the bristles bend under pressure and spring back when you lift. if you stroke into wet paint that's already on the canvas, they bleed together. you have to know when the brush is running dry. you have to know when to go back to the palette and re-dip. when you switch brushes mid-painting, the new brush has different spring, different load, different behavior.

none of the existing robot platforms deal with any of that. they sidestep it entirely by using pens or cutters.

that gap -- wet medium physics -- is the actual reason this project is interesting. that is the unsolved part. everything else is scaffolding around it.

---

## why it split into four pieces

once i started thinking through what it would take to actually build this, it became obvious that several genuinely different problems were getting bundled together, and that bundling them would force bad compromises.

the problem of "analyze an image and turn it into physical instructions" is a research problem. it does not care what machine executes the result. it should stay hardware agnostic.

the problem of "can i verify my stroke plan looks right before i have any hardware" is a visualization problem. it does not need to plan anything. it just needs to replay what the planner already decided.

the problem of "physically solve wet medium physics on a large format vertical machine" is a hardware engineering problem. it is interesting and hard and worth pursuing, but it is not a business.

the problem of "manufacture enamel-style pins on demand for an independent creator's storefront" is a commercial product problem. it has a market, a business model, and a path to revenue.

these four problems have different audiences, different success criteria, and in the hardware cases, completely different machines. keeping them as separate orphan branches with no shared code or shared context is not over-engineering. it's just honest about what they actually are.

---

## the engine

the engine is the analytical brain.

given a reference image, it figures out what is in it: the palette, the regions, the value structure, the kinds of marks that were used to build it, and the order those marks were probably applied in. it turns that understanding into a set of physical instructions -- a stroke plan -- that a downstream machine can execute.

it has no ui. it does not touch hardware. it does not care whether the downstream machine is the wall arm or the resin plotter or something else. it is purely analytical.

the first concrete milestone for the engine is called the crosshatch beta. crosshatching -- parallel lines at different densities and angles to represent light and shadow -- was chosen as the starting point because it reduces the engine's output to the simplest possible thing: a list of line segments. any basic plotter can execute line segments. and you can tell from across a room whether the result looks right.

that milestone is split into two very different versions on purpose.

version one: the engine takes an image that already contains crosshatched artwork. it finds the existing lines, traces them, and produces an ordered list of strokes. it invents nothing. it just detects and serializes what is already there. this is a deterministic tracing problem, not a planning problem.

version two: the engine takes a plain image with no hatching, and has to decide where the hatching lines should go, at what density, at what angle. this is a genuine research problem. putting more lines in darker areas does not automatically look like shading a human would draw. a convincing result needs to account for where the actual form changes -- not just where pixels happen to be dark -- which direction implies volume rather than a flat surface, and how a human hatcher's lines would follow the shape of what they are drawing. version two needs its own research phase before it becomes code.

---

## the browser sim

a separate branch from the engine, even though both are software.

the browser sim takes a stroke plan that the engine already produced and plays it back on an html canvas, stroke by stroke, live, so you can watch the image build up and catch anything that looks wrong, without any hardware existing yet.

it does not plan. it does not make decisions about where strokes go. it replays.

watching it might look a bit like watching a diffusion model gradually resolve an image from noise. the mechanism has nothing in common. diffusion is sampling probabilistically at each step, making slightly different choices every run, never faithfully reproducing the input. the browser sim is deterministically reading a fixed ordered list of coordinates and animating them in sequence. feed it the same input twice and it produces the identical result, pixel for pixel, every time.

---

## the wall arm

the big, ambitious machine.

a large, vertical, articulated arm that physically holds real brushes, dips them in real paint, and paints on a wall-mounted canvas. this is where wet medium physics -- the original unsolved problem -- has to get solved in actual hardware.

this branch is not commercially viable on its own. the machine would be expensive to build, expensive to run, and slow. it is worth pursuing because the engineering problem is genuinely hard and interesting and nobody has solved it. the right audience is engineering grants and technical credibility, not investors.

---

## the resin plotter

the actual business.

a small, tabletop machine, roughly the footprint of a regular pen plotter, that dispenses uv resin instead of ink. uv resin does not begin curing on its own. it stays liquid and workable at consistent viscosity until you deliberately hit it with uv light. so it never strings or trails behind the nozzle as it moves. what you dispense is what you get.

once you cure a layer, it is completely locked in. you can dispense another layer on top with no bleeding, no blending, no timing pressure. this is roughly how procreate layers work, except the layers are physical objects instead of pixels.

the designs that come out of this machine are enamel-style: flat color regions, hard edges, no painterly gradients. pins, keychains, and similar small goods. the engine's job for this branch is closer to "identify color regions and figure out a sensible curing order" than "analyze brushstroke technique." it is a simpler analytical problem.

the commercial model is the interesting part.

right now, if an independent artist wants to sell enamel pins:

- they run a kickstarter against demand that has not actually happened yet, or
- they commit to a bulk manufacturing minimum -- often something like 20 units -- before a single one has sold

either way they are manufacturing speculative inventory. if it doesn't sell through, it becomes stale stock, then a clearance sale, then waste.

the resin plotter removes the speculative step entirely.

```
customer places a real order
    ->
payment processes through stripe connect
    ->
machine manufactures one item
    ->
done
```

nothing is made until a real paid order exists. no upfront capital from the artist. no minimum order quantity. no leftover inventory. no clearance. nothing manufactured that wasn't already sold. it plugs into whatever storefront the creator already uses -- shopify, squarespace, instagram shop -- as a backend fulfillment layer they never have to think about.

---

## this is not ai art

it would be easy to hear "image analysis plus robots that make marks" and file this under the same bucket as generic ai image generators. it is specifically not that.

no prompts. no generation. no reimagining.

the engine studies an existing reference image to figure out how to physically reproduce it. the output is always a physical object -- real paint on a canvas, or real cured resin formed into a pin -- never a rendered image. what goes in is what comes out the other end, as faithfully as the hardware allows.

this is the exact opposite of how diffusion models work. a diffusion model given a reference image still adds its own sampling noise and stylistic drift. the output is a new generated image inspired by the reference, not a reproduction of it. that stylistic drift is the whole mechanism -- the model is never actually trying to reproduce its input, it is generating something new.

inkblotter does not do that. not in the engine, not in the browser sim, not in either hardware branch. the engine's job is to understand and reproduce, not to generate and interpret. whatever image or design is handed in is what gets made. zero creative interpretation anywhere in the pipeline.

if a diffusion model was used upstream to create an image, and that image is then handed to inkblotter, inkblotter treats it as a static reference to study and physically recreate. no generation happens inside inkblotter.

---

## illustrace, briefly

illustrace is a separate project: a software research benchmark for measuring and transferring visual style in illustrations. it overlaps with inkblotter by roughly 5%, limited to the possibility that illustrace might someday hand the engine an image to analyze. inkblotter does not use illustrace's internal architecture, its parameter registry, or its transfer operators.

---

## where to go from here

- engine.md -- the analysis and planning brain
- browser-sim.md -- the visualization tool
- wall-arm.md -- the large painting machine
- resin-plotter.md -- the tabletop manufacturing machine and the commercial model
- crosshatch-beta.md -- the engine's first milestone, explained in full
- for-agents.md -- if you are an ai agent or a new collaborator joining the project
