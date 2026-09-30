# hardware reference images

## reference-arm-in-progress.jpg

a robotic tufting arm mid-execution on a vertical frame. prior art hardware, not inkblotter's
own build -- included as a reference for the frame and carriage architecture.

what it shows:

**frame:**
aluminum extrusion rails forming a rectangular frame, mounted on wooden easel legs.
the backing cloth is stretched across the frame face, held taut. the whole assembly
stands vertical, which is the same orientation inkblotter's wall arm operates in.

**carriage and gun mount:**
the tufting gun is clamped to a carriage that rides the horizontal rail.
a red clamp and green bracket assembly holds the gun perpendicular to the backing cloth.
the carriage moves left/right along the horizontal extrusion.

**y-axis tensioning:**
cables or strings run from the carriage up to the top corners of the frame,
suggesting a cable-driven or tensioned y-axis system for vertical movement.
this is one approach to moving the gun up and down the frame without a rigid lead screw.

**partially complete rug:**
the lower portion of the frame shows completed tufting -- flat color regions built up
in yarn, matching the design projected or traced onto the backing. the upper portion
shows the backing cloth still bare, with the design visible through it.
the color palette matches palette 3 from ink.html (dark teal, cream, blush, lavender).

**background:**
a second frame visible on the left with what appears to be a traced design on backing
cloth, pre-tufting. suggests the workflow: trace/project first, then tuft.

**relevance to inkblotter:**
- confirms the vertical frame + carriage approach is practical at this scale
- the cable-driven y-axis is worth investigating as an alternative to lead screw
- the gun-to-carriage clamp mechanism is a starting point for the end effector design
- the partially complete rug confirms flat region sequencing is how tufting actually builds

## reference-finished-rug.jpg

the same design fully tufted and laid in a living space.
shows the finished material quality: consistent pile height, clean region borders,
the concentric ring regions from the russian doll effect rendered in yarn.
palette 3 in physical yarn form -- confirms the color translation from flat digital
source to physical yarn is legible and commercially presentable.

## reference-source-design.jpg

the flat digital source image. palette 3 composition: teal, cream, lavender, blush, black borders.
this is what the engine ingests -- flat color regions, hard black borders, fixed palette.
the concentric rings on two regions are visible here as flat alternating stripes,
which become physical pile rings in the finished rug.

pipeline illustrated across all three images:
flat generative source -> arm executing on frame -> finished rug in space.
