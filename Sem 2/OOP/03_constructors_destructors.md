# 03. Конструктори и деструктори

## Какво е конструктор?

- Специална член-функция на класа
- Извиква се автоматично при създаване на обект
- Има същото име като класа
- Няма тип за връщане
- Цел: Инициализира състоянието на обекта и при нужда заделя ресурси

```cpp
class Student {
public:
    Student() { // constructor
    }
};
```

- Инициализира данните на обекта
- Гарантира валидно начално състояние
- Изпълнява логика при създаване
- Конструктор без параметри се нарича дефолтен конструктор

## Лоши примери за инициализация

- Забравена инициализация:

```cpp
class Point {
public:
    int x, y;
    Point() {
        // нищо не се прави
    }
};
```

- Двустъпкова инициализация:
```cpp
class Person {
    std::string name;
public:
    Person(std::string n) {
        name = n; // първо празен string, после присвояване
    }
};
```

- Неизползване на конструктор:
```cpp
class Student {
    std::string name;
    double grade;
public:
    Student() { }
    void setGrade(double value) { grade = value; }
};

int main() {
    Student s;
    s.setGrade(4.50);
    return 0;
}
```
- `name` не е инициализирана
- Създава се празен обект и после се пълни с данни

## Конструктор с параметри
- Грешен начин:
```cpp
class Student {
    std::string name_;
    double grade_;
public:
    Student(const std::string& name, double grade) {
        name_ = name;
        grade_ = grade;
    }
};
```
- Две операции: първо се създават празни name и grade, после се инициализират

- Правилен начин: Инициализиращ списък
```cpp
Student(const std::string& name, double grade)
    : name_(name), grade_(grade) {}
```

Предимства:
- По-бързо
- Задължително за const и референции
- По-четим код
- Спестява се невидимо копие

## Кога инициализиращият списък е задължителен
1. Константна променлива:
```cpp
class Student {
    const int id_;
public:
    Student(int id) : id_(id) {} // MANDATORY
};
```

1. Член данни от тип референция:
```cpp
class Wrapper {
    int& ref_;
public:
    Wrapper(int& x) : ref_(x) {} // MANDATORY
};
```

1. Класове без дефолтен конструктор:
```cpp
class FacultyNumber {
    int value_;
public:
    FacultyNumber(int v) : value_(v) {}
};

class Student {
    FacultyNumber fn_;
public:
    Student(int fn) : fn_(fn) {} // MANDATORY
};
```

## Конструктор - допълнение
1. Ред на инициализацията:
```cpp
class Example {
    int x_;
    int y_;
public:
    Example() : y_(2), x_(1) {} // x_ се инициализира първи!
};
```
- Редът на инициализацията винаги отговаря на реда, в който са декларирани член данните на класа, а не реда, в който се извикват в конструктора

1. Дефолтен конструктор:
```cpp
class Student {
public:
    Student() : name_(""), grade_(2.0) {}
};

Student s;
```
- Конструкторът без аргументи е отговорен за първоначалната инициализация на обекта
- Забележка: При дефиниция на друг конструктор, default не се генерира автоматично

## Destructor
- Функция, която се извиква автоматично при унищожаване на обекта
- Освобождава ресурси
- Name: `~ClassName()`

```cpp
class Student {
public:
    ~Student() {
        // cleanup
    }
};
```

Извиква се при следните ситуации:
1. Край на scope
```cpp
{
    Student s("Ivan", 5.50);
} // тук се извиква деструктор
```

1. `delete` или `delete[]` - обектът може да бъде унищожен по преценка на програмиста
1. Унищожаване на временни обекти

## Деструктор - продължение
```cpp
class Array {
    int* data_;
public:
    Array(int n) {
        data_ = new int[n];
    }
    ~Array() {
        delete[] data_;
    }
};
```
- Важно е да се използва `delete[]` иначе - memory leak!

Лоши практики:
- Деструкторът НЕ трябва да:
  - Хвърля exceptions
  - Съдържа сложна логика - възможно е да не се изпълни цялата
  - Разчита на други вече унищожени обекти
  - Се извиква в кода - почти никога не трябва да се вика той, а при нужда `delete`/`delete[]`

## Ред на извикване на деструкторите
```cpp
class A { ~A(){ std::println("A"); } };
class B { ~B(){ std::println("B"); } };
class C {
    A a_;
    B b_;
public:
    ~C(){ std::println("C"); }
};
// Изход: CBA
// Ред на създаване: ABC!
// Ред на деструкторите: обратен на конструкторите
```

Обяснение:
- Деструкторите се извикват в обратен ред на създаване, за да се гарантира безопасно разрушаване на зависимостите
- Следва принципа LIFO

## Неопределено поведение
- Стандартът на C++ не дефинира какво трябва да се случи
- Компилаторът няма задължение да:
  - Даде грешка
  - Даде предупреждение
  - Се държи предвидимо
- Програмата може да:
  - Работи "нормално"
  - Крашне
  - Дава грешни резултати

Примери:
```cpp
class A {
public:
    ~A() { std::println("Destroyed"); }
};

void foo() {
    A* a = new A();
    delete a;
    a->~A(); // UB
}

int* p = new int(5);
delete p;
std::print(*p); // UB
```

## Указател `this`
- `this` е указател към текущия обект (напр. `Student*`)
- `*this` е самият обект (`Student&`)

Защо ни трябва?
- Разграничаване на членове и параметри
- Връщане на текущия обект
- Method chaining

Example:
```cpp
class Student {
private:
    std::string name;
public:
    Student(const std::string& n) : name(n) {}
    Student& setName(const std::string& n) {
        name = n;
        return *this; // връщаме текущия обект
    }
    void print() const {
        std::println(name);
    }
};

// Използване: s.setName("Petar").print(); // chaining
```

## Mistakes with `this`
- Returning temporary object:
```cpp
Student& setGrade(double g) {
    Student tmp;
    tmp.grade_ = g;
    return tmp; // dangling reference
}
```

- Returning `this` instead of `*this`:
```cpp
Student& setGrade(double g) {
    return this; // type error
}
```

- Const method modifying object:
```cpp
class Student {
    double grade_;
public:
    Student& setGrade(double g) const {
        grade_ = g; // cannot – method is const
        return *this;
    }
};
```

## Converting Constructors
- Constructor with one parameter allowing conversion from another type to class type

```cpp
class Temperature {
    double celsius_;
public:
    Temperature(double c) : celsius_(c) {}
};

Temperature t = 36.6;
```

Good practice example:
```cpp
class Meters {
    double value_;
public:
    Meters(double m) : value_(m) {}
};

Meters distance = 5.0; // natural
```

## Bad Examples of Converting Constructors
```cpp
class Student {
public:
    Student(int fn) {}
};

Student s = 12345; // unexpected

void printStudent(Student s);
printStudent(12345); // 12345 → creates Student object

void process(Student s);
process(5); // Student(5)

class Number {
    double value_;
public:
    Number(int v) : value_(v) {}
    Number(double v) : value_(v) {}
};

void print(Number n) {}

int main() {
    print(5); // ambiguous? may choose int or double
}
```

## Explicit Constructors
- Constructor marked with `explicit` keyword
- Prohibits implicit conversions

```cpp
class Temperature {
    double celsius_;
public:
    explicit Temperature(double c) : celsius_(c) {}
};

Temperature t1(36.6); // OK
Temperature t2 = 36.6; // DOES NOT COMPILE!
```

Best practices:
1. Use `explicit` by default
2. Use implicit only if conversion is natural and no confusion risk
3. Libraries like STL always use `explicit`

## Arrays of Objects
- Array of objects = sequence of class instances
- For each element called:
  - Constructor when creating
  - Destructor when destroying

Key principles:
1. Constructors called in index order
2. Destructors called in reverse order
3. Applies to both static and dynamic arrays

Example:
```cpp
class A {
public:
    A() { std::println("A()"); }
    ~A() { std::println("~A()"); }
};

int main() {
    A arr[3];
}
```
Output:
```
A()
A()
A()
~A()
~A()
~A()
```

## Dynamic Arrays of Objects
- Array whose size determined at runtime, memory allocated in heap

Example:
```cpp
class C {
public:
    C() { std::cout << "C()\n"; }
    ~C() { std::cout << "~C()\n"; }
};

int main() {
    C* arr = new C[3];
    delete[] arr;
}
```
Output: C(), C(), C(), ~C(), ~C(), ~C()

Common mistakes:
1. `C* arr = new C[3];` // memory leak + uncalled destructors due to forgotten `delete[]`
2. `delete arr;` // Undefined behavior
3. `D arr[5];` // compilation error due to missing default constructor

## Presentation
[Constructor and Destructor](https://github.com/ttsonkov/OOP_FMI/blob/main/Lecture%2003%20-%20Constructor%20and%20destructor.pdf)