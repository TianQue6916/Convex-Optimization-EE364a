# EE364a Homework 5 additional problems（EE364a 作业 5 附加题）

> 原文：`HW5 附加题.pdf`（5 页），EE364a, Winter 2007–08, Prof. S. Boyd。这是题面；解答见 `HW5 解答.pdf` 第 5 页起的 "Solutions to additional exercises"。

## 1. Schur complements（Schur 补）

考虑对称矩阵 $X = X^T \in \mathbf{R}^{n \times n}$，分块为

```math
X = \begin{bmatrix} A & B \\ B^T & C \end{bmatrix},
```

其中 $A \in \mathbf{R}^{k \times k}$。若 $\det A \ne 0$，矩阵

```math
S = C - B^T A^{-1} B
```

称为 $A$ 在 $X$ 中的 Schur complement。Schur complement 在许多场合出现，并出现在许多重要公式与定理中。例如有 $\det X = \det A \det S$。（这条不必证明。）

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

## 2. Formulate the following optimization problems as semidefinite programs（把下列问题表述为半定规划）

变量为 $x \in \mathbf{R}^n$；$F(x)$ 定义为

```math
F(x) = F_0 + x_1F_1 + x_2F_2 + \cdots + x_nF_n
```

其中 $F_i \in \mathbf{S}^m$。每个小问中 $f$ 的定义域为 $\mathbf{dom}\,f = \{x \in \mathbf{R}^n \mid F(x) \succ 0\}$。

**(a)** Minimize $f(x) = c^T F(x)^{-1} c$，其中 $c \in \mathbf{R}^m$。

**(b)** Minimize $f(x) = \max_{i=1,\dots,K} c_i^T F(x)^{-1} c_i$，其中 $c_i \in \mathbf{R}^m$，$i = 1,\dots,K$。

**(c)** Minimize $f(x) = \sup_{\|c\|_2 \le 1} c^T F(x)^{-1} c$。

**(d)** Minimize $f(x) = \mathbf{E}(c^T F(x)^{-1} c)$，其中 $c$ 是 random vector，均值 $\mathbf{E}\,c = \bar{c}$、covariance $\mathbf{E}(c - \bar{c})(c - \bar{c})^T = S$。

## 3. Optimality conditions and dual for log-optimal investment problem（log-optimal 投资问题的最优性条件与对偶）

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

这里 $P$ 的列是 $p_1,\dots,p_m$，我们引入了新变量 $y_1,\dots,y_m$，并带隐式约束 $y \succ 0$。我们分别把 dual variables $\nu$、$\lambda$、$\nu_0$ 关联到约束 $y = P^Tx$、$x \succeq 0$、$\mathbf{1}^Tx = 1$。定义 $\tilde{\nu}_j = \nu_j/\nu_0$（$j = 1,\dots,m$），证明 dual problem 可以写成

```math
\begin{aligned}
\text{maximize} \quad & \sum_{j=1}^m \pi_j \log(\tilde{\nu}_j/\pi_j) \\
\text{subject to} \quad & P\tilde{\nu} \preceq \mathbf{1},
\end{aligned}
```

变量为 $\tilde{\nu}$。这里 objective 是给定分布 $\pi$ 与 dual variable $\tilde{\nu}$ 之间的（负）Kullback-Leibler divergence（KL 散度）。

## 4. Log-optimal investment strategy（log-optimal 投资策略，数值实例）

本问题求解习题 4.60 描述的 log-optimal investment problem 的一个具体实例，$n = 5$ 项资产、每期 $m = 10$ 种可能结果。问题数据定义在 `log_opt_invest.m` 中，矩阵 $P$ 的各行给出资产回报向量 $p_j^T$。各结果等概率，即 $\pi_j = 1/m$。矩阵 $P$ 的每一列给出对应资产在不同可能结果下的回报。你可以查看这些列以了解资产类型。例如，最后一项资产给出固定的、确定的 $1\%$ 回报；第一项资产风险很高，偶尔有大的回报，而（更经常地）有可观的亏损。

求 log-optimal investment strategy $x^\star$ 及其对应的长期 growth rate $R^\star_{\mathrm{lt}}$。把它与 uniform allocation strategy（$x = (1/n)\mathbf{1}$）得到的长期 growth rate 比较，也与「纯投单项资产」比较。

对 optimal investment strategy 与 uniform investment strategy，分别画出 10 条 accumulated wealth 的样本轨迹，即 $W(T) = W(0)\prod_{t=1}^T \lambda(t)$，$T = 0,\dots,200$，初始财富 $W(0) = 1$。

为省去你自己琢磨如何模拟财富轨迹或如何画好看的图的麻烦，我们已在 `log_opt_invest.m` 中包含了模拟与绘图代码；你只需补上求 $x^\star$ 的代码。

提示：当前版本的 cvx 不处理 logarithm，但你可以用 `geomean()` 求解该问题。

## 5. Maximizing house profit in a gamble and imputed probabilities（在赌局中最大化庄家利润与隐含概率）

一组 $n$ 个参与者就 $m$ 个结果（标记为 $1,\dots,m$）中哪一个会发生下注。参与者 $i$ 提出以价格 $p_i > 0$ 购买至多 $q_i > 0$ 份赌注合约，这些合约押「真实结果落在集合 $S_i \subset \{1,\dots,m\}$ 中」。庄家然后卖给她 $x_i$ 份合约，$0 \le x_i \le q_i$。若真实结果 $j$ 落在 $S_i$ 中，则参与者 $i$ 每份合约收到 $\$1$，即共收到 $x_i$。否则她输了，什么都收不到。庄家总共收进 $x_1p_1 + \cdots + x_np_n$，并支付一个取决于结果 $j$ 的金额 $\sum_{i:\,j\in S_i} x_i$（原文印作 $\sum_{j\in S_i} x_i$，见下 †）。两者之差就是 house profit（庄家利润）。

† 原文求和号把「结果下标 $j$」与「被求和的参与者下标 $i$」写混了；正确含义是「若结果为 $j$，庄家付出所有押中 $j$ 的参与者的 $x_i$ 之和」。

**(a) Optimal house strategy**。庄家应如何决定 $x$，使其 worst-case profit（在所有可能结果上取最坏）最大？（庄家在审查所有参与者报价之后再确定 $x$。）

**(b) Imputed probabilities**。假设 $x^\star$ 使 worst-case house profit 最大。证明：存在可能结果上的一个概率分布 $\pi$（即 $\pi \in \mathbf{R}^m_+$、$\mathbf{1}^T\pi = 1$），使得 $x^\star$ 同时也最大化 expected house profit。解释如何求出 $\pi$。

提示：把 (a) 中的问题表述为 LP；$\pi$ 可以由该 LP 的 optimal dual variables 构造。

**Remark**。给定 $\pi$，报价 $i$ 的「fair」价格为 $p_i^{\mathrm{fair}} = \sum_{j \in S_i} \pi_j$。所有 $p_i > p_i^{\mathrm{fair}}$ 的报价会被全部成交（即 $x_i = q_i$）；所有 $p_i < p_i^{\mathrm{fair}}$ 的报价会被拒绝（即 $x_i = 0$）。

**Remark**。本题说明，如何从一组赌徒的报价中猜出各结果的概率（例如选举结果）。

**(c) Numerical example**。用你的方法处理下面这个简单例子：$n = 5$ 个参与者、$m = 5$ 个可能结果，参与者报价为

| Participant $i$ | $p_i$ | $q_i$ | $S_i$ |
|---|---|---|---|
| 1 | 0.50 | 10 | {1,2} |
| 2 | 0.60 | 5 | {4} |
| 3 | 0.60 | 5 | {1,4,5} |
| 4 | 0.60 | 20 | {2,5} |
| 5 | 0.20 | 10 | {3} |

把 optimal worst-case house profit 与「接受所有报价」（即 $x_i = q_i$）时的 worst-case house profit 比较。求出隐含概率。

## 6. Heuristic suboptimal solution for Boolean LP（Boolean LP 的启发式次优解）

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

的解，于是 $L = c^Tx^{\mathrm{rlx}}$ 是 $p^\star$ 的一个 lower bound。relaxed solution $x^{\mathrm{rlx}}$ 也可以用来猜一个 Boolean 点 $\hat{x}$：按阈值 $t \in [0,1]$ 对其分量做 rounding：

```math
\hat{x}_i = \begin{cases} 1 & x^{\mathrm{rlx}}_i \ge t \\ 0 & \text{otherwise}, \end{cases}
```

（$i = 1,\dots,n$）。显然 $\hat{x}$ 是 Boolean 的（即各分量属于 $\{0,1\}$）。若它对 Boolean LP feasible，即 $A\hat{x} \preceq b$，则它可以被视为 Boolean LP 的一个好的（若未必 optimal 的）点的猜测。它的 objective value $U = c^T\hat{x}$ 是 $p^\star$ 的一个 upper bound。若 $U$ 与 $L$ 接近，则 $\hat{x}$ 接近 optimal；具体地说，$\hat{x}$ 对 Boolean LP 的次优程度不会超过 $(U - L)$。

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

求出 relaxed LP 的解并检查其分量。注意相应的 lower bound $L$。对（比如）100 个均匀分布在 $[0,1]$ 上的 $t$ 值做 threshold rounding。对每个 $t$ 值，记录 objective value $c^T\hat{x}$ 与最大 constraint violation $\max_i (A\hat{x} - b)_i$。把 objective value 与 maximum violation 对 $t$ 作图。务必在图上标出 $\hat{x}$ feasible 与不可行的 $t$ 值。

找出一个使 $\hat{x}$ feasible 且 objective value 最小的 $t$ 值，并记录相应的 upper bound $U$。给出 $p^\star$ 的 upper bound 与 lower bound 之间的 gap $U - L$。若你定义了向量 `obj` 与 `maxviol`，可以用 `U=min(obj(find(maxviol<=0)))` 求出 upper bound。
