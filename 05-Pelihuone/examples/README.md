# Esimerkki: tenttijärjestelmän kuvaus agentille

Tämä kansio on [tehtävän](../README.md) malli. Siinä on sama tiedostosarja, jonka kirjoitat pelihuoneelle — mutta **toisesta järjestelmästä**: opettaja luo monivalintatentin, opiskelija suorittaa sen määräajassa, tulos lasketaan vastauksista.

Aihe on tahallaan eri. Tenttijärjestelmän sääntö "palautus vain kerran" ei ole pelihuoneen sääntö, vaikka se asuu samassa kerroksessa. Lainaa **rakenne, otsikot ja tapa perustella** — älä lauseita.

## Tiedostot ja tehtävän vaiheet

| Vaihe | Tiedosto | Mitä siitä katsotaan |
|-------|----------|----------------------|
| 0 Luokka | [docs/tilaus.md](docs/tilaus.md) | Millaisia kysymyksiä asiakkaalle kannattaa esittää; vastaukset sanatarkasti; mikä jäi havainnoitavaksi |
| 1 Huone | [docs/havainnot.md](docs/havainnot.md) | Mitä nähtiin; ristiriita tilauksen kanssa; avoimet kohdat merkitty oletuksiksi |
| 2 Sanasto | [docs/domain.md](docs/domain.md) | Jokainen sääntö omalla rivillään Paikan ja syyn kanssa; VO-päätös; laskettu näkymä |
| 3 Tarinat | [specs/001-suorita-tentti.md](specs/001-suorita-tentti.md), [specs/002-luo-tentti.md](specs/002-luo-tentti.md) | Given–When–Then, joista osa hylkäyksiä; ei-tavoitteet; tehtävälista |
| 4 Rakenne | [docs/arkkitehtuuri.md](docs/arkkitehtuuri.md), [docs/rajapinta.md](docs/rajapinta.md), [docs/adr/0001-maaraaika-suorituksessa.md](docs/adr/0001-maaraaika-suorituksessa.md), [docs/testaus.md](docs/testaus.md) | Kiellot konkreettisina; kentät sanoina; yksi päätös vaihtoehtoineen; yksi testiaineisto, kello ajossa, kattavuusmatriisi |
| 5 Ohje | [AGENTS.md](AGENTS.md), [docs/agentit.md](docs/agentit.md), [docs/versiohallinta.md](docs/versiohallinta.md) — tässä järjestyksessä | Lukujärjestys, roolit, pysähtymissääntö, tarkistuskomennot, Definition of Done; kuka tekee, kuka tarkistaa, kuka commitoi, kuka yhdistää |
| 6 Tarkistus | [docs/tarkistus.md](docs/tarkistus.md) | Oma kierros ensin, sitten tekoälyn löydöt; jokaisesta päätös ja syy — myös hylätty ehdotus |
| 7 Koeajo | [docs/havainnot.md → Koeajo](docs/havainnot.md#koeajo) | Kolme lausetta: missä kysyi, missä arvasi, mikä lause puuttui — ja mitä kuvaukseen lisättiin |
| Kertaus | [teoriakysymykset.md](teoriakysymykset.md) | Vastaukset tehtävän kertauskysymyksiin |

## Miten tätä käytetään tunnilla

Opiskelija avaa vaiheen tiedoston tästä kansiosta **samalla kun** kirjoittaa pelihuoneen versiota — malli on vieressä, ei edessä. Kun kaikki ovat yrittäneet, opettaja kokoaa luokan version pelihuoneesta: "mitä kaikkea tähän tulisi?" Tämän kansion tiedostoa ei silloin näytetä uudestaan; sen tehtävä on antaa otsikot ja perustelun muoto, ei vastauksia.

Jos joku on kopioinut lauseen tästä kansiosta, se paljastuu luokan versiossa: lause sopii tenttiin, ei huoneeseen.

Koeajo (vaihe 7) kannattaa näyttää ensin tällä esimerkillä: tyhjä repo, nämä tiedostot ensimmäisenä committina, yksi prompti, `specs/001`:n tehtävälistan askel 1. Luokka katsoo, kysyykö agentti, antaako johtaja askeleen Domain-työntekijälle vai tekeekö sen itse, lukeeko katselmoija diffin, ja mihin kerrokseen "palautus vain kerran" päätyy.

## Mitä tässä ei ole

Ei koodia, ei JSONia, ei valmista solutionia. Se on tarkoitus: kuvaus riittää, ja koeajo todistaa sen.
