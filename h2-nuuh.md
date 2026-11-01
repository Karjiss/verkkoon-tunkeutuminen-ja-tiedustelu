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


Sama komento nettiyhteys päällä:

<img width="638" height="238" alt="image" src="https://github.com/user-attachments/assets/962db1bb-71ad-4a94-b574-5c48e798e7c3" />

- Tadaa! Verkko toimii!

## c) Wireshark

Wireshark oli jo asennettuna virtuaalikoneellani, joten aloin heti sieppaamaan liikennettä omasta verkostani.

Käynnistin Wiresharkin komennolla: ```wireshark```

<img width="412" height="337" alt="image" src="https://github.com/user-attachments/assets/fc4060cf-f109-4597-8562-9b1581c28f38" />

Seuraavaksi valitsin interfacen ```eth0```.

Avasin Firefoxin ja pysäytin sieppauksen siinä vaiheessa, kun tietoa alkoi tulemaan Wiresharkiin.

<img width="728" height="268" alt="image" src="https://github.com/user-attachments/assets/46493a02-4c76-4ec0-8c9e-c1630ee2377a" />

- Parissa sekunnissa tuli erittäin paljon dataa.

## d) Oikeesti TCP/IP


Sieppasin eri verkossa lisää liikennettä ja valitsin HTTP-paketin tarkasteltavaksi:

<img width="1866" height="563" alt="image" src="https://github.com/user-attachments/assets/0001a0f2-951c-47d2-a22d-e553d8109e99" />


| Kerros | Tehtävä |    
  | :---: | :---: |
  | Link Layer | Siirtää tiedon hyödyntäen fyysisiä johtoja, sekä MAC-osoitteita. |
  | Internet Layer | Välittää tiedon tietokoneiden välillä IPv4-osoitteita hyödyntäen. |
  | Transport Layer | Tässä tapauksessa TCP vastaa tiedon luotettavasta välityksestä. |
  | Application Layer | HTTP on sovellustason protokolla, joka tekee esimerkin pyynnön. |

([Wikipedia 2025](https://fi.wikipedia.org/wiki/TCP/IP-viitemalli)) ja ([GeeksforGeeks 2026](https://www.geeksforgeeks.org/computer-networks/tcp-ip-model/))

## e) Mitäs tuli surffattua?

Latasin kurssisivulta [pcap](https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/surfing-secure.pcap)-tiedoston, jossa oli tallennettu sieppaus verkosta.

Sen sai avattua suoraan Wiresharkkiin, joten pääsin tutkimaan sieppausta.

Aloitin selvittämällä tietokoneiden määrän klikkaamalla ```Statistics``` --> ```Endpoints```

<img width="400" height="260" alt="image" src="https://github.com/user-attachments/assets/4125d740-ea3f-4369-b8d0-494b34270b6e" />

Valitsin IPv4 välilehden:

<img width="882" height="537" alt="image" src="https://github.com/user-attachments/assets/27419656-020d-4e70-a444-7b3e701883d5" />

- 7 Tietokonetta, joista 5 on ulkopuolella siepatun verkosta.

Sieppauksessa olevat protokollat löytyvät ```Statistics``` --> ```Protocol Hierarchy```.

1. <img width="396" height="127" alt="image" src="https://github.com/user-attachments/assets/05e038c0-9e32-4398-8e2a-f7217b55c283" />

2. <img width="434" height="194" alt="image" src="https://github.com/user-attachments/assets/e289e569-13a0-4017-852f-dd3b78f260fe" />
