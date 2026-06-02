# HexTiles Save Format

JSON files (`.json`) saved and loaded via the 💾 button.

## Current Format — Version 3

```json
{
  "version": 3,
  "tiles": {
    "0,0": { "q": 0, "r": 0, "rotation": 0 },
    "1,0": { "q": 1, "r": 0, "rotation": 3 }
  },
  "viewBox": { "x": -180, "y": -180, "w": 360, "h": 360 },
  "showColors": true,
  "pattern": {
    "curves": [[1, 3], [2, 4], [5, 6]],
    "lineMode": "curved",
    "lineWidth": 10
  }
}
```

### Fields

| Field | Type | Description |
|-------|------|-------------|
| `version` | integer | Save format version. Informational — see compatibility rules below. |
| `tiles` | object | Map of `"q,r"` string keys to tile objects. `q` and `r` are axial hex coordinates; `rotation` is 0–5 (each step = 60°). |
| `viewBox` | object | SVG viewport state: `x`, `y`, `w`, `h` in canvas units. |
| `showColors` | boolean | Whether connected-path colour tracing is enabled. |
| `pattern` | object | Active tile curve design. `curves` is an array of exactly 3 `[sideA, sideB]` pairs (sides numbered 1–6). `lineMode` is `"curved"` or `"straight"`. `lineWidth` is the stroke width in canvas units (2–20, default 10). |

## Version History

| Version | Changes |
|---------|---------|
| 3 | Added `lineWidth` to `pattern`. |
| 2 | Added `showColors` and `pattern`. First version requiring backwards compatibility. |

## Compatibility Rules

- **Load by field presence, not version number.** Use `if (data.field)` checks; do not branch on `data.version`.
- Fields added in newer versions are always optional. The load function falls back to the current app defaults when a field is absent.
- Tile coordinates use axial representation (`q`, `r`). This must not change without a MAJOR version bump.
- When adding a field in a future version, add a row to the version history table above and update the load function.
