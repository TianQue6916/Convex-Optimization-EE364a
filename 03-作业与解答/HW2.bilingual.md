# EE364a HW2 解答（Homework 2 Solutions）

## 题目 2.28

**题干：** Positive semidefinite cone for n = 1, 2, 3. Give an explicit description of the positive semidefinite cone $S^n_+$, in terms of the matrix coefficients and ordinary inequalities, for $n = 1, 2, 3$. To describe a general element of $S^n$, for $n = 1, 2, 3$, use the notation
```math
x_1, \quad \begin{bmatrix} x_1 & x_2 \\ x_2 & x_3 \end{bmatrix}, \quad \begin{bmatrix} x_1 & x_2 & x_3 \\ x_2 & x_4 & x_5 \\ x_3 & x_5 & x_6 \end{bmatrix}.
```

**中文：** 对 $n = 1, 2, 3$，给出正半定锥（positive semidefinite cone）$S^n_+$ 的显式刻画，用矩阵系数和普通不等式表示。对 $n = 1, 2, 3$，一般元素 $S^n$ 用上面的记号表示。

**解答：**

对 $n = 1$，条件就是
```math
x_1 \ge 0.
```

对 $n = 2$，条件为
```math
x_1 \ge 0, \quad x_3 \ge 0, \quad x_1x_3 - x_2^2 \ge 0.
```

对 $n = 3$，条件为
```math
x_1 \ge 0, \quad x_4 \ge 0, \quad x_6 \ge 0,
```
```math
x_1x_4 - x_2^2 \ge 0, \quad x_4x_6 - x_5^2 \ge 0, \quad x_1x_6 - x_3^2 \ge 0,
```
以及
```math
x_1x_4x_6 + 2x_2x_3x_5 - x_1x_5^2 - x_6x_2^2 - x_4x_3^2 \ge 0,
```
即，所有主子式（principal minors）都必须非负。

下面给出 $n = 3$ 的证明（假设 $n = 2$ 的结论已成立）。矩阵
```math
X = \begin{bmatrix} x_1 & x_2 & x_3 \\ x_2 & x_4 & x_5 \\ x_3 & x_5 & x_6 \end{bmatrix}
```
是正半定的（positive semidefinite），当且仅当对所有 $z$，有
```math
z^T X z = x_1 z_1^2 + 2x_2 z_1 z_2 + 2x_3 z_1 z_3 + x_4 z_2^2 + 2x_5 z_2 z_3 + x_6 z_3^2 \ge 0.
```

若 $x_1 = 0$，则必有 $x_2 = x_3 = 0$（否则取 $z_1$ 与 $z_2$、$z_3$ 适当符号即可使二次型为负），于是 $X \succeq 0$ 当且仅当
```math
\begin{bmatrix} x_4 & x_5 \\ x_5 & x_6 \end{bmatrix} \succeq 0.
```
应用 $2 \times 2$ 情形的结论，得到：当 $x_1 = 0$ 时，$X \succeq 0$ 当且仅当
```math
x_2 = x_3 = 0, \quad x_4 \ge 0, \quad x_6 \ge 0, \quad x_4x_6 - x_5^2 \ge 0.
```

现在设 $x_1 \ne 0$。用配方法（completing the square）把二次型写成
```math
z^T X z = x_1\Big(z_1 + \frac{x_2}{x_1}z_2 + \frac{x_3}{x_1}z_3\Big)^2 + \Big(x_4 - \frac{x_2^2}{x_1}\Big)z_2^2 + \Big(x_6 - \frac{x_3^2}{x_1}\Big)z_3^2 + 2\Big(x_5 - \frac{x_2x_3}{x_1}\Big)z_2 z_3,
```
于是显然必须有 $x_1 > 0$，且
```math
\begin{bmatrix} x_4 - \frac{x_2^2}{x_1} & x_5 - \frac{x_2x_3}{x_1} \\ x_5 - \frac{x_2x_3}{x_1} & x_6 - \frac{x_3^2}{x_1} \end{bmatrix} \succeq 0.
```
（理由：平方项系数 $x_1$ 必须非负且不能为 0，否则让其余 $z$ 分量为 0、$z_1$ 取 0 会使二次型为 0 而 $x_1<0$ 时取负；而 $x_1>0$ 时余下的 $2\times2$ 块对任意 $z_2,z_3$ 必须保持非负。）

由上面的 $2 \times 2$ 情形结论，这等价于
```math
x_1x_4 - x_2^2 \ge 0, \quad x_1x_6 - x_3^2 \ge 0,
```
```math
\Big(x_4 - \frac{x_2^2}{x_1}\Big)\Big(x_6 - \frac{x_3^2}{x_1}\Big) - \Big(x_5 - \frac{x_2x_3}{x_1}\Big)^2 \ge 0.
```

第三个不等式展开化简：先通分
```math
\Big(x_4 - \frac{x_2^2}{x_1}\Big)\Big(x_6 - \frac{x_3^2}{x_1}\Big) - \Big(x_5 - \frac{x_2x_3}{x_1}\Big)^2
= \frac{(x_1x_4 - x_2^2)(x_1x_6 - x_3^2) - (x_1x_5 - x_2x_3)^2}{x_1^2}
```
```math
= \frac{x_1x_4x_6 + 2x_2x_3x_5 - x_1x_5^2 - x_6x_2^2 - x_4x_3^2}{x_1} \ge 0.
```

因此，当 $x_1 > 0$ 时，$X \succeq 0$ 当且仅当
```math
x_1x_4 - x_2^2 \ge 0, \quad x_1x_6 - x_3^2 \ge 0,
```
```math
\frac{x_1x_4x_6 + 2x_2x_3x_5 - x_1x_5^2 - x_6x_2^2 - x_4x_3^2}{x_1} \ge 0.
```

把 $x_1 = 0$ 与 $x_1 > 0$ 两种情形合并，结论可以统一表述为：**所有 7 个主子式都必须非负。**

---

## 题目 2.33

**题干：** The monotone nonnegative cone. We define the monotone nonnegative cone as
```math
K_{m+} = \{x \in \mathbf{R}^n \mid x_1 \ge x_2 \ge \cdots \ge x_n \ge 0\},
```
i.e., all nonnegative vectors with components sorted in nonincreasing order.
(a) Show that $K_{m+}$ is a proper cone.
(b) Find the dual cone $K^*_{m+}$. Hint. Use the identity
```math
\sum_{i=1}^{n} x_i y_i = (x_1-x_2)y_1 + (x_2-x_3)(y_1+y_2) + (x_3-x_4)(y_1+y_2+y_3) + \cdots + (x_{n-1}-x_n)(y_1+\cdots+y_{n-1}) + x_n(y_1+\cdots+y_n).
```

**中文：** 单调非负锥（monotone nonnegative cone）。定义
```math
K_{m+} = \{x \in \mathbf{R}^n \mid x_1 \ge x_2 \ge \cdots \ge x_n \ge 0\},
```
即所有分量按非增顺序排列的非负向量。
(a) 证明 $K_{m+}$ 是真锥（proper cone）。
(b) 求对偶锥（dual cone）$K^*_{m+}$。提示：利用上面的恒等式（分部求和公式 / Abel 求和）。

**解答：**

**(a)** 集合 $K_{m+}$ 由 $n$ 个齐次线性不等式定义，因此它是一个闭（多面体）锥（closed polyhedral cone）。

$K_{m+}$ 的内部（interior）非空，因为存在点使所有不等式严格成立，例如
```math
x = (n, n-1, n-2, \ldots, 1).
```

为证 $K_{m+}$ 是尖的（pointed），注意：若 $x \in K_{m+}$，则只有当 $x = 0$ 时才有 $-x \in K_{m+}$（因为 $-x$ 分量非增意味着 $x$ 分量非减，两者同时要求所有分量相等；又非负与非正同时要求分量全为 0）。这推出锥不包含整条直线。

综上，$K_{m+}$ 是闭的、尖的、内部非空，故为真锥（proper cone）。

**(b)** 用提示中的恒等式。因为 $x \in K_{m+}$ 意味着系数 $x_1-x_2, x_2-x_3, \ldots, x_{n-1}-x_n, x_n$ 全部非负，所以
```math
y^T x = \sum_{i=1}^n x_i y_i \ge 0 \ \text{对所有 } x \in K_{m+}
```
当且仅当每个前缀和都非负：
```math
y_1 \ge 0, \quad y_1 + y_2 \ge 0, \quad \ldots, \quad y_1 + y_2 + \cdots + y_n \ge 0.
```
（充分性：若这些前缀和非负，则上述恒等式中每个非负系数乘非负前缀和，求和非负；必要性：令 $x$ 取只在前 $k$ 个分量为 1、其余为 0 的向量即可逼出第 $k$ 个前缀和非负。）

因此
```math
K^*_{m+} = \Big\{ y \ \Big|\ \sum_{i=1}^{k} y_i \ge 0, \ k = 1, \ldots, n \Big\}.
```

---

## 题目 3.2

**题干：** Level sets of convex, concave, quasiconvex, and quasiconcave functions. Some level sets of a function $f$ are shown below. The curve labeled 1 shows $\{x \mid f(x)=1\}$, etc. Could $f$ be convex (concave, quasiconvex, quasiconcave)? Explain your answer. Repeat for the level curves shown below.

**中文：** 凸、凹、拟凸、拟凹函数的水平集（level sets）。下面画出了某个函数 $f$ 的若干水平集，标注为 1 的曲线表示 $\{x \mid f(x)=1\}$，以此类推。问 $f$ 能否是凸（convex）、凹（concave）、拟凸（quasiconvex）、拟凹（quasiconcave）的？解释你的答案。对下面另一组水平曲线重复同样的分析。

[[FIGURE: 第 3 页, 第一组水平曲线：同心椭圆状，标号 1,2,3 由内向外；第二组水平曲线：6 条曲线标号 1-6，其中某方向呈「嵌套括号」形状]]

**解答：**

**第一个函数**可以是拟凸的（quasiconvex），因为它的下水平集（sublevel sets）看起来是凸的。它一定不是凹的或拟凹的（quasiconcave），因为其上水平集（superlevel sets）不是凸的。

它也不是凸的，理由如下。我们沿图中标为 I 的虚线画函数值的变化。沿这条线，函数经过下图黑点所示的那些点。显然在这条线段上函数不是凸的（函数值出现先升后降的形态，违反了凸函数在一条线上的限制仍为凸函数的性质）。

[[FIGURE: 第 4 页, 沿虚线 I 的函数值折线示意图，黑点标出取值点，说明非凸]]

**第二个函数**：重复同样分析可知，它可以是凹的（concave），因此也可以是拟凹的（quasiconcave）。它不可能是凸的或拟凸的，因为它的下水平集不是凸的。

---

## 题目 3.5

**题干：** Running average of a convex function. Suppose $f : \mathbf{R} \to \mathbf{R}$ is convex, with $\mathbf{R}_+ \subseteq \operatorname{dom} f$. Show that its running average $F$, defined as
```math
F(x) = \frac{1}{x}\int_0^x f(t)\, dt, \quad \operatorname{dom} F = \mathbf{R}_{++},
```
is convex. You can assume $f$ is differentiable.

**中文：** 凸函数的滑动平均（running average）。设 $f : \mathbf{R} \to \mathbf{R}$ 是凸函数，且 $\mathbf{R}_+ \subseteq \operatorname{dom} f$。证明其滑动平均 $F$（定义如上，$\operatorname{dom} F = \mathbf{R}_{++}$）是凸的。可假设 $f$ 可微。

**解答：**

$F$ 可微，其一阶导数为（用 Leibniz 法则 / 商法则）
```math
F'(x) = -\frac{1}{x^2}\int_0^x f(t)\, dt + \frac{f(x)}{x}.
```

再求二阶导数：
```math
F''(x) = \frac{2}{x^3}\int_0^x f(t)\, dt - \frac{2f(x)}{x^2} + \frac{f'(x)}{x}.
```

把分子合并、并把常数项写成积分（注意 $\int_0^x f(x)\, dt = xf(x)$，$\int_0^x f'(x)(t-x)\, dt = -\frac{x^2}{2}f'(x)$），可得
```math
F''(x) = \frac{2}{x^3}\int_0^x \big( f(t) - f(x) - f'(x)(t-x) \big)\, dt.
```

下面说明这个积分非负。由 $f$ 的凸性，对任意 $x, t \in \operatorname{dom} f$，有支撑超平面不等式（一阶凸性判据）
```math
f(t) \ge f(x) + f'(x)(t-x).
```
因此被积函数 $f(t) - f(x) - f'(x)(t-x) \ge 0$ 对所有 $t \in [0,x]$ 成立，故积分非负，即 $F''(x) \ge 0$。由「二阶导非负 ⟹ 凸」这一判据，$F$ 是凸函数。

**另一种（更简单？）证明：** 对每个固定 $s$，函数 $x \mapsto f(sx)$ 关于 $x$ 是凸的（这是凸函数与线性变换复合保持凸的性质）。因此积分
```math
\int_0^1 f(sx)\, ds
```
是 $x$ 的凸函数（凸函数对参数的积分/和保持凸）。做变量代换 $t = sx$（即 $t = sx$，$dt = x\, ds$，$s \in [0,1] \Leftrightarrow t \in [0,x]$），得到
```math
\int_0^1 f(sx)\, ds = \frac{1}{x}\int_0^x f(t)\, dt = F(x).
```
于是 $F$ 是凸函数。

---

## 题目 3.6

**题干：** Functions and epigraphs. When is the epigraph of a function a halfspace? When is the epigraph of a function a convex cone? When is the epigraph of a function a polyhedron?

**中文：** 函数与上图（epigraph）。问：一个函数的上图何时是半空间（halfspace）？何时是凸锥（convex cone）？何时是多面体（polyhedron）？

**解答：**

分别是：
- 上图是**半空间** ⟺ 函数是**仿射函数**（affine，即 $f(x) = a^T x + b$）；
- 上图是**凸锥** ⟺ 函数是**正齐次凸函数**（positively homogeneous 且凸，即 $f(\alpha x) = \alpha f(x)$ 对 $\alpha \ge 0$，且凸）；
- 上图是**多面体** ⟺ 函数是**分片仿射**（piecewise-affine）的凸函数。

（说明：上图 $\operatorname{epi} f = \{(x,t) \mid f(x) \le t\}$ 是半空间要求其边界 $f$ 为仿射；是凸锥要求它过原点且对正伸缩封闭，即 $f$ 正齐次且凸；是多面体要求 $f$ 为有限个仿射函数的上包络，即分片仿射且凸。）

---

## 题目 3.15

**题干：** A family of concave utility functions. For $0 < \alpha \le 1$ let
```math
u_\alpha(x) = \frac{x^\alpha - 1}{\alpha},
```
with $\operatorname{dom} u_\alpha = \mathbf{R}_+$. We also define $u_0(x) = \log x$ (with $\operatorname{dom} u_0 = \mathbf{R}_{++}$).
(a) Show that for $x > 0$, $u_0(x) = \lim_{\alpha \to 0} u_\alpha(x)$.
(b) Show that $u_\alpha$ are concave, monotone increasing, and all satisfy $u_\alpha(1) = 0$.

**中文：** 一族凹效用函数（concave utility functions）。对 $0 < \alpha \le 1$ 定义 $u_\alpha(x) = \frac{x^\alpha - 1}{\alpha}$，$\operatorname{dom} u_\alpha = \mathbf{R}_+$。另定义 $u_0(x) = \log x$（$\operatorname{dom} u_0 = \mathbf{R}_{++}$）。
(a) 证明对 $x > 0$，$u_0(x) = \lim_{\alpha \to 0} u_\alpha(x)$。
(b) 证明 $u_\alpha$ 是凹的、单调递增的，且都满足 $u_\alpha(1) = 0$。

（背景说明：这些函数在经济学中常用来刻画商品或金钱数量的效用。$u_\alpha$ 的凹性意味着边际效用（marginal utility，即商品量固定增加所带来的效用增量）随商品量增大而减小；换言之，凹性刻画了「饱和」效应。）

**解答：**

**(a)** 该极限中分子分母都趋于 0（$\alpha \to 0$ 时 $x^\alpha - 1 \to 0$，分母 $\alpha \to 0$），故用洛必达法则（l'Hôpital's rule）：
```math
\lim_{\alpha \to 0} u_\alpha(x) = \lim_{\alpha \to 0} \frac{(d/d\alpha)(x^\alpha - 1)}{(d/d\alpha)\alpha}
= \lim_{\alpha \to 0} \frac{x^\alpha \log x}{1} = \log x.
```
（用到 $\frac{d}{d\alpha}x^\alpha = x^\alpha \log x$。）

**(b)** 直接代入有
```math
u_\alpha(1) = \frac{1^\alpha - 1}{\alpha} = \frac{1 - 1}{\alpha} = 0.
```

一阶导数为
```math
u'_\alpha(x) = x^{\alpha - 1},
```
对 $0 < \alpha < 1$（且 $x > 0$）恒正，故这些函数单调递增。

为证凹性，考察二阶导数：
```math
u''_\alpha(x) = (\alpha - 1) x^{\alpha - 2}.
```
因为 $0 < \alpha \le 1$ 时 $\alpha - 1 < 0$（当 $\alpha < 1$），且 $x^{\alpha-2} > 0$，故 $u''_\alpha(x) < 0$ 对所有 $x > 0$ 成立，由「二阶导负 ⟹ 严格凹」的判据，$u_\alpha$ 严格凹（strictly concave）。（$\alpha = 1$ 时 $u_1(x) = x - 1$，为仿射，既凸又凹。）

---

## 题目 3.16

**题干：** For each of the following functions determine whether it is convex, concave, quasiconvex, or quasiconcave.

**中文：** 判断下列各函数是凸、凹、拟凸还是拟凹。

**(b)** $f(x_1, x_2) = x_1x_2$ on $\mathbf{R}^2_{++}$.

**解答：** $f$ 的 Hessian（Hessian matrix）为
```math
\nabla^2 f(x) = \begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix},
```
它既非正半定也非负半定（其特征值为 $\pm 1$）。因此 $f$ 既非凸也非凹。它是拟凹的（quasiconcave），因为其上水平集
```math
\{(x_1, x_2) \in \mathbf{R}^2_{++} \mid x_1 x_2 \ge \alpha\}
```
是凸的（双曲线 $x_1 x_2 = \alpha$ 在第一象限围出的区域是凸集）。它不是拟凸的。

**(c)** $f(x_1, x_2) = \frac{1}{x_1 x_2}$ on $\mathbf{R}^2_{++}$.

**解答：** $f$ 的 Hessian 为
```math
\nabla^2 f(x) = \frac{1}{x_1 x_2} \begin{bmatrix} \frac{2}{x_1^2} & \frac{1}{x_1 x_2} \\ \frac{1}{x_1 x_2} & \frac{2}{x_2^2} \end{bmatrix} \succeq 0.
```
（该矩阵对称且主子式非负、行列式非负，故正半定。）因此 $f$ 是凸的，也是拟凸的（凸函数必拟凸）。它不是拟凹的，也不是凹的。

**(d)** $f(x_1, x_2) = \frac{x_1}{x_2}$ on $\mathbf{R}^2_{++}$.

**解答：** $f$ 的 Hessian 为
```math
\nabla^2 f(x) = \begin{bmatrix} 0 & -\frac{1}{x_2^2} \\ -\frac{1}{x_2^2} & \frac{2x_1}{x_2^3} \end{bmatrix},
```
它既非正半定也非负半定（行列式 $-\frac{1}{x_2^4} < 0$）。因此 $f$ 不是凸的，也不是凹的。

它是拟凸且拟凹的（即拟线性 quasilinear），因为其下水平集与上水平集都是半空间（形如 $x_1 \le \alpha x_2$ 或 $x_1 \ge \alpha x_2$，均为过原点的半平面）。

**(e)** $f(x_1, x_2) = \frac{x_1^2}{x_2}$ on $\mathbf{R} \times \mathbf{R}_{++}$.

**解答：** $f$ 是凸的（教材第 72 页已提及，见图 3.3）。这容易通过计算 Hessian 验证：
```math
\nabla^2 f(x) = \begin{bmatrix} \frac{2}{x_2} & -\frac{2x_1}{x_2^2} \\ -\frac{2x_1}{x_2^2} & \frac{2x_1^2}{x_2^3} \end{bmatrix}
= \frac{2}{x_2} \begin{bmatrix} 1 \\ -\frac{x_1}{x_2} \end{bmatrix} \begin{bmatrix} 1 & -\frac{x_1}{x_2} \end{bmatrix} \succeq 0.
```
（因为 $\frac{2}{x_2} > 0$，且外积 $v v^T$ 半正定。）因此 $f$ 是凸的，也是拟凸的。它不是凹的，也不是拟凹的（见图）。

[[FIGURE: 第 7 页, $f(x_1,x_2)=x_1^2/x_2$ 的图像，展示凸性]]

---

## 题目 3.18

**题干：** Adapt the proof of concavity of the log-determinant function in §3.1.5 to show the following.
(b) $f(X) = (\det X)^{1/n}$ is concave on $\operatorname{dom} f = S^n_{++}$.

**中文：** 仿照 §3.1.5 中 log-determinant 函数凹性的证明方法，证明：
(b) $f(X) = (\det X)^{1/n}$ 在 $\operatorname{dom} f = S^n_{++}$ 上是凹的。

**解答：**

定义 $g(t) = f(Z + tV)$，其中 $Z \succ 0$ 且 $V \in S^n$（对称矩阵）。用行列式的分解（$Z \succ 0$ 可开方 $Z^{1/2}$）：
```math
g(t) = \big(\det(Z + tV)\big)^{1/n}
= \Big(\det Z^{1/2}\, \det(I + t Z^{-1/2} V Z^{-1/2})\, \det Z^{1/2}\Big)^{1/n}
= (\det Z)^{1/n} \Big( \prod_{i=1}^{n} (1 + t \lambda_i) \Big)^{1/n},
```
其中 $\lambda_i,\ i = 1, \ldots, n$ 是 $Z^{-1/2} V Z^{-1/2}$ 的特征值（eigenvalues）。（用到了 $\det(I + tA) = \prod_i (1 + t\lambda_i)$，以及 $\det Z^{1/2} \cdot \det Z^{1/2} = \det Z$。）

从最后一个等式看出，$g$ 在集合 $\{t \mid Z + tV \succ 0\}$ 上是 $t$ 的凹函数：因为 $\det Z > 0$，而几何平均 $\big(\prod_{i=1}^n x_i\big)^{1/n}$ 在 $\mathbf{R}^n_{++}$ 上是凹的（geometric mean 是凹函数）。于是沿任意方向 $V$，$f$ 在一维直线上都是凹的，故 $f$ 在 $S^n_{++}$ 上凹。

---

## 题目 3.24

**题干：** Some functions on the probability simplex. Let $x$ be a real-valued random variable which takes values in $\{a_1, \ldots, a_n\}$ where $a_1 < a_2 < \cdots < a_n$, with $\operatorname{prob}(x = a_i) = p_i$, $i = 1, \ldots, n$. For each of the following functions of $p$ (on the probability simplex $\{p \in \mathbf{R}^n_+ \mid \mathbf{1}^T p = 1\}$), determine if the function is convex, concave, quasiconvex, or quasiconcave.

**中文：** 概率单纯形（probability simplex）上的若干函数。设 $x$ 是取值为 $\{a_1, \ldots, a_n\}$（$a_1 < a_2 < \cdots < a_n$）的实值随机变量，$\operatorname{prob}(x=a_i) = p_i$。对下面每个关于 $p$ 的函数（定义在概率单纯形 $\{p \in \mathbf{R}^n_+ \mid \mathbf{1}^T p = 1\}$ 上），判断它是凸、凹、拟凸还是拟凹。

**(f)** $\text{quartile}(x) = \inf\{\beta \mid \operatorname{prob}(x \le \beta) \ge 0.25\}$（第一四分位数）。

**解答：** $\text{quartile}(x)$ 的下水平集与上水平集都是凸的（见题目 2.15），所以它既是拟凸的（quasiconvex）又是拟凹的（quasiconcave）。

$\text{quartile}(x)$ 不连续（它只在离散集合 $\{a_1, \ldots, a_n\}$ 上取值），所以它不是凸的也不是凹的。（凸函数或凹函数在其定义域的仿射内部（relative interior）上总是连续的。）

**(g)** The cardinality of the smallest set $A \subseteq \{a_1, \ldots, a_n\}$ with probability $\ge 90\%$.（概率和 $\ge 90\%$ 的最小集合 $A$ 的基数，即所含元素个数。）

**解答：** $f$ 是整数取值的，所以它不可能是凸或凹的（同上，凸/凹函数在其相对内部上连续，而整数取值函数不连续）。

$f$ 是拟凹的，因为它的上水平集是凸的。事实上，$f(p) \ge \alpha$ 当且仅当
```math
\sum_{i=1}^{k} p_{[i]} < 0.9,
```
其中 $k = \max\{i = 1, \ldots, n \mid i < \alpha\}$ 是小于 $\alpha$ 的最大整数，$p_{[i]}$ 是 $p$ 的第 $i$ 大分量。我们知道 $\sum_{i=1}^{k} p_{[i]}$ 是 $p$ 的凸函数（前 $k$ 大分量之和是凸的），故不等式 $\sum_{i=1}^k p_{[i]} < 0.9$ 定义一个凸集。

一般地 $f(p)$ 不是拟凸的。反例：取 $n = 2$，$a_1 = 0$，$a_2 = 1$，$p_1 = (0.1, 0.9)$，$p_2 = (0.9, 0.1)$。则 $f(p_1) = f(p_2) = 1$（单个元素概率 $\ge 0.9$ 即可），但 $f\big(\frac{p_1+p_2}{2}\big) = f(0.5, 0.5) = 2$（需要两个元素），违反了拟凸函数「中点处值不超过两端最大值」的性质。

**(h)** The minimum width interval that contains 90% of the probability, i.e.,
```math
\inf\{\beta - \alpha \mid \operatorname{prob}(\alpha \le x \le \beta) \ge 0.9\}.
```
（包含 90% 概率的最小宽度区间。）

**解答：** 包含 90% 概率的最小宽度区间必形如 $[a_i, a_j]$，$1 \le i \le j \le n$，因为
```math
\operatorname{prob}(\alpha \le x \le \beta) = \sum_{k=i}^{j} p_k = \operatorname{prob}(a_i \le x \le a_j),
```
其中 $i = \min\{k \mid a_k \ge \alpha\}$，$j = \max\{k \mid a_k \le \beta\}$（区间端点取到相邻的取值点即可）。

我们证明该函数是拟凹的。$f(p) \ge \gamma$ 当且仅当所有宽度小于 $\gamma$ 的区间概率都小于 90%，即对满足 $a_j - a_i < \gamma$ 的所有 $i, j$，都有
```math
\sum_{k=i}^{j} p_k < 0.9.
```
这（一系列线性不等式的交集）定义一个凸集。

因为该函数取值于有限集合，故不连续，因而既非凸也非凹。此外它一般也不是拟凸的。反例：$n = 2$，$a_1 = 0$，$a_2 = 1$，$p_1 = (0.95, 0.05)$，$p_2 = (0.05, 0.95)$。则 $f(p_1) = 0$，$f(p_2) = 0$（单点即有 95% 概率），但 $f\big(\frac{p_1+p_2}{2}\big) = f(0.5,0.5) = 1$（需宽 1 的区间才凑够 90%），违反拟凸性。

---

## 题目 3.36

**题干：** Derive the conjugates of the following functions.

**中文：** 推导下列函数的共轭（conjugate）函数。

**(a)** Max function. $f(x) = \max_{i=1,\ldots,n} x_i$ on $\mathbf{R}^n$.

**解答：** 我们证明
```math
f^*(y) = \begin{cases} 0 & \text{若 } y \succeq 0, \ \mathbf{1}^T y = 1 \\ \infty & \text{否则}. \end{cases}
```

先验证 $f^*$ 的定义域。首先设 $y$ 有某个负分量，比如 $y_k < 0$。取向量 $x$，令 $x_k = -t$，$x_i = 0$（$i \ne k$），并令 $t \to \infty$，则
```math
x^T y - \max_i x_i = -t y_k \to \infty,
```
故 $y \notin \operatorname{dom} f^*$。

其次设 $y \succeq 0$ 但 $\mathbf{1}^T y > 1$。取 $x = t\mathbf{1}$ 并令 $t \to \infty$，则
```math
x^T y - \max_i x_i = t\mathbf{1}^T y - t = t(\mathbf{1}^T y - 1) \to \infty,
```
无上界。类似地，当 $y \succeq 0$ 且 $\mathbf{1}^T y < 1$ 时，取 $x = -t\mathbf{1}$ 并令 $t \to \infty$（此时 $x^T y - \max_i x_i = -t\mathbf{1}^T y + t = t(1 - \mathbf{1}^T y) \to \infty$）。

剩下 $y$ 满足 $y \succeq 0$ 且 $\mathbf{1}^T y = 1$ 的情形。此时对任意 $x$，由 Holder 型不等式有 $x^T y \le \max_i x_i$（因为 $\sum_i y_i = 1$ 且 $y_i \ge 0$，$x^T y$ 是 $x_i$ 的加权平均，不超过最大值），故 $x^T y - \max_i x_i \le 0$ 对所有 $x$ 成立，且当 $x = 0$ 时取到等号 0。因此 $f^*(y) = 0$。

**(d)** Power function. $f(x) = x^p$ on $\mathbf{R}_{++}$，其中 $p > 1$。再对 $p < 0$ 重复。

**解答：** 采用标准记号：定义 $q$ 由 $\frac{1}{p} + \frac{1}{q} = 1$ 确定，即 $q = \frac{p}{p-1}$。

先看 $p > 1$ 的情形。此时 $x^p$ 在 $\mathbf{R}_+$ 上严格凸。对 $y < 0$，函数 $yx - x^p$ 在 $x > 0$ 上的最大值在 $x = 0$ 处取到（因为 $y < 0$ 时 $yx - x^p < 0$ 且 $x \to 0$ 时趋于 0，$x \to \infty$ 时趋于 $-\infty$），故 $f^*(y) = 0$。

对 $y > 0$，令 $g(x) = yx - x^p$，其最大值点由一阶条件 $g'(x) = y - p x^{p-1} = 0$ 给出，即
```math
x = (y/p)^{1/(p-1)}.
```
在该点处函数值为
```math
y\,(y/p)^{1/(p-1)} - (y/p)^{p/(p-1)} = (p-1)(y/p)^{q}
```
（用 $q = p/(p-1)$，故 $\frac{1}{p-1} = \frac{q}{p}$，$\frac{p}{p-1} = q$，整理即得）。因此
```math
f^*(y) = \begin{cases} 0 & y \le 0 \\ (p-1)(y/p)^q & y > 0. \end{cases}
```

对 $p < 0$，类似论证可得 $\operatorname{dom} f^* = -\mathbf{R}_+$（即 $y < 0$），且
```math
f^*(y) = \frac{p}{q}(y/p)^q.
```
（注：此处原文给出 $f^*(y) = \frac{p}{q}(y/p)^q$，其中 $p<0$ 使 $\frac{p}{q} = p \cdot \frac{p-1}{p} = p-1 < 0$，与 $y < 0$ 时 $y^q > 0$ 相乘得正，符合共轭函数非负的要求。）
