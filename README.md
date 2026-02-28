# Yandex Internet Speed Test CLI

CLI tool for measuring internet speed using Yandex CDN infrastructure. A terminal alternative to [yandex.ru/internet](https://yandex.ru/internet/) for headless servers.

## Quick Start

```bash
wget -qO- https://raw.githubusercontent.com/stealthsurf-vpn/yandex-speedtest-cli/refs/heads/main/speedtest.sh | bash
```

## What It Measures

- **Download** — download speed via 3 parallel streams from Yandex CDN nodes
- **Upload** — upload speed (50 MB payload) via 3 parallel streams
- **Ping** — HTTP RTT to CDN servers (trimmed mean, 10 samples per server)
- **Connection Info** — IPv4/IPv6, ISP, geolocation

## Example Output

```text
  ╔══════════════════════════════════════╗
  ║      Yandex Internet Speed Test      ║
  ║          by StealthSurf VPN          ║
  ╚══════════════════════════════════════╝

  ✓ Probe servers fetched
  Probe servers: 3 Yandex CDN nodes

  ✓ Connection info received
  IPv4:      203.0.113.1
  ISP:       AS12345 Example ISP
  Location:  Moscow, Moscow, RU

  ✓ Ping measured
  ✓ Download measured
  ✓ Upload measured

  ────────────────────────────────────────
  Results
  ────────────────────────────────────────

  ↓ Download:  85.42 Mbit/s
  ↑ Upload:    42.15 Mbit/s

  ● Ping:      12.34 ms
    min: 10.21 ms / max: 15.67 ms / jitter: 1.23 ms

  ────────────────────────────────────────
  Server: Yandex CDN | 2026-02-28 15:00:00 MSK
```

## Flags

| Flag | Description |
| --- | --- |
| `--debug` | Verbose output: server URLs, data size, timing, per-stream speed breakdown |

```bash
bash speedtest.sh --debug
```

## Dependencies

- `curl`
- `bc`
- `python3`

Missing dependencies are installed automatically via `apt-get`, `yum`, or `apk`.

## How It Works

1. Fetches CDN probe servers from Yandex API (`/internet/api/v0/get-probes`)
2. Detects IP and ISP via `ipv4-internet.yandex.net` and `ipinfo.io`
3. Measures ping — HTTP RTT (`time_starttransfer - time_appconnect`) with top/bottom 25% trimmed
4. Downloads 50 MB files from 3 CDN nodes in parallel (10s, `--max-time`)
5. Uploads 50 MB of random data to 3 CDN nodes in parallel (10s)
6. Sums per-stream speeds to get total throughput

## Supported OS

- Ubuntu / Debian
- CentOS / RHEL / Fedora
- Alpine Linux
