# H2 - Soitto kotiin

## x) Lue ja tiivistä.
- Tärkeitä vagrant komentoja on:
    - ```vagrant init```, ```vagrant up```, ```vagrant halt```, ```vagrant destroy``` ja ```vagrant ssh```
- Tärkeitä Salt-Minion komentoja on:
    - ```master$ hostname -I```, ```master$ sudo salt-key -A```, ```slave$ sudoedit /etc/salt/minion``` ja ```slave$ sudo systemctl restart salt-minion.service```
- Tärkeitä minion hallinta komentoja on:
    - ```master$ sudo salt '*' cmd.run 'hostname -I'```, ```master$ sudo salt '*' grains.items```, ```master$ sudo salt '*' grains.item virtual``` ja ```master$ sudo salt '*' pkg.install httpie```.
 
## a) Hello Vagrant! Osoita jollain komennolla, että Vagrant on asennettu (esim tulostaa vagrantin versionumeron). 

![Näyttökuva 2025-04-07 123950](https://github.com/user-attachments/assets/2189c3f5-b052-414f-80ed-406145b22e47)

## b) Linux Vagrant. Tee Vagrantilla uusi Linux-virtuaalikone.
Seurasin Bene Elohimin ohjeita (https://www.youtube.com/watch?v=GhYm4IvkLQA). 
Komennoilla ``` vagrant box add ubuntu/focal64```, ``` vagrant init ubuntu/focal64``` ja ``` vagrant plugin install vagrant-vbguest``` Luotiin ympäristö, lisättiin ubuntu/focal ympäristöön kuvaksi ja asennettiin virtualbox plugin.

![Näyttökuva 2025-04-06 165820](https://github.com/user-attachments/assets/27f5b249-7485-455e-b187-0ed557765e77)

Kun yritin ottaa yhteyttä koneeseen ```vagrant ssh``` komennolla, se onnistui.

![Näyttökuva 2025-04-06 171106](https://github.com/user-attachments/assets/df4c818b-1e0a-4a2c-8d50-dec85cffe5c9)

Tämän jälkeen poistuin ```exit``` komennolla ja tuhosin virtuaalikoneen ```vagrant destroy``` komennolla.

## c) Linux verkko Vagrantilla
Tehtävässä ongelmana minulla oli, se että vagrant tuhosi virtuaalikoneet, kun ne oli tehty.
```
Skipping unmount of Virtualbox Guest Additions ISO, because it was not mounted.
==> t001: Checking for guest additions in VM...
    t001: The guest additions on this VM do not match the installed version of
    t001: VirtualBox! In most cases this is fine, but in rare cases it can
    t001: prevent things such as shared folders from working properly. If you see
    t001: shared folder errors, please make sure the guest additions within the
    t001: virtual machine match the version of VirtualBox you have installed on
    t001: your host and reload your VM.
    t001:
    t001: Guest Additions Version: 6.0.0 r127566
    t001: VirtualBox Version: 7.1
==> t001: Attempting graceful shutdown of VM...
==> t001: Destroying VM and associated drives...
C:/Users/mikol/.vagrant.d/gems/3.3.6/gems/vagrant-vbguest-0.32.0/lib/vagrant-vbguest/hosts/virtualbox.rb:84:in block in guess_local_iso': undefined method exists?' for class File (NoMethodError)

            path && File.exists?(path)
                        ^^^^^^^^
Did you mean?  exist?
        from C:/Users/mikol/.vagrant.d/gems/3.3.6/gems/vagrant-vbguest-0.32.0/lib/vagrant-vbguest/hosts/virtualbox.rb:83:in each'
```
Kun laitoin kyseisen tekstin CHATGPT, kävi ilmi että vagrantfilen rubyssa oli ongelma, jonka sai korjattua menemällä plugin tiedostoon (```C:/Users/mikol/.vagrant.d/gems/3.3.6/gems/vagrant-vbguest-0.32.0/lib/vagrant-vbguest/hosts/virtualbox.rb```) ja korjaamalla ```file.exists(path) --> file.exist?(path)```. 

Tämän jälkeen loin Teron esimerkin avulla uuden Vagrantfilen, scriptin osa oli vanhentunut ja en saannut salttia toimimaan, koska saltin asennusohjeet olivat muuttuneet. 

Kiitos Giang Le https://github.com/gianglex/Courses/blob/main/Palvelinten-Hallinta/h2-soitto-kotiin.md joka oli korjannut/päivittänyt scriptin, kopioin häneltä scriptin osat, jotka asensivat saltin masteriin ja minioniin.

```
# -*- mode: ruby -*-
# vi: set ft=ruby :
# Copyright 2014-2023 Tero Karvinen http://TeroKarvinen.com

$minion = <<MINION
sudo apt-get update
sudo apt-get -qy install curl
mkdir -p /etc/apt/keyrings
curl -fsSL https://packages.broadcom.com/artifactory/api/security/keypair/SaltProjectKey/public | sudo tee /etc/apt/keyrings/salt-archive-keyring.pgp
curl -fsSL https://github.com/saltstack/salt-install-guide/releases/latest/download/salt.sources | sudo tee /etc/apt/sources.list.d/salt.sources
sudo apt-get update
sudo apt-get -qy install salt-minion
MINION

$master = <<MASTER
sudo apt-get update
sudo apt-get -qy install curl
mkdir -p /etc/apt/keyrings
curl -fsSL https://packages.broadcom.com/artifactory/api/security/keypair/SaltProjectKey/public | sudo tee /etc/apt/keyrings/salt-archive-keyring.pgp
curl -fsSL https://github.com/saltstack/salt-install-guide/releases/latest/download/salt.sources | sudo tee /etc/apt/sources.list.d/salt.sources
sudo apt-get update
sudo apt-get -qy install salt-master
MASTER

Vagrant.configure("2") do |config|
  config.vm.box = "debian/bookworm64"  # Changed box to Debian Bookworm

  config.vm.define "t001" do |t001|
    t001.vm.provision :shell, inline: $minion
    t001.vm.network "private_network", ip: "192.168.12.101"
    t001.vm.hostname = "t001"
  end

	  config.vm.define "t002" do |t002|
		t002.vm.provision :shell, inline: $minion
		t002.vm.network "private_network", ip: "192.168.12.102"
		t002.vm.hostname = "t002"
	end

	config.vm.define "tmaster", primary: true do |tmaster|
		tmaster.vm.provision :shell, inline: $master
		tmaster.vm.network "private_network", ip: "192.168.12.100"
		tmaster.vm.hostname = "tmaster"
	end
end
```

En kopioinut hänen valmiiksi tehtyjä salt-key avaimia.

Giangilta kopioin myös pingaus scriptin 
```
for ip in 192.168.12.100 192.168.12.102 192.168.12.101; do
  ping -c 2 "$ip"
done
```
Jolla sain pingattua kaikki koneet "ristiin" yksi kerrallaan.

![Näyttökuva 2025-04-07 112153](https://github.com/user-attachments/assets/89fa3f4a-a6da-4311-88a2-0d24a60f2782)

## d) Herra-orja verkossa
Aloitin kirjautumalla molempiin orjiin yksikerrallaan ```vagrant ssh t001```.
Tämän jälkeen loin avaimen herralle ```sudoedit /etc/salt/minion```
Tekstitiedoston sisälle kirjoitin masterin ip osoitteen ja orjan id:n.

![Näyttökuva 2025-04-07 131445](https://github.com/user-attachments/assets/d12eb3b1-302c-473b-bc9c-7dcca839cb79)
Tiedoston luomisen jälkeen potkaisin vielä tulevaa orjaa ```sudo service salt-minion restart```.
Tein tämän molemmille orjille.

Tämän jälkeen kirjauduin Masterille ```vagrant ssh tmaster``` ja tarkistin ja hyväksyin kaikki avaimet.
```sudo salt-key -L``` ja ```sudo salt-key -A``` Kun avaimet oli hyväksytty testasin vielä keskusteluyhteyden ```sudo salt '*' test.ping```.

![Näyttökuva 2025-04-07 114641](https://github.com/user-attachments/assets/fe3d8c42-993f-4611-997d-dcd4e8c003cc)

## e) Kokeile vähintään kahta tilaa verkon yli (viisikosta: pkg, file, service, user, cmd)

```sudo salt '*' state.single pkg.installed curl```
![Näyttökuva 2025-04-07 114835](https://github.com/user-attachments/assets/5fa5e7a9-af91-4f93-8f6b-443aea560878)

Komennolla varmistettiin, että curl on asennettu kaikkiin orjiin.

```sudo salt '*' state.single cmd.run 'touch /tmp/koske' creates="/tmp/koske"```

![Näyttökuva 2025-04-07 122233](https://github.com/user-attachments/assets/467906fa-a68d-4158-b235-ad6fcb8e2cdc)

Komento on impotenssi, joka luo orjille tiedoston "koske" ensimmäisellä kerralla ja toisella kerralla koskee tiedostoa "koske". 

## Lähteet:

https://terokarvinen.com/palvelinten-hallinta/   

https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/    

https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/    

https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file/    

https://github.com/gianglex/Courses/blob/main/Palvelinten-Hallinta/h2-soitto-kotiin.md?plain=1

https://github.com/hashicorp/vagrant/issues/4286

http://chatgpt.com

https://www.youtube.com/watch?v=GhYm4IvkLQA
