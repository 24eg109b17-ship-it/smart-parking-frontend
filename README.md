# Smart Parking — Frontend

React 19 and Vite 8 frontend for the Smart Parking Management System.

[Backend repository](https://github.com/24eg109b17-ship-it/smart-parking-backend)

[Live website](https://smart-parking-campus.onrender.com) · [API health](https://smart-parking-api-production-141e.up.railway.app/api/health)

## Features

- Driver registration/login and restored authenticated sessions.
- Vehicle management, parking areas, and a visual slot reservation grid.
- Reservation cancellation, entry/exit recording, a live session timer, and hourly receipts.
- Admin dashboards, users, vehicles, parking area/slot management, reservations, active sessions, and CSV receipt export.
- Responsive campus-themed interface and static-host-compatible hash routing.

This project extends the supplied Smart Parking starter archive. Sample parking locations are demonstration data; availability comes from API requests, not physical sensors. Receipts record calculated charges; the app does not collect online payments.

## Run locally

Requires Node.js 22.12+ (Node 24 recommended), npm, and the running [backend](https://github.com/24eg109b17-ship-it/smart-parking-backend#run-locally).

```sh
git clone https://github.com/24eg109b17-ship-it/smart-parking-frontend.git
cd smart-parking-frontend
npm ci
npm run dev
```

Open `http://localhost:5173`. Vite forwards `/api` to `http://127.0.0.1:8080`. Set `API_PROXY_TARGET` to use another backend address.

Run the backend with its `demo` profile for local-only sample accounts. Local development exposes shortcuts for the demo driver and admin; production builds hide those shortcuts by default.

## Configuration

| Variable | Purpose |
| --- | --- |
| `VITE_API_BASE_URL` | Build-time backend base URL ending in `/api`; defaults to `/api`. |
| `VITE_DEMO_ENABLED` | Set `true` only to expose demo shortcuts intentionally; default hidden in production. |
| `API_PROXY_TARGET` | Development proxy target; defaults to `http://127.0.0.1:8080`. |

For separate hosting, build with `VITE_API_BASE_URL=https://your-backend-host/api`. Also configure the backend's `CORS_ALLOWED_ORIGINS` to the exact frontend origin. `VITE_` values are included in the public bundle and must not contain secrets.

PowerShell:

```powershell
$env:VITE_API_BASE_URL = "https://your-backend-host/api"
npm run build
```

macOS/Linux:

```sh
VITE_API_BASE_URL=https://your-backend-host/api npm run build
```

## Commands

| Command | Purpose |
| --- | --- |
| `npm run dev` | Start local development on port 5173. |
| `npm run lint` | Run Oxlint. |
| `npm test` | Check billing-display boundary cases and formatting. |
| `npm run build` | Generate `dist/` production assets. |
| `npm run preview` | Preview a production build. |

## Render deployment

Create a static site from this repository with Node 24, build command `npm ci && npm run build`, and publish directory `dist`. Set `VITE_API_BASE_URL` to the Railway API URL plus `/api`, and `VITE_DEMO_ENABLED=false`.

`render.yaml` contains these settings. Hash routing means refresh and direct navigation work without a server rewrite rule.

## Docker

```sh
docker build -t smart-parking-frontend .
```

The bundled Nginx configuration forwards `/api` to `backend:8080`. Attach both containers to a shared Docker network, with the backend named `backend`. Nginx expects that name to resolve at startup.

## Structure

```text
src/pages/        Driver and admin pages
src/components/   Shared UI and admin record tables
src/context/      Authentication state and hooks
src/api/          Bearer-token API client
src/lib/          Formatting, CSV export, tests
src/styles/       Campus UI styles
Dockerfile        Production frontend image
render.yaml       Render static-site configuration
```
