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

DOCUMENTACIÓ
Sistemes de fitxers i particions
Mida sector
És la unitat mínima física on es guarden les dades en un disc. Per defecte la mida són 512 Bytes i no es pot modificar.

Mida block
Block (Linux) o Cluster (Windows): és la unitat mínima lògica on es guarden les dades a nivell de sistema operatiu. Per defecte són 4096 Bytes (8 sectors). Aquesta mida sí que la podem modificar quan es formateja la partició.

Per comprovar la informació del sistema de fitxers (inclosa la mida del bloc), utilitzem la comanda tune2fs -l sobre la partició (ex: /dev/sda): <img width="1177" height="679" alt="image" src="https://github.com/user-attachments/assets/98b46b7e-9105-4f8d-af43-83b65e3a36b9" />

Amb les següents comandes podem veure la diferència entre el que ocupa el contingut real i l'espai que ocupa en el sistema operatiu (degut a la mida del bloc): <img width="677" height="284" alt="image" src="https://github.com/user-attachments/assets/4af00d47-cc8a-4605-b755-d005e14d1160" />

Fragmentació interna
És quan els blocs són massa grans per al que es vol guardar i es desaprofita espai al disc. Si tenim molts arxius menuts, ens interessa una mida de bloc petita.

Fragmentació externa
És quan un arxiu no està guardat en blocs consecutius de la memòria i els seus accessos són més lents.

Comprovació: Utilitzem e4defrag -c /home (l'opció -c és per fer el check): <img width="1217" height="624" alt="image" src="https://github.com/user-attachments/assets/6ff20bcb-24fb-4a17-81fa-cc19cb2c927c" />

Desfragmentació: Si calgués desfragmentar, utilitzem la comanda sense la -c: e4defrag /home <img width="1192" height="678" alt="image" src="https://github.com/user-attachments/assets/36d4caba-d735-47c3-bab4-9add5dc3606f" />

Sistemes de fitxers
Per mirar el sistema de fitxers muntat i el seu tipus utilitzem df -Th: <img width="677" height="284" alt="image" src="https://github.com/user-attachments/assets/f3b3d86f-e394-4b08-aae0-a7be89890749" />

Tipus de formatació
Baix nivell: Esborra arxius i intenta arreglar sectors defectuosos físicament.

Mig nivell: No esborra arxius físicament, però arregla sectors defectuosos (lent).

Alt nivell: Només esborra la taula del sistema de fitxers (ràpid).

Gestió de particions
1. Preparació del maquinari (Afegir disc) Afegim un disc de 10GB a la màquina virtual: <img width="696" height="631" alt="image" src="https://github.com/user-attachments/assets/c2e5a7e2-13fb-436b-a15e-f6b7c9f4e339" /> <img width="820" height="557" alt="image" src="https://github.com/user-attachments/assets/32df7532-6d1f-4eb5-a52d-5cd014b9aa11" />

Comprovem que el sistema detecta el nou disc amb fdisk -l: <img width="690" height="275" alt="image" src="https://github.com/user-attachments/assets/e71c35ec-fc3b-4c92-8a62-6fc315eb2d4a" />

2. GPARTED Instal·lem l'eina: sudo apt install gparted: <img width="1110" height="380" alt="image" src="https://github.com/user-attachments/assets/c6878018-bb95-4a49-9f33-bd41b6720155" />

Veiem el disc de 10GB: <img width="1143" height="457" alt="image" src="https://github.com/user-attachments/assets/6cda428a-8922-4752-b4a8-1b4c45736fe9" />

Formatem les particions (ext4 i NTFS): <img width="821" height="212" alt="image" src="https://github.com/user-attachments/assets/1b72434c-b3f1-480d-9344-ef6f46b1b725" />

3. Muntatge i persistència (Fstab) Creem punt de muntatge i muntem manualment. Apareix lost+found: <img width="963" height="602" alt="image" src="https://github.com/user-attachments/assets/f31800c3-f077-43ad-b2e9-f0defb2cf26b" />

Si reiniciem, desapareix: <img width="965" height="338" alt="image" src="https://github.com/user-attachments/assets/8496e2e9-2efc-416d-9845-0da4356d815a" />

Editem /etc/fstab per fer-ho persistent: <img width="1021" height="561" alt="image" src="https://github.com/user-attachments/assets/0bb09748-512b-4fab-aaf8-97c027723864" />

Gestió d’usuaris i grups i permisos
Per a la gestió d'usuaris, podem utilitzar la terminal o l'eina gràfica gnome-system-tools. <img width="909" height="725" alt="image" src="https://github.com/user-attachments/assets/a078a584-dc04-4966-8ba8-667e930d5ea5" />

Fitxers de configuració essencials
1. Fitxer /etc/passwd Conté la informació bàsica de l'usuari (nom, UID, GID, home, shell). <img width="872" height="768" alt="image" src="https://github.com/user-attachments/assets/33919886-e31d-4d61-a025-799319b8e33d" /> Explicació de camps: El número 1000 és l'UID (User ID), que s'incrementa per a cada nou usuari. L'últim camp defineix l'intèrpret de comandes (shell).

2. Fitxer /etc/group Defineix els grups del sistema i els seus membres. <img width="872" height="768" alt="image" src="https://github.com/user-attachments/assets/5c6bfb4f-8955-44c5-a82b-d23e47d9ab13" />

3. Fitxer /etc/shadow Conté les contrasenyes xifrades i la informació de caducitat. <img width="872" height="768" alt="image" src="https://github.com/user-attachments/assets/5287c296-fb08-4f95-ba1e-c3285f4699bb" />

4. Fitxer /etc/gshadow Conté contrasenyes de grup i defineix qui és l'administrador del grup. <img width="863" height="693" alt="image" src="https://github.com/user-attachments/assets/738696e1-202e-4ad4-8e82-f538ec734585" />

Creació i gestió bàsica
Afegir usuaris (adduser): És la comanda recomanada, interactiva. <img width="902" height="452" alt="image" src="https://github.com/user-attachments/assets/1c62e41f-66ad-4f53-9d28-e294b55b814d" /> Les carpetes del home apareixen en iniciar sessió per primera vegada: <img width="548" height="112" alt="image" src="https://github.com/user-attachments/assets/0ca9c0cb-4f8b-4d36-a6fb-78964072cb04" />

Afegir usuaris (useradd): Més manual, cal especificar opcions. Exemple creant l'usuari gina2: <img width="711" height="575" alt="image" src="https://github.com/user-attachments/assets/03911c44-b5d2-45cb-a479-3cbf9c94696e" />

Visualitzar nous usuaris al sistema: Podem veure els 4 últims usuaris creats (blau, roig, groc, verd) consultant el final del fitxer shadow: cat /etc/shadow | tail -4 <img width="809" height="137" alt="image" src="https://github.com/user-attachments/assets/bafb8fa9-2604-45ff-b94d-9eb2cd46c335" />

Eliminar usuaris: Si volem eliminar l'usuari i també el seu directori personal: deluser --remove-home nom_usuari <img width="707" height="338" alt="image" src="https://github.com/user-attachments/assets/0b08f07b-96e4-477f-afab-f9428ad5dff1" />

Gestió de Grups
Crear, modificar i eliminar grups: Exemple: Creem el grup asixb i afegim usuaris. Després modifiquem el nom del grup i l'eliminem. <img width="638" height="142" alt="image" src="https://github.com/user-attachments/assets/9dfc268a-e9b6-4abe-9f1a-ba8351aff799" />

Canviar el nom d'un grup: Si ens equivoquem de nom, utilitzem groupmod -n nom_nou nom_vell. Exemple: canviem parchis per dames. <img width="634" height="81" alt="image" src="https://github.com/user-attachments/assets/0543e25a-d1b4-4c26-b1b6-c059736318c6" />

Afegir usuaris a grups secundaris: Hi ha tres maneres de fer-ho:

gpasswd -a usuari grup

usermod -a -G grup usuari

adduser usuari grup

En la captura veiem com afegim els usuaris roig, verd i groc al grup parchis: <img width="752" height="158" alt="image" src="https://github.com/user-attachments/assets/d5bbd7cb-577f-46ed-b50b-e7c058f4d939" />

I un altre exemple genèric: <img width="622" height="164" alt="image" src="https://github.com/user-attachments/assets/e49f24f4-cfe7-4184-80f9-e45eec0d8013" />

Eliminar usuaris d'un grup: Utilitzem gpasswd -d o deluser usuari grup. <img width="709" height="155" alt="image" src="https://github.com/user-attachments/assets/aeea39ce-2232-4f23-8344-a2632219267b" /> <img width="742" height="70" alt="image" src="https://github.com/user-attachments/assets/d8572704-9b7b-4e17-8a30-9206b065f49e" />

Administració de grups: Amb gpasswd -A (majúscula) fem que un usuari sigui administrador d'un grup (pot gestionar membres sense ser root). <img width="653" height="139" alt="image" src="https://github.com/user-attachments/assets/ac83a8c3-c81c-42a9-845d-649cb2506969" />

Canvi de grup principal i errors d'eliminació: Podem canviar el grup principal d'un usuari amb usermod -g. <img width="742" height="70" alt="2025-11-17_13-02" src="https://github.com/user-attachments/assets/55123161-054f-4db2-abad-e65e7856a07a" /> <img width="632" height="252" alt="image" src="https://github.com/user-attachments/assets/94caf8d9-f800-44c1-947b-28e1b5dfc040" />

Si intentem eliminar un grup que és el principal d'algun usuari, el sistema no ens deixa per seguretat: <img width="684" height="74" alt="image" src="https://github.com/user-attachments/assets/284b726f-e888-4c3d-9f12-27cb7938e2e7" />

Bloqueig i Desbloqueig de Comptes
Podem bloquejar temporalment un compte perquè no pugui iniciar sessió sense esborrar-lo.

Bloquejar: usermod -L gina (Posa un ! al shadow).

Desbloquejar: usermod -U gina (Treu el !).

<img width="818" height="306" alt="image" src="https://github.com/user-attachments/assets/4897df7e-dfea-45a7-a59a-cbbf0efd322c" />

Configuració de l'entorn per defecte
Existeixen fitxers que defineixen com es creen els usuaris nous.

1. /etc/skel (Skeleton) Tot el que posem aquí (com la carpeta prova i el fitxer hola) es copiarà automàticament al home dels nous usuaris. <img width="627" height="420" alt="image" src="https://github.com/user-attachments/assets/701b7d3a-d73d-4917-8649-c8796d2821b4" />

2. /etc/adduser.conf Configuració per defecte de adduser. Aquí hem canviat que els UID comencin pel 3000. <img width="821" height="581" alt="image" src="https://github.com/user-attachments/assets/dcfd72ce-4de2-4f2e-ad81-7392dee2ef37" />

Comprovem que el canvi funciona creant l'usuari gris i veient que té l'ID 3000: <img width="624" height="111" alt="image" src="https://github.com/user-attachments/assets/3af92494-b3bb-4159-af26-53cb4ef0e2e9" />

3. /etc/login.defs Paràmetres globals com la caducitat de contrasenyes. <img width="816" height="598" alt="image" src="https://github.com/user-attachments/assets/2342a2a4-ddec-402b-9b97-28fb46b7ca1b" />

4. /etc/default/useradd Valors per defecte específics de useradd. <img width="814" height="554" alt="image" src="https://github.com/user-attachments/assets/691e61ec-c6ee-4774-b405-3406f1b29bbd" />

Personalització de l'entorn (.profile, .bashrc)
Podem editar els fitxers que s'executen en iniciar sessió per personalitzar l'experiència de l'usuari.

Modificació del .profile: S'executa a l'inici de la sessió. Aquí afegim PWD="/var/$USER" perquè l'usuari vagi directament a una carpeta de /var en entrar. També podem afegir missatges de benvinguda. <img width="818" height="586" alt="image" src="https://github.com/user-attachments/assets/688ef91d-f479-4a86-a4c2-43850cc2ce0a" /> <img width="794" height="760" alt="image" src="https://github.com/user-attachments/assets/5cb7f7aa-e404-465a-8419-47b94e4709a6" />

Comprovem el canvi de directori creant l'usuari rosa i iniciant sessió: <img width="817" height="279" alt="image" src="https://github.com/user-attachments/assets/b1bc51c7-1cca-47a9-8dc3-47d1845daabc" />

Modificació del .bashrc: S'executa en obrir una terminal. Podem afegir Alias (dreceres) o art ASCII perquè es mostri un dibuix en obrir la consola. <img width="824" height="571" alt="image" src="https://github.com/user-attachments/assets/af184873-77f8-4f6c-ad62-4619e2b7b09f" /> <img width="750" height="656" alt="image" src="https://github.com/user-attachments/assets/61650095-dd07-441e-b7b4-c8e36045fcd5" />

Modificació del .bash_logout: S'executa en tancar sessió. Hem afegit un missatge de comiat "adeu fins aviat". <img width="812" height="301" alt="image" src="https://github.com/user-attachments/assets/dea060a4-53b8-4b28-831a-b81934964c9c" /> <img width="736" height="57" alt="image" src="https://github.com/user-attachments/assets/8187d2af-5bfc-4440-9cff-83ee05225af6" />

Permisos i ACLs
Permisos estàndard (chmod)
En Linux, els permisos es divideixen en tres nivells: Usuari (propietari), Grup i Altres (others).

Creem proves i proves2. <img width="628" height="115" alt="Creació fitxers" src="https://github.com/user-attachments/assets/856cca9e-9bbb-4666-9bca-a6d4692dac0c" />

Restringim l'accés perquè "Altres" no puguin entrar-hi: chmod 750 proves <img width="628" height="115" alt="Restricció chmod" src="https://github.com/user-attachments/assets/c2ab4c30-b2e5-41a7-8ded-5e122248a16c" />

Llistes de Control d'Accés (ACL)
Permeten donar permisos a usuaris específics sense obrir el fitxer a tothom.

Mirem l'estat actual: <img width="580" height="358" alt="Comprovació getfacl" src="https://github.com/user-attachments/assets/b888da8c-915d-4693-bdf7-8747550bc268" />

Donem permís només a l'usuari roig: setfacl -m u:roig:rw- fitxer <img width="680" height="354" alt="Assignació setfacl roig" src="https://github.com/user-attachments/assets/3675629a-0d7c-4903-a535-02b6aa011c3a" />

Resultat: L'usuari blau (other) no pot entrar, però roig sí. <img width="720" height="445" alt="Creació directori compartit" src="https://github.com/user-attachments/assets/54f7c174-aee3-44a3-95ba-88a4bca210b9" /> <img width="790" height="61" alt="Error permís denegat blau" src="https://github.com/user-attachments/assets/031b3bda-9cbc-4c92-8c3b-c8463adbf38b" /> <img width="720" height="445" alt="Accés correcte roig" src="https://github.com/user-attachments/assets/e906fd02-e3cf-4e12-acb1-48d65c032379" />

Umask (Màscara de permisos)
És una màscara que resta permisos als valors base en crear nous fitxers.

Valor per defecte usuari: 0002

Valor per defecte root: 0022

<img width="485" height="154" alt="image" src="https://github.com/user-attachments/assets/677e3eda-51da-4f6d-87a1-39fb35a08f26" />

Es pot configurar globalment a /etc/login.defs o per usuari al .profile: <img width="811" height="579" alt="image" src="https://github.com/user-attachments/assets/7f0318de-c1ce-47bc-958a-34453b1b62d1" /> <img width="811" height="579" alt="image" src="https://github.com/user-attachments/assets/9ad49a0a-de5e-4da2-b6e5-c0c4b14ebb0e" />

Prova pràctica canviant l'umask a 033 (més restrictiu) en la sessió actual: <img width="664" height="267" alt="image" src="https://github.com/user-attachments/assets/8178edd7-abb5-4416-989c-e18a725502bc" /> <img width="576" height="465" alt="image" src="https://github.com/user-attachments/assets/e5bb9b6a-8fa8-42c0-a77a-6ade58e68aad" /> <img width="556" height="167" alt="image" src="https://github.com/user-attachments/assets/cc54e340-29cf-4c66-a1fc-947515a7e250" />





























----------------------------------------------
Aquí tens la secció redacactada de nou, amb les explicacions tècniques ampliades, clara i llesta per copiar al teu fitxer Markdown.

Markdown

## Gestió d’usuaris i grups i permisos

Per a la gestió d'usuaris en entorns d'escriptori, tot i que la terminal és l'eina més potent, podem instal·lar eines gràfiques clàssiques.

**Instal·lació d'eines gràfiques:**
Obrim una terminal amb permisos d'administrador (`sudo su`) i executem:
```bash
sudo apt install gnome-system-tools
Un cop instal·lat, podem buscar "Usuaris i grups" al menú d'aplicacions d'Ubuntu. És una alternativa visual per gestionar usuaris, grups i els seus paràmetres bàsics.

<img width="909" height="725" alt="image" src="https://github.com/user-attachments/assets/a078a584-dc04-4966-8ba8-667e930d5ea5" />

Fitxers de configuració implicats
El sistema operatiu Linux guarda la informació dels usuaris i grups en fitxers de text pla situats al directori /etc. És fonamental entendre l'estructura d'aquests quatre fitxers.

1. Fitxer /etc/passwd
Aquest fitxer conté la informació bàsica de tots els comptes d'usuari del sistema. Tothom té permisos per llegir aquest fitxer, per la qual cosa no s'hi guarden dades sensibles com les contrasenyes reals.

<img width="872" height="768" alt="image" src="https://github.com/user-attachments/assets/33919886-e31d-4d61-a025-799319b8e33d" />

Anàlisi d'una línia d'usuari (Exemple pauserra): Cada línia es divideix en 7 camps separats per dos punts (:):

Nom d'usuari (pauserra): El nom que s'utilitza per iniciar sessió.

Contrasenya (x): La x indica que la contrasenya no està aquí, sinó xifrada al fitxer /etc/shadow per seguretat.

UID (User ID - 1000): És l'identificador numèric únic de l'usuari.

El 0 és sempre el root (administrador).

De l'1 al 999 són usuaris del sistema (serveis).

A partir del 1000 són els usuaris normals. El primer usuari creat (pauserra) rep el 1000, el següent el 1001, i així successivament.

GID (Group ID - 1000): És l'identificador del grup principal de l'usuari. Per defecte, es crea un grup amb el mateix nom i ID que l'usuari.

GECOS / Comentaris: Camp de text lliure per posar el nom complet, número de telèfon, etc.

Directori Home (/home/pauserra): La ruta de la carpeta personal on l'usuari guardarà els seus fitxers.

Intèrpret de comandes o Shell (/bin/bash): És el programa que s'executa quan l'usuari inicia sessió i que permet introduir comandes.

Si apareix /bin/bash o /bin/sh, l'usuari pot treballar amb la terminal.

Si apareix /usr/sbin/nologin o /bin/false, l'usuari té prohibit l'accés al sistema (comú en usuaris de serveis).

2. Fitxer /etc/group
Aquest fitxer defineix els grups del sistema. Els grups serveixen per organitzar usuaris i assignar permisos de manera col·lectiva.

<img width="872" height="768" alt="image" src="https://github.com/user-attachments/assets/5c6bfb4f-8955-44c5-a82b-d23e47d9ab13" />

Els camps són: Nom del grup : Contrasenya (x) : GID : Llista de membres. Aquí podem veure quins usuaris pertanyen a un grup com a secundari (separats per comes).

3. Fitxer /etc/shadow
Aquest és un fitxer crític de seguretat. Només l'usuari root hi té accés de lectura. Conté les contrasenyes xifrades i la informació sobre la seva caducitat.

<img width="872" height="768" alt="image" src="https://github.com/user-attachments/assets/5287c296-fb08-4f95-ba1e-c3285f4699bb" />

Aquí es defineixen paràmetres com:

El hash de la contrasenya (la cadena llarga de caràcters inintel·ligibles).

Dies des de l'últim canvi de contrasenya.

Dies mínims i màxims per canviar la contrasenya (caducitat).

Dies d'avís abans que la contrasenya expiri.

4. Fitxer /etc/gshadow
Aquest fitxer és l'equivalent segur del fitxer de grups. Conté informació sensible dels grups.

<img width="863" height="693" alt="image" src="https://github.com/user-attachments/assets/738696e1-202e-4ad4-8e82-f538ec734585" />

La característica més important d'aquest fitxer, a diferència del /etc/group, és que permet definir administradors de grup. Un administrador de grup (que apareix en el tercer camp d'una línia) és un usuari que té permís per afegir o eliminar membres d'aquell grup específic, utilitzant la comanda gpasswd, sense necessitat de tenir permisos de superusuari (root).
