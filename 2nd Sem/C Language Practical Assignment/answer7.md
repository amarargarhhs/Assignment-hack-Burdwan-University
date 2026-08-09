# 7. WAP to Compute the Factors of a Given Number

### 1. Problem Understanding

**Factors (divisors)** of a number `n` are all positive integers that divide `n` exactly (i.e., leave remainder 0). For example, factors of 12 are: **1, 2, 3, 4, 6, 12**.

**Properties to remember:**
- Every number is a factor of itself (`n ÷ n = 1`).
- Every number has 1 as a factor.
- 1 has only one factor (itself).
- Factors always come in **pairs** that multiply to `n` (e.g., 1×12, 2×6, 3×4).

The program must:
1. Accept a number from the user.
2. Find and display **all** its factors.

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| Modulus operator `%` | `n % i == 0` tells us `i` divides `n` exactly → `i` is a factor |
| Loop from 1 to n | Check every possible divisor in that range |
| Counter/collection of factors | Optional — count them, or store them in an array |

### 3. Algorithm (Step-by-Step)

1. Start.
2. Read number `n` from the user.
3. Print "Factors of n are:".
4. **For `i = 1; i <= n; i++`:**
   - If `n % i == 0` → print `i`.
5. Stop.

### 4. The C Program (Source Code)

**Version 1 — Print factors directly (simplest):**
```c
#include <stdio.h>

int main()
{
    int n, i;

    printf("Enter a number: ");
    scanf("%d", &n);

    printf("Factors of %d are: ", n);

    for (i = 1; i <= n; i++)
    {
        if (n % i == 0)
            printf("%d ", i);
    }

    printf("\n");
    return 0;
}
```

**Version 2 — Count and display factors (more complete, often expected in lab records):**
```c
#include <stdio.h>

int main()
{
    int n, i, count = 0;

    printf("Enter a number: ");
    scanf("%d", &n);

    printf("Factors of %d are: ", n);

    for (i = 1; i <= n; i++)
    {
        if (n % i == 0)
        {
            printf("%d ", i);
            count++;
        }
    }

    printf("\nTotal number of factors = %d\n", count);
    return 0;
}
```

> **Which one to submit?** Version 2 is richer — it also prints the factor **count**, showing deeper understanding (e.g., a prime number has exactly 2 factors). Choose it if your examiner values completeness.

### 5. Line-by-Line Explanation (Version 2)

| Code | What it does |
|---|---|
| `int n, i, count = 0;` | `n` = input number, `i` = loop counter, `count` = factor counter |
| `scanf("%d", &n);` | Reads the number — note `&n` because `n` is an `int`, not an array |
| `for (i = 1; i <= n; i++)` | Check every integer from 1 up to `n` |
| `if (n % i == 0)` | If `i` divides `n` with zero remainder, it is a factor |
| `printf("%d ", i);` | Display the factor |
| `count++;` | Increment the total factor count |
| `printf("\nTotal number of factors = %d\n", count);` | Show how many factors were found |

### 6. Dry Run

**Input: n = 6**

| i | 6 % i | Factor? | count |
|---|---|---|---|
| 1 | 0 | ✓ print 1 | 1 |
| 2 | 0 | ✓ print 2 | 2 |
| 3 | 0 | ✓ print 3 | 3 |
| 4 | 2 | ✗ | 3 |
| 5 | 1 | ✗ | 3 |
| 6 | 0 | ✓ print 6 | 4 |

Output: `Factors of 6 are: 1 2 3 6` — Total = 4 ✓

### 7. Sample Input / Output

**Test 1:**
```
Enter a number: 12
Factors of 12 are: 1 2 3 4 6 12
Total number of factors = 6
```

**Test 2:**
```
Enter a number: 17
Factors of 17 are: 1 17
Total number of factors = 2
```

**Test 3:**
```
Enter a number: 1
Factors of 1 are: 1
Total number of factors = 1
```

### 8. Edge Cases Table

| Input | Factors | Total | Remark |
|---|---|---|---|
| 12 | 1 2 3 4 6 12 | 6 | Normal case |
| 17 | 1 17 | 2 | **Prime** → exactly 2 factors |
| 1 | 1 | 1 | Only one factor |
| 36 | 1 2 3 4 6 9 12 18 36 | 9 | Perfect square — factor 6 listed once |
| 0 | *(loop never matches)* | 0 | Division by zero conceptually; every number divides 0 — avoid in simple program |
| Negative (−12) | *(with i from 1 to n loop won't run)* | 0 | Handle by taking absolute value if needed |

### 9. Common Mistakes to Avoid

1. **Using `n % i == 0` reversed** — the condition must be `n % i == 0`, i.e., `i` divides `n`. Writing `i % n == 0` only ever matches `i = n` for positive numbers (except giving wrong results).
2. **Forgetting `&` in `scanf("%d", &n)`** — without `&`, you pass the value (garbage) instead of the address.
3. **Starting the loop at 0** — `n % 0` is a **division-by-zero crash**. Always start at 1.
4. **Stopping the loop at `n/2`** — you'd miss `n` itself as a factor. (Unless you print `n` separately.)
5. **Not handling `n` itself** — the loop must go up to **`i <= n`** to include the number itself.

### 10. Alternative / Improved Versions

**Optimized — loop only to n/2 and add n separately (roughly half the iterations):**
```c
#include <stdio.h>

int main()
{
    int n, i, count = 0;

    printf("Enter a number: ");
    scanf("%d", &n);

    printf("Factors of %d are: ", n);

    for (i = 1; i <= n / 2; i++)
    {
        if (n % i == 0)
        {
            printf("%d ", i);
            count++;
        }
    }

    printf("%d ", n);        /* n is always its own factor */
    count++;

    printf("\nTotal number of factors = %d\n", count);
    return 0;
}
```

**Factor pairs version (advanced, only checks up to √n):**
```c
#include <stdio.h>
#include <math.h>

int main()
{
    int n, i;

    printf("Enter a number: ");
    scanf("%d", &n);

    printf("Factor pairs of %d:\n", n);

    for (i = 1; i <= sqrt(n); i++)
    {
        if (n % i == 0)
        {
            if (i == n / i)
                printf("%d x %d\n", i, i);      /* perfect square middle pair */
            else
                printf("%d x %d\n", i, n / i);
        }
    }
    return 0;
}
```
*(Output for 36: `1 x 36, 2 x 18, 3 x 12, 4 x 9, 6 x 6` — this is very efficient.)*

### 11. Brief Result Table (for the lab record)

| Input number | Factors | Total count |
|---|---|---|
| 12 | 1 2 3 4 6 12 | 6 |
| 17 | 1 17 | 2 |
| 1 | 1 | 1 |
| 36 | 1 2 3 4 6 9 12 18 36 | 9 |

---
