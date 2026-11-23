# EE364a Homework 2 solutions（EE364a 作业 2 解答）

> 原文：`HW2 解答.pdf`（9 页），EE364a, Winter 2007-08, Prof. S. Boyd。
> 覆盖教材习题 2.28, 2.33, 3.2, 3.5, 3.6, 3.15, 3.16(b–e), 3.18(b), 3.24(f–h), 3.36(a,d)，主题为 convex function（凸函数）。
> 译注：3.24 与 3.36 原解答只给出了指定的子题，其余子题未做，本译文照原文只译已给部分。

## 2.28 Positive semidefinite cone for $n = 1, 2, 3$（$n = 1,2,3$ 时的半正定锥）

给出 $n = 1, 2, 3$ 时 positive semidefinite cone（半正定锥）$\mathbf{S}^n_+$ 的显式描述，用矩阵元素与普通不等式表示。为描述 $\mathbf{S}^n$ 的一般元素（$n = 1,2,3$），采用下面的记号：

```math
n = 1:\ x_1; \qquad
n = 2:\ \begin{bmatrix} x_1 & x_2 \\ x_2 & x_3 \end{bmatrix}; \qquad
n = 3:\ \begin{bmatrix} x_1 & x_2 & x_3 \\ x_2 & x_4 & x_5 \\ x_3 & x_5 & x_6 \end{bmatrix}.
```

**Solution**。$n = 1$ 时条件是 $x_1 \ge 0$。$n = 2$ 时条件是

```math
x_1 \ge 0,\quad x_3 \ge 0,\quad x_1 x_3 - x_2^2 \ge 0 .
```

$n = 3$ 时条件是

```math
x_1 \ge 0,\quad x_4 \ge 0,\quad x_6 \ge 0,\quad
x_1 x_4 - x_2^2 \ge 0,\quad x_4 x_6 - x_5^2 \ge 0,\quad x_1 x_6 - x_3^2 \ge 0,
```

并且

```math
x_1 x_4 x_6 + 2 x_2 x_3 x_5 - x_1 x_5^2 - x_6 x_2^2 - x_4 x_3^2 \ge 0,
```

也就是说：**所有 principal minor（主子式）都必须非负**。

下面在「$n = 2$ 的结论已成立」的前提下给出 $n = 3$ 的证明。矩阵

```math
X = \begin{bmatrix} x_1 & x_2 & x_3 \\ x_2 & x_4 & x_5 \\ x_3 & x_5 & x_6 \end{bmatrix}
```

是 positive semidefinite 当且仅当对所有 $z$ 有

```math
z^T X z = x_1 z_1^2 + 2 x_2 z_1 z_2 + 2 x_3 z_1 z_3 + x_4 z_2^2 + 2 x_5 z_2 z_3 + x_6 z_3^2 \ge 0 .
```

若 $x_1 = 0$，则必须有 $x_2 = x_3 = 0$，于是 $X \succeq 0$ 当且仅当

```math
\begin{bmatrix} x_4 & x_5 \\ x_5 & x_6 \end{bmatrix} \succeq 0 .
```

引用 $2 \times 2$ 情形的结论，可得：当 $x_1 = 0$ 时，$X \succeq 0$ 当且仅当

```math
x_2 = x_3 = 0,\quad x_4 \ge 0,\quad x_6 \ge 0,\quad x_4 x_6 - x_5^2 \ge 0 .
```

现在设 $x_1 \neq 0$。此时

```math
\begin{aligned}
z^T X z ={}& x_1 \big(z_1 + (x_2/x_1) z_2 + (x_3/x_1) z_3\big)^2 \\
&+ (x_4 - x_2^2/x_1) z_2^2 + (x_6 - x_3^2/x_1) z_3^2
+ 2 (x_5 - x_2 x_3/x_1) z_2 z_3,
\end{aligned}
```

由此可见必须有 $x_1 > 0$，且

```math
\begin{bmatrix}
x_4 - x_2^2/x_1 & x_5 - x_2 x_3/x_1 \\
x_5 - x_2 x_3/x_1 & x_6 - x_3^2/x_1
\end{bmatrix} \succeq 0 .
```

按上面已研究的 $2 \times 2$ 情形结论，这等价于

```math
x_1 x_4 - x_2^2 \ge 0,\quad x_1 x_6 - x_3^2 \ge 0,\quad
(x_4 - x_2^2/x_1)(x_6 - x_3^2/x_1) - (x_5 - x_2 x_3/x_1)^2 \ge 0 .
```

（第一、二个不等式是把该 $2\times2$ 阵的对角元乘以 $x_1 > 0$ 后得到的，因为乘正常数不改变符号。）第三个不等式化简为

```math
(x_1 x_4 x_6 + 2 x_2 x_3 x_5 - x_1 x_5^2 - x_6 x_2^2 - x_4 x_3^2)/x_1 \ge 0 .
```

因此，若 $x_1 > 0$，则 $X \succeq 0$ 当且仅当

```math
x_1 x_4 - x_2^2 \ge 0,\quad x_1 x_6 - x_3^2 \ge 0,\quad
(x_1 x_4 x_6 + 2 x_2 x_3 x_5 - x_1 x_5^2 - x_6 x_2^2 - x_4 x_3^2)/x_1 \ge 0 .
```

把 $x_1 = 0$ 与 $x_1 > 0$ 两种情形的条件合起来，就是一句话：**7 个 principal minor 都必须非负**。

## 2.33 The monotone nonnegative cone（单调非负锥）

我们定义 monotone nonnegative cone（单调非负锥）为

```math
K_{m+} = \{x \in \mathbf{R}^n \mid x_1 \ge x_2 \ge \cdots \ge x_n \ge 0\},
```

即所有分量按非增顺序排列的非负向量。

**(a)** 证明 $K_{m+}$ 是 proper cone（真锥）。

**(b)** 求 dual cone（对偶锥）$K_{m+}^*$。**Hint**：使用恒等式

```math
\begin{aligned}
\sum_{i=1}^n x_i y_i ={}& (x_1 - x_2) y_1 + (x_2 - x_3)(y_1 + y_2) + (x_3 - x_4)(y_1 + y_2 + y_3) + \cdots \\
&+ (x_{n-1} - x_n)(y_1 + \cdots + y_{n-1}) + x_n (y_1 + \cdots + y_n).
\end{aligned}
```

**Solution**。

**(a)** 集合 $K_{m+}$ 由 $n$ 个齐次 linear inequality 定义，因此是闭的（polyhedral）cone。

$K_{m+}$ 的 interior（内部）非空，因为存在严格满足这些不等式的点，例如 $x = (n, n-1, n-2, \dots, 1)$。

为证明 $K_{m+}$ 是 pointed（尖的），注意若 $x \in K_{m+}$，则只有当 $x = 0$ 时才有 $-x \in K_{m+}$。这意味着该 cone 不包含整条直线。

**(b)** 用 hint，可以看出：对所有 $x \in K_{m+}$ 有 $y^T x \ge 0$，当且仅当

```math
y_1 \ge 0,\quad y_1 + y_2 \ge 0,\quad \dots,\quad y_1 + y_2 + \cdots + y_n \ge 0 .
```

原因是 hint 把 $\sum_i x_i y_i$ 写成了非负系数 $(x_i - x_{i-1})$ 与 $x_n$ 的加权和，而这些系数在 $K_{m+}$ 中可独立取任意非负值。因此

```math
K_{m+}^* = \Big\{ y \ \Big|\ \sum_{i=1}^k y_i \ge 0,\ k = 1,\dots,n \Big\}.
```

## 3.2 Level sets of convex, concave, quasiconvex, and quasiconcave functions（凸、凹、拟凸、拟凹函数的水平集）

下图给出了函数 $f$ 的某些 level set（水平集）。标号为 1 的曲线是 $\{x \mid f(x) = 1\}$，依此类推。

[Figure 1：三个同心闭合曲线族——外围曲线标 1、中间曲线标 2、内层曲线标 3，形状像被压扁的椭圆（convex 的闭合曲线），并画出它们的凸包内部；三条曲线层层嵌套，没有凹陷]

$f$ 可能是 convex（concave、quasiconvex、quasiconcave）吗？解释你的答案。对下图的 level curve（水平曲线）重复该问题。

[Figure 2：另一组水平曲线——曲线标号为 1, 2, 3, 4, 5, 6，呈向外发散的开放曲线（不是闭合的），在图上自左上向右下展开，越靠外的曲线越平缓；同一图上有虚线 I 与 II 各穿过多条水平曲线]

**Solution**。第一个函数可能是 quasiconvex，因为其 sublevel set（下水平集）看上去是 convex 的。它肯定不是 concave 或 quasiconcave，因为其 superlevel set（上水平集）不是 convex。

它也不是 convex，理由如下。我们沿图中标记为 I 的虚线画出函数值。

[Figure 3：沿虚线 I 的函数值剖面——横轴是沿 I 的位置，纵轴是 $f$ 的值；曲线在标号 3、2、1 对应位置出现起伏，整体呈非凸的「先降后升再降」形状]

沿这条线，函数经过下图中标为黑点的那些点。显然在这个线段上，函数不是 convex。

[Figure 4：虚线上的采样点示意——曲线 I 上依次标出若干黑点，相邻黑点之间的弦位于函数图像下方，说明沿该线段的一维函数不满足 convexity]

对第二个函数做同样的分析，可以看出它可能是 concave（因此也可能是 quasiconcave）。它不可能是 convex 或 quasiconvex，因为其 sublevel set 不是 convex 的。

## 3.5 Running average of a convex function（凸函数的滑动平均）

设 $f : \mathbf{R} \to \mathbf{R}$ 是 convex，且 $\mathbf{R}_+ \subseteq \operatorname{dom} f$。证明它的 running average（滑动平均）$F$

```math
F(x) = \frac{1}{x} \int_0^x f(t)\, dt, \qquad \operatorname{dom} F = \mathbf{R}_{++},
```

是 convex 的。可以假设 $f$ 可微。

**Solution**。$F$ 可微，且

```math
F'(x) = -(1/x^2) \int_0^x f(t)\, dt + f(x)/x,
```

```math
\begin{aligned}
F''(x) &= (2/x^3) \int_0^x f(t)\, dt - 2 f(x)/x^2 + f'(x)/x \\
&= (2/x^3) \int_0^x \big( f(t) - f(x) - f'(x)(t - x) \big)\, dt .
\end{aligned}
```

（第二个等号用 $\int_0^x dt = x$ 与 $\int_0^x (t-x)\,dt = -x^2/2$ 把 $-2f(x)/x^2 + f'(x)/x$ 折进积分号内，逐项核对：$(2/x^3)\int_0^x f(x)\,dt = 2f(x)/x^2$，$(2/x^3)\int_0^x f'(x)(t-x)\,dt = f'(x)/x \cdot (-1)$，符号恰与上式一致。）

convexity 现在由下面这个事实推出：

```math
f(t) \ge f(x) + f'(x)(t - x)
```

对所有 $x, t \in \operatorname{dom} f$ 成立，这蕴含 $F''(x) \ge 0$。

这里还有另一个（可能更简单的）证明。对每个固定的 $s$，函数 $f(sx)$ 关于 $x$ 是 convex 的。因此

```math
\int_0^1 f(sx)\, ds
```

是 $x$ 的 convex function。现在作变量替换 $t = sx$，得

```math
\int_0^1 f(sx)\, ds = \frac{1}{x} \int_0^x f(t)\, dt .
```

（第二个证明回避了「$f$ 可微」这一假设：非负权重的凸函数积分仍是凸的，这正是 convexity 保持封闭性的直接应用。）

## 3.6 Functions and epigraphs（函数与上境图）

什么时候一个函数的 epigraph（上境图）是一个 halfspace？什么时候一个函数的 epigraph 是一个 convex cone？什么时候一个函数的 epigraph 是一个 polyhedron？

**Solution**。当函数分别满足：是 affine（仿射）的；是 positively homogeneous（正齐次，即 $f(\alpha x) = \alpha f(x)$ 对 $\alpha \ge 0$）的；是 piecewise-affine（分段仿射）的——时，其 epigraph 分别是 halfspace、convex cone、polyhedron。

## 3.15 A family of concave utility functions（一族凹效用函数）

对 $0 < \alpha \le 1$，令

```math
u_\alpha(x) = \frac{x^\alpha - 1}{\alpha},
```

其中 $\operatorname{dom} u_\alpha = \mathbf{R}_+$。另外定义 $u_0(x) = \log x$（$\operatorname{dom} u_0 = \mathbf{R}_{++}$）。

**(a)** 证明对 $x > 0$ 有 $u_0(x) = \lim_{\alpha \to 0} u_\alpha(x)$。

**(b)** 证明 $u_\alpha$ 是 concave、monotone increasing（单调增）的，且都满足 $u_\alpha(1) = 0$。

这些函数在经济学中常用来建模一定量商品或金钱带来的收益或 utility（效用）。$u_\alpha$ 的 concavity 意味着 marginal utility（边际效用，即商品固定增量所带来的效用增量）随商品数量增加而下降。换句话说，concavity 刻画了 satiation（饱和）效应。

**Solution**。

**(a)** 在这个极限中，分子与分母都趋于零，故使用 l'Hôpital 法则：

```math
\lim_{\alpha \to 0} u_\alpha(x)
= \lim_{\alpha \to 0} \frac{(d/d\alpha)(x^\alpha - 1)}{(d/d\alpha)\,\alpha}
= \lim_{\alpha \to 0} \frac{x^\alpha \log x}{1}
= \log x .
```

**(b)** 直接代入即可看出

```math
u_\alpha(1) = \frac{1^\alpha - 1}{\alpha} = 0 .
```

导数由下式给出：

```math
u_\alpha'(x) = x^{\alpha - 1},
```

它对所有 $x > 0$ 为正（因为 $0 < \alpha < 1$），因此这些函数是递增的。为说明 concavity，考察二阶导数：

```math
u_\alpha''(x) = (\alpha - 1) x^{\alpha - 2} .
```

它对所有 $x > 0$ 为负，故 $u_\alpha$ 是严格 concave 的。

## 3.16

对下列每个函数，判断它是 convex、concave、quasiconvex 还是 quasiconcave。

**(b)** $f(x_1, x_2) = x_1 x_2$，定义在 $\mathbf{R}^2_{++}$ 上。

**Solution**。$f$ 的 Hessian（黑塞矩阵）是

```math
\nabla^2 f(x) = \begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix},
```

它既不是 positive semidefinite 也不是 negative semidefinite（特征值为 $+1$ 与 $-1$）。因此 $f$ 既不是 convex 也不是 concave。它是 quasiconcave 的，因为它的 superlevel set

```math
\{(x_1, x_2) \in \mathbf{R}^2_{++} \mid x_1 x_2 \ge \alpha\}
```

是 convex 的。它不是 quasiconvex 的。

**(c)** $f(x_1, x_2) = 1/(x_1 x_2)$，定义在 $\mathbf{R}^2_{++}$ 上。

**Solution**。$f$ 的 Hessian 是

```math
\nabla^2 f(x) = \frac{2}{x_1 x_2} \begin{bmatrix} 2/(x_1^2) & 1/(x_1 x_2) \\ 1/(x_1 x_2) & 2/(x_2^2) \end{bmatrix} \succeq 0 .
```

（验证 PSD：左上元 $2/x_1^2 > 0$，行列式 $(4/(x_1^2 x_2^2)) - (1/(x_1^2 x_2^2)) = 3/(x_1^2 x_2^2) > 0$，故整阵 $\succeq 0$。）因此 $f$ 是 convex 的、也是 quasiconvex 的。它不是 quasiconcave 或 concave。

**(d)** $f(x_1, x_2) = x_1/x_2$，定义在 $\mathbf{R}^2_{++}$ 上。

**Solution**。$f$ 的 Hessian 是

```math
\nabla^2 f(x) = \begin{bmatrix} 0 & -1/x_2^2 \\ -1/x_2^2 & 2 x_1/x_2^3 \end{bmatrix},
```

它不是 positive semidefinite 也不是 negative semidefinite（左上元为 $0$、右上元非零，特征值一正一负）。因此 $f$ 既不是 convex 也不是 concave。

它是 quasiconvex 且 quasiconcave 的（即 quasilinear（拟线性）），因为它的 sublevel set 与 superlevel set 都是 halfspace。

**(e)** $f(x_1, x_2) = x_1^2/x_2$，定义在 $\mathbf{R} \times \mathbf{R}_{++}$ 上。

**Solution**。$f$ 是 convex 的，教材第 72 页已提到（另见教材图 3.3）。直接算出 Hessian 即可验证：

```math
\nabla^2 f(x) = \begin{bmatrix} 2/x_2 & -2 x_1/x_2^2 \\ -2 x_1/x_2^2 & 2 x_1^2/x_2^3 \end{bmatrix}
= \frac{2}{x_2} \begin{bmatrix} 1 \\ -x_1/x_2 \end{bmatrix} \begin{bmatrix} 1 & -x_1/x_2 \end{bmatrix} \succeq 0 .
```

（最后一式把 Hessian 写成 rank-one 的外积 $\frac{2}{x_2} v v^T$，其中 $v = (1, -x_1/x_2)^T$；因 $x_2 > 0$，该矩阵 $\succeq 0$。）因此 $f$ 是 convex 的、也是 quasiconvex 的。它不是 concave 或 quasiconcave（见教材图）。

## 3.18

仿照 §3.1.5 中对 log-determinant（对数行列式）函数 concavity 的证明，证明下列结论。

**(b)** $f(X) = (\det X)^{1/n}$ 在 $\operatorname{dom} f = \mathbf{S}^n_{++}$ 上是 concave 的。

**Solution**。

**(b)** 定义 $g(t) = f(Z + tV)$，其中 $Z \succ 0$、$V \in \mathbf{S}^n$。

```math
\begin{aligned}
g(t) &= (\det(Z + tV))^{1/n} \\
&= \det(Z^{1/2})^{\,1/n} \det(I + t Z^{-1/2} V Z^{-1/2})^{1/n} \det(Z^{1/2})^{\,1/n} \\
&= (\det Z)^{1/n} \Big( \prod_{i=1}^n (1 + t \lambda_i) \Big)^{1/n},
\end{aligned}
```

其中 $\lambda_i$（$i = 1,\dots,n$）是 $Z^{-1/2} V Z^{-1/2}$ 的 eigenvalues（特征值）。这里第二个等号用了 $\det(AB) = \det A \det B$ 与

```math
\det(Z + tV) = \det(Z^{1/2}) \det(I + t Z^{-1/2} V Z^{-1/2}) \det(Z^{1/2}),
```

第三个等号用了 $\det(I + tM) = \prod_i (1 + t\lambda_i(M))$。

从最后一个等式可以看出，$g$ 是 $t$ 在集合 $\{t \mid Z + tV \succ 0\}$ 上的 concave function，因为 $\det Z > 0$，而 geometric mean（几何平均）$(\prod_{i=1}^n x_i)^{1/n}$ 在 $\mathbf{R}^n_{++}$ 上是 concave 的。

## 3.24 Some functions on the probability simplex（概率单纯形上的若干函数）

设 $x$ 是实值 random variable，取值于 $\{a_1 , \dots, a_n\}$，其中 $a_1 < a_2 < \cdots < a_n$，且 $\operatorname{prob} (x = a_i) = p_i$，$i = 1 , \dots, n$。对下列 $p$ 的函数（定义在 probability simplex $\{p \in \mathbf{R} ^n_+ \mid \mathbf{1} ^T p = 1 \}$ 上），判断它是 convex、concave、quasiconvex 还是 quasiconcave。

**(f)** $\operatorname{quartile}(x) = \inf\{\beta \mid \operatorname{prob}(x \le \beta) \ge 0.25\}$。

**Solution**。$\operatorname{quartile}(x)$ 的 sublevel set 与 superlevel set 都是 convex 的（见习题 2.15），因此它既 quasiconvex 又 quasiconcave。

$\operatorname{quartile}(x)$ 不是连续的（它取值于离散集合 $\{a_1,\dots,a_n\}$），因此它既不是 convex 也不是 concave。（convex 或 concave 函数在其定义域的相对内部总是连续的。）

**(g)** 使概率 $\ge 90\%$ 的最小集合 $A \subseteq \{a_1,\dots,a_n\}$ 的 cardinality（基数）。（cardinality 指 $A$ 中元素的个数。）

**Solution**。$f$ 取整数值，因此它不可能是 convex 或 concave。（convex 或 concave 函数在其定义域的相对内部总是连续的。）

$f$ 是 quasiconcave 的，因为它的 superlevel set 是 convex 的。我们有 $f(p) \ge \alpha$ 当且仅当

```math
\sum_{i=1}^k p_{[i]} < 0.9,
```

其中 $k = \max\{i = 1 , \dots, n \mid i < \alpha\}$ 是小于 $\alpha$ 的最大整数，而 $p_{[i]}$ 是 $p$ 的第 $i$ 大分量。已知 $\sum_{i=1} ^k p_{[i]}$ 是 $p$ 的 convex function，因此不等式 $\sum_{i=1} ^k p_{[i]} < 0 . 9$ 定义的是一个 convex set。

一般情形下 $f(p)$ 不是 quasiconvex 的。例如取 $n = 2$、$a_1 = 0$、$a_2 = 1$，并取 $p^1 = (0.1, 0.9)$、$p^2 = (0.9, 0.1)$。此时 $f(p^1) = f(p^2) = 1$，但

```math
f\big((p^1 + p^2)/2\big) = f(0.5, 0.5) = 2 .
```

（说明：$p^1$ 的最多概率元素是 $0.9$，已满足 $\ge 90\%$，故只需 1 个元素；而 midpoint $(0.5,0.5)$ 需要两个元素才达到 $0.9$，故 $f = 2$。这违反了 quasiconvex 所要求的「下水平集 convex」。）

**(h)** 包含 $90\%$ 概率的最小宽度区间，即

```math
\inf \{\beta - \alpha \mid \operatorname{prob}(\alpha \le x \le \beta) \ge 0.9\} .
```

**Solution**。包含 $90\%$ 概率的最小宽度区间必定形如 $[a_i, a_j]$（$1 \le i \le j \le n$），因为

```math
\operatorname{prob}(\alpha \le x \le \beta) = \sum_{k=i}^j p_k = \operatorname{prob}(a_i \le x \le a_j),
```

其中 $i = \min\{k \mid a_k \ge \alpha\}$，$j = \max\{k \mid a_k \le \beta\}$。

我们来证明该函数是 quasiconcave 的。$f(p) \ge \gamma$ 当且仅当所有宽度小于 $\gamma$ 的区间都具有小于 $90\%$ 的概率，

```math
\sum_{k=i}^j p_k < 0.9
```

对所有满足 $a_j - a_i < \gamma$ 的 $i, j$ 成立。这定义的是一个 convex set（每个这样的不等式都是线性的，且方向一致）。

由于该函数取值于有限集合，它不连续，因此既不是 convex 也不是 concave。此外，一般情形下它不是 quasiconvex 的。考虑 $n = 2$、$a_1 = 0$、$a_2 = 1$、$p^1 = (0.95, 0.05)$、$p^2 = (0.05, 0.95)$ 的例子。此时 $f(p^1) = 0$、$f(p^2) = 0$，但

```math
f\big((p^1 + p^2)/2\big) = 1 .
```

（说明：$p^1$ 与 $p^2$ 各自集中在单个点上，宽度 $0$ 就覆盖了 $\ge 90\%$；而 $(0.5, 0.5)$ 需要区间 $[0,1]$，宽度 $1$。）

## 3.36 Derive the conjugates（推导共轭函数）

**(a) Max function（最大值函数）**。$f(x) = \max_{i=1,\dots,n} x_i$，定义在 $\mathbf{R}^n$ 上。

**Solution**。我们将证明

```math
f^*(y) = \begin{cases} 0 & \text{if } y \succeq 0, \ \mathbf{1}^T y = 1, \\ \infty & \text{otherwise}. \end{cases}
```

先验证 $f^*$ 的 domain。首先设 $y$ 有某个负分量，比如 $y_k < 0$。取向量 $x$ 使 $x_k = - t$、$i \neq k$ 时 $x_i = 0$，令 $t \to \infty$，可见

```math
x^T y - \max_i x_i = -t y_k \to \infty,
```

所以 $y$ 不在 $\operatorname{dom} f^*$ 中。其次，设 $y \succeq 0$ 但 $\mathbf{1}^T y > 1$。取 $x = t\mathbf{1}$ 并令 $t \to \infty$，可见

```math
x^T y - \max_i x_i = t \mathbf{1}^T y - t
```

无上界。类似地，当 $y \succeq 0$ 且 $\mathbf{1} ^T y < 1$ 时，取 $x = - t\mathbf{1}$ 并令 $t \to \infty$。

剩下的一种情形是 $y \succeq 0$ 且 $\mathbf{1}^T y = 1$。此时对所有 $x$ 有

```math
x^T y \le \max_i x_i,
```

因此对所有 $x$ 有 $x^T y - \max_i x_i \le 0$，并且 $x = 0$ 时取等号。所以 $f^*(y) = 0$。

（补充说明第一个不等式的来源：$x^T y = \sum_i y_i x_i \le (\sum_i y_i) \max_i x_i = \max_i x_i$，这里用了 $y \succeq 0$ 与 $\mathbf{1}^T y = 1$。）

**(d) Power function（幂函数）**。$f(x) = x^p$ 定义在 $\mathbf{R} _{+ +}$ 上，其中 $p > 1$。再对 $p < 0$ 重复一遍。

**Solution**。沿用标准记号：定义 $q$ 由方程 $1/p + 1/q = 1$ 给出，即 $q = p/(p-1)$。

先看 $p > 1$ 的情形。此时 $x^p$ 在 $\mathbf{R} _+$ 上严格 convex。对 $y < 0$，函数 $yx - x^p$ 在 $x > 0$ 上于 $x = 0$ 处取得最大值，故 $f^* (y) = 0$。对 $y > 0$，该函数在 $x = (y/ p)^{1 / (p- 1 )}$ 处取得最大值，其值为

```math
y (y/p)^{1/(p-1)} - (y/p)^{p/(p-1)} = (p-1)(y/p)^q .
```

因此

```math
f^*(y) = \begin{cases} 0 & y \le 0, \\ (p-1)(y/p)^q & y > 0 . \end{cases}
```

对 $p < 0$，类似的论证给出 $\operatorname{dom} f^* = - \mathbf{R} _+$ 以及 $f^* (y) = (y/ p)^q$†。

† 原文此处漏了因子 $(p- 1 )$。正确结果是 $f^* (y) = (p- 1 )(y/ p)^q$，与 $p> 1$ 情形同形：由 $1 / p + 1 / q = 1$ 及 $p \lt 0$ 得 $q = p/ (p- 1 ) \in (0 , 1 )$，而 $\mathbf{1} ^T$ 意义上的极值点为 $x = (y/ p)^{1 / (p- 1 )}$，代回 $yx - x^p$ 得 $(p- 1 )u^p = (p- 1 )(y/ p)^q$，其中 $u = (y/ p)^{1 / (p- 1 )}$。数值验算：$p = - 1$、$y = - 1$ 时正确值 $f^* (- 1 ) = \sup_{x> 0} (- x - 1 / x) = - 2$，而 $(p- 1 )(y/ p)^q = (- 2 )(1 )^{1 / 2} = - 2$，与原文的 $(y/ p)^q = 1$ 不符。
