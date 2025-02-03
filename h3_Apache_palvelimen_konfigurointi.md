# Raportti

## Ympäristön kuvaus

- **Päivämäärä ja kellonaika**: 3.2.2025  
- **Laitteisto**: Windows 10 Home, (Intel i7-8700, 32GB RAM, 2 Tb SSD + 2tb HDD)  
- **Virtuaalikoneen asetukset**:  
  - **Käyttöjärjestelmäversio**: Debian 12.9 (Bookworm)  
  - **Asennusmedia**: debian-12.9.0-amd64-netinst.iso  
  - **RAM**: 8GB  
  - **Kiintolevytila**: 20GB (dynaamisesti tilalla)  
  - **Tiedostomuoto**: VDI (Virtual Disk Image)  

## x) Artikkelit

**Ensimmäinen artikkeli: Name-based vs. IP-based Virtual Hosts**  
Nimi-pohjainen isännöinti käyttää verkkosivun nimeä (ei IP-osoitetta) määrittämään, mikä sivu näytetään.  
Yksi IP-osoite voi tukea monia verkkosivuja.  
Apache vertailee ensin IP-osoitteen ja portin perusteella, sitten tarkistaa `ServerName` ja `ServerAlias`.  
On suositeltavaa määrittää aina `ServerName` jokaiseen virtuaali-isäntään.

**Toinen artikkeli: Name Based Virtual Hosts Apachella**  
Apache mahdollistaa useiden verkkosivujen isännöinnin yhdelle IP-osoitteelle nimipohjaisen virtuaali-isännöinnin avulla.  
Artikkelissa käydään läpi, miten Apache asennetaan, määritellään virtuaali-isäntiä ja lisätään verkkosivujen tiedostot.  
Testauksessa käytetään `curl`-komentoa ja `hosts`-tiedoston muokkaamista simuloimaan eri verkkosivujen käyttöä samalla IP-osoitteella.



# Apache-verkkopalvelimen asennus ja konfigurointi


## a) Apache-asennus ja testaus
Asensin Apache-verkkopalvelimen seuraavilla komennoilla:
``` 
sudo apt update
sudo apt install apache2
```
Asensin myös curl-työkalun testauksia varten:
``` 
sudo apt install curl
```
Asennuksen jälkeen testasin, että palvelin vastaa localhost-osoitteesta komennolla:
``` 
curl http://localhost
```
Tämä palautti odotetusti verkkosivun HTML-sisällön.

## b) Etsi lokista rivit, jotka syntyvät, kun lataat omalta palvelimeltasi yhden sivun. Analysoi rivit.
Tarkastelin Apache-palvelimen lokitiedostoa seuraavalla komennolla:
```
sudo tail /var/log/apache2/error.log
```
Seuraavat rivit ilmestyivät lokiin, kun käynnistin palvelimen ja latasin yhden sivun:
```
skibidi/$ sudo tail /var/log/apache2/error.log
[Mon Feb 03 13:44:20.130526 2025] [mpm_event:notice] [pid 7645:tid 7645] AH00492: caught SIGWINCH, shutting down gracefully
[Mon Feb 03 13:44:20.245323 2025] [mpm_event:notice] [pid 11858:tid 11858] AH00489: Apache/2.4.62 (Debian) configured -- resuming normal operations
[Mon Feb 03 13:44:20.245459 2025] [core:notice] [pid 11858:tid 11858] AH00094: Command line: '/usr/sbin/apache2'
[Mon Feb 03 14:53:50.626378 2025] [mpm_event:notice] [pid 11858:tid 11858] AH00492: caught SIGWINCH, shutting down gracefully
[Mon Feb 03 14:53:50.749324 2025] [mpm_event:notice] [pid 12597:tid 12597] AH00489: Apache/2.4.62 (Debian) configured -- resuming normal operations
[Mon Feb 03 14:53:50.749416 2025] [core:notice] [pid 12597:tid 12597] AH00094: Command line: '/usr/sbin/apache2'
[Mon Feb 03 15:27:40.894229 2025] [mpm_event:notice] [pid 12597:tid 12597] AH00493: SIGUSR1 received.  Doing graceful restart
AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using fe80::a00:27ff:fe02:f8c0%enp0s3. Set the 'ServerName' directive globally to suppress this message
[Mon Feb 03 15:27:40.947715 2025] [mpm_event:notice] [pid 12597:tid 12597] AH00489: Apache/2.4.62 (Debian) configured -- resuming normal operations
[Mon Feb 03 15:27:40.947731 2025] [core:notice] [pid 12597:tid 12597] AH00094: Command line: '/usr/sbin/apache2
```
![Image](https://github.com/user-attachments/assets/6cf475e5-276d-443d-a3fe-ea70f01667fd)
Tämä näytti mahdolliset virheet palvelimen toiminnassa.
## Analyysi
```[Mon Feb 03 13:44:20.130526 2025]```: Tämä on aikaleima, joka kertoo, milloin tapahtuma tapahtui.
```AH00489: Apache/2.4.62 (Debian) configured -- resuming normal operations```: Apache on toimintakunnossa.
```[Mon Feb 03 14:53:50.626378 2025] [mpm_event:notice] [pid 11858:tid 11858] AH00492: caught SIGWINCH, shutting down gracefully```: Tässä Apache on taas saanut SIGWINCH-signaalin ja sulkeutuu hallitusti.
(https://stackoverflow.com/questions/780853/what-is-in-apache-2-a-caught-sigwinch-error)
```
graceful shutdown
```
-tarkoittaa, apache lopetti uusien yhteyksien vastaanottamisen, mutta samaan aikaan antoi olemassa olevien yhteyksien valmistua ennen sulkemista
https://camel.apache.org/manual/graceful-shutdown.html

```
Resuming normal operations
```
tarkoittaa, että se on valmis vastaanottamaan pyyntöjä

```[Mon Feb 03 15:27:40.947731 2025] [core:notice] [pid 12597:tid 12597] AH00094: Command line: '/usr/sbin/apache2'``` -Apache on käynnistynyt komennolla /usr/sbin/apache2.

## c) Etusivu uusiksi ja name-based virtual host
Loin hakemistot verkkosivustoille:
``` 
sudo mkdir -p /var/www/example1.local/public_html
sudo mkdir -p /var/www/example2.local/public_html
sudo mkdir -p /var/www/hattu.example.com/public_html
```
Asetin oikeudet hakemistoille:
``` 
sudo chown -R $USER:$USER /var/www/example1.local/public_html
sudo chown -R $USER:$USER /var/www/example2.local/public_html
sudo chown -R $USER:$USER /var/www/hattu.example.com/public_html
sudo chmod -R 755 /var/www
```

Konfigurointi
Loin konfiguraatiotiedostot kullekin verkkosivustolle:
``` 
sudo nano /etc/apache2/sites-available/example1.local.conf
sudo nano /etc/apache2/sites-available/example2.local.conf
sudo nano /etc/apache2/sites-available/hattu.example.com.conf
```
Sisällytin seuraavan sisällön:
### example1.local.conf
```apache
<VirtualHost *:80>
    ServerAdmin webmaster@example1.local
    ServerName example1.local
    DocumentRoot /var/www/example1.local/public_html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
### example2.local.conf
```apache
<VirtualHost *:80>
    ServerAdmin webmaster@example2.local
    ServerName example2.local
    DocumentRoot /var/www/example2.local/public_html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
### hattu.example.com.conf
```apache
<VirtualHost *:80>
    ServerAdmin webmaster@hattu.example.com
    ServerName hattu.example.com
    DocumentRoot /var/www/hattu.example.com/public_html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
## Virtuaali-isäntien käyttöönotto
Otin konfiguraatiot käyttöön:
``` 
sudo a2ensite example1.local.conf
sudo a2ensite example2.local.conf
sudo a2ensite hattu.example.com.conf
```
Poistin oletussivuston:
``` 
sudo a2dissite 000-default.conf
```
Ja käynnistin Apachen uudelleen:
``` 
sudo systemctl reload apache2
```

## Hosts-tiedoston muokkaaminen
Tarkistin ja muokkasin hosts-tiedostoa komennolla:
``` 
sudo nano /etc/hosts
```
Hosts-tiedosto on järjestelmän konfigurointitiedosto, joka vastaa verkkotunnusten (kuten example.com) kääntämisestä IP-osoitteiksi paikallisesti ilman ulkoista DNS-palvelinta. Tähän tiedostoon voidaan lisätä paikallisia nimipalvelumerkintöjä, jotta testipalvelimet ja virtuaali-isännät toimivat oikein.

Lisäsin seuraavat rivit:
```plaintext
127.0.0.1   example1.local
127.0.0.1   example2.local
127.0.0.1   hattu.example.com
```
Tämä mahdollistaa paikallisen nimipalvelun testauksen.

## e) Validin HTML5-sivun luominen
Loin uuden HTML5-sivun palvelimen etusivulle:
``` 
nano /var/www/hattu.example.com/public_html/index.html
```
Ja lisäsin seuraavan sisällön:
```html
<!DOCTYPE html>
<html lang="fi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>hattu.example.com</title>
</head>
<body>
    <h1>Tervetuloa hattu.example.com-sivustolle!</h1>
    <p>Tämä on palvelimen uusi etusivu.</p>
</body>
</html>
```

Tarkistin myös paikallisen palvelimen:
```
curl http://localhost
```

Lisäksi tarkistin, että example1 ja example2 ovat olemassa.
![Image](https://github.com/user-attachments/assets/bd349c6e-d9da-4279-8604-3fcba48f5510)

## f) Testaus curl-komennolla ja response header -analyysi
Testasin verkkosivustojen toimintaa:
``` 
sudo curl -I http://hattu.example.com
```
Tämä palautti seuraavan HTTP-otsaketiedot:
![Image](https://github.com/user-attachments/assets/5b6fde35-3757-437d-b21c-fd5ea9a22594)
```HTTP/1.1 200 OK ``` 200 OK tarkoittaa, että sivu on saatu ilman virheitä (https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/200)
```Date``` Miloin plavelin lähetti vastauksen (https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Date)
```Content type: text/html```kertoo, että palautettu sisältö on HTML-formaatissa. Käytännössä palvelin kertoo, että vastaanotettu data on HTML-sivua. (https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type)
```Content-Length: 154``` Kertoo, kuinka monta tavua on vastauksessa. HTML-sivun koko oli 154 tavua
```Accept-Ranges: bytes:``` Tämä kertoo, että palvelin tukee osittaista tiedostojen lataamista. Sen  hyödyllisyys esintyy suurien tiedostojen, kuten videoiden tai kuvien lataamiseen, koska tieto tulee osissa . (https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept-Ranges)
```Vary: Accept-Encoding:``` kertoo, että palvelin saattaa muuttaa vastauksen sisällön mukaan. (https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Vary)
```ETag:``` ETag (Entity Tag) on tunniste, joka liittyy palautettuun sisältöön. Sen avulla voidaan hallita välimuistia hallinnassa ja se kertoo, onko sisältö muuttunut. Jos sisältö ei ole muuttunut, sivusto voi käyttää välimuistiin tallennettua versiota. https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag

``` 
sudo curl http://hattu.example.com
``` 
![image](https://github.com/user-attachments/assets/6e34587c-e0ec-4849-9f23-993ab82a9f05)

## m) Tehdään Github education pack 👍
## Vaihe 1: Siirry GitHub Education -sivulle
Mene GitHub Education -sivulle osoitteessa: https://education.github.com/pack

## Vaihe 2: Kirjaudu sisään GitHub-tilillesi
Kirjauduin sisään GitHub-tililleni. Jos sinulla ei ole vielä GitHub-tiliä, sinun täytyy luoda sellainen.

## Vaihe 3: Täytin hakemuksen
Täytin hakemuksen GitHub Education Packin saamiseksi. Hakemuksessa ilmoitin, että olen opiskelija, ja liitin mukaan kuvan opiskelijakortistani.
Lähetin kuvan, ja GitHub analysoi sen varmistaakseen, että täytin vaaditut ehdot.
![image](https://github.com/user-attachments/assets/29a6bd80-3a43-48cb-a519-a6b30fa26659)

![image](https://github.com/user-attachments/assets/58f96fbb-4687-4a13-87d5-7579cd05fab6)
![image](https://github.com/user-attachments/assets/e1c479e4-9b8e-4864-9899-b7f752bcdc0c)
## Vaihe 4: Onnistunut lähetys
Kun kuvan analyysi oli valmis, sain ilmoituksen onnistuneesta lähetyksestä.
Vahvistusviestissä mainittiin, että saisin vastauksen hakemukseen viimeistään 10 päivän sisällä.
![image](https://github.com/user-attachments/assets/195a9791-d30b-4a92-b407-f63893037ff6)
## Vaihe 5: Odotusaika
Odotan tällä hetkellä vastausta, joka saapuu GitHubilta arvioidun 10 päivän sisällä.


##Lähteet
https://terokarvinen.com/linux-palvelimet/#h3-hello-web-server
https://httpd.apache.org/docs/2.4/vhosts/name-based.html
https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/
muut lähteet löytyvät tekstistä

