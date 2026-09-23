# <span style="color:#c0392b;">Boyd《凸优化》第 3 章 凸函数（下：§3.3–3.6 + 文献）</span>

> <span style="color:#7f8c8d;">Source: 教材/Convex Optimization (Boyd, Vandenberghe).pdf · Chapter 3 §3.3–3.6 + Bibliography, pp.91–112 (PDF p.105–126) · 行内双语翻译版 · 译者：ZCode（bilingual-translator 铁律）· 习题部分（PDF p.127–140）暂未翻译</span>

## <span style="color:#c0392b;">3.3 The conjugate function（共轭函数）</span>

本节引入一个在后面章节扮演重要角色的运算。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">3.3.1 Definition and examples（定义与例子）</span>

<!-- p.105 -->

设 $f:\mathbb{R}^n\to\mathbb{R}$。函数 $f^*:\mathbb{R}^n\to\mathbb{R}$ 定义为

$$
f^*(y)=\sup_{x\in\mathbf{dom}\,f}\bigl(y^Tx-f(x)\bigr), \tag{3.18}
$$

称为 $f$ 的 conjugate（共轭）。共轭函数的定义域由使上确界有限的 $y\in\mathbb{R}^n$ 组成，即 $y^Tx-f(x)$ 在 $\mathbf{dom}\,f$ 上有上界的那些 $y$。（见图 3.8。）

我们立即看到 $f^*$ 是凸函数：它是 $y$ 的一族凸（其实是仿射）函数的逐点上确界。这与 $f$ 是否凸无关。（注意当 $f$ 凸时，下标 $x\in\mathbf{dom}\,f$ 并不需要，因为按约定，对 $x\notin\mathbf{dom}\,f$ 有 $y^Tx-f(x)=-\infty$。）

我们先看几个简单例子，然后给出共轭的运算规则，从而能导出许多常见凸函数共轭的解析表达式。

<span style="color:#00838f;">**[例 3.21]** 我们推导 $\mathbb{R}$ 上一些凸函数的共轭：

- **仿射函数**。$f(x)=ax+b$。作为 $x$ 的函数，$yx-ax-b$ 有界当且仅当 $y=a$，此时它是常数。因此共轭函数 $f^*$ 的定义域是单点集 $\{a\}$，且 $f^*(a)=-b$。
- **负对数**。$f(x)=-\log x$，$\mathbf{dom}\,f=\mathbb{R}_{++}$。函数 $xy+\log x$ 当 $y\ge0$ 时无上界；否则在 $x=-1/y$ 处达到最大。因此 $\mathbf{dom}\,f^*=\{y \mid y<0\}=-\mathbb{R}_{++}$，且对 $y<0$，$f^*(y)=-\log(-y)-1$。
- **指数函数**。$f(x)=e^x$。$xy-e^x$ 当 $y<0$ 时无界；对 $y>0$，在 $x=\log y$ 处达到最大，故 $f^*(y)=y\log y-y$。对 $y=0$，

<!-- p.106 -->

$f^*(y)=\sup_x-e^x=0$。总之 $\mathbf{dom}\,f^*=\mathbb{R}_+$，$f^*(y)=y\log y-y$（约定 $0\log0=0$）。
- **负熵**。$f(x)=x\log x$（$\mathbf{dom}\,f=\mathbb{R}_+$，$f(0)=0$）。函数 $xy-x\log x$ 对所有 $y$ 在 $\mathbb{R}_+$ 上有上界，故 $\mathbf{dom}\,f^*=\mathbb{R}$。它在 $x=e^{y-1}$ 处达到最大，代入得 $f^*(y)=e^{y-1}$。
- **倒数**。$f(x)=1/x$（$\mathbb{R}_{++}$ 上）。$y>0$ 时 $yx-1/x$ 无上界；$y=0$ 时上确界为 0；$y<0$ 时上确界在 $x=(-y)^{-1/2}$ 达到。因此 $f^*(y)=-2(-y)^{1/2}$，$\mathbf{dom}\,f^*=-\mathbb{R}_+$。

<span style="color:#00838f;">**[例 3.22]** **严格凸二次函数。** 考虑 $f(x)=\frac{1}{2}x^TQx$（$Q\in\mathbf{S}^n_{++}$）。函数 $y^Tx-\frac{1}{2}x^TQx$ 作为 $x$ 的函数对所有 $y$ 有上界，在 $x=Q^{-1}y$ 达到最大，故

$$
f^*(y)=\frac{1}{2}y^TQ^{-1}y.
$$

<span style="color:#00838f;">**[例 3.23]** **对数行列式。** 考虑 $f(X)=\log\det X^{-1}$（$\mathbf{S}^n_{++}$ 上）。共轭函数定义为

$$
f^*(Y)=\sup_{X\succ0}\bigl(\mathbf{tr}(YX)+\log\det X\bigr),
$$

因为 $\mathbf{tr}(YX)$ 是 $\mathbf{S}^n$ 上的标准内积。先证除非 $Y\prec0$，$\mathbf{tr}(YX)+\log\det X$ 无上界：若 $Y\not\prec0$，则 $Y$ 有特征向量 $v$（$\|v\|_2=1$）与特征值 $\lambda\ge0$。取 $X=I+tvv^T$，得

$$
\mathbf{tr}(YX)+\log\det X=\mathbf{tr}Y+t\lambda+\log(1+t),
$$

当 $t\to\infty$ 时无上界。再考虑 $Y\prec0$ 的情形：令对 $X$ 的梯度为零可找到最大化点：

$$
\nabla_X\bigl(\mathbf{tr}(YX)+\log\det X\bigr)=Y+X^{-1}=0
$$

（见 §A.4.1），解得 $X=-Y^{-1}$（确为正定）。因此

$$
f^*(Y)=\log\det(-Y)^{-1}-n,\qquad \mathbf{dom}\,f^*=-\mathbf{S}^n_{++}.
$$

<span style="color:#00838f;">**[例 3.24]** **示性函数。** 设 $I_S$ 是（不必凸的）集合 $S\subseteq\mathbb{R}^n$ 的示性函数，即 $I_S(x)=0$（$\mathbf{dom}\,I_S=S$）。其共轭为

$$
I_S^*(y)=\sup_{x\in S}y^Tx,
$$

即集合 $S$ 的支撑函数。

<!-- p.107 -->

<span style="color:#00838f;">**[例 3.25]** **Log-sum-exp 函数。** 为推导 log-sum-exp 函数 $f(x)=\log(\sum_{i=1}^ne^{x_i})$ 的共轭，先确定使 $y^Tx-f(x)$ 关于 $x$ 的最大值能取到的那些 $y$。令对 $x$ 的梯度为零，得条件

$$
y_i=\frac{e^{x_i}}{\sum_{j=1}^ne^{x_j}},\qquad i=1,\dots,n.
$$

这组方程关于 $x$ 可解当且仅当 $y\succ0$ 且 $\mathbf{1}^Ty=1$。把 $y_i$ 的表达式代入 $y^Tx-f(x)$ 得 $f^*(y)=\sum_{i=1}^ny_i\log y_i$。只要 $y\succeq0$ 且 $\mathbf{1}^Ty=1$（约定 $0\log0=0$），即使 $y$ 的某些分量为零，这个 $f^*$ 的表达式仍然正确。

事实上 $f^*$ 的定义域恰为 $\mathbf{1}^Ty=1$、$y\succeq0$。为证之：设 $y$ 有负分量，比如 $y_k<0$，则取 $x_k=-t$、$x_i=0$（$i\neq k$）并令 $t\to\infty$，可证 $y^Tx-f(x)$ 无上界。若 $y\succeq0$ 但 $\mathbf{1}^Ty\neq1$，取 $x=t\mathbf{1}$，则

$$
y^Tx-f(x)=t\,\mathbf{1}^Ty-t-\log n.
$$

若 $\mathbf{1}^Ty>1$，它当 $t\to\infty$ 时无界增长；若 $\mathbf{1}^Ty<1$，当 $t\to-\infty$ 时无界增长。总之：

$$
f^*(y)=\begin{cases}\sum_{i=1}^ny_i\log y_i & \text{若 } y\succeq0 \text{ 且 } \mathbf{1}^Ty=1\\ \infty & \text{否则}. \end{cases}
$$

换言之，log-sum-exp 函数的共轭是限制在概率单纯形上的负熵函数。

<span style="color:#00838f;">**[例 3.26]** **范数。** 设 $\|\cdot\|$ 是 $\mathbb{R}^n$ 上的范数，对偶范数 $\|\cdot\|_*$。我们将证明 $f(x)=\|x\|$ 的共轭是

$$
f^*(y)=\begin{cases}0 & \|y\|_*\le1\\ \infty & \text{否则}，\end{cases}
$$

即范数的共轭是对偶范数单位球的示性函数。若 $\|y\|_*>1$，则由对偶范数定义存在 $z\in\mathbb{R}^n$ 使 $\|z\|\le1$ 且 $y^Tz>1$。取 $x=tz$ 并令 $t\to\infty$：

$$
y^Tx-\|x\|=t(y^Tz-\|z\|)\to\infty,
$$

故 $f^*(y)=\infty$。反之若 $\|y\|_*\le1$，则对所有 $x$ 有 $y^Tx\le\|x\|\|y\|_*$，从而对所有 $x$ 有 $y^Tx-\|x\|\le0$。因此最大化 $y^Tx-\|x\|$ 的是 $x=0$，最大值 0。

<span style="color:#00838f;">**[例 3.27]** **范数平方。** 考虑 $f(x)=(1/2)\|x\|^2$（$\|\cdot\|$ 为范数，对偶范数 $\|\cdot\|_*$）。我们将证明其共轭为 $f^*(y)=(1/2)\|y\|_*^2$。由 $y^Tx\le\|y\|_*\|x\|$ 得

$$
y^Tx-(1/2)\|x\|^2\le\|y\|_*\|x\|-(1/2)\|x\|^2
$$

<!-- p.108 -->

（对所有 $x$）。右边是关于 $\|x\|$ 的二次函数，最大值为 $(1/2)\|y\|_*^2$。因此对所有 $x$：

$$
y^Tx-(1/2)\|x\|^2\le(1/2)\|y\|_*^2,
$$

即 $f^*(y)\le(1/2)\|y\|_*^2$。为证另一侧不等式，取满足 $y^Tx=\|y\|_*\|x\|$ 的向量 $x$，并缩放使 $\|x\|=\|y\|_*$。对这个 $x$：

$$
y^Tx-(1/2)\|x\|^2=(1/2)\|y\|_*^2,
$$

故 $f^*(y)\ge(1/2)\|y\|_*^2$。

<span style="color:#00838f;">**[例 3.28]** **收益与利润函数。** 考虑一个消耗 $n$ 种资源并生产可售产品的企业。令 $r=(r_1,\dots,r_n)$ 表示消耗的资源量向量，$S(r)$ 表示由所生产产品获得的销售收入（作为消耗资源的函数）。设 $p_i$ 是资源 $i$ 的单价，则企业为资源支付的总金额是 $p^Tr$。企业的 profit（利润）为 $S(r)-p^Tr$。固定资源价格，问：通过明智地选择资源消耗量，能获得的最大利润是多少？这个最大利润由

$$
M(p)=\sup_r\bigl(S(r)-p^Tr\bigr)
$$

给出。函数 $M(p)$ 给出作为资源价格函数的可获得最大利润。用共轭函数的语言：$M(p)=(-S)^*(-p)$。因此最大利润（作为资源价格的函数）与总销售额（作为资源消耗的函数）的共轭密切相关。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">3.3.2 Basic properties（基本性质）</span>

**Fenchel 不等式（Fenchel's inequality）。** 由共轭函数的定义立即得到不等式

$$
f(x)+f^*(y)\ge x^Ty
$$

对所有 $x,y$ 成立。这称为 Fenchel's inequality（Fenchel 不等式）（当 $f$ 可微时也称 Young's inequality，杨氏不等式）。例如取 $f(x)=(1/2)x^TQx$（$Q\in\mathbf{S}^n_{++}$），得不等式

$$
x^Ty\le(1/2)x^TQx+(1/2)y^TQ^{-1}y.
$$

<!-- p.108/109 -->

**共轭的共轭（Conjugate of the conjugate）。** 上面的例子与「conjugate」这个名字暗示：凸函数的共轭的共轭是原函数。这只在一个技术性条件下成立：若 $f$ 凸且 closed（闭，即 $\mathbf{epi}\,f$ 是闭集；见 §A.3.3），则 $f^{**}=f$。例如若 $\mathbf{dom}\,f=\mathbb{R}^n$，则 $f^{**}=f$（见习题 3.39）。

**可微函数（Differentiable functions）。** 可微函数 $f$ 的共轭也称为 $f$ 的 Legendre transform（勒让德变换）。（为了区分一般定义与可微情形，有时用术语 Fenchel conjugate 代替 conjugate。）

设 $f$ 凸且可微，$\mathbf{dom}\,f=\mathbb{R}^n$。$y^Tx-f(x)$ 的任何 maximizer（最大化点）$x^*$ 满足 $y=\nabla f(x^*)$；反之，若 $x^*$ 满足 $y=\nabla f(x^*)$，则 $x^*$ 最大化 $y^Tx-f(x)$。因此若 $y=\nabla f(x^*)$，有

$$
f^*(y)=x^{*T}\nabla f(x^*)-f(x^*).
$$

这使我们能对任何「能把梯度方程 $y=\nabla f(z)$ 解出 $z$」的 $y$ 确定 $f^*(y)$。

也可以这样表达：设 $z\in\mathbb{R}^n$ 任意，定义 $y=\nabla f(z)$，则

$$
f^*(y)=z^T\nabla f(z)-f(z).
$$

**缩放与仿射变换复合（Scaling and composition with affine transformation）。** 对 $a>0$、$b\in\mathbb{R}$，$g(x)=af(x)+b$ 的共轭是 $g^*(y)=af^*(y/a)-b$。设 $A\in\mathbb{R}^{n\times n}$ 非奇异，$b\in\mathbb{R}^n$，则 $g(x)=f(Ax+b)$ 的共轭是

$$
g^*(y)=f^*(A^{-T}y)-b^TA^{-T}y,\qquad \mathbf{dom}\,g^*=A^T\mathbf{dom}\,f^*.
$$

**独立函数之和（Sums of independent functions）。** 若 $f(u,v)=f_1(u)+f_2(v)$（$f_1,f_2$ 凸，共轭分别为 $f_1^*,f_2^*$），则

$$
f^*(w,z)=f_1^*(w)+f_2^*(z).
$$

换言之，独立凸函数之和的共轭是共轭之和。（「独立」指它们是不同变量的函数。）

## <span style="color:#c0392b;">3.4 Quasiconvex functions（拟凸函数）</span>

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">3.4.1 Definition and examples（定义与例子）</span>

<!-- p.109 -->

函数 $f:\mathbb{R}^n\to\mathbb{R}$ 称为 quasiconvex（拟凸）的（或 unimodal，单峰的），如果其定义域与它的所有下水平集

$$
S_\alpha=\{x\in\mathbf{dom}\,f \mid f(x)\le\alpha\}\qquad(\alpha\in\mathbb{R})
$$

都是凸的。函数是 quasiconcave（拟凹）的，若 $-f$ 拟凸，即每个上水平集 $\{x \mid f(x)\ge\alpha\}$ 凸。既拟凸又拟凹的函数称为 quasilinear（拟线性）的。若 $f$ 拟线性，则其定义域与每个 level set（水平集）$\{x \mid f(x)=\alpha\}$ 都是凸的。

<!-- p.110 -->

对 $\mathbb{R}$ 上的函数，拟凸性要求每个下水平集是一个区间（可能是无限区间）。图 3.9 展示了 $\mathbb{R}$ 上拟凸函数的例子。

凸函数具有凸的下水平集，因此是拟凸的。但简单的例子（如图 3.9）表明逆命题不成立。

<span style="color:#00838f;">**[例 3.29]** $\mathbb{R}$ 上的一些例子：

- **对数**。$\log x$（$\mathbb{R}_{++}$ 上）拟凸（且拟凹，从而拟线性）。
- **上取整函数**。$\mathbf{ceil}(x)=\inf\{z\in\mathbb{Z} \mid z\ge x\}$ 拟凸（且拟凹）。

这些例子表明拟凸函数可以是凹的，或是不连续的。现在给 $\mathbb{R}^n$ 上的一些例子。

<span style="color:#00838f;">**[例 3.30]** **向量的长度。** 我们定义 $x\in\mathbb{R}^n$ 的 length（长度）为非零分量的最大下标：

$$
f(x)=\max\{i \mid x_i\neq0\}
$$

（零向量的长度定义为零）。这个函数在 $\mathbb{R}^n$ 上拟凸，因为其下水平集是子空间：

$$
f(x)\le\alpha \iff x_i=0,\ i=\lfloor\alpha\rfloor+1,\dots,n.
$$

<span style="color:#00838f;">**[例 3.31]** 考虑 $f:\mathbb{R}^2\to\mathbb{R}$，$\mathbf{dom}\,f=\mathbb{R}^2_+$，$f(x_1,x_2)=x_1x_2$。这个函数既不凸也不凹，因为其 Hessian

$$
\nabla^2f(x)=\begin{bmatrix}0&1\\1&0\end{bmatrix}
$$

<!-- p.111 -->

indefinite（不定）：有一个正特征值与一个负特征值。但 $f$ 是拟凹的，因为上水平集

$$
\{x\in\mathbb{R}^2_+ \mid x_1x_2\ge\alpha\}
$$

对所有 $\alpha$ 都是凸集。（不过注意 $f$ 在 $\mathbb{R}^2$ 上不是拟凹的。）

<span style="color:#00838f;">**[例 3.32]** **线性分式函数。** 函数

$$
f(x)=\frac{a^Tx+b}{c^Tx+d},\qquad \mathbf{dom}\,f=\{x \mid c^Tx+d>0\}
$$

既拟凸又拟凹，即拟线性。其 $\alpha$-下水平集为

$$
\begin{aligned}
S_\alpha &= \{x \mid c^Tx+d>0,\; (a^Tx+b)/(c^Tx+d)\le\alpha\}\\
&= \{x \mid c^Tx+d>0,\; a^Tx+b\le\alpha(c^Tx+d)\},
\end{aligned}
$$

它是凸的，因为它是开半空间与闭半空间的交。（同样的方法可证其上水平集凸。）

<span style="color:#00838f;">**[例 3.33]** **距离比函数。** 设 $a,b\in\mathbb{R}^n$，定义

$$
f(x)=\frac{\|x-a\|_2}{\|x-b\|_2},
$$

即到 $a$ 的欧氏距离与到 $b$ 的距离之比。则 $f$ 在半空间 $\{x \mid \|x-a\|_2\le\|x-b\|_2\}$ 上拟凸。为看出这一点，考虑 $f$ 的 $\alpha$-下水平集（$\alpha\le1$，因为在该半空间上 $f(x)\le1$）。此下水平集是满足

$$
\|x-a\|_2\le\alpha\|x-b\|_2
$$

的点集。两边平方并整理，等价于

$$
(1-\alpha^2)x^Tx-2(a-\alpha^2b)^Tx+a^Ta-\alpha^2b^Tb\le0.
$$

当 $\alpha\le1$ 时这描述一个凸集（其实是一个欧氏球）。

<span style="color:#00838f;">**[例 3.34]** **内部收益率（internal rate of return）。** 设 $x=(x_0,x_1,\dots,x_n)$ 表示 $n$ 个周期上的 cash flow（现金流）序列，$x_i>0$ 表示第 $i$ 期付给我们，$x_i<0$ 表示我们付出。定义现金流在利率 $r\ge0$ 下的 present value（现值）：

$$
\mathrm{PV}(x,r)=\sum_{i=0}^n(1+r)^{-i}x_i
$$

（因子 $(1+r)^{-i}$ 是第 $i$ 期收支的 discount factor（贴现因子））。现在考虑满足 $x_0<0$ 且 $x_0+x_1+\cdots+x_n>0$ 的现金流：这表示第 0 期我们投资 $|x_0|$，且其余现金流总和 $x_1+\cdots+x_n$（不计贴现）超过初始投资。

<!-- p.112 -->

对这样的现金流，$\mathrm{PV}(x,0)>0$ 且当 $r\to\infty$ 时 $\mathrm{PV}(x,r)\to x_0<0$，因此至少存在一个 $r\ge0$ 使 $\mathrm{PV}(x,r)=0$。我们把现金流的 internal rate of return（内部收益率）定义为使现值为零的最小利率：

$$
\mathrm{IRR}(x)=\inf\{r\ge0 \mid \mathrm{PV}(x,r)=0\}.
$$

内部收益率是 $x$ 的拟凹函数（限制在 $x_0<0$、$x_1+\cdots+x_n>0$ 上）。为看出这一点，注意

$$
\mathrm{IRR}(x)\ge R \iff \mathrm{PV}(x,r)>0 \text{ 对 } 0\le r<R.
$$

左边定义了 IRR 的 $R$-上水平集；右边是以 $r$ 索引的集合 $\{x \mid \mathrm{PV}(x,r)>0\}$ 在 $0\le r<R$ 范围上的交。对每个 $r$，$\mathrm{PV}(x,r)>0$ 定义一个开半空间，所以右边定义一个凸集。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">3.4.2 Basic properties（基本性质）</span>

<!-- p.112/113 -->

上面的例子表明拟凸性是凸性的一个相当大的推广。尽管如此，凸函数的许多性质对拟凸函数也成立或有对应物。例如，Jensen 不等式有一个刻画拟凸性的变体：**函数 $f$ 拟凸当且仅当 $\mathbf{dom}\,f$ 凸，且对任意 $x,y\in\mathbf{dom}\,f$ 与 $0\le\theta\le1$，**

$$
f(\theta x+(1-\theta)y)\le\max\{f(x),f(y)\}, \tag{3.19}
$$

即函数在线段上的值不超过它在端点值的最大值。不等式 (3.19) 有时称为 quasiconvex functions 的 Jensen 不等式（拟凸函数的 Jensen 不等式），见图 3.10。

<span style="color:#00838f;">**[例 3.35]** **非负向量的基数。** 向量 $x\in\mathbb{R}^n$ 的 cardinality（基数/大小）是它非零分量的个数，记作 $\mathbf{card}(x)$。函数 $\mathbf{card}$ 在 $\mathbb{R}^n_+$ 上拟凹（但在 $\mathbb{R}^n$ 上不是）。这直接由修正的 Jensen 不等式

$$
\mathbf{card}(x+y)\ge\min\{\mathbf{card}(x),\mathbf{card}(y)\}
$$

得出（对 $x,y\succeq0$ 成立）。

<span style="color:#00838f;">**[例 3.36]** **半正定矩阵的秩。** 函数 $\mathbf{rank}\,X$ 在 $\mathbf{S}^n_+$ 上拟凹。这由修正的 Jensen 不等式 (3.19) 得出：

$$
\mathbf{rank}(X+Y)\ge\min\{\mathbf{rank}\,X,\mathbf{rank}\,Y\}
$$

对 $X,Y\in\mathbf{S}^n_+$ 成立。（这可看作前一例的推广，因为对 $x\succeq0$ 有 $\mathbf{rank}(\mathbf{diag}(x))=\mathbf{card}(x)$。）

<!-- p.113/114 -->

与凸性一样，拟凸性由函数 $f$ 在直线上的行为刻画：$f$ 拟凸当且仅当它限制到任何与其定义域相交的直线上是拟凸的。特别地，拟凸性可以通过把函数限制到任意直线、再检查所得 $\mathbb{R}$ 上函数的拟凸性来验证。

**$\mathbb{R}$ 上的拟凸函数。** 我们可以对 $\mathbb{R}$ 上的拟凸函数给出简单刻画。考虑 continuous（连续）函数，因为在一般情形陈述条件会很繁琐。连续函数 $f:\mathbb{R}\to\mathbb{R}$ 拟凸当且仅当以下条件至少一个成立：

- $f$ 是非减的；
- $f$ 是非增的；
- 存在点 $c\in\mathbf{dom}\,f$，使得对 $t\le c$（且 $t\in\mathbf{dom}\,f$），$f$ 非增；对 $t\ge c$（且 $t\in\mathbf{dom}\,f$），$f$ 非减。

点 $c$ 可取为 $f$ 的任何 global minimizer（全局最小化点）。图 3.11 展示了这一点。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">3.4.3 Differentiable quasiconvex functions（可微拟凸函数）</span>

**一阶条件（First-order conditions）。** 设 $f:\mathbb{R}^n\to\mathbb{R}$ 可微。则 $f$ 拟凸当且仅当 $\mathbf{dom}\,f$ 凸，且对所有 $x,y\in\mathbf{dom}\,f$：

$$
f(y)\le f(x) \implies \nabla f(x)^T(y-x)\le0. \tag{3.20}
$$

<!-- p.114 -->

这是不等式 (3.2) 在拟凸函数上的类比。证明留作习题（习题 3.43）。

条件 (3.20) 在 $\nabla f(x)\neq0$ 时有简单的几何解释：它说 $\nabla f(x)$ 定义了下水平集 $\{y \mid f(y)\le f(x)\}$ 在点 $x$ 处的支撑超平面（见图 3.12）。

虽然凸性的一阶条件 (3.2) 与拟凸性的一阶条件 (3.20) 相似，但存在重要差异。例如：若 $f$ 凸且 $\nabla f(x)=0$，则 $x$ 是 $f$ 的全局最小化点。但这对拟凸函数不成立：可能有 $\nabla f(x)=0$ 而 $x$ 并非 $f$ 的全局最小化点。

**二阶条件（Second-order conditions）。** 现在设 $f$ 二阶可微。若 $f$ 拟凸，则对所有 $x\in\mathbf{dom}\,f$ 与所有 $y\in\mathbb{R}^n$：

$$
y^T\nabla f(x)=0 \implies y^T\nabla^2f(x)y\ge0. \tag{3.21}
$$

对 $\mathbb{R}$ 上的拟凸函数，这退化为简单条件

$$
f'(x)=0 \implies f''(x)\ge0,
$$

<!-- p.115 -->

即在斜率为零的任何点，二阶导数非负。对 $\mathbb{R}^n$ 上的拟凸函数，条件 (3.21) 的解释稍复杂：与 $n=1$ 一样，凡 $\nabla f(x)=0$ 时必有 $\nabla^2f(x)\succeq0$；当 $\nabla f(x)\neq0$ 时，(3.21) 意味着 $\nabla^2f(x)$ 在 $(n-1)$ 维子空间 $\nabla f(x)^\perp$ 上半正定。这蕴含 $\nabla^2f(x)$ 至多有一个负特征值。

作为（部分）逆命题：若 $f$ 满足

$$
y^T\nabla f(x)=0 \implies y^T\nabla^2f(x)y>0 \tag{3.22}
$$

（对所有 $x\in\mathbf{dom}\,f$ 与所有 $y\in\mathbb{R}^n$、$y\neq0$），则 $f$ 拟凸。这个条件等价于：在 $\nabla f(x)=0$ 的点要求 $\nabla^2f(x)$ 正定，在其他所有点要求 $\nabla^2f(x)$ 在 $(n-1)$ 维子空间 $\nabla f(x)^\perp$ 上正定。

**拟凸性二阶条件的证明。** 通过把函数限制到任意直线，只需考虑 $f:\mathbb{R}\to\mathbb{R}$ 的情形。

先证：若 $f:\mathbb{R}\to\mathbb{R}$ 在区间 $(a,b)$ 上拟凸，则必满足 (3.21)，即若 $f'(c)=0$（$c\in(a,b)$），则必有 $f''(c)\ge0$。反设 $f'(c)=0$、$c\in(a,b)$ 而 $f''(c)<0$，则对小的正 $\epsilon$ 有 $f(c-\epsilon)<f(c)$ 且 $f(c+\epsilon)<f(c)$。于是下水平集 $\{x \mid f(x)\le f(c)-\epsilon\}$ 对小的正 $\epsilon$ 是 disconnected（不连通）的，因此不凸，与 $f$ 拟凸的假设矛盾。

再证：若条件 (3.22) 成立，则 $f$ 拟凸。设 (3.22) 成立，即对每个使 $f'(c)=0$ 的 $c\in(a,b)$ 有 $f''(c)>0$。这意味着 $f'$ 每当穿过值 0 时都是严格递增的，因此至多穿过 0 一次。若 $f'$ 完全不穿过 0，则 $f$ 在 $(a,b)$ 上非增或非减，从而拟凸。否则它恰好穿过 0 一次，设在 $c\in(a,b)$。由 $f''(c)>0$ 得：对 $a<t\le c$，$f'(t)\le0$；对 $c\le t<b$，$f'(t)\ge0$。这表明 $f$ 拟凸。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">3.4.4 Operations that preserve quasiconvexity（保持拟凸性的运算）</span>

**非负加权最大值（Nonnegative weighted maximum）。** 拟凸函数的非负加权最大值

$$
f=\max\{w_1f_1,\dots,w_mf_m\}
$$

<!-- p.116 -->

（$w_i\ge0$，$f_i$ 拟凸）是拟凸的。这个性质可推广到一般的逐点上确界：

$$
f(x)=\sup_{y\in C}\bigl(w(y)g(x,y)\bigr),
$$

其中 $w(y)\ge0$ 且 $g(x,y)$ 对每个 $y$ 关于 $x$ 拟凸。这个事实容易验证：$f(x)\le\alpha$ 当且仅当

$$
w(y)g(x,y)\le\alpha \text{ 对所有 } y\in C,
$$

即 $f$ 的 $\alpha$-下水平集是函数 $w(y)g(x,y)$（视为 $x$ 的函数）的 $\alpha$-下水平集之交。

<span style="color:#00838f;">**[例 3.37]** **广义特征值。** 对称矩阵对 $(X,Y)$（$Y\succ0$）的 maximum generalized eigenvalue（最大广义特征值）定义为

$$
\lambda_{\max}(X,Y)=\sup_{u\neq0}\frac{u^TXu}{u^TYu}=\sup\{\lambda \mid \det(\lambda Y-X)=0\}
$$

（见 §A.5.3）。这个函数在 $\mathbf{dom}\,f=\mathbf{S}^n\times\mathbf{S}^n_{++}$ 上拟凸。为看出这一点，考虑表达式

$$
\lambda_{\max}(X,Y)=\sup_{u\neq0}\frac{u^TXu}{u^TYu}.
$$

对每个 $u\neq0$，函数 $u^TXu/u^TYu$ 关于 $(X,Y)$ 是线性分式的，因此是 $(X,Y)$ 的拟凸函数。我们得出 $\lambda_{\max}$ 拟凸，因为它是拟凸函数族的上确界。

**复合（Composition）。** 若 $g:\mathbb{R}^n\to\mathbb{R}$ 拟凸且 $h:\mathbb{R}\to\mathbb{R}$ 非减，则 $f=h\circ g$ 拟凸。

拟凸函数与仿射或线性分式变换的复合产生拟凸函数。若 $f$ 拟凸，则 $g(x)=f(Ax+b)$ 拟凸，且 $\tilde{g}(x)=f((Ax+b)/(c^Tx+d))$ 在集合

$$
\{x \mid c^Tx+d>0,\; (Ax+b)/(c^Tx+d)\in\mathbf{dom}\,f\}
$$

上拟凸。

<!-- p.116/117 -->

**最小化（Minimization）。** 若 $f(x,y)$ 关于 $x,y$ 联合拟凸且 $C$ 是凸集，则函数

$$
g(x)=\inf_{y\in C}f(x,y)
$$

拟凸。为证明，须证 $\{x \mid g(x)\le\alpha\}$ 凸（$\alpha\in\mathbb{R}$ 任意）。由 $g$ 的定义，$g(x)\le\alpha$ 当且仅当对任意 $\epsilon>0$ 存在

<!-- p.117 -->

$y\in C$ 使 $f(x,y)\le\alpha+\epsilon$。设 $x_1,x_2$ 是 $g$ 的 $\alpha$-下水平集中两点，则对任意 $\epsilon>0$ 存在 $y_1,y_2\in C$ 使

$$
f(x_1,y_1)\le\alpha+\epsilon,\qquad f(x_2,y_2)\le\alpha+\epsilon,
$$

且由于 $f$ 关于 $x,y$ 拟凸，对 $0\le\theta\le1$ 也有

$$
f(\theta x_1+(1-\theta)x_2,\; \theta y_1+(1-\theta)y_2)\le\alpha+\epsilon.
$$

因此 $g(\theta x_1+(1-\theta)x_2)\le\alpha$，即证 $\{x \mid g(x)\le\alpha\}$ 凸。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">3.4.5 Representation via family of convex functions（通过凸函数族的表示）</span>

<!-- p.117/118 -->

在后文中，用凸函数的不等式来表示拟凸函数 $f$ 的（凸的）下水平集会很方便。我们寻找一族由 $t\in\mathbb{R}$ 索引的凸函数 $\phi_t:\mathbb{R}^n\to\mathbb{R}$，满足

$$
f(x)\le t \iff \phi_t(x)\le0, \tag{3.23}
$$

即拟凸函数 $f$ 的 $t$-下水平集是凸函数 $\phi_t$ 的 0-下水平集。显然 $\phi_t$ 须满足：对所有 $x\in\mathbb{R}^n$，

$$
\phi_t(x)\le0 \implies \phi_s(x)\le0 \text{ 对 } s\ge t.
$$

若对每个 $x$，$\phi_t(x)$ 是 $t$ 的非增函数（即 $s\ge t$ 时 $\phi_s(x)\le\phi_t(x)$），则上式满足。

为看到这种表示总存在，可取

$$
\phi_t(x)=\begin{cases}0 & f(x)\le t\\ \infty & \text{否则}，\end{cases}
$$

即 $\phi_t$ 是 $f$ 的 $t$-下水平集的示性函数。显然这种表示不唯一：例如若 $f$ 的下水平集是闭的，可取

$$
\phi_t(x)=\mathbf{dist}\bigl(x,\{z \mid f(z)\le t\}\bigr).
$$

我们通常关心有良好性质（如可微性）的函数族 $\phi_t$。

<span style="color:#00838f;">**[例 3.38]** **凸比凹函数。** 设 $p$ 是凸函数，$q$ 是凹函数，在凸集 $C$ 上 $p(x)\ge0$、$q(x)>0$。则定义在 $C$ 上的函数 $f(x)=p(x)/q(x)$ 拟凸。这里

$$
f(x)\le t \iff p(x)-tq(x)\le0,
$$

所以可取 $\phi_t(x)=p(x)-tq(x)$（$t\ge0$）。对每个 $t$，$\phi_t$ 凸；对每个 $x$，$\phi_t(x)$ 关于 $t$ 递减。

## <span style="color:#c0392b;">3.5 Log-concave and log-convex functions（对数凹与对数凸函数）</span>

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">3.5.1 Definition（定义）</span>

<!-- p.118 -->

函数 $f:\mathbb{R}^n\to\mathbb{R}$ 称为 logarithmically concave（对数凹，log-concave）的，如果 $f(x)>0$ 对所有 $x\in\mathbf{dom}\,f$ 且 $\log f$ 凹。称为 logarithmically convex（对数凸，log-convex）的，如果 $\log f$ 凸。因此 $f$ 对数凸当且仅当 $1/f$ 对数凹。允许 $f$ 取零值会带来方便，此时取 $\log f(x)=-\infty$；在这种情况下，若扩值函数 $\log f$ 凹，则称 $f$ 对数凹。

我们可以不用对数直接表达对数凹性：定义域凸且 $f(x)>0$（对所有 $x\in\mathbf{dom}\,f$）的函数 $f:\mathbb{R}^n\to\mathbb{R}$ 对数凹，当且仅当对所有 $x,y\in\mathbf{dom}\,f$ 与 $0\le\theta\le1$：

$$
f(\theta x+(1-\theta)y)\ge f(x)^\theta f(y)^{1-\theta}.
$$

特别地，对数凹函数在两点平均值处的值至少是它在两点取值的 geometric mean（几何平均）。

由复合规则知道 $e^h$ 在 $h$ 凸时凸，所以对数凸函数是凸的。类似地，非负的凹函数是对数凹的。同样显然：由于对数单调递增，对数凸函数拟凸，对数凹函数拟凹。

<span style="color:#00838f;">**[例 3.39]** 一些简单的对数凹与对数凸函数例子：

- **仿射函数**。$f(x)=a^Tx+b$ 在 $\{x \mid a^Tx+b>0\}$ 上对数凹。
- **幂函数**。$f(x)=x^a$（$\mathbb{R}_{++}$ 上）当 $a\le0$ 时对数凸，当 $a\ge0$ 时对数凹。
- **指数函数**。$f(x)=e^{ax}$ 既对数凸又对数凹。
- **高斯密度的累积分布函数**，

$$
\Phi(x)=\frac{1}{\sqrt{2\pi}}\int_{-\infty}^xe^{-u^2/2}\,du,
$$

对数凹（见习题 3.54）。
- **Gamma 函数**，

$$
\Gamma(x)=\int_0^\infty u^{x-1}e^{-u}\,du,
$$

对 $x\ge1$ 对数凸（见习题 3.52）。
- **行列式**。$\det X$ 在 $\mathbf{S}^n_{++}$ 上对数凹。
- **行列式比迹**。$\det X/\,\mathbf{tr}\,X$ 在 $\mathbf{S}^n_{++}$ 上对数凹（见习题 3.49）。

<!-- p.118/119 -->

<span style="color:#00838f;">**[例 3.40]** **对数凹的密度函数。** 许多常见的概率密度函数都是对数凹的。两个例子：multivariate normal distribution（多元正态分布）

$$
f(x)=\frac{1}{\sqrt{(2\pi)^n\det\Sigma}}e^{-\frac{1}{2}(x-\bar{x})^T\Sigma^{-1}(x-\bar{x})}
$$

<!-- p.119 -->

（其中 $\bar{x}\in\mathbb{R}^n$，$\Sigma\in\mathbf{S}^n_{++}$），以及 $\mathbb{R}^n_+$ 上的 exponential distribution（指数分布）

$$
f(x)=\Bigl(\prod_{i=1}^n\lambda_i\Bigr)e^{-\lambda^Tx}
$$

（其中 $\lambda\succ0$）。另一个例子是凸集 $C$ 上的 uniform distribution（均匀分布）：

$$
f(x)=\begin{cases}1/\alpha & x\in C\\ 0 & x\notin C,\end{cases}
$$

其中 $\alpha=\mathbf{vol}(C)$ 是 $C$ 的 volume（体积，Lebesgue 测度）。此时 $\log f$ 在 $C$ 外取值 $-\infty$、在 $C$ 上取 $-\log\alpha$，因此凹。

一个更奇特（exotic）的例子是 Wishart distribution（Wishart 分布），定义如下：设 $x_1,\dots,x_p\in\mathbb{R}^n$ 是独立的零均值、协方差 $\Sigma\in\mathbf{S}^n$ 的 Gaussian random vectors（高斯随机向量），$p>n$。随机矩阵 $X=\sum_{i=1}^px_ix_i^T$ 具有 Wishart 密度

$$
f(X)=a\,(\det X)^{(p-n-1)/2}e^{-\frac{1}{2}\mathbf{tr}(\Sigma^{-1}X)},
$$

$\mathbf{dom}\,f=\mathbf{S}^n_{++}$，$a$ 为正常数。Wishart 密度对数凹，因为

$$
\log f(X)=\log a+\frac{p-n-1}{2}\log\det X-\frac{1}{2}\mathbf{tr}(\Sigma^{-1}X)
$$

是 $X$ 的凹函数。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">3.5.2 Properties（性质）</span>

**二阶可微的对数凸/凹函数。** 设 $f$ 二阶可微，$\mathbf{dom}\,f$ 凸，则

$$
\nabla^2\log f(x)=\frac{1}{f(x)}\nabla^2f(x)-\frac{1}{f(x)^2}\nabla f(x)\nabla f(x)^T.
$$

我们得出：$f$ 对数凸当且仅当对所有 $x\in\mathbf{dom}\,f$，

$$
f(x)\nabla^2f(x)\succeq\nabla f(x)\nabla f(x)^T,
$$

对数凹当且仅当对所有 $x\in\mathbf{dom}\,f$，

$$
f(x)\nabla^2f(x)\preceq\nabla f(x)\nabla f(x)^T.
$$

**乘法、加法与积分。** 对数凸性与对数凹性在乘法与正缩放下封闭。例如若 $f,g$ 对数凹，则逐点乘积 $h(x)=f(x)g(x)$ 也对数凹，因为 $\log h(x)=\log f(x)+\log g(x)$，而 $\log f(x)$ 与 $\log g(x)$ 是 $x$ 的凹函数。

<!-- p.119/120 -->

简单的例子表明对数凹函数之和一般不对数凹。但 log-convexity（对数凸性）在求和下保持。设 $f,g$ 对数凸，即 $F=\log f$ 与 $G=\log g$ 凸。由凸函数的复合规则，

$$
\log(\exp F+\exp G)=\log(f+g)
$$

<!-- p.120 -->

凸。因此两个对数凸函数之和对数凸。

更一般地，若 $f(x,y)$ 对每个 $y\in C$ 关于 $x$ 对数凸，则

$$
g(x)=\int_Cf(x,y)\,dy
$$

对数凸。

<span style="color:#00838f;">**[例 3.41]** **非负函数的拉普拉斯变换以及矩母函数与累积量母函数。** 设 $p:\mathbb{R}^n\to\mathbb{R}$ 满足 $p(x)\ge0$（对所有 $x$）。$p$ 的 Laplace transform（拉普拉斯变换）

$$
P(z)=\int p(x)e^{-z^Tx}\,dx
$$

在 $\mathbb{R}^n$ 上对数凸（$\mathbf{dom}\,P$ 自然是 $\{z \mid P(z)<\infty\}$）。现在设 $p$ 是一个 density（密度），即满足 $\int p(x)\,dx=1$。函数 $M(z)=P(-z)$ 称为该密度的 moment generating function（矩母函数）。其名称来自：密度的各阶 moment（矩）可由矩母函数在 $z=0$ 处的导数求出，例如

$$
\nabla M(0)=\mathbb{E}\,v,\qquad \nabla^2M(0)=\mathbb{E}\,vv^T,
$$

其中 $v$ 是密度为 $p$ 的随机变量。函数 $\log M(z)$（凸）称为 $p$ 的 cumulant generating function（累积量母函数），因为它的导数给出密度的 cumulants（累积量）。例如累积量母函数在零处的一阶与二阶导数分别是相应随机变量的均值与协方差：

$$
\nabla\log M(0)=\mathbb{E}\,v,\qquad \nabla^2\log M(0)=\mathbb{E}(v-\mathbb{E}\,v)(v-\mathbb{E}\,v)^T.
$$

**对数凹函数的积分。** 在某些特殊情形，对数凹性在积分下保持。若 $f:\mathbb{R}^n\times\mathbb{R}^m\to\mathbb{R}$ 对数凹，则

$$
g(x)=\int f(x,y)\,dy
$$

是 $x$ 的对数凹函数（$\mathbb{R}^n$ 上）。（这里积分在 $\mathbb{R}^m$ 上进行。）这个结果的证明不简单；见参考文献。

<!-- p.120/121 -->

这个结果有许多重要推论，本节其余部分描述其中一些。例如它蕴含：对数凹概率密度的 marginal distributions（边际分布）对数凹。它还蕴含对数凹性对 convolution（卷积）封闭：若 $f,g$ 在 $\mathbb{R}^n$ 上对数凹，则卷积

$$
(f*g)(x)=\int f(x-y)g(y)\,dy
$$

也对数凹。（为看出这一点，注意 $g(y)$ 与 $f(x-y)$ 关于 $(x,y)$ 对数凹，因此乘积 $f(x-y)g(y)$ 也是；再应用积分结果。）

<!-- p.121 -->

设 $C\subseteq\mathbb{R}^n$ 是凸集，$w$ 是 $\mathbb{R}^n$ 中具有对数凹概率密度 $p$ 的随机向量。则函数

$$
f(x)=\mathbf{prob}(x+w\in C)
$$

关于 $x$ 对数凹。为看出这一点，把 $f$ 表示为

$$
f(x)=\int g(x+w)p(w)\,dw,
$$

其中

$$
g(u)=\begin{cases}1 & u\in C\\ 0 & u\notin C\end{cases}
$$

（对数凹），再应用积分结果。

<span style="color:#00838f;">**[例 3.42]** 概率密度函数 $f:\mathbb{R}^n\to\mathbb{R}$ 的 cumulative distribution function（累积分布函数）定义为

$$
F(x)=\mathbf{prob}(w\preceq x)=\int_{-\infty}^{x_n}\cdots\int_{-\infty}^{x_1}f(z)\,dz_1\cdots dz_n,
$$

其中 $w$ 是密度为 $f$ 的随机变量。若 $f$ 对数凹，则 $F$ 对数凹。我们已经遇到过特例：高斯随机变量的累积分布函数

$$
f(x)=\frac{1}{\sqrt{2\pi}}\int_{-\infty}^xe^{-t^2/2}\,dt
$$

对数凹。（见例 3.39 与习题 3.54。）

<span style="color:#00838f;">**[例 3.43]** **良率函数（yield function）。** 设 $x\in\mathbb{R}^n$ 表示某制造产品的一组参数的 nominal（标称）或 target（目标）值。制造过程的变化使产品制造出来时参数取值 $x+w$，其中 $w\in\mathbb{R}^n$ 是表示制造偏差的随机向量，通常假设均值为零。制造过程的 yield（良率）作为标称参数值的函数为

$$
Y(x)=\mathbf{prob}(x+w\in S),
$$

其中 $S\subseteq\mathbb{R}^n$ 表示产品的可接受参数值集合，即产品规格。

<!-- p.121/122 -->

若制造误差 $w$ 的密度对数凹（例如高斯）且产品规格集合 $S$ 凸，则良率函数 $Y$ 对数凹。这蕴含 α-yield region（α-良率区域）——良率超过 $\alpha$ 的标称参数集合——是凸的。例如 95% 良率区域

$$
\{x \mid Y(x)\ge0.95\}=\{x \mid \log Y(x)\ge\log0.95\}
$$

凸，因为它是凹函数 $\log Y$ 的上水平集。

<span style="color:#00838f;">**[例 3.44]** **多面体的体积。** 设 $A\in\mathbb{R}^{m\times n}$。定义

$$
P_u=\{x\in\mathbb{R}^n \mid Ax\preceq u\}.
$$

则其体积 $\mathbf{vol}\,P_u$ 是 $u$ 的对数凹函数。为证明，注意函数

$$
\Psi(x,u)=\begin{cases}1 & Ax\preceq u\\ 0 & \text{否则}\end{cases}
$$

对数凹。由积分结果得

$$
\int\Psi(x,u)\,dx=\mathbf{vol}\,P_u
$$

对数凹。

> <span style="color:#2471a3;">**[译者注｜跨课程连接]** §3.5 与信息论关系最紧密：高斯密度、指数族分布全部对数凹——这正是 MacKay 书里「对数凹分布的边际与卷积仍对数凹」等结论的分析基础；而矩母函数（例 3.41）就是大偏差与 Chernoff 界的入口。例 3.43 的良率函数则是半导体制造统计过程控制的经典模型。</span>

## <span style="color:#c0392b;">3.6 Convexity with respect to generalized inequalities（关于广义不等式的凸性）</span>

<!-- p.122 -->

现在考虑 monotonicity（单调性）与 convexity（凸性）概念的推广：用广义不等式代替 $\mathbb{R}$ 上通常的序。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">3.6.1 Monotonicity with respect to a generalized inequality（关于广义不等式的单调性）</span>

设 $K\subseteq\mathbb{R}^n$ 是真锥，具有相应的广义不等式 $\preceq_K$。函数 $f:\mathbb{R}^n\to\mathbb{R}$ 称为 $K$-nondecreasing（$K$-非减）的，如果

$$
x\preceq_K y \implies f(x)\le f(y),
$$

称为 $K$-increasing（$K$-递增）的，如果

$$
x\preceq_K y,\; x\neq y \implies f(x)<f(y).
$$

类似地定义 $K$-nonincreasing（$K$-非增）与 $K$-decreasing（$K$-递减）函数。

<span style="color:#00838f;">**[例 3.45]** **单调向量函数。** 函数 $f:\mathbb{R}^n\to\mathbb{R}$ 关于 $\mathbb{R}^n_+$ 非减，当且仅当

$$
x_1\le y_1,\dots,x_n\le y_n \implies f(x)\le f(y)
$$

对所有 $x,y$ 成立。这等价于说：$f$ 限制到任何分量 $x_i$ 上（即把 $x_i$ 看作变量、$x_j$（$j\neq i$）固定）是非减的。

<span style="color:#00838f;">**[例 3.46]** **矩阵单调函数。** 函数 $f:\mathbf{S}^n\to\mathbb{R}$ 称为 matrix monotone（矩阵单调，递增、递减）的，如果它关于半正定锥单调。关于变量 $X\in\mathbf{S}^n$ 的矩阵单调函数的一些例子：

<!-- p.123 -->

- $\mathbf{tr}(WX)$（$W\in\mathbf{S}^n$）：当 $W\succeq0$ 时矩阵非减，当 $W\succ0$ 时矩阵递增（$W\preceq0$ 时矩阵非增，$W\prec0$ 时矩阵递减）。
- $\mathbf{tr}(X^{-1})$ 在 $\mathbf{S}^n_{++}$ 上矩阵递减。
- $\det X$ 在 $\mathbf{S}^n_{++}$ 上矩阵递增，在 $\mathbf{S}^n_+$ 上矩阵非减。

**单调性的梯度条件（Gradient conditions for monotonicity）。** 回顾：定义域凸（即区间）的可微函数 $f:\mathbb{R}\to\mathbb{R}$ 非减当且仅当 $f'(x)\ge0$ 对所有 $x\in\mathbf{dom}\,f$；递增若 $f'(x)>0$ 对所有 $x\in\mathbf{dom}\,f$（但逆命题不成立）。这些条件容易推广到关于广义不等式的单调性。定义域凸的可微函数 $f$ 是 $K$-非减的，当且仅当

$$
\nabla f(x)\succeq_{K^*}0 \tag{3.24}
$$

对所有 $x\in\mathbf{dom}\,f$。注意与简单标量情形的差别：梯度必须在对偶不等式意义下非负。对严格情形有：若

$$
\nabla f(x)\succ_{K^*}0 \tag{3.25}
$$

对所有 $x\in\mathbf{dom}\,f$，则 $f$ 是 $K$-递增的。与标量情形一样，逆命题不成立。

我们来证明这些单调性的一阶条件。先设 $f$ 对所有 $x$ 满足 (3.24)，但不是 $K$-非减的，即存在 $x,y$ 使 $x\preceq_K y$ 且 $f(y)<f(x)$。由 $f$ 的可微性，存在 $t\in[0,1]$ 使

$$
\frac{d}{dt}f(x+t(y-x))=\nabla f(x+t(y-x))^T(y-x)<0.
$$

由于 $y-x\in K$，这意味着

$$
\nabla f(x+t(y-x))\notin K^*,
$$

与「(3.24) 处处满足」的假设矛盾。类似地可证 (3.25) 蕴含 $f$ 是 $K$-递增的。

也容易看出 (3.24) 处处成立是必要的。设 (3.24) 对 $x=z$ 不成立。由对偶锥的定义，存在 $v\in K$ 使

$$
\nabla f(z)^Tv<0.
$$

考虑 $h(t)=f(z+tv)$ 作为 $t$ 的函数。我们有 $h'(0)=\nabla f(z)^Tv<0$，因此存在 $t>0$ 使 $h(t)=f(z+tv)<h(0)=f(z)$，即 $f$ 不是 $K$-非减的。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">3.6.2 Convexity with respect to a generalized inequality（关于广义不等式的凸性）</span>

<!-- p.123/124 -->

设 $K\subseteq\mathbb{R}^m$ 是真锥，具有相应的广义不等式 $\preceq_K$。我们说 $f:\mathbb{R}^n\to\mathbb{R}^m$ 是 $K$-convex（$K$-凸）的，如果对所有 $x,y$ 与 $0\le\theta\le1$：

$$
f(\theta x+(1-\theta)y)\preceq_K\theta f(x)+(1-\theta)f(y).
$$

<!-- p.124 -->

若

$$
f(\theta x+(1-\theta)y)\prec_K\theta f(x)+(1-\theta)f(y)
$$

对所有 $x\neq y$ 与 $0<\theta<1$ 成立，则称函数 strictly $K$-convex（严格 $K$-凸）。当 $m=1$（且 $K=\mathbb{R}_+$）时这些定义退化为普通凸性与严格凸性。

<span style="color:#00838f;">**[例 3.47]** **关于逐分量不等式的凸性。** 函数 $f:\mathbb{R}^n\to\mathbb{R}^m$ 关于逐分量不等式（即 $\mathbb{R}^m_+$ 诱导的广义不等式）凸，当且仅当对所有 $x,y$ 与 $0\le\theta\le1$，

$$
f(\theta x+(1-\theta)y)\preceq\theta f(x)+(1-\theta)f(y),
$$

即每个分量 $f_i$ 是凸函数。$f$ 关于逐分量不等式严格凸当且仅当每个分量 $f_i$ 严格凸。

<span style="color:#00838f;">**[例 3.48]** **矩阵凸性（matrix convexity）。** 设 $f$ 是对称矩阵值函数，即 $f:\mathbb{R}^n\to\mathbf{S}^m$。函数 $f$ 关于矩阵不等式凸，如果

$$
f(\theta x+(1-\theta)y)\preceq\theta f(x)+(1-\theta)f(y)
$$

对任意 $x,y$ 与 $\theta\in[0,1]$ 成立。这有时称为 matrix convexity（矩阵凸性）。等价定义是：标量函数 $z^Tf(x)z$ 对所有向量 $z$ 凸。

<!-- p.125 -->

（这通常是证明矩阵凸性的好办法。）矩阵函数严格矩阵凸，如果当 $x\neq y$、$0<\theta<1$ 时

$$
f(\theta x+(1-\theta)y)\prec\theta f(x)+(1-\theta)f(y),
$$

或等价地，$z^Tfz$ 对每个 $z\neq0$ 严格凸。一些例子：

- 函数 $f(X)=XX^T$（$X\in\mathbb{R}^{n\times m}$）矩阵凸，因为对固定的 $z$，函数 $z^TXX^Tz=\|X^Tz\|_2^2$ 是 $X$（的分量）的凸二次函数。同理 $f(X)=X^2$ 在 $\mathbf{S}^n$ 上矩阵凸。
- 函数 $X^p$ 在 $\mathbf{S}^n_{++}$ 上当 $1\le p\le2$ 或 $-1\le p\le0$ 时矩阵凸，当 $0\le p\le1$ 时矩阵凹。
- 函数 $f(X)=e^X$ 在 $\mathbf{S}^n$ 上不矩阵凸（$n\ge2$）。

凸函数的许多结果可以推广到 $K$-凸函数。举一个简单例子：函数 $K$-凸当且仅当它限制到其定义域内任何直线上是 $K$-凸的。本节其余部分列出几个后面会用到的 $K$-凸性结果；更多结果见习题。

**$K$-凸性的对偶刻画。** 函数 $f$ 是 $K$-凸的，当且仅当对每个 $w\succeq_{K^*}0$，（实值的）函数 $w^Tf$ 凸（普通意义下）；$f$ 严格 $K$-凸当且仅当对每个非零 $w\succeq_{K^*}0$，函数 $w^Tf$ 严格凸。（这些直接由对偶不等式的定义与性质得出。）

<!-- p.125 -->

**可微的 $K$-凸函数。** 可微函数 $f$ 是 $K$-凸的，当且仅当其定义域凸，且对所有 $x,y\in\mathbf{dom}\,f$：

$$
f(y)\succeq_K f(x)+Df(x)(y-x).
$$

（这里 $Df(x)\in\mathbb{R}^{m\times n}$ 是 $f$ 在 $x$ 处的 derivative（导数）或 Jacobian matrix（雅可比矩阵）；见 §A.4.1。）$f$ 严格 $K$-凸当且仅当对所有 $x,y\in\mathbf{dom}\,f$、$x\neq y$：

$$
f(y)\succ_K f(x)+Df(x)(y-x).
$$

**复合定理（Composition theorem）。** 复合的许多结果可以推广到 $K$-凸性。例如：若 $g:\mathbb{R}^n\to\mathbb{R}^p$ 是 $K$-凸的，$h:\mathbb{R}^p\to\mathbb{R}$ 凸，且 $\tilde{h}$（$h$ 的扩值延拓）是 $K$-非减的，则 $h\circ g$ 凸。这推广了「凸函数的非减凸函数是凸函数」这一事实。条件「$\tilde{h}$ 是 $K$-非减的」蕴含 $\mathbf{dom}\,h-K=\mathbf{dom}\,h$。

<span style="color:#00838f;">**[例 3.49]** 由 $g(X)=X^TAX+B^TX+X^TB+C$ 定义的二次矩阵函数 $g:\mathbb{R}^{m\times n}\to\mathbf{S}^n$（$A\in\mathbf{S}^m$，$B\in\mathbb{R}^{m\times n}$，$C\in\mathbf{S}^n$）当 $A\succeq0$ 时凸。函数 $h(Y)=-\log\det(-Y)$（$h:\mathbf{S}^n\to\mathbb{R}$）在 $\mathbf{dom}\,h=-\mathbf{S}^n_{++}$ 上凸且递增。由复合定理得：

$$
f(X)=-\log\det\bigl(-(X^TAX+B^TX+X^TB+C)\bigr)
$$

在

$$
\mathbf{dom}\,f=\{X\in\mathbb{R}^{m\times n} \mid X^TAX+B^TX+X^TB+C\prec0\}
$$

上凸。这推广了：当 $a\ge0$ 时，

$$
-\log(-(ax^2+bx+c))
$$

在 $\{x\in\mathbb{R} \mid ax^2+bx+c<0\}$ 上凸。

<!-- p.126 -->

## <span style="color:#c0392b;">Bibliography（文献注释）</span>

凸分析的标准参考是 Rockafellar [Roc70]。其他关于凸函数的书有 Stoer 与 Witzgall [SW70]、Roberts 与 Varberg [RV73]、Van Tiel [vT84]、Hiriart-Urruty 与 Lemaréchal [HUL93]、Ekeland 与 Témam [ET99]、Borwein 与 Lewis [BL00]、Florenzano 与 Le Van [FL01]、Barvinok [Bar02]、Bertsekas, Nedić 与 Ozdaglar [Ber03]。大多数非线性规划教材也有凸函数章节（例如 Mangasarian [Man94]、Bazaraa, Sherali 与 Shetty [BSS93]、Bertsekas [Ber99]、Polyak [Pol87]、Peressini, Sullivan 与 Uhl [PSU88]）。

Jensen 不等式出现于 [Jen06]。以 Jensen 不等式为核心的不等式一般研究见 Hardy, Littlewood 与 Pólya [HLP52] 以及 Beckenbach 与 Bellman [BB65]。

「perspective function（透视函数）」这一术语来自 Hiriart-Urruty 与 Lemaréchal [HUL93, volume 1, page 100]。例 3.19 的定义（相对熵与 Kullback-Leibler 散度）及相关的习题 3.13 见 Cover 与 Thomas [CT91]。

关于拟凸函数（以及凸性的其他推广）的一些重要早期文献有 Nikaidō [Nik54]、Mangasarian [Man94, chapter 9]、Arrow 与 Enthoven [AE61]、Ponstein [Pon67]、Luenberger [Lue68]。更全面的参考文献列表见 Bazaraa, Sherali 与 Shetty [BSS93, page 126]。

Prékopa [Pré80] 给出对数凹函数的综述。拉普拉斯变换的对数凸性见 Barndorff-Nielsen [BN78, §7]。对数凹函数积分结果的证明见 Prékopa [Pré71, Pré73]。

广义不等式在最近的锥规划文献中被大量使用，始于 Nesterov 与 Nemirovski [NN94, page 156]；另见 Ben-Tal 与 Nemirovski [BTN01] 及第 4 章末的参考文献。关于广义不等式的凸性也出现于 Luenberger [Lue69, §8.2] 与 Isii [Isi64] 的工作中。矩阵单调性与矩阵凸性归于 Löwner [Löw34]，详细讨论见 Davis [Dav63]、Roberts 与 Varberg [RV73, page 216]、Marshall 与 Olkin [MO79, §16E]。例 3.48 中 $X^p$ 的凸性与凹性结果见 Bondar [Bon94, theorem 16.1]。$e^X$ 不矩阵凸的简单例子见 Marshall 与 Olkin [MO79, page 474]。

---

> <span style="color:#7f8c8d;">第 3 章正文（§3.1–3.6）与文献注释全部翻译完毕。习题（PDF p.127–140）暂未翻译。下一章：boyd_ch04（Convex optimization problems，凸优化问题，PDF p.141–228）。</span>
