# 3 Osc Panner

A Pure Data patch with three sine oscillators. Each one has its own frequency, gain and stereo pan. There are also six presets and a global frequency multiplier.

## Requirements

- [Pure Data](https://puredata.info/) (vanilla). No externals are needed.

## Usage

1. Open `3_osc_panner.pd` in Pure Data.
2. Turn on DSP (**Media → DSP On**, or `Ctrl+/`).
3. Pick a preset or move the sliders.

## Controls

Each of the three oscillators has:

| Control     | Range       | Description                                  |
|-------------|-------------|----------------------------------------------|
| `Freq`      | 20–2000 Hz  | Oscillator frequency                         |
| `Gain`      | 0–1         | Oscillator amplitude                         |
| `Pan_(L/R)` | 0–1         | Stereo position (0 = left, 1 = right)        |

### Presets

The **Presets** radio (or the `0`–`5` message boxes) loads a set of values for all three oscillators (frequency, gain, pan):

| # | Osc 1 (L)     | Osc 2 (R)     | Osc 3 (center) |
|---|---------------|---------------|----------------|
| 0 | 576 Hz, 0.7   | 512 Hz, 0.7   | 216 Hz, 0.35   |
| 1 | 576 Hz, 0.7   | 432 Hz, 0.7   | 256 Hz, 0.35   |
| 2 | 152 Hz, 0.7   | 144 Hz, 0.7   | 72 Hz, 0.35    |
| 3 | 137 Hz, 0.7   | 128 Hz, 0.7   | 64 Hz, 0.35    |
| 4 | 136 Hz, 0.7   | 128 Hz, 0.7   | 64 Hz, 0.35    |
| 5 | 128 Hz, 0.7   | 128 Hz, 0.7   | 64 Hz, 0.35    |

In presets 2–4, the two oscillators panned hard left and right are 8–9 Hz apart. Played on headphones, this produces a binaural beat. Preset 5 sets them to the same frequency for comparison.

### Octaves

The **Octaves** radio multiplies the frequency of all three oscillators:

| Selection | 0    | 1   | 2 | 3 | 4 | 5 |
|-----------|------|-----|---|---|---|---|
| Multiplier| ×0.25| ×0.5| ×1| ×2| ×3| ×4|

A new setting takes effect right away. A `[t b f]` sends the multiplier to each oscillator's `[* 1]` and then re-outputs its current frequency.

## How it works

- Each voice is `[osc~]` → `[*~ gain]`, split to left and right.
- Panning uses an equal-power law: `pan × π/2` feeds `[cos]` for the left channel and `[sin]` for the right channel. This keeps the perceived loudness steady as a voice moves across the stereo field.
- The three voices are summed for each channel, scaled by `0.33` to prevent clipping, and sent to `[dac~]`.
