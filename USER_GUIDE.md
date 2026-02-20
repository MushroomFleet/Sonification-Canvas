# Sonification-WebUI — User Guide

Sonification-WebUI is a browser-based spectral synthesiser that turns images into audio. You paint directly onto a frequency-vs-time canvas and hear the result in real time. What you see is literally what you hear.

Every sound can be represented as a spectrogram — a picture where time runs left to right, frequency runs bottom to top, and brightness encodes loudness. Sonification-WebUI inverts this: instead of analysing sound into a picture, it synthesises audio from the picture you draw.

---

## Getting Started

```bash
npm install
npm run dev
```

Open **http://localhost:5173** in your browser.

---

## Interface Overview

The interface is divided into five zones:

```
 ┌──────────────────────────────────────────────────────┐
 │                     TOOLBAR                          │
 ├──────────┬───────────────────────────┬───────────────┤
 │          │                           │               │
 │   LEFT   │     SPECTROGRAM CANVAS    │    RIGHT      │
 │  SIDEBAR │                           │   SIDEBAR     │
 │          │                           │               │
 ├──────────┴───────────────────────────┴───────────────┤
 │               TRANSPORT BAR                          │
 ├──────────────────────────────────────────────────────┤
 │              OVERLAY CONTROLS                        │
 └──────────────────────────────────────────────────────┘
```

| Zone | Purpose |
|---|---|
| **Toolbar** (top) | Drawing tools, brush options, Colour Mode toggle |
| **Left Sidebar** | Synthesis engine, frequency scale, window function, Colour Picker |
| **Canvas** (centre) | The spectrogram drawing area — click and drag to paint sound |
| **Right Sidebar** | ADSR envelope editor, Export button |
| **Transport Bar** | Play / Stop, Loop toggle, master Volume |
| **Overlay Controls** | Piano Roll, Beat Grid, Snap-to-Note, BPM, Duration |

---

## Drawing Tools

Six tools are available in the Toolbar. Click a tool button or press its keyboard shortcut to activate it.

### Brush (B)

The default drawing tool. Click and drag on the canvas to paint amplitude (brightness). Brighter pixels produce louder sound at that frequency.

- **Size** slider (1 - 200 px): Controls the brush diameter.
- **Opacity** slider (0 - 100%): Controls how much amplitude is added per stroke.

**Tutorial — Draw a bass note:**
1. Press `B` to select the Brush.
2. Set Size to ~40 and Opacity to 100%.
3. Click near the **bottom** of the canvas and drag horizontally to the right.
4. Press `Space` to play. You should hear a low bass tone.

### Eraser (E)

Removes amplitude from the canvas. Same Size and Opacity controls as the Brush, but subtracts instead of adding.

**Tutorial — Erase part of a stroke:**
1. Draw a horizontal line with the Brush.
2. Press `E` to switch to the Eraser.
3. Click on the middle of the line to erase a section.
4. Play to hear the gap in the sound.

### Harmonic Brush (H)

Paints a fundamental frequency plus its upper harmonics. When you click at a position, the tool stamps a circle at the fundamental frequency row and automatically adds harmonics (2f, 3f, 4f... up to 16f or 20 kHz, whichever is lower).

- **Size** slider (1 - 200 px): Diameter of each harmonic dot.
- **Opacity** slider (0 - 100%): Base amplitude.
- **Harmonics** slider (1 - 16): How many harmonics to add.
- **Rolloff** slider (-18 dB to 0 dB): How quickly upper harmonics fade. -6 dB/octave is a natural rolloff. 0 dB means all harmonics are equally loud.

**Tutorial — Draw a rich musical tone:**
1. Press `H` to select the Harmonic Brush.
2. Set Harmonics to 8, Rolloff to -6 dB.
3. Enable **Snap** in the Overlay Controls (bottom bar) and toggle **Piano Roll** on.
4. Click on a piano roll line near the middle of the canvas.
5. Play — you'll hear a pitched tone with overtones, not just a sine wave.

### Line Tool (L)

Draws a straight line between two points. Click to set the start point, drag to aim, and release to stamp the line.

- Uses the current Brush Size and Opacity settings.
- Great for precise frequency sweeps (glissandi) and straight sustained notes.

**Tutorial — Draw a siren sweep:**
1. Press `L` to select the Line Tool.
2. Click at the bottom-left of the canvas.
3. Drag to the top-right and release.
4. Play to hear a rising frequency sweep.

### Fill / Gradient Tool (F)

Two modes controlled by a single tool:

**Flood Fill** (default): Click on a region to fill all connected pixels of similar amplitude with the current opacity value. Works like a paint-bucket — fills outward until it hits edges or areas with significantly different amplitude.

**Tutorial — Fill a region:**
1. Use the Brush to draw a closed shape (a circle or rectangle outline).
2. Press `F` to select the Fill Tool.
3. Click inside the shape to fill it.
4. Play to hear a band of frequency content.

### Select Tool (S)

Draw a rectangular selection by clicking and dragging. A marching-ants border appears around the selection. A floating toolbar appears below the canvas with these operations:

| Button | Action |
|---|---|
| **Copy** | Copies the selected region to the clipboard |
| **Paste** | Pastes the clipboard contents at the selection origin |
| **Delete** | Clears all amplitude within the selection |
| **Gain slider + Apply** | Scales the amplitude of the selection (0% = silence, 200% = double) |

**Tutorial — Duplicate a sound:**
1. Draw something with the Brush.
2. Press `S`, then drag a rectangle around your drawing.
3. Click **Copy** in the floating toolbar.
4. Click elsewhere on the canvas, then press `Ctrl+V` to paste.
5. Play to hear the original and the copy at different times.

**Keyboard shortcuts for selections:**
- `Ctrl+C` — Copy selection
- `Ctrl+V` — Paste at selection position
- `Delete` or `Backspace` — Delete selection contents
- `Escape` — Deselect (cancel selection)

---

## Keyboard Shortcuts Reference

| Key | Action |
|---|---|
| `Space` | Play / Stop |
| `B` | Brush tool |
| `E` | Eraser tool |
| `H` | Harmonic Brush tool |
| `L` | Line tool |
| `F` | Fill tool |
| `S` | Select tool |
| `Ctrl+Z` | Undo |
| `Ctrl+Shift+Z` | Redo |
| `Ctrl+C` | Copy selection |
| `Ctrl+V` | Paste selection |
| `Delete` / `Backspace` | Delete selection |
| `Escape` | Cancel current stroke or deselect |

---

## Colour Mode

The **Colour** toggle in the Toolbar enables Colour Mode. When active, brush strokes paint colour onto the canvas instead of greyscale, and that colour controls the *timbre* (tone quality) of the sound.

### How Colour Maps to Sound

| Colour Channel | Timbre |
|---|---|
| **Red** | Sawtooth wave — bright and buzzy, all harmonics |
| **Green** | Square wave — hollow and reedy, odd harmonics only |
| **Blue** | Flute / sine — pure and clean, fundamental only |

**Hue** blends between these three archetypes smoothly around the colour wheel.

**Saturation** controls harmonic intensity:
- 0% saturation (grey) = pure sine wave regardless of hue
- 100% saturation = full archetype character

### Colour Picker (Left Sidebar)

When Colour Mode is on, the **Colour Picker** appears at the top of the Left Sidebar with three sliders:

- **Hue** (0° - 360°): Rotates around the colour wheel to select timbre blend.
- **Saturation** (0% - 100%): How much the hue affects the timbre.
- **Lightness** (0% - 100%): Brightness of the painted colour.

A colour swatch previews your current brush colour above the sliders.

**Tutorial — Paint different timbres:**
1. Toggle **Colour** on in the Toolbar.
2. In the Left Sidebar, set Hue to 0° (red), Saturation to 100%.
3. Draw a horizontal line near the middle of the canvas — this paints a bright red line.
4. Change Hue to 120° (green) and draw another line slightly higher.
5. Change Hue to 240° (blue) and draw a third line higher still.
6. Play — the red line sounds buzzy (sawtooth), the green line sounds hollow (square), and the blue line sounds pure (sine).

---

## Synthesis Engines (Left Sidebar)

The **Engine** dropdown in the Left Sidebar selects which algorithm converts your canvas into audio.

### ISTFT (Inverse Short-Time Fourier Transform)

The default engine. Converts each column of the canvas into a frequency-domain frame and runs an inverse FFT to produce a time-domain audio signal. Uses random phases for real-time preview and Griffin-Lim iterative phase reconstruction for export.

- **Griffin-Lim Iters** slider (1 - 64): Higher = better phase coherence during export (smoother sound, less metallic). Has no effect on preview playback.

Best for: general-purpose spectrogram sonification, imported images.

### Additive

Generates audio by summing individual sine-wave oscillators for each active frequency bin. Each row of the canvas becomes a separate oscillator with continuous phase. Caps at 512 simultaneous oscillators (loudest bins prioritised).

Best for: clean tonal sounds, precise pitch control, musical compositions.

### Noise Band

Filters white noise through IIR bandpass filters centred on each active frequency bin. Produces textured, noisy tones.

- **Q Factor** slider: Controls filter sharpness. Higher Q = narrower bands (more tonal), lower Q = wider bands (more noise-like).

Best for: wind, ocean, percussion textures, atmospheric pads.

### Blend

Crossfades between Additive and Noise Band engines.

- **Blend Ratio** slider (0 - 100%): 0% = pure Additive, 100% = pure Noise Band.
- **Q Factor** slider: Same as Noise Band.

Best for: mixing tonal and textural qualities in a single sound.

---

## Frequency Scale (Left Sidebar)

The **Scale** dropdown changes how canvas rows map to frequencies.

| Scale | Description |
|---|---|
| **Logarithmic** | Musical intervals are evenly spaced. An octave always takes the same vertical space. Best for music. |
| **Mel** | Perceptually uniform — mimics how humans hear pitch. Common in speech analysis. |
| **Linear** | Equal Hz per pixel. Low frequencies are compressed, high frequencies are spread out. Useful for visualising harmonics. |
| **ERB** | Equivalent Rectangular Bandwidth. Models the ear's critical bands. Used in psychoacoustics research. |

The canvas always covers **20 Hz** (bottom) to **20,000 Hz** (top).

---

## Window Function (Left Sidebar)

The **Window** dropdown selects the analysis/synthesis window used by the ISTFT engine.

| Window | Character |
|---|---|
| **Hann** | Good all-rounder. Smooth frequency resolution. Default. |
| **Blackman-Harris** | Excellent sidelobe suppression. Good for isolating individual tones. |
| **Hamming** | Similar to Hann but with a slightly higher sidelobe floor. |
| **Rectangular** | No windowing. Maximum frequency resolution but highest sidelobe leakage. |

---

## ADSR Envelope (Right Sidebar)

The **Envelope** section shapes how the overall volume changes over the duration of playback. An SVG visualiser shows the envelope shape in real time as you adjust the sliders.

| Parameter | Range | Description |
|---|---|---|
| **Attack** | 0 - 10,000 ms | Time for volume to ramp from silence to full |
| **Decay** | 0 - 10,000 ms | Time to fall from peak to sustain level |
| **Sustain** | 0 - 100% | Volume level held during the main body |
| **Release** | 0 - 10,000 ms | Time to fade from sustain to silence at the end |

**Tutorial — Create a pad with slow attack:**
1. Set Attack to 2000 ms, Decay to 500 ms, Sustain to 70%, Release to 3000 ms.
2. Draw a broad horizontal band across the entire canvas.
3. Set Duration to 8 seconds in the Overlay Controls.
4. Play — the sound fades in gradually, holds, then fades out.

---

## Transport Bar

Located below the canvas.

| Control | Description |
|---|---|
| **Play / Stop** button (or `Space`) | Synthesises the current canvas and plays it through your speakers. Press again to stop. |
| **Loop** toggle | When enabled, playback restarts automatically when it reaches the end. |
| **Volume** slider (0 - 100%) | Master output volume. |

During playback, a vertical purple playhead line sweeps across the canvas from left to right showing the current playback position.

---

## Overlay Controls

Located at the bottom of the interface.

| Control | Description |
|---|---|
| **Piano Roll** toggle | Shows horizontal guide lines at musical note positions (C1, C2, C3...) on the canvas. Helps you draw at exact pitches. |
| **Beat Grid** toggle | Shows vertical guide lines at beat intervals on the canvas. Helps you align sounds to a tempo. |
| **Snap** toggle | When enabled, brush strokes snap to the nearest musical note (semitone). Ensures drawn frequencies are musically in tune. |
| **BPM** number input (1 - 999) | Sets the tempo for the Beat Grid. |
| **Duration** slider (0.5 - 30 seconds) | Sets the total duration of the canvas in seconds. |

**Tutorial — Draw a melody on the grid:**
1. Toggle **Piano Roll**, **Beat Grid**, and **Snap** all on.
2. Set BPM to 120, Duration to 4 seconds.
3. Select the Brush, set Size to ~15.
4. Draw short horizontal strokes at different note heights, aligned to the beat grid lines.
5. Play to hear a simple melody.

---

## Exporting Audio

Click the **Export** button in the Right Sidebar to open the Export dialog.

| Setting | Options |
|---|---|
| **Format** | WAV, AIFF, FLAC, MP3 |
| **Bit Depth** | 16-bit, 24-bit, 32-bit float |
| **Sample Rate** | 44.1 kHz, 48 kHz |
| **Peak Normalise** toggle | Normalises the output to -0.3 dBFS to prevent clipping |
| **Filename** | Editable text field (default: "sonification-export") |

Click **Render & Download** to synthesise and download. A progress bar shows the export status. You can cancel at any time.

**Notes:**
- MP3 format ignores the bit depth setting (encoded at 320 kbps).
- The ISTFT engine uses Griffin-Lim phase reconstruction during export for higher quality than the real-time preview.
- FLAC export currently produces a WAV file (FLAC encoding is a planned future feature).

**Tutorial — Export your first sound:**
1. Draw something on the canvas.
2. Click **Export** in the Right Sidebar.
3. Choose WAV format, 24-bit, 44.1 kHz.
4. Leave Peak Normalise on.
5. Click **Render & Download**.
6. The file downloads to your browser's default download location.

---

## Undo / Redo

- **Ctrl+Z** — Undo the last drawing operation (up to 50 steps, 200 MB cap).
- **Ctrl+Shift+Z** — Redo.

Every brush stroke, line, fill, paste, delete, or gain adjustment is a separate undo step.

---

## Tips and Techniques

### Creating Different Sound Types

| Sound | How to Draw It |
|---|---|
| **Sustained bass note** | Horizontal line near the bottom of the canvas |
| **High whistle** | Horizontal line near the top |
| **Siren / sweep** | Diagonal line (use the Line tool) |
| **Drum hit / click** | Vertical smear covering many frequencies in a narrow time column |
| **Chord** | Multiple parallel horizontal lines at harmonic intervals (use Harmonic Brush with Snap on) |
| **Noise burst** | Large rectangular fill covering a broad frequency range |
| **Vibrato** | Wavy horizontal line drawn freehand with the Brush |
| **Fade in** | Triangular shape that widens from left to right |

### Using Colour Mode for Rich Timbres

1. Enable Colour Mode.
2. Paint the same note in red (sawtooth) for the attack portion, then continue in blue (sine) for the sustain — this creates a bright attack that mellows out.
3. Use green (square) for hollow, organ-like sustained tones.
4. Mix colours: yellow (red + green) blends sawtooth and square characteristics.

### Working with the Frequency Scale

- Use **Logarithmic** for musical work — octaves are evenly spaced.
- Switch to **Linear** to see the harmonic series as evenly spaced lines.
- Use **Mel** for speech-like sounds.

---

## Technical Reference

| Property | Value |
|---|---|
| Canvas resolution | 2048 x 1024 pixels |
| Frequency range | 20 Hz (bottom) to 20,000 Hz (top) |
| FFT size | 4096 |
| Hop size | 1024 |
| Max undo steps | 50 (200 MB cap) |
| Max additive oscillators | 512 per time column |
| Harmonic brush max | 16 harmonics |
| Preview sample rate | 44,100 Hz |
