# react-native-style-guide
Guida di riferimento per i progetti React Native gestiti da Tiknil e i suoi collaboratori.

L'obiettivo è darsi delle **best practices** sulla stesura del codice per agevolare il lavoro in team e velocizzare la comprensione del codice.

## Riferimenti
Alcuni contenuti di questa guida sono ispirati dai seguenti interessanti articoli di settore:

* *[AirBnb JavaScript Style Guide](https://github.com/airbnb/javascript)*: ottima linea guida per la stesura del codice JavaScript creata da *AirBnb* nel periodo in cui ha investito molto su React Native.

## Sommario
* [Prerequisiti](#prerequisiti)
* [Stile della sintassi](#stile-della-sintassi)
* [IDE](#ide)
* [Tecnologie utilizzate](#tecnologie-utilizzate)
* [Struttura del progetto](#struttura-del-progetto)
* [React Native](#react-native)
* [Redux](#redux)
* [ImmutableJS](#immutablejs)
* [Navigation](#navigation)
* [API](#api)
* [Persistenza dei dati](#persistenza-dei-dati)

## Prerequisiti
Per realizzare un'applicazione in **React Native** è (ovviamente) un prerequisito fondamentale la conoscenza di **JavaScript**.

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
  pop() {
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

[Qui](http://es6-features.org/#Constants) è possibile vedere un recap di tutti gli improvements di ECMAScript 6+.

## IDE
coming soon

## Tecnologie utilizzate
coming soon

## Struttura del progetto
coming soon

## React Native
coming soon

## Redux
coming soon

## ImmutableJS
coming soon

## Navigation
coming soon

## Persistenza dei dati
coming soon

## Test
coming soon
