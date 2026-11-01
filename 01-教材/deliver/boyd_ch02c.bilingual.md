# <span style="color:#c0392b;">Boyd《凸优化》第 2 章 凸集（下：§2.5–2.6 + 文献）</span>

> <span style="color:#7f8c8d;">Source: 教材/Convex Optimization (Boyd, Vandenberghe).pdf · Chapter 2 §2.5–2.6 + Bibliography, pp.46–59 (PDF p.60–73) · 行内双语翻译版 · 译者：ZCode（bilingual-translator 铁律）· 习题部分（PDF p.74–80）暂未翻译</span>

## <span style="color:#c0392b;">2.5 Separating and supporting hyperplanes（分离超平面与支撑超平面）</span>

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">2.5.1 Separating hyperplane theorem（分离超平面定理）</span>

<!-- p.60 -->

本节描述一个后面很重要的想法：用超平面或仿射函数去 separate（分离）不相交的凸集。基本结果是 separating hyperplane theorem（分离超平面定理）：设 $C$ 与 $D$ 是非空、不相交的凸集，即 $C\cap D=\varnothing$。则存在 $a\neq0$ 与 $b$，使得对所有 $x\in C$ 有 $a^Tx\le b$，对所有 $x\in D$ 有 $a^Tx\ge b$。换言之，仿射函数 $a^Tx-b$ 在 $C$ 上非正、在 $D$ 上非负。超平面 $\{x\mid a^Tx=b\}$ 称为集合 $C$ 与 $D$ 的 separating hyperplane（分离超平面），或说它分离 $C$ 与 $D$。（见图 2.19。）

**分离超平面定理的证明。** 这里考虑一个特殊情形，把证明推广到一般情形留作习题（习题 2.22）。我们假设 $C$ 与 $D$ 之间的（欧氏）distance（距离）

```math
\mathbf{dist}(C,D) = \inf\{\|u-v\|_2 \mid u\in C,\; v\in D\}
```

<!-- p.61 -->

为正，且存在点 $c\in C$ 与 $d\in D$ 达到最小距离，即 $\|c-d\|_2=\mathbf{dist}(C,D)$。（例如当 $C$ 与 $D$ 是闭的、且其中一个是有界的时候，这些条件满足。）定义

```math
a = d-c,\qquad b = \frac{\|d\|_2^2-\|c\|_2^2}{2}.
```

我们将证明仿射函数

```math
f(x) = a^Tx-b = (d-c)^T\bigl(x-(1/2)(d+c)\bigr)
```

在 $C$ 上非正、在 $D$ 上非负，即超平面 $\{x\mid a^Tx=b\}$ 分离 $C$ 与 $D$。这个超平面垂直于 $c$ 与 $d$ 之间的线段并穿过其中点（见图 2.20）。

先证 $f$ 在 $D$ 上非负（$f$ 在 $C$ 上非正的证明类似，或通过交换 $C$ 与 $D$ 并考虑 $-f$ 得到）。反设存在点 $u\in D$ 使

```math
f(u) = (d-c)^T\bigl(u-(1/2)(d+c)\bigr) < 0. \tag{2.16}
```

<!-- p.62 -->

把 $f(u)$ 写成

```math
f(u) = (d-c)^T\bigl(u-d+(1/2)(d-c)\bigr) = (d-c)^T(u-d) + (1/2)\|d-c\|_2^2,
```

可见 (2.16) 蕴含 $(d-c)^T(u-d)<0$。现在注意

```math
\left.\frac{d}{dt}\|d+t(u-d)-c\|_2^2\right|_{t=0} = 2(d-c)^T(u-d) < 0,
```

所以对某个小的 $t>0$（$t\le1$），有

```math
\|d+t(u-d)-c\|_2 < \|d-c\|_2,
```

即点 $d+t(u-d)$ 比 $d$ 更靠近 $c$。但 $D$ 是凸的且包含 $d$ 与 $u$，故 $d+t(u-d)\in D$。这与「$d$ 是 $D$ 中离 $C$ 最近的点」的假设矛盾。

<span style="color:#00838f;">**[例 2.19]** **仿射集与凸集的分离。** 设 $C$ 凸，$D$ 仿射，即 $D=\{Fu+g \mid u\in\mathbb{R}^m\}$（$F\in\mathbb{R}^{n\times m}$）。设 $C$ 与 $D$ 不相交，则由分离超平面定理，存在 $a\neq0$ 与 $b$ 使 $a^Tx\le b$（$x\in C$）、$a^Tx\ge b$（$x\in D$）。而 $a^Tx\ge b$ 对一切 $x\in D$ 成立，意味着 $a^TFu\ge b-a^Tg$ 对一切 $u\in\mathbb{R}^m$ 成立。线性函数在 $\mathbb{R}^m$ 上有下界仅当它恒为零，故 $a^TF=0$（从而 $b\le a^Tg$）。于是我们得出：存在 $a\neq0$ 使 $F^Ta=0$ 且 $a^Tx\le a^Tg$ 对一切 $x\in C$ 成立。

<!-- p.63 -->

**严格分离（Strict separation）。** 我们构造的分离超平面满足更强的条件：$a^Tx\lt b$ 对所有 $x\in C$，且 $a^Tx>b$ 对所有 $x\in D$。这称为 $C$ 与 $D$ 的 strict separation（严格分离）。简单的例子表明：一般而言，不相交的凸集未必能被超平面严格分离（即使两个集合都是闭的；见习题 2.23）。但在许多特殊情形下可以建立严格分离。

<span style="color:#00838f;">**[例 2.20]** **一个点与一个闭凸集的严格分离。** 设 $C$ 是闭凸集，$x_0\notin C$。则存在严格分离 $x_0$ 与 $C$ 的超平面。为看出这一点，注意对某个 $\epsilon>0$，集合 $C$ 与 $B(x_0,\epsilon)$ 不相交。由分离超平面定理，存在 $a\neq0$ 与 $b$ 使 $a^Tx\le b$（$x\in C$）、$a^Tx\ge b$（$x\in B(x_0,\epsilon)$）。利用 $B(x_0,\epsilon)=\{x_0+u \mid \|u\|_2\le\epsilon\}$，第二个条件可写成 $a^T(x_0+u)\ge b$ 对所有 $\|u\|_2\le\epsilon$ 成立。使左边最小的 $u$ 是 $u=-\epsilon a/\|a\|_2$；代入得 $a^Tx_0-\epsilon\|a\|_2\ge b$。因此仿射函数

```math
f(x) = a^Tx-b-\epsilon\|a\|_2/2
```

在 $C$ 上为负，在 $x_0$ 处为正。

由此立即得到前面提到过的一个事实：闭凸集是所有包含它的半空间的交。设 $C$ 闭且凸，$S$ 为所有包含 $C$ 的半空间之交集。显然 $x\in C \implies x\in S$。反之，设存在 $x\in S$、$x\notin C$。由严格分离结果，存在严格分离 $x$ 与 $C$ 的超平面，即存在一个包含 $C$ 但不含 $x$ 的半空间；换言之 $x\notin S$，矛盾。

**逆分离超平面定理（Converse separating hyperplane theorems）。** 分离超平面定理的逆命题（「存在分离超平面」蕴含「$C$ 与 $D$ 不相交」）一般不真，除非对 $C$ 或 $D$ 附加超出凸性的额外条件。一个简单的反例：$C=D=\{0\}\subseteq\mathbb{R}$，此时超平面 $x=0$ 分离 $C$ 与 $D$。

<!-- p.64 -->

通过给 $C$、$D$ 加条件，可以导出各种逆分离定理。一个非常简单的例子：设 $C$ 与 $D$ 是凸集，$C$ 是 open（开）的，且存在仿射函数 $f$ 在 $C$ 上非正、在 $D$ 上非负，则 $C$ 与 $D$ 不相交。（为看出这一点：先注意 $f$ 在 $C$ 上必为负——若 $f$ 在 $C$ 的某点为零，则 $f$ 会在该点附近取正值，矛盾；而 $f$ 在 $C$ 上为负、在 $D$ 上非负，所以 $C$ 与 $D$ 必不相交。）把这个逆命题与分离超平面定理合起来，得到如下结果：任何两个凸集 $C$ 与 $D$（至少一个是开的）不相交，当且仅当存在分离超平面。

<span style="color:#00838f;">**[例 2.21]** **严格线性不等式的择一定理（theorem of alternatives）。** 我们推导严格线性不等式组

```math
Ax \prec b \tag{2.17}
```

可解的充要条件。这组不等式 infeasible（不可行）当且仅当（凸）集合

```math
C = \{b-Ax \mid x\in\mathbb{R}^n\},\qquad D = \mathbb{R}^m_{++} = \{y\in\mathbb{R}^m \mid y\succ0\}
```

不相交。集合 $D$ 是开的；$C$ 是仿射集。于是由上面的结果，$C$ 与 $D$ 不相交当且仅当存在分离超平面，即非零 $\lambda\in\mathbb{R}^m$ 与 $\mu\in\mathbb{R}$ 使 $\lambda^Ty\le\mu$ 在 $C$ 上、$\lambda^Ty\ge\mu$ 在 $D$ 上成立。

<!-- p.64/65 -->

每个条件都可化简。第一个意味着 $\lambda^T(b-Ax)\le\mu$ 对所有 $x$ 成立，正如例 2.19 那样可推出 $A^T\lambda=0$ 与 $\lambda^Tb\le\mu$。第二个不等式意味着 $\lambda^Ty\ge\mu$ 对所有 $y\succ0$ 成立，这蕴含 $\mu\le0$ 且 $\lambda\succeq0$、$\lambda\neq0$。

<!-- p.65 -->

合起来：严格不等式组 (2.17) 不可行，当且仅当存在 $\lambda\in\mathbb{R}^m$ 使

```math
\lambda\neq0,\qquad \lambda\succeq0,\qquad A^T\lambda=0,\qquad \lambda^Tb\le0. \tag{2.18}
```

这也是关于变量 $\lambda\in\mathbb{R}^m$ 的一组线性不等式与线性方程。我们说 (2.17) 与 (2.18) 构成一对 alternatives（择一）：对任意数据 $A$ 与 $b$，两者中恰好一个可解。

> <span style="color:#2471a3;">**[译者注｜跨课程连接]** 择一定理在结构上就是 CSAPP/算法课里「证书与对偶」的思想：给不出原系统的解，就给出「解不存在」的证明证书 $\lambda$。第 5 章的 Lagrangian 对偶正是这个思想的完整版。</span>

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">2.5.2 Supporting hyperplanes（支撑超平面）</span>

<!-- p.65 -->

设 $C\subseteq\mathbb{R}^n$，$x_0$ 是其边界 $\mathbf{bd}\,C$ 上的点，即

```math
x_0\in\mathbf{bd}\,C = \mathbf{cl}\,C \setminus \mathbf{int}\,C.
```

若 $a\neq0$ 满足 $a^Tx\le a^Tx_0$ 对所有 $x\in C$，则超平面 $\{x \mid a^Tx=a^Tx_0\}$ 称为 $C$ 在点 $x_0$ 处的 supporting hyperplane（支撑超平面）。这等价于说：点 $x_0$ 与集合 $C$ 被超平面 $\{x\mid a^Tx=a^Tx_0\}$ 分离。几何解释是：超平面 $\{x\mid a^Tx=a^Tx_0\}$ 在 $x_0$ 处与 $C$ 相切（tangent），且半空间 $\{x\mid a^Tx\le a^Tx_0\}$ 包含 $C$。（见图 2.21。）

一个基本结果称为 supporting hyperplane theorem（支撑超平面定理）：对任何非空凸集 $C$ 与任何 $x_0\in\mathbf{bd}\,C$，存在 $C$ 在 $x_0$ 处的支撑超平面。支撑超平面定理很容易从分离超平面定理推出。分两种情形：若 $C$ 的内部非空，对集合 $\{x_0\}$ 与 $\mathbf{int}\,C$ 应用分离超平面定理即可；若 $C$ 的内部为空，则 $C$ 必位于维数小于 $n$ 的仿射集中，任何包含该仿射集的超平面都包含 $C$ 与 $x_0$，因而是（平凡的）支撑超平面。

支撑超平面定理也有一个部分逆命题：若一个集合是闭的、内部非空、且在其边界的每一点都有支撑超平面，则它是凸的。（见习题 2.27。）

## <span style="color:#c0392b;">2.6 Dual cones and generalized inequalities（对偶锥与广义不等式）</span>

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">2.6.1 Dual cones（对偶锥）</span>

<!-- p.65/66 -->

设 $K$ 是锥。集合

```math
K^* = \{y \mid x^Ty\ge0 \text{ 对所有 } x\in K\} \tag{2.19}
```

称为 $K$ 的 dual cone（对偶锥）。顾名思义，$K^*$ 是锥，且总是凸的——即使原来的锥 $K$ 不是（见习题 2.31）。

几何上，$y\in K^*$ 当且仅当 $-y$ 是一个在原点支撑 $K$ 的超平面的法向量。（见图 2.22。）

<span style="color:#00838f;">**[例 2.22]** **子空间。** 子空间 $V\subseteq\mathbb{R}^n$（它是锥）的对偶锥是其正交补 $V^\perp=\{y \mid v^Ty=0 \text{ 对所有 } v\in V\}$。

<span style="color:#00838f;">**[例 2.23]** **非负卦限。** 锥 $\mathbb{R}^n_+$ 是它自己的对偶：

```math
x^Ty\ge0 \text{ 对所有 } x\succeq0 \iff y\succeq0.
```

这样的锥称为 self-dual（自对偶）的。

<span style="color:#00838f;">**[例 2.24]** **半正定锥。** 在对称 $n\times n$ 矩阵集合 $\mathbf{S}^n$ 上使用标准内积 $\mathbf{tr}(XY)=\sum_{i,j=1}^n X_{ij}Y_{ij}$（见 §A.1.1），半正定锥 $\mathbf{S}^n_+$ 是自对偶的，即对 $X,Y\in\mathbf{S}^n$：

```math
\mathbf{tr}(XY)\ge0 \text{ 对所有 } X\succeq0 \iff Y\succeq0.
```

我们来确立这个事实。设 $Y\notin\mathbf{S}^n_+$，则存在 $q\in\mathbb{R}^n$ 使

```math
q^TYq = \mathbf{tr}(qq^TY) < 0.
```

于是半正定矩阵 $X=qq^T$ 满足 $\mathbf{tr}(XY)<0$；故 $Y\notin(\mathbf{S}^n_+)^*$。反之设 $X,Y\in\mathbf{S}^n_+$。把 $X$ 按特征值分解写成 $X=\sum_{i=1}^n\lambda_iq_iq_i^T$（特征值 $\lambda_i\ge0$，$i=1,\dots,n$），则

```math
\mathbf{tr}(YX) = \mathbf{tr}\Bigl(Y\sum_{i=1}^n\lambda_iq_iq_i^T\Bigr) = \sum_{i=1}^n\lambda_iq_i^TYq_i\ge0.
```

这表明 $Y\in(\mathbf{S}^n_+)^*$。

<span style="color:#00838f;">**[例 2.25]** **范数锥的对偶。** 设 $\|\cdot\|$ 是 $\mathbb{R}^n$ 上的范数。相应锥 $K=\{(x,t)\in\mathbb{R}^{n+1} \mid \|x\|\le t\}$ 的对偶是由 dual norm（对偶范数）定义的锥：

```math
K^* = \{(u,v)\in\mathbb{R}^{n+1} \mid \|u\|_*\le v\},
```

<!-- p.67 -->

其中对偶范数由 $\|u\|_*=\sup\{u^Tx \mid \|x\|\le1\}$ 给出（见 (A.1.6)）。

为证明该结果，须证

```math
x^Tu+tv\ge0 \text{ 只要 } \|x\|\le t \iff \|u\|_*\le v. \tag{2.20}
```

先证右边的条件蕴含左边的条件。设 $\|u\|_*\le v$，且对某个 $t>0$ 有 $\|x\|\le t$（若 $t=0$，则 $x$ 必为零，显然 $u^Tx+vt\ge0$）。应用对偶范数的定义与 $\|-x/t\|\le1$，得

```math
u^T(-x/t)\le\|u\|_*\le v,
```

因此 $u^Tx+vt\ge0$。再证左边的条件蕴含右边的条件：设 $\|u\|_*>v$（即右边条件不成立），则由对偶范数的定义，存在 $x$ 满足 $\|x\|\le1$ 且 $x^Tu>v$。取 $t=1$，得

```math
u^T(-x)+v<0,
```

与 (2.20) 左边的条件矛盾。

<!-- p.67/68 -->

对偶锥满足若干性质，例如：

- $K^*$ 是闭的且凸的。
- $K_1\subseteq K_2$ 蕴含 $K_2^*\subseteq K_1^*$。
- 若 $K$ 有非空内部，则 $K^*$ 是尖的（pointed）。
- 若 $K$ 的闭包是尖的，则 $K^*$ 有非空内部。
- $K^{**}$ 是 $K$ 的凸包的闭包。（因此若 $K$ 凸且闭，则 $K^{**}=K$。）

（见习题 2.31。）这些性质表明：若 $K$ 是真锥，则其对偶 $K^*$ 也是真锥，而且 $K^{**}=K$。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">2.6.2 Dual generalized inequalities（对偶广义不等式）</span>

<!-- p.68 -->

现在设凸锥 $K$ 是 proper（真）的，因此它诱导广义不等式 $\preceq_K$。其对偶锥 $K^*$ 也是真的，因此也诱导一个广义不等式。我们把广义不等式 $\preceq_{K^*}$ 称为广义不等式 $\preceq_K$ 的 dual（对偶）。

联系广义不等式与其对偶的一些重要性质：

- $x\preceq_K y$ 当且仅当 $\lambda^Tx\le\lambda^Ty$ 对所有 $\lambda\succeq_{K^*}0$ 成立。
- $x\prec_K y$ 当且仅当 $\lambda^Tx<\lambda^Ty$ 对所有 $\lambda\succeq_{K^*}0$、$\lambda\neq0$ 成立。

由于 $K=K^{**}$，与 $\preceq_{K^*}$ 相关联的对偶广义不等式是 $\preceq_K$，所以把广义不等式与其对偶互换后这些性质同样成立。具体例子：$\lambda\preceq_{K^*}\mu$ 当且仅当 $\lambda^Tx\le\mu^Tx$ 对所有 $x\succeq_K0$ 成立。

<span style="color:#00838f;">**[例 2.26]** **线性严格广义不等式的择一定理。** 设 $K\subseteq\mathbb{R}^m$ 是真锥。考虑严格广义不等式

```math
Ax\prec_K b, \tag{2.21}
```

其中 $x\in\mathbb{R}^n$。我们来推导这个不等式的择一定理。设它不可行，即仿射集 $\{b-Ax \mid x\in\mathbb{R}^n\}$ 与开凸集 $\mathbf{int}\,K$ 不相交。则存在分离超平面：非零 $\lambda\in\mathbb{R}^m$ 与 $\mu\in\mathbb{R}$ 使 $\lambda^T(b-Ax)\le\mu$（对所有 $x$）、$\lambda^Ty\ge\mu$（对所有 $y\in\mathbf{int}\,K$）。第一个条件蕴含 $A^T\lambda=0$ 与 $\lambda^Tb\le\mu$；第二个条件蕴含 $\lambda^Ty\ge\mu$ 对所有 $y\in K$ 成立，这只在 $\lambda\in K^*$ 且 $\mu\le0$ 时可能。

合起来：若 (2.21) 不可行，则存在 $\lambda$ 使

```math
\lambda\neq0,\qquad \lambda\succeq_{K^*}0,\qquad A^T\lambda=0,\qquad \lambda^Tb\le0. \tag{2.22}
```

<!-- p.68/69 -->

现在证逆命题：若 (2.22) 成立，则不等式组 (2.21) 不可行。反设两个不等式组同时成立。则 $\lambda^T(b-Ax)>0$，因为 $\lambda\neq0$、$\lambda\succeq_{K^*}0$、$b-Ax\succ_K0$。但用 $A^T\lambda=0$ 得 $\lambda^T(b-Ax)=\lambda^Tb\le0$，矛盾。

因此不等式组 (2.21) 与 (2.22) 是 alternatives（择一的）：对任意数据 $A,b$，恰好一个可行。（这推广了 $K=\mathbb{R}^m_+$ 的特例 (2.17)、(2.18)。）

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">2.6.3 Minimum and minimal elements via dual inequalities（借助对偶不等式刻画最小元与极小元）</span>

<!-- p.69 -->

我们可以用对偶广义不等式来刻画（可能非凸的）集合 $S\subseteq\mathbb{R}^m$ 关于真锥 $K$ 诱导的广义不等式的最小元与极小元。

**最小元的对偶刻画（Dual characterization of minimum element）。** 先考虑最小元的刻画：$x$ 是 $S$ 的最小元（关于 $\preceq_K$），当且仅当对所有 $\lambda\succ_{K^*}0$，$x$ 是 $\lambda^Tz$ 在 $z\in S$ 上的 unique minimizer（唯一最小化点）。几何上这意味着：对任何 $\lambda\succ_{K^*}0$，超平面

```math
\{z \mid \lambda^T(z-x)=0\}
```

是 $S$ 在 $x$ 处的 strict supporting hyperplane（严格支撑超平面）。（严格支撑超平面指该超平面与 $S$ 只交于点 $x$。）注意不需要集合 $S$ 的凸性。（见图 2.23。）

<!-- p.69/70 -->

为证明此结果，设 $x$ 是 $S$ 的最小元（即 $x\preceq_K z$ 对所有 $z\in S$），且 $\lambda\succ_{K^*}0$。设 $z\in S$、$z\neq x$。由于 $x$ 是最小元，有 $z-x\succeq_K0$。由 $\lambda\succ_{K^*}0$、$z-x\succeq_K0$、$z-x\neq0$ 得 $\lambda^T(z-x)>0$。由于 $z$ 是 $S$ 中任意不等于 $x$ 的元素，这表明 $x$ 是 $\lambda^Tz$ 在 $S$ 上的唯一最小化点。反之，设对所有 $\lambda\succ_{K^*}0$，$x$ 是 $\lambda^Tz$ 在 $S$ 上的唯一最小化点，但 $x$ 不是 $S$ 的最小元。则存在 $z\in S$ 使 $z\not\succeq_K x$。由于 $z-x\not\succeq_K0$，存在 $\tilde{\lambda}\succeq_{K^*}0$ 使 $\tilde{\lambda}^T(z-x)<0$。于是在 $\tilde{\lambda}$ 的邻域内有 $\lambda^T(z-x)<0$（$\lambda\succ_{K^*}0$），与「$x$ 是 $\lambda^Tz$ 在 $S$ 上的唯一最小化点」矛盾。

**极小元的对偶刻画（Dual characterization of minimal elements）。** 再看极小元的类似刻画。这里必要条件与充分条件之间有一个 gap（间隙）。若 $\lambda\succ_{K^*}0$ 且 $x$ 最小化 $\lambda^Tz$（$z\in S$），则 $x$ 是极小的。（见图 2.24。）

<!-- p.70/71 -->

为证明：设 $\lambda\succ_{K^*}0$，$x$ 最小化 $\lambda^Tz$（$z\in S$），但 $x$ 不是极小的，即存在 $z\in S$、$z\neq x$、$z\preceq_Kx$。则 $\lambda^T(x-z)>0$，与「$x$ 是最小化点」的假设矛盾。

逆命题一般不成立：点 $x$ 可以是 $S$ 的极小元，却对任何 $\lambda$ 都不是 $\lambda^Tz$（$z\in S$）的最小化点，如图 2.25 所示。此图提示凸性在逆命题中起重要作用，事实确实如此：只要集合 $S$ 凸，就可以说——对任何极小元 $x$，存在非零的 $\lambda\succeq_{K^*}0$ 使 $x$ 最小化 $\lambda^Tz$（$z\in S$）。

为证明：设 $x$ 极小，即 $((x-K)\setminus\{x\})\cap S=\varnothing$。对凸集 $(x-K)\setminus\{x\}$ 与 $S$ 应用分离超平面定理，得存在 $\lambda\neq0$ 与 $\mu$ 使 $\lambda^T(x-y)\le\mu$（对所有 $y\in K$）、$\lambda^Tz\ge\mu$（对所有 $z\in S$）。由第一个不等式得 $\lambda\succeq_{K^*}0$。由 $x\in S$ 且 $x\in x-K$ 得 $\lambda^Tx=\mu$，所以第二个不等式表明 $\mu$ 是 $\lambda^Tz$ 在 $S$ 上的最小值。因此 $x$ 是 $\lambda^Tz$ 在 $S$ 上的最小化点，其中 $\lambda\neq0$、$\lambda\succeq_{K^*}0$。

<!-- p.71 -->

这个逆定理不能加强为 $\lambda\succ_{K^*}0$：例子表明点 $x$ 可以是凸集 $S$ 的极小点，却对任何 $\lambda\succ_{K^*}0$ 都不是最小化点（见图 2.26 左）。同样，「$\lambda\succeq_{K^*}0$ 时 $\lambda^Tz$ 在 $S$ 上的最小化点必是极小的」也不成立（见图 2.26 右）。

<span style="color:#00838f;">**[例 2.27]** **Pareto 最优生产前沿。** 考虑一个需要 $n$ 种 resources（资源）（如劳动力、电力、天然气、水）来制造的产品。该产品可以按许多方式生产。对每种 production method（生产方式），关联一个 resource vector（资源向量）$x\in\mathbb{R}^n$，其中 $x_i$ 表示该方法制造产品所消耗的第 $i$ 种资源的量。我们假设 $x_i\ge0$（即资源被生产方式消耗）且资源是有价值的（所以任何资源用得越少越好）。production set（生产集）$P\subseteq\mathbb{R}^n$ 定义为对应某种生产方式的所有资源向量的集合。

资源向量为 $P$ 的极小元（关于逐分量不等式）的生产方式称为 Pareto optimal（Pareto 最优）或 efficient（有效率的）。$P$ 的极小元集合称为 efficient production frontier（有效生产前沿）。

<!-- p.71/72 -->

Pareto 最优有一个简单解释。我们说一种生产方式（资源向量 $x$）比另一种（资源向量 $y$）better（更好），如果 $x_i\le y_i$ 对所有 $i$，且对某个 $i$ 有 $x_i\lt y_i$。换言之，一种生产方式比另一种好，是指它每种资源用量都不多于对方，且至少一种资源确实用得更少。这正对应 $x\preceq y$、$x\neq y$。于是可以说：一种生产方式是 Pareto 最优的（有效率的），当且仅当没有比它更好的生产方式。

我们可以通过在 $P$ 上最小化

```math
\lambda^Tx = \lambda_1x_1+\cdots+\lambda_nx_n
```

来寻找 Pareto 最优的生产方式（即极小资源向量），其中 $\lambda\succ0$ 任意。这里向量 $\lambda$ 有一个简单解释：$\lambda_i$ 是第 $i$ 种资源的 price（价格）。在 $P$ 上最小化 $\lambda^Tx$，就是在找（按价格 $\lambda_i$ 计的）总成本最低的生产方式。只要价格全为正，得到的生产方式就保证是有效率的。这些想法见图 2.27。

<!-- p.72/73 -->

（图 2.27 说明：产品需要劳动力与燃料来生产，生产集 $P$ 如阴影所示。两条深色曲线是有效生产前沿。点 $x_1,x_2,x_3$ 是有效率的；$x_4$ 与 $x_5$ 不是（例如 $x_2$ 对应的生产方式燃料不多于、劳动力少于 $x_5$）。点 $x_1$ 也是价格向量 $\lambda$（正）下成本最小的生产方式。点 $x_2$ 是有效率的，但对任何价格向量 $\lambda\succeq0$，都无法通过最小化总成本 $\lambda^Tx$ 找到它。）

> <span style="color:#2471a3;">**[译者注｜第一性原理]** 例 2.27 把 §2.4.2 的抽象概念（最小元/极小元）翻译成了经济学语言：Pareto 最优 = 极小元；加权价格 $\lambda$ 扫描 = 对偶刻画。「正价格能扫到一部分有效点、扫不到全部」正是本节 gap 的现实版本——这是多目标优化与第 5 章对偶间隙的预告。</span>

## <span style="color:#c0392b;">Bibliography（文献注释）</span>

<!-- p.73 -->

Minkowski（闵可夫斯基）通常被认为是对凸集进行首次系统研究的人：他引入了支撑超平面与支撑超平面定理、Minkowski distance function（闵可夫斯基距离函数）（习题 3.34）、凸集的 extreme points（极点）等基本概念，以及其他许多内容。

一些著名的早期综述有 Bonnesen 与 Fenchel [BF48]、Eggleston [Egg58]、Klee [Kle63]、Valentine [Val64]。较新的专讲凸集几何的书有 Lay [Lay82] 与 Webster [Web94]。Klee [Kle71]、Fenchel [Fen83]、Tikhomorov [Tik90]、Berger [Ber90] 对凸性及其在整个数学中的应用历史给出了非常易读的概述。

线性不等式与多面体集合在与 linear programming problem（线性规划问题）的联系中被广泛研究（参考文献见第 4 章末）。线性不等式与线性规划历史上的一些里程碑文献有 Motzkin [Mot33]、von Neumann 与 Morgenstern [vNM53]、Kantorovich [Kan60]、Koopmans [Koo51]、Dantzig [Dan63]。Dantzig [Dan63, Chapter 2] 包含截至 1963 年左右线性不等式的历史综述。

广义不等式在 1960 年代被引入非线性优化（见 Luenberger [Lue69, §8.2] 与 Isii [Isi64]），并在 cone programming（锥规划）中被大量使用（参考文献见第 4 章）。Bellman 与 Fan [BF63] 是关于（半正定锥的）广义线性不等式集合的早期论文。

分离超平面定理的推广与证明见 Rockafellar [Roc70, part III] 与 Hiriart-Urruty 与 Lemaréchal [HUL93, volume 1, §III4]。Dantzig [Dan63, page 21] 把「theorem of the alternative（择一定理）」这一术语归于 von Neumann 与 Morgenstern [vNM53, page 138]。更多择一定理的参考见第 5 章。

例 2.27 的术语（包括 Pareto 最优、有效生产、$\lambda$ 的价格解释）在 Luenberger [Lue95] 中有详细讨论。

凸几何在 classical theory of moments（经典矩理论）中扮演重要角色（Krein 与 Nudelman [KN77]，Karlin 与 Studden [KS66]）。一个著名例子是非负多项式锥与幂矩锥之间的对偶；见习题 2.37。

---

> <span style="color:#7f8c8d;">§2.1–2.6 正文与文献注释翻译完毕。习题（PDF p.74–80）暂未翻译。下一章：boyd_ch03（Convex functions，凸函数，PDF p.81–140）。</span>
