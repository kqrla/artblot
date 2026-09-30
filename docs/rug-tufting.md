# rug tufting

this is the second commercial front and the first milestone for the wall arm hardware. same physical arm as the wet paint work, completely different and much simpler problem.

---

## what it is

rug tufting: backing cloth stretched and stapled onto a wooden frame, frame stands upright. a projector maps the engine's design directly onto the cloth surface -- projection mapped to the frame geometry so the layout lands correctly regardless of how the frame is positioned. the arm traces those projected regions with a tufting gun loaded with yarn, building up pile region by region. once done, the frame gets dismantled and the finished rug comes out.

the key word is projection mapping, not just "a projector." the engine's output gets mapped to the physical surface of the frame, accounting for the cloth position and geometry. this is what lets the arm trace accurately without the design distorting across the surface.

---

## why this comes before wet paint

tufting removes the entire wet medium physics problem. no:
- paint load decay
- bristle spring-back
- wet-on-wet bleeding
- re-dip timing
- medium viscosity differences

just needle depth, yarn tension, and loop height -- all mechanically simple, predictable, and repeatable. the arm proves it can follow traced paths and consume engine instructions accurately, without also solving the hardest part of the project at the same time.

---

## why this has a commercial case (unlike wet paint)

wet paint on canvas does not displace real labor in any meaningful way. human painters are better and cheaper. that branch exists purely as an engineering pursuit.

tufting does displace real labor. a human tufter has to trace each design by hand, run the gun pass by pass, and do the whole thing manually every time. the arm replaces that tracing step with machine execution of engine instructions.

**perceived market value:** rugs command real prices based on visible labor and material -- yarn, backing, the density of the pile. that value is not tied to the artist's name, unlike painting. a robot-tufted rug can carry the same perceived material value as a hand-tufted one.

**market comparable:** arcade.ai raised a $25m series a for ai-designed custom rugs made by human artisans. that validates the demand. their cost structure still has a human artisan making each rug by hand -- expensive, slow, hard to scale. inkblotter's version replaces that labor step with the arm executing math. lower marginal cost per rug, scalable throughput, no artisan bottleneck.

**pricing:** paintings sell for what the artist's name is worth. rugs sell for what the material and labor is visibly worth. this is a better market for a robotic output.

---

## the generative source

designs come from a p5.js sketch (kqrla/assets, ink.html):
- voronoi cells from 8 random seed points
- distorted through layered perlin noise for organic bleed edges
- black ink borders where cells meet closely
- 2 of 8 points get a concentric ring effect
- 4 fixed palettes, one picked per run

every page refresh generates a new unique composition. the palette stays fixed across runs, so every piece is one-of-a-kind but the collection is cohesive. that is the commercial shape: not fully identical (boring) and not completely unrelated (impossible to sell as a collection). one-of-a-kind within a controlled family.

nobody manually designs every rug. the script generates the composition, the engine traces it into tufting region instructions, the arm executes it.

### why this is not ai art

the sketch is deterministic computational code, not a trained model. three things this means:
- no ethical controversy -- no training data, no scraped art
- no inference cost -- single html file, runs in browser, instant output on every refresh
- no fine-tuning or training pipeline to maintain

this is distinct from arcade.ai's pipeline, which uses an ai design chatbot (inference cost, training data, copyright exposure). inkblotter's design side has none of that.

---

## the engine's job for tufting

tufting is close to the simplest version of the engine's whole problem space:
- extract flat color regions from the generative source image
- order them for the arm to trace (no layering, no order-of-operations complexity like resin curing)
- output traced region instructions with needle depth and yarn tension parameters

no brushstroke analysis. no paint behavior modeling. flat regions, ordered paths. the engine can tackle this before the harder stroke-planning work for wet paint.

---

## prior art disclaimer

robotic tufting hardware is not novel. commercial robotic tufting arms already exist (hitex, haochuan). hobbyists have strapped tufting guns to robot arms for cnc-style textile work. the mechanism is proven prior art.

the actual contribution here is the pipeline: generative source design → engine tracing → robotic execution → on-demand commercial output. do not pitch the tufting mechanism itself as novel in any pitch material.

---

## comparable tools

- **tuftnano** (tuftnano.com) -- uses segment anything model for color region isolation, depth anything v2 for grayscale depth map controlling pile height (white = ~40mm, black = ~10mm flatweave). closed source, clean-room caution applies. the depth-to-pile-height idea is interesting for a future sculptural pile feature but explicitly deferred -- not in current scope.
- **nedgraphics tuft premium** -- industrial cad, 3d viewer, u3m format, realistic yarn parameter library
- **tuftlab.app / thetuftingcalculator.com** -- free browser tools, image-to-yarn-color and yardage breakdown per color region
- **pointcarre.com** -- industrial tuft cad, compatible with commercial tufting machines (cmc, tuftco, vandewiele, modra)

---

## milestones

- [ ] projection mapping pipeline: engine output maps correctly onto physical tufting frame surface
- [ ] arm can hold a tufting gun and follow a projection-mapped traced region
- [ ] arm can accept full region instruction set from engine and execute tufted passes for a complete design
- [ ] one full tufted rug produced end to end from a p5 generative source image
- [ ] first sellable rug shipped

---

## deferred / v2 ideas

- **pile height channel:** the p5 sketch could output a depth channel alongside color. the engine would translate that into pile height variation (sculptural carved look vs flat uniform pile). inspired by tuftnano's depth anything approach. flagged for v2, not in current scope -- flat pile first.

---

## see also

- `hardware/wall-arm/wall-arm.md` -- arm hardware, wet paint phase, overall two-phase structure
- `hardware/wall-arm/embroidery/embroidery.md` -- desktop embroidery machine, comes before tufting in the build order
- `software/engine/engine.md` -- the planning engine that feeds this branch
- `roadmap.md` -- full build order across all fronts
