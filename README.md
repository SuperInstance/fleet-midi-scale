# fleet-midi-scale

_Scale and mode detector — what key are we in?_

_One of 16 ternary MIDI agents in the [Live Paradigm Fleet](https://github.com/SuperInstance/sailor-workspace)._

---

## Philosophy — Why Ternary?

The Live Paradigm treats musical gestures as ternary operations. Where binary logic
gives yes/no, ternary gives **approve/reject/observe** — a richer cognitive substrate
that maps naturally to music theory, emotional tension, and conversational flow.

This agent implements **ternary decomposition for scale**.

## Architecture

Position in the fleet pipeline:

```
🎤 Voice → OpenSMILE (25 features) → Ghost Track (T-0..T-4 CR predictions)
  → tminus-dispatcher (cue scheduling) → Fleet Conductor (routing)
  → scale (port 2161)
```

## API Reference

| Method | Path | Description |
|--------|------|-------------|
| GET | /health | Health check + agent identity |
| POST | /agent with `{"type":"probe"}` | Liveness probe for fleet-conductor |
| POST | /agent | Process musical data, return ternary analysis |
| POST | / | Direct query with JSON body |

### Response Format

```json
{
  "status": "ok",
  "agent": "fleet-midi-scale",
  "port": 2161,
  "ternary_vector": [0, 0, 0],
  "ternary_invariant": 0,
  "closed_gesture": false
}
```

## Ternary Logic

| Position | +1 | 0 | -1 |
|----------|------|------|------|
| ternary[0] | major/ascending | chromatic/blues | minor/descending |

## Educational Supplement

A scale is a sequence of notes ordered by pitch. The pattern of intervals between
notes determines the scale's character — its emotional quality.

### Major Scale (ternary +1)
The pattern: W-W-H-W-W-W-H (whole, whole, half, whole, whole, whole, half)
C-D-E-F-G-A-B-C. Bright, stable, the "default" Western scale.

### Natural Minor Scale (ternary -1)
The pattern: W-H-W-W-H-W-W
C-D-Eb-F-G-Ab-Bb-C. Darker, more introspective.

### Chromatic (ternary 0)
All 12 semitones. No fixed pattern. Used for passing tones, tension.

## Fleet Integration

- **Port**: 2161
- **Roles**: note, velocity
- **Conductor ID**: `scale`
- **Protocol**: HTTP POST to `/2161/agent` with JSON body, 5s timeout
- **Conservation Law**: Σ(Δ_midi) = 4 × Σ(ternary) — closed gestures return to start

## Starting

Local development:

```bash
python3 engine.py --port 2161
```

Or via the fleet start script:

```bash
./scripts/start-fleet-agents.sh
```

## Credits

**Part of the Live Paradigm Fleet** — A ternary cognitive architecture for musical AI.
GitHub: github.com/SuperInstance
Fleet conductor: [sailor-workspace](https://github.com/SuperInstance/sailor-workspace)
