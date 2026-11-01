# <span style="color:#c0392b;">Boyd《凸优化》第 2 章 凸集（中：§2.3–2.4）</span>

> <span style="color:#7f8c8d;">Source: 教材/Convex Optimization (Boyd, Vandenberghe).pdf · Chapter 2 §2.3–2.4, pp.35–46 (PDF p.49–60) · 行内双语翻译版 · 译者：ZCode（bilingual-translator 铁律）</span>

## <span style="color:#c0392b;">2.3 Operations that preserve convexity（保持凸性的运算）</span>

本节描述一些保持集合凸性的运算，或能从其他集合构造出凸集的运算。这些运算与 §2.2 的简单例子一起，构成一套 convex sets calculus（凸集演算），对判定或确立集合的凸性非常有用。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">2.3.1 Intersection（交集）</span>

<!-- p.50 -->

凸性在 intersection（交）下保持：若 $S_1$ 与 $S_2$ 凸，则 $S_1\cap S_2$ 凸。这个性质可推广到无穷多个集合的交：若 $S_\alpha$ 对每个 $\alpha\in\mathcal{A}$ 都是凸的，则 $\bigcap_{\alpha\in\mathcal{A}} S_\alpha$ 凸。（子空间、仿射集与凸锥对任意交也封闭。）简单例子：多面体是半空间与超平面（都是凸的）的交，因此是凸的。

<span style="color:#00838f;">**[例 2.7]** 半正定锥 $\mathbf{S}^n_+$ 可以表示为

```math
\bigcap_{z\neq0} \{X\in\mathbf{S}^n \mid z^TXz\ge0\}.
```

对每个 $z\neq0$，$z^TXz$ 是 $X$ 的一个（非恒零的）linear function（线性函数），所以集合 $\{X\in\mathbf{S}^n \mid z^TXz\ge0\}$ 实际上是 $\mathbf{S}^n$ 中的 halfspaces（半空间）。因此半正定锥是无穷多个半空间的交，从而是凸的。

<span style="color:#00838f;">**[例 2.8]** 考虑集合

```math
S = \{x\in\mathbb{R}^m \mid |p(t)|\le1 \text{ 对 } |t|\le\pi/3\}, \tag{2.10}
```

其中 $p(t)=\sum_{k=1}^m x_k\cos kt$。集合 $S$ 可以表示为无穷多个 slabs（板状体/带状区域）的交：$S=\bigcap_{|t|\le\pi/3} S_t$，其中

```math
S_t = \{x \mid -1\le(\cos t,\dots,\cos mt)^Tx\le1\},
```

因此是凸的。$m=2$ 时该定义与该集合见图 2.13 与 2.14。

<!-- p.50/51 -->

在上面的例子里，我们通过把集合表示成（可能无穷多个）半空间的交来确立凸性。§2.5.1 将给出一个逆命题：每个 closed convex set（闭凸集）都是（通常无穷多个）半空间的交。事实上，闭凸集 $S$ 是所有包含它的半空间的交：

```math
S = \bigcap\{H \mid H \text{ 为半空间},\; S\subseteq H\}.
```

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">2.3.2 Affine functions（仿射函数）</span>

<!-- p.51 -->

回顾：函数 $f:\mathbb{R}^n\to\mathbb{R}^m$ 是 affine（仿射）的，若它是线性函数加常数，即形如 $f(x)=Ax+b$，$A\in\mathbb{R}^{m\times n}$，$b\in\mathbb{R}^m$。设 $S\subseteq\mathbb{R}^n$ 凸，$f:\mathbb{R}^n\to\mathbb{R}^m$ 是仿射函数，则 $S$ 在 $f$ 下的 image（像）

```math
f(S) = \{f(x) \mid x\in S\}
```

是凸的。同样，若 $f:\mathbb{R}^k\to\mathbb{R}^n$ 是仿射函数，则 $S$ 在 $f$ 下的 inverse image（原像/逆像）

```math
f^{-1}(S) = \{x \mid f(x)\in S\}
```

也是凸的。

两个简单例子是 scaling（缩放）与 translation（平移）。若 $S\subseteq\mathbb{R}^n$ 凸，$\alpha\in\mathbb{R}$，$a\in\mathbb{R}^n$，则集合 $\alpha S$ 与 $S+a$ 凸，其中

```math
\alpha S = \{\alpha x \mid x\in S\},\qquad S+a = \{x+a \mid x\in S\}.
```

凸集向部分坐标的 projection（投影）是凸的：若 $S\subseteq\mathbb{R}^m\times\mathbb{R}^n$ 凸，则

```math
T = \{x_1\in\mathbb{R}^m \mid (x_1,x_2)\in S \text{ 对某个 } x_2\in\mathbb{R}^n\}
```

凸。两个集合的 sum（和）定义为

```math
S_1+S_2 = \{x+y \mid x\in S_1,\; y\in S_2\}.
```

若 $S_1$ 与 $S_2$ 凸，则 $S_1+S_2$ 凸。为看出这一点：若 $S_1,S_2$ 凸，则 direct product 或 Cartesian product（笛卡尔积）

```math
S_1\times S_2 = \{(x_1,x_2) \mid x_1\in S_1,\; x_2\in S_2\}
```

也凸，而这个集合在线性函数 $f(x_1,x_2)=x_1+x_2$ 下的像正是 $S_1+S_2$。

<!-- p.52 -->

我们还可以考虑 $S_1,S_2\subseteq\mathbb{R}^n\times\mathbb{R}^m$ 的 partial sum（部分和）：

```math
S = \{(x, y_1+y_2) \mid (x,y_1)\in S_1,\; (x,y_2)\in S_2\},
```

其中 $x\in\mathbb{R}^n$，$y_i\in\mathbb{R}^m$。当 $m=0$ 时部分和给出 $S_1\cap S_2$；当 $n=0$ 时它就是集合加法。凸集的部分和是凸的（见习题 2.16）。

<span style="color:#00838f;">**[例 2.9]** **多面体。** 多面体 $\{x \mid Ax\preceq b,\; Cx=d\}$ 可以表示为非负卦限与原点的笛卡尔积在仿射函数 $f(x)=(b-Ax,\; d-Cx)$ 下的原像：

```math
\{x \mid Ax\preceq b,\; Cx=d\} = \{x \mid f(x)\in\mathbb{R}^m_+\times\{0\}\}.
```

<span style="color:#00838f;">**[例 2.10]** **线性矩阵不等式的解集。** 条件

```math
A(x) = x_1A_1+\cdots+x_nA_n \preceq B \tag{2.11}
```

（其中 $B,A_i\in\mathbf{S}^m$）称为关于 $x$ 的 linear matrix inequality（线性矩阵不等式，LMI）。（注意它与普通线性不等式 $a^Tx=x_1a_1+\cdots+x_na_n\le b$（$b,a_i\in\mathbb{R}$）的相似性。）线性矩阵不等式的解集 $\{x \mid A(x)\preceq B\}$ 是凸的：它正是半正定锥在仿射函数 $f:\mathbb{R}^n\to\mathbf{S}^m$，$f(x)=B-A(x)$ 下的原像。

<span style="color:#00838f;">**[例 2.11]** **双曲锥（hyperbolic cone）。** 集合

```math
\{x \mid x^TPx\le(c^Tx)^2,\; c^Tx\ge0\}
```

（其中 $P\in\mathbf{S}^n_+$，$c\in\mathbb{R}^n$）是凸的，因为它是 second-order cone（二阶锥）

```math
\{(z,t) \mid z^Tz\le t^2,\; t\ge0\}
```

在仿射函数 $f(x)=(P^{1/2}x,\; c^Tx)$ 下的原像。

<span style="color:#00838f;">**[例 2.12]** **椭球。** 椭球

```math
\mathcal{E} = \{x \mid (x-x_c)^TP^{-1}(x-x_c)\le1\}
```

（$P\in\mathbf{S}^n_{++}$）是单位欧氏球 $\{u \mid \|u\|_2\le1\}$ 在仿射映射 $f(u)=P^{1/2}u+x_c$ 下的像。（它也是单位球在仿射映射 $g(x)=P^{-1/2}(x-x_c)$ 下的原像。）

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">2.3.3 Linear-fractional and perspective functions（线性分式函数与透视函数）</span>

<!-- p.53 -->

本节研究一类称为 linear-fractional（线性分式）的函数，它比仿射函数更一般，但仍然保持凸性。

**透视函数（The perspective function）。** 我们定义 perspective function（透视函数）$P:\mathbb{R}^{n+1}\to\mathbb{R}^n$，定义域 $\mathbf{dom}\,P=\mathbb{R}^n\times\mathbb{R}_{++}$，为 $P(z,t)=z/t$。（$\mathbb{R}_{++}$ 表示正数集：$\mathbb{R}_{++}=\{x\in\mathbb{R}\mid x>0\}$。）透视函数把向量 scale（缩放）或 normalize（归一化）使最后一个分量为 1，然后丢掉最后一个分量。

<span style="color:#00838f;">**[注 2.1]** 我们可以把透视函数解释为 pin-hole camera（针孔相机）的作用。$\mathbb{R}^3$ 中的针孔相机由一个不透明的水平平面 $x_3=0$（在原点处有一个允许光线通过的单个针孔）和一个水平 image plane（成像平面）$x_3=-1$ 组成。位于相机上方（即 $x_3>0$）的物体 $x$ 在成像平面上成像于点 $-(x_1/x_3, x_2/x_3, 1)$。丢掉像点的最后一个分量（它总为 $-1$），$x$ 处的点成像于 $y=-(x_1/x_3, x_2/x_3)=-P(x)$。见图 2.15。

<!-- p.53/54 -->

若 $C\subseteq\mathbf{dom}\,P$ 凸，则它的像

```math
P(C) = \{P(x) \mid x\in C\}
```

是凸的。这个结果非常直观：透过针孔相机看一个凸的物体，得到凸的像。为确立这一事实，我们证明线段在透视函数下映为线段（这同样直观：透过针孔相机看线段得到线段的像）。设 $x=(\tilde{x},x_{n+1})$，$y=(\tilde{y},y_{n+1})\in\mathbb{R}^{n+1}$，$x_{n+1}>0$，$y_{n+1}>0$，则对 $0\le\theta\le1$，

```math
P(\theta x+(1-\theta)y) = \frac{\theta\tilde{x}+(1-\theta)\tilde{y}}{\theta x_{n+1}+(1-\theta)y_{n+1}} = \mu P(x) + (1-\mu)P(y),
```

其中

```math
\mu = \frac{\theta x_{n+1}}{\theta x_{n+1}+(1-\theta)y_{n+1}}\in[0,1].
```

<!-- p.54 -->

$\theta$ 与 $\mu$ 之间的对应是单调的：当 $\theta$ 在 0 与 1 之间变化（扫出线段 $[x,y]$）时，$\mu$ 也在 0 与 1 之间变化（扫出线段 $[P(x),P(y)]$）。这表明 $P([x,y])=[P(x),P(y)]$。

现在设 $C$ 凸且 $C\subseteq\mathbf{dom}\,P$（即对一切 $x\in C$ 有 $x_{n+1}>0$），$x,y\in C$。为确立 $P(C)$ 的凸性，须证线段 $[P(x),P(y)]$ 在 $P(C)$ 内；但该线段正是线段 $[x,y]$ 在 $P$ 下的像，故属于 $P(C)$。

凸集在透视函数下的原像也是凸的：若 $C\subseteq\mathbb{R}^n$ 凸，则

```math
P^{-1}(C) = \{(x,t)\in\mathbb{R}^{n+1} \mid x/t\in C,\; t>0\}
```

凸。为证明，设 $(x,t)\in P^{-1}(C)$，$(y,s)\in P^{-1}(C)$，$0\le\theta\le1$，须证

```math
\theta(x,t)+(1-\theta)(y,s)\in P^{-1}(C),
```

即

```math
\frac{\theta x+(1-\theta)y}{\theta t+(1-\theta)s}\in C
```

<!-- p.55 -->

（$\theta t+(1-\theta)s>0$ 显然）。这由

```math
\frac{\theta x+(1-\theta)y}{\theta t+(1-\theta)s} = \mu(x/t) + (1-\mu)(y/s)
```

得到，其中

```math
\mu = \frac{\theta t}{\theta t+(1-\theta)s}\in[0,1].
```

**线性分式函数（Linear-fractional functions）。** 线性分式函数由透视函数与仿射函数复合而成。设 $g:\mathbb{R}^n\to\mathbb{R}^{m+1}$ 是仿射的，即

```math
g(x) = \begin{bmatrix}A\\ c^T\end{bmatrix}x + \begin{bmatrix}b\\ d\end{bmatrix}, \tag{2.12}
```

其中 $A\in\mathbb{R}^{m\times n}$，$b\in\mathbb{R}^m$，$c\in\mathbb{R}^n$，$d\in\mathbb{R}$。函数 $f:\mathbb{R}^n\to\mathbb{R}^m$ 由 $f=P\circ g$ 给出，即

```math
f(x) = (Ax+b)/(c^Tx+d),\qquad \mathbf{dom}\,f = \{x \mid c^Tx+d>0\}, \tag{2.13}
```

称为 linear-fractional（线性分式）函数或 projective（射影）函数。当 $c=0$ 且 $d>0$ 时，$f$ 的定义域是 $\mathbb{R}^n$，$f$ 是仿射函数。因此可以把仿射函数与线性函数看作线性分式函数的特例。

<span style="color:#00838f;">**[注 2.2]** **射影解释。** 用矩阵

```math
Q = \begin{bmatrix}A&b\\ c^T&d\end{bmatrix}\in\mathbb{R}^{(m+1)\times(n+1)} \tag{2.14}
```

来表示线性分式函数常常很方便：它作用于（乘）形如 $(x,1)$ 的点，得到 $(Ax+b,\; c^Tx+d)$，

<!-- p.55/56 -->

其结果再被缩放/归一化使最后一个分量为 1，即得 $(f(x),1)$。

这个表示可以几何化地解释：把 $\mathbb{R}^n$ 与 $\mathbb{R}^{n+1}$ 中的一族 rays（射线）联系起来。对每个点 $z\in\mathbb{R}^n$，关联 $\mathbb{R}^{n+1}$ 中的（开）射线 $\mathcal{P}(z)=\{t(z,1)\mid t>0\}$，其最后一个分量取正值。反之，$\mathbb{R}^{n+1}$ 中任何基在原点、最后分量取正值的射线都可写成 $\mathcal{P}(v)=\{t(v,1)\mid t\ge0\}$（某个 $v\in\mathbb{R}^n$）。这个 $\mathbb{R}^n$ 与「最后分量为正的射线构成的半空间」之间的（射影）对应 $\mathcal{P}$ 是一一且到上的。

线性分式函数 (2.13) 可以表示为

```math
f(x) = \mathcal{P}^{-1}(Q\mathcal{P}(x)).
```

即：从 $x\in\mathbf{dom}\,f$（即 $c^Tx+d>0$）出发，构造 $\mathbb{R}^{n+1}$ 中的射线 $\mathcal{P}(x)$；矩阵为 $Q$ 的线性变换作用在这条射线上产生另一条射线 $Q\mathcal{P}(x)$（由于 $x\in\mathbf{dom}\,f$，该射线的最后分量取正值）；最后用逆射影变换恢复出 $f(x)$。

<!-- p.56 -->

与透视函数一样，线性分式函数保持凸性。若 $C$ 凸且位于 $f$ 的定义域内（即对 $x\in C$ 有 $c^Tx+d>0$），则其像 $f(C)$ 凸。这可由上面的结果立即得出：$C$ 在仿射映射 (2.12) 下的像凸，而所得集合在透视函数 $P$ 下的像（即 $f(C)$）也凸。同样，若 $C\subseteq\mathbb{R}^m$ 凸，则原像 $f^{-1}(C)$ 凸。

<span style="color:#00838f;">**[例 2.13]** **条件概率（conditional probabilities）。** 设 $u$ 与 $v$ 是分别取值于 $\{1,\dots,n\}$ 与 $\{1,\dots,m\}$ 的随机变量，$p_{ij}$ 表示 $\mathrm{prob}(u=i, v=j)$。则 conditional probability（条件概率）$f_{ij}=\mathrm{prob}(u=i\mid v=j)$ 由

```math
f_{ij} = \frac{p_{ij}}{\sum_{k=1}^n p_{kj}}
```

给出，即 $f$ 是由 $p$ 经一个线性分式映射得到的。由此：若 $C$ 是 $(u,v)$ 的 joint probabilities（联合概率）的一个凸集，则相应的「给定 $v$ 下 $u$ 的条件概率」的集合也凸。

图 2.16 展示了一个集合 $C\subseteq\mathbb{R}^2$ 及其在线性分式函数

```math
f(x) = \frac{1}{x_1+x_2+1}\,x,\qquad \mathbf{dom}\,f=\{(x_1,x_2)\mid x_1+x_2+1>0\}
```

下的像。

> <span style="color:#2471a3;">**[译者注｜跨课程连接]** 例 2.13 与你大二概率论（6.431/Bertsekas）直接衔接：贝叶斯后验 $P(u\mid v)$ 对似然 $p$ 是线性分式映射——「联合分布的凸集 → 后验分布的凸集」。这正是贝叶斯滤波、EM 等算法中「凸性得以保持」的底层机制。</span>

## <span style="color:#c0392b;">2.4 Generalized inequalities（广义不等式）</span>

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">2.4.1 Proper cones and generalized inequalities（真锥与广义不等式）</span>

<!-- p.57 -->

若锥 $K\subseteq\mathbb{R}^n$ 满足以下条件，则称为 proper cone（真锥）：

- $K$ 是 convex（凸）的。
- $K$ 是 closed（闭）的。
- $K$ 是 solid（实心的），即它有非空的 interior（内部）。
- $K$ 是 pointed（尖）的，即它不包含任何直线（等价地，$x\in K,\;-x\in K \implies x=0$）。

真锥 $K$ 可以用来定义一个 generalized inequality（广义不等式）——$\mathbb{R}^n$ 上一个具有 $\mathbb{R}$ 上标准序的许多性质的 partial ordering（偏序）。我们把真锥 $K$ 与 $\mathbb{R}^n$ 上的偏序

```math
x\preceq_K y \iff y-x\in K
```

联系起来。同样写 $x\succeq_K y$ 表示 $y\preceq_K x$。类似地定义相应的 strict partial ordering（严格偏序）：

```math
x\prec_K y \iff y-x\in\mathbf{int}\,K,
```

并写 $x\succ_K y$ 表示 $y\prec_K x$。（为了区分广义不等式 $\preceq_K$ 与严格广义不等式，有时把 $\preceq_K$ 称为 nonstrict generalized inequality，非严格广义不等式。）

当 $K=\mathbb{R}_+$ 时，偏序 $\preceq_K$ 就是 $\mathbb{R}$ 上通常的序 $\le$，严格偏序 $\prec_K$ 就是通常的严格序 $<$。所以广义不等式把 $\mathbb{R}$ 中普通（非严格与严格）不等式作为特例包含进来。

<span style="color:#00838f;">**[例 2.14]** **非负卦限与逐分量不等式。** 非负卦限 $K=\mathbb{R}^n_+$ 是真锥。相应的广义不等式 $\preceq_K$ 对应向量间的逐分量不等式：$x\preceq_K y$ 意味着 $x_i\le y_i$（$i=1,\dots,n$）。相应的严格不等式对应逐分量严格不等式：$x\prec_K y$ 意味着 $x_i\lt y_i$（$i=1,\dots,n$）。与 $\mathbb{R}^n_+$ 相联系的非严格与严格偏序出现得如此频繁，以至于我们把下标 $\mathbb{R}^n_+$ 省略：当符号 $\preceq$ 或 $\prec$ 出现在向量之间时，默认就是这种含义。

<span style="color:#00838f;">**[例 2.15]** **半正定锥与矩阵不等式。** 半正定锥 $\mathbf{S}^n_+$ 是 $\mathbf{S}^n$ 中的真锥。相应的广义不等式 $\preceq_K$ 就是通常的 matrix inequality（矩阵不等式）：$X\preceq_K Y$ 意味着 $Y-X$ 半正定。$\mathbf{S}^n_+$ 的内部（在 $\mathbf{S}^n$ 中）由正定矩阵组成，所以严格广义不等式也与对称矩阵间通常的严格不等式一致：$X\prec_K Y$ 意味着 $Y-X$ 正定。这里偏序同样出现得太频繁以致省略下标：对对称矩阵我们直接写 $X\preceq Y$ 或 $X\prec Y$，默认广义不等式是关于半正定锥的。

<span style="color:#00838f;">**[例 2.16]** **在 $[0,1]$ 上非负的多项式之锥。** 令 $K$ 定义为

```math
K = \{c\in\mathbb{R}^n \mid c_1+c_2t+\cdots+c_nt^{n-1}\ge0 \text{ 对 } t\in[0,1]\}, \tag{2.15}
```

即 $K$ 是在区间 $[0,1]$ 上非负的 $n-1$ 次多项式（的系数）构成的锥。可以证明 $K$ 是真锥；其内部是「在 $[0,1]$ 上取正的多项式」的系数集合。两个向量 $c,d\in\mathbb{R}^n$ 满足 $c\preceq_K d$ 当且仅当

```math
c_1+c_2t+\cdots+c_nt^{n-1}\le d_1+d_2t+\cdots+d_nt^{n-1}
```

对一切 $t\in[0,1]$ 成立。

**广义不等式的性质（Properties of generalized inequalities）。** 广义不等式 $\preceq_K$ 满足许多性质，例如：

- $\preceq_K$ 对加法保持：若 $x\preceq_K y$ 且 $u\preceq_K v$，则 $x+u\preceq_K y+v$。
- $\preceq_K$ 是 transitive（传递）的：若 $x\preceq_K y$ 且 $y\preceq_K z$，则 $x\preceq_K z$。
- $\preceq_K$ 对非负缩放保持：若 $x\preceq_K y$ 且 $\alpha\ge0$，则 $\alpha x\preceq_K \alpha y$。
- $\preceq_K$ 是 reflexive（自反）的：$x\preceq_K x$。
- $\preceq_K$ 是 antisymmetric（反对称）的：若 $x\preceq_K y$ 且 $y\preceq_K x$，则 $x=y$。
- $\preceq_K$ 对极限保持：若 $x_i\preceq_K y_i$（$i=1,2,\dots$），且 $x_i\to x$、$y_i\to y$（$i\to\infty$），则 $x\preceq_K y$。

<!-- p.58 -->

相应的严格广义不等式 $\prec_K$ 满足（例如）：

- 若 $x\prec_K y$，则 $x\preceq_K y$。
- 若 $x\prec_K y$ 且 $u\preceq_K v$，则 $x+u\prec_K y+v$。
- 若 $x\prec_K y$ 且 $\alpha>0$，则 $\alpha x\prec_K \alpha y$。
- $x\nprec_K x$。
- 若 $x\prec_K y$，则对足够小的 $u$ 与 $v$，有 $x+u\prec_K y+v$。

这些性质由 $\preceq_K$、$\prec_K$ 的定义与真锥的性质继承而来；见习题 2.30。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">2.4.2 Minimum and minimal elements（最小元与极小元）</span>

<!-- p.59 -->

广义不等式的记号（$\preceq_K$、$\prec_K$）意在暗示与 $\mathbb{R}$ 上普通不等式（$\le$、$<$）的类比。虽然普通不等式的许多性质对广义不等式成立，但有些重要的并不成立。最明显的差异是：$\mathbb{R}$ 上的 $\le$ 是 linear ordering（线性序/全序）——任何两点都可比较，即要么 $x\le y$ 要么 $y\le x$。这个性质对其他广义不等式不成立。其后果之一是：在广义不等式的语境下，minimum（最小）与 maximum（最大）之类的概念变得更复杂。本节简要讨论这一点。

我们说 $x\in S$ 是 $S$ 的 minimum element（最小元）（关于广义不等式 $\preceq_K$），如果对每个 $y\in S$ 都有 $x\preceq_K y$。类似地定义 maximum element（最大元）。若一个集合有最小（最大）元，则它是唯一的。一个相关概念是 minimal element（极小元）。我们说 $x\in S$ 是 $S$ 的极小元（关于 $\preceq_K$），如果 $y\in S$、$y\preceq_K x$ 仅当 $y=x$。类似地定义 maximal element（极大元）。一个集合可以有许多不同的极小（极大）元。

我们可以用简单的集合记号描述最小元与极小元。点 $x\in S$ 是 $S$ 的最小元当且仅当

```math
S\subseteq x+K.
```

这里 $x+K$ 表示所有与 $x$ 可比较且（按 $\preceq_K$）大于等于 $x$ 的点。点 $x\in S$ 是极小元当且仅当

```math
(x-K)\cap S = \{x\}.
```

这里 $x-K$ 表示所有与 $x$ 可比较且（按 $\preceq_K$）小于等于 $x$ 的点；它与 $S$ 的公共点只有 $x$。

对 $K=\mathbb{R}_+$（它诱导 $\mathbb{R}$ 上通常的序）来说，minimal 与 minimum 的概念相同，与集合最小元的通常定义一致。

<span style="color:#00838f;">**[例 2.17]** 考虑锥 $\mathbb{R}^2_+$，它在 $\mathbb{R}^2$ 中诱导逐分量不等式。这里可以给出极小元与最小元的简单几何描述：不等式 $x\preceq y$ 意味着 $y$ 在 $x$ 的「右上方」。说 $x\in S$ 是集合 $S$ 的最小元，意味着 $S$ 的其他所有点都在 $x$ 的右上方；说 $x$ 是 $S$ 的极小元，意味着没有其他点位于 $x$ 的「左下方」。见图 2.17。

<!-- p.59/60 -->

<span style="color:#00838f;">**[例 2.18]** **对称矩阵集合的最小元与极小元。** 给每个 $A\in\mathbf{S}^n_{++}$ 关联一个以原点为中心的椭球

```math
\mathcal{E}_A = \{x \mid x^TA^{-1}x\le1\}.
```

我们有 $A\preceq B$ 当且仅当 $\mathcal{E}_A\subseteq\mathcal{E}_B$。设 $v_1,\dots,v_k\in\mathbb{R}^n$ 给定，定义

```math
S = \{P\in\mathbf{S}^n_{++} \mid v_i^TP^{-1}v_i\le1,\; i=1,\dots,k\},
```

<!-- p.60 -->

它对应「包含点 $v_1,\dots,v_k$ 的椭球」的集合。集合 $S$ 没有最小元：对任何包含这些点的椭球，都能找到另一个包含这些点、且与它不可比较的椭球。一个椭球是极小的，如果它包含这些点，但没有更小的椭球也包含它们。图 2.18 展示了 $\mathbb{R}^2$ 中 $k=2$ 的例子。
