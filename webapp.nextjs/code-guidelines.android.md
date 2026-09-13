# Android Wrapper & Publishing

Convert PWA to Android app for Google Play Store using Bubblewrap.

## Prerequisites

- PWA deployed to production URL with valid manifest
- [[webapp.nextjs/icons|Icons]] are all correct
- `npm install -g @bubblewrap/cli`

All the shell commands in this guide assume running from `android/`.

```bash
cd android
```

## Initialize Project

```bash
bubblewrap init --manifest https://YOUR_DOMAIN/manifest.webmanifest
```

Follow the wizard:
- **Domain**: Must match PWA domain
- **Application ID**: e.g., `com.yourcompany.appname`
- **Signing Key**: Generate new or use existing (save securely!)
	- Passwords for Key Store and for the Key should be the same. Otherwise Bubblewrap fails.

## Digital Asset Links

Get SHA-256 fingerprint from `bubblewrap init` output or from this command:

```bash
keytool -list -v -keystore android.keystore -alias android
```

Create `public/.well-known/assetlinks.json`:

```json
[{
  "relation": ["delegate_permission/common.handle_all_urls"],
  "target": {
    "namespace": "android_app",
    "package_name": "com.yourcompany.appname",
    "sha256_cert_fingerprints":
    ["YOUR_SHA_256_FINGERPRINT"]
  }
}, {
  "relation": ["delegate_permission/common.get_login_creds"],
  "target": {
    "namespace": "web",
    "site": "https://YOUR_DOMAIN"
  }
}, {
  "relation": ["delegate_permission/common.get_login_creds"],
  "target": {
    "namespace": "android_app",
    "package_name": "com.yourcompany.appname",
    "sha256_cert_fingerprints":
    ["YOUR_SHA_256_FINGERPRINT"]
  }
}]
```

Deploy before testing TWA.

## Build & Test

```bash
bubblewrap build
bubblewrap install  # with device connected via ADB
```

If browser bar is not gone → Asset Links is not deployed properly.

## Versioning

In `twa-manifest.json`:
- **appVersionName**: Semantic version (e.g., `1.0`, `1.1`)
- **appVersionCode**: Integer, must increment for each Play Store upload

Bubblewrap updates these on every build if `twa-manifest.json` has changed. If it has not changed - do it manually. TODO: Add to the deployment script (e.g. find the last release tag before building an Android app or download the deployed version.txt.

## Play Store Publishing

1. Go to **Google Play Console**
2. Create new app
3. Upload `.aab` file to Production or Internal Testing track
4. Complete store listing, content rating, etc.
