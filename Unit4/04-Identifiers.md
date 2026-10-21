# 📘  Identifiers in C++

> **Goal of this lesson:** Understand what identifiers are, the exact rules for creating them, which names are forbidden or reserved, how keywords differ from identifiers, and how to choose clear, professional names.

> 🔗 *Lesson 2 listed the basic naming rules for variables. This lesson covers identifiers in full — for variables, functions, classes, namespaces, and more.*

---

## 📑 Table of Contents

1. [What is an Identifier?](#1-what-is-an-identifier)
2. [Identifiers Among Tokens](#2-identifiers-among-tokens)
3. [Rules for Identifiers](#3-rules-for-identifiers)
4. [Valid vs Invalid Identifiers](#4-valid-vs-invalid-identifiers)
5. [Keywords: Names You Can't Use](#5-keywords-names-you-cant-use)
6. [Alternative Tokens (Surprise Keywords!)](#6-alternative-tokens-surprise-keywords)
7. [Identifiers with Special Meaning](#7-identifiers-with-special-meaning)
8. [Reserved Identifiers](#8-reserved-identifiers)
9. [Case Sensitivity](#9-case-sensitivity)
10. [Identifier Length](#10-identifier-length)
11. [Naming Conventions](#11-naming-conventions)
12. [Choosing Good Names](#12-choosing-good-names)
13. [Same Identifier, Different Scopes](#13-same-identifier-different-scopes)
14. [Common Beginner Mistakes](#14-common-beginner-mistakes)
15. [Practice Exercises](#15-practice-exercises)
16. [Quick Quiz](#16-quick-quiz)
17. [Summary](#17-summary)

---

## 1. What is an Identifier?

An **identifier** is a **name given by the programmer** to identify an element of the program.

🧑‍🎓 **Analogy:** In a classroom, every student has a **name** so the teacher can call on them. In a program, every variable, function, and class has an **identifier** so the compiler (and other programmers) can refer to it.

### Things That Need Identifiers

| Program Element | Example Identifier |
|-----------------|-------------------|
| Variable | `age`, `totalMarks` |
| Constant | `MAX_SIZE`, `PI` |
| Function | `calculateArea`, `printReport` |
| Class / Struct | `Student`, `BankAccount` |
| Namespace | `Billing`, `MathUtils` |
| Enum and its values | `Color`, `RED`, `GREEN` |
| Function parameter | `length`, `width` |
| Label (for `goto`) | `retry` |

### Example

```cpp
#include <iostream>
using namespace std;

namespace Geometry {                       // Geometry       → namespace identifier
    const double PI = 3.14159;             // PI             → constant identifier

    double circleArea(double radius) {     // circleArea     → function identifier
        return PI * radius * radius;       // radius         → parameter identifier
    }
}

int main() {                               // main           → function identifier
    double userRadius = 5;                 // userRadius     → variable identifier
    cout << Geometry::circleArea(userRadius) << endl;
    return 0;
}
```

📌 **Not identifiers** in this program: `#include`, `namespace`, `const`, `double`, `return`, `int` (these are **keywords**), and `3.14159`, `5` (these are **literals**).

---

## 2. Identifiers Among Tokens

Recall from Lesson 3: the compiler splits code into **tokens**. Identifiers are one of the five token types.

```cpp
int studentCount = 40 ;
```

| Token | Type |
|-------|------|
| `int` | Keyword |
| `studentCount` | **Identifier** ⭐ |
| `=` | Operator |
| `40` | Literal |
| `;` | Punctuator |

### Keyword vs Identifier

| | Keyword | Identifier |
|--|---------|-----------|
| Who defines it? | The C++ language | **You**, the programmer |
| Meaning | Fixed, built-in | Whatever you decide |
| Can you change it? | ❌ No | ✅ Yes |
| Examples | `int`, `if`, `return`, `class` | `age`, `getTotal`, `Student` |
| Analogy | Words in a dictionary | Names of people |

---

## 3. Rules for Identifiers

These are **strict rules** — breaking them causes a **compile error**.

### ✅ Rule 1: Allowed Characters
An identifier can contain only:
- Letters: `a`–`z`, `A`–`Z`
- Digits: `0`–`9`
- Underscore: `_`

```cpp
int marks_2026;      // ✅
int marks-2026;      // ❌ '-' is the minus operator
int marks@2026;      // ❌ '@' not allowed
```

### ✅ Rule 2: Must Not Start with a Digit
The first character must be a **letter or underscore**.

```cpp
int student1;        // ✅
int 1student;        // ❌ starts with a digit
```

💡 **Why?** If identifiers could start with digits, the compiler couldn't tell whether `123` is a number or a name!

### ✅ Rule 3: No Spaces
```cpp
int total marks;     // ❌ compiler sees two separate tokens
int totalMarks;      // ✅
int total_marks;     // ✅
```

### ✅ Rule 4: Cannot Be a Keyword
```cpp
int class = 10;      // ❌ 'class' is a keyword
int return = 5;      // ❌ 'return' is a keyword
int className = 10;  // ✅ contains a keyword, but isn't one
```

### ✅ Rule 5: Case-Sensitive
`total`, `Total`, and `TOTAL` are **three different** identifiers (see Section 9).

### ✅ Rule 6: Must Be Unique in Its Scope
You can't declare two things with the same name in the **same scope**.

```cpp
int main() {
    int score = 10;
    double score = 9.5;   // ❌ error: redeclaration of 'score'
    return 0;
}
```

### 🗺️ Rules as a Flowchart

```
            Is the first character a letter or '_'?
                 │                    │
                YES                   NO ──▶ ❌ Invalid
                 │
    Are all characters letters, digits, or '_'?
                 │                    │
                YES                   NO ──▶ ❌ Invalid
                 │
           Is it a keyword?
                 │                    │
                 NO                  YES ──▶ ❌ Invalid
                 │
     Already declared in the same scope?
                 │                    │
                 NO                  YES ──▶ ❌ Redeclaration error
                 │
             ✅ Valid identifier
     (still check: is it reserved? is it meaningful?)
```

---

## 4. Valid vs Invalid Identifiers

| Identifier | Valid? | Reason |
|------------|--------|--------|
| `age` | ✅ | Simple and clear |
| `studentName` | ✅ | camelCase |
| `student_name` | ✅ | snake_case |
| `MAX_LIMIT` | ✅ | Upper case with underscore |
| `x1` | ✅ | Digit not at start |
| `_temp` | ✅ | Allowed (but see reserved rules in Section 8) |
| `Int` | ✅ | `Int` ≠ `int` (case-sensitive) — but confusing! |
| `classRoom` | ✅ | Keyword is only part of the name |
| `1stPlace` | ❌ | Starts with a digit |
| `first place` | ❌ | Contains a space |
| `first-place` | ❌ | Contains `-` |
| `price$` | ❌ | `$` is not standard (some compilers accept it as an extension — don't rely on it) |
| `rate%` | ❌ | `%` is an operator |
| `float` | ❌ | Keyword |
| `and` | ❌ | Alternative token — reserved (see Section 6) |
| `#count` | ❌ | `#` not allowed |
| `e-mail` | ❌ | Contains `-` |
| `email.id` | ❌ | `.` not allowed |

---

## 5. Keywords: Names You Can't Use

**Keywords** (also called **reserved words**) have special meaning in C++. They can **never** be used as identifiers.

C++ has **around 90+ keywords**. You don't need to memorize them — your editor colors them differently, and you'll learn them naturally.

### Keywords Grouped by Purpose

| Category | Keywords |
|----------|----------|
| **Data types** | `int`, `char`, `float`, `double`, `bool`, `void`, `wchar_t`, `char8_t`, `char16_t`, `char32_t`, `auto` |
| **Type modifiers** | `short`, `long`, `signed`, `unsigned`, `const`, `volatile`, `mutable` |
| **Boolean / null values** | `true`, `false`, `nullptr` |
| **Decision making** | `if`, `else`, `switch`, `case`, `default` |
| **Loops** | `for`, `while`, `do` |
| **Jumps** | `break`, `continue`, `return`, `goto` |
| **OOP** | `class`, `struct`, `union`, `this`, `public`, `private`, `protected`, `virtual`, `friend`, `explicit`, `operator` |
| **Memory** | `new`, `delete`, `sizeof`, `alignof`, `alignas` |
| **Storage** | `static`, `extern`, `register`, `thread_local` |
| **Namespaces** | `namespace`, `using` |
| **Templates** | `template`, `typename`, `concept`, `requires` |
| **Exceptions** | `try`, `catch`, `throw`, `noexcept` |
| **Casting** | `static_cast`, `dynamic_cast`, `const_cast`, `reinterpret_cast` |
| **Compile-time** | `constexpr`, `consteval`, `constinit`, `static_assert`, `decltype` |
| **Other** | `enum`, `inline`, `typedef`, `typeid`, `asm`, `export` |
| **Coroutines (C++20)** | `co_await`, `co_yield`, `co_return` |

### Example of the Error

```cpp
int main() {
    int new = 5;      // ❌ error: expected unqualified-id before 'new'
    return 0;
}
```

💡 **Tip:** If a variable name turns **blue/purple** (keyword color) in your editor, it's a keyword — pick another name!

### Not Keywords (Common Confusion)

These look "special" but are **identifiers**, not keywords:

| Name | What It Actually Is |
|------|---------------------|
| `main` | A function name (identifier) — but reserved for the program's entry point |
| `cout`, `cin`, `endl` | Identifiers in namespace `std` |
| `string`, `vector` | Class names in namespace `std` |
| `std` | A namespace name |
| `include`, `define` | Preprocessor directive names (used after `#`) |
| `NULL` | A macro (prefer `nullptr`, which **is** a keyword) |

```cpp
int cout = 5;   // technically compiles (if not using namespace std), but ⚠️ very confusing — avoid!
```

---

## 6. Alternative Tokens (Surprise Keywords!)

C++ allows **words** in place of some operators. These words are **reserved** and **can't be identifiers**.

| Alternative Token | Same As |
|-------------------|---------|
| `and` | `&&` |
| `or` | `\|\|` |
| `not` | `!` |
| `xor` | `^` |
| `bitand` | `&` |
| `bitor` | `\|` |
| `compl` | `~` |
| `and_eq` | `&=` |
| `or_eq` | `\|=` |
| `xor_eq` | `^=` |
| `not_eq` | `!=` |

```cpp
int and = 1;      // ❌ error — 'and' is reserved
int or = 0;       // ❌ error

bool a = true, b = false;
if (a and not b) {          // ✅ valid C++! Same as: a && !b
    cout << "Works!";
}
```

🎯 **Classroom trick question:** *"Is `int and = 5;` valid?"* — Most students say yes. The answer is **no**!

---

## 7. Identifiers with Special Meaning

Some words have special meaning **only in certain places**, but are **not** keywords. They can still be used as identifiers elsewhere.

| Identifier | Special Meaning In |
|------------|--------------------|
| `override` | Class member functions (OOP) |
| `final` | Classes and virtual functions (OOP) |
| `import` | C++20 modules |
| `module` | C++20 modules |

```cpp
int final = 100;      // ✅ legal — 'final' is not a keyword
int override = 1;     // ✅ legal
```

⚠️ Legal, but **avoid** — it confuses readers who know these words from OOP.

---

## 8. Reserved Identifiers

Some names are **valid** (they compile) but are **reserved for the compiler and standard library**. Using them can cause strange, hard-to-find bugs.

### 🚫 The Three Rules

| Rule | Examples | Reserved Where? |
|------|----------|-----------------|
| Contains a **double underscore** `__` **anywhere** | `__count`, `my__var`, `total__` | Everywhere |
| Starts with **underscore + uppercase letter** | `_Count`, `_MAX`, `_Value` | Everywhere |
| Starts with an **underscore** | `_count`, `_temp` | In the global namespace |

```cpp
int __total = 0;      // ⚠️ reserved — compiles, but don't!
int _Max = 100;       // ⚠️ reserved
int _value = 5;       // ⚠️ reserved at global scope

int main() {
    int _temp = 1;    // technically OK inside a function, but avoid the habit
    return 0;
}
```

### Why It's Dangerous
The compiler and standard library use names like `__cplusplus`, `__FILE__`, `__LINE__`, `_GLIBCXX_...`. If your name accidentally matches one of theirs, you can get **mysterious errors** or **silent bugs**.

```cpp
#include <iostream>
using namespace std;

int main() {
    cout << "Line: " << __LINE__ << endl;            // current line number
    cout << "File: " << __FILE__ << endl;            // current file name
    cout << "C++ version: " << __cplusplus << endl;  // e.g. 201703 for C++17
    return 0;
}
```

✅ **Simple safe rule for students:** **Never start an identifier with `_`, and never use `__`.**

---

## 9. Case Sensitivity

C++ treats uppercase and lowercase letters as **different characters**.

```cpp
#include <iostream>
using namespace std;

int main() {
    int value = 1;
    int Value = 2;
    int VALUE = 3;

    cout << value << " " << Value << " " << VALUE << endl;   // 1 2 3
    return 0;
}
```

All three are **separate variables**. ✅ Legal — but ❌ terrible practice! Imagine debugging that.

### Common Case-Related Errors

```cpp
int totalMarks = 90;
cout << totalmarks;    // ❌ error: 'totalmarks' was not declared in this scope

Int x = 5;             // ❌ error: 'Int' does not name a type
Main() { }             // ❌ the program has no 'main' → linker error
Cout << "Hi";          // ❌ error: 'Cout' was not declared
```

💡 **Tip:** when you see *"was not declared in this scope"*, check spelling **and** capital letters first.

---

## 10. Identifier Length

- The C++ standard sets **no maximum length** for identifiers.
- Compilers support **very long** names (the standard recommends at least 1024 significant characters).

```cpp
int numberOfStudentsWhoPassedTheFinalExaminationWithDistinction = 12;   // legal 😅
```

### Find the Balance

| Too Short ❌ | Just Right ✅ | Too Long ❌ |
|--------------|--------------|------------|
| `n` | `studentCount` | `numberOfAllStudentsInTheClass` |
| `t` | `totalPrice` | `theTotalPriceOfAllItemsInCart` |
| `cA()` | `calculateArea()` | `calculateTheAreaOfTheRectangle()` |

📌 **Rule of thumb:** a name should be **as long as needed to be clear, and no longer**. Variables with a **wider scope** deserve **longer, more descriptive** names; a loop counter used in 2 lines can be `i`.

---

## 11. Naming Conventions

A **convention** is not a compiler rule — it's an agreed **style** that makes code consistent and readable.

### The Main Styles

| Style | Example | Description |
|-------|---------|-------------|
| **camelCase** | `studentName` | First word lowercase, later words capitalized |
| **PascalCase** | `StudentName` | Every word capitalized |
| **snake_case** | `student_name` | All lowercase, words separated by `_` |
| **UPPER_SNAKE_CASE** | `STUDENT_NAME` | All uppercase, words separated by `_` |

🐫 **camelCase** looks like a camel's humps. 🐍 **snake_case** looks like a snake lying flat.

### A Common Convention for C++

| Element | Style | Example |
|---------|-------|---------|
| Variables | camelCase (or snake_case) | `totalMarks`, `total_marks` |
| Functions | camelCase (or snake_case) | `calculateTotal()`, `calculate_total()` |
| Classes / Structs | PascalCase | `Student`, `BankAccount` |
| Constants | UPPER_SNAKE_CASE (or `k` prefix) | `MAX_SIZE`, `kMaxSize` |
| Macros (`#define`) | UPPER_SNAKE_CASE | `#define BUFFER_SIZE 256` |
| Namespaces | lowercase or PascalCase | `billing`, `MathUtils` |
| Enum values | UPPER_SNAKE_CASE or PascalCase | `RED`, `Red` |

📌 **The standard library itself uses snake_case** everywhere: `std::string`, `std::to_string`, `push_back`, `size_t`.

### Example in One Program

```cpp
#include <iostream>
#include <string>
using namespace std;

const int MAX_STUDENTS = 60;                  // constant → UPPER_SNAKE_CASE

namespace school {                            // namespace → lowercase
    struct Student {                          // struct → PascalCase
        string fullName;                      // member → camelCase
        int rollNumber;
    };

    void printStudent(const Student& s) {     // function → camelCase
        cout << s.rollNumber << ": " << s.fullName << endl;
    }
}

int main() {
    school::Student topStudent{"Ananya Rao", 1};   // variable → camelCase
    school::printStudent(topStudent);
    cout << "Class capacity: " << MAX_STUDENTS << endl;
    return 0;
}
```

### 🚫 Hungarian Notation (Outdated)
An old style puts the type in the name: `iCount`, `strName`, `bIsValid`, `fPrice`.
- ❌ Not recommended in modern C++ — editors already show types, and if the type changes, the name becomes misleading.

### ✅ The Golden Rule
> **Consistency beats preference.** Follow your team's (or teacher's) style guide. If there isn't one, pick a style and use it **everywhere** in the project.

---

## 12. Choosing Good Names

Good names make code **read like English**. Bad names force readers to guess.

### Compare

```cpp
// ❌ What does this do?
double f(double a, double b) {
    double c = a * b / 100;
    return a + c;
}
```

```cpp
// ✅ Instantly clear
double addTax(double price, double taxPercent) {
    double taxAmount = price * taxPercent / 100;
    return price + taxAmount;
}
```

Same logic. Same speed. **Completely different readability.**

### 📋 Naming Guidelines

| Guideline | ❌ Bad | ✅ Good |
|-----------|--------|---------|
| Be descriptive | `d` | `daysUntilExam` |
| Variables = **nouns** | `calculate` | `total`, `averageMarks` |
| Functions = **verbs** | `total()` | `calculateTotal()`, `printReport()` |
| Classes = **nouns** (PascalCase) | `ManageStudent` | `Student`, `StudentManager` |
| Booleans = **questions** | `flag`, `status` | `isValid`, `hasPassed`, `canVote` |
| Avoid unclear abbreviations | `stdNm`, `cnt`, `tmp2` | `studentName`, `count` |
| Avoid misleading names | `studentList` (when it's a single int) | `studentCount` |
| Avoid confusing characters | `l`, `O`, `I` (look like `1` and `0`) | `length`, `offset`, `index` |
| Avoid near-duplicate names | `total`, `totals`, `Total` | `subtotal`, `grandTotal` |
| Avoid numbering | `marks1`, `marks2`, `marks3` | `mathMarks`, `scienceMarks` (or an array) |
| Include units if useful | `timeout` | `timeoutSeconds`, `weightKg` |

### When Short Names Are OK ✅

| Name | Accepted Use |
|------|--------------|
| `i`, `j`, `k` | Loop counters |
| `x`, `y`, `z` | Coordinates |
| `n` | Count in short math functions |
| `a`, `b` | Parameters of very small, generic functions like `max(a, b)` |

### Booleans Should Read Naturally

```cpp
bool isAdult = age >= 18;
bool hasPassed = marks >= 40;

if (isAdult && hasPassed) {        // reads like a sentence ✅
    cout << "Eligible";
}
```

🔤 **Test:** read your code aloud. If it sounds like a sentence, your names are good!

---

## 13. Same Identifier, Different Scopes

An identifier must be **unique within its scope**, but the **same name** can be reused in **different scopes**.

```cpp
#include <iostream>
using namespace std;

int count = 100;                 // global

void show() {
    int count = 5;               // local to show() — a DIFFERENT variable
    cout << "show(): " << count << endl;
}

int main() {
    int count = 10;              // local to main()
    cout << "main(): " << count << endl;

    {
        int count = 20;          // inner block — hides main's count
        cout << "block : " << count << endl;
    }

    show();
    cout << "global: " << ::count << endl;
    return 0;
}
```

**Output:**
```
main(): 10
block : 20
show(): 5
global: 100
```

### Shadowing ⚠️
When an inner name **hides** an outer name, it's called **shadowing**. It's legal but a **common source of bugs**:

```cpp
int total = 0;
for (int i = 1; i <= 3; i++) {
    int total = i;   // ⚠️ shadows the outer total — outer one never changes!
}
cout << total;       // prints 0, not what the student expected
```

💡 Compile with warnings to catch this:
```bash
g++ -Wall -Wshadow program.cpp -o program
```

Identifiers also live inside **namespaces** and **classes**, which is how `Graphics::draw` and `CardGame::draw` coexist (see Lesson 4).

---

## 14. Common Beginner Mistakes

| ❌ Mistake | ✅ Correct | Why |
|-----------|-----------|-----|
| `int 2ndRank;` | `int secondRank;` | Can't start with a digit |
| `int roll no;` | `int rollNo;` | No spaces |
| `int e-mail;` | `int email;` | `-` is an operator |
| `double new = 5;` | `double newValue = 5;` | `new` is a keyword |
| `bool not = false;` | `bool isNot = false;` | `not` is an alternative token |
| `int _Count;` / `int __x;` | `int count;` | Reserved identifiers |
| Declaring `Total`, using `total` | Use the exact same case | Case-sensitive |
| `int a, b, c, d;` for marks | `int mathMarks, scienceMarks, ...;` | Meaningless names |
| `int flag;` | `bool isLoggedIn;` | Name doesn't say what it means |
| Same name declared twice in one scope | Use different names | Redeclaration error |
| Inner variable with same name as outer | Use different names | Accidental shadowing |
| `int cout = 0;` | `int outputCount = 0;` | Confusing — clashes with `std::cout` |

---

## 15. Practice Exercises

### 🟢 Easy

**Exercise 1:** Mark each identifier as **Valid** or **Invalid**, and give the reason for invalid ones:
```
1. rollNumber     2. 9lives        3. _value       4. total marks
5. float          6. Float         7. pass%        8. student_1
9. and            10. MAX_SPEED    11. final       12. e.mail
```

**Exercise 2:** List all identifiers in this program (not keywords, literals, or operators):
```cpp
#include <iostream>
using namespace std;

int main() {
    int length = 10;
    int width = 4;
    int area = length * width;
    cout << area << endl;
    return 0;
}
```

**Exercise 3:** Convert each name into **camelCase**, **PascalCase**, **snake_case**, and **UPPER_SNAKE_CASE**:
- "student roll number"
- "maximum speed limit"
- "calculate final grade"

### 🟡 Medium

**Exercise 4:** Rewrite this program with **meaningful identifiers**:
```cpp
#include <iostream>
using namespace std;

int main() {
    double a, b, c;
    cin >> a >> b;
    c = a * b;
    cout << c << endl;
    return 0;
}
```
*(Context: the program reads hours worked and hourly wage, then prints the salary.)*

**Exercise 5:** Find and fix **all** identifier errors:
```cpp
#include <iostream>
using namespace std;

int main() {
    int 1stNumber = 10;
    int second-number = 20;
    int return = 1stNumber + second-number;
    int Sum = 0;
    cout << sum << endl;
    return 0;
}
```

**Exercise 6:** Predict the output:
```cpp
#include <iostream>
using namespace std;

int x = 1;

int main() {
    int x = 2;
    {
        int x = 3;
        cout << x << " ";
    }
    cout << x << " " << ::x << endl;
    return 0;
}
```

**Exercise 7:** Suggest better **boolean** names for these:
`flag1`, `check`, `status`, `ok`, `result` — where they mean: account is locked, user is over 18, file exists, payment completed, student passed.

### 🔴 Challenge

**Exercise 8:** Which of these are **legal but reserved or discouraged**, and why?
```
__counter    _Total    _helper (at global scope)    final    Int    cout (as a variable)
```

**Exercise 9:** Write a small program that demonstrates **case sensitivity** by declaring `score`, `Score`, and `SCORE` with different values and printing them. Then write one sentence explaining why this is bad practice.

**Exercise 10:** Write a program that follows **all naming conventions** in this lesson and contains:
- a global constant for the passing marks
- a namespace for the school
- a struct for a student
- a function that checks if a student passed
- a boolean variable that stores the result

---

### ✅ Solutions

<details>
<summary>Click to view Exercise 1 answers</summary>

| # | Identifier | Valid? | Reason |
|---|------------|--------|--------|
| 1 | `rollNumber` | ✅ | |
| 2 | `9lives` | ❌ | Starts with a digit |
| 3 | `_value` | ✅ | Legal, but reserved at global scope — avoid |
| 4 | `total marks` | ❌ | Contains a space |
| 5 | `float` | ❌ | Keyword |
| 6 | `Float` | ✅ | Different case from `float` — but confusing |
| 7 | `pass%` | ❌ | `%` not allowed |
| 8 | `student_1` | ✅ | |
| 9 | `and` | ❌ | Alternative token (reserved) |
| 10 | `MAX_SPEED` | ✅ | |
| 11 | `final` | ✅ | Not a keyword (special meaning only in classes) — avoid |
| 12 | `e.mail` | ❌ | `.` not allowed |
</details>

<details>
<summary>Click to view Exercise 2 answer</summary>

Identifiers: **`std`**, **`main`**, **`length`**, **`width`**, **`area`**, **`cout`**, **`endl`**

- `iostream` is a header name used by the preprocessor
- `using`, `namespace`, `int`, `return` are keywords
- `10`, `4`, `0` are literals
</details>

<details>
<summary>Click to view Exercise 3 answers</summary>

| Phrase | camelCase | PascalCase | snake_case | UPPER_SNAKE_CASE |
|--------|-----------|------------|------------|------------------|
| student roll number | `studentRollNumber` | `StudentRollNumber` | `student_roll_number` | `STUDENT_ROLL_NUMBER` |
| maximum speed limit | `maximumSpeedLimit` | `MaximumSpeedLimit` | `maximum_speed_limit` | `MAXIMUM_SPEED_LIMIT` |
| calculate final grade | `calculateFinalGrade` | `CalculateFinalGrade` | `calculate_final_grade` | `CALCULATE_FINAL_GRADE` |
</details>

<details>
<summary>Click to view Exercise 4 solution</summary>

```cpp
#include <iostream>
using namespace std;

int main() {
    double hoursWorked, hourlyWage, salary;

    cout << "Enter hours worked: ";
    cin >> hoursWorked;
    cout << "Enter hourly wage: ";
    cin >> hourlyWage;

    salary = hoursWorked * hourlyWage;
    cout << "Salary: " << salary << endl;
    return 0;
}
```
</details>

<details>
<summary>Click to view Exercise 5 solution</summary>

**Errors:**
1. `1stNumber` → starts with a digit
2. `second-number` → contains `-`
3. `return` → keyword used as a variable name
4. `Sum` declared but `sum` used → case mismatch

```cpp
#include <iostream>
using namespace std;

int main() {
    int firstNumber = 10;
    int secondNumber = 20;
    int sum = firstNumber + secondNumber;
    cout << sum << endl;
    return 0;
}
```
**Output:** `30`
</details>

<details>
<summary>Click to view Exercise 6 answer</summary>

**Output:**
```
3 2 1
```
- Inside the inner block, `x` is **3**
- After the block, `main`'s `x` is **2**
- `::x` is the global `x` = **1**
</details>

<details>
<summary>Click to view Exercise 7 answers</summary>

| Meaning | Better Name |
|---------|-------------|
| Account is locked | `isAccountLocked` |
| User is over 18 | `isAdult` |
| File exists | `fileExists` / `doesFileExist` |
| Payment completed | `isPaymentComplete` / `hasPaid` |
| Student passed | `hasPassed` |
</details>

<details>
<summary>Click to view Exercise 8 answers</summary>

| Identifier | Status | Why |
|------------|--------|-----|
| `__counter` | ⚠️ Reserved | Contains a double underscore |
| `_Total` | ⚠️ Reserved | Underscore + uppercase letter |
| `_helper` (global) | ⚠️ Reserved | Leading underscore at global scope |
| `final` | ⚠️ Discouraged | Has special meaning in classes |
| `Int` | ⚠️ Discouraged | Easily confused with keyword `int` |
| `cout` (variable) | ⚠️ Discouraged | Clashes/confuses with `std::cout` |

All of them **compile**, but none should be used.
</details>

<details>
<summary>Click to view Exercise 9 solution</summary>

```cpp
#include <iostream>
using namespace std;

int main() {
    int score = 50;
    int Score = 75;
    int SCORE = 100;

    cout << "score = " << score << endl;
    cout << "Score = " << Score << endl;
    cout << "SCORE = " << SCORE << endl;
    return 0;
}
```

**Why it's bad:** names that differ only in capitalization are easy to mix up, so a small typo silently uses the wrong variable and causes bugs that are hard to spot.
</details>

<details>
<summary>Click to view Exercise 10 solution</summary>

```cpp
#include <iostream>
#include <string>
using namespace std;

const int PASSING_MARKS = 40;              // constant → UPPER_SNAKE_CASE

namespace school {                         // namespace → lowercase
    struct Student {                       // struct → PascalCase
        string fullName;                   // members → camelCase
        int totalMarks;
    };

    bool checkPassed(const Student& student) {   // function → verb, camelCase
        return student.totalMarks >= PASSING_MARKS;
    }
}

int main() {
    school::Student currentStudent{"Rohan Mehta", 72};
    bool hasPassed = school::checkPassed(currentStudent);   // boolean → question form

    cout << currentStudent.fullName << " passed? "
         << boolalpha << hasPassed << endl;
    return 0;
}
```

**Output:**
```
Rohan Mehta passed? true
```
> 💡 `struct` and `const Student&` are covered in later lessons — focus on the **names** here.
</details>

---

## 16. Quick Quiz

**Q1.** What is an identifier?
- a) A built-in word with fixed meaning
- b) A name given by the programmer to a program element
- c) A fixed value like `10`
- d) A symbol like `+`

**Q2.** Which identifier is **valid**?
- a) `2ndValue`
- b) `second value`
- c) `second_value`
- d) `second-value`

**Q3.** Which of these is a **keyword**?
- a) `main`
- b) `cout`
- c) `string`
- d) `nullptr`

**Q4.** Is `int and = 5;` valid?
- a) Yes
- b) No — `and` is a reserved alternative token
- c) Only in C++20
- d) Only with `using namespace std;`

**Q5.** Which identifier is **reserved** and should be avoided?
- a) `totalCount`
- b) `_Total`
- c) `total_count`
- d) `TOTAL`

**Q6.** `marks`, `Marks`, and `MARKS` are:
- a) The same variable
- b) Three different variables
- c) A compile error
- d) Keywords

**Q7.** Which naming style is `BankAccount`?
- a) camelCase
- b) snake_case
- c) PascalCase
- d) UPPER_SNAKE_CASE

**Q8.** Which is the **best** name for a boolean that tells whether a user is logged in?
- a) `flag`
- b) `login`
- c) `isLoggedIn`
- d) `l`

**Q9.** What is the maximum identifier length defined by the C++ standard?
- a) 31 characters
- b) 63 characters
- c) 255 characters
- d) No fixed maximum

**Q10.** An inner variable hiding an outer variable with the same name is called:
- a) Overloading
- b) Shadowing
- c) Overriding
- d) Aliasing

<details>
<summary>✅ Click to view answers</summary>

| Q | Answer | Explanation |
|---|--------|-------------|
| 1 | b) | Programmer-chosen names |
| 2 | c) `second_value` | Others start with a digit or contain a space / `-` |
| 3 | d) `nullptr` | `main`, `cout`, `string` are identifiers |
| 4 | b) | `and` means `&&` |
| 5 | b) `_Total` | Underscore + uppercase is reserved |
| 6 | b) | C++ is case-sensitive |
| 7 | c) PascalCase | Every word capitalized |
| 8 | c) `isLoggedIn` | Reads like a yes/no question |
| 9 | d) | Compilers support very long names |
| 10 | b) Shadowing | |

</details>

---

## 17. Summary

- ✅ An **identifier** is a programmer-chosen **name** for variables, functions, classes, namespaces, constants, and more.
- ✅ **Rules:** only letters, digits, `_` · can't start with a digit · no spaces · no keywords · unique within its scope.
- ✅ **Keywords** (`int`, `class`, `return`, `nullptr`, ...) can never be identifiers.
- ✅ **Alternative tokens** (`and`, `or`, `not`, `xor`, ...) are reserved too.
- ✅ `override`, `final`, `import`, `module` have special meaning only in certain contexts — legal names, but avoid.
- ✅ `main`, `cout`, `string`, `std` are **identifiers**, not keywords.
- ✅ **Reserved identifiers:** anything with `__`, or starting with `_` + uppercase, or starting with `_` at global scope. **Simple rule: never start with `_`.**
- ✅ C++ is **case-sensitive**: `total` ≠ `Total` ≠ `TOTAL`.
- ✅ No maximum length in the standard — make names as long as needed to be clear.
- ✅ **Conventions:** camelCase, PascalCase, snake_case, UPPER_SNAKE_CASE — be **consistent**.
- ✅ **Good names:** variables = nouns, functions = verbs, booleans = questions (`isValid`, `hasPassed`).
- ✅ The same identifier can exist in **different scopes**; watch out for **shadowing**.

---



---

> 📝 *Tip for students: Code is read far more often than it is written. Spending 5 extra seconds choosing a good name can save 5 hours of debugging later!*
