# Contributing to Apps

This monorepo hosts independent browser apps. Each app lives in its own top-level directory and is versioned, branched, and released separately.

## Starting a New Project

### 1. Create a setup branch

Cut a branch from `main` for the initial project setup:

```
git checkout main
git checkout -b exampleproject/setup
```

### 2. Create the project directory and required files

Every project needs these files:

```
ExampleProject/
├── index.html       # The app itself
├── CLAUDE.md        # App-specific constraints for Claude Code
├── CHANGELOG.md     # Version history (Keep a Changelog format)
└── ROADMAP.md       # Planned features and known issues
```

**`index.html`** — The app is a single self-contained HTML file with no build step. Use React 18 and ReactDOM from CDN (unpkg), transpiled in-browser by Babel Standalone. Do not introduce a bundler, build pipeline, or npm dependencies.

Minimal starting template:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>ExampleProject</title>
  <script crossorigin src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
  <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
</head>
<body>
  <div id="root"></div>
  <script type="text/babel">
    const { useState, useEffect, useRef, useCallback } = React;

    const APP_VERSION = "0.1";

    function App() {
      return <div>ExampleProject v{APP_VERSION}</div>;
    }

    ReactDOM.createRoot(document.getElementById("root")).render(<App />);
  </script>
</body>
</html>
```

**`CLAUDE.md`** — Document app-specific constraints that Claude Code must follow: architecture decisions, data format compatibility rules, layout patterns, anything non-obvious that must be preserved across changes.

**`CHANGELOG.md`** — Start with an unreleased section:

```markdown
# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]
```

**`ROADMAP.md`** — List the features planned for v0.1 and beyond, with rough priority. Keep it short and current.

### 3. Add the app to the landing page

Add a link to the new app in the root `index.html` so it appears on the main Apps page.

### 4. Open a PR and merge to `main`

Once the skeleton is in place, open a PR from `exampleproject/setup` → `main` and merge it using a squash commit. The project now exists on `main`.

---

## Development Releases (v0.x)

Versions `0.x` are for initial development. No compatibility promises are made — anything can change between `0.x` releases.

### 5. Create a release branch

```
git checkout main
git checkout -b exampleproject/v0.1
```

This is the integration branch for all v0.1 work.

### 6. Cut feature branches from the release branch

```
git checkout exampleproject/v0.1
git checkout -b exampleproject/my-feature
```

Work on the feature, then open a PR from `exampleproject/my-feature` → `exampleproject/v0.1` and merge it using a **squash commit**.

Repeat for each feature.

### 7. Prepare for release

Before tagging:

1. Update `APP_VERSION` in `ExampleProject/index.html` to `"0.1"`.
2. Fill in `CHANGELOG.md` — replace `[Unreleased]` with `[0.1] - YYYY-MM-DD` and list what changed.
3. Update `ROADMAP.md` — move completed items, add anything new.
4. Open a final PR from `exampleproject/v0.1` → `main` and merge it using a **merge commit** (not squash, so full history is preserved).

### 8. Tag the release

```
git checkout main
git pull
git tag exampleproject/v0.1.0
git push origin exampleproject/v0.1.0
```

Then create a GitHub Release pointing at that tag. The release title should be the app name and version (e.g. `ExampleProject v0.1.0`); the body should paste the relevant section from `CHANGELOG.md`.

Continue this pattern for `v0.2`, `v0.3`, etc. until the app is stable enough for a `v1.0` release.

---

## Graduating to v1.0

`v1.0` signals the first stable public release — the app is feature-complete enough that you're willing to make backwards-compatibility promises going forward.

Follow the same release branch process, but name the branch `exampleproject/v1.0` and update `APP_VERSION` to `"1.0"`. Tag as `exampleproject/v1.0.0`.

---

## Working on Subsequent Releases (v1.1, v1.2, …)

Follow the same pattern as the initial release, substituting the new version number.

Branch naming:

| Purpose | Branch name |
|---|---|
| Release integration | `exampleproject/v1.1` |
| Individual feature | `exampleproject/my-feature` |

When the release branch is ready, merge it into `main` with a **merge commit**, then tag `exampleproject/v1.1.0`.

For patch releases (bug fixes), cut a branch from the relevant release tag, apply the fix, merge into `main`, and tag `exampleproject/v1.1.1`.

---

## Versioning Reference

Versions follow [Semantic Versioning](https://semver.org/) (`MAJOR.MINOR.PATCH`):

| Increment | When |
|---|---|
| 0.x | Initial development — no compatibility promises |
| MAJOR (1+) | Breaking change — e.g. old save files can no longer load |
| MINOR | New features, fully backwards-compatible |
| PATCH | Bug fixes, backwards-compatible |

Git tags use the format `appname/vMAJOR.MINOR.PATCH` (all lowercase), e.g. `exampleproject/v1.0.0`.

---

## Project Standards

All apps must meet the standards in the root [`CLAUDE.md`](./CLAUDE.md):

- **Cross-browser** — Chrome, Firefox, Safari, and Edge (current + recent), desktop and mobile
- **Touch + mouse** — handle both input types; never assume one or the other
- **Portrait and landscape** — both orientations must be fully functional
