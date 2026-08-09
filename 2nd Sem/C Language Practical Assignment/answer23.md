# 23. Menu-Driven Program to Find Area of Rectangle, Square, Circle, and Triangle Using User-Defined Functions

### 1. Problem Understanding

Write a **menu-driven** C program that uses **user-defined functions** to compute areas:

| Shape | Formula | Inputs needed |
|---|---|---|
| Rectangle | length × breadth | l, b |
| Square | side × side | s |
| Circle | π × r² | r |
| Triangle | ½ × base × height | b, h |

The program shows a menu, the user picks a shape, enters the measurements, and a dedicated function computes and prints the area.

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| User-defined functions | `rectArea()`, `squareArea()`, `circleArea()`, `triangleArea()` |
| Menu + `switch` | User selects an option; dispatch to the right function |
| `float` return type | Areas are fractional (especially circle/triangle) |
| `#define PI` | Constant for π |
| `do…while` | Loop the menu until the user quits |

### 3. Algorithm (Step-by-Step)

1. Start.
2. Show the menu (Rectangle / Square / Circle / Triangle / Exit).
3. Read the user's choice.
4. **Switch on choice:**
   - **1 (Rectangle):** read l, b → call `rectArea(l, b)`.
   - **2 (Square):** read s → call `squareArea(s)`.
   - **3 (Circle):** read r → call `circleArea(r)`.
   - **4 (Triangle):** read b, h → call `triangleArea(b, h)`.
   - **5:** quit.
5. Repeat from step 2 until the user quits.

### 4. The C Program (Source Code)

```c
#include <stdio.h>
#define PI 3.14159

/* area of rectangle */
float rectArea(float l, float b)
{
    return l * b;
}

/* area of square */
float squareArea(float s)
{
    return s * s;
}

/* area of circle */
float circleArea(float r)
{
    return PI * r * r;
}

/* area of triangle */
float triangleArea(float base, float height)
{
    return 0.5 * base * height;
}

int main()
{
    int choice;
    float l, b, s, r, base, height;

    do {
        printf("\n------------ AREA MENU ------------\n");
        printf("1. Rectangle\n");
        printf("2. Square\n");
        printf("3. Circle\n");
        printf("4. Triangle\n");
        printf("5. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice)
        {
            case 1:
                printf("Enter length and breadth: ");
                scanf("%f %f", &l, &b);
                printf("Area of Rectangle = %.2f\n", rectArea(l, b));
                break;

            case 2:
                printf("Enter side: ");
                scanf("%f", &s);
                printf("Area of Square = %.2f\n", squareArea(s));
                break;

            case 3:
                printf("Enter radius: ");
                scanf("%f", &r);
                printf("Area of Circle = %.2f\n", circleArea(r));
                break;

            case 4:
                printf("Enter base and height: ");
                scanf("%f %f", &base, &height);
                printf("Area of Triangle = %.2f\n", triangleArea(base, height));
                break;

            case 5:
                printf("Exiting...\n");
                break;

            default:
                printf("Invalid choice! Try again.\n");
        }
    } while (choice != 5);

    return 0;
}
```

### 5. Line-by-Line Explanation

| Code | What it does |
|---|---|
| `#define PI 3.14159` | Constant for π used by the circle function |
| `float rectArea(float l, float b)` | User-defined function — returns `l × b` |
| `float squareArea(float s)` | Returns `s × s` |
| `float circleArea(float r)` | Returns `π × r²` |
| `float triangleArea(float base, float height)` | Returns `½ × base × height` |
| `do { … } while (choice != 5);` | Menu reappears until the user selects Exit |
| `scanf("%f %f", &l, &b)` | Reads two floats — note `&` for each variable |
| `printf("Area of Rectangle = %.2f\n", rectArea(l, b));` | Calls the function **and** prints its returned value |

### 6. Dry Run

**Choice 1 (Rectangle), l = 5, b = 3:**
- `rectArea(5, 3)` → 5 × 3 = **15.00**

**Choice 3 (Circle), r = 7:**
- `circleArea(7)` → 3.14159 × 49 = **153.94**

**Choice 4 (Triangle), base = 8, height = 5:**
- `triangleArea(8, 5)` → 0.5 × 8 × 5 = **20.00**

### 7. Sample Input / Output

```
------------ AREA MENU ------------
1. Rectangle
2. Square
3. Circle
4. Triangle
5. Exit
Enter your choice: 1
Enter length and breadth: 5 3
Area of Rectangle = 15.00

Enter your choice: 2
Enter side: 6
Area of Square = 36.00

Enter your choice: 3
Enter radius: 7
Area of Circle = 153.94

Enter your choice: 4
Enter base and height: 8 5
Area of Triangle = 20.00

Enter your choice: 5
Exiting...
```

### 8. Edge Cases Table

| Case | Behaviour |
|---|---|
| Zero dimensions | Area = 0 (mathematically degenerate but computed fine) |
| Decimal values | Work correctly (float inputs) |
| Negative dimensions | Produce negative area — optionally validate |
| Invalid menu choice | "Invalid choice! Try again." — menu loops |
| Circle with r = 1 | Area = 3.14159 (≈ π) |

### 9. Common Mistakes to Avoid

1. **Using `int` for dimensions** — areas like triangle (0.5 × b × h) lose decimals; use `float`.
2. **Integer division** — `1/2 * base * height` in C is **0** (integer division). Always write `0.5 * base * height`.
3. **Forgetting `&` in `scanf`** — without it, garbage is read.
4. **Not using user-defined functions** — the assignment explicitly requires them; each shape must have its own function.
5. **Defining `PI` inside the function** — define it once with `#define` at the top (or use `M_PI` from `<math.h>`).
6. **Forgetting the `default` case** — invalid input would silently do nothing.

### 10. Alternative Version — `void` functions printing inside

```c
#include <stdio.h>
#define PI 3.14159

void rectArea(void)
{
    float l, b;
    printf("Enter length and breadth: ");
    scanf("%f %f", &l, &b);
    printf("Area of Rectangle = %.2f\n", l * b);
}

void squareArea(void)
{
    float s;
    printf("Enter side: ");
    scanf("%f", &s);
    printf("Area of Square = %.2f\n", s * s);
}

void circleArea(void)
{
    float r;
    printf("Enter radius: ");
    scanf("%f", &r);
    printf("Area of Circle = %.2f\n", PI * r * r);
}

void triangleArea(void)
{
    float base, height;
    printf("Enter base and height: ");
    scanf("%f %f", &base, &height);
    printf("Area of Triangle = %.2f\n", 0.5 * base * height);
}

int main()
{
    int choice;
    do {
        printf("\n1.Rectangle  2.Square  3.Circle  4.Triangle  5.Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);
        switch (choice)
        {
            case 1: rectArea();     break;
            case 2: squareArea();   break;
            case 3: circleArea();   break;
            case 4: triangleArea(); break;
            case 5: printf("Exiting...\n"); break;
            default: printf("Invalid choice!\n");
        }
    } while (choice != 5);
    return 0;
}
```

### 11. Brief Result Table (for the lab record)

| Shape | Inputs | Formula | Area |
|---|---|---|---|
| Rectangle | l=5, b=3 | l × b | 15.00 |
| Square | s=6 | s² | 36.00 |
| Circle | r=7 | πr² | 153.94 |
| Triangle | base=8, height=5 | ½ × b × h | 20.00 |

---
