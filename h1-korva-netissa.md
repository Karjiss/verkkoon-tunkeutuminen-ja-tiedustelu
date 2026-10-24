# h1 - Korva netissä

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





## x) Spektrini

<img width="1191" height="616" alt="Aaltospektri" src="https://github.com/user-attachments/assets/0ba7d608-5053-477f-b388-1967e71b19ad" />

- Taajuus kasvaa vasemmalta oikealle spektrissä.
- Taajuuden kasvaessa antennin pituus, aallon pituus ja läpäisy pienenee.
- Mitä korkeampi taajuus, sitä suurempi kyky siirtää tietoa. 


## a) WebSDR

Tehtävässä oli tarkoitus etäkäyttää WebSDR-ohjelmaradiota ja kuunnella jotain julkista radiolähetystä toisessa maassa.

Avasin [WebSDR](http://websdr.org/)-sivuston, josta aloin etsimään "kohdetta".

Sivun alaosassa olevasta kartasta löysin radioaseman, joka sijaitsee Texasin Gatesvilessä:

<img width="932" height="468" alt="image" src="https://github.com/user-attachments/assets/e9fa898d-b5b4-4e77-b4a9-31951430c8a1" />

- **Kanavan linkki**: http://sdr.kf5jmd.com:8901/

Kanavan avattuani menin sivun alaosaan, josta klikkasin kohtaa: ```Chrome Browser Audio Start Click Here``` äänentoiston käynnistämiseksi.

<img width="1073" height="665" alt="image" src="https://github.com/user-attachments/assets/291ec563-e475-46ef-8d0c-2f2b6fc173a9" />

Äänien käynnistyksen jälkeen kuulin heti tyypillisen jenkkiradion mainoksia taajuuksilla: ```1230.00 kHz```

<img width="1106" height="469" alt="image" src="https://github.com/user-attachments/assets/4ace7ff5-6708-44a3-97d9-66320c751788" />


Halusin kokeilla toisia kanavia, joten katsoin vesiputousta etsiäkseni seuraavan kanavan:

<img width="1084" height="362" alt="image" src="https://github.com/user-attachments/assets/c0d69b7a-0fcc-4f71-aaf8-e31034a96f65" />

Säätelin taajuutta löytääkseni selkeänkuuloisen lähetyksen kohteesta:

<img width="1211" height="484" alt="image" src="https://github.com/user-attachments/assets/c4ffbc99-dcef-4493-9726-8e1113989522" />



- Kuulostaa joltain pastorilta saarnaamassa.

- Taajuus: ```1009.85 kHz```.
- Aallonpituus ($\lambda$) : ```297,07 metriä```. **Aallonpituus lasketaan kaavalla:** $\lambda = \frac{c}{f}$ (Radio-Amatööri-Wiki 2006)


Modulaatioasetukset:

<img width="550" height="121" alt="image" src="https://github.com/user-attachments/assets/5c1fc17c-240e-461e-816b-b8954e21c44e" />

- Käytössä: ```AM```.





## b) Moduwhat?

Aikaisemmassa tehtävässä modulaatioasetus oli AM, joten seuraavaksi kokeilin käyttää FM-modulaatiota.

Aloitin taas [WebSDR](http://websdr.org/)-sivustolta etsimällä eri asemaa.

Valitsin listalta aseman, joka sijaitsee Ruotsissa:

<img width="1837" height="137" alt="image" src="https://github.com/user-attachments/assets/7aa48bfb-9ea2-494b-a508-669adec0652c" />

Käynnistin äänet sivun yläosasta kohdasta: ```No audio? Click here!```

Seuraavaksi laitoin modulaatioasetuksista ```FM``` päälle, muutin kaistan 10 metriin ja aloin säätämään taajuuksia:

<img width="1026" height="503" alt="image" src="https://github.com/user-attachments/assets/a2f6816b-6060-4906-ac8a-edbc4ac72756" />

- Löytyi jotain!
- Taajuus: ```29750.00 KHz```
- Aallonpituus ($\lambda$) : ```10,08 metriä```

Hieman särisevä ääni, mutta selkeästi puhetta ruotsiksi. Muutin vähän kaistanleveyttä edestakaisin, mutta ei vaikuttanut kauheasti tulokseen.

Valitsin myös äänenvoimakkuusasetuksista päälle kohdan: ```squelch```, jonka olisi tarkoitus vähentää suhinaa.

## c) ADS-B

Tehtävänä oli etsiä kiinnostava lentokone ADS-B Exchange -palvelusta. Konetta ja sen lentoa piti kuvailla.
