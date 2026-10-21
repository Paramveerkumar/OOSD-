# 📘  Namespaces in C++

> **Goal of this lesson:** Master namespaces — how to create them, access their members, use `using` safely, nest them, alias them, split them across files, and avoid the name conflicts that break large programs.

> 🔗 *Lesson 3 gave a quick introduction to namespaces. This lesson goes much deeper.*

---

## 📑 Table of Contents

1. [The Problem: Name Collisions](#1-the-problem-name-collisions)
2. [What is a Namespace?](#2-what-is-a-namespace)
3. [Creating a Namespace](#3-creating-a-namespace)
4. [Accessing Members: The Scope Resolution Operator `::`](#4-accessing-members-the-scope-resolution-operator-)
5. [The `using` Declaration](#5-the-using-declaration)
6. [The `using` Directive](#6-the-using-directive)
7. [Where You Put `using` Matters](#7-where-you-put-using-matters)
8. [Ambiguity Errors](#8-ambiguity-errors)
9. [The Global Namespace](#9-the-global-namespace)
10. [Nested Namespaces](#10-nested-namespaces)
11. [Namespace Aliases](#11-namespace-aliases)
12. [Extending a Namespace](#12-extending-a-namespace)
13. [Unnamed (Anonymous) Namespaces](#13-unnamed-anonymous-namespaces)
14. [Inline Namespaces (Advanced)](#14-inline-namespaces-advanced)
15. [Namespaces Across Multiple Files](#15-namespaces-across-multiple-files)
16. [The `std` Namespace](#16-the-std-namespace)
17. [Best Practices](#17-best-practices)
18. [Common Beginner Mistakes](#18-common-beginner-mistakes)
19. [Practice Exercises](#19-practice-exercises)
20. [Quick Quiz](#20-quick-quiz)
21. [Summary](#21-summary)

---

## 1. The Problem: Name Collisions

Imagine you're building a project and use two libraries:

```cpp
// From a Graphics library
void draw() { /* draws a shape */ }

// From a Card Game library
void draw() { /* draws a card from the deck */ }
```

Both define a function called `draw()`. When you include both:

```
error: redefinition of 'void draw()'
```

This is a **name collision** (also called a **naming conflict**). In large projects with thousands of functions and many libraries, collisions are almost guaranteed.

🏙️ **Analogy:** There are many streets called **"MG Road"** in India. A letter addressed only to *"MG Road"* is useless. You need **"MG Road, Bengaluru"** or **"MG Road, Pune"**. The **city** is the namespace!

---

## 2. What is a Namespace?

A **namespace** is a **named region (scope)** that groups related identifiers — variables, functions, classes, and more — and keeps them separate from identifiers in other namespaces.

```
┌───────── Graphics ─────────┐     ┌───────── CardGame ─────────┐
│  void draw()               │     │  void draw()               │
│  int width                 │     │  int deckSize              │
│  class Shape               │     │  class Card                │
└────────────────────────────┘     └────────────────────────────┘
        Graphics::draw()                  CardGame::draw()
              ✅ No conflict — different full names ✅
```

### What Can Go Inside a Namespace?

| ✅ Allowed | Example |
|-----------|---------|
| Variables | `int count = 0;` |
| Constants | `const double PI = 3.14159;` |
| Functions | `void greet() { }` |
| Classes / Structs | `class Student { };` |
| Enums | `enum Color { RED, GREEN };` |
| Other namespaces | `namespace Inner { }` |

---

## 3. Creating a Namespace

### Syntax

```cpp
namespace NamespaceName {
    // declarations and definitions
}
```

📌 **No semicolon** after the closing brace `}` (unlike `class` and `struct`).

### Example

```cpp
#include <iostream>
using namespace std;

namespace Graphics {
    int width = 800;

    void draw() {
        cout << "Drawing a shape on screen" << endl;
    }
}

namespace CardGame {
    int deckSize = 52;

    void draw() {
        cout << "Drawing a card from the deck" << endl;
    }
}

int main() {
    Graphics::draw();
    CardGame::draw();

    cout << "Screen width: " << Graphics::width << endl;
    cout << "Deck size   : " << CardGame::deckSize << endl;
    return 0;
}
```

**Output:**
```
Drawing a shape on screen
Drawing a card from the deck
Screen width: 800
Deck size   : 52
```

### Rules

| Rule | Example |
|------|---------|
| Must be declared at **global scope** or **inside another namespace** | ✅ `namespace A { }` at file level |
| **Cannot** be declared inside a function | ❌ `int main() { namespace X { } }` |
| Name follows identifier rules | ✅ `MathUtils`, ❌ `2D_Graphics` |
| Can be opened **multiple times** (see Section 12) | |

---

## 4. Accessing Members: The Scope Resolution Operator `::`

The **scope resolution operator `::`** tells the compiler **which namespace** a name belongs to.

```
   NamespaceName :: memberName
        │          │
   "look inside"   "find this"
```

```cpp
#include <iostream>

namespace Bank {
    double interestRate = 6.5;

    double calculateInterest(double amount) {
        return amount * interestRate / 100;
    }
}

int main() {
    std::cout << "Rate: " << Bank::interestRate << "%" << std::endl;
    std::cout << "Interest on 10000: " << Bank::calculateInterest(10000) << std::endl;

    Bank::interestRate = 7.0;     // namespace variables can be modified
    std::cout << "New rate: " << Bank::interestRate << "%" << std::endl;
    return 0;
}
```

**Output:**
```
Rate: 6.5%
Interest on 10000: 650
New rate: 7%
```

💡 Inside its own namespace, a member can use other members **without** the prefix — notice `calculateInterest` uses `interestRate` directly.

---

## 5. The `using` Declaration

Writing `Bank::` again and again gets tiring. A **`using` declaration** brings **one specific name** into the current scope.

### Syntax
```cpp
using NamespaceName::memberName;
```

### Example

```cpp
#include <iostream>
using std::cout;     // bring in only cout
using std::endl;     // bring in only endl

namespace MathUtils {
    int square(int n) { return n * n; }
    int cube(int n)   { return n * n * n; }
}

using MathUtils::square;   // bring in only square

int main() {
    cout << square(5) << endl;           // ✅ no prefix needed
    cout << MathUtils::cube(3) << endl;  // cube still needs the prefix
    // cout << cube(3);                  // ❌ error: cube not declared in this scope
    return 0;
}
```

**Output:**
```
25
27
```

✅ **Advantage:** You know exactly which names you brought in — safe and clear.

---

## 6. The `using` Directive

A **`using` directive** makes **all names** from a namespace available without a prefix.

### Syntax
```cpp
using namespace NamespaceName;
```

### Example

```cpp
#include <iostream>
using namespace std;

namespace Physics {
    const double GRAVITY = 9.8;
    const double SPEED_OF_LIGHT = 299792458;

    double weight(double mass) {
        return mass * GRAVITY;
    }
}

using namespace Physics;   // everything from Physics is now available

int main() {
    cout << "Gravity: " << GRAVITY << " m/s^2" << endl;
    cout << "Light  : " << SPEED_OF_LIGHT << " m/s" << endl;
    cout << "Weight of 60 kg: " << weight(60) << " N" << endl;
    return 0;
}
```

### `using` Declaration vs `using` Directive

| | `using` Declaration | `using` Directive |
|--|--------------------|-------------------|
| Syntax | `using std::cout;` | `using namespace std;` |
| Brings in | **One** name | **All** names |
| Risk of conflicts | Low ✅ | Higher ⚠️ |
| Clarity | High — you see what's used | Lower — hard to tell where a name comes from |
| Analogy | Borrowing **one book** from a library | Dumping the **entire library** on your desk |

---

## 7. Where You Put `using` Matters

A `using` statement only works **within the scope where it is written**.

```cpp
#include <iostream>

namespace Greetings {
    void hello() { std::cout << "Hello!" << std::endl; }
}

void functionA() {
    using namespace Greetings;   // only active inside functionA
    hello();                     // ✅
}

void functionB() {
    // hello();                  // ❌ error: not visible here
    Greetings::hello();          // ✅ must use full name
}

int main() {
    functionA();
    functionB();
    return 0;
}
```

### Scope Levels (from widest to narrowest)

```
🌍 File level (global)   → affects the whole file after that line    ⚠️ widest
   🏠 Function level     → affects only that function               ✅ safer
      📦 Block level     → affects only that { } block               ✅ safest
```

💡 **Best practice:** put `using` statements in the **smallest scope** that makes sense — inside functions rather than at the top of the file in larger programs.

⛔ **Never** write `using namespace ...;` in a **header file (.h)**. Every file that includes your header is forced to accept it, which can cause conflicts you can't easily fix.

---

## 8. Ambiguity Errors

When two `using` directives bring in the **same name**, the compiler can't decide which one you mean.

```cpp
#include <iostream>
using namespace std;

namespace English {
    void greet() { cout << "Hello!" << endl; }
}

namespace French {
    void greet() { cout << "Bonjour!" << endl; }
}

using namespace English;
using namespace French;

int main() {
    // greet();          // ❌ error: call of overloaded 'greet()' is ambiguous
    English::greet();    // ✅ fix: use the full name
    French::greet();     // ✅
    return 0;
}
```

📌 **Important:** the error appears only when you **use** the ambiguous name, not when you write the `using` lines.

### A Real-World Ambiguity with `std`

```cpp
#include <iostream>
#include <algorithm>
using namespace std;

int count = 0;   // our own global variable

int main() {
    count++;     // ❌ error: reference to 'count' is ambiguous
                 //    (std::count is a function in <algorithm>!)
    return 0;
}
```

This is exactly why professionals are careful with `using namespace std;`. Other names that commonly clash: `max`, `min`, `distance`, `size`, `data`, `swap`.

**Fixes:**
```cpp
::count++;                 // fix 1: refer to the global one explicitly
// or rename it: int visitorCount = 0;          // fix 2: better name
// or don't use `using namespace std;`          // fix 3
```

---

## 9. The Global Namespace

Anything declared **outside all namespaces** belongs to the **global namespace**. It has no name, so you access it with `::` and **nothing before it**.

```cpp
#include <iostream>
using namespace std;

int value = 100;          // global namespace

namespace Config {
    int value = 50;
}

int main() {
    int value = 5;        // local variable

    cout << "Local  : " << value << endl;          // 5
    cout << "Global : " << ::value << endl;        // 100
    cout << "Config : " << Config::value << endl;  // 50
    return 0;
}
```

**Output:**
```
Local  : 5
Global : 100
Config : 50
```

### Name Lookup Order
When you write a plain name, the compiler searches from the **innermost scope outward**:

```
1. Current block { }
2. Enclosing blocks
3. Function scope
4. Enclosing namespace(s)
5. Global namespace
   (plus anything made visible with using)
```

The **first match wins** — an inner name **hides** an outer one with the same name.

---

## 10. Nested Namespaces

A namespace can contain **another namespace** — useful for organizing large libraries.

### Classic Syntax

```cpp
#include <iostream>
using namespace std;

namespace Company {
    namespace HR {
        void hire() { cout << "Hiring a new employee" << endl; }
    }

    namespace Finance {
        void payroll() { cout << "Processing payroll" << endl; }
    }
}

int main() {
    Company::HR::hire();
    Company::Finance::payroll();
    return 0;
}
```

### C++17 Compact Syntax ✅

```cpp
namespace Company::HR {
    void hire() { std::cout << "Hiring a new employee" << std::endl; }
}

namespace Company::Finance {
    void payroll() { std::cout << "Processing payroll" << std::endl; }
}
```

Both styles produce the same result. Compile with `-std=c++17` (or later) for the compact form:
```bash
g++ -std=c++17 company.cpp -o company
```

### Hierarchy View

```
Company
├── HR
│   └── hire()
└── Finance
    └── payroll()
```

💡 Inner namespaces can use names from outer namespaces **without** a prefix:

```cpp
namespace App {
    const string VERSION = "1.0";

    namespace UI {
        void showVersion() {
            cout << "Version " << VERSION << endl;   // ✅ finds App::VERSION
        }
    }
}
```

---

## 11. Namespace Aliases

Long or deeply nested names can be given a **short nickname** (alias).

### Syntax
```cpp
namespace ShortName = Long::Nested::Name;
```

### Example

```cpp
#include <iostream>
using namespace std;

namespace University {
    namespace Engineering {
        namespace ComputerScience {
            void showCourse() { cout << "B.Tech CSE" << endl; }
        }
    }
}

int main() {
    // Without alias 😩
    University::Engineering::ComputerScience::showCourse();

    // With alias 😊
    namespace CSE = University::Engineering::ComputerScience;
    CSE::showCourse();

    return 0;
}
```

### A Real Example from the Standard Library

```cpp
#include <filesystem>
namespace fs = std::filesystem;   // very common alias

fs::path p = "notes.txt";
```

💡 Aliases are **safer** than `using namespace` because they shorten names **without** dumping everything into your scope.

---

## 12. Extending a Namespace

A namespace can be **opened multiple times** — even in different files. All parts merge into **one** namespace.

```cpp
#include <iostream>
using namespace std;

namespace Tools {
    void hammer() { cout << "Hammering" << endl; }
}

// ... other code ...

namespace Tools {   // same namespace opened again — adds more members
    void screwdriver() { cout << "Screwing" << endl; }
}

int main() {
    Tools::hammer();
    Tools::screwdriver();
    return 0;
}
```

📌 This is how the standard library works: `std` is spread across hundreds of headers (`<iostream>`, `<string>`, `<vector>`, ...). Each header **adds** its parts to `std`.

⚠️ You **cannot define the same member twice**:
```cpp
namespace Tools { int count = 1; }
namespace Tools { int count = 2; }   // ❌ error: redefinition of 'Tools::count'
```

---

## 13. Unnamed (Anonymous) Namespaces

A namespace **without a name** makes its members **private to the current file**. Other `.cpp` files can't see them.

```cpp
#include <iostream>
using namespace std;

namespace {   // unnamed namespace
    int secretCounter = 0;

    void helper() {
        secretCounter++;
    }
}

int main() {
    helper();
    helper();
    cout << "Counter: " << secretCounter << endl;   // ✅ accessible in this file
    return 0;
}
```

### Why Use It?
In a multi-file project, two files might each have a helper function named `log()`. If both are global, the **linker** complains:

```
error: multiple definition of 'log()'
```

Wrapping each one in an unnamed namespace makes it **file-private**, so they never clash.

```
  file1.cpp                       file2.cpp
┌────────────────────┐        ┌────────────────────┐
│ namespace {        │        │ namespace {        │
│   void log() {...} │        │   void log() {...} │
│ }                  │        │ }                  │
└────────────────────┘        └────────────────────┘
   only file1 sees it            only file2 sees it   ✅ no conflict
```

💡 This is the modern C++ replacement for marking global functions `static` to hide them in a file.

---

## 14. Inline Namespaces (Advanced)

> 🎓 *Optional / advanced — useful to recognize when reading library code.*

Members of an **inline namespace** are treated as if they belong directly to the **parent namespace**. Libraries use this for **versioning**.

```cpp
#include <iostream>
using namespace std;

namespace Library {
    namespace v1 {
        void connect() { cout << "Connecting (old v1)" << endl; }
    }

    inline namespace v2 {   // the default version
        void connect() { cout << "Connecting (new v2)" << endl; }
    }
}

int main() {
    Library::connect();       // uses v2 automatically (inline)
    Library::v1::connect();   // can still use old version explicitly
    Library::v2::connect();   // or name v2 explicitly
    return 0;
}
```

**Output:**
```
Connecting (new v2)
Connecting (old v1)
Connecting (new v2)
```

📌 When v3 is released, the library makes `v3` inline instead, and all users automatically get the new version.

---

## 15. Namespaces Across Multiple Files

In real projects, **declarations** go in a header inside a namespace, and **definitions** go in a `.cpp` file inside the **same** namespace.

```
shop-project/
├── main.cpp
├── billing.h
└── billing.cpp
```

### 📄 `billing.h`

```cpp
#pragma once
#include <string>

namespace Billing {
    const double GST_RATE = 0.18;

    double addGST(double amount);
    void printBill(const std::string& item, double amount);   // ✅ std:: in headers
}
```

### 📄 `billing.cpp`

```cpp
#include <iostream>
#include "billing.h"

namespace Billing {
    double addGST(double amount) {
        return amount + amount * GST_RATE;
    }

    void printBill(const std::string& item, double amount) {
        std::cout << "Item  : " << item << std::endl;
        std::cout << "Price : " << amount << std::endl;
        std::cout << "Total : " << addGST(amount) << " (incl. GST)" << std::endl;
    }
}
```

> 💡 Alternatively, you can define functions outside the namespace block using full names:
> ```cpp
> double Billing::addGST(double amount) { ... }
> ```

### 📄 `main.cpp`

```cpp
#include "billing.h"

int main() {
    Billing::printBill("Notebook", 100);
    return 0;
}
```

### Compile & Run

```bash
g++ main.cpp billing.cpp -o shop
./shop
```

**Output:**
```
Item  : Notebook
Price : 100
Total : 118 (incl. GST)
```

---

## 16. The `std` Namespace

**`std`** (short for **standard**) contains **everything** in the C++ Standard Library.

| Category | Examples |
|----------|----------|
| Input/Output | `std::cout`, `std::cin`, `std::endl`, `std::cerr` |
| Strings | `std::string`, `std::getline`, `std::to_string` |
| Containers | `std::vector`, `std::map`, `std::set`, `std::array` |
| Algorithms | `std::sort`, `std::max`, `std::min`, `std::swap`, `std::count` |
| Math | `std::sqrt`, `std::pow`, `std::abs` |
| Nested namespaces | `std::filesystem`, `std::chrono`, `std::ranges` |

### ⛔ Don't Add Your Own Code to `std`

```cpp
namespace std {
    void myFunction() { }   // ❌ undefined behaviour — not allowed!
}
```

The `std` namespace is **reserved** for the standard library. Always create your own namespace instead.

### So... Should Students Use `using namespace std;`?

| Situation | Recommendation |
|-----------|----------------|
| Small practice programs, classroom examples | ✅ Fine |
| Competitive programming | ✅ Common |
| Large projects | ⚠️ Prefer `std::` or specific `using` declarations |
| Header files | ⛔ Never |

---

## 17. Best Practices

1. ✅ **Group related code** into meaningful namespaces: `Billing`, `Geometry`, `Network`.
2. ✅ Use **clear names** — often the project or company name at the top level: `MyCompany::Utils`.
3. ✅ Prefer **`using` declarations** (`using std::cout;`) over **`using` directives** in larger code.
4. ✅ Put `using` in the **smallest possible scope**.
5. ✅ Use **namespace aliases** for long nested names.
6. ✅ Use **unnamed namespaces** for helper functions private to one `.cpp` file.
7. ⛔ **Never** put `using namespace` in header files.
8. ⛔ **Never** add your own declarations to `namespace std`.
9. ⚠️ Avoid naming your variables after common std names (`count`, `max`, `min`, `size`) when using `using namespace std;`.

---

## 18. Common Beginner Mistakes

| ❌ Mistake | ✅ Correct | Why |
|-----------|-----------|-----|
| `namespace Math { ... };` | `namespace Math { ... }` | No semicolon needed (harmless, but unnecessary) |
| Declaring a namespace inside `main()` | Declare at file level | Namespaces can't be inside functions |
| `Math.square(5)` | `Math::square(5)` | Use `::`, not `.` |
| `using Math::*;` | `using namespace Math;` | `*` wildcard doesn't exist in C++ |
| `using namespace std` | `using namespace std;` | Missing semicolon |
| Two `using` directives with same names, then calling the name | Use full names `A::f()` | Ambiguity error |
| `int count;` with `using namespace std;` + `<algorithm>` | Rename or use `::count` | Clashes with `std::count` |
| Defining the same member twice when extending a namespace | Define each member once | Redefinition error |
| `using namespace std;` in a `.h` file | Write `std::` in headers | Pollutes every including file |
| Forgetting to wrap definitions in the namespace in `.cpp` | `namespace X { ... }` or `X::func()` | Otherwise it defines a **different**, global function → linker error |

---

## 19. Practice Exercises

### 🟢 Easy

**Exercise 1:** Create a namespace `School` containing a variable `name` (string) and a function `display()` that prints the school name. Call it from `main()`.

**Exercise 2:** Create two namespaces `Celsius` and `Fahrenheit`, each with a function `boilingPoint()` that returns the boiling point of water in that scale (100 and 212). Print both.

**Exercise 3:** Rewrite the following using **`using` declarations** only (not `using namespace std;`):
```cpp
#include <iostream>
#include <string>

int main() {
    std::string city;
    std::cout << "Enter city: ";
    std::cin >> city;
    std::cout << "You live in " << city << std::endl;
    return 0;
}
```

### 🟡 Medium

**Exercise 4:** Predict the output:
```cpp
#include <iostream>
using namespace std;

int x = 10;

namespace A {
    int x = 20;
}

int main() {
    int x = 30;
    cout << x << " " << ::x << " " << A::x << endl;
    return 0;
}
```

**Exercise 5:** Create nested namespaces `Shapes::TwoD` (with `circleArea(r)`) and `Shapes::ThreeD` (with `sphereVolume(r)`). Use the C++17 compact syntax. Create an alias for each and call both functions.

**Exercise 6:** The code below does not compile. Explain why and fix it **in two different ways**.
```cpp
#include <iostream>
using namespace std;

namespace Cat { void speak() { cout << "Meow" << endl; } }
namespace Dog { void speak() { cout << "Woof" << endl; } }

using namespace Cat;
using namespace Dog;

int main() {
    speak();
    return 0;
}
```

**Exercise 7:** Create a namespace `Counter` with a variable `value = 0` and functions `increment()`, `decrement()`, and `reset()`. Test all three from `main()`.

### 🔴 Challenge

**Exercise 8:** Build a **multi-file** project:
- `calculator.h` → namespace `Calc` with prototypes for `add`, `subtract`, `multiply`, `divide`
- `calculator.cpp` → definitions inside namespace `Calc`; include a **file-private** helper `isZero(double)` in an **unnamed namespace** that `divide` uses to avoid dividing by zero
- `main.cpp` → takes two numbers as input and prints all four results

**Exercise 9:** Create a namespace `App` that you **open three separate times**, adding `version()`, `author()`, and `about()` (where `about()` calls the other two). Call `App::about()`.

**Exercise 10:** Using an **inline namespace**, create `Greeter::v1::hello()` (prints "Hi") and `Greeter::v2::hello()` (prints "Hello, welcome!"), making v2 the default. Show that `Greeter::hello()` calls v2.

---

### ✅ Solutions

<details>
<summary>Click to view Exercise 1 solution</summary>

```cpp
#include <iostream>
#include <string>
using namespace std;

namespace School {
    string name = "Greenwood Public School";

    void display() {
        cout << "School: " << name << endl;
    }
}

int main() {
    School::display();
    return 0;
}
```
</details>

<details>
<summary>Click to view Exercise 2 solution</summary>

```cpp
#include <iostream>
using namespace std;

namespace Celsius {
    double boilingPoint() { return 100; }
}

namespace Fahrenheit {
    double boilingPoint() { return 212; }
}

int main() {
    cout << "Boiling point (C): " << Celsius::boilingPoint() << endl;
    cout << "Boiling point (F): " << Fahrenheit::boilingPoint() << endl;
    return 0;
}
```
</details>

<details>
<summary>Click to view Exercise 3 solution</summary>

```cpp
#include <iostream>
#include <string>
using std::cout;
using std::cin;
using std::endl;
using std::string;

int main() {
    string city;
    cout << "Enter city: ";
    cin >> city;
    cout << "You live in " << city << endl;
    return 0;
}
```
</details>

<details>
<summary>Click to view Exercise 4 answer</summary>

**Output:**
```
30 10 20
```
- `x` → local variable (innermost scope wins) = **30**
- `::x` → global namespace = **10**
- `A::x` → namespace A = **20**
</details>

<details>
<summary>Click to view Exercise 5 solution</summary>

```cpp
#include <iostream>
using namespace std;

namespace Shapes::TwoD {
    double circleArea(double r) {
        return 3.14159 * r * r;
    }
}

namespace Shapes::ThreeD {
    double sphereVolume(double r) {
        return (4.0 / 3.0) * 3.14159 * r * r * r;
    }
}

int main() {
    namespace S2 = Shapes::TwoD;
    namespace S3 = Shapes::ThreeD;

    cout << "Circle area (r=3)  : " << S2::circleArea(3) << endl;
    cout << "Sphere volume (r=3): " << S3::sphereVolume(3) << endl;
    return 0;
}
```
Compile with: `g++ -std=c++17 shapes.cpp -o shapes`
</details>

<details>
<summary>Click to view Exercise 6 solution</summary>

**Why it fails:** both `Cat` and `Dog` bring `speak()` into scope, so the call `speak()` is **ambiguous**.

**Fix 1 — use full names:**
```cpp
int main() {
    Cat::speak();
    Dog::speak();
    return 0;
}
```

**Fix 2 — remove one directive (or use a `using` declaration for the one you want):**
```cpp
using Cat::speak;   // only Cat's speak is brought in

int main() {
    speak();        // Meow
    Dog::speak();   // Woof
    return 0;
}
```
(Remove both `using namespace Cat;` and `using namespace Dog;` lines.)
</details>

<details>
<summary>Click to view Exercise 7 solution</summary>

```cpp
#include <iostream>
using namespace std;

namespace Counter {
    int value = 0;

    void increment() { value++; }
    void decrement() { value--; }
    void reset()     { value = 0; }
}

int main() {
    Counter::increment();
    Counter::increment();
    Counter::increment();
    cout << "After 3 increments: " << Counter::value << endl;   // 3

    Counter::decrement();
    cout << "After 1 decrement : " << Counter::value << endl;   // 2

    Counter::reset();
    cout << "After reset       : " << Counter::value << endl;   // 0
    return 0;
}
```
</details>

<details>
<summary>Click to view Exercise 8 solution</summary>

**calculator.h**
```cpp
#pragma once

namespace Calc {
    double add(double a, double b);
    double subtract(double a, double b);
    double multiply(double a, double b);
    double divide(double a, double b);
}
```

**calculator.cpp**
```cpp
#include <iostream>
#include "calculator.h"

namespace {   // private to this file
    bool isZero(double n) {
        return n == 0;
    }
}

namespace Calc {
    double add(double a, double b)      { return a + b; }
    double subtract(double a, double b) { return a - b; }
    double multiply(double a, double b) { return a * b; }

    double divide(double a, double b) {
        if (isZero(b)) {
            std::cout << "Error: division by zero! ";
            return 0;
        }
        return a / b;
    }
}
```

**main.cpp**
```cpp
#include <iostream>
#include "calculator.h"
using namespace std;

int main() {
    double a, b;
    cout << "Enter two numbers: ";
    cin >> a >> b;

    cout << "Add      : " << Calc::add(a, b) << endl;
    cout << "Subtract : " << Calc::subtract(a, b) << endl;
    cout << "Multiply : " << Calc::multiply(a, b) << endl;
    cout << "Divide   : " << Calc::divide(a, b) << endl;
    return 0;
}
```

**Compile:**
```bash
g++ main.cpp calculator.cpp -o calc
./calc
```
> 💡 The `if` statement is covered in a later lesson — here it simply checks whether `b` is zero.
</details>

<details>
<summary>Click to view Exercise 9 solution</summary>

```cpp
#include <iostream>
#include <string>
using namespace std;

namespace App {
    string version() { return "2.1.0"; }
}

namespace App {
    string author() { return "Student Developer"; }
}

namespace App {
    void about() {
        cout << "Version: " << version() << endl;
        cout << "Author : " << author() << endl;
    }
}

int main() {
    App::about();
    return 0;
}
```
</details>

<details>
<summary>Click to view Exercise 10 solution</summary>

```cpp
#include <iostream>
using namespace std;

namespace Greeter {
    namespace v1 {
        void hello() { cout << "Hi" << endl; }
    }

    inline namespace v2 {
        void hello() { cout << "Hello, welcome!" << endl; }
    }
}

int main() {
    Greeter::hello();       // Hello, welcome!  (v2 is default)
    Greeter::v1::hello();   // Hi
    return 0;
}
```
</details>

---

## 20. Quick Quiz

**Q1.** What is the main purpose of a namespace?
- a) Speed up the program
- b) Prevent name collisions
- c) Allocate memory
- d) Replace functions

**Q2.** Which operator accesses a member of a namespace?
- a) `.`
- b) `->`
- c) `::`
- d) `:`

**Q3.** Which statement brings in **only one** name?
- a) `using namespace std;`
- b) `using std::cout;`
- c) `#include <iostream>`
- d) `namespace std;`

**Q4.** Where can a namespace **NOT** be declared?
- a) At global scope
- b) Inside another namespace
- c) Inside a function
- d) In a header file

**Q5.** What does `::value` refer to?
- a) A local variable
- b) A variable in `std`
- c) A variable in the global namespace
- d) An error

**Q6.** What does an unnamed namespace do?
- a) Makes members visible to all files
- b) Makes members private to the current file
- c) Creates a constant
- d) Nothing — it's invalid

**Q7.** What is `namespace fs = std::filesystem;`?
- a) A using directive
- b) A namespace alias
- c) A nested namespace
- d) A new namespace

**Q8.** Which C++17 syntax creates a nested namespace?
- a) `namespace A.B { }`
- b) `namespace A::B { }`
- c) `namespace A->B { }`
- d) `nested namespace A B { }`

**Q9.** Where should you **never** write `using namespace std;`?
- a) In `main()`
- b) In a small practice program
- c) In a header file
- d) In a function

**Q10.** Two `using` directives bring in functions with the same name. When does the error appear?
- a) At the `using` lines
- b) When the ambiguous name is used
- c) At runtime
- d) Never

<details>
<summary>✅ Click to view answers</summary>

| Q | Answer | Explanation |
|---|--------|-------------|
| 1 | b) | Namespaces keep names separate |
| 2 | c) `::` | Scope resolution operator |
| 3 | b) | A `using` declaration brings in one name |
| 4 | c) | Namespaces can't be declared inside functions |
| 5 | c) | `::` with nothing before it = global namespace |
| 6 | b) | Members are visible only within that file |
| 7 | b) | It creates a short nickname |
| 8 | b) | `namespace A::B { }` (C++17) |
| 9 | c) | It affects every file that includes the header |
| 10 | b) | Ambiguity is reported only when the name is used |

</details>

---

## 21. Summary

- ✅ A **namespace** is a named scope that groups identifiers and **prevents name collisions**.
- ✅ Create: `namespace Name { ... }` — at global scope or inside another namespace, **never inside a function**.
- ✅ Access members with the **scope resolution operator** `::` → `Name::member`.
- ✅ **`using` declaration** (`using std::cout;`) brings in **one** name — safer.
- ✅ **`using` directive** (`using namespace std;`) brings in **all** names — convenient but riskier.
- ✅ `using` works only in the **scope where it's written** — keep it as small as possible.
- ✅ Same name from two directives → **ambiguity error** when used.
- ✅ `::name` refers to the **global namespace**; inner names **hide** outer ones.
- ✅ **Nested namespaces**: `A::B::C` — C++17 allows `namespace A::B { }`.
- ✅ **Aliases**: `namespace fs = std::filesystem;`
- ✅ Namespaces can be **extended** (opened again) — this is how `std` spans many headers.
- ✅ **Unnamed namespaces** make members **file-private**.
- ✅ **Inline namespaces** are used for library **versioning**.
- ✅ Never add to `namespace std`, and never put `using namespace` in headers.

---


---

> 📝 *Tip for students: Next time you get an "ambiguous" or "not declared in this scope" error, ask yourself: "Which namespace does this name live in, and is it visible here?" — that question solves most of them!*
