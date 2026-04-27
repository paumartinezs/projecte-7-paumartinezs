# T03: Servidor de fitxers

IMPORTANT

Abans de crear la màquina virtual, afegim de primera interfície “NAT” i de segona  
“Adaptador pont”.

En primer lloc, instal·lem “l’Active Directory Domain Services” i creem el nou domini.

![imatge1](../pics/imatge1.png)

Li afegim contrasenya del “DSRM”.

![imatge2](../pics/imatge2.png)

I acabem d’instal·lar el domini, seguint els passos que vagi demanant.

![imatge3](../pics/imatge3.png)

![imatge4](../pics/imatge4.png)

![imatge5](../pics/imatge5.png)

Un cop creat el domini, afegim un nou rol del servidor anomenat “File Server Resource  
Manager”.

![imatge6](../pics/imatge6.png)

Un cop creat aquest dos rols, crearem la estructura de les OUs.

Consisteixen en:

Administracio: Gestió de factures i albarans.
Transport: Xofers i caps de flota.
Direccio: Gerència.

---

Creem dos OUs, una per usuaris i l’altre per grups.

![imatge7](../pics/imatge7.png)

Dintre de la OU de “Grups”, creem els grups demanats:

Administracio
Transport
Direccio

![imatge8](../pics/imatge8.png)

## 2. Implementació de Recursos Compartits (Tres mètodes)

Heu de crear les següents estructures de carpetes al servidor, cadascuna amb un mètode diferent:

### A. Carpeta Public (Mètode: Explorador d'arxius)

Compartiu-la per a tothom.
Configuració: Permisos SMB de "Lectura" i permisos NTFS de "Modificació". Verifiqueu la combinació de permisos efectiva.

IMPORTANT: LA CARPETA “PÚBLIC” ES DIU “CARPETA COMPARTIDA”.

Creem una carpeta dintre del disc “C”.

![imatge9](../pics/imatge9.png)

Anem a “Properties” > “Security” > i donem permisos de escritura a tothom.

![imatge10](../pics/imatge10.png)

Compartim la carpeta desde “Properties” > “Sharing” > “Advanced Sharing” i compartim la carpeta.

![imatge11](../pics/imatge11.png)

Donem permisos de lectura a tothom.

![imatge12](../pics/imatge12.png)

### B. Carpeta Operacions (Mètode: Server Manager - FSSM)

Instal·leu el rol File and Storage Services si no hi és.
Creeu el recurs compartit.
Fer que només es mostri pels usuaris amb accés (Acces-Based Enumeration).
Restricció: Només el grup Transport hi pot accedir.

Creem una carpeta al disc c que es digui Operacions

![imatge39](../pics/imatge39.png)

Entrem al server i anem a l’apartat “Shares” > “New share”.

![imatge13](../pics/imatge13.png)

Seleccionem la opció de “SMB Share - Quick”.

![imatge14](../pics/imatge14.png)

Seleccionem la carpeta Operacions.

![imatge15](../pics/imatge15.png)

Li afegim un nom, en el meu cas “Operacions”.

![imatge16](../pics/imatge16.png)

Tenim diverses configuracions de tipus de compartiment, només activem la opció “Enable  
acces-based enumeration” perquè unicament vegi les carpetes a les que té permisos.

![imatge17](../pics/imatge17.png)

Editem els permisos perquè el grup transports, sigui l’únic grup que tingi accés a la carpeta.

![imatge18](../pics/imatge18.png)

Prenem “Create”.

![imatge19](../pics/imatge19.png)

Resultats de la compartició:

![imatge20](../pics/imatge20.png)

### C. Carpeta Confidencial (Mètode: PowerShell bàsic)

Creeu la carpeta Direccio$ (recurs ocult).
Restricció: només hi pot accedir el grup Direccio.
Utilitzeu el cmdlet New-SmbShare per compartir-la.
Configureu una GPO perquè aquesta carpeta aparegui automàticament com a unitat Z:
  només als usuaris de Direcció.

En primer lloc creem una carpeta anomenada “Direccio” al disc C.

![imatge21](../pics/imatge21.png)

Ara entrem a Powershell com a administrador i introduï la següent comanda per donar  
permisos només al grup “Direccio”.

bash
`New-SmbShare -Name “Direccio$” -Path “C:\Direccio” -FullAccess “Direccio”.`



![imatge22](../pics/imatge22.png)

Anem a propietats de la carpeta , “Sharing” > Advanced Sharing” , comprovem que la  
carpeta és oculta.

![imatge23](../pics/imatge23.png)

Tambe anem a “Security” per veure que nomes te permisos el grup “Direccio”

![imatge44](../pics/imatge44.png)

Anem al explorador de fitxer, “Network” > “Map network drive”.

![imatge24](../pics/imatge24.png)

Afegim la lletra del disc i la seva ruta corresponent.

![imatge25](../pics/imatge25.png)

Ara configurarem una politica (GPO) perquè aquesta carpeta aparegui automàticament com a unitat  
Z: només als usuaris de Direcció.

Entrem a les GPO (Gorup Policy Managment) i entrem al següent arxiu.

![imatge26](../pics/imatge26.png)

Seguim la mateixa ruta i afegim un “Mapped Drive”.

User Configuration > Preferences > Windows Settings > Drive Maps > New > Mapped Drive

![imatge27](../pics/imatge27.png)

Copiem la mateixa configuració.

![imatge28](../pics/imatge28.png)

Tornem a entrar a l’arxiu i anem a la següent ruta:

Computer Configuration > Preferences > Windows Settings > Network Shares i en creem una xarxa compartida.

![imatge29](../pics/imatge29.png)

Copiem la següent configuració.

![imatge30](../pics/imatge30.png)

## 3. Control d'Emmagatzematge (FSRM i Quotes NTFS)

El client es queixa que els usuaris guarden fotos personals i omplen el disc. Heu d'actuar:

---

### Quotes NTFS (Control per Volum)

A la unitat de dades, activeu les quotes NTFS des de les propietats del volum.
Establir un límit de 500 MB per defecte per a qualsevol usuari nou.

Anem a les propietats de disc C, i a l’apartat de “Quota”.

Copiem la mateixa configuració per limitar el espai del disc als usuaris.

![imatge31](../pics/imatge31.png)

Quan apliquem, ens apareixerà aquest avís, prenem “OK”.

![imatge32](../pics/imatge32.png)

### FSRM (Control per Carpeta)

Instal·leu el rol File Server Resource Manager.
Quota de Carpeta: A la carpeta Public, apliqueu una quota de 200 MB (Hard Quota).
  Configureu un avís al 90% que enviï un missatge personalitzat: "Compte! FoodLogístic
  t'informa que estàs a punt d'esgotar l'espai compartit."
Filtrat de Fitxers: A la carpeta Operacions, creeu un filtre que impedeixi guardar arxius
  executables (.exe, .msi) i fitxers d'àudio o vídeo.

Anem a “File Server Resource Manager”.

![imatge33](../pics/imatge33.png)

Un cop dintre, anem a “Quota Management” i entrem a la carpeta “Quotas”.

![imatge34](../pics/imatge34.png)

Un cop dintre de la carpeta, prenem “Create Quota”.

![imatge35](../pics/imatge35.png)

Afegim la ruta de la carpeta i anem a “Custom Properties”.

![imatge36](../pics/imatge36.png)

Afegim el límit i prenem “Add”.

![imatge37](../pics/imatge37.png)

Posem 90% del espai perquè generi la notificació, anem a “Event Log” i afegim el missatge  
personalitzat:

![imatge38](../pics/imatge38.png)

Filtrat de Fitxers: A la carpeta Operacions, creeu un filtre que impedeixi guardar arxius
  executables (.exe, .msi) i fitxers d'àudio o vídeo.


Entrem a l’arxiu d’abans i creem:

![imatge40](../pics/imatge40.png)

Possem la ruta i anem a “Custom Properties”.

![imatge41](../pics/imatge41.png)

Seleccionem aquestes dues opcions:

![imatge42](../pics/imatge42.png)

Creem el filtratge i seleccionem la opció de “Save the custom file screen without creating a  
template”.

![imatge43](../pics/imatge43.png)

## 4. Verificació i Auditoria

Connecteu-vos des d'un client Windows 10/11 i comproveu:

Verifiqueu l’accés i la visibilitat dels recursos per un usuari de cada grup.

Usuari Biel Pérez (Transport):

![imatge45](../pics/imatge45.png)

![imatge46](../pics/imatge46.png)

Usuari Pol Hernández:

![imatge47](../pics/imatge47.png)

![imatge48](../pics/imatge48.png)

Usuari Pau Martínez (Direcció):

![imatge49](../pics/imatge49.png)

![imatge50](../pics/imatge50.png)

Què passa si intenteu copiar un .exe a Operacions?

Que no hi ha accés per poder copiar-ho

![imatge51](../pics/imatge51.png)

Si canvieu l'extensió d'un executable a .txt, el servidor el deixa passar? (Proveu el filtratge actiu).

![imatge52](../pics/imatge52.png)

---

[Torna al enunciat](README.md)

[Torna a la pàgina principal](../README.md)