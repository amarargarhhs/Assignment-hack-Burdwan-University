# 6. Write a Function to Find Whether a Given Number Is Prime or Not — Use It to Generate Prime Numbers Less Than 100

### 1. Problem Understanding

A **prime number** is a positive integer greater than 1 that is divisible **only by 1 and itself**. Examples: 2, 3, 5, 7, 11, 13, …

The assignment has **two parts:**
1. **Write a function** `isPrime(n)` that returns whether `n` is prime.
2. **Use that same function** in `main()` to print **all prime numbers less than 100**.

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| Function with return value | `isPrime()` returns 1 (prime) or 0 (not prime) |
| Divisibility test | `n % i == 0` means `i` divides `n` exactly |
| Modulus operator | `%` gives the remainder of division |
| Loop range optimization | Only need to test divisors from 2 up to **√n** (see note below) |
| Nested loops in `main()` | Outer loop 2→99; inner loop is inside the `isPrime` call |

> **Optimisation insight (mathematical):** To test if `n` is prime you only need to check divisors up to **√n**. If `n` has any divisor greater than √n, it must have a matching divisor smaller than √n (because if `a × b = n` and both were > √n, then `a × b > n`). So checking to `√n` is enough.

### 3. Algorithm (Step-by-Step)

**Part 1 — `isPrime(n)` function:**
1. If `n < 2` → return 0 (not prime).
2. For `i = 2; i <= sqrt(n); i++`:
   - If `n % i == 0` → return 0 (divisible, not prime).
3. Return 1 (no divisor found → prime).

**Part 2 — `main()` generation:**
1. For `num = 2; num < 100; num++`:
   - If `isPrime(num) == 1` → print `num`.
2. Stop.

### 4. The C Program (Source Code)

```c
#include <stdio.h>
#include <math.h>

/* function to check whether n is prime:
   returns 1 if prime, 0 otherwise */
int isPrime(int n)
{
    int i;

    if (n < 2)
        return 0;                    /* 0 and 1 are not prime */

    for (i = 2; i <= sqrt(n); i++)
    {
        if (n % i == 0)
            return 0;                /* found a divisor -> not prime */
    }

    return 1;                        /* no divisor found -> prime */
}

int main()
{
    int num;

    printf("Prime numbers less than 100:\n");

    for (num = 2; num < 100; num++)
    {
        if (isPrime(num) == 1)
            printf("%d ", num);
    }

    printf("\n");
    return 0;
}
```

> **Compilation note:** because this program uses `sqrt()` from `<math.h>`, compile it with the math library linked: `gcc prime.c -o prime -lm` (the `-lm` flag on Linux/Unix).

### 5. Line-by-Line Explanation

| Code | What it does |
|---|---|
| `int isPrime(int n)` | The function — takes an integer, returns 1/0 |
| `if (n < 2) return 0;` | Handles 0 and 1 (and negatives) — not prime |
| `for (i = 2; i <= sqrt(n); i++)` | Tests only divisors from 2 up to √n |
| `if (n % i == 0) return 0;` | Any exact divisor proves non-prime; return immediately |
| `return 1;` | Reached only if no divisor was found |
| `for (num = 2; num < 100; num++)` | Walks through every integer from 2 to 99 |
| `if (isPrime(num) == 1) printf(...)` | **Uses the same function** to filter and print primes |

### 6. Dry Run of `isPrime()`

**Check n = 13:**
| i | i ≤ √13 (≈3.6) | 13 % i | Result |
|---|---|---|---|
| 2 | yes | 1 | continue |
| 3 | yes | 1 | continue |
| 4 | no | — | loop ends |

→ return 1 → **13 is prime** ✓

**Check n = 15:**
| i | i ≤ √15 (≈3.8) | 15 % i | Result |
|---|---|---|---|
| 2 | yes | 1 | continue |
| 3 | yes | 0 | **return 0 → not prime** ✓ |

### 7. Sample Output

```
Prime numbers less than 100:
2 3 5 7 11 13 17 19 23 29 31 37 41 43 47 53 59 61 67 71 73 79 83 89 97
```

*(These are exactly the 25 primes below 100 — verified against the standard prime list.)*

### 8. Edge Cases Table

| Input | Result | Reason |
|---|---|---|
| 0 | not prime | Less than 2 |
| 1 | not prime | Definition requires > 1 |
| 2 | prime | Divisible only by 1 and itself (smallest prime) |
| 3 | prime | Only divisors 1, 3 |
| 4 | not prime | 4 % 2 == 0 |
| 9 | not prime | 9 % 3 == 0 |
| 97 | prime | No divisor up to √97 |
| Negative | not prime | Less than 2 |

### 9. Common Mistakes to Avoid

1. **Starting the divisor loop at 1** — every number is divisible by 1, so nothing would ever be prime; start at **2**.
2. **Treating 1 as prime** — 1 is **not** prime by definition; the `n < 2` check handles this.
3. **Testing up to `n` instead of `√n`** — still correct, but slower. The loop `i <= n-1` is wasteful for large numbers.
4. **Forgetting `#include <math.h>`** — compiler error for `sqrt()`.
5. **Not linking the math library** (`-lm`) — linker error on Linux.
6. **Declaring `isPrime` but computing primes inline in `main()`** — the assignment explicitly says to **use the same function**.

### 10. Alternative Versions

**Without `sqrt()` (avoids `-lm`, uses `i*i <= n`):**
```c
#include <stdio.h>

int isPrime(int n)
{
    int i;
    if (n < 2) return 0;
    for (i = 2; i * i <= n; i++)   /* equivalent to i <= sqrt(n) */
        if (n % i == 0)
            return 0;
    return 1;
}

int main()
{
    int num;
    printf("Prime numbers less than 100:\n");
    for (num = 2; num < 100; num++)
        if (isPrime(num))
            printf("%d ", num);
    printf("\n");
    return 0;
}
```

**Using `sqrt()` with a `double` cast (very explicit):**
```c
#include <stdio.h>
#include <math.h>

int isPrime(int n)
{
    int i;
    if (n < 2) return 0;
    for (i = 2; i <= (int)sqrt((double)n); i++)
        if (n % i == 0)
            return 0;
    return 1;
}
```

### 11. Brief Result Table (for the lab record)

**Primes less than 100 (output):**
```
2 3 5 7 11 13 17 19 23 29 31 37 41 43 47 53 59 61 67 71 73 79 83 89 97
```

| Number of primes below 100 | 25 |
|---|---|
| Smallest prime | 2 |
| Largest prime < 100 | 97 |

---
