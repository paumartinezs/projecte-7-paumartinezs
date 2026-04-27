# T09: Estimació temporal de projecte (Diagrama de Gantt professional)

## 📝 Breu descripció
Un dels errors més habituals en equips tècnics novells és confondre **“fer feina”** amb **“gestionar un projecte”**. Començar a configurar servidors, desenvolupar una web o desplegar serveis sense una planificació rigorosa acostuma a provocar retards, colls d’ampolla i solucions improvisades.

En aquest punt del projecte, la direcció de **FoodLogístics S.A.** no vol només una proposta tècnica: **vol garanties**. Vol saber:

* Quan estaran les solucions operatives.
* Quin ordre seguireu.
* Què passarà si alguna tasca es retarda.
* Si sou capaços de treballar com un equip professional.

> **Nota:** El vostre objectiu no és només "fer un Gantt", sinó demostrar que sabeu planificar un projecte real amb criteri tècnic i organitzatiu.

---

## 🎯 Objectius específics de la tasca
Treballareu com a equip de projecte per construir una planificació completa i realista del desplegament de la solució per a FoodLogístics S.A., utilitzant un diagrama de Gantt generat amb **PlantUML (UMLTree)**.

---

## 🚀 Fases de l'activitat

### Fase 1: Anàlisi real del projecte (pensament estructural)
**1.1 Identificació de tasques i dependències** A partir de les tasques reals del projecte (**T01–T08**), identifiqueu:
* Ordre lògic d’execució.
* Tasques que poden anar en paral·lel.
* Tasques bloquejants.
* *Preguntes clau:* Quines tasques no poden començar sense haver-ne acabat una altra? On poden aparèixer colls d’ampolla? Quines tasques són més crítiques?

**1.2 Identificació del camí crític** Determineu quines tasques, si es retarden, afecten tot el projecte i quines tenen marge (*slack*).

### Fase 2: Estimació d’esforç amb criteri (ús d’IA guiat)
Heu d’estimar la durada de cada tasca en hores analitzant els següents **factors obligatoris**:
* Temps de comprensió (lectura, anàlisi) i recerca.
* Temps d’implementació tècnica, proves i errors.
* Temps de documentació i coordinació amb l’equip.
* Possibles interrupcions (classes, exàmens) i temps de marge per imprevistos.
* **Ús d’IA:** Obligatori com a assistent, però amb criteri professional.

### Fase 3: Assignació de recursos (treball en equip real)
Distribuïu les tasques entre els membres de l’equip:
* Definició de rols (qui fa què).
* Gestió de tasques compartides i dependències entre membres.
* **⚠️ Evitar:** Sobrecàrrega d'una persona o temps morts en altres.

### Fase 4: Construcció del diagrama de Gantt (UMLTree)
Utilitzant PlantUML, el diagrama ha de mostrar:
* Tasques (T01–T08) i durada.
* Dependències i execució en paral·lel.
* Visió temporal de les **3 setmanes**.

### Fase 5: Pla de contingència (pensament professional)
Identifiqueu com a mínim **2 riscos crítics reals** (Exemple: retard en servidor, problemes amb cloud) i definiu el seu impacte i l'estratègia de mitigació.

---

## 📂 Què cal lliurar
El lliurament s'ha de fer dins la carpeta `/P01/Planificacio/` del repositori.

### 1. Document de planificació (`README.md`)
Informe professional que inclogui:
- Una breu introducció i context.
- Explicació de l’ordre de les tasques i dependències.
- Justificació de les estimacions temporals.
- Explicació setmana a setmana de la distribució del treball.
- Decisions més importants i reflexió sobre punts crítics.
- Captures de pantalla i evidències del procés.

### 2. Codi PlantUML i imatge del diagrama
- Arxiu amb el codi `.puml` o text.
- Imatge exportada del diagrama de Gantt (coherent amb el README).

### 3. Matriu d’assignació de responsabilitats (RACI)
Taula on es vegi el paper de cada membre:
* **R** (Responsible), **A** (Accountable), **C** (Consulted), **I** (Informed).

### 4. Taula de riscos i contingències
Taula amb: problema possible, part afectada, impacte i mesura de resposta.

---

## ❓ Preguntes clau (Obligatòries al README)
1. Quina és la tasca més crítica del projecte i per què?
2. On heu detectat el principal coll d’ampolla?
3. Quina decisió de planificació ha estat més difícil?
4. Heu hagut de modificar alguna estimació inicial? Per què?
5. Quin risc podria fer fracassar el projecte?
6. Si tinguéssiu una setmana més, què canviaríeu?

---

## ⭐ Criteris de qualitat
* **Coherència** del Gantt amb el projecte.
* **Realisme** de les estimacions.
* **Comprensió** de dependències i camí crític.
* **Qualitat del README** (professional, estructurat i justificat).
* **Ús crític de la IA.**

---

A  l'arxiu [Planificació](Planificacio) hi ha la solució de la Tasca09

[Torna a la pàgina principal](../README.md)
