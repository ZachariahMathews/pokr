# Pokr — Serverless Peer-to-Peer Planning Poker

A lightweight, zero-backend Planning Poker web application for agile engineering teams, deployable purely as a static site on **GitHub Pages**.

## Features

- **Zero Friction & Zero Backend:** No signups, user accounts, or centralized databases.
- **Deterministic Room Links:** Pre-generate room URLs via hash fragments (`/#core-sprint-44`) for calendar invites without server-side routing or 404s.
- **Full-Mesh WebRTC DataChannels:** Peers discover each other via public WebSocket MQTT brokers (`broker.emqx.io` / `test.mosquitto.org` / `broker.hivemq.com`) and communicate directly peer-to-peer via WebRTC (`0.peerjs.com`). If any participant (including the host) disconnects or refreshes, the room continues uninterrupted.
- **Salted SHA-256 Commit-Reveal Protocol:** Eliminates vote peeking and anchoring. During voting, clients broadcast only `SHA-256(value + ":" + salt)` via the Web Crypto API (`crypto.subtle.digest`). On reveal, clients broadcast their cleartext value and salt for independent cryptographic verification by every peer.
- **Zero Text Policy:** No story titles, descriptions, or sensitive ticket data ever enter the application—only standard Fibonacci numbers (`0, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, '?', '☕'`).
- **Dynamic Host Management (`👑`):** First joiner automatically receives the crown, can transfer it to any participant with one click, and if the host disconnects, any participant can claim it.

## Running Locally

Since the entire application is a zero-build single-file bundle (`index.html`), open `index.html` directly in any modern browser or serve the directory with any static file server:

```bash
npx serve .
```

## Deployment

Pushes to `main` automatically deploy to GitHub Pages via [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml) (`actions/deploy-pages@v4`).
