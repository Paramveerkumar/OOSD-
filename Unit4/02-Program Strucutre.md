# Program Structure

**Unit 1: C++ Basics** | Lecture note 2

---

## 1. Parts of a C++ Program

A typical C++ program is organised in the following order:

| # | Section | Purpose | Example |
|---|---|---|---|
| 1 | Documentation (comments) | Describe the program | `// Area of a circle` |
| 2 | Preprocessor directives | Include headers, define macros | `#include <iostream>` |
| 3 | Namespace declaration | Choose which names are visible | `using namespace std;` |
| 4 | Global declarations | Constants, global variables, function prototypes, classes | `const double PI = 3.14159;` |
| 5 | `main()` function | Entry point of execution | `int main() { ... }` |
| 6 | User-defined functions | Reusable blocks called from `main()` | `double area(double r) { ... }` |

Only `main()` is mandatory. Everything else is added as the program needs it.

Complete example: [`examples/program_structure.cpp`](examples/program_structure.cpp)

---

## 2. Anatomy of the Simplest Program

```cpp
#include <iostream>                      // (1)
using namespace std;                     // (2)

int main() {                             // (3)
    cout << "Hello, C++!" << endl;       // (4)
    return 0;                            // (5)
}                                        // (6)
```

| Line | Explanation |
|---|---|
| (1) | `#include` is a **preprocessor directive**. It copies the contents of the header `iostream` (input/output streams) into the program before compilation. |
| (2) | Makes names from the `std` namespace (such as `cout`) usable without the `std::` prefix. |
| (3) | Declares the function `main`. It returns an `int` to the operating system. Execution **begins here**. |
| (4) | A **statement**. `cout` is the standard output stream, `<<` is the insertion operator, `endl` ends the line and flushes the output. Every statement ends with `;`. |
| (5) | Returns `0` to the operating system, which means *success*. A non-zero value signals an error. In `main`, this line may be omitted (the compiler assumes `return 0;`). |
| (6) | The closing brace ends the body (block) of `main`. |

---

## 3. Preprocessor Directives and Header Files

Lines beginning with `#` are processed **before** compilation.

```cpp
#include <iostream>        // standard header: angle brackets
#include "myfile.h"        // your own header: double quotes
#define MAX 100            // macro (text substitution)
```

Common standard headers:

| Header | Provides |
|---|---|
| `<iostream>` | `cin`, `cout`, `cerr` |
| `<iomanip>` | Output formatting (`setw`, `setprecision`) |
| `<cmath>` | `sqrt`, `pow`, `sin`, `abs`, ... |
| `<string>` | `string` class |
| `<cstdlib>` | `rand`, `exit`, `atoi` |
| `<vector>`, `<map>`, ... | STL containers |

Preprocessor lines are **not** statements, so they do not end with a semicolon.

---

## 4. The `main()` Function

- Every program must have exactly one `main()`.
- The operating system calls it when the program starts; the program ends when `main` returns.
- Two standard forms:

```cpp
int main() { ... }                          // no command-line arguments
int main(int argc, char *argv[]) { ... }    // with command-line arguments
```

- The return type is always `int`; `void main()` is **not** standard C++.

---

## 5. Comments

Comments are ignored by the compiler and document the code.

```cpp
// Single-line comment

/* Multi-line
   comment */
```

Good practice: explain *why* the code does something, not just *what* it does. Comments cannot be nested (`/* /* */ */` is an error).

---

## 6. Statements, Blocks and Whitespace

- A **statement** is a complete instruction ending in `;` (declaration, expression, function call, `return`, ...).
- A **block** is a group of statements inside `{ }`; it forms a scope.
- C++ ignores extra spaces, tabs, and blank lines, so use indentation to make code readable.
- C++ is **case-sensitive**: `Main` is not `main`.

---

## 7. Tokens (Building Blocks)

The compiler breaks source code into **tokens**:

| Token | Examples |
|---|---|
| Keywords | `int`, `return`, `class`, `if` |
| Identifiers | `radius`, `totalMarks` |
| Literals | `42`, `3.14`, `'A'`, `"text"` |
| Operators | `+`, `<<`, `==`, `&&` |
| Punctuators | `;`, `,`, `{ }`, `( )` |

---

## 8. Basic Input and Output

```cpp
int age;
cout << "Enter your age: ";     // output to screen
cin >> age;                     // input from keyboard
cout << "Age is " << age << endl;
```

Useful **escape sequences** inside strings and characters:

| Sequence | Meaning |
|---|---|
| `\n` | New line |
| `\t` | Horizontal tab |
| `\\` | Backslash |
| `\"` | Double quote |
| `\0` | Null character |

---

## 9. Compiling and Running

```bash
g++ -std=c++17 -Wall -Wextra -o program_structure 01-cpp-basics/examples/program_structure.cpp
./program_structure
```

Expected output:

```
--------------------
Radius        : 2.5
Area          : 19.6349
Square root 16: 4
Escape codes  : tab[	] newline follows
--------------------
```

(See the *Overview of C++* note for Windows commands, or run it on GitHub with Codespaces.)

---

## 10. Common Errors

| Error message (typical) | Cause | Fix |
|---|---|---|
| `expected ';' before ...` | Missing semicolon | Add `;` at the end of the statement |
| `'cout' was not declared in this scope` | Missing `#include <iostream>` or `std::` | Add the header and `using namespace std;` |
| `undefined reference to 'main'` | No `main()` in the file | Define `int main()` |
| `expected '}' at end of input` | Unbalanced braces | Match every `{` with a `}` |
| `'Cout' was not declared` | Wrong capitalisation | C++ is case-sensitive: use `cout` |

---

## Summary

- A program consists of comments, preprocessor directives, namespace declaration, global declarations, `main()`, and user-defined functions.
- Execution starts at `main()`, which returns `0` on success.
- Preprocessor lines start with `#` and have no semicolon; statements end with `;`.
- Source code is made of tokens: keywords, identifiers, literals, operators, and punctuators.
- `cout <<` writes output, `cin >>` reads input.

## Review Questions

1. List the six sections of a C++ program and state the purpose of each.
2. Explain every line of the "Hello, C++!" program.
3. Why does `main()` return `int`? What does `return 0;` signify?
4. Differentiate `#include <file>` from `#include "file"`.
5. What are the types of comments in C++? Can they be nested?
6. What is a token? Name the five kinds.
7. Find and correct the errors:

```cpp
#include <iostream>
int Main() {
    cout << "Hello"
    return 0;
}
```

8. Write a program that reads two numbers and prints their sum, using the structure described in this note.
