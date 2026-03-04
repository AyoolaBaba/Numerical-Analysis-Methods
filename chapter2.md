# Chapter 3  
Bisection Method to approximate a root using the 'while loop'.

,,,

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
