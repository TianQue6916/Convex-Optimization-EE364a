# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

> <span style="color:#7f8c8d;">Source: SEE复习课/review2_single.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

## <span style="color:#2471a3;">**[lecture]**</span> <span style="color:#c0392b">EE364a Review Session 2（EE364A 复习课 2）</span>

<!-- p.1 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Session outline（本节课大纲）</span>

本次 review session（复习课）围绕三个主题展开：**dual cones（对偶锥）**、**convex functions（凸函数）**、**conjugate function（共轭函数）**——分别对应教材第 2 章的 cone（锥）与对偶锥、第 3 章的凸函数及其保凸运算，以及共轭函数。

<!-- p.2 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Dual cones（对偶锥）</span>

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">**Definition（定义）.** 对 cone（锥）$K$，其 dual cone（对偶锥）定义为 $K^* = \{y \mid y^T x \ge 0,\ \text{for all } x \in K\}$。$y \in K^*$ 当且仅当 halfspace（半空间）$\{z \mid y^T z \ge 0\}$ 包含 $K$。</span>

配图给出锥 $K$ 与其对偶锥 $K^*$：图中以两个 $90°$ 角标注两者的关系——$K^*$ 中每个向量与 $K$ 中所有向量的夹角都不超过 $90°$，这正是定义式 $y^T x \ge 0$ 的几何含义。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 对偶锥最直接的读法是『夹角不超过 $90°$』：$y \in K^*$ 意味着 $y$ 与 $K$ 中任意向量的内积非负，即夹角不超过直角——配图里两个 $90°$ 标注画的正是这件事。你在线性代数（Strang）里学过的 orthogonal complement（正交补）$K^\perp$ 是它的退化特例：当 $K$ 本身是一个 subspace（子空间）时 $K^* = K^\perp$，因为子空间沿 $-x$ 方向也封闭，内积必须恰为零才能对一切 $x$ 非负。把要求从『$= 0$』放宽到『$\ge 0$』，正交补就推广成了对偶锥；这个放宽正是后面 Lagrange 对偶理论的种子——届时乘子的非负约束 $\lambda \succeq 0$，几何本质就是『乘子落在非负象限这个 self-dual cone（自对偶锥）之中』。在 $\mathbb{R}^2$ 里可以亲手验证：顶点在原点、总张角为 $\theta$ 的锥，其对偶锥的总张角恰为 $180° - \theta$。所以锥越『窄』对偶越『宽』，两条边界射线互相垂直。

<!-- p.3 -->

### <span style="color:#2471a3;">**[exercise]**</span> <span style="color:#c0392b">ex. 2.32（习题 2.32）：求对偶锥</span>

Find the dual cone of $\{Ax \mid x \succeq 0\}$, where $A \in \mathbb{R}^{m \times n}$.

> <span style="color:#2471a3;">**[solution]**</span> **Solution（解答）.** 记 $K = \{Ax \mid x \succeq 0\}$，按定义展开，并用 $y^T(Ax) = (A^T y)^T x$ 换元：
>
> ```math
> K^* = \{y \mid y^T x \ge 0 \text{ for all } x \in K\} = \{y \mid (A^T y)^T x \ge 0 \text{ for all } x \succeq 0\}
> ```
>
> 这等价于 $K^* = \{y \mid A^T y \succeq 0\}$，分两步说明：
>
> - **sufficient（充分性）**：若 $A^T y \succeq 0$，则对任意 $x \succeq 0$ 都有 $(A^T y)^T x \ge 0$（非负向量与半正定向量的内积非负）；
> - **necessary（必要性）**：反设存在某个 $i$ 使 $(A^T y)_i < 0$，取 $x = e_i$（标准基向量，满足 $e_i \succeq 0$），则 $(A^T y)^T e_i < 0$，与定义矛盾。

<!-- p.4 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Convex functions（凸函数）：工具与保凸运算</span>

本页是凸函数部分的工具清单：

- **tools（判定工具）**：
  - 凸性的 definition（定义）；
  - first-order condition（一阶条件）；
  - second-order condition（二阶条件）；
  - restriction to a line（限制到直线上验证）；
  - 简单例子：negative log（负对数）函数、norms（范数）、quadratic-over-linear（二次-线性）函数、log-sum-exp（对数-求和-指数）函数等；
- **convexity-preserving operations（保凸运算）**：
  - nonnegative weighted sum（非负加权和）；
  - composition with an affine function（与仿射函数复合）；
  - pointwise maximum and supremum（逐点最大值与逐点上确界）；
  - minimization over convex sets（在凸集上取最小）；
  - composition（复合）；
  - perspective（透视函数）。

<!-- p.5 -->

### <span style="color:#2471a3;">**[example]**</span> <span style="color:#c0392b">Example: sigmoid / logistic function（例：Sigmoid / Logistic 函数）</span>

考虑 sigmoid / logistic function（Sigmoid / Logistic 函数）：

```math
f(x) = \frac{1}{1 + e^{-x}}
```

对它依次提出三个问题：

- 它是 convex（凸）的还是 concave（凹）的？
- 它是 quasiconvex（拟凸）的还是 quasiconcave（拟凹）的？
- 它是 log-convex（对数凸）的还是 log-concave（对数凹）的？

<!-- p.6 -->

### <span style="color:#2471a3;">**[example]**</span> <span style="color:#c0392b">问题一：convex（凸）还是 concave（凹）？</span>

- 它是 convex（凸）的还是 concave（凹）的？配图给出 $f$ 在 $[-10, 10]$ 上的图像：一条从 $0$ 平滑上升到 $1$ 的 S 形曲线。

> <span style="color:#2471a3;">**[solution]**</span> **Solution（解答）.**
>
> - 从图上看，$f$ 既非凸也非凹；
> - 也可以用二阶条件验证：
>
> ```math
> f''(x) = \frac{-e^{-x}\,\bigl(1 - e^{-x}\bigr)}{\bigl(1 + e^{-x}\bigr)^3}
> ```
>
> 当 $x < 0$ 时 $f''(x) > 0$（凸），当 $x \ge 0$ 时 $f''(x) \le 0$（凹）——凸性在原点附近翻转，所以整体上既非凸也非凹。

<!-- p.7 -->

### <span style="color:#2471a3;">**[example]**</span> <span style="color:#c0392b">问题二：quasiconvex（拟凸）还是 quasiconcave（拟凹）？</span>

- 它是 quasiconvex（拟凸）的还是 quasiconcave（拟凹）的？

> <span style="color:#2471a3;">**[solution]**</span> **Solution（解答）.**
>
> - sublevel sets（下水平集）$C_\alpha = \{x \mid f(x) \le \alpha\}$ 都是凸集，故 $f$ 是 quasiconvex（拟凸）的：
>   - 若 $\alpha \le 0$，则 $C_\alpha = \emptyset$；
>   - 若 $\alpha \ge 1$，则 $C_\alpha = \mathbb{R}$；
>   - 若 $0 < \alpha < 1$，则 $C_\alpha = (-\infty,\, f^{-1}(\alpha)]$；
> - 类似地，superlevel sets（上水平集）都是凸集，故 $f$ 是 quasiconcave（拟凹）的；
> - 定义在 $\mathbb{R}$ 上的一元函数：$f$ 单调（monotonic）$\iff$ $f$ 既拟凸又拟凹。

<!-- p.8 -->

### <span style="color:#2471a3;">**[example]**</span> <span style="color:#c0392b">问题三：log-convex（对数凸）还是 log-concave（对数凹）？</span>

- 它是 log-convex（对数凸）的还是 log-concave（对数凹）的？配图给出 $\log f$ 在 $[-10, 10]$ 上的图像（纵轴刻度约 $[-12, 2]$）：一条单调上升、趋于 $0$ 的曲线。

> <span style="color:#2471a3;">**[solution]**</span> **Solution（解答）.**
>
> - $f$ 不是 log-convex（对数凸）的；
> - $f$ 是 log-concave（对数凹）的：$\log f(x)$ 恰好是 log-sum-exp 函数的相反数，在 $z_1 = 1$、$z_2 = -x$ 处取值，即
>
> ```math
> \log f(x) = -\log\!\left(e^{z_1} + e^{z_2}\right)\Big|_{z_1 = 1,\ z_2 = -x} = -\log\!\left(1 + e^{-x}\right)
> ```
>
> 而 log-sum-exp 是凸函数，其相反数为凹，故 $\log f$ 凹，即 $f$ 对数凹。

<!-- p.9 -->

### <span style="color:#2471a3;">**[example]**</span> <span style="color:#c0392b">Example（例）：这个函数是凸的吗？</span>

问题：下面的函数（以 $(x, y, z) \in \mathbb{R}^3$ 为变元）是 convex function（凸函数）吗？

```math
f(x, y, z) = \frac{(x - z)^2}{y + 1} + \max\left\{1 + |x| - y,\ \frac{1}{\sqrt{z}},\ 0\right\}
```

其 domain（定义域）为 $y + 1 > 0$，$z > 0$。

> <span style="color:#2471a3;">**[solution]**</span> **Solution（解答）.** 以下步骤说明该函数是凸的：
>
> - $|x|$ 关于 $x$ 是凸的，$1 - y$ 是仿射的，故 $1 + |x| - y$ 是凸的；
> - $1/\sqrt{z}$ 是 negative-power function（负幂函数），故关于 $z$ 是凸的；
> - max 项是凸的：逐点最大值保凸，且它的各个 argument（变元）都是凸的；
> - $(x - z)^2/(y + 1)$ 是 quadratic-over-linear 函数 $s^2/t$ 与 affine（仿射）映射 $(x, y, z) \mapsto (x - z,\ y + 1)$ 的复合，故是凸的；
> - 左右两项之和是凸的。

<!-- p.10 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Composition rules（复合规则）</span>

<span style="color:#2471a3;">**[theorem]**</span> <span style="color:#c0392b;">**Composition rule（复合规则）.**</span> 设 $g : \mathbb{R}^n \to \mathbb{R}^k$，$h : \mathbb{R}^k \to \mathbb{R}$，二者的 composition（复合）为

```math
f(x) = h(g(x)) = h\bigl(g_1(x),\, g_2(x),\, \ldots,\, g_k(x)\bigr)
```

例如：若各 $g_i$ 是 concave（凹）的，$h$ 是 convex（凸）的，且 $\tilde{h}$ 对每个变元 nonincreasing（单调不增），则 $f$ 是凸的。

> <span style="color:#2471a3;">**[proof]**</span> *Proof（证明）.*（以 $n = 1$、$g$ 与 $h$ 可微的情形为例）由链式法则展开二阶导数：
>
> ```math
> f''(x) = g'(x)^T\, \underbrace{\nabla^2 h(g(x))}_{\succeq\ 0}\, g'(x) \;+\; \Bigl(\underbrace{\nabla h(g(x))}_{\preceq\ 0}\Bigr)^{\!T} \underbrace{g''(x)}_{\preceq\ 0}
> ```
>
> 三处大括号下的符号逐一核对：第一项是关于 $g'(x)$ 的半正定二次型，非负；第二项的两个因子 $\nabla h(g(x)) \preceq 0$（$h$ 单调不增）与 $g''(x) \preceq 0$（$g$ 凹）逐分量相乘，非正乘非正得非负。两项相加得 $f''(x) \ge 0$，故 $f$ 凸。$\square$

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 可以把本页内容提炼成一个可复用的思维模板——『复合函数查表法』：遇到 $f = h(g_1, \ldots, g_k)$，先不急着求导，依次问两个问题：每个 $g_i$ 是凸还是凹？$h$ 是凸还是凹、对每个变元单调不减还是单调不增？四格配对即可下结论：$g$ 凸 + $h$ 凸 + $h$ 单调不减 $\Rightarrow$ $f$ 凸；$g$ 凹 + $h$ 凸 + $h$ 单调不增 $\Rightarrow$ $f$ 凸（本页证明的情形）；另外两格是 $g$ 凹 + $h$ 凹 + $h$ 单调不减 $\Rightarrow$ $f$ 凹、$g$ 凸 + $h$ 凹 + $h$ 单调不增 $\Rightarrow$ $f$ 凹。单调方向为什么必须配套？看证明的第二项就明白了：$\nabla h(g(x)) \preceq 0$ 的每个分量与 $g''(x) \preceq 0$ 的每个分量都非正，两个非正数相乘才非负——若把 $h$ 换成单调不减，这一项的符号就失控了。所以这套规则不是死记硬背的表格，而是链式法则在二阶项上的『符号配平』；Boyd 书 §3.2.4 给出完整的规则表，配合本页 $n = 1$ 的证明，足以应付绝大多数凸性判断题。

<!-- p.11 -->

### <span style="color:#2471a3;">**[exercise]**</span> <span style="color:#c0392b">ex. 3.22(b)（习题 3.22(b)）：用复合规则证凸性</span>

Show that the following function is convex:

```math
f(x, u, v) = -\sqrt{uv - x^T x}
```

on $\mathbf{dom}\, f = \{(x, u, v) \mid uv > x^T x,\ u, v > 0\}$. Use the fact that $x^T x / u$ is convex in $(x, u)$ for $u > 0$, and that $-\sqrt{x_1 x_2}$ is convex on $\mathbb{R}^2_{++}$.

> <span style="color:#2471a3;">**[solution]**</span> **Solution（解答）.**
>
> - 改写 $f(x, u, v) = -\sqrt{u\,\bigl(v - x^T x / u\bigr)}$；
> - $g_1(u, v, x) = u$ 与 $g_2(u, v, x) = v - x^T x / u$ 都是 concave（凹）的：由题给事实，$x^T x / u$ 关于 $(x, u)$（$u > 0$）是凸的，取负号并加上仿射项 $v$ 即得凹函数；
> - 函数
>
> ```math
> h(z_1, z_2) =
> \begin{cases}
> -\sqrt{z_1 z_2}, & \text{if } z \succeq 0\\
> 0, & \text{otherwise}
> \end{cases}
> ```
>
> 是凸的，且对每个变元 decreasing（单调递减）；
> - 因此 $f(u, v, x) = h\bigl(g_1(u, v, x),\, g_2(u, v, x)\bigr)$ 作为复合是凸的。

<!-- p.12 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Conjugate function（共轭函数）</span>

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">**Definition（定义）.** 函数 $f$ 的 conjugate function（共轭函数）定义为</span>

```math
f^*(y) = \sup_{x \in \mathbf{dom}\, f}\left(y^T x - f(x)\right)
```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 共轭函数是凸分析中最优雅的『换坐标』思想：$f^*(y)$ 度量的是『斜率向量为 $y$ 的线性函数 $y^T x$ 能从下方多贴近 $f$ 的图像』。对一维可微凸函数，这个上确界在 $f'(x) = y$ 处取得，于是 $f^*(y) = y\,x(y) - f(x(y))$——自变量从位置 $x$ 换成斜率 $y$，这正是 Legendre 变换（Legendre 于 1787 年在天体力学研究中引入）；Fenchel 在 1949 年的论文 On conjugate convex functions 中把它推广到一般凸函数（允许取 $+\infty$），故也称 Fenchel conjugate（Fenchel 共轭）。一个帮你建立手感的例子：$f(x) = \tfrac{1}{2}x^2$ 的共轭是 $f^*(y) = \tfrac{1}{2}y^2$——抛物线的共轭仍是抛物线，『位置坐标』与『斜率坐标』下的能量形式恰好相同。共轭的威力到对偶性一章会全面显现：对偶函数的计算本质上是共轭计算，因为 $\inf_x\bigl(f(x) - y^T x\bigr) = -f^*(y)$——会算共轭就会算对偶。

<!-- p.13 -->

### <span style="color:#2471a3;">**[exercise]**</span> <span style="color:#c0392b">ex. 3.36(a)（习题 3.36(a)）：max 函数的共轭</span>

Derive the conjugate of the max function

```math
f(x) = \max_{i=1,\ldots,n} x_i
```

on $\mathbb{R}^n$.

> <span style="color:#2471a3;">**[solution]**</span> **Solution（解答，部分）.** 我们先看 $n = 2$ 时会发生什么：
>
> - 第一步，要确定 conjugate function 的自变量 $y$ 的定义域，即 $y^T x - f(x)$ 在何处 bounded above（上方有界）；
> - 先试带负分量的 $y$（即存在某个 $y_k < 0$）：
>   - 例如取 $y = (-1, 0)$；
>   - 那么取 $x = -t e_1$，就有 $y^T x - \max_i x_i = t - 0 \to \infty$（当 $t \to \infty$）；
>   - 所以必须有 $y \succeq 0$；
> - （下一页继续……）

<!-- p.14 -->

### <span style="color:#2471a3;">**[solution]**</span> <span style="color:#c0392b">ex. 3.36(a)（续）：$y \succeq 0$ 的情形与结论</span>

> <span style="color:#2471a3;">**[solution]**</span> **Solution（解答，续）.** 现在考察 $y \succeq 0$ 的情形：
>
> - 试 $y = (0.7, 0.7)$；
> - 取 $x = t\mathbf{1}$（全 1 向量的 $t$ 倍），则 $y^T x - \max_i x_i = t(\mathbf{1}^T y) - t = 1.4t - t \to \infty$（当 $t \to \infty$）；
> - 故 $y = (0.7, 0.7) \notin \mathbf{dom}\, f^*$；
> - 对 $x = t\mathbf{1}$：若 $y \succeq 0$，要使 $y^T x - \max_i x_i$ 上方有界，需要 $\mathbf{1}^T y = 1$；
> - 对 $y \in \{y \mid y \succeq 0,\ \mathbf{1}^T y = 1\}$，$\sup_{x \in \mathbf{dom} f}\bigl(y^T x - \max_i x_i\bigr)$ 等于多少？
>   - 可以证明 $y^T x \le \max_i x_i$（为什么？），且等号在 $x = 0$ 处成立；
>   - 所以对 $y \succeq 0$ 且 $\mathbf{1}^T y = 1$，这个 sup 总是上方有界的；
>   - 于是
>
> ```math
> f^*(y) =
> \begin{cases}
> 0, & \text{if } y \succeq 0 \text{ and } \mathbf{1}^T y = 1\\
> \infty, & \text{otherwise}
> \end{cases}
> ```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这个结果值得停一停：max 函数的共轭在 $\{y \mid y \succeq 0,\ \mathbf{1}^T y = 1\}$ 上取 $0$、其余取 $\infty$——这恰好是 probability simplex（概率单纯形）的 indicator function（示性函数）。你正在学的概率论（Bertsekas）里，$n$ 个互斥结局的离散概率分布正是单纯形上的一个点：$y_i \ge 0$ 且 $\sum_i y_i = 1$。换句话说，对 max 求共轭，得到的是『概率分布全体』这个集合本身——凸分析把概率对象编码成了集合。解答里那个 (why?) 现在可以补上：$y^T x = \sum_i y_i x_i$ 是以 $y_i$ 为权的加权平均，而加权平均永远不会超过最大可能值 $\max_i x_i$，故 $y^T x \le \max_i x_i$，且 $x = 0$ 时两边同为 $0$。再往前看一步：把 max 换成它的光滑近似 log-sum-exp，其共轭是定义在单纯形上的负熵 $\sum_i y_i \log y_i$，而 log-sum-exp 的梯度正是 softmax（归一化指数函数）——机器学习里用 softmax 把得分向量变成概率分布，其凸分析根源就在这条共轭链上。
