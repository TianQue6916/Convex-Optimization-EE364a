> <span style="color:#7f8c8d;">Source: 作业解答/HW2 解答.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

<!-- p.1 -->

# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

## <span style="color:#2471a3;">**[lecture]**</span> <span style="color:#c0392b;">**EE364a Homework 2 solutions（EE364a 作业 2 解答）**</span>

> <span style="color:#7f8c8d;">EE364a, Winter 2007-08 · Prof. S. Boyd（EE364a 课程 · 2007–08 冬季学期 · S. Boyd 教授）</span>

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**2.28 Positive semidefinite cone for n = 1, 2, 3（n = 1, 2, 3 的半正定锥）**</span>

<span style="color:#2471a3;">**[question] Problem（题目）:**</span>

Positive semidefinite cone for $n = 1, 2, 3$. Give an explicit description of the positive semidefinite cone $\mathcal{S}^n_+$, in terms of the matrix coefficients and ordinary inequalities, for $n = 1, 2, 3$. To describe a general element of $\mathcal{S}^n$, for $n = 1, 2, 3$, use the notation

```math
x_1, \qquad \begin{bmatrix} x_1 & x_2 \\ x_2 & x_3 \end{bmatrix}, \qquad \begin{bmatrix} x_1 & x_2 & x_3 \\ x_2 & x_4 & x_5 \\ x_3 & x_5 & x_6 \end{bmatrix}.
```

<span style="color:#2471a3;">**[solution] Solution（解答）:**</span>

> $n = 1$ 时条件为 $x_1 \ge 0$。$n = 2$ 时条件为
>
> $x_1 \ge 0, \qquad x_3 \ge 0, \qquad x_1 x_3 - x_2^2 \ge 0$
>
> $n = 3$ 时条件为
>
> $x_1 \ge 0, \quad x_4 \ge 0, \quad x_6 \ge 0, \quad x_1 x_4 - x_2^2 \ge 0, \quad x_4 x_6 - x_5^2 \ge 0, \quad x_1 x_6 - x_3^2 \ge 0$
>
> 以及
>
> $x_1 x_4 x_6 + 2 x_2 x_3 x_5 - x_1 x_5^2 - x_6 x_2^2 - x_4 x_3^2 \ge 0$
>
> <span style="color:#00838f;">**即全部 principal minors（主子式）都必须非负。**</span>
>
> 我们在假设 $n = 2$ 情形结论成立的前提下，给出 $n = 3$ 情形的证明。矩阵
>
> $X = \begin{bmatrix} x_1 & x_2 & x_3 \\ x_2 & x_4 & x_5 \\ x_3 & x_5 & x_6 \end{bmatrix}$
>
> 为 positive semidefinite（半正定），当且仅当
>
> $z^T X z = x_1 z_1^2 + 2 x_2 z_1 z_2 + 2 x_3 z_1 z_3 + x_4 z_2^2 + 2 x_5 z_2 z_3 + x_6 z_3^2 \ge 0$
>
> 对所有 $z$ 成立。
>
> 若 $x_1 = 0$，则必须有 $x_2 = x_3 = 0$，于是 $X \succeq 0$ 当且仅当
>
> $\begin{bmatrix} x_4 & x_5 \\ x_5 & x_6 \end{bmatrix} \succeq 0$
>
> 应用 $2 \times 2$ 情形的结果，我们得出：若 $x_1 = 0$，则 $X \succeq 0$ 当且仅当
>
> $x_2 = x_3 = 0, \qquad x_4 \ge 0, \qquad x_6 \ge 0, \qquad x_4 x_6 - x_5^2 \ge 0$
>
> 现在假设 $x_1 \ne 0$。我们有
>
> $z^T X z = x_1\left(z_1 + (x_2/x_1) z_2 + (x_3/x_1) z_3\right)^2 + \left(x_4 - x_2^2/x_1\right) z_2^2 + \left(x_6 - x_3^2/x_1\right) z_3^2 + 2\left(x_5 - x_2 x_3/x_1\right) z_2 z_3$
>
> <!-- p.2 -->
>
> 因此显然必须有 $x_1 > 0$，且
>
> $\begin{bmatrix} x_4 - x_2^2/x_1 & x_5 - x_2 x_3/x_1 \\ x_5 - x_2 x_3/x_1 & x_6 - x_3^2/x_1 \end{bmatrix} \succeq 0$
>
> 由上面研究过的 $2 \times 2$ 情形的结果，这等价于
>
> $x_1 x_4 - x_2^2 \ge 0, \qquad x_1 x_6 - x_3^2 \ge 0, \qquad \left(x_4 - x_2^2/x_1\right)\left(x_6 - x_3^2/x_1\right) - \left(x_5 - x_2 x_3/x_1\right)^2 \ge 0$
>
> 第三个不等式化简为
>
> $\left(x_1 x_4 x_6 + 2 x_2 x_3 x_5 - x_1 x_5^2 - x_6 x_2^2 - x_4 x_3^2\right)/x_1 \ge 0$
>
> 因此，若 $x_1 > 0$，则 $X \succeq 0$ 当且仅当
>
> $x_1 x_4 - x_2^2 \ge 0, \qquad x_1 x_6 - x_3^2 \ge 0, \qquad \left(x_1 x_4 x_6 + 2 x_2 x_3 x_5 - x_1 x_5^2 - x_6 x_2^2 - x_4 x_3^2\right)/x_1 \ge 0$
>
> 我们可以把 $x_1 = 0$ 与 $x_1 > 0$ 两种情形的条件合并起来表述为：全部 7 个主子式都必须非负。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这道题做的事，是把「矩阵半正定」这一定性条件翻译成系数级别的普通不等式。$2\times 2$ 情形的 $x_1x_3 - x_2^2 \ge 0$ 你应该似曾相识：它正是 Strang 线代中对称矩阵正定性判据（Sylvester 准则：顺序主子式非负）的二维版本。$3\times 3$ 情形新增的三次不等式恰好是 $\det X$ 的展开式 $x_1x_4x_6 + 2x_2x_3x_5 - x_1x_5^2 - x_6x_2^2 - x_4x_3^2$。证明里「分 $x_1 = 0$ 与 $x_1 > 0$ 讨论、消元后看剩余 $2\times 2$ 块」的写法，是 Schur complement（舒尔补）判定半正定的雏形，与 Strang 课程中「正定矩阵的每个主子矩阵都正定」互为印证。再往深一层：全部主子式非负刻画的是锥 $\mathcal{S}^n_+$，它是 semidefinite program（半定规划，SDP）可行集的骨架。课程后续的图划分、统计估计、控制等问题，最终都要回到「这个锥里的元素到底长什么样」这类具体刻画上。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**2.33 The monotone nonnegative cone（单调非负锥）**</span>

<span style="color:#2471a3;">**[question] Problem（题目）:**</span>

The monotone nonnegative cone. We define the monotone nonnegative cone as

```math
K_{m+} = \left\{ x \in \mathbf{R}^n \mid x_1 \ge x_2 \ge \cdots \ge x_n \ge 0 \right\},
```

i.e., all nonnegative vectors with components sorted in nonincreasing order.

(a) Show that $K_{m+}$ is a proper cone.

(b) Find the dual cone $K_{m+}^*$. Hint. Use the identity

```math
\sum_{i=1}^{n} x_i y_i = (x_1 - x_2)y_1 + (x_2 - x_3)(y_1 + y_2) + (x_3 - x_4)(y_1 + y_2 + y_3) + \cdots + (x_{n-1} - x_n)(y_1 + \cdots + y_{n-1}) + x_n (y_1 + \cdots + y_n).
```

<span style="color:#2471a3;">**[solution] Solution（解答）:**</span>

> **(a)** 集合 $K_{m+}$ 由 $n$ 个齐次线性不等式定义，因此它是一个闭的 polyhedral cone（多面体锥）。
>
> $K_{m+}$ 的 interior（内部）非空，因为存在使这些不等式全部严格成立的点，例如 $x = (n,\, n-1,\, n-2,\, \ldots,\, 1)$。
>
> 为证明 $K_{m+}$ 是 pointed（尖的），我们注意：若 $x \in K_{m+}$，则仅当 $x = 0$ 时才有 $-x \in K_{m+}$。这说明该锥不包含任何一整条直线。
>
> **(b)** 利用提示中的恒等式，我们看到：$y^T x \ge 0$ 对所有 $x \in K_{m+}$ 成立，当且仅当
>
> $y_1 \ge 0, \quad y_1 + y_2 \ge 0, \quad \ldots, \quad y_1 + y_2 + \cdots + y_n \ge 0$
>
> <span style="color:#00838f;">**因此，dual cone（对偶锥）为**</span>
>
> $K_{m+}^* = \left\{\, y \;\middle|\; \sum_{i=1}^{k} y_i \ge 0,\ k = 1, \ldots, n \,\right\}$

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 提示中的恒等式是分部求和（Abel 求和）：它把「分量逐点比较」$x_1 \ge x_2 \ge \cdots \ge x_n \ge 0$ 改写成「相邻差分非负」$x_i - x_{i+1} \ge 0$。它的威力在于让对偶锥的条件从「对一切 $x$ 验证 $y^T x \ge 0$」坍缩成 $n$ 个前缀和不等式 $y_1 + \cdots + y_k \ge 0$，计算量从无限降到有限。另外注意 (a) 中三个条件的分工：封闭性来自不等式定义，实心性（内部非空）来自严格可行点的存在，尖性来自「正负同在只能为零」——任何 proper cone（正常锥）的验证都是这三步。这套「交换求和顺序」的技巧你在概率论（Bertsekas）里见过同源版本：非负随机变量的期望可写成尾部概率积分 $\mathbb{E}[X] = \int_0^\infty \mathbb{P}(X > t)\,dt$，证明用的正是同一招。此外，$K_{m+}^*$ 由有限个线性不等式给出，说明它是多面体锥——这与 6.006 里线性规划（linear program, LP）可行集的多面体结构、以及网络流中「前缀流量不超过前缀容量」式约束完全是同一类对象。

<!-- p.3 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**3.2 Level sets of convex, concave, quasiconvex, and quasiconcave functions（凸、凹、拟凸与拟凹函数的水平集）**</span>

<span style="color:#2471a3;">**[question] Problem（题目）:**</span>

Level sets of convex, concave, quasiconvex, and quasiconcave functions. Some level sets of a function $f$ are shown below. The curve labeled 1 shows $\{x \mid f(x) = 1\}$, etc.

Could $f$ be convex (concave, quasiconvex, quasiconcave)? Explain your answer. Repeat for the level curves shown below.

> <span style="color:#7f8c8d;">（原书此处及下页配有两幅等高线示意图：第一幅为标号 1、2、3 的三条闭合曲线，第二幅为标号 1–6 的等高线；插图中另有虚线 I、II 与黑点标记，图从略。）</span>

<span style="color:#2471a3;">**[solution] Solution（解答）:**</span>

> 第一个函数可能是 quasiconvex（拟凸）的，因为其 sublevel sets（下水平集）看起来是凸的。它肯定不是 concave（凹）或 quasiconcave（拟凹）的，因为其 superlevel sets（上水平集）不是凸集。
>
> 它也不是凸的（convex），理由如下。我们沿标号为 I 的虚线画出函数值。
>
> <!-- p.4 -->
>
> 沿这条直线，函数经过下图中以黑点标记的各个点。显然，在这一线段上该函数不是凸的。
>
> 若对第二个函数重复同样的分析，我们看到它可能是凹的（因此也可能是拟凹的）。它不可能是凸的或拟凸的，因为其下水平集不是凸集。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 凸函数要求下水平集是凸集，而且要求「弯度」处处朝同一方向；拟凸只要求下水平集是凸集。等价地说，拟凸函数沿任何直线的截线都是单峰的：先不增、后不减。只保留「单峰」这一个性质，代价是失去梯度与 Hessian 的整套二阶工具。但换来一个算法红利：在一维上最小化可微拟凸函数时，可以像 6.006 的二分搜索那样，检查中点处次梯度的符号就砍掉一半搜索区间；对一般非凸函数这样做会被局部极小值骗住。本题的图形判读（先看下水平集凸不凸、再看上水平集）也是建模时快速筛查目标函数性质的标准动作。一旦确认拟凸，就可以用二分法套一层凸可行性判定来求解，这正是教材第 4 章处理拟凸优化的正式思路。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**3.5 Running average of a convex function（凸函数的累积平均）**</span>

<span style="color:#2471a3;">**[question] Problem（题目）:**</span>

Running average of a convex function. Suppose $f : \mathbf{R} \to \mathbf{R}$ is convex, with $\mathbf{R}_+ \subseteq \mathbf{dom} f$. Show that its running average $F$, defined as

```math
F(x) = \frac{1}{x}\int_0^x f(t)\,dt, \qquad \mathbf{dom} F = \mathbf{R}_{++},
```

is convex. You can assume $f$ is differentiable.

<span style="color:#2471a3;">**[solution] Solution（解答）:**</span>

> $F$ 是可微的，且
>
> $F'(x) = -\left(1/x^2\right)\int_0^x f(t)\,dt + f(x)/x$
>
> <!-- p.5 -->
>
> $F''(x) = \left(2/x^3\right)\int_0^x f(t)\,dt - 2f(x)/x^2 + f'(x)/x = \left(2/x^3\right)\int_0^x \left(f(t) - f(x) - f'(x)(t - x)\right)\,dt$
>
> 凸性由如下事实立得：对所有 $x, t \in \mathbf{dom} f$，
>
> $f(t) \ge f(x) + f'(x)(t - x)$
>
> 成立，而这蕴含 $F''(x) \ge 0$。
>
> 下面是另一个（更简单的？）证明。对每个 $s$，函数 $f(sx)$ 作为 $x$ 的函数是凸的。因此
>
> $\int_0^1 f(sx)\,ds$
>
> 是 $x$ 的凸函数。现在作变量替换 $t = sx$，得到
>
> $\int_0^1 f(sx)\,ds = \frac{1}{x}\int_0^x f(t)\,dt$

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**3.6 Functions and epigraphs（函数与上图）**</span>

<span style="color:#2471a3;">**[question] Problem（题目）:**</span>

Functions and epigraphs. When is the epigraph of a function a halfspace? When is the epigraph of a function a convex cone? When is the epigraph of a function a polyhedron?

<span style="color:#2471a3;">**[solution] Solution（解答）:**</span>

> <span style="color:#00838f;">**当该函数是凸函数（convex function），并且依次为 affine（仿射）、positively homogeneous（正齐次，即对 $\alpha \ge 0$ 有 $f(\alpha x) = \alpha f(x)$）与 piecewise-affine（分段仿射）时，其 epigraph（上图）依次为 halfspace（半空间）、convex cone（凸锥）与 polyhedron（多面体）。**</span>

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**3.15 A family of concave utility functions（一族凹效用函数）**</span>

<span style="color:#2471a3;">**[question] Problem（题目）:**</span>

A family of concave utility functions. For $0 < \alpha \le 1$ let

```math
u_\alpha(x) = \frac{x^\alpha - 1}{\alpha},
```

with $\mathbf{dom}\, u_\alpha = \mathbf{R}_+$. We also define $u_0(x) = \log x$ (with $\mathbf{dom}\, u_0 = \mathbf{R}_{++}$).

(a) Show that for $x > 0$, $u_0(x) = \lim_{\alpha \to 0} u_\alpha(x)$.

(b) Show that $u_\alpha$ are concave, monotone increasing, and all satisfy $u_\alpha(1) = 0$.

These functions are often used in economics to model the benefit or utility of some quantity of goods or money. Concavity of $u_\alpha$ means that the marginal utility (i.e., the increase in utility obtained for a fixed increase in the goods) decreases as the amount of goods increases. In other words, concavity models the effect of satiation.

<span style="color:#2471a3;">**[solution] Solution（解答）:**</span>

> **(a)** 在这个极限中，分子与分母都趋于零，所以我们使用 l'Hôpital's rule（洛必达法则）：
>
> $\lim_{\alpha \to 0} u_\alpha(x) = \lim_{\alpha \to 0} \frac{(d/d\alpha)\left(x^\alpha - 1\right)}{(d/d\alpha)\,\alpha} = \lim_{\alpha \to 0} \frac{x^\alpha \log x}{1} = \log x$
>
> <!-- p.6 -->
>
> **(b)** 直接观察可得
>
> $u_\alpha(1) = \frac{1^\alpha - 1}{\alpha} = 0$
>
> 导数为
>
> $u_\alpha'(x) = x^{\alpha - 1}$
>
> 它对一切 $x$ 为正（因为 $0 < \alpha < 1$），所以这些函数是递增的。为证明凹性，我们考察二阶导数：
>
> $u_\alpha''(x) = (\alpha - 1)\,x^{\alpha - 2}$
>
> 由于它对一切 $x$ 为负，我们得出 $u_\alpha$ 是 strictly concave（严格凹）的。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**3.16 Convex, concave, quasiconvex, or quasiconcave?（凸、凹、拟凸还是拟凹？）**</span>

<span style="color:#2471a3;">**[question] Problem（题目）:**</span>

For each of the following functions determine whether it is convex, concave, quasiconvex, or quasiconcave.

(b) $f(x_1, x_2) = x_1 x_2$ on $\mathbf{R}_{++}^2$.

<span style="color:#2471a3;">**[solution] Solution（解答）:**</span>

> $f$ 的 Hessian（海森矩阵）为
>
> $\nabla^2 f(x) = \begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix}$
>
> 它既不是半正定的也不是半负定的。因此 $f$ 既不是凸的也不是凹的。它是拟凹的，因为其上水平集
>
> $\left\{ (x_1, x_2) \in \mathbf{R}_{++}^2 \mid x_1 x_2 \ge \alpha \right\}$
>
> 是凸集。它不是拟凸的。

<span style="color:#2471a3;">**[question] Problem（题目）:**</span>

(c) $f(x_1, x_2) = 1/(x_1 x_2)$ on $\mathbf{R}_{++}^2$.

<span style="color:#2471a3;">**[solution] Solution（解答）:**</span>

> $f$ 的 Hessian 为
>
> $\nabla^2 f(x) = \frac{1}{x_1 x_2}\begin{bmatrix} 2/x_1^2 & 1/(x_1 x_2) \\ 1/(x_1 x_2) & 2/x_2^2 \end{bmatrix} \succeq 0$
>
> 因此 $f$ 是凸的、拟凸的。它不是拟凹的或凹的。

<span style="color:#2471a3;">**[question] Problem（题目）:**</span>

(d) $f(x_1, x_2) = x_1 / x_2$ on $\mathbf{R}_{++}^2$.

<span style="color:#2471a3;">**[solution] Solution（解答）:**</span>

> $f$ 的 Hessian 为
>
> $\nabla^2 f(x) = \begin{bmatrix} 0 & -1/x_2^2 \\ -1/x_2^2 & 2x_1/x_2^3 \end{bmatrix}$
>
> 它不是半正定或半负定的。因此 $f$ 不是凸的或凹的。它是拟凸且拟凹的（即 quasilinear（拟线性）），因为其下水平集与上水平集都是半空间。

<!-- p.7 -->

<span style="color:#2471a3;">**[question] Problem（题目）:**</span>

(e) $f(x_1, x_2) = x_1^2 / x_2$ on $\mathbf{R} \times \mathbf{R}_{++}$.

<span style="color:#2471a3;">**[solution] Solution（解答）:**</span>

> $f$ 是凸的，如教材第 72 页所述（另见图 3.3）。通过算出 Hessian 很容易验证这一点：
>
> $\nabla^2 f(x) = \begin{bmatrix} 2/x_2 & -2x_1/x_2^2 \\ -2x_1/x_2^2 & 2x_1^2/x_2^3 \end{bmatrix} = \left(2/x_2\right)\begin{bmatrix} 1 \\ -x_1/x_2 \end{bmatrix}\begin{bmatrix} 1 & -x_1/x_2 \end{bmatrix} \succeq 0$
>
> 因此 $f$ 是凸的、拟凸的。它不是凹的或拟凹的（见该图）。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**3.18 Adapting the concavity proof of the log-determinant function（改编对数行列式函数的凹性证明）**</span>

<span style="color:#2471a3;">**[question] Problem（题目）:**</span>

Adapt the proof of concavity of the log-determinant function in §3.1.5 to show the following.

(b) $f(X) = (\det X)^{1/n}$ is concave on $\mathbf{dom} f = \mathcal{S}^n_{++}$.

<span style="color:#2471a3;">**[solution] Solution（解答）:**</span>

> **(b)** 定义 $g(t) = f(Z + tV)$，其中 $Z \succ 0$，$V \in \mathcal{S}^n$。
>
> $g(t) = \left(\det(Z + tV)\right)^{1/n} = \left(\det Z^{1/2}\,\det\left(I + tZ^{-1/2}VZ^{-1/2}\right)\,\det Z^{1/2}\right)^{1/n} = (\det Z)^{1/n}\left(\prod_{i=1}^{n}\left(1 + t\lambda_i\right)\right)^{1/n}$
>
> 其中 $\lambda_i$（$i = 1, \ldots, n$）是 $Z^{-1/2}VZ^{-1/2}$ 的 eigenvalues（特征值）。由最后一个等式可见，$g$ 在 $\{\, t \mid Z + tV \succ 0 \,\}$ 上是 $t$ 的凹函数，因为 $\det Z > 0$，且 geometric mean（几何均值）$\left(\prod_{i=1}^{n} x_i\right)^{1/n}$ 在 $\mathbf{R}_{++}^n$ 上是凹的。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 3.18 的证明把矩阵问题一路化归到标量：先用 $\det Z^{1/2}\,\det(I + tZ^{-1/2}VZ^{-1/2})\,\det Z^{1/2}$ 做标准化，再读出 $(1 + t\lambda_i)$ 的乘积。关键一步是把任意扰动 $V$ 吸收进 $Z^{-1/2}VZ^{-1/2}$ 的谱里，剩下的只需标量工具。在 Strang 线代里 $\det Z$ 就是全部特征值的乘积，所以 $(\det X)^{1/n}$ 恰是 $X$ 的 $n$ 个特征值的几何均值。而几何均值的凹性来自算术–几何均值不等式（AM–GM），它是「乘积结构 + 对称性 ⇒ 凹性」的典型样本，你在 18.065 讨论特征值与行列式时已经用过这个恒等式。这条「特征值 → 行列式 → 凹性」的链路，后面会以 $\log \det$ 的形式在内点法的 barrier function（障碍函数）与中心路径分析中再次出场：$\log\det$ 的凹性正是保证对数障碍函数为凸的关键。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**3.24 Some functions on the probability simplex（概率单纯形上的一些函数）**</span>

<span style="color:#2471a3;">**[question] Problem（题目）:**</span>

Some functions on the probability simplex. Let $x$ be a real-valued random variable which takes values in $\{a_1, \ldots, a_n\}$ where $a_1 < a_2 < \cdots < a_n$, with $\mathrm{prob}(x = a_i) = p_i$, $i = 1, \ldots, n$. For each of the following functions of $p$ (on the probability simplex $\{p \in \mathbf{R}_+^n \mid \mathbf{1}^T p = 1\}$), determine if the function is convex, concave, quasiconvex, or quasiconcave.

(f) $\mathrm{quartile}(x) = \inf\{\beta \mid \mathrm{prob}(x \le \beta) \ge 0.25\}$.

<span style="color:#2471a3;">**[solution] Solution（解答）:**</span>

> quartile（四分位数）的下水平集与上水平集都是凸的（见习题 2.15），所以它是拟凸且拟凹的。quartile(x) 不是连续的（它在一个离散集合 $\{a_1, \ldots, a_n\}$ 中取值），所以它不是凸的或凹的。（凸函数或凹函数在其定义域的 relative interior（相对内部）上总是连续的。）

<span style="color:#2471a3;">**[question] Problem（题目）:**</span>

(g) The cardinality of the smallest set $A \subseteq \{a_1, \ldots, a_n\}$ with probability $\ge 90\%$. (By cardinality we mean the number of elements in $A$.)

<span style="color:#2471a3;">**[solution] Solution（解答）:**</span>

> $f$ 取整数值，所以它不可能是凸的或凹的。（凸函数或凹函数在其定义域的相对内部上总是连续的。）
>
> <!-- p.8 -->
>
> $f$ 是拟凹的，因为其上水平集是凸的。我们有 $f(p) \ge \alpha$ 当且仅当
>
> $\sum_{i=1}^{k} p_{[i]} < 0.9$
>
> 其中 $k = \max\{i = 1, \ldots, n \mid i < \alpha\}$ 是小于 $\alpha$ 的最大整数，而 $p_{[i]}$ 是 $p$ 中第 $i$ 大的分量。我们知道 $\sum_{i=1}^{k} p_{[i]}$ 是 $p$ 的凸函数，所以不等式 $\sum_{i=1}^{k} p_{[i]} < 0.9$ 定义了一个凸集。
>
> 一般地，$f(p)$ 不是拟凸的。例如，取 $n = 2$，$a_1 = 0$，$a_2 = 1$，并取 $p_1 = (0.1, 0.9)$，$p_2 = (0.9, 0.1)$。则 $f(p_1) = f(p_2) = 1$，但 $f((p_1 + p_2)/2) = f(0.5, 0.5) = 2$。

<span style="color:#2471a3;">**[question] Problem（题目）:**</span>

(h) The minimum width interval that contains 90% of the probability, i.e.,

```math
\inf\left\{\, \beta - \alpha \;\middle|\; \mathrm{prob}(\alpha \le x \le \beta) \ge 0.9 \,\right\}.
```

<span style="color:#2471a3;">**[solution] Solution（解答）:**</span>

> 概率含量达到 90% 的最小宽度区间必然形如 $[a_i, a_j]$，其中 $1 \le i \le j \le n$，因为
>
> $\mathrm{prob}(\alpha \le x \le \beta) = \sum_{k=i}^{j} p_k = \mathrm{prob}(a_i \le x \le a_j)$
>
> 其中 $i = \min\{k \mid a_k \ge \alpha\}$，$j = \max\{k \mid a_k \le \beta\}$。
>
> 我们证明该函数是拟凹的。我们有 $f(p) \ge \gamma$ 当且仅当所有宽度小于 $\gamma$ 的区间的概率都小于 90%：对所有满足 $a_j - a_i < \gamma$ 的 $i, j$，
>
> $\sum_{k=i}^{j} p_k < 0.9$
>
> 这定义了一个凸集。
>
> 由于该函数在一个有限集合上取值，它不连续，因此既不是凸的也不是凹的。此外，它一般也不是拟凸的。考虑 $n = 2$，$a_1 = 0$，$a_2 = 1$，$p_1 = (0.95, 0.05)$，$p_2 = (0.05, 0.95)$ 的例子。则 $f(p_1) = 0$，$f(p_2) = 0$，但 $f((p_1 + p_2)/2) = 1$。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**3.36 Derive the conjugates of the following functions（求下列函数的共轭函数）**</span>

<span style="color:#2471a3;">**[question] Problem（题目）:**</span>

Derive the conjugates of the following functions.

(a) Max function. $f(x) = \max_{i=1,\ldots,n} x_i$ on $\mathbf{R}^n$.

<span style="color:#2471a3;">**[solution] Solution（解答）:**</span>

> <span style="color:#00838f;">**我们将证明：$f^*(y) = 0$，若 $y \succeq 0$ 且 $\mathbf{1}^T y = 1$；否则 $f^*(y) = \infty$。**</span>
>
> 我们先验证 $f^*$ 的定义域。首先假设 $y$ 有一个负分量，比如 $y_k < 0$。如果选取向量 $x$，取 $x_k = -t$，$x_i = 0$（$i \ne k$），并让 $t \to \infty$，我们看到
>
> <!-- p.9 -->
>
> $x^T y - \max_i x_i = -t\,y_k \to \infty$
>
> 所以 $y$ 不属于 $\mathbf{dom} f^*$。接下来，假设 $y \succeq 0$ 但 $\mathbf{1}^T y > 1$。我们取 $x = t\mathbf{1}$ 并让 $t \to \infty$，由此可证
>
> $x^T y - \max_i x_i = t\,\mathbf{1}^T y - t$
>
> 无上界。类似地，当 $y \succeq 0$ 且 $\mathbf{1}^T y < 1$ 时，我们取 $x = -t\mathbf{1}$ 并让 $t \to \infty$。
>
> $y$ 剩下的情形是 $y \succeq 0$ 且 $\mathbf{1}^T y = 1$。此时
>
> $x^T y \le \max_i x_i$
>
> 对所有 $x$ 成立，因此 $x^T y - \max_i x_i \le 0$ 对所有 $x$ 成立，并且在 $x = 0$ 处取等号。所以 $f^*(y) = 0$。

<span style="color:#2471a3;">**[question] Problem（题目）:**</span>

(d) Power function. $f(x) = x^p$ on $\mathbf{R}_{++}$, where $p > 1$. Repeat for $p < 0$.

<span style="color:#2471a3;">**[solution] Solution（解答）:**</span>

> 我们采用标准记号：用方程 $1/p + 1/q = 1$ 定义 $q$，即 $q = p/(p - 1)$。
>
> 从 $p > 1$ 的情形开始。此时 $x^p$ 在 $\mathbf{R}_+$ 上是严格凸的。对 $y < 0$，函数 $yx - x^p$ 在 $x > 0$ 上的最大值于 $x = 0$ 处取得，所以 $f^*(y) = 0$。对 $y > 0$，该函数在 $x = (y/p)^{1/(p-1)}$ 处取得最大值，其值为
>
> $y\,(y/p)^{1/(p-1)} - (y/p)^{p/(p-1)} = (p - 1)\,(y/p)^q$
>
> 因此
>
> $f^*(y) = \begin{cases} 0 & y \le 0 \\ (p - 1)(y/p)^q & y > 0 \end{cases}$
>
> 对 $p < 0$，类似的论证表明 $\mathbf{dom} f^* = -\mathbf{R}_+$，且
>
> $f^*(y) = \frac{p}{q}\left(y/p\right)^q$

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> conjugate function（共轭函数）可以看作 Legendre 变换在非光滑世界的推广：分析力学里从拉格朗日量切换到哈密顿量用的正是 $\sup_x\,(yx - f(x))$ 这一结构，凸分析把它推广到任意凸函数并保留了整套对偶理论。验证共轭的标准动作是三步：先找使 $yx - f(x)$ 最大的 $x$（一阶条件），再代回求值，最后单独检查取不到最大值的方向并令 $f^* = \infty$——本题 (a) 对定义域的逐段验证就是这第三步。(a) 中「$y \succeq 0$ 且 $\mathbf{1}^T y = 1$」的条件并非凭空而来：$y$ 是概率向量时，$x^T y$ 是各分量 $x_i$ 的凸组合，天然被 $\max_i x_i$ 压住。(d) 的配对指数 $1/p + 1/q = 1$ 正是 Hölder 不等式的共轭指数，也是 $p$-范数与 $q$-范数互为 dual norm（对偶范数）的同一套代数——Strang 18.065 里从范数角度看 SVD 几何时用的就是这层对偶。共轭函数是下一章拉格朗日对偶性的代数引擎，值得现在就把这几对函数练熟。
