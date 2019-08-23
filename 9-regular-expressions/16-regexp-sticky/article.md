
# Липкий флаг "y", поиск на конкретной позиции

Чтобы разобрать флаг `y` и понять, чем же он хорош, рассмотрим практический пример.

Одна из часто встречающихся задач регулярных выражений - лексический разбор: мы имеем текст на каком-то языке программирования и с помощью регулярных выражений получаем его структурные элементы.

Например, в браузеры встроен лексический анализатор HTML, а также для языка JavaScript.

Мы не будем погружаться глубоко в тему написания таких анализаторов (это специализированная область со своим набором инструментов и алгоритмов). Но в процессе их работы, вообще, в процессе анализа текста, очень часто возникает вопрос: "Что за сущность находится в тексте на заданной позиции?"

Например, для языка программирования варианты могут быть следующие:

- Это название переменной или функции `pattern:\w+`?
- Или число `pattern:\d+`?
- Или оператор `pattern:[+-/*]`?
- (Или же это синтаксическая ошибка, если не попадает ни под один из ожидаемых вариантов)

То есть, мы применяем разные регулярные выражения и смотрим, какое совпадёт.

Ключевой момент - нас интересует текст с заданной позиции.

При обычном поиске по регулярному выражению поиск идёт во всей строке, а нам надо применить регулярное выражение на конкретной позиции и не искать больше нигде.

Именно это и делает флаг `y`.

Пример:

```js run
let str = "(text before) function ...";

*!*
let regexp = /function/y;
regexp.lastIndex = 5;
*/!*

alert (regexp.exec(str)); // null (совпадение не найдено, в отличие от флага "g"!)

*!*
regexp.lastIndex = 14;
*/!*

alert (regexp.exec(str)); // function (совпадение!)
```

Как мы видим, теперь регулярное выражение проверяет на совпадение только в указанной позиции.

Именно это делает флаг `y` уникальным, и очень важным при написании парсера.

Флаг `y` позволяет проверять регулярное выражение именно на конкретной позиции и двигаться дальше, после того как парсер определит, что именно находится в этой позиции -- шаг за шагом исследуя текст.

Без этого флага регулярное выражение будет выполнять поиск до конца текста, что будет занимать время, особенно, если текст большой. Таким образом парсер будет очень медленным. Флаг `y` для подобных задач -- именно то, что нужно.