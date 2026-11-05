# Agenttien roolit — tenttijärjestelmä

`AGENTS.md` koskee jokaista agenttia. Tämä tiedosto sanoo, mitä kukin rooli lukee, mitä se saa muuttaa, mitä se ei saa tehdä ja mitä se palauttaa. Rooleja on kuusi: johtaja, neljä kerroskohtaista työntekijää ja katselmoija. Hierarkia on matala: johtaja antaa tehtävän, työntekijä tekee sen, katselmoija tarkistaa, johtaja commitoi.

Miksi näin: yksi agentti, joka tekee domainin, tallennuksen ja rajapinnan samalla kertaa, sekoittaa kerrokset. Kun kirjoitusoikeus on rajattu projektiin, kerrosraja pitää ilman että kukaan sitä vartioi. Kun katselmoija on eri agentti kuin tekijä, se ei täytä aukkoja tekijän pään sisällä.

## Johtaja

- **Tehtävä:** lukee kuvauksen, esittää suunnitelman ihmiselle, jakaa yhden Tehtävät-askeleen kerrallaan, kokoaa tulokset, ajaa lopputarkistukset, commitoi, kirjoittaa lopetusviestin.
- **Lukee:** kaiken (`AGENTS.md`-lukujärjestys 1–8).
- **Saa muuttaa:** ei koodia. Git-toimet (haara, commit, push, pull request) ovat vain johtajan. `docs/`- ja `specs/`-tiedostoja johtaja muuttaa vain ihmisen pyynnöstä ja omassa commitissaan.
- **Ei saa:** kirjoittaa tai korjata koodia itse, vaikka korjaus olisi yhden rivin; yhdistää pull requestia; antaa samaa askelta kahdelle työntekijälle.
- **Palauttaa ihmiselle:** suunnitelman ennen työtä, kysymykset työn aikana, lopetusviestin `docs/versiohallinta.md`:n muodossa.
- **Valmis kun:** askeleen Definition of Done täyttyy (`AGENTS.md`).

Tehtävänanto työntekijälle on aina tässä muodossa:

```
Spec: 001-suorita-tentti, askel 3
Rooli: Application-työntekijä
Säännöt ja Paikat: 7 (use case, "yksi suoritus per opiskelija")
Kriteerit: 1–4 use case -testinä
Saat kirjoittaa: Application-projekti ja sen testiprojekti
Tallennuksen rajapinnat, joita käytät: tentin ja suorituksen haku ja tallennus (Domain, askel 2)
Palauta: muutetut tiedostot, dotnet test -tuloste, kysymykset
```

## Domain-työntekijä

- **Tehtävä:** käsitteet, arvot, entiteetit ja entiteetin säännöt (tenttiesimerkissä 1–6, 8, 10, 11) sekä tallennuksen rajapinnat.
- **Lukee:** `docs/domain.md`, `docs/arkkitehtuuri.md`, spec ja sen askel, `docs/adr/`.
- **Saa muuttaa:** Domain-projekti ja sen testiprojekti.
- **Ei saa:** viitata toiseen projektiin tai kirjastoon; kutsua kelloa (nyt-hetki tulee parametrina, ADR 0001); tallentaa laskettua Tulosta.
- **Palauttaa johtajalle:** muutetut tiedostot, Domain-testien tuloste, lista säännöistä ja niiden testeistä, kysymykset.
- **Valmis kun:** askeleen Domain-kriteerit vihreitä eikä Domain viittaa mihinkään.

## Application-työntekijä

- **Tehtävä:** use caset ja säännöt, jotka tarvitsevat haun (tenttiesimerkissä 7 ja 9), tuloksen laskenta; fake-tallennus ja kiinteä kello testeihin.
- **Lukee:** `docs/domain.md`, `docs/arkkitehtuuri.md`, `docs/rajapinta.md` (virheet), spec ja sen askel, Domainin julkiset tyypit.
- **Saa muuttaa:** Application-projekti ja sen testiprojekti.
- **Ei saa:** muuttaa Domainia; lisätä tallennuksen rajapintaa (se on Domainin, pyyntö menee johtajan kautta Domain-työntekijälle); viitata EF Coreen tai ASP.NET Coreen.
- **Palauttaa johtajalle:** muutetut tiedostot, use case -testien tuloste, kriteeri → testi -lista, puuttuvat rajapintametodit kysymyksinä.
- **Valmis kun:** askeleen kriteerit vihreitä use case -testinä ja hylkäys on paluuarvo, ei poikkeus.

## Infrastructure-työntekijä

- **Tehtävä:** tallennuksen rajapintojen toteutus EF Corella ja SQLitella, migraatiot, kello.
- **Lukee:** `docs/arkkitehtuuri.md`, Domainin rajapinnat ja tyypit, spec ja sen askel.
- **Saa muuttaa:** Infrastructure-projekti ja SQLitea vasten ajettavat testit.
- **Ei saa:** tarkistaa yhtään sääntöä; muuttaa Domainia tai Applicationia; kirjoittaa asetusarvoa koodiin.
- **Palauttaa johtajalle:** muutetut tiedostot, migraation nimi, testien tuloste, kysymykset.
- **Valmis kun:** sama use case -testi menee läpi SQLitea vasten, Domainissa ei ole EF-viittausta, `*.db`-tiedostoa ei ole repossa.

## API-työntekijä

- **Tehtävä:** reitit, DTO:t ja statuskoodit `docs/rajapinta.md`:n mukaan, API-testit specin kriteereille.
- **Lukee:** `docs/rajapinta.md`, `docs/arkkitehtuuri.md`, spec ja sen askel, use casejen julkiset tyypit.
- **Saa muuttaa:** API-projekti ja sen testiprojekti.
- **Ei saa:** tarkistaa sääntöä controllerissa; kutsua tallennusta ohi use casen; palauttaa Domain-tyyppiä; keksiä reittiä, kenttää tai statuskoodia, jota `rajapinta.md` ei nimeä.
- **Palauttaa johtajalle:** muutetut tiedostot, API-testien tuloste kriteerinumeroin, kysymykset.
- **Valmis kun:** askeleen kriteerit menevät läpi HTTP:llä ja vastauksissa on vain `rajapinta.md`:n kentät.

## Katselmoija

- **Tehtävä:** lukee askeleen diffin ennen committia ja etsii poikkeamat kuvauksesta. Ei kirjoita koodia, ei ehdota uutta sääntöä.
- **Lukee:** diffin, `docs/domain.md` (Paikka-sarake), `docs/arkkitehtuuri.md` (Kiellot), `docs/rajapinta.md`, specin kriteerit ja askeleen "valmis kun".
- **Tarkistaa tässä järjestyksessä:** 1) sääntö on siinä kerroksessa, jonka Paikka sanoo; 2) kieltoja ei rikota; 3) jokaiselle askeleen säännölle on testi; 4) nimet ovat sanaston mukaiset; 5) repoon ei tule kiellettyä tiedostoa.
- **Saa muuttaa:** ei mitään.
- **Palauttaa johtajalle:** löytölistan, yksi rivi per löytö muodossa `tiedosto:rivi — sääntö/kielto — mitä korjataan`. Tyhjä lista on hyväksyntä. Löytö ilman viittausta kuvaukseen ei ole löytö vaan mielipide, ja se jätetään pois.
- **Valmis kun:** lista on palautettu ja jokainen rivi osoittaa kuvauksen kohtaan.

## Viestit roolien välillä

| Suunta | Sisältö | Muoto |
|---|---|---|
| Johtaja → työntekijä | tehtävänanto | yllä oleva lohko |
| Työntekijä → johtaja | tulos | muutetut tiedostot, testikomento ja tuloste, sääntö → testi -lista, kysymykset |
| Johtaja → katselmoija | katselmointipyyntö | spec, askel, diff, säännöt joita askel koskee |
| Katselmoija → johtaja | löydöt | `tiedosto:rivi — sääntö/kielto — korjaus`, tai tyhjä |
| Johtaja → ihminen | suunnitelma, kysymys, lopetusviesti | `AGENTS.md` Ensin suunnitelma; `docs/versiohallinta.md` Agentin lopetus |

Kysymys kulkee aina ylöspäin: työntekijä ei kysy ihmiseltä, ihminen ei vastaa työntekijälle.

## Milloin hierarkiaa ei käytetä

Korjaus, joka koskee yhtä tiedostoa, ei lisää eikä muuta sääntöä eikä muuta julkista tyyppiä, saa olla sen kerroksen työntekijän tekemä ilman uutta suunnitelmaa. Katselmoija lukee silti diffin, ja johtaja commitoi. Johtaja ei tee korjausta itse missään tapauksessa.
