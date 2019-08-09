# react-native-style-guide
Guida di riferimento per i progetti React Native gestiti da Tiknil e i suoi collaboratori.

L'obiettivo è darsi delle **best practices** sulla stesura del codice per agevolare il lavoro in team e velocizzare la comprensione del codice.

## Riferimenti
Alcuni contenuti di questa guida sono ispirati dai seguenti interessanti articoli di settore:

* *[AirBnb JavaScript Style Guide](https://github.com/airbnb/javascript)*: ottima linea guida per la stesura del codice JavaScript creata da *AirBnb* nel periodo in cui ha investito molto su React Native.

## Sommario
- [react-native-style-guide](#react-native-style-guide)
  - [Riferimenti](#Riferimenti)
  - [Sommario](#Sommario)
  - [Prerequisiti](#Prerequisiti)
  - [Boilerplate](#Boilerplate)
  - [Stile della sintassi](#Stile-della-sintassi)
      - [Let e Const](#Let-e-Const)
      - [Destructuring](#Destructuring)
      - [Moduli](#Moduli)
      - [Classi](#Classi)
      - [Parametri di default](#Parametri-di-default)
      - [Template strings](#Template-strings)
      - [Arrow functions](#Arrow-functions)
      - [this](#this)
    - [ESLint](#ESLint)
    - [Altro](#Altro)
  - [IDE](#IDE)
  - [Tecnologie utilizzate](#Tecnologie-utilizzate)
  - [Struttura del progetto](#Struttura-del-progetto)
  - [React Native](#React-Native)
  - [Redux](#Redux)
  - [Immutable](#Immutable)
  - [Navigation](#Navigation)
  - [Persistenza dei dati](#Persistenza-dei-dati)
  - [Test](#Test)

## Prerequisiti
Per realizzare un'applicazione in **React Native** è (ovviamente) un prerequisito fondamentale la conoscenza di **JavaScript**.

## Boilerplate
Impostando il progetto a partire dal [boilerplate tiknil](https://github.com/tiknil/react-native-boilerplate) si ottiene un progetto già configurato secondo le convenzioni Tiknil, con i moduli principali già installati e pronto per lo sviluppo 🚀


## Stile della sintassi

In _JavaScript_ i `;` sono opzionali e noi preferiamo non utilizzarli perché il codice rimane più pulito e in molti casi ne agevola il mantenimento.
<br>Ad esempio in questo caso:

```
// 👎
promise()
	.then(thenCallback)
	.catch(catchCallback);
	
// 👍
promise()
	.then(thenCallback)
	.catch(catchCallback)
```
Infatti se devo aggiungere il metodo `finally` mi basta aggiungere una riga senza dover cancellare (o spostare) il `;`.

```
promise()
	.then(thenCallback)
	.catch(catchCallback)
	.finally(finallyCallback)
```
È comunque permesso inserire un `;` in qualsiasi caso in cui la sintassi non fosse particolarmente comprensibile.

Faremo, inoltre, utilizzo di *ECMAScript 6+ (ES 2015+)* per utilizzare vari improvements che permettono uno stile di programmazione più evoluto:

* [Let e Const](#let-e-const)
* [Destructuring](#destructuring)
* [Moduli](#moduli)
* [Classi](#classi)
* [Parametri di default](#parametri-di-default)
* [Template strings](#template-strings)
* [Arrow functions](#arrow-functions)
* [this](#this)
* [ESLint](#eslint)
* [Altro](#altro)

#### Let e Const
Evitare di dichiarare variabili tramite `var` e usare invece `const` e `let` a seconda che siano costanti o variabili.
<br>Essi infatti sono *block-scoped*, al contrario di `var` che è *global-scoped* con ovvi vantaggi al fine di evitare errori di riassegnazione di variabili fuori dallo *scope* corrente.

#### Destructuring
Quando è necessario accedere a proprietà multiple di un oggetto è utile utilizzare l'operazione di *destructuring*:

```
// 👎
function getFullName(user) {
  const firstName = user.firstName
  const lastName = user.lastName

  return `${firstName} ${lastName}`
}
	
// 👍
function getFullName(user) {
  const { firstName, lastName } = user
  return `${firstName} ${lastName}`
}

// 👍: è addirittura possibile destrutturare direttamente nel parametro
function getFullName({ firstName, lastName }) {
  return `${firstName} ${lastName}`
}
```

È possibile applicare il *destructuring* anche agli array. Nota: l'ordine è importante!

```
const arr = [1, 2, 3, 4]

// 👎
const first = arr[0]
const second = arr[1]

// 👍
const [first, second] = arr
```

#### Moduli
Utilizzare sempre `import / export` per gestire importazione ed esportazione dei moduli.

```
// 👎
const ReactNativeStyleGuide = require('./ReactNativeStyleGuide');
module.exports = ReactNativeStyleGuide.es6;

// 👍
import ReactNativeStyleGuide from './ReactNativeStyleGuide';
export default ReactNativeStyleGuide.es6;

// 👍: è possibile destrutturare direttamente nell'import
import { es6 } from './ReactNativeStyleGuide';
export default es6;
```

Unica eccezione è per gli [inline requires volti ad ottimizzare il tempo di caricamento dell'app tramite RAM bundles](https://facebook.github.io/react-native/docs/performance#ram-bundles-inline-requires)

#### Classi
Finalmente anche in *JavaScript* è possibile utilizzare la sintassi a classi, molto più leggibile del vecchio sistema a *prototypes*.

```
// 👎
function Queue(contents = []) {
  this.queue = [...contents];
}
Queue.prototype.pop = function () {
  const value = this.queue[0];
  this.queue.splice(0, 1);
  return value;
};

// 👍
class Queue {
  constructor(contents = []) {
    this.queue = [...contents];
  }
  pop = () => {
    const value = this.queue[0];
    this.queue.splice(0, 1);
    return value;
  }
}
```


#### Parametri di default
Ai parametri dei metodi è possibile assegnare un valore di default nel caso il metodo venga invocato senza quel parametro.

```
// 👎: molto male; mai mutare il contenuto dei parametri
function handleThings(opts) {
  opts = opts || {};
  // ...
}

// 👍
function handleThings(opts = {}) {
  // ...
}
```

#### Template strings
Quando vanno costruite programmaticamente delle stringhe è meglio utilizzare il *template* piuttosto che la *concatenazione* perché offre una sintassi più chiara e concisa.

```
// 👎
function sayHi(name) {
  return 'How are you, ' + name + '?';
}

// 👍
function sayHi(name) {
  return `How are you, ${name}?`;
}
```

#### Arrow functions
Nel caso sia necessario utilizzare funzioni anonime (ad esempio in caso di callback inline) usiamo l'_arrow notation_.

```
// 👎
[1, 2, 3].map(function (x) {
  const y = x + 1
  return x * y
})

// 👍
[1, 2, 3].map((x) => {
  const y = x + 1
  return x * y
})
```
Vedi anche la sezione [this](#this)

#### this
Oltre che per le funzioni anonime, le arrow functions sono molto comode per manipolare l'oggetto `this` nelle nostre classi. Difatti in Javascript l'oggetto `this` si comporta in maniera inaspettata rispetto a linguaggi più rigidi. Su [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this) è disponibile una guida completa sull'utilizzo del this in Javascript. Per quanto riguarda lo scopo di questa guida, è sufficiente la seguente distinzione (semplificata):
- L'oggetto `this` all'interno di una funzione standard rappresenta il contesto da cui la funzione è stata chiamata
- L'oggetto `this` all'interno di un'arrow function rappresenta il contesto in cui la funzione è stata chiamata

Supponiamo di avere il sguente codice:
```js
class Test {
  x = 2

  getX() {
    return this.x
  }

  // Stessa funzione ma implementata come arrow function
  arrowGetX = () => this.x
}

class CallbackClass {
  x = 'pippo'
  callback = null

  constructor(cb) {
    this.callback = cb
  }

  callFunction() {
    return this.callback()
  }
}

const t = new Test()
const cb = new CallbackClass(t.getX)
console.log(cb.callFunction()) // Ritorna 'pippo', perchè il this si riferisce al contesto da cui è stata chiamata, ovvero la classe CallbackClass
const arrowCb = new CallbackClass(t.arrowGetX)
console.log(arrowCb.callFunction()) // Ritorna x, perchè il this si riferisce al contesto in cui è stata definita
```

In generale, il comportamento desiderato è quello delle arrow function e quindi conviene usarle sempre per implementare i metodi delle nostre classi. Unica eccezione vale per i metodi riferiti al lifecycle della classe (`constructor`, `componentDidMount`, `componentWillUnmount`, ...) che __non possono__ essere implementati con arrow function.

### ESLint
ESLint è un tool che monitora lo stile in cui il codice JS è stato scritto, segnalando (e fixando automaticamente) inconsistenze di stile o sezioni di codice poco leggibile. Nel [boilerplate tiknil](https://github.com/tiknil/react-native-boilerplate) è presente un file di configurazione (`.eslintrc.js`) che implementa le regole di sintassi elencate di seguito e le best practices consigliate da react native. Configurando l'IDE per utilizzare ESLint, ci si assicura che il codice scritto sia consistente con gli standard e facilmente leggibile.

🔔 TIP: puoi configurare l'IDE per fixare il file aperto con una semplice combinazione di tasti

### Altro
[Qui](http://es6-features.org/#Constants) è possibile vedere un recap di tutti gli improvements di ECMAScript 6+.

## IDE
Per lo sviluppo in react native sono stati selezionati 2 possibili strumenti, un editor e un IDE completo:
- **[Visual Studio Code](https://code.visualstudio.com/)** è stato identificato come il miglior **editor** in quanto rapido e molto completo. Da preferire rispetto ad altri editor quali Atom o Sublime Text
- **[WebStorm](https://www.jetbrains.com/webstorm/)** è un IDE e di conseguenza molto più pesante rispetto a Visual Studio Code. Tuttavia, presenta vari vantaggi quali una miglior integrazione con Flow, un miglior supporto all'autocomplete e al refactoring dei file (es. spostando un file vengono corretti tutti gli import di quel file nel progetto). **PHPStorm** è di fatto un estensione di WebStorm con miglior supporto a PHP ed è interscambiabile.
  
Sia Visual Studio Code che WebStorm/PHPStorm possono essere configurati per supportare comodamente le tecnologie utilizzate (Flow e ESLint)

## Tecnologie utilizzate
In un progetto *React Native* normalmente utilizziamo le seguenti tecnologie di supporto:

Tecnologia | Ruolo | Note
--- | --- | ---
**Yarn** | Dependency manager di *Node* | Consigliato l'utilizzo di Yarn al posto di npm in quanto più rapido nel download delle dipendenze e con un miglior supporto al file di lock.<br>Si può installare tramite npm con il comando `npm install -g yarn`
**Flow** | Static type checker | Permette di introdurre un minimo di *type-safety* in *JavaScript* tramite analisi statica del codice.<br>Gli [IDE](#ide) indicati supportano un plugin di flow che permette di vedere inconsistenze di flow come errori e warnings all'interno del codice. **Non** ha alcun effetto a runtime.
**[Moment](https://momentjs.com/)** | Gestore date e orari | Libreria JS per parsing, validazione, manipolazione e visualizzazione di date e orari.
**[React Native](#react-native)** | Framework Mobile | Framework crossplatform con output mobile nativo basato su [React JS](https://reactjs.org/).
**[Redux](#redux)** | State container | Libreria JS che agevola l'applicazione di un pattern di sviluppo che permette di realizzare codice più testabile e consistente su diverse piattaforme.
**[Immutable](#immutable)** | Dati immutabili | Libreria JS che permette di generare collezioni di dati immutabili.<br>Particolarmente utile in combinazione con lo *state* di React perché permette di evitare inutili refresh di componenti quando non necessario.

## Struttura del progetto
La struttura del progetto è organizzata come segue:

- `android/` Contiene il progetto android e tutto il codice android nativo. Cartella da aprire con Android Studio
- `ios/` Contiene il progetto ios, il codice nativo e il podfile per l'installazione delle dipendenze. Da aprire con Xcode
- `flow-typed/` Contiene file javascript che vengono utilizzati da flow per rilevare dei Tipi utilizzabili all'interno del codice js senza necessità di import
- `src/` Contiene tutto il codice javascript:
  
  - `assets/` contiene gli asset statici usati dal codice javascript (immagini, fonts, ecc)
  - `components/` contiene component React riutilizzabili sul codice. Idealmente dovrebbero essere dump components, che si limitano a comporre il compomente grafico sulla base delle proprietà ricevute e propagano al padre eventi e interazioni dell'utente.
  
    - `template.js` questo file contiene il boilerplate di un componente tipico, da usare come punto di partenza per gli altri componenti
  - `ducks/` Implementazioni dei reducer e delle azioni redux seguendo lo standard dei ducks: https://github.com/erikras/ducks-modular-redux
  - `networking/` Implementazione delle chiamate HTTP
  - `screens/` 
  
    - `[screen-name]`

      - `index.js` Mapping sullo state Redux e dispatch delle azioni necessarie alla schermata
      - `component.js` Implementazione del componente stesso
      - `components/` (opzionale) componenti usati solo da questa schermata
    - `setup.js` Definizione della navigazione tra le schermate (es. creazione di navigators in caso di utilizzo di [`react-navigation`](https://reactnavigation.org/))
  - `translations/` Contiene i file json delle stringhe localizzate su varie lingue (es. `it.json`, `en.json`) 
  - `utils/` funzioni javascript utilizzate in più punti dell'app. In questa cartella NON vanno inseriti componenti react.
  - `i18n.js` file che esporta l'oggetto da utilizzare per ottenere le stringhe localizzate
  - `index.js` Entry point dell'app: inizializza redux ed effettua il render del root component
  - `redux-setup.js` Inizializzazione di redux, chiamato da `src/index.js`  
  -  `styles.js` Esporta un oggetto contente lo stile grafico (font, margini, colori..) dell'app


## React Native
coming soon

## Redux
coming soon

## Immutable
coming soon

## Navigation
coming soon

## Persistenza dei dati
coming soon

## Test
coming soon
