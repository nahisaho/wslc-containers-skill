# Changelog

All notable changes to this project are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/).

## [Unreleased]

### Added

- `README.md` (English) and `README-ja.md` (Japanese) documentation.
- MIT `LICENSE`.
- This `CHANGELOG.md`.

## [0.1.0] - 2026-10-08

### Added

- `wslc-containers` Agent skill (`.github/skills/wslc-containers/SKILL.md`) for
  operating Linux containers via the WSL container CLI (`wslc.exe`) from
  GitHub Copilot running inside WSL.
- `scripts/wslc.sh` wrapper that locates `wslc.exe` on the Windows side
  (default path, common Program Files locations, then `where wslc.exe`).

[Unreleased]: https://github.com/nahisaho/wslc-containers-skill/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/nahisaho/wslc-containers-skill/releases/tag/v0.1.0
