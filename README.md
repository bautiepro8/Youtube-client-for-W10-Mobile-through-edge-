# YouTube Client for Windows 10 Mobile (Edge + native app)

A lightweight YouTube client built to run on **Windows 10 Mobile 1703**. It comes in two forms that share the same code:

- **Web app:** a single file (`index.html`) that runs in Microsoft Edge and in any modern browser.
- **Windows app ("Tubo"):** a small UWP app that wraps the web app in a `WebView`, so it installs like any other app, with its own icon, hardware back button support and real full screen.

It uses the **YouTube Data API v3** and the official embedded player.

**Live site:** https://bautiepro8.github.io/Youtube-client-for-W10-Mobile-through-edge-/

<img width="720" height="1280" alt="wp_ss_20261116_0001" src="https://github.com/user-attachments/assets/76115b4c-c21d-41d5-8ce5-3de4b68c1726" />


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

## Getting an API key

1. Go to [Google Cloud Console](https://console.cloud.google.com/) and create a project.
2. Enable **YouTube Data API v3** under *APIs & Services → Library*.
3. Under *Credentials*, create an **API key**.
4. Recommended: restrict it to **Websites (HTTP referrers)** with `https://bautiepro8.github.io/*` and limit the API to *YouTube Data API v3*. The key is visible in the browser, so the restriction stops others from using it.

The free quota is 10,000 units per day. A search costs 100 units; the other requests (trending, channels, comments) cost 1.

## Compatibility

The web code is written in **ES5** (no async/await, no CSS grid, no CSS variables) so it runs on EdgeHTML 15, the engine of Edge and of the app's `WebView` on Windows 10 Mobile 1703. To update the web app in Edge, reload the page, or close and reopen the tab if it still shows the old version.

## Limitations

- The YouTube API does not provide video streams, so the embedded player is used. Some videos have embedding disabled; in that case use the "Open on YouTube" link.
- **Quality:** YouTube often ignores external quality requests. To force it, the page enlarges the player internally and scales it down to fit the screen, so YouTube picks that resolution. Reaching 1080p depends on the connection and the device. The quality menu shows the quality that is actually playing.
- No background playback with the screen off: Windows suspends the app and the audio stops.
- The app needs an internet connection; it does not work offline.
- No sign-in, commenting, liking or subscribing (this would require OAuth).
- Subtitles only appear if the video has them, either uploaded or auto-generated.

## Credits & Contributors

- **[bautiepro8](https://github.com):** Project creator, concept, testing on Windows 10 Mobile hardware, and repository maintenance.
- **Claude (Anthropic):** Core ES5 web client architecture (`index.html`), UI/UX design, multi-language support, customizable themes/AMOLED dark mode, UWP wrapper logic (`MainPage.xaml` / `MainPage.xaml.cs`), and project packaging.
- **Gemini (Google):** Lightweight build environment setup (VS Build Tools & Windows 10 SDK), `.csproj` configuration for ARM32 compilation, certificate signing, and `.appx` sideload troubleshooting.

## Disclaimer

Personal, unofficial project. Not affiliated with YouTube or Google. Use of the API is subject to the [YouTube API Services Terms of Service](https://developers.google.com/youtube/terms/api-services-terms-of-service).
