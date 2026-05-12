# 03. Motion in 2D

## Table of Contents
1. [Vector Decomposition](#vector-decomposition)
2. [Projectile Motion](#projectile-motion)
3. [Circular Motion](#circular-motion)
4. [Relative Motion](#relative-motion)
5. [Worked Examples](#worked-examples)

---

## Vector Decomposition

Treat horizontal and vertical motion independently.

$$v_{0x} = v_0\cos\theta, \qquad v_{0y} = v_0\sin\theta$$

This is the main idea behind 2D kinematics.

---

## Projectile Motion

### Core Equations

Horizontal motion:

$$x = v_{0x}t, \qquad v_x = v_{0x}$$

Vertical motion:

$$y = y_0 + v_{0y}t - \frac12 gt^2$$
$$v_y = v_{0y} - gt$$

### Main Projectile Cases

| Case | What Makes It Special | Best Starting Equation |
|---|---|---|
| Launched from ground, lands at same level | symmetric path | range/time formulas |
| Horizontal launch | $v_{0y}=0$ | vertical fall time first |
| Launched from a height | landing level differs | solve vertical motion first |
| Targeting a specific point | both $x$ and $y$ matter | eliminate $t$ or solve simultaneously |

### Standard Results for Same Launch and Landing Level

$$T = \frac{2v_0\sin\theta}{g}$$
$$R = \frac{v_0^2\sin(2\theta)}{g}$$
$$h_{\max} = \frac{v_0^2\sin^2\theta}{2g}$$

---

## Circular Motion

### Angular and Linear Relations

$$v = \omega r$$
$$a_c = \frac{v^2}{r} = \omega^2 r$$
$$T = \frac{2\pi}{\omega}, \qquad f = \frac1T$$

### Cases

| Case | Constant? | Key Acceleration |
|---|---|---|
| Uniform circular motion | speed constant | centripetal only |
| Non-uniform circular motion | speed changes | centripetal + tangential |

Centripetal acceleration always points toward the center.

---

## Relative Motion

Relative velocity is a vector difference.

$$\vec{v}_{A/B} = \vec{v}_A - \vec{v}_B$$

### Common Uses
- boats crossing rivers
- airplanes in wind
- moving walkways
- one vehicle observed from another

---

## Worked Examples

### Example 1 — Standard Projectile
A ball is launched at $20\,\text{m/s}$ and $45^\circ$ with $g=10\,\text{m/s}^2$.

$$R = \frac{20^2\sin 90^\circ}{10} = 40\,\text{m}$$
$$h_{\max} = \frac{20^2\sin^2 45^\circ}{20} = 10\,\text{m}$$
$$T = \frac{2(20)\sin 45^\circ}{10} \approx 2.83\,\text{s}$$

### Example 2 — Horizontal Launch
A projectile leaves a table at $15\,\text{m/s}$ from height $20\,\text{m}$.

Vertical motion:

$$20 = \frac12 (10)t^2 \Rightarrow t = 2\,\text{s}$$

Horizontal motion:

$$x = 15(2) = 30\,\text{m}$$

### Example 3 — Centripetal Acceleration
A car moves on a circular track of radius $50\,\text{m}$ at $20\,\text{m/s}$.

$$a_c = \frac{20^2}{50} = 8\,\text{m/s}^2$$

### Example 4 — Relative Velocity in One Line
Car A moves at $25\,\text{m/s}$ east, car B at $15\,\text{m/s}$ east.

$$v_{A/B} = 25 - 15 = 10\,\text{m/s east}$$

### Example 5 — Boat Crossing a River
A boat moves $4\,\text{m/s}$ north relative to water while the river flows $3\,\text{m/s}$ east.

Ground speed magnitude:

$$v = \sqrt{4^2 + 3^2} = 5\,\text{m/s}$$

Direction:

$$\theta = \tan^{-1}\left(\frac{3}{4}\right) \approx 36.9^\circ \text{ east of north}$$
