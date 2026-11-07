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

**_NOTE:_**  Muutin tehtävänannon vihjeissä olevaa komentoa lataamiseen, sillä "rtl_433" ei löydä pakettia, mutta "rtl-433" löytää.

Latasin rtl_433-ohjelman Linuxille syöttämällä terminaaliin komennon: ```sudo apt-get install rtl-433```

<img width="599" height="131" alt="image" src="https://github.com/user-attachments/assets/13c6f762-5564-4256-9ff1-af5c7f2fde40" />

- Ladattu!
- Ilmoittaa, että ei ole sopivaa laitetta vastaanottamaan dataa.


## c) Automaattinen analyysi

Tätä tehtävää varten latasin näytesieppauksen [tehtävänannosta](https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/samples/Converted_433.92M_2000k.cs8).

Avasin ladatun sieppauksen komennolla: ```rtl_433 ./Downloads/Converted_433.92M_2000k.cs8```

<img width="836" height="645" alt="image" src="https://github.com/user-attachments/assets/ca38b67c-50b3-4020-a94f-1e84347df9fe" />

- 



