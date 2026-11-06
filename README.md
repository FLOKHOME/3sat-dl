# 3sat-dl

*Deutsch | [English](README.en.md)*

Sendungen und Filme aus der **3sat-Mediathek** per Kommandozeile
herunterladen — **immer maximale Qualität**, Ergebnis ist eine **MKV**
(verlustfreies Remux) mit eingebetteten Untertiteln, Metadaten und Cover.

## Windows-EXE (ohne Installation)

Von [Releases](https://github.com/FLOKHOME/3sat-dl/releases) die aktuelle
`3sat-windows.exe` laden und direkt starten — **yt-dlp und ffmpeg sind
bereits eingebaut** (kein Python, kein pip, kein ffmpeg nötig).

> Hinweis: Die EXE ist nicht signiert — beim ersten Start meldet Windows
> SmartScreen „Unbekannter Herausgeber". Über „Weitere Informationen →
> Trotzdem ausführen" starten. Zusätzlich wird `3sat-linux` mitgebuildet;
> macOS-Nutzer starten das Tool aus dem Quellcode.

## Kommandos

### Suche
```bash
3sat-dl search "sterben"
```
Zeigt Titel, Rubrik und Verfügbarkeit (z. B. „Noch 15 Stunden").

### Details anzeigen
```bash
3sat-dl info https://www.3sat.de/film/spielfilm/sterben-104.html
```

### Download
```bash
3sat-dl dl https://www.3sat.de/film/spielfilm/sterben-104.html
3sat-dl dl <url> --out ~/Videos/3sat

# Qualitätscheck: nur die ersten 60 Sekunden
3sat-dl dl <url> --test

# Alle Untertitel mitspeichern und einbetten
3sat-dl dl <url> --subs all

# Ohne Untertitel
3sat-dl dl <url> --subs none

# Geringere Auflösung
3sat-dl dl <url> --max-height 720
```

## Optionen

| Option | Bedeutung |
|---|---|
| `--out DIR` | Zielordner (Standard: `~/Videos/3sat`, Windows: `%USERPROFILE%\Videos\3sat`) |
| `--max-height N` | max. Auflösung (Standard 1080) |
| `--subs auto\|all\|none` | Untertitel (Standard: de/deu automatisch einbetten) |
| `--test` | nur die ersten 60 s laden |

## Wie es funktioniert

1. **Suche**: Die Suchseite `3sat.de/suche?q=…` ist serverseitig gerendert —
   Teaser-Karten (Titel, Rubrik, Verfügbarkeit) werden daraus geparst.
2. **Qualität**: yt-dlp liest die 3sat-Mediathek-API (HLS bis 1080p/5,35 Mbit
   plus progressive MP4 bis 1080p/6,6 Mbit). Die Format-Sortierung
   `res,tbr` übersteuert yt-dlps Code-Präferenz — sonst wählt es VP9 mit
   *weniger* Bitrate. Die MP4-Datei mit der höchsten Bitrate gewinnt.
3. **Audio**: „TV Ton" wird der Audiodeskription vorgezogen
   (Format-Filter `ba[format_id*=TV_Ton]`, Fallback auf beste Spur).
4. **MKV**: verlustfreies Remux; deutsche Untertitel werden automatisch
   gefunden, nach SRT konvertiert und eingebettet; Metadaten + Cover rein.
5. 3sat-Mediathek-Inhalte sind zeitlich begrenzt (Suche zeigt Restzeit).

## Voraussetzungen (nur bei Skript-Nutzung)

- Python 3 (kein pip nötig) — die EXE braucht nichts davon
- **ffmpeg** im PATH (Remux nach MKV)
- yt-dlp wird automatisch von PyPI geladen (`~/arte-dl/lib`)

## Haftungshinweis

Nur für Inhalte nutzen, die 3sat legal und kostenlos anbietet.
Private Nutzung — keine Weiterverbreitung der heruntergeladenen Sendungen.
