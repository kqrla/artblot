# brush and paint types -- reference

a reference for the physical materials inkblotter will eventually work with. different combinations of brush, paint, and surface behave completely differently. this doc tracks what we know and what we have tested.

---

## brush types

### round brush
- tapers to a point
- versatile: can make thin detail lines or wider fills depending on pressure
- the most common brush type
- good for: outlines, detail, organic curves
- robot challenge: tip behavior changes completely with pressure, hard to control precisely

### flat brush
- square-edged, uniform width
- predictable stroke width (less pressure-sensitive than round)
- good for: flat fills, straight strokes, blocking in color regions
- robot challenge: low -- most predictable brush type, good for v1

### fan brush
- bristles spread in a fan shape
- used for blending, texture, foliage effects
- good for: grass texture, tree foliage, soft blends
- robot challenge: high -- results are inherently variable, hard to control

### filbert brush
- oval-shaped, between round and flat
- makes soft-edged strokes
- good for: blending, petals, organic shapes
- robot challenge: medium

### liner / rigger brush
- very long thin bristles, holds a lot of paint
- good for: long continuous thin lines, grass, hair
- robot challenge: medium -- requires steady slow movement

---

## paint types

### acrylic
- viscosity: medium-high (can be thinned with water)
- open time: 10-30 minutes depending on humidity
- drying time: fast, strokes dry within minutes
- re-wettable: no, once dry it is permanent
- bleed on wet: minimal
- robot friendliness: best for v1
- notes: thickens in open wells over a session, especially in dry climates. keep wells covered or use slow-dry medium.

### gouache
- viscosity: medium (creamier than acrylic)
- open time: similar to acrylic
- drying time: fast-medium
- re-wettable: yes, dried gouache can be reactivated with water
- bleed on wet: low-medium
- robot friendliness: good, but re-wettability means old strokes can be disturbed
- notes: the medium in the reference painting. very opaque, matte finish, flat color regions.

### watercolor
- viscosity: thin (mostly water)
- open time: long
- drying time: variable, depends heavily on water ratio and paper absorbency
- re-wettable: yes
- bleed on wet: high, bleeds and blooms on wet paper
- robot friendliness: hardest. water ratio is everything. small errors cascade.
- notes: not recommended for v1 or v2. save for after wet medium physics is understood.

### oil paint
- viscosity: thick (can be thinned with linseed oil or mineral spirits)
- open time: hours to days
- drying time: days to weeks
- re-wettable: during open time, yes. after curing, no.
- bleed on wet: high but slow and controllable
- robot friendliness: cleanup requires solvents, very slow feedback loop
- notes: not recommended until inkblotter is mature.

---

## surface types

| surface | tooth | absorbency | best for | notes |
|---|---|---|---|---|
| illustration board | low | low | acrylic, gouache | flat, stable, predictable -- recommended for v1 |
| watercolor paper 300gsm | medium | high | watercolor, gouache | will not buckle, but absorbency is variable |
| canvas board | medium | low-medium | acrylic, oil | standard painting surface |
| gesso-primed mdf | low | very low | acrylic | smooth, very stable, cheap |
| plain printer paper | very low | medium | testing only | will buckle with any moisture |

---

## calibration matrix (to be filled in during the wall arm branch)

as the wall arm branch runs calibration experiments, fill in this table:

| brush | paint | surface | re-dip interval (mm) | width at normal pressure (mm) | notes |
|---|---|---|---|---|---|
| flat 6mm | acrylic | illustration board | ? | ? | tbd |
| round 4mm | acrylic | illustration board | ? | ? | tbd |
| flat 6mm | gouache | illustration board | ? | ? | tbd |

this table becomes the calibration library that the wall arm branch uses to translate stroke plans into arm behavior.
