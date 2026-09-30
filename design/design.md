# design -- rug tufting

## generative source contract

the engine does not care what generated the source image. it cares about the output format:
- flat color regions with hard borders
- no gradients, no anti-aliased blends between regions
- black (or near-black) border lines separating regions
- a fixed palette per run (colors do not vary mid-image)

any generative source that fulfills this contract can feed the tufting pipeline.
the source is swappable.

## current reference implementation: ink.html

kqrla/assets/ink.html is the first example of a source that fulfills this contract.
it is not the only possible one, and it is not locked in.

what it does:
- 8 random voronoi seed points placed in a normalized coordinate space
- perlin noise distortion applied 3 times per pixel (the "ink bleed" -- organic edges)
- voronoi region assignment per pixel: closest seed point wins
- black border where second-closest and closest distances differ by less than 0.05 units
- first 2 of 8 points get the "russian doll" effect: concentric rings via
  floor(minDist * 12) % 2, alternating between region color and black
- remaining 6 points: flat solid color regions
- 4 fixed palettes, one picked randomly per run:
  - palette 0: red / crimson / brown / teal / coral
  - palette 1: near-black / burnt orange / dark red / sage / teal
  - palette 2: silver / sky blue / forest / charcoal / red / purple / magenta (7 colors)
  - palette 3: dark teal / cream / off-white / lavender / blush -- the one in the reference images
- last color in the shuffled palette becomes the background
- 2x supersampled offscreen buffer, downscaled to canvas
- grain pass: +/- 15 random noise per pixel channel
- spacebar regenerates with a new seed; window resize also regenerates

palette 3 is particularly suited to tufting -- its soft flat color blocking maps cleanly
to discrete yarn regions with minimal transition complexity.

## why the source is swappable

the engine reads the output image, not the source code. it does not know or care whether
the image came from ink.html, a different p5 sketch, a hand-drawn scan, or any other
tool that produces flat regions with hard borders.

this means:
- ink.html can be replaced or extended without touching the engine
- different generative styles (different region shapes, different border treatments,
  different palette logic) can be dropped in as long as the output contract holds
- future sources could have more palettes, different region counts, different distortion
  algorithms, animated outputs -- none of that changes the engine's job

## what the engine actually needs from any source

- a raster or vector image
- identifiable flat color regions (consistent fill, no gradient)
- clear hard border lines between regions (black or near-black, consistent width)
- a bounded palette (the engine needs to know which colors map to which yarn colors)

the engine's job for tufting:
1. ingest the source image
2. identify and isolate each color region
3. order regions for efficient arm traversal (minimize gun travel distance)
4. output traced region instructions: path coordinates, needle depth, yarn tension, loop height

## palette notes

since the source palette is fixed per run (not random per region), the engine can build
a static yarn color mapping once per palette and reuse it for every piece in that run.
this is what makes production runs tractable -- the arm does not have to re-calibrate
yarn color mid-piece.

palette 3 (dark teal / cream / off-white / lavender / blush) is the reference palette
from the images sent. the finished rug, the in-progress arm photo, and the flat source
image all use this palette. it is a good first calibration target for yarn color matching.
