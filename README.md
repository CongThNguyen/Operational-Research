# Operational-Research
Description of the term Operational Research

### Optimisation

When talking to people, many often ask me what is operational research ? Abstractly, operational research is a branch of Mathematics (Optimization Mathematics) and it provides optimal outcomes given any problems. Most of the time, I find myself giving people different kinds of answer because this topic is fairly tricky to explain without an example at hand so here I give you an example (coming from this [this page](http://people.brunel.ac.uk/~mastjjb/jeb/or/morelp.html):

> A carpenter makes tables and chairs. Each table can be sold for a profit of £30 and each chair for a profit of £10.
> 1. The carpenter can afford to spend up to 40 hours per week working and takes six hours to make a table and three hours to make a chair.
> 2. Customer demand requires that they make at least three times as many chairs as tables.
> 3. Tables take up four times as much storage space as chairs and there is room for at most four tables each week.

The goal is to **maximise** the profit that the carpenter can make in 1 week, by finding the optimal number of tables $x_T$ and number of chairs $x_C$ that they should make.

This is a *linear programming problem*: we need to maximise the weekly profit, given by the objective function $f(x_T, x_C) = 30x_T + 10x_C$, subject to 3 linear constraints:

$$
\begin{align}
0. & & x_T &\geq 0, x_C \geq 0 \\
1. & & 6x_T + 3x_C &\leq 40 \\
2. & & x_C &\geq 3x_T \\
3. & & x_C/4 + x_T &\leq 4
\end{align}
$$

We can use [`scipy.optimize.linprog()`](https://docs.scipy.org/doc/scipy/reference/tutorial/optimize.html#linear-programming-linprog) to solve this problem, but first we need to rearrange it a little bit, to fit the kind of input that this function takes.

$$
\begin{align}
&\min_{x_T, x_C} -30x_T - 10x_C, \\
&\text{such that} \quad
\begin{bmatrix}
-1 & 0 \\
0 & -1 \\
6 & 3 \\
3 & -1 \\
1 & \frac{1}{4}
\end{bmatrix}
\begin{bmatrix}
x_T \\ x_C
\end{bmatrix}
\leq
\begin{bmatrix}
0 \\ 0 \\ 40 \\ 0 \\ 4
\end{bmatrix}.
\end{align}
$$

Looking at the documentation, the default bounds on the variable values is that they are non-negative, which corresponds to our problem here -- so we don't need to specify them.
