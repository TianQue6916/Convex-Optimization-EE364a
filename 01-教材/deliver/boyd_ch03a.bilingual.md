# <span style="color:#c0392b;">Boyd《凸优化》第 3 章 凸函数（上：§3.1–3.2）</span>

> <span style="color:#7f8c8d;">Source: 教材/Convex Optimization (Boyd, Vandenberghe).pdf · Chapter 3 §3.1–3.2, pp.67–90 (PDF p.81–104) · 行内双语翻译版 · 译者：ZCode（bilingual-translator 铁律）</span>

## <span style="color:#c0392b;">3.1 Basic properties and examples（基本性质与例子）</span>

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">3.1.1 Definition（定义）</span>

<!-- p.81 -->

函数 $f:\mathbb{R}^n\to\mathbb{R}$ 是 convex（凸）的，如果 $\mathbf{dom}\,f$ 是凸集，且对所有 $x,y\in\mathbf{dom}\,f$ 与满足 $0\le\theta\le1$ 的 $\theta$，有

```math
f(\theta x+(1-\theta)y) \le \theta f(x)+(1-\theta)f(y). \tag{3.1}
```

几何上，这个不等式意味着 $(x,f(x))$ 与 $(y,f(y))$ 之间的线段——即从 $x$ 到 $y$ 的 chord（弦）——位于 $f$ 的图像之上（图 3.1）。

若当 $x\neq y$ 且 $0<\theta<1$ 时 (3.1) 中严格不等号成立，则称 $f$ 是 strictly convex（严格凸）的。若 $-f$ 是凸的，则称 $f$ 是 concave（凹）的；若 $-f$ 严格凸，则称 $f$ 是 strictly concave（严格凹）的。

对 affine function（仿射函数），(3.1) 中总取等号，所以所有仿射（从而也包括线性）函数既凸又凹。反之，任何既凸又凹的函数都是仿射的。

函数是凸的，当且仅当它限制到任何与其定义域相交的直线上都是凸的。换言之，$f$ 凸当且仅当对所有 $x\in\mathbf{dom}\,f$ 与所有 $v$，函数 $g(t)=f(x+tv)$ 是凸的（在其定义域 $\{t \mid x+tv\in\mathbf{dom}\,f\}$ 上）。这个性质非常有用：它允许我们通过把函数限制到直线上来检验凸性。

凸函数分析是一个发展成熟的领域，我们不做深入展开。一个简单结果是：凸函数在其定义域的 relative interior（相对内部）上是 continuous（连续）的；它只可能在相对边界上不连续。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">3.1.2 Extended-value extensions（扩值延拓）</span>

<!-- p.82 -->

把凸函数延拓到整个 $\mathbb{R}^n$（在定义域之外取值 $\infty$）常常很方便。若 $f$ 凸，我们定义其 extended-value extension（扩值延拓）$\tilde{f}:\mathbb{R}^n\to\mathbb{R}\cup\{\infty\}$：

```math
\tilde{f}(x) = \begin{cases} f(x) & x\in\mathbf{dom}\,f\\ \infty & x\notin\mathbf{dom}\,f. \end{cases}
```

延拓 $\tilde{f}$ 定义在整个 $\mathbb{R}^n$ 上，取值于 $\mathbb{R}\cup\{\infty\}$。由延拓 $\tilde{f}$ 可恢复原函数 $f$ 的定义域：$\mathbf{dom}\,f=\{x \mid \tilde{f}(x)<\infty\}$。

延拓可以简化记号：不再需要显式描述定义域，也不必在每次提到 $f(x)$ 时加上限定语「对所有 $x\in\mathbf{dom}\,f$」。例如基本定义不等式 (3.1)，用延拓 $\tilde{f}$ 可以表述为：对 $0<\theta<1$，

```math
\tilde{f}(\theta x+(1-\theta)y)\le\theta\tilde{f}(x)+(1-\theta)\tilde{f}(y)
```

对任意 $x,y$ 成立（$\theta=0$ 或 $\theta=1$ 时不等式恒成立）。当然这里必须用 extended arithmetic（扩展算术）与扩展序来解释该不等式。当 $x,y$ 都属于 $\mathbf{dom}\,f$ 时，它与 (3.1) 一致；若任一在定义域外，则右边为 $\infty$，不等式自然成立。作为这种记号技巧的另一个例子：设 $f_1,f_2$ 是 $\mathbb{R}^n$ 上两个凸函数，逐点求和 $f=f_1+f_2$ 的定义域是 $\mathbf{dom}\,f=\mathbf{dom}\,f_1\cap\mathbf{dom}\,f_2$，且对 $x\in\mathbf{dom}\,f$ 有 $f(x)=f_1(x)+f_2(x)$。用扩值延拓可以直接说：对任意 $x$，$\tilde{f}(x)=\tilde{f}_1(x)+\tilde{f}_2(x)$——方程中 $f$ 的定义域被自动定义为 $\mathbf{dom}\,f_1\cap\mathbf{dom}\,f_2$，因为只要 $x\notin\mathbf{dom}\,f_1$ 或 $x\notin\mathbf{dom}\,f_2$ 就有 $\tilde{f}(x)=\infty$。这个例子依赖扩展算术来自动定义定义域。

本书中，在歧义无害时，我们用同一个符号表示凸函数及其延拓。这等价于假设所有凸函数都被隐式延拓，即在定义域外定义为 $\infty$。

<span style="color:#00838f;">**[例 3.1]** **凸集的示性函数（indicator function）。** 设 $C\subseteq\mathbb{R}^n$ 是凸集，考虑定义域为 $C$、对所有 $x\in C$ 取值 $I_C(x)=0$ 的（凸）函数。也就是说，该函数在集合 $C$ 上恒为零。其扩值延拓为

```math
\tilde{I}_C(x) = \begin{cases} 0 & x\in C\\ \infty & x\notin C. \end{cases}
```

凸函数 $\tilde{I}_C$ 称为集合 $C$ 的 indicator function（示性函数）。

<!-- p.83 -->

我们可以用示性函数 $\tilde{I}_C$ 玩好几种记号把戏。例如「在集合 $C$ 上最小化函数 $f$（设 $f$ 定义在整个 $\mathbb{R}^n$ 上）」与「在整个 $\mathbb{R}^n$ 上最小化函数 $f+\tilde{I}_C$」是同一回事：按我们的约定，$f+\tilde{I}_C$ 就是 $f$ 限制到集合 $C$。

类似地，可以把凹函数延拓为在定义域外取值 $-\infty$。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">3.1.3 First-order conditions（一阶条件）</span>

设 $f$ 是 differentiable（可微）的（即梯度 $\nabla f$ 在定义域每一点存在，且定义域是开的）。则 $f$ 凸当且仅当 $\mathbf{dom}\,f$ 凸，且

```math
f(y) \ge f(x)+\nabla f(x)^T(y-x) \tag{3.2}
```

对所有 $x,y\in\mathbf{dom}\,f$ 成立（见图 3.2）。

由 $y$ 表达的仿射函数 $f(x)+\nabla f(x)^T(y-x)$ 当然是 $f$ 在 $x$ 附近的 first-order Taylor approximation（一阶泰勒近似）。不等式 (3.2) 陈述：对凸函数，一阶泰勒近似其实是函数的 global underestimator（全局低估器）。反之，若一个函数的一阶泰勒近似总是它的全局低估器，则该函数是凸的。

不等式 (3.2) 表明：从凸函数的局部信息（它在一点的值与导数）可以导出全局信息（它的一个全局低估器）。这也许是凸函数最重要的性质，它解释了凸函数与凸优化问题的一些非凡性质。作为一个简单例子，(3.2) 表明：若 $\nabla f(x)=0$，则对所有 $y\in\mathbf{dom}\,f$ 有 $f(y)\ge f(x)$，即 $x$ 是函数 $f$ 的 global minimizer（全局最小化点）。

<!-- p.84 -->

严格凸也可以用一阶条件刻画：$f$ 严格凸当且仅当 $\mathbf{dom}\,f$ 凸，且对 $x,y\in\mathbf{dom}\,f$、$x\neq y$ 有

```math
f(y) > f(x)+\nabla f(x)^T(y-x). \tag{3.3}
```

对凹函数有相应的刻画：$f$ 凹当且仅当 $\mathbf{dom}\,f$ 凸且 $f(y)\le f(x)+\nabla f(x)^T(y-x)$ 对所有 $x,y\in\mathbf{dom}\,f$ 成立。

**一阶凸性条件的证明。** 先证 $n=1$ 的情形：可微函数 $f:\mathbb{R}\to\mathbb{R}$ 凸当且仅当

```math
f(y)\ge f(x)+f'(x)(y-x) \tag{3.4}
```

对 $\mathbf{dom}\,f$ 中所有 $x,y$ 成立。

先设 $f$ 凸且 $x,y\in\mathbf{dom}\,f$。由于 $\mathbf{dom}\,f$ 凸（即是一个 interval（区间）），对一切 $0\lt t\le1$ 有 $x+t(y-x)\in\mathbf{dom}\,f$，且由 $f$ 的凸性

```math
f(x+t(y-x))\le(1-t)f(x)+tf(y).
```

两边除以 $t$ 得

```math
f(y)\ge f(x)+\frac{f(x+t(y-x))-f(x)}{t},
```

令 $t\to0$ 取极限即得 (3.4)。

再证充分性：设函数对 $\mathbf{dom}\,f$（一个区间）中所有 $x,y$ 满足 (3.4)。任取 $x\neq y$ 与 $0\le\theta\le1$，令 $z=\theta x+(1-\theta)y$。应用 (3.4) 两次：

```math
f(x)\ge f(z)+f'(z)(x-z),\qquad f(y)\ge f(z)+f'(z)(y-z).
```

第一式乘 $\theta$、第二式乘 $1-\theta$ 后相加：

```math
\theta f(x)+(1-\theta)f(y)\ge f(z),
```

即证 $f$ 凸。

现在证一般情形 $f:\mathbb{R}^n\to\mathbb{R}$。设 $x,y\in\mathbb{R}^n$，考虑 $f$ 限制到穿过它们的直线上，即 $g(t)=f(ty+(1-t)x)$，则 $g'(t)=\nabla f(ty+(1-t)x)^T(y-x)$。

先设 $f$ 凸，这意味着 $g$ 凸，于是由上面的论证 $g(1)\ge g(0)+g'(0)$，即

```math
f(y)\ge f(x)+\nabla f(x)^T(y-x).
```

再设这个不等式对任意 $x,y$ 成立。若 $ty+(1-t)x\in\mathbf{dom}\,f$ 且 $\tilde{t}y+(1-\tilde{t})x\in\mathbf{dom}\,f$，则有

```math
f(ty+(1-t)x)\ge f(\tilde{t}y+(1-\tilde{t})x)+\nabla f(\tilde{t}y+(1-\tilde{t})x)^T(y-x)(t-\tilde{t}),
```

即 $g(t)\ge g(\tilde{t})+g'(\tilde{t})(t-\tilde{t})$。前面已见过这意味着 $g$ 凸。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">3.1.4 Second-order conditions（二阶条件）</span>

<!-- p.85 -->

现在设 $f$ 是 twice differentiable（二阶可微）的，即 Hessian（海森矩阵）或二阶导数 $\nabla^2f$ 在定义域每一点存在且定义域是开的。则 $f$ 凸当且仅当 $\mathbf{dom}\,f$ 凸且其 Hessian 半正定：

```math
\nabla^2f(x)\succeq0 \quad \text{对所有 } x\in\mathbf{dom}\,f.
```

对 $\mathbb{R}$ 上的函数，这退化为简单条件 $f''(x)\ge0$（且 $\mathbf{dom}\,f$ 凸，即一个区间），意味着导数 nondecreasing（非减）。条件 $\nabla^2f(x)\succeq0$ 在几何上可解释为函数图像在 $x$ 处具有正（向上）的 curvature（曲率）。我们把二阶条件的证明留作习题（习题 3.8）。

类似地，$f$ 凹当且仅当 $\mathbf{dom}\,f$ 凸且 $\nabla^2f(x)\preceq0$ 对所有 $x\in\mathbf{dom}\,f$。严格凸可以由二阶条件部分刻画：若 $\nabla^2f(x)\succ0$ 对所有 $x\in\mathbf{dom}\,f$，则 $f$ 严格凸。但逆命题不成立：例如 $f(x)=x^4$ 严格凸，但在 $x=0$ 处二阶导数为零。

<span style="color:#00838f;">**[例 3.2]** **二次函数。** 考虑二次函数 $f:\mathbb{R}^n\to\mathbb{R}$，$\mathbf{dom}\,f=\mathbb{R}^n$：

```math
f(x)=(1/2)x^TPx+q^Tx+r,
```

其中 $P\in\mathbf{S}^n$，$q\in\mathbb{R}^n$，$r\in\mathbb{R}$。由于 $\nabla^2f(x)=P$（对所有 $x$），$f$ 凸当且仅当 $P\succeq0$（凹当且仅当 $P\preceq0$）。对二次函数，严格凸容易刻画：$f$ 严格凸当且仅当 $P\succ0$（严格凹当且仅当 $P\prec0$）。

<span style="color:#00838f;">**[注 3.1]** 在凸性与凹性的一阶、二阶刻画中，「$\mathbf{dom}\,f$ 凸」这一单独要求不能去掉。例如函数 $f(x)=1/x^2$（$\mathbf{dom}\,f=\{x\in\mathbb{R} \mid x\neq0\}$）对所有 $x\in\mathbf{dom}\,f$ 满足 $f''(x)>0$，但它不是凸函数。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">3.1.5 Examples（例子）</span>

<!-- p.85/86 -->

我们已经提过：所有线性与仿射函数都凸（且凹），也描述过凸与凹的二次函数。本节再给一些凸函数与凹函数的例子。先看几个 $\mathbb{R}$ 上（变量为 $x$）的函数：

- Exponential（指数函数）：$e^{ax}$ 在 $\mathbb{R}$ 上凸，对任意 $a\in\mathbb{R}$。
- Powers（幂函数）：$x^a$ 在 $\mathbb{R}_{++}$ 上当 $a\ge1$ 或 $a\le0$ 时凸，当 $0\le a\le1$ 时凹。
- Powers of absolute value（绝对值的幂）：$|x|^p$（$p\ge1$）在 $\mathbb{R}$ 上凸。
- Logarithm（对数）：$\log x$ 在 $\mathbb{R}_{++}$ 上凹。
- Negative entropy（负熵）：$x\log x$（在 $\mathbb{R}_{++}$ 上，或在 $\mathbb{R}_+$ 上且定义 $x=0$ 时取 0）是凸的。

<!-- p.86/87 -->

这些例子的凸性或凹性可以通过验证基本不等式 (3.1) 来证明，或通过检查二阶导数非负/非正。例如 $f(x)=x\log x$：$f'(x)=\log x+1$，$f''(x)=1/x$，故 $x>0$ 时 $f''(x)>0$，即负熵函数（严格）凸。

现在给几个 $\mathbb{R}^n$ 上有趣的例子：

- Norms（范数）：$\mathbb{R}^n$ 上的每个范数都是凸的。
- Max function（最大值函数）：$f(x)=\max\{x_1,\dots,x_n\}$ 在 $\mathbb{R}^n$ 上凸。
- Quadratic-over-linear function（二次-线性分式函数）：$f(x,y)=x^2/y$，$\mathbf{dom}\,f=\mathbb{R}\times\mathbb{R}_{++}=\{(x,y)\in\mathbb{R}^2 \mid y>0\}$，是凸的（图 3.3）。
- Log-sum-exp（对数-和-指数）：$f(x)=\log(e^{x_1}+\cdots+e^{x_n})$ 在 $\mathbb{R}^n$ 上凸。这个函数可以解释为 max 函数的一个 differentiable（可微，其实是 analytic（解析））的近似，因为对所有 $x$ 有

```math
\max\{x_1,\dots,x_n\}\le f(x)\le\max\{x_1,\dots,x_n\}+\log n.
```

（当 $x$ 的分量全相等时第二个不等式取等。）图 3.4 展示了 $n=2$ 时的 $f$。

- Geometric mean（几何平均）：$f(x)=(\prod_{i=1}^n x_i)^{1/n}$ 在 $\mathbf{dom}\,f=\mathbb{R}^n_{++}$ 上凹。
- Log-determinant（对数行列式）：$f(X)=\log\det X$ 在 $\mathbf{dom}\,f=\mathbf{S}^n_{++}$ 上凹。

这些例子的凸性（或凹性）可以用几种方式验证：直接验证不等式 (3.1)、验证 Hessian 半正定、或把函数限制到任意直线上并验证所得一元函数的凸性。

**范数。** 若 $f:\mathbb{R}^n\to\mathbb{R}$ 是范数，$0\le\theta\le1$，则

```math
f(\theta x+(1-\theta)y)\le f(\theta x)+f((1-\theta)y)=\theta f(x)+(1-\theta)f(y).
```

不等号由 triangle inequality（三角不等式）得出，等号由范数的 homogeneity（齐次性）得出。

**最大值函数。** $f(x)=\max_i x_i$ 满足（$0\le\theta\le1$）：

```math
f(\theta x+(1-\theta)y)=\max_i(\theta x_i+(1-\theta)y_i)\le\theta\max_i x_i+(1-\theta)\max_i y_i=\theta f(x)+(1-\theta)f(y).
```

**二次-线性分式函数。** 为证 $f(x,y)=x^2/y$ 凸，注意（$y>0$ 时）：

```math
\nabla^2f(x,y)=\frac{2}{y^3}\begin{bmatrix}y^2&-xy\\-xy&x^2\end{bmatrix}=\frac{2}{y^3}\begin{bmatrix}y\\-x\end{bmatrix}\begin{bmatrix}y\\-x\end{bmatrix}^T\succeq0.
```

<!-- p.88 -->

**对数-和-指数。** log-sum-exp 函数的 Hessian 为

```math
\nabla^2f(x)=\frac{1}{(\mathbf{1}^Tz)^2}\bigl((\mathbf{1}^Tz)\,\mathbf{diag}(z)-zz^T\bigr),
```

其中 $z=(e^{x_1},\dots,e^{x_n})$。为验证 $\nabla^2f(x)\succeq0$，须证对所有 $v$，

```math
v^T\nabla^2f(x)v=\frac{1}{(\mathbf{1}^Tz)^2}\left[\Bigl(\sum_{i=1}^n z_i\Bigr)\Bigl(\sum_{i=1}^n v_i^2z_i\Bigr)-\Bigl(\sum_{i=1}^n v_iz_i\Bigr)^2\right]\ge0.
```

这正是 Cauchy-Schwarz inequality（柯西-施瓦茨不等式）$(a^Ta)(b^Tb)\ge(a^Tb)^2$ 应用于分量 $a_i=v_i\sqrt{z_i}$、$b_i=\sqrt{z_i}$ 的结果。

**几何平均。** 类似地可以证明几何平均 $f(x)=(\prod_{i=1}^nx_i)^{1/n}$ 在 $\mathbf{dom}\,f=\mathbb{R}^n_{++}$ 上凹。其 Hessian $\nabla^2f(x)$ 为

```math
\frac{\partial^2f}{\partial x_k^2}=-\frac{(n-1)(\prod_{i=1}^nx_i)^{1/n}}{n^2x_k^2},\qquad
\frac{\partial^2f}{\partial x_k\partial x_l}=\frac{(\prod_{i=1}^nx_i)^{1/n}}{n^2x_kx_l}\ (k\neq l),
```

可表达为

```math
\nabla^2f(x)=-\frac{\prod_{i=1}^nx_i^{1/n}}{n^2}\bigl(n\,\mathbf{diag}(1/x_1^2,\dots,1/x_n^2)-qq^T\bigr),
```

其中 $q_i=1/x_i$。须证 $\nabla^2f(x)\preceq0$，即对所有 $v$，

```math
v^T\nabla^2f(x)v=-\frac{\prod_{i=1}^nx_i^{1/n}}{n^2}\left[n\sum_{i=1}^n v_i^2/x_i^2-\Bigl(\sum_{i=1}^n v_i/x_i\Bigr)^2\right]\le0.
```

这同样由 Cauchy-Schwarz 不等式（取 $a=\mathbf{1}$、$b_i=v_i/x_i$）得出。

**对数行列式。** 对函数 $f(X)=\log\det X$，可以通过考虑任意直线 $X=Z+tV$（$Z,V\in\mathbf{S}^n$）来验证凹性。定义 $g(t)=f(Z+tV)$，把 $g$ 限制在使 $Z+tV\succ0$ 的 $t$ 区间上。不失一般性可设 $t=0$ 在此区间内，即 $Z\succ0$。我们有

```math
g(t)=\log\det(Z+tV)=\log\det\bigl(Z^{1/2}(I+tZ^{-1/2}VZ^{-1/2})Z^{1/2}\bigr)=\sum_{i=1}^n\log(1+t\lambda_i)+\log\det Z,
```

其中 $\lambda_1,\dots,\lambda_n$ 是 $Z^{-1/2}VZ^{-1/2}$ 的特征值。因此

```math
g'(t)=\sum_{i=1}^n\frac{\lambda_i}{1+t\lambda_i},\qquad g''(t)=-\sum_{i=1}^n\frac{\lambda_i^2}{(1+t\lambda_i)^2}.
```

由于 $g''(t)\le0$，得出 $f$ 凹。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">3.1.6 Sublevel sets（下水平集）</span>

<!-- p.89 -->

函数 $f:\mathbb{R}^n\to\mathbb{R}$ 的 α-sublevel set（α-下水平集）定义为

```math
C_\alpha=\{x\in\mathbf{dom}\,f \mid f(x)\le\alpha\}.
```

凸函数的下水平集对任何 $\alpha$ 值都是凸的。证明直接由凸性定义得出：若 $x,y\in C_\alpha$，则 $f(x)\le\alpha$、$f(y)\le\alpha$，于是对 $0\le\theta\le1$ 有 $f(\theta x+(1-\theta)y)\le\alpha$，故 $\theta x+(1-\theta)y\in C_\alpha$。

逆命题不成立：一个函数的所有下水平集都凸，但它本身可以不是凸函数。例如 $f(x)=-e^x$ 在 $\mathbb{R}$ 上不凸（其实它严格凹），但它的所有下水平集都凸。

若 $f$ 凹，则它的 α-superlevel set（α-上水平集）$\{x\in\mathbf{dom}\,f \mid f(x)\ge\alpha\}$ 是凸集。sublevel set property（下水平集性质）常常是确立集合凸性的好办法：把集合表示成某个凸函数的下水平集，或某个凹函数的上水平集。

<span style="color:#00838f;">**[例 3.3]** $x\in\mathbb{R}^n_+$ 的 geometric mean（几何平均）与 arithmetic mean（算术平均）分别为

```math
G(x)=\Bigl(\prod_{i=1}^nx_i\Bigr)^{1/n},\qquad A(x)=\frac{1}{n}\sum_{i=1}^nx_i
```

（在我们的 $G$ 的定义中取 $0^{1/n}=0$）。arithmetic-geometric mean inequality（算术-几何平均不等式）陈述 $G(x)\le A(x)$。设 $0\le\alpha\le1$，考虑集合

```math
\{x\in\mathbb{R}^n_+ \mid G(x)\ge\alpha A(x)\},
```

即「几何平均至少为算术平均的 $\alpha$ 倍」的向量集合。这个集合是凸的，因为它是函数 $G(x)-\alpha A(x)$（凹）的 0-上水平集。事实上该集合还是 positively homogeneous（正齐次）的，所以它是一个凸锥。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">3.1.7 Epigraph（上图）</span>

<!-- p.89/90 -->

函数 $f:\mathbb{R}^n\to\mathbb{R}$ 的 graph（图像）定义为

```math
\{(x,f(x)) \mid x\in\mathbf{dom}\,f\},
```

它是 $\mathbb{R}^{n+1}$ 的子集。函数 $f$ 的 epigraph（上图）定义为

```math
\mathbf{epi}\,f=\{(x,t) \mid x\in\mathbf{dom}\,f,\; f(x)\le t\},
```

它也是 $\mathbb{R}^{n+1}$ 的子集。（「Epi」意为「在……之上」，epigraph 即「图像之上」。）定义见图 3.5。

凸集与凸函数之间的联系通过上图建立：**函数凸当且仅当它的上图是凸集**。函数凹当且仅当它的 hypograph（下图）

```math
\mathbf{hypo}\,f=\{(x,t) \mid t\le f(x)\}
```

是凸集。

<span style="color:#00838f;">**[例 3.4]** **矩阵分式函数（matrix fractional function）。** 函数 $f:\mathbb{R}^n\times\mathbf{S}^n\to\mathbb{R}$ 定义为

```math
f(x,Y)=x^TY^{-1}x
```

在 $\mathbf{dom}\,f=\mathbb{R}^n\times\mathbf{S}^n_{++}$ 上凸。（它推广了二次-线性分式函数 $f(x,y)=x^2/y$，$\mathbf{dom}\,f=\mathbb{R}\times\mathbb{R}_{++}$。）

<!-- p.90 -->

确立 $f$ 凸性的一个简单办法是通过它的上图：

```math
\begin{aligned}
\mathbf{epi}\,f &= \{(x,Y,t) \mid Y\succ0,\; x^TY^{-1}x\le t\}\\
&= \left\{(x,Y,t)\;\middle|\;\begin{bmatrix}Y&x\\x^T&t\end{bmatrix}\succeq0,\; Y\succ0\right\},
\end{aligned}
```

这里用了分块矩阵半正定性的 Schur complement condition（舒尔补条件）（见 §A.5.5）。最后一个条件是关于 $(x,Y,t)$ 的 linear matrix inequality（线性矩阵不等式），因此 $\mathbf{epi}\,f$ 凸。

当 $n=1$ 时，矩阵分式函数退化为二次-线性分式函数 $x^2/y$，相应的 LMI 表示为

```math
\begin{bmatrix}y&x\\x&t\end{bmatrix}\succeq0,\qquad y>0
```

（其图像见图 3.3）。

许多凸函数的结果可以借助上图用几何方式证明（或解释），再应用凸集的结果。例如考虑凸性的一阶条件

```math
f(y)\ge f(x)+\nabla f(x)^T(y-x) \qquad (f \text{ 凸}，\ x,y\in\mathbf{dom}\,f).
```

<!-- p.91 -->

我们可以在 $\mathbf{epi}\,f$ 的语境下几何化地解释这个基本不等式。若 $(y,t)\in\mathbf{epi}\,f$，则

```math
t\ge f(y)\ge f(x)+\nabla f(x)^T(y-x).
```

这可以写成：

```math
(y,t)\in\mathbf{epi}\,f \implies \begin{bmatrix}\nabla f(x)\\-1\end{bmatrix}^T\left(\begin{bmatrix}y\\t\end{bmatrix}-\begin{bmatrix}x\\f(x)\end{bmatrix}\right)\le0.
```

这意味着由 $(\nabla f(x),-1)$ 定义的超平面在边界点 $(x,f(x))$ 处支撑 $\mathbf{epi}\,f$；见图 3.6。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">3.1.8 Jensen's inequality and extensions（Jensen 不等式及其推广）</span>

<!-- p.91/92 -->

基本不等式 (3.1)，即

```math
f(\theta x+(1-\theta)y)\le\theta f(x)+(1-\theta)f(y),
```

有时称为 Jensen's inequality（Jensen 不等式）。它很容易推广到多于两点的凸组合：若 $f$ 凸，$x_1,\dots,x_k\in\mathbf{dom}\,f$，$\theta_1,\dots,\theta_k\ge0$ 且 $\theta_1+\cdots+\theta_k=1$，则

```math
f(\theta_1x_1+\cdots+\theta_kx_k)\le\theta_1f(x_1)+\cdots+\theta_kf(x_k).
```

与凸集情形一样，该不等式可推广到无穷和、积分与期望。例如，若 $p(x)\ge0$（$S\subseteq\mathbf{dom}\,f$），$\int_Sp(x)\,dx=1$，则（积分存在时）

```math
f\Bigl(\int_Sp(x)x\,dx\Bigr)\le\int_Sf(x)p(x)\,dx.
```

最一般的情形可取任何 support（支撑）在 $\mathbf{dom}\,f$ 中的 probability measure（概率测度）。若 $x$ 是随机变量、$x\in\mathbf{dom}\,f$ 以概率 1 成立、$f$ 凸，则（期望存在时）

```math
f(\mathbb{E}\,x)\le\mathbb{E}\,f(x). \tag{3.5}
```

从这一般形式可以恢复基本不等式 (3.1)：取随机变量 $x$ 的支撑为 $\{x_1,x_2\}$，$\mathrm{prob}(x=x_1)=\theta$、$\mathrm{prob}(x=x_2)=1-\theta$ 即可。因此不等式 (3.5) 刻画了凸性：若 $f$ 不凸，则存在随机变量 $x$（$x\in\mathbf{dom}\,f$ 以概率 1 成立）使 $f(\mathbb{E}\,x)>\mathbb{E}\,f(x)$。

<!-- p.92/93 -->

这些不等式现在都叫 Jensen 不等式，尽管 Jensen 当年研究的是非常简单的那一个：

```math
f\Bigl(\frac{x+y}{2}\Bigr)\le\frac{f(x)+f(y)}{2}.
```

<span style="color:#00838f;">**[注 3.2]** 可以这样解释 (3.5)：设 $x\in\mathbf{dom}\,f\subseteq\mathbb{R}^n$，$z$ 是 $\mathbb{R}^n$ 中任意 zero mean random vector（零均值随机向量），则

```math
\mathbb{E}\,f(x+z)\ge f(x).
```

也就是说，randomization 或 dithering（随机化/抖动，即给自变量加一个零均值随机向量）不可能平均地降低凸函数的值。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">3.1.9 Inequalities（不等式）</span>

许多著名的不等式都可以通过把 Jensen 不等式应用于某个适当的凸函数来导出。（事实上，可以把凸性与 Jensen 不等式做成一套不等式理论的基础。）举一个简单例子，考虑 arithmetic-geometric mean inequality（算术-几何平均不等式）：

```math
\sqrt{ab}\le(a+b)/2 \tag{3.6}
```

（$a,b\ge0$）。函数 $-\log x$ 是凸的；取 $\theta=1/2$ 的 Jensen 不等式给出

```math
-\log\Bigl(\frac{a+b}{2}\Bigr)\le\frac{-\log a-\log b}{2}.
```

两边取指数即得 (3.6)。

<!-- p.93 -->

一个不那么平凡的例子是 Hölder's inequality（赫尔德不等式）的证明：对 $p>1$、$1/p+1/q=1$、$x,y\in\mathbb{R}^n$，

```math
\sum_{i=1}^nx_iy_i\le\Bigl(\sum_{i=1}^n|x_i|^p\Bigr)^{1/p}\Bigl(\sum_{i=1}^n|y_i|^q\Bigr)^{1/q}.
```

由 $-\log x$ 的凸性与一般 $\theta$ 的 Jensen 不等式，得到更一般的算术-几何平均不等式

```math
a^\theta b^{1-\theta}\le\theta a+(1-\theta)b,
```

对 $a,b\ge0$、$0\le\theta\le1$ 成立。代入

```math
a=\frac{|x_i|^p}{\sum_{j=1}^n|x_j|^p},\qquad b=\frac{|y_i|^q}{\sum_{j=1}^n|y_j|^q},\qquad \theta=1/p,
```

得

```math
\Bigl(\frac{|x_i|^p}{\sum_{j=1}^n|x_j|^p}\Bigr)^{1/p}\Bigl(\frac{|y_i|^q}{\sum_{j=1}^n|y_j|^q}\Bigr)^{1/q}\le\frac{|x_i|^p}{p\sum_{j=1}^n|x_j|^p}+\frac{|y_i|^q}{q\sum_{j=1}^n|y_j|^q}.
```

对 $i$ 求和即得 Hölder 不等式。

> <span style="color:#2471a3;">**[译者注｜跨课程连接]** §3.1 是全书第一次正面回答「为什么凸这么好」：一阶条件 (3.2) 说「局部信息给出全局低估」——这正是 6.006/CLRS 里贪心算法正确性证明的本质（局部最优选择导致全局最优解，只在凸结构下才有保证）。而 Jensen 不等式 $f(\mathbb{E}x)\le\mathbb{E}f(x)$ 将是你概率论（6.431）里期望、方差、熵不等式的万能工具。</span>

## <span style="color:#c0392b;">3.2 Operations that preserve convexity（保持凸性的运算）</span>

<!-- p.93 -->

本节描述一些保持函数凸性或凹性的运算，或能构造新凸函数与凹函数的运算。先讲简单运算（加法、缩放、逐点上确界），再讲一些更精细的运算（其中有些把简单运算作为特例包含进来）。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">3.2.1 Nonnegative weighted sums（非负加权和）</span>

显然，若 $f$ 凸且 $\alpha\ge0$，则函数 $\alpha f$ 凸。若 $f_1$ 与 $f_2$ 都是凸函数，则它们的和 $f_1+f_2$ 也凸。把非负缩放与加法结合起来：凸函数的集合本身就是一个 convex cone（凸锥）——凸函数的非负加权和

```math
f = w_1f_1+\cdots+w_mf_m
```

是凸的。类似地，凹函数的非负加权和是凹的。严格凸（凹）函数的非负且不全为零的加权和是严格凸（凹）的。

这些性质可推广到无穷和与积分。例如，若 $f(x,y)$ 对每个 $y\in\mathcal{A}$ 关于 $x$ 凸，且 $w(y)\ge0$（对每个 $y\in\mathcal{A}$），则函数

```math
g(x)=\int_{\mathcal{A}}w(y)f(x,y)\,dy
```

关于 $x$ 凸（积分存在时）。

凸性在非负缩放与加法下保持这件事可以直接验证，也可以借助相应的上图来看。例如若 $w\ge0$ 且 $f$ 凸，则

```math
\mathbf{epi}(wf)=\begin{bmatrix}I&0\\0&w\end{bmatrix}\mathbf{epi}\,f,
```

它凸，因为凸集在线性映射下的像是凸的。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">3.2.2 Composition with an affine mapping（与仿射映射复合）</span>

<!-- p.93/94 -->

设 $f:\mathbb{R}^n\to\mathbb{R}$，$A\in\mathbb{R}^{n\times m}$，$b\in\mathbb{R}^n$。定义 $g:\mathbb{R}^m\to\mathbb{R}$：

```math
g(x)=f(Ax+b),\qquad \mathbf{dom}\,g=\{x \mid Ax+b\in\mathbf{dom}\,f\}.
```

则若 $f$ 凸，$g$ 也凸；若 $f$ 凹，$g$ 也凹。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">3.2.3 Pointwise maximum and supremum（逐点最大值与上确界）</span>

<!-- p.94 -->

若 $f_1$ 与 $f_2$ 是凸函数，则它们的 pointwise maximum（逐点最大值）$f$，定义为

```math
f(x)=\max\{f_1(x),f_2(x)\},\qquad \mathbf{dom}\,f=\mathbf{dom}\,f_1\cap\mathbf{dom}\,f_2,
```

也是凸的。这个性质容易验证：若 $0\le\theta\le1$，$x,y\in\mathbf{dom}\,f$，则

```math
\begin{aligned}
f(\theta x+(1-\theta)y)
&= \max\{f_1(\theta x+(1-\theta)y),\; f_2(\theta x+(1-\theta)y)\}\\
&\le \max\{\theta f_1(x)+(1-\theta)f_1(y),\; \theta f_2(x)+(1-\theta)f_2(y)\}\\
&\le \theta\max\{f_1(x),f_2(x)\}+(1-\theta)\max\{f_1(y),f_2(y)\}\\
&= \theta f(x)+(1-\theta)f(y),
\end{aligned}
```

即确立 $f$ 的凸性。容易证明：若 $f_1,\dots,f_m$ 凸，则它们的逐点最大值

```math
f(x)=\max\{f_1(x),\dots,f_m(x)\}
```

也凸。

<span style="color:#00838f;">**[例 3.5]** **分段线性函数。** 函数

```math
f(x)=\max\{a_1^Tx+b_1,\dots,a_L^Tx+b_L\}
```

定义一个 piecewise-linear（分段线性，实际是分段仿射）函数（有 $L$ 个或更少的区域）。它凸，因为它是仿射函数的逐点最大值。逆命题也成立：任何有 $L$ 个或更少区域的分段线性凸函数都可以表示成这种形式。（见习题 3.29。）

<span style="color:#00838f;">**[例 3.6]** **最大 $r$ 个分量之和。** 对 $x\in\mathbb{R}^n$，用 $x_{[i]}$ 表示 $x$ 的第 $i$ 大的分量，即

```math
x_{[1]}\ge x_{[2]}\ge\cdots\ge x_{[n]}
```

是按非增序排列的分量。则函数

```math
f(x)=\sum_{i=1}^rx_{[i]},
```

即 $x$ 的最大 $r$ 个元素之和，是凸函数。可以这样看：

```math
f(x)=\sum_{i=1}^rx_{[i]}=\max\{x_{i_1}+\cdots+x_{i_r} \mid 1\le i_1<i_2<\cdots<i_r\le n\},
```

即 $x$ 的 $r$ 个不同分量的所有可能和的最大值。由于它是 $n!/(r!(n-r)!)$ 个线性函数的逐点最大值，它是凸的。作为推广可以证明：只要 $w_1\ge w_2\ge\cdots\ge w_r\ge0$，函数 $\sum_{i=1}^rw_ix_{[i]}$ 也是凸的。（见习题 3.19。）

<!-- p.95 -->

逐点最大值性质可推广到无穷多个凸函数的 pointwise supremum（逐点上确界）。若对每个 $y\in\mathcal{A}$，$f(x,y)$ 关于 $x$ 凸，则函数

```math
g(x)=\sup_{y\in\mathcal{A}}f(x,y) \tag{3.7}
```

关于 $x$ 凸。$g$ 的定义域为

```math
\mathbf{dom}\,g=\{x \mid (x,y)\in\mathbf{dom}\,f \text{ 对所有 } y\in\mathcal{A},\; \sup_{y\in\mathcal{A}}f(x,y)<\infty\}.
```

类似地，一族凹函数的 pointwise infimum（逐点下确界）是凹函数。

用上图的语言：函数的逐点上确界对应上图之交。按 (3.7) 定义的 $f,g$ 与 $\mathcal{A}$，有

```math
\mathbf{epi}\,g=\bigcap_{y\in\mathcal{A}}\mathbf{epi}\,f(\cdot,y).
```

于是结果由「一族凸集之交是凸的」得出。

<span style="color:#00838f;">**[例 3.7]** **集合的支撑函数（support function）。** 设 $C\subseteq\mathbb{R}^n$，$C\neq\varnothing$。与集合 $C$ 相关联的 support function（支撑函数）$S_C$ 定义为

```math
S_C(x)=\sup\{x^Ty \mid y\in C\}
```

（自然地，$\mathbf{dom}\,S_C=\{x \mid \sup_{y\in C}x^Ty<\infty\}$）。对每个 $y\in C$，$x^Ty$ 是 $x$ 的线性函数，所以 $S_C$ 是一族线性函数的逐点上确界，因而凸。

<span style="color:#00838f;">**[例 3.8]** **到集合最远点的距离。** 设 $C\subseteq\mathbb{R}^n$。到 $C$ 最远点的距离（按任何范数）

```math
f(x)=\sup_{y\in C}\|x-y\|
```

是凸的。为看出这一点，注意对任意 $y$，函数 $\|x-y\|$ 关于 $x$ 凸。由于 $f$ 是一族凸函数（由 $y\in C$ 索引）的逐点上确界，它是 $x$ 的凸函数。

<span style="color:#00838f;">**[例 3.9]** **作为权重函数的最小二乘代价。** 设 $a_1,\dots,a_n\in\mathbb{R}^m$。在加权最小二乘问题中，我们在 $x\in\mathbb{R}^m$ 上最小化目标函数 $\sum_{i=1}^nw_i(a_i^Tx-b_i)^2$。我们把 $w_i$ 称为 weights（权重），并允许 $w_i$ 为负（这使目标函数可能无下界）。我们定义（最优）加权最小二乘代价为

```math
g(w)=\inf_x\sum_{i=1}^nw_i(a_i^Tx-b_i)^2,
```

定义域为

```math
\mathbf{dom}\,g=\left\{w\;\middle|\;\inf_x\sum_{i=1}^nw_i(a_i^Tx-b_i)^2>-\infty\right\}.
```

<!-- p.96 -->

由于 $g$ 是一族（以 $x\in\mathbb{R}^m$ 索引的）$w$ 的线性函数的下确界，它是 $w$ 的凹函数。

我们至少可以在 $g$ 定义域的一部分上导出它的显式表达式。设 $W=\mathbf{diag}(w)$（以 $w_1,\dots,w_n$ 为对角元的对角矩阵），$A\in\mathbb{R}^{n\times m}$ 以 $a_i^T$ 为行，则

```math
g(w)=\inf_x(Ax-b)^TW(Ax-b)=\inf_x(x^TA^TWAx-2b^TWAx+b^TWb).
```

由此看出：若 $A^TWA\not\succeq0$，该二次函数关于 $x$ 无下界，故 $g(w)=-\infty$，即 $w\notin\mathbf{dom}\,g$。当 $A^TWA\succ0$（一个严格线性矩阵不等式）时，可以通过解析地最小化二次函数给出 $g$ 的简单表达式：

```math
\begin{aligned}
g(w) &= b^TWb-b^TWA(A^TWA)^{-1}A^TWb\\
&= \sum_{i=1}^nw_ib_i^2-\sum_{i=1}^nw_i^2b_i^2a_i^T\Bigl(\sum_{j=1}^nw_ja_ja_j^T\Bigr)^{-1}a_i.
\end{aligned}
```

从这个表达式看 $g$ 的凹性并不直观（但它确实成立，例如可由矩阵分式函数的凸性得出；见例 3.4）。

<span style="color:#00838f;">**[例 3.10]** **对称矩阵的最大特征值。** 函数 $f(X)=\lambda_{\max}(X)$（$\mathbf{dom}\,f=\mathbf{S}^m$）是凸的。为看出这一点，把 $f$ 表为

```math
f(X)=\sup\{y^TXy \mid \|y\|_2=1\},
```

即一族（由 $y\in\mathbb{R}^m$ 索引的）$X$ 的线性函数 $y^TXy$ 的逐点上确界。

<span style="color:#00838f;">**[例 3.11]** **矩阵的范数。** 考虑 $f(X)=\|X\|_2$，$\mathbf{dom}\,f=\mathbb{R}^{p\times q}$，其中 $\|\cdot\|_2$ 表示 spectral norm（谱范数）或 maximum singular value（最大奇异值）。$f$ 的凸性由

```math
f(X)=\sup\{u^TXv \mid \|u\|_2=1,\; \|v\|_2=1\}
```

得出：它是 $X$ 的线性函数族的逐点上确界。

作为推广，设 $\|\cdot\|_a$ 与 $\|\cdot\|_b$ 分别是 $\mathbb{R}^p$ 与 $\mathbb{R}^q$ 上的范数。矩阵 $X\in\mathbb{R}^{p\times q}$ 的 induced norm（诱导范数）定义为

```math
\|X\|_{a,b}=\sup_{v\neq0}\frac{\|Xv\|_a}{\|v\|_b}.
```

（当两个范数都是欧氏范数时，它退化为谱范数。）诱导范数可表达为

```math
\|X\|_{a,b}=\sup\{\|Xv\|_a \mid \|v\|_b=1\}=\sup\{u^TXv \mid \|u\|_{a*}=1,\; \|v\|_b=1\},
```

<!-- p.96/97 -->

其中 $\|\cdot\|_{a*}$ 是 $\|\cdot\|_a$ 的对偶范数，且用了事实 $\|z\|_a=\sup\{u^Tz \mid \|u\|_{a*}=1\}$。既然把 $\|X\|_{a,b}$ 表成了 $X$ 的线性函数的上确界，它就是凸函数。

**表示为仿射函数族的逐点上确界（Representation as pointwise supremum of affine functions）。** 上面的例子展示了一个确立函数凸性的好方法：把它表示成一族仿射函数的逐点上确界。除一个技术性条件外，逆命题也成立：几乎每个凸函数都可以表示成一族仿射函数的逐点上确界。例如，若 $f:\mathbb{R}^n\to\mathbb{R}$ 凸且 $\mathbf{dom}\,f=\mathbb{R}^n$，则

```math
f(x)=\sup\{g(x) \mid g \text{ 仿射}，\; g(z)\le f(z) \text{ 对所有 } z\}.
```

换言之，$f$ 是它的所有 affine global underestimators（仿射全局低估器）的逐点上确界。下面给出证明；$\mathbf{dom}\,f\neq\mathbb{R}^n$ 的情形留作习题（习题 3.28）。

<!-- p.97 -->

设 $f$ 凸且 $\mathbf{dom}\,f=\mathbb{R}^n$。不等式

```math
f(x)\ge\sup\{g(x) \mid g \text{ 仿射}，\; g(z)\le f(z) \text{ 对所有 } z\}
```

是清楚的：若 $g$ 是 $f$ 的任何仿射低估器，则 $g(x)\le f(x)$。为确立等号，我们将证明：对每个 $x\in\mathbb{R}^n$，存在一个仿射函数 $g$，它是 $f$ 的全局低估器，且满足 $g(x)=f(x)$。

$f$ 的上图当然是凸集。因此可以在点 $(x,f(x))$ 处找到它的一个支撑超平面：即 $a\in\mathbb{R}^n$、$b\in\mathbb{R}$，$(a,b)\neq0$，且

```math
\begin{bmatrix}a\\b\end{bmatrix}^T\begin{bmatrix}x-z\\f(x)-t\end{bmatrix}\le0
```

对所有 $(z,t)\in\mathbf{epi}\,f$ 成立。这意味着

```math
a^T(x-z)+b(f(x)-f(z)-s)\le0 \tag{3.8}
```

对所有 $z\in\mathbf{dom}\,f=\mathbb{R}^n$ 与所有 $s\ge0$ 成立（因为 $(z,t)\in\mathbf{epi}\,f$ 意味着 $t=f(z)+s$，某个 $s\ge0$）。要 (3.8) 对所有 $s\ge0$ 成立，必须有 $b\ge0$。若 $b=0$，则 (3.8) 退化为 $a^T(x-z)\le0$（对所有 $z\in\mathbb{R}^n$），从而 $a=0$，与 $(a,b)\neq0$ 矛盾。于是 $b>0$，即支撑超平面不是竖直的。

利用 $b>0$，把 (3.8) 在 $s=0$ 时改写为

```math
g(z)=f(x)+(a/b)^T(x-z)\le f(z)
```

对所有 $z$。函数 $g$ 是 $f$ 的仿射低估器，且满足 $g(x)=f(x)$。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">3.2.4 Composition（复合）</span>

<!-- p.97/98 -->

本节考察 $h:\mathbb{R}^k\to\mathbb{R}$ 与 $g:\mathbb{R}^n\to\mathbb{R}^k$ 的条件，保证它们的复合 $f=h\circ g:\mathbb{R}^n\to\mathbb{R}$ 凸或凹，其中

```math
f(x)=h(g(x)),\qquad \mathbf{dom}\,f=\{x\in\mathbf{dom}\,g \mid g(x)\in\mathbf{dom}\,h\}.
```

**标量复合（Scalar composition）。** 先考虑 $k=1$ 的情形：$h:\mathbb{R}\to\mathbb{R}$，$g:\mathbb{R}^n\to\mathbb{R}$。可以只考虑 $n=1$（因为凸性由函数在与其定义域相交的任意直线上的行为决定）。

为发现复合规则，先假设 $h$ 与 $g$ 二阶可微，$\mathbf{dom}\,g=\mathbf{dom}\,h=\mathbb{R}$。此时 $f$ 的凸性退化为 $f''\ge0$（即 $f''(x)\ge0$ 对所有 $x\in\mathbb{R}$）。复合函数 $f=h\circ g$ 的二阶导数为

```math
f''(x)=h''(g(x))g'(x)^2+h'(g(x))g''(x). \tag{3.9}
```

<!-- p.98 -->

例如设 $g$ 凸（$g''\ge0$），$h$ 凸且 nondecreasing（非减）（$h''\ge0$，$h'\ge0$），则由 (3.9) 得 $f''\ge0$，即 $f$ 凸。类似地，(3.9) 给出结果：

```math
\begin{aligned}
& f \text{ 凸}， && \text{若 } h \text{ 凸且非减，} g \text{ 凸}；\\
& f \text{ 凸}， && \text{若 } h \text{ 凸且非增，} g \text{ 凹}；\\
& f \text{ 凹}， && \text{若 } h \text{ 凹且非减，} g \text{ 凹}；\\
& f \text{ 凹}， && \text{若 } h \text{ 凹且非增，} g \text{ 凸}.
\end{aligned} \tag{3.10}
```

这些陈述在 $g,h$ 二阶可微且定义域为全 $\mathbb{R}$ 时有效。事实上，非常相似的复合规则在一般情形 $n>1$ 下也成立，不需要假设 $h,g$ 可微或 $\mathbf{dom}\,g=\mathbb{R}^n$、$\mathbf{dom}\,h=\mathbb{R}$：

```math
\begin{aligned}
& f \text{ 凸}， && \text{若 } h \text{ 凸}，\ \tilde{h} \text{ 非减}，\ g \text{ 凸}；\\
& f \text{ 凸}， && \text{若 } h \text{ 凸}，\ \tilde{h} \text{ 非增}，\ g \text{ 凹}；\\
& f \text{ 凹}， && \text{若 } h \text{ 凹}，\ \tilde{h} \text{ 非减}，\ g \text{ 凹}；\\
& f \text{ 凹}， && \text{若 } h \text{ 凹}，\ \tilde{h} \text{ 非增}，\ g \text{ 凸}.
\end{aligned} \tag{3.11}
```

这里 $\tilde{h}$ 表示函数 $h$ 的扩值延拓（对凸的 $h$，把 $\mathbf{dom}\,h$ 之外的点赋值 $\infty$；对凹的 $h$ 赋值 $-\infty$）。这些结果与 (3.10) 的唯一差别是：要求扩值延拓函数 $\tilde{h}$ 在整个 $\mathbb{R}$ 上非增或非减。

<!-- p.98/99 -->

为理解这意味着什么：设 $h$ 凸，所以 $\tilde{h}$ 在 $\mathbf{dom}\,h$ 外取值 $\infty$。说 $\tilde{h}$ 非减，意味着对任意 $x,y\in\mathbb{R}$（$x\lt y$）有 $\tilde{h}(x)\le\tilde{h}(y)$。特别地，若 $y\in\mathbf{dom}\,h$ 则 $x\in\mathbf{dom}\,h$。换言之，$h$ 的定义域向负方向无限延伸：它是 $\mathbb{R}$，或形如 $(-\infty,a)$ 或 $(-\infty,a]$ 的区间。类似地，「$h$ 凸且 $\tilde{h}$ 非增」意味着 $h$ 非增且 $\mathbf{dom}\,h$ 向正方向无限延伸。见图 3.7。

<span style="color:#00838f;">**[例 3.12]** 一些简单例子可说明复合定理中对 $h$ 的条件：

- 函数 $h(x)=\log x$（$\mathbf{dom}\,h=\mathbb{R}_{++}$）凹，且 $\tilde{h}$ 非减。
- 函数 $h(x)=x^{1/2}$（$\mathbf{dom}\,h=\mathbb{R}_+$）凹，且满足 $\tilde{h}$ 非减。
- 函数 $h(x)=x^{3/2}$（$\mathbf{dom}\,h=\mathbb{R}_+$）凸，但不满足 $\tilde{h}$ 非减：例如 $\tilde{h}(-1)=\infty$ 但 $\tilde{h}(1)=1$。
- 函数 $h(x)=x^{3/2}$（$x\ge0$）、$h(x)=0$（$x<0$），$\mathbf{dom}\,h=\mathbb{R}$，凸且满足 $\tilde{h}$ 非减。

<!-- p.99 -->

复合结果 (3.11) 可以直接证明（不假设可微、不用公式 (3.9)）。作为例子，我们证明如下复合定理：若 $g$ 凸，$h$ 凸，$\tilde{h}$ 非减，则 $f=h\circ g$ 凸。设 $x,y\in\mathbf{dom}\,f$，$0\le\theta\le1$。由于 $x,y\in\mathbf{dom}\,f$，有 $x,y\in\mathbf{dom}\,g$ 且 $g(x),g(y)\in\mathbf{dom}\,h$。由 $\mathbf{dom}\,g$ 凸得 $\theta x+(1-\theta)y\in\mathbf{dom}\,g$，又由 $g$ 的凸性

```math
g(\theta x+(1-\theta)y)\le\theta g(x)+(1-\theta)g(y). \tag{3.12}
```

由 $g(x),g(y)\in\mathbf{dom}\,h$ 得 $\theta g(x)+(1-\theta)g(y)\in\mathbf{dom}\,h$，即 (3.12) 的右边属于 $\mathbf{dom}\,h$。现在用 $\tilde{h}$ 非减的假设（其定义域向负方向无限延伸）：由于 (3.12) 的右边属于 $\mathbf{dom}\,h$，可推出左边 $g(\theta x+(1-\theta)y)\in\mathbf{dom}\,h$。这意味着 $\theta x+(1-\theta)y\in\mathbf{dom}\,f$。至此我们已证明 $\mathbf{dom}\,f$ 凸。

再用 $\tilde{h}$ 非减与不等式 (3.12)：

```math
h(g(\theta x+(1-\theta)y))\le h(\theta g(x)+(1-\theta)g(y)). \tag{3.13}
```

<!-- p.100 -->

由 $h$ 的凸性：

```math
h(\theta g(x)+(1-\theta)g(y))\le\theta h(g(x))+(1-\theta)h(g(y)). \tag{3.14}
```

把 (3.13) 与 (3.14) 合起来：

```math
h(g(\theta x+(1-\theta)y))\le\theta h(g(x))+(1-\theta)h(g(y)),
```

复合定理得证。

<span style="color:#00838f;">**[例 3.13]** **简单复合结果。**

- 若 $g$ 凸，则 $\exp g(x)$ 凸。
- 若 $g$ 凹且为正，则 $\log g(x)$ 凹。
- 若 $g$ 凹且为正，则 $1/g(x)$ 凸。
- 若 $g$ 凸且非负，$p\ge1$，则 $g(x)^p$ 凸。
- 若 $g$ 凸，则 $-\log(-g(x))$ 在 $\{x \mid g(x)<0\}$ 上凸。

<span style="color:#00838f;">**[注 3.3]** 「单调性须对扩值延拓 $\tilde{h}$ 成立而不只是对 $h$」这一要求不能去掉。例如取 $g(x)=x^2$（$\mathbf{dom}\,g=\mathbb{R}$），$h(x)=0$（$\mathbf{dom}\,h=[1,2]$）。$g$ 凸，$h$ 凸且非减。但复合函数 $f=h\circ g$：

```math
f(x)=0,\qquad \mathbf{dom}\,f=[-\sqrt{2},-1]\cup[1,\sqrt{2}]
```

不凸，因为其定义域不凸。这里当然 $\tilde{h}$ 不是非减的。

**向量复合（Vector composition）。** 现在看更复杂的 $k\ge1$ 的情形。设

```math
f(x)=h(g(x))=h(g_1(x),\dots,g_k(x)),
```

其中 $h:\mathbb{R}^k\to\mathbb{R}$，$g_i:\mathbb{R}^n\to\mathbb{R}$。同样不失一般性可设 $n=1$。与 $k=1$ 的情形一样，先假设函数二阶可微、$\mathbf{dom}\,g=\mathbb{R}$、$\mathbf{dom}\,h=\mathbb{R}^k$，以发现复合规则。我们有

```math
f''(x)=g'(x)^T\nabla^2h(g(x))g'(x)+\nabla h(g(x))^Tg''(x), \tag{3.15}
```

它是 (3.9) 的向量类比。问题仍是确定使 $f''(x)\ge0$ 对所有 $x$ 成立的条件（凹性则是 $\le0$）。由 (3.15) 可导出许多规则，例如：

```math
\begin{aligned}
& f \text{ 凸}， && \text{若 } h \text{ 凸}，\ h \text{ 对每个自变量非减}，\ g_i \text{ 凸}；\\
& f \text{ 凸}， && \text{若 } h \text{ 凸}，\ h \text{ 对每个自变量非增}，\ g_i \text{ 凹}；\\
& f \text{ 凹}， && \text{若 } h \text{ 凹}，\ h \text{ 对每个自变量非减}，\ g_i \text{ 凹}.
\end{aligned}
```

<!-- p.101 -->

与标量情形一样，类似的一般复合结果在 $n>1$、不假设 $h,g$ 可微、一般定义域下也成立；一般结果中 $h$ 的单调性条件必须对扩值延拓 $\tilde{h}$ 成立。

为理解「扩值延拓 $\tilde{h}$ 单调」条件的含义，考虑 $h:\mathbb{R}^k\to\mathbb{R}$ 凸且 $\tilde{h}$ 非减的情形：即只要 $u\preceq v$ 就有 $\tilde{h}(u)\le\tilde{h}(v)$。这意味着若 $v\in\mathbf{dom}\,h$ 则 $u$ 也是：$h$ 的定义域必须沿 $-\mathbb{R}^k_+$ 方向无限延伸。可以紧凑地表为 $\mathbf{dom}\,h-\mathbb{R}^k_+=\mathbf{dom}\,h$。

<span style="color:#00838f;">**[例 3.14]** **向量复合的例子。**

- 令 $h(z)=z_{[1]}+\cdots+z_{[r]}$（$z\in\mathbb{R}^k$ 的最大 $r$ 个分量之和）。则 $h$ 凸且对每个自变量非减。设 $g_1,\dots,g_k$ 是 $\mathbb{R}^n$ 上的凸函数，则复合函数 $f=h\circ g$（即 $g_i$ 中最大 $r$ 个的逐点和）凸。
- 函数 $h(z)=\log(\sum_{i=1}^ke^{z_i})$ 凸且对每个自变量非减，所以只要 $g_i$ 凸，$\log(\sum_{i=1}^ke^{g_i})$ 就凸。
- 对 $0\lt p\le1$，函数 $h(z)=(\sum_{i=1}^kz_i^p)^{1/p}$ 在 $\mathbb{R}^k_+$ 上凹，其延拓（对 $z\not\succeq0$ 取 $-\infty$）对每个分量非减。所以若 $g_i$ 凹且非负，则 $f(x)=(\sum_{i=1}^kg_i(x)^p)^{1/p}$ 凹。
- 设 $p\ge1$，$g_1,\dots,g_k$ 凸且非负，则函数 $(\sum_{i=1}^kg_i(x)^p)^{1/p}$ 凸。为证明，考虑函数 $h:\mathbb{R}^k\to\mathbb{R}$：

```math
h(z)=\Bigl(\sum_{i=1}^k\max\{z_i,0\}^p\Bigr)^{1/p},
```

$\mathbf{dom}\,h=\mathbb{R}^k$，故 $h=\tilde{h}$。这个函数凸且非减，所以 $h(g(x))$ 是 $x$ 的凸函数。当 $z\succeq0$ 时 $h(z)=(\sum_{i=1}^kz_i^p)^{1/p}$，于是结论是 $(\sum_{i=1}^kg_i(x)^p)^{1/p}$ 凸。
- 几何平均 $h(z)=(\prod_{i=1}^kz_i)^{1/k}$ 在 $\mathbb{R}^k_+$ 上凹，其延拓对每个自变量非减。因此若 $g_1,\dots,g_k$ 是非负凹函数，则它们的几何平均 $(\prod_{i=1}^kg_i)^{1/k}$ 也是。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">3.2.5 Minimization（最小化）</span>

<!-- p.101/102 -->

我们已经看到：任意一族凸函数的最大值或上确界是凸的。而某些特殊形式的最小化也产生凸函数。若 $f$ 关于 $(x,y)$ 凸，$C$ 是凸的非空集合，则函数

```math
g(x)=\inf_{y\in C}f(x,y) \tag{3.16}
```

关于 $x$ 凸（只要 $g(x)>-\infty$ 对所有 $x$ 成立）。$g$ 的定义域是 $\mathbf{dom}\,f$ 在其 $x$ 坐标上的投影：

```math
\mathbf{dom}\,g=\{x \mid (x,y)\in\mathbf{dom}\,f \text{ 对某个 } y\in C\}.
```

<!-- p.102 -->

我们对 $x_1,x_2\in\mathbf{dom}\,g$ 验证 Jensen 不等式来证明。设 $\epsilon>0$，则存在 $y_1,y_2\in C$ 使 $f(x_i,y_i)\le g(x_i)+\epsilon$（$i=1,2$）。令 $\theta\in[0,1]$，有

```math
\begin{aligned}
g(\theta x_1+(1-\theta)x_2)
&= \inf_{y\in C}f(\theta x_1+(1-\theta)x_2,\; y)\\
&\le f(\theta x_1+(1-\theta)x_2,\; \theta y_1+(1-\theta)y_2)\\
&\le \theta f(x_1,y_1)+(1-\theta)f(x_2,y_2)\\
&\le \theta g(x_1)+(1-\theta)g(x_2)+\epsilon.
\end{aligned}
```

由 $\epsilon>0$ 任意，得

```math
g(\theta x_1+(1-\theta)x_2)\le\theta g(x_1)+(1-\theta)g(x_2).
```

这个结果也可以用上图来看。按 (3.16) 定义的 $f,g$ 与 $C$，并假设对每个 $x$ 下确界在 $y\in C$ 上达到，则

```math
\mathbf{epi}\,g=\{(x,t) \mid (x,y,t)\in\mathbf{epi}\,f \text{ 对某个 } y\in C\}.
```

于是 $\mathbf{epi}\,g$ 凸，因为它是凸集向部分分量的投影。

<span style="color:#00838f;">**[例 3.15]** **舒尔补（Schur complement）。** 设二次函数

```math
f(x,y)=x^TAx+2x^TBy+y^TCy
```

（$A$ 与 $C$ 对称）关于 $(x,y)$ 凸，这意味着

```math
\begin{bmatrix}A&B\\B^T&C\end{bmatrix}\succeq0.
```

我们可以把 $g(x)=\inf_yf(x,y)$ 表示为

```math
g(x)=x^T(A-BC^\dagger B^T)x,
```

其中 $C^\dagger$ 是 $C$ 的 pseudo-inverse（伪逆）（见 §A.5.4）。由最小化规则，$g$ 凸，所以 $A-BC^\dagger B^T\succeq0$。若 $C$ 可逆（即 $C\succ0$），则矩阵 $A-BC^{-1}B^T$ 称为 $C$ 在矩阵 $\begin{bmatrix}A&B\\B^T&C\end{bmatrix}$ 中的 Schur complement（舒尔补）（见 §A.5.5）。

<span style="color:#00838f;">**[例 3.16]** **到集合的距离。** 点 $x$ 到集合 $S\subseteq\mathbb{R}^n$ 的距离（按范数 $\|\cdot\|$）定义为

```math
\mathbf{dist}(x,S)=\inf_{y\in S}\|x-y\|.
```

函数 $\|x-y\|$ 关于 $(x,y)$ 凸，所以若集合 $S$ 凸，距离函数 $\mathbf{dist}(x,S)$ 是 $x$ 的凸函数。

<span style="color:#00838f;">**[例 3.17]** 设 $h$ 凸。则函数

```math
g(x)=\inf\{h(y) \mid Ay=x\}
```

凸。为看出这一点，定义

```math
f(x,y)=\begin{cases}h(y) & \text{若 } Ay=x\\ \infty & \text{否则}，\end{cases}
```

它关于 $(x,y)$ 凸。则 $g$ 是 $f$ 关于 $y$ 的最小值，因此凸。（直接证明 $g$ 凸也不难。）

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">3.2.6 Perspective of a function（函数的透视）</span>

<!-- p.103 -->

若 $f:\mathbb{R}^n\to\mathbb{R}$，则 $f$ 的 perspective（透视）是函数 $g:\mathbb{R}^{n+1}\to\mathbb{R}$：

```math
g(x,t)=tf(x/t),
```

定义域为

```math
\mathbf{dom}\,g=\{(x,t) \mid x/t\in\mathbf{dom}\,f,\; t>0\}.
```

透视运算保持凸性：若 $f$ 凸，则其透视函数 $g$ 凸。同样，若 $f$ 凹，则 $g$ 凹。

这可以用几种方式证明，例如直接验证定义不等式（见习题 3.33）。这里我们用上图与 §2.3.3 中 $\mathbb{R}^{n+1}$ 上的透视映射给一个简短证明（这也解释了「透视」这个名字）。对 $t>0$：

```math
(x,t,s)\in\mathbf{epi}\,g
\iff tf(x/t)\le s
\iff f(x/t)\le s/t
\iff (x/t,\; s/t)\in\mathbf{epi}\,f.
```

因此 $\mathbf{epi}\,g$ 是 $\mathbf{epi}\,f$ 在「把 $(u,v,w)$ 映到 $(u,w)/v$」的透视映射下的原像。由 §2.3.3 得 $\mathbf{epi}\,g$ 凸，故函数 $g$ 凸。

<span style="color:#00838f;">**[例 3.18]** **欧氏范数平方。** 凸函数 $f(x)=x^Tx$（在 $\mathbb{R}^n$ 上）的透视是

```math
g(x,t)=t(x/t)^T(x/t)=\frac{x^Tx}{t},
```

它在 $(x,t)$ 上凸（$t>0$）。我们可以用另外几种方法推出 $g$ 的凸性：第一，可以把 $g$ 表示为二次-线性分式函数 $x_i^2/t$ 之和（§3.1.5 已证其凸）；也可以把 $g$ 表示为矩阵分式函数 $x^T(tI)^{-1}x$ 的特例（见例 3.4）。

<span style="color:#00838f;">**[例 3.19]** **负对数。** 考虑凸函数 $f(x)=-\log x$（$\mathbb{R}_{++}$ 上）。其透视是

```math
g(x,t)=-t\log(x/t)=t\log(t/x)=t\log t-t\log x,
```

在 $\mathbb{R}^2_{++}$ 上凸。函数 $g$ 称为 $t$ 与 $x$ 的 relative entropy（相对熵）。当 $x=1$ 时 $g$ 退化为负熵函数。

<!-- p.104 -->

由 $g$ 的凸性可以确立若干有趣的有关函数的凸性或凹性。首先，两个向量 $u,v\in\mathbb{R}^n_{++}$ 的相对熵，定义为

```math
\sum_{i=1}^nu_i\log(u_i/v_i),
```

关于 $(u,v)$ 凸，因为它是 $u_i,v_i$ 的相对熵之和。

一个密切相关的函数是 $u,v\in\mathbb{R}^n_{++}$ 之间的 Kullback-Leibler divergence（Kullback-Leibler 散度）：

```math
D_{kl}(u,v)=\sum_{i=1}^n\bigl(u_i\log(u_i/v_i)-u_i+v_i\bigr), \tag{3.17}
```

它是凸的，因为它是相对熵加上一个 $(u,v)$ 的线性函数。KL 散度满足 $D_{kl}(u,v)\ge0$，且 $D_{kl}(u,v)=0$ 当且仅当 $u=v$，因此可用作两个正向量之间 deviation（偏离）的度量；见习题 3.13。（注意当 $u,v$ 是 probability vectors（概率向量），即 $\mathbf{1}^Tu=\mathbf{1}^Tv=1$ 时，相对熵与 KL 散度相同。）

若在相对熵函数中取 $v_i=\mathbf{1}^Tu$，则得到 $u\in\mathbb{R}^n_{++}$ 的一个凹（且齐次）函数：

```math
\sum_{i=1}^nu_i\log(\mathbf{1}^Tu/u_i)=(\mathbf{1}^Tu)\sum_{i=1}^nz_i\log(1/z_i),
```

其中 $z=u/(\mathbf{1}^Tu)$，称为 normalized entropy function（归一化熵函数）。向量 $z=u/\mathbf{1}^Tu$ 是 normalized vector（归一化向量）或 probability distribution（概率分布）（分量和为 1）；$u$ 的归一化熵是这个归一化分布的熵乘以 $\mathbf{1}^Tu$。

<span style="color:#00838f;">**[例 3.20]** 设 $f:\mathbb{R}^m\to\mathbb{R}$ 凸，$A\in\mathbb{R}^{m\times n}$，$b\in\mathbb{R}^m$，$c\in\mathbb{R}^n$，$d\in\mathbb{R}$。定义

```math
g(x)=(c^Tx+d)\,f\bigl((Ax+b)/(c^Tx+d)\bigr),
```

其中

```math
\mathbf{dom}\,g=\{x \mid c^Tx+d>0,\; (Ax+b)/(c^Tx+d)\in\mathbf{dom}\,f\}.
```

则 $g$ 凸。

> <span style="color:#2471a3;">**[译者注｜跨课程连接]** 例 3.19 的 relative entropy 与 KL 散度是信息论（MacKay 书 Ch.1-2）的核心量：「相对熵是凸的」这一条分析性质，正是编码定理与 EM 算法收敛性的共同根。两门课在这里交汇——把它当作信号点记牢。</span>

---

> <span style="color:#7f8c8d;">§3.1–3.2 翻译完毕。§3.3 共轭函数起见 boyd_ch03b.bilingual.md。</span>
