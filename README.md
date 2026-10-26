# Archive

A local music archive player for Windows. Point it at a folder of songs and that folder becomes a playlist. New songs show up automatically.

**[Download the latest installer](https://github.com/redlair-code/archive/releases/latest)**

## Features

- Folder-based playlists with live updates
- Playback queue, shuffle, next/previous, seekable waveform
- Volume and loudness boost
- Embedded cover art from MP3 / FLAC / M4A tags
- Custom folder covers
- Accent colors, dark/light/auto themes, corner styles, background image or animated GIF, glass and glow settings
- Native Windows media keys and media popup
- Remembers your folders, covers, and settings between launches
- Optional email + password accounts (press **Skip for now** and the app behaves exactly as before)
- A host view listing every account's email, signup date, and last launch
- Automatic updates: new versions are downloaded and offered inside the app

Supported audio: `mp3`, `wav`, `flac`, `m4a`, `ogg`, `aac`, `opus`.

## Install

1. Download `Archive-Setup-x.y.z.exe` from the [latest release](https://github.com/redlair-code/archive/releases/latest).
2. Windows may show "Windows protected your PC" because the installer is not code-signed. Click **More info → Run anyway**.
3. Open **Archive**, click **+ Attach folder**, and pick a music folder.

The app only reads the folders you attach. It never copies, moves, or uploads your music.

## Updates

Archive checks for a newer release when it starts and every few hours. When an update finishes downloading, a pill appears in the top-right corner: **Update vX.Y.Z is ready → Restart**. You can also check manually in **Settings → Updates**.

Anyone still on 1.0.0 needs to install 1.1.0 or newer once by hand; after that, updates arrive inside the app.

## Accounts

Accounts are optional. On first launch you can create one with an email and password, or press **Skip for now** and never see the prompt again. No account is needed to play music, and signing out never touches your folders, covers, or settings.

Accounts are backed by Supabase (row-level security on, passwords hashed by the auth service). Only the email address, signup date, and last launch time are stored. The public key shipped in `supabase.json` is safe to expose; the database policies are what restrict access. The service-role key must never be added to the app.

Setting up from scratch: create a Supabase project, run `supabase-schema.sql` in the SQL Editor, then put the Project URL and publishable key into `supabase.json`. The first account to sign up becomes the host automatically. To promote an existing account by hand:

```sql
update public.profiles set is_admin = true where email = 'you@example.com';
```

## Build from source

Requires Node.js 20+.

```powershell
npm install
npm run dist
```

The installer is written to `dist\Archive-Setup-<version>.exe`.

To publish a new release (requires `gh auth login` or a `GH_TOKEN`):

```powershell
$env:GH_TOKEN = (gh auth token)
npm run release:patch
```

This bumps the patch version, builds the installer, and publishes it to GitHub Releases. Existing installs will see the update.

## Project layout

```
main.js         Electron main process: scanning, file watching, persistence, updates, accounts
preload.js      Safe bridge exposed to the UI as window.archive
src/index.html  The complete player UI
supabase.json   Supabase project URL + public (publishable) key
supabase-schema.sql  Database tables, RLS policies, and functions for accounts
build/icon.ico  App and installer icon
```
