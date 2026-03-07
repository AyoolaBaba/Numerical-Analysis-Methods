# Chapter 3

### Forward differentiation approximation Method

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
### output:

h            Absolute Error         Reduction factor    
------------------------------------------------------
0.5          0.1854181601184709                         
0.25         0.12776867710089196    1.4512020029138777  
0.125        0.07225039292956516    1.7684149790776915  
0.0625       0.03815217748298682    1.8937423155410655  
0.03125      0.019573887040617487   1.9491364900501262  
0.015625     0.009910221068472835   1.9751211305353678  
0.0078125    0.004985780110301419   1.9876971806271069  
0.00390625   0.00250053851755135    1.9938825478216344  
0.001953125  0.0012521789951432975  1.9969497390149018

 <img width="559" height="417" alt="image" src="https://github.com/user-attachments/assets/2f52f84c-22e3-4287-a648-598509611f76" />

### Forward-difference approximation 
for the first derivative at every point in the interval except at the right-hand side. that accepts:

a mathematical function,

the bounds of an interval,

the number of subintervals.

```python

import numpy as np
import matplotlib.pyplot as plt

def ForwardDiff(f, a, b, N):
    x = np.linspace(a, b, N+1) # creates an array of evenly spaced values over a specified range
    # The third parameter specifies how many points are returnse so for N intervals, you need N +1 points
    h = x[1] - x[0] #  H is the distance between the first interval and the second interval which is the same distance between all points
    df = [] # Start off with a blank list of differentials
    for j in np.arange(len(x)-1): # J is a way to step through the array x
        # What's up with the -1 in the definition of the loop?: The array starts at 0 whilst the length of x starts at 1 so

        approx = ( f(x[j + 1]) - f(x[j]) ) / h # Approximation, f ( x + h) = f (at value after j)

        df.append( approx ) # List the approximation for each j

    return df # Return the approximation for each j

f = lambda x: np.sin(x)
a = -0.01
b = 0.01
N = 20

approximations = ForwardDiff(f, a, b, N)
print(approximations)

```
output: [0.9999548336745894, 0.9999638335523439, 0.9999718334663424, 0.9999788334085924, 0.9999848333720924, 0.999989833350842, 0.9999938333398427, 0.9999968333350927, 0.9999988333335933, 0.9999998333333416, 0.9999998333333416, 0.9999988333335933, 0.9999968333350936, 0.9999938333398418, 0.9999898333508411, 0.9999848333720933, 0.9999788334085933, 0.9999718334663433, 0.9999638335523404, 0.999954833674591]

### First order approximation of the first derivative 
for f(x) = sin(x) on the interval [0 - 2π] with 100 sub intervals. plot f(x), f'(x) and the approximation of f'(x)

```python

import numpy as np
import matplotlib.pyplot as plt

def ForwardDiff(f,a,b,N):
    x = np.linspace(a,b,N+1) # creates an array of evenly spaced values over a specified range
    # The third parameter specifies how many points are returnse so for N intervals, you need N +1 points
    h = x[1] - x[0] #  H is the distance between the first interval and the second interval which is the same distance between all points
    df = [] # Start off with a blank list of differentials
    for j in np.arange(len(x)-1): # J is a way to step through the array x
        # What's up with the -1 in the definition of the loop?: The array starts at 0 whilst the length of x starts at 1 so

        approx = ( f(x[j + 1]) - f(x[j]) ) / h # Approximation, f ( x + h) = f (at value after j)

        df.append( approx ) # List the approximation for each j

    return df # Return the approximation for each j

f = lambda x: np.sin(x)
exact_df = lambda x: np.cos(x)
a = 0
b = 2*(np.pi)
N = 100 # Number of evenly spaced values over the specified range
x = np.linspace(a,b,N+1) # creates an array of evenly spaced values in range a- b
# The N+1 is there because the third input is the points between the spaces rather than the spaces themselves. So for 100 spaces, we need 101 points

df = ForwardDiff(f,a,b,N) # 'df' holds the values of the array that ForwardDiff produces

# In the next line we plot three curves:
# 1) the function f(x)
# 2) the function f'(x)
# 3) the approximation of f'(x)
# Why is x plotted as [0:-1] in approximation plot: this excludes the last point of x as an array as arrays start from 0 so there would be one more value for n in df that starts from 1
plt.plot(x,f(x),'b',x,exact_df(x),'r--',x[0:-1], df, 'k-.')
plt.grid()
plt.legend(['f(x) = sin(x)',
            'exact first deriv',
            'approx first deriv'])
plt.show()

```
<img width="568" height="413" alt="image" src="https://github.com/user-attachments/assets/e5698cff-100c-4d67-9ed2-15e971a54181" />

### Building the first derivative function in a much smarter way – using NumPy arrays in Python.

```python

import numpy as np
import matplotlib.pyplot as plt

def ForwardDiff(f,a,b,N):
    x = np.linspace(a,b,N+1)
    y = f(x)
    return ( y[1:] - y[:-1] ) / (x[1] - x[0])

f = lambda x: np.cos(x)
exact_df = lambda x: -np.sin(x)
a = 0
b = 4*(np.pi)
N = 100
x = np.linspace(a,b,N+1)

df = ForwardDiff(f,a,b,N)

plt.plot(x,f(x),'b',x,exact_df(x),'r--',x[0:-1], df, 'k-.')
plt.grid()
plt.legend(['f(x) = sin(x)',
            'exact first deriv',
            'approx first deriv'])
plt.show()

```
<img width="568" height="413" alt="image" src="https://github.com/user-attachments/assets/706df680-6937-4b66-a2ba-cffe9f8074d9" />


### Finds a first order approximation for the first derivative of on the interval.

The left-hand plot should show the function in blue and the approximate first derivative as a red dashed curve. Sample code for this exercise is given below.

```python

import matplotlib.pyplot as plt
import numpy as np

def ForwardDiff(f,a,b,N):
    x = np.linspace(a,b,N+1)
    y = f(x)
    return ( y[1:] - y[:-1] ) / (x[1] - x[0])

f = lambda x: np.sin(x) - x*np.sin(x)
a = 0
b = 15
N = 300
x = np.linspace(a,b,N+1) # Creates an array of evenly spaced values over a specified range
y = f(x) # Assign y to f(x)
df = ForwardDiff(f,a,b,N) # 'df' holds the values of the array that ForwardDiff produces

fig, ax = plt.subplots(1,2) # Creates 2 side-by-side subplots, .
ax[0].plot(x,y,'b',x[0:-1],df,'r--') # Plots two different datasets on the first subplot: a blue line for y vs x and a red dashed line for df vs x[0:-1].
ax[0].grid() # Adds grid lines to the first subplot

exact = lambda x: np.cos(x) - x*np.cos(x) - np.sin(x) # The exact derivative assigned to 'exact'

ax[1].semilogy(x[0:-1],abs(exact(x[0:-1]) - df)) # Creates a semilogarithmic plot with x[0:-1] on the x-axis and the absolute error between the exact values and df on the y-axis
ax[1].grid() # Adds grid lines to the plot.

```

<img width="554" height="413" alt="image" src="https://github.com/user-attachments/assets/da98492d-d675-4356-9d93-b9f2c79a5cf7" />

### Central Differentiation
A Python function CentralDiff(f, a, b, N) that takes a mathematical function f, the start and end values of an interval [a, b] and the number N of subintervals to use. It should return a second order numerical approximation to the first derivative on the interval.

```python

import numpy as np
import matplotlib.pyplot as plt

def CentralDiff(f,a,b,N):
    x = np.linspace(a,b,N+1)
    y = f(x)
    return ( y[2:] - y[:-2] ) / ( 2*(x[1] - x[0]) )

f = lambda x: np.sin(x)
exact_df = lambda x: np.cos(x)
a = 0
b = 10
N = 1000
x = np.linspace(a,b,N+1)

df = CentralDiff(f,a,b,N)

plt.plot(x,f(x),'b',x,exact_df(x),'r--',x[1:-1], df, 'k-.')
plt.grid()
plt.legend(['f(x) = sin(x)',
            'exact first deriv',
            'approx first deriv'])
plt.show()

```

<img width="568" height="413" alt="image" src="https://github.com/user-attachments/assets/4e828110-1c73-40d1-bda5-81c04e9215a8" />

### Second order numerical approximation to the second derivative on the interval

```python

import numpy as np
import matplotlib.pyplot as plt

def SecondDiff(f,a,b,N):
    x = np.linspace(a,b,N+1)
    y = f(x)

    return (-2*y[1:-1] + y[2:] + y[:-2] ) / ( (x[1] - x[0])**2 )

f = lambda x: np.sin(x) - x*np.sin(x)
exact_df = lambda x: x*np.sin(x) - 2*np.cos(x) - np.sin(x)
a = 0
b = 10
N = 1000
x = np.linspace(a,b,N+1)

df = SecondDiff(f,a,b,N)

plt.plot(x,f(x),'b',x,exact_df(x),'r--',x[1:-1], df, 'k-.')
plt.grid()
plt.legend(['f(x) = sin(x) - xsin(x)',
            'exact second deriv',
            'approx second deriv'])
plt.show()

```
<img width="546" height="414" alt="image" src="https://github.com/user-attachments/assets/fece9553-3d2e-432f-a336-b46f78146684" />

### Forward Mode AD

```python

import numpy as np
# Basic operations on dual numbers represented as tuples (value, derivative)
def dual_add(a, b):
    """Add two dual numbers:
    (a, a') + (b, b') = (a + b, a' + b')"""
    return (a[0] + b[0], a[1] + b[1])

def dual_multiply(a, b):
    """Multiply two dual numbers:
    (a, a') * (b, b') = (a*b, a'*b + a*b')"""
    return (a[0] * b[0], a[1] * b[0] + a[0] * b[1])

def dual_power(a, n):
    """Raise dual number to integer power n:
    (a, a')^n = (a^n, n*a^(n-1)*a')"""
    return (a[0]**n, n * a[0]**(n-1) * a[1])

def dual_sin(a):
    """Sine of dual number:
    sin(a, a') = (sin(a), cos(a)*a')"""
    return (np.sin(a[0]), np.cos(a[0]) * a[1])

x = (2, 1)
u = dual_power(x, 2)
v = dual_sin(x)
f = dual_multiply(u, v)
print(f)

```
output: (3.637189707302727, 1.9726023611141572)

