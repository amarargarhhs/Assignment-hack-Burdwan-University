# 3. Menu-Driven Program to Create Mathematical 3D Objects (Visual Python)

### 1. Problem Understanding

Write a **menu-driven program** using **Visual Python (VPython)** that creates six mathematical 3D objects:

| I | II | III | IV | V | VI |
|---|---|---|---|---|---|
| **curve** | **sphere** | **cone** | **arrow** | **ring** | **cylinder** |

The user selects an object from a menu, and the program renders it in a 3D scene. All code is wrapped in **user-defined functions** (one per object) as required by the lab instructions.

**Two flavours of VPython exist:**
- **Old Visual Python:** `from visual import *` (classic module).
- **Modern VPython 7:** `from vpython import *` (installed with `pip install vpython`, runs in Jupyter/GlowScript/web).

The answers below use the **modern `vpython`** package, which is the current standard.

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| `from vpython import *` | Imports VPython's 3D objects, `vector`, and `color` |
| 3D primitives | `curve()`, `sphere()`, `cone()`, `arrow()`, `ring()`, `cylinder()` |
| `vector(x, y, z)` | A point/position/axis in 3D space |
| `color.red` etc. | Built-in colour constants |
| `scene` | The 3D display window |
| Menu + `if/elif` | User picks which object to draw |

### 3. Algorithm (Step-by-Step)

1. Import `vpython`.
2. Define **six functions**, one per object:
   - `draw_curve()` → `curve(...)`
   - `draw_sphere()` → `sphere(...)`
   - `draw_cone()` → `cone(...)`
   - `draw_arrow()` → `arrow(...)`
   - `draw_ring()` → `ring(...)`
   - `draw_cylinder()` → `cylinder(...)`
3. In `main()` show the menu (1–6 plus 7 to exit).
4. Dispatch the choice to the matching function.
5. Loop until the user exits.

### 4. The Python Program (Source Code)

```python
from vpython import *

def draw_curve():
    # A smooth 3D helix-like curve through given points
    pts = [vector(-2, 0, 0), vector(-1, 1, 0), vector(0, 0, 0),
           vector(1, -1, 0), vector(2, 0, 0)]
    curve(pos=pts, color=color.cyan, radius=0.05)
    print("Curve created.")

def draw_sphere():
    sphere(pos=vector(0, 0, 0), radius=1, color=color.red)
    print("Sphere created.")

def draw_cone():
    cone(pos=vector(0, 0, 0), axis=vector(0, 2, 0), radius=1,
         color=color.orange)
    print("Cone created.")

def draw_arrow():
    arrow(pos=vector(-2, 0, 0), axis=vector(4, 2, 0),
          shaftwidth=0.2, color=color.green)
    print("Arrow created.")

def draw_ring():
    ring(pos=vector(0, 0, 0), axis=vector(0, 1, 0),
         radius=1.5, thickness=0.1, color=color.magenta)
    print("Ring created.")

def draw_cylinder():
    cylinder(pos=vector(-1, 0, 0), axis=vector(0, 3, 0),
             radius=0.6, color=color.yellow)
    print("Cylinder created.")

def main():
    while True:
        print("\n---------- 3D OBJECT MENU ----------")
        print("1. Curve")
        print("2. Sphere")
        print("3. Cone")
        print("4. Arrow")
        print("5. Ring")
        print("6. Cylinder")
        print("7. Exit")
        choice = input("Enter your choice (1-7): ")

        if choice == '1':
            draw_curve()
        elif choice == '2':
            draw_sphere()
        elif choice == '3':
            draw_cone()
        elif choice == '4':
            draw_arrow()
        elif choice == '5':
            draw_ring()
        elif choice == '6':
            draw_cylinder()
        elif choice == '7':
            print("Exiting the program. Goodbye!")
            break
        else:
            print("Invalid choice! Please enter 1 to 7.")

if __name__ == "__main__":
    main()
```

### 5. Line-by-Line Explanation

| Code | What it does |
|---|---|
| `from vpython import *` | Imports all VPython classes (sphere, cone, ..., `vector`, `color`) |
| `curve(pos=pts, color=color.cyan, radius=0.05)` | Draws a smooth 3D line through the list of positions |
| `sphere(pos=..., radius=1, color=color.red)` | Solid sphere centred at `pos` |
| `cone(pos=..., axis=..., radius=..., color=...)` | Cone whose **axis vector** sets height & direction |
| `arrow(pos=..., axis=..., shaftwidth=..., color=...)` | Arrow from `pos` along `axis`; `shaftwidth` = thickness |
| `ring(pos=..., axis=..., radius=..., thickness=...)` | A flat torus/ring in the plane perpendicular to `axis` |
| `cylinder(pos=..., axis=..., radius=..., color=...)` | Solid cylinder along `axis` |
| `while True:` | Menu repeats until user exits |
| `if choice == '1': draw_curve()` | Dispatch to the chosen function |

> **Note on `pos` and `axis`:** `pos` is where the object's base/centre is, and `axis` is the vector along which it extends. For example `cylinder(pos=(-1,0,0), axis=(0,3,0))` starts at (−1,0,0) and grows 3 units in the +y direction.

### 6. Dry Run (Menu Interaction)

```
---------- 3D OBJECT MENU ----------
1. Curve
2. Sphere
3. Cone
4. Arrow
5. Ring
6. Cylinder
7. Exit
Enter your choice (1-7): 2
Sphere created.        ← a red sphere appears at the origin

Enter your choice (1-7): 5
Ring created.          ← a magenta ring appears

Enter your choice (1-7): 7
Exiting the program. Goodbye!
```

### 7. Visual Results of Each Object

| Choice | Object | Typical appearance |
|---|---|---|
| 1 | Curve | Cyan wavy line through the 5 points |
| 2 | Sphere | Red ball of radius 1 at origin |
| 3 | Cone | Orange cone pointing up 2 units |
| 4 | Arrow | Green arrow from (−2,0,0) to (2,2,0) |
| 5 | Ring | Magenta flat ring lying horizontally |
| 6 | Cylinder | Yellow column rising 3 units |

### 8. Edge Cases & Environment Notes

| Case | Behaviour / Note |
|---|---|
| Running in a terminal (not Jupyter) | VPython 7 opens a browser window to render the 3D scene |
| Running in Jupyter notebook | The 3D scene renders **inline** in the notebook |
| Old `visual` module | Use `from visual import *` (Python 2 / old environments) — same object names |
| Not installed | Install with `pip install vpython` |
| Multiple objects in one run | Each menu choice adds another object to the same `scene` |
| Invalid choice | Handled by the `else` branch |

### 9. Common Mistakes to Avoid

1. **`import visual` instead of `from vpython import *`** — then you must prefix everything (`visual.sphere(...)`).
2. **Forgetting `vector()`** — VPython objects expect `vector` positions, not plain tuples (tuples often work, but `vector` is explicit and correct).
3. **Cone/cylinder with wrong `axis`** — the `axis` controls height and orientation; using `vector(0,0,0)` gives zero height (invisible).
4. **Ring thickness too large/small** — set a visible `thickness` (e.g., 0.1).
5. **No menu loop** — a menu-driven program must keep prompting until exit.
6. **Comparing `choice` (string) to integers** — compare with `'1'`, not `1`.

### 10. Alternative Version — Old Visual Python (for legacy labs)

```python
from visual import *

def main():
    while True:
        print("\n1.Curve 2.Sphere 3.Cone 4.Arrow 5.Ring 6.Cylinder 7.Exit")
        ch = input("Choice: ")

        if ch == '1':
            curve(pos=[(-2,0,0), (-1,1,0), (0,0,0), (1,-1,0), (2,0,0)], color=color.cyan)
        elif ch == '2':
            sphere(pos=(0,0,0), radius=1, color=color.red)
        elif ch == '3':
            cone(pos=(0,0,0), axis=(0,2,0), radius=1, color=color.orange)
        elif ch == '4':
            arrow(pos=(-2,0,0), axis=(4,2,0), shaftwidth=0.2, color=color.green)
        elif ch == '5':
            ring(pos=(0,0,0), axis=(0,1,0), radius=1.5, thickness=0.1, color=color.magenta)
        elif ch == '6':
            cylinder(pos=(-1,0,0), axis=(0,3,0), radius=0.6, color=color.yellow)
        elif ch == '7':
            print("Goodbye!")
            break
        else:
            print("Invalid choice!")

main()
```

### 11. Brief Result Table (for the lab record)

| Menu option | 3D object | VPython call |
|---|---|---|
| 1 | Curve | `curve(pos=[...], color=...)` |
| 2 | Sphere | `sphere(pos=..., radius=..., color=...)` |
| 3 | Cone | `cone(pos=..., axis=..., radius=..., color=...)` |
| 4 | Arrow | `arrow(pos=..., axis=..., shaftwidth=..., color=...)` |
| 5 | Ring | `ring(pos=..., axis=..., radius=..., thickness=..., color=...)` |
| 6 | Cylinder | `cylinder(pos=..., axis=..., radius=..., color=...)` |
| 7 | — | Exit |

---
