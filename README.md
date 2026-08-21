# Kali Linux OpenVPN Setup Guide

A clean, beginner-friendly guide to configuring and using OpenVPN (`.ovpn`) client connections on Kali Linux — written for cybersecurity students and lab environments.

## Table of Contents

- [Overview](#overview)
- [Learning Objectives](#learning-objectives)
- [Requirements](#requirements)
- [Installation](#installation)
- [Configuration](#configuration)
- [Connecting to a VPN](#connecting-to-a-vpn)
- [Verifying the Connection](#verifying-the-connection)
- [Disconnecting](#disconnecting)
- [Troubleshooting](#troubleshooting)
- [Security Notes](#security-notes)
- [Ethical / Legal Use](#ethical--legal-use)
- [Repository Structure](#repository-structure)
- [License](#license)

## Overview

This repository documents how to set up and use an **OpenVPN client** on **Kali Linux**, using a standard `.ovpn` configuration file.

**OpenVPN** is an open-source VPN protocol/software that creates an encrypted tunnel between your machine and a VPN server, routing your traffic through that server. A `.ovpn` file bundles the connection settings (server address, port, protocol, ciphers) and, often, the certificates/keys needed to authenticate.

**Why this matters in a cybersecurity lab:** VPNs are used to reach isolated lab networks, practice safe/anonymized connectivity, simulate remote-access scenarios, and understand how encrypted tunnels, routing, and DNS behave — all foundational networking concepts for security work.

This repo teaches the full lifecycle: install → configure → connect → verify → disconnect → troubleshoot, with security best practices baked in at every step.

## Learning Objectives

By following this guide you will be able to:

- Understand core VPN and OpenVPN concepts
- Install and configure OpenVPN on Kali Linux
- Safely import and reference an `.ovpn` configuration
- Connect to and disconnect from a VPN via the CLI
- Verify that a VPN tunnel is actually active
- Troubleshoot common connection failures
- Handle VPN credentials and certificates securely (and avoid leaking them into version control)

## Requirements

- Kali Linux (rolling or recent release)
- `openvpn` package
- A terminal with `sudo`/root privileges
- A valid `.ovpn` configuration from an **authorized** VPN provider or your own account
- An active internet connection

> Package names and exact commands may vary slightly between Kali versions — check `apt-cache policy openvpn` if in doubt.

## Installation

```bash
sudo apt update
sudo apt install openvpn
```

Verify the install:

```bash
openvpn --version
```

See [docs/installation.md](docs/installation.md) for a full walkthrough.

## Configuration

Place your `.ovpn` file in the `config/` directory. This repo ships only a **sanitized placeholder**:

```text
config/example.ovpn
```

Replace it locally with your own authorized provider's file — do **not** commit your real one. See [docs/configuration.md](docs/configuration.md) for details on file permissions and safe credential handling.

## Connecting to a VPN

```bash
sudo openvpn --config config/example.ovpn
```

- `--config` — path to your `.ovpn` file
- Runs in the foreground by default; use `--daemon` to background it, or open a second terminal to keep monitoring
- If the file uses `auth-user-pass` without a credentials file, you'll be prompted for a username and password interactively

Full explanation in [docs/usage.md](docs/usage.md).

## Verifying the Connection

```bash
ip addr show tun0          # confirm the tun interface exists and has an IP
ip route                   # confirm traffic is routed through the tunnel
curl ifconfig.me           # confirm your public IP matches the VPN server
```

Details in [docs/verification.md](docs/verification.md).

## Disconnecting

If running in the foreground: press `Ctrl+C`.

If backgrounded:

```bash
sudo pkill openvpn
```

or, more precisely, find and kill the specific process:

```bash
ps aux | grep openvpn
sudo kill <PID>
```

## Troubleshooting

See [docs/troubleshooting.md](docs/troubleshooting.md) for fixes to:

- `openvpn: command not found`
- Permission denied
- Configuration file not found
- Authentication failure
- DNS resolution issues
- Routing problems
- Connection timeouts
- Certificate/config errors
- Missing `tun`/`tap` interface

## Security Notes

- **Never** commit real `.ovpn` files, private keys, certificates, or passwords
- Use the provided `.gitignore` — it blocks `*.ovpn`, `*.key`, `*.pem`, `.env`, etc., while keeping `config/example.ovpn` tracked
- Store real credentials outside the repo (or in a git-ignored local file) — see [docs/configuration.md](docs/configuration.md)
- Review `git status` / `git diff` before every commit to catch accidental secret exposure
- Treat any `.ovpn` file as sensitive — it can contain embedded certificates and private keys, not just a server address

## Ethical / Legal Use

Only connect to VPN servers and networks you are **authorized** to use. This repository is for legitimate VPN configuration, Linux administration, and networking education — it does not cover or endorse unauthorized access, credential theft, or bypassing network controls.

## Repository Structure

```text
kali-linux-openvpn-guide/
├── README.md
├── LICENSE
├── .gitignore
├── CONTRIBUTING.md
├── SECURITY.md
├── docs/
│   ├── installation.md
│   ├── configuration.md
│   ├── usage.md
│   ├── troubleshooting.md
│   └── verification.md
├── config/
│   └── example.ovpn
├── scripts/
│   └── README.md
└── assets/
    └── screenshots/
```

### Conceptual Connection Flow

```text
Kali Linux
    ↓
OpenVPN Client
    ↓
.ovpn Configuration
    ↓
VPN Server
    ↓
Encrypted VPN Tunnel
    ↓
VPN Network
```

*(Conceptual diagram — actual behavior depends on your provider's configuration.)*

## License

MIT — see [LICENSE](LICENSE). This license covers the documentation and scripts in this repository only; it does not grant any rights to third-party VPN services or their configuration formats.
