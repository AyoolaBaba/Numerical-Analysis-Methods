# Numerical-Analysis-Methods
By **Ayoola Babalola**

## Project Structure

1. [Chapter 1 – Floating-Point Arithmetic](chapter1.md)
2. [Chapter 2 – Error and Stability](chapter2.md)
3. [Chapter 3 – Function Approximation](chapter3.md)
4. [Chapter 4 – Root Finding](chapter4.md)
5. [Chapter 5 – Numerical Integration](chapter5.md)
6. [Chapter 6 – Differential Equations](chapter6.md)
7. [Chapter 7 – Conclusion](chapter7.md)


## Overview
This project presents a structured computational study of core numerical
analysis methods, implemented and analysed in Python. The work is organised
into seven chapters, progressing from floating-point arithmetic and error
analysis to numerical approximation, integration, and differential equation
solvers.

Rather than focusing solely on implementation, the project emphasises *why*
numerical methods are required, *how* numerical errors arise, and *what*
trade-offs exist between accuracy and efficiency in practical computation.

All experiments were developed and executed in Jupyter notebooks, originally
using Google Colab and later curated into a reproducible GitHub project.

---

## Motivation
Computers operate using finite-precision binary arithmetic and therefore cannot
represent real numbers or exact mathematical operations perfectly. As a result,
numerical computations inevitably involve approximation and error.

Numerical analysis provides the theoretical and practical tools needed to
control these errors while maintaining efficient computation. This project
investigates these ideas through concrete examples, analytical reasoning, and
code-based experiments.

---

## Chapter Overview

### Chapter 1 — Floating-Point Arithmetic and Rounding Error
Investigates how real numbers are represented in computers and why rounding
errors are unavoidable. Includes analytical reasoning and computational
examples demonstrating error propagation in basic arithmetic.

### Chapter 2 — Error, Conditioning, and Stability
Examines how errors propagate through algorithms, distinguishing between
well-conditioned and ill-conditioned problems and analysing numerical
stability in practice.

### Chapter 3 — Function Approximation and Truncation Error
Explores polynomial and series approximations of functions, highlighting the
role of truncation error and the trade-off between accuracy and computational
cost.

### Chapter 4 — Root-Finding Methods
Implements and compares numerical root-finding algorithms, analysing their
convergence properties, robustness, and sensitivity to initial conditions.

### Chapter 5 — Numerical Integration
Studies numerical quadrature methods and their accuracy, with attention to
error behaviour and efficiency for different classes of functions.

### Chapter 6 — Numerical Solutions of Differential Equations
Investigates time-stepping methods for ordinary differential equations,
including stability and accuracy considerations.

### Chapter 7 — Summary and Reflections
Synthesises key insights across all chapters, reflecting on limitations,
patterns, and directions for further exploration.

---

## Key Takeaways
- Numerical errors are unavoidable but can be controlled through careful method
  selection.
- Floating-point representation fundamentally limits precision.
- Algorithmic stability is often more important than formal accuracy.
- There is a persistent trade-off between computational cost and numerical
  reliability.

---

## Technologies Used
- Python
- NumPy
- SciPy
- Matplotlib
- Jupyter Notebooks
- Google Colab

---

## How to Run
All notebooks can be viewed directly on GitHub or executed locally using
Jupyter. They can also be opened and run in Google Colab without modification.

---

## Author
Independent computational project by **Ayoola Babalola**.
