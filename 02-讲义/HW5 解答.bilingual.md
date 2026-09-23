> <span style="color:#7f8c8d;">Source: 作业解答/HW5 解答.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

## EE364a Homework 5 Solutions（EE364a 作业 5 解答）

> <span style="color:#7f8c8d;">EE364a, Winter 2007-08 · Prof. S. Boyd · 本文档为作业 5 的官方解答，覆盖教材习题 4.15、4.60、5.13、6.2 及附加题 1–6</span>

<!-- p.1 -->

### <span style="color:#c0392b;">4.15 Boolean LP 的松弛（Relaxation of Boolean LP）</span>

<span style="color:#2471a3;">**[question]**</span>

**4.15 Relaxation of Boolean LP.** In a Boolean linear program, the variable $x$ is constrained to have components equal to zero or one:

```math
\begin{array}{ll}
\text{minimize} & c^T x \\
\text{subject to} & Ax \preceq b \\
& x_i \in \{0, 1\}, \quad i = 1, \ldots, n
\end{array}
\tag{1}
```

In general, such problems are very difficult to solve, even though the feasible set is finite (containing at most $2^n$ points). In a general method called relaxation, the constraint that $x_i$ be zero or one is replaced with the linear inequalities $0 \le x_i \le 1$:

```math
\begin{array}{ll}
\text{minimize} & c^T x \\
\text{subject to} & Ax \preceq b \\
& 0 \le x_i \le 1, \quad i = 1, \ldots, n
\end{array}
\tag{2}
```

We refer to this problem as the LP relaxation of the Boolean LP (4.67). The LP relaxation is far easier to solve than the original Boolean LP.

(a) Show that the optimal value of the LP relaxation (4.68) is a lower bound on the optimal value of the Boolean LP (4.67). What can you say about the Boolean LP if the LP relaxation is infeasible?

(b) It sometimes happens that the LP relaxation has a solution with $x_i \in \{0, 1\}$. What can you say in this case?

<span style="color:#2471a3;">**[solution]**</span>

> **解答（Solution）.**
>
> (a) 松弛问题（relaxation）的可行集（feasible set）包含 Boolean LP 的可行集。由此可知：若松弛问题不可行（infeasible），则 Boolean LP 也不可行；并且松弛问题的最优值（optimal value）小于等于 Boolean LP 的最优值。
>
> (b) 此时，松弛问题的最优解同时也是 Boolean LP 的最优解。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> LP 松弛（LP relaxation）是整数规划里最古老、最通用的「先放松、再修正」思想：把离散约束 $x_i \in \{0,1\}$ 换成连续盒子 $0 \le x_i \le 1$，用一点精度换多项式时间可解性。原问题为什么难？可行集至多 $2^n$ 个点，$n = 100$ 时暴力枚举约有 $10^{30}$ 种组合——这正是 6.006 中 NP 难（NP-hard）问题的典型处境：目前没有已知的多项式时间算法，实践中只能靠松弛给出界（bound）、再用启发式找好解（附加题 6 的阈值取整就是一个完整的小型案例）。松弛还有一条「安全」性质：松弛不可行则原问题必不可行（本题 (a) 说的正是它），这使它成为分支定界（branch and bound）等精确算法剪枝的依据。方向也要记牢：极小化问题中可行域扩大只会让最优值不变或变小，因此松弛值是原问题的下界；最大化问题则相反。本题 (b) 则是「运气好」的情形：若松弛最优解恰好自动落在单位立方体的顶点上（$x_i \in \{0,1\}$），松弛就没有任何损失，整数最优解直接到手。

### <span style="color:#c0392b;">4.60 对数最优投资策略（Log-optimal investment strategy）</span>

<span style="color:#2471a3;">**[question]**</span>

**4.60 Log-optimal investment strategy.** We consider a portfolio problem with $n$ assets held over $N$ periods. At the beginning of each period, we re-invest our total wealth, redistributing it over the $n$ assets using a fixed, constant, allocation strategy $x \in \mathbb{R}^n$, where $x \succeq 0$, $\mathbf{1}^T x = 1$. In other words, if $W(t-1)$ is our wealth at the beginning of period $t$, then during period $t$ we invest $x_i W(t-1)$ in asset $i$.

<!-- p.2 -->

We denote by $\lambda(t)$ the total return during period $t$, i.e., $\lambda(t) = W(t)/W(t-1)$. At the end of the $N$ periods our wealth has been multiplied by the factor $\prod_{t=1}^{N} \lambda(t)$. We call

```math
\frac{1}{N} \sum_{t=1}^{N} \log \lambda(t)
```

the growth rate of the investment over the $N$ periods. We are interested in determining an allocation strategy $x$ that maximizes growth of our total wealth for large $N$.

We use a discrete stochastic model to account for the uncertainty in the returns. We assume that during each period there are $m$ possible scenarios, with probabilities $\pi_j$, $j = 1, \ldots, m$. In scenario $j$, the return for asset $i$ over one period is given by $p_{ij}$. Therefore, the return $\lambda(t)$ of our portfolio during period $t$ is a random variable, with $m$ possible values $p_1^T x, \ldots, p_m^T x$, and distribution

```math
\pi_j = \operatorname{prob}(\lambda(t) = p_j^T x), \qquad j = 1, \ldots, m.
```

We assume the same scenarios for each period, with (identical) independent distributions. Using the law of large numbers, we have

```math
\lim_{N \to \infty} \frac{1}{N} \log\!\left( \frac{W(N)}{W(0)} \right) = \lim_{N \to \infty} \frac{1}{N} \sum_{t=1}^{N} \log \lambda(t) = \mathbb{E} \log \lambda(t) = \sum_{j=1}^{m} \pi_j \log(p_j^T x).
```

In other words, with investment strategy $x$, the long term growth rate is given by

```math
R_{\mathrm{lt}} = \sum_{j=1}^{m} \pi_j \log(p_j^T x).
```

The investment strategy $x$ that maximizes this quantity is called the log-optimal investment strategy, and can be found by solving the optimization problem

```math
\begin{array}{ll}
\text{maximize} & \sum_{j=1}^{m} \pi_j \log(p_j^T x) \\
\text{subject to} & x \succeq 0, \quad \mathbf{1}^T x = 1,
\end{array}
```

with variable $x \in \mathbb{R}^n$. Show that this is a convex optimization problem.

<span style="color:#2471a3;">**[solution]**</span>

> **解答（Solution）.** 其实这道题没有多少要做的：约束 $x \succeq 0$、$\mathbf{1}^T x = 1$ 显然是凸的（convex），所以只需证明目标函数是凹的（concave）（因为问题是最大化）。只需几步：首先，注意 $\log$ 是凹函数，因此 $\log(p_j^T x)$ 作为 $x$ 的函数是凹的（其定义域为开半空间（open halfspace）$\{x \mid p_j^T x > 0\}$）。由于 $\pi_j \ge 0$，我们得出结论：凹函数之和
>
> ```math
> \sum_{j=1}^{m} \pi_j \log(p_j^T x)
> ```
>
> 是凹的。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 为什么优化目标是 $\mathbb{E}\log \lambda$ 而不是期望收益 $\mathbb{E}\lambda$？第一性原理在于复利：$N$ 期后的总财富是乘积 $\prod_t \lambda(t)$，取对数才把乘法变加法，大数定律（law of large numbers，Bertsekas 概率论的核心定理之一）才有用武之地——对数收益的时间平均几乎必然收敛到它的期望。这正是 Kelly 公式（Kelly criterion，1956 年 John Kelly 在贝尔实验室研究信息传输速率时提出）的灵魂：最大化期望对数增长就是最大化长期复利增速，而且 $\log$ 在 $0$ 处发散到 $-\infty$，等于对「血本无归」的场景施加无穷惩罚，策略因此自动规避破产风险。本文后面的数值也很说明问题：对数最优策略 $R_{\mathrm{lt}} = 0.0231$，均匀分配只有 $0.0114$，且后者的财富轨迹波动剧烈。与 Strang 线代的呼应：$\log$ 的凹性（$\log'' x = -1/x^2 < 0$）是整个问题成为凸优化的全部原因，「非负系数加权和保持凹性」正是凹函数封闭性质的直接应用，Jensen 不等式（Jensen's inequality）同样可以由它立得。

<!-- p.3 -->

### <span style="color:#c0392b;">5.13 Boolean LP 的拉格朗日松弛（Lagrangian relaxation of Boolean LP）</span>

<span style="color:#2471a3;">**[question]**</span>

**5.13 Lagrangian relaxation of Boolean LP.** A Boolean linear program is an optimization problem of the form

```math
\begin{array}{ll}
\text{minimize} & c^T x \\
\text{subject to} & Ax \preceq b \\
& x_i \in \{0, 1\}, \quad i = 1, \ldots, n,
\end{array}
```

and is, in general, very difficult to solve. In exercise 4.15 we studied the LP relaxation of this problem,

```math
\begin{array}{ll}
\text{minimize} & c^T x \\
\text{subject to} & Ax \preceq b \\
& 0 \le x_i \le 1, \quad i = 1, \ldots, n,
\end{array}
\tag{3}
```

which is far easier to solve, and gives a lower bound on the optimal value of the Boolean LP. In this problem we derive another lower bound for the Boolean LP, and work out the relation between the two lower bounds.

(a) **Lagrangian relaxation.** The Boolean LP can be reformulated as the problem

```math
\begin{array}{ll}
\text{minimize} & c^T x \\
\text{subject to} & Ax \preceq b \\
& x_i(1 - x_i) = 0, \quad i = 1, \ldots, n,
\end{array}
```

which has quadratic equality constraints. Find the Lagrange dual of this problem. The optimal value of the dual problem (which is convex) gives a lower bound on the optimal value of the Boolean LP. This method of finding a lower bound on the optimal value is called Lagrangian relaxation.

(b) Show that the lower bound obtained via Lagrangian relaxation, and via the LP relaxation (5.107), are the same. **Hint.** Derive the dual of the LP relaxation (5.107).

<span style="color:#2471a3;">**[solution]**</span>

> **解答（Solution）.**
>
> (a) 拉格朗日函数（Lagrangian）为
>
> ```math
> \begin{aligned}
> L(x, \mu, \nu) &= c^T x + \mu^T(Ax - b) - \nu^T x + x^T \operatorname{diag}(\nu)\, x \\
> &= x^T \operatorname{diag}(\nu)\, x + (c + A^T \mu - \nu)^T x - b^T \mu.
> \end{aligned}
> ```
>
> 对 $x$ 极小化，得到对偶函数（dual function）
>
> ```math
> g(\mu, \nu) =
> \begin{cases}
> -b^T \mu - (1/4) \sum_{i=1}^{n} (c_i + a_i^T \mu - \nu_i)^2 / \nu_i, & \nu \succeq 0, \\
> -\infty, & \text{otherwise},
> \end{cases}
> ```
>
> 其中 $a_i$ 是 $A$ 的第 $i$ 列，并采用如下约定：当 $a \ne 0$ 时 $a^2/0 = \infty$，当 $a = 0$ 时 $a^2/0 = 0$。

<!-- p.4 -->

> 由此得到的对偶问题（dual problem）为
>
> ```math
> \begin{array}{ll}
> \text{maximize} & -b^T \mu - \frac{1}{4} \sum_{i=1}^{n} (c_i + a_i^T \mu - \nu_i)^2 / \nu_i \\
> \text{subject to} & \nu \succeq 0, \quad \mu \succeq 0.
> \end{array}
> ```
>
> 为了化简这个对偶，我们对 $\nu$ 做解析优化：注意到
>
> ```math
> \sup_{\nu_i \ge 0} \left( -\frac{(c_i + a_i^T \mu - \nu_i)^2}{\nu_i} \right)
> =
> \begin{cases}
> 4(c_i + a_i^T \mu), & c_i + a_i^T \mu \le 0, \\
> 0, & c_i + a_i^T \mu \ge 0,
> \end{cases}
> = \min\{0,\; 4(c_i + a_i^T \mu)\}.
> ```
>
> 这使我们可以从对偶问题中消去 $\nu$，把它化简为
>
> ```math
> \begin{array}{ll}
> \text{maximize} & -b^T \mu + \sum_{i=1}^{n} \min\{0,\; c_i + a_i^T \mu\} \\
> \text{subject to} & \mu \succeq 0.
> \end{array}
> ```
>
> (b) 我们按提示来做。LP 松弛的拉格朗日函数与对偶函数为
>
> ```math
> \begin{aligned}
> & \; L(x, u, v, w) = c^T x + u^T(Ax - b) - v^T x + w^T(x - \mathbf{1}) \\
> & \qquad = (c + A^T u - v + w)^T x - b^T u - \mathbf{1}^T w, \\
> & \; g(u, v, w) =
> \begin{cases}
> -b^T u - \mathbf{1}^T w, & A^T u - v + w + c = 0, \\
> -\infty, & \text{otherwise}.
> \end{cases}
> \end{aligned}
> ```
>
> 对偶问题为
>
> ```math
> \begin{array}{ll}
> \text{maximize} & -b^T u - \mathbf{1}^T w \\
> \text{subject to} & A^T u - v + w + c = 0 \\
> & u \succeq 0, \quad v \succeq 0, \quad w \succeq 0,
> \end{array}
> ```
>
> 它与上面导出的拉格朗日松弛问题等价。我们得出结论：两种松弛给出相同的值。

### <span style="color:#c0392b;">6.2 用常向量做 ℓ1、ℓ2 与 ℓ∞ 范数逼近（ℓ1-, ℓ2-, and ℓ∞-norm approximation by a constant vector）</span>

<span style="color:#2471a3;">**[question]**</span>

**6.2 ℓ1-, ℓ2-, and ℓ∞-norm approximation by a constant vector.** What is the solution of the norm approximation problem with one scalar variable $x \in \mathbb{R}$,

```math
\text{minimize} \quad \| x\mathbf{1} - b \|,
```

for the ℓ1-, ℓ2-, and ℓ∞-norms?

<span style="color:#2471a3;">**[solution]**</span>

> **解答（Solution）.**
>
> (a) ℓ2 范数（ℓ2-norm）：平均值（average）$\mathbf{1}^T b / m$。
>
> (b) ℓ1 范数（ℓ1-norm）：$b$ 的各分量的（某一个）中位数（median）。
>
> (c) ℓ∞ 范数（ℓ∞-norm）：中程点（midrange point）$(\max_i b_i - \min_i b_i)/2$。

<!-- p.5 -->

## <span style="color:#c0392b;">附加习题解答（Solutions to additional exercises）</span>

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">附加题 1 舒尔补（Schur complements）</span>

<span style="color:#2471a3;">**[question]**</span>

**1. Schur complements.** Consider a matrix $X = X^T \in \mathbb{R}^{n \times n}$ partitioned as

```math
X = \begin{bmatrix} A & B \\ B^T & C \end{bmatrix},
```

where $A \in \mathbb{R}^{k \times k}$. If $\det A \ne 0$, the matrix $S = C - B^T A^{-1} B$ is called the Schur complement of $A$ in $X$. Schur complements arise in many situations and appear in many important formulas and theorems. For example, we have $\det X = \det A \det S$. (You don't have to prove this.)

(a) The Schur complement arises when you minimize a quadratic form over some of the variables. Let $f(u, v) = [\,u^T \;\; v^T\,]\, X\, [\,u^T \;\; v^T\,]^T$, where $u \in \mathbb{R}^k$. Let $g(v)$ be the minimum value of $f$ over $u$, i.e., $g(v) = \inf_u f(u, v)$. Of course $g(v)$ can be $-\infty$. Show that if $A \succ 0$, we have $g(v) = v^T S v$.

(b) The Schur complement arises in several characterizations of positive definiteness or semidefiniteness of a block matrix. As examples we have the following three theorems:

- $X \succ 0$ if and only if $A \succ 0$ and $S \succ 0$.
- If $A \succ 0$, then $X \succeq 0$ if and only if $S \succeq 0$.
- $X \succeq 0$ if and only if $A \succeq 0$, $B^T(I - AA^\dagger) = 0$ and $C - B^T A^\dagger B \succeq 0$, where $A^\dagger$ is the pseudo-inverse of $A$. ($C - B^T A^\dagger B$ serves as a generalization of the Schur complement in the case where $A$ is positive semidefinite but singular.)

Prove one of these theorems. (You can choose which one.)

(c) Recognizing Schur complements often helps to represent nonlinear convex constraints as linear matrix inequalities (LMIs). Consider the function

```math
f(x) = (Ax + b)^T \left( P_0 + x_1 P_1 + \cdots + x_n P_n \right)^{-1} (Ax + b),
```

where $A \in \mathbb{R}^{m \times n}$, $b \in \mathbb{R}^m$, and $P_i = P_i^T \in \mathbb{R}^{m \times m}$, with domain

```math
\operatorname{dom} f = \{ x \in \mathbb{R}^n \mid P_0 + x_1 P_1 + \cdots + x_n P_n \succ 0 \}.
```

This is the composition of the matrix fractional function and an affine mapping, and so is convex. Give an LMI representation of $\operatorname{epi} f$. That is, find a symmetric matrix $F(x, t)$, affine in $(x, t)$, for which

```math
x \in \operatorname{dom} f, \qquad f(x) \le t \;\Longleftrightarrow\; F(x, t) \succeq 0.
```

<span style="color:#2471a3;">**[solution]**</span>

<!-- p.6 -->

> **解答（Solution）.**
>
> (a) 若 $A \succ 0$，则 $g(v) = v^T S v$。
>
> 我们有 $f(u, v) = u^T A u + 2 v^T B u + v^T C v$。若 $A \succ 0$，可以令 $f$ 对 $u$ 的梯度等于零，从而在 $u$ 上极小化 $f$。我们得到 $u^\star(v) = -A^{-1} B v$，于是
>
> ```math
> g(v) = f(u^\star(v), v) = v^T (C - B^T A^{-1} B)\, v = v^T S v.
> ```
>
> (b) 正定与半正定分块矩阵。
>
> **• $X \succ 0$ 当且仅当 $A \succ 0$ 且 $S \succ 0$。**
>
> 设 $X \succ 0$。则对一切非零的 $(u, v)$ 都有 $f(u, v) > 0$；特别地，对一切非零 $u$ 有 $f(u, 0) = u^T A u > 0$（因此 $A \succ 0$），并且 $f(-A^{-1} B v, v) = v^T (C - B^T A^{-1} B) v > 0$（因此 $S = C - B^T A^{-1} B \succ 0$）。这就证明了「仅当」（only if）部分。
>
> 为证明「当」（if）部分，需要证明：若 $A \succ 0$ 且 $S \succ 0$，则对一切非零的 $(u, v)$（即 $u, v$ 不同时为零）都有 $f(u, v) > 0$。若 $v \ne 0$，则由 (a) 得
>
> ```math
> f(u, v) \ge \inf_u f(u, v) = v^T S v > 0.
> ```
>
> 若 $v = 0$ 且 $u \ne 0$，则 $f(u, 0) = u^T A u > 0$。
>
> **• 若 $A \succ 0$，则 $X \succeq 0$ 当且仅当 $S \succeq 0$。**
>
> 由 (a) 可知：若 $A \succ 0$，则 $\inf_u f(u, v) = v^T S v$。若 $S \succeq 0$，则对一切 $u, v$ 都有
>
> ```math
> f(u, v) \ge \inf_u f(u, v) = v^T S v \ge 0,
> ```
>
> 从而 $X \succeq 0$。这就证明了「当」部分。为证明「仅当」部分，注意：$f(u, v) \ge 0$ 对一切 $(u, v)$ 成立，意味着 $\inf_u f(u, v) \ge 0$ 对一切 $v$ 成立，即 $S \succeq 0$。
>
> **• $X \succeq 0$ 当且仅当 $A \succeq 0$、$B^T(I - AA^\dagger) = 0$ 且 $C - B^T A^\dagger B \succeq 0$。**
>
> 设 $A \in \mathbb{R}^{k \times k}$ 且 $\operatorname{rank}(A) = r$。则存在矩阵 $Q_1 \in \mathbb{R}^{k \times r}$、$Q_2 \in \mathbb{R}^{k \times (k - r)}$ 与可逆对角矩阵 $\Lambda \in \mathbb{R}^{r \times r}$，使得
>
> ```math
> A = \left[\, Q_1 \quad Q_2 \,\right]
> \begin{bmatrix} \Lambda & 0 \\ 0 & 0 \end{bmatrix}
> \left[\, Q_1 \quad Q_2 \,\right]^T,
> ```
>
> 且 $[\,Q_1 \;\; Q_2\,]^T [\,Q_1 \;\; Q_2\,] = I$。矩阵
>
> ```math
> \begin{bmatrix} Q_1 & Q_2 & 0 \\ 0 & 0 & I \end{bmatrix} \in \mathbb{R}^{n \times n}
> ```
>
> 非奇异，因此
>
> ```math
> \begin{bmatrix} A & B \\ B^T & C \end{bmatrix} \succeq 0
> \;\Longleftrightarrow\;
> \begin{bmatrix} Q_1 & Q_2 & 0 \\ 0 & 0 & I \end{bmatrix}^{\!T}
> \begin{bmatrix} A & B \\ B^T & C \end{bmatrix}
> \begin{bmatrix} Q_1 & Q_2 & 0 \\ 0 & 0 & I \end{bmatrix} \succeq 0
> ```

<!-- p.7 -->

> ```math
> \begin{aligned}
> &\;\Longleftrightarrow\;
> \begin{bmatrix}
> \Lambda & 0 & Q_1^T B \\
> 0 & 0 & Q_2^T B \\
> B^T Q_1 & B^T Q_2 & C
> \end{bmatrix} \succeq 0 \\
> &\;\Longleftrightarrow\;
> Q_2^T B = 0, \quad
> \begin{bmatrix} \Lambda & Q_1^T B \\ B^T Q_1 & C \end{bmatrix} \succeq 0.
> \end{aligned}
> ```
>
> 我们有：$\Lambda \succ 0$ 当且仅当 $A \succeq 0$。可以验证
>
> ```math
> A^\dagger = Q_1 \Lambda^{-1} Q_1^T, \qquad I - AA^\dagger = Q_2 Q_2^T.
> ```
>
> 因此
>
> ```math
> Q_2^T B = 0 \;\Longleftrightarrow\; Q_2 Q_2^T B = (I - A^\dagger A) B = 0.
> ```
>
> 此外，由于 $\Lambda$ 可逆，
>
> ```math
> \begin{bmatrix} \Lambda & Q_1^T B \\ B^T Q_1 & C \end{bmatrix} \succeq 0
> \;\Longleftrightarrow\;
> \Lambda \succ 0, \;\; C - B^T Q_1 \Lambda^{-1} Q_1^T B = C - B^T A^\dagger B \succeq 0.
> ```
>
> (c) $f$ 的上图（epigraph）是满足 $P_0 + x_1 P_1 + \cdots + x_n P_n \succ 0$ 且
>
> ```math
> (Ax + b)^T \left( P_0 + x_1 P_1 + \cdots + x_n P_n \right)^{-1} (Ax + b) \le t
> ```
>
> 的点 $(x, t)$ 全体。利用 (b) 中的第二个结论，可以把第二个不等式写成
>
> ```math
> \begin{bmatrix}
> t & (Ax + b)^T \\
> Ax + b & P_0 + x_1 P_1 + \cdots + x_n P_n
> \end{bmatrix} \succeq 0.
> ```
>
> 这是关于变量 $x, t$ 的线性矩阵不等式（linear matrix inequality，LMI），即一个凸约束。

### <span style="color:#c0392b;">附加题 2 将下列优化问题表述为半定规划（Formulate as semidefinite programs）</span>

<span style="color:#2471a3;">**[question]**</span>

**2.** Formulate the following optimization problems as semidefinite programs. The variable is $x \in \mathbb{R}^n$; $F(x)$ is defined as

```math
F(x) = F_0 + x_1 F_1 + x_2 F_2 + \cdots + x_n F_n
```

with $F_i \in \mathbb{S}^m$. The domain of $f$ in each subproblem is $\operatorname{dom} f = \{ x \in \mathbb{R}^n \mid F(x) \succ 0 \}$.

(a) Minimize $f(x) = c^T F(x)^{-1} c$ where $c \in \mathbb{R}^m$.

(b) Minimize $f(x) = \max_{i=1,\ldots,K} c_i^T F(x)^{-1} c_i$ where $c_i \in \mathbb{R}^m$, $i = 1, \ldots, K$.

(c) Minimize $f(x) = \sup_{\|c\|_2 \le 1} c^T F(x)^{-1} c$.

(d) Minimize $f(x) = \mathbb{E}\!\left( c^T F(x)^{-1} c \right)$ where $c$ is a random vector with mean $\mathbb{E}\, c = \bar{c}$ and covariance $\mathbb{E}(c - \bar{c})(c - \bar{c})^T = S$.

<span style="color:#2471a3;">**[solution]**</span>

<!-- p.8 -->

> **解答（Solution）.**
>
> (a)
>
> ```math
> \begin{array}{ll}
> \text{minimize} & t \\
> \text{subject to} & \begin{bmatrix} F(x) & c \\ c^T & t \end{bmatrix} \succeq 0.
> \end{array}
> ```
>
> (b)
>
> ```math
> \begin{array}{ll}
> \text{minimize} & t \\
> \text{subject to} & \begin{bmatrix} F(x) & c_i \\ c_i^T & t \end{bmatrix} \succeq 0, \quad i = 1, \ldots, K.
> \end{array}
> ```
>
> (c) $f(x) = \lambda_{\max}(F(x)^{-1})$，因此 $f(x) \le t$ 当且仅当 $F(x)^{-1} \preceq tI$。利用舒尔补（Schur complement）可得
>
> ```math
> \begin{array}{ll}
> \text{minimize} & t \\
> \text{subject to} & \begin{bmatrix} F(x) & I \\ I & tI \end{bmatrix} \succeq 0.
> \end{array}
> ```
>
> (d) $f(x) = \bar{c}^T F(x)^{-1} \bar{c} + \operatorname{tr}(F(x)^{-1} S)$。若把 $S$ 分解为 $S = \sum_{k=1}^{m} c_k c_k^T$，则问题等价于
>
> ```math
> \operatorname{minimize} \quad \bar{c}^T F(x)^{-1} \bar{c} + \sum_{k=1}^{m} c_k^T F(x)^{-1} c_k,
> ```
>
> 从而可以写成如下 SDP（semidefinite program，半定规划）：
>
> ```math
> \begin{array}{ll}
> \text{minimize} & t_0 + \sum_k t_k \\
> \text{subject to} &
> \begin{bmatrix} F(x) & \bar{c} \\ \bar{c}^T & t_0 \end{bmatrix} \succeq 0, \\
> & \begin{bmatrix} F(x) & c_k \\ c_k^T & t_k \end{bmatrix} \succeq 0, \quad k = 1, \ldots, m.
> \end{array}
> ```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 舒尔补（Schur complement）本质上是分块高斯消元在正定性世界的化身：想验证 $X \succeq 0$，先用左上块 $A$ 消去左下角，剩下的「余项」$S = C - B^T A^{-1} B$ 是否半正定就决定了答案——用 Strang 线代的语言说，这就是对 $X$ 做 $LDL^T$ 分解后中间那个块对角阵的第二个块。附加题 1(a) 还给出了能量视角：二次型 $f(u,v)$ 先对 $u$ 取最小，恰好「漏」下 $v^T S v$，等价于把二次型投影到余下的变量上。1(b) 第三条定理中的伪逆（pseudo-inverse）$A^\dagger$ 则把消元推广到奇异 $A$：按 SVD 视角，$A^\dagger = Q_1 \Lambda^{-1} Q_1^T$ 只在非零奇异值方向取倒数，$AA^\dagger$ 是到列空间的投影，而条件 $B^T(I - AA^\dagger) = 0$ 正是在要求 $B$ 不把能量漏进 $A$ 的零空间。本道附加题则是舒尔补的收获季：四个小题用的是同一招——把 $c^T F(x)^{-1} c \le t$ 这类分式不等式一次性抬升为 LMI，矩阵分式型目标就此全部进入半定规划（SDP）的射程。

### <span style="color:#c0392b;">附加题 3 对数最优投资问题的最优性条件与对偶（Optimality conditions and dual for log-optimal investment problem）</span>

<span style="color:#2471a3;">**[question]**</span>

**3. Optimality conditions and dual for log-optimal investment problem.**

(a) Show that the optimality conditions for the log-optimal investment problem described in exercise 4.60 can be expressed as: $\mathbf{1}^T x = 1$, $x \succeq 0$, and for each $i$,

```math
\begin{aligned}
x_i > 0 &\implies \sum_{j=1}^{m} \pi_j \frac{p_{ij}}{p_j^T x} = 1, \\
x_i = 0 &\implies \sum_{j=1}^{m} \pi_j \frac{p_{ij}}{p_j^T x} \le 1.
\end{aligned}
```

We can interpret this as follows. $p_{ij} / p_j^T x$ is a random variable, which gives the ratio of the investment gain with asset $i$ only, to the investment gain with our mixed portfolio $x$. The optimality condition is that, for each asset we invest in, the expected value of this ratio is one, and for each asset we do not invest in, the expected value cannot exceed one. Very roughly speaking, this means our portfolio does as well as any of the assets that we choose to invest in, and cannot do worse than any assets that we do not invest in.

**Hint.** You can start from the simple criterion given in §4.2.3, or the KKT conditions, or additional exercise 1 from homework 4.

<!-- p.9 -->

(b) In this part we will derive the dual of the log-optimal investment problem. We start by writing the problem as

```math
\begin{array}{ll}
\text{minimize} & -\sum_{j=1}^{m} \pi_j \log y_j \\
\text{subject to} & y = P^T x \\
& x \succeq 0 \\
& \mathbf{1}^T x = 1.
\end{array}
```

Here, $P$ has columns $p_1, \ldots, p_m$, and we have introduced new variables $y_1, \ldots, y_m$, with the implicit constraint $y \succ 0$. We will associate dual variables $\nu$, $\lambda$ and $\nu_0$ with the constraints $y = P^T x$, $x \succeq 0$, and $\mathbf{1}^T x = 1$, respectively.

Defining $\tilde{\nu}_j = \nu_j / \nu_0$ for $j = 1, \ldots, m$, show that the dual problem can be written as

```math
\begin{array}{ll}
\text{maximize} & \sum_{j=1}^{m} \pi_j \log(\tilde{\nu}_j / \pi_j) \\
\text{subject to} & P \tilde{\nu} \preceq \mathbf{1},
\end{array}
```

with variable $\tilde{\nu}$. The objective here is the (negative) Kullback-Leibler divergence between the given distribution $\pi$ and the dual variable $\tilde{\nu}$.

<span style="color:#2471a3;">**[solution]**</span>

> **解答（Solution）.**
>
> (a) 该问题等价于在概率单纯形（probability simplex）上极小化 $f(x) = -\sum_{j=1}^{m} \pi_j \log(p_j^T x)$。<span style="color:#00838f;">若 $x$ 可行，则最优性条件（optimality condition）为：对所有满足 $z \succeq 0$、$\mathbf{1}^T z = 1$ 的 $z$，都有 $\nabla f(x)^T (z - x) \ge 0$。</span>这当且仅当对每个 $k$，
>
> ```math
> x_k > 0 \implies \frac{\partial f}{\partial x_k} = \min_{i=1,\ldots,n} \frac{\partial f}{\partial x_i} = \nabla f(x)^T x.
> ```
>
> 由 $f(x) = -\sum_{j=1}^{m} \pi_j \log(p_j^T x)$ 可得
>
> ```math
> \nabla f(x)_i = -\sum_{j=1}^{m} \pi_j \left( 1 / p_j^T x \right) p_{ij},
> ```
>
> 因此
>
> ```math
> \nabla f(x)^T x = -\sum_{j=1}^{m} \pi_j \left( 1 / p_j^T x \right) p_j^T x = -1.
> ```
>
> 最优性条件为：$\mathbf{1}^T x = 1$、$x \succeq 0$，且对每个 $i$，
>
> ```math
> \begin{aligned}
> x_i > 0 &\implies \sum_{j=1}^{m} \pi_j \frac{p_{ij}}{p_j^T x} = 1, \\
> x_i = 0 &\implies \sum_{j=1}^{m} \pi_j \frac{p_{ij}}{p_j^T x} \le 1.
> \end{aligned}
> ```
>
> (b) 拉格朗日函数为
>
> ```math
> L(x, \nu, \lambda, \nu_0) = -\sum_{j=1}^{m} \pi_j \log y_j + \nu^T (y - P^T x) - \lambda^T x + \nu_0 (\mathbf{1}^T x - 1).
> ```

<!-- p.10 -->

> 该式除非 $-\nu^T P^T - \lambda^T + \nu_0 \mathbf{1}^T = 0$，否则无下界。由于 $L$ 关于 $y$ 是可分的（separable），可以通过对每个 $y_j$ 分别极小化来求 $L$ 在 $y$ 上的极小值。我们求得极小值在 $y_j = \pi_j / \nu_j$ 处取得，因此
>
> ```math
> g(\nu, \lambda, \nu_0) = 1 - \nu_0 + \sum_{j=1}^{m} \pi_j \log(\nu_j / \pi_j),
> ```
>
> 前提是 $P \nu \preceq \nu_0 \mathbf{1}$。于是对偶问题可写为
>
> ```math
> \begin{array}{ll}
> \text{maximize} & 1 - \nu_0 + \sum_{j=1}^{m} \pi_j \log(\nu_j / \pi_j) \\
> \text{subject to} & P \nu \preceq \nu_0 \mathbf{1}
> \end{array}
> ```
>
> 变量为 $\nu \in \mathbb{R}^m$、$\nu_0 \in \mathbb{R}$，并带有隐式约束（implicit constraint）$\nu \succ 0$。
>
> 我们可以进一步化简：对 $\nu_0$ 做解析优化。由约束不等式可见 $\nu_0 > 0$。定义 $\tilde{\nu} = \nu / \nu_0$，得到以 $\tilde{\nu}$、$\nu_0$ 为变量的问题
>
> ```math
> \begin{array}{ll}
> \text{maximize} & 1 - \nu_0 + \sum_{j=1}^{m} \pi_j \log(\tilde{\nu}_j \nu_0 / \pi_j) \\
> \text{subject to} & \nu_0 P \tilde{\nu} \preceq \nu_0 \mathbf{1}.
> \end{array}
> ```
>
> 从约束中约去 $\nu_0$，得
>
> ```math
> \begin{array}{ll}
> \text{maximize} & 1 - \nu_0 + \log \nu_0 + \sum_{j=1}^{m} \pi_j \log(\tilde{\nu}_j / \pi_j) \\
> \text{subject to} & P \tilde{\nu} \preceq \mathbf{1}.
> \end{array}
> ```
>
> $\nu_0$ 的最优值显然为 $\nu_0 = 1$，于是最终得到对偶问题
>
> ```math
> \begin{array}{ll}
> \text{maximize} & \sum_{j=1}^{m} \pi_j \log(\tilde{\nu}_j / \pi_j) \\
> \text{subject to} & P \tilde{\nu} \preceq \mathbf{1}.
> \end{array}
> ```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这道题的对偶目标是 $\sum_j \pi_j \log(\tilde{\nu}_j / \pi_j)$，恰为 KL 散度（Kullback-Leibler divergence，又称相对熵）的负值：由 Gibbs 不等式（你在 Bertsekas 概率论里处理极大似然时已用过它的特例）知 $\sum_j \pi_j \log(\pi_j / \tilde{\nu}_j) \ge 0$，等号当且仅当两个分布相同，所以该目标恒 $\le 0$。对偶变量 $\tilde{\nu}$ 的经济学身份是状态价格（state price）：把 $P \tilde{\nu} \preceq \mathbf{1}$ 与 $x \succeq 0$、$\mathbf{1}^T x = 1$ 结合看，它等价于说任何成本为 1 的组合按 $\tilde{\nu}$ 加权的赔付不超过 1，即不存在按 $\tilde{\nu}$ 计价的套利；对偶问题便是在全体「无套利」定价中，寻找与真实分布 $\pi$ 相对熵最小的一个——这正是「最小相对熵」方法的雏形，把 Kelly 准则与信息论焊接在一起。推导过程本身也是范本：先消去 $\lambda$（不等式约束化为不等式），再对 $\nu_0$ 做一维解析优化（$\max_{\nu_0 > 0}(1 - \nu_0 + \log \nu_0) = 0$ 在 $\nu_0 = 1$ 处取得），层层化简后结构自然浮现——这种「解析消元」套路在习题 5.13 与附加题 5 中还会反复出现，值得收藏。

### <span style="color:#c0392b;">附加题 4 对数最优投资策略（数值实验）（Log-optimal investment strategy）</span>

<span style="color:#2471a3;">**[question]**</span>

**4. Log-optimal investment strategy.** In this problem you will solve a specific instance of the log-optimal investment problem described in exercise 4.60, with $n = 5$ assets and $m = 10$ possible outcomes in each period. The problem data are defined in `log_opt_invest.m`, with the rows of the matrix $P$ giving the asset return vectors $p_j^T$. The outcomes are equiprobable, i.e., we have $\pi_j = 1/m$. Each column of the matrix $P$ gives the return of the associated asset in the different posible outcomes. You can examine the columns to get an idea of the types of assets. For example, the last asset gives a fixed and certain return of 1%; the first asset is a very risky one, with occasional large return, and (more often) substantial loss.

Find the log-optimal investment strategy $x^\star$, and its associated long term growth rate $R^\star_{\mathrm{lt}}$. Compare this to the long term growth rate obtained with a uniform allocation strategy, i.e., $x = (1/n)\mathbf{1}$, and also with a pure investment in each asset. For the optimal investment strategy, and also the uniform investment strategy, plot 10 sample trajectories of the accumulated wealth, i.e., $W(T) = W(0) \prod_{t=1}^{T} \lambda(t)$, for $T = 0, \ldots, 200$, with initial wealth $W(0) = 1$.

<!-- p.11 -->

To save you the trouble of figuring out how to simulate the wealth trajectories or plot them nicely, we've included the simulation and plotting code in `log_opt_invest.m`; you just have to add the code needed to find $x^\star$.

**Hint:** The current version of cvx doesn't handle the logarithm, but you can use `geomean()` to solve the problem.

<span style="color:#2471a3;">**[solution]**</span>

> **解答（Solution）.**
>
> (a) 求解本问题所用代码如下：
>
> ```matlab
> P = [3.5000    1.1100    1.1100    1.0400    1.0100;
>      0.5000    0.9700    0.9800    1.0500    1.0100;
>      0.5000    0.9900    0.9900    0.9900    1.0100;
>      0.5000    1.0500    1.0600    0.9900    1.0100;
>      0.5000    1.1600    0.9900    1.0700    1.0100;
>      0.5000    0.9900    0.9900    1.0600    1.0100;
>      0.5000    0.9200    1.0800    0.9900    1.0100;
>      0.5000    1.1300    1.1000    0.9900    1.0100;
>      0.5000    0.9300    0.9500    1.0400    1.0100;
>      3.5000    0.9900    0.9700    0.9800    1.0100];
> [m,n] = size(P);
> % Find log-optimal investment policy
> cvx_begin
>     variable x_opt(n)
>     maximize(geomean(P*x_opt))
>     sum(x_opt) == 1
>     x_opt >= 0
> cvx_end
> x_opt
> x_unif = ones(n,1)/n;
> R_opt = sum(log(P*x_opt))/m
> R_unif = sum(log(P*x_unif))/m
> ```
>
> 求得对数最优投资策略为
>
> ```math
> x_{\mathrm{opt}} = (0.0580,\; 0.4000,\; 0.2923,\; 0.2497,\; 0.0000).
> ```
>
> 该策略实现的长期增长率（long term growth rate）为 $R^{*}_{\mathrm{lt}} = 0.0231$；相比之下，均匀分配策略（uniform allocation strategy）的增长率为 $R_{\mathrm{unif}} = 0.0114$。

<!-- p.12 -->

> 显然，资产 1 是高风险资产：投入该资产的财富将以 3.50 的倍数增长（概率 20%），否则减半（概率 80%）。另一方面，资产 5 是每期收益率确定为 1% 的资产。最后，资产 2、3、4 是低风险资产。结果表明，本例的对数最优策略是：只把很少的财富投入高风险资产、完全不投入确定收益资产，而把大部分财富投入资产 2。
>
> (b) 生成随机事件序列与轨迹图的代码如下：
>
> ```matlab
> % Generate random event sequences
> rand('state',0);
> N = 10; T = 200;
> w_opt = []; w_unif = [];
> for i = 1:N
>     events = ceil(rand(1,T)*m);
>     P_event = P(events,:);
>     w_opt = [w_opt [1; cumprod(P_event*x_opt)]];
>     w_unif = [w_unif [1; cumprod(P_event*x_unif)]];
> end
> % Plot wealth versus time
> figure
> semilogy(w_opt,'g')
> hold on
> semilogy(w_unif,'r--')
> grid
> axis tight
> xlabel('time')
> ylabel('wealth')
> ```
>
> 由此生成下图：

<!-- p.13 -->

> <span style="color:#7f8c8d;">**图（原 PDF 插图）:** 对数最优策略与均匀分配策略的财富轨迹（semilogy 半对数图）。横轴为 $t$（约 20–200），纵轴为财富 $W$（$10^{-1}$ 至 $10^{3}$，对数刻度）；绿色实线为对数最优策略，红色虚线为均匀分配策略。</span>
>
> 对数最优投资策略使财富持续稳定增长。相比之下，均匀分配策略生成的轨迹相当随机：少数轨迹财富大幅增长，而许多表现不佳。原因在于该策略把 20% 的财富投入了高风险资产。

### <span style="color:#c0392b;">附加题 5 赌局中庄家利润最大化与隐含概率（Maximizing house profit in a gamble and imputed probabilities）</span>

<span style="color:#2471a3;">**[question]**</span>

**5. Maximizing house profit in a gamble and imputed probabilities.** A set of $n$ participants bet on which one of $m$ outcomes, labeled $1, \ldots, m$, will occur. Participant $i$ offers to purchase up to $q_i > 0$ gambling contracts, at price $p_i > 0$, that the true outcome will be in the set $S_i \subset \{1, \ldots, m\}$. The house then sells her $x_i$ contracts, with $0 \le x_i \le q_i$. If the true outcome $j$ is in $S_i$, then participant $i$ receives \$1 per contract, i.e., $x_i$. Otherwise, she loses, and receives nothing. The house collects a total of $x_1 p_1 + \cdots + x_n p_n$, and pays out an amount that depends on the outcome $j$,

```math
\sum_{i:\, j \in S_i} x_i.
```

The difference is the house profit.

(a) **Optimal house strategy.** How should the house decide on $x$ so that its worst-case profit (over the possible outcomes) is maximized? (The house determines $x$ after examining all the participant offers.)

(b) **Imputed probabilities.** Suppose $x^\star$ maximizes the worst-case house profit. Show that there exists a probability distribution $\pi$ on the possible outcomes (i.e., $\pi \in \mathbb{R}^m_+$, $\mathbf{1}^T \pi = 1$) for which $x^\star$ also maximizes the expected house profit. Explain how to find $\pi$.

<!-- p.14 -->

**Hint.** Formulate the problem in part (a) as an LP; you can construct $\pi$ from optimal dual variables for this LP.

**Remark.** Given $\pi$, the 'fair' price for offer $i$ is $p^{\mathrm{fair}}_i = \sum_{j \in S_i} \pi_j$. All offers with $p_i > p^{\mathrm{fair}}_i$ will be completely filled (i.e., $x_i = q_i$); all offers with $p_i < p^{\mathrm{fair}}_i$ will be rejected (i.e., $x_i = 0$).

**Remark.** This exercise shows how the probabilities of outcomes (e.g., elections) can be guessed from the offers of a set of gamblers.

(c) **Numerical example.** Carry out your method on the simple example below with $n = 5$ participants, $m = 5$ possible outcomes, and participant offers

| Participant $i$ | $p_i$ | $q_i$ | $S_i$ |
| --- | --- | --- | --- |
| 1 | 0.50 | 10 | $\{1,2\}$ |
| 2 | 0.60 | 5 | $\{4\}$ |
| 3 | 0.60 | 5 | $\{1,4,5\}$ |
| 4 | 0.60 | 20 | $\{2,5\}$ |
| 5 | 0.20 | 10 | $\{3\}$ |

Compare the optimal worst-case house profit with the worst-case house profit, if all offers were accepted (i.e., $x_i = q_i$). Find the imputed probabilities.

<span style="color:#2471a3;">**[solution]**</span>

> **解答（Solution）.**
>
> (a) 最坏情形庄家利润（worst-case house profit）为
>
> ```math
> p^T x - \max_{j=1,\ldots,m} \; \sum_{i:\, j \in S_i} x_i,
> ```
>
> 它是 $x$ 的分段线性凹函数（piecewise-linear concave function）。为求使最坏情形利润最大的 $x$，我们求解问题
>
> ```math
> \begin{array}{ll}
> \text{maximize} & p^T x - \max_{j=1,\ldots,m} a_j^T x \\
> \text{subject to} & 0 \preceq x \preceq q,
> \end{array}
> ```
>
> 变量为 $x$。$a_j^T$ 是子集矩阵（subset matrix）$A$ 的各行，其中
>
> ```math
> A_{ji} = \begin{cases} 1, & j \in S_i, \\ 0, & \text{otherwise}. \end{cases}
> ```
>
> (b) (a) 中的问题可以表达为
>
> ```math
> \begin{array}{ll}
> \text{maximize} & p^T x - t \\
> \text{subject to} & t\mathbf{1} \succeq Ax \\
> & 0 \preceq x \preceq q,
> \end{array}
> \tag{4}
> ```
>
> 其中 $t$ 是新引入的标量变量。拉格朗日函数为
>
> ```math
> L(x, t, \lambda_1, \lambda_2, \lambda_3) = t - p^T x + \lambda_1^T (Ax - t\mathbf{1}) - \lambda_2^T x + \lambda_3^T (x - q).
> ```

<!-- p.15 -->

> 该式当且仅当 $\mathbf{1}^T \lambda_1 = 1$ 且 $A^T \lambda_1 - \lambda_2 + \lambda_3 = p$ 时有下界。对偶问题可写为
>
> ```math
> \begin{array}{ll}
> \text{maximize} & -q^T \lambda_3 \\
> \text{subject to} & \mathbf{1}^T \lambda_1 = 1 \\
> & A^T \lambda_1 - \lambda_2 + \lambda_3 = p \\
> & \lambda_1 \succeq 0, \quad \lambda_2 \succeq 0, \quad \lambda_3 \succeq 0,
> \end{array}
> \tag{5}
> ```
>
> 变量为 $\lambda_1$、$\lambda_2$ 与 $\lambda_3$。注意 $\lambda_1$ 必须满足 $\mathbf{1}^T \lambda_1 = 1$ 且 $\lambda_1 \succeq 0$，因而是一个概率分布。
>
> 设 $x^\star_{\mathrm{wc}}$、$t^\star$、$\lambda_1^\star$、$\lambda_2^\star$ 与 $\lambda_3^\star$ 是问题 (4) 的原问题与对偶问题最优解，并取 $\pi = \lambda_1^\star$。为最大化期望庄家利润，我们求解问题
>
> ```math
> \begin{array}{ll}
> \text{maximize} & p^T x - \pi^T A x \\
> \text{subject to} & 0 \preceq x \preceq q.
> \end{array}
> \tag{6}
> ```
>
> 设 $b_1^T, \ldots, b_n^T$ 为 $A^T$ 的各行。我们知道：点 $x^\star_{\mathrm{e}}$ 是问题 (6) 的最优解，当且仅当——当 $p_i - b_i^T \pi > 0$ 时 $x^\star_{\mathrm{e},i} = q_i$；当 $p_i - b_i^T \pi < 0$ 时 $x^\star_{\mathrm{e},i} = 0$；当 $p_i - b_i^T \pi = 0$ 时 $0 \le x^\star_{\mathrm{e},i} \le q_i$。
>
> 为说明 $x^\star_{\mathrm{e}} = x^\star_{\mathrm{wc}}$，我们来看问题 (4) 的一个 KKT 条件（KKT conditions）。它可以写为
>
> ```math
> p - A^T \pi = \lambda_3^\star - \lambda_2^\star,
> ```
>
> 其中 $\pi = \lambda_1^\star$。若 $p_i - b_i^T \pi > 0$，则必须有 $\lambda_{3,i}^\star - \lambda_{2,i}^\star > 0$，这意味着 $\lambda_{2,i}^\star = 0$ 且 $\lambda_{3,i}^\star > 0$（由互补松弛（complementary slackness）），于是 $x^\star_{\mathrm{wc},i} = q_i$。类似地，若 $p_i - b_i^T \pi < 0$，则 $\lambda_{3,i}^\star - \lambda_{2,i}^\star < 0$，即 $\lambda_{2,i}^\star > 0$ 且 $\lambda_{3,i}^\star = 0$，于是 $x^\star_{\mathrm{wc},i} = 0$。最后，当 $p_i - b_i^T \pi = 0$ 时，必有 $\lambda_{2,i}^\star = 0$ 且 $\lambda_{3,i}^\star = 0$，于是 $0 \le x^\star_{\mathrm{wc},i} \le q_i$。
>
> 总结：为找到一个使同一个 $x^\star$ 同时最大化最坏情形利润与期望利润的结果概率分布，我们求解对偶 LP (5)，并取 $\pi = \lambda_1^\star$。
>
> (c) 下面的 cvx 代码求解该问题。
>
> ```matlab
> % solution for gambling problem
> A = [1 0 1 0 0;
>      1 0 0 1 0;
>      0 0 0 0 1;
>      0 1 1 0 0;
>      0 0 1 1 0];
> p = [0.5; 0.6; 0.6; 0.6; 0.2];
> q = [10; 5; 5; 20; 10];
> n = 5; m = 5;
> ```

<!-- p.16 -->

> ```matlab
> cvx_begin
>     variables x(n) t
>     dual variable lambda1
>     maximize (p'*x-t)
>     subject to
>         lambda1: A*x <= t
>         x >= 0
>         x <= q
> cvx_end
> % optimal worst case house profit
> pwc = cvx_optval
> % optimal worst case profit if all offer are accepted
> pwc_accept = p'*q-max(A*q)
> % imputed probabilities
> pi = lambda1
> % fair prices
> pfair = A'*pi
> % optimal purchase quantities
> xopt = x
> ```
>
> 结果总结如下表。我们发现最优最坏情形庄家利润为 3.5；而若接受所有报价，最坏情形庄家利润为 $-5$。隐含概率（imputed probabilities）为
>
> ```math
> \pi = (0.1145,\; 0.3855,\; 0.0945,\; 0.1910,\; 0.2145).
> ```
>
> 相应的公平价格（fair prices）与最优合约数量如下表所示。
>
> | Participant $i$ | $p_i$ | $p^{\mathrm{fair}}_i$ | $q_i$ | $x_i$ | $S_i$ |
> | --- | --- | --- | --- | --- | --- |
> | 1 | 0.50 | 0.5000 | 10 | 5 | $\{1,2\}$ |
> | 2 | 0.60 | 0.1910 | 5 | 5 | $\{4\}$ |
> | 3 | 0.60 | 0.5200 | 5 | 5 | $\{1,4,5\}$ |
> | 4 | 0.60 | 0.6000 | 20 | 5 | $\{2,5\}$ |
> | 5 | 0.20 | 0.0945 | 10 | 10 | $\{3\}$ |

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 附加题 5 是「对偶变量即价格」最生动的现身场合：LP 对偶变量 $\lambda_1$ 天然满足非负且和为一（对偶约束 $\mathbf{1}^T \lambda_1 = 1$、$\lambda_1 \succeq 0$），于是摇身一变成为结果上的概率分布 $\pi$；所谓公平价格 $p^{\mathrm{fair}}_i = \sum_{j \in S_i} \pi_j$，不过是「这份合约在分布 $\pi$ 下的期望赔付」。互补松弛（complementary slackness）随后给出教科书式的经济学解释：报价高于公平价的参与者被全额满足（$x_i = q_i$）、低于公平价的被拒单（$x_i = 0$）、恰等于公平价的随意成交。这正是预测市场的理论根基——从 1988 年设立至今的 Iowa Electronic Markets 这类选举预测市场，其成交价反推出的候选人获胜概率常不输民调，数学内核就是本题：从一群逐利交易者的报价中「隐含」（impute）出概率分布。还可以与 4.60 对照着看：那里是给定收益分布 $\pi$ 去找最优组合 $x$，这里是由最优行为 $x$（报价）反推 $\pi$，对偶的两侧互为镜像，也与附加题 3 中对偶变量获得价格解释一脉相承。

### <span style="color:#c0392b;">附加题 6 Boolean LP 的启发式次优解（Heuristic suboptimal solution for Boolean LP）</span>

<span style="color:#2471a3;">**[question]**</span>

**6. Heuristic suboptimal solution for Boolean LP.** This exercise builds on exercises 4.15 and 5.13, which involve the Boolean LP

```math
\begin{array}{ll}
\text{minimize} & c^T x \\
\text{subject to} & Ax \preceq b \\
& x_i \in \{0, 1\}, \quad i = 1, \ldots, n,
\end{array}
```

with optimal value $p^\star$. Let $x_{\mathrm{rlx}}$ be a solution of the LP relaxation

```math
\begin{array}{ll}
\text{minimize} & c^T x \\
\text{subject to} & Ax \preceq b \\
& 0 \preceq x \preceq \mathbf{1},
\end{array}
```

so $L = c^T x_{\mathrm{rlx}}$ is a lower bound on $p^\star$. The relaxed solution $x_{\mathrm{rlx}}$ can also be used to guess a Boolean point $\hat{x}$, by rounding its entries, based on a threshold $t \in [0, 1]$:

```math
\hat{x}_i = \begin{cases} 1, & x_{\mathrm{rlx},i} \ge t, \\ 0, & \text{otherwise}, \end{cases}
```

for $i = 1, \ldots, n$. Evidently $\hat{x}$ is Boolean (i.e., has entries in $\{0, 1\}$). If it is feasible for the Boolean LP, i.e., if $A\hat{x} \preceq b$, then it can be considered a guess at a good, if not optimal, point for the Boolean LP. Its objective value, $U = c^T \hat{x}$, is an upper bound on $p^\star$. If $U$ and $L$ are close, then $\hat{x}$ is nearly optimal; specifically, $\hat{x}$ cannot be more than $(U - L)$-suboptimal for the Boolean LP.

This rounding need not work; indeed, it can happen that for all threshold values, $\hat{x}$ is infeasible. But for some problem instances, it can work well. Of course, there are many variations on this simple scheme for (possibly) constructing a feasible, good point from $x_{\mathrm{rlx}}$.

Finally, we get to the problem. Generate problem data using

```matlab
rand('state',0);
n=100;
m=300;
A=rand(m,n);
b=A*ones(n,1)/2;
c=-rand(n,1);
```

You can think of $x_i$ as a job we either accept or decline, and $-c_i$ as the (positive) revenue we generate if we accept job $i$. We can think of $Ax \preceq b$ as a set of limits on $m$ resources. $A_{ij}$, which is positive, is the amount of resource $i$ consumed if we accept job $j$; $b_i$, which is positive, is the amount of resource $i$ available.

Find a solution of the relaxed LP and examine its entries. Note the associated lower bound $L$. Carry out threshold rounding for (say) 100 values of $t$, uniformly spaced over $[0, 1]$. For each value of $t$, note the objective value $c^T \hat{x}$ and the maximum constraint violation $\max_i (A\hat{x} - b)_i$. Plot the objective value and the maximum violation versus $t$. Be sure to indicate on the plot the values of $t$ for which $\hat{x}$ is feasible, and those for which it is not.

Find a value of $t$ for which $\hat{x}$ is feasible, and gives minimum objective value, and note the associated upper bound $U$.

<!-- p.17 -->

Give the gap $U - L$ between the upper bound on $p^\star$ and the lower bound on $p^\star$. If you define vectors `obj` and `maxviol`, you can find the upper bound as `U=min(obj(find(maxviol<=0)))`.

<span style="color:#2471a3;">**[solution]**</span>

<!-- p.18 -->

> **解答（Solution）.** 以下 Matlab 代码求出该解：
>
> ```matlab
> % generate data for boolean LP relaxation & heuristic
> rand('state',0);
> n=100;
> m=300;
> A=rand(m,n);
> b=A*ones(n,1)/2;
> c=-rand(n,1);
> % solve LP relaxation
> cvx_begin
>     variable x(n)
>     minimize (c'*x)
>     subject to
>         A*x <= b
>         x>=0
>         x<=1
> cvx_end
> xrlx = x;
> L=cvx_optval;
> % sweep over threshold & round
> thres=0:0.01:1;
> maxviol = zeros(length(thres),1);
> obj = zeros(length(thres),1);
> for i=1:length(thres)
>     xhat = (xrlx>=thres(i));
>     maxviol(i) = max(A*xhat-b);
>     obj(i) = c'*xhat;
> end
> % find least upper bound and associated threshold
> i_feas=find(maxviol<=0);
> U=min(obj(i_feas))
> t=min(i_feas);
> min_thresh=thres(t)
> % plot objective and max violation versus threshold
> ```

<!-- p.19 -->

> <span style="color:#7f8c8d;">**图 1（Figure 1）:** 违反量与目标值随阈值变化的曲线（Plots of violation and objective vs threshold）。上图为最大违反量（max violation）随阈值（threshold）的变化（纵轴约 $-30$ 至 $30$），下图为目标值（objective）的变化（纵轴约 $-50$ 至 $0$，并画出下界 $L$ 的水平参考线）；红色线段对应使 $\hat{x}$ 不可行的阈值区间，蓝色线段对应 $\hat{x}$ 可行的阈值区间。</span>
>
> ```matlab
> subplot(2,1,1)
> plot(thres(1:t-1),maxviol(1:t-1),'r',thres(t:end),maxviol(t:end),'b','linewidth',2)
> xlabel('threshold');
> ylabel('max violation');
> subplot(2,1,2)
> hold on; plot(thres,L*ones(size(thres)),'k','linewidth',2);
> plot(thres(1:t-1),obj(1:t-1),'r',thres(t:end),obj(t:end),'b','linewidth',2);
> xlabel('threshold');
> ylabel('objective');
> ```
>
> 由松弛 LP 得到的下界为 $L = -33.1672$。我们求得阈值 $t = 0.6006$ 在可行 $\hat{x}$ 中给出最好（最小）的目标值：$U = -32.4450$。两者之差为 0.7222，因此取 $t = 0.6006$ 的 $\hat{x}$ 次优程度不超过 0.7222。
>
> 在图 1 中，红色线段表示使 $\hat{x}$ 不可行的阈值取值，蓝色线段对应可行的 $\hat{x}$。可以看到，最大违反量随阈值增大而减小。原因在于约束矩阵 $A$ 只有非负元素：阈值为 0 时所有工作都被选中，这是一个不可行解；随着阈值增大，项目被依次移除（不再新增项目），最大违反量因此单调下降。对一般的 Boolean LP，相应的图像未必表现出这种单调性。

