# Debianin asentaminen Oracle VM VirtualBoxiin ja LibreWolf-selaimen asentaminen

## Tiivistelmä
Asensin Oracle VM VirtualBox -ohjelmaan Debian 12 -käyttöjärjestelmän (debian-12.9.0-amd64-netinst.iso). Järjestelmä asennettiin vaiheittain. Lopuksi asensin LibreWolf-selaimen terminaalin avulla. Raportissa kuvailen asennusprosessin yksityiskohtaisesti.

## Ympäristön kuvaus
- **Päivämäärä ja kellonaika**: 14.1.2025  
- **Laitteisto**: Windows 10 Home, (Intel i7, 32GB RAM, 2 Tb SSD + 2tb HDD)  
- **Virtuaalikoneen asetukset**:  
  - **Käyttöjärjestelmäversio**: Debian 12.9 (Bookworm)  
  - **Asennusmedia**: debian-12.9.0-amd64-netinst.iso  
  - **RAM**: 8GB  
  - **Kiintolevytila**: 20GB (dynaamisesti tilalla)  
  - **Tiedostomuoto**: VDI (Virtual Disk Image)  

## Asennuksen vaiheet ja tulokset

### Vaihe 1: Virtuaalikoneen luominen Oracle VM VirtualBoxissa

1. **Virtuaalikoneen luominen**:  
   - Nimesin koneen *(System name)*.  
   - Valitsin käyttöjärjestelmäksi Linux ja versioksi Debian (64-bit) (Opettaja suositteli).  
   - Määritin muistimääräksi 8GB RAM, jotta käyttöjärjestelmä toimisi sujuvasti. Tulevaisuudessa sen määrää voi muokata.  

2. **Kiintolevyn luominen**:  
   - Valitsin vaihtoehdon *Create a virtual hard disk now*, koska en ollut aiemmin käyttänyt Debian-pohjaista virtuaalikonetta.  
   - Tiedostotyypiksi valitsin VDI (Virtual Disk Image).  
   - Valitsin *Dynamically allocated*, jotta levytila laajenee tarpeen mukaan.  
   - Määritin kiintolevyn koon 20GB ja tallensin tiedoston nimellä `Debian.vdi` kiintolevylleni.  

3. **Virtuaalikoneen käynnistäminen**:  
   - Valitsin käynnistyslevyksi ladatun `debian-12.9.0-amd64-netinst.iso`-tiedoston.  
   - Valitsin asennustavaksi *Graphical Install*.  

### Vaihe 2: Debianin asennus

1. **Käyttöjärjestelmän asetukset**:  
   - Kieleksi valitsin *englanti (English)*.  
   - Sijainniksi valitsin *Suomi*.  
   - *Locales*-asetuksissa valitsin *en_US.UTF-8*.  
   - Näppäimistöksi valitsin *suomi (Finnish)*.  

2. **Verkkoasetukset**:  
   - Määritin *Hostname*-kenttään (User name) nimen. Domain-nimen jätin tyhjäksi.  

3. **Käyttäjien luominen**:  
   - Syötin root-salasanan ja loin uuden käyttäjätilin.  

4. **Levyosion luominen**:  
   - Valitsin *Use entire disk* ja asetin kaikki tiedostot yhteen osioon (*All files in one partition*), kuten järjestelmä suositteli.  

5. **Paketinhallinnan asetukset**:  
   - Valitsin pääpalvelimeksi *deb.debian.org*.  
   - *Popularity-contestiin* vastasin "No".  

6. **Ohjelmistovalinnat**:  
   - Valitsin seuraavat paketit:  
     - Gnome (työpöytäympäristö)  
     - Debian desktop environment  
     - Web server (opettaja mainitsi asiasta tunnilla)  
     - SSH server (opettaja mainitsi asiasta tunnilla)  
     - Standard system utilities  

7. **Grub Boot Loader**:  
   - Valitsin *Yes*, jotta käynnistyslatausohjelma asennetaan.  

8. **Asennuksen viimeistely**:  
   - Käynnistin järjestelmän uudelleen ja testasin sen toiminnan. Kaikki vaikutti toimivan moitteettomasti.  
   - Uudelleen käynnistin järjestelmän ja varmistin, että kaikki toimi.  
 ![Image](https://github.com/user-attachments/assets/086d9f15-548a-438a-970d-d54efcdbb025)

### Vaihe 3: LibreWolf-selaimen asentaminen

1. **LibreWolfin asennus terminaalissa**:  
   - Käynnistin Firefox-selaimen ja avasin LibreWolfin asennusohjeet.  
   - Suoritin seuraavat komennot terminaalissa:
     ```bash
     sudo apt update && sudo apt install extrepo -y
     sudo extrepo enable librewolf
     sudo apt update && sudo apt install librewolf -y
     ```
   - Terminaali pyysi root-oikeuksia, jotka annoin suorittamalla *su*-komennon.  

2. **LibreWolfin toimivuus**:  
   - Asennuksen jälkeen LibreWolf käynnistyi ilman ongelmia.  

## Havainnot ja johtopäätökset

- Debianin asentaminen Oracle VM VirtualBoxiin onnistui selkeiden vaiheiden avulla.  
- LibreWolf-selaimen asennus oli suoraviivainen, ja ohjeet verkkosivustolla olivat hyödyllisiä.  
- Virtuaalikone toimi odotetusti, mutta suorituskyky oli hieman hidas, mikä on odotettavissa virtuaalikoneympäristössä.  

## Neljä GNU:n vapautta

1. **Vapaus käyttää ohjelmistoa mihin vaan tarkoitukseen (Freedom 0)**:  
   Käyttäjä voi käyttää ohjelmistoa vapaasti mihin tahansa tarkoitukseen. Käyttäjä ei ole sidottu ohjelmiston käyttöön mihinkään erityiseen kontekstiin tai rajoitukseen.

2. **Vapaus tutkia ohjelmiston sisäistä rakennetta ja muokata sitä (Freedom 1)**:  
   Käyttäjällä on oikeus tutkia ohjelmiston lähdekoodia ja ymmärtää sen toimintaa. Käyttäjä voi myös tehdä muutoksia ohjelmistoon ja myös muokata sitä omiin tarpeisiinsa. Tämä vapaus edistää oppimista ja mahdollistaa ohjelmiston kehittämistä.

3. **Vapaus jakaa ohjelmistoa eteenpäin (Freedom 2)**:  
   Käyttäjä saa jakaa ohjelmiston vapaasti eteenpäin, olipa kyseessä alkuperäinen versio tai muokattu versio. Tällöin ohjelmisto voi levitä ja kehittyä yhteisön tukemana, ja jokainen voi hyötyä ohjelmiston käytöstä.

4. **Vapaus jakaa ohjelmistoa muokattuna versiona (Freedom 3)**:  
   Tämä vapaus antaa käyttäjälle oikeuden jakaa ohjelmistoa muokattuna versiona. Tällöin ohjelmistoon tehtävät parannukset, korjaukset ja uudet ominaisuudet voivat hyödyttää laajempaa yhteisöä.  

## Viittaukset / Lähteet
- [Debian: Debianin virallinen sivusto](https://www.debian.org/)
- [Oracle VM VirtualBox: VirtualBoxin virallinen sivusto](https://www.virtualbox.org/)
- [LibreWolf: LibreWolfin asennusohjeet Debianille](https://librewolf.net/installation/debian/)
- [Ionos Digital Guide: Debian version tarkastus](https://www.ionos.com/digitalguide/server/know-how/how-to-check-debian-version/)
- [Neljä GNU:n vapautta: GNU Operating System](https://www.gnu.org/philosophy/free-sw.html)

## Vapaaehtoinen bonus: suosikkiohjelmani Linuxilla
Yksi ensimmäisistä ohjelmista, jonka huomasin Debianilla, oli **2048**-peli, joka oli valmiiksi asennettuna ja löytyi nopeasti sovellusvalikosta. Pelissä tavoitteena on yhdistää samanlaisia numeroita, jotta pääsee korkeampiin arvoihin. Pelasin peliä noin 20 minuutin ajan täysin unohtaen kaiken muun ympäriltäni. Peli oli hauska ja koukuttava.



![Image](https://github.com/user-attachments/assets/5358fc47-e658-40b6-9376-ec7891042e4d)

