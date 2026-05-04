# Solving Homogeneous Linear ODEs with Constant Coefficients

## Table of Contents
1. [Introduction](#introduction)
2. [Characteristic Equation](#characteristic-equation)
3. [Three Cases Based on Discriminant](#three-cases-based-on-discriminant)
4. [Theorem 6.2.1 - Complete Solution](#theorem-621---complete-solution)
5. [Linear Independence](#linear-independence)

---

## Introduction

A **homogeneous linear ODE of 2nd order with constant coefficients** has the form:

$$a_2 y''(t) + a_1 y'(t) + a_0 y(t) = 0$$

Where:
- $a_2, a_1, a_0$ are **constant coefficients** (real numbers)
- $a_2 \neq 0$
- $y(t)$ is the unknown function
- **Homogeneous** means the right side equals **0**

The goal is to find **all solutions** $y(t)$.

---

## Characteristic Equation

### Why Exponential Solutions?

The key insight is that we propose a solution of the form:

$$y(t) = e^{\lambda t}$$

where $\lambda$ is a constant to be determined.

### Computing Derivatives

If $y(t) = e^{\lambda t}$, then:
- $y'(t) = \lambda e^{\lambda t}$
- $y''(t) = \lambda^2 e^{\lambda t}$

### Deriving the Characteristic Equation

Substitute $y(t) = e^{\lambda t}$ into the homogeneous ODE:

$$a_2 (\lambda^2 e^{\lambda t}) + a_1 (\lambda e^{\lambda t}) + a_0 (e^{\lambda t}) = 0$$

Factor out $e^{\lambda t}$ (which is never zero):

$$e^{\lambda t} (a_2 \lambda^2 + a_1 \lambda + a_0) = 0$$

Since $e^{\lambda t} \neq 0$:

$$\boxed{a_2 \lambda^2 + a_1 \lambda + a_0 = 0}$$

This is the **characteristic equation**.

---

## Three Cases Based on Discriminant

The **discriminant** of the characteristic equation is:

$$\Delta = a_1^2 - 4a_2a_0$$

The sign of $\Delta$ determines the nature of the roots and the form of the general solution.

---

### **CASE 1: $\Delta > 0$ (Two Distinct Real Roots)**

#### Condition
The discriminant is **strictly positive**: $\Delta > 0$

#### Finding the Roots
$$\lambda_{1,2} = \frac{-a_1 \pm \sqrt{\Delta}}{2a_2}$$

You get **two different real roots**: $\lambda_1 \neq \lambda_2$

#### General Solution
$$\boxed{y(t) = c_1 e^{\lambda_1 t} + c_2 e^{\lambda_2 t}}$$

where $c_1, c_2$ are arbitrary constants.

#### Example
$$y'' - 5y' + 6y = 0$$

**Characteristic equation:**
$$\lambda^2 - 5\lambda + 6 = 0$$

**Coefficients:** $a_2 = 1, a_1 = -5, a_0 = 6$

**Calculate discriminant:**
$$\Delta = (-5)^2 - 4(1)(6) = 25 - 24 = 1 > 0$$ ✓

**Roots:**
$$\lambda = \frac{5 \pm 1}{2} \implies \lambda_1 = 2, \lambda_2 = 3$$

**General solution:**
$$y(t) = c_1 e^{2t} + c_2 e^{3t}$$

---

### **CASE 2: $\Delta = 0$ (One Repeated Real Root)**

#### Condition
The discriminant equals **zero**: $\Delta = 0$

#### Finding the Root
$$\lambda = \frac{-a_1}{2a_2}$$

You get **one repeated real root** (double root): $\lambda_1 = \lambda_2 = \lambda$

#### Why the Form Changes
If we only had $y_1(t) = e^{\lambda t}$, we would get:
$$y(t) = (c_1 + c_2) e^{\lambda t}$$

But this has only **one arbitrary constant**, not two! For a 2nd order ODE, we need **two independent solutions**.

The second solution is $y_2(t) = t e^{\lambda t}$

#### General Solution
$$\boxed{y(t) = (c_1 + c_2 t) e^{\lambda t}}$$

where $c_1, c_2$ are arbitrary constants.

#### Example
$$y'' - 4y' + 4y = 0$$

**Characteristic equation:**
$$\lambda^2 - 4\lambda + 4 = 0$$

**Coefficients:** $a_2 = 1, a_1 = -4, a_0 = 4$

**Calculate discriminant:**
$$\Delta = (-4)^2 - 4(1)(4) = 16 - 16 = 0$$ ✓

**Root (double):**
$$\lambda = \frac{4}{2} = 2$$

**General solution:**
$$y(t) = (c_1 + c_2 t) e^{2t}$$

**Note:** Both $e^{2t}$ and $t e^{2t}$ are solutions!

---

### **CASE 3: $\Delta < 0$ (Two Complex Conjugate Roots)**

#### Condition
The discriminant is **negative**: $\Delta < 0$

#### Finding the Roots
$$\lambda_{1,2} = \frac{-a_1 \pm i\sqrt{|\Delta|}}{2a_2}$$

where $i = \sqrt{-1}$

The roots are **complex conjugates**: $\lambda = \alpha \pm i\beta$

where:
- $\alpha = \frac{-a_1}{2a_2}$ (real part)
- $\beta = \frac{\sqrt{|\Delta|}}{2a_2}$ (imaginary part)

#### Converting to Real Solutions
Complex exponentials $e^{(\alpha + i\beta)t}$ can be converted to real trigonometric functions using **Euler's formula**:

$$e^{i\theta} = \cos(\theta) + i\sin(\theta)$$

#### General Solution
$$\boxed{y(t) = e^{\alpha t}(c_1 \cos(\beta t) + c_2 \sin(\beta t))}$$

where $c_1, c_2$ are arbitrary constants.

#### Example
$$y'' + 2y' + 5y = 0$$

**Characteristic equation:**
$$\lambda^2 + 2\lambda + 5 = 0$$

**Coefficients:** $a_2 = 1, a_1 = 2, a_0 = 5$

**Calculate discriminant:**
$$\Delta = 2^2 - 4(1)(5) = 4 - 20 = -16 < 0$$ ✓

**Roots (complex):**
$$\lambda = \frac{-2 \pm \sqrt{-16}}{2} = \frac{-2 \pm 4i}{2} = -1 \pm 2i$$

**Extract real and imaginary parts:**
- $\alpha = -1$
- $\beta = 2$

**General solution:**
$$y(t) = e^{-t}(c_1 \cos(2t) + c_2 \sin(2t))$$

**Physical interpretation:** This is a **damped oscillation** (the exponential $e^{-t}$ decays as $t \to \infty$).

---

## Theorem 6.2.1 - Complete Solution

### Theorem Statement

**Theorem 6.2.1 (Solutions to Homogeneous Equations with Constant Coefficients)**

Let $c_1, c_2 \in \mathbb{R}$ be two arbitrary constants and let $\lambda_1, \lambda_2$ be the roots of the characteristic equation.

Then any solution to 
$$a_2 y''(t) + a_1 y'(t) + a_0 y(t) = 0$$

fulfills exactly one of the following cases:

- **If** $\lambda_1, \lambda_2 \in \mathbb{R}$ **with** $\lambda_1 \neq \lambda_2$:
$$y(t) = c_1 e^{\lambda_1 t} + c_2 e^{\lambda_2 t}$$

- **If** $\lambda_1, \lambda_2 \in \mathbb{R}$ **with** $\lambda_1 = \lambda_2 = \lambda$:
$$y(t) = (c_1 + tc_2) e^{\lambda t}$$

- **If** $\lambda_1, \lambda_2 \in \mathbb{C}$ **then** $\lambda_1, \lambda_2 = \alpha \pm i\beta$ with $\alpha, \beta \in \mathbb{R}$:
$$y(t) = c_1 e^{\alpha t} \cos(\beta t) + c_2 e^{\alpha t} \sin(\beta t)$$

### Existence and Uniqueness for Initial Value Problems

**Moreover**, for every fixed $t_0, y_0, y_1 \in \mathbb{R}$, there exists a **unique solution** to the initial value problem:

$$\begin{cases}
a_2 y''(t) + a_1 y'(t) + a_0 y(t) = 0 \\
y(t_0) = y_0 \\
y'(t_0) = y_1
\end{cases}$$

#### What This Means

1. **Existence:** A solution always exists
2. **Uniqueness:** The solution is unique (one and only one)
3. **General constants:** The constants $c_1$ and $c_2$ are uniquely determined by the initial conditions

#### Example: Using Initial Conditions

Solve:
$$\begin{cases}
y'' - 5y' + 6y = 0 \\
y(0) = 1 \\
y'(0) = 0
\end{cases}$$

**Step 1: General solution** (from Case 1)
$$y(t) = c_1 e^{2t} + c_2 e^{3t}$$

**Step 2: Apply $y(0) = 1$**
$$y(0) = c_1 + c_2 = 1 \quad \cdots (i)$$

**Step 3: Compute derivative**
$$y'(t) = 2c_1 e^{2t} + 3c_2 e^{3t}$$

**Step 4: Apply $y'(0) = 0$**
$$y'(0) = 2c_1 + 3c_2 = 0 \quad \cdots (ii)$$

**Step 5: Solve the system**

From $(i)$: $c_1 = 1 - c_2$

Substitute into $(ii)$: $2(1 - c_2) + 3c_2 = 0$
$$2 - 2c_2 + 3c_2 = 0$$
$$c_2 = -2, \quad c_1 = 3$$

**Step 6: Unique particular solution**
$$\boxed{y(t) = 3e^{2t} - 2e^{3t}}$$

✓ This is the **unique** solution satisfying the ODE and initial conditions.

---

## Linear Independence

### Definition

Two functions $y_1(t)$ and $y_2(t)$ are **linearly independent** if there do **not exist** constants $c_1, c_2$ (not both zero) such that:

$$c_1 y_1(t) + c_2 y_2(t) = 0 \quad \text{for all } t$$

In other words, one function **cannot be written as a constant multiple** of the other.

### Why Linear Independence Matters

For a 2nd order ODE, we need:
- **Two solutions** $y_1(t)$ and $y_2(t)$
- That are **linearly independent**

Only then can we form the general solution:
$$y(t) = c_1 y_1(t) + c_2 y_2(t)$$

with two **arbitrary constants** that can be adjusted to any initial conditions.

### Testing Linear Independence: The Wronskian

The **Wronskian** of two functions is:

$$W(y_1, y_2) = \begin{vmatrix} y_1 & y_2 \\ y_1' & y_2' \end{vmatrix} = y_1 y_2' - y_2 y_1'$$

#### Decision Rule
- **If $W \neq 0$** → functions are **linearly independent** ✓
- **If $W = 0$** → functions are **linearly dependent** ✗

### Example 1: Linearly Independent Functions

**Functions:** $y_1(t) = e^{2t}$ and $y_2(t) = e^{3t}$

**Derivatives:**
$$y_1'(t) = 2e^{2t}, \quad y_2'(t) = 3e^{3t}$$

**Wronskian:**
$$W = e^{2t} \cdot 3e^{3t} - e^{3t} \cdot 2e^{2t} = 3e^{5t} - 2e^{5t} = e^{5t}$$

**Is $W \neq 0$?** Yes! $e^{5t} \neq 0$ for all $t$.

**Conclusion:** $e^{2t}$ and $e^{3t}$ are **linearly independent** ✓

### Example 2: Linearly Dependent Functions

**Functions:** $y_1(t) = e^{2t}$ and $y_2(t) = 5e^{2t}$

**Notice:** $y_2(t) = 5 \cdot y_1(t)$ (one is a constant multiple of the other)

**Derivatives:**
$$y_1'(t) = 2e^{2t}, \quad y_2'(t) = 10e^{2t}$$

**Wronskian:**
$$W = e^{2t} \cdot 10e^{2t} - 5e^{2t} \cdot 2e^{2t} = 10e^{4t} - 10e^{4t} = 0$$

**Is $W = 0$?** Yes!

**Conclusion:** $e^{2t}$ and $5e^{2t}$ are **linearly dependent** ✗

### Example 3: Case 2 (Double Root)

**Functions:** $y_1(t) = e^{2t}$ and $y_2(t) = te^{2t}$

**Derivatives:**
$$y_1'(t) = 2e^{2t}$$
$$y_2'(t) = e^{2t} + 2te^{2t}$$

**Wronskian:**
$$W = e^{2t}(e^{2t} + 2te^{2t}) - te^{2t} \cdot 2e^{2t}$$
$$= e^{4t} + 2te^{4t} - 2te^{4t}$$
$$= e^{4t}$$

**Is $W \neq 0$?** Yes! $e^{4t} \neq 0$ for all $t$.

**Conclusion:** $e^{2t}$ and $te^{2t}$ are **linearly independent** ✓

### Example 4: Case 3 (Complex Roots)

**Functions:** $y_1(t) = e^{-t}\cos(2t)$ and $y_2(t) = e^{-t}\sin(2t)$

**Derivatives:**
$$y_1'(t) = -e^{-t}\cos(2t) - 2e^{-t}\sin(2t)$$
$$y_2'(t) = -e^{-t}\sin(2t) + 2e^{-t}\cos(2t)$$

**Wronskian:**
$$W = e^{-t}\cos(2t)[-e^{-t}\sin(2t) + 2e^{-t}\cos(2t)]$$
$$\quad - e^{-t}\sin(2t)[-e^{-t}\cos(2t) - 2e^{-t}\sin(2t)]$$

After simplification:
$$W = 2e^{-2t}$$

**Is $W \neq 0$?** Yes! $2e^{-2t} \neq 0$ for all $t$.

**Conclusion:** $e^{-t}\cos(2t)$ and $e^{-t}\sin(2t)$ are **linearly independent** ✓

### Key Observation for Our Three Cases

In all three cases from Theorem 6.2.1, the Wronskian is **always non-zero**!

| Case | Solutions | Wronskian | Independence |
|---|---|---|---|
| Case 1 | $e^{\lambda_1 t}, e^{\lambda_2 t}$ | $\neq 0$ | ✓ Independent |
| Case 2 | $e^{\lambda t}, te^{\lambda t}$ | $\neq 0$ | ✓ Independent |
| Case 3 | $e^{\alpha t}\cos(\beta t), e^{\alpha t}\sin(\beta t)$ | $\neq 0$ | ✓ Independent |

This guarantees that we can always form a general solution with two arbitrary constants!

---

## Summary: Decision Tree

```
Homogeneous ODE: a₂y'' + a₁y' + a₀y = 0
                           ↓
                  Characteristic Eq: a₂λ² + a₁λ + a₀ = 0
                           ↓
              Calculate Δ = a₁² - 4a₂a₀
                           ↓
          ┌─────────────────┼─────────────────┐
          ↓                 ↓                 ↓
       Δ > 0            Δ = 0             Δ < 0
   Two real roots    One double root   Complex conjugate
      λ₁ ≠ λ₂            λ = λ             λ = α ± iβ
          ↓                 ↓                 ↓
    y = c₁e^(λ₁t)    y = (c₁ + c₂t)e^(λt)  y = e^(αt)[c₁cos(βt) + c₂sin(βt)]
      + c₂e^(λ₂t)
```

---

## Quick Reference Table

| Aspect | Case 1 ($\Delta > 0$) | Case 2 ($\Delta = 0$) | Case 3 ($\Delta < 0$) |
|---|---|---|---|
| **Roots** | Two distinct real | One double real | Two complex conjugate |
| **Roots** | $\lambda_1 \neq \lambda_2$ | $\lambda = \lambda_1 = \lambda_2$ | $\alpha \pm i\beta$ |
| **Solution 1** | $e^{\lambda_1 t}$ | $e^{\lambda t}$ | $e^{\alpha t}\cos(\beta t)$ |
| **Solution 2** | $e^{\lambda_2 t}$ | $te^{\lambda t}$ | $e^{\alpha t}\sin(\beta t)$ |
| **General Form** | $c_1e^{\lambda_1 t} + c_2e^{\lambda_2 t}$ | $(c_1 + c_2 t)e^{\lambda t}$ | $e^{\alpha t}(c_1\cos(\beta t) + c_2\sin(\beta t))$ |
| **Wronskian** | $e^{(\lambda_1+\lambda_2)t}(\lambda_2-\lambda_1)$ | $e^{2\lambda t}$ | $\beta e^{2\alpha t}$ |
| **Independence** | ✓ Yes | ✓ Yes | ✓ Yes |

---

## Additional Notes

- The **characteristic equation** is the algebraic equation obtained by substituting $y = e^{\lambda t}$ into the ODE
- The **discriminant** determines which case applies
- **Linear independence** is guaranteed by the Wronskian being non-zero
- **Theorem 6.2.1** guarantees that solutions exist and are unique for any initial conditions
- Initial conditions (Cauchy problem) uniquely determine the constants $c_1$ and $c_2$
