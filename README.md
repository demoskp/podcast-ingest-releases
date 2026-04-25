# Podcast Ingest — Linux Installer

Automated podcast studio ingestion system. Transfers recordings from ATEM and RODECaster SSDs to your NAS.

## Quick Install

Run this on a fresh Ubuntu/Debian/Linux Mint machine:

```bash
curl -sLO https://github.com/demoskp/podcast-ingest-releases/releases/latest/download/install.sh
sudo bash install.sh
```

The installer will:

1. Ask for your NAS connection details (IP, share path, username, password)
2. Ask for your SSD filesystem labels (defaults: ATEM_SSD, RODE_SSD)
3. Download and install the backend service and frontend app
4. Mount your NAS via SMB (credentials stored securely)
5. Set up udev rules so SSDs auto-mount when plugged in
6. Set up Arduino serial permissions
7. Create a systemd service that starts on boot

## Update

To update to the latest version:

```bash
curl -sLO https://github.com/demoskp/podcast-ingest-releases/releases/latest/download/install.sh
sudo bash install.sh --update
```

This only updates the binaries — your config, NAS mount, and udev rules are preserved.

## What Gets Installed

| Component | Location | Description |
|-----------|----------|-------------|
| Backend service | `/usr/local/bin/podcast-ingest` | API server (runs as systemd service) |
| Frontend app | System packages | Electron app (find in app menu) |
| Config | `/etc/podcast-ingest/.env` | NAS and device settings |
| NAS credentials | `/etc/podcast-ingest/.nas-credentials` | Root-only readable |
| SSD auto-mount | `/etc/udev/rules.d/90-podcast-ssd.rules` | Mounts SSDs by label |
| Arduino access | `/etc/udev/rules.d/91-podcast-arduino.rules` | Serial permissions + `/dev/podcast-arduino` symlink |

## Useful Commands

```bash
# View live logs
journalctl -u podcast-ingest -f

# Restart the backend
sudo systemctl restart podcast-ingest

# Check service status
systemctl status podcast-ingest
```

## Requirements

- Ubuntu, Debian, or Linux Mint
- Network access to your NAS (SMB)
- USB connection to Arduino switch controller
- ATEM and RODECaster SSDs
