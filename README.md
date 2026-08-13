# stunnel for EdgeRouter 4

Fully static [stunnel](https://www.stunnel.org/) builds for the Ubiquiti EdgeRouter 4 (Cavium Octeon MIPS64).

## Features

- **Fully static binary** - No dependencies, copy and run
- **Automatic updates** - GitHub Actions checks daily for new stunnel releases
- **TLS 1.3 support** - Built with OpenSSL 1.1.1w

## Download

Get the latest release from the [Releases page](../../releases/latest).

| File | Description |
|------|-------------|
| `stunnel` | Standalone binary |
| `stunnel-X.XX-er4-mips64-static.tar.gz` | Package with sample config |
| `*.sha256` | SHA256 checksums |

## Installation

```bash
# Copy to router
scp stunnel ubnt@<router-ip>:/tmp/

# Install on router
ssh ubnt@<router-ip>
sudo cp /tmp/stunnel /usr/local/bin/
sudo chmod +x /usr/local/bin/stunnel

# Verify
/usr/local/bin/stunnel -version
```

## Configuration

Create config directory and configuration file:

```bash
sudo mkdir -p /config/stunnel
```

Example `/config/stunnel/stunnel.conf`:

```ini
pid = /var/run/stunnel.pid
output = /var/log/stunnel.log

[myservice]
client = yes
accept = 127.0.0.1:8080
connect = remote.server.com:443
```

Run stunnel:

```bash
/usr/local/bin/stunnel /config/stunnel/stunnel.conf
```

### Persistence Across Reboots

```bash
sudo mkdir -p /config/scripts/post-config.d
sudo tee /config/scripts/post-config.d/stunnel.sh << 'EOF'
#!/bin/bash
/usr/local/bin/stunnel /config/stunnel/stunnel.conf
EOF
sudo chmod +x /config/scripts/post-config.d/stunnel.sh
```

## Build Details

| Component | Version | Notes |
|-----------|---------|-------|
| OpenSSL | 1.1.1w | Statically linked |
| libc | glibc | Statically linked |
| Architecture | MIPS64 BE | Cavium Octeon |

### Manual Build Trigger

From the [Actions tab](../../actions/workflows/build.yml):

1. Click "Run workflow"
2. Optionally specify a version (leave empty for latest)
3. Check "Force rebuild" to rebuild an existing version

## License

This repository contains build automation only, licensed under AGPL-3.0-or-later. stunnel is licensed under GPLv2+ with OpenSSL exception. See [stunnel.org](https://www.stunnel.org/) for details.
