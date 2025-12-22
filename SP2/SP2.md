---
layout: default
title: "Sprint 2: Instal·lació, Configuració de Programari de Base i Gestió de Fitxers"
---

# Índex
- [Sistemes de fitxers i particions](#sistemes-de-fitxers-i-particions)
  - [Mida sector](#mida-sector)
  - [Mida block](#mida-block)
  - [Fragmentació interna](#fragmentació-interna)
  - [Fragmentació externa](#fragmentació-externa)
  - [Tipus de formateig](#tipus-de-formateig)
   - [Baix nivell](#baix-nivell)
   - [Mig nivell](#mig-nivell)
   - [Alt nivell](#alt-nivell)
- [Gestió de particions](#gestió-de-particions)
- [Gestió d’usuaris i grups i permisos](#gestió-dusuaris-i-grups-i-permisos)
- [Gestió de processos](#gestió-de-processos)
- [Còpies de seguretat i automatització de tasques](#teoria-copies-de-seguretat)
  - [Teoria Còpies de Seguretat](#teoria-copies-de-seguretat)
  - [Teoria Comandes Backups](#teoria-comandes-backups)
  - [Pràctica comandes Bàsiques](#TASQUES-PRÀCTIQUES)
    - [cp](#cp)
    - [rsync](#rsync)
    - [dd](#dd)
  - [Pràctica programes Backups](#pràctica-programes-backups)
    - [Deja-Dup](#deja-dup)
    - [Duplicity](#duplicity)
  - [Teoria Automatització (scripts, cron i anacron)](#5.-Teoria-i-Pràctica-d'Automatització:-scripts,-Cron-i-Anacron)
)
  - [Pràctica automatització](#TASQUES-PRÀCTIQUES)
    - [cron](#cron)
    - [anacron](#anacron)
- [Quotes d’usuari](#quotes-dusuari)


--------------------------------------------------------------




# DOCUMENTACIÓ

## Sistemes de fitxers i particions

- ### Mida sector
  És la unitat mínima física on es guarden les dades en un disc. Per defecte la mida són **512 Bytes** i no es pot modificar.

- ### Mida block
  Block (Linux) o Cluster (Windows): és la unitat mínima lògica on es guarden les dades a nivell de sistema operatiu. Per defecte són **4096 Bytes** (8 sectors).
  Aquesta mida sí que la podem modificar quan es formateja la partició. Cada partició del disc pot tenir una mida de bloc i un sistema de fitxers diferent.

  Per comprovar la informació del sistema de fitxers (inclosa la mida del bloc), utilitzem la comanda `tune2fs -l` sobre la partició (ex: `/dev/sda`):
  <img width="1177" height="679" alt="image" src="https://github.com/user-attachments/assets/98b46b7e-9105-4f8d-af43-83b65e3a36b9" />

  Amb les següents comandes podem veure la diferència entre el que ocupa el contingut real i l'espai que ocupa en el sistema operatiu (degut a la mida del bloc):
  <img width="677" height="284" alt="image" src="https://github.com/user-attachments/assets/4af00d47-cc8a-4605-b755-d005e14d1160" />

- ### Fragmentació interna
  És quan els blocs són massa grans per al que es vol guardar i es desaprofita espai al disc.
  Exemple: Si tenim molts arxius menuts, ens interessa una mida de bloc petita per no malgastar espai. Si és una pel·lícula (arxiu gran), ens interessa un bloc gran per agilitzar la lectura.

- ### Fragmentació externa
  És quan un arxiu no està guardat en blocs consecutius de la memòria i els seus accessos són més lents, per tant baixa el rendiment.
  Exemple: El desfragmentador de Windows. En Linux, com que el sistema ext4 està ben optimitzat, generalment no fa falta, però tenim l'eina `e4defrag` per comprovar-ho.

  **Comprovació:**
  Utilitzem `e4defrag -c /home` (l'opció `-c` és per fer el *check* o diagnòstic):
  <img width="1217" height="624" alt="image" src="https://github.com/user-attachments/assets/6ff20bcb-24fb-4a17-81fa-cc19cb2c927c" />

  **Desfragmentació:**
  Si calgués desfragmentar, utilitzem la comanda sense la `-c`: `e4defrag /home`
  <img width="1192" height="678" alt="image" src="https://github.com/user-attachments/assets/36d4caba-d735-47c3-bab4-9add5dc3606f" />

- ### Sistemes de fitxers
  Hi ha molts sistemes de fitxers. Exemples: Windows (NTFS, FAT32), Linux (ext4, xfs). Cada sistema de fitxers està optimitzat per a uns usos concrets i té limitacions:
  * **Compatibilitat:** Ubuntu pot accedir a NTFS i ext4, però Windows no pot accedir a ext4 nativament.
  * **Mida:** FAT32 només permet fitxers de màxim 4GB; NTFS arriba fins a 16TB.

  Per mirar el sistema de fitxers muntat i el seu tipus utilitzem `df -Th`:
  <img width="677" height="284" alt="image" src="https://github.com/user-attachments/assets/f3b3d86f-e394-4b08-aae0-a7be89890749" />

- ### Tipus de formateig
- ### **Baix nivell:**
  - Esborra arxius, sistema de fitxers i intenta arreglar sectors defectuosos físicament. Necessitem programes específics, no es pot fer des del S.O. convencional.
- ### **Mig nivell:**
  - No esborra els arxius físicament (es poden recuperar), però si troba sectors defectuosos els arregla (format lent).
- ### **Alt nivell:**
  - No esborra els arxius, només esborra la taula del sistema de fitxers. És molt ràpid però recuperable. Si troba sectors defectuosos els ignora (casella "formato rápido").

- ### Gestió de particions
  Una partició és un tros físic del disc dur. Un volum és una capa d'abstracció que es posa damunt de les particions. Amb el GParted podem gestionar particions però no podem modificar la mida del bloc fàcilment un cop creada.

  **1. Preparació del maquinari (Afegir disc)**
  Primer, afegim un disc de 10GB a la màquina virtual:
  <img width="696" height="631" alt="image" src="https://github.com/user-attachments/assets/c2e5a7e2-13fb-436b-a15e-f6b7c9f4e339" />
  <img width="820" height="557" alt="image" src="https://github.com/user-attachments/assets/32df7532-6d1f-4eb5-a52d-5cd014b9aa11" />

  Podem comprovar que el sistema detecta el nou disc amb `fdisk -l`. A la captura veiem el disc del sistema i el nou de 10GB:
  <img width="690" height="275" alt="image" src="https://github.com/user-attachments/assets/e71c35ec-fc3b-4c92-8a62-6fc315eb2d4a" />

  **2. GPARTED**
  Fem un `sudo apt update` i instal·lem l'eina amb `sudo apt install gparted`:
  <img width="1110" height="380" alt="image" src="https://github.com/user-attachments/assets/c6878018-bb95-4a49-9f33-bd41b6720155" />

  Aquí podem veure el disc de 10GB que hem creat abans:
  <img width="1143" height="457" alt="image" src="https://github.com/user-attachments/assets/6cda428a-8922-4752-b4a8-1b4c45736fe9" />

  Formatem les dues particions: la primera en **ext4** i la segona en **NTFS**:
  <img width="821" height="212" alt="image" src="https://github.com/user-attachments/assets/1b72434c-b3f1-480d-9344-ef6f46b1b725" />

  **3. Muntatge i persistència (Fstab)**
  Un cop creada la partició, cal muntar-la.
  Aquí veiem com hem creat un punt de muntatge. Sabem que està ben muntada perquè apareix la carpeta `lost+found` (pròpia d'ext4 per a recuperació de dades en cas d'error):
  <img width="963" height="602" alt="image" src="https://github.com/user-attachments/assets/f31800c3-f077-43ad-b2e9-f0defb2cf26b" />

  Si fem un `reboot`, la partició es desmuntarà i la carpeta `lost+found` desapareixerà perquè el muntatge manual no és permanent:
  <img width="965" height="338" alt="image" src="https://github.com/user-attachments/assets/8496e2e9-2efc-416d-9845-0da4356d815a" />

  Per fer-ho persistent, editem el fitxer `/etc/fstab` i afegim la línia corresponent al nou disc:
  <img width="1021" height="561" alt="image" src="https://github.com/user-attachments/assets/0bb09748-512b-4fab-aaf8-97c027723864" />
    
    
-----------------------------------------------------------------------------------------------

## Gestió d’usuaris i grups i permisos

04/11/2025
Per començar la gestió, obrim una terminal i accedim com a superusuari amb sudo su.

Gestió gràfica d'usuaris
Tot i que la gestió es fa principalment per terminal, Ubuntu disposa d'una eina gràfica clàssica per gestionar usuaris i grups. Per instal·lar-la executem:

sudo apt install gnome-system-tools

Un cop instal·lada, la podem trobar al menú d'aplicacions cercant "Usuaris i grups". Aquesta eina permet crear usuaris, assignar-los a grups i gestionar privilegis de manera visual. <img width="909" height="725" alt="image" src="https://github.com/user-attachments/assets/a078a584-dc04-4966-8ba8-667e930d5ea5" />

Fitxers de configuració essencials
Tota la informació dels usuaris i grups es guarda en quatre fitxers de text pla situats al directori /etc.

1. Fitxer /etc/passwd Aquest fitxer conté la informació pública dels comptes d'usuari. Qualsevol usuari del sistema pot llegir aquest fitxer. <img width="872" height="768" alt="image" src="https://github.com/user-attachments/assets/33919886-e31d-4d61-a025-799319b8e33d" />

Analitzem la línia de l'usuari pauserra per entendre cada camp (separats per :): pauserra:x:1000:1000:pauserra,,,:/home/pauserra:/bin/bash

pauserra: És el nom d'usuari (login name) que s'utilitza per iniciar sessió.

x: Indica que la contrasenya està guardada de manera xifrada al fitxer /etc/shadow. Antigament es guardava aquí, però per seguretat es va moure.

1000 (UID): És l'Identificador d'Usuari. El sistema operatiu identifica els usuaris per números, no per noms.

El 0 és sempre el root.

De l'1 al 999 són usuaris del sistema (dimonis i serveis).

A partir del 1000 comencen els usuaris normals. El primer usuari creat (pauserra) té el 1000, el següent el 1001, etc.

1000 (GID): És l'Identificador de Grup principal. Per defecte, es crea un grup amb el mateix nom que l'usuari.

pauserra,,, (GECOS): Camp de comentaris. Sol incloure el nom complet de l'usuari, número d'oficina o telèfon.

/home/pauserra: És el directori personal (home). Quan l'usuari entra al sistema, apareixerà en aquesta carpeta.

/bin/bash: És l'intèrpret de comandes (Shell).

Què és l'intèrpret? És el programa que s'executa quan l'usuari inicia sessió. Fa d'intermediari entre l'usuari i el nucli (kernel) del sistema. Recull les comandes que escrivim, les processa i retorna el resultat. Si aquí poséssim /bin/false o /usr/sbin/nologin, l'usuari no podria entrar al sistema.

2. Fitxer /etc/group Aquest fitxer defineix els grups del sistema i quins usuaris en formen part. <img width="872" height="768" alt="image" src="https://github.com/user-attachments/assets/5c6bfb4f-8955-44c5-a82b-d23e47d9ab13" />

Cada línia indica: NomDelGrup : x : GID : LlistaUsuaris

Nom del grup: Identificador textual.

x: Contrasenya del grup (no s'utilitza habitualment).

GID: Identificador numèric del grup.

Membres: Llista d'usuaris que tenen aquest grup com a grup secundari.

3. Fitxer /etc/shadow Aquest fitxer és crític per a la seguretat. Conté les contrasenyes dels usuaris de forma xifrada (hash) i informació sobre la seva caducitat. Només l'usuari root té permisos per llegir-lo. <img width="872" height="768" alt="image" src="https://github.com/user-attachments/assets/5287c296-fb08-4f95-ba1e-c3285f4699bb" />

Aquí es defineix quant de temps pot durar una contrasenya, quants dies d'avís rep l'usuari abans que caduqui i si el compte està bloquejat (si el hash comença per !, l'accés està tancat).

4. Fitxer /etc/gshadow Aquest fitxer és als grups el mateix que el shadow és als usuaris. Conté les contrasenyes xifrades dels grups. <img width="863" height="693" alt="image" src="https://github.com/user-attachments/assets/738696e1-202e-4ad4-8e82-f538ec734585" />

La característica més important d'aquest fitxer, a diferència del /etc/group, és que l'últim camp permet definir els administradors del grup. Un administrador de grup pot afegir o eliminar membres d'aquell grup específic sense necessitat de tenir permisos de superusuari (root).


-----------------------------------------------------------------------------------------------
11/11/25
COMANDES BASIQUES 
afegir usuari adduser
<img width="902" height="452" alt="image" src="https://github.com/user-attachments/assets/1c62e41f-66ad-4f53-9d28-e294b55b814d" />

Les carpetes apareixen un cop hem iniciat sessió amb l'usuari que hem creat.
<img width="548" height="112" alt="image" src="https://github.com/user-attachments/assets/0ca9c0cb-4f8b-4d36-a6fb-78964072cb04" />

afegim usuari gina2 amb la comanda useradd
<img width="711" height="575" alt="image" src="https://github.com/user-attachments/assets/03911c44-b5d2-45cb-a479-3cbf9c94696e" />

grups
<img width="901" height="703" alt="image" src="https://github.com/user-attachments/assets/8ac8ae4a-5ba6-4738-9c16-35f59c68e4a7" />

al borrar el usuari no se borra el home, se ti que fer d'una altra manera
<img width="707" height="338" alt="image" src="https://github.com/user-attachments/assets/0b08f07b-96e4-477f-afab-f9428ad5dff1" />

# Bloqueig i Desbloqueig Temporal d'un Compte d'Usuari

## Estat Inicial
- **Comanda:** `cat /etc/shadow | grep gina`
- **Observació:** L'entrada de l'usuari gina comença amb el hash de la contrasenya normal (ex. `$`). Això indica que el compte està desbloquejat.

## Bloqueig del Compte
- **Comanda:** `usermod -L gina`
- **Acció:** L'opció `-L` bloqueja el compte.
- **Efecte al Fitxer:** El hash de la contrasenya a `/etc/shadow` es prefixa amb un signe d'exclamació (`!`), impedint els intents d'inici de sessió. Exemple: `gina:!$y$j9T...`

## Desbloqueig del Compte
- **Comanda:** `usermod -U gina`
- **Acció:** L'opció `-U` desbloqueja el compte.
- **Efecte al Fitxer:** El signe d'exclamació (`!`) es treu, restaurant l'estat desbloquejat del compte i permetent a l'usuari iniciar sessió de nou.

<img width="818" height="306" alt="image" src="https://github.com/user-attachments/assets/4897df7e-dfea-45a7-a59a-cbbf0efd322c" />
creem el grup asixb i tres usuaris ivan pau iker aaron modifiquem el nom del grup i l'eliminem
<img width="638" height="142" alt="image" src="https://github.com/user-attachments/assets/9dfc268a-e9b6-4abe-9f1a-ba8351aff799" />


tres maneres d'afegir un usuari a un grup
<img width="622" height="164" alt="image" src="https://github.com/user-attachments/assets/e49f24f4-cfe7-4184-80f9-e45eec0d8013" />


amb la segona comanda i el parametre -A en majuscula es el admin del grup


<img width="653" height="139" alt="image" src="https://github.com/user-attachments/assets/ac83a8c3-c81c-42a9-845d-649cb2506969" />


amb la -g minuscula es modifica el grup principal d'un usuari però no s'afegeix al grup
<img width="632" height="252" alt="image" src="https://github.com/user-attachments/assets/94caf8d9-f800-44c1-947b-28e1b5dfc040" />




-----------------------------------------------------------------------------------------------

17/11/25


Creo quatre usuaris nous amb la comanda adduser:

adduser blau
adduser roig
adduser groc
adduser verd


Per comprovar les últimes línies del fitxer /etc/shadow executo:

cat /etc/shadow | tail -4

<img width="809" height="137" alt="image" src="https://github.com/user-attachments/assets/bafb8fa9-2604-45ff-b94d-9eb2cd46c335" />

Aquest fitxer guarda informació sensible, com les contrasenyes xifrades i les dates d’expiració. Només l’administrador el pot consultar.

Creació d’un grup

Creo un grup nou:

<img width="634" height="81" alt="image" src="https://github.com/user-attachments/assets/0543e25a-d1b4-4c26-b1b6-c059736318c6" />

Si m’equivoco amb el nom, el puc canviar amb:

groupmod -n parchis dames

Afegir usuaris al grup parchis

Puc utilitzar diverses comandes:

gpasswd -a roig parchis
usermod -a -G parchis verd
adduser groc parchis

<img width="752" height="158" alt="image" src="https://github.com/user-attachments/assets/d5bbd7cb-577f-46ed-b50b-e7c058f4d939" />

Cada eina fa el mateix: afegeix usuaris a un grup secundari.

Eliminar usuaris del grup
gpasswd -d roig parchis
deluser verd parchis

<img width="709" height="155" alt="image" src="https://github.com/user-attachments/assets/aeea39ce-2232-4f23-8344-a2632219267b" /> <img width="742" height="70" alt="image" src="https://github.com/user-attachments/assets/d8572704-9b7b-4e17-8a30-9206b065f49e" />
Canviar el grup principal d’un usuari
usermod -g parchis roig

<img width="742" height="70" alt="2025-11-17_13-02" src="https://github.com/user-attachments/assets/55123161-054f-4db2-abad-e65e7856a07a" />

L’opció -g canvia el grup principal.
Un usuari només en té un, però pot pertànyer a tants grups secundaris com vulgui.

Intentar eliminar el grup parchis
groupdel parchis

<img width="684" height="74" alt="image" src="https://github.com/user-attachments/assets/284b726f-e888-4c3d-9f12-27cb7938e2e7" />

No puc eliminar-lo perquè encara és el grup principal d’un usuari.
Un grup només es pot eliminar si cap usuari el té com a grup principal.

Fitxers de configuració importants

Aquests fitxers defineixen el comportament d’adduser i useradd:

/etc/skel: Conté fitxers predeterminats copiats al home dels nous usuaris.

/etc/adduser.conf: Configuració general d’adduser.

/etc/login.defs: Paràmetres globals de contrasenyes i comptes.

/etc/default/useradd: Valors per defecte específics de useradd.

Modificant /etc/skel
cd /etc/skel/
ls -la
mkdir prova
touch hola
ls -la

<img width="627" height="420" alt="image" src="https://github.com/user-attachments/assets/701b7d3a-d73d-4917-8649-c8796d2821b4" />

Tot allò que es trobi dins /etc/skel/ es copiarà automàticament al directori personal dels nous usuaris creats amb adduser.
Per tant, qualsevol fitxer o carpeta que hi deixo, apareixerà també al seu home.

Editant /etc/adduser.conf
<img width="821" height="581" alt="image" src="https://github.com/user-attachments/assets/dcfd72ce-4de2-4f2e-ad81-7392dee2ef37" />

Aquí modifico el rang d’identificadors d’usuari i grup perquè comencin a partir del 3000.

Editant /etc/login.defs
<img width="816" height="598" alt="image" src="https://github.com/user-attachments/assets/2342a2a4-ddec-402b-9b97-28fb46b7ca1b" />

Aquí estableixo cada quant expira la contrasenya i altres paràmetres globals.

Verificació del canvi d’UID/GID
ls /var
cat /etc/passwd | grep gris

<img width="624" height="111" alt="image" src="https://github.com/user-attachments/assets/3af92494-b3bb-4159-af26-53cb4ef0e2e9" />

Comprovo que l’usuari gris ha estat creat amb UID i GID 3000, tal com vaig configurar.

Editant /etc/default/useradd
<img width="814" height="554" alt="image" src="https://github.com/user-attachments/assets/691e61ec-c6ee-4774-b405-3406f1b29bbd" />

Creo un usuari amb:

useradd negre
cat /etc/passwd | grep negre
cat /etc/shadow | grep negre


Així verifico com useradd aplica els valors per defecte.

Contingut de /etc/skel i directors personals
clear
ls -la /etc/skel/
ls -la /home/groc
ls -la /home/verd

Modificant els fitxers inicials dels nous usuaris
.profile
<img width="818" height="586" alt="image" src="https://github.com/user-attachments/assets/688ef91d-f479-4a86-a4c2-43850cc2ce0a" />

He afegit:

PWD="/var/$USER"


Això fa que en iniciar sessió s’estableixi aquest camí com a directori actual.

.bashrc
<img width="824" height="571" alt="image" src="https://github.com/user-attachments/assets/af184873-77f8-4f6c-ad62-4619e2b7b09f" />

He afegit un àlies:

alias connexió="ls -la"

.bash_logout
<img width="812" height="301" alt="image" src="https://github.com/user-attachments/assets/dea060a4-53b8-4b28-831a-b81934964c9c" />

Aquest fitxer s’executa quan tanquem la sessió.
Hi puc afegir missatges o neteges automàtiques.

Creació d’un nou usuari (rosa)
adduser rosa

<img width="817" height="279" alt="image" src="https://github.com/user-attachments/assets/b1bc51c7-1cca-47a9-8dc3-47d1845daabc" />

Quan inicio sessió com rosa i faig pwd, apareix:

/var/rosa


És el nou directori que vaig definir a .profile.
---------------------------------------------------------------------------
TASQUES .bashrc, .bash_logout i .profile
1. Afegir un dibuix ASCII al .bashrc
<img width="750" height="656" alt="image" src="https://github.com/user-attachments/assets/61650095-dd07-441e-b7b4-c8e36045fcd5" />

Cada vegada que un usuari obre una terminal, apareix el dibuix.

2. Afegir un missatge a .bash_logout
<img width="736" height="57" alt="image" src="https://github.com/user-attachments/assets/8187d2af-5bfc-4440-9cff-83ee05225af6" />

Quan l’usuari tanca sessió, es mostra "adeu fins aviat".

3. Afegir un dibuix ASCII i missatge de benvinguda a .profile
<img width="794" height="760" alt="image" src="https://github.com/user-attachments/assets/5cb7f7aa-e404-465a-8419-47b94e4709a6" />

Així, en iniciar sessió, qualsevol usuari veu el missatge personalitzat.
-----------------------------------------------------------------







-----------------------------------------------------------------------------------------------
24/11/25


Permisos estàndard i restriccions (chmod)
En Linux, els permisos es divideixen en tres nivells: Usuari (propietari), Grup i Altres (others).

En aquest exemple, primer creem un directori proves i un fitxer proves2. Per defecte, el sistema assigna uns permisos on "Altres" (usuaris que no són el propietari ni del grup) poden tenir accés de lectura. 
<img width="628" height="115" alt="Creació fitxers" src="https://github.com/user-attachments/assets/856cca9e-9bbb-4666-9bca-a6d4692dac0c" />

Objectiu: Volem que others (la resta d'usuaris del sistema) no puguin llegir ni modificar aquests fitxers per seguretat.

Utilitzem la comanda chmod per modificar els permisos:
750 per al directori (Tots els permisos per al propietari, lectura/execució per al grup, cap per a altres).
640 per al fitxer (Lectura/escriptura per al propietari, lectura per al grup, cap per a altres).
<img width="628" height="115" alt="Restricció chmod" src="https://github.com/user-attachments/assets/c2ab4c30-b2e5-41a7-8ded-5e122248a16c" />

Llistes de Control d'Accés (ACL)
Els permisos estàndard són rígids: o dones permís a tots els "altres" o a cap. Què passa si volem que ningú entri, excepte un usuari concret (ex: l'usuari 'roig')? Aquí entren en joc les ACL (Access Control Lists).

Primer, comprovem l'estat actual dels permisos amb la comanda getfacl. Veiem que "other" està buit (---), tal com hem configurat abans. 
<img width="580" height="358" alt="Comprovació getfacl" src="https://github.com/user-attachments/assets/b888da8c-915d-4693-bdf7-8747550bc268" />

Assignació de permisos específics (setfacl): Per donar permisos només a l'usuari roig sense obrir el fitxer a tothom, utilitzem setfacl.
La sintaxi és: setfacl -m u:nom_usuari:permisos fitxer
-m: modificar.
u:roig:rw-: donem permisos de lectura i escriptura a l'usuari 'roig'.
<img width="680" height="354" alt="Assignació setfacl roig" src="https://github.com/user-attachments/assets/3675629a-0d7c-4903-a535-02b6aa011c3a" /> (Nota: En aplicar ACLs, apareix un símbol + al final dels permisos quan fem un ls -l, indicant que hi ha permisos estesos).
Comprovació d'accés
Per verificar que la configuració funciona, creem un directori anomenat compartida i fem proves d'accés real amb diferents usuaris.
<img width="720" height="445" alt="Creació directori compartit" src="https://github.com/user-attachments/assets/54f7c174-aee3-44a3-95ba-88a4bca210b9" />
Prova 1: Accés denegat (Usuari Blau) L'usuari blau forma part de "altres" i no té cap ACL específica. Com que hem restringit l'accés a "altres", quan intenta accedir-hi, el sistema li denega el permís. <img width="790" height="61" alt="Error permís denegat blau" src="https://github.com/user-attachments/assets/031b3bda-9cbc-4c92-8c3b-c8463adbf38b" />
Prova 2: Accés permès (Usuari Roig) En canvi, l'usuari roig, gràcies a la ACL que hem configurat anteriorment, sí que té permisos per entrar i treballar, tot i que la resta d'usuaris (others) no poden. <img width="720" height="445" alt="Accés correcte roig" src="https://github.com/user-attachments/assets/e906fd02-e3cf-4e12-acb1-48d65c032379" />

---------------------------------------------------------

25/11/25

## Permisos Umask

  - ### Valors dels Permisos

    Cada permís té un valor numèric (octal) i una representació en lletra:

      * **r (Read/Llegir):** 4
      * **w (Write/Escriure):** 2
      * **x (Execute/Executar):** 1
      * **- (Sense permís):** 0

  - ### Permisos Base (Per defecte)

    El sistema assigna uns permisos màxims inicials abans d'aplicar cap filtre (umask):

      * **Directoris:** `777` (`rwxrwxrwx`) -\> Necessiten la `x` per accedir-hi.
      * **Arxius:** `666` (`rw-rw-rw-`) -\> No tenen `x` per seguretat.

  - ### Què és l'Umask

    És una màscara que **resta** permisos als valors base. Determina quins permisos es prohibeixen per defecte.
    Amb la comanda `umask` podem veure la màscara actual:

      * **Usuari normal:** `0002` (Permet escriptura al grup).
      * **Root:** `0022` (Més restrictiu, prohibeix escriptura a grup i altres).

<img width="485" height="154" alt="image" src="https://github.com/user-attachments/assets/677e3eda-51da-4f6d-87a1-39fb35a08f26" />

  - ### Càlcul dels Permisos Finals

    La fórmula és: `Permís Final = Base - Umask`.
    *Exemple (Arxiu amb umask 002):* `666 - 002 = 664` (`rw-rw-r--`).
    *Exemple (Carpeta amb umask 022):* `777 - 022 = 755` (`rwxr-xr-x`).

  - ### Configuració Global

    Si volem canviar l'umask per a **tots els usuaris**, modifiquem el fitxer `/etc/login.defs`.
<img width="811" height="579" alt="image" src="https://github.com/user-attachments/assets/7f0318de-c1ce-47bc-958a-34453b1b62d1" />

  - ### Configuració per Usuari

    Si volem canviar-ho només per a un usuari concret, modifiquem el fitxer `.profile` (o `.bashrc`) al seu directori personal.
<img width="811" height="579" alt="image" src="https://github.com/user-attachments/assets/9ad49a0a-de5e-4da2-b6e5-c0c4b14ebb0e" />

  - ### Configuració Temporal i Prova Pràctica

    Podem canviar la màscara temporalment a la sessió actual amb la comanda `umask`.

    1.  **Estat inicial:** Creem directori `proves` i arxiu `proves2`. Amb `ls -l` veiem els permisos estàndard.
<img width="664" height="267" alt="image" src="https://github.com/user-attachments/assets/8178edd7-abb5-4416-989c-e18a725502bc" />

    2.  **Canvi de màscara:** Executem `umask 033` (treu escriptura i execució a grup i altres).

<img width="576" height="465" alt="image" src="https://github.com/user-attachments/assets/e5bb9b6a-8fa8-42c0-a77a-6ade58e68aad" />

    3.  **Resultat:** En crear nous elements (amb l'usuari `prova`), veiem que s'aplica la nova restricció.

<img width="556" height="167" alt="image" src="https://github.com/user-attachments/assets/cc54e340-29cf-4c66-a1fc-947515a7e250" />


---------------------------------------------------------------------------

## Gestió de processos

Un procés es defineix com una instància dinàmica que es genera quan s'executa un programa o codi. És l'element fonamental que el sistema operatiu ha de gestionar, organitzar i supervisar constantment per assegurar el bon funcionament del sistema.

Per veure els processos filtrats per usuari, podem usar la comanda `pstree -p -h [usuari]`: <img width="613" height="769" alt="image" src="https://github.com/user-attachments/assets/20abdc31-0c86-435d-bb2f-d9232f205605" />

Si volem filtrar per trobar els processos del terminal, podem fer-ho així: <img width="614" height="188" alt="image" src="https://github.com/user-attachments/assets/f420dbce-d5df-4121-b02d-613eba0b3ced" />

En aquesta imatge, veiem la sortida de `ps aux`, que mostra informació detallada sobre cada procés. Aquí expliquem què significa cada columna: <img width="614" height="690" alt="image" src="https://github.com/user-attachments/assets/166e265e-05d7-4a2c-8fdd-867173af594b" />

Si volem veure només els processos relacionats amb `gnome-terminal`, podem fer servir `ps aux | grep gnome-terminal`: <img width="612" height="99" alt="image" src="https://github.com/user-attachments/assets/459f952e-2adc-4a13-972f-fd6b00b49acb" />

Si volem finalitzar un procés, podem fer-ho amb la comanda `kill -9 [PID]`, on `[PID]` és l'identificador del procés.

Per interactuar amb processos des de la línia de comandes, podem utilitzar les següents opcions:

* Entrant a `top`, per veure els processos en temps real. Si volem aturar un procés, utilitzem `Ctrl + C`; per aturar un procés temporalment, `Ctrl + Z` i després podem veure els processos aturats amb `jobs`. Per tornar a executar un procés aturat, utilitzem `fg %[n.job]`.

  <img width="235" height="49" alt="image" src="https://github.com/user-attachments/assets/895d777c-e8c5-4c7c-96d7-3f77f4e46350" />

* Per executar un procés en segon pla, afegim l’ampersand (`&`) després del comandament. Així el procés seguirà corrent sense bloquejar la terminal:

  <img width="319" height="30" alt="image" src="https://github.com/user-attachments/assets/ae8d6692-7932-47bc-adae-709dc9ddf8e7" />

* Per canviar la prioritat d'un procés, utilitzem la comanda `renice`. Si volem llançar un procés amb una prioritat específica, podem usar `nice`:

  <img width="587" height="767" alt="image" src="https://github.com/user-attachments/assets/dbcb63ea-1b88-4840-8b73-e674cea7cf2f" />

Cada procés té dues identificacions importants assignades pel sistema operatiu:

1. **Identificador de Procés (PID):** És un número únic assignat a cada procés durant la seva existència.
2. **Usuari vinculat:** El procés s'executa sota un usuari específic, amb els permisos i restriccions que aquest usuari té.

Els processos poden canviar d'estat de manera contínua (com passar de "Actiu" a "Esperant recursos", o bé quedar en "Zombi" després d'acabar, esperant ser netejat). El sistema operatiu és el responsable de gestionar la planificació i l'ús del temps de la CPU, assegurant que tot funcioni adequadament.

Per gestionar els processos des de la línia de comandes, tenim una sèrie d'eines i comandes:

| Acció                 | Comandes Essencials    | Descripció                                                                                          |
| :-------------------- | :--------------------- | :-------------------------------------------------------------------------------------------------- |
| **Visualització**     | `ps`, `top`, `htop`    | Permeten veure l'estat dels processos, el consum de CPU i memòria, i la llista de tasques actives.  |
| **Finalització**      | `kill`, `pkill`        | Permeten aturar processos específics mitjançant el PID o per nom d'usuari o grup.                   |
| **Priorització**      | `nice`, `renice`       | Modifiquen la prioritat d'un procés, regulant quins processos poden accedir més recursos de la CPU. |
| **Gestió de Serveis** | `systemctl`, `service` | Gestionen serveis de fons o daemons, permetent iniciar, aturar o reiniciar serveis del sistema.     |

* **Permisos:** Un procés només pot accedir als recursos que l'usuari té permès.
* **Tipus d'execució:** Els processos poden funcionar com a processos interactius (en primer pla) o com a serveis de fons (daemons).



-----------------------------------------------------------------------------


01/12/25

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

#### TASCA 1. Creació d'un Script de Còpia de Seguretat

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

#### TASCA 2: Triar un programa i fer 3 tipus de copies de seguretat

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

