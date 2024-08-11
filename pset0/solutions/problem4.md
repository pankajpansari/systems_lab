```c
#include <cstdio>
#include <cctype>

int main() {
    unsigned long nc = 0, nw = 0, nl = 0;
    bool in_spaces = true;
    while (true) {
        int ch = fgetc(stdin);
        if (ch == EOF) {
            break;
        }
        ++nc;

        bool this_space = isspace((unsigned char) ch);   // `ch` would be OK too
        if (in_spaces && !this_space) {
            ++nw;
        }
        in_spaces = this_space;

        if (ch == '\n') {
            ++nl;
        }
    }
    fprintf(stdout, "%8lu %7lu %7lu\n", nl, nw, nc);
}
```

Some notes:

- You must `#include <cctype>` to gain access to `isspace`. Reference material for a library function will usually tell you what to include for that library function; for instance, in a manual page:
 
    SYNOPSIS

    ```c
    #include <ctype.h>

    int
    isspace(int c);
    ```

Or on https://cppreference.com, look for the “Defined in header” note.
 
- The C and C++ languages have unfortunate features that can trip up an inexperienced programmer, and that an experienced programmer will treat defensively. Here, we deal with a design flaw in `<cctype>` functions like `isspace`. Even though their argument type is `int`, these functions can crash or cause a security hole if the actual argument has an unusual value like -2. (“The behavior is undefined if the value of `ch` is not representable as `unsigned char` and is not equal to `EOF`.”) Because we’ve been bitten by this flaw, we often cast the argument of `isspace` to `unsigned char` if it isn’t an `unsigned char` already. We hope the class will introduce you to these kinds of defensive patterns.
 
- The `fprintf` format `"%8lu %7lu %7lu\n"` is better than the more-obvious `"%8lu%8lu%8lu\n"` because it ensures there’s at least one space between the counts, even when standard input has more than 9999999 words or bytes.
 