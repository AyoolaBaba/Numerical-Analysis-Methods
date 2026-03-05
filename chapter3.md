# Chapter 3

Forward differentiation approximation Method

```python

import numpy as np
import matplotlib.pyplot as plt

f = lambda x: np.sin(x) * (1-x) # Create a function (annonymus lambda function) that takes an input x that returns sinx*(1-x) and call it f
exact = -np.sin(1) # defining the exact value of f'(x) at x = 1

H = 2.0**(-np.arange(1,10)) #  H is the set of values for the size of interval that can hold any value from 2^(-1) - 2^(-10)
AbsError = [] # start off with a blank list of errors

# Create columns with column headers
print(f"{'h':<12} {'Absolute Error':<22} {'Reduction factor':<20}")
print("------------------------------------------------------")

# Fill the rows of the table
for h in H:

  approx = (f(1 + h) - f(1))/h # Approximation of the derivative of f(x) at x=1

  AbsError.append(abs((approx - exact)/exact)) # finding the absolute error

  if h==H[0]:
    reduction_factor = ''
  else:
    reduction_factor = AbsError[-2]/AbsError[-1]

  print(f"{h:<12} {AbsError[-1]:<22} {reduction_factor:<20}")

# Make a plot
plt.loglog(H, AbsError, 'b-*') # to see more clearly what the relation between the error and h is
plt.grid()
plt.show()

```

