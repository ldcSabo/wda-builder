# wda-builder

A GitHub Actions repository that builds [WebDriverAgent](https://github.com/appium/WebDriverAgent) for real iOS devices and uploads the build artifacts for download.

## How to trigger the build

1. Go to the **Actions** tab of this repository.
2. Select **Build WDA** in the left sidebar.
3. Click **Run workflow**.
4. Optionally enter a WDA version tag (e.g. `v9.11.0`). Leave blank to use the latest release.
5. Click the green **Run workflow** button.
6. The build takes approximately 10–20 minutes. When it finishes, scroll down to the **Artifacts** section of the completed run to download the files.