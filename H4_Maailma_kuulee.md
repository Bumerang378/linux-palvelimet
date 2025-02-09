# Raportti H4 MAAILMA KUULEEE 😲

## Ympäristön kuvaus

- **Päivämäärä**: 9.2.2025  
- **Laitteisto**: Windows 10 Home, (Intel i7-8700, 32GB RAM, 2 Tb SSD + 2tb HDD)  
- **Virtuaalikoneen asetukset**:  
  - **Käyttöjärjestelmäversio**: Debian 12.9 (Bookworm)  
  - **Asennusmedia**: debian-12.9.0-amd64-netinst.iso  
  - **RAM**: 8GB  
  - **Kiintolevytila**: 20GB (dynaamisesti tilalla)  
  - **Tiedostomuoto**: VDI (Virtual Disk Image)  

### x)
Artikkeli 1
- Artikkeli käsittelee pilvipalvelimen määrittelyä ja sen asennusta.
Se sisältää vaiheet pilvipalvelimen luomiseen, web-palvelimen asentamiseen ja palvelimen suojaamiseen.

Kysymyksiin vastaukset:

- a) Pilvipalvelimen vuokraus ja asennus: Pilvipalvelimen voi vuokrata useista eri palveluista, kuten Amazon Web Services (AWS) tai Microsoft Azure. Asennusprosessi vaihtelee palveluntarjoajan mukaan.
- d) Palvelin suojaan palomuurilla: Palomuuri on tärkeä työkalu palvelimen suojaamiseen hakkereilta. Se voidaan asentaa joko palvelimelle tai erilliseen laitteeseen.
- e) Kotisivut palvelimelle: Kotisivut voidaan asentaa palvelimelle useilla eri ohjelmistoilla, kuten Apache tai Nginx.
- f) Palvelimen ohjelmien päivitys: On tärkeää pitää palvelimen ohjelmat ajan tasalla suojaamiseksi tietoturvaongelmilta.

Artikkeli 2

- Artikkeli käsittelee virtuaalisen yksityisen palvelimen (VPS) määrittelyä ja asennusta.
Se sisältää vaiheet VPS:n luomiseen, käyttäjän luomiseen ja suojaamiseen, ohjelmistojen päivittämiseen ja DNS-nimen lisäämiseen.

## Virtuaalipalvelimen käyttöönotto ja konfigurointi [Kohdat a - c]
### a) Vuokraa oma virtuaalipalvelin
Valitsin palvelimen sijainniksi Suomen, koska se helpottaa lainmukaisten vaatimusten täyttämistä ja tarjoaa paremman yhteysnopeuden käyttäjille.
- ![image](https://github.com/user-attachments/assets/77547f76-16c7-49bb-8f6b-9174946bbb3e)

#### Palvelimen tekniset tiedot:
- 1 ydin
- 1 GB RAM
- 10 GB tallennustilaa
- Hinta: 3,00 €/kk
![image](https://github.com/user-attachments/assets/a0eca52a-1570-46ce-aae0-60c84b96e266)

Käyttöjärjestelmäksi valitsin Debianin sen vakauden ja yhteensopivuuden kurssin kanssa.
![image](https://github.com/user-attachments/assets/ee77541a-4ce7-410c-95a1-5734a772a561)

#### Verkkokonfiguraatio:
- IPv4 käytössä
- Utility Network käytössä
- Julkinen IPv6 käytössä
![image](https://github.com/user-attachments/assets/5e71f44d-5c20-42f5-aa02-f80e81dc5894)

Kirjautumismenetelmäksi valitsin SSH-avaimen, joka parantaa tietoturvaa verrattuna salasanoihin.
![image](https://github.com/user-attachments/assets/7543d470-97bb-4b78-b43d-534994ecd040)

### b) Tee alkutoimet omalla virtuaalipalvelimellasi

#### 1. SSH-avaimen luonti ja käyttönotto
Ensimmäiseksi asensin OpenSSH-clientin komennolla:
```
sudo apt-get install openssh-client
```
![image](https://github.com/user-attachments/assets/c4838977-ddaa-4625-a377-ff1547de0422)

Tämän jälkeen loimme SSH-avaimen komennolla:
```
ssh-keygen
```
![image](https://github.com/user-attachments/assets/e82679f0-4b8e-48a9-bae1-c266d3d177bc)

Prosessin aikana jätin salasanan tyhjäksi, sillä palvelimen oletussuojaukset ovat riittävät. Avaimen luonnin jälkeen siirryin `.ssh`-hakemistoon ja tarkistin julkisen avaimen:
![image](https://github.com/user-attachments/assets/152b494c-7c59-4e2f-9b60-0c5bbf7e7af0)
```
cd .ssh
cat id_rsa.pub
```
![image](https://github.com/user-attachments/assets/7017567a-f183-4cc9-989a-b15f794e759b)

```
cat id_rsa.pub
```
![image](https://github.com/user-attachments/assets/865c99c2-2c80-455b-b557-17f8982ce54c)
Kopioin tämän avaimen ja lisäsin sen UpCloudin SSH Key -asetuksiin. Tämän jälkeen palvelin käynnistettiin **Deploy**-napilla.

![image](https://github.com/user-attachments/assets/72baa809-fc72-4f52-9cab-282076d19fba)
![image](https://github.com/user-attachments/assets/047b2dd1-c6ca-49b6-b058-7261cdb1c2a2)

#### 2. Palvelimelle kirjautuminen
Kun palvelin oli otettu käyttöön, yhdistyin siihen SSH:n avulla:
```
ssh root@94.237.116.121
```
![image](https://github.com/user-attachments/assets/095b496f-d7d8-4486-8ca7-0f414676fef0)


Kirjoitin "yes", jotta yhteys muodostui ja tallennettiin tunnetuksi isännäksi.

#### 3. Uuden käyttäjän luonti ja root-oikeuksien poistaminen
Loin uuden käyttäjän komennolla:
```
sudo adduser skibidi
```
Asetin salasanan ja täytin pyydetyt käyttäjätiedot. Lisäsin käyttäjän sudo-ryhmään:
```
sudo adduser skibidi sudo
```
Kopioin rootin SSH-asetukset uudelle käyttäjälle:
```
sudo cp -n -r /root/.ssh /home/skibidi
sudo chown -R skibidi:skibidi /home/skibidi/.ssh
```
Kirjauduin uudella käyttäjällä palvelimelle:
```
ssh skibidi@94.237.116.121
```
![image](https://github.com/user-attachments/assets/d8c11bd2-1eda-41ec-9c91-a6ebcac03090)

Tarkastin, että kaikki toimii komennolla.
```
sudo echo (oma viesti)
```
![image](https://github.com/user-attachments/assets/a5ab3147-6692-4577-aca3-5d5b9315f263)

Suljin root-tunnuksen komennolla:
```
sudo usermod --lock root
```
![image](https://github.com/user-attachments/assets/0db588a1-af10-46a6-b5f3-bbbb9271aa90)

Poistin rootin SSH-hakemiston:
```
sudo rm -r /root/.ssh
```
![image](https://github.com/user-attachments/assets/7e8a35fa-cf39-46b4-a15d-b2c099954697)

Tarkistin, ettei root-käyttäjällä ollut enää pääsyä palvelimeen.
![image](https://github.com/user-attachments/assets/4e7b8efe-19af-40b3-95cc-3d0e65b0dc2a)

#### 4. Palomuurin käyttöönotto
Asensin ja otin käyttöön UFW-palomuurin:
```
sudo apt-get install ufw
sudo ufw allow 22/tcp
sudo ufw enable
```
![image](https://github.com/user-attachments/assets/5e67ec06-912a-439e-b0ea-7e5ccbecd814)

Tarkistin avoimet portit komennolla:
```
sudo ufw status verbose
```
![image](https://github.com/user-attachments/assets/66be8da9-f35a-431f-b279-08881b8d4f1c)

### c) Asenna webpalvelin ja testaa

#### 1. Apache2-webpalvelimen asennus ja testisivun luonti
Asensin Apache2-palvelimen komennolla:
```
sudo apt-get install apache2
```
Avasin HTTP-liikenteen portin 80:
```
sudo ufw allow 80/tcp
```
Tarkistin palvelimen julkisen IP-osoitteen:
```
hostname -I
```
![image](https://github.com/user-attachments/assets/863a9362-80eb-4850-b9f3-e567161e741b)

Loin testisivun komennolla:
```
echo 'Moikka moii!' | sudo tee /var/www/html/index.html
```
![image](https://github.com/user-attachments/assets/7a27892f-e27f-47ed-832d-846e13071d65)

Tarkistin sivun toiminnan avaamalla sen virtuaalikoneenselaimessa, sekä kokeilemalla yhteyttä puhelimen kautta.
![image](https://github.com/user-attachments/assets/90854b74-b77f-49aa-b097-2ed7c1cda7df)
![image](https://github.com/user-attachments/assets/935bc983-544e-45f1-8383-a0113dd33b46)



### LÄHTEET
https://terokarvinen.com/linux-palvelimet/#h4-maailma-kuulee
https://susannalehto.fi/2022/teoriasta-kaytantoon-pilvipalvelimen-avulla-h4/
https://terokarvinen.com/2017/first-steps-on-a-new-virtual-private-server-an-example-on-digitalocean/
