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
  - ### Sistemes de fitxers
  - ### Tipus de format

  - Baix nivell 
  - Mig nivell 
  - Alt nivell

  - ### Gestió de particions

  - Preparació del Maquinari
  - GPARTED
  - Comandes i muntatge (fstab)

## Gestió d’usuaris i grups i permisos

  - ### Eines gràfiques i fitxers de configuració
  - ### Comandes bàsiques de gestió
  - ### Bloqueig i desbloqueig d'usuaris
  - ### Gestió de grups i administradors
  - ### Configuració avançada (skel, login.defs)
  - ### Personalització de l'entorn (bashrc, profile)
  - ### Permisos estàndard (chmod)
  - ### Llistes de Control d'Accés (ACL)
  - ### Permisos Umask

## Gestió de processos PENDENT

## Còpies de seguretat i automatització de tasques (PENDENT)

## Quotes d’usuari (PENDENT)

-----

# DOCUMENTACIÓ

## Sistemes de fitxers i particions

  - ### Mida sector

  És la unitat mínima física on es guarden les dades en un disc. Per defecte la mida són **512 Bytes** i no la puc modificar.

  - ### Mida block

  Block (Linux) o Cluster (Windows): és la unitat mínima lògica on es guarden les dades a nivell de sistema operatiu. Per defecte són **4096 Bytes** (8 sectors).
  Aquesta mida sí que la puc modificar quan formato la partició. Cada partició del disc pot tenir una mida de bloc i un sistema de fitxers diferent.

  Per comprovar la informació del sistema de fitxers (inclosa la mida del bloc), utilitzo la comanda `tune2fs -l` sobre la partició (ex: `/dev/sda`):
  \<img width="1177" height="679" alt="image" src="[https://github.com/user-attachments/assets/98b46b7e-9105-4f8d-af43-83b65e3a36b9](https://github.com/user-attachments/assets/98b46b7e-9105-4f8d-af43-83b65e3a36b9)" /\>

  Amb les següents comandes puc veure la diferència entre el que ocupa el contingut real i l'espai que ocupa en el sistema operatiu (degut a la mida del bloc):
  \<img width="677" height="284" alt="image" src="[https://github.com/user-attachments/assets/4af00d47-cc8a-4605-b755-d005e14d1160](https://github.com/user-attachments/assets/4af00d47-cc8a-4605-b755-d005e14d1160)" /\>

  - ### Fragmentació interna

  Es produeix quan els blocs són massa grans per al fitxer que vull guardar i es desaprofita espai al disc.
  Exemple: Si tinc molts arxius menuts, m'interessa una mida de bloc petita per no malgastar espai. Si és una pel·lícula (arxiu gran), m'interessa un bloc gran per agilitzar la lectura.

  - ### Fragmentació externa

  Es produeix quan un arxiu no està guardat en blocs consecutius de la memòria. Això fa que els accessos siguin més lents i baixi el rendiment.
  Exemple: El desfragmentador de Windows. En Linux, com que el sistema ext4 està ben optimitzat, generalment no fa falta, però tinc l'eina `e4defrag` per comprovar-ho.

  **Comprovació:**
  Utilitzo `e4defrag -c /home` (l'opció `-c` és per fer el *check* o diagnòstic):
  \<img width="1217" height="624" alt="image" src="[https://github.com/user-attachments/assets/6ff20bcb-24fb-4a17-81fa-cc19cb2c927c](https://github.com/user-attachments/assets/6ff20bcb-24fb-4a17-81fa-cc19cb2c927c)" /\>

  **Desfragmentació:**
  Si calgués desfragmentar, utilitzo la comanda sense la `-c`: `e4defrag /home`
  \<img width="1192" height="678" alt="image" src="[https://github.com/user-attachments/assets/36d4caba-d735-47c3-bab4-9add5dc3606f](https://github.com/user-attachments/assets/36d4caba-d735-47c3-bab4-9add5dc3606f)" /\>

  - ### Sistemes de fitxers

  Hi ha molts sistemes de fitxers. Exemples: Windows (NTFS, FAT32), Linux (ext4, xfs). Cada sistema de fitxers està optimitzat per a uns usos concrets i té limitacions:
  \* **Compatibilitat:** Ubuntu pot accedir a NTFS i ext4, però Windows no pot accedir a ext4 nativament.
  \* **Mida:** FAT32 només permet fitxers de màxim 4GB; NTFS arriba fins a 16TB.

  Per mirar el sistema de fitxers muntat i el seu tipus utilitzo `df -Th`:
  \<img width="677" height="284" alt="image" src="[https://github.com/user-attachments/assets/f3b3d86f-e394-4b08-aae0-a7be89890749](https://github.com/user-attachments/assets/f3b3d86f-e394-4b08-aae0-a7be89890749)" /\>

  - ### Tipus de format

  - **Baix nivell:** Esborra arxius, sistema de fitxers i intenta arreglar sectors defectuosos físicament. Necessito programes específics, no ho puc fer des del S.O. convencional.
  - **Mig nivell:** No esborra els arxius físicament (es poden recuperar), però si troba sectors defectuosos els arregla (format lent).
  - **Alt nivell:** No esborra els arxius, només esborra la taula del sistema de fitxers. És molt ràpid però recuperable. Si troba sectors defectuosos els ignora (casella "format ràpid").

  - ### Gestió de particions

  Una partició és un tros físic del disc dur. Un volum és una capa d'abstracció que es posa damunt de les particions. Amb el GParted puc gestionar particions però no puc modificar la mida del bloc fàcilment un cop creada.

  **1. Preparació del maquinari (Afegir disc)**
  Primer, afegeixo un disc de 10GB a la màquina virtual:
  \<img width="696" height="631" alt="image" src="[https://github.com/user-attachments/assets/c2e5a7e2-13fb-436b-a15e-f6b7c9f4e339](https://github.com/user-attachments/assets/c2e5a7e2-13fb-436b-a15e-f6b7c9f4e339)" /\>
  \<img width="820" height="557" alt="image" src="[https://github.com/user-attachments/assets/32df7532-6d1f-4eb5-a52d-5cd014b9aa11](https://github.com/user-attachments/assets/32df7532-6d1f-4eb5-a52d-5cd014b9aa11)" /\>

  Puc comprovar que el sistema detecta el nou disc amb `fdisk -l`. A la captura veig el disc del sistema i el nou de 10GB:
  \<img width="690" height="275" alt="image" src="[https://github.com/user-attachments/assets/e71c35ec-fc3b-4c92-8a62-6fc315eb2d4a](https://github.com/user-attachments/assets/e71c35ec-fc3b-4c92-8a62-6fc315eb2d4a)" /\>

  **2. GPARTED**
  Faig un `sudo apt update` i instal·lo l'eina amb `sudo apt install gparted`:
  \<img width="1110" height="380" alt="image" src="[https://github.com/user-attachments/assets/c6878018-bb95-4a49-9f33-bd41b6720155](https://github.com/user-attachments/assets/c6878018-bb95-4a49-9f33-bd41b6720155)" /\>

  Aquí puc veure el disc de 10GB que he creat abans:
  \<img width="1143" height="457" alt="image" src="[https://github.com/user-attachments/assets/6cda428a-8922-4752-b4a8-1b4c45736fe9](https://github.com/user-attachments/assets/6cda428a-8922-4752-b4a8-1b4c45736fe9)" /\>

  Formato les dues particions: la primera en **ext4** i la segona en **NTFS**:
  \<img width="821" height="212" alt="image" src="[https://github.com/user-attachments/assets/1b72434c-b3f1-480d-9344-ef6f46b1b725](https://github.com/user-attachments/assets/1b72434c-b3f1-480d-9344-ef6f46b1b725)" /\>

  **3. Muntatge i persistència (Fstab)**
  Un cop creada la partició, cal muntar-la.
  Aquí veig com he creat un punt de muntatge. Sé que està ben muntada perquè apareix la carpeta `lost+found` (pròpia d'ext4 per a recuperació de dades en cas d'error):
  \<img width="963" height="602" alt="image" src="[https://github.com/user-attachments/assets/f31800c3-f077-43ad-b2e9-f0defb2cf26b](https://github.com/user-attachments/assets/f31800c3-f077-43ad-b2e9-f0defb2cf26b)" /\>

  Si faig un `reboot`, la partició es desmuntarà i la carpeta `lost+found` desapareixerà perquè el muntatge manual no és permanent:
  \<img width="965" height="338" alt="image" src="[https://github.com/user-attachments/assets/8496e2e9-2efc-416d-9845-0da4356d815a](https://github.com/user-attachments/assets/8496e2e9-2efc-416d-9845-0da4356d815a)" /\>

##   Per fer-ho persistent, edito el fitxer `/etc/fstab` i afegeixo la línia corresponent al nou disc:   \<img width="1021" height="561" alt="image" src="[https://github.com/user-attachments/assets/0bb09748-512b-4fab-aaf8-97c027723864](https://github.com/user-attachments/assets/0bb09748-512b-4fab-aaf8-97c027723864)" /\>          

## Gestió d’usuaris i grups i permisos

  - ### Eines gràfiques i fitxers de configuració

  Per gestionar usuaris gràficament, instal·lo les eines del sistema gnome:
  `sudo apt install gnome-system-tools`
  Busco "usuaris" al menú de cerca d'Ubuntu. És una alternativa per poder gestionar gràficament els usuaris i grups.
  \<img width="909" height="725" alt="image" src="[https://github.com/user-attachments/assets/a078a584-dc04-4966-8ba8-667e930d5ea5](https://github.com/user-attachments/assets/a078a584-dc04-4966-8ba8-667e930d5ea5)" /\>

  **Fitxers implicats:**

  **1. /etc/passwd:** Conté la informació bàsica dels usuaris. Puc veure camps com el nom, el UID (que comença per 1000 per a usuaris normals), el GID, el directori home i l'intèrpret de comandes (shell).
  \<img width="872" height="768" alt="image" src="[https://github.com/user-attachments/assets/33919886-e31d-4d61-a025-799319b8e33d](https://github.com/user-attachments/assets/33919886-e31d-4d61-a025-799319b8e33d)" /\>

  **2. /etc/group:** Conté la informació dels grups del sistema.
  \<img width="872" height="768" alt="image" src="[https://github.com/user-attachments/assets/5c6bfb4f-8955-44c5-a82b-d23e47d9ab13](https://github.com/user-attachments/assets/5c6bfb4f-8955-44c5-a82b-d23e47d9ab13)" /\>

  **3. /etc/shadow:** Emmagatzema totes les contrasenyes dels usuaris (encriptades) i la informació sobre la seva caducitat. Només és accessible per l'administrador per seguretat.
  \<img width="872" height="768" alt="image" src="[https://github.com/user-attachments/assets/5287c296-fb08-4f95-ba1e-c3285f4699bb](https://github.com/user-attachments/assets/5287c296-fb08-4f95-ba1e-c3285f4699bb)" /\>

  **4. /etc/gshadow:** Conté les contrasenyes dels grups i informació d'administració. A diferència del `/etc/group`, aquí és l'únic lloc on puc veure qui és l'usuari administrador d'un grup.
  \<img width="863" height="693" alt="image" src="[https://github.com/user-attachments/assets/738696e1-202e-4ad4-8e82-f538ec734585](https://github.com/user-attachments/assets/738696e1-202e-4ad4-8e82-f538ec734585)" /\>

  - ### Comandes bàsiques de gestió

  **Afegir usuaris:**
  Utilitzo `adduser` (més interactiu i complet):
  \<img width="902" height="452" alt="image" src="[https://github.com/user-attachments/assets/1c62e41f-66ad-4f53-9d28-e294b55b814d](https://github.com/user-attachments/assets/1c62e41f-66ad-4f53-9d28-e294b55b814d)" /\>

  Les carpetes del *home* apareixen un cop he iniciat sessió amb l'usuari que he creat.
  \<img width="548" height="112" alt="image" src="[https://github.com/user-attachments/assets/0ca9c0cb-4f8b-4d36-a6fb-78964072cb04](https://github.com/user-attachments/assets/0ca9c0cb-4f8b-4d36-a6fb-78964072cb04)" /\>

  També puc utilitzar `useradd` (més manual, com en l'exemple de l'usuari 'gina2'):
  \<img width="711" height="575" alt="image" src="[https://github.com/user-attachments/assets/03911c44-b5d2-45cb-a479-3cbf9c94696e](https://github.com/user-attachments/assets/03911c44-b5d2-45cb-a479-3cbf9c94696e)" /\>

  **Eliminar usuaris:**
  En esborrar l'usuari, el directori *home* no s'esborra automàticament; s'ha de fer manualment o amb paràmetres específics.
  \<img width="707" height="338" alt="image" src="[https://github.com/user-attachments/assets/0b08f07b-96e4-477f-afab-f9428ad5dff1](https://github.com/user-attachments/assets/0b08f07b-96e4-477f-afab-f9428ad5dff1)" /\>

  - ### Bloqueig i Desbloqueig d'usuaris

  **Estat Inicial:**
  Comprovo l'estat amb `cat /etc/shadow | grep gina`. Si el hash comença normal (ex. `$`), el compte està desbloquejat.

  **Bloqueig:**
  Executo `usermod -L gina`. L'opció `-L` (Lock) posa un signe d'exclamació (`!`) davant del hash a `/etc/shadow`, impedint l'inici de sessió.

  **Desbloqueig:**
  Executo `usermod -U gina`. L'opció `-U` (Unlock) treu el signe d'exclamació i restaura l'accés.
  \<img width="818" height="306" alt="image" src="[https://github.com/user-attachments/assets/4897df7e-dfea-45a7-a59a-cbbf0efd322c](https://github.com/user-attachments/assets/4897df7e-dfea-45a7-a59a-cbbf0efd322c)" /\>

  - ### Gestió de grups i administradors

  Creo un grup i modifico el seu nom o l'elimino:
  \<img width="638" height="142" alt="image" src="[https://github.com/user-attachments/assets/9dfc268a-e9b6-4abe-9f1a-ba8351aff799](https://github.com/user-attachments/assets/9dfc268a-e9b6-4abe-9f1a-ba8351aff799)" /\>
  \<img width="901" height="703" alt="image" src="[https://github.com/user-attachments/assets/8ac8ae4a-5ba6-4738-9c16-35f59c68e4a7](https://github.com/user-attachments/assets/8ac8ae4a-5ba6-4738-9c16-35f59c68e4a7)" /\>

  **Afegir usuaris a grups:**
  Hi ha diverses maneres d'afegir un usuari a un grup secundari (per exemple, al grup 'parchis'):
  1. `gpasswd -a roig parchis`
  2. `usermod -a -G parchis verd`
  3. `adduser groc parchis`
  \<img width="622" height="164" alt="image" src="[https://github.com/user-attachments/assets/e49f24f4-cfe7-4184-80f9-e45eec0d8013](https://github.com/user-attachments/assets/e49f24f4-cfe7-4184-80f9-e45eec0d8013)" /\>
  \<img width="752" height="158" alt="image" src="[https://github.com/user-attachments/assets/d5bbd7cb-577f-46ed-b50b-e7c058f4d939](https://github.com/user-attachments/assets/d5bbd7cb-577f-46ed-b50b-e7c058f4d939)" /\>

  **Administració de grups:**
  Amb el paràmetre `-A` (majúscula) en la comanda `gpasswd`, puc definir l'administrador del grup.
  \<img width="653" height="139" alt="image" src="[https://github.com/user-attachments/assets/ac83a8c3-c81c-42a9-845d-649cb2506969](https://github.com/user-attachments/assets/ac83a8c3-c81c-42a9-845d-649cb2506969)" /\>

  **Canvi de grup principal:**
  Utilitzo `usermod -g` (minúscula) per canviar el grup principal. Un usuari només en té un.
  \<img width="632" height="252" alt="image" src="[https://github.com/user-attachments/assets/94caf8d9-f800-44c1-947b-28e1b5dfc040](https://github.com/user-attachments/assets/94caf8d9-f800-44c1-947b-28e1b5dfc040)" /\>

  **Eliminar grups:**
  Utilitzo `groupdel`. No puc eliminar un grup si encara és el grup principal d'algun usuari.
  \<img width="684" height="74" alt="image" src="[https://github.com/user-attachments/assets/284b726f-e888-4c3d-9f12-27cb7938e2e7](https://github.com/user-attachments/assets/284b726f-e888-4c3d-9f12-27cb7938e2e7)" /\>

  - ### Configuració avançada (skel, login.defs)

  **1. /etc/skel (Skeleton):**
  Tot el que poso dins d'aquest directori es copiarà automàticament al *home* dels nous usuaris.
  He creat una carpeta `prova` i un fitxer `hola` a `/etc/skel`, i verifico que apareixen als nous usuaris.
  \<img width="627" height="420" alt="image" src="[https://github.com/user-attachments/assets/701b7d3a-d73d-4917-8649-c8796d2821b4](https://github.com/user-attachments/assets/701b7d3a-d73d-4917-8649-c8796d2821b4)" /\>

  **2. /etc/adduser.conf:**
  Configuro el rang d'IDs. He modificat perquè els nous usuaris comencin a partir del UID 3000.
  \<img width="821" height="581" alt="image" src="[https://github.com/user-attachments/assets/dcfd72ce-4de2-4f2e-ad81-7392dee2ef37](https://github.com/user-attachments/assets/dcfd72ce-4de2-4f2e-ad81-7392dee2ef37)" /\>

  **3. /etc/login.defs:**
  Estableixo polítiques globals, com la caducitat de contrasenyes.
  \<img width="816" height="598" alt="image" src="[https://github.com/user-attachments/assets/2342a2a4-ddec-402b-9b97-28fb46b7ca1b](https://github.com/user-attachments/assets/2342a2a4-ddec-402b-9b97-28fb46b7ca1b)" /\>

  **4. /etc/default/useradd:**
  Conté els valors per defecte específics per a la comanda `useradd`.
  \<img width="814" height="554" alt="image" src="[https://github.com/user-attachments/assets/691e61ec-c6ee-4774-b405-3406f1b29bbd](https://github.com/user-attachments/assets/691e61ec-c6ee-4774-b405-3406f1b29bbd)" /\>

  Comprovo que els canvis funcionen creant un usuari i mirant el seu UID:
  \<img width="624" height="111" alt="image" src="[https://github.com/user-attachments/assets/3af92494-b3bb-4159-af26-53cb4ef0e2e9](https://github.com/user-attachments/assets/3af92494-b3bb-4159-af26-53cb4ef0e2e9)" /\>

  - ### Personalització de l'entorn (bashrc, profile)

  Puc modificar fitxers ocults al *home* de l'usuari (o a l'skel) per personalitzar l'experiència:

  **.profile:** He afegit `PWD="/var/$USER"` perquè en iniciar sessió s'estableixi aquest camí i un missatge de benvinguda.
  \<img width="818" height="586" alt="image" src="[https://github.com/user-attachments/assets/688ef91d-f479-4a86-a4c2-43850cc2ce0a](https://github.com/user-attachments/assets/688ef91d-f479-4a86-a4c2-43850cc2ce0a)" /\>

  **.bashrc:** He afegit un àlies (`alias connexió="ls -la"`) i un dibuix ASCII que es mostra en obrir la terminal.
  \<img width="824" height="571" alt="image" src="[https://github.com/user-attachments/assets/af184873-77f8-4f6c-ad62-4619e2b7b09f](https://github.com/user-attachments/assets/af184873-77f8-4f6c-ad62-4619e2b7b09f)" /\>

  **.bash\_logout:** He configurat un missatge de comiat ("adeu fins aviat") que surt al tancar la sessió.
  \<img width="812" height="301" alt="image" src="[https://github.com/user-attachments/assets/dea060a4-53b8-4b28-831a-b81934964c9c](https://github.com/user-attachments/assets/dea060a4-53b8-4b28-831a-b81934964c9c)" /\>

  - ### Permisos estàndard (chmod)

  En Linux, els permisos es divideixen en Propietari, Grup i Altres.
  Creo un directori `proves` i un fitxer `proves2`.
  \<img width="628" height="115" alt="Creació fitxers" src="[https://github.com/user-attachments/assets/856cca9e-9bbb-4666-9bca-a6d4692dac0c](https://github.com/user-attachments/assets/856cca9e-9bbb-4666-9bca-a6d4692dac0c)" /\>

  Vull restringir l'accés a "Altres". Utilitzo `chmod`:
  \* **750** per al directori (Tots propietari, lectura/execució grup, cap altres).
  \* **640** per al fitxer (Lectura/escriptura propietari, lectura grup, cap altres).
  \<img width="628" height="115" alt="Restricció chmod" src="[https://github.com/user-attachments/assets/c2ab4c30-b2e5-41a7-8ded-5e122248a16c](https://github.com/user-attachments/assets/c2ab4c30-b2e5-41a7-8ded-5e122248a16c)" /\>

  - ### Llistes de Control d'Accés (ACL)

  Si vull donar permís a un usuari concret (ex: 'roig') sense obrir el fitxer a tothom, utilitzo ACLs.

  1. Comprovo l'estat amb `getfacl`.
  \<img width="580" height="358" alt="Comprovació getfacl" src="[https://github.com/user-attachments/assets/b888da8c-915d-4693-bdf7-8747550bc268](https://github.com/user-attachments/assets/b888da8c-915d-4693-bdf7-8747550bc268)" /\>

  2. Assigno permisos amb `setfacl -m u:roig:rw- fitxer`. Això dona lectura i escriptura a l'usuari 'roig'.
  \<img width="680" height="354" alt="Assignació setfacl roig" src="[https://github.com/user-attachments/assets/3675629a-0d7c-4903-a535-02b6aa011c3a](https://github.com/user-attachments/assets/3675629a-0d7c-4903-a535-02b6aa011c3a)" /\>
  *(El símbol `+` al final dels permisos indica presència d'ACL).*

  **Comprovació:**
  L'usuari 'blau' (altres) té l'accés denegat:
  \<img width="790" height="61" alt="Error permís denegat blau" src="[https://github.com/user-attachments/assets/031b3bda-9cbc-4c92-8c3b-c8463adbf38b](https://github.com/user-attachments/assets/031b3bda-9cbc-4c92-8c3b-c8463adbf38b)" /\>
  L'usuari 'roig' (amb ACL) hi pot accedir:
  \<img width="720" height="445" alt="Accés correcte roig" src="[https://github.com/user-attachments/assets/e906fd02-e3cf-4e12-acb1-48d65c032379](https://github.com/user-attachments/assets/e906fd02-e3cf-4e12-acb1-48d65c032379)" /\>

  - ### Permisos Umask

  L'Umask és una "màscara" que resta permisos als valors base (777 per directoris, 666 per fitxers) quan es creen de nou.

  \* **Valors:** r=4, w=2, x=1.
  \* **Umask típic:** 002 (permet escriptura grup) o 022 (root, més restrictiu).

  \<img width="485" height="154" alt="image" src="[https://github.com/user-attachments/assets/677e3eda-51da-4f6d-87a1-39fb35a08f26](https://github.com/user-attachments/assets/677e3eda-51da-4f6d-87a1-39fb35a08f26)" /\>

  **Configuració:**
  Puc canviar l'umask global a `/etc/login.defs` o per usuari a `.profile`.
  \<img width="811" height="579" alt="image" src="[https://github.com/user-attachments/assets/7f0318de-c1ce-47bc-958a-34453b1b62d1](https://github.com/user-attachments/assets/7f0318de-c1ce-47bc-958a-34453b1b62d1)" /\>

  **Prova pràctica:**
  1. Executo `umask 033` (treu escriptura i execució a grup i altres).
  \<img width="576" height="465" alt="image" src="[https://github.com/user-attachments/assets/e5bb9b6a-8fa8-42c0-a77a-6ade58e68aad](https://github.com/user-attachments/assets/e5bb9b6a-8fa8-42c0-a77a-6ade58e68aad)" /\>
  2. En crear nous fitxers, veig que s'aplica la nova restricció automàticament.
  \<img width="556" height="167" alt="image" src="[https://github.com/user-attachments/assets/cc54e340-29cf-4c66-a1fc-947515a7e250](https://github.com/user-attachments/assets/cc54e340-29cf-4c66-a1fc-947515a7e250)" /\>
