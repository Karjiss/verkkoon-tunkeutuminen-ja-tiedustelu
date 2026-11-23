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

Hyödynsin tehtävissä Network Security Labin GitHub [readme:tä](https://github.com/ssam246/Network-Security-Lab/tree/main) (ssam246).


### 1. ICMP Spoofing


Otin Kalilla jälleen SSH-yhteyden mininettiin, jossa siirryin labran skriptihakemistoon komennolla: ```cd lab/01-Network-Security-Lab/scripts/```

<img width="732" height="246" alt="image" src="https://github.com/user-attachments/assets/e13223ad-13a9-4a6b-b2b0-b77a30d77e90" />

- hub_topo.py = Luo labran verkkotopologian.
- sniff_icmp.py = Kuuntelee ICMP-liikennettä.
- spoof_icmp.py = Tekee itse väärentämisen.

Aloitin ajamalla komennon: ```sudo -E python hub_topo.py``` (Käytin taas -E parametriä, joka korjaa xterm ongelmani.) Ajoin myös ```pingall```-komennon testatakseni yhteyden.

<img width="361" height="242" alt="image" src="https://github.com/user-attachments/assets/9f2e02c5-7859-4b1e-9e02-07e46309c388" />

- It works!

Avasin xterm-ikkunat h1 ja h2 komennoilla: ```xterm h1``` ja ```xterm h2```.

Käynnistin h1-terminaalissa Wiresharkin hyökkäyksen liikenteentarkkailua varten komennolla: ```wireshark &```

Wiresharkissa kuuntelin adapteria ```h1-eth0```. Lisäksi käynnistin spooffaus-skriptin samassa terminaalissa komennolla: ```python spoof_icmp.py```

<img width="602" height="386" alt="image" src="https://github.com/user-attachments/assets/00225756-695f-4e92-8bfc-e778e010f5dd" />

- Hyökkäys uhrin pingiä vaille valmis.

Ajoin h2-terminaalissa pingin komennolla: ```ping 10.0.0.1``` ja aloin seuraamaan Wireshark-liikennettä:

<img width="957" height="417" alt="image" src="https://github.com/user-attachments/assets/b45adf22-020b-4145-91da-d645257ca0bf" />

Wireshark kaappauksesta selviää että h2 lähettää ping requestin kohteeseen 10.0.0.1, johon h1 vastaa normaalisti ping replyllä. Seuraavaksi h1 lähettää kuitenkin väärennetyn samanlaisen reply-paketin, jonka huomaa esimerkiksi erilaisesta ID:stä. Oikeassa vastauksessa ID on request- ja reply-paketissa sama, kun taas väärennetyssä se on eri (id=0x0000).

Väärennyksen huomaisi myös MAC-osoitteista, mikäli olisin ajanut spooffauksen vaikka h3-koneelta. Silloin erona olisi ollut ID ja source MAC. Skripti ```spoof_icmp_py``` muuttaa ymmärtääkseni myös lähettäjän IP-osoitteen samaksi, kuin oikean vastauksen lähettäjä.



### TCP Session Hijacking

TCP-Session Hijacking on hyökkäys, jossa kaapataan käynnissä oleva yhteys koneiden välillä. ([Abnormal 2026](https://abnormal.ai/learning/tcp-session-hijacking))

Tässä labrassa käytetyt skriptit:

```tcp_server.py``` = Luo simuloidun TCP-palvelimen.
```tcp_client.py``` = Luo simuloidun TCP-työaseman.
```sniff_tcp_session.py``` = Kuuntelee TCP-istunnon tietoja.
```tcp_hijack.py``` = Skripti, joka tekee TCP-kaappauksen käyttäen väärennettyjä paketteja.

Avasin terminaalit: ```h1```,```h2``` ja 2x ```h3```.

Hyökkääjänä toimii h3, joten avasin h3-terminaalissa ensiksi mielenkiinnosta Wiresharkin jälleen komennolla: ```wireshark &``` ja aloin kuuntelemaan verkkoa ```h3-eth0```. Avasin myös TCP-istunnon tietojenkuuntelun komennolla: ```python sniff_tcp_session.py```

Ajoin h1-terminaalissa komennon: ```python tcp_server.py```

Sitten h2-terminaalissa komennon: ```python tcp_client.py```

<img width="842" height="402" alt="image" src="https://github.com/user-attachments/assets/de2f4228-c484-43d6-9b8f-8f242cc4e3a7" />


- Tässä kohtaa h1-työasema ottaa yhteyden h2-palvelimeen ja h3-snifferi poimii lokiin yhteyden ```SEQ``` ja ```ACK``` tiedot.


Sitten ajoin hyökkäyksen toisessa h3-terminaalissa komennolla:```python tcp_hijack.py```

<img width="839" height="406" alt="image" src="https://github.com/user-attachments/assets/3fca664d-dc11-4253-b50b-d2baa32aebcf" />

**Wireshark**:

<img width="710" height="362" alt="image" src="https://github.com/user-attachments/assets/96e75439-8a6c-4b36-823a-a2ee8e46bfea" />

Readme ei sisällä tietoa siitä, miltä lopputuloksen pitäisi näyttää. Aloin kuitenkin analysoimaan asiaa.

Ensiksi koneet h1 ja h2 keskustelevat keskenään ja luovat luotetun yhteyden. Noin puolen sekuntin kuluttua h1 sulkee yhteyden. Tässä vaiheessa h3 on nuuhkinut liikennettä ja h3 lähettää väärennetyn TCP-paketin sisältäen ```SEQ``` ja ```ACK``` tiedot kuittausta varten. Mielestäni tässä kohtaa tulee ongelmana se, että yhteys on jo suljettu siinä vaiheessa, kun kaappaus-skripti ajetaan.

Wiresharkissa näkyvässä paketissa 13 h3 lähettää väärennetyn TCP-paketin h1-palvelimelle. Paketissa 14 palvelin vastaa ```RST```-vastauksella, joka sulkee yhteyden välittömästi ([Pico s.a](https://www.pico.net/kb/what-is-a-tcp-reset-rst/)).

Uskon tämän johtuvan siitä, että oikea yhteys suljetaan ennen kaappausyritystä.

<img width="1567" height="448" alt="image" src="https://github.com/user-attachments/assets/58e0e285-657f-4b53-83fd-40f035dfcc5b" />

- Tässä näkyy oikeat paketit ja väärennetty paketti.


## c) 02-SDN-DDos_Simulation

TCP SYN-Flood on palvelunestohyökkäys, jossa hyökkääjä pyrkii estämään palvelimen oikean liikenteen. Siinä hyökkääjä hyödyntää 3-vaiheista kättelyä lähettämällä paljon SYN-paketteja palvelimelle, sitten palvelin vastaa ja jää odottamaan vastausta, mitä ei tule. Hyökkääjä toistaa tätä, kunnes palvelin ei kykene toimimaan enään oikein. ([Cloudflare s.a](https://www.cloudflare.com/learning/ddos/syn-flood-ddos-attack/))

Lähdin kokeilemaan TCP SYN-Flood hyökkäystä noudattamalla GitHub käyttäjän [santhisenan](https://github.com/santhisenan/SDN_DDoS_Simulation/blob/master/README.md) ohjeita.

Aloitin luomalla ympäristön. Siirryin SDN_DDoS_Simulation-tryout -hakemistoon kotihakemistosta komennolla: ```cd lab/02-SDN_DDoS_Simulation-tryout/```

Avasin toisen terminaali-ikkunan samaan hakemistoon ja ajoin komennon: ```ryu-manager main.py``` käynnistääkseni verkko-ohjaimen.

<img width="496" height="202" alt="image" src="https://github.com/user-attachments/assets/d0466d7d-42bf-41c3-bedf-7e845372de50" />


Ajoin ensimmäisessä terminaalissa komennon: ```sudo python tree_topology.py```, joka luo skriptin perusteella verkkotopologian ja alkaa floodaamaan.

Avasin vielä kolmannen terminaali-ikkunan, johon ajoin Wiresharkin root-oikeuksilla (jotta näen kaiken liikenteen verkossa) komennolla: ```sudo -E wireshark &``` ja valitsin kaikki äsken luodut hostit, eli: ```s1-eth1``` - ```s7-eth3```.

**Skriptin output**:

<img width="388" height="283" alt="image" src="https://github.com/user-attachments/assets/551a7398-2ac4-4439-a694-04fcb50f4698" />

**Wireshark**:

<img width="1567" height="757" alt="image" src="https://github.com/user-attachments/assets/102bb6f0-5285-4195-b420-3baf8488ce0d" />


- Tavaraa tuli järjettömän paljon lyhyessä ajassa, mutta tämä ei vaikuta silmääni SYN-Floodilta flagin perusteella.

Overall tuntuu, että en saanut tätäkään toimimaan aivan kuten olisin halunnut. Ehkä ongelma on virtuaalikoneessa, ehkä käyttäjässä. 



## Lähteet


Abnormal. 31.8.2026. TCP Session Hijacking: How It Works and How to Detect It. Luettavissa: https://abnormal.ai/learning/tcp-session-hijacking.

Cloudflare. s.a. SYN flood attack. Luettavissa: https://www.cloudflare.com/learning/ddos/syn-flood-ddos-attack/. 

Jaksi. 2024. sshesame. GitHub-repo. Luettavissa: https://github.com/jaksi/sshesame.

Karvinen, T. & Iso-Anttila, L. 2026. Verkkoon tunkeutuminen ja tiedustelu. URL: https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/. Luettu: 23.11.2026.

Pico. s.a. What is a TCP Reset (RST)? Luettavissa: https://www.pico.net/kb/what-is-a-tcp-reset-rst/.

santhisenan s.a. SDN_DDoS_Simulation. GitHub. Luettavissa: https://github.com/santhisenan/SDN_DDoS_Simulation/blob/master/README.md. .

ssam246 s.a. Network-Security-Lab. GitHubä-repo. Luettavissa: https://github.com/ssam246/Network-Security-Lab/tree/main.

Stack Overflow 2021. X11 connection rejected because of wrong authentication in Mininet. Luettavissa: https://stackoverflow.com/questions/67319171/x11-connection-rejected-because-of-wrong-authentication-in-mininet.

Sudo Manual s.a. Sudo 1.8.10. Luettavissa: https://www.sudo.ws/docs/man/1.8.10/sudo.man/.

Iso-Anttila, L. 2025. Luentokalvot.
