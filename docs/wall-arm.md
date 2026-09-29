# wall arm -- large vertical articulated arm (engineering and tufting branch)

this branch covers the large scale physical hardware: a vertical, articulated arm that moves up, down, and sideways. it does two things, on the same hardware, in two phases: tufted rugs first, wet paint second.

this used to be described as track a plus track b together. it is now understood as one of two hardware branches, distinct from the resin plotter branch (track c).

---

## positioning

wet paint painting is not commercially viable on its own, and it is worth being completely honest about why: it is not solving any problem. a human painter is better and cheaper at painting than this arm will ever be. this branch does not displace labor, does not reduce cost, and is not optimizing anything that was inefficient before. it exists purely because wet medium physics is a hard, unsolved, genuinely interesting engineering and mathematical problem, worth pursuing for its own sake, the same way an interesting math problem does not need to solve anything to be worth solving. it is suited toward hardware grants, engineering competitions, and research credibility, not toward vc funding or a storefront business model, and it should never be pitched as solving a labor or efficiency problem, because it is not.

tufting and embroidery are different. both replace real human labor with a machine, so both carry a genuine cost and efficiency argument on top of being easier engineering problems than wet paint. they come first, both because they are mechanically simpler and because they are the pieces of this work that actually have a commercial case. see below for tufting, and see embroidery.md for the smaller desktop scale embroidery branch.

---

## what lives here

- hardware: articulated arm or gantry, moves in three dimensions, wall mounted or large format
- brush holding, switching, and calibration (see hardware notes below)
- wet medium physics: paint load decay, bristle spring-back, wet-on-wet bleeding, re-dip behavior
- paint types: acrylic, gouache, watercolor, oil (see brush-and-paint-types.md)
- consumes stroke plans produced by the engine branch (see engine.md and stroke-plan-format.md)

---

## tufting comes first

before this arm ever touches wet paint, it should learn to tuft yarn. same physical hardware, same articulated arm, much easier problem, and there is a real commercial path on the other end of it.

rug tufting works like this: backing cloth gets stretched and stapled onto a wooden frame, the frame stands upright, and a projector throws the design onto the cloth surface so the outlines are visible. a tufter traces those projected lines with a tufting gun loaded with yarn, building up pile as it goes region by region. once the design is fully tufted, the backing comes off the frame and the finished rug comes out.

this is the exact setup inkblotter's wall arm targets. the projector replaces hand-drawing the design onto the backing -- the engine produces the layout, the projector displays it on the cloth, and the arm traces it with the gun instead of a human hand. the frame stays up while the arm works, then gets dismantled when the piece is done. same workflow a human tufter uses, just with the arm doing the tracing step.

this maps cleanly onto what the arm already needs to do. the engine studies a design and produces an ordered set of instructions, exactly like it does for painting, except instead of stroke plans for a loaded brush, it produces traced regions for a tufting gun. no re-dip timing. no paint load decay. no bristle spring-back. no wet-on-wet bleeding. just needle depth, yarn tension, and loop height, all of which are mechanically simple and repeatable compared to a wet brush.

robotic tufting hardware itself is not new. commercial robotic tufting arms already exist (hitex, haochuan, and others), and hobbyists have already strapped ordinary tufting guns to robot arms for cnc style textile art. none of that is the innovation here. the mechanism is proven prior art. what is new is the pipeline feeding it: a generative source image, the engine tracing and sequencing it into instructions, and the on demand commercial model wrapped around the output. do not frame the tufting mechanism itself as novel in any pitch material, the pipeline and the business model are the actual contribution.

that is the whole point of doing tufting first. it proves out the arm's motion control, its ability to follow a traced path accurately, and its ability to consume instructions from the engine, without also having to solve wet medium physics at the same time. wet paint stays the harder milestone for later, once the arm has already proven it can move accurately and reliably.

### why tufting also has a commercial case, not just an engineering one

unlike large scale robot painted canvas, a tufted rug is a real object people actually buy to furnish a room with, and the market already pays real prices for rugs based on visible labor and material, not on an artist's name. a painting's value leans heavily on who made it, which a robot obviously does not have. a rug's value leans on how it looks and feels, which the arm can deliver directly.

the market for custom generative rugs is already proven, not hypothetical. arcade.ai raised a 25 million dollar series a doing exactly this category: an ai design chatbot generates a custom rug, and a human artisan then makes it by hand, sold through their own marketplace. that validates real paying demand for one of a kind generative rug designs.

the difference is the cost structure. arcade's model has a human artisan actually making each rug, which is expensive, slow, and caps throughput by however many artisans are available. inkblotter's tufting pipeline replaces that human labor step with the robotic arm executing the engine's traced instructions directly, math, not manual craft. that means the marginal cost per rug is much lower and the pipeline can scale in a way a human-in-the-loop model structurally cannot.

tufting also skips the layering and ordering complexity that resin curing has. it is close to the simplest version of this whole pipeline: the engine traces flat projected regions, the arm follows them with yarn, done. no order-of-operations problem, no curing steps, no drying time. cheaper and faster for the engine to plan for, and cheaper and faster for the arm to execute.

### the generative source images

designs for tufting do not have to be manually created one at a time. a p5.js sketch generates a fresh composition every time the page is refreshed, so every output is unique, while the color palette stays fixed and controlled. the result is a family of one-of-a-kind pieces that still clearly belong together, which matters commercially: infinite unique pieces that still feel like a cohesive collection are a much easier sell than either fully identical repeats or completely unrelated one-offs.

the sketch currently has two distinct fixed palettes it can draw from, not one continuous palette, two separate defined color sets. the reference image reviewed earlier in this project was one output using one of those two palettes. source: a single self contained html file, ink.html, in the kqrla/assets repository on github, built as a codepen originally (codepen.io/hollandblumer). the whole thing runs client side in the browser, no server, no build step, and produces a new composition on every refresh.

this also means nobody has to sit down and manually design every single rug. the script generates the composition, the engine turns it into tufting instructions, the arm executes it.

this is also why the generative side of this is not ai art either. p5.js is a computational graphics tool, not a generative model. the sketch is deterministic code, rules, randomness seeded and controlled by the programmer, mathematical composition, not a neural network trained on other people's art and sampling from a learned distribution. nothing is generated from a prompt and nothing is trained on scraped images. every composition comes from explicit code the programmer wrote, same as a fractal generator or a physics simulation. that keeps the generative source images consistent with the rest of inkblotter's position: computation and math throughout, never a generative model anywhere in the pipeline.

---

## hardware options (undecided)

### gantry / cnc-style
- moves on x/y axes, brush on z
- very precise, well-understood mechanically
- limitation: fixed orientation, cannot do angled strokes easily without rotating the brush mount

### vertical wall plotter (axidraw style)
- hangs on a wall, two-motor belt system
- compact, can work on large surfaces
- less rigid than gantry, wobble can actually add expressiveness

### robot arm (6-dof, articulated)
- most expressive, can change brush angle, apply side pressure, flick wrist
- moves up, down, and sideways, not fixed to a single plane
- hardest to calibrate and program
- closest to how a human paints, and the defining hardware shape of this branch

### custom rig
- build something novel around the specific constraints of brush switching and paint dipping

---

## brush holding and switching

a pen plotter just clamps a pen, the pen does not wear, does not need rinsing, does not change shape. a brush holder needs to grip firmly without crushing the ferrule, release and re-grip a different brush, and handle different handle diameters.

options: magnetic quick-release, collet chuck, fixed slots in a carousel.

---

## paint delivery and re-dip

a brush loaded with paint runs dry after some number of strokes. approaches to knowing when to re-dip: time or stroke-count heuristic, pressure sensor on the brush mount, vision feedback watching for dry marks, paint-specific decay curves.

---

## paint types (initial scope)

| paint type | viscosity | drying time | bleed risk | notes |
|---|---|---|---|---|
| acrylic | medium-high | fast (minutes) | low | forgiving, common, good for v1 |
| gouache | medium | medium | low-medium | opaque, re-wettable, used in reference painting |
| watercolor | thin | variable | high | hardest, very sensitive to water ratio |
| oil | thick | very slow (days) | low | richest but impractical early on |

recommended starting point: acrylic.

---

## surface and canvas

| surface | tooth | absorbency | notes |
|---|---|---|---|
| watercolor paper (300gsm) | medium | high | forgiving, will not buckle |
| canvas board | medium | low-medium | standard painting surface |
| illustration board | low | low | smooth, great for precise strokes |
| plain printer paper | very low | medium | cheap for testing only |

recommended starting point: illustration board.

---

## milestones

### phase one: tufting (comes first, proves motion control)

- [ ] arm can move in a controlled path across three axes
- [ ] arm can hold a tufting gun and follow a traced line
- [ ] arm can accept traced regions from the engine and execute them as tufted passes
- [ ] arm can complete one full tufted piece from a generative source image
- [ ] first sellable tufted rug produced end to end

### phase two: wet paint (comes after, the harder problem)

- [ ] arm can hold one brush and make a mark
- [ ] arm can re-dip at a fixed interval
- [ ] arm can switch between two brushes
- [ ] calibration data collected for one brush plus one paint plus one surface combo
- [ ] calibration library started (stroke length vs paint load vs mark quality)
- [ ] can accept a stroke plan from the engine branch and execute it with predictable results

see wet-medium-physics.md and brush-and-paint-types.md for the deeper material research behind this branch.
