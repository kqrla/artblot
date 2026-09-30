# design -- resin plotter

## input format

enamel-style designs: flat color regions with hard outlines, no gradients, no wet blending.
simpler engine problem than brushstroke painting -- closer to tufting's flat region tracing.

## engine job for resin

- ingest enamel-style design (vector or rasterized flat regions)
- determine layer order: which regions must cure before adjacent ones are deposited
- output layer-by-layer dispense paths with resin volume and cure timing per layer

the layer ordering problem is the interesting part -- resin regions that touch cannot
both be wet simultaneously or they bleed. the engine sequences so each region cures
before its neighbors are deposited.

## commercial design source

- a creator's existing artwork (plugs into their storefront)
- generative source (same p5 pipeline at smaller scale)
- custom submitted designs per order

nothing gets produced speculatively. one order in, one piece manufactured.
