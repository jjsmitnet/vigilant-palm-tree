# SCUMM‑lite – Builder & Runtime

Een lichte SCUMM‑achtige scene‑engine voor je landingspagina. Alles draait client‑side (HTML/JS/CSS/JSON) en werkt op GitHub Pages.

## Inhoud
- Features
- Mapstructuur
- Builder gebruiken
- Runtime gebruiken
- Scene JSON schema
- GitHub Pages deploy
- MIDI & audio
- Tips & performance
- Troubleshooting
- Licentie & assets

## Features
- Fullscreen SCUMM‑feel (geen inventory/knoppen)
- Walkable areas (polygons) met distance scaling op Y
- 4‑richtingen loopanimatie (6 frames) met diagonale beweging
- Hotspots: rechthoek of poly → speech of link
- Parallax‑lagen met diepte
- MIDI (optioneel) + fallback OGG/MP3 (start op eerste klik)
- Pages‑ready: alles client‑side, export/import scene.json

## Mapstructuur
```
/ (repo root)
  ├─ index.html           # runtime
  ├─ builder.html         # builder
  ├─ /scenes/scene.json   # export uit builder
  └─ /assets/             # je images, sprites, audio (optioneel)
```

## Builder gebruiken
1. Open builder.html (op Pages of lokaal).
2. Background uploaden.
3. Parallax: upload lagen, stel depth in, orden met ↑/↓.
4. Walkable area: Draw walkable polygon → dubbelklik om te sluiten; stel Scale min/max in.
5. Hotspots: Rect hotspot (sleep/resize) of Poly hotspot (dubbelklik sluiten). Stel actie: speech (met {{visitors}}) of link.
6. Sprites: upload 6 losse PNG’s per richting (Up/Down/Left/Right). Builder maakt automatisch een horizontale spritesheet.
7. Audio: optioneel MIDI + fallback (OGG/MP3).
8. Movement: Direction handling (dominant‑as of hoek), Diagonal speed fix (√2).
9. Export → scene.json (Import werkt ook).

## Runtime gebruiken
- Plaats index.html in de root en scenes/scene.json op z'n plek.
- Open index.html → klik om te lopen, klik hotspots voor tekst of link.

Gedrag:
- Richting via dominante as of hoek (4‑dir animatie).
- √2‑correctie voor diagonalen (optioneel).
- Distance scaling lineair binnen walkable poly (Y‑range).
- Parallax volgt subtiel de muis.

## Scene JSON schema (verkort)
```json
{
  "meta": { "engine": "scumm-lite", "version": "0.4", "width": 1280, "height": 720 },
  "background": "/assets/bg/scene.png",
  "parallaxLayers": [
    { "id": "layer_0", "depth": 0.15, "image": "/assets/bg/boats_mid.png" }
  ],
  "player": {
    "sprites": {
      "up": "/assets/sprites/up.png",
      "down": "/assets/sprites/down.png",
      "left": "/assets/sprites/left.png",
      "right": "/assets/sprites/right.png"
    },
    "frameSize": [32, 48],
    "frames": 6,
    "fps": 6,
    "startPos": [640, 480],
    "dirMode": "fourDirNearest",
    "diagSpeedFix": true,
    "flipRightFromLeft": false
  },
  "walkableAreas": [
    { "id": "walk_1", "points": [100,500, 400,500, 320,620], "scaleMin": 0.8, "scaleMax": 1.2 }
  ],
  "hotspots": [
    { "id": "sign", "shape": { "type": "rect", "x": 900, "y": 420, "w": 120, "h": 60 },
      "action": { "type": "speech", "text": "Je bent bezoeker {{visitors}}!" } },
    { "id": "city", "shape": { "type": "poly", "points": [200,500,260,480,320,500] },
      "action": { "type": "link", "url": "/projecten", "openIn": "_self" } }
  ],
  "audio": { "midi": "/assets/music/scene1.mid", "fallback": "/assets/music/scene1.ogg" }
}
```

## GitHub Pages deploy
1. Publieke repo met bovenstaande structuur.
2. index.html en builder.html in de root.
3. scene.json in /scenes/ en assets in /assets/.
4. Activeer Pages (Settings → Pages).
5. Bezoek https://<user>.github.io/<repo>/index.html en /builder.html

## MIDI & audio
- Fallback (aanbevolen): render je MIDI naar OGG/MP3 en koppel als audio.fallback.
- MIDI synth: runtime gebruikt Tone.js + @tonejs/midi als basis.

## Tips & performance
- 1280×720 als startresolutie.
- Comprimeer PNG’s of gebruik WebP; kleine spritesheets.
- Beperk parallax op mobiel.
- Gebruik paden in /assets/ voor caching.

## Troubleshooting
- Zwart scherm → check scenes/scene.json pad (DevTools).
- Geen audio → eerst klikken; gebruik fallback.
- Niet lopen → klik binnen walkable area.
- Scale raar → check scaleMin/Max & poly Y-range.
- Sprites verspringen → controleer frameSize en sheet lay‑out.

## Licentie & assets
Code vrij te gebruiken binnen je project. Assets: gebruik alleen met de juiste rechten.
