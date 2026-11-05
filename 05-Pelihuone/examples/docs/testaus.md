# Testaus — tenttijärjestelmä

`docs/arkkitehtuuri.md` sanoo, mitä testejä on ja millä tasolla. Specit sanovat, mitkä kriteerit todistavat minkäkin säännön. Tämä tiedosto sanoo loput: millä aineistolla testit ajetaan, miten kello asetetaan käynnissä olevaan ohjelmaan, miten ihminen ajaa hyväksymiskriteerit, mikä sääntö testataan missä, ja kuka ajaa mitäkin milloin.

## Testiaineisto

Yksi lähtötila kaikille tasoille ja molemmille speceille. Specin Given-osa kuvaa vain sen, mikä tästä poikkeaa.

| Mitä | Arvo |
|---|---|
| Tentti T | julkaistu, aikaikkuna 8.00–20.00, kesto 60 min, kolme kysymystä (K1–K3), jokaisessa neljä vaihtoehtoa, oikeat 2, 1, 4 |
| Tentti L | luonnos, ei kysymyksiä |
| Käyttäjät | opiskelija `123`, opiskelija `456`, opettaja `500` |
| Kiinteä nyt | **10.00** samana päivänä kuin T:n ikkuna, ellei kriteeri toisin sano |
| Tila ennen kriteeriä | ei suorituksia |

Aineisto syntyy kullakin tasolla samasta kuvauksesta: Domain-testit `new`:llä ja tehdasmetodeilla; use case -testit `Fakes/TestExam.Default()`-apurilla, joka täyttää muistitallennuksen ja asettaa kiinteän kellon; API-testit ja ihmisen ajo opettajan reiteillä (`POST tentit`, `POST tentit/{id}/kysymykset`, `POST tentit/{id}/julkaise`), ei tietokantaseedillä, koska käynnistys ei lisää demodataa.

## Kiinteä kello käynnissä olevassa ohjelmassa

Domain ja Application eivät kutsu kelloa (ADR 0001). Testissä kello on `FixedClock`. Käynnissä olevassa ohjelmassa kello asetetaan kehitysympäristön asetuksella `Clock:FixedNow` (`appsettings.Development.json`, ISO-8601). Kun asetus on annettu ja ympäristö on Development, `IClock`-toteutus on `FixedClock`; muissa ympäristöissä asetus ohitetaan ja käynnistys lokittaa varoituksen. Kellon siirto on uusi arvo ja uudelleenkäynnistys.

## Hyväksymiskriteerien ajo

Kriteeri on testi: API-testiprojektissa yksi testiluokka per spec ja yksi testi per kriteeri nimellä `Spec001_K02_...`.

```
dotnet test tests/Api.Tests --filter "FullyQualifiedName~Spec001"
→  Passed! - Failed: 0, Passed: 11, Skipped: 0, Total: 11
```

Ihmisen käsiajo käyttää samoja pyyntöjä: `tests/http/000-lahtotila.http` (tentti T ja L opettajana), `tests/http/001-suorita-tentti.http` ja `tests/http/002-luo-tentti.http`, yksi pyyntö per kriteeri kommentilla `# K3 → 409 opiskelijalla on jo suoritus`, ryhmiteltynä kellon arvon mukaan. Ajojärjestys 000 → 002 → 001, koska suoritus tarvitsee julkaistun tentin. Tulos kirjataan pull requestin runkoon: spec, kriteerit, kellon arvot, `dotnet test` -rivi.

## Kattavuusmatriisi

| Sääntö | Paikka (taso) | Spec ja kriteerit | Hylkäys |
|---|---|---|---|
| 1 Aikaikkuna | arvo (Domain) | 002 | on |
| 2 Kesto > 0 | Tentti (Domain) | 002 | on |
| 3 Vaihtoehdot 2–6 | Kysymys (Domain) | 002 | on |
| 4 Julkaisu vaatii kysymyksen | Tentti (Domain) | 002 | on |
| 5 Julkaistua ei muuteta | Tentti (Domain) | 002 | on |
| 6 Aloitus vain julkaistuun, ikkunassa | Tentti (Domain) | 001: 1, 4 | on (4) |
| 7 Yksi suoritus per opiskelija | use case | 001: 3 | on (3) |
| 8 Määräaika | Tentti laskee, Suoritus tallentaa (Domain) | 001: 1, 2 | läpimeno kahdella arvolla |
| 9 Vastaus omaan kysymykseen | use case | 001: 5, 6 | on (6) |
| 10 Ei vastausta palautuksen tai määräajan jälkeen | Suoritus (Domain) | 001: 7, 10 | on (7, 10) |
| 11 Palautus kerran, ennen määräaikaa | Suoritus (Domain) | 001: 8, 9 | on (9) |
| Tulos laskettuna | use case | 001: 8 | — |
| Oikea vaihtoehto ei vuoda | API | 001: 11 | — |

Jokaisella säännöllä on rivi. Katselmoija lukee diffin tätä vasten: askeleen sääntö, jolla ei ole riviä, on pysähtymisen paikka.

## Milloin ja kuka

| Milloin | Kuka | Mitä ajetaan | Tuloste menee |
|---|---|---|---|
| Askeleen aikana | työntekijä | oman kerroksen testiprojekti | paluuviestiin johtajalle |
| Ennen committia | katselmoija | diff matriisia, Paikkaa ja kieltoja vasten | löytölistaan |
| Ennen committia | johtaja | `dotnet build`, `dotnet test` koko solutionille | commit-viestin runkoon |
| Ennen pull requestin yhdistämistä | ihminen | specin kriteerit suodattimella ja `.http`-tiedostolla, kello asetettuna | pull requestin runkoon |
| Ennen `master`-yhdistystä | ihminen | `arkkitehtuuri.md`:n Valmis-lauseet rivi riviltä | pull requestin runkoon |

## Mitä ei testata

- Suorituskykyä tai kuormaa.
- Selainta; sitä ei ole.
- Tenttien arkistointia tai poistamista; ne eivät ole tämän kuvauksen tavoitteita.
