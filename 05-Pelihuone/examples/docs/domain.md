# Domain — tenttijärjestelmä

Taktinen DDD-kuvaus: sanasto, käsitteet, säännöt paikkoineen. Ei bounded contexteja, ei domain-eventtejä.

## Sanasto

Asiakkaan sanat. Sama sana koodissa.

| Sana | Koodissa | Yksi lause |
|------|----------|------------|
| **Tentti** | `Exam` | Opettajan luoma monivalintakoe, jolla on aikaikkuna ja kesto. |
| **Kysymys** | `Question` | Tentin yksi kohta: teksti ja 2–6 vaihtoehtoa, joista yksi on oikea. |
| **Vaihtoehto** | `Option` | Kysymyksen yksi vastausmahdollisuus, tunnistetaan järjestysnumerolla. |
| **Aikaikkuna** | `TimeWindow` | Väli, jonka sisällä suorituksen voi aloittaa ja jonka jälkeen mitään ei oteta vastaan. |
| **Suoritus** | `Attempt` | Yhden opiskelijan yksi yritys yhteen tenttiin: alkaa, kerää vastauksia, palautetaan kerran. |
| **Vastaus** | `Answer` | Suorituksen valinta yhteen kysymykseen. Uusi vastaus korvaa edellisen. |
| **Määräaika** | `Deadline` | Hetki, jonka jälkeen suoritukseen ei oteta vastausta eikä palautusta. |
| **Opiskelija** | `StudentNumber` | Opiskelijanumero. Ei käyttäjätiliä tässä versiossa. |
| **Tulos** | `Score` | Oikeiden vastausten määrä suhteessa kysymysten määrään. Lasketaan, ei tallenneta. |

Valinnat: asiakas sanoi sekä "koe" että "tentti" — valittiin **tentti**, koska se oli Moodlessakin. Tunnisteet ovat koodissa englanniksi kurssin tavan mukaan; sarake "Koodissa" on ainoa sallittu vastine (lisätty koeajon jälkeen, ks. `havainnot.md`).

## Käsitteet

Kentät sanoina.

- **Tentti:** nimi, aikaikkuna (alku ja loppu), kesto minuutteina, tila (luonnos tai julkaistu), kysymykset.
- **Kysymys:** teksti, vaihtoehdot järjestyksessä, oikean vaihtoehdon numero.
- **Suoritus:** tentti, opiskelija, aloitettu, määräaika, palautettu (voi puuttua), vastaukset.
- **Vastaus:** kysymys, valittu vaihtoehto.

## Invariantit

Paikan ratkaisee kysymys: *mitä tietoa tarkistus tarvitsee?* Yhden olion oma data → entiteetti tai arvo. Haku tai toinen olio → use case.

| # | Sääntö | Paikka | Miksi juuri siinä |
|---|--------|--------|-------------------|
| 1 | Aikaikkunan alku on ennen loppua | **Aikaikkuna** (arvo) | Kaksi kellonaikaa, joilla on yhdessä sääntö. Virheellistä ikkunaa ei voi edes luoda. |
| 2 | Kesto on suurempi kuin nolla | **Tentti** | Tentin oma kenttä. |
| 3 | Kysymyksellä on 2–6 vaihtoehtoa ja oikea on yksi niistä | **Kysymys** | Kysymyksen oma data. Kysymystä ei synny ilman tätä. |
| 4 | Julkaistavalla tentillä on vähintään yksi kysymys | **Tentti** | Tentti tuntee omat kysymyksensä. |
| 5 | Julkaistuun tenttiin ei lisätä eikä poisteta kysymyksiä | **Tentti** | Tila on tentin oma kenttä. |
| 6 | Suorituksen voi aloittaa vain julkaistuun tenttiin, aikaikkunan sisällä | **Tentti** | Tentin tila ja ikkuna ovat sen omaa dataa; *nyt-hetki annetaan parametrina*, jotta sääntöä voi testata ilman kelloa. |
| 7 | Yksi suoritus per opiskelija per tentti | **Use case** (aloita suoritus) | Vaatii haun: onko tällä opiskelijalla jo suoritus tähän tenttiin. Yksi olio ei voi tietää sitä. |
| 8 | Määräaika on aloitus + kesto, mutta ei koskaan aikaikkunan lopun jälkeen | **Tentti** laskee, **Suoritus** tallentaa | Tentti tuntee keston ja ikkunan; suoritus saa valmiin määräajan syntyessään. Ks. ADR 0001. |
| 9 | Vastaus kohdistuu tentin omaan kysymykseen ja olemassa olevaan vaihtoehtoon | **Use case** (vastaa) | Tarvitsee sekä suorituksen että tentin kysymykset — kaksi oliota. |
| 10 | Vastausta ei oteta vastaan palautuksen jälkeen eikä määräajan jälkeen | **Suoritus** | Palautettu-kenttä ja määräaika ovat suorituksen omaa dataa. |
| 11 | Palautus tehdään vain kerran ja vain ennen määräaikaa | **Suoritus** | Sama kuin edellä. Tämä on järjestelmän "tulos vain kerran". |

Säännöt 7 ja 9 ovat ainoat use casessa. Kaikki muut voi tarkistaa yhden olion omasta datasta — ja silloin niitä ei voi ohittaa tulemalla toisesta suunnasta.

## Arvo vai luku

- **Aikaikkuna on arvo** (value object): kaksi kellonaikaa, joilla on yhteinen sääntö, eikä omaa identiteettiä. Kaksi samanlaista ikkunaa ovat sama ikkuna.
- **Vaihtoehdon numero jää luvuksi.** Sillä ei ole omaa sääntöä irrallaan kysymyksestä; sääntö 3 asuu Kysymyksessä.
- **Opiskelijanumero jää merkkijonoksi.** Ei kirjautumista, ei muotosääntöä tässä versiossa. Jos muoto tulee myöhemmin, siitä tulee arvo silloin.
- **Määräaika jää kellonajaksi.** Se on laskettu kerran (sääntö 8), ei itsenäinen käsite.

## Laskettu näkymä: Tulos

Tulosta **ei tallenneta**. Se lasketaan suorituksen vastauksista ja tentin oikeista vaihtoehdoista aina, kun sitä kysytään. Syy on sama kuin LeagueHubin sarjataulukolla: tallennettu kopio voi erota lähteestään, laskettu ei. Jos oikea vaihtoehto joskus korjataan (ei-tavoite nyt), tulos korjaantuu ilman uudelleenlaskentaa.

## Käyttäytymiset

| Tekeminen | Kuka aloittaa | Sitovat säännöt | Mitä se ei tee |
|-----------|---------------|-----------------|----------------|
| Luo tentti | Opettaja | 1, 2 | Ei julkaise. Syntyy luonnoksena. |
| Lisää kysymys | Opettaja | 3, 5 | Ei järjestä uudelleen, ei muokkaa olemassa olevaa. |
| Julkaise tentti | Opettaja | 4 | Ei lähetä ilmoitusta. |
| Aloita suoritus | Opiskelija | 6, 7, 8 | Ei näytä oikeita vaihtoehtoja. |
| Vastaa | Opiskelija | 9, 10 | Ei kerro, oliko vastaus oikein. |
| Palauta | Opiskelija | 11 | Ei laske tulosta talteen. |
| Katso tulos | Opiskelija | — (vain palautettu suoritus) | Ei näytä, mitkä kysymykset menivät oikein. |

## Yksi päätös

"Palautus vain kerran" on **Suorituksessa**, koska palautettu-kenttä on suorituksen omaa dataa — mikään reitti ei pääse palauttamaan kahdesti, tuli se mistä suunnasta tahansa. "Yksi suoritus per opiskelija" on **use casessa**, koska se vaatii haun kaikista suorituksista; yksi suoritus ei tiedä toisista. Molemmat voisi laittaa controlleriin, ja silloin seuraava reitti unohtaisi ne.
