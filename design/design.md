# design -- rug tufting

## generative source

designs come from a p5.js sketch (kqrla/assets, ink.html).

- voronoi cells from 8 random seed points
- distorted through layered perlin noise for organic bleed edges
- black ink borders where cells meet closely
- 2 of 8 points get a concentric ring effect
- 4 fixed palettes, one picked per run

every page refresh produces a new unique composition. palette stays fixed across runs --
each piece is one-of-a-kind but the collection is cohesive. that is the commercial shape.

## not ai art

the sketch is deterministic computational code, not a trained model:
- no ethical controversy -- no training data, no scraped art
- no inference cost -- single html file, browser only, instant on every refresh
- no fine-tuning or training pipeline to maintain

## engine job for tufting

- extract flat color regions from the generative source image
- order regions for efficient arm traversal
- output traced region instructions: path, needle depth, yarn tension, loop height

flat regions and ordered paths only. no brushstroke analysis, no paint modeling.
closest to the simplest version of the engine's problem space.

## palette notes

4 palettes in the sketch. the lighter cream/pink palette suits tufting especially well --
its flat soft color blocking maps cleanly to discrete yarn regions with minimal
transition complexity. palettes can be locked per production run for collection cohesion.
