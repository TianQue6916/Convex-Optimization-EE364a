> <span style="color:#7f8c8d;">Source: SEE官方讲义/05Duality.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

## <span style="color:#c0392b;">5. Duality（对偶性）</span>

<!-- p.1 -->

<span style="color:#2471a3;">**[lecture]**</span>

### <span style="color:#c0392b;">本讲提纲</span>

本讲是全课程的理论核心，围绕对偶性展开：

- Lagrange dual problem（拉格朗日对偶问题）
- weak and strong duality（弱对偶性与强对偶性）
- geometric interpretation（几何解释）
- optimality conditions（最优性条件）
- perturbation and sensitivity analysis（扰动与灵敏度分析）
- examples（例题）
- generalized inequalities（广义不等式）

<!-- p.2 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">Lagrangian（拉格朗日函数）</span>

<span style="color:#00838f;">**标准形式问题**（standard form problem，不要求凸性）：</span>

```math
\begin{array}{ll}
\text{minimize} & f_0(x)\\
\text{subject to} & f_i(x)\le 0,\quad i=1,\dots,m\\
& h_i(x)=0,\quad i=1,\dots,p
\end{array}
```

变量 $x\in\mathbb{R}^n$，定义域（domain）$D$，最优值（optimal value）$p^\star$。

<span style="color:#2471a3;">**[definition]**</span>

<span style="color:#00838f;">**定义**：拉格朗日函数 $L:\mathbb{R}^n\times\mathbb{R}^m\times\mathbb{R}^p\to\mathbb{R}$，定义域 $\operatorname{dom}L=D\times\mathbb{R}^m\times\mathbb{R}^p$，</span>

```math
L(x,\lambda,\nu)=f_0(x)+\sum_{i=1}^{m}\lambda_i f_i(x)+\sum_{i=1}^{p}\nu_i h_i(x)
```

- 它是目标函数与各约束函数的加权和（weighted sum）；
- $\lambda_i$ 是与不等式约束 $f_i(x)\le 0$ 相关联的 Lagrange multiplier（拉格朗日乘子）；
- $\nu_i$ 是与等式约束 $h_i(x)=0$ 相关联的拉格朗日乘子。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 从第一性原理看，拉格朗日函数做的事情是把"违反约束"明码标价后并入目标函数：当 $f_i(x)>0$ 违反不等式时，$\lambda_if_i(x)$ 形成正的罚项；约束满足时这一项不起作用。所以 $\lambda$、$\nu$ 本质上是"价格"——它们衡量约束被收紧或放松一单位时最优值会变化多少，本讲第 22–23 页的灵敏度分析会把这句话变成精确的不等式与导数公式。你在 Strang 线性代数里已经见过它的雏形：条件极值中的拉格朗日乘子法 $\nabla f=\lambda\nabla g$，本质是"目标梯度与约束梯度平行"的一阶条件，只是当时只有等式约束；这里推广到 $m$ 个不等式加 $p$ 个等式约束，而且暂时不要求问题凸。关键转变在于视角：把一个约束优化问题嵌入一族关于 $(x,\lambda,\nu)$ 的无约束极小化问题，本讲的对偶函数、对偶问题直到 KKT 条件，全部从这一个"加权"定义里生长出来。

<!-- p.3 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">Lagrange dual function（拉格朗日对偶函数）</span>

<span style="color:#2471a3;">**[definition]**</span>

<span style="color:#00838f;">**定义**：拉格朗日对偶函数 $g:\mathbb{R}^m\times\mathbb{R}^p\to\mathbb{R}$，</span>

```math
g(\lambda,\nu)=\inf_{x\in D}L(x,\lambda,\nu)=\inf_{x\in D}\Bigl(f_0(x)+\sum_{i=1}^{m}\lambda_i f_i(x)+\sum_{i=1}^{p}\nu_i h_i(x)\Bigr)
```

- $g$ 是凹函数（concave），对某些 $(\lambda,\nu)$ 可取 $-\infty$（原因：$L$ 关于 $(\lambda,\nu)$ 仿射，故 $g$ 是一族仿射函数的逐点下确界）；
- <span style="color:#00838f;">**下界性质**（lower bound property）：若 $\lambda\succeq 0$，则 $g(\lambda,\nu)\le p^\star$。</span>

**证明**：若 $\tilde{x}$ 可行且 $\lambda\succeq 0$，则

```math
f_0(\tilde{x})\ \ge\ L(\tilde{x},\lambda,\nu)\ \ge\ \inf_{x\in D}L(x,\lambda,\nu)=g(\lambda,\nu)
```

对所有可行 $\tilde{x}$ 取极小值即得 $p^\star\ge g(\lambda,\nu)$。$\blacksquare$

<!-- p.4 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">例：线性方程组的最小范数解（least-norm solution）</span>

<span style="color:#2471a3;">**[example]**</span>

```math
\begin{array}{ll}
\text{minimize} & x^Tx\\
\text{subject to} & Ax=b
\end{array}
```

**对偶函数**的推导：

- 拉格朗日函数为 $L(x,\nu)=x^Tx+\nu^T(Ax-b)$；
- 要在 $x$ 上极小化 $L$，令梯度为零：

```math
\nabla_x L(x,\nu)=2x+A^T\nu=0\ \Longrightarrow\ x=-(1/2)A^T\nu
```

- 回代到 $L$ 得到 $g$：

```math
g(\nu)=L\bigl(-(1/2)A^T\nu,\ \nu\bigr)=-\frac14\nu^TAA^T\nu-b^T\nu
```

它是 $\nu$ 的凹函数。

下界性质：对一切 $\nu$ 都有 $p^\star\ge-\frac14\nu^TAA^T\nu-b^T\nu$。

<!-- p.5 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">例：标准形式线性规划（standard form LP）</span>

<span style="color:#2471a3;">**[example]**</span>

```math
\begin{array}{ll}
\text{minimize} & c^Tx\\
\text{subject to} & Ax=b,\quad x\succeq 0
\end{array}
```

对偶函数：

- 拉格朗日函数为

```math
L(x,\lambda,\nu)=c^Tx+\nu^T(Ax-b)-\lambda^Tx=-b^T\nu+(c+A^T\nu-\lambda)^Tx
```

- $L$ 关于 $x$ 是仿射的（affine），因此

```math
g(\lambda,\nu)=\inf_x L(x,\lambda,\nu)=
\begin{cases}
-b^T\nu & A^T\nu-\lambda+c=0\\
-\infty & \text{否则}
\end{cases}
```

$g$ 在仿射集 $\{(\lambda,\nu)\mid A^T\nu-\lambda+c=0\}$ 上是线性的，故为凹函数。

下界性质：若 $A^T\nu+c\succeq 0$，则 $p^\star\ge -b^T\nu$。

<!-- p.6 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">例：等式约束范数极小化（equality constrained norm minimization）</span>

<span style="color:#2471a3;">**[example]**</span>

```math
\begin{array}{ll}
\text{minimize} & \|x\|\\
\text{subject to} & Ax=b
\end{array}
```

对偶函数：

```math
g(\nu)=\inf_x\bigl(\|x\|-\nu^TAx+b^T\nu\bigr)=
\begin{cases}
b^T\nu & \|A^T\nu\|_*\le 1\\
-\infty & \text{否则}
\end{cases}
```

其中 $\|v\|_*=\sup_{\|u\|\le 1}u^Tv$ 是 $\|\cdot\|$ 的 dual norm（对偶范数）。

**证明**：利用 $\inf_x(\|x\|-y^Tx)=0$（当 $\|y\|_*\le 1$）、$=-\infty$（否则）：

- 若 $\|y\|_*\le 1$，则对一切 $x$ 有 $\|x\|-y^Tx\ge 0$，且 $x=0$ 时取等号；
- 若 $\|y\|_*>1$，取 $x=tu$，其中 $\|u\|\le 1$、$u^Ty=\|y\|_*>1$，则

```math
\|x\|-y^Tx=t(\|u\|-\|y\|_*)\ \longrightarrow\ -\infty\quad(t\to\infty)
```

下界性质：若 $\|A^T\nu\|_*\le 1$，则 $p^\star\ge b^T\nu$。

<!-- p.7 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">例：双向划分问题（two-way partitioning，非凸）</span>

<span style="color:#2471a3;">**[example]**</span>

```math
\begin{array}{ll}
\text{minimize} & x^TWx\\
\text{subject to} & x_i^2=1,\quad i=1,\dots,n
\end{array}
```

- 这是一个非凸问题；可行集包含 $2^n$ 个离散点；
- 解读：把 $\{1,\dots,n\}$ 划分成两个集合；$W_{ij}$ 是把 $i,j$ 分到同一集合的代价，$-W_{ij}$ 是分到不同集合的代价。

对偶函数：

```math
g(\nu)=\inf_x\Bigl(x^TWx+\sum_i \nu_i\,(x_i^2-1)\Bigr)=\inf_x\;x^T\bigl(W+\operatorname{diag}(\nu)\bigr)x-\mathbf{1}^T\nu
=
\begin{cases}
-\mathbf{1}^T\nu & W+\operatorname{diag}(\nu)\succeq 0\\
-\infty & \text{否则}
\end{cases}
```

下界性质：若 $W+\operatorname{diag}(\nu)\succeq 0$，则 $p^\star\ge -\mathbf{1}^T\nu$。

例：取 $\nu=-\lambda_{\min}(W)\mathbf{1}$，得下界 $p^\star\ge n\lambda_{\min}(W)$。

<!-- p.8 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">Lagrange 对偶与共轭函数（conjugate function）</span>

```math
\begin{array}{ll}
\text{minimize} & f_0(x)\\
\text{subject to} & Ax\preceq b,\quad Cx=d
\end{array}
```

对偶函数：

```math
\begin{aligned}
g(\lambda,\nu)&=\inf_{x\in\operatorname{dom}f_0}\Bigl(f_0(x)+(A^T\lambda+C^T\nu)^Tx-b^T\lambda-d^T\nu\Bigr)\\
&=-f_0^*(-A^T\lambda-C^T\nu)-b^T\lambda-d^T\nu
\end{aligned}
```

- 回顾 conjugate function（共轭函数）的定义：$f^*(y)=\sup_{x\in\operatorname{dom}f}\,(y^Tx-f(x))$；
- 若 $f_0$ 的共轭已知，可大幅简化对偶函数的推导。

例：熵极大化（entropy maximization）问题中

```math
f_0(x)=\sum_{i=1}^{n}x_i\log x_i,\qquad f_0^*(y)=\sum_{i=1}^{n}\bigl(e^{y_i}-1\bigr)
```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 共轭函数 $f^*(y)=\sup_{x}(y^Tx-f(x))$ 是凸分析的中心构件：它是勒让德变换（Legendre 变换，力学与热力学中在共轭变量对如能量/温度之间切换的标准工具）向非光滑凸函数的推广，由 Fenchel 在 1949 年前后系统化，故又称 Fenchel 共轭。本页的意义在于把"推导对偶函数"这个最费力的步骤变成"查共轭表"：$g$ 恰好等于负共轭在 $-A^T\lambda-C^T\nu$ 处的取值再减去常数项。这也解释了对偶理论为何在凸分析语言下格外整洁——对偶函数 $g$ 是凹的、对偶问题是凸的，根源都是"共轭函数自动是凸的"这一代数事实，与原问题凸不凸无关。以后遇到 $\log$、范数、二次型这类常见目标函数，先查共轭表（教材 3.3 节）往往比现场求下确界快得多。

<!-- p.9 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">The dual problem（对偶问题）</span>

<span style="color:#2471a3;">**[definition]**</span>

<span style="color:#00838f;">**定义（Lagrange dual problem，拉格朗日对偶问题）**：</span>

```math
\begin{array}{ll}
\text{maximize} & g(\lambda,\nu)\\
\text{subject to} & \lambda\succeq 0
\end{array}
```

- 它在对偶函数给出的下界中寻找最好的（即最大的）一个，最优值记作 $d^\star$；
- 这是一个凸优化问题（无论原问题是否凸）；
- 若 $\lambda\succeq 0$ 且 $(\lambda,\nu)\in\operatorname{dom}g$，则称 $(\lambda,\nu)$ 为 dual feasible（对偶可行）；
- 实践中常把隐式约束 $(\lambda,\nu)\in\operatorname{dom}g$ 显式写出，以简化问题。

例：标准形式 LP 及其对偶（第 5–5 页）：

```math
\begin{array}{ll}
\text{minimize} & c^Tx\\
\text{subject to} & Ax=b\\
& x\succeq 0
\end{array}
\qquad\qquad
\begin{array}{ll}
\text{maximize} & -b^T\nu\\
\text{subject to} & A^T\nu+c\succeq 0
\end{array}
```

<!-- p.10 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">Weak and strong duality（弱对偶性与强对偶性）</span>

<span style="color:#2471a3;">**[theorem]**</span>

<span style="color:#00838f;">**弱对偶性（weak duality）：$d^\star\le p^\star$。**</span>

- 恒成立（无论问题凸或不凸）；
- 可用来给困难问题寻找非平凡下界。例如求解半定规划（SDP）

```math
\begin{array}{ll}
\text{maximize} & -\mathbf{1}^T\nu\\
\text{subject to} & W+\operatorname{diag}(\nu)\succeq 0
\end{array}
```

即给出第 5–7 页双向划分问题最优值的下界。

<span style="color:#00838f;">**强对偶性（strong duality）：$d^\star= p^\star$。**</span>

- 一般并不成立；
- 对凸问题（通常）成立；
- 保证凸问题强对偶性的条件称为 constraint qualification（约束规范）。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 弱对偶性最漂亮的应用就演示在本页：第 5–7 页的双向划分问题要在 $2^n$ 个离散点上极小化 $x^TWx$，它等价于 max-cut 一类的组合难题，是 NP-hard 的——用 6.006 的话说，除非 P=NP，不存在多项式时间算法求精确解。对偶理论把"求精确解"替换成"证下界"：解一个多项式时间可解的半定规划，立刻得到 $p^\star\ge n\lambda_{\min}(W)$ 这样可计算、可验证的下界。这是处理 NP-hard 优化问题的标准范式——先构造多项式时间可解的松弛（relaxation），松弛的对偶给出原问题的界，界还能喂给分支定界等精确算法做剪枝。注意方向：极小化问题的下界来自对偶可行点而非原问题可行点；对偶可行性只需检查一个半正定性与线性等式，而原问题可行点要在 $2^n$ 个候选里枚举，几乎不可行。至于下界何时能收紧成等号（强对偶性），正是下一页 Slater 条件要回答的问题。

<!-- p.11 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">Slater's constraint qualification（Slater 约束规范）</span>

<span style="color:#2471a3;">**[theorem]**</span>

<span style="color:#00838f;">**定理**：对凸问题</span>

```math
\begin{array}{ll}
\text{minimize} & f_0(x)\\
\text{subject to} & f_i(x)\le 0,\quad i=1,\dots,m\\
& Ax=b
\end{array}
```

若它是 strictly feasible（严格可行）的，即

```math
\exists\, x\in\operatorname{int}D:\quad f_i(x)<0,\ \ i=1,\dots,m,\qquad Ax=b
```

则强对偶性成立。

- Slater 条件还保证对偶最优值可以达到（attained，当 $p^\star>-\infty$ 时）；
- 该条件可以再放宽：例如可将 $\operatorname{int}D$ 换成 $\operatorname{relint}D$（相对仿射包的内部）；线性不等式不必严格成立；等等；
- 此外还存在许多其他类型的约束规范。

<!-- p.12 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">例：不等式形式 LP（inequality form LP）</span>

<span style="color:#2471a3;">**[example]**</span>

原问题（primal problem）：

```math
\begin{array}{ll}
\text{minimize} & c^Tx\\
\text{subject to} & Ax\preceq b
\end{array}
```

对偶函数：

```math
g(\lambda)=\inf_x\Bigl((c+A^T\lambda)^Tx-b^T\lambda\Bigr)=
\begin{cases}
-b^T\lambda & A^T\lambda+c=0\\
-\infty & \text{否则}
\end{cases}
```

对偶问题：

```math
\begin{array}{ll}
\text{maximize} & -b^T\lambda\\
\text{subject to} & A^T\lambda+c=0,\quad \lambda\succeq 0
\end{array}
```

- 由 Slater 条件：若存在 $\tilde{x}$ 使 $A\tilde{x}\prec b$，则 $p^\star=d^\star$；
- 事实上，除非原问题与对偶问题都不可行，总有 $p^\star=d^\star$。

<!-- p.13 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">例：二次规划（quadratic program, QP）</span>

<span style="color:#2471a3;">**[example]**</span>

原问题（设 $P\in\mathbf{S}_{++}^n$，即正定）：

```math
\begin{array}{ll}
\text{minimize} & x^TPx\\
\text{subject to} & Ax\preceq b
\end{array}
```

对偶函数：

```math
g(\lambda)=\inf_x\Bigl(x^TPx+\lambda^T(Ax-b)\Bigr)=-\frac14\lambda^TAP^{-1}A^T\lambda-b^T\lambda
```

对偶问题：

```math
\begin{array}{ll}
\text{maximize} & -(1/4)\lambda^TAP^{-1}A^T\lambda-b^T\lambda\\
\text{subject to} & \lambda\succeq 0
\end{array}
```

- 由 Slater 条件：若存在 $\tilde{x}$ 使 $A\tilde{x}\prec b$，则 $p^\star=d^\star$；
- 事实上，$p^\star=d^\star$ 恒成立。

<!-- p.14 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">例：一个具有强对偶性的非凸问题</span>

<span style="color:#2471a3;">**[example]**</span>

```math
\begin{array}{ll}
\text{minimize} & x^TAx+2b^Tx\\
\text{subject to} & x^Tx\le 1
\end{array}
```

由于 $A\not\succeq 0$，问题非凸。

对偶函数：$g(\lambda)=\inf_x\bigl(x^T(A+\lambda I)x+2b^Tx-\lambda\bigr)$

- 若 $A+\lambda I\not\succeq 0$，或 $A+\lambda I\succeq 0$ 但 $b\notin\mathcal{R}(A+\lambda I)$，则其无下界；
- 否则在 $x=-(A+\lambda I)^{\dagger}b$ 处取极小：$g(\lambda)=-b^T(A+\lambda I)^{\dagger}b-\lambda$。

对偶问题及其等价 SDP：

```math
\begin{array}{ll}
\text{maximize} & -b^T(A+\lambda I)^{\dagger}b-\lambda\\
\text{subject to} & A+\lambda I\succeq 0\\
& b\in\mathcal{R}(A+\lambda I)
\end{array}
\qquad\qquad
\begin{array}{ll}
\text{maximize} & -t-\lambda\\
\text{subject to} & \begin{bmatrix}A+\lambda I & b\\ b^T & t\end{bmatrix}\succeq 0
\end{array}
```

尽管原问题非凸，强对偶性依然成立（这一点并不容易证明）。

<!-- p.15 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">Geometric interpretation（几何解释）</span>

为简单起见，考虑只带一个约束 $f_1(x)\le 0$ 的问题。

对偶函数的几何解读：

```math
g(\lambda)=\inf_{(u,t)\in G}\,(t+\lambda u),\qquad G=\{(f_1(x),\,f_0(x))\mid x\in D\}
```

（图示：$(u,t)$ 平面上标出点集 $G$、支撑线 $\lambda u+t=g(\lambda)$ 及其在 $t$ 轴上的截距 $g(\lambda)$；第二幅图另标出 $p^\star$ 与 $d^\star$ 的相对位置——对偶最优值作为支撑线截距的上确界，位于 $p^\star$ 下方。）

- $\lambda u+t=g(\lambda)$ 是 $G$ 的（非竖直）supporting hyperplane（支撑超平面）；
- 该超平面与 $t$ 轴相交于 $t=g(\lambda)$。

<!-- p.16 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">上图变体（epigraph variation）与强对偶性</span>

把 $G$ 换成如下上图式集合 $A$ 后，同样的几何解读成立：

```math
A=\{(u,t)\mid f_1(x)\le u,\ f_0(x)\le t\ \text{对某个}\ x\in D\}
```

（图示：集合 $A$、点 $(0,p^\star)$、支撑线 $\lambda u+t=g(\lambda)$ 及截距 $g(\lambda)$。）

**强对偶性**：

- 若在 $(0,p^\star)$ 处存在 $A$ 的非竖直支撑超平面，则强对偶性成立；
- 对凸问题，$A$ 是凸集，故在 $(0,p^\star)$ 处必有支撑超平面；
- <span style="color:#00838f;">Slater 条件的几何版本：若存在 $(\tilde{u},\tilde{t})\in A$ 使 $\tilde{u}<0$，则 $(0,p^\star)$ 处的支撑超平面必非竖直。</span>

<!-- p.17 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">Complementary slackness（互补松弛）</span>

<span style="color:#2471a3;">**[theorem]**</span>

设强对偶性成立，$x^\star$ 为原问题最优解，$(\lambda^\star,\nu^\star)$ 为对偶最优解，则 $f_0(x^\star)=g(\lambda^\star,\nu^\star)$，且

```math
\begin{aligned}
f_0(x^\star)=g(\lambda^\star,\nu^\star)
&=\inf_x\Bigl(f_0(x)+\sum_{i=1}^m\lambda_i^\star f_i(x)+\sum_{i=1}^p\nu_i^\star h_i(x)\Bigr)\\
&\le f_0(x^\star)+\sum_{i=1}^m\lambda_i^\star f_i(x^\star)+\sum_{i=1}^p\nu_i^\star h_i(x^\star)
\ \le\ f_0(x^\star)
\end{aligned}
```

因此，上式两处不等号都必须取等号：

- $x^\star$ 极小化 $L(x,\lambda^\star,\nu^\star)$；
- 对 $i=1,\dots,m$ 有 $\lambda_i^\star f_i(x^\star)=0$——这就是 complementary slackness（互补松弛）：

```math
\lambda_i^\star>0\ \Longrightarrow\ f_i(x^\star)=0,\qquad
f_i(x^\star)<0\ \Longrightarrow\ \lambda_i^\star=0
```

<!-- p.18 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">Karush-Kuhn-Tucker（KKT）conditions（KKT 条件）</span>

<span style="color:#2471a3;">**[theorem]**</span>

对 $f_i$、$h_i$ 可微的问题，下列四个条件统称为 KKT conditions（KKT 条件）：

1. **原始约束**（primal constraints）：$f_i(x)\le 0,\ i=1,\dots,m$；$h_i(x)=0,\ i=1,\dots,p$；
2. **对偶约束**（dual constraints）：$\lambda\succeq 0$；
3. **互补松弛**（complementary slackness）：$\lambda_i f_i(x)=0,\ i=1,\dots,m$；
4. **拉格朗日函数对 $x$ 的梯度为零**：

```math
\nabla f_0(x)+\sum_{i=1}^m\lambda_i\nabla f_i(x)+\sum_{i=1}^p\nu_i\nabla h_i(x)=0
```

由第 5–17 页的论证：若强对偶性成立，且 $x,\lambda,\nu$ 分别为原、对偶最优解，则它们必满足 KKT 条件。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 理解 KKT 条件最省力的方式：它是"无约束时令梯度为零"在约束世界里的完整推广。第 4 条平稳性条件说的是 $\nabla f_0(x)$ 可以写成约束梯度 $\nabla f_i$、$\nabla h_i$ 的线性组合——当约束不活跃（$f_i(x)<0$）时，互补松弛强制 $\lambda_i=0$，条件就退化成你熟悉的 $\nabla f_0(x)=0$；当解贴在约束边界上时，乘子恰好补上梯度缺掉的分量。历史上，Karush 在 1939 年的硕士论文里已给出这套条件，Kuhn 与 Tucker 于 1951 年重新发现并推广后才广为流传，故以三人姓氏命名。对一般问题，KKT 条件在最优点处只是"必要"的；真正的奇迹在下一页：凸问题加 Slater 条件下它变成充要条件——"验证一个点是否最优"从全局搜索塌缩为"解一个方程组加不等式组"。这正是连续优化里"最优性证书可被高效验证"的形态，与 6.006 中 NP 问题候选解可多项式时间验证的思想遥相呼应。

<!-- p.19 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">凸问题的 KKT 条件</span>

若 $\tilde{x},\tilde{\lambda},\tilde{\nu}$ 满足某个凸问题的 KKT 条件，则它们分别最优（原、对偶最优）：

- 由互补松弛：$f_0(\tilde{x})=L(\tilde{x},\tilde{\lambda},\tilde{\nu})$；
- 由第 4 条（结合凸性）：$g(\tilde{\lambda},\tilde{\nu})=L(\tilde{x},\tilde{\lambda},\tilde{\nu})$；

因此 $f_0(\tilde{x})=g(\tilde{\lambda},\tilde{\nu})$。

<span style="color:#00838f;">**若 Slater 条件满足：$x$ 最优，当且仅当存在 $\lambda,\nu$ 使 $x,\lambda,\nu$ 一起满足 KKT 条件。**</span>

- 回顾：Slater 条件蕴含强对偶性，且对偶最优值可以达到；
- 它推广了无约束问题的最优性条件 $\nabla f_0(x)=0$。

<!-- p.20 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">例：注水法（water-filling，设 $\alpha_i>0$）</span>

<span style="color:#2471a3;">**[example]**</span>

```math
\begin{array}{ll}
\text{minimize} & -\sum_{i=1}^n\log(x_i+\alpha_i)\\
\text{subject to} & x\succeq 0,\quad \mathbf{1}^Tx=1
\end{array}
```

$x$ 最优，当且仅当 $x\succeq 0$、$\mathbf{1}^Tx=1$，且存在 $\lambda\in\mathbb{R}^n$、$\nu\in\mathbb{R}$ 使

```math
\lambda\succeq 0,\qquad \lambda_i x_i=0,\qquad \frac{1}{x_i+\alpha_i}+\lambda_i=\nu\quad(i=1,\dots,n)
```

分情形求解：

- 若 $\nu<1/\alpha_i$：则 $\lambda_i=0$，$x_i=1/\nu-\alpha_i$；
- 若 $\nu\ge 1/\alpha_i$：则 $\lambda_i=\nu-1/\alpha_i$，$x_i=0$；
- $\nu$ 由下式确定：$\mathbf{1}^Tx=\sum_{i=1}^n\max\{0,\ 1/\nu-\alpha_i\}=1$。

**解读**：

- 有 $n$ 块地面，第 $i$ 块地面的高度为 $\alpha_i$；
- 用单位量的水去淹灌；
- 最终水面高度为 $1/\nu^\star$。

（图示：各块地面高度 $\alpha_i$、水深 $x_i$ 与统一水位线 $1/\nu^\star$——低于水位的地面被注入深 $x_i$ 的水，高于水位的地面保持干燥。）

<!-- p.21 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">Perturbation and sensitivity analysis（扰动与灵敏度分析）</span>

（未扰动的）优化问题及其对偶：

```math
\begin{array}{ll}
\text{minimize} & f_0(x)\\
\text{subject to} & f_i(x)\le 0,\ i=1,\dots,m\\
& h_i(x)= 0,\ i=1,\dots,p
\end{array}
\qquad\qquad
\begin{array}{ll}
\text{maximize} & g(\lambda,\nu)\\
\text{subject to} & \lambda\succeq 0
\end{array}
```

扰动后的问题及其对偶：

```math
\begin{array}{ll}
\text{minimize} & f_0(x)\\
\text{subject to} & f_i(x)\le u_i,\ i=1,\dots,m\\
& h_i(x)=v_i,\ i=1,\dots,p
\end{array}
\qquad\qquad
\begin{array}{ll}
\text{maximize} & g(\lambda,\nu)-u^T\lambda-v^T\nu\\
\text{subject to} & \lambda\succeq 0
\end{array}
```

- $x$ 是原始变量；$u,v$ 是参数；
- $p^\star(u,v)$ 表示作为 $u,v$ 的函数的最优值；
- 我们关心的是：从未扰动问题及其对偶的解出发，能获得关于 $p^\star(u,v)$ 的哪些信息。

<!-- p.22 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">Global sensitivity result（全局灵敏度结果）</span>

<span style="color:#2471a3;">**[theorem]**</span>

设未扰动问题满足强对偶性，且 $\lambda^\star,\nu^\star$ 是未扰动问题的对偶最优解。

把弱对偶性应用于扰动后的问题：

```math
p^\star(u,v)\ \ge\ g(\lambda^\star,\nu^\star)-u^T\lambda^\star-v^T\nu^\star\ =\ p^\star(0,0)-u^T\lambda^\star-v^T\nu^\star
```

**灵敏度解读**：

- 若 $\lambda_i^\star$ 大：收紧第 $i$ 个约束（$u_i<0$）会使 $p^\star$ 大幅上升；
- 若 $\lambda_i^\star$ 小：放松第 $i$ 个约束（$u_i>0$）也不会使 $p^\star$ 下降太多；
- 若 $\nu_i^\star$ 大且为正：取 $v_i<0$ 会使 $p^\star$ 大幅上升；若 $\nu_i^\star$ 大且为负：取 $v_i>0$ 会使 $p^\star$ 大幅上升；
- 若 $\nu_i^\star$ 小且为正：取 $v_i>0$ 不会使 $p^\star$ 下降太多；若 $\nu_i^\star$ 小且为负：取 $v_i<0$ 不会使 $p^\star$ 下降太多。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 本页揭示乘子的"经济学身份"：$\lambda_i^\star$ 是第 $i$ 个约束的影子价格（shadow price），即最优值对约束右端扰动的敏感度。全局不等式 $p^\star(u,v)\ge p^\star(0,0)-u^T\lambda^\star-v^T\nu^\star$ 对一切扰动方向成立，不需要任何可微性假设；若 $p^\star$ 在原点可微，下一页会进一步给出精确公式 $\lambda_i^\star=-\partial p^\star/\partial u_i$。这与 6.006 里 LP 对偶的经济学解读完全同源：网络流、最短路等问题的对偶变量对应"每单位资源的边际价值"，也对应资源分配与市场出清模型中的均衡价格。回看上一页扰动问题的对偶目标里多出的 $-u^T\lambda-v^T\nu$ 两项，含义正是"放宽约束要向乘子支付租金"。工程上做功率分配、资源分配时，这套结果让你不必重新求解就能回答"预算再多一成能改善多少"这类问题，是凸优化实用价值最直接的体现之一。

<!-- p.23 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">局部灵敏度（local sensitivity）</span>

<span style="color:#2471a3;">**[theorem]**</span>

若（进一步）$p^\star(u,v)$ 在 $(0,0)$ 处可微，则

```math
\lambda_i^\star=-\frac{\partial p^\star(0,0)}{\partial u_i},\qquad
\nu_i^\star=-\frac{\partial p^\star(0,0)}{\partial v_i}
```

**证明**（以 $\lambda_i^\star$ 为例）：由全局灵敏度结果，

```math
\frac{\partial p^\star(0,0)}{\partial u_i}=\lim_{t\searrow 0}\frac{p^\star(te_i,0)-p^\star(0,0)}{t}\ \ge\ -\lambda_i^\star,
\qquad
\frac{\partial p^\star(0,0)}{\partial u_i}=\lim_{t\nearrow 0}\frac{p^\star(te_i,0)-p^\star(0,0)}{t}\ \le\ -\lambda_i^\star
```

故左右极限相等，等号成立。

（图示：单不等式约束问题的 $p^\star(u)$ 曲线位于直线 $p^\star(0)-\lambda^\star u$ 之上、并在 $u=0$ 处相切——乘子正是这条支撑线的斜率。）

<!-- p.24 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">Duality and problem reformulations（对偶性与问题改写）</span>

- 同一个问题的等价表述，可能导出差别极大的对偶；
- 当对偶函数难以推导、或对偶本身索然无味时，改写原问题会很有用。

常见改写手段：

- 引入新变量与等式约束；
- 把显式约束改为隐式约束（或反过来）；
- 变换目标函数或约束函数，例如用凸且递增的 $\varphi$ 把 $f_0(x)$ 替换为 $\varphi(f_0(x))$。

<!-- p.25 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">改写一：引入新变量与等式约束</span>

考虑问题：minimize $f_0(Ax+b)$。

- 其对偶函数是常数：$g=\inf_x L(x)=\inf_x f_0(Ax+b)=p^\star$；
- 强对偶性成立（$d^\star=p^\star$），但这个对偶毫无用处。

改写后的问题及其对偶：

```math
\begin{array}{ll}
\text{minimize} & f_0(y)\\
\text{subject to} & Ax+b-y=0
\end{array}
\qquad\qquad
\begin{array}{ll}
\text{maximize} & b^T\nu-f_0^*(\nu)\\
\text{subject to} & A^T\nu=0
\end{array}
```

对偶函数由下式得出：

```math
g(\nu)=\inf_{x,y}\bigl(f_0(y)-\nu^Ty+\nu^TAx+b^T\nu\bigr)=
\begin{cases}
-f_0^*(\nu)+b^T\nu & A^T\nu=0\\
-\infty & \text{否则}
\end{cases}
```

<!-- p.26 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">例：范数逼近问题（norm approximation）minimize $\|Ax-b\|$</span>

<span style="color:#2471a3;">**[example]**</span>

改写为引入新变量 $y$：

```math
\begin{array}{ll}
\text{minimize} & \|y\|\\
\text{subject to} & y=Ax-b
\end{array}
```

可以查 $\|\cdot\|$ 的共轭，也可以直接推导对偶函数：

```math
\begin{aligned}
g(\nu)&=\inf_{x,y}\bigl(\|y\|+\nu^Ty-\nu^TAx+b^T\nu\bigr)\\
&=
\begin{cases}
b^T\nu+\inf_y\bigl(\|y\|+\nu^Ty\bigr) & A^T\nu=0\\
-\infty & \text{否则}
\end{cases}
=
\begin{cases}
b^T\nu & A^T\nu=0,\ \ \|\nu\|_*\le 1\\
-\infty & \text{否则}
\end{cases}
\end{aligned}
```

（参见本讲第 5–4 页。）

范数逼近问题的对偶：

```math
\begin{array}{ll}
\text{maximize} & b^T\nu\\
\text{subject to} & A^T\nu=0,\quad \|\nu\|_*\le 1
\end{array}
```

<!-- p.27 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">显式约束与隐式约束（implicit constraints）</span>

带盒约束（box constraints）的 LP——原问题与对偶问题：

```math
\begin{array}{ll}
\text{minimize} & c^Tx\\
\text{subject to} & Ax=b\\
& -1\preceq x\preceq 1
\end{array}
\qquad\qquad
\begin{array}{ll}
\text{maximize} & -b^T\nu-\mathbf{1}^T\lambda_1-\mathbf{1}^T\lambda_2\\
\text{subject to} & c+A^T\nu+\lambda_1-\lambda_2=0\\
& \lambda_1\succeq 0,\quad \lambda_2\succeq 0
\end{array}
```

把盒约束变为隐式（吸收进目标函数）的改写：

```math
\begin{array}{ll}
\text{minimize} & f_0(x)=
\begin{cases}
c^Tx & -1\preceq x\preceq 1\\
+\infty & \text{否则}
\end{cases}\\[6pt]
\text{subject to} & Ax=b
\end{array}
```

对偶函数：

```math
g(\nu)=\inf_{-1\preceq x\preceq 1}\bigl(c^Tx+\nu^T(Ax-b)\bigr)=-b^T\nu-\|A^T\nu+c\|_1
```

对偶问题：maximize $-b^T\nu-\|A^T\nu+c\|_1$（除 $\nu$ 自由外无其他约束）。

<!-- p.28 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">Problems with generalized inequalities（带广义不等式的问题）</span>

<span style="color:#2471a3;">**[definition]**</span>

```math
\begin{array}{ll}
\text{minimize} & f_0(x)\\
\text{subject to} & f_i(x)\preceq_{K_i} 0,\quad i=1,\dots,m\\
& h_i(x)=0,\quad i=1,\dots,p
\end{array}
```

其中 $\preceq_{K_i}$ 是 $\mathbb{R}^{k_i}$ 上的 generalized inequality（广义不等式）。

各定义与标量情形完全平行：

- $f_i(x)\preceq_{K_i}0$ 的拉格朗日乘子是向量 $\lambda_i\in\mathbb{R}^{k_i}$；
- 拉格朗日函数 $L:\mathbb{R}^n\times\mathbb{R}^{k_1}\times\cdots\times\mathbb{R}^{k_m}\times\mathbb{R}^p\to\mathbb{R}$ 定义为

```math
L(x,\lambda_1,\dots,\lambda_m,\nu)=f_0(x)+\sum_{i=1}^m\lambda_i^Tf_i(x)+\sum_{i=1}^p\nu_ih_i(x)
```

- 对偶函数 $g:\mathbb{R}^{k_1}\times\cdots\times\mathbb{R}^{k_m}\times\mathbb{R}^p\to\mathbb{R}$ 定义为

```math
g(\lambda_1,\dots,\lambda_m,\nu)=\inf_{x\in D}\,L(x,\lambda_1,\dots,\lambda_m,\nu)
```

<!-- p.29 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">广义不等式情形：下界性质与对偶问题</span>

<span style="color:#00838f;">**下界性质**：若 $\lambda_i\succeq_{K_i^*}0$（$K_i^*$ 为 $K_i$ 的 dual cone，对偶锥），则 $g(\lambda_1,\dots,\lambda_m,\nu)\le p^\star$。</span>

**证明**：若 $\tilde{x}$ 可行且 $\lambda_i\succeq_{K_i^*}0$，则

```math
\begin{aligned}
f_0(\tilde{x})&\ \ge\ f_0(\tilde{x})+\sum_{i=1}^m\lambda_i^Tf_i(\tilde{x})+\sum_{i=1}^p\nu_ih_i(\tilde{x})\\
&\ \ge\ \inf_{x\in D}L(x,\lambda_1,\dots,\lambda_m,\nu)\ =\ g(\lambda_1,\dots,\lambda_m,\nu)
\end{aligned}
```

对所有可行 $\tilde{x}$ 取极小值即得 $p^\star\ge g(\lambda_1,\dots,\lambda_m,\nu)$。

对偶问题：

```math
\begin{array}{ll}
\text{maximize} & g(\lambda_1,\dots,\lambda_m,\nu)\\
\text{subject to} & \lambda_i\succeq_{K_i^*}0,\quad i=1,\dots,m
\end{array}
```

- 弱对偶性：$p^\star\ge d^\star$ 恒成立；
- 强对偶性：凸问题在约束规范成立时 $p^\star=d^\star$（例如 Slater 条件：原问题严格可行）。

<!-- p.30 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">例：半定规划（semidefinite program, SDP）</span>

<span style="color:#2471a3;">**[example]**</span>

原始 SDP（$F_i,\,G\in\mathbf{S}^k$）：

```math
\begin{array}{ll}
\text{minimize} & c^Tx\\
\text{subject to} & x_1F_1+\cdots+x_nF_n\preceq G
\end{array}
```

- 拉格朗日乘子是矩阵 $Z\in\mathbf{S}^k$；
- 拉格朗日函数 $L(x,Z)=c^Tx+\operatorname{tr}\bigl(Z(x_1F_1+\cdots+x_nF_n-G)\bigr)$；
- 对偶函数

```math
g(Z)=\inf_x L(x,Z)=
\begin{cases}
-\operatorname{tr}(GZ) & \operatorname{tr}(F_iZ)+c_i=0,\ \ i=1,\dots,n\\
-\infty & \text{否则}
\end{cases}
```

对偶 SDP：

```math
\begin{array}{ll}
\text{maximize} & -\operatorname{tr}(GZ)\\
\text{subject to} & Z\succeq 0,\quad \operatorname{tr}(F_iZ)+c_i=0,\ \ i=1,\dots,n
\end{array}
```

若原始 SDP 严格可行（存在 $x$ 使 $x_1F_1+\cdots+x_nF_n\prec G$），则 $p^\star=d^\star$。
