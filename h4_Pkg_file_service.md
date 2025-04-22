# h4 Pkg-file-service

## x) Lue ja tiivistä.
- CMS:llä voi hallita suuria määriä demoneita.
- Package-file-servicellä voi säädellä muunmuassa ohjelmien asennusta.
- init.sls tiedostolla voidaan konffata muun muunmuassa avaamaan portteja.

## a) Apache easy mode. Asenna Apache, korvaa sen testisivu ja varmista, että demoni käynnistyy.

Giang Le:n raportin avulla tehty tehtävät https://github.com/gianglex/Courses/blob/1642c80416c246029967532a99e79a561b22e082/Palvelinten-Hallinta/h4-pkg-file-service.md?plain=1

Asensin aluksi curlin ```sudo apt-get install -y curl``` ja Apache2 ```sudo apt-get install -y apache2```
Index.html sivustolle kirjoitin Hengissä 22.4.2025. ```echo "Hengissä 19.04.2025"|sudo tee /var/www/html/index.html```
```sudo systemctl enable apache2 --now``` ja aloitusivu oli korvattu ```curl localhost```

![Näyttökuva 2025-04-22 113635](https://github.com/user-attachments/assets/7bf6f718-89d2-495e-9e96-261756f6ed51)

Tämän pohjalta saatiin tehtyä .sls tiedosto. 
```sudo mkdir /srv/salt/```

```sudo mkdir /srv/salt/apache2```

```sudoedit /srv/salt/apache2/init.sls```

```
curl:
  pkg.installed

apache2:
  pkg.installed

apache2_service:
  service.running:
    - name: apache2
    - enable: True

index_html:
  file.managed:
    - name: /var/www/html/index.html
    - contents: |
        Hengissä 22.4.2025
```
.sls tiedoston luominen onnistui, mutta komennon ajaminen minioneille toi ongelmia, mihin viittasin edellisessä viikko tehtävässä. Jostain syystä koneiden uudelleen käynnistys korjasi ongelman hetkellisesti.


![Näyttökuva 2025-04-22 115603](https://github.com/user-attachments/assets/58e03761-1c31-4617-8268-b510d70172d5)

![Näyttökuva 2025-04-22 115627](https://github.com/user-attachments/assets/146fd6b3-7c11-45a5-b97a-5a479c703833)

 ![Näyttökuva 2025-04-22 115943](https://github.com/user-attachments/assets/723ed918-76f4-4c4f-8c8a-053ca7c6b2a8)

Ongelma liittyi "oom-kill" "prosessiin" tai vastaavaa, mutta virtuaalikoneessa ei ollut tarpeeksi muistia.
Poistun masterilta ja tarkitin toimivuuden minionilla ja siellähän localhost olikin "Hengaillaa".

![Näyttökuva 2025-04-22 120046](https://github.com/user-attachments/assets/a10ad985-ea46-47b7-9a25-4c4824450fd7)


## b) SSHouto. Lisää uusi portti, jossa SSHd kuuntelee.
Aluksi testattiin, että portti 8888 oli kiinni ja se olikin ```ssh -p 8888 vagrant@192.168.33.101```.

```sudoedit /etc/ssh/sshd_config``` --> sshd konffaus tiedosto --> 
```
# Lines omitted for brevity
Port 22
Port 8888
# Lines omitted for brevity
```

![Näyttökuva 2025-04-22 154707](https://github.com/user-attachments/assets/4dacd2ef-8ee4-476a-a092-24423cb9d1fc)

Tämän jälkeen kun näitä yritin ajaa minioneille alkoi tapahtumaan taas, (sama oom-kill)

![Näyttökuva 2025-04-22 160907](https://github.com/user-attachments/assets/fa90c8e4-e719-454c-a34d-df8d27af8de1)


