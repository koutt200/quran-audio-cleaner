# تنقية التلاوة | Quran Audio Cleaner

A responsive Arabic, right-to-left audio-cleaning website. Adapted from the supplied visual prototype into a working, dependency-free browser app.

## Features

- Local file selection and drag-and-drop. Audio never uploads to an application server.
- Web Audio decoding, followed by processing in a module Web Worker to keep the interface responsive.
- Conservative spectral noise reduction estimated from the quietest sampled frames, with overlapping FFT windows.
- Optional 50/60 Hz hum notches and a configurable high-pass filter.
- Optional linked-channel peak normalization toward -1 dBFS, capped at +20 dB gain.
- Original/processed playback, real waveforms, cancellation and PCM 16-bit WAV download.
- Three presets and adjustable noise strength.
- Optional IndexedDB archive, limited to five saved recordings on the current browser. Nothing saves permanently until the user clicks Save.
- Built-in synthetic demo signal. It is not a Quran recording.
- No account, analytics, runtime libraries, remote fonts or paid API.

## Deployment layout

The root `index.html` is a self-contained production bundle of the same interface, DSP code and worker, so GitHub Pages needs no external runtime files. The source archive included in this repository contains the editable modules and regression tests described below.

## Run locally

Serve this folder over HTTP rather than opening index.html as a file:

```sh
python3 -m http.server 8080
```

Open the address printed by the server in a modern browser. GitHub Pages serves the same static files, with no build step.

## Deploy on GitHub Pages

Upload `index.html`, `style.css`, `app.js`, `dsp.js`, `worker.js` and `icon.svg` to the same directory on the repository's main branch. In Settings > Pages, select deployment from the main branch and the root folder. No API keys or server are needed.

## Tests

With Node.js installed:

```sh
npm test
```

The dependency-free DSP tests cover FFT round-trip accuracy, bypass, hum rejection, synthetic background reduction, preservation of a test tone, silence, very short inputs, linked stereo normalization and WAV headers/length. They do not establish perceptual quality on human speech or recitation.

UI checks were also run in Chromium at desktop and mobile sizes for decoding, worker processing, valid WAV export, archive save/reload, bad-file errors, recovery and horizontal overflow.

## Limits and honest expectations

- This is conventional digital signal processing, not an AI speech-separation model. It reduces steady background noise, low rumble and electrical hum. It does not remove room echo, music, other speakers or arbitrary sounds reliably.
- Strong settings may introduce artifacts or affect speech. Always compare with the original and keep an original copy.
- Input is limited to 80 MiB, eight minutes and one or two channels, with an additional 128 MiB decoded-audio guard. Lower-powered devices may need much shorter clips. Decoding itself depends on the browser and available device memory.
- Decoding requests 32 kHz. WAV output uses the actual decoded sample rate, with preserved channel count and duration. WAV can be larger than a compressed source.
- Codec support depends on the browser. WAV and MP3 are recommended fallbacks when another format fails.
- There is no loudness (LUFS) normalization, content recognition, verse editing or automatic deletion of silence.
- Saved recordings live only in this browser's storage, which can be cleared or evicted. Download important files.
- Opening the website contacts the website host to load static assets. The app itself makes no audio-upload requests.

## Files

- `index.html`: Arabic interface and guide
- `style.css`: responsive presentation
- `app.js`: file handling, UI, worker orchestration and device archive
- `dsp.js`: filters, spectral processing, normalization and WAV encoding
- `worker.js`: background processing
- `icon.svg`: local vector icon
- `dsp.test.js`: DSP regression checks

No private credentials or source audio are required or included.
