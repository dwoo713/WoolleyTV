# WoolleyTV releases

Update channel for WoolleyTV, a family home screen for a Fire TV. `update.json` points at the newest
release APK; the TV checks it from Settings > Phone & updates.

## Add-ons

WoolleyTV add-ons are JSON packages of media sources (they contain no code). Sources play on the
TV's Watch page.

```json
{"id": "org.example.news", "name": "Local News", "version": "1.0.0",
 "description": "What it is", "icon": "https://.../icon.png",
 "sources": [{"name": "Live", "type": "m3u", "url": "https://..."}]}
```

Source types: `m3u` (M3U/M3U8 channel list), `rss` (video or podcast feed), `folder` (web directory
listing), `link` (one HLS/MP4 stream).

A repository is a JSON file listing add-ons, inline or by `{"id": "...", "manifest": "https://.../addon.json"}`:

```json
{"name": "My Repo", "addons": [ ... ]}
```

Add a repository on the TV (Watch > + Add-ons > + Add repository) or on the phone page (Add-ons tab).
Installed add-ons update automatically once a day when their `version` changes.

`repository.json` here is a starter repository of free, official feeds.
