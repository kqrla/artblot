# tufting

tufting is the first commercial milestone for the wall arm. same physical hardware as the wet paint work, but a much simpler mechanical problem and a real commercial case on the other end.

see also: `hardware/wall-arm/wall-arm.md` for how tufting fits into the arm's two-phase roadmap.

---

## the mechanism

rug tufting works like this: backing cloth gets stretched and stapled onto a wooden frame, the frame stands upright, and a projector throws the design onto the cloth surface so the outlines are visible. a tufter traces those projected lines with a tufting gun loaded with yarn, building up pile as it goes region by region. once the design is fully tufted, the backing comes off the frame and the finished rug comes out.

this is the exact setup inkblotter's wall arm targets. the projector replaces hand-drawing the design onto the backing -- the engine produces the layout, the projector displays it on the cloth, and the arm traces it with the gun instead of a human hand. the frame stays up while the arm works, then gets dismantled when the piece is done. same workflow a human tufter uses, just with the arm doing the tracing step.

this maps cleanly onto what the arm already needs to do. the engine studies a design and produces an ordered set of instructions, except instead of stroke plans for a loaded brush, it produces traced regions for a tufting gun. no re-dip timing. no paint load decay. no bristle spring-back. no wet-on-wet bleeding. just needle depth, yarn tension, and loop height -- all mechanically simple and repeatable compared to a wet brush.

---

## prior art disclaimer

robotic tufting hardware itself is not new. commercial robotic tufting arms already exist (hitex, haochuan, and others), and hobbyists have already strapped ordinary tufting guns to robot arms for cnc-style textile art. none of that is the innovation here. the mechanism is proven prior art.

what is new is the pipeline feeding it: a generative source image, the engine tracing and sequencing it into instructions, and the on-demand commercial model wrapped around the output. do not frame the tufting mechanism itself as novel in any pitch material. the pipeline and the business model are the actual contribution.

---

## the generative source

designs for tufting come from a p5.js sketch (kqrla/assets, ink.html). it generates fresh voronoi compositions on every page refresh -- eight random seed points distorted through layered perlin noise for organic bleed edges, black ink borders where two cells meet closely, and two of the eight points get a concentric ring effect. four fixed palettes total, one picked per run.

the sketch is entirely client-side. no server, no model, no inference cost, no training data, no copyright exposure. refresh the page, get a new design.

this gives the collection a specific commercial shape: infinite unique pieces that still clearly belong together because the palette stays fixed. one-of-a-kind but cohesive. that is a much easier sell than either fully identical repeats or completely unrelated one-offs.

nobody has to manually design every rug. the sketch generates the composition, the engine turns it into tufting instructions, the arm executes it.

---

## why this is not ai art

the p5 sketch is deterministic computational code, not a trained model or diffusion pipeline. three things this means in practice:

- no ethical controversy -- no training data, no scraped-art issue
- no inference cost -- single html file, runs in browser, instant output
- no fine-tuning or training pipeline to maintain

this is distinct from the engine's "not ai art" positioning (which covers the engine's lack of diffusion models). the generative source is separately clean on all three fronts.

---

## why tufting first (not wet paint)

tufting proves out arm motion control and engine instruction following without also having to solve wet medium physics at the same time. once tufting is working, the arm has already demonstrated it can move accurately, follow traced paths, and consume engine output reliably. wet paint adds the hard layer -- re-dip timing, paint load decay, bristle spring-back, wet-on-wet bleeding -- on top of a foundation that's already proven.

---

## commercial case

rugs have real perceived value tied to material and labor rather than artist name. a painting's value leans heavily on who made it -- a robot obviously does not have a name that sells. a rug's value leans on how it looks and feels, which the arm can deliver directly.

**comparable:** arcade.ai raised $25m series a for ai-designed custom rugs made by human artisans. that validates the market. the difference is cost structure -- arcade uses human artisan labor (expensive, slow, caps throughput). inkblotter's pipeline replaces that labor step with the arm executing engine instructions directly. lower marginal cost, scalable throughput, no human-in-the-loop bottleneck.

---

## comparable tools and prior art to watch

- **tuftnano** (tuftnano.com) -- uses segment anything model to isolate color regions, depth anything v2 to generate grayscale depth map controlling pile height (white = ~40mm pile, black = flatweave ~10mm). closed source, clean-room caution applies. the depth-channel idea is interesting for a future pile-height feature but is explicitly deferred -- not in current scope.
- **nedgraphics tuft premium** -- industrial CAD, 3D viewer, u3m file format, realistic yarn parameter library
- **tuftlab.app / thetuftingcalculator.com** -- free browser tools, image-to-yarn-color and yardage breakdown per color region
- **pointcarre.com** -- industrial tuft CAD, compatible with commercial tufting machines (CMC, tuftco, vandewiele, modra)

---

## milestones

- [ ] arm can hold a tufting gun and follow a projected traced line on the backing
- [ ] arm can accept traced region instructions from the engine and execute tufted passes
- [ ] arm can complete one full tufted piece from a p5 generative source image end to end
- [ ] first sellable tufted rug produced and shipped
