# 20. WAP to Calculate GCD of Two Numbers (i) With Recursion, (ii) Without Recursion

### 1. Problem Understanding

The **GCD (Greatest Common Divisor)** of two numbers is the largest positive integer that divides both of them **exactly**. 

- GCD(12, 18) = 6 (common divisors: 1, 2, 3, 6)
- GCD(15, 25) = 5
- GCD(7, 13) = 1 (coprime/relatively prime)

**Euclid's algorithm** is the standard method:
```
gcd(a, b) = gcd(b, a % b)      while b ≠ 0
gcd(a, 0) = a
```

The assignment needs **two implementations:**
1. **Recursion** — the function calls itself using `gcd(b, a % b)`.
2. **Iteration (Euclid's algorithm with a loop)** — repeatedly replace `(a, b)` with `(b, a % b)` until `b == 0`.

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| Euclid's algorithm | Repeated remainder: `gcd(a,b) = gcd(b, a%b)` |
| Base case | `b == 0` → answer is `a` |
| Modulus operator `%` | Gives the remainder needed by Euclid |
| Recursive case | `gcd(b, a % b)` |
| Iterative loop | `while (b != 0) { t = b; b = a % b; a = t; }` |
| Swapping handles a < b | `gcd(15, 25)` automatically becomes `gcd(25, 15)` via `%` |

### 3. Algorithm (Step-by-Step)

**Recursion — `gcdRecursive(a, b)`:**
1. If `b == 0` → return `a`.
2. Else return `gcdRecursive(b, a % b)`.

**Iteration — `gcdIterative(a, b)`:**
1. `while (b != 0):`
   - `t = b;`
   - `b = a % b;`
   - `a = t;`
2. Return `a`.

**main():** read two numbers, call both functions, print results.

### 4. The C Program (Source Code)

```c
#include <stdio.h>

/* ---------- (i) GCD USING RECURSION ---------- */
int gcdRecursive(int a, int b)
{
    if (b == 0)
        return a;                 /* base case */
    return gcdRecursive(b, a % b); /* Euclid's rule */
}

/* ---------- (ii) GCD WITHOUT RECURSION (iteration) ---------- */
int gcdIterative(int a, int b)
{
    int t;

    while (b != 0)
    {
        t = b;
        b = a % b;
        a = t;
    }
    return a;
}

int main()
{
    int x, y;

    printf("Enter two numbers: ");
    scanf("%d %d", &x, &y);

    if (x == 0 && y == 0)
    {
        printf("GCD of 0 and 0 is undefined.\n");
        return 1;
    }

    if (x < 0) x = -x;            /* GCD works on absolute values */
    if (y < 0) y = -y;

    printf("\nGCD using RECURSION : gcd(%d, %d) = %d\n", x, y, gcdRecursive(x, y));
    printf("GCD using ITERATION : gcd(%d, %d) = %d\n", x, y, gcdIterative(x, y));

    return 0;
}
```

### 5. Line-by-Line Explanation

| Code | What it does |
|---|---|
| `int gcdRecursive(int a, int b)` | Returns GCD by **calling itself** with the remainder |
| `if (b == 0) return a;` | **Base case** — when remainder hits 0, `a` is the GCD |
| `return gcdRecursive(b, a % b);` | **Recursive case** — Euclid: gcd(a,b) = gcd(b, a%b) |
| `int gcdIterative(int a, int b)` | Same logic, but as a **loop** |
| `t = b; b = a % b; a = t;` | Steps the pair (a,b) forward: old b becomes new a, remainder becomes new b |
| `while (b != 0)` | Loop until the remainder becomes zero |
| `if (x < 0) x = -x;` | GCD is defined only for positive values; take absolute values |

### 6. Dry Run

**GCD(48, 36)**

**Recursion trace:**
```
gcd(48, 36)
 = gcd(36, 48 % 36 = 12)
 = gcd(12, 36 % 12 = 0)
 = 12
```

**Iteration trace:**

| a | b | t | b = a % b |
|---|---|---|---|
| 48 | 36 | 36 | 12 |
| 36 | 12 | 12 | 0 |
| 12 | 0 | — | loop ends |

Result: **12** ✓

**Also note the swap effect — GCD(15, 25):**
```
gcd(15, 25) = gcd(25, 15) = gcd(15, 10) = gcd(10, 5) = gcd(5, 0) = 5
```
The first `%` automatically swaps by making `gcd(b, a%b)`.

### 7. Sample Input / Output

**Test 1:**
```
Enter two numbers: 48 36

GCD using RECURSION : gcd(48, 36) = 12
GCD using ITERATION : gcd(48, 36) = 12
```

**Test 2:**
```
Enter two numbers: 7 13

GCD using RECURSION : gcd(7, 13) = 1
GCD using ITERATION : gcd(7, 13) = 1
```

**Test 3 (with zero):**
```
Enter two numbers: 0 25

GCD using RECURSION : gcd(0, 25) = 25
GCD using ITERATION : gcd(0, 25) = 25
```

### 8. Edge Cases Table

| Input (a, b) | GCD | Remark |
|---|---|---|
| 48, 36 | 12 | Normal case |
| 7, 13 | 1 | Coprime numbers |
| 0, 25 | 25 | `gcd(0, n) = n` (base case handles it) |
| 25, 0 | 25 | Loop never runs, returns 25 |
| 0, 0 | undefined | Guarded in main |
| −12, 18 | 6 | Handled by absolute-value conversion |
| 12, 12 | 12 | Equal numbers |

### 9. Common Mistakes to Avoid

1. **Reversing the remainder order** — Euclid requires `a % b`, not `b % a` (with the recursion on `(b, a%b)`).
2. **Missing base case in recursion** — never terminates → stack overflow.
3. **Using `a %= b; b %= a;` in iteration** — destructive; must use a temporary to preserve old `b`.
4. **Forgetting GCD of (0, n) is n** — the `b == 0` check handles this only if it's placed before any modulo.
5. **Not handling negatives** — modulo of negatives behaves differently across C compilers; convert to absolute values.
6. **`gcd(0,0)`** — returning 0 is mathematically wrong (undefined); guard it.

### 10. Recursion vs Iteration — Comparison Table

| Aspect | Recursion | Iteration |
|---|---|---|
| Implementation | Calls itself with `(b, a%b)` | `while` loop stepping (a,b) |
| Base case | `b == 0` | Loop condition `b != 0` |
| Readability | Matches Euclid exactly | Slightly more verbose |
| Stack usage | O(log n) frames | Constant |
| Performance | Equivalent (both O(log min(a,b))) | No call overhead |

### 11. Alternative — naive method (both divisors checked, for understanding)

```c
#include <stdio.h>

int gcdNaive(int a, int b)
{
    int i, gcd = 1;
    for (i = 1; i <= a && i <= b; i++)
    {
        if (a % i == 0 && b % i == 0)
            gcd = i;
    }
    return gcd;
}
```
*(Slower — O(min(a,b)) — but very intuitive; good to contrast with Euclid.)*

**Recursive one-liner:**
```c
int gcd(int a, int b)
{
    return (b == 0) ? a : gcd(b, a % b);
}
```

### 12. Brief Result Table (for the lab record)

| a | b | GCD |
|---|---|---|
| 48 | 36 | 12 |
| 15 | 25 | 5 |
| 7 | 13 | 1 |
| 0 | 25 | 25 |
| 12 | 12 | 12 |

---
