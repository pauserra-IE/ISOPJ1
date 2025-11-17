---
layout: default
title: "Sprint 2: Instal·lació, Configuració de Programari de Base i Gestió de Fitxers"
---

# ÍNDEX
## Sistemes de fitxers i particions
- ### Mida sector
  És la unitat mínima fisica on es guarden les dades en un disc per defecte la mida son 512 Bytes i no es pot modificar
- ### Mida block
  Block (Linux) o CLuster (Windows): és la unitat mínima lògica on es guarden les dades a nivell de sistema operatiu. Per defecte son 4096 Bytes (8 sectors).
  Aquesta mida si que la podem modificar quan es formateja la partició.
  Cada particio del disc pot tenir una mida de bloc i un sistema de fitxers diferent
- ### Fragmentació interna
  És quan els blocs son massa grans per al que es vol guardar i es desaprofita espai al disc. Exemple si es un arxiu menut interessa.... si es una pelicula interessa ...
- ### Fragmentació externa
  és quan un arxiu no esta guardat en blocvs consecutius de la memoria i els seus accessos son mes lents, per tant baixa rendiment. exemple: desfragmentado de windows. En linux com esta ben optimitzat no     fa falta pero hi ha una eina que ens diu si fa falta.
  ### Sistemes de fitxers
  Hi ha molts sistemes de fitxers. Exemples: Windows (NFTS,FAT32), linux (ext4). Cada sistema de fitxers esta optimitzat per a unes coses o unes altres i cada sistema de fitxers té una serie de limitacions. Limitacions en quant a accedir des d'un sistema opetatiu (nfts poden accedir tant ubuntu com windows però ext4 pot accedir ubuntu pero windows no). Limitacions de mida màxima de fitxers
  per exemple fat32 nomes permet 4gb, nfts 16tb, etc
  
- ### Tipus de formateig
  - Baix nivell: Esborra arxius, sistemes de fitxers i intenta arreglar sectors defectuosos però necessitem programes especifics no es pot fer a traves del sistema operatiu.   
  - Mig nivell:  No esborra els arxius, es poden recuperar els arxius i si troba sectors defectuosos si que els arregla (format lento)
  - Alt nivell: no esborra els arxiu, nomes esborra el sistema de fitxers. per tant es pot recuperar, si troba sectors defectuosos els ignora totalment (casella formato rapido)
- ### Gestió de particions
  una particio és un troç fisic del disc dur. amb el gparted podem gestionar particions per o no podem modificar la mida del bloc.
   Un volum és una capa d'abstracció que es posa damunt de les particions i/o discos
  - GPARTED
    fem un sudo apt update
    instalem gparted
    <img width="1110" height="380" alt="image" src="https://github.com/user-attachments/assets/c6878018-bb95-4a49-9f33-bd41b6720155" />
    Aqui podem veure el disc de 10gb que hem creat abans
    <img width="1143" height="457" alt="image" src="https://github.com/user-attachments/assets/6cda428a-8922-4752-b4a8-1b4c45736fe9" />

  - Comandes
    
    
    
## Gestió de processos
-----------
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

4t fitxers
i tambe podem veure els usuaris que formen part d'un grup 

pero a diferencia del /etc/group , aqui es l'unic lloc on veurem qui es l'usuari administrador d'un grup
<img width="863" height="693" alt="image" src="https://github.com/user-attachments/assets/738696e1-202e-4ad4-8e82-f538ec734585" />




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



comanda bloquejar i desbloquejar l'usuari
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
## Còpies de seguretat i automatització de tasques
## Quotes d’usuari
-----------



AFEGIR CAPTURESSSS
Afegim un disc de 10gb
<img width="696" height="631" alt="image" src="https://github.com/user-attachments/assets/c2e5a7e2-13fb-436b-a15e-f6b7c9f4e339" />
<img width="820" height="557" alt="image" src="https://github.com/user-attachments/assets/32df7532-6d1f-4eb5-a52d-5cd014b9aa11" />s



fdisl -l
captura del disk on esta el sistema operatiucl
<img width="690" height="275" alt="image" src="https://github.com/user-attachments/assets/e71c35ec-fc3b-4c92-8a62-6fc315eb2d4a" />


tune 2fs -l /dev/sda
<img width="1177" height="679" alt="image" src="https://github.com/user-attachments/assets/98b46b7e-9105-4f8d-af43-83b65e3a36b9" />


per a mirar sistema de fitxers
df -Th
<img width="677" height="284" alt="image" src="https://github.com/user-attachments/assets/f3b3d86f-e394-4b08-aae0-a7be89890749" />



afegir captura a mida de blooc

amb aquestes dues comandes veurem el que ocupa en contingut i el que ocpua en sistema operatiu
<img width="677" height="284" alt="image" src="https://github.com/user-attachments/assets/4af00d47-cc8a-4605-b755-d005e14d1160" />


formatem les dues particions 
la primera en ext4 i la segona en nfts

comanda  tune + (FALTA obrir el gparted)
<img width="821" height="212" alt="image" src="https://github.com/user-attachments/assets/1b72434c-b3f1-480d-9344-ef6f46b1b725" />



aqui explicar que hem creat l'enllaç  que apunta a un altre disco i explicar perque surt el lost+found
<img width="963" height="602" alt="image" src="https://github.com/user-attachments/assets/f31800c3-f077-43ad-b2e9-f0defb2cf26b" />


fem un reboot i expliquem perque ara si que ens surt (perque no esta montat) i despres el torem a muntar per a veure que si que surt 
<img width="965" height="338" alt="image" src="https://github.com/user-attachments/assets/8496e2e9-2efc-416d-9845-0da4356d815a" />


nem al fstab i afegim la linea
<img width="1021" height="561" alt="image" src="https://github.com/user-attachments/assets/0bb09748-512b-4fab-aaf8-97c027723864" />


ls /mnt/p1

e4defrag -c /home
esta eina et diu si necessita desfragmentar

<img width="1217" height="624" alt="image" src="https://github.com/user-attachments/assets/6ff20bcb-24fb-4a17-81fa-cc19cb2c927c" />


si cal fragmentar es la comanda
e4defrag /home
<img width="1192" height="678" alt="image" src="https://github.com/user-attachments/assets/36d4caba-d735-47c3-bab4-9add5dc3606f" />



















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



