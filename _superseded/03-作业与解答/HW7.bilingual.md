# EE364a HW7 解答（作业 7 解答）

> 本文为 Stanford EE364a（Convex Optimization，Winter 2007-08）Homework 7 官方解答的双语译注版。中文为主体，英文术语首次出现时以 `English（中文）` 行内标注，之后直接用 `English`。公式使用行内 `$...$` 或独立 ```math 围栏。

---

## 题目 1（Problem 1）—— 8.16 多面体内的最大体积矩形

**题干：** Maximum volume rectangle inside a polyhedron. Formulate the following problem as a convex optimization problem. Find the rectangle

```math
R = \{ x \in \mathbb{R}^n \mid l \preceq x \preceq u \}
```

of maximum volume, enclosed in a polyhedron $P = \{ x \mid Ax \preceq b \}$. The variables are $l, u \in \mathbb{R}^n$. Your formulation should not involve an exponential number of constraints.

**中文：** 求多面体（polyhedron）内的最大体积矩形（rectangle）。把下面的问题表述成一个凸优化问题（convex optimization problem）。要找到矩形 $R = \{ x \in \mathbb{R}^n \mid l \preceq x \preceq u \}$，使其体积最大，且被包含在多面体 $P = \{ x \mid Ax \preceq b \}$ 内。变量（variable）是 $l, u \in \mathbb{R}^n$。你的表述不能含有指数级数量的约束（constraint）。

**解答：**

**第一步：指出朴素方法的低效，并说明可绕开。**

一种直接但非常低效的做法，是用 $m \cdot 2^n$ 个不等式 $A v_i \preceq b$ 来表达约束 $R \subseteq P$，其中 $v_i$ 是矩形 $R$ 的 $2^n$ 个顶点（corner / vertex）。其依据是：如果一个盒子的所有顶点都落在多面体内部，那么整个盒子也落在内部。

（说明）设矩形的顶点集合为 $\{v_i\}_{i=1}^{2^n}$，每个顶点 $v_i$ 的分量取 $l_j$ 或 $u_j$。多面体 $P = \{ x \mid Ax \preceq b \}$ 是凸集，而矩形 $R$ 是这 $2^n$ 个顶点的凸包（convex hull）：

```math
R = \operatorname{conv}\{v_1, \dots, v_{2^n}\}.
```

因此 $R \subseteq P$ 当且仅当所有 $v_i \in P$，即 $A v_i \preceq b$ 对所有 $i$ 成立。这正是 $m \cdot 2^n$ 个不等式，约束数量随 $n$ 指数增长，不可接受。

**第二步：构造等价的线性约束（每行只用 $O(1)$ 个量）。**

定义矩阵 $A$ 的正部（positive part）与负部（negative part）：

```math
a^{+}_{ij} = \max\{a_{ij}, 0\}, \qquad a^{-}_{ij} = \max\{-a_{ij}, 0\}.
```

（含义）对每个元素，$a^{+}_{ij}$ 保留其正部分、$a^{-}_{ij}$ 保留其绝对值中的负部分，故 $a_{ij} = a^{+}_{ij} - a^{-}_{ij}$ 且 $a^{+}_{ij}, a^{-}_{ij} \ge 0$。

**第三步：证明 $R \subseteq P$ 的充要条件。**

**充分性（sufficiency）：** 设约束 $\sum_{i=1}^{n}(a^{+}_{ij}u_j - a^{-}_{ij}l_j) \le b_i$ 对所有 $i$ 成立。对任意 $x \in R$，有 $l_j \le x_j \le u_j$。因为 $a^{+}_{ij} \ge 0$，故 $a^{+}_{ij} x_j \le a^{+}_{ij} u_j$；因为 $-a^{-}_{ij} \le 0$，且 $x_j \ge l_j$，故 $-a^{-}_{ij} x_j \le -a^{-}_{ij} l_j$（两边同乘负数方向要反过来，等价地 $a^{-}_{ij} x_j \ge a^{-}_{ij} l_j$）。于是

```math
a_{ij} x_j = (a^{+}_{ij} - a^{-}_{ij}) x_j \le a^{+}_{ij} u_j - a^{-}_{ij} l_j.
```

对 $j$ 求和，得

```math
(Ax)_i = \sum_{j=1}^{n} a_{ij} x_j \le \sum_{j=1}^{n}(a^{+}_{ij} u_j - a^{-}_{ij} l_j) \le b_i.
```

这证明 $Ax \preceq b$ 对所有 $x \in R$ 成立，即 $R \subseteq P$。

**必要性（necessity）：** 若 $R \subseteq P$，则特别地，构造一个特定的顶点 $\hat{v}$，其第 $j$ 个分量按 $a_{ij}$ 的符号选取：

```math
\hat{v}_j = \begin{cases} u_j, & a_{ij} \ge 0,\\ l_j, & a_{ij} < 0. \end{cases}
```

由 $R \subseteq P$ 知 $A\hat{v} \preceq b$，其第 $i$ 行正是

```math
\sum_{j: a_{ij} \ge 0} a_{ij} u_j + \sum_{j: a_{ij} < 0} a_{ij} l_j
= \sum_{j=1}^{n} (a^{+}_{ij} u_j - a^{-}_{ij} l_j) \le b_i.
```

故该约束成立。

（结论）因此 $R \subseteq P$ 的充要条件就是这 $m$ 个线性不等式：

```math
\sum_{j=1}^{n}(a^{+}_{ij}u_j - a^{-}_{ij}l_j) \le b_i, \qquad i = 1,\dots,m,
```

约束数量从 $m \cdot 2^n$ 降到 $m$。

**第四步：写出最大化体积的凸问题。**

矩形 $R$ 的体积为 $\prod_{i=1}^{n}(u_i - l_i)$。直接最大化体积的几何平均（geometric mean）$(\prod_{i=1}^{n}(u_i - l_i))^{1/n}$ 是一个凹最大化问题（concave maximization，等价于凸优化），其形式为：

```math
\begin{aligned}
& \text{maximize} && \left(\prod_{i=1}^{n}(u_i - l_i)\right)^{1/n} \\
& \text{subject to} && \sum_{j=1}^{n}(a^{+}_{ij}u_j - a^{-}_{ij}l_j) \le b_i, \quad i=1,\dots,m,
\end{aligned}
```

并含隐式约束 $u \succeq l$（否则 $u_i - l_i < 0$，体积无意义）。

（关键判据）目标函数 $\left(\prod_i (u_i - l_i)\right)^{1/n}$ 是凹函数（concave function），因为它是对数凹（log-concave）函数的几何平均；约束是仿射的（affine），所以这是一个凸优化问题。

**第五步：取对数得到等价的更简洁形式。**

对目标取对数（log 是单调递增且是凹的复合，几何平均的 $\log$ 恰是凹函数 $\frac{1}{n}\sum_i \log(u_i - l_i)$），得到等价问题：

```math
\begin{aligned}
& \text{maximize} && \sum_{i=1}^{n} \log(u_i - l_i) \\
& \text{subject to} && \sum_{j=1}^{n}(a^{+}_{ij}u_j - a^{-}_{ij}l_j) \le b_i, \quad i=1,\dots,m.
\end{aligned}
```

这里目标 $\sum_i \log(u_i - l_i)$ 是凹函数（各项 $\log$ 均为凹），约束仍为仿射，故该问题也是凸优化问题，且形式更易处理。隐式约束 $u \succeq l$ 由 $\log$ 的定义域（domain）自然保证。

---

## 题目 2（Problem 2）—— 9.30 梯度法与牛顿法

**题干：** Gradient and Newton methods. Consider the unconstrained problem

```math
\text{minimize} \quad f(x) = -\sum_{i=1}^{m}\log(1 - a_i^T x) - \sum_{i=1}^{n}\log(1 - x_i^2),
```

with variable $x \in \mathbb{R}^n$, and $\operatorname{dom} f = \{ x \mid a_i^T x < 1,\ i=1,\dots,m,\ |x_i| < 1,\ i=1,\dots,n \}$. This is the problem of computing the analytic center of the set of linear inequalities $a_i^T x \le 1$, $i=1,\dots,m$, and $|x_i| \le 1$, $i=1,\dots,n$. Note that we can choose $x^{(0)} = 0$ as our initial point.

(a) Use the gradient method to solve the problem, using reasonable choices for the backtracking parameters, and a stopping criterion of the form $\|\nabla f(x)\|_2 \le \eta$. Plot the objective function and step length versus iteration number. Experiment with the backtracking parameters $\alpha$ and $\beta$.

(b) Repeat using Newton's method, with stopping criterion based on the Newton decrement $\lambda^2$. Look for quadratic convergence. You may use a general purpose dense solver, although it is better to use one based on a Cholesky factorization.

**中文：** 考虑无约束问题（unconstrained problem）

```math
\text{minimize} \quad f(x) = -\sum_{i=1}^{m}\log(1 - a_i^T x) - \sum_{i=1}^{n}\log(1 - x_i^2),
```

变量为 $x \in \mathbb{R}^n$，定义域 $\operatorname{dom} f = \{ x \mid a_i^T x < 1,\ i=1,\dots,m,\ |x_i| < 1,\ i=1,\dots,n \}$。这是计算线性不等式组 $a_i^T x \le 1$（$i=1,\dots,m$）与 $|x_i| \le 1$（$i=1,\dots,n$）的解析中心（analytic center）的问题。注意我们可以取 $x^{(0)} = 0$ 作为初始点。

(a) 用梯度法（gradient method）求解，使用合理的回溯（backtracking）参数，停止准则（stopping criterion）形如 $\|\nabla f(x)\|_2 \le \eta$。画出目标函数值与步长（step length）随迭代次数（iteration number）变化的曲线。实验不同的回溯参数 $\alpha$ 和 $\beta$ 对总迭代次数的影响。

(b) 改用牛顿法（Newton's method），停止准则基于牛顿减量（Newton decrement）$\lambda^2$。观察二次收敛（quadratic convergence）。不必用习题 9.27 里的高效牛顿步长算法，可以用通用稠密（dense）求解器，不过基于 Cholesky 分解（Cholesky factorization）的更好。

**解答：**

**第一步：用链式法则求梯度与 Hessian。**

先写出目标函数各项。令 $f(x) = -\sum_{i=1}^{m}\log(1 - a_i^T x) - \sum_{i=1}^{n}\log(1 - x_i^2)$。定义辅助向量

```math
d = \frac{1}{1 - Ax} \in \mathbb{R}^m,\qquad
```

即 $d_i = \frac{1}{1 - a_i^T x}$（分量除法）。

（第一项梯度）对 $-\log(1 - a_i^T x)$ 关于 $x$ 求梯度：

```math
\nabla_x \big[-\log(1 - a_i^T x)\big] = -\frac{1}{1 - a_i^T x}\cdot (-a_i) = \frac{a_i}{1 - a_i^T x}.
```

故第一项的梯度为 $\sum_{i=1}^{m} \frac{a_i}{1 - a_i^T x} = A^T d$，其中 $d = \frac{1}{1 - Ax}$。

（第二项梯度）对 $-\log(1 - x_i^2) = -\log(1 - x_i) - \log(1 + x_i)$，有

```math
\frac{\partial}{\partial x_i}\big[-\log(1 - x_i^2)\big]
= \frac{1}{1 - x_i} - \frac{1}{1 + x_i}.
```

（分量式可合并为 $\frac{2x_i}{1 - x_i^2}$，但逐项形式更便于写代码。）因此梯度为

```math
\nabla f(x) = A^T d - \frac{1}{1 + x} + \frac{1}{1 - x},
```

其中 $\frac{1}{1 \pm x}$ 表示逐分量（element-wise）除法，即 $\left(\frac{1}{1 \pm x}\right)_i = \frac{1}{1 \pm x_i}$。

（Hessian）对 $\nabla f(x)$ 再求导。第一项 $A^T d$ 中 $d_i = \frac{1}{1 - a_i^T x}$，故

```math
\frac{\partial d_i}{\partial x_j} = \frac{a_{ij}}{(1 - a_i^T x)^2} = a_{ij} d_i^2.
```

于是 $\nabla (A^T d) = A^T \operatorname{diag}(d^2) A$，其中 $\operatorname{diag}(d^2)$ 表示以 $d_i^2$ 为对角元的对角矩阵。第二项 $-\frac{1}{1+x} + \frac{1}{1-x}$ 是逐分量的，其 Jacobian（Jacobi 矩阵）是对角阵，对角元为

```math
\frac{1}{(1 + x_i)^2} + \frac{1}{(1 - x_i)^2}.
```

因此 Hessian 为

```math
\nabla^2 f(x) = A^T \operatorname{diag}(d^2) A + \operatorname{diag}\!\left(\frac{1}{(1 + x)^2} + \frac{1}{(1 - x)^2}\right).
```

（正定性判据）由于 $\operatorname{diag}(d^2)$ 的各对角元 $d_i^2 > 0$（在定义域内 $1 - a_i^T x > 0$）且第二个对角阵的各对角元 $\frac{1}{(1+x_i)^2} + \frac{1}{(1-x_i)^2} > 0$，因此 $\nabla^2 f(x) \succ 0$（正定，positive definite）。故 $f$ 在定义域上是严格凸的（strictly convex），极小点唯一，即解析中心。

**第二步：初始点与可行性。**

取 $x^{(0)} = 0$。此时 $a_i^T x^{(0)} = 0 < 1$ 且 $|x_i^{(0)}| = 0 < 1$，故 $x^{(0)} \in \operatorname{dom} f$，是可行初始点。

**第三步：梯度法（backtracking line search）。**

更新方向取负梯度 $v = -\nabla f(x)$。用回溯线搜索（backtracking line search）确定步长 $t$：先令 $t = 1$，只要试探点 $x + t v$ 不在定义域内（即 $\max(A(x+tv)) \ge 1$ 或 $\max|x+tv| \ge 1$）就收缩 $t \leftarrow \beta t$；再执行 Armijo 条件（Armijo condition）

```math
f(x + t v) \le f(x) + \alpha t \, \nabla f(x)^T v.
```

不满足则继续 $t \leftarrow \beta t$。方向导数 $f'(x) = \nabla f(x)^T v = -\|\nabla f(x)\|_2^2 < 0$，满足下降方向条件。停止准则为 $\|\nabla f(x^{(k)})\|_2 \le \eta$（示例取 $\eta = 10^{-3}$，$\alpha = 0.01$，$\beta = 0.5$）。

[[FIGURE: 第 2 页, 梯度法：左图为 $f(x^{(k)}) - p^\star$（半对数坐标）随迭代次数 $k$ 变化，约 500 次迭代从 $10^4$ 量级降到 $10^{-8}$；右图为步长 $t^{(k)}$ 随 $k$ 变化，步长多数落在 $0$ 到 $0.016$ 之间]]

（收敛特性）梯度法对 $m=200, n=100$ 的实例需要约 500 次迭代才能把 $f - p^\star$ 降到 $10^{-8}$，呈近似线性收敛（linear convergence），步长普遍很小。

**第四步：牛顿法（Newton method）。**

牛顿步为求解线性系统

```math
\nabla^2 f(x)\, v = -\nabla f(x),
```

即 $v = -H^{-1} \nabla f(x)$，其中 $H = \nabla^2 f(x)$。由于 $H \succ 0$，可用 Cholesky 分解 $H = LL^T$ 求解，先前代（forward substitution）$L y = -\nabla f(x)$，再回代（back substitution）$L^T v = y$。停止准则用牛顿减量平方 $\lambda^2$（示例取 $\lambda^2 \le 10^{-8}$），其中

```math
\lambda^2 = \nabla f(x)^T \nabla^2 f(x)^{-1} \nabla f(x) = -\nabla f(x)^T v.
```

（收敛性判据）因 $f$ 是自和谐（self-concordant）的（由 log-barrier 函数组成），牛顿法在局部区域具有二次收敛（quadratic convergence），即误差满足 $f(x^{(k)}) - p^\star \le \lambda(x^{(k)})^2$，每次迭代 $\lambda^2$ 近似平方级下降。

[[FIGURE: 第 4 页, 牛顿法：左图为 $f(x^{(k)}) - p^\star$（半对数）随 $k$ 变化，仅约 6–7 次迭代便从 $10^5$ 量级骤降到 $10^{-10}$，呈典型二次收敛；右图为步长 $t^{(k)}$ 随 $k$ 变化，后期步长收敛到 1]]

**第五步：关键公式小结（供编程）。**

```math
\begin{aligned}
d &= \frac{1}{1 - Ax},\\
\nabla f(x) &= A^T d - \frac{1}{1+x} + \frac{1}{1-x},\\
\nabla^2 f(x) &= A^T \operatorname{diag}(d^2) A + \operatorname{diag}\!\left(\frac{1}{(1+x)^2} + \frac{1}{(1-x)^2}\right).
\end{aligned}
```

---

## 题目 3（Problem 3）—— 9.31 若干近似牛顿法

**题干：** Some approximate Newton methods. The cost of Newton's method is dominated by the cost of evaluating the Hessian $\nabla^2 f(x)$ and the cost of solving the Newton system. For large problems, it is sometimes useful to replace the Hessian by a positive definite approximation that makes it easier to form and solve for the search step. For each of the approximate Newton methods described below, test the method on some instances of the analytic centering problem described in exercise 9.30, and compare the results to those obtained using the Newton method and gradient method.

(a) Re-using the Hessian. We evaluate and factor the Hessian only every $N$ iterations, where $N > 1$, and use the search step $\Delta x = -H^{-1}\nabla f(x)$, where $H$ is the last Hessian evaluated.

(b) Diagonal approximation. We replace the Hessian by its diagonal, so we only have to evaluate the $n$ second derivatives $\partial^2 f(x)/\partial x_i^2$, and computing the search step is very easy.

**中文：** 若干近似牛顿法（approximate Newton method）。牛顿法的成本主要由计算 Hessian $\nabla^2 f(x)$ 与求解牛顿系统（Newton system）的成本主导。对于大规模问题，有时把 Hessian 换成一个正定近似（positive definite approximation），使搜索步（search step）更容易形成和求解，是有用的。对下面每种近似牛顿法，在习题 9.30 描述的解析中心问题的一些实例上测试，并与牛顿法和梯度法的结果比较。

(a) 重用 Hessian：每隔 $N$ 次迭代（$N > 1$）才计算并分解一次 Hessian，其余迭代用最近一次算出的 $H$ 构成搜索步 $\Delta x = -H^{-1}\nabla f(x)$。

(b) 对角近似：把 Hessian 换成它的对角线，于是只需计算 $n$ 个二阶导数 $\partial^2 f(x)/\partial x_i^2$，计算搜索步非常容易。

**解答：**

**第一步：问题背景与 Hessian 结构。**

沿用 9.30 的解析中心问题，其 Hessian（见上题第五步）为

```math
\nabla^2 f(x) = A^T \operatorname{diag}(d^2) A + D, \qquad D = \operatorname{diag}\!\left(\frac{1}{(1+x)^2} + \frac{1}{(1-x)^2}\right),
```

其中 $D$ 是对角且正定的。计算 $A^T \operatorname{diag}(d^2) A$ 的代价为 $O(m n^2)$，对其做 Cholesky 分解代价约 $\tfrac13 n^3$ flop（浮点运算，floating-point operation），而每次前代/回代的代价约 $2 n^2$ flop。这些成本正是近似方法的优化对象。

**(a) 重用 Hessian（re-using the Hessian）。**

**思路：** 每隔 $N$ 步才重算 $H$ 并做 Cholesky 分解 $H = L L^T$；在中间迭代沿用旧的 $L$，只用前代/回代 $v = -L^{-T}(L^{-1}\nabla f(x))$ 求方向。这样把昂贵的分解平摊（amortize）到 $N$ 次迭代上。

**实现要点（伪代码逻辑）：** 当 $\mathrm{rem}(\text{iter}-1, N) = 0$ 时重算 $H$ 与 $L = \operatorname{chol}(H, \text{'lower'})$，计入 $\tfrac13 n^3$ flop；否则分解成本计为 $0$。每次求解方向 $v = -L' \setminus (L \setminus g)$，计入 $2n^2$ flop。其余（线搜索、域检查）与牛顿法相同。

**结果与结论：** [[FIGURE: 第 6 页, 横轴为累计 flop 数（约 $0$ 到 $2.5\times10^6$），纵轴为 $f(x^{(k)})-p^\star$（半对数），对比 $N=1$（纯牛顿）、$N=15$、$N=30$ 三条曲线。曲线显示：以真实计算量（flop）衡量，$N=15$、$N=30$ 比 $N=1$ 收敛更快]]

（结论）若以真实工作量（flop 数）衡量收敛速度，用一份分解后的 Hessian 连续用多步能加快收敛；但若只看迭代次数，该方法比基本牛顿法差（因为每步方向更不准确，需要更多步）。

**(b) 对角近似（diagonal approximation）。**

**思路：** 用 $H_{\text{diag}} = \operatorname{diag}(\nabla^2 f(x))$ 代替完整 Hessian，即只取对角线上的 $n$ 个二阶导：

```math
(H_{\text{diag}})_{ii} = \frac{\partial^2 f(x)}{\partial x_i^2} = (A^T \operatorname{diag}(d^2) A)_{ii} + \frac{1}{(1+x_i)^2} + \frac{1}{(1-x_i)^2}.
```

搜索步为 $v = -H_{\text{diag}}^{-1} \nabla f(x)$，因为 $H_{\text{diag}}$ 是对角阵，求逆只需 $n$ 次除法，代价极低。

**结果与结论：** [[FIGURE: 第 8 页, 横轴为迭代次数 $k$（约 $0$ 到 $800$），纵轴为 $f(x^{(k)})-p^\star$（半对数），曲线显示收敛极慢，约需 800 次迭代才降到 $10^{-8}$]]

（结论）对角近似的收敛行为与梯度法非常接近——因为它丢弃了 Hessian 的全部非对角信息，只保留了对角缩放，本质上等价于一种按坐标缩放的梯度法，不具备牛顿法的二次收敛，属于线性收敛。

（对比小结）三种方法在"真实 flop"维度上的优劣为：周期性重用 Hessian（$N=15,30$）优于纯牛顿法（$N=1$），而对角近似最差，退化为梯度法量级的收敛速度。这印证了 Hessian 的曲率信息对收敛速度的关键作用。

---

## 题目 4（Problem 4）—— 附加题 1：三路线性分类

**题干：** Three-way linear classification. We are given data $x^{(1)},\dots,x^{(N)}$, $y^{(1)},\dots,y^{(M)}$, $z^{(1)},\dots,z^{(P)}$, three nonempty sets of vectors in $\mathbb{R}^n$. We wish to find three affine functions on $\mathbb{R}^n$,

```math
f_i(z) = a_i^T z - b_i, \qquad i = 1,2,3,
```

that satisfy

```math
\begin{aligned}
f_1(x^{(j)}) &> \max\{f_2(x^{(j)}), f_3(x^{(j)})\}, \quad j=1,\dots,N,\\
f_2(y^{(j)}) &> \max\{f_1(y^{(j)}), f_3(y^{(j)})\}, \quad j=1,\dots,M,\\
f_3(z^{(j)}) &> \max\{f_1(z^{(j)}), f_2(z^{(j)})\}, \quad j=1,\dots,P.
\end{aligned}
```

In words: $f_1$ is the largest of the three functions on the $x$ data points, $f_2$ is the largest on the $y$ data points, $f_3$ is the largest on the $z$ data points. The functions partition $\mathbb{R}^n$ into three regions $R_1, R_2, R_3$ defined by where each function is the largest. Pose this as a convex optimization problem. You may not use strict inequalities in your formulation. Solve the specific instance given in `sep3way_data.m`.

**中文：** 三路线性分类（three-way linear classification）。给定三组非空向量集 $x^{(1)},\dots,x^{(N)}$、$y^{(1)},\dots,y^{(M)}$、$z^{(1)},\dots,z^{(P)}$（均在 $\mathbb{R}^n$ 中）。我们希望找 $\mathbb{R}^n$ 上的三个仿射函数（affine function）$f_i(z) = a_i^T z - b_i$（$i=1,2,3$），满足上述不等式。换言之：$f_1$ 在 $x$ 数据点上最大，$f_2$ 在 $y$ 数据点上最大，$f_3$ 在 $z$ 数据点上最大。这三个函数把 $\mathbb{R}^n$ 划分成三个区域（region）$R_1, R_2, R_3$，每个区域对应"哪个函数最大"。把这个问题表述成凸优化问题。你的表述中不能使用严格不等式（strict inequality）。求解 `sep3way_data.m` 中给出的具体实例。

**解答：**

**第一步：把严格不等式转成齐次可缩放的非严格形式。**

原始约束（如 $f_1(x^{(j)}) > \max\{f_2(x^{(j)}), f_3(x^{(j)})\}$）是关于 $a_i, b_i$ 齐次的（homogeneous）——若 $(a_i, b_i)$ 满足，则对任意 $\kappa > 0$，$(\kappa a_i, \kappa b_i)$ 也满足。因此可把"严格大于 $0$ 的裕度"归一化（normalize）为至少 $1$，把严格不等式换成非严格不等式：

```math
\begin{aligned}
f_1(x^{(j)}) &\ge \max\{f_2(x^{(j)}), f_3(x^{(j)})\} + 1, \quad j=1,\dots,N,\\
f_2(y^{(j)}) &\ge \max\{f_1(y^{(j)}), f_3(y^{(j)})\} + 1, \quad j=1,\dots,M,\\
f_3(z^{(j)}) &\ge \max\{f_1(z^{(j)}), f_2(z^{(j)})\} + 1, \quad j=1,\dots,P.
\end{aligned}
```

（说明为何可行）因为整体缩放自由度存在，若存在可行解满足严格不等式，则可放大其系数，使最小裕度 $\ge 1$，从而满足上述非严格版本；反之亦然。

**第二步：用平移自由度做规范化。**

注意到这些不等式只涉及 $a_i$ 之间的差与 $b_i$ 之间的差：若把所有 $a_i$ 同时加同一个向量 $\alpha$、把所有 $b_i$ 同时加同一个数 $\beta$，则每个差值 $f_p - f_q = (a_p - a_q)^T z - (b_p - b_q)$ 不变，不等式不受影响。因此可"不失一般性"（without loss of generality）加两个规范化约束：

```math
a_1 + a_2 + a_3 = 0, \qquad b_1 + b_2 + b_3 = 0.
```

这一步消去了平移引起的解的非唯一性，使问题成为良定（well-posed）的。

**第三步：写出凸优化问题。**

变量为 $a_1, a_2, a_3 \in \mathbb{R}^n$ 与 $b_1, b_2, b_3 \in \mathbb{R}$。每个约束 $\max\{f_2, f_3\}$ 是凸函数（有限个仿射函数的最大是凸的），而"仿射函数 $\ge$ 凸函数 $+1$"是凸约束。完整问题为

```math
\begin{aligned}
& \text{find} && a_1, a_2, a_3 \in \mathbb{R}^n,\ b_1, b_2, b_3 \in \mathbb{R} \\
& \text{subject to} && a_1^T x^{(j)} - b_1 \ge \max\{a_2^T x^{(j)} - b_2,\ a_3^T x^{(j)} - b_3\} + 1,\ j=1,\dots,N,\\
&&& a_2^T y^{(j)} - b_2 \ge \max\{a_1^T y^{(j)} - b_1,\ a_3^T y^{(j)} - b_3\} + 1,\ j=1,\dots,M,\\
&&& a_3^T z^{(j)} - b_3 \ge \max\{a_1^T z^{(j)} - b_1,\ a_2^T z^{(j)} - b_2\} + 1,\ j=1,\dots,P,\\
&&& a_1 + a_2 + a_3 = 0,\quad b_1 + b_2 + b_3 = 0.
\end{aligned}
```

（凸性判据）所有约束左侧为仿射函数，右侧为凸函数 $\max\{\cdot\}$ 加常数 $1$，形如"仿射 $\ge$ 凸"，是凸约束；等式约束为仿射，故整体是凸优化问题（严格说是一个凸可行性问题，feasibility problem）。

**第四步：CVX 实现（原文脚本）。**

```matlab
cvx_begin
variables a1(2) a2(2) a3(2) b1 b2 b3
a1'*X - b1 >= max(a2'*X - b2, a3'*X - b3) + 1;
a2'*Y - b2 >= max(a1'*Y - b1, a3'*Y - b3) + 1;
a3'*Z - b3 >= max(a1'*Z - b1, a2'*Z - b2) + 1;
a1 + a2 + a3 == 0;
b1 + b2 + b3 == 0;
cvx_end
```

其中 $X, Y, Z$ 的列分别为 $x^{(j)}, y^{(j)}, z^{(j)}$ 数据点（本题 $n=2$，$N=M=P=20$）。

**第五步：几何解释与图示。**

三个函数给出三条决策边界（decision boundary）：$f_1 = f_2$、$f_2 = f_3$、$f_3 = f_1$，即直线 $a_p^T z - b_p = a_q^T z - b_q$。三线交于一点（"最混淆点" maximally confusing point），由

```math
\begin{bmatrix} a_1 - a_2 \\ a_1 - a_3 \end{bmatrix} p = \begin{bmatrix} b_1 - b_2 \\ b_1 - b_3 \end{bmatrix}
```

解出。三个区域为

```math
R_1 = \{z \mid f_1(z) > \max\{f_2(z), f_3(z)\}\},\quad
R_2 = \{z \mid f_2(z) > \max\{f_1(z), f_3(z)\}\},\quad
R_3 = \{z \mid f_3(z) > \max\{f_1(z), f_2(z)\}\}.
```

[[FIGURE: 第 13 页, 三路分离结果：横纵坐标范围 $[-6,6]$，三类点分别以 `*`（X，右上）、`o`（Y，左上）、`+`（Z，右下）标注，三条黑色直线为决策边界，把平面划分为三个扇形区域，各类数据点分别落在对应区域内]]

（结论）CVX 求得的 $a_1,a_2,a_3,b_1,b_2,b_3$ 正确地把三类数据点分离进各自的区域，验证了该凸可行性建模的有效性。

---

## 题目 5（Problem 5）—— 附加题 2：正则化最小二乘的高效数值方法

**题干：** Efficient numerical method for a regularized least-squares problem. We consider a regularized least squares problem with smoothing,

```math
\text{minimize} \quad \sum_{i=1}^{k}(a_i^T x - b_i)^2 + \delta \sum_{i=1}^{n-1}(x_i - x_{i+1})^2 + \eta \sum_{i=1}^{n} x_i^2,
```

where $x \in \mathbb{R}^n$ is the variable, and $\delta, \eta > 0$ are parameters.

(a) Express the optimality conditions for this problem as a set of linear equations involving $x$ (these are called the normal equations).

(b) Now assume that $k \ll n$. Describe an efficient method to solve the normal equations found in (2a). Give an approximate flop count for a general method that does not exploit structure, and also for your efficient method.

(c) A numerical instance. Choose $k = 100$ and $n = 2000$, $\delta = \eta = 1$. Randomly generate $A$ and $b$, form and solve the normal equations generically, then implement your efficient method and verify the two solutions nearly agree and that your method is much faster.

**中文：** 正则化最小二乘（regularized least-squares）问题的高效数值方法。考虑带平滑（smoothing）的正则化最小二乘问题

```math
\text{minimize} \quad \sum_{i=1}^{k}(a_i^T x - b_i)^2 + \delta \sum_{i=1}^{n-1}(x_i - x_{i+1})^2 + \eta \sum_{i=1}^{n} x_i^2,
```

其中 $x \in \mathbb{R}^n$ 是变量，$\delta, \eta > 0$ 是参数。

(a) 把该问题的最优性条件（optimality condition）表示成关于 $x$ 的一组线性方程（称为正规方程 normal equations）。

(b) 假设 $k \ll n$。描述一个求解 (2a) 中正规方程的高效方法。分别给出不利用结构的通用方法的近似 flop 数，以及你的高效方法的 flop 数。

(c) 数值实例。取 $k = 100$、$n = 2000$、$\delta = \eta = 1$，随机生成 $A$ 和 $b$，先用通用方法形成并求解正规方程，再实现高效方法，验证两解几乎相同、且高效方法快得多。

**解答：**

**第一步：把目标写成二次型，得到正规方程。**

设 $A \in \mathbb{R}^{k \times n}$ 是以 $a_i$ 为行的矩阵，$\Delta \in \mathbb{R}^{n \times n}$ 是三对角（tridiagonal）矩阵：

```math
\Delta =
\begin{bmatrix}
1 & -1 & 0 & \cdots & 0 & 0 & 0\\
-1 & 2 & -1 & \cdots & 0 & 0 & 0\\
0 & -1 & 2 & \cdots & 0 & 0 & 0\\
\vdots & \vdots & \vdots & \ddots & \vdots & \vdots & \vdots\\
0 & 0 & 0 & \cdots & 2 & -1 & 0\\
0 & 0 & 0 & \cdots & -1 & 2 & -1\\
0 & 0 & 0 & \cdots & 0 & -1 & 1
\end{bmatrix}.
```

（$\Delta$ 的来源）平滑项 $\sum_{i=1}^{n-1}(x_i - x_{i+1})^2 = x^T \Delta x$，其中 $\Delta$ 两端为 $1$（边界点只连一条边），内部为 $2$（每个内部点 $x_i$ 出现在 $(x_{i-1}-x_i)^2$ 和 $(x_i - x_{i+1})^2$ 两项中）。数据项 $\sum_{i=1}^k (a_i^T x - b_i)^2 = \|Ax - b\|_2^2$，正则项 $\eta \sum_i x_i^2 = \eta x^T x$。于是目标函数展开为

```math
\|Ax - b\|_2^2 + \delta x^T \Delta x + \eta x^T x
= x^T(A^T A + \delta \Delta + \eta I)x - 2 b^T A x + b^T b.
```

（最优性条件）这是无约束的严格凸二次问题（$A^T A + \delta \Delta + \eta I \succ 0$，因 $\delta \Delta + \eta I$ 是正定的：$\Delta \succeq 0$ 且 $\eta I \succ 0$）。令梯度为零：

```math
\nabla_x \Big[x^T(A^T A + \delta \Delta + \eta I)x - 2 b^T A x\Big]
= 2(A^T A + \delta \Delta + \eta I)x - 2 A^T b = 0,
```

得到正规方程

```math
(A^T A + \delta \Delta + \eta I)\, x^\star = A^T b. \tag{1}
```

**(b) 高效求解（Sherman–Morrison–Woodbury 公式）。**

**通用方法 flop 数：** 若不利用结构，直接求解 $(1)$ 的 $n \times n$ 系统，Cholesky 分解代价约 $\frac13 n^3$ flop。

**高效方法：** 当 $k \ll n$ 时，$A^T A$ 是一个低秩（low-rank）修正（rank 至多 $k$），而 $\delta \Delta + \eta I$ 是三对角、易于分解的。设 $g = A^T b$，用 Sherman–Morrison–Woodbury 公式（矩阵求逆引理，matrix inversion lemma）把解写成

```math
x^\star = (\delta\Delta + \eta I)^{-1} g
 - (\delta\Delta + \eta I)^{-1} A^T \big(I + A(\delta\Delta + \eta I)^{-1} A^T\big)^{-1} A(\delta\Delta + \eta I)^{-1} g. \tag{2}
```

（公式的推导依据）这里对矩阵 $F = P + A^T A$ 应用 Woodbury 恒等式，其中 $P = \delta \Delta + \eta I$：

```math
(P + A^T A)^{-1} = P^{-1} - P^{-1} A^T (I + A P^{-1} A^T)^{-1} A P^{-1}.
```

因为 $A^T A = A^T I_k A$，取中间单位阵为 $I_k$（$k$ 维），故 $I + A P^{-1} A^T$ 是 $k \times k$ 的小矩阵，求逆代价仅 $\frac13 k^3$。

**分步计算与 flop 计数：**

1. 解 $P z_1 = g$ 与 $P Z_2 = A^T$，其中 $z_1 \in \mathbb{R}^n$、$Z_2 \in \mathbb{R}^{n \times k}$。因 $P$ 三对角，一次分解约 $n$ flop，随后每个右端向量的前代/回代约 $4n$ flop，共 $k+1$ 个右端。此步总代价约 $4nk + 5n$ flop。

2. 计算 $A z_1$（$2nk$ flop）与 $A Z_2$（$2 n k^2$ flop，因 $A Z_2$ 是 $k \times k$ 矩阵，每个元素是长度 $n$ 的内积）。

3. 解 $(I + A Z_2) z_3 = A z_1$，这是 $k \times k$ 系统，代价约 $\frac13 k^3$ flop。

4. 合成 $x^\star = z_1 - Z_2 z_3$，其中 $Z_2 z_3$ 是 $n \times k$ 乘 $k \times 1$，代价 $2nk$ flop。

（总 flop 数）仅保留最高阶项，总代价约

```math
2 n k^2 \ \text{flop},
```

当 $k \ll n$ 时远小于通用方法的 $\frac13 n^3$。

**(c) 数值实例与验证。**

取 $n = 2000$、$k = 100$、$\delta = \eta = 1$。核心思路：

- 用 `spdiags` 构造三对角矩阵 `D`（即 $\Delta$，内部对角为 $2$，两端对角为 $1$）。
- 通用方法：直接 `F = A'*A + eta*I + delta*D; x_gen = F\g`，其中 `g = A'*b`。
- 高效方法：令 `P = eta*I + delta*D`（三对角，易分解），按公式 $(2)$ 用矩阵左除 `\` 实现：

```matlab
Z_0 = P \ [g  A'];          % 一次分解 P，多右端求解
z_1 = Z_0(:, 1);            % P^{-1} g
Z_2 = Z_0(:, 2:k+1);        % P^{-1} A^T
z_3 = (sparse(1:k,1:k,1) + A*Z_2) \ (A*z_1);  % 解 k×k 系统
x_eff = z_1 - Z_2*z_3;      % 合成解
```

- 验证：计算相对误差（relative error）

```math
\frac{\|x_{\text{eff}} - x_{\text{gen}}\|_2}{\|x_{\text{gen}}\|_2},
```

应接近机器精度量级（约 $10^{-15}$），且高效方法的 CPU 时间远小于通用方法。

（关键技术说明）为让 Matlab 识别并利用三对角结构，须用 `spdiags` 把矩阵声明为稀疏（sparse）矩阵，或用 `sparse` 显式转换；否则 Matlab 可能不自动利用带状（banded）结构，导致速度变慢。此外，求解多个右端时，应把所有右端并成一个矩阵一次调用 `F\A`，让 Matlab 只分解一次 `F`、再做多次回代。

---

（全卷译注完成。共 5 题：8.16、9.30、9.31、附加题 1（三路线性分类）、附加题 2（正则化最小二乘）。）
