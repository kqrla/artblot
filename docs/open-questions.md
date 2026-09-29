# open questions -- things not decided yet

this file tracks the major open decisions in inkblotter. update it as decisions get made.

---

## hardware (the wall arm branch)

### arm type
- status: undecided
- options: gantry/cnc, vertical wall plotter, 6-dof robot arm, custom rig
- decision criteria: build complexity, stroke expressiveness, brush angle control, cost
- leaning toward: gantry or vertical plotter for v1 (proven, simpler), arm for v2+

### brush switching mechanism
- status: undecided
- options: magnetic quick-release, collet chuck, fixed slots in a carousel, manual swap
- decision criteria: reliability, speed, how many brushes needed simultaneously
- leaning toward: fixed slot carousel for v1 (simplest)

### paint well design
- status: undecided
- options: open wells, sealed wells with wicks, custom dispensers
- decision criteria: evaporation rate of acrylic, ease of cleaning, number of colors
- leaning toward: small open wells with lids between sessions

### rinse station
- status: undecided
- options: static water cup the arm dips into, flowing water rinse, multiple water stages
- decision criteria: how much cross-contamination is acceptable, time per rinse
- leaning toward: two-stage water cup (dirty then clean)

### paint load sensing
- status: research needed
- options: time/distance heuristic, weight sensor on brush mount, camera watching mark quality, brush conductance
- leaning toward: distance heuristic for v1, sensor later

---

## software (the engine branch)

### stroke planning algorithm
- status: starting with crosshatch (simple), expanding later
- open: what comes after crosshatch? contour lines? region fill? full non-photorealistic rendering?

### ingestion and study phase
- status: not started, research-heavy
- open: how long should ingestion take? what does it output? how do we validate that the study was correct?

### layer ordering
- status: not started
- open: how does the engine branch decide what gets painted first? background before foreground? light before dark?

### color mixing
- status: not in scope yet
- open: does inkblotter mix paint on a palette (hard), swap to a pre-mixed well (simpler), or just use a fixed palette (v1)?

---

## convergence

### stroke plan format version
- status: v0 draft exists (see stroke-plan-format.md)
- open: what fields does the wall arm branch actually need? will not know until the wall arm branch is further along.

### when do the two tracks actually merge?
- status: open
- criteria: the engine branch can output a stroke plan from a real image; the wall arm branch can execute a stroke plan with predictable results for at least one brush and paint combo

---

## relationship to illustrace

- status: settled -- 5% overlap, step-sister project
- inkblotter might receive an image from illustrace to paint someday
- inkblotter does not use illustrace's architecture, parameter registry, or transfer operators
- no further integration planned unless decided otherwise

---

## decision log

| date | decision | rationale |
|---|---|---|
| project start | two-the wall arm branchrchitecture, no shared context | avoids premature coupling while both tracks are figuring out fundamentals |
| project start | crosshatch as the engine branch beta milestone | simplest possible stroke language, validates full pipeline cheaply |
| project start | browser sim before hardware | lets the engine branch be tested visually before the wall arm branch exists |
| project start | acrylic as v1 paint | most predictable wet medium, fast drying, cheap to test |
