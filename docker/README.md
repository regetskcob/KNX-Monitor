# KNX Monitor in Docker (ioBroker, NAS, Proxmox)

The server behind the Home Assistant add-on and the KNX box also runs as a
plain Docker container: bus monitor and diagnostics in the browser,
continuous recording, watchers and alarms, and optionally an MQTT connection
for ioBroker or any other system. Images for amd64 and arm64 (Raspberry Pi 4/5
with a 64-bit OS) under one name: `ghcr.io/regetskcob/knx-monitor`.

Using Home Assistant? Take the [add-on](https://knx-monitor.de/addon/) instead,
it is set up with one click and needs no password.

## Start

1. Copy [`compose.yaml`](compose.yaml) to a folder on the host. Next to it
   create a file `.env` with your own password (at least 8 characters):
   `BOX_PASSWORD=your-own-password`. Without it Compose refuses to start, as
   the container is reachable from your network.
2. `docker compose up -d`
3. Open `http://<host>:8765`, sign in with any user name and the password.
4. Pick your KNX IP gateway (or enter its address), import your ETS project.

The container uses the host network, because KNXnet/IP over UDP, the gateway
search and routing need it. History, project and settings live in `./data`.

The Linux app and, later, the other apps can connect to the container too:
in the app "Add KNX box", enter `<host>:8765` and the password.

Update: `docker compose pull && docker compose up -d`.

## Settings

| Variable | Meaning |
| --- | --- |
| `BOX=1` | Standalone mode with password (required outside Home Assistant). |
| `BOX_PASSWORD` | Password for the browser and for pairing apps, at least 8 characters. Used on the first start; change it later in the app under box settings. |
| `BOX_NAME` | Name shown in the apps. |
| `LAN_ACCESS=yes` | Reachable from your network (required in Docker). |
| `PORT` | Web port, default 8765. |
| `TZ` | Time zone for the daily history files, e.g. `Europe/Berlin`. |
| `MQTT_HOST`, `MQTT_PORT`, `MQTT_USER`, `MQTT_PASSWORD`, `MQTT_SSL` | Optional MQTT broker (port 1883 by default, `MQTT_SSL=true` for TLS). |
| `MQTT_DISCOVERY=false` | Leave out the Home Assistant discovery topics (recommended for ioBroker). |

## ioBroker

1. In ioBroker install the **MQTT** adapter (Broker/Client), mode **Server/broker**,
   port 1883, with a user and password.
2. In `compose.yaml` set `MQTT_HOST` to your ioBroker host, `MQTT_USER` and
   `MQTT_PASSWORD`, and `MQTT_DISCOVERY: "false"`. Restart the container.
3. The values appear as states under `mqtt.0.knx_monitor`:

| State | Value |
| --- | --- |
| `connected` | bus connected, `true`/`false` |
| `recording` | continuous recording on, `true`/`false` |
| `per_minute`, `repeats_per_minute` | telegrams and repeats per minute |
| `today` | telegrams today |
| `last` | time of the last telegram (ISO 8601, UTC) |
| `status` | `online`/`offline` (offline if the container stops) |
| `event.watcher` | JSON of the last watcher hit: name, group address, source, value |
| `event.alarm` | JSON of the last alarm or fault change: `event_type` `alarm_on`, `alarm_off`, `fault_on` or `fault_off` |
| `watcher.<id>`, `alarm.<type>` | the same per watcher and per alarm type |
| `state` | all values above as one JSON |

To pause or resume the recording, set `mqtt.0.knx_monitor.recording.set` to
`ON` or `OFF`.

Watchers are set up in KNX Monitor under Tools › Watchers; each hit updates
`event.watcher`, so a script or Blockly trigger on that state can switch,
notify or log.

A native ioBroker adapter (admin tab, states without MQTT) is planned if
enough users want it: vote with 👍 in
[issue #3](https://github.com/regetskcob/KNX-Monitor/issues/3).

## Notes

- 32-bit operating systems (armv7) are not supported; use a 64-bit OS.
- No Home Assistant events or notifications here; use MQTT and your system's
  automations instead.
- On its own the container sends nothing to the internet; data only goes to
  destinations you set up yourself (MQTT, ntfy, webhook, InfluxDB).
- Data in `./data` is not encrypted by the container; protect the folder and its
  backups yourself.
