# V0 Architecture

## Purpose

V0 tests interaction quality, not product completeness. One Android phone supplies continuous audio and selected still images to one ongoing AI conversation.

## Components

### Android activity

A single screen owns the visible prototype:

- rear-camera preview;
- START and STOP controls;
- large LOOK NOW control;
- connection state;
- brief “frame sent” indicator.

Keeping the Activity visible and the screen on avoids foreground-service complexity during the first experiment.

### Camera controller

Use CameraX:

- `Preview` for the live view;
- `ImageAnalysis` for cheap local comparisons;
- `STRATEGY_KEEP_ONLY_LATEST` so analysis never builds a stale frame queue.

LOOK NOW must request a fresh, full-quality frame rather than blindly reuse an old analysis frame.

### Frame gate

The frame gate accepts capture requests from independent triggers:

| Trigger | Initial behavior |
|---|---|
| LOOK NOW | Send immediately; bypass automatic cooldown |
| Heartbeat | Send about every 8 seconds |
| Scene change | Send after a substantial low-resolution image difference |
| Motion + settle | Send after movement followed by about 750 ms of stillness |
| Visual speech cue | Add later for obvious phrases such as “look at this” |

Automatic triggers share a 1–2 second minimum interval. Exact values must be tuned from real sessions.

For scene comparison, periodically reduce the analysis image to approximately 64×36 grayscale and compute mean absolute pixel difference against the last accepted observation. V0 does not need object detection, optical flow, SLAM, embeddings, or ML Kit.

### Motion detector

Use the gyroscope and/or accelerometer only as a capture hint:

1. detect meaningful movement;
2. mark the view as changing;
3. wait until motion remains below a threshold for roughly 0.5–1 second;
4. ask the frame gate for a fresh observation.

### Realtime client

Use OpenAI Realtime over WebRTC for:

- microphone input;
- assistant audio output;
- the ongoing conversation;
- image inputs inserted into that same conversation.

Images are asynchronous observations within the voice session, not separate vision chats.

### Token endpoint

The Android app must not contain a normal OpenAI API key.

A minimal server endpoint:

1. authenticates with the normal server-side API key;
2. requests a short-lived Realtime client secret;
3. returns that secret to the phone.

It does not need a database, image storage, session history, or audio/image proxying.

## Working memory

The first experiment relies on the ongoing Realtime conversation for continuity. Do not build a separate memory subsystem yet.

If testing exposes repeatable failures, add a compact rolling state containing only useful observations such as:

- last meter readings and when they were seen;
- connector state changes;
- parts already inspected;
- last-seen tool locations;
- uncertainties and conflicting observations.

V0 requires no memory between sessions.

## Expected complications

- autofocus and lighting changes can create false scene-change triggers;
- hands moving through view may trigger unnecessary frames;
- image rate, latency, and cost require real-world tuning;
- audio routing varies across Android phones and Bluetooth devices;
- background camera and microphone operation requires foreground-service work that is intentionally postponed.

None of these prevents the visible, screen-on V0 experiment.
