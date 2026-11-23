# EE364a Homework 6 additional problems（EE364a 作业 6 附加题）

> 原文：`HW6 附加题.pdf`（7 页），EE364a, Winter 2007–08, Prof. S. Boyd。这是题面；解答见 `HW6 解答.pdf` 第 2 页起的 "Solutions to additional exercises"。

## 1. Minimax rational fit to the exponential（指数函数的 minimax 有理拟合）

（见习题 6.9。）考虑具体实例，数据为

```math
t_i = -3 + 6(i-1)/(k-1), \qquad y_i = e^{t_i}, \qquad i = 1,\dots,k,
```

其中 $k = 201$。（换句话说，数据是把指数函数在区间 $[-3,3]$ 上均匀采样得到的。）求形如

```math
f(t) = \frac{a_0 + a_1 t + a_2 t^2}{1 + b_1 t + b_2 t^2}
```

的函数，使 $\max_{i=1,\dots,k} |f(t_i) - y_i|$ 最小。（我们要求 $1 + b_1 t_i + b_2 t_i^2 > 0$ 对 $i = 1,\dots,k$ 成立。）

求出 $a_0$、$a_1$、$a_2$、$b_1$、$b_2$ 的 optimal values，并给出 optimal objective value，计算精度 $0.001$。在同一张图上画出数据与 optimal rational function fit。另画一张图给出 fitting error，即 $f(t_i) - y_i$。

提示：可以在 `cvx_end` 之后用 `strcmp(cvx_status,'Solved')` 检查一个 feasibility problem 是否可行。

## 2. Maximum likelihood prediction of team ability（球队实力的 maximum likelihood 预测）

有 $n$ 支球队参加一个 tournament（联赛）。我们把每支球队的实力用一个数 $a_j \in [0,1]$（$j = 1,\dots,n$）建模。当球队 $j$ 与 $k$ 交手时，球队 $j$ 获胜的概率等于 $\mathrm{prob}(a_j - a_k + v > 0)$，其中 $v \sim \mathcal{N}(0,\sigma^2)$。

给定 $m$ 场已结束比赛的胜负结果，表示为

```math
(j^{(i)},\ k^{(i)},\ y^{(i)}), \qquad i = 1,\dots,m,
```

意思是第 $i$ 场由球队 $j^{(i)}$ 对球队 $k^{(i)}$；$y^{(i)} = 1$ 表示球队 $j^{(i)}$ 获胜，$y^{(i)} = -1$ 表示球队 $k^{(i)}$ 获胜。（我们假设没有平局。）

**(a)** 把「给定这些结果，求球队实力的 maximum likelihood estimate $a^\star \in \mathbf{R}^n$」表述为一个 convex optimization problem。你会发现 game incidence matrix $A \in \mathbf{R}^{m \times n}$ 很有用，其定义为

```math
A_{il} = \begin{cases} y^{(i)} & l = j^{(i)} \\ -y^{(i)} & l = k^{(i)} \\ 0 & \text{otherwise} . \end{cases}
```

prior constraints $a_i \in [0,1]$ 应当包含在问题表述中。另外注意：若给所有球队实力加上同一个常数，比赛结果的概率不变。这意味着 $a^\star$ 只能确定到一个常数（像势能那样）。但这不影响 ML 估计问题，也不影响用估计出的参数做出的任何后续预测。

**(b)** 对 `team_data.m` 中给出的球队数据（矩阵 `train`）求 $a^\star$。（该矩阵给出的是一个每两队各交手一次的联赛的结果。）

CVX 不支持 concave function $\log\Phi$（$\Phi$ 是单位 Gaussian 的累积分布），但我们在课程网站上提供了一个足够好的近似 `log_normcdf`。该函数被重载以处理向量输入（逐元素）。

可以用下面的命令构造 $A$：

```matlab
A = sparse(1:m,train(:,1),train(:,3),m,n) + ...
sparse(1:m,train(:,2),-train(:,3),m,n);
```

**(c)** 用 (b) 中求得的 maximum likelihood estimate $a^\star$ 预测下一年联赛各场比赛的结果（见矩阵 `test`），预测式为 $\hat{y}^{(i)} = \mathrm{sign}(a^\star_{j^{(i)}} - a^\star_{k^{(i)}})$。把这些预测与 `test` 第三列给出的实际结果比较，给出正确预测的比例。

`train` 与 `test` 中的比赛是同一批，因此另一种更简单的预测方法是「假定去年赢的那支队今年还会赢」。给出这种简单方法的正确预测百分比。

## 3. Piecewise-linear fitting（分段线性拟合）

在许多应用中，模型中的某个函数不是由公式给出，而是以表格数据（tabulated data）的形式给出。这种表格数据可能来自经验测量、历史数据、对某个复杂表达式的数值求值，或对某个问题在若干自变量取值上的求解。为了在 convex optimization model 中使用它，我们必须用一个与求解器或其它系统兼容的 convex function 去拟合这些数据。本题探索这类一般问题中一个非常简单的例子。

假设给定数据 $(x_i, y_i)$，$i = 1,\dots,m$，其中 $x_i, y_i \in \mathbf{R}$。我们假设 $x_i$ 已排序，即 $x_1 < x_2 < \cdots < x_m$。设 $a_0 < a_1 < a_2 < \cdots < a_K$ 是一组固定的 knot points（节点），且 $a_0 \le x_1$、$a_K \ge x_m$。说明如何找出定义在 $[a_0,a_K]$ 上、以 $a_i$ 为节点的 convex piecewise linear function $f$，使 least-squares fitting criterion

```math
\sum_{i=1}^m (f(x_i) - y_i)^2
```

最小。你必须说明变量是什么、它们如何参数化 $f$，以及如何保证 $f$ 的 convexity。

**Hints（提示）**。一种方法是基于 Lagrange basis（Lagrange 基）$f_0,\dots,f_K$，它们是满足

```math
f_j(a_i) = \delta_{ij}, \qquad i,j = 0,\dots,K
```

的 piecewise linear function。另一种方法是定义 $f(x) = \alpha_i x + \beta_i$（$x \in (a_{i-1},a_i]$），然后对参数 $\alpha_i$ 与 $\beta_i$ 加上条件以保证 $f$ 连续且 convex。

把你的方法应用到文件 `pwl_fit_data.m` 中的数据（其中 $x_j \in [0,1]$）。求出最好的 affine fit（对应 $a = (0,1)$），以及 1、2、3 个 internal knot point（在 $[0,1]$ 内均匀分布）时最好的 piecewise-linear convex function fit。（例如，3 个 internal knot point 时 $a_0 = 0$、$a_1 = 0.25$、$a_2 = 0.50$、$a_3 = 0.75$、$a_4 = 1$。）给出每种的 least-squares fitting cost。画出数据与求得的分段线性拟合。把每个函数表示成

```math
f(x) = \max_{i=1\dots,K} (\alpha_i x + \beta_i)
```

的形式。（在这种形式下，函数很容易被纳入优化问题。）

## 4. Robust least-squares with interval coefficient matrix（区间系数矩阵的 robust least-squares）

$\mathbf{R}^{m \times n}$ 中的 interval matrix（区间矩阵）是元素为区间的矩阵：

```math
\mathcal{A} = \{A \in \mathbf{R}^{m \times n} \mid |A_{ij} - \bar{A}_{ij}| \le R_{ij},\ i = 1,\dots,m,\ j = 1,\dots,n\} .
```

矩阵 $\bar{A} \in \mathbf{R}^{m \times n}$ 称为 nominal value 或 center value，$R \in \mathbf{R}^{m \times n}$（逐元素非负）称为 radius（半径）。

带 interval matrix 的 robust least-squares problem 为

```math
\text{minimize} \quad \sup_{A \in \mathcal{A}} \|Ax - b\|_2 ,
```

优化变量为 $x \in \mathbf{R}^n$。问题数据为 $\bar{A}$（即 $\mathcal{A}$ 与 $R$）以及 $b \in \mathbf{R}^m$。objective 作为 $x$ 的函数称为 worst-case residual norm（最坏情形残差范数）。robust least-squares problem 显然是一个 convex optimization problem。

**(a)** 把 interval matrix robust least-squares problem 表述为一个标准优化问题，例如 QP、SOCP 或 SDP。需要时可以引入新变量。你的改写中变量与约束的数目应当随 $m,n$ 线性增长，而**不是**指数增长。

**(b)** 考虑 $m = 4$、$n = 3$ 的具体实例：

```math
A = \begin{bmatrix}
60 \pm 0.05 & 45 \pm 0.05 & -8 \pm 0.05 \\
90 \pm 0.05 & 30 \pm 0.05 & -30 \pm 0.05 \\
0 \pm 0.05 & -8 \pm 0.05 & -4 \pm 0.05 \\
30 \pm 0.05 & 10 \pm 0.05 & -10 \pm 0.05
\end{bmatrix}, \quad
b = \begin{bmatrix} -6 \\ -3 \\ 18 \\ -9 \end{bmatrix} .
```

（$A$ 中每个条目的第一部分给出 $\bar{A}_{ij}$，第二部分给出 $R_{ij}$，这里全都是 $0.05$。）求 nominal problem（即 minimize $\|\bar{A}x - b\|_2$）的解 $x_{\mathrm{ls}}$，以及 robust least-squares 解 $x_{\mathrm{rls}}$。对这两个解，分别求 nominal residual norm 与 worst-case residual norm。确保结果说得通。

## 5. Total variation image interpolation（total variation 图像插值）

灰度图像表示为 $m \times n$ 的强度矩阵 $U^{\mathrm{orig}}$。已知 $U^{\mathrm{orig}}_{ij}$ 在 $(i,j) \in K$ 上的取值，其中 $K \subset \{1,\dots,m\} \times \{1,\dots,n\}$。你的任务是插值整幅图像，即猜测缺失的值。重建图像表示为 $U \in \mathbf{R}^{m \times n}$，其中 $U$ 满足插值条件 $U_{ij} = U^{\mathrm{orig}}_{ij}$（$(i,j) \in K$）。

重建通过「在插值条件约束下极小化某个 roughness measure（粗糙度度量）」来得到。一种常见的 roughness measure 是（平方）$\ell_2$ variation：

```math
\sum_{i=2}^m \sum_{j=2}^n \big( (U_{ij} - U_{i-1,j})^2 + (U_{ij} - U_{i,j-1})^2 \big) .
```

另一种方法改为极小化 total variation（全变差）：

```math
\sum_{i=2}^m \sum_{j=2}^n \big( |U_{ij} - U_{i-1,j}| + |U_{ij} - U_{i,j-1}| \big) .
```

显然两种方法都导致 convex optimization problem。

对 `tv_img_interp.m` 中给出的数据实例分别做 $\ell_2$ 与 total variation 插值。该脚本会定义 $m$、$n$ 以及矩阵 `Uorig` 与 `Known`。`Known` 是 $m \times n$ 矩阵，若 $(i,j) \in K$ 则 $(i,j)$ 项为一，否则为零。该 m 文件还有画图的骨架代码。（我们给出整幅原图，以便你把重建结果与原图比较；显然你的解不能访问 $(i,j) \notin K$ 的 $U^{\mathrm{orig}}_{ij}$。）

† 译注：原文的求和范围 $i = 2,\dots,m$、$j = 2,\dots,n$ 只覆盖内部的相邻差分，漏掉了第一行与第一列的差分（通常意义下的全变差应遍历所有相邻像素对）。原文解答中的代码用 `2:end` 与 `1:end-1`，与上面的公式一致，因此这是「题面写法」与「通常 TV 定义」的出入，不影响解答自洽。

## 6. Relaxed and discrete A-optimal experiment design（松弛与离散 A-optimal 实验设计）

本题讨论第 387 页描述的 A-optimal experiment design problem，数据生成如下。

```matlab
n = 5; % dimension of parameters to be estimated
p = 20; % number of available types of measurements
m = 30; % total number of measurements to be carried out
randn('state', 0);
V=randn(n,p); % columns are vi, the possible measurement vectors
```

求解 relaxed A-optimal experiment design problem

```math
\begin{aligned}
\text{minimize} \quad & (1/m)\, \mathbf{tr}\left( \sum_{i=1}^p \lambda_i v_i v_i^T \right)^{-1} \\
\text{subject to} \quad & \mathbf{1}^T \lambda = 1, \quad \lambda \succeq 0,
\end{aligned}
```

变量为 $\lambda \in \mathbf{R}^p$。求 optimal point $\lambda^\star$ 与 relaxed problem 相应的 optimal value。该 optimal value 是 discrete A-optimal experiment design problem

```math
\begin{aligned}
\text{minimize} \quad & \mathbf{tr}\left( \sum_{i=1}^p m_i v_i v_i^T \right)^{-1} \\
\text{subject to} \quad & m_1 + \cdots + m_p = m, \quad m_i \in \{0,\dots,m\},\ i = 1,\dots,p,
\end{aligned}
```

的 optimal value 的一个 lower bound，后者的变量为 $m_1,\dots,m_p$。为得到该离散问题的一个 suboptimal point，把 $m\lambda^\star$ 的各分量取整得到整数 $\hat{m}_i$。若需要，用手工或其它方法调整它们使其和为 $m$，并计算得到的 objective value。这当然是离散问题 optimal value 的一个 upper bound。给出该 upper bound 与 relaxed problem 所得 lower bound 之间的 gap。

注意两个 objective value 都可以解释为 mean-square estimation error $\mathbf{E}\|\hat{x} - x\|_2^2$。
