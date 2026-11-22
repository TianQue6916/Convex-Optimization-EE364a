# EE364a HW5 解答（作业 5 解答）

## 题目 1（Problem 1 — 4.15 Relaxation of Boolean LP）

**题干：** In a Boolean linear program, the variable $x$ is constrained to have components equal to zero or one:

```math
\begin{aligned}
\text{minimize}\quad & c^T x \\
\text{subject to}\quad & Ax \preceq b \\
& x_i \in \{0,1\},\quad i=1,\dots,n.
\end{aligned}
```

In general, such problems are very difficult to solve, even though the feasible set is finite (containing at most $2^n$ points).

**中文：** 在 Boolean linear program（布尔线性规划，简称 Boolean LP）中，变量 $x$ 的各分量被约束为只能取 0 或 1（见式 (1)）。一般来说，这类问题极难求解，即使其 feasible set（可行集）是有限的（至多含 $2^n$ 个点）。

---

In a general method called relaxation, the constraint that $x_i$ be zero or one is replaced with the linear inequalities $0 \le x_i \le 1$:

```math
\begin{aligned}
\text{minimize}\quad & c^T x \\
\text{subject to}\quad & Ax \preceq b \\
& 0 \le x_i \le 1,\quad i=1,\dots,n.
\end{aligned}
```

**中文：** 在一种称为 relaxation（松弛）的通用方法中，把「$x_i$ 只能取 0 或 1」这一约束替换为线性不等式 $0\le x_i\le 1$，得到问题 (2)。我们称该问题为 Boolean LP (4.67) 的 LP relaxation（线性规划松弛，式 (4.68)）。LP relaxation 远比原 Boolean LP 容易求解。

**(a)** Show that the optimal value of the LP relaxation (4.68) is a lower bound on the optimal value of the Boolean LP (4.67). What can you say about the Boolean LP if the LP relaxation is infeasible?

**(a) 中文：** 证明 LP relaxation (4.68) 的最优值是 Boolean LP (4.67) 最优值的 lower bound（下界）。如果 LP relaxation 不可行（infeasible），你能对 Boolean LP 说些什么？

**(b)** It sometimes happens that the LP relaxation has a solution with $x_i \in \{0,1\}$. What can you say in this case?

**(b) 中文：** LP relaxation 有时会有满足 $x_i\in\{0,1\}$ 的解。此时你能得出什么结论？

**解答：**

**(a)** 松弛问题的 feasible set 包含 Boolean LP 的 feasible set。原因如下：Boolean LP 的可行集是

```math
\{ x \mid Ax \preceq b,\ x_i\in\{0,1\}\ \forall i \},
```

而 LP relaxation 的可行集是

```math
\{ x \mid Ax \preceq b,\ 0\le x_i\le 1\ \forall i \}.
```

由于 $\{0,1\}\subseteq[0,1]$，前者显然是后者的子集。因此：

- **若 LP relaxation 不可行（infeasible）**，即松弛后的可行集为空，则作为其子集的 Boolean LP 可行集也必为空，故 **Boolean LP 也一定不可行**。
- 由于松弛问题在更大的可行集上求极小值，其最优值一定不超过 Boolean LP 的最优值，即 **LP relaxation 的最优值是 Boolean LP 最优值的下界**：

```math
p^*_{\text{relax}} \le p^*_{\text{Boolean}}.
```

**(b)** 若 LP relaxation 的最优解 $x^*$ 满足 $x_i^*\in\{0,1\}$（即各分量恰好取 0 或 1），则该 $x^*$ 本身也落在 Boolean LP 的可行集中。又因为它是松弛问题的 optimal solution，其目标值 $c^T x^*$ 等于 $p^*_{\text{relax}}$，而由 (a) 知 $p^*_{\text{relax}}\le p^*_{\text{Boolean}}$。但 $x^*$ 又是 Boolean LP 的可行解，故 $p^*_{\text{Boolean}}\le c^T x^* = p^*_{\text{relax}}$。综合两边得到 $p^*_{\text{Boolean}}=c^T x^*$，因此 **该松弛最优解同时也是 Boolean LP 的最优解**。

## 题目 2（Problem 2 — 4.60 Log-optimal investment strategy）

**题干：** We consider a portfolio problem with $n$ assets held over $N$ periods. At the beginning of each period, we re-invest our total wealth, redistributing it over the $n$ assets using a fixed, constant, allocation strategy $x\in\mathbb{R}^n$, where $x\succeq 0$, $\mathbf{1}^T x=1$. In other words, if $W(t-1)$ is our wealth at the beginning of period $t$, then during period $t$ we invest $x_i W(t-1)$ in asset $i$. We denote by $\lambda(t)$ the total return during period $t$, i.e., $\lambda(t)=W(t)/W(t-1)$.

**中文：** 考虑一个 $n$ 种资产、持有 $N$ 个周期的 portfolio（投资组合）问题。每个周期开始时，我们把全部财富再投资，用一个固定不变的 allocation strategy（配置策略）$x\in\mathbb{R}^n$（其中 $x\succeq0$，$\mathbf{1}^T x=1$）把财富在 $n$ 种资产间重新分配。也就是说，若 $W(t-1)$ 是周期 $t$ 开始时的财富，则周期 $t$ 内我们在资产 $i$ 上投资 $x_iW(t-1)$。用 $\lambda(t)$ 表示周期 $t$ 的总收益率，即 $\lambda(t)=W(t)/W(t-1)$。

---

At the end of the $N$ periods our wealth has been multiplied by the factor $\prod_{t=1}^{N}\lambda(t)$. We call

```math
\frac{1}{N}\sum_{t=1}^{N}\log\lambda(t)
```

the growth rate of the investment over the $N$ periods. We are interested in determining an allocation strategy $x$ that maximizes growth of our total wealth for large $N$.

**中文：** 在 $N$ 个周期结束时，我们的财富被乘以因子 $\prod_{t=1}^N\lambda(t)$。我们把 $\frac1N\sum_{t=1}^N\log\lambda(t)$ 称为这 $N$ 个周期内的 growth rate（增长率）。我们关心的是确定一个 allocation strategy $x$，使得在 $N$ 很大时总财富的增长最大化。

---

We use a discrete stochastic model to account for the uncertainty in the returns. We assume that during each period there are $m$ possible scenarios, with probabilities $\pi_j$, $j=1,\dots,m$. In scenario $j$, the return for asset $i$ over one period is given by $p_{ij}$. Therefore, the return $\lambda(t)$ of our portfolio during period $t$ is a random variable, with $m$ possible values $p_1^T x,\dots,p_m^T x$, and distribution $\pi_j=\mathrm{prob}(\lambda(t)=p_j^T x)$, $j=1,\dots,m$. We assume the same scenarios for each period, with (identical) independent distributions. Using the law of large numbers, we have

```math
\lim_{N\to\infty}\frac{1}{N}\log\frac{W(N)}{W(0)}
=\lim_{N\to\infty}\frac{1}{N}\sum_{t=1}^{N}\log\lambda(t)
=\mathbb{E}\log\lambda(t)
=\sum_{j=1}^{m}\pi_j\log(p_j^T x).
```

**中文：** 我们用离散 stochastic model（随机模型）来描述收益的不确定性。假设每个周期有 $m$ 种可能 scenario（情景），概率为 $\pi_j$，$j=1,\dots,m$。在第 $j$ 种情景下，资产 $i$ 一个周期的收益为 $p_{ij}$。于是投资组合在周期 $t$ 的收益 $\lambda(t)$ 是一个 random variable（随机变量），有 $m$ 个可能取值 $p_1^T x,\dots,p_m^T x$，分布为 $\pi_j=\mathrm{prob}(\lambda(t)=p_j^T x)$。假设每个周期都用相同的情景，且各周期独立同分布。由 law of large numbers（大数定律）得到上式。

---

In other words, with investment strategy $x$, the long term growth rate is given by

```math
R_{\mathrm{lt}}=\sum_{j=1}^{m}\pi_j\log(p_j^T x).
```

The investment strategy $x$ that maximizes this quantity is called the log-optimal investment strategy, and can be found by solving the optimization problem

```math
\begin{aligned}
\text{maximize}\quad & \sum_{j=1}^{m}\pi_j\log(p_j^T x) \\
\text{subject to}\quad & x\succeq 0,\quad \mathbf{1}^T x=1,
\end{aligned}
```

with variable $x\in\mathbb{R}^n$. Show that this is a convex optimization problem.

**中文：** 换句话说，在投资策略 $x$ 下，长期增长率为 $R_{\mathrm{lt}}=\sum_{j=1}^m\pi_j\log(p_j^T x)$。最大化该量的投资策略 $x$ 称为 log-optimal investment strategy（对数最优投资策略），可通过求解上面的优化问题得到，变量为 $x\in\mathbb{R}^n$。证明这是一个 convex optimization problem（凸优化问题）。

**解答：** 实际上这题要做的不多。约束 $x\succeq0$ 和 $\mathbf{1}^T x=1$ 显然是凸的（半正定锥 $\mathbb{R}^n_+$ 与仿射超平面 $\{x\mid \mathbf{1}^T x=1\}$ 都是凸集，二者之交仍凸）。因此只需证明目标函数是 concave（凹的，因为它是被极大化的）。分几步完成：

**第一步**：函数 $\log$ 是 concave function（凹函数）。由 concave 的复合规则——一个 concave 且单调非减的函数与一个 affine（仿射）函数的复合仍是 concave——可知 $\log(p_j^T x)$ 关于 $x$ 是 concave 的（在定义域即开半空间 $\{x\mid p_j^T x>0\}$ 上）。

**第二步**：由于权重 $\pi_j\ge0$，凹函数乘以非负系数仍是凹函数，故 $\pi_j\log(p_j^T x)$ 仍是 concave。

**第三步**：凹函数的非负加权和仍是凹函数。因此

```math
\sum_{j=1}^{m}\pi_j\log(p_j^T x)
```

是 concave 的。

综上，目标函数是凹的，约束集是凸的，故该 maximization 问题是一个 convex optimization problem。

## 题目 3（Problem 3 — 5.13 Lagrangian relaxation of Boolean LP）

**题干：** A Boolean linear program is an optimization problem of the form

```math
\begin{aligned}
\text{minimize}\quad & c^T x \\
\text{subject to}\quad & Ax \preceq b \\
& x_i \in \{0,1\},\quad i=1,\dots,n,
\end{aligned}
```

and is, in general, very difficult to solve. In exercise 4.15 we studied the LP relaxation of this problem,

```math
\begin{aligned}
\text{minimize}\quad & c^T x \\
\text{subject to}\quad & Ax \preceq b \\
& 0 \le x_i \le 1,\quad i=1,\dots,n,
\end{aligned}
```

which is far easier to solve, and gives a lower bound on the optimal value of the Boolean LP. In this problem we derive another lower bound for the Boolean LP, and work out the relation between the two lower bounds.

**中文：** Boolean LP 是一般情形下极难求解的优化问题（形如上式）。在习题 4.15 中我们研究了它的 LP relaxation（式 (3)，即 (5.107)），后者易解得多，并给出 Boolean LP 最优值的一个下界。本题我们推导 Boolean LP 的另一个下界，并厘清这两个下界之间的关系。

**(a)** Lagrangian relaxation. The Boolean LP can be reformulated as the problem

```math
\begin{aligned}
\text{minimize}\quad & c^T x \\
\text{subject to}\quad & Ax \preceq b \\
& x_i(1-x_i)=0,\quad i=1,\dots,n,
\end{aligned}
```

which has quadratic equality constraints. Find the Lagrange dual of this problem. The optimal value of the dual problem (which is convex) gives a lower bound on the optimal value of the Boolean LP. This method of finding a lower bound on the optimal value is called Lagrangian relaxation.

**(a) 中文：** Lagrangian relaxation（拉格朗日松弛）。Boolean LP 可重写为具有二次等式约束的问题（$x_i(1-x_i)=0$）。求该问题的 Lagrange dual（拉格朗日对偶）。对偶问题（是凸的）的最优值给出 Boolean LP 最优值的下界。这种求下界的方法称为 Lagrangian relaxation。

**(b)** Show that the lower bound obtained via Lagrangian relaxation, and via the LP relaxation (5.107), are the same. *Hint.* Derive the dual of the LP relaxation (5.107).

**(b) 中文：** 证明由 Lagrangian relaxation 得到的下界与由 LP relaxation (5.107) 得到的下界相同。*提示：* 推导 LP relaxation (5.107) 的对偶。

**解答：**

**(a)** 记不等式约束 $Ax\preceq b$ 对应的对偶变量为 $\mu\succeq0$，等式约束 $x_i(1-x_i)=0$（即 $x_i - x_i^2 = 0$，等价地 $x_i^2=x_i$）对应的对偶变量为 $\nu$。Lagrangian（拉格朗日函数）为

```math
\begin{aligned}
L(x,\mu,\nu)
&= c^T x + \mu^T(Ax-b) - \nu^T x + x^T \mathrm{diag}(\nu) x \\
&= x^T \mathrm{diag}(\nu) x + (c + A^T\mu - \nu)^T x - b^T\mu.
\end{aligned}
```

这里我们把二次等式约束 $x_i(1-x_i)=0$ 写成 $-x_i + x_i^2 = 0$，乘上 $\nu_i$ 后求和，得到 $-\nu^T x + x^T\mathrm{diag}(\nu)x$。

**对 $x$ 求极小得到 dual function（对偶函数）$g(\mu,\nu)=\inf_x L(x,\mu,\nu)$**：这是一个关于 $x$ 的二次型 $\sum_i \nu_i x_i^2 + \sum_i (c_i + a_i^T\mu - \nu_i)x_i$，其中 $a_i$ 是 $A$ 的第 $i$ 列。各项可分离。对每个分量，若 $\nu_i>0$，二次函数 $\nu_i x_i^2 + (c_i+a_i^T\mu-\nu_i)x_i$ 在

```math
x_i = -\frac{c_i + a_i^T\mu - \nu_i}{2\nu_i}
```

处取得最小值，最小值为 $-\dfrac{(c_i+a_i^T\mu-\nu_i)^2}{4\nu_i}$。因此

```math
g(\mu,\nu) =
\begin{cases}
-b^T\mu - \dfrac14\sum_{i=1}^{n}\dfrac{(c_i + a_i^T\mu - \nu_i)^2}{\nu_i}, & \nu\succeq 0, \\[2ex]
-\infty, & \text{otherwise},
\end{cases}
```

其中 $a_i$ 是 $A$ 的第 $i$ 列，并采用约定：$a^2/0=\infty$（当 $a\neq0$）、$a^2/0=0$（当 $a=0$）。

**由此得到的对偶问题为**

```math
\begin{aligned}
\text{maximize}\quad & -b^T\mu - \frac14\sum_{i=1}^{n}\frac{(c_i + a_i^T\mu - \nu_i)^2}{\nu_i} \\
\text{subject to}\quad & \nu\succeq 0,\quad \mu\succeq 0.
\end{aligned}
```

为了简化这个对偶问题，我们对 $\nu$ 做解析优化。注意对每个 $i$，在 $\nu_i\ge0$ 上求

```math
\sup_{\nu_i\ge0}\left(-\frac{(c_i+a_i^T\mu-\nu_i)^2}{\nu_i}\right).
```

记 $d_i = c_i + a_i^T\mu$。分情形讨论：

- 若 $d_i\le0$：取 $\nu_i = -d_i$ 时分子为 $0$，而上确界实际出现在 $\nu_i=-d_i$（此时目标为 $0$，但最优值需仔细分析）。更精确地，令 $\nu_i = -d_i\ge0$ 代入得 $0$；而当 $d_i\le0$ 时该式的最大值为 $0$。但要与答案一致，我们验证答案给出的形式 $4(c_i+a_i^T\mu)$（当 $c_i+a_i^T\mu\le0$）与 $0$（当 $c_i+a_i^T\mu\ge0$），即 $\min\{0,4(c_i+a_i^T\mu)\}$。

  具体地，设 $t=\nu_i>0$，表达式为 $-\dfrac{(d_i-t)^2}{t}=-(t-2d_i+\dfrac{d_i^2}{t})=-t+2d_i-\dfrac{d_i^2}{t}$。对 $t$ 求导并令为零：$-1+\dfrac{d_i^2}{t^2}=0\Rightarrow t=|d_i|$（$t>0$ 处）。当 $d_i\le0$ 时 $t=-d_i$，代入得 $-(-d_i)+2d_i-\dfrac{d_i^2}{-d_i}=d_i+2d_i+d_i=4d_i$。当 $d_i\ge0$ 时 $t=d_i$ 处导数为零是极小点（二阶导为负，实为该凹函数的最大点），代入得 $-d_i+2d_i-d_i=0$。故统一为

```math
\sup_{\nu_i\ge0}\left(-\frac{(c_i+a_i^T\mu-\nu_i)^2}{\nu_i}\right)
=\min\{0,\,4(c_i+a_i^T\mu)\}.
```

这样我们就能从对偶问题中消去 $\nu$，将其简化为

```math
\begin{aligned}
\text{maximize}\quad & -b^T\mu + \sum_{i=1}^{n}\min\{0,\, c_i + a_i^T\mu\} \\
\text{subject to}\quad & \mu\succeq 0.
\end{aligned}
```

**(b)** 按提示推导 LP relaxation (5.107) 的对偶。对约束 $Ax\preceq b$、$-x\preceq0$、$x\preceq 1$ 分别引入对偶变量 $u\succeq0$、$v\succeq0$、$w\succeq0$。Lagrangian 为

```math
\begin{aligned}
L(x,u,v,w)
&= c^T x + u^T(Ax-b) - v^T x + w^T(x-\mathbf{1}) \\
&= (c + A^T u - v + w)^T x - b^T u - \mathbf{1}^T w.
\end{aligned}
```

对 $x$ 求下确界：若 $c+A^T u - v + w \neq 0$，则线性项可让 $L$ 趋于 $-\infty$；故 dual function 为

```math
g(u,v,w) =
\begin{cases}
-b^T u - \mathbf{1}^T w, & A^T u - v + w + c = 0, \\
-\infty, & \text{otherwise}.
\end{cases}
```

**对偶问题为**

```math
\begin{aligned}
\text{maximize}\quad & -b^T u - \mathbf{1}^T w \\
\text{subject to}\quad & A^T u - v + w + c = 0 \\
& u\succeq 0,\ v\succeq 0,\ w\succeq 0.
\end{aligned}
```

现在证明它与 (a) 中简化后的对偶问题等价。由约束 $A^T u - v + w + c=0$，即 $v = A^T u + w + c$，且 $v\succeq0$，等价于要求 $A^T u + w + c \succeq 0$，即 $w_i \ge -(c_i + a_i^T u)$ 对所有 $i$。在最大化问题中，为使 $- \mathbf{1}^T w$ 尽量大（即 $\sum_i w_i$ 尽量小），最优时应取 $w_i = \max\{0,\ -(c_i + a_i^T u)\}$，从而 $\mathbf{1}^T w = \sum_i \max\{0,\ -(c_i+a_i^T u)\} = -\sum_i \min\{0,\ c_i+a_i^T u\}$。代入目标 $-b^T u - \mathbf{1}^T w = -b^T u + \sum_i \min\{0,\ c_i+a_i^T u\}$，约束只剩 $u\succeq0$。这正是 (a) 简化后的对偶问题。

因此两个松弛给出相同的值，**Lagrangian relaxation 与 LP relaxation 的下界相同**。

## 题目 4（Problem 4 — 6.2 $\ell_1$-、$\ell_2$-、$\ell_\infty$-norm approximation by a constant vector）

**题干：** What is the solution of the norm approximation problem with one scalar variable $x\in\mathbb{R}$,

```math
\text{minimize}\quad \|x\mathbf{1}-b\|,
```

for the $\ell_1$-, $\ell_2$-, and $\ell_\infty$-norms?

**中文：** 对于只有一个标量变量 $x\in\mathbb{R}$ 的 norm approximation（范数逼近）问题 $\min_x\|x\mathbf{1}-b\|$，在 $\ell_1$、$\ell_2$、$\ell_\infty$ 范数下其解分别是什么？

**解答：**

**(a) $\ell_2$-范数：** 解为平均值 $\dfrac{\mathbf{1}^T b}{m}$。推导：目标为 $\sum_i (x-b_i)^2$，对 $x$ 求导令零得 $2\sum_i(x-b_i)=0\Rightarrow x=\frac1m\sum_i b_i = \dfrac{\mathbf{1}^T b}{m}$。

**(b) $\ell_1$-范数：** 解为 $b$ 各分量的（一个）median（中位数）。推导：目标 $\sum_i |x-b_i|$ 的次梯度为零的条件是「比 $x$ 小的分量个数」与「比 $x$ 大的分量个数」相等，这正是中位数的定义（当 $m$ 为偶数时，两个中间值之间的任意点都是解）。

**(c) $\ell_\infty$-范数：** 解为 midrange point（中点值）$\dfrac{\max_i b_i + \min_i b_i}{2}$。推导：目标 $\max_i |x-b_i|$ 在 $x$ 取到区间 $[\min b_i,\max b_i]$ 的中点（即把 $b$ 的极值「包」在 $x$ 两侧等距）时最小，最小值为 $\dfrac{\max b_i - \min b_i}{2}$。

〔注：原书 (c) 处写为 `(max b_i − min b_i)/2`，按上下文应为 midrange point $\dfrac{\max_i b_i + \min_i b_i}{2}$，原文疑似漏了加号，待核。〕

## 题目 5（题目 5 — Additional exercise 1: Schur complements）

**题干：** Consider a matrix $X=X^T\in\mathbb{R}^{n\times n}$ partitioned as

```math
X=\begin{bmatrix} A & B \\ B^T & C \end{bmatrix},
```

where $A\in\mathbb{R}^{k\times k}$. If $\det A\neq0$, the matrix $S=C-B^T A^{-1}B$ is called the Schur complement of $A$ in $X$. Schur complements arise in many situations and appear in many important formulas and theorems. For example, we have $\det X=\det A\det S$. (You don't have to prove this.)

**中文：** 考虑对称分块矩阵 $X=\begin{bmatrix}A&B\\B^T&C\end{bmatrix}$，其中 $A\in\mathbb{R}^{k\times k}$。若 $\det A\neq0$，则矩阵 $S=C-B^TA^{-1}B$ 称为 $A$ 在 $X$ 中的 Schur complement（舒尔补）。Schur complement 出现在许多场合和重要公式、定理中。例如 $\det X=\det A\det S$（无需证明）。

**(a)** The Schur complement arises when you minimize a quadratic form over some of the variables. Let $f(u,v)=[u^T\ v^T]X[u^T\ v^T]^T$, where $u\in\mathbb{R}^k$. Let $g(v)$ be the minimum value of $f$ over $u$, i.e., $g(v)=\inf_u f(u,v)$. Of course $g(v)$ can be $-\infty$. Show that if $A\succ0$, we have $g(v)=v^T S v$.

**(a) 中文：** Schur complement 出现在对部分变量求二次型极小值时。设 $f(u,v)=[u^T\ v^T]X[u^T\ v^T]^T$，其中 $u\in\mathbb{R}^k$。令 $g(v)$ 为 $f$ 关于 $u$ 的极小值，即 $g(v)=\inf_u f(u,v)$（$g(v)$ 可取 $-\infty$）。证明若 $A\succ0$，则 $g(v)=v^T S v$。

**(b)** The Schur complement arises in several characterizations of positive definiteness or semidefiniteness of a block matrix. As examples we have the following three theorems: ... Prove one of these theorems. (You can choose which one.)

**(b) 中文：** Schur complement 出现在对分块矩阵正定/半正定的若干刻画中。例如以下三条定理（$X\succ0\iff A\succ0$ 且 $S\succ0$；若 $A\succ0$ 则 $X\succeq0\iff S\succeq0$；$X\succeq0\iff A\succeq0$、$B^T(I-AA^\dagger)=0$ 且 $C-B^TA^\dagger B\succeq0$，其中 $A^\dagger$ 是 $A$ 的 pseudo-inverse（伪逆））。证明其中一条（可自选）。

**(c)** Recognizing Schur complements often helps to represent nonlinear convex constraints as linear matrix inequalities (LMIs). Consider the function

```math
f(x)=(Ax+b)^T(P_0+x_1P_1+\cdots+x_nP_n)^{-1}(Ax+b),
```

where $A\in\mathbb{R}^{m\times n}$, $b\in\mathbb{R}^m$, and $P_i=P_i^T\in\mathbb{R}^{m\times m}$, with domain $\mathrm{dom}\,f=\{x\in\mathbb{R}^n\mid P_0+x_1P_1+\cdots+x_nP_n\succ0\}$. This is the composition of the matrix fractional function and an affine mapping, and so is convex. Give an LMI representation of $\mathrm{epi}\,f$. That is, find a symmetric matrix $F(x,t)$, affine in $(x,t)$, for which $x\in\mathrm{dom}\,f,\ f(x)\le t \iff F(x,t)\succeq0$.

**(c) 中文：** 识别 Schur complement 常有助于把非线性凸约束表示为 linear matrix inequalities（线性矩阵不等式，LMI）。考虑上面的函数 $f$（matrix fractional function（矩阵分式函数）与 affine mapping（仿射映射）的复合，因而是凸的）。给出 $\mathrm{epi}\,f$（上图）的 LMI 表示：即找一个关于 $(x,t)$ 仿射的对称矩阵 $F(x,t)$，使 $x\in\mathrm{dom}\,f,\ f(x)\le t\iff F(x,t)\succeq0$。

**解答：**

**(a)** 展开 $f(u,v)$：

```math
f(u,v)=u^T A u + 2v^T B u + v^T C v.
```

（因为 $[u^T\ v^T]\begin{bmatrix}A&B\\B^T&C\end{bmatrix}\begin{bmatrix}u\\v\end{bmatrix}=u^TAu+u^TBv+v^TB^Tu+v^TCv$，又 $u^TBv$ 是标量等于其转置 $v^TB^Tu$，故合并为 $2v^TBu$。）

若 $A\succ0$，则 $f$ 关于 $u$ 是严格凸的二次函数，可在唯一极小点处最小化。令 $f$ 关于 $u$ 的梯度为零：

```math
\nabla_u f = 2Au + 2Bv = 0 \quad\Rightarrow\quad u^\star(v) = -A^{-1}Bv.
```

（此处用到了 $B^T u$ 关于 $u$ 的梯度是 $B$，故 $2v^TBu$ 关于 $u$ 的梯度是 $2Bv$。）代入得

```math
\begin{aligned}
g(v) &= f(u^\star(v), v) \\
&= v^T B^T A^{-1}B v - 2v^T B A^{-1}B v + v^T C v \\
&= v^T(C - B^T A^{-1}B)v = v^T S v.
\end{aligned}
```

（其中 $u^TAu$ 项代入 $u^\star$ 得 $v^TB^TA^{-1}Bv$，$2v^TBu$ 项代入得 $-2v^TBA^{-1}Bv$。）

**(b)** 证明第二条：**若 $A\succ0$，则 $X\succeq0\iff S\succeq0$**。

*充分性（若 $S\succeq0$）：* 由 (a) 知当 $A\succ0$ 时有 $\inf_u f(u,v)=v^T S v$。于是对任意 $u,v$，

```math
f(u,v)\ge\inf_u f(u,v)=v^T S v \ge 0.
```

由于 $f(u,v)=[u^T\ v^T]X[u^T\ v^T]^T$ 对任意 $(u,v)$ 都 $\ge0$，故 $X\succeq0$。

*必要性（若 $X\succeq0$）：* 由 $X\succeq0$ 知 $f(u,v)\ge0$ 对所有 $(u,v)$ 成立，特别地对所有 $v$ 有 $\inf_u f(u,v)\ge0$。由 (a) 知 $\inf_u f(u,v)=v^T S v$，故 $v^T S v\ge0$ 对所有 $v$ 成立，即 $S\succeq0$。

证毕。

**(c)** $f$ 的 epigraph 是满足 $P_0+x_1P_1+\cdots+x_nP_n\succ0$ 且

```math
(Ax+b)^T(P_0+x_1P_1+\cdots+x_nP_n)^{-1}(Ax+b)\le t
```

的点 $(x,t)$ 的集合。利用 (b) 的第二条结果（$A\succ0$ 时 $X\succeq0\iff S\succeq0$，其中 $S$ 是 Schur complement），取 $A_0=P_0+x_1P_1+\cdots+x_nP_n$（此处 $A_0\succ0$），则第二个不等式等价于分块矩阵

```math
\begin{bmatrix}
t & (Ax+b)^T \\
Ax+b & P_0+x_1P_1+\cdots+x_nP_n
\end{bmatrix}\succeq 0.
```

（其 Schur complement 恰为 $t-(Ax+b)^T(P_0+\cdots+x_nP_n)^{-1}(Ax+b)$。）这是一个关于变量 $x,t$ 的线性矩阵不等式，即凸约束。故取

```math
F(x,t)=\begin{bmatrix}
t & (Ax+b)^T \\
Ax+b & P_0+x_1P_1+\cdots+x_nP_n
\end{bmatrix},
```

它是关于 $(x,t)$ 仿射的对称矩阵，且满足 $x\in\mathrm{dom}\,f,\ f(x)\le t\iff F(x,t)\succeq0$。

## 题目 6（题目 6 — Additional exercise 2: Formulate as SDPs）

**题干：** Formulate the following optimization problems as semidefinite programs. The variable is $x\in\mathbb{R}^n$; $F(x)$ is defined as

```math
F(x)=F_0+x_1F_1+x_2F_2+\cdots+x_nF_n
```

with $F_i\in\mathbb{S}^m$. The domain of $f$ in each subproblem is $\mathrm{dom}\,f=\{x\in\mathbb{R}^n\mid F(x)\succ0\}$.

**(a)** Minimize $f(x)=c^T F(x)^{-1}c$ where $c\in\mathbb{R}^m$.
**(b)** Minimize $f(x)=\max_{i=1,\dots,K}c_i^T F(x)^{-1}c_i$ where $c_i\in\mathbb{R}^m$, $i=1,\dots,K$.
**(c)** Minimize $f(x)=\sup_{\|c\|_2\le1}c^T F(x)^{-1}c$.
**(d)** Minimize $f(x)=\mathbb{E}(c^T F(x)^{-1}c)$ where $c$ is a random vector with mean $\mathbb{E}\,c=\bar c$ and covariance $\mathbb{E}(c-\bar c)(c-\bar c)^T=S$.

**中文：** 把下列优化问题表示为 semidefinite programs（半定规划，SDP）。变量为 $x\in\mathbb{R}^n$；$F(x)=F_0+x_1F_1+\cdots+x_nF_n$，其中 $F_i\in\mathbb{S}^m$（$m$ 阶对称矩阵）。每个子问题中 $f$ 的定义域为 $\mathrm{dom}\,f=\{x\mid F(x)\succ0\}$。

**(a)** 极小化 $f(x)=c^TF(x)^{-1}c$，$c\in\mathbb{R}^m$。
**(b)** 极小化 $f(x)=\max_{i=1,\dots,K}c_i^TF(x)^{-1}c_i$。
**(c)** 极小化 $f(x)=\sup_{\|c\|_2\le1}c^TF(x)^{-1}c$。
**(d)** 极小化 $f(x)=\mathbb{E}(c^TF(x)^{-1}c)$，其中 $c$ 是均值为 $\bar c$、协方差为 $S$ 的随机向量。

**解答：**

**(a)** 引入 epigraph 变量 $t$，$f(x)\le t\iff c^TF(x)^{-1}c\le t$。由 Schur complement（$F(x)\succ0$ 时），这等价于 $\begin{bmatrix}F(x)&c\\c^T&t\end{bmatrix}\succeq0$。故 SDP 为

```math
\begin{aligned}
\text{minimize}\quad & t \\
\text{subject to}\quad & \begin{bmatrix} F(x) & c \\ c^T & t \end{bmatrix}\succeq 0.
\end{aligned}
```

**(b)** 同理，对每个 $i$ 加一条 LMI 约束：

```math
\begin{aligned}
\text{minimize}\quad & t \\
\text{subject to}\quad & \begin{bmatrix} F(x) & c_i \\ c_i^T & t \end{bmatrix}\succeq 0,\quad i=1,\dots,K.
\end{aligned}
```

**(c)** $f(x)=\lambda_{\max}(F(x)^{-1})$（因为 $\sup_{\|c\|_2\le1}c^TF^{-1}c$ 就是 $F^{-1}$ 的最大特征值）。故 $f(x)\le t\iff F(x)^{-1}\preceq tI$。由 Schur complement 得到：

```math
\begin{aligned}
\text{minimize}\quad & t \\
\text{subject to}\quad & \begin{bmatrix} F(x) & I \\ I & tI \end{bmatrix}\succeq 0.
\end{aligned}
```

（$F(x)\succ0$ 时，该矩阵的 Schur complement 为 $tI-I^TF(x)^{-1}I=tI-F(x)^{-1}\succeq0$，即 $F(x)^{-1}\preceq tI$。）

**(d)** 由期望的线性性：

```math
f(x)=\bar c^T F(x)^{-1}\bar c + \mathrm{tr}(F(x)^{-1}S).
```

（因为 $\mathbb{E}[c^TF^{-1}c]=\mathrm{tr}(F^{-1}\mathbb{E}[cc^T])=\mathrm{tr}(F^{-1}(S+\bar c\bar c^T))=\mathrm{tr}(F^{-1}S)+\bar c^TF^{-1}\bar c$。）将 $S$ 分解为 $S=\sum_{k=1}^{m}c_kc_k^T$，问题等价于

```math
\text{minimize}\quad \bar c^T F(x)^{-1}\bar c + \sum_{k=1}^{m}c_k^T F(x)^{-1}c_k,
```

可写为如下 SDP：

```math
\begin{aligned}
\text{minimize}\quad & t_0 + \sum_{k=1}^{m} t_k \\
\text{subject to}\quad & \begin{bmatrix} F(x) & \bar c \\ \bar c^T & t_0 \end{bmatrix}\succeq 0 \\
& \begin{bmatrix} F(x) & c_k \\ c_k^T & t_k \end{bmatrix}\succeq 0,\quad k=1,\dots,m.
\end{aligned}
```

## 题目 7（题目 7 — Additional exercise 3: Optimality conditions and dual for log-optimal investment problem）

**题干：**

**(a)** Show that the optimality conditions for the log-optimal investment problem described in exercise 4.60 can be expressed as: $\mathbf{1}^T x=1$, $x\succeq0$, and for each $i$,

```math
x_i>0 \Rightarrow \sum_{j=1}^{m}\pi_j\frac{p_{ij}}{p_j^T x}=1,\qquad
x_i=0 \Rightarrow \sum_{j=1}^{m}\pi_j\frac{p_{ij}}{p_j^T x}\le1.
```

**中文：** (a) 证明习题 4.60 中对数最优投资问题的最优性条件可表示为：$\mathbf{1}^Tx=1$、$x\succeq0$，且对每个 $i$ 有上式。我们可以这样解释：$p_{ij}/p_j^Tx$ 是一个随机变量，表示「仅投资资产 $i$ 的收益」与「投资组合 $x$ 的收益」之比。最优性条件是说：对每个我们投资的资产，这个比值的期望为 1；对每个我们不投资的资产，这个比值的期望不超过 1。粗略地说，我们的组合不输给任何我们选择投资的资产，也不差于任何我们不投资的资产。

**(b)** In this part we will derive the dual of the log-optimal investment problem. We start by writing the problem as,

```math
\begin{aligned}
\text{minimize}\quad & -\sum_{j=1}^{m}\pi_j\log y_j \\
\text{subject to}\quad & y=P^T x,\quad x\succeq0,\quad \mathbf{1}^T x=1.
\end{aligned}
```

Here, $P$ has columns $p_1,\dots,p_m$, and we have introduced the new variables $y_1,\dots,y_m$, with the implicit constraint $y\succ0$. We will associate dual variables $\nu$, $\lambda$ and $\nu_0$ with the constraints $y=P^T x$, $x\succeq0$, and $\mathbf{1}^T x=1$, respectively. Defining $\tilde\nu_j=\nu_j/\nu_0$ for $j=1,\dots,m$, show that the dual problem can be written as

```math
\begin{aligned}
\text{maximize}\quad & \sum_{j=1}^{m}\pi_j\log(\tilde\nu_j/\pi_j) \\
\text{subject to}\quad & P\tilde\nu\preceq\mathbf{1},
\end{aligned}
```

with variable $\tilde\nu$. The objective here is the (negative) Kullback-Leibler divergence between the given distribution $\pi$ and the dual variable $\tilde\nu$.

**中文：** (b) 推导对数最优投资问题的对偶。先把问题写为上面的极小化形式，其中 $P$ 的列为 $p_1,\dots,p_m$，并引入新变量 $y_1,\dots,y_m$（隐式约束 $y\succ0$）。把约束 $y=P^Tx$、$x\succeq0$、$\mathbf{1}^Tx=1$ 分别与对偶变量 $\nu$、$\lambda$、$\nu_0$ 关联。定义 $\tilde\nu_j=\nu_j/\nu_0$（$j=1,\dots,m$），证明对偶问题可写成上式的极大化形式，变量为 $\tilde\nu$。这里的目标函数是给定分布 $\pi$ 与对偶变量 $\tilde\nu$ 之间的（负）Kullback-Leibler divergence（KL 散度）。

**解答：**

**(a)** 该问题等价于在概率单纯形上极小化 $f(x)=-\sum_{j=1}^m\pi_j\log(p_j^Tx)$。若 $x$ 可行，最优性条件为对一切满足 $z\succeq0,\mathbf{1}^Tz=1$ 的 $z$ 有 $\nabla f(x)^T(z-x)\ge0$（这是凸集上极小化的一阶最优性条件）。这一条件成立当且仅当对每个 $k$：

```math
x_k>0 \Rightarrow \frac{\partial f}{\partial x_k}=\min_{i=1,\dots,n}\frac{\partial f}{\partial x_i}=\nabla f(x)^T x.
```

由 $f(x)=-\sum_{j=1}^m\pi_j\log(p_j^Tx)$，求偏导（用链式法则：$\partial(-\log(p_j^Tx))/\partial x_i=-p_{ij}/(p_j^Tx)$）：

```math
\frac{\partial f}{\partial x_i}
=-\sum_{j=1}^{m}\pi_j\frac{1}{p_j^T x}\,p_{ij},
```

所以

```math
\nabla f(x)^T x
=-\sum_{j=1}^{m}\pi_j\frac{1}{p_j^T x}\,p_j^T x
=-\sum_{j=1}^{m}\pi_j=-1.
```

（最后一步用 $\sum_j\pi_j=1$。）于是最优性条件为：$\mathbf{1}^Tx=1$、$x\succeq0$，且对每个 $i$，

```math
x_i>0 \Rightarrow \frac{\partial f}{\partial x_i}=-1\iff \sum_{j=1}^{m}\pi_j\frac{p_{ij}}{p_j^Tx}=1,
```

```math
x_i=0 \Rightarrow \frac{\partial f}{\partial x_i}\ge -1\iff \sum_{j=1}^{m}\pi_j\frac{p_{ij}}{p_j^Tx}\le1.
```

这正是题目要求的形式。

**(b)** Lagrangian 为

```math
L(x,\nu,\lambda,\nu_0)
=-\sum_{j=1}^{m}\pi_j\log y_j + \nu^T(y-P^Tx) - \lambda^T x + \nu_0(\mathbf{1}^Tx-1).
```

（这里 $\lambda\succeq0$ 对应 $x\succeq0$，$\nu_0$ 对应等式 $\mathbf{1}^Tx=1$。）关于 $x$ 的线性部分系数为 $-\nu^TP^T-\lambda^T+\nu_0\mathbf{1}^T$，若它不为零，则 $L$ 关于 $x$ 无下界。故要求

```math
-P\nu - \lambda + \nu_0\mathbf{1}=0.
```

由于 $L$ 关于 $y$ 是可分离的，可对每个 $y_j$ 分别极小化。对 $-\pi_j\log y_j+\nu_j y_j$ 关于 $y_j$ 求导令零：

```math
-\frac{\pi_j}{y_j}+\nu_j=0 \Rightarrow y_j=\frac{\pi_j}{\nu_j}.
```

（前提 $\nu_j>0$，否则无下界。）代入得 dual function：

```math
g(\nu,\lambda,\nu_0)=1-\nu_0+\sum_{j=1}^{m}\pi_j\log(\nu_j/\pi_j),
```

其中要求 $P\nu\preceq\nu_0\mathbf{1}$（由 $-\nu^TP^T-\lambda^T+\nu_0\mathbf{1}^T=0$ 与 $\lambda\succeq0$ 得到 $P\nu\preceq\nu_0\mathbf{1}$）。于是对偶问题为

```math
\begin{aligned}
\text{maximize}\quad & 1-\nu_0+\sum_{j=1}^{m}\pi_j\log(\nu_j/\pi_j) \\
\text{subject to}\quad & P\nu\preceq\nu_0\mathbf{1},
\end{aligned}
```

变量为 $\nu\in\mathbb{R}^m$、$\nu_0\in\mathbb{R}$，含隐式约束 $\nu\succ0$。

进一步对 $\nu_0$ 做解析优化。由约束不等式知 $\nu_0>0$。定义 $\tilde\nu=\nu/\nu_0$，则问题变为关于 $\tilde\nu,\nu_0$ 的：

```math
\begin{aligned}
\text{maximize}\quad & 1-\nu_0+\sum_{j=1}^{m}\pi_j\log(\tilde\nu_j\nu_0/\pi_j) \\
\text{subject to}\quad & \nu_0 P\tilde\nu\preceq\nu_0\mathbf{1}.
\end{aligned}
```

约束中消去 $\nu_0$（$\nu_0>0$）得 $P\tilde\nu\preceq\mathbf{1}$。目标中 $\sum_j\pi_j\log(\tilde\nu_j\nu_0/\pi_j)=\sum_j\pi_j\log(\tilde\nu_j/\pi_j)+\log\nu_0\cdot\sum_j\pi_j=\sum_j\pi_j\log(\tilde\nu_j/\pi_j)+\log\nu_0$。故问题为

```math
\begin{aligned}
\text{maximize}\quad & 1-\nu_0+\log\nu_0+\sum_{j=1}^{m}\pi_j\log(\tilde\nu_j/\pi_j) \\
\text{subject to}\quad & P\tilde\nu\preceq\mathbf{1}.
\end{aligned}
```

关于 $\nu_0$ 的项 $1-\nu_0+\log\nu_0$ 是 concave 函数，其最大值在 $\nu_0=1$ 处取得（导数 $-\mathbf{1}+1/\nu_0=0$）。故最终对偶问题为

```math
\begin{aligned}
\text{maximize}\quad & \sum_{j=1}^{m}\pi_j\log(\tilde\nu_j/\pi_j) \\
\text{subject to}\quad & P\tilde\nu\preceq\mathbf{1}.
\end{aligned}
```

## 题目 8（题目 8 — Additional exercise 4: Log-optimal investment strategy, numerical）

**题干：** In this problem you will solve a specific instance of the log-optimal investment problem described in exercise 4.60, with $n=5$ assets and $m=10$ possible outcomes in each period. The problem data are defined in `log_opt_invest.m`, with the rows of the matrix $P$ giving the asset return vectors $p_j^T$. The outcomes are equiprobable, i.e., we have $\pi_j=1/m$. ... Find the log-optimal investment strategy $x^\star$, and its associated long term growth rate $R^\star_{\mathrm{lt}}$. Compare this to the long term growth rate obtained with a uniform allocation strategy, i.e., $x=(1/n)\mathbf{1}$, and also with a pure investment in each asset. For the optimal investment strategy, and also the uniform investment strategy, plot 10 sample trajectories of the accumulated wealth ... for $T=0,\dots,200$, with initial wealth $W(0)=1$.

**中文：** 本题求解习题 4.60 中对数最优投资问题的一个具体实例：$n=5$ 种资产、每个周期 $m=10$ 种可能结果。数据定义在 `log_opt_invest.m` 中，矩阵 $P$ 的行给出资产收益向量 $p_j^T$。各结果等概率，即 $\pi_j=1/m$。……求对数最优投资策略 $x^\star$ 及其长期增长率 $R^\star_{\mathrm{lt}}$，并与均匀配置策略 $x=(1/n)\mathbf{1}$、以及纯投资于单一资产的情形比较。对最优策略与均匀策略，各画 10 条累积财富轨迹 $W(T)=W(0)\prod_{t=1}^T\lambda(t)$，$T=0,\dots,200$，初始财富 $W(0)=1$。*提示：* cvx 当前版本不直接处理对数，可用 `geomean()` 求解。

**解答：**

**(a)** 用如下代码求解（用 `geomean` 代替对数，因为最大化 $\sum_j\pi_j\log(p_j^Tx)$ 在等概率 $\pi_j=1/m$ 时等价于最大化 $\prod_j(p_j^Tx)^{1/m}$，即几何平均 `geomean(P*x_opt)`）：

```matlab
P = [3.5000 1.1100 1.1100 1.0400 1.0100;
     0.5000 0.9700 0.9800 1.0500 1.0100;
     0.5000 0.9900 0.9900 0.9900 1.0100;
     0.5000 1.0500 1.0600 0.9900 1.0100;
     0.5000 1.1600 0.9900 1.0700 1.0100;
     0.5000 0.9900 0.9900 1.0600 1.0100;
     0.5000 0.9200 1.0800 0.9900 1.0100;
     0.5000 1.1300 1.1000 0.9900 1.0100;
     0.5000 0.9300 0.9500 1.0400 1.0100;
     3.5000 0.9900 0.9700 0.9800 1.0100];
[m,n] = size(P);
cvx_begin
    variable x_opt(n)
    maximize(geomean(P*x_opt))
    sum(x_opt) == 1
    x_opt >= 0
cvx_end
x_opt
x_unif = ones(n,1)/n;
R_opt = sum(log(P*x_opt))/m
R_unif = sum(log(P*x_unif))/m
```

求得对数最优投资策略为

```math
x^\star = (0.0580,\ 0.4000,\ 0.2923,\ 0.2497,\ 0.0000).
```

该策略实现的长期增长率为 $R^\star_{\mathrm{lt}}=0.0231$。相比之下，均匀配置策略的增长率为 $R_{\mathrm{unif}}=0.0114$。

显然资产 1 是高风险资产：投资于它的金额以 20% 的概率增长 3.50 倍，以 80% 的概率减半。资产 5 是每周期固定收益 1% 的确定性资产。资产 2、3、4 是低风险资产。本例中对数最优策略是：在高风险资产上只投很少的财富，在确定收益资产上不投，而把大部分财富投在资产 2 上。

**(b)** 生成随机事件序列和轨迹图的代码如下：

```matlab
rand('state',0);
N = 10; T = 200;
w_opt = []; w_unif = [];
for i = 1:N
    events = ceil(rand(1,T)*m);
    P_event = P(events,:);
    w_opt  = [w_opt  [1; cumprod(P_event*x_opt)]];
    w_unif = [w_unif [1; cumprod(P_event*x_unif)]];
end
figure
semilogy(w_opt,'g')
hold on
semilogy(w_unif,'r--')
grid
axis tight
xlabel('time')
ylabel('wealth')
```

[[FIGURE: 页码 13, 横轴为时间 t（0–200）、纵轴为财富 W（对数坐标，范围约 10^{-1} 到 10^3）。绿色实线为对数最优策略的 10 条财富轨迹（持续上升），红色虚线为均匀配置策略的 10 条轨迹（随机波动，少数大幅增长、多数表现差）。]]

对数最优投资策略稳定地增加财富；而均匀配置策略产生的轨迹相当随机，少数财富大幅增长，多数表现不佳。这是因为均匀策略把 20% 的财富投在了高风险资产上。

## 题目 9（题目 9 — Additional exercise 5: Maximizing house profit in a gamble and imputed probabilities）

**题干：** A set of $n$ participants bet on which one of $m$ outcomes, labeled $1,\dots,m$, will occur. Participant $i$ offers to purchase up to $q_i>0$ gambling contracts, at price $p_i>0$, that the true outcome will be in the set $S_i\subset\{1,\dots,m\}$. The house then sells her $x_i$ contracts, with $0\le x_i\le q_i$. If the true outcome $j$ is in $S_i$, then participant $i$ receives $\$1$ per contract, i.e., $x_i$. Otherwise, she loses, and receives nothing. The house collects a total of $x_1p_1+\cdots+x_np_n$, and pays out an amount that depends on the outcome $j$, $\sum_{i:j\in S_i}x_i$. The difference is the house profit.

**中文：** 有 $n$ 个参与者对 $m$ 个可能结果（编号 $1,\dots,m$）中哪一个会发生进行下注。参与者 $i$ 提出以价格 $p_i>0$ 购买至多 $q_i>0$ 份 gambling contract（赌博合约），押注真实结果落在集合 $S_i\subset\{1,\dots,m\}$ 中。庄家（house）随后卖给她 $x_i$ 份合约，$0\le x_i\le q_i$。若真实结果 $j\in S_i$，则参与者 $i$ 每份合约获得 $\$1$（即 $x_i$）；否则她输掉、一无所获。庄家总共收取 $x_1p_1+\cdots+x_np_n$，并支付随结果 $j$ 而定的赔付额 $\sum_{i:j\in S_i}x_i$。二者之差即为庄家利润。

**(a)** Optimal house strategy. How should the house decide on $x$ so that its worst-case profit (over the possible outcomes) is maximized? (The house determines $x$ after examining all the participant offers.)

**(a) 中文：** 最优庄家策略。庄家应如何决定 $x$，使其 worst-case profit（最坏情形利润，即在所有可能结果上的最小值）最大化？（庄家在查看所有参与者报价后决定 $x$。）

**(b)** Imputed probabilities. Suppose $x^\star$ maximizes the worst-case house profit. Show that there exists a probability distribution $\pi$ on the possible outcomes (i.e., $\pi\in\mathbb{R}^m_+$, $\mathbf{1}^T\pi=1$) for which $x^\star$ also maximizes the expected house profit. Explain how to find $\pi$. *Hint.* Formulate the problem in part (a) as an LP; you can construct $\pi$ from optimal dual variables for this LP.

**(b) 中文：** Imputed probabilities（归算概率）。设 $x^\star$ 最大化最坏情形利润。证明存在一个结果上的概率分布 $\pi$（$\pi\in\mathbb{R}^m_+$，$\mathbf{1}^T\pi=1$），使得 $x^\star$ 同时也最大化期望利润。说明如何求 $\pi$。*提示：* 把 (a) 中的问题写成 LP；可从该 LP 的最优对偶变量构造 $\pi$。

**(c)** Numerical example. Carry out your method on the simple example below with $n=5$ participants, $m=5$ possible outcomes, and participant offers ... Compare the optimal worst-case house profit with the worst-case house profit if all offers were accepted (i.e., $x_i=q_i$). Find the imputed probabilities.

**(c) 中文：** 数值例子。对下面 $n=5$、$m=5$ 的简单例子执行你的方法。比较最优最坏情形利润与「接受所有报价」（即 $x_i=q_i$）时的最坏情形利润。求归算概率。

**解答：**

**(a)** 最坏情形庄家利润为

```math
p^T x - \max_{j=1,\dots,m}\sum_{i:j\in S_i}x_i,
```

它是 $x$ 的 piecewise-linear concave（分段线性凹）函数。为最大化最坏情形利润，求解如下问题：

```math
\begin{aligned}
\text{maximize}\quad & p^T x - \max_{j=1,\dots,m} a_j^T x \\
\text{subject to}\quad & 0\preceq x\preceq q,
\end{aligned}
```

其中 $a_j^T$ 是子集矩阵 $A$ 的行，$A_{ji}=1$ 若 $j\in S_i$，否则 $A_{ji}=0$。

**(b)** (a) 中的问题可写成

```math
\begin{aligned}
\text{maximize}\quad & p^T x - t \\
\text{subject to}\quad & t\mathbf{1}\succeq Ax \\
& 0\preceq x\preceq q,
\end{aligned}
```

其中 $t$ 是新标量变量。Lagrangian 为

```math
L(x,t,\lambda_1,\lambda_2,\lambda_3)=t-p^Tx+\lambda_1^T(Ax-t\mathbf{1})-\lambda_2^Tx+\lambda_3^T(x-q).
```

该式有下界当且仅当 $\mathbf{1}^T\lambda_1=1$ 且 $A^T\lambda_1-\lambda_2+\lambda_3=p$。对偶问题可写为

```math
\begin{aligned}
\text{maximize}\quad & -q^T\lambda_3 \\
\text{subject to}\quad & \mathbf{1}^T\lambda_1=1 \\
& A^T\lambda_1-\lambda_2+\lambda_3=p \\
& \lambda_1\succeq0,\ \lambda_2\succeq0,\ \lambda_3\succeq0,
\end{aligned}
```

变量为 $\lambda_1,\lambda_2,\lambda_3$。注意 $\lambda_1$ 满足 $\mathbf{1}^T\lambda_1=1$ 且 $\lambda_1\succeq0$，因此它是一个概率分布。

设 $x^\star_{\mathrm{wc}},t^\star,\lambda_1^\star,\lambda_2^\star,\lambda_3^\star$ 是问题 (4) 的原始-对偶最优解，令 $\pi=\lambda_1^\star$。为最大化期望利润，求解

```math
\begin{aligned}
\text{maximize}\quad & p^T x-\pi^T A x \\
\text{subject to}\quad & 0\preceq x\preceq q.
\end{aligned}
```

设 $b_1^T,\dots,b_n^T$ 为 $A^T$ 的行（即 $A$ 的列）。点 $x^\star_e$ 是问题 (6) 的最优解当且仅当：$p_i-b_i^T\pi>0$ 时 $x^\star_{ei}=q_i$；$p_i-b_i^T\pi<0$ 时 $x^\star_{ei}=0$；$p_i-b_i^T\pi=0$ 时 $0\le x^\star_{ei}\le q_i$。

为说明 $x^\star_e=x^\star_{\mathrm{wc}}$，考察问题 (4) 的一条 KKT 条件（complementary slackness，互补松弛）：

```math
p-A^T\pi=\lambda_3^\star-\lambda_2^\star,
```

其中 $\pi=\lambda_1^\star$。若 $p_i-b_i^T\pi>0$，则必有 $\lambda_{3i}^\star-\lambda_{2i}^\star>0$，由互补松弛知 $\lambda_{2i}^\star=0$ 且 $\lambda_{3i}^\star>0$，从而 $x^\star_{\mathrm{wc}\,i}=q_i$。类似地，若 $p_i-b_i^T\pi<0$，则 $\lambda_{3i}^\star-\lambda_{2i}^\star<0$，知 $\lambda_{2i}^\star>0$、$\lambda_{3i}^\star=0$，从而 $x^\star_{\mathrm{wc}\,i}=0$。最后，若 $p_i-b_i^T\pi=0$，则 $\lambda_{2i}^\star=0$、$\lambda_{3i}^\star=0$，从而 $0\le x^\star_{\mathrm{wc}\,i}\le q_i$。

总之，为求使同一个 $x^\star$ 同时最大化最坏情形利润与期望利润的概率分布，解对偶 LP (5)，令 $\pi=\lambda_1^\star$。

**(c)** 用如下 cvx 代码求解：

```matlab
A = [1 0 1 0 0;
     1 0 0 1 0;
     0 0 0 0 1;
     0 1 1 0 0;
     0 0 1 1 0];
p = [0.5; 0.6; 0.6; 0.6; 0.2];
q = [10; 5; 5; 20; 10];
n = 5; m = 5;
cvx_begin
    variables x(n) t
    dual variable lambda1
    maximize (p'*x - t)
    subject to
        lambda1 : A*x <= t
        x >= 0
        x <= q
cvx_end
pwc = cvx_optval
pwc_accept = p'*q - max(A*q)
pi = lambda1
pfair = A'*pi
xopt = x
```

结果汇总如下表。最优最坏情形利润为 $3.5$；若接受所有报价，最坏情形利润为 $-5$。归算概率为

```math
\pi=(0.1145,\ 0.3855,\ 0.0945,\ 0.1910,\ 0.2145).
```

相应的公平价格与最优合约数如下：

| 参与者 $i$ | $p_i$ | $p^{\mathrm{fair}}_i$ | $q_i$ | $x_i$ | $S_i$ |
|---|---|---|---|---|---|
| 1 | 0.50 | 0.5000 | 10 | 5 | {1,2} |
| 2 | 0.60 | 0.1910 | 5 | 5 | {4} |
| 3 | 0.60 | 0.5200 | 5 | 5 | {1,4,5} |
| 4 | 0.60 | 0.6000 | 20 | 5 | {2,5} |
| 5 | 0.20 | 0.0945 | 10 | 10 | {3} |

（说明：公平价格 $p^{\mathrm{fair}}_i=\sum_{j\in S_i}\pi_j$。所有 $p_i>p^{\mathrm{fair}}_i$ 的报价被完全接受 $x_i=q_i$；所有 $p_i\lt p^{\mathrm{fair}}_i$ 的报价被拒绝 $x_i=0$。）

## 题目 10（题目 10 — Additional exercise 6: Heuristic suboptimal solution for Boolean LP）

**题干：** This exercise builds on exercises 4.15 and 5.13, which involve the Boolean LP

```math
\begin{aligned}
\text{minimize}\quad & c^T x \\
\text{subject to}\quad & Ax\preceq b \\
& x_i\in\{0,1\},\quad i=1,\dots,n,
\end{aligned}
```

with optimal value $p^\star$. Let $x^{\mathrm{rlx}}$ be a solution of the LP relaxation

```math
\begin{aligned}
\text{minimize}\quad & c^T x \\
\text{subject to}\quad & Ax\preceq b,\quad 0\preceq x\preceq1,
\end{aligned}
```

so $L=c^Tx^{\mathrm{rlx}}$ is a lower bound on $p^\star$. The relaxed solution $x^{\mathrm{rlx}}$ can also be used to guess a Boolean point $\hat x$, by rounding its entries, based on a threshold $t\in[0,1]$:

```math
\hat x_i=\begin{cases}1 & x^{\mathrm{rlx}}_i\ge t \\ 0 & \text{otherwise},\end{cases}\quad i=1,\dots,n.
```

Evidently $\hat x$ is Boolean (i.e., has entries in $\{0,1\}$). If it is feasible for the Boolean LP, i.e., if $A\hat x\preceq b$, then it can be considered a guess at a good, if not optimal, point for the Boolean LP. Its objective value, $U=c^T\hat x$, is an upper bound on $p^\star$. If $U$ and $L$ are close, then $\hat x$ is nearly optimal; specifically, $\hat x$ cannot be more than $(U-L)$-suboptimal for the Boolean LP.

**中文：** 本题建立在习题 4.15 和 5.13 之上，涉及最优值为 $p^\star$ 的 Boolean LP。设 $x^{\mathrm{rlx}}$ 是其 LP relaxation 的解，则 $L=c^Tx^{\mathrm{rlx}}$ 是 $p^\star$ 的下界。松弛解 $x^{\mathrm{rlx}}$ 也可用于通过按阈值 $t\in[0,1]$ 对其分量四舍五入（rounding）来猜测一个 Boolean 点 $\hat x$（$\hat x_i=1$ 若 $x^{\mathrm{rlx}}_i\ge t$，否则 $0$）。显然 $\hat x$ 是 Boolean 的（分量取 $\{0,1\}$）。若它对 Boolean LP 可行（$A\hat x\preceq b$），则可视为对 Boolean LP 一个好（即便非最优）点的猜测。其目标值 $U=c^T\hat x$ 是 $p^\star$ 的上界。若 $U$ 与 $L$ 接近，则 $\hat x$ 近似最优；具体地，$\hat x$ 至多是 $(U-L)$-次优的。

**中文（任务部分）：** 生成问题数据：

```matlab
rand('state',0);
n=100; m=300;
A=rand(m,n);
b=A*ones(n,1)/2;
c=-rand(n,1);
```

可把 $x_i$ 理解为「接受或拒绝一项工作」，$-c_i$ 是接受工作 $i$ 产生的（正的）收益；$Ax\preceq b$ 是 $m$ 种资源的限制，$A_{ij}$（正）是接受工作 $j$ 消耗资源 $i$ 的量，$b_i$（正）是资源 $i$ 的可用量。求松弛 LP 的解并检查其分量，记下界 $L$。对（例如）100 个在 $[0,1]$ 上均匀分布的阈值 $t$ 做阈值四舍五入；对每个 $t$ 记目标值 $c^T\hat x$ 与最大约束违反 $\max_i(A\hat x-b)_i$。画出目标值与最大违反随 $t$ 的变化，并标明哪些 $t$ 使 $\hat x$ 可行、哪些不可行。找一个使 $\hat x$ 可行且目标值最小的 $t$，记对应的上界 $U$，给出上界与下界之差 $U-L$。

**解答：** 用如下 Matlab 代码求解：

```matlab
rand('state',0);
n=100; m=300;
A=rand(m,n);
b=A*ones(n,1)/2;
c=-rand(n,1);
cvx_begin
    variable x(n)
    minimize (c'*x)
    subject to
        A*x <= b
        x >= 0
        x <= 1
cvx_end
xrlx = x;
L = cvx_optval;

thres = 0:0.01:1;
maxviol = zeros(length(thres),1);
obj = zeros(length(thres),1);
for i=1:length(thres)
    xhat = (xrlx>=thres(i));
    maxviol(i) = max(A*xhat-b);
    obj(i) = c'*xhat;
end

i_feas = find(maxviol<=0);
U = min(obj(i_feas));
t = min(i_feas);
min_thresh = thres(t);

subplot(2,1,1)
plot(thres(1:t-1),maxviol(1:t-1),'r',thres(t:end),maxviol(t:end),'b','linewidth',2)
xlabel('threshold'); ylabel('max violation');
subplot(2,1,2)
hold on; plot(thres,L*ones(size(thres)),'k','linewidth',2);
plot(thres(1:t-1),obj(1:t-1),'r',thres(t:end),obj(t:end),'b','linewidth',2);
xlabel('threshold'); ylabel('objective');
```

由松弛 LP 得到的下界为 $L=-33.1672$。发现阈值 $t=0.6006$ 给出了可行 $\hat x$ 的最优（最小）目标值 $U=-32.4450$，差为 $0.7222$。故 $\hat x$（$t=0.6006$）至多次优 $0.7222$。

[[FIGURE: 页码 19, Figure 1：两张子图，横轴均为 threshold（0–1）。上图纵轴为 max violation（最大违反，范围约 -50 到 0），下图纵轴为 objective（目标值，范围约 -30 到 +30，含一条水平黑线标出下界 L=-33.1672）。红实线表示使 \hat x 不可行的阈值，蓝实线表示可行的阈值。]]

在图 1 中，红线对应使 $\hat x$ 不可行的阈值，蓝线对应可行的 $\hat x$。我们看到最大违反随阈值增大而减小。这是因为约束矩阵 $A$ 只有非负元素：阈值 $0$ 时所有工作都被选中，是不行解；随阈值增大，项目被逐个移除（不再新增项目），最大违反单调下降。对一般 Boolean LP，对应曲线未必单调。
