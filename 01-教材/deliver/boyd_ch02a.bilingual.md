# <span style="color:#c0392b;">Boyd《凸优化》第 2 章 凸集（上：§2.1–2.2）</span>

> <span style="color:#7f8c8d;">Source: 教材/Convex Optimization (Boyd, Vandenberghe).pdf · Chapter 2 §2.1–2.2, pp.21–35 (PDF p.35–49) · 行内双语翻译版 · 译者：ZCode（bilingual-translator 铁律）</span>

## <span style="color:#c0392b;">2.1 Affine and convex sets（仿射集与凸集）</span>

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">2.1.1 Lines and line segments（直线与线段）</span>

<!-- p.35 -->

设 $x_1 \neq x_2$ 是 $\mathbb{R}^n$ 中的两个点。形如

$$
y = \theta x_1 + (1-\theta)x_2,\qquad \theta\in\mathbb{R}
$$

的点构成穿过 $x_1$ 与 $x_2$ 的 line（直线）。参数值 $\theta=0$ 对应 $y=x_2$，$\theta=1$ 对应 $y=x_1$；介于 0 与 1 之间的 $\theta$ 对应 $x_1$ 与 $x_2$ 之间的（闭）line segment（线段）。

把 $y$ 写成

$$
y = x_2 + \theta(x_1 - x_2)
$$

给出另一种解释：$y$ 是 base point（基点）$x_2$（对应 $\theta=0$）加上 direction（方向）$x_1-x_2$（从 $x_2$ 指向 $x_1$）按参数 $\theta$ 缩放后的和。于是 $\theta$ 给出了 $y$ 在「从 $x_2$ 到 $x_1$ 的路途」中所占的比例：$\theta$ 从 0 增加到 1 时，点 $y$ 从 $x_2$ 移动到 $x_1$；当 $\theta>1$ 时，$y$ 位于直线上 $x_1$ 之外的部分。（见图 2.1。）

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">2.1.2 Affine sets（仿射集）</span>

<!-- p.36 -->

若集合 $C\subseteq\mathbb{R}^n$ 中任意两个不同点之间的直线仍在 $C$ 内，即对任意 $x_1,x_2\in C$ 与任意 $\theta\in\mathbb{R}$ 都有 $\theta x_1+(1-\theta)x_2\in C$，则称 $C$ 是 affine（仿射）的。换句话说：只要线性组合的系数之和为 1，$C$ 就包含其中任意两点的线性组合。

这个想法可以推广到多于两个点。我们把形如 $\theta_1x_1+\cdots+\theta_kx_k$（其中 $\theta_1+\cdots+\theta_k=1$）的点称为点 $x_1,\dots,x_k$ 的 affine combination（仿射组合）。从仿射集的定义（它包含其中任意两点的仿射组合）出发，用归纳法可以证明：仿射集包含其点的所有仿射组合——若 $C$ 是仿射集，$x_1,\dots,x_k\in C$，且 $\theta_1+\cdots+\theta_k=1$，则 $\theta_1x_1+\cdots+\theta_kx_k\in C$。

若 $C$ 是仿射集且 $x_0\in C$，则集合

$$
V = C - x_0 = \{x - x_0 \mid x\in C\}
$$

是一个 subspace（子空间），即对加法与标量乘法封闭。为看出这一点，设 $v_1,v_2\in V$，$\alpha,\beta\in\mathbb{R}$，则 $v_1+x_0\in C$，$v_2+x_0\in C$，于是

$$
\alpha v_1 + \beta v_2 + x_0 = \alpha(v_1+x_0) + \beta(v_2+x_0) + (1-\alpha-\beta)x_0 \in C,
$$

因为 $C$ 仿射且 $\alpha+\beta+(1-\alpha-\beta)=1$。由 $\alpha v_1+\beta v_2+x_0\in C$ 得 $\alpha v_1+\beta v_2\in V$。

因此仿射集 $C$ 可以表示为

$$
C = V + x_0 = \{v + x_0 \mid v\in V\},
$$

即一个子空间加一个 offset（偏移）。与仿射集 $C$ 相联系的子空间 $V$ 不依赖于 $x_0$ 的选取，所以 $x_0$ 可取 $C$ 中任意点。我们定义仿射集 $C$ 的 dimension（维数）为子空间 $V=C-x_0$（$x_0$ 为 $C$ 中任意元素）的维数。

<span style="color:#00838f;">**[例 2.1]** **线性方程组的解集。** 线性方程组的解集 $C=\{x \mid Ax=b\}$（$A\in\mathbb{R}^{m\times n}$，$b\in\mathbb{R}^m$）是仿射集。证明：设 $x_1,x_2\in C$，即 $Ax_1=b$，$Ax_2=b$，则对任意 $\theta$，

$$
A(\theta x_1+(1-\theta)x_2) = \theta Ax_1 + (1-\theta)Ax_2 = \theta b + (1-\theta)b = b,
$$

即仿射组合 $\theta x_1+(1-\theta)x_2$ 也在 $C$ 中。与 $C$ 相联系的子空间是 $A$ 的 nullspace（零空间）。

反之亦然：每个仿射集都可以表示为某个线性方程组的解集。

<!-- p.37 -->

集合 $C\subseteq\mathbb{R}^n$ 中点的所有仿射组合组成的集合称为 $C$ 的 affine hull（仿射包），记作 $\mathbf{aff}\, C$：

$$
\mathbf{aff}\, C = \{\theta_1x_1+\cdots+\theta_kx_k \mid x_1,\dots,x_k\in C,\; \theta_1+\cdots+\theta_k=1\}.
$$

仿射包是包含 $C$ 的最小仿射集，含义是：若 $S$ 是任何满足 $C\subseteq S$ 的仿射集，则 $\mathbf{aff}\, C\subseteq S$。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">2.1.3 Affine dimension and relative interior（仿射维数与相对内部）</span>

我们定义集合 $C$ 的 affine dimension（仿射维数）为其仿射包的维数。仿射维数在凸分析与优化中很有用，但并不总是与其他维数定义一致。例如考虑 $\mathbb{R}^2$ 中的 unit circle（单位圆）$\{x\in\mathbb{R}^2 \mid x_1^2+x_2^2=1\}$：它的仿射包是整个 $\mathbb{R}^2$，因此仿射维数是 2；但按大多数维数定义，$\mathbb{R}^2$ 中单位圆的维数是 1。

若集合 $C\subseteq\mathbb{R}^n$ 的仿射维数小于 $n$，则该集合位于仿射集 $\mathbf{aff}\, C\neq\mathbb{R}^n$ 中。我们把 $C$ 的 relative interior（相对内部）定义为它在 $\mathbf{aff}\, C$ 意义下的内部，记作 $\mathbf{relint}\, C$：

$$
\mathbf{relint}\, C = \{x\in C \mid B(x,r)\cap\mathbf{aff}\, C \subseteq C \text{ 对某个 } r>0\},
$$

其中 $B(x,r)=\{y \mid \|y-x\|\le r\}$ 是范数 $\|\cdot\|$ 下以 $x$ 为心、半径 $r$ 的球。（这里 $\|\cdot\|$ 是任意范数；所有范数定义出同一个相对内部。）进而可定义集合 $C$ 的 relative boundary（相对边界）为 $\mathbf{cl}\, C \setminus \mathbf{relint}\, C$，其中 $\mathbf{cl}\, C$ 是 $C$ 的 closure（闭包）。

<span style="color:#00838f;">**[例 2.2]** 考虑 $\mathbb{R}^3$ 中 $(x_1,x_2)$ 平面上的正方形

$$
C = \{x\in\mathbb{R}^3 \mid -1\le x_1\le 1,\; -1\le x_2\le 1,\; x_3=0\}.
$$

它的仿射包是 $(x_1,x_2)$ 平面，即 $\mathbf{aff}\, C=\{x\in\mathbb{R}^3 \mid x_3=0\}$。$C$ 的内部为空，但相对内部为

$$
\mathbf{relint}\, C = \{x\in\mathbb{R}^3 \mid -1<x_1<1,\; -1<x_2<1,\; x_3=0\}.
$$

它在 $\mathbb{R}^3$ 中的边界就是它自己；它的相对边界是线框轮廓 $\{x\in\mathbb{R}^3 \mid \max\{|x_1|,|x_2|\}=1,\; x_3=0\}$。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">2.1.4 Convex sets（凸集）</span>

<!-- p.38 -->

若集合 $C$ 中任意两点之间的线段仍在 $C$ 内，即对任意 $x_1,x_2\in C$ 与任意满足 $0\le\theta\le1$ 的 $\theta$ 都有

$$
\theta x_1 + (1-\theta)x_2 \in C,
$$

则称 $C$ 是 convex（凸）的。粗略地说：凸集就是集合中每一点都能被其他每一点「看见」——沿着两点之间一条 unobstructed（无遮挡）的直线路径，而无遮挡的意思是路径全程位于集合内。每个仿射集都是凸集，因为它包含任意两点间的整条直线，当然也包含线段。图 2.2 展示了 $\mathbb{R}^2$ 中一些简单的凸集与非凸集。

我们称形如 $\theta_1x_1+\cdots+\theta_kx_k$（其中 $\theta_1+\cdots+\theta_k=1$ 且 $\theta_i\ge0,\; i=1,\dots,k$）的点为点 $x_1,\dots,x_k$ 的 convex combination（凸组合）。与仿射集一样，可以证明：集合是凸的当且仅当它包含其点的所有凸组合。点的凸组合可以看作这些点的 mixture（混合）或 weighted average（加权平均），$\theta_i$ 就是 $x_i$ 在混合中所占的比例。

<!-- p.38/39 -->

集合 $C$ 的 convex hull（凸包）记作 $\mathbf{conv}\, C$，是 $C$ 中点的所有凸组合的集合：

$$
\mathbf{conv}\, C = \{\theta_1x_1+\cdots+\theta_kx_k \mid x_i\in C,\; \theta_i\ge0,\; \theta_1+\cdots+\theta_k=1\}.
$$

顾名思义，$\mathbf{conv}\, C$ 总是凸的。它是包含 $C$ 的最小凸集：若 $B$ 是任何包含 $C$ 的凸集，则 $\mathbf{conv}\, C\subseteq B$。图 2.3 展示了凸包的定义。

凸组合的概念可以推广到无穷级数、积分，以及最一般形式的 probability distributions（概率分布）。设 $\theta_1,\theta_2,\dots$ 满足

$$
\theta_i\ge0,\; i=1,2,\dots,\qquad \sum_{i=1}^{\infty}\theta_i = 1,
$$

且 $x_1,x_2,\dots\in C$（$C\subseteq\mathbb{R}^n$ 凸），则（若级数收敛）

$$
\sum_{i=1}^{\infty}\theta_i x_i \in C.
$$

更一般地，设 $p:\mathbb{R}^n\to\mathbb{R}$ 对所有 $x\in C$ 满足 $p(x)\ge0$ 且 $\int_C p(x)\,dx=1$（$C\subseteq\mathbb{R}^n$ 凸），则（若积分存在）$\int_C p(x)x\,dx\in C$。

在最一般的形式中：设 $C\subseteq\mathbb{R}^n$ 凸，$x$ 是随机向量且 $x\in C$ 以概率 1 成立，则 $\mathbb{E}\, x\in C$。事实上这一形式包含前面所有形式作为特例：例如设随机变量 $x$ 只取两个值 $x_1,x_2$，$\mathrm{prob}(x=x_1)=\theta$，$\mathrm{prob}(x=x_2)=1-\theta$（$0\le\theta\le1$），则 $\mathbb{E}\,x=\theta x_1+(1-\theta)x_2$，回到了两点的简单凸组合。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">2.1.5 Cones（锥）</span>

<!-- p.39 -->

若对每个 $x\in C$ 与每个 $\theta\ge0$ 都有 $\theta x\in C$，则称集合 $C$ 为 cone（锥），或 nonnegative homogeneous（非负齐次）的。若 $C$ 既是凸集又是锥，则称它为 convex cone（凸锥），即对任意 $x_1,x_2\in C$ 与 $\theta_1,\theta_2\ge0$，有

$$
\theta_1x_1+\theta_2x_2\in C.
$$

这种形式的点在几何上构成以 0 为 apex（顶点）、边缘穿过 $x_1$ 与 $x_2$ 的二维 pie slice（扇形楔）。（见图 2.4。）

形如 $\theta_1x_1+\cdots+\theta_kx_k$（$\theta_1,\dots,\theta_k\ge0$）的点称为 $x_1,\dots,x_k$ 的 conic combination（锥组合）或 nonnegative linear combination（非负线性组合）。若 $x_i$ 属于凸锥 $C$，则它们的每个锥组合都属于 $C$；反之，$C$ 是凸锥当且仅当它包含其元素的所有锥组合。与凸（或仿射）组合一样，锥组合的概念也可推广到无穷和与积分。

集合 $C$ 的 conic hull（锥包）是 $C$ 中点的所有锥组合的集合，即

$$
\{\theta_1x_1+\cdots+\theta_kx_k \mid x_i\in C,\; \theta_i\ge0,\; i=1,\dots,k\},
$$

它也是包含 $C$ 的最小凸锥（见图 2.5）。

## <span style="color:#c0392b;">2.2 Some important examples（一些重要的例子）</span>

<!-- p.41 -->

本节描述全书会反复遇到的一些重要凸集例子。先看几个简单例子：

- empty set（空集）$\varnothing$、任何单点（singleton，单元素集）$\{x_0\}$、以及全空间 $\mathbb{R}^n$，都是 $\mathbb{R}^n$ 的仿射（从而凸）子集。
- 任何 line（直线）都是仿射的；若它穿过零点，则它是子空间，从而也是凸锥。
- line segment（线段）是凸的，但不仿射（除非退化成一个点）。
- ray（射线）$\{x_0+\theta v \mid \theta\ge0\}$（$v\neq0$）是凸的，但不仿射；若其 base（基点）$x_0=0$，则它是凸锥。
- 任何 subspace（子空间）都是仿射的，也是凸锥（从而是凸的）。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">2.2.1 Hyperplanes and halfspaces（超平面与半空间）</span>

hyperplane（超平面）是形如

$$
\{x \mid a^Tx = b\}
$$

的集合，其中 $a\in\mathbb{R}^n$，$a\neq0$，$b\in\mathbb{R}$。从分析角度看，它是 $x$ 的分量之间一个非平凡线性方程的解集（因而是仿射集）。从几何角度看，超平面 $\{x\mid a^Tx=b\}$ 可以解释为「与给定向量 $a$ 的 inner product（内积）为常数」的点集，即一个以 $a$ 为 normal vector（法向量）的超平面；常数 $b\in\mathbb{R}$ 决定超平面相对原点的 offset（偏移）。把超平面写成

$$
\{x \mid a^T(x-x_0)=0\}
$$

即可理解这一几何解释，其中 $x_0$ 是超平面上的任意点（即满足 $a^Tx_0=b$ 的任意点）。这个表示又可写成

$$
\{x \mid a^T(x-x_0)=0\} = x_0 + a^\perp,
$$

其中 $a^\perp$ 表示 $a$ 的 orthogonal complement（正交补），即所有与 $a$ 正交的向量组成的集合：

$$
a^\perp = \{v \mid a^Tv=0\}.
$$

这说明超平面由偏移 $x_0$ 加上所有与（法）向量 $a$ 正交的向量构成（见图 2.6）。

<!-- p.41/42 -->

超平面把 $\mathbb{R}^n$ 分成两个 halfspaces（半空间）。一个（闭）halfspace（半空间）是形如

$$
\{x \mid a^Tx \le b\} \tag{2.1}
$$

的集合，其中 $a\neq0$，即一个（非平凡）线性不等式的解集。半空间是凸的，但不仿射（见图 2.7）。

<!-- p.43 -->

半空间 (2.1) 也可以表示为

$$
\{x \mid a^T(x-x_0)\le0\}, \tag{2.2}
$$

其中 $x_0$ 是相关超平面上的任意点（即满足 $a^Tx_0=b$）。表示 (2.2) 给出一个简单的几何解释：半空间由 $x_0$ 加上所有与（外法向）向量 $a$ 成 obtuse（钝）角或直角的向量构成（见图 2.8）。

半空间 (2.1) 的 boundary（边界）是超平面 $\{x\mid a^Tx=b\}$。集合 $\{x \mid a^Tx<b\}$ 是半空间 $\{x\mid a^Tx\le b\}$ 的内部，称为 open halfspace（开半空间）。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">2.2.2 Euclidean balls and ellipsoids（欧氏球与椭球）</span>

<!-- p.43 -->

$\mathbb{R}^n$ 中的（Euclidean）ball（欧氏球，或简称 ball，球）形如

$$
B(x_c, r) = \{x \mid \|x-x_c\|_2 \le r\} = \{x \mid (x-x_c)^T(x-x_c)\le r^2\},
$$

其中 $r>0$，$\|\cdot\|_2$ 表示 Euclidean norm（欧氏范数），即 $\|u\|_2=(u^Tu)^{1/2}$。向量 $x_c$ 是球的 center（球心），标量 $r$ 是 radius（半径）；$B(x_c,r)$ 由与球心距离不超过 $r$ 的所有点组成。欧氏球另一种常见表示是

$$
B(x_c,r) = \{x_c + ru \mid \|u\|_2\le1\}.
$$

<!-- p.44 -->

欧氏球是凸集：若 $\|x_1-x_c\|_2\le r$，$\|x_2-x_c\|_2\le r$，且 $0\le\theta\le1$，则

$$
\begin{aligned}
\|\theta x_1+(1-\theta)x_2-x_c\|_2
&= \|\theta(x_1-x_c)+(1-\theta)(x_2-x_c)\|_2\\
&\le \theta\|x_1-x_c\|_2 + (1-\theta)\|x_2-x_c\|_2 \le r.
\end{aligned}
$$

（这里用到 $\|\cdot\|_2$ 的齐次性与三角不等式；见 §A.1.2。）

与之相关的一族凸集是 ellipsoids（椭球），形如

$$
\mathcal{E} = \{x \mid (x-x_c)^TP^{-1}(x-x_c)\le1\}, \tag{2.3}
$$

其中 $P=P^T\succ0$，即 $P$ 是对称 positive definite（正定）矩阵。向量 $x_c\in\mathbb{R}^n$ 是椭球的中心。矩阵 $P$ 决定椭球从 $x_c$ 向每个方向延伸多远：$\mathcal{E}$ 的 semi-axes（半轴）长度为 $\sqrt{\lambda_i}$，其中 $\lambda_i$ 是 $P$ 的 eigenvalues（特征值）。球是 $P=r^2I$ 时的椭球。图 2.9 展示了 $\mathbb{R}^2$ 中的一个椭球。

<!-- p.44/45 -->

椭球另一种常见表示是

$$
\mathcal{E} = \{x_c + Au \mid \|u\|_2\le1\}, \tag{2.4}
$$

其中 $A$ 是方阵且 nonsingular（非奇异）。在这个表示中，不失一般性可假设 $A$ 对称正定：取 $A=P^{1/2}$ 即得 (2.3) 定义的椭球。当 (2.4) 中的矩阵 $A$ 对称半正定但 singular（奇异）时，集合 (2.4) 称为 degenerate ellipsoid（退化椭球）；其仿射维数等于 $A$ 的 rank（秩）。退化椭球同样是凸的。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">2.2.3 Norm balls and norm cones（范数球与范数锥）</span>

设 $\|\cdot\|$ 是 $\mathbb{R}^n$ 上的任意范数（见 §A.1.2）。由范数的一般性质可以证明：半径 $r$、中心 $x_c$ 的 norm ball（范数球）$\{x \mid \|x-x_c\|\le r\}$ 是凸的。与范数 $\|\cdot\|$ 相关联的 norm cone（范数锥）是集合

$$
C = \{(x,t) \mid \|x\|\le t\} \subseteq \mathbb{R}^{n+1}.
$$

<!-- p.45 -->

它（顾名思义）是一个凸锥。

<span style="color:#00838f;">**[例 2.3]** **二阶锥（second-order cone）。** 二阶锥是欧氏范数对应的范数锥，即

$$
\begin{aligned}
C &= \{(x,t)\in\mathbb{R}^{n+1} \mid \|x\|_2\le t\}\\
&= \left\{\begin{bmatrix}x\\t\end{bmatrix} \;\middle|\; \begin{bmatrix}x\\t\end{bmatrix}^T\begin{bmatrix}I&0\\0&-1\end{bmatrix}\begin{bmatrix}x\\t\end{bmatrix}\le0,\; t\ge0\right\}.
\end{aligned}
$$

二阶锥还有几个别名：因为它由二次不等式定义，所以叫 quadratic cone（二次锥）；也叫 Lorentz cone（洛伦兹锥）或 ice-cream cone（冰激凌锥）。图 2.10 展示了 $\mathbb{R}^3$ 中的二阶锥。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">2.2.4 Polyhedra（多面体）</span>

<!-- p.45 -->

polyhedron（多面体）定义为有限个线性等式与不等式的解集：

$$
P = \{x \mid a_j^Tx\le b_j,\; j=1,\dots,m,\;\; c_j^Tx=d_j,\; j=1,\dots,p\}. \tag{2.5}
$$

因此多面体是有限个半空间与超平面的 intersection（交）。仿射集（如子空间、超平面、直线）、射线、线段与半空间都是多面体。容易证明多面体是凸集。bounded polyhedron（有界多面体）有时称为 polytope（多胞形），但有些作者使用相反的约定（即 polytope 指任何形如 (2.5) 的集合，polyhedron 指其中有界的）。图 2.11 展示了一个由五个半空间相交定义的多面体。

<!-- p.46 -->

使用紧凑记号很方便：

$$
P = \{x \mid Ax\preceq b,\; Cx=d\} \tag{2.6}
$$

表示 (2.5)，其中

$$
A=\begin{bmatrix}a_1^T\\ \vdots\\ a_m^T\end{bmatrix},\qquad
C=\begin{bmatrix}c_1^T\\ \vdots\\ c_p^T\end{bmatrix},
$$

符号 $\preceq$ 表示 $\mathbb{R}^m$ 中的向量不等式或 componentwise inequality（逐分量不等式）：$u\preceq v$ 意味着 $u_i\le v_i$（$i=1,\dots,m$）。

<span style="color:#00838f;">**[例 2.4]** **非负卦限（nonnegative orthant）。** 非负卦限是分量均非负的点集，即

$$
\mathbb{R}^n_+ = \{x\in\mathbb{R}^n \mid x_i\ge0,\; i=1,\dots,n\} = \{x\in\mathbb{R}^n \mid x\succeq0\}.
$$

（这里 $\mathbb{R}_+$ 表示非负数集：$\mathbb{R}_+=\{x\in\mathbb{R}\mid x\ge0\}$。）非负卦限既是多面体又是锥（因此称为 polyhedral cone，多面锥）。

**Simplexes（单纯形）。** 单纯形是另一类重要的多面体。设 $k+1$ 个点 $v_0,\dots,v_k\in\mathbb{R}^n$ 是 affinely independent（仿射无关）的，即 $v_1-v_0,\dots,v_k-v_0$ 线性无关。由它们确定的 simplex（单纯形）为

$$
C = \mathbf{conv}\{v_0,\dots,v_k\} = \{\theta_0v_0+\cdots+\theta_kv_k \mid \theta\succeq0,\; \mathbf{1}^T\theta=1\}, \tag{2.7}
$$

<!-- p.47 -->

其中 $\mathbf{1}$ 表示全 1 向量。该单纯形的仿射维数为 $k$，因此有时称为 $\mathbb{R}^n$ 中的 $k$-维单纯形。

<span style="color:#00838f;">**[例 2.5]** **几个常见的单纯形。** 1-维单纯形是线段；2-维单纯形是三角形（含内部）；3-维单纯形是四面体。unit simplex（单位单纯形）是由零向量与单位向量确定的 $n$-维单纯形，即由 $0,e_1,\dots,e_n\in\mathbb{R}^n$ 确定，可表示为满足

$$
x\succeq0,\qquad \mathbf{1}^Tx\le1
$$

的向量集合。probability simplex（概率单纯形）是由单位向量 $e_1,\dots,e_n\in\mathbb{R}^n$ 确定的 $(n-1)$-维单纯形，是满足

$$
x\succeq0,\qquad \mathbf{1}^Tx=1
$$

的向量集合。概率单纯形中的向量对应 $n$ 个元素集合上的概率分布，$x_i$ 解释为第 $i$ 个元素的概率。

要把单纯形 (2.7) 描述成多面体（即 (2.6) 的形式），按如下步骤进行。由定义，$x\in C$ 当且仅当 $x=\theta_0v_0+\theta_1v_1+\cdots+\theta_kv_k$，其中某个 $\theta\succeq0$ 且 $\mathbf{1}^T\theta=1$。等价地，定义 $y=(\theta_1,\dots,\theta_k)$ 与

$$
B = [\,v_1-v_0\;\; \cdots\;\; v_k-v_0\,]\in\mathbb{R}^{n\times k},
$$

则可以说：$x\in C$ 当且仅当

$$
x = v_0 + By \tag{2.8}
$$

对某个 $y\succeq0$、$\mathbf{1}^Ty\le1$ 成立。注意点 $v_0,\dots,v_k$ 的仿射无关性意味着矩阵 $B$ 的 rank（秩）为 $k$。因此存在非奇异矩阵 $A=(A_1,A_2)\in\mathbb{R}^{n\times n}$ 使

$$
AB=\begin{bmatrix}A_1\\A_2\end{bmatrix}B=\begin{bmatrix}I\\0\end{bmatrix}.
$$

<!-- p.47/48 -->

用 $A$ 左乘 (2.8) 得

$$
A_1x = A_1v_0 + y,\qquad A_2x = A_2v_0.
$$

由此看出：$x\in C$ 当且仅当 $A_2x=A_2v_0$，且向量 $y=A_1x-A_1v_0$ 满足 $y\succeq0$、$\mathbf{1}^Ty\le1$。换言之：

$$
x\in C \iff A_2x=A_2v_0,\quad A_1x\succeq A_1v_0,\quad \mathbf{1}^TA_1x\le 1+\mathbf{1}^TA_1v_0,
$$

这是关于 $x$ 的一组线性等式与不等式，因此描述一个多面体。

**多面体的凸包描述（Convex hull description of polyhedra）。** 有限集 $\{v_1,\dots,v_k\}$ 的凸包是

$$
\mathbf{conv}\{v_1,\dots,v_k\} = \{\theta_1v_1+\cdots+\theta_kv_k \mid \theta\succeq0,\; \mathbf{1}^T\theta=1\}.
$$

这个集合是多面体且有界，但（除 simplex 等特殊情形外）不容易把它写成 (2.5) 的形式，即用一组线性等式与不等式表达。这种凸包描述的一个推广是

$$
\{\theta_1v_1+\cdots+\theta_kv_k \mid \theta_1+\cdots+\theta_m=1,\; \theta_i\ge0,\; i=1,\dots,k\}, \tag{2.9}
$$

其中 $m\le k$：这里考虑 $v_i$ 的非负线性组合，但只要求前 $m$ 个系数之和为 1。也可以把 (2.9) 解释为点 $v_1,\dots,v_m$ 的凸包加上点 $v_{m+1},\dots,v_k$ 的锥包。集合 (2.9) 定义一个多面体；反之，每个多面体都可以表示成这种形式（虽然我们不证明这一点）。

<!-- p.48 -->

多面体如何表示是一个微妙的问题，而且有非常重要的实际影响。举个简单例子：考虑 $\mathbb{R}^n$ 中 $\ell_\infty$-范数的 unit ball（单位球）

$$
C = \{x \mid |x_i|\le1,\; i=1,\dots,n\}.
$$

集合 $C$ 可以用 (2.5) 的形式以 $2n$ 个线性不等式 $\pm e_i^Tx\le1$ 描述（$e_i$ 是第 $i$ 个单位向量）。而要用凸包形式 (2.9) 描述它至少需要 $2^n$ 个点：$C=\mathbf{conv}\{v_1,\dots,v_{2^n}\}$，其中 $v_1,\dots,v_{2^n}$ 是分量全为 $1$ 或 $-1$ 的所有 $2^n$ 个向量。因此当 $n$ 很大时，两种表示的规模差别巨大。

> <span style="color:#2471a3;">**[译者注｜跨课程连接]** 「同一集合的两种表示规模可以指数级不同」正是 CLRS 里 NP 难问题的影子：多面体的 H-表示（不等式）与 V-表示（顶点凸包）之间的转换，本质上和 0/1 背包里「n 个物品 → 2^n 个子集」是同一类复杂度墙。</span>

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">2.2.5 The positive semidefinite cone（半正定锥）</span>

<!-- p.48 -->

我们用记号 $\mathbf{S}^n$ 表示对称 $n\times n$ 矩阵的集合

$$
\mathbf{S}^n = \{X\in\mathbb{R}^{n\times n} \mid X=X^T\},
$$

它是一个维数为 $n(n+1)/2$ 的向量空间。用 $\mathbf{S}^n_+$ 表示对称 positive semidefinite（半正定）矩阵的集合：

$$
\mathbf{S}^n_+ = \{X\in\mathbf{S}^n \mid X\succeq0\},
$$

用 $\mathbf{S}^n_{++}$ 表示对称 positive definite（正定）矩阵的集合：

$$
\mathbf{S}^n_{++} = \{X\in\mathbf{S}^n \mid X\succ0\}.
$$

（这个记号意在类比 $\mathbb{R}_+$ 表示非负实数、$\mathbb{R}_{++}$ 表示正实数。）

<!-- p.49 -->

集合 $\mathbf{S}^n_+$ 是凸锥：若 $\theta_1,\theta_2\ge0$ 且 $A,B\in\mathbf{S}^n_+$，则 $\theta_1A+\theta_2B\in\mathbf{S}^n_+$。这可以直接从半正定的定义看出：对任意 $x\in\mathbb{R}^n$，

$$
x^T(\theta_1A+\theta_2B)x = \theta_1x^TAx + \theta_2x^TBx \ge 0
$$

（当 $A\succeq0$，$B\succeq0$，$\theta_1,\theta_2\ge0$ 时）。

<span style="color:#00838f;">**[例 2.6]** **$\mathbf{S}^2$ 中的半正定锥。** 我们有

$$
X=\begin{bmatrix}x&y\\y&z\end{bmatrix}\in\mathbf{S}^2_+
\quad\Longleftrightarrow\quad
x\ge0,\; z\ge0,\; xz\ge y^2.
$$

这个锥的边界如图 2.12 所示（在 $\mathbb{R}^3$ 中以 $(x,y,z)$ 绘制）。
