# hardware -- resin plotter

## machine overview

tabletop, pen-plotter footprint. small enough for a desk.
dispenses UV resin in controlled paths, cures each layer with UV light before the next.

## key properties

- dispenser head: deposits resin along a traced path without stringing
- UV curing: each deposited layer gets locked before the next is laid
- layer registration: each subsequent layer aligns precisely to the one below
- no wet medium physics: resin stays exactly where placed until the UV fires

## the UV resin advantage

UV resin's no-cure-until-lit property makes dispensing clean and layerable.
a layer can be built up and only committed when the UV fires.
this is what makes the layer ordering problem tractable vs wet paint.

## end product

small hard goods: enamel-style pins, keychains, small decorative objects.

## open questions

- [ ] dispenser nozzle geometry that minimizes stringing at path endpoints
- [ ] optimal travel speed vs resin viscosity for clean edge definition
- [ ] UV exposure time per layer depth
- [ ] layer-to-layer registration tolerance at pin scale
