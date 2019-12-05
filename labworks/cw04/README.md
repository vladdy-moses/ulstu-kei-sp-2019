# Лабораторная работа 4. Процедуры и функции. Рекурсия

## Цель работы

Изучить использование процедур и функций.
Понять рекурсию.

## Процедуры и функции

Процедуры и функции помогают сделать Вашу программу понятной.

Для понимания, что это такое, введём понятие **подпрограммы**.
Подпрограммой называют именованную часть программы, которую можно вызвать в коде программы сколько угодно раз.

Теперь всё просто: **функция** - подпрограмма, которая возвращает какое-то значение (например, _функция расчёта синуса_).
**Процедура** - подпрограмма, которая не возвращает никаких значений.

Приведём пример, не связанный с программированием.

Например, мы должны кому-то подробно описать, как купить хлеб.
В общих чертах это может выглядеть так:

```
1 Взять деньги.
2 Дойти до магазина.
3 Непосредственно купить хлеб.
4 Дойти до дома.
```

Далее мы начинаем описывать подробности.
Например, третий шаг можно описать так:

```
3.1 Взять продуктовую корзину.
3.2 Найти хлебный отдел.
3.3 Выбрать подходящий хлеб.
3.4 Положить хлеб в продуктову корзину.
3.5 Пройти на кассу.
3.6 Расплатиться за покупки.
3.7 Оставить продуктовую корзину.
```

Далее можно уже расписать, что такое `выбрать подходящий хлеб` и так далее.

> Кстати, такой способ описания задачи называется **сверху-вниз**, или **от общего к частному**, или **декомпозиция**.
> Как можно догадаться, есть и противоположная схема проектирования, когда мы сначала описываем части, а потом складываем их во что-то общее.

До этого мы писали программы, где не нужно было разделение на процедуры и функции.
Весь наш код мог поместиться в _главной_ процедуре `main`:

```c
int main() {
    // а тут наш код
}
```

> Кстати, в данном случае `main` является функцией, т.к. она возвращает значение целого типа `int`.

Данная подпрограмма называется главной, потому что при старте нашей программы код именно этой процедуры выполняется в первую очередь.
Также наша программа считается завершённой, когда подходит к концу выполнение `main`.

Если мы хотим описать свою подпрограмму, мы должны определиться:
* возвращает ли она какое-то значение и какого оно должно быть типа;
* какое у нашей подпрограммы будет название;
* какие аргументы наша подпрограмма будет включать.

Всё это называется _сигнатурой_ процедуры (или функции).

> Сигнатура без конкретного названия аргументов также называется _прототипом_ процедуры (или функции).

Примеры прототипов расчёта квадратного корня из библиотеки `math.h`:

```c
double      sqrt(double);
float       sqrtf(float);
long double sqrtl(long double);
```

Что мы тут видим?
Что есть три функции, у каждой разный тип возвращаемого значения (слово до sqrt*).
Каждая функция требует наличие одного аргумента (типы данный в скобках), причём тип данных аргумента совпадает с типом возвращаемого значения.

Обратите внимание, что мы не знаем _реализации_ расчёта квадратного корня, мы видим лишь _описание_ функции.
Тем не менее, мы можем её использовать у себя в программе:

```c
#include <stdio.h>
#include <math.h>

int main()
{
    printf("%f", sinf(3.1415 / 2)); // Выведет 1.000000
    return 0;
}
```

Пример описания своей функции:

```c
#include <stdio.h>

// объявляем сигнатуры функций
float func1(int);
int func2(int,int);

// главная функция
int main()
{
    printf("%f", func1(func2(1,2)));
    // Выведет на экран 3.500000
    return 0;
}

// реализация наший функций
float func1(int x) {
    return x + 0.5;
}
int func2(int x, int y) {
    return x + y;
}
```

Такое описание функций (вверху сигнатуры, внизу реализации) позволяет избежать ошибок при сборке программ.

## Рекурсия

В примере выше видно, что функция `func1` принимает на вход результат выполнения функции `func2`.

Однако, никто не запрещает вызывать одну функцию внутри другой:

```c
// реализация наший функций
float func1(int x) {
    return func2(x,x) + 0.5;
}
int func2(int x, int y) {
    return x + y;
}
```

Также никто не запрещает вызвать функцию внутри самой себя!

Рассмотрим другой пример:

```c
#include <stdio.h>

// объявляем сигнатуры функций
int fact(int);

// главная функция
int main()
{
    printf("%d! = %d", 6, fact(6));
    // Выведет на экран "6! = 720"
    return 0;
}

// реализация наший функций
int fact(int i) {
    if (i == 1) {
        return 1;
    } else {
        return i * fact(i - 1);
    }
}
```

Данная программа считает факториал числа.

Здесь объявляется функция `fact`, которая принимает на вход число (для которого ищется факториал) и возвращает этот самый факториал.

Внутри функции `fact` есть проверка:

- ЕСЛИ число, для которого надо найти факториал, равно 1, то возвращаем 1.
- ИНАЧЕ возвращаем произведение этого числа на факториал от _(число - 1)_.

Получается некоторая странная реализация цикла.

В любой функции, которая вызывается рекурсивно, обязательно должно быть **условие выхода из рекурсии**.
В нашем примере это проверка `i == 1`, когда рекурсия "обрывается".

Также иногда полезно объявить ещё одну функцию, которая "запускает" рекурсию с начальными значениями.

## Варианты заданий

Необходимо реализовать одну из рекурсивных функций.

Запрещается использовать циклы, массивы, списки и т.д.

Как выбрать номер задания:

* возьмите вариант от первых лабораторных работ;
* разделите число на 6;
* возьмите остаток от деления;
* полученное число и есть номер задания.

## Задания

| Номер задания | Текст |
|---|---|
| 1 | Дано натуральное число n. Выведите все числа от 1 до n. |
| 2 | Даны два целых числа A и В (каждое в отдельной строке). Выведите все числа от A до B включительно, в порядке возрастания, если A < B, или в порядке убывания в противном случае. |
| 3 | Дано натуральное число N. Вычислите сумму его цифр. |
| 4 | Дано натуральное число, обозначающее номер числа в ряду чисел Фибоначчи. Выведите указанное число. |
| 5 | Дано натуральное число N. Выведите слово YES, если число N является точной степенью двойки, или слово NO в противном случае. Нельзя использовать операцию возведения в степень. |
| 0 | Дано натуральное число N. Выведите все его цифры по одной, в обычном порядке, разделяя их пробелами или новыми строками. |

## Ссылки

* [Лабораторная работа №3](../cw03/README.md)
* [Лабораторная работа №5](../cw05/README.md)