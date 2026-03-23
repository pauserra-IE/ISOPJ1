---
layout: default
title: "Sprint 4: Configuració del Programari de Base i Sistemes d’Emmagatzematge en Ubuntu "
---




#Teoria RAID
Introduccio:
Els sistemes d'emmagatzematge redundant (RAID) son...
Hi ha x tipus


---

# 🧩 Pràctica RAID 1

En aquesta pràctica configurarem un **RAID 1 (mirroring)** utilitzant `mdadm`.

---

## 🔹 1. Afegir discs

Afegim **dos discs iguals de 2 GB** a la màquina virtual:

![Discs afegits](https://github.com/user-attachments/assets/516df895-ed3a-4a0f-ac38-e8e92aa93fec)

![Configuració discs](https://github.com/user-attachments/assets/2a8bce43-409e-43a0-a440-a1ca993c0089)

---

## 🔹 2. Preparació del sistema

Iniciem la màquina i actualitzem els repositoris:

```bash
apt update
```

Instal·lem `mdadm`:

```bash
apt install mdadm
```

---

## 🔹 3. Identificar els discs

Comprovem els discs disponibles:

```bash
fdisk -l
```

![Llistat discs](https://github.com/user-attachments/assets/0b8258c4-02b0-4a95-aa9f-1082dbe0b484)

---

## 🔹 4. Preparar els discs

Creem una partició a cada disc:

```bash
fdisk /dev/sdb
```

Passos dins `fdisk`:

* `n` → nova partició
* Intro → valors per defecte
* `w` → guardar canvis

Repetim el procés per `/dev/sdc`:

```bash
fdisk /dev/sdc
```

---

## 🔹 5. Crear punt de muntatge

```bash
cd /mnt/
mkdir raid1
chmod 777 raid1/
ls -l
```

![Carpeta RAID](https://github.com/user-attachments/assets/dcfedb0c-a977-417e-9ef7-9d9ebe07997d)

---

## 🔹 6. Crear el RAID 1

```bash
mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sdb1 /dev/sdc1
```

![Creació RAID](https://github.com/user-attachments/assets/6d8a87af-86c5-4f12-8994-d10637378b9b)

---

## 🔹 7. Formatar el RAID

```bash
mkfs.ext4 /dev/md0
```

![Format RAID](https://github.com/user-attachments/assets/2ec0a135-e783-4cbe-929d-d3e60e103dc2)

---

## 🔹 8. Comprovar estat del RAID

```bash
mdadm --detail /dev/md0
```

![Detall RAID](https://github.com/user-attachments/assets/be055875-7980-49bf-bc27-e193185f3bea)

---

## 🔹 9. Guardar configuració

Obtenim la configuració:

```bash
mdadm --detail --scan
```

Exemple de sortida:

```
ARRAY /dev/md0 metadata=1.2 UUID=08e06a6d:3a112259:318350e6:d1895d81
```

La guardem a l’arxiu de configuració:

```bash
mdadm --detail --scan > /etc/mdadm/mdadm.conf
```

![Config mdadm](https://github.com/user-attachments/assets/023c7638-fe43-4e8b-a477-4e73a4c4440d)

Afegim també els dispositius:

```bash
nano /etc/mdadm/mdadm.conf
```

Afegir línia:

```
DEVICE /dev/sdb1 /dev/sdc1
```

![Editar mdadm.conf](https://github.com/user-attachments/assets/4a6de626-4963-4daf-bbc6-03d5f998e4a0)

---

## 🔹 10. Configurar muntatge automàtic

Editem `/etc/fstab`:

```bash
nano /etc/fstab
```

Afegim:

```
/dev/md0    /mnt/raid1    ext4    defaults    0    0
```

![fstab](https://github.com/user-attachments/assets/60cb0a32-176c-48b6-8195-65290897c580)

---

## 🔹 11. Verificació

```bash
ls -la /mnt/raid1/
```

![Verificació](https://github.com/user-attachments/assets/d51c2764-f430-4e7f-a34a-6795da8d3327)

---

## ✅ Explicació 

Si tot ha funcionat correctament, veurem una línia com aquesta:

```
drwx------ 2 root root 16384 ... lost+found
```

### 🔍 Què significa?

* **`lost+found`**: carpeta creada automàticament en sistemes de fitxers `ext4`.
* Indica que el sistema de fitxers s’ha creat correctament.
* Confirma que el RAID `/dev/md0` està **muntat correctament**.
* Permisos `drwx------`: només l’usuari `root` té accés complet (com és habitual).

---
Aquí tens els apartats afegits i millorats, integrats amb el mateix estil clar i estructurat:

---

## ⚠️ Possibles problemes

Si després d’editar `mdadm.conf` i `/etc/fstab` el RAID **no es munta correctament després de reiniciar**, podem forçar l’actualització de la configuració d’arrencada amb:

```bash
update-initramfs -u -k all
```

### 🔍 Explicació

* **`update-initramfs`**: reconstrueix la imatge `initramfs`, necessària per arrencar el sistema.
* **`-u` (update)**: actualitza la imatge existent.
* **`-k all`**: aplica l’actualització a **tots els nuclis instal·lats**.

👉 Això assegura que el sistema detecti correctament el RAID durant l’arrencada.

---

## 🧪 Proves de funcionament

### 🔹 1. Crear fitxer de prova

Creem un fitxer dins del RAID:

![Fitxer de prova](https://github.com/user-attachments/assets/3c3d4c1a-8d62-4e57-a09a-d611b41523a2)

---

### 🔹 2. Simular fallada d’un disc

Simulem la fallada d’un dels discos i comprovem l’estat del RAID:

![Comprovació RAID](https://github.com/user-attachments/assets/1b9e30fc-803a-47b6-831e-faa6b4559b18)

👉 El RAID hauria de continuar funcionant (mode degradat), ja que és un RAID 1.

---

### 🔹 3. Eliminar i tornar a afegir el disc

Eliminem el disc fallat:

```bash
mdadm /dev/md0 -r /dev/sdb1
```

El tornem a afegir:

```bash
mdadm /dev/md0 -a /dev/sdb1
```

![Reinserció disc](https://github.com/user-attachments/assets/063a3685-42a3-423e-ba9e-d0eac9b62645)

---

### 🔹 4. Verificar reconstrucció

```bash
mdadm --detail /dev/md0
```

afegir captura Rebuild del raid

👉 El sistema començarà la **reconstrucció (rebuild)** automàticament.

---

## 🗑️ Com esborrar el RAID

### 🔹 1. Eliminar muntatge automàtic

Editem `/etc/fstab` i **eliminem o comentem** la línia del RAID:

![Eliminar fstab](https://github.com/user-attachments/assets/1e7f111d-4688-4c84-8002-e67f79981fc2)

---

### 🔹 2. Desmuntar el RAID

```bash
umount /dev/md0
```

👉 Desmunta el sistema de fitxers del RAID.

---

### 🔹 3. Aturar el RAID

```bash
mdadm --stop /dev/md0
```

👉 Atura el dispositiu RAID.

---

### 🔹 4. Eliminar configuració del RAID

```bash
mdadm --remove /dev/md0
```

👉 Pot donar error si ja està eliminat (és normal).

---

### 🔹 5. Eliminar punt de muntatge

```bash
rm -r /mnt/raid1/
```

👉 Esborrem la carpeta creada.

---

### 🔹 6. Netejar els discos

```bash
mdadm --zero-superblock /dev/sdb1 /dev/sdc1
```

👉 Elimina la informació RAID dels discos.

---

### 🔹 7. Verificació

```bash
mdadm --detail /dev/md0
```

👉 Ja no hauria d’existir.

---

### 🔹 8. Netejar configuració

Editem:

```bash
nano /etc/mdadm/mdadm.conf
```

I eliminem les línies del RAID:

![Neteja mdadm.conf](https://github.com/user-attachments/assets/5b0730c2-a575-4dcd-b886-73494c05c61a)

---

### 🔹 9. Reiniciar el sistema

Després de reiniciar:

![Sense RAID](https://github.com/user-attachments/assets/1fe97c15-3e9f-48c0-9c6a-73e8bc97e5b5)

👉 El RAID ja no existeix al sistema.

---

## ✅ Resultat final

* RAID eliminat correctament
* Discos nets i reutilitzables
* Sistema sense configuració residual

---

