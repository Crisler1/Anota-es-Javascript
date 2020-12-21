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
});


function createUser({username = 'Anonymous'} = {}) {

};


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

console.log(hello() );

var result = (function(){
  var name = "Barry";
  return name;
})();

console.log(result);

```


* Factory Functions (Uma "Factory Function" é uma função que cria objetos e os retorna):
```javascript 

const cat = () => {

  const sound = 'miau',
  let color = 'white';

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

const smurfy = cat();
console.log( smurfy.talk() );

console.log( smurfy.getColor() );
smurfy.setColor('black');
console.log( smurfy.getColor() );



const makeCounter = function() {

  let _count = 0;

  const changeBy = (val) => {
    _count += val;
  }

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

console.log(Counter1.getValue());

Counter1.increment();
Counter2.increment();
console.log(Counter1.getValue());

Counter1.decrement();
console.log(Counter1.getValue());

console.log(Counter2.getValue());

```

Outro exemplo de _Factory Functions_ mais avançado:
```javascript

const dataConnection = (data = {}) => {
  const settings = data;

  return Object.freeze({

    getSettings: () => settings,

    modifySettings: (addData = {}) => {
      return Object.assign(settings, addData);

      
    }
  });
} 

const connection = dataConnection({ ip: '127.0.0.1', port: '8080'});


console.log(connection.getSettings());
console.log(connection.getSettings().ip);

connection.method = 'http';





connection.modifySettings({ method: 'http'});

console.log(connection.getSettings());

connection.modifySettings({ip: '192.168.2.11'});

console.log(connection.getSettings());
```

---------------------------------------------------

## Classes em ES6

* Exemplo de classes em ES6:
```javascript
class Person {
  constructor(name) {

    this.name = name;
  }

  greeting() {
    console.log(`Hello, my name is ${this.name}`);
  }
}

let john_doe = new Person("John Doe");

john_doe.greeting();

console.log(john_doe instanceof Person);
console.log(john_doe instanceof Object);

console.log( typeof john_doe);

console.log( typeof Person);

console.log( typeof Person.prototype.greeting);

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
    return this;
  }
  setModel(model){
    this.model = model;
    return this;
  }
  setColor(color){
    this.color = color;
    return this;
  }

  getCarInfo(){
    return this;
  }
}

const car = new Car('Ford', 'F-150','red').setColor('blue').setModel('Ranger');

console.log( car.getCarInfo());
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

}
