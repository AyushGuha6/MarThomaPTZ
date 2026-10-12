# PTZ Camera Web Controller

> A web interface for operating a pan–tilt–zoom (PTZ) camera, built for the Mar Thoma Church of San Francisco Bay Area.

## Overview

The church needed a way to control a PTZ camera from a browser. This app serves a web interface whose controls send pan, tilt, and zoom commands through a Node.js/Express server to the camera.

## My contribution

I built this as a software internship project (Feb–Aug 2022), including the server and the browser control interface.

## Tools

Node.js, Express, EJS, SVG.

## Project structure

```
server.js     Express server
public/       static assets for the browser interface
views/        page templates
config/       configuration
package.json
```

## Run locally

```bash
git clone https://github.com/AyushGuha6/MarThomaPTZ.git
cd MarThomaPTZ
npm install
npm start
```

## Limitations

- Requires a compatible PTZ camera to be fully functional.
- Camera addresses and credentials belong in local configuration and should never be committed to the repository.

## Acknowledgments

Built for the Mar Thoma Church of San Francisco Bay Area.
