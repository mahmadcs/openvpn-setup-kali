# Troubleshooting

## `openvpn: command not found`

OpenVPN isn't installed or isn't on your `PATH`.

```bash
sudo apt update && sudo apt install openvpn
```

## Permission denied

OpenVPN needs root privileges to create a `tun` device and modify routing:

```bash
sudo openvpn --config config/your-provider.ovpn
```

If you still get denied, check that your user is in the `sudo` group and that the `.ovpn` file itself is readable (`chmod 600` restricts to the owner, not to root).

## Configuration file not found

Double-check the path and that you're running the command from the repo root (or use an absolute path):

```bash
ls -la config/
sudo openvpn --config /full/path/to/config/your-provider.ovpn
```

## Authentication failure

- Confirm username/password are correct and haven't expired
- If using a credentials file with `auth-user-pass /path/to/file`, confirm it has exactly two lines (username, then password) with no trailing spaces
- Some providers rotate credentials periodically — check your provider's dashboard/docs for current values

## DNS problems (can't resolve hostnames once connected)

- Check `/etc/resolv.conf` — it should reflect DNS pushed by the VPN
- Some setups need `resolvconf` or `systemd-resolved` integration for the VPN's `dhcp-option DNS` directives to apply automatically
- As a temporary diagnostic (not a permanent fix), try resolving via IP directly: `curl https://1.1.1.1`

## Routing problems (connected, but no internet)

- Confirm `redirect-gateway` is present in the config if you expect all traffic routed through the VPN
- Check `ip route` for a conflicting or missing default route
- Restart the OpenVPN client cleanly (`Ctrl+C` or `pkill openvpn`, then reconnect) rather than layering a second instance on top

## Connection timeout

- Verify the `remote` host/port in the `.ovpn` file is correct and reachable
- Check if the port (commonly 443, 1194, or 53) is blocked by your network/firewall
- Try an alternate server or protocol (TCP vs UDP) if your provider offers multiple options

## Certificate / configuration errors

- Confirm `<ca>`, `<cert>`, and `<key>` blocks (if present) are complete and not truncated — a partially copied `.ovpn` file is a common cause
- Check the OpenVPN log output for the specific TLS error (`TLS handshake failed`, `certificate verify failed`, etc.) rather than guessing

## Missing `tun`/`tap` interface

```bash
lsmod | grep tun
sudo modprobe tun
```

Rare on standard Kali installs, more common in minimal containers/VMs.

## Still stuck?

Re-run with higher verbosity for more diagnostic detail:

```bash
sudo openvpn --config config/your-provider.ovpn --verb 6
```
