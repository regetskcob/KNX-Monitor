# KNX Monitor

KNX Monitor as a sidebar panel in Home Assistant: group monitor, objects,
communication map, recordings and bus diagnostics – in the browser, on any
device, also on the go via the Home Assistant app.

## How it works

Browsers cannot speak UDP. The small server in the add-on forwards the app's
KNXnet/IP packets via WebSocket to the gateway (tunneling, NAT mode) and stores
the ETS catalog, recordings and long-term history in the add-on's data folder
(included in backups).

- Access only through Home Assistant (Ingress) – requests to the port from
  anywhere else in the LAN are rejected.
- The add-on keeps **one** connection to the bus, shared by all open views
  (tunnel, routing, IP Secure or the Home Assistant KNX integration). While
  the bus monitor tool is open, the add-on hands its tunnel over to it –
  recording pauses for that time and resumes when you close the bus monitor.
- Use **Pause** on the start screen to free the tunnel, e.g. for ETS.
- The add-on uses the host network so it can reach the gateway via UDP and
  discover gateways on your network.

## Getting started

1. Start the add-on and open **KNX Monitor** in the sidebar.
2. Pick your gateway (discovered automatically) or enter its IP address.
3. Optionally import your ETS project (`.knxproj` or group address CSV) to get
   names, datapoint types and devices.

Gateway address and language are chosen in the app and remembered per browser.

## Continuous recording

Once you connect in the app, the add-on stays connected – also when no browser
is open – and reconnects by itself after gateway restarts or network dropouts.
Every telegram goes into the history (Recordings → History), one file per day;
finished days are compressed. Retention (days), size limit and write interval
are set in the app. Writing less often spares an SD card, but a power cut then
loses the last block.

Once a minute the add-on notes whether the bus was connected. Gaps in the
history are explained in the day list ("bus disconnected for 3 min",
"add-on not running for 11 h").

The history, settings and an imported keyring are part of Home Assistant
backups.

## Connection types

- **UDP / TCP tunnel** – TCP copes better with lossy links (KNXnet/IP 2.0).
  Optionally request a fixed tunnel address; if it is taken, a free one is used.
- **Routing** – multicast 224.0.23.12 via an IP router, uses no tunnel slot.
- **IP Secure** – user and password of the secure tunnel (or import the ETS
  keyring, then they are taken from it).
- **Home Assistant** – no own tunnel: reads what the KNX integration of Home
  Assistant receives and sends via `knx.send` / `knx.read`. Device access
  (programming mode, address check, bus monitor) is not available this way.

## KNX Data Secure

Import the keyring exported from ETS (project → export keyring) under
Project → KNX Secure. Secured group telegrams are then decrypted live and in
the recording; writing to secured addresses is encrypted.

## Alarm notifications

When a group address switches to an alarm or fault state, the add-on creates a
Home Assistant notification (at most one per address every 5 minutes; can be
turned off in the app menu under "Alarm notifications"). With continuous
recording this also works without an open browser.

## Watchers

Watchers (Tools → Watchers) report a group address as soon as it is sent,
optionally only for a certain value. In the add-on they run on the server:
they keep working without an open browser and send a Home Assistant
notification (at most one per watcher and minute; also as push if
`notify_service` is set). An open KNX Monitor additionally plays the sound
and shows the hit. Watchers are stored in the add-on and shared by all
browsers.

## Configuration

```yaml
notify_service: notify.mobile_app_my_iphone
```

- `notify_service` (optional): additionally send a push notification via a
  notify service, e.g. `notify.mobile_app_my_iphone`. Leave empty for a
  Home Assistant notification only.

## Support

Bugs and feature requests: <https://github.com/regetskcob/KNX-Monitor/issues>
