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

## Live TV guide channels

An add-on can also publish live channels and their schedule with a `guide` block. Its channels
join the Live TV guide next to Pluto TV's, under a tab named for the provider, and can be
favorited, marked local, or hidden like any other channel.

```json
{"id": "org.example.live", "name": "Example Live", "version": "1.0.0",
 "guide": {
   "provider": "Example Live",
   "m3u": "https://.../channels.m3u",
   "epg": "https://.../guide.xml.gz",
   "channels": [{
     "id": "news1", "name": "Example News", "number": "101", "category": "News",
     "logo": "https://.../news1.png", "local": false,
     "stream": "https://.../news1/index.m3u8",
     "programs": [{"title": "Evening News", "start": "2026-10-18T18:00:00-05:00",
                   "stop": "2026-10-18T18:30:00-05:00", "about": "...", "rating": "TV-G"}]
   }]
 }}
```

Every field in `guide` is optional; use whichever fits:

| Field | What it does |
|---|---|
| `provider` | Name of the guide tab (defaults to the add-on name). |
| `m3u` | Channels from an M3U playlist; reads `tvg-id`, `tvg-name`, `tvg-logo`, `tvg-chno`, `group-title` (category). |
| `epg` | XMLTV schedule (`.xml` or `.xml.gz`); programmes match channels by `tvg-id` / `id`. |
| `channels` | Channels listed directly; same `id` as an M3U entry overrides it. |
| `stream` | HLS (`.m3u8`, MPEG-TS segments) or MP4, played in WoolleyTV's player. fMP4/CMAF HLS doesn't play on this Fire TV. |
| `open` | Instead of `stream`: an app or web link opened in the app that handles it. |
| `programs` | Inline schedule; `start`/`stop` as ISO-8601 with offset, or epoch milliseconds. |
| `local` | `true` puts the channel in the Local tab and at the top of All. |

A channel with no schedule shows as one long block with its name. Only add channels you have the
right to share.
