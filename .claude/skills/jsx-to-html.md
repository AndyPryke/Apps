# jsx-to-html

Convert a React JSX app into a self-contained HTML file that runs directly in any browser with no build step. Use this when the user wants to ship or share a React component/app as a single standalone file.

## When to invoke

- User says "convert this to a standalone HTML file", "make this work without a build step", "create a self-contained version"
- User references a `.jsx`, `.tsx`, or `.js` React file and wants a deployable HTML output
- User asks to "do what was done" with HexTiles or Particles (the existing apps in this repo use this pattern)

## What this skill produces

A single `.html` file that:
- Loads React 18, ReactDOM, and Babel Standalone from CDN (no npm, no bundler)
- Transpiles JSX in the browser at runtime via `<script type="text/babel">`
- Mounts the app at `<div id="root">`
- Requires zero server infrastructure

## Step-by-step process

### 1. Read the source

Read the target JSX/TSX file(s). Identify:
- All `import` statements (what libraries are used)
- Which React hooks are used (`useState`, `useEffect`, `useRef`, etc.)
- Whether TypeScript types/annotations are present
- Whether Tailwind className utilities are used
- The root component name (default export or convention)

### 2. Map imports to CDN scripts

Build the `<head>` CDN list using this table. Add only what the source actually imports.

| Import | CDN script tag | Global name |
|--------|---------------|-------------|
| `react` | `<script crossorigin src="https://unpkg.com/react@18/umd/react.production.min.js"></script>` | `React` |
| `react-dom/client` or `react-dom` | `<script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>` | `ReactDOM` |
| Babel (always needed) | `<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>` | — |
| `matter-js` | `<script src="https://unpkg.com/matter-js@0.19.0/build/matter.min.js"></script>` | `Matter` |
| `tailwindcss` or Tailwind classNames detected | `<script src="https://cdn.tailwindcss.com"></script>` | — |
| `d3` | `<script src="https://unpkg.com/d3@7/dist/d3.min.js"></script>` | `d3` |
| `three` | `<script src="https://unpkg.com/three@0.160.0/build/three.min.js"></script>` | `THREE` |
| `chart.js` | `<script src="https://unpkg.com/chart.js@4/dist/chart.umd.min.js"></script>` | `Chart` |
| `tone` | `<script src="https://unpkg.com/tone@14/build/Tone.js"></script>` | `Tone` |

For any library not in this table, search for its unpkg CDN URL and the global name it exposes (check the library's UMD build). Ask the user if uncertain.

### 3. Transform the JSX code

Apply these transformations to the component source:

**a. Remove all import statements**
Delete every `import ... from '...'` line.

**b. Replace React hook imports with a global destructuring**
Add at the top of the script block, using only the hooks actually present in the code:
```js
const { useState, useEffect, useRef, useCallback, useMemo, useReducer, useContext, createContext } = React;
```

**c. Remove TypeScript annotations** (if the source is `.tsx`)
- Remove `: Type` parameter annotations
- Remove `<Type>` generic calls (e.g. `useState<string>` → `useState`)
- Remove `interface` and `type` declarations
- Remove `as Type` casts
- Remove `React.FC`, `React.ReactNode`, etc. type annotations

**d. Remove export keywords**
- `export default function App()` → `function App()`
- `export function Foo()` → `function Foo()`
- `export const x =` → `const x =`

**e. Replace the entry point**
Remove any existing `render(...)` call or CRA/Vite entry point and add at the very end:
```js
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);
```
Use the actual root component name.

**f. Keep everything else verbatim**
Do not refactor, simplify, rename, or restructure the component logic. The conversion is purely mechanical.

### 4. Assemble the HTML file

Use this shell. Fill in the placeholders:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>{{APP_TITLE}}</title>
  {{TAILWIND_SCRIPT_IF_NEEDED}}
  <script crossorigin src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
  <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
  {{EXTRA_CDN_SCRIPTS}}
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
  <style>
    {{GLOBAL_STYLES}}
  </style>
</head>
<body>
  <div id="root"></div>
  <script type="text/babel">
    const { {{USED_HOOKS}} } = React;

    {{TRANSFORMED_COMPONENT_CODE}}

    const root = ReactDOM.createRoot(document.getElementById('root'));
    root.render(<{{ROOT_COMPONENT}} />);
  </script>
</body>
</html>
```

Notes on ordering:
- Tailwind CDN must come **before** React scripts if used (it scans the DOM)
- Babel Standalone must come **last** among CDN scripts (it processes `text/babel` blocks after page load)
- Put app-wide CSS resets in `<style>` (e.g. `html, body, #root { margin: 0; height: 100%; }`)

### 5. Choose the output filename

- Default: place the output alongside the source file, named `index.html` (or `<appname>.html` if index is taken)
- If converting an existing HTML file to a cleaner version, suffix with `_v2.html`
- Ask the user if it's unclear

### 6. Write the file and report

Write the assembled HTML file. Then report:
- Output file path
- CDN scripts added and why
- Any imports that couldn't be automatically mapped (and what to do about them)
- Any TypeScript that required non-trivial stripping

## Common pitfalls

- **CSS Modules / `.module.css` imports**: These cannot be used in standalone HTML. Convert to inline styles or Tailwind utilities. Flag this to the user.
- **Relative asset imports** (`import logo from './logo.png'`): These won't work. Inline SVGs or use absolute URLs. Flag to user.
- **Dynamic `import()`**: Babel standalone does not support dynamic imports in all environments. Flag to user.
- **`process.env.*`**: No Node env in browser. Inline the values or remove the guards.
- **TypeScript path aliases** (`@/components/...`): Strip these correctly — they won't resolve in-browser.

## Style guidance: inline styles vs Tailwind

- If the source uses Tailwind `className=` utilities → keep them, add Tailwind CDN
- If the source uses CSS Modules or styled-components → convert to inline style objects, matching the existing style as closely as possible
- If the source uses plain CSS → copy into the `<style>` block
- Do not mix approaches beyond what the source already does

## Example: what a complete output looks like

See `/HexTiles/index.html` (inline styles + SVG canvas, no Tailwind) and `/Particles/snowflakes_gemini_3_9.html` (Tailwind + Matter.js) in this repository as reference implementations of the pattern.
