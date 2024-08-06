# IWA Bundling Example

A barebones example for bundling an [Isolated Web Application](https://github.com/WICG/isolated-web-apps)

## Prep and Bundle

```console
$ git clone https://github.com/michaelwasserman/iwa-bundling-example.git
$ cd iwa-bundling-example
$ openssl genpkey -algorithm Ed25519 -out ed25519key.pem
$ npm i
$ npm init
$ npm run build
```

This creates `iwa-bundling-example.swbn`. Download a pre-built copy <a href="https://raw.githubusercontent.com/michaelwasserman/iwa-bundling-example/main/iwa-bundling-example.swbn">here</a>.
Note: Keep the new `ed25519key.pem` private key file secure; do not share it in a public repo :)

## Run

Run Chrome M124+ and enable flags:
* chrome://flags/#enable-isolated-web-apps
* chrome://flags/#enable-isolated-web-app-dev-mode

```console
$ chrome --enable-features=IsolatedWebApps,IsolatedWebAppDevMode
```

Visit chrome://web-app-internals/ and point "Install IWA from Signed Web Bundle" to iwa-bundling-example.swbn

Visit chrome://apps and launch "IWA Bundling Example"

Note: If [reinstall fails](https://issues.chromium.org/issues/40286084), try restarting Chrome.

## Alternative: Self-Host a Dev Mode Proxy

```console
$ git clone https://github.com/michaelwasserman/iwa-bundling-example.git
$ cd iwa-bundling-example/static
$ python3 -m http.server [port]
```

## Alternative method using Web Cryptography API, `node`, `deno`, or `bun` (from `webbundle` linked below)

`package.json`
```
{
  "name": "webbundle-webcrypto",
  "version": "0.1.0",
  "description": "Generate Signed Web Bundle with Web Cryptography API",
  "license": "WTFPLv2",
  "repository": {
    "type": "git",
    "url": "https://github.com/guest271314/webbundle"
  },
  "dependencies": {
    "base32-encode": "^2.0.0",
    "mime": "^2.6.0",
    "wbn": "^0.0.9",
    "wbn-sign-webcrypto": "https://github.com/guest271314/wbn-sign-webcrypto.git",
    "zod": "^3.22.4"
  }
}
```

### Install dependencies

```
bun install
```
 or 

```
npm install
```

or dynamically fetch dependencies without creating a `node_modules` folder and create the `.swbn` file and IWA

```
deno run -A --unstable-byonm --import-map=import-map.json index.js

```

### Create Ed25519 algorithm keys using Web Cryptography API in `node`, `deno`, and `bun`.

`generateWebCryptoKeys.js`
```
import { writeFileSync } from "node:fs";
import { webcrypto } from "node:crypto";
const algorithm = { name: "Ed25519" };
const encoder = new TextEncoder();
const cryptoKey = await webcrypto.subtle.generateKey(
  algorithm,
  true, /* extractable */
  ["sign", "verify"],
);
const privateKey = JSON.stringify(
  await webcrypto.subtle.exportKey("jwk", cryptoKey.privateKey),
);
writeFileSync("./privateKey.json", encoder.encode(privateKey));
const publicKey = JSON.stringify(
  await webcrypto.subtle.exportKey("jwk", cryptoKey.publicKey),
);
writeFileSync("./publicKey.json", encoder.encode(publicKey));
```

### Build the Signed Web Bundle and Isolated Web App
Write `signed.swbn` to current directory

Node.js
```
node --experimental-default-type=module index.js
```
Bun
```
bun run index.js
```
Deno
```
deno run --unstable-byonm -A index.js
```
Visit chrome://web-app-internals/ and point "Install IWA via Dev Mode Proxy" to http://localhost:[port]/

## Docs and resources:

* [Isolated Web Apps Explainer](https://github.com/WICG/isolated-web-apps)
* [NPM webbundle-webpack-plugin for Isolated Web App (Signed Web Bundle)](https://www.npmjs.com/package/webbundle-webpack-plugin#isolated-web-app-signed-web-bundle)
* [GoogleChromeLabs webbundle-webpack-plugin for Isolated Web App (Signed Web Bundle)](https://github.com/GoogleChromeLabs/webbundle-plugins/tree/main/packages/webbundle-webpack-plugin#isolated-web-app-signed-web-bundle)
* [wbn-sign-webcrypto](https://github.com/guest271314/wbn-sign-webcrypto)
* [webbundle](https://github.com/guest271314/webbundle)
* [Isolated Web App Utilities](https://github.com/guest271314/isolated-web-app-utilities)

## Other IWA examples:

* https://github.com/sonkkeli/borderless
* https://github.com/GoogleChromeLabs/telnet-client
* https://github.com/michaelwasserman/iwa-windowing-example
* https://coralfish-dev-access.glitch.me/
* https://github.com/guest271314/telnet-client (fork of Google Chrome Labs `telnet-client`)
* https://github.com/guest271314/direct-sockets-http-ws-server

## IWA APIs

* https://github.com/WICG/manifest-incubations/blob/gh-pages/borderless-explainer.md
* https://github.com/WICG/webusb/blob/main/unrestricted-usb-explainer.md
* https://github.com/WICG/controlled-frame/blob/main/README.md
* https://github.com/WICG/direct-sockets/blob/main/docs/explainer.md
* https://github.com/WICG/web-smart-card/
