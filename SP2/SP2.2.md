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

### 5. Teoria i Pràctica d'Automatització: scripts, Cron i Anacron

Cron i Anacron són les dues eines principals en sistemes Unix/Linux per a l'automatització de tasques periòdiques. Aquestes tasques s'executen generalment mitjançant **scripts** de shell que contenen una sèrie d'ordres a realitzar.

#### Diferències principals

| Característica | **Cron** | **Anacron** |
| --- | --- | --- |
| **Execució** | Basada en una data i hora exacta (minut, hora, dia). | Basada en intervals de temps (freqüència diària, setmanal, mensual). |
| **Dependència** | Requereix que el sistema estigui encès 24/7. | No requereix que el sistema estigui sempre encès. |
| **Tasca perduda** | Si el sistema està apagat a l'hora programada, la tasca no s'executa. | Si el sistema estava apagat, executa la tasca pendent en tornar-se a engegar. |
| **Ús ideal** | Servidors que no s'apaguen mai i tasques precises. | Ordinadors personals o estacions de treball que s'apaguen sovint. |

---

#### CRON: Configuració i directoris

El servei de Cron utilitza diversos fitxers i directoris de configuració segons l'àmbit de la tasca (usuari o sistema).

**Arxius i directoris importants:**

<img width="909" height="516" alt="image" src="https://github.com/user-attachments/assets/7cae644a-c4e3-4263-a6d5-8b88567d815d" />

Si volem programar una tasca per a un usuari específic, utilitzem la comanda `crontab -e` (o `crontab -e -u [usuari]` amb permisos de superusuari) per editar el fitxer de configuració personal.

<img width="1309" height="500" alt="image" src="https://github.com/user-attachments/assets/91b044ed-a40d-43ce-9698-66c23a894adb" />

<img width="792" height="554" alt="image" src="https://github.com/user-attachments/assets/be1b1a1b-1150-4ac0-8e25-40a22cbba871" />

A més de la configuració manual, el sistema disposa de carpetes predeterminades on podem moure els nostres scripts directament:

* `/etc/cron.hourly` (cada hora)
* `/etc/cron.daily` (cada dia)
* `/etc/cron.weekly` (cada setmana)
* `/etc/cron.monthly` (cada mes)

Aquesta metodologia és molt pràctica, ja que no cal conèixer la sintaxi de cron (els 5 asteriscs); només cal desar el fitxer executable dins de la carpeta corresponent.

<img width="470" height="208" alt="image" src="https://github.com/user-attachments/assets/f39b3c75-5bfd-4bea-9c8a-a51c83bfa262" />

#### ANACRON: El fitxer anacrontab

Anacron es gestiona principalment a través del fitxer `/etc/anacrontab`. Està dissenyat per garantir que les tasques de manteniment s'executin encara que l'ordinador hagi estat apagat.

<img width="808" height="347" alt="image" src="https://github.com/user-attachments/assets/5bd5b17f-d9be-49bd-b1ca-c00c5a58f1b6" />

---

### TASQUES PRÀCTIQUES

#### 1. Creació d'un Script de Còpia de Seguretat

Primer, crearem un script anomenat `copies.sh` al nostre directori personal (home).

**Codi de l'script:**
```
#!/bin/bash
# TIMESTAMP genera una marca de temps única (Exemple: 20231024_153000)
TIMESTAMP=$(date +"%Y%m%d_%H%M%S")

# tar crea un fitxer comprimit .tar.gz
# -c: crear, -v: visualitzar el procés, -f: especificar el fitxer
tar -cvf /home/pauserra/Escriptori/copies_$TIMESTAMP.tar.gz /home/pauserra/Imatges

```

<img width="886" height="191" alt="image" src="https://github.com/user-attachments/assets/1d246cfe-cacc-40fc-9655-e53f0059e5a6" />

Un cop creat, és imprescindible donar-li **permisos d'execució**:

```
chmod +x copies.sh

```
<img width="785" height="341" alt="image" src="https://github.com/user-attachments/assets/bbc8bdb9-35a1-4dff-8047-e147ef45490f" />

#### 2. Programació amb Cron

Per programar l'execució automàtica, editem el fitxer `/etc/crontab` i afegim la línia corresponent amb la sintaxi de temps i l'usuari que l'ha d'executar.

<img width="901" height="414" alt="image" src="https://github.com/user-attachments/assets/ae4d6c42-2cdb-4171-9b8d-dc64bd5eddeb" />

Podem verificar que el fitxer s'ha creat correctament a l'hora indicada:
<img width="901" height="414" alt="image" src="https://github.com/user-attachments/assets/056047d7-e293-44f9-a412-f26519af6641" />

#### 3. Automatització amb Anacron (via cron.daily)

Si volem que la tasca s'executi diàriament sense dependre d'una hora exacta, copiem el nostre script a la carpeta `cron.daily`:

<img width="768" height="71" alt="image" src="https://github.com/user-attachments/assets/6cc9eb8f-1a0a-4e70-bc39-5f22e11d0c13" />

A la configuració de `/etc/anacrontab`, podem veure que les tasques diàries s'executaran amb un retard (delay) determinat després de l'arrencada del sistema (per exemple, 5 minuts després d'engegar).

<img width="806" height="313" alt="image" src="https://github.com/user-attachments/assets/120f5a72-139c-4d66-bf60-d8725a3d2ca5" />

Finalment, en fer un **reboot** i esperar el temps de marge configurat, l'script s'executarà automàticament.
<img width="416" height="189" alt="image" src="https://github.com/user-attachments/assets/fa1ab1d5-4420-4081-a15f-bac724a2a40c" />

TASCA 2: Triar un programa i fer 3 tipus de copies de seguretat

He tirat rsync per gestionar diferents tipus de còpies de seguretat. A continuació, resumeixo el procés seguit a les captures i la documentació corresponent per a cada mètode:

1. Còpia de seguretat completa
A la primera captura, s'ha realitzat una transferència inicial de totes les dades.

Comanda utilitzada: rsync -av --progress /home/pauserra/Baixades /home/pauserra/Escriptori
<img width="877" height="167" alt="image" src="https://github.com/user-attachments/assets/b615ca1d-fb6d-46cb-9fe7-d4e90ca061eb" />

Explicació: S'ha copiat el directori sencer "Baixades" a l'escriptori. L'opció -a (archive) permet conservar permisos i dates, mentre que -v i --progress ens donen visibilitat sobre el que s'està transferint. És la base necessària abans de fer qualsevol còpia incremental o diferencial.

2. Còpia de seguretat incremental
A la segona captura, s'ha afegit un fitxer nou (fitxer_nou) i s'ha executat una còpia que només processa els canvis.

Comanda utilitzada: rsync -av --progress --link-dest=/home/pauserra/Escriptori /home/pauserra/Baixades/ /home/pauserra/Escriptori
<img width="1141" height="200" alt="image" src="https://github.com/user-attachments/assets/b4d33403-a730-43ea-98f7-2293f2df3311" />

Explicació: Mitjançant --link-dest, l'eina compara l'origen amb una còpia anterior. Si un fitxer no ha canviat, crea un "hard link" (enllaç físic) en lloc de tornar-lo a copiar, estalviant molt d'espai en el disc. Només es transfereixen realment les dades noves.

3. Còpia de seguretat diferencial
A la darrera captura, s'ha creat un segon fitxer (fitxer_nou2) i s'ha aplicat un mètode diferencial simplificat.

Comanda utilitzada: rsync -av --progress --ignore-existing /home/pauserra/Baixades/ /home/pauserra/Escriptori
<img width="991" height="147" alt="image" src="https://github.com/user-attachments/assets/73afe859-8864-4866-b6bd-dc1ef0f381f8" />

Explicació: L'opció --ignore-existing fa que rsync salti qualsevol fitxer que ja existeixi a la destinació, centrant-se exclusivament a copiar els fitxers nous creats des de l'última sincronització. És un mètode ràpid per assegurar-se que la destinació té les darreres novetats sense revisar modificacions en fitxers ja existents.







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
