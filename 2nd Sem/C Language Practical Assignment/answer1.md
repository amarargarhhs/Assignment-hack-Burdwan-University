# 1. Write a Program to Calculate Sum and Product of Digits of an Integer

### 1. Problem Understanding

Given an integer (e.g., 1234), we must:

- **Sum** all its digits: 1 + 2 + 3 + 4 = **10**
- **Product** of all its digits: 1 × 2 × 3 × 4 = **24**

The digits are extracted one by one using the **modulus** and **division** operators:

- `n % 10` gives the **last digit**.
- `n / 10` removes the last digit (integer division).

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| Modulus operator `%` | `n % 10` extracts the last digit |
| Integer division `/` | `n / 10` drops the last digit |
| Loop | Repeat until `n` becomes 0 |
| Accumulators | `sum` and `product` variables collect results |
| Initialisation | `sum = 0`, `product = 1` (multiply identity) |

### 3. Algorithm (Step-by-Step)

1. Start.
2. Read the integer `n`.
3. Initialise `sum = 0` and `product = 1`.
4. **While `n != 0`:**
   - `digit = n % 10` (extract last digit).
   - `sum = sum + digit`.
   - `product = product * digit`.
   - `n = n / 10` (remove the last digit).
5. Print `sum` and `product`.
6. Stop.

### 4. The C Program (Source Code)

```c
#include <stdio.h>

int main()
{
    int n, digit, sum = 0, product = 1;

    printf("Enter an integer: ");
    scanf("%d", &n);

    while (n != 0)
    {
        digit = n % 10;      /* last digit */
        sum = sum + digit;
        product = product * digit;
        n = n / 10;          /* remove last digit */
    }

    printf("Sum of digits = %d\n", sum);
    printf("Product of digits = %d\n", product);

    return 0;
}
```

### 5. Line-by-Line Explanation

| Code | What it does |
|---|---|
| `int n, digit, sum = 0, product = 1;` | `sum` starts at 0, `product` at 1 |
| `scanf("%d", &n)` | Reads the integer — `&n` gives its address |
| `while (n != 0)` | Loop until no digits remain |
| `digit = n % 10;` | Extracts the last digit |
| `sum = sum + digit;` | Adds the digit to the running sum |
| `product = product * digit;` | Multiplies the digit into the running product |
| `n = n / 10;` | Removes the last digit (integer division) |

### 6. Dry Run

**Input: n = 1234**

| Iteration | n (before) | digit | sum | product | n (after) |
|---|---|---|---|---|---|
| 1 | 1234 | 4 | 4 | 4 | 123 |
| 2 | 123 | 3 | 7 | 12 | 12 |
| 3 | 12 | 2 | 9 | 24 | 1 |
| 4 | 1 | 1 | 10 | 24 | 0 |

Loop ends (n = 0).

**Result: Sum = 10, Product = 24** ✓

### 7. Sample Input / Output

**Test 1:**
```
Enter an integer: 1234
Sum of digits = 10
Product of digits = 24
```

**Test 2:**
```
Enter an integer: 987
Sum of digits = 24
Product of digits = 504
```

### 8. Edge Cases Table

| Input | Sum | Product | Remark |
|---|---|---|---|
| 0 | 0 | 1 | Loop never runs; product stays 1 (mathematical convention) |
| 5 | 5 | 5 | Single digit |
| 1000 | 1 | 0 | Product becomes 0 immediately (0 × anything) |
| 12345 | 15 | 120 | Normal case |
| Negative | depends on `%` | depends on `%` | On most compilers `-123 % 10` is `-3`; handle sign separately if needed |

> **Note on input 0:** If the user enters 0, the loop body never executes, so `sum = 0` and `product = 1`. Strictly, the digit of 0 is 0, so the product should arguably be 0. You may handle `n == 0` specially.

### 9. Common Mistakes to Avoid

1. **Starting `product = 0`** — 0 × anything = 0, so the product would always be 0. It must start at **1**.
2. **Forgetting `n = n / 10;`** — infinite loop (n never changes).
3. **Using `%` on 0** — `0 % 10` is valid (= 0) but the loop never runs anyway.
4. **Reading with `scanf("%d", n)` (no `&`)** — garbage value read.
5. **Not handling input 0** — output is 0 and 1, which may surprise the examiner; special-case it.

### 10. Alternative Version — with input 0 handled

```c
#include <stdio.h>

int main()
{
    int n, digit, sum = 0, product = 1;

    printf("Enter an integer: ");
    scanf("%d", &n);

    if (n == 0)
    {
        printf("Sum of digits = 0\n");
        printf("Product of digits = 0\n");
        return 0;
    }

    while (n != 0)
    {
        digit = n % 10;
        sum += digit;
        product *= digit;
        n /= 10;
    }

    printf("Sum of digits = %d\n", sum);
    printf("Product of digits = %d\n", product);

    return 0;
}
```

### 11. Brief Result Table (for the lab record)

| Input | Sum of digits | Product of digits |
|---|---|---|
| 1234 | 10 | 24 |
| 987 | 24 | 504 |
| 5 | 5 | 5 |
| 1000 | 1 | 0 |

---
