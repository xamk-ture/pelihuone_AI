# Versiohallinta — tenttijärjestelmä

Tämä tiedosto sanoo, miten repo syntyy, miten haarat ja commitit elävät ja miten agentin sessio päättyy. Roolit on kuvattu `docs/agentit.md`:ssä; tässä sanotaan, kuka saa tehdä minkäkin git-toimen.

## Haarat

| Haara | Mitä siellä on | Kuka luo | Kuka yhdistää siihen |
|---|---|---|---|
| `master` | Julkaistu linja. Vain se, mikä on käyty läpi ja jonka `arkkitehtuuri.md`:n **Valmis**-lauseet pitävät. | Ihminen alustuksessa | Ihminen, pull requestilla `development`-haarasta |
| `development` | Työlinja. Feature-haarat lähtevät siitä ja palaavat siihen. | Ihminen alustuksessa | Ihminen, pull requestilla feature-haarasta |
| `feature/NNN-aihe` | Yhden specin työ. Sama numero kuin specillä: `feature/001-suorita-tentti`. | Johtaja-agentti `development`-haarasta | — (poistetaan yhdistämisen jälkeen) |

- Yksi spec, yksi feature-haara.
- Pitkäikäisiin haaroihin ei pushata suoraan. Ei rebasea, ei force pushia mihinkään haaraan. Jos `development` on edennyt, se yhdistetään feature-haaraan (`git merge development`), ei toisin päin.

## Repon alustus

1. `docs/tarkistus.md` siirretään pois kansiosta; se on kirjoittajan työväline, ei osa kuvausta.
2. `git init -b master` kansiossa, jossa `AGENTS.md` on.
3. Ensimmäinen commit sisältää vain kuvauksen: `AGENTS.md`, `docs/`, `specs/`, `.gitignore` ja `.editorconfig`. Koodia ei ole.
4. `development` luodaan heti `master`-haarasta.
5. Remote annetaan erikseen. Ilman remotea ei pushata, ja johtaja sanoo sen lopetusviestissä.

## Spec ennen koodia

- Uusi ominaisuus alkaa uudella `specs/NNN-*.md`-tiedostolla, joka commitoidaan **ennen** ensimmäistä koodiriviä.
- Kuvauksen muutos ja koodin muutos ovat eri commiteissa. Jos sääntö muuttuu, järjestys on `domain.md` → spec → koodi, jokainen omana committina.
- Uusi rakennepäätös on uusi `docs/adr/NNNN-*.md`, ei muutos vanhaan.
- Työntekijä-agentti ei muuta `docs/`-kansiota koskaan; kuvauksen muutos on johtajan ja ihmisen asia.

## Askeleen työnkulku

1. Johtaja avaa feature-haaran `development`-haarasta tai jatkaa olemassa olevaa.
2. Johtaja antaa yhden specin **Tehtävät**-askeleen työntekijälle. Yksi askel on yksi commit.
3. Katselmoija lukee diffin ennen committia `domain.md`:n Paikka-saraketta, `arkkitehtuuri.md`:n kieltoja ja specin kriteerejä vasten. Löydöt korjataan ennen committia.
4. `dotnet build` ja `dotnet test` ovat vihreitä ennen committia. Punaista testiä ei commitoida.
5. Johtaja commitoi ja pushaa feature-haaran. Vain johtaja tekee git-toimia.
6. Kun specin kaikki askeleet ovat tehty ja hyväksymiskriteerit ajettu HTTP:llä, johtaja avaa pull requestin `development`-haaraan.

## Commit

- **Yksi asia per commit:** yksi tehtävälistan askel. Muotoilun siivous on oma commit.
- **Otsikko** nimeää päätöksen asiakkaan sanoin, ei tiedostoa: "Suoritus hylkää toisen palautuksen" eikä "päivitetty Suoritus.cs".
- **Runko** kertoo specin ja askeleen, säännöt, ajetut kriteerit ja testien tulosteen.
- Testi ja toteutus ovat samassa commitissa.

Esimerkki:

```
Suoritus hylkää toisen palautuksen

Spec 001, askel 1: Suoritus-entiteetti ja säännöt 1–6, 8, 10, 11 domain.md:n Paikan mukaan.
Nyt-hetki parametrina (ADR 0001).

Kriteerit 1, 2, 7, 9, 10 vihreitä Domain-testinä.
dotnet test: Passed 12, Failed 0.
```

## Pull request

- Yksi pull request per feature-haara. Otsikko on specin nimi: `001 Suorita tentti`.
- Runko: mitä tehtiin askeleittain, mitkä hyväksymiskriteerit ajettiin HTTP:llä ja mitä ne tulostivat, mitkä jäivät ajamatta ja miksi.
- **Ihminen yhdistää.** Agentti avaa pull requestin eikä yhdistä sitä, ei edes pyydettäessä samassa sessiossa.
- Yhdistetään merge-committina, ei squashina, jotta jokainen askel näkyy historiassa.

## Ei repoon

`.gitignore` vartioi näitä, mutta sääntö pätee, vaikka tiedosto olisi lisätty väkisin.

- Tietokanta ja käännöstulos: `*.db`, `bin/`, `obj/`, `.vs/`, `TestResults/`.
- Tokenit, salasanat, konekohtaiset asetukset (`appsettings.*.local.json`, `.env`).
- `docs/tarkistus.md`.
- Koodia, joka ei käänny, tai testiä, joka on punainen.

**Jos salaisuus pääsi repoon:** se vaihdetaan heti, koska poisto uudella commitilla jättää sen historiaan. Johtaja kertoo asian lopetusviestissä ensimmäisenä rivinä, ja ihminen päättää historian siivouksesta.

## Agentin lopetus

Sessio päättyy viestiin, jonka vain johtaja kirjoittaa. Sana "valmis" ei korvaa sitä, että `arkkitehtuuri.md`:n **Valmis**-kohdan lauseet pitävät. Viestissä on tässä järjestyksessä:

1. Haaran nimi ja se, onko haara pushattu (ja jos ei, miksi).
2. Commitit: hash ja otsikko.
3. Ajetut tarkistukset ja niiden tuloste: `dotnet build`, `dotnet test`, HTTP-kriteerit.
4. Kriteerit, jotka jäivät ajamatta, ja syy.
5. Katselmoijan löydöt, jotka jäivät korjaamatta, ja syy.
6. Avoimet kysymykset ihmiselle, viittauksella `domain.md`:n sääntöön tai `havainnot.md`:n Avoimet-kohtaan.
7. Jos askel jäi kesken: mihin kohtaan ja mikä kysymys avautui.
