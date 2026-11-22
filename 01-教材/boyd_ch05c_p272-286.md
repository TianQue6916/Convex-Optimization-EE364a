# <span style="color:#c0392b;">Boyd《凸优化》第 5 章 对偶（下：§5.8–5.9 + 文献）</span>

> <span style="color:#7f8c8d;">Source: 教材/Convex Optimization (Boyd, Vandenberghe).pdf · Chapter 5 §5.8–5.9 + Bibliography, pp.258–271 (PDF p.272–286) · 行内双语翻译版 · 译者：ZCode（bilingual-translator 铁律）· 习题部分（PDF p.287–291 起）暂未翻译</span>

## <span style="color:#c0392b;">5.8 Theorems of alternatives（择一定理）</span>

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.8.1 Weak alternatives via the dual function（借助对偶函数的弱择一）</span>

<!-- p.272 -->

本节把拉格朗日对偶理论应用于判定不等式与等式组

```math
f_i(x)\le0,\quad i=1,\dots,m,\qquad h_i(x)=0,\quad i=1,\dots,p \tag{5.72}
```

的可行性。设不等式组 (5.72) 的定义域 $\mathcal{D}=\bigcap_{i=1}^m\mathbf{dom}\,f_i\cap\bigcap_{i=1}^p\mathbf{dom}\,h_i$ 非空。可以把 (5.72) 看作目标 $f_0=0$ 的标准问题 (5.1)，即

```math
\begin{aligned}
\text{minimize}\quad & 0\\
\text{subject to}\quad & f_i(x)\le0,\quad i=1,\dots,m\\
& h_i(x)=0,\quad i=1,\dots,p.
\end{aligned} \tag{5.73}
```

此问题的最优值为

```math
p^\star=\begin{cases}0 & \text{(5.72) 可行}\\ \infty & \text{(5.72) 不可行}，\end{cases} \tag{5.74}
```

所以求解优化问题 (5.73) 与求解不等式组 (5.72) 是同一回事。

**对偶函数。** 我们给不等式组 (5.72) 关联对偶函数

```math
g(\lambda,\nu)=\inf_{x\in\mathcal{D}}\Bigl(\sum_{i=1}^m\lambda_if_i(x)+\sum_{i=1}^p\nu_ih_i(x)\Bigr),
```

<!-- p.273 -->

它与优化问题 (5.73) 的对偶函数相同。由于 $f_0=0$，对偶函数关于 $(\lambda,\nu)$ 是 positive homogeneous（正齐次）的：对 $\alpha>0$，$g(\alpha\lambda,\alpha\nu)=\alpha g(\lambda,\nu)$。与 (5.73) 相关联的对偶问题是在 $\lambda\succeq0$ 下最大化 $g(\lambda,\nu)$。由于 $g$ 齐次，此对偶问题的最优值为

```math
d^\star=\begin{cases}\infty & \text{「}\lambda\succeq0,\ g(\lambda,\nu)>0\text{」可行}\\ 0 & \text{「}\lambda\succeq0,\ g(\lambda,\nu)>0\text{」不可行}，\end{cases} \tag{5.75}
```

弱对偶性告诉我们 $d^\star\le p^\star$。结合 (5.74) 与 (5.75) 得到：若不等式组

```math
\lambda\succeq0,\qquad g(\lambda,\nu)>0 \tag{5.76}
```

可行（意味着 $d^\star=\infty$），则不等式组 (5.72) 不可行（因为此时 $p^\star=\infty$）。事实上，可以把不等式 (5.76) 的任何解 $(\lambda,\nu)$ 解释为系统 (5.72) 不可行的证明或 certificate（证书）。

可以用原系统的可行性重述这个蕴含：若原不等式组 (5.72) 可行，则不等式组 (5.76) 必不可行。可以把满足 (5.72) 的 $x$ 解释为确立不等式组 (5.76) 不可行的证书。

两个不等式（与等式）组称为 weak alternatives（弱择一的），如果两组中至多一个可行。于是系统 (5.72) 与 (5.76) 是弱择一的。无论不等式 (5.72) 是否凸（即 $f_i$ 凸、$h_i$ 仿射）都如此；而且择一不等式组 (5.76) 总是凸的（$g$ 凹，约束 $\lambda_i\ge0$ 凸）。

**严格不等式。** 我们也可以研究严格不等式组

```math
f_i(x)<0,\quad i=1,\dots,m,\qquad h_i(x)=0,\quad i=1,\dots,p \tag{5.77}
```

的可行性。按非严格不等式组那样定义 $g$，有择一不等式组

```math
\lambda\succeq0,\qquad \lambda\neq0,\qquad g(\lambda,\nu)\ge0. \tag{5.78}
```

可以直接证明 (5.77) 与 (5.78) 是弱择一的。设存在 $\tilde{x}$ 使 $f_i(\tilde{x})<0$、$h_i(\tilde{x})=0$。则对任何 $\lambda\succeq0$、$\lambda\neq0$、$\nu$：

```math
\lambda_1f_1(\tilde{x})+\cdots+\lambda_mf_m(\tilde{x})+\nu_1h_1(\tilde{x})+\cdots+\nu_ph_p(\tilde{x})<0.
```

于是

```math
g(\lambda,\nu)=\inf_{x\in\mathcal{D}}\Bigl(\sum_{i=1}^m\lambda_if_i(x)+\sum_{i=1}^p\nu_ih_i(x)\Bigr)\le\sum_{i=1}^m\lambda_if_i(\tilde{x})+\sum_{i=1}^p\nu_ih_i(\tilde{x})<0.
```

<!-- p.274 -->

因此 (5.77) 的可行性蕴含不存在满足 (5.78) 的 $(\lambda,\nu)$。

于是：可以通过给出系统 (5.78) 的解来证明 (5.77) 不可行；可以通过给出系统 (5.77) 的解来证明 (5.78) 不可行。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.8.2 Strong alternatives（强择一）</span>

当原不等式组凸（$f_i$ 凸、$h_i$ 仿射）且某种约束规格成立时，上述弱择一对成为 strong alternatives（强择一对），即两个择一中恰好一个成立。换言之，每个不等式组可行当且仅当另一个不可行。

本节假设 $f_i$ 凸、$h_i$ 仿射，于是不等式组 (5.72) 可表达为

```math
f_i(x)\le0,\ i=1,\dots,m,\qquad Ax=b,
```

其中 $A\in\mathbb{R}^{p\times n}$。

**严格不等式。** 先研究严格不等式组

```math
f_i(x)<0,\ i=1,\dots,m,\qquad Ax=b, \tag{5.79}
```

及其择一

```math
\lambda\succeq0,\qquad \lambda\neq0,\qquad g(\lambda,\nu)\ge0. \tag{5.80}
```

我们需要一个技术性条件：存在 $x\in\mathbf{relint}\,\mathcal{D}$ 使 $Ax=b$。换言之，我们不仅假设线性等式约束相容，还假设它们在 $\mathbf{relint}\,\mathcal{D}$ 中有解。（常常 $\mathcal{D}=\mathbb{R}^n$，此时条件在等式约束相容时即满足。）在此条件下，不等式组 (5.79) 与 (5.80) 中恰好一个可行。换言之，不等式组 (5.79) 与 (5.80) 是强择一的。

我们将通过考虑相关优化问题来确立这个结果：

```math
\begin{aligned}
\text{minimize}\quad & s\\
\text{subject to}\quad & f_i(x)-s\le0,\quad i=1,\dots,m\\
& Ax=b
\end{aligned} \tag{5.81}
```

变量 $x,s$，定义域 $\mathcal{D}\times\mathbb{R}$。此问题的最优值 $p^\star$ 为负当且仅当严格不等式组 (5.79) 有解。

问题 (5.81) 的拉格朗日对偶函数为

```math
\inf_{x\in\mathcal{D},\,s}\Bigl(s+\sum_{i=1}^m\lambda_i(f_i(x)-s)+\nu^T(Ax-b)\Bigr)=\begin{cases}g(\lambda,\nu) & \mathbf{1}^T\lambda=1\\ -\infty & \text{otherwise},\end{cases}
```

<!-- p.275 -->

因此可以把 (5.81) 的对偶问题表达为

```math
\begin{aligned}
\text{maximize}\quad & g(\lambda,\nu)\\
\text{subject to}\quad & \lambda\succeq0,\quad \mathbf{1}^T\lambda=1.
\end{aligned}
```

现在注意 Slater 条件对问题 (5.81) 成立。由假设存在 $\tilde{x}\in\mathbf{relint}\,\mathcal{D}$ 使 $A\tilde{x}=b$。选任何 $\tilde{s}>\max_if_i(\tilde{x})$ 得点 $(\tilde{x},\tilde{s})$，它对 (5.81) 严格可行。因此 $d^\star=p^\star$，且对偶最优 $d^\star$ 达到。换言之，存在 $(\lambda^\star,\nu^\star)$ 使

```math
g(\lambda^\star,\nu^\star)=p^\star,\qquad \lambda^\star\succeq0,\qquad \mathbf{1}^T\lambda^\star=1. \tag{5.82}
```

现在设严格不等式组 (5.79) 不可行，即 $p^\star\ge0$。则 (5.82) 的 $(\lambda^\star,\nu^\star)$ 满足择一不等式组 (5.80)。类似地，若择一不等式组 (5.80) 可行，则 $d^\star=p^\star\ge0$，这表明严格不等式组 (5.79) 不可行。因此不等式组 (5.79) 与 (5.80) 是强择一的：每个可行当且仅当另一个不可行。

**非严格不等式。** 现在考虑非严格不等式组

```math
f_i(x)\le0,\ i=1,\dots,m,\qquad Ax=b, \tag{5.83}
```

及其择一

```math
\lambda\succeq0,\qquad g(\lambda,\nu)>0. \tag{5.84}
```

我们将证明：在以下条件成立时它们是强择一的——存在 $x\in\mathbf{relint}\,\mathcal{D}$ 使 $Ax=b$，且问题 (5.81) 的最优值 $p^\star$ 达到。例如当 $\mathcal{D}=\mathbb{R}^n$ 且 $\max_if_i(x)\to\infty$（当 $x\to\infty$）时成立。

在这些假设下，与严格情形一样有 $p^\star=d^\star$，且原始与对偶最优值都达到。现在设非严格不等式组 (5.83) 不可行，即 $p^\star>0$。（这里用原始最优值达到的假设。）则 (5.82) 的 $(\lambda^\star,\nu^\star)$ 满足择一不等式组 (5.84)。因此不等式组 (5.83) 与 (5.84) 是强择一的：每个可行当且仅当另一个不可行。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.8.3 Examples（例子）</span>

**线性不等式。** 考虑线性不等式组 $Ax\preceq b$。对偶函数为

```math
g(\lambda)=\inf_x\lambda^T(Ax-b)=\begin{cases}-b^T\lambda & A^T\lambda=0\\ -\infty & \text{otherwise}.\end{cases}
```

因此择一不等式组为

```math
\lambda\succeq0,\qquad A^T\lambda=0,\qquad b^T\lambda<0.
```

<!-- p.276 -->

这些事实上是强择一的。理由：相关问题 (5.81) 中的最优达到，除非无下界。

现在考虑严格线性不等式组 $Ax\prec b$，其强择一系统为

```math
\lambda\succeq0,\qquad \lambda\neq0,\qquad A^T\lambda=0,\qquad b^T\lambda\le0.
```

事实上我们之前在 §2.5.1 遇到过（并证明过）这个结果；见第 50 页的 (2.17) 与 (2.18)。

**椭球的交。** 考虑 $m$ 个椭球，描述为

```math
\mathcal{E}_i=\{x \mid f_i(x)\le0\},\qquad f_i(x)=x^TA_ix+2b_i^Tx+c_i,\ i=1,\dots,m,
```

其中 $A_i\in\mathbf{S}^n_{++}$。我们问：这些椭球的交何时有非空内部？这等价于严格二次不等式组

```math
f_i(x)=x^TA_ix+2b_i^Tx+c_i<0,\qquad i=1,\dots,m \tag{5.85}
```

的可行性。对偶函数 $g$ 为

```math
\begin{aligned}
g(\lambda)&=\inf_x\bigl(x^TA(\lambda)x+2b(\lambda)^Tx+c(\lambda)\bigr)\\
&=\begin{cases}-b(\lambda)^TA(\lambda)^\dagger b(\lambda)+c(\lambda) & A(\lambda)\succeq0,\ b(\lambda)\in\mathcal{R}(A(\lambda))\\ -\infty & \text{otherwise},\end{cases}
\end{aligned}
```

其中

```math
A(\lambda)=\sum_{i=1}^m\lambda_iA_i,\qquad b(\lambda)=\sum_{i=1}^m\lambda_ib_i,\qquad c(\lambda)=\sum_{i=1}^m\lambda_ic_i.
```

注意对 $\lambda\succeq0$、$\lambda\neq0$，有 $A(\lambda)\succ0$，所以可以把对偶函数的表达式简化为

```math
g(\lambda)=-b(\lambda)^TA(\lambda)^{-1}b(\lambda)+c(\lambda).
```

系统 (5.85) 的强择一因此为

```math
\lambda\succeq0,\qquad \lambda\neq0,\qquad -b(\lambda)^TA(\lambda)^{-1}b(\lambda)+c(\lambda)\ge0. \tag{5.86}
```

我们可以给这对强择一简单的几何解释。对任何非零 $\lambda\succeq0$，（可能为空的）椭球

```math
\mathcal{E}_\lambda=\{x \mid x^TA(\lambda)x+2b(\lambda)^Tx+c(\lambda)\le0\}
```

包含 $\mathcal{E}_1\cap\cdots\cap\mathcal{E}_m$，因为 $f_i(x)\le0$ 蕴含 $\sum_{i=1}^m\lambda_if_i(x)\le0$。而 $\mathcal{E}_\lambda$ 内部为空当且仅当

```math
\inf_x\bigl(x^TA(\lambda)x+2b(\lambda)^Tx+c(\lambda)\bigr)=-b(\lambda)^TA(\lambda)^{-1}b(\lambda)+c(\lambda)\ge0.
```

<!-- p.276/277 -->

因此择一系统 (5.86) 意味着 $\mathcal{E}_\lambda$ 内部为空。弱对偶性显然：若 (5.86) 成立，则 $\mathcal{E}_\lambda$ 包含交 $\mathcal{E}_1\cap\cdots\cap\mathcal{E}_m$ 且内部为空，所以自然交的内部为空。强择一陈述（不显然的）事实：若交 $\mathcal{E}_1\cap\cdots\cap\mathcal{E}_m$ 内部为空，则可以构造包含该交且内部为空的椭球 $\mathcal{E}_\lambda$。

<!-- p.277 -->

**Farkas 引理。** 本节描述一对由严格与非严格线性不等式混合而成的强择一，称为 Farkas' lemma（Farkas 引理）：不等式系统

```math
Ax\preceq0,\qquad c^Tx<0, \tag{5.87}
```

（$A\in\mathbb{R}^{m\times n}$，$c\in\mathbb{R}^n$）与等式不等式系统

```math
A^Ty+c=0,\qquad y\succeq0 \tag{5.88}
```

是强择一的。

可以用 LP 对偶直接证明 Farkas 引理。考虑 LP

```math
\begin{aligned}
\text{minimize}\quad & c^Tx\\
\text{subject to}\quad & Ax\preceq0
\end{aligned} \tag{5.89}
```

及其对偶

```math
\begin{aligned}
\text{maximize}\quad & 0\\
\text{subject to}\quad & A^Ty+c=0\\
& y\succeq0. \tag{5.90}
\end{aligned}
```

原始 LP (5.89) 齐次，所以当 (5.87) 不可行时最优值 0、当 (5.87) 可行时最优值 $-\infty$。对偶 LP (5.90) 当 (5.88) 可行时最优值 0、当 (5.88) 不可行时最优值 $-\infty$。由于 $x=0$ 对 (5.89) 可行，可以排除 LP 强对偶性可能失效的那一种情形，所以必有 $p^\star=d^\star$。结合上面的注释，这表明 (5.87) 与 (5.88) 是强择一的。

<span style="color:#00838f;">**[例 5.10]** **无套利价格界（Arbitrage-free bounds on price）。** 考虑 $n$ 种资产，投资期初价格分别为 $p_1,\dots,p_n$。投资期末资产价值为 $v_1,\dots,v_n$。若 $x_1,\dots,x_n$ 表示每种资产的初始投资（$x_j<0$ 表示资产 $j$ 的 short position 空头头寸），初始投资成本为 $p^Tx$，投资最终价值为 $v^Tx$。投资期末的资产价值 $v$ 不确定。假设只有 $m$ 种可能的 scenarios（情景）或 outcomes（结果）。若结果 $i$ 发生，资产最终价值为 $v(i)$，因此投资的总价值为 $v(i)^Tx$。

若存在投资向量 $x$ 使 $p^Tx<0$ 且在所有可能情景下最终价值非负（即 $v(i)^Tx\ge0$，$i=1,\dots,m$），则称存在 arbitrage（套利）。条件 $p^Tx<0$ 意味着接受这个投资组合还有钱拿；条件 $v(i)^Tx\ge0$（$i=1,\dots,m$）意味着无论发生什么结果

<!-- p.277/278 -->

最终价值非负。所以套利对应 guaranteed money-making investment strategy（稳赚不赔的投资策略）。通常假设价格与价值使套利不存在。这意味着不等式系统

```math
Vx\succeq0,\qquad p^Tx<0
```

不可行，其中 $V_{ij}=v(i)_j$。

<!-- p.278 -->

用 Farkas 引理：当且仅当存在 $y$ 使

```math
-V^Ty+p=0,\qquad y\succeq0
```

时无套利。

我们可以用这个无套利价格与价值的刻画求解几个有趣的问题。例如设价值 $V$ 已知，且除最后一个 $p_n$ 外所有价格已知。与无套利假设一致的价格 $p_n$ 的集合是一个区间，可以通过求解一对 LP 找到。LP

```math
\begin{aligned}
\text{minimize}\quad & p_n\\
\text{subject to}\quad & V^Ty=p,\quad y\succeq0
\end{aligned}
```

（变量 $p_n,y$）的最优值给出资产 $n$ 的最小可能无套利价格。同一 LP 改为最大化则给出资产 $n$ 的最大可能价格。若两个值相等（即无套利假设导致资产 $n$ 的唯一价格），我们称 market is complete（市场完全）。例子见习题 5.38。这个方法可以用来给基于其他底层资产最终价值的 derivative（衍生品）或 option（期权）找价格界，即当资产 $n$ 的价值或 payoff（回报）是其他资产价值的函数时。

## <span style="color:#c0392b;">5.9 Generalized inequalities（广义不等式）</span>

<!-- p.278/279 -->

本节考察拉格朗日对偶如何扩展到带广义不等式约束的问题：

```math
\begin{aligned}
\text{minimize}\quad & f_0(x)\\
\text{subject to}\quad & f_i(x)\preceq_{K_i}0,\quad i=1,\dots,m\\
& h_i(x)=0,\quad i=1,\dots,p,
\end{aligned} \tag{5.91}
```

其中 $K_i\subseteq\mathbb{R}^{k_i}$ 是真锥。目前不假设问题 (5.91) 凸。设 (5.91) 的定义域 $\mathcal{D}=\bigcap_{i=0}^m\mathbf{dom}\,f_i\cap\bigcap_{i=1}^p\mathbf{dom}\,h_i$ 非空。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.9.1 The Lagrange dual（拉格朗日对偶）</span>

对 (5.91) 中每个广义不等式 $f_i(x)\preceq_{K_i}0$ 关联拉格朗日乘子向量 $\lambda_i\in\mathbb{R}^{k_i}$，定义相应的拉格朗日函数：

```math
L(x,\lambda,\nu)=f_0(x)+\lambda_1^Tf_1(x)+\cdots+\lambda_m^Tf_m(x)+\nu_1h_1(x)+\cdots+\nu_ph_p(x),
```

其中 $\lambda=(\lambda_1,\dots,\lambda_m)$，$\nu=(\nu_1,\dots,\nu_p)$。对偶函数的定义与标量不等式问题完全一样：

```math
g(\lambda,\nu)=\inf_{x\in\mathcal{D}}L(x,\lambda,\nu)=\inf_{x\in\mathcal{D}}\Bigl(f_0(x)+\sum_{i=1}^m\lambda_i^Tf_i(x)+\sum_{i=1}^p\nu_ih_i(x)\Bigr).
```

由于拉格朗日函数关于对偶变量 $(\lambda,\nu)$ 仿射、对偶函数是拉格朗日函数的逐点下确界，对偶函数凹。

<!-- p.279 -->

与标量不等式问题一样，对偶函数给出原始问题 (5.91) 最优值 $p^\star$ 的下界。对标量不等式问题，要求 $\lambda_i\ge0$；这里对偶变量的非负性要求被替换为条件

```math
\lambda_i\succeq_{K_i^*}0,\qquad i=1,\dots,m,
```

其中 $K_i^*$ 表示 $K_i$ 的对偶锥。换言之，与不等式相关联的拉格朗日乘子必须是 dual nonnegative（对偶非负）的。

弱对偶性由对偶锥的定义立即得出。若 $\lambda_i\succeq_{K_i^*}0$ 且 $f_i(\tilde{x})\preceq_{K_i}0$，则 $\lambda_i^Tf_i(\tilde{x})\le0$。因此对任何原始可行点 $\tilde{x}$ 与任何 $\lambda_i\succeq_{K_i^*}0$：

```math
f_0(\tilde{x})+\sum_{i=1}^m\lambda_i^Tf_i(\tilde{x})+\sum_{i=1}^p\nu_ih_i(\tilde{x})\le f_0(\tilde{x}).
```

对 $\tilde{x}$ 取下确界得 $g(\lambda,\nu)\le p^\star$。

拉格朗日对偶优化问题为

```math
\begin{aligned}
\text{maximize}\quad & g(\lambda,\nu)\\
\text{subject to}\quad & \lambda_i\succeq_{K_i^*}0,\quad i=1,\dots,m.
\end{aligned} \tag{5.92}
```

无论原始问题 (5.91) 是否凸，总有弱对偶性：$d^\star\le p^\star$（$d^\star$ 表示对偶问题 (5.92) 的最优值）。

**Slater 条件与强对偶性。** 可以预期：当原始问题凸且满足适当的约束规格时，强对偶性（$d^\star=p^\star$）成立。例如对问题

```math
\begin{aligned}
\text{minimize}\quad & f_0(x)\\
\text{subject to}\quad & f_i(x)\preceq_{K_i}0,\quad i=1,\dots,m\\
& Ax=b
\end{aligned}
```

（$f_0$ 凸、$f_i$ 是 $K_i$-凸的），广义版 Slater 条件是：存在 $x\in\mathbf{relint}\,\mathcal{D}$ 使 $Ax=b$ 且 $f_i(x)\prec_{K_i}0$（$i=1,\dots,m$）。这个条件蕴含强对偶性（以及对偶最优达到）。

<span style="color:#00838f;">**[例 5.11]** **半定规划的拉格朗日对偶。** 考虑不等式形式的半定规划

```math
\begin{aligned}
\text{minimize}\quad & c^Tx\\
\text{subject to}\quad & x_1F_1+\cdots+x_nF_n+G\preceq0
\end{aligned} \tag{5.93}
```

（$F_1,\dots,F_n,G\in\mathbf{S}^k$）。（$f_1$ 仿射，$K_1$ 是 $\mathbf{S}^k_+$ 半正定锥。）为约束关联对偶变量或乘子 $Z\in\mathbf{S}^k$，拉格朗日函数为

```math
L(x,Z)=c^Tx+\mathbf{tr}\bigl((x_1F_1+\cdots+x_nF_n+G)Z\bigr)=x_1(c_1+\mathbf{tr}(F_1Z))+\cdots+x_n(c_n+\mathbf{tr}(F_nZ))+\mathbf{tr}(GZ),
```

<!-- p.280 -->

关于 $x$ 仿射。对偶函数为

```math
g(Z)=\inf_xL(x,Z)=\begin{cases}\mathbf{tr}(GZ) & \mathbf{tr}(F_iZ)+c_i=0,\ i=1,\dots,n\\ -\infty & \text{otherwise}.\end{cases}
```

对偶问题可表达为

```math
\begin{aligned}
\text{maximize}\quad & \mathbf{tr}(GZ)\\
\text{subject to}\quad & \mathbf{tr}(F_iZ)+c_i=0,\quad i=1,\dots,n\\
& Z\succeq0.
\end{aligned}
```

（用了 $\mathbf{S}^k_+$ 自对偶的事实，即 $(\mathbf{S}^k_+)^*=\mathbf{S}^k_+$；见 §2.6。）若半定规划 (5.93) 严格可行，即存在 $x$ 使 $x_1F_1+\cdots+x_nF_n+G\prec0$，则强对偶性成立。

<span style="color:#00838f;">**[例 5.12]** **标准形式锥规划的拉格朗日对偶。** 考虑锥规划

```math
\begin{aligned}
\text{minimize}\quad & c^Tx\\
\text{subject to}\quad & Ax=b\\
& x\succeq_K0,
\end{aligned}
```

$A\in\mathbb{R}^{m\times n}$、$b\in\mathbb{R}^m$、$K\subseteq\mathbb{R}^n$ 真锥。为等式约束关联乘子 $\nu\in\mathbb{R}^m$、为非负性约束关联乘子 $\lambda\in\mathbb{R}^n$。拉格朗日函数为

```math
L(x,\lambda,\nu)=c^Tx-\lambda^Tx+\nu^T(Ax-b),
```

所以对偶函数为

```math
g(\lambda,\nu)=\inf_xL(x,\lambda,\nu)=\begin{cases}-b^T\nu & A^T\nu-\lambda+c=0\\ -\infty & \text{otherwise}.\end{cases}
```

对偶问题可表达为

```math
\begin{aligned}
\text{maximize}\quad & -b^T\nu\\
\text{subject to}\quad & A^T\nu+c=\lambda\\
& \lambda\succeq_{K^*}0.
\end{aligned}
```

消去 $\lambda$ 并定义 $y=-\nu$，此问题可化简为

```math
\begin{aligned}
\text{maximize}\quad & b^Ty\\
\text{subject to}\quad & A^Ty\preceq_{K^*}c,
\end{aligned}
```

它是涉及对偶广义不等式的不等式形式锥规划。若 Slater 条件成立，即存在 $x\succ_K0$ 使 $Ax=b$，则强对偶性成立。

<!-- p.280/281 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.9.2 Optimality conditions（最优性条件）</span>

§5.5 的最优性条件容易推广到带广义不等式的问题。先推导互补松弛条件。

<!-- p.281 -->

**互补松弛性。** 设原始与对偶最优值相等，并在最优点 $x^\star,\lambda^\star,\nu^\star$ 达到。与 §5.5.2 一样，互补松弛条件直接由等式 $f_0(x^\star)=g(\lambda^\star,\nu^\star)$ 及 $g$ 的定义得出。我们有

```math
f_0(x^\star)=g(\lambda^\star,\nu^\star)\le f_0(x^\star)+\sum_{i=1}^m\lambda_i^{\star T}f_i(x^\star)+\sum_{i=1}^p\nu_i^\star h_i(x^\star)\le f_0(x^\star),
```

因此得出：$x^\star$ 最小化 $L(x,\lambda^\star,\nu^\star)$，且第二行的两个和为零。由于第二个和为零（$x^\star$ 满足等式约束），有 $\sum_{i=1}^m\lambda_i^{\star T}f_i(x^\star)=0$。由于此和中每项非正，得出

```math
\lambda_i^{\star T}f_i(x^\star)=0,\qquad i=1,\dots,m, \tag{5.94}
```

它推广了互补松弛条件 (5.48)。由 (5.94) 可以得出

```math
\lambda_i^\star\succ_{K_i^*}0 \implies f_i(x^\star)=0,\qquad f_i(x^\star)\prec_{K_i}0 \implies \lambda_i^\star=0.
```

不过，与标量不等式问题相比，可能出现 $\lambda_i^\star\neq0$ 且 $f_i(x^\star)\neq0$ 却仍满足 (5.94) 的情况。

**KKT 条件。** 现在加上函数 $f_i,h_i$ 可微的假设，把 §5.5.3 的 KKT 条件推广到带广义不等式的问题。由于 $x^\star$ 最小化 $L(x,\lambda^\star,\nu^\star)$，其关于 $x$ 的梯度在 $x^\star$ 为零：

```math
\nabla f_0(x^\star)+\sum_{i=1}^mDf_i(x^\star)^T\lambda_i^\star+\sum_{i=1}^p\nu_i^\star\nabla h_i(x^\star)=0,
```

其中 $Df_i(x^\star)\in\mathbb{R}^{k_i\times n}$ 是 $f_i$ 在 $x^\star$ 处求值的 derivative（导数）（见 §A.4.1）。于是若强对偶性成立，任何原始最优 $x^\star$ 与任何对偶最优 $(\lambda^\star,\nu^\star)$ 必须满足最优性条件（KKT 条件）：

```math
\begin{aligned}
f_i(x^\star)&\preceq_{K_i}0,&& i=1,\dots,m\\
h_i(x^\star)&=0,&& i=1,\dots,p\\
\lambda_i^\star&\succeq_{K_i^*}0,&& i=1,\dots,m\\
\lambda_i^{\star T}f_i(x^\star)&=0,&& i=1,\dots,m\\
\nabla f_0(x^\star)+\sum_{i=1}^mDf_i(x^\star)^T\lambda_i^\star+\sum_{i=1}^p\nu_i^\star\nabla h_i(x^\star)&=0.
\end{aligned} \tag{5.95}
```

若原始问题凸，逆命题也成立，即条件 (5.95) 是 $x^\star,(\lambda^\star,\nu^\star)$ 最优性的充分条件。

<!-- p.282 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.9.3 Perturbation and sensitivity analysis（扰动与敏感性分析）</span>

§5.6 的结果可以推广到涉及广义不等式的问题。考虑问题的相应扰动版本：

```math
\begin{aligned}
\text{minimize}\quad & f_0(x)\\
\text{subject to}\quad & f_i(x)\preceq_{K_i}u_i,\quad i=1,\dots,m\\
& h_i(x)=v_i,\quad i=1,\dots,p,
\end{aligned}
```

$u_i\in\mathbb{R}^{k_i}$，$v\in\mathbb{R}^p$。定义 $p^\star(u,v)$ 为扰动问题的最优值。与标量不等式情形一样，原问题凸时 $p^\star$ 是凸函数。

设 $(\lambda^\star,\nu^\star)$ 对原（未扰动）问题的对偶最优（假设对偶间隙为零）。则对所有 $u,v$：

```math
p^\star(u,v)\ge p^\star-\sum_{i=1}^m\lambda_i^{\star T}u_i-\nu^{\star T}v,
```

即全局敏感性不等式 (5.57) 的类比。局部敏感性结果也成立：若 $p^\star(u,v)$ 在 $u=0,v=0$ 可微，则最优对偶变量 $\lambda_i^\star$ 满足

```math
\lambda_i^\star=-\nabla_{u_i}p^\star(0,0),
```

即 (5.58) 的类比。

<span style="color:#00838f;">**[例 5.13]** **不等式形式的半定规划。** 考虑不等式形式的半定规划（如例 5.11）。原始问题为

```math
\begin{aligned}
\text{minimize}\quad & c^Tx\\
\text{subject to}\quad & F(x)=x_1F_1+\cdots+x_nF_n+G\preceq0
\end{aligned}
```

（变量 $x\in\mathbb{R}^n$，$F_1,\dots,F_n,G\in\mathbf{S}^k$），对偶问题为

```math
\begin{aligned}
\text{maximize}\quad & \mathbf{tr}(GZ)\\
\text{subject to}\quad & \mathbf{tr}(F_iZ)+c_i=0,\quad i=1,\dots,n\\
& Z\succeq0,
\end{aligned}
```

变量 $Z\in\mathbf{S}^k$。设 $x^\star$ 与 $Z^\star$ 分别是原始与对偶最优、对偶间隙为零。互补松弛条件是 $\mathbf{tr}(F(x^\star)Z^\star)=0$。由于 $F(x^\star)\preceq0$、$Z^\star\succeq0$，可以得出 $F(x^\star)Z^\star=0$。因此互补松弛条件可表达为

```math
\mathcal{R}(F(x^\star))\perp\mathcal{R}(Z^\star),
```

即原始与对偶矩阵的 ranges（值域）正交。

设 $p^\star(U)$ 表示扰动 SDP

```math
\begin{aligned}
\text{minimize}\quad & c^Tx\\
\text{subject to}\quad & F(x)=x_1F_1+\cdots+x_nF_n+G\preceq U
\end{aligned}
```

<!-- p.283 -->

的最优值。则对所有 $U$，$p^\star(U)\ge p^\star-\mathbf{tr}(Z^\star U)$。若 $p^\star(U)$ 在 $U=0$ 可微，则

```math
\nabla p^\star(0)=-Z^\star.
```

这意味着对小的 $U$，扰动 SDP 的最优值非常接近（下界）$p^\star-\mathbf{tr}(Z^\star U)$。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.9.4 Theorems of alternatives（择一定理）</span>

<!-- p.283/284 -->

我们可以推导广义不等式与等式系统

```math
f_i(x)\preceq_{K_i}0,\ i=1,\dots,m,\qquad h_i(x)=0,\ i=1,\dots,p \tag{5.96}
```

的择一定理，其中 $K_i\subseteq\mathbb{R}^{k_i}$ 是真锥。也考虑带严格不等式的系统：

```math
f_i(x)\prec_{K_i}0,\ i=1,\dots,m,\qquad h_i(x)=0,\ i=1,\dots,p. \tag{5.97}
```

设 $\mathcal{D}=\bigcap_{i=0}^m\mathbf{dom}\,f_i\cap\bigcap_{i=1}^p\mathbf{dom}\,h_i$ 非空。

**弱择一。** 给系统 (5.96) 与 (5.97) 关联对偶函数

```math
g(\lambda,\nu)=\inf_{x\in\mathcal{D}}\Bigl(\sum_{i=1}^m\lambda_i^Tf_i(x)+\sum_{i=1}^p\nu_ih_i(x)\Bigr),
```

$\lambda=(\lambda_1,\dots,\lambda_m)$（$\lambda_i\in\mathbb{R}^{k_i}$）、$\nu\in\mathbb{R}^p$。与 (5.76) 类比，我们断言

```math
\lambda_i\succeq_{K_i^\star}0,\ i=1,\dots,m,\qquad g(\lambda,\nu)>0 \tag{5.98}
```

是系统 (5.96) 的弱择一。为验证：设存在满足 (5.96) 的 $x$ 与满足 (5.98) 的 $(\lambda,\nu)$，则得矛盾：

```math
0<g(\lambda,\nu)\le\lambda_1^Tf_1(x)+\cdots+\lambda_m^Tf_m(x)+\nu_1h_1(x)+\cdots+\nu_ph_p(x)\le0.
```

<!-- p.284 -->

因此系统 (5.96) 与 (5.98) 至少一个不可行，即两系统弱择一。

类似地可以证明 (5.97) 与系统

```math
\lambda_i\succeq_{K_i^*}0,\ i=1,\dots,m,\qquad \lambda\neq0,\qquad g(\lambda,\nu)\ge0
```

构成一对弱择一。

**强择一。** 现在设函数 $f_i$ 是 $K_i$-凸的、$h_i$ 仿射。先考虑带严格不等式的系统

```math
f_i(x)\prec_{K_i}0,\ i=1,\dots,m,\qquad Ax=b, \tag{5.99}
```

及其择一

```math
\lambda_i\succeq_{K_i^\star}0,\ i=1,\dots,m,\qquad \lambda\neq0,\qquad g(\lambda,\nu)\ge0. \tag{5.100}
```

我们已经看到 (5.99) 与 (5.100) 是弱择一的。它们也是强择一的，只要下列约束规格成立：存在 $\tilde{x}\in\mathbf{relint}\,\mathcal{D}$ 使 $A\tilde{x}=b$。

为证明，选一组向量 $e_i\succ_{K_i}0$，考虑问题

```math
\begin{aligned}
\text{minimize}\quad & s\\
\text{subject to}\quad & f_i(x)\preceq_{K_i}se_i,\quad i=1,\dots,m\\
& Ax=b
\end{aligned} \tag{5.101}
```

变量 $x$ 与 $s\in\mathbb{R}$。Slater 条件成立：只要 $\tilde{s}$ 足够大，$(\tilde{x},\tilde{s})$ 满足严格不等式 $f_i(\tilde{x})\prec_{K_i}\tilde{s}e_i$。

(5.101) 的对偶为

```math
\begin{aligned}
\text{maximize}\quad & g(\lambda,\nu)\\
\text{subject to}\quad & \lambda_i\succeq_{K_i^*}0,\quad i=1,\dots,m\\
& \sum_{i=1}^me_i^T\lambda_i=1
\end{aligned} \tag{5.102}
```

变量 $\lambda=(\lambda_1,\dots,\lambda_m)$ 与 $\nu$。

现在设系统 (5.99) 不可行。则问题 (5.101) 的最优值非负。由于 Slater 条件满足，强对偶性成立且对偶最优达到。因此存在 $(\tilde{\lambda},\tilde{\nu})$ 满足 (5.102) 的约束且 $g(\tilde{\lambda},\tilde{\nu})\ge0$，即系统 (5.100) 有解。

与标量不等式情形一样，「存在 $x\in\mathbf{relint}\,\mathcal{D}$ 使 $Ax=b$」不足以使非严格不等式系统

```math
f_i(x)\preceq_{K_i}0,\ i=1,\dots,m,\qquad Ax=b
```

与其择一

```math
\lambda_i\succeq_{K_i^\star}0,\ i=1,\dots,m,\qquad g(\lambda,\nu)>0
```

成为强择一。还需要一个附加条件，例如问题 (5.101) 的最优值达到。

<span style="color:#00838f;">**[例 5.14]** **线性矩阵不等式的可行性。** 以下系统是强择一的：

```math
F(x)=x_1F_1+\cdots+x_nF_n+G\prec0
```

（$F_i,G\in\mathbf{S}^k$），与

```math
Z\succeq0,\quad Z\neq0,\quad \mathbf{tr}(GZ)\ge0,\quad \mathbf{tr}(F_iZ)=0,\ i=1,\dots,n
```

（$Z\in\mathbf{S}^k$）。这由一般结果得出：取 $K$ 为半正定锥 $\mathbf{S}^k_+$，且

```math
g(Z)=\inf_x\bigl(\mathbf{tr}(F(x)Z)\bigr)=\begin{cases}\mathbf{tr}(GZ) & \mathbf{tr}(F_iZ)=0,\ i=1,\dots,n\\ -\infty & \text{otherwise}.\end{cases}
```

<!-- p.285 -->

非严格不等式的情形稍复杂，需要对矩阵 $F_i$ 加一个额外假设才有强择一。这样的条件之一是

```math
\sum_{i=1}^nv_iF_i\succeq0 \implies \sum_{i=1}^nv_iF_i=0.
```

若此条件成立，则以下系统是强择一的：

```math
F(x)=x_1F_1+\cdots+x_nF_n+G\preceq0
```

与

```math
Z\succeq0,\quad \mathbf{tr}(GZ)>0,\quad \mathbf{tr}(F_iZ)=0,\ i=1,\dots,n
```

（见习题 5.44）。

<!-- p.286 -->

## <span style="color:#c0392b;">Bibliography（文献注释）</span>

**Lagrange duality（拉格朗日对偶）** 的详细讲解见 Luenberger [Lue69, chapter 8]、Rockafellar [Roc70, part VI]、Whittle [Whi71]、Hiriart-Urruty 与 Lemaréchal [HUL93]、Bertsekas, Nedić 与 Ozdaglar [Ber03]。名称来自拉格朗日为带等式约束的优化问题所创的 multiplier method（乘子法）；见 Courant 与 Hilbert [CH53, chapter IV]。

§5.2.5 中矩阵博弈的 max-min 结果早于线性规划对偶。它由 von Neumann 与 Morgenstern [vNM53, page 153] 借助择一定理证明。第 227 页线性规划的强对偶结果归功于 von Neumann [vN63] 与 Gale, Kuhn 与 Tucker [GKT51]。非凸二次问题 (5.32) 的强对偶性是非线性优化 trust region methods（信赖域方法）文献中的基本结果（Nocedal 与 Wright [NW99, page 78]）。它也与控制理论中的 S-procedure 相关，见附录 §B.1。把 §5.3.2 的强对偶性证明推广到精化 Slater 条件 (5.27)，见 Rockafellar [Roc70, page 277]。

保证鞍点性质 (5.47) 的条件见 Rockafellar [Roc70, part VII] 与 Bertsekas, Nedić 与 Ozdaglar [Ber03, chapter 2]；另见习题 5.25。

KKT 条件以 Karush（其未发表的 1939 年硕士论文由 Kuhn [Kuh76] 总结）、Kuhn 与 Tucker [KT51] 命名。相关的最优性条件也由 John [Joh85] 推导。例 5.2 的 water-filling（注水）算法在信息论与通信中有应用（Cover 与 Thomas [CT91, page 252]）。

Farkas 引理由 Farkas [Far02] 发表。它是线性不等式与等式系统最著名的择一定理，但存在许多变体；见 Mangasarian [Man94, §2.4]。Farkas 引理在资产定价中的应用（例 5.10）由 Bertsimas 与 Tsitsiklis [BT97, page 167] 与 Ross [Ros99] 讨论。

拉格朗日对偶到广义不等式问题的推广见 Isii [Isi64]、Luenberger [Lue69, chapter 8]、Berman [Ber73]、Rockafellar [Roc89, page 47]。它在锥规划语境下的讨论见 Nesterov 与 Nemirovski [NN94, §4.2] 与 Ben-Tal 与 Nemirovski [BTN01, lecture 2]。广义不等式的择一定理由 Ben-Israel [BI69]、Berman 与 Ben-Israel [BBI71]、Craven 与 Kohila [CK77] 研究。Bellman 与 Fan [BF63]、Wolkowicz [Wol81]、Lasserre [Las95] 给出 Farkas 引理到线性矩阵不等式的扩展。

---

> <span style="color:#7f8c8d;">第 5 章正文（§5.1–5.9）与文献注释全部翻译完毕。习题（PDF p.287 起）暂未翻译。**Boyd 前 5 章正文翻译全部完成。**下一步：MacKay 信息论教材 Ch.1–6。</span>
