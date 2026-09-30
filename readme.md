# embroidery

this branch covers the desktop embroidery machine -- the first commercial front in the build order.

smallest hardware footprint of any branch. desktop, pen-plotter scale. no wet medium physics.
a needle pokes thread through dissolvable stabilizer backing. once done, the backing dissolves
and the embroidered piece comes out. cheapest branch to build and cheapest per unit to run.

## structure

- `docs/` -- concept, commercial case, stitch types, disambiguation
- `hardware/` -- machine design, needle mechanism, dissolvable stabilizer
- `design/` -- simple design inputs, stitch path planning, engine job
- `milestones/` -- build checkpoints end to end

embroidery comes first across all commercial fronts. it proves the pipeline end to end
at the lowest possible cost before anything harder. see `roadmap.md` on main.
