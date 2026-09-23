# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

> <span style="color:#7f8c8d;">Source: SEE复习课/review7_single.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

## <span style="color:#c0392b;">**EE364a Review Session 7（EE364a 复习课 7）**</span>

<!-- p.1 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**开场：复习课提纲**</span>

<span style="color:#2471a3;">**[section]**</span> 本讲为 EE364a 的第 7 次复习课（Review Session 7），session outline（内容提纲）如下：

- **derivatives and chain rule（导数与链式法则）**（Appendix A.4，附录 A.4）
- **numerical linear algebra（数值线性代数）**（Appendix C，附录 C）
  - factor and solve method（分解—求解方法）
  - exploiting structure and sparsity（利用结构与稀疏性）

<!-- p.2 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Derivative and gradient（导数与梯度）**</span>

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">当 $f$ 是实值函数（即 $f:\mathbb{R}^n \to \mathbb{R}$）时，其 derivative（导数）$Df(x)$ 是一个 $1\times n$ 矩阵，也就是一个行向量（row vector）。</span>

- 它的转置称为该函数的 **gradient（梯度）**：

  ```math
  \nabla f(x) = Df(x)^T,
  ```

  它是一个（列）向量，即属于 $\mathbb{R}^n$。

- 它的各分量是 $f$ 的 **partial derivatives（偏导数）**：$\nabla f(x)_i = \dfrac{\partial f(x)}{\partial x_i}$，其中 $i = 1, \dots, n$。

- $f$ 在点 $x$ 处的 **first-order approximation（一阶近似）** 可以表示为（关于 $z$ 的仿射函数）：

  $f(x) + \nabla f(x)^T(z - x)$。

<!-- p.3 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Example：log-sum-exp 函数的梯度**</span>

<span style="color:#2471a3;">**[example]**</span> **Example（例）:** 求函数 $g:\mathbb{R}^m \to \mathbb{R}$ 的梯度：

```math
g(y) = \log \sum_{i=1}^{m} \exp(y_i).
```

> <span style="color:#2471a3;">**[solution]**</span> **Solution（解答）:**
>
> ```math
> \nabla g(y) = \frac{1}{\sum_{i=1}^{m} \exp y_i}
> \begin{bmatrix}
> \exp y_1\\
> \vdots\\
> \exp y_m
> \end{bmatrix}
> ```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这个结果值得停下来想一想：$\log\sum_i \exp(y_i)$ 正是机器学习中 softmax 的对数，也就是所谓的 log-sum-exp 函数——它是 max 函数的光滑近似。它的梯度 $\exp(y_i)/\sum_j \exp(y_j)$ 恰好构成一个概率分布：各分量非负、总和为一。这与你正在学的 Bertsekas 概率论直接接轨——把 $p_i = \exp(y_i)/\sum_j \exp(y_j)$ 看作类别分布（categorical distribution）的概率，则 $\nabla g(y) = p$，梯度本身就是这个分布。在统计与概率图模型里，$\log\sum_i\exp(y_i)$ 被称为对数配分函数（log-partition function），它对参数求导得到各状态的期望占据率，这与对矩母函数（moment generating function）$M(t)=E[e^{tX}]$ 取对数后求导得到 $E[X]$ 是同一套机制。所以"对数—求和—指数"的组合反复出现并非巧合：它是把一组未归一化的非负权重变成概率分布的标准手法，下一页的例 A.2 会立刻用到。

<!-- p.4 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Chain rule（链式法则）**</span>

<span style="color:#2471a3;">**[theorem]**</span> 设 $f:\mathbb{R}^n \to \mathbb{R}^m$ 与 $g:\mathbb{R}^m \to \mathbb{R}^p$ 均可微（differentiable），定义 $h:\mathbb{R}^n \to \mathbb{R}^p$ 为 $h(x) = g(f(x))$，则

```math
Dh(x) = Dg(f(x))\,Df(x).
```

<span style="color:#2471a3;">**[theorem]**</span> **Composition with an affine function（与仿射函数复合）:** 设 $g:\mathbb{R}^m \to \mathbb{R}^p$ 可微，$A \in \mathbb{R}^{m\times n}$，$b \in \mathbb{R}^m$，定义 $h:\mathbb{R}^n \to \mathbb{R}^p$ 为 $h(x) = g(Ax + b)$，则 $h$ 的导数为 $Dh(x) = Dg(Ax + b)\,A$。

当 $g$ 是实值函数（即 $p = 1$）时，梯度形式为 $\nabla h(x) = A^T \nabla g(Ax + b)$。

<!-- p.5 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Example A.2：log-sum-exp 复合仿射映射的梯度**</span>

<span style="color:#2471a3;">**[example]**</span> **Example A.2（例 A.2）:** 求函数 $h:\mathbb{R}^n \to \mathbb{R}$ 的梯度：

```math
h(x) = \log \sum_{i=1}^{m} \exp(a_i^T x + b_i),
```

其中 $a_1, \dots, a_m \in \mathbb{R}^n$，$b_1, \dots, b_m \in \mathbb{R}$。

**Hint（提示）:** $h$ 是仿射函数 $Ax + b$（其中 $A \in \mathbb{R}^{m\times n}$ 的各行分别为 $a_1^T, \dots, a_m^T$）与函数 $g(y) = \log\left(\sum_{i=1}^m \exp y_i\right)$ 的复合。

> <span style="color:#2471a3;">**[solution]**</span> **Solution（解答）:**
>
> ```math
> \nabla g(y) = \frac{1}{\sum_{i=1}^{m} \exp y_i}
> \begin{bmatrix}
> \exp y_1\\
> \vdots\\
> \exp y_m
> \end{bmatrix}
> ```
>
> 再由复合公式（composition formula）可得
>
> ```math
> \nabla h(x) = \frac{1}{\mathbf{1}^T z}\, A^T z,
> \qquad z_i = \exp(a_i^T x + b_i),\quad i = 1, \dots, m
> ```

<!-- p.6 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Second derivative（二阶导数）**</span>

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">当 $f$ 是实值函数（即 $f:\mathbb{R}^n \to \mathbb{R}$）时，其二阶导数（second derivative）即 **Hessian matrix（Hessian 矩阵）** $\nabla^2 f(x)$，是一个 $n \times n$ 矩阵，其分量为 $\nabla^2 f(x)_{ij} = \dfrac{\partial^2 f(x)}{\partial x_i\, \partial x_j}$，其中 $i = 1, \dots, n$，$j = 1, \dots, n$。</span>

- $f$ 在 $x$ 处或附近的 **second-order approximation（二阶近似）** 是如下关于 $z$ 的二次函数（quadratic function）：

  ```math
  \hat{f}(z) = f(x) + \nabla f(x)^T(z - x) + (1/2)(z - x)^T \nabla^2 f(x)\,(z - x).
  ```

<!-- p.7 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Chain rule for second derivative（二阶导数的链式法则）**</span>

- <span style="color:#2471a3;">**[theorem]**</span> **Composition with scalar function（与标量函数复合）:** 设 $f:\mathbb{R}^n \to \mathbb{R}$，$g:\mathbb{R} \to \mathbb{R}$，且 $h(x) = g(f(x))$，则 $h$ 的二阶导数为

  $\nabla^2 h(x) = g'(f(x))\,\nabla^2 f(x) + g''(f(x))\,\nabla f(x)\nabla f(x)^T$。

- <span style="color:#2471a3;">**[theorem]**</span> **Composition with affine function（与仿射函数复合）:** 设 $g:\mathbb{R}^m \to \mathbb{R}$，$A \in \mathbb{R}^{m\times n}$，$b \in \mathbb{R}^m$，定义 $h:\mathbb{R}^n \to \mathbb{R}$ 为 $h(x) = g(Ax + b)$，则 $h$ 的二阶导数为

  $\nabla^2 h(x) = A^T \nabla^2 g(Ax + b)\,A$。

<!-- p.8 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Example A.4：log-sum-exp 复合仿射映射的 Hessian**</span>

<span style="color:#2471a3;">**[example]**</span> **Example A.4（例 A.4）:** 求函数 $h:\mathbb{R}^n \to \mathbb{R}$ 的 Hessian 矩阵：

```math
h(x) = \log \sum_{i=1}^{m} \exp(a_i^T x + b_i),
```

其中 $a_1, \dots, a_m \in \mathbb{R}^n$，$b_1, \dots, b_m \in \mathbb{R}$。

**Hint（提示）:** 对于 $g(y) = \log\left(\sum_{i=1}^m \exp y_i\right)$，我们有

```math
\nabla g(y) = \frac{1}{\sum_{i=1}^{m} \exp y_i}
\begin{bmatrix} \exp y_1\\ \vdots\\ \exp y_m \end{bmatrix},
\qquad
\nabla^2 g(y) = \mathrm{diag}(\nabla g(y)) - \nabla g(y)\nabla g(y)^T.
```

> <span style="color:#2471a3;">**[solution]**</span> **Solution（解答）:** 利用与仿射函数复合的链式法则（chain rule for composition with affine function），
>
> ```math
> \nabla^2 h(x) = A^T \left( \frac{1}{\mathbf{1}^T z}\,\mathrm{diag}(z) - \frac{1}{(\mathbf{1}^T z)^2}\,z z^T \right) A,
> ```
>
> 其中 $z_i = \exp(a_i^T x + b_i)$，$i = 1, \dots, m$。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 例 A.4 的结果背后藏着一个漂亮的概率解释：记 softmax 权重 $p_i = z_i/\sum_j z_j$，则可以验证 $\nabla^2 h(x)$ 恰好等于向量组 $\{a_i\}$ 在分布 $p$ 下的协方差矩阵（covariance matrix）$\sum_i p_i (a_i - \bar{a})(a_i - \bar{a})^T$，其中均值向量 $\bar{a} = \sum_i p_i a_i$ 正是上一页的梯度 $A^T p$。协方差矩阵天然半正定——这正接上 Strang 线代里"实对称矩阵半正定 ⟺ 特征值全非负 ⟺ 能写成 $B^T B$"这条等价链。而在凸优化中，Hessian 矩阵半正定正是二阶可微函数为凸的充要条件，于是 log-sum-exp 函数的凸性几乎"免费"得到：凸函数与仿射映射复合后仍是凸函数。这条链路把概率论的方差概念、线代的正定性判据与优化的二阶条件串成一线，是本课程最典型的论证模式之一。

<!-- p.9 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Numerical linear algebra（数值线性代数）**</span>

<span style="color:#2471a3;">**[section]**</span> **factor-solve method for $Ax = b$（求解 $Ax = b$ 的分解—求解方法）:**

- 考虑 $n$ 个变量、$n$ 个方程的线性方程组，即 $A$ 是方阵（square）。
- 计算成本为 $f + s$：
  - $f$ 是 factorization（分解）的 **flop count（浮点运算次数）**；
  - $s$ 是 solve step（求解步）的浮点运算次数。
- 若做一次分解、随后 $k$ 次求解，则计算成本为 $f + ks$。

<!-- p.10 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**LU factorization（LU 分解）**</span>

- 非奇异（nonsingular）矩阵 $A$ 可以分解为 $A = PLU$。
- $f = (2/3)n^3$（Gaussian elimination，高斯消元）。
- $s = 2n^2$（forward and back substitution，前代与回代求解）。
- 例如，可以用成本 $f + ns = (8/3)n^3$ 计算 $n \times n$ 矩阵的逆（为什么？）

> <span style="color:#2471a3;">**[solution]**</span> **Solution（解答）:**
>
> - 把 $AX = I$ 写成 $A[x_1\ \cdots\ x_n] = [e_1\ \cdots\ e_n]$；
> - 然后对 $i = 1, \dots, n$ 逐个求解 $Ax_i = e_i$。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 分解—求解的记账方式值得与 6.006 的复杂度分析对照：$f = (2/3)n^3$、$s = 2n^2$ 意味着分解是 $O(n^3)$ 而单次求解只有 $O(n^2)$，于是"摊销"（amortization）思想在这里体现得淋漓尽致——只要同一个 $A$ 要面对多个右端项，就应该分解一次、反复求解。求逆的成本 $(8/3)n^3$ 恰是 $n$ 次求解叠加的结果，这也解释了实践中"能不解逆就不解逆"的惯例：显式逆矩阵既费计算又损精度。另外注意 $A = PLU$ 中的置换矩阵 $P$ 并非可有可无：主元选取把大元素换到对角线位置，避免用小主元做除数而放大舍入误差，这与 CSAPP 中浮点数表示与舍入误差的内容直接相关——同样的消元步骤，选错主元可能让数值结果面目全非。渐近复杂度与数值稳定性，是衡量数值算法的两把不同的尺子。

<!-- p.11 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Cholesky factorization（Cholesky 分解）**</span>

- 对称正定（symmetric, positive definite）矩阵 $A$ 可以分解为 $A = LL^T$。
- $f = (1/3)n^3$。
- $s = 2n^2$。
- prob. 9.31a：每 $N$ 次迭代只做一次分解，但每次迭代都要求解：
  - 每 $N$ 步中的那一步，计算量为 $f + s = (1/3)n^3 + 2n^2$ flops；
  - 其余各步的计算量为 $s = 2n^2$ flops。

<!-- p.12 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Exploiting structure（利用结构）**</span>

<span style="color:#2471a3;">**[section]**</span> 求解 $Ax = b$ 的计算成本随 $A$ 的结构（structure of $A$）而变：

| $A$ 的结构 | $f$ | $s$ |
|---|---|---|
| 无特殊结构 none | $(2/3)n^3$ | $2n^2$ |
| 对称正定 symmetric, positive definite | $(1/3)n^3$ | $2n^2$ |
| 下三角 lower triangular | $0$ | $n^2$ |
| $k$-banded（$k$ 带状：当 $|i - j| > k$ 时 $a_{ij} = 0$） | $4nk^2$ | $6nk$ |
| 块对角 block diag（$m$ 个块） | $(2/3)n^3/m^2$ | $2n^2/m$ |
| DFT（用 FFT 求解） | $0$ | $5n\log n$ |

<!-- p.13 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Block elimination（块消元）**</span>

<span style="color:#2471a3;">**[algorithm]**</span> 求解

```math
\begin{bmatrix} A_{11} & A_{12} \\ A_{21} & A_{22} \end{bmatrix}
\begin{bmatrix} x_1 \\ x_2 \end{bmatrix}
=
\begin{bmatrix} b_1 \\ b_2 \end{bmatrix}
```

- 第一个方程 $A_{11}x_1 + A_{12}x_2 = b_1$ 给出

  $x_1 = A_{11}^{-1}(b_1 - A_{12}x_2)$；

- 于是第二个方程变为

  $(A_{22} - A_{21}A_{11}^{-1}A_{12})\,x_2 = b_2 - A_{21}A_{11}^{-1}b_1$；

- 若 $A_{11}$ 与 $S = A_{22} - A_{21}A_{11}^{-1}A_{12}$ 都容易求逆，则可以获得加速。

<!-- p.14 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Example：块对角系统的求解**</span>

<span style="color:#2471a3;">**[example]**</span> **Example（例）:** 求解方程组

```math
\begin{bmatrix} A & 0 \\ 0 & B \end{bmatrix} x = \begin{bmatrix} b \\ c \end{bmatrix},
```

其中 $A \in \mathbb{R}^{n\times n}$，$B \in \mathbb{R}^{n\times n}$，$b \in \mathbb{R}^n$，$c \in \mathbb{R}^n$，且矩阵 $A$、$B$ 均非奇异。

- <span style="color:#2471a3;">**[question]**</span> 暴力法（brute-force method）的浮点运算次数是多少？

> <span style="color:#2471a3;">**[solution]**</span> **Solution（解答）:** $(2/3)(2n)^3 = (16/3)n^3$。

- <span style="color:#2471a3;">**[question]**</span> 如何利用该结构？

> <span style="color:#2471a3;">**[solution]**</span> **Solution（解答）:**
>
> - 把 $x$ 划分为 $x = (x_1, x_2)$；
> - 则 $x_1 = A^{-1}b$，$x_2 = B^{-1}c$；
> - 浮点运算次数：$2\,(2/3)n^3 = (4/3)n^3$。

<!-- p.15 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Example：宽块系统（$m \gg n$ 的情形）**</span>

<span style="color:#2471a3;">**[example]**</span> **Example（例）:** 求解方程组

```math
\begin{bmatrix} I & B \\ C & I \end{bmatrix} x = \begin{bmatrix} b_1 \\ b_2 \end{bmatrix},
```

其中 $B \in \mathbb{R}^{m\times n}$，$C \in \mathbb{R}^{n\times m}$，且 $m \gg n$；同时假设整个矩阵非奇异。

- <span style="color:#2471a3;">**[question]**</span> 暴力法的浮点运算次数是多少？

> <span style="color:#2471a3;">**[solution]**</span> **Solution（解答）:** $(2/3)(m+n)^3$。

- <span style="color:#2471a3;">**[question]**</span> 如何利用该结构？

> <span style="color:#2471a3;">**[solution]**</span> **Solution（解答）:**
>
> - 用块消元得到方程 $(I - CB)x_2 = b_2 - Cb_1$ 以及 $x_1 = b_1 - Bx_2$；
> - 浮点运算次数：构造 $I - CB$ 花费 $2mn^2$，计算 $b_2 - Cb_1$ 需 $2mn$，解出 $x_2$ 需 $(2/3)n^3$，计算 $x_1$ 需 $2mn$；总复杂度为 $2mn^2$。

<!-- p.16 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Solving almost separable linear equations（求解几乎可分离的线性方程组）**</span>

<span style="color:#2471a3;">**[example]**</span> **Example（例）:** 考虑如下含 $2n + m$ 个方程的方程组：

```math
\begin{aligned}
Ax + By &= c\\
Dx + Ey + Fz &= g\\
Hy + Jz &= k
\end{aligned}
```

其中 $A, J \in \mathbb{R}^{n\times n}$，$B, H \in \mathbb{R}^{n\times m}$，$D, F \in \mathbb{R}^{m\times n}$，$E \in \mathbb{R}^{m\times m}$，$c, k \in \mathbb{R}^n$，$g \in \mathbb{R}^m$，且 $n > m$。

<!-- p.17 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Example（续）：重排为箭头系统**</span>

> <span style="color:#2471a3;">**[solution]**</span> **Solution（解答，续）:** 需要求解如下方程组：
>
> ```math
> \begin{bmatrix}
> A & B & 0\\
> D & E & F\\
> 0 & H & J
> \end{bmatrix}
> \begin{bmatrix} x\\ y\\ z \end{bmatrix}
> =
> \begin{bmatrix} c\\ g\\ k \end{bmatrix}
> ```
>
> - 朴素做法：把它当作稠密（dense）矩阵来处理。
> - 可以先对方程与变量重新排序，从而利用其结构：
>
> ```math
> \begin{bmatrix}
> A & 0 & B\\
> 0 & J & H\\
> D & F & E
> \end{bmatrix}
> \begin{bmatrix} x\\ z\\ y \end{bmatrix}
> =
> \begin{bmatrix} c\\ k\\ g \end{bmatrix}
> ```
>
> 此时方程组形如一个 "arrow（箭头）" 系统，我们可以用块消元（block elimination）高效求解。

<!-- p.18 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Example（续）：块消元推导**</span>

> <span style="color:#2471a3;">**[solution]**</span> **Solution（解答，续）:**
>
> - 由于
>
> ```math
> \begin{bmatrix} A & 0\\ 0 & J \end{bmatrix}
> \begin{bmatrix} x\\ z \end{bmatrix}
> +
> \begin{bmatrix} B\\ H \end{bmatrix} y
> =
> \begin{bmatrix} c\\ k \end{bmatrix}
> ```
>
> 于是
>
> ```math
> \begin{bmatrix} x\\ z \end{bmatrix}
> =
> \begin{bmatrix} A^{-1}c\\ J^{-1}k \end{bmatrix}
> -
> \begin{bmatrix} A^{-1}B\\ J^{-1}H \end{bmatrix} y
> ```
>
> - 我们已知 $\begin{bmatrix} D & F \end{bmatrix}\begin{bmatrix} x\\ z \end{bmatrix} + Ey = g$；
> - 把上面推导出的表达式代入，得
>
> ```math
> \begin{bmatrix} D & F \end{bmatrix}
> \left(
> \begin{bmatrix} A^{-1}c\\ J^{-1}k \end{bmatrix}
> -
> \begin{bmatrix} A^{-1}B\\ J^{-1}H \end{bmatrix} y
> \right)
> + Ey = g
> ```
>
> - 因此
>
> ```math
> (E - DA^{-1}B - FJ^{-1}H)\,y = g - DA^{-1}c - FJ^{-1}k
> ```

<!-- p.19 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Example（续）：高效求解算法**</span>

> <span style="color:#2471a3;">**[algorithm]**</span> **Solution（解答，续）:** 于是可以按如下方式利用结构、高效求解该方程组：
>
> - 构造 $M = A^{-1}B$，$n = A^{-1}c$，$P = J^{-1}H$，$q = J^{-1}k$；
> - 计算 $r = g - Dn - Fq$；
> - 计算 $S = E - DM - FP$；
> - 求出 $y = S^{-1}r$，$x = n - My$，$z = q - Py$。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这一整套操作的核心是 $S = E - DA^{-1}B - FJ^{-1}H$，即分块意义下高斯消元"剩下的那一块"——Schur complement（舒尔补）。回忆 Strang 线代里的消元过程：用主元行消去下方各行之后，矩阵右下角残留的子块，在分块视角下正是 $S$。这里的第一性原理是：计算顺序本身就是一种资源。把"箭头系统"重排后再消元，等于把一个 $2n + m$ 维的大问题缩成一个只有 $m$ 维（且 $n > m$）的小问题来求逆，其余步骤都只涉及已知矩阵的乘法。这与 6.006 中动态规划"先解子问题、再拼出原问题"的精神相通：先对容易求逆的 $A$、$J$ 下手，再用它们的答案表达剩余的自由度，而不是对整个大矩阵硬做 $O(n^3)$ 分解。

<!-- p.20 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Using sparsity in Matlab（在 Matlab 中利用稀疏性）**</span>

- 用 `sparse`、`spalloc`、`speye`、`spdiags`、`spones` 构造稀疏（sparse）矩阵；
- 用 `spy`、`nnz` 对其进行可视化与分析；
- 还有 `sprand`、`sprandn`、`eigs`、`svds` 可用；
- 注意不要不小心把稀疏矩阵弄成了稠密矩阵。

<!-- p.21 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**在附加题 2（additional problem 2）中利用稀疏性**</span>

- （稀疏的）三对角矩阵 $\Delta \in \mathbb{R}^{n\times n}$：

```math
\Delta =
\begin{bmatrix}
1 & -1 & 0 & \cdots & 0 & 0 & 0\\
-1 & 2 & -1 & \cdots & 0 & 0 & 0\\
0 & -1 & 2 & \cdots & 0 & 0 & 0\\
\vdots & \vdots & \vdots & & \vdots & \vdots & \vdots\\
0 & 0 & 0 & \cdots & 2 & -1 & 0\\
0 & 0 & 0 & \cdots & -1 & 2 & -1\\
0 & 0 & 0 & \cdots & 0 & -1 & 1
\end{bmatrix}.
```

- 在 Matlab 中可以按如下方式构造：

```
e = ones(n,1);
D = spdiags([-e 2*e -e],[-1 0 1], n,n);
D(1,1) = 1; D(n,n) = 1;
```

- 单位矩阵的稀疏版本可以用 `speye` 构造。

<!-- p.22 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Sparse Cholesky factorization with permutations（带置换的稀疏 Cholesky 分解·下箭头）**</span>

考虑对下箭头（downward arrow）矩阵 $A_d$ 做分解 $A_d = L_d L_d^T$，取 $n = 3000$：

- `nnz(A_d)=8998`；
- 调用 `L_d=chol(A_d,'lower')`（旧版本的 Matlab 中请用 `L_d=chol(A_d)'`）；
- 图中为 $A_d$ 与 $L_d$ 的稀疏模式（`spy` 图）可视化；
- `nnz(L_d)=5999`，分解耗时 `tf_d=0.0022` 秒；
- 求解 $A_d x = b$ 时调用 `x=L_d'\(L_d\b)`，运行耗时 `ts_d=0.0020` 秒。

<!-- p.23 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Sparse Cholesky factorization with permutations（带置换的稀疏 Cholesky 分解·上箭头）**</span>

现在看对上箭头（upward arrow）矩阵 $A_u$ 做分解 $A_u = L_u L_u^T$：

- 同样地，`nnz(A_u)=8998`；
- 调用 `L_u=chol(A_u,'lower')`；
- 图中为 $A_u$ 与 $L_u$ 的稀疏模式可视化；
- `nnz(L_u)=4501500`，计算耗时 `tf_u=3.7288` 秒；
- 调用 `x=L_u'\(L_u\b)` 已不再高效；运行耗时 `ts_u=0.5673` 秒。

<!-- p.24 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Sparse Cholesky factorization with permutations（带置换的稀疏 Cholesky 分解·置换方案）**</span>

改为带置换地对 $A_u$ 做分解：$A_u = P L_p L_p^T P^T$。

- 调用 `[L_p,pp,P]=chol(A_u,'lower')`；
- 图中为 $L_p$ 与 $P$ 的稀疏模式可视化；
- $P$ 是一个次对角线与右上角元素为 1 的 Toeplitz matrix（Toeplitz 矩阵），即 $P_{1,n} = 1$，$P_{k+1,k} = 1$（$k = 1, \dots, n$），其余元素全为 0；
- 分解只需 `tf_p=0.0028` 秒；
- 求解 $A_u x = b$ 用 `x=P'\(L_p'\(L_p\(P\b)))`；求解耗时 `ts_p=0.0042` 秒。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 为什么上下箭头矩阵的分解成本相差上千倍？答案是 fill-in（填充）：Cholesky 分解会往 $L$ 中填入原矩阵没有的非零元，而填充的模式由消元顺序决定。用 6.006 的图视角看：顶点是变量，非零元 $a_{ij} \neq 0$ 是边，消去变量 $k$ 相当于把 $k$ 的所有邻居两两连边。下箭头矩阵对应的图是一条链，消元只牵动相邻节点，几乎不产生填充；上箭头矩阵对应的图是"一点连全部"，第一个被消的点就会迫使所有邻居互连，非零元瞬间膨胀到 450 万个。这三页里的置换矩阵 $P$ 只是把消元顺序整个颠倒，让"中心点"最后登场，图又变回链，于是成本回到链的水平。最小度（minimum degree）等排序启发式在稀疏求解器中会被自动执行——稀疏矩阵计算的一半功夫，其实花在安排计算顺序上。
