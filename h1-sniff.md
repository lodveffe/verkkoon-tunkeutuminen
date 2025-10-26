# Sniff

## Tiivistys

- Wireshark on johtava työkalu verkon kuunteluun ja analysointiin. 
- Network interface on kuin verkkokortti, vaikkei välttämättä ole fyysinen. Etuliitteitä: __en__ = wired ethernet, __wl__ = WLAN, __lo__ = loopback adapter
- Omat network interfacet voi checkata `$ ip a`


## Virtuaalikoneen Internetyhteyden katkaiseminen ja palauttaminen

Network Adapter-asetuksista pystyy valitsemaan onko yhteydessä nettiin vai ei:

<img width="861" height="350" alt="image" src="https://github.com/user-attachments/assets/148a39e5-8945-4649-9795-9993fa76c376" />

Testaamatta on tekemättä;

<img width="1066" height="606" alt="image" src="https://github.com/user-attachments/assets/45678e8c-4663-4514-a447-768017b7d79e" />


## Wireshark

Neljä kerrosta TCP/IP-mallista:

<img width="1811" height="465" alt="image" src="https://github.com/user-attachments/assets/48d92ec9-e0dc-4835-8991-9a027c46011b" />

<img width="913" height="81" alt="image" src="https://github.com/user-attachments/assets/baae108c-c46b-4d54-9f4b-02be79fbd665" />

## Mitäs tuli surffattua?

Oon sen verran noobie wiresharkin kanssa että hyvä jos jotain saa tästä sieppauksesta selvää. Nooh katotaan.

- Seitsemän eri IP-osoitetta näyttää löytyvän
- 7 ja puoli sekuntia about kestänyt pakettien kaappaaminen
- Googlessa ja terokarvinen.comissa käyty

### Minkä merkkinen verkkokortti?

Sen tiedon luulisi löytyvän Ethernet layeriltä, mutta näyttää vaan `Ethernet II, src: 52:54:00:2f:e1:e5` 

<img width="1832" height="696" alt="image" src="https://github.com/user-attachments/assets/14ce2fac-1898-40e2-9daf-b596f1f0252f" />

### Millä weppipalvelimella on surffailtu?

Näkyy että terokarvinen.comissa on käyty:

<img width="1812" height="176" alt="image" src="https://github.com/user-attachments/assets/c69af0ae-feed-4dae-bb3e-7749f13af339" />


## Lähteet

https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/

https://terokarvinen.com/network-interface-linux/

https://terokarvinen.com/wireshark-getting-started/

