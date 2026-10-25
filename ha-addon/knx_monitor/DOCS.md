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

## Comments, reporting a problem, exports

- **Comments** can be added to telegrams, charts and recordings, with up to
  three screenshots. They are stored per installation in the add-on's data
  folder and listed under Tools › Comments.
- **Report a problem** (Tools): start it, reproduce the fault and tap "It
  happened now" each time, then create the diagnostic package and send it to
  your electrician. Opening a package shows its findings. Names can be left
  out of the package.
- **Exports** collects everything in one page: telegrams as PDF, CSV or ETS
  file, reports, a combined ZIP and the last exports.

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
turned off with `alarm_notifications` in the add-on configuration). With
continuous recording this also works without an open browser.

## Watchers

Watchers (Tools → Watchers) report a group address as soon as it is sent,
optionally only for a certain value. In the add-on they run on the server:
they keep working without an open browser and send a Home Assistant
notification (at most one per watcher and minute; also as push if
`notify_service` is set; can be turned off with `watcher_notifications`). An open KNX Monitor additionally plays the sound
and shows the hit. Watchers are stored in the add-on and shared by all
browsers.

## Automations

The add-on fires Home Assistant events you can use as triggers ("Event" in
the automation editor), also without an open browser. They fire whether or
not notifications are on:

| Event | When | Data |
| --- | --- | --- |
| `knx_monitor_watcher` | a watcher fires | `watcher_id`, `name`, `label`, `address`, `source`, `value` (number rules), `raw`, `installation` |
| `knx_monitor_alarm` | an alarm or fault object switches on or off | `address`, `name`, `kind` (`alarm`/`fault`), `active`, `installation` |
| `knx_monitor_bus` | the bus connection is lost or back | `connected`, `state`, `reason`, `installation` |

Blueprints to import with one click:

- [Watcher fired](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fregetskcob%2FKNX-Monitor%2Fblob%2Fmain%2Fblueprints%2Fautomation%2Fknx_monitor_watcher.yaml)
- [Alarm or fault](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fregetskcob%2FKNX-Monitor%2Fblob%2Fmain%2Fblueprints%2Fautomation%2Fknx_monitor_alarm.yaml)
- [Bus connection lost or back](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fregetskcob%2FKNX-Monitor%2Fblob%2Fmain%2Fblueprints%2Fautomation%2Fknx_monitor_bus.yaml)

Example without a blueprint:

```yaml
triggers:
  - trigger: event
    event_type: knx_monitor_alarm
    event_data:
      active: true
actions:
  - action: notify.mobile_app_my_iphone
    data:
      title: "KNX alarm"
      message: "{{ trigger.event.data.name }} ({{ trigger.event.data.address }})"
```

If you notify yourself through automations, turn off `alarm_notifications`
and `watcher_notifications` below, so you do not get two messages.

## Device and entities (MQTT)

If an MQTT broker is set up in Home Assistant (usually the Mosquitto add-on
with the MQTT integration), the add-on appears as the device **KNX Monitor**
by itself; no credentials to enter. It brings:

| Entity | What |
| --- | --- |
| Bus connected | connectivity of the add-on's bus connection |
| Recording | switch to pause and resume continuous recording |
| Telegrams per minute, Telegrams today | bus activity |
| Repeats per minute, Last telegram | diagnostics |
| Watcher, Alarm | event entities with the same data as the events above |

On the device page under Automations you find **device triggers** for each
watcher and for alarms and faults switching on or off, so you do not need to
type event names. If the add-on stops, the device shows as unavailable.

Without a broker everything else keeps working; the device appears as soon as
one is set up (the add-on checks every 5 minutes).

## Without an own tunnel

If Home Assistant already has the KNX integration set up, the add-on suggests
sharing its connection: connection mode "Home Assistant" instead of an own
tunnel slot on the gateway (see Connection types).

## Configuration

```yaml
alarm_notifications: true
watcher_notifications: true
notify_service: notify.mobile_app_my_iphone
```

- `alarm_notifications`: Home Assistant notification when an alarm or fault
  object triggers (default on).
- `watcher_notifications`: Home Assistant notification when a watcher fires
  (default on).

- `notify_service` (optional): additionally send a push notification via a
  notify service, e.g. `notify.mobile_app_my_iphone`. Leave empty for a
  Home Assistant notification only.

## Support

Bugs and feature requests: <https://github.com/regetskcob/KNX-Monitor/issues>
