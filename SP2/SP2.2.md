---
layout: default
title: "Sprint 2: Instal·lació, Configuració de Programari de Base i Gestió de Fitxers"
---

# ÍNDEX
## Còpies de seguretat i automatització de tasques

### 1. Teoria Còpies de Seguretat
Que es una copia
3 tipus de copies de seguretat
### 2. Teoria Comandes Backups

### 3. Pràctica comandes Backups
  - cp
  - rsync
  - dd

### 4. Pràctica programes Backups
  - Deja-Dup
  - Duplicity

### 5. Teoria Automatització scripts.cron i anacron

### 6. Pràctica automatització
  - cron
  - anacron
------------------------------
DOCUMENTACIO

### 1. Teoria Còpies de Seguretat
Que es una copia

3 tipus de copies de seguretat, ventatjes i desventatges i que es necessita per recuperar cadascuna
Tipus: completa, diferencial, incremental

1-Completa
2-Diferencial: ventatjaocupa menys que una completa. la diferencial sempre es fa a partir de la completa. desventatja: per a recuperarla necessites la ultima completa 
3-Incremental: es necessita la ultima completa o diferencial i totes les incrementals anteriors. ventatja ocupen molt poc ja que nomes conte la diferencia...
fer una taula en 6 columnes: tipus, ques es, avantatges, desventatges i que es necessita per a recuperarles



cp: es una copìa simple que no es inteligent. transfereix arxius nomes localment. no optimitza ni temps ni espai
rsync: es una eina inteligent que nomes copia els fitxers modificats. treballa en local i en remot.
dd:No es una eina per a copiar (es una eina per a clonar) s'utlitza quan volem copiar tot el contingut d'un disc o una particio.


Pràctica:


captura fdisk -l
<img width="1203" height="769" alt="image" src="https://github.com/user-attachments/assets/d0af3951-2465-4402-bc89-5dfcd497d8b0" />

captura comprovacio despres de crear les particions:
<img width="781" height="562" alt="image" src="https://github.com/user-attachments/assets/5190a366-914a-4855-b589-d3d51bd48e7e" />



captura cp -R /home/pauserra/Documents/* /var/copies
<img width="953" height="221" alt="image" src="https://github.com/user-attachments/assets/fc9fe6b2-00f8-4716-b231-02a0beea17a5" />

captura rsync
<img width="918" height="482" alt="image" src="https://github.com/user-attachments/assets/0a76a433-ff91-40a8-bd2d-1479e9359646" />
