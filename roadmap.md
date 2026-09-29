# roadmap

this is the build order. it is not a strict waterfall -- the commercial fronts and the engineering track run in parallel, decoupled from each other. but within the commercial side, there is a clear sequence based on cost and mechanical complexity.

---

## the two tracks

**commercial track** -- embroidery, then tufting, then resin plotter. each one proves out the engine and motion control at increasing complexity before the next. all three displace real labor and have a genuine commercial case.

**engineering track** -- the wall arm's wet paint work. runs independently, not commercially motivated, not blocked by the commercial track and does not block it. suited toward grants and research credibility. starts whenever the arm hardware exists, regardless of where the commercial track is.

these two tracks do not depend on each other. the engine and browser sim serve both.

---

## commercial track: build order

### front 1 -- embroidery (first)

**why first:** cheapest hardware to build, desktop footprint, no wet medium physics at all. the mechanical problem is close to the simplest in the whole project: needle position, stitch length, thread tension, path order. the engine's planning job here is close to the crosshatch beta in complexity.

**what it proves:** that the engine can trace a design into machine instructions and a physical machine can execute them accurately enough to produce a sellable output. motion control, path following, and the commercial on-demand model all get validated here before anything harder.

**the output:** small embroidered goods on dissolvable stabilizer -- patches, small hoop art, simple doodles. nothing gets manufactured until a real paid order exists.

milestones:
- [ ] machine can hold a needle and follow a traced path through dissolvable backing
- [ ] engine can produce a stitch path from a simple design (outline + fill)
- [ ] one complete embroidered piece produced end to end from a generative or custom input
- [ ] first sellable embroidered piece shipped

see: `hardware/wall-arm/embroidery/embroidery.md`

---

### front 2 -- tufting (second)

**why second:** mechanically proven (commercial robotic tufters already exist), but larger scale than embroidery and requires the physical tufting frame setup. easier than wet paint because there is no wet medium physics -- needle depth, yarn tension, and loop height are all mechanically simple and repeatable.

**what it proves:** that the arm can work at large format scale, follow projected regions accurately across a tufting frame, and execute the engine's traced region instructions for a full-size rug.

**the setup:** backing cloth stretched on a frame, projector throws the engine's layout onto the backing, arm traces it with a tufting gun, frame dismantled after.

**the generative source:** a p5.js sketch (kqrla/assets, ink.html) generates fresh voronoi compositions on every refresh from four fixed palettes. no ai, no server, no inference -- just deterministic code producing a new design each run. the engine traces the generated color regions into tufting instructions.

**the commercial case:** rugs have real perceived value tied to material and labor rather than artist name. arcade.ai validated the market (raised $25m series a for ai-designed custom rugs made by human artisans). inkblotter's version removes the human artisan step and replaces it with the arm executing the engine's instructions directly -- lower marginal cost, scalable throughput.

milestones:
- [ ] arm can move across the tufting frame and follow a traced region
- [ ] arm can hold a tufting gun and execute tufted passes
- [ ] engine can produce tufting region instructions from a p5 generative design
- [ ] one complete tufted rug produced end to end
- [ ] first sellable rug shipped

see: `hardware/wall-arm/wall-arm.md` (tufting section)

---

### front 3 -- resin plotter (third, its own independent front)

**why third:** the resin plotter is a completely different machine from the wall arm -- small, tabletop, pen-plotter footprint. it does not depend on the wall arm's hardware being built or proven. it runs as its own front that happens to come after embroidery and tufting in the build sequence, but it is not waiting on them either.

**what it proves:** on-demand manufacturing of small hard goods (pins, keychains) at the lowest marginal cost per unit of any branch. uv resin's no-cure-until-lit property makes dispensing clean and layerable.

**the commercial model:** plugs into a creator's existing storefront as a backend fulfillment layer. one real paid order triggers one manufactured item. no speculative inventory, no minimum order quantity, no kickstarter.

milestones:
- [ ] machine can dispense uv resin along a traced path without stringing
- [ ] engine can produce layered fill region instructions from an enamel-style design
- [ ] one cured layer deposited and locked before the next
- [ ] one complete pin manufactured end to end
- [ ] first sellable pin shipped through a real storefront integration

see: `hardware/resin-plotter/resin-plotter.md`

---

## engineering track: wall arm wet paint (runs in parallel, decoupled)

wet paint painting is not commercially motivated and is not sequenced against the commercial track. it runs whenever the arm hardware exists and the engineering bandwidth is there. it exists because wet medium physics is a hard, unsolved, genuinely interesting engineering problem -- not because it will make money.

**the core problem:** a brush loaded with paint behaves completely differently from a pen. paint load decays as you stroke, bristles bend and spring back, wet paint bleeds into wet paint on the canvas, and the arm has to know when to re-dip. none of that is solvable by pen-plotter logic.

milestones:
- [ ] arm can hold one brush and make a mark
- [ ] arm can re-dip at a fixed interval
- [ ] arm can switch between two brushes
- [ ] calibration data collected for one brush + one paint + one surface
- [ ] calibration library started (stroke length vs paint load vs mark quality)
- [ ] can accept a stroke plan from the engine and execute it with predictable results

see: `hardware/wall-arm/wall-arm.md` (wet paint section), `hardware/wall-arm/wet-medium-physics.md`

---

## software (serves all tracks)

the engine and browser sim are not on either track -- they serve everything and run continuously alongside both.

**engine:** crosshatch beta is the first concrete milestone. version one traces existing hatched artwork. version two (later, harder) decides where hatching should go on a plain image. see `software/engine/`.

**browser sim:** visualizes the engine's output stroke by stroke before any hardware exists. deterministic replay, not sampling. see `software/browser-sim/`.

---

## why this order

- embroidery first: cheapest, smallest, proves the pipeline end to end with the least risk
- tufting second: same arm hardware as wet paint, de-risks motion control before adding wet medium complexity, has a real commercial case
- resin plotter third: its own machine, its own front, not waiting on tufting -- just comes later in the sequence because embroidery and tufting prove the engine and on-demand model first
- wet paint: runs whenever, decoupled, not commercially motivated, not a gate for anything else
