# mbe-infra

Docker Compose setup for the MBE RCS platform (backend + customer frontend + admin frontend).

## Repos

| Service | Repo |
|---|---|
| Backend (Node.js) | [cxa10/jcdb](https://github.com/cxa10/jcdb) |
| Customer Frontend (React) | [cxa10/mbe-rcs-platform-dev](https://github.com/cxa10/mbe-rcs-platform-dev) |
| Admin Frontend (React) | [cxa10/mbe-admin-panel-dev](https://github.com/cxa10/mbe-admin-panel-dev) |

## Voraussetzungen

- Docker & Docker Compose installiert
- Alle drei Service-Repos geklont als Geschwister-Ordner neben `mbe-infra`:

```
~/Desktop/
├── mbe-infra/         ← dieses Repo
├── jcdb/
├── mbe-rcs-platform-dev/
└── mbe-admin-panel-dev/
```

- `.env` Datei in `jcdb/` angelegt (Vorlage: `jcdb/.env.example`)

---

## Lokal starten (mit MySQL, Dev-Modus)

Startet alle drei Services **plus** einen lokalen MySQL 8 Container:

```bash
docker compose -f docker-compose.yml -f docker-compose.dev.yml up --build
```

| Service | URL |
|---|---|
| Backend API | http://localhost:3001 |
| Customer Frontend | http://localhost:80 |
| Admin Frontend | http://localhost:81 |
| MySQL | localhost:3306 |

Stoppen:

```bash
docker compose -f docker-compose.yml -f docker-compose.dev.yml down
```

---

## Auf Netcup deployen (Production)

Auf dem Server läuft eine externe MySQL-Instanz. Nur `docker-compose.yml` verwenden — kein Dev-Compose.

1. Repos auf dem Server klonen (gleiche Ordnerstruktur wie oben)
2. `.env` in `jcdb/` mit den Produktionswerten befüllen (`DB_HOST` auf den externen DB-Host setzen)
3. Starten:

```bash
docker compose -f docker-compose.yml up --build -d
```

Stoppen:

```bash
docker compose -f docker-compose.yml down
```

---

## Umgebungsvariablen (jcdb/.env)

Vorlage: [`jcdb/.env.example`](https://github.com/cxa10/jcdb/blob/main/.env.example)

| Variable | Beschreibung |
|---|---|
| `PORT` | Backend-Port (default: 3001) |
| `JWT_SECRET` | Mindestens 32 Zeichen |
| `DB_HOST` | Lokal: `db` (Docker), Prod: externer Host |
| `DB_PORT` | Default: 3306 |
| `DB_USER` | Datenbankbenutzer |
| `DB_PASSWORD` | Datenbankpasswort |
| `DB_NAME` | Datenbankname |
| `NODE_ENV` | `development` oder `production` |
