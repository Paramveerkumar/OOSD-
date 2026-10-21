#  Variables and Data Types

> **Goal of this lesson:** Learn how C++ stores information in memory using variables, understand the different data types, take input from the user, and convert between types safely.

---

## 📑 Table of Contents

1. [What is a Variable?](#1-what-is-a-variable)
2. [Declaring and Initializing Variables](#2-declaring-and-initializing-variables)
3. [Rules for Naming Variables](#3-rules-for-naming-variables)
4. [What is a Data Type?](#4-what-is-a-data-type)
5. [Fundamental Data Types](#5-fundamental-data-types)
6. [Type Modifiers](#6-type-modifiers)
7. [The `sizeof` Operator](#7-the-sizeof-operator)
8. [The `string` Type](#8-the-string-type)
9. [Taking Input with `cin`](#9-taking-input-with-cin)
10. [Constants: `const` and `constexpr`](#10-constants-const-and-constexpr)
11. [The `auto` Keyword](#11-the-auto-keyword)
12. [Type Conversion (Casting)](#12-type-conversion-casting)
13. [Variable Scope: Local vs Global](#13-variable-scope-local-vs-global)
14. [Overflow and Precision](#14-overflow-and-precision)
15. [Common Beginner Mistakes](#15-common-beginner-mistakes)
16. [Practice Exercises](#16-practice-exercises)
17. [Quick Quiz](#17-quick-quiz)
18. [Summary](#18-summary)

---

## 1. What is a Variable?

A **variable** is a **named storage location in memory** that holds a value. The value can **change** (vary) while the program runs — that's why it's called a *variable*.

📦 **Analogy:** Think of a variable as a **labelled box**.
- The **label** is the variable's **name** (`age`)
- The **contents** are the **value** (`20`)
- The **type of box** decides what can go inside (**data type**: `int`)

```
   age            price           grade
 ┌──────┐      ┌────────┐       ┌──────┐
 │  20  │      │ 99.50  │       │ 'A'  │
 └──────┘      └────────┘       └──────┘
   int           double           char
```

Every variable has:

| Property | Example |
|----------|---------|
| **Name** | `age` |
| **Type** | `int` |
| **Value** | `20` |
| **Memory address** | e.g. `0x7ffd5a3c` (where it lives in RAM) |

---

## 2. Declaring and Initializing Variables

### Declaration
Telling the compiler the **type** and **name** of a variable.

```cpp
int age;        // declares an integer variable named age
```

⚠️ An uninitialized local variable contains a **garbage (unpredictable) value**. Always give it a value before using it!

### Initialization
Giving the variable a value **at the time of declaration**.

```cpp
int age = 20;
```

### Assignment
Giving (or changing) a value **after** declaration.

```cpp
int age;
age = 20;   // assignment
age = 21;   // value changed
```

### Three Ways to Initialize

```cpp
int a = 10;     // Copy initialization (classic style)
int b(10);      // Direct initialization
int c{10};      // Brace / uniform initialization (Modern C++) ✅ recommended
```

💡 **Why brace initialization `{}` is safer:** it prevents accidental data loss.

```cpp
int x = 3.9;    // compiles, but x becomes 3 (decimal part lost silently!)
int y{3.9};     // ❌ compile error: narrowing conversion — the compiler protects you
```

### Declaring Multiple Variables

```cpp
int x = 5, y = 10, z = 15;
```

### Example

```cpp
#include <iostream>
using namespace std;

int main() {
    int age{20};
    double height{5.8};
    char grade{'A'};

    cout << "Age: " << age << endl;
    cout << "Height: " << height << endl;
    cout << "Grade: " << grade << endl;

    age = 21;   // change value
    cout << "Next year age: " << age << endl;

    return 0;
}
```

**Output:**
```
Age: 20
Height: 5.8
Grade: A
Next year age: 21
```

---

## 3. Rules for Naming Variables

### ✅ Rules (must follow — otherwise compile error)

1. Can contain **letters, digits, and underscore `_`**
2. Must **start with a letter or underscore** (not a digit)
3. **No spaces** or special symbols (`@`, `#`, `$`, `-`, etc.)
4. **Cannot be a keyword** (`int`, `return`, `class`, `if`, ...)
5. **Case-sensitive** → `age`, `Age`, and `AGE` are **three different** variables

| Name | Valid? | Reason |
|------|--------|--------|
| `age` | ✅ | |
| `student_name` | ✅ | |
| `_count` | ✅ | (allowed, but avoid leading `_` in your own code) |
| `marks2` | ✅ | |
| `2marks` | ❌ | Starts with a digit |
| `student name` | ❌ | Contains a space |
| `total-marks` | ❌ | `-` not allowed |
| `int` | ❌ | Keyword |
| `price$` | ❌ | `$` not standard |

### 💡 Good Practices (style)

- Use **meaningful names**: `totalMarks` ✅ instead of `tm` ❌
- Pick one style and stay consistent:
  - **camelCase** → `studentName`
  - **snake_case** → `student_name`
- Constants are often written in **UPPER_CASE** → `MAX_SIZE`

---

## 4. What is a Data Type?

A **data type** tells the compiler:
1. **What kind of value** the variable will store (whole number, decimal, character, true/false)
2. **How much memory** to reserve
3. **What operations** are allowed on it

```
                       C++ Data Types
                             │
        ┌────────────────────┼─────────────────────┐
        │                    │                     │
   Fundamental           Derived             User-Defined
  (built-in)                                         
        │                    │                     │
  int, float,          array, pointer,       class, struct,
  double, char,        reference,            enum, union
  bool, void           function
```

In this lesson we focus on **fundamental types** (plus `string`). Others come in later lessons.

---

## 5. Fundamental Data Types

| Type | Stores | Example | Typical Size |
|------|--------|---------|--------------|
| `int` | Whole numbers | `int age = 20;` | 4 bytes |
| `float` | Decimal numbers (single precision) | `float pi = 3.14f;` | 4 bytes |
| `double` | Decimal numbers (double precision) | `double price = 99.99;` | 8 bytes |
| `char` | A single character | `char grade = 'A';` | 1 byte |
| `bool` | `true` or `false` | `bool passed = true;` | 1 byte |
| `void` | No value / "nothing" | used for functions | — |

> 📌 Sizes are **typical** on modern 64-bit systems. The C++ standard only guarantees minimum sizes, so they can vary by compiler and platform.

### 🔢 `int` — Integers

```cpp
int students = 45;
int temperature = -5;
```
- Stores whole numbers, positive or negative — **no decimal point**
- Typical range: **−2,147,483,648 to 2,147,483,647** (about ±2.1 billion)

### 🔸 `float` and `double` — Decimal Numbers

```cpp
float  weight = 65.5f;      // 'f' suffix marks a float literal
double pi     = 3.14159265358979;
```

| | `float` | `double` |
|--|--------|----------|
| Size | 4 bytes | 8 bytes |
| Precision | ~6–7 digits | ~15–16 digits |
| Use | When memory matters | **Default choice** for decimals ✅ |

💡 A decimal literal like `3.14` is a **`double`** by default. Write `3.14f` for a `float`.

### 🔤 `char` — Single Character

```cpp
char grade = 'A';
char symbol = '#';
char digit = '7';     // the character '7', not the number 7
```
- Uses **single quotes** `' '`
- Stores exactly **one** character
- Internally stored as a number using **ASCII** codes:

```cpp
char ch = 'A';
cout << ch << endl;        // A
cout << (int)ch << endl;   // 65   (ASCII value of 'A')

char next = 'A' + 1;
cout << next << endl;      // B
```

| Character | ASCII |
|-----------|-------|
| `'A'` – `'Z'` | 65 – 90 |
| `'a'` – `'z'` | 97 – 122 |
| `'0'` – `'9'` | 48 – 57 |
| `' '` (space) | 32 |

### ✔️ `bool` — True or False

```cpp
bool isRaining = false;
bool hasPassed = true;

cout << hasPassed << endl;              // 1
cout << boolalpha << hasPassed << endl; // true
```
- `true` is printed as **1**, `false` as **0** (unless you use `boolalpha`)
- Used heavily in conditions (`if`, loops) — coming in later lessons

### 🚫 `void` — No Type
Means "no value". You can't create a `void` variable. It is used for functions that return nothing:
```cpp
void sayHello() {
    cout << "Hello!";
}
```

---

## 6. Type Modifiers

**Modifiers** change the size or range of basic types.

| Modifier | Effect |
|----------|--------|
| `signed` | Can store negative and positive values (default for `int`) |
| `unsigned` | Only zero and positive values → doubles the positive range |
| `short` | Smaller size (usually 2 bytes) |
| `long` | Larger size (4 or 8 bytes, depending on system) |
| `long long` | At least 8 bytes |

### Common Combinations

| Type | Typical Size | Typical Range |
|------|--------------|---------------|
| `short` | 2 bytes | −32,768 to 32,767 |
| `unsigned short` | 2 bytes | 0 to 65,535 |
| `int` | 4 bytes | −2,147,483,648 to 2,147,483,647 |
| `unsigned int` | 4 bytes | 0 to 4,294,967,295 |
| `long` | 4 bytes (Windows) / 8 bytes (Linux, macOS) | varies |
| `long long` | 8 bytes | about ±9.2 × 10¹⁸ |
| `unsigned long long` | 8 bytes | 0 to about 1.8 × 10¹⁹ |
| `long double` | 8, 12, or 16 bytes | more precision than `double` |

```cpp
unsigned int marks = 95;
long long population = 8000000000LL;   // 8 billion — too big for int!
short smallNumber = 100;
```

💡 **When to use what?**
- Everyday whole numbers → `int`
- Very large numbers (population, factorials, competitive programming) → `long long`
- Decimals → `double`

---

## 7. The `sizeof` Operator

`sizeof` tells you how many **bytes** a type or variable uses **on your system**.

```cpp
#include <iostream>
using namespace std;

int main() {
    cout << "char        : " << sizeof(char)        << " byte"  << endl;
    cout << "bool        : " << sizeof(bool)        << " byte"  << endl;
    cout << "short       : " << sizeof(short)       << " bytes" << endl;
    cout << "int         : " << sizeof(int)         << " bytes" << endl;
    cout << "long        : " << sizeof(long)        << " bytes" << endl;
    cout << "long long   : " << sizeof(long long)   << " bytes" << endl;
    cout << "float       : " << sizeof(float)       << " bytes" << endl;
    cout << "double      : " << sizeof(double)      << " bytes" << endl;
    cout << "long double : " << sizeof(long double) << " bytes" << endl;
    return 0;
}
```

**Possible output (64-bit Linux):**
```
char        : 1 byte
bool        : 1 byte
short       : 2 bytes
int         : 4 bytes
long        : 8 bytes
long long   : 8 bytes
float       : 4 bytes
double      : 8 bytes
long double : 16 bytes
```

🧪 **Try it:** Run this on your own computer — your results for `long` and `long double` may differ!

### Finding Min/Max Values

```cpp
#include <iostream>
#include <climits>   // for INT_MAX, INT_MIN
using namespace std;

int main() {
    cout << "int min: " << INT_MIN << endl;
    cout << "int max: " << INT_MAX << endl;
    return 0;
}
```

---

## 8. The `string` Type

`string` stores **text (a sequence of characters)**. It isn't a fundamental type — it comes from the **standard library**, so you need `#include <string>`.

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string name = "Priya";
    string city{"Delhi"};

    cout << "Name: " << name << endl;
    cout << "City: " << city << endl;
    cout << "Length of name: " << name.length() << endl;

    string greeting = "Hello, " + name + "!";   // joining strings
    cout << greeting << endl;

    return 0;
}
```

**Output:**
```
Name: Priya
City: Delhi
Length of name: 5
Hello, Priya!
```

| `char` | `string` |
|--------|----------|
| One character | Many characters |
| Single quotes `'A'` | Double quotes `"Apple"` |
| Built-in | Needs `#include <string>` |

---

## 9. Taking Input with `cin`

`cin` (**character input**) reads values from the keyboard using the **extraction operator `>>`**.

```cpp
#include <iostream>
using namespace std;

int main() {
    int age;
    cout << "Enter your age: ";
    cin >> age;

    cout << "You are " << age << " years old." << endl;
    return 0;
}
```

**Sample run:**
```
Enter your age: 19
You are 19 years old.
```

### Reading Multiple Values

```cpp
int a, b;
cout << "Enter two numbers: ";
cin >> a >> b;            // user can type: 10 20
cout << "Sum = " << a + b << endl;
```

### 📌 Reading Full Lines with `getline`

`cin >> ` stops reading at the **first space**:

```cpp
string name;
cin >> name;              // input "Rahul Sharma" → name = "Rahul" only!
```

Use `getline` to read a whole line:

```cpp
string fullName;
cout << "Enter full name: ";
getline(cin, fullName);   // input "Rahul Sharma" → fullName = "Rahul Sharma"
```

⚠️ **The `cin` + `getline` trap:** after `cin >> number;`, a leftover newline stays in the input. The next `getline` reads that empty line immediately. Fix it with `cin.ignore()`:

```cpp
int age;
string name;

cout << "Enter age: ";
cin >> age;
cin.ignore();               // throw away the leftover newline

cout << "Enter full name: ";
getline(cin, name);
```

### `cout` vs `cin`

| | `cout` | `cin` |
|--|--------|-------|
| Purpose | Output (screen) | Input (keyboard) |
| Operator | `<<` (insertion) | `>>` (extraction) |
| Memory trick | Data flows **out to** cout ⬅ | Data flows **from** cin ➡ into the variable |

---

## 10. Constants: `const` and `constexpr`

A **constant** is a variable whose value **cannot change** after initialization.

```cpp
const double PI = 3.14159;
const int MAX_STUDENTS = 60;

PI = 3.0;   // ❌ error: assignment of read-only variable
```

- A `const` **must be initialized** when declared
- Use constants for fixed values → code becomes **safer and more readable**

### `constexpr` (Modern C++)
Means the value is known at **compile time**:

```cpp
constexpr int DAYS_IN_WEEK = 7;
constexpr double GRAVITY = 9.8;
```

💡 **Simple rule for now:** use `constexpr` for fixed values known while writing code, and `const` for values that are fixed once computed at runtime (e.g., from user input).

### Literals
A **literal** is a fixed value written directly in code:

| Literal | Type |
|---------|------|
| `42` | `int` |
| `42u` | `unsigned int` |
| `42LL` | `long long` |
| `3.14` | `double` |
| `3.14f` | `float` |
| `'A'` | `char` |
| `"Hello"` | string literal |
| `true` / `false` | `bool` |

---

## 11. The `auto` Keyword

With `auto`, the compiler **figures out the type automatically** from the value.

```cpp
auto count = 10;       // int
auto price = 99.5;     // double
auto letter = 'Z';     // char
auto flag = true;      // bool
auto big = 5000000000LL; // long long
```

⚠️ `auto` **must** be initialized — otherwise the compiler can't guess the type:
```cpp
auto x;   // ❌ error
```

💡 **Tip for beginners:** write explicit types (`int`, `double`) while learning. `auto` becomes very useful later with complex types (iterators, STL).

---

## 12. Type Conversion (Casting)

Converting a value from one type to another.

### 1️⃣ Implicit Conversion (Automatic)
Done automatically by the compiler.

```cpp
int a = 5;
double b = a;        // int → double: b = 5.0 (safe, no data lost)

double c = 9.99;
int d = c;           // double → int: d = 9 (decimal part LOST!)
```

**Promotion order** in mixed expressions — the smaller type is converted to the larger one:
```
bool → char → short → int → long → long long → float → double → long double
```

```cpp
int x = 5;
double y = 2.5;
auto result = x + y;   // x is promoted to double → result = 7.5 (double)
```

### ⚠️ The Integer Division Trap

```cpp
int a = 7, b = 2;
cout << a / b << endl;          // 3   (not 3.5!) — int / int = int
cout << 7.0 / 2 << endl;        // 3.5 — one double makes it double division
```

### 2️⃣ Explicit Conversion (Manual Casting)

```cpp
int total = 7, count = 2;

double avg1 = (double)total / count;              // C-style cast
double avg2 = static_cast<double>(total) / count; // C++ style ✅ recommended

cout << avg2 << endl;   // 3.5
```

💡 Prefer `static_cast<type>(value)` — it is clearer and easier to search for in code.

### Char ↔ Int Conversions

```cpp
char ch = 'a';
int code = static_cast<int>(ch);       // 97

int n = 66;
char letter = static_cast<char>(n);    // 'B'

char digitChar = '7';
int digit = digitChar - '0';           // 7  (useful trick!)
```

---

## 13. Variable Scope: Local vs Global

**Scope** is the region of the program where a variable can be used.

```cpp
#include <iostream>
using namespace std;

int globalCount = 100;      // 🌍 Global: usable everywhere in this file

int main() {
    int localValue = 5;     // 🏠 Local: only usable inside main()

    {
        int blockVar = 10;  // 📦 Block scope: only inside these braces
        cout << blockVar << endl;
    }
    // cout << blockVar;    // ❌ error: blockVar doesn't exist here

    cout << globalCount << endl;
    cout << localValue << endl;
    return 0;
}
```

| | Local | Global |
|--|-------|--------|
| Declared | Inside a function / block `{ }` | Outside all functions |
| Accessible | Only inside that block | Anywhere in the file |
| Default value | **Garbage** (if not initialized) | **0** |

💡 **Good practice:** prefer local variables. Too many globals make programs hard to debug.

---

## 14. Overflow and Precision

### Integer Overflow
Storing a value **bigger than the type can hold** gives wrong results.

```cpp
#include <iostream>
#include <climits>
using namespace std;

int main() {
    unsigned int u = 0;
    u = u - 1;
    cout << u << endl;          // 4294967295 — wraps around!

    int big = INT_MAX;
    cout << big << endl;        // 2147483647
    // big + 1 → overflow (undefined behaviour for signed int — never rely on it!)

    long long safe = (long long)INT_MAX + 1;
    cout << safe << endl;       // 2147483648 ✅
    return 0;
}
```

🔢 **Analogy:** a car's odometer that shows only 4 digits — after `9999`, it rolls over to `0000`.

### Floating-Point Precision
Decimals are stored approximately, not exactly.

```cpp
double a = 0.1 + 0.2;
cout.precision(17);
cout << a << endl;     // 0.30000000000000004 (not exactly 0.3!)
```

📌 **Lesson:** never compare decimals with `==` directly. (We'll learn the correct way in the Operators lesson.)

---

## 15. Common Beginner Mistakes

| ❌ Mistake | ✅ Correct | Why |
|-----------|-----------|-----|
| `int x; cout << x;` | `int x = 0;` | Uninitialized variables hold garbage |
| `char c = "A";` | `char c = 'A';` | `char` uses single quotes |
| `string s = 'Hello';` | `string s = "Hello";` | Strings use double quotes |
| `int 2num = 5;` | `int num2 = 5;` | Names can't start with a digit |
| `double avg = 7 / 2;` → 3 | `double avg = 7.0 / 2;` → 3.5 | int / int = int |
| `int phone = 9876543210;` | `long long phone = 9876543210LL;` (or store as `string`) | Too big for `int` |
| `cin << x;` | `cin >> x;` | Input uses `>>` |
| Using `string` without `#include <string>` | Add `#include <string>` | Don't rely on other headers including it |
| `const int MAX;` | `const int MAX = 100;` | Constants must be initialized |
| `Age = 20;` when declared as `age` | `age = 20;` | C++ is case-sensitive |

---

## 16. Practice Exercises

### 🟢 Easy

**Exercise 1:** Declare variables to store your name, age, height (in meters), and grade letter. Print them all.

**Exercise 2:** Print the size (in bytes) of `int`, `char`, `float`, `double`, and `bool` using `sizeof`.

**Exercise 3:** Take two integers as input and print their sum, difference, and product.

### 🟡 Medium

**Exercise 4:** Take the radius of a circle as input and print its **area** and **circumference**. Use `const double PI = 3.14159;`

**Exercise 5:** Take a character as input and print its **ASCII value**.

**Exercise 6:** Take marks of 3 subjects (integers) as input and print the **average as a decimal** (e.g., `85.67`, not `85`).

**Exercise 7:** Swap two numbers using a third variable.

### 🔴 Challenge

**Exercise 8:** Swap two integers **without** using a third variable.

**Exercise 9:** Take temperature in **Celsius** and convert it to **Fahrenheit**.
Formula: `F = (C × 9 / 5) + 32` — be careful of integer division!

**Exercise 10:** Take the user's **full name** (with spaces) and **age** as input, then print:
```
Hello Rahul Sharma! In 5 years you will be 24.
```

**Exercise 11:** Predict the output **before** running it, then check:
```cpp
int a = 10, b = 4;
double c = a / b;
double d = static_cast<double>(a) / b;
char e = 'A' + 2;
cout << c << " " << d << " " << e << endl;
```

---

### ✅ Solutions

<details>
<summary>Click to view Exercise 1 solution</summary>

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string name = "Ananya";
    int age = 18;
    double height = 1.62;
    char grade = 'A';

    cout << "Name   : " << name << endl;
    cout << "Age    : " << age << endl;
    cout << "Height : " << height << " m" << endl;
    cout << "Grade  : " << grade << endl;
    return 0;
}
```
</details>

<details>
<summary>Click to view Exercise 2 solution</summary>

```cpp
#include <iostream>
using namespace std;

int main() {
    cout << "int    : " << sizeof(int) << endl;
    cout << "char   : " << sizeof(char) << endl;
    cout << "float  : " << sizeof(float) << endl;
    cout << "double : " << sizeof(double) << endl;
    cout << "bool   : " << sizeof(bool) << endl;
    return 0;
}
```
</details>

<details>
<summary>Click to view Exercise 3 solution</summary>

```cpp
#include <iostream>
using namespace std;

int main() {
    int a, b;
    cout << "Enter two numbers: ";
    cin >> a >> b;

    cout << "Sum        = " << a + b << endl;
    cout << "Difference = " << a - b << endl;
    cout << "Product    = " << a * b << endl;
    return 0;
}
```
</details>

<details>
<summary>Click to view Exercise 4 solution</summary>

```cpp
#include <iostream>
using namespace std;

int main() {
    const double PI = 3.14159;
    double radius;

    cout << "Enter radius: ";
    cin >> radius;

    double area = PI * radius * radius;
    double circumference = 2 * PI * radius;

    cout << "Area          = " << area << endl;
    cout << "Circumference = " << circumference << endl;
    return 0;
}
```
</details>

<details>
<summary>Click to view Exercise 5 solution</summary>

```cpp
#include <iostream>
using namespace std;

int main() {
    char ch;
    cout << "Enter a character: ";
    cin >> ch;

    cout << "ASCII value of '" << ch << "' is " << static_cast<int>(ch) << endl;
    return 0;
}
```
</details>

<details>
<summary>Click to view Exercise 6 solution</summary>

```cpp
#include <iostream>
using namespace std;

int main() {
    int m1, m2, m3;
    cout << "Enter marks of 3 subjects: ";
    cin >> m1 >> m2 >> m3;

    double average = static_cast<double>(m1 + m2 + m3) / 3;
    cout << "Average = " << average << endl;
    return 0;
}
```
</details>

<details>
<summary>Click to view Exercise 7 solution</summary>

```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 5, b = 10;
    cout << "Before: a = " << a << ", b = " << b << endl;

    int temp = a;
    a = b;
    b = temp;

    cout << "After : a = " << a << ", b = " << b << endl;
    return 0;
}
```
</details>

<details>
<summary>Click to view Exercise 8 solution</summary>

```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 5, b = 10;
    cout << "Before: a = " << a << ", b = " << b << endl;

    a = a + b;   // a = 15
    b = a - b;   // b = 5
    a = a - b;   // a = 10

    cout << "After : a = " << a << ", b = " << b << endl;
    return 0;
}
```
> ⚠️ This trick can overflow with very large numbers. In real code, use a temp variable or `std::swap(a, b);`
</details>

<details>
<summary>Click to view Exercise 9 solution</summary>

```cpp
#include <iostream>
using namespace std;

int main() {
    double celsius;
    cout << "Enter temperature in Celsius: ";
    cin >> celsius;

    double fahrenheit = (celsius * 9.0 / 5.0) + 32;   // 9.0 avoids integer division
    cout << celsius << " C = " << fahrenheit << " F" << endl;
    return 0;
}
```
</details>

<details>
<summary>Click to view Exercise 10 solution</summary>

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string fullName;
    int age;

    cout << "Enter your full name: ";
    getline(cin, fullName);

    cout << "Enter your age: ";
    cin >> age;

    cout << "Hello " << fullName << "! In 5 years you will be " << age + 5 << "." << endl;
    return 0;
}
```
> 💡 Here `getline` comes **before** `cin >>`, so no `cin.ignore()` is needed.
</details>

<details>
<summary>Click to view Exercise 11 answer</summary>

**Output:**
```
2 2.5 C
```
- `c = a / b` → `10 / 4` is integer division = `2`, then stored as `2.0` → printed as `2`
- `d` → `10.0 / 4` = `2.5`
- `e` → `'A' + 2` = 67 = `'C'`
</details>

---

## 17. Quick Quiz

**Q1.** Which data type is best for storing `3.14159`?
- a) `int`
- b) `char`
- c) `double`
- d) `bool`

**Q2.** Which variable name is **invalid**?
- a) `total_marks`
- b) `_value`
- c) `marks2`
- d) `2marks`

**Q3.** What is the output of `cout << 9 / 2;`?
- a) 4.5
- b) 4
- c) 5
- d) Error

**Q4.** What is the typical size of `char`?
- a) 1 byte
- b) 2 bytes
- c) 4 bytes
- d) 8 bytes

**Q5.** Which is the correct way to declare a character?
- a) `char c = "A";`
- b) `char c = 'A';`
- c) `char c = A;`
- d) `string c = 'A';`

**Q6.** What does `cout << true;` print by default?
- a) true
- b) TRUE
- c) 1
- d) 0

**Q7.** Which keyword makes a variable's value unchangeable?
- a) `static`
- b) `auto`
- c) `const`
- d) `fixed`

**Q8.** Which function reads a full line including spaces?
- a) `cin >>`
- b) `getline()`
- c) `scanf()`
- d) `read()`

**Q9.** What type does `auto x = 5.0;` give `x`?
- a) `int`
- b) `float`
- c) `double`
- d) `auto`

**Q10.** What happens with `int x{7.5};`?
- a) x = 7
- b) x = 8
- c) x = 7.5
- d) Compile error

<details>
<summary>✅ Click to view answers</summary>

| Q | Answer | Explanation |
|---|--------|-------------|
| 1 | c) `double` | Decimal values need a floating-point type |
| 2 | d) `2marks` | Names can't start with a digit |
| 3 | b) 4 | Integer division drops the decimal |
| 4 | a) 1 byte | `char` is always 1 byte |
| 5 | b) `char c = 'A';` | Single quotes for `char` |
| 6 | c) 1 | Use `boolalpha` to print `true` |
| 7 | c) `const` | |
| 8 | b) `getline()` | `cin >>` stops at whitespace |
| 9 | c) `double` | `5.0` is a double literal |
| 10 | d) Compile error | Brace init blocks narrowing conversions |

</details>

---

## 18. Summary

- ✅ A **variable** is a named memory location: it has a **name, type, value, and address**.
- ✅ **Declare** → `int x;` | **Initialize** → `int x = 5;` | Prefer **brace init** → `int x{5};`
- ✅ Names: letters, digits, `_`; can't start with a digit; no keywords; **case-sensitive**.
- ✅ Fundamental types: `int`, `float`, `double`, `char`, `bool`, `void`.
- ✅ Modifiers: `short`, `long`, `long long`, `signed`, `unsigned`.
- ✅ Use `sizeof` to check memory size; sizes can vary by system.
- ✅ `string` stores text and needs `#include <string>`.
- ✅ `cin >>` reads input (stops at space); `getline(cin, s)` reads a full line.
- ✅ `const` / `constexpr` create values that can't change.
- ✅ `auto` lets the compiler deduce the type.
- ✅ **int / int = int** — use `static_cast<double>()` for decimal results.
- ✅ Watch out for **overflow** and **floating-point precision** issues.

---

### ⏮️ Previous Lesson
👈 **Lesson 1: Overview of C++**

### ⏭️ Next Lesson
👉 **Lesson 3: Operators in C++**

---

> 📝 *Tip for students: Change the values in every example and predict the output before running it. Being surprised by the output is the fastest way to learn!*
