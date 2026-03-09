---
layout: default
title: "Sprint 5: Monitoratge, Auditories i Programari Client/Servidor"
---

FACILITY
*-> totes les aplicacions o tos els serveis
- ->missat nomes este nivell de servei = nivell acció

logger -i -s -p mail.err aturant el sistema


## Directoris i fitxers importants

En aquest directori estan tots els logs del sistema pero hi ha serveis que que instalem que posen logs a les seves propies carpetes.
<img width="1181" height="235" alt="image" src="https://github.com/user-attachments/assets/a4107def-0e00-4b8b-9cd2-d9e2b57926f1" />

si fem cat syslog podrem veure...
<img width="1208" height="709" alt="image" src="https://github.com/user-attachments/assets/7b87c153-76c5-4a76-8bdd-89f41addc4ca" />

ROTACIÓ DE LOGS: Consisteix en...
els antics els compremeix i els fica a un altre, el numero de dies es pot personalitzar...
per a fer-ho anem a /etc/logrotate.d/
<img width="1012" height="109" alt="image" src="https://github.com/user-attachments/assets/8470044a-eda1-4ee9-973c-e55b6c4faed9" />

per exemple si fem un cat de rsyslog veurem que...

<img width="987" height="536" alt="image" src="https://github.com/user-attachments/assets/059936de-5357-460b-878b-4a96f5aabba8" />

per a ... antigament anavem a 7etc/ryslog.conf pero tal com es veu a la captura ara es troba a /etc/rsyslog.d/50-defaul.conf
<img width="1233" height="683" alt="image" src="https://github.com/user-attachments/assets/cc484c0c-752c-4122-8982-aad728ef4138" />

Anem a /etc/ryslog.d/50-default.conf

<img width="1045" height="683" alt="image" src="https://github.com/user-attachments/assets/7920b47f-14d3-4934-97d3-7967775d70a6" />

Aqui realitzarem diverses proves
fem un tail -f /var/log/syslog 
aixi veurem els canvis que fessem.

ara provem la comanda logger -i -s -p kern.notice Prova pau

i a la l'altra finestra (terminal) on tenim ober la comanda tail -f /var/log/syslog, veurem...
<img width="807" height="460" alt="image" src="https://github.com/user-attachments/assets/ff3595b7-112e-4d89-9393-5df407d9a5c4" />


ara farem una altra prova amb mail.notice

<img width="856" height="457" alt="image" src="https://github.com/user-attachments/assets/eff3f65b-138f-417e-a861-e4a99fad8712" />

<img width="917" height="333" alt="image" src="https://github.com/user-attachments/assets/5764edfe-4c00-490d-92f8-d2e4d4d261c4" />

Prova 3: (IMPORTANT: ho he fet malament he tocat el mail.err en lloc de mail.log)

i ara veiem que prova 3 si que surt a la terminal de baix pero si fem un cat no surt
<img width="784" height="179" alt="image" src="https://github.com/user-attachments/assets/424b7b37-6b36-4fc0-9360-4ed966deaf40" />


Prova 4 llevem el = de mail.err 

<img width="1231" height="409" alt="image" src="https://github.com/user-attachments/assets/1cbf2e38-065e-4ca2-8207-5c42abeebff9" />
prova 6

Prova crit:
ara afegim "*crit     -/var/log/pau.log" a 50-default.conf
<img width="808" height="342" alt="image" src="https://github.com/user-attachments/assets/fe5fed93-f0e2-461a-8012-f54bd46821f8" />
i tot seguit...


Proves journalctl:

journalctl --facility=mail
<img width="646" height="69" alt="image" src="https://github.com/user-attachments/assets/32039832-b9f5-4be2-a548-0a72b9e1c1c6" />


## TASCA 1:Rendiment:
Entrar a monitor del sistema: Fer 3 captures que es veigue que es pot monitoritzar els processos, pestanya de recursos i sistemes de fitxers


## TASCA 2: 
Escenari imagineu que A traves de una imatge configurem que els logs dels alumnes vinguin al profe.
simularem aixo en dues maquines virtuals:
2 maquines ubuntu i una ha de rebre els logs de l'altra. hem de desactivar firewalls, instalar un pasquet i al 50-default.conf configurar la ip...


03/03/26
Pràctica Conjunta (Valle)






# Servidor d'Actualitzacions

**Data:** 09/03/26

Aquesta documentació explica com configurar un servidor local d'actualitzacions amb `apt-mirror` i Apache, i com configurar un client per obtenir paquets des d’aquest servidor.

---

## 1. Obrir el servidor

Accedim com a root:

```
sudo su
````

Actualitzem els repositoris:

```
apt update
```

Instal·lem Apache2:

```
apt install apache2
```

![Apache instal·lat](https://github.com/user-attachments/assets/c6d88018-875c-4891-b17b-3fed85095733)

Instal·lem `apt-mirror`:

```
apt install apt-mirror
```

![Apt-mirror instal·lat](https://github.com/user-attachments/assets/9c8c385f-f0db-4e80-bb24-4873e5de5858)

---

## 2. Configurar `apt-mirror`

Editem la configuració:

```
nano /etc/apt/mirror.list
```

* Afegim tots els repositoris que volem descarregar al servidor local.
* D’aquesta manera, els clients no hauran de descarregar els paquets d’internet.
* Per a la prova, comentem tots els altres repositoris i afegim només el de Google Chrome.

![Configuració mirror.list](https://github.com/user-attachments/assets/8dfb681b-44de-4dd7-8897-4c4fade3c069)

Executem la descàrrega dels paquets:

```
apt-mirror
```

![Execució apt-mirror](https://github.com/user-attachments/assets/4cb18ca3-68ee-49cf-93eb-6bfc22c24901)

---

## 3. Configurar Apache

Creem un **softlink** per servir els paquets amb Apache:

```
ln -s /var/spool/apt-mirror/mirror/dl.google.com/linux/chrome/deb /var/www/html/
```

![Softlink creat](https://github.com/user-attachments/assets/0f47648b-d9b5-45e8-a12d-0caf98b9c944)

Verifiquem que el softlink s’ha creat correctament:

![Verificació softlink](https://github.com/user-attachments/assets/a53ace71-5df1-42cd-b8e3-b7348bd4414a)

Comprovem la IP del servidor (en aquest exemple `10.0.2.9`):

![IP del servidor](https://github.com/user-attachments/assets/24cdcfce-a646-42c5-8754-3ff2e09418e3)

---

## 4. Configurar el client

Obrim el fitxer de repositoris del client:

```
nano /etc/apt/sources.list
```

![Sources.list client](https://github.com/user-attachments/assets/abe74f5b-1515-474a-af9f-a00c40a2acb9)

Com que Google Chrome necessita signatura, la importem:

![Signatura Chrome](https://github.com/user-attachments/assets/bd56d477-e673-4cf1-a3c2-771ea4cf20f9)

Fem un `apt update` per comprovar que el client obté els paquets del servidor local:

![Apt update client](https://github.com/user-attachments/assets/05e5dd6b-010a-44a7-ad4f-3a7fc0214330)

Instal·lem el paquet des del servidor:

```
apt install google-chrome-stable
```

![Instal·lació Chrome](https://github.com/user-attachments/assets/feb84d4a-a253-46c7-a5d9-23b3ba70c911)

---

## 5. Tasca Servidor d'Actualitzacions amb un altre paquet

Ara repetiré el procés fet a classe amb un altre paquet, per exemple **htop** (programa que serveix per monitoritzar processos del sistema).




