# Tarkistus — tenttijärjestelmä

Työloki päätöksistä. Aloitettu vaiheessa 0, täydennetty vaiheessa 6. Poistetaan reposta ennen koeajoa; kopio jää kirjoittajalle.

## Oma kierros (ennen tekoälyä)

| # | Löytö | Tyyppi |
|---|-------|--------|
| 1 | `domain.md` sääntö 9 sanoi "use case", mutta perustelu puuttui | Paikka ilman perustelua |
| 2 | `arkkitehtuuri.md` Valmis-kohta: "järjestelmä toimii luotettavasti" | Sopisi mihin tahansa järjestelmään |
| 3 | Spec 001 kriteeri: "tulos näytetään oikein" | Ei voi todeta ajamalla |
| 4 | Mistä nyt-hetki tulee? Kuvaus ei sanonut — agentti käyttäisi `DateTime.Now`:ia Domainissa | Agentti joutuisi päättämään |

## Tekoälyn löydöt

**Vaihe 0** — prompti: "Mitkä tärkeät kysymykset puuttuvat?" Ehdotti kysymystä "pitääkö jokaiseen kysymykseen vastata ennen palautusta?". Ei ollut omalla listalla.

**Vaihe 2** — prompti: "Onko jokainen Paikka perusteltu?" Ehdotti sääntöä 8 (määräaika) use caseen, koska "määräaika riippuu tentistä".

**Vaihe 6, kuivaharjoitus** — prompti README:n vaiheesta 6, uusi sessio. Tekoäly sanoi joutuvansa arvaamaan:
- mistä kello tulee testeissä (sama kuin oma löytö 4)
- palautetaanko 409 vai 422, kun vastataan vieraaseen kysymykseen
- saako sama opiskelija aloittaa uudelleen, jos edellinen suoritus jäi palauttamatta

**Vaihe 6, kattavuus** — säännölle 5 (julkaistuun ei lisätä kysymyksiä) ei ollut hylkäyskriteeriä spec 002:ssa.

## Päätökset

| Löytö | Päätös | Miksi |
|-------|--------|-------|
| Oma 1: sääntö 9 ilman perustelua | Korjattu | Lisätty "tarvitsee sekä suorituksen että tentin kysymykset". |
| Oma 2: geneerinen Valmis-lause | Korjattu | Korvattu viidellä todettavalla lauseella. |
| Oma 3: "näytetään oikein" | Korjattu | Kriteeri 8: tulos 2/3 tietyillä vastauksilla. |
| Oma 4 + AI: kello | Korjattu | Kielto 4 `arkkitehtuuri.md`:hen ja ADR 0001:n viimeinen seuraus. |
| AI vaihe 0: vastattava kaikkiin? | Kysytään asiakkaalta | Ei ehditty → oletus `havainnot.md`:ssä. |
| AI vaihe 2: sääntö 8 use caseen | **Hylätty** | Määräaika lasketaan kerran ja tallennetaan suoritukseen (ADR 0001); sen jälkeen tarkistus tarvitsee vain suorituksen omaa dataa. Tekoäly ei tiennyt ADR:stä, koska sitä ei vielä ollut. |
| AI: 409 vai 422 | Korjattu | `rajapinta.md`:hen erillinen rivi: vieras kysymys on ainoa 422. |
| AI: uusi aloitus palauttamattoman jälkeen | Korjattu | Sääntö 7 täsmennetty: "yksi suoritus", palautettu tai ei. |
| AI: säännön 5 hylkäys puuttui | Korjattu | Spec 002 kriteeri 8. |
