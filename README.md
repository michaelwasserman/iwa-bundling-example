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

This creates `dist/signed.swbn`.

Note: Keep the new `ed25519key.pem` private key file secure; do not share it in a public repo :)

## Run

```console
$ chrome --enable-features=IsolatedWebApps,IsolatedWebAppDevMode
```

chrome://web-app-internals/ -> "Install IWA from Signed Web Bundle" -> dist/signed.swbn

Note: If [reinstall fails with a manifest error](crbug.com/1494141), try restarting Chrome.

chrome://apps -> "IWA Bundling Example"

## Docs and resources:

* [Isolated Web Apps Explainer](https://github.com/WICG/isolated-web-apps)
* [NPM webbundle-webpack-plugin for Isolated Web App (Signed Web Bundle)](https://www.npmjs.com/package/webbundle-webpack-plugin#isolated-web-app-signed-web-bundle)
* [GoogleChromeLabs webbundle-webpack-plugin for Isolated Web App (Signed Web Bundle)](https://github.com/GoogleChromeLabs/webbundle-plugins/tree/main/packages/webbundle-webpack-plugin#isolated-web-app-signed-web-bundle)

## Other IWA examples:

* https://github.com/sonkkeli/borderless
* https://github.com/GoogleChromeLabs/telnet-client
* https://coralfish-dev-access.glitch.me/

## IWA APIs

* https://github.com/WICG/manifest-incubations/blob/gh-pages/borderless-explainer.md
* https://github.com/WICG/webusb/blob/main/unrestricted-usb-explainer.md
* https://github.com/WICG/controlled-frame/blob/main/README.md
* https://github.com/WICG/direct-sockets/blob/main/docs/explainer.md
* https://github.com/WICG/web-smart-card/
