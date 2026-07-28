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
- `~/.cargo/bin/wayland-pipewire-idle-inhibit` — screen-dim inhibitor (installed via `cargo`)
- `~/.config/systemd/user/wayland-pipewire-idle-inhibit.service` — systemd unit for screen-dim inhibition

If `cargo` is not available the install script will automatically install `rustup` into `~/.cargo` (no sudo required), then build and install `wayland-pipewire-idle-inhibit`.

## Uninstall

```bash
curl -fsSL https://raw.githubusercontent.com/doggsire/audio-idle-inhibit/main/uninstall | bash
```

## Requirements

- `pactl` (PulseAudio or PipeWire with PulseAudio compatibility)
- `systemd-inhibit`
- `cargo` — if not present, the install script automatically installs `rustup` into `~/.cargo` (no sudo required)

## SteamOS / Steam Deck

Tested on SteamOS in both Desktop Mode and Gaming Mode (Gamescope). Sleep inhibition uses `systemd-inhibit`, and screen-dim inhibition uses the Wayland idle-inhibit protocol which Gamescope honors natively. No input simulation or hacks are required.
