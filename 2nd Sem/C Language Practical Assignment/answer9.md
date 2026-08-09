# 9. WAP to Print a Triangle of Stars (Take Number of Lines From User)

### 1. Problem Understanding

Print a **right-angled triangle of stars** where the user gives the number of lines. For example, with `n = 5`:

```
*
* *
* * *
* * * *
* * * * *
```

**Pattern logic:**
- **Outer loop** (rows) runs from `1` to `n`.
- **Inner loop** (columns) prints exactly `row` number of stars — row 1 prints 1 star, row 2 prints 2 stars, …, row n prints n stars.
- After each row, print a **newline**.

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| Nested loops | Outer loop = rows, inner loop = columns |
| Relationship `j <= i` | The inner loop's limit depends on the current row number |
| `printf("* ")` | Prints one star (with a space for readability) |
| `printf("\n")` | Moves to the next line after each row |

### 3. Algorithm (Step-by-Step)

1. Start.
2. Read `n` (number of lines) from the user.
3. **For `i = 1; i <= n; i++` (each row):**
   - **For `j = 1; j <= i; j++`:** print `* `
   - Print a newline `\n`.
4. Stop.

### 4. The C Program (Source Code)

```c
#include <stdio.h>

int main()
{
    int n, i, j;

    printf("Enter the number of lines: ");
    scanf("%d", &n);

    for (i = 1; i <= n; i++)          /* outer loop -> rows */
    {
        for (j = 1; j <= i; j++)      /* inner loop -> stars in this row */
            printf("* ");

        printf("\n");                 /* move to next line */
    }

    return 0;
}
```

### 5. Line-by-Line Explanation

| Code | What it does |
|---|---|
| `int n, i, j;` | `n` = number of rows, `i` = row counter, `j` = star counter |
| `scanf("%d", &n);` | Reads how many lines the triangle should have |
| `for (i = 1; i <= n; i++)` | Runs once per row; prints `n` rows total |
| `for (j = 1; j <= i; j++)` | Row `i` prints exactly `i` stars — `j` runs 1, 2, …, i |
| `printf("* ");` | Outputs one star and a space |
| `printf("\n");` | After finishing row `i`, drops to the next line |

### 6. Dry Run — n = 3

| i (row) | j runs (1 to i) | Stars printed | After row |
|---|---|---|---|
| 1 | 1 | `*` | `*` + newline |
| 2 | 1, 2 | `* *` | `* *` + newline |
| 3 | 1, 2, 3 | `* * *` | `* * *` + newline |

Output:
```
*
* *
* * *
```

### 7. Sample Input / Output

**Test 1:**
```
Enter the number of lines: 4
*
* *
* * *
* * * *
```

**Test 2:**
```
Enter the number of lines: 1
*
```

**Test 3:**
```
Enter the number of lines: 6
*
* *
* * *
* * * *
* * * * *
* * * * * *
```

### 8. Edge Cases Table

| Input n | Output | Remark |
|---|---|---|
| 4 | 4-row triangle | Normal case |
| 1 | single `*` | Minimum useful case |
| 0 | nothing printed | Loops don't run — acceptable |
| Negative | nothing printed | Loops don't run — acceptable |
| Large (e.g., 20) | 20 rows | Works, just long output |

### 9. Common Mistakes to Avoid

1. **Using `j <= n` in the inner loop** — that would print `n` stars in *every* row (a rectangle), not a triangle. Correct limit is `j <= i`.
2. **Reversing the loops** — outer loop must control rows; inner loop controls stars per row.
3. **Forgetting the newline** — all stars would print on one line.
4. **Starting both loops at 0** — with `i=0` the first row prints 0 stars, shifting the whole triangle up one row. Starting at 1 keeps row number = star count.
5. **Not using `&` in `scanf`** — reads garbage into `n`.

### 10. Pattern Variations (examiners often ask for these too)

**Left-aligned triangle (done above):**
```
*
* *
* * *
```

**Right-aligned triangle (spaces + stars):**
```c
for (i = 1; i <= n; i++)
{
    for (j = 1; j <= n - i; j++)      /* leading spaces */
        printf("  ");

    for (j = 1; j <= i; j++)          /* stars */
        printf("* ");

    printf("\n");
}
```
```
        *
      * *
    * * *
```

**Upside-down triangle:**
```c
for (i = n; i >= 1; i--)
{
    for (j = 1; j <= i; j++)
        printf("* ");
    printf("\n");
}
```
```
* * * *
* * *
* *
*
```

**Full pyramid (centered):**
```c
for (i = 1; i <= n; i++)
{
    for (j = 1; j <= n - i; j++)
        printf(" ");

    for (j = 1; j <= (2 * i - 1); j++)
        printf("*");

    printf("\n");
}
```
```
    *
   ***
  *****
 *******
*********
```

### 11. Brief Result Table (for the lab record)

| Number of lines (n) | Output structure |
|---|---|
| 4 | 4 rows; row r has r stars |
| 6 | 6 rows; row r has r stars |
| 1 | 1 star |

---
