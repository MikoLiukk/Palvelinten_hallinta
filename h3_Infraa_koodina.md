# h3 Infraa koodina
## x) Lue ja tiivistä.
https://terokarvinen.com/2024/hello-salt-infra-as-code/
- ```$ sudo apt-get update``` ja ```$ sudo apt-get -y install salt-minion```
- ```$ sudo mkdir -p /srv/salt/hello/``` ja ```$ cd /srv/salt/hello/```
- ```sudoedit init.sls```
- ```$ sudo salt-call --local state.apply hello```

https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml
- YAML perustuu sisennykseen (block structure)
- Käytä 2 välilyöntiä per sisennystaso.
- Listat ja sanakirjat voivat olla sisäkkäisiä

## a) Hei infrakoodi! Kokeile paikallisesti (esim 'sudo salt-call --local') infraa koodina.
Aloitin tehtävän kirjautumalla masteriin ```vagrant ssh tmaster```.
Tämän jälkeen loin tiedostopuun ja init.sls tiedoston sinne.
```sudo mkdir /srv/salt/```
```sudo mkdir /srv/salt/examplefile```
```sudoedit /srv/salt/examplefile/init.sls/```
Init.sls filen sisältö Giangilta: (https://github.com/gianglex/Courses/blob/main/Palvelinten-Hallinta/h3-infraa-koodina.md)
```
create_example_file:
  file.managed:
    - name: /tmp/example
    - contents: example content
```
Tämän jälkeen ```sudo salt-call --local state.apply examplefile```
![Näyttökuva 2025-04-14 132817](https://github.com/user-attachments/assets/6e9eab43-2de8-441f-bcb6-62b9510bca41)

Tässä kohtaa huomiona, että Total run time: 20,075ms. En tiedä miksi vagrant toimi hitaasti, onko raudasta kyse vai oliko tietokoneella vain huonopäivä.
Käynnistin rautakoneen uudelleen ja vagrantin, mutta vagrant toimi vaan hitaammin.

## b) Aja esimerkki sls-tiedostosi verkon yli orjalla.
Tässä tehtävässä alkoi timeoutteja tulemaan ja minionit ei vastannut.
``` sudo salt 't001' state.apply```
![Näyttökuva 2025-04-14 164828](https://github.com/user-attachments/assets/5ec244c4-a105-4a63-a388-3db515acad19)

Tarkistin, että mikä salt-masterissa on vikana

![Näyttökuva 2025-04-14 164842](https://github.com/user-attachments/assets/482e9247-8687-4de2-8ec6-0db30b846ddf)

Kokeilin pinggailla orjia, ei löytynyt

![Näyttökuva 2025-04-14 164911](https://github.com/user-attachments/assets/494edc3a-8a8b-4d86-9acd-1d45f18e3bf6)

Yhtäkkiä komennoilla ```sudo salt 't001' state.apply ezamplefile --state-output=terse``` ja ```sudo salt 't001' state.apply examplefile -l error``` alkoi näkymään elämisen merkkejä.

![Näyttökuva 2025-04-14 164928](https://github.com/user-attachments/assets/dcea93b3-c110-4ff3-a720-204d6e230f4f)

Ja ongelma olikin sitten korjaantunut?

![Näyttökuva 2025-04-14 165123](https://github.com/user-attachments/assets/f22e18c1-7605-493e-82e5-27cf7c80a6be)

## c) Tee sls-tiedosto, joka käyttää vähintään kahta eri tilafunktiota näistä: package, file, service, user. Tarkista eri ohjelmalla, että lopputulos on oikea. Osoita useammalla ajolla, että sls-tiedostosi on idempotentti.
