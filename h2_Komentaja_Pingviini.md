\## Ympäristön kuvaus  
\- \*\*Päivämäärä ja kellonaika\*\*: 27.1.2025    
\- \*\*Laitteisto\*\*: Windows 10 Home, (Intel i7-8700, 32GB RAM, 2 Tb SSD + 2tb HDD)    
\- \*\*Virtuaalikoneen asetukset\*\*:    
 - \*\*Käyttöjärjestelmäversio\*\*: Debian 12.9 (Bookworm)    
 - \*\*Asennusmedia\*\*: debian-12.9.0-amd64-netinst.iso    
 - \*\*RAM\*\*: 8GB    
 - \*\*Kiintolevytila\*\*: 20GB (dynaamisesti tilalla)    
 - \*\*Tiedostomuoto\*\*: VDI (Virtual Disk Image)    
x) Lue ja tiivistä  
•    Komentorivi on tehokas työkalu Linuxissa.  
•    Sillä voi navigoida tiedostojärjestelmässä, käsitellä tiedostoja ja asentaa ohjelmia.  
•    Tärkeitä komentoja: pwd, ls, cd, nano, mkdir, mv, cp, rmdir, rm, ssh, scp, man, apt-get.  
•    Hyödynnä tabulaattoria ja nuolinäppäimiä tehokkaaseen työskentelyyn.  
•    Muista tärkeät hakemistot kuten /, /home/, /etc/ ja /var/log/.  
•    Käytä sudo-komentoa järjestelmänvalvojan oikeuksilla.  
•    Asenna ohjelmia paketinhallinnan avulla (apt-get).  
Oma kysymys: Artikkelissa mainitaan, että rm-komennolla ei ole roskakoria. Miten tiedostoja voi sitten palauttaa, jos ne on vahingossa poistettu rm-komennolla? Onko olemassa työkaluja tai menetelmiä poistettujen tiedostojen palauttamiseen?  
(https://terokarvinen.com/2020/command-line-basics-revisited/?fromSearch=command%20line%20basics%20revisited)
a) Micro-editorin asentaminen ja käyttö
1\. Päivitys ja asennus:  
Aloitin micro-editorin asentamisen komennolla:  
PICTURE1.PNG
sudo apt-get update
sudo apt-get install micro
Tämä päivitti pakettilistan ja asensi micro-editorin. Asennus sujui ilman virheitä.
2\. Micro-editorin käyttö:  
Avasin micro-editorin komennolla:  
micro  
Kirjoitin editoriin tekstin "tämä on vain testi" ja toisen rivin "Testi?". Tämän jälkeen tallensin tiedoston ja suljin editorin.  
PICTURE2.PNG
b) Apt: Kolmen uuden komentoriviohjelman asennus
1\. Asennus:  
Asensin seuraavat ohjelmat komennolla:  
sudoapt-get install cowsay tree htop  
Tämä komento asentaa kaikki kolme ohjelmaa kerralla.    
PICTURE3.PNG
2\. Testaus:  
\- Cowsay: Suoritin seuraavan komennon:  
cowsay testi  
Komento tulosti tekstin "Hei Tero ja tämän tehtävän tarkastaja :)" lehmän puhekuplassa.  
PICTURE4.PNG
\- Tree: Suoritin seuraavan komennon:  
tree  
Tämä komento listasi hakemistorakenteen puun muodossa.  
PICTURE5.PNG
\- Htop: Avasin htopin komennolla:  
htop  
Tämä komento avasi interaktiivisen prosessinhallinnan.  
PICTURE6.PNG
c) FHS: Tärkeiden kansioiden esittely  
Esittelen tässä tärkeimmät kansiot, jotka on listattu 'Command Line Basics Revisited' kappaleessa 'Important directories'. Työskentelin komentokehotteessa ja haen esimerkkejä kunkin tärkeän kansion sisältämistä tiedostoista tai kansioista.  
Tiedostot ja kansiot  
Käytin komentoa ”ls /bin”, jonka jälkeen näytölle tuli suuri määrä tiedostoja      
PICTURE7.PNG
Tiedostojen keskeltä löytyi minua kiinnostava tiedosto perustuen sen nimeen   
PICTURE8.PNG
Aloitin hakemalla tiedostoa nimeltä 'Cat' ja 'Catman'. Käytin seuraavia komentoja:  
\* \`-l /bin/cat\`   
 Tämä komento näyttää tiedoston /bin/cat yksityiskohtaiset tiedot.  
PICTURE 9.PNG
\* \`file /bin/cat\`
 Komento selvittää, minkä tyyppinen tiedosto /bin/cat on.
\* \`cat /etc/skibidi\`   
PICTURE 10.PNG
 Yritin käyttää \`cat\`-komentoa lukeakseni tiedostoa '/etc/skibidi', mutta sain virheilmoituksen, että tiedostoa ei löytynyt.  
komennolla cat /etc/hostname, terminaali printtas hostname(joka siis oli minun hostname)  
PICTURE 11.PNG
Seuraavaksi tarkistin, oliko 'catman'-komento olemassa omassa Debian-järjestelmässäni. Käytin komentoa:  
\* \`man catman\`   
 Tämä paljasti, että 'catman' on asennettuna järjestelmääni, ja sain avattua sen catman:in manuaalin minulle.  
Mandb ja Catman  
Kävin tarkistamassa, että 'catman' on osa \`mandb\`-ohjelmistoa. 'catman' on työkalu, joka luo tai päivittää esimuotoiltuja man-sivuja ('cat pages'). Näitä käytetään man-sivujen näyttämiseen nopeammin, sillä ne on jo muotoiltu valmiiksi, eikä niitä tarvitse luoda lennossa. Lisätietoja löytyy man-sivulta: (https://man7.org/linux/man-pages/man8/catman.8.html) ja (https://www.ibm.com/docs/en/aix/7.1?topic=c-catman-command).  
Koska \`catman\` oli jo asennettu järjestelmääni, kokeilin seuraavaa komentoa:  
\`sudo catman\`, komennon jälkeen temrinaali kävi kaikka tiedostoja läpi   
PICTURE 12.PNG  
Tämä komento sudo catman päivittää manuaalisivujen tietokannan. catman-komento luo tai päivittää kaikkien käyttäjien man-sivujen esimuotoillut versiot järjestelmässä. Tämä nopeuttaa man-sivujen näyttämistä, koska järjestelmän ei tarvitse muotoilla niitä lennossa jokaiselle käyttäjälle.  
https://man7.org/linux/man-pages/man8/catman.8.html
d) Grep-komennon käyttö
1\. Esimerkki 1:  
Suoritin seuraavan komennon:  
grep -r "bash" /etc  
Tämä komento etsi "bash"-ilmaisua kaikista tiedostoista /etc-hakemistossa ja sen alihakemistoissa. PICTURE 13.PNG
2\. Esimerkki 2:  
Suoritin seuraavan komennon:  
grep "root" /etc/passwd   
PICTURE 14.PNG  
Tämä komento etsi "root"-käyttäjän tiedot /etc/passwd-tiedostosta.
e) Pipe (Putket)
Suoritin seuraavan komennon:  
ls -l | grep "\\.txt"  
\- ls -l listaa kaikki tiedostot yksityiskohtaisesti.  
\- | siirtää tuloksen seuraavalle komennolle.  
\- grep "\\.txt" suodattaa vain ne tiedostot, joiden nimi päättyy ".txt".  
PICTURE 15.PNG  
f) Rauta: Koneen tiedot
1\. Lshw-tiedot:  
Tarkistin, onko "lshw" asennettu komennolla:  
which lshw  
Vastaus oli "/usr/bin/lshw", eli ohjelma on asennettu.  
PICTURE 16.PNG
2\. Käyttö:  
Suoritin seuraavan komennon:  
sudo lshw -short -sanitize  
Komento antoi seuraavat tiedot: (Tässä vaiheessa listataan laitteistotiedot kuten prosessori, muisti ja levy).  
PICTURE 17.PNG
g) Vapaaehtoinen: Lokeista valinta ja analyysi
Suoritin seuraavan komennon:  
sudo journalctl -f  
PICTURE 18.PNG
1\. root :TTY=pts/3 ; PWD=/home ; USER=root ; COMMAND=usr/bin/lshw -short -sanitize  
•    root: Tämä viittaa siihen, että tapahtuma on kirjattu pääkäyttäjän (root) toimesta tai pääkäyttäjän kontekstissa. (https://wiki.debian.org/Root)  
•    TTY=pts/3: TTY kertoo päätelaitteen, jota käytettiin. pts/3 on pseudopääte, joka tyypillisesti viittaa etäyhteydellä (esim. SSH) käytettyyn terminaali-istuntoon. (https://askubuntu.com/questions/66195/what-is-a-tty-and-how-do-i-access-a-tty)  
•    PWD=/home: PWD (Print Working Directory) näyttää nykyisen työhakemiston, joka oli /home komennon suoritushetkellä. Se ei kuitenkaan välttämättä ole sama kuin pääkäyttäjän kotihakemisto, joka on yleensä /root. (https://en.wikipedia.org/wiki/Pwd)  
•    USER=root: Vahvistaa, että käyttäjä, joka komennon suoritti, oli root. (https://wiki.debian.org/Root)  
•    COMMAND=/usr/bin/lshw -short -sanitize: Tämä on varsinainen suoritettu komento. lshw on työkalu, joka näyttää tietoja järjestelmän laitteistosta (https://packages.debian.org/buster/lshw). -short -optio pyytää lyhyempää tulostetta, ja -sanitize poistaa tulosteesta mahdollisesti arkaluonteisia tietoja, kuten sarjanumerot ja IP-osoitteet.   
Tulkinta: Tämä rivi kertoo, että pääkäyttäjä on suorittanut lshw-komennon etäyhteydellä käytetystä terminaali-istunnosta. Todennäköisesti pääkäyttäjä on tarkastellut järjestelmän laitteistokokoonpanoa tiivistetyssä muodossa ja varmistanut, ettei arkaluonteisia tietoja paljasteta.  
2\. packagekit.service: Consumed 1.882s CPU time.  
•    packagekit.service: Tämä viittaa PackageKit-palveluun. PackageKit on järjestelmänlaajuinen ohjelmistopakettien hallintatyökalu, joka tarjoaa yhtenäisen rajapinnan eri paketinhallintajärjestelmille. (https://wiki.debian.org/PackageKit)  
•    Consumed 1.882s CPU time: Tämä kertoo, että PackageKit-palvelu on käyttänyt prosessoriaikaa 1.882 sekuntia.  
PICTURE 19.PNG  
h) Vapaaehtoinen: Micro-editorin plugin  
Tässä tehtävässä asensin ja kokeilin palletero-nimistä pluginia micro-editorille. Seuraavassa on kuvattu prosessi vaihe vaiheelta:  
1\. Pluginin asennus:  
•    micro komento terminaalissa: Avaa micro-editorin terminaalissa kirjoittamalla komennon micro.  
•    micron sisällä painetaan CTRL + E: Tämä avaa micro-editorin sisäisen komentotilan.  
•    Varmistus plugin-tuen olemassaolosta: Komentorivillä pitäisi näkyä teksti, joka ilmaisee, että plugin-tuki on ladattu (esim. "Plugin support enabled").  
•    Pluginin asennus: Asennetaan palletero-plugin komennolla:   
•    plugin install palletero  
•    Tarkistus: Tarkistin, että plugin on onnistuneesti ladattu komennolla plugin list   
PICTURE 20.PNG
2\. Pluginin käyttö:  
•    Ctrl-SPACE micro-editorissa: Paina Ctrl-SPACE näppäinyhdistelmää micro-editorissa. Tämä avaa palletero-pluginin komentorivin. (https://github.com/terokarvinen/palettero)
•    Komentorivin sisältö: Komentorivillä on noin 117 riviä erilaisia komentoja ja asetuksia.  
•    Väriteeman muokkaus: palletero-pluginillä voi muokata monia asioita, mutta tässä tapauksessa keskityin väriteeman muokkaamiseen. Väriteema-asetus löytyy:   
o    Skrollaamalla: Komentorivin pohjalta löytyvät halutut väriasetukset, jonne voi skrollata, mutta suosittelen navigoida nuolinäppäimillä.  
o    Kirjoittamalla: Komennon colorscheme komentoriville, jolloin asetus löytyy listan yläpäästä.  
•    Väriteeman valinta: Kun oikea väriteema on kohdalla (navigointi nuolinäppäimillä tai kirjoittamalla), valitaan se painamalla Enter. Valitsin väriteemaksi sunny-day tähän talvisen harmauden kompensoimiseksi.  
PICTURE 22.PNG PICTURE 23.PNG "
