# wslc-containers-skill

[日本語版 README](README-ja.md) | [Changelog](CHANGELOG.md) | [License: MIT](LICENSE)

An Agent skill that lets **GitHub Copilot running inside WSL** operate Linux
containers through the built-in **WSL container CLI (`wslc.exe`)** — no Docker
Desktop required.

## Features

- Build, run, list, inspect, stop and remove containers via `wslc`.
- Manage images, networks and volumes.
- A wrapper script resolves the Windows-side `wslc.exe` path automatically
  (including paths with spaces such as `/mnt/c/Program Files/WSL/wslc.exe`).
- Built-in safety guidelines for Copilot (confirm before running unfamiliar
  images, check state before destructive operations).

## Requirements

- Windows with WSL **2.9.3 or later** (`wsl.exe --version`; update with `wsl --update`).
- WSL container feature available (`wslc.exe` installed with WSL).
- GitHub Copilot CLI running in a WSL distribution with Windows interop (`/mnt/c`) enabled.

## Repository layout

```
.github/skills/wslc-containers/
├── SKILL.md          # Skill definition and instructions for Copilot
└── scripts/wslc.sh   # Wrapper that locates and runs wslc.exe
```

## Installation

Clone this repository, or copy the skill directory into your project:

```bash
git clone https://github.com/nahisaho/wslc-containers-skill.git
# or, into an existing project
cp -r wslc-containers-skill/.github/skills/wslc-containers <your-project>/.github/skills/
```

Copilot discovers project skills under `.github/skills/`. To use it personally
across projects, place it under `~/.copilot/skills/` instead.

## Usage

Ask Copilot in natural language, for example:

- "Pull alpine and run `uname -a` in it using wslc."
- "Build the Dockerfile in this directory and run it on port 8080."
- "List running containers and show logs for `web`."

You can also call the wrapper directly:

```bash
bash .github/skills/wslc-containers/scripts/wslc.sh version
bash .github/skills/wslc-containers/scripts/wslc.sh image ls
bash .github/skills/wslc-containers/scripts/wslc.sh run --rm docker.io/library/alpine:latest echo hello
```

### Common commands

| Purpose | Command |
|---|---|
| Version / info | `wslc version`, `wslc system info` |
| Pull / list images | `wslc pull <image>`, `wslc image ls` |
| Build | `wslc build -t <tag> <path>` |
| Run | `wslc run --rm -it <image> <cmd>` / `wslc run -d --name <n> -p <h>:<c> <image>` |
| List containers | `wslc container ps` |
| Exec / logs | `wslc exec <c> <cmd>`, `wslc logs <c>` |
| Stop / restart / remove | `wslc stop <c>`, `wslc restart <c>`, `wslc remove <c>` |
| Networks / volumes | `wslc network ls`, `wslc volume ls` |

(In the skill, `wslc` means the wrapper script `scripts/wslc.sh`.)

## Security notes

`wslc run` / `wslc exec` execute arbitrary code in containers. Confirm before
using unfamiliar images or untrusted registries, and validate names, ports and
paths before use.

## Troubleshooting

If `wslc.exe` cannot be found, the wrapper exits non-zero and suggests
`wsl --update`. Make sure WSL is >= 2.9.3 and Windows interop is enabled.

## License

[MIT](LICENSE) © 2026 nahisaho
