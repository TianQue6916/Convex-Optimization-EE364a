> <span style="color:#7f8c8d;">Source: SEE官方讲义/06Approximation.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

## <span style="color:#2471a3;">**[lecture]**</span> <span style="color:#c0392b">6. Approximation and fitting（近似与拟合）</span>

<!-- p.1 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">章节导览：6. Approximation and fitting（近似与拟合）</span>

本讲用凸优化处理 approximation and fitting（近似与拟合）类问题，涵盖四个主题：

- **norm approximation（范数近似）**——最小化 $\|Ax-b\|$ 型的问题；
- **least-norm problems（最小范数问题）**——在满足 $Ax=b$ 的解集中找范数最小者；
- **regularized approximation（正则化近似）**——同时要求近似精度与解的"小"；
- **robust approximation（鲁棒近似）**——数据 $A$ 不确定时的近似。

---

<!-- p.2 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Norm approximation（范数近似）</span>

问题形式：

```math
\text{minimize} \quad \|Ax-b\|
```

其中 $A \in \mathbb{R}^{m\times n}$ 且 $m \ge n$（方程个数多于未知数），$\|\cdot\|$ 是 $\mathbb{R}^m$ 上的一个 norm（范数）。解 $x^\star = \operatorname{argmin}_x \|Ax-b\|$ 有三种 interpretation（解读）：

- **geometric（几何视角）**：$Ax^\star$ 是 $\mathcal{R}(A)$（range，列空间）中离 $b$ 最近的点；
- **estimation（估计视角）**：linear measurement model（线性测量模型）$y = Ax + v$——$y$ 是 measurement（测量值），$x$ 是 unknown（未知量），$v$ 是 measurement error（测量误差）；给定 $y = b$，对 $x$ 的 best guess（最佳猜测）就是 $x^\star$；
- **optimal design（最优设计视角）**：$x$ 是 design variables（设计变量，输入），$Ax$ 是 result（结果，输出）；$x^\star$ 是对期望结果 $b$ 逼近得最好的设计。

---

<!-- p.3 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">三个典型例子（examples）</span>

- **least-squares approximation（最小二乘近似）**（取 $\|\cdot\|_2$）：解满足 normal equations（正规方程）$A^TAx = A^Tb$；当 $\operatorname{rank} A = n$ 时，$x^\star = (A^TA)^{-1}A^Tb$。
- **Chebyshev approximation（切比雪夫近似）**（取 $\|\cdot\|_\infty$，最小化最大残差）：可化为 LP（linear program，线性规划）：

```math
\begin{array}{ll}
\text{minimize} & t \\
\text{subject to} & -t\mathbf{1} \preceq Ax-b \preceq t\mathbf{1}
\end{array}
```

- **sum of absolute residuals approximation（绝对残差和近似）**（取 $\|\cdot\|_1$）：同样可化为 LP：

```math
\begin{array}{ll}
\text{minimize} & \mathbf{1}^T y \\
\text{subject to} & -y \preceq Ax-b \preceq y
\end{array}
```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 为什么三种范数的命运如此不同？$\ell_2$ 范数处处可微且是二次函数，令梯度为零便得到正规方程 $A^TAx=A^Tb$，有闭式解；而 $\ell_1$ 与 $\ell_\infty$ 在某些点不可微，但它们都是分段线性函数，可以借助 epigraph（上图）技巧引入辅助变量 $t$ 或 $y$，把目标与约束全部写成线性函数，于是问题变成线性规划。你在 6.006 里见过"问题的结构决定求解代价"的思路，这里同理：LP 存在多项式时间算法（如内点法，见本课程后续讲次），所以这两类近似依然是"可有效求解"的。另一个连接在 Strang 线代：当 $\operatorname{rank}A=n$ 时 $A^TA$ 正定，正规方程的解唯一。不过 CSAPP 讲过浮点数的精度限制——直接构装 $A^TA$ 会把 condition number（条件数）平方，数值上可能很危险，因此实际软件（如 MATLAB 反斜杠、LAPACK）多用 QR 或 SVD 分解来解最小二乘，这正是"数学上的解"与"数值上稳定的解"的区别。

---

<!-- p.4 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Penalty function approximation（惩罚函数近似）</span>

更一般的写法：

```math
\begin{array}{ll}
\text{minimize} & \phi(r_1) + \cdots + \phi(r_m) \\
\text{subject to} & r = Ax - b
\end{array}
```

其中 $A \in \mathbb{R}^{m\times n}$，$\phi : \mathbb{R} \to \mathbb{R}$ 是 convex penalty function（凸惩罚函数），对每个残差分量 $r_i$ 单独计罚。三类典型惩罚函数：

- **quadratic（二次惩罚）**：$\phi(u) = u^2$；
- **deadzone-linear（死区线性惩罚）**，宽度为 $a$：$\phi(u) = \max\{0, |u| - a\}$；
- **log-barrier（对数障碍惩罚）**，极限为 $a$：

```math
\phi(u) = \begin{cases} -a^2 \log\left(1 - (u/a)^2\right), & |u| < a \\ \infty, & \text{otherwise} \end{cases}
```

（讲义原图对比三条 $\phi(u)$ 曲线（横轴 $u \in [-1.5, 1.5]$）：deadzone-linear 在死区内惩罚为零；quadratic 增长最陡；log-barrier 在 $|u| \to a$ 时发散到 $\infty$。）

---

<!-- p.5 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">示例：残差直方图（$m=100$，$n=30$）</span>

数值实验：取 $m=100$、$n=30$，分别用四种惩罚函数求解并画出 residual（残差）的 histogram（直方图）：$\phi(u)=|u|$（即 $p=1$）、$\phi(u)=u^2$（即 $p=2$）、$\phi(u)=\max\{0,|u|-a\}$（Deadzone，死区）、$\phi(u)=-\log(1-u^2)$（Log barrier，对数障碍）。

结论：**penalty function（惩罚函数）的形状对残差的 distribution（分布）有很大影响**。

---

<!-- p.6 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Huber penalty function（Huber 惩罚函数）</span>

带 parameter（参数）$M$ 的 Huber 惩罚函数：

```math
\phi_{\mathrm{hub}}(u) = \begin{cases} u^2, & |u| \le M \\ M(2|u| - M), & |u| > M \end{cases}
```

它在原点附近是二次的，而 $|u| > M$ 之后仅 linear growth（线性增长）——大残差不再被平方放大，这使得近似对 outlier（离群点）不敏感。

讲义两幅图：左图是 $M=1$ 的 Huber 惩罚曲线；右图用 quadratic（虚线）与 Huber（实线）两种惩罚，对 42 个数据点 $(t_i, y_i)$（圆圈）拟合 affine function（仿射函数）$f(t) = \alpha + \beta t$——离群点把二次拟合的直线明显拽偏，而 Huber 拟合几乎不受影响。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> Huber 惩罚是 robust statistics（鲁棒统计）的标志性成果，由 Peter Huber 于 1964 年提出。从概率论角度理解最直观：最小二乘等价于假设噪声服从高斯分布时的 maximum likelihood（最大似然）估计——高斯密度含 $e^{-v^2/2\sigma^2}$，取负对数恰好得到 $v^2$ 惩罚；但真实数据中的大残差往往来自重尾噪声或真离群点，二次惩罚会让单个离群点主导整个拟合。Huber 的设计是分段策略：小残差（$|u|\le M$）按高斯噪声处理用二次惩罚，大残差降级为线性增长，把离群点的影响从"平方级"压到"线性级"。这类按残差大小切换惩罚形式的估计方法在统计学中统称 M-estimator（M-估计量）。你可以把 $M$ 理解为"多大算离群"的阈值，它正是把先验知识注入模型的旋钮。

---

<!-- p.7 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Least-norm problems（最小范数问题）</span>

与范数近似互补的问题：

```math
\begin{array}{ll}
\text{minimize} & \|x\| \\
\text{subject to} & Ax = b
\end{array}
```

这里 $A \in \mathbb{R}^{m\times n}$ 且 $m \le n$（未知数多于方程，欠定系统），$\|\cdot\|$ 是 $\mathbb{R}^n$ 上的范数。解 $x^\star = \operatorname{argmin}_{Ax=b} \|x\|$ 的三种解读：

- **geometric（几何视角）**：$x^\star$ 是 affine set（仿射集）$\{x \mid Ax = b\}$ 上离原点 $0$ 最近的点；
- **estimation（估计视角）**：$b = Ax$ 是对 $x$ 的（完美）测量；$x^\star$ 是与测量相容的最小（"most plausible"，最合理）估计；
- **design（设计视角）**：$x$ 是设计变量（输入），$b$ 是要求的结果（输出）；$x^\star$ 是满足要求的最小（"most efficient"，最高效）设计。

---

<!-- p.8 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">最小范数问题的例子（examples）</span>

**线性方程组的 least-squares solution（最小二乘解）**（取 $\|\cdot\|_2$）：可通过 optimality conditions（最优性条件）求解：

```math
2x + A^T\nu = 0, \qquad Ax = b
```

其中 $\nu$ 是等式约束的 Lagrange multiplier（拉格朗日乘子），两式联立即可解出 $x^\star$。

**minimum sum of absolute values（绝对值和最小化）**（取 $\|\cdot\|_1$）：可化为 LP：

```math
\begin{array}{ll}
\text{minimize} & \mathbf{1}^T y \\
\text{subject to} & -y \preceq x \preceq y, \quad Ax = b
\end{array}
```

$\ell_1$ 目标倾向于产生 sparse solution（稀疏解）$x^\star$——即大量分量恰好为零。

**extension（推广）：least-penalty problem（最小惩罚问题）**：

```math
\begin{array}{ll}
\text{minimize} & \phi(x_1) + \cdots + \phi(x_n) \\
\text{subject to} & Ax = b
\end{array}
```

其中 $\phi : \mathbb{R} \to \mathbb{R}$ 是凸惩罚函数。

---

<!-- p.9 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Regularized approximation（正则化近似）</span>

一个 vector optimization（向量优化）问题——以 $\mathbb{R}^2_+$ 为序（w.r.t. $\mathbb{R}^2_+$）同时最小化两个目标：

```math
\text{minimize (w.r.t. } \mathbb{R}^2_+) \quad \left(\|Ax-b\|,\ \|x\|\right)
```

其中 $A \in \mathbb{R}^{m\times n}$，$\mathbb{R}^m$ 与 $\mathbb{R}^n$ 上的范数可以不同。interpretation（解读）：找一个近似得好（$Ax \approx b$）且 $x$ 小的解。三种动机：

- **estimation（估计）**：线性测量模型 $y = Ax + v$，并有 prior knowledge（先验知识）表明 $\|x\|$ 小；
- **optimal design（最优设计）**：小的 $x$ 更便宜或更高效；或者线性模型 $y = Ax$ 只在小 $x$ 范围内有效；
- **robust approximation（鲁棒近似）**：同样好的近似 $Ax \approx b$，$x$ 小者对 $A$ 的误差不如 $x$ 大者敏感。

---

<!-- p.10 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Scalarized problem（标量化问题）与 Tikhonov regularization（Tikhonov 正则化）</span>

用权重把双目标标量化：

```math
\text{minimize} \quad \|Ax-b\| + \gamma\|x\|
```

- $\gamma > 0$ 变动时，解点描绘出 optimal trade-off curve（最优权衡曲线）；
- 另一常见做法：最小化 $\|Ax-b\|_2^2 + \delta\|x\|_2^2$（$\delta > 0$），即 Tikhonov regularization（Tikhonov 正则化）：

```math
\text{minimize} \quad \|Ax-b\|_2^2 + \delta \|x\|_2^2
```

它可以改写成一个最小二乘问题：

```math
\text{minimize} \quad \left\| \begin{bmatrix} A \\ \sqrt{\delta}\, I \end{bmatrix} x - \begin{bmatrix} b \\ 0 \end{bmatrix} \right\|_2^2
```

闭式解为 $x^\star = (A^TA + \delta I)^{-1}A^Tb$。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> Tikhonov 正则化（在统计学习文献里又名 ridge regression，岭回归）值得从线性代数深处看一眼。Strang 告诉你 $A^TA$ 半正定，其特征值是 $A$ 奇异值的平方；当 $A$ 接近秩亏时最小特征值趋近 $0$，$(A^TA)^{-1}$ 巨大，解对 $b$ 的微小扰动极端敏感。加上 $\delta I$ 后特征值整体平移 $\delta$，最小值不再靠近零，condition number（条件数）骤降，数值上变稳——这正呼应 CSAPP 里浮点误差放大的讨论。代价是 bias（偏差）：$x^\star$ 不再精确解原问题，而是一个有偏估计，这正是概率论中经典的 bias–variance trade-off（偏差–方差权衡），$\delta$ 就是那个旋钮。历史上 Tikhonov 于 1963 年从不适定问题出发提出该方法，统计学界 Hoerl–Kennard 于 1970 年独立发展出岭回归，两者殊途同归，如今它是几乎所有正则化方法的祖师爷。

---

<!-- p.11 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Optimal input design（最优输入设计）</span>

考虑 impulse response（脉冲响应）为 $h$ 的 linear dynamical system（线性动态系统）：

```math
y(t) = \sum_{\tau=0}^{t} h(\tau)\, u(t-\tau), \qquad t = 0, 1, \ldots, N
```

input design problem（输入设计问题）是一个含 3 个目标的 multicriterion problem（多准则问题）：

1. 相对期望输出 $y_{\mathrm{des}}$ 的 tracking error（跟踪误差）：$J_{\mathrm{track}} = \sum_{t=0}^{N} (y(t) - y_{\mathrm{des}}(t))^2$
2. input magnitude（输入幅度）：$J_{\mathrm{mag}} = \sum_{t=0}^{N} u(t)^2$
3. input variation（输入变化率）：$J_{\mathrm{der}} = \sum_{t=0}^{N-1} (u(t+1) - u(t))^2$

语义：用一个 small（小）且 slowly varying（缓变）的输入信号去跟踪期望输出。regularized least-squares formulation（正则化最小二乘表述）：

```math
\text{minimize} \quad J_{\mathrm{track}} + \delta J_{\mathrm{der}} + \eta J_{\mathrm{mag}}
```

固定 $\delta, \eta$ 后，这就是关于 $u(0), \ldots, u(N)$ 的一个最小二乘问题。

---

<!-- p.12 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">示例：最优权衡曲线上的 3 个解</span>

（图示 slide）讲义在最优权衡曲线上取三个点，展示三组解（每组上图为输入 $u(t)$，下图为输出 $y(t)$）：(top) $\delta = 0$、small $\eta$（小 $\eta$）；(middle) $\delta = 0$、larger $\eta$（较大 $\eta$）；(bottom) large $\delta$（大 $\delta$）。从目标函数 $J_{\mathrm{track}} + \delta J_{\mathrm{der}} + \eta J_{\mathrm{mag}}$ 即可推断三组解的形态：$\eta$ 增大则 $J_{\mathrm{mag}}$ 权重升高，输入幅度被压小；$\delta$ 增大则 $J_{\mathrm{der}}$ 权重升高，输入被强平滑、趋于缓变；代价都是跟踪误差变大——这正是 optimal trade-off curve（最优权衡曲线）的含义。

---

<!-- p.13 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Signal reconstruction（信号重构）</span>

同样是以 $\mathbb{R}^2_+$ 为序的向量优化问题：

```math
\text{minimize (w.r.t. } \mathbb{R}^2_+) \quad \left(\|\hat{x} - x_{\mathrm{cor}}\|_2,\ \phi(\hat{x})\right)
```

- $x \in \mathbb{R}^n$ 是 unknown signal（未知信号）；
- $x_{\mathrm{cor}} = x + v$ 是 $x$ 的（已知）corrupted version（受污染版本），$v$ 为 additive noise（加性噪声）；
- 变量 $\hat{x}$（reconstructed signal，重构信号）是对 $x$ 的 estimate（估计）；
- $\phi : \mathbb{R}^n \to \mathbb{R}$ 是 regularization function（正则化函数）或 smoothing objective（平滑目标）。

两个典型例子——quadratic smoothing（二次平滑）与 total variation smoothing（全变差平滑）：

```math
\phi_{\mathrm{quad}}(\hat{x}) = \sum_{i=1}^{n-1} (\hat{x}_{i+1} - \hat{x}_i)^2, \qquad
\phi_{\mathrm{tv}}(\hat{x}) = \sum_{i=1}^{n-1} |\hat{x}_{i+1} - \hat{x}_i|
```

---

<!-- p.14 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">二次平滑示例（quadratic smoothing example）</span>

（图示 slide）横轴为采样指标 $i$（$0$ 至 $3000$ 以上），纵轴为信号幅值（约 $-0.5$ 至 $0.5$）：一幅图给出 original signal（原始信号）$x$ 与 noisy signal（带噪信号）$x_{\mathrm{cor}}$；另外三幅图给出 trade-off curve（权衡曲线）$\|\hat{x}-x_{\mathrm{cor}}\|_2$ versus $\phi_{\mathrm{quad}}(\hat{x})$ 上三个解的重构信号 $\hat{x}$。

---

<!-- p.15 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">total variation reconstruction example（全变差重构示例）</span>

（图示 slide）同样的实验设置：横轴 $i$（$0$ 至 $2000$），原始信号 $x$ 与带噪信号 $x_{\mathrm{cor}}$，以及权衡曲线上的三个重构解 $\hat{x}_i$；本页图中权衡曲线标注为 $\|\hat{x}-x_{\mathrm{cor}}\|_2$ versus $\phi_{\mathrm{quad}}(\hat{x})$。

页面结论针对二次平滑：**quadratic smoothing（二次平滑）会把噪声与信号中的 sharp transitions（陡峭突变）一并抹平**。

---

<!-- p.16 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">total variation smoothing（全变差平滑）保持陡峭突变</span>

（图示 slide）同样的图组：原始信号 $x$ 与带噪信号 $x_{\mathrm{cor}}$，权衡曲线上三个重构解 $\hat{x}$，此处权衡曲线为 $\|\hat{x}-x_{\mathrm{cor}}\|_2$ versus $\phi_{\mathrm{tv}}(\hat{x})$（横轴 $i$ 为 $0$–$2000$，纵轴约 $-2$ 至 $2$）。

结论：**total variation smoothing（全变差平滑）能保持信号中的 sharp transitions（陡峭突变）**——去噪的同时不把突变抹圆。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 二次平滑与全变差平滑的分野，本质仍是 $\ell_2$ 与 $\ell_1$ 的分野——和第 8 页"$\ell_1$ 倾向产生稀疏解"是同一个原理在信号域的翻版。看导数就明白：$\phi_{\mathrm{quad}}$ 对相邻样本差 $d_i=\hat{x}_{i+1}-\hat{x}_i$ 施加 $d_i^2$ 惩罚，梯度 $2d_i$ 随差值线性增长，大的突变代价极高，于是噪声与边缘被一起磨成缓坡；而 $\phi_{\mathrm{tv}}$ 用 $|d_i|$，边际惩罚恒为常数，只要跟踪误差的收益足够，保留少量陡峭突变反而更"划算"，重构因此呈 piecewise constant（分段常数）形态，边缘得以幸存。这一方法源自 Rudin–Osher–Fatemi 于 1992 年发表的图像去噪论文，是图像处理领域被引用最多的工作之一。你在第 8 页见过 $\ell_1$ 目标的 LP 表示（$-y \preceq x \preceq y$），这里的 $\phi_{\mathrm{tv}}$ 同样可以化为 LP 求解——"分段线性目标可线性化"是贯穿本讲的一个通用模式。

---

<!-- p.17 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Robust approximation（鲁棒近似）</span>

现在考虑 $A$ 不确定（uncertain $A$）时的 $\min \|Ax-b\|$。两种 approach（处理思路）：

- **stochastic（随机方法）**：假设 $A$ 是随机的，最小化期望残差 $\mathbb{E}\|Ax-b\|$；
- **worst-case（最坏情况方法）**：给定 $A$ 的可能取值集合 $\mathcal{A}$，最小化最坏残差 $\sup_{A\in\mathcal{A}} \|Ax-b\|$。

一般情形下不可解，仅在某些 special cases（特殊情形）下 tractable（可有效求解）——特定的范数 $\|\cdot\|$、特定的 distribution（分布）、特定的集合 $\mathcal{A}$。

示例：$A(u) = A_0 + uA_1$，

- $x_{\mathrm{nom}}$ 最小化 $\|A_0x - b\|_2^2$（nominal，名义问题）；
- $x_{\mathrm{stoch}}$ 最小化 $\mathbb{E}\|A(u)x - b\|_2^2$，其中 $u$ 在 $[-1,1]$ 上 uniform（均匀分布）；
- $x_{\mathrm{wc}}$ 最小化 $\sup_{-1\le u\le 1} \|A(u)x - b\|_2^2$（worst-case，最坏情况）。

讲义图示画出 $r(u) = \|A(u)x - b\|_2$（横轴 $u \in [-2,2]$）的三条曲线，分别对应 $x_{\mathrm{nom}}$、$x_{\mathrm{stoch}}$、$x_{\mathrm{wc}}$。由三者的定义即可推断曲线形态：$x_{\mathrm{nom}}$ 只管名义模型，$x_{\mathrm{wc}}$ 把最坏残差压到最低，$x_{\mathrm{stoch}}$ 介于两者之间。

---

<!-- p.18 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Stochastic robust least-squares（随机鲁棒最小二乘）</span>

设 $A = \bar{A} + U$，$U$ 为随机矩阵，$\mathbb{E}\,U = 0$，$\mathbb{E}\,U^TU = P$。目标：

```math
\text{minimize} \quad \mathbb{E}\,\|(\bar{A}+U)x - b\|_2^2
```

- 目标函数有 explicit expression（显式表达式）：

```math
\begin{aligned}
\mathbb{E}\|Ax-b\|_2^2
&= \mathbb{E}\|\bar{A}x - b + Ux\|_2^2 \\
&= \|\bar{A}x-b\|_2^2 + \mathbb{E}\,x^TU^TUx
= \|\bar{A}x-b\|_2^2 + x^TPx
\end{aligned}
```

  （第二步用 $\mathbb{E}\,U=0$：交叉项 $(\bar{A}x-b)^T\,\mathbb{E}[U]\,x = 0$ 消失。）
- 因此 robust LS problem（鲁棒最小二乘问题）等价于普通 LS problem（最小二乘问题）：

```math
\text{minimize} \quad \|\bar{A}x-b\|_2^2 + \|P^{1/2}x\|_2^2
```

- 当 $P = \delta I$ 时，恰好得到 Tikhonov regularized problem（Tikhonov 正则化问题）：

```math
\text{minimize} \quad \|\bar{A}x-b\|_2^2 + \delta\|x\|_2^2
```

---

<!-- p.19 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Worst-case robust least-squares（最坏情况鲁棒最小二乘）</span>

设不确定集 $\mathcal{A} = \{\bar{A} + u_1A_1 + \cdots + u_pA_p \mid \|u\|_2 \le 1\}$。目标可改写为：

```math
\text{minimize} \quad \sup_{A\in\mathcal{A}} \|Ax-b\|_2^2
= \sup_{\|u\|_2\le 1} \|P(x)u + q(x)\|_2^2
```

其中 $P(x) = \begin{bmatrix} A_1x & A_2x & \cdots & A_px \end{bmatrix}$，$q(x) = \bar{A}x - b$。

- 由第 5–14 页，下列两个问题之间 strong duality（强对偶性）成立：

```math
\begin{array}{ll}
\text{maximize} & \|Pu+q\|_2^2 \\
\text{subject to} & \|u\|_2^2 \le 1
\end{array}
\qquad\qquad
\begin{array}{ll}
\text{minimize} & t + \lambda \\
\text{subject to} & \begin{bmatrix} I & P & q \\ P^T & \lambda I & 0 \\ q^T & 0 & t \end{bmatrix} \succeq 0
\end{array}
```

- 因此 robust LS problem 等价于 SDP（semidefinite program，半定规划）：

```math
\begin{array}{ll}
\text{minimize} & t + \lambda \\
\text{subject to} & \begin{bmatrix} I & P(x) & q(x) \\ P(x)^T & \lambda I & 0 \\ q(x)^T & 0 & t \end{bmatrix} \succeq 0
\end{array}
```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 最坏情况问题为什么摇身一变成了半定规划？关键工具是 Schur complement（舒尔补），你在 Strang 的分块矩阵消元里其实见过它的影子：分块矩阵 $\begin{bmatrix} Q & s \\ s^T & r \end{bmatrix} \succeq 0$（$Q \succ 0$）等价于 $r - s^TQ^{-1}s \ge 0$。对固定的 $x$，内层 $\sup_{\|u\|_2\le 1}\|P(x)u+q(x)\|_2^2$ 是单位球上的二次函数最大化，取拉格朗日对偶后恰是"存在 $\lambda,t$ 使上述 $3\times 3$ 矩阵半正定"；由于第 5 讲已证这对偶没有 duality gap（对偶间隙），"对每个 $x$ 都要防住最坏的 $u$"就能整体打包成一个矩阵不等式约束，鲁棒问题于是落入 SDP 框架。两条路线的哲学对比也有意思：stochastic 方法只优化期望，对分布尾部毫无防护；worst-case 方法防住所有 $\|u\|_2\le 1$ 的扰动，但解偏保守——这正是概率决策中"平均表现好"与"任何情形都不太差"的取舍，后者即博弈论中的 minimax（最小最大化）思想。

---

<!-- p.20 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">示例：残差直方图对比</span>

数值实验：取 $r(u) = \|(A_0 + u_1A_1 + u_2A_2)x - b\|_2$，$u$ 在 unit disk（单位圆盘）上均匀分布，对三个不同的 $x$ 画出残差 $r(u)$ 的 frequency（频率）直方图（图例：$x_{\mathrm{ls}}$、$x_{\mathrm{tik}}$、$x_{\mathrm{rls}}$；横轴 $0$–$5$，纵轴 $0$–$0.25$）。

- $x_{\mathrm{ls}}$ 最小化 $\|A_0x - b\|_2$；
- $x_{\mathrm{tik}}$ 最小化 $\|A_0x - b\|_2^2 + \|x\|_2^2$（Tikhonov solution，Tikhonov 解）；
- $x_{\mathrm{wc}}$ 最小化 $\sup_{\|u\|_2\le 1}\ \|A_0x - b\|_2^2 + \|x\|_2^2$（图例中的 $x_{\mathrm{rls}}$）。
