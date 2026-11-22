> <span style="color:#7f8c8d;">Source: SEE官方讲义/02ConvexSets.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

## <span style="color:#c0392b;">2. Convex sets 凸集</span>

<!-- p.1 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">本讲概览 Convex sets 凸集</span>

<span style="color:#2471a3;">**[lecture]**</span> 本讲（原书第 2 章）内容包括：

- 仿射集与凸集（affine and convex sets）
- 若干重要例子（some important examples）
- 保持凸性的运算（operations that preserve convexity）
- 广义不等式（generalized inequalities）
- 分离超平面与支撑超平面（separating and supporting hyperplanes）
- 对偶锥与广义不等式（dual cones and generalized inequalities）

> <span style="color:#7f8c8d;">slide 2–1</span>

<!-- p.2 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Affine set 仿射集</span>

<span style="color:#2471a3;">**[definition]**</span> 过点 $x_1, x_2$ 的直线（line）：所有形如下式的点：

```math
x = \theta x_1 + (1-\theta)x_2, \qquad \theta \in \mathbf{R}
```

<span style="color:#00838f;">**仿射集（affine set）**：包含集合中任意两个不同点之间整条直线的集合。</span>

> <span style="color:#7f8c8d;">图（slide 2–2）：过 $x_1$、$x_2$ 的直线，沿途标注 $\theta = 1.2,\ 1,\ 0.6,\ 0,\ -0.2$；$\theta$ 超出 $[0,1]$ 时点落在线段的延长线上。</span>

例：线性方程组的解集 $\{x \mid Ax = b\}$ 是仿射集（反之，任何仿射集都能表示为某个线性方程组 $Ax = b$ 的解集）。

> <span style="color:#7f8c8d;">slide 2–2</span>

<!-- p.3 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Convex set 凸集</span>

过点 $x_1$ 与 $x_2$ 的线段（line segment）：所有形如 $x = \theta x_1 + (1-\theta)x_2$（其中 $0 \le \theta \le 1$）的点。

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">**凸集（convex set）**：包含集合中任意两点之间线段的集合。</span> 用符号表述：

```math
x_1, x_2 \in C,\quad 0 \le \theta \le 1 \implies \theta x_1 + (1-\theta)x_2 \in C
```

> <span style="color:#7f8c8d;">图（slide 2–3）：三个示例集合，其中一个是凸集，两个是非凸集。</span>

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 为什么整门课把"凸"当作第一道分水岭？从第一性原理看，优化的根本困难在于"局部最优陷阱"：可行集形状不规则时，目标函数可能在多处各有一个局部低谷，梯度法只能保证滑进最近的那个。而凸集加凸函数的组合彻底排除了这种陷阱——任意两个可行点之间的整条线段仍在集合内，目标值沿线段不会"先降后升"，因此任何局部最优都是全局最优。你在 Strang 线代里已经见过这种"封闭性带来好性质"的化身：子空间对线性组合封闭，而线性方程组 $Ax=b$ 的解集是仿射集，这正是本页例子的来源。概率论中指数族分布的最大似然估计也因凸性而具备唯一最优解。后面学到对偶性与 KKT 条件时会看到，凸性是强对偶性成立最常用的充分条件，所以本书先用一整章把"什么集合、什么函数是凸的"彻底工具化。

> <span style="color:#7f8c8d;">slide 2–3</span>

<!-- p.4 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Convex combination and convex hull 凸组合与凸包</span>

$x_1, \dots, x_k$ 的凸组合（convex combination）：任何形如下式的点：

```math
x = \theta_1 x_1 + \theta_2 x_2 + \cdots + \theta_k x_k, \qquad \theta_1 + \cdots + \theta_k = 1,\ \theta_i \ge 0
```

<span style="color:#00838f;">**凸包（convex hull）** $\operatorname{conv} S$：由 $S$ 中点的所有凸组合构成的集合。</span>

> <span style="color:#7f8c8d;">slide 2–4</span>

<!-- p.5 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Convex cone 凸锥</span>

$x_1$ 与 $x_2$ 的锥组合（conic combination，又称非负组合 nonnegative combination）：任何形如下式的点：

```math
x = \theta_1 x_1 + \theta_2 x_2, \qquad \theta_1 \ge 0,\ \theta_2 \ge 0
```

<span style="color:#00838f;">**凸锥（convex cone）**：包含集合中各点的所有锥组合的集合。</span>

> <span style="color:#7f8c8d;">图（slide 2–5）：以原点 $0$ 为顶点、由 $x_1$ 与 $x_2$ 张成的锥形阴影区域。</span>

> <span style="color:#7f8c8d;">slide 2–5</span>

<!-- p.6 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Hyperplanes and halfspaces 超平面与半空间</span>

<span style="color:#00838f;">**超平面（hyperplane）**：形如 $\{x \mid a^Tx = b\}$（$a \ne 0$）的集合。</span>

<span style="color:#00838f;">**半空间（halfspace）**：形如 $\{x \mid a^Tx \le b\}$（$a \ne 0$）的集合。</span>

- $a$ 是法向量（normal vector）；
- 超平面既是仿射集也是凸集；半空间是凸集。

> <span style="color:#7f8c8d;">图（slide 2–6）：上图：超平面 $\{x \mid a^Tx = b\}$、其上的点 $x_0$ 与法向量 $a$；下图：超平面把空间分成 $a^Tx \le b$ 与 $a^Tx \ge b$ 两侧。</span>

> <span style="color:#7f8c8d;">slide 2–6</span>

<!-- p.7 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Euclidean balls and ellipsoids 欧氏球与椭球</span>

以 $x_c$ 为中心、$r$ 为半径的（欧氏）球（(Euclidean) ball）：

```math
B(x_c, r) = \{x \mid \|x - x_c\|_2 \le r\} = \{x_c + ru \mid \|u\|_2 \le 1\}
```

<span style="color:#00838f;">**椭球（ellipsoid）**：形如下式的集合，其中 $P \in S^n_{++}$（即 $P$ 对称正定 symmetric positive definite）：</span>

```math
\{x \mid (x - x_c)^T P^{-1} (x - x_c) \le 1\}
```

另一种表示：$\{x_c + Au \mid \|u\|_2 \le 1\}$，其中 $A$ 为方阵且非奇异（nonsingular）。

> <span style="color:#7f8c8d;">图（slide 2–7）：椭球及其中心 $x_c$。</span>

> <span style="color:#7f8c8d;">slide 2–7</span>

<!-- p.8 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Norm balls and norm cones 范数球与范数锥</span>

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">**范数（norm）**：满足以下条件的函数 $\|\cdot\|$：</span>

- $\|x\| \ge 0$，且 $\|x\| = 0$ 当且仅当 $x = 0$；
- $\|tx\| = |t|\,\|x\|$，对任意 $t \in \mathbf{R}$；
- $\|x + y\| \le \|x\| + \|y\|$（三角不等式）。

记号：$\|\cdot\|$ 表示一般（未具体指明的）范数；$\|\cdot\|_{\mathrm{symb}}$ 表示下标符号所指的某个特定范数。

范数球（norm ball）：中心为 $x_c$、半径为 $r$ 的集合 $\{x \mid \|x - x_c\| \le r\}$。

范数锥（norm cone）：$\{(x, t) \mid \|x\| \le t\}$。欧氏范数对应的范数锥称为二阶锥（second-order cone）。

> <span style="color:#7f8c8d;">图（slide 2–8）：三维 $(x_1, x_2, t)$ 空间中的范数锥，横轴 $x_1, x_2$ 刻度从 $-1$ 到 $1$，纵轴 $t$ 刻度 $0$ 到 $1$。</span>

范数球与范数锥都是凸集。

> <span style="color:#7f8c8d;">slide 2–8</span>

<!-- p.9 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Polyhedra 多面体</span>

<span style="color:#00838f;">**多面体（polyhedron）**：有限个线性不等式与等式的解集</span>

```math
Ax \preceq b, \qquad Cx = d
```

其中 $A \in \mathbf{R}^{m \times n}$，$C \in \mathbf{R}^{p \times n}$，$\preceq$ 表示分量不等式（componentwise inequality）。

> <span style="color:#7f8c8d;">图（slide 2–9）：平面上的多面体 $P$，由 $a_1, \dots, a_5$ 五个半空间的边界线围成。</span>

多面体是有限个半空间与超平面的交。

> <span style="color:#7f8c8d;">slide 2–9</span>

<!-- p.10 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Positive semidefinite cone 半正定锥</span>

记号：

- $S^n$：全体 $n \times n$ 对称矩阵（symmetric matrices）构成的集合；
- $S^n_+ = \{X \in S^n \mid X \succeq 0\}$：半正定（positive semidefinite）的 $n \times n$ 矩阵；
- $S^n_{++} = \{X \in S^n \mid X \succ 0\}$：正定（positive definite）的 $n \times n$ 矩阵。

```math
X \in S^n_+ \iff z^T X z \ge 0 \quad \text{for all } z
```

$S^n_+$ 是一个凸锥。

例：

```math
\begin{bmatrix} x & y \\ y & z \end{bmatrix} \in S^2_+
```

> <span style="color:#7f8c8d;">图（slide 2–10）：集合 $\left\{\begin{bmatrix} x & y \\ y & z \end{bmatrix} \in S^2_+\right\}$ 在 $(x, y, z)$ 三维空间中呈锥形区域（冰淇淋蛋筒状）。</span>

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 半正定（positive semidefinite）这个概念你在 Strang 线代里反复打过交道：$X \succeq 0$ 的二次型定义"$z^TXz \ge 0$ 对所有 $z$ 成立"，等价于 $X$ 的全部特征值非负——这正是 18.06 正定性一讲的核心判据，也等价于存在 Cholesky 分解 $X = LL^T$。三种刻画各有分工：特征值便于数值判断，Cholesky 分解便于高效验证与采样，而二次型定义最容易用来证凸性——把 $z$ 固定后，$z^TXz \ge 0$ 就是一条关于矩阵元素的线性约束，整个锥相当于无穷多条半空间的交，而交保凸，所以 $S^n_+$ 自动是凸锥。图中"冰淇淋蛋筒"形状来自二维情形：边界抛物面 $xz - y^2 = 0$ 恰是行列式为零的秩 1 矩阵。记住这个锥，它是半定规划（SDP）可行域的骨架，也是后面矩阵不等式 $X \preceq Y$ 的语义载体。

> <span style="color:#7f8c8d;">slide 2–10</span>

<!-- p.11 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Operations that preserve convexity 保持凸性的运算</span>

建立集合 $C$ 凸性的实用方法：

1. 直接按定义验证：$x_1, x_2 \in C,\ 0 \le \theta \le 1 \implies \theta x_1 + (1-\theta)x_2 \in C$；
2. 证明 $C$ 是由简单凸集（超平面、半空间、范数球等）经过保持凸性的运算得到的，这些运算包括：
   - 交（intersection）
   - 仿射函数（affine functions）
   - 透视函数（perspective function）
   - 线性分式函数（linear-fractional functions）

> <span style="color:#7f8c8d;">slide 2–11</span>

<!-- p.12 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Intersection 交</span>

<span style="color:#c0392b;">（任意多个）凸集的交仍是凸集。</span>

例：$S = \{x \in \mathbf{R}^m \mid |p(t)| \le 1\ \text{for}\ |t| \le \pi/3\}$，其中

```math
p(t) = x_1 \cos t + x_2 \cos 2t + \cdots + x_m \cos mt
```

$m = 2$ 时的情形：

> <span style="color:#7f8c8d;">图（slide 2–12）：上图：三角多项式 $p(t)$ 的波形，要求在 $|t| \le \pi/3$ 内满足 $|p(t)| \le 1$（纵轴 $p(t)$ 刻度 $-1$ 到 $1$，横轴 $t$ 到 $\pi$）；下图：满足约束的系数 $(x_1, x_2)$ 构成的集合 $S$。</span>

> <span style="color:#7f8c8d;">slide 2–12</span>

<!-- p.13 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Affine function 仿射函数</span>

设 $f : \mathbf{R}^n \to \mathbf{R}^m$ 是仿射函数（affine function），即 $f(x) = Ax + b$，其中 $A \in \mathbf{R}^{m \times n}$，$b \in \mathbf{R}^m$。

- 凸集在 $f$ 下的像（image）是凸集：若 $S \subseteq \mathbf{R}^n$ 是凸集，则 $f(S) = \{f(x) \mid x \in S\}$ 是凸集；
- 凸集在 $f$ 下的原像（inverse image）是凸集：若 $C \subseteq \mathbf{R}^m$ 是凸集，则 $f^{-1}(C) = \{x \in \mathbf{R}^n \mid f(x) \in C\}$ 是凸集。

例：

- 缩放（scaling）、平移（translation）、投影（projection）；
- 线性矩阵不等式（linear matrix inequality）的解集 $\{x \mid x_1A_1 + \cdots + x_mA_m \preceq B\}$（其中 $A_i, B \in S^p$）；
- 双曲锥（hyperbolic cone）$\{x \mid x^TPx \le (c^Tx)^2,\ c^Tx \ge 0\}$（其中 $P \in S^n_+$）。

> <span style="color:#7f8c8d;">slide 2–13</span>

<!-- p.14 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Perspective and linear-fractional function 透视函数与线性分式函数</span>

<span style="color:#2471a3;">**[definition]**</span> 透视函数（perspective function）$P : \mathbf{R}^{n+1} \to \mathbf{R}^n$：

```math
P(x, t) = x/t, \qquad \operatorname{dom} P = \{(x, t) \mid t > 0\}
```

凸集在透视函数下的像与原像都是凸集。

<span style="color:#2471a3;">**[definition]**</span> 线性分式函数（linear-fractional function）$f : \mathbf{R}^n \to \mathbf{R}^m$：

```math
f(x) = \frac{Ax + b}{c^Tx + d}, \qquad \operatorname{dom} f = \{x \mid c^Tx + d > 0\}
```

凸集在线性分式函数下的像与原像都是凸集。

> <span style="color:#7f8c8d;">slide 2–14</span>

<!-- p.15 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Example of a linear-fractional function 线性分式函数示例</span>

<span style="color:#2471a3;">**[example]**</span> 取线性分式函数

```math
f(x) = \frac{1}{x_1 + x_2 + 1}\, x
```

> <span style="color:#7f8c8d;">图（slide 2–15）：左图：$\mathbf{R}^2$ 中的凸集 $C$（坐标 $x_1, x_2$）；右图：经 $f$ 映射得到的像 $f(C)$，两图坐标范围均为 $-1$ 到 $1$。</span>

> <span style="color:#7f8c8d;">slide 2–15</span>

<!-- p.16 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Generalized inequalities 广义不等式</span>

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">凸锥 $K \subseteq \mathbf{R}^n$ 若满足以下三个条件，则称为**正常锥（proper cone）**：</span>

- $K$ 是闭的（closed，包含其边界）；
- $K$ 是实心的（solid，具有非空内部）；
- $K$ 是尖的（pointed，不包含整条直线）。

例：

- 非负卦限（nonnegative orthant）：$K = \mathbf{R}^n_+ = \{x \in \mathbf{R}^n \mid x_i \ge 0,\ i = 1, \dots, n\}$；
- 半正定锥：$K = S^n_+$；
- $[0, 1]$ 上取非负值的多项式（系数向量构成的锥）：

```math
K = \{x \in \mathbf{R}^n \mid x_1 + x_2 t + x_3 t^2 + \cdots + x_n t^{n-1} \ge 0 \text{ for } t \in [0, 1]\}
```

> <span style="color:#7f8c8d;">slide 2–16</span>

<!-- p.17 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Generalized inequality induced by a proper cone 由正常锥定义的广义不等式</span>

由正常锥 $K$ 定义的广义不等式（generalized inequality）：

```math
x \preceq_K y \iff y - x \in K, \qquad x \prec_K y \iff y - x \in \operatorname{int} K
```

例：

- 分量不等式（$K = \mathbf{R}^n_+$）：$x \preceq_{\mathbf{R}^n_+} y \iff x_i \le y_i,\ i = 1, \dots, n$；
- 矩阵不等式（$K = S^n_+$）：$X \preceq_{S^n_+} Y \iff Y - X$ 半正定（positive semidefinite）。

这两类不等式极其常用，以至于我们省略 $\preceq_K$ 的下标 $K$，直接记作 $\preceq$。

性质：$\preceq_K$ 的许多性质与 $\mathbf{R}$ 上的 $\le$ 类似，例如

```math
x \preceq_K y,\ u \preceq_K v \implies x + u \preceq_K y + v
```

> <span style="color:#7f8c8d;">slide 2–17</span>

<!-- p.18 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Minimum and minimal elements 最小元与极小元</span>

$\preceq_K$ 一般不是全序（linear ordering）：可能出现 $x \npreceq_K y$ 且 $y \npreceq_K x$ 同时成立。

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">若 $y \in S \implies x \preceq_K y$，则称 $x \in S$ 是 $S$ 关于 $\preceq_K$ 的**最小元（minimum element）**。</span>

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">若 $y \in S,\ y \preceq_K x \implies y = x$，则称 $x \in S$ 是 $S$ 关于 $\preceq_K$ 的一个**极小元（minimal element）**。</span>

例（$K = \mathbf{R}^2_+$）：$x_1$ 是 $S_1$ 的最小元；$x_2$ 是 $S_2$ 的一个极小元。

> <span style="color:#7f8c8d;">图（slide 2–18）：非负象限中的两个集合 $S_1$ 与 $S_2$：$S_1$ 含最小元 $x_1$；$S_2$ 无最小元，$x_2$ 只是不可再改进的极小元。</span>

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 本页"最小元 vs 极小元"的区分，本质上是离散数学 6.042J 里偏序（partial order）与全序（total order）之别的翻版：$\preceq_K$ 满足自反、传递、反对称，是一个偏序，但两个元素可能互不可比，就像 Hasse 图上没有连线的一对节点。$\mathbf{R}$ 上的 $\le$ 是全序，所以最小元与极小元必然重合；而在 $\mathbf{R}^2_+$ 中，$(0,1)$ 与 $(1,0)$ 谁也不"小于"谁，二者都是极小元，集合却没有最小元。优化理论为何要保留这对概念？因为多目标优化与 Pareto 分析（见本讲末页的生产前沿）中，"不可再改进"的极小性才是现实的解概念，强求唯一最小元常常导致无解。下一页将用对偶锥的权重向量 $\lambda$ 来检验极小性，那是这套语言第一次真正发挥威力。

> <span style="color:#7f8c8d;">slide 2–18</span>

<!-- p.19 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Separating hyperplane theorem 分离超平面定理</span>

<span style="color:#2471a3;">**[theorem]**</span> <span style="color:#c0392b;">**分离超平面定理（separating hyperplane theorem）**：若 $C$ 与 $D$ 是互不相交（disjoint）的凸集，则存在 $a \ne 0$ 与 $b$，使得对 $x \in C$ 有 $a^Tx \le b$，对 $x \in D$ 有 $a^Tx \ge b$。</span>

> <span style="color:#7f8c8d;">图（slide 2–19）：凸集 $C$ 与 $D$ 被超平面 $\{x \mid a^Tx = b\}$ 分开，法向量 $a$ 指向 $a^Tx \ge b$ 一侧。</span>

超平面 $\{x \mid a^Tx = b\}$ 分离（separate）$C$ 与 $D$。严格分离（strict separation）需要附加假设（例如 $C$ 是闭集，$D$ 是单元素集）。

> <span style="color:#7f8c8d;">slide 2–19</span>

<!-- p.20 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Supporting hyperplane theorem 支撑超平面定理</span>

集合 $C$ 在边界点 $x_0$ 处的支撑超平面（supporting hyperplane）：

```math
\{x \mid a^Tx = a^Tx_0\}
```

其中 $a \ne 0$，且对所有 $x \in C$ 有 $a^Tx \le a^Tx_0$。

> <span style="color:#7f8c8d;">图（slide 2–20）：凸集 $C$、其边界点 $x_0$ 与支撑超平面的法向量 $a$。</span>

<span style="color:#2471a3;">**[theorem]**</span> <span style="color:#c0392b;">**支撑超平面定理（supporting hyperplane theorem）**：若 $C$ 是凸集，则在 $C$ 的每个边界点处都存在支撑超平面。</span>

> <span style="color:#7f8c8d;">slide 2–20</span>

<!-- p.21 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Dual cones and generalized inequalities 对偶锥与广义不等式</span>

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">锥 $K$ 的**对偶锥（dual cone）**：</span>

```math
K^* = \{y \mid y^Tx \ge 0 \text{ for all } x \in K\}
```

例：

- $K = \mathbf{R}^n_+$：$K^* = \mathbf{R}^n_+$；
- $K = S^n_+$：$K^* = S^n_+$；
- $K = \{(x, t) \mid \|x\|_2 \le t\}$：$K^* = \{(x, t) \mid \|x\|_2 \le t\}$；
- $K = \{(x, t) \mid \|x\|_1 \le t\}$：$K^* = \{(x, t) \mid \|x\|_\infty \le t\}$。

前三个例子是自对偶锥（self-dual cones）。

正常锥的对偶锥仍是正常锥，因此可以定义广义不等式：

```math
y \succeq_{K^*} 0 \iff y^Tx \ge 0 \text{ for all } x \succeq_K 0
```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 对偶锥（dual cone）的直觉完全可以从 Strang 线代的正交性出发搭建：$y^Tx$ 是内积，$K^* = \{y \mid y^Tx \ge 0,\ \forall x \in K\}$ 就是"与锥中每个向量夹角都不超过 $90°$ 的那些 $y$"，相当于把 $K$ 包装成一族半空间。$\ell_1$ 锥的对偶是 $\ell_\infty$ 锥这条尤其值得记住：它正是 18.065 中对偶范数恒等式 $\|x\|_1 = \max_{\|z\|_\infty \le 1} z^Tx$ 的几何化身，而这两种范数直接支撑着后面稀疏正则化（LASSO）的理论。非负卦限、半正定锥、二阶锥三者自对偶（self-dual）并非巧合：正是这种"对偶变换下的不变性"，让 LP、SDP、SOCP 三大家族各自拥有对称而漂亮的对偶理论。到第 5 章学对偶性时会看到，弱对偶性不等式的证明本质上就是一次对偶锥的成员检验。

> <span style="color:#7f8c8d;">slide 2–21</span>

<!-- p.22 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Minimum and minimal elements via dual inequalities 用对偶不等式刻画最小元与极小元</span>

关于 $\preceq_K$ 的最小元（minimum element）：$x$ 是 $S$ 的最小元，当且仅当对所有 $\lambda \succ_{K^*} 0$，$x$ 都是 $\lambda^Tz$ 在 $S$ 上的唯一最小化点（unique minimizer）。

> <span style="color:#7f8c8d;">图（slide 2–22 上）：对每个方向 $\lambda$，线性函数 $\lambda^Tz$ 在 $S$ 上的唯一最小化点都是同一个点 $x$。</span>

关于 $\preceq_K$ 的极小元（minimal element）：

- 若 $x$ 对某个 $\lambda \succ_{K^*} 0$ 在 $S$ 上最小化 $\lambda^Tz$，则 $x$ 是极小元；

> <span style="color:#7f8c8d;">图（slide 2–22 下）：不同方向 $\lambda_1, \lambda_2$ 各自"托"出 $S$ 的极小点 $x_1, x_2$。</span>

- 反之，若 $x$ 是凸集 $S$ 的极小元，则存在非零的 $\lambda \succeq_{K^*} 0$，使得 $x$ 在 $S$ 上最小化 $\lambda^Tz$。

> <span style="color:#7f8c8d;">slide 2–22</span>

<!-- p.23 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Optimal production frontier 最优生产前沿</span>

- 不同生产方法消耗不同数量的资源向量 $x \in \mathbf{R}^n$；
- 生产集（production set）$P$：所有可能的生产方法对应的资源向量 $x$ 构成的集合；
- 有效（Pareto 最优，Pareto optimal）的生产方法对应关于 $\mathbf{R}^n_+$ 极小的资源向量 $x$。

例（$n = 2$）：$x_1, x_2, x_3$ 是有效的；$x_4, x_5$ 不是。

> <span style="color:#7f8c8d;">图（slide 2–23）：两轴分别为劳动（labor）与燃料（fuel）的资源平面上的生产集 $P$，$\lambda$ 为检验极小性的权重方向；$x_1, x_2, x_3$ 位于 Pareto 前沿上（有效），$x_4, x_5$ 可被改进（非有效）。</span>

> <span style="color:#7f8c8d;">slide 2–23</span>
