# OpenCode Legacy: OpenCode Desktop for Windows 7 / 8 / 8.1

An **unofficial community build** of the [OpenCode](https://github.com/anomalyco/opencode) desktop app that runs on 64-bit Windows 7, 8 and 8.1, with lighter settings for low-end PCs.

> This project is not affiliated with or endorsed by the OpenCode project. For the official app on current systems, use the original repository.

## Download

Get the latest portable zip from the **[Releases](../../releases)** page. Nothing is installed: you unzip a folder and run it.

## Quick start

1. Download the zip from Releases and unzip it to a folder.
2. You need a **Node.js x64 that runs on your Windows**. Version 22.15 or newer is required; 22.19.0 is what this has been tested with.
3. Open the unzipped folder, go to `resources`, and create a folder named `win7`.
4. Copy your `node.exe` into it. The final path must be:

   ```
   resources\win7\node.exe
   ```

   The `server` folder is already inside `resources\win7`.
5. Run `OpenCode Dev.exe`.

If the app shows an error about `opencode-cli.exe`, the `win7` folder or `node.exe` is in the wrong place.

## What is different from the official app

| Part | Official OpenCode | This build |
| --- | --- | --- |
| Electron | Stock Electron 44 (Windows 10 and up) | Community [electron-legacy](https://github.com/Alex313031/electron-legacy) 44.2.0, which supports legacy Windows |
| Background server | Bundled `opencode-cli.exe` (Windows 10 and up) | The same server code, run by your own `node.exe` |
| Chromium settings | Defaults | Lighter settings on Windows 7/8/8.1: fewer renderer processes, no smooth scrolling, low-end memory mode |

The changes are small and live in one patch file, [`opencode-win7.patch`](opencode-win7.patch), plus a build workflow, [`.github/workflows/win7-desktop.yml`](.github/workflows/win7-desktop.yml). They are applied to OpenCode commit `375f654e6dc62a3224ca3663fa335d8b57584797`.

## Known limitations

- **Terminal pane:** the built-in terminal needs Windows 10 and may not work.
- **Updates:** do not accept app updates. They could replace this build with the stock one.
- **Security:** the legacy Chromium engine no longer receives security fixes from Google. Do not open untrusted websites inside the app.
- **Testing:** lightly tested on Windows 8 x64. Reports from Windows 7 and 8.1 are welcome.
- **Not supported:** 32-bit Windows.

## Troubleshooting

- **Blank or flickering window:** start the app with hardware acceleration off. In a Command Prompt opened in the app folder:

  ```
  set OPENCODE_DISABLE_GPU=1
  "OpenCode Dev.exe"
  ```

- **Low-end tuning:** it turns on by default on Windows 7/8/8.1. Set `OPENCODE_LOWEND=1` to force it on, or `OPENCODE_LOWEND=0` to turn it off.
- **Error mentioning `opencode-cli.exe`:** check `resources\win7\node.exe` and `resources\win7\server\opencode.mjs`.
- **Node problems:** run `node --version` in a Command Prompt. It must print 22.15 or newer.

When reporting a bug, please include your Windows version, `node --version`, what you did, and the error text or a screenshot.

## Build it yourself

You do not need Bun or a Windows 10 PC. GitHub builds it for you:

1. Fork this repository.
2. Open **Actions → win7-desktop → Run workflow**.
3. Leave `server_only` unticked for a full build, or tick it for a quick server-only test.
4. When the run turns green, download the **opencode-win7-portable** artifact from the bottom of the run page.

The workflow downloads the legacy Electron zip, builds the server bundle, applies the patch, and packages the app. Its inputs let you point at a different electron-legacy release and version.

## Credits and licenses

- [OpenCode](https://github.com/anomalyco/opencode), MIT license.
- [Electron](https://www.electronjs.org/), MIT license.
- [electron-legacy](https://github.com/Alex313031/electron-legacy) by Alex313031, BSD-3-Clause license, which makes current Electron run on legacy Windows.
- Chromium and its dependencies, under their respective licenses (included in the release zip).

The patch and workflow in this repository are released under the same MIT license as OpenCode. See [LICENSE](LICENSE).
