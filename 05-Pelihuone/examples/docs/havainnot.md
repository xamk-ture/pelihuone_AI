# Havainnot — tenttijärjestelmä

Kirjattu havainnoinnin jälkeen: katsottiin yksi paperitentin tarkistus ja yksi Moodle-tentin asetussivu opettajan kanssa. Tilauksen kysymykset ja vastaukset ovat `tilaus.md`:ssä; tässä on se, mitä nähtiin.

## Tilanne

Kurssin tentit pidetään nyt paperilla luokassa tai Moodlen tenttiaktiviteetilla. Paperitentti tarkistetaan käsin ja pisteet kirjataan Exceliin. Moodle-tentti toimii, mutta kysymyksiä ei saa sieltä ulos muihin käyttöihin, ja aikarajan käyttäytyminen on opiskelijoille epäselvä.

Opettaja tekee tentit, opiskelijat suorittavat. Ei muita rooleja.

## Nykyinen tapa ja sen ongelmat

Kolme oikeaa tapausta asiakkaan sanoin:

1. "Opiskelija palautti paperin ja tuli puolen tunnin päästä pyytämään sitä takaisin, kun muisti yhden vastauksen. Sanoin ei, mutta sitä ei lue missään."
2. "Moodle-tentissä yksi aloitti klo 19.50, kun tentti sulkeutui klo 20. Hän sai tunnin, vaikka ikkuna meni kiinni — ja valitti, kun toinen ei saanut."
3. "Tarkistin paperitentit ja yhdessä kysymyksessä oli väärä oikea vastaus mallissa. Jouduin laskemaan 40 pistemäärää uudestaan käsin."

## Havainnoitaessa selvinneet

| Kysymys (`tilaus.md`) | Mitä nähtiin |
|------------------------|--------------|
| Miten tentit nyt etenevät? | Paperi: jaetaan, kerätään, tarkistetaan mallin mukaan, pisteet Exceliin käsin. Moodle: opettaja asettaa "avoinna 8–20" ja "aikaraja 60 min" erikseen — Moodle antaa täyden tunnin ikkunan lopun yli, jos asetus "aikaraja voittaa" on päällä. Se oli päällä. |
| Mitä opiskelija näkee, kun aika loppuu? | Moodle palauttaa automaattisesti. Asiakas: "En halua sitä. Palauttamaton on palauttamaton." |
| Missä kohtaa pistevirhe syntyy? | Tulos lasketaan kerran ja kirjoitetaan Exceliin. Kun malli korjataan, Excel ei tiedä. Tästä syystä tulosta **ei tallenneta** uudessa järjestelmässä — se lasketaan vastauksista aina. |

Ristiriita tilauksen kanssa: tilaus sanoi "ikkuna menee kiinni, piste", mutta nykyinen Moodle-asetus tekee päinvastoin. Asiakas vahvisti tilauksen: ikkunan loppu voittaa.

## Avoimet

Näihin ei saatu vastausta. Oletus on merkitty, ja agentin pitää pysähtyä, jos tehtävä riippuu siitä.

| Avoin kysymys | Oletus tänään |
|---------------|---------------|
| Mitä tapahtuu, jos opiskelija sulkee selaimen kesken? | Suoritus jatkuu määräaikaan asti; vastaukset, jotka ehdittiin lähettää, säilyvät. Ei automaattista palautusta (asiakkaan toive edellä). |
| Pitääkö jokaiseen kysymykseen vastata ennen palautusta? | Ei. Vastaamaton kysymys on nolla pistettä. |
| Voiko vastausta muuttaa ennen palautusta? | Kyllä: uusi vastaus samaan kysymykseen korvaa edellisen. |
| Montako vaihtoehtoa enintään? | Kuusi. |
| Mitä tehdään oikean vastauksen korjaukselle julkaisun jälkeen? | Ei-tavoite tässä versiossa. Koska tulos lasketaan aina vastauksista, korjaus olisi myöhemmin halpa. |

## Koeajo

Spec 001, askeleet 1–3, suunnittelutila ensin joka askeleella.

- **Missä agentti kysyi:** askeleen 1 suunnitelmassa — "Onko Aikaikkuna oma tyyppi vai kaksi kenttää Tentissä?" Vastaus oli `domain.md`:n Arvo vai luku -osiossa, mutta agentti luki sen vasta kysyttyään. Lukujärjestys `AGENTS.md`:ssä oli oikea; agentti ohitti sen. Lisätty `AGENTS.md`:hen: "suunnitelman kohta 1 listaa luetut tiedostot".
- **Missä se arvasi:** askeleessa 3 se nimesi tallennuksen rajapinnan `ISubmissionRepository`, vaikka sanasto sanoo "suoritus". Ei kerrosvirhe, mutta kieli lipsui englantiin. Kuvauksesta puuttui päätös koodin kielestä.
- **Mikä lause puuttui:** "Tunnisteet koodissa englanniksi, sanaston vastine: Tentti = Exam, Suoritus = Attempt, Vastaus = Answer." Lisätty `domain.md`:n sanastoon sarakkeeksi.

Säännöt osuivat Paikka-sarakkeen mukaisiin kerroksiin: "palautus vain kerran" Suorituksessa, "yksi suoritus per opiskelija" use casessa, eikä Domainissa ollut EF-viittausta.
