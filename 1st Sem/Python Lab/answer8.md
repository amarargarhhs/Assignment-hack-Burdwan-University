# 8. WAP to Display the Curve P(t) = 15000(1+t) / (15 + eᵗ) — Bacteria Population Growth

### 1. Problem Understanding

The population **P(t)** of bacteria after time **t** follows the growth model:

> **P(t) = 15000 × (1 + t) / (15 + eᵗ)**

where:
- **P(t)** = population of bacteria,
- **t** = time (e.g. in hours),
- **eᵗ** = exponential function of time.

**Note:** the question text writes the denominator as `(15 + e)`, which is almost certainly a typo for `(15 + e^t)` — the term must involve **t**, otherwise the denominator is constant and the curve would be a straight line (not a growth curve). The correct standard model is used above: **eᵗ** in the denominator.

The task: plot **t (x-axis)** against **P(t) (y-axis)**.

**Behaviour of the curve:**
- At **t = 0**: P = 15000 × 1 / (15 + 1) = 15000/16 ≈ **937.5**.
- For small t the numerator grows fast (1 + t), so P rises.
- Eventually eᵗ grows faster than (1 + t), so P peaks and then falls — the classic **population crash** after resources run out.

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| `numpy` | `np.linspace` + `np.exp` for eᵗ |
| `np.exp(t)` | Computes eᵗ for every t in the array |
| `P = 15000 * (1 + t) / (15 + np.exp(t))` | Vectorized formula |
| `matplotlib.pyplot` | Plotting the curve |
| `plt.plot(t, P, ...)` | The t vs P graph |
| `plt.xlabel/ylabel/title` | Labels and title |

### 3. Algorithm (Step-by-Step)

1. Import `numpy as np` and `matplotlib.pyplot as plt`.
2. Create `t = np.linspace(0, 10, 400)` — time from 0 to 10 hours.
3. Compute `P = 15000 * (1 + t) / (15 + np.exp(t))`.
4. Plot `plt.plot(t, P)`.
5. Add axis labels, title, grid.
6. `plt.show()`.

### 4. The Python Program (Source Code)

```python
# Bacteria population growth: P(t) = 15000(1 + t) / (15 + e^t)

import numpy as np
import matplotlib.pyplot as plt

def population(t):
    """Return the bacteria population at time t."""
    return 15000 * (1 + t) / (15 + np.exp(t))

def main():
    t = np.linspace(0, 10, 400)      # time from 0 to 10 hours
    P = population(t)                # population at every t

    plt.plot(t, P, color='green', linewidth=2)
    plt.xlabel("Time t (hours)")
    plt.ylabel("Population P(t)")
    plt.title("Bacteria Population Growth: P(t) = 15000(1+t)/(15+e^t)")
    plt.grid(True, linestyle='--', alpha=0.6)
    plt.show()

if __name__ == "__main__":
    main()
```

### 5. Line-by-Line Explanation

| Code | What it does |
|---|---|
| `import numpy as np` | Array math library |
| `def population(t):` | User-defined function for the model |
| `15000 * (1 + t)` | Numerator — grows linearly with time |
| `15 + np.exp(t)` | Denominator — grows exponentially with time |
| `np.linspace(0, 10, 400)` | 400 sample times between 0 and 10 hours |
| `P = population(t)` | Vectorized call — returns 400 values at once |
| `plt.plot(t, P, color='green', ...)` | Green growth curve |
| `plt.grid(...)` | Dashed grid for readability |

### 6. Dry Run (Analytical Values)

| t | eᵗ | (1 + t) | Numerator | Denominator | P(t) |
|---|---|---|---|---|---|
| 0 | 1.00 | 1 | 15000 | 16 | **937.5** |
| 1 | 2.72 | 2 | 30000 | 17.72 | ≈ **1693** |
| 2 | 7.39 | 3 | 45000 | 22.39 | ≈ **2010** |
| 3 | 20.09 | 4 | 60000 | 35.09 | ≈ **1710** |
| 4 | 54.60 | 5 | 75000 | 69.60 | ≈ **1078** |
| 5 | 148.41 | 6 | 90000 | 163.41 | ≈ **551** |
| 7 | 1096.63 | 8 | 120000 | 1111.63 | ≈ **108** |
| 10 | 22026.47 | 11 | 165000 | 22041.47 | ≈ **7.5** |

The population **rises, peaks around t ≈ 2 (≈ 2000), then crashes** as the exponential term dominates.

### 7. Sample Output

A window titled **"Bacteria Population Growth: P(t) = 15000(1+t)/(15+e^t)"** shows a green curve that:
- Starts near 937.5,
- Climbs to a peak of about 2000 near t ≈ 2,
- Falls rapidly to near zero by t ≈ 10.

### 8. Edge Cases & Notes

| Case | Behaviour / Note |
|---|---|
| t = 0 | P = 15000/16 ≈ 937.5 (initial population) |
| Large t | eᵗ dominates → P → 0 (population dies out) |
| The typo `(15 + e)` in the question | Denominator constant 17.72 → straight line, not a growth curve; **the intended model is `15 + e^t`** |
| Negative t | Physically unusual for population over time |
| Peak location | dP/dt = 0 near t ≈ 2 (can be found analytically or numerically) |

### 9. Common Mistakes to Avoid

1. **Writing `e` instead of `np.exp(t)`** — the variable must be time-dependent; `np.exp(t)` is the correct eᵗ.
2. **Using `math.exp` on an array** — `math.exp` works only on a single number; use `np.exp` for arrays.
3. **Forgetting parentheses** — `15000 * (1 + t) / (15 + np.exp(t))`, not `15000 * 1 + t / ...`.
4. **Too small a t-range** — with t only up to 2 or 3 you'd miss the peak and the crash; use up to 8–10.
5. **Not labelling axes** — always label Time (x) and Population (y).

### 10. Alternative Version — Also Print the Peak

```python
import numpy as np
import matplotlib.pyplot as plt

t = np.linspace(0, 10, 1000)
P = 15000 * (1 + t) / (15 + np.exp(t))

peak_t = t[np.argmax(P)]        # time of the maximum population
print(f"Peak population ≈ {P.max():.0f} at t ≈ {peak_t:.2f} hours")

plt.plot(t, P, 'g-')
plt.xlabel("Time t (hours)")
plt.ylabel("Population P(t)")
plt.title("Bacteria Population Growth")
plt.grid(True, linestyle='--', alpha=0.6)
plt.show()
```

### 11. Brief Result Table (for the lab record)

| t | 0 | 1 | 2 | 3 | 4 | 5 | 7 | 10 |
|---|---|---|---|---|---|---|---|---|
| **P(t)** | ≈ 937 | ≈ 1693 | ≈ 2010 | ≈ 1710 | ≈ 1078 | ≈ 551 | ≈ 108 | ≈ 7 |

**Observation:** population grows to a maximum of about 2000 at t ≈ 2 hours, then declines to almost zero as eᵗ outgrows (1 + t) — a boom-and-bust growth pattern.

---
