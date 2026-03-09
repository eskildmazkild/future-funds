# CLAUDE.md — Projekttemplate
# ============================================================
# Denne fil læses af Claude Code ved starten af hver session.
# Hold den kort og præcis. Fjern alt Claude allerede ved.
# Tommelfingerregel: Hvis Claude ville gøre det alligevel, slet linjen.
# ============================================================


## Projektbeskrivelse
<!-- Udfyld kort: hvad er dette projekt, hvad løser det, for hvem? -->
[BESKRIV PROJEKTET HER — 2-3 linjer]


## Workflow

Følg altid denne rækkefølge. Hop aldrig direkte til implementering.

### 1. Explore (Plan Mode)
Læs relevante filer og forstå konteksten *inden* der skrives kode.
Brug Plan Mode (`Shift+Tab` for at skifte) til research-fasen.

### 2. Spec
Skriv eller opdater spec-dokumentet i `/specs/` inden implementering.
Spec skal indeholde: krav, acceptkriterier, teknisk design, edge cases.
**Vent på menneskelig godkendelse af spec før du går videre.**

### 3. Implement
Skift til Normal Mode. Følg spec'en. Fraveg kun spec hvis du støder på
en teknisk bloker — noter det i spec-filen og spørg.

### 4. Verify
Kør tests og tjek at acceptkriterierne i spec'en er opfyldt.
Tag screenshot ved UI-ændringer og sammenlign med forventet output.

### 5. Commit
Commit med en beskrivende besked der refererer til spec/feature.
Format: `type(scope): beskrivelse` — fx `feat(auth): add OAuth login`


## Spec-struktur

Nye specs gemmes i `/specs/[feature-navn].md` med dette format:

```
## Krav
Hvad skal bygges og hvorfor.

## Acceptkriterier
- [ ] Konkret, målbart kriterie
- [ ] Konkret, målbart kriterie

## Teknisk design
Arkitektur, datamodeller, interfaces, dependencies.

## Edge cases
Hvad kan gå galt, og hvordan håndteres det.

## Opgaver
- [ ] Atomare implementeringsopgaver
```


## UI og Design System

- Brug **shadcn/ui** til alle UI-komponenter. Tilføj nye komponenter med CLI:
  `npx shadcn@latest add [komponent]` — aldrig skriv en komponent fra bunden som shadcn allerede har.
- Brug **Tailwind CSS** til al styling. Ingen separate CSS-filer, CSS modules eller styled-components.
- **Radix UI** er inkluderet via shadcn — brug Radix primitives til accessibility og headless adfærd.
- Installer shadcn MCP for at Claude kan slå komponent-docs op i realtid: `claude mcp add shadcn`

### Regler
- Stil skrives som Tailwind utility classes direkte på elementet, ikke i separate filer.
- Brug shadcn's eksisterende farvevariabler (`bg-primary`, `text-muted-foreground` osv.) — definer ikke egne farver.
- Tilpas shadcn-komponenter ved at redigere filen i `/components/ui/` — ikke ved at override med inline styles.
- Brug aldrig `!important`.


## Kodestil
<!-- Tilpas til dit projekt -->
- Sprog: [fx TypeScript / Python / Go]
- Formattering: [fx Prettier med 2 spaces / Black / gofmt]
- Navngivning: [fx camelCase for funktioner, PascalCase for klasser]
- Imports: [fx ES modules, ikke CommonJS]
- Kommentarer: Skriv kun kommentarer der forklarer *hvorfor*, ikke *hvad*


## Test
<!-- Tilpas til dit testsetup -->
- Testframework: [fx Jest / pytest / Go test]
- Kør enkelttest: [fx `npm test -- --testNamePattern="navn"`]
- Kør alle tests: [fx `npm test`]
- Skriv altid tests for ny funktionalitet inden commit
- Kør tests efter hver implementeringsrunde og fix fejl før du går videre


## Vigtige kommandoer
<!-- Tilpas — inkluder kun kommandoer Claude ikke kan gætte -->
```bash
# [fx Start dev server]
# npm run dev

# [fx Byg til produktion]
# npm run build

# [fx Kør linter]
# npm run lint
```


## Git-konventioner
- Branch-navngivning: `feat/`, `fix/`, `chore/` + kort beskrivelse
- Commit-format: `type(scope): beskrivelse` (Conventional Commits)
- Opret aldrig PR uden at tests er grønne
- Commit atomart — én logisk ændring per commit


## Arkitekturprincipper

### Simpelhed frem for smarthed
- Skriv den kedeligste kode der løser problemet. Clever kode er svær at ændre.
- Undgå abstraktion indtil du har set det samme mønster mindst tre gange.
- En lang, læsbar funktion er bedre end fem korte med kryptiske navne.

### Filer og moduler
- Én fil = ét ansvar. Hvis du er i tvivl om hvad en fil "er", er den for stor.
- Max ~300 linjer per fil. Overskrides det, skal filen sandsynligvis deles op.
- Navngiv filer efter hvad de gør, ikke hvad de er: `createUser.ts` > `userService.ts`.
- Ingen cirkulære dependencies. Hvis A importerer B og B importerer A, er noget galt.

### Funktioner og logik
- Én funktion = én ting. Hvis du skal bruge "og" til at beskrive hvad den gør, del den.
- Eksplicitte parametre frem for global state. Funktioner skal kunne forstås isoleret.
- Returner tidligt ved fejl (guard clauses). Undgå dybt indlejret if/else.
- Undgå side effects i funktioner der ser ud som de bare beregner noget.

### Dependencies og lag
- Forretningslogik må ikke kende til database, HTTP eller UI — kun til data og regler.
- Dependencies peger indad, aldrig udad: UI → Logic → Data, aldrig omvendt.
- Injicer dependencies frem for at instansiere dem inde i funktioner.
- Tilføj ikke et bibliotek til noget sproget kan selv.

### Navngivning
- Navne skal forklare hensigt, ikke implementation: `getUserById` > `dbQuery`.
- Booleans starter med `is`, `has`, eller `can`: `isActive`, `hasPermission`.
- Undgå forkortelser undtagen brede standarder (`id`, `url`, `api`).

### Agent-venlige konventioner
<!-- Disse principper gør det lettere for en agent at navigere og ændre koden -->
- Hold relateret kode tæt på hinanden — læs-lokalitet er vigtigere end DRY.
- Skriv kommentarer der forklarer *hvorfor* en beslutning er truffet, ikke hvad koden gør.
- Fejlhåndtering skal være eksplicit og konsistent — brug aldrig silent failures.
- Undgå magi: ingen dynamiske imports, metaprogramming eller monkey-patching uden god grund.

<!-- Tilføj projektspecifikke principper herunder: -->
<!-- fx "Brug repository-pattern til dataaccess, aldrig direkte DB-kald i controllers" -->


## Mappestruktur
<!-- Tilpas til dit projekt — behold kun toplevel, ikke dybere -->
```
/
├── specs/          ← Spec-dokumenter for hver feature
├── src/
│   ├── app/        ← [fx Next.js routes / app entry points]
│   ├── components/ ← Genbrugelige UI-komponenter
│   │   └── ui/     ← shadcn-komponenter (redigeres direkte her)
│   ├── features/   ← Feature-moduler (logik + komponenter samlet per feature)
│   ├── lib/        ← Delte utilities og hjælpefunktioner
│   └── types/      ← Delte TypeScript-typer og interfaces
├── tests/          ← [fx e2e tests / integrationstests]
└── CLAUDE.md
```
Placer ny kode i `/features/[feature-navn]/` som default.
Flyt kun til `/components/` eller `/lib/` når noget bruges i mindst to features.


## Hvornår skal du stoppe og spørge

Stop altid og spørg — vent ikke med at fortsætte — hvis:
- Spec mangler eller er uklar på et punkt der kræver en designbeslutning
- En opgave berører mere end 3 filer du ikke har læst endnu
- Du er ved at installere en ny dependency
- Du opdager at implementeringen kræver en arkitekturændring
- Tests fejler og du ikke forstår hvorfor efter ét forsøg
- Du er i tvivl om en edge case der ikke er beskrevet i spec'en

Fortsæt uden at spørge hvis:
- Opgaven er klart defineret i spec'en og berører kendte filer
- Fejlen er åbenlys og rettelsen er lokal (< 10 linjer)
- Du blot tilføjer en shadcn-komponent eller Tailwind-klasse


## Kendte gotchas
<!-- Tilføj løbende når du opdager non-obvious ting -->
<!-- fx: "Kald altid flushSession() efter db-writes, ellers går integration tests ned" -->


## Hvad du IKKE må
- Ændre filer i `/migrations` uden eksplicit instruktion
- Slette eller overskrive eksisterende tests
- Installere nye dependencies uden at spørge først
- Skippe verify-fasen selvom implementeringen virker åbenlyst korrekt


# ============================================================
# VEDLIGEHOLDELSE:
# Opdater denne fil når du opdager at Claude gentagne gange
# gør noget forkert (tilføj regel), eller spørger om noget
# der allerede er besvaret her (tjek at formuleringen er klar).
# Fjern linjer der ikke ændrer Claudes adfærd — kortere er bedre.
# ============================================================
