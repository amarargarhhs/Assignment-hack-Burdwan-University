# 9. WAP to Display Graphs of the Three Equations of Motion

### 1. Problem Understanding

Using the standard **equations of motion** for uniform acceleration:

| Eq | Formula | Meaning | What we plot |
|---|---|---|---|
| I | **v = u + at** | final velocity after time t | v vs t (straight line) |
| II | **s = ut + ½at²** | displacement after time t | s vs t (parabola) |
| III | **s = (v² − u²) / (2a)** | displacement from initial to final velocity | s vs v (parabola) |

where:
- **u** = initial velocity (m/s),
- **v** = final velocity (m/s),
- **a** = acceleration (m/s²),
- **t** = time (s),
- **s** = displacement (m).

**Note on equation III:** the question writes `s=(v*v-u*u)/2*a`. With operator precedence this parses as `((v²−u²)/2)*a` — **wrong**. The correct physical formula divides by **2a**:

> **s = (v² − u²) / (2a)**

The corrected form is used below.

**Sample values used for plotting:** u = 0 m/s, a = 2 m/s², t from 0 to 10 s, v from 0 to 20 m/s.

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| `numpy` | `np.linspace`, `np.sqrt` for arrays |
| `v = u + a*t` | Linear equation I |
| `s = u*t + 0.5*a*t**2` | Quadratic equation II |
| `s = (v**2 - u**2) / (2*a)` | Equation III |
| `plt.subplot(1, 3, i)` | Three graphs side by side |
| `plt.plot(...)` | Each motion graph |

### 3. Algorithm (Step-by-Step)

1. Import `numpy as np` and `matplotlib.pyplot as plt`.
2. Choose **u = 0**, **a = 2**.
3. **Graph 1 (v vs t):** `t1 = np.linspace(0, 10)`; `v = u + a*t1`; plot.
4. **Graph 2 (s vs t):** `s2 = u*t1 + 0.5*a*t1**2`; plot.
5. **Graph 3 (s vs v):** `v3 = np.linspace(0, 20)`; `s3 = (v3**2 - u**2)/(2*a)`; plot.
6. Add titles/labels to each subplot, `plt.tight_layout()`, `plt.show()`.

### 4. The Python Program (Source Code)

```python
# Graphs of the three equations of motion (u = 0, a = 2)

import numpy as np
import matplotlib.pyplot as plt

u = 0.0      # initial velocity (m/s)
a = 2.0      # acceleration (m/s^2)

def v_after_t(t):
    """Eq I: v = u + a*t"""
    return u + a * t

def s_after_t(t):
    """Eq II: s = u*t + 0.5*a*t^2"""
    return u * t + 0.5 * a * t**2

def s_from_v(v):
    """Eq III: s = (v^2 - u^2) / (2*a)"""
    return (v**2 - u**2) / (2 * a)

def main():
    t = np.linspace(0, 10, 200)      # time 0..10 s
    v = np.linspace(0, 20, 200)      # velocity 0..20 m/s

    plt.figure(figsize=(12, 4))

    # Graph I — v vs t
    plt.subplot(1, 3, 1)
    plt.plot(t, v_after_t(t), 'b-')
    plt.xlabel("Time t (s)")
    plt.ylabel("Velocity v (m/s)")
    plt.title("I: v = u + at")
    plt.grid(True, linestyle='--', alpha=0.5)

    # Graph II — s vs t
    plt.subplot(1, 3, 2)
    plt.plot(t, s_after_t(t), 'r-')
    plt.xlabel("Time t (s)")
    plt.ylabel("Displacement s (m)")
    plt.title("II: s = ut + (1/2)at^2")
    plt.grid(True, linestyle='--', alpha=0.5)

    # Graph III — s vs v
    plt.subplot(1, 3, 3)
    plt.plot(v, s_from_v(v), 'g-')
    plt.xlabel("Velocity v (m/s)")
    plt.ylabel("Displacement s (m)")
    plt.title("III: s = (v^2 - u^2)/(2a)")
    plt.grid(True, linestyle='--', alpha=0.5)

    plt.tight_layout()
    plt.show()

if __name__ == "__main__":
    main()
```

### 5. Line-by-Line Explanation

| Code | What it does |
|---|---|
| `u = 0.0`, `a = 2.0` | Constants for a uniformly accelerating object from rest |
| `def v_after_t(t): return u + a * t` | Function for equation I (linear in t) |
| `def s_after_t(t): return u*t + 0.5*a*t**2` | Function for equation II (quadratic in t) |
| `def s_from_v(v): return (v**2 - u**2)/(2*a)` | Function for equation III (quadratic in v) |
| `np.linspace(0, 10, 200)` | 200 time samples 0…10 s |
| `np.linspace(0, 20, 200)` | 200 velocity samples 0…20 m/s |
| `plt.figure(figsize=(12, 4))` | Wide figure to fit three side-by-side graphs |
| `plt.subplot(1, 3, 1)` | 1 row, 3 columns; activates the 1st subplot |
| `plt.plot(t, v_after_t(t), 'b-')` | Draws the blue straight line v = 2t |
| `plt.tight_layout()` | Prevents overlapping titles/labels |
| `plt.show()` | Displays all three graphs |

### 6. Dry Run (Analytical Values with u = 0, a = 2)

| t (s) | v = 2t (m/s) | s = t² (m) | v (m/s) | s = v²/4 (m) |
|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 0 |
| 2 | 4 | 4 | 4 | 4 |
| 5 | 10 | 25 | 10 | 25 |
| 10 | 20 | 100 | 20 | 100 |

**Shapes:**
- **I:** straight line through the origin, slope = a = 2.
- **II:** upward parabola s = t².
- **III:** upward parabola s = v²/4.

> **Check (consistency):** at t = 5, equation II gives s = 25 m; equation III with v = 10 gives s = (100−0)/(2·2) = 25 m. ✓ The three graphs agree.

### 7. Sample Output

Three side-by-side graphs:
1. **I: v = u + at** — blue straight line from (0,0) to (10,20).
2. **II: s = ut + ½at²** — red parabola from (0,0) reaching 100 m at t = 10.
3. **III: s = (v²−u²)/(2a)** — green parabola from (0,0) reaching 100 m at v = 20.

### 8. Edge Cases & Notes

| Case | Behaviour / Note |
|---|---|
| u = 0 (from rest) | All three graphs pass through the origin — used in the sample |
| u ≠ 0 | Graph I starts at v = u; graphs II/III shift accordingly |
| a = 0 | Constant velocity — equation III is undefined (division by zero); graph I is horizontal |
| a < 0 (deceleration) | v decreases with t; curves slope downward |
| Large t | Displacement grows quadratically — values get big fast |

### 9. Common Mistakes to Avoid

1. **Precedence error in equation III** — `(v*v - u*u)/2*a` computes `((v²−u²)/2)×a`. The correct formula is `(v**2 - u**2) / (2*a)`.
2. **Using `^` instead of `**`** — `v^2` is XOR in Python, not squaring.
3. **`math` vs `numpy`** — use `np.linspace` and array operations, not `math` functions, for whole arrays.
4. **Plotting all three on one axis** — they have different variables and scales; subplots make them readable.
5. **Forgetting units in labels** — (m/s), (s), (m) are expected in a physics lab record.
6. **Not using functions** — the lab requires user-defined functions for the formulas.

### 10. Alternative Version — All Three on One Graph (s and v against t)

```python
import numpy as np
import matplotlib.pyplot as plt

u, a = 0.0, 2.0
t = np.linspace(0, 10, 200)

v = u + a * t
s = u * t + 0.5 * a * t**2

plt.plot(t, v, 'b-', label='v = u + at')
plt.plot(t, s, 'r-', label='s = ut + 0.5at^2')
plt.xlabel("Time t (s)")
plt.ylabel("v (m/s) or s (m)")
plt.title("Equations of Motion (u=0, a=2)")
plt.legend()
plt.grid(True, linestyle='--', alpha=0.5)
plt.show()
```

### 11. Brief Result Table (for the lab record)

| Quantity | t = 2 s | t = 5 s | t = 10 s |
|---|---|---|---|
| v = u + at | 4 m/s | 10 m/s | 20 m/s |
| s = ut + ½at² | 4 m | 25 m | 100 m |
| s = (v²−u²)/(2a) | 4 m | 25 m | 100 m |

**Observation:** velocity grows linearly with time, while displacement grows quadratically; all three equations give consistent values.

---
