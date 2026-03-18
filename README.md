# wda-builder

A GitHub Actions workflow that builds [Appium WebDriverAgent (WDA)](https://github.com/appium/WebDriverAgent) on a macOS runner and produces a downloadable `.ipa` artifact. This lets you build WDA for a real iOS device (iOS 17+/18) from Windows **without needing a Mac or Xcode locally**.

---

## What this repo does

- Clones the official Appium WebDriverAgent repository
- Builds it with Xcode on a GitHub-hosted macOS runner
- Strips the embedded XCTest frameworks required for iOS 17+/18 preinstalled-style installation
- Packages the result as `WebDriverAgent.ipa`
- Uploads both the `.ipa` and the raw `.app` directory as downloadable GitHub Actions artifacts

---

## How to trigger the build

1. Go to the **Actions** tab of this repository.
2. Select **Build WDA** in the left sidebar.
3. Click **Run workflow**.
4. Optionally enter a WDA version tag (e.g. `v9.11.0`). Leave blank to use the latest release.
5. Click the green **Run workflow** button.

The build takes approximately 10–20 minutes. When it finishes, scroll down to the **Artifacts** section of the completed run to download the files.

---

## How to download the artifact

1. Open the completed workflow run (green checkmark in the **Actions** tab).
2. Scroll to the **Artifacts** section at the bottom of the page.
3. Download **WebDriverAgent.ipa** (or **WebDriverAgentRunner-Runner.app** for the raw bundle).

---

## How to sign and install WDA on a real device

### Option A – Sideloadly (Windows, recommended)

1. Install [Sideloadly](https://sideloadly.io/) on Windows.
2. Connect your iPhone via USB and trust the computer.
3. Open Sideloadly, drag `WebDriverAgent.ipa` into the window.
4. Enter your Apple ID when prompted (a free account works for 7-day certificates).
5. Click **Start** and wait for installation to finish.

### Option B – AltStore / AltServer (Windows)

1. Install [AltServer](https://altstore.io/) on Windows and [AltStore](https://altstore.io/) on your iPhone.
2. In AltStore on the device, go to **My Apps → +** and select `WebDriverAgent.ipa`.
3. AltStore will sign and install using your Apple ID.

### Option C – pymobiledevice3 (direct .app install, developer mode required)

> Requires the device to be in **Developer Mode** (Settings → Privacy & Security → Developer Mode).

```bash
pip install pymobiledevice3
pymobiledevice3 apps install WebDriverAgentRunner-Runner.app
```

---

## How to launch WDA after installation

After the app is installed, start WDA using pymobiledevice3:

```bash
pymobiledevice3 developer dvt xcuitest run io.appium.WebDriverAgentRunner.xctrunner
```

WDA will listen on `http://<device-ip>:8100` by default. You can verify it is running by opening that URL in a browser or with:

```bash
curl http://<device-ip>:8100/status
```

---

## Notes

- Tested target: iOS 17+ / iOS 18 (arm64 devices).
- UDID example used during development: `00008140-0009754A34F9801C` (iPhone, iOS 18.6.2).
- The XCTest framework stripping step (`rm -rf Frameworks/XC*.framework` etc.) is required so that WDA can be preinstalled without the Xcode host app on the device.
- For Appium-based automation, point your Appium config to `wdaLocalPort: 8100` and set the `udid` capability to your device UDID.
