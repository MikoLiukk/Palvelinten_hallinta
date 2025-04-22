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
.sls tiedoston luominen onnistui, mutta komennon ajaminen minioneille toi ongelmia, mihin viittasin edellisessä viikko tehtävässä.


 
