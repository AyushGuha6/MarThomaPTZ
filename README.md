# MarThomaPTZ

Web-based control panel for a PTZ (pan-tilt-zoom) IP camera. Click a zone on a map-style interface to move the camera to a preset shot. Built for live-streaming a church service.

## What it does

- Serves a zone-based control interface (Express + EJS)
- Reads camera position presets from a config file
- Sends PTZ move commands to an IP-based camera when a zone is clicked (`GET /zoneClicked/:key`)
- CORS-enabled for cross-origin requests

## Running it

```bash
npm install
npm run devStart
```

Server runs at **http://localhost:3000**.

## Built with

Node.js, Express, EJS, Axios, Nodemon
