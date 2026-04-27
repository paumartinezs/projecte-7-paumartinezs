# T03: Servidor de fitxers

## Introducció

Quan el volum de negoci creix, també ho fa el volum de dades i sovint aquí comencen els problemes. La informació es compartimenta, de manera que manca una visió global de la situació.

FoodLogistic no ha estat una excepció, cada departament guardava la documentació de forma local. És per això que un dels requisits de l’encàrrec que n’heu rebut és solucionar aquest problema.

L'objectiu és implementar una infraestructura de fitxers segura, organitzada i amb control d'espai, utilitzant permisos **NTFS/SMB**, **quotes** i **filtratge de fitxers (FSRM)**. Haureu de demostrar el domini de les tres vies d'administració: **l'Explorador de fitxers**, el **Server Manager** i **PowerShell**.

# Descripció de l'activitat

L'activitat es divideix en fites que simulen el procés de consultoria i implementació real.

---

## 1. Preparació i Seguretat de Grups (AD)

Abans de crear el servidor de fitxers, cal preparar el terreny a l'Active Directory. Sobre un domini (`foodlogistic.test`), on se us recomana crear una estructura de OUs coherent —que caldrà justificar per les necessitats que desenvolupareu—, creeu els següents grups de seguretat:

- **Administracio**: Gestió de factures i albarans.  
- **Transport**: Xofers i caps de flota.  
- **Direccio**: Gerència.  

---

## 2. Implementació de Recursos Compartits (Tres mètodes)

Heu de crear les següents estructures de carpetes al servidor, cadascuna amb un mètode diferent:

### A. Carpeta *Public* (Mètode: Explorador d'Arxius)

- Compartiu-la per a tothom.  
- **Permisos SMB**: Lectura  
- **Permisos NTFS**: Modificació  
- Verifiqueu la combinació de permisos efectiva.

---

### B. Carpeta *Operacions* (Mètode: Server Manager – FSSM)

1. Instal·leu el rol **File and Storage Services** si no hi és.  
2. Creeu el recurs compartit.  
3. Activeu **Access-Based Enumeration** (mostrar només als usuaris amb accés).  
4. **Restricció**: Només el grup **Transport** hi pot accedir.

---

### C. Carpeta *Confidencial* (Mètode: PowerShell bàsic)

- Creeu la carpeta `Direccio$` (recurs ocult).  
- **Restricció**: només hi pot accedir el grup **Direccio**.  
- Utilitzeu `New-SmbShare` per compartir-la.  
- Configureu una **GPO** perquè aparegui com a unitat **Z:** només pels usuaris de Direcció.

---

### D. Carpeta *Confidencial* (Mètode: PowerShell avançat)

*(Podeu triar entre el mètode C o D; el D té una valoració més alta.)*

- Creeu la carpeta `Direccio`.  
- **Restricció**: només grup **Direccio**.  
- Utilitzeu `New-SmbShare` habilitant **Access-Based Enumeration** via PowerShell.  
- Configureu GPO per mapar la unitat **Z:** als usuaris de Direcció.

---

## 3. Control d'Emmagatzematge (FSRM i Quotes NTFS)

El client es queixa que els usuaris guarden fotos personals i omplen el disc. Heu d'actuar:

### Quotes NTFS (Control per Volum)

- A la unitat de dades, activeu les quotes NTFS.  
- Límit per defecte: **500 MB** per a usuaris nous.

### FSRM (Control per Carpeta)

1. Instal·leu el rol **File Server Resource Manager**.  

#### Quota de Carpeta
- A la carpeta **Public**, quota de **200 MB (Hard Quota)**.  
- Avís al 90% amb missatge personalitzat:  
  > "Compte! FoodLogístic t'informa que estàs a punt d'esgotar l'espai compartit."

#### Filtrat de Fitxers
- A la carpeta **Operacions**, creeu un filtre que impedeixi guardar:
  - `.exe`
  - `.msi`
  - Fitxers d’àudio/vídeo  

---

## 4. Verificació i Auditoria

Des d'un client Windows 10/11:

- Verifiqueu l’accés i visibilitat per un usuari de cada grup.  
- Proveu què passa si copieu un `.exe` a Operacions.  
- Si canvieu l’extensió d’un `.exe` a `.txt`, el filtratge actiu ho detecta?

---

# Què cal lliurar

Es lliurarà un **informe tècnic detallat** en format **Markdown** que inclogui:

### ✅ Resum de Configuració
Taula amb:
- Nom de carpeta  
- Camí UNC  
- Grups amb accés  
- Mètode de creació usat  

### ✅ Evidències
Captures i explicacions de configuracions dels tres apartats.

### ✅ Proves de funcionament
Verificacions des del client:
- Controls d’accés  
- Quotes  
- Restriccions de tipus de fitxers  

---

A l'arxiu [solucio.md](solucio.md) hi ha la solució de la Tasca03

[Torna a la pàgina principal](../README.md)