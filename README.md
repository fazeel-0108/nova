# Nova

A personal AI assistant that runs in the browser and installs like an app
(Android / iPhone / desktop). This repository contains the **static frontend
only** — it is published with GitHub Pages at
**https://fazeel-0108.github.io/nova/**.

## How it works

- `index.html`, `app.js`, `bgfx.js`, `styles.css`, `sw.js`, `manifest.webmanifest`, `icons/`
  are the whole app: chat, study tools, notes, voice (manual mic + hands-free
  wake word) and device commands. All state lives in the browser's local storage.
- The **server-side routes the app needs — the AI relay, cross-device sync and
  the passcode gate — are hosted separately** (`/api/chat`, `/api/sync`,
  `/api/gate`). `apiBase()` in `app.js` points them at that relay; the API keys
  live there as secrets, never in this repo and never in the browser.
- Open the app on your phone and use **Add to Home screen** / **Install app**.

## Tests

The voice state machine, microphone ownership and duplicate-event handling run
as a real suite against `app.js`, using deterministic fakes for speech
recognition, TTS and the microphone:

```bash
npm i --no-save playwright && npx playwright install chromium
node tests/run-voice-tests.mjs      # exits non-zero if any assertion fails
```

Or open `tests/voice-lifecycle.html` in a browser and read the summary on the
page. The same suite runs automatically in GitHub Actions on every push.

## Notes

- Voice needs a secure context (https or localhost) for the microphone.
- Hands-free listening uses the browser's speech recognition; a website cannot
  use a phone's low-power wake-word DSP, so it only listens while the page is open.
- Browser speech recognition availability varies by browser — Chrome and Edge
  support it; the Android app can also use the device's own recognizer.
