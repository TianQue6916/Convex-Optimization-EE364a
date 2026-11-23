# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

> <span style="color:#7f8c8d;">Source: SEE复习课/review5_single.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

## EE364a Review Session 5（EE364a 复习课 第 5 讲）

<!-- p.1 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**开场与通知 Announcements**</span>

本页为开场标题页，主题为：EE364a 复习课第 5 讲（EE364a Review Session 5）。

课程通知（announcements）：

- 作业 1 与作业 2 已批改完毕（homeworks 1 and 2 graded）。
- 作业 4 参考答案已发布（homework 4 solutions），请特别查看附加题 1 的解答（check solution to additional problem 1）。
- SCPD 电话答疑时间（phone-in office hours）：每周二 18:00–19:00，电话 650-723-1156。

<!-- p.2 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**互补松弛 Complementary slackness**</span>

考虑如下（不一定是凸的）优化问题（optimization problem）：

```math
\begin{array}{ll}
\text{minimize}   & f_0(x) \\
\text{subject to} & f_i(x) \le 0, \quad i = 1, \ldots, m, \\
                  & h_i(x) = 0, \quad i = 1, \ldots, p.
\end{array}
```

设 $x^\star$ 为原问题（primal problem）的最优点，$(\lambda^\star, \nu^\star)$ 为对偶问题（dual problem）的最优点，并假设强对偶性（strong duality）成立（即 $p^\star = d^\star$）。这意味着：

```math
\begin{aligned}
f_0(x^\star) &= g(\lambda^\star, \nu^\star)
   = \inf_x \left( f_0(x) + \sum_{i=1}^{m} \lambda_i^\star f_i(x) + \sum_{i=1}^{p} \nu_i^\star h_i(x) \right) \\
&\le f_0(x^\star) + \sum_{i=1}^{m} \lambda_i^\star f_i(x^\star) + \sum_{i=1}^{p} \nu_i^\star h_i(x^\star)
   \le f_0(x^\star).
\end{aligned}
```

读这条不等式链的方法：第一个等号就是强对偶性（$d^\star = g(\lambda^\star, \nu^\star)$ 且 $p^\star = d^\star$）；中间的 "$\le$" 是把下确界换成在 $x = x^\star$ 处的具体取值（下确界不超过任何一点的取值）；最后一个 "$\le$" 来自 $x^\star$ 的可行性与 $\lambda^\star \succeq 0$：不等式约束项之和 $\sum_i \lambda_i^\star f_i(x^\star) \le 0$，而等式约束项之和为零。

<!-- p.3 -->

这一结果有几个重要结论（conclusions）。

- $x^\star$ 在 $x$ 上最小化 $L(x, \lambda^\star, \nu^\star)$（但注意 $L(x, \lambda^\star, \nu^\star)$ 还可能有别的最小化点）。我们很快会看到它的一次应用。

另一个结论是：

```math
\sum_{i=1}^{m} \lambda_i^\star f_i(x^\star) = 0.
```

这是什么意思？

<span style="color:#2471a3;">**[question]**</span> what does this mean?（这意味着什么？）

> <span style="color:#2471a3;">**[solution]**</span> 答案（answer）：逐项地，$\lambda_i^\star f_i(x^\star) = 0$，$i = 1, \ldots, m$。等价地说：$\lambda_i^\star > 0 \Rightarrow f_i(x^\star) = 0$；或者 $f_i(x^\star) < 0 \Rightarrow \lambda_i^\star = 0$。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 互补松弛（complementary slackness）是 KKT 条件（KKT conditions）的组成部分，值得从直觉上吃透：把拉格朗日乘子理解为约束的"价格"，只有当约束在最优点处被顶满（$f_i(x^\star) = 0$，即紧约束）时，它的价格 $\lambda_i^\star$ 才可能为正；若约束尚有富余（$f_i(x^\star) < 0$），价格必为零——没有人会为用不完的资源付钱。这条"价格—资源"对应在 6.006 的最大流-最小割对偶中能找到精确同构：最小割里被饱和的边对应流量约束取等号，正如紧约束对应正乘子；未饱和的边则像松弛约束一样"免费"。另外注意本页推导的技巧：整条链的首尾都是 $f_0(x^\star)$，被夹在中间的加权和被迫为零，这种"等式—不等式夹逼"是对偶性证明中反复出现的套路，后面推导 KKT 条件时还会一再见到。

<!-- p.4 -->

### <span style="color:#2471a3;">**[example]**</span> <span style="color:#c0392b;">**例：在矩形盒上最小化线性函数 Example: minimizing a linear function over a rectangle**</span>

来看一个我们熟悉的问题：

```math
\begin{array}{ll}
\text{minimize}   & c^T x \\
\text{subject to} & 0 \preceq x \preceq 1.
\end{array}
```

<span style="color:#2471a3;">**[question]**</span> what is the solution?（解是什么？）

> <span style="color:#2471a3;">**[solution]**</span> 答案（answer）：若 $c_i > 0$，则 $x_i^\star = 0$；若 $c_i < 0$，则 $x_i^\star = 1$；若 $c_i = 0$，则 $0 \le x_i^\star \le 1$。

我们也可以换一个视角来看：把对偶变量（dual variables）$\lambda$ 关联到约束 $0 \preceq x$，把对偶变量 $\mu$ 关联到约束 $x \preceq 1$。拉格朗日函数（Lagrangian）为

```math
L(x, \lambda, \mu) = c^T x - \lambda^T x + \mu^T (x - \mathbf{1}).
```

它有下界（bounded）当且仅当 $c = \lambda - \mu$。任何对偶最优点 $(\lambda^\star, \mu^\star)$ 都必须满足 $c = \lambda^\star - \mu^\star$。

<!-- p.5 -->

<span style="color:#2471a3;">**[question]**</span> suppose $c_i > 0$, what can we say about $\lambda_i^\star x_i^\star$ and $\mu_i^\star$?（设 $c_i > 0$，关于 $\lambda_i^\star x_i^\star$ 与 $\mu_i^\star$ 能说出什么？）

> <span style="color:#2471a3;">**[solution]**</span> 答案（answer）：此时 $\lambda_i^\star > 0$，于是由互补松弛得 $x_i^\star = 0$，进而 $\mu_i^\star = 0$。

<span style="color:#2471a3;">**[question]**</span> suppose $c_i < 0$, what can we say about $\lambda_i^\star x_i^\star$ and $\mu_i^\star$?（设 $c_i < 0$ 呢？）

> <span style="color:#2471a3;">**[solution]**</span> 答案（answer）：此时 $\mu_i^\star > 0$，于是由互补松弛得 $x_i^\star = 1$，进而 $\lambda_i^\star = 0$。

<span style="color:#2471a3;">**[question]**</span> what about when $c_i = 0$?（那 $c_i = 0$ 时呢？）

> <span style="color:#2471a3;">**[solution]**</span> 答案（answer）：此时 $\lambda_i^\star - \mu_i^\star = 0$。但 $\lambda_i^\star > 0$ 与 $\mu_i^\star > 0$ 不能同时成立，否则会同时要求 $x_i = 0$ 与 $x_i = 1$。因此必有 $\lambda_i^\star = 0$ 且 $\mu_i^\star = 0$。

<span style="color:#2471a3;">**[question]**</span> can we write down $\lambda^\star$ and $\mu^\star$?（能把 $\lambda^\star$ 与 $\mu^\star$ 写成显式表达式吗？）

> <span style="color:#2471a3;">**[solution]**</span> 答案（answer）：可以。$\lambda^\star$ 是 $c$ 的正部（positive part），$\mu^\star$ 是 $c$ 的负部（negative part），即 $\lambda^\star = \max(c, 0)$，$\mu^\star = \min(c, 0)$。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 本例的收尾其实藏着一个漂亮的结构：任意向量 $c$ 都可以写成 $c = \lambda^\star - \mu^\star$，其中 $\lambda^\star = \max(c, 0)$ 是正部、$\mu^\star = \min(c, 0)$ 是负部，两者逐分量不同时非零，因此内积 $\lambda^{\star T} \mu^\star = 0$——这正是互补松弛的向量形式。用 Strang 线代的眼光看，这相当于把 $c$ 拆到两个互不相交的支撑集上，与正交分解的思想一脉相承：只不过这里不是按子空间拆，而是按锥的方向拆。这个分解也绝不只是解题技巧：L1 正则化（regularization）中的软阈值算子、以及后续课程中的近端方法，本质上都是"取正部、截断负部"一类操作的变体。在这个小例子里，对偶变量不再是抽象的影子价格，而是显式等于目标向量经过简单逐分量运算的结果，非常适合作为建立对偶直觉的第一站。

<!-- p.6 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**拉格朗日乘子与约束灵敏度 Lagrange multipliers as sensitivities**</span>

拉格朗日乘子（Lagrange multipliers）$\lambda$ 与 $\nu$ 同时也是约束在最优点处活跃程度的度量。例如，把第一条约束放松为 $-u_1 \le x_1$，则有

```math
\lambda_1^\star = -\left. \frac{\partial p^\star}{\partial u_1} \right|_{u_1 = 0}.
```

也就是说，最优拉格朗日乘子是最优值（optimal value）对约束扰动（perturbation）的局部灵敏度（local sensitivities）。

<!-- p.7 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**原问题与对偶问题的最优点 Primal and dual optimal points**</span>

假设强对偶性成立，即 $p^\star = d^\star$，且 $p^\star$ 与 $d^\star$ 都能达到（achieved）。

什么时候我们能从对偶最优点 $(\lambda^\star, \nu^\star)$ 出发找出原问题最优点 $x^\star$？考虑在对偶最优点处取值的拉格朗日函数：

```math
L(x, \lambda^\star, \nu^\star) = f_0(x) + \sum_{i=1}^{m} \lambda_i^\star f_i(x) + \sum_{i=1}^{p} \nu_i^\star h_i(x).
```

假设它的最小化点（minimizer）$\tilde{x}$ 唯一。由于 $x^\star$ 也最小化 $L(x, \lambda^\star, \nu^\star)$，故必有 $\tilde{x} = x^\star$。

<!-- p.8 -->

### <span style="color:#2471a3;">**[example]**</span> <span style="color:#c0392b;">**例：多面体上的最小范数解 Example: least-norm solution over a polyhedron**</span>

考虑问题

```math
\begin{array}{ll}
\text{minimize}   & x^T x \\
\text{subject to} & Ax \preceq b,
\end{array}
```

其中 $x \in \mathbb{R}^{1000}$，$A \in \mathbb{R}^{10 \times 1000}$，并假设 $A$ 满秩（full rank）。拉格朗日函数为

```math
L(x, \lambda) = x^T x + \lambda^T (Ax - b),
```

其最小化点为 $x = -(1/2) A^T \lambda$。对偶问题可写为

```math
\begin{array}{ll}
\text{maximize}   & -(1/4)\, \lambda^T A A^T \lambda - b^T \lambda \\
\text{subject to} & \lambda \succeq 0.
\end{array}
```

<span style="color:#2471a3;">**[question]**</span> does strong duality hold?（强对偶性成立吗？）

> <span style="color:#2471a3;">**[solution]**</span> 答案（answer）：成立，因为我们可以求解 $Ax = b$。

<!-- p.9 -->

<span style="color:#2471a3;">**[question]**</span> suppose $\lambda^\star$ is optimal for the dual problem, can we find a primal optimal point? and if so, is it unique?（设 $\lambda^\star$ 是对偶问题的最优点，我们能找出原问题的最优点吗？若能，它唯一吗？）

> <span style="color:#2471a3;">**[solution]**</span> 答案（answer）：能。$\tilde{x} = -(1/2) A^T \lambda^\star$ 最小化拉格朗日函数，而 $\tilde{x}$ 唯一，故 $\tilde{x} = x^\star$。

- 就上面这个例子而言，给定对偶最优点 $\lambda^\star$，我们就能唯一地构造出原问题最优点 $x^\star$。

<span style="color:#2471a3;">**[question]**</span> how many variables does the primal problem have?（原问题有多少个变量？）

> <span style="color:#2471a3;">**[solution]**</span> 答案（answer）：1000 个。

<span style="color:#2471a3;">**[question]**</span> how many variables does the dual problem have?（对偶问题有多少个变量？）

> <span style="color:#2471a3;">**[solution]**</span> 答案（answer）：10 个。

- 这是"先解对偶问题、再从对偶解构造原问题解"的一大优势。不过我们稍后会看到，如果充分挖掘原问题自身的结构，这两个问题的求解时间其实大致相同。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 为什么"能解 $Ax = b$"就意味着强对偶性？钥匙是 Slater 条件（Slater's condition）：对不等式约束 $Ax \preceq b$，只要存在严格可行点（$Ax \prec b$），强对偶性就成立。用 Strang 线代的语言说，$A \in \mathbb{R}^{10 \times 1000}$ 满行秩意味着 $A$ 的行空间张满 $\mathbb{R}^{10}$，于是 $Ax$ 可以取到任意的目标值：想严格可行，只需解 $Ax = b - \epsilon \mathbf{1}$（$\epsilon > 0$），立即得到 $Ax \prec b$。这个例子还展示了"维数杠杆"：原问题有 1000 个变量，对偶问题只有 10 个变量，先解对偶、再反推原解看似是巨大的节省。但讲义也预告了一个重要提醒：如果充分挖掘原问题自身的结构（例如按稀疏模式选择高效的 Cholesky 分解），两个问题的实际求解时间大致相当——决定快慢的从来不是变量个数，而是每次迭代的代价与可利用的结构。

<!-- p.10 -->

### <span style="color:#2471a3;">**[example]**</span> <span style="color:#c0392b;">**例：线性规划 Example: linear program**</span>

现在考虑问题

```math
\begin{array}{ll}
\text{minimize}   & c^T x \\
\text{subject to} & Ax \preceq b,
\end{array}
```

其中 $A$ 是"瘦"的（skinny，行数远多于列数）且满秩。同时假设强对偶性成立，且 $p^\star$ 与 $d^\star$ 都能达到。拉格朗日函数为

```math
L(x, \lambda) = c^T x + \lambda^T (Ax - b).
```

它有下界当且仅当 $A^T \lambda + c = 0$。对偶问题可写为

```math
\begin{array}{ll}
\text{maximize}   & -b^T \lambda \\
\text{subject to} & A^T \lambda + c = 0, \\
                  & \lambda \succeq 0.
\end{array}
```

<!-- p.11 -->

<span style="color:#2471a3;">**[question]**</span> can we find a primal optimal point $x^\star$ from a dual optimal point $\lambda^\star$ by minimizing the Lagrangian?（能通过最小化拉格朗日函数，从对偶最优点 $\lambda^\star$ 得到原问题最优点 $x^\star$ 吗？）

> <span style="color:#2471a3;">**[solution]**</span> 答案（answer）：不能。此时 $L(x, \lambda^\star) = -b^T \lambda^\star$：任何 $x$ 都最小化 $L(x, \lambda^\star)$，但并非每个 $x$ 都是原问题的最优点。

<span style="color:#2471a3;">**[question]**</span> can we do this by any other method?（能用其他方法做到吗？）

> <span style="color:#2471a3;">**[solution]**</span> 答案（answer）：可以借助互补松弛：若 $\lambda_i^\star > 0$，则必有 $a_i^T x^\star = b_i$。因此，若由此得到的线性方程组解唯一，它就是原问题的一个最优点。

- 并非总能从对偶最优点构造出原问题最优点——即使强对偶性成立也是如此。

<!-- p.12 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**择一定理 Theorems of alternatives**</span>

我们想判定下面这个（不一定凸的）线性不等式与等式系统的可行性（feasibility）：

```math
f_i(x) \le 0, \quad i = 1, \ldots, m, \qquad h_i(x) = 0, \quad i = 1, \ldots, p. \qquad (1)
```

假设定义域（domain）$D = \bigcap_{i=1}^{m} \mathrm{dom}\, f_i \cap \bigcap_{i=1}^{p} \mathrm{dom}\, h_i$ 非空。

我们可以把这个判定问题写成

```math
\begin{array}{ll}
\text{minimize}   & 0 \\
\text{subject to} & f_i(x) \le 0, \quad i = 1, \ldots, m, \\
                  & h_i(x) = 0, \quad i = 1, \ldots, p,
\end{array}
```

并有

```math
p^\star =
\begin{cases}
0,       & \text{if (1) is feasible}, \\
+\infty, & \text{if (1) is infeasible}.
\end{cases}
```

（即：若系统 (1) 可行，则 $p^\star = 0$；若 (1) 不可行，则 $p^\star = +\infty$。）

<!-- p.13 -->

对偶函数（dual function）为

```math
g(\lambda, \nu) = \inf_{x \in D} \left( \sum_{i=1}^{m} \lambda_i f_i(x) + \sum_{i=1}^{p} \nu_i h_i(x) \right).
```

<span style="color:#2471a3;">**[question]**</span> what happens if there exist $\lambda \succeq 0$ and $\nu$ for which $g(\lambda, \nu) > 0$?（若存在 $\lambda \succeq 0$ 与 $\nu$ 使得 $g(\lambda, \nu) > 0$，会发生什么？）

> <span style="color:#2471a3;">**[solution]**</span> 答案（answer）：$d^\star = +\infty$。（对偶函数关于 $(\lambda, \nu)$ 是正齐次的：把这样的点乘以任意大的正数，$g$ 值随之任意增大。）

<span style="color:#2471a3;">**[question]**</span> what happens if there does not exist $\lambda \succeq 0$ and $\nu$ for which $g(\lambda, \nu) > 0$?（若不存在这样的 $\lambda \succeq 0$ 与 $\nu$ 呢？）

> <span style="color:#2471a3;">**[solution]**</span> 答案（answer）：$d^\star = 0$。

<!-- p.14 -->

于是

```math
d^\star =
\begin{cases}
+\infty, & \text{if } \lambda \succeq 0,\ g(\lambda, \nu) > 0 \text{ is feasible}, \\
0,       & \text{if } \lambda \succeq 0,\ g(\lambda, \nu) > 0 \text{ is infeasible}.
\end{cases}
```

（即：若系统 "$\lambda \succeq 0$，$g(\lambda, \nu) > 0$" 可行，则 $d^\star = +\infty$；若它不可行，则 $d^\star = 0$。）

回忆弱对偶性（weak duality）：$d^\star \le p^\star$。若 (1) 可行，则 $p^\star = 0$，从而 $d^\star = 0$，这意味着系统

```math
\lambda \succeq 0, \qquad g(\lambda, \nu) > 0. \qquad (2)
```

不可行。反之，若 (2) 可行，则 $d^\star = +\infty$，这意味着 $p^\star = +\infty$，从而 (1) 不可行。注意：两个系统可能同时不可行。

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">(1) 与 (2) 称为一对**弱择一**（weak alternatives）系统：两者之中至多有一个可行。</span>

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">若两个系统之中**恰好**有一个成立，则称它们互为**强择一**（strong alternatives）。</span>

<!-- p.15 -->

对择一定理来说，不等式组是否取"严格"不等号是有区别的。例如，若考虑系统

```math
f_i(x) < 0, \quad i = 1, \ldots, m, \qquad h_i(x) = 0, \quad i = 1, \ldots, p. \qquad (3)
```

那么 (2) 就不再是 (3) 的弱择一系统。此时弱择一系统变为

```math
\lambda \succeq 0, \qquad \lambda \ne 0, \qquad g(\lambda, \nu) \ge 0. \qquad (4)
```

<!-- p.16 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Farkas 引理 Farkas' lemma**</span>

不等式系统

```math
Ax \preceq 0, \qquad c^T x < 0
```

（其中 $A \in \mathbb{R}^{m \times n}$，$c \in \mathbb{R}^n$）与不等式系统

```math
A^T y + c = 0, \qquad y \succeq 0
```

是一对强择一系统（strong alternatives）。

<span style="color:#2471a3;">**[question]**</span> how can we show this?（如何证明这一点？）

<!-- p.17 -->

> <span style="color:#2471a3;">**[solution]**</span> 解答（solution）：使用 LP 对偶。考虑线性规划（linear program, LP）

```math
\begin{array}{ll}
\text{minimize}   & c^T x \\
\text{subject to} & Ax \preceq 0,
\end{array}
```

及其对偶问题

```math
\begin{array}{ll}
\text{maximize}   & 0 \\
\text{subject to} & A^T y + c = 0, \\
                  & y \succeq 0.
\end{array}
```

原 LP 是齐次的（homogeneous），因此：若原不等式系统不可行，则最优值为 $0$；若原不等式系统可行，则最优值为 $-\infty$。对偶问题在对手不等式系统可行时最优值为 $0$，在对手不等式系统不可行时最优值为 $-\infty$。由于 $p^\star = d^\star$（为什么？），这两个系统互为强择一。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> Farkas 引理（Gyula Farkas，1902 年发表）是凸分析中最古老的择一定理，几何上它是分离超平面（separating hyperplane）定理的代数化身：$Ax \preceq 0$ 刻画一个以原点为顶点的多面体锥，$c^T x < 0$ 问的是"锥里是否存在让目标严格为负的点"；而 $A^T y + c = 0,\ y \succeq 0$ 恰好等价于 $-c$ 落在 $A$ 的列生成的锥里。两种情形非此即彼、不可兼得，"分离"因此是穷尽的。你在 Strang 线代里熟悉的"向量与子空间"的关系，在这里升级为"向量与锥"的关系：$c$ 要么能被锥表示出来，要么能被锥严格分开。顺带回应原文那声"why？"：原 LP 恒有可行点 $x = 0$（不会出现"两边都不可行"的退化情形），故 LP 的强对偶性给出 $p^\star = d^\star$。择一定理也是 6.006 中最短路、网络流等模型可行性论证的代数根基。

<!-- p.18 -->

### <span style="color:#2471a3;">**[example]**</span> <span style="color:#c0392b;">**应用：套利 Application: arbitrage**</span>

考虑 $n$ 种资产（asset），价格依次为 $p_1, p_2, \ldots, p_n$；在投资期结束时，资产的价值为 $v_1, v_2, \ldots, v_n$。$x_1, x_2, \ldots, x_n$ 表示在各资产上的初始投资额（$x_j < 0$ 表示我们欠 $-x_j$ 数量的资产 $j$，即持有空头）。

- 初始投资的成本为 $p^T x$，投资的最终价值为 $v^T x$。
- $v$ 是不确定的：共有 $m$ 种可能场景（scenario），记为 $v^{(1)}, \ldots, v^{(m)}$。若场景 $i$ 发生，投资的最终价值就是 $v^{(i)T} x$。
- <span style="color:#00838f;">若存在投资组合 $x$ 满足 $p^T x < 0$，且对 $i = 1, \ldots, m$ 都有 $v^{(i)T} x \ge 0$，则称存在**套利**（arbitrage）。</span>

<!-- p.19 -->

在金融中，通常假设不存在套利（no arbitrage）。这意味着不等式系统

```math
V x \succeq 0, \qquad p^T x < 0
```

不可行，其中 $V$ 是以 $v^{(1)T}, \ldots, v^{(m)T}$ 为各行的矩阵。

由 Farkas 引理，上述系统不可行当且仅当存在 $y$ 使得

```math
V^T y = p, \qquad y \succeq 0.
```

<span style="color:#2471a3;">**[question]**</span> suppose that $V$ is known, and all the prices except the last price $p_n$ are known. we wish to find the set of prices $p_n$ that are consistent with the no-arbitrage assumption. what kind of set is this?（设 $V$ 已知，且除最后一个价格 $p_n$ 外的所有价格都已知。我们想求出与无套利假设相容的价格 $p_n$ 的集合。这是一个什么样的集合？）

> <span style="color:#2471a3;">**[solution]**</span> 解答（solution）：显然必须有 $p \succeq 0$，这与直觉一致。因此该集合必是一个区间（它是多面体（polyhedron）到 $\mathbb{R}$ 上的投影（projection））。我们可以通过求解一对 LP 来找出这个区间。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这页的结论在金融里有个更贴近概率论的名字：状态价格（state prices）。$y \succeq 0$ 且 $V^T y = p$ 说明今天的价格向量 $p$ 可以写成各场景终值向量 $v^{(i)}$ 的非负线性组合，权重 $y_i$ 就是每个场景的"单价"；若把 $y$ 归一化（除以 $\mathbf{1}^T y$），它就变成一个概率分布——这正是概率论中风险中性测度（risk-neutral measure）思想的雏形：在该测度下，资产现价等于按场景加权的期望终值（期望定价）。于是，"无套利"与"存在状态价格"是同一事实的两种表述，Farkas 引理正是把这条金融直觉翻译成线性不等式语言的那座桥。最后的区间结论也不要放过：与无套利相容的价格集合是多面体在 $\mathbb{R}$ 上的投影，而线性映射的像保持凸性，凸集在一维上自然是区间——"投影保凸"是本课程反复出现的手法，后面做半定规划（SDP）消除变量时还会用到。

<!-- p.20 -->

为求 $p_n$ 的最小值，我们求解

```math
\begin{array}{ll}
\text{minimize}   & p_n \\
\text{subject to} & V^T y = p, \\
                  & y \succeq 0.
\end{array}
```

而为求 $p_n$ 的最大值，我们解同一个 LP，但改为最大化。
