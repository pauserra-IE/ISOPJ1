---
layout: default
title: "Sprint 3: Administració de Dominis i Seguretat"
---

#INSTAL·LACIÓ DOMINI LDAP I UNIR CLIENT AL DOMINI



DOMINIS: 
usuaris
grups
recursos (carpetes, impresores)
U.O (unitats organitzatives)---> (GPO, etc.)


12/01/26
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
<img width="546" height="315" alt="image" src="https://github.com/user-attachments/assets/2ecb7e72-4e87-421e-91a0-6525bcc63bd9" />

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
hauria de sortir una linia


fem un reboot


mos logejem en alu1 (passwd: alu1)

whoami
cd ..
ls



