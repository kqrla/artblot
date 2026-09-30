# hardware -- rug tufting

## frame setup

backing cloth gets stretched and stapled onto a wooden frame. the frame stands upright.
the arm works against this vertical surface, same orientation as the wet paint setup.
once the piece is fully tufted the frame is dismantled and the rug comes out.

## projection mapping

a projector maps the engine's region layout directly onto the cloth surface.
this is projection mapping, not just displaying an image -- the engine output gets
transformed to account for the frame's physical position, tilt, and the cloth surface geometry,
so the design lands correctly and the arm's traced paths match what is projected.

calibration steps (to be fully scoped):
- [ ] frame position and geometry measured and registered
- [ ] projector position calibrated against frame surface
- [ ] engine output warped to match physical surface
- [ ] arm path coordinates derived from projection mapping output

## arm and tufting gun interface

the same articulated arm holds the tufting gun end effector instead of a brush.
key parameters:
- needle depth -- how far the needle punches through the backing
- loop height -- how tall the resulting pile is
- yarn tension -- how tightly the yarn feeds through the gun
- travel speed -- how fast the arm traces each region pass

none of these involve wet medium physics. all are mechanically repeatable.
