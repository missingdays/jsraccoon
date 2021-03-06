---
layout: post

title:  "ES6: Параметры по умолчанию"
categories: JavaScript
author: "Виталий Ртищев"
date:   2016-01-09 20:50:58 +0300
type: article

comments: true
identifier: es6-defaults

prevTitle: "Операторы Spread и Reset"
prevLink: "http://jsraccoon.ru/es6-spread-rest"

nextTitle: "Реструктуризующее присваивание"
nextLink: "http://jsraccoon.ru/es6-destructuring"

description: "Параметры по умолчанию в ES6 призваны обобщить сразу несколько паттернов и существенно упростить восприятие функций."

tags: [javascript, es6]
---

## ES5 реализации
Если вы просмотрите исходный код любой библиотеки, то с огромной долей вероятности обнаружите, что там используется паттерн "параметры по умолчанию". Работает это следующим образом:
{% highlight javascript %}
var sum = function(a, b) {
  a = a || 10;
  b = b || 20;
  return a + b;
};
sum(1, 2); // 3
sum(1);    // 21
sum();     // 30
{% endhighlight %} 

Функция `sum` принимает два параметра `a` и `b` и возвращает их сумму. Если параметр `b` не был передан, то он автоматически становится равным `20`, если же не было передано ни одного параметра − `a` присваивается значение `10`, а `b` − `20`. Всё работает хорошо до тех пор, пока не появляется необходимость передать [ложное значение](https://developer.mozilla.org/ru/docs/Glossary/Falsy), например, ноль:
{% highlight javascript %}
// Выражение ( 0 || 10 ) вернет 10, так как 0 − ложное значение
sum(0, 1); // 11
{% endhighlight %} 

Для того, чтобы избежать подобных проблем, можно использовать одно из следующих решений:
{% highlight javascript %}
var sum = function(a, b) {
  // Принимаются все переданные значения, кроме undefined
  a = (a !== undefined) ? a : 10;
  b = (b !== undefined) ? b : 20;
  return a + b;
};

sum(0, 5); // 5
sum(undefined, 40); // 50
sum(40, undefined); // 60
sum(40, null);      // NaN

var sum = function(a, b) {
  // Принимаются все переданные значения
  a = (0 in arguments) ? a : 10;
  b = (1 in arguments) ? b : 20;
  return a + b;
}; 

sum(0, 5); // 5
sum(undefined, 40); // NaN
sum(40, undefined); // NaN
sum(40, null); // NaN
{% endhighlight %} 

## ES6 реализация
С релизом ES6 появилась возможность задавать параметры по умолчанию при объявлении функции:
{% highlight javascript %}
var sum = function(a = 10, b = 20) {
  return a + b;
};

sum(); // 30

sum(1, 2); // 3
sum(5);    // 25

sum(undefined, 5); // 15
sum(5, undefined); // 25

sum(null, 5); // 5 − null преобразуется в 0
sum(1, null); // 1 − null преобразуется в 0
{% endhighlight %} 

Чтобы пропустить какой-либо параметр достаточно передать в функцию `undefined` (параментр не будет пропущен при передаче любого другого ложного значения). Новая синтаксическая конструкция сочетает в себе несколько прошлых реализаций и в большей степени похожа на `a !== undefined ? a : 10`, чем на более распросраненный паттерн `a || 10`. 

Подобная реализация позволяет не только присваивать определённые значения, но и вычислять их динамически с помощью простых выражений или функций:
{% highlight javascript %}
var reverse = function(str) {
  return str.split('').reverse().join('');
};

var logReverse = function(phrase = 'Hello World!', reversed = reverse(phrase)) {
  console.log(reversed); 
};

logReverse(); // !dlroW olleH
logReverse('ES6 is awesome'); // emosewa si 6SE
{% endhighlight %} 

Тем не менее, использовать переменную в своей же инициализации нельзя:
{% highlight javascript %}
var x = 10, y = 11, z = 12;
var f = function(x = 1, y = x + 1, z = z + 2) {
  console.log(x, y, z);
};

f(); // 1 2 null
{% endhighlight %} 

Подобное поведение называется [временной мёртвой зоной](http://jsraccoon.ru/es6-block-scoped-declarations/). Как и при объявлении переменных с помощью операторов `let` и `const`, имя переменной `z` резервируется, поэтому интерпретатор никогда не использует переменную с таким же именем из более высокой области видимости.