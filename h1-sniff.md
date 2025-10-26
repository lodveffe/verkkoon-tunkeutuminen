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

[Wikipediasta](https://en.wikipedia.org/wiki/Internet_protocol_suite) kun luntataan, niin nähdään että Application layeristä löytyy TLS eli Transport Layer Security, Transport layerista itse TCP eli Transmission Control Protocol, Internet layerista IPv4, ja Link layeristä Ethernet. Jokaisesta TCP/IP-mallin kerroksesta löytyi jotain.

## Mitäs tuli surffattua?

Oon sen verran noobie wiresharkin kanssa että hyvä jos jotain saa tästä sieppauksesta selvää. Nooh katotaan.

- Seitsemän eri IP-osoitetta näyttää löytyvän
- 7 ja puoli sekuntia about kestänyt pakettien kaappaaminen
- Googlessa ja terokarvinen.comissa käyty

### Minkä merkkinen verkkokortti?

Sen tiedon luulisi löytyvän Ethernet-kohdasta, mutta näyttää vaan `Ethernet II, src: 52:54:00:2f:e1:e5` Jos joku mua älykkäämpi ristiinarvioija osaa kertoa palauteosiossa mistä se merkki löytyy niin olisi jees.

<img width="1832" height="696" alt="image" src="https://github.com/user-attachments/assets/14ce2fac-1898-40e2-9daf-b596f1f0252f" />

### Millä weppipalvelimella on surffailtu?

Näkyy että terokarvinen.comissa on käyty:

<img width="1812" height="176" alt="image" src="https://github.com/user-attachments/assets/c69af0ae-feed-4dae-bb3e-7749f13af339" />

## Oman liikenteen analyysi

Pistin wiresharkin tallentamaan liikennettä ja otin vain selaimen esiin taustalta, enkä edes klikannut mitään niin tämmösiä QUIC-protokollia alkoi jo satelemaan.

<img width="1812" height="176" alt="image" src="https://github.com/user-attachments/assets/6a81f824-5a44-46c0-9b6f-36bf10184811" />

[Tämän](https://www.auvik.com/franklyit/blog/what-is-quic-protocol/) lähteen mukaan on siis joku Googlen kehittämä nopea vaihtoehto TCP-protokollalle nettiselaimilla. 
Näkyy myös, että HTTPS yhteydestä on kysymys, kun portti 443 on kyseessä:

<img width="907" height="321" alt="image" src="https://github.com/user-attachments/assets/6e0fe865-008e-49de-936c-26e4d72c1478" />

QUIC-protokollalla on kuitenkin jonkin sortin handshaket:

<img width="917" height="229" alt="image" src="https://github.com/user-attachments/assets/575be4e6-3913-4e8c-8083-fc2515a98477" />
 

## Lähteet

https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/

https://terokarvinen.com/network-interface-linux/

https://terokarvinen.com/wireshark-getting-started/

https://www.auvik.com/franklyit/blog/what-is-quic-protocol/

https://docs.google.com/document/d/1g5nIXAIkN_Y-7XJW5K45IblHd_L2f5LTaDUDwvZ5L6g/edit?pli=1&tab=t.0

https://en.wikipedia.org/wiki/Internet_protocol_suite
