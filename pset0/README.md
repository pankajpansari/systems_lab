## FoCS Problem Set 0: C++ Warmup
=================================

The goal of this ungraded problem set is to introduce you to course infrastructure, and to warm up your skills in the programming language we use, C++, and the kinds of programs we’ll write. The programming is not intended to be difficult, but don’t settle for sloppy results. The course aims to teach you robust, secure, and understandable coding, as well as fast and correct coding.

Solutions are provided (in /solutions), and you should look at them. Try to work through the problems yourself first, but don’t worry if you get stuck and peek at the solution. Understanding the solution is already a great step. The problems get progressively harder. If you get through all of the problems you are definitely ready for the course; if you get through part 4, you are very likely ready for the course.

You do not need to submit anything for this problem set.

## Overview

This course teaches you the fundamentals of computer systems and systems programming, using C++ as its primary implementation language. C++ is a popular language for low-level systems coding, and unlike C, its predecessor, it comes with an extensive data structure library and high-level abstraction facilities.

Though the course uses C++, it is about systems programming. We don’t dive deep into the language, we do not use its object-oriented features, and we attempt to avoid its most difficult byways.

Some familiarity with C++’s data structures and libraries will be useful. You can get this from free resources on the web, although a textbook and reference is better. Some web resources, such as reference pages, contain an almost overwhelming amount of information. Don’t despair! A good way to use reference pages is to scan down for example code, which can be more concise and clear than the English-language reference material.

- C++ tutorials from LearnCpp.com; cplusplus.com; w3schools.com

- Consult the Resources section in Moodle

It will also be useful to have some familiarity with Unix shell syntax. You should know what standard input and standard output mean, and how to perform simple redirections. Specifically:

- A simple shell command like `prog1` runs `prog1` in the current context: its standard input and standard output are inherited from the shell, which usually means that its input reads from the keyboard and its output is printed to the terminal.

- Typing `prog1 < IN` redirects `prog1`’s standard input to come from a file. In this instance, `prog1` will observe the contents of the `IN` file when reading from its standard input.

- Similarly, `prog1 > OUT` redirects `prog1`’s standard output to go to a file. If the file `OUT` already exists, its contents are overwritten.

- `prog1 < IN > OUT` performs both redirections.

- `prog1 | prog2` (pronounced “`prog1` pipe to `prog2`”) redirects `prog2`’s standard input to read from `prog1`’s standard output. Thus, all the bytes written by `prog1` are read by `prog2` in order.

## Part 0. Course setup and Git

**Objective: Prepare a CS 61 environment and get familiar with the terminal.**

1. Follow the GitHub setup instructions.

2. Create a file in your `cs61-psets/pset0` directory called `hello.txt` that contains one line, the word “`awesome`”. Hit 'return' or 'enter' on your keyboard to create a line break after the word. You can use your favorite text editor (such as `Visual Studio Code`).

    Check your work by running the command make test-hello. This should print something like this:
    ```
    user@xyz:~/focs-2024-psets/pset0$ make test-hello
    ✅ Cool! hello.txt exists and has the right contents.
    ```
3. The git status command will show you that `hello.txt` is not yet part of your official repository:

    ```
    user@xyz:~/focs-2024-psets/pset0$ git status
    On branch main
    Your branch is up to date with 'origin/main'.

    Untracked files:
      (use "git add <file>..." to include in what will be committed)
        hello.txt

    no changes added to commit (use "git add" and/or "git commit -a")
    ```
    
    Add the `hello.txt` file to your repository and push it to GitHub (that is, upload it to the shared GitHub repository). Verify that it has arrived on GitHub by looking at the web page for your repository.

## Part 1. Invoke the compiler

**Objective: Use the C++ compiler and make to build a program.**

Create a C++ source file exiter.cc containing the following code.

```c
#include <cstdlib>

int main() {
    exit(0);
}
```

**QUESTION.** What would this program do?

Then compile the program and run it. Do this two ways.

1. First, call the compiler directly and run the program on the terminal. Here’s how:

    ```bash
    $ c++ -std=gnu++2a -Wall -g -O3 exiter.cc -o exiter
    $ ./exiter
    ```

    The first command invokes the compiler, asking it to compile your source file `(exiter.cc)` into an executable (a program that can be run, here called `exiter`). The flags supplied to the compiler are:

   -  `-std=gnu++2a` selects a recent version of the C++ standard.
   -  `-Wall` asks the compiler to warn you about possible mistakes in your code. You should always make sure that your code compiles without warnings.
   -  `-g` adds debugging information to the compiler’s output.
   -  `-O3` asks the compiler to optimize its code. This makes the code run faster, but can make the result harder to debug and can slow down compilation.

The compiler exits silently if can complete its work without encountering an error or warning, so be excited if there’s no output! If there is an error, you’ll need to fix that error and invoke the compiler again.

The second command (./exiter) invokes the compiled executable, which, as advertised, does nothing.

2. Second, use our provided makefile to build the program. Here’s how:

    ```bash
    $ rm -f exiter
    $ make exiter
      COMPILE exiter.cc
      LINK exiter
    $ ./exiter
    ```

    The first command, `rm -f exiter`, removes the old executable if you made one. The second command, `make exiter`, builds the new executable from rules in the provided GNUmakefile.

## Why?

Makefiles and the `make` program are often used to build software. A makefile defines a set of recipes for building programs and other *targets*; the `make` program reads these recipes and builds targets according to their rules. Crucially, `make` analyzes which *sources* change from run to run and uses this information to speed up the build process. We provide makefiles for all problem sets and lecture codes in this course, and these makefiles have recipes for invoking the compiler, so you will not generally need to invoke the compiler yourself. But it’s super useful to know how to do so!

Our makefiles often hide the arguments of the programs they run. This is to help focus your attention on any warnings or errors. If you want to see the arguments, supply `V=1` as an argument to `make`. For instance:

```bash
$ make clean
$ make V=1 exiter
g++  -std=gnu++2a -Wall -Wextra -Wshadow -g  -fsanitize=undefined -MD -MF .deps/exiter.d -MP -O3 -o exiter.o -c exiter.cc
g++ -std=gnu++2a -Wall -Wextra -Wshadow -g  -fsanitize=undefined  -O3 exiter.o -o exiter
```

As you can see, the makefile supplies more arguments than the simple compiler invocation! For fun, why not try to figure out what those arguments do? (`-Wextra` and `-Wshadow` especially.)

Unlike the direct invocation in step 1, our makefile runs the compiler twice to produce an executable. The first invocation, `COMPILE exiter.cc`, produces an intermediate file called an object file, `exiter.o`; the second invocation, `LINK exiter`, uses the object file to produce an executable. This is unnecessary for a single-file program like `exiter.cc`, but useful for programs generated from multiple source files.

## Part 2. Say hello

**Objective: Recall C++’s output facility.**

Create a program `hello` that prints `hello, kitty` to the terminal (the standard output) and then exits. That is, write a C++ source file `hello.cc`, then compile it into a `hello` executable that behaves like this:

```bash
$ make hello
$ ./hello
hello, kitty
$
```

It’s important that the “hello” message appears on its own line—this output is wrong:

```bash
user@xyz:~/focs-2024-psets/pset0$ ./hello
hello, kittyuser@xyz:~/focs-2024-psets/pset0$ 
```

Hints:

- Start with the code from `exiter.cc`.
- Use the [fprintf library function](https://pubs.opengroup.org/onlinepubs/009695399/functions/fprintf.html).
- To get access to `fprintf`, you will need to `#include <cstdio>`.
- If you make changes to your code, you need to recompile it before you run it again.

## Part 3. Count bytes

**Objective: Recall more of C++’s input/output facilities.**

Now let’s start really programming. Write a program `bc61` that counts the number of bytes in its standard input and prints the result to standard output. Examples:

```bash
$ make bc61
$ echo foo | ./bc61
       4
$ ./bc61 < /dev/null
       0
$ yes | head -c 1000 | ./bc61
    1000
$ ./hello | ./bc61
      13
```

Use the `fgetc` and `fprintf` library functions for C-style I/O. Make sure you test your `bc61` program with different inputs.

> Because your `bc61` program waits for input, running `bc61` on its own will appear to hang the terminal. This can be confusing!
>
>```bash
>$ ./bc61
>[...... nothing happens here forever!!! .......]
>```
>
> When this happens, press Control-C to kill `bc61` without waiting for it to complete its work. Alternately, type stuff, press Return to start a new line, and then press Control-D to pass `bc61` some input. (Pressing Control-D at the beginning of a line informs the terminal that `bc61`’s input has ended; if `bc61` attempts to read more characters, it will receive an end-of-file indication. Pressing Control-D anywhere but at the beginning of a blank line will simply release whatever you've typed on the current line so that `bc61` can read it.)

## Part 4. Count words and lines (harder)

**Objective: Reason through a nontrivial specification for an input/output program.**

Write a program `wc61` that counts words and lines as well as bytes. A word is a sequence of one or more non-whitespace characters, and a line is zero or more bytes terminated by the newline character `\n`. The printout should give lines, words, and bytes in that order. Examples:

```bash
$ echo foo | ./wc61
       1       1       4
$ ./wc61 < /dev/null
       0       0       0
$ yes | head -c 1000 | ./wc61
     500     500    1000
$ yes Hello | head -c 1000 | ./wc61
     166     167    1000
$ ./hello | ./wc61
       1       2      13
$ echo "  hi  " | ./wc61
       1       1       7
```

Use the [isspace](https://cplusplus.com/reference/cctype/isspace/) library function, which is declared in the \<`cctype`\> header, to detect whitespace.

Though this seems like a simple specification, getting simple things right on all input cases is always hard, and it may take you a while to get the answer right! Make sure you create your own test cases; the `echo " WORDS " | ./wc61` pattern is quite useful.

## Part 5. Implement a library function (hard)

**Objective: Understand a library function’s specification, reason about special cases, and practice pointer notation.**

Implement a function `mystrstr` that has exactly the same behavior as [strstr](https://pubs.opengroup.org/onlinepubs/9699919799/functions/strstr.html).

This driver program `strstr61.cc` may be useful for testing:

```c
#include <cstring>
#include <cassert>
#include <cstdio>

char* mystrstr(const char* s1, const char* s2) {
    // Your code here
    return nullptr;
}

int main(int argc, char* argv[]) {
    assert(argc == 3);
    printf("strstr(\"%s\", \"%s\") = %p\n",
           argv[1], argv[2], strstr(argv[1], argv[2]));
    printf("mystrstr(\"%s\", \"%s\") = %p\n",
           argv[1], argv[2], mystrstr(argv[1], argv[2]));
    assert(strstr(argv[1], argv[2]) == mystrstr(argv[1], argv[2]));
}
```
Examples:

```bash
$ make strstr61
$ ./strstr61 "hello, world" "world"
strstr("hello, world", "world") = 0x7ffee2c97bca
mystrstr("hello, world", "world") = 0x7ffee2c97bca
$ ./strstr61 "nfsnafndkanfkan" ""
strstr("nfsnafndkanfkan", "") = 0x7ffee579bbcb
mystrstr("nfsnafndkanfkan", "") = 0x7ffee579bbcb
```

No matter what arguments you supply, the strstr and mystrstr lines should print the same result.

1. First, implement `mystrstr` using array notation (brackets `[]`). Make sure you read the [specification](https://pubs.opengroup.org/onlinepubs/9699919799/functions/strstr.html) for `strstr` carefully so you catch all special cases; do not call any library functions.

2. Second, implement `mystrstr` exclusively using pointer operations: do not use brackets at any point. 

## Part 6. Sort arguments (hard)

**Objective: Handle C strings, program arguments, and more complex library functions. Implement algorithms described in pseudocode (such as insertion sort).**

Write a program `sortargs61` that prints out its command-line arguments, one per line, in dictionary order (also called [lexicographic order](https://en.wikipedia.org/wiki/Lexicographic_order)). Examples:

```bash
$ ./sortargs61 a b c
a
b
c
$ ./sortargs61 e d c b a
a
b
c
d
e
$ ./sortargs61 A a A a A a
A
A
A
a
a
a
$ ./sortargs61
$ ./sortargs61 hello, world, this is a program
a
hello,
is
program
this
world,
```

> A C or C++ program’s command-line arguments are passed in an array called `argv` that is passed to main. Up til now, your main functions have taken no arguments; sortargs61’s main function should take two arguments, as follows:
> 
> ```c
> int main(int argc, char* argv[])
> ```
> 
> argc is the number of arguments, and `argv[I]` is the Ith argument represented as a C string. Note that the `argv[0]` argument is always the name of the program and should generally not be considered a true argument. ([Tutorial](https://www.tutorialspoint.com/cprogramming/c_command_line_arguments.htm))

1. First, use a minimal subset of the C library: use `strcmp` and an $O(n^2)$ algorithm such as [insertion sort](https://en.wikipedia.org/wiki/Insertion_sort) or ][selection sort](https://en.wikipedia.org/wiki/Selection_sort). (Recall that `strcmp(x, y)` returns an integer that is less than zero when `x < y` in lexicographic order, equal to zero when `x` and `y` have the same characters in the same order, and greater than zero when `x > y` in lexicographic order.)

2. (More advanced) Now do it using C++ library features: use `std::string` (the C++ library type for automatically-managed strings), `std::vector` (the C++ library type for automatically-managed, dynamically-sized arrays), `std::sort` (the C++ library’s sort algorithm), and C++-style basic input/output.

## Going further

Here are some other things you should be able to do in C++:

- Dynamically allocate and free an object with `new` and `delete`.
- Dynamically allocate and free an array of objects with `new[]` and `delete[]`.
- Define a structure type.
- Use the C library’s string functions correctly, including memory management.
- Write a function that takes three arguments, a size and two `void*` pointers to objects, and exchanges the values of the two objects (each of which have the given size).

-----
This lab assignment was adapted from [CS 61: Systems Programming and Machine Organization (2024)](https://cs61.seas.harvard.edu/site/2024/) Problem Set 0.