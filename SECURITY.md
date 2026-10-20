# Security Policy

## Supported versions

Only the latest release of the app and of the Home Assistant add-on receives
security fixes.

## Reporting a vulnerability

Please **do not open a public issue** for security problems. Use GitHub's
[private vulnerability reporting](https://github.com/regetskcob/KNX-Monitor/security/advisories/new)
instead. You will get a response as soon as possible; please allow reasonable
time for a fix before disclosing details.

## Scope

KNX Monitor talks to KNX IP gateways on the local network, runs as an add-on
inside Home Assistant behind Ingress, and as the KNX Box on a Raspberry Pi.
Relevant reports include, for example:

- ways to reach the add-on's WebSocket/HTTP server without Home Assistant
  authentication
- leaking ETS project data, passwords or recordings
- sending telegrams to the bus without user action
- using the add-on's UDP relay to reach hosts outside the local network
  (it only forwards KNXnet/IP packets to private, VPN and multicast addresses)

Plain KNXnet/IP tunneling is unencrypted; use KNX IP Secure and Data Secure
where the installation supports them. Weaknesses of the KNX protocol itself are
out of scope.
