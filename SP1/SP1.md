---
layout: default
title: "Sprint 1: Instal·lació, Configuració Inicial i Programari de Base"
---

## Llicenciament
Ubuntu té aquestes llicencies:
- **Nucli Linux:** GPLv2, permet usar, modificar i redistribuir el kernel.  
- **Eines del sistema:** GPL/LGPL, inclou utilitats bàsiques com bash i coreutils.  
- **Biblioteques i paquets addicionals:** MIT, Apache 2.0, BSD; compatibles amb codi obert.  
- **Marca i logotip d’Ubuntu:** propietat de Canonical Ltd.; l'ús comercial requereix seguir les Ubuntu Trademark Guidelines.

---

## Virtualització i instal·lació del SO Ubuntu
En primer lloc obrim VirtualBox per a començar a configurar la maquina virtual.
<img width="877" height="472" alt="Captura de pantalla de 2025-09-30 13-32-49" src="https://github.com/user-attachments/assets/53540824-a77c-495a-aeec-34c4b0be8bc0" />

Sel·leccionem la iso que volem, en aquest cas fare la instal·lació de Ubuntu 24.04.1.  
Marquem la casella "Skip unattended installation" i fem clic a següent.

A continuació ajustarem els paràmetres de la màquina.
<img width="877" height="472" alt="Captura de pantalla de 2025-09-30 13-33-28" src="https://github.com/user-attachments/assets/9f2469ac-f5cc-49a5-afc5-bfe5937e2c1d" />
<img width="877" height="472" alt="Captura de pantalla de 2025-09-30 13-33-37" src="https://github.com/user-attachments/assets/1766d42e-5027-427e-884d-36d8db4a38d6" />

### Justificació de l’elecció
He escollit 8 GB de RAM i 6 nuclis de CPU perquè son recursos suficients per executar el sistema operatiu sense problemes.  
Pel que fa a l’emmagatzematge, he assignat 80 GB de disc: 40 GB per a Ubuntu i 40 GB reservats per a Windows, que instal·larem més endavant.

<img width="877" height="472" alt="Captura de pantalla de 2025-09-30 13-33-43" src="https://github.com/user-attachments/assets/583c71df-b9e5-4cfe-9da5-7977680ef400" />

- Una vegada ja hem configurat tots els parametres ja podem iniciar la maquina per començar la instal·lació.  
- Després de configurar la distribució del teclat, etc., quan arribem a l’apartat de tipus d’instal·lació, seleccionem “Alguna altra cosa” (per a fer una instal·lació manual) i així definirem les particions:

<img width="1280" height="883" alt="Captura de pantalla de 2025-10-01 23-36-14" src="https://github.com/user-attachments/assets/61aab1d1-ae62-4be9-b1b4-0b7fffb32c1e" />

---

### Justificació de l’elecció de particions
- He creat una partició /boot/efi d’1 GB per assegurar una arrencada correcta en mode UEFI i deixar espai suficient per a futures actualitzacions del gestor d’arrencada.  
- L'arrel / té 25 GB, suficients per allotjar el sistema operatiu, paquets i aplicacions addicionals sense problemes.  
- La partició /home ocupa la resta de l’espai disponible per emmagatzemar documents i configuracions personals.  
- Finalment, he afegit una partició swap de 4 GB. Tot i que no seria estrictament necessària, ja que el host disposa de 32 GB de RAM, pot resultar útil en casos puntuals.  
- Un cop creades totes les particions, seleccionem la / (arrel) com a destinació d’instal·lació del sistema i procedim amb la instal·lació d’Ubuntu.







## Gestors d'arrencada per a instal·lacions DUALS
## Punts de restauració
## Configuració de xarxa
## Comandes generals i instal·lacions

