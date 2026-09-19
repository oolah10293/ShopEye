# ShopEye

**A wearable AI vision experiment for sharing a physical workspace with an AI assistant.**

ShopEye asks a simple question:

> Can an AI that hears the conversation and sees selected moments from an ongoing job behave like someone who has been standing beside you watching the work?

The first version is deliberately **not smart glasses**. It uses one Android phone, its rear camera, and an earbud or headset. The phone is mounted so its camera sees roughly what the user sees while both hands remain free.

## The experience

Start one session, point the phone at the work area, put in an earbud, and work normally.

The assistant should:

- listen and respond through a continuous voice session;
- receive useful camera frames automatically rather than a 30 fps video stream;
- treat successive images as observations from one continuous job;
- remember recent facts such as meter readings, connector states, inspected parts, and where tools were last seen;
- stay quiet unless asked, something meaningfully changes, or the user appears about to make a mistake.

Natural questions should work:

- “Look at this.”
- “Was this plugged in before?”
- “What did that meter read earlier?”
- “Which one of these parts did I already check?”
- “Where did I leave that tool?”

## V0 hardware

- One Android phone
- Rear camera
- Phone microphone or connected headset microphone
- Earbud, headset, or phone speaker
- A simple mount that lets the rear camera see the workbench

## V0 interface

The app stays intentionally minimal:

- full-screen rear-camera preview;
- **START** and **STOP**;
- one very large **LOOK NOW** button;
- a small indicator that flashes whenever an image is actually transmitted.

There is no account-management screen, history browser, settings maze, object database, or polished UI in V0.

## Visual sampling

ShopEye does not send continuous video. The phone decides locally when a still image is worth transmitting.

A frame may be sent because of:

1. **LOOK NOW** — immediate fresh capture, bypassing automatic rate limits;
2. **visual speech cue** — a phrase such as “look at this” (added after basic voice and vision work);
3. **scene change** — a cheap comparison of low-resolution grayscale frames detects substantial change;
4. **movement then stillness** — phone sensors detect movement followed by roughly 0.5–1 second of relative stillness;
5. **heartbeat** — a slow image every 5–10 seconds prevents the visual context from becoming indefinitely stale.

Initial tuning targets:

- heartbeat: about 8 seconds;
- movement-settle delay: about 750 ms;
- minimum interval between automatic images: 1–2 seconds;
- LOOK NOW: always immediate.

These are starting points, not requirements. Real workbench use will decide the thresholds.

## Proposed V0 architecture

```text
CameraX preview + ImageAnalysis
              |
              v
          Frame gate  <---- LOOK NOW
         /     |           scene change
        /      |           motion + settle
       /       |           heartbeat
      v        v       v
       Selected JPEG frames
              |
              v
OpenAI Realtime session over WebRTC
       |                  |
       v                  v
 continuous audio     image observations
```

The Android client should obtain a short-lived Realtime client secret from a tiny token endpoint. A normal API key must not be embedded in the APK.

The token endpoint is authentication plumbing only. It does not proxy the session, store images, or become a larger cloud system.

## Smallest useful build

Development remains runnable at every checkpoint:

1. Rear CameraX preview with START, STOP, and LOOK NOW. LOOK NOW captures a known-good JPEG and flashes the indicator locally.
2. Hands-free Realtime voice conversation over WebRTC.
3. LOOK NOW inserts the current image into the same ongoing Realtime conversation.
4. Heartbeat and simple scene-change sampling.
5. Motion-and-settle sampling using phone sensors.
6. Speech-triggered capture for obvious visual phrases.
7. Explicit working memory only if tests prove the Realtime conversation alone loses important state.

See [docs/V0_PLAN.md](docs/V0_PLAN.md) for the implementation plan.

## Success test

After 20–30 minutes of ordinary work, ShopEye should feel more like a person who has been watching the job than an assistant receiving unrelated photographs.

It succeeds if it:

- retains useful short-term state;
- notices meaningful changes;
- answers questions about earlier observations;
- requires very little manual camera interaction;
- occasionally prevents a plausible mistake without becoming an irritating narrator.

See [docs/TEST_PLAN.md](docs/TEST_PLAN.md) for concrete bench tests.

## Scope boundary

V0 exists only to test whether shared, short-term visual continuity is genuinely useful.

Until that is proven, this project will not design:

- smart-glasses hardware;
- permanent visual memory;
- cloud storage or multi-user infrastructure;
- object recognition databases;
- video streaming to the model;
- polished product UI;
- background/screen-off Android operation;
- elaborate computer vision.

If the experiment works, the phone camera can later be replaced by glasses without changing the basic pattern: camera and microphone observations enter one continuous assistant session.

## Status

**Concept documented; implementation not started.**

The next task is Checkpoint 1: create the smallest Android Studio project that opens the rear camera and produces a fresh JPEG when LOOK NOW is pressed.
