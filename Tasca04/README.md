# T04: Servidor d’impressió

## Introducció

En el món de la logística, la impressió continua tenint un paper molt important: albarans, fulls de transport, etc., necessiten ser impresos. El vostre client no és una excepció.

L'empresa té un magatzem on el volum d'impressió d'albarans és crític. Si una impressora falla o es col·lapsa, els camions no poden sortir, cosa que trenca la cadena de fred. L'objectiu d'aquesta activitat és configurar un **Servidor d'Impressió a Windows Server** que gestioni aquest volum mitjançant **Printer Pooling** (cua d’impressió compartida) per balancejar la càrrega entre dos dispositius.

# Descripció de l'activitat

El servidor haurà de tenir implementats:
- Serveis de directori (**Active Directory**)
- Servei de servidor d’impressió (**Print Server**)

Això permetrà la gestió centralitzada d’usuaris, equips i recursos d’impressió.  
El client, integrat al domini, haurà de poder autenticar-se amb credencials de xarxa i accedir als recursos compartits, especialment a les impressores desplegades des del servidor.

Heu de documentar cada pas del procés com si es tractés d’un **informe tècnic per a un client real**, utilitzant llenguatge clar i justificacions adequades.

L’activitat es divideix en quatre fases tècniques:

---

## 1. Preparació de l'entorn i simulació de maquinari

- Verificar o preparar l’entorn necessari (o reutilitzar-ne un d’existent).
- Instal·lar i configurar **dues instàncies d’impressora PDF24** al servidor (guia disponible als materials).
- Assignar noms corporatius:
  - `IMP_MAGATZEM_A`
  - `IMP_MAGATZEM_B`

---

## 2. Instal·lació del Rol i Configuració del Pool

### Instal·lació del rol
- Instal·lar el rol **Print and Document Services** al Windows Server.
- Accedir a la consola **Print Management**.

### Configuració del Printer Pooling
1. Obrir les propietats de `IMP_MAGATZEM_A`.
2. A la pestanya **Ports**, activar la casella **Enable printer pooling**.
3. Seleccionar també el port assignat a la impressora `IMP_MAGATZEM_B`.
4. A partir d’aquest punt, ambdues impressores treballen sota **un mateix nom de xarxa**, repartint-se la càrrega.

**Nota tècnica:**  
A `IMP_MAGATZEM_A` han de quedar seleccionats **els dos ports virtuals**.

---

## 3. Desplegament automatitzat mitjançant GPO

L’empresa no vol que els mossos de magatzem instal·lin manualment la impressora.

Passos:
- Crear una GPO anomenada: **GPO_Impressores_Magatzem**.
- Utilitzar l'opció **Deploy with Group Policy** des de Print Management.
- Vincular la impressora al domini o bé a una **OU específica** on es trobi l’usuari de proves.
- Verificar que en iniciar sessió a un client Windows 11, la impressora apareix automàticament.

**Nota tècnica:**  
Pot ser necessari:
- Tancar sessió
- O forçar l’actualització amb `gpupdate /force`

---

## 4. Prova de càrrega i Seguretat

### Simulació de balanceig
- Enviar **10 documents seguits** a la impressora del magatzem.
- Observar com el servidor reparteix la càrrega entre les dues instàncies.

### Restriccions horàries
- Configurar:
  - *Printing Priorities*  
  **o**
  - *Available Times*
- L’objectiu és que la impressora només funcioni en horari laboral  
  **Exemple:** de **06:00 a 22:00**.

  ---

A l'arxiu [solucio.md](solucio.md) hi ha la solució de la Tasca04

[Torna a la pàgina principal](../README.md)