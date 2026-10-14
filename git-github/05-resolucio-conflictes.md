# Guia 5: Resolució de conflictes a GitHub

## Introducció

En un món ideal, treballar amb control de versions seria un procés lineal i sense conflictes. Malauradament, això no sempre és així. En aquesta fase veurem com es poden produir conflictes i com resoldre'ls.

Primer de tot, què és un conflicte? Un conflicte es produeix quan un fitxer pateix modificacions en dos llocs diferents, pot ser al repositori local i al repositori remot, dues branques diferents (no patiu encara que és un tema que veurem més endavant) o també quan dos o més usuaris treballen sobre el mateix repositori. Quan això passa i fem una sincronització, Git no sap quina versió del fitxer és la correcta i ens demana que resolguem el conflicte.

A l'escenari més senzill, amb un sol usuari treballant al repositori. En aquests casos, les bones pràctiques poden evitar l'aparició de conflictes. Com s'indica a la [Guia 2. Treballant en el repositori amb Git](02-comandes-git.md) la bona pràctica per evitar conflictes és assegurar-nos de no començar a editar en local fins no haver sincronitzat amb el repositori remot i assegurar-nos que en acabar la sessió de treball, sincronitzem tots els commits locals amb el repositori remot.

Quan es treballa en equip, és molt més fàcil que apareguin conflictes. Per això, és important que tots els membres de l'equip segueixin les bones pràctiques i sincronitzin sovint amb el repositori remot.

En aquesta guia treballarem amb l'escenari d'usuari únic treballant sense branques, però els conceptes que veurem són aplicables a qualsevol escenari.

## Resolució de conflictes

### L'escenari

Imaginem el cas d'un sol usuari treballant sobre un repositori sense branques (és a dir, treballant sempre a `main`). Aquest usuari fa canvis directament des del navegador de GitHub (per exemple, edita un fitxer des de la interfície web, o bé algú altre amb accés hi fa un canvi) sense que l'usuari els hagi baixat (`pull`) al seu ordinador.

Mentrestant, a l'ordinador local, l'usuari també edita fitxers, fa `add` i `commit` dels seus canvis. Quan intenta pujar (`push`) els seus commits des de Visual Studio Code, Git el rebutja perquè el repositori remot conté canvis (els fets des de GitHub) que l'usuari no té encara al seu historial local.

En aquest escenari, l'ordre dels fets és:

1. Al repositori remot (GitHub) es produeix un canvi que genera un nou commit que l'usuari no té en local.
2. L'usuari, sense haver fet `pull` abans, edita el mateix fitxer en local, el prepara (`add`) i el confirma (`commit`).
3. Quan intenta sincronitzar (`push`), VS Code mostra un avís indicant que el repositori remot té canvis que no estan al local i que cal baixar-los primer.

### Pas 1: Intentar sincronitzar i rebre l'avís

Quan premem el botó de **Sincronitzar els canvis** (o **Push**) a la barra d'estat inferior de VS Code, si el remot té commits que el local no té, Git/VS Code aturarà l'operació i proposarà fer primer un `pull` (baixar els canvis).

En acceptar, VS Code baixarà els canvis remots i intentarà fusionar-los (`merge`) automàticament amb els canvis locals. Si els dos canvis afecten línies diferents del fitxer, Git els combina sol i no cal fer res més. Però si els dos canvis afecten **les mateixes línies** d'un fitxer, Git no pot decidir quina versió és la correcta i es produeix un **conflicte**.

### Pas 2: Identificar els fitxers en conflicte

Quan hi ha un conflicte, VS Code ho indica de diverses maneres:

- A la barra lateral del **Control de codi font** (icona de Git), el fitxer afectat apareix dins d'un apartat anomenat **Merge Changes** (Canvis de fusió), amb una icona d'exclamació.
- Si obrim el fitxer, Git hi insereix unes marques especials per delimitar les dues versions en conflicte:

```plain
<<<<<<< HEAD
// Contingut de la versió local (els canvis que hem fet nosaltres)
=======
// Contingut de la versió remota (els canvis que ja hi havia a GitHub)
>>>>>>> origin/main
```

- `<<<<<<< HEAD` marca l'inici del nostre canvi local.
- `=======` separa les dues versions.
- `>>>>>>> origin/main` marca el final del canvi que ve del repositori remot.

![Conflicte mostrat a Visual Studio Code](img/VSCodeConflicte.png)

### Pas 3: Resoldre el conflicte amb l'editor de VS Code

VS Code facilita molt la resolució de conflictes gràcies al seu editor integrat. En obrir un fitxer en conflicte, just a sobre de cada bloc en conflicte apareixen uns enllaços d'acció ràpida:

- **Accept Current Change** (Acceptar el canvi actual): es queda només amb la versió local (la que hi ha entre `<<<<<<< HEAD` i `=======`).
- **Accept Incoming Change** (Acceptar el canvi entrant): es queda només amb la versió remota (la que hi ha entre `=======` i `>>>>>>> origin/main`).
- **Accept Both Changes** (Acceptar ambdós canvis): manté les dues versions, una després de l'altra.
- **Compare Changes** (Comparar els canvis): obre una vista en paral·lel per comparar amb més detall ambdues versions abans de decidir.

Per resoldre el conflicte:

1. Clica l'opció que correspongui segons quin contingut final vulguis (o bé edita manualment el text per combinar ambdues versions com creguis convenient).
2. Comprova que les marques `<<<<<<< HEAD`, `=======` i `>>>>>>> origin/main` han desaparegut del fitxer. Si queda alguna marca, el conflicte encara no està resolt.
3. Desa el fitxer.

Si hi ha més d'un fitxer en conflicte, repeteix el procés amb cadascun d'ells. VS Code et permet anar saltant entre conflictes dins del mateix fitxer amb els botons **Previous Change** i **Next Change** que apareixen a la part superior de l'editor.

### Pas 4: Marcar el conflicte com a resolt i finalitzar la fusió

1. Un cop resolts tots els fitxers en conflicte, torna a la vista de **Control de codi font**.
2. Afegeix els fitxers resolts a la zona de preparació (**Stage Changes**), clicant el símbol `+` al costat de cada fitxer o des del botó **Stage All Changes**.
3. Confirma la fusió amb un **Commit** (VS Code sol proposar automàticament un missatge del tipus `Merge branch 'main' of ...`, que es pot deixar tal com està o personalitzar).
4. Finalment, torna a prémer **Sincronitzar els canvis** (o **Push**) per pujar al repositori remot tant el merge com els teus commits originals.
