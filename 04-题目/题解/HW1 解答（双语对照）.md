# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

> <span style="color:#7f8c8d;">Source: 作业解答/HW1 解答.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

## <span style="color:#2471a3;">**[lecture]**</span> <span style="color:#c0392b">EE364a Homework 1 Solutions（EE364a 作业 1 解答）</span>

> <span style="color:#7f8c8d;">EE364a, Winter 2007-08 · Prof. S. Boyd（斯坦福 EE364a，2007–08 冬季学期 · S. Boyd 教授）</span>

---

<!-- p.1 -->

### <span style="color:#2471a3;">**[question]**</span> <span style="color:#c0392b">Exercise 2.1（习题 2.1）</span>

**2.1** Let $C \subseteq \mathbb{R}^n$ be a convex set, with $x_1, \ldots, x_k \in C$, and let $\theta_1, \ldots, \theta_k \in \mathbb{R}$ satisfy $\theta_i \ge 0$, $\theta_1 + \cdots + \theta_k = 1$. Show that $\theta_1 x_1 + \cdots + \theta_k x_k \in C$. (The definition of convexity is that this holds for $k = 2$; you must show it for arbitrary $k$.) *Hint.* Use induction on $k$.

> <span style="color:#2471a3;">**[solution] Solution（解答）:**</span> 从 convex set（凸集）的定义出发做 induction（归纳法）即可轻松证明。我们以 $k = 3$ 为例说明这个想法，一般情形留给读者。设 $x_1, x_2, x_3 \in C$，且 $\theta_1 + \theta_2 + \theta_3 = 1$、$\theta_1, \theta_2, \theta_3 \ge 0$。我们将证明 $y = \theta_1 x_1 + \theta_2 x_2 + \theta_3 x_3 \in C$。至少有一个 $\theta_i$ 不等于 1；不失一般性（without loss of generality）可设 $\theta_1 \ne 1$。于是可以把 $y$ 写成
>
> ```math
> y = \theta_1 x_1 + (1 - \theta_1)(\mu_2 x_2 + \mu_3 x_3),
> ```
>
> 其中 $\mu_2 = \theta_2/(1-\theta_1)$、$\mu_3 = \theta_3/(1-\theta_1)$。注意 $\mu_2, \mu_3 \ge 0$，且
>
> ```math
> \mu_2 + \mu_3 = \frac{\theta_2 + \theta_3}{1 - \theta_1} = \frac{1 - \theta_1}{1 - \theta_1} = 1.
> ```
>
> 由于 $C$ 是凸集且 $x_2, x_3 \in C$，我们得出 $\mu_2 x_2 + \mu_3 x_3 \in C$。由于该点与 $x_1$ 都在 $C$ 中，故 $y \in C$。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这道题揭示了一个容易被忽略的设计细节：convexity（凸性）的定义只对 $k=2$ 的 convex combination（凸组合）提出要求，却自动蕴含任意 $k$ 的版本。证明的关键一步是把三点组合重排成"组合的组合"：先对 $x_2, x_3$ 做一次权重为 $(\mu_2, \mu_3)$ 的凸组合，再把结果与 $x_1$ 组合——induction（归纳法）正是把 $k=2$ 这颗种子推广到任意 $k$ 的标准机器，你在 6.042J 里已经反复用过它。另一个值得记住的手法是权重重整化（renormalization）：把 $\theta_2, \theta_3$ 同除以 $1-\theta_1$，使剩余权重重新归一，相当于"把剩下的点打包成一个新的凸组合问题"，这是凸分析中反复出现的套路。到概率论（Bertsekas）里它会以几乎相同的面貌再现：条件期望正是在给定事件内部把概率分布重新归一化。定义取最小、性质靠推导，这种"公理极简主义"贯穿整个凸优化课程。

### <span style="color:#2471a3;">**[question]**</span> <span style="color:#c0392b">Exercise 2.2（习题 2.2）</span>

**2.2** Show that a set is convex if and only if its intersection with any line is convex. Show that a set is affine if and only if its intersection with any line is affine.

> <span style="color:#2471a3;">**[solution] Solution（解答）:**</span> 我们证明第一部分。两个凸集的交集是凸集。因此，若 $S$ 是凸集，则 $S$ 与任意 line（直线）的交集是凸的。
>
> 反之，设 $S$ 与任意直线的交集都是凸集。任取 $S$ 中两个不同的点 $x_1$ 与 $x_2$。$S$ 与过 $x_1, x_2$ 的直线的交集是凸的，因此 $x_1$ 与 $x_2$ 的凸组合都属于该交集，从而也属于 $S$。

### <span style="color:#2471a3;">**[question]**</span> <span style="color:#c0392b">Exercise 2.5（习题 2.5）</span>

**2.5** What is the distance between two parallel hyperplanes $\{x \in \mathbb{R}^n \mid a^T x = b_1\}$ and $\{x \in \mathbb{R}^n \mid a^T x = b_2\}$?

> <span style="color:#2471a3;">**[solution] Solution（解答）:**</span> 两个 hyperplane（超平面）之间的 distance（距离）为 $|b_1 - b_2| / \|a\|_2$。为看清这一点，考虑下图中的构造（图示：两条平行超平面 $a^T x = b_1$ 与 $a^T x = b_2$，以及过原点、沿 normal vector（法向量）$a$ 方向的直线，直线与两超平面分别交于点 $x_1 = (b_1/\|a\|_2^2)\,a$ 与 $x_2 = (b_2/\|a\|_2^2)\,a$）。
>
> <!-- p.2 -->
>
> 两超平面之间的距离也等于点 $x_1$ 与 $x_2$ 之间的距离，其中 $x_1, x_2$ 是两超平面与过原点且平行于法向量 $a$ 的直线的交点。这两点由下式给出：
>
> ```math
> x_1 = (b_1 / \|a\|_2^2)\, a, \qquad x_2 = (b_2 / \|a\|_2^2)\, a,
> ```
>
> 于是距离为
>
> ```math
> \|x_1 - x_2\|_2 = |b_1 - b_2| / \|a\|_2.
> ```

### <span style="color:#2471a3;">**[question]**</span> <span style="color:#c0392b">Exercise 2.7 Voronoi description of halfspace（习题 2.7：半空间的 Voronoi 描述）</span>

**2.7** *Voronoi description of halfspace.* Let $a$ and $b$ be distinct points in $\mathbb{R}^n$. Show that the set of all points that are closer (in Euclidean norm) to $a$ than $b$, i.e., $\{x \mid \|x - a\|_2 \le \|x - b\|_2\}$, is a halfspace. Describe it explicitly as an inequality of the form $c^T x \le d$. Draw a picture.

> <span style="color:#2471a3;">**[solution] Solution（解答）:**</span> 由于 norm（范数）总是非负的，$\|x - a\|_2 \le \|x - b\|_2$ 当且仅当 $\|x - a\|_2^2 \le \|x - b\|_2^2$，于是
>
> ```math
> \begin{aligned}
> \|x - a\|_2^2 \le \|x - b\|_2^2
> &\iff (x - a)^T (x - a) \le (x - b)^T (x - b) \\
> &\iff x^T x - 2 a^T x + a^T a \le x^T x - 2 b^T x + b^T b \\
> &\iff 2 (b - a)^T x \le b^T b - a^T a.
> \end{aligned}
> ```
>
> 因此该集合确实是一个 halfspace（半空间）。我们可以取 $c = 2(b - a)$、$d = b^T b - a^T a$。这在几何上完全合理：与 $a$ 和 $b$ 等距的点构成一个超平面，其法方向正是 $b - a$ 的方向。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这个结果背后藏着一个漂亮的第一性原理事实：比较两个平方距离时，$x^T x$ 项恰好抵消，剩下 $2(b-a)^T x \le b^T b - a^T a$——一个关于 $x$ 的 affine（仿射）不等式。"点到两定点谁更近"这个看似二次的问题，居然严格线性，根源就是二次项相消。你在 Strang 线代的最小二乘一章见过同款机制：正规方程 $A^T A \hat{x} = A^T b$ 之所以是线性方程组，也是因为把距离平方 $\|A x - b\|^2$ 展开后，对 $x$ 的二阶信息凝聚成常数矩阵 $A^T A$、一阶信息变成线性项。几何上，$\{x \mid \|x-a\|_2 = \|x-b\|_2\}$ 就是线段 $ab$ 的垂直平分超平面，法向量自然沿 $b-a$；不等式一侧给出"靠近 $a$ 的那半边"。这种成对比较所诱导的半空间结构，后续会以最近邻分类、聚类分配等形态反复出现——本质都是用仿射不等式切割空间。

### <span style="color:#2471a3;">**[question]**</span> <span style="color:#c0392b">Exercise 2.8（习题 2.8）</span>

**2.8** Which of the following sets $S$ are polyhedra? If possible, express $S$ in the form $S = \{x \mid Ax \preceq b, Fx = g\}$.

(a) $S = \{y_1 a_1 + y_2 a_2 \mid -1 \le y_1 \le 1, \ -1 \le y_2 \le 1\}$, where $a_1, a_2 \in \mathbb{R}^n$.

(b) $S = \{x \in \mathbb{R}^n \mid x \succeq 0, \ \mathbf{1}^T x = 1, \ \sum_{i=1}^n x_i a_i = b_1, \ \sum_{i=1}^n x_i a_i^2 = b_2\}$, where $a_1, \ldots, a_n \in \mathbb{R}$ and $b_1, b_2 \in \mathbb{R}$.

<!-- p.3 -->

(c) $S = \{x \in \mathbb{R}^n \mid x \succeq 0, \ x^T y \le 1 \text{ for all } y \text{ with } \|y\|_2 = 1\}$.

(d) $S = \{x \in \mathbb{R}^n \mid x \succeq 0, \ x^T y \le 1 \text{ for all } y \text{ with } \sum_{i=1}^n |y_i| = 1\}$.

> <span style="color:#2471a3;">**[solution] Solution（解答）:**</span>
>
> **(a)** $S$ 是一个 polyhedron（多面体）。它是以 $a_1 + a_2$、$a_1 - a_2$、$-a_1 + a_2$、$-a_1 - a_2$ 为顶点的 parallelogram（平行四边形），下图画出 $\mathbb{R}^2$ 中的一个例子（$a_1, a_2$ 标在两条边方向上，$c_1, c_2$ 标在两条相应的外法线方向上）。
>
> 为简单起见，假设 $a_1$ 与 $a_2$ 线性无关。我们可以把 $S$ 表示为三个集合的交集：
>
> - $S_1$：由 $a_1$ 与 $a_2$ 张成的 plane（平面）
> - $S_2 = \{z + y_1 a_1 + y_2 a_2 \mid a_1^T z = a_2^T z = 0, \ -1 \le y_1 \le 1\}$。这是一块平行于 $a_2$ 且与 $S_1$ 正交的 slab（平板）
> - $S_3 = \{z + y_1 a_1 + y_2 a_2 \mid a_1^T z = a_2^T z = 0, \ -1 \le y_2 \le 1\}$。这是一块平行于 $a_1$ 且与 $S_1$ 正交的平板
>
> 每个集合都可以用线性不等式描述。
>
> - $S_1$ 可以描述为
>
> ```math
> v_k^T x = 0, \qquad k = 1, \ldots, n - 2,
> ```
>
> 其中 $v_k$ 是 $n - 2$ 个与 $a_1$ 和 $a_2$ 都正交的独立向量（它们构成矩阵 $[a_1\ a_2]^T$ 的 nullspace（零空间）的一组基）。
>
> - 设 $c_1$ 是位于 $a_1, a_2$ 张成的平面内、且与 $a_2$ 正交的向量。例如可以取
>
> ```math
> c_1 = a_1 - \frac{a_1^T a_2}{\|a_2\|_2^2}\, a_2.
> ```
>
> 那么 $x \in S_2$ 当且仅当
>
> ```math
> -|c_1^T a_1| \le c_1^T x \le |c_1^T a_1|.
> ```
>
> <!-- p.4 -->
>
> - 类似地，设 $c_2$ 是位于 $a_1, a_2$ 张成的平面内、且与 $a_1$ 正交的向量，例如
>
> ```math
> c_2 = a_2 - \frac{a_2^T a_1}{\|a_1\|_2^2}\, a_1.
> ```
>
> 那么 $x \in S_3$ 当且仅当
>
> ```math
> -|c_2^T a_2| \le c_2^T x \le |c_2^T a_2|.
> ```
>
> 综合起来，我们可以把 $S$ 描述为 $2n$ 个线性不等式的解集：
>
> ```math
> \begin{aligned}
> v_k^T x &\le 0, \qquad k = 1, \ldots, n - 2 \\
> -v_k^T x &\le 0, \qquad k = 1, \ldots, n - 2 \\
> c_1^T x &\le |c_1^T a_1| \\
> -c_1^T x &\le |c_1^T a_1| \\
> c_2^T x &\le |c_2^T a_2| \\
> -c_2^T x &\le |c_2^T a_2|.
> \end{aligned}
> ```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这里 $c_1 = a_1 - \dfrac{a_1^T a_2}{\|a_2\|_2^2}\, a_2$ 的构造你应当觉得眼熟——这正是 Strang 线代里 Gram-Schmidt orthogonalization（Gram-Schmidt 正交化）的第一步：从 $a_1$ 中减去它在 $a_2$ 方向上的 projection（投影）$\dfrac{a_1^T a_2}{\|a_2\|_2^2}\, a_2$，余下的部分便与 $a_2$ 正交。解答借它造出"位于 $a_1 a_2$ 平面内、垂直于 $a_2$"的法向量，从而把"沿 $a_1$ 方向的偏移量有界"翻译成一对线性不等式 $|c_1^T x| \le |c_1^T a_1|$。再往深看一层，整道题的策略是逐层切片：先用 $n-2$ 个约束 $v_k^T x = 0$ 把图形钉在平面 $S_1$ 上，再用两块正交的平板限定两个方向上的偏移幅度。高维空间里的"平行四边形"必须写成 $2n$ 个线性不等式之交——这正是 polyhedron（多面体）定义中"有限个半空间之交"这一要求的具体代价与具体收益。

> **(b)** $S$ 是一个多面体，由线性不等式 $x_k \ge 0$ 与三个 equality constraints（等式约束）定义。
>
> **(c)** $S$ 不是多面体。它是 unit ball（单位球）$\{x \mid \|x\|_2 \le 1\}$ 与 nonnegative orthant（非负象限）$\mathbb{R}^n_+$ 的交集。这由下面的事实（由 Cauchy-Schwarz inequality（Cauchy-Schwarz 不等式）推出）得到：
>
> ```math
> x^T y \le 1 \text{ for all } y \text{ with } \|y\|_2 = 1 \iff \|x\|_2 \le 1.
> ```
>
> 虽然本例中我们把 $S$ 定义成了半空间的交集，但它不是多面体：刻画它需要无穷多个半空间，而多面体的定义只允许有限个。
>
> **(d)** $S$ 是一个多面体。$S$ 是集合 $\{x \mid |x_k| \le 1, \ k = 1, \ldots, n\}$ 与非负象限 $\mathbb{R}^n_+$ 的交集。这由下面的事实得到：
>
> ```math
> x^T y \le 1 \text{ for all } y \text{ with } \sum_{i=1}^n |y_i| = 1 \iff |x_i| \le 1, \quad i = 1, \ldots, n.
> ```
>
> 我们可以如下证明。先假设 $|x_i| \le 1$ 对所有 $i$ 成立。那么当 $\sum_i |y_i| = 1$ 时，
>
> ```math
> x^T y = \sum_i x_i y_i \le \sum_i |x_i| |y_i| \le \sum_i |y_i| = 1.
> ```
>
> 反之，设 $x$ 是满足"$x^T y \le 1$ 对所有满足 $\sum_i |y_i| = 1$ 的 $y$ 成立"的非零向量。特别地，我们可以如下选取 $y$：取 $k$ 为使 $|x_k| = \max_i |x_i|$ 的一个下标，令 $y_k = 1$（若 $x_k > 0$）、$y_k = -1$（若 $x_k < 0$），其余 $y_i = 0$。在这一取法下
>
> ```math
> x^T y = \sum_i x_i y_i = y_k x_k = |x_k| = \max_i |x_i|.
> ```
>
> <!-- p.5 -->
>
> 因此必有 $\max_i |x_i| \le 1$。
>
> 以上说明，我们可以用有限个线性不等式来描述 $S$：它是非负象限与集合 $\{x \mid -\mathbf{1} \preceq x \preceq \mathbf{1}\}$ 的交集，即下列 $2n$ 个线性不等式
>
> ```math
> -x_i \le 0, \qquad x_i \le 1, \qquad i = 1, \ldots, n
> ```
>
> 的解集。注意，与 (c) 中一样，这里的 $S$ 也是作为无穷多个半空间的交集给出的。区别在于：此处大多数线性不等式是 redundant（冗余的），只需有限个即可刻画 $S$。
>
> 除了一些平凡情形外，以上集合都不是 affine sets（仿射集）或 subspaces（子空间）。例如，(a) 中定义的集合当 $a_1 = a_2 = 0$ 时是一个子空间（从而是仿射集）；(b) 中定义的集合当 $n = 1$ 且 $S = \{1\}$ 时是一个仿射集；等等。

### <span style="color:#2471a3;">**[question]**</span> <span style="color:#c0392b">Exercise 2.11 Hyperbolic sets（习题 2.11：双曲集合）</span>

**2.11** *Hyperbolic sets.* Show that the hyperbolic set $\{x \in \mathbb{R}^2_+ \mid x_1 x_2 \ge 1\}$ is convex. As a generalization, show that $\{x \in \mathbb{R}^n_+ \mid \prod_{i=1}^n x_i \ge 1\}$ is convex. *Hint.* If $a, b \ge 0$ and $0 \le \theta \le 1$, then $a^\theta b^{1-\theta} \le \theta a + (1 - \theta) b$; see §3.1.9.

> <span style="color:#2471a3;">**[solution] Solution（解答）:**</span>
>
> **(a)** 我们不用提示来证明第一部分。取集合中两点 $(x_1, x_2)$ 与 $(y_1, y_2)$，考虑它们的 convex combination（凸组合）$z$。若 $x \succeq y$，则 $z = \theta x + (1 - \theta) y \succeq y$，显然 $z_1 z_2 \ge y_1 y_2 \ge 1$。若 $y \succeq x$，证明类似。
>
> 设 $y \nsucceq x$ 且 $x \nsucceq y$，即 $(y_1 - x_1)(y_2 - x_2) < 0$。那么
>
> ```math
> \begin{aligned}
> (\theta x_1 + (1 - \theta) y_1)(\theta x_2 + (1 - \theta) y_2)
> &= \theta^2 x_1 x_2 + (1 - \theta)^2 y_1 y_2 + \theta (1 - \theta) x_1 y_2 + \theta (1 - \theta) x_2 y_1 \\
> &= \theta x_1 x_2 + (1 - \theta) y_1 y_2 - \theta (1 - \theta)(y_1 - x_1)(y_2 - x_2) \\
> &\ge 1.
> \end{aligned}
> ```
>
> **(b)** 设 $\prod_i x_i \ge 1$ 且 $\prod_i y_i \ge 1$。利用提示中的不等式，我们有
>
> ```math
> \prod_i \left( \theta x_i + (1 - \theta) y_i \right) \ge \prod_i x_i^{\theta} y_i^{1-\theta} = \Big( \prod_i x_i \Big)^{\theta} \Big( \prod_i y_i \Big)^{1-\theta} \ge 1.
> ```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 提示中的不等式 $a^\theta b^{1-\theta} \le \theta a + (1-\theta) b$ 就是 weighted AM-GM inequality（加权算术–几何平均不等式），它正是原书 §3.1.9 对数凹性内容的"实战预演"。它为什么成立？借助 $\ln$ 的凹性一步到位：$\theta \ln a + (1-\theta) \ln b \le \ln(\theta a + (1-\theta) b)$，两边取指数即得——本质上只是"凹函数的图像在割线下方"这一件小事。别小看它：geometric programming（几何规划，原书 §4.5）能被改写成凸优化的全部秘密都押在这个不等式上——把 posynomial（正项式）的变量换成 $x_i = e^{z_i}$ 后，目标函数取对数就变成 log-sum-exp 型的凸函数。你在 6.042J 的证明课上见过 AM-GM 的初等版本，到概率论（Bertsekas）里它将以 Jensen's inequality（Jensen 不等式）之特例的身份再次登台。三门课共用同一个凹性内核，这是数学结构跨课程复用的典型样本，值得单独记入你的"可复用武器库"。

### <span style="color:#2471a3;">**[question]**</span> <span style="color:#c0392b">Exercise 2.12（习题 2.12）</span>

**2.12** Which of the following sets are convex?

(a) A slab, i.e., a set of the form $\{x \in \mathbb{R}^n \mid \alpha \le a^T x \le \beta\}$.

(b) A rectangle, i.e., a set of the form $\{x \in \mathbb{R}^n \mid \alpha_i \le x_i \le \beta_i, \ i = 1, \ldots, n\}$. A rectangle is sometimes called a hyperrectangle when $n > 2$.

(c) A wedge, i.e., $\{x \in \mathbb{R}^n \mid a_1^T x \le b_1, \ a_2^T x \le b_2\}$.

<!-- p.6 -->

(d) The set of points closer to a given point than a given set, i.e., $\{x \mid \|x - x_0\|_2 \le \|x - y\|_2 \text{ for all } y \in S\}$, where $S \subseteq \mathbb{R}^n$.

(e) The set of points closer to one set than another, i.e., $\{x \mid \operatorname{dist}(x, S) \le \operatorname{dist}(x, T)\}$, where $S, T \subseteq \mathbb{R}^n$, and $\operatorname{dist}(x, S) = \inf\{\|x - z\|_2 \mid z \in S\}$.

(f) The set $\{x \mid x + S_2 \subseteq S_1\}$, where $S_1, S_2 \subseteq \mathbb{R}^n$ with $S_1$ convex.

(g) The set of points whose distance to $a$ does not exceed a fixed fraction $\theta$ of the distance to $b$, i.e., the set $\{x \mid \|x - a\|_2 \le \theta \|x - b\|_2\}$. You can assume $a \ne b$ and $0 \le \theta \le 1$.

> <span style="color:#2471a3;">**[solution] Solution（解答）:**</span>
>
> **(a)** 一个 slab（平板）是两个 halfspaces（半空间）的交集，因此它既是凸集，也是 polyhedron（多面体）。
>
> **(b)** 与 (a) 类似，rectangle（矩形）是有限个半空间之交，因此是凸集，也是多面体。
>
> **(c)** wedge（楔形）是两个半空间的交集，所以它是凸的，也是多面体。当 $b_1 = 0$ 且 $b_2 = 0$ 时，它是一个 cone（锥）。
>
> **(d)** 这个集合是凸的，因为它可以表示为
>
> ```math
> \bigcap_{y \in S} \{x \mid \|x - x_0\|_2 \le \|x - y\|_2\},
> ```
>
> 即半空间之交。（回顾习题 2.7：对固定的 $y$，集合 $\{x \mid \|x - x_0\|_2 \le \|x - y\|_2\}$ 是一个半空间。）
>
> **(e)** 一般而言这个集合不是凸的，正如下面 $\mathbb{R}$ 中的例子所示。取 $S = \{-1, 1\}$、$T = \{0\}$，我们有
>
> ```math
> \{x \mid \operatorname{dist}(x, S) \le \operatorname{dist}(x, T)\} = \{x \in \mathbb{R} \mid x \le -1/2 \text{ or } x \ge 1/2\},
> ```
>
> 这显然不是凸集。
>
> <!-- p.7 -->
>
> **(f)** 这个集合是凸的。$x + S_2 \subseteq S_1$ 当且仅当对所有 $y \in S_2$ 都有 $x + y \in S_1$。因此
>
> ```math
> \{x \mid x + S_2 \subseteq S_1\} = \bigcap_{y \in S_2} \{x \mid x + y \in S_1\} = \bigcap_{y \in S_2} (S_1 - y),
> ```
>
> 即诸凸集 $S_1 - y$ 的交集。
>
> **(g)** 这个集合是凸的，实际上是一个 ball（球）。
>
> ```math
> \begin{aligned}
> \{x \mid \|x - a\|_2 \le \theta \|x - b\|_2\}
> &= \{x \mid \|x - a\|_2^2 \le \theta^2 \|x - b\|_2^2\} \\
> &= \{x \mid (1 - \theta^2) x^T x - 2 (a - \theta^2 b)^T x + (a^T a - \theta^2 b^T b) \le 0\}.
> \end{aligned}
> ```
>
> 若 $\theta = 1$，这是一个半空间。若 $\theta < 1$，它是一个球 $\{x \mid (x - x_0)^T (x - x_0) \le R^2\}$，center（中心）$x_0$ 与 radius（半径）$R$ 由下式给出：
>
> ```math
> x_0 = \frac{a - \theta^2 b}{1 - \theta^2}, \qquad
> R = \left( \frac{\theta^2 \|b\|_2^2 - \|a\|_2^2}{1 - \theta^2} + \|x_0\|_2^2 \right)^{1/2}.
> ```

### <span style="color:#2471a3;">**[question]**</span> <span style="color:#c0392b">Exercise 2.15 Some sets of probability distributions（习题 2.15：概率分布的若干集合）</span>

**2.15** *Some sets of probability distributions.* Let $x$ be a real-valued random variable with $\operatorname{prob}(x = a_i) = p_i$, $i = 1, \ldots, n$, where $a_1 < a_2 < \cdots < a_n$. Of course $p \in \mathbb{R}^n$ lies in the standard probability simplex $P = \{p \mid \mathbf{1}^T p = 1, \ p \succeq 0\}$. Which of the following conditions are convex in $p$? (That is, for which of the following conditions is the set of $p \in P$ that satisfy the condition convex?)

(a) $\alpha \le \operatorname{E} f(x) \le \beta$, where $\operatorname{E} f(x)$ is the expected value of $f(x)$, i.e., $\operatorname{E} f(x) = \sum_{i=1}^n p_i f(a_i)$. (The function $f : \mathbb{R} \to \mathbb{R}$ is given.)

(b) $\operatorname{prob}(x > \alpha) \le \beta$.

(c) $\operatorname{E} |x^3| \le \alpha \operatorname{E} |x|$.

(d) $\operatorname{E} x^2 \le \alpha$.

(e) $\operatorname{E} x^2 \ge \alpha$.

(f) $\operatorname{var}(x) \le \alpha$, where $\operatorname{var}(x) = \operatorname{E} (x - \operatorname{E} x)^2$ is the variance of $x$.

(g) $\operatorname{var}(x) \ge \alpha$.

(h) $\operatorname{quartile}(x) \ge \alpha$, where $\operatorname{quartile}(x) = \inf\{\beta \mid \operatorname{prob}(x \le \beta) \ge 0.25\}$.

(i) $\operatorname{quartile}(x) \le \alpha$.

> <span style="color:#2471a3;">**[solution] Solution（解答）:**</span> 首先注意，约束 $p_i \ge 0$（$i = 1, \ldots, n$）定义了半空间，$\sum_{i=1}^n p_i = 1$ 定义了一个 hyperplane（超平面），所以 $P$ 是一个 polyhedron（多面体）。
>
> 前五个约束实际上是关于概率 $p_i$ 的 linear inequalities（线性不等式）。
>
> <!-- p.8 -->
>
> **(a)** $\operatorname{E} f(x) = \sum_{i=1}^n p_i f(a_i)$，所以该约束等价于两个线性不等式
>
> ```math
> \alpha \le \sum_{i=1}^n p_i f(a_i) \le \beta.
> ```
>
> **(b)** $\operatorname{prob}(x \ge \alpha) = \sum_{i:\, a_i \ge \alpha} p_i$，所以该约束等价于线性不等式
>
> ```math
> \sum_{i:\, a_i \ge \alpha} p_i \le \beta.
> ```
>
> **(c)** 该约束等价于线性不等式
>
> ```math
> \sum_{i=1}^n p_i \left( |a_i^3| - \alpha |a_i| \right) \le 0.
> ```
>
> **(d)** 该约束等价于线性不等式
>
> ```math
> \sum_{i=1}^n p_i a_i^2 \le \alpha.
> ```
>
> **(e)** 该约束等价于线性不等式
>
> ```math
> \sum_{i=1}^n p_i a_i^2 \ge \alpha.
> ```
>
> 因此前五个约束定义的都是凸集。
>
> **(f)** 约束
>
> ```math
> \operatorname{var}(x) = \operatorname{E} x^2 - (\operatorname{E} x)^2 = \sum_{i=1}^n p_i a_i^2 - \Big( \sum_{i=1}^n p_i a_i \Big)^2 \le \alpha
> ```
>
> 一般不是凸的。作为 counterexample（反例），可取 $n = 2$、$a_1 = 0$、$a_2 = 1$、$\alpha = 1/5$。$p = (1, 0)$ 与 $p = (0, 1)$ 是两个满足 $\operatorname{var}(x) \le \alpha$ 的点，但它们的凸组合 $p = (1/2, 1/2)$ 不满足。
>
> **(g)** 该约束等价于
>
> ```math
> \sum_{i=1}^n a_i^2 p_i + \Big( \sum_{i=1}^n a_i p_i \Big)^2 = b^T p + p^T A p \le \alpha,
> ```
>
> 其中 $b_i = a_i^2$ 且 $A = a a^T$。这定义了一个凸集，因为矩阵 $a a^T$ 是 positive semidefinite（半正定）的。
>
> 我们把 $\operatorname{quartile}(x)$ 记为 $f(p)$，以强调它是 $p$ 的函数。下页的图展示了这个定义：它画出了分布 $p$（满足 $f(p) = a_2$）的 cumulative distribution（累积分布）。

<!-- p.9 -->

> （图示：阶梯状的累积分布 $\operatorname{prob}(x \le \beta)$，在 $a_1, a_2, \ldots, a_n$ 处依次跃升 $p_1$、$p_1 + p_2$、……、$p_1 + p_2 + \cdots + p_{n-1}$，最终达到 $1$；纵轴标出水平线 $0.25$，横轴为 $\beta$。）

> **(h)** 约束 $f(p) \ge \alpha$ 等价于
>
> ```math
> \operatorname{prob}(x \le \beta) < 0.25 \quad \text{for all } \beta < \alpha.
> ```
>
> 若 $\alpha \le a_1$，这总是成立。否则，定义 $k = \max\{i \mid a_i < \alpha\}$。这是一个与 $p$ 无关的固定整数。约束 $f(p) \ge \alpha$ 成立，当且仅当
>
> ```math
> \operatorname{prob}(x \le a_k) = \sum_{i=1}^k p_i < 0.25.
> ```
>
> 这是关于 $p$ 的一个 strict linear inequality（严格线性不等式），定义了一个 open halfspace（开半空间）。
>
> **(i)** 约束 $f(p) \le \alpha$ 等价于
>
> ```math
> \operatorname{prob}(x \le \beta) \ge 0.25 \quad \text{for all } \beta \ge \alpha.
> ```
>
> 这里定义 $k = \max\{i \mid a_i \le \alpha\}$。同样，这是一个与 $p$ 无关的固定整数。约束 $f(p) \le \alpha$ 成立，当且仅当
>
> ```math
> \operatorname{prob}(x \le a_k) = \sum_{i=1}^k p_i \ge 0.25.
> ```
>
> 若 $\alpha \le a_1$，则没有 $p$ 满足 $f(p) \le \alpha$，这意味着该集合为空。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> (f) 与 (g) 的对比是本题组最精彩的一处：同样是"方差约束"，$\operatorname{var}(x) \le \alpha$ 的下水平集不凸，而 $\operatorname{var}(x) \ge \alpha$ 对应的集合却凸。机理全在代数结构里：把 $\operatorname{var}(x)$ 写成 $p$ 的函数，它是"线性项 $\sum p_i a_i^2$ 减去凸函数 $(a^T p)^2 = p^T (a a^T) p$"，线性减凸得到的是 concave（凹）函数；凹函数的上水平集（$\ge$ 型）自动是凸集，下水平集（$\le$ 型）则不然。二次型 $p^T A p$ 的凸性完全由 $A$ 的 positive semidefinite（半正定性）决定——这正是你在 Strang 线代里学过的正定性判据：特征值全部非负，等价于 $x^T A x \ge 0$ 对所有 $x$ 成立；此处 $A = a a^T$ 显然半正定，因为 $p^T a a^T p = (a^T p)^2 \ge 0$。反例里 $n=2$ 的两点分布恰好把"端点可行、中点不可行"的非凸几何暴露无遗。顺带一提，这与概率论（Bertsekas）中"方差不是期望的线性泛函、但二阶矩 $x \mapsto x^2$ 的期望由分布线性决定"这组事实互为表里——凸性分析总在提醒你区分"约束作用在分布上"还是"作用在随机变量上"。
