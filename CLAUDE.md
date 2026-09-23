# Werkinstructies voor het bijwerken van deze wiki

Dit is een persoonlijke huisdieren- en terrariumwiki, gebouwd met Quartz 5 en gepubliceerd via
GitHub Pages. Deze instructies gelden voor elke LLM of agent die inhoud toevoegt of wijzigt.
Lees dit bestand voordat je iets in `content/` aanpast.

Dit bestand staat bewust in de repository-root en niet in `content/`, zodat het geen pagina op de
site wordt.

## Doel en publiek

De wiki is een naslagwerk, geen blog. Er wordt in gelezen om iets op te zoeken: welke waarden een
bak moet halen, wanneer een dier gevoerd is, wat er nodig is voordat een nieuwe soort in huis komt.

Twee lezers:

1. Sander zelf, meestal met een concrete vraag.
2. Iemand die de dieren tijdelijk verzorgt en snel moet kunnen vinden wat waar hoort.

Schrijf daarnaar: feitelijk, compact, scanbaar.

## Toon en opmaak

- Nederlands, zakelijk, in hele zinnen. Geen wervende of vertellende toon.
- Geen welkomstteksten, inleidingen of aankondigingen van wat er volgt. Begin bij de inhoud.
- Geen emoji-callouts (`> 💡`, `> 📝`, `> ⚠️`). Een waarschuwing is een gewone zin of een vetgedrukt
  label aan het begin van een alinea.
- Geen "Laatste update"-regel in de tekst. Quartz toont de datum automatisch op basis van git.
- Metrische eenheden in platte tekst: `26-30°C`, `60-80%`, `80 × 40 × 40 cm`. Geen LaTeX.
- Tabellen voor alles wat vergeleken wordt (klimaat, voeding, afmetingen). Lopende tekst voor
  uitleg en uitzonderingen.
- Emoji uitsluitend in de `title` in de frontmatter, één per pagina, als herkenningspunt in de
  zijbalk.

## Mappenstructuur

```
content/
├── index.md                              alleen een "Inhoud"-lijst, verder niets
├── Huidige Huisdieren/
│   ├── index.md                          overzichtstabel van alle groepen
│   └── <Soortgroep>.md                   één pagina per soortgroep
├── Toekomstige Huisdieren/
│   ├── index.md                          kandidaten met parameters en beslispunten
│   └── <Soort>.md                        eigen pagina zodra een soort serieus overwogen wordt
├── Voederkweek/
│   ├── index.md                          overzicht met status en afnemers
│   └── <Voederdier>.md
├── Handleidingen en Substraatrecepten/
│   └── index.md                          substraatrecepten en klimaattabel per bewoner
└── Seizoensschema en Onderhoudslog.md    terugkerend en seizoensgebonden onderhoud
```

`docs/` is de meegeleverde documentatie van Quartz zelf. Dat is geen wiki-inhoud: niet bewerken en
niet als voorbeeld voor de schrijfstijl gebruiken.

## Sjabloon voor een soortpagina

In deze volgorde, secties overslaan die niet van toepassing zijn:

1. `## Overzicht` — tabel met de dieren zelf: naam, soort, geslacht, geboortedatum, datum in huis,
   herkomst.
2. `## Wettelijke status` — alleen als er regelgeving speelt (houdverbod, overgangsregeling, CITES).
3. `## Klimaat` — tabel met temperatuur en luchtvochtigheid per soort, plus het nevel- of
   vochtregime. Bij meerdere soorten op één pagina: kolom per soort.
4. `## Verblijf / inrichting` — minimale afmetingen, substraat, inrichting, ventilatie.
5. `## Voeding` — tabel met wat, hoe vaak en hoeveel. Splits naar levensstadium waar dat uitmaakt
   (juveniel, volwassen, rond de vervelling).
6. `## Gedrag en hantering` — wat normaal gedrag is en wat niet, en waar bij het hanteren op gelet
   moet worden.
7. `## Bronnen` — zie hieronder.

## Feiten, aannames en verzinsels

Dit is de belangrijkste regel van dit bestand: **de wiki bevat geen verzonnen gegevens.**

- Namen, rassen, data, aantallen, leeftijden en meetwaarden komen van Sander of uit een
  geraadpleegde bron. Niets daarvan invullen op gevoel of "voor de volledigheid".
- Ontbrekende gegevens expliciet open laten, bijvoorbeeld "soortnaam nog vast te leggen" of een
  lege tabelcel. Een zichtbaar gat is bruikbaar; een plausibel verzinsel is schadelijk.
- Houd gescheiden wat wij doen en wat een caresheet adviseert. Gebruik koppen als
  "Huidige setup" tegenover "Naslag: kweektips uit caresheets" wanneer die twee uiteenlopen.
- Spreken Sanders opgaven elkaar tegen (bijvoorbeeld een leeftijd tegenover een geboortedatum),
  los dat niet stilzwijgend op. Kies de meest specifieke opgave, zet beide in het antwoord en
  vraag om bevestiging.
- Klimaat- en voedingswaarden alleen overnemen uit een bron die daadwerkelijk is opgehaald. Is er
  voor een soort of kleurvorm geen caresheet gevonden, zet dat dan op de pagina en benoem welke
  waarden als richtlijn zijn aangehouden.

## Bronnen

- Elke pagina met verzorgingsinformatie eindigt met `## Bronnen` en een lijst links in de vorm
  `[Titel — Site](url)`.
- Alleen bronnen opnemen die in dezelfde sessie zijn opgehaald en gelezen. Een link uit een
  zoekresultaat zonder de pagina te bekijken is niet genoeg.
- Voorkeursvolgorde: LICG en Nederlandse specialistensites, daarna gespecialiseerde caresheets,
  daarna algemene hobbysites.
- Webshop- of productpagina's mogen als verwijzing, maar presenteer ze niet als caresheet.

## Wikilinks

- Verwijs met `[[Paginanaam]]`; de resolutie staat op shortest path, dus het pad is niet nodig.
- Naar een mapindex verwijzen met `[[Map/index|Zichtbare tekst]]`.
- Gebruik **geen** escaped pipe (`\|`) binnen een tabelcel om een linktekst te zetten. Maak in
  plaats daarvan een aparte kolom met de kale link erin.
- Controleer na elke wijziging dat alle verwijzingen bestaan:

```bash
cd content && python3 - <<'PY'
import pathlib, re
files = sorted(pathlib.Path(".").rglob("*.md"))
stems = set()
for f in files:
    stems.add(str(f).replace("\\", "/")[:-3])
    stems.add(f.stem)
bad = [(str(f), m.split("|")[0].strip())
       for f in files
       for m in re.findall(r"\[\[([^\]]+)\]\]", f.read_text(encoding="utf-8"))
       if m.split("|")[0].strip() not in stems]
print("kapotte links:", bad or "geen")
PY
```

## Frontmatter

```yaml
---
title: 🐁 Stekelmuizen
tags: [huisdieren, knaagdieren]
---
```

Geen datumvelden toevoegen; die komen uit git. Tags kort houden en hergebruiken wat er al is.

## Bij een nieuw dier: welke bestanden bijwerken

1. De soortpagina in `content/Huidige Huisdieren/` — nieuw bestand, of een extra hoofdstuk op de
   bestaande pagina van die groep.
2. `content/Huidige Huisdieren/index.md` — rij in de overzichtstabel.
3. `content/index.md` — alleen als er een nieuwe groep bijkomt.
4. `content/Handleidingen en Substraatrecepten/index.md` — rij in de klimaattabel per bewoner, en
   een afwijkend substraatrecept als de soort dat vraagt.
5. `content/Seizoensschema en Onderhoudslog.md` — regels voor voeren, water en schoonmaken.
6. `content/Voederkweek/index.md` — kolom "Gaat naar" aanvullen als het dier voederdieren eet.
7. `content/Toekomstige Huisdieren/index.md` — het dier daar verwijderen als het uit de
   wensenlijst komt.

## Controle vóór het committen

- Linkcontrole uit het script hierboven: geen kapotte verwijzingen.
- `grep -rn "^> " content/` levert niets op (geen achtergebleven callouts).
- `grep -rn '\\|' content/` levert geen escaped pipes in tabellen op.
- Geen "Laatste update", geen welkomsttekst, geen leestijd-vermelding in de tekst.

## Git

- Werk rechtstreeks in de lokale clone en commit daar.
- **Niet pushen.** De agent-omgeving heeft geen GitHub-credentials; Sander pusht zelf met
  `git push origin main`. Sluit af met die opdracht en met wat er gepusht gaat worden.
- Commitberichten in het Engels, gebiedende wijs in de titel, met daaronder een toelichting op de
  inhoudelijke keuzes.
- Bij de melding "Another git process seems to be running": verwijder `.git/index.lock` en
  eventueel `.git/HEAD.lock`.

## Site-instellingen

Staan in `quartz.config.default.yaml`. Vastgelegde keuzes, niet ongevraagd terugdraaien:

- `pageTitle: 🐾 Huisdieren Wiki` — de naam in de zijbalk.
- `content-meta` met `showReadingTime: false` — geen leestijd onder de titel.
- `explorer` met `title: Inhoud`, `folderDefaultState: open` en `useSavedState: false` — de
  inhoudsopgave staat open en toont altijd de actuele structuur in plaats van een in de browser
  opgeslagen oude staat.
- De footer verwijst naar de eigen repository, niet naar het Quartz-project.
