# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1] - 2026-05-31

### Added

- Change line width in the tile design screen (#6).
- Custom filename input when saving PNG or JSON (#9).
- Startup tip modal with link to help; can be dismissed permanently (#20).
- Path count and longest path length displayed on the main screen (#18).
- Unlimited zoom out option in tile design screen (advanced, with warning) (#17).
- Prior versions section in this changelog, linked from the help screen (#13).

### Changed

- Help screen redesigned with flowing bullet-point layout (#8).

### Fixed

- Credits (♥) button no longer cut off on narrow mobile screens (#14).
- Save as PNG now caps canvas size at 4096 px so large tile grids export correctly.

## [1.0] - 2026-05-30

### Added

- Initial release.

---

## Prior Versions

Every release of Hex Tile Designer is a single self-contained `index.html` file that runs entirely in the browser with no installation required.

### How to download an older version

1. Go to the [Releases page](https://github.com/AndyPryke/Apps/releases) and find the release tagged `hextiles/vX.Y.Z`.
2. Open the release, then browse to `HexTiles/index.html` in the source tree at that tag, or click **Source code** to download a zip.
3. Save `index.html` to your device and open it in any modern browser — no internet connection required after that.

### Save file compatibility

All versions of Hex Tile Designer can load save files (`.json`) created by any previous version.
