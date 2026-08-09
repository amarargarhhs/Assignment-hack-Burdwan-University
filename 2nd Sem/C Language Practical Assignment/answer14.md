# 14. Write a Program Which Takes Radius of a Circle as Input, Passes It to Another Function That Computes Area and Circumference, and Displays Them

### 1. Problem Understanding

1. Read the **radius** of a circle from the user.
2. **Pass it to another function** that computes:
   - **Area** = π × r²
   - **Circumference** = 2 × π × r
3. **Display** both results.

**Design choice:** The assignment says "a function that computes area and circumference". This is naturally done by a function that takes the radius and **outputs both results** — best implemented by returning one value and using a **pointer parameter** for the second, or by displaying from within the function.

I'll show the pointer-based version (clearest demonstration), then a simple display-in-function variant.

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| `float`/`double` | Area and circumference are decimal values |
| `M_PI` or constant | π = 3.14159… — use `#define PI 3.14159` (portable) or `M_PI` from `<math.h>` |
| `pow()` function | `pow(r, 2)` from `<math.h>` (or simply `r * r`) |
| Passing by value + pointer out | Radius passed by value; second result returned through a pointer parameter |
| Return value | Function returns one value; pointer handles the other |

### 3. Algorithm (Step-by-Step)

1. Start.
2. Read radius `r`.
3. Call `compute(r, &circumference)`:
   - **Inside:** `area = PI * r * r;` `*circ = 2 * PI * r;`
   - Return `area`.
4. Back in `main()`, display the returned `area` and the `circumference` filled by the pointer.
5. Stop.

### 4. The C Program (Source Code)

```c
#include <stdio.h>
#define PI 3.14159

/* function that computes area and circumference of a circle.
   returns area; stores circumference through the pointer parameter */
float compute(float r, float *circumference)
{
    float area = PI * r * r;          /* area = pi * r^2 */

    *circumference = 2 * PI * r;      /* circumference = 2 * pi * r */

    return area;
}

int main()
{
    float radius, area, circumference;

    printf("Enter the radius of the circle: ");
    scanf("%f", &radius);

    area = compute(radius, &circumference);   /* pass radius, address of circumference */

    printf("\nRadius        = %.2f\n", radius);
    printf("Area          = %.2f\n", area);
    printf("Circumference = %.2f\n", circumference);

    return 0;
}
```

### 5. Line-by-Line Explanation

| Code | What it does |
|---|---|
| `#define PI 3.14159` | A constant for π (avoids float rounding inconsistencies in `M_PI`) |
| `float compute(float r, float *circumference)` | Takes radius by value, pointer for the second output |
| `float area = PI * r * r;` | Area formula π·r² |
| `*circumference = 2 * PI * r;` | Circumference formula 2πr, written to caller's variable via dereference |
| `return area;` | Area handed back through the return value |
| `area = compute(radius, &circumference);` | Radius passed by value, `&circumference` lets the function fill it in |

### 6. Dry Run

**Input: r = 7** (using PI = 3.14159)

| Step | Calculation | Result |
|---|---|---|
| Area | 3.14159 × 7 × 7 | **153.93791** |
| Circumference | 2 × 3.14159 × 7 | **43.98226** |

**Input: r = 5**

| Step | Calculation | Result |
|---|---|---|
| Area | 3.14159 × 25 | **78.53975** |
| Circumference | 2 × 3.14159 × 5 | **31.41590** |

### 7. Sample Input / Output

**Test 1:**
```
Enter the radius of the circle: 7

Radius        = 7.00
Area          = 153.94
Circumference = 43.98
```

**Test 2:**
```
Enter the radius of the circle: 5

Radius        = 5.00
Area          = 78.54
Circumference = 31.42
```

**Test 3 (decimal radius):**
```
Enter the radius of the circle: 2.5

Radius        = 2.50
Area          = 19.63
Circumference = 15.71
```

### 8. Edge Cases Table

| Radius | Area (π=3.14159) | Circumference | Remark |
|---|---|---|---|
| 0 | 0 | 0 | Degenerate circle (point) — formulas still work |
| 1 | 3.14159 | 6.28318 | Unit circle |
| 2.5 | 19.635 | 15.708 | Fractional input |
| Negative | Positive | Negative | Mathematically meaningless; optionally validate |

> **Validation note:** you may add `if (radius < 0) { printf("Radius cannot be negative.\n"); return 1; }` before computing.

### 9. Common Mistakes to Avoid

1. **Using `int` for radius/area** — integer division truncates; always use `float` or `double`.
2. **Forgetting `&` for the pointer output** — `compute(radius, circumference)` passes a garbage address → crash or wrong value.
3. **Defining PI as an integer** — `#define PI 3` gives wildly wrong results. Use `3.14159`.
4. **Using `2 * 22/7 * r`** — `22/7` is **integer division** (3) in C! Write `2 * 22.0/7 * r` or `2 * (22.0/7) * r`.
5. **Confusing the two formulas** — Area is π·r², circumference is 2·π·r (not π·r²).
6. **Not including `<math.h>`** if using `pow()` or `M_PI` — and then `M_PI` may be undefined on some compilers unless `_USE_MATH_DEFINES` is set (Windows/VS).

### 10. Alternative Versions

**Version A — simpler: function prints directly (no pointers):**
```c
#include <stdio.h>
#define PI 3.14159

void compute(float r)
{
    float area = PI * r * r;
    float circumference = 2 * PI * r;
    printf("\nRadius        = %.2f\n", r);
    printf("Area          = %.2f\n", area);
    printf("Circumference = %.2f\n", circumference);
}

int main()
{
    float radius;
    printf("Enter the radius of the circle: ");
    scanf("%f", &radius);
    compute(radius);
    return 0;
}
```

**Version B — using `double` and `M_PI` from `<math.h>`:**
```c
#include <stdio.h>
#include <math.h>
#define _USE_MATH_DEFINES

double computeArea(double r) { return M_PI * r * r; }
double computeCircum(double r) { return 2 * M_PI * r; }

int main()
{
    double radius;
    printf("Enter the radius of the circle: ");
    scanf("%lf", &radius);

    printf("\nRadius        = %.2lf\n", radius);
    printf("Area          = %.2lf\n", computeArea(radius));
    printf("Circumference = %.2lf\n", computeCircum(radius));
    return 0;
}
```
*(Note `%lf` for `double`, and this uses two functions — one per result.)*

### 11. Brief Result Table (for the lab record)

| Radius (r) | Area (πr²) | Circumference (2πr) |
|---|---|---|
| 1 | 3.14 | 6.28 |
| 5 | 78.54 | 31.42 |
| 7 | 153.94 | 43.98 |
| 2.5 | 19.63 | 15.71 |

---
