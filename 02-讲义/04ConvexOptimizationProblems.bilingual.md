# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

## <span style="color:#c0392b;">4. 凸优化问题 Convex Optimization Problems</span>

> <span style="color:#7f8c8d;">Source: SEE官方讲义/04ConvexOptimizationProblems.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

<!-- p.1 -->

<span style="color:#2471a3;">**[lecture]**</span>

### <span style="color:#c0392b;">第 4 讲：凸优化问题 Convex Optimization Problems</span>

本讲内容（topics）：

- 标准形式的优化问题（optimization problem in standard form）
- 凸优化问题（convex optimization problems）
- 拟凸优化（quasiconvex optimization）
- 线性优化（linear optimization）
- 二次优化（quadratic optimization）
- 几何规划（geometric programming）
- 广义不等式约束（generalized inequality constraints）
- 半定规划（semidefinite programming）
- 向量优化（vector optimization）

<!-- p.2 -->

<span style="color:#2471a3;">**[definition]**</span>

### <span style="color:#c0392b;">标准形式的优化问题 Optimization problem in standard form</span>

```math
\begin{array}{ll}
\text{minimize} & f_0(x) \\
\text{subject to} & f_i(x) \le 0, \quad i = 1, \ldots, m \\
& h_i(x) = 0, \quad i = 1, \ldots, p
\end{array}
```

- $x \in \mathbb{R}^n$ 是 optimization variable（优化变量）
- $f_0 : \mathbb{R}^n \to \mathbb{R}$ 是 objective（目标函数）或 cost function（成本函数）
- $f_i : \mathbb{R}^n \to \mathbb{R}$，$i = 1, \ldots, m$，是 inequality constraint functions（不等式约束函数）
- $h_i : \mathbb{R}^n \to \mathbb{R}$ 是 equality constraint functions（等式约束函数）

optimal value（最优值）定义为：

```math
p^\star = \inf\{ f_0(x) \mid f_i(x) \le 0,\ i = 1, \ldots, m,\ h_i(x) = 0,\ i = 1, \ldots, p \}
```

- 若问题 infeasible（不可行），即没有 $x$ 满足约束，则 $p^\star = \infty$
- 若问题 unbounded below（下无界），则 $p^\star = -\infty$

<!-- p.3 -->

<span style="color:#2471a3;">**[definition]**</span>

### <span style="color:#c0392b;">最优点与局部最优点 Optimal and locally optimal points</span>

若 $x \in \mathrm{dom}\, f_0$ 且满足所有约束，则称 $x$ 是 feasible（可行的）；一个可行点 $x$ 是 optimal（最优）的，若 $f_0(x) = p^\star$；全部最优点的集合记为 $X_{\mathrm{opt}}$。

若存在 $R > 0$ 使得 $x$ 对如下问题最优，则称 $x$ 是 locally optimal（局部最优）的：

```math
\begin{array}{ll}
\text{minimize (over } z\text{)} & f_0(z) \\
\text{subject to} & f_i(z) \le 0, \quad i = 1, \ldots, m, \\
& h_i(z) = 0, \quad i = 1, \ldots, p, \\
& \|z - x\|_2 \le R
\end{array}
```

<span style="color:#2471a3;">**[example]**</span> **例子（取 $n = 1$，$m = p = 0$）：**

- $f_0(x) = 1/x$，$\mathrm{dom}\, f_0 = \mathbb{R}_{++}$：$p^\star = 0$，但不存在 optimal point（最优点）——下确界取不到
- $f_0(x) = -\log x$，$\mathrm{dom}\, f_0 = \mathbb{R}_{++}$：$p^\star = -\infty$（下无界）
- $f_0(x) = x \log x$，$\mathrm{dom}\, f_0 = \mathbb{R}_{++}$：$p^\star = -1/e$，且 $x = 1/e$ 是最优点
- $f_0(x) = x^3 - 3x$：$p^\star = -\infty$，但 $x = 1$ 处有 local optimum（局部最优）点

<!-- p.4 -->

<span style="color:#2471a3;">**[definition]**</span>

### <span style="color:#c0392b;">隐式约束 Implicit constraints</span>

标准形式的优化问题自带一个 implicit constraint（隐式约束）：

```math
x \in D = \bigcap_{i=0}^{m} \mathrm{dom}\, f_i \ \cap\ \bigcap_{i=1}^{p} \mathrm{dom}\, h_i
```

- 我们称 $D$ 为该问题的 domain（问题定义域）
- 约束 $f_i(x) \le 0$、$h_i(x) = 0$ 则称为 explicit constraints（显式约束）
- 若问题没有显式约束（$m = p = 0$），则称之为 unconstrained（无约束）问题

<span style="color:#2471a3;">**[example]**</span> **例子：**

```math
\text{minimize} \quad f_0(x) = -\sum_{i=1}^{k} \log(b_i - a_i^T x)
```

这是一个无约束问题，但带有隐式约束 $a_i^T x < b_i$。

<!-- p.5 -->

<span style="color:#2471a3;">**[definition]**</span>

### <span style="color:#c0392b;">可行性问题 Feasibility problem</span>

```math
\begin{array}{ll}
\text{find} & x \\
\text{subject to} & f_i(x) \le 0, \quad i = 1, \ldots, m \\
& h_i(x) = 0, \quad i = 1, \ldots, p
\end{array}
```

可视为一般问题在 $f_0(x) = 0$ 时的特例：

```math
\begin{array}{ll}
\text{minimize} & 0 \\
\text{subject to} & f_i(x) \le 0, \quad i = 1, \ldots, m \\
& h_i(x) = 0, \quad i = 1, \ldots, p
\end{array}
```

- 若约束可行，则 $p^\star = 0$，且任何可行点 $x$ 都是最优的
- 若约束不可行，则 $p^\star = \infty$

<!-- p.6 -->

<span style="color:#2471a3;">**[definition]**</span>

### <span style="color:#c0392b;">凸优化问题 Convex optimization problem</span>

standard form convex optimization problem（标准形式凸优化问题）：

```math
\begin{array}{ll}
\text{minimize} & f_0(x) \\
\text{subject to} & f_i(x) \le 0, \quad i = 1, \ldots, m \\
& a_i^T x = b_i, \quad i = 1, \ldots, p
\end{array}
```

- $f_0, f_1, \ldots, f_m$ 为 convex function（凸函数）；equality constraints（等式约束）为 affine（仿射）的
- 若 $f_0$ 是 quasiconvex（拟凸）的（且 $f_1, \ldots, f_m$ 凸），则问题为 quasiconvex problem（拟凸问题）

也常写成：

```math
\begin{array}{ll}
\text{minimize} & f_0(x) \\
\text{subject to} & f_i(x) \le 0, \quad i = 1, \ldots, m \\
& Ax = b
\end{array}
```

<span style="color:#00838f;">**重要性质：凸优化问题的 feasible set（可行集）是凸集。**</span>

<!-- p.7 -->

<span style="color:#2471a3;">**[example]**</span>

### <span style="color:#c0392b;">例子 Example</span>

```math
\begin{array}{ll}
\text{minimize} & f_0(x) = x_1^2 + x_2^2 \\
\text{subject to} & f_1(x) = x_1/(1 + x_2^2) \le 0 \\
& h_1(x) = (x_1 + x_2)^2 = 0
\end{array}
```

- $f_0$ 是凸的；可行集 $\{(x_1, x_2) \mid x_1 = -x_2 \le 0\}$ 是凸集
- 但按我们的定义它**不是**凸优化问题：$f_1$ 不是凸函数，$h_1$ 不是仿射函数
- 它与下面的凸问题 equivalent（等价）但不 identical（相同）：

```math
\begin{array}{ll}
\text{minimize} & x_1^2 + x_2^2 \\
\text{subject to} & x_1 \le 0 \\
& x_1 + x_2 = 0
\end{array}
```

<!-- p.8 -->

<span style="color:#2471a3;">**[theorem]**</span>

### <span style="color:#c0392b;">局部最优即全局最优 Local and global optima</span>

<span style="color:#00838f;">**凸问题的任何局部最优点都是（全局）最优的。**</span>

<span style="color:#2471a3;">**[proof]**</span> *Proof（证明）：* 设 $x$ 局部最优，而存在最优解 $y$ 使 $f_0(y) < f_0(x)$。

$x$ 局部最优意味着存在 $R > 0$，使得

```math
z \text{ feasible},\ \|z - x\|_2 \le R \implies f_0(z) \ge f_0(x)
```

考虑 $z = \theta y + (1 - \theta)x$，取 $\theta = R/(2\|y - x\|_2)$：

- 因 $f_0(y) < f_0(x)$ 知 $\|y - x\|_2 > R$，故 $0 < \theta < 1/2$
- $z$ 是两个可行点的 convex combination（凸组合），因而也可行
- $\|z - x\|_2 = \theta\|y - x\|_2 = R/2$，且由 $f_0$ 的凸性

```math
f_0(z) \le \theta f_0(y) + (1 - \theta) f_0(x) < f_0(x)
```

这与"$x$ 局部最优"的假设矛盾。$\square$

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这条证明值得提炼成一个可复用的思维模板——**"凸组合采样法"**：要否定"局部最优但非全局最优"，就在连接局部最优解 $x$ 与更优解 $y$ 的线段上取一个离 $x$ 仅 $R/2$ 的点 $z$，然后让凸性的两条性质同时发力——可行集凸保证 $z$ 可行，目标函数凸保证 $f_0(z)$ 不超过端点值的加权平均——两下夹出矛盾。整个论证只用到定义性质，一阶信息都没碰，这正是"凸问题没有局部陷阱"的本质：任何局部最优自动全局最优。对照你在 6.006 学过的局部搜索（local search / hill climbing）：在一般图或非凸目标上，局部搜索会卡在局部极值，必须靠随机重启或模拟退火逃离；而对凸问题，最朴素的下降法也不会被局部极值欺骗——这是凸性能成为"可解问题"代名词的算法根源。另外注意构造中取 $\theta$ 时刻意"留一半余量"（落点在 $R/2$ 而非 $R$），确保 $z$ 严格落在局部最优性保证的球内——这种减半余量的手法与实分析中的 $\epsilon/2$ 技巧同一血脉。

<!-- p.9 -->

<span style="color:#2471a3;">**[theorem]**</span>

### <span style="color:#c0392b;">可微 $f_0$ 的最优性判据 Optimality criterion for differentiable $f_0$</span>

<span style="color:#00838f;">**$x$ 是最优点的充要条件是：$x$ 可行，且对一切可行点 $y$ 有**</span>

```math
\nabla f_0(x)^T (y - x) \ge 0
```

> **图示：** 若 $\nabla f_0(x) \ne 0$，则 $\nabla f_0(x)$ 在 $x$ 处定义了可行集 $X$ 的一个 supporting hyperplane（支撑超平面）；$-\nabla f_0(x)$ 指向可行域内部，意味着沿任何可行方向都再也"降不下去"。

<!-- p.10 -->

<span style="color:#2471a3;">**[example]**</span>

### <span style="color:#c0392b;">最优性判据的特例 Special cases</span>

- **无约束问题：** $x$ 最优 $\iff$

```math
x \in \mathrm{dom}\, f_0, \qquad \nabla f_0(x) = 0
```

- **等式约束问题**

```math
\begin{array}{ll}
\text{minimize} & f_0(x) \\
\text{subject to} & Ax = b
\end{array}
```

$x$ 最优 $\iff$ 存在 $\nu$ 使得

```math
x \in \mathrm{dom}\, f_0, \qquad Ax = b, \qquad \nabla f_0(x) + A^T \nu = 0
```

- **非负卦限（nonnegative orthant）上的最小化**

```math
\begin{array}{ll}
\text{minimize} & f_0(x) \\
\text{subject to} & x \succeq 0
\end{array}
```

$x$ 最优 $\iff$ $x \in \mathrm{dom}\, f_0$、$x \succeq 0$，且逐分量满足：若 $x_i = 0$，则 $(\nabla f_0(x))_i \ge 0$；若 $x_i > 0$，则 $(\nabla f_0(x))_i = 0$。

<!-- p.11 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">等价的凸问题 Equivalent convex problems</span>

若一个问题与另一个问题的解能彼此轻易互相求出，就称两个问题（非正式地）equivalent（等价）。

以下是一些保持凸性的常用变换：

**消去等式约束（eliminating equality constraints）：**

```math
\begin{array}{ll}
\text{minimize} & f_0(x) \\
\text{subject to} & f_i(x) \le 0, \quad i = 1, \ldots, m \\
& Ax = b
\end{array}
```

等价于

```math
\begin{array}{ll}
\text{minimize (over } z\text{)} & f_0(Fz + x_0) \\
\text{subject to} & f_i(Fz + x_0) \le 0, \quad i = 1, \ldots, m
\end{array}
```

其中 $F$ 与 $x_0$ 满足

```math
Ax = b \iff x = Fz + x_0 \text{ for some } z
```

<!-- p.12 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">引入等式约束与松弛变量 Introducing equality constraints and slack variables</span>

**引入等式约束（introducing equality constraints）：**

```math
\begin{array}{ll}
\text{minimize} & f_0(A_0 x + b_0) \\
\text{subject to} & f_i(A_i x + b_i) \le 0, \quad i = 1, \ldots, m
\end{array}
```

等价于

```math
\begin{array}{ll}
\text{minimize (over } x, y_i\text{)} & f_0(y_0) \\
\text{subject to} & f_i(y_i) \le 0, \quad i = 1, \ldots, m \\
& y_i = A_i x + b_i, \quad i = 0, 1, \ldots, m
\end{array}
```

**为线性不等式引入松弛变量（slack variables）：**

```math
\begin{array}{ll}
\text{minimize} & f_0(x) \\
\text{subject to} & a_i^T x \le b_i, \quad i = 1, \ldots, m
\end{array}
```

等价于

```math
\begin{array}{ll}
\text{minimize (over } x, s\text{)} & f_0(x) \\
\text{subject to} & a_i^T x + s_i = b_i, \quad i = 1, \ldots, m \\
& s_i \ge 0, \quad i = 1, \ldots, m
\end{array}
```

<!-- p.13 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">上图形式与部分最小化 Epigraph form and partial minimization</span>

**上图形式（epigraph form）：** 标准形式凸问题等价于

```math
\begin{array}{ll}
\text{minimize (over } x, t\text{)} & t \\
\text{subject to} & f_0(x) - t \le 0 \\
& f_i(x) \le 0, \quad i = 1, \ldots, m \\
& Ax = b
\end{array}
```

**对部分变量最小化（minimizing over some variables）：**

```math
\begin{array}{ll}
\text{minimize} & f_0(x_1, x_2) \\
\text{subject to} & f_i(x_1) \le 0, \quad i = 1, \ldots, m
\end{array}
```

等价于

```math
\begin{array}{ll}
\text{minimize} & \tilde{f}_0(x_1) \\
\text{subject to} & f_i(x_1) \le 0, \quad i = 1, \ldots, m
\end{array}
```

其中 $\tilde{f}_0(x_1) = \inf_{x_2} f_0(x_1, x_2)$。

<!-- p.14 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">拟凸优化 Quasiconvex optimization</span>

```math
\begin{array}{ll}
\text{minimize} & f_0(x) \\
\text{subject to} & f_i(x) \le 0, \quad i = 1, \ldots, m \\
& Ax = b
\end{array}
```

其中 $f_0 : \mathbb{R}^n \to \mathbb{R}$ 为 quasiconvex（拟凸）函数，$f_1, \ldots, f_m$ 为凸函数。

<span style="color:#00838f;">拟凸问题可能出现不是（全局）最优的局部最优点。</span>

> **图示：** 拟凸目标函数的图像呈"单峰"形状——存在局部平坦的台阶，曲线上的局部极小点未必是全局极小点。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 拟凸性只约束 sublevel set（下水平集）的形状，而不约束函数值之间的"加权平均"关系——这是它与凸性的本质区别。由此推出两条不对称的运算规则：两个拟凸函数之和一般不再拟凸，但拟凸函数的单调变换仍是拟凸的，因为单调变换不改变下水平集。🎥 Boyd 在视频中把拟凸的另一个名字直接点破：*"there's another name for quasiconvex, it's called unimodal"*（拟凸又叫单峰函数）。他还调侃过学界围绕凸性推广形成的"亚文化"：*"We have looked at all of these things, the 27 different extensions, and identified two that seemed above threshold"*——我们审视过全部 27 种凸性推广，只有两种真正越过了"有用"的门槛，拟凸性是其中之一。这句话是很好的学术判断力训练：定义层面的推广铺天盖地，但只有能兑换成算法红利（如下两页的二分法）的推广才值得花时间。对工程学习者来说，掌握判据"下水平集全凸"就足以应对绝大多数应用场景。

<!-- p.15 -->

<span style="color:#2471a3;">**[definition]**</span>

### <span style="color:#c0392b;">$f_0$ 下水平集的凸表示 Convex representation of sublevel sets of $f_0$</span>

若 $f_0$ 拟凸，则存在一族函数 $\phi_t$，满足：

- 对固定的 $t$，$\phi_t(x)$ 关于 $x$ 是凸的
- $f_0$ 的 $t$-下水平集就是 $\phi_t$ 的 0-下水平集，即

```math
f_0(x) \le t \iff \phi_t(x) \le 0
```

<span style="color:#2471a3;">**[example]**</span> **例子：**

```math
f_0(x) = \frac{p(x)}{q(x)}
```

其中 $p$ 为凸、$q$ 为凹，且在 $\mathrm{dom}\, f_0$ 上 $p(x) \ge 0$、$q(x) > 0$。可取

```math
\phi_t(x) = p(x) - t\, q(x)
```

- 对 $t \ge 0$，$\phi_t$ 关于 $x$ 是凸的（凸函数减去 $t$ 倍凹函数）
- $p(x)/q(x) \le t$ 当且仅当 $\phi_t(x) \le 0$

<!-- p.16 -->

<span style="color:#2471a3;">**[algorithm]**</span>

### <span style="color:#c0392b;">借助凸可行性问题求解拟凸优化 Quasiconvex optimization via convex feasibility problems</span>

```math
\phi_t(x) \le 0, \qquad f_i(x) \le 0, \quad i = 1, \ldots, m, \qquad Ax = b \tag{1}
```

- 对固定的 $t$，这是关于 $x$ 的 convex feasibility problem（凸可行性问题）
- 若 (1) feasible（可行），可断言 $t \ge p^\star$；若 infeasible（不可行），可断言 $t \le p^\star$

**拟凸优化的 Bisection method（二分法）：** 给定 $l \le p^\star$、$u \ge p^\star$、tolerance（容差）$\epsilon > 0$。

repeat（重复）：

1. $t := (l + u)/2$
2. 求解凸可行性问题 (1)
3. 若 (1) 可行，则 $u := t$；否则 $l := t$

until（直到）$u - l \le \epsilon$。

恰好需要 $\lceil \log_2((u - l)/\epsilon) \rceil$ 次迭代（其中 $u$、$l$ 取初始值）。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这个算法就是你在 6.006 学过的 binary search（二分搜索）的连续版本：把"在有序数组中找元素"换成"在最优值区间中定位 $p^\star$"，把"比较中点与目标"换成"解一个凸可行性问题"。连接路径完全同构——每次迭代把不确定性区间减半，故迭代次数为 $\lceil \log_2((u-l)/\epsilon) \rceil$，与 6.006 中二分搜索的 $O(\log n)$ 分析一字不差，只是这里的 $n$ 对应"初始区间长度 ÷ 精度"。🎥 Boyd 在视频里把这点讲得极其直白：*"at each step that interval of ignorance divides by two, so your ignorance halves at each iteration. So it takes log ten steps, and you reduce your ignorance by 1000, and 20 steps a million roughly."*（每步"无知区间"减半——10 步把无知缩小一千倍，20 步约一百万倍。）他还补了一个 slide 上没写的工程细节：若不知道 $l$、$u$ 的初始界，可先用指数探测（$t = 1, 2, 4, 8, \ldots$）找到可行与不可行的两端，再进入标准二分——这正是 6.006 中 unbounded binary search 的同款技巧。

<!-- p.17 -->

<span style="color:#2471a3;">**[definition]**</span>

### <span style="color:#c0392b;">线性规划 Linear program (LP)</span>

```math
\begin{array}{ll}
\text{minimize} & c^T x + d \\
\text{subject to} & Gx \preceq h \\
& Ax = b
\end{array}
```

- objective（目标函数）与约束函数均为仿射函数的凸问题
- 可行集是一个 polyhedron（多面体）

> **图示：** 多面体 $P$ 的顶点处标出最优点 $x^\star$ 与目标下降方向 $-c$——线性目标沿 $-c$ 方向下降，直到被多面体的面拦住，最优点落在顶点或面上。

<!-- p.18 -->

<span style="color:#2471a3;">**[example]**</span>

### <span style="color:#c0392b;">例子 Examples</span>

**膳食问题（diet problem）：** 选择 $n$ 种食物的用量 $x_1, \ldots, x_n$

- 食物 $j$ 的单价为 $c_j$，每单位含营养素 $i$ 的量为 $a_{ij}$
- 健康膳食要求营养素 $i$ 的摄入量至少为 $b_i$

求最便宜的健康膳食，即求解：

```math
\begin{array}{ll}
\text{minimize} & c^T x \\
\text{subject to} & Ax \succeq b, \quad x \succeq 0
\end{array}
```

**分段线性最小化（piecewise-linear minimization）：**

```math
\text{minimize} \quad \max_{i=1,\ldots,m} (a_i^T x + b_i)
```

等价于 LP：

```math
\begin{array}{ll}
\text{minimize} & t \\
\text{subject to} & a_i^T x + b_i \le t, \quad i = 1, \ldots, m
\end{array}
```

<!-- p.19 -->

<span style="color:#2471a3;">**[example]**</span>

### <span style="color:#c0392b;">多面体的切比雪夫中心 Chebyshev center of a polyhedron</span>

多面体

```math
P = \{ x \mid a_i^T x \le b_i,\ i = 1, \ldots, m \}
```

的 Chebyshev center（切比雪夫中心）是其最大内切球的球心：

```math
B = \{ x_c + u \mid \|u\|_2 \le r \}
```

> **图示：** 在多面体内画一个尽可能大的圆（球），圆心 $x^{c}$ 即切比雪夫中心，半径 $r$ 为内切半径。

- 对所有 $x \in B$ 都有 $a_i^T x \le b_i$，当且仅当

```math
\sup\{ a_i^T (x_c + u) \mid \|u\|_2 \le r \} = a_i^T x_c + r \|a_i\|_2 \le b_i
```

- 因此，$x_c$ 与 $r$ 可通过求解如下 LP 确定：

```math
\begin{array}{ll}
\text{maximize} & r \\
\text{subject to} & a_i^T x_c + r \|a_i\|_2 \le b_i, \quad i = 1, \ldots, m
\end{array}
```

<!-- p.20 -->

<span style="color:#2471a3;">**[definition]**</span>

### <span style="color:#c0392b;">线性分式规划（及广义形式）(Generalized) linear-fractional program</span>

```math
\begin{array}{ll}
\text{minimize} & f_0(x) \\
\text{subject to} & Gx \preceq h \\
& Ax = b
\end{array}
```

其中 linear-fractional program（线性分式规划）的目标为

```math
f_0(x) = \frac{c^T x + d}{e^T x + f}, \qquad \mathrm{dom}\, f_0 = \{ x \mid e^T x + f > 0 \}
```

- 这是一个 quasiconvex optimization problem（拟凸优化问题），可用 bisection（二分法）求解
- 它也等价于如下以 $y, z$ 为变量的 LP：

```math
\begin{array}{ll}
\text{minimize} & c^T y + d z \\
\text{subject to} & Gy \preceq h z \\
& Ay = b z \\
& e^T y + f z = 1 \\
& z \ge 0
\end{array}
```

<!-- p.21 -->

<span style="color:#2471a3;">**[example]**</span>

### <span style="color:#c0392b;">广义线性分式规划与 Von Neumann 增长经济模型 Generalized linear-fractional program and Von Neumann model of a growing economy</span>

generalized linear-fractional program（广义线性分式规划）：

```math
f_0(x) = \max_{i=1,\ldots,r} \frac{c_i^T x + d_i}{e_i^T x + f_i}, \qquad \mathrm{dom}\, f_0 = \{ x \mid e_i^T x + f_i > 0,\ i = 1, \ldots, r \}
```

这是一个拟凸优化问题，可用二分法求解。

**例子：Von Neumann 增长经济模型（Von Neumann model of a growing economy）：**

```math
\begin{array}{ll}
\text{maximize (over } x, x^+\text{)} & \min_{i=1,\ldots,n} x_i^+ / x_i \\
\text{subject to} & x^+ \succeq 0, \quad B x^+ \preceq A x
\end{array}
```

- $x, x^+ \in \mathbb{R}^n$：$n$ 个部门在本期与下一期的 activity level（活动水平）
- $(Ax)_i$、$(Bx^+)_i$：商品 $i$ 的产量与消耗量
- $x_i^+ / x_i$：部门 $i$ 的 growth rate（增长率）

配置活动水平，使增长最慢的部门的增长率最大化。

<!-- p.22 -->

<span style="color:#2471a3;">**[definition]**</span>

### <span style="color:#c0392b;">二次规划 Quadratic program (QP)</span>

```math
\begin{array}{ll}
\text{minimize} & (1/2) x^T P x + q^T x + r \\
\text{subject to} & Gx \preceq h \\
& Ax = b
\end{array}
```

- $P \in S^n_+$，因此 objective（目标函数）是 convex quadratic（凸二次）函数
- 即：在多面体上最小化一个凸二次函数

> **图示：** 多面体 $P$ 中标出最优点 $x^\star$ 与该点处的 $-\nabla f_0(x^\star)$——二次目标的等高线是椭圆，最优点未必在顶点，而在 $-\nabla f_0(x^\star)$ 方向被可行集"顶住"的位置。

<!-- p.23 -->

<span style="color:#2471a3;">**[example]**</span>

### <span style="color:#c0392b;">例子 Examples</span>

**least-squares（最小二乘）：**

```math
\text{minimize} \quad \|Ax - b\|_2^2
```

- 有解析解 $x^\star = A^\dagger b$（$A^\dagger$ 为 pseudo-inverse，伪逆）
- 可以追加线性约束，例如 $l \preceq x \preceq u$（此时成为 QP）

**随机成本的线性规划（linear program with random cost）：**

```math
\begin{array}{ll}
\text{minimize} & \bar{c}^T x + \gamma\, x^T \Sigma x = \mathbb{E}\, c^T x + \gamma\, \mathrm{var}(c^T x) \\
\text{subject to} & Gx \preceq h, \quad Ax = b
\end{array}
```

- $c$ 是 random vector（随机向量），均值为 $\bar{c}$，协方差为 $\Sigma$
- 于是 $c^T x$ 是随机变量，均值为 $\bar{c}^T x$，方差为 $x^T \Sigma x$
- $\gamma > 0$ 是 risk aversion parameter（风险厌恶参数），控制 expected cost（期望成本）与 variance（方差，即风险）之间的 trade-off（权衡）

<!-- p.24 -->

<span style="color:#2471a3;">**[definition]**</span>

### <span style="color:#c0392b;">二次约束二次规划 Quadratically constrained quadratic program (QCQP)</span>

```math
\begin{array}{ll}
\text{minimize} & (1/2) x^T P_0 x + q_0^T x + r_0 \\
\text{subject to} & (1/2) x^T P_i x + q_i^T x + r_i \le 0, \quad i = 1, \ldots, m \\
& Ax = b
\end{array}
```

- $P_i \in S^n_+$；objective（目标）与 constraint（约束）都是凸二次函数
- 若 $P_1, \ldots, P_m \in S^n_{++}$（正定），则可行域是 $m$ 个 ellipsoid（椭球）与一个仿射集的 intersection（交集）

<!-- p.25 -->

<span style="color:#2471a3;">**[definition]**</span>

### <span style="color:#c0392b;">二阶锥规划 Second-order cone programming</span>

```math
\begin{array}{ll}
\text{minimize} & f^T x \\
\text{subject to} & \|A_i x + b_i\|_2 \le c_i^T x + d_i, \quad i = 1, \ldots, m \\
& Fx = g
\end{array}
```

（其中 $A_i \in \mathbb{R}^{n_i \times n}$，$F \in \mathbb{R}^{p \times n}$）

- 这些不等式称为 second-order cone (SOC) constraints（二阶锥约束）：

```math
(A_i x + b_i,\ c_i^T x + d_i) \in \text{second-order cone in } \mathbb{R}^{n_i+1}
```

- 当 $n_i = 0$ 时退化为 LP；当 $c_i = 0$ 时退化为 QCQP
- 比 QCQP 与 LP 都更一般

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 本讲至此已经铺出一条严格的表达能力层级链：$\mathrm{LP} \subseteq \mathrm{QP} \subseteq \mathrm{QCQP} \subseteq \mathrm{SOCP} \subseteq \mathrm{SDP}$——本页指出 SOCP 在 $n_i = 0$、$c_i = 0$ 时分别退化为 LP 与 QCQP，第 37 页还会演示 LP 与 SOCP 如何写成 SDP。连接 Strang 线代：QP/QCQP 中的 $P \in S^n_+$ 正是你熟悉的半正定矩阵，其几何化身恰是椭球——特征向量定主轴方向，特征值平方根的倒数定半轴长度，因此上一页"可行域是椭球之交"的说法就是正定性的一句话翻译；SOCP 的锥约束 $\|A_ix + b_i\|_2 \le c_i^Tx + d_i$ 则把范数与仿射函数嫁接在一起。设计思路层面，Boyd 有意按"表达能力递增、而求解代价仍是多项式时间"来组织这条谱系：即将在第 12 讲登场的 interior-point method（内点法）对整条谱系统一适用，于是"把问题改写成 SOCP/SDP"本身就等价于"把问题变成可解问题"——这正呼应 6.006 的核心命题：多项式时间可解性是可行与不可行的分界线。现代建模语言 CVX（见 SEE官方讲义/AdditionalNotes3CVX.pdf）正是借助这条层级链完成自动凸性验证与问题转化。

<!-- p.26 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">鲁棒线性规划 Robust linear programming</span>

优化问题中的参数常常是不确定的（uncertain），例如在 LP

```math
\begin{array}{ll}
\text{minimize} & c^T x \\
\text{subject to} & a_i^T x \le b_i, \quad i = 1, \ldots, m
\end{array}
```

中，$c$、$a_i$、$b_i$ 都可能存在 uncertainty（不确定性）。

处理不确定性的两种常用方法（为简单起见，只考虑 $a_i$ 的不确定性）：

- **deterministic model（确定性模型）：** 约束必须对一切 $a_i \in E_i$ 成立：

```math
\begin{array}{ll}
\text{minimize} & c^T x \\
\text{subject to} & a_i^T x \le b_i \text{ for all } a_i \in E_i, \quad i = 1, \ldots, m
\end{array}
```

- **stochastic model（随机模型）：** $a_i$ 是随机变量；约束只需以 probability（概率）$\eta$ 成立：

```math
\begin{array}{ll}
\text{minimize} & c^T x \\
\text{subject to} & \mathrm{prob}(a_i^T x \le b_i) \ge \eta, \quad i = 1, \ldots, m
\end{array}
```

<!-- p.27 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">确定性方法：化为 SOCP Deterministic approach via SOCP</span>

- 取一个 ellipsoid（椭球）作为 $E_i$：

```math
E_i = \{ \bar{a}_i + P_i u \mid \|u\|_2 \le 1 \}
```

（$\bar{a}_i \in \mathbb{R}^n$，$P_i \in \mathbb{R}^{n \times n}$）

椭球中心为 $\bar{a}_i$，semi-axes（半轴）由 $P_i$ 的 singular values/vectors（奇异值/奇异向量）确定。

- 于是 robust LP（鲁棒 LP）

```math
\begin{array}{ll}
\text{minimize} & c^T x \\
\text{subject to} & a_i^T x \le b_i \quad \forall a_i \in E_i, \ i = 1, \ldots, m
\end{array}
```

等价于 SOCP：

```math
\begin{array}{ll}
\text{minimize} & c^T x \\
\text{subject to} & \bar{a}_i^T x + \|P_i^T x\|_2 \le b_i, \quad i = 1, \ldots, m
\end{array}
```

（依据：$\sup_{\|u\|_2 \le 1} (\bar{a}_i + P_i u)^T x = \bar{a}_i^T x + \|P_i^T x\|_2$）

<!-- p.28 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">随机方法：化为 SOCP Stochastic approach via SOCP</span>

- 假设 $a_i$ 服从 Gaussian（高斯分布），均值为 $\bar{a}_i$，协方差为 $\Sigma_i$（$a_i \sim \mathcal{N}(\bar{a}_i, \Sigma_i)$）
- 于是 $a_i^T x$ 是 Gaussian r.v.（高斯随机变量），均值为 $\bar{a}_i^T x$，方差为 $x^T \Sigma_i x$；因此

```math
\mathrm{prob}(a_i^T x \le b_i) = \Phi\!\left( \frac{b_i - \bar{a}_i^T x}{\|\Sigma_i^{1/2} x\|_2} \right)
```

其中 $\Phi(x) = \frac{1}{\sqrt{2\pi}} \int_{-\infty}^{x} e^{-t^2/2}\, dt$ 是 $\mathcal{N}(0,1)$ 的 CDF（累积分布函数）。

- 于是 robust LP（取 $\eta \ge 1/2$）

```math
\begin{array}{ll}
\text{minimize} & c^T x \\
\text{subject to} & \mathrm{prob}(a_i^T x \le b_i) \ge \eta, \quad i = 1, \ldots, m
\end{array}
```

等价于 SOCP：

```math
\begin{array}{ll}
\text{minimize} & c^T x \\
\text{subject to} & \bar{a}_i^T x + \Phi^{-1}(\eta)\, \|\Sigma_i^{1/2} x\|_2 \le b_i, \quad i = 1, \ldots, m
\end{array}
```

<!-- p.29 -->

<span style="color:#2471a3;">**[definition]**</span>

### <span style="color:#c0392b;">几何规划 Geometric programming</span>

monomial function（单项式函数）：

```math
f(x) = c\, x_1^{a_1} x_2^{a_2} \cdots x_n^{a_n}, \qquad \mathrm{dom}\, f = \mathbb{R}^n_{++}
```

其中 $c > 0$；exponent（指数）$\alpha_i$ 可取任意实数。

posynomial function（正项式函数）：若干单项式之和

```math
f(x) = \sum_{k=1}^{K} c_k\, x_1^{a_{1k}} x_2^{a_{2k}} \cdots x_n^{a_{nk}}, \qquad \mathrm{dom}\, f = \mathbb{R}^n_{++}
```

geometric program（几何规划，GP）：

```math
\begin{array}{ll}
\text{minimize} & f_0(x) \\
\text{subject to} & f_i(x) \le 1, \quad i = 1, \ldots, m \\
& h_i(x) = 1, \quad i = 1, \ldots, p
\end{array}
```

其中 $f_i$ 为正项式，$h_i$ 为单项式。

<!-- p.30 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">凸形式的几何规划 Geometric program in convex form</span>

作变量替换 $y_i = \log x_i$，并对目标与约束取对数：

- 单项式 $f(x) = c\, x_1^{a_1} \cdots x_n^{a_n}$ 变换为

```math
\log f(e^{y_1}, \ldots, e^{y_n}) = a^T y + b \qquad (b = \log c)
```

- 正项式 $f(x) = \sum_{k=1}^{K} c_k\, x_1^{a_{1k}} \cdots x_n^{a_{nk}}$ 变换为

```math
\log f(e^{y_1}, \ldots, e^{y_n}) = \log \left( \sum_{k=1}^{K} e^{a_k^T y + b_k} \right) \qquad (b_k = \log c_k)
```

- 几何规划由此变换为凸问题：

```math
\begin{array}{ll}
\text{minimize} & \log \left( \sum_{k=1}^{K} \exp(a_{0k}^T y + b_{0k}) \right) \\
\text{subject to} & \log \left( \sum_{k=1}^{K} \exp(a_{ik}^T y + b_{ik}) \right) \le 0, \quad i = 1, \ldots, m \\
& Gy + d = 0
\end{array}
```

<!-- p.31 -->

<span style="color:#2471a3;">**[example]**</span>

### <span style="color:#c0392b;">悬臂梁设计 Design of cantilever beam</span>

> **图示：** 一根水平悬臂梁左端固定，右端受垂直向下的力 $F$；梁被划分为 segment 4、segment 3、segment 2、segment 1（从固定端向自由端编号），各段横截面尺寸可不同。

- $N$ 个 segment（段），每段单位长度，横截面为 $w_i \times h_i$ 的矩形
- 给定的 vertical force（垂直力）$F$ 施加在右端

design problem（设计问题）：

- minimize（最小化）：total weight（总重量）
- subject to（约束条件）：$w_i$、$h_i$ 的 upper & lower bounds（上界与下界）
- 长宽比 $h_i/w_i$ 的上界与下界
- 每段 stress（应力）的上界
- 梁端 vertical deflection（垂直挠度）的上界

variables（变量）：$w_i$，$h_i$，$i = 1, \ldots, N$。

<!-- p.32 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">目标函数与约束函数 Objective and constraint functions</span>

- 总重量 $w_1 h_1 + \cdots + w_N h_N$ 是正项式
- 长宽比 $h_i/w_i$ 及其倒数 $w_i/h_i$ 是单项式
- 段 $i$ 中的最大应力为 $6iF/(w_i h_i^2)$，是单项式
- 段 $i$ 右端处中轴的 vertical deflection（垂直挠度）$y_i$ 与 slope（转角）$v_i$ 由以下递推定义：

```math
\begin{aligned}
v_i &= \frac{12(i - 1/2) F}{E\, w_i h_i^3} + v_{i+1} \\
y_i &= \frac{6(i - 1/3) F}{E\, w_i h_i^3} + v_{i+1} + y_{i+1}
\end{aligned}
```

对 $i = N, N-1, \ldots, 1$ 计算，且 $v_{N+1} = y_{N+1} = 0$（$E$ 为 Young's modulus，杨氏模量）。

$v_i$ 与 $y_i$ 是 $w$、$h$ 的正项式函数。

<!-- p.33 -->

<span style="color:#2471a3;">**[example]**</span>

### <span style="color:#c0392b;">写成几何规划 Formulation as a GP</span>

```math
\begin{array}{ll}
\text{minimize} & w_1 h_1 + \cdots + w_N h_N \\
\text{subject to} & w_{\max}^{-1} w_i \le 1, \quad w_{\min} w_i^{-1} \le 1, \quad i = 1, \ldots, N \\
& h_{\max}^{-1} h_i \le 1, \quad h_{\min} h_i^{-1} \le 1, \quad i = 1, \ldots, N \\
& S_{\max}^{-1} w_i^{-1} h_i \le 1, \quad S_{\min} w_i h_i^{-1} \le 1, \quad i = 1, \ldots, N \\
& 6iF \sigma_{\max}^{-1} w_i^{-1} h_i^{-2} \le 1, \quad i = 1, \ldots, N \\
& y_{\max}^{-1} y_1 \le 1
\end{array}
```

note（注意）：

- 我们把 $w_{\min} \le w_i \le w_{\max}$ 与 $h_{\min} \le h_i \le h_{\max}$ 写成

```math
w_{\min}/w_i \le 1, \quad w_i/w_{\max} \le 1, \quad h_{\min}/h_i \le 1, \quad h_i/h_{\max} \le 1
```

- 我们把 $S_{\min} \le h_i/w_i \le S_{\max}$ 写成

```math
S_{\min} w_i/h_i \le 1, \quad h_i/(w_i S_{\max}) \le 1
```

<!-- p.34 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">最小化非负矩阵的谱半径 Minimizing spectral radius of nonnegative matrix</span>

Perron-Frobenius eigenvalue（Perron-Frobenius 特征值）$\lambda_{\mathrm{pf}}(A)$：

- 对（逐元素）positive（正）的 $A \in \mathbb{R}^{n \times n}$ 存在
- 是 $A$ 的一个实正特征值，等于 spectral radius（谱半径）$\max_i |\lambda_i(A)|$
- 决定 $A^k$ 的渐近增长（衰减）速率：当 $k \to \infty$ 时 $A^k \sim \lambda_{\mathrm{pf}}^k$
- 另一种刻画：$\lambda_{\mathrm{pf}}(A) = \inf\{ \lambda \mid Av \preceq \lambda v \text{ for some } v \succ 0 \}$

**最小化由正项式构成的矩阵的谱半径：**

- 最小化 $\lambda_{\mathrm{pf}}(A(x))$，其中 $A(x)$ 的元素 $A(x)_{ij}$ 是 $x$ 的正项式
- 等价几何规划：

```math
\begin{array}{ll}
\text{minimize} & \lambda \\
\text{subject to} & \sum_{j=1}^{n} A(x)_{ij} v_j / (\lambda v_i) \le 1, \quad i = 1, \ldots, n
\end{array}
```

variables（变量）：$\lambda$，$v$，$x$。

<!-- p.35 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">广义不等式约束 Generalized inequality constraints</span>

带广义不等式约束的凸问题：

```math
\begin{array}{ll}
\text{minimize} & f_0(x) \\
\text{subject to} & f_i(x) \preceq_{K_i} 0, \quad i = 1, \ldots, m \\
& Ax = b
\end{array}
```

- $f_0 : \mathbb{R}^n \to \mathbb{R}$ 凸；$f_i : \mathbb{R}^n \to \mathbb{R}^{k_i}$ 关于 proper cone（正常锥）$K_i$ 为 $K_i$-convex（$K_i$-凸）
- 与标准凸问题性质相同（可行集凸、局部最优即全局最优等）

conic form problem（锥形式问题）：目标与约束均为仿射的特例：

```math
\begin{array}{ll}
\text{minimize} & c^T x \\
\text{subject to} & Fx + g \preceq_K 0 \\
& Ax = b
\end{array}
```

它把 linear programming（线性规划，$K = \mathbb{R}^m_+$）推广到 nonpolyhedral cones（非多面体锥）。

<!-- p.36 -->

<span style="color:#2471a3;">**[definition]**</span>

### <span style="color:#c0392b;">半定规划 Semidefinite program (SDP)</span>

```math
\begin{array}{ll}
\text{minimize} & c^T x \\
\text{subject to} & x_1 F_1 + x_2 F_2 + \cdots + x_n F_n + G \preceq 0 \\
& Ax = b
\end{array}
```

其中 $F_i, G \in S^k$。

- 该不等式约束称为 linear matrix inequality（线性矩阵不等式，LMI）
- 多条 LMI 约束的问题也涵盖在内：例如

```math
x_1 \hat{F}_1 + \cdots + x_n \hat{F}_n + \hat{G} \preceq 0, \qquad x_1 \tilde{F}_1 + \cdots + x_n \tilde{F}_n + \tilde{G} \preceq 0
```

等价于单条 LMI：

```math
x_1 \begin{bmatrix} \hat{F}_1 & 0 \\ 0 & \tilde{F}_1 \end{bmatrix} + x_2 \begin{bmatrix} \hat{F}_2 & 0 \\ 0 & \tilde{F}_2 \end{bmatrix} + \cdots + x_n \begin{bmatrix} \hat{F}_n & 0 \\ 0 & \tilde{F}_n \end{bmatrix} + \begin{bmatrix} \hat{G} & 0 \\ 0 & \tilde{G} \end{bmatrix} \preceq 0
```

<!-- p.37 -->

<span style="color:#2471a3;">**[example]**</span>

### <span style="color:#c0392b;">LP 与 SOCP 作为 SDP LP and SOCP as SDP</span>

**LP 与等价的 SDP：**

```math
\begin{array}{ll}
\text{LP:} & \begin{array}{ll} \text{minimize} & c^T x \\ \text{subject to} & Ax \preceq b \end{array}
\qquad
\begin{array}{ll} \text{SDP:} & \text{minimize } c^T x \\ & \text{subject to } \mathrm{diag}(Ax - b) \preceq 0 \end{array}
\end{array}
```

（注意广义不等号 $\preceq$ 在两处的不同解释：分量序与矩阵半正定序。）

**SOCP 与等价的 SDP：**

```math
\begin{array}{ll}
\text{SOCP:} & \begin{array}{ll} \text{minimize} & f^T x \\ \text{subject to} & \|A_i x + b_i\|_2 \le c_i^T x + d_i, \ i = 1, \ldots, m \end{array}
\end{array}
```

```math
\begin{array}{ll}
\text{SDP:} & \text{minimize } f^T x \\
& \text{subject to } \begin{bmatrix} (c_i^T x + d_i) I & A_i x + b_i \\ (A_i x + b_i)^T & c_i^T x + d_i \end{bmatrix} \succeq 0, \quad i = 1, \ldots, m
\end{array}
```

<!-- p.38 -->

<span style="color:#2471a3;">**[example]**</span>

### <span style="color:#c0392b;">特征值最小化 Eigenvalue minimization</span>

```math
\text{minimize} \quad \lambda_{\max}(A(x))
```

其中 $A(x) = A_0 + x_1 A_1 + \cdots + x_n A_n$（各 $A_i \in S^k$ 给定）。

等价 SDP：

```math
\begin{array}{ll}
\text{minimize} & t \\
\text{subject to} & A(x) \preceq t I
\end{array}
```

- variables（变量）：$x \in \mathbb{R}^n$，$t \in \mathbb{R}$
- 依据：

```math
\lambda_{\max}(A) \le t \iff A \preceq t I
```

<!-- p.39 -->

<span style="color:#2471a3;">**[example]**</span>

### <span style="color:#c0392b;">矩阵范数最小化 Matrix norm minimization</span>

```math
\text{minimize} \quad \|A(x)\|_2 = \left( \lambda_{\max}(A(x)^T A(x)) \right)^{1/2}
```

其中 $A(x) = A_0 + x_1 A_1 + \cdots + x_n A_n$（各 $A_i \in \mathbb{R}^{p \times q}$ 给定）。

等价 SDP：

```math
\begin{array}{ll}
\text{minimize} & t \\
\text{subject to} & \begin{bmatrix} tI & A(x) \\ A(x)^T & tI \end{bmatrix} \succeq 0
\end{array}
```

- variables（变量）：$x \in \mathbb{R}^n$，$t \in \mathbb{R}$
- 该约束依据：

```math
\|A\|_2 \le t \iff A^T A \preceq t^2 I,\ t \ge 0 \iff \begin{bmatrix} tI & A \\ A^T & tI \end{bmatrix} \succeq 0
```

<!-- p.40 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">向量优化 Vector optimization</span>

general vector optimization problem（一般向量优化问题）：

```math
\begin{array}{ll}
\text{minimize (w.r.t. } K\text{)} & f_0(x) \\
\text{subject to} & f_i(x) \le 0, \quad i = 1, \ldots, m \\
& h_i(x) = 0, \quad i = 1, \ldots, p
\end{array}
```

vector objective（向量目标）$f_0 : \mathbb{R}^n \to \mathbb{R}^q$，关于 proper cone（正常锥）$K \in \mathbb{R}^q$ 求最小。

convex vector optimization problem（凸向量优化问题）：

```math
\begin{array}{ll}
\text{minimize (w.r.t. } K\text{)} & f_0(x) \\
\text{subject to} & f_i(x) \le 0, \quad i = 1, \ldots, m \\
& Ax = b
\end{array}
```

其中 $f_0$ 为 $K$-convex（$K$-凸），$f_1, \ldots, f_m$ 为凸。

<!-- p.41 -->

<span style="color:#2471a3;">**[definition]**</span>

### <span style="color:#c0392b;">最优与 Pareto 最优点 Optimal and Pareto optimal points</span>

achievable objective values（可达目标值）的集合：

```math
\mathcal{O} = \{ f_0(x) \mid x \text{ feasible} \}
```

- 可行点 $x$ 是 optimal（最优）的，若 $f_0(x)$ 是 $\mathcal{O}$ 的 minimum value（最小值）
- 可行点 $x$ 是 Pareto optimal（Pareto 最优）的，若 $f_0(x)$ 是 $\mathcal{O}$ 的 minimal value（极小值）

> **图示：** 左图中 $f_0(x^\star)$ 是可达集 $\mathcal{O}$ 的全局最小值，$x^\star$ 为最优；右图中 $f_0(x_{\mathrm{po}})$ 只是 $\mathcal{O}$ 的边界（极小值）点，不存在全面更优者，$x_{\mathrm{po}}$ 为 Pareto 最优。

<!-- p.42 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">多准则优化 Multicriterion optimization</span>

取 $K = \mathbb{R}^q_+$ 的向量优化问题：

```math
f_0(x) = (F_1(x), \ldots, F_q(x))
```

- 有 $q$ 个不同的 objectives（目标）$F_i$；粗略地说，我们希望所有 $F_i$ 都小
- 可行点 $x^\star$ 是 optimal（最优）的，若

```math
y \text{ feasible} \implies f_0(x^\star) \preceq f_0(y)
```

若存在最优点，则各目标 noncompeting（不相互竞争）。

- 可行点 $x_{\mathrm{po}}$ 是 Pareto optimal（Pareto 最优）的，若

```math
y \text{ feasible},\ f_0(y) \preceq f_0(x_{\mathrm{po}}) \implies f_0(x_{\mathrm{po}}) = f_0(y)
```

若存在多个 Pareto 最优值，则各目标之间存在 trade-off（权衡）。

<!-- p.43 -->

<span style="color:#2471a3;">**[example]**</span>

### <span style="color:#c0392b;">正则化最小二乘 Regularized least-squares</span>

```math
\text{minimize (w.r.t. } \mathbb{R}^2_+\text{)} \quad \left( \|Ax - b\|_2^2,\ \|x\|_2^2 \right)
```

> **图示：** 以 $F_1(x) = \|Ax - b\|_2^2$ 为横轴、$F_2(x) = \|x\|_2^2$ 为纵轴，画出可达目标集 $\mathcal{O}$；示例取 $A \in \mathbb{R}^{100 \times 10}$，粗线即由全部 Pareto optimal points（Pareto 最优点）构成的"边界"。

<!-- p.44 -->

<span style="color:#2471a3;">**[example]**</span>

### <span style="color:#c0392b;">投资组合优化中的风险-收益权衡 Risk return trade-off in portfolio optimization</span>

```math
\begin{array}{ll}
\text{minimize (w.r.t. } \mathbb{R}^2_+\text{)} & (-\bar{p}^T x,\ x^T \Sigma x) \\
\text{subject to} & \mathbf{1}^T x = 1, \quad x \succeq 0
\end{array}
```

- $x \in \mathbb{R}^n$ 是 investment portfolio（投资组合）；$x_i$ 是投入资产 $i$ 的比例
- $p \in \mathbb{R}^n$ 是资产相对价格变化向量；建模为 random variable（随机变量），均值为 $\bar{p}$，协方差为 $\Sigma$
- $\bar{p}^T x = \mathbb{E}\, r$ 是 expected return（期望收益）；$x^T \Sigma x = \mathrm{var}\, r$ 是 return variance（收益方差）

> **图示：** 左图为 mean return（平均收益）对 standard deviation of return（收益标准差）的可达集与边界；右图显示边界上四个不同 allocation（配置）$x(1), \ldots, x(4)$ 中各资产的持仓比例随风险厌恶程度的变化。

<!-- p.45 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">标量化 Scalarization</span>

求 Pareto 最优点的方法：选取 $\lambda \succ_{K^*} 0$ 并求解标量问题

```math
\begin{array}{ll}
\text{minimize} & \lambda^T f_0(x) \\
\text{subject to} & f_i(x) \le 0, \quad i = 1, \ldots, m \\
& h_i(x) = 0, \quad i = 1, \ldots, p
\end{array}
```

<span style="color:#00838f;">**若 $x$ 是标量问题的最优解，则它是向量优化问题的 Pareto 最优解。**</span>

> **图示：** 用不同法向量 $\lambda_1, \lambda_2, \lambda_3$ 的支撑超平面去"推"可达目标集 $\mathcal{O}$，分别顶到 $f_0(x_1), f_0(x_2), f_0(x_3)$ 三个 Pareto 最优点——换一个权向量就换一个 Pareto 点。

对 convex vector optimization problem（凸向量优化问题），通过遍取 $\lambda \succ_{K^*} 0$，可以找到（几乎）所有 Pareto 最优点。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 标量化是连接向量优化与你正在学的概率论（Bertsekas）的天然桥梁：第 44 页的投资组合问题把"收益"与"风险"这两个随机变量的矩（$\mathbb{E}\,r$ 与 $\mathrm{var}\,r$）并成二维目标 $(-\bar{p}^Tx,\ x^T\Sigma x)$，而标量化按 $\gamma$ 加权后得到的正是 Markowitz 于 1952 年提出的 mean-variance model（均值-方差模型）——Markowitz 因此获 1990 年诺贝尔经济学奖，$\gamma$ 就是 risk aversion（风险厌恶）系数。第一性原理地看，Pareto 最优刻画的是"无法不牺牲一方而改善另一方"的不可改进点；标量化则证明：每个正权向量 $\lambda$ 定义一条支撑超平面，它与可达目标集 $\mathcal{O}$ 的切点必为 Pareto 最优点——这就是"线性加权必不亏"的几何解释。凸性在此再次兑现红利：对凸向量优化问题，几乎所有 Pareto 最优点都能被某个 $\lambda$ 取到；而在非凸问题中，加权法只能取到"被支撑"的那部分 Pareto 点，会漏掉凹陷边界上的点——这一缺口要等你以后接触非凸优化时才能真正体会。第 43、46、47 页反复出现的 $\gamma$ 扫描曲线，就是金融学里的 efficient frontier（有效前沿）。

<!-- p.46 -->

<span style="color:#2471a3;">**[example]**</span>

### <span style="color:#c0392b;">多准则问题的标量化 Scalarization for multicriterion problems</span>

求 Pareto 最优点：最小化正加权和

```math
\lambda^T f_0(x) = \lambda_1 F_1(x) + \cdots + \lambda_q F_q(x)
```

<span style="color:#2471a3;">**[example]**</span> **例子：**

- 第 4–43 页的正则化最小二乘问题：取 $\lambda = (1, \gamma)$，$\gamma > 0$：

```math
\text{minimize} \quad \|Ax - b\|_2^2 + \gamma \|x\|_2^2
```

对固定的 $\gamma$，这是一个 LS（最小二乘）问题。

> **图示：** 横轴 $\|Ax-b\|_2^2$、纵轴 $\|x\|_2^2$ 的 optimal trade-off curve（最优权衡曲线）上，标出 $\gamma = 1$ 时加权目标所对应的那个点。

<!-- p.47 -->

<span style="color:#2471a3;">**[example]**</span>

### <span style="color:#c0392b;">标量化（续）：风险-收益权衡 Scalarization (continued): risk-return trade-off</span>

- 第 4–44 页的风险-收益权衡问题：

```math
\begin{array}{ll}
\text{minimize} & -\bar{p}^T x + \gamma\, x^T \Sigma x \\
\text{subject to} & \mathbf{1}^T x = 1, \quad x \succeq 0
\end{array}
```

对固定的 $\gamma > 0$，这是一个 quadratic program（二次规划）。
