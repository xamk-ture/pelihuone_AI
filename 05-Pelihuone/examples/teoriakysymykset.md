# Kertauskysymykset — vastaukset

[Tehtävän](../README.md) kysymykset. Vastaukset tenttijärjestelmän esimerkillä; pelihuoneen vastaus on sama rakenne eri sanoilla.

**1. Miksi huoneessa käyntiä ei voi korvata hyvällä promptilla?**
Prompti sisältää vain sen, mitä kirjoittaja jo tietää. Huoneesta (tai asiakastapaamisesta) tulee se, mitä kukaan ei tiennyt kysyä: tenttiesimerkissä "19.50 aloittaja ei saa tuntia" ja "tulos ei kerro, mikä meni oikein". Ilman niitä agentti olisi päättänyt molemmat — todennäköisesti toisin.

**2. Millä kysymyksellä ratkaiset, kuuluuko "samaa pelipistettä ei kahdelle samaan aikaan" entiteettiin vai use caseen?**
"Mitä tietoa tarkistus tarvitsee?" Päällekkäisyys vaatii haun muista varauksista (ja tapahtumista), siis use case. Tenttiesimerkin vastine on sääntö 7, "yksi suoritus per opiskelija": yksi suoritus ei tiedä toisista.

**3. Miksi aikaväli on value object mutta pelipisteen numero ei?**
Aikavälillä on kaksi kenttää, joilla on yhteinen sääntö (alku ennen loppua), eikä identiteettiä: kaksi samanlaista väliä ovat sama väli. Numerolla ei ole omaa sääntöä irrallaan omistajastaan. Tenttiesimerkissä sama jako: Aikaikkuna on arvo, vaihtoehdon numero jää luvuksi.

**4. Mikä ero on hyväksymiskriteerillä "varaus toimii" ja Given–When–Then-muodolla? Anna yksi esimerkki hylkäystapauksesta.**
"Toimii" ei kerro lähtötilaa, tekemistä eikä sitä, mistä onnistumisen tunnistaa — sitä ei voi ajaa. Given–When–Then pakottaa kaikki kolme. Hylkäysesimerkki: *Given: suoritus on palautettu. When: opiskelija palauttaa uudestaan. Then: 409 ja syy "suoritus on jo palautettu".* Hylkäystapaukset ovat tärkeimmät, koska juuri ne agentti jättää tekemättä, jos niitä ei ole kirjoitettu.

**5. Miksi "vapaana nyt" ei tallenneta, vaan lasketaan?**
Tallennettu kopio voi erota lähteestään; laskettu ei. Tenttiesimerkin Tulos on sama asia: se lasketaan vastauksista ja oikeista vaihtoehdoista, joten mahdollinen korjaus ei vaadi uudelleenlaskentaa eikä kahta totuutta. Sama syy kuin LeagueHubin sarjataulukolla.

**6. Mihin ADR vastaa, mihin spec ei vastaa?**
Spec sanoo *mitä* tehdään ja mistä valmius todetaan. ADR sanoo, *miksi rakenne on tämä eikä toinen* — vaihtoehdot, jotka hylättiin, ja seuraukset, joihin suostuttiin. ADR 0001:n tieto "määräaika on suorituksessa, siksi säännöt 10 ja 11 ovat entiteetissä" ei mahdu speciin, mutta ilman sitä seuraava sessio laskisi määräajan uudestaan use casessa.

**7. Miksi tehtävälista pilkkoo specin, vaikka agentti osaisi tehdä kaiken kerralla?**
Koska tarkistuksen omistaa se, joka ajaa sen. Yksi iso prompti tuottaa yhden ison diffin, jota ei voi lukea Paikka-saraketta vasten. Askel "Domain ja säännöt 1–6, ei viittauksia" on tarkistettavissa yhdellä katseella; "koko järjestelmä" ei ole. Lisäksi väärä käänne askeleessa 1 (sääntö controllerissa) on halpa korjata, askeleessa 6 kallis.

**8. Mikä `AGENTS.md`:n rivi esti (tai olisi estänyt) koeajossa arvauksen?**
Pysähtymissääntö: "jos joudut valitsemaan statuskoodin, kentän nimen tai kirjaston, jota kuvaus ei mainitse, pysähdy ja kysy". Ilman sitä agentti täyttää aukon sillä, mitä se on nähnyt useimmin — ja tutorial-koodissa sääntö on controllerissa ja aika tulee `DateTime.Now`:sta. Toinen rivi, joka estää eri arvauksen: "nyt-hetki annetaan parametrina".

**9. Miksi agentti ajetaan ensin suunnittelutilassa, ja mitä teet, jos suunnitelma paljastaa kuvauksesta puuttuvan tiedon?**
Suunnitelmasta näkee agentin tulkinnan ennen kuin siitä tulee koodia: väärä Paikka tai keksitty projekti korjataan yhdellä lauseella, ei purkamalla diffiä. Jos suunnitelma paljastaa puuttuvan tiedon (tenttiesimerkissä koodin kieli sanastolle), tieto lisätään **ensin tiedostoon** ja pyydetään uusi suunnitelma. Pelkkä vastaus keskustelussa katoaa session mukana, ja seuraava sessio arvaisi uudestaan.

**10. Miksi tekoälyä käytetään tänään vasta oman version jälkeen, ja mitä `tarkistus.md` todistaa?**
Tyhjästä tekoäly kirjoittaa uskottavan yleisen kuvauksen, joka on väärä juuri niissä kohdissa, joita se ei voi tietää — eikä virhettä huomaa, jos ei ole itse ajatellut asiaa. Oman version jälkeen tekoälyn vastaus on vertailukohta. `tarkistus.md` todistaa, että päätökset ovat kirjoittajan: siinä näkyy, mitä tekoäly ehdotti ja mitä hylättiin perusteluineen (tenttiesimerkissä säännön 8 siirto use caseen hylättiin ADR 0001:n vuoksi).

**11. Miksi katselmoija on eri agentti kuin tekijä, ja miksi vain johtaja commitoi mutta ei yhdistä pull requestia?**
Tekijä täyttää oman työnsä aukot päässään samalla tavalla kuin kuvauksen kirjoittaja: se ei näe, että "palautus vain kerran" päätyi controlleriin, koska se muistaa tarkoittaneensa entiteettiä. Katselmoija lukee vain diffin ja `domain.md`:n Paikka-sarakkeen, eikä sillä ole tekijän muistia — siksi se löytää saman, minkä vierustoveri löytää vaiheessa 6. Commit on yksi paikka historiassa, ja sen pitää vastata yhtä askelta: kun vain johtaja commitoi, kaksi työntekijää ei voi tuottaa kahta puolikasta committia samaan askeleeseen, ja lopetusviesti kertoo kaikki commitit yhdestä paikasta. Pull requestin yhdistäminen on taas se kohta, jossa ihminen ajaa hyväksymiskriteerit itse (`AGENTS.md`, Tarkistukset). Jos agentti saisi yhdistää, "valmis" olisi taas pelkkä sana. Tenttiesimerkin koeajossa (`havainnot.md` → Koeajo) tallennuksen rajapinta sai askeleessa 3 nimen `ISubmissionRepository` sanaston ohi; roolijaolla portti olisi ollut Domain-työntekijän tehtävä ja nimi katselmoijan löytö ennen committia, ei vasta ihmisen diffinluvussa.

**12. Miksi testiaineisto on yksi ja kuvattu `testaus.md`:ssä, eikä jokaisen testin oma? Ja miksi kellon asettaminen käynnissä olevaan ohjelmaan on päätös, joka pitää kirjoittaa?**
Kriteerin numero tarkoittaa jotain vain, jos sen Given lähtee samasta tilasta kaikilla tasoilla: Domain-testi, use case -testi, API-testi ja ihmisen käsiajo todistavat samaa kriteeriä 2 ("19.50 aloittaja saa määräajaksi 20.00") vain, jos tentti T on kaikissa sama ikkunalla 8–20 ja kestolla 60 min. Kun jokainen testi rakentaa oman tenttinsä, sama numero voi tarkoittaa neljää eri tilannetta, eikä punainen testi kerro enää, mikä sääntö petti. Yksi kuvattu aineisto tekee myös specin Given-osasta lyhyen: se sanoo vain poikkeaman. Kello taas on kielletty Domainissa ja Applicationissa, joten testissä se on helppo korvata — mutta ihminen ajaa kriteerit käynnissä olevaa ohjelmaa vasten, ja siihen kellon vaihtaminen on tuotantokoodin ominaisuus. Jos sitä ei ole päätetty (tenttiesimerkissä kehitysympäristön asetus, jota tuotanto ei lue), agentti joko jättää ominaisuuden pois, jolloin kriteeriä 2 ei voi ajaa käsin, tai keksii testireitin, joka jää tuotantoon.
