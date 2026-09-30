# browser-sim

orphan branch. the html canvas visualization layer for the engine's output.

the browser sim runs engine output stroke by stroke in the browser before any hardware
exists. it is deterministic -- given the same stroke plan, it always produces the same
visual output in the same order. it is not a generative model, not a diffusion renderer,
not sampling from anything. it is a replay of exactly what the engine decided.

this branch is separate from the engine branch. the engine decides. the browser sim shows.
they converge when the engine can produce output and the sim can replay it.

## structure

- `docs/` -- what the browser sim is, what it is not, scope and framing
- `sim/` -- html canvas implementation notes, replay architecture, live stroke rendering
- `research/` -- material rendering comparables, yarn/stitch sim research, tufting tools

## progression

version one: pens on html canvas, crosshatch lines rendered live stroke by stroke.
version two: pencils and markers.
version three (later): real material texture rendering for embroidery thread and tufting yarn.

the sim never generates. it always replays.
