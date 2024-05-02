---
title: "Advanced Typescript"
description: "Lavorare in JavaScript con i vantaggi offerti dalla tipizzazione forte."
date: 2024-04-20T14:17:01+02:00
draft: false
ShowToc: true
tags: ["typescript"]
image: "posts/advanced-typescript/images/typescript.webp"
cover:
  image: "posts/advanced-typescript/images/typescript.webp"
  caption:
---

<!-- 
  https://medium.com/@mvsg/6-advanced-typescript-tricks-for-clean-code-90cee774dbf3 
  https://www.freecodecamp.org/news/advanced-typescript-types-cheat-sheet-with-examples/
  https://www.typescriptlang.org/docs/handbook/declaration-merging.html
-->

## 0. Breve Introduzione
TypeScript è un linguaggio di programmazione che nasce come superset di JavaScript. Questo significa che comprende tutta la sintassi e le funzionalità di JavaScript, aggiungendo però ulteriori caratteristiche. Il principale valore aggiunto di TypeScript rispetto a Js è la tipizzazione statica, cioè il controllo dei tipi al momento della compilazione del codice. Il controllo dei tipi è il modo in cui un linguaggio di programmazione conferma che tutte le operazioni hanno ricevuto il tipo di dati corretto per eseguire l'operazione.

I tipi forniti da Typescript sono: 
- numero, stringa, booleano, bigint, simbolo, oggetto e funzione
- any: può essere usato per escludere il controllo dei tipi.
- null e undefined: tipi primitivi che rappresentano l'assenza di qualsiasi valore di un oggetto.
- Array<T>: un array di valori di tipo T.
- Tuple<T>: una tupla di valori di tipo T1, T2 e così via.

Una volta scritto il codice e definiti i tipi appropriati, l'applicazione TypeScript viene tradotta da un compilatore (transpiler) in un'applicazione JavaScript standard eseguibile su qualsiasi engine.

---

## 1. Tipi Avanzati
È possibile costruire nuovi tipi basati su quelli esistenti, sfruttando i tipi avanzati di TypeScript. Con l'aiuto di questi tipi, si possono modificare e manipolare i tipi in modi forti, dando al codice maggiore flessibilità e manutenibilità.

- **Tipi Mappati**

I tipi mappati iterano sulle proprietà di un tipo esistente e applicano una trasformazione per creare un nuovo tipo. Un caso d'uso comune è quello di creare una versione di sola lettura di un tipo.

```ts
type Readonly<T> = {
  readonly [P in keyof T]: T[P];
};

interface Point {
  x: number;
  y: number;
}

type ReadonlyPoint = Readonly<Point>;
```

- **Tipi Condizionali**

I tipi condizionali consentono di creare un nuovo tipo in base a una condizione. La sintassi è simile a quella dell'operatore ternario, utilizzando la parola chiave _extends_ come vincolo di tipo.

```ts
type NonNullable<T> = T extends null | undefined ? never : T;
```

---

## 2. Decoratori
I decoratori in TypeScript sono una potente funzione che consente di aggiungere metadati, modificare o estendere il comportamento di classi, metodi, proprietà e parametri. Sono funzioni di ordine superiore che possono essere usate per osservare, modificare o sostituire definizioni di classi, metodi, accessiori, proprietà o parametri.

- **Esempio di decoratore di classe**

```ts
function LogClass(target: Function) {
  console.log(`Class ${target.name} was defined.`);
}

@LogClass
class MyClass {
  constructor() {}
}
```

- **Esempio di decoratore di proprietà**

```ts
function DefaultValue(value: any) {
  return (target: any, key: string) => {
    target[key] = value;
  };
}

class MyClass {
  @DefaultValue(42)
  myProperty: number;
}

const instance = new MyClass();
console.log(instance.myProperty); // Output: 42
```

- **Esempio di decoratore di parametro**

```ts
function LogParameter(target: any, key: string, parameterIndex: number) {
  console.log(
    `Parameter at index ${parameterIndex} of method ${key} was called.`
  );
}

class MyClass {
  myMethod(@LogParameter value: number) {
    console.log(`Inside myMethod with value ${value}.`);
  }
}

const instance = new MyClass();
instance.myMethod(5);
```

---

## 3. Spazi dei nomi (Namespaces)
I Namespaces in TypeScript sono un modo per organizzare e raggruppare il codice correlato. Aiutano a evitare collisioni di nomi e promuovono la modularità incapsulando il codice che appartiene allo stesso gruppo. I namespace possono contenere classi, interfacce, funzioni, variabili e altri namespace.

- **Definizione**

Per definire uno spazio dei nomi, si usa la parola chiave _namespace_ seguita dal nome dello spazio dei nomi. Si può poi aggiungere il codice relativo all'interno delle parentesi graffe.
```ts
namespace MyNamespace {
  export class MyClass {
    constructor(public value: number) {}

    displayValue() {
      console.log(`The value is: ${this.value}`);
    }
  }
}
```

- **Utilizzo**

Per usare il codice di uno spazio dei nomi, si può usare il nome completamente qualificato o importare il codice usando un'importazione dello spazio dei nomi.
```js
// Using the fully-qualified name
const instance1 = new MyNamespace.MyClass(5);
instance1.displayValue(); // Output: The value is: 5

// Using a namespace import
import MyClass = MyNamespace.MyClass;

const instance2 = new MyClass(10);
instance2.displayValue(); // Output: The value is: 10
```

- **Namespaces Annidati**

I namespaces possono essere annidati per creare una gerarchia e organizzare ulteriormente il codice.
```ts
namespace OuterNamespace {
  export namespace InnerNamespace {
    export class MyClass {
      constructor(public value: number) {}

      displayValue() {
        console.log(`The value is: ${this.value}`);
      }
    }
  }
}

// Using the fully-qualified name
const instance = new OuterNamespace.InnerNamespace.MyClass(15);
instance.displayValue(); // Output: The value is: 15
```

---

## 4. Mixins
I mixin in TypeScript sono un modo per comporre le classi da parti più piccole, chiamate _classi mixin_. Permettono di riutilizzare e condividere il comportamento di classi diverse, promuovendo la modularità e la riusabilità del codice.

- **Definizione**

Per definire una classe mixin, creare una classe che estenda un parametro di tipo generico con una firma del costruttore. In questo modo, la classe mixin può essere combinata con altre classi.
```ts
class TimestampMixin<TBase extends new (...args: any[]) => any>(Base: TBase) {
  constructor(...args: any[]) {
    super(...args);
  }

  getTimestamp() {
    return new Date();
  }
}
```

- **Utilizzo**

Per usare una classe mixin, si definisce una classe base e si applica la classe mixin ad essa usando la parola chiave extends.
```ts
class MyBaseClass {
  constructor(public value: number) {}

  displayValue() {
    console.log(`The value is: ${this.value}`);
  }
}

class MyMixedClass extends TimestampMixin(MyBaseClass) {
  constructor(value: number) {
    super(value);
  }
}
```

In questo esempio, definiamo una classe base chiamata MyBaseClass con un metodo displayValue. Si crea poi una nuova classe chiamata MyMixedClass, che estende la classe base e vi applica la classe mixin TimestampMixin.

Vediamo come funziona in pratica la classe mixin.

```ts
const instance = new MyMixedClass(42);
instance.displayValue(); // Output: The value is: 42

const timestamp = instance.getTimestamp();
console.log(`The timestamp is: ${timestamp}`); // Output: The timestamp is: [current date and time]
```

In questo esempio, creiamo un'istanza della classe MyMixedClass, che include sia il metodo displayValue della MyBaseClass sia il metodo getTimestamp della classe mixin TimestampMixin. Chiamiamo quindi entrambi i metodi e visualizziamo i loro risultati.

---

## 5. Type Guards
I type guards in TypeScript sono un modo per restringere il tipo di una variabile o di un parametro all'interno di uno specifico blocco di codice. Consentono di distinguere tra diversi tipi e di accedere a proprietà o metodi specifici per tali tipi, promuovendo la sicurezza dei tipi e riducendo la probabilità di errori di runtime.

- **Definizione**

Per definire una protezione di tipo, creare una funzione che accetta una variabile o un parametro e restituisce un predicato di tipo. Un predicato di tipo è un'espressione booleana che restringe il tipo del parametro nell'ambito della funzione.
```ts
function isString(value: any): value is string {
  return typeof value === "string";
}
```

- **Utilizzo**

Per utilizzare una protezione di tipo, è sufficiente richiamare la funzione di protezione di tipo in un'istruzione condizionale, come un'istruzione if o un'istruzione switch.
```ts
function processValue(value: string | number) {
  if (isString(value)) {
    console.log(`The length of the string is: ${value.length}`);
  } else {
    console.log(`The square of the number is: ${value * value}`);
  }
}
```

---

## 6. Utility Types
Gli _utility types_ in TypeScript forniscono un modo pratico per trasformare i tipi esistenti in nuovi tipi. Permettono di creare tipi più complessi e flessibili senza doverli definire da zero, favorendo la riusabilità del codice e la sicurezza dei tipi.

- **Utilizzo**

Per usare un tipo di utilità, si deve applicare il tipo di utilità a un tipo esistente, usando la sintassi delle parentesi angolari. TypeScript fornisce una serie di tipi di utilità incorporati, come _Partial_, _Readonly_, _Pick_ e _Omit_.

```ts
interface Person {
  name: string;
  age: number;
  email: string;
}

type PartialPerson = Partial<Person>;
type ReadonlyPerson = Readonly<Person>;
type NameAndAge = Pick<Person, "name" | "age">;
type WithoutEmail = Omit<Person, "email">;
```

<!-- USEFUL STUFF -->
<!-- NOTE: Put the audio files in the same dir of index.md -->
<!-- {{<audio img-src="images/<COVER_IMAGE>" src="posts/<POST_NAME>/<AUDIO_NAME>" width="100%" caption="<AUDIO_NAME>" >}} -->

<!-- {{< youtube "<YOUTUBE_VID_ID>" >}} -->
