# 18. WAP to Display Fibonacci Series (i) Using Recursion, (ii) Using Iteration

### 1. Problem Understanding

The **Fibonacci series** is defined by:
```
F(0) = 0
F(1) = 1
F(n) = F(n-1) + F(n-2)   for n >= 2
```

So the series is: **0, 1, 1, 2, 3, 5, 8, 13, 21, 34, …**

The assignment requires **two implementations:**
1. **Recursion** — the function calls itself: `fib(n) = fib(n-1) + fib(n-2)`.
2. **Iteration** — a loop with two running variables (`a` and `b`).

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| Base case | `n == 0` → 0, `n == 1` → 1 (stops recursion) |
| Recursive case | `fib(n-1) + fib(n-2)` |
| Iteration | `a = b; b = c;` — shifting the last two values |
| Return value | Each version has a `fib(n)` function returning the n-th term |

### 3. Algorithm (Step-by-Step)

**Recursion — `fib(n)`:**
1. If `n == 0` → return 0.
2. If `n == 1` → return 1.
3. Else return `fib(n-1) + fib(n-2)`.

**Iteration — `fib(n)`:**
1. If `n == 0` → 0; if `n == 1` → 1.
2. Set `a = 0, b = 1`.
3. Loop `i = 2 … n`: `c = a + b; a = b; b = c;`
4. Return `b`.

**main() for both:** read how many terms `n`, print F(0)…F(n-1) by calling `fib(i)` in a loop.

### 4. The C Program (Source Code)

```c
#include <stdio.h>

/* ---------- (i) FIBONACCI USING RECURSION ---------- */
int fibRecursive(int n)
{
    if (n == 0)
        return 0;
    if (n == 1)
        return 1;
    return fibRecursive(n - 1) + fibRecursive(n - 2);
}

/* ---------- (ii) FIBONACCI USING ITERATION ---------- */
int fibIterative(int n)
{
    int a = 0, b = 1, c, i;

    if (n == 0)
        return 0;
    if (n == 1)
        return 1;

    for (i = 2; i <= n; i++)
    {
        c = a + b;      /* next term */
        a = b;          /* shift window */
        b = c;
    }
    return b;
}

int main()
{
    int n, i;

    printf("How many terms of the Fibonacci series? ");
    scanf("%d", &n);

    /* using recursion */
    printf("\nFibonacci series using RECURSION :\n");
    for (i = 0; i < n; i++)
        printf("%d ", fibRecursive(i));
    printf("\n");

    /* using iteration */
    printf("\nFibonacci series using ITERATION :\n");
    for (i = 0; i < n; i++)
        printf("%d ", fibIterative(i));
    printf("\n");

    return 0;
}
```

### 5. Line-by-Line Explanation

| Code | What it does |
|---|---|
| `int fibRecursive(int n)` | Returns the n-th Fibonacci number by **calling itself** |
| `if (n == 0) return 0;` / `if (n == 1) return 1;` | **Base cases** — without these, recursion never stops (stack overflow) |
| `return fibRecursive(n-1) + fibRecursive(n-2);` | **Recursive case** — breaks the problem into two smaller ones |
| `int fibIterative(int n)` | Same result, but computed with a **loop** |
| `a = 0, b = 1` | Seeds for F(0) and F(1) |
| `c = a + b; a = b; b = c;` | Computes the next term and slides the window forward |
| `for (i = 0; i < n; i++) printf("%d ", fib(i));` | Prints terms F(0) to F(n−1) |

### 6. Dry Run

**Iteration, n = 6 (print F(0)…F(5)):**

For each call `fibIterative(i)`:

| i | a | b | c | loop steps | returns |
|---|---|---|---|---|---|
| 0 | — | — | — | (base) | 0 |
| 1 | — | — | — | (base) | 1 |
| 2 | 0 | 1 | 1 | 1 step | 1 |
| 3 | 0 | 1 | 1 | 2 steps: c=1,a=1,b=1 → c=2,a=1,b=2 | 2 |
| 4 | 0 | 1 | … | → b becomes 3 | 3 |
| 5 | 0 | 1 | … | → b becomes 5 | 5 |

**Recursion trace for `fibRecursive(4)`:**
```
fib(4)
 ├─ fib(3)             ├─ fib(2)
 │   ├─ fib(2)  =1       │   ├─ fib(1)=1
 │   │   ├─ fib(1)=1     │   └─ fib(0)=0
 │   │   └─ fib(0)=0     └── =1
 │   └─ fib(1)  =1
 └─ fib(2)
     ├─ fib(1)=1
     └─ fib(0)=0        =1
fib(4) = fib(3)+fib(2) = 2 + 1 = 3  ✓
```

### 7. Sample Input / Output

**Test 1:**
```
How many terms of the Fibonacci series? 8

Fibonacci series using RECURSION :
0 1 1 2 3 5 8 13

Fibonacci series using ITERATION :
0 1 1 2 3 5 8 13
```

**Test 2:**
```
How many terms of the Fibonacci series? 12

Fibonacci series using RECURSION :
0 1 1 2 3 5 8 13 21 34 55 89

Fibonacci series using ITERATION :
0 1 1 2 3 5 8 13 21 34 55 89
```

### 8. Edge Cases Table

| n (terms) | Series | Remark |
|---|---|---|
| 1 | 0 | Base case only |
| 2 | 0 1 | Both base cases |
| 8 | 0 1 1 2 3 5 8 13 | Normal |
| 0 | (nothing printed) | Loop never runs |

> **Caution:** both versions use `int`; Fibonacci grows fast. F(46) ≈ 1.8 billion, which overflows `int` (max ≈ 2.1 billion). For more terms use `long`/`long long`.

### 9. Common Mistakes to Avoid

1. **Missing base cases in recursion** — infinite recursion → **stack overflow** crash.
2. **Using `n` as the number of terms but starting at 1** — series must start at F(0)=0; printing `fib(1)…fib(n)` drops the first term.
3. **Wrong iteration order** — `a = b; b = c;` must use the *old* `b`; swapping the statements breaks the series.
4. **`int` overflow for large n** — use `long long` or limit the terms.
5. **Recursion without a return value** — `fib(n-1) + fib(n-2)` requires the function to *return* a value.
6. **Forgetting to reset variables** if the function is called in a loop — each call must start from `a=0,b=1`.

### 10. Recursion vs Iteration — Comparison Table

| Aspect | Recursion | Iteration |
|---|---|---|
| Implementation | Function calls itself | Loop with variables |
| Base case | `n==0`, `n==1` | Returns directly |
| Readability | Very close to math definition | Slightly more code |
| Performance | **Slow** — computes same sub-problems repeatedly (O(2ⁿ)) | **Fast** — one pass (O(n)) |
| Memory | Each call uses stack space | Constant O(1) extra |
| Risk | Stack overflow for large n | None |

> This is why the recursive version is fine for a *demonstration*, but the iterative one is used in practice.

### 11. Alternative — printing the series directly without repeated function calls

**Iterative version that prints as it computes (most efficient):**
```c
#include <stdio.h>

int main()
{
    int n, i;
    long a = 0, b = 1, c;

    printf("How many terms? ");
    scanf("%d", &n);

    printf("Fibonacci series: ");
    for (i = 0; i < n; i++)
    {
        printf("%ld ", a);
        c = a + b;
        a = b;
        b = c;
    }
    printf("\n");
    return 0;
}
```

**Recursive version that returns `long long`:**
```c
long long fibRecursive(int n)
{
    if (n == 0) return 0;
    if (n == 1) return 1;
    return fibRecursive(n - 1) + fibRecursive(n - 2);
}
```

### 12. Brief Result Table (for the lab record)

| Number of terms | Series |
|---|---|
| 8 | 0 1 1 2 3 5 8 13 |
| 10 | 0 1 1 2 3 5 8 13 21 34 |
| 12 | 0 1 1 2 3 5 8 13 21 34 55 89 |

---
