# Lempiväri: violetti

## x) 

### __TUSKAN PYRAMIDI__

David J. Biancon Tuskan pyramidi kertoo, mihin kannattaa keskittyä tuottaakseen hyökkääjälle mahdollisimman paljon tuskaa. Alimpana pyramidissa ovat hashit ja ip-osoitteet, eli niiden kerääminen ei paljoa hyökkääjää hetkauta, sillä ne ovat helposti muutettavissa -> hyökkäys jatkuu. Keskitasossa on hyökkääjän työkalujen tunnistaminen ja yksittäisten artefaktien (jälkien/havaintojen) spottaaminen. Näiden muuttaminen on jo hyökkääjälle huomattavasti vaivalloisempaa muuttaa, kuin ip-osoitteiden tai hashien muuttaminen. Lopuksi pyramidin huipulla on TTPs (tactics, techniques and procedures). Tällä tasolla isket hyökkääjälle luun kurkkuun tunnistamalla jokaisen liikkeen lokeja silmäilemällä ja vastaamalla ja ennakoimalla niihin.

### _Timanttimalli_

Timanttimallin ideana on helpottaa kyberhyökkäysten analysointia keräämällä tietoa timantin joka kulmasta; hyökkääjästä, kyvykkyydestä, infrasta ja uhrista. Näihin kohtiin kun täyttää mahdollisimman paljon tietoa ja yhdistelee timantin kulmia, niin johan alkaa hommat ratkeamaan. Tällä mallilla kiinnitetään huomiota juurikin isompaan kontekstiin, eikä yksittäisiin havantoihin. Tai paremmin sanottuna tällä saadaan ne yksittäiset havainnot paremmin kontekstiin. 

## a) _Apache.log_

    sudo apt-get update
    sudo apt-get install apache2
    sudo systemctl start apache2
    curl localhost
    
<img width="676" height="82" alt="image" src="https://github.com/user-attachments/assets/96ede5a1-7131-4695-83b1-1c417fec2893" />

[Apachen docseista](https://httpd.apache.org/docs/2.4/logs.html
) löytyy infot, kuinka näitä tulkitaan.
Eli ensimmäiseksi on hostin ip-osoite. Mulla näkyi tuossa `::1` 127.0.0.1 sijaan, eli IPv6 localhost-osoite. (Stackoverflow).

Sen perässä ensimmäinen `-` meinaa Apache docsien mukaan, että "_information is not available_". Tässä voisi olla `-` sijaan `RFC 1413` identiteetti ilmoitettuna, mutta oletuksena Apache ei tätä koita edes selvittää, sillä tulos on epäluotettava. [Täältä](https://www.rfc-editor.org/rfc/rfc1413) löytyy lisää `RFC1413`:sta.

Seuraavaksi on taas `-`. Tässä olisi muuten näkyvillä HTTP-autentikoitu `userid`, mutta koska sivu ei ole salasanan takana, niin se kohta jää myös tyhjäksi. 

Tähän väliin aikaleima.

Aikaleiman jälkeen heittomerkkien sisällä on tärkeää tietoa asiakkaan pyynnöstä palvelimelta. [W3Schoolssista](https://www.w3schools.com/tags/ref_httpmethods.asp) kävin muistuttamassa itseäni HTTP:sta. `GET` on HTTP-metodi, joka pyytää saada vastaanottaa dataa palvelimelta. Koska hain pelkällä localhostilla, niin pyyntö tuli suoraan juureen, eli sitä meinaa tuo `/` GETin perässä. Tätä seuraa tieto HTTP-versiosta, joka on tässä 1.1.

Statuskoodi 200 tarkoittaa, että kaikki OK.

Statuskoodin jälkeen näkyy palautetun datan koko tavuina.

Lopuksi kerrotaan vielä mistä asiakas saapui. Koska hain sivua suoraan enkä tullut minkään linkin kautta, niin mulla näkyi siinä `"-"`. `curl / 8.15.0` kertoo mistä browserista pyyntö tuli.


## b) _Nmapped_

<img width="1198" height="389" alt="image" src="https://github.com/user-attachments/assets/28c18e9a-6ee1-49b6-8634-c6a443cac8cb" />

Näkyy ketä skannattiin eli localhost 127.0.0.1 ja sen toinen osoite eli ipv6-versio ::1. 
Defaulttiskannilla nmap skannaa 1000 yleisintä TCP-porttia ja portti 80 on ainoa auki oleva ja loput kiinni, joten nmap ilmoittaa tämän myös. Sieltä nähään että kyseessä on Apache Debiaanille.
Kun -A eli aggressiivinen skannaus oli päällä, niin nähdään tietoa käyttöjärjestelmästä. Lopuksi vielä infoa siitä, kuinka monen reitittimen läpi paketit ovat käyneet läpi.


## c) _Skriptit_

-A meinaa Aggressive, eli tällä skannauksella nmap lisäsi hakuun OS-detectionin, version detectionin sekä Nmap Scripting Enginen eli NSE:n ja vielä tracerouten. (Nmap 2025)


## d) _Jäljet lokissa_

<img width="1427" height="535" alt="image" src="https://github.com/user-attachments/assets/6c464d38-ea48-4c09-b74e-f6990fdc726b" />

Nmap Scripting Engine on selkeästi tehnyt HTTP-pyyntöjä palvelimelle. Siksi näkyvät selkeästi lokeissa. Jos lokit olisivat massiiviset, niin piipusta `|` ja `grep`istä olisi hyötyä.

## e) _Wire sharking_

Wiresharkissa kun iskee tuohon yleiseen filtterikohtaan "nmap", niin ei saada tulosta, joten täytyy mennä `edit` -> `find packet`. Sitten voi valita hakutavaksi merkkijonon ja hakea "nmap".

<img width="1846" height="823" alt="image" src="https://github.com/user-attachments/assets/53345ee9-7638-44e9-9688-6893bb699ae5" />



<img width="644" height="82" alt="image" src="https://github.com/user-attachments/assets/c81ea86e-691b-4237-ba28-1cf3dc3f1539" />


## Lähteet

https://kravensecurity.com/diamond-model-analysis/

https://detect-respond.blogspot.com/2013/03/the-pyramid-of-pain.html

https://httpd.apache.org/docs/2.4/logs.html

https://stackoverflow.com/questions/25608220/what-does-1-mean-in-apache-logs

https://www.rfc-editor.org/rfc/rfc1413

https://nmap.org/book/port-scanning-tutorial.html
