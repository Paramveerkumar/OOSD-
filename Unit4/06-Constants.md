#  Constants in C++

> **Goal of this lesson:** Learn every way to create fixed values in C++ — literals, `const`, `constexpr`, `#define`, and `enum` — understand how they differ, and know which one to use and when.

> 🔗 *Lesson 2 briefly introduced `const`, `constexpr`, and literals. This lesson covers constants in full depth.*

---

## 📑 Table of Contents

1. [What is a Constant?](#1-what-is-a-constant)
2. [Why Use Constants?](#2-why-use-constants)
3. [Types of Constants in C++](#3-types-of-constants-in-c)
4. [Literal Constants](#4-literal-constants)
   - [Integer Literals](#41-integer-literals)
   - [Floating-Point Literals](#42-floating-point-literals)
   - [Character Literals & Escape Sequences](#43-character-literals--escape-sequences)
   - [String Literals & Raw Strings](#44-string-literals--raw-strings)
   - [Boolean & Pointer Literals](#45-boolean--pointer-literals)
5. [`const` Variables](#5-const-variables)
6. [`constexpr` — Compile-Time Constants](#6-constexpr--compile-time-constants)
7. [`const` vs `constexpr`](#7-const-vs-constexpr)
8. [`#define` — Macro Constants](#8-define--macro-constants)
9. [Enumeration Constants (`enum`)](#9-enumeration-constants-enum)
10. [`const` with Function Parameters](#10-const-with-function-parameters)
11. [`consteval` and `constinit` (Advanced)](#11-consteval-and-constinit-advanced)
12. [Magic Numbers](#12-magic-numbers)
13. [Which Constant Should I Use?](#13-which-constant-should-i-use)
14. [Common Beginner Mistakes](#14-common-beginner-mistakes)
15. [Practice Exercises](#15-practice-exercises)
16. [Quick Quiz](#16-quick-quiz)
17. [Summary](#17-summary)

---

## 1. What is a Constant?

A **constant** is a value that **cannot be changed** while the program runs.

| Variable | Constant |
|----------|----------|
| Value **can** change | Value **cannot** change |
| `int age = 20; age = 21;` ✅ | `const int DAYS = 7; DAYS = 8;` ❌ |
| Like a **whiteboard** — write, erase, rewrite | Like a **stone carving** — fixed forever |

### Real-World Constants

| Constant | Value |
|----------|-------|
| Days in a week | 7 |
| Value of π | 3.14159... |
| Speed of light | 299,792,458 m/s |
| Freezing point of water | 0 °C |
| Months in a year | 12 |

These values **never change**, so in a program they should be **constants**.

---

## 2. Why Use Constants?

### ✅ 1. Safety — Prevents Accidental Changes

```cpp
const double PI = 3.14159;
PI = 3;   // ❌ compile error — the compiler protects you!
```

### ✅ 2. Readability — Names Explain Meaning

```cpp
double fee = marks * 50;            // ❌ what is 50?
double fee = marks * FEE_PER_MARK;  // ✅ clear
```

### ✅ 3. Easy Maintenance — Change in One Place

```cpp
const double GST_RATE = 0.18;
// Used in 30 places in the program...
// If the tax rate changes, update ONE line instead of 30!
```

### ✅ 4. Performance — Compiler Can Optimize
The compiler knows the value never changes, so it can replace it directly in the machine code.

---

## 3. Types of Constants in C++

```
                         Constants in C++
                                │
     ┌──────────────┬───────────┼────────────┬──────────────┐
     │              │           │            │              │
  Literal        const      constexpr     #define         enum
 Constants     Variables    Variables     Macros       Constants
     │              │           │            │              │
  42, 3.14,    const int   constexpr    #define PI     enum Color
  'A', "Hi",   MAX = 100;  int N = 5;   3.14159        { RED, GREEN }
  true
```

| Type | Example | Has a name? | Type-checked? |
|------|---------|-------------|---------------|
| Literal | `42`, `'A'`, `"Hello"` | ❌ | ✅ |
| `const` | `const int MAX = 100;` | ✅ | ✅ |
| `constexpr` | `constexpr int N = 5;` | ✅ | ✅ |
| `#define` | `#define PI 3.14159` | ✅ | ❌ (text replacement) |
| `enum` | `enum Color { RED, GREEN };` | ✅ | ✅ |

---

## 4. Literal Constants

A **literal** is a fixed value written **directly** in the source code.

```cpp
int age = 20;            // 20 is a literal
double pi = 3.14;        // 3.14 is a literal
char grade = 'A';        // 'A' is a literal
string name = "Riya";    // "Riya" is a literal
```

### 4.1 Integer Literals

Whole numbers can be written in **four number systems**:

| System | Base | Prefix | Example | Decimal Value |
|--------|------|--------|---------|---------------|
| **Decimal** | 10 | none | `255` | 255 |
| **Octal** | 8 | `0` | `0377` | 255 |
| **Hexadecimal** | 16 | `0x` or `0X` | `0xFF` | 255 |
| **Binary** (C++14) | 2 | `0b` or `0B` | `0b11111111` | 255 |

```cpp
#include <iostream>
using namespace std;

int main() {
    int dec = 255;
    int oct = 0377;
    int hex = 0xFF;
    int bin = 0b11111111;

    cout << dec << " " << oct << " " << hex << " " << bin << endl;
    return 0;
}
```

**Output:**
```
255 255 255 255
```

⚠️ **Octal trap!** A leading `0` makes a number **octal**:
```cpp
int pinCode = 010;    // ❗ this is 8, not 10!
cout << pinCode;      // 8
```

#### Digit Separators (C++14)
Use `'` to make large numbers readable. The compiler ignores it.

```cpp
long long population = 1'400'000'000;   // 1.4 billion
int salary = 75'000;
int binaryMask = 0b1111'0000;
```

#### Integer Suffixes
A suffix sets the **type** of the literal:

| Suffix | Type | Example |
|--------|------|---------|
| none | `int` | `100` |
| `u` / `U` | `unsigned int` | `100u` |
| `l` / `L` | `long` | `100L` |
| `ll` / `LL` | `long long` | `100LL` |
| `ul` / `UL` | `unsigned long` | `100UL` |
| `ull` / `ULL` | `unsigned long long` | `100ULL` |

💡 Prefer **uppercase `L`** — lowercase `l` looks like the digit `1` (`100l` vs `1001`).

### 4.2 Floating-Point Literals

| Form | Example | Value |
|------|---------|-------|
| Decimal | `3.14` | 3.14 |
| Decimal (no leading digit) | `.5` | 0.5 |
| Scientific notation | `1.5e3` | 1.5 × 10³ = 1500 |
| Negative exponent | `2.5e-4` | 0.00025 |

**Scientific notation:** `e` means **"× 10 to the power of"**.

```cpp
double avogadro = 6.022e23;     // 6.022 × 10²³
double electron = 9.109e-31;    // mass of electron in kg
```

#### Floating-Point Suffixes

| Suffix | Type | Example |
|--------|------|---------|
| none | `double` (default) | `3.14` |
| `f` / `F` | `float` | `3.14f` |
| `l` / `L` | `long double` | `3.14L` |

```cpp
float  a = 3.14f;
double b = 3.14;
long double c = 3.14L;
```

### 4.3 Character Literals & Escape Sequences

A character literal is a **single character** inside **single quotes**.

```cpp
char letter = 'A';
char digit = '9';
char symbol = '@';
```

#### Escape Sequences
Some characters can't be typed directly, so we use a **backslash `\`** followed by a code:

| Escape | Meaning | Example Output |
|--------|---------|----------------|
| `\n` | New line | moves to next line |
| `\t` | Horizontal tab | inserts a tab space |
| `\\` | Backslash | `\` |
| `\'` | Single quote | `'` |
| `\"` | Double quote | `"` |
| `\0` | Null character | marks end of C-strings |
| `\a` | Alert (beep) | system beep (may not work everywhere) |
| `\r` | Carriage return | cursor to start of line |
| `\b` | Backspace | moves cursor back one |

```cpp
#include <iostream>
using namespace std;

int main() {
    cout << "Name:\tRiya\n";
    cout << "Path: C:\\Users\\Riya\n";
    cout << "She said \"Hello!\"\n";
    cout << "It\'s fine\n";
    return 0;
}
```

**Output:**
```
Name:	Riya
Path: C:\Users\Riya
She said "Hello!"
It's fine
```

#### Wide and Unicode Characters (for reference)

| Prefix | Type | Example |
|--------|------|---------|
| none | `char` | `'A'` |
| `L` | `wchar_t` | `L'A'` |
| `u8` | `char8_t` (C++20) | `u8'A'` |
| `u` | `char16_t` | `u'A'` |
| `U` | `char32_t` | `U'A'` |

### 4.4 String Literals & Raw Strings

A **string literal** is a sequence of characters in **double quotes**.

```cpp
cout << "Hello, World!";
```

📌 Internally, `"Hi"` is stored as **3 characters**: `'H'`, `'i'`, `'\0'` (the hidden null terminator).

```
 "Hi"  →  ┌───┬───┬────┐
          │ H │ i │ \0 │
          └───┴───┴────┘
```

#### Adjacent String Concatenation
String literals placed next to each other are **joined automatically**:

```cpp
cout << "Hello, "
        "World!"     // becomes "Hello, World!"
     << endl;
```

Useful for splitting long text across lines.

#### Raw String Literals (C++11)
Escape sequences get messy with file paths and special text. **Raw strings** treat everything **exactly as written**:

```
R"( ...your text... )"
```

```cpp
// ❌ Normal string — needs escaping
cout << "C:\\Program Files\\MyApp\\data.txt" << endl;

// ✅ Raw string — no escaping needed
cout << R"(C:\Program Files\MyApp\data.txt)" << endl;

// ✅ Multi-line raw string
cout << R"(Line 1
Line 2
    "Quotes" and \n are printed as-is)" << endl;
```

**Output:**
```
C:\Program Files\MyApp\data.txt
C:\Program Files\MyApp\data.txt
Line 1
Line 2
    "Quotes" and \n are printed as-is
```

### 4.5 Boolean & Pointer Literals

| Literal | Type | Meaning |
|---------|------|---------|
| `true` | `bool` | Yes / 1 |
| `false` | `bool` | No / 0 |
| `nullptr` | pointer | "Points to nothing" (covered in the Pointers lesson) |

```cpp
bool isOpen = true;
int* ptr = nullptr;   // preview — pointers come later
```

---

## 5. `const` Variables

The **`const`** keyword makes a variable **read-only** after initialization.

### Syntax
```cpp
const dataType NAME = value;
```

### Example

```cpp
#include <iostream>
using namespace std;

int main() {
    const double PI = 3.14159;
    const int DAYS_IN_WEEK = 7;
    const string COLLEGE = "ABC Institute";

    double radius = 5;
    cout << "Area: " << PI * radius * radius << endl;
    cout << "Days: " << DAYS_IN_WEEK << endl;
    cout << "College: " << COLLEGE << endl;

    // PI = 3.0;   // ❌ error: assignment of read-only variable 'PI'
    return 0;
}
```

### 📌 Rules of `const`

**Rule 1: Must be initialized at declaration**
```cpp
const int MAX;          // ❌ error: uninitialized const 'MAX'
const int MAX = 100;    // ✅
```

**Rule 2: Cannot be changed afterwards**
```cpp
const int MAX = 100;
MAX = 200;              // ❌ error
MAX++;                  // ❌ error
```

**Rule 3: Can be initialized with a runtime value**
`const` doesn't require the value to be known while compiling — it just can't change **after** it's set.

```cpp
int age;
cout << "Enter your age: ";
cin >> age;

const int BIRTH_YEAR = 2026 - age;   // ✅ computed at runtime, then fixed
// BIRTH_YEAR = 2000;                // ❌ cannot change
```

**Rule 4: `const` placement — both are the same**
```cpp
const int A = 10;   // more common ✅
int const B = 10;   // also valid
```

---

## 6. `constexpr` — Compile-Time Constants

**`constexpr`** (C++11) means: *"this value must be computed while compiling — before the program even runs."*

### Syntax
```cpp
constexpr dataType NAME = value;
```

### Example

```cpp
#include <iostream>
using namespace std;

constexpr int SECONDS_PER_MINUTE = 60;
constexpr int MINUTES_PER_HOUR = 60;
constexpr int SECONDS_PER_HOUR = SECONDS_PER_MINUTE * MINUTES_PER_HOUR;  // ✅ computed by compiler

int main() {
    cout << "Seconds in an hour: " << SECONDS_PER_HOUR << endl;   // 3600
    return 0;
}
```

### ❌ `constexpr` Cannot Use Runtime Values

```cpp
int n;
cin >> n;
constexpr int SIZE = n;   // ❌ error: 'n' is not a constant expression
const int SIZE2 = n;      // ✅ const is fine
```

### Why `constexpr` Matters: Array Sizes
Array sizes must be known **at compile time**:

```cpp
constexpr int CLASS_SIZE = 40;
int marks[CLASS_SIZE];        // ✅ guaranteed to work
```

### `constexpr` Functions (Preview)
A function marked `constexpr` **can** be evaluated at compile time if given constant arguments:

```cpp
constexpr int square(int x) {
    return x * x;
}

constexpr int AREA = square(5);   // ✅ computed by the compiler → 25

int main() {
    int side;
    cin >> side;
    int area = square(side);      // ✅ also works at runtime
    return 0;
}
```

> 📚 Functions are covered in detail in a later lesson.

---

## 7. `const` vs `constexpr`

| Feature | `const` | `constexpr` |
|---------|---------|-------------|
| Meaning | "I promise not to change this" | "This must be known at compile time" |
| Value known when? | Compile time **or** runtime | **Compile time only** |
| Can use `cin` input? | ✅ Yes | ❌ No |
| Use as array size? | Only if initialized with a constant expression | ✅ Always |
| Introduced in | C++98 | C++11 |
| Analogy | A sealed envelope — sealed after you write it | A printed book — fixed before it reaches you |

```cpp
int userInput;
cin >> userInput;

const int A = 10;              // ✅ compile-time value
const int B = userInput;       // ✅ runtime value
constexpr int C = 10;          // ✅ compile-time value
constexpr int D = userInput;   // ❌ error
```

💡 **Simple rule:**
- Value is **known while writing code** → use `constexpr`
- Value is **calculated or read while running** → use `const`

---

## 8. `#define` — Macro Constants

`#define` is a **preprocessor directive** that performs **text replacement before compilation**.

### Syntax
```cpp
#define NAME value      // no '=' and no ';'
```

### Example

```cpp
#include <iostream>
using namespace std;

#define PI 3.14159
#define MAX_STUDENTS 60
#define SCHOOL "Greenwood School"

int main() {
    cout << "PI: " << PI << endl;
    cout << "Max students: " << MAX_STUDENTS << endl;
    cout << "School: " << SCHOOL << endl;
    return 0;
}
```

Before compiling, the preprocessor replaces every `PI` with `3.14159`, like **find & replace** in a text editor.

### ⚠️ Problems with `#define`

**Problem 1: No type checking**
```cpp
#define AGE "twenty"
int x = AGE;    // error appears at this line, with a confusing message
```

**Problem 2: No scope — affects everything after it**
```cpp
void f() {
    #define LIMIT 10
}
int LIMIT = 5;  // ❌ becomes: int 10 = 5;  → strange error!
```

**Problem 3: Common syntax mistakes**
```cpp
#define MAX = 100;      // ❌ MAX becomes "= 100;"
int x = MAX;            // becomes: int x = = 100;;  → error
```

**Problem 4: Macro "functions" can give wrong answers**
```cpp
#define SQUARE(x) x * x

cout << SQUARE(5);        // 5 * 5 = 25         ✅
cout << SQUARE(2 + 3);    // 2 + 3 * 2 + 3 = 11 ❌ (expected 25!)
```
Because it's **text replacement**, not calculation. Fix with parentheses — or better, use a `constexpr` function:
```cpp
#define SQUARE(x) ((x) * (x))           // safer macro
constexpr int square(int x) { return x * x; }   // ✅ best
```

**Problem 5: Harder debugging**
The debugger sees `3.14159`, not `PI`.

### `#define` vs `const` vs `constexpr`

| Feature | `#define` | `const` | `constexpr` |
|---------|-----------|---------|-------------|
| Processed by | Preprocessor | Compiler | Compiler |
| Has a type | ❌ | ✅ | ✅ |
| Respects scope / namespaces | ❌ | ✅ | ✅ |
| Visible in debugger | ❌ | ✅ | ✅ |
| Syntax | `#define X 10` | `const int X = 10;` | `constexpr int X = 10;` |
| Recommended for constants | ❌ (legacy) | ✅ | ✅ ⭐ |

📌 **Modern C++:** prefer `constexpr`/`const`. Keep `#define` for things only the preprocessor can do (include guards, conditional compilation).

---

## 9. Enumeration Constants (`enum`)

An **enumeration** creates a **set of named integer constants** — perfect for a fixed list of options.

### Plain `enum`

```cpp
#include <iostream>
using namespace std;

enum Day { MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY };

int main() {
    Day today = WEDNESDAY;
    cout << "Day number: " << today << endl;   // 2
    return 0;
}
```

By default values start at **0** and increase by **1**:

```
MONDAY=0, TUESDAY=1, WEDNESDAY=2, THURSDAY=3, FRIDAY=4, SATURDAY=5, SUNDAY=6
```

### Custom Values

```cpp
enum HttpStatus {
    OK = 200,
    NOT_FOUND = 404,
    SERVER_ERROR = 500
};

enum Level { LOW = 1, MEDIUM, HIGH };   // MEDIUM = 2, HIGH = 3 (continues from previous)
```

### Scoped `enum class` (C++11) ✅ Recommended

Plain enums leak their names into the surrounding scope, which causes conflicts:

```cpp
enum Color { RED, GREEN };
enum TrafficLight { RED, YELLOW };   // ❌ error: RED already declared
```

`enum class` keeps names inside the enum:

```cpp
#include <iostream>
using namespace std;

enum class Color { Red, Green, Blue };
enum class TrafficLight { Red, Yellow, Green };   // ✅ no conflict

int main() {
    Color shirt = Color::Red;
    TrafficLight signal = TrafficLight::Green;

    if (signal == TrafficLight::Green) {
        cout << "Go!" << endl;
    }

    cout << static_cast<int>(shirt) << endl;   // 0 — explicit conversion needed
    return 0;
}
```

| | `enum` | `enum class` |
|--|--------|--------------|
| Access | `RED` | `Color::Red` |
| Name conflicts | ❌ Possible | ✅ None |
| Auto-converts to `int` | Yes | No (needs `static_cast`) |
| Recommended | Legacy code | ✅ Modern C++ |

> 📚 Enums are covered in more depth in a later lesson.

---

## 10. `const` with Function Parameters

`const` on a parameter means **the function promises not to modify it**.

```cpp
#include <iostream>
using namespace std;

double calculateTax(const double amount, const double rate) {
    // amount = 0;   // ❌ error: assignment of read-only parameter
    return amount * rate;
}

int main() {
    cout << calculateTax(1000, 0.18) << endl;   // 180
    return 0;
}
```

### Preview: `const` References (very common!)
```cpp
void printName(const string& name) {   // no copy, and cannot be modified
    cout << name << endl;
}
```

This pattern (`const Type&`) is used everywhere in professional C++. You'll learn **references** in a later lesson — for now, recognize that `const` protects the value.

### Preview: `const` with Pointers
```cpp
const int* p1;        // pointer to a constant int (value can't change)
int* const p2 = &x;   // constant pointer (address can't change)
```
> 📚 Covered fully in the Pointers lesson.

---

## 11. `consteval` and `constinit` (Advanced)

> 🎓 *Optional — C++20 features. Good to recognize, not required for beginners.*

| Keyword | Meaning |
|---------|---------|
| `consteval` | A function that **must** be evaluated at compile time — calling it with a runtime value is an error |
| `constinit` | A variable that **must be initialized at compile time**, but **can be changed** later |

```cpp
consteval int cube(int x) {
    return x * x * x;
}

constinit int counter = 0;   // initialized at compile time, can still change

int main() {
    constexpr int a = cube(3);   // ✅ 27, computed at compile time
    // int n; std::cin >> n;
    // int b = cube(n);          // ❌ error: n is not a constant

    counter++;                   // ✅ constinit doesn't mean const
    return 0;
}
```

Compile with: `g++ -std=c++20 file.cpp`

### The Full "const Family"

| Keyword | Compile-time required? | Can change later? |
|---------|-----------------------|-------------------|
| `const` | ❌ | ❌ |
| `constexpr` | ✅ | ❌ |
| `consteval` | ✅ (functions only) | — |
| `constinit` | ✅ (initialization only) | ✅ |

---

## 12. Magic Numbers

A **magic number** is an unexplained number written directly in code.

### ❌ With Magic Numbers

```cpp
double total = price + price * 0.18;
if (marks >= 33) { ... }
for (int i = 0; i < 7; i++) { ... }
double area = 3.14159 * r * r;
```

Questions a reader asks: *What is 0.18? Why 33? Why 7?*

### ✅ With Named Constants

```cpp
constexpr double GST_RATE = 0.18;
constexpr int PASSING_MARKS = 33;
constexpr int DAYS_IN_WEEK = 7;
constexpr double PI = 3.14159;

double total = price + price * GST_RATE;
if (marks >= PASSING_MARKS) { ... }
for (int i = 0; i < DAYS_IN_WEEK; i++) { ... }
double area = PI * r * r;
```

Now the code **explains itself**. ✨

### When Are Plain Numbers OK?
- `0` and `1` in obvious situations (starting a loop at 0, adding 1)
- `2` in clearly mathematical formulas (`2 * PI * r`, dividing by 2 for half)
- Values that are self-explanatory in context

📌 **Rule of thumb:** if a number makes someone ask *"why that number?"*, give it a name.

---

## 13. Which Constant Should I Use?

```
              Do you need a fixed value?
                         │
         ┌───────────────┴────────────────┐
         │                                │
   A set of related              A single value
   named options?                         │
   (days, colors, levels)                 │
         │                    Known while writing code?
         ▼                         │              │
    enum class                    YES             NO
                                   │       (input / calculated)
                                   ▼              ▼
                               constexpr        const

     #define → only for include guards & conditional compilation
```

### Quick Reference

| Situation | Use |
|-----------|-----|
| π, gravity, days in a week, tax rate | `constexpr` |
| Value from user input that shouldn't change after | `const` |
| Array size | `constexpr` |
| Set of named choices (menu options, directions, states) | `enum class` |
| Function parameter you won't modify | `const` |
| Include guards, platform-specific code | `#define` |
| Old code you're reading | You'll see `#define` a lot — understand it, but don't copy the habit |

### Naming Constants
Common styles (from Lesson 5):
```cpp
constexpr int MAX_STUDENTS = 60;   // UPPER_SNAKE_CASE — most common in teaching
constexpr int kMaxStudents = 60;   // 'k' prefix — Google style guide
```

---

## 14. Common Beginner Mistakes

| ❌ Mistake | ✅ Correct | Why |
|-----------|-----------|-----|
| `const int MAX;` | `const int MAX = 100;` | `const` must be initialized |
| `const int X = 5; X = 10;` | Use a normal variable if it must change | Can't modify a constant |
| `constexpr int N = userInput;` | `const int N = userInput;` | `constexpr` needs compile-time values |
| `#define PI = 3.14` | `#define PI 3.14` | No `=` in `#define` |
| `#define PI 3.14;` | `#define PI 3.14` | The `;` becomes part of the replacement |
| `#define SQUARE(x) x*x` | `constexpr int square(int x)` | Macro gives wrong results with expressions |
| `int code = 0123;` expecting 123 | `int code = 123;` | Leading `0` means **octal** (=83) |
| `char c = "A";` | `char c = 'A';` | Single quotes for characters |
| `cout << "C:\new\test";` | `"C:\\new\\test"` or `R"(C:\new\test)"` | `\n` and `\t` are escape sequences! |
| `float f = 3.14;` (warning in some setups) | `float f = 3.14f;` | `3.14` is a `double` literal |
| Two plain `enum`s with the same value names | Use `enum class` | Name conflicts |
| Magic numbers everywhere | Named `constexpr` values | Readability & maintenance |

---

## 15. Practice Exercises

### 🟢 Easy

**Exercise 1:** Declare constants for the number of days in a week, hours in a day, and minutes in an hour. Use them to calculate and print the **total minutes in a week**.

**Exercise 2:** Print the following output using **escape sequences** only (one `cout` statement):
```
Name	Marks
Riya	95
"Well done!"
```

**Exercise 3:** Predict the output:
```cpp
cout << 10 << " " << 010 << " " << 0x10 << " " << 0b10 << endl;
```

### 🟡 Medium

**Exercise 4:** Take the **radius** as input. Using a `constexpr` for π, print the area and circumference of the circle.

**Exercise 5:** Ask the user for their **birth year**, then store their age in a `const int`. Try to change it and note the compiler error message.

**Exercise 6:** Find and fix **all** errors:
```cpp
#include <iostream>
using namespace std;

#define TAX_RATE = 0.18;
const int MAX_ITEMS;

int main() {
    int n;
    cin >> n;
    constexpr int LIMIT = n;
    cout << TAX_RATE << " " << MAX_ITEMS << " " << LIMIT;
    return 0;
}
```

**Exercise 7:** Print this Windows file path using a **raw string literal**:
```
C:\Users\Student\Documents\new_folder\test.cpp
```
Then print it again using a **normal string** with escape sequences.

### 🔴 Challenge

**Exercise 8:** Explain why the output is not 25, then fix it in **two** ways:
```cpp
#define SQUARE(x) x * x
cout << SQUARE(2 + 3);
```

**Exercise 9:** Create an `enum class Grade { A, B, C, D, F };`. Store a student's grade, and print its **numeric value** using `static_cast<int>`.

**Exercise 10:** Rewrite this program to remove **all magic numbers**:
```cpp
#include <iostream>
using namespace std;

int main() {
    double basicSalary;
    cin >> basicSalary;

    double hra = basicSalary * 0.40;
    double da = basicSalary * 0.12;
    double pf = basicSalary * 0.12;
    double gross = basicSalary + hra + da;
    double net = gross - pf;

    cout << "Gross: " << gross << endl;
    cout << "Net  : " << net << endl;
    return 0;
}
```

---

### ✅ Solutions

<details>
<summary>Click to view Exercise 1 solution</summary>

```cpp
#include <iostream>
using namespace std;

constexpr int DAYS_IN_WEEK = 7;
constexpr int HOURS_IN_DAY = 24;
constexpr int MINUTES_IN_HOUR = 60;

int main() {
    int minutesInWeek = DAYS_IN_WEEK * HOURS_IN_DAY * MINUTES_IN_HOUR;
    cout << "Minutes in a week: " << minutesInWeek << endl;   // 10080
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
    cout << "Name\tMarks\nRiya\t95\n\"Well done!\"\n";
    return 0;
}
```
</details>

<details>
<summary>Click to view Exercise 3 answer</summary>

**Output:**
```
10 8 16 2
```
- `10` → decimal = 10
- `010` → octal = 8
- `0x10` → hexadecimal = 16
- `0b10` → binary = 2
</details>

<details>
<summary>Click to view Exercise 4 solution</summary>

```cpp
#include <iostream>
using namespace std;

constexpr double PI = 3.14159;

int main() {
    double radius;
    cout << "Enter radius: ";
    cin >> radius;

    cout << "Area          : " << PI * radius * radius << endl;
    cout << "Circumference : " << 2 * PI * radius << endl;
    return 0;
}
```
</details>

<details>
<summary>Click to view Exercise 5 solution</summary>

```cpp
#include <iostream>
using namespace std;

constexpr int CURRENT_YEAR = 2026;

int main() {
    int birthYear;
    cout << "Enter birth year: ";
    cin >> birthYear;

    const int AGE = CURRENT_YEAR - birthYear;   // runtime value → const, not constexpr
    cout << "Your age: " << AGE << endl;

    // AGE = 50;   // ❌ error: assignment of read-only variable 'AGE'
    return 0;
}
```
</details>

<details>
<summary>Click to view Exercise 6 solution</summary>

**Errors:**
1. `#define TAX_RATE = 0.18;` → no `=` and no `;` in `#define`
2. `const int MAX_ITEMS;` → `const` must be initialized
3. `constexpr int LIMIT = n;` → `n` is a runtime value; use `const`

```cpp
#include <iostream>
using namespace std;

constexpr double TAX_RATE = 0.18;   // better than #define
const int MAX_ITEMS = 50;

int main() {
    int n;
    cin >> n;
    const int LIMIT = n;
    cout << TAX_RATE << " " << MAX_ITEMS << " " << LIMIT;
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
    cout << R"(C:\Users\Student\Documents\new_folder\test.cpp)" << endl;
    cout << "C:\\Users\\Student\\Documents\\new_folder\\test.cpp" << endl;
    return 0;
}
```
> 💡 Without escaping, `\n` in `\new_folder` and `\t` in `\test.cpp` would become a new line and a tab!
</details>

<details>
<summary>Click to view Exercise 8 solution</summary>

**Why:** `#define` is text replacement, so `SQUARE(2 + 3)` becomes `2 + 3 * 2 + 3` = 2 + 6 + 3 = **11**.

**Fix 1 — parentheses in the macro:**
```cpp
#define SQUARE(x) ((x) * (x))
cout << SQUARE(2 + 3);   // ((2 + 3) * (2 + 3)) = 25
```

**Fix 2 — use a `constexpr` function (best):**
```cpp
constexpr int square(int x) {
    return x * x;
}
cout << square(2 + 3);   // 25
```
</details>

<details>
<summary>Click to view Exercise 9 solution</summary>

```cpp
#include <iostream>
using namespace std;

enum class Grade { A, B, C, D, F };

int main() {
    Grade myGrade = Grade::B;
    cout << "Grade value: " << static_cast<int>(myGrade) << endl;   // 1
    return 0;
}
```
</details>

<details>
<summary>Click to view Exercise 10 solution</summary>

```cpp
#include <iostream>
using namespace std;

constexpr double HRA_RATE = 0.40;   // House Rent Allowance
constexpr double DA_RATE  = 0.12;   // Dearness Allowance
constexpr double PF_RATE  = 0.12;   // Provident Fund deduction

int main() {
    double basicSalary;
    cout << "Enter basic salary: ";
    cin >> basicSalary;

    double hra   = basicSalary * HRA_RATE;
    double da    = basicSalary * DA_RATE;
    double pf    = basicSalary * PF_RATE;
    double gross = basicSalary + hra + da;
    double net   = gross - pf;

    cout << "Gross: " << gross << endl;
    cout << "Net  : " << net << endl;
    return 0;
}
```
</details>

---

## 16. Quick Quiz

**Q1.** Which keyword makes a variable read-only?
- a) `static`
- b) `const`
- c) `final`
- d) `fixed`

**Q2.** What is the value of `0x1F`?
- a) 15
- b) 31
- c) 16
- d) 1F

**Q3.** What is the value of `012`?
- a) 12
- b) 10
- c) 2
- d) Error

**Q4.** Which is **correct**?
- a) `#define MAX = 100`
- b) `#define MAX 100;`
- c) `#define MAX 100`
- d) `define MAX 100`

**Q5.** Which line causes a compile error?
- a) `const int A = 10;`
- b) `constexpr int B = 20;`
- c) `const int C;`
- d) `const double D = 3.5;`

**Q6.** `int n; cin >> n;` — which is valid?
- a) `constexpr int X = n;`
- b) `const int X = n;`
- c) `#define X n` then `constexpr int Y = X;`
- d) None

**Q7.** What does `1.5e3` equal?
- a) 1.53
- b) 15
- c) 150
- d) 1500

**Q8.** Which escape sequence prints a backslash?
- a) `\b`
- b) `\\`
- c) `\/`
- d) `\s`

**Q9.** What is the default first value in `enum Color { RED, GREEN, BLUE };`?
- a) 1
- b) 0
- c) -1
- d) Undefined

**Q10.** Why is `constexpr` preferred over `#define` for constants?
- a) It is shorter to type
- b) It has a type and respects scope
- c) It runs faster at runtime
- d) `#define` doesn't work in C++

<details>
<summary>✅ Click to view answers</summary>

| Q | Answer | Explanation |
|---|--------|-------------|
| 1 | b) `const` | |
| 2 | b) 31 | 1×16 + 15 = 31 |
| 3 | b) 10 | Leading `0` = octal: 1×8 + 2 = 10 |
| 4 | c) | No `=` and no `;` |
| 5 | c) | `const` must be initialized |
| 6 | b) | `const` accepts runtime values |
| 7 | d) 1500 | 1.5 × 10³ |
| 8 | b) `\\` | |
| 9 | b) 0 | Enums start at 0 by default |
| 10 | b) | Type safety and scope |

</details>

---

## 17. Summary

- ✅ A **constant** is a value that **cannot change** during program execution.
- ✅ Constants improve **safety, readability, maintenance**, and enable **optimization**.
- ✅ **Literals** are fixed values written in code: integers, floats, characters, strings, `true`/`false`, `nullptr`.
- ✅ Integer literals can be **decimal**, **octal (`0`)**, **hex (`0x`)**, or **binary (`0b`)**; use `'` as a digit separator.
- ✅ **Suffixes** set literal types: `u`, `L`, `LL`, `f`.
- ✅ **Escape sequences** (`\n`, `\t`, `\\`, `\"`) print special characters; **raw strings** `R"(...)"` avoid escaping.
- ✅ **`const`** = read-only after initialization; must be initialized; can hold runtime values.
- ✅ **`constexpr`** = value **must** be known at compile time; preferred for fixed values.
- ✅ **`#define`** = text replacement — no type, no scope; avoid for constants in modern C++.
- ✅ **`enum class`** = a safe set of named constants.
- ✅ `const` parameters protect function arguments from modification.
- ✅ Replace **magic numbers** with named constants.

---



---

> 📝 *Tip for students: Before writing any number in your code, ask "Will someone understand why this number is here?" If not, give it a name with `constexpr`!*
