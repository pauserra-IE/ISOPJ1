---
layout: default
title: "Sprint 2: Instal·lació, Configuració de Programari de Base i Gestió de Fitxers"
---

# ÍNDEX
## Còpies de seguretat i automatització de tasques

### 1. Teoria Còpies de Seguretat
Que es una copia
3 tipus de copies de seguretat
### 2. Teoria Comandes Backups

### 3. Pràctica comandes Backups
  - cp
  - rsync
  - dd

### 4. Pràctica programes Backups
  - Deja-Dup
  - Duplicity

### 5. Teoria Automatització scripts.cron i anacron

### 6. Pràctica automatització
  - cron
  - anacron
------------------------------
DOCUMENTACIO

01/12/25
### 1. Teoria Còpies de Seguretat
Que es una copia

3 tipus de copies de seguretat, ventatjes i desventatges i que es necessita per recuperar cadascuna
Tipus: completa, diferencial, incremental

1-Completa
2-Diferencial: ventatjaocupa menys que una completa. la diferencial sempre es fa a partir de la completa. desventatja: per a recuperarla necessites la ultima completa 
3-Incremental: es necessita la ultima completa o diferencial i totes les incrementals anteriors. ventatja ocupen molt poc ja que nomes conte la diferencia...
fer una taula en 6 columnes: tipus, ques es, avantatges, desventatges i que es necessita per a recuperarles



cp: es una copìa simple que no es inteligent. transfereix arxius nomes localment. no optimitza ni temps ni espai
rsync: es una eina inteligent que nomes copia els fitxers modificats. treballa en local i en remot.
dd:No es una eina per a copiar (es una eina per a clonar) s'utlitza quan volem copiar tot el contingut d'un disc o una particio.


Pràctica:


captura fdisk -l
<img width="1203" height="769" alt="image" src="https://github.com/user-attachments/assets/d0af3951-2465-4402-bc89-5dfcd497d8b0" />

captura comprovacio despres de crear les particions:
<img width="781" height="562" alt="image" src="https://github.com/user-attachments/assets/5190a366-914a-4855-b589-d3d51bd48e7e" />



captura cp -R /home/pauserra/Documents/* /var/copies
<img width="953" height="221" alt="image" src="https://github.com/user-attachments/assets/fc9fe6b2-00f8-4716-b231-02a0beea17a5" />

captura rsync
<img width="918" height="482" alt="image" src="https://github.com/user-attachments/assets/0a76a433-ff91-40a8-bd2d-1479e9359646" />

09/12/25

<img width="903" height="415" alt="image" src="https://github.com/user-attachments/assets/ade638e8-8fa5-4f99-a5d3-abd9ffcb1294" />
Primer de tot, he muntat la meva partició d'origen (/dev/sdb1) a la carpeta /var/copies i he fet un ls per confirmar què hi havia a dins. He vist que hi tenia les carpetes simulacre i simulacre2.

Després, m'he situat al directori /var i he creat una nova carpeta anomenada clonacio (mkdir clonacio) per fer-la servir com a punt de muntatge per al disc de destí.
Tot seguit, he intentat muntar la partició de destí (/dev/sdc1) en aquesta nova carpeta. El sistema m'ha donat un error (wrong fs type), segurament perquè la partició estava buida o sense formatar, però no m'ha importat perquè el meu pla era sobreescriure-la completament.

Llavors he llançat l'ordre clau: dd. He ordenat copiar tot el que hi havia a l'origen (if=/dev/sdb1) cap al destí (of=/dev/sdc1), mostrant el progrés. En poc més de mig segon, he clonat 1,1 GB de dades.
Finalment, per assegurar-me que tot ha anat bé, he comparat les dues particions calculant el seu md5sum. En veure que els dos codis resultants eren idèntics, he verificat que la còpia és una rèplica exacta i perfecta de l'original.

### 5. Teoria Automatització scripts.cron i anacron
Cron i anacron son 2 eines de d'automatitzacio per a execitar tasques periodiques. aquestes tasques periodiques son a traves de scripts

DIFERENCIES
Cron executa tasques programades en una data i hora especifiques. Si el sistema esta apagat la tasca es perd. Es ideal per a tasques en dates i hores concretes i per a accions especifiques d'un usuari.

Anacron és ideal per a executar tasques periodiques on no cal una data i una hora especifiques.Normalment s'utilitza per a tasques de manteniment del sistema. No requereix que el sistema estigui obert perque quan s'obri ja s'executara. (no es perd la tasca com al cron)


CRON 
Arxius i directoris importants

<img width="909" height="516" alt="image" src="https://github.com/user-attachments/assets/7cae644a-c4e3-4263-a6d5-8b88567d815d" /> per a un usuari

per a tots els usuaris



Ara crearem un script copies.sh a la nostra home 
<img width="886" height="191" alt="image" src="https://github.com/user-attachments/assets/1d246cfe-cacc-40fc-9655-e53f0059e5a6" />

i li donem permisos al script
<img width="785" height="341" alt="image" src="https://github.com/user-attachments/assets/bbc8bdb9-35a1-4dff-8047-e147ef45490f" />

ara fem un nano /etc/crontab i afegim la ultima linia
<img width="901" height="414" alt="image" src="https://github.com/user-attachments/assets/ae4d6c42-2cdb-4171-9b8d-dc64bd5eddeb" />

i es crea a l'hora que li hem dit
<img width="901" height="414" alt="image" src="https://github.com/user-attachments/assets/056047d7-e293-44f9-a412-f26519af6641" />


copiem el scrip de la home a cron.daily
<img width="768" height="71" alt="image" src="https://github.com/user-attachments/assets/6cc9eb8f-1a0a-4e70-bc39-5f22e11d0c13" />

per a que s'executi al engegar-s'hi al cap de 1 minut
<img width="806" height="313" alt="image" src="https://github.com/user-attachments/assets/120f5a72-139c-4d66-bf60-d8725a3d2ca5" />

i ara al fer un reboot al cap de 1 minut


tasques:
cron i anacron i un script que faci algo
i triar un programa que faci 3 tipus de copies de seguretat. fer completa i restaurarla i fer diferencial i restaurarla

revisar la captura que falta d'avui













































15/12/25

Maquina en 2 discos de 1gb

Quotes de discos/usuaris

Definició: la quota de disc és la limitacio d'espai de disc que es dona als usuaris
hi ha un limit de temps, etc.





sudo su
cd 

fdisk -l
--captura a sdc1 

mkfs.ext4 /dev/sdc1

apt update

apt install quota
--captura




cd /mnt/

mkdir dades_usuaris
-- captura

nano /etc/fstab/
AFEGIR LINIA
/dev/sdc1              /mnt/dades_usuari            ext4           defaults,usrquota,grpquota     0       0
--captura

guardar, tancar i reiniciar maquina



dos maneres per comprovar que quan reiniciem que el punt de muntatge es correcte

ls /mnt/dades_usuaris

df -T 
--captura



adduser prova
--captura

cd /mn/dades_usuari
ls -l

hauria d'apariexer lost+found i un altre arxiu

quotacheck -cug /mnt/dades_usuari
hauria d'apareixeer aquota.group aquota.user lost+found

--captura



quotaon /mnt/dades_usuari
ls
quotaoff /mnt/dades_usuari
ls
explicacio: serveixen per activar i desactivar aquest mecanisme de control de quotes en un sistema de fitxers muntat.
--captura

quota -u prova

repquota /dev/sdc1
--captura


edquota -u prova
CANVIAR LINIA A 
/dev/sdc1         0      1024     2048     0        0
--captura
guardar i sortir

explicacio: es pot configurar per la part de bloc o per la part de nodes


chmod 777 /mnt/dades_usuaris
--captura

clear


su prova
dd if=/dev/zero of=test bs=1K  count=800

--captura

dd if=/dev/zero of=test2 bs=1K  count=800
--captura

sudo su
repquota /dev/sdc1

--captura

quota -u prova
--captura
su prova
dd if=/dev/zero of=test3 bs=1K  count=800
hauria de sortir error en escriure
ls
ls -l
--captura
explicacio: l'arxiu esta incomplet

exit
clear


repquota /dev/sdc1
quotaoff /mnt/dades_usuari
su prova
dd if=/dev/zero of=test4 bs=1K  count=800
--captura
explicació: com que hem fet quotaoff ara si que ens deixa crear l'arxiu test4


exit


edquota -t 

EXPLICACIÓaquest periode de gracia es el que perdefete als usuaris
sempre que editem aquest fitxer es per a tots els usuaris pero es pot fer per a un usuari en concret
--captura


setquota -T -u prova 1296000 1296000 /mnt/dades_usuaris

explicacio de la comanda:

repquota /dev/sdc1
explicacio de la comanda:
--captura
