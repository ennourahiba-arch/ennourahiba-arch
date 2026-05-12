# 01. Measurements

## Table of Contents
1. [SI Units](#si-units)
2. [Scalars and Vectors](#scalars-and-vectors)
3. [Vector Components](#vector-components)
4. [Dimensional Analysis](#dimensional-analysis)
5. [Uncertainty and Significant Figures](#uncertainty-and-significant-figures)
6. [Worked Examples](#worked-examples)

---

## SI Units

### Base Quantities

| Quantity | Symbol | SI Unit | Unit Symbol |
|---|---|---|---|
| Length | $L$ | meter | m |
| Mass | $m$ | kilogram | kg |
| Time | $t$ | second | s |
| Electric current | $I$ | ampere | A |
| Temperature | $T$ | kelvin | K |
| Amount of substance | $n$ | mole | mol |
| Luminous intensity | $I_v$ | candela | cd |

To avoid notation confusion, keep in mind that $I$ and $I_v$ are **different symbols**: $I$ denotes electric current, while $I_v$ specifically denotes **luminous intensity**.

### Common Derived Units in Mechanics

| Quantity | Formula | SI Unit |
|---|---|---|
| Velocity | $v = \Delta x/\Delta t$ | m/s |
| Acceleration | $a = \Delta v/\Delta t$ | m/s$^2$ |
| Force | $F = ma$ | N = kg·m/s$^2$ |
| Work / Energy | $W = Fd$ | J = N·m |
| Power | $P = W/t$ | W = J/s |
| Momentum | $p = mv$ | kg·m/s |
| Pressure | $P = F/A$ | Pa = N/m$^2$ |

---

## Scalars and Vectors

### Scalars
Scalars have magnitude only.

Examples: mass, time, temperature, distance, speed, energy.

### Vectors
Vectors have magnitude and direction.

Examples: displacement, velocity, acceleration, force, momentum.

### Vector Notation
- Magnitude: $|\vec{A}|$
- Components in 2D: $\vec{A} = A_x\hat{i} + A_y\hat{j}$
- Magnitude from components:

$$|\vec{A}| = \sqrt{A_x^2 + A_y^2}$$

---

## Vector Components

If a vector $\vec{A}$ makes angle $\theta$ with the positive $x$-axis:

$$A_x = A\cos\theta, \qquad A_y = A\sin\theta$$

### Typical Use Cases

| Situation | Best Representation |
|---|---|
| Inclined plane | Parallel and perpendicular components |
| Projectile motion | Horizontal and vertical components |
| Multiple forces | Resolve each force into $x$ and $y$ |
| Relative motion | Subtract velocity vectors |

---

## Dimensional Analysis

Dimensional analysis checks whether an equation is dimensionally consistent.

### Base Symbols
- Length: $[L]$
- Mass: $[M]$
- Time: $[T]$

### Useful Dimensions

| Quantity | Dimensions |
|---|---|
| Velocity | $[LT^{-1}]$ |
| Acceleration | $[LT^{-2}]$ |
| Force | $[MLT^{-2}]$ |
| Energy | $[ML^2T^{-2}]$ |
| Momentum | $[MLT^{-1}]$ |

### Example Check
Is $v^2 = 2ax$ dimensionally correct?

Left side:

$$[v^2] = [LT^{-1}]^2 = [L^2T^{-2}]$$

Right side:

$$[ax] = [LT^{-2}][L] = [L^2T^{-2}]$$

So the equation is dimensionally valid.

---

## Uncertainty and Significant Figures

### Absolute and Relative Uncertainty

If a length is measured as $L = 2.50 \pm 0.02\,\text{m}$:
- Absolute uncertainty: $0.02\,\text{m}$
- Relative uncertainty:

$$\frac{0.02}{2.50} = 0.008 = 0.8\%$$

### Quick Rules
- Add/subtract: match decimal places.
- Multiply/divide: match significant figures.
- Final physics answer should not claim more precision than the data.

---

## Worked Examples

### Example 1 — Average Speed
A robot moves $120\,\text{m}$ in $8\,\text{s}$. Find average speed.

$$v_{\text{avg}} = \frac{120}{8} = 15\,\text{m/s}$$

### Example 2 — Vector Components
A force of $50\,\text{N}$ acts at $37^\circ$ above horizontal.

$$F_x = 50\cos 37^\circ \approx 40\,\text{N}$$
$$F_y = 50\sin 37^\circ \approx 30\,\text{N}$$

### Example 3 — Dimensional Check
Check whether $x = vt + \tfrac12 at^2$ has correct dimensions.

$$[vt] = [LT^{-1}][T] = [L]$$
$$[at^2] = [LT^{-2}][T^2] = [L]$$

Both terms have dimensions of length, so the equation is consistent.

### Example 4 — Unit Conversion
Convert $72\,\text{km/h}$ to m/s.

$$72\times \frac{1000}{3600} = 20\,\text{m/s}$$

---

## Checklist Before Solving Any Mechanics Problem
- Use SI units.
- Label every vector direction.
- Convert angles and units before substitution.
- Check dimensions after deriving the formula.
- Round only at the end.
