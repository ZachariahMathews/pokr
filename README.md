# Pokr — Serverless Peer-to-Peer Planning Poker

A lightweight, zero-backend Planning Poker web application for agile engineering teams, deployable purely as a static site on **GitHub Pages**.

## Features

- **Zero Friction & Zero Backend:** No signups, user accounts, or centralized databases.
- **Deterministic Room Links:** Pre-generate room URLs via hash fragments (`/#core-sprint-44`) for calendar invites without server-side routing or 404s.
- **Enterprise-Safe Discovery & Full-Mesh WebRTC:** Discovery and signaling run strictly over HTTPS/WSS (port 443) via the default PeerJS cloud broker (`0.peerjs.com`) with zero external MQTT brokers or non-standard ports. Peers converge into a direct browser-to-browser WebRTC DataChannel mesh (`JOIN_REQUEST` → `ROSTER_SYNC` / `PEER_ANNOUNCE` with deterministic `myPeerId > targetPeerId` tie-breaking).
- **Salted SHA-256 Commit-Reveal Protocol:** Eliminates vote peeking and anchoring. During voting, clients broadcast only `SHA-256(value + ":" + salt)` via the Web Crypto API (`crypto.subtle.digest`). On reveal, clients broadcast their cleartext value and salt for independent cryptographic verification by every peer.
- **Zero Text Policy:** No story titles, descriptions, or sensitive ticket data ever enter the application—only standard Fibonacci numbers (`0, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, '?', '☕'`).
- **Dynamic Host Management (`👑`):** Joiners probe `${roomName}__lead` on entry; if vacant, they can immediately claim `👑` (binding to `${roomName}__lead`) or wait (polling every 2s). Mid-session `👑` transfers happen instantaneously in-band (`SET_HOST`) over existing DataChannels.

## Running Locally

Since the entire application is a zero-build single-file bundle (`index.html`), open `index.html` directly in any modern browser or serve the directory with any static file server:

```bash
npx serve .
```

## Deployment

Pushes to `main` automatically deploy to GitHub Pages via [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml) (`actions/deploy-pages@v4`).
