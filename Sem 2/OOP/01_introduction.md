# Въведение в ООП

- [Презентация](https://github.com/ttsonkov/OOP_FMI/blob/main/Lecture%2001%20-%20Introduction%20to%20OOP.pdf)

## Видове памет

- **Stack**: Локални променливи, автоматично управление (LIFO), бързо.
- **Heap**: Динамична памет (new/delete), ръчно управление, голям обем.
- **Глобална**: Статични/глобални променливи.
- **Program Code**: Компилиран код.

## Процедурно програмиране

- Данни и функции отделени, логика в много функции.
- Проблеми: липса на контрол на достъпа, трудна поддръжка.

Пример:

```cpp
struct BankAccount { double balance; };
void deposit(BankAccount& acc, double amt) { acc.balance += amt; }
int main() { BankAccount acc{100.0}; deposit(acc, 50); }
```

## Обектно-ориентирано програмиране

- Програмна парадигма: системи като набор от обекти, които взаимодействат.
- Основни идеи: данни + поведение заедно, контрол на достъпа, модел на реалния свят.

Пример:

```cpp
struct BankAccount {
    double balance;
    void deposit(double amt) { balance += amt; }
};
int main() { BankAccount acc{100.0}; acc.deposit(50); }
```

## Предимства на ООП

- Енкапсулация, модулност, повторна употреба, по-лесна поддръжка, подходящо за големи системи.

## class и struct

- `class`: по подразбиране private членове.
- `struct`: по подразбиране public членове.

Пример:

```cpp
class Point {
private:
    int x, y;
public:
    Point(int a, int b) : x(a), y(b) {}
};
```

## Енумерации

- Ограничен набор от стойности.

```cpp
enum Color { Red, Green, Blue };
enum class Status { OK = 200, Error = 500 };
```

## Наименовани пространства (namespaces)

- Организиране на декларации, избягване на конфликти.

```cpp
namespace A { void print(); }
namespace B { void print(); }
```

## Динамични обекти

- Създаване с `new`, унищожаване с `delete`.

Пример:

```cpp
Person* p = new Person("Todor");
p->greet();
delete p;
```

## Влагане на обекти (composition)

- Клас съдържа обект от друг клас.

```cpp
class Car {
    Engine engine_;
public:
    void print() { engine_.print(); }
};
```

## Цялостен пример

```cpp
#include <print>
#include <string>

struct Student {
    std::string name;
    int age;
    double grade;
    void printInfo() {
        std::print("Име: {}\n", name);
        std::print("Възраст: {}\n", age);
        std::print("Оценка: {}\n", grade);
    }
};

int main() {
    Student s{"Ivan", 20, 5.50};
    s.printInfo();
}
```
