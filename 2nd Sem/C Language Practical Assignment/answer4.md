# 4. Write a Program to Find the Sum of First n Terms of the Series S = 1 - 2 + 3 - 4 + 5 - ...

### 1. Problem Understanding

The series alternates in sign, odd terms are added, even terms are subtracted:

```
S = 1 - 2 + 3 - 4 + 5 - 6 + ...
```

For `n = 6`: `S = 1 - 2 + 3 - 4 + 5 - 6 = -3`

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| Alternating signs | Odd i → add; even i → subtract |
| Modulus check | `i % 2 == 1` → odd, `i % 2 == 0` → even |
| Loop | Accumulate each signed term |

### 3. Algorithm (Step-by-Step)

1. Start.
2. Read `n`.
3. Set `sum = 0`.
4. **For `i = 1; i <= n; i++`:**
   - If `i` is odd → `sum = sum + i`.
   - Else → `sum = sum - i`.
5. Print `sum`.
6. Stop.

### 4. The C Program (Source Code)

```c
#include <stdio.h>

int main()
{
    int n, i, sum = 0;

    printf("Enter the value of n: ");
    scanf("%d", &n);

    for (i = 1; i <= n; i++)
    {
        if (i % 2 == 1)      /* odd term -> add */
            sum = sum + i;
        else                 /* even term -> subtract */
            sum = sum - i;
    }

    printf("Sum of first %d terms = %d\n", n, sum);

    return 0;
}
```

### 5. Line-by-Line Explanation

| Code | What it does |
|---|---|
| `int sum = 0;` | Accumulator (this series stays an integer) |
| `for (i = 1; i <= n; i++)` | Runs for each term |
| `if (i % 2 == 1)` | Detects odd terms |
| `sum = sum + i;` / `sum = sum - i;` | Adds odd, subtracts even |

### 6. Dry Run

**n = 6:**

| i | odd/even | operation | sum |
|---|---|---|---|
| 1 | odd | +1 | 1 |
| 2 | even | −2 | −1 |
| 3 | odd | +3 | 2 |
| 4 | even | −4 | −2 |
| 5 | odd | +5 | 3 |
| 6 | even | −6 | −3 |

**Result: −3** ✓

### 7. Sample Input / Output

**Test 1:**
```
Enter the value of n: 6
Sum of first 6 terms = -3
```

**Test 2:**
```
Enter the value of n: 5
Sum of first 5 terms = 3
```

### 8. Edge Cases Table

| n | Sum | Closed form |
|---|---|---|
| 1 | 1 | (1+1)/2 = 1 |
| 2 | −1 | −2/2 = −1 |
| 5 | 3 | (5+1)/2 = 3 |
| 6 | −3 | −6/2 = −3 |
| 0 | 0 | Loop never runs |

> **Closed form:** if n is even → `-n/2`; if n is odd → `(n+1)/2`.

### 9. Common Mistakes to Avoid

1. **Wrong parity condition** — `i % 2 == 0` subtracts odd terms instead.
2. **Forgetting the sign changes** — adding every term gives the plain sum 1+2+3+...
3. **Off-by-one in the loop range** — must run to `n`, not `n-1`.

### 10. Alternative Version — using a sign variable

```c
#include <stdio.h>

int main()
{
    int n, i, sum = 0, sign = 1;

    printf("Enter the value of n: ");
    scanf("%d", &n);

    for (i = 1; i <= n; i++)
    {
        sum = sum + sign * i;
        sign = -sign;        /* flip sign each iteration */
    }

    printf("Sum of first %d terms = %d\n", n, sum);
    return 0;
}
```

### 11. Brief Result Table (for the lab record)

| n | Sum |
|---|---|
| 5 | 3 |
| 6 | −3 |
| 10 | −5 |

---
