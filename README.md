# Second Voice — Vocal Harmony Trainer

A browser-based practice tool for singers learning to sing harmony. Sing into your microphone and hear a real-time pitch-shifted "second voice" at whatever interval you choose (a third, a fifth, an octave, etc.), so you can train your ear against a live harmony line instead of a static backing track.

Runs entirely client-side — no audio is ever uploaded or sent to a server.

## Features

- **Real-time pitch shifting** using a phase-vocoder algorithm, shifting your voice by any interval from a minor 3rd to an octave, up or down
- **Interval picker** covering the common harmony intervals (3rds, 4ths, 5ths, 6ths, octaves) in both directions
- **Mix controls** to balance your lead voice against the generated harmony, including a "harmony only" mode for testing yourself
- **Microphone selector**, since browsers/OSes don't always default to the mic you actually want (especially relevant if you use a Bluetooth headset or hearing aid, which the OS may otherwise select automatically)
- **Live input level meter** to confirm the selected mic is actually picking up your voice
- **Recording**, capturing your lead voice and the harmony together, with playback and download (dry vocal is time-aligned to the harmony's processing delay so the recorded take doesn't sound like an echo)
- **Waveform scope** for a simple visual of the live signal

## How it works

Pitch shifting is done with a phase vocoder: incoming audio is analyzed in overlapping frames (STFT), each frame's per-bin phase is tracked and adjusted to the target pitch ratio, and the result is resynthesized and resampled back to the original tempo. This avoids the "chipmunk" pitch/formant coupling of naive resampling and avoids the phase-cancellation artifacts of naive time-domain granular pitch shifters. The trade-off is a small amount of inherent processing latency (roughly 50–100ms), which is normal for any real-time pitch shifter, hardware or software.

All DSP runs inside an `AudioWorkletProcessor`, so it processes audio on a dedicated real-time thread rather than the main UI thread.

## Requirements

- A modern browser with `AudioWorklet` support (Chrome, Edge, or Firefox recommended; Safari's support is less consistent)
- **A secure context.** Microphone access requires the page be served over `https://` or `http://localhost` — opening the HTML file directly (`file://`) will *not* work; this is a browser security restriction, not a bug in the app
- Headphones are strongly recommended — without them, the mic can pick up the harmony coming out of your speakers and create feedback

## Running it

Pick whichever fits how you want to use it:

**Locally, with VS Code**
Install the [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) extension, right-click the HTML file, and choose "Open with Live Server." This serves it over `http://127.0.0.1`, which satisfies the secure-context requirement.

**Locally, with Python**
```
python -m http.server 8000
```
then open `http://localhost:8000/index.html`. (If this fails with a socket/permission error, it's usually antivirus or firewall software blocking Python from opening a listening port — Live Server above is a good fallback.)

**Hosted, with GitHub Pages**
1. Push this repo to GitHub (public repo, for free Pages hosting)
2. In the repo, rename the HTML file to `index.html` if it isn't already
3. Go to Settings → Pages, set Source to "Deploy from a branch," branch `main`, folder `/ (root)`
4. Your app will be live at `https://<your-username>.github.io/<repo-name>/`

**Quick sharing / no install**
Paste the HTML/CSS/JS into an online editor like [jsbin.com](https://jsbin.com) — just make sure you open the *standalone output URL* (not the embedded editor preview pane), since embedded iframes are commonly blocked from requesting microphone access.

## Usage

1. Open the app and select your microphone from the dropdown if the default isn't correct
2. Tap the power button and allow microphone access
3. Pick a harmony interval (major 3rd up is the default — the most common harmony line)
4. Adjust the Your Voice / Harmony mix sliders to taste
5. Sing — the harmony voice follows your pitch and rhythm in real time
6. Optionally hit Record to capture a take, then play it back or download it

## Limitations

- This is a **fixed-interval** shifter — it transposes your voice by a constant number of semitones, it does not do intelligent scale/key-aware harmonization
- Designed and tested for monophonic vocal input; it isn't intended for polyphonic material (full band mixes, chords, etc.)
- Small processing latency is inherent to real-time pitch shifting and can't be fully eliminated

## License

Add a `LICENSE` file to this repo to specify how others may use this code. [MIT](https://choosealicense.com/licenses/mit/) is a common, permissive default for small projects like this — GitHub can generate one for you when creating the repo, or via "Add file → Create new file" named `LICENSE`.

## Made with Claude


---
## Creative idea by: 
*Roadlesstrodden*
