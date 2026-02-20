# Sonification-Canvas

A browser-based spectral synthesiser that turns drawings into audio. Paint directly onto a frequency-vs-time spectrogram canvas and hear the result in real time — what you see is literally what you hear.

**[Try it live](https://scuffedepoch.com/sonification-canvas/)** | **[Full User Guide](USER_GUIDE.md)**

---

## What Is This?

Every sound can be represented as a spectrogram — a picture where time runs left to right, frequency runs bottom to top, and brightness encodes loudness. A bass note is a horizontal line near the bottom. A siren is a diagonal streak. A snare is a vertical burst.

Sonification-Canvas inverts this relationship: instead of analysing sound into a picture, it **synthesises audio from a picture you draw**.

Draw a horizontal line at the bottom of the canvas — you hear bass. Draw one at the top — you hear a whistle. Draw a diagonal — you hear a sweep. Fill a rectangle — you hear a noise band. It's that direct.

---

## Features

### Drawing Tools
- **Brush** — Freehand amplitude painting with adjustable size and opacity
- **Eraser** — Subtract amplitude to carve shapes and create rhythmic gaps
- **Harmonic Brush** — Paint a fundamental frequency plus up to 16 overtones automatically
- **Line Tool** — Precise straight lines for sweeps, glissandi, and sustained notes
- **Fill / Gradient** — Flood fill enclosed regions or draw smooth amplitude gradients
- **Select Tool** — Rectangular selection with copy, paste, delete, and gain scaling

### Synthesis Engines
- **ISTFT** — Inverse Short-Time Fourier Transform with Griffin-Lim phase reconstruction for export
- **Additive** — Up to 512 phase-coherent sine oscillators per time column
- **Noise Band** — IIR bandpass-filtered white noise for textured, atmospheric sounds
- **Blend** — Crossfade between Additive and Noise Band for hybrid timbres

### Colour Mode
Toggle Colour Mode to paint timbre directly onto the canvas using colour:
- **Red** = Sawtooth wave (bright, buzzy)
- **Green** = Square wave (hollow, reedy)
- **Blue** = Sine / flute (pure, clean)
- Saturation controls harmonic intensity; hue blends smoothly between archetypes

### Audio & Export
- Real-time preview playback with ADSR envelope shaping
- Export to **WAV**, **AIFF**, **MP3** (320 kbps), or **FLAC**
- Configurable bit depth (16 / 24 / 32-bit float) and sample rate (44.1 / 48 kHz)
- Peak normalisation to -0.3 dBFS
- Non-blocking export via Web Worker

### Musical Aids
- **Piano Roll** overlay with labelled note lines
- **Beat Grid** overlay synced to configurable BPM
- **Snap-to-Note** for pitch-perfect drawing
- **4 frequency scales**: Logarithmic, Mel, Linear, ERB

---

## Live Demo

The app is deployed and ready to use at:

### [https://scuffedepoch.com/sonification-canvas/](https://scuffedepoch.com/sonification-canvas/)

The live site includes a **built-in interactive tutorial** — click the **?** button in the top-right corner of the toolbar to access step-by-step guides, feature documentation, sound recipes, and a full keyboard shortcut reference, all without leaving the app.

---

## Quick Start

### Use Online
Visit **[scuffedepoch.com/sonification-canvas](https://scuffedepoch.com/sonification-canvas/)** — no install needed.

### Run Locally
```bash
git clone https://github.com/MushroomFleet/Sonification-Canvas.git
cd Sonification-Canvas
npm install
npm run dev
```
Open **http://localhost:5173** in your browser.

### Build for Production
```bash
npm run build
```
The `dist/` folder contains a fully static site ready for any web host.

---

## Documentation

- **[USER_GUIDE.md](USER_GUIDE.md)** — Comprehensive reference covering every tool, engine, control, and keyboard shortcut with step-by-step tutorials
- **Built-in Tutorial** — Click the **?** button in the app toolbar for an in-app interactive guide with the same content, navigable by section

---

## Keyboard Shortcuts

| Key | Action |
|---|---|
| `Space` | Play / Stop |
| `B` `E` `H` `L` `F` `S` | Brush, Eraser, Harmonic, Line, Fill, Select |
| `Ctrl+Z` | Undo |
| `Ctrl+Shift+Z` | Redo |
| `Ctrl+C` / `Ctrl+V` | Copy / Paste selection |
| `Delete` | Delete selection |
| `Escape` | Cancel stroke or deselect |

---

## Tech Stack

| Layer | Technology |
|---|---|
| UI Framework | React 19 + TypeScript |
| Build | Vite 7 |
| State | Zustand 5 |
| Styling | Tailwind CSS v4 |
| DSP | Web Audio API + fft.js |
| Encoding | Custom WAV/AIFF encoders, lamejs (MP3) |
| Export | Web Worker (non-blocking) |

---

## License

MIT

---

## Citation

### Academic Citation

If you use this codebase in your research or project, please cite:

```bibtex
@software{sonification_canvas,
  title = {Sonification-Canvas: Browser-Based Spectral Synthesis Workstation},
  author = {Drift Johnson},
  year = {2025},
  url = {https://github.com/MushroomFleet/Sonification-Canvas},
  version = {1.0.0}
}
```

### Donate:

[![Ko-Fi](https://cdn.ko-fi.com/cdn/kofi3.png?v=3)](https://ko-fi.com/driftjohnson)
