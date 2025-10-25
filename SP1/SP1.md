---
layout: default
title: "Sprint 1: Instal·lació, Configuració Inicial i Programari de Base"
---

## LLICENCIAMENT
Ubuntu té aquestes llicencies:
- **Nucli Linux:** GPLv2, permet usar, modificar i redistribuir el kernel.  
- **Eines del sistema:** GPL/LGPL, inclou utilitats bàsiques com bash i coreutils.  
- **Biblioteques i paquets addicionals:** MIT, Apache 2.0, BSD; compatibles amb codi obert.  
- **Marca i logotip d’Ubuntu:** propietat de Canonical Ltd.; l'ús comercial requereix seguir les Ubuntu Trademark Guidelines.

---

## VIRTUALITZACIÓ I INSTAL·LACIÓ DEL SO UBUNTU
En primer lloc obrim VirtualBox per a començar a configurar la maquina virtual.
<img width="877" height="472" alt="Captura de pantalla de 2025-09-30 13-32-49" src="https://github.com/user-attachments/assets/53540824-a77c-495a-aeec-34c4b0be8bc0" />

Sel·leccionem la iso que volem, en aquest cas fare la instal·lació de Ubuntu 24.04.1.  
Marquem la casella "Skip unattended installation" i fem clic a següent.

A continuació ajustarem els paràmetres de la màquina.
<img width="877" height="472" alt="Captura de pantalla de 2025-09-30 13-33-28" src="https://github.com/user-attachments/assets/9f2469ac-f5cc-49a5-afc5-bfe5937e2c1d" />
<img width="877" height="472" alt="Captura de pantalla de 2025-09-30 13-33-37" src="https://github.com/user-attachments/assets/1766d42e-5027-427e-884d-36d8db4a38d6" />

### Justificació de l’elecció
He escollit 8 GB de RAM i 6 nuclis de CPU perquè son recursos suficients per executar el sistema operatiu sense problemes.  
Pel que fa a l’emmagatzematge, he assignat 80 GB de disc: 40 GB per a Ubuntu i 40 GB reservats per a Windows, que instal·larem més endavant.

<img width="877" height="472" alt="Captura de pantalla de 2025-09-30 13-33-43" src="https://github.com/user-attachments/assets/583c71df-b9e5-4cfe-9da5-7977680ef400" />

- Una vegada ja hem configurat tots els parametres ja podem iniciar la maquina per començar la instal·lació.  
- Després de configurar la distribució del teclat, etc., quan arribem a l’apartat de tipus d’instal·lació, seleccionem “Alguna altra cosa” (per a fer una instal·lació manual) i així definirem les particions:

<img width="1280" height="883" alt="Captura de pantalla de 2025-10-01 23-36-14" src="https://github.com/user-attachments/assets/61aab1d1-ae62-4be9-b1b4-0b7fffb32c1e" />

---

### Justificació de l’elecció de particions
- He creat una partició /boot/efi d’1 GB per assegurar una arrencada correcta en mode UEFI i deixar espai suficient per a futures actualitzacions del gestor d’arrencada.  
- L'arrel / té 25 GB, suficients per allotjar el sistema operatiu, paquets i aplicacions addicionals sense problemes.  
- La partició /home ocupa la resta de l’espai disponible per emmagatzemar documents i configuracions personals.  
- Finalment, he afegit una partició swap de 4 GB. Tot i que no seria estrictament necessària, ja que el host disposa de 32 GB de RAM, pot resultar útil en casos puntuals.  
- Un cop creades totes les particions, seleccionem la / (arrel) com a destinació d’instal·lació del sistema i procedim amb la instal·lació d’Ubuntu.
  
---
## INSTAL·LACIÓ DE WINDOWS

A conitnuació carreguem la iso del windows 10 i iniciem la màquina per començar la instalacio
<img width="991" height="778" alt="Captura de pantalla de 2025-10-02 15-01-19" src="https://github.com/user-attachments/assets/06e7044c-1c75-44be-a8e9-4f468417366e" />

Quan arribessim a la pantalla de la ubicació d'instalació del windows em de triar la partició de 40gb que hem deixat lliure
<img width="812" height="700" alt="Captura de pantalla de 2025-10-02 14-55-41" src="https://github.com/user-attachments/assets/42107a86-00f7-4ed7-898c-522feefa7871" />

Una vegada acabi la instalació, al següent apartat procedirem a recuperar el grub.

---
## GESTORS D'ARRENCADA PER A INSTAL·LACIONS DUALS
En una instal·lació dual boot amb Ubuntu i Windows, el gestor d’arrencada s’encarrega de permetre escollir quin sistema operatiu iniciar.
Ubuntu utilitza GRUB (GRand Unified Bootloader) com a gestor d’arrencada principal, mentre que Windows fa servir el seu propi gestor (Windows Boot Manager).

Quan s’instal·la Windows després d’Ubuntu, el seu instal·lador sobreescriu el sector d’arrencada (MBR o EFI), i això fa que el GRUB quedi eliminat o inactiu.
A conseqüència d’això, el sistema arrenca directament a Windows i no mostra el menú per escollir Ubuntu.

---
###  Arrencar amb Super Grub Disk

Primer accedim als paràmetres de la màquina a l'apartat d'emmagatzematge i sel·leccionem la iso de Super Grub Disk
<img width="883" height="556" alt="image" src="https://github.com/user-attachments/assets/9bf0f958-951d-44cf-8d97-18ab91396807" />

A continuació iniciem el boot menu de la màquina i sel·leccionem el supergrub
<img width="641" height="570" alt="image" src="https://github.com/user-attachments/assets/cb17e061-2254-4eb2-9fa2-1feb4d2135e2" />

Triem la opció de detect and show boot methods
<img width="641" height="570" alt="image" src="https://github.com/user-attachments/assets/510aad5d-0b44-40c6-ac4e-03957fda92bc" />

I per ultim seleccionem el kernel de linux
<img width="641" height="570" alt="image" src="https://github.com/user-attachments/assets/0258d2f4-a58f-4991-9220-c84683fd059b" />

---
### Iniciar Ubuntu 

Una vegada ha iniciat Ubuntu fem el següent:

1. Obrir el terminal.
2. Reinstal·lar GRUB amb:
   sudo apt install --reinstall grub-pc
<img width="692" height="411" alt="image" src="https://github.com/user-attachments/assets/69d5a9f7-a6dc-4ed9-a533-cd6bbd1a3786" />

3. Quan ho demani, seleccionar **/dev/sda** com a dispositiu.
<img width="692" height="411" alt="image" src="https://github.com/user-attachments/assets/22aac8e4-d365-483b-b21a-e6ad79629602" />

---

### 🔍 Identificar la partició EFI

Podem utilitzar aquesta comanda per a llistar les particions del disc:

sudo fdisk -l
Identificar a quin **/dev/sda** està el **Sistema EFI**.
En el meu cas està a **/dev/sda7**.
<img width="693" height="410" alt="image" src="https://github.com/user-attachments/assets/ff262242-bca7-4a4d-a104-aeb304275efa" />

Muntar la partició EFI:


sudo mount /dev/sda7 /boot/efi
<img width="590" height="60" alt="image" src="https://github.com/user-attachments/assets/821ed4d8-532c-4091-9a53-c55ba474a0d8" />

---
###  Reinstal·lar GRUB en mode UEFI

Un cop muntada la partició EFI, executar la comanda següent per reinstal·lar GRUB en mode UEFI:

sudo grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=Ubuntu
<img width="853" height="85" alt="image" src="https://github.com/user-attachments/assets/f4dab4c3-1b5e-49ea-b106-86e09b93a35f" />

---
### 📝 Editar la configuració de GRUB

Obrir l’arxiu de configuració de GRUB:

sudo nano /etc/default/grub

Comentar les línies següents:

#GRUB_TIMEOUT_STYLE=hidden
#GRUB_TIMEOUT=0

I descomentar aquesta línia:
GRUB_DISABLE_OS_PROBER=false

<img width="700" height="583" alt="image" src="https://github.com/user-attachments/assets/d4bf452a-b394-4815-9b36-89a5222e3bc6" />


Guardar l’arxiu i actualitzar la configuració de GRUB:


sudo update-grub
<img width="718" height="182" alt="image" src="https://github.com/user-attachments/assets/df1e83c2-f0af-4990-b9df-fbcd88147f42" />


---

### 🧾 Configurar l’ordre d’arrencada EFI

Instal·lar l’eina **efibootmgr** per gestionar l’ordre d’arrencada:

sudo apt-get install efibootmgr

Comprovar l’ordre actual amb la comanda:
sudo efibootmgr
<img width="1208" height="237" alt="image" src="https://github.com/user-attachments/assets/39cd956d-57d0-47fa-b50a-0e7f984497aa" />

Verificar que **Ubuntu** sigui el primer i **Windows** el segon.
En aquest cas, l’ordre és **0006,0004**.

Si no estiguessin en ordre, cal modificar-ho amb:


sudo efibootmgr -o 0006,0004


---

### 🔁 Comprovació final

Apagar la màquina i comprovar que:

* El **GRUB** apareix correctament en iniciar.
* Tant **Ubuntu** com **Windows** es poden iniciar des del menú del GRUB sense errors.

<img width="999" height="399" alt="image" src="https://github.com/user-attachments/assets/e3f72ed2-bee3-4eae-8aae-e2891e248b5c" />

---

## 💾 PUNTS DE RESTAURACIÓ

Els punts de restauració ens permeten tornar el sistema a un estat anterior en cas d’error o configuració incorrecta. A Ubuntu, aquesta funció es pot gestionar amb l’eina Timeshift.

### 🗂️ Emmagatzematge

Abans de crear punts de restauració, afegim un disc addicional de 15 GB a la màquina virtual, que s’utilitzarà exclusivament per guardar les còpies del sistema.
Obrir la configuració de la màquina virtual a VirtualBox.

🔹 Pas 1: Afegir el disc a VirtualBox
Anar a la secció Emmagatzematge i afegim un nou disc virtual de 15GB com a disc secundari
<img width="747" height="430" alt="Captura de pantalla de 2025-10-07 12-43-37" src="https://github.com/user-attachments/assets/5b0d7397-994a-4031-bcc2-7d780e8a015c" />

Guardar els canvis i iniciar la màquina virtual.

🔹 Pas 2: Crear la partició al nou disc

Un cop Ubuntu ha iniciat, obrim un terminal i fem el següent:

sudo fdisk /dev/sdb


Per veure l’ajuda dins fdisk, premer m.

Crear una nova partició:

Premer n per a crear una nova partició.

Tipus de partició:

p → Primary (partició principal)

e → Extended (contenidor per a particions lògiques)
Seleccionem p.

Número de partició: 1 (per defecte).

Acceptar la mida per defecte (tota la capacitat disponible, 15 GB).

Finalment, escriure els canvis i sortir (w).

🔹 Pas 3: Formatejar la partició

Després de crear la partició, cal formatar-la amb sistema de fitxers ext4:

sudo mkfs.ext4 /dev/sdb1
<img width="806" height="276" alt="image" src="https://github.com/user-attachments/assets/4e5e0943-14f6-4f4a-b0fa-adeadc17b625" />


------------
### 🗂️ Creació de fitxers de prova
Per a després comprovar que els punts de restauració funcionen crearem un fitxer i un directori al Escriptori mateix.
Per exemple:
sudo touch hola
sudo mkdir adeu
<img width="600" height="120" alt="image" src="https://github.com/user-attachments/assets/6a4f0f3d-669b-4d5f-8859-61bf2a8af957" />


### ⚙️ Instal·lació del Timeshift

Obrir la terminal i obtenim permisos d’administrador:
sudo su

Instal·lar Timeshift amb la comanda:
apt install timeshift
<img width="811" height="148" alt="image" src="https://github.com/user-attachments/assets/9fb4b301-4af9-4204-b48d-63787a14f85a" />


Iniciar Timeshift.
Triem el tipus d'instància
<img width="1312" height="853" alt="Captura de pantalla de 2025-10-07 12-57-26" src="https://github.com/user-attachments/assets/a6eca57b-05ce-4dc7-9267-0bf53b558d07" />

Seleccionem la ubicacó de la instància. En aquest cas he seleccionat el disc sdb1 que hem creat abans.
<img width="1312" height="853" alt="Captura de pantalla de 2025-10-07 12-57-55" src="https://github.com/user-attachments/assets/3bdb4478-26d0-41aa-9d69-7d7647edd474" />

Seleccionem els nivells de les instantànies segons preferència (a l'arrencada, diària, setmanal, etc.)
<img width="1312" height="853" alt="Captura de pantalla de 2025-10-07 12-59-02" src="https://github.com/user-attachments/assets/c48fa450-5902-4a5e-885f-b87f9e8c5774" />

Configurem quins directoris volem incloure o excloure de la instanània. En aquest cas he exclos el directori root i he inclos tots els arxius del directori home.
<img width="1312" height="853" alt="Captura de pantalla de 2025-10-07 12-59-44" src="https://github.com/user-attachments/assets/f78a7938-d0db-4f35-8a0b-e6365d5d197c" />

###  Verificació

Un cop acabada de configurar reinciem el sistema per comprovar que la instanània s'ha configurat bé. Les instanànies d'arrencada es creen 10 minuts després d'inicar el sistema per tant ens esperem a que la faci.
Obrim el timeshift i comprovem que s'hagi creat la instantània
<img width="1312" height="853" alt="Captura de pantalla de 2025-10-07 13-04-26" src="https://github.com/user-attachments/assets/9bfaedd9-e4db-4eea-a801-010a925d21b7" />


Després eliminem el fitxer i directori de prova que hem creat abans a l'Escriptori
sudo rm hola 
sudo rm -r adeu

Restaurem la instanània i comprovem que l'arxiu i el directori de prova s'han restaurat
<img width="1240" height="825" alt="Captura de pantalla de 2025-10-07 13-10-25" src="https://github.com/user-attachments/assets/533a916c-7128-4f55-aafa-50e2e0c5a60e" />


------------

## 🌐 CONFIGURACIÓ DE XARXA

A continuació es configura la xarxa de la màquina virtual per utilitzar una IP manual.

⚙️ Paràmetres de xarxa

Obrir la configuració de VirtualBox i establir l’adaptador de xarxa en mode Pont
<img width="861" height="498" alt="Captura de pantalla de 2025-10-07 12-44-02" src="https://github.com/user-attachments/assets/185352bd-4971-4576-9f73-2b4eeed1da01" />

Iniciar la màquina virtual.

### 🌐 Opció 1: Configuració d’una IP manual des de la interfície gràfica d’Ubuntu

A més de fer-ho per terminal amb Netplan, també és possible configurar una IP estàtica des dels paràmetres de xarxa d’Ubuntu mitjançant la interfície gràfica.


Anem a Paràmetres i a la barra lateral, seleccionem Xarxa

Si estem connectats per cable, fem clic a la icona ⚙️ de cablejat

Si és una connexió Wi-Fi, fem el mateix sobre la xarxa sense fils activa.

A la pestanya IPv4, canviem el mode de Automàtic (DHCP) a Manual.

Omplim els camps següents amb la configuració desitjada:

<img width="1275" height="806" alt="Captura de pantalla de 2025-10-07 13-42-34" src="https://github.com/user-attachments/assets/9f2b5e17-c940-432a-bfef-bef884a74c3c" />


Seleccionem Aplica per desar la configuració.

Provem amb un ping 
<img width="1241" height="788" alt="Captura de pantalla de 2025-10-07 13-43-31" src="https://github.com/user-attachments/assets/03e7d2fe-079f-4760-b09d-c85d6a45e5f2" />

### 📄 Opció 2: Configurar una IP manual editant el fitxer de configuració de Netplan.

Obrir el fitxer de configuració amb la comanda

sudo nano /etc/netplan/01-network-manager-all.yaml


Modificar el fitxer per definir una IP estàtica. Exemple:
<img width="1121" height="729" alt="Captura de pantalla de 2025-10-07 13-59-48" src="https://github.com/user-attachments/assets/053d3f08-2867-4cae-ab95-a4fc8e2d510d" />

Desar els canvis i aplicar la configuració:

sudo netplan apply

Comprovem amb un ping

<img width="1121" height="729" alt="Captura de pantalla de 2025-10-07 14-07-07" src="https://github.com/user-attachments/assets/9cafa21a-c34b-4fbd-9217-0fc29600bf8d" />


------------




## COMANDES GENERALS I INSTAL·LACIONS

En aquest apartat veurem com instal·lar i desinstalar un paquet (per exemple Audacity) utilitzant diferents eines disponibles al sistema:
APT, Aptitude, DPKG i la gestió de repositoris.

### 1. Instal·lació i desinstal·lació amb APT

APT (Advanced Package Tool) és el gestor de paquets principal d’Ubuntu i permet instal·lar, actualitzar o eliminar programes des de la línia d’ordres.

🔹 Actualitzar repositoris i sistema
sudo apt update (actualitza la llista de paquets disponibles segons el fitxer /etc/apt/sources.list.)
<img width="816" height="230" alt="image" src="https://github.com/user-attachments/assets/c628b5d8-10c7-4486-b713-49ae7c82271e" />

sudo apt upgrade (instal·la les noves versions disponibles sense afegir nous paquets)
<img width="827" height="130" alt="image" src="https://github.com/user-attachments/assets/70687aeb-b1c4-466a-b452-bb5a11d2faee" />cclear

🔹 Instal·lar Audacity
sudo apt install audacity
<img width="1041" height="669" alt="image" src="https://github.com/user-attachments/assets/2c26665f-bf17-4f1a-8bac-3806c1c4f51b" />

🔹 Verificar dependències
apt-cache depends audacity
<img width="820" height="587" alt="image" src="https://github.com/user-attachments/assets/3a1aab20-e472-4084-bdc1-ffc12dcba73a" />

🔹 Desinstal·lar Audacity
sudo apt remove audacity

🔹 Eliminar completament (fitxers de configuració inclosos)
sudo apt purge audacity
<img width="820" height="587" alt="image" src="https://github.com/user-attachments/assets/ecdea8aa-442a-49a9-adcc-a02ac4a828b9" />

🔹 Netejar el sistema
sudo apt autoremove
<img width="815" height="145" alt="image" src="https://github.com/user-attachments/assets/72833c21-86da-45bb-84af-2dc683df0146" />

sudo apt clean
Això elimina paquets que ja no s’utilitzen i fitxers descarregats.

🧮 2. Instal·lació i desinstal·lació amb APTITUDE

Aptitude és una interfície avançada (en mode text o gràfic) que utilitza APT, però gestiona millor les dependències.

🔹 Instal·lar Aptitude
sudo apt install aptitude
<img width="804" height="317" alt="image" src="https://github.com/user-attachments/assets/af8dfced-76f5-46a4-aa86-b0bf1ecdac91" />

🔹 Instal·lar Audacity amb Aptitude
sudo aptitude install audacity
<img width="801" height="454" alt="image" src="https://github.com/user-attachments/assets/7b39bef1-62bd-4d1a-a7a4-318323b30724" />

🔹 Desinstal·lar Audacity
sudo aptitude remove audacity


🔹 Eliminar completament Audacity i configuracions
sudo aptitude purge audacity
<img width="810" height="221" alt="image" src="https://github.com/user-attachments/assets/2ff90393-b45f-4ced-b803-227c906a9fbf" />

Aptitude recorda les dependències instal·lades i pot eliminar-les automàticament si ja no són necessàries.

📦 3. Instal·lació i desinstal·lació amb DPKG

DPKG és el gestor de paquets de baix nivell utilitzat per APT.
Permet instal·lar paquets .deb de manera directa, sense necessitat d’Internet.

🔹 Instal·lar un paquet .deb

Primer cal descarregar el paquet

Després anem al directori on ens em baixat el paquet i executem aquesta comanda
sudo dpkg -i audacity_*.deb
<img width="949" height="137" alt="image" src="https://github.com/user-attachments/assets/b400756a-ec14-4835-aa59-96aced03ebe6" />

Si es produeix un error per dependències trencades com ha passat amb l'audacity, podem arreglar-ho amb
sudo apt --fix-broken install
<img width="810" height="221" alt="image" src="https://github.com/user-attachments/assets/2ff90393-b45f-4ced-b803-227c906a9fbf" />

🔹 Consultar informació del paquet
dpkg -s audacity
<img width="978" height="321" alt="image" src="https://github.com/user-attachments/assets/fdb8f097-1a52-464f-90b5-0d9a2eecda84" />

🔹 Desinstal·lar el paquet
sudo dpkg -r audacity

🔹 Eliminar completament (incloent configuració)
sudo dpkg -P audacity
<img width="804" height="175" alt="image" src="https://github.com/user-attachments/assets/a76047be-cee4-46dc-a08d-30c77b7b6128" />





🌍 4. Gestió de repositoris

A Ubuntu, els repositoris són les fonts oficials o externes des d’on es poden descarregar i instal·lar paquets.
Aquests repositoris estan definits al seguent arxiu i permeten afegir tant fonts oficials com repositoris personalitzats.

sudo nano /etc/apt/sources.list.d/ubuntu.sources


Afegim una nova entrada amb el següent format:

Types: deb
URIs: http://archive.ubuntu.com/ubuntu/
Suites: noble noble-updates noble-backports
Components: main restricted universe multiverse
Architectures: amd64
<img width="816" height="325" alt="image" src="https://github.com/user-attachments/assets/9b6f5629-9460-46f9-a719-ebfb2d59f589" />


🔸 Aquesta línia exemplifica com podríem afegir un repositori personalitzat, però en aquest cas utilitzarem els repositoris oficials per instal·lar el paquet audacity.

Guardem els canvis (Ctrl + O, Enter, Ctrl + X) i actualitzem la llista de paquets:

sudo apt update

🔹 Instal·lació d’Audacity des dels repositoris oficials

Un cop actualitzada la informació dels repositoris, podem instal·lar Audacity directament utilitzant el repositori oficial d’Ubuntu (que ja inclou el component universe).

sudo apt install audacity
<img width="816" height="325" alt="image" src="https://github.com/user-attachments/assets/f300f8e4-38c9-45e5-a620-270cfe441e9f" />

🔹 Eliminació del paquet

Només el programa:

sudo apt remove audacity

Programa i fitxers de configuració:

sudo apt purge audacity
<img width="949" height="138" alt="image" src="https://github.com/user-attachments/assets/e3a990a1-7abf-408e-a2db-5f7181ff6664" />


✅ Conclusió

Cada eina (APT, Aptitude, DPKG i Repositoris) permet gestionar el programari d’Ubuntu amb diferents nivells de control:

APT és el més comú i pràctic per ús diari.

Aptitude ofereix una millor gestió automàtica de dependències.

DPKG s’utilitza per instal·lacions manuals de fitxers .deb.

Repositoris determinen d’on provenen els paquets i quin programari està disponible.


