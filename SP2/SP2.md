---
layout: default
title: "Sprint 2: Instal·lació, Configuració de Programari de Base i Gestió de Fitxers"
---

# ÍNDEX
## Sistemes de fitxers i particions
- ### Mida sector
- ### Mida block
- ### Fragmentació interna
- ### Fragmentació externa
- ### Tipus de formatge
  - Baix nivell 
  - Mig nivell 
  - Alt nivell
- ### Gestió de particions
  - Preparació del Maquinari
  - GPARTED
  - Comandes i muntatge (fstab)
## Gestió d’usuaris i grups i permisos
## Gestió de processos 
## Còpies de seguretat i automatització de tasques
## Quotes d’usuari


-----------------------------------------------

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
  - **Baix nivell:** Esborra arxius, sistema de fitxers i intenta arreglar sectors defectuosos físicament. Necessitem programes específics, no es pot fer des del S.O. convencional.
  - **Mig nivell:** No esborra els arxius físicament (es poden recuperar), però si troba sectors defectuosos els arregla (format lent).
  - **Alt nivell:** No esborra els arxius, només esborra la taula del sistema de fitxers. És molt ràpid però recuperable. Si troba sectors defectuosos els ignora (casella "formato rápido").

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

Tasques realitzades sobre .bashrc, .bash_logout i .profile
1. Afegir un dibuix ASCII al .bashrc
<img width="750" height="656" alt="image" src="https://github.com/user-attachments/assets/61650095-dd07-441e-b7b4-c8e36045fcd5" />

Cada vegada que un usuari obre una terminal, apareix el dibuix.

2. Afegir un missatge a .bash_logout
<img width="736" height="57" alt="image" src="https://github.com/user-attachments/assets/8187d2af-5bfc-4440-9cff-83ee05225af6" />

Quan l’usuari tanca sessió, es mostra "adeu fins aviat".

3. Afegir un dibuix ASCII i missatge de benvinguda a .profile
<img width="794" height="760" alt="image" src="https://github.com/user-attachments/assets/5cb7f7aa-e404-465a-8419-47b94e4709a6" />

Així, en iniciar sessió, qualsevol usuari veu el missatge personalitzat.








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




