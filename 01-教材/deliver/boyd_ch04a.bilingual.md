# <span style="color:#c0392b;">Boyd《凸优化》第 4 章 凸优化问题（上：§4.1–4.2）</span>

> <span style="color:#7f8c8d;">Source: 教材/Convex Optimization (Boyd, Vandenberghe).pdf · Chapter 4 §4.1–4.2, pp.127–146 (PDF p.141–160) · 行内双语翻译版 · 译者：ZCode（bilingual-translator 铁律）</span>

## <span style="color:#c0392b;">4.1 Optimization problems（优化问题）</span>

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">4.1.1 Basic terminology（基本术语）</span>

<!-- p.141 -->

我们用记号

```math
\begin{aligned}
\text{minimize}\quad & f_0(x)\\
\text{subject to}\quad & f_i(x)\le0, && i=1,\dots,m\\
& h_i(x)=0, && i=1,\dots,p
\end{aligned} \tag{4.1}
```

描述「在所有满足 $f_i(x)\le0$（$i=1,\dots,m$）与 $h_i(x)=0$（$i=1,\dots,p$）的 $x$ 中，找出使 $f_0(x)$ 最小的 $x$」这一问题。我们称 $x\in\mathbb{R}^n$ 为 optimization variable（优化变量），$f_0:\mathbb{R}^n\to\mathbb{R}$ 为 objective function（目标函数）或 cost function（成本函数）。不等式 $f_i(x)\le0$ 称为 inequality constraints（不等式约束），相应函数 $f_i:\mathbb{R}^n\to\mathbb{R}$ 称为 inequality constraint functions（不等式约束函数）。等式 $h_i(x)=0$ 称为 equality constraints（等式约束），函数 $h_i:\mathbb{R}^n\to\mathbb{R}$ 称为 equality constraint functions（等式约束函数）。若没有约束（即 $m=p=0$），则称问题 (4.1) 是 unconstrained（无约束）的。

目标函数与所有约束函数都有定义的点集

```math
\mathcal{D}=\bigcap_{i=0}^m\mathbf{dom}\,f_i\ \cap\ \bigcap_{i=1}^p\mathbf{dom}\,h_i
```

称为优化问题 (4.1) 的 domain（定义域）。点 $x\in\mathcal{D}$ 是 feasible（可行）的，如果它满足约束 $f_i(x)\le0$（$i=1,\dots,m$）与 $h_i(x)=0$（$i=1,\dots,p$）。若至少存在一个可行点，则称问题 (4.1) 是 feasible（可行）的，否则 infeasible（不可行）。所有可行点的集合称为 feasible set（可行集）或 constraint set（约束集）。

<!-- p.142 -->

问题 (4.1) 的 optimal value（最优值）$p^\star$ 定义为

```math
p^\star=\inf\{f_0(x) \mid f_i(x)\le0,\ i=1,\dots,m,\; h_i(x)=0,\ i=1,\dots,p\}.
```

我们允许 $p^\star$ 取扩值 $\pm\infty$。若问题不可行，则 $p^\star=\infty$（遵循空集下确界为 $\infty$ 的标准约定）。若存在可行点 $x_k$ 使 $f_0(x_k)\to-\infty$（$k\to\infty$），则 $p^\star=-\infty$，称问题 (4.1) 是 unbounded below（无下界）的。

**最优点与局部最优点。** 若 $x^\star$ 可行且 $f_0(x^\star)=p^\star$，则称 $x^\star$ 是一个 optimal point（最优点），或称它 solves（求解）问题 (4.1)。所有最优点的集合是 optimal set（最优集）：

```math
X_{\mathrm{opt}}=\{x \mid f_i(x)\le0,\ i=1,\dots,m,\; h_i(x)=0,\ i=1,\dots,p,\; f_0(x)=p^\star\}.
```

若问题 (4.1) 存在最优点，则称最优值 is attained 或 achieved（被达到），问题 solvable（可解）。若 $X_{\mathrm{opt}}$ 为空，则称最优值 not attained（未被达到）。（问题无下界时总会发生。）满足 $f_0(x)\le p^\star+\epsilon$（$\epsilon>0$）的可行点 $x$ 称为 ǫ-suboptimal（$\epsilon$-次优）点，所有 $\epsilon$-次优点的集合称为问题 (4.1) 的 ǫ-suboptimal set（$\epsilon$-次优集）。

我们称可行点 $x$ 是 locally optimal（局部最优）的，若存在 $R>0$ 使

```math
f_0(x)=\inf\{f_0(z) \mid f_i(z)\le0,\ i=1,\dots,m,\; h_i(z)=0,\ i=1,\dots,p,\; \|z-x\|_2\le R\},
```

换言之，$x$ 求解了把额外约束 $\|z-x\|_2\le R$ 加入后的优化问题。粗略地说，$x$ 在可行集的邻近点上最小化 $f_0$。术语 globally optimal（全局最优）有时用来区分「局部最优」与「最优」。本书中 optimal 一律指全局最优。

若 $x$ 可行且 $f_i(x)=0$，则称第 $i$ 个不等式约束 $f_i(x)\le0$ 在 $x$ 处是 active（起作用/激活）的；若 $f_i(x)<0$，则称该约束 inactive（不起作用）的。（等式约束在所有可行点处都起作用。）若删去某个约束不改变可行集，则称该约束 redundant（冗余）。

<span style="color:#00838f;">**[例 4.1]** 用几个简单的无约束优化问题（变量 $x\in\mathbb{R}$，$\mathbf{dom}\,f_0=\mathbb{R}_{++}$）说明这些定义：

- $f_0(x)=1/x$：$p^\star=0$，但最优值未被达到。
- $f_0(x)=-\log x$：$p^\star=-\infty$，问题无下界。
- $f_0(x)=x\log x$：$p^\star=-1/e$，在唯一最优点 $x^\star=1/e$ 处达到。

<!-- p.142/143 -->

**可行性问题（Feasibility problems）。** 若目标函数恒为零，则最优值要么为零（可行集非空），要么为 $\infty$（可行集为空）。这称为 feasibility problem（可行性问题），有时写作

```math
\begin{aligned}
\text{find}\quad & x\\
\text{subject to}\quad & f_i(x)\le0, && i=1,\dots,m\\
& h_i(x)=0, && i=1,\dots,p.
\end{aligned}
```

可行性问题就是判定约束是否 consistent（相容），若相容则找出一个满足它们的点。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">4.1.2 Expressing problems in standard form（以标准形式表达问题）</span>

我们把 (4.1) 称为 standard form（标准形式）的优化问题。标准形式约定不等式与等式约束的右端为零。这总可以通过减去非零右端来安排：例如把等式约束 $g_i(x)=\tilde{g}_i(x)$ 表示为 $h_i(x)=0$，其中 $h_i(x)=g_i(x)-\tilde{g}_i(x)$。类似地把形如 $f_i(x)\ge0$ 的不等式表示为 $-f_i(x)\le0$。

<span style="color:#00838f;">**[例 4.2]** **箱约束（box constraints）。** 考虑优化问题

```math
\begin{aligned}
\text{minimize}\quad & f_0(x)\\
\text{subject to}\quad & l_i\le x_i\le u_i,\quad i=1,\dots,n,
\end{aligned}
```

其中 $x\in\mathbb{R}^n$ 是变量。这些约束称为 variable bounds（变量界，因为它们给出每个 $x_i$ 的上下界）或 box constraints（箱约束，因为可行集是一个 box（箱））。用标准形式表达为

```math
\begin{aligned}
\text{minimize}\quad & f_0(x)\\
\text{subject to}\quad & l_i-x_i\le0, && i=1,\dots,n\\
& x_i-u_i\le0, && i=1,\dots,n.
\end{aligned}
```

共有 $2n$ 个不等式约束函数：$f_i(x)=l_i-x_i$（$i=1,\dots,n$）与 $f_i(x)=x_{i-n}-u_{i-n}$（$i=n+1,\dots,2n$）。

<!-- p.143/144 -->

**最大化问题。** 按惯例我们集中讨论最小化问题。最大化问题

```math
\begin{aligned}
\text{maximize}\quad & f_0(x)\\
\text{subject to}\quad & f_i(x)\le0,\; i=1,\dots,m\\
& h_i(x)=0,\; i=1,\dots,p
\end{aligned} \tag{4.2}
```

可以通过在相同约束下最小化 $-f_0$ 来求解。借助这种对应，可以为最大化问题 (4.2) 定义上面的所有术语。例如 (4.2) 的最优值定义为

```math
p^\star=\sup\{f_0(x) \mid f_i(x)\le0,\ i=1,\dots,m,\; h_i(x)=0,\ i=1,\dots,p\},
```

可行点 $x$ 满足 $f_0(x)\ge p^\star-\epsilon$ 时称为 $\epsilon$-次优。考虑最大化问题时，目标有时不称 cost（成本）而称 utility（效用）或 satisfaction level（满意度）。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">4.1.3 Equivalent problems（等价问题）</span>

<!-- p.144 -->

本书以非正式的方式使用「优化问题等价」的概念：若从一个问题的解能容易地得到另一个问题的解，反之亦然，则称两个问题 equivalent（等价）。（给出等价的正式定义是可能的，但很复杂。）

举一个简单例子。考虑问题

```math
\begin{aligned}
\text{minimize}\quad & \tilde{f}(x)=\alpha_0f_0(x)\\
\text{subject to}\quad & \tilde{f}_i(x)=\alpha_if_i(x)\le0, && i=1,\dots,m\\
& \tilde{h}_i(x)=\beta_ih_i(x)=0, && i=1,\dots,p,
\end{aligned} \tag{4.3}
```

其中 $\alpha_i>0$（$i=0,\dots,m$），$\beta_i\neq0$（$i=1,\dots,p$）。这个问题由标准形式问题 (4.1) 通过把目标与不等式约束函数乘正的常数、把等式约束函数乘非零常数得到。因此问题 (4.3) 与原问题 (4.1) 的可行集相同；$x$ 对原问题 (4.1) 最优当且仅当它对缩放后的问题 (4.3) 最优，所以两个问题等价。但两个问题 (4.1) 与 (4.3) 并不相同（除非所有 $\alpha_i,\beta_i$ 等于 1），因为目标与约束函数不同。

下面描述一些产生等价问题的一般变换。

**变量替换（Change of variables）。** 设 $\phi:\mathbb{R}^n\to\mathbb{R}^n$ 是一一映射，其像覆盖问题定义域 $\mathcal{D}$，即 $\phi(\mathbf{dom}\,\phi)\supseteq\mathcal{D}$。定义函数 $\tilde{f}_i$ 与 $\tilde{h}_i$：

```math
\tilde{f}_i(z)=f_i(\phi(z)),\ i=0,\dots,m;\qquad \tilde{h}_i(z)=h_i(\phi(z)),\ i=1,\dots,p.
```

考虑问题

```math
\begin{aligned}
\text{minimize}\quad & \tilde{f}_0(z)\\
\text{subject to}\quad & \tilde{f}_i(z)\le0, && i=1,\dots,m\\
& \tilde{h}_i(z)=0, && i=1,\dots,p,
\end{aligned} \tag{4.4}
```

变量为 $z$。我们说标准形式问题 (4.1) 与问题 (4.4) 通过 change of variable（变量替换）或 substitution of variable $x=\phi(z)$ 相联系。两个问题显然等价：若 $x$ 求解 (4.1)，则 $z=\phi^{-1}(x)$ 求解 (4.4)；若 $z$ 求解 (4.4)，则 $x=\phi(z)$ 求解 (4.1)。

<!-- p.145 -->

**目标与约束函数的变换。** 设 $\psi_0:\mathbb{R}\to\mathbb{R}$ 单调递增，$\psi_1,\dots,\psi_m:\mathbb{R}\to\mathbb{R}$ 满足 $\psi_i(u)\le0$ 当且仅当 $u\le0$，$\psi_{m+1},\dots,\psi_{m+p}:\mathbb{R}\to\mathbb{R}$ 满足 $\psi_i(u)=0$ 当且仅当 $u=0$。定义复合函数

```math
\tilde{f}_i(x)=\psi_i(f_i(x)),\ i=0,\dots,m;\qquad \tilde{h}_i(x)=\psi_{m+i}(h_i(x)),\ i=1,\dots,p.
```

显然相应的问题与标准形式问题 (4.1) 等价：可行集相同，最优点相同。（上面例 (4.3) 中用适当常数缩放目标与约束函数，是所有 $\psi_i$ 均为线性的特例。）

<span style="color:#00838f;">**[例 4.3]** **最小范数与最小范数平方问题。** 一个简单的例子是无约束欧氏范数最小化问题

```math
\text{minimize}\quad \|Ax-b\|_2, \tag{4.5}
```

变量 $x\in\mathbb{R}^n$。由于范数总是非负的，我们同样可以求解

```math
\text{minimize}\quad \|Ax-b\|_2^2=(Ax-b)^T(Ax-b), \tag{4.6}
```

即最小化欧氏范数的平方。问题 (4.5) 与 (4.6) 显然等价，最优点相同。但两个问题不同：例如 (4.5) 的目标在任何满足 $Ax-b=0$ 的 $x$ 处不可微，而 (4.6) 的目标对所有 $x$ 可微（其实是二次的）。

**松弛变量（Slack variables）。** 一个简单变换基于观察：$f_i(x)\le0$ 当且仅当存在 $s_i\ge0$ 满足 $f_i(x)+s_i=0$。利用这个变换得到问题

```math
\begin{aligned}
\text{minimize}\quad & f_0(x)\\
\text{subject to}\quad & s_i\ge0, && i=1,\dots,m\\
& f_i(x)+s_i=0, && i=1,\dots,m\\
& h_i(x)=0, && i=1,\dots,p,
\end{aligned} \tag{4.7}
```

变量为 $x\in\mathbb{R}^n$ 与 $s\in\mathbb{R}^m$。这个问题有 $n+m$ 个变量、$m$ 个不等式约束（$s_i$ 的非负性）与 $m+p$ 个等式约束。新变量 $s_i$ 称为与原不等式约束 $f_i(x)\le0$ 相关联的 slack variable（松弛变量）。引入松弛变量把每个不等式约束替换为一个等式约束加一个非负性约束。问题 (4.7) 与原标准形式问题 (4.1) 等价：若 $(x,s)$ 对 (4.7) 可行，则 $x$ 对原问题可行（因为 $s_i=-f_i(x)\ge0$）；反之若 $x$ 对原问题可行，则 $(x,s)$ 对 (4.7) 可行（取 $s_i=-f_i(x)$）。类似地，$x$ 对原问题 (4.1) 最优当且仅当 $(x,s)$（$s_i=-f_i(x)$）对问题 (4.7) 最优。

<!-- p.146 -->

**消去等式约束（Eliminating equality constraints）。** 若能用某个参数 $z\in\mathbb{R}^k$ 显式参数化等式约束

```math
h_i(x)=0,\qquad i=1,\dots,p \tag{4.8}
```

的所有解，则可以按下述方法从问题中消去等式约束。设函数 $\phi:\mathbb{R}^k\to\mathbb{R}^n$ 满足：$x$ 满足 (4.8) 当且仅当存在某个 $z\in\mathbb{R}^k$ 使 $x=\phi(z)$。则优化问题

```math
\begin{aligned}
\text{minimize}\quad & \tilde{f}_0(z)=f_0(\phi(z))\\
\text{subject to}\quad & \tilde{f}_i(z)=f_i(\phi(z))\le0,\quad i=1,\dots,m
\end{aligned}
```

与原问题 (4.1) 等价。这个变换后的问题变量为 $z\in\mathbb{R}^k$，有 $m$ 个不等式约束、没有等式约束。若 $z$ 对变换后的问题最优，则 $x=\phi(z)$ 对原问题最优。反之若 $x$ 对原问题最优，则（$x$ 可行）至少存在一个 $z$ 使 $x=\phi(z)$；任何这样的 $z$ 都对变换后的问题最优。

**消去线性等式约束。** 当等式约束都是线性的（形如 $Ax=b$）时，消去变量的过程可以更显式地描述，也容易数值执行。若 $Ax=b$ 不相容（即 $b\notin\mathcal{R}(A)$），则原问题不可行。假设不是这种情况，令 $x_0$ 表示等式约束的任一解，$F\in\mathbb{R}^{n\times k}$ 是任何满足 $\mathcal{R}(F)=\mathcal{N}(A)$ 的矩阵，则线性方程 $Ax=b$ 的一般解为 $Fz+x_0$（$z\in\mathbb{R}^k$）。（可取 $F$ 满秩，此时 $k=n-\mathbf{rank}A$。）把 $x=Fz+x_0$ 代入原问题，得到问题

```math
\begin{aligned}
\text{minimize}\quad & f_0(Fz+x_0)\\
\text{subject to}\quad & f_i(Fz+x_0)\le0,\quad i=1,\dots,m,
\end{aligned}
```

变量为 $z$，它与原问题等价，没有等式约束，变量减少了 $\mathbf{rank}A$ 个。

**引入等式约束。** 我们也可以向问题中引入等式约束与新变量。一般情形复杂且启发不大，我们给出一个后面有用的典型例子。考虑问题

```math
\begin{aligned}
\text{minimize}\quad & f_0(A_0x+b_0)\\
\text{subject to}\quad & f_i(A_ix+b_i)\le0, && i=1,\dots,m\\
& h_i(x)=0, && i=1,\dots,p,
\end{aligned}
```

<!-- p.147 -->

其中 $x\in\mathbb{R}^n$，$A_i\in\mathbb{R}^{k_i\times n}$，$f_i:\mathbb{R}^{k_i}\to\mathbb{R}$。此问题中目标与约束函数是 $f_i$ 与由 $A_ix+b_i$ 定义的仿射变换的复合。

引入新变量 $y_i\in\mathbb{R}^{k_i}$ 与新等式约束 $y_i=A_ix+b_i$（$i=0,\dots,m$），构成等价问题

```math
\begin{aligned}
\text{minimize}\quad & f_0(y_0)\\
\text{subject to}\quad & f_i(y_i)\le0, && i=1,\dots,m\\
& y_i=A_ix+b_i, && i=0,\dots,m\\
& h_i(x)=0, && i=1,\dots,p.
\end{aligned}
```

此问题有 $k_0+\cdots+k_m$ 个新变量 $y_0\in\mathbb{R}^{k_0},\dots,y_m\in\mathbb{R}^{k_m}$，以及 $k_0+\cdots+k_m$ 个新等式约束 $y_0=A_0x+b_0,\dots,y_m=A_mx+b_m$。此问题的目标与不等式约束是 independent（相互独立）的，即涉及不同的优化变量。

**对部分变量优化（Optimizing over some variables）。** 我们总有

```math
\inf_{x,y}f(x,y)=\inf_x\tilde{f}(x),\qquad \tilde{f}(x)=\inf_y f(x,y).
```

换言之，总可以先对部分变量最小化，再对剩余变量最小化。这个简单而一般的原则可用于把问题变换为等价形式。一般情形繁琐且无启发，我们描述一个例子。

设变量 $x\in\mathbb{R}^n$ 划分为 $x=(x_1,x_2)$，$x_1\in\mathbb{R}^{n_1}$，$x_2\in\mathbb{R}^{n_2}$，$n_1+n_2=n$。考虑问题

```math
\begin{aligned}
\text{minimize}\quad & f_0(x_1,x_2)\\
\text{subject to}\quad & f_i(x_1)\le0, && i=1,\dots,m_1\\
& \tilde{f}_i(x_2)\le0, && i=1,\dots,m_2,
\end{aligned} \tag{4.9}
```

其中约束是 independent（独立）的：每个约束函数只依赖 $x_1$ 或 $x_2$。先对 $x_2$ 最小化。定义 $x_1$ 的函数

```math
\tilde{f}_0(x_1)=\inf\{f_0(x_1,z) \mid \tilde{f}_i(z)\le0,\ i=1,\dots,m_2\}.
```

则问题 (4.9) 等价于

```math
\begin{aligned}
\text{minimize}\quad & \tilde{f}_0(x_1)\\
\text{subject to}\quad & f_i(x_1)\le0,\quad i=1,\dots,m_1. \tag{4.10}
\end{aligned}
```

<!-- p.148 -->

<span style="color:#00838f;">**[例 4.4]** **带部分变量约束的二次函数最小化。** 考虑严格凸二次目标、部分变量无约束的问题：

```math
\begin{aligned}
\text{minimize}\quad & x_1^TP_{11}x_1+2x_1^TP_{12}x_2+x_2^TP_{22}x_2\\
\text{subject to}\quad & f_i(x_1)\le0,\quad i=1,\dots,m,
\end{aligned}
```

其中 $P_{11}$、$P_{22}$ 对称。这里可以对 $x_2$ 解析地最小化：

```math
\inf_{x_2}\bigl(x_1^TP_{11}x_1+2x_1^TP_{12}x_2+x_2^TP_{22}x_2\bigr)=x_1^T\bigl(P_{11}-P_{12}P_{22}^{-1}P_{12}^T\bigr)x_1
```

（见 §A.5.5）。因此原问题等价于

```math
\begin{aligned}
\text{minimize}\quad & x_1^T\bigl(P_{11}-P_{12}P_{22}^{-1}P_{12}^T\bigr)x_1\\
\text{subject to}\quad & f_i(x_1)\le0,\quad i=1,\dots,m.
\end{aligned}
```

**上图形式（Epigraph form）。** 标准问题 (4.1) 的 epigraph form（上图形式）是问题

```math
\begin{aligned}
\text{minimize}\quad & t\\
\text{subject to}\quad & f_0(x)-t\le0\\
& f_i(x)\le0, && i=1,\dots,m\\
& h_i(x)=0, && i=1,\dots,p,
\end{aligned} \tag{4.11}
```

变量为 $x\in\mathbb{R}^n$ 与 $t\in\mathbb{R}$。容易看出它与原问题等价：$(x,t)$ 对 (4.11) 最优当且仅当 $x$ 对 (4.1) 最优且 $t=f_0(x)$。注意上图形式问题的目标函数是变量 $x,t$ 的 linear function（线性函数）。

上图形式问题 (4.11) 可以几何化地解释为「graph space（图像空间）」$(x,t)$ 中的优化问题：我们在 $f_0$ 的 epigraph（上图）上最小化 $t$，受 $x$ 上的约束（见图 4.1）。

<!-- p.148/149 -->

**隐式与显式约束。** 通过 §3.1.2 已提到的简单技巧，可以通过重新定义目标函数的定义域把任何约束隐式地并入目标函数。极端例子：标准形式问题可以表示为无约束问题

```math
\text{minimize}\quad F(x), \tag{4.12}
```

其中把 $F$ 定义为 $f_0$ 但定义域限制到可行集：

```math
\mathbf{dom}\,F=\{x\in\mathbf{dom}\,f_0 \mid f_i(x)\le0,\ i=1,\dots,m,\; h_i(x)=0,\ i=1,\dots,p\},
```

且对 $x\in\mathbf{dom}\,F$ 有 $F(x)=f_0(x)$。（等价地，可定义 $F(x)$ 在不可行点取值 $\infty$。）问题 (4.1) 与 (4.12) 显然等价：可行集、最优点、最优值都相同。

<!-- p.149 -->

当然这个变换不过是一个记号把戏。把约束隐式化并没有使问题更易分析或求解——尽管问题 (4.12) 名义上无约束，变换在某些方面反而使问题更难。例如设原问题的目标 $f_0$ 可微（从而其定义域开），受限的目标函数 $F$ 可能不可微，因为其定义域很可能不开。

反过来，我们也会遇到带隐式约束的问题，然后可以把它显式化。简单例子：考虑无约束问题

```math
\text{minimize}\quad f(x), \tag{4.13}
```

其中

```math
f(x)=\begin{cases}x^Tx & Ax=b\\ \infty & \text{否则}.\end{cases}
```

于是目标函数在 $Ax=b$ 定义的仿射集上等于二次型 $x^Tx$，在仿射集外为 $\infty$。既然显然只需关注满足 $Ax=b$ 的点，我们说问题 (4.13) 的目标中隐藏着一个 implicit equality constraint（隐式等式约束）$Ax=b$。可以通过构造等价问题把隐式等式约束显式化：

```math
\begin{aligned}
\text{minimize}\quad & x^Tx\\
\text{subject to}\quad & Ax=b. \tag{4.14}
\end{aligned}
```

问题 (4.13) 与 (4.14) 显然等价但不相同：(4.13) 无约束但目标不可微；(4.14) 有等式约束但目标与约束函数可微。

<!-- p.150 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">4.1.4 Parameter and oracle problem descriptions（参数描述与神谕描述）</span>

对标准形式 (4.1) 的问题，还有一个问题：目标与约束函数是如何规定的。许多情形这些函数有解析或 closed form（闭式）表达，即由涉及变量 $x$ 与一些参数的公式给出。例如目标为二次的：$f_0(x)=(1/2)x^TPx+q^Tx+r$。为规定目标函数，给出系数（也称 problem parameters 问题参数或 problem data 问题数据）$P\in\mathbf{S}^n$、$q\in\mathbb{R}^n$、$r\in\mathbb{R}$。我们称之为 parameter problem description（参数问题描述）：要解的具体问题（即 problem instance 问题实例）通过给出目标与约束函数表达式中各参数的值来规定。

其他情形目标与约束函数由 oracle models（神谕模型，也称 black box 黑箱或 subroutine models 子程序模型）描述。在神谕模型中，我们不显式知道 $f$，但可以在任何 $x\in\mathbf{dom}\,f$ 处求值 $f(x)$（通常还有某些导数）。这称为 querying the oracle（询问神谕），通常伴随某种代价（如时间）。我们还会得到关于函数的一些 prior information（先验信息），如凸性与值的界。神谕模型的具体例子：无约束问题中最小化函数 $f$。函数值 $f(x)$ 与梯度 $\nabla f(x)$ 在一个子程序中求值；我们可以在任何 $x\in\mathbf{dom}\,f$ 调用该子程序，但看不到其源代码。以 $x$ 为参数调用子程序会返回 $f(x)$ 与 $\nabla f(x)$。注意在神谕模型中，我们从不知道函数本身，只知道询问过的点上的函数值（与某些导数）。（我们也知道一些给定的先验信息，如可微性与凸性。）

实际中参数描述与神谕描述的区分并不截然。给定参数描述，可以为它构造一个神谕（询问时求值所需函数与导数即可）。第三部分研究的大多数算法以神谕模型工作，但当限制到特定的参数化问题族时可以更高效。

## <span style="color:#c0392b;">4.2 Convex optimization（凸优化）</span>

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">4.2.1 Convex optimization problems in standard form（标准形式凸优化问题）</span>

<!-- p.150/151 -->

convex optimization problem（凸优化问题）是形如

```math
\begin{aligned}
\text{minimize}\quad & f_0(x)\\
\text{subject to}\quad & f_i(x)\le0, && i=1,\dots,m\\
& a_i^Tx=b_i, && i=1,\dots,p
\end{aligned} \tag{4.15}
```

的问题，其中 $f_0,\dots,f_m$ 是凸函数。与一般标准形式问题 (4.1) 比较，凸问题有三个附加要求：

<!-- p.151 -->

- 目标函数必须是凸的；
- 不等式约束函数必须是凸的；
- 等式约束函数 $h_i(x)=a_i^Tx-b_i$ 必须是仿射的。

我们立即注意到一个重要性质：凸优化问题的可行集是凸的，因为它是问题定义域

```math
\mathcal{D}=\bigcap_{i=0}^m\mathbf{dom}\,f_i
```

（凸集）与 $m$ 个（凸的）下水平集 $\{x \mid f_i(x)\le0\}$ 及 $p$ 个超平面 $\{x \mid a_i^Tx=b_i\}$ 的交。（不失一般性可设 $a_i\neq0$：若某 $i$ 有 $a_i=0,b_i=0$，则第 $i$ 个等式约束可删去；若 $a_i=0,b_i\neq0$，则第 $i$ 个等式约束不相容，问题不可行。）因此，在凸优化问题中，我们在一个凸集上最小化一个凸目标函数。

若 $f_0$ 是 quasiconvex（拟凸）而非凸，则称问题 (4.15) 是（标准形式的）quasiconvex optimization problem（拟凸优化问题）。由于凸或拟凸函数的下水平集凸，我们得出：对凸或拟凸优化问题，$\epsilon$-次优集是凸的；特别地最优集凸。若目标严格凸，则最优集至多包含一个点。

**凹最大化问题。** 稍微滥用记号，若目标函数 $f_0$ 凹、不等式约束函数 $f_1,\dots,f_m$ 凸，我们也把

```math
\begin{aligned}
\text{maximize}\quad & f_0(x)\\
\text{subject to}\quad & f_i(x)\le0, && i=1,\dots,m\\
& a_i^Tx=b_i, && i=1,\dots,p
\end{aligned} \tag{4.16}
```

称为凸优化问题。这个 concave maximization problem（凹最大化问题）很容易通过最小化凸目标 $-f_0$ 来求解。我们为最小化问题描述的所有结果、结论与算法都容易移植到最大化情形。类似地，最大化问题 (4.16) 当 $f_0$ 拟凹时称为拟凸的。

<!-- p.151/152 -->

**抽象形式凸优化问题。** 注意凸优化问题定义中的一个 subtlety（微妙之处）。考虑 $x\in\mathbb{R}^2$ 的例子：

```math
\begin{aligned}
\text{minimize}\quad & f_0(x)=x_1^2+x_2^2\\
\text{subject to}\quad & f_1(x)=x_1/(1+x_2^2)\le0\\
& h_1(x)=(x_1+x_2)^2=0,
\end{aligned} \tag{4.17}
```

它是标准形式 (4.1) 的。这个问题不是标准形式的凸优化问题，因为等式约束函数 $h_1$ 不是仿射的，

<!-- p.152 -->

不等式约束函数 $f_1$ 不凸。尽管如此，可行集 $\{x \mid x_1\le0,\ x_1+x_2=0\}$ 是凸的。所以虽然此问题是在凸集上最小化凸函数 $f_0$，按我们的定义它不是凸优化问题。

当然，这个问题很容易重新表述为

```math
\begin{aligned}
\text{minimize}\quad & f_0(x)=x_1^2+x_2^2\\
\text{subject to}\quad & \tilde{f}_1(x)=x_1\le0\\
& \tilde{h}_1(x)=x_1+x_2=0,
\end{aligned} \tag{4.18}
```

它是标准凸优化形式：$f_0$ 与 $\tilde{f}_1$ 凸，$\tilde{h}_1$ 仿射。

一些作者用术语 abstract convex optimization problem（抽象凸优化问题）描述「在凸集上最小化凸函数」这一（抽象的）问题。按这种术语，问题 (4.17) 是抽象凸优化问题。本书不使用这种术语。对我们而言，凸优化问题不只是「在凸集上最小化凸函数」；还要求可行集具体地由涉及凸函数的不等式组与线性等式约束描述。问题 (4.17) 不是凸优化问题，而问题 (4.18) 是。（不过两个问题等价。）

我们采用较严格的凸优化问题定义在实践中无碍。要求解「在凸集上最小化凸函数」的抽象问题，需要找到该集合用凸不等式与线性等式约束的描述。如上面的例子所示，这通常很直接。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">4.2.2 Local and global optima（局部与全局最优）</span>

凸优化问题的一个基本性质：任何局部最优点也是（全局）最优的。为看出这一点，设 $x$ 是凸优化问题的局部最优点，即 $x$ 可行且对某个 $R>0$：

```math
f_0(x)=\inf\{f_0(z) \mid z \text{ 可行}，\ \|z-x\|_2\le R\}. \tag{4.19}
```

再设 $x$ 不是全局最优，即存在可行 $y$ 使 $f_0(y)\lt f_0(x)$。显然 $\|y-x\|_2>R$（否则 $f_0(x)\le f_0(y)$）。考虑点

```math
z=(1-\theta)x+\theta y,\qquad \theta=\frac{R}{2\|y-x\|_2}.
```

则 $\|z-x\|_2=R/2\lt R$，且由可行集凸性，$z$ 可行。由 $f_0$ 凸性：

```math
f_0(z)\le(1-\theta)f_0(x)+\theta f_0(y)<f_0(x),
```

与 (4.19) 矛盾。因此不存在满足 $f_0(y)\lt f_0(x)$ 的可行 $y$，即 $x$ 全局最优。

<!-- p.153 -->

拟凸优化问题的局部最优点并不都是全局最优的；见 §4.2.5。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">4.2.3 An optimality criterion for differentiable f₀（可微目标的最优性判据）</span>

设凸优化问题的目标 $f_0$ 可微，则对所有 $x,y\in\mathbf{dom}\,f_0$：

```math
f_0(y)\ge f_0(x)+\nabla f_0(x)^T(y-x) \tag{4.20}
```

（见 §3.1.3）。令 $\mathcal{X}$ 表示可行集：

```math
\mathcal{X}=\{x \mid f_i(x)\le0,\ i=1,\dots,m,\; h_i(x)=0,\ i=1,\dots,p\}.
```

则 **$x$ 最优当且仅当 $x\in\mathcal{X}$ 且**

```math
\nabla f_0(x)^T(y-x)\ge0 \text{ 对所有 } y\in\mathcal{X}. \tag{4.21}
```

这个最优性判据可以几何化理解：若 $\nabla f_0(x)\neq0$，它意味着 $-\nabla f_0(x)$ 定义了可行集在 $x$ 处的支撑超平面（见图 4.2）。

**最优性条件的证明。** 先设 $x\in\mathcal{X}$ 且满足 (4.21)。则若 $y\in\mathcal{X}$，由 (4.20) 得 $f_0(y)\ge f_0(x)$，即 $x$ 是 (4.1) 的最优点。

反之设 $x$ 最优，但条件 (4.21) 不成立，即对某个 $y\in\mathcal{X}$ 有

```math
\nabla f_0(x)^T(y-x)<0.
```

<!-- p.154 -->

考虑点 $z(t)=ty+(1-t)x$（$t\in[0,1]$ 参数）。由于 $z(t)$ 在 $x$ 与 $y$ 的线段上且可行集凸，$z(t)$ 可行。我们断言对小的正 $t$ 有 $f_0(z(t))\lt f_0(x)$，这将证明 $x$ 非最优。为此注意

```math
\left.\frac{d}{dt}f_0(z(t))\right|_{t=0}=\nabla f_0(x)^T(y-x)<0,
```

所以对小的正 $t$ 有 $f_0(z(t))\lt f_0(x)$。

我们将在第 5 章更深入地研究最优性条件，这里先看几个简单例子。

**无约束问题。** 对无约束问题（$m=p=0$），条件 (4.21) 退化为著名充要条件

```math
\nabla f_0(x)=0 \tag{4.22}
```

（$x$ 最优）。虽然我们已见过这个最优性条件，看看它如何从 (4.21) 推出仍有用。设 $x$ 最优（即 $x\in\mathbf{dom}\,f_0$），且对所有可行 $y$ 有 $\nabla f_0(x)^T(y-x)\ge0$。由于 $f_0$ 可微，其定义域（按定义）是开的，所以所有充分接近 $x$ 的 $y$ 都可行。取 $y=x-t\nabla f_0(x)$（$t\in\mathbb{R}$ 参数）。对小的正 $t$，$y$ 可行，故

```math
\nabla f_0(x)^T(y-x)=-t\|\nabla f_0(x)\|_2^2\ge0,
```

由此得 $\nabla f_0(x)=0$。

根据方程 (4.22) 解的个数有几种可能的情形。若 (4.22) 无解，则没有最优点，问题最优值未被达到。这里可区分两种情形：问题无下界；或最优值有限但未达到。另一方面方程 (4.22) 可能有多个解，此时每个解都是 $f_0$ 的最小化点。

<span style="color:#00838f;">**[例 4.5]** **无约束二次优化。** 考虑最小化二次函数

```math
f_0(x)=(1/2)x^TPx+q^Tx+r
```

的问题，其中 $P\in\mathbf{S}^n_+$（这使 $f_0$ 凸）。$x$ 是 $f_0$ 最小化点的充要条件为

```math
\nabla f_0(x)=Px+q=0.
```

视这个（线性）方程无解、一解或多解有几种情形：

- 若 $q\notin\mathcal{R}(P)$，则无解。此时 $f_0$ 无下界。
- 若 $P\succ0$（即 $f_0$ 严格凸的条件），则有唯一最小化点 $x^\star=-P^{-1}q$。

<!-- p.155 -->

- 若 $P$ 奇异但 $q\in\mathcal{R}(P)$，则最优集是（仿射）集合 $X_{\mathrm{opt}}=-P^\dagger q+\mathcal{N}(P)$，其中 $P^\dagger$ 表示 $P$ 的 pseudo-inverse（伪逆）（见 §A.5.4）。

<span style="color:#00838f;">**[例 4.6]** **解析中心（analytic centering）。** 考虑（无约束）最小化（凸）函数 $f_0:\mathbb{R}^n\to\mathbb{R}$ 的问题：

```math
f_0(x)=-\sum_{i=1}^m\log(b_i-a_i^Tx),\qquad \mathbf{dom}\,f_0=\{x \mid Ax\prec b\},
```

其中 $a_1^T,\dots,a_m^T$ 是 $A$ 的行。$f_0$ 可微，所以 $x$ 最优的充要条件是

```math
Ax\prec b,\qquad \nabla f_0(x)=\sum_{i=1}^m\frac{1}{b_i-a_i^Tx}a_i=0. \tag{4.23}
```

（条件 $Ax\prec b$ 就是 $x\in\mathbf{dom}\,f_0$。）若 $Ax\prec b$ 不可行，则 $f_0$ 的定义域为空。设 $Ax\prec b$ 可行，仍有几种可能的情形（见习题 4.2）：

- (4.23) 无解，故问题没有最优点。这当且仅当 $f_0$ 无下界时发生。
- (4.23) 有多个解。此时可以证明解构成一个仿射集。
- (4.23) 有唯一解，即 $f_0$ 有唯一最小化点。这当且仅当开多面体 $\{x \mid Ax\prec b\}$ 非空且有界时发生。

<!-- p.155/156 -->

**只有等式约束的问题。** 考虑只有等式约束、没有不等式约束的情形：

```math
\begin{aligned}
\text{minimize}\quad & f_0(x)\\
\text{subject to}\quad & Ax=b.
\end{aligned}
```

此时可行集是仿射的。设它非空（否则问题不可行）。可行 $x$ 的最优性条件是

```math
\nabla f_0(x)^T(y-x)\ge0
```

必须对所有满足 $Ay=b$ 的 $y$ 成立。由于 $x$ 可行，每个可行 $y$ 形如 $y=x+v$（某个 $v\in\mathcal{N}(A)$）。最优性条件因此可表达为：$\nabla f_0(x)^Tv\ge0$ 对所有 $v\in\mathcal{N}(A)$。线性函数在子空间上非负则必在该子空间上为零，所以 $\nabla f_0(x)^Tv=0$ 对所有 $v\in\mathcal{N}(A)$。换言之，$\nabla f_0(x)\perp\mathcal{N}(A)$。

<!-- p.156/157 -->

利用 $\mathcal{N}(A)^\perp=\mathcal{R}(A^T)$，这个最优性条件可表达为 $\nabla f_0(x)\in\mathcal{R}(A^T)$，即存在 $\nu\in\mathbb{R}^p$ 使

```math
\nabla f_0(x)+A^T\nu=0.
```

连同 $Ax=b$（即 $x$ 可行）的要求，这就是经典的 Lagrange multiplier optimality condition（拉格朗日乘子最优性条件），我们将在第 5 章更详细地研究。

**在非负卦限上最小化。** 再举一个例子：问题

```math
\begin{aligned}
\text{minimize}\quad & f_0(x)\\
\text{subject to}\quad & x\succeq0,
\end{aligned}
```

唯一的不等式约束是变量的非负性。最优性条件 (4.21) 为

```math
x\succeq0,\qquad \nabla f_0(x)^T(y-x)\ge0 \text{ 对所有 } y\succeq0.
```

项 $\nabla f_0(x)^Ty$ 是 $y$ 的线性函数，除非 $\nabla f_0(x)\succeq0$，它在 $y\succeq0$ 上无下界。于是条件退化为 $-\nabla f_0(x)^Tx\ge0$。但 $x\succeq0$ 且 $\nabla f_0(x)\succeq0$，故必有 $\nabla f_0(x)^Tx=0$，即

```math
\sum_{i=1}^n(\nabla f_0(x))_ix_i=0.
```

此和中每项都是两个非负数之积，故每项必为零，即 $(\nabla f_0(x))_ix_i=0$（$i=1,\dots,n$）。最优性条件因此可表达为

```math
x\succeq0,\qquad \nabla f_0(x)\succeq0,\qquad x_i(\nabla f_0(x))_i=0,\ i=1,\dots,n.
```

最后一个条件称为 complementarity（互补性），因为它意味着向量 $x$ 与 $\nabla f_0(x)$ 的 sparsity patterns（稀疏模式，即非零分量的下标集合）complementary（互补，交为空）。我们将在第 5 章再次遇到互补性条件。

> <span style="color:#2471a3;">**[译者注｜第一性原理]** §4.2 是全书的枢纽：局部最优 = 全局最优（4.2.2）+ 一阶最优判据 (4.21)（几何上：负梯度是可行集的支撑超平面）。把这两条记牢，第 5 章的 KKT 条件只是它们在拉格朗日函数上的推广。</span>

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">4.2.4 Equivalent convex problems（等价的凸问题）</span>

<!-- p.156/157 -->

看看 §4.1.3 描述的变换中哪些保持凸性是有用的。

**消去等式约束。** 对凸问题，等式约束必须是线性的（形如 $Ax=b$）。此时可通过找出 $Ax=b$ 的一个特解 $x_0$ 与一个张成 $A$ 零空间的矩阵 $F$ 来消去它们，得到问题

```math
\begin{aligned}
\text{minimize}\quad & f_0(Fz+x_0)\\
\text{subject to}\quad & f_i(Fz+x_0)\le0,\quad i=1,\dots,m,
\end{aligned}
```

<!-- p.157 -->

变量为 $z$。由于凸函数与仿射函数的复合是凸的，消去等式约束保持问题的凸性。而且消去等式约束（以及从变换后问题的解重构原问题的解）只涉及标准线性代数运算。

至少在原则上，这意味着我们可以只关注没有等式约束的凸优化问题。然而许多情况下保留等式约束更好：消去它们可能使问题更难理解与分析，或破坏求解算法的效率。当变量 $x$ 维数很大、消去等式约束会破坏稀疏性或其他有用结构时尤其如此。

**引入等式约束。** 可以向凸优化问题引入新变量与新等式约束，只要等式约束是线性的，所得问题也凸。例如若某个目标或约束函数形如 $f_i(A_ix+b_i)$（$A_i\in\mathbb{R}^{k_i\times n}$），可引入新变量 $y_i\in\mathbb{R}^{k_i}$，把 $f_i(A_ix+b_i)$ 换成 $f_i(y_i)$，并添加线性等式约束 $y_i=A_ix+b_i$。

**松弛变量。** 引入松弛变量得到新约束 $f_i(x)+s_i=0$。由于凸问题中等式约束函数必须是仿射的，必须有 $f_i$ 仿射。换言之：为线性不等式引入松弛变量保持问题的凸性。

**上图问题形式。** 凸优化问题 (4.15) 的上图形式是

```math
\begin{aligned}
\text{minimize}\quad & t\\
\text{subject to}\quad & f_0(x)-t\le0\\
& f_i(x)\le0, && i=1,\dots,m\\
& a_i^Tx=b_i, && i=1,\dots,p.
\end{aligned}
```

目标是线性（从而凸）的，新约束函数 $f_0(x)-t$ 关于 $(x,t)$ 也凸，所以上图形式问题也是凸的。

<!-- p.157/158 -->

有时人们说 linear objective（线性目标）对凸优化是 universal（普适）的，因为任何凸优化问题都容易变换为线性目标的问题。凸问题的上图形式有若干实际用途：假设凸优化问题的目标是线性的，可以简化理论分析；也可以简化算法开发，因为一个能求解线性目标凸优化问题的算法，用上述变换就能求解任何凸优化问题（只要它能处理约束 $f_0(x)-t\le0$）。

**对部分变量最小化。** 对部分变量最小化凸函数保持凸性。因此若 (4.9) 中的 $f_0$ 关于 $x_1,x_2$ 联合凸，$f_i$（$i=1,\dots,m_1$）与 $\tilde{f}_i$（$i=1,\dots,m_2$）凸，则等价问题 (4.10) 凸。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">4.2.5 Quasiconvex optimization（拟凸优化）</span>

<!-- p.158 -->

回顾：quasiconvex optimization problem（拟凸优化问题）具有标准形式

```math
\begin{aligned}
\text{minimize}\quad & f_0(x)\\
\text{subject to}\quad & f_i(x)\le0, && i=1,\dots,m\\
& Ax=b,
\end{aligned} \tag{4.24}
```

其中不等式约束函数 $f_1,\dots,f_m$ 凸，目标 $f_0$ 是拟凸的（而非凸优化问题中的凸）。（拟凸约束函数可以替换为等价的凸约束函数，即凸且具有相同 0-下水平集的约束函数，如 §3.4.5。）

本节指出凸与拟凸优化问题的一些基本差异，并展示如何把求解拟凸优化问题约化为求解一列凸优化问题。

**局部最优解与最优性条件。** 凸优化与拟凸优化最重要的差异：拟凸优化问题可以有不是（全局）最优的局部最优解。这一现象甚至在 $\mathbb{R}$ 上拟凸函数无约束最小化的简单情形就能看到（图 4.3）。

尽管如此，§4.2.3 给出的最优性条件 (4.21) 的一个变体对目标可微的拟凸优化问题成立。令 $\mathcal{X}$ 表示拟凸优化问题 (4.24) 的可行集。由拟凸性的一阶条件 (3.20) 得：若

```math
x\in\mathcal{X},\qquad \nabla f_0(x)^T(y-x)>0 \text{ 对所有 } y\in\mathcal{X}\setminus\{x\}, \tag{4.25}
```

<!-- p.159 -->

则 $x$ 最优。

这个判据与凸优化中类似的判据 (4.21) 有两个重要差异：

- 条件 (4.25) 只是最优性的 sufficient（充分）条件；简单例子表明它对最优点未必成立。相比之下，条件 (4.21) 对 $x$ 求解凸问题是必要且充分的。
- 条件 (4.25) 要求 $f_0$ 的梯度非零，而条件 (4.21) 不要求。事实上凸情形下 $\nabla f_0(x)=0$ 时条件 (4.21) 满足且 $x$ 最优。

<!-- p.159/160 -->

**借助凸可行性问题求解拟凸优化。** 求解拟凸优化的一种一般方法依赖 §3.4.5 描述的「用一族凸不等式表示拟凸函数的下水平集」。设 $\phi_t:\mathbb{R}^n\to\mathbb{R}$（$t\in\mathbb{R}$）是满足

```math
f_0(x)\le t \iff \phi_t(x)\le0
```

的凸函数族，且对每个 $x$，$\phi_t(x)$ 是 $t$ 的非增函数（$s\ge t$ 时 $\phi_s(x)\le\phi_t(x)$）。

设 $p^\star$ 表示拟凸优化问题 (4.24) 的最优值。若可行性问题

```math
\begin{aligned}
\text{find}\quad & x\\
\text{subject to}\quad & \phi_t(x)\le0\\
& f_i(x)\le0, && i=1,\dots,m\\
& Ax=b
\end{aligned} \tag{4.26}
```

可行，则 $p^\star\le t$。反之若问题 (4.26) 不可行，则可断定 $p^\star\ge t$。问题 (4.26) 是 convex feasibility problem（凸可行性问题），因为不等式约束函数全凸、等式约束线性。

于是，我们可以通过求解凸可行性问题 (4.26) 来检验拟凸优化问题的最优值 $p^\star$ 是小于还是大于给定值 $t$。若凸可行性问题可行，则 $p^\star\le t$，且任何可行点 $x$ 对拟凸问题可行并满足 $f_0(x)\le t$；若不可行，则 $p^\star\ge t$。

<!-- p.160 -->

这个观察可以作为用 bisection（二分法）求解拟凸优化问题 (4.24) 的简单算法的基础：每步求解一个凸可行性问题。设问题可行，从已知包含最优值 $p^\star$ 的区间 $[l,u]$ 出发。在区间中点 $t=(l+u)/2$ 处求解凸可行性问题，判定最优值在区间的下半还是上半，并相应更新区间。这产生一个仍包含最优值但宽度为初始区间一半的新区间。重复直到区间宽度足够小：

<span style="color:#00838f;">**[算法 4.1]** **拟凸优化的二分法。** 给定 $l\le p^\star$，$u\ge p^\star$，容差 $\epsilon>0$。

重复：
1. $t:=(l+u)/2$。
2. 求解凸可行性问题 (4.26)。
3. 若 (4.26) 可行，则 $u:=t$；否则 $l:=t$。

直到 $u-l\le\epsilon$。

区间 $[l,u]$ 保证包含 $p^\star$，即每步都有 $l\le p^\star\le u$。每次迭代区间被分成两半（bisected），所以 $k$ 次迭代后区间长度为 $2^{-k}(u-l)$（$u-l$ 为初始区间长度）。因此算法终止前恰好需要 $\lceil\log_2((u-l)/\epsilon)\rceil$ 次迭代。每步涉及求解凸可行性问题 (4.26)。
