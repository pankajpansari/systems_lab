```c
#include <cstdio>
#include <cstdlib>

int main() {
    fprintf(stdout, "hello, kitty\n");
    exit(0);
}
```

Note the `\n` at the end of the format string: that’s what starts a new line. In C and C++, you must explicitly say when to end a line (as distinct from Python’s print statement).