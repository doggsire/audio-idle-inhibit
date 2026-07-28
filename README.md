# audio-idle-inhibit

A systemd service that prevents the system from suspending while audio is playing.

## How it works

The `audio-idle-inhibit` service polls `pactl` every second and holds a
`systemd-inhibit --what=sleep` lock while audio is playing, preventing
the system from suspending.

## Install

```bash
curl -fsSL https://raw.githubusercontent.com/doggsire/audio-idle-inhibit/main/install | bash
```

This installs:
- `~/.local/bin/audio-idle-inhibit` — the sleep-inhibit polling script
- `~/.config/systemd/user/audio-idle-inhibit.service` — systemd unit for sleep inhibition

## Uninstall

```bash
curl -fsSL https://raw.githubusercontent.com/doggsire/audio-idle-inhibit/main/uninstall | bash
```

## Requirements

- `pactl` (PulseAudio or PipeWire with PulseAudio compatibility)
- `systemd-inhibit`
- `curl`

## SteamOS / Steam Deck

Tested on SteamOS in both Desktop Mode and Gaming Mode (Gamescope).
