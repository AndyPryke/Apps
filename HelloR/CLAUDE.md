# Claude Code – HelloR Constraints

## Architecture

HelloR is a **single HTML file** (`index.html`) with no build step.

- React 18 and ReactDOM are loaded from CDN (unpkg)
- JSX is transpiled in-browser by Babel Standalone
- R runs in the browser via [webR](https://docs.r-wasm.org/webr/latest/) loaded as an ES module from `https://webr.r-wasm.org/latest/webr.mjs`
- Do not introduce a build pipeline, bundler, or npm dependencies
- Do not add external CSS frameworks or utility libraries

## webR Initialisation Pattern

webR is loaded via a `<script type="module">` block and exposed to the React app through a window-level Promise:

```html
<script>
  window._webRLoaded = new Promise(function(resolve, reject) {
    window._webRReady = resolve;
    window._webRFailed = reject;
  });
</script>
<script type="module">
  const { WebR } = await import('https://webr.r-wasm.org/latest/webr.mjs');
  const webR = new WebR();
  await webR.init();
  window._webRReady(webR);
</script>
```

The React component awaits `window._webRLoaded` inside a `useEffect`. Do not change this pattern — it is the only reliable way to bridge ES module scope and Babel-transpiled React code.

## Running R Code

Use `webR.evalRString(code)` when the R expression returns a character string.

To capture `cat()` or `print()` output (which goes to stdout rather than returning a value), wrap with `capture.output`:

```javascript
const output = await webR.evalRString(
  'paste(capture.output(cat("Hello, R!")), collapse = "\\n")'
);
```

When passing user input into R code as a string literal, escape backslashes and double-quotes:

```javascript
const safe = value.trim().replace(/\\/g, '\\\\').replace(/"/g, '\\"');
```

## Portrait / Landscape Layout

The app uses a centred single-column layout that works in both orientations without separate branches. Any future control that behaves differently in landscape must detect orientation with `window.matchMedia("(orientation: landscape)")` and render accordingly.
