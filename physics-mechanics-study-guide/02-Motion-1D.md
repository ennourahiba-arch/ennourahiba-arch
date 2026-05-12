# 02. Motion in 1D

## Table of Contents
1. [Core Quantities](#core-quantities)
2. [Constant Velocity](#constant-velocity)
3. [Constant Acceleration](#constant-acceleration)
4. [Free Fall](#free-fall)
5. [Motion Graphs](#motion-graphs)
6. [Worked Examples](#worked-examples)

---

## Core Quantities

| Quantity | Symbol | Meaning |
|---|---|---|
| Position | $x$ | location on an axis |
| Displacement | $\Delta x$ | $x_f - x_i$ |
| Velocity | $v$ | rate of change of position |
| Speed | $|v|$ | magnitude of velocity |
| Acceleration | $a$ | rate of change of velocity |

### Sign Convention
Choose one positive direction and keep it throughout the problem.

---

## Constant Velocity

When $a = 0$:

$$x = x_0 + vt$$

### Use This Case When
- speed and direction stay constant
- no acceleration is given
- velocity-time graph is flat

### Example
A drone moves at $12\,\text{m/s}$ for $15\,\text{s}$.

$$x - x_0 = vt = 12 \times 15 = 180\,\text{m}$$

---

## Constant Acceleration

When $a$ is constant:

$$v = v_0 + at$$
$$x = x_0 + v_0 t + \frac12 at^2$$
$$v^2 = v_0^2 + 2a(x-x_0)$$
$$x = x_0 + \frac{v+v_0}{2}t$$

### Which Formula Should You Use?

| Known / Unknown Pattern | Best Equation |
|---|---|
| Need final velocity and know time | $v = v_0 + at$ |
| Need displacement and know time | $x = x_0 + v_0 t + \tfrac12 at^2$ |
| Time not given | $v^2 = v_0^2 + 2a\Delta x$ |
| Average velocity approach | $\Delta x = \tfrac{v+v_0}{2}t$ |

### Common Cases
1. Starting from rest: $v_0 = 0$
2. Braking: $a < 0$
3. Speeding up in positive direction: $a > 0$
4. Moving negative but slowing down: signs must be tracked carefully

---

## Free Fall

Take upward as positive unless stated otherwise.

$$a = -g$$
$$v = v_0 - gt$$
$$y = y_0 + v_0 t - \frac12 gt^2$$
$$v^2 = v_0^2 - 2g(y-y_0)$$

### Use This Case When
- gravity is the only force affecting vertical motion
- air resistance is ignored

### Standard Subcases

| Case | Initial Condition | Useful Fact |
|---|---|---|
| Dropped object | $v_0=0$ | starts from rest |
| Thrown upward | $v_0>0$ | at top: $v=0$ |
| Thrown downward | $v_0<0$ if up is positive | acceleration still $-g$ |

---

## Motion Graphs

| Graph | Slope | Area |
|---|---|---|
| $x$ vs $t$ | velocity | not usually used |
| $v$ vs $t$ | acceleration | displacement |
| $a$ vs $t$ | not usually used | change in velocity |

---

## Worked Examples

### Example 1 — Constant Velocity
A train moves at $30\,\text{m/s}$ for $10\,\text{s}$.

$$\Delta x = vt = 30 \times 10 = 300\,\text{m}$$

### Example 2 — Accelerating from Rest
A car starts from rest and accelerates at $2\,\text{m/s}^2$ for $5\,\text{s}$.

$$x = 0 + 0 + \frac12(2)(5^2) = 25\,\text{m}$$
$$v = 0 + 2(5) = 10\,\text{m/s}$$

### Example 3 — Braking Distance
A bicycle slows from $14\,\text{m/s}$ to rest with $a = -2\,\text{m/s}^2$.

$$0 = 14^2 + 2(-2)\Delta x$$
$$\Delta x = 49\,\text{m}$$

### Example 4 — Ball Thrown Upward
A ball is thrown upward at $20\,\text{m/s}$. Find maximum height using $g=10\,\text{m/s}^2$.

At the top, $v=0$.

$$0 = 20^2 - 2(10)h$$
$$h = 20\,\text{m}$$

### Example 5 — Dropped from a Height
A stone is dropped from $45\,\text{m}$.

$$45 = \frac12 (10)t^2$$
$$t^2 = 9 \Rightarrow t = 3\,\text{s}$$

---

## Quick Error Checks
- If an object stops, set $v=0$ at that instant.
- Use the same sign convention in every equation.
- If time is not given, prefer $v^2 = v_0^2 + 2a\Delta x$.
