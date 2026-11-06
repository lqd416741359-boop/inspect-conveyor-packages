# Result Schema

Use one record per completed package track.

| Field | Meaning |
|---|---|
| `package_id` | Stable ID within the video |
| `crossing_time` | Timestamp at the inspection line |
| `dimensions` | `L × W × H` plus unit, or `unavailable` |
| `dimension_mode` | `depth_calibrated`, `plane_calibrated`, `estimated`, or `uncalibrated` |
| `label_facing_up` | `yes`, `no`, or `uncertain` |
| `damage` | One or more visible-damage classes |
| `confidence` | `high`, `medium`, or `low` |
| `evidence` | Short reason and representative timestamp |

Example:

```json
{
  "package_id": "P-0003",
  "crossing_time": "00:00:06.240",
  "dimensions": "42 × 31 × 28 cm (estimated)",
  "dimension_mode": "estimated",
  "label_facing_up": "no",
  "damage": ["dented"],
  "confidence": "medium",
  "evidence": "Side-face label and crushed upper-right corner visible at 00:00:06.180"
}
```

