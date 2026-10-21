#  Structure of a C++ Program

> **Goal of this lesson:** Understand how a C++ program is organized — every section, from comments and header files to `main()`, functions, namespaces, and multi-file projects — so you can read and write well-structured code.

---

## 📑 Table of Contents

1. [Why Program Structure Matters](#1-why-program-structure-matters)
2. [The Big Picture: Sections of a C++ Program](#2-the-big-picture-sections-of-a-c-program)
3. [Section 1: Documentation (Comments)](#3-section-1-documentation-comments)
4. [Section 2: Preprocessor Directives](#4-section-2-preprocessor-directives)
5. [Section 3: Namespaces](#5-section-3-namespaces)
6. [Section 4: Global Declarations](#6-section-4-global-declarations)
7. [Section 5: Function Declarations (Prototypes)](#7-section-5-function-declarations-prototypes)
8. [Section 6: The `main()` Function](#8-section-6-the-main-function)
9. [Section 7: User-Defined Functions](#9-section-7-user-defined-functions)
10. [Statements, Blocks, and Tokens](#10-statements-blocks-and-tokens)
11. [Whitespace, Indentation, and Style](#11-whitespace-indentation-and-style)
12. [Multi-File Program Structure](#12-multi-file-program-structure)
13. [Types of Errors](#13-types-of-errors)
14. [Common Beginner Mistakes](#14-common-beginner-mistakes)
15. [Practice Exercises](#15-practice-exercises)
16. [Quick Quiz](#16-quick-quiz)
17. [Summary](#17-summary)

---

## 1. Why Program Structure Matters

A C++ program isn't just a pile of statements. It follows a **structure** — a fixed order of sections that the compiler expects and other programmers recognise.

🏠 **Analogy:** A program is like a **house**.
- **Foundation** → header files (`#include`) — tools you build with
- **Blueprint notes** → comments
- **Main entrance** → `main()` — where everyone (the computer) enters
- **Rooms** → functions — each has its own job
- **Address** → namespace — prevents mixing up houses with the same name

A well-structured program is:
- ✅ **Easier to read** — for you and others
- ✅ **Easier to debug** — errors are easier to find
- ✅ **Easier to extend** — add features without breaking things

---

## 2. The Big Picture: Sections of a C++ Program

```
┌─────────────────────────────────────────────┐
│ 1. Documentation Section (comments)         │  ← What the program does, author, date
├─────────────────────────────────────────────┤
│ 2. Preprocessor Directives (#include, ...)  │  ← Header files and macros
├─────────────────────────────────────────────┤
│ 3. Namespace Declaration                    │  ← using namespace std; (optional)
├─────────────────────────────────────────────┤
│ 4. Global Declarations                      │  ← Global variables & constants
├─────────────────────────────────────────────┤
│ 5. Function Declarations (Prototypes)       │  ← Tells compiler functions exist
├─────────────────────────────────────────────┤
│ 6. main() Function                          │  ← ⭐ Program starts here (REQUIRED)
├─────────────────────────────────────────────┤
│ 7. User-Defined Function Definitions        │  ← Code for your own functions
└─────────────────────────────────────────────┘
```

📌 **Only `main()` is required.** All other sections are optional but used in almost every real program.

### A Complete Example Showing Every Section

```cpp
/*
 * Program : Rectangle Calculator
 * Author  : Your Name
 * Date    : 2026
 * Purpose : Calculates area and perimeter of a rectangle
 */

// ===== 2. Preprocessor Directives =====
#include <iostream>
#include <string>

// ===== 3. Namespace =====
using namespace std;

// ===== 4. Global Declarations =====
const string APP_NAME = "Rectangle Calculator";

// ===== 5. Function Prototypes =====
double calculateArea(double length, double width);
double calculatePerimeter(double length, double width);

// ===== 6. main() Function =====
int main() {
    double length, width;

    cout << "=== " << APP_NAME << " ===" << endl;
    cout << "Enter length: ";
    cin >> length;
    cout << "Enter width: ";
    cin >> width;

    cout << "Area      = " << calculateArea(length, width) << endl;
    cout << "Perimeter = " << calculatePerimeter(length, width) << endl;

    return 0;
}

// ===== 7. Function Definitions =====
double calculateArea(double length, double width) {
    return length * width;
}

double calculatePerimeter(double length, double width) {
    return 2 * (length + width);
}
```

**Sample run:**
```
=== Rectangle Calculator ===
Enter length: 5
Enter width: 3
Area      = 15
Perimeter = 16
```

Now let's study each section in detail. 👇

---

## 3. Section 1: Documentation (Comments)

**Comments** are notes for humans. The compiler **completely ignores** them.

### Types of Comments

```cpp
// Single-line comment: everything after // on this line is ignored

/*
   Multi-line comment:
   can span many lines
*/

int age = 20;   // Inline comment: explains this line
```

### Documentation Header (Good Practice)
Start every program file with a header comment:

```cpp
/*
 * File    : student_report.cpp
 * Author  : Priya Verma
 * Date    : 21-Oct-2026
 * Purpose : Calculates and displays student grades
 */
```

### Good vs Bad Comments

```cpp
// ❌ Bad: repeats what the code already says
x = x + 1;   // add 1 to x

// ✅ Good: explains WHY
x = x + 1;   // skip the header row in the file
```

💡 **Tip:** Comments are also useful for **temporarily disabling code** while debugging:
```cpp
// cout << "Debug value: " << total << endl;
```

⚠️ Multi-line comments **cannot be nested**:
```cpp
/* outer /* inner */ still outer */   // ❌ error: comment ends at the first */
```

---

## 4. Section 2: Preprocessor Directives

Lines beginning with **`#`** are **preprocessor directives**. They are processed **before compilation**.

📌 They do **not** end with a semicolon.

### 4.1 `#include` — Including Header Files

A **header file** contains declarations of ready-made tools (functions, classes, objects) you can use.

```cpp
#include <iostream>    // cin, cout
#include <string>      // string
#include <cmath>       // sqrt(), pow(), abs()
```

### Angle Brackets `< >` vs Quotes `" "`

| Syntax | Searches | Used For |
|--------|----------|----------|
| `#include <iostream>` | System / standard library folders | Standard library headers |
| `#include "myfile.h"` | **Your project folder first**, then system folders | Your own header files |

### Commonly Used Standard Headers

| Header | Provides |
|--------|----------|
| `<iostream>` | `cin`, `cout`, `cerr`, `endl` |
| `<string>` | `string` class |
| `<cmath>` | `sqrt`, `pow`, `sin`, `cos`, `abs`, `ceil`, `floor` |
| `<iomanip>` | `setw`, `setprecision`, `fixed` (formatting output) |
| `<vector>` | `vector` (dynamic arrays) |
| `<algorithm>` | `sort`, `max`, `min`, `reverse` |
| `<climits>` | `INT_MAX`, `INT_MIN` |
| `<fstream>` | File input/output |

💡 **What does `#include` actually do?** The preprocessor literally **copies the entire content** of the header file into your program at that line.

### ⚠️ About `<bits/stdc++.h>`
You may see this in competitive programming:
```cpp
#include <bits/stdc++.h>   // includes almost everything
```
- Works only with **GCC** — not standard C++, fails on MSVC
- Slows down compilation
- ✅ Fine for contests, ❌ avoid in real projects and assignments

### 4.2 `#define` — Macros

Creates a **text replacement** before compilation.

```cpp
#define PI 3.14159
#define MAX_SIZE 100

double area = PI * r * r;   // preprocessor replaces PI with 3.14159
```

💡 **Modern C++ prefers `const` / `constexpr`** over `#define` for constants, because they have a type and follow scope rules:
```cpp
constexpr double PI = 3.14159;   // ✅ preferred
```

### 4.3 Other Directives (Preview)

| Directive | Purpose |
|-----------|---------|
| `#ifndef` / `#define` / `#endif` | Include guards (see Section 12) |
| `#pragma once` | Simpler include guard |
| `#if`, `#ifdef`, `#else` | Conditional compilation |

---

## 5. Section 3: Namespaces

### The Problem
Imagine two libraries both have a function called `print()`. How does the compiler know which one you mean? 🤔

### The Solution: Namespaces
A **namespace** is a **named container** that groups related names and prevents conflicts.

🏫 **Analogy:** Two students named "Rahul" in different classes. You say **"Rahul from 10-A"** vs **"Rahul from 10-B"**. The class is the namespace!

### The `std` Namespace
All standard library names (`cout`, `cin`, `string`, `endl`, ...) live in the **`std`** namespace.

### Three Ways to Use `std`

**Way 1: Full name every time** (most explicit, professional style ✅)
```cpp
#include <iostream>

int main() {
    std::cout << "Hello" << std::endl;
    return 0;
}
```

**Way 2: `using` declaration** — bring in only specific names ✅
```cpp
#include <iostream>
using std::cout;
using std::endl;

int main() {
    cout << "Hello" << endl;
    return 0;
}
```

**Way 3: `using` directive** — bring in everything from `std`
```cpp
#include <iostream>
using namespace std;

int main() {
    cout << "Hello" << endl;
    return 0;
}
```

| Approach | Pros | Cons |
|----------|------|------|
| `std::cout` | No conflicts, clear | More typing |
| `using std::cout;` | Clean and safe | Must list each name |
| `using namespace std;` | Least typing | Possible name conflicts in big projects; **never put it in header files** |

📌 **In this course:** we use `using namespace std;` in small programs to keep examples short. That's perfectly fine for learning.

### Creating Your Own Namespace

```cpp
#include <iostream>
using namespace std;

namespace English {
    void greet() { cout << "Hello!" << endl; }
}

namespace Hindi {
    void greet() { cout << "Namaste!" << endl; }
}

int main() {
    English::greet();   // Hello!
    Hindi::greet();     // Namaste!
    return 0;
}
```

Same function name, no conflict — thanks to namespaces! 🎉

---

## 6. Section 4: Global Declarations

Variables and constants declared **outside all functions** are **global** — accessible from every function in the file.

```cpp
#include <iostream>
using namespace std;

const double TAX_RATE = 0.18;   // global constant ✅ (good use)
int visitorCount = 0;           // global variable ⚠️ (use carefully)

void addVisitor() {
    visitorCount++;
}

int main() {
    addVisitor();
    addVisitor();
    cout << "Visitors: " << visitorCount << endl;     // 2
    cout << "Tax rate: " << TAX_RATE * 100 << "%" << endl;
    return 0;
}
```

💡 **Best practice:**
- ✅ Global **constants** are fine and common
- ⚠️ Avoid global **variables** — any function can change them, making bugs hard to trace

---

## 7. Section 5: Function Declarations (Prototypes)

### The Problem
The compiler reads code **from top to bottom**. If you call a function before the compiler has seen it, you get an error:

```cpp
#include <iostream>
using namespace std;

int main() {
    sayHello();      // ❌ error: 'sayHello' was not declared in this scope
    return 0;
}

void sayHello() {
    cout << "Hello!" << endl;
}
```

### Solution 1: Define the function before `main()`

```cpp
void sayHello() {
    cout << "Hello!" << endl;
}

int main() {
    sayHello();      // ✅ works
    return 0;
}
```

### Solution 2: Use a Function Prototype ✅ (common in structured programs)

A **prototype** tells the compiler: *"this function exists; its full code comes later."*

```cpp
#include <iostream>
using namespace std;

void sayHello();     // prototype (note the semicolon!)

int main() {
    sayHello();      // ✅ works
    return 0;
}

void sayHello() {    // definition
    cout << "Hello!" << endl;
}
```

### Declaration vs Definition

| | Declaration (Prototype) | Definition |
|--|--|--|
| Example | `int add(int a, int b);` | `int add(int a, int b) { return a + b; }` |
| Has body `{ }`? | ❌ No | ✅ Yes |
| Ends with `;`? | ✅ Yes | ❌ No |
| Purpose | Tells the compiler it exists | Provides the actual code |
| How many times? | Can appear many times | Exactly **once** |

💡 In prototypes, parameter names are optional: `int add(int, int);` is valid. Keeping names makes it more readable.

> 📚 Functions are covered in full detail in a later lesson. For now, just understand **where** they go in a program.

---

## 8. Section 6: The `main()` Function

### ⭐ Key Facts
- Every C++ program **must have exactly one** `main()` function
- Execution **always starts** at the first line of `main()`
- When `main()` ends, the **program ends**

### Valid Forms of `main()`

```cpp
int main() {
    // ...
    return 0;
}
```

```cpp
int main(int argc, char* argv[]) {
    // argc = number of command-line arguments
    // argv = the arguments themselves
    return 0;
}
```

❌ `void main()` is **not standard C++** — some old compilers (Turbo C++) accept it, but modern compilers reject it.

### Return Value of `main()`

| Return | Meaning |
|--------|---------|
| `return 0;` | Program finished **successfully** |
| Non-zero (e.g. `return 1;`) | Program ended with an **error** |

💡 If you leave out `return 0;` in `main()`, the compiler automatically treats it as `return 0;`. (This is true **only** for `main()`, not other functions.) Writing it explicitly is still good practice.

### Command-Line Arguments Example (Bonus)

```cpp
#include <iostream>
using namespace std;

int main(int argc, char* argv[]) {
    cout << "Number of arguments: " << argc << endl;
    for (int i = 0; i < argc; i++) {
        cout << "argv[" << i << "] = " << argv[i] << endl;
    }
    return 0;
}
```

```bash
g++ args.cpp -o args
./args hello world
```

**Output:**
```
Number of arguments: 3
argv[0] = ./args
argv[1] = hello
argv[2] = world
```

### Flow of Execution

```
   Program starts
        │
        ▼
 ┌──────────────┐
 │   main()     │──── calls ───▶ calculateArea() ──┐
 │  statement 1 │                                  │
 │  statement 2 │◀──── returns value ──────────────┘
 │  statement 3 │──── calls ───▶ calculatePerimeter()
 │     ...      │◀──── returns value ──────────────
 │  return 0;   │
 └──────────────┘
        │
        ▼
   Program ends
```

---

## 9. Section 7: User-Defined Functions

Functions written by **you** to perform specific tasks. They keep `main()` short and organized.

### Without Functions (everything in `main`) ❌

```cpp
int main() {
    cout << "==========" << endl;
    cout << "Report Card" << endl;
    cout << "==========" << endl;
    // ... 50 more lines ...
    cout << "==========" << endl;
    cout << "Thank You" << endl;
    cout << "==========" << endl;
    return 0;
}
```

### With Functions ✅

```cpp
#include <iostream>
#include <string>
using namespace std;

void printLine();
void printTitle(string title);

int main() {
    printTitle("Report Card");
    cout << "Math: 90" << endl;
    cout << "Science: 85" << endl;
    printTitle("Thank You");
    return 0;
}

void printLine() {
    cout << "==========" << endl;
}

void printTitle(string title) {
    printLine();
    cout << title << endl;
    printLine();
}
```

📌 **Rule of thumb:** if you copy-paste the same code twice, it probably belongs in a function.

---

## 10. Statements, Blocks, and Tokens

### Statements
A **statement** is a single instruction. Most statements end with a **semicolon `;`**.

```cpp
int x = 10;              // declaration statement
x = x + 5;               // expression statement
cout << x;               // output statement
return 0;                // return statement
;                        // empty statement (valid, does nothing)
```

### Blocks (Compound Statements)
A **block** is a group of statements inside **curly braces `{ }`**. Blocks define **scope**.

```cpp
int main() {             // block starts
    int a = 5;
    {                    // nested block
        int b = 10;
        cout << a + b;
    }                    // b is destroyed here
    return 0;
}                        // block ends
```

📌 A block itself does **not** need a semicolon after `}` (except for things like `class` and `struct` — coming later).

### Tokens — The Building Blocks
The compiler breaks your code into the smallest meaningful units called **tokens**.

```cpp
int total = price + 50;
```
Tokens: `int` | `total` | `=` | `price` | `+` | `50` | `;`

| Token Type | Description | Examples |
|------------|-------------|----------|
| **Keywords** | Reserved words with special meaning | `int`, `return`, `if`, `class`, `while` |
| **Identifiers** | Names you give | `total`, `price`, `calculateArea` |
| **Literals** | Fixed values | `50`, `3.14`, `'A'`, `"Hello"`, `true` |
| **Operators** | Perform operations | `+`, `-`, `=`, `<<`, `==` |
| **Punctuators** | Structure the code | `;`, `{ }`, `( )`, `[ ]`, `,` |

### Some C++ Keywords

```
int      float    double   char     bool     void     auto
if       else     switch   case     default  for      while
do       break    continue return   const    constexpr
class    struct   public   private  protected namespace using
new      delete   true     false    nullptr  this     static
```

> C++ has around **90+ keywords**. You'll learn them gradually — no need to memorize now!

---

## 11. Whitespace, Indentation, and Style

### Whitespace
Spaces, tabs, and newlines are mostly **ignored** by the compiler. These two programs are identical to the compiler:

```cpp
int main(){int a=5;int b=10;std::cout<<a+b;return 0;}
```

```cpp
int main() {
    int a = 5;
    int b = 10;
    std::cout << a + b;
    return 0;
}
```

But the second one is **far easier for humans** to read! 👀

⚠️ Exceptions: whitespace matters **inside strings** (`"Hello World"`) and **inside tokens** (`int` cannot be written as `i nt`).

### Indentation
Indent code inside every block by **4 spaces** (or 1 tab) consistently.

```cpp
// ❌ Hard to read
int main() {
int x = 5;
if (x > 0) {
cout << "Positive";
}
return 0;
}

// ✅ Easy to read
int main() {
    int x = 5;
    if (x > 0) {
        cout << "Positive";
    }
    return 0;
}
```

### Brace Styles
Both are common — pick one and be **consistent**:

```cpp
// K&R / Java style
int main() {
    return 0;
}

// Allman style
int main()
{
    return 0;
}
```

### ✅ Style Checklist
- One statement per line
- Consistent indentation (4 spaces)
- Meaningful variable and function names
- Spaces around operators: `a + b`, not `a+b`
- Blank lines between logical sections
- Comment the **why**, not the **what**

💡 **Tip:** VS Code can auto-format your code: **Shift + Alt + F** (Windows) / **Shift + Option + F** (macOS).

---

## 12. Multi-File Program Structure

Real projects are split into **multiple files** to keep code organized.

### Typical Structure

```
calculator-project/
├── main.cpp          ← contains main()
├── mathutils.h       ← header: declarations (prototypes)
└── mathutils.cpp     ← source: definitions (actual code)
```

### 📄 `mathutils.h` (Header File — the "menu")

```cpp
#pragma once   // include guard: prevents including this file twice

int add(int a, int b);
int multiply(int a, int b);
```

### 📄 `mathutils.cpp` (Source File — the "kitchen")

```cpp
#include "mathutils.h"

int add(int a, int b) {
    return a + b;
}

int multiply(int a, int b) {
    return a * b;
}
```

### 📄 `main.cpp`

```cpp
#include <iostream>
#include "mathutils.h"   // our own header — uses quotes
using namespace std;

int main() {
    cout << "5 + 3 = " << add(5, 3) << endl;
    cout << "5 * 3 = " << multiply(5, 3) << endl;
    return 0;
}
```

### Compile Together

```bash
g++ main.cpp mathutils.cpp -o calculator
./calculator
```

**Output:**
```
5 + 3 = 8
5 * 3 = 15
```

🍽️ **Analogy:** The **header** is the restaurant **menu** (what's available). The **.cpp** file is the **kitchen** (how it's made). Customers (`main.cpp`) only need the menu!

### Include Guards
If a header is included twice, you get **redefinition errors**. Two ways to prevent this:

```cpp
// Option 1: #pragma once (simple, supported by all major compilers)
#pragma once
```

```cpp
// Option 2: Traditional include guard (100% standard)
#ifndef MATHUTILS_H
#define MATHUTILS_H

int add(int a, int b);
int multiply(int a, int b);

#endif
```

### File Types

| Extension | Purpose |
|-----------|---------|
| `.cpp` | Source file (definitions, `main()`) |
| `.h` / `.hpp` | Header file (declarations) |
| `.o` / `.obj` | Object file (compiled, not yet linked) |
| `.exe` / no extension | Final executable |

---

## 13. Types of Errors

Understanding program structure helps you understand **errors**.

| Error Type | When | Example | Who Catches It |
|------------|------|---------|----------------|
| **Syntax / Compile-time** | Rules of the language broken | Missing `;`, misspelled keyword | Compiler |
| **Linker** | Function declared but never defined, or `main` missing | `undefined reference to 'add(int, int)'` | Linker |
| **Runtime** | Program crashes while running | Dividing an integer by zero | Shows up when running |
| **Logical** | Program runs but gives the **wrong answer** | Using `+` instead of `*` | Only **you** (by testing!) |

### Examples

```cpp
// 1. Syntax error
int x = 5     // ❌ expected ';'

// 2. Linker error
int add(int a, int b);     // declared...
int main() {
    add(2, 3);             // ...but never defined anywhere
}                          // ❌ undefined reference to 'add(int, int)'

// 3. Runtime error
int a = 10, b = 0;
cout << a / b;             // ❌ crash / undefined behaviour

// 4. Logical error
double area = length + width;   // ❌ runs fine, but should be length * width
```

💡 **Logical errors are the hardest** — the compiler can't help you. Always test with values where you **know the correct answer**.

---

## 14. Common Beginner Mistakes

| ❌ Mistake | ✅ Correct | Why |
|-----------|-----------|-----|
| `#include <iostream>;` | `#include <iostream>` | Directives don't end with `;` |
| `#include "iostream"` for standard headers | `#include <iostream>` | Use `< >` for standard library |
| `void main()` | `int main()` | Not standard C++ |
| Calling a function before declaring it | Add a prototype above `main()` | Compiler reads top to bottom |
| Prototype without `;` → `int add(int a, int b)` | `int add(int a, int b);` | Prototypes end with `;` |
| Two `main()` functions in one project | Only one `main()` | Linker error: multiple definition |
| `using namespace std;` inside a header file | Use `std::` in headers | Forces it on every file that includes it |
| No indentation | Indent every block | Code becomes unreadable |
| Mismatched `{` `}` | Match every opening brace | Confusing compile errors |
| Compiling only `main.cpp` in a multi-file project | `g++ main.cpp mathutils.cpp` | Linker can't find definitions |

---

## 15. Practice Exercises

### 🟢 Easy

**Exercise 1:** Label each part of the program below with its section name (documentation, preprocessor, namespace, global, prototype, main, definition).
```cpp
// Program to greet a user
#include <iostream>
using namespace std;

const string GREETING = "Welcome";

void greet();

int main() {
    greet();
    return 0;
}

void greet() {
    cout << GREETING << "!" << endl;
}
```

**Exercise 2:** Write a program with a proper **documentation header** (file name, author, date, purpose) that prints your college name.

**Exercise 3:** Identify the tokens and their types in:
```cpp
float price = 99.5;
```

### 🟡 Medium

**Exercise 4:** The program below doesn't compile. Fix it **using a function prototype** (don't move the function).
```cpp
#include <iostream>
using namespace std;

int main() {
    showMessage();
    return 0;
}

void showMessage() {
    cout << "Structure matters!" << endl;
}
```

**Exercise 5:** Rewrite this program **without** `using namespace std;`
```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string name;
    cout << "Enter name: ";
    cin >> name;
    cout << "Hi " << name << endl;
    return 0;
}
```

**Exercise 6:** Create two namespaces `Circle` and `Square`, each with a function `area()` that takes one `double` and returns the area. Call both from `main()`.

**Exercise 7:** Reformat this code with proper indentation, spacing, and one statement per line:
```cpp
#include <iostream>
using namespace std;int main(){int a=10;int b=20;int sum=a+b;cout<<"Sum: "<<sum<<endl;return 0;}
```

### 🔴 Challenge

**Exercise 8:** Write a well-structured **Student Info** program that has all 7 sections:
- Documentation header
- Required `#include`s
- Namespace
- A global constant `SCHOOL_NAME`
- Prototypes for `printHeader()` and `printStudent(string name, int roll)`
- `main()` that reads a student's name and roll number, then calls both functions
- Function definitions after `main()`

**Exercise 9:** Create a **multi-file project**:
- `geometry.h` → prototypes for `rectangleArea` and `triangleArea`
- `geometry.cpp` → definitions
- `main.cpp` → uses both functions

Compile it with `g++`.

**Exercise 10:** Classify each error as **Syntax, Linker, Runtime, or Logical**:
1. `cout << "Hello"` (missing semicolon)
2. A function is declared but its body is never written
3. Average calculated as `(a + b + c) / 2`
4. `int x = 10 / 0;` happening while the program runs with user input `0`
5. Writing `retrun 0;`

---

### ✅ Solutions

<details>
<summary>Click to view Exercise 1 solution</summary>

```cpp
// Program to greet a user              ← 1. Documentation
#include <iostream>                     ← 2. Preprocessor directive
using namespace std;                    ← 3. Namespace

const string GREETING = "Welcome";      ← 4. Global declaration

void greet();                           ← 5. Function prototype

int main() {                            ← 6. main() function
    greet();
    return 0;
}

void greet() {                          ← 7. Function definition
    cout << GREETING << "!" << endl;
}
```
> 💡 Bonus observation: this program uses `string` without `#include <string>`. It may compile because `<iostream>` sometimes includes it indirectly, but you should always add `#include <string>` yourself.
</details>

<details>
<summary>Click to view Exercise 2 solution</summary>

```cpp
/*
 * File    : college.cpp
 * Author  : Your Name
 * Date    : 21-Oct-2026
 * Purpose : Displays the college name
 */

#include <iostream>
using namespace std;

int main() {
    cout << "ABC Institute of Technology" << endl;
    return 0;
}
```
</details>

<details>
<summary>Click to view Exercise 3 solution</summary>

| Token | Type |
|-------|------|
| `float` | Keyword |
| `price` | Identifier |
| `=` | Operator |
| `99.5` | Literal |
| `;` | Punctuator |
</details>

<details>
<summary>Click to view Exercise 4 solution</summary>

```cpp
#include <iostream>
using namespace std;

void showMessage();   // prototype added

int main() {
    showMessage();
    return 0;
}

void showMessage() {
    cout << "Structure matters!" << endl;
}
```
</details>

<details>
<summary>Click to view Exercise 5 solution</summary>

```cpp
#include <iostream>
#include <string>

int main() {
    std::string name;
    std::cout << "Enter name: ";
    std::cin >> name;
    std::cout << "Hi " << name << std::endl;
    return 0;
}
```
</details>

<details>
<summary>Click to view Exercise 6 solution</summary>

```cpp
#include <iostream>
using namespace std;

namespace Circle {
    double area(double radius) {
        return 3.14159 * radius * radius;
    }
}

namespace Square {
    double area(double side) {
        return side * side;
    }
}

int main() {
    cout << "Circle area (r=2): " << Circle::area(2) << endl;
    cout << "Square area (s=4): " << Square::area(4) << endl;
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
    int a = 10;
    int b = 20;
    int sum = a + b;

    cout << "Sum: " << sum << endl;
    return 0;
}
```
</details>

<details>
<summary>Click to view Exercise 8 solution</summary>

```cpp
/*
 * File    : student_info.cpp
 * Author  : Your Name
 * Date    : 21-Oct-2026
 * Purpose : Reads and displays student information
 */

#include <iostream>
#include <string>

using namespace std;

const string SCHOOL_NAME = "Greenwood Public School";

void printHeader();
void printStudent(string name, int roll);

int main() {
    string name;
    int roll;

    cout << "Enter student name: ";
    getline(cin, name);
    cout << "Enter roll number: ";
    cin >> roll;

    printHeader();
    printStudent(name, roll);

    return 0;
}

void printHeader() {
    cout << "\n==============================" << endl;
    cout << "   " << SCHOOL_NAME << endl;
    cout << "==============================" << endl;
}

void printStudent(string name, int roll) {
    cout << "Name        : " << name << endl;
    cout << "Roll Number : " << roll << endl;
}
```
</details>

<details>
<summary>Click to view Exercise 9 solution</summary>

**geometry.h**
```cpp
#pragma once

double rectangleArea(double length, double width);
double triangleArea(double base, double height);
```

**geometry.cpp**
```cpp
#include "geometry.h"

double rectangleArea(double length, double width) {
    return length * width;
}

double triangleArea(double base, double height) {
    return 0.5 * base * height;
}
```

**main.cpp**
```cpp
#include <iostream>
#include "geometry.h"
using namespace std;

int main() {
    cout << "Rectangle (4 x 5): " << rectangleArea(4, 5) << endl;
    cout << "Triangle (b=6, h=3): " << triangleArea(6, 3) << endl;
    return 0;
}
```

**Compile & run:**
```bash
g++ main.cpp geometry.cpp -o geometry
./geometry
```

**Output:**
```
Rectangle (4 x 5): 20
Triangle (b=6, h=3): 9
```
</details>

<details>
<summary>Click to view Exercise 10 answers</summary>

| # | Error Type | Reason |
|---|------------|--------|
| 1 | Syntax | Missing semicolon |
| 2 | Linker | Declaration found, but no definition to link |
| 3 | Logical | Runs fine, but divides by 2 instead of 3 |
| 4 | Runtime | Division by zero happens while running |
| 5 | Syntax | `retrun` is not a keyword — compiler doesn't recognise it |
</details>

---

## 16. Quick Quiz

**Q1.** Which section is **mandatory** in every C++ program?
- a) Documentation
- b) `#include`
- c) `main()` function
- d) Namespace

**Q2.** Preprocessor directives begin with:
- a) `//`
- b) `#`
- c) `@`
- d) `$`

**Q3.** Which is used to include **your own** header file?
- a) `#include <myfile.h>`
- b) `#include "myfile.h"`
- c) `#import myfile.h`
- d) `using myfile.h;`

**Q4.** What is a function prototype?
- a) The function's full code
- b) A declaration telling the compiler a function exists
- c) A comment describing a function
- d) The first line of `main()`

**Q5.** Which namespace contains `cout` and `cin`?
- a) `io`
- b) `standard`
- c) `std`
- d) `main`

**Q6.** What does `return 0;` from `main()` mean?
- a) Error occurred
- b) Program ended successfully
- c) Restart the program
- d) Return to the first line

**Q7.** `undefined reference to 'add(int, int)'` is which type of error?
- a) Syntax error
- b) Linker error
- c) Runtime error
- d) Logical error

**Q8.** Which of these is a **keyword**?
- a) `main`
- b) `cout`
- c) `return`
- d) `std`

**Q9.** What is the purpose of `#pragma once`?
- a) Run the program once
- b) Prevent a header file from being included more than once
- c) Declare a constant
- d) Make `main()` run first

**Q10.** Which line is **correct**?
- a) `#include <iostream>;`
- b) `void main() { }`
- c) `int add(int a, int b);`
- d) `using namespace std`

<details>
<summary>✅ Click to view answers</summary>

| Q | Answer | Explanation |
|---|--------|-------------|
| 1 | c) `main()` | Execution starts there; everything else is optional |
| 2 | b) `#` | |
| 3 | b) `"myfile.h"` | Quotes search your project folder first |
| 4 | b) | Prototype = declaration without a body |
| 5 | c) `std` | |
| 6 | b) | Non-zero signals an error |
| 7 | b) Linker error | Declared but never defined |
| 8 | c) `return` | `main`, `cout`, `std` are identifiers, not keywords |
| 9 | b) | It's an include guard |
| 10 | c) | (a) extra `;` (b) non-standard (d) missing `;` |

</details>

---

## 17. Summary

- ✅ A C++ program has **7 sections**: Documentation → Preprocessor → Namespace → Globals → Prototypes → `main()` → Function definitions.
- ✅ **Only `main()` is required** — execution always starts there.
- ✅ **Comments** (`//`, `/* */`) are ignored by the compiler; explain **why**, not what.
- ✅ **Preprocessor directives** start with `#`, run before compilation, and have **no semicolon**.
- ✅ `#include <...>` for standard headers, `#include "..."` for your own.
- ✅ **Namespaces** prevent name conflicts; standard library names live in `std`.
- ✅ **Prototypes** let you call functions before their definitions.
- ✅ `int main()` returns `0` on success; `void main()` is not standard.
- ✅ Code is made of **tokens**: keywords, identifiers, literals, operators, punctuators.
- ✅ Use **consistent indentation and style** — the compiler doesn't care, but humans do.
- ✅ Large programs split into **headers (.h)** and **sources (.cpp)**, protected by **include guards**.
- ✅ Errors can be **syntax, linker, runtime, or logical**.

---


---

> 📝 *Tip for students: Take any program you wrote earlier and reorganize it with all 7 sections. Good structure is a habit — start building it now!*
