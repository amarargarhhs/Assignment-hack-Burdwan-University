# 19. WAP to Calculate Factorial of a Number (i) Using Recursion, (ii) Using Iteration

### 1. Problem Understanding

The **factorial** of a non-negative integer n, written **n!**, is the product of all positive integers from 1 to n:
```
0! = 1
1! = 1
2! = 2 × 1 = 2
3! = 3 × 2 × 1 = 6
4! = 4 × 3 × 2 × 1 = 24
5! = 120
```

**Mathematical definition (recursive form):**
```
n! = n × (n-1)!          for n > 0
0! = 1                    base case
```

The assignment requires **two implementations:**
1. **Recursion** — `fact(n) = n * fact(n-1)`.
2. **Iteration** — a loop multiplying 1 × 2 × 3 × … × n.

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| Base case | `0! = 1` — stops the recursion |
| Recursive case | `n * fact(n-1)` |
| Accumulator variable | In iteration, `fact *= i` |
| Overflow awareness | Factorials grow extremely fast — use `long`/`long long` |

### 3. Algorithm (Step-by-Step)

**Recursion — `factRecursive(n)`:**
1. If `n == 0` → return 1.
2. Else return `n * factRecursive(n-1)`.

**Iteration — `factIterative(n)`:**
1. Set `fact = 1`.
2. Loop `i = 2 … n`: `fact = fact * i`.
3. Return `fact`.

**main():** read `n`, call both functions, print results.

### 4. The C Program (Source Code)

```c
#include <stdio.h>

/* ---------- (i) FACTORIAL USING RECURSION ---------- */
long factRecursive(int n)
{
    if (n == 0)
        return 1;                /* base case: 0! = 1 */
    return n * factRecursive(n - 1);
}

/* ---------- (ii) FACTORIAL USING ITERATION ---------- */
long factIterative(int n)
{
    long fact = 1;
    int i;

    for (i = 2; i <= n; i++)
        fact = fact * i;

    return fact;
}

int main()
{
    int n;

    printf("Enter a number: ");
    scanf("%d", &n);

    if (n < 0)
    {
        printf("Factorial is not defined for negative numbers.\n");
        return 1;
    }

    printf("\nUsing RECURSION : %d! = %ld\n", n, factRecursive(n));
    printf("Using ITERATION : %d! = %ld\n", n, factIterative(n));

    return 0;
}
```

### 5. Line-by-Line Explanation

| Code | What it does |
|---|---|
| `long factRecursive(int n)` | Returns n! by **calling itself** with a smaller argument |
| `if (n == 0) return 1;` | **Base case** — 0! = 1; stops the recursion |
| `return n * factRecursive(n - 1);` | **Recursive case** — n! = n × (n−1)! |
| `long fact = 1;` | Start the product at 1 (multiplicative identity) |
| `for (i = 2; i <= n; i++) fact *= i;` | Multiplies 2×3×…×n; starting at 2 skips the ×1 |
| `if (n < 0)` guard | Negative factorial is mathematically undefined |
| `%ld` | Format specifier for `long` |

### 6. Dry Run

**Recursion trace for n = 5:**
```
fact(5) = 5 * fact(4)
        = 5 * 4 * fact(3)
        = 5 * 4 * 3 * fact(2)
        = 5 * 4 * 3 * 2 * fact(1)
        = 5 * 4 * 3 * 2 * 1 * fact(0)
        = 5 * 4 * 3 * 2 * 1 * 1
        = 120
```

**Iteration for n = 5:**

| i | fact before | fact after |
|---|---|---|
| 2 | 1 | 2 |
| 3 | 2 | 6 |
| 4 | 6 | 24 |
| 5 | 24 | 120 |

Result: **120** ✓

### 7. Sample Input / Output

**Test 1:**
```
Enter a number: 5

Using RECURSION : 5! = 120
Using ITERATION : 5! = 120
```

**Test 2:**
```
Enter a number: 0

Using RECURSION : 0! = 1
Using ITERATION : 0! = 1
```

**Test 3:**
```
Enter a number: 10

Using RECURSION : 10! = 3628800
Using ITERATION : 10! = 3628800
```

**Test 4 (negative):**
```
Enter a number: -3
Factorial is not defined for negative numbers.
```

### 8. Edge Cases Table

| n | n! | Remark |
|---|---|---|
| 0 | 1 | Base case — by definition |
| 1 | 1 | 1! = 1 |
| 5 | 120 | Normal |
| 10 | 3,628,800 | Fine for `long` |
| 20 | 2,432,902,008,176,640,000 | Needs `long long` / `unsigned long long` |
| Negative | Undefined | Guarded in main |

> **Overflow warning:** 20! ≈ 2.4 × 10¹⁸. `long` is 64-bit on most modern systems (~9.2 × 10¹⁸ max), so up to 20! fits. Beyond that use `long long` or `double`. The `long` return type used above handles up to 20 comfortably on 64-bit compilers.

### 9. Common Mistakes to Avoid

1. **Missing the base case** — recursion without `n == 0` never terminates → **stack overflow**.
2. **Using `int` for the result** — 13! ≈ 6.2 billion overflows 32-bit `int` (max ~2.1 billion). Use `long`/`long long`.
3. **Starting the loop at 1** — harmless (×1), but starting at 2 is one fewer iteration.
4. **Not guarding negative input** — mathematically invalid; the program prints nonsense or loops forever for negative recursion.
5. **Writing `n * factRecursive(n)` (no decrement)** — infinite recursion; must be `n-1`.
6. **Confusing `%d` with `%ld`** — printing a `long` with `%d` is undefined behaviour on some platforms.

### 10. Recursion vs Iteration — Comparison Table

| Aspect | Recursion | Iteration |
|---|---|---|
| Implementation | Function calls itself | Loop with a product variable |
| Base case | `n == 0 → 1` | Loop simply stops |
| Readability | Mirrors the math formula exactly | Requires more code |
| Stack usage | n+1 stack frames | Constant |
| Risk | Stack overflow for very large n | None |
| Performance | Similar for small n (some overhead) | Slightly faster |

### 11. Alternative — `long long` version for larger numbers

```c
#include <stdio.h>

long long factRecursive(int n)
{
    if (n == 0)
        return 1;
    return n * factRecursive(n - 1);
}

long long factIterative(int n)
{
    long long fact = 1;
    int i;
    for (i = 2; i <= n; i++)
        fact *= i;
    return fact;
}

int main()
{
    int n;
    printf("Enter a number: ");
    scanf("%d", &n);

    if (n < 0)
    {
        printf("Factorial is not defined for negative numbers.\n");
        return 1;
    }

    printf("\nUsing RECURSION : %d! = %lld\n", n, factRecursive(n));
    printf("Using ITERATION : %d! = %lld\n", n, factIterative(n));
    return 0;
}
```

### 12. Brief Result Table (for the lab record)

| n | n! |
|---|---|
| 0 | 1 |
| 1 | 1 |
| 5 | 120 |
| 6 | 720 |
| 10 | 3628800 |

---
