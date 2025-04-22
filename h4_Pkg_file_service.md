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

Koitin testi pinggiä.

![Näyttökuva 2025-04-22 161231](https://github.com/user-attachments/assets/06dc99b5-633c-437b-bf40-0a911fe4e9a1)

Toinen minioneista oli kuollut.

![Näyttökuva 2025-04-22 161612](https://github.com/user-attachments/assets/8a7287e5-e711-4d3f-b48b-3b11ea1ff699)
![Näyttökuva 2025-04-22 161614](https://github.com/user-attachments/assets/8a4cdda9-e9cc-4e32-8cde-b47a94173106)
![Näyttökuva 2025-04-22 161713](https://github.com/user-attachments/assets/74d1d482-d3ce-4086-aba7-5b45f35f41c5)
![Näyttökuva 2025-04-22 161715](https://github.com/user-attachments/assets/4a0a9388-8ce8-4474-96c4-7bc3124e8c7b)
![Näyttökuva 2025-04-22 162455](https://github.com/user-attachments/assets/e37c0b90-c608-4c48-befe-1988a45e3aa6)
![Näyttökuva 2025-04-22 162538](https://github.com/user-attachments/assets/fb531935-6e78-4b28-84c0-1c0cb3fb8643)
![Näyttökuva 2025-04-22 162541](https://github.com/user-attachments/assets/2943202f-b51e-4b9c-a2d9-622ded05834a)

Näillä avuilla (ChatGPT) sain korjattua virheen vihdoin pois.

![Näyttökuva 2025-04-22 162646](https://github.com/user-attachments/assets/5435ad1e-3bc3-4526-8298-ca64402e8d73)
![Näyttökuva 2025-04-22 162826](https://github.com/user-attachments/assets/43cce057-b931-4481-a235-8cb5067764d0)
![Näyttökuva 2025-04-22 162907](https://github.com/user-attachments/assets/f690b12e-0992-4efd-8565-56c12040244e)

## Lähteet
https://terokarvinen.com/palvelinten-hallinta/
https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=karvinen%20salt%20ssh
https://terokarvinen.com/2018/apache-user-homepages-automatically-salt-package-file-service-example/
https://github.com/jndg/palvelintenhallinta/blob/main/h4.md
http://chatgpt.com
