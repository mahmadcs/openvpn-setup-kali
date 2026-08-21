# Installation

## 1. Update package lists

```bash
sudo apt update
```

## 2. Install OpenVPN

```bash
sudo apt install openvpn
```

Kali ships OpenVPN in its default repositories, so this is normally all that's required. If it's already installed, `apt` will simply report it's up to date.

## 3. Confirm the install

```bash
openvpn --version
```

You should see version output starting with something like `OpenVPN 2.6.x`.

## 4. Confirm the `tun` kernel module is available

OpenVPN needs a `tun`/`tap` virtual network device:

```bash
lsmod | grep tun
```

If nothing is returned:

```bash
sudo modprobe tun
```

This is rarely needed on a standard Kali install/kernel, but is a common fix inside minimal containers or some VM images.

## Notes

- Command names above assume a Debian/Kali `apt`-based system.
- If you're running Kali inside a container or minimal cloud image, you may also need `iproute2` and `iptables` (usually already present).
