# YouTube Client for Windows 10 Mobile (Edge + native app)

A lightweight YouTube client built to run on **Windows 10 Mobile 1703**. It comes in two forms that share the same code:

- **Web app:** a single file (`index.html`) that runs in Microsoft Edge and in any modern browser.
- **Windows app ("Tubo"):** a small UWP app that wraps the web app in a `WebView`, so it installs like any other app, with its own icon, hardware back button support and real full screen.

It uses the **YouTube Data API v3** and the official embedded player.

**Live site:** https://bautiepro8.github.io/Youtube-client-for-W10-Mobile-through-edge-/

## Features

- **Trending** videos by region and **search**, with thumbnail, duration, views and upload date.
- **Pagination** ("More results") for lists, channels and comments.
- **Player** with custom controls: progress bar, skip back or forward 10 or 30 seconds, play/pause.
- **Full screen** mode that hides the custom controls.
- **Subtitles** that can be turned on or off (the video reloads and resumes at the same second).
- **Quality selector** up to 1080p (best effort, see limitations).
- **Comments** sorted by top or newest, with expandable replies.
- **Channel pages** with avatar, subscriber count and all of the channel's videos.
- **Interface languages:** Spanish, English, Portuguese and French.
- **Customizable colors:** 8 accent colors and 4 backgrounds (including pure black for AMOLED screens), applied instantly from Settings.
- The phone's hardware back button returns to the previous screen.

## Using the web app

1. Open the live site in Edge.
2. Tap the gear icon (⚙) and paste your YouTube **API key**. It is stored only on the device.
3. Pick a region, a language and your colors, then tap **Save**.
4. Optional: in the Edge menu, choose **Pin to Start** to keep it like any other app.

## Using the Windows app

The app (`Tubo/`) is a thin UWP shell around the live site. Whenever `index.html` is updated on GitHub Pages, the app shows the new version without being rebuilt or reinstalled. It needs an internet connection.

What the app adds on top of the website:

- Installs with its own icon and tile, and opens without browser chrome.
- The hardware back button moves back through the site (video → list) and closes the app when there is nothing left to go back to.
- Real full screen: hides the status bar and system buttons.
- Keeps the screen on while a video is open.
- Links that leave the site (for example "Open on YouTube") open in the system browser.
- Shows a "Retry" screen when there is no connection.
- The API key you enter in Settings stays inside the app.

### Requirements to build it

- Windows 10 or 11 PC with **Visual Studio** (2019 or 2022) and the **Universal Windows Platform development** workload. Build Tools work too.
- A Windows 10 SDK, version **14393 or newer**. The app targets the newest one installed and supports phones from build 14393 up, which includes 1703 (build 15063).
- Internet access on the first build, to restore NuGet packages.

### Option A: install straight from Visual Studio (easiest)

1. On the phone, enable **Settings → Update & security → For developers → Developer mode**.
2. Connect the phone by USB.
3. Open `Tubo.sln`. If Visual Studio offers to retarget the project, accept.
4. In the toolbar choose **Release**, **ARM** and **Device**, then press **Ctrl+F5**.

### Option B: build an `.appx` with the script

1. Open PowerShell inside the `Tubo` folder.
2. If scripts are blocked, run `Set-ExecutionPolicy -Scope Process Bypass`.
3. Run `.\preparar_y_compilar.ps1`.

The script creates any missing icons, makes a self-signed test certificate (`TuboKey.pfx`, password `1234`), finds MSBuild and the newest SDK, then builds an ARM package. The result is in `Tubo\AppPackages\`: the `.appx`, a `Dependencies` folder and the certificate (`.cer`).

To install the `.appx` on the phone with the Windows SDK deployment tool:

```
WinAppDeployCmd devices
WinAppDeployCmd install -file Tubo_1.0.0.0_ARM.appx -guid <device-guid>
```

Install the packages in `Dependencies\ARM\` first if the tool asks for them. Depending on the phone, you may also need to trust the `.cer` certificate; if you run into certificate errors, use Option A instead.

### Using your own fork

If you host the site somewhere else, change `SiteHost` and `SiteUrl` at the top of `Tubo/MainPage.xaml.cs`, and update the `Publisher` and `PublisherDisplayName` in `Tubo/Package.appxmanifest` together with the certificate subject in the script (`CN=...`). Do not commit `*.pfx` files, `bin/`, `obj/` or `AppPackages/`.

## Getting an API key

1. Go to [Google Cloud Console](https://console.cloud.google.com/) and create a project.
2. Enable **YouTube Data API v3** under *APIs & Services → Library*.
3. Under *Credentials*, create an **API key**.
4. Recommended: restrict it to **Websites (HTTP referrers)** with `https://julioxhernanz99-debug.github.io/*` and limit the API to *YouTube Data API v3*. The key is visible in the browser, so the restriction stops others from using it.

The free quota is 10,000 units per day. A search costs 100 units; the other requests (trending, channels, comments) cost 1.

## Publishing the website (GitHub Pages)

1. Upload `index.html` to the root of the repository.
2. Go to **Settings → Pages**, choose *Deploy from a branch*, branch `main` and folder `/ (root)`.
3. Wait a couple of minutes and open the address GitHub shows.

It must be served over **https**. Opening the file directly (`file://`) breaks the YouTube player. This is also why the app loads the live site instead of bundling the page: the embedded player needs a real https origin.

## Compatibility

The web code is written in **ES5** (no async/await, no CSS grid, no CSS variables) so it runs on EdgeHTML 15, the engine of Edge and of the app's `WebView` on Windows 10 Mobile 1703. To update the web app in Edge, reload the page, or close and reopen the tab if it still shows the old version.

## Limitations

- The YouTube API does not provide video streams, so the embedded player is used. Some videos have embedding disabled; in that case use the "Open on YouTube" link.
- **Quality:** YouTube often ignores external quality requests. To force it, the page enlarges the player internally and scales it down to fit the screen, so YouTube picks that resolution. Reaching 1080p depends on the connection and the device. The quality menu shows the quality that is actually playing.
- No background playback with the screen off: Windows suspends the app and the audio stops.
- The app needs an internet connection; it does not work offline.
- No sign-in, commenting, liking or subscribing (this would require OAuth).
- Subtitles only appear if the video has them, either uploaded or auto-generated.

## Repository structure

```
index.html                    # the whole web app: HTML, CSS and JavaScript
README.md                     # this file
Tubo.sln                      # Visual Studio solution
Tubo/
  Tubo.csproj                 # UWP project
  Package.appxmanifest        # app identity, icons and capabilities
  App.xaml / App.xaml.cs      # app entry point
  MainPage.xaml / .xaml.cs    # WebView shell (back button, full screen, offline screen)
  Properties/Default.rd.xml   # .NET Native directives
  Assets/                     # tile, store and splash icons
  preparar_y_compilar.ps1     # script: icons, test certificate, ARM build
```

## Disclaimer

Personal, unofficial project. Not affiliated with YouTube or Google. Use of the API is subject to the [YouTube API Services Terms of Service](https://developers.google.com/youtube/terms/api-services-terms-of-service).
