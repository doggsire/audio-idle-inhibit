# audio-idle-inhibit

A simple systemd service that prevents the system from going idle while audio is playing, detected via `pactl`.

## How it works

The installer downloads a small shell script (`audio-idle-inhibit`) to `~/.local/bin/` and installs a systemd user service that runs it. The script polls `pactl` every second and uses `systemd-inhibit` to block idle when audio is active.

## Install

```bash
curl -fsSL https://raw.githubusercontent.com/doggsire/audio-idle-inhibit/main/install | bash
```

This installs:
- `~/.local/bin/audio-idle-inhibit` — the polling script
- `~/.config/systemd/user/audio-idle-inhibit.service` — the systemd unit

## Uninstall

```bash
curl -fsSL https://raw.githubusercontent.com/doggsire/audio-idle-inhibit/main/uninstall | bash
```

## Requirements

- `pactl` (PulseAudio or PipeWire with PulseAudio compatibility)
- `systemd-inhibit`

## SteamOS / Steam Deck

This works on SteamOS in both Desktop Mode and Gaming Mode. The service blocks both the `idle` and `sleep` inhibitor locks, which is necessary on the Steam Deck where the system suspends via the sleep path rather than the idle path.
