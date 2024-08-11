```c
#include <cstdio>

int main() {
    unsigned long n = 0;
    while (fgetc(stdin) != EOF) {
        ++n;
    }
    fprintf(stdout, "%8lu\n", n);
}
```
We used an `unsigned long` to keep track of the number of characters. Unsigned numbers cannot be negative, and are often used to count things. The `fprintf` specifier for `unsigned long` is `lu`, where `l` means `long` and `u` means `unsigned`. It’s required that `fprintf` specifiers match with the actual arguments, so the compiler would produce a warning if you left the `l`  off. (Try it to see!)