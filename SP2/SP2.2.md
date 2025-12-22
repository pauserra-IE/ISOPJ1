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

### 5. Teoria Automatització i scripts cron i anacron

### 6. Pràctica automatització
  - cron
  - anacron
------------------------------
DOCUMENTACIO

01/12/25
### 1. Teoria Còpies de Seguretat
Aquí tens la documentació millorada d'aquest apartat, estructurada de manera més professional i clara per al teu fitxer de GitHub.

---

### 1. Teoria de Còpies de Seguretat (Backups)

Una **còpia de seguretat** és un duplicat de dades que es realitza per poder recuperar la informació en cas de pèrdua, corrupció o fallada del sistema. És la mesura de seguretat més crítica en l'administració de sistemes.

#### Tipus de Còpies de Seguretat

A continuació, es comparen els tres mètodes principals segons la seva gestió de dades i recuperació:

| Tipus | Descripció | Avantatges | Desavantatges | Requisits de Recuperació | Espai Ocupat |
| --- | --- | --- | --- | --- | --- |
| **Completa** | Copia totes les dades seleccionades. | Recuperació fàcil i ràpida. | Lentitud i gran consum d'espai. | Només la darrera còpia completa. | Molt alt |
| **Diferencial** | Copia els canvis fets des de l'última **Completa**. | Més ràpida que la completa i estalvia espai. | Creix amb el temps fins a la següent completa. | L'última Completa + l'última Diferencial. | Mitjà |
| **Incremental** | Copia els canvis des de la **darrera còpia** (sigui completa o incremental). | La més ràpida i la que menys espai ocupa. | Recuperació més complexa i lenta. | L'última Completa + **totes** les incrementals en ordre. | Baix |

---

#### Eines de Còpia i Clonació en Linux

* **`cp` (Copy):** És una eina de còpia simple i local. No és "intel·ligent", la qual cosa significa que si tornem a executar la còpia, tornarà a copiar tots els fitxers de zero sense optimitzar temps ni espai.
* **`rsync` (Remote Sync):** Eina avançada i intel·ligent. Només transfereix els fitxers modificats (còpia incremental a nivell de blocs). Permet treballar tant en local com de forma remota (via SSH) i manté els permisos i propietaris.
* **`dd` (Data Duplicator):** No és una eina de còpia de fitxers convencional, sinó una eina de **clonació a nivell de bits**. S'utilitza per copiar discos sencers o particions bit a bit, ideal per a forense o replicació de sistemes.

---

### Part Pràctica: Gestió de Discos i Còpies

#### 1. Identificació de Discos i Particions

Abans de realitzar qualsevol còpia en dispositius físics, fem servir `fdisk -l` per llistar els dispositius d'emmagatzematge disponibles.

<img width="1203" height="769" alt="image" src="https://github.com/user-attachments/assets/d0af3951-2465-4402-bc89-5dfcd497d8b0" />

Després de crear les particions necessàries per allotjar les còpies, verifiquem que l'estructura és correcta:

<img width="781" height="562" alt="image" src="https://github.com/user-attachments/assets/5190a366-914a-4855-b589-d3d51bd48e7e" />

---

#### 2. Execució de Còpies de Seguretat

**Ús de `cp` recursiu:**
Executem una còpia de tot el contingut del directori `Documents` de l'usuari cap al directori de destinació `/var/copies` utilitzant el paràmetre `-R` (recursiu).

<img width="953" height="221" alt="image" src="https://github.com/user-attachments/assets/fc9fe6b2-00f8-4716-b231-02a0beea17a5" />

**Ús de `rsync` per a sincronització eficient:**
Utilitzem `rsync` per assegurar que només es copiïn les noves modificacions, optimitzant així el rendiment del sistema.

<img width="918" height="482" alt="image" src="https://github.com/user-attachments/assets/0a76a433-ff91-40a8-bd2d-1479e9359646" />

---
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






15/15/25

## Quotes d’usuari

Les quotes de disc, també anomenades quotes d’usuari, són un mecanisme del sistema operatiu que permet controlar i restringir la quantitat d’espai de disc i el nombre de fitxers que poden utilitzar els usuaris o els grups dins d’un sistema.

### Verificació dels discos disponibles

En primer lloc, comprovem els discos presents al sistema utilitzant la comanda `fdisk -l`.

<img width="593" height="337" alt="image" src="https://github.com/user-attachments/assets/f57aea23-8e03-4e40-800a-d4f00a009770" />

## Instal·lació del suport per a quotes

Per poder gestionar les quotes, instal·lem el paquet necessari amb la comanda:

`apt install quota`

<img width="786" height="247" alt="image" src="https://github.com/user-attachments/assets/12d137b6-0748-4de9-a6ac-5dcf8d83e02e" />

### Creació del directori de dades

A continuació, creem un directori dins de `/mnt` que servirà com a punt de muntatge per a les dades dels usuaris.

<img width="548" height="54" alt="image" src="https://github.com/user-attachments/assets/99e1f9c7-02a1-4bf7-9426-0c7d8cab5f8c" />

### Configuració del muntatge permanent

Editem l’arxiu `/etc/fstab` i afegim la següent entrada:

```
/dev/sdc1   /mnt/dades_usuaris   ext4   defaults,usrquota,grpquota   0   0
```

Significat dels camps:

* **/dev/sdc1**: partició del disc que es muntarà.
* **/mnt/dades_usuaris**: carpeta on es podrà accedir al contingut.
* **ext4**: sistema de fitxers utilitzat.
* **usrquota**: habilita el control de quotes per usuari.
* **grpquota**: habilita el control de quotes per grup.
* **0 0**: opcions relacionades amb la còpia de seguretat i la comprovació del disc.

<img width="928" height="415" alt="image" src="https://github.com/user-attachments/assets/156bbda2-92e5-410b-b713-0bac0d8906a4" />

Un cop guardats els canvis, reiniciem la màquina perquè la configuració tingui efecte.

<img width="541" height="105" alt="image" src="https://github.com/user-attachments/assets/1a2115fd-2511-4078-899d-0c843d8f0bf6" />

### Comprovació del punt de muntatge

Després de reiniciar, podem verificar que el disc està muntat correctament de dues formes:

* Consultant el contingut del directori amb `ls /mnt/dades_usuaris`
* Revisant els sistemes de fitxers actius amb `df -T`

<img width="651" height="261" alt="image" src="https://github.com/user-attachments/assets/ac388e06-be85-4c38-a23d-ab6c70ca54ba" />

### Creació d’un usuari de prova

Creem un usuari anomenat **prova**, que utilitzarem per validar el funcionament de les quotes.

<img width="812" height="548" alt="image" src="https://github.com/user-attachments/assets/764bf323-c82b-4827-aeff-e2f1f4289a05" />

Dins del directori de dades s’han de generar automàticament els fitxers:

* `lost+found`
* `aquota.user`
* `aquota.group`

<img width="689" height="49" alt="image" src="https://github.com/user-attachments/assets/b7fa2c1d-c2ea-47d8-beff-fc943df48167" />

### Activació del control de quotes

Per assegurar el correcte funcionament, activem i desactivem les quotes del sistema de fitxers:

* `quotaoff /mnt/dades_usuaris`
* `quotaon /mnt/dades_usuaris`

Aquestes ordres permeten habilitar o aturar temporalment el sistema de control de quotes.

### Consulta de l’estat de les quotes

Comprovem l’estat de les quotes configurades mitjançant les ordres següents:

* `quota -u prova` per consultar un usuari concret.
* `repquota /dev/sdc1` per obtenir un informe general del disc.

<img width="947" height="181" alt="image" src="https://github.com/user-attachments/assets/887a3288-b52c-4572-9909-9884f1dcebf4" />

### Assignació de límits d’espai

Editem la quota de l’usuari **prova** amb la comanda:

`edquota -u prova`

Configurem els valors de la següent manera:

```
/dev/sdc1   0   1024   2048   0   0
```

* **Límit tou**: 1024 blocs (aproximadament 1 MB).
* **Límit dur**: 2048 blocs (aproximadament 2 MB).
* No s’estableix cap límit pel nombre de fitxers.

### Assignació de permisos

Canviem els permisos del directori per permetre l’escriptura als usuaris:

`chmod 777 /mnt/dades_usuaris`

<img width="739" height="291" alt="image" src="https://github.com/user-attachments/assets/188f011e-ffbb-4a3f-b8dc-a639017889a2" />

## Verificació pràctica amb l’usuari prova

Accedim amb l’usuari **prova** i comencem a crear fitxers per comprovar els límits configurats.

Creació del primer fitxer:

<img width="862" height="116" alt="image" src="https://github.com/user-attachments/assets/29f33a5a-47dd-415a-aff6-b9de66f3dd2e" />

Creació del segon fitxer fins arribar a 1600 KB:

<img width="862" height="116" alt="image" src="https://github.com/user-attachments/assets/b13303e4-1610-49f9-b867-8b88cfed9aaa" />

En intentar crear un tercer fitxer, el sistema indica que s’ha superat la quota establerta i només permet crear l’arxiu parcialment.

<img width="870" height="312" alt="image" src="https://github.com/user-attachments/assets/80e272c6-efee-4328-98d8-d3a4b44095e6" />

## Configuració del període de gràcia

El període de gràcia general del sistema es pot modificar amb la comanda:

`edquota -t`

Aquest valor afecta totes les quotes del disc.

<img width="765" height="178" alt="image" src="https://github.com/user-attachments/assets/68739e8e-065b-4db4-b409-30c6f7fbf732" />

Si volem definir un període de gràcia específic només per a un usuari concret, podem utilitzar:

`setquota -T -u prova 1296000 1296000 /mnt/dades_usuaris`

<img width="903" height="228" alt="image" src="https://github.com/user-attachments/assets/8358248d-f6d3-40a7-87af-3d62a747f258" />
