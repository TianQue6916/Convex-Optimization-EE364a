# <span style="color:#c0392b;">Boyd《凸优化》第 5 章 对偶（上：§5.1–5.4）</span>

> <span style="color:#7f8c8d;">Source: 教材/Convex Optimization (Boyd, Vandenberghe).pdf · Chapter 5 §5.1–5.4, pp.215–240 (PDF p.229–254) · 行内双语翻译版 · 译者：ZCode（bilingual-translator 铁律）</span>

## <span style="color:#c0392b;">5.1 The Lagrange dual function（拉格朗日对偶函数）</span>

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.1.1 The Lagrangian（拉格朗日函数）</span>

<!-- p.229 -->

考虑标准形式 (4.1) 的优化问题：

$$
\begin{aligned}
\text{minimize}\quad & f_0(x)\\
\text{subject to}\quad & f_i(x)\le0,\quad i=1,\dots,m\\
& h_i(x)=0,\quad i=1,\dots,p,
\end{aligned} \tag{5.1}
$$

变量 $x\in\mathbb{R}^n$。我们假设其定义域 $\mathcal{D}=\bigcap_{i=0}^m\mathbf{dom}\,f_i\cap\bigcap_{i=1}^p\mathbf{dom}\,h_i$ 非空，记 (5.1) 的最优值为 $p^\star$。我们不假设问题 (5.1) 凸。

Lagrangian duality（拉格朗日对偶）的基本想法是：把约束以约束函数的加权和加到目标函数上，从而把 (5.1) 的约束纳入考虑。我们定义与问题 (5.1) 相关联的 Lagrangian（拉格朗日函数）$L:\mathbb{R}^n\times\mathbb{R}^m\times\mathbb{R}^p\to\mathbb{R}$：

$$
L(x,\lambda,\nu)=f_0(x)+\sum_{i=1}^m\lambda_if_i(x)+\sum_{i=1}^p\nu_ih_i(x),
$$

定义域 $\mathbf{dom}\,L=\mathcal{D}\times\mathbb{R}^m\times\mathbb{R}^p$。我们把 $\lambda_i$ 称为与第 $i$ 个不等式约束 $f_i(x)\le0$ 相关联的 Lagrange multiplier（拉格朗日乘子）；类似地 $\nu_i$ 称为与第 $i$ 个等式约束 $h_i(x)=0$ 相关联的拉格朗日乘子。向量 $\lambda$ 与 $\nu$ 称为与问题 (5.1) 相关联的 dual variables（对偶变量）或 Lagrange multiplier vectors（拉格朗日乘子向量）。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.1.2 The Lagrange dual function（拉格朗日对偶函数）</span>

<!-- p.230 -->

我们定义 Lagrange dual function（拉格朗日对偶函数，或简称 dual function，对偶函数）$g:\mathbb{R}^m\times\mathbb{R}^p\to\mathbb{R}$ 为拉格朗日函数关于 $x$ 的最小值：对 $\lambda\in\mathbb{R}^m$、$\nu\in\mathbb{R}^p$，

$$
g(\lambda,\nu)=\inf_{x\in\mathcal{D}}L(x,\lambda,\nu)=\inf_{x\in\mathcal{D}}\Bigl(f_0(x)+\sum_{i=1}^m\lambda_if_i(x)+\sum_{i=1}^p\nu_ih_i(x)\Bigr).
$$

当拉格朗日函数关于 $x$ 无下界时，对偶函数取值 $-\infty$。由于对偶函数是 $(\lambda,\nu)$ 的一族仿射函数的逐点下确界，它是 concave（凹）的——即使问题 (5.1) 不凸。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.1.3 Lower bounds on optimal value（最优值的下界）</span>

对偶函数给出问题 (5.1) 最优值 $p^\star$ 的下界：对任何 $\lambda\succeq0$ 与任何 $\nu$，

$$
g(\lambda,\nu)\le p^\star. \tag{5.2}
$$

这个重要性质容易验证。设 $\tilde{x}$ 是问题 (5.1) 的可行点（即 $f_i(\tilde{x})\le0$、$h_i(\tilde{x})=0$），且 $\lambda\succeq0$。则

$$
\sum_{i=1}^m\lambda_if_i(\tilde{x})+\sum_{i=1}^p\nu_ih_i(\tilde{x})\le0,
$$

因为第一个和中每项非正、第二个和中每项为零，于是

$$
L(\tilde{x},\lambda,\nu)=f_0(\tilde{x})+\sum_{i=1}^m\lambda_if_i(\tilde{x})+\sum_{i=1}^p\nu_ih_i(\tilde{x})\le f_0(\tilde{x}).
$$

因此

$$
g(\lambda,\nu)=\inf_{x\in\mathcal{D}}L(x,\lambda,\nu)\le L(\tilde{x},\lambda,\nu)\le f_0(\tilde{x}).
$$

由于 $g(\lambda,\nu)\le f_0(\tilde{x})$ 对每个可行点 $\tilde{x}$ 成立，不等式 (5.2) 得证。

<!-- p.230/231 -->

下界 (5.2) 见图 5.1（一个 $x\in\mathbb{R}$、单不等式约束的简单问题）。当 $g(\lambda,\nu)=-\infty$ 时，不等式 (5.2) 成立但 vacuous（空洞）。只有当 $\lambda\succeq0$ 且 $(\lambda,\nu)\in\mathbf{dom}\,g$（即 $g(\lambda,\nu)>-\infty$）时，对偶函数才给出 $p^\star$ 的 nontrivial lower bound（非平凡下界）。我们把满足 $\lambda\succeq0$ 且 $(\lambda,\nu)\in\mathbf{dom}\,g$ 的数对 $(\lambda,\nu)$ 称为 dual feasible（对偶可行）的——原因后面会明白。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.1.4 Linear approximation interpretation（线性近似解释）</span>

<!-- p.232 -->

拉格朗日函数与下界性质可以基于集合 $\{0\}$ 与 $-\mathbb{R}_+$ 的示性函数的 linear approximation（线性近似）给出简单解释。

先把原问题 (5.1) 重写为无约束问题：

$$
\text{minimize}\quad f_0(x)+\sum_{i=1}^mI_-(f_i(x))+\sum_{i=1}^pI_0(h_i(x)), \tag{5.3}
$$

其中 $I_-:\mathbb{R}\to\mathbb{R}$ 是非正实数的示性函数：

$$
I_-(u)=\begin{cases}0 & u\le0\\ \infty & u>0,\end{cases}
$$

类似地 $I_0$ 是 $\{0\}$ 的示性函数。在表述 (5.3) 中，函数 $I_-(u)$ 可以解释为表达我们对约束函数值 $u=f_i(x)$ 的 irritation（恼怒）或 displeasure（不快）：$f_i(x)\le0$ 时为零，$f_i(x)>0$ 时为无穷。类似地，$I_0(u)$ 给出我们对等式约束值 $u=h_i(x)$ 的不快。可以把 $I_-$ 想成「brick wall（砖墙）」或「infinitely hard（无限硬）」的不快函数：当 $f_i(x)$ 从非正变为正时，我们的不快从零跳到无穷。

现在在表述 (5.3) 中，把函数 $I_-(u)$ 替换为线性函数 $\lambda_iu$（$\lambda_i\ge0$），把 $I_0(u)$ 替换为 $\nu_iu$。目标变为拉格朗日函数 $L(x,\lambda,\nu)$，而对偶函数值 $g(\lambda,\nu)$ 是问题

$$
\text{minimize}\quad L(x,\lambda,\nu)=f_0(x)+\sum_{i=1}^m\lambda_if_i(x)+\sum_{i=1}^p\nu_ih_i(x) \tag{5.4}
$$

的最优值。在这个表述中，我们用 linear 或「soft（软）」的不快函数代替 $I_-$ 与 $I_0$。对不等式约束，$f_i(x)=0$ 时不快为零，$f_i(x)>0$ 时不快为正（设 $\lambda_i>0$）；约束「违反得越多」不快越大。与原表述（任何非正的 $f_i(x)$ 都可接受）不同，在软表述中，我们从有 margin（裕量）的约束（即 $f_i(x)<0$）中实际上获得 pleasure（快感）。

显然用线性函数 $\lambda_iu$ 近似示性函数 $I_-(u)$ 相当粗糙。但线性函数至少是示性函数的 underestimator（低估器）。由于对所有 $u$ 有 $\lambda_iu\le I_-(u)$、$\nu_iu\le I_0(u)$，我们立即看到对偶函数给出原问题最优值的下界。

<!-- p.232 -->

用「软」版本替换「硬」约束的想法在考虑 interior-point methods（内点法，§11.2.1）时会再次出现。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.1.5 Examples（例子）</span>

本节给出几个能推导拉格朗日对偶函数解析表达式的例子。

**线性方程组的最小二乘解。** 考虑问题

$$
\begin{aligned}
\text{minimize}\quad & x^Tx\\
\text{subject to}\quad & Ax=b,
\end{aligned} \tag{5.5}
$$

其中 $A\in\mathbb{R}^{p\times n}$。此问题没有不等式约束，有 $p$ 个（线性）等式约束。拉格朗日函数为 $L(x,\nu)=x^Tx+\nu^T(Ax-b)$，定义域 $\mathbb{R}^n\times\mathbb{R}^p$。

<!-- p.233 -->

对偶函数为 $g(\nu)=\inf_xL(x,\nu)$。由于 $L(x,\nu)$ 是 $x$ 的凸二次函数，可以从最优性条件 $\nabla_xL(x,\nu)=2x+A^T\nu=0$ 找到最小化点，得 $x=-(1/2)A^T\nu$。因此对偶函数为

$$
g(\nu)=L(-(1/2)A^T\nu,\nu)=-(1/4)\nu^TAA^T\nu-b^T\nu,
$$

它是凹二次函数，定义域 $\mathbb{R}^p$。下界性质 (5.2) 陈述：对任何 $\nu\in\mathbb{R}^p$，

$$
-(1/4)\nu^TAA^T\nu-b^T\nu\le\inf\{x^Tx \mid Ax=b\}.
$$

**标准形式 LP。** 考虑标准形式的 LP：

$$
\begin{aligned}
\text{minimize}\quad & c^Tx\\
\text{subject to}\quad & Ax=b\\
& x\succeq0,
\end{aligned} \tag{5.6}
$$

它有不等式约束函数 $f_i(x)=-x_i$（$i=1,\dots,n$）。为构造拉格朗日函数，为 $n$ 个不等式约束引入乘子 $\lambda_i$、为等式约束引入乘子 $\nu_i$，得

$$
L(x,\lambda,\nu)=c^Tx-\sum_{i=1}^n\lambda_ix_i+\nu^T(Ax-b)=-b^T\nu+(c+A^T\nu-\lambda)^Tx.
$$

对偶函数为

$$
g(\lambda,\nu)=\inf_xL(x,\lambda,\nu)=-b^T\nu+\inf_x(c+A^T\nu-\lambda)^Tx,
$$

解析地容易确定，因为线性函数只在恒为零时有下界。于是 $g(\lambda,\nu)=-\infty$，除非 $c+A^T\nu-\lambda=0$，此时为 $-b^T\nu$：

$$
g(\lambda,\nu)=\begin{cases}-b^T\nu & A^T\nu-\lambda+c=0\\ -\infty & \text{否则}.\end{cases}
$$

注意对偶函数 $g$ 只在 $\mathbb{R}^m\times\mathbb{R}^p$ 的一个 proper affine subset（真仿射子集）上有限。我们会看到这很常见。

<!-- p.233/234 -->

下界性质 (5.2) 只在 $\lambda$ 与 $\nu$ 满足 $\lambda\succeq0$ 与 $A^T\nu-\lambda+c=0$ 时非平凡。此时 $-b^T\nu$ 是 LP (5.6) 最优值的下界。

**双向划分问题。** 考虑（非凸）问题

$$
\begin{aligned}
\text{minimize}\quad & x^TWx\\
\text{subject to}\quad & x_i^2=1,\quad i=1,\dots,n,
\end{aligned} \tag{5.7}
$$

其中 $W\in\mathbf{S}^n$。约束把 $x_i$ 限制为 $1$ 或 $-1$，所以问题等价于找分量为 $\pm1$ 的向量最小化 $x^TWx$。可行集有限（含 $2^n$ 个点），所以原则上可以通过检查每个可行点的目标值来求解。但可行点数指数增长，这只有小问题（比如 $n\le30$）可行。一般（比如 $n>50$）问题 (5.7) 非常难解。

<!-- p.234 -->

可以把问题 (5.7) 解释为 $n$ 个元素集合（比如 $\{1,\dots,n\}$）上的 two-way partitioning problem（双向划分问题）：可行 $x$ 对应划分

$$
\{1,\dots,n\}=\{i \mid x_i=-1\}\cup\{i \mid x_i=1\}.
$$

矩阵系数 $W_{ij}$ 可解释为元素 $i$ 与 $j$ 在同一划分的成本，$-W_{ij}$ 是 $i$ 与 $j$ 在不同划分的成本。(5.7) 的目标是所有元素对的总成本，问题 (5.7) 就是找总成本最小的划分。

现在推导此问题的对偶函数。拉格朗日函数为

$$
L(x,\nu)=x^TWx+\sum_{i=1}^n\nu_i(x_i^2-1)=x^T(W+\mathbf{diag}(\nu))x-\mathbf{1}^T\nu.
$$

对 $x$ 最小化得拉格朗日对偶函数：

$$
g(\nu)=\inf_x\bigl(x^T(W+\mathbf{diag}(\nu))x-\mathbf{1}^T\nu\bigr)=\begin{cases}-\mathbf{1}^T\nu & W+\mathbf{diag}(\nu)\succeq0\\ -\infty & \text{否则}，\end{cases}
$$

这里用了「二次型的下确界要么为零（型半正定时）要么为 $-\infty$（型非半正定时）」这一事实。

这个对偶函数给困难问题 (5.7) 的最优值提供下界。例如可取对偶变量的具体值

$$
\nu=-\lambda_{\min}(W)\mathbf{1},
$$

它对偶可行，因为 $W+\mathbf{diag}(\nu)=W-\lambda_{\min}(W)I\succeq0$。这给出最优值 $p^\star$ 的界：

$$
p^\star\ge-\mathbf{1}^T\nu=n\lambda_{\min}(W). \tag{5.8}
$$

<span style="color:#00838f;">**[注 5.1]** 这个 $p^\star$ 的下界也可以不用拉格朗日对偶函数得到。先把约束 $x_1^2=1,\dots,x_n^2=1$ 替换为 $\sum_{i=1}^nx_i^2=n$，得到修改的问题

$$
\begin{aligned}
\text{minimize}\quad & x^TWx\\
\text{subject to}\quad & \sum_{i=1}^nx_i^2=n.
\end{aligned} \tag{5.9}
$$

原问题 (5.7) 的约束蕴含这里的约束，所以问题 (5.9) 的最优值是 (5.7) 最优值 $p^\star$ 的下界。而修改后的问题 (5.9) 容易作为 eigenvalue problem（特征值问题）求解，最优值为 $n\lambda_{\min}(W)$。

<!-- p.235 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.1.6 The Lagrange dual function and conjugate functions（拉格朗日对偶函数与共轭函数）</span>

回顾 §3.3：函数 $f:\mathbb{R}^n\to\mathbb{R}$ 的 conjugate（共轭）$f^*$ 为

$$
f^*(y)=\sup_{x\in\mathbf{dom}\,f}\bigl(y^Tx-f(x)\bigr).
$$

共轭函数与拉格朗日对偶函数密切相关。一个简单联系：考虑问题

$$
\begin{aligned}
\text{minimize}\quad & f(x)\\
\text{subject to}\quad & x=0
\end{aligned}
$$

（不太有趣，一眼可解）。此问题的拉格朗日函数为 $L(x,\nu)=f(x)+\nu^Tx$，对偶函数为

$$
g(\nu)=\inf_x\bigl(f(x)+\nu^Tx\bigr)=-\sup_x\bigl((-\nu)^Tx-f(x)\bigr)=-f^*(-\nu).
$$

更一般（也更有用）地，考虑带线性不等式与等式约束的优化问题：

$$
\begin{aligned}
\text{minimize}\quad & f_0(x)\\
\text{subject to}\quad & Ax\preceq b\\
& Cx=d.
\end{aligned} \tag{5.10}
$$

用 $f_0$ 的共轭可以把问题 (5.10) 的对偶函数写为

$$
\begin{aligned}
g(\lambda,\nu)&=\inf_x\bigl(f_0(x)+\lambda^T(Ax-b)+\nu^T(Cx-d)\bigr)\\
&=-b^T\lambda-d^T\nu+\inf_x\bigl(f_0(x)+(A^T\lambda+C^T\nu)^Tx\bigr)\\
&=-b^T\lambda-d^T\nu-f_0^*(-A^T\lambda-C^T\nu). \tag{5.11}
\end{aligned}
$$

$g$ 的定义域由 $f_0^*$ 的定义域得出：

$$
\mathbf{dom}\,g=\{(\lambda,\nu) \mid -A^T\lambda-C^T\nu\in\mathbf{dom}\,f_0^*\}.
$$

用几个例子说明。

<!-- p.235/236 -->

**等式约束的范数最小化。** 考虑问题

$$
\begin{aligned}
\text{minimize}\quad & \|x\|\\
\text{subject to}\quad & Ax=b,
\end{aligned} \tag{5.12}
$$

其中 $\|\cdot\|$ 是任意范数。回顾（例 3.26，第 93 页）$f_0=\|\cdot\|$ 的共轭为

$$
f_0^*(y)=\begin{cases}0 & \|y\|_*\le1\\ \infty & \text{否则}，\end{cases}
$$

即对偶范数单位球的示性函数。用上面结果 (5.11)，问题 (5.12) 的对偶函数为

$$
g(\nu)=-b^T\nu-f_0^*(-A^T\nu)=\begin{cases}-b^T\nu & \|A^T\nu\|_*\le1\\ -\infty & \text{否则}.\end{cases}
$$

<!-- p.236 -->

**熵最大化。** 考虑 entropy maximization problem（熵最大化问题）

$$
\begin{aligned}
\text{minimize}\quad & f_0(x)=\sum_{i=1}^nx_i\log x_i\\
\text{subject to}\quad & Ax\preceq b\\
& \mathbf{1}^Tx=1
\end{aligned} \tag{5.13}
$$

（$\mathbf{dom}\,f_0=\mathbb{R}^n_{++}$）。负熵函数 $u\log u$（标量变量 $u$）的共轭是 $e^{v-1}$（见例 3.21，第 91 页）。由于 $f_0$ 是不同变量的负熵函数之和，其共轭为

$$
f_0^*(y)=\sum_{i=1}^ne^{y_i-1},
$$

$\mathbf{dom}\,f_0^*=\mathbb{R}^n$。用结果 (5.11)，(5.13) 的对偶函数为

$$
g(\lambda,\nu)=-b^T\lambda-\nu-\sum_{i=1}^ne^{-a_i^T\lambda-\nu-1}=-b^T\lambda-\nu-e^{-\nu-1}\sum_{i=1}^ne^{-a_i^T\lambda},
$$

其中 $a_i$ 是 $A$ 的第 $i$ 列。

**最小体积覆盖椭球。** 考虑变量 $X\in\mathbf{S}^n$ 的问题：

$$
\begin{aligned}
\text{minimize}\quad & f_0(X)=\log\det X^{-1}\\
\text{subject to}\quad & a_i^TXa_i\le1,\quad i=1,\dots,m,
\end{aligned} \tag{5.14}
$$

$\mathbf{dom}\,f_0=\mathbf{S}^n_{++}$。问题 (5.14) 有简单的几何解释：给每个 $X\in\mathbf{S}^n_{++}$ 关联以原点为中心的椭球 $\mathcal{E}_X=\{z \mid z^TXz\le1\}$。椭球体积正比于 $(\det X^{-1})^{1/2}$，所以 (5.14) 的目标（除常数与因子 2 外）是 $\mathcal{E}_X$ 体积的对数。

<!-- p.236/237 -->

问题 (5.14) 的约束是 $a_i\in\mathcal{E}_X$。因此问题 (5.14) 是确定以原点为中心、包含点 $a_1,\dots,a_m$ 的最小体积椭球。

问题 (5.14) 的不等式约束是仿射的；可表达为 $\mathbf{tr}\bigl((a_ia_i^T)X\bigr)\le1$。例 3.23（第 92 页）中已找到 $f_0$ 的共轭：

$$
f_0^*(Y)=\log\det(-Y)^{-1}-n,\qquad \mathbf{dom}\,f_0^*=-\mathbf{S}^n_{++}.
$$

应用结果 (5.11)，问题 (5.14) 的对偶函数为

$$
g(\lambda)=\begin{cases}\log\det\bigl(\sum_{i=1}^m\lambda_ia_ia_i^T\bigr)-\mathbf{1}^T\lambda+n & \sum_{i=1}^m\lambda_ia_ia_i^T\succ0\\ -\infty & \text{否则}，\end{cases} \tag{5.15}
$$

<!-- p.237 -->

于是对任何满足 $\lambda\succeq0$ 且 $\sum_{i=1}^m\lambda_ia_ia_i^T\succ0$ 的 $\lambda$，数

$$
\log\det\Bigl(\sum_{i=1}^m\lambda_ia_ia_i^T\Bigr)-\mathbf{1}^T\lambda+n
$$

是问题 (5.14) 最优值的下界。

## <span style="color:#c0392b;">5.2 The Lagrange dual problem（拉格朗日对偶问题）</span>

对每对满足 $\lambda\succeq0$ 的 $(\lambda,\nu)$，拉格朗日对偶函数给出优化问题 (5.1) 最优值 $p^\star$ 的下界。于是我们有了依赖参数 $\lambda,\nu$ 的下界。一个自然的问题是：从拉格朗日对偶函数能得到的最好下界是什么？这导出优化问题

$$
\begin{aligned}
\text{maximize}\quad & g(\lambda,\nu)\\
\text{subject to}\quad & \lambda\succeq0.
\end{aligned} \tag{5.16}
$$

此问题称为与问题 (5.1) 相关联的 Lagrange dual problem（拉格朗日对偶问题）。在此语境下，原问题 (5.1) 有时称为 primal problem（原始问题/主问题）。描述满足 $\lambda\succeq0$ 且 $g(\lambda,\nu)>-\infty$ 的数对 $(\lambda,\nu)$ 的术语 dual feasible（对偶可行）现在说得通了：正如其名，它意味着 $(\lambda,\nu)$ 对对偶问题 (5.16) 可行。若 $(\lambda^\star,\nu^\star)$ 对问题 (5.16) 最优，我们称它们为 dual optimal（对偶最优）或 optimal Lagrange multipliers（最优拉格朗日乘子）。

拉格朗日对偶问题 (5.16) 是凸优化问题：要最大化的目标凹、约束凸。无论原始问题 (5.1) 是否凸都如此。

<!-- p.238 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.2.1 Making dual constraints explicit（使对偶约束显式化）</span>

上面的例子表明：对偶函数的定义域

$$
\mathbf{dom}\,g=\{(\lambda,\nu) \mid g(\lambda,\nu)>-\infty\}
$$

的维数小于 $m+p$ 并不少见。许多情形我们能识别 $\mathbf{dom}\,g$ 的仿射包，并把它描述为一组线性等式约束。粗略地说，这意味着我们能识别对偶问题 (5.16) 的目标 $g$ 中「隐藏」或「隐式」的等式约束。此时可以构造等价问题，把这些等式约束显式地作为约束给出。下面的例子演示这个想法。

**标准形式 LP 的拉格朗日对偶。** 第 219 页我们找到标准形式 LP

$$
\begin{aligned}
\text{minimize}\quad & c^Tx\\
\text{subject to}\quad & Ax=b\\
& x\succeq0
\end{aligned} \tag{5.17}
$$

的拉格朗日对偶函数：

$$
g(\lambda,\nu)=\begin{cases}-b^T\nu & A^T\nu-\lambda+c=0\\ -\infty & \text{否则}.\end{cases}
$$

严格地说，标准形式 LP 的拉格朗日对偶问题是：在 $\lambda\succeq0$ 下最大化这个对偶函数 $g$，即

$$
\begin{aligned}
\text{maximize}\quad & g(\lambda,\nu)=\begin{cases}-b^T\nu & A^T\nu-\lambda+c=0\\ -\infty & \text{否则}\end{cases}\\
\text{subject to}\quad & \lambda\succeq0.
\end{aligned} \tag{5.18}
$$

这里 $g$ 只在 $A^T\nu-\lambda+c=0$ 时有限。可以构造等价问题，把这些等式约束显式化：

$$
\begin{aligned}
\text{maximize}\quad & -b^T\nu\\
\text{subject to}\quad & A^T\nu-\lambda+c=0\\
& \lambda\succeq0.
\end{aligned} \tag{5.19}
$$

<!-- p.238/239 -->

这个问题又可以表达为

$$
\begin{aligned}
\text{maximize}\quad & -b^T\nu\\
\text{subject to}\quad & A^T\nu+c\succeq0,
\end{aligned} \tag{5.20}
$$

是不等式形式的 LP。

<!-- p.239 -->

注意这三个问题间的微妙区别。标准形式 LP (5.17) 的拉格朗日对偶是问题 (5.18)，它与 (5.19)、(5.20) 等价（但不相同）。稍微滥用术语，我们把问题 (5.19) 或 (5.20) 称为标准形式 LP (5.17) 的拉格朗日对偶。

**不等式形式 LP 的拉格朗日对偶。** 类似地可以找不等式形式线性规划

$$
\begin{aligned}
\text{minimize}\quad & c^Tx\\
\text{subject to}\quad & Ax\preceq b
\end{aligned} \tag{5.21}
$$

的拉格朗日对偶问题。拉格朗日函数为

$$
L(x,\lambda)=c^Tx+\lambda^T(Ax-b)=-b^T\lambda+(A^T\lambda+c)^Tx,
$$

所以对偶函数为

$$
g(\lambda)=\inf_xL(x,\lambda)=-b^T\lambda+\inf_x(A^T\lambda+c)^Tx.
$$

线性函数的下确界为 $-\infty$，除非它恒为零，所以对偶函数为

$$
g(\lambda)=\begin{cases}-b^T\lambda & A^T\lambda+c=0\\ -\infty & \text{否则}.\end{cases}
$$

对偶变量 $\lambda$ 对偶可行，若 $\lambda\succeq0$ 且 $A^T\lambda+c=0$。

LP (5.21) 的拉格朗日对偶是在所有 $\lambda\succeq0$ 上最大化 $g$。同样可以通过显式包含对偶可行性条件重新表述：

$$
\begin{aligned}
\text{maximize}\quad & -b^T\lambda\\
\text{subject to}\quad & A^T\lambda+c=0\\
& \lambda\succeq0,
\end{aligned} \tag{5.22}
$$

它是标准形式的 LP。

注意标准形式与不等式形式 LP 及其对偶之间有趣的对称性：标准形式 LP 的对偶是只有不等式约束的 LP，反之亦然。还可以验证 (5.22) 的拉格朗日对偶（等价于）原问题 (5.21)。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.2.2 Weak duality（弱对偶性）</span>

<!-- p.239/240 -->

拉格朗日对偶问题的最优值记作 $d^\star$，按定义它是从拉格朗日对偶函数能得到的 $p^\star$ 的最好下界。特别地，有简单而重要的不等式

$$
d^\star\le p^\star, \tag{5.23}
$$

即使原问题不凸也成立。这个性质称为 weak duality（弱对偶性）。

弱对偶不等式 (5.23) 在 $d^\star$、$p^\star$ 为无穷时也成立。例如若原始问题无下界（$p^\star=-\infty$），则必有 $d^\star=-\infty$，即拉格朗日对偶问题不可行。反之若对偶问题无上界（$d^\star=\infty$），则必有 $p^\star=\infty$，即原始问题不可行。

<!-- p.240 -->

我们把差 $p^\star-d^\star$ 称为原问题的 optimal duality gap（最优对偶间隙），它给出原始问题最优值与从拉格朗日对偶函数能得到的最好（即最大）下界之间的 gap（间隙）。最优对偶间隙总是非负。

界 (5.23) 有时可用于给难解问题的最优值找下界，因为对偶问题总是凸的，许多情形能高效求解得到 $d^\star$。例如考虑第 219 页描述的双向划分问题 (5.7)。其对偶问题是 SDP：

$$
\begin{aligned}
\text{maximize}\quad & -\mathbf{1}^T\nu\\
\text{subject to}\quad & W+\mathbf{diag}(\nu)\succeq0,
\end{aligned}
$$

变量 $\nu\in\mathbb{R}^n$。这个问题即使 $n$ 相当大（如 $n=1000$）也能高效求解。其最优值是双向划分问题最优值的下界，且总不差于基于 $\lambda_{\min}(W)$ 的下界 (5.8)。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.2.3 Strong duality and Slater's constraint qualification（强对偶性与 Slater 约束规格）</span>

若等式

$$
d^\star=p^\star \tag{5.24}
$$

成立（即最优对偶间隙为零），则称 strong duality（强对偶性）成立。这意味着从拉格朗日对偶函数能得到的最好界是 tight（紧的）。

强对偶性一般不成立。但若原始问题 (5.1) 凸，即形如

$$
\begin{aligned}
\text{minimize}\quad & f_0(x)\\
\text{subject to}\quad & f_i(x)\le0,\quad i=1,\dots,m,\\
& Ax=b
\end{aligned} \tag{5.25}
$$

（$f_0,\dots,f_m$ 凸），我们通常（但非总是）有强对偶性。有许多结果确立凸性之外、使强对偶性成立的条件。这些条件称为 constraint qualifications（约束规格）。一个简单的约束规格是 Slater's condition（Slater 条件）：存在 $x\in\mathbf{relint}\,\mathcal{D}$ 使

$$
f_i(x)<0,\quad i=1,\dots,m,\qquad Ax=b. \tag{5.26}
$$

这样的点有时称为 strictly feasible（严格可行）的，因为不等式约束以严格不等号成立。Slater's theorem（Slater 定理）陈述：若 Slater 条件成立（且问题凸），则强对偶性成立。

当某些不等式约束函数 $f_i$ 是仿射时，Slater 条件可以精化。若前 $k$ 个约束函数 $f_1,\dots,f_k$ 仿射，则只要下面较弱的条件成立强对偶性就成立：存在 $x\in\mathbf{relint}\,\mathcal{D}$ 使

$$
\begin{aligned}
f_i(x)\le0,&\quad i=1,\dots,k,\\
f_i(x)<0,&\quad i=k+1,\dots,m,\\
Ax=b.&\end{aligned} \tag{5.27}
$$

<!-- p.240/241 -->

换言之，仿射不等式不需要严格成立。注意当约束全是线性等式与不等式、且 $\mathbf{dom}\,f_0$ 开时，精化的 Slater 条件 (5.27) 退化为可行性。

<!-- p.241 -->

Slater 条件（及精化 (5.27)）不仅蕴含凸问题的强对偶性，还蕴含：当 $d^\star>-\infty$ 时对偶最优值被达到，即存在对偶可行的 $(\lambda^\star,\nu^\star)$ 使 $g(\lambda^\star,\nu^\star)=d^\star=p^\star$。我们将在 §5.3.2 证明：当原始问题凸且 Slater 条件成立时强对偶性成立。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.2.4 Examples（例子）</span>

**线性方程组的最小二乘解。** 回顾问题 (5.5)：$\min x^Tx\ \text{s.t.}\ Ax=b$。相应的对偶问题是

$$
\text{maximize}\quad -(1/4)\nu^TAA^T\nu-b^T\nu,
$$

是无约束的凹二次最大化问题。Slater 条件就是原始问题可行，所以只要 $b\in\mathcal{R}(A)$（即 $p^\star<\infty$）就有 $p^\star=d^\star$。事实上这个问题总有强对偶性，即使 $p^\star=\infty$：那是 $b\notin\mathcal{R}(A)$ 的情形，此时存在 $z$ 使 $A^Tz=0$、$b^Tz\neq0$，于是对偶函数沿直线 $\{tz \mid t\in\mathbb{R}\}$ 无上界，故 $d^\star=\infty$。

**LP 的拉格朗日对偶。** 由 Slater 条件的较弱形式：对任何 LP（标准或不等式形式），只要原始问题可行就有强对偶性。把这个结果应用到对偶上，得出：只要对偶可行，LP 强对偶性成立。这只剩下强对偶性可能失效的一种情形：原始与对偶问题都不可行。这种 pathological case（病态情形）确实可能发生；见习题 5.23。

<!-- p.241/242 -->

**QCQP 的拉格朗日对偶。** 考虑 QCQP

$$
\begin{aligned}
\text{minimize}\quad & (1/2)x^TP_0x+q_0^Tx+r_0\\
\text{subject to}\quad & (1/2)x^TP_ix+q_i^Tx+r_i\le0,\quad i=1,\dots,m,
\end{aligned} \tag{5.28}
$$

$P_0\in\mathbf{S}^n_{++}$，$P_i\in\mathbf{S}^n_+$（$i=1,\dots,m$）。拉格朗日函数为

$$
L(x,\lambda)=(1/2)x^TP(\lambda)x+q(\lambda)^Tx+r(\lambda),
$$

其中

$$
P(\lambda)=P_0+\sum_{i=1}^m\lambda_iP_i,\qquad q(\lambda)=q_0+\sum_{i=1}^m\lambda_iq_i,\qquad r(\lambda)=r_0+\sum_{i=1}^m\lambda_ir_i.
$$

<!-- p.242 -->

对一般的 $\lambda$ 可以推导 $g(\lambda)$ 的表达式，但相当复杂。不过若 $\lambda\succeq0$，则 $P(\lambda)\succ0$，且

$$
g(\lambda)=\inf_xL(x,\lambda)=-(1/2)q(\lambda)^TP(\lambda)^{-1}q(\lambda)+r(\lambda).
$$

因此可以把对偶问题表达为

$$
\begin{aligned}
\text{maximize}\quad & -(1/2)q(\lambda)^TP(\lambda)^{-1}q(\lambda)+r(\lambda)\\
\text{subject to}\quad & \lambda\succeq0.
\end{aligned} \tag{5.29}
$$

Slater 条件说：若二次不等式约束严格可行，即存在 $x$ 使 $(1/2)x^TP_ix+q_i^Tx+r_i<0$（$i=1,\dots,m$），则 (5.29) 与 (5.28) 之间强对偶性成立。

**熵最大化。** 下一个例子是熵最大化问题 (5.13)：$\min\sum_{i=1}^nx_i\log x_i\ \text{s.t.}\ Ax\preceq b,\ \mathbf{1}^Tx=1$，定义域 $\mathcal{D}=\mathbb{R}^n_+$。拉格朗日对偶函数已在第 222 页推导；对偶问题为

$$
\begin{aligned}
\text{maximize}\quad & -b^T\lambda-\nu-e^{-\nu-1}\sum_{i=1}^ne^{-a_i^T\lambda}\\
\text{subject to}\quad & \lambda\succeq0,
\end{aligned} \tag{5.30}
$$

变量 $\lambda\in\mathbb{R}^m$、$\nu\in\mathbb{R}$。(5.13) 的（较弱）Slater 条件告诉我们：若存在 $x\succ0$ 满足 $Ax\preceq b$ 与 $\mathbf{1}^Tx=1$，则最优对偶间隙为零。

可以对偶变量 $\nu$ 解析地最大化来简化对偶问题 (5.30)。固定 $\lambda$，目标函数在关于 $\nu$ 的导数为零时最大：

$$
\nu=\log\sum_{i=1}^ne^{-a_i^T\lambda}-1.
$$

把这个 $\nu$ 的最优值代入对偶问题得

$$
\begin{aligned}
\text{maximize}\quad & -b^T\lambda-\log\Bigl(\sum_{i=1}^ne^{-a_i^T\lambda}\Bigr)\\
\text{subject to}\quad & \lambda\succeq0,
\end{aligned}
$$

这是带非负性约束的 geometric program（几何规划，凸形式）。

<!-- p.242/243 -->

**最小体积覆盖椭球。** 考虑问题 (5.14)：$\min\log\det X^{-1}\ \text{s.t.}\ a_i^TXa_i\le1$（$i=1,\dots,m$），定义域 $\mathcal{D}=\mathbf{S}^n_{++}$。拉格朗日对偶函数由 (5.15) 给出，所以对偶问题可表达为

$$
\begin{aligned}
\text{maximize}\quad & \log\det\Bigl(\sum_{i=1}^m\lambda_ia_ia_i^T\Bigr)-\mathbf{1}^T\lambda+n\\
\text{subject to}\quad & \lambda\succeq0
\end{aligned} \tag{5.31}
$$

<!-- p.243 -->

（当 $X\not\succ0$ 时取 $\log\det X=-\infty$）。问题 (5.14) 的（较弱）Slater 条件是存在 $X\in\mathbf{S}^n_{++}$ 使 $a_i^TXa_i\le1$（$i=1,\dots,m$）。这总满足，所以 (5.14) 与对偶问题 (5.31) 之间强对偶性总成立。

**有强对偶性的非凸二次问题。** 在罕见情形下非凸问题也有强对偶性。一个重要例子：在单位球上最小化非凸二次函数：

$$
\begin{aligned}
\text{minimize}\quad & x^TAx+2b^Tx\\
\text{subject to}\quad & x^Tx\le1,
\end{aligned} \tag{5.32}
$$

$A\in\mathbf{S}^n$、$A\not\succeq0$、$b\in\mathbb{R}^n$。由于 $A\not\succeq0$，这不是凸问题。这个问题有时称为 trust region problem（信赖域问题），出现在「在单位球上（假设近似在该区域内大致有效）最小化函数的二阶近似」中。

拉格朗日函数为

$$
L(x,\lambda)=x^TAx+2b^Tx+\lambda(x^Tx-1)=x^T(A+\lambda I)x+2b^Tx-\lambda,
$$

所以对偶函数为

$$
g(\lambda)=\begin{cases}-b^T(A+\lambda I)^\dagger b-\lambda & A+\lambda I\succeq0,\ b\in\mathcal{R}(A+\lambda I)\\ -\infty & \text{otherwise},\end{cases}
$$

其中 $(A+\lambda I)^\dagger$ 是 $A+\lambda I$ 的伪逆。因此拉格朗日对偶问题为

$$
\begin{aligned}
\text{maximize}\quad & -b^T(A+\lambda I)^\dagger b-\lambda\\
\text{subject to}\quad & A+\lambda I\succeq0,\quad b\in\mathcal{R}(A+\lambda I),
\end{aligned} \tag{5.33}
$$

变量 $\lambda\in\mathbb{R}$。虽然从这个表达式看不出来，这是一个凸优化问题。事实上它容易求解，因为可表达为

$$
\begin{aligned}
\text{maximize}\quad & -\sum_{i=1}^n(q_i^Tb)^2/(\lambda_i+\lambda)-\lambda\\
\text{subject to}\quad & \lambda\ge-\lambda_{\min}(A),
\end{aligned}
$$

其中 $\lambda_i$ 与 $q_i$ 是 $A$ 的特征值与相应（正交归一）特征向量，且当 $q_i^Tb=0$ 时 $(q_i^Tb)^2/0$ 解释为 0，否则为 $\infty$。

<!-- p.243/244 -->

尽管原问题 (5.32) 不凸，此问题的最优对偶间隙总为零：问题 (5.32) 与 (5.33) 的最优值总是相同。事实上更一般的结果成立：对任何「二次目标 + 一个二次不等式约束」的优化问题，只要 Slater 条件成立，强对偶性成立；见 §B.1。

<!-- p.244 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.2.5 Mixed strategies for matrix games（矩阵博弈的混合策略）</span>

本节用强对偶性推导 zero-sum matrix games（零和矩阵博弈）的一个基本结果。考虑两人博弈：玩家 1 选择（走子）$k\in\{1,\dots,n\}$，玩家 2 选择 $l\in\{1,\dots,m\}$。然后玩家 1 向玩家 2 支付 $P_{kl}$，$P\in\mathbb{R}^{n\times m}$ 是博弈的 payoff matrix（支付矩阵）。玩家 1 的目标是支付尽量小，玩家 2 的目标是最大化支付。

玩家使用 randomized 或 mixed strategies（随机化或混合策略）：每个玩家按概率分布随机、独立地选择：

$$
\mathrm{prob}(k=i)=u_i,\ i=1,\dots,n;\qquad \mathrm{prob}(l=i)=v_i,\ i=1,\dots,m.
$$

这里 $u$ 与 $v$ 给出两个玩家选择的概率分布，即其相应的 strategies（策略）。玩家 1 向玩家 2 的 expected payoff（期望支付）为

$$
\sum_{k=1}^n\sum_{l=1}^mu_kv_lP_{kl}=u^TPv.
$$

玩家 1 想选 $u$ 最小化 $u^TPv$，玩家 2 想选 $v$ 最大化 $u^TPv$。

先从玩家 1 的角度分析博弈，假设她的策略 $u$ 被玩家 2 知道（显然给玩家 2 优势）。玩家 2 会选 $v$ 最大化 $u^TPv$，产生期望支付

$$
\sup\{u^TPv \mid v\succeq0,\ \mathbf{1}^Tv=1\}=\max_{i=1,\dots,m}(P^Tu)_i.
$$

玩家 1 能做的最好是选 $u$ 最小化这个最坏情况支付，即选策略 $u$ 求解问题

$$
\begin{aligned}
\text{minimize}\quad & \max_{i=1,\dots,m}(P^Tu)_i\\
\text{subject to}\quad & u\succeq0,\quad \mathbf{1}^Tu=1,
\end{aligned} \tag{5.34}
$$

它是分段线性凸优化问题。记其最优值为 $p_1^\star$。这是玩家 1 能安排的最小期望支付——假设玩家 2 知道玩家 1 的策略并按自己最大优势行动。

<!-- p.244/245 -->

类似地可以考虑玩家 2 的策略 $v$ 被玩家 1 知道的情形（给玩家 1 优势）。此时玩家 1 选 $u$ 最小化 $u^TPv$，产生期望支付

$$
\inf\{u^TPv \mid u\succeq0,\ \mathbf{1}^Tu=1\}=\min_{i=1,\dots,n}(Pv)_i.
$$

玩家 2 选 $v$ 最大化它，即选策略 $v$ 求解问题

$$
\begin{aligned}
\text{maximize}\quad & \min_{i=1,\dots,n}(Pv)_i\\
\text{subject to}\quad & v\succeq0,\quad \mathbf{1}^Tv=1,
\end{aligned} \tag{5.35}
$$

<!-- p.245 -->

这是另一个凸优化问题，目标分段线性（凹）。记其最优值为 $p_2^\star$。这是玩家 2 能保证获得的最大期望支付——假设玩家 1 知道玩家 2 的策略。

直觉上显然：知道对手的策略有优势（至少不会有害），而且容易证明总有 $p_1^\star\ge p_2^\star$。可以把差 $p_1^\star-p_2^\star$（非负）解释为「知道对手策略」赋予玩家的优势。

用对偶性可以建立一个初看令人惊讶的结果：$p_1^\star=p_2^\star$。换言之，在带混合策略的矩阵博弈中，知道对手的策略没有优势。我们将通过证明问题 (5.34) 与 (5.35) 是强对偶性成立的拉格朗日对偶问题来确立这个结果。

先把 (5.34) 表述为 LP：

$$
\begin{aligned}
\text{minimize}\quad & t\\
\text{subject to}\quad & u\succeq0,\quad \mathbf{1}^Tu=1\\
& P^Tu\preceq t\mathbf{1},
\end{aligned}
$$

额外变量 $t\in\mathbb{R}$。为 $P^Tu\preceq t\mathbf{1}$ 引入乘子 $\lambda$、为 $u\succeq0$ 引入 $\mu$、为 $\mathbf{1}^Tu=1$ 引入 $\nu$，拉格朗日函数为

$$
t+\lambda^T(P^Tu-t\mathbf{1})-\mu^Tu+\nu(1-\mathbf{1}^Tu)=\nu+(1-\mathbf{1}^T\lambda)t+(P\lambda-\nu\mathbf{1}-\mu)^Tu,
$$

所以对偶函数为

$$
g(\lambda,\mu,\nu)=\begin{cases}\nu & \mathbf{1}^T\lambda=1,\ P\lambda-\nu\mathbf{1}=\mu\\ -\infty & \text{否则}.\end{cases}
$$

对偶问题为

$$
\begin{aligned}
\text{maximize}\quad & \nu\\
\text{subject to}\quad & \lambda\succeq0,\ \mathbf{1}^T\lambda=1,\ \mu\succeq0\\
& P\lambda-\nu\mathbf{1}=\mu.
\end{aligned}
$$

消去 $\mu$ 得 (5.34) 的拉格朗日对偶：

$$
\begin{aligned}
\text{maximize}\quad & \nu\\
\text{subject to}\quad & \lambda\succeq0,\ \mathbf{1}^T\lambda=1\\
& P\lambda\succeq\nu\mathbf{1},
\end{aligned}
$$

变量 $\lambda,\nu$。但显然它等价于 (5.35)。由于 LP 可行，强对偶性成立：问题 (5.34) 与 (5.35) 的最优值相等。

> <span style="color:#2471a3;">**[译者注｜跨课程连接]** 本节的 minimax 定理是博弈论的基石，也直接通向 CLRS 里「随机算法对最坏情况输入的期望性能」的思想：先手劣势 = 弱对偶间隙 = $p_1^\star-p_2^\star$，而混合策略使间隙归零。</span>

## <span style="color:#c0392b;">5.3 Geometric interpretation（几何解释）</span>

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.3.1 Weak and strong duality via set of values（借助值集合的弱与强对偶）</span>

<!-- p.246 -->

可以用集合

$$
\mathcal{G}=\{(f_1(x),\dots,f_m(x),\,h_1(x),\dots,h_p(x),\,f_0(x))\in\mathbb{R}^m\times\mathbb{R}^p\times\mathbb{R} \mid x\in\mathcal{D}\} \tag{5.36}
$$

（约束与目标函数取值的集合）给出对偶函数的简单几何解释。(5.1) 的最优值 $p^\star$ 用 $\mathcal{G}$ 容易表达为

$$
p^\star=\inf\{t \mid (u,v,t)\in\mathcal{G},\ u\preceq0,\ v=0\}.
$$

求对偶函数在 $(\lambda,\nu)$ 处的值，就是在 $(u,v,t)\in\mathcal{G}$ 上最小化仿射函数

$$
(\lambda,\nu,1)^T(u,v,t)=\sum_{i=1}^m\lambda_iu_i+\sum_{i=1}^p\nu_iv_i+t,
$$

即

$$
g(\lambda,\nu)=\inf\{(\lambda,\nu,1)^T(u,v,t) \mid (u,v,t)\in\mathcal{G}\}.
$$

特别地，若下确界有限，则不等式

$$
(\lambda,\nu,1)^T(u,v,t)\ge g(\lambda,\nu)
$$

定义 $\mathcal{G}$ 的一个支撑超平面。这有时称为 nonvertical supporting hyperplane（非竖直支撑超平面），因为法向量的最后一个分量非零。

<!-- p.246/247 -->

现在设 $\lambda\succeq0$。则显然：若 $u\preceq0$、$v=0$，有 $t\ge(\lambda,\nu,1)^T(u,v,t)$。因此

$$
\begin{aligned}
p^\star&=\inf\{t \mid (u,v,t)\in\mathcal{G},\ u\preceq0,\ v=0\}\\
&\ge \inf\{(\lambda,\nu,1)^T(u,v,t) \mid (u,v,t)\in\mathcal{G},\ u\preceq0,\ v=0\}\\
&\ge \inf\{(\lambda,\nu,1)^T(u,v,t) \mid (u,v,t)\in\mathcal{G}\}\\
&= g(\lambda,\nu),
\end{aligned}
$$

即得到弱对偶性。这个解释见图 5.3 与 5.4（单不等式约束的简单问题）。

<!-- p.247/248 -->

**上图变体。** 本节描述基于 $\mathcal{G}$ 的对偶几何解释的一个变体，它解释了为什么（大多数）凸问题有强对偶性。定义集合 $\mathcal{A}\subseteq\mathbb{R}^m\times\mathbb{R}^p\times\mathbb{R}$：

$$
\mathcal{A}=\mathcal{G}+\bigl(\mathbb{R}^m_+\times\{0\}\times\mathbb{R}_+\bigr), \tag{5.37}
$$

更显式地：

$$
\mathcal{A}=\{(u,v,t) \mid \exists x\in\mathcal{D},\ f_i(x)\le u_i,\ i=1,\dots,m,\ h_i(x)=v_i,\ i=1,\dots,p,\ f_0(x)\le t\}.
$$

<!-- p.248 -->

可以把 $\mathcal{A}$ 想成 $\mathcal{G}$ 的某种上图形式：$\mathcal{A}$ 包含 $\mathcal{G}$ 中的所有点，以及「更差」的点——目标或不等式约束函数值更大的点。

用 $\mathcal{A}$ 可以把最优值表达为

$$
p^\star=\inf\{t \mid (0,0,t)\in\mathcal{A}\}.
$$

在点 $(\lambda,\nu)$（$\lambda\succeq0$）处求对偶函数，可以在 $\mathcal{A}$ 上最小化仿射函数 $(\lambda,\nu,1)^T(u,v,t)$：若 $\lambda\succeq0$，则

$$
g(\lambda,\nu)=\inf\{(\lambda,\nu,1)^T(u,v,t) \mid (u,v,t)\in\mathcal{A}\}.
$$

若下确界有限，则

$$
(\lambda,\nu,1)^T(u,v,t)\ge g(\lambda,\nu)
$$

定义 $\mathcal{A}$ 的一个非竖直支撑超平面。特别地，由于 $(0,0,p^\star)\in\mathbf{bd}\,\mathcal{A}$，有

$$
p^\star=(\lambda,\nu,1)^T(0,0,p^\star)\ge g(\lambda,\nu), \tag{5.38}
$$

即弱对偶下界。强对偶性成立当且仅当 (5.38) 对某个对偶可行的 $(\lambda,\nu)$ 取等，即 $\mathcal{A}$ 在其边界点 $(0,0,p^\star)$ 处存在非竖直支撑超平面。

第二种解释见图 5.5。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.3.2 Proof of strong duality under constraint qualification（约束规格下强对偶性的证明）</span>

<!-- p.248/249 -->

本节证明：对凸问题，Slater 约束规格保证强对偶性（且对偶最优达到）。考虑原始问题 (5.25)（$f_0,\dots,f_m$ 凸），设 Slater 条件成立：存在 $\tilde{x}\in\mathbf{relint}\,\mathcal{D}$ 使 $f_i(\tilde{x})<0$（$i=1,\dots,m$）且 $A\tilde{x}=b$。为简化证明，再加两个假设：$\mathcal{D}$ 有非空内部（故 $\mathbf{relint}\,\mathcal{D}=\mathbf{int}\,\mathcal{D}$），以及 $\mathbf{rank}A=p$。设 $p^\star$ 有限。（由于有可行点，只可能 $p^\star=-\infty$ 或 $p^\star$ 有限；若 $p^\star=-\infty$，则由弱对偶性 $d^\star=-\infty$。）

<!-- p.249 -->

集合 $\mathcal{A}$（由 (5.37) 定义）在底层问题凸时容易证明是凸的。定义第二个凸集

$$
\mathcal{B}=\{(0,0,s)\in\mathbb{R}^m\times\mathbb{R}^p\times\mathbb{R} \mid s<p^\star\}.
$$

集合 $\mathcal{A}$ 与 $\mathcal{B}$ 不相交。为看出这一点，设 $(u,v,t)\in\mathcal{A}\cap\mathcal{B}$。由 $(u,v,t)\in\mathcal{B}$ 得 $u=0$、$v=0$、$t<p^\star$。由 $(u,v,t)\in\mathcal{A}$，存在 $x$ 满足 $f_i(x)\le0$（$i=1,\dots,m$）、$Ax-b=0$、$f_0(x)\le t<p^\star$，这与 $p^\star$ 是原始问题最优值矛盾。

由 §2.5.1 的分离超平面定理，存在 $(\tilde{\lambda},\tilde{\nu},\mu)\neq0$ 与 $\alpha$ 使

$$
(u,v,t)\in\mathcal{A} \implies \tilde{\lambda}^Tu+\tilde{\nu}^Tv+\mu t\ge\alpha, \tag{5.39}
$$

$$
(u,v,t)\in\mathcal{B} \implies \tilde{\lambda}^Tu+\tilde{\nu}^Tv+\mu t\le\alpha. \tag{5.40}
$$

由 (5.39) 得 $\tilde{\lambda}\succeq0$ 且 $\mu\ge0$（否则 $\tilde{\lambda}^Tu+\mu t$ 在 $\mathcal{A}$ 上无下界，与 (5.39) 矛盾）。条件 (5.40) 只意味着 $\mu t\le\alpha$ 对所有 $t<p^\star$，故 $\mu p^\star\le\alpha$。结合 (5.39)，对任何 $x\in\mathcal{D}$：

$$
\sum_{i=1}^m\tilde{\lambda}_if_i(x)+\tilde{\nu}^T(Ax-b)+\mu f_0(x)\ge\alpha\ge\mu p^\star. \tag{5.41}
$$

设 $\mu>0$。此时可把 (5.41) 除以 $\mu$ 得

$$
L(x,\tilde{\lambda}/\mu,\tilde{\nu}/\mu)\ge p^\star
$$

对所有 $x\in\mathcal{D}$，由此对 $x$ 最小化得 $g(\lambda,\nu)\ge p^\star$，其中定义

$$
\lambda=\tilde{\lambda}/\mu,\qquad \nu=\tilde{\nu}/\mu.
$$

由弱对偶性 $g(\lambda,\nu)\le p^\star$，所以事实上 $g(\lambda,\nu)=p^\star$。这表明强对偶性成立且对偶最优达到——至少在 $\mu>0$ 的情形。

<!-- p.250 -->

现在考虑 $\mu=0$ 的情形。由 (5.41)，对所有 $x\in\mathcal{D}$：

$$
\sum_{i=1}^m\tilde{\lambda}_if_i(x)+\tilde{\nu}^T(Ax-b)\ge0. \tag{5.42}
$$

把它应用于满足 Slater 条件的点 $\tilde{x}$，得

$$
\sum_{i=1}^m\tilde{\lambda}_if_i(\tilde{x})\ge0.
$$

<!-- p.250/251 -->

由于 $f_i(\tilde{x})<0$ 且 $\tilde{\lambda}_i\ge0$，得 $\tilde{\lambda}=0$。由 $(\tilde{\lambda},\tilde{\nu},\mu)\neq0$ 且 $\tilde{\lambda}=0$、$\mu=0$，得 $\tilde{\nu}\neq0$。于是 (5.42) 蕴含对所有 $x\in\mathcal{D}$ 有 $\tilde{\nu}^T(Ax-b)\ge0$。但 $\tilde{x}$ 满足 $\tilde{\nu}^T(A\tilde{x}-b)=0$，且由于 $\tilde{x}\in\mathbf{int}\,\mathcal{D}$，$\mathcal{D}$ 中存在使 $\tilde{\nu}^T(Ax-b)<0$ 的点——除非 $A^T\tilde{\nu}=0$。当然这与 $\mathbf{rank}A=p$ 的假设矛盾。

<!-- p.251 -->

证明背后的几何思想见图 5.6（单不等式约束的简单问题）。分离 $\mathcal{A}$ 与 $\mathcal{B}$ 的超平面定义了 $\mathcal{A}$ 在 $(0,p^\star)$ 处的支撑超平面。Slater 约束规格用来确立该超平面必非竖直（即法向量形如 $(\lambda^\star,1)$）。（凸问题单不等式约束但强对偶性失效的简单例子见习题 5.21。）

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.3.3 Multicriterion interpretation（多准则解释）</span>

<!-- p.251 -->

没有等式约束的问题

$$
\begin{aligned}
\text{minimize}\quad & f_0(x)\\
\text{subject to}\quad & f_i(x)\le0,\quad i=1,\dots,m
\end{aligned} \tag{5.43}
$$

的拉格朗日对偶，与（无约束）多准则问题

$$
\text{minimize (w.r.t. }\mathbb{R}^{m+1}_+)\quad F(x)=(f_1(x),\dots,f_m(x),f_0(x)) \tag{5.44}
$$

的标量化方法（见 §4.7.4）之间有自然的联系。在标量化中，选正向量 $\tilde{\lambda}$ 并最小化标量函数 $\tilde{\lambda}^TF(x)$；任何最小化点保证 Pareto 最优。由于可以用正常数缩放 $\tilde{\lambda}$ 而不影响最小化点，不失一般性取 $\tilde{\lambda}=(\lambda,1)$。于是标量化中最小化的函数是

$$
\tilde{\lambda}^TF(x)=f_0(x)+\sum_{i=1}^m\lambda_if_i(x),
$$

它正是问题 (5.43) 的拉格朗日函数。

为确立「凸多准则问题的每个 Pareto 最优点都对某个非负权重向量 $\tilde{\lambda}$ 最小化 $\tilde{\lambda}^TF(x)$」，我们考虑集合 $\mathcal{A}$（由 (4.62) 定义）：

$$
\mathcal{A}=\{t\in\mathbb{R}^{m+1} \mid \exists x\in\mathcal{D},\ f_i(x)\le t_i,\ i=0,\dots,m\},
$$

它与拉格朗日对偶中出现的集合 $\mathcal{A}$（(5.37) 定义）完全相同。这里也是把所需的权重向量为构造为集合在任意 Pareto 最优点处的支撑超平面。在多准则优化中，我们把权重向量的分量解释为目标函数之间的相对权重。当我们把权重向量与 $f_0$ 相关联的最后一个分量固定为 1 时，其他权重的解释是相对 $f_0$ 的成本，即相对目标的成本。

## <span style="color:#c0392b;">5.4 Saddle-point interpretation（鞍点解释）</span>

<!-- p.251/252 -->

本节给出拉格朗日对偶的几种解释。本节材料后面不会用到。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.4.1 Max-min characterization of weak and strong duality（弱与强对偶的 max-min 刻画）</span>

可以把原始与对偶优化问题表达为更对称的形式。为简化讨论假设没有等式约束；结果容易扩展。先注意

$$
\sup_{\lambda\succeq0}L(x,\lambda)=\sup_{\lambda\succeq0}\Bigl(f_0(x)+\sum_{i=1}^m\lambda_if_i(x)\Bigr)=\begin{cases}f_0(x) & f_i(x)\le0,\ i=1,\dots,m\\ \infty & \text{otherwise}.\end{cases}
$$

<!-- p.252 -->

事实上，设 $x$ 不可行且某 $i$ 有 $f_i(x)>0$，则取 $\lambda_j=0$（$j\neq i$）且 $\lambda_i\to\infty$ 可见 $\sup_{\lambda\succeq0}L(x,\lambda)=\infty$。另一方面若 $f_i(x)\le0$（$i=1,\dots,m$），则 $\lambda$ 的最优选择是 $\lambda=0$，$\sup_{\lambda\succeq0}L(x,\lambda)=f_0(x)$。这意味着可以把原始问题的最优值表达为

$$
p^\star=\inf_x\sup_{\lambda\succeq0}L(x,\lambda).
$$

由对偶函数的定义，也有

$$
d^\star=\sup_{\lambda\succeq0}\inf_xL(x,\lambda).
$$

于是弱对偶性可表达为不等式

$$
\sup_{\lambda\succeq0}\inf_xL(x,\lambda)\le\inf_x\sup_{\lambda\succeq0}L(x,\lambda), \tag{5.45}
$$

强对偶性表达为等式

$$
\sup_{\lambda\succeq0}\inf_xL(x,\lambda)=\inf_x\sup_{\lambda\succeq0}L(x,\lambda).
$$

强对偶性意味着关于 $x$ 的最小化与关于 $\lambda\succeq0$ 的最大化可以交换次序而不影响结果。

事实上，不等式 (5.45) 不依赖 $L$ 的任何性质：对任何 $f:\mathbb{R}^n\times\mathbb{R}^m\to\mathbb{R}$（及任何 $W\subseteq\mathbb{R}^n$、$Z\subseteq\mathbb{R}^m$），有

$$
\sup_{z\in Z}\inf_{w\in W}f(w,z)\le\inf_{w\in W}\sup_{z\in Z}f(w,z). \tag{5.46}
$$

这个一般不等式称为 max-min inequality（max-min 不等式）。当等号成立时，即

$$
\sup_{z\in Z}\inf_{w\in W}f(w,z)=\inf_{w\in W}\sup_{z\in Z}f(w,z), \tag{5.47}
$$

我们说 $f$（与 $W$、$Z$）满足 strong max-min property（强 max-min 性质）或 saddle-point property（鞍点性质）。当然强 max-min 性质只在特殊情形成立，例如当 $f:\mathbb{R}^n\times\mathbb{R}^m\to\mathbb{R}$ 是强对偶性成立的问题的拉格朗日函数、$W=\mathbb{R}^n$、$Z=\mathbb{R}^m_+$。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.4.2 Saddle-point interpretation（鞍点解释）</span>

<!-- p.252/253 -->

我们称数对 $\tilde{w}\in W$、$\tilde{z}\in Z$ 是 $f$（与 $W$、$Z$）的 saddle-point（鞍点），如果

$$
f(\tilde{w},z)\le f(\tilde{w},\tilde{z})\le f(w,\tilde{z})
$$

对所有 $w\in W$、$z\in Z$。换言之，$\tilde{w}$ 最小化 $f(w,\tilde{z})$（在 $w\in W$ 上），$\tilde{z}$ 最大化 $f(\tilde{w},z)$（在 $z\in Z$ 上）：

$$
f(\tilde{w},\tilde{z})=\inf_{w\in W}f(w,\tilde{z}),\qquad f(\tilde{w},\tilde{z})=\sup_{z\in Z}f(\tilde{w},z).
$$

<!-- p.253 -->

这意味着强 max-min 性质 (5.47) 成立，且公共值为 $f(\tilde{w},\tilde{z})$。

回到拉格朗日对偶的讨论：若 $x^\star$ 与 $\lambda^\star$ 是强对偶性成立问题的原始与对偶最优点，则它们构成拉格朗日函数的鞍点。逆命题也成立：若 $(x,\lambda)$ 是拉格朗日函数的鞍点，则 $x$ 原始最优、$\lambda$ 对偶最优、最优对偶间隙为零。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.4.3 Game interpretation（博弈解释）</span>

可以用 continuous zero-sum game（连续零和博弈）解释 max-min 不等式 (5.46)、max-min 等式 (5.47) 与鞍点性质。若第一个玩家选 $w\in W$，第二个玩家选 $z\in Z$，则玩家 1 向玩家 2 支付 $f(w,z)$。玩家 1 因此想最小化 $f$，玩家 2 想最大化 $f$。（博弈称为连续的，因为选择是向量而非离散的。）

设玩家 1 先选，然后玩家 2 在得知玩家 1 的选择后选。玩家 2 想最大化支付 $f(w,z)$，所以会选 $z\in Z$ 最大化 $f(w,z)$。所得支付为 $\sup_{z\in Z}f(w,z)$，依赖第一个玩家的选择 $w$。（这里假设上确界达到；若不然最优支付可任意接近 $\sup_{z\in Z}f(w,z)$。）玩家 1 知道（或假设）玩家 2 会遵循这一策略，所以会选 $w\in W$ 使这个给玩家 2 的最坏情况支付尽量小。于是玩家 1 选

$$
\operatorname*{argmin}_{w\in W}\sup_{z\in Z}f(w,z),
$$

产生玩家 1 向玩家 2 的支付

$$
\inf_{w\in W}\sup_{z\in Z}f(w,z).
$$

现在设出牌次序颠倒：玩家 2 必须先选 $z\in Z$，然后玩家 1（知道 $z$）选 $w\in W$。按类似论证，若玩家遵循最优策略，玩家 2 应选 $z\in Z$ 最大化 $\inf_{w\in W}f(w,z)$，产生支付

$$
\sup_{z\in Z}\inf_{w\in W}f(w,z).
$$

max-min 不等式 (5.46) 陈述（直觉上显然的）事实：后手更好，或更准确地说，玩家在选择前知道对手的选择更好。换言之，若玩家 1 必须先选，玩家 2 的支付更大。当鞍点性质 (5.47) 成立时，后手没有优势。

<!-- p.253/254 -->

若 $(\tilde{w},\tilde{z})$ 是 $f$（与 $W$、$Z$）的鞍点，则它称为博弈的 solution（解）；$\tilde{w}$ 称为玩家 1 的 optimal choice（最优选择）或 strategy（策略），$\tilde{z}$ 称为玩家 2 的最优选择或策略。此情形后手无优势。

<!-- p.254 -->

考虑支付函数是拉格朗日函数、$W=\mathbb{R}^n$、$Z=\mathbb{R}^m_+$ 的特例。此时玩家 1 选择原始变量 $x$，玩家 2 选择对偶变量 $\lambda\succeq0$。按上述论证，玩家 2 若必须先选，其最优选择是任何对偶最优的 $\lambda^\star$，产生支付 $d^\star$；反之若玩家 1 必须先选，其最优选择是任何原始最优的 $x^\star$，产生支付 $p^\star$。

问题的最优对偶间隙恰等于后手玩家获得的优势，即在选择前知道对手选择的优势。若强对偶性成立，则知道对手的选择对玩家没有优势。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.4.4 Price or tax interpretation（价格或税收解释）</span>

拉格朗日对偶有一个有趣的经济解释。设变量 $x$ 表示企业如何运营，$f_0(x)$ 表示在 $x$ 运营的成本，即 $-f_0(x)$ 是在运营条件 $x$ 的 profit（利润，比如以美元计）。每个约束 $f_i(x)\le0$ 表示某种 limit（限制），如资源限制（仓库空间、劳动力）或监管限制（环境）。在尊重限制下最大化利润的运营条件可以通过求解问题

$$
\begin{aligned}
\text{minimize}\quad & f_0(x)\\
\text{subject to}\quad & f_i(x)\le0,\quad i=1,\dots,m
\end{aligned}
$$

找到。最优利润为 $-p^\star$。

现在想象第二种情形：限制可以被违反，但要支付与违反量（由 $f_i$ 度量）成线性的额外成本。于是企业为第 $i$ 个限制或约束支付 $\lambda_if_i(x)$。对不紧的约束企业还获得支付：若 $f_i(x)<0$，则 $\lambda_if_i(x)$ 代表给企业的支付。系数 $\lambda_i$ 的解释是违反 $f_i(x)\le0$ 的 price（价格）；其单位是每单位违反（按 $f_i$ 度量）多少美元。同样的价格下，企业可以出售第 $i$ 个约束的任何「未用」部分。设 $\lambda_i\ge0$，即企业必须为违反付费（约束不紧时获得收入）。

举例：设原问题的第一个约束 $f_1(x)\le0$ 表示仓库空间限制（如平方米）。在这种新安排下，开放企业以每平方米 $\lambda_1$ 美元的价格租用额外仓库空间、并以同样价格出租未用空间的可能性。

<!-- p.254/255 -->

企业在运营条件 $x$、约束价格 $\lambda_i$ 下的总成本是 $L(x,\lambda)=f_0(x)+\sum_{i=1}^m\lambda_if_i(x)$。企业显然会运营得使总成本 $L(x,\lambda)$ 最小，产生成本 $g(\lambda)$。因此对偶函数表示作为约束价格向量 $\lambda$ 的函数的企业最优成本。最优对偶值 $d^\star$ 是企业在最不利价格集下的最优成本。

<!-- p.255 -->

用这个解释可以把弱对偶性改述为：第二种情形（约束违反可以买卖）下企业的最优成本小于等于第一种情形（约束不可违反）下的成本——即使价格最不利。这很显然：若 $x^\star$ 在第一种情形最优，则 $x^\star$ 在第二种情形的运营成本将低于 $f_0(x^\star)$，因为可以从不紧的约束获得一些收入。最优对偶间隙则是「允许企业为约束违反付费（并为不紧约束收款）」能给企业带来的最小可能优势。

现在设强对偶性成立且对偶最优达到。可以把对偶最优的 $\lambda^\star$ 解释为一组价格：在此价格下，允许企业为约束违反付费（或为不紧约束收款）没有优势。因此对偶最优的 $\lambda^\star$ 有时称为原问题的 shadow prices（影子价格）。

---

> <span style="color:#7f8c8d;">§5.1–5.4 翻译完毕。§5.5–5.7 见 boyd_ch05b.bilingual.md，§5.8–5.9 见 boyd_ch05c.bilingual.md。</span>
