# resin plotter -- table mount print on demand (commercial branch)

this branch covers the small, tabletop, pen-plotter-form-factor hardware that dispenses uv resin instead of ink, for on demand manufacturing of small items like pins and keychains.

this is a completely separate hardware category from the wall arm branch, not a variant of it. it should live as its own orphan branch in the repo, not nested under or derived from the wall arm work.

---

## positioning

this branch is the commercial, vc shaped side of the project. it solves a real, painful problem for independent artists and creators: right now, getting enamel pins or similar small physical goods made requires either a kickstarter to raise funds against speculative demand, or committing to a bulk minimum order, commonly around 20 units, before a single item has actually sold.

both paths mean manufacturing inventory before demand is confirmed. if it does not sell through, that is stale inventory sitting unsold, which eventually becomes a clearance sale, which is waste.

this branch removes the speculative front end entirely. nothing is manufactured until a real order and real payment exist.

---

## why table mount, not vertical

track a and the wall arm branch are vertical, articulated arms that move up, down, and sideways across a large wall mounted canvas. this branch is the opposite shape: a small, table mounted, regular pen-plotter style machine, because the actual products are tiny. pins and keychains need a canvas the size of a coaster, not a wall.

regular pen plotters already exist at this scale. what makes this branch new is not the form factor, it is what comes out of the nozzle.

---

## why resin

uv resin does not dry on its own. it stays liquid and fully workable until deliberately cured with uv light. that is a genuinely interesting material to build around for a few reasons:

- no racing against paint load decay or dry time mid print
- no re-dip timing pressure, since resin does not thicken or skin over on its own
- curing is a discrete, controllable step, not something gradual and unpredictable
- this sidesteps almost all of the wet medium physics problem that the wall arm branch has to deal with, since that entire problem set assumes evaporative drying

uv resin is also easier to work with than regular two part epoxy resin specifically. two part resin starts curing the moment it is mixed, which means it thickens over time and can string or trail when dispensed, leaving thin sticky threads behind the nozzle as it moves. uv resin does not kick off a curing reaction until it is hit with uv light, so it stays at a consistent, predictable viscosity for as long as needed and does not string during dispensing. that makes it a meaningfully cleaner material for a plotter style nozzle to work with, since dispensing behavior stays consistent from the first unit of the day to the last.

resin is mechanically and procedurally simpler to work with than paint, even though the end product looks and feels handmade.

---

## the business model

this is a backend manufacturing layer that plugs into a storefront a creator already has: shopify, squarespace, or an instagram shop. it is not a new marketplace, it slots into what artists already use.

flow:

1. artist connects their existing storefront to the inkblotter backend
2. artist uploads a design once
3. a customer places a real order and pays through something like stripe connect
4. only then does the resin plotter manufacture that one physical item, on demand
5. item ships

no upfront capital from the artist. no bulk minimum. no kickstarter. no stale inventory. no clearance sales. this is also a more sustainable model from an overconsumption standpoint, since nothing gets made that was not already sold.

---

## form factor

- small, desktop sized, not wall mounted
- closer to a standard pen plotter footprint than the wall arm's gantry or articulated arm
- likely a flat bed or small gantry, since resin dispensing needs a stable, level surface
- built in or attachable uv curing light for the final step

---

## substrate / products (early scope)

- enamel style pins
- keychains
- possibly coasters or small phone accessories later

these are all small enough that a tabletop plotter with a resin nozzle can realistically cover the full print area.

---

## layered curing, like working in layers in procreate

uv resin's cure on demand behavior means a design does not have to go down in one shot. it can be built up in layers, similar to working with layers in an illustration app like procreate:

1. dispense the first layer, cure it with uv light
2. dispense the second layer on top, cure it
3. repeat for however many layers the design needs

this is meaningfully different from wet painting, where a wall arm branch stroke either has to be planned to avoid wet on wet contact entirely, or has to account for real bleeding and blending if it does touch a wet layer. resin layering has none of that ambiguity, since each layer is fully locked in place by the uv cure before the next one goes down. no bleed, no blend, no timing pressure.

## relationship to the engine branch

enamel style designs are traditionally flat: solid color regions separated by clean edges, no gradients, no blending, closer to a static vector illustration than a painting. this actually makes the resin plotter's ingestion problem simpler than the wall arm branch's, since the engine does not need to reconstruct brushstroke direction, pressure, or painterly texture here, just color regions and their boundaries, plus which layer each region belongs to for curing order.

the resin plotter most likely needs its own planning output vocabulary, closer to fill regions and color pours than brush strokes, since resin behaves nothing like a loaded brush. it may share underlying image analysis work with the engine branch (palette extraction, region segmentation) but should not assume it can directly consume the same stroke plan format built for the wall arm branch. the engine's ingestion step for this branch is closer to flattening a design into ordered, curable color layers than to studying painterly technique.

---

## open questions

- what is the resin equivalent of a stroke plan: fill regions, outline plus fill, layered color pours?
- how many resin colors can be loaded simultaneously, and how are they switched or mixed?
- how is uv curing timed relative to multi color designs, cure after each color or only at the end?
- what does the storefront integration actually look like technically: a shopify app, a squarespace extension, an api a developer wires up manually?
- what does the stripe connect flow look like end to end, from customer checkout to manufacturing trigger?
- what is the realistic per unit cost and turnaround time compared to bulk enamel pin manufacturing?
- does this need its own separate repo entirely, given how little it shares with the wall arm branch?

---

## status

concept stage. no hardware or software decisions made yet. exists as its own named branch specifically so it does not get folded into or confused with the wall arm branch's wet medium physics work, which does not apply here.
