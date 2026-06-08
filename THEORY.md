# fleet-midi-scale: Theory

_Musical theory and ternary logic behind the scale agent._

---

## Scale Detection Algorithm

This agent detects scales from note input using interval pattern matching:

1. Extract unique pitch classes from input notes
2. Build interval vector (ordered semitone distances from root)
3. Match against known scale templates (major, natural minor, harmonic minor, etc.)
4. Assign ternary based on mode brightness

### Mode Brightness (Church Modes, brightest to darkest)

Lydian (+1) → Ionian (+1) → Mixolydian (0) → Dorian (0) → Aeolian (-1) → Phrygian (-1) → Locrian (-1)

The word "mode" comes from Latin *modus* — "measure, manner, tune."
Each mode has a characteristic note (the *mediant*) that defines its quality.

---

_This document is part of the educational supplement for [fleet-midi-scale](https://github.com/SuperInstance/fleet-midi-scale)._
