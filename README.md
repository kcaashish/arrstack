# arrstack

A lightweight, automated media management stack powered by Docker Compose.

---

## 📦 What's Inside

| Service         | Purpose                                                 |
| :-------------- | :------------------------------------------------------ |
| **Jellyfin**    | Self-hosted media server & streaming                    |
| **Audiobookshelf** | Self-hosted audiobook & podcast server                |
| **Seerr**       | Media request & discovery portal for users              |
| **Prowlarr**    | Indexer management (Torrents/Usenet)                    |
| **Radarr**      | Movie acquisition & library management                  |
| **Sonarr**      | TV show acquisition & library management                |
| **qBittorrent** | Torrent download client                                 |
| **wud**         | Monitor stack for version updates & notify              |
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

## 📁 Folder Structure

Sonarr and Radarr hardlink imports from `torrents/` into `media/`, so a file
doesn't occupy space twice while it seeds. That only works when both directories
reach the container through a **single mount**.

This is the part that's easy to get wrong. `link()` fails with `EXDEV` across two
separate bind mounts even when they're on the same filesystem, because the kernel
compares mount identity rather than device number. Giving two mounts a shared
`/data` path prefix looks like it should work and doesn't — the import silently
falls back to a copy.

That's what the `library/` level is for. It's mounted whole as
`${MEDIA_DIR}/library:/data`, while `appdata/` stays outside it so service
configs aren't exposed to every container.

```text
data/
├── appdata/              # service configs — deliberately outside library/
│   ├── caddy/
│   ├── jellyfin/
│   ├── audiobookshelf/
│   ├── seerr/
│   ├── prowlarr/
│   ├── sonarr/
│   ├── radarr/
│   ├── qbittorrent/
│   ├── wud/
│   └── bazarr/
└── library/              # mounted as a single /data into Sonarr and Radarr
    ├── torrents/
    │   ├── movies/
    │   └── tv/
    └── media/
        ├── movies/
        ├── tv/
        ├── audiobooks/
        └── podcasts/
```

To confirm hardlinks actually work once the stack is up:

```bash
docker exec sonarr sh -c 'touch /data/torrents/.hltest &&
  ln /data/torrents/.hltest /data/media/tv/.hltest; echo "exit=$?";
  rm -f /data/torrents/.hltest /data/media/tv/.hltest'
```

`exit=0` means imports will hardlink. `Cross-device link` means they're being
copied and the library is using twice the space it needs.

---

## 🛡️ License

Distributed under the MIT License. See `LICENSE` for details.
