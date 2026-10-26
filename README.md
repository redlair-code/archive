# Archive

A local music archive player for Windows. Point it at a folder of songs and that folder becomes a playlist. New songs show up automatically.

**[Download the latest installer](https://github.com/redlair-code/archive/releases/latest)**

## Features

- Folder-based playlists with live updates
- Playback queue, shuffle, next/previous, seekable waveform
- Volume and loudness boost
- Embedded cover art from MP3 / FLAC / M4A tags
- Custom folder covers
- Accent colors, dark/light/auto themes, corner styles, background image, glass and glow settings
- Native Windows media keys and media popup
- Remembers your folders, covers, and settings between launches
- Automatic updates: new versions are offered inside the app

Supported audio: `mp3`, `wav`, `flac`, `m4a`, `ogg`, `aac`, `opus`.

## Install

1. Download `Archive-Setup-x.y.z.exe` from the [latest release](https://github.com/redlair-code/archive/releases/latest).
2. Windows may show "Windows protected your PC" because the installer is not code-signed. Click **More info â†’ Run anyway**.
3. Open **Archive**, click **+ Attach folder**, and pick a music folder.

The app only reads the folders you attach. It never copies, moves, or uploads your music.

## Updates

Archive checks for a newer release when it starts and every few hours. When an update is ready it shows a **Restart** prompt. You can also check manually in **Settings â†’ Updates**.

The first public release with the updater is **1.1.0**. Anyone on 1.0.0 needs to install 1.1.0 once; after that, updates are automatic.

## Build from source

Requires Node.js 20+.

```powershell
npm install
npm run dist
```

The installer is written to `dist\Archive-Setup-<version>.exe`.

To publish a new release (requires `GH_TOKEN` or `gh auth login`):

```powershell
npm run release:patch
```

This bumps the patch version, builds the installer, and publishes it to GitHub Releases. Existing installs will see the update.

## Project layout

```
main.js         Electron main process: scanning, file watching, persistence, updates
preload.js      Safe bridge exposed to the UI as window.archive
src/index.html  The complete player UI
build/icon.ico  App and installer icon
```
