# Лабораторная работа 6. Основные принципы ООП

## Цель работы

Изучить основные принципы объектно-ориентированного программирования: инкапсуляцию, полиморфизм, наследование.

## Инкапсуляция

Представим, что мы разрабатываем сайт, где клиент банка может предварительно рассчитать себе кредит.
Обычно для этого необходим средний доход клиента и срок, на который он готов этот кредит взять.
На выходе сайт должен показать наиболее выгодные условия: сумму (в рублях) и процентную ставку (в процентах).

Всё хорошо, но есть одна проблема: нам никто не рассказал, как считать сумму кредита и процентную ставку.
Зато сказали, что есть программист Вася, который знает, как это делается.

Мы подходим к Васе и ставим задачу о разработке алгоритма, который:

* на вход принимает доход клиента (дробный) и срок кредита (в месяцах, целый);
* на выход возвращает максимальную сумму кредита (дробную) и процентную ставку (тоже дробную).

После этого Вася довольно кивает и пропадает на неделю.
Мы пока сделаем форму ввода данных, сверстаем красивую веб-страницу и т.д.

Через неделю Вася приходит и говорит, что он сделал _класс_ и даёт нам его описание:

```cpp
class CreditCalc
{
private:
    void _GetDataFromWebService(); // получает какие-то данные (нам не интересно)
    void _Calculate(); // производит расчёт (тоже лишнее)

public:
    CreditCalc(double salary, int period); // конструктор класса
    double GetMaxSum(); // возвращение максимальной суммы
    double GetCreditPercent(); // возвращение процентной ставки
};
```

Нас это полностью удовлетворяет, и мы начинаем этот класс использовать:

```cpp
#include <iostream>
#include "CreditCalc.h"

using namespace std;

int main() {
    double salary;
    int period;

    // Как будто клиент банка ввёл на сайте данные
    cin >> salary;
    cin >> period;

    // подсчёт и вывод данных
    CreditCalc* calc = new CreditCalc(salary, period);
    cout << "max sum: " << calc->GetMaxSum() << " (rub)" << endl;
    cout << "percent: " << calc->GetCreditPercent() << " %" << endl;

    // обязательно чистим за собой
    delete calc;

    return 0;
}

```

Заметили, что чего-то не хватает?
А не хватает реализации самого алгоритма расчёта!
Тем не менее, всё нормально работает.

Это и называется инкапсуляцией.

**Инкапсуляция** – это свойство системы, позволяющее объединить данные и методы, работающие с ними, в классе, и скрыть детали реализации от пользователя.

> Другие классы видят только те поля и методы, которые помечены как `public`.

## Наследование

Например, нас попросили написать программу-калькулятор площадей геометрических фигур.

Мы написали класс `Прямоугольник` (`Rectangle`), у которого в конструкторе задаётся ширина и высота, а также есть метод по расчёту площади:

```cpp
// ЗАГОЛОВОК
class Rectangle {
private:
    double width;
    double height;
public:
    Rectangle(double width, double height); // конструктор
    double GetArea(); // "дай площадь"
};

// РЕАЛИЗАЦИЯ
Rectangle::Rectangle(double width, double height) {
    this->width = width;
    this->height = height;
}

double Rectangle::GetArea() {
    return this->width * this->height;
}
```

Это было нетрудно.
Теперь нас попросили считать площадь квадрата.
Мы помним, что квадрат - это прямоугольник, у которого ширина и высота равны.

Конечно, можно было бы определить новый класс с новыми методами, но здесь на помощь приходит наследование.

**Наследование** – это свойство системы, позволяющее описать новый класс на основе уже существующего с частично или полностью заимствующейся функциональностью.

Возникает вопрос: как сказать нашей программе, что новый класс `Квадрат` (`Square`) - этот тот же самый прямоугольник?

```cpp
// ОПИСАНИЕ
class Square : public Rectangle {
public:
    Square(double side);
};

// РЕАЛИЗАЦИЯ
Square::Square(double side) : Rectangle(side, side) { }
```

Заметьте, как мало кода мы написали?

На самом деле тут произошло следующее:

* в описании класса мы сказали, что он наследует все методы от родительского (то есть от прямоугольника);
* также в описании класса мы указали конструктор - то новое, чего нет у прямоугольника, но есть у квадрата;
* в реализации мы сказали, что при вызове конструктора квадрата необходимо автоматически запускать конструктор прямоугольника.

Полный код по описанию наследования в одном файле:

```cpp
#include <iostream>

using namespace std;

// ОПРЕДЕЛЕНИЯ классов
class Rectangle {
private:
    double width;
    double height;
public:
    Rectangle(double width, double height); // конструктор
    double GetArea(); // "дай площадь"
};

class Square : public Rectangle {
public:
    Square(double side);
};

// ТОЧКА ВХОДА в приложение
int main() {
    Rectangle* r = new Rectangle(2,4.155);
    Square* s = new Square(16);
    
    cout << "area of r: " << r->GetArea() << endl; // 8.31 = 2 * 4.155
    cout << "area of s: " << s->GetArea() << endl; // 256 = 16 * 16
    
    delete r, s;
    
    return 0;
}

// РЕАЛИЗАЦИЯ для прямоугольника
Rectangle::Rectangle(double width, double height) {
    this->width = width;
    this->height = height;
}

double Rectangle::GetArea() {
    return this->width * this->height;
}

// РЕАЛИЗАЦИЯ для квадрата
Square::Square(double side) : Rectangle(side, side) { }
```

## Полиморфизм

Мы продолжаем писать наш калькулятор площадей геометрических фигур.

И вот незадача: у нас появилась окружность!
Мы не можем описать окружность через прямоугольник, т.к. у неё нет ширины и высоты.

Значит, опишем класс с окружностью как делали это с прямоугольником:

```cpp
// ОПИСАНИЕ
class Circle {
private:
    double radius;
public:
    Circle(double radius); // конструктор
    double GetArea(); // "дай площадь"
};

// РЕАЛИЗАЦИЯ
Circle::Circle(double radius) {
    this->radius = radius;
}

double Circle::GetArea() {
    return 3.14159265358979323 * this->radius * this->radius;
}
```

Пока всё просто: мы сделали то же самое, что и для прямоугольника, просто поменяли данные в конструкторе и переписали формулу расчёта.

На этом можно было бы остановиться, но есть одна вещь, которая не выходит из головы: ведь у всех геометрических фигур есть площадь.
Нам что, для каждой фигуры необходимо придумывать свой метод `double GetArea()`?

Конечно нет.
Мы можем создать класс `ГеометрическаяФигура` (`Figure`), описать в нём метод получения площади (ведь у всех фигур можно получить площадь?) и наследовать наш прямоугольник и окружность от этого нового базового класса.

Это будет выглядеть так:

```cpp
// ОПИСАНИЕ
class Figure {
public:
    virtual double GetArea(); // "Дай площадь"
};

// РЕАЛИЗАЦИЯ
double Figure::GetArea() {
    return -1; // Площадь не может быть меньше нуля.
}
```

_(также логично, что `Circle` и `Rectangle` теперь наследуются на `Figure`)_

Стоит внимательнее остановиться на описании нового базового класса, в особенности на слове **virtual**.
Оно означает, что если у какого-нибудь дочернего класса мы забудем описать формулу для расчёта площади (сейчас мы ни для кого не забыли), то площадь будет рассчитываться так, как указано в этом классе (то есть будет возвращаться -1).

Это удобно, когда мы _знаем_, что все потомки базового класса обладают каким-то свойством (например, наличие площади или периметра у геометрических фигур или возможность пить у животных), но мы не знаем, _как именно_ это свойство реализовано (у круга площадь рассчитывается по одному алгоритму, у прямоугольника по второму, а у криволинейной трапеции совсем иначе).

Это и называется полиморфизмом.

**Полиморфизм** – это свойство системы использовать объекты с одинаковым интерфейсом без информации о типе и внутренней структуре объекта.

Кстати, при описании переменной можно будет использовать имя базового класса.
Полный пример с описанием всех классов и их использованием в одном файле:

```cpp
#include <iostream>

using namespace std;

// ОПРЕДЕЛЕНИЯ классов
class Figure {
public:
    virtual double GetArea(); // "Дай площадь"
};

class Rectangle : public Figure {
private:
    double width;
    double height;
public:
    Rectangle(double width, double height); // конструктор
    double GetArea(); // "Дай площадь"
};

class Circle : public Figure {
private:
    double radius;
public:
    Circle(double radius); // конструктор
    double GetArea(); // "Дай площадь"
};

class Square : public Rectangle {
public:
    Square(double side);
};

// ТОЧКА ВХОДА в приложение
int main() {
    Figure* f = new Rectangle(2,4.155); // тут в переменной f лежит прямоугольник
    cout << "area of rectangle: " << f->GetArea() << endl;

    delete f;
    f = new Square(16); // теперь переменная f - это квадрат
    cout << "area of square: " << f->GetArea() << endl;

    delete f;
    f = new Circle(10); // а теперь переменная f - это окружность
    cout << "area of circle: " << f->GetArea() << endl;
    
    delete f;
    return 0;
}

// РЕАЛИЗАЦИЯ для любой геометрической фигуры
double Figure::GetArea() {
    return -1; // Площадь не может быть меньше нуля.
}

// РЕАЛИЗАЦИЯ для прямоугольника
Rectangle::Rectangle(double width, double height) {
    this->width = width;
    this->height = height;
}

double Rectangle::GetArea() {
    return this->width * this->height;
}

// РЕАЛИЗАЦИЯ для квадрата
Square::Square(double side) : Rectangle(side, side) { }

// РЕАЛИЗАЦИЯ для окружности
Circle::Circle(double radius) {
    this->radius = radius;
}

double Circle::GetArea() {
    return 3.14159265358979323 * this->radius * this->radius;
}
```

Обратите внимание на процедуру `main()`, где в одну переменную с типом "геометрическая фигура" записывается сначала прямоугольник, затем квадрат, а затем окружность.

> [В статье на хабре](https://habr.com/ru/post/87205/) описаны и другие примеры инкапсуляции, полиморфизма и наследования.
> Рекомендую также ознакомиться.

## Задание

В этой лабораторной работе **нет вариантов** выполнения.

Всем студентам предлагается:
* описать базовый класс Животное (`Animal`), у которого будут виртуальные методы "говорить", "пить" и "двигаться". В базовом классе реализация методов - вывод на экран прочерка.
* от этого класса унаследовать 3 класса: Змея (`Snake`), Собака (`Dog`) и Кошка (`Cat`).
* у всех классов необходимо хранить и принимать в конструкторе кличку.
* у каждого класса должна быть своя реализация методов (вывод на экран, как животное делает то или иное действие: например, змея ползает, собака лакает, кошка мяукает и т.д.)
* в главном классе по аналогии с примером создать 3 переменные типа `Animal *`, инстанцировать в них 3 экземпляра различных животных (придумайте клички самостоятельно) и продемонстрировать принципы полиморфизма (например, животное "собака Белка" должно гавкать, а животное "змея Генрих" должно шипеть).

## Ссылки

* [Лабораторная работа №5](../cw05/README.md)
* [Лабораторная работа №7](../cw07/README.md)