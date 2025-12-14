# Osho Discourses Downloader

A minimal web app to browse and download preloaded Osho discourse audio links (English & Hindi). Frontend is static HTML/JS; backend is a tiny Express proxy that streams files without storing them.

## Features
- Language toggle (English / Hindi) with preloaded lists from `links.js`.
- Search filter and per-item download button; downloads can be routed through the Express proxy.
- Share button uses Web Share API with clipboard/prompt fallbacks.
- Proxy `/download` endpoint streams remote audio directly to the client (no server storage).

## Project structure
- `index.html` – main page layout and controls.
- `styles.css` – styling for layout, list, and download buttons.
- `script.js` – client logic: rendering, search, per-item downloads, share handling.
- `links.js` – preloaded audio link lists.
- `index.js` – Express server serving static assets and the `/download` proxy.
- `package.json` – Node entrypoint and dependencies.

## Prerequisites
- Node.js 18+ (uses built-in fetch).

## Local setup
```bash
npm install
npm start
```
Then open http://localhost:3000.

## Configuring the proxy domain (optional)
Set your deployed domain in `script.js`:
```js
const RAILWAY_DOMAIN = "https://your-app.up.railway.app";
```
Leaving it blank uses direct links; setting it routes downloads through `/download?url=...` on your server.

## Deploying to Railway
1) Push this repo to GitHub. 2) Create a new Railway project from the repo. 3) Railway will run `npm install` and `npm start`; the app listens on `PORT` automatically. 4) Set `RAILWAY_DOMAIN` in `script.js` to your Railway URL and redeploy.

## `/download` endpoint behavior
- Validates that `url` is http/https.
- Streams the remote response to the client; no disk writes.
- Derives filename from `Content-Disposition` when available, otherwise from the URL path.
- Applies a 15s upstream timeout.

## Customizing links
Edit `links.js` to add/remove audio URLs. Each line becomes one downloadable item.

## Notes
- No files are persisted on the server; the proxy is pass-through.
- If Web Share is unsupported on desktop, the Share button falls back to copy/prompt.

## License
MIT
