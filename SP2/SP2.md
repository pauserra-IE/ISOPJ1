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
## Gestió d’usuaris i grups i permisos
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



