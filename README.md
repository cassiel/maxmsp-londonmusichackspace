-*- word-wrap: t; -*-

# maxmsp-londonmusichackspace

Tutorial material for London Music Hackspace MaxMSP workshop.

## BASICS

Use of Max 6 Projects. Implicit dependencies.

## 1-VIDEOTRACKING

Warming up with some video input and processing.

### video-101

Simple video capture (via Andrew Benson).

- `qmetro` to generate frames (possibly dropping)

- we unpack the incoming frames into red, green and blue components

- `jit.slide` adds optional time-blurring for areas getting brighter and/or darker

### thresholds

Movement detection in different areas of the image.

- frame-by-frame difference information, on frames which have been converted to monochrome

- `jit.slide` for temporal smoothing

- second black/white thresholding to flatten the greyscale introduced by the `jit.slide`

- `jit.3m` for simple averaging into a multislider trace

- motion-intersection: create one (or more) hot-spots by down-sampling with a low-resolution matrix and masking

- sound-generation (make-sound): simple gating, with a line generator to take off any sharp edges (needs a volume control - but Max 6 has one built-in)

## 2-NEWTON (via Rob Ramirez)

Simple physics modelling - with a bit of sound.

### newton

- using Jitter and OpenGL for display. A `jit.gl.render` renderer to a named context, a single `jit.gl.gridshape` sphere, and a `jit.gl.handle` for manipulating the scene

- use of `attrui` to configure attributes (possibly persisted by the objects themselves)

- adding physics: `jit.phys.body` (attached to gridshape) in a `jit.phys.world` (attached to render): the body will fall under gravity); the world can be "reset"

- add a `jit.gl.physdraw` for visual "debugging" and a `jit.phys.picker` for picking things up

- the worldbox constrains movement to a fixed space

- constraints - add a `jit.phys.hinge` with `position1` and `position2` to position the hinge and the object it's attached to

- duplicate the body and hinge (with a new body name) - place the hinges side by side

- scaling issues: turn up gravity

- behaviour: add some restitution, experiment with damping

- turn on collision detection - and unpack dictionaries to gather `impulse` messages

- simple FM synth (good for metallic bell sounds): takes a MIDI pitch (transformed into frequency) and an amplitude (transformed into attenuation in dB)

- two `cycle~` objects (carrier, modulator); two `line~` objects as envelope generators

- magic modulator factor of 3.5

- synthesiser is velocity sensitive, for both volume (carrier) and "brightness" (modulator)

### newton-modular

- code clean-up: an abstraction for the shape, body and hinge (`one-body`)

- world and context are now both named (`WORLD`, `CTX`) so that the abstractions are seen by the main patcher

- `one-body` arguments: body name, hinge position, note played on collision

- each `one-body` has its own synthesiser

- (some) use of `pattrstorage` - to see restitution values (a better use would be, say, hinge positions and lengths - possible exercise)

- external animation: extra inlet to `one-body` to allow impulses - attach to video burglar alarm
