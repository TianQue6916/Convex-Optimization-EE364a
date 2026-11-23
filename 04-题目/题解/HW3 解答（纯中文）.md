# EE364a Homework 3 solutions（EE364a 作业 3 解答）

> 原文：`HW3 解答.pdf`（19 页），EE364a, Winter 2007-08, Prof. S. Boyd。
> 前 9 页是教材习题解答（3.42, 3.54, 3.57, 4.1, 4.4, 4.8(a–e), 4.17），从第 10 页起是 "Solutions to additional exercises"（附加题解答，三道：最优活动水平数值题、cvx 约束改写、照明问题）。
> 译注：原文第 9 页的矩阵数据因 PDF 排版把 $A$、$c^{\max}$、$p$、$p^{\mathrm{disc}}$、$q$ 挤在一处而严重串行；本译文以原文第 10 页 Matlab 代码中的数值为准复原，并在该处标注。

## 3.42 Approximation width（逼近宽度）

设 $f_0,\dots,f_n : \mathbf{R} \to \mathbf{R}$ 是给定的连续函数。考虑把 $f_0$ 逼近为 $f_1,\dots,f_n$ 的 linear combination（线性组合）的问题。对 $x \in \mathbf{R}^n$，我们说 $f = x_1 f_1 + \cdots + x_n f_n$ 在区间 $[0,T]$ 上以 tolerance（容差）$\epsilon > 0$ 逼近 $f_0$，若 $|f(t) - f_0(t)| \le \epsilon$ 对 $0 \le t \le T$ 成立。现在取定容差 $\epsilon > 0$，并把 approximation width 定义为使 $f$ 在 $[0,T]$ 上逼近 $f_0$ 的最大 $T$：

```math
W(x) = \sup \big\{ T \ \big|\ |x_1 f_1(t) + \cdots + x_n f_n(t) - f_0(t)| \le \epsilon \ \text{for} \ 0 \le t \le T \big\} .
```

证明 $W$ 是 quasiconcave（拟凹）的。

**Solution**。为证 $W$ 是 quasiconcave，只需证集合 $\{x \mid W(x) \ge \alpha\}$ 对所有 $\alpha$ 都是 convex 的。我们有 $W(x) \ge \alpha$ 当且仅当

```math
-\epsilon \le x_1 f_1(t) + \cdots + x_n f_n(t) - f_0(t) \le \epsilon
```

对所有 $t \in [0,\alpha)$ 成立。因此集合 $\{x \mid W(x) \ge \alpha\}$ 是无穷多个 halfspace 的交（每个 $t$ 对应两个），从而是 convex set。

（要点：把「$|g(t)| \le \epsilon$ 对一切 $t$ 成立」逐点拆成两条关于 $x$ 的 linear inequality，再对 $t$ 取交。convex set 的任意交仍 convex，无穷交也成立。）

## 3.54 Log-concavity of Gaussian cumulative distribution function（Gaussian 累积分布函数的对数凹性）

Gaussian random variable（高斯随机变量）的 cumulative distribution function（累积分布函数）

```math
f(x) = \frac{1}{\sqrt{2\pi}} \int_{-\infty}^{x} e^{-t^2/2}\, dt
```

是 log-concave（对数凹）的。这可以由「两个 log-concave 函数的 convolution（卷积）仍 log-concave」这一一般结果推出。本题引导你给出一个简单的自足证明。回忆：$f$ 是 log-concave 当且仅当

```math
f''(x) f(x) \le (f'(x))^2 \quad \text{for all} \ x .
```

**(a)** 验证 $f''(x) f(x) \le (f'(x))^2$ 对 $x \ge 0$ 成立。剩下的难点是对 $x < 0$ 证明该不等式。

**(b)** 验证对任意 $t$ 与 $x$ 有 $t^2/2 \ge x^2/2 + xt$。

**(c)** 用 (b) 证明 $e^{-t^2/2} \le e^{-x^2/2 - xt}$。并由此推出

```math
\int_{-\infty}^{x} e^{-t^2/2}\, dt \le e^{-x^2/2} \int_{-\infty}^{x} e^{-xt}\, dt .
```

**(d)** 用 (c) 验证 $f''(x) f(x) \le (f'(x))^2$ 对 $x \le 0$ 成立。

**Solution**。$f$ 的各阶导数为

```math
f'(x) = e^{-x^2/2}/\sqrt{2\pi}, \qquad f''(x) = -x e^{-x^2/2}/\sqrt{2\pi} .
```

**(a)** 当 $x \ge 0$ 时 $f''(x) \le 0$，而 $f(x) > 0$，故 $f''(x) f(x) \le 0 \le (f'(x))^2$。

**(b)** 由于 $t^2/2$ 是 convex 的，有

```math
t^2/2 \ge x^2/2 + x(t - x) = xt - x^2/2 .
```

这就是一般不等式

```math
g(t) \ge g(x) + g'(x)(t - x),
```

它对任意可微 convex function 成立，此处取 $g(t) = t^2/2$。

（注意：把上面那个式子整理即 $t^2/2 \ge x^2/2 + x(t-x) = xt - x^2/2$，移项得 $t^2/2 \ge x^2/2 + xt - x^2$… 直接核对更简单，见下。）

建立 $t^2/2 \ge x^2/2 + xt$ 的另一个（可能更简单的）办法是注意

```math
t^2/2 + x^2/2 - xt = (1/2)(x - t)^2 \ge 0 .
```

只需把 $x^2/2 - xt$ 移到另一边即得。

**(c)** 取指数并积分。（由 $t^2/2 \ge x^2/2 + xt$ 得 $-t^2/2 \le -x^2/2 - xt$，取指数得 $e^{-t^2/2} \le e^{-x^2/2} e^{-xt}$；两边在 $(-\infty, x]$ 上积分，因子 $e^{-x^2/2}$ 与 $t$ 无关可提出。）

**(d)** 这个基本不等式化归为

```math
-x e^{-x^2/2} \int_{-\infty}^{x} e^{-t^2/2}\, dt \le e^{-x^2},
```

即

```math
\int_{-\infty}^{x} e^{-t^2/2}\, dt \le \frac{e^{-x^2/2}}{-x} .
```

（两边同除以 $(-x) e^{-x^2/2} > 0$ 即得，其中 $x \le 0$ 保证 $x \ne 0$ 时 $-x > 0$。）

这可由 (c) 推出，因为

```math
\int_{-\infty}^{x} e^{-xt}\, dt = \frac{e^{-x^2}}{-x} .
```

（积分：$\int e^{-xt} dt = e^{-xt}/(-x)$，代入上下限，$t \to -\infty$ 时 $e^{-xt} \to 0$ 因 $x \le 0$，$t = x$ 时得 $e^{-x^2}/(-x)$。代回 (c) 得 $\int e^{-t^2/2} dt \le e^{-x^2/2} \cdot e^{-x^2}/(-x) \le e^{-x^2/2}/(-x)$，因 $e^{-x^2} \le 1$，正是所需。）

## 3.57

证明函数 $f(X) = X^{-1}$ 在 $\mathbf{S}^n_{++}$ 上是 matrix convex（矩阵凸）的。

**Solution**。我们需要证明：对任意 $v \in \mathbf{R}^n$，函数

```math
g(X) = v^T X^{-1} v
```

在 $\mathbf{S}^n_{++}$ 上关于 $X$ 是 convex 的。这由教材例 3.4 推出。

## 4.1

考虑优化问题

```math
\begin{aligned}
\text{minimize} \quad & f_0(x_1, x_2) \\
\text{subject to} \quad & 2 x_1 + x_2 \ge 1 \\
& x_1 + 3 x_2 \ge 1 \\
& x_1 \ge 0,\ x_2 \ge 0 .
\end{aligned}
```

画出 feasible set（可行集）的草图。对下列每个 objective function（目标函数），给出 optimal set（最优解集）与 optimal value（最优值）。

**(a)** $f_0(x_1, x_2) = x_1 + x_2$。

**(b)** $f_0(x_1, x_2) = -x_1 - x_2$。

**(c)** $f_0(x_1, x_2) = x_1$。

**(d)** $f_0(x_1, x_2) = \max\{x_1, x_2\}$。

**(e)** $f_0(x_1, x_2) = x_1^2 + 9 x_2^2$。

**Solution**。feasible set 如下图所示。

[Figure 1：第一象限内的可行域——由直线 $2x_1 + x_2 = 1$（过 $(1/2, 0)$ 与 $(0,1)$）与 $x_1 + 3x_2 = 1$（过 $(1,0)$ 与 $(0, 1/3)$）围成的无界区域，顶点为 $(2/5, 1/5)$；坐标轴标出 $(0,1)$、$(2/5,1/5)$、$(1,0)$ 三个关键点]

**(a)** $x^\star = (2/5, 1/5)$。

**(b)** 下界无界（unbounded below）。

**(c)** $X_{\mathrm{opt}} = \{(0, x_2) \mid x_2 \ge 1\}$。

**(d)** $x^\star = (1/3, 1/3)$。

**(e)** $x^\star = (1/2, 1/6)$。它之所以最优，是因为它满足 $2 x_1 + x_2 = 7/6 > 1$、$x_1 + 3 x_2 = 1$，并且

```math
\nabla f_0(x^\star) = (1, 3)
```

垂直于直线 $x_1 + 3 x_2 = 1$。（推导：$\nabla f_0 = (2x_1, 18x_2)$，在 $(1/2, 1/6)$ 处等于 $(1, 3)$，正好是该约束的 normal vector；即 KKT 乘子条件在该 active constraint 上满足，另一约束 strictly inactive 故其乘子为零。）

## 4.4 [P. Parrilo] Symmetries and convex optimization（对称性与凸优化）

设 $\mathcal{G} = \{Q_1,\dots,Q_k\} \subseteq \mathbf{R}^{n \times n}$ 是一个 group（群），即对乘法与取逆封闭。我们说函数 $f : \mathbf{R}^n \to \mathbf{R}$ 是 $\mathcal{G}$-invariant（$\mathcal{G}$-不变）的，或称关于 $\mathcal{G}$ 对称，若 $f(Q_i x) = f(x)$ 对所有 $x$ 与 $i = 1,\dots,k$ 成立。定义

```math
\bar{x} = (1/k) \sum_{i=1}^k Q_i x,
```

它是 $x$ 在其 $\mathcal{G}$-orbit（$\mathcal{G}$-轨道）上的平均。把 $\mathcal{G}$ 的 fixed subspace（不动子空间）定义为

```math
\mathcal{F} = \{x \mid Q_i x = x,\ i = 1,\dots,k\} .
```

**(a)** 证明对任意 $x \in \mathbf{R}^n$ 有 $\bar{x} \in \mathcal{F}$。

**(b)** 证明若 $f : \mathbf{R}^n \to \mathbf{R}$ 是 convex 且 $\mathcal{G}$-invariant，则 $f(\bar{x}) \le f(x)$。

**(c)** 我们称优化问题

```math
\begin{aligned}
\text{minimize} \quad & f_0(x) \\
\text{subject to} \quad & f_i(x) \le 0,\ i = 1,\dots,m
\end{aligned}
```

是 $\mathcal{G}$-invariant 的，若 objective $f_0$ 是 $\mathcal{G}$-invariant 的，且 feasible set 是 $\mathcal{G}$-invariant 的，后者意思是

```math
f_1(x) \le 0, \dots, f_m(x) \le 0 \ \Longrightarrow\ f_1(Q_i x) \le 0, \dots, f_m(Q_i x) \le 0
```

对 $i = 1,\dots,k$ 成立。证明：若该问题是 convex 且 $\mathcal{G}$-invariant 的，并且存在一个 optimal point（最优点），则必存在一个落在 $\mathcal{F}$ 中的 optimal point。换句话讲，我们可以不失一般性地把等式约束 $x \in \mathcal{F}$ 加进问题。

**(d)** 作为例子，设 $f$ 是 convex 且 symmetric（对称）的，即对每个 permutation matrix（置换矩阵）$P$ 有 $f(Px) = f(x)$。证明：若 $f$ 有 minimizer（极小点），则它有形如 $\alpha \mathbf{1}$ 的 minimizer。（意思是：要在 $x \in \mathbf{R}^n$ 上极小化 $f$，也可以等价地在 $t \in \mathbf{R}$ 上极小化 $f(t\mathbf{1})$。）

**Solution**。

**(a)** 先观察：把每个 $Q_i$ 左乘某个固定的 $Q_j$，得到的仍是 $Q_i$ 的一个排列：

```math
Q_j Q_i = Q_{\sigma(i)}, \qquad i = 1,\dots,k,
```

其中 $\sigma$ 是一个 permutation。这是群论的基本结论，但我们也很容易自己证出来。首先注意由封闭性，每个 $Q_j Q_i$ 都等于某个 $Q_s$。现设 $Q_j Q_i = Q_k Q_i = Q_s$。两边右乘 $Q_i^{-1}$，得 $Q_j = Q_k$。因此从下标 $i$ 到下标 $s$ 的映射是一一的，即一个 permutation。

于是

```math
Q_j \bar{x} = (1/k) \sum_{i=1}^k Q_j Q_i x = (1/k) \sum_{i=1}^k Q_{\sigma(i)} x = (1/k) \sum_{i=1}^k Q_i x = \bar{x} .
```

这对每个 $j$ 都成立，因此 $\bar{x} \in \mathcal{F}$。

**(b)** 利用 $f$ 的 convexity 与 invariance，

```math
f(\bar{x}) \le (1/k) \sum_{i=1}^k f(Q_i x) = (1/k) \sum_{i=1}^k f(x) = f(x) .
```

（第一步是 Jensen's inequality（Jensen 不等式）：$f$ 在 $k$ 个点的平均处取值不超过其取值的平均。）

**(c)** 设 $x^\star$ 是一个 optimal solution。$x^\star$ 是可行的，且

```math
\begin{aligned}
f_0(x^\star) &= f_0\Big( (1/k) \sum_{i=1}^k Q_i x^\star \Big) \\
&\le (1/k) \sum_{i=1}^k f_0(Q_i x^\star) \\
&= f_0(x^\star) .
\end{aligned}
```

（第一步用了 $x^\star \in \mathcal{F}$ 的对称平均恒等式，但要按 (a) 的方式理解：把 $x^\star$ 的平均记作 $\bar{x}^\star$，则 $\bar{x}^\star \in \mathcal{F}$ 且由 feasibility 的 $\mathcal{G}$-invariance 仍可行；第二行用 Jensen；第三行用 $f_0$ 的 $\mathcal{G}$-invariance。）因此 $\bar{x}^\star$ 也是 optimal 的，且它落在 $\mathcal{F}$ 中。

**(d)** 设 $x^\star$ 是 $f$ 的一个 minimizer。令 $\bar{x} = (1/n!) \sum_P P x^\star$，其中求和遍历所有 permutation。由于 $\bar{x}$ 在任意 permutation 下不变，可得 $\bar{x} = \alpha \mathbf{1}$ 对某个 $\alpha \in \mathbf{R}$ 成立（permutation-symmetric 的向量各分量必须相等）。由 Jensen's inequality 有

```math
f(\bar{x}) \le (1/n!) \sum_P f(P x^\star) = f(x^\star),
```

这说明 $\bar{x}$ 也是 minimizer。

## 4.8 Some simple LPs（几个简单的 LP）

给出下列各 LP 的显式解。

### (a) Minimizing a linear function over an affine set（在仿射集上极小化线性函数）

```math
\begin{aligned}
\text{minimize} \quad & c^T x \\
\text{subject to} \quad & Ax = b .
\end{aligned}
```

**Solution**。分三种情形。

- **问题不可行**（$b \notin \mathcal{R}(A)$）。optimal value 为 $+\infty$。

- **问题可行，且 $c$ 与 $A$ 的 nullspace 正交**。此时可把 $c$ 分解为

```math
c = A^T \lambda + \hat{c}, \qquad A\hat{c} = 0 .
```

（$\hat{c}$ 是 $c$ 在 $A$ 的 nullspace 中的分量；$A^T \lambda$ 与 nullspace 正交。）若 $\hat{c} = 0$，则在 feasible set 上 objective function 退化为常数：

```math
c^T x = \lambda^T A x + \hat{c}^T x = \lambda^T b .
```

optimal value 为 $\lambda^T b$。所有 feasible solution 都是 optimal 的。

- **问题可行，且 $c$ 不在 $A^T$ 的 range 中**（即 $\hat{c} \neq 0$，原文此处印作 "range of $A$"，语义上应为 range of $A^T$）。此时问题无界（$p^\star = -\infty$）。验证：注意 $x = x_0 - t\hat{c}$ 对所有 $t$ 都可行（因 $A\hat{c} = 0$ 使 $Ax = Ax_0 = b$）；当 $t \to \infty$ 时 objective value 无界下降。

汇总：

```math
p^\star = \begin{cases}
+\infty & b \notin \mathcal{R}(A), \\
\lambda^T b & c = A^T \lambda \ \text{for some} \ \lambda, \\
-\infty & \text{otherwise} .
\end{cases}
```

### (b) Minimizing a linear function over a halfspace（在半空间上极小化线性函数）

```math
\begin{aligned}
\text{minimize} \quad & c^T x \\
\text{subject to} \quad & a^T x \le b,
\end{aligned}
```

其中 $a \neq 0$。

**Solution**。这个问题总是可行的。向量 $c$ 可分解为平行于 $a$ 的分量与正交于 $a$ 的分量：

```math
c = a\lambda + \hat{c}, \qquad a^T \hat{c} = 0 .
```

- **若 $\lambda > 0$**，问题下界无界。取 $x = -ta$ 并令 $t \to \infty$：

```math
c^T x = -t c^T a = -t \lambda a^T a \to -\infty
```

并且

```math
a^T x - b = -t a^T a - b \le 0
```

对大的 $t$ 成立，所以 $t$ 大时 $x$ 可行。直观上，沿 $-a$ 方向走得很远，就能找到 objective value 任意负的可行点。

- **若 $\hat{c} \neq 0$**，问题下界无界。取 $x = (b/a^T a)a - t\hat{c}$ 并令 $t \to \infty$。（因 $a^T\hat{c} = 0$，该 $x$ 对一切 $t$ 满足 $a^T x = b$，可行；而 $c^T x = c^T a \cdot b/(a^Ta) - t\hat{c}^T\hat{c} \to -\infty$。）

- **若 $c = a\lambda$ 且 $\lambda \le 0$**，optimal value 为 $c^T a\, b/(a^Ta) = \lambda b$。

汇总，optimal value 为

```math
p^\star = \begin{cases}
\lambda b & c = a\lambda \ \text{for some} \ \lambda \le 0, \\
-\infty & \text{otherwise} .
\end{cases}
```

### (c) Minimizing a linear function over a rectangle（在矩形上极小化线性函数）

```math
\begin{aligned}
\text{minimize} \quad & c^T x \\
\text{subject to} \quad & l \preceq x \preceq u,
\end{aligned}
```

其中 $l \preceq u$。

**Solution**。objective 与 constraints 都是 separable（可分离）的：objective 是各项 $c_i x_i$ 之和，每项只依赖一个变量；每个 constraint 也只依赖一个变量。因此可以逐个分量独立地极小化。optimal $x_i^\star$ 是在约束 $l_i \le x_i \le u_i$ 下极小化 $c_i x_i$ 的解。若 $c_i > 0$，则 $x_i^\star = l_i$；若 $c_i < 0$，则 $x_i^\star = u_i$；若 $c_i = 0$，则区间 $[l_i, u_i]$ 内任何 $x_i$ 都 optimal。因此 optimal value 为

```math
p^\star = l^T c^+ + u^T c^-,
```

其中 $c_i^+ = \max\{c_i, 0\}$，$c_i^- = \max\{-c_i, 0\}$。

### (d) Minimizing a linear function over the probability simplex（在概率单纯形上极小化线性函数）

```math
\begin{aligned}
\text{minimize} \quad & c^T x \\
\text{subject to} \quad & \mathbf{1}^T x = 1, \quad x \succeq 0 .
\end{aligned}
```

若把等式约束换为不等式 $\mathbf{1}^T x \le 1$，情况如何？

可以把该 LP 解释为一个简单的 portfolio optimization（投资组合优化）问题。向量 $x$ 表示在各项资产上分配总预算的比例，$x_i$ 是投在资产 $i$ 上的比例。每项投资的 return（回报）是固定的，由 $c_i$ 给出，故总回报（我们要最大化的是 $-c^T x$）为 $c^T x$。若把预算约束 $\mathbf{1}^T x = 1$ 换成不等式 $\mathbf{1}^T x \le 1$，我们就有了「不把全部预算投出去」这一选项。

**Solution**。设 $c$ 的分量按递增顺序排列，且

```math
c_1 = c_2 = \cdots = c_k < c_{k+1} \le \cdots \le c_n .
```

对所有可行 $x$ 有

```math
c^T x \ge c_1 (\mathbf{1}^T x) = c_{\min},
```

等号成立当且仅当

```math
x_1 + \cdots + x_k = 1, \quad x_1 \ge 0,\ \dots,\ x_k \ge 0, \quad x_{k+1} = \cdots = x_n = 0 .
```

（理由：$c^T x = \sum_i c_i x_i \ge \sum_i c_1 x_i = c_1 \mathbf{1}^T x = c_1$，因为 $c_i \ge c_1$ 且 $x_i \ge 0$；等号要求所有放在 $c_i > c_1$ 的分量上的权重为零。）于是 optimal value 为 $p^\star = c_1 = c_{\min}$。在投资解释下这个选择很明显：若回报固定且已知，就把全部预算投在回报最高的那项投资上。

若把等式换成不等式，optimal value 等于

```math
p^\star = \min\{0, c_{\min}\} .
```

（若 $c_{\min} \le 0$，$x$ 的取法与上面相同；否则取 $x = 0$——不投资即可，回报为 $0$。）

### (e) Minimizing a linear function over a unit box with a total budget constraint（带总预算约束的单位箱上的线性函数极小化）

```math
\begin{aligned}
\text{minimize} \quad & c^T x \\
\text{subject to} \quad & \mathbf{1}^T x = \alpha, \quad 0 \preceq x \preceq \mathbf{1},
\end{aligned}
```

其中 $\alpha$ 是 $0$ 与 $n$ 之间的整数。若 $\alpha$ 不是整数（但满足 $0 \le \alpha \le n$）会怎样？若把等式换成不等式 $\mathbf{1}^T x \le \alpha$ 又会怎样？

**Solution**。先看 $\alpha$ 为整数的情形。设

```math
c_1 \le \cdots \le c_{i-1} < c_i = \cdots = c_\alpha = \cdots = c_k < c_{k+1} \le \cdots \le c_n .
```

optimal value 为

```math
c_1 + c_2 + \cdots + c_\alpha,
```

即 $c$ 中最小的 $\alpha$ 个元素之和。$x$ 是最优的当且仅当

```math
x_1 = \cdots = x_{i-1} = 1, \quad x_i + \cdots + x_k = \alpha - i + 1, \quad x_{k+1} = \cdots = x_n = 0 .
```

（因为 $c_i = \cdots = c_\alpha$ 并列，权重 $1$ 可以在下标 $i$ 到 $k$ 之间任意分配，只要总和补足 $\alpha$ 即可。）

若 $\alpha$ 不是整数，optimal value 为

```math
p^\star = c_1 + c_2 + \cdots + c_{\lfloor \alpha \rfloor} + c_{1 + \lfloor \alpha \rfloor} (\alpha - \lfloor \alpha \rfloor) .
```

（即：前 $\lfloor\alpha\rfloor$ 个坐标全取 $1$，下一个坐标取小数部分 $\alpha - \lfloor\alpha\rfloor$。）

在不等式约束 $\mathbf{1}^T x \le \alpha$（$\alpha$ 为 $0$ 到 $n$ 之间的整数）的情形，optimal value 是 $c$ 中最小的 $\alpha$ 个**非正**系数之和。

## 4.17 Optimal activity levels（最优活动水平）

考虑选择 $n$ 个非负 activity level（活动水平），记作 $x_1 , \dots, x_n$。这些活动消耗 $m$ 种有限的 resource（资源）。活动 $j$ 消耗资源 $i$ 的量为 $A_{ij} x_j$，其中 $A_{ij}$ 给定。总资源消耗是可加的，因此资源 $i$ 的总消耗为 $c_i = \sum_{j=1} ^n A_{ij} x_j$。（通常 $A_{ij} \ge 0$，即活动 $j$ 消耗资源 $i$；但我们允许 $A_{ij} < 0$，这表示活动 $j$ 实际上作为副产物产出资源 $i$。）

每种资源消耗有限制：必须有 $c_i \le c_i^{\max}$，其中 $c_i^{\max}$ 给定。

每个活动产生 revenue（收益），它是活动水平的分段线性 concave function：

```math
r_j(x_j) = \begin{cases} p_j x_j & 0 \le x_j \le q_j, \\ p_j q_j + p_j^{\mathrm{disc}} (x_j - q_j) & x_j \ge q_j . \end{cases}
```

这里 $p_j > 0$ 是 basic price（基本价格），$q_j > 0$ 是 quantity discount level（数量折扣起点），$p_j^{\mathrm{disc}}$ 是活动 $j$（之产品）的 quantity discount price（数量折扣价）。（有 $0 < p_j^{\mathrm{disc}} < p_j$。）总收益是各活动收益之和，即 $\sum_{j=1} ^n r_j(x_j)$。目标是在遵守资源限制的前提下选择活动水平以最大化总收益。说明如何把该问题表述为一个 LP。

**Solution**。基本问题可以写成

```math
\begin{aligned}
\text{maximize} \quad & \sum_{j=1}^n r_j(x_j) \\
\text{subject to} \quad & x \succeq 0 \\
& Ax \preceq c^{\max} .
\end{aligned}
```

这是一个 convex optimization problem，因为 objective 是 concave 的、约束是一组 linear inequality。为把它转化为等价的 LP，先把收益函数写成

```math
r_j(x_j) = \min\{ p_j x_j,\ p_j q_j + p_j^{\mathrm{disc}}(x_j - q_j) \},
```

这成立是因为 $r_j$ 是 concave 的（concave 的分段线性函数等于其线性片段的逐点最小值）。由此可知 $r_j(x_j) \ge u_j$ 当且仅当

```math
p_j x_j \ge u_j, \qquad p_j q_j + p_j^{\mathrm{disc}}(x_j - q_j) \ge u_j .
```

（方向核对：$u_j \le \min\{f_1, f_2\}$ 等价于 $u_j \le f_1$ 且 $u_j \le f_2$。这样把「$u_j$ 不超过 concave 函数」这个非凸关系转成了两条 linear inequality。）

于是可构造 LP

```math
\begin{aligned}
\text{maximize} \quad & \mathbf{1}^T u \\
\text{subject to} \quad & x \succeq 0 \\
& Ax \preceq c^{\max} \\
& p_j x_j \ge u_j, \quad p_j q_j + p_j^{\mathrm{disc}}(x_j - q_j) \ge u_j, \quad j = 1,\dots,n,
\end{aligned}
```

变量为 $x$ 与 $u$。

为说明该 LP 与原问题等价，固定 $x$。LP 中最后一组约束保证 $u_i \le r_i(x)$，因此对每个可行的 $(x, u)$，LP 的 objective 都不超过总收益。另一方面，总可以取 $u_i = r_i(x)$，此时两个 objective 相等。

# Solutions to additional exercises（附加题解答）

## 附加题 1 · Optimal activity levels（最优活动水平，数值实例）

对教材习题 4.17 描述的最优活动水平问题，求解下面这个具体实例：

```math
A = \begin{bmatrix}
1 & 2 & 0 & 1 \
0 & 0 & 3 & 1 \
0 & 3 & 1 & 1 \
2 & 1 & 2 & 5 \
1 & 0 & 3 & 2
\end{bmatrix}, \quad
c^{\max} = \begin{bmatrix} 100 \ 100 \ 100 \ 100 \ 100 \end{bmatrix}, \quad
p = \begin{bmatrix} 3 \ 2 \ 7 \ 6 \end{bmatrix}, \quad
p^{\mathrm{disc}} = \begin{bmatrix} 2 \ 1 \ 4 \ 2 \end{bmatrix}, \quad
q = \begin{bmatrix} 4 \ 10 \ 5 \ 10 \end{bmatrix}.
```

† 原文第 9 页把这五个量挤在一处排版，PDF 提取后严重串行，上表数值以原文第 10 页 Matlab 代码 `A=[...]`、`cmax=[...]`、`p=[...]`、`pdisc=[...]`、`q=[...]` 为准复原。

可以按你在习题 4.17 解答中得到的 LP 来做，也可以更直接地用 cvx。给出 optimal activity level、每个活动产生的收益、以及最优解产生的总收益。另外给出每个活动水平的 average price per unit（单位平均价格），即某个活动对应的收益与该活动水平之比。（这些数值应当落在每个活动 basic price 与 discounted price 之间。）用一段很简短的话解释（至少评述一下）你得到的解。

**Solution**。下面的 Matlab/CVX 代码求解该问题。（这里我们把问题写成接近原始陈述的形式，让 CVX 去做「改写为 LP」这份苦工！）

```matlab
A=[ 1 2 0 1;
0 0 3 1;
0 3 1 1;
2 1 2 5;
1 0 3 2];
cmax=[100;100;100;100;100];
p=[3;2;7;6];
pdisc=[2;1;4;2];
q=[4; 10 ;5; 10];
cvx_begin
variable x(4)
maximize( sum(min(p.*x,p.*q+pdisc.*(x-q))) )
subject to
x >= 0;
A*x <= cmax
cvx_end
x
r=min(p.*x,p.*q+pdisc.*(x-q))
totr=sum(r)
avgPrice=r./x
```

代码的运行结果为

```
x =
4.0000
22.5000
31.0000
1.5000
r =
12.0000
32.5000
139.0000
9.0000
totr =
192.5000
avgPrice =
3.0000
1.4444
4.4839
6.0000
```

我们注意到第 3 个活动的水平最高，它同时也是 basic price 最高的那个。由于它的 discounted price 也高，其活动水平高于折扣起点 $q_3 = 5$，并且它对总收益的贡献最大。第 4 个活动的 discounted price 明显低于 basic price，因此其活动水平低于折扣起点；此外它占用大量资源，所以其活动水平很低。

（数据核对：$x_4 = 1 . 5 < q_4 = 1 0$，故 $r_4 = 6 \times 1 . 5 = 9$；$x_3 = 3 1 > q_3 = 5$，故 $r_3 = 7 \cdot 5 + 4 \cdot(3 1 - 5 ) = 3 5 + 1 0 4 = 1 3 9$；$x_2 = 2 2 . 5 > q_2 = 1 0$，故 $r_2 = 2 \cdot 1 0 + 1 \cdot 1 2 . 5 = 3 2 . 5$；$x_1 = 4$ 恰好等于 $q_1$，两种公式给出同一个值 $1 2$。总收益 $1 2 + 3 2 . 5 + 1 3 9 + 9 = 1 9 2 . 5$，与输出一致。）

## 附加题 2 · Reformulating constraints in cvx（在 cvx 中改写约束）

下面每个 cvx 代码片段都描述了标量变量 `x`、`y`、`z` 上的一个 convex constraint（凸约束），但违反了 cvx 的规则集（rule set），因此是无效的。简要说明每个片段为何无效。然后把每个片段改写成符合 cvx 规则集的等价形式。改写时可以使用线性等式与不等式约束，以及用 cvx 函数构造的不等式。也可以引入新变量，或使用 LMI（线性矩阵不等式）。若你的改写与原约束的等价性不明显，务必（简要）说明为什么等价。

请通过构造一个含这些约束的小问题并用 cvx 求解来检验你的改写。测试问题不必可行；只要验证 cvx 能无错地处理你的约束就够了。

**Remark（评注）**。这看起来像是「怎么用 cvx 软件」或「cvx 使用技巧」的题目。但它实际上检验的是你是否理解了各种 composition rule（复合规则）、convex analysis 以及约束改写规则。

**(a)** `norm( [ x + 2*y , x - y ] ) == 0`

**(b)** `square( square( x + y ) ) <= x - y`

**(c)** `1/x + 1/y <= 1; x >= 0; y >= 0`

**(d)** `norm([ max( x , 1 ) , max( y , 2 ) ]) <= 3*x + y`

**(e)** `x*y >= 1; x >= 0; y >= 0`

**(f)** `( x + y )^2 / sqrt( y ) <= x - y + 5`

**(g)** `x^3 + y^3 <= 1; x>=0; y>=0`

**(h)** `x+z <= 1+sqrt(x*y-z^2); x>=0; y>=0`

**Solution**。

**(a)** 左边被正确识别为 convex，但等式约束只允许左右两边都是 affine 的。由于一个向量的 norm 为零当且仅当该向量为零，可以把约束写成 `x+2*y==0; x-y==0`，或者干脆写成 `x==0; y==0`。

**(b)** 问题在于 `square()` 只能接受 affine 参数，因为它是 convex 但**不是** increasing（单调增）的。改正办法是用 `square_pos()`：

```matlab
square_pos( square( x + y ) ) <= x - y
```

我们也可以通过引入附加变量来改写该约束：

```matlab
variable t
square( x+y ) <= t
square( t ) <= x - y
```

注意：一般来说，通过引入新变量来分解 objective **不一定**奏效。这里之所以可行，是因为外层的 square 函数在 $\mathbf{R}_+$ 上是 convex 且单调的。

另一种改法是把约束写成

```matlab
( x + y )^4 <= x - y
```

（`(x+y)^4` 是 convex 的——四次幂在整条实轴上 convex，且它是 convex 单调函数 $u^4$ 与 affine 函数 $x+y$ 的复合，符合 composition rule。）

**(c)** `1/x` 不是 convex 的，除非你把定义域限制在 $\mathbf{R} _{+ +}$。可以把它写成 `inv_pos(x)+inv_pos(y)<=1`。`inv_pos` 函数的定义域是 $\mathbf{R} _{+ +}$，因此约束 `x > 0`、`y > 0` 被（隐含地）包含进来了。

**(d)** 问题在于 `norm()` 只能接受 affine 参数，因为它是 convex 但**不是** increasing 的。一种改正方法是引入新变量 `u` 与 `v`：

```matlab
norm( [ u , v ] ) <= 3*x + y
max( x , 1 ) <= u
max( y , 2 ) <= v
```

通过引入新变量来分解之所以在这里奏效，是因为 norm 在 $\mathbf{R}^2_+$ 上 convex 且单调，特别是在 $(1,\infty) \times (2,\infty)$ 上单调。

**(e)** $xy$ 不是 concave 的，所以照字面写行不通。但我们可以把约束表述为 `x>=inv_pos(y)`。（这里也可以把 `x` 与 `y` 对调。）另一个解法是把约束写成 `geomean([x,y])>=1`。我们还可以给出 LMI 表示：

```matlab
[ x 1; 1 y ] == semidefinite(2)
```

**(f)** 当我们试图把一个 convex function 除以一个 concave function 时就出问题了。可以把它写成

```matlab
quad_over_lin(x+y,sqrt(y)) <= x-y+5
```

这样可行，因为 `quad_over_lin` 关于第二个参数是 monotone decreasing（单调递减）的，故在此可以接受一个 concave 函数，而 `sqrt` 正是 concave 的。

**(g)** 函数 $x^3 + y^3$ 在 $x \ge 0$、$y \ge 0$ 时是 convex 的。但 $x^3$ 在 $x < 0$ 时不是 convex 的，所以 cvx 会拒绝这条语句。改写该约束的一种方式是

```matlab
quad_pos_over_lin(square(x),x) + quad_pos_over_lin(square(y),y) <= 1
```

这样可行，因为 `quad_pos_over_lin` 关于第一个参数是 convex 且 increasing 的，故其第一个参数可以接受一个 convex 函数。（而 `quad_over_lin` 关于第一个参数不是 increasing 的，所以行不通。）

另一种更简单的改法是把约束写成

```matlab
pow_pos(x,3) + pow_pos(y,3) <= 1
```

**(h)** 这里的问题是 $xy$ 不是 concave 的，导致 cvx 拒绝该语句。改正时注意

```math
\sqrt{xy - z^2} = \sqrt{y\,(x - z^2/y)},
```

于是可以把约束改写为

```matlab
x+z <= 1+geomean([x-quad_over_lin(z,y),y])
```

这样可行，因为 `geomean` 是 concave 的，且关于每个参数非减（nondecreasing），因此其第一个参数可以接受一个 concave 函数。

我们可以通过在 cvx 中写下面这个 feasibility problem（可行性问题，它显然不可行）来检验这些改写：

```matlab
cvx_begin
variables x y u v z
x == 0;
y == 0;
( x + y )^4 <= x - y;
inv_pos(x) + inv_pos(y) <= 1;
norm( [ u ; v ] ) <= 3*x + y;
max( x , 1 ) <= u;
max( y , 2 ) <= v;
x >= inv_pos(y);
x >= 0;
y >= 0;
quad_over_lin(x + y , sqrt(y)) <= x - y + 5;
pow_pos(x,3) + pow_pos(y,3) <= 1;
x+z <= 1+geomean([x-quad_over_lin(z,y),y])
cvx_end
```

## 附加题 3 · The illumination problem（照明问题）

本题讨论第 1 讲（第 9–11 页）描述的 illumination problem。我们取 $I^{\mathrm{des}} = 1$、$p^{\max} = 1$，于是问题为

```math
\begin{aligned}
\text{minimize} \quad & f_0(p) = \max_{k=1,\dots,n} |\log(a_k^T p)| \
\text{subject to} \quad & 0 \le p_j \le 1, \quad j = 1,\dots,m,
\end{aligned}
```

变量为 $p \in \mathbf{R}^n$。（式 (1)）你要针对一个具体的问题实例，计算若干近似解，并把结果与精确解比较。

正如课上所述，该问题等价于

```math
\begin{aligned}
\text{minimize} \quad & \max_{k=1,\dots,n} h(a_k^T p) \
\text{subject to} \quad & 0 \le p_j \le 1, \quad j = 1,\dots,m,
\end{aligned}
```

（式 (2)）其中 $h(u) = \max\{u, 1/u\}$（$u > 0$）。下图画出函数 $h$，它是非线性的、不可微的、且 convex 的。为看清 (1) 与 (2) 的等价性，注意

```math
\begin{aligned}
f_0(p) &= \max_{k=1,\dots,n} |\log(a_k^T p)| \
&= \max_{k=1,\dots,n} \max\{\log(a_k^T p), \log(1/a_k^T p)\} \
&= \log \max_{k=1,\dots,n} \max\{a_k^T p, 1/a_k^T p\} \
&= \log \max_{k=1,\dots,n} h(a_k^T p),
\end{aligned}
```

而由于对数函数是单调递增的，极小化 $f_0$ 等价于极小化 $\max_{k=1,\dots,n} h(a_k^T p)$。

[Figure 2：函数 $h(u) = \max\{u, 1 / u\}$ 的图像——横轴 $u \in (0 , 4 ]$，纵轴 $h(u)$；曲线在 $u = 1$ 处取最小值 $1$；$u < 1$ 时沿 $1 / u$ 下降，$u > 1$ 时沿 $u$ 线性上升，整体呈「V」形（在 $u=1$ 处有尖点、不可微）]

**问题实例**。具体的问题数据对应下图所示的几何布置，用课上给出的公式

```math
a_{kj} = r_{kj}^{-2} \max\{\cos\theta_{kj}, 0\}
```

计算。共有 10 盏灯（$m = 10$）与 20 块 patch（$n = 20$）。取 $I^{\mathrm{des}} = 1$、$p^{\max} = 1$。问题数据在课程网站文件 `illum_data.m` 中给出。运行该脚本会构造矩阵 $A$（其各行是 $a_k^T$），并画出下图所示的灯/patch 几何布置。

[Figure 3：一维灯与 patch 的几何布置——横轴为位置坐标 $[0,1]$，纵轴用高为 $0.5$ 的竖线标出 10 盏灯的位置（大致均匀分布在 $[0,1]$ 上），并用刻度标出 20 块 patch 的位置；灯与 patch 之间的耦合由距离的平方反比与入射角余弦决定]

**等功率灯（Equal lamp powers）**。取 $p_j = \gamma$（$j = 1,\dots,m$）。在区间 $[0,1]$ 上画出 $f_0(p)$ 随 $\gamma$ 的变化。用图解法确定 $\gamma$ 的最优值，以及相应的 objective value。

可以在 Matlab 中用 `max(abs(log(A*p)))` 计算 objective function $f_0(p)$。

**带饱和的 least-squares（Least-squares with saturation）**。求解 least-squares 问题

```math
\text{minimize} \quad \sum_{k=1}^n (a_k^T p - 1)^2 = \|Ap - \mathbf{1}\|_2^2 .
```

若解中某些 $p_i$ 为负，把它们置为 $0$；若某些值大于 $1$，把它们置为 $1$。给出由此得到的 $f_0(p)$ 值。

least-squares 解可以用 Matlab 的反斜杠运算符计算：`A\b` 返回 least-squares 问题

```math
\text{minimize} \quad \|Ax - b\|_2^2
```

的解。

**正则化 least-squares（Regularized least-squares）**。求解正则化 least-squares 问题

```math
\text{minimize} \quad \sum_{k=1}^n (a_k^T p - 1)^2 + \rho \sum_{j=1}^m (p_j - 0.5)^2
= \|Ap - \mathbf{1}\|_2^2 + \rho \|p - (1/2)\mathbf{1}\|_2^2,
```

其中 $\rho > 0$ 是参数。逐步增大 $\rho$ 直到 $p$ 的所有分量都落在区间 $[0,1]$ 内。给出由此得到的 $f_0(p)$ 值。

可以用 Matlab 的反斜杠运算符求解该正则化 least-squares 问题（把 $\sqrt{\rho}I$ 堆叠到 $A$ 下面、把 $\sqrt{\rho}\cdot 0.5\,\mathbf{1}$ 堆叠到 $\mathbf{1}$ 下面即可）。

**Chebyshev approximation（切比雪夫逼近）**。求解问题

```math
\begin{aligned}
\text{minimize} \quad & \max_{k=1,\dots,n} |a_k^T p - 1| = \|Ap - \mathbf{1}\|_\infty \
\text{subject to} \quad & 0 \le p_j \le 1, \quad j = 1,\dots,m .
\end{aligned}
```

我们可以把这个间题看成是用分段线性函数 $|u - 1| + 1$ 逼近非线性函数 $h(u)$。如下图所示，该逼近在 $u = 1$ 附近效果很好。

[Figure 4：逼近效果对照——横轴 $u \in (0,2]$，纵轴数值；实线为 $h(u) = \max\{u, 1/u\}$，虚线为 $|u-1|+1$；两条曲线在 $u = 1$ 处相切（都取最小值 $1$），在 $u = 0.5$ 与 $u = 2$ 处偏差最大（虚线值 $1.5$，$h$ 值 $2$），说明该分段线性逼近在 $u=1$ 邻域内贴合、远离时偏低]

可以用 cvx 求解该 Chebyshev approximation 问题。（convex）函数 $\|Ap - \mathbf{1}\|_\infty$ 在 cvx 中可写成 `norm(A*p-ones(n,1),inf)`。给出由此得到的 $f_0(p)$ 值。

**精确解（Exact solution）**。最后，用 cvx 精确求解

```math
\begin{aligned}
\text{minimize} \quad & \max_{k=1,\dots,n} \max(a_k^T p,\ 1/a_k^T p) \\
\text{subject to} \quad & 0 \le p_j \le 1, \quad j = 1,\dots,m .
\end{aligned}
```

你可能会觉得 `inv_pos()` 函数有用。给出由此得到的（optimal）$f_0(p)$ 值。

**Solution**。下面的 Matlab 脚本用前面提出的各种 heuristic method 求近似解，同时求出精确解。

```matlab
% illum_sol: finds approximate and exact solutions of
% the illumination problem
clear all;
% load input data
illum_data;
% heuristic method 1: equal lamp powers
% --------------------------------------
nopts=1000;
p = logspace(-3,0,nopts);
f = zeros(size(p));
for k=1:nopts
f(k) = max(abs(log(A*p(k)*ones(m,1))));
end;
[val_equal,imin] = min(f);
p_equal = p(imin)*ones(m,1);
% heuristic method 2: least-squares with saturation
% -------------------------------------------------
p_ls_sat = A\ones(n,1);
p_ls_sat = max(p_ls_sat,0); % rounding negative p_i to 0
p_ls_sat = min(p_ls_sat,1); % rounding p_i > 1 to 1
val_ls_sat = max(abs(log(A*p_ls_sat)));
% heuristic method 3: regularized least-squares
% ---------------------------------------------
rhos = linspace(1e-3,1,nopts);
crit = [];
for j=1:nopts
p = [A; sqrt(rhos(j))*eye(m)]\[ones(n,1); sqrt(rhos(j))*0.5*ones(m,1)];
crit = [ crit norm(p-0.5,inf) ];
end
idx = find(crit <= 0.5);
rho = rhos(idx(1)); % smallest rho s.t. p is in [0,1]
p_ls_reg = [A; sqrt(rho)*eye(m)]\[ones(n,1); sqrt(rho)*0.5*ones(m,1)];
val_ls_reg = max(abs(log(A*p_ls_reg)));
% heuristic method 4: chebyshev approximation
% -------------------------------------------
cvx_begin
variable p_cheb(m)
minimize(norm(A*p_cheb-1, inf))
subject to
p_cheb >= 0
p_cheb <= 1
cvx_end
val_cheb = max(abs(log(A*p_cheb)));
% exact solution:
% ---------------
cvx_begin
variable p_exact(m)
minimize(max([A*p_exact; inv_pos(A*p_exact)]))
subject to
p_exact >= 0
p_exact <= 1
cvx_end
val_exact = max(abs(log(A*p_exact)));
% Results
% -------
[p_equal p_ls_sat p_ls_reg p_cheb p_exact]
[val_equal val_ls_sat val_ls_reg val_cheb val_exact]
```

结果汇总于下表。

| 方法 | method 1（等功率） | method 2（LS+饱和） | method 3（正则化 LS） | method 4（Chebyshev） | exact（精确解） |
|---|---|---|---|---|---|
| $f_0(p)$ | 0.4693 | 0.8628 | 0.4439 | 0.4198 | 0.3575 |
| $p_1$ | 0.3448 | 1 | 0.5004 | 1 | 1 |
| $p_2$ | 0.3448 | 0 | 0.4777 | 0.1165 | 0.2023 |
| $p_3$ | 0.3448 | 1 | 0.0833 | 0 | 0 |
| $p_4$ | 0.3448 | 0 | 0.0002 | 0 | 0 |
| $p_5$ | 0.3448 | 0 | 0.4561 | 1 | 1 |
| $p_6$ | 0.3448 | 1 | 0.4354 | 0 | 0 |
| $p_7$ | 0.3448 | 0 | 0.4597 | 1 | 1 |
| $p_8$ | 0.3448 | 1 | 0.4307 | 0.0249 | 0.1882 |
| $p_9$ | 0.3448 | 0 | 0.4034 | 0 | 0 |
| $p_{10}$ | 0.3448 | 1 | 0.4526 | 1 | 1 |

**读表要点**（译者补充，不属原文）：四种 heuristic 里 Chebyshev approximation（0.4198）最接近精确解（0.3575）；least-squares 直接解（0.8628）最差，因为无约束 LS 把一半的 $p_j$ 推到 $[0,1]$ 之外的边界上，饱和操作把「least-squares 最优」破坏殆尽。精确解的结构很规律：$p$ 只在三档取值——上下界 $0$ 或 $1$，或某个内部值（$0.2023$ / $0.1882$）。这正是「把 $\max_k h(a_k^T p)$ 压平时只有少数几个 patch 成为 active constraint」的体现，也是 KKT 结构给出的稀疏 pattern。
