# Chapter 2
## Error, Convergence, and Stability in Root-Finding Algorithms

A central theme in numerical analysis is understanding **how numerical algorithms behave in the presence of approximation error**.  
When solving equations numerically we rarely obtain the exact solution. Instead, we generate **iterative approximations** that converge toward the true root.

In this chapter, I investigate and code several classical **root-finding algorithms**:

- Bisection Method
- Fixed Point Iteration
- Newton’s Method
- Secant Method

For each algorithm, the focus is not only on computing a root, but also on examining:

- **Convergence behaviour**
- **Propagation of numerical error**
- **Algorithmic stability**
- **Computational efficiency**

Understanding these aspects helps distinguish **well-conditioned numerical problems from unstable or poorly behaved ones**.

---

# Bisection Method

The **Bisection Method** is one of the most stable and reliable root-finding algorithms.  
It works by repeatedly halving an interval that contains a root.

The method requires a **sign change condition**:

$$
f(a)f(b) < 0
$$

This guarantees that a root exists in the interval \([a,b]\) by the **Intermediate Value Theorem**.

The algorithm reduces the interval width by a factor of **2 at each iteration**, meaning the **error decreases linearly**.


Below is my implementation using a `while` loop.

```python
def Bisection_method(f , a , b , tol=1e-7):

  error = abs(b - a)

  if f(a) * f(b) >= 0:
      print("this methord isn't appropiate")
      quit()

  i = 1

  while error / 2 > tol:
    c = (b + a) / 2

    if f(a) * f(c) <= 0:
      b = c
      error = abs(b - a)
      i = i + 1
      print(i)
    else:
      a = c
      error = abs(b - a)
      i = i + 1
      print(i)

  return c

```
Approximate root: 3.155607581138611

Because the interval is halved at each step, the error satisfies:

$$|e_{k+1}| \approx \frac{1}{2} |e_k|$$

This predictable reduction makes the bisection method numerically stable, although it converges relatively slowly compared to other methods.

## Analysing Error Convergence

To better understand the behaviour of the algorithm, we can analyse how the error decreases between iterations. 

The following code plots:

$$\log_2(e_{k+1}) \quad \text{vs} \quad \log_2(e_k)$$

which reveals the order of convergence.

Here is my plotting code:

```python

import numpy as np
import matplotlib.pyplot as plt

def plot_error_progression(errors):
    # Calculating the log2 of the absolute error at step k and k+1
    log_errors = np.log2(errors)
    log_errors_k = log_errors[:-1]  # log errors at step k (excluding the last one)
    log_errors_k_plus_1 = log_errors[1:]  # log errors at step k+1 (excluding the first one)

    # Plotting log_errors_k+1 vs log_errors_k
    plt.scatter(log_errors_k, log_errors_k_plus_1, label='Log Error at k+1 vs Log Error at k')

    # Fitting a straight line to the data points
    slope, intercept = np.polyfit(log_errors_k, log_errors_k_plus_1, deg=1)
    best_fit_line = slope * log_errors_k + intercept
    plt.plot(log_errors_k, best_fit_line, color='red', label='Best Fit Line')
    print("slope is:", slope)
    print("intercept is:", intercept)

    # Setting up the plot
    plt.xlabel('Log2 of Absolute Error at Step k')
    plt.ylabel('Log2 of Absolute Error at Step k+1')
    plt.title('Log2 of Absolute Error at Step k+1 vs Step k')
    plt.legend()
    plt.show()

plot_error_progression([0.08088022903976166, 0.014213562373095012, 0.00042058396836819334, 2.1238982250704197e-06, 3.157747396898003e-10])

```

output: 

" slope is: 1.6459984522406022
intercept is: -0.5513844534356405 "

Image:

<img width="574" height="455" alt="image" src="https://github.com/user-attachments/assets/e43e656a-42d9-4121-b6bf-89141724a95f" />

A slope of $\approx 1.65$ indicates that the method converges faster than a linear method (where $p=1$) but slower than a quadratic method (where $p=2$, such as Newton's Method).

This analysis provides insight into how rapidly errors shrink between iterations and allows us to empirically observe the rate of convergence.

### Bisection Method with Fixed Iterations
The number of iterations required to achieve a given tolerance can be predicted mathematically:

$$N \ge \log_2 \left(\frac{b-a}{\text{tol}}\right)$$

This leads to a version implemented using a `for` loop.

```python

def Bisection_method(f , a , b , tol=1e-4):

  error = abs(b - a)
  count = math.ceil((math.log2((abs(b-a))/ tol)))

  if f(a) * f(b) >= 0:
      print("this methord isn't appropiate")
      quit()

  for i in range (0,count):
    c = (b + a) / 2

    if f(a) * f(c) <= 0:
      b = c
      error = abs(b - a)
    else:
      a = c
      error = abs(b - a)
  return c


def f(x):
    return 3*(math.sin(x)) + 9 - x**2 - math.cos(x)

import math

root = Bisection_method(f, 1, 5)
print("Approximate root:", root)

```
Output: Approximate root: 3.15557861328125

This version highlights how error bounds can be predicted analytically, which is a key concept in numerical analysis.

### Fixed Point Iteration
Fixed point iteration solves equations of the form:

$$x = g(x)$$

The algorithm repeatedly evaluates:

$$x_{n+1} = g(x_n)$$

Convergence depends heavily on the derivative of $g(x)$:

$$|g'(x)| < 1$$

If this condition is not satisfied, the iteration may diverge, demonstrating an example of **numerical instability**.

```python

import math

#defining the function we've created
def fixed_point_iteration(g, x0, tol, max_iter):
  x = x0
  for i in range (max_iter):
    y = g(x)
    x = y #left hand holds the new value, find the point on x = y where xn returns a y value
    print (x) #each time we go through this, it will switch
  return x

def g(x):
  return (0.9*x + 0.1)

root = fixed_point_iteration(g, 1.5, 2**-10, 22)
print("Approximate root:", root)

```

output: 1.4500000000000002
1.4050000000000002
1.3645000000000003
1.3280500000000004
1.2952450000000004
1.2657205000000005
1.2391484500000005
1.2152336050000005
1.1937102445000005
1.1743392200500007
1.1569052980450008
1.1412147682405007
1.1270932914164509
1.114383962274806
1.1029455660473255
1.092651009442593
1.0833859084983337
1.0750473176485005
1.0675425858836505
1.0607883272952854
1.054709494565757
1.0492385451091812
Approximate root: 1.0492385451091812

This sequence gradually converges toward the fixed point.

To prevent infinite loops in unstable cases, we add a stopping condition:

```python

import math

def fixed_point_iteration(g, x0, tol, max_iter):
  x = x0
  n = 0
  i = 1
  for i in range (max_iter):
    y = g(x)
    x = y
    i = i + 1
    if abs(y - x) < tol:
      return x
  else:
    print("iteration has not converged")

def g(x):
  return (math.cos(x))

root = fixed_point_iteration(g, 2, 10**-8, 22)
print("Approximate root:", root)

```

This reflects a common principle in numerical algorithms: **robust stopping criteria** are essential for stability.

### Newton's Method

Newton’s method is significantly faster than bisection or fixed point iteration because it uses derivative information. 

The update formula is:

$$x_{n+1} = x_n - \frac{f(x_n)}{f'(x_n)}$$



When close to the root, Newton’s method exhibits **quadratic convergence**, meaning the number of correct digits roughly doubles each iteration.


```python

import math

def Newton(f, fprime, x0, tol=1e-12):
  x = x0
  for i in range (0,30):
    x_new = x - f(x)/(fprime(x))
    if abs(x_new - x) < tol:
      return x_new
    x = x_new
  else:
    print("iteration has not converged")

def f(x):
  return (x**2 - 2)

def fprime(x):
  return (2*x)

root = Newton(f, fprime, 1, 10**-12)
print("Approximate root:", root)

```
output:
Approximate root: 1.414213562373095

However, Newton's method can become numerically unstable if:

the derivative is close to zero

the initial guess is poor

This motivates adding safeguards.

Newton’s method for approximating a root that checks for a dervitative = 0

```python
import math

def Newton(f, fprime, x0, tol=1e-12):
  x = x0
  while abs(f(x)) > 10**-4:
    x_new = x - f(x)/(fprime(x))
    if abs(x_new - x) < tol:
      return x_new
    if fprime(x) == 0:
      print("derivative is 0")
    x = x_new
  else:
    print("iteration has not converged")

def f(x):
  return (x**2 - 4)

def fprime(x):
  return (2*x)

root = Newton(f, fprime, 5, 10**-12)
print("Approximate root:", root)
```

Output: 

Iteration has not converged
Approximate root: None

This prevents division by zero and highlights how numerical algorithms must guard against unstable conditions.

### Tracking Error in Newton's Method

To study convergence behaviour, we record the absolute error between successive iterates (max 30 iterations).

```python

import math
import numpy as np

def Newton_with_error_tracking(f, fprime, x0, tol=1e-12):

  x = x0

  list_errors = []

  for i in range (0,30):
    if fprime(x) == 0:
      raise ValueError ("derivative is 0")

    x_new = x - f(x)/(fprime(x))
    current_error = abs(x_new - x)
    list_errors.append (current_error)

    if current_error < tol:
      return [x_new, list_errors]

    x = x_new

  else:
    print("iteration has not converged")

def f(x):
  return (x**2 - 4)

def fprime(x):
  return (2*x)

[root, list_errors] = Newton_with_error_tracking(f, fprime, 3, 10**-4)
print("Approximate root:", root)
print("List of errors:", list_errors)

```
output: Approximate root: 2.000000000026214
List of errors: [0.8333333333333335, 0.16025641025641013, 0.006400016384041862, 1.0240000000383276e-05]

Tracking error provides insight into how quickly the algorithm converges, allowing us to compare methods quantitatively.

---

## Secant Method

The **Secant Method** approximates Newton’s method but avoids the necessity of computing derivatives. Instead, it approximates the derivative using the slope between two previous points.

### Iteration Formula
Given two initial guesses, $x_{n}$ and $x_{n-1}$, the next approximation $x_{n+1}$ is calculated as:

$$x_{n+1} = x_n - f(x_n) \frac{x_n - x_{n-1}}{f(x_n) - f(x_{n-1})}$$



### Comparison
* **Performance:** Typically faster than the **Bisection Method** due to a higher order of convergence (approximately $1.618$).
* **Stability:** Slightly less stable than **Newton's Method**, as it depends on two initial points and can diverge if the function is not well-behaved.


```python

import math

def Secant(f, a, b, iter):


  for i in range (0, iter):
    if f(a) == f(b):
      raise ValueError ("f(a) = f(b)")
    if (f(b) - f(a)) == 0:
      raise ValueError ("f(b) - f(a) = 0")

    x_new = b - (f(b)*(b - a))/(f(b) - f(a))
    a = b
    b = x_new
  return x_new

def f(x):
  return (math.log(x) - math.sin(x))


root = Secant(f, 1, 5, 5)
print("Approximate root:", root)

```

Approximate root: 2.2191071499595116

Secant Method with tracking

```python

import math

def Secant_with_list(f, a, b, iter):

  list_errors = []

  for i in range (0, iter):
    if f(a) == f(b):
      raise ValueError ("f(a) = f(b)")
    if (f(b) - f(a)) == 0:
      raise ValueError ("f(b) - f(a) = 0")

    x_new = b - (f(b)*(b - a))/(f(b) - f(a))
    a = b
    b = x_new

    current_error = abs(x_new - 2**(1/2))
    list_errors.append (current_error)

  return [x_new, list_errors]

def f(x):
  return (x**2 - 2)


[root, list_errors] = Secant_with_list(f, 1, 2, 5)
print("Approximate root:", root)
print("List of errors:", list_errors)

```
Approximate root: 1.4142135620573204
List of errors: [0.08088022903976166, 0.014213562373095012, 0.00042058396836819334, 2.1238982250704197e-06, 3.157747396898003e-10]


