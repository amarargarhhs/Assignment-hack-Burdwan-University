# 8. Write a Macro That Swaps Two Numbers. Write a Program to Use It.

### 1. Problem Understanding

A **macro** in C is a preprocessor directive — a piece of code defined with `#define` that gets **textually replaced** by the preprocessor before the compiler even runs. Here we define a macro that swaps two numbers, and then a complete program that **uses** that macro.

**Why a macro instead of a function?**
- Macros are expanded **inline** — no function-call overhead.
- Macros work for **any data type** (int, float, char, …) because the swap is done at the call site with whatever type you pass.
- The preprocessor performs pure **text substitution**.

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| `#define` macro | `#define SWAP(a,b) ...` — textual replacement |
| Temporary variable | The classic 3-line swap: `t = a; a = b; b = t;` |
| Preprocessor phase | Macro expansion happens *before* compilation |
| `do { } while(0)` wrapper | Wraps multi-statement macros so they behave safely like a single statement |

### 3. Algorithm (Step-by-Step)

1. Define the macro `SWAP(a, b)`:
   - Declare a temporary variable `t`.
   - `t = a;` → `a = b;` → `b = t;`
2. In `main()`:
   - Read two numbers.
   - Print them before swap.
   - Call `SWAP(x, y)`.
   - Print them after swap (values should now be exchanged).
3. Stop.

### 4. The C Program (Source Code)

```c
#include <stdio.h>

/* macro definition to swap two numbers */
#define SWAP(a, b)  do { \
                        int t = a; \
                        a = b; \
                        b = t; \
                    } while (0)

int main()
{
    int x, y;

    printf("Enter two numbers: ");
    scanf("%d %d", &x, &y);

    printf("Before swap: x = %d, y = %d\n", x, y);

    SWAP(x, y);            /* calling the macro */

    printf("After swap : x = %d, y = %d\n", x, y);

    return 0;
}
```

**Simpler macro form (often used in lab records):**
```c
#define SWAP(a, b)  { int t = a; a = b; b = t; }
```

> The `do { … } while(0)` wrapper is the **best practice**. It makes the macro a single logical statement, so it can be safely used inside an `if` without a trailing `;` breaking the `else` branch (see Common Mistakes, point 4).

### 5. Line-by-Line Explanation

| Code | What it does |
|---|---|
| `#define SWAP(a, b)` | Defines a macro named `SWAP` taking two arguments `a` and `b` |
| `do { … } while(0)` | Groups the swap statements so the macro behaves like one statement |
| `int t = a;` | Temporary variable holds the first value |
| `a = b;` | Second value goes into the first variable |
| `b = t;` | Saved original first value goes into the second variable |
| `SWAP(x, y);` | Preprocessor replaces this line with the swap code (with `a`→`x`, `b`→`y`) |
| `printf("After swap ...")` | Proves the values were exchanged |

### 6. How the Macro Is Expanded (Preprocessor View)

Before compilation, the line:
```c
SWAP(x, y);
```
becomes (textually):
```c
do { int t = x; x = y; y = t; } while (0);
```

This is why macros can be dangerous — the substitution is pure text, so `t` is declared fresh inside the block and the whole thing executes as a single statement.

### 7. Dry Run

**Input: x = 5, y = 9**

| Step | x | y | t |
|---|---|---|---|
| After input | 5 | 9 | — |
| `int t = x` → t = 5 | 5 | 9 | 5 |
| `x = y` → x = 9 | 9 | 9 | 5 |
| `y = t` → y = 5 | 9 | 5 | 5 |

Final: **x = 9, y = 5** — swapped ✓

### 8. Sample Input / Output

**Test 1:**
```
Enter two numbers: 5 9
Before swap: x = 5, y = 9
After swap : x = 9, y = 5
```

**Test 2:**
```
Enter two numbers: 100 200
Before swap: x = 100, y = 200
After swap : x = 200, y = 100
```

### 9. Edge Cases & Special Notes

| Case | Behaviour |
|---|---|
| Same values (e.g., 7 7) | After swap they remain 7 7 — no visible change, but logically correct |
| Negative numbers | Works fine (e.g., −3 ↔ 5) |
| Float/double/char | The macro swaps whatever type is passed at the call site |
| Swapping with itself `SWAP(x, x)` | t = x, x = x, x = t → value unchanged — harmless |
| Side-effect arguments (e.g., `SWAP(x++, y)`) | **Danger**: `x++` is evaluated multiple times because of textual substitution |

### 10. Common Mistakes to Avoid

1. **Missing backslash `\`** after each macro line — the preprocessor treats a newline as the end of the macro; each continuing line *must* end with `\`.
2. **No space issues** — a macro is defined by the *first* token after the name; `#define SWAP (a,b)` would make `SWAP` expand to nothing and `(a,b)` become code. Correct form: `#define SWAP(a,b)`.
3. **Forgetting the temporary variable** — `a = b; b = a;` loses the original value of `a` (both end up equal to old `b`).
4. **Using a bare `{ … }` macro inside `if/else`**:
   ```c
   if (condition)
       SWAP(x, y);      /* expands to { ... };  */
   else
       ...
   ```
   The `;` after the block creates an **empty statement**, and the `else` becomes a syntax error. The `do { … } while(0)` wrapper avoids this.
5. **Defining `t` globally or statically** — declaring the temp inside the macro block (as shown) keeps it safe and reentrant.
6. **Thinking a macro is a function** — macros have **no type checking** and are replaced textually; a function would need `void swap(int*, int*)`.

### 11. Alternative Approaches

**Macro using a global/static temp (older style):**
```c
#define SWAP(a, b)  { int t; t = a; a = b; b = t; }
```

**Function-based swap (for comparison — needs pointers):**
```c
#include <stdio.h>

void swap(int *a, int *b)
{
    int t = *a;
    *a = *b;
    *b = t;
}

int main()
{
    int x, y;
    printf("Enter two numbers: ");
    scanf("%d %d", &x, &y);
    printf("Before swap: x = %d, y = %d\n", x, y);
    swap(&x, &y);
    printf("After swap : x = %d, y = %d\n", x, y);
    return 0;
}
```

**Macro with arguments of any type — pointer-based (most robust):**
```c
#define SWAP(a, b)  do { typeof(a) t = a; a = b; b = t; } while (0)
```
*(`typeof` is a GNU extension — works with gcc/clang, not standard C.)*

### 12. Brief Result Table (for the lab record)

| Input | Before swap | After swap |
|---|---|---|
| 5 9 | x=5, y=9 | x=9, y=5 |
| 100 200 | x=100, y=200 | x=200, y=100 |
| −3 7 | x=−3, y=7 | x=7, y=−3 |

---
