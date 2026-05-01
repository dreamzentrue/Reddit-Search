# AGENTS.md

## Cursor Cloud specific instructions

### Base Environment

This repository's cloud environment is configured as a build host for **OpenClaw** (from source). The update script installs Node.js 24 and pnpm so that `pnpm install && pnpm build` works immediately in any OpenClaw checkout.

### Versions


| Tool    | Version                   | Source                                    |
| ------- | ------------------------- | ----------------------------------------- |
| Node.js | 24.x (via NodeSource apt) | system-wide                               |
| pnpm    | latest (via Corepack)     | `corepack prepare pnpm@latest --activate` |


### Shell Environment

`PNPM_HOME` and PATH are configured in both `/etc/profile.d/pnpm.sh` and `~/.bashrc` so all bash sessions (login, interactive, non-interactive via `bash -lc`) get them automatically.

- `PNPM_HOME="$HOME/.local/share/pnpm"`
- pnpm `global-bin-dir` is set to `$PNPM_HOME`
- `pnpm link --global` installs binaries into `$PNPM_HOME`, which is on PATH

### Building OpenClaw from source

```bash
git clone https://github.com/openclaw/openclaw.git /tmp/openclaw
cd /tmp/openclaw
pnpm install
pnpm build
pnpm ui:build
pnpm link --global
openclaw --version
```

### Caveats for containers/headless environments

- The `@discordjs/opus` package build scripts are ignored by pnpm (cosmetic warning, not blocking).
- OpenClaw gateway service uses systemd user services by default. In containers without systemd, run the gateway in the foreground instead: `openclaw gateway --foreground` (or equivalent). This is expected and not a failure.
- `pnpm approve-builds` is interactive and should NOT be run in automated scripts. The opus build skip is harmless.

