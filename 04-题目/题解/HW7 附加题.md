# EE364a Homework 7 additional problems（EE364a 作业 7 附加题）

> 原文：`HW7 附加题.pdf`（4 页），EE364a, Winter 2007–08, Prof. S. Boyd。含两段实现提示（针对习题 9.30 / 9.31）与两道附加题。解答见 `HW7 解答.pdf` 第 10 页起的 "Solutions to additional exercises"。

## Suggestions for exercise 9.30（对习题 9.30 的建议）

生成一个测试实例，我们推荐如下：

```matlab
n = 100;
m = 200;
randn('state',1);
A=randn(m,n);
```

当然，你也应当用不同的维度、不同的数据来测试你的代码。

在所有情形下，务必让你的 line search 首先找到一个使试探点落在 $\mathbf{dom}\,f$ 内的步长；如果你试图在定义域外计算 $f$，你会得到复数，而且永远回不来。

为求 $\nabla f(x)$ 与 $\nabla^2 f(x)$ 的表达式，请用 chain rule（见附录 A.4）；若你试图逐项计算 $\partial^2 f(x)/\partial x_i\partial x_j$，你会后悔的。

计算 Newton step 可以用 `vnt=-H\g`。

## Suggestions for exercise 9.31（对习题 9.31 的建议）

对 9.31a，你应当试 $N = 1$、$N = 15$ 与 $N = 30$。你不妨计算并存储 Hessian 的 Cholesky factorization，然后回代得到 search direction——尽管对这么小的问题在 Matlab 里你其实看不到什么加速。算出 Hessian 后，可以取 Cholesky 分解为 `L=chol(H,'lower')`。然后可以按 `-L'\(L\g)` 计算 search step，其中 `g` 是当前点的 gradient。Matlab 会做正确的事：先用前代解 `L\g`，再用回代解 `-L'\(L\g)`。每次替换是 $n^2$ 阶。

为公平比较三种方法（即 $N = 1$、$N = 15$、$N = 30$）的收敛性，横轴应当显示大约所需的总 flops 数，而不是迭代次数。你可以用每次分解 $n^3/3$、每次求解 $2n^2$ 来算大约的 flops 数（这里每次「求解」包含一次前代与一次回代）。

# Additional exercises（附加题）

## 1. Three-way linear classification（三类线性分类）

给定数据

```math
x^{(1)},\dots,x^{(N)}, \qquad y^{(1)},\dots,y^{(M)}, \qquad z^{(1)},\dots,z^{(P)},
```

它们是 $\mathbf{R}^n$ 中三个非空向量集合。我们希望找出 $\mathbf{R}^n$ 上的三个 affine function

```math
f_i(z) = a_i^T z - b_i, \qquad i = 1,2,3,
```

满足下列性质：

```math
\begin{aligned}
f_1(x^{(j)}) &> \max\{f_2(x^{(j)}), f_3(x^{(j)})\}, \quad j = 1,\dots,N, \\
f_2(y^{(j)}) &> \max\{f_1(y^{(j)}), f_3(y^{(j)})\}, \quad j = 1,\dots,M, \\
f_3(z^{(j)}) &> \max\{f_1(z^{(j)}), f_2(z^{(j)})\}, \quad j = 1,\dots,P .
\end{aligned}
```

用文字说：$f_1$ 在 $x$ 数据点上三者中最大，$f_2$ 在 $y$ 数据点上三者中最大，$f_3$ 在 $z$ 数据点上三者中最大。可以给出一个简单的几何解释：这三个函数把 $\mathbf{R}^n$ 分成三个区域

```math
\begin{aligned}
R_1 &= \{z \mid f_1(z) > \max\{f_2(z), f_3(z)\}\}, \\
R_2 &= \{z \mid f_2(z) > \max\{f_1(z), f_3(z)\}\}, \\
R_3 &= \{z \mid f_3(z) > \max\{f_1(z), f_2(z)\}\},
\end{aligned}
```

它们按「各函数是三者的最大值」来划分。我们的目标是找出满足 $x^{(j)} \in R_1$、$y^{(j)} \in R_2$、$z^{(j)} \in R_3$ 的函数。

把它表述为 convex optimization problem。你的表述中不得使用 strict inequality（严格不等式）。

求解 `sep3way_data.m` 中给出的三维分离问题的具体实例，其中矩阵 `X`、`Y`、`Z` 的各列分别给出 $x^{(j)}$（$j = 1,\dots,N$）、$y^{(j)}$（$j = 1,\dots,M$）与 $z^{(j)}$（$j = 1,\dots,P$）。为省去你画数据点与分离边界的麻烦，我们已在 `sep3way_data.m` 中包含了绘图代码。（注意 `a1`、`a2`、`a3`、`b1`、`b2` 含任意数值；你应当用 cvx 算出正确的值。）

## 2. Efficient numerical method for a regularized least-squares problem（正则化 least-squares 问题的高效数值解法）

考虑带 smoothing（平滑项）的 regularized least squares problem

```math
\text{minimize} \quad \sum_{i=1}^k (a_i^T x - b_i)^2 + \delta \sum_{i=1}^{n-1} (x_i - x_{i+1})^2 + \eta \sum_{i=1}^n x_i^2 ,
```

其中 $x \in \mathbf{R}^n$ 是变量，$\delta, \eta > 0$ 是参数。

**(a)** 把该问题的最优性条件表述为涉及 $x$ 的一组线性方程。（这称为 normal equations（正规方程）。）

**(b)** 现在假设 $k \ll n$。描述一种求解 (2a) 中 normal equations 的高效方法。给出不利用结构的通用方法的近似 flop 数，以及你这种高效方法的近似 flop 数。

**(c)** 数值实例。本小问你试用你的高效方法。取 $k = 100$、$n = 2000$、$\delta = \eta = 1$。首先按这些维度随机生成 $A$ 与 $b$。按 (2a) 构造 normal equations，用通用方法求解。接着写（简短的）代码实现你的高效方法，并在你的问题实例上运行。验证两种方法求得的解几乎相同，且你的高效方法比通用方法快得多。

注意：为确保从高效方法中得到加速，你需要了解一些 Matlab 的知识。你的方法应当涉及求解系数矩阵为 tridiagonal（三对角）的线性方程。在这种情况下，分解与回代都可以非常高效地进行。Matlab 文档说求解方程时会识别并利用 banded matrix（带状矩阵），但我们发现并非总是如此。为确保 Matlab 知道你的矩阵是三对角的，你可以用 `spdiags` 把矩阵声明为 sparse（稀疏），`spdiags` 可用来创建三对角矩阵。你也可以按常规方式创建三对角矩阵，然后用 `sparse` 把它转成稀疏矩阵。

还有一件你需要知道的事。假设你要解一组具有相同系数矩阵的线性方程，即你需要计算 $F^{-1}a_1,\dots,F^{-1}a_m$，其中 $F$ 可逆、$a_i$ 是列向量。把各列拼接起来，这可以表示成单个矩阵

```math
\big[ F^{-1}a_1\ \cdots\ F^{-1}a_m \big] = F^{-1}\big[ a_1\ \cdots\ a_m \big] .
```

要用 Matlab 计算这个矩阵，你应当把各右端项收集到一个矩阵中（如上），并用 Matlab 的反斜杠运算符：`F\A`。它会做正确的事：对矩阵 $F$ 只分解一次，并对各右端项做多次回代。
