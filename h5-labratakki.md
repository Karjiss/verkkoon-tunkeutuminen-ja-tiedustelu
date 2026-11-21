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


<img width="306" height="140" alt="image" src="https://github.com/user-attachments/assets/cc05671d-3178-4f30-9dfe-e3f50545660e" />

Käynnistin verkko-ohjaimet syöttämällä ensimmäiseen terminaaliin komennon: ```ryu-manager ryu.app.simple_switch_13```

<img width="265" height="62" alt="image" src="https://github.com/user-attachments/assets/aa4f1c22-723c-4755-93d3-b69710600790" />

Avasin SSH-yhteyden toisessa terminaalissa ja syötin sinne komennon: ```sudo mn --topo single,3 --mac --switch ovsk --controller remote```, joka luo verkon topologian ja suljetun yhteyden työasemien välille.

<img width="376" height="153" alt="image" src="https://github.com/user-attachments/assets/23de9333-9998-4268-8a0a-7c633187afab" />


Seuraavaksi testasin yhteyksien toimivuuden ```pingall```-komennolla:

<img width="169" height="59" alt="image" src="https://github.com/user-attachments/assets/6ac80e97-69e8-4fab-ad8a-e38f60f36c24" />

- All good!

Avasin työaseman 1 terminaalia emuloivan ikkunan komennolla: ```xterm h1```

<img width="275" height="25" alt="image" src="https://github.com/user-attachments/assets/bac91cbd-1f51-425e-bfc4-6b29ab8964b0" />

- Sain virheilmoituksen: "X11 connection rejected because of wrong authentication."

Löysin [Stackoverflow:sta](https://stackoverflow.com/questions/67319171/x11-connection-rejected-because-of-wrong-authentication-in-mininet) keskustelun, jossa oli korjaus tähän ongelmaan.

> [!NOTE]
> Lisäsin parametrin "-E" komentoon sudon jälkeen. Sudon manuaalin mukaan tämä parametri ilmoittaa, että haluaa säilyttää nykyiset ympäristömuuttujat. Ymmärtääkseni tämän johdosta ```xterm```-oikeudet säilyvät ([Sudo Manual s.a](https://www.sudo.ws/docs/man/1.8.10/sudo.man/)).

Loin verkkotopologian uudestaan korjatulla komennolla: ```sudo -E mn --topo single,3 --mac --switch ovsk --controller remote```

 <img width="546" height="158" alt="image" src="https://github.com/user-attachments/assets/c8f2d2a8-e83e-4994-9939-0098b97de6ea" />

- Xterm toimii!

## b) Samassa hakemistossa on myös ICMP Spoofing ja TCP Session Hijacking. Aja molemmat labrat läpi ja kerro, miten molemmat tekniikat toimivat



## c) Hakemistossa 02-SDN-DDos_Simulation tryout-kansiossa on työkalut, jotta voit ajaa TCP SYN-Flood-hyökkäyksen turvallisesti

### Kirjoita, miten ajoit hyökkäyksen ja miten kyseinen hyökkäys toimii


## Lähteet
