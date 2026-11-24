# RustDesk Server

Runs the open-source [RustDesk Server][rustdesk-server] (`hbbs` ID/rendezvous
and `hbbr` relay) on Home Assistant. The RustDesk version is part of the app
version: `1.1.16-1` runs RustDesk Server 1.1.16.

## Options

| Option | Description |
|---|---|
| `relay_host` | Public hostname or IP (optionally `:port`) of your relay, e.g. `rustdesk.example.com`. Leave empty on a LAN-only setup. |
| `always_use_relay` | Force all connections through the relay (disables P2P). |
| `encrypted_only` | Reject clients that don't use this server's key. Turn on once every client has the key. |
| `log_level` | `trace`, `debug`, `info`, `warn` or `error`. |
| `private_key` / `public_key` | Optional. Bring an existing keypair into a new install. Both or neither. |

## Ports

The app uses host networking. Make sure nothing else on the host uses these
ports, and forward them on your router for access from outside your home:

| Port | Protocol | Used by |
|---|---|---|
| 21115 | TCP | hbbs – NAT type test |
| 21116 | TCP + **UDP** | hbbs – ID registration / hole punching |
| 21117 | TCP | hbbr – relay |
| 21118 | TCP | hbbs – web client (optional) |
| 21119 | TCP | hbbr – web client (optional) |

If you use a DNS name, it must point straight at your home IP. A proxied
Cloudflare record (orange cloud) does not work; use **DNS only**.

## Client setup

1. Start the app and open the **Log** tab. The public key is printed there.
2. In the RustDesk client: **Settings → Network → ID/Relay server**
   - **ID server:** your HA IP or public hostname
   - **Relay server:** the same host (or what you set in `relay_host`)
   - **API server:** leave empty
   - **Key:** the public key from the log

## Keys, database and backups

The keys (`id_ed25519`, `id_ed25519.pub`) and the database (`db_v2.sqlite3`)
are stored in the app's config folder, visible as `addon_configs/<app>` over
Samba or in the File editor. They are kept across updates and included in Home
Assistant backups.

**Never delete or regenerate the keys** – every client would need the new key.

### Updating from 1.0.x

Version 1.0.x stored the keys in the hidden `/data` folder. On the first start
of 1.1.16-1 they are copied to the config folder automatically (the originals
stay in `/data`). The log shows `Copying existing keys and database from /data
to /config`, followed by the same public key as before.

### Moving to another install (e.g. from a local copy to the GitHub repository)

A different install source gives the app a new ID, so it starts with an empty
config folder. Before you remove the old app:

1. Copy `id_ed25519` and `id_ed25519.pub` (and `db_v2.sqlite3*`) from the old
   app's `addon_configs` folder, **or** copy their contents.
2. Install the new app, then either put the files in its `addon_configs` folder
   or paste the contents into the `private_key` / `public_key` options.
3. Start it and check the log shows the same public key as before.

[rustdesk-server]: https://github.com/rustdesk/rustdesk-server
