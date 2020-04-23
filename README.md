# voice-techbridgers

A WebRTC meeting service using [mediasoup](https://mediasoup.org).


## Features
* Audio/Video
* Chat
* Screen sharing
* File sharing
* Different layouts
* Internationalization support

## Installation
* Prerequisites:
Currently multiparty-meeting will only run on nodejs v10.*
To install see here [here](https://github.com/nodesource/distributions/blob/master/README.md#debinstall).

```bash
$ sudo apt install npm build-essential redis
```

* Clone the project:

```bash
$ git clone https://github.com/virendrasingh281/voice-techbridgers.git
$ cd voice-techbridgers
```

* Copy `server/config/config.example.js` to `server/config/config.js` :

```bash
$ cp server/config/config.example.js server/config/config.js
```

* Copy `app/public/config/config.example.js` to `app/public/config/config.js` :

```bash
$ cp app/public/config/config.example.js app/public/config/config.js
```

* Edit your two `config.js` with appropriate settings (listening IP/port, logging options, **valid** TLS certificate, don't forget ip setting in last section in server config: (webRtcTransport), etc).

* Set up the browser app:

```bash
$ cd app
$ npm install
$ npm run build
```
This will build the client application and copy everythink to `server/public` from where the server can host client code to browser requests.

* Set up the server:

```bash
$ cd ..
$ cd server
$ npm install
```

## Run it locally

* Run the Node.js server application in a terminal:

```bash
$ cd server
$ npm start
```
* Note: Do not run the server as root. If you need to use port 80/443 make a iptables-mapping for that or use systemd configuration for that (see further down this doc).
* Test your service in a webRTC enabled browser: `https://yourDomainOrIPAdress:3443/roomname`

## Deploy it in a server

* Stop your locally running server. Copy systemd-service file `multiparty-meeting.service` to `/etc/systemd/system/` and check location path settings:
```bash
$ cp voice-techbridgers.service /etc/systemd/system/
$ edit /etc/systemd/system/voice-techbridgers.service
```

* Reload systemd configuration and start service:

```bash
$ systemctl daemon-reload
$ systemctl start voice-techbridgers
```

* If you want to start voice-techbridgers at boot time:
```bash
$ systemctl enable voice-techbridgers
```

## Ports and firewall

* 3443/tcp (default https webserver and signaling - adjustable in `server/config.js`)
* 4443/tcp (default `npm start` port for developing with live browser reload, not needed in production environments - adjustable in app/package.json)
* 40000-49999/udp/tcp (media ports - adjustable in `server/config.js`)

## Load balanced installation
To deploy this as a load balanced cluster, have a look at [HAproxy](HAproxy.md).

## Learning management integration
To integrate with an LMS (e.g. Moodle), have a look at [LTI](LTI/LTI.md).

## TURN configuration

* You need an additional [TURN](https://github.com/coturn/coturn)-server for clients located behind restrictive firewalls! Add your server and credentials to `app/public/config/config.js`
