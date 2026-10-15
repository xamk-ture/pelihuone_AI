# Arkkitehtuuri — tenttijärjestelmä

## Osat

Neljä projektia. Ei enempää.

| Osa | Saa tehdä |
|-----|-----------|
| **Domain** | Käsitteet, arvot ja säännöt 1–6, 8, 10, 11. Tallennuksen rajapinnat (mitä tarvitaan, ei miten). |
| **Application** | Use caset: yhden tekemisen kulku ja säännöt 7 ja 9. Tuloksen laskenta. |
| **Infrastructure** | Tallennuksen toteutus (EF Core, SQLite). Kello. |
| **API** | HTTP sisään ja ulos: reitit, DTO:t, statuskoodit. Ei sääntöjä. |

## Suunta

Riippuvuudet osoittavat sisäänpäin. Kieltoina:

- Domain ei viittaa mihinkään projektiin eikä tietokanta- tai HTTP-kirjastoon.
- Application viittaa vain Domainiin.
- Infrastructure viittaa Domainiin ja Applicationiin, ei API:in.
- API viittaa Applicationiin ja kokoaa Infrastructuren composition rootissa. API ei viittaa Domainin tyyppeihin vastauksissaan.

Kääntäjä valvoo: väärä projektiviittaus ei käänny.

## Kiellot

1. Älä tarkista määräaikaa, palautusta tai aikaikkunaa controllerissa. Niillä on Paikka `domain.md`:ssä.
2. Älä vie EF Corea tai SQLiten nimeä Domainiin.
3. Älä anna API:n ohittaa use casea ja kutsua tallennusta suoraan.
4. Älä kutsu järjestelmän kelloa Domainissa tai Applicationissa. Nyt-hetki tulee rajapinnan kautta, jonka toteutus on Infrastructuressa. Testissä kello on kiinteä.
5. Älä palauta Kysymystä sellaisenaan opiskelijalle: oikean vaihtoehdon numero ei saa näkyä suorituksen aikana eikä tuloksessa.
6. Älä tallenna tulosta. Se lasketaan.

## Elinkaari

Muistissa pidetty tallennus (testit) elää prosessin ajan, koska lista ei saa nollautua pyyntöjen välillä. Tietokantaan kytketty tallennus elää yhden pyynnön ajan, koska yhteys on kallis ja pyynnöt eivät saa jakaa toistensa tilaa. Ne eivät ole sama asia: ensimmäinen *on* tila, toinen *avaa yhteyden* tilaan.

## Ei-tavoitteet

- Ei kirjautumista. Opiskelijanumero on merkkijono pyynnössä.
- Ei esseekysymyksiä, ei monta oikeaa vaihtoehtoa, ei satunnaista järjestystä.
- Ei arvosana-asteikkoa, ei hyväksymisrajaa. Tulos on pisteet ja maksimi.
- Ei ilmoituksia, ei sähköpostia.
- Ei oikean vaihtoehdon korjausta julkaisun jälkeen.
- Ei selainta, ei CQRS:ää, ei pilveä, ei Dockeria, ei CI:tä.
- Käynnistys ei lisää demodataa.

## Valmis

Sanoin, ei skriptinä:

- Toinen palautus samaan suoritukseen hylätään ja vastaus kertoo syyn.
- Suoritus, joka alkaa kymmenen minuuttia ennen ikkunan loppua, saa määräajaksi ikkunan lopun, ei tuntia.
- Opiskelija ei näe oikeaa vaihtoehtoa missään vastauksessa.
- Sääntöjen 1–6, 8, 10 ja 11 testit eivät käynnistä web-palvelinta eivätkä avaa tietokantaa.
- Testissä kelloa voi siirtää ilman odottamista.
