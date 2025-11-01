# Lempiväri: violetti

## x) 

### __TUSKAN PYRAMIDI__

David J. Biancon Tuskan pyramidi kertoo, mihin kannattaa keskittyä tuottaakseen hyökkääjälle mahdollisimman paljon tuskaa. Alimpana pyramidissa ovat hashit ja ip-osoitteet, eli niiden kerääminen ei paljoa hyökkääjää hetkauta, sillä ne ovat helposti muutettavissa -> hyökkäys jatkuu. Keskitasossa on hyökkääjän työkalujen tunnistaminen ja yksittäisten artefaktien (jälkien/havaintojen) spottaaminen. Näiden muuttaminen on jo hyökkääjälle huomattavasti vaivalloisempaa muuttaa, kuin ip-osoitteiden tai hashien muuttaminen. Lopuksi pyramidin huipulla on TTPs (tactics, techniques and procedures). Tällä tasolla isket hyökkääjälle luun kurkkuun tunnistamalla jokaisen liikkeen lokeja silmäilemällä ja vastaamalla ja ennakoimalla niihin.

### _Timanttimalli_

Timanttimallin ideana on helpottaa kyberhyökkäysten analysointia keräämällä tietoa timantin joka kulmasta; hyökkääjästä, kyvykkyydestä, infrasta ja uhrista. Näihin kohtiin kun täyttää mahdollisimman paljon tietoa ja yhdistelee timantin kulmia, niin johan alkaa hommat ratkeamaan. Tällä mallilla kiinnitetään huomiota juurikin isompaan kontekstiin, eikä yksittäisiin havantoihin. Tai paremmin sanottuna tällä saadaan ne yksittäiset havainnot paremmin kontekstiin. 

## a)

    sudo apt-get update
    sudo apt-get install apache2
    sudo systemctl start apache2
    curl localhost
    
<img width="676" height="82" alt="image" src="https://github.com/user-attachments/assets/96ede5a1-7131-4695-83b1-1c417fec2893" />

[Apachen docseista](https://httpd.apache.org/docs/2.4/logs.html
) löytyy infot, kuinka näitä tulkitaan.
Eli ensimmäiseksi on hostin ip-osoite. Mulla näkyi tuossa `::1` 127.0.0.1 sijaan, eli IPv6 localhost-osoite. (Stackoverflow).
Sen perässä ensimmäinen `-` meinaa Apache docsien mukaan, että "_information is not available_". Tässä voisi olla `-` sijaan `RFC 1413` identiteetti ilmoitettuna, mutta oletuksena Apache ei tätä koita edes selvittää, sillä tulos on epäluotettava. [Täältä](https://www.rfc-editor.org/rfc/rfc1413) löytyy lisää `RFC1413`:sta.

## b)

<img width="1198" height="389" alt="image" src="https://github.com/user-attachments/assets/28c18e9a-6ee1-49b6-8634-c6a443cac8cb" />


## d)

<img width="1427" height="535" alt="image" src="https://github.com/user-attachments/assets/6c464d38-ea48-4c09-b74e-f6990fdc726b" />

## e)

Wiresharkissa kun iskee tuohon yleiseen filtterikohtaan "nmap", niin ei saada tulosta, joten täytyy mennä `edit` -> `find packet`. Sitten voi valita hakutavaksi merkkijonon ja hakea "nmap".

<img width="1846" height="823" alt="image" src="https://github.com/user-attachments/assets/53345ee9-7638-44e9-9688-6893bb699ae5" />



<img width="644" height="82" alt="image" src="https://github.com/user-attachments/assets/c81ea86e-691b-4237-ba28-1cf3dc3f1539" />


## Lähteet

https://kravensecurity.com/diamond-model-analysis/

https://detect-respond.blogspot.com/2013/03/the-pyramid-of-pain.html

https://httpd.apache.org/docs/2.4/logs.html

https://stackoverflow.com/questions/25608220/what-does-1-mean-in-apache-logs

https://www.rfc-editor.org/rfc/rfc1413
