# 15. Write a Program to Find Sum of n Elements Entered by User Using Dynamic Memory Allocation (malloc/calloc)

### 1. Problem Understanding

1. Ask the user how many elements (`n`) they want to enter.
2. Allocate memory for `n` integers **dynamically** using `malloc()` (or `calloc()`).
3. Read the `n` elements into that allocated memory.
4. Compute and print their **sum**.
5. **Free** the allocated memory (good practice, prevents memory leak).

Unlike a fixed array (`int a[100]`), dynamic allocation uses **exactly the memory needed** at runtime.

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| `malloc()` | Allocates `size` bytes of uninitialised memory — returns `void*` |
| `calloc()` | Allocates memory for `n` items and **zero-initialises** them — returns `void*` |
| `sizeof(int)` | Size of one integer (usually 4 bytes) |
| Pointer arithmetic | `ptr[i]` is the same as `*(ptr + i)` |
| `free()` | Releases the allocated memory back to the heap |
| `stdlib.h` | Header for `malloc`, `calloc`, `free` |
| NULL check | After allocation, verify the pointer isn't NULL (memory exhausted) |

### 3. Program Structure

```
main()
 ├── read n
 ├── allocate: ptr = (int *)malloc(n * sizeof(int));   [or calloc]
 ├── if (ptr == NULL) → error, exit
 ├── loop i=0..n-1: scanf("%d", &ptr[i]);   sum += ptr[i];
 ├── print sum
 ├── free(ptr)
 └── stop
```

### 4. Algorithm (Step-by-Step)

1. Start.
2. Read `n` (number of elements).
3. Allocate memory: `ptr = (int *)malloc(n * sizeof(int));`
4. **Check if allocation failed** (`ptr == NULL`) → print error, stop.
5. Read `n` elements using `ptr[i]`, adding each to `sum`.
6. Print the total sum.
7. `free(ptr);` — release the memory.
8. Stop.

### 5. The C Program (Source Code)

**Version 1 — using `malloc()`:**
```c
#include <stdio.h>
#include <stdlib.h>      /* for malloc(), calloc(), free() */

int main()
{
    int n, i, sum = 0;
    int *ptr;

    printf("How many elements? ");
    scanf("%d", &n);

    /* allocate memory for n integers */
    ptr = (int *)malloc(n * sizeof(int));

    if (ptr == NULL)          /* memory allocation failed */
    {
        printf("Memory allocation failed!\n");
        return 1;
    }

    printf("Enter %d elements: ", n);
    for (i = 0; i < n; i++)
    {
        scanf("%d", &ptr[i]);     /* ptr[i] same as *(ptr + i) */
        sum += ptr[i];
    }

    printf("Sum of %d elements = %d\n", n, sum);

    free(ptr);                    /* release the memory */
    return 0;
}
```

**Version 2 — using `calloc()`:**
```c
#include <stdio.h>
#include <stdlib.h>

int main()
{
    int n, i, sum = 0;
    int *ptr;

    printf("How many elements? ");
    scanf("%d", &n);

    /* calloc allocates and zero-initialises n elements */
    ptr = (int *)calloc(n, sizeof(int));

    if (ptr == NULL)
    {
        printf("Memory allocation failed!\n");
        return 1;
    }

    printf("Enter %d elements: ", n);
    for (i = 0; i < n; i++)
    {
        scanf("%d", &ptr[i]);
        sum += ptr[i];
    }

    printf("Sum of %d elements = %d\n", n, sum);

    free(ptr);
    return 0;
}
```

### 6. Line-by-Line Explanation (malloc version)

| Code | What it does |
|---|---|
| `#include <stdlib.h>` | Declares `malloc`, `calloc`, `free` |
| `int *ptr;` | A pointer that will hold the address of the allocated block |
| `ptr = (int *)malloc(n * sizeof(int));` | Allocates `n × 4` bytes (n integers); `(int *)` casts the returned `void*` to an integer pointer |
| `if (ptr == NULL)` | Guards against allocation failure (heap exhausted) |
| `scanf("%d", &ptr[i])` | `ptr[i]` dereferences the i-th integer; note `&ptr[i]` gives its address |
| `sum += ptr[i];` | Accumulates each element into the sum |
| `free(ptr);` | Returns memory to the OS — prevents a **memory leak** |

### 7. Sample Input / Output

**Test 1:**
```
How many elements? 5
Enter 5 elements: 10 20 30 40 50
Sum of 5 elements = 150
```

**Test 2:**
```
How many elements? 4
Enter 4 elements: 7 8 9 10
Sum of 4 elements = 34
```

### 8. Edge Cases Table

| Case | Behaviour |
|---|---|
| n = 0 | Allocates 0 bytes (implementation-defined but usually OK); sum = 0 |
| n = 1 | Single element; sum = that element |
| Negative elements | Summed correctly (e.g., 10 + (−5) = 5) |
| Very large n | If allocation fails, `NULL` check prints error and exits safely |
| Integer overflow | Sum could overflow `int` for huge inputs — use `long` if needed |

### 9. malloc() vs calloc() — Table for the Lab Record

| Feature | `malloc()` | `calloc()` |
|---|---|---|
| Syntax | `malloc(size)` | `calloc(count, size)` |
| Memory initialised? | **No** (garbage values) | **Yes** — all bytes set to 0 |
| Number of arguments | 1 (total bytes) | 2 (count, element size) |
| Use case | Faster when you'll overwrite anyway | When you need zero-initialised memory |
| Both return | `void*` — cast to the needed pointer type | Same |

> **Note:** For this sum program, `malloc` is usually preferred because we immediately fill every element with `scanf` — initialisation is unnecessary.

### 10. Common Mistakes to Avoid

1. **Forgetting `#include <stdlib.h>`** — implicit declaration of `malloc` causes a compile warning/error.
2. **Forgetting the size multiplication** — `malloc(n)` allocates only `n` **bytes**, not `n` integers. Always `n * sizeof(int)`.
3. **Not checking for NULL** — dereferencing a NULL pointer crashes the program.
4. **Forgetting `(int *)` cast** — valid in C (implicit conversion from `void*`) but many compilers warn; casting is clearer.
5. **Accessing beyond n** — `ptr[n]` and beyond is **out of bounds** — undefined behaviour.
6. **Not calling `free()`** — memory leak; repeated runs gradually exhaust the heap.
7. **Using `calloc(n, sizeof(int))` then reusing as double** — wrong size per element; always match `sizeof(type)` with the pointer type.

### 11. Alternative — sum in a separate function

```c
#include <stdio.h>
#include <stdlib.h>

int sumElements(int *arr, int n)
{
    int i, s = 0;
    for (i = 0; i < n; i++)
        s += arr[i];
    return s;
}

int main()
{
    int n, i;
    int *ptr;

    printf("How many elements? ");
    scanf("%d", &n);

    ptr = (int *)malloc(n * sizeof(int));

    if (ptr == NULL)
    {
        printf("Memory allocation failed!\n");
        return 1;
    }

    printf("Enter %d elements: ", n);
    for (i = 0; i < n; i++)
        scanf("%d", &ptr[i]);

    printf("Sum of %d elements = %d\n", n, sumElements(ptr, n));

    free(ptr);
    return 0;
}
```

### 12. Brief Result Table (for the lab record)

| n | Elements entered | Sum |
|---|---|---|
| 5 | 10 20 30 40 50 | 150 |
| 4 | 7 8 9 10 | 34 |
| 3 | −5 15 20 | 30 |

---
