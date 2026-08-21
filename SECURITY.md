# Security Policy

## Purpose of This Repository

This project is **educational documentation** for configuring OpenVPN on Kali Linux. It is intended for use in authorized lab environments and with VPN services/accounts you are permitted to use.

## Reporting a Security Concern

If you spot an issue with this repository itself (e.g., a documentation error that could lead someone to misconfigure their VPN insecurely, or — despite our precautions — any accidentally committed sensitive data), please open an issue describing the problem in general terms.

**Do not** post real credentials, private keys, certificates, tokens, or other secrets in a GitHub issue, pull request, or commit message — including as part of a "here's what leaked" report. If you believe a secret was accidentally committed to this repo's history, report it privately rather than quoting it publicly, so it can be scrubbed and rotated.

## What This Repository Does Not Cover

This project intentionally does not include and will not accept contributions involving:

- Credential theft or harvesting techniques
- VPN bypass or circumvention methods
- Unauthorized access to networks or systems
- Persistence mechanisms or malware
- Exploitation of third-party services

## Handling Your Own Credentials

- Never commit real `.ovpn` files, keys, certificates, or passwords — see [docs/configuration.md](docs/configuration.md)
- Use the provided `.gitignore`
- Review `git status`/`git diff` before every commit

## Scope

This is a documentation/education repository, not production software — there is no runtime attack surface beyond the shell commands documented here, which should only ever be run against infrastructure you're authorized to use.
