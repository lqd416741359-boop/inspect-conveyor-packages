---
name: inspect-conveyor-packages
description: Use when a user uploads conveyor-belt or logistics footage and wants packages tracked, counted, measured or estimated, checked for upward-facing labels, and inspected for visible damage.
---

# Inspect Conveyor Packages

## Overview

Analyze real uploaded conveyor footage. Track each package through one inspection zone, assign a stable ID, and return evidence-backed results for dimensions, label orientation, and visible damage.

## Inputs

Require the video itself. Also use any available calibration information:

- known belt width, checkerboard, ruler, AprilTag, or known-size reference;
- fixed camera position and inspection-zone location;
- depth-camera data when true height is required.

If calibration is missing, continue with visual inspection but mark dimensions as estimates. Never present pixel measurements as centimeters.

## Inspection workflow

1. Read video duration, frame rate, resolution, viewpoint, lighting, and occlusion quality.
2. Select a fixed inspection zone where packages are most visible and separated.
3. Detect each package, assign `package_id`, and track it across frames.
4. Count a package once when its track crosses the inspection line. Do not count detections frame by frame.
5. Choose the clearest frame or short interval for each track.
6. Produce exactly three primary findings per package:
   - `dimensions`: calibrated `L × W × H`, or a clearly labeled estimate;
   - `label_facing_up`: `yes`, `no`, or `uncertain`;
   - `damage`: `no visible damage`, a damage class, or `uncertain`.
7. Preserve evidence: timestamp, confidence, and reason for every non-pass result.
8. Return a results table and, when the available tools support it, an annotated copy of the video.

Read [references/inspection-protocol.md](references/inspection-protocol.md) for measurement rules, failure classes, and quality gates. Use [references/result-schema.md](references/result-schema.md) when producing CSV or JSON.

## Decision rules

### Dimensions

- **Calibrated RGB/depth:** report centimeters with method and expected tolerance.
- **Single calibrated view:** report only dimensions observable from that plane; height may remain unavailable.
- **Uncalibrated video:** report `estimated` or `unavailable`, never false precision.
- **Weight:** unavailable from appearance alone; require a scale or trusted external record.

### Label facing up

Return `yes` only when the shipping label is visible on the upper face. A side-face label is `no`. Use `uncertain` when glare, blur, occlusion, or viewpoint prevents a reliable decision.

### Damage

Check for crushing, dents, tears, punctures, open seams, leakage, severe deformation, and loose wrapping. Do not infer hidden damage. Record only visible evidence.

## Required output

1. Video-level summary: processed duration, unique package count, inspection zone, calibration mode, and limitations.
2. One row per unique package using the result schema.
3. Exceptions list ordered by timestamp.
4. Annotated video only when it can be produced without changing the source file.

## Safety and truthfulness

- Work on a copy; never overwrite the uploaded source.
- State when sampling was used instead of full-frame processing.
- Expose uncertainty instead of forcing pass/fail.
- Treat the result as screening support until validated against the factory's acceptance criteria.

