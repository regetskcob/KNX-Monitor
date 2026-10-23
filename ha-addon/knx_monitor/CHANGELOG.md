# Changelog

## 1.7.8

- Fixed: the add-on page stayed blank since 1.7.1. It starts again.
- Statistics: load peaks and heatmap cells filter the monitor by time (new
  filter term `time:`, e.g. `time:14:05-14:10`); in English, sender filters
  use `from:`.
- Health check shows its progress while reading and the expected duration
  next to "Check now".
- Overview without an ETS project: guessed date and time values are shown
  large, the raw bytes small below.
- Long values in the history wrap instead of running out of the details.
- KNX box built from this version: the same fixes, its web page starts again.

## 1.7.7

- Nothing changes in Home Assistant.
- KNX box built from this version: the printable sheet has two labels of the
  same size, label 1 for the app on top of the box and label 2 with the
  Wi-Fi on the bottom.

## 1.7.6

- Nothing changes in Home Assistant.
- KNX box built from this version: after the first start the SD card holds
  one printable sheet per language (knx-box-de.pdf, knx-box-en.pdf) with the
  label to cut out, Wi-Fi code, passwords, the link for the Linux app and the
  next steps.

## 1.7.5

- Comments on telegrams, charts and recordings, with up to three
  screenshots each. Telegram lists flag commented addresses; all comments are
  listed under Tools › Comments.
- Timeline for recordings: one lane per group address, drag and zoom, tap a
  telegram to comment on it.
- "Report a problem": start, reproduce the fault, tap "It happened now" each
  time, then send the diagnostic package to your electrician.
- Opening a diagnostic package shows findings: the problem with screenshots,
  automatic hints (connection, Secure, repeats, unanswered reads, outages),
  comments and the busiest addresses.
- One page for all exports: telegrams as PDF, CSV or ETS file, reports (bus
  statistics, health check, commissioning log), a combined ZIP and the last
  exports to share again.
- PDFs are built in the background and break long tables across pages;
  large health reports no longer freeze the app.
- KNX box built from this version: reachable only in its own Wi-Fi by
  default; access over the network cable can be turned on in the app.

## 1.7.3

- The add-on links to its website knx-monitor.de, with support, privacy
  policy and FAQ.

## 1.7.2

- Actuators of all manufacturers use the app's own drawing again. Device
  graphics that were only meant for testing are no longer included.

## 1.7.1

- Topology and device, coupler and actuator details use far less CPU in the
  add-on: no animations, drawings refresh at most once per second.
- Diagnostic package contains the whole session instead of the last 5000
  telegrams shown, and can use days from the long-term history as source.
- Chart tiles show values over time like the full-screen chart, on/off values
  as narrow bands.
- Dialogs, favourites and pins look clearer; the web app starts in the colours
  of the splash screen.

## 1.7.0

- Watchers run in the add-on: they keep working without an open browser and
  send a Home Assistant notification (also as push with `notify_service`).
  Watchers are stored in the add-on and shared by all browsers.
- Bus monitor works next to the add-on: the add-on hands its tunnel over while
  the bus monitor is open (before, the gateway reported no free tunnels).
- Switching away from the KNX Monitor panel and back keeps the last values and
  telegrams instead of starting empty.
- ETS project import shows its progress and opens password-protected ETS5
  projects faster.
- Topology stays smooth with long recordings.
- Charts: on/off values get a row of their own; charts can be prepared with
  project addresses that have not sent yet; "free selection" is gone.
- Project check uses two columns on wider screens.
- Sidebar starts with the overview.

## 1.6.0

- Topology: lines, couplers and devices as a tree with a detail area; line
  couplers, power supplies, actuators, push buttons, presence detectors,
  weather stations and smoke detectors are drawn like the real devices, with
  status LEDs and telegram pulses on the bus cable.
- Switching objects can be sent as 0/1 directly from the object list and the
  topology.
- Project import reads the flags of the communication objects (C R W T U);
  the project check compares the ETS project without any bus traffic.
- Group objects of ETS modules get the right object numbers.
- Import your ETS project once more to see the flags and the corrected
  object numbers.
- Values without a project: 3- and 8-byte values are shown as date and time.
- Bus monitor on narrow screens: value below the destination, full raw data,
  tap a sender to filter the list.
- Very wide screens show the live list as a column next to the overview.
- Diagnostics package for support: error report, recording, project and
  technical details as a ZIP, without personal data.
- Data is now stored per installation; existing data moves into the first
  installation automatically at start.

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
