# Inspection Protocol

## Quality gate before analysis

Record whether the footage has:

- a fixed or moving camera;
- a clear belt plane and direction;
- enough spacing between packages;
- motion blur, glare, obstruction, or low resolution;
- a known-size reference or depth information.

If package identity cannot be maintained through the inspection zone, do not claim an exact count. Report the cause and a count range when possible.

## Tracking and counting

- Track by persistent identity, not by bounding-box detections alone.
- Count only a completed crossing of one virtual line.
- Ignore a track that reverses before completing the crossing.
- Merge brief lost-and-found segments when appearance, position, direction, and timing support the same identity.
- Flag overlapping packages rather than silently treating them as one.

## Dimension modes

| Mode | Inputs | Allowed result |
|---|---|---|
| Depth-calibrated | Intrinsics/extrinsics plus depth or stereo | L × W × H with stated tolerance |
| Plane-calibrated | Belt plane and known-size reference | Length/width on plane; height only with additional geometry |
| Reference estimate | Known object visible near the package | Approximate dimensions with low/medium confidence |
| Uncalibrated | Ordinary video only | Pixel footprint or qualitative size; no centimeter claim |

Perspective changes with position. Measure at the calibrated inspection zone, not anywhere in the frame.

## Label orientation

- `yes`: label boundary and meaningful label content are on the top face.
- `no`: label is visible on a side face, underside indication is clear, or the required top face is blank.
- `uncertain`: label is unreadable or face assignment is ambiguous.

This check concerns orientation, not address or barcode validity, unless the user separately requests OCR or barcode verification.

## Visible-damage classes

- `crushed`
- `dented`
- `torn_or_punctured`
- `open_seam`
- `leaking`
- `severely_deformed`
- `loose_or_failed_wrap`
- `no_visible_damage`
- `uncertain`

Multiple classes may apply. Include a short visible-evidence note and the best timestamp.

## Verification

Review at least the first crossing, every flagged exception, overlapping tracks, and the final crossing. Confirm that the number of output rows equals the number of unique completed tracks.

