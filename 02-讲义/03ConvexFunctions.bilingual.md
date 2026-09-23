# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

## <span style="color:#c0392b;">3. 凸函数 Convex Functions</span>

> <span style="color:#7f8c8d;">Source: SEE官方讲义/03ConvexFunctions.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

<!-- p.1 -->

<span style="color:#2471a3;">**[lecture]**</span>

### <span style="color:#c0392b;">第 3 讲：凸函数 Convex Functions</span>

本讲内容（topics）：

- 基本性质与例子（basic properties and examples）
- 保持凸性的运算（operations that preserve convexity）
- 共轭函数（conjugate function）
- 拟凸函数（quasiconvex functions）
- 对数凹与对数凸函数（log-concave and log-convex functions）
- 关于广义不等式（generalized inequalities）的凸性

<!-- p.2 -->

<span style="color:#2471a3;">**[definition]**</span>

### <span style="color:#c0392b;">定义 Definition</span>

<span style="color:#00838f;">**定义：** 函数 $f:\mathbb{R}^n \to \mathbb{R}$ 是 convex function（凸函数），若其定义域 $\mathrm{dom}\,f$ 是 convex set（凸集），且</span>

```math
f(\theta x + (1-\theta)y) \le \theta f(x) + (1-\theta) f(y)
```

<span style="color:#00838f;">对所有 $x, y \in \mathrm{dom}\,f$、$0 \le \theta \le 1$ 成立。</span>

> **图示：** 曲线上两点 $(x, f(x))$ 与 $(y, f(y))$ 之间的弦（割线）整段位于函数图像上方——这正是上述不等式的几何含义。

- 若 $-f$ 是凸的，则称 $f$ 为 concave function（凹函数）。
- 若 $\mathrm{dom}\,f$ 是凸集，且对 $x, y \in \mathrm{dom}\,f$、$x \ne y$、$0 < \theta < 1$ 有

```math
f(\theta x + (1-\theta)y) < \theta f(x) + (1-\theta) f(y)
```

  则称 $f$ 为 strictly convex function（严格凸函数）。

<!-- p.3 -->

<span style="color:#2471a3;">**[example]**</span>

### <span style="color:#c0392b;">$\mathbb{R}$ 上的例子 Examples on $\mathbb{R}$</span>

**convex（凸）的例子：**

- affine（仿射）函数：$ax + b$，定义在 $\mathbb{R}$ 上，任意 $a, b \in \mathbb{R}$
- exponential（指数）函数：$e^{ax}$，任意 $a \in \mathbb{R}$
- 幂函数（powers）：$x^{\alpha}$，定义在 $\mathbb{R}_{++}$ 上，$\alpha \ge 1$ 或 $\alpha \le 0$
- 绝对值的幂（powers of absolute value）：$|x|^p$，定义在 $\mathbb{R}$ 上，$p \ge 1$
- negative entropy（负熵）：$x \log x$，定义在 $\mathbb{R}_{++}$ 上

**concave（凹）的例子：**

- affine（仿射）函数：$ax + b$，定义在 $\mathbb{R}$ 上，任意 $a, b \in \mathbb{R}$
- 幂函数：$x^{\alpha}$，定义在 $\mathbb{R}_{++}$ 上，$0 \le \alpha \le 1$
- logarithm（对数）函数：$\log x$，定义在 $\mathbb{R}_{++}$ 上

<!-- p.4 -->

<span style="color:#2471a3;">**[example]**</span>

### <span style="color:#c0392b;">$\mathbb{R}^n$ 与 $\mathbb{R}^{m\times n}$ 上的例子 Examples on $\mathbb{R}^n$ and $\mathbb{R}^{m\times n}$</span>

affine（仿射）函数既凸又凹；所有 norm（范数）都是凸的。

$\mathbb{R}^n$ 上的例子：

- affine 函数 $f(x) = a^T x + b$
- 范数：$\|x\|_p = \left(\sum_{i=1}^n |x_i|^p\right)^{1/p}$（$p \ge 1$）；$\|x\|_{\infty} = \max_k |x_k|$

$\mathbb{R}^{m\times n}$（$m \times n$ 矩阵）上的例子：

- affine 函数

```math
f(X) = \mathrm{tr}(A^T X) + b = \sum_{i=1}^m \sum_{j=1}^n A_{ij} X_{ij} + b
```

- spectral norm（谱范数，即最大奇异值范数）：

```math
f(X) = \|X\|_2 = \sigma_{\max}(X) = \left(\lambda_{\max}(X^T X)\right)^{1/2}
```

<!-- p.5 -->

<span style="color:#2471a3;">**[theorem]**</span>

### <span style="color:#c0392b;">凸函数在直线上的限制 Restriction of a convex function to a line</span>

$f:\mathbb{R}^n \to \mathbb{R}$ 是 convex（凸）的，当且仅当：对任意 $x \in \mathrm{dom}\,f$、$v \in \mathbb{R}^n$，一元函数

```math
g(t) = f(x + tv), \qquad \mathrm{dom}\,g = \{t \mid x + tv \in \mathrm{dom}\,f\}
```

关于 $t$ 是 convex（凸）的。

也就是说，可以通过检验一元函数的凸性来检验 $f$ 的凸性。

**例子：** $f: S^n \to \mathbb{R}$，$f(X) = \log\det X$，$\mathrm{dom}\,f = S^n_{++}$。

```math
g(t) = \log\det(X + tV) = \log\det X + \log\det\!\left(I + tX^{-1/2} V X^{-1/2}\right) = \log\det X + \sum_{i=1}^n \log(1 + t\lambda_i)
```

其中 $\lambda_i$ 是 $X^{-1/2} V X^{-1/2}$ 的特征值（eigenvalues）。

$g$ 关于 $t$ 是 concave（凹）的（对任意选取的 $X \succ 0$、$V$ 都成立）；因此 $f$ 是凹的。

<!-- p.6 -->

<span style="color:#2471a3;">**[definition]**</span>

### <span style="color:#c0392b;">扩展值延拓 Extended-value extension</span>

函数 $f$ 的 extended-value extension（扩展值延拓）$\tilde{f}$ 定义为

```math
\tilde{f}(x) = \begin{cases} f(x), & x \in \mathrm{dom}\,f \\ \infty, & x \notin \mathrm{dom}\,f \end{cases}
```

它常能简化记号。例如，条件

```math
0 \le \theta \le 1 \implies \tilde{f}(\theta x + (1-\theta)y) \le \theta \tilde{f}(x) + (1-\theta) \tilde{f}(y)
```

（作为 $\mathbb{R} \cup \{\infty\}$ 中的不等式）与下面两个条件的整体含义相同：

- $\mathrm{dom}\,f$ 是凸集；
- 对 $x, y \in \mathrm{dom}\,f$，$0 \le \theta \le 1 \implies f(\theta x + (1-\theta)y) \le \theta f(x) + (1-\theta) f(y)$。

<!-- p.7 -->

<span style="color:#2471a3;">**[theorem]**</span>

### <span style="color:#c0392b;">一阶条件 First-order condition</span>

若 $\mathrm{dom}\,f$ 是开集，且 gradient（梯度）

```math
\nabla f(x) = \left(\frac{\partial f(x)}{\partial x_1},\ \frac{\partial f(x)}{\partial x_2},\ \ldots,\ \frac{\partial f(x)}{\partial x_n}\right)
```

在每个 $x \in \mathrm{dom}\,f$ 处都存在，则称 $f$ 是 differentiable（可微）的。

<span style="color:#00838f;">**一阶条件（first-order condition）：** 定义域为凸集的可微函数 $f$ 是 convex（凸）的，当且仅当对所有 $x, y \in \mathrm{dom}\,f$，</span>

```math
f(y) \ge f(x) + \nabla f(x)^T (y - x)
```

> **图示：** 在点 $(x, f(x))$ 处画出切线（一阶 Taylor 近似）$f(x) + \nabla f(x)^T(y-x)$；它在每一点 $y$ 处都位于函数值 $f(y)$ 之下——一阶近似是 $f$ 的全局下估计（global underestimator）。

<!-- p.8 -->

<span style="color:#2471a3;">**[theorem]**</span>

### <span style="color:#c0392b;">二阶条件 Second-order conditions</span>

若 $\mathrm{dom}\,f$ 是开集，且 Hessian 矩阵 $\nabla^2 f(x) \in S^n$，

```math
\nabla^2 f(x)_{ij} = \frac{\partial^2 f(x)}{\partial x_i \partial x_j}, \qquad i, j = 1, \ldots, n
```

在每个 $x \in \mathrm{dom}\,f$ 处都存在，则称 $f$ 是 twice differentiable（二阶可微）的。

<span style="color:#00838f;">**二阶条件（second-order conditions）：** 对定义域为凸集的二阶可微函数 $f$：</span>

- $f$ 是 convex（凸）的，当且仅当对所有 $x \in \mathrm{dom}\,f$，

```math
\nabla^2 f(x) \succeq 0
```

- 若对所有 $x \in \mathrm{dom}\,f$ 都有 $\nabla^2 f(x) \succ 0$，则 $f$ 是严格凸的。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 二阶条件把凸性判定完全归结为 Hessian 矩阵的半正定性（positive semidefinite）。而判断对称矩阵是否半正定，正是 Strang 线代的主场：特征值全部非负、各阶主子式（principal minors）非负、$x^TAx \ge 0$ 对一切 $x$ 成立，这三条等价路径在这里全部适用。注意逻辑上的精细差别：$\nabla^2 f(x) \succeq 0$ 处处成立是凸性的充要条件，而 $\nabla^2 f(x) \succ 0$ 只是严格凸的充分条件——$f(x) = x^4$ 严格凸，却在原点处 $f''(0) = 0$，并不正定，这与 Strang 在 18.065 中强调的"半正定矩阵允许零特征值"一脉相承。本讲 slide 9 中 $2A^TA \succeq 0$ 对任意 $A$ 成立，正是 Gram 矩阵恒半正定这一线代结论的直接应用。往后学到半定规划（SDP）时，被要求半正定的将是矩阵变量的仿射组合而非具体的 Hessian，但"特征值判据"这一内核不变。

<!-- p.9 -->

<span style="color:#2471a3;">**[example]**</span>

### <span style="color:#c0392b;">例子 Examples</span>

**二次函数（quadratic function）：** $f(x) = (1/2)x^T P x + q^T x + r$（$P \in S^n$）

$\nabla f(x) = Px + q$，$\nabla^2 f(x) = P$。当 $P \succeq 0$ 时是 convex（凸）的。

**least-squares（最小二乘）目标函数：** $f(x) = \|Ax - b\|_2^2$

$\nabla f(x) = 2A^T(Ax - b)$，$\nabla^2 f(x) = 2A^T A$。对任意 $A$ 都是 convex（凸）的。

**quadratic-over-linear（二次-线性）函数：** $f(x, y) = x^2/y$

```math
\nabla^2 f(x, y) = \frac{2}{y^3} \begin{bmatrix} y \\ -x \end{bmatrix} \begin{bmatrix} y \\ -x \end{bmatrix}^T \succeq 0
```

当 $y > 0$ 时是 convex（凸）的。

> **图示：** 曲面 $f(x,y) = x^2/y$ 在 $x \in [-2, 2]$、$y \in (0, 2]$ 范围内的图像，呈碗状上凸形态。

<!-- p.10 -->

<span style="color:#2471a3;">**[example]**</span>

### <span style="color:#c0392b;">log-sum-exp 与几何均值</span>

**log-sum-exp（对数-求和-指数）函数：** $f(x) = \log \sum_{k=1}^n \exp x_k$ 是 convex（凸）的。

记 $z_k = \exp x_k$，则

```math
\nabla^2 f(x) = \frac{1}{\mathbf{1}^T z}\,\mathrm{diag}(z) - \frac{1}{(\mathbf{1}^T z)^2}\, z z^T
```

要证明 $\nabla^2 f(x) \succeq 0$，必须验证对所有 $v$ 都有 $v^T \nabla^2 f(x) v \ge 0$：

```math
v^T \nabla^2 f(x) v = \frac{\left(\sum_k z_k v_k^2\right)\left(\sum_k z_k\right) - \left(\sum_k v_k z_k\right)^2}{\left(\sum_k z_k\right)^2} \ge 0
```

其中用到 $\left(\sum_k v_k z_k\right)^2 \le \left(\sum_k z_k v_k^2\right)\left(\sum_k z_k\right)$（由 Cauchy-Schwarz inequality，Cauchy-Schwarz 不等式）。

**geometric mean（几何均值）：** $f(x) = \left(\prod_{k=1}^n x_k\right)^{1/n}$，定义在 $\mathbb{R}^n_{++}$ 上，是 concave（凹）的。

（证明思路与 log-sum-exp 类似。）

<!-- p.11 -->

<span style="color:#2471a3;">**[definition]**</span>

### <span style="color:#c0392b;">上图与下水平集 Epigraph and sublevel set</span>

$f: \mathbb{R}^n \to \mathbb{R}$ 的 $\alpha$-sublevel set（$\alpha$-下水平集）：

```math
C_{\alpha} = \{x \in \mathrm{dom}\,f \mid f(x) \le \alpha\}
```

<span style="color:#00838f;">convex（凸）函数的所有下水平集都是凸集（反之不成立）。</span>

$f: \mathbb{R}^n \to \mathbb{R}$ 的 epigraph（上图）：

```math
\mathrm{epi}\,f = \{(x, t) \in \mathbb{R}^{n+1} \mid x \in \mathrm{dom}\,f,\ f(x) \le t\}
```

> **图示：** 把函数图像（曲线 $f$）连同其上方整块区域（$\mathrm{epi}\,f$）视为一个集合，即"图像以上的部分"。

<span style="color:#00838f;">$f$ 是 convex（凸）函数，当且仅当 $\mathrm{epi}\,f$ 是凸集。</span>

<!-- p.12 -->

<span style="color:#2471a3;">**[theorem]**</span>

### <span style="color:#c0392b;">Jensen 不等式 Jensen's inequality</span>

**基本不等式：** 若 $f$ 是 convex（凸）的，则对 $0 \le \theta \le 1$，

```math
f(\theta x + (1-\theta)y) \le \theta f(x) + (1-\theta) f(y)
```

**推广：** 若 $f$ 是 convex（凸）的，则对任意 random variable（随机变量）$z$，

```math
f(\mathbb{E}\,z) \le \mathbb{E}\,f(z)
```

基本不等式是取离散分布时的特例：$\mathrm{prob}(z = x) = \theta$，$\mathrm{prob}(z = y) = 1 - \theta$。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> Jensen 不等式是凸分析与概率论之间的枢纽。你在 Bertsekas 概率论里见过的方差非负性 $\mathrm{var}(z) = \mathbb{E}z^2 - (\mathbb{E}z)^2 \ge 0$，就是取 $f(u) = u^2$ 时 Jensen 不等式的特例：平方函数是凸的，所以"先平方再取期望"不小于"先取期望再平方"。换成指数函数 $f(u) = e^u$，得到 $\mathbb{E}e^z \ge e^{\mathbb{E}z}$，说明矩母函数（moment generating function）在对数尺度上位于其切线之上，这也是后续章节用对数矩母函数推导 Chernoff 界的出发点。几何直观只有一句话：凸图像在切线上方，故"先平均后求值 $\le$ 先求值后平均"。把这条不等式练成反射弧，信息论里的熵不等式与统计估计中的下界证明都会顺手得多。

<!-- p.13 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">保持凸性的运算 Operations that preserve convexity</span>

建立函数凸性的实用方法：

1. 直接验证定义（常可通过"限制到直线上"来简化）；
2. 对二阶可微函数，证明 $\nabla^2 f(x) \succeq 0$；
3. 证明 $f$ 是由简单凸函数经过保持凸性的运算构造出来的。

保持凸性的运算包括：

- 非负加权和（nonnegative weighted sum）
- 与 affine（仿射）函数复合（composition with affine function）
- 逐点最大值与逐点上确界（pointwise maximum and supremum）
- 复合（composition）
- 最小化（minimization）
- 透视（perspective）

<!-- p.14 -->

<span style="color:#2471a3;">**[theorem]**</span>

### <span style="color:#c0392b;">非负加权和与仿射复合 Positive weighted sum & composition with affine function</span>

**非负倍数（nonnegative multiple）：** 若 $f$ 是凸的且 $\alpha \ge 0$，则 $\alpha f$ 是凸的。

**求和（sum）：** 若 $f_1, f_2$ 都是凸的，则 $f_1 + f_2$ 是凸的（可推广到无穷和与积分）。

**与仿射函数复合（composition with affine function）：** 若 $f$ 是凸的，则 $f(Ax + b)$ 是凸的。

**例子：**

- 线性不等式的 log barrier（对数障碍）函数：

```math
f(x) = -\sum_{i=1}^m \log(b_i - a_i^T x), \qquad \mathrm{dom}\,f = \{x \mid a_i^T x < b_i,\ i = 1, \ldots, m\}
```

- 仿射函数的（任意）范数：$f(x) = \|Ax + b\|$。

<!-- p.15 -->

<span style="color:#2471a3;">**[theorem]**</span>

### <span style="color:#c0392b;">逐点最大值 Pointwise maximum</span>

<span style="color:#00838f;">若 $f_1, \ldots, f_m$ 都是 convex（凸）的，则 $f(x) = \max\{f_1(x), \ldots, f_m(x)\}$ 是凸的。</span>

**例子：**

- piecewise-linear（分段线性）函数：$f(x) = \max_{i=1,\ldots,m}\,(a_i^T x + b_i)$ 是凸的。
- $x \in \mathbb{R}^n$ 的最大的 $r$ 个分量之和：

```math
f(x) = x_{[1]} + x_{[2]} + \cdots + x_{[r]}
```

  是凸的（$x_{[i]}$ 表示 $x$ 中第 $i$ 大的分量）。

**证明：** 把它写成若干线性函数的逐点最大值即可：

```math
f(x) = \max\{x_{i_1} + x_{i_2} + \cdots + x_{i_r} \mid 1 \le i_1 < i_2 < \cdots < i_r \le n\}
```

<!-- p.16 -->

<span style="color:#2471a3;">**[theorem]**</span>

### <span style="color:#c0392b;">逐点上确界 Pointwise supremum</span>

<span style="color:#00838f;">若 $f(x, y)$ 对每个 $y \in \mathcal{A}$ 关于 $x$ 都是 convex（凸）的，则</span>

```math
g(x) = \sup_{y \in \mathcal{A}} f(x, y)
```

<span style="color:#00838f;">是 convex（凸）的。</span>

**例子：**

- 集合 $\mathcal{C}$ 的 support function（支撑函数）：$S_{\mathcal{C}}(x) = \sup_{y \in \mathcal{C}}\, y^T x$ 是凸的。
- 到集合 $\mathcal{C}$ 中最远点的距离：$f(x) = \sup_{y \in \mathcal{C}}\, \|x - y\|$。
- 对称矩阵的最大特征值：对 $X \in S^n$，

```math
\lambda_{\max}(X) = \sup_{\|y\|_2 = 1} y^T X y
```

<!-- p.17 -->

<span style="color:#2471a3;">**[theorem]**</span>

### <span style="color:#c0392b;">与标量函数复合 Composition with scalar functions</span>

$g: \mathbb{R}^n \to \mathbb{R}$ 与 $h: \mathbb{R} \to \mathbb{R}$ 的复合（composition）：$f(x) = h(g(x))$。

<span style="color:#00838f;">$f$ 是 convex（凸）的，若：</span>

- $g$ 是凸的，$h$ 是凸的，且 $\tilde{h}$ 单调不减（nondecreasing）；
- $g$ 是凹的，$h$ 是凸的，且 $\tilde{h}$ 单调不增（nonincreasing）。

- 证明（取 $n = 1$，$g, h$ 可微）：$f''(x) = h''(g(x))\, g'(x)^2 + h'(g(x))\, g''(x)$。
- 注意：单调性必须对扩展值延拓 $\tilde{h}$ 成立。

**例子：**

- 若 $g$ 是凸的，则 $\exp g(x)$ 是凸的。
- 若 $g$ 是凹的且取正值，则 $1/g(x)$ 是凸的。

<!-- p.18 -->

<span style="color:#2471a3;">**[theorem]**</span>

### <span style="color:#c0392b;">向量复合 Vector composition</span>

$g: \mathbb{R}^n \to \mathbb{R}^k$ 与 $h: \mathbb{R}^k \to \mathbb{R}$ 的复合：

```math
f(x) = h(g(x)) = h(g_1(x), g_2(x), \ldots, g_k(x))
```

<span style="color:#00838f;">$f$ 是 convex（凸）的，若：</span>

- 各 $g_i$ 是凸的，$h$ 是凸的，且 $\tilde{h}$ 对每个分量都单调不减；
- 各 $g_i$ 是凹的，$h$ 是凸的，且 $\tilde{h}$ 对每个分量都单调不增。

**证明**（取 $n = 1$，$g, h$ 可微）：

```math
f''(x) = g'(x)^T \nabla^2 h(g(x))\, g'(x) + \nabla h(g(x))^T g''(x)
```

**例子：**

- 若各 $g_i$ 是凹的且取正值，则 $\sum_{i=1}^m \log g_i(x)$ 是 concave（凹）的。
- 若各 $g_i$ 是凸的，则 $\log \sum_{i=1}^m \exp g_i(x)$ 是 convex（凸）的。

<!-- p.19 -->

<span style="color:#2471a3;">**[theorem]**</span>

### <span style="color:#c0392b;">最小化 Minimization</span>

<span style="color:#00838f;">若 $f(x, y)$ 关于 $(x, y)$ 是 convex（凸）的，且 $\mathcal{C}$ 是凸集，则</span>

```math
g(x) = \inf_{y \in \mathcal{C}} f(x, y)
```

<span style="color:#00838f;">是 convex（凸）的。</span>

**例子：**

- 设 $f(x, y) = x^T A x + 2x^T B y + y^T C y$，其中

```math
\begin{bmatrix} A & B \\ B^T & C \end{bmatrix} \succeq 0, \qquad C \succ 0
```

  对 $y$ 最小化，得 $g(x) = \inf_y f(x, y) = x^T (A - B C^{-1} B^T)\, x$。$g$ 是凸的，因此 Schur complement（Schur 补）$A - BC^{-1}B^T \succeq 0$。
- 到集合的距离（distance to a set）：$\mathrm{dist}(x, S) = \inf_{y \in S} \|x - y\|$，当 $S$ 是凸集时是凸的。

<!-- p.20 -->

<span style="color:#2471a3;">**[definition]**</span>

### <span style="color:#c0392b;">透视 Perspective</span>

函数 $f: \mathbb{R}^n \to \mathbb{R}$ 的 perspective（透视）是函数 $g: \mathbb{R}^n \times \mathbb{R} \to \mathbb{R}$：

```math
g(x, t) = t\, f(x/t), \qquad \mathrm{dom}\,g = \{(x, t) \mid x/t \in \mathrm{dom}\,f,\ t > 0\}
```

<span style="color:#00838f;">若 $f$ 是 convex（凸）的，则 $g$ 是凸的。</span>

**例子：**

- $f(x) = x^T x$ 是凸的；因此 $g(x, t) = x^T x / t$ 在 $t > 0$ 时是凸的。
- 负对数 $f(x) = -\log x$ 是凸的；因此 relative entropy（相对熵）$g(x, t) = t \log t - t \log x$ 在 $\mathbb{R}^2_{++}$ 上是凸的。
- 若 $f$ 是凸的，则

```math
g(x) = (c^T x + d)\, f\!\left((Ax + b)/(c^T x + d)\right)
```

  在 $\{x \mid c^T x + d > 0,\ (Ax+b)/(c^T x + d) \in \mathrm{dom}\,f\}$ 上是凸的。

<!-- p.21 -->

<span style="color:#2471a3;">**[definition]**</span>

### <span style="color:#c0392b;">共轭函数 The conjugate function</span>

函数 $f$ 的 conjugate（共轭）为

```math
f^*(y) = \sup_{x \in \mathrm{dom}\,f} \left(y^T x - f(x)\right)
```

> **图示：** 对固定的 $y$，$f^*(y)$ 是直线 $y^Tx$ 与曲线 $f(x)$ 之间的最大竖直间隙；过切点的这条斜率为 $y$ 的直线在纵轴上的截距为 $-f^*(y)$（图中标出了点 $(0, -f^*(y))$）。

- $f^*$ 是 convex（凸）的（即使 $f$ 本身不是凸的）。
- 共轭函数将在第 5 章中发挥作用。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 共轭函数是 Legendre 变换的凸分析推广，两者思想一致：换一套变量描述同一个对象。原函数用"位置—斜率"关系刻画，共轭 $f^*(y)$ 则记录"斜率为 $y$ 的仿射直线最高能顶到多高"。它是对偶性（duality）的种子：第 5 章的 Lagrange 对偶函数 $g(\lambda, \nu) = \inf_x L(x, \lambda, \nu)$ 本质上就是对 $x$ 做共轭式的取极操作，而弱对偶性（weak duality）正是 Fenchel 不等式 $f(x) + f^*(y) \ge y^T x$ 的直接化身。即使 $f$ 不凸，$f^*$ 也自动是凸的，因为它是无穷多条（关于 $x$ 的）仿射函数的逐点上确界——恰好用上本讲 p.16 的结论。对可微的严格凸函数，共轭还是一对镜像：$(1/2)x^TQx$ 对应 $(1/2)y^TQ^{-1}y$，由梯度映射 $\nabla f(x) = y$ 撮合；这层"求逆"结构在 KKT 条件与牛顿法的一阶最优性方程里会一再现身。

<!-- p.22 -->

<span style="color:#2471a3;">**[example]**</span>

### <span style="color:#c0392b;">共轭函数的例子 Examples</span>

- 负对数 $f(x) = -\log x$：

```math
f^*(y) = \sup_{x > 0} \left(xy + \log x\right) = \begin{cases} -1 - \log(-y), & y < 0 \\ \infty, & \text{其他情形} \end{cases}
```

- 严格凸二次函数 $f(x) = (1/2)x^T Q x$，$Q \in S^n_{++}$：

```math
f^*(y) = \sup_{x} \left(y^T x - (1/2)\, x^T Q x\right) = \frac{1}{2}\, y^T Q^{-1} y
```

<!-- p.23 -->

<span style="color:#2471a3;">**[definition]**</span>

### <span style="color:#c0392b;">拟凸函数 Quasiconvex functions</span>

<span style="color:#00838f;">$f: \mathbb{R}^n \to \mathbb{R}$ 是 quasiconvex function（拟凸函数），若 $\mathrm{dom}\,f$ 是凸集，且所有下水平集</span>

```math
S_{\alpha} = \{x \in \mathrm{dom}\,f \mid f(x) \le \alpha\}
```

<span style="color:#00838f;">对一切 $\alpha$ 都是凸集。</span>

> **图示：** 一维曲线上标出两个水平 $\alpha$ 与 $\beta$，以及曲线上三点 $a, b, c$；无论取哪个水平，位于水平线之下的点集（下水平集）始终是一个区间，即凸集。

- 若 $-f$ 是拟凸的，则称 $f$ 是 quasiconcave function（拟凹函数）。
- 若 $f$ 既是拟凸的又是拟凹的，则称 $f$ 是 quasilinear function（拟线性函数）。

<!-- p.24 -->

<span style="color:#2471a3;">**[example]**</span>

### <span style="color:#c0392b;">例子 Examples</span>

- $\sqrt{|x|}$ 在 $\mathbb{R}$ 上是拟凸的。
- $\mathrm{ceil}(x) = \inf\{z \in \mathbb{Z} \mid z \ge x\}$ 是拟线性的。
- $\log x$ 在 $\mathbb{R}_{++}$ 上是拟线性的。
- $f(x_1, x_2) = x_1 x_2$ 在 $\mathbb{R}^2_{++}$ 上是拟凹的。
- linear-fractional（线性分式）函数

```math
f(x) = \frac{a^T x + b}{c^T x + d}, \qquad \mathrm{dom}\,f = \{x \mid c^T x + d > 0\}
```

  是拟线性的。
- 距离比（distance ratio）

```math
f(x) = \frac{\|x - a\|_2}{\|x - b\|_2}, \qquad \mathrm{dom}\,f = \{x \mid \|x - a\|_2 \le \|x - b\|_2\}
```

  是拟凸的。

<!-- p.25 -->

<span style="color:#2471a3;">**[example]**</span>

### <span style="color:#c0392b;">内部收益率 Internal rate of return</span>

- 现金流（cash flow）$x = (x_0, \ldots, x_n)$：$x_i$ 是第 $i$ 期的支付额（若 $x_i > 0$，表示这笔钱付给我们）。
- 假设 $x_0 < 0$ 且 $x_0 + x_1 + \cdots + x_n > 0$。
- 现金流 $x$ 在利率 $r$ 下的 present value（现值）：

```math
\mathrm{PV}(x, r) = \sum_{i=0}^n (1 + r)^{-i} x_i
```

- internal rate of return（内部收益率）是使 $\mathrm{PV}(x, r) = 0$ 的最小利率：

```math
\mathrm{IRR}(x) = \inf\{r \ge 0 \mid \mathrm{PV}(x, r) = 0\}
```

IRR 是拟凹的：其超水平集（superlevel set）是若干半空间（halfspace）的交：

```math
\mathrm{IRR}(x) \ge R \iff \sum_{i=0}^n (1 + r)^{-i} x_i \ge 0 \quad \text{对 } 0 \le r \le R
```

<!-- p.26 -->

<span style="color:#2471a3;">**[theorem]**</span>

### <span style="color:#c0392b;">性质 Properties</span>

**修正的 Jensen 不等式（modified Jensen inequality）：** 对拟凸函数 $f$，

```math
0 \le \theta \le 1 \implies f(\theta x + (1-\theta)y) \le \max\{f(x), f(y)\}
```

**一阶条件：** 定义域为凸集的可微函数 $f$ 是拟凸的，当且仅当

```math
f(y) \le f(x) \implies \nabla f(x)^T (y - x) \le 0
```

> **图示：** 在点 $x$ 处画出梯度向量 $\nabla f(x)$；所有函数值不超过 $f(x)$ 的点 $y$ 都与该梯度向量成钝角（内积非正）。

拟凸函数之和未必是拟凸的。

<!-- p.27 -->

<span style="color:#2471a3;">**[definition]**</span>

### <span style="color:#c0392b;">对数凹与对数凸函数 Log-concave and log-convex functions</span>

正值函数 $f$ 是 log-concave（对数凹）的，若 $\log f$ 是凹的：

```math
f(\theta x + (1-\theta)y) \ge f(x)^{\theta} f(y)^{1-\theta}, \qquad 0 \le \theta \le 1
```

$f$ 是 log-convex（对数凸）的，若 $\log f$ 是凸的。

- 幂函数：$x^a$ 在 $\mathbb{R}_{++}$ 上，当 $a \le 0$ 时是对数凸的，当 $a \ge 0$ 时是对数凹的。
- 许多常见的概率密度（probability density）都是对数凹的，例如 normal（正态/高斯）分布：

```math
f(x) = \frac{1}{\sqrt{(2\pi)^n \det \Sigma}}\, e^{-\frac{1}{2}(x - \bar{x})^T \Sigma^{-1} (x - \bar{x})}
```

- 累积高斯分布函数（cumulative Gaussian distribution function）$\Phi$ 是对数凹的：

```math
\Phi(x) = \frac{1}{\sqrt{2\pi}} \int_{-\infty}^{x} e^{-u^2/2}\, du
```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 对数凹性把乘法结构折叠成加法结构：$f$ 对数凹 $\iff$ $\log f$ 凹，即密度在任意两点处的加权几何平均不超过其中点值。你正在 Bertsekas 概率论里啃的高斯分布天然对数凹：它的 $\log f$ 是负定二次型加常数，凹性一目了然，这也顺带解释了为什么高斯分布的下水平集是 ellipsoid（椭球）。概率论视角的回报在 slides 28–29："积分保持对数凹"意味着高斯密度的边际分布、两个对数凹密度的卷积、以及"高斯变量落入凸集的概率"统统对数凹，不必逐个硬算积分。工程上的兑现是 slide 30：良率函数对数凹 $\Rightarrow$ 良率区域 $\{x \mid Y(x) \ge \alpha\}$ 是凸集，于是参数标定问题可以放心交给凸优化求解器。直观地说，对数凹密度是"单峰、尾部不鼓包"的密度，以它为基础的推断与优化问题性质格外好。

<!-- p.28 -->

<span style="color:#2471a3;">**[theorem]**</span>

### <span style="color:#c0392b;">对数凹函数的性质 Properties of log-concave functions</span>

- 二阶可微、定义域为凸集的 $f$ 是对数凹的，当且仅当对所有 $x \in \mathrm{dom}\,f$，

```math
f(x)\, \nabla^2 f(x) \preceq \nabla f(x)\, \nabla f(x)^T
```

- 对数凹函数的乘积仍是对数凹的。
- 对数凹函数之和不总是对数凹的。
- 积分性质：若 $f: \mathbb{R}^n \times \mathbb{R}^m \to \mathbb{R}$ 是对数凹的，则

```math
g(x) = \int f(x, y)\, dy
```

  是对数凹的（这一点并不容易证明）。

<!-- p.29 -->

<span style="color:#2471a3;">**[theorem]**</span>

### <span style="color:#c0392b;">积分性质的推论 Consequences of the integration property</span>

- 对数凹函数 $f, g$ 的 convolution（卷积）$f * g$ 是对数凹的：

```math
(f * g)(x) = \int f(x - y)\, g(y)\, dy
```

- 若 $\mathcal{C} \subseteq \mathbb{R}^n$ 是凸集，且随机变量 $y$ 具有对数凹的概率密度函数（pdf），则

```math
f(x) = \mathrm{prob}(x + y \in \mathcal{C})
```

  是对数凹的。

**证明：** 把 $f(x)$ 写成对数凹函数乘积的积分：

```math
f(x) = \int g(x + y)\, p(y)\, dy, \qquad g(u) = \begin{cases} 1, & u \in \mathcal{C} \\ 0, & u \notin \mathcal{C} \end{cases}
```

其中 $p$ 是 $y$ 的概率密度函数。

<!-- p.30 -->

<span style="color:#2471a3;">**[example]**</span>

### <span style="color:#c0392b;">例子：良率函数 Yield function</span>

```math
Y(x) = \mathrm{prob}(x + w \in \mathcal{S})
```

- $x \in \mathbb{R}^n$：产品的标称参数值（nominal parameter values）；
- $w \in \mathbb{R}^n$：制造产品中参数的随机偏差（random variations）；
- $\mathcal{S}$：可接受取值的集合（acceptable values）。

若 $\mathcal{S}$ 是凸集且 $w$ 具有对数凹的概率密度，则：

- $Y$ 是对数凹的；
- 良率区域（yield regions）$\{x \mid Y(x) \ge \alpha\}$ 是凸集。

<!-- p.31 -->

<span style="color:#2471a3;">**[definition]**</span>

### <span style="color:#c0392b;">关于广义不等式的凸性 Convexity with respect to generalized inequalities</span>

<span style="color:#00838f;">$f: \mathbb{R}^n \to \mathbb{R}^m$ 是 K-convex（K-凸）的，若 $\mathrm{dom}\,f$ 是凸集，且对 $x, y \in \mathrm{dom}\,f$、$0 \le \theta \le 1$，</span>

```math
f(\theta x + (1-\theta)y) \preceq_K \theta f(x) + (1-\theta) f(y)
```

**例子：** $f: S^m \to S^m$，$f(X) = X^2$ 是 $S^m_{+}$-凸的。

**证明：** 对固定的 $z \in \mathbb{R}^m$，标量函数 $z^T X^2 z = \|Xz\|_2^2$ 关于 $X$ 是凸的，即

```math
z^T (\theta X + (1-\theta) Y)^2 z \le \theta\, z^T X^2 z + (1-\theta)\, z^T Y^2 z
```

对 $X, Y \in S^m$、$0 \le \theta \le 1$ 成立。因此

```math
(\theta X + (1-\theta) Y)^2 \preceq_{S^m_{+}} \theta X^2 + (1-\theta) Y^2
```
