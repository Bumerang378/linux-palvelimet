# Raportti

## Ympäristön kuvaus

- **Päivämäärä ja kellonaika**: 27.1.2025  
- **Laitteisto**: Windows 10 Home, (Intel i7-8700, 32GB RAM, 2 Tb SSD + 2tb HDD)  
- **Virtuaalikoneen asetukset**:  
  - **Käyttöjärjestelmäversio**: Debian 12.9 (Bookworm)  
  - **Asennusmedia**: debian-12.9.0-amd64-netinst.iso  
  - **RAM**: 8GB  
  - **Kiintolevytila**: 20GB (dynaamisesti tilalla)  
  - **Tiedostomuoto**: VDI (Virtual Disk Image)  

---

### x) Lue ja tiivistä

- Komentorivi on tehokas työkalu Linuxissa.
- Sillä voi navigoida tiedostojärjestelmässä, käsitellä tiedostoja ja asentaa ohjelmia.
- Tärkeitä komentoja: pwd, ls, cd, nano, mkdir, mv, cp, rmdir, rm, ssh, scp, man, apt-get.
- Hyödynnä tabulaattoria ja nuolinäppäimiä tehokkaaseen työskentelyyn.
- Muista tärkeät hakemistot kuten /, /home/, /etc/ ja /var/log/.
- Käytä sudo-komentoa järjestelmänvalvojan oikeuksilla.
- Asenna ohjelmia paketinhallinnan avulla (apt-get).

Oma kysymys: Artikkelissa mainitaan, että rm-komennolla ei ole roskakoria. Miten tiedostoja voi sitten palauttaa, jos ne on vahingossa poistettu rm-komennolla? Onko olemassa työkaluja tai menetelmiä poistettujen tiedostojen palauttamiseen?  
(https://terokarvinen.com/2020/command-line-basics-revisited/?fromSearch=command%20line%20basics%20revisited)

---

### a) Micro-editorin asentaminen ja käyttö

1. **Päivitys ja asennus**:  
   Aloitin micro-editorin asentamisen komennolla:

   ![Image](https://github.com/user-attachments/assets/e580d1b7-c1be-41e5-abc8-6d7a1d60f5e9)

sudo apt-get update sudo apt-get install micro

Tämä päivitti pakettilistan ja asensi micro-editorin. Asennus sujui ilman virheitä.

2. **Micro-editorin käyttö**:  
Avasin micro-editorin komennolla:

micro

Kirjoitin editoriin tekstin "tämä on vain testi" ja toisen rivin "Testi?". Tämän jälkeen tallensin tiedoston ja suljin editorin.

![Image](https://github.com/user-attachments/assets/71856b7c-c250-4625-920c-0b7eaedc1743)

---

### b) Apt: Kolmen uuden komentoriviohjelman asennus

1. **Asennus**:  
Asensin seuraavat ohjelmat komennolla:
sudo apt-get install cowsay tree htop

Tämä komento asentaa kaikki kolme ohjelmaa kerralla.

![Image](https://github.com/user-attachments/assets/7b810382-0961-4d3f-8e93-d3b4c6866b67)

2. **Testaus**:
- **Cowsay**: Suoritin seuraavan komennon:

  ```
  cowsay testi
  ```

  Komento tulosti tekstin "Hei Tero ja tämän tehtävän tarkastaja :)" lehmän puhekuplassa.

  ![Image](https://github.com/user-attachments/assets/69ce64b5-c08d-4949-8bb0-19a988bca441)

- **Tree**: Suoritin seuraavan komennon:

  ```
  tree
  ```

  Tämä komento listasi hakemistorakenteen puun muodossa.

  ![Image](https://github.com/user-attachments/assets/5d72a58a-5637-4a51-b85a-afc979d8b693)

- **Htop**: Avasin htopin komennolla:

  ```
  htop
  ```

  Tämä komento avasi interaktiivisen prosessinhallinnan.

  ![Image](https://github.com/user-attachments/assets/a24f63fd-c816-4cb3-9a71-c9772e6ec762)

---

### c) FHS: Tärkeiden kansioiden esittely

Esittelen tässä tärkeimmät kansiot, jotka on listattu 'Command Line Basics Revisited' kappaleessa 'Important directories'. Työskentelin komentokehotteessa ja haen esimerkkejä kunkin tärkeän kansion sisältämistä tiedostoista tai kansioista.

- **Tiedostot ja kansiot**  
Käytin komentoa ”ls /bin”, jonka jälkeen näytölle tuli suuri määrä tiedostoja.

![Image](https://github.com/user-attachments/assets/82ed5a2c-c087-41cf-83d1-19b69ca4b957)

Tiedostojen keskeltä löytyi minua kiinnostava tiedosto perustuen sen nimeen.

![Image](https://github.com/user-attachments/assets/26cb46f2-f75e-4f03-b18a-cb7dfda35632)

Aloitin hakemalla tiedostoa nimeltä 'Cat' ja 'Catman'. Käytin seuraavia komentoja:
-l /bin/cat

Tämä komento näyttää tiedoston /bin/cat yksityiskohtaiset tiedot.

![Image](https://github.com/user-attachments/assets/59e45065-70a8-4565-9f85-fd0af38eb02b)
file /bin/cat

Komento selvittää, minkä tyyppinen tiedosto /bin/cat on.
cat /etc/skibidi

![Image](https://github.com/user-attachments/assets/63c9c101-e855-48c1-a62b-e65344310af4)

Yritin käyttää `cat`-komentoa lukeakseni tiedostoa '/etc/skibidi', mutta sain virheilmoituksen, että tiedostoa ei löytynyt. Komennolla `cat /etc/hostname`, terminaali printtas hostname (joka siis oli minun hostname).

![Image](https://github.com/user-attachments/assets/c227e1c5-f15a-4b94-b6c4-db37e9b4e087)

Seuraavaksi tarkistin, oliko 'catman'-komento olemassa omassa Debian-järjestelmässäni. Käytin komentoa:
man catman

Tämä paljasti, että 'catman' on asennettuna järjestelmääni, ja sain avattua sen catmanin manuaalin minulle.

**Mandb ja Catman**  
Kävin tarkistamassa, että 'catman' on osa `mandb`-ohjelmistoa. 'catman' on työkalu, joka luo tai päivittää esimuotoiltuja man-sivuja ('cat pages'). Näitä käytetään man-sivujen näyttämiseen nopeammin, sillä ne on jo muotoiltu valmiiksi, eikä niitä tarvitse luoda lennossa. Lisätietoja löytyy man-sivulta: (https://man7.org/linux/man-pages/man8/catman.8.html) ja (https://www.ibm.com/docs/en/aix/7.1?topic=c-catman-command).

Koska `catman` oli jo asennettu järjestelmääni, kokeilin seuraavaa komentoa:
sudo catman

Komennon jälkeen terminaali kävi kaikkia tiedostoja läpi.

![Image](https://github.com/user-attachments/assets/2e9e7d34-d9ed-4678-b272-ea59907f29e4)

Tämä komento `sudo catman` päivittää manuaalisivujen tietokannan. `catman`-komento luo tai päivittää kaikkien käyttäjien man-sivujen esimuotoillut versiot järjestelmässä. Tämä nopeuttaa man-sivujen näyttämistä, koska järjestelmän ei tarvitse muotoilla niitä lennossa jokaiselle käyttäjälle.

https://man7.org/linux/man-pages/man8/catman.8.html

---

### d) Grep-komennon käyttö

1. **Esimerkki 1**:  
 Suoritin seuraavan komennon:
grep -r "bash" /etc

Tämä komento etsi "bash"-ilmaisua kaikista tiedostoista /etc-hakemistossa ja sen alihakemistoissa.

![Image](https://github.com/user-attachments/assets/444f6d3f-cec1-4648-82d2-ae58e7f907bc)

2. **Esimerkki 2**:  
Käytin komentoa:
grep "error" /var/log/syslog

Tämä komento etsi "error"-ilmaisua järjestelmän lokitiedostosta /var/log/syslog.

![Image](https://github.com/user-attachments/assets/7b6cd78a-0b84-47da-92ba-8f1c73f0e11f)

`grep` on erittäin tehokas työkalu tiedostojen sisällön hakemiseen. Sitä voi käyttää myös säännöllisten lausekkeiden kanssa.

---

## e) Pipe (Putket)

Suoritin seuraavan komennon:
```bash
ls -l | grep "\.txt"
```
- `ls -l` listaa kaikki tiedostot yksityiskohtaisesti.
- `|` siirtää tuloksen seuraavalle komennolle.
- `grep "\.txt"` suodattaa vain ne tiedostot, joiden nimi päättyy ".txt".

![PICTURE 15.PNG](PICTURE%2015.PNG)

## f) Rauta: Koneen tiedot

### 1. Lshw-tiedot:
Tarkistin, onko "lshw" asennettu komennolla:
```bash
which lshw
```
Vastaus oli `/usr/bin/lshw`, eli ohjelma on asennettu.

![PICTURE 16.PNG](PICTURE%2016.PNG)

### 2. Käyttö:
Suoritin seuraavan komennon:
```bash
sudo lshw -short -sanitize
```
Komento antoi seuraavat tiedot: (Tässä vaiheessa listataan laitteistotiedot kuten prosessori, muisti ja levy).

![PICTURE 17.PNG](PICTURE%2017.PNG)

## g) Vapaaehtoinen: Lokeista valinta ja analyysi

Suoritin seuraavan komennon:
```bash
sudo journalctl -f
```

![PICTURE 18.PNG](PICTURE%2018.PNG)

### 1. root :TTY=pts/3 ; PWD=/home ; USER=root ; COMMAND=usr/bin/lshw -short -sanitize
- **root**: Viittaa siihen, että tapahtuma on kirjattu pääkäyttäjän (root) toimesta tai pääkäyttäjän kontekstissa. [(Lähde)](https://wiki.debian.org/Root)
- **TTY=pts/3**: TTY kertoo päätelaitteen, jota käytettiin. `pts/3` on pseudopääte, joka tyypillisesti viittaa etäyhteydellä (esim. SSH) käytettyyn terminaali-istuntoon. [(Lähde)](https://askubuntu.com/questions/66195/what-is-a-tty-and-how-do-i-access-a-tty)
- **PWD=/home**: PWD (Print Working Directory) näyttää nykyisen työhakemiston, joka oli `/home` komennon suoritushetkellä. [(Lähde)](https://en.wikipedia.org/wiki/Pwd)
- **USER=root**: Vahvistaa, että käyttäjä, joka komennon suoritti, oli root. [(Lähde)](https://wiki.debian.org/Root)
- **COMMAND=/usr/bin/lshw -short -sanitize**: Suoritettu komento, joka näyttää tietoja järjestelmän laitteistosta. [(Lähde)](https://packages.debian.org/buster/lshw)

Tulkinta: Pääkäyttäjä on suorittanut `lshw`-komennon etäyhteydellä käytetystä terminaali-istunnosta. Todennäköisesti hän on tarkastellut järjestelmän laitteistokokoonpanoa tiivistetyssä muodossa ja varmistanut, ettei arkaluonteisia tietoja paljasteta.

### 2. packagekit.service: Consumed 1.882s CPU time.
- **packagekit.service**: Viittaa PackageKit-palveluun, joka on ohjelmistopakettien hallintatyökalu. [(Lähde)](https://wiki.debian.org/PackageKit)
- **Consumed 1.882s CPU time**: PackageKit-palvelu on käyttänyt prosessoriaikaa 1.882 sekuntia.

![PICTURE 19.PNG](PICTURE%2019.PNG)

## h) Vapaaehtoinen: Micro-editorin plugin

Tässä tehtävässä asensin ja kokeilin `palletero`-nimistä pluginia micro-editorille. Seuraavassa on kuvattu prosessi vaihe vaiheelta:

### 1. Pluginin asennus:
- **Micro komento terminaalissa**: Avaa micro-editorin terminaalissa komennolla:
  ```bash
  micro
  ```
- **CTRL + E**: Avaa micro-editorin sisäisen komentotilan.
- **Varmistus plugin-tuen olemassaolosta**: Komentorivillä tulisi näkyä teksti, joka ilmaisee, että plugin-tuki on ladattu (esim. `Plugin support enabled`).
- **Pluginin asennus**:
  ```bash
  plugin install palletero
  ```
- **Tarkistus**:
  ```bash
  plugin list
  ```

![PICTURE 20.PNG](PICTURE%2020.PNG)

### 2. Pluginin käyttö:
- **Ctrl-SPACE micro-editorissa**: Paina `Ctrl-SPACE` näppäinyhdistelmää micro-editorissa. [(Lähde)](https://github.com/terokarvinen/palettero)
- **Komentorivin sisältö**: Komentorivillä on noin 117 riviä erilaisia komentoja ja asetuksia.
- **Väriteeman muokkaus**:
  - **Skrollaamalla**: Komentorivin pohjalta löytyvät halutut väriasetukset.
  - **Kirjoittamalla**: Komennon `colorscheme` avulla asetuksen voi hakea listan yläpäästä.
- **Väriteeman valinta**: Kun oikea väriteema on kohdalla, valitaan se painamalla `Enter`. Valitsin väriteemaksi `sunny-day` talvisen harmauden kompensoimiseksi.

![PICTURE 22.PNG](PICTURE%2022.PNG) ![PICTURE 23.PNG](PICTURE%2023.PNG)
