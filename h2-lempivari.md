# Lempiväri: violetti

## x) _Tekstien tiivistykset_ 

### __TUSKAN PYRAMIDI__

[David J. Biancon Tuskan pyramidi](https://detect-respond.blogspot.com/2013/03/the-pyramid-of-pain.html
) kertoo, mihin kannattaa keskittyä tuottaakseen hyökkääjälle mahdollisimman paljon tuskaa. Alimpana pyramidissa ovat hashit ja ip-osoitteet, eli niiden kerääminen ei paljoa hyökkääjää hetkauta, sillä ne ovat helposti muutettavissa -> hyökkäys jatkuu. Keskitasossa on hyökkääjän työkalujen tunnistaminen ja yksittäisten artefaktien (jälkien/havaintojen) spottaaminen. Näiden muuttaminen on jo hyökkääjälle huomattavasti vaivalloisempaa muuttaa, kuin ip-osoitteiden tai hashien muuttaminen. Lopuksi pyramidin huipulla on TTPs (tactics, techniques and procedures). Tällä tasolla isket hyökkääjälle luun kurkkuun tunnistamalla jokaisen liikkeen lokeja silmäilemällä ja vastaamalla ja ennakoimalla niihin.

### _Timanttimalli_

[Timanttimallin](https://kravensecurity.com/diamond-model-analysis/
) ideana on helpottaa kyberhyökkäysten analysointia keräämällä tietoa timantin joka kulmasta; hyökkääjästä, kyvykkyydestä, infrasta ja uhrista. Näihin kohtiin kun täyttää mahdollisimman paljon tietoa ja yhdistelee timantin kulmia, niin johan alkaa hommat ratkeamaan. Tällä mallilla kiinnitetään huomiota juurikin isompaan kontekstiin, eikä yksittäisiin havantoihin. Tai paremmin sanottuna tällä saadaan ne yksittäiset havainnot paremmin kontekstiin. 

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

-A (Aggressive) lisäsi hakuun OS-detectionin, version detectionin sekä Nmap Scripting Enginen eli NSE:n ja vielä tracerouten. (Nmap 2025)

Näkyy ketä skannattiin eli localhost 127.0.0.1 ja sen toinen osoite eli ipv6-versio ::1. 
Defaulttiskannilla nmap skannaa 1000 yleisintä TCP-porttia ja portti 80 on ainoa auki oleva ja loput kiinni, joten nmap ilmoittaa tämän myös. Sieltä nähään että kyseessä on Apache Debiaanille.
Kun -A eli aggressiivinen skannaus oli päällä, niin nähdään tietoa käyttöjärjestelmästä. Lopuksi vielä infoa siitä, kuinka monen reitittimen läpi paketit ovat käyneet läpi.


## c) _Skriptit_

Skriptit jotka oli käytössä:

- http-server-header
- http-title

## d) _Jäljet lokissa_

<img width="1427" height="535" alt="image" src="https://github.com/user-attachments/assets/6c464d38-ea48-4c09-b74e-f6990fdc726b" />


Nmap Scripting Engine on tehnyt HTTP-pyyntöjä palvelimelle. Siksi näkyvät selkeästi lokeissa. Jos lokit olisivat massiiviset, niin piipusta `|` ja `grep`istä olisi hyötyä.

## e) _Wire sharking_

Wiresharkissa kun iskee tuohon yleiseen filtterikohtaan "nmap", niin ei saada tulosta, joten täytyy mennä `edit` -> `find packet`. Sitten voi valita hakutavaksi merkkijonon ja hakea "nmap".

<img width="1846" height="823" alt="image" src="https://github.com/user-attachments/assets/53345ee9-7638-44e9-9688-6893bb699ae5" />


Lähdin googlailemaan tuota nmaplowercheckiä, mutten saanut mitään järkevää vastausta, joten oli pakko käänyty tekoälyn puoleen. ChatGPT vastasi, että se on NSE:n tapa tsekata palvelimen käyttäytyminen olemattomien URI:en kanssa luomalla juuri näitä /nmaplowercheckliibalaaba12345 yms. (ChatGPT 2025)

## f) _Net grep_

<img width="1002" height="404" alt="image" src="https://github.com/user-attachments/assets/90a19599-b35e-42e8-8349-1ec5f281d856" />

Jostain syystä tulokset näyttävät olevan kuin salattua kun näyttää vaan `#`, vaikka kyse on salaamattomasta portista 80

## g) _Agentti_

        sudo nmap localhost -A --script-args http.useragent="vaikka Internet Explorer"

## h) _Pienemmät jäljet_

<img width="927" height="372" alt="image" src="https://github.com/user-attachments/assets/0a959b5f-ab4a-4230-81cc-6de3c988c33f" />

User-agent muuttui ja nyt joku voisi luulla että pyynnöt ovat tulleet ihan perus internet explorerista. Tokin tuo /nmaplowercheck123421 on edelleen dead giveaway.

## i) _LoWeR ChEcK_

Jospa nyt vihdoin saatais toi nmaplowercheck piiloon.

<img width="690" height="77" alt="image" src="https://github.com/user-attachments/assets/b3195801-c872-48e7-aaa3-dc6b8905394d" />

Okeei sieltä löytyi mätsi. 

<img width="352" height="74" alt="image" src="https://github.com/user-attachments/assets/996f5897-e961-4d00-aa47-7ba33eb3d1e5" />


<img width="687" height="84" alt="image" src="https://github.com/user-attachments/assets/82d5276b-2e59-4e0b-b74c-d15e86d9e2f5" />

ja sieltä muokataan noiden nimet.

Testaamatta = tekemättä

Apache access loki:

<img width="1026" height="72" alt="image" src="https://github.com/user-attachments/assets/d661597d-959b-4809-bbd2-a31760d87d83" />

ja wireshark:

<img width="442" height="36" alt="image" src="https://github.com/user-attachments/assets/2b9df131-b93c-4dd9-bcff-b853a937c7cd" />

## j) _Invisible, invincible_

Tätä voisin testata vielä palautuksen jälkeen. Ja muutenkin vähän raportin ulkoasua siistiä ja lisätä tekstiä netgrep-kohtaan, mutta nyt täytyy palauttaa tämä versio.

## Lähteet

https://kravensecurity.com/diamond-model-analysis/

https://detect-respond.blogspot.com/2013/03/the-pyramid-of-pain.html

https://httpd.apache.org/docs/2.4/logs.html

https://stackoverflow.com/questions/25608220/what-does-1-mean-in-apache-logs

https://www.rfc-editor.org/rfc/rfc1413

https://nmap.org/book/port-scanning-tutorial.html
