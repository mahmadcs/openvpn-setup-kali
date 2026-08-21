# Configuration

## What's in a `.ovpn` file

An `.ovpn` file is a plain-text OpenVPN client config. A typical file includes:

- **Connection settings** — `remote <host> <port>`, `proto tcp|udp`, `dev tun`
- **Behavior flags** — `nobind`, `persist-key`, `persist-tun`, `resolv-retry infinite`
- **Auth method** — `auth-user-pass` (username/password) and/or embedded certificates
- **Crypto settings** — `cipher`, `auth`, `data-ciphers`
- **Embedded credentials/certs** — optionally inline `<ca>`, `<cert>`, `<key>` blocks

Because certificates and keys can be embedded directly in the file, **an `.ovpn` file should be treated as sensitive by default**, even if it doesn't obviously "look like" a secret.

## Where to put your file

Place your real, working configuration at:

```text
config/<your-provider>.ovpn
```

This repo's `.gitignore` ignores every `*.ovpn` file except the tracked placeholder `config/example.ovpn`, so your real file stays local and is never committed.

## File permissions

Restrict read access to your own user:

```bash
chmod 600 config/your-provider.ovpn
```

## Handling `auth-user-pass` credentials

You have two options:

**1. Interactive prompt (simplest):**
Leave `auth-user-pass` with no filename argument — OpenVPN will prompt for a username and password each time you connect.

**2. Local credentials file (for repeated/scripted use):**
Create a two-line file (username on line 1, password on line 2):

```text
your_username
your_password
```

Reference it in the `.ovpn` file:

```text
auth-user-pass /path/to/credentials.txt
```

Then lock it down and keep it out of git:

```bash
chmod 600 credentials.txt
```

The `.gitignore` in this repo already excludes `credentials*` and `secrets*` filename patterns.

## Never commit

- Real `.ovpn` files with embedded certs/keys
- Standalone `.key` / `.pem` / `.crt` files
- Credentials files
- `.env` files with real values (only `.env.example` with placeholders is safe)
