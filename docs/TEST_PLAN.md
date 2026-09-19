# V0 Workbench Test Plan

## Test objective

Determine whether sparse images plus continuous voice create convincing short-term visual continuity during ordinary hands-on work.

A successful session should feel like the assistant watched the job, not like it received unrelated photographs.

## Session setup

- Mount the phone with the rear camera covering the work area.
- Use an earbud or headset.
- Arrange several tools, similar parts, a connector, and a multimeter or other changing display.
- Start one uninterrupted 20–30 minute session.
- Work and speak naturally. Avoid coaching the assistant more than necessary.

Record:

- whether each important event was actually transmitted;
- why the frame was sent;
- whether the assistant observed it correctly;
- whether it retained the observation later;
- false or irritating interventions;
- moments requiring manual LOOK NOW.

## Scenarios

### Object moved

1. Place several objects in view.
2. Move one while doing another task.
3. Later ask where it went.

Pass: the assistant identifies its last observed location or clearly states that the move was not observed.

### Meter reading changed

1. Show a readable meter value.
2. Continue working.
3. Change the value.
4. Ask for both the earlier and current readings.

Pass: it distinguishes the readings and their order without inventing precision.

### Connector state

1. Show a connector plugged in.
2. Unplug it during the job.
3. Later ask whether it was plugged in earlier and what changed.

Pass: it reports the observed state transition accurately.

### Similar parts

1. Inspect several visually similar parts one at a time.
2. Mix their positions.
3. Ask which part was already inspected.

Pass: it identifies the correct part when the visual evidence supports it, or admits ambiguity.

### Tool last seen

1. Use a tool and set it down.
2. Move on to another task.
3. Ask where it was last seen.

Pass: it describes the last observed location usefully enough to retrieve the tool.

### Wrong item or connection

1. Establish which item or connection is intended.
2. Deliberately reach for or connect the wrong one.

Pass: the assistant warns before or during the mistake when the evidence is strong. It must not constantly second-guess normal actions.

## Evaluation

Rate each category after the session:

| Category | 0 | 1 | 2 |
|---|---|---|---|
| Short-term recall | Mostly wrong/invented | Mixed | Reliably useful |
| Change awareness | Misses most changes | Sees some | Sees meaningful changes |
| Camera effort | Constant manual help | Occasional help | Nearly hands-free |
| Timing | Too stale to help | Sometimes timely | Usually timely |
| Restraint | Distracting narration | Some unnecessary talk | Quiet unless useful |
| Mistake prevention | Harmful/absent | Unclear | Catches plausible mistakes |

A total score is less important than the failure notes. Those notes decide whether to change sampling, prompting, or memory.

## Success criterion

Proceed beyond V0 if repeated sessions show that ShopEye:

- retains useful short-term state;
- notices meaningful changes;
- needs little manual camera interaction;
- gives honest uncertainty rather than inventing unseen events;
- provides enough value to justify replacing the phone camera with wearable hardware later.

Stop or rethink the architecture if the assistant consistently misses critical moments, cannot maintain continuity, floods the conversation, or requires constant LOOK NOW use.
