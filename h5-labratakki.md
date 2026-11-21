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

## b) Samassa hakemistossa on myös ICMP Spoofing ja TCP Session Hijacking. Aja molemmat labrat läpi ja kerro, miten molemmat tekniikat toimivat



## c) Hakemistossa 02-SDN-DDos_Simulation tryout-kansiossa on työkalut, jotta voit ajaa TCP SYN-Flood-hyökkäyksen turvallisesti

### Kirjoita, miten ajoit hyökkäyksen ja miten kyseinen hyökkäys toimii


## Lähteet
