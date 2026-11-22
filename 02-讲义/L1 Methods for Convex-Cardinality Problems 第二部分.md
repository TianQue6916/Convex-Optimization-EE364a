# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

> <span style="color:#7f8c8d;">Source: 补充材料/L1 Methods for Convex-Cardinality Problems 第二部分.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

## ℓ1-norm Methods for Convex-Cardinality Problems, Part II（凸基数问题的 ℓ1 范数方法·第二部分）

> <span style="color:#7f8c8d;">元信息：本文为 Stanford EE364b 课堂 slides，每页页脚均为 “EE364b, Stanford University”，正文 slide 编号 1–20 在各节末尾以灰色脚注保留。</span>

<!-- p.1 -->

<span style="color:#2471a3;">**[lecture]**</span>

<span style="color:#c0392b;">**ℓ1-norm Methods for Convex-Cardinality Problems, Part II（凸基数问题的 ℓ1 范数方法·第二部分）**</span>

- 本讲主题：
  - total variation（全变差）
  - iterated weighted ℓ1 heuristic（迭代加权 ℓ1 启发式）
  - matrix rank constraints（矩阵秩约束）
- 主讲机构：EE364b, Stanford University

<!-- p.2 -->

### <span style="color:#c0392b;">Total variation reconstruction（全变差重构）</span>

<span style="color:#2471a3;">**[section]**</span> 核心想法：用**分段常数**（piecewise constant）的 $\hat{x}$ 去拟合受损信号 $x_{\mathrm{cor}}$，跳变（jump）数不超过 $k$。

- 这是一个**凸基数问题**（convex-cardinality problem）：minimize $\|\hat{x} - x_{\mathrm{cor}}\|_2$，subject to $\mathrm{card}(Dx) \le k$（其中 $D$ 是一阶差分矩阵（first-order difference matrix））。
- 启发式（heuristic）：minimize $\|\hat{x} - x_{\mathrm{cor}}\|_2 + \gamma\|Dx\|_1$；通过调节 $\gamma$ 来调整跳变的数量。
- $\|Dx\|_1$ 就是信号 $\hat{x}$ 的**全变差**（total variation）。
- 因此该方法被称为**全变差重构**（total variation reconstruction, TVR）。
- 与基于 $\ell_2$ 的重构不同：TVR 在滤除高频噪声的同时，还能保留尖锐的跳变。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 为什么把 $\ell_2$ 换成 $\|Dx\|_1$ 就能“保边去噪”？从第一性原理看：$\ell_2$ 项惩罚的是误差的平方，一个大的重构误差其平方代价远高于把它摊成许多小斜坡，所以最小二乘天然倾向于“抹平”尖锐变化——这正是你在概率论里见过的高斯噪声下最大似然估计的必然行为（高斯似然等价于平方误差项）。而 $\|Dx\|_1$ 对差分只取绝对值再求和，代价随跳变高度近似线性增长，于是优化器宁可保留少数几个干净的大跳变，也不愿制造大量小抖动来“稀释”误差。这与 Strang 18.065 里讲的稀疏性与压缩感知是同一件事：$\ell_1$ 诱导稀疏解，只不过这里“稀疏”的对象是信号的差分（即跳变位置稀疏），数学上恰好等价于“分段常数”这一结构先验。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 1</span>

<!-- p.3 -->

### <span style="color:#c0392b;">Example（例，BV book §6.3.3）</span>

<span style="color:#2471a3;">**[example]**</span> 信号 $x \in \mathbb{R}^{2000}$ 与受损信号 $x_{\mathrm{cor}} \in \mathbb{R}^{2000}$。

图示说明：单幅曲线图，横轴为索引 $i$（0–2000），纵轴为幅值（−2–2），图例同时画出原始信号 $x$ 与受损信号 $x_{\mathrm{cor}}$（本例取自 BV 教材 §6.3.3）。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 2</span>

<!-- p.4 -->

### <span style="color:#c0392b;">Total variation reconstruction for three values of γ（三个 γ 取值下的全变差重构）</span>

<span style="color:#2471a3;">**[section]**</span> 图示说明：三幅并排曲线图，均为全变差重构结果 $\hat{x}$ 随索引 $i$（0–2000）的曲线，纵轴范围 −2–2；三幅图对应三个不同的 $\gamma$ 取值。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 3</span>

<!-- p.5 -->

### <span style="color:#c0392b;">ℓ2 reconstruction for three values of γ（三个 γ 取值下的 ℓ2 重构）</span>

<span style="color:#2471a3;">**[section]**</span> 图示说明：三幅并排曲线图，均为 $\ell_2$ 重构结果 $\hat{x}$（横轴 $i$：0–2000，纵轴：−2–2），对应三个不同 $\gamma$ 取值。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 4</span>

<!-- p.6 -->

### <span style="color:#c0392b;">Example: 2D total variation reconstruction（例：二维全变差重构）</span>

<span style="color:#2471a3;">**[example]**</span> 把全变差思想搬到二维图像：

- $x \in \mathbb{R}^n$ 是 $N \times N$ 网格上的像素值（$N = 31$，故 $n = 961$）。
- 假设：$x$ 只有相对较少的取值大变化（即边界，boundaries）。
- 我们有 $m = 120$ 个线性测量：$y = Fx$（$F_{ij} \sim \mathcal{N}(0, 1)$）。
- 写成凸基数问题：

```math
\begin{array}{ll}
\text{minimize} & \mathrm{card}(x_{i,j} - x_{i+1,j}) + \mathrm{card}(x_{i,j} - x_{i,j+1}) \\
\text{subject to} & y = Fx
\end{array}
```

- $\ell_1$ 启发式（目标函数是全变差的二维版本）：

```math
\begin{array}{ll}
\text{minimize} & \sum_{i,j} \left( |x_{i,j} - x_{i+1,j}| + |x_{i,j} - x_{i,j+1}| \right) \\
\text{subject to} & y = Fx
\end{array}
```

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 5</span>

<!-- p.7 -->

### <span style="color:#c0392b;">TV reconstruction（全变差重构结果）</span>

<span style="color:#2471a3;">**[section]**</span> 图示说明：两幅并排图像，左为 original（原始图像），右为 TV reconstruction（全变差重构）；网格坐标约 0–30 × 0–35，色标 −0.5–1.5。原图注：……在变量数比测量数多 8 倍的情况下，这结果不算差！

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 6</span>

<!-- p.8 -->

### <span style="color:#c0392b;">ℓ2 reconstruction（ℓ2 重构结果）</span>

<span style="color:#2471a3;">**[section]**</span> 图示说明：两幅并排图像，左为 original（原始图像），右为 ℓ2 reconstruction（$\ell_2$ 重构），坐标设置与上一页相同。原图注：……当变量数比测量数多 8 倍时，这就是你预期的结果。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 7</span>

<!-- p.9 -->

### <span style="color:#c0392b;">Iterated weighted ℓ1 heuristic（迭代加权 ℓ1 启发式）</span>

<span style="color:#2471a3;">**[algorithm]**</span> 目标：在 $x \in \mathcal{C}$ 上最小化 $\mathrm{card}(x)$。迭代格式（伪代码保持原文）：

```
w := 1
repeat
    minimize ||diag(w)x||_1 over x ∈ C
    w_i := 1/(ε + |x_i|)
```

- 第一次迭代就是基本 ℓ1 启发式（basic $\ell_1$ heuristic）。
- 它增大了小分量 $x_i$ 的相对权重。
- 通常 5 步或更少即收敛。
- 相比基本 ℓ1 启发式，常有适度的改进（即 $\mathrm{card}(x)$ 的下降）。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 8</span>

<!-- p.10 -->

### <span style="color:#c0392b;">Interpretation（解释）</span>

<span style="color:#2471a3;">**[section]**</span> 这一页解释迭代加权 ℓ1 到底在近似什么：

- 不失一般性（wlog）可取 $x \succeq 0$：把 $x$ 写成 $x = x^+ - x^-$，其中 $x^+, x^- \succeq 0$，并把 $\mathrm{card}(x)$ 替换为 $\mathrm{card}(x^+) + \mathrm{card}(x^-)$。
- 我们采用近似 $\mathrm{card}(z) \approx \log(1 + z/\epsilon)$，其中 $\epsilon > 0$，$z \in \mathbb{R}_+$。
- 代入该近似，得到（非凸）问题：

```math
\begin{array}{ll}
\text{minimize} & \sum_{i=1}^n \log(1 + x_i/\epsilon) \\
\text{subject to} & x \in \mathcal{C}, \quad x \succeq 0
\end{array}
```

- 在当前点对目标函数线性化（linearize）：

```math
\sum_{i=1}^n \log(1 + x_i/\epsilon) \approx \sum_{i=1}^n \log(1 + x_i^{(k)}/\epsilon) + \sum_{i=1}^n \frac{x_i - x_i^{(k)}}{\epsilon + x_i^{(k)}}
```

<!-- p.11 -->

### <span style="color:#c0392b;">Interpretation (cont.)（解释·续：凸子问题与迭代）</span>

<span style="color:#2471a3;">**[section]**</span> 接上页：舍去线性化中的常数项，求解所得凸问题

```math
\begin{array}{ll}
\text{minimize} & \sum_{i=1}^n w_i x_i \\
\text{subject to} & x \in \mathcal{C}, \quad x \succeq 0
\end{array}
```

其中 $w_i = 1/(\epsilon + x_i)$，其解即下一个迭代点（next iterate）。

- 重复上述过程直至收敛，即得到一个局部解（local solution）。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这两页揭示了迭代加权 ℓ1 的本质：$\log(1 + z/\epsilon)$ 在 $z = 0$ 附近斜率约为 $1/\epsilon$、非常陡峭，这迫使解把尽可能多的分量压到恰好为零，于是它成了基数 $\mathrm{card}$ 的光滑代理函数。每轮迭代在当前点做一阶泰勒展开、再解一个凸问题——这与你在 EE364A 牛顿法一讲中“在当前点取局部线性/二次近似后走一步”的思路同构，也可以纳入 MM（majorization-minimization，极大化-极小化）框架来理解：每步解的是原非凸目标的一个局部代理。再补一个 6.006 视角：$\mathrm{card}$ 最小化是组合优化问题，大规模时没有已知的多项式时间算法；而这里的每个子问题（如 LP）都是多项式时间可解的，整个方法就是“用一串可解的凸问题去逼近一个不可解的组合问题”。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 10</span>

<!-- p.12 -->

### <span style="color:#c0392b;">Sparse solution of linear inequalities（线性不等式组的稀疏解）</span>

<span style="color:#2471a3;">**[example]**</span>

- 在多面体（polyhedron）$\{x \mid Ax \preceq b\}$（$A \in \mathbb{R}^{100 \times 50}$）上最小化 $\mathrm{card}(x)$。
- ℓ1 启发式找到 $x \in \mathbb{R}^{50}$，$\mathrm{card}(x) = 44$。
- 迭代加权 ℓ1 启发式找到 $\mathrm{card}(x) = 36$ 的 $x$。
- （通过分支定界（branch & bound）求出的全局解为 $\mathrm{card}(x) = 32$。）

图示说明：横轴为迭代次数 iteration（1–6），纵轴为 $\mathrm{card}(x)$（0–50）；两条曲线分别为 iterated ℓ1（迭代加权 ℓ1）与 ℓ1（基本启发式），后者为水平直线。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 11</span>

<!-- p.13 -->

### <span style="color:#c0392b;">Detecting changes in time series model（检测时间序列模型中的变化）</span>

<span style="color:#2471a3;">**[section]**</span>

- AR(2)（二阶自回归）标量时间序列模型：$y(t+2) = a(t)\,y(t+1) + b(t)\,y(t) + v(t)$，其中 $v(t)$ 为独立同分布（IID）的 $\mathcal{N}(0, 0.5^2)$ 噪声。
- 假设：$a(t)$ 与 $b(t)$ 分段常数（piecewise constant），很少发生变化。
- 给定 $y(t)$，$t = 1, \ldots, T$，估计 $a(t), b(t)$，$t = 1, \ldots, T - 2$。
- 启发式：在变量 $a(t), b(t)$，$t = 1, \ldots, T - 1$ 上最小化：

```math
\sum_{t=1}^{T-2} \bigl( y(t+2) - a(t)y(t+1) - b(t)y(t) \bigr)^2 + \gamma \sum_{t=1}^{T-2} \bigl( |a(t+1) - a(t)| + |b(t+1) - b(t)| \bigr)
```

- 调节 $\gamma$，在拟合精度与 $a, b$ 的变化次数之间做权衡（trade off）。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 12</span>

<!-- p.14 -->

### <span style="color:#c0392b;">Time series and true coefficients（时间序列与真实系数）</span>

<span style="color:#2471a3;">**[section]**</span> 图示说明：两幅并排曲线图。一幅为时间序列 $y(t)$ 随 $t$（0–300）的曲线，纵轴 −3–3；另一幅为真实系数 $a(t)$ 与 $b(t)$ 随 $t$（50–300）的曲线，纵轴 −1–1。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 13</span>

<!-- p.15 -->

### <span style="color:#c0392b;">TV heuristic and iterated TV heuristic（TV 启发式与迭代 TV 启发式）</span>

<span style="color:#2471a3;">**[section]**</span> 图示说明：左图为 TV 启发式结果，$\gamma = 10$；右图为迭代 TV 结果，共 5 次迭代，$\epsilon = 0.005$。两图均为估计系数随 $t$（50–300）的曲线，纵轴 −1–1。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 14</span>

<!-- p.16 -->

### <span style="color:#c0392b;">Extension to matrices（推广到矩阵）</span>

<span style="color:#2471a3;">**[section]**</span> 从向量走向矩阵：

- **秩**（Rank）是矩阵情形下 $\mathrm{card}$ 的自然类似物。
- **凸-秩问题**（convex-rank problem）：除目标函数或约束中出现的 Rank 之外，其余部分都是凸的。
- 当矩阵为对角阵时，秩问题退化为基数问题：$\mathrm{Rank}(\mathrm{diag}(x)) = \mathrm{card}(x)$。
- ℓ1 启发式的矩阵类似物：使用**核范数**（nuclear norm）$\|X\|_* = \sum_i \sigma_i(X)$（奇异值之和；它是谱范数（spectral norm）的对偶范数（dual norm））。
- 当 $X \succeq 0$ 时，核范数退化为 $\mathrm{Tr}\, X$（正如 $x \succeq 0$ 时 $\|x\|_1$ 退化为 $1^T x$）。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 核范数是本讲最漂亮的“升维”操作，直接对应 Strang 线代里的 SVD：矩阵的“非零元素个数”不再按分量数，而按奇异值 $\sigma_i$ 来数，于是基数 card 对应秩 Rank，$\ell_1$ 范数对应核范数 $\|X\|_*$。为什么核范数是秩的最佳凸代理？直觉与 $\ell_1$ 完全同理：把“数非零奇异值的个数”换成“对所有奇异值求和”，会偏好只含少数大奇异值的（低秩）解。又由 SVD 的性质，半正定矩阵的奇异值就是其特征值，所以 $X \succeq 0$ 时核范数恰好等于迹 $\mathrm{Tr}\,X$——这与 $x \succeq 0$ 时 $\|x\|_1 = 1^T x$ 完全平行，一一对齐。最后从 6.006 的复杂度视角看：秩最小化同样是 NP-hard 的组合问题，迹启发式（下一页起）就是它多项式时间可解的凸松弛。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 15</span>

<!-- p.17 -->

### <span style="color:#c0392b;">Factor modeling（因子建模）</span>

<span style="color:#2471a3;">**[section]**</span>

- 给定矩阵 $\Sigma \in S^n_{+}$，求形如 $\hat{\Sigma} = FF^T + D$ 的近似，其中 $F \in \mathbb{R}^{n \times r}$，$D$ 为非负对角阵。
- 这给出一个潜在因子模型（factor model，含 $r$ 个因子）：$x = Fz + v$，$v \sim \mathcal{N}(0, D)$，$z \sim \mathcal{N}(0, I)$。
- 因子数最少的模型：

```math
\begin{array}{ll}
\text{minimize} & \mathrm{Rank}\, X \\
\text{subject to} & X \succeq 0, \quad D \succeq 0 \ \text{diagonal} \\
 & X + D \in \mathcal{C}
\end{array}
```

- 变量为 $D, X \in S^n$。其中 $\mathcal{C}$ 是 $\Sigma$ 的“可接受近似”构成的凸集。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 16</span>

<!-- p.18 -->

### <span style="color:#c0392b;">Factor modeling (cont.)（因子建模·续：KL 散度与迹启发式）</span>

<span style="color:#2471a3;">**[section]**</span>

- 例如，用 KL 散度（KL divergence）刻画“可接受近似”：

```math
\mathcal{C} = \left\{ \hat{\Sigma} \,\middle|\, -\log\det(\Sigma^{-1/2}\hat{\Sigma}\Sigma^{-1/2}) + \mathrm{Tr}(\Sigma^{-1/2}\hat{\Sigma}\Sigma^{-1/2}) - n \le \epsilon \right\}
```

- **迹启发式**（trace heuristic）：

```math
\begin{array}{ll}
\text{minimize} & \mathrm{Tr}\, X \\
\text{subject to} & X \succeq 0, \quad D \succeq 0 \ \text{diagonal} \\
 & X + D \in \mathcal{C}
\end{array}
```

- 变量为 $d \in \mathbb{R}^n$，$X \in S^n$。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 17</span>

<!-- p.19 -->

### <span style="color:#c0392b;">Example（例：因子模型的迹启发式）</span>

<span style="color:#2471a3;">**[example]**</span>

- $x = Fz + v$，$z \sim \mathcal{N}(0, I)$，$v \sim \mathcal{N}(0, D)$，$D$ 为对角阵；$F \in \mathbb{R}^{20 \times 3}$。
- $\Sigma$ 是由 $N = 3000$ 个样本得到的经验协方差矩阵（empirical covariance matrix）。
- 可接受近似集合：

```math
\mathcal{C} = \left\{ \hat{\Sigma} \,\middle|\, \|\Sigma^{-1/2}(\hat{\Sigma} - \Sigma)\Sigma^{-1/2}\| \le \beta \right\}
```

- 迹启发式：

```math
\begin{array}{ll}
\text{minimize} & \mathrm{Tr}\, X \\
\text{subject to} & X \succeq 0, \quad d \succeq 0 \\
 & \|\Sigma^{-1/2}(X + \mathrm{diag}(d) - \Sigma)\Sigma^{-1/2}\| \le \beta
\end{array}
```

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 18</span>

<!-- p.20 -->

### <span style="color:#c0392b;">Trace approximation results（迹近似结果）</span>

<span style="color:#2471a3;">**[section]**</span> 图示说明：两幅并排对数坐标图。左图为 $\mathrm{Rank}(X)$ 随 $\beta$（$10^{-2}$–$10^{0}$）的变化曲线，纵轴 2–16；右图为 $X$ 的特征值 $\lambda_i(X)$ 随 $\beta$ 的变化，纵轴 $10^{-4}$–$10^{2}$。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 19</span>

<!-- p.21 -->

### <span style="color:#c0392b;">Results（结果：β = 0.1357 处的检验）</span>

<span style="color:#2471a3;">**[section]**</span>

- 对 $\beta = 0.1357$（权衡曲线（tradeoff curve）的拐点，knee），我们发现：
  - $\angle(\mathrm{range}(X),\, \mathrm{range}(FF^T)) = 6.8^\circ$
  - $\|d - \mathrm{diag}(D)\| \,/\, \|\mathrm{diag}(D)\| = 0.07$
- 也就是说：我们从经验协方差中恢复出了（潜在的）因子模型。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这页是“盲恢复”式的漂亮收官。可接受近似集合用 KL 散度定义——你在概率论里见过它作为相对熵，衡量两个分布的差异；这里 $\Sigma^{-1/2}\hat{\Sigma}\Sigma^{-1/2}$ 相当于把 $\hat{\Sigma}$ 按 $\Sigma$ 的几何“白化”后再与单位阵比对，偏离程度即两者的统计距离。$\beta = 0.1357$ 取在权衡曲线的拐点：$\beta$ 太小会过拟合样本噪声（3000 个样本终究有限，经验协方差本身带估计误差），太大则会把真实因子抹掉——这正是正则化路径上挑“肘部”的标准操作。结果本身用 Strang 线代的语言解读最清楚：$\angle(\mathrm{range}(X), \mathrm{range}(FF^T)) = 6.8^\circ$ 说明恢复出的列空间与真值的列空间几乎对齐（主角度极小）；对角项相对误差仅 0.07，说明特异性方差 $D$ 也被准确识别。

> <span style="color:#7f8c8d;">EE364b, Stanford University · slide 20</span>
