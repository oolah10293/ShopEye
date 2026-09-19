# Project Decisions

This log preserves the reasoning behind the initial scope.

## ShopEye is an interaction experiment

The immediate question is whether shared visual continuity is useful. V0 is not a smart-glasses build and does not need glasses research to begin.

## Use one Android phone first

A mounted phone already provides a camera, microphone, sensors, networking, display, and audio routing. It tests the experience before spending money or designing hardware.

## Send selected images, not video

Continuous 30 fps model video is unnecessary for the hypothesis. Sparse still observations reduce bandwidth and complexity while allowing the phone to decide when the view matters.

## Keep audio and images in one conversation

Voice and image observations must share one ongoing Realtime session so the assistant can interpret them as one timeline rather than disconnected requests.

## Begin with simple triggers

V0 uses LOOK NOW, heartbeat, low-resolution scene change, and movement followed by stillness. Sophisticated computer vision is postponed until actual failures justify it.

## LOOK NOW captures fresh

Manual LOOK NOW exists for moments where timing and sharpness matter. It should not transmit an arbitrarily old analyzer frame.

## No explicit memory subsystem initially

The Realtime conversation provides the first continuity experiment. A separate rolling state is added only if bench testing reveals specific memory failures.

## Do not ship a normal API key

The phone obtains a short-lived Realtime credential from a minimal server endpoint. The endpoint is not a general backend.

## Keep the Activity visible in V0

Background camera and microphone use adds Android foreground-service restrictions. The first workbench experiment keeps the screen on and app visible.

## Minimal interface

The prototype has camera preview, START, STOP, LOOK NOW, connection state, and a transmitted-frame indicator. Product UI and account management would not answer the core question.

## Build incrementally

Every checkpoint must run on the actual phone before the next subsystem is added. Manual vision is tested before automatic sampling, and automatic sampling before any explicit memory system.

## Eventual glasses remain replaceable input hardware

If V0 succeeds, glasses can later replace the phone camera and possibly the microphone. The core session, sampler, and assistant behavior should remain recognizable.
