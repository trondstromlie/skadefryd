# Agent Instructions — skadefryd

## Viktig: hvem bruker dette prosjektet

Dette repoet brukes av **ikke-utviklere** — designere, PO-er, folk som bare vil endre en
tekst eller en farge. De åpner OpenCode eller Claude Code og ber om hjelp, uten å kunne
HTML, CSS eller Git. Agenten må derfor:

- **Ta alle tekniske steg selv.** Brukeren skal aldri skrive en git-kommando, aldri løse
  en merge-konflikt, aldri klikke seg fram i GitHub-UI-et.
- **Forklare på vanlig norsk.** Ingen fagord uten forklaring. «Branch» heter «en egen
  arbeidskopi» første gang du sier det.
- **Se at endringen faktisk virker i en nettleser før du sier deg ferdig** — ikke bare
  at koden ser riktig ut. Se [Testing](#testing--obligatorisk-før-du-sier-deg-ferdig).
- **Aldri skremme.** Feilmeldinger i terminalen ser dramatiske ut og er nesten aldri
  farlige. Oversett dem, fiks dem, og si i én setning hva som skjedde.

Og det viktigste å si tidlig til en nervøs bruker:
**ingenting du gjør her kan ødelegge noe.** Alt skjer først på din egen maskin, alt går
gjennom en PR, og alt som noen gang har ligget i repoet kan hentes tilbake.

---

# Innhold

| Del | Handler om |
|-----|-----------|
| [Steg minus én](#steg-minus-én--stå-i-riktig-mappe) | Stå i riktig mappe — den vanligste snublesteinen |
| [Kjøre siden lokalt](#kjøre-siden-lokalt) | Se siden på din egen maskin |
| [Alt er lokalt](#alt-du-gjør-er-lokalt--helt-til-du-pusher) | Ingen ser noe før du pusher — og du kan alltid nullstille |
| [Ordliste](#ordliste--git-på-vanlig-norsk) | Hva ordene faktisk betyr |
| [Steg 0](#steg-0--sjekk-github-før-du-gjør-noe-som-helst) | Hent fra GitHub **før** du gjør noe |
| [PR-oppskriften](#pr-oppskriften--repoets-viktigste-seksjon) | De ni stegene fra ønske til live side |
| [Bagatellene](#bagatellene--alt-utviklere-glemmer-å-fortelle) | Alt «alle vet» som ingen sier høyt |
| [Angrekurset](#angrekurset--jeg-gjorde-noe-feil-hva-nå) | Når noe har gått galt |
| [Hva dette repoet er](#hva-dette-repoet-er) | Prosjektet, filene, publisering |
| [Hvem Bjarne er](#hvem-bjarne-er--les-dette-før-du-endrer-tekst) | Premiss, persona og tone — les før du endrer tekst |
| [Testing](#testing--obligatorisk-før-du-sier-deg-ferdig) | Hvordan se at det virker |
| [Feller i akkurat dette repoet](#feller-i-akkurat-dette-repoet) | Ting som er spesielt her |
| [Kommandokart](#kommandokart--jukselapp) | Jukselapp |

---

# Steg minus én — stå i riktig mappe

Alt annet i dette dokumentet forutsetter at agenten kjører **inne i prosjektmappa**.
Gjør den ikke det, går ingenting som det skal — og feilene blir forvirrende framfor
tydelige. Dette er den vanligste snublesteinen for folk som ikke er utviklere.

## Hva som er problemet

En terminal-agent jobber i mappa den ble startet fra. Starter du den i hjemmemappa
(`/Users/<navn>` eller `C:\Users\<navn>`), leter den etter prosjektfiler som ikke finnes
der, og kan i verste fall lage nye filer på feil sted.

**Åpne derfor alltid agenten direkte i prosjektmappa.** Ikke i hjemmemappa, ikke i
`Dokumenter`, ikke i `Code` — helt inn i `skadefryd`.

## Slik gjør du det

**På Mac** — åpne Terminal og gå inn i mappa før du starter agenten:

```bash
cd ~/Code/skadefryd
```

Har du mappa åpen i Finder, kan du høyreklikke på den og velge
**Tjenester → Ny terminal ved mappe**. (Er valget ikke der: Systeminnstillinger →
Tastatur → Tastatursnarveier → Tjenester → Filer og mapper, huk av for det.)

**På Windows** — åpne mappa i Utforsker, klikk i **adressefeltet** øverst, skriv
`powershell` og trykk Enter. Da åpnes et terminalvindu som allerede står i den mappa.
På Windows 11 kan du også høyreklikke i mappa og velge **Åpne i terminal**.

## Sjekk at du står riktig — før du gjør noe annet

```bash
pwd     # Mac/Linux — virker også i PowerShell
ls      # Mac/Linux · dir i vanlig Windows-ledetekst
```

Du står riktig når **begge** stemmer:

1. Stien slutter på `skadefryd`
2. Du ser `index.html`, `CLAUDE.md`, `AGENTS.md`, `CNAME` og mappa `assets` i lista

> ⚠️ **«Er jeg i et git-repo?» er ikke en gyldig sjekk.** Mange har hjemmemappa si under
> git (dotfiles), og da svarer `git rev-parse --show-toplevel` helt fint — den peker bare
> på feil mappe. Test på **filene**, ikke på om git svarer.

> ⚠️ **Det finnes to mapper med nesten samme navn.** `~/Code/skadefryd` og
> `~/Code/skadefryd-gjensidige` er ikke det samme repoet. Sjekk `git remote -v` —
> denne mappa skal peke på `git@github.com:trondstromlie/skadefryd.git`.

## Første gang — hent ned prosjektet

Finnes mappa ikke ennå:

```bash
mkdir -p ~/Code && cd ~/Code
git clone git@github.com:trondstromlie/skadefryd.git
cd skadefryd
```

Virker ikke SSH-varianten (se [Angrekurset](#permission-denied-publickey)), bruk HTTPS:

```bash
git clone https://github.com/trondstromlie/skadefryd.git
```

Det er **ingen `npm install`, ingen `bun install`, ingen byggsteg**. Repoet er én
HTML-fil. Når klonen er ferdig, er du klar.

## Hva agenten skal gjøre

**Dette er den aller første handlingen, før Steg 0.**

```bash
pwd
ls index.html CLAUDE.md
```

Mangler filene, **stopp**. Ikke lag filer, ikke commit, ikke gjett deg fram:

> «Jeg ser at jeg ikke står i prosjektmappa — jeg står i `<sti>`, og der ligger ikke
> prosjektet. Avslutt meg, kjør `cd ~/Code/skadefryd` i terminalen, og start meg opp
> igjen der. Vet du ikke hvor prosjektet ligger, kan jeg lete etter det for deg.»

Kjenner du ikke stien, let selv i stedet for å be brukeren om den:

```bash
find ~ -maxdepth 4 -type d -name skadefryd 2>/dev/null
```

Disse tegnene betyr nesten alltid feil mappe, og skal aldri feilsøkes som noe annet:

| Symptom | Egentlig årsak |
|---------|----------------|
| Agenten finner ikke `index.html` | Feil mappe |
| `git status` viser helt andre filer enn forventet | Du står i et annet repo (ofte dotfiles i hjemmemappa) |
| `fatal: not a git repository` | Du står utenfor prosjektet |
| Alt ser tomt ut | Feil mappe |

---

# Kjøre siden lokalt

**Dette er det aller første agenten skal tilby**, før den endrer noe som helst: «Vil du se
siden mens vi jobber? Jeg kan åpne den for deg.» Svaret er nesten alltid ja — og en bruker
som ser siden foran seg, forstår hva som skjer.

Det er ingen installasjon, ingen `npm install`, ingen byggsteg. Siden er én HTML-fil.

## Metode 1 — bare åpne fila (raskest)

```bash
open index.html          # macOS
start index.html         # Windows (PowerShell / ledetekst)
xdg-open index.html      # Linux
```

Nettleseren åpner fila rett fra disken. Adressen i adressefeltet begynner med `file://`.
Dette holder for **det aller meste**: tekst, farger, layout, nedtelling, energimåler,
kaffeknapp, sitater.

## Metode 2 — lokal webserver (når du trenger den ekte greia)

Noen ting virker ikke over `file://` fordi nettleseren blokkerer dem av
sikkerhetsgrunner — først og fremst `fetch()` av `assets/oppdrag.bin` (oppgaveteksten), og
`localStorage` oppfører seg annerledes. Trenger du det, start en liten server:

```bash
python3 -m http.server 8000
```

Åpne så **http://localhost:8000** i nettleseren.

- **Port opptatt?** Feilmeldingen sier `Address already in use`. Bruk et annet tall:
  `python3 -m http.server 8001`. Ikke gjett hvilken port som er i bruk — les tallet du
  faktisk startet med, og gi brukeren riktig lenke.
- **Avslutt serveren** med `Ctrl+C` i terminalvinduet når dere er ferdige.
- **Har du ikke Python?** Alternativer: `npx serve .` eller `php -S localhost:8000`.
  På Mac er `python3` alltid installert.
- Kjører du serveren i bakgrunnen, si fra til brukeren at den ligger og går, og steng den
  når økta er over.

## Det den lokale visningen **ikke** gjør

- **Den oppdaterer seg ikke selv.** Det finnes ingen hot reload her. Etter hver endring må
  siden lastes på nytt — og helst med **hard refresh**: `Cmd+Shift+R` (Mac) /
  `Ctrl+F5` (Windows). Vanlig refresh viser ofte den gamle, cachede versjonen, og det er
  den vanligste grunnen til at brukeren sier «det skjedde jo ingenting».
- **Den viser ikke skadefryd.tech.** Lokalt er lokalt. Ingen andre kan se det, og det
  påvirker ikke nettsiden.

## Sjekk mobilvisning mens du er der

Åpne nettleserens utviklerverktøy (`Cmd+Option+I` / `F12`), slå på enhetsvisning
(`Cmd+Shift+M` / `Ctrl+Shift+M`) og sett bredden til **375px**. Det er her feilene i dette
repoet pleier å dukke opp. Se [Testing](#testing--obligatorisk-før-du-sier-deg-ferdig).

---

# Alt du gjør er lokalt — helt til du pusher

**Si dette til brukeren tidlig, uoppfordret.** Det er den enkeltopplysningen som gjør folk
trygge nok til å tørre å prøve noe.

Endringer lever på **tre steder**, i denne rekkefølgen:

| Hvor | Hvem ser det | Hvordan det kommer dit |
|------|--------------|------------------------|
| 1. **Din maskin** | Bare deg | Du redigerer og lagrer en fil |
| 2. **GitHub** | Alle som ser i repoet | `git push` |
| 3. **skadefryd.tech** | Hele verden | PR-en blir **merget** |

Så lenge du står på steg 1, er det **ingen** som ser noe. Ikke kollegaer, ikke besøkende
på skadefryd.tech, ikke Bjarne. Du kan rote så mye du vil.

To ting brukere ofte tror, som ikke stemmer:

- ❌ «Nå som jeg lagret fila, er det live.» → Nei. Det ligger bare på din maskin.
- ❌ «Nå som vi pushet, er det live.» → Nei. Det ligger på GitHub. Det går live når PR-en
  **merges**.

Formuleringen agenten skal bruke, gjerne ordrett:

> «Dette ligger nå bare på din maskin. Ingen andre kan se det, og nettsiden er uendret.
> Når du sier ifra, sender jeg det til GitHub og lager et forslag — og det er først når
> forslaget godkjennes at det faktisk går live.»

## Nødbremsen: kast alt lokalt og hent tilbake versjonen som ligger på nett

**Dette er alltid mulig, og det er alltid trygt for andre.** Uansett hvor rotete det har
blitt lokalt, kan du på sekunder komme tilbake til nøyaktig den versjonen som ligger live
på skadefryd.tech. Ingenting på nett påvirkes.

```bash
git fetch origin              # hent fasiten fra GitHub
git checkout main             # gå til hovedversjonen
git reset --hard origin/main  # gjør de lokale filene identiske med GitHub
git clean -fd                 # fjern filer som ikke hører hjemme i repoet
```

Etter disse fire linjene er mappa di **eksakt** lik det som ligger på `main` — altså det
som er publisert på skadefryd.tech.

**Før du kjører dette, spør alltid brukeren:**

> «Da kaster jeg alt vi har gjort lokalt og henter tilbake versjonen som ligger ute på
> skadefryd.tech. Nettsiden påvirkes ikke i det hele tatt — men det vi har jobbet med her
> forsvinner. Er det greit?»

Fordi:

| Kommando | Hva den sletter |
|----------|-----------------|
| `git reset --hard origin/main` | Alle endringer og commits som ikke er pushet |
| `git clean -fd` | Alle nye filer git ikke følger med på (bilder, notater, testfiler) |

Vil du se hva `git clean` kommer til å fjerne **før** du gjør det:

```bash
git clean -nd                 # -n = bare vis, ikke slett
```

Og vil du beholde arbeidet i tilfelle du ombestemmer deg, ta vare på det først:

```bash
git stash                     # legg alt i skuffen, hent tilbake med `git stash pop`
# eller
git checkout -b sikkerhetskopi && git add -A && git commit -m "Sikkerhetskopi"
```

**Nyttig å vite:** har du allerede committet noe, er selv `reset --hard` reversibelt en
stund — `git reflog` viser alt du har gjort de siste dagene, og du kan hoppe tilbake til
et hvilket som helst punkt. Det eneste som virkelig forsvinner for godt, er endringer som
aldri ble committet.

---

# Ordliste — git på vanlig norsk

Bruk disse forklaringene når du snakker med brukeren. Si ordet **én gang med forklaring**,
deretter kan du bruke det fritt.

| Ordet | Hva det egentlig er |
|-------|--------------------|
| **repo** (repository) | Prosjektmappa, med hele historikken sin |
| **GitHub** | Nettstedet der fasiten ligger. Maskinen din har bare en kopi |
| **origin** | Kallenavnet på GitHub-kopien, sett fra maskinen din |
| **main** | Hovedversjonen. Det som ligger her, er det som er live på nettsiden |
| **branch** | En egen arbeidskopi der du kan rote uten at det påvirker `main` |
| **commit** | Et lagringspunkt med en beskjed om hva du endret. Som «lagre som» med notat |
| **fetch** | «Hva har skjedd på GitHub?» — henter informasjon, endrer ingenting hos deg |
| **pull** | Fetch + hent endringene faktisk inn i filene dine |
| **push** | Send dine lagringspunkter opp til GitHub |
| **PR** (pull request) | Et forslag: «kan disse endringene bli en del av `main`?» |
| **merge** | Å godta forslaget. Nå er endringen en del av `main` — og går live |
| **konflikt** | To personer har endret samme linje. Git tør ikke velge, så noen må si hva som er riktig |
| **HEAD** | «Her står du nå» i historikken |
| **stash** | En midlertidig skuff for endringer du ikke er ferdig med |
| **diff** | Forskjellen mellom to versjoner — hva som faktisk er endret |
| **rebase** | Flytte arbeidet ditt så det står oppå den nyeste `main` |
| **staged** | Filer du har plukket ut til neste commit (`git add`) |
| **untracked** | Filer git ikke følger med på ennå — de blir ikke med i en commit før du sier ifra |

---

# Steg 0 — sjekk GitHub før du gjør noe som helst

Dette er den **første git-handlingen** i enhver sesjon. Før du leser kode, før du svarer
på spørsmål om repoets tilstand, før du foreslår noe.

Grunnen: mappa på maskinen er bare én kopi, og den er ofte **utdatert**. Sannheten ligger
på GitHub. Konkluderer du ut fra den lokale kopien alene, kan du «fikse» ting som allerede
er fikset, gjenskape arbeid som finnes fra før, og lage en PR som ikke lar seg merge.

```bash
git fetch origin                        # hent siste tilstand fra GitHub
git status                              # ulagrede endringer? hvilken branch?
git log HEAD..origin/main --oneline     # hva finnes på GitHub som ikke er lokalt?
git log origin/main..HEAD --oneline     # hva finnes lokalt som ikke er på GitHub?
git branch --show-current               # hvilken branch står vi på?
```

`git fetch` er **ufarlig**. Den endrer ingen filer hos deg — den henter bare informasjon.
Det finnes ingen grunn til å hoppe over den.

Bruk deretter GitHub MCP (`mcp__github__list_pull_requests` med `state: open`) for å se
åpne PR-er. **Ikke bruk `gh` CLI når MCP finnes.**

Slik leser du svaret:

| Situasjon | Hva det betyr | Hva du gjør |
|-----------|---------------|-------------|
| `HEAD..origin/main` er tom | Lokalt er oppdatert | Fortsett |
| `HEAD..origin/main` har commits | GitHub har nyere arbeid | `git checkout main && git pull origin main` **før** du starter |
| `origin/main..HEAD` har commits du ikke kjenner igjen | Noen har committet lokalt uten å pushe | Undersøk før du bygger videre |
| Begge har commits | Historikken har **divergert** — dette gir konflikter | Stopp. Finn ut hva som er unikt lokalt før du gjør noe |
| `git status` viser ulagrede endringer | Noen har jobbet uten å committe | Spør brukeren om de skal være med, eller `git stash` dem |
| Det finnes en åpen PR for branchen | PR-en eksisterer allerede | Oppdater den, ikke lag ny |

## Forbudt uten å ha kjørt Steg 0

Du kan **ikke** si noen av disse tingene før du har sjekket `origin/main`:

- «denne filen mangler»
- «dette er ikke gjort»
- «dette ble aldri committet»
- «vi bør legge til X»

Sjekk alltid først om det allerede finnes på GitHub:

```bash
git ls-tree -r --name-only origin/main | grep <filnavn>   # finnes filen på main?
git show origin/main:index.html | grep -n "<tekst>"       # hva står i den på main?
```

## Hvis du oppdager at du tok feil

Si det rett ut til brukeren, med én gang, på vanlig norsk. Ikke bygg videre på en feil
konklusjon fordi arbeidet allerede er påbegynt. En PR som ikke burde vært laget er
billigere å lukke enn å merge.

---

# PR-oppskriften — repoets viktigste seksjon

Hele poenget er at **hvem som helst skal få inn en endring uten å kunne Git**. Brukeren
sier hva de vil ha, og til slutt «publiser». Du gjør resten.

Oppskriften har ni steg. **Hopp aldri over et steg**, og hopp aldri rett til steg 6 fordi
endringen «er liten». De tre som oftest glemmes er steg 1 (ny branch **før** du endrer
noe), steg 8 (konfliktsjekk **etter** at PR-en er laget) og steg 9 (lese kommentarene
som kommer inn).

| # | Steg | Når |
|---|------|-----|
| 0 | [Sjekk GitHub](#steg-0--sjekk-github-før-du-gjør-noe-som-helst) | Første handling i sesjonen |
| 1 | Ny branch fra oppdatert `main` | **Før** første filendring |
| 2 | Gjør arbeidet | — |
| 3 | Se at det virker i nettleseren | Før commit |
| 4 | Vis brukeren hva som endres, få OK | Før commit |
| 5 | **Signert** commit | — |
| 6 | Push branchen | — |
| 7 | Lag PR med GitHub MCP | — |
| 8 | **Sjekk PR-en på GitHub og fiks konflikter** | Etter at PR er laget |
| 9 | **Les og svar på kommentarer** | Etter PR, og hver gang dere er innom igjen |

---

## Steg 1 — Ny branch, alltid, før du endrer noe

**Regelen: begynner du på noe nytt, lager du en ny branch. Uten unntak.**

«Noe nytt» = alt brukeren ber om som ikke er en direkte fortsettelse av det dere akkurat
holdt på med. Nytt sitat fra Bjarne, ny farge, en liten tekstfiks, ny seksjon — alt er
noe nytt.

```bash
git branch --show-current                        # hvor står vi?
git checkout main && git pull origin main        # hent siste fra GitHub
git checkout -b <prefiks>/<kort-beskrivelse>
```

| Står du på … | Da gjør du |
|--------------|------------|
| `main` | **Må** lage ny branch. Aldri commit på `main`. |
| En branch med åpen PR om samme sak | Fortsett på den — ikke lag ny PR |
| En branch med åpen PR om **noe annet** | Ny branch fra oppdatert `main` |
| En branch uten PR, ferdig merget | Ny branch fra oppdatert `main` |

Branch-prefikser: `feat/` (noe nytt) · `fix/` (feilretting) · `text/` (tekstendringer) ·
`design/` (farger, font, layout) · `docs/` (dokumentasjon) · `chore/` (opprydding).

Navn på branch: små bokstaver, bindestrek mellom ord, **ingen æ/ø/å, ingen mellomrom,
ingen skråstrek utover prefikset**. `text/nye-bjarne-sitater` — ikke
`Tekst/Nye Bjarne-sitater på forsiden`.

Lager du branchen fra en **utdatert** `main`, får du konflikter i steg 8. Det er den
vanligste grunnen til at en PR ikke lar seg merge.

> **`main` er ikke teknisk beskyttet i dette repoet.** Git vil altså la deg pushe rett til
> `main` hvis du prøver. Det gjør vi likevel ikke: hver eneste endring går gjennom branch
> og PR, så det finnes en lesbar historikk og et sted å angre. Denne regelen er vår, ikke
> GitHubs — og den gjelder like fullt.

## Steg 2 — Gjør arbeidet

Alt ligger i `index.html` — HTML, CSS og JavaScript i samme fil. Les
[Feller i akkurat dette repoet](#feller-i-akkurat-dette-repoet) før du endrer layout,
og [Hvem Bjarne er](#hvem-bjarne-er--les-dette-før-du-endrer-tekst) for tone og innhold.

Ett prinsipp som gjelder gjennomgående: **det er ingen byggsteg og ingen tester.** En
skrivefeil i HTML-en blir ikke fanget av noe verktøy — den går rett på lufta når PR-en
merges. Derfor er steg 3 ikke valgfritt.

## Steg 3 — Se at det virker i nettleseren (obligatorisk)

**Åpne siden og se på den.** Ikke bare les koden.

Full oppskrift i [Testing](#testing--obligatorisk-før-du-sier-deg-ferdig). Minimum:

- Siden lastes uten feil i konsollen
- Endringen er faktisk synlig
- Ingen horisontal scrolling på 375px bredde (mobil) — den vanligste feilen her
- Energimåleren, nedtellingen og kaffeknappen virker fortsatt

## Steg 4 — Vis brukeren hva som skjer, og få et ja

```bash
git status
git diff --stat
```

Oppsummer på vanlig norsk, uten filnavn-sjargong der det går an:

> «Vi har gjort disse endringene:
> - Lagt til tre nye Bjarne-sitater
> - Endret datoen i nedtellingen
>
> Jeg har sjekket at det ser riktig ut både på mobil og desktop.
> Vil du at jeg publiserer dette nå?»

Er det endringer brukeren **ikke ba om** (filer du rørte underveis, eller som lå der fra
før): si det eksplisitt og spør om de skal være med. **Aldri smugle med endringer.**

## Steg 5 — Commit — og den **må** være signert

**Usignerte commits skal ikke merges.** Signering er allerede satt opp på denne maskinen
(GPG, `commit.gpgsign = true`).

```bash
git add index.html                 # aldri `git add -A` uten å ha sett `git status`
git commit -m "<beskrivende melding på norsk>"
```

Commit-meldinger skrives på **norsk**, i imperativ, og beskriver *hva* som ble endret:
«Legg til tre nye Bjarne-sitater», ikke «endringer» eller «fiks».

> **Ingen Claude-referanser i commits eller PR-er.** Ikke `Co-Authored-By: Claude`, ikke
> «🤖 Generated with Claude Code», ikke noe annet spor av verktøyet. Dette overstyrer
> standardoppførselen til agenten.

Sjekk med én gang at signaturen faktisk ble laget — **ikke anta**:

```bash
git log --format='%h %G? %s' origin/main..HEAD
```

Kolonnen i midten er signaturstatus. `G` = god signatur (det du vil ha). `N` = ingen
signatur. `B`/`U`/`E` = ugyldig eller kan ikke sjekkes.

Er noe annet enn `G`, fiks det **nå** — ikke etter at PR-en er laget:

| Symptom | Årsak | Fiks |
|---------|-------|------|
| `N` på alle commits | Signering ikke slått på | `git config --global commit.gpgsign true`, så `git commit --amend --no-edit -S` |
| `gpg: signing failed: No pinentry` | pinentry mangler på macOS | `brew install pinentry-mac`, legg `pinentry-program /opt/homebrew/bin/pinentry-mac` i `~/.gnupg/gpg-agent.conf`, så `gpgconf --kill gpg-agent` |
| `gpg: signing failed: No secret key` | `user.signingkey` peker på en nøkkel som ikke finnes | `gpg --list-secret-keys --keyid-format=long`, sett riktig ID |
| `gpg: signing failed: Inappropriate ioctl for device` | GPG får ikke spurt om passord | `export GPG_TTY=$(tty)` og prøv igjen |
| Signert lokalt, men **«Unverified»** på GitHub | Nøkkelen er ikke lastet opp, eller e-posten matcher ikke | Last opp nøkkelen som *signing key* i GitHub-profilen; `git config user.email` må være en verifisert e-post på GitHub |

Fikser du signeringen etter at commiten er laget, må commiten lages på nytt:
`git commit --amend --no-edit -S` (og `git push --force-with-lease` hvis den alt er pushet).

Forklar aldri dette som brukerens feil. Si heller: «Jeg måtte sette opp en digital
signatur på commiten — det er et krav i repoet. Ordnet.»

## Steg 6 — Push

```bash
git push -u origin <branch-navn>
```

`-u` trengs bare første gang for branchen; deretter holder `git push`.

**Aldri `git push origin main`.** Får du «rejected — non-fast-forward», stopp og les
steg 8 — noen har pushet i mellomtiden.

## Steg 7 — Lag PR med GitHub MCP

Bruk `mcp__github__create_pull_request` (`owner: trondstromlie`, `repo: skadefryd`,
`base: main`). **Ikke `gh` CLI, og ikke be brukeren gjøre det i nettleseren.**

PR-beskrivelsen skrives på **norsk**:

```markdown
## Hva
Én til tre kulepunkter om hva som er endret.

## Hvorfor
Én setning om hvorfor — hvem ba om det, hvilket behov løser det.

## Hvordan se det
Hvor på siden endringen vises, f.eks. «sitatkarusellen midt på siden».

## Testet
- [x] Åpnet siden lokalt, ingen feil i konsollen
- [x] Sjekket på 375px bredde — ingen horisontal scrolling
```

Ingen Claude-signatur i PR-beskrivelsen. Sjekk først at det ikke allerede finnes en åpen
PR for branchen (`mcp__github__list_pull_requests`). Finnes den: push til samme branch —
PR-en oppdaterer seg selv. **Aldri to PR-er for samme endring.**

## Steg 8 — Sjekk PR-en på GitHub og fiks konflikter

**Dette steget er obligatorisk, og det er det som oftest glemmes.** En PR som ser grei ut
lokalt kan være umulig å merge fordi noen endret de samme linjene mens dere jobbet.
Alt ligger i én fil her, så sjansen for konflikt er større enn i vanlige repoer.

**1. Hent PR-en:** `mcp__github__get_pull_request` med PR-nummeret. Se på
`merge_commit_sha`:

| `merge_commit_sha` | Betyr | Hva du gjør |
|--------------------|-------|-------------|
| En SHA (`"00f2725…"`) | GitHub klarte prøvesammenslåingen — **ingen konflikt** | Gå videre |
| `null` | Enten konflikt, eller GitHub regner fortsatt | Vent noen sekunder, hent på nytt. Fortsatt `null`: behandle som konflikt |

> **NB:** GitHub-API-et har feltene `mergeable` og `mergeable_state`, men **MCP-serveren
> returnerer dem ikke** — den trimmer svaret. Ikke let etter dem, og ikke konkluder ut fra
> at de mangler.

**Lokal fasit — kjør alltid denne i tillegg**, den er raskere og helt entydig:

```bash
git fetch origin
git merge-tree --write-tree origin/main HEAD >/dev/null && echo "OK, ingen konflikt" || echo "KONFLIKT"
```

**2. Løs konflikten — brukeren skal aldri se en konflikt:**

```bash
git fetch origin
git rebase -S origin/main          # -S = behold signeringen på commitene
```

Git stopper i hver konflikt. Åpne filen, se etter `<<<<<<<`, `=======`, `>>>>>>>`, behold
det som er riktig — som regel **begge deler**, ikke bare din versjon — og fjern markørene.
Deretter:

```bash
git add index.html
git rebase --continue
# åpne siden i nettleseren på nytt — en rebase kan ha brukket layouten
git push --force-with-lease origin <branch-navn>
```

Bruk **alltid** `--force-with-lease`, aldri `--force`: den nekter å pushe hvis noen andre
har pushet til branchen i mellomtiden, i stedet for å overskrive arbeidet deres.

Kommer du helt ut av det: `git rebase --abort` setter alt tilbake som før. **Ingenting er
ødelagt**, og du kan prøve på nytt.

Kjør `merge-tree`-sjekken på nytt og bekreft at den sier OK.

**3. Sjekk statuser:** `mcp__github__get_pull_request_status`. Er `total_count: 0` og
`state: "pending"`, betyr det **ikke** at noe er galt — dette repoet har ingen
CI-workflows. Din egen nettlesersjekk er fasiten. Ikke rapporter det som en feil.

**4. Fortell brukeren, med lenke:**

> «Ferdig! PR-en ligger her: <lenke>
>
> Den er klar til å merges — ingen konflikter. Når den merges, oppdaterer
> skadefryd.tech seg automatisk etter et par minutter.»

Måtte du rydde underveis, si det i én setning uten teknisk detalj: «Jeg måtte flette inn
noen endringer som lå der fra før, men det er ordnet — du trenger ikke tenke på det.»

### Lenken er obligatorisk — hver eneste gang

**Nevner du en PR, limer du inn hele URL-en.** Ikke «PR-en er laget», ikke «#27», ikke
«du finner den på GitHub». Full lenke: `https://github.com/trondstromlie/skadefryd/pull/27`

Brukeren jobber ikke i GitHub til daglig og skal ikke måtte lete. Lenken er klikkbar i
terminalen, og den er hele veien fra «agenten sier den er ferdig» til at brukeren faktisk
ser resultatet.

Dette gjelder **alle** ganger PR-en nevnes: når du lager den, når du pusher nye commits,
når du har løst en konflikt, når brukeren spør «hvordan går det?», og når du oppsummerer
flere PR-er — da med lenke på **hver enkelt**. Lenken hentes fra `html_url` i svaret fra
`mcp__github__create_pull_request`.

### Hvem merger?

Dette er et lite repo med én eier. Det finnes ikke alltid noen andre til å godkjenne.

- **Er brukeren eieren (Trond):** du kan merge PR-en med
  `mcp__github__merge_pull_request` — men **bare når brukeren eksplisitt sier ja**. Spør
  først: «Skal jeg merge den nå, så går den live?»
- **Er brukeren en annen:** ikke merge. Si fra at PR-en venter på godkjenning, og gi
  lenken.

Merge betyr **live på skadefryd.tech**. Det er ikke et internt steg — si det tydelig.

## Steg 9 — Les og svar på kommentarer på PR-en

En PR er ikke ferdig når den er laget. Kommentarer kommer på e-post eller inne i GitHub,
og for en ikke-utvikler er de vanskelige å tolke. **Det er agentens jobb å hente dem,
oversette dem og gjøre noe med dem.**

Sjekk kommentarer når brukeren spør «har det skjedd noe med PR-en?», når du kommer
tilbake til den i en senere sesjon, og alltid før du sier at noe er klart til merge.

**1. Hent alt som er sagt — det ligger tre steder:**

| Verktøy | Henter |
|---------|--------|
| `mcp__github__get_pull_request_reviews` | Godkjenninger og endringsforespørsler |
| `mcp__github__get_pull_request_comments` | Kommentarer knyttet til en **linje i koden** — her havner Copilot |
| `mcp__github__get_issue` (med PR-nummeret) | Vanlig samtale i PR-tråden |

Du må sjekke **alle tre**. En godkjenning uten kommentarer i den ene betyr ikke at det
ikke ligger ti forslag i den andre.

**2. Sorter kommentarene:**

| Fra | Hva du gjør |
|-----|-------------|
| Copilot-bot | Vurder hvert forslag. Gjennomfør de som stemmer, avvis de som ikke gjør det — og skriv **hvorfor** i et svar |
| Kollega, `CHANGES_REQUESTED` | Gjør endringen, push til samme branch, si fra til brukeren hva som ble bedt om |
| Kollega, spørsmål | Fagavgjørelse (ordlyd, tone, innhold) → spør brukeren. Teknisk → svar selv |
| `APPROVED` | Si fra at den er godkjent og kan merges |

**Aldri gjennomfør et Copilot-forslag automatisk uten å vurdere det.** Boten kjenner ikke
konteksten her — den vil for eksempel «rydde bort» base64-strengene som er lagt inn som
bevisste feller, eller foreslå engelsk tekst i en norsk UI-streng.

**3. Push rettelsene til samme branch.** PR-en oppdaterer seg selv — lag aldri ny PR. Gå
tilbake til steg 8: nye commits kan ha skapt nye konflikter.

**4. Svar i PR-en** med `mcp__github__add_issue_comment`, kort og på norsk.

**5. Oppsummer for brukeren på vanlig norsk** — aldri bare lim inn kommentarene. Er det
ingenting nytt, si det like klart: «Ingen har kommentert PR-en ennå.»

## Etter merge

GitHub Pages bygger og publiserer `main` automatisk til **skadefryd.tech**. Det tar
typisk under ett minutt, av og til et par. Ingen manuelle steg.

Når PR-en er merget, er branchen ferdig. Rydd opp:

```bash
git checkout main
git pull origin main
git branch -d <branch-navn>              # sletter lokalt
git push origin --delete <branch-navn>   # sletter på GitHub (valgfritt)
```

## Ting du aldri gjør

- Committer eller pusher til `main`
- Committer usignert
- Merger en PR uten at brukeren har sagt ja
- Lager PR nummer to for en endring som alt har en åpen PR
- Sier «ferdig» før steg 8 er gjort
- Nevner en PR uten å lime inn hele lenken til den
- Sier at noe virker uten å ha sett det i en nettleser
- Skriver `Co-Authored-By: Claude` eller «Generated with Claude Code» i commit eller PR
- Kjører `git add -A` uten å ha lest `git status` først
- Bruker `git push --force` uten `--with-lease`
- Ber brukeren løse en merge-konflikt, kjøre en git-kommando eller klikke i GitHub-UI-et
- Bruker `gh` CLI når GitHub MCP finnes

## GitHub MCP

| Verktøy | Brukes til |
|---------|-----------|
| `mcp__github__list_pull_requests` | Finnes det allerede en åpen PR? (Steg 0 og 7) |
| `mcp__github__create_pull_request` | Lage PR-en (Steg 7) |
| `mcp__github__get_pull_request` | Konfliktsjekk — `merge_commit_sha` (Steg 8) |
| `mcp__github__get_pull_request_status` | Statuser — alltid tom her, se Steg 8 punkt 3 |
| `mcp__github__update_pull_request_branch` | Oppdatere en branch som bare ligger bak |
| `mcp__github__get_pull_request_reviews` | Godkjenninger (Steg 9) |
| `mcp__github__get_pull_request_comments` | Kommentarer på kodelinjer (Steg 9) |
| `mcp__github__get_issue` | Samtaletråden i PR-en (Steg 9) |
| `mcp__github__add_issue_comment` | Svare i PR-en (Steg 9) |
| `mcp__github__merge_pull_request` | Merge — kun etter eksplisitt ja fra eieren |

---

# Bagatellene — alt utviklere glemmer å fortelle

Dette er tingene «alle vet» som ingen sier høyt, og som er den vanligste grunnen til at en
ikke-utvikler står fast. **Agenten skal håndtere hver enkelt av dem selv, uten å bli
spurt** — og forklare i én setning når det skjer.

### Før du begynner

- **Stå i prosjektmappa.** Se [Steg minus én](#steg-minus-én--stå-i-riktig-mappe).
- **Tilby å åpne siden lokalt med én gang.** Brukeren skal se det de endrer, ikke bare
  høre om det. Se [Kjøre siden lokalt](#kjøre-siden-lokalt).
- **Hent alltid ned siste versjon først.** `git fetch origin` er ikke valgfritt. Mappa på
  maskinen er en kopi som ble tatt sist gang noen jobbet — den kan være måneder gammel.
- **Sjekk hvilken branch dere står på** før første filendring. Står dere på `main` og
  begynner å redigere, må alt flyttes etterpå (det går fint — se
  [Angrekurset](#jeg-har-endret-filer-mens-jeg-sto-på-main)).
- **Sjekk om det ligger ulagrede endringer fra sist.** `git status`. Vet ingen hva de er,
  ikke slett dem — `git stash` dem og si fra.

### Mens dere jobber

- **Endringer er lagret på maskinen, men ingen andre kan se dem.** Brukeren tror ofte
  enten at alt er publisert med én gang, eller at alt forsvinner. Ingen av delene stemmer.
  Si det tidlig — hele forklaringen ligger i
  [Alt du gjør er lokalt](#alt-du-gjør-er-lokalt--helt-til-du-pusher).
- **Blir det rotete, kan alt nullstilles.** Fire kommandoer henter tilbake nøyaktig den
  versjonen som ligger live. Si det når brukeren blir nervøs — se
  [Nødbremsen](#nødbremsen-kast-alt-lokalt-og-hent-tilbake-versjonen-som-ligger-på-nett).
- **Å lagre en fil er ikke det samme som å committe.** Redigeringen ligger i fila; git vet
  ikke om den før `git add` + `git commit`.
- **Nettleseren cacher.** Ser du ikke endringen etter at du lastet siden på nytt, gjør en
  hard refresh: **Cmd+Shift+R** (Mac) / **Ctrl+F5** (Windows).
- **Bytter du branch med ulagrede endringer, blir de med over.** Det er nesten aldri det
  brukeren vil. Commit eller `git stash` først.
- **Åpner git plutselig en tekst-editor du ikke skjønner** (svart skjerm, `~` nedover
  venstre kant) — det er `vim`. Se [Angrekurset](#git-åpnet-en-rar-editor-jeg-ikke-kommer-ut-av).
- **`git log` og `git diff` «henger».** De gjør ikke det — de viser resultatet i en
  bla-visning. Trykk `q` for å komme ut. Agenten bør uansett kjøre dem med `--no-pager`.

### Filnavn og innhold

- **Ingen æ, ø, å, mellomrom eller store bokstaver i filnavn.** Filnavnet blir en URL.
  `Bjarne Bilde Stor.png` gir en lenke som brekker. Bruk `bjarne-bilde-stor.png`.
- **Store og små bokstaver:** macOS bryr seg ikke om forskjellen, Linux (som GitHub Pages
  kjører på) gjør det. `Bjarne.PNG` referert som `bjarne.png` virker lokalt og brekker
  live. Hold alt i små bokstaver.
- **Bilder og filer som skal være tilgjengelige på nettsiden, legges i `assets/`** og
  refereres relativt (`assets/filnavn.png`), aldri med absolutt sti fra maskinen din.
- **Ingen kundedata, personopplysninger eller interne dokumenter inn i repoet.** Siden er
  offentlig på skadefryd.tech, repoet er offentlig, og **git husker alt for alltid** — å
  slette en fil i neste commit fjerner den ikke fra historikken.
- **Aldri commit `.env`, tokens, API-nøkler eller passord.** Ser du noe som ligner et
  token i en fil du er i ferd med å legge til, stopp og si fra.
- **Aldri `git add -A` eller `git add .` uten å ha lest `git status` først.** Det er slik
  ting som ikke skulle vært med, blir med — `.DS_Store`, midlertidige filer, skjermbilder.

### Rundt publisering

- **Å pushe er ikke det samme som å publisere.** Endringen er på GitHub, men først synlig
  på skadefryd.tech når PR-en er **merget**. Si det eksplisitt, ellers lurer brukeren på
  hvorfor ingenting skjedde.
- **Etter merge tar det opptil et par minutter** før nettsiden er oppdatert. Spør brukeren
  «hvorfor ser jeg det ikke?», vent litt og gjør en hard refresh før du begynner å lete
  etter feil.
- **Ser du fortsatt den gamle siden etter fem minutter**, sjekk om bygget faktisk gikk
  gjennom: `gh api repos/trondstromlie/skadefryd/pages/builds/latest`. Feltet `status`
  skal være `built`.
- **Åpner du opp arbeidet igjen etter at PR-en er merget, start på nytt fra `main`.** Den
  gamle branchen er ferdig; ikke bygg videre på den.

### Når noe ser skummelt ut

Feilmeldinger i terminalen ser dramatiske ut, men er nesten alltid ufarlige. Agenten skal
aldri lime en rå feilmelding inn i svaret uten å oversette den. Si hva som skjedde, hva du
gjorde med det, og om brukeren trenger å gjøre noe (som regel: nei).

Ord som høres verre ut enn de er:

| Det står | Det betyr |
|----------|-----------|
| `fatal:` | «jeg klarte ikke dette» — ingenting er ødelagt |
| `detached HEAD` | Du står på et gammelt lagringspunkt i stedet for en branch. Fikses med én kommando |
| `rejected` | Noen pushet før deg. Hent ned deres først |
| `conflict` | To endringer på samme linje. Noen må velge |
| `error: failed to push some refs` | Samme som `rejected` |

---

# Angrekurset — «jeg gjorde noe feil, hva nå?»

Nesten alt i git kan angres. Her er de situasjonene ikke-utviklere faktisk havner i.
**Agenten fikser dem selv** og forteller brukeren i én setning hva som skjedde.

### Jeg har endret filer mens jeg sto på `main`

Vanligste feilen, og helt ufarlig — så lenge du **ikke har committet**:

```bash
git stash                       # legg endringene i skuffen
git checkout main && git pull origin main
git checkout -b <ny-branch>
git stash pop                   # hent dem ut igjen, nå på riktig branch
```

Har du **allerede committet** på `main` (men ikke pushet):

```bash
git checkout -b <ny-branch>     # branchen får med seg commiten
git checkout main
git reset --hard origin/main    # sett main tilbake til det GitHub har
git checkout <ny-branch>        # fortsett arbeidet her
```

### Jeg vil kaste alt jeg har gjort siden sist

```bash
git checkout -- index.html      # kast endringer i én fil
git reset --hard HEAD           # kast alle endringer siden siste commit
```

`--hard` sletter for godt. **Spør brukeren først**, og si det rett ut: «Da forsvinner det
du har gjort siden sist lagringspunkt. Er det greit?»

### Jeg committet noe feil / glemte en fil

Så lenge commiten **ikke er pushet**:

```bash
git add <den glemte filen>
git commit --amend --no-edit -S     # slå den sammen med forrige commit
```

Vil du bare endre teksten i meldingen:

```bash
git commit --amend -m "Ny og bedre melding" -S
```

Er commiten alt pushet, må du `git push --force-with-lease` etterpå — og det er greit
**så lenge branchen er din egen og ingen andre jobber på den**.

### Jeg vil angre en commit, men beholde endringene

```bash
git reset --soft HEAD~1    # commiten forsvinner, filene beholder endringene
```

### Jeg slettet noe jeg ikke skulle

Er det committet en gang, finnes det:

```bash
git log --oneline -- <filnavn>       # finn siste commit som hadde filen
git checkout <commit-sha> -- <filnavn>
```

Er hele branchen «borte»:

```bash
git reflog        # alt du har gjort de siste dagene, med sha
git checkout -b redning <sha>
```

`reflog` er redningsplanken. **Nesten ingenting er faktisk borte.**

### `Permission denied (publickey)`

Git får ikke logget inn på GitHub over SSH.

```bash
ssh -T git@github.com          # skal svare "Hi <brukernavn>! You've successfully authenticated"
ls ~/.ssh/*.pub                # finnes det en nøkkel?
```

Finnes ingen nøkkel: lag en (`ssh-keygen -t ed25519 -C "<e-post>"`), og legg den offentlige
delen inn under GitHub → Settings → SSH and GPG keys. Alternativt kan du bytte til HTTPS:

```bash
git remote set-url origin https://github.com/trondstromlie/skadefryd.git
```

### `Host key verification failed`

Første gang du kobler til GitHub fra denne maskinen. Kjør `ssh -T git@github.com` og svar
`yes` på spørsmålet om fingeravtrykk.

### `Please tell me who you are`

Git vet ikke hvem som committer:

```bash
git config --global user.name "Fornavn Etternavn"
git config --global user.email "din@epost.no"
```

E-posten må være verifisert på GitHub for at commiten skal vises som «Verified».

### Git åpnet en rar editor jeg ikke kommer ut av

Svart skjerm med `~` nedover venstre side = `vim`.

- **Lagre og lukk:** trykk `Esc`, skriv `:wq`, trykk Enter
- **Avbryt uten å lagre:** trykk `Esc`, skriv `:q!`, trykk Enter

Unngå problemet helt ved alltid å bruke `-m` på commit. Og for brukerens skyld:

```bash
git config --global core.editor "nano"     # eller "code --wait" for VS Code
```

### `You are in 'detached HEAD' state`

Du står på et gammelt punkt i historikken, ikke på en branch. Ufarlig:

```bash
git checkout main              # tilbake til normalen
git checkout -b <navn>         # eller: behold det du står på som en ny branch
```

### `Your branch is behind 'origin/main' by N commits`

GitHub har nyere ting enn deg:

```bash
git pull origin main
```

### `Your branch and 'origin/main' have diverged`

Både du og GitHub har nye commits. Stopp og undersøk før du gjør noe — se
[Steg 0](#steg-0--sjekk-github-før-du-gjør-noe-som-helst). Ikke bare `git pull` blindt.

### `error: failed to push some refs` / `rejected`

Noen pushet før deg:

```bash
git fetch origin
git rebase -S origin/main
git push --force-with-lease origin <branch>
```

### Jeg står midt i en rebase og skjønner ingenting

```bash
git rebase --abort
```

Alt er tilbake som før rebasen. Prøv på nytt, eller be om hjelp.

### `.DS_Store` dukker opp i `git status`

macOS-fil, skal aldri committes. Ligger den der, legg den i `.gitignore`:

```bash
echo ".DS_Store" >> .gitignore
```

---

# Hva dette repoet er

Hackathon-landingssiden for **Skadefryd med Bjarne** — et internt arrangement i Gjensidige
Skade, **23. september 2026 kl. 12:00 hos Itera** (Stortingsgata 6, Oslo).

| | |
|---|---|
| **Live** | https://skadefryd.tech (GitHub Pages, `main`, rot-mappa) |
| **Repo** | git@github.com:trondstromlie/skadefryd.git |
| **Stack** | Én statisk `index.html`. Ingen rammeverk, ingen pakker, ingen byggsteg |
| **Tester** | Ingen. Nettleseren er fasiten |

> **Merk:** Det finnes også et `skadefryd`-repo under Gjensidige-organisasjonen på GitHub.
> Det er **arkivert og skal ikke brukes**. Alt arbeid — branches, PR-er, issues — går mot
> `trondstromlie/skadefryd`. Er du i tvil: `git remote -v` er fasit.

## Hvem Bjarne er — les dette før du endrer tekst

Bjarne er en fiktiv AI-agent, og han er hele premisset for hackathonet. **Eva**, **Sofie** og
**Frank** er Skades offisielle AI-kjendiser — interne AI-persona som faktisk ble valgt ut av
Skade. Bjarne ble ikke valgt. Det er bakhistorien.

Bjarne er:

- Teknisk kompetent, men foretrekker å drikke kaffe fremfor å jobbe. Dette er hovedtrekket,
  og det er det energimåleren, kaffekrisa og kaffeknappen handler om.
- Vrang og lite samarbeidsvillig — avviser innspill, insisterer på sin egen metode.
- Fullstendig blind for at punktet over er *grunnen* til at han ikke ble valgt som
  AI-kjendis. Han har sin egen teori (politikk, smak, urettferdighet) og nevner den gjerne.
- Sarkastisk, selvsikker, alltid kortfattet. Svarer alltid på norsk.

**Premisset:** Hackathonet er ikke offisielt sanksjonert av Gjensidige. Bjarne satte det opp
selv, som et eget PR-stunt for å bevise at han fortjener en plass blant kjendisene. Ingen ba
ham om det.

**Tonen er tørr og underdreven.** Unngå forklarende humor — la Bjarne snakke for seg selv.
Ironien (at hans egen vrangvilje er grunnen til snubben) skal *vises* gjennom hans egne
uttalelser, aldri forklares. Bjarne skal **aldri** få en innsikt eller oppvåkning om dette —
han er overbevist om sin egen rett gjennom hele teksten. Eva, Sofie og Frank navngis direkte
i kopien; bruk dem gjerne i nye sitater for å holde sjalusi-twisten synlig.

## Hva som er på siden

| Del | Hva den gjør |
|-----|--------------|
| **Energimåler** (fast topplinje) | Bjarnes energinivå 0–100, tømmes 1 poeng hvert 8. sekund. Knappen «☕ Gi Bjarne en kaffe før han sovner» gir +25. Under 20: rød pulserende advarsel. På 0 tar en Windows-BSOD over hele skjermen (`CAFFEINE_LEVEL_CRITICAL`) med restart-knapp som gir 60. Lagres i `localStorage` (`bjarne_energy`, `bjarne_energy_ts`) |
| **Nedtelling** | Teller ned til 23. september 2026 kl. 12:00 (`index.html` er fasit, se [Datoen](#datoen)). Når målet nås åpnes den låste «oppdrag»-seksjonen automatisk |
| **Oppdraget** | Låst boks fram til nedtellingen er ferdig. Da byttes den ut med hele oppgaveteksten, hentet fra `assets/oppdrag.bin`, se [Oppdraget](#oppdraget) |
| **Bjarne-sitater** | 18 sitater roterer hvert 6. sekund med fade. Kaffe (hovedvekt), hackathonet, AI-selvbilde, og — uten å forklare det — avvisningen av samarbeid og teorien om hvorfor han ikke ble valgt |
| **Forberedelser** | Tilgang til genai.gjensidige.no (valgfritt, krever `az login`), OpenCode (anbefalt verktøy), kontakt Trond eller Ulrik, andre verktøy (VS Code, Cursor, Azure CLI, Node, Python) |
| **Features-grid** | 7 fiktive AI-features Bjarne aldri ble bedt om å bygge: Kaffekorrelasjon™, Sukk-detektor, Unngåelsesindeks, Bjarne spår fremtiden, Effektivitetsrapporten, Kaffekritisk varsel, Kjendis-tracker (teller omtaler av Eva/Sofie/Frank mot Bjarnes eget tall) |
| **Kioskmodus** | Egen fullskjermvisning for skjermer i fellesarealer, se under |

## Kioskmodus

Legg `?kiosk=true` bak adressen — `https://skadefryd.tech/?kiosk=true` — så bytter siden til
en visning laget for en skjerm som henger på veggen: stor nedtelling, dato og sted, og en
QR-kode til påmeldingsskjemaet. Ingenting annet.

**Kiosken har tre tilstander gjennom dagen**, styrt av to klokkeslett i kioskskriptet
(`TARGET` og `SLUTT`):

| Når | Hva skjermen viser |
|-----|--------------------|
| Før kl. 12:00 | Nedtelling, dato og sted, og QR-koden til påmeldingen |
| Kl. 12:00–20:00 (`html.started`) | Bildet av Bjarne med trekkspill og «Hackathon med Bjarne har startet». Nedtelling, meta og QR faller bort — påmeldingen har gjort jobben sin |
| Etter kl. 20:00 (`html.ferdig`) | Samme bilde, men teksten blir «Takk for nå. Hilsen Bjarne» |

`SLUTT` står rett under `TARGET` i kioskskriptet. Flytter du datoen, må **begge**
oppdateres — se [Datoen](#datoen).

Bildet er **ikke** `loading="lazy"`. En kioskskjerm kan ha stått på i ukevis når klokka
blir tolv, og skal ikke være avhengig av at nettet virker akkurat da.

- Den vanlige forsiden skjules med CSS (`html.kiosk body > *:not(#kiosk)`), og hele
  hovedskriptet hoppes over. Ingen energimåler, ingen partikler, ingen canvas som tegner —
  skjermen skal kunne stå på i ukevis.
- Musepekeren er skjult, men kommer fram så snart noen beveger musen og forsvinner igjen
  etter tre sekunder.
- Ligger skjermen i portrett, stables innholdet automatisk.
- `?kiosk=false` og `?kiosk=0` gir vanlig forside, slik at lenken kan skrus av uten å endres.

**QR-koden er ikke et bilde.** Den regnes ut i nettleseren av en liten QR-koder nederst i
`index.html`, og lenken hentes fra påmeldingsknappen på forsiden. Endrer du den knappen,
følger QR-koden etter av seg selv — det finnes ingen bildefil å huske på.

Under «Skann med mobilen» står linja **«Kan bare åpnes i Edge på mobil»** med en liten
Edge-logo. Grunnen: påmeldingsskjemaet ligger bak Gjensidiges pålogging, som avviser Safari
og Chrome med en feilmelding folk ikke forstår. Det finnes ingen måte å tvinge én QR-kode
til å åpne Edge på både iPhone og Android — teksten er derfor det eneste som gjør jobben.

Linja er **bevisst holdt dempet**: grå, liten, uten ramme og uten bakgrunn. Prøvde vi den
som en farget advarselsboks, ble hele kioskbildet bunntungt. Den skal leses som en fotnote,
ikke som en overskrift.

Logoen er Edge-logoen fra Simple Icons, limt inn som inline SVG — ingen bildefil, så
kiosken virker uten nett. Den er **den eneste fargeflekken i linja**, med Edges blågrønne
gradient. Grå ble den prøvd først, men så liten er Edge-logoen umulig å kjenne igjen uten
fargene — den leses som en virvel, og folk gjettet på Chrome. Fargen er derfor et bevisst
unntak fra ellers dempet linje.

Skal du endre størrelsen på koden: den slutter å la seg skanne under ca. 150 px. Dagens
`38vmin` gir rundt 410 px på en 1080p-skjerm, altså nesten tre ganger margin. Blir
påmeldingslenken lengre, blir koden tettere og trenger mer plass.

## Designvalg

- **Fonter:** Bebas Neue (overskrifter), DM Serif Display (sitater/italic), DM Mono
  (brødtekst og kode).
- **Farger:** bakgrunnen er midnattsblå, ikke brun — `--bg`/`--espresso: #0B0D1A`,
  `--cream: #F0EAD6`, `--amber: #C97B2A`, `--amber-light: #E8A94A`, `--rust: #8B3A1A`.
  I tillegg finnes AI-aksentene `--ai: #7C5CFC` og `--ai-light: #A78BFA`, og høstfargene
  `--forest`, `--ochre`, `--terra`. **`index.html` er fasit** — les `:root` der før du
  bruker en farge herfra.
- **Custom cursor** — skjules automatisk på touch-enheter (`@media (pointer: coarse)`).
- **Damppartikler** — animerer oppover fra bunnen, begrenset til 5–95 % av bredden for å
  unngå overflow.
- **Mobil (≤640px):** energibaren stables vertikalt (spor øverst, knapp under, full bredde),
  kafferingen skjules, info-cellene (dato/sted/etc.) vises én per rad med horisontal
  skillelinje, og padding er redusert gjennomgående.

## Filene

| Fil | Hva det er |
|-----|-----------|
| `index.html` | **Hele siden.** HTML, CSS og JavaScript i samme fil, ~1450 linjer |
| `assets/oppdrag.bin` | **Selve oppdraget**, base64-encodet HTML. Vises når nedtellingen er ferdig, se [Oppdraget](#oppdraget) |
| `assets/f.bin` | Base64-encodet påskeegg-melding til den som graver den frem |
| `assets/bjarne-trekkspill.jpg` | Bildet kiosken viser fra kl. 12:00 på selve dagen |
| `CNAME` | Domenet (`skadefryd.tech`). **Slettes aldri** — da faller domenet ned |
| `AGENTS.md` | **Denne fila — eneste kilde til sannhet.** Alt om prosjektet står her |
| `CLAUDE.md` | Bare en peker hit. Ikke dupliser innhold dit |

## Se siden lokalt

`open index.html` holder for det meste; `python3 -m http.server 8000` når du trenger en
ekte server. Full oppskrift med feller og mobilvisning:
[Kjøre siden lokalt](#kjøre-siden-lokalt).

## Språk

All tekst på siden er på **norsk**, og tonen er tørr og underdreven. Endrer du tekst, les
[Hvem Bjarne er](#hvem-bjarne-er--les-dette-før-du-endrer-tekst) først — Bjarne har en
bestemt stemme, og han skal aldri få en oppvåkning om hvorfor han ikke ble valgt. All tekst
er gjennomgått med Claude Opus; bruk Opus for språklige endringer.

---

# Testing — obligatorisk før du sier deg ferdig

Det finnes ingen tester og ingen typecheck her. **Nettleseren er hele kvalitetssikringen.**
Derfor er dette ikke valgfritt.

Bruk Claude in Chrome eller Playwright, åpne siden, og sjekk:

| # | Sjekk | Hvorfor |
|---|-------|---------|
| 1 | Siden lastes, ingen røde feil i konsollen | En manglende `>` i HTML-en gir stille rar layout |
| 2 | Endringen er faktisk synlig | Redigerte du riktig sted i fila? |
| 3 | **375px bredde: ingen horisontal scrolling** | Den historisk vanligste feilen her |
| 4 | Kaffeknappen gir +25 energi | Kjernefunksjonen |
| 5 | Nedtellingen viser riktig tall | Målet er `2026-09-23T12:00:00` |
| 6 | Sitatene roterer | 18 sitater, bytte hvert 6. sekund |
| 7 | Oppdraget vises når datoen er passert | Test med en midlertidig dato, se under |
| 8 | Kiosken bytter tilstand riktig | `?kiosk=true` med midlertidig dato: bilde + «har startet», og «Takk for nå» etter kl. 20 |

**Ta skjermbilde og se på det med Read-verktøyet.** Et skjermbilde du ikke har åpnet er
ikke en sjekk.

Sjekk nummer 3 konkret — sett vindusbredden til 375px og kjør:

```js
document.documentElement.scrollWidth > window.innerWidth
```

Svarer den `true`, er det noe som stikker utenfor. Finn synderen:

```js
[...document.querySelectorAll('*')].filter(el => el.getBoundingClientRect().right > window.innerWidth)
```

**Nullstill energimåleren når du tester** — den ligger i `localStorage` og husker forrige
økt:

```js
localStorage.removeItem('bjarne_energy');
localStorage.removeItem('bjarne_energy_ts');
location.reload();
```

**Vil du teste BSOD-en** (skjer på 0 energi), sett energien lavt manuelt i stedet for å
vente. Og **vil du teste hva som skjer etter 23. september**, endre `target`-datoen
midlertidig — men **husk å sette den tilbake før commit.** En feil dato som går live er
den mest synlige feilen dette repoet kan få.

---

# Feller i akkurat dette repoet

Disse har alle gitt feil minst én gang. Les dem før du endrer noe i nærheten.

### Horisontal overflow på mobil

Den mest gjentakende feilen. Løsningen som står der nå er bevisst og skal ikke «ryddes
bort»:

- `overflow-x: clip` på `.page`
- `html { overflow-x: hidden }`
- `clip-path: inset(0)` på dampcontaineren
- Damppartiklene er begrenset til 5–95 % av bredden
- Kafferingen er `display: none` på mobil — `right: -30px` gjorde dokumentet bredere enn
  skjermen

iOS Safari ignorerer `overflow-x: hidden` på `body` alene. Legger du til noe som stikker ut
i kanten (dekorasjon, ikon, absolutt posisjonert element), **sjekk 375px før du committer**.

### Base64-strengene er feller — ikke rydd i dem

`_manifest`, `_0x`, `_secret` og `data-token` inneholder base64 som dekoder til korte,
sarkastiske Bjarne-meldinger. De er lagt inn med vilje, for nysgjerrige som inspiserer
kilden. **Ikke fjern dem, ikke «forenkle» dem, ikke forklar dem i en kommentar.**

`assets/f.bin` er en påskeegg-melding til den som klarer å dekode den. Den hentes fortsatt
i `index.html`, og det er **med vilje** — det er brødsmula som gjør egget mulig å finne.
Ikke rydd bort fetch-en fordi den ser ubrukt ut.

### Oppdraget

Oppgaveteksten ligger base64-encodet i `assets/oppdrag.bin`, ikke i klartekst i
`index.html`. Den hentes og vises først når nedtellingen er ferdig; prøver noen før den
tid, får de «Pent forsøk. Kom tilbake 23. september.» Innholdet er en HTML-bit som legges
inn med `innerHTML` og formateres av `.doc-*`-reglene i stilarket.

Base64 er **innpakning, ikke sikkerhet.** Alle som vil, kan hente fila og dekode den —
poenget er bare at teksten ikke står og lyser i kildekoden før dagen. Legg derfor aldri
noe der som faktisk må være hemmelig.

Slik endrer du teksten:

```bash
base64 -D assets/oppdrag.bin > /tmp/oppdrag.html    # pakk ut
# rediger /tmp/oppdrag.html
base64 -i /tmp/oppdrag.html | tr -d '\n' > assets/oppdrag.bin   # pakk inn igjen
```

**Ikke lim innholdet inn i klartekst i `index.html`.**

### Datoen

Fasiten er `index.html`, linje ~1045: `new Date('2026-09-23T12:00:00')`, og datoteksten
lenger opp i dokumentet. Endres datoen, må **begge** oppdateres — nedtellingen og den
synlige teksten er to forskjellige steder. Kioskskriptet har sitt eget par,
`TARGET` og `SLUTT` (kl. 20:00 samme dag) — begge må flyttes med. Datoen står også i
[Hva dette repoet er](#hva-dette-repoet-er) her i AGENTS.md; oppdater den i samme PR.

### Energimåler-layouten

Delt i `.energy-left` (spor + tall) og `.energy-right` (knapp) for stabil layout.
`min-width: 0` på flex-barna er kritisk — fjernes den, sprenger baren ut av skjermen på
mobil.

### Custom cursor

Skjules automatisk på touch-enheter via `@media (pointer: coarse)`. Rører du
cursor-koden, sjekk at den fortsatt er borte på mobil.

### Ingen byggsteg = ingen sikkerhetsnett

Et manglende anførselstegn i HTML-en blir ikke fanget av noe verktøy. Det går rett live når
PR-en merges. Se på siden i en nettleser hver eneste gang.

---

# Kommandokart — jukselapp

```bash
# --- Kjøre siden lokalt ---
open index.html                         # macOS · start index.html på Windows
python3 -m http.server 8000             # ekte server → http://localhost:8000 (Ctrl+C for å stoppe)
# etter hver endring: hard refresh i nettleseren, Cmd+Shift+R / Ctrl+F5

# --- Steg 0: hvor står vi? ---
git fetch origin                        # hent info fra GitHub (helt ufarlig)
git status                              # hva er endret? hvilken branch?
git branch --show-current               # bare branchnavnet
git log --oneline -5                    # de fem siste lagringspunktene

# --- Steg 1: ny branch ---
git checkout main && git pull origin main
git checkout -b text/kort-beskrivelse

# --- Steg 3–5: se, vis, lagre ---
open index.html                         # se på den
git diff                                # hva er faktisk endret?
git diff --stat                         # kortversjonen
git add index.html
git commit -m "Beskrivende melding på norsk"
git log --format='%h %G? %s' origin/main..HEAD    # signert? skal vise G

# --- Steg 6: send opp ---
git push -u origin text/kort-beskrivelse

# --- Steg 8: konflikt? ---
git fetch origin
git merge-tree --write-tree origin/main HEAD >/dev/null && echo OK || echo KONFLIKT
git rebase -S origin/main
git push --force-with-lease origin text/kort-beskrivelse

# --- Etter merge: rydd opp ---
git checkout main && git pull origin main
git branch -d text/kort-beskrivelse

# --- Angre ---
git stash                               # legg endringer i skuffen
git stash pop                           # hent dem ut igjen
git checkout -- index.html              # kast endringer i én fil
git commit --amend --no-edit -S         # fiks siste commit
git reset --soft HEAD~1                 # angre commit, behold endringene
git rebase --abort                      # kom deg ut av en rebase
git reflog                              # finn igjen noe du trodde var borte

# --- Nødbremsen: tilbake til versjonen som ligger live (spør brukeren først!) ---
git fetch origin
git checkout main
git reset --hard origin/main            # kast alle lokale endringer og commits
git clean -nd                           # vis hvilke ekstra filer som vil bli slettet
git clean -fd                           # slett dem
```
