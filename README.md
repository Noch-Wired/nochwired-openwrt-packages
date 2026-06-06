# OpenWrt Noch Wired Package Repository

A custom [opkg](https://openwrt.org/docs/guide-user/additional-software/opkg) package repository for OpenWrt, providing pre-built packages for various OpenWrt versions and architectures.

---

## Available Packages

| Package | Description | Version |
|---|---|---|
| `accel-ppp` | High performance PPPoE/PPTP/L2TP/SSTP server | latest |

---

## Supported OpenWrt Versions

| OpenWrt Version | x86_64 | aarch64 |
|---|---|---|
| 24.10.x | ✅ | ✅ |
| 23.05.x | ✅ | ✅ |

---

## Quick Install

SSH into your OpenWrt device and run the following script to auto-detect your version and architecture and add the correct feed:

```sh
# Auto-detect OpenWrt version and architecture
REPO="https://enock295simiyu.github.io/nochwired-openwrt-packages/packages"
VERSION=$(cat /etc/openwrt_release | grep DISTRIB_RELEASE | cut -d= -f2 | tr -d '"' | cut -d. -f1,2)
ARCH=$(opkg print-architecture | tail -1 | awk '{print $2}')

echo "Detected OpenWrt $VERSION on $ARCH"

# Add the feed
echo "src/gz custom $REPO/$VERSION/$ARCH" >> /etc/opkg/customfeeds.conf

# Update and install
opkg update
opkg install accel-ppp
```

---

## Manual Setup

### Step 1 — Add the Repository Feed

SSH into your OpenWrt device:

```sh
ssh root@192.168.1.1
```

Edit `/etc/opkg/customfeeds.conf` and add the line for your OpenWrt version and architecture:

#### OpenWrt 24.10.x

```sh
# x86_64
echo "src/gz custom https://enock295simiyu.github.io/nochwired-openwrt-packages/packages/24.10/x86_64" \
  >> /etc/opkg/customfeeds.conf

# aarch64
echo "src/gz custom https://enock295simiyu.github.io/nochwired-openwrt-packages/packages/24.10/aarch64" \
  >> /etc/opkg/customfeeds.conf
```

#### OpenWrt 23.05.x

```sh
# x86_64
echo "src/gz custom https://enock295simiyu.github.io/nochwired-openwrt-packages/packages/23.05/x86_64" \
  >> /etc/opkg/customfeeds.conf

# aarch64
echo "src/gz custom https://enock295simiyu.github.io/nochwired-openwrt-packages/packages/23.05/aarch64" \
  >> /etc/opkg/customfeeds.conf
```

### Step 2 — Update Package Lists

```sh
opkg update
```

### Step 3 — Install a Package

```sh
opkg install accel-ppp
```

---

## Package Details

### accel-ppp

A high performance PPPoE/PPTP/L2TP/SSTP server for Linux.

#### Install

```sh
opkg install accel-ppp
```

#### Configuration

After installing, the configuration file is located at:

```
/etc/accel-ppp/accel-ppp.conf
```

A sample configuration is provided at:

```
/etc/accel-ppp/accel-ppp.conf.example
```

#### Start / Stop / Restart

```sh
# Start
/etc/init.d/accel-ppp start

# Stop
/etc/init.d/accel-ppp stop

# Restart
/etc/init.d/accel-ppp restart

# Enable on boot
/etc/init.d/accel-ppp enable
```

#### Uninstall

```sh
opkg remove accel-ppp
```

---

## Troubleshooting

### opkg update fails with certificate error

```sh
# Disable SSL verification (not recommended for production)
opkg --no-check-certificate update

# Or install ca-certificates first
opkg install ca-certificates
opkg update
```

### Package not found after opkg update

Make sure the feed URL matches your exact OpenWrt version and architecture:

```sh
# Check your OpenWrt version
cat /etc/openwrt_release

# Check your architecture
opkg print-architecture

# Verify the feed was added correctly
cat /etc/opkg/customfeeds.conf
```

### Verify the feed URL is reachable

```sh
# Test connectivity to the repository
wget -q -O /dev/null \
  https://enock295simiyu.github.io/nochwired-openwrt-packages/packages/24.10/x86_64/Packages \
  && echo "Feed reachable" || echo "Feed not reachable"
```

### Dependency errors during install

```sh
# Force install with missing dependencies
opkg install --force-depends accel-ppp

# Or install dependencies manually first
opkg install kmod-pppoe kmod-pppox
opkg install accel-ppp
```

---

## Feed URL Reference

| OpenWrt Version | Architecture | Feed URL |
|---|---|---|
| 24.10 | x86_64 | `https://enock295simiyu.github.io/nochwired-openwrt-packages/packages/24.10/x86_64` |
| 24.10 | aarch64 | `https://enock295simiyu.github.io/nochwired-openwrt-packages/packages/24.10/aarch64` |
| 23.05 | x86_64 | `https://enock295simiyu.github.io/nochwired-openwrt-packages/packages/23.05/x86_64` |
| 23.05 | aarch64 | `https://enock295simiyu.github.io/nochwired-openwrt-packages/packages/23.05/aarch64` |

---

## Contributing

Contributions are welcome! To request a new package or report an issue:

1. Open an [issue](https://github.com/enock295simiyu/nochwired-openwrt-packages/issues)
2. Or submit a pull request to the [build repository](https://github.com/enock295simiyu/openwrt-accel-ppp)

---

## License

This repository contains build scripts and configurations. Individual packages are subject to their own licenses:

| Package | License |
|---|---|
| accel-ppp | GPL-2.0 |

---

## Links

- [OpenWrt Documentation](https://openwrt.org/docs/start)
- [opkg Usage Guide](https://openwrt.org/docs/guide-user/additional-software/opkg)
- [accel-ppp Project](https://accel-ppp.org/)
- [Build Repository](https://github.com/enock295simiyu/openwrt-accel-ppp)