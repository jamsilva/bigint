# bigint

Usage example:
```c
#include "bigint.h"

#include <stdio.h>

int main(){
    char buf[65536];

    /* neat trick to avoid having to write &a, &b, &c everywhere */
    bigint a[1], b[1], c[1];

    /* bigints have to be initialized (same as memset'ed to zero) */
    bigint_init(a);
    bigint_init(b);
    bigint_init(c);

    /* create bigint from string */
    bigint_from_str(a, "123456789");
    bigint_from_str(b, "987654321");

    /* c = a * b */
    /* first parameter is destination parameter */
    bigint_mul(c, a, b);

    /* write and print */
    puts(bigint_write(buf, sizeof(buf), c));

    /* bigints have to be free'd */
    bigint_free(a);
    bigint_free(b);
    bigint_free(c);

    return 0;
}
```

Implementation notes:
* BigInts consist of an array of bigint_words which are unsigned ints as defined in bigint.h
* Try adjusting bigint_word in bigint.h for maximum performance
* If there is a highest word, it should always be non-zero, as assured by bigint_raw_truncate
* Multiplication uses [the Karatsuba algorithm](https://en.wikipedia.org/wiki/Karatsuba_algorithm) for large integers
* If you have to calculate both division and modulo, use div_mod
* The bigint_raw_* methods expect the dst parameter to be sufficiently big, so be careful with those!
