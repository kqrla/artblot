# glossary -- terms used across inkblotter

---

## project terms

**inkblotter**
the project. a physical computing robot that paints images with real brushes and real paint.

**the wall arm branch**
the hardware and physical calibration side of the project. teaches the robot arm to physically paint. runs independently of the engine branch.

**the engine branch**
the software side of the project. takes an image and figures out how to decompose it into a stroke plan. runs independently of the wall arm branch.

**convergence**
the point at which the wall arm branch and the engine branch connect. the engine branch outputs a stroke plan, the wall arm branch executes it. this is the milestone both tracks are building toward.

**stroke plan**
the shared data format between the engine branch and the wall arm branch. a json file describing an ordered sequence of physical marks: position, brush, color, pressure, speed, re-dip instructions. see stroke-plan-format.md.

**calibration library**
the database the wall arm branch builds during calibration. maps combinations of brush, paint, and surface to expected physical outcomes (stroke width, paint load decay rate, etc.).

**ingestion / study phase**
the part of the engine branch where the software analyzes a reference painting before planning strokes. figures out palette, regions, stroke directions, value structure, layer order.

**illustrace**
a step-sister project (by the same author) that researches how to measure and transfer visual style. approximately 5% overlap with inkblotter. inkblotter might someday receive an image from illustrace to paint.

---

## physical terms

**wet medium physics**
the set of phenomena that make painting with real paint fundamentally different from plotting with a pen. paint load decay, bristle deformation, wet-on-wet bleeding, re-dip variance, etc. the core unsolved problem inkblotter is addressing.

**paint load**
how much paint is currently on a brush. starts high after a dip, decreases as the brush strokes across the surface.

**paint load decay**
the process of a brush running out of paint as it strokes. the rate depends on paint type, brush type, stroke speed, and pressure.

**re-dip**
the action of dipping the brush back into a paint well to reload paint. must happen at the right moment -- too early wastes paint, too late produces dry strokes.

**bristle spring-back**
when pressure on a brush is released, the bristles return to their resting shape. this affects stroke width and taper at the start and end of strokes.

**wet-on-wet**
painting over a stroke that has not dried yet. causes color mixing, bleeding, and blurring. the most complex wet medium phenomenon. avoided in v1.

**dry-on-dry**
painting over a stroke that has fully dried. clean edges, no bleeding. the safe approach for v1.

**taper**
the natural narrowing of a stroke at its start or end, caused by the brush lifting off the surface. in human painting, a natural and expressive mark-making quality.

**ferrule**
the metal band on a paintbrush that connects the handle to the bristles. should not be crushed by a brush holder clamp.

---

## software and algorithm terms

**crosshatch / hatching**
a drawing technique using parallel lines to represent value (lightness/darkness). dark areas have dense lines, light areas have sparse lines. the first stroke language inkblotter's the engine branch implements.

**stroke language**
the vocabulary of marks a system knows how to make. crosshatch uses only straight lines. a full painterly stroke language would include curved strokes, tapered strokes, washes, dry-brush, etc.

**value**
in visual art: how light or dark something is, independent of color. in crosshatching, line density encodes value.

**npr (non-photorealistic rendering)**
the field of computer graphics focused on making images look like they were made by human hands: painterly, sketchy, illustrative. inkblotter's the engine branch draws from npr research.

**vpype**
an open-source python-based pen plotter path processing pipeline. a likely starting point for the crosshatch beta.

**svg path**
a vector format describing drawing paths as sequences of move/line/curve commands. what most pen plotter software outputs. the crosshatch beta will work with or output svg paths.

**gcode**
the low-level machine control language used by cnc machines and 3d printers. describes arm movements in terms of motor coordinates. the wall arm branch will eventually translate stroke plans into gcode (or equivalent) for the specific hardware chosen.
