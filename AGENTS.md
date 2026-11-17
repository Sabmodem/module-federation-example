# Module Federation Example - Agent Instructions

## Quick Start
```bash
docker compose up -d     # Start all services (gateway, host_app, remote_app)
docker compose down      # Cleanup
```
- **Main UI**: http://localhost:80

## Architecture
```
gateway → host_app (serves main UI)
         → remote_app (exposes HelloWorld component via module federation)
```

## Key Facts

**Entry points:**
- `host_app/src/main.js` - Host app entry
- `remote_app/src/main.js` - Remote app entry

**Module federation:**
- Host consumes remote via `/ui/remote_app/assets/remoteEntry.js`
- Remote exposes `./HelloWorld` from `src/components/HelloWorld.vue`
- Both apps share Vue as a singleton

**Build/deploy:**
- Docker uses multi-stage builds: Node.js → build → nginx production
- Each app has its own `nginx.conf` for local testing
- Remote app uses `base: '/ui/remote_app/'` in vite config

**Routing (nginx):**
- Host app served at `/`
- Remote app assets proxied at `/ui/remote_app/*`

**Vite config files:** `*.mjs` (not `.js`)
