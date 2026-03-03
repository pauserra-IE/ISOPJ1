---
layout: default
title: "Sprint 5: Monitoratge, Auditories i Programari Client/Servidor"
---

# Sprint 5: Monitoratge, Auditories i Programari Client/Servidor

Aquest document recull els conceptes clau sobre la gestió de logs a Linux, la configuració del servei `rsyslog`, la rotació de fitxers i el monitoratge del sistema.

## 1. Conceptes Fonamentals de Logging

Per gestionar els logs, Linux utilitza dos conceptes principals per classificar la informació:

* **Facility:** Defineix l'origen o el tipus de programa que genera el missatge (ex: `auth`, `cron`, `kern`, `mail`). L'asterisc (`*`) s'utilitza per indicar totes les fonts.
* **Priority (Nivell):** Defineix la gravetat del missatge (ex: `debug`, `info`, `notice`, `warning`, `err`, `crit`, `alert`, `emerg`).
* Si posem un punt (`.`), estem indicant aquest nivell i tots els superiors.
* Si posem un igual (`.=`), indiquem **només** aquell nivell específic.



### L'eina `logger`

La comanda `logger` permet afegir entrades al log del sistema manualment des de la terminal.

> **Exemple:** `logger -i -s -p mail.err "Missatge d'error"`
> * `-i`: Afegeix el PID del procés.
> * `-s`: Mostra el missatge també per la sortida d'error estàndard.
> * `-p`: Especifica la *facility* i la *prioritat*.
> 
> 

---

## 2. Directoris i Fitxers Importants

La majoria dels logs del sistema s'emmagatzemen a `/var/log`. Tot i que molts serveis escriuen al fitxer general `syslog`, alguns paquets instal·lats creen les seves pròpies carpetes per gestionar les seves dades de manera independent.

<img width="1181" height="235" alt="image" src="[https://github.com/user-attachments/assets/a4107def-0e00-4b8b-9cd2-d9e2b57926f1](https://github.com/user-attachments/assets/a4107def-0e00-4b8b-9cd2-d9e2b57926f1)" />

Si visualitzem el fitxer `syslog`, podem veure l'activitat general del sistema en temps real:

<img width="1208" height="709" alt="image" src="[https://github.com/user-attachments/assets/7b87c153-76c5-4a76-8bdd-89f41addc4ca](https://github.com/user-attachments/assets/7b87c153-76c5-4a76-8bdd-89f41addc4ca)" />

---

## 3. Rotació de Logs (`logrotate`)

Per evitar que els fitxers de log omplin tot el disc dur, el sistema utilitza **logrotate**. Aquesta eina:

1. Comprimeix els logs antics (sovint en format `.gz`).
2. Els reanomena (ex: `syslog.1`, `syslog.2.gz`).
3. Elimina els fitxers més vells segons una configuració de dies o mida.

La configuració es troba a `/etc/logrotate.d/`.

<img width="1012" height="109" alt="image" src="[https://github.com/user-attachments/assets/8470044a-eda1-4ee9-973c-e55b6c4faed9](https://github.com/user-attachments/assets/8470044a-eda1-4ee9-973c-e55b6c4faed9)" />

Podem veure la configuració específica de `rsyslog` per entendre com gestiona els seus propis fitxers:

<img width="987" height="536" alt="image" src="[https://github.com/user-attachments/assets/059936de-5357-460b-878b-4a96f5aabba8](https://github.com/user-attachments/assets/059936de-5357-460b-878b-4a96f5aabba8)" />

---

## 4. Configuració de `rsyslog`

Antigament, tota la configuració es trobava a `/etc/rsyslog.conf`. Actualment, a les distribucions basades en Ubuntu/Debian, la configuració de les regles per defecte es troba a:
`/etc/rsyslog.d/50-default.conf`

<img width="1233" height="683" alt="image" src="[https://github.com/user-attachments/assets/cc484c0c-752c-4122-8982-aad728ef4138](https://github.com/user-attachments/assets/cc484c0c-752c-4122-8982-aad728ef4138)" />

### Proves de funcionament

Per veure els canvis en directe mentre fem proves, executem en una terminal:
`tail -f /var/log/syslog`

#### Prova 1: Kern.notice

Executem: `logger -i -s -p kern.notice "Prova pau"`
El resultat a la terminal de monitoratge serà:

<img width="807" height="460" alt="image" src="[https://github.com/user-attachments/assets/ff3595b7-112e-4d89-9393-5df407d9a5c4](https://github.com/user-attachments/assets/ff3595b7-112e-4d89-9393-5df407d9a5c4)" />

#### Prova 2: Mail.notice

Podem provar amb diferents serveis com el de correu:

<img width="856" height="457" alt="image" src="[https://github.com/user-attachments/assets/eff3f65b-138f-417e-a861-e4a99fad8712](https://github.com/user-attachments/assets/eff3f65b-138f-417e-a861-e4a99fad8712)" />
<img width="917" height="333" alt="image" src="[https://github.com/user-attachments/assets/5764edfe-4c00-490d-92f8-d2e4d4d261c4](https://github.com/user-attachments/assets/5764edfe-4c00-490d-92f8-d2e4d4d261c4)" />

#### Prova 3 i 4: Filtres de nivell

Si modifiquem el fitxer de configuració per filtrar nivells específics (per exemple, usant o traient l'igual `=` a `mail.err`), el comportament del log canviarà, registrant només aquest nivell o tots els superiors.

<img width="784" height="179" alt="image" src="[https://github.com/user-attachments/assets/424b7b37-6b36-4fc0-9360-4ed966deaf40](https://github.com/user-attachments/assets/424b7b37-6b36-4fc0-9360-4ed966deaf40)" />
<img width="1231" height="409" alt="image" src="[https://github.com/user-attachments/assets/1cbf2e38-065e-4ca2-8207-5c42abeebff9](https://github.com/user-attachments/assets/1cbf2e38-065e-4ca2-8207-5c42abeebff9)" />

#### Prova 5: Fitxers de log personalitzats

Podem redirigir logs a fitxers propis afegint una línia al fitxer `50-default.conf`. Per exemple, per enviar totes les alertes crítiques a un fitxer nou:
`*crit -/var/log/pau.log`

<img width="808" height="342" alt="image" src="[https://github.com/user-attachments/assets/fe5fed93-f0e2-461a-8012-f54bd46821f8](https://github.com/user-attachments/assets/fe5fed93-f0e2-461a-8012-f54bd46821f8)" />

---

## 5. El sistema `journalctl`

A més dels fitxers de text tradicionals, els sistemes moderns amb `systemd` utilitzen un log binari gestionat per `journalctl`. Això permet fer cerques molt més ràpides i filtrades.

Per exemple, per veure només els logs de correu:
`journalctl --facility=mail`

<img width="646" height="69" alt="image" src="[https://github.com/user-attachments/assets/32039832-b9f5-4be2-a548-0a72b9e1c1c6](https://github.com/user-attachments/assets/32039832-b9f5-4be2-a548-0a72b9e1c1c6)" />

---

## TASQUES A REALITZAR

### TASCA 1: Monitor del Sistema (Rendiment)

Accedeix a l'entorn gràfic del **Monitor del Sistema** i realitza 3 captures de pantalla on es visualitzi la capacitat de monitoritzar:

1. **Processos:** Llista de programes en execució i consum.
2. **Recursos:** Gràfiques d'ús de CPU, Memòria i Xarxa.
3. **Sistemes de fitxers:** Ús de l'espai en disc de les unitats muntades.

### TASCA 2: Servidor de Logs Centralitzat

Configurarem un escenari on una màquina (Client) envia els seus logs a una altra màquina (Servidor/Professor).

* **Requisits:** 2 Màquines Virtuals Ubuntu.
* **Passos clau:**
1. Desactivar/Configurar firewalls per permetre el tràfic UDP/TCP (port 514).
2. Habilitar el mòdul de recepció al servidor `rsyslog`.
3. Al client, editar el fitxer `50-default.conf` per apuntar a la IP del servidor.


