# Usage

## Connect

Run OpenVPN in the foreground, pointing at your config:

```bash
sudo openvpn --config config/your-provider.ovpn
```

Watch the output — a successful connection ends with a line similar to:

```text
Initialization Sequence Completed
```

If `auth-user-pass` has no file argument, you'll be prompted for a username and password at this point.

### Running in the background

```bash
sudo openvpn --config config/your-provider.ovpn --daemon --log /tmp/openvpn.log
```

Then monitor the log:

```bash
tail -f /tmp/openvpn.log
```

(Keep log files out of the repo — they can contain server IPs and connection metadata; the `.gitignore` already excludes `*.log`.)

## Monitor

While connected, useful commands in a second terminal:

```bash
ip addr show tun0     # tunnel interface + assigned IP
ip route               # routing table, should show a default route via the tunnel
ps aux | grep openvpn  # confirm the process is running
```

## Verify

See [verification.md](verification.md) for a full checklist.

## Disconnect

**Foreground session:** press `Ctrl+C` in the terminal running OpenVPN.

**Background/daemon session:**

```bash
sudo pkill openvpn
```

Or target a specific PID:

```bash
ps aux | grep openvpn
sudo kill <PID>
```

Confirm the tunnel is down:

```bash
ip addr show tun0   # should report "does not exist" once torn down
```
