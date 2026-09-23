# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

> <span style="color:#7f8c8d;">Source: 补充材料/L1 Methods for Convex-Cardinality Problems 第一部分.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

## <span style="color:#2471a3;">**[lecture]**</span> <span style="color:#c0392b">$\ell_1$-norm Methods for Convex-Cardinality Problems（凸-基数问题的 $\ell_1$ 范数方法 · 第一部分）</span>

本讲义为 Stanford EE364b 的 slide 集（共 31 页），主题是 convex-cardinality problems（凸-基数问题）的 $\ell_1$ 范数启发式方法：问题来源、启发式本身、两种理论解释（凸松弛与凸包络）、应用例子与近期理论结果。

<!-- p.1 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">标题页：$\ell_1$-norm Methods for Convex-Cardinality Problems</span>

本讲标题为 $\ell_1$-norm Methods for Convex-Cardinality Problems（凸-基数问题的 $\ell_1$ 范数方法），出自 Stanford EE364b。本讲覆盖以下主题：

- problems involving cardinality（涉及基数的问题）
- the $\ell_1$-norm heuristic（$\ell_1$ 范数启发式）
- convex relaxation 与 convex envelope interpretations（凸松弛与凸包络两种解释）
- examples（应用例子）
- recent results（近期理论结果）

> <span style="color:#7f8c8d;">EE364b, Stanford University</span>

<!-- p.2 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">用于基数问题的 $\ell_1$ 范数启发式</span>

cardinality problems（基数问题）经常出现，但难以精确求解。一个依赖 $\ell_1$-norm（$\ell_1$ 范数）的简单 heuristic（启发式方法）似乎效果很好，多年来在众多领域被广泛使用：

- sparse design（稀疏设计）
- 统计学中的 LASSO 与 robust estimation（鲁棒估计）
- machine learning（机器学习）中的 support vector machine（支持向量机，SVM）
- 信号处理与地球物理中的 total variation reconstruction（全变分重构）
- compressed sensing（压缩感知）

新的理论结果保证该方法确实有效——至少对若干问题是如此。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 1</span>

<!-- p.3 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Cardinality（基数）</span>

向量 $x\in\mathbb{R}^n$ 的 cardinality（基数）记作 $\mathrm{card}(x)$，定义为其 nonzero components（非零分量）的个数。$\mathrm{card}$ 是 separable（可分离的）；对标量 $x$：

```math
\mathrm{card}(x)=\begin{cases}0, & x=0\\[2pt] 1, & x\neq 0\end{cases}
```

$\mathrm{card}$ 在 $\mathbb{R}^n_+$ 上是 quasiconcave（拟凹的），但在整个 $\mathbb{R}^n$ 上不是；因为对 $x,y\succeq 0$ 成立

$\mathrm{card}(x+y)\ \ge\ \min\{\mathrm{card}(x),\,\mathrm{card}(y)\}$

但除此之外它不再具有任何凸性性质。基数出现在许多问题中。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 为什么值得为"数非零分量个数"这么朴素的函数单开一讲？从第一性原理看，$\mathrm{card}$ 把连续向量映射到离散计数 $\{0,1,\dots,n\}$，而离散性正是凸性失效的总根源：凸分析处理的集合与函数必须"连成一片"，而稀疏向量集合 $\{x \mid \mathrm{card}(x)\le k\}$ 是碎裂的。你在 Strang 线性代数中学过子空间对加法封闭，但两个 $k$-稀疏向量的和可能完全稠密——这说明稀疏向量集连子空间都不是，更谈不上是凸集。拟凹性只在非负象限成立的原因同样朴素：$x,y\succeq 0$ 相加时，非零位置只会取并集、不会正负相消，因此基数不会下降；一旦允许异号分量，$x=(1,0)$ 与 $y=(-1,0)$ 相加得零向量，基数从 1 掉到 0，性质随即破坏。把"逐元素稀疏"换成"奇异值稀疏"，就接到 18.065（Strang 矩阵方法）的低秩近似主题——同一套 $\ell_1$ 思想迁移过去便是 nuclear norm（核范数）最小化与矩阵补全。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 2</span>

<!-- p.4 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">一般的凸-基数问题</span>

convex-cardinality problem（凸-基数问题）指的是：只要把目标函数或约束中出现的 $\mathrm{card}$ 去掉，其余部分就是凸问题的那一类问题。下面给出两个例子（其中 $C$、$f$ 均为凸；记号 minimize（最小化）与 subject to（约束条件）是优化问题的标准写法）：

- convex minimum cardinality problem（凸最小基数问题）：

```math
\begin{array}{ll}
\text{minimize} & \mathrm{card}(x)\\
\text{subject to} & x\in C
\end{array}
```

- 带基数约束（cardinality constraint）的凸问题：

```math
\begin{array}{ll}
\text{minimize} & f(x)\\
\text{subject to} & x\in C,\quad \mathrm{card}(x)\le k
\end{array}
```

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 3</span>

<!-- p.5 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">求解凸-基数问题</span>

考虑 $x\in\mathbb{R}^n$ 上的凸-基数问题：

- 如果固定 $x$ 的 sparsity pattern（稀疏模式，即哪些分量为零/非零），问题就变成一个凸问题。
- 通过求解与所有可能稀疏模式对应的 $2^n$ 个凸问题，可以解出凸-基数问题（当 $n\le 10$ 时或许还算可行；$n$ 超过 15 左右就不现实了……）。
- 一般的凸-基数问题是 NP-hard（NP-难的）。
- 可以用 branch-and-bound（分支定界）做全局求解：
  - 对特定问题实例可能奏效（需要一些运气）；
  - 最坏情况下退化为检查全部（或其中许多）$2^n$ 个稀疏模式。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 4</span>

<!-- p.6 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">作为凸-基数问题的 Boolean LP（布尔线性规划）</span>

- Boolean LP（布尔线性规划）：

```math
\begin{array}{ll}
\text{minimize} & c^Tx\\
\text{subject to} & Ax\preceq b,\quad x_i\in\{0,1\}
\end{array}
```

它涵盖许多著名的（难的）问题，例如 3-SAT 与 traveling salesman problem（旅行商问题）。

- 它可以表达为

```math
\begin{array}{ll}
\text{minimize} & c^Tx\\
\text{subject to} & Ax\preceq b,\quad \mathrm{card}(x)+\mathrm{card}(1-x)\le n
\end{array}
```

因为 $\mathrm{card}(x)+\mathrm{card}(1-x)\le n \iff x_i\in\{0,1\}$。

- 结论：一般的凸-基数问题是难的（hard）。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这一页用一行等价式把凸-基数问题钉死在复杂性理论的"难"问题上，论证链值得完整记住：Boolean LP 把变量限制在 $\{0,1\}$，它包含 3-SAT 与旅行商问题等著名难题作为特例；你在 6.006（CLRS 第 34 章）学过 3-SAT 是 NP-complete 的，且 NP 中任何问题都可多项式时间归约到它。于是若有人能多项式时间求解一般凸-基数问题，就能解出 Boolean LP，进而解出 3-SAT——与复杂性假定矛盾，故一般凸-基数问题 NP-hard。前一页提到的 branch-and-bound（分支定界）正是 6.006 中回溯搜索加剪枝思想的连续优化版：搜索树上每个节点固定一部分分量的稀疏模式，用凸松弛算出问题下界，下界一旦不优于当前最好解就整枝剪掉。这正解释了上一页的判断——特定实例靠运气可以解好，最坏情况仍免不了枚举指数多个稀疏模式。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 5</span>

<!-- p.7 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Sparse design（稀疏设计）</span>

```math
\begin{array}{ll}
\text{minimize} & \mathrm{card}(x)\\
\text{subject to} & x\in C
\end{array}
```

- 寻找满足一组 specifications（设计指标）的最稀疏 design vector（设计向量）$x$。
- $x$ 的零分量能简化设计，或者干脆对应着根本不需要的部件。
- 例子：
  - FIR filter design（FIR 滤波器设计）——零系数减少所需硬件；
  - antenna array beamforming（天线阵列波束成形）——零系数对应不需要的天线单元；
  - truss design（桁架设计）——零系数对应不需要的杆件；
  - wire sizing（布线宽度设计）——零系数对应不需要的导线。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 6</span>

<!-- p.8 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Sparse modeling / regressor selection（稀疏建模 / 回归变量选择）</span>

用 $k$ 个 regressors（回归变量，从 $n$ 个候选中选出）的线性组合来拟合向量 $b\in\mathbb{R}^m$：

```math
\begin{array}{ll}
\text{minimize} & \|Ax-b\|_2\\
\text{subject to} & \mathrm{card}(x)\le k
\end{array}
```

- 得到一个 $k$-term model（$k$ 项模型）。
- 选出（合起来）最能拟合或解释 $b$ 的 $k$ 个回归变量子集。
- 原则上可以通过枚举全部 $\binom{n}{k}$ 种选法求解。
- 变体形式：
  - 在 $\|Ax-b\|_2\le\epsilon$ 约束下最小化 $\mathrm{card}(x)$；
  - 最小化 $\|Ax-b\|_2+\lambda\,\mathrm{card}(x)$。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 7</span>

<!-- p.9 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Sparse signal reconstruction（稀疏信号重构）</span>

- 在以下条件下估计信号 $x$：
  - 带噪测量 $y=Ax+v$，其中 $v\sim\mathcal{N}(0,\sigma^2 I)$（$A$ 已知，$v$ 未知）；
  - 先验信息：$\mathrm{card}(x)\le k$。
- maximum likelihood estimate（最大似然估计）$\hat{x}_{\mathrm{ml}}$ 是下面问题的解：

```math
\begin{array}{ll}
\text{minimize} & \|Ax-y\|_2\\
\text{subject to} & \mathrm{card}(x)\le k
\end{array}
```

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 8</span>

<!-- p.10 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Estimation with outliers（带离群点的估计）</span>

- 测量模型为 $y_i=a_i^Tx+v_i+w_i,\ i=1,\dots,m$。
- 噪声 $v_i\sim\mathcal{N}(0,\sigma^2)$ 相互独立。
- 对 $w$ 的唯一假设是稀疏性：$\mathrm{card}(w)\le k$。
- $B=\{i \mid w_i\neq 0\}$ 是 bad measurements（坏测量）即 outliers（离群点）的集合。
- $x$ 的最大似然估计通过求解下述问题得到：

```math
\begin{array}{ll}
\text{minimize} & \sum_{i\notin B}(y_i-a_i^Tx)^2\\
\text{subject to} & |B|\le k
\end{array}
```

变量为 $x$ 与 $B\subseteq\{1,\dots,m\}$。

- 等价于

```math
\begin{array}{ll}
\text{minimize} & \|y-Ax-w\|_2^2\\
\text{subject to} & \mathrm{card}(w)\le k
\end{array}
```

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 9</span>

<!-- p.11 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Minimum number of violations（最少违反个数）</span>

- 给定一组凸不等式 $f_1(x)\le 0,\ \dots,\ f_m(x)\le 0,\ x\in C$。
- 选取 $x$ 使被违反的不等式个数最少：

```math
\begin{array}{ll}
\text{minimize} & \mathrm{card}(t)\\
\text{subject to} & f_i(x)\le t_i,\quad i=1,\dots,m\\
& x\in C,\quad t\succeq 0
\end{array}
```

- 判断"是否可以一个不等式都不违反"本身是一个（容易的）convex feasibility problem（凸可行性问题）。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 10</span>

<!-- p.12 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Linear classifier with fewest errors（错误数最少的线性分类器）</span>

- 给定数据 $(x_1,y_1),\dots,(x_m,y_m)\in\mathbb{R}^n\times\{-1,1\}$。
- 我们寻找线性（仿射）classifier（分类器）$y\approx\mathrm{sign}(w^Tx+v)$。
- 分类错误对应 $y_i(w^Tx_i+v)\le 0$。
- 要找给出最少分类错误的 $w,\ v$：

```math
\begin{array}{ll}
\text{minimize} & \mathrm{card}(t)\\
\text{subject to} & y_i(w^Tx_i+v)+t_i\ge 1,\quad i=1,\dots,m
\end{array}
```

变量为 $w,\ v,\ t$（这里利用了 $w$、$v$ 的 homogeneity（齐次性），即尺度可任意归一化）。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 11</span>

<!-- p.13 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Smallest set of mutually infeasible inequalities（互不相容不等式的最小子集）</span>

- 给定一组 mutually infeasible（互不相容）的凸不等式 $f_1(x)\le 0,\ \dots,\ f_m(x)\le 0$。
- 找出其中基数最小的 infeasible（不可行）子集。
- infeasibility certificate（不可行性证书）为 $g(\lambda)=\inf_x\left(\sum_{i=1}^m \lambda_i f_i(x)\right)\ge 1,\ \lambda\succeq 0$。
- 要找基数最小的不可行子集，求解

```math
\begin{array}{ll}
\text{minimize} & \mathrm{card}(\lambda)\\
\text{subject to} & g(\lambda)\ge 1,\quad \lambda\succeq 0
\end{array}
```

（假设一定的 constraint qualifications（约束品性）成立。）

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 12</span>

<!-- p.14 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Portfolio investment with linear and fixed costs（含线性成本与固定成本的投资组合投资）</span>

- 用预算 $B$ 购买股票 $i$，金额（美元）为 $x_i\ge 0$。
- 交易费由 fixed cost（固定成本）与 linear cost（线性成本）构成：$\beta\,\mathrm{card}(x)+\alpha^Tx$。
- 预算约束为 $1^Tx+\beta\,\mathrm{card}(x)+\alpha^Tx\le B$。
- 投资的 mean return（平均收益）为 $\mu^Tx$；variance（方差）为 $x^T\Sigma x$。
- 在平均收益不低于 $R_{\min}$ 的条件下最小化投资方差（风险）：

```math
\begin{array}{ll}
\text{minimize} & x^T\Sigma x\\
\text{subject to} & \mu^Tx\ge R_{\min},\quad x\succeq 0\\
& 1^Tx+\beta\,\mathrm{card}(x)+\alpha^Tx\le B
\end{array}
```

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 13</span>

<!-- p.15 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Piecewise constant fitting（分段常数拟合）</span>

- 用至多有 $k$ 个 jumps（跳变）的 piecewise constant（分段常数）信号 $\hat{x}$ 去拟合受污染信号 $x^{\mathrm{cor}}$。
- 一旦跳变的位置（下标）固定，问题就是凸的。
- $\hat{x}$ 是至多 $k$ 个跳变的分段常数信号 $\iff \mathrm{card}(D\hat{x})\le k$，其中 $D$ 为一阶差分矩阵：

```math
D=\begin{bmatrix}
1 & -1 & & & \\
 & 1 & -1 & & \\
 & & \ddots & \ddots & \\
 & & & 1 & -1
\end{bmatrix}\in\mathbb{R}^{(n-1)\times n}
```

- 写成凸-基数问题：

```math
\begin{array}{ll}
\text{minimize} & \|\hat{x}-x^{\mathrm{cor}}\|_2\\
\text{subject to} & \mathrm{card}(D\hat{x})\le k
\end{array}
```

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 14</span>

<!-- p.16 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Piecewise linear fitting（分段线性拟合）</span>

- 用至多有 $k$ 个 kinks（折点）的 piecewise linear（分段线性）信号 $\hat{x}$ 拟合 $x^{\mathrm{cor}}$。
- 写成凸-基数问题：

```math
\begin{array}{ll}
\text{minimize} & \|\hat{x}-x^{\mathrm{cor}}\|_2\\
\text{subject to} & \mathrm{card}(\nabla^2\hat{x})\le k
\end{array}
```

其中 $\nabla^2$ 为二阶差分算子：

```math
\nabla^2=\begin{bmatrix}
-1 & 2 & -1 & & \\
 & -1 & 2 & -1 & \\
 & & \ddots & \ddots & \ddots \\
 & & & -1 & 2 & -1
\end{bmatrix}
```

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 15</span>

<!-- p.17 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">$\ell_1$-norm heuristic（$\ell_1$ 范数启发式）</span>

- 用 $\gamma\|z\|_1$ 替换 $\mathrm{card}(z)$，或在目标函数中加入 regularization term（正则化项）$\gamma\|z\|_1$。
- $\gamma>0$ 是用来调节到期望稀疏度的参数（当 $\mathrm{card}$ 出现在约束中，或作为目标函数中的一项时）。
- 更精细的版本使用 $\sum_i w_i|z_i|$ 或 $\sum_i w_i(z_i)_+ + \sum_i v_i(z_i)_-$（$(z_i)_+$、$(z_i)_-$ 分别表示正部与负部），其中 $w$、$v$ 为正权重。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 为什么用 $\ell_1$ 而不是更平滑的 $\ell_2$ 来替代 $\mathrm{card}$？有两个互补的解释。几何解释：$\ell_1$ 单位球是多面体（菱形体），在坐标轴方向有尖角，而 $\ell_2$ 球处处光滑；当可行性边界从外面逼近 $\ell_1$ 球时，首次接触点大概率落在某个尖角上，而尖角处部分坐标恰好为零——稀疏解由此产生，$\ell_2$ 球的切点则几乎所有坐标都非零。概率论解释（连接你正在学的 Bertsekas 估计理论）：最小二乘损失对应高斯噪声下的最大似然，$\ell_2$ 惩罚对应高斯先验下的 MAP 估计；把惩罚换成 $\ell_1$，则对应 Laplace distribution（拉普拉斯先验），其密度在零点形成尖峰，先验上直接偏好"恰好为零"的系数。Tibshirani 1996 年提出 LASSO 时给出的正是这套统计视角，两种解释殊途同归。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 16</span>

<!-- p.18 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">例：最小基数问题</span>

- 从（难的）最小基数问题出发：

```math
\begin{array}{ll}
\text{minimize} & \mathrm{card}(x)\\
\text{subject to} & x\in C
\end{array}
```

（$C$ 为凸集。）

- 应用启发式，得到（容易的）$\ell_1$ 范数最小化问题：

```math
\begin{array}{ll}
\text{minimize} & \|x\|_1\\
\text{subject to} & x\in C
\end{array}
```

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 17</span>

<!-- p.19 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">例：基数约束问题</span>

- 从（难的）基数约束问题出发（$f$、$C$ 均凸）：

```math
\begin{array}{ll}
\text{minimize} & f(x)\\
\text{subject to} & x\in C,\quad \mathrm{card}(x)\le k
\end{array}
```

- 应用启发式，得到（容易的）$\ell_1$ 约束问题：

```math
\begin{array}{ll}
\text{minimize} & f(x)\\
\text{subject to} & x\in C,\quad \|x\|_1\le\beta
\end{array}
```

或 $\ell_1$ 正则化问题：

```math
\begin{array}{ll}
\text{minimize} & f(x)+\gamma\|x\|_1\\
\text{subject to} & x\in C
\end{array}
```

调整 $\beta$、$\gamma$ 使得 $\mathrm{card}(x)\le k$。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 18</span>

<!-- p.20 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Polishing（抛光）</span>

- 先用 $\ell_1$ 启发式找到具备所需稀疏度的 $\hat{x}$。
- 固定 $\hat{x}$ 的稀疏模式。
- 在该稀疏模式下重新求解（凸）优化问题，得到最终的（启发式）解。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 19</span>

<!-- p.21 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Interpretation as convex relaxation（作为凸松弛的解释）</span>

- 从下面的问题出发：

```math
\begin{array}{ll}
\text{minimize} & \mathrm{card}(x)\\
\text{subject to} & x\in C,\quad \|x\|_\infty\le R
\end{array}
```

- 它等价于 mixed Boolean convex problem（混合布尔凸问题）：

```math
\begin{array}{ll}
\text{minimize} & 1^Tz\\
\text{subject to} & |x_i|\le Rz_i,\quad i=1,\dots,n\\
& x\in C,\quad z_i\in\{0,1\},\ i=1,\dots,n
\end{array}
```

变量为 $x$、$z$。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 20</span>

<!-- p.22 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">凸松弛解释（续）：松弛与等价化简</span>

- 现在把 $z_i\in\{0,1\}$ 松弛（relax）为 $z_i\in[0,1]$，得到

```math
\begin{array}{ll}
\text{minimize} & 1^Tz\\
\text{subject to} & |x_i|\le Rz_i,\quad i=1,\dots,n\\
& x\in C\\
& 0\le z_i\le 1,\quad i=1,\dots,n
\end{array}
```

它等价于

```math
\begin{array}{ll}
\text{minimize} & (1/R)\|x\|_1\\
\text{subject to} & x\in C,\quad \|x\|_\infty\le R
\end{array}
```

——这正是 $\ell_1$ 启发式。

- 该问题的最优值是原问题最优值的 lower bound（下界）。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这两页值得抽象成一个可复用的模板：**精确改写 → 凸松弛 → 下界**。第一步无损：引入布尔指示变量 $z_i$，用 $|x_i|\le Rz_i$ 强制"非零分量必须被计入"，基数被精确编码为 $\mathbf{1}^Tz$。第二步有损但换来凸性：把 $z_i\in\{0,1\}$ 松弛成 $z_i\in[0,1]$。第三步收获结论：可行集扩大只会让最优值不变或更小，因此松弛问题的最优值是原问题的 lower bound（下界），这就是 $\ell_1$ 启发式在理论上的立足点。你在 6.006 学过整数规划（如 0-1 背包、最小顶点覆盖）的 LP relaxation（线性规划松弛）——同样是把 $\{0,1\}$ 松弛到 $[0,1]$，松弛值用于设计近似算法的性能保证。凸优化的特殊之处在于：这里的松弛恰好等价于一个 $\ell_1$ 问题，让"下界"同时变成了可以直接执行的算法。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 21</span>

<!-- p.23 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Interpretation via convex envelope（通过凸包络的解释）</span>

- 函数 $f$ 在集合 $C$ 上的 convex envelope（凸包络）$f^{\mathrm{env}}$，是在 $C$ 上对 $f$ 做下方估计的最大的凸函数（underestimator，指处处不超过 $f$ 的函数）。
- $\mathrm{epi}(f^{\mathrm{env}})=\mathrm{Co}(\mathrm{epi}(f))$，即凸包络的上图（epigraph）等于 $f$ 的上图的 convex hull（凸包）。
- $f^{\mathrm{env}}=(f^*)^*$（需满足若干技术性条件）。
- 对标量 $x$，$|x|$ 是 $\mathrm{card}(x)$ 在 $[-1,1]$ 上的凸包络。
- 对 $x\in\mathbb{R}^n$，$(1/R)\|x\|_1$ 是 $\mathrm{card}(x)$ 在 $\{z \mid \|z\|_\infty\le R\}$ 上的凸包络。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 22</span>

<!-- p.24 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Weighted and asymmetric $\ell_1$ heuristics（加权与非对称 $\ell_1$ 启发式）</span>

- 在凸集 $C$ 上最小化 $\mathrm{card}(x)$。
- 假设我们知道 $x_i$ 在 $C$ 上的上下界：$x\in C \implies l_i\le x_i\le u_i$（最好的界可通过求解 $2n$ 个凸问题得到）。
- 若 $u_i<0$ 或 $l_i>0$，则对所有 $x\in C$ 都有 $\mathrm{card}(x_i)=1$（即 $x_i\neq 0$）。
- 假设 $l_i<0$、$u_i>0$，凸松弛与凸包络两种解释都建议采用

```math
\sum_{i=1}^{n}\left(\frac{(x_i)_+}{u_i}+\frac{(x_i)_-}{-l_i}\right)
```

作为 $\mathrm{card}(x)$ 的 surrogate（替代函数，同时也是下界）。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 23</span>

<!-- p.25 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Regressor selection（回归变量选择）</span>

```math
\begin{array}{ll}
\text{minimize} & \|Ax-b\|_2\\
\text{subject to} & \mathrm{card}(x)\le k
\end{array}
```

- 启发式求解流程：
  - 最小化 $\|Ax-b\|_2+\gamma\|x\|_1$；
  - 找出使 $\mathrm{card}(x)\le k$ 成立的最小 $\gamma$ 值；
  - 固定相应的稀疏模式（即被选中的回归变量子集），求使 $\|Ax-b\|_2$ 最小的 $x$。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 24</span>

<!-- p.26 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Example（例：BV 教材例 6.4）</span>

- $A\in\mathbb{R}^{10\times 20}$，$x\in\mathbb{R}^{20}$，$b\in\mathbb{R}^{10}$。
- dashed curve（虚线曲线）：通过 enumeration（枚举）得到的精确最优值。
- solid curve（实线曲线）：带抛光的 $\ell_1$ 启发式。
- 图示为本例的权衡曲线：横轴为 $\mathrm{card}(x)$（范围 0 到 4），纵轴为 $\|Ax-b\|_2$（范围 0 到 10），两条曲线分别给出精确解与启发式解的拟合误差随基数的分布。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 25</span>

<!-- p.27 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Sparse signal reconstruction（稀疏信号重构）</span>

- 凸-基数问题：

```math
\begin{array}{ll}
\text{minimize} & \|Ax-y\|_2\\
\text{subject to} & \mathrm{card}(x)\le k
\end{array}
```

- $\ell_1$ 启发式：

```math
\begin{array}{ll}
\text{minimize} & \|Ax-y\|_2\\
\text{subject to} & \|x\|_1\le\beta
\end{array}
```

（称为 LASSO。）

- 另一种形式：最小化 $\|Ax-y\|_2+\gamma\|x\|_1$（称为 basis pursuit denoising（基追踪去噪））。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 26</span>

<!-- p.28 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Example（例：$\ell_1$ 重构）</span>

- 信号 $x\in\mathbb{R}^n$，$n=1000$，$\mathrm{card}(x)=30$。
- $m=200$ 个（随机）带噪测量：$y=Ax+v$，$v\sim\mathcal{N}(0,\sigma^2 I)$，$A_{ij}\sim\mathcal{N}(0,1)$。
- 左图：原始信号；右图：$\gamma=10^{-3}$ 的 $\ell_1$ 重构。
- 图示两幅子图横轴均为样本下标（1 到 1000），纵轴为幅值（$-1$ 到 $1$），对比原始稀疏信号与其 $\ell_1$ 重构结果。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 27</span>

<!-- p.29 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">例（续）：$\ell_2$ 重构对比</span>

- $\ell_2$ 重构：最小化 $\|Ax-y\|_2+\gamma\|x\|_2$，其中 $\gamma=10^{-3}$。
- 左图：原始信号；右图：$\ell_2$ 重构。
- 图示坐标范围与上一页相同；右图显示 $\ell_2$ 重构把能量分散到全部坐标上，无法还原稀疏结构——这正是 $\ell_1$ 与 $\ell_2$ 的关键差别。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 28</span>

<!-- p.30 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Some recent theoretical results（一些近期理论结果）</span>

- 设 $y=Ax$，$A\in\mathbb{R}^{m\times n}$，$\mathrm{card}(x)\le k$。
- 要重构 $x$，显然需要 $m\ge k$。
- 若 $m\ge n$ 且 $A$ 满秩（full rank），则无需基数假设也能重构 $x$。
- 那么，$\ell_1$ 启发式（在 $Ax=y$ 约束下最小化 $\|x\|_1$）何时能（精确地）重构 $x$？

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 29</span>

<!-- p.31 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Candès、Donoho、Romberg、Tao 等人的近期结果</span>

- （对某些选择 $A$）若 $m\ge (C\log n)\,k$，则 $\ell_1$ 启发式能以 overwhelming probability（压倒性概率）精确重构 $x$。
- $C$ 是 absolute constant（绝对常数）；有效的 $A$ 包括：
  - $A_{ij}\sim\mathcal{N}(0,\sigma^2)$（高斯随机矩阵）；
  - $Ax$ 给出 $x$ 在 $m$ 个频率处的 Fourier transform（傅里叶变换），这些频率从均匀分布中抽取。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这两页浓缩了 2004–2006 年应用数学界最著名的突破之一。Emmanuel Candès、Justin Romberg 与 Terence Tao 的论文 "Robust Uncertainty Principles"（IEEE Transactions on Information Theory, 2006）与 David Donoho 的论文 "Compressed Sensing"（同一期刊 2006）共同奠定了 compressed sensing（压缩感知）理论：对随机性质的测量矩阵（如高斯随机矩阵、随机频域采样），$m\ge Ck\log n$ 量级的测量足以以极高概率精确恢复 $k$-稀疏信号——测量数从 $n$ 骤降到 $k\log n$ 量级，这正是磁共振成像（MRI）加速采集与雷达欠采样的理论依据。证明的核心工具是 restricted isometry property（受限等距性质）：要求 $A$ 在所有 $2k$-稀疏向量上近似保持欧氏长度。它与你在 Strang 线代中学的"正交变换保内积"一脉相承，只是把"对全体向量保距"弱化为"只对稀疏子集保距"。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 30</span>
