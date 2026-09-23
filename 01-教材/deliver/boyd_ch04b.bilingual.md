# <span style="color:#c0392b;">Boyd《凸优化》第 4 章 凸优化问题（中：§4.3–4.5）</span>

> <span style="color:#7f8c8d;">Source: 教材/Convex Optimization (Boyd, Vandenberghe).pdf · Chapter 4 §4.3–4.5, pp.146–166 (PDF p.160–180) · 行内双语翻译版 · 译者：ZCode（bilingual-translator 铁律）</span>

## <span style="color:#c0392b;">4.3 Linear optimization problems（线性优化问题）</span>

<!-- p.160 -->

当目标与约束函数全为仿射时，问题称为 linear program（线性规划，LP）。一般线性规划形如

$$
\begin{aligned}
\text{minimize}\quad & c^Tx+d\\
\text{subject to}\quad & Gx\preceq h\\
& Ax=b,
\end{aligned} \tag{4.27}
$$

其中 $G\in\mathbb{R}^{m\times n}$，$A\in\mathbb{R}^{p\times n}$。线性规划当然是凸优化问题。目标中的常数 $d$ 常被省略，因为它不影响最优（或可行）集。由于可以通过最小化 $-c^Tx-d$（仍是凸的）来最大化仿射目标 $c^Tx+d$，我们也把目标与约束函数都为仿射的最大化问题称为 LP。

LP 的几何解释见图 4.4：LP (4.27) 的可行集是一个多面体 $\mathcal{P}$；问题是在 $\mathcal{P}$ 上最小化仿射函数 $c^Tx+d$（或等价地线性函数 $c^Tx$）。

**标准形式与不等式形式线性规划。** LP (4.27) 的两个特例如此常见以致有专名。standard form LP（标准形式 LP）中唯一的不等式是逐分量非负约束 $x\succeq0$：

$$
\begin{aligned}
\text{minimize}\quad & c^Tx\\
\text{subject to}\quad & Ax=b\\
& x\succeq0.
\end{aligned} \tag{4.28}
$$

<!-- p.161 -->

若 LP 没有等式约束，则称为 inequality form LP（不等式形式 LP），通常写作

$$
\begin{aligned}
\text{minimize}\quad & c^Tx\\
\text{subject to}\quad & Ax\preceq b. \tag{4.29}
\end{aligned}
$$

**把 LP 转换为标准形式。** 有时把一般 LP (4.27) 变换为标准形式 (4.28) 有用（例如为了使用标准形式 LP 的算法）。第一步为不等式引入 slack variables（松弛变量）$s_i$，得到

$$
\begin{aligned}
\text{minimize}\quad & c^Tx+d\\
\text{subject to}\quad & Gx+s=h\\
& Ax=b\\
& s\succeq0.
\end{aligned}
$$

第二步把变量 $x$ 表示为两个非负变量之差：$x=x^+-x^-$，$x^+,x^-\succeq0$。这产生问题

$$
\begin{aligned}
\text{minimize}\quad & c^Tx^+-c^Tx^-+d\\
\text{subject to}\quad & Gx^+-Gx^-+s=h\\
& Ax^+-Ax^-=b\\
& x^+\succeq0,\; x^-\succeq0,\; s\succeq0,
\end{aligned}
$$

它是标准形式 LP，变量为 $x^+,x^-,s$。（此问题与原问题 (4.27) 的等价性见习题 4.10。）

这些（以及例题与习题中的许多其他）操作问题的技巧，可用于把大量问题表述为线性规划。稍微滥用术语，人们常把能表述为 LP 的问题称为 LP，即使它不具有 (4.27) 的形式。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">4.3.1 Examples（例子）</span>

<!-- p.162 -->

LP 出现在大量领域与应用中；这里给几个典型例子。

**膳食问题（Diet problem）。** 健康膳食包含 $m$ 种 nutrients（营养素），数量至少为 $b_1,\dots,b_m$。我们可以通过选择 $n$ 种不同食物的非负数量 $x_1,\dots,x_n$ 来组成这样的膳食。一单位食物 $j$ 含营养素 $i$ 的量为 $a_{ij}$，成本为 $c_j$。我们要确定满足营养要求的最便宜膳食。此问题可表述为 LP：

$$
\begin{aligned}
\text{minimize}\quad & c^Tx\\
\text{subject to}\quad & Ax\succeq b\\
& x\succeq0.
\end{aligned}
$$

这个问题的几种变体也可表述为 LP。例如可以要求膳食中某营养素恰为一定量（给出线性等式约束），或在上界之外再对某营养素加上界。

**多面体的 Chebyshev 中心。** 考虑找位于由线性不等式描述的多面体

$$
\mathcal{P}=\{x\in\mathbb{R}^n \mid a_i^Tx\le b_i,\ i=1,\dots,m\}
$$

内的最大欧氏球的问题。（最优球的中心称为多面体的 Chebyshev center（切比雪夫中心）；它是多面体内最深的点，即离边界最远；见 §8.5.1。）我们把球表示为

$$
\mathcal{B}=\{x_c+u \mid \|u\|_2\le r\}.
$$

问题变量是中心 $x_c\in\mathbb{R}^n$ 与半径 $r$；我们希望在约束 $\mathcal{B}\subseteq\mathcal{P}$ 下最大化 $r$。

<!-- p.162/163 -->

先考虑较简单的约束：$\mathcal{B}$ 位于半空间 $a_i^Tx\le b_i$ 内，即

$$
\|u\|_2\le r \implies a_i^T(x_c+u)\le b_i. \tag{4.30}
$$

由于

$$
\sup\{a_i^Tu \mid \|u\|_2\le r\}=r\|a_i\|_2,
$$

<!-- p.163 -->

可以把 (4.30) 写成

$$
a_i^Tx_c+r\|a_i\|_2\le b_i, \tag{4.31}
$$

这是关于 $x_c$ 与 $r$ 的线性不等式。换言之，「球位于不等式 $a_i^Tx\le b_i$ 确定的半空间内」这一约束可以写成一个线性不等式。因此 $\mathcal{B}\subseteq\mathcal{P}$ 当且仅当 (4.31) 对所有 $i=1,\dots,m$ 成立。于是 Chebyshev 中心可以通过求解 LP

$$
\begin{aligned}
\text{maximize}\quad & r\\
\text{subject to}\quad & a_i^Tx_c+r\|a_i\|_2\le b_i,\quad i=1,\dots,m
\end{aligned}
$$

确定，变量为 $r$ 与 $x_c$。（更多关于 Chebyshev 中心的内容见 §8.5.1。）

**动态活动规划（Dynamic activity planning）。** 考虑在 $N$ 个时间周期上选择（规划）经济中 $n$ 个活动（或部门）的 activity levels（活动水平）的问题。令 $x_j(t)\ge0$（$t=1,\dots,N$）表示部门 $j$ 在周期 $t$ 的活动水平。活动按其活动水平的比例消耗与生产产品或 goods（商品）。每单位活动 $j$ 生产商品 $i$ 的量是 $a_{ij}$；类似地每单位活动 $j$ 消耗商品 $i$ 的量是 $b_{ij}$。周期 $t$ 生产的商品总量是 $Ax(t)\in\mathbb{R}^m$，消耗量是 $Bx(t)\in\mathbb{R}^m$。（虽然我们称这些为「商品」，它们也可包括污染物等 unwanted products（不需要的产品）。）

一周期消耗的商品不能超过上一周期生产的：必须对所有 $t=1,\dots,N$ 有 $Bx(t+1)\preceq Ax(t)$。初始商品向量 $g_0\in\mathbb{R}^m$ 给定，它约束第一周期的活动水平：$Bx(1)\preceq g_0$。活动未消耗的 excess goods（剩余商品）向量为

$$
\begin{aligned}
s(0)&=g_0-Bx(1)\\
s(t)&=Ax(t)-Bx(t+1),\quad t=1,\dots,N-1\\
s(N)&=Ax(N).
\end{aligned}
$$

目标是最大化剩余商品的 discounted total value（贴现总值）：

$$
c^Ts(0)+\gamma c^Ts(1)+\cdots+\gamma^Nc^Ts(N),
$$

其中 $c\in\mathbb{R}^m$ 给出商品的价值，$\gamma>0$ 是 discount factor（贴现因子）。（若第 $i$ 种产品不需要（如污染物），$c_i$ 为负；$|c_i|$ 是单位处置成本。）

<!-- p.163/164 -->

合起来得到 LP：

$$
\begin{aligned}
\text{maximize}\quad & c^Ts(0)+\gamma c^Ts(1)+\cdots+\gamma^Nc^Ts(N)\\
\text{subject to}\quad & x(t)\succeq0, && t=1,\dots,N\\
& s(t)\succeq0, && t=0,\dots,N\\
& s(0)=g_0-Bx(1)\\
& s(t)=Ax(t)-Bx(t+1), && t=1,\dots,N-1\\
& s(N)=Ax(N),
\end{aligned}
$$

变量为 $x(1),\dots,x(N)$，$s(0),\dots,s(N)$。这是一个标准形式 LP；变量 $s(t)$ 是约束 $Bx(t+1)\preceq Ax(t)$ 相关联的松弛变量。

<!-- p.164 -->

**Chebyshev 不等式。** 考虑离散随机变量 $x$ 在 $\{u_1,\dots,u_n\}\subseteq\mathbb{R}$（$n$ 个元素）上的概率分布。我们用向量 $p\in\mathbb{R}^n$ 描述 $x$ 的分布：

$$
p_i=\mathrm{prob}(x=u_i),
$$

所以 $p$ 满足 $p\succeq0$、$\mathbf{1}^Tp=1$。反之，满足这两条的 $p$ 定义 $x$ 的一个概率分布。我们假设 $u_i$ 已知且固定，但分布 $p$ 未知。

若 $f$ 是 $x$ 的任意函数，则

$$
\mathbb{E}\,f=\sum_{i=1}^np_if(u_i)
$$

是 $p$ 的线性函数。若 $S$ 是 $\mathbb{R}$ 的任意子集，则

$$
\mathrm{prob}(x\in S)=\sum_{u_i\in S}p_i
$$

也是 $p$ 的线性函数。

虽然不知道 $p$，但我们有如下形式的先验知识：知道 $x$ 的某些函数的期望的上下界，以及 $\mathbb{R}$ 某些子集的概率。这些先验知识可表示为关于 $p$ 的线性不等式约束：

$$
\alpha_i\le a_i^Tp\le\beta_i,\qquad i=1,\dots,m.
$$

问题是给出 $\mathbb{E}\,f_0(x)=a_0^Tp$ 的上下界（$f_0$ 是 $x$ 的某个函数）。找下界时求解 LP：

$$
\begin{aligned}
\text{minimize}\quad & a_0^Tp\\
\text{subject to}\quad & p\succeq0,\quad \mathbf{1}^Tp=1\\
& \alpha_i\le a_i^Tp\le\beta_i,\quad i=1,\dots,m,
\end{aligned}
$$

变量 $p$。这个 LP 的最优值给出：在所有与先验信息一致的分布中 $\mathbb{E}\,f_0(X)$ 的最低可能值。而且 bound 是 sharp（尖锐）的：最优解给出一个与先验信息一致且达到下界的分布。类似地，可在相同约束下最大化 $a_0^Tp$ 找到最佳上界。（§7.4.1 将更详细地考虑 Chebyshev 不等式。）

<!-- p.164/165 -->

**分段线性最小化。** 考虑（无约束）最小化分段线性凸函数

$$
f(x)=\max_{i=1,\dots,m}(a_i^Tx+b_i)
$$

的问题。可以把它变换为等价 LP：先构造上图问题

$$
\begin{aligned}
\text{minimize}\quad & t\\
\text{subject to}\quad & \max_{i=1,\dots,m}(a_i^Tx+b_i)\le t,
\end{aligned}
$$

<!-- p.165 -->

再把该不等式表示为 $m$ 个独立的不等式：

$$
\begin{aligned}
\text{minimize}\quad & t\\
\text{subject to}\quad & a_i^Tx+b_i\le t,\quad i=1,\dots,m.
\end{aligned}
$$

这是（不等式形式的）LP，变量 $x$ 与 $t$。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">4.3.2 Linear-fractional programming（线性分式规划）</span>

在多面体上最小化仿射函数之比的问题称为 linear-fractional program（线性分式规划）：

$$
\begin{aligned}
\text{minimize}\quad & f_0(x)\\
\text{subject to}\quad & Gx\preceq h\\
& Ax=b
\end{aligned} \tag{4.32}
$$

其中目标函数为

$$
f_0(x)=\frac{c^Tx+d}{e^Tx+f},\qquad \mathbf{dom}\,f_0=\{x \mid e^Tx+f>0\}.
$$

目标函数拟凸（事实上拟线性），所以线性分式规划是拟凸优化问题。

**变换为线性规划。** 若可行集

$$
\{x \mid Gx\preceq h,\ Ax=b,\ e^Tx+f>0\}
$$

非空，则线性分式规划 (4.32) 可以变换为等价的线性规划

$$
\begin{aligned}
\text{minimize}\quad & c^Ty+dz\\
\text{subject to}\quad & Gy-hz\preceq0\\
& Ay-bz=0\\
& e^Ty+fz=1\\
& z\ge0
\end{aligned} \tag{4.33}
$$

变量 $y,z$。为证明等价性，先注意：若 $x$ 对 (4.32) 可行，则数对

$$
y=\frac{x}{e^Tx+f},\qquad z=\frac{1}{e^Tx+f}
$$

对 (4.33) 可行，且目标值相同：$c^Ty+dz=f_0(x)$。由此 (4.32) 的最优值大于等于 (4.33) 的最优值。反之若 $(y,z)$ 对 (4.33) 可行且 $z\neq0$，则 $x=y/z$ 对 (4.32) 可行，目标值相同：$f_0(x)=c^Ty+dz$。若 $(y,z)$ 对 (4.33) 可行且 $z=0$，$x_0$ 对 (4.32) 可行，则 $x=x_0+ty$ 对所有 $t\ge0$ 对 (4.32) 可行；且 $\lim_{t\to\infty}f_0(x_0+ty)=c^Ty+dz$，所以我们可以找到 (4.32) 中目标值任意接近 $(y,z)$ 目标值的可行点。因此 (4.32) 的最优值小于等于 (4.33) 的最优值。

<!-- p.166 -->

**广义线性分式规划。** 线性分式规划 (4.32) 的推广是 generalized linear-fractional program（广义线性分式规划），其中

$$
f_0(x)=\max_{i=1,\dots,r}\frac{c_i^Tx+d_i}{e_i^Tx+f_i},\qquad \mathbf{dom}\,f_0=\{x \mid e_i^Tx+f_i>0,\ i=1,\dots,r\}.
$$

目标函数是 $r$ 个拟凸函数的逐点最大值，因此拟凸，所以这个问题是拟凸的。当 $r=1$ 时退化为标准线性分式规划。

<span style="color:#00838f;">**[例 4.7]** **冯·诺依曼增长问题（Von Neumann growth problem）。** 考虑有 $n$ 个部门的经济体，当期活动水平 $x_i>0$，下期活动水平 $x_i^+>0$。（此问题只考虑一个周期。）有 $m$ 种 goods（商品）被活动消耗也被活动生产：活动水平 $x$ 消耗商品 $Bx\in\mathbb{R}^m$，生产商品 $Ax$。下期消耗的商品不能超过当期生产的商品：$Bx^+\preceq Ax$。部门 $i$ 在该周期内的 growth rate（增长率）为 $x_i^+/x_i$。

冯·诺依曼增长问题是找活动水平向量 $x$ 使经济体所有部门的最小增长率最大化。此问题可表达为广义线性分式问题：

$$
\begin{aligned}
\text{maximize}\quad & \min_{i=1,\dots,n}x_i^+/x_i\\
\text{subject to}\quad & x^+\succeq0\\
& Bx^+\preceq Ax,
\end{aligned}
$$

定义域 $\{(x,x^+) \mid x\succ0\}$。注意此问题关于 $x$ 与 $x^+$ 齐次，所以可以用显式约束 $x\succeq\mathbf{1}$ 替换隐式约束 $x\succ0$。

## <span style="color:#c0392b;">4.4 Quadratic optimization problems（二次优化问题）</span>

<!-- p.166/167 -->

若目标函数是（凸）二次的、约束函数是仿射的，则凸优化问题 (4.15) 称为 quadratic program（二次规划，QP）。二次规划可表达为

$$
\begin{aligned}
\text{minimize}\quad & (1/2)x^TPx+q^Tx+r\\
\text{subject to}\quad & Gx\preceq h\\
& Ax=b,
\end{aligned} \tag{4.34}
$$

其中 $P\in\mathbf{S}^n_+$，$G\in\mathbb{R}^{m\times n}$，$A\in\mathbb{R}^{p\times n}$。二次规划中我们在一个多面体上最小化凸二次函数（图 4.5）。

<!-- p.167 -->

若 (4.15) 的目标与不等式约束函数都是（凸）二次的，如

$$
\begin{aligned}
\text{minimize}\quad & (1/2)x^TP_0x+q_0^Tx+r_0\\
\text{subject to}\quad & (1/2)x^TP_ix+q_i^Tx+r_i\le0,\quad i=1,\dots,m\\
& Ax=b,
\end{aligned} \tag{4.35}
$$

其中 $P_i\in\mathbf{S}^n_+$（$i=0,1,\dots,m$），则问题称为 quadratically constrained quadratic program（二次约束二次规划，QCQP）。QCQP 中我们在一个可行区域（是椭球的交，当 $P_i\succ0$）上最小化凸二次函数。

在 (4.34) 中取 $P=0$，二次规划把线性规划作为特例包含。在 (4.35) 中取 $P_i=0$（$i=1,\dots,m$），QCQP 把二次规划（从而线性规划）作为特例包含。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">4.4.1 Examples（例子）</span>

**最小二乘与回归。** 最小化凸二次函数

$$
\|Ax-b\|_2^2=x^TA^TAx-2b^TAx+b^Tb
$$

的问题是（无约束）QP。它在许多领域出现，有许多名字，如 regression analysis（回归分析）或 least-squares approximation（最小二乘逼近）。这个问题简单到有著名的解析解 $x=A^\dagger b$，其中 $A^\dagger$ 是 $A$ 的伪逆（见 §A.5.4）。

<!-- p.167/168 -->

当加入线性不等式约束后，问题称为 constrained regression（约束回归）或 constrained least-squares（约束最小二乘），不再有简单的解析解。例如带变量上下界的回归：

$$
\begin{aligned}
\text{minimize}\quad & \|Ax-b\|_2^2\\
\text{subject to}\quad & l_i\le x_i\le u_i,\quad i=1,\dots,n,
\end{aligned}
$$

它是一个 QP。（第 6、7 章将更深入地研究最小二乘与回归问题。）

**多面体间的距离。** $\mathbb{R}^n$ 中多面体 $\mathcal{P}_1=\{x \mid A_1x\preceq b_1\}$ 与 $\mathcal{P}_2=\{x \mid A_2x\preceq b_2\}$ 之间的（欧氏）距离定义为

$$
\mathbf{dist}(\mathcal{P}_1,\mathcal{P}_2)=\inf\{\|x_1-x_2\|_2 \mid x_1\in\mathcal{P}_1,\ x_2\in\mathcal{P}_2\}.
$$

若多面体相交，距离为零。为找 $\mathcal{P}_1$ 与 $\mathcal{P}_2$ 之间的距离，可以求解 QP：

$$
\begin{aligned}
\text{minimize}\quad & \|x_1-x_2\|_2^2\\
\text{subject to}\quad & A_1x_1\preceq b_1,\quad A_2x_2\preceq b_2,
\end{aligned}
$$

变量 $x_1,x_2\in\mathbb{R}^n$。这个问题不可行当且仅当某个多面体为空。最优值为零当且仅当多面体相交，此时最优的 $x_1$ 与 $x_2$ 相等（是交 $\mathcal{P}_1\cap\mathcal{P}_2$ 中一点）。否则最优的 $x_1$ 与 $x_2$ 分别是 $\mathcal{P}_1$ 与 $\mathcal{P}_2$ 中彼此最近的点。（第 8 章将更详细地研究涉及距离的几何问题。）

**方差定界。** 再考虑 Chebyshev 不等式的例子（第 150 页）：变量是未知概率分布 $p\in\mathbb{R}^n$，我们对其有一些先验信息。随机变量 $f(x)$ 的方差为

$$
\mathbb{E}\,f^2-(\mathbb{E}\,f)^2=\sum_{i=1}^nf_i^2p_i-\Bigl(\sum_{i=1}^nf_ip_i\Bigr)^2
$$

（$f_i=f(u_i)$），它是 $p$ 的 concave quadratic（凹二次）函数。由此，在给定先验信息下最大化 $f(x)$ 的方差，可以求解 QP：

$$
\begin{aligned}
\text{maximize}\quad & \sum_{i=1}^nf_i^2p_i-\Bigl(\sum_{i=1}^nf_ip_i\Bigr)^2\\
\text{subject to}\quad & p\succeq0,\quad \mathbf{1}^Tp=1\\
& \alpha_i\le a_i^Tp\le\beta_i,\quad i=1,\dots,m.
\end{aligned}
$$

<!-- p.168/169 -->

最优值给出：在所有与先验信息一致的分布上 $f(x)$ 的最大可能方差；最优 $p$ 给出达到此最大方差的分布。

**随机成本线性规划。** 考虑 LP

$$
\begin{aligned}
\text{minimize}\quad & c^Tx\\
\text{subject to}\quad & Gx\preceq h\\
& Ax=b,
\end{aligned}
$$

变量 $x\in\mathbb{R}^n$。假设 cost function（成本向量）$c\in\mathbb{R}^n$ 是随机的，均值 $\bar{c}$，协方差 $\mathbb{E}(c-\bar{c})(c-\bar{c})^T=\Sigma$。（为简化叙述，假设其他问题参数是确定性的。）对给定的 $x\in\mathbb{R}^n$，成本 $c^Tx$ 是（标量）随机变量，均值 $\mathbb{E}\,c^Tx=\bar{c}^Tx$，方差

$$
\mathbf{var}(c^Tx)=\mathbb{E}(c^Tx-\mathbb{E}\,c^Tx)^2=x^T\Sigma x.
$$

一般在小期望成本与小成本方差之间存在 trade-off（权衡）。把方差纳入考虑的一种方式是最小化期望与方差的线性组合：

$$
\mathbb{E}\,c^Tx+\gamma\,\mathbf{var}(c^Tx),
$$

称为 risk-sensitive cost（风险敏感成本）。参数 $\gamma\ge0$ 称为 risk-aversion parameter（风险厌恶参数），因为它设定成本方差与期望值的相对权重。（$\gamma>0$ 时，我们愿意用期望成本的增加换取成本方差的足够大的减小。）为最小化风险敏感成本，求解 QP：

$$
\begin{aligned}
\text{minimize}\quad & \bar{c}^Tx+\gamma x^T\Sigma x\\
\text{subject to}\quad & Gx\preceq h\\
& Ax=b.
\end{aligned}
$$

<!-- p.169/170 -->

**Markowitz 投资组合优化。** 考虑经典的组合问题：在某时段持有 $n$ 种 assets（资产）或 stocks（股票）。令 $x_i$ 表示整个时段持有的资产 $i$ 的数量（以美元计，按周期开始时的价格）。资产 $i$ 的正常 long position（多头头寸）对应 $x_i>0$；short position（空头头寸，即期末买回该资产的义务）对应 $x_i<0$。令 $p_i$ 表示资产 $i$ 在该时段的 relative price change（相对价格变化），即其价格变化除以期初价格。组合的整体回报为 $r=p^Tx$（美元）。优化变量是 portfolio vector（组合向量）$x\in\mathbb{R}^n$。

可以对组合考虑多种约束。最简单的约束集是 $x_i\ge0$（即无空头头寸）与 $\mathbf{1}^Tx=B$（即总投资预算为 $B$，常取 1）。

我们对价格变化取随机模型：$p\in\mathbb{R}^n$ 是随机向量，均值 $\bar{p}$，协方差 $\Sigma$。因此组合 $x\in\mathbb{R}^n$ 下，回报 $r$ 是（标量）随机变量，均值 $\bar{p}^Tx$，方差 $x^T\Sigma x$。组合 $x$ 的选择涉及回报的 mean（均值）与 variance（方差）之间的权衡。Markowitz 引入的经典组合优化问题是 QP：

$$
\begin{aligned}
\text{minimize}\quad & x^T\Sigma x\\
\text{subject to}\quad & \bar{p}^Tx\ge r_{\min}\\
& \mathbf{1}^Tx=1,\quad x\succeq0,
\end{aligned}
$$

变量为组合 $x$。这里我们在达到最小可接受平均回报 $r_{\min}$、并满足组合预算与禁空约束的条件下，最小化回报方差（与组合风险相关联）。

<!-- p.170 -->

许多推广是可能的。例如一个标准推广是允许空头头寸（$x_i<0$）。为此引入变量 $x_{\mathrm{long}}$ 与 $x_{\mathrm{short}}$：

$$
x_{\mathrm{long}}\succeq0,\qquad x_{\mathrm{short}}\succeq0,\qquad x=x_{\mathrm{long}}-x_{\mathrm{short}},\qquad \mathbf{1}^Tx_{\mathrm{short}}\le\eta\,\mathbf{1}^Tx_{\mathrm{long}}.
$$

最后的约束把期初的总空头头寸限制为期初总多头头寸的某个比例 $\eta$。

另一个推广是在组合优化问题中加入 linear transaction costs（线性交易成本）。从给定的初始组合 $x_{\mathrm{init}}$ 出发，买卖资产以达到组合 $x$（然后如上持有整个时段）。买卖资产被收取与买卖量成正比的 transaction fee（交易费）。为处理这一点，引入变量 $u_{\mathrm{buy}}$ 与 $u_{\mathrm{sell}}$，确定持有期前每种资产的买卖量。约束为

$$
x=x_{\mathrm{init}}+u_{\mathrm{buy}}-u_{\mathrm{sell}},\qquad u_{\mathrm{buy}}\succeq0,\qquad u_{\mathrm{sell}}\succeq0.
$$

把简单预算约束 $\mathbf{1}^Tx=1$ 替换为「初始买卖（含交易费）净现金流为零」的条件：

$$
(1-f_{\mathrm{sell}})\mathbf{1}^Tu_{\mathrm{sell}}=(1+f_{\mathrm{buy}})\mathbf{1}^Tu_{\mathrm{buy}}.
$$

左边是卖出资产的总收益减卖出交易费，右边是买入资产的总成本（含交易费）。常数 $f_{\mathrm{buy}}\ge0$、$f_{\mathrm{sell}}\ge0$ 是买卖的交易费率（为简单假设各资产相同）。在最小平均回报、预算与交易约束下最小化回报方差的问题是一个 QP，变量 $x,u_{\mathrm{buy}},u_{\mathrm{sell}}$。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">4.4.2 Second-order cone programming（二阶锥规划）</span>

<!-- p.170/171 -->

与二次规划密切相关的问题是 second-order cone program（二阶锥规划，SOCP）：

$$
\begin{aligned}
\text{minimize}\quad & f^Tx\\
\text{subject to}\quad & \|A_ix+b_i\|_2\le c_i^Tx+d_i,\quad i=1,\dots,m\\
& Fx=g,
\end{aligned} \tag{4.36}
$$

其中 $x\in\mathbb{R}^n$ 是优化变量，$A_i\in\mathbb{R}^{n_i\times n}$，$F\in\mathbb{R}^{p\times n}$。形如

$$
\|Ax+b\|_2\le c^Tx+d
$$

（$A\in\mathbb{R}^{k\times n}$）的约束称为 second-order cone constraint（二阶锥约束），因为它等价于要求仿射函数 $(Ax+b,\ c^Tx+d)$ 位于 $\mathbb{R}^{k+1}$ 中的二阶锥内。

当 $c_i=0$（$i=1,\dots,m$）时，SOCP (4.36) 等价于一个 QCQP（把每个约束平方得到）。类似地若 $A_i=0$（$i=1,\dots,m$），则 SOCP (4.36) 退化为（一般）LP。但二阶锥规划比 QCQP（当然也比 LP）更一般。

<!-- p.171 -->

**鲁棒线性规划（Robust linear programming）。** 考虑不等式形式的 LP

$$
\begin{aligned}
\text{minimize}\quad & c^Tx\\
\text{subject to}\quad & a_i^Tx\le b_i,\quad i=1,\dots,m,
\end{aligned}
$$

其中参数 $c,a_i,b_i$ 有某些 uncertainty（不确定性）或 variation（变化）。为简化叙述，假设 $c$ 与 $b_i$ 固定，$a_i$ 已知位于给定的椭球内：

$$
a_i\in\mathcal{E}_i=\{\bar{a}_i+P_iu \mid \|u\|_2\le1\},
$$

其中 $P_i\in\mathbb{R}^{n\times n}$。（若 $P_i$ 奇异，得到维数为 $\mathbf{rank}\,P_i$ 的「扁平」椭球；$P_i=0$ 意味着 $a_i$ 完全已知。）

我们要求约束对所有可能的参数值 $a_i$ 都满足，这导出 robust linear program（鲁棒线性规划）：

$$
\begin{aligned}
\text{minimize}\quad & c^Tx\\
\text{subject to}\quad & a_i^Tx\le b_i \text{ 对所有 } a_i\in\mathcal{E}_i,\ i=1,\dots,m. \tag{4.37}
\end{aligned}
$$

鲁棒线性约束「$a_i^Tx\le b_i$ 对所有 $a_i\in\mathcal{E}_i$」可以表示为

$$
\sup\{a_i^Tx \mid a_i\in\mathcal{E}_i\}\le b_i,
$$

其左边可表达为

$$
\sup\{a_i^Tx \mid a_i\in\mathcal{E}_i\}=\bar{a}_i^Tx+\sup\{u^TP_i^Tx \mid \|u\|_2\le1\}=\bar{a}_i^Tx+\|P_i^Tx\|_2.
$$

因此鲁棒线性约束可表达为

$$
\bar{a}_i^Tx+\|P_i^Tx\|_2\le b_i,
$$

显然这是一个二阶锥约束。于是鲁棒 LP (4.37) 可以表示为 SOCP：

$$
\begin{aligned}
\text{minimize}\quad & c^Tx\\
\text{subject to}\quad & \bar{a}_i^Tx+\|P_i^Tx\|_2\le b_i,\quad i=1,\dots,m.
\end{aligned}
$$

注意额外的范数项起到 regularization terms（正则化项）的作用：它们防止 $x$ 在参数 $a_i$ 有相当大不确定性的方向上取值过大。

<!-- p.171/172 -->

**带随机约束的线性规划。** 上面的鲁棒 LP 也可以在统计框架下考虑。这里假设参数 $a_i$ 是独立的高斯随机向量，均值 $\bar{a}_i$，协方差 $\Sigma_i$。我们要求每个约束 $a_i^Tx\le b_i$ 以超过 $\eta$（$\eta\ge0.5$）的概率（或 confidence（置信度））成立：

$$
\mathrm{prob}(a_i^Tx\le b_i)\ge\eta. \tag{4.38}
$$

<!-- p.172 -->

我们将证明这个 probability constraint（概率约束）可以表示为二阶锥约束。令 $u=a_i^Tx$，$\sigma^2$ 表示其方差，该约束可写成

$$
\mathrm{prob}\left(\frac{u-\bar{u}}{\sigma}\le\frac{b_i-\bar{u}}{\sigma}\right)\ge\eta.
$$

由于 $(u-\bar{u})/\sigma$ 是零均值单位方差的高斯变量，上述概率就是 $\Phi((b_i-\bar{u})/\sigma)$，其中

$$
\Phi(z)=\frac{1}{\sqrt{2\pi}}\int_{-\infty}^ze^{-t^2/2}\,dt
$$

是零均值单位方差高斯随机变量的累积分布函数。因此概率约束 (4.38) 可表示为

$$
\frac{b_i-\bar{u}}{\sigma}\ge\Phi^{-1}(\eta),
$$

或等价地 $\bar{u}+\Phi^{-1}(\eta)\sigma\le b_i$。由 $u=\bar{a}_i^Tx$ 与 $\sigma=(x^T\Sigma_ix)^{1/2}$ 得

$$
\bar{a}_i^Tx+\Phi^{-1}(\eta)\|\Sigma_i^{1/2}x\|_2\le b_i.
$$

由 $\eta\ge1/2$ 的假设，$\Phi^{-1}(\eta)\ge0$，所以这个约束是二阶锥约束。总之，问题

$$
\begin{aligned}
\text{minimize}\quad & c^Tx\\
\text{subject to}\quad & \mathrm{prob}(a_i^Tx\le b_i)\ge\eta,\quad i=1,\dots,m
\end{aligned}
$$

可表示为 SOCP：

$$
\begin{aligned}
\text{minimize}\quad & c^Tx\\
\text{subject to}\quad & \bar{a}_i^Tx+\Phi^{-1}(\eta)\|\Sigma_i^{1/2}x\|_2\le b_i,\quad i=1,\dots,m.
\end{aligned}
$$

（第 6 章将更深入地考虑鲁棒凸优化问题。另见习题 4.13、4.28 与 4.59。）

<!-- p.172/173 -->

<span style="color:#00838f;">**[例 4.8]** **带损失风险约束的组合优化。** 再考虑上面描述的经典 Markowitz 组合问题（第 155 页）。这里假设价格变化向量 $p\in\mathbb{R}^n$ 是高斯随机变量，均值 $\bar{p}$，协方差 $\Sigma$。因此回报 $r$ 是高斯随机变量，均值 $\bar{r}=\bar{p}^Tx$，方差 $\sigma_r^2=x^T\Sigma x$。考虑形如

$$
\mathrm{prob}(r\le\alpha)\le\beta \tag{4.39}
$$

的 loss risk constraint（损失风险约束），其中 $\alpha$ 是给定的 unwanted return level（不想要的回报水平，例如大损失），$\beta$ 是给定的最大概率。

与上面鲁棒 LP 的随机解释一样，可以用单位高斯随机变量的累积分布函数 $\Phi$ 表达这个约束。不等式 (4.39) 等价于

$$
\bar{p}^Tx+\Phi^{-1}(\beta)\,\|\Sigma^{1/2}x\|_2\ge\alpha.
$$

只要 $\beta\le1/2$（即 $\Phi^{-1}(\beta)\le0$），这个损失风险约束是二阶锥约束。（若 $\beta>1/2$，损失风险约束关于 $x$ 变为非凸的。）

<!-- p.173 -->

于是，在损失风险有界（$\beta\le1/2$）下最大化期望回报的问题可以表述为带一个二阶锥约束的 SOCP：

$$
\begin{aligned}
\text{maximize}\quad & \bar{p}^Tx\\
\text{subject to}\quad & \bar{p}^Tx+\Phi^{-1}(\beta)\,\|\Sigma^{1/2}x\|_2\ge\alpha\\
& x\succeq0,\quad \mathbf{1}^Tx=1.
\end{aligned}
$$

这个问题有许多推广。例如可以施加多个损失风险约束：

$$
\mathrm{prob}(r\le\alpha_i)\le\beta_i,\qquad i=1,\dots,k
$$

（$\beta_i\le1/2$），表达我们愿意为各种损失水平（$\alpha_i$）接受的风险（$\beta_i$）。

**最小曲面（Minimal surface）。** 考虑可微函数 $f:\mathbb{R}^2\to\mathbb{R}$（$\mathbf{dom}\,f=C$）。其图像的 surface area（曲面面积）为

$$
A=\int_C\sqrt{1+\|\nabla f(x)\|_2^2}\;dx=\int_C\|(\nabla f(x),1)\|_2\,dx,
$$

它是 $f$ 的凸泛函。minimal surface problem（最小曲面问题）是找在若干约束（例如 $f$ 在 $C$ 边界上的给定值）下最小化 $A$ 的函数 $f$。

我们将通过离散化 $f$ 来逼近这个问题。设 $C=[0,1]\times[0,1]$，$f_{ij}$ 表示 $f$ 在点 $(i/K,j/K)$ 的值（$i,j=0,\dots,K$）。用前向差分可得 $f$ 在 $x=(i/K,j/K)$ 处梯度的近似表达式：

$$
\nabla f(x)\approx K\begin{bmatrix}f_{i+1,j}-f_{i,j}\\ f_{i,j+1}-f_{i,j}\end{bmatrix}.
$$

代入图像面积表达式并把积分近似为和，得到图像面积的近似：

$$
A\approx A_{\mathrm{disc}}=\frac{1}{K^2}\sum_{i,j=0}^{K-1}\left\|\begin{bmatrix}K(f_{i+1,j}-f_{i,j})\\ K(f_{i,j+1}-f_{i,j})\\ 1\end{bmatrix}\right\|_2.
$$

<!-- p.173/174 -->

离散化面积近似 $A_{\mathrm{disc}}$ 是 $f_{ij}$ 的凸函数。我们可以对 $f_{ij}$ 考虑各种各样的约束，如其任意元素上的等式或不等式约束（例如边界值），或其 moments（矩）上的约束。作为例子，考虑找左、右边界值固定的最小面积曲面：

$$
\begin{aligned}
\text{minimize}\quad & A_{\mathrm{disc}}\\
\text{subject to}\quad & f_{0j}=l_j,\quad j=0,\dots,K\\
& f_{Kj}=r_j,\quad j=0,\dots,K
\end{aligned} \tag{4.40}
$$

其中 $f_{ij}$（$i,j=0,\dots,K$）是变量，$l_j,r_j$ 是方形左右边界的给定值。

<!-- p.174/175 -->

可以通过引入新变量 $t_{ij}$（$i,j=0,\dots,K-1$）把问题 (4.40) 变换为 SOCP：

$$
\begin{aligned}
\text{minimize}\quad & (1/K^2)\sum_{i,j=0}^{K-1}t_{ij}\\
\text{subject to}\quad & \left\|\begin{bmatrix}K(f_{i+1,j}-f_{i,j})\\ K(f_{i,j+1}-f_{i,j})\\ 1\end{bmatrix}\right\|_2\le t_{ij},\quad i,j=0,\dots,K-1\\
& f_{0j}=l_j,\quad j=0,\dots,K\\
& f_{Kj}=r_j,\quad j=0,\dots,K.
\end{aligned}
$$

## <span style="color:#c0392b;">4.5 Geometric programming（几何规划）</span>

<!-- p.174 -->

本节描述一族在其自然形式下非凸的优化问题。然而，这些问题可以通过变量替换与目标、约束函数的变换，转化为凸优化问题。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">4.5.1 Monomials and posynomials（单项式与正项式）</span>

定义域 $\mathbf{dom}\,f=\mathbb{R}^n_{++}$、形如

$$
f(x)=c\,x_1^{a_1}x_2^{a_2}\cdots x_n^{a_n} \tag{4.41}
$$

（$c>0$，$a_i\in\mathbb{R}$）的函数 $f:\mathbb{R}^n\to\mathbb{R}$ 称为 monomial function（单项式函数），或简称 monomial（单项式）。单项式的 exponents（指数）$a_i$ 可以是任何实数（包括分数或负数），但系数 $c$ 只能为正。（「monomial」一词与代数中的标准定义冲突——那里指数必须是非负整数——但这不应引起混淆。）单项式之和，即形如

$$
f(x)=\sum_{k=1}^Kc_kx_1^{a_{1k}}x_2^{a_{2k}}\cdots x_n^{a_{nk}}
$$

<!-- p.175 -->

（$c_k>0$）的函数，称为 posynomial function（正项式函数，$K$ 项），或简称 posynomial（正项式）。

Posynomials 对加法、乘法与非负缩放封闭。Monomials 对乘法与除法封闭。若 posynomial 乘以 monomial，结果是 posynomial；类似地，posynomial 可以除以 monomial，结果仍是 posynomial。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">4.5.2 Geometric programming（几何规划）</span>

形如

$$
\begin{aligned}
\text{minimize}\quad & f_0(x)\\
\text{subject to}\quad & f_i(x)\le1, && i=1,\dots,m\\
& h_i(x)=1, && i=1,\dots,p
\end{aligned} \tag{4.43}
$$

的优化问题（其中 $f_0,\dots,f_m$ 是 posynomials，$h_1,\dots,h_p$ 是 monomials）称为 geometric program（几何规划，GP）。此问题的定义域是 $\mathcal{D}=\mathbb{R}^n_{++}$；约束 $x\succ0$ 是隐式的。

**几何规划的扩展。** 若干扩展很容易处理。若 $f$ 是 posynomial，$h$ 是 monomial，则约束 $f(x)\le h(x)$ 可以表示为 $f(x)/h(x)\le1$ 来处理（因为 $f/h$ 是 posynomial）。这包括形如 $f(x)\le a$（$f$ 是 posynomial，$a>0$）的约束作为特例。类似地，若 $h_1,h_2$ 都是非零 monomial 函数，则等式约束 $h_1(x)=h_2(x)$ 可表示为 $h_1(x)/h_2(x)=1$ 来处理（因为 $h_1/h_2$ 是 monomial）。可以通过最小化其逆（也是 monomial）来最大化非零 monomial 目标函数。

例如考虑问题

$$
\begin{aligned}
\text{maximize}\quad & x/y\\
\text{subject to}\quad & 2\le x\le3\\
& x^2+3y/z\le\sqrt{y}\\
& x/y=z^2,
\end{aligned}
$$

变量 $x,y,z\in\mathbb{R}$（及隐式约束 $x,y,z>0$）。用上述简单变换，得到等价的标准形式 GP：

$$
\begin{aligned}
\text{minimize}\quad & x^{-1}y\\
\text{subject to}\quad & 2x^{-1}\le1,\quad (1/3)x\le1\\
& x^2y^{-1/2}+3y^{1/2}z^{-1}\le1\\
& xy^{-1}z^{-2}=1.
\end{aligned}
$$

我们把这种容易变换为标准形式 (4.43) 的等价 GP 的问题也称为 GP。（如同把容易变换为 LP 的问题称为 LP 一样。）

<!-- p.176 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">4.5.3 Geometric program in convex form（凸形式的几何规划）</span>

Geometric programs 一般不是凸优化问题，但可以通过变量替换与目标、约束函数的变换转化为凸问题。

我们用变量 $y_i=\log x_i$，即 $x_i=e^{y_i}$。若 $f$ 是 (4.41) 给出的关于 $x$ 的 monomial 函数：

$$
f(x)=c\,x_1^{a_1}x_2^{a_2}\cdots x_n^{a_n},
$$

则

$$
f(x)=f(e^{y_1},\dots,e^{y_n})=c(e^{y_1})^{a_1}\cdots(e^{y_n})^{a_n}=e^{a^Ty+b},
$$

其中 $b=\log c$。变量替换 $y_i=\log x_i$ 把 monomial 函数变成一个 affine function（仿射函数）的指数。类似地，若 $f$ 是 (4.42) 给出的 posynomial：

$$
f(x)=\sum_{k=1}^Kc_kx_1^{a_{1k}}x_2^{a_{2k}}\cdots x_n^{a_{nk}},
$$

则

$$
f(x)=\sum_{k=1}^Ke^{a_k^Ty+b_k},
$$

其中 $a_k=(a_{1k},\dots,a_{nk})$，$b_k=\log c_k$。变量替换后，posynomial 成为仿射函数的指数之和。

几何规划 (4.43) 用新变量 $y$ 可表达为

$$
\begin{aligned}
\text{minimize}\quad & \sum_{k=0}^{K_0}e^{a_{0k}^Ty+b_{0k}}\\
\text{subject to}\quad & \sum_{k=1}^{K_i}e^{a_{ik}^Ty+b_{ik}}\le1,\quad i=1,\dots,m\\
& e^{g_i^Ty+h_i}=1,\quad i=1,\dots,p,
\end{aligned}
$$

其中 $a_{ik}\in\mathbb{R}^n$（$i=0,\dots,m$）包含 posynomial 不等式约束的指数，$g_i\in\mathbb{R}^n$（$i=1,\dots,p$）包含原几何规划的 monomial 等式约束的指数。

<!-- p.176/177 -->

现在对目标与约束函数取对数进行变换，得到问题

$$
\begin{aligned}
\text{minimize}\quad & \tilde{f}_0(y)=\log\Bigl(\sum_{k=0}^{K_0}e^{a_{0k}^Ty+b_{0k}}\Bigr)\\
\text{subject to}\quad & \tilde{f}_i(y)=\log\Bigl(\sum_{k=1}^{K_i}e^{a_{ik}^Ty+b_{ik}}\Bigr)\le0,\quad i=1,\dots,m\\
& \tilde{h}_i(y)=g_i^Ty+h_i=0,\quad i=1,\dots,p.
\end{aligned} \tag{4.44}
$$

由于 $\tilde{f}_i$ 凸、$\tilde{h}_i$ 仿射，这是一个凸优化问题。我们称之为 geometric program in convex form（凸形式的几何规划）。为与原几何规划区分，把 (4.43) 称为 geometric program in posynomial form（正项式形式的几何规划）。

<!-- p.177 -->

注意正项式形式几何规划 (4.43) 与凸形式几何规划 (4.44) 之间的变换不涉及任何 computation（计算）：两个问题的 problem data（问题数据）相同，只是改变了目标与约束函数的形式。若 posynomial 目标与约束函数都只有一项（即是 monomials），则凸形式几何规划 (4.44) 退化为（一般）线性规划。因此可以把几何规划看作线性规划的推广或扩展。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">4.5.4 Examples（例子）</span>

**Frobenius 范数对角缩放。** 考虑矩阵 $M\in\mathbb{R}^{n\times n}$ 及相关联的线性函数 $u\mapsto y=Mu$。假设我们缩放坐标，即换变量 $\tilde{u}=Du$、$\tilde{y}=Dy$，$D$ 为对角阵且 $D_{ii}>0$。新坐标下线性函数为 $\tilde{y}=DMD^{-1}\tilde{u}$。

现在假设要选择缩放使所得矩阵 $DMD^{-1}$ 尽量小。用 Frobenius norm（Frobenius 范数，平方）度量矩阵大小：

$$
\|DMD^{-1}\|_F^2=\mathbf{tr}\bigl((DMD^{-1})^T(DMD^{-1})\bigr)=\sum_{i,j=1}^n(DMD^{-1})^2_{ij}=\sum_{i,j=1}^nM_{ij}^2d_i^2/d_j^2,
$$

其中 $D=\mathbf{diag}(d)$。由于这是关于 $d$ 的 posynomial，选择缩放 $d$ 最小化 Frobenius 范数的问题是一个无约束几何规划：

$$
\text{minimize}\quad \sum_{i,j=1}^nM_{ij}^2d_i^2/d_j^2,
$$

变量 $d$。此几何规划中出现的指数只有 $0,2,-2$。

<!-- p.177/178 -->

**悬臂梁设计。** 考虑 cantilever beam（悬臂梁）的设计：它由 $N$ 个 segment（段）组成，从右到左编号 $1,\dots,N$（图 4.6）。每段长度为单位长度、横截面为宽 $w_i$ 高 $h_i$ 的均匀矩形。竖直载荷（力）$F$ 施加在梁的右端。此载荷使梁 deflection（挠曲，向下），并在每段中 induces stress（诱发应力）。我们假设挠度小、材料线弹性，Young's modulus（杨氏模量）$E$。

<!-- p.178/179 -->

设计变量是 $N$ 段的宽度 $w_i$ 与高度 $h_i$。我们要最小化梁的总体积（与其重量成正比）$w_1h_1+\cdots+w_Nh_N$，并满足一些设计约束：段的宽度与高度的上下界

$$
w_{\min}\le w_i\le w_{\max},\qquad h_{\min}\le h_i\le h_{\max},\quad i=1,\dots,N,
$$

以及 aspect ratios（长宽比）

$$
S_{\min}\le h_i/w_i\le S_{\max}.
$$

此外还有材料最大允许应力与梁端竖直挠度的限制。

先看最大应力约束。段 $i$ 中的最大应力记为 $\sigma_i$，由 $\sigma_i=6iF/(w_ih_i^2)$ 给出。施加约束

$$
\frac{6iF}{w_ih_i^2}\le\sigma_{\max},\quad i=1,\dots,N
$$

保证梁内任何地方应力不超过最大允许值 $\sigma_{\max}$。最后一个约束是梁端竖直挠度（记为 $y_1$）的限制：$y_1\le y_{\max}$。挠度 $y_1$ 可由涉及梁段挠度与斜率的 recursion（递推）求出：

$$
\begin{aligned}
v_i&=12(i-1/2)\frac{F}{Ew_ih_i^3}+v_{i+1},\\
y_i&=6(i-1/3)\frac{F}{Ew_ih_i^3}+v_{i+1}+y_{i+1},
\end{aligned} \tag{4.45}
$$

（$i=N,N-1,\dots,1$），初值 $v_{N+1}=y_{N+1}=0$。此递推中 $y_i$ 是段 $i$ 右端的挠度，$v_i$ 是该点斜率。可以用递推 (4.45) 表明这些挠度与斜率量实际上是变量 $w,h$ 的 posynomial 函数：先注意 $v_{N+1},y_{N+1}$ 为零，因此是 posynomials。再设 $v_{i+1},y_{i+1}$ 是 $w,h$ 的 posynomial 函数，则 (4.45) 左边方程表明 $v_i$ 是一个 monomial 与一个 posynomial（即 $v_{i+1}$）之和，因此是 posynomial；右边方程表明挠度 $y_i$ 是一个 monomial 与两个 posynomial（$v_{i+1},y_{i+1}$）之和，因此是 posynomial。特别地，梁端挠度 $y_1$ 是 posynomial。

<!-- p.179/180 -->

问题于是为

$$
\begin{aligned}
\text{minimize}\quad & \sum_{i=1}^Nw_ih_i\\
\text{subject to}\quad & w_{\min}\le w_i\le w_{\max},\quad i=1,\dots,N\\
& h_{\min}\le h_i\le h_{\max},\quad i=1,\dots,N\\
& S_{\min}\le h_i/w_i\le S_{\max},\quad i=1,\dots,N\\
& 6iF/(w_ih_i^2)\le\sigma_{\max},\quad i=1,\dots,N\\
& y_1\le y_{\max},
\end{aligned} \tag{4.46}
$$

变量 $w,h$。这是一个 GP：目标是 posynomial，所有约束都能表示为 posynomial 不等式。（事实上除了挠度限制（一个复杂的 posynomial 不等式）外，所有约束都能表示为 monomial 不等式。）当段数 $N$ 大时，posynomial $y_1$ 中出现的 monomial 项数大约按 $N^2$ 增长。习题 4.31 探讨了另一种表述：把 $v_1,\dots,v_N$ 与 $y_1,\dots,y_N$ 作为变量引入，并把递推的修改版本作为一组约束。这种表述避免了 monomial 项数的增长。

**借助 Perron-Frobenius 理论最小化谱半径。** 设矩阵 $A\in\mathbb{R}^{n\times n}$ 逐元素非负（$A_{ij}\ge0$，$i,j=1,\dots,n$）且 irreducible（不可约），即矩阵 $(I+A)^{n-1}$ 逐元素为正。Perron-Frobenius theorem（Perron-Frobenius 定理）陈述：$A$ 有一个等于其 spectral radius（谱半径，即特征值模的最大者）的正实特征值 $\lambda_{\mathrm{pf}}$。Perron-Frobenius 特征值 $\lambda_{\mathrm{pf}}$ 决定 $A^k$ 当 $k\to\infty$ 的渐近增长或衰减速率；事实上矩阵 $((1/\lambda_{\mathrm{pf}})A)^k$ 收敛。粗略地说：当 $k\to\infty$，若 $\lambda_{\mathrm{pf}}>1$ 则 $A^k$ 按 $\lambda_{\mathrm{pf}}^k$ 增长，若 $\lambda_{\mathrm{pf}}<1$ 则按 $\lambda_{\mathrm{pf}}^k$ 衰减。

非负矩阵理论的一个基本结果：Perron-Frobenius 特征值由

$$
\lambda_{\mathrm{pf}}=\inf\{\lambda \mid Av\preceq\lambda v \text{ 对某个 } v\succ0\}
$$

给出（且下确界达到）。不等式 $Av\preceq\lambda v$ 可表达为

$$
\sum_{j=1}^nA_{ij}v_j/(\lambda v_i)\le1,\qquad i=1,\dots,n, \tag{4.47}
$$

这是关于变量 $A_{ij},v_i,\lambda$ 的一组 posynomial 不等式。于是条件 $\lambda_{\mathrm{pf}}\le\lambda$ 可以表示为关于 $A,v,\lambda$ 的一组 posynomial 不等式。

<!-- p.180 -->

这使我们能用几何规划求解一些涉及 Perron-Frobenius 特征值的优化问题。设矩阵 $A$ 的元素是某个底层变量 $x\in\mathbb{R}^k$ 的 posynomial 函数。此时不等式 (4.47) 是关于变量 $x\in\mathbb{R}^k$、$v\in\mathbb{R}^n$、$\lambda\in\mathbb{R}$ 的 posynomial 不等式。考虑选择 $x$ 最小化 $A$ 的 Perron-Frobenius 特征值（或谱半径）的问题，可能还有关于 $x$ 的 posynomial 不等式约束：

$$
\begin{aligned}
\text{minimize}\quad & \lambda_{\mathrm{pf}}(A(x))\\
\text{subject to}\quad & f_i(x)\le1,\quad i=1,\dots,p,
\end{aligned}
$$

其中 $f_i$ 是 posynomials。用上面的刻画，可以把这个问题表达为 GP：

$$
\begin{aligned}
\text{minimize}\quad & \lambda\\
\text{subject to}\quad & \sum_{j=1}^nA_{ij}v_j/(\lambda v_i)\le1,\quad i=1,\dots,n\\
& f_i(x)\le1,\quad i=1,\dots,p,
\end{aligned}
$$

变量 $x,v,\lambda$。

作为具体例子，考虑一个简单的细菌种群动力学模型：时间或周期记为 $t=0,1,2,\dots$（小时）。向量 $p(t)\in\mathbb{R}^4_+$ 刻画周期 $t$ 的种群年龄分布：$p_1(t)$ 是 0 到 1 小时龄的总数，$p_2(t)$ 是 1 到 2 小时龄的总数，以此类推。（任意）假设细菌寿命不超过 4 小时。种群随时间传播为 $p(t+1)=Ap(t)$，其中

$$
A=\begin{bmatrix}b_1&b_2&b_3&b_4\\ s_1&0&0&0\\ 0&s_2&0&0\\ 0&0&s_3&0\end{bmatrix}.
$$

这里 $b_i$ 是年龄组 $i$ 细菌的 birth rate（出生率），$s_i$ 是从年龄组 $i$ 进入 $i+1$ 的 survival rate（存活率）。假设 $b_i>0$、$0<s_i<1$，这蕴含 $A$ 不可约。

<!-- p.180/181 -->

$A$ 的 Perron-Frobenius 特征值决定种群的渐近增长或衰减速率。若 $\lambda_{\mathrm{pf}}<1$，种群按 $\lambda_{\mathrm{pf}}^t$ 收敛到零，半衰期为 $-1/\log_2\lambda_{\mathrm{pf}}$ 小时；若 $\lambda_{\mathrm{pf}}>1$，种群按 $\lambda_{\mathrm{pf}}^t$ 几何增长，倍增时间为 $1/\log_2\lambda_{\mathrm{pf}}$ 小时。最小化 $A$ 的谱半径对应寻找种群的最快衰减率（或最慢增长率）。

作为 $A$ 所依赖的底层变量，取 $c_1,c_2$——环境中影响细菌出生率与存活率的两种化学物质的 concentrations（浓度）。把出生率与存活率建模为两种浓度的 monomial 函数：

$$
\begin{aligned}
b_i&=b_i^{\mathrm{nom}}(c_1/c_1^{\mathrm{nom}})^{\alpha_i}(c_2/c_2^{\mathrm{nom}})^{\beta_i},\quad i=1,\dots,4\\
s_i&=s_i^{\mathrm{nom}}(c_1/c_1^{\mathrm{nom}})^{\gamma_i}(c_2/c_2^{\mathrm{nom}})^{\delta_i},\quad i=1,\dots,3.
\end{aligned}
$$

其中 $b_i^{\mathrm{nom}}$ 是 nominal birth rate（标称出生率），$s_i^{\mathrm{nom}}$ 是标称存活率，$c_i^{\mathrm{nom}}$ 是化学物质 $i$ 的标称浓度。常数 $\alpha_i,\beta_i,\gamma_i,\delta_i$ 给出浓度偏离标称值对出生率与存活率的影响。例如 $\alpha_2=-0.3$ 与 $\gamma_1=0.5$ 表示：化学物质 1 的浓度高于标称浓度时，1 到 2 小时龄细菌的出生率下降，0 到 1 小时龄细菌的存活率上升。

<!-- p.181 -->

假设浓度 $c_1,c_2$ 可以通过施药独立地增加或减少（比如在 2 倍范围内），提出的问题是找使种群衰减率最大化（即最小化 $\lambda_{\mathrm{pf}}(A)$）的药物配比。用上述方法，这个问题可以表述为 GP：

$$
\begin{aligned}
\text{minimize}\quad & \lambda\\
\text{subject to}\quad & b_1v_1+b_2v_2+b_3v_3+b_4v_4\le\lambda v_1\\
& s_1v_1\le\lambda v_2\\
& s_2v_2\le\lambda v_3\\
& s_3v_3\le\lambda v_4\\
& 1/2\le c_i/c_i^{\mathrm{nom}}\le2,\quad i=1,2\\
& b_i=b_i^{\mathrm{nom}}(c_1/c_1^{\mathrm{nom}})^{\alpha_i}(c_2/c_2^{\mathrm{nom}})^{\beta_i},\quad i=1,\dots,4\\
& s_i=s_i^{\mathrm{nom}}(c_1/c_1^{\mathrm{nom}})^{\gamma_i}(c_2/c_2^{\mathrm{nom}})^{\delta_i},\quad i=1,\dots,3,
\end{aligned}
$$

变量 $b_i,s_i,c_i,v_i,\lambda$。

> <span style="color:#2471a3;">**[译者注｜第一性原理]** §4.5 的核心魔术是：$y_i=\log x_i$ 之后，monomial 变成仿射函数的指数、posynomial 变成 log-sum-exp（§3.1.5 证明过它凸）。「一个看起来非凸的问题族，通过一个变量替换整体变凸」——这预告了全书最重要的方法论：**凸性不是问题的属性，而是问题表示的属性。**</span>

---

> <span style="color:#7f8c8d;">§4.3–4.5 翻译完毕。§4.6–4.7 见 boyd_ch04c.bilingual.md。</span>
