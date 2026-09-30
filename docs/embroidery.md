# embroidery -- concept and commercial case

## what it is

a small desktop machine, pen-plotter in footprint, that stitches thread through
dissolvable stabilizer backing via a needle-poke mechanism. once the design is fully
stitched the backing dissolves and the embroidered piece comes out.

mechanically closer to a scaled-down version of tufting than to anything on the wall arm.
no wet medium physics anywhere in the process.

## why this comes first

cheapest hardware to build. smallest footprint. simplest mechanics. no paint, no resin,
no yarn tension calibration at scale. the engine job here is close to crosshatch-beta
complexity -- outline paths and fill regions, not stroke analysis or layer ordering.

it proves the full pipeline end to end at the lowest possible risk before anything harder.

## commercial case

embroidery displaces real human labor. the machine replaces hand stitching with machine
execution of engine-generated stitch paths.

design inputs are simple: hearts, doodles, small decorative shapes. the engine job stays
simple too -- outline paths and basic fill stitches only.

on-demand model: nothing gets made until a real order comes in. no speculative inventory,
no minimum order quantity, no clearance risk.

## not the same as cross-stitch

cross-stitch is grid-locked -- every stitch is an X on a counted grid.
embroidery is free-form -- the needle follows any path the engine outputs.
this branch does embroidery. the engine outputs arbitrary paths, not grid coordinates.

## not the same as tufting or quilting

- tufting: loops punched through backing to create a pile surface (rugs)
- quilting: stitching multiple fabric layers together (blankets)
- embroidery: thread stitched through a single backing for surface decoration

## stitch vocabulary

this branch needs:

outlines:
- back stitch -- most common outline stitch, clean and solid
- chain stitch -- slightly raised outline, good for visible borders
- stem stitch -- rope-like outline, good for curves

fills:
- satin stitch -- parallel lines filling a region, smooth surface
- fill/tatami stitch -- rows of running stitches for larger regions
- french knots -- raised dot texture for small detail fills

the engine outputs mostly back stitch or chain stitch outlines plus satin stitch fills.
nothing more complex is needed for hearts, doodles, and simple decorative shapes.
