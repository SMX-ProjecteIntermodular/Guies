# Guia 7: Treball col·laboratiu amb Git i GitHub

> "Cap de nosaltres és tan intel·ligent com tots nosaltres junts."
>
> — Ken Blanchard

## Introducció

A la guia anterior hem vist com consultar l'historial de commits i com desfer canvis locals o remots mitjançant `git reset` i `git revert`. Quan col·labores amb altres persones o contribueixes a un projecte que no t'és propi, també has de saber organitzar la feina amb GitHub.

En aquesta guia aprendràs tres mecanismes fonamentals:

1. **`Fork`**: per obtenir una còpia independent d'un repositori remot sobre el qual no tens permisos d'escriptura directa.
2. **`Pull Request` (PR)**: per proposar, revisar i validar canvis abans d'integrar-los al projecte original.
3. **`Issues`**: per planificar tasques, documentar errors (*bugs*) i organitzar la feina de l'equip de manera traçable.

El flux que seguirem és senzill: crearàs un *fork*, hi faràs els canvis, els pujaràs a GitHub i obriràs un *Pull Request* cap al repositori original.

---

## 1. `Fork` de repositoris

### Què és un `fork` i quan s'utilitza?

Un **`fork`** és una còpia completa i independent d'un repositori de GitHub creada dins del teu compte o organització. A diferència d'un simple `git clone`, que només descarrega el projecte a l'ordinador, un *fork* crea un **repositori remot propi al núvol**.

Aquest mecanisme és útil en dos escenaris habituals:

* **Contribució a projectes *Open Source* o d'altres equips:** no tens permisos d'escriptura al repositori original, però pots modificar la còpia del teu compte i enviar una proposta de canvi mitjançant un *Pull Request*.
* **Entorns educatius i plantilles de pràctiques:** el professorat publica un repositori base amb l'enunciat o l'esquelet del projecte. Cada alumne en fa un *fork*, hi desenvolupa la solució i lliura el *Pull Request* o l'enllaç al seu *fork*.

### Els dos remots: `origin` i `upstream`

Quan treballes amb un *fork*, intervenen tres còpies del projecte:

* **`upstream` (repositori original):** és el projecte del qual prové el *fork*. Normalment hi tens permisos de lectura.
* **`origin` (el teu *fork* a GitHub):** és la teva còpia al núvol. Hi pots publicar els teus commits.
* **Repositori local (el teu ordinador):** és la còpia en què treballes amb Visual Studio Code.

### Procediment pas a pas per crear i clonar un `fork`

#### Pas 1: Crear el *fork* a GitHub

1. Accedeix amb el navegador al repositori original de GitHub.
2. Fes clic al botó **Fork** de la cantonada superior dreta.
3. Selecciona el teu usuari com a propietari (*Owner*).
4. Mantén el nom proposat, llevat que el professorat o l'enunciat indiquin el contrari.
5. Prem **Create fork**.

#### Pas 2: Clonar el teu *fork* amb Visual Studio Code

1. Obre Visual Studio Code amb la sessió iniciada al teu compte de GitHub.
2. Obre la paleta d'ordres (`Ctrl + Shift + P` o `Cmd + Shift + P` a macOS) i executa **`Git: Clone`**.
3. Selecciona **Clone from GitHub**.
4. Tria el repositori del teu compte (`el-teu-usuari/nom-repositori`).
5. Escull la carpeta local on vols desar el projecte i prem **Open**.

El repositori remot del teu compte quedarà configurat automàticament com a `origin`.

#### Pas 3: Vincular el repositori original (`upstream`)

Si vols rebre les actualitzacions del projecte original al repositori local, afegeix el repositori corresponent amb el nom `upstream`.

Des de Visual Studio Code:

1. Obre la paleta d'ordres.
2. Executa **`Git: Add Remote`**.
3. Enganxa l'URL HTTPS del repositori original i prem `Retorn`.
4. Escriu el nom **`upstream`** i prem `Retorn`.

També ho pots fer des del terminal:

```bash
git remote add upstream https://github.com/propietari-original/nom-repositori.git
git remote -v
```

La sortida ha de mostrar les URL corresponents a `origin` i `upstream`.

### Sincronitzar el *fork* amb el repositori original

Abans de començar una tasca nova, comprova si el repositori original ha rebut canvis:

1. Entra al teu *fork* a GitHub.
2. Localitza l'avís que indica si el repositori està al dia respecte a l'original.
3. Si cal actualitzar-lo, fes clic a **Sync fork** i segueix les indicacions de GitHub.
4. Després, torna a Visual Studio Code i prem **Sincronitzar canvis** al panell de control de codi font.

Quan el teu repositori local estigui actualitzat, ja podràs començar a editar els fitxers.

---

## 2. `Pull Request` (PR) com a mecanisme de col·laboració

### Què és un `Pull Request`?

Un **`Pull Request` (PR)** és una petició formal als responsables d'un repositori perquè revisin i integrin els canvis que has preparat al teu *fork*.

Més enllà d'aportar codi, un *Pull Request* és una eina central de col·laboració i qualitat a GitHub:

* **Revisió de codi (*Code Review*):** l'equip pot inspeccionar les diferències línia per línia, deixar comentaris i suggerir millores.
* **Validació automàtica (CI/CD):** es poden executar proves unitàries, comprovacions d'estil (*linters*) i compilacions automàtiques.
* **Documentació de les decisions:** el títol, la descripció, els comentaris i els commits expliquen què s'ha canviat i per què.

### Com preparar els canvis

1. Comprova que el teu *fork* estigui sincronitzat amb el repositori original.
2. Edita els fitxers necessaris a Visual Studio Code.
3. Desa els canvis.
4. Obre el panell **Control de codi font** (`Ctrl + Shift + G` o `Cmd + Shift + G` a macOS).
5. Revisa les modificacions abans de confirmar-les.
6. Fes clic a `+` per preparar (*stage*) els fitxers que vulguis incloure al commit.
7. Escriu un missatge de *commit* descriptiu i prem **Commit**.
8. Prem **Sincronitzar canvis** per publicar el commit al teu *fork* de GitHub.

Des del terminal, l'equivalent seria:

```bash
git add .
git commit -m "fix: corregeix el càlcul de l'IVA al resum de factura"
git push origin
```

> 💡 **Recordatori:** Comprova amb `git status` que no hi hagi fitxers importants pendents de preparar o publicar.

### Com crear un `Pull Request`

> 💡 **Recomanació per a VS Code:** pots instal·lar l'extensió oficial **GitHub Pull Requests and Issues**, publicada per GitHub. Permet consultar PR i *Issues* sense sortir de l'editor.

#### Des de la pàgina web de GitHub

1. Obre el teu *fork* a GitHub després de publicar els commits.
2. Fes clic a **Contribute**.
3. Selecciona **Open pull request**.
4. Comprova que el repositori de destinació sigui el projecte original.
5. Revisa la llista de canvis.
6. Escriu un títol clar i una descripció completa.
7. Afegeix els revisors indicats per l'equip o el professorat.
8. Prem **Create pull request**.

Si el canvi està relacionat amb un *Issue*, escriu `Closes #<número>` a la descripció. D'aquesta manera, GitHub establirà la relació entre tots dos elements.

### Bones pràctiques per obrir un `Pull Request`

* **Fes PR petits i atòmics:** cada *Pull Request* ha de resoldre una única tasca o funcionalitat concreta.
* **Escriu un títol clar:** resumeix l'objectiu del canvi, per exemple, `feat: afegeix filtre de cerca per categoria`.
* **Redacta una descripció completa:** explica **què** fa el canvi, **per què** és necessari i **com provar-lo**. Afegeix captures de pantalla quan modifiquis una interfície gràfica.
* **Vincula la tasca relacionada:** inclou `Closes #14` si el PR resol l'*Issue* número 14.
* **Fes una auto-revisió:** repassa el *diff* abans d'enviar-lo per detectar codi mort, missatges de prova, fitxers temporals o canvis no relacionats.
* **Utilitza un *Draft Pull Request* per a feina en curs:** si vols rebre comentaris abans d'acabar, selecciona **Create draft pull request**. Així els revisors sabran que la proposta encara no està preparada per integrar.
* **No afegeixis canvis d'altres tasques:** si reps una incidència nova durant la revisió, registra-la en un *Issue* separat.

### Bones pràctiques per revisar i acceptar un `Pull Request`

Quan actuïs com a revisor o revisora:

* **Revisa amb empatia i rigor tècnic:** comenta el codi, no l'autor. Formula suggeriments constructius.
* **Comprova que el canvi resol la tasca:** contrasta el codi i les proves amb la descripció del PR i l'*Issue* associat.
* **Prova els canvis:** executa l'aplicació i les proves indicades per l'autor.
* **Verifica els *checks* automàtics:** no aprovis ni integris un PR amb proves fallides o conflictes sense resoldre.
* **Deixa comentaris clars:** explica el problema observat, la ubicació exacta i, si és possible, una alternativa.
* **Segueix el criteri d'integració del projecte:** si la interfície de GitHub ofereix més d'una opció, consulta les normes del repositori o pregunta-ho al professorat.

Si l'autor ha de corregir alguna observació, pot fer nous commits al seu *fork*. Aquests commits s'afegiran automàticament al PR pendent.

---

## 3. `Issues`: utilitat, gestió i bones pràctiques

### Què és un `Issue` i per a què serveix?

Un **`Issue`** és el sistema de seguiment de tasques integrat a GitHub. Tot i que sovint s'associa a errors de programari, també serveix per organitzar qualsevol unitat de feina:

* **Errors (*Bugs*):** documentar fallades detectades durant el desenvolupament o les proves.
* **Noves funcionalitats:** descriure una millora abans de començar a programar-la.
* **Tasques tècniques i documentals:** refactoritzar codi, actualitzar guies, configurar serveis o preparar un desplegament.
* **Planificació:** integració amb **GitHub Projects** per visualitzar l'estat de les tasques.

### Elements clau d'un `Issue`

1. **Títol:** descriu el problema o la tasca de manera breu i concreta.
2. **Descripció:** aporta el context, els criteris d'acceptació i els passos necessaris per reproduir o completar la feina.
3. **Etiquetes (*Labels*):** categoritzen l'*Issue* segons la naturalesa o prioritat. Les més habituals són:
   * `bug`
   * `enhancement`
   * `documentation`
   * `good first issue`
   * `blocked`
4. **Assignacions (*Assignees*):** indiquen qui és responsable de la tasca.
5. **Fites (*Milestones*):** agrupen diversos *Issues* dins d'un lliurament, una versió o un període concret.
6. **Vinculació amb *Pull Requests*:** permet veure quina proposta de canvi resol cada incidència.

### Crear i tancar un `Issue` automàticament

1. Obre la pestanya **Issues** del repositori a GitHub.
2. Prem **New issue**.
3. Selecciona una plantilla, si n'hi ha.
4. Escriu un títol descriptiu.
5. Completa la descripció i els criteris d'acceptació.
6. Assigna les etiquetes, les persones i la fita corresponents.
7. Prem **Submit new issue**.

Quan el canvi estigui preparat, crea un *Pull Request* i afegeix a la seva descripció una frase com:

```text
Closes #12
```

Quan els responsables acceptin el PR al repositori original, GitHub tancarà automàticament l'*Issue* número 12.

### Gestió d'`Issues` des de Visual Studio Code

Amb l'extensió **GitHub Pull Requests and Issues** instal·lada:

1. Obre la vista **GitHub** de la barra d'activitat.
2. Desplega l'apartat **Issues**.
3. Selecciona un *Issue* per consultar-ne la informació.
4. Utilitza el botó **+** per crear-ne un de nou sense sortir de l'editor.

Després d'iniciar la feina, mantén l'*Issue* actualitzat amb comentaris breus sobre l'estat, les dificultats o els resultats obtinguts.

### Bones pràctiques en la creació i gestió d'`Issues`

* **Un tema per cada *Issue*:** no barregis errors o funcionalitats independents en una mateixa incidència.
* **Escriu títols descriptius i concrets:** evita títols com *"Error a la web"* o *"No funciona"*.
* **Afegeix tot el context necessari:** quan reportis un error, inclou:
  * Una descripció breu.
  * Els passos exactes per reproduir-lo.
  * El comportament esperat.
  * El comportament observat.
  * L'entorn utilitzat: sistema operatiu, navegador, versió del llenguatge o *framework*.
* **Afegeix proves quan sigui possible:** una captura de pantalla, un missatge d'error o un exemple mínim faciliten la investigació.
* **Comprova que no sigui duplicat:** cerca entre els *Issues* oberts i tancats abans de crear-ne un de nou.
* **Mantén la conversa actualitzada:** si canvies d'enfocament, trobes una solució alternativa o necessites ajuda, deixa'n constància perquè l'equip conegui l'estat real de la tasca.
