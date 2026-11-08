# h3 - Aaltoja Harjaamassa

- Kurssi: [Verkkoon tunkeutuminen ja tiedustelu](https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/) (Karvinen 2026)
- Opettaja: Tero Karvinen
- Raportin kirjoittaja: Jani Karjalainen

## Käyttöympäristö

- Käyttöjärjestelmä: Microsoft Windows 10 Home
- Emolevy: Gigabyte Z170-Gaming K3
- Prosessori: Intel i5-6600K
- Näytönohjain: NVIDIA GeForce RTX 2060
- RAM: 16 GB DDR4
- Virtualisointiohjelmisto: **VMWare Workstation Pro**

## x) Lue ja tiivistä


### Universal Radio Hacker SDR Tutorial on 433 MHz radio plugs ([Hubmartin 2019](https://www.youtube.com/watch?v=sbqMqb6FVMY&t=199s))

- Tallentamalla URH-sieppauksen pääset suoraan käsittelemään sitä URH-sovelluksessa.
- Voit tarkistaa bittisyvyyden maalaamalla yksittäisen bitin viestistä.

### Decode 433.92 MHz weather station data ([Cornelius 2024](https://www.onetransistor.eu/2022/01/decode-433mhz-ask-signal.html))

- Kirjoittaja osti Lidlistä langattoman sääaseman, josta hän halusi selvittää radioprotokollan, jotta voi tarvittaessa rakentaa tai hankkia siihen uuden anturin.
- SDR laitteilla ja työkaluilla kuten URH voidaan analysoida signaaleja useista laitteista.
- URH:illa voit dekoodata (purkaa) viestejä, tässä tapauksessa kirjoittaja teki mukautetun dekooderin laitteelle.


## b) rtl_433

> [!NOTE]
> Muutin tehtävänannon vihjeissä olevaa komentoa lataamiseen, sillä "rtl_433" ei löydä pakettia, mutta "rtl-433" löytää.

Latasin rtl_433-ohjelman Linuxille syöttämällä terminaaliin komennon: ```sudo apt-get install rtl-433```

<img width="599" height="131" alt="image" src="https://github.com/user-attachments/assets/13c6f762-5564-4256-9ff1-af5c7f2fde40" />

- Ladattu!
- Ilmoittaa ohjelman versionumeron ja kertoo, ettei löydä sopivaa laitetta vastaanottamaan dataa.


## c) Automaattinen analyysi

Tätä tehtävää varten latasin näytesieppauksen [tehtävänannosta](https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/samples/Converted_433.92M_2000k.cs8).

Avasin ladatun sieppauksen komennolla: ```rtl_433 ./Downloads/Converted_433.92M_2000k.cs8```

<img width="836" height="645" alt="image" src="https://github.com/user-attachments/assets/ca38b67c-50b3-4020-a94f-1e84347df9fe" />

Näytteestä löytyvät valmistajat ovat ainakin: ```KlikAanKlikUit-Switch```, ```Proove-Security``` ja ```Nexa-Security```.

Signaalit lähetetään yhdestä ID:stä (8785315), jonka olettaisin olevan kaukosäädin. **8785315** lähettää OFF-komennon napista 3.

Timestampien perusteella kaukosäädintä myös painetaan useita kertoja.


## d) Too compex 16?

Latasin tehtävän d) [näytteen](https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/samples/Recorded-HackRF-20250411_183354-433_92MHz-2MSps-2MHz.complex16s) tehtävänannosta ja katsoin Teron vinkeistä, miten näyte muunnetaan ```complex16s``` --> ```cs8```, eli rtl_433-yhteensopivaksi.

Käytin komentoa: ```mv Recorded-HackRF-20250411_183354-433_92MHz-2MSps-2MHz.complex16s Recorded-HackRF-20250411_183354-433_92MHz-2MSps-2MHz.cs8```

- Vain nimi muuttuu. ```complex16s``` tilalle tulee ```cs8```.

Muutoksen jälkeen pystyin avaamaan näytteen rtl_433:sella komennolla: ```rtl_433 Recorded-HackRF-20250411_183354-433_92MHz-2MSps-2MHz.cs8```.

<img width="820" height="749" alt="image" src="https://github.com/user-attachments/assets/2d8bf25c-db21-4240-9886-3c9c903ba456" />

- Tämä on sama näyte, kuin aikaisemmassa tehtävässä. Se oli alunperin vain urh-yhteensopivassa muodossa.


## e) Ultimate

Tässä tehtävässä minun piti asentaa URH (Ultimate Radio Hacker).

Asensin URH:in avaamalla terminaalin, jonne syötin komennon: ```pipx install urh```, komentoa: ```pipx ensurepath``` ei tarvinnut ajaa, sillä pipx on jo asennettuna Kalille, sekä ```ensurepath``` on jo ajettu aiemmin.

<img width="949" height="266" alt="image" src="https://github.com/user-attachments/assets/47a5e29c-6554-42e0-acdf-9808913003fa" />

- Ready to go!

<img width="822" height="528" alt="image" src="https://github.com/user-attachments/assets/4cd794f8-677d-4ea5-bc19-ff36c2e9699b" />

## Tarkastele näytettä [1-on-on-on-HackRF-20250412_113805-433_912MHz-2MSps-2MHz.complex16s](https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/samples/1-on-on-on-HackRF-20250412_113805-433_912MHz-2MSps-2MHz.complex16s) URH:illa

Latasin jälleen [näytteen](https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/samples/1-on-on-on-HackRF-20250412_113805-433_912MHz-2MSps-2MHz.complex16s), jota aloin tutkimaan.

Siirryin kotihakemistosta ```/Downloads```-hakemistoon komennolla: ```cd Downloads```

Avasin URH:in komennolla: ```urh``` ja valitsin ```file``` --> ```open```.

<img width="873" height="550" alt="image" src="https://github.com/user-attachments/assets/82a1d1ff-b964-4bb8-bda1-07332af70d41" />

Valitsin avautuneesta ikkunasta ```Downloads``` --> ```1-on-on-on-HackRF-20250412_113805-433_912MHz-2MSps-2MHz.complex16s```.

<img width="1213" height="528" alt="image" src="https://github.com/user-attachments/assets/c2bae1e2-782a-4a32-b2b1-8ebfe4675fbd" />

URH:iin avautui vesiputousnäkymä näytteestä:

<img width="1904" height="507" alt="image" src="https://github.com/user-attachments/assets/e9bc9f44-b4e2-440c-9e9a-8d18484327c5" />

## f) Yleiskuva

**Näytteen nimestä selviää:**
- Taajuus = 433,92 MHz
- Ajankohta = 12.4.2025 11:38

Klikkaamalla info-kuvaketta näkyy myös lisää tietoa:

<img width="1116" height="304" alt="image" src="https://github.com/user-attachments/assets/562cf38c-6357-46fd-ba70-cad16a9f8b01" />

- Näytteitä = 5 491 580 kpl
- Näytteitä sekunnissa (Sps) = 1 miljoonaa (Tiedostonimessä lukee kuitenkin 2 miljoonaa SPS?)
- Näytteen pituus = 5,49 sekuntia (Jos SPS = 2 milj. Näytteen pituus muuttuu --> 2,71 sekuntia)

Näytteessä näkyy kolme lähetettyä signaalia, todennäköisesti kolme napin painallusta. Painallusten välissä on taukoja.

g) Bittistä

Tässä tehtävässä käytin apuna ensimmäisen tehtävän Hubmartinin videota. Katsoin myös videon ASK-moduloinnista ([Rohde & Schwarz 2020](https://www.youtube.com/watch?v=eJ5m0Sbr2qw)) ja luin artikkelin: Amplitude Shift Keying ([GeeksForGeeks 2025](https://www.geeksforgeeks.org/electronics-engineering/amplitude-shift-keying/)).

Oikea modulaatio on ```ASK```.

Aloitin tutkimalla näytteen ensimmäistä bittiä: 

> [!NOTE]
> Tässä vaiheessa myös kiinnitin huomiota tiedostonimessä olevaan 2 miljoonan SPS-arvoon. Päätin muuttaa arvon URH:issa 2 miljoonaan (Ei mitään ideaa, vaikuttaako tämä lopputulokseen).

<img width="1630" height="436" alt="image" src="https://github.com/user-attachments/assets/6f51c999-1ca3-49e9-90b6-75e2c42a6902" />

- Pituus = 261,00 µs (0,000261 sekuntia)

Yhden raakabitin pituus sekunteina on järjettömän lyhyt aika.
