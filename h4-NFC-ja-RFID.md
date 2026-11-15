# h4 - NFC ja RFID

- Kurssi: [Verkkoon tunkeutuminen ja tiedustelu](https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/) (Karvinen & Iso-Anttila 2026)
- Opettajat: Tero Karvinen & Lari Iso-Anttila
- Raportin kirjoittaja: Jani Karjalainen

## Käyttöympäristö

- Käyttöjärjestelmä: Microsoft Windows 10 Home
- Emolevy: Gigabyte Z170-Gaming K3
- Prosessori: Intel i5-6600K
- Näytönohjain: NVIDIA GeForce RTX 2060
- RAM: 16 GB DDR4
- Virtualisointiohjelmisto: **VMWare Workstation Pro**


## 1. Tarkastele käytössäsi olevia RFID tuotteita, mieti miten hyvin olet suojautunut RFID urkinnalta

### Kotoani löytyvät RFID-tuotteet:

| Laite | Info |
  | :---: | :---: |
  | iPhone |  Apple Pay & luku/kirjoitus kikkailulla | 
  | Koiran mikrosiru |  Koirien tiedot eläinlääkärissä | 
  | Parkkihallin ovilätkä |  Aukeaa ovet | 
  | Roskaputken avauslätkä | Roskaputkien avaaminen |
  | Maksukortit |  Visa jnejne.. |
  | Kotiavaimet |  Abloyn sähkölukko-avaimet |
  | HSL-kortti | Matkan osto julkisissa | 
  | Passi | Matkustusturvallisuus, väärennysten ehkäisy jne. |
  
Suojautumisen puolesta maksukortit, Apple pay, passi ja kotiavaimet ovat varmaankin turvallisimmat RFID-tuotteet minulla. Niiden liikenne on salattu hyvin. Haavoittuvimmat laitteet ovat varmasti koiran mikrosiru, parkkihallin ja roskaputkien avauslätkät. 

En omista mitään special lompakkoa, mutta korttini ovat päällekäin vaikeuttaen urkintaa. Avaimenperästäni parkkihalli- ja roskislätkäni ovat huonossa asemassa, mutta riskin vaara on mielestäni niin pieni, ettei se haittaa. En kanna passiani turhaan ympäriinsä, vaikka se onkin todennäköisesti turvallisin RFID-laite omistuksessani.



## 2. Tutustu APDU komentojen rakenteeseen

Kysyin [Geminiltä](https://share.gemini.google/aUzHcb5IWvsj) APDU-komennoista, sillä en ole ennen näistä kuullutkaan. Käytin promptina tehtävänannon otsikkoa: "Tutustu APDU komentojen rakenteeseen".

APDU on siis tiedonsiirtomuoto, jota käytetään älykorttien ja älykorttilukijoiden välisessä viestinnässä.

Laitteiden välinen viestintä on isäntä-orja tyylistä, jossa lukija on isäntä joka lähettää komentoviestejä (C-APDU) ja älykortti on orja, joka lähettää vastausviestejä (R-APDU).

### Komento-APDU (Lukijan lähettämä 4 tavun header ja valinnainen body)

| Tavu | Kenttä | Tila | Kuvaus |
| :---: | :---: | :---: | :---: |
| 1 | CLA (Class) | Pakollinen | Määrittää komennon luokan |
| 2 | INS (Instruction) | Pakollinen | Käsky (SELECT, READ, VERIFY jnejne) |
| 3 | P1 (Parameter 1) | Pakollinen | Komennon ensimmäinen parametri |
| 4 | P2 (Parameter 2) | Pakollinen | Komennon toinen parametri |
| 5 | Lc (Length of command) | Valinnainen | Ilmoittaa tulevan data-kentän pituuden tavuina |
| 6..n | Data | Valinnainen | Kortille kirjoitettava tai lähetettävä hyötykuorma (pituus Lc) |
| n+1 | Le (Length expected) | Valinnainen | Odotetun vastausdatan enimmäispituus tavuina ( Arvo "00" tarkoittaa yleensä kaikkea saatavilla olevaa dataa 256 tavuun asti) |

> [!NOTE]
> Tavurivin n-kirjain edustaa datakentän viimeistä tavua.

### Vastaus-APDU (Älykortin palauttama valinnainen data ja pakollinen kahden tavun tilasana)

| Tavu | Kenttä | Tila | Kuvaus |
| :---: | :---: | :---: | :---: |
| 1..n | Data | Valinnainen | Kortin palauttama tieto |
| n+1 | SW1 (Status Word 1) | Pakollinen | Komennon suorituksen tilakoodin ensimmäinen osa |
| n+2 | SW2 (Status Word 2) | Pakollinen | Komennon suorituksen tilakoodin toinen osa |

### APDU-komentoja ([Ambimat engineering team 2023](https://ambimat.com/smart-card-selected-apdu-commands/)):

- ```SELECT``` (A4)
  - Valitsee kortilla sovelluksen tai tiedoston aktiiviseksi jatkoa varten.
- ```READ BINARY``` (B0)
  - Lukee raakadataa tavu kerrallaan "läpinäkyvästä" tiedostosta, joka on tekstitiedosto täynnä peräkkäisiä tavuja.
- ```UPDATE BINARY``` (D6)
  - Kirjoittaa tai ylikirjoittaa dataa "läpinäkyvään" tiedostoon.
- ```READ RECORD``` (B2)
  - Lukee yksittäisen tietueen rakenteellisesta tiedostosta, joka on enemmänkin tietokanta verrattuna äskeiseen "läpinäkyvään" tiedostoon.
- ```VERIFY``` (20)
  - Todentaa käyttäjän tai lukijan oikeudet, esimerkiksi PIN-koodin tarkistus.

Suluissa olevat heksadesimaaliluvut ovat komentojen tunnisteet.

## 3. Tutki ja kerro minkä mielenkiintoisen RFID hakkerointi uutiset löysit

### Major Backdoor in Millions of RFID Cards Allows Instant Cloning ([Naraine 2024](https://thehackernews.com/2024/03/dormakaba-locks-used-in-millions-of.html))

Artikkeli kertoo siitä, miten Ranskalainen tietoturvayritys löysi takaoven yhden maailman isoimpien mikrosiruvalmistajien kortteihin. Haavoittuvuus mahdollisti välittömän RFID-älykortti kloonauksen, jonka johdosta hyökkääjä voisi avata toimistojen ja hotellien ovia ympäri maailmaa.

Tällaiset haavoittuvuudet laajasti käytetyissä laitteissa ovat aina mielenkiintoisia. Tehdäänkö niille tarpeeksi pentestejä, ennen julkaisua?

### 'Ghost Tapping' may let scammers skim tap-to-pay cards from inches away ([Elder 2026](https://news4sanantonio.com/news/investigations/new-warning-ghost-tapping-may-let-scammers-skim-tap-to-pay-cards-from-inches-away))

Löysin myös mielenkiintoisen artikkelin "Ghost Tapping"-ilmiöstä, jossa hyökkääjät skimmaa uhrien pankkikortteja.

Yhdysvalloissa rikolliset pyrkivät varastamaan korteilta tietoja tai rahaa lähimaksua hyödyntäen.

Jäinkin miettimään, että onko tämä ongelma enemmänkin Yhdysvalloissa, sillä ymmärtääkseni maksukortit ovat huonommin suojattuja siellä kuin Euroopassa. Itse varaudun isoissa tapahtumissa siirtämään maksukortit esimerkiksi rintataskuun, jotta ainakin huomaan, jos jotain tapahtuu.

## Lähteet

Ambimat engineering team. 22.5.2023. Ambimat Electronics. Smart Card Selected APDU Commands. Luettavissa: https://ambimat.com/smart-card-selected-apdu-commands/. Luettu: 15.11.2026.

Elder, J. 24.42026. 'Ghost Tapping' may let scammers skim tap-to-pay cards from inches away. News 4 San Antonio. Luettavissa: https://news4sanantonio.com/news/investigations/new-warning-ghost-tapping-may-let-scammers-skim-tap-to-pay-cards-from-inches-away. Luettu: 15.11.2026.

Gemini 2026. Keskustelu tekoälyn kanssa. Prompt: Tutustu APDU komentojen rakenteeseen. Luettavissa: https://share.gemini.google/aUzHcb5IWvsj. Luettu: 16.11.2026.

Karvinen, T. & Iso-Anttila, L. 2026. Verkkoon tunkeutuminen ja tiedustelu. Luettavissa: https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/

Naraine, R. 20.8.2024. Major Backdoor in Millions of RFID Cards Allows Instant Cloning. The Hacker News. Luettavissa: https://thehackernews.com/2024/03/dormakaba-locks-used-in-millions-of.html. Luettu: 15.11.2026.
