# 12. Write a Program That Swaps Two Numbers Using Pointers

### 1. Problem Understanding

Swap two numbers **using pointers** — the whole point is to demonstrate that a function can modify the caller's variables by passing their **addresses**. This is the classic C example of **call by reference** (emulated with pointers).

**Why pointers are needed:** In C, function arguments are passed **by value** (a copy). A plain `swap(x, y)` inside a function would swap the copies only, leaving the originals untouched. Passing **addresses** (`&x`, `&y`) lets the function write directly into the caller's memory.

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| Pointer declaration | `int *a` — a variable that holds an address of an `int` |
| Address-of operator | `&x` — gives the memory address of variable `x` |
| Dereference operator | `*a` — accesses the value stored at the address `a` points to |
| Call by reference | Passing `&x, &y` lets the function modify the originals |
| Temporary variable | Needed so the first assignment doesn't lose the original value |

### 3. Algorithm (Step-by-Step)

1. Start.
2. `main()` reads two numbers `x` and `y`.
3. Print "before swap".
4. Call `swap(&x, &y)` — passing **addresses**.
5. **Inside `swap(int *a, int *b)`:**
   - `t = *a;` (store the value at address `a`)
   - `*a = *b;` (write value at address `b` into address `a`)
   - `*b = t;` (write the saved value into address `b`)
6. Back in `main()`, print "after swap" — originals are now swapped.
7. Stop.

### 4. The C Program (Source Code)

```c
#include <stdio.h>

/* function that swaps two numbers using pointers */
void swap(int *a, int *b)
{
    int t;

    t = *a;        /* save value pointed by a */
    *a = *b;       /* copy value of b into a  */
    *b = t;        /* copy saved value into b */
}

int main()
{
    int x, y;

    printf("Enter two numbers: ");
    scanf("%d %d", &x, &y);

    printf("Before swap: x = %d, y = %d\n", x, y);

    swap(&x, &y);            /* pass addresses of x and y */

    printf("After swap : x = %d, y = %d\n", x, y);

    return 0;
}
```

### 5. Line-by-Line Explanation

| Code | What it does |
|---|---|
| `void swap(int *a, int *b)` | Function receives **addresses** of two integers — `a` and `b` are pointer variables |
| `int t;` | Temporary storage for the first value |
| `t = *a;` | Dereference `a` to *read* the value it points to and save it |
| `*a = *b;` | Dereference `b` to *read* its value and write it into the memory `a` points to |
| `*b = t;` | Put the saved original value into the memory `b` points to |
| `swap(&x, &y);` | Pass addresses — so `*a` inside the function IS the same memory as `x` in `main` |

### 6. Memory View (How the Pointers Work)

Suppose `x` is stored at address `2000` holding `5`, and `y` at `2004` holding `9`:

```
BEFORE CALL:              INSIDE swap(&x,&y):
x @2000 = 5   ──►  a = 2000   (a points to x)
y @2004 = 9   ──►  b = 2004   (b points to y)

Step 1: t = *a   → t = 5
Step 2: *a = *b → memory@2000 = 9   (x now 9)
Step 3: *b = t  → memory@2004 = 5   (y now 5)
```

The changes are made **directly in main's memory**, so after the call `x = 9, y = 5`. ✓

### 7. Dry Run

**Input: x = 5, y = 9**

| Step | t | x (memory @2000) | y (memory @2004) |
|---|---|---|---|
| Before call | — | 5 | 9 |
| `t = *a` | 5 | 5 | 9 |
| `*a = *b` | 5 | **9** | 9 |
| `*b = t` | 5 | 9 | **5** |

Final: **x = 9, y = 5** ✓

### 8. Sample Input / Output

**Test 1:**
```
Enter two numbers: 5 9
Before swap: x = 5, y = 9
After swap : x = 9, y = 5
```

**Test 2:**
```
Enter two numbers: 100 7
Before swap: x = 100, y = 7
After swap : x = 7, y = 100
```

**Test 3 (identical values):**
```
Enter two numbers: 4 4
Before swap: x = 4, y = 4
After swap : x = 4, y = 4
```

### 9. Edge Cases & Special Notes

| Case | Behaviour |
|---|---|
| Identical values | Swapped → no visible change, logically correct |
| Negative numbers | Works normally |
| `swap(&x, &x)` (same variable) | t=x, x=x, x=t → value unchanged — harmless |
| Swapping floats | Needs a `float *` version (pointers are type-specific) |
| Wrong: `swap(x, y)` without `&` | Function gets values as addresses → compile warning / crash |

### 10. Common Mistakes to Avoid

1. **Forgetting `&` in the call** — `swap(x, y)` passes *values*, not addresses; the compiler warns and the function can't modify originals.
2. **Forgetting `*` inside the function** — writing `a = b` only copies the *addresses* (making pointers point at each other's targets) instead of the values.
3. **No temporary variable** — `*a = *b; *b = *a;` loses the original `*a` (both end up equal).
4. **Mixing up `*` and `&`** — `&` *takes* an address, `*` *follows* an address. `swap(*x, *y)` is wrong — you'd dereference before the call.
5. **Declaring `t` as a pointer** — `int *t;` then `t = *a;` is a type error; `t` must be a plain `int` because it stores a *value*.

### 11. Comparison — Why a Non-Pointer Version Fails

```c
/* WRONG — passes by value, swap has no effect on main() */
void badSwap(int a, int b)
{
    int t = a;
    a = b;
    b = t;      /* only local copies swapped */
}
```
If `main` calls `badSwap(x, y)`, x and y stay unchanged — because `a` and `b` are copies. This is exactly why the assignment demands **pointers**.

### 12. Alternative Version — pointer-to-pointer technique

```c
#include <stdio.h>

void swap(int **a, int **b)     /* swaps the addresses, not the values */
{
    int *t = *a;
    *a = *b;
    *b = t;
}

int main()
{
    int x = 5, y = 9;
    int *p = &x, *q = &y;

    printf("p points to %d, q points to %d\n", *p, *q);

    swap(&p, &q);               /* now p points to y, q points to x */

    printf("p points to %d, q points to %d\n", *p, *q);
    return 0;
}
```
*(Less common for this exercise, but a good advanced demonstration.)*

### 13. Brief Result Table (for the lab record)

| Input | Before swap | After swap |
|---|---|---|
| 5 9 | x=5, y=9 | x=9, y=5 |
| 100 7 | x=100, y=7 | x=7, y=100 |
| −8 3 | x=−8, y=3 | x=3, y=−8 |

---
