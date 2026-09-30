# design -- embroidery

## input format

simple designs: hearts, doodles, small decorative shapes, simple icons.
flat outlines and fill regions. no gradient, no complex layering.

engine job:
- trace outlines as stitch paths (back stitch or chain stitch)
- fill regions as satin stitch or fill stitch paths
- order paths to minimize needle travel distance

no brushstroke analysis. no layer ordering. no physics modeling.

## stitch path output format

the engine outputs:
- list of stitch positions in order (x, y coordinates)
- stitch type per segment (back stitch, satin stitch, fill stitch)
- thread color per segment
- travel moves (needle up, move, needle down)

simplest instruction format in the project.

## generative source (optional)

the p5.js sketch (kqrla/assets, ink.html) can feed this branch at small scale.
the lighter cream/pink palette suits embroidery especially -- soft flat color blocking
maps cleanly to simple fill regions. simple custom inputs (a heart, a doodle) also
work directly without the generative source.
