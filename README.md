# RustDesk Server for Home Assistant

[![GitHub Release][releases-shield]][releases]
[![License][license-shield]][license]

A Home Assistant app (formerly "add-on") that runs the free, open-source
[RustDesk Server][rustdesk-server] – `hbbs` (ID/rendezvous) and `hbbr` (relay) –
so your RustDesk clients connect through your own server instead of the public
one.

The app version tells you which RustDesk Server it runs: `1.1.16-1` is
RustDesk Server **1.1.16**, app revision 1.

## Installation

1. In Home Assistant go to **Settings → Apps → App Store**.
2. Open the **⋮** menu → **Repositories** and add
   `https://github.com/drax-uk/ha-rustdesk-server`.
3. Install **RustDesk Server**, start it and read the public key from the log.

Setup, ports, options and key handling are in the
[app documentation](rustdesk-server/DOCS.md).

## Features

- RustDesk Server pinned to a known release, with the download verified
  against the upstream SHA-256 digest.
- Keys and database in the app's config folder: easy to back up and to move.
- Public key printed in the log on every start.
- Optional encrypted-only mode, forced relay, and importing an existing keypair.
- Renovate opens a pull request when a new RustDesk release comes out.

## Credits

This app builds on
[**SankeerthBoddu/ha-rustdesk-server**][upstream-app] by
[Sankeerth Boddu][upstream-author] (MIT licensed). The pinned and verified
RustDesk download, the s6-overlay services, the keypair handling, the
translations, the GitHub workflows and the Renovate setup are adapted from
that project. Thank you!

RustDesk and RustDesk Server are made by the [RustDesk team][rustdesk]
(AGPL-3.0). The RustDesk name and logo belong to them; this project is not
affiliated with or endorsed by RustDesk. The app icon and logo are taken from
the official [RustDesk repository][rustdesk-res] (`res/icon.png`, and
`res/logo-header.svg` rendered to PNG).

## License

MIT – see [LICENSE](LICENSE).

[license]: https://github.com/drax-uk/ha-rustdesk-server/blob/main/LICENSE
[license-shield]: https://img.shields.io/github/license/drax-uk/ha-rustdesk-server.svg
[releases]: https://github.com/drax-uk/ha-rustdesk-server/releases
[releases-shield]: https://img.shields.io/github/v/release/drax-uk/ha-rustdesk-server
[rustdesk]: https://github.com/rustdesk/rustdesk
[rustdesk-res]: https://github.com/rustdesk/rustdesk/tree/master/res
[rustdesk-server]: https://github.com/rustdesk/rustdesk-server
[upstream-app]: https://github.com/SankeerthBoddu/ha-rustdesk-server
[upstream-author]: https://github.com/SankeerthBoddu
