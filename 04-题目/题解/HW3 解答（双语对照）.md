# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

> <span style="color:#7f8c8d;">Source: 作业解答/HW3 解答.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

## <span style="color:#2471a3;">**[lecture]**</span> <span style="color:#c0392b">EE364a Homework 3 Solutions（EE364a 作业 3 解答）</span>

> <span style="color:#7f8c8d;">EE364a, Winter 2007-08 · Prof. S. Boyd（斯坦福 EE364a 课程，2007–08 冬季学期 · S. Boyd 教授）</span>

---

<!-- p.1 -->

### <span style="color:#2471a3;">**[question]**</span> <span style="color:#c0392b">Exercise 3.42 Approximation width（习题 3.42：逼近宽度）</span>

**3.42** *Approximation width.* Let $f_0, \ldots, f_n : \mathbf{R} \to \mathbf{R}$ be given continuous functions. We consider the problem of approximating $f_0$ as a linear combination of $f_1, \ldots, f_n$. For $x \in \mathbf{R}^n$, we say that $f = x_1 f_1 + \cdots + x_n f_n$ approximates $f_0$ with tolerance $\epsilon > 0$ over the interval $[0, T]$ if $|f(t) - f_0(t)| \le \epsilon$ for $0 \le t \le T$. Now we choose a fixed tolerance $\epsilon > 0$ and define the *approximation width* as the largest $T$ such that $f$ approximates $f_0$ over the interval $[0, T]$:

```math
W(x) = \sup\{T \mid |x_1 f_1(t) + \cdots + x_n f_n(t) - f_0(t)| \le \epsilon \ \text{ for } \ 0 \le t \le T\}.
```

Show that $W$ is quasiconcave.

> <span style="color:#2471a3;">**[solution] Solution（解答）:**</span> 为证明 $W$ 是 quasiconcave（拟凹）的，我们证明集合 $\{x \mid W(x) \ge \alpha\}$ 对所有 $\alpha$ 都是 convex set（凸集）。我们有 $W(x) \ge \alpha$ 当且仅当
>
> ```math
> -\epsilon \le x_1 f_1(t) + \cdots + x_n f_n(t) - f_0(t) \le \epsilon
> ```
>
> 对所有 $t \in [0, \alpha)$ 成立。因此集合 $\{x \mid W(x) \ge \alpha\}$ 是无穷多个 halfspace（半空间）的 intersection（交集）（每个 $t$ 对应两个半空间），故为凸集。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这道题给出了处理 quasiconcave（拟凹）函数的标准套路：不硬啃定义，而是转而证明每个 superlevel set（超水平集）$\{x \mid W(x) \ge \alpha\}$ 都是 convex set（凸集）——这与 convex（凸）函数用 sublevel set（下水平集）刻画的方式恰成对偶。证明的引擎是把"优化式性质"翻译成"可行性约束"：$W(x) \ge \alpha$ 无非是说"逼近误差约束在 $[0, \alpha)$ 上处处成立"，于是超水平集成了无穷多个 halfspace（半空间）的交。你在 6.006 里见过这个思想的算法版本：binary search on the answer（在答案上二分），把"求最大宽度"化成一串"宽度至少为 $\alpha$ 是否可行"的判定问题，而判定版本通常远比优化版本容易。凸优化还额外送一份红利：此处的判定问题只含线性不等式，是 LP（linear program，线性规划）可行性问题，属于 polynomial time（多项式时间）可解类——这与最短路、网络流同属一个复杂度家族，正是"LP ↔ 多项式可解性"这条连接的具体体现。

### <span style="color:#2471a3;">**[question]**</span> <span style="color:#c0392b">Exercise 3.54 Log-concavity of Gaussian cumulative distribution function（习题 3.54：高斯累积分布函数的对数凹性）</span>

**3.54** *Log-concavity of Gaussian cumulative distribution function.* The cumulative distribution function of a Gaussian random variable,

```math
f(x) = \frac{1}{\sqrt{2\pi}} \int_{-\infty}^{x} e^{-t^2/2}\, dt,
```

is log-concave. This follows from the general result that the convolution of two log-concave functions is log-concave. In this problem we guide you through a simple self-contained proof that $f$ is log-concave. Recall that $f$ is log-concave if and only if $f''(x)f(x) \le f'(x)^2$ for all $x$.

(a) Verify that $f''(x)f(x) \le f'(x)^2$ for $x \ge 0$. That leaves us the hard part, which is to show the inequality for $x < 0$.

(b) Verify that for any $t$ and $x$ we have $t^2/2 \ge -x^2/2 + xt$.

(c) Using part (b) show that $e^{-t^2/2} \le e^{x^2/2 - xt}$. Conclude that

```math
\int_{-\infty}^{x} e^{-t^2/2}\, dt \le e^{x^2/2} \int_{-\infty}^{x} e^{-xt}\, dt.
```

(d) Use part (c) to verify that $f''(x)f(x) \le f'(x)^2$ for $x \le 0$.

> <span style="color:#2471a3;">**[solution] Solution（解答）:**</span> $f$ 的导数为
>
> ```math
> f'(x) = e^{-x^2/2}/\sqrt{2\pi}, \qquad f''(x) = -xe^{-x^2/2}/\sqrt{2\pi}.
> ```
>
> <!-- p.2 -->
>
> (a) 对 $x \ge 0$ 有 $f''(x) \le 0$。
>
> (b) 由于 $t^2/2$ 是 convex（凸）的，我们有
>
> ```math
> t^2/2 \ge x^2/2 + x(t - x) = xt - x^2/2.
> ```
>
> 这就是那条对任何 differentiable convex function（可微凸函数）都成立的 general inequality（一般不等式）
>
> ```math
> g(t) \ge g(x) + g'(x)(t - x),
> ```
>
> 应用于 $g(t) = t^2/2$。建立 $t^2/2 \ge -x^2/2 + xt$ 的另一种（更简单的？）办法是注意到
>
> ```math
> t^2/2 + x^2/2 - xt = (1/2)(x - t)^2 \ge 0.
> ```
>
> 然后只需把 $x^2/2 - xt$ 移到不等号另一边。
>
> (c) 两边取 exponential（指数）再 integral（积分）即可。
>
> (d) 这个基本不等式化为
>
> ```math
> -xe^{-x^2/2} \int_{-\infty}^{x} e^{-t^2/2}\, dt \le e^{-x^2},
> ```
>
> 即
>
> ```math
> \int_{-\infty}^{x} e^{-t^2/2}\, dt \le \frac{e^{-x^2/2}}{-x}.
> ```
>
> 这可以由 (c) 得到，因为
>
> ```math
> \int_{-\infty}^{x} e^{-xt}\, dt = \frac{e^{-x^2}}{-x}.
> ```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 高斯分布的 log-concavity（对数凹性）是概率论里许多定量结论的隐藏地基。先把条件看穿：$f''f \le f'^2$ 等价于 $(\log f)'' \le 0$，即 $\log f$ 是 concave（凹）函数；对高斯密度而言 $\log f = -t^2/2 + \text{常数}$，凹性一目了然，真正的难点是证明 integral（积分）——也就是累积分布函数本身——不破坏这一性质，本题用初等的逐点估计把这件事做成了自足的证明。更一般的结论是 Prékopa 定理（1973 年）：两个 log-concave（对数凹）函数的 convolution（卷积）仍是对数凹的，而 CDF 恰好是密度与区间示性函数的卷积，故本题结论是其特例。在 Bertsekas 的概率论框架里，对数凹性直接控制高斯 tail（尾部）的形状：它保证尾部衰减不慢于指数尺度，诸如 Mills 型比值 $1 - \Phi(x) \approx \varphi(x)/x$（$x$ 较大时）的估计都能由此展开。这也是后续 maximum likelihood（最大似然）估计唯一、posterior（后验）分布良定等结论常用的技术条件。

### <span style="color:#2471a3;">**[question]**</span> <span style="color:#c0392b">Exercise 3.57（习题 3.57：矩阵逆的矩阵凸性）</span>

**3.57** Show that the function $f(X) = X^{-1}$ is matrix convex on $\mathbf{S}^n_{++}$.

> <span style="color:#2471a3;">**[solution] Solution（解答）:**</span> 我们必须证明：对任意 $v \in \mathbf{R}^n$，函数
>
> ```math
> g(X) = v^T X^{-1} v
> ```
>
> 在 $\mathbf{S}^n_{++}$ 上关于 $X$ 是 convex（凸）的。这由教材 example 3.4（例 3.4）即可得出。

### <span style="color:#2471a3;">**[question]**</span> <span style="color:#c0392b">Exercise 4.1（习题 4.1）</span>

**4.1** Consider the optimization problem

```math
\begin{array}{ll}
\text{minimize} & f_0(x_1, x_2) \\
\text{subject to} & 2x_1 + x_2 \ge 1 \\
& x_1 + 3x_2 \ge 1 \\
& x_1 \ge 0, \quad x_2 \ge 0.
\end{array}
```

Make a sketch of the feasible set. For each of the following objective functions, give the optimal set and the optimal value.

<!-- p.3 -->

(a) $f_0(x_1, x_2) = x_1 + x_2$.

(b) $f_0(x_1, x_2) = -x_1 - x_2$.

(c) $f_0(x_1, x_2) = x_1$.

(d) $f_0(x_1, x_2) = \max\{x_1, x_2\}$.

(e) $f_0(x_1, x_2) = x_1^2 + 9x_2^2$.

> <span style="color:#2471a3;">**[solution] Solution（解答）:**</span> feasible set（可行集）如图所示（图示：可行集为第一象限内由直线 $2x_1 + x_2 = 1$ 与 $x_1 + 3x_2 = 1$ 围出的无界区域，图中标出三个顶点 $(1, 0)$、$(2/5, 1/5)$、$(0, 1)$）。
>
> (a) $x^\star = (2/5, 1/5)$。
>
> (b) 问题 unbounded below（无下界）。
>
> (c) $X_{\mathrm{opt}} = \{(0, x_2) \mid x_2 \ge 1\}$。
>
> (d) $x^\star = (1/3, 1/3)$。
>
> (e) $x^\star = (1/2, 1/6)$。它之所以最优，是因为它满足 $2x_1 + x_2 = 7/6 > 1$、$x_1 + 3x_2 = 1$，而且
>
> ```math
> \nabla f_0(x^\star) = (1, 3)
> ```
>
> 与直线 $x_1 + 3x_2 = 1$ perpendicular（垂直）。

### <span style="color:#2471a3;">**[question]**</span> <span style="color:#c0392b">Exercise 4.4 [P. Parrilo] Symmetries and convex optimization（习题 4.4：对称性与凸优化）</span>

**4.4** [P. Parrilo] *Symmetries and convex optimization.* Suppose $G = \{Q_1, \ldots, Q_k\} \subseteq \mathbf{R}^{n \times n}$ is a group, i.e., closed under products and inverse. We say that the function $f : \mathbf{R}^n \to \mathbf{R}$ is $G$-invariant, or symmetric with respect to $G$, if $f(Q_i x) = f(x)$ holds for all $x$ and $i = 1, \ldots, k$. We define $\bar{x} = (1/k)\sum_{i=1}^k Q_i x$, which is the average of $x$ over its $G$-orbit. We define the *fixed subspace* of $G$ as

```math
F = \{x \mid Q_i x = x, \ i = 1, \ldots, k\}.
```

(a) Show that for any $x \in \mathbf{R}^n$, we have $\bar{x} \in F$.

(b) Show that if $f : \mathbf{R}^n \to \mathbf{R}$ is convex and $G$-invariant, then $f(\bar{x}) \le f(x)$.

<!-- p.4 -->

(c) We say the optimization problem

```math
\begin{array}{ll}
\text{minimize} & f_0(x) \\
\text{subject to} & f_i(x) \le 0, \quad i = 1, \ldots, m
\end{array}
```

is $G$-invariant if the objective $f_0$ is $G$-invariant, and the feasible set is $G$-invariant, which means

```math
f_1(x) \le 0, \ldots, f_m(x) \le 0 \implies f_1(Q_i x) \le 0, \ldots, f_m(Q_i x) \le 0,
```

for $i = 1, \ldots, k$. Show that if the problem is convex and $G$-invariant, and there exists an optimal point, then there exists an optimal point in $F$. In other words, we can adjoin the equality constraints $x \in F$ to the problem, without loss of generality.

(d) As an example, suppose $f$ is convex and symmetric, i.e., $f(Px) = f(x)$ for every permutation $P$. Show that if $f$ has a minimizer, then it has a minimizer of the form $\alpha\mathbf{1}$. (This means to minimize $f$ over $x \in \mathbf{R}^n$, we can just as well minimize $f(t\mathbf{1})$ over $t \in \mathbf{R}$.)

> <span style="color:#2471a3;">**[solution] Solution（解答）:**</span>
>
> (a) 我们首先观察到：用某个固定的 $Q_j$ 去乘每个 $Q_i$，得到的仍是诸 $Q_i$ 的一个 permutation（置换）：
>
> ```math
> Q_j Q_i = Q_{\sigma(i)}, \qquad i = 1, \ldots, k,
> ```
>
> 其中 $\sigma$ 是某个置换。这是 group theory（群论）的一条基本结论，不过我们自己证明它也不难。首先，由 closedness（封闭性），每个 $Q_j Q_i$ 都等于某个 $Q_s$。现在假设 $Q_j Q_i = Q_k Q_i = Q_s$。两边右乘 $Q_i^{-1}$，便得 $Q_j = Q_k$。因此从下标 $i$ 到下标 $s$ 的映射是 one-to-one（一一）的，即一个置换。
>
> 于是我们有
>
> ```math
> Q_j \bar{x} = (1/k)\sum_{i=1}^k Q_j Q_i x = (1/k)\sum_{i=1}^k Q_{\sigma(i)} x = (1/k)\sum_{i=1}^k Q_i x = \bar{x}.
> ```
>
> 这对每个 $j$ 都成立，所以 $\bar{x} \in F$。
>
> (b) 利用 $f$ 的 convexity（凸性）与 invariance（不变性），
>
> ```math
> f(\bar{x}) \le (1/k)\sum_{i=1}^k f(Q_i x) = (1/k)\sum_{i=1}^k f(x) = f(x).
> ```

<!-- p.5 -->

> (c) 设 $x^\star$ 是某个 optimal solution（最优解）。那么 $\bar{x}^\star$ 是 feasible（可行）的，且
>
> ```math
> \begin{aligned}
> f_0(\bar{x}^\star)
> &= f_0\bigl((1/k)\textstyle\sum_{i=1}^k Q_i x^\star\bigr) \\
> &\le (1/k)\sum_{i=1}^k f_0(Q_i x^\star) \\
> &= f_0(x^\star).
> \end{aligned}
> ```
>
> 因此 $\bar{x}^\star$ 也是最优的。
>
> (d) 设 $x^\star$ 是 $f$ 的一个 minimizer（极小化点）。令 $\bar{x} = (1/n!)\sum_P P x^\star$，其中求和遍历所有 permutation（置换）。由于 $\bar{x}$ 在任何置换下都保持不变，我们得出 $\bar{x} = \alpha\mathbf{1}$（对某个 $\alpha \in \mathbf{R}$）。由 Jensen's inequality（Jensen 不等式），
>
> ```math
> f(\bar{x}) \le (1/n!)\sum_P f(P x^\star) = f(x^\star),
> ```
>
> 这说明 $\bar{x}$ 也是一个极小化点。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这道题展示的 symmetrization（对称化）技巧值得单独收进工具箱：把点沿 group（群）轨道求平均，就得到一个既保持结构、目标值又不会更差的"代表点"。用 Strang 线代的眼光看，平均映射 $x \mapsto \bar{x}$ 正是往 fixed subspace（不动子空间）$F$ 上的 projection（投影）：(a) 问证明任何点的像都落在 $F$ 中，而对 $F$ 中的点作平均又保持不动，这两条合起来恰好是投影算子的幂等性 $P^2 = P$，$F$ 则是这个投影的像子空间。(b) 问是 Jensen's inequality（Jensen 不等式）的一次干净利落的应用：convexity（凸性）给出"平均点处的函数值不超过函数值的平均"，$G$-invariance（群不变性）再把右端的平均折叠回 $f(x)$ 本身。这一思想影响深远：随机优化中的 Polyak–Ruppert averaging（1990 年代提出，用于随机逼近的收敛加速）本质上就是对迭代轨道做同样的平均；利用对称性约化半定规划的规模，则由 Gatermann 与 Parrilo 在 2002 年系统化。追根溯源，"用群作用切割解空间"的思路可追溯到 Galois 在 1830 年代开创的群论。

### <span style="color:#2471a3;">**[question]**</span> <span style="color:#c0392b">Exercise 4.8 Some simple LPs（习题 4.8：几个简单的 LP）</span>

**4.8** *Some simple LPs.* Give an explicit solution of each of the following LPs.

(a) *Minimizing a linear function over an affine set.*

```math
\begin{array}{ll}
\text{minimize} & c^T x \\
\text{subject to} & Ax = b.
\end{array}
```

> <span style="color:#2471a3;">**[solution] Solution（解答）:**</span> 我们区分三种 possible cases（情形）。
>
> - 问题 infeasible（不可行）（$b \notin \mathcal{R}(A)$）。此时 optimal value（最优值）为 $+\infty$。
> - 问题 feasible（可行），且 $c$ 与 $A$ 的 nullspace（零空间）orthogonal（正交）。我们可以把 $c$ 分解为
>
> ```math
> c = A^T \lambda + \hat{c}, \qquad A\hat{c} = 0.
> ```
>
> （$\hat{c}$ 是 $c$ 在 $A$ 的零空间中的分量；$A^T \lambda$ 与零空间正交。）若 $\hat{c} = 0$，则在可行集上 objective function（目标函数）化为一个常数：
>
> ```math
> c^T x = \lambda^T A x + \hat{c}^T x = \lambda^T b.
> ```
>
> 最优值为 $\lambda^T b$。所有 feasible solution（可行解）都是 optimal（最优）的。
> - 问题可行，且 $c$ 不在 $A^T$ 的 range（值域）中（$\hat{c} \ne 0$）。此时问题 unbounded（无界）（$p^\star = -\infty$）。为验证这一点，注意 $x = x_0 - t\hat{c}$ 对一切 $t$ 都可行；当 $t$ 趋于无穷时，目标值无界地下降。
>
> 综上，
>
> ```math
> p^\star =
> \begin{cases}
> +\infty & b \notin \mathcal{R}(A) \\
> \lambda^T b & c = A^T \lambda \ \text{ for some } \lambda \\
> -\infty & \text{otherwise.}
> \end{cases}
> ```

<!-- p.6 -->

(b) *Minimizing a linear function over a halfspace.*

```math
\begin{array}{ll}
\text{minimize} & c^T x \\
\text{subject to} & a^T x \le b,
\end{array}
```

where $a \ne 0$.

> <span style="color:#2471a3;">**[solution] Solution（解答）:**</span> 这个问题总是 feasible（可行）的。向量 $c$ 可以分解为平行于 $a$ 的分量与正交于 $a$ 的分量：
>
> ```math
> c = a\lambda + \hat{c}, \qquad \text{with } a^T \hat{c} = 0.
> ```
>
> - 若 $\lambda > 0$，问题 unbounded below（无下界）。取 $x = -ta$，并令 $t$ 趋于无穷：
>
> ```math
> c^T x = -tc^T a = -t\lambda a^T a \to -\infty
> ```
>
> 且
>
> ```math
> a^T x - b = -ta^T a - b \le 0
> ```
>
> 当 $t$ 充分大时成立，所以这样的 $x$ 对充分大的 $t$ 可行。直观地说，沿 $-a$ 方向走得足够远，就能找到目标值任意负的可行点。
> - 若 $\hat{c} \ne 0$，问题无下界。取 $x = ba - t\hat{c}$，并令 $t$ 趋于无穷。
> - 若 $c = a\lambda$（某个 $\lambda \le 0$），最优值为 $c^T ab = \lambda b$。
>
> 综上，最优值为
>
> ```math
> p^\star =
> \begin{cases}
> \lambda b & c = a\lambda \ \text{ for some } \lambda \le 0 \\
> -\infty & \text{otherwise.}
> \end{cases}
> ```

(c) *Minimizing a linear function over a rectangle.*

```math
\begin{array}{ll}
\text{minimize} & c^T x \\
\text{subject to} & l \preceq x \preceq u,
\end{array}
```

where $l$ and $u$ satisfy $l \preceq u$.

> <span style="color:#2471a3;">**[solution] Solution（解答）:**</span> objective（目标函数）与 constraint（约束）都是 separable（可分离）的：目标是诸项 $c_i x_i$ 之和，每一项只依赖单个 variable（变量）；每条约束也只涉及单个变量。因此我们可以对 $x$ 的每个 component（分量）独立地极小化来求解该问题。最优的 $x_i^\star$ 是 $c_i x_i$ 在约束 $l_i \le x_i \le u_i$ 下的极小点：若 $c_i > 0$，则 $x_i^\star = l_i$；若 $c_i < 0$，则 $x_i^\star = u_i$；若 $c_i = 0$，则区间 $[l_i, u_i]$ 内任何 $x_i$ 都是最优的。因此，问题的最优值为
>
> ```math
> p^\star = l^T c_+ + u^T c_-,
> ```
>
> 其中 $c_i^+ = \max\{c_i, 0\}$，$c_i^- = \max\{-c_i, 0\}$。

<!-- p.7 -->

(d) *Minimizing a linear function over the probability simplex.*

```math
\begin{array}{ll}
\text{minimize} & c^T x \\
\text{subject to} & \mathbf{1}^T x = 1, \quad x \succeq 0.
\end{array}
```

What happens if the equality constraint is replaced by an inequality $\mathbf{1}^T x \le 1$?

We can interpret this LP as a simple portfolio optimization problem. The vector $x$ represents the allocation of our total budget over different assets, with $x_i$ the fraction invested in asset $i$. The return of each investment is fixed and given by $-c_i$, so our total return (which we want to maximize) is $-c^T x$. If we replace the budget constraint $\mathbf{1}^T x = 1$ with an inequality $\mathbf{1}^T x \le 1$, we have the option of not investing a portion of the total budget.

> <span style="color:#2471a3;">**[solution] Solution（解答）:**</span> 设 $c$ 的分量已按递增次序排列，且
>
> ```math
> c_1 = c_2 = \cdots = c_k < c_{k+1} \le \cdots \le c_n.
> ```
>
> 对所有 feasible（可行的）$x$，我们有
>
> ```math
> c^T x \ge c_1(\mathbf{1}^T x) = c_{\min},
> ```
>
> 其中 equality（等号）成立当且仅当
>
> ```math
> x_1 + \cdots + x_k = 1, \qquad x_1 \ge 0, \ldots, x_k \ge 0, \qquad x_{k+1} = \cdots = x_n = 0.
> ```
>
> 我们得出 optimal value（最优值）为 $p^\star = c_1 = c_{\min}$。在投资解释下，这一选择相当显然：如果各项投资的 return（回报）固定且已知，就应把全部预算投入回报最高的那项投资。
>
> 若把等式约束换成不等式，则最优值为
>
> ```math
> p^\star = \min\{0, c_{\min}\}.
> ```
>
> （若 $c_{\min} \le 0$，$x$ 的取法与上面相同；否则取 $x = 0$。）

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 4.8(d) 值得驻足，因为 probability simplex（概率单纯形）$\{x \mid \mathbf{1}^T x = 1,\ x \succeq 0\}$ 正是离散 distribution（分布）的几何化身：分量 $x_i$ 就是落在第 $i$ 个结果上的概率质量。在这个视角下，$c^T x$ 恰是"取值为 $c_i$、分布为 $x$"的随机变量的 expectation（期望），于是"在单纯形上极小化一个线性函数"等价于问"什么样的分布让期望最小"，答案自然是退化成 one-hot（独热）分布——把全部质量压在最便宜的分量 $c_{\min}$ 上。题中的投资解释是这个数学事实的经济学重述：当回报固定已知时，最优组合毫不分散，全部资金押给回报最高的资产。把等式约束放松为 $\mathbf{1}^T x \le 1$ 后，最优值变成 $\min\{0, c_{\min}\}$，相当于引入"持有现金"这个收益恒为零的备选项——这正是期权价值的非负性。你在 Bertsekas 概率论中学过的期望线性性，与"线性函数在紧凸集上的极小必在 extreme point（极点）取得"这条凸分析事实，在这里严丝合缝地咬合在一起。

(e) *Minimizing a linear function over a unit box with a total budget constraint.*

```math
\begin{array}{ll}
\text{minimize} & c^T x \\
\text{subject to} & \mathbf{1}^T x = \alpha, \quad 0 \preceq x \preceq \mathbf{1},
\end{array}
```

where $\alpha$ is an integer between $0$ and $n$. What happens if $\alpha$ is not an integer (but satisfies $0 \le \alpha \le n$)? What if we change the equality to an inequality $\mathbf{1}^T x \le \alpha$?

> <span style="color:#2471a3;">**[solution] Solution（解答）:**</span> 我们先考虑 $\alpha$ 为 integer（整数）的情形。设
>
> ```math
> c_1 \le \cdots \le c_{i-1} < c_i = \cdots = c_\alpha = \cdots = c_k < c_{k+1} \le \cdots \le c_n.
> ```
>
> 最优值为
>
> ```math
> c_1 + c_2 + \cdots + c_\alpha,
> ```

<!-- p.8 -->

> 即 $c$ 中最小的 $\alpha$ 个元素之和。$x$ 是最优的当且仅当
>
> ```math
> x_1 = \cdots = x_{i-1} = 1, \qquad x_i + \cdots + x_k = \alpha - i + 1, \qquad x_{k+1} = \cdots = x_n = 0.
> ```
>
> 若 $\alpha$ 不是整数，则最优值为
>
> ```math
> p^\star = c_1 + c_2 + \cdots + c_{\lfloor\alpha\rfloor} + c_{1+\lfloor\alpha\rfloor}(\alpha - \lfloor\alpha\rfloor).
> ```
>
> 对于不等式约束 $\mathbf{1}^T x \le \alpha$（$\alpha$ 为 $0$ 到 $n$ 之间的整数）的情形，最优值是 $c$ 的 $\alpha$ 个最小的 nonpositive（非正）系数之和。

### <span style="color:#2471a3;">**[question]**</span> <span style="color:#c0392b">Exercise 4.17 Optimal activity levels（习题 4.17：最优活动水平）</span>

**4.17** *Optimal activity levels.* We consider the selection of $n$ nonnegative activity levels, denoted $x_1, \ldots, x_n$. These activities consume $m$ resources, which are limited. Activity $j$ consumes $A_{ij} x_j$ of resource $i$, where $A_{ij}$ are given. The total resource consumption is additive, so the total of resource $i$ consumed is $c_i = \sum_{j=1}^n A_{ij} x_j$. (Ordinarily we have $A_{ij} \ge 0$, i.e., activity $j$ consumes resource $i$. But we allow the possibility that $A_{ij} < 0$, which means that activity $j$ actually generates resource $i$ as a by-product.) Each resource consumption is limited: we must have $c_i \le c_i^{\max}$, where $c_i^{\max}$ are given. Each activity generates revenue, which is a piecewise-linear concave function of the activity level:

```math
r_j(x_j) =
\begin{cases}
p_j x_j & 0 \le x_j \le q_j \\
p_j q_j + p_j^{\mathrm{disc}}(x_j - q_j) & x_j \ge q_j.
\end{cases}
```

Here $p_j > 0$ is the basic price, $q_j > 0$ is the quantity discount level, and $p_j^{\mathrm{disc}}$ is the quantity discount price, for (the product of) activity $j$. (We have $0 < p_j^{\mathrm{disc}} < p_j$.) The total revenue is the sum of the revenues associated with each activity, i.e., $\sum_{j=1}^n r_j(x_j)$. The goal is to choose activity levels that maximize the total revenue while respecting the resource limits. Show how to formulate this problem as an LP.

> <span style="color:#2471a3;">**[solution] Solution（解答）:**</span> 基本问题可以表示为
>
> ```math
> \begin{array}{ll}
> \text{maximize} & \sum_{j=1}^n r_j(x_j) \\
> \text{subject to} & x \succeq 0 \\
> & Ax \preceq c^{\max}.
> \end{array}
> ```
>
> 这是一个 convex optimization problem（凸优化问题），因为 objective（目标）是 concave（凹）函数，而 constraints（约束）是一组 linear inequality（线性不等式）。为把它转化为一个等价的 LP（linear program，线性规划），我们先把收益函数表示为
>
> ```math
> r_j(x_j) = \min\{p_j x_j, \ p_j q_j + p_j^{\mathrm{disc}}(x_j - q_j)\},
> ```
>
> 该式成立正是因为 $r_j$ 是凹的。于是，$r_j(x_j) \ge u_j$ 当且仅当
>
> ```math
> p_j x_j \ge u_j, \qquad p_j q_j + p_j^{\mathrm{disc}}(x_j - q_j) \ge u_j.
> ```

<!-- p.9 -->

> 我们可以构造如下 LP：
>
> ```math
> \begin{array}{ll}
> \text{maximize} & \mathbf{1}^T u \\
> \text{subject to} & x \succeq 0 \\
> & Ax \preceq c^{\max} \\
> & p_j x_j \ge u_j, \quad p_j q_j + p_j^{\mathrm{disc}}(x_j - q_j) \ge u_j, \quad j = 1, \ldots, n,
> \end{array}
> ```
>
> 其中 variable（变量）为 $x$ 与 $u$。为证明这个 LP 与原问题等价，我们固定 $x$。LP 中最后一组约束保证 $u_i \le r_i(x)$，因此我们得出：对每一个可行的 $x$ 与 LP 中的 $u$，LP 的 objective（目标值）不超过 total revenue（总收益）。另一方面，我们总可以取 $u_i = r_i(x)$，此时两个目标值相等。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Solutions to additional exercises（补充习题解答）</span>

<!-- p.10 -->

### <span style="color:#2471a3;">**[exercise]**</span> <span style="color:#c0392b">Additional Exercise 1 Optimal activity levels（补充习题 1：最优活动水平）</span>

**1.** *Optimal activity levels.* Solve the optimal activity level problem described in exercise 4.17 in *Convex Optimization*, for the instance with problem data

```math
A =
\begin{bmatrix}
1 & 2 & 0 & 1 \\
0 & 0 & 3 & 1 \\
0 & 3 & 1 & 1 \\
2 & 1 & 2 & 5 \\
1 & 0 & 3 & 2
\end{bmatrix},
\qquad
c^{\max} =
\begin{bmatrix}
100 \\ 100 \\ 100 \\ 100 \\ 100
\end{bmatrix},
\qquad
p =
\begin{bmatrix}
3 \\ 2 \\ 7 \\ 6
\end{bmatrix},
\qquad
p^{\mathrm{disc}} =
\begin{bmatrix}
2 \\ 1 \\ 4 \\ 2
\end{bmatrix},
\qquad
q =
\begin{bmatrix}
4 \\ 10 \\ 5 \\ 10
\end{bmatrix}.
```

You can do this by forming the LP you found in your solution of exercise 4.17, or more directly, using cvx. Give the optimal activity levels, the revenue generated by each one, and the total revenue generated by the optimal solution. Also, give the average price per unit for each activity level, i.e., the ratio of the revenue associated with an activity, to the activity level. (These numbers should be between the basic and discounted prices for each activity.) Give a very brief story explaining, or at least commenting on, the solution you find.

> <span style="color:#2471a3;">**[solution] Solution（解答）:**</span> 下面的 Matlab/CVX 代码求解该问题。（这里我们把问题写成接近其原始陈述的形式，把改写成 LP 的工作交给 CVX 完成！）
>
> ```matlab
> A=[ 1 2 0 1;
> 0 0 3 1;
> 0 3 1 1;
> 2 1 2 5;
> 1 0 3 2];
> cmax=[100;100;100;100;100];
> p=[3;2;7;6];
> pdisc=[2;1;4;2];
> q=[4; 10 ;5; 10];
> cvx_begin
> variable x(4)
> maximize( sum(min(p.*x,p.*q+pdisc.*(x-q))) )
> subject to
> x >= 0;
> A*x <= cmax
> cvx_end
> x
> r=min(p.*x,p.*q+pdisc.*(x-q))
> ```
>
> <!-- p.11 -->
>
> ```matlab
> totr=sum(r)
> avgPrice=r./x
> ```
>
> 代码的运行结果为
>
> ```text
> x =
>     4.0000
>    22.5000
>    31.0000
>     1.5000
>
> r =
>    12.0000
>    32.5000
>   139.0000
>     9.0000
>
> totr =
>   192.5000
>
> avgPrice =
>     3.0000
>     1.4444
>     4.4839
>     6.0000
> ```
>
> 我们注意到第 3 个 activity level（活动水平）最高，它同时也是 basic price（基本价格）最高的活动。由于它的 quantity discount price（数量折扣价格）也很高，其活动水平高过了 discount quantity level（折扣数量门槛），因而它对 total revenue（总收益）的贡献最大。第 4 个活动的折扣价格比其基本价格低得多，因此其活动水平低于折扣数量门槛；此外，它还需要消耗大量 resource（资源），所以其活动水平较低。

### <span style="color:#2471a3;">**[exercise]**</span> <span style="color:#c0392b">Additional Exercise 2 Reformulating constraints in cvx（补充习题 2：在 cvx 中改写约束）</span>

**2.** *Reformulating constraints in cvx.* Each of the following cvx code fragments describes a convex constraint on the scalar variables x, y, and z, but violates the cvx rule set,

<!-- p.12 -->

and so is invalid. Briefly explain why each fragment is invalid. Then, rewrite each one in an equivalent form that conforms to the cvx rule set. In your reformulations, you can use linear equality and inequality constraints, and inequalities constructed using cvx functions. You can also introduce additional variables, or use LMIs. Be sure to explain (briefly) why your reformulation is equivalent to the original constraint, if it is not obvious.

Check your reformulations by creating a small problem that includes these constraints, and solving it using cvx. Your test problem doesn't have to be feasible; it's enough to verify that cvx processes your constraints without error.

*Remark.* This looks like a problem about 'how to use cvx software', or 'tricks for using cvx'. But it really checks whether you understand the various composition rules, convex analysis, and constraint reformulation rules.

```text
(a) norm( [ x + 2*y , x - y ] ) == 0
(b) square( square( x + y ) ) <= x - y
(c) 1/x + 1/y <= 1;  x >= 0; y >= 0
(d) norm([ max( x , 1 ) , max( y , 2 ) ]) <= 3*x + y
(e) x*y >= 1;  x >= 0; y >= 0
(f) ( x + y )^2 / sqrt( y ) <= x - y + 5
(g) x^3 + y^3 <= 1;  x>=0; y>=0
(h) x+z <= 1+sqrt(x*y-z^2);  x>=0; y>=0
```

> <span style="color:#2471a3;">**[solution] Solution（解答）:**</span>
>
> (a) 左端被正确识别为 convex（凸）的，但 equality constraint（等式约束）只允许两端都是 affine（仿射）的。由于向量的 norm（范数）为零当且仅当该向量为零向量，我们可以把该约束写成 `x+2*y==0; x-y==0`，或干脆写成 `x==0; y==0`。
>
> (b) 问题在于 `square()` 只接受 affine（仿射）参数，因为它是 convex（凸）的但不是 increasing（增）的。修正方法是改用 `square_pos()`：
>
> ```text
> square_pos( square( x + y ) ) <= x - y
> ```
>
> 我们也可以通过引入一个 additional variable（附加变量）来改写该约束：
>
> ```text
> variable t
> square( x+y ) <= t
> square( t ) <= x - y
> ```
>
> 注意，一般而言，通过引入新 variable（变量）来分解表达式并不保证可行。这里之所以可行，是因为外层的 square 函数在 $\mathbf{R}_+$ 上既是 convex（凸）的又是 monotonic（单调）的。
>
> 另一种做法是把约束改写为
>
> <!-- p.13 -->
>
> ```text
> ( x + y )^4 <= x - y
> ```
>
> (c) `1/x` 并不是 convex（凸）的，除非把 domain（定义域）限制在 $\mathbf{R}_{++}$ 上。我们可以把它写成 `inv_pos(x)+inv_pos(y)<=1`。`inv_pos` 函数的定义域是 $\mathbf{R}_{++}$，因此约束 `x > 0`、`y > 0` 已（隐式地）包含在内。
>
> (d) 问题在于 `norm()` 只接受仿射参数，因为它是凸的但不是增的。一种修正方法是引入新变量 $u$ 与 $v$：
>
> ```text
> norm( [ u , v ] ) <= 3*x + y
> max( x , 1 ) <= u
> max( y , 2 ) <= v
> ```
>
> 通过引入新变量来分解表达式在这里可行，因为 norm 在 $\mathbf{R}^2_+$ 上是 convex（凸）且 monotonic（单调）的，特别是在 $(1, \infty) \times (2, \infty)$ 上。
>
> (e) $xy$ 不是 concave（凹）的，所以按原样写行不通。但我们可以把该约束写成 `x>=inv_pos(y)`。（这里 $x$ 与 $y$ 可以互换。）另一种解法是把约束写成 `geomean([x,y])>=1`。我们还可以给出一个 LMI（linear matrix inequality，线性矩阵不等式）表示：
>
> ```text
> [ x 1; 1 y ] == semidefinite(2)
> ```
>
> (f) 这种写法在"用 concave（凹）函数去除 convex（凸）函数"处失败。我们可以写成
>
> ```text
> quad_over_lin(x+y,sqrt(y)) <= x-y+5
> ```
>
> 这可行，因为 `quad_over_lin` 对第二个 argument（自变量）是 monotone decreasing（单调递减）的，因此第二个位置可以接受凹函数，而 `sqrt` 是凹的。
>
> (g) 函数 $x^3 + y^3$ 在 $x \ge 0$、$y \ge 0$ 上是 convex（凸）的。但 $x^3$ 在 $x < 0$ 时不是凸的，所以 cvx 会拒绝这条语句。一种改写方式是
>
> ```text
> quad_pos_over_lin(square(x),x) + quad_pos_over_lin(square(y),y) <= 1
> ```
>
> 这可行，因为 `quad_pos_over_lin` 在其第一个 argument（自变量）上是 convex（凸）且 increasing（增）的，因此第一个位置可以接受凸函数。（而 `quad_over_lin` 在第一个自变量上不是增的，所以行不通。）
>
> 另一种更简单的做法是把约束改写为
>
> ```text
> pow_pos(x,3) + pow_pos(y,3) <= 1
> ```
>
> (h) 这里的问题是 $xy$ 不是 concave（凹）的，这会导致 cvx 拒绝该语句。为修正它，注意到
>
> ```math
> \sqrt{xy - z^2} = \sqrt{y\,(x - z^2/y)},
> ```
>
> 所以我们可以把约束改写为
>
> <!-- p.14 -->
>
> ```text
> x+z <= 1+geomean([x-quad_over_lin(z,y),y])
> ```
>
> 这可行，因为 geomean 是 concave（凹）的，且对每个 argument（自变量）都 nondecreasing（非降），因此第一个位置可以接受 concave（凹）函数。
>
> 我们可以通过在 cvx 中写出如下 feasibility problem（可行性问题）来检验上述改写（该问题显然 infeasible（不可行））：
>
> ```text
> cvx_begin
> variables x y u v z
> x == 0;
> y == 0;
> ( x + y )^4 <= x - y;
> inv_pos(x) + inv_pos(y) <= 1;
> norm( [ u ; v ] ) <= 3*x + y;
> max( x , 1 ) <= u;
> max( y , 2 ) <= v;
> x >= inv_pos(y);
> x >= 0;
> y >= 0;
> quad_over_lin(x + y , sqrt(y)) <= x - y + 5;
> pow_pos(x,3) + pow_pos(y,3) <= 1;
> x+z <= 1+geomean([x-quad_over_lin(z,y),y])
> cvx_end
> ```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这道题表面考 cvx 用法，实际考的是 disciplined convex programming（DCP，规则化凸编程，Grant、Boyd 与 Ye 于 2006 年提出）的 composition rule（复合规则）体系：每个 atom（原子函数）登记两枚标签——curvature（凸性：凸/凹/仿射）与 monotonicity（单调性：增/减/无）——表达式合法当且仅当标签按规则传递。其第一性原理只有一条：凸性沿复合传递，但前提是单调性方向"咬合"——convex 且 nondecreasing（非降）的函数可吃凸参数（如 exp 作用于凸表达式）；convex 且 nonincreasing（非增）的函数可吃凹参数（(f) 中 `quad_over_lin` 之于 `sqrt(y)`）；convex 但无单调性的函数只肯吃仿射参数（`square` 的限制即源于此）。(b)、(d)、(g)、(h) 的修复共用同一招：引入 intermediate variable（中间变量）把非仿射参数"拉平"，再靠单调性缝合，等价性由"最优点处中间变量总能顶到边界"保证。这与你写程序时维护函数签名、类型约束是同构的思路：用语法层的静态检查换取求解器的零意外失败，这是 CVX 工具链最重要的设计决策之一。

### <span style="color:#2471a3;">**[exercise]**</span> <span style="color:#c0392b">Additional Exercise 3 The illumination problem（补充习题 3：照明问题）</span>

**3.** *The illumination problem.* This exercise concerns the illumination problem described in lecture 1 (pages 9–11). We'll take $I^{\mathrm{des}} = 1$ and $p^{\max} = 1$, so the problem is

```math
\begin{array}{ll}
\text{minimize} & f_0(p) = \max_{k=1,\ldots,n} |\log(a_k^T p)| \\
\text{subject to} & 0 \le p_j \le 1, \quad j = 1, \ldots, m,
\end{array}
\tag{1}
```

with variable $p \in \mathbf{R}^n$. You will compute several approximate solutions, and compare the results to the exact solution, for a specific problem instance.

As mentioned in the lecture, the problem is equivalent to

```math
\begin{array}{ll}
\text{minimize} & \max_{k=1,\ldots,n} h(a_k^T p) \\
\text{subject to} & 0 \le p_j \le 1, \quad j = 1, \ldots, m,
\end{array}
\tag{2}
```

where $h(u) = \max\{u, 1/u\}$ for $u > 0$. The function $h$, shown in the figure below, is nonlinear, nondifferentiable, and convex. To see the equivalence between (1) and (2), we note that

```math
f_0(p)
= \max_{k=1,\ldots,n} |\log(a_k^T p)|
= \max_{k=1,\ldots,n} \max\{\log(a_k^T p), \log(1/a_k^T p)\}
```

<!-- p.15 -->

```math
= \log \max_{k=1,\ldots,n} \max\{a_k^T p, \ 1/a_k^T p\}
= \log \max_{k=1,\ldots,n} h(a_k^T p),
```

and since the logarithm is a monotonically increasing function, minimizing $f_0$ is equivalent to minimizing $\max_{k=1,\ldots,n} h(a_k^T p)$.

> <span style="color:#7f8c8d;">（图示：函数 $h(u) = \max\{u, 1/u\}$ 在 $u > 0$ 上的图像，横轴 $u$ 标至 $4$，纵轴 $h(u)$ 标至 $5$。）</span>

*The problem instance.* The specific problem data are for the geometry shown below, using the formula

```math
a_{kj} = r_{kj}^{-2} \max\{\cos\theta_{kj}, 0\}
```

from the lecture. There are 10 lamps ($m = 10$) and 20 patches ($n = 20$). We take $I^{\mathrm{des}} = 1$ and $p^{\max} = 1$. The problem data are given in the file `illum_data.m` on the course website. Running this script will construct the matrix $A$ (which has rows $a_k^T$), and plot the lamp/patch geometry as shown below.

> <span style="color:#7f8c8d;">（图示：灯（lamps）与区域块（patches）的二维几何布局图，横轴标至 $1$，纵轴标至 $2$。）</span>

<!-- p.16 -->

**(a) Equal lamp powers.** Take $p_j = \gamma$ for $j = 1, \ldots, m$. Plot $f_0(p)$ versus $\gamma$ over the interval $[0, 1]$. Graphically determine the optimal value of $\gamma$, and the associated objective value.

You can evaluate the objective function $f_0(p)$ in Matlab as `max(abs(log(A*p)))`.

**(b) Least-squares with saturation.** Solve the least-squares problem

```math
\text{minimize} \quad \sum_{k=1}^n (a_k^T p - 1)^2 = \|Ap - \mathbf{1}\|_2^2.
```

If the solution has negative values for some $p_i$, set them to zero; if some values are greater than 1, set them to 1. Give the resulting value of $f_0(p)$.

Least-squares solutions can be computed using the Matlab backslash operator: `A\b` returns the solution of the least-squares problem

```math
\text{minimize} \quad \|Ax - b\|_2^2.
```

**(c) Regularized least-squares.** Solve the regularized least-squares problem

```math
\text{minimize} \quad \sum_{k=1}^n (a_k^T p - 1)^2 + \rho \sum_{j=1}^m (p_j - 0.5)^2 = \|Ap - \mathbf{1}\|_2^2 + \rho\|p - (1/2)\mathbf{1}\|_2^2,
```

where $\rho > 0$ is a parameter. Increase $\rho$ until all coefficients of $p$ are in the interval $[0, 1]$. Give the resulting value of $f_0(p)$.

You can use the backslash operator in Matlab to solve the regularized least-squares problem.

**(d) Chebyshev approximation.** Solve the problem

```math
\begin{array}{ll}
\text{minimize} & \max_{k=1,\ldots,n} |a_k^T p - 1| = \|Ap - \mathbf{1}\|_\infty \\
\text{subject to} & 0 \le p_j \le 1, \quad j = 1, \ldots, m.
\end{array}
```

We can think of this problem as obtained by approximating the nonlinear function $h(u)$ by a piecewise-linear function $|u - 1| + 1$. As shown in the figure below, this is a good approximation around $u = 1$.

<!-- p.17 -->

> <span style="color:#7f8c8d;">（图示：$h(u)$ 与分段线性函数 $|u - 1| + 1$ 的对比图，横轴 $u$ 标至 $2$，纵轴标至 $4$，两条曲线在 $u = 1$ 附近几乎重合。）</span>

You can solve the Chebyshev approximation problem using cvx. The (convex) function $\|Ap - \mathbf{1}\|_\infty$ can be expressed in cvx as `norm(A*p-ones(n,1),inf)`. Give the resulting value of $f_0(p)$.

**(e) Exact solution.** Finally, use cvx to solve

```math
\begin{array}{ll}
\text{minimize} & \max_{k=1,\ldots,n} \max(a_k^T p, \ 1/a_k^T p) \\
\text{subject to} & 0 \le p_j \le 1, \quad j = 1, \ldots, m
\end{array}
```

exactly. You may find the `inv_pos()` function useful. Give the resulting (optimal) value of $f_0(p)$.

> <span style="color:#2471a3;">**[solution] Solution（解答）:**</span> 下面的 Matlab 脚本用题目提出的各 heuristic method（启发式方法）求得近似解，并给出 exact solution（精确解）。
>
> ```matlab
> % illum_sol: finds approximate and exact solutions of
> %            the illumination problem
> clear all;
> % load input data
> illum_data;
> % heuristic method 1: equal lamp powers
> % --------------------------------------
> nopts=1000;
> p = logspace(-3,0,nopts);
> f = zeros(size(p));
> for k=1:nopts
> f(k) = max(abs(log(A*p(k)*ones(m,1))));
> ```
>
> <!-- p.18 -->
>
> ```matlab
> end;
> [val_equal,imin] = min(f);
> p_equal = p(imin)*ones(m,1);
> % heuristic method 2: least-squares with saturation
> % -------------------------------------------------
> p_ls_sat = A\ones(n,1);
> p_ls_sat = max(p_ls_sat,0);
> % rounding negative p_i to 0
> p_ls_sat = min(p_ls_sat,1);
> % rounding p_i > 1 to 1
> val_ls_sat = max(abs(log(A*p_ls_sat)));
> % heuristic method 3: regularized least-squares
> % ---------------------------------------------
> rhos = linspace(1e-3,1,nopts);
> crit = [];
> for j=1:nopts
> p = [A; sqrt(rhos(j))*eye(m)]\[ones(n,1); sqrt(rhos(j))*0.5*ones(m,1)];
> crit = [ crit norm(p-0.5,inf) ];
> end
> idx = find(crit <= 0.5);
> rho = rhos(idx(1));
> % smallest rho s.t. p is in [0,1]
> p_ls_reg = [A; sqrt(rho)*eye(m)]\[ones(n,1); sqrt(rho)*0.5*ones(m,1)];
> val_ls_reg = max(abs(log(A*p_ls_reg)));
> % heuristic method 4: chebyshev approximation
> % -------------------------------------------
> cvx_begin
> variable p_cheb(m)
> minimize(norm(A*p_cheb-1, inf))
> subject to
> p_cheb >= 0
> p_cheb <= 1
> cvx_end
> val_cheb = max(abs(log(A*p_cheb)));
> % exact solution:
> % ---------------
> cvx_begin
> variable p_exact(m)
> minimize(max([A*p_exact; inv_pos(A*p_exact)]))
> subject to
> p_exact >= 0
> ```
>
> <!-- p.19 -->
>
> ```matlab
> p_exact <= 1
> cvx_end
> val_exact = max(abs(log(A*p_exact)));
> % Results
> % -------
> [p_equal p_ls_sat p_ls_reg p_cheb p_exact]
> [val_equal val_ls_sat val_ls_reg val_cheb val_exact]
> ```
>
> 结果汇总于下表。
>
> | | method 1（方法 1） | method 2（方法 2） | method 3（方法 3） | method 4（方法 4） | exact（精确解） |
> |---|---|---|---|---|---|
> | $f_0(p)$ | 0.4693 | 0.8628 | 0.4439 | 0.4198 | 0.3575 |
> | $p_1$ | 0.3448 | 1 | 0.5004 | 1 | 1 |
> | $p_2$ | 0.3448 | 0 | 0.4777 | 0.1165 | 0.2023 |
> | $p_3$ | 0.3448 | 1 | 0.0833 | 0 | 0 |
> | $p_4$ | 0.3448 | 0 | 0.0002 | 0 | 0 |
> | $p_5$ | 0.3448 | 0 | 0.4561 | 1 | 1 |
> | $p_6$ | 0.3448 | 1 | 0.4354 | 0 | 0 |
> | $p_7$ | 0.3448 | 0 | 0.4597 | 1 | 1 |
> | $p_8$ | 0.3448 | 1 | 0.4307 | 0.0249 | 0.1882 |
> | $p_9$ | 0.3448 | 0 | 0.4034 | 0 | 0 |
> | $p_{10}$ | 0.3448 | 1 | 0.4526 | 1 | 1 |
