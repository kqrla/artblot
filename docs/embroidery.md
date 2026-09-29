# embroidery -- small desktop scale stitching (commercial branch)

this branch covers a small, desktop scale machine, closer in size to a regular pen plotter than to the wall arm, that produces embroidered thread designs on dissolvable stabilizer backing.

---

## the mechanism

embroiderers already use a water soluble stabilizer backing: you stitch through it, and once the stitching is done, the backing dissolves away in water, leaving only the stitched threadwork behind, fully self supporting.

this machine automates the stitching step. instead of an articulated arm dipping a brush or a tufting gun punching yarn through cloth on a frame, this is a small desktop plotter with a needle mechanism that pokes thread through the dissolvable backing along a traced path, closer to a scaled down tufting motion than to anything the wall arm does.

no wet medium physics at all. no paint, no resin curing, no re-dip timing, no bristle spring back. the mechanical problem here is close to the simplest one in the whole project: needle position, stitch length, thread tension, and path order.

note: embroidery is not the same thing as cross-stitch. cross-stitch is one specific technique, small x shaped stitches locked to a fixed grid, closer to pixel art in thread. embroidery is the broader craft, most of it free form stitched lines and shapes with no grid at all. this branch is general embroidery, tracing arbitrary paths, not grid locked cross-stitch. also not the same thing as tufting or quilting: tufting punches loops of yarn through backing cloth to build up a pile surface, like a rug. quilting stitches together layers, a decorative top layer, padding, and a backing layer, to join them into something like a blanket. embroidery here is thread stitched along traced paths on dissolvable backing, a different mechanism from all three.

---

## stitch types

embroidery is not one single stitch technique, the outline and the fill typically use different stitches.

for outlines and line work:

- back stitch -- a simple continuous line, the most basic outline stitch
- stem stitch -- a slightly twisted, rope-like line
- chain stitch -- looks like a small linked chain, common for bold lettering and doodle style line art

for filling solid areas:

- satin stitch -- straight parallel stitches packed tightly to fully cover a shape, gives the smooth, slightly shiny filled look seen on small patches and badges
- fill stitch (also called tatami stitch) -- a woven grid style fill
- french knots -- small raised dots, used for fine detail like eyes or texture

given the scope of this branch, small doodles and simple designs on dissolvable backing, the most relevant stitches are likely back stitch or chain stitch for outlines, and satin stitch for any small filled areas. the engine's planning job for this branch is mostly about tracing a path and picking a stitch type per region, not the full stitch vocabulary above.

---

## why this is likely the cheapest branch

- desktop footprint, similar scale to the resin plotter, not the large wall arm
- no wet medium anything, so none of the calibration or material research the wall arm needs
- dissolvable stabilizer backing is cheap and already commercially available, nothing custom to source
- designs do not need to be complex. most embroidery people actually buy or make is simple: hearts, small doodles, basic line marks, not elaborate compositions
- because designs are mostly simple line work, the engine's planning job here is close to the crosshatch beta in complexity, line tracing and stitch path ordering, no color region extraction, no layering, no curing order

---

## why this has a real commercial case

same reasoning as tufting: this genuinely displaces human labor and lowers cost, rather than being a pure engineering exercise. small embroidered goods, patches, small hoop art, simple appliques, are already something people buy, and a machine that stitches on demand from a generative or custom design removes the same speculative manufacturing problem the resin plotter removes: nothing gets made until a real order exists.

---

## relationship to the other branches

this is not the wall arm at a smaller scale. it is closer in spirit and in hardware scale to the resin plotter: small desktop machine, commercial branch, engine's planning job is comparatively simple. it should be treated as its own branch, not a feature of either existing hardware branch.

status: early idea stage, not yet scoped in detail. flagged here so the idea is not lost, not yet a committed branch with milestones.
