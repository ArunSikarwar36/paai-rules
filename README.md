# PAAI rules

Ad-blocking rules that PAAI AdBlocker and PAAI Browser download by themselves,
so a new ad fix reaches every install without reinstalling the apps.

**Android app:** download the latest APK from
[Releases](https://github.com/ArunSikarwar36/paai-rules/releases/latest).

The files are **data only**. The apps check their format when they load them,
ignore anything invalid, and add them to the rules built into the apps. They
never replace or remove the built-in rules.

| File | Used by | Refreshed |
|---|---|---|
| `domains.txt` | DNS blocking on Windows and Android | every 24 h |
| `rules.json` | PAAI Browser on Windows and Android | every 6 h |

## domains.txt

One domain per line. Subdomains are blocked too, so `ads.example.com` also
blocks `v1.ads.example.com`. Hosts-file lines (`0.0.0.0 ads.example.com`) and
AdBlock lines (`||ads.example.com^`) work as well.

Only add servers that deliver nothing but ads or tracking. A server that also
delivers the content (videos, songs, images) breaks the app when blocked.

## rules.json

```json
{
  "version": 1,
  "blockUrls": ["^https://[^/]*\\.example\\.com/ads/"],
  "silenceAudioUrls": ["^https://audio-ads\\.example\\.net/[^?]*\\.mp3(\\?|$)"],
  "youtube": {
    "adKeys": ["newAdField"],
    "hideSelectors": ["ytd-new-ad-renderer"]
  }
}
```

- **blockUrls**: regular expressions matched against the full request URL
  (case-insensitive). Matching requests are blocked in PAAI Browser. Use this
  when the ad server also delivers content, so only the ad path can be blocked.
- **silenceAudioUrls**: regular expressions for audio ads that have to play to
  the end (Amazon Music). PAAI Browser answers them with a short silent MP3.
- **silenceMediaUrls**: like silenceAudioUrls, but answered with a real
  1-second silent media file, for players that stall on the short MP3
  (Spotify's phone site). Android app 0.3.0 and later.
- **youtube.adKeys**: property names removed from YouTube's player data
  (letters, digits and `_` only).
- **youtube.hideSelectors**: CSS selectors of YouTube page elements to hide.

Patterns must work in both JavaScript and Java/Kotlin: keep to plain syntax
(`^ $ . * + ? [] () | \d \.`, no lookbehind or named groups). Limits: 200
entries per list, 300 characters per entry.
