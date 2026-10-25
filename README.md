# KNX Monitor

Bus monitor and diagnostic tool for KNX. Available today as a Home Assistant
add-on and for Linux; the apps for iPhone, iPad, Android, Mac and Windows
follow in the coming weeks. The KNX Box, a Raspberry Pi that records on site,
is a first preview, set up with the app; its image is listed under Downloads.

**Website:** <https://knx-monitor.de>

This repository hosts the Home Assistant add-on repository, the Linux and
KNX Box downloads, and the issue tracker. The source code of KNX Monitor is not
public.

[![Add repository to my Home Assistant](https://my.home-assistant.io/badges/supervisor_add_addon_repository.svg)](https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https%3A%2F%2Fgithub.com%2Fregetskcob%2FKNX-Monitor)

## Downloads

- **Linux:** [latest release](https://github.com/regetskcob/KNX-Monitor/releases/latest)
- **iPhone, iPad, Mac:** App Store, coming in the next weeks
- **Android:** Google Play, coming in the next weeks
- **Windows:** coming in the next weeks
- **KNX Box (Raspberry Pi image, preview):** releases tagged `box-v…`
- **Docker (ioBroker, NAS, Proxmox):** `ghcr.io/regetskcob/knx-monitor`, see [docker/README.md](docker/README.md)

## Home Assistant add-on

1. Click the button above, or go to **Settings → Add-ons → Add-on Store → ⋮ →
   Repositories** and add `https://github.com/regetskcob/KNX-Monitor`.
2. Install **KNX Monitor**, start it and enable **Show in sidebar**.

Configuration: [DOCS.md](ha-addon/knx_monitor/DOCS.md).

## Support

Bugs, questions and requests: [issues](https://github.com/regetskcob/KNX-Monitor/issues).
Please do not upload your ETS project (`.knxproj`) or recordings publicly, they
can contain addresses, room names and passwords.

Security issues: see [SECURITY.md](SECURITY.md).

## License

KNX Monitor is proprietary software, see [LICENSE](LICENSE). Versions released
before 20 October 2026 were published under the MIT License and remain
available under it.

## Trademark

KNX is a trademark of the KNX Association. This project is not affiliated with
or endorsed by the KNX Association.
