# h5 - Labratakki

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

## a) Aja tunnilla esitetty ARP hyökkäys ja tutki, miten se toimii

Luin Larin tehtävänannon ohjeet ja aloin etenemään niiden avulla.

Aloitin avaamalla kaksi terminaali-ikkunaa Kalilla ja otin SSH-yhteyden mininettiin komennolla: ```ssh -X mininet@192.168.171.129```


<img width="377" height="154" alt="image" src="https://github.com/user-attachments/assets/3668bf5e-1829-416b-b79b-a1a73570e5e1" />


Käynnistin verkko-ohjaimet syöttämällä ensimmäiseen terminaaliin komennon: ```ryu-manager ryu.app.simple_switch_13```

<img width="336" height="83" alt="image" src="https://github.com/user-attachments/assets/f26246ce-d383-4a75-9ea3-332b1b5712f2" />

Avasin SSH-yhteyden toisessa terminaalissa ja syötin sinne komennon: ```sudo mn --topo single,3 --mac --switch ovsk --controller remote```, joka luo verkon topologian ja suljetun yhteyden työasemien välille.

<img width="530" height="238" alt="image" src="https://github.com/user-attachments/assets/5a92ff2d-5bd6-4c47-8b35-d2c000b411a9" />

Seuraavaksi testasin yhteyksien toimivuuden ```pingall```-komennolla:

<img width="256" height="87" alt="image" src="https://github.com/user-attachments/assets/9d0eb9f9-fcee-47e8-a260-f13d208f028a" />

- All good!

Avasin työaseman 1 terminaalia emuloivan ikkunan komennolla: ```xterm h1```

<img width="416" height="46" alt="image" src="https://github.com/user-attachments/assets/04d4dad8-358f-42e9-90f3-e701823f636d" />


- Sain virheilmoituksen: "X11 connection rejected because of wrong authentication."

Löysin [Stackoverflow:sta](https://stackoverflow.com/questions/67319171/x11-connection-rejected-because-of-wrong-authentication-in-mininet) keskustelun, jossa oli korjaus tähän ongelmaan.

> [!NOTE]
> Lisäsin parametrin "-E" komentoon sudon jälkeen. Sudon manuaalin mukaan tämä parametri ilmoittaa, että haluaa säilyttää nykyiset ympäristömuuttujat. Ymmärtääkseni tämän johdosta ```xterm```-oikeudet säilyvät ([Sudo Manual s.a](https://www.sudo.ws/docs/man/1.8.10/sudo.man/)).

Loin verkkotopologian uudestaan korjatulla komennolla: ```sudo -E mn --topo single,3 --mac --switch ovsk --controller remote``` ja kokeilin komentoa: ```xterm h1``` uudelleen.

<img width="536" height="239" alt="image" src="https://github.com/user-attachments/assets/90beee55-615e-4420-9775-ea0b1d1cae36" />

- Xterm toimii!

Syötin xterm-ikkunassa komennon: ```arp -n``` katsoakseni koneiden MAC-osoitteet.

<img width="434" height="105" alt="image" src="https://github.com/user-attachments/assets/a7e39950-89d2-4df5-83c7-be7fdca18eec" />

Sitten avasin uuden työaseman "h3" komennolla: ```xterm h3``` ja syötin uuteen xterm-ikkunaan komennon: ```sysctl -w net.ipv4.ip_forward=1```, joka mahdollistaa IP-reitittämisen.

Ajoin myös komennon: ```iptables -t nat -A PREROUTING -p tcp --dport 22 -j REDIRECT --to-ports 22```

<img width="362" height="79" alt="image" src="https://github.com/user-attachments/assets/9f61b356-e495-41d8-8db8-f9001f74772f" />

Seuraavaksi aloitin verkkoliikenteen kaappauksen tcpdumpilla, tallentaen liikenteen omaan tiedostoonsa komennolla: ```tcpdump -i h3-eth0 -w arp-p.pcap &```

<img width="367" height="41" alt="image" src="https://github.com/user-attachments/assets/6121d5d5-bd8a-41a6-9102-f2a864f5daab" />

- Nuuhkii.

Avasin Wiresharkin ja aloin kuuntelemaan h3-eth0 verkkoa komennolla: ```wireshark &```
> [!NOTE]
> En nähnyt diassa olevaa h1-eth0 verkkoa h3 työasemalla, joten aloin kuuntelemaan ainoaa mahdollista verkkoliitäntää.

<img width="659" height="230" alt="image" src="https://github.com/user-attachments/assets/c4c1e19b-df58-4262-b2d2-8f0380479880" />

Vielä ennen hyökkäystä syötin komennon: ```sshesame -port 22 -listen_address 0.0.0.0```, joka on helposti käyttöönotettava SSH-honeypot ([Jaksi 2024](https://github.com/jaksi/sshesame))

<img width="377" height="54" alt="image" src="https://github.com/user-attachments/assets/bc235224-350a-4bbb-8e67-61ef983cc84f" />

Avasin uuden h3-terminaalin, josta ajoin itse hyökkäyksen komennolla: ```python3 ./arp_poison.py```

Sitten avasin jälleen h1-terminaalin ja yritin ottaa ssh-yhteyttä h1-->h2 komennolla: ```ssh h2```

<img width="370" height="215" alt="image" src="https://github.com/user-attachments/assets/ebc2ae12-b97a-4bdd-ba82-7878a5d26ced" />

- Tadaa!

Tein vielä ARP-pyynnön todentaakseni myrkytyksen:

<img width="399" height="42" alt="image" src="https://github.com/user-attachments/assets/9ed56f83-eb06-4dea-ba4c-0cdd4d7fb9ab" />

Poistin vanhan avaimen h1-terminaalissa komennolla: ```ssh-keygen –R h2``` ja otin uudestaan ssh-yhteyden:

<img width="819" height="245" alt="image" src="https://github.com/user-attachments/assets/62e745d4-edaa-4828-8f2a-44ac5d1d0b0f" />

- Vasemmalla uhrin näkökulmasta saan varoitusviestin, joka kertoo mahdollisesta "Man in the middle"-hyökkäyksestä.
- Oikealla Sshesame ottaa vastaan SSH-yhteyden uhrilta ja tulostaa omaan lokiinsa selkokielisenä tunnuksen, salasanan ja SSH-version. Myös jatkokomennot tulevat suoraan sshesame lokiin.

Myös Wireshark kaappauksessa näkyy myrkytys mielestäni selvästi:

<img width="662" height="146" alt="image" src="https://github.com/user-attachments/assets/c2d9eb0c-5a17-42f3-a703-c552dbc507e6" />

Paketissa 3 h3 tekee ARP-kyselyn uhrikoneelle ja uhri vastaa siihen. Paketissa 5 h3 kuitenkin vastaa tismalleen samalla tavalla väärentäen "henkilöllisyytensä". Lopuissa paketeissa hyökkääjä myös sylkee ARP-vastauksia parin sekunnin välein pitääkseen hyökkäyksen voimassa.

Mielenkiintoinen demo!



## b) Samassa hakemistossa on myös ICMP Spoofing ja TCP Session Hijacking. Aja molemmat labrat läpi ja kerro, miten molemmat tekniikat toimivat



## c) Hakemistossa 02-SDN-DDos_Simulation tryout-kansiossa on työkalut, jotta voit ajaa TCP SYN-Flood-hyökkäyksen turvallisesti

### Kirjoita, miten ajoit hyökkäyksen ja miten kyseinen hyökkäys toimii


## Lähteet
