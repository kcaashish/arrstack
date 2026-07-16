# arrstack

A lightweight, automated media management stack powered by Docker Compose.

---

## 📦 What's Inside

| Service         | Purpose                                                 |
| :-------------- | :------------------------------------------------------ |
| **Jellyfin**    | Self-hosted media server & streaming                    |
| **Seerr**       | Media request & discovery portal for users              |
| **Prowlarr**    | Indexer management (Torrents/Usenet)                    |
| **Radarr**      | Movie acquisition & library management                  |
| **Sonarr**      | TV show acquisition & library management                |
| **qBittorrent** | Torrent download client                                 |
| **Caddy**       | Reverse proxy with automatic SSL/TLS via Cloudflare DNS |

---

## ⚡ Quick Start

### 1. Prerequisites

Ensure you have **Docker** and the **Docker Compose V2** plugin installed on
your host machine.

### 2. Clone & Configure

```bash
git clone https://github.com/kcaashish/arrstack.git
cd arrstack
cp .env.example .env
cp Caddyfile.example Caddyfile

```

_Open the `.env`, and `Caddyfile` file and customize your paths (e.g., media
storage location) and domain._

### 3. Deploy

```bash
docker compose up -d

```

---

## 📁 Recommended Folder Structure

For fast imports and atomic moves (hardlinks), structure your media directories
like this:

```text
data/
├── appdata/
│   ├── caddy/
│   ├── jellyfin/
│   ├── seerr/
│   ├── prowlarr/
│   ├── sonarr/
│   ├── radarr/
│   ├── qbittorrent/
│   └── bazarr/
├── torrents/
│   ├── movies/
│   └── tv/
└── media/
    ├── movies/
    └── tv/

```

---

## 🛡️ License

Distributed under the MIT License. See `LICENSE` for details.
