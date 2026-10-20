# Changelog

## 1.5.1

- Security: the add-on's own files – the bus connection with the IP Secure
  passwords and the access data of the KNX box – are no longer reachable
  through the add-on's file store. Projects, recordings and history are
  unaffected.

## 1.5.0

- New look: KNX Monitor now uses Horizont, the design system it shares with
  the other regetskcob projects. Inside Home Assistant the add-on takes Home
  Assistant's neutral colours, so it sits calmly in its frame; the green
  accent, the type and the lines stay.
- New icon: a bus terminal on the green bus cable.
- Values, addresses and times are set in a monospaced font; every status
  shows a glyph and a word, not just a colour.
- Telegram types are letters on a tint: W in the accent, R amber, A blue.
- From 600 px the menu is a sidebar on the left that can be shown and hidden.
- Recordings: a long press on record keeps the last 10 seconds; before a
  replay you can change values, remove telegrams and save the edit as a copy.
- Charts are saved per installation and show up as their own section on the
  overview; addresses are picked like on the dashboard.
- Expert mode (on by default in the add-on) gathers the workshop tools and
  can replay a recording onto the bus after an explicit confirmation; the
  live list shows the hop count for each telegram.
- Search sits in the title bar, the quick filters are one Filter button
  with counts, and the commissioning filters fit in one row.

## 1.4.0

- Quick filters such as "Unanswered" now also work in the overview and
  objects: an address matches if any of its telegrams does.
- Room numbers such as "OG K1" in ETS names are recognised, so objects are
  grouped by the right room.
- Dialogs now match the look of the rest of the app.
- The "Newest" marker stays visible while scrolling.
- More robust saving: catalogue, commissioning and settings are written
  safely, so an interrupted write no longer leaves an empty file.

## 1.3.0

- New: continuous recording. The add-on keeps one bus connection open on its
  own (also without an open browser), reconnects immediately after dropouts
  and writes the history itself. All open views share this one tunnel.
- New: connection types UDP or TCP tunnel, fixed tunnel address with
  fallback, KNXnet/IP routing, KNX IP Secure, and a Home Assistant mode that
  reads the telegrams of the KNX integration without its own tunnel.
- New: KNX Data Secure – import the ETS keyring; secured telegrams are
  decrypted live and in the recording.
- History: finished days are compressed, size limit, write interval,
  availability log that explains gaps (bus disconnected vs. add-on not running).
- Alarm notifications now come from the add-on itself, also without a browser.
- Long-term statistics: activity heatmap, busiest addresses and senders.
- Expert mode is on by default in the add-on.

## 1.2.0

- New: "Health" tool – reads known status addresses, reports newly silent
  addresses, cycle watchdog, plausibility, DPT check per sender, bus quality.
- New: commissioning checklist with notes and PDF report.
- The communication map starts with an overview of all addresses and devices.
- Senders/receivers from the ETS project based on communication flags,
  tunnel addresses shown separately in the topology.
- Thousands separators, kW/kWh, average values and step lines in the overview.
- Prebuilt images for amd64 and aarch64.

## 1.1.0

- Alarm notifications via Home Assistant, bus monitor, topology.

## 1.0.0

- Initial release.
