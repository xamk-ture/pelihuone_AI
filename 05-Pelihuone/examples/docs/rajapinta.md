# Rajapinta — tenttijärjestelmä

## Versio

Kaikki reitit polun `api/v1` alla. Rikkova muutos (kenttä katoaa tai vaihtaa nimeä) on uusi versio, ei hiljainen muutos.

## Reitit

| Verbi ja polku | Tekeminen | Kuka |
|----------------|-----------|------|
| POST `tentit` | Luo tentti luonnoksena | Opettaja |
| POST `tentit/{id}/kysymykset` | Lisää kysymys luonnokseen | Opettaja |
| POST `tentit/{id}/julkaise` | Julkaise tentti | Opettaja |
| GET `tentit` | Listaa tentit sivuina | Kaikki |
| GET `tentit/{id}` | Yksi tentti ilman oikeita vaihtoehtoja | Kaikki |
| POST `suoritukset` | Aloita suoritus (tentti, opiskelija) | Opiskelija |
| POST `suoritukset/{id}/vastaukset` | Vastaa yhteen kysymykseen | Opiskelija |
| POST `suoritukset/{id}/palauta` | Palauta suoritus | Opiskelija |
| GET `suoritukset/{id}/tulos` | Tulos palautetusta suorituksesta | Opiskelija |

Roolia ei tarkisteta (ei kirjautumista). Sarake kertoo, kenelle reitti on tarkoitettu, jotta myöhempi auth tietää mihin tarttua.

## Kentät sanoina

Vastauksessa on nämä kentät, **ei muita**. Domain-oliota ei palauteta sellaisenaan.

| Reitti | Sisään | Ulos |
|--------|--------|------|
| Luo tentti | nimi, ikkunan alku, ikkunan loppu, kesto minuutteina | tunniste, nimi, ikkuna, kesto, tila |
| Lisää kysymys | teksti, vaihtoehdot listana, oikean numero | kysymyksen tunniste, järjestysnumero |
| Julkaise | — | tunniste, tila |
| Listaa tentit | sivu, sivukoko | sivu, sivukoko, kokonaismäärä, tentit (tunniste, nimi, ikkuna, tila) |
| Yksi tentti | — | tunniste, nimi, ikkuna, kesto, tila, kysymykset (tunniste, teksti, vaihtoehdot) — **ei oikeaa numeroa** |
| Aloita suoritus | tentin tunniste, opiskelijanumero | suorituksen tunniste, aloitettu, määräaika |
| Vastaa | kysymyksen tunniste, vaihtoehdon numero | suorituksen tunniste, vastattujen määrä |
| Palauta | — | suorituksen tunniste, palautettu |
| Tulos | — | pisteet, maksimi |

Tulos ei kerro kysymyksittäin, mikä meni oikein (asiakkaan vaatimus, `havainnot.md`).

## Virheet

| Tilanne | Koodi | Miksi |
|---------|-------|-------|
| Puuttuva tai väärän tyyppinen kenttä | 400 | Muotovirhe tarkistetaan rajalla ennen use casea. |
| Tenttiä tai suoritusta ei ole | 404 | Tunniste ei osu. |
| Sääntö hylkää (toinen palautus, ikkuna kiinni, luonnos, määräaika ohi, toinen suoritus samalle opiskelijalle) | 409 | Pyyntö oli muodoltaan oikea, mutta tila ei salli sitä. Valittiin 409 eikä 422, koska kyse on aina tilasta, ei sisällöstä. |
| Vastaus kysymykseen, joka ei kuulu tenttiin, tai vaihtoehtoon jota ei ole | 422 | Sisältö on väärä, tila ei. Ainoa 422 tässä versiossa. |

Sääntövirhe on **paluuarvo** use casesta, ei poikkeus. Rajalla se muuttuu koodiksi ja viestiksi, joka nimeää rikotun säännön asiakkaan sanoin ("suoritus on jo palautettu"). Poikkeus jää yllätyksille: tietokanta ei vastaa.

## Paginaatio

Tenttilista palautetaan sivuina: sivu, sivukoko, kokonaismäärä. Oletus 20, enintään 100. Yksittäisen tentin kysymyksiä ei sivuteta: niitä on enintään kymmeniä ja ne kuuluvat yhteen.
