# Verifying Your VPN Connection

Don't assume the tunnel is up just because `openvpn` didn't error out. Confirm it with these checks.

## 1. Confirm the tunnel interface exists

```bash
ip addr show tun0
```

You should see a `tun0` interface with an IP address assigned by the VPN server. (Some configs use `tun1` or another index — check the `dev` line in your `.ovpn` file.)

## 2. Confirm routing goes through the tunnel

```bash
ip route
```

Look for a route via the `tun` interface. If `redirect-gateway` is set in the config, your default route should point through the tunnel.

## 3. Confirm your public IP changed

```bash
curl ifconfig.me
```

Compare this to your public IP *before* connecting. It should now match the VPN server's IP range, not your own ISP's.

## 4. Check DNS resolution isn't leaking

```bash
cat /etc/resolv.conf
```

Ideally this reflects DNS servers pushed by the VPN, not your regular ISP DNS. If it still shows your normal DNS, traffic may be leaking outside the tunnel — see [troubleshooting.md](troubleshooting.md#dns-problems).

## 5. Watch the OpenVPN log for errors

If you started with `--daemon --log`, tail the log and look for warnings after the initial handshake:

```bash
tail -f /tmp/openvpn.log
```

A healthy connection shows `Initialization Sequence Completed` with no repeated reconnect/retry messages afterward.
