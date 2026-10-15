# AGENTS.md — tenttijärjestelmä

Tämä repo on kuvaus ennen koodia. Lue ennen kuin kirjoitat mitään:

1. `docs/domain.md` — kieli, säännöt ja kenen vastuulla kukin sääntö on
2. `docs/arkkitehtuuri.md` — osat, riippuvuuden suunta, kiellot, ei-tavoitteet
3. `docs/rajapinta.md` — reitit, kentät sanoina, statuskoodit
4. `docs/versiohallinta.md` — haarat, commit, mitä ei repoon
5. `docs/adr/` — tehdyt rakennepäätökset
6. `docs/tilaus.md` ja `docs/havainnot.md` — asiakkaan sanat ja oletukset; näihin palataan, kun jokin tuntuu puuttuvan
7. Se `specs/NNN-*.md`, jota pyydetty muutos koskee, ja vain sen **Tehtävät**-osion pyydetty askel

## Ensin suunnitelma

Ennen kuin muutat yhtään tiedostoa, esitä suunnitelma pyydetylle askeleelle: mitkä tiedostot luot tai muutat, missä kerroksessa kukin sääntö tarkistetaan (viittaus `domain.md`:n riviin), mitkä testit kirjoitat ja miten valmius todetaan. Odota hyväksyntää.

## Älä

- Älä keksi käsitettä, projektia, reittiä tai haaraa, jota kuvaus ei nimeä.
- Älä aloita tutoriaalista äläkä yleisestä Clean Architecture -pohjasta. Projektit ovat `docs/arkkitehtuuri.md`:n neljä, ei enempää.
- Älä tee useampaa tehtävälistan askelta yhdellä kertaa, vaikka se tuntuisi tehokkaalta.
- Älä kutsu kelloa Domainissa. Aika annetaan parametrina (ks. ADR 0001).

## Pysähdy ja kysy, jos

- säännöllä ei ole Paikkaa `docs/domain.md`:ssä
- spec on ristiriidassa `docs/`-tiedostojen kanssa
- `docs/havainnot.md`:n **Avoimet**-kohta koskee tehtävää, jota teet
- joudut valitsemaan statuskoodin, kentän nimen tai kirjaston, jota `docs/rajapinta.md` tai `docs/arkkitehtuuri.md` ei mainitse

Arvaus näyttää aivan koodilta. Kysymys on halvempi.

## Tarkistukset

- Käännös: `dotnet build` repon juuressa, ei varoituksia väärästä projektiviittauksesta.
- Testit: `dotnet test` — Domain-testit eivät käynnistä web-palvelinta eivätkä avaa tietokantaa.
- Hyväksymiskriteerit: specin Given–When–Then-rivit ajetaan HTTP-kutsuina käynnissä olevaa APIa vasten. Ihminen ajaa ne; "valmis" ilman ajoa ei kelpaa.

## Definition of Done (sama joka muutokselle)

Kriteerit ajettu · diff luettu `domain.md`:n Paikka-saraketta vasten · jokaiselle säännölle testi · historia kertoo askeleet · ei tietokantatiedostoa, käännöstulosta eikä tokenia repossa · lopetusviesti nimeää haaran ja commitit.
