# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

> <span style="color:#7f8c8d;">Source: SEE复习课/review1_single.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

## <span style="color:#2471a3;">**[lecture]**</span> <span style="color:#c0392b;">EE364a Review Session 1（EE364a 复习课 1）</span>

<!-- p.1 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Administrative Info（课程行政信息）</span>

开场先给出 administrative info（课程行政信息）：office hours（答疑时间）为每周二 4–6pm、周三 4–8pm，地点在 Packard 277；review session（复习课）的定位是讲解 example problems（例题）并提供 homework hints（作业提示）；homework（作业）须在每周四 5pm 前提交；有问题可发邮件给助教组：ee364a-win0708-staff@lists.stanford.edu。

---

<!-- p.2 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Combinations and Hulls（组合与包）</span>

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">对表达式 $y = \theta_1 x_1 + \cdots + \theta_k x_k$：它是 $x_1, \ldots, x_k$ 的 linear combination（线性组合）；若 $\sum_i \theta_i = 1$，是 affine combination（仿射组合）；若 $\sum_i \theta_i = 1$ 且 $\theta_i \ge 0$，是 convex combination（凸组合）；若只要求 $\theta_i \ge 0$，是 conic combination（锥组合）。</span>

集合 $S = \{x_1, \ldots, x_k\}$ 的各类 hull（包）指由 $S$ 中元素的全部相应组合构成的集合，各有标准记号：linear hull（线性包）记作 $\operatorname{span}(S)$；affine hull（仿射包）记作 $\operatorname{aff}(S)$；convex hull（凸包）记作 $\operatorname{conv}(S)$；conic hull（锥包）记作 $\operatorname{cone}(S)$。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这四类组合其实只由两条约束区分：权重是否归一（$\sum_i \theta_i = 1$）与权重是否非负（$\theta_i \ge 0$）。你在 Strang 线性代数里最熟悉的 $\operatorname{span}$ 对应 linear hull——全部线性组合构成的 subspace（子空间），本质上就是矩阵列向量的 column space（列空间）思想。加上「权重和为 1」后得到 affine hull（仿射包），直觉是允许系数为负（可以做减法）但不许整体缩放，所以仿射集不必过原点——$\mathbb{R}^3$ 中三个不共线的点张成的平面就是它。再把系数限制为非负，得到 convex combination（凸组合），即点与点之间的加权平均，权重就像一个概率分布。下一页的单位向量例子会把包含关系 conv ⊆ aff ⊆ span 一次看全；记忆抓手是：约束越强，得到的集合越小——线性组合最自由，凸组合最保守。

---

<!-- p.3 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">A Few Simple Relations & An Example（几条简单关系与一个例子）</span>

<span style="color:#2471a3;">**[example]**</span> 例一，几条简单的包含关系：

```math
\operatorname{conv}(S) \subseteq \operatorname{aff}(S) \subseteq \operatorname{span}(S), \qquad \operatorname{conv}(S) \subseteq \operatorname{cone}(S) \subseteq \operatorname{span}(S).
```

例二，取 $S = \{(1,0,0), (0,1,0), (0,0,1)\} \subseteq \mathbb{R}^3$，问：它的 linear hull（线性包）、affine hull（仿射包）、convex hull（凸包）、conic hull（锥包）分别是什么？

- linear hull（线性包）：$\mathbb{R}^3$——三个向量线性无关，能张成整个空间。
- affine hull（仿射包）：过 $(1,0,0)$、$(0,1,0)$、$(0,0,1)$ 三点的 hyperplane（超平面），即平面 $x_1 + x_2 + x_3 = 1$。
- convex hull（凸包）：以这三点为 vertices（顶点）的 triangle（三角形）。
- conic hull（锥包）：$\mathbb{R}^3_+$（非负卦限）。

---

<!-- p.4 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Important Rules（重要规则）</span>

规则一：intersection（交集）保持集合类型。若对每个 $\alpha \in \mathcal{A}$，$S_\alpha$ 都是 subspace（子空间）/ affine set（仿射集）/ convex set（凸集）/ convex cone（凸锥）之一，则交集 $\bigcap_{\alpha \in \mathcal{A}} S_\alpha$ 仍是同类型的集合。

<span style="color:#2471a3;">**[example]**</span> 例：polyhedron（多面体）就是有限个 halfspace（半空间）与 hyperplane（超平面）的交集。

规则二：保持凸性的函数。affine（仿射）、perspective（透视）与 linear fractional（线性分式）三类函数保持凸性：若 $C$ 是凸集，$f$ 是这三类函数之一，则像 $f(C)$ 与原像 $f^{-1}(C)$ 都是凸集。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 为什么教材反复强调「任意一族（甚至无穷族）凸集的交集仍是凸集」？因为这是从简单凸集构造复杂凸集的基本手段：halfspace（半空间）是最简单的凸集，有限个半空间相交便得到 polyhedron（多面体）——线性规划（LP）的 feasible set（可行集）正是这样搭出来的，你在 6.006 里见过的网络流、最短路线性模型，其约束集都是「若干半空间的交」。注意交集封闭但并集一般不封闭：$[0,1] \cup [2,3]$ 就不是凸集，这也解释了为什么「可行域的并」通常要借助整数变量来处理。至于保凸三件套，perspective function（透视函数）$(x, t) \mapsto x/t$（$t > 0$）看似冷门，却是 Boyd 教材第 2.3 节把许多不等式统一写成凸形式的关键机关——它相当于对约束做「按尺度缩放」而不破坏凸性，后面分析 quasiconvex（拟凸）问题时还会再遇到它。

---

<!-- p.5 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Quantized Measurements（量化测量）</span>

<span style="color:#2471a3;">**[example]**</span> 考虑如下 measurement setup（测量配置）：

```math
y = 0.1\,\operatorname{floor}(10\,Ax)
```

其中 $x \in \mathbb{R}^2$ 是 input（输入），$y \in \mathbb{R}^5$ 是 measurement（测量值），$A \in \mathbb{R}^{5 \times 2}$——$\operatorname{floor}(\cdot)$ 把每个测量值量化到 0.1 的网格上。

- 给定测量值 $y$，我们想找出与这些测量一致的全部输入构成的集合，即

```math
X = \{x \mid 0 \le a_i^T x - y_i \le 0.1,\; i = 1, \ldots, 5\}.
```

我们可以用仿真来探索这个集合：随机取一个 $x \in \mathbb{R}^2$，若 $x$ 与 $y$ 一致就画下这个点，重复多次。在下图中，blue circles（蓝色圆点）表示落在 $X$ 内的点，red dot（红点）是 least-squares solution（最小二乘解）$x_{\mathrm{ls}} = A^\dagger y$，其中 $A^\dagger$ 是 pseudoinverse（伪逆）。

---

<!-- p.6 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Quantized Measurements（量化测量）——仿真图示</span>

本页为纯图示 slide：在 $x_1$–$x_2$ 平面上绘制上一页描述的随机采样结果——蓝色圆点为与测量一致的点集 $X$ 内的样本，红点为最小二乘解 $x_{\mathrm{ls}} = A^\dagger y$。

---

<!-- p.7 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Quantized Measurements（量化测量）——多面体结构</span>

从仿真结果我们怀疑 $X$ 是一个 polyhedron（多面体），即 $X = \{x \mid Fx \le g\}$。容易验证可以取：

```math
F = \begin{bmatrix} -a_1^T \\ a_1^T \\ \vdots \\ -a_5^T \\ a_5^T \end{bmatrix}, \qquad g = \begin{bmatrix} -y_1 \\ y_1 + 0.1 \\ \vdots \\ -y_5 \\ y_5 + 0.1 \end{bmatrix}.
```

（每组约束 $0 \le a_i^T x - y_i \le 0.1$ 拆成两条半空间不等式：$-a_i^T x \le -y_i$ 与 $a_i^T x \le y_i + 0.1$。）

---

<!-- p.8 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Solution Set of a Quadratic Inequality（二次不等式的解集）</span>

<span style="color:#2471a3;">**[question]**</span> Let $C \subseteq \mathbb{R}^n$ be the solution set of a quadratic inequality,

```math
C = \{x \in \mathbb{R}^n \mid x^T A x + b^T x + c \le 0\},
```

with $A \in \mathbb{S}^n$, $b \in \mathbb{R}^n$, and $c \in \mathbb{R}$. Show that $C$ is convex if $A \succeq 0$.

> <span style="color:#2471a3;">**[solution]**</span> 我们证明 $C$ 与任意一条直线 $\{\hat{x} + tv \mid t \in \mathbb{R}\}$ 的交集是凸的。把直线参数化代入二次表达式，得
>
> ```math
> (\hat{x} + tv)^T A (\hat{x} + tv) + b^T (\hat{x} + tv) + c = \alpha t^2 + \beta t + \gamma
> ```
>
> 其中
>
> ```math
> \alpha = v^T A v, \qquad \beta = b^T v + 2\,\hat{x}^T A v, \qquad \gamma = c + b^T \hat{x} + \hat{x}^T A \hat{x}.
> ```

<!-- p.9 -->

> 由 $\hat{x}$ 与 $v$ 定义的直线与 $C$ 的交集为
>
> ```math
> \{\hat{x} + tv \mid \alpha t^2 + \beta t + \gamma \le 0\},
> ```
>
> 当 $\alpha \ge 0$ 时它是凸的——此时满足不等式的 $t$ 构成 $\mathbb{R}$ 中的一个区间，而线段正是区间的仿射像。当 $A \succeq 0$ 时，这对任何 $v$ 都成立（因为 $\alpha = v^T A v \ge 0$），故 $C$ 是凸集。$\square$

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 把这个证明抽象成一个可复用的思维模板——不妨命名为「直线截口法」：一个集合是凸集，当且仅当它与任意一条直线的交是（可能为空的）区间，因为线段本来就是区间的仿射像。遇到「带二次型的集合是否凸」这类问题，几乎都可以先沿 $x = \hat{x} + tv$ 参数化，把多元问题降维成一元二次式 $\alpha t^2 + \beta t + \gamma$ 的符号讨论，而二次项系数 $\alpha = v^T A v$ 的正负正由 $A$ 的半正定性兜底。这里用到的正是 Strang 线性代数第 6 章的三重等价：实对称矩阵 $A \succeq 0$ 当且仅当所有 eigenvalue（特征值）非负，当且仅当 $v^T A v \ge 0$ 对一切 $v$ 成立——一道题把三个等价定义全串了起来。这套「对称矩阵 + 二次型」的语言也是本课程第 3 章二阶条件（Hessian 半正定 $\Leftrightarrow$ 凸函数）的雏形，值得现在就练熟。

---

<!-- p.10 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Voronoi Sets and Polyhedral Decomposition（Voronoi 集与多面体分解）</span>

<span style="color:#2471a3;">**[question]**</span> Let $x_0, \ldots, x_K \in \mathbb{R}^n$. Consider the set of points that are closer (in Euclidean norm) to $x_0$ than the other $x_i$, i.e.,

```math
V = \{x \in \mathbb{R}^n \mid \|x - x_0\|_2 \le \|x - x_i\|_2,\; i = 1, \ldots, K\}.
```

What kind of set is $V$?

> <span style="color:#2471a3;">**[solution]**</span> Answer（解答）：$V$ 是一个 polyhedron（多面体）。可以将它写成 $V = \{x \mid Ax \preceq b\}$，其中
>
> ```math
> A = 2\begin{bmatrix} (x_1 - x_0)^T \\ (x_2 - x_0)^T \\ \vdots \\ (x_K - x_0)^T \end{bmatrix}, \qquad b = \begin{bmatrix} x_1^T x_1 - x_0^T x_0 \\ x_2^T x_2 - x_0^T x_0 \\ \vdots \\ x_K^T x_K - x_0^T x_0 \end{bmatrix}.
> ```
>
> （check this!（请自行验证！）：把 $\|x - x_0\|_2^2 \le \|x - x_i\|_2^2$ 展开后，两侧的二次项 $x^T x$ 精确相消，剩下的正是上面的线性不等式。）

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这一页藏着一个漂亮的「意外消去」：$\|x - x_0\|_2^2$ 与 $\|x - x_i\|_2^2$ 展开后各含一个 $x^T x$ 项，相减时精确抵消，二次问题瞬间退化为线性不等式——这正是 Voronoi 图（计算几何中按「离哪个站点最近」划分空间得到的结构）的每个 cell（胞腔）都是多边形/多面体的根本原因。可以和你正在学的 6.006 连起来：nearest-neighbor search（最近邻搜索）搬到连续空间后，其「决策边界」就是这些 Voronoi 胞腔的拼接，相邻两站点的边界是它们连线的中垂面（二维时是中垂线）——这也是最近邻分类器的分类边界呈 piecewise-linear（分段线性）的原因：训练点给定后，特征空间被自动切成一堆凸多面体。顺带一提，「展开平方范数让二次项相消」这个技巧在核方法、距离几何里还会反复出现，值得单独记一笔。

---

<!-- p.11 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Conic Hull of Outer Products（外积的锥包）</span>

<span style="color:#2471a3;">**[question]**</span> Consider the set of rank-$k$ outer products, defined as

```math
\{\,X X^T \mid X \in \mathbb{R}^{n \times k},\; \operatorname{rank} X = k\,\}.
```

Describe its conic hull in simple terms.

> <span style="color:#2471a3;">**[solution]**</span> Solution（解答）：注意到 $XX^T \succeq 0$ 且 $\operatorname{rank}(XX^T) = k$。这类矩阵的 positive combination（正组合）秩最高可达 $n$，但绝不会低于 $k$。事实上，设 $A$、$B$ 为秩为 $k$ 的 positive semidefinite（半正定）矩阵，并设 $v \in \mathcal{N}(A + B)$（$A + B$ 的 nullspace（零空间）），则
>
> ```math
> (A + B)v = 0 \;\Longleftrightarrow\; v^T (A + B) v = 0 \;\Longleftrightarrow\; v^T A v + v^T B v = 0.
> ```
>
> 两个非负数之和为零意味着各自为零，于是
>
> ```math
> v^T A v = 0 \;\Longleftrightarrow\; Av = 0, \qquad v^T B v = 0 \;\Longleftrightarrow\; Bv = 0.
> ```
>
> 故 $\mathcal{N}(A + B)$ 中的任何向量必属于 $\mathcal{N}(A)$ 与 $\mathcal{N}(B)$。

<!-- p.12 -->

> 这说明 $\dim \mathcal{N}(A + B)$ 不能大于 $\dim \mathcal{N}(A)$ 或 $\dim \mathcal{N}(B)$，因此半正定矩阵的正组合只会 gain rank（增秩），不会降秩。由此得到结论：rank-$k$ outer product（秩 $k$ 外积）集合的 conic hull（锥包）就是 rank greater than or equal to $k$（秩大于等于 $k$）的半正定矩阵之集，再加上 zero matrix（零矩阵）。$\square$

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这道题的核心引理 $v^T A v = 0 \iff Av = 0$（$A \succeq 0$）值得收进你的「证明策略模板库」：把半正定矩阵写成平方根 $A = A^{1/2}A^{1/2}$（Strang 第 6 章：实对称半正定矩阵特征值非负、可正交对角化，故可开平方），则 $v^T A v = \|A^{1/2} v\|_2^2$，而范数为零当且仅当向量为零，二次型命题立刻降为线性代数命题。零空间维数的比较则直接调用 Strang 的公式 $\dim \mathcal{N}(A) = n - \operatorname{rank}(A)$：零空间只能变小、秩只能变大，所以正组合「只增秩不降秩」。再看结论的形状：「秩 $\ge k$ 的半正定矩阵」加上孤立的零矩阵竟是一个凸锥——两个高秩矩阵之和可能秩很低（秩在一般加法下并不稳定），但「零空间的包含关系」让这个下界在锥组合下得以保持。用 6.006 的复杂度视角看：带精确秩约束（如 rank $\le k$）的矩阵优化问题一般是 NP-hard 的，而本课程后半程的 semidefinite program（半定规划, SDP）正是把秩约束放松掉，换来多项式时间可解的凸问题。
