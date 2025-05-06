## a) Oma miniprojekti.
# Linkki projektin loppu tulokseen: https://github.com/MikoLiukk/Miniprojekti/tree/master

Kaikki projektiin liittyvät tiedostot ja tiedostopuu löytyvät ylläolevasta linkistä, joten en niitä käy erikseen läpi.
Myös käytettävät teknologiat löytyvät linkistä.

### Projektin toteutus
Halusin tehdä automatisoidun palvelinympäristön, joka käyttää kurssilla käytettyä Saltia.
Tavoitteena oli rakentaa helposti hallittava infra, johon sisältyy web-palvelin, palomuuri, tietokanta ja palveluiden hallinta,

### Ongelma tilanteet
Minulle projektin aikana tuli ongelmia saltin "oom-kill" (out of memory) tilan kanssa tämän sain korjattua, muutamalla muistin kokoa suoraan VirtualBoxin asetuksista.
Toinen ongelma tilanne tuli kun "vahingossa" (heittomerkit siksi koska tietokoneilla mitään ei tapahdu vahingossa, vaan ne niitä käsketään suorittamaan komennot) 
hukutin/poistin/korvasin ssh-avaimet ja en päässyt master-koneeseen enään sisälle. Joten jouduin tuhoamaan kaiken mitä olin jo tehnyt ja aloittamaan alusta, onneksi oli muistiinpanot.
![Näyttökuva 2025-05-05 120745](https://github.com/user-attachments/assets/7d29a6b9-c4a1-4552-b062-aea6a0bac771)
![Näyttökuva 2025-05-05 152454](https://github.com/user-attachments/assets/76a68f0b-a1f9-40b5-99f8-ed8ddac930b9)
![Näyttökuva 2025-05-05 153142](https://github.com/user-attachments/assets/07ff8e64-6f09-4601-b99e-05ee0aa81816)

## Tässä vielä kuvia "dumbattuna" näkee mitä on myös tehty.
Kaikki kuitenkin löytyy projekti linkistä, joten voi itsekkin katsoa mitä löytyy :D.

![Näyttökuva 2025-05-05 113924](https://github.com/user-attachments/assets/48f9aae5-3095-480f-a012-c3f6898c91d6)

![Näyttökuva 2025-05-05 113955](https://github.com/user-attachments/assets/395c6147-a12f-4047-8e9b-9fa84bfc7d6e)

![Näyttökuva 2025-05-05 114058](https://github.com/user-attachments/assets/85328f8f-db56-497c-8615-9e08299bab28)

![Näyttökuva 2025-05-05 114148](https://github.com/user-attachments/assets/5a27ba2b-2e80-4af1-8533-25ce9209dfdd)

![Näyttökuva 2025-05-05 115623](https://github.com/user-attachments/assets/94df5e36-b9e1-4fa5-b21e-b929f7a30e7c)

![Näyttökuva 2025-05-05 115626](https://github.com/user-attachments/assets/6f96446d-fd0d-4bd7-9971-7a6380faaf9d)

![Näyttökuva 2025-05-05 115706](https://github.com/user-attachments/assets/54c8ac55-0e2b-4663-aa3c-c7dd76a5f66d)

![Näyttökuva 2025-05-05 115950](https://github.com/user-attachments/assets/9823fa9b-fbf3-451e-a074-195c3dfaa235)

![Näyttökuva 2025-05-05 122158](https://github.com/user-attachments/assets/e243eb7f-0c56-44a7-8dbb-36421ae7c36e)
![Näyttökuva 2025-05-05 122219](https://github.com/user-attachments/assets/2068508c-99a6-4280-aaa8-23b4da47ff58)
![Näyttökuva 2025-05-05 122236](https://github.com/user-attachments/assets/0776f975-bb5c-47e3-b7fa-ff55c1f9e316)
![Näyttökuva 2025-05-05 122253](https://github.com/user-attachments/assets/2720eff4-638b-43e9-9944-13bea8d9a6ba)
![Näyttökuva 2025-05-05 122327](https://github.com/user-attachments/assets/9b46d12a-f727-4d10-bbab-4126e2a36038)
![Näyttökuva 2025-05-05 122421](https://github.com/user-attachments/assets/fd103234-3595-4635-b368-7f11ac1f6fac)
![Näyttökuva 2025-05-05 122439](https://github.com/user-attachments/assets/e5a90698-3068-4db6-9749-374354048cff)

![Näyttökuva 2025-05-05 125207](https://github.com/user-attachments/assets/3c86622d-a072-4285-9660-06c60e37458f)

![Näyttökuva 2025-05-05 125303](https://github.com/user-attachments/assets/9b358ce3-023a-4935-9d49-c643d172e3f4)

![Näyttökuva 2025-05-05 125457](https://github.com/user-attachments/assets/aa417fb9-87bc-4937-9e5c-5412a80f04d7)


## Lähteet: 
https://terokarvinen.com/palvelinten-hallinta/
https://terokarvinen.com/2018/simple-secrets-in-salt-pillars/?fromSearch=salt
https://docs.saltproject.io/en/3006/topics/tutorials/pillar.html
https://www.postgresql.org/docs/17/tutorial-install.html
https://mmonit.com/
https://chatgpt.com/
