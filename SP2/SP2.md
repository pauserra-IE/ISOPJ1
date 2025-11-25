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
## Gestió de processos (PENDENT)
## Còpies de seguretat i automatització de tasques (PENDENT)
## Quotes d’usuari (PENDENT)


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
Obrim una terminal en sudo su
04/11/25
sudo apt install gnome-system-tools

busquem usuaris al menu de cerca d'ubuntu

Es una alternativa per poder gestionar gràficament els usuaris i grups
<img width="909" height="725" alt="image" src="https://github.com/user-attachments/assets/a078a584-dc04-4966-8ba8-667e930d5ea5" />

Fixers implicats:
1r fitxer  /etc/passwd
<img width="872" height="768" alt="image" src="https://github.com/user-attachments/assets/33919886-e31d-4d61-a025-799319b8e33d" />

explicar cada camp de la linia de usuari pauserra  (com ara que el numero 1000 que va canviant segons l'usuari  etc) 

explicar que es l'interpret

2n fitxer /etc/group
<img width="872" height="768" alt="image" src="https://github.com/user-attachments/assets/5c6bfb4f-8955-44c5-a82b-d23e47d9ab13" />


3r fitxer

estan totes les contrasenyes dels usuaris i tot el que fa referència a la caducitat de les contrasenyes

tot el que fa referencia als passwords dels grups. 
<img width="872" height="768" alt="image" src="https://github.com/user-attachments/assets/5287c296-fb08-4f95-ba1e-c3285f4699bb" />
º
4t fitxers
i tambe podem veure els usuaris que formen part d'un grup 

pero a diferencia del /etc/group , aqui es l'unic lloc on veurem qui es l'usuari administrador d'un grup
<img width="863" height="693" alt="image" src="https://github.com/user-attachments/assets/738696e1-202e-4ad4-8e82-f538ec734585" />


-----------------------------------------------------------------------------------------------
11/11/25
COMANDES BASIQUES 
COMANDA
adduser

<img width="810" height="642" alt="image" src="https://github.com/user-attachments/assets/1a5fccc3-0752-4e28-9b39-65e79d7ba25b" />

les carpetes del home es creen una vegada l'usuari inicia sessio amb la interficie gràfica
<img width="605" height="118" alt="image" src="https://github.com/user-attachments/assets/8b7dac62-21f7-43b3-a7ae-93543b8dab8b" />

COMANDA 2
useradd gina 2
passwd 


<img width="613" height="144" alt="image" src="https://github.com/user-attachments/assets/40d3752c-9dff-464d-8c10-0113bcdbf202" />


usermod -s /bin/bash gina2

ls


mkdir gina2
ls -l



<img width="716" height="396" alt="image" src="https://github.com/user-attachments/assets/a6de74cb-c9ba-48d6-99bb-1d5f751de42b" />

chown gina2:gina2 gina2
<img width="667" height="24" alt="image" src="https://github.com/user-attachments/assets/8c18b30f-9526-4369-973b-dd316509ff68" />





<img width="616" height="117" alt="image" src="https://github.com/user-attachments/assets/c06b4626-50d9-4a0a-a0c8-d028624c5b1e" />


<img width="810" height="228" alt="image" src="https://github.com/user-attachments/assets/84c32c7b-699b-4844-9e2d-7808725db6da" />

comanda deluser i userdel (arreglar)
<img width="625" height="146" alt="image" src="https://github.com/user-attachments/assets/a327b10f-517b-4133-bee0-a7922bcb9a85" />



comanda per a bloquejar i desbloquejar l'usuari
cat /etc/shadow | grep gina









afegim 4 usuaris ivan,pau,iker,aaron
<img width="781" height="184" alt="image" src="https://github.com/user-attachments/assets/0d42870a-b58e-48b0-9f32-52ac603812c8" />


groupmod -n asix asixb
cat /etc/group | grep asix


groupdel asix
cat /etc/group | grep asix




addgroup asix1r



3 mnaeres d'afegir usuaris a un grup

metode 1:
adduser ivan asix1r


metode 2

gpasswd -a pau asix1r



metode 3:
usermod -a -G asix1r iker



per comprovar que els hem afegit fem:
cat /etc/group |grep asix1r



gpasswd -A aaron asix1r
cat /etc/group cat /etc/group |grep asix1r


cat /etc/gshadow |grep asix1r

surt pero entre dos punts pq esta com a administrador : aaron :


per a eliminarlo 
gpasswd -d aaron 




podem borrar un grup encara que el grup tingui usuaris amb 
groupdel  asix1r
cat /etc/group | grep asix1r

pero els usuaris no s'eliminen




addgroup hola



usermod -g hola ivan

cat /etc/group | grep hola
no esta
cat /etc/gshadow | grep asix1r
tampoc esta

per tant amb aquesta comanda estem canviant el grup principal 

modifica el grup principal de l'usuari

comprovar amb un cat /etc/passed




-----------

17/11/25


Afegim aquests 4 usuaris
adduser blau
adduser roig
adduser groc
adduser verd


cat /etc/shadow | tail -4
<img width="809" height="137" alt="image" src="https://github.com/user-attachments/assets/bafb8fa9-2604-45ff-b94d-9eb2cd46c335" />
Serveix per mostrar les últimes 4 línies del fitxer /etc/shadow. Aquest fitxer conté informació sensible sobre les contrasenyes dels usuaris del sistema, com ara les contrasenyes encriptades, les dates d'expiració, etc.

Creem un grup
<img width="634" height="81" alt="image" src="https://github.com/user-attachments/assets/0543e25a-d1b4-4c26-b1b6-c059736318c6" />
Si ens hem equivocat de nom podem modificar-lo amb la comanda:
groupmod -n parchis dames


Afegim els usuaris al grup parchis. Podem utilitzar les seguents comandes:
gpasswd -a roig parchis
usermod -a -G parchis verd
adduser groc parchis
<img width="752" height="158" alt="image" src="https://github.com/user-attachments/assets/d5bbd7cb-577f-46ed-b50b-e7c058f4d939" />

Ara provarem d'eliminar alguns usuaris del grup. Podem utilitzar les seguents comandes:
gpasswd -d roig parchis
deluser verd parchis
<img width="709" height="155" alt="image" src="https://github.com/user-attachments/assets/aeea39ce-2232-4f23-8344-a2632219267b" />

<img width="742" height="70" alt="image" src="https://github.com/user-attachments/assets/d8572704-9b7b-4e17-8a30-9206b065f49e" />

Ara executarem la comanda
usermod -g parchis roig
<img width="742" height="70" alt="2025-11-17_13-02" src="https://github.com/user-attachments/assets/55123161-054f-4db2-abad-e65e7856a07a" />
EXPLCICACIÓ: usermod -g serveix per modificar el grup prinicipal de l'usuari. Un usuari nomes té un grup principal pero pot formar part de molts de grups 
El grup principal es pot establir de manera fixa com aquesta comanda o de manera temporal.


Ara provarem d'eliminar el grup amb la comanda
groupdel parchis
<img width="684" height="74" alt="image" src="https://github.com/user-attachments/assets/284b726f-e888-4c3d-9f12-27cb7938e2e7" />

EXPLICACIÓ: Sempre es pot eliminar un grup pero si el grup es el grup principal de l'usuari no es pot esborrar, per aixo ens salta l'error.


#### Prova de Comandes per a la Creació i Configuració d'Usuaris amb adduser i useradd
/etc/skel: Conté els arxius predeterminats per als nous usuaris (afecta adduser).
/etc/adduser.conf: Configura el comportament de adduser per a la creació d'usuaris (afecta adduser).
/etc/login.defs: Estableix paràmetres globals per a la creació d'usuaris i gestió de contrasenyes (afecta adduser i useradd).
/etc/default/useradd: Configura els valors per defecte en crear usuaris amb useradd (afecta useradd).



cd /etc/skel/
ls -la
mkdir prova
touch hola
ls -la
<img width="627" height="420" alt="image" src="https://github.com/user-attachments/assets/701b7d3a-d73d-4917-8649-c8796d2821b4" />
EXPLICACIÓ:Tot el que es troba dins del directori /etc/skel/ es copiarà automàticament al directori personal de qualsevol nou usuari creat mitjançant la comanda adduser. Això inclou fitxers i subdirectoris com el que acabem de crear. Per exemple, si creem un nou usuari, el directori prova i el fitxer hola es copiaran al directori home d'aquest usuari.


Ara procedim a editar el fitxer de configuració principal de adduser amb la comanda nano /etc/adduser.conf.

Un cop a dins, aquí establim un nou rang per als identificadors d'usuari (UID) i de grup (GID) del sistema. L'objectiu és que els nous usuaris no comencin a comptar des del 1000, sinó des del 3000.
<img width="821" height="581" alt="image" src="https://github.com/user-attachments/assets/dcfd72ce-4de2-4f2e-ad81-7392dee2ef37" />



/etc/login.defs 
Aqui establim cada quant volem que expire la contrasenya
<img width="816" height="598" alt="image" src="https://github.com/user-attachments/assets/2342a2a4-ddec-402b-9b97-28fb46b7ca1b" />




ls /var
cat /etc/passwd | grep gris
<img width="624" height="111" alt="image" src="https://github.com/user-attachments/assets/3af92494-b3bb-4159-af26-53cb4ef0e2e9" />

EXPLICACIÓ: Ha funcionat el canvi d'ID: Els números 3000:3000 demostren que l'usuari gris ha agafat correctament la nova configuració (FIRST_UID=3000 i FIRST_GID=3000) que havíem establert al fitxer.


ls -la /var/gris/
CAPTURA
EXPLICACIÓ: 




nano /etc/default/useradd

<img width="814" height="554" alt="image" src="https://github.com/user-attachments/assets/691e61ec-c6ee-4774-b405-3406f1b29bbd" />


nano /etc/default/useradd
useradd negre
cat /etc/passwd | grep negre
cat /etc/shadow | grep negre
CAPTURA
EXPLCIACIÓ



clear
ls -la /etc/skel/
ls -la /home/groc
ls -la /home/verd



cd /etc/skel/
nano .profile
<img width="818" height="586" alt="image" src="https://github.com/user-attachments/assets/688ef91d-f479-4a86-a4c2-43850cc2ce0a" />
CAPTURA
EXPLICACIÓ: hem afegir PWD="/var/$USER"





nano .bashr
<img width="824" height="571" alt="image" src="https://github.com/user-attachments/assets/af184873-77f8-4f6c-ad62-4619e2b7b09f" />
EXPLICACIÓ:hem afegit alias connexió="ls -la"


nano .bash_logout
<img width="812" height="301" alt="image" src="https://github.com/user-attachments/assets/dea060a4-53b8-4b28-831a-b81934964c9c" />
EXPLICACIÓ


adduser rosa
<img width="817" height="279" alt="image" src="https://github.com/user-attachments/assets/b1bc51c7-1cca-47a9-8dc3-47d1845daabc" />


ctrl+f5
mos logegem en rosa
i fem la comanda pwd
hauria de mostrar /var/rosa

ara fem un ls
hauria de sortir snap



TASQUES:

prova definir algo al bashrf,al bash logout i al .profile
i crear un nou usuari per comrpovar que s'ha afegit
(per exemple algun dibuix en ascii o algo xulo que es mostri)











24/11/25


<img width="628" height="115" alt="image" src="https://github.com/user-attachments/assets/856cca9e-9bbb-4666-9bca-a6d4692dac0c" />
volem que others no puguin llegir ni modificarlo
per tant farem els seguents canvis:
<img width="628" height="115" alt="image" src="https://github.com/user-attachments/assets/c2ab4c30-b2e5-41a7-8ded-5e122248a16c" />
I fem getfacl per a comprovar
<img width="580" height="358" alt="image" src="https://github.com/user-attachments/assets/b888da8c-915d-4693-bdf7-8747550bc268" />

Si de others necessitem que nomes 1 usuari pugui  llegir, escrirue i tot (per exemple l'usuari blau) fem:
Es pot fer per usuari o per grup. ho farem per a l'usuari Roig
<img width="645" height="242" alt="image" src="https://github.com/user-attachments/assets/37296eb8-e2b1-4321-9b00-969a6077bm'he canviat la mascara temoralment
f23" />

Creem una directori anomenat compartida

<img width="720" height="445" alt="image" src="https://github.com/user-attachments/assets/54f7c174-aee3-44a3-95ba-88a4bca210b9" />

Com que blau no te permisos ens surt el seguent error:
<img width="790" height="61" alt="image" src="https://github.com/user-attachments/assets/031b3bda-9cbc-4c92-8c3b-c8463adbf38b" />
En canvi Roig si que te permisos:
<img width="720" height="445" alt="image" src="https://github.com/user-attachments/assets/e906fd02-e3cf-4e12-acb1-48d65c032379" />

---------------------------------------------------------

25/11/25

Entesos, aquí tens el codi Markdown exacte amb les etiquetes d'imatge tal com me les has passat, sense modificar res, preparat per copiar i enganxar:

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

    \<img width="485" height="154" alt="image" src="[https://github.com/user-attachments/assets/677e3eda-51da-4f6d-87a1-39fb35a08f26](https://github.com/user-attachments/assets/677e3eda-51da-4f6d-87a1-39fb35a08f26)" /\>

  - ### Càlcul dels Permisos Finals

    La fórmula és: `Permís Final = Base - Umask`.
    *Exemple (Arxiu amb umask 002):* `666 - 002 = 664` (`rw-rw-r--`).
    *Exemple (Carpeta amb umask 022):* `777 - 022 = 755` (`rwxr-xr-x`).

  - ### Configuració Global

    Si volem canviar l'umask per a **tots els usuaris**, modifiquem el fitxer `/etc/login.defs`.
    \<img width="811" height="579" alt="image" src="[https://github.com/user-attachments/assets/7f0318de-c1ce-47bc-958a-34453b1b62d1](https://github.com/user-attachments/assets/7f0318de-c1ce-47bc-958a-34453b1b62d1)" /\>

  - ### Configuració per Usuari

    Si volem canviar-ho només per a un usuari concret, modifiquem el fitxer `.profile` (o `.bashrc`) al seu directori personal.
    \<img width="811" height="579" alt="image" src="[https://github.com/user-attachments/assets/9ad49a0a-de5e-4da2-b6e5-c0c4b14ebb0e](https://github.com/user-attachments/assets/9ad49a0a-de5e-4da2-b6e5-c0c4b14ebb0e)" /\>

  - ### Configuració Temporal i Prova Pràctica

    Podem canviar la màscara temporalment a la sessió actual amb la comanda `umask`.

    1.  **Estat inicial:** Creem directori `proves` i arxiu `proves2`. Amb `ls -l` veiem els permisos estàndard.

    \<img width="664" height="267" alt="image" src="[https://github.com/user-attachments/assets/8178edd7-abb5-4416-989c-e18a725502bc](https://github.com/user-attachments/assets/8178edd7-abb5-4416-989c-e18a725502bc)" /\>

    2.  **Canvi de màscara:** Executem `umask 033` (treu escriptura i execució a grup i altres).

    \<img width="576" height="465" alt="image" src="[https://github.com/user-attachments/assets/e5bb9b6a-8fa8-42c0-a77a-6ade58e68aad](https://github.com/user-attachments/assets/e5bb9b6a-8fa8-42c0-a77a-6ade58e68aad)" /\>

    3.  **Resultat:** En crear nous elements (amb l'usuari `prova`), veiem que s'aplica la nova restricció.

    \<img width="556" height="167" alt="image" src="[https://github.com/user-attachments/assets/cc54e340-29cf-4c66-a1fc-947515a7e250](https://github.com/user-attachments/assets/cc54e340-29cf-4c66-a1fc-947515a7e250)" /\>




