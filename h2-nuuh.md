# h2 - Nuuh

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

### **Wireshark - Getting Started** ([Karvinen 2025a](https://terokarvinen.com/wireshark-getting-started/))

- Voit suodattaa Wireshark sieppauksia erilaisilla suodattimilla, kuten:
    - ```dns``` = Näyttää vain DNS tulokset
    - ```tls``` = Näyttää TLS-salatut tulokset
    - ```http``` = Näyttää salaamattomat http-yhteydet
    - ```tcp.port == <PORTTI>``` = Porttisuodatin
    - ```ip.addr == <IP-OSOITE>``` = IP-suodatin
    - ```frame contains "STRING"``` = Etsii tekstin tuloksista


### **Network Interface Names on Linux** ([Karvinen 2025b](https://terokarvinen.com/network-interface-linux/))

- Network Interface, eli verkkoliitäntä on melkein kuin virtuaalinen verkkokortti.

- Verkkoliitännän prefixit useimmissa moderneissa Linux-järjestelmissä:

  | Prefix | Liitäntä |
  | :---: | :---: |
  | en | Ethernet |
  | wl | WLAN |
  | lo | Loopback-sovitin |

## b) Ei voi kalastaa. Osoita, että pystyt katkaisemaan ja palauttamaan virtuaalikoneen Internet-yhteyden

Klikkasin verkkovalikkoa virtuaalikoneen yläosasta ja katkaisin yhteyden verkkoon: ```Wired connection 1``` klikkaamalla sen alla olevaa ```Disconnect``` -nappia.

<img width="549" height="241" alt="image" src="https://github.com/user-attachments/assets/86653bbc-9738-4c94-bb4d-d511de6d6ea9" />

Kokeilin pingata Googlen nimipalveluun komennolla: ```ping 8.8.8.8```

<img width="622" height="228" alt="image" src="https://github.com/user-attachments/assets/9b6eaa70-3163-4176-a304-90c731ec86e9" />

- Verkko katki!

Sama komento verkko päällä:

<img width="638" height="238" alt="image" src="https://github.com/user-attachments/assets/962db1bb-71ad-4a94-b574-5c48e798e7c3" />

- Tadaa! Verkko toimii!
