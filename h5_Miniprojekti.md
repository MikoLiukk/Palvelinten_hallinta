## a) Oma miniprojekti.
# Linkki projektin loppu tulokseen: https://github.com/MikoLiukk/Miniprojekti/tree/master

Kaikki projektiin liittyvät tiedostot ja tiedostopuu löytyvät ylläolevasta linkistä, joten en niitä käy erikseen läpi.

### Projektin toteutus
Halusin tehdä automatisoidun palvelinympäristön, joka käyttää kurssilla käytettyä Saltia.
Tavoitteena oli rakentaa helposti hallittava infra, johon sisältyy web-palvelin, palomuuri, tietokanta ja palveluiden hallinta,

### Ongelma tilanteet
Minulle projektin aikana tuli ongelmia saltin "oom-kill" (out of memory) tilan kanssa tämän sain korjattua, muutamalla muistin kokoa suoraan VirtualBoxin asetuksista.
Toinen ongelma tilanne tuli kun "vahingossa" (heittomerkit siksi koska tietokoneilla mitään ei tapahdu vahingossa, vaan ne niitä käsketään suorittamaan komennot) 
hukutin/poistin/korvasin ssh-avaimet ja en päässyt master-koneeseen enään sisälle. Joten jouduin tuhoamaan kaiken mitä olin jo tehnyt ja aloittamaan alusta, onneksi oli muistiinpanot.
![Näyttökuva 2025-05-05 152454](https://github.com/user-attachments/assets/76a68f0b-a1f9-40b5-99f8-ed8ddac930b9)
![Näyttökuva 2025-05-05 153142](https://github.com/user-attachments/assets/07ff8e64-6f09-4601-b99e-05ee0aa81816)

## Lähteet: 
https://terokarvinen.com/palvelinten-hallinta/
https://terokarvinen.com/2018/simple-secrets-in-salt-pillars/?fromSearch=salt
https://docs.saltproject.io/en/3006/topics/tutorials/pillar.html
https://www.postgresql.org/docs/17/tutorial-install.html
https://mmonit.com/
https://chatgpt.com/
