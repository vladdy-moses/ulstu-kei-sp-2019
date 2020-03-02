# Лабораторная работа №2. Побитовые операторы

## Цель работы

Приобрести навыки в практическом использовании побитовых операторов, в том числе побитового сдвига.

## Виды операторов в языке C

В семействе С-подобных языков существует множество различных операторов.
Наверное, самый популярный оператор языка - инкремент (прибавление единицы к числу) - `i++` или `++i`.
Также очевидно, что операции сложения, деления, вычитания и умножения содержат операторы `+`, `/`, `-` и `*` соответственно.

> А вот оператора возведения в степень (например, `2^3`) в C-подобных языках нет.
> Для этого необходимо использовать функцию `pow`.

Согласно [сайту Иванова А.М.](https://xn----7sbbfb7a7aej.xn--p1ai/informatika_kabinet/programm/programm_10.html) **оператор** — это элемент языка, задающий полное описание действия, которое необходимо выполнить.

Операторы можно разделить на:

* _унарные_ — те, которые выполняют действия над одним элементом.
  Примером таких операторов являются `++` и `--`, которые прибавляют или вычитают единицу из значения переменной или выражения.
* _бинарные_ — те, которые требуют для выполнения действия два элемента.
  Приведённые выше операторы сложения, умножения и т.д. как раз относятся к бинарным.
* _тернарные_ — те, которые требуют три элемента.
  Пример: условный (тернарный) оператор.
  Синтаксис на псевдоязыке: `var result = условие ? значение1 : значение2;`. Зачастую обозначается как `?:`.

Как многие элементы нашей жизни, операторы С-подобных языков можно классифицировать по-разному.
Например, их можно разделить на:

* _арифметические_: `+`, `/`, `--` и т.п.
* _относительные_: `>`, `<`, `>=`, `==`, `!=` и т.п.
* _логические_: `&&`, `||`, `!`.
* _побитовые_: `&`, `|`, `<<`, `>>`, `^`, `~`.
* _условные_: `?:`.

Если с первыми тремя видами операторов мы сталкивались в предыдущих работах (особенно в [третьей](../cw03/README.md)), то побитовые операторы нам ещё не знакомы.
Остановимся на них более подробно.

## Логические операции

Давайте вспомним, что все вычисления на современных компьютерах являются двоичными, т.е. наименьшая из возможных единиц хранения и обработки информации - двоичный бит, который принимает значение 0 или 1.
На самом деле, два значения бита можно называть по-разному: ИСТИНА и ЛОЖЬ, низкое напряжение и высокое напряжение, НЕТ и ДА и т.д.

В связи с развитием компьютерной техники получили развитие науки, так или иначе работающие с такими двоичными представлениями, а именно _математическая логика_ (особенно логика высказываний) и _дискретная математика_.
Эти дисциплины наверняка были в вашей учебной программе на ранних курсах.

Вспоминая эти дисциплины, в памяти могут всплыть логические операции сложения (логическое ИЛИ, дизъюнкция), умножения (логическое И, конъюнкция) и отрицания (логическое НЕ, инверсия).

Напомним их _таблицы истинности_:

| a   | b   | a ИЛИ b |
| --- | --- | ------- |
| 0   | 0   | 0       |
| 0   | 1   | 1       |
| 1   | 0   | 1       |
| 1   | 1   | 1       |

> Как можно видеть, результат _логического ИЛИ_ равен ЛОЖЬ, только если оба выражения ложны.
> Пример: "Я живу или в США, или в Северной Корее". Оба выражения ложны - результат ложный. В любом другом случае результат будет истинным.

| a   | b   | a И b |
| --- | --- | ----- |
| 0   | 0   | 0     |
| 0   | 1   | 0     |
| 1   | 0   | 0     |
| 1   | 1   | 1     |

> Для _логического И_ наоборот необходима истинность всех выражений.
> Например, высказывание "Я и преподаватель, и студент" может быть верно только для магистрантов, когда студент действительно может заниматься преподаванием.
> Аспиранты, кстати, студентами не являются, зато являются обучающимися.

| a   | НЕ a |
| --- | ---- |
| 0   | 1    |
| 1   | 0    |

> _Логическое отрицание_ делает из лжи правду и наоборот.
> Например, высказывание "идёт дождь" противоположно высказыванию "не идёт дождь" (а идёт снег, например, или ничего не идёт).

Тут же следует упомянуть ещё одну операцию - _исключающее ИЛИ_ (XOR).
При ней результат равен истине только тогда, когда выражения разные:

| a   | b   | a XOR b |
| --- | --- | ------- |
| 0   | 0   | 0       |
| 0   | 1   | 1       |
| 1   | 0   | 1       |
| 1   | 1   | 0       |

## Побитовые операции

На заре развития компьютерной техники было крайне необходимо экономить ресурсы.
Как вы можете знать, оперативная память исчислялась килобайтами, а количество операций процессора - сотнями тысяч в секунду.
Сейчас такие объёмы и скорости кажутся ничтожно малыми.

Так сложилось, что минимально адресуемая единица памяти - это 1 байт, то есть 8 бит.
Это было придумано в первую очередь для экономии ресурсов, ведь таким образом можно значительно сократить затраты на вычисление адреса в памяти.
Однако для записи одного логического результата (ИСТИНА или ЛОЖЬ) необходим только 1 бит из 8, которые нам доступны.
Таким образом, пропадает целых 7 бит, которые можно было бы использовать.

Для решения этой проблемы были придуманы операторы, которые позволяют выполнять логические операции "пачкой" (или пакетно) ко всему байту (или даже нескольким байтам).

Рассмотрим пример.

Пусть в первом байте будет записано число 21.
В двоичной системе оно будет представлено как `10101`.
Допишем в начало нули, чтобы количество разрядов (битов) стало равным 8 (как в байте) - `00010101`.

Пусть во втором байте будет записано число 35: `00100011`.

Операция _побитового И_ будет применять операцию _логического И_ для каждой пары битов первого и второго числа (в скобках указано число в десятичной системе счисления):

```
00010101 (21)
00100011 (35)
-------- И
00000001 (1)
```

> Можно заметить, что бит = 1 в результате вышел только в самом младшем (правом) разряде.
> Догадайтесь самостоятельно, почему так получилось.

Рассмотрим вывод операции _побитовое ИЛИ_.
В результирующем байте бит = 1 будет на том месте, где хотя бы один бит исходных чисел был равен 1:

```
00010101 (21)
00100011 (35)
-------- ИЛИ
00110111 (55)
```

Также рассмотрим примеры _побитового отрицания_ и _побитового исключающего ИЛИ_:

```
00010101 (21)
-------- НЕ
11101010 (234)
```

```
00010101 (21)
00100011 (35)
-------- XOR
00110110 (54)
```

## Побитовые операторы в языке C/C++

Чтобы объявить в языке C/C++ один байт, необходимо использовать тип данных `unsigned char`.

Объявим переменные с исходными байтами из примера выше:

```c
unsigned char a = 21; // 00010101
unsigned char b = 35; // 00100011
```

Ниже расположены код, который демонстрирует использование операторов побитового И, ИЛИ, XOR и НЕ:

```c
// выполнение операций
unsigned char bitwiseAnd = a & b; // побитовое И
unsigned char bitwiseOr = a | b;  // побитовое ИЛИ
unsigned char bitwiseXor = a ^ b; // побитовое XOR
unsigned char bitwiseNot = ~a;    // побитовое НЕ

// отображение результатов
printf("a & b = %d\n", bitwiseAnd); // должно быть 1
printf("a | b = %d\n", bitwiseOr);  // должно быть 55
printf("a ^ b = %d\n", bitwiseXor); // должно быть 54
printf("~a = %d\n", bitwiseNot);    // должно быть 234
```

## Побитовый сдвиг

Отдельно следует рассмотреть операторы побитового сдвига `<<` и `>>`.
Их не следует путать с аналогичными операторами работы с потоками в C++.

Данные операторы позволяют "дополнить" нулевыми битами байт слева или справа.
Но так как количество битов в байте ограничено, биты с другого края забываются.

Рассмотрим пример, для которого опять же возьмём число 21 (`00010101`).

Давайте сначала сдвинем байты влево на 2 разряда.
В языке C/C++ это записывается как `a << 2`.
Стрелочка показывает, куда следует производить сдвиг.

```
  00010101 (21)
  -------- << 2
00010101   (сдвинули на 2 бита)
0001010100 (дописали нули в начало)
  01010100 (отрезали лишнее, получилось 84 = 21 * 2 * 2)
```

> После серии экспериментов можно догадаться, что на самом деле исходное число умножается на 2 в степени сдвига.

Теперь давайте рассмотрим сдвиг вправо также на два бита:

```
00010101   (21)
--------   >> 2
  00010101 (сдвинули вправо)
0000010101 (дописали нулями)
00000101   (убрали лишнее, получилось 5 = 21 / 2 / 2 целое)
```

Как видно, пришлось "потерять" один бит, которые был равен 1.

Те же операции на языке C/C++:

```c
unsigned char shiftLeft  = a << 2; // сдвиг влево на 2
unsigned char shiftRight = a >> 2; // сдвиг вправо на 2

printf("a << 2 = %d\n", shiftLeft);  // должно быть 84
printf("a >> 2 = %d\n", shiftRight); // должно быть 5
```

## Задание на лабораторную работу

1. Уточнить номер вашего варианта у преподавателя. Номер варианта выдаёт только он.
2. Набрать программу по выполнению побитовых операций из раздела "Побитовые операторы в языке C/C++".
3. Дополнить программу для вычисления сдвигов влево и вправо из раздела "Побитовый сдвиг".
4. Удостовериться, что программа запускается и возвращает те же результаты, что и указаны в комментариях (слова после `//`).
5. Изменить значение переменных `a` и `b` следующим образом:
   1. В переменную `a` задать номер варианта.
   2. В переменную `b` задать произведение месяца и дня Вашего рождения. Если полученное число больше 255, то разделить его на 2 (вычисления можно выполнить не в программе).
6. Рассчитать на бумаге результаты выполнения побитовых операций и перевести полученные числа в десятичную систему счисления.
7. Проверить самого себя по выводу программы.

Для защиты лабораторной работы **обязательно** требуется наличие расчётов на бумаге для всех побитовых операций с Вашими значениями `a` и `b`.