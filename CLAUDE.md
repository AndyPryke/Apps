# Claude Code – Project Standards

These constraints apply to all projects in this repository. Follow them on every change.

## Monorepo Structure

This repository contains multiple independent apps. Each app is in its own top-level directory (e.g. `HexTiles/`). Apps are versioned, branched, and released independently.

## Versioning

Each app uses [Semantic Versioning](https://semver.org/) (`MAJOR.MINOR.PATCH`).

- **MAJOR** – breaking change (e.g. saved files from previous versions can no longer load)
- **MINOR** – new features, fully backwards-compatible
- **PATCH** – bug fixes, backwards-compatible

Update `APP_VERSION` in source and `CHANGELOG.md` for the relevant app before releasing.

Git tags use the format `app/vMAJOR.MINOR.PATCH` (e.g. `hextiles/v1.0.0`).

## Release Process

1. Cut a release branch from `main` named `app/vMAJOR.MINOR` (e.g. `hextiles/v1.1`)
2. Cut individual feature branches from the release branch
3. Merge each feature branch into the release branch via a PR with squash commits
4. When all features are complete, merge the release branch into `main` using a **merge commit** (not squash)
5. Create a GitHub Release tagged `app/vMAJOR.MINOR.PATCH` pointing at `main`

## Cross-Browser Compatibility

Target modern evergreen browsers — current and recent versions of Chrome, Firefox, Safari, and Edge on both desktop and mobile.

- Do not use APIs unavailable in Safari or that require polyfills
- Always handle both touch and mouse input; never assume one or the other
- Use `env(safe-area-inset-*)` for layout that may overlap mobile notches or home bars

## Orientation Support

All apps must be fully functional in both **portrait** and **landscape** orientations.

- Use `window.matchMedia("(orientation: landscape)")` to detect and respond to orientation changes
- Both layouts must be fully functional — not just visually scaled
- Any change to one orientation's UI must be reviewed for impact on the other
