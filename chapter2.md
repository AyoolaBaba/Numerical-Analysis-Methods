# Chapter 2
## Error, Convergence, and Stability in Root-Finding Algorithms

A central theme in numerical analysis is understanding **how numerical algorithms behave in the presence of approximation error**.  
When solving equations numerically we rarely obtain the exact solution. Instead, we generate **iterative approximations** that converge toward the true root.

This chapter investigates several classical **root-finding algorithms**:

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

\[
f(a)f(b) < 0
\]

This guarantees that a root exists in the interval \([a,b]\) by the **Intermediate Value Theorem**.

The algorithm reduces the interval width by a factor of **2 at each iteration**, meaning the **error decreases linearly**.

Below is an implementation using a `while` loop.

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
Bisection Method to approximate a root using the 'while loop'.

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

import math

def f(x):
    #return x**2 - 2 = "This methord is not appropiate" = correct
    #return math.sin(x) + x**2 - 2*(math.log(x)) - 5 = 2.495319150388241 = correct
    #return math.exp(x)*(5 - x) - 5 = 4.96511422842741 = correct
    return 3*(math.sin(x)) + 9 - x**2 - math.cos(x)

root = Bisection_method(f, 1, 5)
print("Approximate root:", root)

```
This code snippet returns: Approximate root: 3.155607581138611


here is plotting code:

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
slope is: 1.6459984522406022
intercept is: -0.5513844534356405
image:

<img width="574" height="455" alt="image" src="https://github.com/user-attachments/assets/e43e656a-42d9-4121-b6bf-89141724a95f" />

Bisection Method to approximate a root using the 'for loop'.

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

Fixed Point Method that approximates a root when you give it max iterations.

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

Fixed Point Method that approximates a root with a stopping condition since it is possible that the iteration does not converge. It is to avoid infinite loops.

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

Newton’s method for approximating a root

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

iteration has not converged
Approximate root: None

Newton’s method with error tracking that returns a list of absolute errors between the iterates and the exact solution with max 30 iterations.

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

Secant Method

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


