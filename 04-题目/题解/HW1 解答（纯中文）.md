# EE364a Homework 1 solutions（EE364a 作业 1 解答）

> 原文：`HW1 解答.pdf`（9 页），EE364a, Winter 2007-08, Prof. S. Boyd。
> 覆盖教材习题 2.1, 2.2, 2.5, 2.7, 2.8, 2.11, 2.12, 2.15，主题为 convex set（凸集）。
> 译注：原文与解答中的 `θ`、`μ` 是作者本人的记号，本译文逐字保留，不改写成 `λ`；原文两处笔误以 `†` 标注并在行内说明。

## 2.1

设 $C \subseteq \mathbf{R}^n$ 是一个 convex set（凸集），且 $x_1,\dots,x_k \in C$，又设 $\theta_1,\dots,\theta_k \in \mathbf{R}$ 满足 $\theta_i \ge 0$、$\theta_1 + \cdots + \theta_k = 1$。证明 $\theta_1 x_1 + \cdots + \theta_k x_k \in C$。(convexity 的定义只保证 $k = 2$ 时成立；需要对任意 $k$ 证明。）**Hint（提示）**：对 $k$ 用 induction（归纳法）。

**Solution（解答）**。由 convex set 的定义直接做 induction 即可。我们先对 $k = 3$ 说明思路，一般情形留给读者。设 $x_1, x_2, x_3 \in C$，且 $\theta_1 + \theta_2 + \theta_3 = 1$、$\theta_1, \theta_2, \theta_3 \ge 0$。要证

```math
y = \theta_1 x_1 + \theta_2 x_2 + \theta_3 x_3 \in C .
```

至少有一个 $\theta_i$ 不等于 $1$；不失一般性（without loss of generality）可以假设 $\theta_1 \neq 1$。于是可以把 $y$ 写成

```math
y = \theta_1 x_1 + (1 - \theta_1)(\mu_2 x_2 + \mu_3 x_3)
```

其中 $\mu_2 = \theta_2/(1 - \theta_1)$，并且 $\mu_2 = \theta_3/(1 - \theta_1)$†。注意 $\mu_2, \mu_3 \ge 0$，并且

```math
\mu_1 + \mu_2 = \frac{\theta_2 + \theta_3}{1 - \theta_1} = \frac{1 - \theta_1}{1 - \theta_1} = 1 .
```

由于 $C$ 是 convex，且 $x_2, x_3 \in C$，我们得到 $\mu_2 x_2 + \mu_3 x_3 \in C$。而这一点与 $x_1$ 都在 $C$ 中，故 $y \in C$。

† 原文此处有两处 typo（笔误），逐字保留如下，读者按语义修正：(1) 第二个等式左边应写 $\mu_3 = \theta_3/(1-\theta_1)$，原文印成 $\mu_2$；(2) 下一式左边应写 $\mu_2 + \mu_3$，原文印成 $\mu_1 + \mu_2$。修正后两式自洽：$\mu_2 + \mu_3 = (\theta_2+\theta_3)/(1-\theta_1) = 1$。这不是理解障碍，但抄写公式时必须改过来。

## 2.2

证明：一个集合是 convex 当且仅当它与任意直线的交是 convex。再证明：一个集合是 affine（仿射）当且仅当它与任意直线的交是 affine。

**Solution**。先证第一部分。两个 convex set 的交是 convex。因此若 $S$ 是 convex set，则 $S$ 与一条直线的交是 convex。

反过来，设 $S$ 与任意直线的交都是 convex。取 $S$ 中任意两个互不相同的点 $x_1$ 与 $x_2$。$S$ 与过 $x_1, x_2$ 的直线的交是 convex，因此 $x_1$ 与 $x_2$ 的 convex combination（凸组合）属于该交，从而也属于 $S$。

## 2.5

两个平行 hyperplane（超平面）$\{x \in \mathbf{R}^n \mid a^T x = b_1\}$ 与 $\{x \in \mathbf{R}^n \mid a^T x = b_2\}$ 之间的距离是多少？

**Solution**。两 hyperplane 之间的距离是 $|b_1 - b_2|/\|a\|_2$。为看清这一点，考虑下图的构造。

[Figure 1：一条数轴上的示意图——过原点的直线方向为法向量 $a$；直线 $a^T x = b_2$ 与 $a^T x = b_1$ 都与该直线垂直（图中用 $a$ 标注法向方向），两交点为 $x_2 = (b_2/\|a\|_2^2)a$ 与 $x_1 = (b_1/\|a\|_2^2)a$；两平行 hyperplane 之间的垂直距离即 $x_1$ 与 $x_2$ 的距离]

两 hyperplane 之间的距离也等于 $x_1$ 与 $x_2$ 两点之间的距离，其中 $x_1, x_2$ 是 hyperplane 与「过原点且平行于 normal vector（法向量）$a$」的直线的交点。这两点由下式给出：

```math
x_1 = (b_1/\|a\|_2^2)a, \qquad x_2 = (b_2/\|a\|_2^2)a,
```

而距离为

```math
\|x_1 - x_2\|_2 = |b_1 - b_2|/\|a\|_2 .
```

## 2.7 Voronoi description of halfspace（半空间的 Voronoi 描述）

设 $a$ 与 $b$ 是 $\mathbf{R}^n$ 中两个不同的点。证明：所有「按 Euclidean norm（欧氏范数）到 $a$ 比到 $b$ 更近」的点构成的集合，即 $\{x \mid \|x - a\|_2 \le \|x - b\|_2\}$，是一个 halfspace（半空间）。把它显式写成形如 $c^T x \le d$ 的不等式。并画图。

**Solution**。因为 norm 总是非负的，所以 $\|x - a\|_2 \le \|x - b\|_2$ 当且仅当 $\|x - a\|_2^2 \le \|x - b\|_2^2$，于是

```math
\begin{aligned}
\|x - a\|_2^2 \le \|x - b\|_2^2
&\iff (x - a)^T (x - a) \le (x - b)^T (x - b) \\
&\iff x^T x - 2a^T x + a^T a \le x^T x - 2b^T x + b^T b \\
&\iff 2(b - a)^T x \le b^T b - a^T a .
\end{aligned}
```

因此该集合确实是 halfspace。可以取 $c = 2(b - a)$，$d = b^T b - a^T a$。这在几何上很合理：与 $a$ 和 $b$ 等距的点构成一个 hyperplane，其法向沿 $b - a$ 方向。

## 2.8

下列集合 $S$ 中哪些是 polyhedron（多面体）？若可能，把 $S$ 写成 $S = \{x \mid Ax \preceq b,\ Fx = g\}$ 的形式。

**(a)** $S = \{y_1 a_1 + y_2 a_2 \mid -1 \le y_1 \le 1,\ -1 \le y_2 \le 1\}$，其中 $a_1, a_2 \in \mathbf{R}^n$。

**(b)** $S = \{x \in \mathbf{R}^n \mid x \succeq 0,\ \mathbf{1}^T x = 1,\ \sum_{i=1}^n x_i a_i = b_1,\ \sum_{i=1}^n x_i a_i^2 = b_2\}$，其中 $a_1,\dots,a_n \in \mathbf{R}$ 且 $b_1, b_2 \in \mathbf{R}$。

**(c)** $S = \{x \in \mathbf{R}^n \mid x \succeq 0,\ x^T y \le 1 \text{ 对所有满足 } \|y\|_2 = 1 \text{ 的 } y\}$。

**(d)** $S = \{x \in \mathbf{R}^n \mid x \succeq 0,\ x^T y \le 1 \text{ 对所有满足 } \sum_{i=1}^n |y_i| = 1 \text{ 的 } y\}$。

**Solution**。

**(a)** $S$ 是 polyhedron。它是以 $a_1 + a_2$、$a_1 - a_2$、$-a_1 + a_2$、$-a_1 - a_2$ 为顶点的 parallelogram（平行四边形），下图给出 $\mathbf{R}^2$ 中的一个例子。

[Figure 2：$\mathbf{R}^2$ 中的平行四边形——两条边分别沿 $a_1$、$a_2$ 方向，顶点为 $\pm a_1 \pm a_2$；图中标出与 $a_2$ 正交的方向 $c_1$ 与与 $a_1$ 正交的方向 $c_2$，用于后文把 $S$ 切成两个 slab]

为简单起见，我们假设 $a_1$ 与 $a_2$ 线性无关。可以把 $S$ 表示成三个集合的交：

- $S_1$：由 $a_1$ 与 $a_2$ 张成的平面；
- $S_2 = \{z + y_1 a_1 + y_2 a_2 \mid a_1^T z = a_2^T z = 0,\ -1 \le y_1 \le 1\}$。这是一个平行于 $a_2$、并且与 $S_1$ 正交的 slab（板层）；
- $S_3 = \{z + y_1 a_1 + y_2 a_2 \mid a_1^T z = a_2^T z = 0,\ -1 \le y_2 \le 1\}$。这是一个平行于 $a_1$、并且与 $S_1$ 正交的 slab。

这三个集合都可以用 linear inequality（线性不等式）描述。

- $S_1$ 可写成

```math
v_k^T x = 0, \qquad k = 1,\dots,n-2,
```

其中 $v_k$ 是 $n-2$ 个相互独立的向量，都与 $a_1$ 和 $a_2$ 正交（它们构成矩阵 $[\,a_1\ a_2\,]^T$ 的 nullspace（零空间）的一组基）。

- 取 $c_1$ 为「位于 $a_1, a_2$ 张成的平面内、且与 $a_2$ 正交」的向量。例如可以取

```math
c_1 = a_1 - \frac{a_1^T a_2}{\|a_2\|_2^2} a_2 .
```

于是 $x \in S_2$ 当且仅当

```math
-|c_1^T a_1| \le c_1^T x \le |c_1^T a_1| .
```

- 同理，取 $c_2$ 为「位于 $a_1, a_2$ 张成的平面内、且与 $a_1$ 正交」的向量，例如

```math
c_2 = a_2 - \frac{a_2^T a_1}{\|a_1\|_2^2} a_1 .
```

于是 $x \in S_3$ 当且仅当

```math
-|c_2^T a_2| \le c_2^T x \le |c_2^T a_2| .
```

合起来，可以把 $S$ 描述为 $2n$ 个 linear inequality 的解集：

```math
\begin{aligned}
v_k^T x &\le 0, && k = 1,\dots,n-2, \\
-v_k^T x &\le 0, && k = 1,\dots,n-2, \\
c_1^T x &\le |c_1^T a_1|, \\
-c_1^T x &\le |c_1^T a_1|, \\
c_2^T x &\le |c_2^T a_2|, \\
-c_2^T x &\le |c_2^T a_2| .
\end{aligned}
```

**(b)** $S$ 是 polyhedron，由 linear inequality $x_k \ge 0$ 与三个等式约束定义。

**(c)** $S$ 不是 polyhedron。它是 unit ball（单位球）$\{x \mid \|x\|_2 \le 1\}$ 与非负 orthant（象限锥）$\mathbf{R}^n_+$ 的交。这由下面这个事实推出（该事实来自 Cauchy–Schwarz inequality（柯西–施瓦茨不等式））：

```math
x^T y \le 1 \text{ 对所有满足 } \|y\|_2 = 1 \text{ 的 } y
\iff \|x\|_2 \le 1 .
```

尽管在这个例子里我们把 $S$ 定义成了若干 halfspace 的交，它仍然不是 polyhedron，因为 polyhedron 的定义要求是**有限**多个 halfspace 的交。

**(d)** $S$ 是 polyhedron。$S$ 是集合 $\{x \mid |x_k| \le 1,\ k = 1,\dots,n\}$ 与非负 orthant $\mathbf{R}^n_+$ 的交。这由下面这个事实推出：

```math
x^T y \le 1 \text{ 对所有满足 } \sum_{i=1}^n |y_i| = 1 \text{ 的 } y
\iff |x_i| \le 1,\ i = 1,\dots,n .
```

证明如下。先设对所有 $i$ 都有 $|x_i| \le 1$。那么当 $\sum_i |y_i| = 1$ 时，

```math
x^T y = \sum_i x_i y_i \le \sum_i |x_i| |y_i| \le \sum_i |y_i| = 1 .
```

反过来，设 $x$ 是非零向量，且对所有满足 $\sum_i |y_i| = 1$ 的 $y$ 都有 $x^T y \le 1$。特别地，我们可以这样取 $y$：令 $k$ 是使 $|x_k| = \max_i |x_i|$ 的下标，取 $y_k = 1$（若 $x_k > 0$）、$y_k = -1$（若 $x_k < 0$），并令 $i \neq k$ 时 $y_i = 0$。对这个 $y$ 有

```math
x^T y = \sum_i x_i y_i = y_k x_k = |x_k| = \max_i |x_i| .
```

因此必须有 $\max_i |x_i| \le 1$。

以上说明我们可以用有限个 linear inequality 描述 $S$：它是非负 orthant 与集合 $\{x \mid -1 \preceq x \preceq 1\}$ 的交，也就是下面 $2n$ 个 linear inequality 的解：

```math
\begin{aligned}
-x_i &\le 0, && i = 1,\dots,n, \\
x_i &\le 1, && i = 1,\dots,n .
\end{aligned}
```

注意，与 (c) 部分一样，这里 $S$ 最初也是作为无穷多个 halfspace 的交给出的。区别在于：这里的绝大多数 linear inequality 是冗余的，只需有限个就能刻画 $S$。

这些集合中没有一个（除某些平凡情形外）是 affine set（仿射集）或 subspace（子空间）。例如 (a) 定义的集合在 $a_1 = a_2 = 0$ 时是 subspace（从而是 affine set）；(b) 定义的集合在 $n = 1$ 且 $S = \{1\}$ 时是 affine set；等等。

## 2.11 Hyperbolic sets（双曲集）

证明 hyperbolic set（双曲集）$\{x \in \mathbf{R}^2_+ \mid x_1 x_2 \ge 1\}$ 是 convex。作为推广，证明 $\{x \in \mathbf{R}^n_+ \mid \prod_{i=1}^n x_i \ge 1\}$ 是 convex。**Hint**：若 $a, b \ge 0$ 且 $0 \le \theta \le 1$，则 $a^\theta b^{1-\theta} \le \theta a + (1-\theta)b$；见 §3.1.9。

**Solution**。

**(a)** 第一部分我们不用 hint 来证。考虑集合中两点 $(x_1, x_2)$ 与 $(y_1, y_2)$ 的 convex combination $z$。若 $x \succeq y$，则 $z = \theta x + (1-\theta) y \succeq y$，从而显然 $z_1 z_2 \ge y_1 y_2 \ge 1$。$y \succeq x$ 的情形同理。

下面设 $y \not\succeq x$ 且 $x \not\succeq y$，即 $(y_1 - x_1)(y_2 - x_2) < 0$。此时

```math
\begin{aligned}
& (\theta x_1 + (1-\theta) y_1)(\theta x_2 + (1-\theta) y_2) \\
={}& \theta^2 x_1 x_2 + (1-\theta)^2 y_1 y_2 + \theta(1-\theta) x_1 y_2 + \theta(1-\theta) x_2 y_1 \\
={}& \theta x_1 x_2 + (1-\theta) y_1 y_2 - \theta(1-\theta)(y_1 - x_1)(y_2 - x_2) \\
\ge{}& 1 .
\end{aligned}
```

上式第二个等号把交叉项 $\theta(1-\theta)(x_1 y_2 + x_2 y_1)$ 改写为 $\theta(1-\theta)\big[(x_1 x_2 + y_1 y_2) - (y_1-x_1)(y_2-x_2)\big]$，于是前一行的 $\theta^2 x_1 x_2 + \theta(1-\theta) x_1 x_2 = \theta x_1 x_2$ 归并成立。最后一步用 $\theta x_1 x_2 + (1-\theta) y_1 y_2 \ge \theta \cdot 1 + (1-\theta) \cdot 1 = 1$，再加上 $-\theta(1-\theta)(y_1-x_1)(y_2-x_2) \ge 0$（因为该乘积为负）。

**(b)** 设 $\prod_i x_i \ge 1$ 且 $\prod_i y_i \ge 1$。用 hint 中的不等式，有

```math
\prod_i (\theta x_i + (1-\theta) y_i) \ge \prod_i x_i^{\theta} y_i^{1-\theta}
= \Big(\prod_i x_i\Big)^{\theta} \Big(\prod_i y_i\Big)^{1-\theta} \ge 1 .
```

这里第一个不等号对每个因子分别用 $a^\theta b^{1-\theta} \le \theta a + (1-\theta)b$（取 $a = x_i$、$b = y_i$），乘起来即得。

## 2.12

下列集合中哪些是 convex？

**(a)** 一个 slab（板层），即形如 $\{x \in \mathbf{R}^n \mid \alpha \le a^T x \le \beta\}$ 的集合。

**(b)** 一个 rectangle（矩形），即形如 $\{x \in \mathbf{R}^n \mid \alpha_i \le x_i \le \beta_i,\ i = 1,\dots,n\}$ 的集合。当 $n > 2$ 时 rectangle 有时称为 hyperrectangle（超矩形）。

**(c)** 一个 wedge（楔形），即 $\{x \in \mathbf{R}^n \mid a_1^T x \le b_1,\ a_2^T x \le b_2\}$。

**(d)** 「到给定点比到给定集合更近」的点集，即

```math
\{x \mid \|x - x_0\|_2 \le \|x - y\|_2 \text{ 对所有 } y \in S\},
```

其中 $S \subseteq \mathbf{R}^n$。

**(e)** 「到一个集合比到另一个集合更近」的点集，即

```math
\{x \mid \operatorname{dist}(x, S) \le \operatorname{dist}(x, T)\},
```

其中 $S, T \subseteq \mathbf{R}^n$，且

```math
\operatorname{dist}(x, S) = \inf\{\|x - z\|_2 \mid z \in S\} .
```

**(f)** 集合 $\{x \mid x + S_2 \subseteq S_1\}$，其中 $S_1, S_2 \subseteq \mathbf{R}^n$ 且 $S_1$ 是 convex。

**(g)** 「到 $a$ 的距离不超过到 $b$ 距离的固定比例 $\theta$」的点集，即 $\{x \mid \|x - a\|_2 \le \theta \|x - b\|_2\}$。可以假设 $a \neq b$ 且 $0 \le \theta \le 1$。

**Solution**。

**(a)** slab 是两个 halfspace 的交，因此是 convex set，也是 polyhedron。

**(b)** 与 (a) 同理，rectangle 是有限个 halfspace 的交，因此是 convex set，也是 polyhedron。

**(c)** wedge 是两个 halfspace 的交，因此是 convex，也是 polyhedron。当 $b_1 = 0$ 且 $b_2 = 0$ 时它还是 cone（锥）。

**(d)** 该集合是 convex，因为它可以写成

```math
\bigcap_{y \in S} \{x \mid \|x - x_0\|_2 \le \|x - y\|_2\},
```

即若干 halfspace 的交。（回忆习题 2.7：对固定的 $y$，集合 $\{x \mid \|x - x_0\|_2 \le \|x - y\|_2\}$ 是一个 halfspace。）

**(e)** 一般情形下该集合不是 convex，下面 $\mathbf{R}$ 中的例子即可说明。取 $S = \{-1, 1\}$、$T = \{0\}$，则

```math
\{x \mid \operatorname{dist}(x, S) \le \operatorname{dist}(x, T)\}
= \{x \in \mathbf{R} \mid x \le -1/2 \ \text{或}\ x \ge 1/2\},
```

它显然不是 convex。

**(f)** 该集合是 convex。$x + S_2 \subseteq S_1$ 等价于对所有 $y \in S_2$ 有 $x + y \in S_1$。因此

```math
\{x \mid x + S_2 \subseteq S_1\} = \bigcap_{y \in S_2} \{x \mid x + y \in S_1\} = \bigcap_{y \in S_2} (S_1 - y),
```

即 convex set $S_1 - y$ 的交。

**(g)** 该集合是 convex 的，实际上是一个 ball（球）。

```math
\begin{aligned}
& \{x \mid \|x - a\|_2 \le \theta \|x - b\|_2\} \\
={}& \{x \mid \|x - a\|_2^2 \le \theta^2 \|x - b\|_2^2\} \\
={}& \{x \mid (1 - \theta^2) x^T x - 2(a - \theta^2 b)^T x + (a^T a - \theta^2 b^T b) \le 0\} .
\end{aligned}
```

若 $\theta = 1$，这是一个 halfspace。若 $\theta < 1$，它是一个 ball

```math
\{x \mid (x - x_0)^T (x - x_0) \le R^2\},
```

其中心 $x_0$ 与半径 $R$ 由下式给出：

```math
x_0 = \frac{a - \theta^2 b}{1 - \theta^2}, \qquad
R = \left( \frac{\theta^2 \|b\|_2^2 - \|a\|_2^2}{1 - \theta^2} + \|x_0\|_2^2 \right)^{1/2} .
```

（推导：把二次式除以 $1 - \theta^2$ 后配方，$(x - x_0)^T(x - x_0) \le \|x_0\|_2^2 - (a^Ta - \theta^2 b^Tb)/(1-\theta^2) = R^2$。）

## 2.15 Some sets of probability distributions（若干概率分布集合）

设 $x$ 是实值 random variable（随机变量），满足 $\operatorname{prob}(x = a_i) = p_i$，$i = 1,\dots,n$，其中 $a_1 < a_2 < \cdots < a_n$。当然 $p \in \mathbf{R}^n$ 落在标准 probability simplex（概率单纯形）$P = \{p \mid \mathbf{1}^T p = 1,\ p \succeq 0\}$ 中。下列条件中哪些关于 $p$ 是 convex 的？（也就是说：对下列哪个条件，「满足该条件的 $p \in P$ 构成的集合」是 convex？）

**(a)** $\alpha \le \mathbf{E} f(x) \le \beta$，其中 $\mathbf{E} f(x)$ 是 $f(x)$ 的 expectation（期望），即 $\mathbf{E} f(x) = \sum_{i=1}^n p_i f(a_i)$。（函数 $f : \mathbf{R} \to \mathbf{R}$ 给定。）

**(b)** $\operatorname{prob}(x > \alpha) \le \beta$。

**(c)** $\mathbf{E} |x^3| \le \alpha \mathbf{E} |x|$。

**(d)** $\mathbf{E} x^2 \le \alpha$。

**(e)** $\mathbf{E} x^2 \ge \alpha$。

**(f)** $\operatorname{var}(x) \le \alpha$，其中 $\operatorname{var}(x) = \mathbf{E}(x - \mathbf{E} x)^2$ 是 $x$ 的 variance（方差）。

**(g)** $\operatorname{var}(x) \ge \alpha$。

**(h)** $\operatorname{quartile}(x) \ge \alpha$，其中 $\operatorname{quartile}(x) = \inf\{\beta \mid \operatorname{prob}(x \le \beta) \ge 0.25\}$。

**(i)** $\operatorname{quartile}(x) \le \alpha$。

**Solution**。先注意：约束 $p_i \ge 0$（$i = 1,\dots,n$）定义的是 halfspace，而 $\sum_{i=1}^n p_i = 1$ 定义的是一个 hyperplane，因此 $P$ 是 polyhedron。

前五个约束实际上都是关于概率 $p_i$ 的 linear inequality。

**(a)** $\mathbf{E} f(x) = \sum_{i=1}^n p_i f(a_i)$，所以该约束等价于两个 linear inequality

```math
\alpha \le \sum_{i=1}^n p_i f(a_i) \le \beta .
```

**(b)** $\operatorname{prob}(x \ge \alpha) = \sum_{i : a_i \ge \alpha} p_i$，所以该约束等价于一个 linear inequality

```math
\sum_{i : a_i \ge \alpha} p_i \le \beta .
```

† 题面写的是 $\operatorname{prob}(x > \alpha)$，解答里写成了 $\operatorname{prob}(x \ge \alpha)$，$>$ 与 $\ge$ 不一致。当某个 $a_i$ 恰好等于 $\alpha$ 时两种写法给出不同集合；解答的写法对应严格不等号版本的闭包，做题时应以题面的 $>$ 为准。

**(c)** 该约束等价于一个 linear inequality

```math
\sum_{i=1}^n p_i (|a_i^3| - \alpha |a_i|) \le 0 .
```

**(d)** 该约束等价于一个 linear inequality

```math
\sum_{i=1}^n p_i a_i^2 \le \alpha .
```

**(e)** 该约束等价于一个 linear inequality

```math
\sum_{i=1}^n p_i a_i^2 \ge \alpha .
```

因此前五个约束定义的集合都是 convex。

**(f)** 约束

```math
\operatorname{var}(x) = \mathbf{E} x^2 - (\mathbf{E} x)^2
= \sum_{i=1}^n p_i a_i^2 - \Big(\sum_{i=1}^n p_i a_i\Big)^2 \le \alpha
```

一般情形下不是 convex 的。反例：取 $n = 2$、$a_1 = 0$、$a_2 = 1$、$\alpha = 1/5$。$p = (1, 0)$ 与 $p = (0, 1)$ 都满足 $\operatorname{var}(x) \le \alpha$，但它们的 convex combination $p = (1/2, 1/2)$ 不满足。（前者方差为 $0$，后者方差为 $1/4 > 1/5$。）

**(g)** 该约束等价于

```math
\sum_{i=1}^n a_i^2 p_i + \Big(\sum_{i=1}^n a_i p_i\Big)^2 = b^T p + p^T A p \le \alpha,
```

其中 $b_i = a_i^2$，$A = aa^T$。这定义了一个 convex set，因为矩阵 $aa^T$ 是 positive semidefinite（半正定）。

为强调 $\operatorname{quartile}(x)$ 是 $p$ 的函数，记 $\operatorname{quartile}(x) = f(p)$。下图说明了该定义，图中画出了一个 $f(p) = a_2$ 的分布 $p$ 的 cumulative distribution（累积分布）。

[Figure 3：累积分布函数阶梯图——横轴为 $\beta$，纵轴为 $\operatorname{prob}(x \le \beta)$，在 $a_1, a_2, \dots, a_n$ 处依次跳升 $p_1, p_2, \dots, p_{n-1}$，末级为 $p_1 + \cdots + p_{n-1}$；图中用水平虚线标出高度 $0.25$，并在 $a_2$ 处标出阶梯值 $p_1 + p_2$ 已越过 $0.25$，因此 $f(p) = a_2$]

**(h)** 约束 $f(p) \ge \alpha$ 等价于

```math
\operatorname{prob}(x \le \beta) < 0.25 \quad \text{对所有} \ \beta < \alpha .
```

若 $\alpha \le a_1$，该条件恒成立。否则定义 $k = \max\{i \mid a_i < \alpha\}$。这是一个固定整数，与 $p$ 无关。约束 $f(p) \ge \alpha$ 成立当且仅当

```math
\operatorname{prob}(x \le a_k) = \sum_{i=1}^k p_i < 0.25 .
```

这是关于 $p$ 的 strict linear inequality（严格线性不等式），它定义的是一个 open halfspace（开半空间）。

**(i)** 约束 $f(p) \le \alpha$ 等价于

```math
\operatorname{prob}(x \le \beta) \ge 0.25 \quad \text{对所有} \ \beta \ge \alpha .
```

这里令 $k = \max\{i \mid a_i \le \alpha\}$。这同样是固定整数，与 $p$ 无关。约束 $f(p) \le \alpha$ 成立当且仅当

```math
\operatorname{prob}(x \le a_k) = \sum_{i=1}^k p_i \ge 0.25 .
```

若 $\alpha \le a_1$，则没有 $p$ 满足 $f(p) \le \alpha$，也就是说该集合为空集。
