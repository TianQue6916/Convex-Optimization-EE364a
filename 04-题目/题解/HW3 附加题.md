# EE364a Homework 3 additional problems（EE364a 作业 3 附加题）

> 原文：`HW3 附加题.pdf`（4 页），EE364a, Winter 2007–08, Prof. S. Boyd。这是题面，解答见 `HW3 解答.pdf` 第 10 页起的 "Solutions to additional exercises"。

## 1. Optimal activity levels（最优活动水平）

求解 Convex Optimization 一书习题 4.17 描述的最优活动水平问题，实例数据为

```math
A = \begin{bmatrix}
1 & 2 & 0 & 1 \\
0 & 0 & 3 & 1 \\
0 & 3 & 1 & 1 \\
2 & 1 & 2 & 5 \\
1 & 0 & 3 & 2
\end{bmatrix}, \quad
c^{\max} = \begin{bmatrix} 100 \\ 100 \\ 100 \\ 100 \\ 100 \end{bmatrix}, \quad
p = \begin{bmatrix} 3 \\ 2 \\ 7 \\ 6 \end{bmatrix}, \quad
p^{\mathrm{disc}} = \begin{bmatrix} 2 \\ 1 \\ 4 \\ 2 \end{bmatrix}, \quad
q = \begin{bmatrix} 4 \\ 10 \\ 5 \\ 10 \end{bmatrix}.
```

† 原文此处五个量在 PDF 里挤排，pdftotext/pdfminer 提取出的行序完全错乱（`A` 的每一行与 $c^{\max}$ 的每一分量交错成 "1 2 0 1 100" 之类的行）；上表数值依据教材习题 4.17 的结构 + 原文解答第 10 页 Matlab 代码 `A=[...]`、`cmax=[100;100;100;100;100]`、`p=[3;2;7;6]`、`pdisc=[2;1;4;2]`、`q=[4;10;5;10]` 复原。

做这道题可以构造你在习题 4.17 解答中得到的 LP，用 cvx 求解该 LP；也可以更直接地用 cvx 函数写。（你甚至可以把两种解法都实现一遍，用另一种作为对习题 4.17 解答的检验。）

给出 optimal activity level、每个活动产生的 revenue（收益）、以及 optimal solution 生成的总 revenue。另外给出每个活动水平的 average price per unit（单位平均价格），即某个活动对应的 revenue 与该活动水平之比。（这些数值应当落在每个活动的 basic price（基本价格）与 discounted price（折扣价）之间。）用一段很简短的话解释、或至少评述一下你得到的解。

## 2. Reformulating constraints in cvx（在 cvx 中改写约束）

下面每个 cvx 代码片段都描述了标量变量 `x`、`y`、`z` 上的一个 convex constraint，但违反了 cvx 的 rule set（规则集），因此是无效的。简要说明每个片段为何无效；然后把每个片段改写成符合 cvx rule set 的等价形式。改写时可以使用 linear equality/inequality constraints（线性等式与不等式约束），以及用 cvx 函数构造的不等式。也可以引入额外变量，或使用 LMI（线性矩阵不等式）。若你的改写与原约束的等价性不明显，务必（简要）说明为什么等价。

请通过构造一个包含这些约束的小问题、并用 cvx 求解它来检验你的改写。测试问题不必可行；只要验证 cvx 能无错地处理你的约束就够了。

**Remark（评注）**。这看起来像是「怎么用 cvx 软件」或「cvx 使用技巧」的题目。但它实际上检验的是你是否理解了各种 composition rule（复合规则）、convex analysis（凸分析）以及 constraint reformulation rule（约束改写规则）。

**(a)** `norm( [ x + 2*y , x - y ] ) == 0`

**(b)** `square( square( x + y ) ) <= x - y`

**(c)** `1/x + 1/y <= 1; x >= 0; y >= 0`

**(d)** `norm([ max( x , 1 ) , max( y , 2 ) ]) <= 3*x + y`

**(e)** `x*y >= 1; x >= 0; y >= 0`

**(f)** `( x + y )^2 / sqrt( y ) <= x - y + 5`

**(g)** `x^3 + y^3 <= 1; x>=0; y>=0`

**(h)** `x+z <= 1+sqrt(x*y-z^2); x>=0; y>=0`

## 3. The illumination problem（照明问题）

本题讨论第 1 讲（第 9–11 页）描述的 illumination problem。我们取 $I^{\mathrm{des}} = 1$、$p^{\max} = 1$，于是问题为

```math
\begin{aligned}
\text{minimize} \quad & f_0(p) = \max_{k=1,\dots,n} |\log(a_k^T p)| \\
\text{subject to} \quad & 0 \le p_j \le 1, \quad j = 1,\dots,m,
\end{aligned}
```

（式 (1)）变量为 $p \in \mathbf{R}^n$。你要针对一个具体的问题实例，计算若干 approximate solution（近似解），并把结果与 exact solution（精确解）比较。

正如课上所述，该问题等价于

```math
\begin{aligned}
\text{minimize} \quad & \max_{k=1,\dots,n} h(a_k^T p) \\
\text{subject to} \quad & 0 \le p_j \le 1, \quad j = 1,\dots,m,
\end{aligned}
```

（式 (2)）其中 $h(u) = \max\{u, 1/u\}$（$u > 0$）。函数 $h$ 如下图，它是 nonlinear（非线性）、nondifferentiable（不可微）且 convex 的。为看清 (1) 与 (2) 的等价性，我们注意

```math
\begin{aligned}
f_0(p) &= \max_{k=1,\dots,n} |\log(a_k^T p)| \\
&= \max_{k=1,\dots,n} \max\{\log(a_k^T p), \log(1/a_k^T p)\} \\
&= \log \max_{k=1,\dots,n} \max\{a_k^T p, 1/a_k^T p\} \\
&= \log \max_{k=1,\dots,n} h(a_k^T p),
\end{aligned}
```

而由于 logarithm（对数）是单调递增函数，极小化 $f_0$ 等价于极小化 $\max_{k=1,\dots,n} h(a_k^T p)$。

[Figure 1：函数 $h(u) = \max\{u, 1/u\}$ 的图像——横轴 $u \in [0,4]$（刻度 0,1,2,3,4），纵轴 $h(u) \in [0,5]$（刻度 0..5）；曲线在 $u=1$ 处取最小值 $1$；$u<1$ 时沿 $1/u$ 急剧下降、$u>1$ 时沿直线 $u$ 上升，故在 $u=1$ 处有尖点]

**The problem instance（问题实例）**。具体问题数据对应下图所示的几何布置，用课上给出的公式

```math
a_{kj} = r_{kj}^{-2} \max\{\cos\theta_{kj}, 0\}
```

计算。共有 10 盏灯（$m = 10$）与 20 块 patch（$n = 20$）。取 $I^{\mathrm{des}} = 1$、$p^{\max} = 1$。问题数据在课程网站文件 `illum_data.m` 中给出。运行该脚本会构造矩阵 $A$（其各行是 $a_k^T$），并画出下图所示的灯/patch 几何布置。

[Figure 2：灯与 patch 的一维几何布置——横轴为位置坐标 $[0,1]$（刻度 0, 0.2, 0.4, 0.6, 0.8, 1），纵轴 $[0,2]$；用高 $0.5$ 的竖线标出 10 盏灯的位置（大致均匀分布在 $[0,1]$ 上），灯与 patch 的耦合系数由 $r_{kj}^{-2}\max\{\cos\theta_{kj},0\}$ 给出]

**Equal lamp powers（等功率灯）**。取 $p_j = \gamma$（$j = 1,\dots,m$）。在区间 $[0,1]$ 上画出 $f_0(p)$ 随 $\gamma$ 的变化。用图解法确定 $\gamma$ 的最优值与相应的 objective value。

可以在 Matlab 中用 `max(abs(log(A*p)))` 计算 objective function $f_0(p)$。

**Least-squares with saturation（带饱和的最小二乘）**。求解 least-squares 问题

```math
\text{minimize} \quad \sum_{k=1}^n (a_k^T p - 1)^2 = \|Ap - \mathbf{1}\|_2^2 .
```

若解中某些 $p_i$ 为负，把它们置为 $0$；若某些值大于 $1$，把它们置为 $1$。给出由此得到的 $f_0(p)$ 值。

least-squares solution 可以用 Matlab 的反斜杠运算符计算：`A\b` 返回 least-squares 问题

```math
\text{minimize} \quad \|Ax - b\|_2^2
```

的解。

**Regularized least-squares（正则化最小二乘）**。求解正则化 least-squares 问题

```math
\text{minimize} \quad \sum_{k=1}^n (a_k^T p - 1)^2 + \rho \sum_{j=1}^m (p_j - 0.5)^2
= \|Ap - \mathbf{1}\|_2^2 + \rho \|p - (1/2)\mathbf{1}\|_2^2,
```

其中 $\rho > 0$ 是参数。逐步增大 $\rho$ 直到 $p$ 的所有 coefficient（系数）都落在区间 $[0,1]$ 内。给出由此得到的 $f_0(p)$ 值。

可以用 Matlab 的反斜杠运算符求解该正则化 least-squares 问题（把 $\sqrt{\rho}I$ 堆叠到 $A$ 下面、把 $\sqrt{\rho}\cdot 0.5\,\mathbf{1}$ 堆叠到 $\mathbf{1}$ 下面即可）。

**Chebyshev approximation（切比雪夫逼近）**。求解问题

```math
\begin{aligned}
\text{minimize} \quad & \max_{k=1,\dots,n} |a_k^T p - 1| = \|Ap - \mathbf{1}\|_\infty \\
\text{subject to} \quad & 0 \le p_j \le 1, \quad j = 1,\dots,m .
\end{aligned}
```

我们可以把这个间题看成是用 piecewise-linear function（分段线性函数）$|u - 1| + 1$ 逼近非线性函数 $h(u)$。如下图所示，该逼近在 $u = 1$ 附近效果很好。

[Figure 3：逼近效果对照——横轴 $u \in [0,2]$（刻度 0, 0.5, 1, 1.5, 2），纵轴 $[0,4]$（刻度 0..3.5）；实线为 $h(u) = \max\{u, 1/u\}$，虚线为 $|u-1|+1$；两线在 $u=1$ 处相切（都取最小值 $1$），在 $u=0.5$ 与 $u=2$ 处虚线值 $1.5$ 而 $h$ 值 $2$]

可以用 cvx 求解该 Chebyshev approximation 问题。（convex）函数 $\|Ap - \mathbf{1}\|_\infty$ 在 cvx 中可写成 `norm(A*p-ones(n,1),inf)`。给出由此得到的 $f_0(p)$ 值。

**Exact solution（精确解）**。最后，用 cvx 精确求解

```math
\begin{aligned}
\text{minimize} \quad & \max_{k=1,\dots,n} \max(a_k^T p,\ 1/a_k^T p) \\
\text{subject to} \quad & 0 \le p_j \le 1, \quad j = 1,\dots,m
\end{aligned}
```

你可能会觉得 `inv_pos()` 函数有用。给出由此得到的（optimal）$f_0(p)$ 值。
