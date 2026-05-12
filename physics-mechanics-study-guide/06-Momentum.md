# 06. Momentum

## Table of Contents
1. [Linear Momentum](#linear-momentum)
2. [Impulse](#impulse)
3. [Conservation of Momentum](#conservation-of-momentum)
4. [Collisions and Explosions](#collisions-and-explosions)
5. [Center of Mass](#center-of-mass)
6. [Worked Examples](#worked-examples)

---

## Linear Momentum

$$\vec{p} = m\vec{v}$$

Momentum is a vector.

---

## Impulse

$$\vec{J} = \Delta \vec{p} = \vec{F}_{avg}\Delta t$$

### Use Impulse When
- collision time is given
- average force is requested
- force acts over a short interval

---

## Conservation of Momentum

For an isolated system:

$$\sum \vec{p}_{before} = \sum \vec{p}_{after}$$

### Collision Types

| Type | Momentum Conserved? | Kinetic Energy Conserved? | Objects Stick? |
|---|---|---|---|
| Elastic | Yes | Yes | No |
| Inelastic | Yes | No | Sometimes |
| Perfectly inelastic | Yes | No | Yes |
| Explosion | Yes | No requirement | Objects separate |

---

## Collisions and Explosions

### 1D Elastic Shortcut
For two objects in 1D, use momentum plus kinetic energy conservation.

### Perfectly Inelastic Shortcut
If objects stick together:

$$v_f = \frac{m_1v_{1i} + m_2v_{2i}}{m_1+m_2}$$

### Explosion from Rest
If the system starts from rest:

$$\sum \vec{p}_{after} = 0$$

---

## Center of Mass

$$x_{cm} = \frac{\sum m_ix_i}{\sum m_i}, \qquad v_{cm} = \frac{\sum m_iv_i}{\sum m_i}$$

---

## Worked Examples

### Example 1 — Momentum
A $4\,\text{kg}$ cart moves at $3\,\text{m/s}$.

$$p = 4(3) = 12\,\text{kg·m/s}$$

### Example 2 — Impulse Force
A $0.15\,\text{kg}$ baseball changes velocity from $-40$ to $+50\,\text{m/s}$ in $0.01\,\text{s}$.

$$\Delta p = 0.15(50-(-40)) = 13.5\,\text{kg·m/s}$$
$$F_{avg} = \frac{13.5}{0.01} = 1350\,\text{N}$$

### Example 3 — Perfectly Inelastic Collision
A $1000\,\text{kg}$ car at $20\,\text{m/s}$ hits an identical parked car and they stick.

$$v_f = \frac{1000(20)}{2000} = 10\,\text{m/s}$$

### Example 4 — Explosion
A $100\,\text{kg}$ fragment moves backward at $40\,\text{m/s}$ after separation from a $900\,\text{kg}$ section. Initial momentum is zero.

$$0 = 100(-40) + 900v$$
$$v = 4.44\,\text{m/s forward}$$

### Example 5 — Center of Mass
Two masses: $2\,\text{kg}$ at $x=1\,\text{m}$ and $3\,\text{kg}$ at $x=5\,\text{m}$.

$$x_{cm} = \frac{2(1)+3(5)}{5} = 3.4\,\text{m}$$

---

## Common Mistakes
- Momentum conservation applies to the whole isolated system, not necessarily to each object.
- Kinetic energy is not conserved in every collision.
- Momentum direction matters; assign signs carefully.
