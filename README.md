# MarThomaPTZ

MarThomaPTZ is a small web-based control panel for an IP PTZ
(pan-tilt-zoom) camera. The interface displays a church image with clickable
zones. Clicking a zone calls the corresponding camera preset so an operator
can quickly change the camera view during a live stream.

## Features

- Express server with an EJS-rendered control page
- Image-map zones loaded from a JSON configuration file
- PTZ preset control through the camera's HTTP CGI endpoint
- Embedded camera media view
- Static asset serving for the image, stylesheet, and ImageMapster libraries
- CORS headers for browser clients

## Requirements

- Node.js and npm
- A PTZ camera reachable from the machine running this application
- A camera that supports the following endpoints:
  - `/cgi-bin/ptzctrl.cgi?ptzcmd&poscall&<preset>`
  - `/media_view.html`

The application currently uses plain HTTP for camera communication. Make sure
the camera and control panel are on a trusted network.

## Installation and development

```bash
npm install
npm run devStart
```

The development server starts on **http://localhost:3000**. `devStart` uses
Nodemon, so the server restarts when source files change.

There is no separate production script in `package.json`; run the server with
Node directly when Nodemon is not needed:

```bash
node server.js
```

## Configuration

Edit [`config/config.json`](config/config.json) before starting the server:

```json
{
  "camera_ip": "192.168.1.100",
  "image_map_coordinates": {
    "1": "0,0,900,900",
    "2": "3111,1026,3769,1531"
  }
}
```

### Configuration fields

- `camera_ip`: Hostname or IP address of the PTZ camera. Do not include
  `http://` or a trailing path.
- `image_map_coordinates`: Object whose keys identify camera presets and whose
  values are rectangular image-map coordinates in
  `left,top,right,bottom` format. The coordinates are based on
  [`public/church.jpg`](public/church.jpg).

Each zone key is used as the camera preset value. For example, clicking zone
`2` sends a request equivalent to:

```text
http://<camera_ip>/cgi-bin/ptzctrl.cgi?ptzcmd&poscall&2
```

The zone keys and preset identifiers must match the presets configured on the
camera.

## How it works

1. `server.js` reads `config/config.json` when the home page is requested.
2. The server renders `views/index.ejs` with the configured zones.
3. The browser overlays the zones on `public/church.jpg`.
4. Clicking a zone requests `GET /zoneClicked/:key`.
5. The server sends the matching preset command to the camera.
6. The page embeds the camera's `/media_view.html` stream below the map.

## HTTP endpoint

### `GET /zoneClicked/:key`

Sends the requested preset key to the configured camera and returns the
requested route parameters as JSON.

Example:

```bash
curl http://localhost:3000/zoneClicked/2
```

Only use valid, trusted preset keys. This endpoint is not authenticated and
should not be exposed directly to the public internet.

## Project structure

```text
.
├── config/config.json       # Camera address and image-map zones
├── public/church.jpg        # Map image
├── public/style.css         # Page styling
├── public/*.js              # jQuery and ImageMapster browser libraries
├── server.js                # Express server and PTZ route
├── views/index.ejs          # Control-panel page template
├── package.json             # Scripts and dependencies
└── package-lock.json        # Locked dependency versions
```

## Security and deployment notes

- The application has no login or authorization layer.
- CORS is configured with a wildcard origin.
- Camera commands and the embedded media view use HTTP, not HTTPS.
- Keep the application and camera on a private network or place them behind
  an authenticated reverse proxy and HTTPS-enabled network boundary.
- Replace example or site-specific camera addresses before sharing the
  configuration.

## Development status

This repository does not currently include automated tests or a CI workflow.
Validate camera connectivity and each configured zone manually after changing
the configuration.

## Built with

Node.js, Express, EJS, Nodemon, jQuery, and ImageMapster.
