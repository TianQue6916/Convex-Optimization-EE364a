# EE364a Homework 8 additional problems · Standard form LP barrier method（EE364a 作业 8 附加题：标准形 LP 的 barrier method）

> 原文：`HW8 附加题.pdf`（3 页），EE364a, Winter 2007–08, Prof. S. Boyd。这是编程作业的完整任务说明；解答见 `HW8 解答.pdf` 第 5 页起的 "Solutions to additional exercises"（原文只复述任务，未给代码）。

下面三道练习中，你将为求解 standard form LP（标准形线性规划）

```math
\begin{aligned}
\text{minimize} \quad & c^T x \\
\text{subject to} \quad & Ax = b, \quad x \succeq 0,
\end{aligned}
```

实现一个 barrier method（障碍法/内点法）。变量为 $x \in \mathbf{R}^n$，其中 $A \in \mathbf{R}^{m \times n}$，$m < n$。整个练习中我们假设 $A$ 行满秩，且 sublevel sets（下水平集）$\{x \mid Ax = b,\ x \succeq 0,\ c^Tx \le \gamma\}$ 都有界。（若不成立，centering problem 下界无界。）

## 1. Centering step（中心化步）

对 centering problem

```math
\begin{aligned}
\text{minimize} \quad & c^T x - \sum_{i=1}^n \log x_i \\
\text{subject to} \quad & Ax = b,
\end{aligned}
```

实现 Newton's method：变量为 $x$，给定严格可行初始点 $x_0$。

你的代码应当接受 $A$、$b$、$c$、$x_0$，并返回 primal optimal point $x^\star$、dual optimal point $\nu^\star$ 以及执行的 Newton 步数。

用 block elimination method（块消去法）计算 Newton step。（你也可以通过 KKT system 计算 Newton step，并与块消去法算出的结果比较。两种步应当接近，但若某个 $x_i$ 非常小，你可能会得到关于 KKT matrix 条件数的警告。）

对不同的数据与初始点画出 $\lambda^2/2$ 随迭代 $k$ 的变化，以验证你的实现具有渐近 quadratic convergence。stopping criterion 可用 $\lambda^2/2 \le 10^{-6}$。对固定的问题实例，试验算法参数 $\alpha$ 与 $\beta$ 的取值，观察它们对所需总 Newton 步数的影响。检查你算出的 $x^\star$ 与 $\nu^\star$（近似）满足 KKT conditions。

为生成随机问题数据（即 $A$、$b$、$c$、$x_0$），我们推荐如下做法。先随机生成 $A$。（你可能想检查它是否行满秩。）然后生成随机正向量 $x_0$，取 $b = Ax_0$。（这保证 $x_0$ 严格可行。）参数 $c$ 可以随机选取。为确保 sublevel sets 有界，可以在 $A$ 上加一行全正元素。若希望能用同一份数据重复运行，务必为 uniform 与 normal 随机数生成器设好状态。

下面这些提示可能有用。

- 我们推荐用公式 $\lambda^2 = -\Delta x_{\mathrm{nt}}^T \nabla f(x)$ 计算 $\lambda^2$。你其实并不需要 $\lambda$ 本身；用 $\lambda^2$ 就够了。（这一点很重要，原因见下。）
- 你算出的 Newton step $\Delta x_{\mathrm{nt}}$ 可能带有小的数值误差。当 $x$ 接近最优时，$\lambda^2$ 的计算值（即 $\lambda^2 = -\Delta x_{\mathrm{nt}}^T\nabla f(x)$）实际上可能（略微）为负。若你开平方求 $\lambda$，会得到复数，而且永远回不来。此外，你的 line search 将永不退出。但这只在 $x$ 接近最优时发生。所以如果你以 $\lambda^2/2 \le 10^{-6}$ 为退出条件，一切都会正常，即使 $\lambda^2$ 的计算值为负。
- 对 line search，你必须先把步长 $t$ 乘以 $\beta$，直到 $x + t\Delta x_{\mathrm{nt}}$ 可行（即严格正）。若不这样做，你在计算 $f$ 时就会对负数取对数，而且永远回不来。

（† 译注：提示第一条里的公式在 PDF 中印作 $\lambda^2 = -\Delta x_{\mathrm{nt}}^T\nabla f(x)$，按书中定义应为 $\lambda^2 = -\Delta x_{\mathrm{nt}}^T\nabla^2 f(x)\Delta x_{\mathrm{nt}}$（Newton decrement 的定义）——但两者在数值上相等，因为 $\Delta x_{\mathrm{nt}} = -[\nabla^2f(x)]^{-1}\nabla f(x)$，故 $-\Delta x_{\mathrm{nt}}^T\nabla f(x) = -\nabla f(x)^T[\nabla^2f]^{-1}\nabla f(x) = \lambda^2$。原文第二处提示里写的是同一个「简化」形式，不是笔误。）

## 2. LP solver with strictly feasible starting point（带严格可行初始点的 LP solver）

用 (1) 中的 centering 代码，实现一个 barrier method 来求解 standard form LP

```math
\begin{aligned}
\text{minimize} \quad & c^T x \\
\text{subject to} \quad & Ax = b, \quad x \succeq 0,
\end{aligned}
```

变量为 $x \in \mathbf{R}^n$，给定严格可行初始点 $x_0$。你的 LP solver 应当以 $A$、$b$、$c$、$x_0$ 为参数，返回 $x^\star$。

当由 $n/t$ 度量的 duality gap（对偶间隙）小于 $10^{-3}$ 时，可以终止你的 barrier method。（若把容差取得小得多，你可能会遇到数值问题。）用若干问题实例把你的 LP solver 与 cvx 求出的解做对比。

(1) 中关于如何生成随机数据的评论在这里同样适用。

试验参数 $\mu$，观察它对各 centering step 所需 Newton 步数、以及求解整个问题所需总 Newton 步数的影响。

对 $n = 500$、$m = 100$ 的问题实例画出算法进展：纵轴为 duality gap（对数刻度），横轴为累计总 Newton 步数（线性刻度）。

你的算法应当返回一个 $2 \times k$ 矩阵 `history`（$k$ 是 centering step 总数），其第一行含每个 centering step 所需的 Newton 步数，第二行给出每个 centering step 结束时的 duality gap。为得到与书中图（例如第 572 页图 11.4）相似的图，你应使用如下代码：

```matlab
[xx, yy] = stairs(cumsum(history(1,:)),history(2,:));
semilogy(xx,yy);
```

## 3. LP solver（通用 LP solver）

用 (2) 中的代码，实现一个通用的 standard form LP solver：它以 $A$、$b$、$c$ 为参数，判定（严格）可行性与否，并在问题（严格）可行时返回一个 optimal point。

你需要实现一个 phase I method（第一阶段方法），它判定问题是否严格可行，若严格可行则找出一个严格可行点，从而可以喂给 (2) 中的代码。事实上，你可以用 (2) 中的代码来实现 phase I method。

为找出严格可行初始点 $x_0$，我们求解 phase I problem

```math
\begin{aligned}
\text{minimize} \quad & t \\
\text{subject to} \quad & Ax = b \\
& x \succeq (1 - t)\mathbf{1}, \quad t \ge 0,
\end{aligned}
```

变量为 $x$ 与 $t$。若我们能找到一个可行的 $(x,t)$ 且 $t < 1$，则 $x$ 对原问题是严格可行的。反之亦然，因此原 LP 严格可行当且仅当 $t^\star < 1$，其中 $t^\star$ 是 phase I problem 的 optimal value。

我们可以用任何满足 $Ax^0 = b$ 的 $x^0$ 以及 $t^0 = 2 - \min_i x_i^0$ 来初始化 phase I problem 的 $x$ 与 $t$。（这里可以假设 $\min_i x_i^0 \le 0$；否则 $x^0$ 已经是严格可行点，我们直接就完成了。）你可以用变量替换 $z = x + (t - 1)\mathbf{1}$ 把 phase I problem 变换成 (2) 中的形式。

（† 原文印作 $z = x + (t-1)$，其中 $t - 1$ 是标量、$x$ 是向量；应理解为 $z = x + (t-1)\mathbf{1}$，即对每个分量加上同一个标量。补齐推导：由约束 $x_i \ge 1 - t$ 得 $x_i + (t-1) \ge 0$，所以取 $z_i = x_i + (t-1)$ 后约束正好变成 $z \succeq 0$；而 $Ax = b$ 变为 $Az = b + (t-1)A\mathbf{1}$，不再是固定的等式组——这正是为什么原文说「变换成 (2) 中的形式」时要小心处理右端项随 $t$ 变化这一点。第 572 页书中的标准做法是把 $t$ 与 $z$ 一起当作变量求解。）

用若干数值例子（既有可行实例也有不可行实例）把你的 LP solver 与 cvx 对比检验。
