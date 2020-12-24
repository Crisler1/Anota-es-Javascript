#  Javascript: breves anotações sobre a linguagem

  Lista pessoal de anotações e truques 

  ## Tabela de Conteúdo
- [Versões do Javascript](#versões-do-javascript)
- [Declaração de variáveis](#declaração-de-variáveis)
- [Operadores](#operadores)
- [Condicionais](#condicionais)
- [Funções, Parâmetros e Argumentos](#funções-parâmetros-e-argumentos)
- [Classes em ES6](#classes-em-es6)
- [Classes em ES2019](#classes-em-es2019)
- [Singleton (ES6 classe)](#singleton-es6-classe)
- [Tratamento de Arrays](#tratamento-de-arrays)
- [Tratamento de Strings](#tratamento-de-strings)
- [Expressões Regulares](#expressões-regulares)
- [Objetos](#objetos)
- [Namespaces](#namespaces)
- [Closures](#closures)
- [Proxies](#proxies)
- [Set](#set)
- [Map](#map)
- [Debugging e Console](#debugging-e-console)
- [De Callback a Promises](#de-callback-a-promises)
- [Promises](#promises)
- [Async-Await](#async-await)
- [setTimeOut](#setTimeOut)
- [DOM](#dom)


-----------------------------------------------
## Versões do Javascript

![js_version.png](./assets/js_version.png)

## **Compatibilidade com NodeJS**
[https://node.green](https://node.green)

------------------------------------------------

## Declaração de Variáveis

* Declarar multiváriaveis de maneira mais compacta
```javascript
var i, j, k;
var l = m = n = 0;
var a = 1, b = 2;
```
--------------------------------------------------

## Operadores:

* O operador lógico _OR_ (avalia da esquerda para a direita as expressões e retorna a primeira delas como "truthy".
Se todas forem avaliadas como "falsy", retorna _false_. Todos os valores são "truthy" exceto: _false_, _0_ _""_, _null_,
_undefined_, e _NAN_)
```javascript
var foo = false || '🙂'; // '🙂'

var bar = 3 || false; // 3

var foobar = null || 0 || undefined || "NyanCat"; // "Nyancat"
```

* Operador Ternário
```javascript
var now = new Date();
var greeting = "Good" + ((now.getHours() > 17) ? "evening." : "day");
console.log(greeting);
```
* Uso do operador _!_ (not):
```javascript
// A dupla negação retorna um booleano dependendo da "truthiness" da expressão.
// Retornará false quando o valor é: false, 0, null, undefined e NaN.
// Se não for nenhum desses casos, então será retornado true.
// Tem sentido usar-lo para avaliar expressões que não são booleanas.
let foo;
if (!!foo) { // 'Result is false' dado que o valor de foo é null
  console.log('Result is true');
} else {
  console.log('Result is false');
}

// Observe os seguintes exemplos:
!!0 // false
!!1 // true
!!"" // false (strings vazias são falsy)
!!window.biz // false(se a variável biz não está declarada, se avalia como falsy)
!!undefined // false(undefined é falsy)
!!null // false (null é falsy)
!!{} // true(um objeto vázio é truthy)
!![] // true(um array vázio é truthuy. PHP programmers beware!)
!!NaN // false (NaN é falsy)
```

* Operador de coalescência nula (Nullish coalescing operator(??)):
```javascript
// Node.js 14 required (ES2020)

0 ?? 'default' // 0
'' ?? 'default' // ''
false ?? 'default' // false
NaN ?? 'default' // NaN
null ?? 'default' // 'default'
undefined ?? 'default' // 'default'
undefined ?? null // null
null ?? undefined // undefined
```
* Operador de encadeamento opcional (optional chaining operator(?.)):
```javascript
// Node.js 14 required (ES2020)
const object = {id: 123, names:{first: 'Alice', last: 'Smith'}};
const firstName = object?.names?.first; // 'Alice'
const home = object?.adress?.home; // undefined

// Em combinação com nullish coalescing operator: 
const middleName = object?.names?.middle ?? '(no middle name)'; // '(no middle name)'

const user = {};
alert(user.address.street); // Error!
alert(user && user.address && user.address.street ); // undefined (no error)
alert( user?.address?.street); // undefined (no error)


// A avaliação para imediatamente after: car ?.
const car = null;
alert(car?.motor.valves.anything); // undefined
```

----------------------------------------------------------------------------------------------

## Condicionais:

* Condicional _if_ comparando multiplos critérios
```javascript
const fruit = 'strawberry';

// Criamos um array com aqueles critérios para compara-los
const criteria = ['apple', 'strawberry', 'cranberries'];

if (criteria.include(fruit)) {
  // true
}
```

-------------------------------------------------------------------------------------------

# Funções, parâmetros e argumentos:

* Parâmetros vs argumentos. Um  parâmetro é uma variável na declaração da função. Um parâmetro é o valor que é passado 
em uma função no momento dela ser chamada.
```javascript
function sayHi(name) { // 👈 parâmetro
}

sayHi('samantha'): // 👈 argumento
```

* _arguments_ (array de argumentos que podem ser passados a uma função):
```javascript
function printArgumentES6(...arg){
  arg.forEach((a) => console.log(a));
}
printArgumentsES6() {
  for(let argument of arguments) {
    console.log(argument);
  }
}
printArgumentsES6ForOf(3712,7123);

function food(...arg){
  const [egg, cheese] = args;
}

food'🥚', '🧀');
```
* Uso de destructuring para melhorar a legibilidade do significado dos parâmetros que mandamos em uma função:
```javascript
function createMenu({title, body, buttonText, denyable= false}){
  // ...
  // Observe que estabelecemos um valor default para o argumento "cancellable", 
}

createMenu({
  title: 'my title',
  body: 'my body',
  buttonText: 'send form',
  cancellable: true
}); // Observa que é totalmente compreensivel o significado dos parâmetros. Isto chamamos de 'named paramaters'.
    // Sem embargo, não teriamos entendido nada se: createMenu('my title', 'my body', 'send form', true);
    // Saberiamos entender o que significa em true na segunda chamada? Provavelmente não!

    

// Podemos fazer o seguinte se todos os parâmetros forem opcionais:
function createUser({username = 'Anonymous'} = {}) {
//....
};

// Observa que se nossas funções tambêm retornam objetos, esta usando o padrão RORO (receive object, return object).
createUser();



```


* Self Invoking Functions, tambêm chamadas IIFE (Immediately Function Expression):
```javascript
var hello;
(function(){

  hello = function(){
    return "Hi boy!";
  };
})();

console.log(hello() ); // "Hi boy!"

var result = (function(){
  var name = "Barry";
  return name;
})();

console.log(result); // O contéudo da variável result é o resultado da execução da função.
// Neste caso: "Barry"
```


* Factory Functions (Uma "Factory Function" é uma função que cria objetos e os retorna):
```javascript 
// Exemplo de Factory Function
const cat = () => {

  const sound = 'miau',
  let color = 'white';


//  definimos getters and setters para acessar as propriedades
  const setColor = (newColor) => {
    color = newColor;
  }
  const getColor = (newColor) => {
    return color;
  }

  return {
    talk: () => sound,
    setColor, 
    getColor
  }
}

const smurfy = cat(); // No deve usar "new"!
console.log( smurfy.talk() ); // miau
//console.log( smurfy instanceof cat); // TypeError
console.log( smurfy.getColor() ); // 'white'
smurfy.setColor('black');
console.log( smurfy.getColor() ); // 'black'


// Outro exemplo potenciando o uso de "closures"
const makeCounter = function() {
// Propriedades Privadas
  let _count = 0;


// Métodos Privados
  const changeBy = (val) => {
    _count += val;
  }


// Definimos os Métodos Publicos
  return Object.freeze({
    increment: function(){
      changeBy(1);
    },
    decrement: function(){
      changeBy(-1);
    },
    getValue: function(){
      return _count;
    }
  })
};

const Counter1 = makeCounter();
const Counter2 = makeCounter();

console.log(Counter1.getValue()); // 0

Counter1.increment();
Counter2.increment();
console.log(Counter1.getValue()); // 2

Counter1.decrement();
console.log(Counter1.getValue()); // 1

console.log(Counter2.getValue()); // 0

```

Outro exemplo de _Factory Functions_ mais avançado:
```javascript
// Exemplo avançado de Factory Function
const dataConnection = (data = {}) => {
  const settings = data; // Armazena os parâmetros que são passados para chamar a Factory Function

  return Object.freeze({
    // Object.freeze evita a eliminação e a adição de propriedades, modificação do prototype, etc.
    getSettings: () => settings,

    modifySettings: (addData = {}) => {
      return Object.assign(settings, addData);
    // Adiciomos ao objeto 'settings' novas propriedades
    // Ou modificamos o valor das existentes
    }
  });
} 

const connection = dataConnection({ ip: '127.0.0.1', port: '8080'});
// Passo o parâmetro como um objeto

console.log(connection.getSettings()); // {ip: "127.0.0.1", port: "80-80"}
console.log(connection.getSettings().ip);// 127.0.0.1

connection.method = 'http'; /* Isto não funcinará!
  Graças a 'object.freeze' não podemos acessar a propriedade do objeto
  Só verá um erro se usar: 'use strict';

  connection.settings.method = 'http'; => Isso não funcionará! TypeError

*/


connection.modifySettings({ method: 'http'});
// Usamos um métodos definidos no objeto para modificar os valores que contém no objeto.
console.log(connection.getSettings());

connection.modifySettings({ip: '192.168.2.11'}); 
// Podemos sobreescrever os valores do objeto 'settings'.
console.log(connection.getSettings()); // {ip: "192.168.2.11", port: "8080", method: "http"}
```

---------------------------------------------------

## Classes em ES6

* Exemplo de classes em ES6:
```javascript
class Person {
  constructor(name) {
    // constructor  é um método opcional. Se a classe não tem propriedade pode ser omitido.
    this.name = name;
  }

  greeting() {
    console.log(`Hello, my name is ${this.name}`);
  }
}

let john_doe = new Person("John Doe");

john_doe.greeting(); // Hello, my name is John Doe

console.log(john_doe instanceof Person); // true
console.log(john_doe instanceof Object); // true

console.log( typeof john_doe); // Objeto

console.log( typeof Person); // Função!!
// (Cuidado, poderiamos pensar que não retornará "class" porem nos retorna "function")
console.log( typeof Person.prototype.greeting); // Função !! 
// (Cuidado, poderiamos pensar que não retornará "method" porém nos retorna "function")

```

* Exemplo de Chaining Methods (métodos encadeados):
```javascript
class Car {
  constructor(brand, model, color) {
    this.brand = brand;
    this.model = model;
    this.color = color;
  }

  setBrand(brand){
    this.brand = brand;
    return this; // Retornando this por encadeamento
  }
  setModel(model){
    this.model = model;
    return this; // Retornando this por encadeamento
  }
  setColor(color){
    this.color = color;
    return this; // Retornando this por encadeamento
  }

  getCarInfo(){
    return this; // Retornando this por encadeamento
  }
}

const car = new Car('Ford', 'F-150','red').setColor('blue').setModel('Ranger'); // Métodos encadeados

console.log( car.getCarInfo()); // {brand:"Ford", model:"Fiesta", color:"Purple"}
```

* Métodos estáticos (nos permite usar esses métodos sem gerar a instância da classe):
```javascript
class Utilities {
  static generateRandomInteger() {
    // Isto permite um método estático.
    // Permite se chamado sem necessidade de instanciar a classe.
  return Math.floor(Math.random() * 11); // retorna número inteiro aleátorio entre 0 e 10
  }

  static coinToss(){
    return Math.random() > .5; // Retorna 'true' ou 'false' de maneira aleátoria
    }
  static capitalize(str) {
    return str.charAt(0).toUpperCase() + str.slice(1); // retorna uma string colocando a primeira letra maiúscula
  }

  static delay(miliseconds, callback){
    setTimeout(callback, miliseconds);
  }

  static delay(miliseconds, callback){
    setTimeout(callback, miliseconds);
  }

  static pause(miliseconds) {
    const start = Date.now();
    let now = start;
    while(now - start < miliseconds) {
      now = Date.now();
    }
  }

  static wait(timeout){
    return new Promise(resolve => setTimeout(resolve, timeout));
  }

  static generateToken(){
    const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789_-$';
    let rand, i;
    let bits = 64;
    let result = '';
    while (bits > 0){
      rand = Math.floor(Math.random() * 0x100000000); // 32-bit integer
      // base 64 means 6 bits per character, so we use the top 30 bits from rand to give 30/6=5 characters.
      for (i = 26; i > 0 && bits > 0; i -= 6, bits -= 6) result += chars[0x3f & rand >>> i];
    }
    return result;
  }
}

let num = Utilitie.generateRandomInteger();
console.log(num); // 2

console.log(Utilities.coinToss()); // true | false

console.log(Utilities.capitalize('batman')); // 'Batman'

Utilities.delay(3000, function (){
  return console.log('delayed output');
}); // 'delayed output', três segundos depois

Utilities.generateToken(); // 'DyvlJ_mhQ8X'

Utilities.pause(2000); // Stop the execution of javascipt for the specified time

// wait() example:
Utilities.wait(5000).then(()=> doSomething());

// or even....
await Utilities.wait(5000);
doSomething();
```
* Herança
```javascript
class Rectangle {
  constructor(len, wid) {
    this.len = len;
    this.wid = wid;
  }

  getArea(){
    return this.len * this.wid;
  }

  getInfo () {
    return 'I am a naughty rectangle';
  }
}

class Square extends Rectangle { // Extendemos a classe
  constructor(len){
    super(len, len); // Invocamos ao constructor a classe pai

  // Podemos fazer referencia ao metodo pai, mas não é necessário!
  // Pode omiti-lo, ja que o método é herdado automaticamente.
  /* 
  getArea() {
  return super.getArea(); // avisa que assim que invoco a um determinado metodo do pai

  }
  */
// Sobrescrever um método
getInfo() {
  return 'I am a cheeky square!';
  }
 }

let square = new Square(4);
console.log(square.getArea()); // 16. Observa que este método foi herdado da classe pai

console.log(square instanceof Square); // true
console.log(square instanceof Rectangle); // true. Lembre-se que Square extende a classe Rectangle

console.log(square.getInfo()); // I am a cheeky square!
```

------------------------------------------

## Classes em  ES2019

* exemplo de classe em ES2019:
```javascript
// Node.js 12.4 required

class MyES2019Class {
  // Não é necessário o constructor para declarar as propriedades públicas (porém podem usar se necessário).

  x = 1; // public property

  #counter = 0; // private property

  static z = 3; // static property

  incB() {
    this.#counter++;
  }
  getCounter () {
    return this.#counter
  }
}

  let foo = new MyES2019Class();

  foo.incB(); // Runs OK
  foo.#counter = 7; // Error - private property cannot be modified outside class
  foo.#counter; // Error - private property cannot read outside class
  foo.counter; // undefined

  foo.getCounter(); // 12
  ```

  ----------------------------------------------------------------------------------------

  ## Singleton (Classe ES6):

  * Exemplo de singleton pattern usando classes de ES6:
  ```javascript
  class Item {
    constructor() {
      if(!Item.instance){
        this._data = [];
        Item.instance = this;
      }
      return Object.freeze(Item.instance);
    }

    // Metódos
    add(Item){
      this._data.push(item);
    }

    get(id){
      return this._data.find(d => d.id === id);
    }
  }
  // Pode-se exportar esta classe como módulo:
  module.exports = Item;

  // Importe-o assim:
  const Item = require('./Item');
  const foo = new Item();

  let first = new Item();
  let second = new Item();

  first.add({id: 1, name:'John'});
  second.add({id: 2, name:'Louis'});

  console.log(second); // [{id: 1, name: 'John'}, {id:2, name: 'Louis'}]

  console.log(second.get(2)); // {id: 2, name: 'Louis'}

  console.log(first === second); // true (São a mesma instância)
  ```

-----------------------------------------------------------------------

## Tratamento de Arrays

* Criar um array com _n_ número de posições com valor por default
```javascript
new Array(3).fill(0); // [0,0,0]
```

* Clonar um array
```javascript

/**
* recursiveArrayClone() Cria um clone exato de um array. Suporta matrizes multidimensionais.
* 
* @code
* console.log( recursiveArrayClone(['a', {}, [4, 0]])); // ['a', {} [4, 0]]
* @endcode
* 
* @param {array} arrayToClone. O array que se quer clonar.
* @return {array} clonedArray. Retorna um novo array.
*/
function recursiveArrayClone(arrayToClone){
  let clonedArray = [];
  for (let i = 0; i < arrayToClone.length; i++) {
    if ( Array.isArray(arrayToClone[i])) {
      clonedArray.push(arrayToClone[i]);
    }
  }
  return clonedArray;
} 
```

* Esvaziar um array
```javascript
const myArray = ['a','b','c'];
myArray.length = 0;
console.log(myArray); // []
```

* Retirar elementos _falsy_ de um array
```javascript
const myArray = [0, "0", "example", null, undefined, NaN, false];

let arrayFiltered = myArray.filter(Boolean);
console.log(arrayFiltered); // ["0", "example"]
```

* Uso de: _array.join_ (retorna uma string)
```javascript
let a = ['Wind', 'Rain', 'Fire'];
a.join(); // 'Wind','Rain','Fire'
a.join(''); // 'WindRainFire'

let text = "add the potato please, I always prefer more potato" ;
text = text.split("potato").join("cheese");
// Substitui "potato" por "cheese" em todas as aparições que houver em "text"
```
* Uso de: _array.concat_ (Cria um novo array com a concatenação de 2 ou mais arrays)
```javascript
const arr1 = ['a','b','c'];
const arr2 = ['d','e','f'];
const arr3 = arr1.concat(arr2); // arr3 is a new array ['a','b','c','d','e','f']
```
* Uso de: _array.slice_(cria uma cópia do array, porém retirando alguns elementos dos originais)
```javascript
var fruits = ['Banana','Orange','Lemon','Apple','Mango'];
var citrus = fruits.slice(1, 3); // citrus == ['Orange','Lemon']


var reduced_fruits = fruits.splice(1); // reduced_fruits == ['Orange','Lemon','Apple','Mango']
```

* Uso de _array.splice_ (permite acessar ou eliminar elementos de um array. Tambem é útil para dividir um array em dois)
```javascript
var instruments = ['guitar', 'bass', 'keyboard'];
instruments.splice(2,0, 'drums') // instruments = ['guitar','bass','drums','keyboard']

var planets = ['Mercury','Venus','Earth','Mars','Pluto','Jupiter','Saturn','Uranus','Neptune'];
planets.splice(4,1);
// (sorry, Pluto) planets == ['Mercury','Venus','Earth','Mars','Jupiter','Saturn',''Uranus','Neptune'];

array_example.splice(0, 0, "foo"); // Acessa "foo" na primeira posição de um array_example


var myFish = ['Angel','Clown','Mandarin','Sturgeon',"CherubFish"];
var removedFish = myFish.splice(2);
console.log(myFish); //['Angel','Clown']
console.log(removedFish); // ['Mandarin','Sturgeon','CherubFish']
```

* Uso de: _array.indexOf_(busca um determinado elemento de um array)
```javascript
var a = ['foo','bar','biz'];
a.indexOf('bar'); // 1
a.indexOf('NyanCat'); // -11

if (a.indexOf('NyanCat') === -1) {
  // element doesn't exists in array
}
```

* Uso de: _array.filter_ (cria um novo array apartir de outro com aqueles elementos que passam um filtro especificado)
```javascript
function isBigEnough(value) {
  return >= 10;
}

const numbers = [12, 5, 8, 130, 44];

const filteredNumbers = numbers.filter(isBigEnough); // filteredNumbers is [12, 130, 44]
```

* Uso de: _array.forEach_ (recorre ao array aplicando uma função a cada elemento)
```javascript
var a = ['a','b','c'];
a.forEach(function(element){
  console.log(element);
});

// Outra maneira de implementa-lo:
function fooFunction(currentValue,index,array) {
  //...
}
a.forEach(fooFunction, this); // Podemos nomear a função em vez de declarar dentro do forEach().
// Ocasionalmente, podemos passar o objeto 'this' a função.
```

* Uso de: _array.reduce_ (aplica uma função a um acumulador e a cada valor de um array(de esquerda a direita) para reduzir-lo a um único valor.)
```javascript
const cart = [{ref:'yuh',price: 23,},{ref:'jum',price: 44}];
const result = cart.reduce(function(accum, v){
  return accum + v.price;
},0); // Neste caso, e 0 é o acumulador.
console.log(result); // 67

const arrayOfBooleans = [true,false,true,true];
const totalOfTrues = arrayOfBooleans.reduce(function(accumulator, arrayElement){
  return arrayElement ? ++accumulator : accumulator;
},0
);
console.log(totalOfTrues); // 3
```

--------------------------------------------------------------------
## Tratamento de Strings:


* Uso de: _string.match_ (cria um array apartir de comparar uma string com uma regular expression)
```javascript
var str = "The rain in SPAIN stays mainly in the plain";
var res = str.match(/ain/gi); // ["ain","AIN","ain","ain"]

var text = "The phone number is 93 555 22 33";
var result = text.match(/\d+/); // ["93"]
var allResults = text.match(/\d+/g); // allResults is ["93", "555", "22", "33"]
var numberOfResults = text.match(/\d+/g).lenght; // 4

var text = "<p> Hello {{name}}, you are {{age}} years old. </p>";
var extract = text.match(/\{\{(.+?)\}\}/g);
console.log(extract); // ["{{name}}","{{age}}"]
```

* Diferenças entre _string.substr_ e _string.substring_ (ambos métodos retornam uma nova string apartir do original,
porém observa a diferença dos resultados):
```javascript
var text = "example";
console.log(text.substr(1,5)); // "xampl"
// (a partir do indice dado, retorna tantos caracteres quanto aponte no segundo parâmetro)


console.log(text.substring(1,5)); // "xamp"
// (a partir do índice dado, retorna caracteres apartir de outro indice dado)
```

* Uso de: _string.split_ (genera um novo array apartir de cortar uma string pelo parâmetro indicado):
```javascript
var array_of_words = "A new car".split(" ");
console.log(array_of_words) ; // ["A", "new", "car"]

console.log("hello".split("")); // ["h","e","l","l","o"]

var text = "Add the potato please, I always prefer more potato";
text = text.split("potato").join("cheese");
// Substitui "potato" por "cheese" em todas as aparições que há "text"
```

------------------------------------------------------------

## Regular Expression

* Uso de: _regexObj.search_ (retorna um booleano se encontra uma coincidencia na string dado)

```javascript
var str = "The best things in life are free";
var pattern = new RegExp(/life/g);
var res = pattern.test(str); // true
```
-------------------------------------------------------------

## objetos:

* Contexto the _this_:
```javascript
var biz = {
  name: "Crisler",
  "full-Name": "Crisler Wintler" // Compilado contendo um hífen


// error, "this" aponta para um objeto global:
greetings: 'Hello, I am' + this.name,
 
// correto, "this" aponta a "biz":

introduce: function(){
  return 'My name is' + this.name;
}
get FullName() { return this["fullName"]}
}

console.log(biz.greetings); // 'Hello, I am undefined'
console.log(biz.introduce()) ; // 'My name is Crisler'

console.log(biz.full-name); // Error!
console.log(biz["full-name"]); "Crisler Wintler"

delete biz["full-name"]; // Sempre retorna true! Inclusive quando existe a propriedade apagar
```

* Obter um array a partir das propriedades de um objeto:
```javascript
var foo = {
  name: 'Crisler',
  nick: 'Bear'
}

var arrayOfProperties = Object.keys(foo);
console.log(arrayOfProperties); ['name', 'nick']
```

* Criar um array apartir de um objeto:
```javascript
var biz = {
  length: 2,
  '0': 'biz',
  '1': 'foo'
}

var arrayFromObject = Array.prototype.slice.call(biz);
console.log(arrayFromObject); // ['biz','foo']
console.log(arrayFromObject.length);* // 2
console.log(arrayFromObject[1]); // 'foo'
```

* Clonar um objeto:
```javascript
function deepClone(originalObject) {
  const clonedObject = {};
  for (let key in originalObject) {
    if (typeof (originalObject[key] != 'object') {
      clonedObject[key] = originalObject[key];
    })
  } else {
    clonedObject[key] = deepClone(originalObject[key]);
  }
}
  return clonedObject;
}

const newObject = deepClone(originalObject);


// Método alternativo:
const obj = {a: 1};
const copy = Object.assign({}, obj);
console.log(copy); // {a: 1}
```


* Iterar um objeto:
```javascript
const obj = {
  id: 1, 
  name: "gowthan",
  active: true
}

for (let key in obj) {
  if(obj.hasOwnProperty(key)){
    console.log(`${key} : ${obj[key]}`) // "name: gowtham" na segunda iteração. 
  }
}
//  Nota: Utiliza "hasOwnProperty()" para garantir que iteramos so as propriedades do objeto, já que um "for in" tambem poderia iterar sobre propriedades aninhadas do prototype

// Outra opção!
Object.keys(obj).forEach(key => {
  console.log(`${key} : ${obj[key]}`);
});

// Si só nos interessam os valores
Object.values(obj).forEach(key => {
  console.log(value);
});

// Isto inclue as propriedades não enumeráveis

Object.getOwnPropertyNames(obj).forEach(key => {
  console.log(`${key} : ${obj[key]}`);
});
```

* Cria um objeto literal apartir dos parâmetros de uma URL:
```javascript
// On this URL: https: //example.com/index.php?foo=foo&bar=bar

Object.fromEntries([...new URL(location).searchParams]); // {foo: "foo", bar: "bar"}
```

----------------------------------------------------------------------------


## Namespaces:

Podemos agrupar funções e valores em um namespace. Isto nos permite organizar melhor nosso código. Existem diferentes maneiras de obter este resultado, uma delas é usar namespaces

* Namespaces com objetos literais:
```javascript
const myApplication = {
  version: '1.0',
  name: 'my aplication',
  config: {
    url: '127.0.0.1', 
    port: 3000
  },
  init: function() {
    // ....
  }
};
```

* Namespaces com IIFE (immediatly invoked function expression):
```javascript
const myApplication = {};

(function(){
  let _protocol = 'https'; // Isso é uma variável "privada", não é acessível fora do escopo.

this.version = '1.2',
this.name = 'my application',
this.config = {
  url: '127.0.0.1',
  port: 3000,
  protocol: _protocol
},
this.init = function() {
  //...
}
}).apply(myApplication);
```

---------------------------------------------------------

## Closures

* Podemos usar closures para organizar nosso código. Um simples exemplo com IIFE functions que nos permite  criar um "módulo", uma interessante
alternativa a um namespace:
```javascript
const geoModule = (function() {
  const _pi = 3.141592; // Propriedade Privada
  const _info = 'This is a module for doing geometrical calculations';
  
  const circleArea = (radius) => {
    if (!Number(radius)) throw new Error('You must provide a radius value');

    return _pi * radius * radius;
  };

  // Definimos que métodos e propriédades são acessíveis publicamente
  return Object.freeze({
    calculateCircleArea: circleArea,
    information: _info
  })
})();

geoModule.calculateCircleArea(5); // 78.5398
geoModule.information; // 'This is a module for doing geometrical calculations'
```

------------------------------------------------------------

## Proxies:
O objeto proxy é usado para definir um comportamento personalizado para operações fundamentais (por exemplo, para observar propriedades,
quando se assinam, enumeração, invocação de funções e etc)

* Exemplo de uso básico de proxies:
```javascript
let product;

(function() {
  let _price = 0;

  product = {};

  // Podemos definir alguns proxies para acessar a propriedade 'price'.
  // Os proxies são muito úteis para fazer validações. Por exemplo, no método 'set'. Neste caso, estamos "interceptando" o acesso a leitura e escritura da propriedade 'price'.
  Object.defineProperty(product, 'price', {
    set: function (price) {
      _price = price;
    },

    get: function (){
      return _price;
    }
  });
})();

product.price = 100; // ao fazer isso, acionamos o proxie 'set'

// Na seguinte linha, acionamos o proxie 'get'
console.log(product.price) // 100
```
---------------------------------------------------------------

## Set:

* Exemplo básico de _set_:
```javascript
const set = new Set([1, 2, 3, 4, 5]);
// Armazena valores únicos de qualquer tipo, inclusive valores primitivos ou objetos de referência.
// Se forma apartir de um objeto iterável. Pode criar vazio passando null.

set.has(4); // 
set.has('4'); // false. Não foi armazenado em uma string, se não é um inteiro.

set.delete(5); // truques

set.add('foo'); // {1, 2, 3, 4, "foo"}
const s = new Set([{a:1},{b:2},{a:1}]);
console.log(s); // {{a:1},{b:2},{a:1}}
// Observa que neste caso parece existir um objeto duplicado, na realidade são objetos distintos!

console.log(s.size); // 3

s.clear(); // Vazia de elementos no set
```

* Descompor strings usando _set_ (lembre que não permite valores duplicados):
```javascript
let mySet = new Set('foooooooooooo');
// Isto acontece por que estamos passando um valor iterável(neste caso uma string)
console.log(mySet); // {"f", "o"}

for (let i of mySet) {
  console.log(i);
} // Uma maneira de iterar um set

mySet.forEach(i => console.log(i)); // Outra maneira de iterar um set.
// Observe, não funcionam: filter, map e reduce.
// Porém podem transforma-lo em um array para usar estes métodos assim: [...mySet]
```

* Eliminar elementos duplicados em um array mediante _set_ e _spread_ operator:
```javascript 
let nums = [1, 2, 2, 2, 2, 3, 4];
function deleteDuplicated(items) {
  return [... new Set(items)]; // retorna um array
}
console.log( deleteDuplicated(nums)); // [1, 2, 3, 4]
```
------------------------------------------------------------

## Map:

* Primeiros passos com _map_:
```javascript
const biz = new Map();
biz.set('name', 'John');
biz.set('age', 53);

console.log(biz.size); // 22

for (let [key, value] of biz) {
  console.log(`${key} => ${value}`);
  // name => John
  // age => 53
}

console.log(biz.get('age')); // 53

biz.delete('age'); // true
console.log(biz) // {"name" => "John"}

biz.clear(); // {}
```

* Mais opções interessantes com _map_:
```javascript
const biz = new Map([ ['name','John'],['Surname', 'Doe'], [undefined, null]]);

console.log(biz.has('name')); // true
console.log(biz.has('Give me a cookie')); // false

console.log(biz) // {"name" => "John", "Surname" => "Doe"}
```

--------------------------------------------------------------------

## Debugging e Console:

* O uso do comando _debugger_ (estabelece um "breakpoint" no código):
```javascript
// some code...
debugger;
// some code...
```

* A propriedade _table_ do objeto _console_ (retorna uma tabela apartir de um objeto ou um array)
```javascript

 