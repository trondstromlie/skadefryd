# Skadefryd med Bjarne — Prosjektoversikt

## Hva er dette

En statisk hackathon-landingsside for et internt arrangement i Gjensidige Skade, 30. september 2026 hos Itera (Stortingsgata 6, Oslo). Siden er live på GitHub Pages.

**URL:** https://trondstromlie.github.io/skadefryd/  
**Repo:** git@github.com:trondstromlie/skadefryd.git  
**Stack:** Én enkelt `index.html` — ingen byggsteg, ingen dependencies.

---

## Hvem er Bjarne

Bjarne er en fiktiv AI-agent som er premisset for hackathonet. Eva, Sofie og Frank er Skades offisielle AI-kjendiser — interne AI-persona som faktisk ble valgt ut av Skade. Bjarne ble ikke valgt. Det er hele bakhistorien.

Bjarne er:
- Teknisk kompetent, men foretrekker å drikke kaffe fremfor å jobbe (hovedtrekket — beholdes uendret fra Frank-versjonen: energimåler, kaffekrise, kaffeknapp)
- Vrang og lite samarbeidsvillig — avviser innspill, insisterer på sin egen metode
- Fullstendig blind for at punktet over er *grunnen* til at han ikke ble valgt som AI-kjendis. Han har sin egen teori (politikk, smak, urettferdighet) og nevner den gjerne.
- Sarkastisk, selvsikker, alltid kortfattet
- Svarer alltid på norsk

**Premisset:** Dette hackathonet er ikke offisielt sanksjonert av Gjensidige. Bjarne satte det opp selv, som et eget PR-stunt for å bevise at han fortjener en plass blant kjendisene. Ingen ba ham om det.

Toneleiet på siden er tørt og underdrevet. Unngå forklarende humor — la Bjarne snakke for seg selv. Ironien (at hans egen vrangete er grunnen til snubben) skal vises gjennom hans egne uttalelser, ikke forklares. Bjarne skal ALDRI få en innsikt/oppvåkning om dette — han er overbevist om sin egen rett gjennom hele teksten.

Eva, Sofie og Frank navngis direkte i kopien som referanse til de "utvalgte" — bruk dem gjerne i fremtidige sitater/tekst for å holde sjalusi-twisten synlig.

---

## Hva som er bygget

### Energimåler (fast topplinje)
- Viser Bjarnes energinivå 0–100, tømmes 1 poeng hvert 8. sekund
- Knapp: "☕ Gi Bjarne en kaffe før han sovner" (+25 energi)
- Under 20: rød pulserende advarsel — "Bjarne er kaffekritisk — nærm deg med forsiktighet"
- På 0: Windows BSOD tar over hele skjermen (feilkode: `CAFFEINE_LEVEL_CRITICAL`)
- BSOD har restart-knapp som gir 60 energi
- Energinivå lagres i `localStorage` (`bjarne_energy`, `bjarne_energy_ts`)

### Nedtelling
- Teller ned til 30. september 2026 kl. 12:00
- Når målet er nådd: låst "oppdrag"-seksjon åpnes automatisk og viser Bjarne-promptet

### Bjarne-sitater
- 18 sitater roterer hvert 6. sekund med fade
- Dekker: kaffe (hovedvekt), hackathonet, AI-selvbilde, og — uten å forklare det — hans avvisning av samarbeid og teorien om hvorfor han ikke ble valgt som AI-kjendis (nevner Eva, Sofie, Frank ved navn i flere av sitatene)

### Forberedelser-seksjon
- Tilgang til genai.gjensidige.no (valgfritt, krever `az login`)
- OpenCode (https://opencode.ai/) — anbefalt verktøy
- Kontakt Trond eller Ulrik
- Andre verktøy (VS Code, Cursor, Azure CLI, Node, Python)

### Features-grid
7 fiktive AI-features Bjarne aldri ble bedt om å bygge: Kaffekorrelasjon™, Sukk-detektor, Unngåelsesindeks, Bjarne spår fremtiden, Effektivitetsrapporten, Kaffekritisk varsel, Kjendis-tracker (teller omtaler av Eva/Sofie/Frank mot Bjarnes eget tall).

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

---

## Promptet er automatisk skjult

Det ekte oppdragspromptet ligger base64-encodet i `assets/f.bin` og dekodes kun når datoen 30. september 2026 er passert. Ingen manuell handling kreves.

`_manifest`, `_0x`, `_secret` og `data-token` i koden er bevisste feller — base64-strenger som dekoder til korte, sarkastiske Bjarne-meldinger ("riktig variabel, feil innhold" osv.), ikke det ekte promptet. Utviklere som inspiserer kildekoden og prøver å dekode dem manuelt... vel, de har fortjent svaret de får.
