# Skadefryd med Frank — Prosjektoversikt

## Hva er dette

En statisk hackathon-landingsside for et internt arrangement i Gjensidige Skade, 30. september 2026 hos Itera (Stortingsgata 6, Oslo). Siden er live på GitHub Pages.

**URL:** https://trondstromlie.github.io/skadefryd/  
**Repo:** git@github.com:trondstromlie/skadefryd.git  
**Stack:** Én enkelt `index.html` — ingen byggsteg, ingen dependencies.

---

## Hvem er Frank

Frank er en fiktiv AI-assistent som er premisset for hackathonet. Han er:
- Teknisk kompetent, men foretrekker å drikke kaffe fremfor å jobbe
- Sarkastisk, litt arrogant, alltid kortfattet
- Svarer alltid på norsk
- Mener kaffen på Gjensidige er dårligere enn på Itera (dokumentert)

Toneleiet på siden er tørt og underdrevet. Unngå forklarende humor — la Frank snakke for seg selv.

---

## Hva som er bygget

### Energimåler (fast topplinje)
- Viser Franks energinivå 0–100, tømmes 1 poeng hvert 8. sekund
- Knapp: "☕ Gi Frank en kaffe før han sovner" (+25 energi)
- Under 20: rød pulserende advarsel — "Frank er kaffekritisk — nærm deg med forsiktighet"
- På 0: Windows BSOD tar over hele skjermen (feilkode: `CAFFEINE_LEVEL_CRITICAL`)
- BSOD har restart-knapp som gir 60 energi
- Energinivå lagres i `localStorage`

### Nedtelling
- Teller ned til 30. september 2026 kl. 12:00
- Når målet er nådd: låst "oppdrag"-seksjon åpnes automatisk og viser Frank-promptet

### Frank-sitater
- 18 sitater roterer hvert 6. sekund med fade
- Dekker: hackathonet, kaffe, latskap, AI-selvbilde, kaffemaskinen på Gjensidige

### Forberedelser-seksjon
- Tilgang til genai.gjensidige.no (valgfritt, krever `az login`)
- OpenCode (https://opencode.ai/) — anbefalt verktøy
- Kontakt Trond eller Ulrik
- Andre verktøy (VS Code, Cursor, Azure CLI, Node, Python)

### Features-grid
6 fiktive AI-features Frank aldri ba om: Kaffekorrelasjon™, Sukk-detektor, Unngåelsesindeks, Frank spår fremtiden, Effektivitetsrapporten, Kaffekritisk varsel.

---

## Viktige designvalg

- **Font:** Bebas Neue (overskrifter), DM Serif Display (sitater/italic), DM Mono (brødtekst/kode)
- **Farger:** `--espresso: #1A0E06`, `--cream: #F5ECD7`, `--amber: #C97B2A`, `--rust: #8B3A1A`
- **Custom cursor** — skjules automatisk på touch-enheter (`@media (pointer: coarse)`)
- **Damppartikler** — animerer oppover fra bunnen, begrenset til 5–95% av bredden for å unngå overflow

---

## Kjente quirks og fixes

- **Horizontal overflow på mobil:** Løst med `overflow-x: clip` på `.page`, `html { overflow-x: hidden }`, og `clip-path: inset(0)` på dampcontaineren. iOS Safari ignorerer `overflow-x: hidden` på `body` alene.
- **Kafferingen:** Skjult på mobil (`display: none`) — `right: -30px` forårsaket dokumentet å bli bredere enn viewport.
- **Energimåler-layout:** Delt i `.energy-left` (spor + tall) og `.energy-right` (knapp) for stabil layout. `min-width: 0` på flex-barn er kritisk.
- **Norsk språk:** All tekst er gjennomgått med Claude Opus. Bruk Opus for fremtidige språklige endringer.

---

## Mobiloppsett (≤640px)

- Energibaren stables vertikalt (spor øverst, knapp under, full bredde)
- Kafferingen skjules
- Info-celler (dato/sted/etc.) vises én per rad med horisontal skillelinje
- Padding redusert gjennomgående

---

## Deployment

Siden deployes automatisk via GitHub Pages fra `main`-branchen, rot-mappen.  
Push til `main` → live innen ~1 minutt.

```bash
git add index.html
git commit -m "beskrivelse"
git push
```
