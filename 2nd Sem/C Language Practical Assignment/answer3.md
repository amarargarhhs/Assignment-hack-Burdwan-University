# 3. Write a Program to Find the Sum of First n Terms of the Series S = 1 + 1/2 + 1/3 + 1/4 + ...

### 1. Problem Understanding

Find the sum of the first `n` terms of the **harmonic series**:

```
S = 1 + 1/2 + 1/3 + 1/4 + ... + 1/n
```

For `n = 5`: `S = 1 + 0.5 + 0.3333 + 0.25 + 0.2 = 2.283333`

**Critical pitfall:** in C, `1/2` is **0** (integer division). You must write `1.0 / i` to get a real fraction.

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| `float`/`double` | The sum is fractional — never use `int` for it |
| Integer vs real division | `1/i` → 0 (int); `1.0/i` → correct fraction |
| Loop | Accumulate `1.0/i` for i = 1 to n |

### 3. Algorithm (Step-by-Step)

1. Start.
2. Read `n`.
3. Set `sum = 0.0`.
4. **For `i = 1; i <= n; i++`:**
   - `sum = sum + 1.0 / i;`
5. Print `sum`.
6. Stop.

### 4. The C Program (Source Code)

```c
#include <stdio.h>

int main()
{
    int n, i;
    float sum = 0.0;

    printf("Enter the value of n: ");
    scanf("%d", &n);

    for (i = 1; i <= n; i++)
    {
        sum = sum + 1.0 / i;   /* 1.0 ensures real division */
    }

    printf("Sum of first %d terms = %.6f\n", n, sum);

    return 0;
}
```

### 5. Line-by-Line Explanation

| Code | What it does |
|---|---|
| `float sum = 0.0;` | Accumulator for the fractional sum |
| `for (i = 1; i <= n; i++)` | Runs for each denominator from 1 to n |
| `sum += 1.0 / i;` | `1.0/i` is a real division; `1/i` would truncate to 0 |

### 6. Dry Run

**n = 5:**

| i | 1.0/i | sum |
|---|---|---|
| 1 | 1.0 | 1.0 |
| 2 | 0.5 | 1.5 |
| 3 | 0.3333 | 1.8333 |
| 4 | 0.25 | 2.0833 |
| 5 | 0.2 | 2.2833 |

**Result: 2.283333** ✓

### 7. Sample Input / Output

**Test 1:**
```
Enter the value of n: 5
Sum of first 5 terms = 2.283333
```

**Test 2:**
```
Enter the value of n: 10
Sum of first 10 terms = 2.928968
```

### 8. Edge Cases Table

| n | Sum | Remark |
|---|---|---|
| 1 | 1.0 | First term only |
| 5 | 2.283333 | Normal |
| 10 | 2.928968 | Normal |
| 0 | 0.0 | Loop never runs |
| Very large n | ~ ln(n) + γ (Euler–Mascheroni constant ≈ 0.577) | Series diverges slowly |

### 9. Common Mistakes to Avoid

1. **Writing `1 / i` instead of `1.0 / i`** — integer division yields 0 for every term → sum = 0.
2. **Declaring `sum` as `int`** — decimal part lost.
3. **Starting the loop at 0** — `1.0/0` is division by zero (undefined/error).
4. **Using `double` but printing with `%f`** — works, but `%lf` is more correct for `double`.

### 10. Alternative Version — using `double`

```c
#include <stdio.h>

int main()
{
    int n, i;
    double sum = 0.0;

    printf("Enter the value of n: ");
    scanf("%d", &n);

    for (i = 1; i <= n; i++)
        sum += 1.0 / i;

    printf("Sum of first %d terms = %.6lf\n", n, sum);
    return 0;
}
```

### 11. Brief Result Table (for the lab record)

| n | Sum |
|---|---|
| 1 | 1.000000 |
| 5 | 2.283333 |
| 10 | 2.928968 |

---
