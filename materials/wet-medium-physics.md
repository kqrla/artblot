# wet medium physics -- the core unsolved problem

this is what makes inkblotter different from every pen plotter that came before it. nobody has cracked this for a painting robot yet.

---

## what wet medium physics means

a pen deposits ink uniformly and predictably. a paintbrush does not.

the mark a brush makes depends on:

- how much paint is loaded on the bristles right now
- how long ago it was dipped
- how fast the arm is moving
- how much pressure is applied
- the angle of the brush to the surface
- the brush type and condition (new bristles vs worn)
- the paint viscosity (which changes with temperature and air exposure)
- the surface absorbency
- whether there is still wet paint on the surface from a previous stroke

all of these interact. there is no simple formula.

---

## key phenomena to model or compensate for

### paint load decay
a brush loaded with paint runs dry as it strokes. the mark starts opaque and full, then fades as paint is depleted. the rate depends on:
- paint viscosity (thin paint depletes faster)
- stroke speed (faster = less paint per mm)
- pressure (more pressure = more paint released per mm)
- bristle type (synthetic holds less than natural hair)

what we need: a model or lookup table of paint remaining vs stroke distance for each brush and paint combination.

### bristle spring-back
when you press a brush into a surface, the bristles splay out. when you lift, they spring back. this means:
- stroke width does not equal brush size, it depends on pressure
- the beginning and end of a stroke look different from the middle
- a taper effect happens naturally when you slow down or lift off

what we need: pressure-to-width mapping per brush type.

### wet-on-wet bleeding
if you paint over a stroke that has not dried, the colors merge, blur, and bleed into each other. this is the most complex phenomenon. it depends on:
- how wet the first layer is (time elapsed)
- paint viscosity of both layers
- how much pressure the second stroke applies
- surface absorbency

for v1: avoid wet-on-wet entirely. let each stroke dry before painting over it, or plan strokes to never overlap in the same session.

### re-dip behavior
every re-dip loads a slightly different amount of paint depending on:
- dip depth
- how long the brush stays in the well
- paint viscosity at that moment (it thickens as it sits)
- wipe-off behavior (how hard you wipe on the well edge)

what we need: a consistent mechanical dip routine that minimizes variance.

### brush wear and deformation
bristles bend, splay, and lose their shape over time. a brand new round brush comes to a perfect point. after 20 strokes it may be fanned out.

for v1: use cheap synthetic brushes and replace them frequently. do not try to model wear.

---

## research questions (open)

1. can we measure paint load on the brush in real time? (weight sensor, optical, conductance?)
2. can we detect a dry stroke happening in real time? (camera watching the mark being made?)
3. is there a simple heuristic (stroke distance threshold) that predicts re-dip need well enough for v1?
4. how much does acrylic viscosity change over a 2-hour painting session as it sits in open wells?
5. can wet-on-wet bleeding be turned into a feature rather than a bug for certain stroke types?

---

## wet medium physics roadmap

| phase | what we tackle |
|---|---|
| v1 | ignore wet-on-wet, use fixed re-dip intervals, acrylic only |
| v2 | calibration curves for paint load decay per brush and paint combo |
| v3 | pressure-to-width mapping, taper control |
| v4 | wet-on-wet detection and avoidance (timing and layer planning) |
| v5 | wet-on-wet as intentional tool (blending, soft edges) |

---

## reference painting notes

the reference painting uploaded at project start (gouache landscape, pink sky, navy mountains, green hills) shows:

- hard edges between color regions, suggests opaque paint applied dry-on-dry
- flat color regions, minimal visible brushstroke texture, suggests loaded brush and slow deliberate strokes
- limited palette, probably 6-8 colors, good for color well setup
- layered sky gradient, warm pink sky likely painted first, then cooler tones laid over when dry

this painting style is friendly for inkblotter v1. it does not require wet-on-wet blending or fine detail, just clean region fills with flat opaque paint.
