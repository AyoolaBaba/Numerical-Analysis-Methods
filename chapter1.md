# Numerical Analysis Study
## Chapter 1: The Foundations of Numerical Approximation

### 1.1 Introduction: Why Numerical Analysis?
In pure mathematics, we operate in a continuum of real numbers ($\mathbb{R}$). However, digital computers are finite systems that store data in discrete bits. This study explores **Numerical Analysis**—the discipline of performing efficient calculations while managing the inevitable errors that arise from hardware limitations.

Computer errors generally fall into two categories:
* **Rounding Errors:** Inherent inaccuracies resulting from approximating real numbers with binary floating-point representations.
* **Truncation Errors:** Errors introduced by approximating infinite processes (like power series) with finite operations.

---
## 1.2 Proof of Rounding Error Bound
To understand why calculations fail, we must quantify the "gap" between a real number $x$ and its floating-point representation $fl(x)$. 

### The Model
For any real number $x$ within the range of a floating-point system, the representation follows:
$$fl(x) = x(1 + \delta)$$
where $\delta$ is the relative error.

### The Proof
1.  **Machine Epsilon ($\epsilon_{mach}$):** In a binary system with $p$ bits of precision, the distance between 1 and the next representable power of 2 is $2^{1-p}$.
2.  **Rounding to Nearest:** When a computer rounds to the nearest floating-point number, the absolute error $|fl(x) - x|$ is at most half the distance between two consecutive representable numbers (the Unit in the Last Place).
3.  **Relative Bound:** Therefore, the relative error $\delta$ is bounded by:
$$|\delta| = \left| \frac{fl(x) - x}{x} \right| \leq \frac{1}{2} \beta^{1-p}$$
This $\frac{1}{2} \beta^{1-p}$ is defined as the **Machine Epsilon**, the fundamental limit of precision for the system.



## 1.3 Empirical Study: Error Propagation in Loops
To demonstrate how these infinitesimal rounding errors can lead to catastrophic failure, I conducted an experiment using a simple iterative loop.

### Implementation
The following algorithm repeatedly multiplies a value by 2 and keeps it within the $(0, 1)$ range. Theoretically, for a starting value like $1.8$, the behavior should be predictable.

```python
# Showcasing how rounding errors produce incorrect answers
x = 1.8
print(f"Initial value: {x}")

for n in range(50):
    if x <= 0.5:        
        x = 2 * x      
    else:        
        x = 2 * x - 1      
    print(f"Iteration {n}: {x}")

```

## 1.4 Transition: From Error to Numerical Strategy

The divergence observed in the Bernoulli Map simulation in Section 1.3 serves as a foundational "case study" for why standard arithmetic fails in discrete systems. 

**The Bridge: From Error to Method**

The divergence observed in Section 1.3, where the value of **1.8** rapidly decays into numerical noise through a simple doubling map, serves as a stark warning: mathematical logic does not always translate directly into computational accuracy. Because $1.8$ cannot be represented exactly in binary, each iteration of $2x$ or $2x - 1$ acts as an amplifier for the hidden "rounding noise" in the least significant bits. By the 50th iteration, the original signal is completely lost to this floating-point drift. This phenomenon mandates the specialized **Numerical Methods** discussed in the following chapters. We move beyond "naive" arithmetic to explore **Iterative Stability**, **Error Propagation Analysis**, and **Pivoting Strategies**—tools specifically designed to maintain the integrity of a solution even when the underlying hardware is inherently limited. Without these methods, even the most elegant mathematical models remain vulnerable to the same catastrophic loss of precision demonstrated here.


