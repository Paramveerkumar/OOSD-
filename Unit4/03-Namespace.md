# Namespace

**Unit 1: C++ Basics** | Lecture note 3

---

## 1. Why Do We Need Namespaces?

In a large program (or when using several libraries), two different parts may use the **same name** for different things. For example, a college library and a hospital system might both define a function called `display()`. When both are used in one program, the compiler cannot tell which one is meant. This is called a **name collision** (or name clash).

A **namespace** is a named region of code that gives its names their own scope. Two namespaces can both contain `display()` without any conflict, because each is reached through its own namespace name.

> **Analogy:** two students in different classes may both be named "Ravi". Saying "Ravi of class 10-A" removes the confusion. The class name works like a namespace.

---

## 2. Defining a Namespace

```cpp
namespace Name {
    // variables, functions, classes, constants ...
}
```

```cpp
namespace College {
    int students = 500;
    void display() { cout << "College: " << students << " students" << endl; }
}
```

- A namespace is defined at global level (or inside another namespace).
- It is **not** followed by a semicolon after the closing brace.
- It can contain variables, functions, classes, enums, constants, and other namespaces.

---

## 3. Accessing Namespace Members

### 3.1 Qualified name (scope resolution operator `::`)

```cpp
College::display();
Hospital::display();
```

The `::` operator says "the `display` that belongs to `College`." This is the safest and clearest method.

### 3.2 `using` declaration (one name)

```cpp
using College::students;
cout << students;            // no prefix needed for this one name
```

### 3.3 `using` directive (all names)

```cpp
using namespace Hospital;
display();                   // calls Hospital::display()
```

### Comparison

| Method | Syntax | Brings in | Risk of collision |
|---|---|---|---|
| Qualified name | `College::display()` | Nothing (explicit every time) | None |
| `using` declaration | `using College::students;` | One name | Low |
| `using` directive | `using namespace College;` | Every name | Higher |

**What if two directives bring in the same name?**

```cpp
using namespace A;
using namespace B;      // both define f()
f();                    // error: call of overloaded 'f()' is ambiguous
```

The compiler reports an **ambiguity** error. The fix is to write `A::f()` or `B::f()`.

---

## 4. The `std` Namespace

Everything in the C++ standard library (`cout`, `cin`, `endl`, `string`, `vector`, ...) is defined inside the namespace **`std`**. So there are two ways to write the same thing:

```cpp
std::cout << "Hello" << std::endl;       // full names

using namespace std;                      // or import everything once
cout << "Hello" << endl;
```

**Good practice**

- In small teaching programs, `using namespace std;` is acceptable.
- In **header files**, never write `using namespace std;` because it forces that choice on every file that includes the header.
- In large projects, prefer `std::` or `using std::cout;`.

---

## 5. Nested Namespaces

A namespace can be placed inside another one. Use `::` repeatedly to reach it.

```cpp
namespace College {
    namespace Library {
        int books = 12000;
    }
}
cout << College::Library::books;
```

(C++17 also allows the shorter form `namespace College::Library { ... }`.)

---

## 6. Namespace Alias

A long namespace name can be given a short nickname:

```cpp
namespace Lib = College::Library;
cout << Lib::books;
```

---

## 7. Extending a Namespace

The same namespace can be reopened in several places (or several files). The members are all added to the one namespace.

```cpp
namespace College { int students = 500; }
namespace College { void showName() { ... } }     // adds to College
```

This is how the standard library itself is spread over many headers.

---

## 8. Unnamed (Anonymous) Namespace

```cpp
namespace {
    int secretCode = 42;
}
```

Members of an unnamed namespace are visible only inside the **same source file**. It is the modern replacement for declaring a global variable `static`.

---

## 9. Complete Example

File: [`examples/namespace_advanced.cpp`](examples/namespace_advanced.cpp) (simpler version: [`examples/namespace_demo.cpp`](examples/namespace_demo.cpp))

```bash
g++ -std=c++17 -Wall -Wextra -o namespace_advanced 01-cpp-basics/examples/namespace_advanced.cpp
./namespace_advanced
```

Output:

```
College: 500 students
Hospital: 40 beds
Library: 12000 books
Books via alias: 12000
Students (using declaration): 500
Hospital: 40 beds
College name: Demo College
Secret code: 42
Printed with the full name std::cout
```

---

## 10. Common Mistakes

| Mistake | Problem | Fix |
|---|---|---|
| Putting `;` after the closing `}` of a namespace | Harmless but unnecessary | Remove it |
| Using two `using namespace` lines with the same function name | Ambiguity error | Use qualified names |
| Writing `using namespace std;` in a header file | Pollutes every file that includes it | Use `std::` in headers |
| Defining a namespace inside a function | Not allowed | Define at global (or namespace) level |
| Forgetting `::` (writing `College.display()`) | `.` is for objects, not namespaces | Use `College::display()` |

---

## Summary

- A namespace groups related names to avoid **name collisions**.
- Access members with `Name::member`, a `using` declaration (one name), or a `using` directive (all names).
- The standard library lives in `std`.
- Namespaces can be nested, given aliases, reopened to add members, or left unnamed to keep names private to one file.

## Review Questions

1. What is a namespace? Why is it needed?
2. What is the use of the scope resolution operator `::`?
3. Differentiate a `using` declaration from a `using` directive.
4. Why is `using namespace std;` discouraged in header files?
5. Two namespaces `A` and `B` both define `show()`. Write code that calls each one, and explain what error appears if both are imported with `using namespace`.
6. What is an unnamed namespace? How does it differ from a named one?
7. What is a namespace alias? Write an example.

## Practice Programs

1. Create namespaces `Square` and `Circle`, each with a function `area()`; call both from `main()`.
2. Create a namespace `Temperature` with `toFahrenheit()` and `toCelsius()`; use a `using` declaration to call only one of them without a prefix.
3. Create `Company` containing a nested namespace `HR`; put a variable `employees` in `HR` and print it using an alias.
4. Write two namespaces that both define a variable named `count` and print both values in one program.
