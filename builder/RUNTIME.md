# SCUMM-lite Runtime – README

## Overzicht
Deze runtime voert een interactieve SCUMM-achtige scène uit, zoals geëxporteerd door de SCUMM-lite Builder. Hij is gebouwd als één HTML-bestand en draait direct op GitHub Pages of elke andere statische HTML-host.

## Functies
- Fullscreen canvas met pixel-art rendering
- Achtergrond en optionele parallaxlagen
- Walkable areas (polygoon) met distance scaling
- Hotspots: Rechthoekig of polygoon; Actie: spraak (typewriter effect) of hyperlink
- Speleranimatie in 4 richtingen, 6 frames per richting (diagonaal via dominante as of hoekberekening)
- Diagonale snelheidsfix (√2)
- MIDI-afspeeloptie via Tone.js/@tonejs/midi of fallback audio (OGG/MP3)
- Bezoeker teller via {{visitors}} placeholder in hotspot-tekst
- Alle assets worden geladen uit scene.json

## Bestandsstructuur
```
/ (root van je repo)
├── index.html        # Deze runtime
├── scenes/
│   └── scene.json    # Geëxporteerd vanuit de builder
└── assets/           # Eventuele losse assets (indien niet in Data-URL)
```

## Installatie & Gebruik
1. Exporteer je scène vanuit de builder naar scenes/scene.json.
2. Zet index.html (deze runtime) in de root van je project.
3. Zet eventuele losse assets (afbeeldingen, audio) in /assets/ of laat ze als Data-URL in de JSON.
4. Publiceer de repository via GitHub Pages.
5. Open de GitHub Pages URL – de runtime laadt scene.json en voert de scène uit.

## Interactie
- Klik op walkable area → speler loopt naar punt.
- Klik op hotspot → voert actie uit (spraak of link).
- Eerste klik start muziek.

## Aanpassen
- SCENE_URL bovenin script → pad naar scene.json
- Standaardresolutie → scene.meta.width & scene.meta.height
- Spelersnelheid, frames, animatie en instellingen → vanuit builder instellen en exporteren.
