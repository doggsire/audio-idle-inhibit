# audio-idle-inhibit

A systemd service that prevents the system from going idle or dimming the screen while audio is playing.

## How it works

Two components run as systemd user services:

- **`wayland-pipewire-idle-inhibit`** — monitors PipeWire for active audio and inhibits screen dimming via the standard Wayland [`zwp_idle_inhibit_manager_v1`](https://wayland.app/protocols/idle-inhibit-unstable-v1) protocol. Gamescope (SteamOS Gaming Mode) supports this protocol since v3.15.14, so screen dimming is properly blocked without hacks.

- **`audio-idle-inhibit`** — polls `pactl` every second and holds a `systemd-inhibit --what=sleep` lock while audio is playing, preventing the system from suspending.

## Install

```bash
curl -fsSL https://raw.githubusercontent.com/doggsire/audio-idle-inhibit/main/install | bash
```

This installs:
- `~/.local/bin/audio-idle-inhibit` — the sleep-inhibit polling script
- `~/.config/systemd/user/audio-idle-inhibit.service` — systemd unit for sleep inhibition
- `~/.local/bin/wayland-pipewire-idle-inhibit` — screen-dim inhibitor (pre-built binary)
- `~/.config/systemd/user/wayland-pipewire-idle-inhibit.service` — systemd unit for screen-dim inhibition

The install script downloads a pre-built `wayland-pipewire-idle-inhibit` binary directly from
this repo's [GitHub Releases](https://github.com/doggsire/audio-idle-inhibit/releases/latest),
so no Rust or `cargo` is required. If the pre-built binary is unavailable it falls back to
`cargo install` automatically.

## Uninstall

```bash
curl -fsSL https://raw.githubusercontent.com/doggsire/audio-idle-inhibit/main/uninstall | bash
```

## Requirements

- `pactl` (PulseAudio or PipeWire with PulseAudio compatibility)
- `systemd-inhibit`
- `curl`

## SteamOS / Steam Deck

Tested on SteamOS in both Desktop Mode and Gaming Mode (Gamescope). Sleep inhibition uses `systemd-inhibit`, and screen-dim inhibition uses the Wayland idle-inhibit protocol which Gamescope honors natively. No input simulation or hacks are required.
