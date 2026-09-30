# engine

orphan branch. the headless research core that feeds every other branch.

the engine takes a design input and produces machine instructions -- stroke plans for
the wall arm, traced regions for tufting, stitch paths for embroidery, layer sequences
for the resin plotter. it does not know or care which hardware is consuming its output.
it is pure software, pure research, and runs completely separately from all hardware branches.

this branch has no commercial pressure and no hardware dependency. it converges with
the hardware branches when it can produce output they can consume, and the hardware
is proven enough to execute it reliably. you are the merge point -- nothing converges
automatically.

## structure

- `docs/` -- what the engine is, what it is not, scope and framing
- `research/` -- crosshatch beta, image analysis approaches, comparable work
- `formats/` -- stroke plan format and other output specs shared with hardware branches

## key distinction

the engine is not the browser sim. the browser sim visualizes what the engine produces.
they are separate orphan branches with no shared history.

see also: `roadmap.md` on main for how the engine converges with hardware over time.
