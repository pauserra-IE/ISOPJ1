---
layout: default
title: "Sprint 2: Instal·lació, Configuració de Programari de Base i Gestió de Fitxers"
---

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

bloquejar i desbloquejar un usuari
