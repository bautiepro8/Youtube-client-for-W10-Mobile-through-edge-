# YouTube Client for Windows 10 Mobile (Edge)

A single-file web client for YouTube (`index.html`), built to run in **Microsoft Edge on Windows 10 Mobile 1703** and in any modern browser. It uses the **YouTube Data API v3** and the official embedded player.

**Live site:** https://julioxhernanz99-debug.github.io/Youtube-client-for-W10-Mobile-through-edge-/

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
- The phone's hardware back button returns to the previous screen.

## How to use

1. Open the live site in Edge.
2. Tap the gear icon (⚙) and paste your YouTube **API key**. It is stored only in the device's browser.
3. Pick a region and a language, then tap **Save**.
4. Optional: in the Edge menu, choose **Pin to Start** to keep it like any other app.

## Getting an API key

1. Go to [Google Cloud Console](https://console.cloud.google.com/) and create a project.
2. Enable **YouTube Data API v3** under *APIs & Services → Library*.
3. Under *Credentials*, create an **API key**.
4. Recommended: restrict it to **Websites (HTTP referrers)** with `https://julioxhernanz99-debug.github.io/*` and limit the API to *YouTube Data API v3*. The key is visible in the browser, so the restriction stops others from using it.

The free quota is 10,000 units per day. A search costs 100 units; the other requests (trending, channels, comments) cost 1.

## Publishing it (GitHub Pages)

1. Upload `index.html` to the root of the repository.
2. Go to **Settings → Pages**, choose *Deploy from a branch*, branch `main` and folder `/ (root)`.
3. Wait a couple of minutes and open the address GitHub shows.

It must be served over **https**. Opening the file directly (`file://`) breaks the YouTube player.

## Compatibility

The code is written in **ES5** (no async/await, no CSS grid, no CSS variables) so it runs on EdgeHTML 15, the engine of Edge on Windows 10 Mobile 1703. To update the page on the phone, reload it, or close and reopen the tab if it still shows the old version.

## Limitations

- The YouTube API does not provide video streams, so the embedded player is used. Some videos have embedding disabled; in that case use the "Open on YouTube" link.
- **Quality:** YouTube often ignores external quality requests. To force it, the page enlarges the player internally and scales it down to fit the screen, so YouTube picks that resolution. Reaching 1080p depends on the connection and the device. The quality menu shows the quality that is actually playing.
- No background playback with the screen off.
- No sign-in, commenting, liking or subscribing (this would require OAuth).
- Subtitles only appear if the video has them, either uploaded or auto-generated.

## Structure

```
index.html   # the whole app: HTML, CSS and JavaScript
README.md    # this file
```

## Disclaimer

Personal, unofficial project. Not affiliated with YouTube or Google. Use of the API is subject to the [YouTube API Services Terms of Service](https://developers.google.com/youtube/terms/api-services-terms-of-service).
