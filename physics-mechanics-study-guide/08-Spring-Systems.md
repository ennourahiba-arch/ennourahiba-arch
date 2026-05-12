# 08. Spring Systems

## Table of Contents
1. [Core Spring Model](#core-spring-model)
2. [Comparison Table of 14 Spring Cases](#comparison-table-of-14-spring-cases)
3. [How to Choose the Right Model](#how-to-choose-the-right-model)
4. [10 Worked Examples](#10-worked-examples)
5. [Common Mistakes](#common-mistakes)

---

## Core Spring Model

### Hooke's Law

$$F_s = -kx$$

The spring force points toward equilibrium.

### Spring Energy

$$U_s = \frac12 kx^2$$

### Simple Harmonic Motion for a Mass-Spring System

$$a = -\frac{k}{m}x$$
$$\omega = \sqrt{\frac{k}{m}}$$
$$T = 2\pi\sqrt{\frac{m}{k}}$$
$$f = \frac{1}{2\pi}\sqrt{\frac{k}{m}}$$

### Position, Velocity, Acceleration

$$x(t) = A\cos(\omega t + \phi)$$
$$v(t) = -A\omega\sin(\omega t + \phi)$$
$$a(t) = -\omega^2 x$$

---

## Comparison Table of 14 Spring Cases

| # | Spring Case | What Happens | Main Equation(s) | Use It When |
|---|---|---|---|---|
| 1 | Static stretch/compression | spring in equilibrium under applied load | $F=kx$ | finding deformation from a steady force |
| 2 | Horizontal frictionless spring-block | SHM about natural length | $\omega=\sqrt{k/m}$ | ideal oscillation on a table |
| 3 | Vertical spring at new equilibrium | SHM about shifted equilibrium | $mg = kx_{eq}$ and $\omega=\sqrt{k/m}$ | hanging mass on spring |
| 4 | Released from maximum compression | starts at turning point | $x(0)=A,\ v(0)=0$ | compressed then let go |
| 5 | Released from equilibrium with shove | starts with max speed | $x(0)=0$ | object crosses center with initial velocity |
| 6 | Energy conversion case | spring PE to KE/other PE | $\frac12 kx^2 \leftrightarrow \frac12 mv^2$ | speed from compression |
| 7 | Spring with friction | energy decreases each pass | $E_i + W_f = E_f$ | rough horizontal motion |
| 8 | Incline plus spring | gravity and spring both matter | $\frac12 kx^2 + mgh$ | launch or stop on a slope |
| 9 | Two springs in series | softer equivalent system | $\frac{1}{k_{eq}}=\frac{1}{k_1}+\frac{1}{k_2}$ | one mass attached through two springs |
| 10 | Two springs in parallel | stiffer equivalent system | $k_{eq}=k_1+k_2$ | both springs stretch equally |
| 11 | Block between two springs | net restoring force increases | $k_{eq}=k_1+k_2$ | centered mass attached on both sides |
| 12 | Spring launch problem | stored energy gives exit speed | $\frac12 kx^2 = \frac12 mv^2$ | launcher/trampoline-type setup |
| 13 | Spring cutoff / losing contact | spring acts only while compressed | split motion into contact and free phases | object leaves spring |
| 14 | Damped/forced overview | real oscillations lose or gain energy | qualitative: amplitude changes | identifying non-ideal behavior |

---

## How to Choose the Right Model

| If the question asks for... | Start with... |
|---|---|
| deformation under a steady load | $F=kx$ |
| period or frequency | $T=2\pi\sqrt{m/k}$ |
| maximum speed | energy or $v_{max}=\omega A$ |
| equilibrium extension in vertical case | $mg=kx_{eq}$ |
| combined springs | find $k_{eq}$ first |
| rough surface | energy with friction work |
| launch off a spring | split spring-contact phase from free-motion phase |

---

## 10 Worked Examples

### Example 1 — Static Stretch
A force of $30\,\text{N}$ stretches a spring by $0.15\,\text{m}$.

$$k = \frac{F}{x} = \frac{30}{0.15} = 200\,\text{N/m}$$

### Example 2 — Period of a Horizontal Spring
A $2\,\text{kg}$ mass is attached to a spring with $k=50\,\text{N/m}$.

$$T = 2\pi\sqrt{\frac{2}{50}} = 2\pi\sqrt{0.04} = 0.4\pi \approx 1.26\,\text{s}$$

### Example 3 — Maximum Speed
A spring-mass oscillator has amplitude $0.10\,\text{m}$, mass $0.5\,\text{kg}$, and $k=200\,\text{N/m}$.

$$\omega = \sqrt{\frac{200}{0.5}} = 20\,\text{rad/s}$$
$$v_{max} = \omega A = 20(0.10) = 2.0\,\text{m/s}$$

### Example 4 — Vertical Equilibrium Extension
A $1.5\,\text{kg}$ mass hangs from a spring with $k=300\,\text{N/m}$.

$$x_{eq} = \frac{mg}{k} = \frac{1.5(10)}{300} = 0.05\,\text{m}$$

### Example 5 — Energy from Compression
A spring with $k=200\,\text{N/m}$ is compressed by $0.30\,\text{m}$ and pushes a $2\,\text{kg}$ block on a frictionless surface.

$$\frac12 kx^2 = \frac12 mv^2$$
$$9 = v^2 \Rightarrow v = 3\,\text{m/s}$$

### Example 6 — Two Springs in Series
Two springs $k_1=100\,\text{N/m}$ and $k_2=300\,\text{N/m}$ are connected in series.

$$\frac1{k_{eq}} = \frac1{100} + \frac1{300} = \frac4{300}$$
$$k_{eq} = 75\,\text{N/m}$$

### Example 7 — Two Springs in Parallel
Two springs $k_1=100\,\text{N/m}$ and $k_2=300\,\text{N/m}$ are connected in parallel.

$$k_{eq} = 100 + 300 = 400\,\text{N/m}$$

### Example 8 — Block Between Two Springs
A block is attached between springs of $120\,\text{N/m}$ and $80\,\text{N/m}$.

$$k_{eq} = 120 + 80 = 200\,\text{N/m}$$

If $m=2\,\text{kg}$:

$$\omega = \sqrt{\frac{200}{2}} = 10\,\text{rad/s}$$

### Example 9 — Spring on an Incline
A spring launches a $1\,\text{kg}$ block up a frictionless incline. The spring constant is $200\,\text{N/m}$ and compression is $0.20\,\text{m}$. The block rises $0.30\,\text{m}$ vertically.

Initial spring energy:

$$U_s = \frac12 (200)(0.20)^2 = 4\,\text{J}$$

Gain in gravitational energy:

$$mgh = 1(10)(0.30) = 3\,\text{J}$$

Remaining kinetic energy:

$$K = 1\,\text{J}$$
$$\frac12 mv^2 = 1 \Rightarrow v = \sqrt{2}\,\text{m/s}$$

### Example 10 — Spring with Friction
A $1\,\text{kg}$ block compresses a spring ($k=100\,\text{N/m}$) by $0.40\,\text{m}$ on a surface with constant friction $2\,\text{N}$. Find the speed at equilibrium.

Initial spring energy:

$$U_s = \frac12 (100)(0.40)^2 = 8\,\text{J}$$

Work by friction over $0.40\,\text{m}$:

$$W_f = -fd = -2(0.40) = -0.8\,\text{J}$$

At equilibrium, all remaining mechanical energy is kinetic:

$$K = 8 - 0.8 = 7.2\,\text{J}$$
$$\frac12 (1)v^2 = 7.2 \Rightarrow v = \sqrt{14.4} \approx 3.79\,\text{m/s}$$

---

## Common Mistakes
- Measuring displacement from the wrong equilibrium point in a vertical spring.
- Forgetting that series springs reduce stiffness while parallel springs increase stiffness.
- Using SHM formulas when friction or loss of contact makes the motion non-ideal.
- Forgetting that the spring force changes with position, so constant-force formulas do not apply directly.
