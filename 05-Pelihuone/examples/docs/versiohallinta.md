# Versiohallinta — tenttijärjestelmä

## Haarat

- `master` on julkaistu linja. Siellä on vain se, mikä on käyty läpi.
- `development` on työlinja. Feature-haarat lähtevät siitä ja palaavat siihen.
- Feature-haara on nimeltään `feature/NNN-aihe`, sama numero kuin specillä: `feature/001-suorita-tentti`.
- Yhdistäminen aina pull requestilla: feature → development → master. Ei suoraa pushia pitkäikäisiin haaroihin.

## Spec ennen koodia

- Repon ensimmäinen commit sisältää vain `AGENTS.md`-, `docs/`- ja `specs/`-tiedostot. Koodia ei ole.
- Uusi ominaisuus alkaa uudella `specs/NNN-*.md`-tiedostolla, joka commitoidaan **ennen** ensimmäistä koodiriviä.
- Kuvauksen muutos ja koodin muutos ovat eri commiteissa. Jos sääntö muuttuu, `domain.md` muuttuu ensin.

## Commit

- Yksi asia per commit: yksi tehtävälistan askel on luonteva yksikkö.
- Viesti nimeää päätöksen, ei tiedostoa: "Suoritus hylkää toisen palautuksen" eikä "päivitetty Suoritus.cs".

## Ei repoon

- Tietokantatiedosto (`*.db`), käännöstulos (`bin/`, `obj/`), `.vs/`.
- Tokenit, salasanat, konekohtaiset asetukset.
- Ei force pushia mihinkään haaraan.

## Agentin lopetus

Sessio päättyy viestiin, joka kertoo haaran nimen, commitit ja sen, mitkä tarkistukset ajettiin ja mitä ne tulostivat. Sana "valmis" ei korvaa sitä, että `arkkitehtuuri.md`:n **Valmis**-kohdan lauseet pitävät.
