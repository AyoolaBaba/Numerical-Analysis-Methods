# Chapter 3  
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

,,,

it returns: Approximate root: 3.155607581138611


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

,,,

output: slope is: 1.6459984522406022
intercept is: -0.5513844534356405

