# 04. Newton's Laws

## Table of Contents
1. [Newton's Three Laws](#newtons-three-laws)
2. [Free-Body Diagrams](#free-body-diagrams)
3. [Common Force Cases](#common-force-cases)
4. [Worked Examples](#worked-examples)

---

## Newton's Three Laws

### First Law
An object remains at rest or in uniform motion unless acted on by a net external force.

### Second Law

$$\sum \vec{F} = m\vec{a}$$

### Third Law
For every force, there is an equal and opposite force on another object.

---

## Free-Body Diagrams

### Standard Forces

| Force | Symbol | Direction |
|---|---|---|
| Weight | $mg$ | downward |
| Normal | $N$ | perpendicular to surface |
| Tension | $T$ | along rope/string |
| Friction | $f$ | opposes actual or impending motion |
| Spring force | $F_s=-kx$ | toward equilibrium |

### FBD Procedure
1. Isolate one object.
2. Draw only forces on that object.
3. Choose axes.
4. Write $\sum F_x = ma_x$ and $\sum F_y = ma_y$.

---

## Common Force Cases

| Case | Key Equation | What to Watch |
|---|---|---|
| Horizontal, no friction | $F=ma$ | only net horizontal force matters |
| Horizontal with friction | $F-f=ma$ | choose static or kinetic friction correctly |
| Inclined plane | $mg\sin\theta$ along slope | use parallel/perpendicular axes |
| Connected masses | same $a$ in ideal rope | write one equation per mass |
| Circular motion | $\sum F_r = mv^2/r$ | centripetal is not a new force |
| Equilibrium | $\sum F=0$ | acceleration is zero |

### Friction

$$f_s \le \mu_s N, \qquad f_{s,\max} = \mu_s N, \qquad f_k = \mu_k N$$

### Inclined Plane Components

$$mg_{\parallel} = mg\sin\theta, \qquad mg_{\perp} = mg\cos\theta$$

---

## Worked Examples

### Example 1 — Box on a Frictionless Surface
A $5\,\text{kg}$ box is pushed by $20\,\text{N}$.

$$a = \frac{20}{5} = 4\,\text{m/s}^2$$

### Example 2 — Static Friction Check
A $5\,\text{kg}$ box rests on a horizontal surface with $\mu_s=0.4$. An external force of $15\,\text{N}$ is applied.

$$N = mg = 5(10) = 50\,\text{N}$$
$$f_{s,\max} = 0.4(50) = 20\,\text{N}$$

Since $15 < 20$, the box does not move.

### Example 3 — Kinetic Friction
Using the same box, now apply $25\,\text{N}$ with $\mu_k=0.3$.

$$f_k = 0.3(50) = 15\,\text{N}$$
$$F_{\text{net}} = 25 - 15 = 10\,\text{N}$$
$$a = \frac{10}{5} = 2\,\text{m/s}^2$$

### Example 4 — Block on an Incline
A block slides down a $30^\circ$ incline without friction.

$$a = g\sin 30^\circ = 10(0.5) = 5\,\text{m/s}^2$$

### Example 5 — Atwood-Like System
A $3\,\text{kg}$ block on a frictionless table is connected to a hanging $2\,\text{kg}$ mass.

Driving force: $2g = 20\,\text{N}$

$$a = \frac{20}{3+2} = 4\,\text{m/s}^2$$
$$T = m_1 a = 3(4) = 12\,\text{N}$$

### Example 6 — Centripetal Force
A $1000\,\text{kg}$ car turns on a flat curve of radius $50\,\text{m}$ at $15\,\text{m/s}$.

$$F_c = \frac{mv^2}{r} = \frac{1000(15^2)}{50} = 4500\,\text{N}$$

---

## Problem-Solving Reminders
- Static friction is not always equal to $\mu_s N$; it adjusts up to that maximum.
- Tension is the same only for ideal massless ropes and frictionless pulleys.
- “Centripetal force” means the net inward force, not an extra force added to the diagram.
