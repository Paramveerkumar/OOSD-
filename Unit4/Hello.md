# Overview of C++

**Unit 1: C++ Basics** | Lecture note 1

---

## 1. What is C++?

C++ is a general-purpose, compiled, statically typed programming language that supports **procedural**, **object-oriented**, and **generic** programming. It gives programmers high-level abstractions (classes, templates, a rich standard library) while keeping low-level control over memory and hardware.

## 2. History

| Year | Event |
|---|---|
| 1972 | Dennis Ritchie develops **C** at Bell Labs |
| 1979 | Bjarne Stroustrup begins "**C with Classes**" at Bell Labs |
| 1983 | Renamed **C++** (the `++` is C's increment operator: "C, incremented") |
| 1985 | First commercial release; *The C++ Programming Language* published |
| 1998 | First ISO standard: **C++98** |
| 2011 | **C++11**, a major modernisation (`auto`, lambdas, move semantics, smart pointers) |
| 2014 / 2017 | **C++14**, **C++17** (refinements, `std::optional`, structured bindings) |
| 2020 | **C++20** (concepts, ranges, coroutines, modules) |
| 2023 | **C++23** |

This course uses **C++17**.

## 3. Features of C++

| Feature | Explanation |
|---|---|
| Object-oriented | Classes, objects, inheritance, polymorphism, encapsulation, abstraction |
| Compiled | Source is translated to native machine code, so programs run fast |
| Statically typed | Type errors are caught at compile time |
| Portable | Standard C++ compiles on Windows, Linux, macOS, and embedded systems |
| Rich library | Standard Template Library (containers, algorithms, iterators) |
| Memory control | Pointers, `new`/`delete`, and direct hardware access |
| Multi-paradigm | Procedural, object-oriented, generic, and functional styles |
| Extensible | Operator overloading and user-defined types |

## 4. Object-Oriented Concepts

1. **Object:** an entity with state (data) and behaviour (functions).
2. **Class:** a blueprint from which objects are created.
3. **Encapsulation:** bundling data and the functions that operate on it.
4. **Data hiding:** restricting direct access to internal data (`private`).
5. **Abstraction:** exposing essential features, hiding implementation detail.
6. **Inheritance:** a new class reuses and extends an existing class.
7. **Polymorphism:** one interface, many behaviours (overloading, virtual functions).

## 5. C vs C++

| Aspect | C | C++ |
|---|---|---|
| Paradigm | Procedural | Procedural + object-oriented + generic |
| Data and functions | Separate | Combined in classes |
| Data hiding | Not supported | Supported (`private`, `protected`) |
| Function overloading | No | Yes |
| Operator overloading | No | Yes |
| References | No | Yes |
| Namespaces | No | Yes |
| Memory allocation | `malloc` / `free` | `new` / `delete` |
| I/O | `printf` / `scanf` | `cout` / `cin` |
| Exception handling | No | Yes (`try`, `catch`, `throw`) |

## 6. Applications of C++

Operating systems and device drivers, game engines, web browsers, databases, compilers, embedded and real-time systems, financial trading systems, scientific computing, and graphics or CAD software.

## 7. How a C++ Program Is Built

```
source.cpp --> Preprocessor --> Compiler --> Assembler --> Linker --> executable
               (#include,       (to assembly)  (to object     (joins object
                #define)                        code .o)       files + libraries)
```

On the command line:

```bash
g++ -std=c++17 -Wall -Wextra -o hello hello.cpp
./hello
```

## 8. Your First Program

```cpp
#include <iostream>             // input/output library
using namespace std;            // use the standard namespace

int main() {                    // execution starts here
    cout << "Hello, C++!" << endl;
    return 0;                   // 0 means success
}
```

Full version: [`examples/hello.cpp`](examples/hello.cpp)

## 9. Setting Up and Running C++ Programs

A C++ program needs a **compiler** (turns code into an executable) and an **editor or IDE** (where you write code). Install the compiler first, then check it:

```bash
g++ --version
```

### 9.1 macOS

**Step 1: Install the compiler.** Open *Terminal* and run:

```bash
xcode-select --install
```

This installs Apple's Command Line Tools, which include `clang++`. On a Mac, the command `g++` is simply a shortcut to Apple's `clang++`, so `g++ --version` will mention "Apple clang". That is normal, and it compiles standard C++ correctly. (Optional: `brew install gcc` installs the real GNU compiler as `g++-14` or similar, using [Homebrew](https://brew.sh).)

**Step 2: Write the program.** Save the code as `hello.cpp` (for example in a folder `~/cpp`).

**Step 3: Compile and run.**

```bash
cd ~/cpp
g++ -std=c++17 -Wall -Wextra -o hello hello.cpp
./hello
```

### 9.2 Windows

There are three common ways. Pick **one**.

| Option | Best for | Install |
|---|---|---|
| **MinGW-w64 (GCC) via MSYS2** | Same commands as Linux/Mac | Install [MSYS2](https://www.msys2.org), open the *MSYS2 UCRT64* terminal, run `pacman -S mingw-w64-ucrt-x86_64-gcc`, then add `C:\msys64\ucrt64\bin` to the system **PATH** |
| **Visual Studio Community** (MSVC) | Full IDE with debugger | Install [Visual Studio Community](https://visualstudio.microsoft.com), choose the **Desktop development with C++** workload |
| **WSL (Windows Subsystem for Linux)** | Real Linux environment | In PowerShell (as administrator): `wsl --install`, then in Ubuntu run `sudo apt install build-essential` |

**Compile and run with MinGW (PowerShell or Command Prompt):**

```powershell
cd C:\cpp
g++ -std=c++17 -Wall -Wextra -o hello.exe hello.cpp
.\hello.exe
```

**With Visual Studio's compiler**, open *Developer Command Prompt for VS* and run:

```powershell
cl /EHsc /std:c++17 hello.cpp
hello.exe
```

Or simply create a *Console App* project in Visual Studio and press **Ctrl + F5** to run.

### 9.3 Linux (for reference)

```bash
sudo apt install build-essential       # Ubuntu / Debian
g++ -std=c++17 -o hello hello.cpp && ./hello
```

### 9.4 Command Summary

| Task | macOS / Linux | Windows (MinGW) |
|---|---|---|
| Compile | `g++ -std=c++17 -o hello hello.cpp` | `g++ -std=c++17 -o hello.exe hello.cpp` |
| Run | `./hello` | `.\hello.exe` |
| Compile with warnings | add `-Wall -Wextra` | add `-Wall -Wextra` |
| Several source files | `g++ main.cpp util.cpp -o app` | same |

The `Makefile` in this repository (`make`, `make run F=...`) works on macOS, Linux, and WSL. On plain Windows, compile files individually as shown above.

### 9.5 Tools: Editors and IDEs

| Tool | Platform | Notes |
|---|---|---|
| **Visual Studio Code** | Windows, macOS, Linux | Free, lightweight; install the *C/C++* extension by Microsoft (and optionally *Code Runner* to run with one click) |
| **Visual Studio Community** | Windows | Free for students; best debugger on Windows |
| **Xcode** | macOS | Apple's IDE; create a *Command Line Tool* project |
| **CLion** | All | JetBrains IDE (CMake based); free for students |
| **Code::Blocks** | Windows, Linux, macOS | Simple, beginner friendly; download the version bundled with MinGW |
| **Dev-C++** | Windows | Very light; good for small programs |
| **Online compilers** | Any browser | [OnlineGDB](https://www.onlinegdb.com), [Compiler Explorer](https://godbolt.org), [Programiz](https://www.programiz.com/cpp-programming/online-compiler); no installation needed |

**Recommended for this course:** VS Code + `g++` (MSYS2 on Windows, Command Line Tools on macOS), because every command in this repository then works the same way on all computers.

### 9.6 Common Problems

| Message / Symptom | Cause and fix |
|---|---|
| `'g++' is not recognized` (Windows) | Compiler folder is not on **PATH**; add it and reopen the terminal |
| `command not found: g++` (macOS) | Run `xcode-select --install` |
| Console window closes immediately (Windows IDE) | Run from a terminal, or use **Ctrl + F5** in Visual Studio |
| `error: 'cout' was not declared` | Missing `#include <iostream>` or `using namespace std;` |
| `undefined reference to main` | The file has no `main()` function, or you compiled the wrong file |
| Old language features unavailable | Add `-std=c++17` to the compile command |

## Summary

- C++ grew out of C, adding classes and later templates, exceptions, and a large standard library.
- It is compiled, statically typed, and supports several programming paradigms.
- Its object-oriented features are encapsulation, abstraction, inheritance, and polymorphism.
- Programs go through preprocessing, compilation, assembly, and linking.
- Install a compiler (`clang++`/`g++` on macOS, MinGW or Visual Studio on Windows), then compile with `g++ -std=c++17` and run the executable; VS Code is a good free editor for all platforms.

## Review Questions

1. Who developed C++, and why was it first called "C with Classes"?
2. List five features of C++.
3. Differentiate between C and C++ in at least five points.
4. Explain the stages of compilation of a C++ program.
5. Name and briefly describe the four main principles of object-oriented programming.
6. Write a program that prints "Welcome to C++".
7. List the steps to install a C++ compiler and run a program on your own computer (macOS or Windows).
8. Name three IDEs or editors for C++ and one advantage of each.
9. What does the command `g++ -std=c++17 -Wall -o hello hello.cpp` do? Explain each part.
