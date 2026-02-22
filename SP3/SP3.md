---
layout: default
title: "Sprint 3: Administració de Dominis i Seguretat"
---

# Instal·lació de Domini LDAP i Unió d’un Client al Domini

## 1. Introducció
Abans de començar amb la pràctica explicaré els conceptes bàsics dels dominis LDAP, així com els elements principals que els formen. 


## 2. Teoria dels Dominis

Un **domini** és una estructura centralitzada que permet administrar usuaris, recursos i polítiques de manera unificada dins d’una xarxa.

### 2.1 Usuaris
Els usuaris representen les identitats que poden autenticar-se al domini.

- Inici de sessió centralitzat
- Gestió de credencials
- Assignació de permisos

---

### 2.2 Recursos
Els recursos són els elements compartits dins del domini.

Exemples:
- Carpetes compartides
- Impressores
- Serveis de xarxa

Els permisos d’accés als recursos es gestionen a través del domini.

---

### 2.3 Unitats Organitzatives (U.O.)
Les **Unitats Organitzatives (OU)** permeten organitzar els objectes del domini de manera jeràrquica.

Funcions principals:
- Classificar usuaris i equips
- Aplicar **GPO (Group Policy Objects)**
- Facilitar l’administració del domini

Exemples d’ús:
- OU per departaments
- OU per tipus d’usuari
- OU per equips

---

## 3. Conclusió
La correcta estructura d’usuaris, recursos i unitats organitzatives és clau per a una administració eficient d’un domini LDAP i per garantir seguretat, escalabilitat i facilitat de gestió.


## TASCA 1 CONFIGURACIÓ DEL SERVIDOR I EXEMPLES DE CADA COMANDA DE LDAP:
PASSOS PREVIS
En primer lloc configurem la xarxa NAT 

Creem l'adaptador de xarxa NAT
<img width="575" height="247" alt="image" src="https://github.com/user-attachments/assets/60c5c47d-0594-421f-a127-970a271280bd" />

i li assignem a una maquina 
<img width="492" height="229" alt="image" src="https://github.com/user-attachments/assets/5a4acedc-c3b1-45a7-aeab-1cf32910f86c" />

i a l'altra
<img width="492" height="229" alt="image" src="https://github.com/user-attachments/assets/33a3c796-83ef-434b-bb90-fe7f3c872ab8" />

PRÀCTICA

Obrim el servidor
Fiquem una ip fixa

<img width="1183" height="384" alt="image" src="https://github.com/user-attachments/assets/c2cdd129-2c47-4fac-8123-533fb9fd9d17" />

ping 8.8.8.8
<img width="557" height="111" alt="image" src="https://github.com/user-attachments/assets/6aa5a4e1-84d2-4203-85a9-41d2ef2d8acf" />

fem un nano /etc/hostname i canviem el seguent
<img width="386" height="112" alt="image" src="https://github.com/user-attachments/assets/b874bbca-03a6-4f6e-824b-edb414e8b239" />

fem un nano /etc/hosts i canviem el seguent
<img width="607" height="314" alt="image" src="https://github.com/user-attachments/assets/7952f282-ed7c-4147-a135-b02de2497687" />
canviem el nom antic pel nou i afegim la seguent linia
10.0.2.15 pauserver.proves.cat pauserver


instalem slapd i ldap-utils
<img width="820" height="210" alt="image" src="https://github.com/user-attachments/assets/bbbd53f0-82c0-4ec8-b88f-83611ea28b82" />

assignem una contrasenya d'administrador
<img width="826" height="465" alt="image" src="https://github.com/user-attachments/assets/b5c36b6b-23a7-4622-8927-7687d5fe0546" />

password pauserver


slapcat:
<img width="546" height="315" alt="image" src="https://github.com/user-attachments/assets/d9985df4-c46f-4bd7-a632-13eba8c147dc" />


descomprimim la carpeta darcius
<img width="599" height="47" alt="image" src="https://github.com/user-attachments/assets/22df3a28-6c65-44d8-9852-df5e91ee2ce4" />

a continuacio fem un dpkg-reconfigure slapd i seguim el seguent procés:

<img width="822" height="526" alt="image" src="https://github.com/user-attachments/assets/a90a797d-c508-469d-9eff-19915c4ae2df" />

<img width="828" height="535" alt="image" src="https://github.com/user-attachments/assets/94bf9cc4-eaf3-4070-8c64-9023f630de8c" />

<img width="844" height="580" alt="image" src="https://github.com/user-attachments/assets/177e13fa-18d4-408a-8082-63db98446f76" />


<img width="674" height="442" alt="image" src="https://github.com/user-attachments/assets/b6f22157-bf4a-4e52-b09f-c2179ad229fe" />



<img width="828" height="568" alt="image" src="https://github.com/user-attachments/assets/e98b7895-7c24-4935-9b9e-41ba0e6b2e3a" />

<img width="822" height="582" alt="image" src="https://github.com/user-attachments/assets/103bc5bb-c79e-46b2-9739-4f781484d8b5" />


i tal com es pot veure a la captura s'ha fet correctament
<img width="894" height="450" alt="image" src="https://github.com/user-attachments/assets/d8b79c53-1d2b-4bc7-9f72-1d1b5fdf685b" />


modifiquem uo.ldif
<img width="513" height="189" alt="image" src="https://github.com/user-attachments/assets/0558d58b-fe33-4e67-9f8a-41c50f78cb5d" />

modifiquem grup.ldif
<img width="551" height="229" alt="image" src="https://github.com/user-attachments/assets/22c5286e-9151-4a99-b79a-8f9077717b42" />

modifiquem usu.ldif
<img width="551" height="565" alt="image" src="https://github.com/user-attachments/assets/e3f11e88-2047-4ea5-a975-ed98754ec36e" />


una vegada modificats fem 
ldapadd -c -x -D "cn=admin,dc=proves,dc=cat" -W -f uo.ldif
ldapadd -c -x -D "cn=admin,dc=proves,dc=cat" -W -f usu.ldif
ldapadd -c -x -D "cn=admin,dc=proves,dc=cat" -W -f grup.ldif

despres d'executar cadascuna de les 3 comandes ens hauria de dir adding new entry tal com es veu a la seguent captura:

<img width="551" height="565" alt="image" src="https://github.com/user-attachments/assets/f3b7a58c-7034-46d1-849c-fba154dd4d43" />


ARA OBRIM EL CLIENT

ping 10.0.2.15
<img width="653" height="203" alt="image" src="https://github.com/user-attachments/assets/1bc43fe5-e620-4813-b45f-fd7e8d1854d5" />


sudo apt update

<img width="875" height="40" alt="image" src="https://github.com/user-attachments/assets/98f18aa5-ddad-4538-a7a7-7f55e0308f55" />

<img width="1012" height="532" alt="image" src="https://github.com/user-attachments/assets/a6bae4ee-fb29-4908-9eca-dc478bd81816" />

<img width="773" height="439" alt="image" src="https://github.com/user-attachments/assets/02d76fca-3b10-4083-9abf-f814ff7c24b0" />



<img width="1001" height="481" alt="image" src="https://github.com/user-attachments/assets/d17103c8-077e-4b7a-94ab-ae9a5c83a331" />

<img width="1003" height="541" alt="image" src="https://github.com/user-attachments/assets/8345d61c-12fb-4b03-ae5c-c4e512c0e3ad" />

<img width="1013" height="553" alt="image" src="https://github.com/user-attachments/assets/7b84518e-987b-4a3c-af4d-f5dc7d1fa23b" />

<img width="1007" height="512" alt="image" src="https://github.com/user-attachments/assets/13a2bba2-0705-4fab-882e-7ac77f5a0123" />

<img width="1010" height="562" alt="image" src="https://github.com/user-attachments/assets/7468c7f9-ce76-4d3b-87b1-7a69e0994739" />

si ens em equivocat en algo fem:
<img width="775" height="38" alt="image" src="https://github.com/user-attachments/assets/64aa3d3d-c49e-45d5-abfd-d37f212d4ec3" />



<img width="923" height="532" alt="image" src="https://github.com/user-attachments/assets/aa4b2b7f-78c1-4799-a178-4a6c4ea13358" />

<img width="1013" height="563" alt="image" src="https://github.com/user-attachments/assets/cee031a3-d9ef-46f1-b01c-c06486626fe8" />
<img width="1007" height="523" alt="image" src="https://github.com/user-attachments/assets/29d6529e-ae9a-4b5a-8256-8a4cca21825f" />

per a encriptar
<img width="1016" height="583" alt="image" src="https://github.com/user-attachments/assets/ac33522e-2402-4053-a9d9-93007f722e26" />


fem un nano
<img width="991" height="576" alt="image" src="https://github.com/user-attachments/assets/6ef33eba-ba77-453b-bad1-d250cc147f48" />

nano i afegim la linia al finakl:
<img width="815" height="513" alt="image" src="https://github.com/user-attachments/assets/0ba1de84-ee5f-4820-b2d7-3438cc68529d" />

borrem use_authok de les 3 lines:

<img width="1013" height="595" alt="image" src="https://github.com/user-attachments/assets/981bfcee-3b45-44c1-988d-b605882af0ef" />

afegim la linia:


<img width="796" height="217" alt="image" src="https://github.com/user-attachments/assets/f073779d-4ecb-4fc9-8737-2de9a608e2d6" />

getent passwd | grep alu1
ens retorna ... per tant s'ha creat correctament
<img width="683" height="45" alt="image" src="https://github.com/user-attachments/assets/6be6539a-e6fc-48d3-af11-69008c4befaf" />


fem un reboot


mos logejem en alu1 (passwd: alu1)
whoami
cd ..
ls
<img width="573" height="253" alt="image" src="https://github.com/user-attachments/assets/854749a4-b03d-49e9-b6ef-28522934d970" />





## TASCA 2: INSTALACIÓ DE L'ENTORN GRÀFIC LDAP ACCOUNT MANAGER

Per facilitar la gestió del servidor LDAP en l'entorn Ubuntu 24.04, he analitzat diferents alternatives per a la interfície d'usuari. Finalment, he optat per la instal·lació de **LDAP Account Manager (LAM)** per la seva lleugeresa i facilitat d'integració web.

### Comparativa amb altres eines gràfiques

| Eina | Tipus | Dificultat | Motiu de l'elecció / descart |
| :--- | :--- | :--- | :--- |
| **LDAP Account Manager (LAM)** | Web (PHP) | **Molt Baixa** | **Seleccionat.** Instal·lació directa via `apt`, interfície moderna i gestió d'usuaris intuïtiva. |
| **phpLDAPadmin** | Web (PHP) | Mitja | Descartat per problemes de compatibilitat de codi amb les versions més recents de PHP a Ubuntu 24.04. |
| **Apache Directory Studio** | Escriptori (Java) | Mitja | Descartat per requerir instal·lació de JRE i ser una eina massa pesada per a una gestió ràpida. |
| **JXplorer** | Escriptori (Java) | Mitja | Descartat per tenir una interfície desactualitzada i menys funcional que LAM. |

### Procés d'instal·lació i configuració

1. **Instal·lació del paquet:**
   ```
   sudo apt update
   sudo apt install ldap-account-manager -y
<img width="869" height="144" alt="image" src="https://github.com/user-attachments/assets/f765a3cc-4c35-482e-96a7-0d598287d490" />

2. **Accés a la interfície:**
    L'eina està disponible a l'adreça: http://localhost/lam
<img width="478" height="334" alt="image" src="https://github.com/user-attachments/assets/8677641a-d839-4b37-ad3f-39f93cc8d72c" />

3. **Configuració del perfil:**

  Accedim a LAM Configuration > Edit server profiles

  <img width="474" height="669" alt="image" src="https://github.com/user-attachments/assets/2ed477ef-0ff5-477d-854c-b2a4a77bf9cd" />

  Des d'aquest menu podem configurar el Tree suffix (ex: dc=my-domain,dc=com) i l'Admin address (ex: cn=manager,dc=my-domain,dc=com).

Això ens permet gestionar Unitats Organitzatives, usuaris i grups de forma visual sense dependre de fitxers .ldif manuals.



26/01/26

##Samba
El Servidor Samba serveix per a compartir fitxers en xarxa entre diferents sistemes operatius.

La diferència principal respecte a altres sistemes de compartició és que Samba utilitza autenticació, ja sigui:

Mitjançant usuaris de Samba o a través de LDAP

Permet l’accés tant des de màquines Windows com Ubuntu/Linux.

🧪 Pràctica
🔌 Inici

Engeguem el servidor i el client.

⚙️ Configuració del servidor Samba
1️⃣ Actualitzar el sistema

Obrim un terminal al servidor i accedim com a superusuari:

sudo su
apt update

2️⃣ Instal·lació de Samba
apt install samba

<img width="815" height="392" alt="image" src="https://github.com/user-attachments/assets/e881aca7-b799-4e56-a445-671bfdcddf63" />

⚠️ Nota: Comprovar que la instal·lació finalitza correctament.

3️⃣ Creació d’usuaris Samba

Afegim tres usuaris (exemple: blau, groc, roig) amb la següent comanda:

useradd -M -s /sbin/nologin blau

<img width="737" height="423" alt="image" src="https://github.com/user-attachments/assets/4a012fea-2c4b-4bdc-ae3a-41b9b24b6c38" />

📌 Nota:
Aquests usuaris no són vàlids per al sistema operatiu, només s’utilitzen per a l’accés al servidor Samba.

4️⃣ Assignar contrasenya Samba

Per a cada usuari, executem:

smbpasswd -a nom_usuari

<img width="402" height="299" alt="image" src="https://github.com/user-attachments/assets/a35af88f-f03a-46e4-beb0-a2f2fd2b8bf7" />

5️⃣ Configuració del fitxer smb.conf

Editem el fitxer de configuració:

nano /etc/samba/smb.conf


Afegim la configuració al final del fitxer:

<img width="807" height="563" alt="image" src="https://github.com/user-attachments/assets/e565e86d-e5e7-4a1f-ab2b-5ad3a45c6862" />

6️⃣ Reiniciar serveis Samba
systemctl restart smbd nmbd

<img width="898" height="706" alt="image" src="https://github.com/user-attachments/assets/43111fe5-c580-4c73-b866-0371475ad0b0" />

💻 Configuració del client
1️⃣ Instal·lar eines necessàries
sudo apt update
sudo apt install smbclient

<img width="894" height="371" alt="image" src="https://github.com/user-attachments/assets/6c8df766-600a-4653-804f-77c5fd47cba6" />

2️⃣ Comprovació de connectivitat

Fem un ping al servidor per assegurar-nos que hi ha connexió:

<img width="899" height="499" alt="image" src="https://github.com/user-attachments/assets/99c9f2bf-3e31-4a3f-be8b-b8713523ac4a" />

📁 Accés al recurs compartit

Anem a Fitxers → Altres ubicacions

Introduïm la ruta del servidor:

smb://10.0.2.15/proves/


El sistema ens demanarà si volem connectar-nos:

Com a anònim

O amb un usuari Samba

<img width="578" height="577" alt="image" src="https://github.com/user-attachments/assets/51214508-86b4-4dbb-a312-c6a1ae4005d3" />

🔐 Resultats segons l’usuari
❌ Usuari Roig

No es pot connectar perquè està definit com a invalid user.

👁️ Usuari Groc

Pot llegir els fitxers

❌ No pot escriure, ja que no té permisos d’escriptura

<img width="537" height="170" alt="image" src="https://github.com/user-attachments/assets/97a6fe46-e1c2-42a3-aa34-79593b372a40" />

✅ Usuari Blau

Pot llegir i escriure

S’ha creat una carpeta amb el seu nom per comprovar-ho

<img width="629" height="283" alt="image" src="https://github.com/user-attachments/assets/55d9c13d-25eb-4879-9ff0-a72d207d5c82" />

si volem que un anònim tingui permisos de lectura i escriptura fem els seguents canvis a l'arxiu smb.conf
<img width="415" height="255" alt="image" src="https://github.com/user-attachments/assets/65fe6b71-b847-4c0c-8d26-899219fd6648" />
i fem un systemctl restart smbd nmbd per aplicar els canvis



## TASCA 3: Autenticació Samba amb usuaris LDAP

Una vegada tenim el servidor LDAP operatiu i Samba funcionant, configurem Samba perquè utilitzi el directori LDAP com a font d'usuaris (backend). Això permetrà que l'usuari **alu1** (que existeix al LDAP però no al sistema local com a usuari Samba) pugui accedir als recursos.

### 1. Instal·lació de paquets de connexió

Necessitem les llibreries que permeten a Samba parlar amb el servidor LDAP. Al servidor:

```
sudo apt install libnss-ldap libpam-ldap smbldap-tools -y

```
<img width="816" height="85" alt="image" src="https://github.com/user-attachments/assets/b6689b43-22d7-4947-8ec5-52dd94fa4f2a" />

### 2. Configuració del backend a `smb.conf`

Editem l'arxiu de configuració:

```
nano /etc/samba/smb.conf

```
<img width="596" height="28" alt="image" src="https://github.com/user-attachments/assets/e0c01a41-ce35-4699-812e-9cc617463179" />

Dins del bloc **`[global]`**, afegim (o modifiquem) les següents línies per indicar a Samba on és el servidor LDAP i com connectar-s'hi:

```
   # Configuració Backend LDAP
   passdb backend = ldapsam:ldap://localhost
   ldap suffix = dc=proves,dc=cat
   ldap user suffix = ou=usuaris
   ldap group suffix = ou=grups
   # DN de l'administrador que has configurat abans
   ldap admin dn = cn=admin,dc=proves,dc=cat
   ldap ssl = no

```
<img width="928" height="355" alt="image" src="https://github.com/user-attachments/assets/3e09c58a-c632-422f-aae9-045d5647c1fd" />

*Nota: Assegura't que `ou=usuaris` i `ou=grups` coincideixen amb les Unitats Organitzatives que has creat al teu fitxer `uo.ldif`.*

### 3. Emmagatzemar la contrasenya de l'admin LDAP

Perquè Samba pugui fer consultes, necessita la contrasenya de l'usuari `cn=admin`. L'hem de guardar de forma segura:

```
sudo smbpasswd -w pauserver

```

*(Si la comanda funciona correctament, hauràs de veure un missatge indicant que s'ha guardat el secret).*

### 4. Crear el recurs compartit per a `alu1`

Al final del fitxer `/etc/samba/smb.conf`, afegim una carpeta específica on només l'usuari del LDAP tingui accés d'escriptura:

```
[recurs_alu1]
   comment = Carpeta privada per a alu1 (LDAP)
   path = /srv/samba/alu1
   browseable = yes
   read only = no
   guest ok = no
   valid users = alu1
   writable = yes
   create mask = 0770
   directory mask = 0770

```
<img width="829" height="730" alt="image" src="https://github.com/user-attachments/assets/ee182a04-2d4d-48ec-9e28-c18d4c81307e" />

### 5. Aplicar canvis

Creem la carpeta física, assignem permisos i reiniciem el servei:

```
# Creem la carpeta
mkdir -p /srv/samba/alu1

# Donem permisos oberts a nivell de sistema (Samba filtrarà l'accés)
chmod 777 /srv/samba/alu1

# Reiniciem Samba per carregar la nova configuració LDAP
systemctl restart smbd nmbd

```
<img width="687" height="86" alt="image" src="https://github.com/user-attachments/assets/4ad9d7ad-4b1b-4191-83c6-5b6c7a53e24a" />


### 6. Comprovació des del Client

Des del client (on ja hem comprovat que tenim connectivitat):

1. Obrim el navegador de fitxers.
2. Anem a "Altres ubicacions" -> `smb://10.0.2.15/recurs_alu1`.
3. Ens demanarà autenticació.
4. Introduïm:
* **Usuari:** `alu1`
* **Contrasenya:** (la contrasenya que hagis posat al `usu.ldif` per a alu1).
5. Si tot és correcte, podrem entrar i crear carpetes.

---
09/02/26

## CONFIGURACIÓ DEL SERVIDOR NFS

### 1. Teoria: NFS vs Samba

**NFS (Network File System)** és un protocol que permet compartir fitxers i carpetes en una xarxa local, funcionant de manera nativa en sistemes Unix/Linux.

Principals diferències amb Samba:

* **Autenticació:** NFS realitza l'autenticació principalment a nivell de *host* (IP de la màquina client), mentre que Samba ho fa a nivell d'usuari/contrasenya.
* **Recursos:** NFS està dissenyat per compartir fitxers, però **no permet compartir impressores** (a diferència de Samba).
* **Compatibilitat:** Tot i ser natiu de Linux, els clients Windows (versions Pro/Enterprise) poden accedir-hi si s'activen les característiques opcionals.

### Exercici 1. Configuració de NFS sense LDAP

#### A. Configuració del Servidor

1. **Instal·lació del servei**
Entrem com a superusuari (root) i instal·lem el paquet necessari:
```
sudo su
apt update
apt install nfs-kernel-server

```


<img width="698" height="50" alt="image" src="https://github.com/user-attachments/assets/82a40224-56cd-45bf-a3c2-076785220786" />

2. **Creació de la carpeta compartida**
Creem una carpeta a l'arrel anomenada `1exercici`, li donem permisos totals i canviem el propietari a `nobody:nogroup` (usuari genèric per a NFS):

```
cd /
mkdir 1exercici
chmod 777 1exercici
chown nobody:nogroup 1exercici

```


Creem un fitxer de prova a dins:
```
touch /1exercici/hola
ls -l | grep 1exercici

```


<img width="700" height="197" alt="image" src="https://github.com/user-attachments/assets/32c1e94d-82e3-4e9a-b94b-32ba902f1272" />

3. **Exportar la carpeta**
Editem el fitxer de configuració `/etc/exports` per definir qui pot accedir i com:
```
nano /etc/exports

```


Afegim la següent línia al final del fitxer:
```
/1exercici *(rw,sync,no_subtree_check)

```
<img width="932" height="380" alt="image" src="https://github.com/user-attachments/assets/b1f375f1-6fcf-4c67-9e99-3a0cb6e16db9" />


> **Explicació dels paràmetres:**


> * `*`: Permet l'accés a qualsevol IP de la xarxa (es podria restringir per IP o rang).
> * `rw`: Permisos de lectura i escriptura.
> * `sync`: Confirma l'escriptura al disc abans de respondre (més segur).
> * `no_subtree_check`: Millora el rendiment evitant comprovacions de subarbres.
> 
> 


4. **Reiniciar el servei**
Apliquem els canvis i verifiquem l'estat:
```
systemctl restart nfs-kernel-server
systemctl status nfs-kernel-server

```
<img width="977" height="310" alt="image" src="https://github.com/user-attachments/assets/fbcad89f-9ff5-446e-b9bb-e50282c1c99f" />



#### B. Configuració del Client (Linux)

1. **Instal·lació de dependències**
Al client, entrem com a root i instal·lem les eines necessàries:
```
sudo su
apt update
apt install nfs-common rpcbind

```

<img width="768" height="95" alt="image" src="https://github.com/user-attachments/assets/0741877c-be0c-4426-9f19-35c9e8e1660d" />

2. **Preparació del punt de muntatge**
Creem la carpeta local on muntarem el recurs remot i li donem permisos:
```
mkdir -p /mnt/nfs_client
chmod 777 /mnt/nfs_client

```


*(Inserir captura de la creació de la carpeta)*
3. **Comprovació de connectivitat**
Fem un ping a la IP del servidor (10.0.2.15) per assegurar que tenim xarxa.
*(Inserir captura del ping)*
4. **Muntatge automàtic amb `/etc/fstab**`
Perquè la carpeta es monti automàticament en arrencar el sistema, editem el fitxer `/etc/fstab`:
```
nano /etc/fstab

```


Afegim la següent línia al final (substituint la IP per la del teu servidor):
```
10.0.2.15:/1exercici /mnt/nfs_client nfs auto,noatime,nolock,bg,nfsvers=3,intr,tcp,actimeo=1800 0 0

```


5. **Verificació**
Reiniciem el client (`reboot`) o muntem manualment amb `mount -a`.
Comprovem que podem veure el fitxer creat al servidor:
```
ls -l /mnt/nfs_client

```


Hauríem de veure el fitxer `hola`.
*(Inserir captura del resultat del ls)*

#### C. Configuració del Client (Windows)

Per accedir des de Windows, cal activar el client NFS:

1. Anar a **"Activar o desactivar les característiques de Windows"**.
2. Buscar i marcar **"Services for NFS"** (Serveis per a NFS).
3. Des del CMD o connectar unitat de xarxa, muntar el recurs.
*(Inserir captura de l'activació de la característica a Windows)*

---

Molt ben pensat. Farem una documentació molt més professional, tècnica i detallada que la del teu company. Explicarem exactament quines comandes cal posar i on, perquè quedi un manual perfecte per al teu `SP3.md`.

Ho dividirem en dues parts per no perdre'ns: primer prepararem el recurs al servidor i després el connectarem des del Windows.

Aquí tens el pas a pas exacte. Avisa'm quan vagis completant les captures!

---

### Exercici 2. Configuració de NFS amb Windows


**1. Creació de la carpeta específica per a Windows i assignació de permisos**
Anem a crear una carpeta nova per diferenciar-la de la resta, li donarem els permisos i crearem un fitxer de prova a dins.
Executa al terminal del servidor:

```bash
mkdir /nfs_windows
chmod 777 /nfs_windows
chown nobody:nogroup /nfs_windows
touch /nfs_windows/prova_windows.txt
ls -l / | grep nfs_windows

```

📸 **CAPTURA 1:** Fes una captura on es vegi el resultat de l'`ls -l`. S'ha de veure la carpeta `nfs_windows` amb els permisos `drwxrwxrwx` i el propietari `nobody nogroup`.

**2. Afegir la carpeta a l'arxiu d'exportacions**
Hem de dir-li al servidor NFS que comparteixi aquesta nova carpeta.

```bash
nano /etc/exports

```

Afegeix aquesta línia al final del document:

```text
/nfs_windows *(rw,sync,no_subtree_check)

```

📸 **CAPTURA 2:** Fes una captura de l'editor `nano` amb la línia acabada d'afegir. Després guarda i surt (`Ctrl+O`, `Enter`, `Ctrl+X`).

**3. Reiniciar el servei perquè s'apliquin els canvis**

```
systemctl restart nfs-kernel-server
systemctl status nfs-kernel-server
```

💻 **MÀQUINA A UTILITZAR ARA:** CLIENT (Windows)

**4. Activar el client NFS a Windows**
Per defecte, Windows no entén el protocol NFS. Ho hem d'activar a les característiques del sistema.

* Obre el menú d'inici i busca: **"Activar o desactivar las características de Windows"** (o "Turn Windows features on or off" si està en anglès).
* S'obrirà una finestra amb una llista. Busca la carpeta anomenada **"Servicios para NFS"** (Services for NFS).
* Desplega-la i marca la casella **"Cliente para NFS"** (Client for NFS).
* Fes clic a Acceptar i espera que s'instal·li.
📸 **CAPTURA 3:** Fes una captura just abans de donar-li a "Acceptar", on es vegi clarament la finestra amb la casella "Cliente para NFS" marcada.

**5. Muntar la unitat de xarxa des del CMD**
Ara muntarem la carpeta del servidor Linux com si fos un disc dur més del nostre Windows (li assignarem la lletra `Z:`).

* Obre el símbol del sistema (**CMD**).
* Executa la següent comanda (⚠️ **Important:** Canvia `10.0.2.15` per la IP real del teu servidor Linux):

```cmd
mount \\10.0.2.15\nfs_windows Z:

```

📸 **CAPTURA 4:** Fes una captura de la finestra del CMD on es vegi la comanda executada i el missatge de confirmació que diu que s'ha connectat correctament (normalment diu "Z: está conectado correctamente a...").

**6. Comprovació a l'Explorador de Fitxers**

* Obre l'explorador de fitxers de Windows ("Este equipo" o "Mi PC").
* Veuràs que tens una nova unitat de xarxa amb la lletra `Z:`.
* Entra a dins i comprova que hi ha el fitxer `prova_windows.txt` que havíem creat al servidor.
📸 **CAPTURA 5:** Fes una captura de l'explorador de fitxers de Windows on es vegi que estàs dins del disc `Z:` i es veu el fitxer de text creat al servidor Linux.

---

Amb això tindràs una documentació molt superior: ben estructurada, amb separació clara entre servidor i client, i justificant què fa cada comanda.

Vols que revisem el Bloc 2 (Client Linux) un cop tinguis aquestes captures, o hi ha alguna part d'aquest procés amb Windows que et doni error?
