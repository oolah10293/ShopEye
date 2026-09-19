# Minimum Android V0 Plan

## Ground rules

- Kotlin Android app.
- One Activity.
- CameraX for rear preview and image capture/analysis.
- OpenAI Realtime over WebRTC.
- No database, navigation framework, background service, or permanent history.
- Keep the project runnable after every checkpoint.

## Checkpoint 1 — Camera

Deliver:

- full-screen rear-camera preview;
- camera permission handling;
- START, STOP, and LOOK NOW controls;
- LOOK NOW produces a fresh JPEG;
- local indicator flashes when the JPEG is produced.

Acceptance check:

- repeat captures are current, sharp enough to read a workbench meter, correctly rotated, and do not freeze the preview.

No network or AI code belongs in this checkpoint.

## Checkpoint 2 — Voice

Deliver:

- START requests a short-lived credential from the token endpoint;
- Android establishes one Realtime WebRTC session;
- microphone audio reaches the assistant;
- assistant audio returns through the selected phone/headset route;
- STOP cleanly closes the session.

Acceptance check:

- hold a natural hands-free conversation for at least ten minutes without touching the screen.

## Checkpoint 3 — Manual vision

Deliver:

- LOOK NOW captures a fresh JPEG;
- the image is inserted into the active Realtime conversation;
- the indicator flashes only after transmission succeeds;
- the assistant's instructions identify all images as observations from one continuous physical workspace.

Acceptance check:

- point at an object, say “look at this,” press LOOK NOW, and receive a relevant answer without starting a new conversation.

This is the first meaningful proof of the concept.

## Checkpoint 4 — Automatic visual sampling

Deliver:

- about 8-second heartbeat;
- low-resolution grayscale scene comparison;
- shared 1–2 second automatic-send cooldown;
- simple logging of trigger reason and send time.

Acceptance check:

- normal work produces a sensible stream of observations without obvious frame flooding;
- an unchanged view still refreshes periodically;
- meaningful scene changes are normally observed within a few seconds.

## Checkpoint 5 — Motion and settle

Deliver:

- movement detection from accelerometer/gyro;
- a capture after movement ends and the phone remains relatively still for about 750 ms;
- integration with the same frame gate and cooldown.

Acceptance check:

- turning the mounted phone toward another object normally causes a useful post-movement image, not a blurred in-motion frame.

## Checkpoint 6 — Visual speech cues

Deliver:

- use available Realtime transcription/events to identify a small explicit phrase list;
- phrases such as “look,” “look at this,” “what is this,” and “read this” request a fresh frame;
- debounce repeated transcript fragments.

Acceptance check:

- common explicit visual requests trigger promptly without needing LOOK NOW.

Do not attempt a general local semantic classifier in V0.

## Checkpoint 7 — Evidence-based memory

Run the bench tests before implementing this checkpoint.

Only if failures show the need, add a compact session-state representation for facts the assistant should retain. Design it around observed failure cases, not imagined future requirements.

## After V0 proves useful

Possible later work, deliberately outside this plan:

- foreground service for screen-off/background operation;
- better adaptive sampling;
- cost and bandwidth controls;
- optional session summaries;
- glasses as the camera source;
- durable memory with explicit privacy controls.
