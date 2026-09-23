# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

> <span style="color:#7f8c8d;">Source: SEE官方讲义/07StatisticalEst.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

## 7. Statistical Estimation（统计估计）

<!-- p.1 -->

### <span style="color:#2471a3;">**[lecture]**</span> <span style="color:#c0392b;">Statistical Estimation（统计估计）</span>

本讲三大主题：

- maximum likelihood estimation（最大似然估计）
- optimal detector design（最优检测器设计）
- experiment design（实验设计）

<!-- p.2 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Parametric distribution estimation（参数分布估计）</span>

- distribution estimation problem（分布估计问题）：由随机变量的观测值出发，估计其概率密度 $p(y)$。
- parametric distribution estimation（参数分布估计）：从一族以参数 $x$ 索引的密度 $p_x(y)$ 中选取一个。

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">**maximum likelihood estimation（最大似然估计）：在 $x$ 上最大化 $\log p_x(y)$。**</span>

- $y$ 是观测值；
- $l(x) = \log p_x(y)$ 称为 log-likelihood function（对数似然函数）；
- 可以显式地添加约束 $x \in C$；等价地，也可以直接定义当 $x \notin C$ 时 $p_x(y) = 0$；
- 当 $\log p_x(y)$ 在 $y$ 固定时关于 $x$ 是凹函数时，这是一个凸优化问题。

```math
\text{maximize (over } x\text{)} \quad \log p_x(y)
```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这条定义值得从你正在学的 Bertsekas 概率论出发拆解一遍：MLE 问的是"参数 $x$ 取何值时，眼前这批观测 $y$ 出现的概率密度最大"。当样本独立同分布时，联合密度等于各样本边缘密度之积，这正是下一页 $p_x(y)=\prod_i p(y_i-a_i^T x)$ 的来历。为什么先取对数再最大化？从第一性原理看有三层：其一，连乘大量小于 1 的数会数值下溢，取对数把乘积变成求和，计算稳定；其二，$\log$ 是严格递增函数，不改变最大值点的位置；其三，常见分布族的对数似然恰好是参数的凹函数，于是"最大化凹函数"落回本书的凸优化框架——等价于最小化凸的负对数似然 $-l(x)$。还要留意约束处理的巧思：不显式写 $x\in C$ 也行，只需令集合外密度为零，似然会自然把最优解压回集合内部。这条"噪声密度形状决定问题凸性"的主线，将贯穿后面所有例子。

<!-- p.3 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Linear measurements with IID noise（独立同分布噪声下的线性测量）</span>

linear measurement model（线性测量模型）：

```math
y_i = a_i^T x + v_i, \qquad i = 1, \ldots, m
```

- $x \in \mathbb{R}^n$ 是未知参数向量；
- $v_i$ 是独立同分布（IID）的测量噪声，密度为 $p(z)$；
- $y_i$ 是测量值：$y \in \mathbb{R}^m$ 的密度为 $p_x(y) = \prod_{i=1}^{m} p\left(y_i - a_i^T x\right)$。

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">**maximum likelihood estimate（最大似然估计）：下述问题的任一解 $x$**</span>（其中 $y$ 是观测值）：

```math
\text{maximize} \quad l(x) = \sum_{i=1}^{m} \log p\left(y_i - a_i^T x\right)
```

<!-- p.4 -->

### <span style="color:#2471a3;">**[example]**</span> <span style="color:#c0392b;">examples（三种典型噪声下的显式结果）</span>

**Gaussian noise（高斯噪声）$N(0, \sigma^2)$**：$p(z) = (2\pi\sigma^2)^{-1/2} e^{-z^2/(2\sigma^2)}$，

```math
l(x) = -\frac{m}{2}\log(2\pi\sigma^2) - \frac{1}{2\sigma^2} \sum_{i=1}^{m} \left(a_i^T x - y_i\right)^2
```

maximum likelihood（ML，最大似然）估计就是最小二乘（least-squares, LS）解。

**Laplacian noise（拉普拉斯噪声）**：$p(z) = (1/(2a))\, e^{-|z|/a}$，

```math
l(x) = -m\log(2a) - \frac{1}{a} \sum_{i=1}^{m} \left|a_i^T x - y_i\right|
```

ML 估计就是 $\ell_1$-norm solution（$\ell_1$ 范数解）。

**uniform noise on $[-a, a]$（区间 $[-a,a]$ 上的均匀噪声）**：

```math
l(x) =
\begin{cases}
-m\log(2a), & \left|a_i^T x - y_i\right| \le a,\ i = 1, \ldots, m \\
-\infty, & \text{otherwise}
\end{cases}
```

即当所有 $\left|a_i^T x - y_i\right| \le a$ 时为 $-m\log(2a)$，否则为 $-\infty$。ML 估计是任何一个满足 $\left|a_i^T x - y_i\right| \le a$（$i = 1, \ldots, m$）的 $x$。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这张对应表是本讲最值得记住的结论：噪声密度决定损失形状，噪声尾部越重，最优损失对大误差的惩罚就越温和。高斯噪声给出二次损失，ML 估计退化为你在 Strang 线代里早已熟悉的最小二乘：解 $\left(\sum_i a_i a_i^T\right)^{-1}\sum_i y_i a_i$ 正是正规方程与列空间投影思想的结合。拉普拉斯噪声给出 $\ell_1$ 损失：绝对值函数对离群点只做线性惩罚，不像平方那样把大误差放大再放大，因此对野值更鲁棒——这也是后来正则化、稀疏估计偏爱 $\ell_1$ 的同款理由。均匀噪声最极端：密度在区间外严格为零，似然直接判 $-\infty$，优化问题退化成可行性问题，任何把全部残差压进 $[-a,a]$ 的 $x$ 都同样好。二次、绝对值、可行域，三种损失对应三种尾部行为，背后是同一个似然原理。

<!-- p.5 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Logistic regression（Logistic 回归）</span>

随机变量 $y \in \{0, 1\}$，其分布为

```math
p = \text{prob}(y = 1) = \frac{\exp(a^T u + b)}{1 + \exp(a^T u + b)}
```

- $a, b$ 是参数；$u \in \mathbb{R}^n$ 是（可观测的）explanatory variables（解释变量）；
- 估计问题：由 $m$ 次观测 $(u_i, y_i)$ 估计 $a, b$。

log-likelihood function（对数似然函数）（当 $y_1 = \cdots = y_k = 1$，$y_{k+1} = \cdots = y_m = 0$ 时）：

```math
\begin{aligned}
l(a, b)
&= \log\left(
\prod_{i=1}^{k} \frac{\exp(a^T u_i + b)}{1 + \exp(a^T u_i + b)}
\prod_{i=k+1}^{m} \frac{1}{1 + \exp(a^T u_i + b)}
\right) \\
&= \sum_{i=1}^{k} \left(a^T u_i + b\right) - \sum_{i=1}^{m} \log\left(1 + \exp(a^T u_i + b)\right)
\end{aligned}
```

该函数关于 $a, b$ 是凹的（concave）。

<!-- p.6 -->

### <span style="color:#2471a3;">**[example]**</span> <span style="color:#c0392b;">example（$n = 1$、$m = 50$ 次测量的数值例）</span>

图为 Logistic 曲线拟合结果：横轴为 $u$（0 到 10），纵轴为 $\text{prob}(y = 1)$（0 到 1）。

- 圆点标出 50 个数据点 $(u_i, y_i)$；
- 实曲线是 ML 估计得到的 $p = \exp(au + b)/(1 + \exp(au + b))$。

<!-- p.7 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">(Binary) hypothesis testing（二元假设检验）</span>

<span style="color:#2471a3;">**[definition]**</span> detection（hypothesis testing）problem（检测，即假设检验问题）：对随机变量 $X \in \{1, \ldots, n\}$ 作一次观测，在以下两个假设之间做选择：

- hypothesis 1（假设 1）：$X$ 由分布 $p = (p_1, \ldots, p_n)$ 生成；
- hypothesis 2（假设 2）：$X$ 由分布 $q = (q_1, \ldots, q_n)$ 生成。

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">**randomized detector（随机化检测器）：非负矩阵 $T \in \mathbb{R}^{2 \times n}$，满足 $\mathbf{1}^T T = \mathbf{1}^T$（即每一列的元素之和为 1）。**</span>

- 若观测到 $X = k$，则以概率 $t_{1k}$ 选择假设 1，以概率 $t_{2k}$ 选择假设 2；
- 若 $T$ 的所有元素都取 0 或 1，则称为 deterministic detector（确定性检测器）。

<!-- p.8 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">detection probability matrix 与 multicriterion formulation（检测概率矩阵与多准则形式）</span>

detection probability matrix（检测概率矩阵）：

```math
D = \begin{bmatrix} Tp & Tq \end{bmatrix} =
\begin{bmatrix}
1 - P_{\mathrm{fp}} & P_{\mathrm{fn}} \\
P_{\mathrm{fp}} & 1 - P_{\mathrm{fn}}
\end{bmatrix}
```

- $P_{\mathrm{fp}}$：当 $X$ 由分布 1 生成时却选择假设 2 的概率，即 false positive（假阳性）；
- $P_{\mathrm{fn}}$：当 $X$ 由分布 2 生成时却选择假设 1 的概率，即 false negative（假阴性）。

<span style="color:#2471a3;">**[section]**</span> multicriterion formulation of detector design（检测器设计的多准则形式）：

```math
\begin{aligned}
\text{minimize (w.r.t. } \mathbb{R}_+^2\text{)} \quad & (P_{\mathrm{fp}}, P_{\mathrm{fn}}) = \left((Tp)_2,\ (Tq)_1\right) \\
\text{subject to} \quad & t_{1k} + t_{2k} = 1, \quad k = 1, \ldots, n \\
& t_{ik} \ge 0, \quad i = 1, 2, \quad k = 1, \ldots, n
\end{aligned}
```

变量为 $T \in \mathbb{R}^{2 \times n}$。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 假设检验可以无缝接到 Bertsekas 概率论里的经典内容。$P_{\mathrm{fp}}$ 与 $P_{\mathrm{fn}}$ 就是数理统计中的第一类、第二类错误概率：分布 1 为真却判成 2，或者分布 2 为真却判成 1。本页的处理方式是本书一贯的多准则思路：不人为指定哪个错误更重要，而是求 $(P_{\mathrm{fp}}, P_{\mathrm{fn}})$ 在 $\mathbb{R}_+^2$ 序下的 Pareto 最优面，再用标量化逐点取回——这与用加权求和处理多目标优化的做法完全一致。下一页会看到：固定权重 $\lambda$ 时的最优检测器恰是似然比检验，与 Neyman–Pearson 引理的精神相通；若把 $\lambda$ 解释成两个假设的先验概率之比，它又变成贝叶斯派的 MAP 检测。一个参数 $\lambda$ 串起频率派与贝叶斯派两种视角，这也解释了随机化检测器为何允许 $t_{1k}$ 取 0 到 1 之间的中间值。

<!-- p.9 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">scalarization 与 minimax detector（标量化与极小极大检测器）</span>

scalarization（标量化，权重 $\lambda > 0$）：

```math
\begin{aligned}
\text{minimize} \quad & (Tp)_2 + \lambda (Tq)_1 \\
\text{subject to} \quad & t_{1k} + t_{2k} = 1, \quad t_{ik} \ge 0, \quad i = 1, 2, \quad k = 1, \ldots, n
\end{aligned}
```

这是一个 LP，且具有简单的解析解。

<span style="color:#2471a3;">**[theorem]**</span> <span style="color:#c0392b;">解析解：由 likelihood ratio test（似然比检验）给出的确定性检测器</span>

```math
(t_{1k}, t_{2k}) =
\begin{cases}
(1, 0), & p_k \ge \lambda q_k \\
(0, 1), & p_k < \lambda q_k
\end{cases}
```

- 若对某个 $k$ 有 $p_k = \lambda q_k$，则任何满足 $0 \le t_{1k} \le 1$、$t_{1k} = 1 - t_{2k}$ 的取值都最优（也就是说，Pareto-optimal detectors（Pareto 最优检测器）中包含非确定性检测器）。

<span style="color:#2471a3;">**[section]**</span> minimax detector（极小极大检测器）：

```math
\begin{aligned}
\text{minimize} \quad & \max\{P_{\mathrm{fp}}, P_{\mathrm{fn}}\} = \max\{(Tp)_2,\ (Tq)_1\} \\
\text{subject to} \quad & t_{1k} + t_{2k} = 1, \quad t_{ik} \ge 0, \quad i = 1, 2, \quad k = 1, \ldots, n
\end{aligned}
```

同样是一个 LP；其解通常不是确定性的。

<!-- p.10 -->

### <span style="color:#2471a3;">**[example]**</span> <span style="color:#c0392b;">example（数值例）</span>

两个分布 $p$、$q$ 构成矩阵 $P$ 的两列（$n = 4$）：

```math
P =
\begin{bmatrix}
0.70 & 0.10 \\
0.20 & 0.10 \\
0.05 & 0.70 \\
0.05 & 0.10
\end{bmatrix}
```

图为 $(P_{\mathrm{fp}}, P_{\mathrm{fn}})$ 平面（两轴范围均为 0 到 1）上的解：解 1、2、3（以及两端点）是确定性的；4 是极小极大检测器。

<!-- p.11 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Experiment design（实验设计）</span>

对未知量 $x \in \mathbb{R}^n$ 进行 $m$ 次线性测量 $y_i = a_i^T x + w_i,\ i = 1, \ldots, m$：

- 测量误差 $w_i$ 为 IID 的 $N(0, 1)$；
- ML（最小二乘）估计为

```math
\hat{x} = \left(\sum_{i=1}^{m} a_i a_i^T\right)^{-1} \sum_{i=1}^{m} y_i a_i
```

- 误差 $e = \hat{x} - x$ 均值为零，协方差为

```math
E = \mathbb{E}\, e e^T = \left(\sum_{i=1}^{m} a_i a_i^T\right)^{-1}
```

confidence ellipsoids（置信椭球）由 $\{x \mid (x - \hat{x})^T E^{-1} (x - \hat{x}) \le \beta\}$ 给出。

experiment design（实验设计）：选取 $a_i \in \{v_1, \ldots, v_p\}$（一组候选的 test vectors（测试向量）），使 $E$ 尽可能"小"。

<!-- p.12 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">vector optimization formulation 与 relaxed experiment design（向量优化形式与松弛实验设计）</span>

向量优化形式：

```math
\begin{aligned}
\text{minimize (w.r.t. } S_+^n\text{)} \quad & E = \left(\sum_{k=1}^{p} m_k v_k v_k^T\right)^{-1} \\
\text{subject to} \quad & m_k \ge 0, \quad m_1 + \cdots + m_p = m, \quad m_k \in \mathbb{Z}
\end{aligned}
```

- 变量为 $m_k$（取值等于 $v_k$ 的测量向量 $a_i$ 的个数）；
- 由于含整数约束，该问题一般难以求解。

relaxed experiment design（松弛实验设计）：设 $m \gg p$，以 $\lambda_k = m_k / m$ 为（连续）实变量：

```math
\begin{aligned}
\text{minimize (w.r.t. } S_+^n\text{)} \quad & E = (1/m)\left(\sum_{k=1}^{p} \lambda_k v_k v_k^T\right)^{-1} \\
\text{subject to} \quad & \lambda \succeq 0, \quad \mathbf{1}^T \lambda = 1
\end{aligned}
```

- 常用标量化（common scalarizations）：最小化 $\log \det E$、$\mathrm{tr}\, E$、$\lambda_{\max}(E)$ 等；
- 可以添加其他凸约束，例如限制实验成本 $c^T \lambda \le B$。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 实验设计把"该做哪些实验"变成了一个矩阵大小问题。矩阵 $\sum_k m_k v_k v_k^T$ 是估计的信息矩阵，其逆 $E$ 是误差协方差；衡量 $E$ 的大小有三种常见口径——迹（A-最优）、行列式（D-最优）、最大特征值（E-最优）——它们正是 Strang 线代中特征值、行列式与正定性一章的三个标量摘要。注意 $\lambda \succeq 0$、$\mathbf{1}^T\lambda = 1$ 的约束保证了信息矩阵半正定，$E$ 才有意义。原始问题带整数约束 $m_k\in\mathbb{Z}$，属于组合优化，一般不存在多项式时间算法；把它松弛成连续比例 $\lambda_k = m_k/m$ 在 $m \gg p$ 时近似合理——这与 6.006 中"整数规划难解、LP 松弛给出近似解或界"的思想同源，只是这里松弛后得到的是凸问题，还能顺势塞进工程约束，例如实验成本上限 $c^T\lambda \le B$，让统计精度与预算同台协商。

<!-- p.13 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">D-optimal design（D-最优设计）</span>

D-最优设计（D-optimal design）：

```math
\begin{aligned}
\text{minimize} \quad & \log \det \left(\sum_{k=1}^{p} \lambda_k v_k v_k^T\right)^{-1} \\
\text{subject to} \quad & \lambda \succeq 0, \quad \mathbf{1}^T \lambda = 1
\end{aligned}
```

interpretation（解释）：最小化置信椭球的体积。

对偶问题（dual problem）：

```math
\begin{aligned}
\text{maximize} \quad & \log \det W + n \log n \\
\text{subject to} \quad & v_k^T W v_k \le 1, \quad k = 1, \ldots, p
\end{aligned}
```

interpretation（解释）：$\{x \mid x^T W x \le 1\}$ 是以原点为中心、包含所有测试向量 $v_k$ 的 minimum volume ellipsoid（最小体积椭球）。

<span style="color:#2471a3;">**[theorem]**</span> <span style="color:#c0392b;">complementary slackness（互补松弛）</span>：设 $\lambda$、$W$ 分别为原问题与对偶问题的最优解，则

```math
\lambda_k \left(1 - v_k^T W v_k\right) = 0, \quad k = 1, \ldots, p
```

即最优实验只使用位于 $W$ 所定义椭球边界上的向量 $v_k$。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这是本讲最漂亮的对偶几何。原问题生活在统计世界：最小化置信椭球的体积；对偶问题却生活在纯几何世界：寻找以原点为中心、罩住全部测试向量 $v_k$ 的最小体积椭球。互补松弛条件 $\lambda_k(1 - v_k^T W v_k) = 0$ 随之变成一句设计守则：若某个测试向量被真正使用（$\lambda_k > 0$），它就必须落在最优椭球的边界上（$v_k^T W v_k = 1$）；躺在椭球内部的向量对缩小椭球毫无贡献，不值得测量。回忆 Strang 线代：椭球体积由形状矩阵行列式的倒数控制，所以 $\log\det$ 目标是在同时照看所有方向的半轴乘积，不像迹只管平均值、也不像 $\lambda_{\max}$ 只管最坏方向。对偶性在这里不只是求解工具，更是一把把统计最优性翻译成几何语言的词典——这正是第 5 讲对偶解释思想的又一次登场。

<!-- p.14 -->

### <span style="color:#2471a3;">**[example]**</span> <span style="color:#c0392b;">example（$p = 20$ 的数值例）</span>

图中解为 $\lambda_1 = 0.5$、$\lambda_2 = 0.5$：设计只使用两个向量，它们位于最优 $W$ 所定义的椭圆的边界上。

<!-- p.15 -->

### <span style="color:#2471a3;">**[solution]**</span> <span style="color:#c0392b;">derivation of dual（第 7–13 页对偶问题的推导）</span>

> 注：原文标题写作「page 1–13」，按上下文应指第 7–13 页。以下为推导过程。
>
> 首先引入新变量 $X$，把原问题重新表述为：
>
> ```math
> \begin{aligned}
> \text{minimize} \quad & \log \det X^{-1} \\
> \text{subject to} \quad & X = \sum_{k=1}^{p} \lambda_k v_k v_k^T, \quad \lambda \succeq 0, \quad \mathbf{1}^T \lambda = 1
> \end{aligned}
> ```
>
> Lagrangian（拉格朗日函数）：
>
> ```math
> L(X, \lambda, Z, z, \nu) = \log \det X^{-1} + \mathrm{tr}\left(Z\left(X - \sum_{k=1}^{p} \lambda_k v_k v_k^T\right)\right) - z^T \lambda + \nu\left(\mathbf{1}^T \lambda - 1\right)
> ```
>
> - 对 $X$ 极小化：令梯度为零，得 $-X^{-1} + Z = 0$；
> - 对 $\lambda_k$ 的极小值为 $-\infty$，除非 $-v_k^T Z v_k - z_k + \nu = 0$。
>
> 由此得到对偶问题（dual problem）：
>
> ```math
> \begin{aligned}
> \text{maximize} \quad & n + \log \det Z - \nu \\
> \text{subject to} \quad & v_k^T Z v_k \le \nu, \quad k = 1, \ldots, p
> \end{aligned}
> ```
>
> 作变量替换 $W = Z / \nu$，再对 $\nu$ 优化，即得第 7–13 页的对偶问题。
