# sim implementation notes

## core architecture

the sim receives a stroke plan from the engine and replays it.
every stroke is drawn in the order the engine decided, at the speed the engine specified,
one mark at a time on an html canvas.

this is deterministic replay, not generation. given the same stroke plan input,
the sim always produces the same visual output in the same order.

## medium progression

### version 1: pens
- hard-edged marks
- consistent line width per pen type
- no pressure variation, no load decay
- easiest to render, closest to what the crosshatch-beta already outputs

### version 2: pencils and markers
- pencils: slightly textured marks, lighter pressure = thinner/lighter line
- markers: flat color, possible bleed at edge when overlapping wet passes
- both still relatively simple vs wet paint

### version 3: real material rendering (deferred)
- embroidery thread: real texture, sheen, strand-level color variation
- tufting yarn: pile texture, depth, real photographed fiber appearance
- reference: colorwork.studio technique (photograph real yarn under controlled lighting,
  render swatches from photographs using traditional computer graphics, webgl2 in browser)
- clean-room caution: colorwork.studio source is not public -- reference technique only,
  never implementation

## what the sim is not

- not a diffusion model renderer
- not image generation
- not ai art
- not sampling from a distribution
- not reimagining the input design

it draws what the engine said to draw, in the order the engine said to draw it.
the input has to already be in the right style. the sim does not transform it.
for crosshatch-beta v1: input must already be a cross-hatched image.
the sim traces it. it does not create the hatching.
