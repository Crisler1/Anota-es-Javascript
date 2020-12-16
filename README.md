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