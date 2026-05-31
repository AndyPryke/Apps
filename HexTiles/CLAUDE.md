# Claude Code – HexTiles Constraints

## Architecture

HexTiles is a **single HTML file** (`index.html`) with no build step.

- React 18 and ReactDOM are loaded from CDN (unpkg)
- JSX is transpiled in-browser by Babel Standalone
- Do not introduce a build pipeline, bundler, or npm dependencies
- Do not add external CSS frameworks or utility libraries

## Save File Backwards Compatibility

**Every version of the load function must be able to load files saved by all previous versions.**

- See [`SaveFormat.md`](./SaveFormat.md) for the full format specification and version history
- Load all fields defensively: check for presence before using (`if (data.field)`)
- The `version` field is informational only — do not gate loading behaviour on it; use field presence instead
- New fields added to the save format must have sensible in-app defaults when absent from a file

When adding new fields to the save format:
1. Increment `version` in `saveJson`
2. Add the new version entry to `SaveFormat.md`
3. Add a defensive fallback in `handleLoadJson`

## Portrait / Landscape Layout

The toolbar renders as two entirely separate branches depending on `isLandscape` (bottom toolbar in portrait, left sidebar in landscape).

- Any new UI control must be added to **both** branches
- The landscape toolbar scales with viewport height using `clamp()` — apply the same pattern to any new buttons
- The portrait toolbar uses fixed pixel sizes — match the style of existing controls
- Test any UI change in both orientations before considering it done
