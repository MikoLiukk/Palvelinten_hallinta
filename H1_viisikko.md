# H1 - Viisikko

## x) Lue ja tiivistä. 
- Salt:tia käytetään yleensä hallinnoimaan suuria määriä slave-koneita verkon yli
- Tärkeimmät tilafunktiot ovat: pkg, file, service, user ja cmd
- Slave koneita voi ohjata vaikka olisivatkin NAT tai palomuurin takana.
- Asentamisen jälkeen täytyy:
  - Konfiguroida Master ja Slave
  - Aloittaa palvelut
  - Hyväksyä slave-avaimet
  - Verifoida asennus
  - Asentaa mahdolliset riippuvuudet.
- Raportin on oltava toistettava, täsmällinen, helppolukuinen ja viitatta lähteisiin.
  
## a) Asenna Debian 12-Bookworm virtuaalikoneeseen. 
Tehty Linux-palvelimet kurssilla https://github.com/MikoLiukk/Linux-Palvelimet-2025/blob/main/H1_Linux_Virtuaalikone_asennus.md

## b) Asenna Salt (salt-minion) Linuxille (uuteen virtuaalikoneeseesi).
Tein Salt project:n ohjeiden mukaan Salt-minionin asentamisen. https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/linux-deb.html

Keyrings-polun luonti

```mkdir -p /etc/apt/keyrings```    

Julkisen avaimen lataus

```curl -fsSL https://packages.broadcom.com/artifactory/api/security/keypair/SaltProjectKey/public | sudo tee /etc/apt/keyrings/salt-archive-keyring.pgp```    

Salt:n apt repon konfigurointi jotta esim apt-get install osaa etsiä oikeasta paikasta salt-minion ohjelman

```curl -fsSL https://github.com/saltstack/salt-install-guide/releases/latest/download/salt.sources | sudo tee /etc/apt/sources.list.d/salt.sources```    
Paketinhallinnan päivitys uuden apt repon konfiguroinnin jälkeen

```sudo apt-get update```   

Varsinainen Salt-minionin asennus. 

```sudo apt-get -y install salt-minion```   

Lopuksi vielä version tarkastus asennuksesn varmentamiseksi. 

```sudo salt-call --version```   

![Näyttökuva 2025-03-31 191846](https://github.com/user-attachments/assets/f0cc66cc-02f6-42e2-86bf-0982646a4cde)

## c) Saltin tärkeimmät tilafunktiot

### pkg

![Näyttökuva 2025-03-31 191955](https://github.com/user-attachments/assets/f2ac5e10-e498-4a5c-8910-ba5f7f7aaa37)

Komento ```sudo salt-call --local -l info state.single pkg.installed bash-completion``` asentaa slave-koneeseen bash-conpletion.

### file 

![Näyttökuva 2025-03-31 193432](https://github.com/user-attachments/assets/ed52453d-3e13-47c9-83c2-e5c2dbce8fd7)

Komento ```sudo salt-call --local state.single file.managed /tmp/tiedosto contents="taalla ollaan" -l info``` luo tiedoston "tiedosto" slave-koneeseen ja sisältäen tekstin "taalla ollaan"

### service

![Näyttökuva 2025-03-31 193014](https://github.com/user-attachments/assets/b36a23f4-c3bd-4404-8567-c9d8958e7e42)

Komennolla ```sudo salt-call --local state.single service.running apache2 enable=True``` käynnistetään apache2 . Apache2 oli jo käynnissä.

### user

![Näyttökuva 2025-03-31 192538](https://github.com/user-attachments/assets/e27f73ab-e947-424f-89db-241f48c90d82)

Komento ```sudo salt-call --local -l info state.single user.absent turha``` tarkistetaan, että käyttäjää "turha" ei ole järjestelmässä.

### cmd

![Näyttökuva 2025-03-31 192713](https://github.com/user-attachments/assets/59e579ab-8fdf-4c19-83d7-4318a1c40e5d)

Komento ```sudo salt-call --local -l info state.single cmd.run 'touch /tmp/koske' creates="/tmp/koske"``` luo tiedoston "koske" ja toisella kertaa koskee tiedostoa "koske.

## d) Idempotentti. 

Impotentin avulla komentoa voidaan ajaa loputtamasti ja lopputulos on sama kuin ensimmäisellä 

![Näyttökuva 2025-03-31 193553](https://github.com/user-attachments/assets/d942b32c-5b92-4734-b427-105ca8b5db72)


## Lähteet: 
Karvinen, T. 2025. Palvelinten Hallinta.   
https://terokarvinen.com/palvelinten-hallinta/   
Tehtävänanto.   

Karvinen, T. 2021. Run Salt Command Locally.    
https://terokarvinen.com/2021/salt-run-command-locally/   
Tehtävät x, a, b, c, d.    

Karvinen, T. 2018. Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux.    
https://terokarvinen.com/2018/03/28/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/   
Tehtävät x, a, b, c, d.    

Karvinen, T. 2006. Raportin kirjoittaminen.    
https://terokarvinen.com/2006/06/04/raportin-kirjoittaminen-4/   
Tehtävät x, a, b, c, d.    

Salt Project. s.a. Salt install guide for Linux (DEB).    
https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/linux-deb.html   
Tehtävät x, a, b, c, d.    
