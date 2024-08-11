**Part 1**

This solution doesn’t sort the arguments in place; instead, it uses selection sort to repeatedly select and print the smallest argument, removing it after printing.

```c
#include <cstring>
#include <cstdio>

int main(int argc, char* argv[]) {
    while (argc > 1) {
        // find the smallest argument
        int smallest = 1;
        for (int i = 2; i < argc; ++i) {
            if (strcmp(argv[i], argv[smallest]) < 0) {
                smallest = i;
            }
        }

        // print it
        fprintf(stdout, "%s\n", argv[smallest]);

        // remove it from the argument set
        argv[smallest] = argv[argc - 1];
        --argc;
    }
}
```

This solution sorts the arguments in place using insertion sort.

```c
#include <cstring>
#include <cstdio>

int main(int argc, char* argv[]) {
    // insertion sort
    for (int i = 2; i < argc; ++i) {
        char* x = argv[i];
        int j = i - 1;
        while (j >= 0 && strcmp(argv[j], x) > 0) {
            argv[j + 1] = argv[j];
            --j;
        }
        argv[j + 1] = x;
    }

    for (int i = 1; i < argc; ++i) {
        fprintf(stdout, "%s\n", argv[i]);
    }
}
```
**Part 2**

Solution using advanced C++ features

More concise code can be written by using more advanced features, such as `auto` and range initialization. You don’t need to understand these features for this class—we will explain them in lecture as required—but they do make C++ more pleasant to program.

```c
#include <string>
#include <vector>
#include <algorithm>
#include <iostream>

int main(int argc, char** argv) {
    std::vector<std::string> args(&argv[1], &argv[argc]); // range initialization
    std::sort(args.begin(), args.end());
    for (auto& s : args) { // range `for`
        std::cout << s << '\n';
    }
}
```