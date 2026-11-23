# EE364a Homework 5 solutions（EE364a 作业 5 解答）

> 原文：`HW5 解答.pdf`（19 页），EE364a, Winter 2007-08, Prof. S. Boyd。前 4 页是教材习题解答（4.15, 4.60, 5.13, 6.2），第 5 页起是 "Solutions to additional exercises"（附加题 6 道：Schur complement、SDP 表述、log-optimal 问题的最优性条件与 dual、log-optimal 数值实例、赌局与隐含概率、Boolean LP 的启发式次优解）。

## 4.15 Relaxation of Boolean LP（Boolean LP 的松弛）

在 Boolean linear program（布尔线性规划）中，变量 $x$ 的各分量被约束为零或一：

```math
\begin{aligned}
\text{minimize} \quad & c^T x \\
\text{subject to} \quad & Ax \preceq b \\
& x_i \in \{0,1\}, \quad i = 1,\dots,n .
\end{aligned}
```

（式 (1)）一般地，这类问题极难求解，即使 feasible set 是有限的（至多含 $2^n$ 个点）。

在一种称为 relaxation（松弛）的一般方法中，把「$x_i$ 为零或一」这一约束换成线性不等式 $0 \le x_i \le 1$：

```math
\begin{aligned}
\text{minimize} \quad & c^T x \\
\text{subject to} \quad & Ax \preceq b \\
& 0 \le x_i \le 1, \quad i = 1,\dots,n .
\end{aligned}
```

（式 (2)）我们称该问题为 Boolean LP (4.67) 的 LP relaxation。LP relaxation 远比原 Boolean LP 好解。

**(a)** 证明 LP relaxation (4.68) 的 optimal value 是 Boolean LP (4.67) 的 optimal value 的 lower bound。若 LP relaxation 不可行，你能对 Boolean LP 说什么？

**(b)** 有时 LP relaxation 的解恰有 $x_i \in \{0,1\}$。此时你能说什么？

**Solution**。

**(a)** relaxation 的 feasible set 包含 Boolean LP 的 feasible set。由此推出：若 relaxation 不可行，则 Boolean LP 不可行；且 relaxation 的 optimal value 小于或等于 Boolean LP 的 optimal value。

**(b)** relaxation 的 optimal solution 对 Boolean LP 也是 optimal 的。

（理由：该解既在 relaxation 的 feasible set 中（故 $x_i \in \{0,1\}$ 让它在 Boolean LP 的 feasible set 里），objective value 又已达到 Boolean LP 的下界，故它对该 LP 也最优。）

## 4.60 Log-optimal investment strategy（对数最优投资策略）

考虑一个 portfolio problem（投资组合问题）：$n$ 项资产，持有 $N$ 个时期。每个时期开始时，我们重新投资全部财富，用一个固定不变的 allocation strategy（配置策略）$x \in \mathbf{R}^n$ 在各资产间分配，其中 $x \succeq 0$、$\mathbf{1}^T x = 1$。换句话说，若 $W(t-1)$ 是第 $t$ 期开始时的财富，则在第 $t$ 期内我们在资产 $i$ 上投入 $x_i W(t-1)$。记 $\lambda(t)$ 为第 $t$ 期的总回报，即 $\lambda(t) = W(t)/W(t-1)$。$N$ 期结束时，我们的财富乘以因子 $\prod_{t=1}^N \lambda(t)$。我们称

```math
\frac{1}{N} \sum_{t=1}^N \log \lambda(t)
```

为 $N$ 个时期上的 growth rate（增长率）。我们关心的是找出使总财富在 $N$ 很大时增长最快的 allocation strategy $x$。

我们用离散随机模型刻画回报的不确定性。假设每一期内有 $m$ 种可能情形，概率为 $\pi_j$，$j = 1,\dots,m$。在情形 $j$ 下，资产 $i$ 在单期内的回报由 $p_{ij}$ 给出。因此我们组合在第 $t$ 期的回报 $\lambda(t)$ 是 random variable，取 $m$ 个可能值 $p_1^T x,\dots,p_m^T x$，分布为

```math
\pi_j = \mathrm{prob}(\lambda(t) = p_j^T x), \qquad j = 1,\dots,m .
```

假设每一期的情形集合相同、分布（相同且）独立。由 law of large numbers（大数律），我们有

```math
\lim_{N\to\infty} \frac{1}{N}\log \frac{W(N)}{W(0)} = \lim_{N\to\infty} \frac{1}{N}\sum_{t=1}^N \log \lambda(t) = \mathbf{E}\log \lambda(t) = \sum_{j=1}^m \pi_j \log(p_j^T x).
```

换句话说，在投资策略 $x$ 下，长期 growth rate 为

```math
R_{\mathrm{lt}} = \sum_{j=1}^m \pi_j \log(p_j^T x).
```

使该量最大的投资策略 $x$ 称为 log-optimal investment strategy，可通过求解优化问题

```math
\begin{aligned}
\text{maximize} \quad & \sum_{j=1}^m \pi_j \log(p_j^T x) \\
\text{subject to} \quad & x \succeq 0, \quad \mathbf{1}^T x = 1,
\end{aligned}
```

得到，变量为 $x \in \mathbf{R}^n$。

证明这是一个 convex optimization problem。

**Solution**。其实这题没多少可做的。约束 $x \succeq 0$、$\mathbf{1}^T x = 1$ 显然是 convex 的，所以我们只需证明 objective 是 concave 的（因为它要被极大化）。这只要几步：首先注意 $\log$ 是 concave 的，所以 $\log(p_j^T x)$ 关于 $x$ 是 concave 的（在定义域上，即开半空间 $\{x \mid p_j^T x > 0\}$）。由于 $\pi_j \ge 0$，可知 concave function 之和

```math
\sum_{j=1}^m \pi_j \log(p_j^T x)
```

是 concave 的。

（这里用到 composition rule 中最基本的一条：非负加权和的 concavity，以及 affine 映射复合不破坏 concavity。）

## 5.13 Lagrangian relaxation of Boolean LP（Boolean LP 的 Lagrangian 松弛）

Boolean linear program 是形如

```math
\begin{aligned}
\text{minimize} \quad & c^T x \\
\text{subject to} \quad & Ax \preceq b \\
& x_i \in \{0,1\}, \quad i = 1,\dots,n
\end{aligned}
```

的优化问题，一般极难求解。在习题 4.15 中我们研究了它的 LP relaxation

```math
\begin{aligned}
\text{minimize} \quad & c^T x \\
\text{subject to} \quad & Ax \preceq b \\
& 0 \le x_i \le 1, \quad i = 1,\dots,n,
\end{aligned}
```

（式 (3)）它远比原问题好解，并给出 Boolean LP optimal value 的 lower bound。本题中我们推导 Boolean LP 的另一个 lower bound，并弄清这两个 lower bound 之间的关系。

**(a) Lagrangian relaxation（Lagrangian 松弛）**。Boolean LP 可以改写为

```math
\begin{aligned}
\text{minimize} \quad & c^T x \\
\text{subject to} \quad & Ax \preceq b \\
& x_i(1 - x_i) = 0, \quad i = 1,\dots,n,
\end{aligned}
```

它有 quadratic equality constraints（二次等式约束）。求该问题的 Lagrange dual。dual problem（它是 convex 的）的 optimal value 给出 Boolean LP optimal value 的一个 lower bound。这种求 lower bound 的方法称为 Lagrangian relaxation。

**(b)** 证明由 Lagrangian relaxation 得到的 lower bound 与由 LP relaxation (5.107) 得到的 lower bound 相同。提示：推导 LP relaxation (5.107) 的 dual。

**Solution**。

**(a)** Lagrangian 为

```math
\begin{aligned}
L(x,\mu,\nu) &= c^T x + \mu^T(Ax - b) - \nu^T x + x^T \mathbf{diag}(\nu) x \\
&= x^T \mathbf{diag}(\nu) x + (c + A^T\mu - \nu)^T x - b^T\mu .
\end{aligned}
```

对 $x$ 极小化给出 dual function

```math
g(\mu,\nu) = \begin{cases} -b^T\mu - (1/4)\sum_{i=1}^n (c_i + a_i^T\mu - \nu_i)^2/\nu_i & \nu \succeq 0 \\ -\infty & \text{otherwise}, \end{cases}
```

其中 $a_i$ 是 $A$ 的第 $i$ 列，并采用约定：$a^2/0 = \infty$ 若 $a \ne 0$，$a^2/0 = 0$ 若 $a = 0$。

（† 记号提醒：这里把约束 $x_i(1-x_i) = 0$ 写成 $x_i - x_i^2 = 0$ 后乘上乘子 $\nu_i$ **并从** $c^Tx + \mu^T(Ax-b)$ **中减去**，即 $L = c^Tx + \mu^T(Ax-b) - \sum_i \nu_i(x_i - x_i^2)$。展开得 $-\nu^T x + x^T\mathbf{diag}(\nu)x$，与原文一致。若按「加」的惯例写，则所有 $\nu_i$ 变号——由于 $\nu$ 本身无符号约束，两种约定给出同一个 dual problem。读者若发现符号对不上，通常是这个约定差异。$\nu \succeq 0$ 来自「二次项 $+\nu_i x_i^2$ 必须非负否则对 $x_i$ 无界」。）

得到的 dual problem 为

```math
\begin{aligned}
\text{maximize} \quad & -b^T\mu - (1/4)\sum_{i=1}^n (c_i + a_i^T\mu - \nu_i)^2/\nu_i \\
\text{subject to} \quad & \nu \succeq 0, \quad \mu \succeq 0 .
\end{aligned}
```

为化简该 dual，我们对 $\nu$ 解析地优化。注意

```math
\sup_{\nu_i \ge 0} \left( -\frac{(c_i + a_i^T\mu - \nu_i)^2}{\nu_i} \right) = \begin{cases} 4(c_i + a_i^T\mu) & c_i + a_i^T\mu \le 0 \\ 0 & c_i + a_i^T\mu \ge 0 \end{cases} = \min\{0,\ 4(c_i + a_i^T\mu)\}.
```

（推导：令 $s = c_i + a_i^T\mu$，则 $-(s-\nu)^2/\nu = 2s - s^2/\nu - \nu$。对 $\nu > 0$ 求驻点 $s^2/\nu^2 - 1 = 0$ 得 $\nu = |s|$，代入得 $2s - 2|s|$，故 $s \le 0$ 时为 $4s$、$s \ge 0$ 时取 $\nu \to 0$ 给 $0$。）

这使我们能从 dual problem 中消去 $\nu$，化简为

```math
\begin{aligned}
\text{maximize} \quad & -b^T\mu + \sum_{i=1}^n \min\{0,\ c_i + a_i^T\mu\} \\
\text{subject to} \quad & \mu \succeq 0 .
\end{aligned}
```

**(b)** 我们按提示来做。LP relaxation 的 Lagrangian 与 dual function 为

```math
\begin{aligned}
L(x,u,v,w) &= c^T x + u^T(Ax - b) - v^T x + w^T(x - \mathbf{1}) \\
&= (c + A^Tu - v + w)^T x - b^Tu - \mathbf{1}^Tw \\
g(u,v,w) &= \begin{cases} -b^Tu - \mathbf{1}^Tw & A^Tu - v + w + c = 0 \\ -\infty & \text{otherwise} . \end{cases}
\end{aligned}
```

dual problem 为

```math
\begin{aligned}
\text{maximize} \quad & -b^Tu - \mathbf{1}^Tw \\
\text{subject to} \quad & A^Tu - v + w + c = 0 \\
& u \succeq 0, \quad v \succeq 0, \quad w \succeq 0,
\end{aligned}
```

它与上面推导的 Lagrange relaxation 问题是等价的。因此我们得出结论：两个 relaxation 给出相同的值。

（† 译注：原文对「等价」这一步只给了结论。补齐过程如下。由等式约束得 $v - w = A^Tu + c$，记 $s = A^Tu + c$；又 $v \succeq 0$、$w \succeq 0$，故 $s$ 的每个分量可正可负，但 $v_i - w_i = s_i$ 且 $v_i, w_i \ge 0$。为使 objective 中的 $-\mathbf{1}^Tw$ 尽可能大（即 $\mathbf{1}^Tw$ 尽可能小），对每个 $i$：若 $s_i \ge 0$ 取 $v_i = s_i$、$w_i = 0$（贡献 $0$）；若 $s_i < 0$ 取 $v_i = 0$、$w_i = -s_i$（贡献 $s_i$）。于是 $-\mathbf{1}^Tw = \sum_i \min\{0, s_i\}$，代入即得 (a) 中消去 $\nu$ 之后的 dual problem。两边一模一样，故两个下界相同。）

## 6.2 $\ell_1$-, $\ell_2$-, and $\ell_\infty$-norm approximation by a constant vector（用常向量作 $\ell_1$、$\ell_2$、$\ell_\infty$ 范数逼近）

带一个标量变量 $x \in \mathbf{R}$ 的 norm approximation problem

```math
\text{minimize} \quad \|x\mathbf{1} - b\|
```

在 $\ell_1$、$\ell_2$、$\ell_\infty$ 范数下的解分别是什么？

**Solution**。

**(a)** $\ell_2$-norm：$\mathbf{1}^T b/m$（平均值）。

**(b)** $\ell_1$-norm：$b$ 的各分量的（某个）median（中位数）。

**(c)** $\ell_\infty$-norm：midrange point（中点值）$(b_{\max} + b_{\min})/2$。

† 原文此处印作 $(b_{\max} - b_{\min})/2$，那是**最优值**而不是最优解。核对：$\min_x \max_i |x - b_i|$ 的最优点必落在 $[b_{\min}, b_{\max}]$ 的中点（否则往更远的一端移动会减小最大距离），此时最大距离为 $(b_{\max}-b_{\min})/2$。原文公式把 argmin 与 min 混了。

# Solutions to additional exercises（附加题解答）

## 附加题 1 · Schur complements（Schur 补）

考虑对称矩阵 $X = X^T \in \mathbf{R}^{n \times n}$，分块为

```math
X = \begin{bmatrix} A & B \\ B^T & C \end{bmatrix},
```

其中 $A \in \mathbf{R}^{k \times k}$。若 $\det A \ne 0$，矩阵

```math
S = C - B^T A^{-1} B
```

称为 $A$ 在 $X$ 中的 Schur complement（Schur 补）。Schur complement 在许多场合出现，并出现在许多重要公式与定理中。例如有 $\det X = \det A \det S$。（这条不必证明。）

**(a)** Schur complement 出现在「对某些变量极小化一个 quadratic form」的场合。设 $f(u,v) = [u^T\ v^T]X[u^T\ v^T]^T$，其中 $u \in \mathbf{R}^k$。令 $g(v)$ 为 $f$ 关于 $u$ 的最小值，即 $g(v) = \inf_u f(u,v)$。当然 $g(v)$ 可能是 $-\infty$。证明：若 $A \succ 0$，则 $g(v) = v^T S v$。

**(b)** Schur complement 出现在分块矩阵正定/半正定的若干刻画中。例如下面三个定理：

- $X \succ 0$ 当且仅当 $A \succ 0$ 且 $S \succ 0$。
- 若 $A \succ 0$，则 $X \succeq 0$ 当且仅当 $S \succeq 0$。
- $X \succeq 0$ 当且仅当 $A \succeq 0$、$B^T(I - AA^{\dagger}) = 0$ 且 $C - B^T A^{\dagger} B \succeq 0$，其中 $A^{\dagger}$ 是 $A$ 的 pseudo-inverse（伪逆）。（$C - B^TA^{\dagger}B$ 充当 Schur complement 在「$A$ 半正定但奇异」情形下的推广。）

证明其中一个定理。（你可以任选一个。）

**(c)** 识别出 Schur complement 常常有助于把 nonlinear convex constraint 表示成 LMI（linear matrix inequality，线性矩阵不等式）。考虑函数

```math
f(x) = (Ax + b)^T (P_0 + x_1 P_1 + \cdots + x_n P_n)^{-1} (Ax + b),
```

其中 $A \in \mathbf{R}^{m \times n}$、$b \in \mathbf{R}^m$、$P_i = P_i^T \in \mathbf{R}^{m \times m}$，定义域为

```math
\mathbf{dom}\,f = \{x \in \mathbf{R}^n \mid P_0 + x_1P_1 + \cdots + x_nP_n \succ 0\} .
```

它是 matrix fractional function（矩阵分式函数）与 affine mapping 的复合，故为 convex。给出 $\mathbf{epi}\,f$ 的 LMI 表示。也就是说，找出一个在 $(x,t)$ 上 affine 的对称矩阵 $F(x,t)$，使得

```math
x \in \mathbf{dom}\,f,\ f(x) \le t \iff F(x,t) \succeq 0 .
```

**Solution**

**(a)** 若 $A \succ 0$，则 $g(v) = v^T S v$。

我们有

```math
f(u,v) = u^T A u + 2v^T B^T u + v^T C v .
```

若 $A \succ 0$，我们可以把 $f$ 关于 $u$ 的梯度设为零来极小化 $f$。得到

```math
u^\star(v) = -A^{-1}Bv,
```

于是

```math
g(v) = f(u^\star(v), v) = v^T (C - B^TA^{-1}B)v = v^T S v .
```

（代人验算：$u^{\star T}Au^\star = v^TB^TA^{-1}Bv$，$2v^TB^Tu^\star = -2v^TB^TA^{-1}Bv$，相加得 $-v^TB^TA^{-1}Bv$，再加 $v^TCv$ 即 $v^T(C-B^TA^{-1}B)v$。）

**(b)** Positive definite and semidefinite block matrices（正定与半正定分块矩阵）。

- **$X \succ 0$ 当且仅当 $A \succ 0$ 且 $S \succ 0$。**

设 $X \succ 0$。则 $f(u,v) > 0$ 对所有非零 $(u,v)$ 成立。特别地，$f(u,0) = u^TAu > 0$ 对所有非零 $u$ 成立（故 $A \succ 0$），且 $f(-A^{-1}Bv, v) = v^T(C - B^TA^{-1}B)v > 0$（故 $S = C - B^TA^{-1}B \succ 0$）。这就证明了「仅当」部分。

为证「若」部分，我们要证：若 $A \succ 0$ 且 $S \succ 0$，则 $f(u,v) > 0$ 对所有非零 $(u,v)$（即对所有不同时为零的 $u,v$）成立。若 $v \ne 0$，则由 (a) 得

```math
f(u,v) \ge \inf_u f(u,v) = v^T S v > 0 .
```

若 $v = 0$ 且 $u \ne 0$，则 $f(u,0) = u^TAu > 0$。

- **若 $A \succ 0$，则 $X \succeq 0$ 当且仅当 $S \succeq 0$。**

由 (a) 知若 $A \succ 0$，则 $\inf_u f(u,v) = v^TSv$。若 $S \succeq 0$，则

```math
f(u,v) \ge \inf_u f(u,v) = v^T S v \ge 0
```

对所有 $u,v$ 成立，故 $X \succeq 0$。这就证明了「若」部分。

为证「仅当」部分，我们注意 $f(u,v) \ge 0$ 对所有 $(u,v)$ 成立意味着 $\inf_u f(u,v) \ge 0$ 对所有 $v$ 成立，即 $S \succeq 0$。

- **$X \succeq 0$ 当且仅当 $A \succeq 0$、$B^T(I - AA^{\dagger}) = 0$、$C - B^TA^{\dagger}B \succeq 0$。**

设 $A \in \mathbf{R}^{k \times k}$ 且 $\mathrm{rank}(A) = r$。则存在矩阵 $Q_1 \in \mathbf{R}^{k \times r}$、$Q_2 \in \mathbf{R}^{k \times (k-r)}$ 与可逆对角矩阵 $\Lambda \in \mathbf{R}^{r \times r}$ 使

```math
A = \begin{bmatrix} Q_1 & Q_2 \end{bmatrix} \begin{bmatrix} \Lambda & 0 \\ 0 & 0 \end{bmatrix} \begin{bmatrix} Q_1 & Q_2 \end{bmatrix}^T,
```

且 $[Q_1\ Q_2]^T[Q_1\ Q_2] = I$。矩阵

```math
\begin{bmatrix} Q_1 & Q_2 & 0 \\ 0 & 0 & I \end{bmatrix} \in \mathbf{R}^{n \times n}
```

非奇异，因此

```math
\begin{bmatrix} A & B \\ B^T & C \end{bmatrix} \succeq 0 \iff \begin{bmatrix} Q_1 & Q_2 & 0 \\ 0 & 0 & I \end{bmatrix}^T \begin{bmatrix} A & B \\ B^T & C \end{bmatrix} \begin{bmatrix} Q_1 & Q_2 & 0 \\ 0 & 0 & I \end{bmatrix} \succeq 0
```

```math
\iff \begin{bmatrix} \Lambda & 0 & Q_1^TB \\ 0 & 0 & Q_2^TB \\ B^TQ_1 & B^TQ_2 & C \end{bmatrix} \succeq 0
```

```math
\iff Q_2^TB = 0, \quad \begin{bmatrix} \Lambda & Q_1^TB \\ B^TQ_1 & C \end{bmatrix} \succeq 0 .
```

（第一步是 congruence（合同变换）不改变半正定性；中间的矩阵正是把 $X$ 用分块对角矩阵左右夹住的乘积，其中 $A$ 化成 $\mathbf{diag}(\Lambda,0)$ 形态。最后一步用到：若一个分块矩阵半正定、且某个对角块为零块，则对应的非对角块必须为零。）

我们有：$\Lambda \succ 0$ 当且仅当 $A \succeq 0$。可以验证

```math
A^{\dagger} = Q_1\Lambda^{-1}Q_1^T, \qquad I - AA^{\dagger} = Q_2Q_2^T .
```

因此

```math
Q_2^TB = 0 \iff Q_2Q_2^TB = (I - AA^{\dagger})B = 0 .
```

此外，由于 $\Lambda$ 可逆，

```math
\begin{bmatrix} \Lambda & Q_1^TB \\ B^TQ_1 & C \end{bmatrix} \succeq 0 \iff \Lambda \succ 0,\ C - B^TQ_1\Lambda^{-1}Q_1^TB = C - B^TA^{\dagger}B \succeq 0 .
```

（最后一步是把上一条定理（$A \succ 0$ 的情形）用在这个更小的分块矩阵上。）

**(c)** $f$ 的 epigraph 是满足下面两条的点 $(x,t)$ 的集合：

```math
P_0 + x_1P_1 + \cdots + x_nP_n \succ 0
```

以及

```math
(Ax + b)^T (P_0 + x_1P_1 + \cdots + x_nP_n)^{-1} (Ax + b) \le t .
```

用 (b) 的第二个结论，可以把第二个不等式写成

```math
\begin{bmatrix} t & (Ax+b)^T \\ (Ax+b) & P_0 + x_1P_1 + \cdots + x_nP_n \end{bmatrix} \succeq 0 .
```

这是关于变量 $x,t$ 的 linear matrix inequality，即一个 convex constraint。

（这里正是把 $X = \begin{bmatrix} t & (Ax+b)^T \\ (Ax+b) & P \end{bmatrix}$ 与 $A \to t$（标量）、$B \to (Ax+b)^T$、$C \to P$ 对应；条件 $t > 0$ 由 $t \succeq 0$ 与 $X \succeq 0$ 自动得到（除非 $(Ax+b) = 0$ 且 $t=0$，此时也满足 $f(x) = 0 \le t$），于是 (b) 的判据给出 $C - B^TA^{-1}B = P - (Ax+b)(Ax+b)^T/t \succeq 0$，两边乘 $t>0$ 并整理即 $f(x) \le t$。）

## 附加题 2 · Formulate the following optimization problems as semidefinite programs（把下列问题表述为半定规划）

变量为 $x \in \mathbf{R}^n$；$F(x)$ 定义为

```math
F(x) = F_0 + x_1F_1 + x_2F_2 + \cdots + x_nF_n
```

其中 $F_i \in \mathbf{S}^m$。每个小问中 $f$ 的定义域为 $\mathbf{dom}\,f = \{x \in \mathbf{R}^n \mid F(x) \succ 0\}$。

**(a)** Minimize $f(x) = c^T F(x)^{-1} c$，其中 $c \in \mathbf{R}^m$。

**(b)** Minimize $f(x) = \max_{i=1,\dots,K} c_i^T F(x)^{-1} c_i$，其中 $c_i \in \mathbf{R}^m$，$i = 1,\dots,K$。

**(c)** Minimize $f(x) = \sup_{\|c\|_2 \le 1} c^T F(x)^{-1} c$。

**(d)** Minimize $f(x) = \mathbf{E}(c^T F(x)^{-1} c)$，其中 $c$ 是 random vector，均值 $\mathbf{E}\,c = \bar{c}$、covariance $\mathbf{E}(c - \bar{c})(c - \bar{c})^T = S$。

**Solution**。

**(a)**

```math
\begin{aligned}
\text{minimize} \quad & t \\
\text{subject to} \quad & \begin{bmatrix} F(x) & c \\ c^T & t \end{bmatrix} \succeq 0 .
\end{aligned}
```

**(b)**

```math
\begin{aligned}
\text{minimize} \quad & t \\
\text{subject to} \quad & \begin{bmatrix} F(x) & c_i \\ c_i^T & t \end{bmatrix} \succeq 0, \quad i = 1,\dots,K .
\end{aligned}
```

**(c)** $f(x) = \lambda_{\max}(F(x)^{-1})$，故 $f(x) \le t$ 当且仅当 $F(x)^{-1} \preceq tI$。用 Schur complement 得

```math
\begin{aligned}
\text{minimize} \quad & t \\
\text{subject to} \quad & \begin{bmatrix} F(x) & I \\ I & tI \end{bmatrix} \succeq 0 .
\end{aligned}
```

（为什么 $F^{-1} \preceq tI \iff \begin{bmatrix} F & I \\ I & tI \end{bmatrix} \succeq 0$：后者的 Schur complement 为 $tI - I^TF^{-1}I = tI - F^{-1} \succeq 0$，正是 $F^{-1} \preceq tI$。而非对角块取 $I$ 而不是 $c$，是因为 $\sup_{\|c\|_2\le1} c^TF^{-1}c = \lambda_{\max}(F^{-1})$，其等号在 $c$ 取 $F^{-1}$ 的主特征向量时达到——用 $I$ 是把所有方向一起约束。）

**(d)** $f(x) = \bar{c}^T F(x)^{-1}\bar{c} + \mathbf{tr}(F(x)^{-1}S)$。若把 $S$ 分解为 $S = \sum_{k=1}^m c_k c_k^T$，则问题等价于

```math
\text{minimize} \quad \bar{c}^T F(x)^{-1}\bar{c} + \sum_{k=1}^m c_k^T F(x)^{-1}c_k,
```

我们可以把它写成 SDP

```math
\begin{aligned}
\text{minimize} \quad & t_0 + \sum_k t_k \\
\text{subject to} \quad & \begin{bmatrix} F(x) & \bar{c} \\ \bar{c}^T & t_0 \end{bmatrix} \succeq 0 \\
& \begin{bmatrix} F(x) & c_k \\ c_k^T & t_k \end{bmatrix} \succeq 0, \quad k = 1,\dots,m .
\end{aligned}
```

（$\mathbf{tr}(F^{-1}S) = \sum_k \mathbf{tr}(F^{-1}c_kc_k^T) = \sum_k c_k^TF^{-1}c_k$，用迹的循环性。）

## 附加题 3 · Optimality conditions and dual for log-optimal investment problem（log-optimal 投资问题的最优性条件与对偶）

**(a)** 证明习题 4.60 描述的 log-optimal investment problem 的最优性条件可以写成：$\mathbf{1}^T x = 1$、$x \succeq 0$，且对每个 $i$，

```math
x_i > 0 \implies \sum_{j=1}^m \pi_j \frac{p_{ij}}{p_j^T x} = 1, \qquad x_i = 0 \implies \sum_{j=1}^m \pi_j \frac{p_{ij}}{p_j^T x} \le 1 .
```

我们可以这样解释：$p_{ij}/p_j^Tx$ 是一个 random variable，它给出「只投资产 $i$」的投资收益与我们混合组合 $x$ 的投资收益之比。最优性条件是：对我们投了钱的每项资产，这个比值的期望等于一；对我们没投钱的每项资产，这个期望不能超过一。很粗略地说，这意味着我们的组合表现不差于我们选择投资的任何资产，也不会差于我们没投资的任何资产。

提示：可以从 §4.2.3 给出的简单判据出发，或用 KKT conditions，或用作业 4 的附加题 1。

**(b)** 本小问我们推导 log-optimal investment problem 的 dual。先把问题写成

```math
\begin{aligned}
\text{minimize} \quad & -\sum_{j=1}^m \pi_j \log y_j \\
\text{subject to} \quad & y = P^T x, \quad x \succeq 0, \quad \mathbf{1}^T x = 1 .
\end{aligned}
```

这里 $P$ 的列是 $p_1,\dots,p_m$，我们引入了新变量 $y_1,\dots,y_m$，并带隐式约束 $y \succ 0$。我们分别把 dual variables $\nu$、$\lambda$、$\nu_0$ 关联到约束 $y = P^Tx$、$x \succeq 0$、$\mathbf{1}^Tx = 1$。定义

```math
\tilde{\nu}_j = \nu_j/\nu_0, \qquad j = 1,\dots,m,
```

证明 dual problem 可以写成

```math
\begin{aligned}
\text{maximize} \quad & \sum_{j=1}^m \pi_j \log(\tilde{\nu}_j/\pi_j) \\
\text{subject to} \quad & P\tilde{\nu} \preceq \mathbf{1},
\end{aligned}
```

变量为 $\tilde{\nu}$。这里 objective 是给定分布 $\pi$ 与 dual variable $\tilde{\nu}$ 之间的（负）Kullback-Leibler divergence。（KL 散度，$\sum_j \pi_j \log(\pi_j/\tilde\nu_j)$ 的相反数。）

**Solution**。

**(a)** 该问题等同于在 probability simplex 上极小化 $f(x) = -\sum_{j=1}^m \pi_j \log(p_j^Tx)$。若 $x$ feasible，optimality condition 是

```math
\nabla f(x)^T (z - x) \ge 0
```

对所有满足 $z \succeq 0$、$\mathbf{1}^Tz = 1$ 的 $z$ 成立。这成立当且仅当对每个 $k$，

```math
x_k > 0 \implies \frac{\partial f}{\partial x_k} = \min_{i=1,\dots,n} \frac{\partial f}{\partial x_i} = \nabla f(x)^T x .
```

（这里的判据就是作业 4 附加题 1 的结论：$x_k > 0$ 的分量其偏导都等于最小值，$x_k = 0$ 的分量其偏导不小于最小值。）

由 $f(x) = -\sum_{j=1}^m \pi_j \log(p_j^Tx)$，得

```math
\nabla f(x)_i = -\sum_{j=1}^m \pi_j \frac{1}{p_j^Tx} p_{ij},
```

于是

```math
\nabla f(x)^T x = -\sum_{j=1}^m \pi_j \frac{1}{p_j^Tx} p_j^T x = -1 .
```

（因为 $p_j^Tx$ 与 $j$ 的求和无冲突、可约掉，剩下 $-\sum_j \pi_j = -1$。）

最优性条件为：$\mathbf{1}^Tx = 1$、$x \succeq 0$，且对每个 $i$，

```math
x_i > 0 \implies \sum_{j=1}^m \pi_j \frac{p_{ij}}{p_j^Tx} = 1, \qquad x_i = 0 \implies \sum_{j=1}^m \pi_j \frac{p_{ij}}{p_j^Tx} \le 1 .
```

（把上一段的两式合并：$x_i > 0$ 时 $\nabla f(x)_i = -1$，即 $\sum_j \pi_j p_{ij}/(p_j^Tx) = 1$；$x_i = 0$ 时 $\nabla f(x)_i \ge -1$，即该和 $\le 1$。）

**(b)** Lagrangian 为

```math
L(x,\nu,\lambda,\nu_0) = -\sum_{j=1}^m \pi_j \log y_j + \nu^T(y - P^Tx) - \lambda^T x + \nu_0(\mathbf{1}^Tx - 1).
```

除非 $-\nu^TP - \lambda + \nu_0\mathbf{1} = 0$，它下界无界。由于 $L$ 关于 $y$ 是可分离的，我们可以逐个 $y_j$ 极小化 $L$。我们求得最小值在 $y_j = \pi_j/\nu_j$ 处取到，于是

```math
g(\nu,\lambda,\nu_0) = 1 - \nu_0 + \sum_{j=1}^m \pi_j \log(\nu_j/\pi_j),
```

只要 $P\nu \preceq \nu_0\mathbf{1}$。于是可以把 dual problem 写成

```math
\begin{aligned}
\text{maximize} \quad & 1 - \nu_0 + \sum_{j=1}^m \pi_j \log(\nu_j/\pi_j) \\
\text{subject to} \quad & P\nu \preceq \nu_0\mathbf{1}
\end{aligned}
```

变量为 $\nu \in \mathbf{R}^m$、$\nu_0 \in \mathbf{R}$。它有隐式约束 $\nu \succ 0$。

（逐项验算：$- \pi_j\log y_j + \nu_jy_j$ 对 $y_j$ 求导得 $-\pi_j/y_j + \nu_j = 0$，即 $y_j = \pi_j/\nu_j$，代回得 $\pi_j\log(\nu_j/\pi_j) + \pi_j$，对 $j$ 求和得 $\sum_j \pi_j\log(\nu_j/\pi_j) + \sum_j\pi_j = \sum_j \pi_j\log(\nu_j/\pi_j) + 1$，再减去 $\nu_0$ 即得 $g$。）

我们可以进一步化简：对 $\nu_0$ 解析地优化。由约束不等式可见 $\nu_0 > 0$。定义 $\tilde{\nu} = \nu/\nu_0$，得到关于变量 $\tilde{\nu},\nu_0$ 的问题

```math
\begin{aligned}
\text{maximize} \quad & 1 - \nu_0 + \sum_{j=1}^m \pi_j \log(\tilde{\nu}_j\nu_0/\pi_j) \\
\text{subject to} \quad & \nu_0P\tilde{\nu} \preceq \nu_0\mathbf{1} .
\end{aligned}
```

从约束中约去 $\nu_0$，得

```math
\begin{aligned}
\text{maximize} \quad & 1 - \nu_0 + \log\nu_0 + \sum_{j=1}^m \pi_j \log(\tilde{\nu}_j/\pi_j) \\
\text{subject to} \quad & P\tilde{\nu} \preceq \mathbf{1} .
\end{aligned}
```

$\nu_0$ 的 optimal value 显然是 $\nu_0 = 1$（因为 $1 - \nu_0 + \log\nu_0$ 在 $\nu_0 = 1$ 处取到最大值 $0$）。于是最终得到 dual problem

```math
\begin{aligned}
\text{maximize} \quad & \sum_{j=1}^m \pi_j \log(\tilde{\nu}_j/\pi_j) \\
\text{subject to} \quad & P\tilde{\nu} \preceq \mathbf{1} .
\end{aligned}
```

## 附加题 4 · Log-optimal investment strategy（log-optimal 投资策略，数值实例）

本问题求解习题 4.60 描述的 log-optimal investment problem 的一个具体实例，$n = 5$ 项资产、每期 $m = 10$ 种可能结果。问题数据定义在 `log_opt_invest.m` 中，矩阵 $P$ 的各行给出资产回报向量 $p_j^T$。各结果等概率，即 $\pi_j = 1/m$。矩阵 $P$ 的每一列给出对应资产在不同可能结果下的回报。你可以查看这些列以了解资产类型。例如，最后一项资产给出固定的、确定的 $1\%$ 回报；第一项资产风险很高，偶尔有大的回报，而（更经常地）有可观的亏损。

求 log-optimal investment strategy $x^\star$ 及其对应的长期 growth rate $R^\star_{\mathrm{lt}}$。把它与 uniform allocation strategy（均匀配置策略，$x = (1/n)\mathbf{1}$）得到的长期 growth rate 比较，也与「纯投单项资产」比较。

对 optimal investment strategy 与 uniform investment strategy，分别画出 10 条 accumulated wealth（累积财富）的样本轨迹，即 $W(T) = W(0)\prod_{t=1}^T \lambda(t)$，$T = 0,\dots,200$，初始财富 $W(0) = 1$。

为省去你自己琢磨如何模拟财富轨迹或如何画好看的图的麻烦，我们已在 `log_opt_invest.m` 中包含了模拟与绘图代码；你只需补上求 $x^\star$ 的代码。

提示：当前版本的 cvx 不处理 logarithm，但你可以用 `geomean()` 求解该问题。

**Solution**。

**(a)** 下面的代码用于求解该问题：

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
% Find log-optimal investment policy
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

求得 log-optimal investment strategy 为

```math
x_{\mathrm{opt}} = (0.0580,\ 0.4000,\ 0.2923,\ 0.2497,\ 0.0000).
```

该策略达到长期 growth rate $R_{\mathrm{lt}}^\star = 0.0231$。相比之下，uniform allocation strategy 达到的 growth rate 为 $R_{\mathrm{unif}} = 0.0114$。

显然资产 1 是高风险资产：投在它上面的财富会以 $20\%$ 的概率增长为 $3.50$ 倍，以 $80\%$ 的概率减半。另一方面，资产 5 是每期确定回报 $1\%$ 的资产。最后，资产 2、3、4 是低风险资产。结果在这个例子里，log-optimal policy 是把极少的财富投在高风险资产上、不投在有保证的资产上，而把大部分财富投在资产 2 上。

（`geomean` 的用法要点：$\max \sum_j \pi_j\log(p_j^Tx)$ 在 $\pi_j = 1/m$ 时等于 $\log\big(\prod_j p_j^Tx\big)^{1/m} = \log \mathbf{geomean}(Px)$，而 $\log$ 单调，故等价于极大化 `geomean(P*x)`。）

**(b)** 下面的代码用于生成随机事件序列与轨迹图：

```matlab
% Generate random event sequences
rand('state',0);
N = 10; T = 200;
w_opt = []; w_unif = [];
for i = 1:N
events = ceil(rand(1,T)*m);
P_event = P(events,:);
w_opt = [w_opt [1; cumprod(P_event*x_opt)]];
w_unif = [w_unif [1; cumprod(P_event*x_unif)]];
end
% Plot wealth versus time
figure
semilogy(w_opt,'g')
hold on
semilogy(w_unif,'r--')
grid
axis tight
xlabel('time')
ylabel('wealth')
```

它生成下面的图：

[Figure 1：累积财富轨迹（纵轴对数刻度 $10^{-1}$ 到 $10^3$，横轴 $t$ 从 0 到 200）——绿色实线为 log-optimal policy 的 10 条轨迹，全部单调上升、末端集中在 $10^1$ 到 $10^2$ 之间；红色虚线为 uniform policy 的 10 条轨迹，散布极宽，少数末端超过 $10^2$，多数在 $10^0$ 上下，个别一度跌破 $10^0$]

log-optimal investment policy 一致地增加财富。而 uniform allocation policy 产生相当随机的轨迹：少数财富大增，许多表现很差。原因在于该策略把 $20\%$ 的财富投在了高风险资产上。

## 附加题 5 · Maximizing house profit in a gamble and imputed probabilities（在赌局中最大化庄家利润与隐含概率）

一组 $n$ 个参与者就 $m$ 个结果（标记为 $1,\dots,m$）中哪一个会发生下注。参与者 $i$ 提出以价格 $p_i > 0$ 购买至多 $q_i > 0$ 份赌注合约，这些合约押「真实结果落在集合 $S_i \subset \{1,\dots,m\}$ 中」。庄家然后卖给她 $x_i$ 份合约，$0 \le x_i \le q_i$。若真实结果 $j$ 落在 $S_i$ 中，则参与者 $i$ 每份合约收到 $\$1$，即共收到 $x_i$；否则她输了，什么都收不到。庄家总共收进 $x_1p_1 + \cdots + x_np_n$，并支付一个取决于结果 $j$ 的金额

```math
\sum_{j \in S_i} x_i .
```

两者之差就是 house profit（庄家利润）。

（† 原文此处求和号下标写作 $\sum_{j \in S_i} x_i$，符号略欠严谨：应理解为 $\sum_{i:\ j \in S_i} x_i$，即「结果 $j$ 出现时，所有押中 $j$ 的参与者各得 $x_i$，求和」。）

**(a) Optimal house strategy（庄家最优策略）**。庄家应如何决定 $x$，使其 worst-case profit（最坏情形利润，在所有可能结果上取最坏）最大？（庄家在审查所有参与者报价之后再确定 $x$。）

**(b) Imputed probabilities（隐含概率）**。假设 $x^\star$ 使 worst-case house profit 最大。证明：存在可能结果上的一个概率分布 $\pi$（即 $\pi \in \mathbf{R}^m_+$、$\mathbf{1}^T\pi = 1$），使得 $x^\star$ 同时也最大化 expected house profit（期望庄家利润）。解释如何求出 $\pi$。

提示：把 (a) 中的问题表述为 LP；$\pi$ 可以由该 LP 的 optimal dual variables 构造。

**Remark（评注）**。给定 $\pi$，报价 $i$ 的「fair」价格为 $p_i^{\mathrm{fair}} = \sum_{j \in S_i} \pi_j$。所有 $p_i > p_i^{\mathrm{fair}}$ 的报价会被全部成交（即 $x_i = q_i$）；所有 $p_i < p_i^{\mathrm{fair}}$ 的报价会被拒绝（即 $x_i = 0$）。

**Remark（评注）**。本题说明，如何从一组赌徒的报价中猜出各结果的概率（例如选举结果）。

**(c) Numerical example（数值实例）**。用你的方法处理下面这个简单例子：$n = 5$ 个参与者、$m = 5$ 个可能结果，参与者报价为

| Participant $i$ | $p_i$ | $q_i$ | $S_i$ |
|---|---|---|---|
| 1 | 0.50 | 10 | {1,2} |
| 2 | 0.60 | 5 | {4} |
| 3 | 0.60 | 5 | {1,4,5} |
| 4 | 0.60 | 20 | {2,5} |
| 5 | 0.20 | 10 | {3} |

把 optimal worst-case house profit 与「接受所有报价」（即 $x_i = q_i$）时的 worst-case house profit 比较。求出隐含概率。

**Solution**。

**(a)** worst-case house profit 为

```math
p^T x - \max_{j=1,\dots,m} \sum_{j \in S_i} x_i,
```

它是 $x$ 的 piecewise-linear concave function（分段线性凹函数）。为找出使 worst-case profit 最大的 $x$，我们求解问题

```math
\begin{aligned}
\text{maximize} \quad & p^T x - \max_{j=1,\dots,m} a_j^T x \\
\text{subject to} \quad & 0 \preceq x \preceq q,
\end{aligned}
```

变量为 $x$。$a_j^T$ 是 subset matrix（子集矩阵）$A$ 的各行，其中

```math
A_{ji} = \begin{cases} 1 & j \in S_i \\ 0 & \text{otherwise} . \end{cases}
```

（即 $a_j^Tx = \sum_{i:\, j\in S_i} x_i$，正是结果 $j$ 出现时庄家要付出的总额。）

**(b)** (a) 中的问题可以表述为

```math
\begin{aligned}
\text{maximize} \quad & p^T x - t \\
\text{subject to} \quad & t\mathbf{1} \succeq Ax \\
& 0 \preceq x \preceq q,
\end{aligned}
```

（式 (4)）其中 $t$ 是新的标量变量。Lagrangian 为

```math
L(x,t,\lambda_1,\lambda_2,\lambda_3) = t - p^T x + \lambda_1^T(Ax - t\mathbf{1}) - \lambda_2^T x + \lambda_3^T(x - q).
```

它下界有界当且仅当 $\mathbf{1}^T\lambda_1 = 1$ 且 $A^T\lambda_1 - \lambda_2 + \lambda_3 = p$。dual 可以写成

```math
\begin{aligned}
\text{maximize} \quad & -q^T\lambda_3 \\
\text{subject to} \quad & \mathbf{1}^T\lambda_1 = 1 \\
& A^T\lambda_1 - \lambda_2 + \lambda_3 = p \\
& \lambda_1 \succeq 0, \quad \lambda_2 \succeq 0, \quad \lambda_3 \succeq 0,
\end{aligned}
```

（式 (5)）变量为 $\lambda_1$、$\lambda_2$、$\lambda_3$。注意 $\lambda_1$ 必须满足 $\mathbf{1}^T\lambda_1 = 1$ 与 $\lambda_1 \succeq 0$，因此它是一个 probability distribution。

设 $x^{\star}_{\mathrm{wc}}$、$t^\star$、$\lambda_1^\star$、$\lambda_2^\star$、$\lambda_3^\star$ 是问题 (4) 的 primal 与 dual optimal 解，令 $\pi^\star = \lambda_1^\star$。为最大化 expected house profit，我们求解问题

```math
\begin{aligned}
\text{maximize} \quad & p^T x - \pi^{\star T}Ax \\
\text{subject to} \quad & 0 \preceq x \preceq q .
\end{aligned}
```

（式 (6)）设 $b_1^T,\dots,b_n^T$ 是 $A^T$ 的各行。我们知道，一个点 $x^{\star}_e$ 是问题 (6) 的 optimal 解当且仅当：$p_i - b_i^T\pi^\star > 0$ 时 $x^\star_{ei} = q_i$；$p_i - b_i^T\pi^\star < 0$ 时 $x^\star_{ei} = 0$；$p_i - b_i^T\pi^\star = 0$ 时 $0 \le x^\star_{ei} \le q_i$。

（这是 (6) 这种 separable linear program 的显然最优性条件：每个 $x_i$ 独立地在 $[0,q_i]$ 里按系数符号取端点。）

为看清为何 $x^\star_e = x^\star_{\mathrm{wc}}$，我们看问题 (4) 的某条 KKT condition。它可以写成

```math
p - A^T\pi^\star = \lambda_3^\star - \lambda_2^\star,
```

其中 $\pi^\star = \lambda_1^\star$。若 $p_i - b_i^T\pi^\star > 0$，则必有 $\lambda^\star_{3i} - \lambda^\star_{2i} > 0$，意味着 $\lambda^\star_{2i} = 0$ 且 $\lambda^\star_{3i} > 0$（由 complementary slackness（互补松弛）），于是 $x^\star_{\mathrm{wc}i} = q_i$。类似地，若 $p_i - b_i^T\pi^\star < 0$，则 $\lambda^\star_{3i} - \lambda^\star_{2i} < 0$，意味着 $\lambda^\star_{2i} > 0$ 且 $\lambda^\star_{3i} = 0$，于是 $x^\star_{\mathrm{wc}i} = 0$。最后，当 $p_i - b_i^T\pi^\star = 0$ 时，必有 $\lambda^\star_{2i} = 0$ 且 $\lambda^\star_{3i} = 0$，于是 $0 \le x^\star_{\mathrm{wc}i} \le q_i$。

（要把「$\lambda_{3i} > 0 \Rightarrow x_i = q_i$」这一步讲清：问题 (4) 中 $\lambda_3$ 对应约束 $x \preceq q$，即 $x - q \preceq 0$，故 complementary slackness 为 $\lambda_{3i}(x_i - q_i) = 0$；$\lambda_{2i}$ 对应 $-x \preceq 0$，故 $\lambda_{2i}x_i = 0$。）

总之，为找出一个使同一个 $x^\star$ 既最大化 worst-case 也最大化 expected house profit 的概率分布，我们求解 dual LP (5)，并取 $\pi^\star = \lambda_1^\star$。

**(c)** 下面的 cvx 代码求解该问题。

```matlab
% solution for gambling problem
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
maximize (p'*x-t)
subject to
lambda1: A*x <= t
x >= 0
x <= q
cvx_end
% optimal worst case house profit
pwc = cvx_optval
% optimal worst case profit if all offer are accepted
pwc_accept = p'*q-max(A*q)
% imputed probabilities
pi = lambda1
% fair prices
pfair = A'*pi
% optimal purchase quantities
xopt = x
```

结果汇总于下表。optimal worst-case house profit 为 $3.5$；若接受所有报价，worst-case house profit 为 $-5$。隐含概率为

```math
\pi = (0.1145,\ 0.3855,\ 0.0945,\ 0.1910,\ 0.2145).
```

对应的 fair prices 与 optimal contract numbers 见下表。

| Participant $i$ | $p_i$ | $p_i^{\mathrm{fair}}$ | $q_i$ | $x_i$ | $S_i$ |
|---|---|---|---|---|---|
| 1 | 0.50 | 0.5000 | 10 | 5 | {1,2} |
| 2 | 0.60 | 0.1910 | 5 | 5 | {4} |
| 3 | 0.60 | 0.5200 | 5 | 5 | {1,4,5} |
| 4 | 0.60 | 0.6000 | 20 | 5 | {2,5} |
| 5 | 0.20 | 0.0945 | 10 | 10 | {3} |

（核对 $p^{\mathrm{fair}}$：$\pi_1+\pi_2 = 0.1145+0.3855 = 0.5000$；$\pi_4 = 0.1910$；$\pi_1+\pi_4+\pi_5 = 0.1145+0.1910+0.2145 = 0.5200$；$\pi_2+\pi_5 = 0.3855+0.2145 = 0.6000$；$\pi_3 = 0.0945$。于是参与者 2、4 的报价 $0.60$ 严格高于 fair price，前者被全额成交；参与者 1、4 的价格恰等于 fair price，$x$ 在 $[0,q]$ 内任取，表中取中间值。总的 worst-case profit 验算：$\sum_i p_ix_i = 0.5\cdot5+0.6\cdot5+0.6\cdot5+0.6\cdot5+0.2\cdot10 = 13.5$，最坏结果付出 $10$，故 $13.5-10 = 3.5$，与 `cvx_optval` 一致。）

## 附加题 6 · Heuristic suboptimal solution for Boolean LP（Boolean LP 的启发式次优解）

本题建立在习题 4.15 与 5.13 之上，二者涉及 Boolean LP

```math
\begin{aligned}
\text{minimize} \quad & c^T x \\
\text{subject to} \quad & Ax \preceq b \\
& x_i \in \{0,1\}, \quad i = 1,\dots,n,
\end{aligned}
```

其 optimal value 为 $p^\star$。设 $x^{\mathrm{rlx}}$ 是 LP relaxation

```math
\begin{aligned}
\text{minimize} \quad & c^T x \\
\text{subject to} \quad & Ax \preceq b \\
& 0 \preceq x \preceq \mathbf{1}
\end{aligned}
```

的解，于是 $L = c^Tx^{\mathrm{rlx}}$ 是 $p^\star$ 的一个 lower bound。relaxed solution $x^{\mathrm{rlx}}$ 也可以用来猜一个 Boolean 点 $\hat{x}$：按阈值 $t \in [0,1]$ 对其分量做 rounding（取整）：

```math
\hat{x}_i = \begin{cases} 1 & x^{\mathrm{rlx}}_i \ge t \\ 0 & \text{otherwise}, \end{cases}
```

$i = 1,\dots,n$。显然 $\hat{x}$ 是 Boolean 的（即各分量属于 $\{0,1\}$）。若它对 Boolean LP feasible，即 $A\hat{x} \preceq b$，则它可以被视为 Boolean LP 的一个好的（若未必 optimal 的）点的猜测。它的 objective value $U = c^T\hat{x}$ 是 $p^\star$ 的一个 upper bound。若 $U$ 与 $L$ 接近，则 $\hat{x}$ 接近 optimal；具体地说，$\hat{x}$ 对 Boolean LP 的次优程度不会超过 $(U - L)$。

这种 rounding 未必有效；事实上可能对所有阈值取值 $\hat{x}$ 都不可行。但对某些问题实例它效果很好。

当然，从这个 $x^{\mathrm{rlx}}$ 构造（可能）可行的好点，这个简单方案还有许多变体。

最后进入正题。用

```matlab
rand('state',0);
n=100;
m=300;
A=rand(m,n);
b=A*ones(n,1)/2;
c=-rand(n,1);
```

生成问题数据。你可以把 $x_i$ 想成「我们接受或拒绝的某项工作」，把 $-c_i$ 想成接受工作 $i$ 所产生的（正的）revenue。$Ax \preceq b$ 可以看成 $m$ 种资源的限制：$A_{ij}$（正）是接受工作 $j$ 时消耗的资源 $i$ 的量；$b_i$（正）是可用资源 $i$ 的量。

求出 relaxed LP 的解并检查其分量。注意相应的 lower bound $L$。对（比如）100 个均匀分布在 $[0,1]$ 上的 $t$ 值做 threshold rounding。对每个 $t$ 值，记录 objective value $c^T\hat{x}$ 与最大 constraint violation（约束违反量）$\max_i (A\hat{x} - b)_i$。把 objective value 与 maximum violation 对 $t$ 作图。务必在图上标出 $\hat{x}$ feasible 与不可行的 $t$ 值。

找出一个使 $\hat{x}$ feasible 且 objective value 最小的 $t$ 值，并记录相应的 upper bound $U$。给出 $p^\star$ 的 upper bound 与 lower bound 之间的 gap $U - L$。若你定义了向量 `obj` 与 `maxviol`，可以用 `U=min(obj(find(maxviol<=0)))` 求出 upper bound。

**Solution**。下面的 Matlab 代码求出解

```matlab
% generate data for boolean LP relaxation & heuristic
rand('state',0);
n=100;
m=300;
A=rand(m,n);
b=A*ones(n,1)/2;
c=-rand(n,1);
% solve LP relaxation
cvx_begin
variable x(n)
minimize (c'*x)
subject to
A*x <= b
x>=0
x<=1
cvx_end
xrlx = x;
L=cvx_optval;
% sweep over threshold & round
thres=0:0.01:1;
maxviol = zeros(length(thres),1);
obj = zeros(length(thres),1);
for i=1:length(thres)
xhat = (xrlx>=thres(i));
maxviol(i) = max(A*xhat-b);
obj(i) = c'*xhat;
end
% find least upper bound and associated threshold
i_feas=find(maxviol<=0);
U=min(obj(i_feas))
t=min(i_feas);
min_thresh=thres(t)
% plot objective and max violation versus threshold
subplot(2,1,1)
plot(thres(1:t-1),maxviol(1:t-1),'r',thres(t:end),maxviol(t:end),'b','linewidth',2)
xlabel('threshold');
ylabel('max violation');
subplot(2,1,2)
hold on; plot(thres,L*ones(size(thres)),'k','linewidth',2);
plot(thres(1:t-1),obj(1:t-1),'r',thres(t:end),obj(t:end),'b','linewidth',2);
xlabel('threshold');
ylabel('objective');
```

[Figure 2（原文 Figure 1）：上下两幅图，横轴都是 threshold $t \in [0,1]$（刻度 0, 0.1, …, 1）。上图纵轴 max violation，从 $30$ 降到 $-30$：曲线在 $t < 0.6006$ 段为红色（$\hat{x}$ infeasible）且随 $t$ 增大单调下降，在 $t \ge 0.6006$ 段为蓝色（$\hat{x}$ feasible）继续下降到约 $-30$。下图纵轴 objective：黑色水平线为 lower bound $L = -33.1672$；红色段（infeasible 区域）的 objective 从约 $-40$ 上升到 $-32.4$，蓝色段（feasible 区域）继续上升到约 $0$；最优点在红蓝交界 $t = 0.6006$ 处]

由 relaxed LP 得到的 lower bound 是 $L = -33.1672$。我们发现阈值 $t = 0.6006$ 给出可行 $\hat{x}$ 的最好（最小）objective value：$U = -32.4450$。差值为 $0.7222$。因此 $t = 0.6006$ 时的 $\hat{x}$，其次优程度不超过 $0.7222$。

图 1 中，红色线表示给出 infeasible $\hat{x}$ 的阈值取值，蓝色线对应 feasible 的 $\hat{x}$。我们看到 maximum violation 随阈值增大而减小。这之所以发生，是因为约束矩阵 $A$ 只有非负元素。阈值为 $0$ 时所有工作都被选中，这是不可行解。随着我们增大阈值，工作被逐个移除（而不新增工作），于是 maximum violation 单调减小。对一般的 boolean LP，相应的图未必表现出单调行为。

（读图要点：$U - L$ 就是把 relaxed LP 的分数解「硬压成 $0/1$」所付的代价。这个 0.72 相对 $|L| \approx 33$ 只有约 $2\%$，说明 threshold rounding 在这个随机实例上相当有效——原因是 $A$ 全为正、$b$ 取 $A\mathbf{1}/2$ 使得约束「恰好一半的工作量可承受」，分数解集中在中段，阈值化损失的资源平衡不大。）
