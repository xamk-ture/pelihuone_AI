# Spec 002 — Luo ja julkaise tentti

## Tarina

Opettajana haluan luoda tentin luonnoksena, lisätä siihen monivalintakysymyksiä ja julkaista sen, kun se on valmis, jotta opiskelijat voivat suorittaa sen tenttipäivänä eikä kukaan voi muuttaa kysymyksiä kesken.

## Käsitteet

`docs/domain.md`. Ei uusia sanoja.

## Tekemiset

| Tekeminen | Onnistuu kun | Epäonnistuu kun |
|-----------|--------------|-----------------|
| Luo tentti | Nimi ei tyhjä, ikkunan alku ennen loppua, kesto > 0. Syntyy luonnoksena. | Muotovirhe → 400. Alku lopun jälkeen tai kesto 0 → 409. |
| Lisää kysymys | Tentti on luonnos, 2–6 vaihtoehtoa, oikea on yksi niistä. | Julkaistu → 409. Vaihtoehtoja väärä määrä tai oikea ei osu → 409. |
| Julkaise | Luonnos, vähintään yksi kysymys. | Ei kysymyksiä tai jo julkaistu → 409. |
| Listaa / katso | Aina. Sivuina. Kysymykset ilman oikeaa numeroa. | — |

## Säännöt ja Paikka

- 1 (alku ennen loppua) — Aikaikkuna
- 2 (kesto > 0) — Tentti
- 3 (2–6 vaihtoehtoa, oikea yksi niistä) — Kysymys
- 4 (julkaistavalla ≥ 1 kysymys) — Tentti
- 5 (julkaistuun ei lisätä eikä poisteta) — Tentti

Kaikki entiteetissä tai arvossa. Tässä specissä ei ole yhtään use case -sääntöä: mikään tekeminen ei tarvitse hakua tai toista oliota.

## Hyväksymiskriteerit

1. **Given:** ei tenttejä. **When:** opettaja luo tentin "Arkkitehtuuri 1", ikkuna 15.10. 8.00–20.00, kesto 60. **Then:** tentti syntyy tilassa luonnos.
2. **Given:** ei tenttejä. **When:** opettaja luo tentin, jonka ikkunan loppu on ennen alkua. **Then:** hylätään, 409, syy "ikkunan alku on lopun jälkeen".
3. **Given:** luonnos L. **When:** opettaja lisää kysymyksen, jossa neljä vaihtoehtoa ja oikea on 2. **Then:** L:llä on yksi kysymys.
4. **Given:** L. **When:** opettaja lisää kysymyksen, jossa yksi vaihtoehto. **Then:** hylätään, 409.
5. **Given:** L. **When:** opettaja lisää kysymyksen, jossa neljä vaihtoehtoa ja oikea on 7. **Then:** hylätään, 409.
6. **Given:** L ilman kysymyksiä. **When:** opettaja julkaisee. **Then:** hylätään, 409, syy "tentillä ei ole kysymyksiä".
7. **Given:** L, jolla on kysymys. **When:** opettaja julkaisee. **Then:** tila on julkaistu.
8. **Given:** julkaistu J. **When:** opettaja lisää kysymyksen. **Then:** hylätään, 409, syy "tentti on julkaistu".
9. **Given:** 25 tenttiä. **When:** listataan sivukoolla 20. **Then:** ensimmäinen sivu antaa 20 ja kokonaismäärän 25; toinen sivu antaa 5.
10. **Given:** J, jolla on kysymyksiä. **When:** haetaan J. **Then:** kysymyksissä on teksti ja vaihtoehdot, ei oikean numeroa.

Kriteerit 2, 4, 5, 6 ja 8 ajetaan myös Domain-testeinä.

## Ei-tavoitteet

- Ei kysymyksen muokkausta eikä poistoa — ei luonnoksessakaan tässä versiossa. Uusi kysymys lisätään, väärä jää. (Oletus; kirjattu `havainnot.md`:n ulkopuolelta, kysytään asiakkaalta.)
- Ei kysymysten järjestyksen vaihtoa.
- Ei tentin kopiointia uusintaa varten.
- Ei opettajan tunnistamista.

## Tehtävät

1. **Domain:** Aikaikkuna, Tentti, Kysymys ja säännöt 1–5. Valmis kun Domain-testit kriteereille 2, 4, 5, 6, 8 ovat vihreät. *Jos spec 001 on jo tehty, tämä askel on tehty — tarkista eikä kirjoita uudestaan.*
2. **Application:** luo, lisää kysymys, julkaise, listaa, hae — muistitallennuksella. Valmis kun kriteerit 1–10 menevät läpi use case -testinä.
3. **Infrastructure:** tallennus kysymyksineen. Valmis kun sama testi menee läpi SQLitea vasten.
4. **API:** reitit `docs/rajapinta.md`:n mukaan, kriteeri 10 tarkistettu vastauksesta. Valmis kun kriteerit 1–10 menevät läpi HTTP:llä.
