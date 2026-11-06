# 3sat-dl

*[Deutsch](README.md) | English*

Download shows and films from the **3sat Mediathek** from the command line —
**always maximum quality**, output is an **MKV** (lossless remux) with
embedded subtitles, metadata and cover art.

## Windows EXE (no installation)

Grab the latest `3sat-windows.exe` from the
[Releases](https://github.com/FLOKHOME/3sat-dl/releases) page and run it —
**yt-dlp and ffmpeg are bundled** (no Python, no pip, no ffmpeg needed).

> Note: The EXE is unsigned — Windows SmartScreen will warn on first start
> ("Unknown publisher"). Click "More info → Run anyway". A `3sat-linux`
> binary is built as well; macOS users can run the tool from source.

## Commands

### Search
```bash
3sat-dl search "sterben"
```
Shows title, category and remaining availability (e.g. "Noch 15 Stunden").

### Show details
```bash
3sat-dl info https://www.3sat.de/film/spielfilm/sterben-104.html
```

### Download
```bash
3sat-dl dl https://www.3sat.de/film/spielfilm/sterben-104.html
3sat-dl dl <url> --out ~/Videos/3sat

# Quality check: only the first 60 seconds
3sat-dl dl <url> --test

# Save and embed all subtitles
3sat-dl dl <url> --subs all

# Without subtitles
3sat-dl dl <url> --subs none

# Lower resolution
3sat-dl dl <url> --max-height 720
```

## Options

| Option | Meaning |
|---|---|
| `--out DIR` | target folder (default: `~/Videos/3sat`) |
| `--max-height N` | max. resolution (default 1080) |
| `--subs auto\|all\|none` | subtitles (default: de/deu embedded automatically) |
| `--test` | download only the first 60 seconds |

## How it works

1. **Search**: the search page `3sat.de/suche?q=…` is server-side rendered —
   teaser cards (title, category, availability) are parsed from it.
2. **Quality**: yt-dlp reads the 3sat mediathek API (HLS up to
   1080p/5.35 Mbit plus progressive MP4 up to 1080p/6.6 Mbit). The format
   sort `res,tbr` overrides yt-dlp's codec preference — otherwise it picks
   VP9 with *less* bitrate. The highest-bitrate MP4 wins.
3. **Audio**: "TV Ton" is preferred over audio description
   (format filter `ba[format_id*=TV_Ton]`, fallback to best track).
4. **MKV**: lossless remux; German subtitles are found automatically,
   converted to SRT and embedded; metadata and cover art included.
5. 3sat mediathek content expires (search shows remaining time).

## Requirements (script usage only)

- Python 3 (no pip needed) — the EXE needs none of this
- **ffmpeg** on PATH (remux to MKV)
- yt-dlp is auto-fetched from PyPI

## Disclaimer

Only use for content 3sat offers legally and for free. Private use only —
do not redistribute downloaded shows.
