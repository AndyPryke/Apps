# Claude Code – HelloR Constraints

## Architecture

HelloR is a **single HTML file** (`index.html`) with no build step.

- React 18 and ReactDOM are loaded from CDN (unpkg)
- JSX is transpiled in-browser by Babel Standalone
- Do not introduce a build pipeline, bundler, or npm dependencies
- Do not add external CSS frameworks or utility libraries
