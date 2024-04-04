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

Visit chrome://web-app-internals/ and point "Install IWA via Dev Mode Proxy" to http://localhost:[port]/

## Docs and resources:

* [Isolated Web Apps Explainer](https://github.com/WICG/isolated-web-apps)
* [NPM webbundle-webpack-plugin for Isolated Web App (Signed Web Bundle)](https://www.npmjs.com/package/webbundle-webpack-plugin#isolated-web-app-signed-web-bundle)
* [GoogleChromeLabs webbundle-webpack-plugin for Isolated Web App (Signed Web Bundle)](https://github.com/GoogleChromeLabs/webbundle-plugins/tree/main/packages/webbundle-webpack-plugin#isolated-web-app-signed-web-bundle)

## Other IWA examples:

* https://github.com/sonkkeli/borderless
* https://github.com/GoogleChromeLabs/telnet-client
* https://github.com/michaelwasserman/iwa-windowing-example
* https://coralfish-dev-access.glitch.me/

## IWA APIs

* https://github.com/WICG/manifest-incubations/blob/gh-pages/borderless-explainer.md
* https://github.com/WICG/webusb/blob/main/unrestricted-usb-explainer.md
* https://github.com/WICG/controlled-frame/blob/main/README.md
* https://github.com/WICG/direct-sockets/blob/main/docs/explainer.md
* https://github.com/WICG/web-smart-card/
