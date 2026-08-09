# 22. Create Matrix Class Using Templates — Menu-Driven Program (Product, Transpose, Sum, Difference)

### 1. Problem Understanding

Write a **template-based Matrix class** in C++ that works with **any numeric type** (int, float, double, …), then a **menu-driven** program performing:

| # | Operation | Rule |
|---|---|---|
| 1 | **Product** | A is m×n, B must be n×p → result m×p |
| 2 | **Transpose** | m×n → n×m (swap rows/columns) |
| 3 | **Sum** | A and B must have the **same** dimensions |
| 4 | **Difference** | Same dimension requirement |

**Why templates?** One `Matrix<T>` class can store `int`, `float`, or `double` matrices without rewriting the code. The compiler generates a concrete class per used type.

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| Class template | `template <class T> class Matrix { ... };` |
| Dynamic allocation | `T **data` — 2D array of size rows×cols |
| Constructor/Destructor | Allocate in constructor, `delete[]` in destructor |
| Friend function | `operator>>`/`<<` need access to private members |
| Operator overloading | `+`, `-`, `*` for sum, difference, product |
| Menu + switch | Dispatch user choice |

### 3. The C++ Program (Source Code)

```cpp
#include <iostream>
using namespace std;

template <class T>
class Matrix
{
private:
    int rows, cols;
    T **data;

public:
    /* constructor */
    Matrix(int r, int c)
    {
        rows = r;
        cols = c;
        data = new T *[rows];
        for (int i = 0; i < rows; i++)
            data[i] = new T[cols];
    }

    /* destructor */
    ~Matrix()
    {
        for (int i = 0; i < rows; i++)
            delete[] data[i];
        delete[] data;
    }

    int getRows() { return rows; }
    int getCols() { return cols; }

    /* read the matrix */
    void read()
    {
        for (int i = 0; i < rows; i++)
            for (int j = 0; j < cols; j++)
                cin >> data[i][j];
    }

    /* display the matrix */
    void display()
    {
        for (int i = 0; i < rows; i++)
        {
            for (int j = 0; j < cols; j++)
                cout << data[i][j] << " ";
            cout << endl;
        }
    }

    /* transpose: returns a new Matrix with rows and cols swapped */
    Matrix transpose()
    {
        Matrix T(cols, rows);
        for (int i = 0; i < rows; i++)
            for (int j = 0; j < cols; j++)
                T.data[j][i] = data[i][j];
        return T;
    }

    /* sum: this + other (dimensions must match) */
    Matrix operator+(Matrix &other)
    {
        Matrix res(rows, cols);
        for (int i = 0; i < rows; i++)
            for (int j = 0; j < cols; j++)
                res.data[i][j] = data[i][j] + other.data[i][j];
        return res;
    }

    /* difference: this - other (dimensions must match) */
    Matrix operator-(Matrix &other)
    {
        Matrix res(rows, cols);
        for (int i = 0; i < rows; i++)
            for (int j = 0; j < cols; j++)
                res.data[i][j] = data[i][j] - other.data[i][j];
        return res;
    }

    /* product: this * other (this.cols must equal other.rows) */
    Matrix operator*(Matrix &other)
    {
        Matrix res(rows, other.cols);
        for (int i = 0; i < rows; i++)
        {
            for (int j = 0; j < other.cols; j++)
            {
                res.data[i][j] = 0;
                for (int k = 0; k < cols; k++)
                    res.data[i][j] += data[i][k] * other.data[k][j];
            }
        }
        return res;
    }
};

int main()
{
    int r1, c1, r2, c2, choice;

    cout << "Enter dimensions of Matrix A (rows cols): ";
    cin >> r1 >> c1;
    cout << "Enter dimensions of Matrix B (rows cols): ";
    cin >> r2 >> c2;

    cout << "\nEnter elements of Matrix A (" << r1 << "x" << c1 << "):\n";
    Matrix<int> A(r1, c1);
    A.read();

    cout << "\nEnter elements of Matrix B (" << r2 << "x" << c2 << "):\n";
    Matrix<int> B(r2, c2);
    B.read();

    cout << "\nMatrix A:\n"; A.display();
    cout << "\nMatrix B:\n"; B.display();

    do {
        cout << "\n---------- MATRIX MENU ----------\n";
        cout << "1. Product\n";
        cout << "2. Transpose\n";
        cout << "3. Sum\n";
        cout << "4. Difference\n";
        cout << "5. Quit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice)
        {
            case 1:   /* product */
                if (c1 != r2)
                    cout << "Product not possible (A.cols != B.rows).\n";
                else
                {
                    Matrix<int> P = A * B;
                    cout << "\nProduct (A x B):\n";
                    P.display();
                }
                break;

            case 2:   /* transpose */
            {
                Matrix<int> T = A.transpose();
                cout << "\nTranspose of A:\n";
                T.display();
                break;
            }

            case 3:   /* sum */
                if (r1 != r2 || c1 != c2)
                    cout << "Sum not possible (dimensions differ).\n";
                else
                {
                    Matrix<int> S = A + B;
                    cout << "\nSum (A + B):\n";
                    S.display();
                }
                break;

            case 4:   /* difference */
                if (r1 != r2 || c1 != c2)
                    cout << "Difference not possible (dimensions differ).\n";
                else
                {
                    Matrix<int> D = A - B;
                    cout << "\nDifference (A - B):\n";
                    D.display();
                }
                break;

            case 5:
                cout << "Exiting...\n";
                break;

            default:
                cout << "Invalid choice! Try again.\n";
        }
    } while (choice != 5);

    return 0;
}
```

### 4. Line-by-Line Explanation of Key Parts

| Code | What it does |
|---|---|
| `template <class T> class Matrix` | Declares a **generic class** usable with any type `T` |
| `T **data;` | 2D dynamic array — pointer to rows, each row a pointer to columns |
| `data[i] = new T[cols];` | Allocates each row in the constructor |
| `~Matrix()` | **Destructor** — frees every row, then the row array (prevents memory leak) |
| `Matrix transpose()` | Returns a matrix with swapped dimensions; `T.data[j][i] = data[i][j]` |
| `Matrix operator+(Matrix &other)` | **Operator overload** — adds element-wise; must validate equal sizes before calling |
| `Matrix operator-(Matrix &other)` | Element-wise subtraction |
| `Matrix operator*(Matrix &other)` | **Triple loop** — standard matrix multiplication (rows × cols × inner) |
| `for k... res.data[i][j] += data[i][k] * other.data[k][j]` | The dot product of row `i` of A with column `j` of B |
| `Matrix<int> A(r1, c1);` | Instantiates the template with `T = int` |

### 5. Matrix Multiplication — the Algorithm

For `A[m×n] × B[n×p] = C[m×p]`:
```
C[i][j] = Σ(k=0 to n-1)  A[i][k] * B[k][j]
```
- Row `i` of A dotted with column `j` of B.
- Dimension check: **A's columns must equal B's rows** (`c1 == r2`).
- The result has **A's rows × B's columns**.

### 6. Dry Run

**A = [[1,2],[3,4]], B = [[5,6],[7,8]]**

**Product (A×B):**
```
C[0][0] = 1×5 + 2×7 = 19
C[0][1] = 1×6 + 2×8 = 22
C[1][0] = 3×5 + 4×7 = 43
C[1][1] = 3×6 + 4×8 = 50
```
```
A x B = | 19  22 |
        | 43  50 |
```

**Sum:**
```
| 1+5  2+6 |   =   | 6   8 |
| 3+7  4+8 |       | 10  12 |
```

**Difference (A−B):**
```
| 1-5  2-6 |   =   | -4  -4 |
| 3-7  4-8 |       | -4  -4 |
```

**Transpose of A:**
```
| 1  3 |
| 2  4 |
```

### 7. Sample Run

```
Enter dimensions of Matrix A (rows cols): 2 2
Enter dimensions of Matrix B (rows cols): 2 2

Enter elements of Matrix A (2x2):
1 2
3 4

Enter elements of Matrix B (2x2):
5 6
7 8

Matrix A:
1 2
3 4

Matrix B:
5 6
7 8

---------- MATRIX MENU ----------
1. Product
...
Enter your choice: 1
Product (A x B):
19 22
43 50

Enter your choice: 3
Sum (A + B):
6 8
10 12

Enter your choice: 2
Transpose of A:
1 3
2 4

Enter your choice: 5
Exiting...
```

### 8. Edge Cases Table

| Case | Behaviour |
|---|---|
| c1 != r2 (product) | Guarded — prints "Product not possible" |
| Different dims (sum/difference) | Guarded — prints message |
| Square matrices | All operations work |
| 1×1 matrices | Behaves like scalar arithmetic |
| Rectangular matrices | Transpose swaps dims correctly |
| Non-numeric type (e.g., `Matrix<char>`) | Works via templates, but sum/product of chars is odd — intended for numbers |

### 9. Common Mistakes to Avoid

1. **Missing dimension checks** — matrix product and sum crash or read out-of-bounds memory if dimensions don't match. Always validate first.
2. **Transpose in-place on a rectangular matrix** — the result has different dimensions, so it **must** go into a new matrix (as done here).
3. **Not freeing memory in the destructor** — memory leak; also the compiler-generated **copy constructor** would share the same `data` pointer (shallow copy → double free). For safety add a copy constructor / copy assignment, or use `vector<vector<T>>`.
4. **Initialising `res.data[i][j] = 0;` before the k-loop** — otherwise the product accumulates garbage.
5. **Template code split across .cpp files** — templates must be fully visible at instantiation (header or same file).
6. **Integer overflow** for large products — use `double` or `long long` as the type parameter.

### 10. Safer Version — using `std::vector` (avoids manual memory + copy problems)

```cpp
#include <iostream>
#include <vector>
using namespace std;

template <class T>
class Matrix
{
    int rows, cols;
    vector<vector<T>> data;

public:
    Matrix(int r, int c) : rows(r), cols(c)
    {
        data.resize(r, vector<T>(c, 0));
    }

    void read()
    {
        for (int i = 0; i < rows; i++)
            for (int j = 0; j < cols; j++)
                cin >> data[i][j];
    }

    void display()
    {
        for (int i = 0; i < rows; i++)
        {
            for (int j = 0; j < cols; j++)
                cout << data[i][j] << " ";
            cout << endl;
        }
    }

    Matrix transpose()
    {
        Matrix T(cols, rows);
        for (int i = 0; i < rows; i++)
            for (int j = 0; j < cols; j++)
                T.data[j][i] = data[i][j];
        return T;
    }

    Matrix operator+(Matrix &o)
    {
        Matrix r(rows, cols);
        for (int i = 0; i < rows; i++)
            for (int j = 0; j < cols; j++)
                r.data[i][j] = data[i][j] + o.data[i][j];
        return r;
    }

    Matrix operator-(Matrix &o)
    {
        Matrix r(rows, cols);
        for (int i = 0; i < rows; i++)
            for (int j = 0; j < cols; j++)
                r.data[i][j] = data[i][j] - o.data[i][j];
        return r;
    }

    Matrix operator*(Matrix &o)
    {
        Matrix r(rows, o.cols);
        for (int i = 0; i < rows; i++)
            for (int j = 0; j < o.cols; j++)
                for (int k = 0; k < cols; k++)
                    r.data[i][j] += data[i][k] * o.data[k][j];
        return r;
    }
};
```
*(The `main()` stays the same — this version is memory-safe and copy-safe.)*

### 11. Brief Result Table (for the lab record)

| Operation | Input A = [[1,2],[3,4]], B = [[5,6],[7,8]] |
|---|---|
| Product (A×B) | [[19,22],[43,50]] |
| Sum (A+B) | [[6,8],[10,12]] |
| Difference (A−B) | [[−4,−4],[−4,−4]] |
| Transpose of A | [[1,3],[2,4]] |

---

# (h) Menu-Driven Program to Find Area of Rectangle, Square, Circle, and Triangle Using User-Defined Functions

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
