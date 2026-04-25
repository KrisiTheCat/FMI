# Лекция 2: Енкапсулация. Абстракция. Работа с потоци.

- [Презентация](https://github.com/ttsonkov/OOP_FMI/blob/main/Lecture%2002%20-%20Abstraction%20and%20encapsulation.pdf)

## Енкапсулация

- Скриване на вътрешното състояние (data hiding) чрез private полета.
- Предоставяне на контролирани методи за достъп (get/set).
- Повишава надеждността и поддръжката.

Пример:

```cpp
class Temperature {
private:
    double celsius_;
public:
    Temperature(double c) : celsius_(c) {}
    double getCelsius() const { return celsius_; }
    double getFahrenheit() const { return celsius_ * 9.0 / 5.0 + 32.0; }
    void setCelsius(double c) { celsius_ = c; }
};
```

## Абстракция

- Скриване на сложността, работа с високо ниво на концепции.
- Позволява повторна употреба и модулност.

Разлика с енкапсулация:
- Енкапсулация: крие член-променливите (private полета).
- Абстракция: крие несъществените детайли (как работи, показва какво прави).

## Константност в C++

- `const` член-функции: не променят състоянието на обекта.
- `const` обекти: могат да извикват само const функции.

Пример:

```cpp
class Point {
public:
    int getX() const { return x; }  // const function
};

const Point p(1, 2);
std::cout << p.getX();  // OK
```

- `mutable`: позволява промяна на членове в const функции (за кеширане, броене).

```cpp
class Counter {
    mutable int count = 0;
public:
    void increment() const { count++; }
};
```

## Потоци и работа с файлове

- `#include <iostream>`: std::cin, std::cout, std::cerr
- `#include <fstream>`: std::ifstream (четене), std::ofstream (писане), std::fstream (двете)

Режими: std::ios::in, std::ios::out, std::ios::app, etc.

Пример:

```cpp
std::ofstream out("file.txt");
out << "Hello" << std::endl;
out.close();

std::ifstream in("file.txt");
std::string line;
while (std::getline(in, line)) {
    std::cout << line << std::endl;
}
```

Добри практики: проверка дали е отворен, затваряне на файлове.

## std::string

- Динамичен контейнер за символи, автоматично управление на паметта.
- Основни операции: конструиране, размер (size()), добавяне (+=), вмъкване (insert), изтриване (erase), търсене (find), подстринг (substr).

Примери:
- `std::string s = "Hello";`
- `s += " World";`
- `s.find("lo")` → позиция

## std::print и std::format (C++20)

- `std::format`: форматира в string с placeholders {}.
- `std::print`: директно извежда форматирани данни.

Примери:
- `std::format("{:.2f}", 3.14)` → "3.14"
- `std::println("a = {}, b = {}", a, b);`

Сравнение с std::cout:
- std::print е по-бърз и четим, особено с много аргументи.
- За нови проекти: предпочитай std::print.