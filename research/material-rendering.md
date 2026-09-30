# material rendering research

research into how other tools handle real material texture rendering in the browser.
all flagged with clean-room caution where source is not public.

## colorwork.studio

knit colorwork design tool. previews designs in real yarn colors before knitting.

technique (observed, not from source):
1. photograph each yarn line under consistent, controlled studio lighting
2. render actual knit swatches from those photographs using traditional computer graphics
3. runs on webgl2 in browser, live rendering

their source is not public. clean-room approach: reference the observable technique only,
never copy implementation.

relevance: the "photograph real material, render from photographs" approach is the right
model for this sim's eventual embroidery thread and tufting yarn previews.
flat color blocks look fake next to real material texture.

## tufting sim comparables

### tuftnano (tuftnano.com)
closest direct comparable for the tufting branch.
uses segment anything model to isolate color regions from an image,
then depth anything v2 to generate a grayscale depth map controlling pile height
(white = ~40mm pile, black = flatweave ~10mm). likely closed source, clean-room caution.

### nedgraphics tuft premium
industrial cad, 3d viewer, u3m file format, realistic yarn parameter library.

### tuftlab.app / thetuftingcalculator.com
free browser tools. image-to-yarn-color and yardage breakdown per color region.

### pointcarre.com
industrial tuft cad. compatible with commercial tufting machines (cmc, tuftco, vandewiele, modra).

## knit/stitch sim comparables (github)

### yunniko/crochet-simulator
live 3d loop-through-loop stitch tubes. real topology, not texture approximation.

### hannah144/crochet-simulator
basic stitch and gauge framework. explicitly relies on human visual comparison against
real gauge squares for accuracy validation.

### tylervick/graphghan issue 51
detailed discussion of why knit and crochet need genuinely different yarn geometry.
cites academic "stitch meshes" research.

## academic references

- kuiwuchn.github.io/rtstitch -- real-time fiber-level yarn rendering
- kuiwuchn.github.io/stitchmodeling -- stitch geometry modeling
- cornell "fitting procedural yarn models for realistic cloth rendering" (kavita bala et al)
  captures real photographed thread, fits procedural yarn model. closest academic match
  to colorwork.studio's likely approach.
- "rapid simulation of yarn-level realism in fuzzy yarn knitted fabrics"
  fibers and polymers journal. uses consumer-grade imaging.
- dragonbook/awesome-cloth -- curated ongoing cloth research list
