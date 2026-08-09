# 10. WAP to Perform Actions on an Array Entered by the User (Menu-Driven)

### 1. Problem Understanding

Write a **menu-driven** program that:
1. Lets the user **enter an array** (size + elements).
2. Displays a **menu** with these operations:
   - Print **even**-valued elements
   - Print **odd**-valued elements
   - Print **sum** and **average**
   - Print **maximum** and **minimum** element
   - **Remove duplicates**
   - Print array in **reverse** order
3. Lets the user **re-enter the array** or **quit**.

The cleanest design uses **one function per operation**, a global/`static` array or a size variable shared via the menu loop, and an infinite loop with a `switch`.

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| Array | Fixed-size container `int a[MAX]` |
| Passing arrays to functions | Arrays are passed by reference (address) automatically in C |
| `switch` statement | Dispatches the menu choice to the right function |
| `%` operator | `a[i] % 2 == 0` → even, `a[i] % 2 != 0` → odd |
| Sum & average | Accumulate sum, divide by `n` (careful: integer division) |
| Max/min | Standard linear scan keeping running best values |
| Remove duplicates | Nested loop comparison + shifting elements left |
| `do…while` loop | Guarantees the menu shows at least once |

### 3. Program Design

```
main()
 ├── declare array, size
 ├── do { show menu → switch(choice)
 │       ├── 1: print even elements
 │       ├── 2: print odd elements
 │       ├── 3: sum & average
 │       ├── 4: max & min
 │       ├── 5: remove duplicates
 │       ├── 6: reverse order
 │       ├── 7: re-enter array
 │       └── 8: quit
 └── } while (choice != 8)
```

### 4. The C Program (Source Code)

```c
#include <stdio.h>
#define MAX 100

/* 1. print even-valued elements */
void printEven(int a[], int n)
{
    int i, found = 0;
    printf("Even elements: ");
    for (i = 0; i < n; i++)
    {
        if (a[i] % 2 == 0)
        {
            printf("%d ", a[i]);
            found = 1;
        }
    }
    if (!found)
        printf("(none)");
    printf("\n");
}

/* 2. print odd-valued elements */
void printOdd(int a[], int n)
{
    int i, found = 0;
    printf("Odd elements: ");
    for (i = 0; i < n; i++)
    {
        if (a[i] % 2 != 0)
        {
            printf("%d ", a[i]);
            found = 1;
        }
    }
    if (!found)
        printf("(none)");
    printf("\n");
}

/* 3. sum and average */
void sumAvg(int a[], int n)
{
    int i, sum = 0;
    for (i = 0; i < n; i++)
        sum += a[i];
    printf("Sum = %d\n", sum);
    printf("Average = %.2f\n", (float)sum / n);
}

/* 4. maximum and minimum */
void maxMin(int a[], int n)
{
    int i, max, min;
    max = min = a[0];
    for (i = 1; i < n; i++)
    {
        if (a[i] > max)
            max = a[i];
        if (a[i] < min)
            min = a[i];
    }
    printf("Maximum = %d\n", max);
    printf("Minimum = %d\n", min);
}

/* 5. remove duplicates (shifts elements, updates size by reference) */
void removeDuplicates(int a[], int *np)
{
    int i, j, k;
    int n = *np;

    for (i = 0; i < n; i++)
    {
        for (j = i + 1; j < n; j++)
        {
            if (a[i] == a[j])          /* duplicate found at j */
            {
                for (k = j; k < n - 1; k++)
                    a[k] = a[k + 1];   /* shift everything left */
                n--;
                j--;                   /* re-check same j position */
            }
        }
    }

    *np = n;                           /* update size */

    printf("Array after removing duplicates: ");
    for (i = 0; i < n; i++)
        printf("%d ", a[i]);
    printf("\n");
}

/* 6. print in reverse order */
void reverse(int a[], int n)
{
    int i;
    printf("Array in reverse order: ");
    for (i = n - 1; i >= 0; i--)
        printf("%d ", a[i]);
    printf("\n");
}

/* 7. (re)enter the array */
void enterArray(int a[], int *np)
{
    int i, n;
    do {
        printf("Enter the number of elements (1 to %d): ", MAX);
        scanf("%d", &n);
    } while (n < 1 || n > MAX);

    printf("Enter %d elements: ", n);
    for (i = 0; i < n; i++)
        scanf("%d", &a[i]);

    *np = n;
}

int main()
{
    int a[MAX];
    int n = 0;
    int choice;

    do {
        printf("\n----------- MENU -----------\n");
        printf("1. Print even elements\n");
        printf("2. Print odd elements\n");
        printf("3. Sum and average\n");
        printf("4. Maximum and minimum\n");
        printf("5. Remove duplicates\n");
        printf("6. Print array in reverse\n");
        printf("7. Re-enter array\n");
        printf("8. Quit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        if (n == 0 && choice >= 1 && choice <= 6)
        {
            printf("Please enter the array first (option 7).\n");
            continue;
        }

        switch (choice)
        {
            case 1:  printEven(a, n);          break;
            case 2:  printOdd(a, n);           break;
            case 3:  sumAvg(a, n);             break;
            case 4:  maxMin(a, n);             break;
            case 5:  removeDuplicates(a, &n);  break;
            case 6:  reverse(a, n);            break;
            case 7:  enterArray(a, &n);        break;
            case 8:  printf("Exiting...\n");   break;
            default: printf("Invalid choice! Try again.\n");
        }
    } while (choice != 8);

    return 0;
}
```

### 5. Line-by-Line Explanation of Key Parts

| Code | What it does |
|---|---|
| `#define MAX 100` | Fixed maximum array size |
| `void printEven(...)` / `printOdd(...)` | Scan the array; print numbers where `%2` is 0 (even) or 1 (odd) |
| `(float)sum / n` | Casts sum to `float` **before** division → average with decimals |
| `max = min = a[0]` | Seed max/min with the first element, then scan the rest |
| `removeDuplicates(int a[], int *np)` | Takes size **by pointer** so it can shrink the array for the caller |
| `a[k] = a[k + 1]` | Shifts elements left to overwrite a duplicate |
| `n--; j--;` | After removing a duplicate, size shrinks and index `j` re-checks the same spot |
| `enterArray(a, &n)` | Re-reads size + values, updates `n` via pointer |
| `if (n == 0 && choice 1..6)` | Guards against using the array before it is entered |
| `do…while(choice != 8)` | Menu keeps reappearing until the user quits |

### 6. Dry Run — Remove Duplicates

**Array: {3, 5, 3, 7, 5, 3}, n = 6**

| i | j | a[i] | a[j] | Match? | Array after shift | n |
|---|---|---|---|---|---|---|
| 0 | 1 | 3 | 5 | no | {3,5,3,7,5,3} | 6 |
| 0 | 2 | 3 | 3 | **yes** | {3,5,7,5,3} | 5 |
| 0 | 2 | 3 | 5 | no | — | 5 |
| 0 | 3 | 3 | 3 | **yes** | {3,5,7,5} | 4 |
| 0 | 3 | 3 | 5 | no | — | 4 |
| 1 | 2 | 5 | 7 | no | — | 4 |
| 1 | 3 | 5 | 5 | **yes** | {3,5,7} | 3 |

Final: **{3, 5, 7}, n = 3** ✓

### 7. Sample Run

```
----------- MENU -----------
1. Print even elements
2. Print odd elements
...
Enter your choice: 7
Enter the number of elements (1 to 100): 5
Enter 5 elements: 10 15 10 22 15

----------- MENU -----------
Enter your choice: 1
Even elements: 10 10 22

----------- MENU -----------
Enter your choice: 3
Sum = 72
Average = 14.40

----------- MENU -----------
Enter your choice: 5
Array after removing duplicates: 10 15 22

----------- MENU -----------
Enter your choice: 8
Exiting...
```

### 8. Edge Cases Table

| Case | Behaviour |
|---|---|
| All even numbers | `printOdd` prints "(none)" |
| All odd numbers | `printEven` prints "(none)" |
| Duplicates | Removed by shifting; size shrinks |
| All elements identical | Array collapses to a single element |
| n = 1 | Max = min = the only element; reverse prints it once |
| Negative numbers | Treated normally by all functions |
| Choice before entering array | Guarded message: "Please enter the array first" |

### 9. Common Mistakes to Avoid

1. **Integer division for average** — `sum/n` truncates decimals. Cast first: `(float)sum / n`.
2. **Forgetting `&n` in `removeDuplicates`** — the caller must learn the new size, so pass the address.
3. **Not resetting `j` after removing a duplicate** — a skipped duplicate would survive. Use `j--`.
4. **Out-of-bounds access in `maxMin` when n == 0** — seed with `a[0]` only after a guard (we ensure the array is entered first).
5. **Loop in `reverse` starting at `n` instead of `n - 1`** — `a[n]` is out of bounds (array is 0-indexed).
6. **Entering elements as `scanf("%d", a[i])`** — must use `&a[i]`.

### 10. Brief Result Table (for the lab record)

| Operation | Example input array {10, 15, 10, 22, 15} |
|---|---|
| Even elements | 10 10 22 |
| Odd elements | 15 15 |
| Sum | 72 |
| Average | 14.40 |
| Maximum | 22 |
| Minimum | 10 |
| After removing duplicates | 10 15 22 |
| Reverse order | 15 22 10 15 10 |

---
