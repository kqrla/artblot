# generative source contract

the engine does not depend on any specific generative source.
it reads output images. it does not care how they were made.

## the contract

any generative source feeding the engine must produce:
- flat color regions with hard borders
- no gradients, no anti-aliased blends between regions  
- black or near-black border lines separating regions
- a fixed, bounded palette per run

if a source image fulfills this contract, the engine can process it.
the source is fully swappable.

## current reference implementation

kqrla/assets/ink.html -- a p5.js sketch producing perlin-distorted voronoi compositions.
documented in detail in the rug-tufting branch under design/design.md.
this is one example of a compliant source, not the required one.

## why this matters

keeping the source swappable means:
- the engine is not coupled to any specific aesthetic
- different branches can use different sources (embroidery might use simpler geometric
  inputs; tufting uses the voronoi sketch; resin plotter might use vector enamel designs)
- the generative side can evolve without touching engine code
- future sources can be added without engine changes as long as the output contract holds
