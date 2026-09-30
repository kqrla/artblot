# resin-plotter

this branch covers everything specific to the tabletop UV resin plotter front.
completely different machine from the wall arm -- small, desktop footprint, pen-plotter scale.
most directly commercial branch: plugs into a creator's existing storefront as a
backend fulfillment layer, manufactures only when a real paid order comes in.

## structure

- `docs/` -- concept and full spec
- `hardware/` -- machine design, dispenser head, UV curing, layer registration
- `design/` -- enamel-style design input, engine layer ordering, region fills
- `milestones/` -- build checkpoints end to end
- `commercial/` -- storefront integration model, unit economics, no-inventory rationale

see also: `roadmap.md` on main for how this fits into the full build order.
