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

Liikennettä oli ainakin TeroKarvinen.com nettisivulle.

## g) Minkä merkkinen verkkokortti käyttäjällä on?

Avasin sieppauksen ensimmäisen paketin, joka tuli IP-osoitteesta: ```192.168.122.7```. Avasin Link Layerin "Ethernet II" osion:

<img width="1371" height="686" alt="image" src="https://github.com/user-attachments/assets/9f3943a5-e08c-43bf-a537-c827efff6683" />

- MAC-osoite = ```52:54:00:2f:e1:e5```
- Valmistajatunniste on 3 ensimmäistä oktettia, tässä tapauksessa "52:54:00"

Etsin netistä osumia kyseisen MAC-osoitteen valmistajatunnisteeseen:

<figure>
  <img width="100%" alt="MacLookup MAC-haku" src="https://github.com/user-attachments/assets/78a9cc68-1c7f-44e4-b0b5-db57e6251221" />
  <figcaption><em>Lähde: <a href="https://maclookup.app/search/result?mac=52:54:00:2f:e1:e5" target="_blank">MacLookup - MAC Address Lookup</a></em></figcaption>
</figure>

- Ei osumaa laitevalmistajaan.

Sain osuman kuitenkin [PureUtil](https://www.pureutil.org/tools/mac-address-lookup) sivuston MAC Address Lookupista:


<img width="1207" height="773" alt="image" src="https://github.com/user-attachments/assets/00fb9567-38f6-4a8f-98a1-e3322165b29c" />

- Vendor = ```QEMU / KVM virtual NIC```

Myös Googlen AI-yhteenveto yritti tätä vaihtoehtoa minulle tyrkyttää, mutta en luottanut siihen 100%.

## h) Millä weppipalvelimella käyttäjä on surffaillut?

Vaikka yhteydet on suojattu, DNS-kyselyt tulevat silti luettavassa muodossa sieppaukseen. Huomasin myös, että TLS-protokollan paketeissa Info-osiossa lukee: "Client Hello" ja sivuston URL.

Aloitin sitten filtteröimällä tulokset näyttämään vain DNS-protokollan paketit:

<img width="1868" height="709" alt="image" src="https://github.com/user-attachments/assets/a5903f27-3827-45a1-9f7e-4f2e47407be6" />

- DNS-kyselyiden perusteella käyttäjä on avannut selaimensa, syöttänyt URL:in "terokarvinen.com" ja surffaillut kyseisellä sivustolla.

Myös filtteröimällä TLS-protokollan saa hyvän kuvan surffailusta:

<img width="1913" height="511" alt="image" src="https://github.com/user-attachments/assets/80c47119-39e2-4cd7-9f88-3764c075f6d2" />

- TLS-salauksesta huolimatta kättelyvaiheessa näkyy URL!
- Käyttäjä on käynyt terokarvinen.com sivustolla, sekä terokarvinen.goatcounter.com sivustolla.

## i) Analyysi

Sieppasin pätkän liikennettä Wiresharkilla analyysia varten:

<img width="1909" height="948" alt="image" src="https://github.com/user-attachments/assets/5c713c8d-1dd1-4aa8-bbfe-0e7bb90f9d15" />

- Avasin example.com verkkosivun.

Huomasin sieppauksen alkupäässä olevan uusia protokollia, joista en ollut kuullut aikaisemmin. Huomasin myös, että paketissa 3 oli tietoa syyllisestä!

<img width="955" height="262" alt="image" src="https://github.com/user-attachments/assets/241c9ed4-4a44-4471-917f-a4a8288c1481" />

- Spotify selkeästi aiheuttaa hälinää verkossa.

Googletin "mDNS Spotify", mutta tulokset vaikutti mahdolliselta 15 tunnin kaninkololta. Googlen automaattinen AI-yhteenveto sisälsi linkin "[Spotify for Developers](https://developer.spotify.com/documentation/commercial-hardware/implementation/faqs)" -sivustolle, josta löysin tuloksia. Ymmärtääkseni Spotify lähettää Multicast DNS -kyselyitä löytääkseen laitteita, joilla voi käyttää spotifytä(?).

Tämän "taustamelun" jälkeen aloin tutkimaan example.com dataa. Avasin paketin "11", joka on DNS-kysely:

<img width="1742" height="819" alt="image" src="https://github.com/user-attachments/assets/e7c5672a-4534-4658-baf6-8e2fc7f5f4ca" />

- Tässä virtuaalikoneeni lähettää DNS-kyselyn URL:ista "example.com".
- Lähde IP = ```192.168.136.128```
- Kohde IP = ```192.168.136.2```
- Lähde portti = ```52028```
- Kohde portti = ```53```

Valitsin alhaalta kohdan: ```Response In: 14```

<img width="939" height="293" alt="image" src="https://github.com/user-attachments/assets/6ef7de8a-2966-4d8d-b0bf-9340e3e670ea" />

- Example.com = ```172.66.147.243```

Kun virtuaalikone on saanut IP-osoitteen, se yhdistää suoraan juuri saatuun IP-osoitteeseen. 

Yhteys muodostetaan QUIC-protokollalla, joka on rakennettu UDP:n päälle. Se on yhdistelmä UDP:n nopeutta ja TLS turvallisuutta ([Check Point s.a](https://www.checkpoint.com/cyber-hub/network-security/what-is-quic/)).

Filtteröin sieppauksen näyttämään vain ```QUIC```-protokollan paketit:

<img width="1914" height="415" alt="image" src="https://github.com/user-attachments/assets/909f7d60-268f-407e-959d-716c132b06aa" />

Kuvassa näkyy QUIC-protokollan kättely, sekä salattu liikenne kättelyn jälkeen:

### Initial-vaihe
- Paketit 17-20
  - Paketissa "18" käyttäjä (minä) lähettää kryptografisen kättelyn alun:
    <img width="952" height="285" alt="image" src="https://github.com/user-attachments/assets/ec359163-b7d3-479f-b506-bb6b7c33dcdd" />

  - Paketissa "19" palvelin lähettää kuittauksen, joka sisältää sen oman initial-paketin:
    <img width="966" height="724" alt="image" src="https://github.com/user-attachments/assets/796712b1-5df8-451c-a3a1-caee1cd1376e" />
### Handshake-vaihe
- Paketit 21-25
  - Näissä paketeissa käyttäjä ja palvelin vaihtavat handshake-paketteja, joissa salausavaimet ja varmenteet tulevat kuvioihin.
    <img width="976" height="704" alt="image" src="https://github.com/user-attachments/assets/e5eb0a17-e7ff-4fbe-b2be-41701a7f3504" />


### Protected Payload -vaihe
- Paketit 26-35
  - Liikenne muuttuu salatuksi kättelyn jälkeen.

Käytin kättelyprosessissa lähteenä YouTube-videota: [How QUIC Works - The Handshake](https://www.youtube.com/watch?v=fHBUOlvS3ts) (Chreer 2021).

Huomasin myös, että QUIC:in kättely on erittäin nopea. Sieppauksen mukaan se vei noin 16 millisekuntia!


## Lähteet

Check Point. s.a. What is QUIC?. Luettavissa: https://www.checkpoint.com/cyber-hub/network-security/what-is-quic/

Chreer. 1.7.2021. How QUIC Works - The Handshake. Youtube-video. Katsottavissa: https://www.youtube.com/watch?v=fHBUOlvS3ts

GeeksforGeeks. 11.6.2026. TCP/IP Model. Luettavissa: https://www.geeksforgeeks.org/computer-networks/tcp-ip-model/

Karvinen, T. 2025a. Wireshark - Getting Started. Luettavissa: https://terokarvinen.com/wireshark-getting-started/

Karvinen, T. 2025b. Network Interface Names on Linux. Luettavissa: https://terokarvinen.com/network-interface-linux/

Karvinen, T. 2026. surfing-secure.pcap. Luettavissa: https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/surfing-secure.pcap

Karvinen, T. 2026. Verkkoon tunkeutuminen ja tiedustelu -kurssisivu. Luettavissa: https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/

MacLookup. 2026. MAC Address Lookup. Luettavissa: https://maclookup.app/search/result?mac=52:54:00:2f:e1:e5

PureUtil. 2026. MAC Address Lookup. Luettavissa: https://www.pureutil.org/tools/mac-address-lookup

Spotify. s.a. Spotify for Developers. Luettavissa: https://developer.spotify.com/documentation/commercial-hardware/implementation/faqs

Wikipedia. 29.11.2025. TCP/IP-viitemalli. Luettavissa: https://fi.wikipedia.org/wiki/TCP/IP-viitemalli



