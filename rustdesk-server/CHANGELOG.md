# Changelog

The app version is `<RustDesk Server version>-<app revision>`, so `1.1.16-1`
is the first release of this app with RustDesk Server 1.1.16.

## 1.1.16-1

Runs **RustDesk Server 1.1.16**.

### Changes

- RustDesk Server is pinned to 1.1.16 (previously whatever was newest at build
  time), and the download is checked against the upstream SHA-256 digest.
- The running hbbs/hbbr version is printed in the log at startup.
- Keys and the database moved from the hidden `/data` folder to the app's
  config folder (`addon_configs/<app>` in Samba / File editor). On the first
  start they are **copied** from `/data` automatically, so existing clients
  keep working with the same key.
- hbbs and hbbr now run as separate s6 services; if either stops, the app
  stops so the watchdog can restart it.
- New option `encrypted_only` (off by default): reject clients that don't use
  the server's key.
- New options `private_key` / `public_key`: bring an existing keypair into a
  fresh install (e.g. when moving to the GitHub-installed app).
- Friendly option and port names in the Home Assistant UI.
- App icon and logo.
- Only `amd64` is supported (aarch64/armv7 removed).

Existing options (`relay_host`, `always_use_relay`, `log_level`) are unchanged.

## 1.0.1

- Fix build on newer Supervisor: default BUILD_FROM to the HA base image.

## 1.0.0

- Initial release: hbbs + hbbr in a single app, keys persisted in /data, public
  key printed to the log.
