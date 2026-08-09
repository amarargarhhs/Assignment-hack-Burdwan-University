# 2. Write a Program to Reverse a Given Number

### 1. Problem Understanding

Reverse the digits of an integer:

- Input **1234** → Output **4321**
- Input **98765** → Output **56789**

The technique extracts the last digit repeatedly and **builds the reversed number**:

```
reverse = reverse * 10 + last_digit
```

- Extract last digit: `n % 10`
- Remove last digit: `n / 10`

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| Modulus `%` | Extracts the last digit |
| Integer division `/` | Removes the last digit |
| Accumulator | `reverse` is built digit by digit |
| Multiplication by 10 | Shifts `reverse` left, making room for the next digit |

### 3. Algorithm (Step-by-Step)

1. Start.
2. Read the number `n`.
3. Set `reverse = 0`.
4. **While `n != 0`:**
   - `digit = n % 10`
   - `reverse = reverse * 10 + digit`
   - `n = n / 10`
5. Print `reverse`.
6. Stop.

### 4. The C Program (Source Code)

```c
#include <stdio.h>

int main()
{
    int n, digit, reverse = 0;

    printf("Enter a number: ");
    scanf("%d", &n);

    while (n != 0)
    {
        digit = n % 10;
        reverse = reverse * 10 + digit;
        n = n / 10;
    }

    printf("Reversed number = %d\n", reverse);

    return 0;
}
```

### 5. Line-by-Line Explanation

| Code | What it does |
|---|---|
| `int n, digit, reverse = 0;` | `reverse` accumulates the result |
| `while (n != 0)` | Loop until no digits left |
| `digit = n % 10;` | Gets the last digit |
| `reverse = reverse * 10 + digit;` | Appends the digit: 0 → 4 → 43 → 432 → 4321 |
| `n = n / 10;` | Drops the last digit |

### 6. Dry Run

**Input: n = 1234**

| Iteration | n (before) | digit | reverse | n (after) |
|---|---|---|---|---|
| 1 | 1234 | 4 | 4 | 123 |
| 2 | 123 | 3 | 43 | 12 |
| 3 | 12 | 2 | 432 | 1 |
| 4 | 1 | 1 | 4321 | 0 |

**Result: 4321** ✓

### 7. Sample Input / Output

**Test 1:**
```
Enter a number: 1234
Reversed number = 4321
```

**Test 2:**
```
Enter a number: 98765
Reversed number = 56789
```

### 8. Edge Cases Table

| Input | Output | Remark |
|---|---|---|
| 0 | 0 | Loop never runs; reverse stays 0 |
| 5 | 5 | Single digit — same reversed |
| 1000 | 1 | Leading zeros vanish (1 is correct as integer) |
| 12321 | 12321 | Palindrome — same reversed |
| −123 | −321 (most compilers) | `-123 % 10 = -3`; sign handled naturally by `%` |

### 9. Common Mistakes to Avoid

1. **Forgetting `n = n / 10;`** — infinite loop.
2. **Printing digits as you extract them** — that prints "4321" in order which looks right, but it is NOT the reversed *number*; building `reverse` properly is the standard approach.
3. **Overflow for very large numbers** — `reverse` can overflow `int`; use `long` for big inputs.
4. **`reverse` not initialised** — garbage accumulation.

### 10. Alternative — printing digits directly (string-like approach)

```c
#include <stdio.h>

int main()
{
    int n, digit;
    printf("Enter a number: ");
    scanf("%d", &n);

    printf("Reversed number = ");
    while (n != 0)
    {
        digit = n % 10;
        printf("%d", digit);
        n = n / 10;
    }
    printf("\n");
    return 0;
}
```

### 11. Brief Result Table (for the lab record)

| Input | Reversed |
|---|---|
| 1234 | 4321 |
| 98765 | 56789 |
| 0 | 0 |
| 1000 | 1 |

---
