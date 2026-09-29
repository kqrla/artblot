# stroke plan format -- the shared contract

the stroke plan is the data format that connects the engine branch (software) to the wall arm branch (hardware). the engine branch outputs it, the wall arm branch executes it. this is the convergence point of the whole project.

---

## purpose

the stroke plan answers: what exactly should the arm do, in what order, with what tools?

it is not a rendered image. it is not gcode (though it can be translated to gcode). it is inkblotter's own intermediate representation, designed to carry enough information for physical execution including paint-specific details that gcode does not have.

---

## format (json)

```json
{
  "version": "0.1",
  "canvas": {
    "width_mm": 210,
    "height_mm": 297,
    "surface": "illustration_board"
  },
  "palette": [
    { "id": "c0", "hex": "#f2c4a0", "name": "warm peach", "paint_type": "acrylic" },
    { "id": "c1", "hex": "#3b5e8c", "name": "navy blue", "paint_type": "acrylic" }
  ],
  "brushes": [
    { "id": "b0", "type": "flat", "size": 6, "description": "flat wash brush 6mm" },
    { "id": "b1", "type": "round", "size": 2, "description": "round detail brush 2mm" }
  ],
  "strokes": [
    {
      "id": 0,
      "brush": "b0",
      "color": "c1",
      "start": { "x": 45.2, "y": 120.8 },
      "end": { "x": 98.4, "y": 122.1 },
      "pressure_curve": [0.8, 0.9, 0.85, 0.7],
      "speed_mm_s": 30,
      "angle_deg": 12,
      "redip_before": true,
      "rinse_before": false,
      "layer": 0
    }
  ]
}
```

---

## field reference

### canvas
| field | type | description |
|---|---|---|
| width_mm / height_mm | float | physical canvas dimensions |
| surface | string | surface type (affects paint behavior) |

### palette entry
| field | type | description |
|---|---|---|
| id | string | referenced by strokes |
| hex | string | target color |
| paint_type | string | acrylic / gouache / watercolor / oil |

### brush entry
| field | type | description |
|---|---|---|
| id | string | referenced by strokes |
| type | string | flat / round / fan / filbert / etc |
| size | int | brush size in mm |

### stroke entry
| field | type | description |
|---|---|---|
| id | int | stroke index in order |
| brush | string | brush id to use |
| color | string | palette color id |
| start / end | {x, y} | stroke endpoints in mm |
| pressure_curve | float[] | normalized pressure 0-1 sampled across stroke length |
| speed_mm_s | float | arm travel speed |
| angle_deg | float | brush tilt angle (0 = perpendicular to surface) |
| redip_before | bool | dip in paint before this stroke |
| rinse_before | bool | rinse brush before this stroke |
| layer | int | layer order (0 = base, higher = on top) |

---

## version history

### v0 (crosshatch beta)
much simpler, just line segments for the browser sim:
```json
{
  "strokes": [
    { "id": 0, "x1": 10, "y1": 20, "x2": 50, "y2": 20, "weight": 1.0 }
  ]
}
```

### v0.1 (current target)
full format above, designed for physical execution with one brush and one paint type.

### future
- bezier curves for non-straight strokes
- multi-touch strokes (side of brush)
- wash / glaze strokes (very diluted paint, large area coverage)
- dry-brush strokes (minimal paint load, scratchy texture)

---

## translation to hardware

the stroke plan is not gcode. the wall arm branch has a translation layer that converts stroke plan entries into motor commands, accounting for:

- the specific arm/gantry coordinate system
- calibration offsets (where paint wells are, where brush rack is)
- physical limits (max arm speed, acceleration curves)
- real-time adjustments (if paint load sensor says dry, insert a redip)
