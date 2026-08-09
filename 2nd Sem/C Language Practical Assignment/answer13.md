# 13. Write a Program in Which a Function Is Passed the Address of Two Variables and Then Alters Their Contents

### 1. Problem Understanding

This program demonstrates **call by reference** — a function receives the **addresses** of two variables and **modifies the contents** stored at those addresses. Since the function works directly on the caller's memory, the changes are visible back in `main()` after the call.

This is the same core idea as the pointer-swap program (Q12), but the emphasis here is on **altering contents** — so we demonstrate a function that actually **changes the values** (e.g., adding both, doubling them, exchanging them, etc.).

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| Function parameters as pointers | `void alter(int *p, int *q)` — receives addresses |
| Address-of operator `&` | `&a, &b` in the call |
| Dereference `*` | Reading/writing the value at the address: `*p`, `*q` |
| Call by reference | Modifications to `*p`/`*q` affect the originals in `main` |
| Effect visible to caller | The whole point — original variables change |

### 3. Algorithm (Step-by-Step)

1. Start.
2. `main()` declares `a` and `b`, reads their values.
3. Print the original values.
4. Call `alter(&a, &b)` — passing **addresses**.
5. **Inside `alter(int *p, int *q)`:**
   - Store the sum of the two original values.
   - Assign each variable new values (demonstrates alteration).
6. Back in `main()`, print the values again — they have changed.
7. Stop.

### 4. The C Program (Source Code)

```c
#include <stdio.h>

/* function that receives addresses of two variables
   and alters the contents stored at those addresses */
void alter(int *p, int *q)
{
    int sum = *p + *q;    /* sum of the original values */

    *p = sum;             /* alter first variable  */
    *q = sum * 2;         /* alter second variable */
}

int main()
{
    int a = 10, b = 20;

    printf("Before calling alter():\n");
    printf("a = %d, b = %d\n", a, b);

    alter(&a, &b);        /* pass addresses of a and b */

    printf("\nAfter calling alter():\n");
    printf("a = %d, b = %d\n", a, b);

    return 0;
}
```

### 5. Line-by-Line Explanation

| Code | What it does |
|---|---|
| `void alter(int *p, int *q)` | `p` and `q` are pointers — they will hold the **addresses** of `a` and `b` |
| `int sum = *p + *q;` | Reads both values via dereference, stores their sum |
| `*p = sum;` | Writes a **new value into the memory** `p` points to (i.e., alters `a`) |
| `*q = sum * 2;` | Writes another **new value** into the memory `q` points to (i.e., alters `b`) |
| `alter(&a, &b);` | Passes the **addresses** so the function can modify the originals |
| Second `printf` | Proves the variables in `main` were genuinely altered |

### 6. Memory View (How It Works)

Suppose `a` is at address `1000` holding `10`, and `b` at `1004` holding `20`:

```
Before call:      a @1000 = 10,   b @1004 = 20
Call: alter(&a, &b)  →  p = 1000, q = 1004

Inside function:
  sum = *p + *q   → 10 + 20 = 30
  *p = sum        → memory@1000 = 30    (a is now 30)
  *q = sum * 2    → memory@1004 = 60    (b is now 60)
```

Because `p` and `q` hold the *addresses* of `a` and `b`, writing through them alters **the original variables** — not copies.

### 7. Dry Run

| Step | sum | a (@1000) | b (@1004) |
|---|---|---|---|
| Before call | — | 10 | 20 |
| `sum = *p + *q` | 30 | 10 | 20 |
| `*p = sum` | 30 | **30** | 20 |
| `*q = sum * 2` | 30 | 30 | **60** |

Final: **a = 30, b = 60** — both contents altered ✓

### 8. Sample Input / Output

**Test 1 (initialised values):**
```
Before calling alter():
a = 10, b = 20

After calling alter():
a = 30, b = 60
```

**Test 2 (user-entered values):**
```
Enter two numbers: 5 7
Before calling alter():
a = 5, b = 7

After calling alter():
a = 12, b = 24
```

### 9. Variant — Many Ways to Alter the Contents

The assignment only says "alters their contents", so any sensible transformation qualifies. Common variants:

```c
/* Variant A — swap the two values */
void swapIt(int *p, int *q)
{
    int t = *p;
    *p = *q;
    *q = t;
}

/* Variant B — increment both by a fixed amount */
void increment(int *p, int *q, int inc)
{
    *p += inc;
    *q += inc;
}

/* Variant C — double both */
void doubleBoth(int *p, int *q)
{
    *p = *p * 2;
    *q = *q * 2;
}

/* Variant D — assign entirely new values */
void reassign(int *p, int *q)
{
    *p = 100;
    *q = 200;
}
```

### 10. Common Mistakes to Avoid

1. **Forgetting `&` in the call** — `alter(a, b)` passes *values*; the compiler complains about mismatched types and the function cannot alter anything.
2. **Forgetting `*` inside the function** — `p = sum;` assigns to the *pointer variable itself* (an address), not to the value it points to.
3. **Using the parameters as if they were normal integers** — `*p` is required on every read and write of the target.
4. **Declaring `sum` as a pointer** — it holds a value (the sum), so it must be a plain `int`, not `int *`.
5. **Assuming changes happen without pointers** — a by-value function (`void alter(int p, int q)`) would alter only local copies; `main` would see no change.

### 11. Comparison: By-Value vs. By-Reference

| | By value `alter(int p, int q)` | By reference `alter(int *p, int *q)` |
|---|---|---|
| What is passed | Copy of the values | Addresses of the variables |
| Can originals change? | **No** — only local copies | **Yes** — writes go to main's memory |
| Dereference needed? | No | Yes (`*p`, `*q`) |
| Example call | `alter(a, b)` | `alter(&a, &b)` |

This contrast is exactly why the assignment asks for addresses — it's teaching **call by reference**.

### 12. Brief Result Table (for the lab record)

| Operation | Before call (a, b) | After call (a, b) |
|---|---|---|
| Sum & double (`*p = sum, *q = sum*2`) | 10, 20 | 30, 60 |
| Swap | 10, 20 | 20, 10 |
| Increment by 5 | 10, 20 | 15, 25 |
| Double both | 10, 20 | 20, 40 |
| Reassign | 10, 20 | 100, 200 |

---
