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
- ### Tipus de formatació
  - Baix nivell 
  - Mig nivell 
  - Alt nivell
- ### Gestió de particions
  - Preparació del Maquinari
  - GPARTED
  - Comandes i muntatge (fstab)
## Gestió d’usuaris i grups i permisos
- ### Fitxers de configuració d'usuaris (/etc/passwd, shadow, group)
- ### Creació i gestió d'usuaris i grups
- ### Bloqueig de comptes
- ### Configuració avançada (skel, login.defs)
- ### Personalització de l'entorn (.bashrc, .profile)
## Permisos avançats
- ### Permisos estàndard i ACLs
- ### Umask

-----------------------------------------------

# DOCUMENTACIÓ

## Sistemes de fitxers i particions

- ### Mida sector
  És la unitat mínima física on es guarden les dades en un disc. Per defecte la mida són **512 Bytes** i no es pot modificar per programari, ja que ve definida de fàbrica.

- ### Mida block
  Block (Linux) o Cluster (Windows): és la unitat mínima lògica on es guarden les dades a nivell de sistema operatiu. Per defecte solen ser **4096 Bytes** (8 sectors).
  Aquesta mida sí que la podem modificar quan es formateja la partició. Cada partició del disc pot tenir una mida de bloc i un sistema de fitxers diferent.

  Per comprovar la informació del sistema de fitxers (inclosa la mida del bloc), utilitzem la comanda `tune2fs -l` sobre la partició (ex: `/dev/sda`):
  <img width="1177" height="679" alt="image" src="https://github.com/user-attachments/assets/98b46b7e-9105-4f8d-af43-83b65e3a36b9" />

  Amb les següents comandes podem veure la diferència entre el que ocupa el contingut real i l'espai que ocupa en el sistema operatiu (degut a la mida del bloc):
  <img width="677" height="284" alt="image" src="https://github.com/user-attachments/assets/4af00d47-cc8a-4605-b755-d005e14d1160" />

- ### Fragmentació interna
  És quan els blocs són massa grans per al que es vol guardar i es desaprofita espai al disc.
  Exemple: Si tenim molts arxius menuts (fitxers de text), ens interessa una mida de bloc petita per no malgastar espai. Si és una pel·lícula (arxiu gran), ens interessa un bloc gran per agilitzar la lectura i reduir la quantitat de blocs a gestionar.

- ### Fragmentació externa
  És quan un arxiu no està guardat en blocs consecutius de la memòria i els seus accessos són més lents, per tant baixa el rendiment, ja que el capçal del disc ha de saltar de banda a banda.
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

- ### Tipus de formatació
  - **Baix nivell:** Esborra arxius, sistema de fitxers i intenta arreglar sectors defectuosos físicament magnèticament. Necessitem programes específics del fabricant, no es pot fer des del S.O. convencional fàcilment.
  - **Mig nivell:** No esborra els arxius físicament (es poden recuperar amb software forense), però si troba sectors defectuosos els marca per no utilitzar-los (format lent).
  - **Alt nivell:** No esborra els arxius, només esborra la taula d'assignació del sistema de fitxers. És molt ràpid però recuperable. Si troba sectors defectuosos els ignora (equivalent a la casella "formato rápido").

- ### Gestió de particions
  Una partició és un tros físic del disc dur aïllat lògicament. Un volum és una capa d'abstracció que es posa damunt de les particions. Amb el GParted podem gestionar particions.

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
  Un cop creada la partició, cal muntar-la per poder accedir-hi.
  Aquí veiem com hem creat un punt de muntatge. Sabem que està ben muntada perquè apareix la carpeta `lost+found` (pròpia d'ext4 per a recuperació de dades):
  <img width="963" height="602" alt="image" src="https://github.com/user-attachments/assets/f31800c3-f077-43ad-b2e9-f0defb2cf26b" />

  Si fem un `reboot`, la partició es desmuntarà i la carpeta `lost+found` desapareixerà, ja que el muntatge manual no és permanent:
  <img width="965" height="338" alt="image" src="https://github.com/user-attachments/assets/8496e2e9-2efc-416d-9845-0da4356d815a" />

  Per fer-ho persistent, editem el fitxer `/etc/fstab` i afegim la línia corresponent al UUID del nou disc:
  <img width="1021" height="561" alt="image" src="https://github.com/user-attachments/assets/0bb09748-512b-4fab-aaf8-97c027723864" />
    
-----------------------------------------------------------------------------------------------

## Gestió d’usuaris i grups i permisos

Per a la gestió d'usuaris, podem utilitzar la terminal (mètode recomanat) o eines gràfiques.
Per instal·lar l'eina gràfica clàssica: `sudo apt install gnome-system-tools`.
Es pot cercar com a "Usuaris i grups" al menú d'Ubuntu.

<img width="909" height="725" alt="image" src="https://github.com/user-attachments/assets/a078a584-dc04-4966-8ba8-667e930d5ea5" />

### Fitxers de configuració essencials

El sistema emmagatzema la informació dels comptes en fitxers de text pla situats a `/etc`.

**1. Fitxer `/etc/passwd`**
Conté la informació bàsica de l'usuari. Tothom té permís de lectura sobre aquest fitxer.
<img width="872" height="768" alt="image" src="https://github.com/user-attachments/assets/33919886-e31d-4d61-a025-799319b8e33d" />

Cada línia representa un usuari i té 7 camps separats per dos punts (`:`):
1.  **Nom d'usuari:** El nom per fer login (ex: `pauserra`).
2.  **Contrasenya:** Apareix una `x`. Indica que la contrasenya està xifrada a `/etc/shadow`.
3.  **UID (User ID):** Identificador numèric únic de l'usuari. Els usuaris normals solen començar pel 1000.
4.  **GID (Group ID):** Identificador del grup principal de l'usuari.
5.  **Comentari (GECOS):** Informació extra com nom complet, telèfon, etc.
6.  **Directori Home:** La carpeta personal de l'usuari (ex: `/home/pauserra`).
7.  **Intèrpret de comandes (Shell):** El programa que s'executa en iniciar sessió (ex: `/bin/bash`). Si posa `/bin/false` o `/usr/sbin/nologin`, l'usuari no pot iniciar sessió.

**2. Fitxer `/etc/group`**
Defineix els grups del sistema i els seus membres.
<img width="872" height="768" alt="image" src="https://github.com/user-attachments/assets/5c6bfb4f-8955-44c5-a82b-d23e47d9ab13" />

Camps:
1.  **Nom del grup.**
2.  **Contrasenya del grup:** Sol ser una `x`.
3.  **GID:** Identificador numèric del grup.
4.  **Membres:** Llista d'usuaris que pertanyen a aquest grup com a secundari.

**3. Fitxer `/etc/shadow`**
Conté les contrasenyes xifrades i informació de caducitat. Només l'usuari *root* pot llegir-lo per seguretat.
<img width="872" height="768" alt="image" src="https://github.com/user-attachments/assets/5287c296-fb08-4f95-ba1e-c3285f4699bb" />

**4. Fitxer `/etc/gshadow`**
Conté informació segura dels grups, com contrasenyes de grup i, molt important, **qui són els administradors del grup**. A diferència de `/etc/group`, aquí podem veure qui té permisos per gestionar el grup.
<img width="863" height="693" alt="image" src="https://github.com/user-attachments/assets/738696e1-202e-4ad4-8e82-f538ec734585" />

---

### Comandes bàsiques de gestió

**Afegir usuaris:**
- `adduser nom`: És un script interactiu (més fàcil). Crea el home, demana contrasenya i dades personals.
- `useradd nom`: Comanda de baix nivell. Crea l'usuari però sense home ni contrasenya per defecte (s'han d'especificar amb opcions).

Exemple creació amb `adduser`:
<img width="902" height="452" alt="image" src="https://github.com/user-attachments/assets/1c62e41f-66ad-4f53-9d28-e294b55b814d" />

Les carpetes apareixen un cop hem iniciat sessió amb l'usuari creat:
<img width="548" height="112" alt="image" src="https://github.com/user-attachments/assets/0ca9c0cb-4f8b-4d36-a6fb-78964072cb04" />

Exemple creació amb `useradd`:
<img width="711" height="575" alt="image" src="https://github.com/user-attachments/assets/03911c44-b5d2-45cb-a479-3cbf9c94696e" />

**Gestió de grups:**
Crear grups amb `addgroup` o `groupadd`. Per eliminar un grup: `groupdel`.
Si intentem esborrar un grup que és el principal d'un usuari, el sistema no ens deixarà.
<img width="901" height="703" alt="image" src="https://github.com/user-attachments/assets/8ac8ae4a-5ba6-4738-9c16-35f59c68e4a7" />

**Esborrar usuaris:**
Si utilitzem `userdel nom`, l'usuari s'esborra però la seva carpeta home es queda.
Per esborrar-ho tot utilitzem `deluser --remove-home nom`.
<img width="707" height="338" alt="image" src="https://github.com/user-attachments/assets/0b08f07b-96e4-477f-afab-f9428ad5dff1" />

### Bloqueig i Desbloqueig Temporal d'un Compte

**Estat Inicial**
Comanda: `cat /etc/shadow | grep gina`
L'entrada comença amb el hash (ex. `$`). Compte desbloquejat.

**Bloqueig del Compte**
Comanda: `usermod -L gina`
L'opció `-L` (Lock) posa un signe d'exclamació (`!`) davant del hash a `/etc/shadow`, impedint el login.

**Desbloqueig del Compte**
Comanda: `usermod -U gina`
L'opció `-U` (Unlock) treu l'exclamació i restaura l'accés.

<img width="818" height="306" alt="image" src="https://github.com/user-attachments/assets/4897df7e-dfea-45a7-a59a-cbbf0efd322c" />

### Gestió avançada de Grups

Tres maneres d'afegir un usuari a un grup secundari:
1. `adduser usuari grup` (Més senzill)
2. `usermod -aG grup usuari` (Important posar la `-a` d'append, sinó l'esborra dels altres grups)
3. `gpasswd -a usuari grup`

<img width="622" height="164" alt="image" src="https://github.com/user-attachments/assets/e49f24f4-cfe7-4184-80f9-e45eec0d8013" />

**Administrador de grup:**
Amb la comanda `gpasswd -A usuari grup` convertim l'usuari en administrador del grup (pot afegir i treure gent d'aquell grup sense ser root).
<img width="653" height="139" alt="image" src="https://github.com/user-attachments/assets/ac83a8c3-c81c-42a9-845d-649cb2506969" />

**Canviar grup principal:**
Amb `usermod -g grup usuari` (minúscula) canviem el grup principal (GID a /etc/passwd).
<img width="632" height="252" alt="image" src="https://github.com/user-attachments/assets/94caf8d9-f800-44c1-947b-28e1b5dfc040" />

---

### Configuració i automatització d'usuaris

Existeixen fitxers que defineixen com es creen els usuaris nous.

**1. `/etc/skel` (Skeleton)**
Tot el que posem en aquesta carpeta es copiarà automàticament al directori `/home` dels nous usuaris que creem. És útil per posar manuals, plantilles o configuracions per defecte.
<img width="627" height="420" alt="image" src="https://github.com/user-attachments/assets/701b7d3a-d73d-4917-8649-c8796d2821b4" />

**2. `/etc/adduser.conf`**
Configuració per defecte de la comanda `adduser`. Podem canviar el rang d'UIDs, el directori home per defecte, etc.
<img width="821" height="581" alt="image" src="https://github.com/user-attachments/assets/dcfd72ce-4de2-4f2e-ad81-7392dee2ef37" />

**3. `/etc/login.defs`**
Paràmetres globals de seguretat, com la caducitat de contrasenyes i la creació de correu del sistema.
<img width="816" height="598" alt="image" src="https://github.com/user-attachments/assets/2342a2a4-ddec-402b-9b97-28fb46b7ca1b" />

**4. `/etc/default/useradd`**
Valors per defecte específics de la comanda `useradd` (shell per defecte, data d'expiració, etc.).
<img width="814" height="554" alt="image" src="https://github.com/user-attachments/assets/691e61ec-c6ee-4774-b405-3406f1b29bbd" />

### Personalització de l'entorn (.bashrc i .profile)

Aquests fitxers ocults es troben al home de l'usuari i s'executen en iniciar sessió.

* **.profile:** S'executa a l'inici de la sessió de login. Ideal per mostrar missatges de benvinguda.
    Exemple: Afegir un dibuix ASCII o canviar el directori inicial.
    <img width="794" height="760" alt="image" src="https://github.com/user-attachments/assets/5cb7f7aa-e404-465a-8419-47b94e4709a6" />

* **.bashrc:** S'executa cada vegada que s'obre una terminal nova. Ideal per crear `alias` (dreceres de comandes) o configurar el prompt.
    Exemple: `alias connexio="ls -la"`
    <img width="824" height="571" alt="image" src="https://github.com/user-attachments/assets/af184873-77f8-4f6c-ad62-4619e2b7b09f" />

* **.bash_logout:** S'executa quan l'usuari tanca la sessió. Útil per netejar la pantalla o mostrar missatges de comiat.
    <img width="812" height="301" alt="image" src="https://github.com/user-attachments/assets/dea060a4-53b8-4b28-831a-b81934964c9c" />

---

## Permisos avançats

### Permisos estàndard i restriccions (chmod)

En Linux, els permisos es divideixen en tres nivells: Usuari (propietari), Grup i Altres (others).

Exemple pràctic: Creem un directori `proves` i un fitxer `proves2`.
<img width="628" height="115" alt="Creació fitxers" src="https://github.com/user-attachments/assets/856cca9e-9bbb-4666-9bca-a6d4692dac0c" />

Si volem que la resta d'usuaris (others) no puguin llegir ni modificar res, utilitzem `chmod`:
* `750` per al directori (Propietari: tot, Grup: lectura/execució, Altres: res).
* `640` per al fitxer (Propietari: rw, Grup: r, Altres: res).

<img width="628" height="115" alt="Restricció chmod" src="https://github.com/user-attachments/assets/c2ab4c30-b2e5-41a7-8ded-5e122248a16c" />

### Llistes de Control d'Accés (ACL)

Els permisos estàndard són rígids. Les ACL permeten donar permisos a usuaris específics sense obrir el fitxer a tothom.

1.  Comprovem permisos amb `getfacl`.
    <img width="580" height="358" alt="Comprovació getfacl" src="https://github.com/user-attachments/assets/b888da8c-915d-4693-bdf7-8747550bc268" />

2.  Assignem permís només a un usuari (ex: `roig`) amb `setfacl`.
    Sintaxi: `setfacl -m u:roig:rw- fitxer`
    <img width="680" height="354" alt="Assignació setfacl roig" src="https://github.com/user-attachments/assets/3675629a-0d7c-4903-a535-02b6aa011c3a" />
    *(El símbol `+` al final dels permisos indica presència d'ACLs).*

3.  **Resultat:** L'usuari `blau` (que és 'other') no pot entrar, però l'usuari `roig` (gràcies a l'ACL) sí que pot.
    <img width="790" height="61" alt="Error permís denegat blau" src="https://github.com/user-attachments/assets/031b3bda-9cbc-4c92-8c3b-c8463adbf38b" />
    <img width="720" height="445" alt="Accés correcte roig" src="https://github.com/user-attachments/assets/e906fd02-e3cf-4e12-acb1-48d65c032379" />

### Umask

- **Conceptes bàsics:**
  * **r (Llegir):** 4 | **w (Escriure):** 2 | **x (Executar):** 1
  * **Permisos Base:** Directoris `777`, Arxius `666`.

- **Què és l'Umask?**
  És una màscara que **resta** permisos als valors base en crear un fitxer nou.
  * **Usuari normal:** `0002` (Resta escriptura a 'altres').
  * **Root:** `0022` (Resta escriptura a 'grup' i 'altres').

  <img width="485" height="154" alt="image" src="https://github.com/user-attachments/assets/677e3eda-51da-4f6d-87a1-39fb35a08f26" />

- **Càlcul:** `Permís Final = Base - Umask`.
  Exemple (Arxiu, umask 002): `666 - 002 = 664` (`rw-rw-r--`).

- **Configuració:**
  * **Global:** `/etc/login.defs`.
      <img width="811" height="579" alt="image" src="https://github.com/user-attachments/assets/7f0318de-c1ce-47bc-958a-34453b1b62d1" />
  * **Per usuari:** `.profile` o `.bashrc`.
      <img width="811" height="579" alt="image" src="https://github.com/user-attachments/assets/9ad49a0a-de5e-4da2-b6e5-c0c4b14ebb0e" />

- **Prova pràctica:**
  Canviem l'umask temporalment amb `umask 033`. Els nous fitxers es crearan amb menys permisos.
  <img width="576" height="465" alt="image" src="https://github.com/user-attachments/assets/e5bb9b6a-8fa8-42c0-a77a-6ade58e68aad" />
  <img width="556" height="167" alt="image" src="https://github.com/user-attachments/assets/cc54e340-29cf-4c66-a1fc-947515a7e250" />
