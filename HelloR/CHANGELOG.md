# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0] - 2026-06-06

### Added

- Loads R in the browser via WebAssembly (webR) and runs `cat("Hello, R!")`, displaying the output in a terminal-style console.
- Collapsible R session info box showing `sessionInfo()` output.
- Name input: enter your name and R greets you personally using `cat(paste0("Hello, ", name, "!"))`.
- Iris pairs plot (base R) coloured by species with a personalised title, generated via the `png()` device and displayed inline.
- Iris pairs plot (ggplot2 via GGally) coloured by species with a personalised title, generated via `ggpairs()` and displayed inline.

---

## Prior Versions

Every release of HelloR is a single self-contained `index.html` file that runs entirely in the browser.

### How to download an older version

1. Go to the [Releases page](https://github.com/AndyPryke/Apps/releases) and find the release tagged `hellor/vX.Y.Z`.
2. Browse to `HelloR/index.html` in the source tree at that tag, or click **Source code** to download a zip.
3. Save `index.html` to your device and open it in any modern browser — no internet connection required (other than loading webR from CDN).
