
#  Enumerations (`enum`) in C++

> **Goal of this lesson:** Learn how to create your own set of named constants using `enum` and `enum class`, understand how they work internally, avoid their common traps, and use them to write clear, safe, readable programs.

> 🔗 *Lesson 6 introduced enums briefly as a kind of constant. This lesson covers them in full depth.*

---

## 📑 Table of Contents

1. [What is an Enumeration?](#1-what-is-an-enumeration)
2. [Why Do We Need Enums?](#2-why-do-we-need-enums)
3. [Plain (Unscoped) `enum`](#3-plain-unscoped-enum)
4. [How Enums Work Internally](#4-how-enums-work-internally)
5. [Assigning Custom Values](#5-assigning-custom-values)
6. [Enum ↔ Integer Conversion](#6-enum--integer-conversion)
7. [Problems with Plain Enums](#7-problems-with-plain-enums)
8. [Scoped Enums: `enum class`](#8-scoped-enums-enum-class)
9. [`enum` vs `enum class`](#9-enum-vs-enum-class)
10. [Specifying the Underlying Type](#10-specifying-the-underlying-type)
11. [Enums with `switch`](#11-enums-with-switch)
12. [`using enum` (C++20)](#12-using-enum-c20)
13. [Converting Enums to Text](#13-converting-enums-to-text)
14. [Enums with Functions](#14-enums-with-functions)
15. [Looping Through Enum Values](#15-looping-through-enum-values)
16. [Bit Flags with Enums (Advanced)](#16-bit-flags-with-enums-advanced)
17. [Best Practices](#17-best-practices)
18. [Common Beginner Mistakes](#18-common-beginner-mistakes)
19. [Practice Exercises](#19-practice-exercises)
20. [Quick Quiz](#20-quick-quiz)
21. [Summary](#21-summary)

---

## 1. What is an Enumeration?

An **enumeration** (`enum`) is a **user-defined data type** consisting of a **fixed set of named integer constants**.

> **"Enumerate"** means **"to list items one by one."** An enum is literally a **list of named values**.

🚦 **Analogy:** A traffic light has only **three** possible states — Red, Yellow, Green. It can never be "Purple" or "47". An enum lets you create a type that allows **only those exact values**.

### Real-World Examples of Enums

| Enum | Possible Values |
|------|-----------------|
| Day of week | Monday, Tuesday, ..., Sunday |
| Traffic light | Red, Yellow, Green |
| Direction | North, South, East, West |
| Grade | A, B, C, D, F |
| Order status | Placed, Shipped, Delivered, Cancelled |
| Game difficulty | Easy, Medium, Hard |
| Card suit | Hearts, Diamonds, Clubs, Spades |
| Payment method | Cash, Card, UPI, NetBanking |

📌 Use an enum whenever a variable can take **one value from a small, fixed set of options**.

---

## 2. Why Do We Need Enums?

Imagine storing the **status of an online order**. Without enums, you have two options — both bad.

### ❌ Option 1: Using Integers

```cpp
int status = 2;

if (status == 2) {          // what does 2 mean? 🤔
    cout << "...";
}
status = 57;                // compiles, but 57 is not a valid status! 😱
```

**Problems:** magic numbers, unreadable, no protection against invalid values.

### ❌ Option 2: Using Strings

```cpp
string status = "Shipped";

if (status == "shipped") {  // ❌ fails — case mismatch!
    cout << "...";
}
status = "Shiped";          // typo compiles fine 😱
```

**Problems:** typos aren't caught, slower comparisons, more memory.

### ✅ Option 3: Using an Enum

```cpp
enum class OrderStatus { Placed, Shipped, Delivered, Cancelled };

OrderStatus status = OrderStatus::Shipped;

if (status == OrderStatus::Shipped) {   // ✅ clear and readable
    cout << "Your order is on the way!";
}
// status = 57;                          // ❌ compile error — protected!
// status = OrderStatus::Shiped;         // ❌ compile error — typo caught!
```

### Comparison

| Feature | `int` | `string` | `enum` |
|---------|-------|----------|--------|
| Readable | ❌ | ✅ | ✅ |
| Catches typos | ❌ | ❌ | ✅ |
| Blocks invalid values | ❌ | ❌ | ✅ |
| Fast & small | ✅ | ❌ | ✅ |
| Editor auto-complete | ❌ | ❌ | ✅ |

🏆 **Enums give you the readability of strings with the speed of integers — plus safety.**

---

## 3. Plain (Unscoped) `enum`

### Syntax

```cpp
enum EnumName {
    VALUE1,
    VALUE2,
    VALUE3
};   // ⚠️ semicolon required!
```

- `EnumName` → the **new type** you are creating
- `VALUE1, VALUE2, ...` → the **enumerators** (the allowed values)
- **Semicolon after `}` is required** (unlike namespaces)

### Example

```cpp
#include <iostream>
using namespace std;

enum Direction {
    NORTH,
    SOUTH,
    EAST,
    WEST
};

int main() {
    Direction heading = EAST;      // declare a variable of type Direction

    cout << "Heading value: " << heading << endl;   // 2

    heading = NORTH;               // change to another valid value
    cout << "New heading: " << heading << endl;     // 0
    return 0;
}
```

**Output:**
```
Heading value: 2
New heading: 0
```

### Declaring Variables

```cpp
enum Season { SPRING, SUMMER, AUTUMN, WINTER };

Season current = SUMMER;          // ✅ most common
Season next{AUTUMN};              // ✅ brace initialization
Season last;                      // ⚠️ uninitialized — holds garbage
```

### Declaring the Enum and a Variable Together

```cpp
enum Size { SMALL, MEDIUM, LARGE } coffeeSize = MEDIUM;   // works, but less common
```

---

## 4. How Enums Work Internally

Each enumerator is stored as an **integer**. By default:
- The **first** enumerator gets **0**
- Each next one gets **previous + 1**

```cpp
enum Day { MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY };
```

```
 Enumerator:  MONDAY  TUESDAY  WEDNESDAY  THURSDAY  FRIDAY  SATURDAY  SUNDAY
 Value:          0       1         2          3        4        5        6
```

```cpp
#include <iostream>
using namespace std;

enum Day { MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY };

int main() {
    cout << "MONDAY    = " << MONDAY << endl;
    cout << "WEDNESDAY = " << WEDNESDAY << endl;
    cout << "SUNDAY    = " << SUNDAY << endl;
    cout << "Size of Day: " << sizeof(Day) << " bytes" << endl;
    return 0;
}
```

**Output:**
```
MONDAY    = 0
WEDNESDAY = 2
SUNDAY    = 6
Size of Day: 4 bytes
```

📌 Printing a plain enum shows its **number**, not its **name**. To print names, see [Section 13](#13-converting-enums-to-text).

💡 At runtime, an enum is **just a number** — so it's as fast and small as an `int`. The names exist only to help **you** and the **compiler**.

---

## 5. Assigning Custom Values

You can give enumerators **specific values**.

### Explicit Values

```cpp
enum HttpStatus {
    OK = 200,
    CREATED = 201,
    BAD_REQUEST = 400,
    NOT_FOUND = 404,
    SERVER_ERROR = 500
};
```

### Partial Values — The Count Continues

If you set one value, the following ones **continue counting from it**:

```cpp
enum Month {
    JAN = 1, FEB, MAR, APR, MAY, JUN,
    JUL, AUG, SEP, OCT, NOV, DEC
};
// FEB = 2, MAR = 3, ..., DEC = 12  ✅ months now match real numbering
```

```cpp
enum Example {
    A,          // 0
    B = 10,     // 10
    C,          // 11
    D = 5,      // 5
    E           // 6
};
```

### Duplicate and Negative Values Are Allowed

```cpp
enum Level {
    LOW = -1,
    NORMAL = 0,
    DEFAULT = 0,    // ✅ duplicate value allowed (an alias)
    HIGH = 1
};
```

⚠️ Duplicate **names** are not allowed — only duplicate **values**:
```cpp
enum Bad { RED, GREEN, RED };   // ❌ error: redeclaration of 'RED'
```

### Character Values

Enumerators are integers, so character literals work too (stored as ASCII):

```cpp
enum Grade {
    GRADE_A = 'A',   // 65
    GRADE_B = 'B',   // 66
    GRADE_C = 'C'    // 67
};

cout << static_cast<char>(GRADE_B);   // B
```

---

## 6. Enum ↔ Integer Conversion

### Enum → Integer: ✅ Automatic (Plain Enums)

```cpp
enum Color { RED, GREEN, BLUE };

Color c = GREEN;
int n = c;            // ✅ n = 1 (implicit conversion)
int total = c + 10;   // ✅ 11
```

### Integer → Enum: ❌ Not Automatic

```cpp
Color c1 = 2;                       // ❌ error: invalid conversion from 'int' to 'Color'
Color c2 = static_cast<Color>(2);   // ✅ c2 = BLUE
```

### ⚠️ Danger: Casting an Invalid Number

```cpp
Color c = static_cast<Color>(99);   // compiles! but 99 isn't RED, GREEN, or BLUE
```

The compiler trusts your cast. **Always validate** numbers (like user input) before casting:

```cpp
int choice;
cin >> choice;

if (choice >= RED && choice <= BLUE) {
    Color selected = static_cast<Color>(choice);
    // safe to use
} else {
    cout << "Invalid color!" << endl;
}
```

> 💡 `if` statements are covered in a later lesson — here they just check that the number is in range.

---

## 7. Problems with Plain Enums

Plain enums have **three serious problems**.

### ❌ Problem 1: Names Leak into the Surrounding Scope

Enumerator names become **global** (or enclosing-scope) names:

```cpp
enum Color { RED, GREEN, BLUE };
enum TrafficLight { RED, YELLOW, GREEN };   // ❌ error: 'RED' and 'GREEN' already declared

int RED = 5;                                 // ❌ error: also conflicts
```

Programmers often work around this with **prefixes**:
```cpp
enum Color { COLOR_RED, COLOR_GREEN, COLOR_BLUE };
enum TrafficLight { LIGHT_RED, LIGHT_YELLOW, LIGHT_GREEN };
```
It works — but it's clumsy.

### ❌ Problem 2: Comparing Unrelated Enums

```cpp
enum Fruit { APPLE, BANANA };
enum Animal { DOG, CAT };

Fruit f = APPLE;
Animal a = DOG;

if (f == a) {                   // ⚠️ compiles (at most a warning)! Both are 0
    cout << "An apple is a dog?!" << endl;   // 😱 prints!
}
```

Comparing a fruit with an animal makes **no sense**, but plain enums allow it.

### ❌ Problem 3: Silent Conversion to Integers

```cpp
enum Day { MONDAY, TUESDAY };
int x = TUESDAY * 100;          // ✅ compiles — but does multiplying a day make sense?
```

**The solution to all three problems → `enum class`** 👇

---

## 8. Scoped Enums: `enum class`

Introduced in **C++11**, `enum class` (also called a **scoped enum** or **strongly-typed enum**) fixes all the problems of plain enums.

### Syntax

```cpp
enum class EnumName {
    Value1,
    Value2,
    Value3
};
```

(`enum struct` is identical to `enum class` — but `enum class` is the common form.)

### Example

```cpp
#include <iostream>
using namespace std;

enum class Color { Red, Green, Blue };
enum class TrafficLight { Red, Yellow, Green };    // ✅ no conflict!

int main() {
    Color shirt = Color::Red;                       // must use Color::
    TrafficLight signal = TrafficLight::Green;

    if (signal == TrafficLight::Green) {
        cout << "Go!" << endl;
    }

    // cout << shirt;                               // ❌ error: no implicit conversion
    cout << static_cast<int>(shirt) << endl;        // ✅ 0

    // if (shirt == signal) { }                     // ❌ error: different types
    // int n = Color::Blue;                         // ❌ error: no implicit conversion
    return 0;
}
```

**Output:**
```
Go!
0
```

### ✅ How `enum class` Fixes Each Problem

| Problem | Plain `enum` | `enum class` |
|---------|--------------|--------------|
| Name conflicts | `RED` leaks into scope ❌ | Must write `Color::Red` ✅ |
| Comparing unrelated enums | Allowed ❌ | Compile error ✅ |
| Silent int conversion | Allowed ❌ | Requires `static_cast` ✅ |

### Converting `enum class` ↔ Integer

```cpp
enum class Level { Low = 1, Medium = 2, High = 3 };

Level lv = Level::Medium;

int n = static_cast<int>(lv);              // enum → int: 2
Level fromInt = static_cast<Level>(3);     // int → enum: Level::High
```

💡 **C++23** adds `std::to_underlying(lv)` (from `<utility>`) as a cleaner way to get the number.

---

## 9. `enum` vs `enum class`

| Feature | `enum` (plain / unscoped) | `enum class` (scoped) |
|---------|---------------------------|----------------------|
| Introduced | C (old) | C++11 |
| Access syntax | `RED` | `Color::Red` |
| Names leak into scope | ✅ Yes (bad) | ❌ No (good) |
| Implicit conversion to `int` | ✅ Yes | ❌ No — needs `static_cast` |
| Compare different enum types | Allowed (bad) | Compile error (good) |
| Default underlying type | Chosen by compiler | `int` |
| Can be printed with `cout` directly | ✅ (prints number) | ❌ (cast first) |
| Naming style | Often `UPPER_CASE` | Often `PascalCase` |
| **Recommendation** | Legacy code, bit flags | ✅ **Default choice in modern C++** |

📌 **Rule:** Use `enum class` unless you have a specific reason not to.

---

## 10. Specifying the Underlying Type

The **underlying type** is the integer type used to store the enum. You can choose it with `:`.

```cpp
enum class Color : char { Red, Green, Blue };              // 1 byte
enum class Status : short { Active, Inactive };            // 2 bytes
enum class BigId : long long { Max = 9000000000LL };       // 8 bytes
enum Weekday : unsigned char { MON, TUE, WED };            // works for plain enums too
```

### Why Specify It?

1. **Save memory** — storing millions of values (e.g., pixels, game tiles)
2. **Control size** for files, networks, or hardware
3. **Need large values** that don't fit in `int`

```cpp
#include <iostream>
using namespace std;

enum class Small : char { A, B, C };
enum class Normal { A, B, C };              // int by default

int main() {
    cout << "Small : " << sizeof(Small) << " byte" << endl;    // 1
    cout << "Normal: " << sizeof(Normal) << " bytes" << endl;  // 4
    return 0;
}
```

---

## 11. Enums with `switch`

Enums and `switch` statements are **best friends** — a `switch` handles each enum value cleanly.

> 💡 The `switch` statement is covered fully in the Decision Making lesson. Here's a preview.

```cpp
#include <iostream>
using namespace std;

enum class TrafficLight { Red, Yellow, Green };

void showAction(TrafficLight light) {
    switch (light) {
        case TrafficLight::Red:
            cout << "STOP 🛑" << endl;
            break;
        case TrafficLight::Yellow:
            cout << "GET READY ⚠️" << endl;
            break;
        case TrafficLight::Green:
            cout << "GO ✅" << endl;
            break;
    }
}

int main() {
    showAction(TrafficLight::Red);
    showAction(TrafficLight::Green);
    return 0;
}
```

**Output:**
```
STOP 🛑
GO ✅
```

### 🛡️ Compiler Safety Bonus

Compile with warnings enabled:
```bash
g++ -Wall program.cpp -o program
```

If you **forget a case**, the compiler warns you:
```
warning: enumeration value 'Yellow' not handled in switch [-Wswitch]
```

This is a huge advantage over using plain `int`s — when you add a new value to the enum later, the compiler shows you **every switch you need to update**. 🎯

---

## 12. `using enum` (C++20)

Writing `TrafficLight::` repeatedly can be tiring. **C++20** lets you bring enum names into a scope with `using enum`:

```cpp
#include <iostream>
using namespace std;

enum class TrafficLight { Red, Yellow, Green };

void showAction(TrafficLight light) {
    switch (light) {
        using enum TrafficLight;          // names available inside this switch
        case Red:    cout << "STOP" << endl; break;
        case Yellow: cout << "READY" << endl; break;
        case Green:  cout << "GO" << endl; break;
    }
}

int main() {
    showAction(TrafficLight::Yellow);
    return 0;
}
```

Compile with: `g++ -std=c++20 program.cpp -o program`

💡 Like `using namespace`, keep `using enum` in a **small scope** (Lesson 4).

---

## 13. Converting Enums to Text

`cout` prints enums as **numbers**. To display **names**, write a conversion function.

### Method 1: `switch` Function ✅ (Most Common)

```cpp
#include <iostream>
#include <string>
using namespace std;

enum class Day { Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday };

string toString(Day d) {
    switch (d) {
        case Day::Monday:    return "Monday";
        case Day::Tuesday:   return "Tuesday";
        case Day::Wednesday: return "Wednesday";
        case Day::Thursday:  return "Thursday";
        case Day::Friday:    return "Friday";
        case Day::Saturday:  return "Saturday";
        case Day::Sunday:    return "Sunday";
    }
    return "Unknown";
}

int main() {
    Day today = Day::Friday;
    cout << "Today is " << toString(today) << endl;
    return 0;
}
```

**Output:**
```
Today is Friday
```

### Method 2: Array Lookup (when values are 0, 1, 2, ...)

```cpp
#include <iostream>
#include <string>
using namespace std;

enum class Suit { Hearts, Diamonds, Clubs, Spades };

const string SUIT_NAMES[] = { "Hearts", "Diamonds", "Clubs", "Spades" };

int main() {
    Suit card = Suit::Clubs;
    cout << SUIT_NAMES[static_cast<int>(card)] << endl;   // Clubs
    return 0;
}
```

⚠️ Method 2 only works when values are **consecutive from 0**, and the array must stay in the **same order** as the enum. Method 1 is safer.

> 📚 Arrays are covered in a later lesson.

---

## 14. Enums with Functions

Enums work naturally as **function parameters** and **return types**, making function signatures self-explanatory.

```cpp
#include <iostream>
#include <string>
using namespace std;

enum class Grade { A, B, C, D, F };

// Enum as a RETURN type
Grade calculateGrade(int marks) {
    if (marks >= 90) return Grade::A;
    if (marks >= 75) return Grade::B;
    if (marks >= 60) return Grade::C;
    if (marks >= 40) return Grade::D;
    return Grade::F;
}

// Enum as a PARAMETER
string remark(Grade g) {
    switch (g) {
        case Grade::A: return "Outstanding!";
        case Grade::B: return "Very Good";
        case Grade::C: return "Good";
        case Grade::D: return "Needs Improvement";
        case Grade::F: return "Failed";
    }
    return "";
}

int main() {
    int marks;
    cout << "Enter marks: ";
    cin >> marks;

    Grade g = calculateGrade(marks);
    cout << "Remark: " << remark(g) << endl;
    return 0;
}
```

**Sample run:**
```
Enter marks: 82
Remark: Very Good
```

### Why Enum Parameters Are Better

```cpp
void setVolume(int level);           // ❌ what values are allowed? 0-10? 0-100?
void setVolume(VolumeLevel level);   // ✅ only valid levels can be passed
```

```cpp
createAccount("Riya", true, false);                              // ❌ what do true/false mean?
createAccount("Riya", AccountType::Savings, Notify::NoEmail);    // ✅ self-explanatory
```

### Enums Inside Structs and Classes (Preview)

```cpp
struct Order {
    int id;
    enum class Status { Placed, Shipped, Delivered } status;
};

Order o{101, Order::Status::Shipped};
```

> 📚 Structs and classes are covered in later lessons.

---

## 15. Looping Through Enum Values

C++ has no built-in way to loop over an enum, but a common trick is to add a **`Count`** value at the end:

```cpp
#include <iostream>
#include <string>
using namespace std;

enum class Planet { Mercury, Venus, Earth, Mars, Count };   // Count = 4

const string PLANET_NAMES[] = { "Mercury", "Venus", "Earth", "Mars" };

int main() {
    for (int i = 0; i < static_cast<int>(Planet::Count); i++) {
        Planet p = static_cast<Planet>(i);
        cout << i << ": " << PLANET_NAMES[static_cast<int>(p)] << endl;
    }
    return 0;
}
```

**Output:**
```
0: Mercury
1: Venus
2: Earth
3: Mars
```

📌 `Count` automatically equals the **number of real values** — when you add a new planet before `Count`, the loop updates itself.

⚠️ Works only for enums with **consecutive values starting at 0**.

> 📚 `for` loops are covered in a later lesson.

---

## 16. Bit Flags with Enums (Advanced)

> 🎓 *Optional — uses bitwise operators, covered in the Operators lesson.*

Sometimes a value can have **several options at once** (e.g., a file that's both readable *and* writable). Each option is given a **power of 2**, so it occupies its own **bit**:

```cpp
#include <iostream>
using namespace std;

enum Permission {
    READ    = 1,   // 0001
    WRITE   = 2,   // 0010
    EXECUTE = 4,   // 0100
    DELETE  = 8    // 1000
};

int main() {
    int userPerms = READ | WRITE;     // combine: 0011 = 3

    if (userPerms & READ)    cout << "Can read" << endl;
    if (userPerms & WRITE)   cout << "Can write" << endl;
    if (userPerms & EXECUTE) cout << "Can execute" << endl;   // not printed
    return 0;
}
```

**Output:**
```
Can read
Can write
```

```
 READ    = 0 0 0 1
 WRITE   = 0 0 1 0
           ───────  OR (|)
 Combined= 0 0 1 1   → has READ and WRITE
```

📌 Plain `enum` is often used here because it converts to `int` automatically. With `enum class`, you must define the `|` and `&` operators yourself (operator overloading — a later topic).

---

## 17. Best Practices

1. ✅ **Prefer `enum class`** over plain `enum` in new code.
2. ✅ Use **PascalCase** for the enum name (`OrderStatus`) and its values (`Shipped`) with `enum class`; `UPPER_CASE` is common for plain enums.
3. ✅ Use a **singular** name: `Color`, not `Colors` — a variable holds **one** color.
4. ✅ Use enums instead of **magic numbers** or **strings** for fixed sets of options.
5. ✅ Use enums instead of **`bool` parameters** that are unclear at the call site.
6. ✅ Always **initialize** enum variables.
7. ✅ Handle **every** enum value in `switch` statements, and compile with `-Wall` to catch missing cases.
8. ✅ **Validate integers** before casting them to an enum.
9. ✅ Specify the **underlying type** when memory or size matters.
10. ✅ Write a `toString()` function when you need to display names.

---

## 18. Common Beginner Mistakes

| ❌ Mistake | ✅ Correct | Why |
|-----------|-----------|-----|
| `enum Color { RED, GREEN }` (no `;`) | `enum Color { RED, GREEN };` | Semicolon required after `}` |
| `enum Color { RED; GREEN; BLUE };` | `enum Color { RED, GREEN, BLUE };` | Separate with commas |
| `Color c = 1;` | `Color c = static_cast<Color>(1);` | No implicit int → enum |
| `Color c = Red;` (with `enum class`) | `Color c = Color::Red;` | Scoped enums need the prefix |
| `cout << Color::Red;` (enum class) | `cout << static_cast<int>(Color::Red);` | No implicit conversion |
| Expecting `cout` to print `"RED"` | Write a `toString()` function | Enums print as numbers |
| Two plain enums with the same value names | Use `enum class` or prefixes | Names leak and conflict |
| `enum Color { RED, GREEN, RED };` | Unique enumerator names | Duplicate names not allowed |
| Assuming the first value is 1 | It's **0** by default | Set `= 1` if needed |
| `static_cast<Color>(userInput)` without checks | Validate the range first | Invalid values compile silently |
| Using `Colors` as the enum name | Use `Color` | A variable holds one value |
| Forgetting a case in `switch` | Handle all values, use `-Wall` | Silent bugs |

---

## 19. Practice Exercises

### 🟢 Easy

**Exercise 1:** Create a plain `enum Season { SPRING, SUMMER, AUTUMN, WINTER };`. Print the integer value of each season.

**Exercise 2:** Predict the values of each enumerator:
```cpp
enum Code { A, B = 5, C, D = 2, E, F };
```

**Exercise 3:** Create an `enum Month` where `JAN` starts at **1**. Print the value of `JUN` and `DEC`.

### 🟡 Medium

**Exercise 4:** The code below fails to compile. Explain why and fix it using `enum class`:
```cpp
enum Fruit { APPLE, ORANGE, MANGO };
enum Color { RED, ORANGE, GREEN };
```

**Exercise 5:** Create `enum class Direction { North, East, South, West };` and write a function `string toString(Direction d)` that returns the name. Print all four directions.

**Exercise 6:** Create `enum class Size : char { Small, Medium, Large };`. Print `sizeof(Size)` and compare it with a normal `enum class` without an underlying type.

**Exercise 7:** Ask the user to enter a number from **0–2** for coffee size (Small, Medium, Large). Validate the input, convert it to an `enum class`, and print the price:
Small = ₹100, Medium = ₹150, Large = ₹200.

### 🔴 Challenge

**Exercise 8:** Create `enum class Operation { Add, Subtract, Multiply, Divide };` and a function `double calculate(double a, double b, Operation op)`. Test all four operations.

**Exercise 9:** Create `enum class TrafficLight { Red, Yellow, Green };` and a function `TrafficLight next(TrafficLight current)` that returns the next light in the cycle:
**Red → Green → Yellow → Red**. Print the sequence for 6 changes starting from Red.

**Exercise 10:** Explain what's wrong with this program, even though it compiles:
```cpp
enum Animal { DOG, CAT };
enum Vehicle { CAR, BIKE };

int main() {
    Animal pet = DOG;
    Vehicle ride = CAR;
    if (pet == ride) {
        cout << "Same!" << endl;
    }
    return 0;
}
```
Then rewrite it so the compiler **prevents** this mistake.

---

### ✅ Solutions

<details>
<summary>Click to view Exercise 1 solution</summary>

```cpp
#include <iostream>
using namespace std;

enum Season { SPRING, SUMMER, AUTUMN, WINTER };

int main() {
    cout << "SPRING = " << SPRING << endl;   // 0
    cout << "SUMMER = " << SUMMER << endl;   // 1
    cout << "AUTUMN = " << AUTUMN << endl;   // 2
    cout << "WINTER = " << WINTER << endl;   // 3
    return 0;
}
```
</details>

<details>
<summary>Click to view Exercise 2 answer</summary>

| Enumerator | Value | Reason |
|------------|-------|--------|
| `A` | 0 | First, default |
| `B` | 5 | Explicitly set |
| `C` | 6 | B + 1 |
| `D` | 2 | Explicitly set |
| `E` | 3 | D + 1 |
| `F` | 4 | E + 1 |
</details>

<details>
<summary>Click to view Exercise 3 solution</summary>

```cpp
#include <iostream>
using namespace std;

enum Month { JAN = 1, FEB, MAR, APR, MAY, JUN, JUL, AUG, SEP, OCT, NOV, DEC };

int main() {
    cout << "JUN = " << JUN << endl;   // 6
    cout << "DEC = " << DEC << endl;   // 12
    return 0;
}
```
</details>

<details>
<summary>Click to view Exercise 4 solution</summary>

**Why it fails:** plain enum names leak into the surrounding scope, so `ORANGE` is declared twice.

```cpp
#include <iostream>
using namespace std;

enum class Fruit { Apple, Orange, Mango };
enum class Color { Red, Orange, Green };   // ✅ no conflict

int main() {
    Fruit f = Fruit::Orange;
    Color c = Color::Orange;
    cout << static_cast<int>(f) << " " << static_cast<int>(c) << endl;   // 1 1
    return 0;
}
```
</details>

<details>
<summary>Click to view Exercise 5 solution</summary>

```cpp
#include <iostream>
#include <string>
using namespace std;

enum class Direction { North, East, South, West };

string toString(Direction d) {
    switch (d) {
        case Direction::North: return "North";
        case Direction::East:  return "East";
        case Direction::South: return "South";
        case Direction::West:  return "West";
    }
    return "Unknown";
}

int main() {
    cout << toString(Direction::North) << endl;
    cout << toString(Direction::East)  << endl;
    cout << toString(Direction::South) << endl;
    cout << toString(Direction::West)  << endl;
    return 0;
}
```
</details>

<details>
<summary>Click to view Exercise 6 solution</summary>

```cpp
#include <iostream>
using namespace std;

enum class Size : char { Small, Medium, Large };
enum class DefaultSize { Small, Medium, Large };

int main() {
    cout << "Size (char)      : " << sizeof(Size) << " byte" << endl;          // 1
    cout << "DefaultSize (int): " << sizeof(DefaultSize) << " bytes" << endl;  // 4
    return 0;
}
```
</details>

<details>
<summary>Click to view Exercise 7 solution</summary>

```cpp
#include <iostream>
using namespace std;

enum class CoffeeSize { Small, Medium, Large };

int getPrice(CoffeeSize size) {
    switch (size) {
        case CoffeeSize::Small:  return 100;
        case CoffeeSize::Medium: return 150;
        case CoffeeSize::Large:  return 200;
    }
    return 0;
}

int main() {
    int choice;
    cout << "Choose size (0 = Small, 1 = Medium, 2 = Large): ";
    cin >> choice;

    if (choice < 0 || choice > 2) {
        cout << "Invalid choice!" << endl;
        return 1;
    }

    CoffeeSize size = static_cast<CoffeeSize>(choice);
    cout << "Price: Rs. " << getPrice(size) << endl;
    return 0;
}
```
</details>

<details>
<summary>Click to view Exercise 8 solution</summary>

```cpp
#include <iostream>
using namespace std;

enum class Operation { Add, Subtract, Multiply, Divide };

double calculate(double a, double b, Operation op) {
    switch (op) {
        case Operation::Add:      return a + b;
        case Operation::Subtract: return a - b;
        case Operation::Multiply: return a * b;
        case Operation::Divide:   return a / b;
    }
    return 0;
}

int main() {
    double x = 20, y = 4;
    cout << "Add      : " << calculate(x, y, Operation::Add) << endl;       // 24
    cout << "Subtract : " << calculate(x, y, Operation::Subtract) << endl;  // 16
    cout << "Multiply : " << calculate(x, y, Operation::Multiply) << endl;  // 80
    cout << "Divide   : " << calculate(x, y, Operation::Divide) << endl;    // 5
    return 0;
}
```
</details>

<details>
<summary>Click to view Exercise 9 solution</summary>

```cpp
#include <iostream>
#include <string>
using namespace std;

enum class TrafficLight { Red, Yellow, Green };

TrafficLight next(TrafficLight current) {
    switch (current) {
        case TrafficLight::Red:    return TrafficLight::Green;
        case TrafficLight::Green:  return TrafficLight::Yellow;
        case TrafficLight::Yellow: return TrafficLight::Red;
    }
    return TrafficLight::Red;
}

string toString(TrafficLight light) {
    switch (light) {
        case TrafficLight::Red:    return "Red";
        case TrafficLight::Yellow: return "Yellow";
        case TrafficLight::Green:  return "Green";
    }
    return "Unknown";
}

int main() {
    TrafficLight light = TrafficLight::Red;
    cout << toString(light);

    for (int i = 0; i < 6; i++) {
        light = next(light);
        cout << " -> " << toString(light);
    }
    cout << endl;
    return 0;
}
```

**Output:**
```
Red -> Green -> Yellow -> Red -> Green -> Yellow -> Red
```
</details>

<details>
<summary>Click to view Exercise 10 solution</summary>

**What's wrong:** `DOG` and `CAR` are both `0`. Plain enums convert to integers, so comparing an animal with a vehicle compiles and prints `"Same!"` — a meaningless comparison that hides a logic bug.

**Fix with `enum class`:**
```cpp
#include <iostream>
using namespace std;

enum class Animal { Dog, Cat };
enum class Vehicle { Car, Bike };

int main() {
    Animal pet = Animal::Dog;
    Vehicle ride = Vehicle::Car;

    // if (pet == ride) { }   // ❌ compile error: no match for 'operator=='
    //                         //    the compiler now catches the mistake ✅
    return 0;
}
```
</details>

---

## 20. Quick Quiz

**Q1.** What is an enumeration?
- a) A built-in data type for decimals
- b) A user-defined type with a fixed set of named integer constants
- c) A type of loop
- d) A string array

**Q2.** In `enum Color { RED, GREEN, BLUE };`, what is the value of `BLUE`?
- a) 1
- b) 2
- c) 3
- d) "BLUE"

**Q3.** In `enum Level { LOW = 5, MEDIUM, HIGH };`, what is `HIGH`?
- a) 2
- b) 6
- c) 7
- d) 3

**Q4.** Which line is **valid**?
- a) `Color c = 1;`
- b) `Color c = static_cast<Color>(1);`
- c) `Color c = "GREEN";`
- d) `Color c = Color(GREEN, 1);`

**Q5.** How do you access `Red` in `enum class Color { Red, Green };`?
- a) `Red`
- b) `Color.Red`
- c) `Color::Red`
- d) `Color->Red`

**Q6.** What does `cout << static_cast<int>(Color::Green);` print for the enum in Q5?
- a) Green
- b) 0
- c) 1
- d) Error

**Q7.** Which is **NOT** an advantage of `enum class` over plain `enum`?
- a) No name conflicts
- b) No implicit conversion to int
- c) Cannot compare different enum types
- d) Prints names automatically with `cout`

**Q8.** What is the default underlying type of an `enum class`?
- a) `char`
- b) `short`
- c) `int`
- d) `long long`

**Q9.** Is a semicolon required after the closing `}` of an enum?
- a) Yes
- b) No
- c) Only for `enum class`
- d) Only for plain enum

**Q10.** Which C++20 feature lets you write `Red` instead of `Color::Red` inside a scope?
- a) `using namespace Color;`
- b) `using enum Color;`
- c) `import Color;`
- d) `#include Color`

<details>
<summary>✅ Click to view answers</summary>

| Q | Answer | Explanation |
|---|--------|-------------|
| 1 | b) | A list of named integer constants |
| 2 | b) 2 | RED=0, GREEN=1, BLUE=2 |
| 3 | c) 7 | LOW=5, MEDIUM=6, HIGH=7 |
| 4 | b) | int → enum needs an explicit cast |
| 5 | c) `Color::Red` | Scoped enums use `::` |
| 6 | c) 1 | Red=0, Green=1 |
| 7 | d) | Neither kind prints names automatically |
| 8 | c) `int` | |
| 9 | a) Yes | Both kinds need `;` |
| 10 | b) | `using enum` (C++20) |

</details>

---

## 21. Summary

- ✅ An **enumeration** is a user-defined type with a **fixed set of named integer constants**.
- ✅ Enums replace **magic numbers** and **strings** — readable, fast, and safe.
- ✅ Syntax: `enum Name { A, B, C };` — **semicolon required**.
- ✅ Values start at **0** and increase by **1**; you can set **custom values**, and counting continues from the last one.
- ✅ Plain enum → `int` is **automatic**; `int` → enum needs **`static_cast`** — validate first!
- ✅ Plain enums have problems: **name leakage**, **comparing unrelated enums**, **silent int conversion**.
- ✅ **`enum class`** (C++11) fixes all of them: access with `Name::Value`, no implicit conversion, type-safe.
- ✅ Choose the **underlying type** with `: char`, `: short`, etc. to control size.
- ✅ Enums pair perfectly with **`switch`** — and `-Wall` warns about missing cases.
- ✅ **`using enum`** (C++20) shortens names inside a scope.
- ✅ Print names using a **`toString()`** function.
- ✅ Use enums as **function parameters and return types** for self-documenting code.
- ✅ A **`Count`** enumerator helps loop through values.
- ✅ **Bit flags** (powers of 2) let one variable hold multiple options.
- ✅ **Default choice in modern C++: `enum class`.**

---



---

> 📝 *Tip for students: Whenever you catch yourself writing `if (status == 3)` or `if (type == "admin")`, stop and ask: "Should this be an enum?" The answer is usually yes!*
