# EE364a HW8 解答（作业 8 解答）

> 说明：本文为 Stanford EE364a（Winter 2007-08，Prof. S. Boyd）作业 8 官方解答的**行内双语译注 + 完整解答**。中文为主体，英文术语首次出现时以 `English（中文）` 标注，之后直接用 English。公式使用行内 `$...$` 与独立 ```math 围栏，不用 ``，不用 Unicode 上下标。

---

## 题目 1（9.8）

**题干：** Steepest descent method in $\ell_\infty$-norm. Explain how to find a steepest descent direction in the $\ell_\infty$-norm, and give a simple interpretation.

**中文：** 在 $\ell_\infty$-范数（$\ell_\infty$-norm，无穷范数）下的最速下降法（steepest descent method）。请解释如何在 $\ell_\infty$-范数下求最速下降方向（steepest descent direction），并给出一个简单的直观解释。

**解答：**

归一化的最速下降方向（normalized steepest descent direction）为

```math
\Delta x_{\mathrm{nsd}} = -\mathrm{sign}\big(\nabla f(x)\big),
```

其中符号函数（sign）按分量（componentwise）取。直观解释（interpretation）为：若关于 $x_k$ 的偏导数（partial derivative）为正，则我们朝减小 $x_k$ 的方向迈步；若为正，则朝增大 $x_k$ 的方向迈步。

〔原文此处前后两句均写"为正"，按对称性应为"为正则减小 $x_k$，为负则增大 $x_k$"，待核。〕

未归一化的最速下降方向（unnormalized steepest descent direction）为

```math
\Delta x_{\mathrm{sd}} = -\| \nabla f(x) \|_1 \, \mathrm{sign}\big(\nabla f(x)\big).
```

**推导要点：** 在一般范数 $\|\cdot\|$ 下，最速下降方向是最小化一阶 Taylor 近似的方向，即求解

```math
\Delta x_{\mathrm{nsd}} = \arg\min_{\|v\| \le 1} \nabla f(x)^T v .
```

当取 $\ell_\infty$-范数时，约束 $\|v\|_\infty \le 1$ 即各分量 $|v_i| \le 1$，最小化 $\sum_i (\nabla f)_i v_i$ 的最优解是取 $v_i = -\mathrm{sign}\big((\nabla f)_i\big)$，从而得到 $\Delta x_{\mathrm{nsd}} = -\mathrm{sign}(\nabla f(x))$。

---

## 题目 2（10.1）

**题干：** Nonsingularity of the KKT matrix. Consider the KKT matrix

```math
\begin{bmatrix} P & A^T \\ A & 0 \end{bmatrix},
```

where $P \in \mathbb{S}^n_+$, $A \in \mathbb{R}^{p\times n}$, and $\operatorname{rank} A = p < n$.

**中文：** KKT 矩阵（KKT matrix）的非奇异性（nonsingularity）。考虑 KKT 矩阵（如上），其中 $P$ 是半正定对称矩阵（positive semidefinite, $\mathbb{S}^n_+$），$A$ 是 $p\times n$ 矩阵，且 $\operatorname{rank} A = p < n$（即 $A$ 行满秩，行数小于列数）。

**(a) 子问：** 证明下面每一条都与 KKT 矩阵非奇异等价：
- $\mathcal{N}(P) \cap \mathcal{N}(A) = \{0\}$；
- 若 $Ax = 0$ 且 $x \neq 0$，则 $x^T P x > 0$；
- $F^T P F \succ 0$，其中 $F \in \mathbb{R}^{n\times(n-p)}$ 是满足 $\mathcal{R}(F) = \mathcal{N}(A)$ 的矩阵；
- 对某个 $Q \succeq 0$，有 $P + A^T Q A \succ 0$。

**(b) 子问：** 证明若 KKT 矩阵非奇异，则它恰好有 $n$ 个正特征值（positive eigenvalue）和 $p$ 个负特征值（negative eigenvalue）。

**解答：**

**(a)** 记 $\mathcal{N}(\cdot)$ 为矩阵的零空间（nullspace），$\mathcal{R}(\cdot)$ 为值域（range）。

**条件 1 与条件 2 等价。**

- （$2 \Rightarrow 1$）设 $x \in \mathcal{N}(A) \cap \mathcal{N}(P)$ 且 $x \neq 0$，则 $Ax = 0$ 且 $x \neq 0$，但 $x^T P x = 0$，这与条件 2 矛盾。故不存在这样的非零 $x$，即 $\mathcal{N}(P) \cap \mathcal{N}(A) = \{0\}$。
- （$1 \Rightarrow 2$）反设条件 2 不成立，即存在 $x$ 使 $Ax = 0$、$x \neq 0$，但 $x^T P x = 0$。因为 $P \succeq 0$（半正定），$x^T P x = 0$ 意味着 $P x = 0$，即 $x \in \mathcal{N}(P)$。于是 $x \in \mathcal{N}(P) \cap \mathcal{N}(A)$ 且 $x \neq 0$，这与条件 1 矛盾。

**条件 2 与条件 3 等价。**

- 若 $Ax = 0$ 且 $x \neq 0$，则 $x$ 必可写成 $x = Fz$，其中 $z \neq 0$（因为 $\operatorname{rank}(F) = n - p$，即 $F$ 的列构成 $\mathcal{N}(A)$ 的一组基，而 $\dim \mathcal{N}(A) = n - p$）。于是

```math
x^T P x = z^T F^T P F z > 0,
```

这正等价于 $F^T P F \succ 0$。

**条件 2 与条件 4 等价。**

- （$2 \Rightarrow 4$）若条件 2 成立，则对任意非零 $x$，

```math
x^T (P + A^T A) x = x^T P x + \| A^T x \|_2^2 > 0.
```

因此取 $Q = I$ 即满足条件 4（$P + A^T Q A \succ 0$）。这里关键是：若 $x \neq 0$ 且 $Ax \neq 0$，则 $\|A^T x\|_2^2 > 0$；若 $Ax = 0$，则由条件 2 得 $x^T P x > 0$。两者至少一项严格为正，故和严格大于 0。

- （$4 \Rightarrow 2$）若条件 4 对某个 $Q \succeq 0$ 成立，则对任意非零 $x$，

```math
x^T (P + A^T Q A) x = x^T P x + x^T A^T Q A x > 0.
```

特别地，若 $Ax = 0$ 且 $x \neq 0$，则第二项 $x^T A^T Q A x = (Ax)^T Q (Ax) = 0$，故必须有 $x^T P x > 0$。这正是条件 2。

**四条件与 KKT 矩阵非奇异的等价性。**

- （四条件之一成立 $\Rightarrow$ KKT 矩阵非奇异）反设存在 $x$ 满足 $Ax = 0$、$Px = 0$、$x \neq 0$，则

```math
\begin{bmatrix} P & A^T \\ A & 0 \end{bmatrix}
\begin{bmatrix} x \\ 0 \end{bmatrix} = 0,
```

说明 KKT 矩阵奇异（singular）。因此若 KKT 矩阵非奇异，则不存在这样的非零 $x$，即条件 1（等价于其余条件）成立。

- （KKT 矩阵奇异 $\Rightarrow$ 四条件不成立）设 KKT 矩阵奇异，即存在不全为零的 $x, z$ 使

```math
\begin{bmatrix} P & A^T \\ A & 0 \end{bmatrix}
\begin{bmatrix} x \\ z \end{bmatrix} = 0.
```

展开得两个方程：

```math
P x + A^T z = 0, \qquad A x = 0.
```

对第一个方程左乘 $x^T$，得 $x^T P x + x^T A^T z = 0$。利用 $Ax = 0$，第二项 $x^T A^T z = (Ax)^T z = 0$，故 $x^T P x = 0$。由 $P \succeq 0$ 得 $Px = 0$。若 $x \neq 0$，则 $x \in \mathcal{N}(P) \cap \mathcal{N}(A)$，与条件 1 矛盾。故只能 $x = 0$。此时由 $x, z$ 不全为零知 $z \neq 0$，而第一个方程退化为 $A^T z = 0$，这与 $\operatorname{rank} A = p$（即 $A$ 行满秩，$A^T$ 列满秩，零空间为 $\{0\}$）矛盾。

综上，四条件与 KKT 矩阵非奇异等价。

**(b)** 由 (a) 部分（条件 4，取 $Q = I$），$P + A^T A \succ 0$。因此存在非奇异矩阵 $R \in \mathbb{R}^{n\times n}$ 使

```math
R^T (P + A^T A) R = I.
```

设 $AR = U \Sigma V_1^T$ 为 $AR$ 的奇异值分解（singular value decomposition, SVD），其中 $U \in \mathbb{R}^{p\times p}$（正交），$\Sigma = \operatorname{diag}(\sigma_1, \dots, \sigma_p) \in \mathbb{R}^{p\times p}$，$V_1 \in \mathbb{R}^{n\times p}$。取 $V_2 \in \mathbb{R}^{n\times(n-p)}$ 使

```math
V = \begin{bmatrix} V_1 & V_2 \end{bmatrix}
```

为正交矩阵（即 $V_2$ 的列张成 $V_1$ 列空间的正交补），并定义

```math
S = \begin{bmatrix} \Sigma \\ 0 \end{bmatrix} \in \mathbb{R}^{p\times n}.
```

于是 $AR = U S V^T$，因此

```math
V^T R^T (P + A^T A) R V = V^T R^T P R V + S^T S = I.
```

（这里用到 $(AR)^T(AR) = V S^T U^T U S V^T = V S^T S V^T$。）故

```math
V^T R^T P R V = I - S^T S
```

是对角矩阵。记该矩阵为 $\Lambda$：

```math
\Lambda = V^T R^T P R V = \operatorname{diag}\big(1 - \sigma_1^2, \dots, 1 - \sigma_p^2, 1, \dots, 1\big).
```

对 KKT 矩阵作合同变换（congruence transformation）：

```math
\begin{bmatrix} V^T R^T & 0 \\ 0 & U^T \end{bmatrix}
\begin{bmatrix} P & A^T \\ A & 0 \end{bmatrix}
\begin{bmatrix} R V & 0 \\ 0 & U \end{bmatrix}
=
\begin{bmatrix} \Lambda & S^T \\ S & 0 \end{bmatrix},
```

且合同变换保持惯性指数（inertia，即正、负、零特征值的个数），故 KKT 矩阵的 inertia 等于右端矩阵的 inertia。

对右端矩阵作行、列置换（permutation），得到一个分块对角矩阵，其对角块为 $n$ 个：

```math
\begin{bmatrix} \lambda_i & \sigma_i \\ \sigma_i & 0 \end{bmatrix}, \quad i = 1, \dots, p,
```

以及

```math
\lambda_i = 1, \quad i = p+1, \dots, n.
```

（后 $n-p$ 个是平凡的一阶块 $[1]$，对应一个正特征值 1。）

每个 $2\times 2$ 块的特征值由特征方程

```math
\det\begin{bmatrix} \lambda_i - \mu & \sigma_i \\ \sigma_i & -\mu \end{bmatrix}
= -\mu(\lambda_i - \mu) - \sigma_i^2 = 0
```

即

```math
\mu^2 - \lambda_i \mu - \sigma_i^2 = 0
```

解得

```math
\mu = \frac{\lambda_i \pm \sqrt{\lambda_i^2 + 4\sigma_i^2}}{2},
```

一个为正、一个为负（因为 $\sqrt{\lambda_i^2 + 4\sigma_i^2} > |\lambda_i|$）。

因此共有 $p$ 个正特征值、$p$ 个负特征值（来自 $p$ 个 $2\times 2$ 块），再加上后 $n-p$ 个正特征值（每个一阶块 $[1]$），合计

```math
p + (n - p) = n
```

个正特征值、$p$ 个负特征值。证毕。

---

## 题目 3（11.13）

**题干：** Self-concordance and negative entropy.

(a) Show that the negative entropy function $x \log x$ (on $\mathbb{R}_{++}$) is not self-concordant.

(b) Show that for any $t > 0$, $t x \log x - \log x$ is self-concordant (on $\mathbb{R}_{++}$).

**中文：** 自和谐性（self-concordance）与负熵（negative entropy）。

(a) 证明负熵函数 $x \log x$（在 $\mathbb{R}_{++}$ 上，即 $x > 0$）不是自和谐的（self-concordant）。

(b) 证明对任意 $t > 0$，函数 $t x \log x - \log x$ 在 $\mathbb{R}_{++}$ 上是自和谐的。

**背景（关键判据）：** 一元函数 $f$ 自和谐的定义是：对定义域内所有 $x$，

```math
|f'''(x)| \le 2\, f''(x)^{3/2}.
```

（这里 $f''(x) > 0$。）这是本题的核心判据，必须显式验证。

**解答：**

**(a)** 考虑 $f(x) = x \log x$，逐阶求导：

```math
f'(x) = 1 + \log x, \qquad
f''(x) = \frac{1}{x}, \qquad
f'''(x) = -\frac{1}{x^2}.
```

于是

```math
\frac{|f'''(x)|}{f''(x)^{3/2}}
= \frac{1/x^2}{(1/x)^{3/2}}
= \frac{1/x^2}{1/x^{3/2}}
= \frac{1}{\sqrt{x}},
```

当 $x \to 0^+$ 时它无上界（unbounded above）。特别地，自和谐不等式 $|f'''(x)| \le 2 f''(x)^{3/2}$ 在 $x = 1/5$ 处失败（因为此时 $\dfrac{1}{\sqrt{1/5}} = \sqrt{5} > 2$），故 $f$ 不自和谐。

**(b)** 考虑 $g(x) = t x \log x - \log x$，逐阶求导：

```math
g'(x) = -\frac{1}{x} + t + t \log x, \qquad
g''(x) = \frac{1}{x^2} + \frac{t}{x}, \qquad
g'''(x) = -\frac{2}{x^3} - \frac{t}{x^2}.
```

于是

```math
\frac{|g'''(x)|}{g''(x)^{3/2}}
= \frac{2/x^3 + t/x^2}{(1/x^2 + t/x)^{3/2}}
= \frac{2 + t x}{(1 + t x)^{3/2}}.
```

（分子分母同乘 $x^3$：分子 $(2/x^3 + t/x^2)\cdot x^3 = 2 + tx$；分母 $(1/x^2 + t/x)^{3/2} = \big((1+tx)/x^2\big)^{3/2} = (1+tx)^{3/2}/x^3$，故比值即 $(2+tx)/(1+tx)^{3/2}$。）

定义

```math
h(a) = \frac{2 + a}{(1 + a)^{3/2}},
```

则 $h(tx) = \dfrac{|g'''(x)|}{g''(x)^{3/2}}$。我们有 $h(0) = 2$，下面证明对 $a > 0$ 有 $h'(a) < 0$，即 $h$ 在 $a > 0$ 上递减，从而 $h(a) \le h(0) = 2$，于是

```math
\frac{|g'''(x)|}{g''(x)^{3/2}} \le 2.
```

求导：

```math
h'(a)
= \frac{(1+a)^{3/2} - \tfrac{3}{2}(1+a)^{1/2}(2+a)}{(1+a)^3}
= \frac{(1+a)^{1/2}\big((1+a) - \tfrac{3}{2}(2+a)\big)}{(1+a)^3}
= \frac{-\big(2 + a/2\big)}{(1+a)^{5/2}}
< 0,
```

对 $a > 0$ 成立。证毕。

（注：这里 $g''(x) = 1/x^2 + t/x > 0$ 对 $x > 0$ 恒成立，满足自和谐定义中二阶导为正的前提。）

---

## 附加练习解答（Solutions to additional exercises）

### 标准形式 LP 的障碍法（Standard form LP barrier method）

**题干：** 在下面三个练习中，实现一个求解标准形式 LP（standard form linear program）的障碍法（barrier method）：

```math
\begin{array}{ll}
\mathrm{minimize} & c^T x \\
\mathrm{subject\ to} & Ax = b, \quad x \succeq 0,
\end{array}
```

其中变量 $x \in \mathbb{R}^n$，$A \in \mathbb{R}^{m\times n}$ 且 $m < n$。全题假设 $A$ 满秩（full rank），且子水平集（sublevel set）$\{x \mid Ax = b,\ x \succeq 0,\ c^T x \le \gamma\}$ 均有界（bounded）。（若非如此，中心化问题将无下界。）

**中文：** 实现障碍法求解标准形式线性规划（linear program, LP）：目标最小化 $c^T x$，约束为 $Ax = b$ 且 $x \succeq 0$（分量非负）。

---

### 练习 1：中心化步骤（Centering step）

**题干：** 实现 Newton 法（Newton's method）求解中心化问题（centering problem）：

```math
\begin{array}{ll}
\mathrm{minimize} & c^T x - \displaystyle\sum_{i=1}^n \log x_i \\
\mathrm{subject\ to} & Ax = b,
\end{array}
```

给定严格可行起点 $x_0$。代码应接受 $A, b, c, x_0$，返回 $x^\star$（原始最优点 primal optimal point）、$\nu^\star$（对偶最优点 dual optimal point），以及执行的 Newton 步数。用分块消元法（block elimination method）计算 Newton 步。（也可经 KKT 系统计算，并与分块消元结果比较——两步应接近，但当某个 $x_i$ 很小时可能收到 KKT 矩阵条件数（condition number）警告。）

画出 $\lambda^2/2$ 关于迭代次数 $k$ 的图，验证实现具有渐近二次收敛（asymptotic quadratic convergence）。停止准则（stopping criterion）可取 $\lambda^2/2 \le 10^{-6}$。实验改变参数 $\alpha$、$\beta$，观察对固定实例所需总 Newton 步数的影响。检查计算得到的 $x^\star$ 与 $\nu^\star$（近似）满足 KKT 条件（KKT conditions）。

**中文：** 用 Newton 法求中心化问题（在 $Ax=b$ 下最小化 $c^T x - \sum_i \log x_i$，即对数障碍（logarithmic barrier）目标）的最优点，并验证二次收敛与 KKT 条件。

**数据生成建议：** 先随机生成 $A$（检查是否满秩），再生成随机正向量 $x_0$ 并取 $b = Ax_0$（这保证 $x_0$ 严格可行），$c$ 可随机取。为确保子水平集有界，可给 $A$ 加一行全正的行。若要复现同一数据，需设置均匀与正态随机数生成器（random number generator）的种子（seed）。

**提示：**
- 建议用 $\lambda^2 = -\Delta x_{\mathrm{nt}}^T \nabla f(x)$ 计算（Newton 减量 Newton decrement 的平方）。不必单独算 $\lambda$，直接用 $\lambda^2$ 即可（理由见下）。
- 计算的 Newton 步 $\Delta x_{\mathrm{nt}}$ 可能有小数值误差。当 $x$ 接近最优点时，$\lambda^2 = -\Delta x_{\mathrm{nt}}^T \nabla f(x)$ 的计算值可能（略）为负。若对其开方求 $\lambda$ 会得到复数（complex number）而无法恢复，且线搜索永不退出。但这只在 $x$ 接近最优时发生，所以只要按 $\lambda^2/2 \le 10^{-6}$ 退出就没问题（即便 $\lambda^2$ 计算值为负）。
- 线搜索（line search）中，必须先将步长 $t$ 反复乘 $\beta$ 直到 $x + t\Delta x_{\mathrm{nt}}$ 可行（严格正）。否则求 $f$ 时会对负数取对数，无法恢复。

**解答：**

Newton 步 $\Delta x_{\mathrm{nt}}$ 由 KKT 系统定义：

```math
\begin{bmatrix} H & A^T \\ A & 0 \end{bmatrix}
\begin{bmatrix} \Delta x_{\mathrm{nt}} \\ w \end{bmatrix}
=
\begin{bmatrix} -g \\ 0 \end{bmatrix},
```

其中

```math
H = \operatorname{diag}\Big(\frac{1}{x_1^2}, \dots, \frac{1}{x_n^2}\Big),
\qquad
g = c - \Big(\frac{1}{x_1}, \dots, \frac{1}{x_n}\Big).
```

（$H$ 是目标函数 $c^T x - \sum_i \log x_i$ 的 Hessian，$g$ 是梯度。）KKT 系统可用分块消元高效求解：先解

```math
A H^{-1} A^T w = -A H^{-1} g,
```

再令

```math
\Delta x_{\mathrm{nt}} = -H^{-1}(A^T w + g).
```

KKT 最优性条件（KKT optimality condition）为

```math
A^T \nu^\star + c - \Big(\frac{1}{x_1^\star}, \dots, \frac{1}{x_n^\star}\Big) = 0.
```

当 Newton 法收敛，即 $\Delta x_{\mathrm{nt}} \approx 0$ 时，$w$ 就是对偶最优点 $\nu^\star$。

（分块消元推导：由第一行 $H \Delta x_{\mathrm{nt}} + A^T w = -g$ 得 $\Delta x_{\mathrm{nt}} = -H^{-1}(A^T w + g)$；代入第二行 $A \Delta x_{\mathrm{nt}} = 0$ 得 $-A H^{-1}(A^T w + g) = 0$，即 $A H^{-1} A^T w = -A H^{-1} g$。）

以下函数用 Newton 法计算解析中心（analytic center）：

```matlab
function [x_star, nu_star, lambda_hist] = lp_acent(A,b,c,x_0)
% solves problem
% minimize c'*x - sum(log(x))
% subject to A*x = b
% using Newton's method, given strictly feasible starting point x0
% input (A, b, c, x_0)
% returns primal and dual optimal points
% lambda_hist is a vector showing lambda^2/2 for each newton step
% returns [], [] if MAXITERS reached, or x_0 not feasible
% algorithm parameters
ALPHA = 0.01;
BETA = 0.5;
EPSILON = 1e-6;
MAXITERS = 100;
if (min(x_0) <= 0) || (norm(A*x_0 - b) > 1e-3) % x0 not feasible
  fprintf('FAILED');
  nu_star = []; x_star = []; lambda_hist=[];
  return;
end
m = length(b);
n = length(x_0);
x = x_0; lambda_hist = [];
for iter = 1:MAXITERS
  H = diag(x.^(-2));
  g = c - x.^(-1);
  % lines below compute newton step via whole KKT system
  % M = [ H A'; A zeros(m,m)];
  % d = M\[-g; zeros(m,1)];
  % dx = d(1:n);
  % w = d(n+1:end);
  % newton step by elimination method
  w = (A*diag(x.^2)*A')\(-A*diag(x.^2)*g);
  dx = -diag(x.^2)*(A'*w + g);
  lambdasqr = -g'*dx; % dx'*H*dx;
  lambda_hist = [lambda_hist lambdasqr/2];
  if lambdasqr/2 <= EPSILON break; end
  % backtracking line search
  % first bring the point inside the domain
  t = 1; while min(x+t*dx) <= 0 t = BETA*t; end
  % now do backtracking line search
  while c'*(t*dx)-sum(log(x+t*dx))+sum(log(x))-ALPHA*t*g'*dx > 0
    t = BETA*t;
  end
  x = x + t*dx;
end
if iter == MAXITERS % MAXITERS reached
  fprintf('ERROR: MAXITERS reached.\n');
  x_star = []; nu_star = [];
else
  x_star = x;
  nu_star = w;
end
```

**代码说明：** 这里 $H^{-1} = \operatorname{diag}(x_1^2, \dots, x_n^2)$，故 `diag(x.^2)` 即 $H^{-1}$。`w = (A*diag(x.^2)*A')\(-A*diag(x.^2)*g)` 即解 $A H^{-1} A^T w = -A H^{-1} g$；`dx = -diag(x.^2)*(A'*w + g)` 即 $\Delta x_{\mathrm{nt}} = -H^{-1}(A^T w + g)$。$\lambda^2$ 用 `lambdasqr = -g'*dx`（等价于 `dx'*H*dx`）。回溯线搜索分两步：先把 $t$ 缩小到点严格可行（`min(x+t*dx) > 0`），再做标准的回溯线搜索（backtracking line search）。

数据按题设生成，取 $A \in \mathbb{R}^{100\times 500}$。下方画出 Newton 减量随 Newton 步数的图，二次收敛（quadratic convergence）清晰可见。两种方法算出的 Newton 方向非常接近；函数返回点满足 KKT 最优性条件。

[[FIGURE: 页 9, Newton 减量 $\lambda^2/2$ 随 Newton 步数（横轴 1–11）的对数纵轴图，从约 $10^2$ 快速降到 $10^{-12}$，呈二次收敛的凸曲线]]

---

### 练习 2：带严格可行起点的 LP 求解器（LP solver with strictly feasible starting point）

**题干：** 利用第 1 部分的中心化代码，实现一个障碍法求解标准形式 LP（同前）。LP 求解器接受 $A, b, c, x_0$，返回 $x^\star$。当对偶间隙（duality gap，以 $n/t$ 度量）小于 $10^{-3}$ 时终止。（若容差取得太小可能遇到数值困难。）在多个实例上对照 cvx 的解验证你的 LP 求解器。第 1 部分关于随机数据生成的说明同样适用。

实验参数 $\mu$，观察其对每次中心化步骤的 Newton 步数、以及求解所需总 Newton 步数的影响。对一个 $n = 500$、$m = 100$ 的实例画出算法进展：纵轴为对偶间隙（对数尺度），横轴为累计总 Newton 步数（线性尺度）。

算法应返回 $2\times k$ 矩阵 `history`（$k$ 为中心化总步数），第一行为每次中心化的 Newton 步数，第二行为每次中心化结束时的对偶间隙。为使图与书中（如 11.4 图，页 572）一致，可用代码：

```matlab
[xx, yy] = stairs(cumsum(history(1,:)), history(2,:));
semilogy(xx,yy);
```

**中文：** 用障碍法（内点法 interior-point method 的障碍变体）从严格可行起点求解 LP，验证对偶间隙终止，并与 cvx 对照。

**解答：**

以下函数用障碍法求解 LP：

```matlab
function [x_star, history, gap] = lp_barrier(A,b,c,x_0)
% solves standard form LP
% minimize c^T x
% subject to Ax = b, x >= 0;
% using barrier method, given strictly feasible x0
% uses function std_form_LP_acent() to carry out centering steps
% returns:
% - primal optimal point x_star
% - history, a 2xk matrix that returns number of newton steps
% in each centering step (top row) and duality gap (bottom row)
% (k is total number of centering steps)
% - gap, optimal duality gap
% barrier method parameters
T_0 = 1;
MU = 20;
EPSILON = 1e-3; % duality gap stopping criterion
n = length(x_0);
t = T_0;
x = x_0;
history = [];
while(1)
  [x_star, nu_star, lambda_hist] = lp_acent(A,b,t*c,x);
  x = x_star;
  gap = n/t;
  history = [history [length(lambda_hist); gap]];
  if gap < EPSILON break; end
  t = MU*t;
end
```

**代码说明：** 障碍法外层循环逐步增大参数 $t$（乘 $\mu = 20$），每步用第 1 部分的 `lp_acent` 对目标 $t c^T x - \sum_i \log x_i$ 做中心化（相当于对原目标 $c^T x$ 施加障碍项 $-\frac{1}{t}\sum_i \log x_i$）。对偶间隙（duality gap）为 $n/t$。当 $n/t < 10^{-3}$ 时停止。

以下脚本生成测试数据并画出障碍法进展，同时用 cvx 检查计算结果：

```matlab
% script that generates data and tests the functions
% std_form_LP_acent
% std_form_LP_barrier
clear all;
m = 100;
n = 500;
rand('seed',0);
randn('seed',0);
A = [randn(m-1,n); ones(1,n)];
x_0 = rand(n,1) + 0.1;
b = A*x_0;
c = randn(n,1);
% analytic centering
figure
[x_star, nu_star, lambda_hist] = lp_acent(A,b,c,x_0);
semilogy(lambda_hist,'bo-')
xlabel('iters')
ylabel('lambdasqr/2')
% solve the LP with barrier
figure
[x_star, history, gap] = lp_barrier(A,b,c,x_0);
[xx, yy] = stairs(cumsum(history(1,:)), history(2,:));
semilogy(xx,yy,'bo-');
xlabel('iters')
ylabel('gap')
p_star = c'*x_star;
% solve LP using cvx for comparison
cvx_begin
variable x(n)
minimize(c'*x)
subject to
A*x == b
x >= 0
cvx_end
fprintf('\n\nOptimal value found by barrier method:\n');
p_star
fprintf('Optimal value found by CVX:\n');
cvx_optval
fprintf('Duality gap from barrier method:\n');
gap
```

**脚本说明：** 数据生成中，$A$ 由 $m-1$ 行标准正态随机行加一行全 1 行构成（全正行保证子水平集有界），$x_0$ 为正随机向量，$b = A x_0$ 保证严格可行，$c$ 随机。第一幅图画出中心化的 Newton 减量（对数轴）随迭代次数的收敛；第二幅图用 `stairs`/`semilogy` 画出对偶间隙（对数轴）随累计 Newton 步数的阶梯下降。

[[FIGURE: 页 12, 对偶间隙（对数纵轴，约 $10^{-3}$ 到 $10^3$）随累计 Newton 步数（横轴 10–55）的阶梯下降图]]

---

### 练习 3：LP 求解器（LP solver）

**题干：** 利用第 2 部分的代码，实现一个通用的标准形式 LP 求解器，接受 $A, b, c$，判断（严格）可行性，并在问题（严格）可行时返回最优点。你需要实现一个阶段 I 方法（phase I method），判断问题是否严格可行，若是则找到一个严格可行点，再喂给第 2 部分代码。事实上可以用第 2 部分代码实现阶段 I。

为找严格可行初始点 $x_0$，求解阶段 I 问题：

```math
\begin{array}{ll}
\mathrm{minimize} & t \\
\mathrm{subject\ to} & Ax = b \\
& x \succeq (1 - t)\mathbf{1} \\
& t \ge 0,
\end{array}
```

变量为 $x$ 和 $t$。若能找到可行 $(x, t)$ 且 $t < 1$，则 $x$ 对原问题严格可行。反之亦然，故原 LP 严格可行当且仅当 $t^\star < 1$（$t^\star$ 为阶段 I 问题的最优值）。

阶段 I 问题可用任意满足 $Ax_0 = b$ 的 $x_0$ 初始化 $x$，并取 $t_0 = 2 - \min_i x_0^i$（这里可假设 $\min_i x_0^i \le 0$，否则 $x_0$ 已是严格可行点，直接结束）。可用变量代换 $z = x + (t-1)\mathbf{1}$ 把阶段 I 问题化为第 2 部分的形式。

在多个数值例子（含可行与不可行实例）上对照 cvx 检查你的 LP 求解器。

**中文：** 实现通用 LP 求解器（阶段 I 找可行点 + 阶段 II 障碍法求最优），处理可行与不可行两种情形。

**解答：**

以下函数实现完整 LP 求解器（阶段 I + 阶段 II）：

```matlab
function [x_star,p_star,gap,status,nsteps] = lp_solve(A,b,c);
% solves the LP
% minimize c^T x
% subject to Ax = b, x >= 0;
% using a barrier method
% computes a strictly feasible point by carrying out a phase I method
% returns:
% - a primal optimal point x_star
% - the primal optimal value p_star
% - status: either 'Infeasible' or 'Solved'
% - nsteps(1): number of newton steps for phase I
% - nsteps(2): number of newton steps for phase II
[m,n] = size(A);
nsteps = zeros(2,1);
% phase I
x0 = A\b; t0 = 2+max(0,-min(x0));
A1 = [A, -A*ones(n,1)];
b1 = b - A*ones(n,1);
z0 = x0 + t0*ones(n,1) - ones(n,1);
c1 = [zeros(n,1); 1];
[z_star, history, gap] = lp_barrier(A1,b1,c1,[z0;t0]);
if (z_star(n+1) >= 1)
  fprintf('\nProblem is infeasible\n');
  x_star = []; p_star = Inf; status = 'Infeasible';
  nsteps(1) = sum(history(1,:)); gap = [];
  return;
end
fprintf('\nFeasible point found\n');
nsteps(1) = sum(history(1,:));
x_0 = z_star(1:n) - z_star(n+1)*ones(n,1) + ones(n,1);
% phase II
[x_star, history, gap] = lp_barrier(A,b,c,x_0);
status = 'Solved'; p_star = c'*x_star;
nsteps(2) = sum(history(1,:));
```

**推导说明（阶段 I 的变量代换）：** 阶段 I 问题约束为 $x \succeq (1-t)\mathbf{1}$，即 $x - (1-t)\mathbf{1} \succeq 0$。令 $z = x + (t-1)\mathbf{1} = x - (1-t)\mathbf{1}$，则 $z \succeq 0$，且 $x = z - (t-1)\mathbf{1}$。代入 $Ax = b$ 得

```math
A\big(z - (t-1)\mathbf{1}\big) = b
\;\Longrightarrow\;
A z - A(t-1)\mathbf{1} = b
\;\Longrightarrow\;
A z - A\mathbf{1}\, t = b - A\mathbf{1}.
```

故增广矩阵取 $A_1 = [A,\ -A\mathbf{1}]$（$A\mathbf{1} = A \cdot \mathrm{ones}(n,1)$）、$b_1 = b - A\mathbf{1}$，变量为 $[z; t]$，目标为最小化 $t$（即 $c_1 = [0,\dots,0,1]$）。初始 $z_0 = x_0 + (t_0 - 1)\mathbf{1}$，其中 $t_0 = 2 - \min_i x_0^i$（当 $\min_i x_0^i \le 0$ 时 $t_0 \ge 2$，保证 $z_0 \succeq 0$ 严格可行）。

**判定逻辑：** 若阶段 I 最优 $t^\star = z_\mathrm{star}(n+1) \ge 1$，则原问题不可行；否则 $t^\star < 1$，由 $x = z - (t-1)\mathbf{1}$ 反解出 $x_0 = z_\mathrm{star}(1:n) - z_\mathrm{star}(n+1)\mathbf{1} + \mathbf{1}$，即严格可行点，进入阶段 II。

以下在两个实例（一个不可行、一个可行）上测试，并与 cvx 对照：

```matlab
% solves standard form LP for two problem instances
clear all;
m = 100;
n = 500;
% infeasible problem instance
rand('seed',0);
randn('seed',0);
A = [rand(m-1,n); ones(1,n)];
b = randn(m,1);
c = randn(n,1);
[x_star,p_star,gap,status,nsteps] = lp_solve(A,b,c);
% solve LP using cvx for comparison
cvx_begin
variable x(n)
minimize(c'*x)
subject to
A*x == b
x >= 0
cvx_end
% feasible problem instance
A = [randn(m-1,n); ones(1,n)];
v = rand(n,1) + 0.1;
b = A*v;
c = randn(n,1);
[x_star,p_star,gap,status,nsteps] = lp_solve(A,b,c);
% solve LP using cvx for comparison
cvx_begin
variable x(n)
minimize(c'*x)
subject to
A*x == b
x >= 0
cvx_end
fprintf('\n\nOptimal value found by barrier method:\n');
p_star
fprintf('Optimal value found by CVX:\n');
cvx_optval
fprintf('Duality gap from barrier method:\n');
gap
```

**脚本说明：** 不可行实例取随机 $b$（不与 $A$ 的列生成可行点匹配），可行实例取 $b = A v$（$v$ 为正随机向量）保证可行。两例均与 cvx 输出对比最优值。

---

## 附：关键概念速查（Key concepts）

- **最速下降（steepest descent）**：在给定范数下最小化一阶 Taylor 近似的方向；$\ell_\infty$-范数下退化为逐分量取符号。
- **KKT 矩阵（KKT matrix）**：$\begin{bmatrix} P & A^T \\ A & 0 \end{bmatrix}$ 的惯性指数由 $P$ 在 $\mathcal{N}(A)$ 上的限制决定。
- **惯性指数（inertia）**：对称矩阵正、负、零特征值的个数三元组，合同变换下不变。
- **自和谐函数（self-concordant function）**：一元情形判据为 $|f'''(x)| \le 2 f''(x)^{3/2}$。
- **障碍法（barrier method）**：对数障碍 $-\sum_i \log x_i$ 逐步加权，配合中心化 Newton 步与对偶间隙 $n/t$ 终止。
- **Newton 减量（Newton decrement）**：$\lambda(x) = (-\nabla f^T \Delta x_{\mathrm{nt}})^{1/2}$，其平方用于停止准则与二次收敛验证。
- **阶段 I / 阶段 II（phase I / phase II）**：先构造辅助问题找严格可行点，再正式求解。
