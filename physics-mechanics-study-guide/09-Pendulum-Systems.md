# 09. Pendulum Systems

## Table of Contents
1. [Core Pendulum Model](#core-pendulum-model)
2. [Comparison Table of 14 Pendulum Cases](#comparison-table-of-14-pendulum-cases)
3. [How to Choose the Right Model](#how-to-choose-the-right-model)
4. [15 Worked Examples](#15-worked-examples)
5. [Common Mistakes](#common-mistakes)

---

## Core Pendulum Model

For a simple pendulum of length $L$ and small angular displacement $\theta$:

$$\tau = -mgL\sin\theta$$

For small angles, $\sin\theta \approx \theta$ (in radians), so motion becomes SHM:

$$\theta'' + \frac{g}{L}\theta = 0$$
$$\omega = \sqrt{\frac{g}{L}}$$
$$T = 2\pi\sqrt{\frac{L}{g}}$$
$$f = \frac{1}{2\pi}\sqrt{\frac{g}{L}}$$

### Small-Angle Warning
These formulas are accurate only for small amplitudes, typically below about $10^\circ$ to $15^\circ$.

---

## Comparison Table of 14 Pendulum Cases

| # | Pendulum Case | What Happens | Main Equation(s) | Use It When |
|---|---|---|---|---|
| 1 | Simple pendulum, small angle | SHM in angle | $T=2\pi\sqrt{L/g}$ | standard exam pendulum |
| 2 | Large-angle pendulum | not exact SHM | period increases slightly | conceptual correction case |
| 3 | Pendulum released from rest | starts at max angle | $\theta(0)=\theta_{max}$ | turning-point start |
| 4 | Pendulum passes equilibrium | max speed at bottom | energy conservation | speed/tension at lowest point |
| 5 | Pendulum with given speed at bottom | convert KE to height | $\frac12 mv^2 = mg\Delta h$ | rise after launch |
| 6 | Clock pendulum | period setting | $T\propto\sqrt{L}$ | timing and design |
| 7 | Changing gravity | period changes with $g$ | $T=2\pi\sqrt{L/g}$ | Moon/Earth comparison |
| 8 | Changing length | period changes with $L$ | $T\propto\sqrt{L}$ | shorter/longer pendulum comparison |
| 9 | Pendulum in accelerating frame | effective gravity changes | $g_{eff}$ idea | elevator or vehicle problems |
| 10 | Conical pendulum | bob moves in a horizontal circle | use tension components | uniform circular motion with string angle |
| 11 | Ballistic pendulum | collision then swing | momentum then energy | bullet-block style problems |
| 12 | Physical pendulum | rigid body swings about pivot | $T=2\pi\sqrt{I/(mgd)}$ | rod/plate pendulum |
| 13 | Damped pendulum | amplitude decreases | qualitative energy loss | real-life oscillation |
| 14 | Pendulum with tension question | tension differs by position | radial force equation | speed/tension at bottom or side |

---

## How to Choose the Right Model

| If the question asks for... | Start with... |
|---|---|
| period of a simple pendulum | $T=2\pi\sqrt{L/g}$ |
| speed at the bottom or top | energy conservation |
| tension in the string | radial force balance |
| collision plus swing | momentum first, then energy |
| horizontal circular path | conical pendulum equations |
| rigid body oscillating | physical pendulum formula |
| elevator/accelerating frame | replace $g$ with effective gravity |

---

## 15 Worked Examples

### Example 1 — Simple Pendulum Period
A pendulum has length $1.0\,\text{m}$ with $g=9.8\,\text{m/s}^2$.

$$T = 2\pi\sqrt{\frac{1.0}{9.8}} \approx 2.01\,\text{s}$$

### Example 2 — Frequency
Using the same pendulum:

$$f = \frac{1}{T} \approx 0.50\,\text{Hz}$$

### Example 3 — Length for a 2 s Period
Find the length needed for $T=2\,\text{s}$.

$$2 = 2\pi\sqrt{\frac{L}{9.8}}$$
$$L = \frac{9.8}{\pi^2} \approx 0.99\,\text{m}$$

### Example 4 — Moon vs Earth
For the same length, compare periods on Earth and Moon, where $g_{moon}=1.62\,\text{m/s}^2$.

$$\frac{T_{moon}}{T_{earth}} = \sqrt{\frac{9.8}{1.62}} \approx 2.46$$

The pendulum is much slower on the Moon.

### Example 5 — Speed at Bottom from Release Angle
A pendulum of length $2\,\text{m}$ is released from $30^\circ$.

Height drop:

$$\Delta h = L(1-\cos\theta) = 2(1-\cos 30^\circ) \approx 0.268\,\text{m}$$

Energy conservation:

$$mg\Delta h = \frac12 mv^2$$
$$v = \sqrt{2g\Delta h} \approx 2.30\,\text{m/s}$$

### Example 6 — Maximum Angle from Bottom Speed
A bob passes the lowest point at $4\,\text{m/s}$. How high can it rise?

$$\frac12 mv^2 = mgh$$
$$h = \frac{4^2}{2(10)} = 0.8\,\text{m}$$

### Example 7 — Tension at the Bottom
A $0.5\,\text{kg}$ bob on a $1.2\,\text{m}$ string moves at $3\,\text{m/s}$ at the lowest point.

$$T - mg = \frac{mv^2}{L}$$
$$T = mg + \frac{mv^2}{L} = 0.5(10) + \frac{0.5(9)}{1.2} = 8.75\,\text{N}$$

### Example 8 — Tension at the Side Position
At angle $\theta$, the radial equation is

$$T - mg\cos\theta = \frac{mv^2}{L}$$

If $m=1\,\text{kg}$, $L=2\,\text{m}$, $v=2\,\text{m/s}$, and $\theta=60^\circ$:

$$T = \frac{1(4)}{2} + 10\cos 60^\circ = 2 + 5 = 7\,\text{N}$$

### Example 9 — Clock Correction by Length Change
If a pendulum clock runs too fast, should you lengthen or shorten the pendulum?

Since $T \propto \sqrt{L}$, increase $L$ to increase the period and slow the clock.

### Example 10 — Conical Pendulum Speed Relation
For a conical pendulum, tension components give

$$T\cos\theta = mg, \qquad T\sin\theta = \frac{mv^2}{r}$$

Dividing:

$$\tan\theta = \frac{v^2}{rg}$$

Use this when the bob moves in a horizontal circle at constant height.

### Example 11 — Conical Pendulum Example
A bob moves in a circle of radius $0.5\,\text{m}$ and the string makes $30^\circ$ with the vertical.

$$v^2 = rg\tan\theta = 0.5(10)\tan 30^\circ \approx 2.89$$
$$v \approx 1.70\,\text{m/s}$$

### Example 12 — Ballistic Pendulum Speed After Collision
A $0.02\,\text{kg}$ bullet embeds in a $1.98\,\text{kg}$ block, and the combined mass swings upward by $0.20\,\text{m}$.

After collision, energy gives speed just after impact:

$$\frac12 (2.00)V^2 = (2.00)(10)(0.20)$$
$$V = 2\,\text{m/s}$$

### Example 13 — Ballistic Pendulum Bullet Speed
Now use momentum during collision:

$$m_b v_b = (m_b + M)V$$
$$0.02 v_b = 2.00(2) = 4$$
$$v_b = 200\,\text{m/s}$$

### Example 14 — Physical Pendulum
A uniform rod of length $L$ pivoted at one end has

$$I = \frac13 mL^2, \qquad d = \frac{L}{2}$$

So

$$T = 2\pi\sqrt{\frac{I}{mgd}} = 2\pi\sqrt{\frac{(1/3)mL^2}{mg(L/2)}} = 2\pi\sqrt{\frac{2L}{3g}}$$

### Example 15 — Effective Gravity in an Elevator
If an elevator accelerates upward with acceleration $a$, replace gravity by

$$g_{eff} = g + a$$

Then

$$T = 2\pi\sqrt{\frac{L}{g+a}}$$

So an upward-accelerating elevator makes the pendulum oscillate faster.

---

## Common Mistakes
- Using small-angle formulas for very large swings.
- Forgetting that pendulum displacement is angular, not linear, in the basic model.
- Solving ballistic pendulum problems with energy during the collision; use momentum during impact, energy after impact.
- Confusing conical pendulum motion with back-and-forth SHM.
