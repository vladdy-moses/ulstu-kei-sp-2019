# Лабораторная работа 3. Вложенные циклы

## Цель работы

Изучить алгоритмическую конструкцию "цикл".

## Немного про циклы

Цикл - алгоритмическая конструкция, действия в которой повторяются многократно.

Для любого цикла должно быть определено _тело цикла_ и _условие повторения_.

Можно выделить следующие виды циклов:
- с предусловием;
- с постусловием;
- с параметром.

Цикл с предусловием подразумевает, что выполнение тела цикла будет после очередной проверки условия повторения.
То есть может случиться так, что тело цикла ни разу не выполнится (если условие повторения изначально ложно).

Пример такого цикла: _"Пока идёт дождь, надо брать на улицу зонт"_.

Цикл с предусловием в C-подобных языках определяется следующей конструкцией:

```c
while (/*условие повторения*/) {
    // тело цикла
}
```

Цикл с посусловием аналогичен, но условие повторения проверяется уже после выполнения тела цикла.
Это может быть полезно, если необходимо выполнить тело цикло как минимум один раз.

Пример: _"Иди направо. Если упрёшься в стену, поверни налево, иначе продолжай движение."_.

Цикл с предусловием в C-подобных языках определяется следующей конструкцией:

```c
do {
    // тело цикла
} while (/*условие повторения*/);
```

Цикл с параметром позволяет упростить повторение, если есть какой-то параметр.
Например, когда необходимо вывести на экран все чётные числа от 2 до 100, переменную цикла, условие повторения и операцию прибавления можно объединить одним оператором.

Сравните два примера одного и того же цикла:

```c
int i = 2;
while (i <= 100) {
    printf("%d ", i);
    i += 2;
}
```

```c
for (int i = 2; i <= 100; i += 2) {
    printf("%d ", i);
}
```

Первый цикл с постусловием, второй с параметром.

> Как и условия, циклы можно вкладывать друг в друга.
> Например, цикл с параметром `i`, а в теле цикла ещё один цикл с параметром `j`.

> Кстати, переменные `i`, `j`, `k`, `r` для обозначения параметров циклов являются негласным стандартом.
> В "верхнем" цикле обычно используется `i`, во вложенном - `j`, в следующем вложенном - `k` и.т.д.

## Варианты заданий

| Вариант | Задание 1 | Задание 2 |
| ------- | --------- | --------- |
| 1       | 3         | 8         |
| 2       | 6         | 6         |
| 3       | 9         | 1         |
| 4       | 5         | 3         |
| 5       | 10        | 10        |
| 6       | 2         | 2         |
| 7       | 1         | 5         |
| 8       | 8         | 9         |
| 9       | 7         | 7         |
| 10      | 4         | 2         |
| 11      | 4         | 4         |
| 12      | 2         | 9         |
| 13      | 3         | 4         |
| 14      | 6         | 10        |
| 15      | 5         | 7         |
| 16      | 8         | 6         |
| 17      | 10        | 8         |
| 18      | 7         | 3         |
| 19      | 9         | 5         |
| 20      | 1         | 1         |
| 21      | 10        | 6         |
| 22      | 7         | 5         |
| 23      | 4         | 10        |
| 24      | 5         | 3         |
| 25      | 9         | 4         |
| 26      | 2         | 2         |
| 27      | 1         | 9         |
| 28      | 3         | 8         |
| 29      | 6         | 7         |
| 30      | 8         | 1         |

## Задание 1

1: 
```
        5
      4 5
    3 4 5
  2 3 4 5
1 2 3 4 5
  2 3 4 5
    3 4 5
      4 5
        5
```

2:
```
        1
      2 1
    3 2 1
  4 3 2 1
5 4 3 2 1
  4 3 2 1
    3 2 1
      2 1
        1
```

3:
```
        1
      1 2 1
    1 2 3 2 1
  1 2 3 4 3 2 1
1 2 3 4 5 4 3 2 1
```

4:
```
        5
      4 5 4
    3 4 5 4 3
  2 3 4 5 4 3 2
1 2 3 4 5 4 3 2 1
```

5:
```
        1
      2 1 2
    3 2 1 2 3
  4 3 2 1 2 3 4
5 4 3 2 1 2 3 4 5
```

6:
```
        5
      5 4 5
    5 4 3 4 5
  5 4 3 2 3 4 5
5 4 3 2 1 2 3 4 5
```

7:
```
5 4 3 2 1 
  5 4 3 2
    5 4 3
      5 4
        5
      5 4
    5 4 3
  5 4 3 2 
5 4 3 2 1
```

8:
```
        1
      1 2
    1 2 3
  1 2 3 4
1 2 3 4 5
  2 3 4 5
    3 4 5
      4 5
        5
```

9:
```
1 2 3 4 5
  2 3 4 5
    3 4 5
      4 5
        5
      4 5
    3 4 5
  2 3 4 5
1 2 3 4 5
```

10:
```
1 2 3 4 5
  1 2 3 4
    1 2 3
      1 2
        1
      1 2
    1 2 3
  1 2 3 4
1 2 3 4 5
```

## Задание 2

1: 
```
1 2 3 4 5
  2 3 4 5
    3 4 5
      4 5
        5
        5 4
        5 4 3
        5 4 3 2
        5 4 3 2 1
```

2:
```
1 2 3 4 5
  1 2 3 4
    1 2 3
      1 2
        1
        2 1
        3 2 1
        4 3 2 1
        5 4 3 2 1
```

3:
```
5 4 3 2 1
  5 4 3 2
    5 4 3
      5 4
        5
        4 5
        3 4 5
        2 3 4 5
        1 2 3 4 5
```

4:
```
5 4 3 2 1
  4 3 2 1
    3 2 1 
      2 1
        1
        1 2 
        1 2 3
        1 2 3 4
        1 2 3 4 5
```

5:
```
        1
        1 2 
        1 2 3
        1 2 3 4
        1 2 3 4 5
5 4 3 2 1
  4 3 2 1
    3 2 1 
      2 1
        1
```

6:
```
        5
        4 5
        3 4 5
        2 3 4 5
        1 2 3 4 5
5 4 3 2 1
  5 4 3 2
    5 4 3
      5 4
        5
```

7:
```
        1
        2 1
        3 2 1
        4 3 2 1
        5 4 3 2 1
1 2 3 4 5
  1 2 3 4
    1 2 3
      1 2
        1
```

8:
```
        1
        2 1
        3 2 1
        4 3 2 1
        5 4 3 2 1
      4 5  
    3 4 5
  2 3 4 5
1 2 3 4 5
```

9:
```
        5
        4 5
        3 4 5
        2 3 4 5
        1 2 3 4 5
        1
      2 1
    3 2 1
  4 3 2 1
5 4 3 2 1
```

10:
```
        1
        1 2 
        1 2 3
        1 2 3 4
        1 2 3 4 5
      1 2
    1 2 3
  1 2 3 4
1 2 3 4 5
```

## Ссылки

* [Лабораторная работа №2](../cw02/README.md)
* [Лабораторная работа №4](../cw04/README.md)
