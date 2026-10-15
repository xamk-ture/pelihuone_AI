# Spec 001 — Suorita tentti

## Tarina

Opiskelijana haluan aloittaa julkaistun tentin tenttipäivänä, vastata kysymyksiin omassa tahdissani määräajan sisällä ja palauttaa kerran, jotta saan pisteeni ilman, että kukaan tarkistaa paperia käsin.

## Käsitteet

Sanasto ja kentät: `docs/domain.md`. Tässä ei uusia sanoja.

## Tekemiset

| Tekeminen | Onnistuu kun | Epäonnistuu kun |
|-----------|--------------|-----------------|
| Aloita suoritus | Tentti on julkaistu, ikkuna on auki, opiskelijalla ei ole suoritusta tähän tenttiin. Vastaus kertoo määräajan. | Luonnos, ikkuna kiinni tai toinen suoritus → 409 ja syy. |
| Vastaa | Suoritusta ei ole palautettu, määräaika ei ole ohi, kysymys kuuluu tenttiin ja vaihtoehto on olemassa. Uusi vastaus korvaa vanhan. | Palautettu tai määräaika ohi → 409. Vieras kysymys tai vaihtoehto → 422. |
| Palauta | Ensimmäinen palautus ennen määräaikaa. | Toinen palautus tai määräaika ohi → 409. |
| Katso tulos | Suoritus on palautettu. Pisteet ja maksimi. | Ei palautettu → 409. |

## Säännöt ja Paikka

Vain viittaukset `docs/domain.md`:n riveihin, ei kopiota:

- 6 (julkaistu, ikkuna auki) — Tentti
- 7 (yksi suoritus per opiskelija) — use case
- 8 (määräaika) — Tentti laskee, Suoritus tallentaa; ADR 0001
- 9 (oma kysymys, olemassa oleva vaihtoehto) — use case
- 10 (ei vastausta palautuksen tai määräajan jälkeen) — Suoritus
- 11 (palautus kerran, ennen määräaikaa) — Suoritus

## Hyväksymiskriteerit

Kello on testissä kiinteä ja siirrettävä. Tentti T: ikkuna 8.00–20.00, kesto 60 min, kolme kysymystä.

1. **Given:** T on julkaistu, kello 10.00, opiskelijalla 123 ei ole suoritusta. **When:** 123 aloittaa T:n. **Then:** suoritus syntyy ja määräaika on 11.00.
2. **Given:** sama kuin 1, mutta kello 19.50. **When:** 123 aloittaa. **Then:** määräaika on 20.00, ei 20.50.
3. **Given:** 123:lla on jo suoritus T:hen. **When:** 123 aloittaa T:n uudestaan. **Then:** hylätään, 409, syy "opiskelijalla on jo suoritus tähän tenttiin".
4. **Given:** T on luonnos. **When:** 123 aloittaa. **Then:** hylätään, 409.
5. **Given:** suoritus S alkoi 10.00, kello 10.30. **When:** 123 vastaa kysymykseen 2 vaihtoehdolla 3, sitten vaihtoehdolla 1. **Then:** S:llä on yksi vastaus kysymykseen 2 ja se on 1.
6. **Given:** S, kello 10.30. **When:** 123 vastaa kysymykseen, joka ei kuulu T:hen. **Then:** hylätään, 422.
7. **Given:** S, kello 11.01. **When:** 123 vastaa. **Then:** hylätään, 409, syy "määräaika on ohi".
8. **Given:** S, kello 10.45, kaksi oikeaa vastausta kolmesta. **When:** 123 palauttaa. **Then:** palautettu-aika on 10.45; tulos on 2/3.
9. **Given:** S on palautettu. **When:** 123 palauttaa uudestaan. **Then:** hylätään, 409, syy "suoritus on jo palautettu".
10. **Given:** S on palautettu. **When:** 123 vastaa. **Then:** hylätään, 409.
11. **Given:** mikä tahansa vastaus suorituksen tai tentin reiteiltä. **Then:** vastauksessa ei ole kenttää, joka kertoo oikean vaihtoehdon.

Kriteerit 1, 2, 7, 9 ja 10 ajetaan myös Domain-testeinä ilman HTTP:tä.

## Ei-tavoitteet

- Ei automaattista palautusta määräajan tullessa. Palauttamaton suoritus jää palauttamatta; tulosta ei ole.
- Ei "jäljellä oleva aika" -laskuria. Vastaus kertoo määräajan, selain laskee jos haluaa.
- Ei vastausten tallennusta luonnoksena erikseen: vastaus on vastaus.
- Ei tentin kysymysten satunnaistusta.
- Ei kirjautumista: opiskelijanumero tulee pyynnössä.

## Tehtävät

Yksi askel per prompti. Jokaisella oma tarkistus.

1. **Domain: Aikaikkuna, Tentti, Kysymys, Suoritus ja säännöt 1–6, 8, 10, 11.** Nyt-hetki parametrina. Valmis kun Domain-testit kriteereille 1, 2, 7, 9, 10 ovat vihreät eikä Domain viittaa mihinkään.
2. **Domain: tallennuksen rajapinnat** tentille ja suoritukselle: hae tunnisteella, hae opiskelijan suoritus tenttiin, tallenna. Valmis kun Application kääntyy niitä vasten ilman toteutusta.
3. **Application: aloita suoritus** (sääntö 7) muistitallennuksella testissä. Valmis kun kriteerit 1–4 menevät läpi use case -testinä.
4. **Application: vastaa ja palauta** (sääntö 9) ja tuloksen laskenta. Valmis kun kriteerit 5–10 menevät läpi use case -testinä.
5. **Infrastructure: EF Core + SQLite** -toteutus rajapinnoille ja kello. Valmis kun sama use case -testi menee läpi SQLitea vasten ja Domain-projektissa ei ole EF-viittausta.
6. **API: reitit, DTO:t ja koodit** `docs/rajapinta.md`:n mukaan. Valmis kun kriteerit 1–11 menevät läpi HTTP:llä ja kriteeri 11 on tarkistettu jokaisesta vastauksesta.
7. **Siivous:** `arkkitehtuuri.md`:n Valmis-kohta luettu rivi riviltä diffiä vasten. Valmis kun jokaiseen lauseeseen voi osoittaa testin tai vastauksen.
