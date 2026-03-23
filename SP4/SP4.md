---
layout: default
title: "Sprint 4: Configuració del Programari de Base i Sistemes d’Emmagatzematge en Ubuntu "
---




#Teoria RAID
Introduccio:
Els sistemes d'emmagatzematge redundant (RAID) son...
Hi ha x tipus


#Pràctica RAID 1:

Farem un RAID 1

Farem falla run disco per hardware i per software. 

En primer lloc Afegim dos discs iguals de 2gb
<img width="615" height="418" alt="image" src="https://github.com/user-attachments/assets/516df895-ed3a-4a0f-ac38-e8e92aa93fec" />

<img width="443" height="314" alt="image" src="https://github.com/user-attachments/assets/2a8bce43-409e-43a0-a440-a1ca993c0089" />


Iniciem la maquina 

Fem un apt update
<img width="738" height="136" alt="image" src="https://github.com/user-attachments/assets/28e1ed48-b1a5-4e62-bc0f-87588ed77306" />

Instalem mdadm amb apt install mdadm
<img width="732" height="97" alt="image" src="https://github.com/user-attachments/assets/390d1f69-b732-4e2c-b5cb-bf2754c50cc5" />

Ara identifiquem els dos discs que hem afegit amb la comanda fdisk -l

<img width="681" height="633" alt="image" src="https://github.com/user-attachments/assets/0b8258c4-02b0-4a95-aa9f-1082dbe0b484" />

preparem els dos discos:
fdisk /dev/sdb
introduim "n" (new)
tot per defecte (prement intro)
i w (per guardar els canvis ) 
<img width="790" height="578" alt="image" src="https://github.com/user-attachments/assets/eb9a6d2d-64ae-40ff-9953-b3ed60933ab4" />
i repteim el procés amb fdisk /dev/sdc

<img width="798" height="578" alt="image" src="https://github.com/user-attachments/assets/e0fb6b3e-4a9d-476b-b3ba-279170a037e4" />

ara preparem la carpeta del raid 
cd /mnt/
mkdir raid1
chmod 777 raid1/
ls -l 
<img width="535" height="143" alt="image" src="https://github.com/user-attachments/assets/dcfedb0c-a977-417e-9ef7-9d9ebe07997d" />


fem el raid amb la comanda
mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sdb1 /dev/sdc1
<img width="1032" height="195" alt="image" src="https://github.com/user-attachments/assets/6d8a87af-86c5-4f12-8994-d10637378b9b" />


formatem el disc 
mkfs.ext4 /dev/md0
<img width="920" height="261" alt="image" src="https://github.com/user-attachments/assets/2ec0a135-e783-4cbe-929d-d3e60e103dc2" />


per a mirar com estan els raids
mdadm --detail /dev/md0
<img width="801" height="595" alt="image" src="https://github.com/user-attachments/assets/be055875-7980-49bf-bc27-e193185f3bea" />


ara tenim que enviar la informacio que ens dona la comanda "mdadm --detail --scan"  a mdadm.conf:


ARRAY /dev/md0 metadata=1.2 UUID=08e06a6d:3a112259:318350e6:d1895d81

per aixo utilitzem la comanda "mdadm --detail --scan > /etc/mdadm/mdadm.conf"

<img width="793" height="66" alt="image" src="https://github.com/user-attachments/assets/023c7638-fe43-4e8b-a477-4e73a4c4440d" />


ara editem "nano /etc/mdadm/mdadm.conf" i afegim la linea
"DEVICE /dev/sdb1 /dev/sdc1" 
<img width="770" height="119" alt="image" src="https://github.com/user-attachments/assets/4a6de626-4963-4daf-bbc6-03d5f998e4a0" />

ara editem nano /etc/fstab i afegim la linea "/dev/md0        /mnt/raid1      ext4    defaults 0 0"

<img width="839" height="434" alt="image" src="https://github.com/user-attachments/assets/60cb0a32-176c-48b6-8195-65290897c580" />
