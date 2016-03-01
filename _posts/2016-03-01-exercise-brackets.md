---
title:  "Последовательность скобок"
categories: JavaScript
date:   2016-03-01 11:00:00 +0300
author_name: "Евгений Бовыкин"
author: missingdays
type: exercise
identifier: exercise-brackets

description: "Напишите функцию, которая будет исправлять последовательность скобок, используя минимальное количество изменений."

tags: [javascript]
---

Вы пишите текстовый редактор с крутой подсветкой синтаксиса. Вы хотите исправлять ошибки программиста, в том числе неправильно расставленные скобки.

Напишите функцию, которая будет исправлять последовательность скобок, используя минимальное количество изменений. Возможные скобки `[]`, `()`, `{}`, `<>`. Интуитивно понятно, что такое правильная последовательность, однако более формальное определение, а так же основные методы работы с ними можно найти [тут](http://neerc.ifmo.ru/wiki/index.php?title=%D0%9F%D1%80%D0%B0%D0%B2%D0%B8%D0%BB%D1%8C%D0%BD%D1%8B%D0%B5_%D1%81%D0%BA%D0%BE%D0%B1%D0%BE%D1%87%D0%BD%D1%8B%D0%B5_%D0%BF%D0%BE%D1%81%D0%BB%D0%B5%D0%B4%D0%BE%D0%B2%D0%B0%D1%82%D0%B5%D0%BB%D1%8C%D0%BD%D0%BE%D1%81%D1%82%D0%B8). 

Метод должен принимать строку, состоящую только из разрешенных скобок, и возвращать строку, содержащую правильную последовательность скобок. [Edit distance](https://en.wikipedia.org/wiki/Edit_distance) этих двух строк должна быть минимальной (другими словами, они должны отличаться как можно меньше).

*Изменять можно только закрывающие скобки, т.е. `]})>`. Открывающие скобки должны остататься без изменений. Если исправить строку невозможно, верните `null`.*

Примеры
{% highlight javascript %}

correctBrackets("[}"); // "[]"
correctBrackets("[()}"); // "[()]"
correctBrackets("[[[)])"); // "[[[]]]"
correctBrackets("<{[(>}])"); // "<{[()]}>"
correctBrackets("]]"); // null
correctBrackets("[[[))"); // null
correctBrackets("{((])]"); // "{(())}"

{% endhighlight %}
