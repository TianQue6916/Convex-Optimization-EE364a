# EE364a HW1 解答（Homework 1 Solutions）

> 课程：EE364a Convex Optimization（凸优化），Winter 2007-08，Prof. S. Boyd
> 本文为官方解答的**双语译注 + 完整推导**版。

---

## 题目 1（Problem 2.1）

**题干：** Let $C \subseteq \mathbb{R}^n$ be a convex set（凸集）, with $x_1, \dots, x_k \in C$, and let $\theta_1, \dots, \theta_k \in \mathbb{R}$ satisfy $\theta_i \ge 0$, $\theta_1 + \cdots + \theta_k = 1$. Show that $\theta_1 x_1 + \cdots + \theta_k x_k \in C$. (The definition of convexity is that this holds for $k = 2$; you must show it for arbitrary $k$.) *Hint.* Use induction on $k$.

**中文：** 设 $C \subseteq \mathbb{R}^n$ 是一个凸集（convex set），$x_1, \dots, x_k \in C$，且实数 $\theta_1, \dots, \theta_k$ 满足 $\theta_i \ge 0$、$\theta_1 + \cdots + \theta_k = 1$。证明 $\theta_1 x_1 + \cdots + \theta_k x_k \in C$。（凸性的定义只要求 $k=2$ 的情形成立；本题要求你对任意 $k$ 证明。）*提示：* 对 $k$ 用归纳法。

**解答：**

本题就是凸集定义从「两点凸组合」推广到「有限点凸组合」的标准归纳证明。核心技巧是：把 $k$ 个点的凸组合，改写成 $x_1$ 与「其余 $k-1$ 个点的一个凸组合」这两者的凸组合，从而能套用 $k=2$ 的定义。原书以 $k=3$ 为例说明思路，一般情形留给读者；这里我们把一般情形也补完整。

**情形 $k=3$（原书示例）：** 设 $x_1, x_2, x_3 \in C$，且 $\theta_1 + \theta_2 + \theta_3 = 1$，$\theta_1, \theta_2, \theta_3 \ge 0$。我们要证明

```math
y = \theta_1 x_1 + \theta_2 x_2 + \theta_3 x_3 \in C.
```

至少有一个 $\theta_i$ 不等于 1（因为三个非负数之和为 1，若三者全等于 1 则和必为 3，矛盾）。不失一般性（without loss of generality）设 $\theta_1 \neq 1$。则 $1 - \theta_1 \neq 0$，我们可以把 $y$ 写成

```math
y = \theta_1 x_1 + (1 - \theta_1)\left(\mu_2 x_2 + \mu_3 x_3\right),
```

其中

```math
\mu_2 = \frac{\theta_2}{1 - \theta_1}, \qquad \mu_3 = \frac{\theta_3}{1 - \theta_1}.
```

（注意原书此处有一处笔误，写成 $\mu_2 = \theta_3/(1-\theta_1)$，实际上第二个系数应为 $\mu_3 = \theta_3/(1-\theta_1)$，〔原书此处笔误：第二个定义应为 $\mu_3$，待核〕。）

下面逐步验证：

1. 由 $\theta_2 \ge 0$ 且 $1 - \theta_1 > 0$，得 $\mu_2 \ge 0$；同理 $\mu_3 \ge 0$。
2. 二者之和为

```math
\mu_2 + \mu_3 = \frac{\theta_2}{1-\theta_1} + \frac{\theta_3}{1-\theta_1} = \frac{\theta_2 + \theta_3}{1-\theta_1} = \frac{1 - \theta_1}{1-\theta_1} = 1.
```

所以 $\mu_2 x_2 + \mu_3 x_3$ 是 $x_2, x_3$ 的凸组合。因为 $C$ 是凸集且 $x_2, x_3 \in C$，由凸集定义（$k=2$ 情形）得 $\mu_2 x_2 + \mu_3 x_3 \in C$。又该点与 $x_1$ 都在 $C$ 中，且 $\theta_1 + (1-\theta_1) = 1$、$\theta_1 \ge 0$、$1-\theta_1 \ge 0$，再次由凸集定义得 $y \in C$。

**一般情形（归纳证明，补全）：** 对 $k$ 做数学归纳。

- **基础**：$k=1$ 时，$\theta_1 = 1$，$\theta_1 x_1 = x_1 \in C$，成立；$k=2$ 即凸集定义本身。
- **归纳假设**：假设对任意 $k-1$ 个 $C$ 中点的凸组合，其结果仍在 $C$ 中。
- **归纳步骤**：设 $x_1, \dots, x_k \in C$，$\theta_i \ge 0$，$\sum_{i=1}^k \theta_i = 1$。若某 $\theta_j = 1$，则其余全为 0，结论平凡。否则，取任意 $\theta_1 \neq 1$（可重排下标使其成立，因 $\sum \theta_i = 1$ 且各 $\theta_i \ge 0$，不可能所有 $\theta_i = 1$）。定义

```math
\mu_i = \frac{\theta_i}{1-\theta_1}, \quad i = 2, \dots, k.
```

则 $\mu_i \ge 0$，且

```math
\sum_{i=2}^k \mu_i = \frac{\sum_{i=2}^k \theta_i}{1-\theta_1} = \frac{1-\theta_1}{1-\theta_1} = 1.
```

由归纳假设，$\sum_{i=2}^k \mu_i x_i \in C$。而

```math
\sum_{i=1}^k \theta_i x_i = \theta_1 x_1 + (1-\theta_1)\underbrace{\sum_{i=2}^k \mu_i x_i}_{\in\, C},
```

这是 $C$ 中两个点的凸组合，故仍在 $C$ 中。归纳完成，结论对任意 $k$ 成立。∎

---

## 题目 2（Problem 2.2）

**题干：** Show that a set is convex if and only if its intersection with any line is convex. Show that a set is affine if and only if its intersection with any line is affine.

**中文：** 证明：一个集合是凸集（convex set），当且仅当它与任意直线的交集都是凸集。证明：一个集合是仿射集（affine set），当且仅当它与任意直线的交集都是仿射集。

**解答：**

**第一部分（凸集）：**

- **（⇒）** 两个凸集的交集仍是凸集。因此若 $S$ 是凸集，则 $S$ 与一条直线（直线本身是凸集，因为直线上的凸组合仍在直线上）的交集是凸集。

  > 补一句「直线是凸集」的理由：直线 $\ell = \{x_0 + t v \mid t \in \mathbb{R}\}$ 上两点 $x_0 + t_1 v$、$x_0 + t_2 v$ 的凸组合为 $x_0 + (\theta t_1 + (1-\theta)t_2)v$，仍是直线上的点。

- **（⇐）** 反之，设 $S$ 与任意直线的交集都是凸集。取 $S$ 中任意两个不同点 $x_1, x_2 \in S$。考虑过 $x_1, x_2$ 的直线，$S$ 与该直线的交集是凸集（由假设）。因此 $x_1$ 与 $x_2$ 的任意凸组合都属于该交集，从而也属于 $S$。即对任意 $x_1, x_2 \in S$、$\theta \in [0,1]$，有 $\theta x_1 + (1-\theta)x_2 \in S$，这正是凸集的定义。∎

**第二部分（仿射集）：** 仿射集的定义是「对任意 $x_1, x_2 \in S$ 与任意 $\theta \in \mathbb{R}$（注意这里 $\theta$ 可取全体实数，不限于 $[0,1]$），$\theta x_1 + (1-\theta)x_2 \in S$」。证明与第一部分完全平行：

- **（⇒）** 两个仿射集的交集仍是仿射集，而直线是仿射集，故凸方向（affine 方向）的「$S$ 与任意直线之交是仿射集」成立。
- **（⇐）** 设 $S$ 与任意直线的交集是仿射集。取 $x_1, x_2 \in S$，过这两点的直线与 $S$ 的交是仿射集，故 $x_1, x_2$ 的任意仿射组合（$\theta \in \mathbb{R}$）都在其中，从而在 $S$ 中。∎

---

## 题目 3（Problem 2.5）

**题干：** What is the distance between two parallel hyperplanes（超平面） $\{x \in \mathbb{R}^n \mid a^T x = b_1\}$ and $\{x \in \mathbb{R}^n \mid a^T x = b_2\}$?

**中文：** 两个平行超平面 $\{x \in \mathbb{R}^n \mid a^T x = b_1\}$ 与 $\{x \in \mathbb{R}^n \mid a^T x = b_2\}$ 之间的距离是多少？

**解答：**

这两个超平面之间的距离是

```math
d = \frac{|b_1 - b_2|}{\|a\|_2}.
```

推导如下。关键观察：两个平行超平面之间的距离，等于它们与「过原点且平行于法向量 $a$ 的直线」的交点之间的距离。设这两个交点分别为 $x_1, x_2$，它们满足

```math
x_1 = \frac{b_1}{\|a\|_2^2} a, \qquad x_2 = \frac{b_2}{\|a\|_2^2} a.
```

（验证：$a^T x_1 = \dfrac{b_1}{\|a\|_2^2} a^T a = b_1$，故 $x_1$ 确在第一个超平面上；$x_2$ 同理。）

于是距离为

```math
\|x_1 - x_2\|_2 = \left\|\frac{b_1 - b_2}{\|a\|_2^2} a\right\|_2 = \frac{|b_1 - b_2|}{\|a\|_2^2} \|a\|_2 = \frac{|b_1 - b_2|}{\|a\|_2}.
```

**几何解释：** 沿法向量 $a$ 方向，两个超平面的「偏移量」之差为 $b_1 - b_2$；单位法向量 $\hat a = a / \|a\|_2$ 上，超平面 $a^T x = b$ 与原点距离为 $|b|/\|a\|_2$。二者相减即得间距 $|b_1 - b_2|/\|a\|_2$。

[[FIGURE: 第 1–2 页，示意图 —— 法向量 $a$，两个平行超平面 $a^T x = b_1$ 与 $a^T x = b_2$，以及它们在法向量方向上的交点 $x_1 = (b_1/\|a\|_2^2)a$、$x_2 = (b_2/\|a\|_2^2)a$]]

---

## 题目 4（Problem 2.7）

**题干：** Voronoi description of halfspace. Let $a$ and $b$ be distinct points in $\mathbb{R}^n$. Show that the set of all points that are closer (in Euclidean norm) to $a$ than $b$, i.e., $\{x \mid \|x - a\|_2 \le \|x - b\|_2\}$, is a halfspace（半空间）. Describe it explicitly as an inequality of the form $c^T x \le d$. Draw a picture.

**中文：** 半空间的 Voronoi（Voronoi）描述。设 $a$ 与 $b$ 是 $\mathbb{R}^n$ 中两个不同点。证明「到 $a$ 比到 $b$ 更近（按欧氏范数）的所有点的集合」，即 $\{x \mid \|x - a\|_2 \le \|x - b\|_2\}$，是一个半空间。并把它显式写成形如 $c^T x \le d$ 的不等式。画图。

**解答：**

因为范数总是非负的，所以 $\|x - a\|_2 \le \|x - b\|_2$ 当且仅当两边平方后 $\|x - a\|_2^2 \le \|x - b\|_2^2$ 成立（这一步用到「非负数上平方是保序的」）。于是逐行等价变换：

```math
\|x - a\|_2^2 \le \|x - b\|_2^2
```

```math
\Longleftrightarrow (x - a)^T (x - a) \le (x - b)^T (x - b)
```

展开左边与右边：

```math
x^T x - 2a^T x + a^T a \le x^T x - 2b^T x + b^T b
```

两边消去 $x^T x$：

```math
-2a^T x + a^T a \le -2b^T x + b^T b
```

移项：

```math
2(b - a)^T x \le b^T b - a^T a.
```

因此该集合确实是半空间。我们可取

```math
c = 2(b - a), \qquad d = b^T b - a^T a,
```

即半空间为 $\{x \mid c^T x \le d\}$。

**几何意义：** 到 $a$、$b$ 等距的点构成一个超平面，其法向量沿 $b - a$ 方向（即 $a$ 指向 $b$ 的方向），该超平面垂直平分线段 $ab$；而「更靠近 $a$」的那一侧就是上述半空间。

[[FIGURE: 第 2 页附近，示意图 —— 两点 $a$、$b$ 与垂直平分超平面，法向量沿 $b-a$ 方向，半空间为靠近 $a$ 的一侧]]

---

## 题目 5（Problem 2.8）

**题干：** Which of the following sets $S$ are polyhedra（多面体）? If possible, express $S$ in the form $S = \{x \mid Ax \preceq b, Fx = g\}$.

(a) $S = \{y_1 a_1 + y_2 a_2 \mid -1 \le y_1 \le 1,\ -1 \le y_2 \le 1\}$，其中 $a_1, a_2 \in \mathbb{R}^n$。

(b) $S = \{x \in \mathbb{R}^n \mid x \succeq 0,\ 1^T x = 1,\ \sum_{i=1}^n x_i a_i = b_1,\ \sum_{i=1}^n x_i a_i^2 = b_2\}$，其中 $a_1, \dots, a_n \in \mathbb{R}$，$b_1, b_2 \in \mathbb{R}$。

(c) $S = \{x \in \mathbb{R}^n \mid x \succeq 0,\ x^T y \le 1\ \text{对所有满足}\ \|y\|_2 = 1\ \text{的}\ y\}$。

(d) $S = \{x \in \mathbb{R}^n \mid x \succeq 0,\ x^T y \le 1\ \text{对所有满足}\ \sum_{i=1}^n |y_i| = 1\ \text{的}\ y\}$。

**中文：** 下列集合 $S$ 中，哪些是多面体（polyhedron）？如果可能，把 $S$ 表成形如 $S = \{x \mid Ax \preceq b, Fx = g\}$ 的形式。

**解答：**

多面体的定义：能表示成有限多个半空间（线性不等式）与超平面（线性等式）交集的集合，即 $\{x \mid Ax \preceq b, Fx = g\}$。

**（a）$S$ 是多面体。** 它是角点为 $a_1 + a_2$、$a_1 - a_2$、$-a_1 + a_2$、$-a_1 - a_2$ 的平行四边形（parallelogram）。为简单起见，设 $a_1$ 与 $a_2$ 线性无关。$S$ 可表为三个集合的交集：

- $S_1$：由 $a_1, a_2$ 张成的平面（plane）。
- $S_2 = \{z + y_1 a_1 + y_2 a_2 \mid a_1^T z = a_2^T z = 0,\ -1 \le y_1 \le 1\}$：一个平行于 $a_2$、垂直于 $S_1$ 的「板」（slab）。
- $S_3 = \{z + y_1 a_1 + y_2 a_2 \mid a_1^T z = a_2^T z = 0,\ -1 \le y_2 \le 1\}$：一个平行于 $a_1$、垂直于 $S_1$ 的板。

下面说明每个集合都能用线性不等式描述。

- $S_1$ 可描述为

```math
v_k^T x = 0, \quad k = 1, \dots, n-2,
```

其中 $v_k$ 是 $n-2$ 个与 $a_1$、$a_2$ 都正交的线性无关向量（它们构成矩阵 $[a_1\ a_2]^T$ 零空间（nullspace）的一组基）。写成线性不等式即为

```math
v_k^T x \le 0,\quad -v_k^T x \le 0, \quad k = 1, \dots, n-2.
```

- 设 $c_1$ 是 $a_1, a_2$ 所张平面内的、与 $a_2$ 正交的向量。例如取

```math
c_1 = a_1 - \frac{a_1^T a_2}{\|a_2\|_2^2} a_2.
```

（这是把 $a_1$ 对 $a_2$ 做 Gram–Schmidt 正交化的结果，因此 $a_2^T c_1 = 0$。）则 $x \in S_2$ 当且仅当

```math
-|c_1^T a_1| \le c_1^T x \le |c_1^T a_1|.
```

- 类似地，设 $c_2$ 是 $a_1, a_2$ 所张平面内的、与 $a_1$ 正交的向量，例如

```math
c_2 = a_2 - \frac{a_2^T a_1}{\|a_1\|_2^2} a_1.
```

则 $x \in S_3$ 当且仅当

```math
-|c_2^T a_2| \le c_2^T x \le |c_2^T a_2|.
```

汇总起来，$S$ 是下列 $2n$ 个线性不等式的解集：

```math
\begin{aligned}
v_k^T x &\le 0, & k &= 1, \dots, n-2 \\
-v_k^T x &\le 0, & k &= 1, \dots, n-2 \\
c_1^T x &\le |c_1^T a_1| \\
-c_1^T x &\le |c_1^T a_1| \\
c_2^T x &\le |c_2^T a_2| \\
-c_2^T x &\le |c_2^T a_2|.
\end{aligned}
```

（共 $(n-2) + (n-2) + 1 + 1 + 1 + 1 = 2n$ 个不等式。）故 $S$ 是多面体。

[[FIGURE: 第 3 页，示例图 —— $\mathbb{R}^2$ 中由 $a_1, a_2$ 张成的平行四边形及其角点，标注向量 $a_1, a_2, c_1, c_2$]]

**（b）$S$ 是多面体。** 它由线性不等式 $x_k \ge 0$（即 $x \succeq 0$）与三个线性等式约束（$1^T x = 1$、$\sum_i x_i a_i = b_1$、$\sum_i x_i a_i^2 = b_2$）定义。这正是 $Ax \preceq b, Fx = g$ 的标准形式，故为多面体。注意：约束中不涉及 $x$ 的非线性项——$a_i$、$a_i^2$ 是给定的常数，$\sum_i x_i a_i$ 与 $\sum_i x_i a_i^2$ 对 $x$ 都是线性的。

**（c）$S$ 不是多面体。** 它其实是单位球（unit ball）$\{x \mid \|x\|_2 \le 1\}$ 与非负卦限（nonnegative orthant）$\mathbb{R}_+^n$ 的交集。这由下面的事实（可由 Cauchy–Schwarz 不等式推出）得到：

```math
x^T y \le 1\ \text{对所有}\ \|y\|_2 = 1\ \text{的}\ y \quad\Longleftrightarrow\quad \|x\|_2 \le 1.
```

推导：若 $\|x\|_2 \le 1$，则由 Cauchy–Schwarz 不等式 $x^T y \le \|x\|_2 \|y\|_2 = \|x\|_2 \le 1$；反之，若 $x^T y \le 1$ 对所有单位向量 $y$ 成立，取 $y = x / \|x\|_2$（当 $x \neq 0$），则 $x^T y = \|x\|_2 \le 1$。

虽然这里 $S$ 被写成无穷多个半空间的交集，但它**不是**多面体——因为多面体的定义要求**有限**个半空间，而单位球需要无穷多个半空间（每个支撑超平面一个）才能刻画。

**（d）$S$ 是多面体。** $S$ 是集合 $\{x \mid |x_k| \le 1, k = 1, \dots, n\}$ 与非负卦限 $\mathbb{R}_+^n$ 的交集。这由下面的事实得到：

```math
x^T y \le 1\ \text{对所有}\ \sum_{i=1}^n |y_i| = 1\ \text{的}\ y \quad\Longleftrightarrow\quad |x_i| \le 1,\ i = 1, \dots, n.
```

证明如下：

- **（⇒）** 先设对所有 $i$ 有 $|x_i| \le 1$。则对任意满足 $\sum_i |y_i| = 1$ 的 $y$，

```math
x^T y = \sum_i x_i y_i \le \sum_i |x_i| |y_i| \le \sum_i |y_i| = 1.
```

（第一处不等式用 $x_i y_i \le |x_i||y_i|$，第二处用 $|x_i| \le 1$。）

- **（⇐）** 反之，设非零向量 $x$ 满足「对所有 $\sum_i |y_i| = 1$ 的 $y$，$x^T y \le 1$」。取 $k$ 为满足 $|x_k| = \max_i |x_i|$ 的下标，构造 $y$：若 $x_k > 0$ 令 $y_k = 1$，若 $x_k < 0$ 令 $y_k = -1$，对其余 $i \neq k$ 令 $y_i = 0$。此时 $\sum_i |y_i| = 1$，且

```math
x^T y = \sum_i x_i y_i = y_k x_k = |x_k| = \max_i |x_i|.
```

由假设 $x^T y \le 1$，故必须有 $\max_i |x_i| \le 1$，即对所有 $i$ 有 $|x_i| \le 1$。

综上，$S$ 可由有限个线性不等式描述：它是非负卦限与集合 $\{x \mid -1 \preceq x \preceq 1\}$ 的交集，即 $2n$ 个线性不等式

```math
\begin{aligned}
-x_i &\le 0, & i &= 1, \dots, n \\
x_i &\le 1, & i &= 1, \dots, n
\end{aligned}
```

的解集。注意：与 (c) 类似，$S$ 原本也是写成无穷多个半空间的交集；区别在于这里绝大多数线性不等式是**冗余的**（redundant），只需有限多个就能刻画 $S$。

**补充说明：** 这些集合都不是仿射集（affine set）或子空间（subspace），除非某些平凡情形。例如 (a) 中集合当 $a_1 = a_2 = 0$ 时是子空间（从而是仿射集）；(b) 中集合当 $n = 1$ 且 $S = \{1\}$ 时是仿射集；等等。

---

## 题目 6（Problem 2.11）

**题干：** Hyperbolic sets. Show that the hyperbolic set $\{x \in \mathbb{R}_+^2 \mid x_1 x_2 \ge 1\}$ is convex. As a generalization, show that $\{x \in \mathbb{R}_+^n \mid \prod_{i=1}^n x_i \ge 1\}$ is convex. *Hint.* If $a, b \ge 0$ and $0 \le \theta \le 1$, then $a^\theta b^{1-\theta} \le \theta a + (1-\theta)b$; see §3.1.9.

**中文：** 双曲集合（hyperbolic set）。证明双曲集合 $\{x \in \mathbb{R}_+^2 \mid x_1 x_2 \ge 1\}$ 是凸集。作为推广，证明 $\{x \in \mathbb{R}_+^n \mid \prod_{i=1}^n x_i \ge 1\}$ 是凸集。*提示：* 若 $a, b \ge 0$ 且 $0 \le \theta \le 1$，则 $a^\theta b^{1-\theta} \le \theta a + (1-\theta)b$；见 §3.1.9。

**解答：**

**（a）二维情形（不用提示直接证）：** 取集合中两点 $(x_1, x_2)$ 与 $(y_1, y_2)$ 的一个凸组合 $z$，即 $z = \theta x + (1-\theta)y$，其中 $0 \le \theta \le 1$。

- 若 $x \succeq y$（即 $x_1 \ge y_1$ 且 $x_2 \ge y_2$），则 $z = \theta x + (1-\theta)y \succeq y$，从而显然

```math
z_1 z_2 \ge y_1 y_2 \ge 1.
```

$y \succeq x$ 的情形同理。

- 若 $y \not\succeq x$ 且 $x \not\succeq y$，即 $(y_1 - x_1)(y_2 - x_2) < 0$（两个坐标分量一个增大一个减小，乘积为负）。此时计算

```math
\begin{aligned}
(\theta x_1 + (1-\theta)y_1)(\theta x_2 + (1-\theta)y_2)
&= \theta^2 x_1 x_2 + (1-\theta)^2 y_1 y_2 + \theta(1-\theta)x_1 y_2 + \theta(1-\theta)x_2 y_1 \\
&= \theta x_1 x_2 + (1-\theta)y_1 y_2 - \theta(1-\theta)(y_1 - x_1)(y_2 - x_2) \\
&\ge 1.
\end{aligned}
```

最后一步用到：$x_1 x_2 \ge 1$、$y_1 y_2 \ge 1$，故 $\theta x_1 x_2 + (1-\theta)y_1 y_2 \ge \theta + (1-\theta) = 1$；且 $-\theta(1-\theta)(y_1-x_1)(y_2-x_2) > 0$（因为 $(y_1-x_1)(y_2-x_2) < 0$），故整个式子 $\ge 1$。

（验证中间那个展开等式的关键一步：$\theta^2 x_1 x_2 + (1-\theta)^2 y_1 y_2 + \theta(1-\theta)(x_1 y_2 + x_2 y_1) = \theta x_1 x_2 + (1-\theta) y_1 y_2 - \theta(1-\theta)(y_1-x_1)(y_2-x_2)$，左右两边对 $x_1 x_2$、$y_1 y_2$、交叉项分别相等即可核对。）

因此集合是凸集。∎

**（b）一般 $n$ 维情形（用提示）：** 设 $\prod_i x_i \ge 1$ 且 $\prod_i y_i \ge 1$。用提示中的不等式（这是「加权算术–几何平均不等式」，即 $a^\theta b^{1-\theta} \le \theta a + (1-\theta)b$），对每一维 $i$ 取 $a = x_i$、$b = y_i$，得

```math
\theta x_i + (1-\theta)y_i \ge x_i^\theta\, y_i^{1-\theta}.
```

对 $i$ 连乘：

```math
\prod_i \left(\theta x_i + (1-\theta)y_i\right) \ge \prod_i x_i^\theta y_i^{1-\theta} = \left(\prod_i x_i\right)^{\theta} \left(\prod_i y_i\right)^{1-\theta} \ge 1^\theta \cdot 1^{1-\theta} = 1.
```

（倒数第二步：$\prod_i x_i^\theta y_i^{1-\theta} = \left(\prod_i x_i\right)^{\theta}\left(\prod_i y_i\right)^{1-\theta}$，因为 $\prod_i x_i^\theta = \left(\prod_i x_i\right)^\theta$；再由 $\prod_i x_i \ge 1$、$\prod_i y_i \ge 1$ 得整体 $\ge 1$。）

故 $n$ 维双曲集合也是凸集。∎

---

## 题目 7（Problem 2.12）

**题干：** Which of the following sets are convex?

(a) A slab（板），即 $\{x \in \mathbb{R}^n \mid \alpha \le a^T x \le \beta\}$。

(b) A rectangle（矩形），即 $\{x \in \mathbb{R}^n \mid \alpha_i \le x_i \le \beta_i,\ i = 1, \dots, n\}$。当 $n > 2$ 时矩形也称超矩形（hyperrectangle）。

(c) A wedge（楔），即 $\{x \in \mathbb{R}^n \mid a_1^T x \le b_1,\ a_2^T x \le b_2\}$。

(d) 到给定点比到给定集合更近的点的集合，即 $\{x \mid \|x - x_0\|_2 \le \|x - y\|_2\ \text{对所有}\ y \in S\}$，其中 $S \subseteq \mathbb{R}^n$。

(e) 到一个集合比到另一个集合更近的点的集合，即 $\{x \mid \operatorname{dist}(x, S) \le \operatorname{dist}(x, T)\}$，其中 $S, T \subseteq \mathbb{R}^n$，$\operatorname{dist}(x, S) = \inf\{\|x - z\|_2 \mid z \in S\}$。

(f) 集合 $\{x \mid x + S_2 \subseteq S_1\}$，其中 $S_1, S_2 \subseteq \mathbb{R}^n$ 且 $S_1$ 凸。

(g) 到 $a$ 的距离不超过到 $b$ 距离的固定比例 $\theta$ 的点的集合，即 $\{x \mid \|x - a\|_2 \le \theta \|x - b\|_2\}$。可设 $a \neq b$ 且 $0 \le \theta \le 1$。

**中文：** 下列集合哪些是凸集（convex set）？

**解答：**

**（a）板（slab）是凸集。** 它是两个半空间的交集：$\{x \mid \alpha \le a^T x \le \beta\} = \{x \mid a^T x \le \beta\} \cap \{x \mid -a^T x \le -\alpha\}$。半空间是凸集，凸集的有限交集仍是凸集，因此板是凸集，且是多面体（polyhedron）。

**（b）矩形（rectangle）是凸集。** 与 (a) 同理，它是 $2n$ 个半空间 $\{x \mid x_i \le \beta_i\}$、$\{x \mid -x_i \le -\alpha_i\}$ 的有限交集，故是凸集且是多面体。

**（c）楔（wedge）是凸集。** 它是两个半空间的交集，故是凸集且是多面体。此外，当 $b_1 = 0$ 且 $b_2 = 0$ 时它是锥（cone，即对非负缩放封闭）。

**（d）该集合是凸集。** 因为它可表为

```math
\bigcap_{y \in S} \{x \mid \|x - x_0\|_2 \le \|x - y\|_2\},
```

即一族半空间的交集（回顾题 2.7：对固定的 $y$，集合 $\{x \mid \|x - x_0\|_2 \le \|x - y\|_2\}$ 是半空间）。半空间的（任意，包括无穷）交集仍是凸集。

**（e）一般情况下该集合不是凸集。** 反例如下（在 $\mathbb{R}$ 中）：取 $S = \{-1, 1\}$、$T = \{0\}$。则

```math
\{x \mid \operatorname{dist}(x, S) \le \operatorname{dist}(x, T)\} = \{x \in \mathbb{R} \mid x \le -\tfrac12\ \text{或}\ x \ge \tfrac12\}.
```

（验证：当 $|x| \ge \tfrac12$ 时，$x$ 到 $\{-1,1\}$ 中较近那点的距离 $\le$ 到 $0$ 的距离 $|x|$；具体地，$x \ge 1/2$ 时 $\operatorname{dist}(x,S) = x-1 \le x = \operatorname{dist}(x,T)$ 不对，应重算——到 $1$ 的距离为 $x-1$，到 $0$ 的距离为 $x$，$x-1 \le x$ 恒成立，故 $x \ge 1/2$ 显然满足；而 $1/2 \le x < 1$ 时到 $1$ 的距离 $1-x$ 与到 $0$ 的距离 $x$ 相比，$1-x \le x \iff x \ge 1/2$，成立。对称地 $x \le -1/2$ 也满足。）该集合是两个区间的并集 $(-\infty, -1/2] \cup [1/2, \infty)$，显然不是凸集（例如 $-1$ 与 $1$ 都在集合中，但它们的凸组合 $0$ 不在其中）。

**（f）该集合是凸集。** 注意 $x + S_2 \subseteq S_1$ 等价于「对所有 $y \in S_2$，$x + y \in S_1$」。因此

```math
\{x \mid x + S_2 \subseteq S_1\} = \bigcap_{y \in S_2} \{x \mid x + y \in S_1\} = \bigcap_{y \in S_2} (S_1 - y),
```

其中 $S_1 - y = \{s - y \mid s \in S_1\}$ 是凸集 $S_1$ 的平移，仍是凸集；凸集的交集仍是凸集。故该集合凸。

**（g）该集合是凸集，实际上是一个球（ball）。** 推导：

```math
\{x \mid \|x - a\|_2 \le \theta \|x - b\|_2\} = \{x \mid \|x - a\|_2^2 \le \theta^2 \|x - b\|_2^2\}
```

展开：

```math
\|x - a\|_2^2 - \theta^2 \|x - b\|_2^2 \le 0
```

```math
\Longleftrightarrow (1 - \theta^2) x^T x - 2(a - \theta^2 b)^T x + (a^T a - \theta^2 b^T b) \le 0.
```

- 若 $\theta = 1$，上式是一次不等式（$x^T x$ 项系数 $1 - \theta^2 = 0$），故是半空间。
- 若 $\theta < 1$，则 $1 - \theta^2 > 0$，它是二次不等式且二次项系数为正，对应一个球

```math
\{x \mid (x - x_0)^T (x - x_0) \le R^2\},
```

球心 $x_0$ 与半径 $R$ 由下式给出（配方即得）：

```math
x_0 = \frac{a - \theta^2 b}{1 - \theta^2},
```

```math
R = \left(\frac{\theta^2 \|b\|_2^2 - \|a\|_2^2}{1 - \theta^2} + \|x_0\|_2^2\right)^{1/2}.
```

（配方法：把 $(1-\theta^2)x^T x - 2(a-\theta^2 b)^T x + (a^T a - \theta^2 b^T b) \le 0$ 除以 $1-\theta^2$ 后配方为 $\|x - x_0\|_2^2 \le R^2$，其中 $x_0 = (a-\theta^2 b)/(1-\theta^2)$，常数项整理即得 $R^2$ 的表达式。）球是凸集。∎

---

## 题目 8（Problem 2.15）

**题干：** Some sets of probability distributions. Let $x$ be a real-valued random variable（随机变量）with $\operatorname{prob}(x = a_i) = p_i$, $i = 1, \dots, n$, where $a_1 < a_2 < \cdots < a_n$. Of course $p \in \mathbb{R}^n$ lies in the standard probability simplex（概率单纯形）$P = \{p \mid 1^T p = 1, p \succeq 0\}$. Which of the following conditions are convex in $p$? (That is, for which of the following conditions is the set of $p \in P$ that satisfy the condition convex?)

(a) $\alpha \le \mathbb{E} f(x) \le \beta$，其中 $\mathbb{E} f(x)$ 是 $f(x)$ 的期望值，即 $\mathbb{E} f(x) = \sum_{i=1}^n p_i f(a_i)$。（函数 $f : \mathbb{R} \to \mathbb{R}$ 给定。）

(b) $\operatorname{prob}(x > \alpha) \le \beta$。

(c) $\mathbb{E}|x^3| \le \alpha \,\mathbb{E}|x|$。

(d) $\mathbb{E} x^2 \le \alpha$。

(e) $\mathbb{E} x^2 \ge \alpha$。

(f) $\operatorname{var}(x) \le \alpha$，其中 $\operatorname{var}(x) = \mathbb{E}(x - \mathbb{E}x)^2$ 是 $x$ 的方差（variance）。

(g) $\operatorname{var}(x) \ge \alpha$。

(h) $\operatorname{quartile}(x) \ge \alpha$，其中 $\operatorname{quartile}(x) = \inf\{\beta \mid \operatorname{prob}(x \le \beta) \ge 0.25\}$（四分位数，quartile）。

(i) $\operatorname{quartile}(x) \le \alpha$。

**中文：** 一些概率分布构成的集合。设 $x$ 是实值随机变量（random variable），$\operatorname{prob}(x = a_i) = p_i$，$i = 1, \dots, n$，其中 $a_1 < a_2 < \cdots < a_n$。显然 $p \in \mathbb{R}^n$ 落在标准概率单纯形（probability simplex）$P = \{p \mid 1^T p = 1, p \succeq 0\}$ 中。下列哪些条件关于 $p$ 是凸的？（即：对下列哪些条件，满足该条件的 $p \in P$ 所组成的集合是凸集？）

**解答：**

先指出：约束 $p_i \ge 0$（$i = 1, \dots, n$）定义半空间，$\sum_{i=1}^n p_i = 1$ 定义超平面，故 $P$ 是多面体。判断某个约束关于 $p$ 是否凸，关键是看它能否写成 $p$ 的线性不等式（凸）、还是涉及 $p$ 的二次/非凸项。

**（a）** $\mathbb{E} f(x) = \sum_{i=1}^n p_i f(a_i)$，所以该约束等价于两个线性不等式

```math
\alpha \le \sum_{i=1}^n p_i f(a_i) \le \beta.
```

线性不等式定义凸集（半空间），故该约束关于 $p$ 是凸的。

**（b）** $\operatorname{prob}(x \ge \alpha) = \sum_{i : a_i \ge \alpha} p_i$（注意原题写的是 $x > \alpha$，解答按 $\ge$ 处理，因为离散分布下二者在特定 $\alpha$ 上可能一致，〔原书此处 $>$ 与 $\ge$ 的细微差别不影响凸性结论，待核〕），所以该约束等价于线性不等式

```math
\sum_{i : a_i \ge \alpha} p_i \le \beta.
```

故是凸的。

**（c）** 该约束等价于线性不等式

```math
\sum_{i=1}^n p_i\left(|a_i^3| - \alpha |a_i|\right) \le 0.
```

（因为 $\mathbb{E}|x^3| = \sum_i p_i |a_i^3|$，$\mathbb{E}|x| = \sum_i p_i |a_i|$，移项即得。）故是凸的。

**（d）** 该约束等价于线性不等式

```math
\sum_{i=1}^n p_i a_i^2 \le \alpha.
```

故是凸的。

**（e）** 该约束等价于线性不等式

```math
\sum_{i=1}^n p_i a_i^2 \ge \alpha.
```

故是凸的。

综上，前五个约束都定义凸集。

**（f）** 约束

```math
\operatorname{var}(x) = \mathbb{E}x^2 - (\mathbb{E}x)^2 = \sum_{i=1}^n p_i a_i^2 - \left(\sum_{i=1}^n p_i a_i\right)^2 \le \alpha
```

一般情况下**不是**凸的。因为 $-(\sum_i p_i a_i)^2$ 是 $p$ 的凹函数（负的平方项），整个方差是「凸项 $\sum p_i a_i^2$ 减凸项 $(\sum p_i a_i)^2$」，是凸减凸，结果未必凸。

反例：取 $n = 2$，$a_1 = 0$，$a_2 = 1$，$\alpha = 1/5$。则

- $p = (1, 0)$：$\mathbb{E}x^2 = 0$，$\mathbb{E}x = 0$，$\operatorname{var} = 0 \le 1/5$，满足。
- $p = (0, 1)$：$\mathbb{E}x^2 = 1$，$\mathbb{E}x = 1$，$\operatorname{var} = 1 - 1 = 0 \le 1/5$，满足。

但它们的凸组合 $p = (1/2, 1/2)$：$\mathbb{E}x^2 = 1/2$，$\mathbb{E}x = 1/2$，$\operatorname{var} = 1/2 - 1/4 = 1/4 > 1/5$，**不满足**。故该约束非凸。

**（g）** 该约束等价于

```math
\sum_{i=1}^n a_i^2 p_i - \left(\sum_{i=1}^n a_i p_i\right)^2 = b^T p - p^T A p \le \alpha,
```

其中 $b_i = a_i^2$ 且 $A = a a^T$（即 $A_{ij} = a_i a_j$，为对称半正定矩阵）。注意这里因为 $(\sum_i a_i p_i)^2 = p^T (a a^T) p = p^T A p$，而 $A$ 是半正定（positive semidefinite）的，所以 $-p^T A p$ 是凹函数，$b^T p - p^T A p$ 是「线性减凸」= 凹函数。凹函数的「下水平集」$\{p \mid \text{凹函数} \le \alpha\}$ 是凸集，故该约束定义凸集。

（严格说：$\operatorname{var}(x) \ge \alpha \iff \sum a_i^2 p_i - p^T A p \ge \alpha$，即 $-(\sum a_i^2 p_i - p^T A p) \le -\alpha$，左边 $p^T A p - b^T p$ 是凸函数（$A$ 半正定），故是凸函数的下水平集，凸。）

下面处理四分位数。记 $f(p) = \operatorname{quartile}(x)$，强调它是 $p$ 的函数。图说明了定义：它展示了分布 $p$ 的累积分布函数（CDF），当 $f(p) = a_2$ 时的情形。

[[FIGURE: 第 8 页，示意图 —— 概率分布 $p$ 的累积分布函数（CDF）曲线，横轴为 $\beta$，纵轴为 $\operatorname{prob}(x \le \beta)$，在 $a_1, a_2, \dots, a_n$ 处跳跃（阶梯），水平线 0.25 标注四分位点]]

**（h）** 约束 $f(p) \ge \alpha$ 等价于

```math
\operatorname{prob}(x \le \beta) < 0.25\ \text{对所有}\ \beta < \alpha.
```

- 若 $\alpha \le a_1$，则恒成立（因为最小的取值就是 $a_1$，对 $\beta < a_1$，$\operatorname{prob}(x \le \beta) = 0 < 0.25$）。
- 否则，定义 $k = \max\{i \mid a_i < \alpha\}$。这是固定的整数，与 $p$ 无关（只由 $\alpha$ 和 $a_i$ 决定）。约束 $f(p) \ge \alpha$ 成立当且仅当

```math
\operatorname{prob}(x \le a_k) = \sum_{i=1}^k p_i < 0.25.
```

这是 $p$ 的严格线性不等式，定义开半空间（open halfspace）。开半空间是凸集。故该约束凸。

**（i）** 约束 $f(p) \le \alpha$ 等价于

```math
\operatorname{prob}(x \le \beta) \ge 0.25\ \text{对所有}\ \beta \ge \alpha.
```

定义 $k = \max\{i \mid a_i \le \alpha\}$（同样是固定的整数，与 $p$ 无关）。约束 $f(p) \le \alpha$ 成立当且仅当

```math
\operatorname{prob}(x \le a_k) = \sum_{i=1}^k p_i \ge 0.25.
```

这是 $p$ 的（非严格）线性不等式，定义半空间，故是凸集。

若 $\alpha \le a_1$，则没有 $p$ 满足 $f(p) \le \alpha$（因为四分位数至少是 $a_1$），即集合为空集。空集按定义也是凸集。

**小结：** 前五项（a–e）为线性约束、凸；(f) 非凸；(g) 凸；(h) 凸（开半空间）；(i) 凸（半空间或空集）。故除 (f) 外，其余所有约束关于 $p$ 都是凸的。
