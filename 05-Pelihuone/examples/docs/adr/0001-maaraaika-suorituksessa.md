# ADR 0001 — Määräaika tallennetaan suoritukseen, ei lasketa joka kerta

**Tila:** hyväksytty · **Päivä:** 2026-10-15

## Tilanne

Suorituksella on määräaika: aloitus + tentin kesto, mutta ei koskaan aikaikkunan lopun jälkeen (sääntö 8). Sitä tarvitaan jokaisessa vastauksessa ja palautuksessa (säännöt 10 ja 11). Kysymys on, **missä määräaika on olemassa**: lasketaanko se joka tarkistuksessa tentin tiedoista, vai lasketaanko kerran ja tallennetaan suoritukseen.

## Vaihtoehdot

1. **Lasketaan joka kerta.** Vastaa-use case hakee tentin, laskee määräajan ja vertaa kelloon. Suoritus ei tiedä määräaikaansa.
2. **Lasketaan kerran, tallennetaan suoritukseen.** Tentti laskee määräajan, kun suoritus aloitetaan; suoritus kantaa sen mukanaan ja tarkistaa itse säännöt 10 ja 11.
3. **Suoritus tuntee tentin** ja laskee itse. Vaatii, että suoritus pitää viittauksen tenttiin ja sen kestoon.

## Päätös

Vaihtoehto 2.

## Seuraukset

- Säännöt 10 ja 11 ovat **Suorituksen omaa dataa** → entiteetissä, ei use casessa. Ne voi testata ilman tenttiä ja ilman tallennusta: luo suoritus määräajalla, siirrä kelloa, palauta.
- Tentin keston muutos ei vaikuta käynnissä oleviin suorituksiin. Tämä on asiakkaan tahto ("ikkuna menee kiinni, piste") ja myös reilumpi: kaikki saivat sen ajan, joka heille aloitettaessa luvattiin.
- Suoritukseen tulee yksi kenttä lisää. Se on hinta.
- Vaihtoehto 1 olisi vienyt kolme sääntöä use caseen ja tehnyt jokaisesta vastauksesta tentin haun. Vaihtoehto 3 olisi sitonut suorituksen tenttiin tavalla, jota mikään muu sääntö ei tarvitse.
- Nyt-hetki annetaan parametrina kaikkialla, missä määräaikaa verrataan (kielto 4). Ilman sitä tätä päätöstä ei voisi testata siirtämättä konetta.
