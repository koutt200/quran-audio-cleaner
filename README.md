# تنقية التلاوة | Quran Audio Cleaner

A responsive, Arabic RTL audio-cleaning website. Adapted from a visual prototype into a working browser application.

## What it does

- Processes audio entirely on the visitor's device. No recording upload, accounts, analytics or third-party runtime dependencies.
- Reduces steady background noise with overlapping FFT windows and a conservative estimate from quiet sampled frames.
- Offers optional 50/60 Hz hum filters, a configurable high-pass filter and linked-channel peak normalization toward -1 dBFS, capped at +20 dB gain.
- Plays original and processed audio, draws actual waveforms, supports cancellation, and exports PCM 16-bit WAV.
- Saves up to five recordings in the current browser only when the visitor explicitly chooses Save. Important recordings should also be downloaded.
- Includes a synthetic test-tone demo, not a Quran recording.

## Files and local use

`index.html` contains the readable interface, CSS, DSP and application source. Its background worker is created from the embedded DSP using a local Blob URL. This self-contained deployment avoids missing runtime assets and needs no build step.

Serve the repository locally:

```sh
python3 -m http.server 8080
```

Open the local address printed by the server in a modern browser. GitHub Pages serves the same file directly from the main branch/root.

## Regression tests

With Node.js installed, from the repository directory:

```sh
node dsp.test.mjs
```

The test extracts the actual DSP source embedded in index.html, then checks FFT round-trip, bypass, hum rejection, synthetic noise reduction, test-tone preservation, silence, short inputs, linked stereo normalization and valid WAV headers/length. It needs no npm packages.

Chromium checks were also run against the deployed HTTPS website at desktop and mobile sizes: demo decoding, background worker processing, five-second WAV export with expected size, saved-archive persistence after reload, invalid-file errors and recovery, guide navigation and horizontal overflow. No browser JavaScript errors were observed.

These are functional and synthetic-signal checks, not a perceptual quality benchmark for speech or recitation.

## Limits

- Conventional digital signal processing, not AI speech separation. It does not reliably remove room echo, music or other speakers. Strong settings can affect speech or introduce artifacts. Keep the original and compare before saving.
- Inputs are limited to 80 MiB, eight minutes and one or two channels, plus a 128 MiB decoded-audio guard. Lower-powered devices may need much shorter clips. Decoding depends on browser codec support and device memory.
- The decoder requests 32 kHz. Output uses the actual decoded sample rate and preserves duration and channel count. WAV may be larger than MP3 input.
- There is no LUFS normalization, verse recognition, content editing or automatic silence removal.
- Browser storage can be cleared or evicted. Saved recordings are not a cloud backup.
- Loading the website contacts GitHub Pages for static content. The app makes no audio-upload requests.

No credentials or private audio recordings are included.
