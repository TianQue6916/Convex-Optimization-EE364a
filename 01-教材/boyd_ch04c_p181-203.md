# <span style="color:#c0392b;">Boyd《凸优化》第 4 章 凸优化问题（下：§4.6–4.7 + 文献）</span>

> <span style="color:#7f8c8d;">Source: 教材/Convex Optimization (Boyd, Vandenberghe).pdf · Chapter 4 §4.6–4.7 + Bibliography, pp.166–188 (PDF p.181–203) · 行内双语翻译版 · 译者：ZCode（bilingual-translator 铁律）· 习题部分（PDF p.203–228）暂未翻译</span>

## <span style="color:#c0392b;">4.6 Generalized inequality constraints（广义不等式约束）</span>

<!-- p.181 -->

标准形式凸优化问题 (4.15) 的一个非常有用的推广：允许不等式约束函数取向量值，并在约束中使用广义不等式：

```math
\begin{aligned}
\text{minimize}\quad & f_0(x)\\
\text{subject to}\quad & f_i(x)\preceq_{K_i}0,\quad i=1,\dots,m\\
& Ax=b,
\end{aligned} \tag{4.48}
```

其中 $f_0:\mathbb{R}^n\to\mathbb{R}$，$K_i\subseteq\mathbb{R}^{k_i}$ 是真锥，$f_i:\mathbb{R}^n\to\mathbb{R}^{k_i}$ 是 $K_i$-凸的。我们称此问题为带 generalized inequality constraints（广义不等式约束）的（标准形式）凸优化问题。问题 (4.15) 是 $K_i=\mathbb{R}_+$（$i=1,\dots,m$）的特例。

普通凸优化问题的许多结果对带广义不等式的问题成立。例如：

- 可行集、任何下水平集、以及最优集都是凸的。
- 问题 (4.48) 的任何局部最优点都是全局最优的。
- §4.2.3 给出的可微 $f_0$ 的最优性条件无需任何改变即成立。

我们还将看到（第 11 章），带广义不等式约束的凸优化问题通常可以像普通凸优化问题一样容易地求解。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">4.6.1 Conic form problems（锥形式问题）</span>

<!-- p.182 -->

带广义不等式的最简单的凸优化问题之一是 conic form problems（锥形式问题，或 cone programs，锥规划），它有线性目标和一个不等式约束函数——仿射的（因此 $K$-凸）：

```math
\begin{aligned}
\text{minimize}\quad & c^Tx\\
\text{subject to}\quad & Fx+g\preceq_K0\\
& Ax=b.
\end{aligned} \tag{4.49}
```

当 $K$ 是非负卦限时，锥形式问题退化为线性规划。可以把锥形式问题看作线性规划的推广：逐分量不等式被替换为广义线性不等式。延续与线性规划的类比，我们称锥形式问题

```math
\begin{aligned}
\text{minimize}\quad & c^Tx\\
\text{subject to}\quad & x\succeq_K0\\
& Ax=b
\end{aligned}
```

为 standard form（标准形式）的锥形式问题；类似地，问题

```math
\begin{aligned}
\text{minimize}\quad & c^Tx\\
\text{subject to}\quad & Fx+g\preceq_K0
\end{aligned}
```

称为 inequality form（不等式形式）的锥形式问题。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">4.6.2 Semidefinite programming（半定规划）</span>

当 $K$ 是 $\mathbf{S}^k_+$（$k\times k$ 半正定矩阵锥）时，相应的锥形式问题称为 semidefinite program（半定规划，SDP），形如

```math
\begin{aligned}
\text{minimize}\quad & c^Tx\\
\text{subject to}\quad & x_1F_1+\cdots+x_nF_n+G\preceq0\\
& Ax=b,
\end{aligned} \tag{4.50}
```

其中 $G,F_1,\dots,F_n\in\mathbf{S}^k$，$A\in\mathbb{R}^{p\times n}$。这里的不等式是一个 linear matrix inequality（线性矩阵不等式，见例 2.10）。

若矩阵 $G,F_1,\dots,F_n$ 都是对角的，则 (4.50) 中的 LMI 等价于 $n$ 个线性不等式，SDP (4.50) 退化为线性规划。

<!-- p.182/183 -->

**标准形式与不等式形式的半定规划。** 沿用与 LP 的类比：standard form SDP（标准形式 SDP）有线性等式约束以及变量 $X\in\mathbf{S}^n$ 的（矩阵）非负性约束：

```math
\begin{aligned}
\text{minimize}\quad & \mathbf{tr}(CX)\\
\text{subject to}\quad & \mathbf{tr}(A_iX)=b_i,\quad i=1,\dots,p\\
& X\succeq0,
\end{aligned} \tag{4.51}
```

其中 $C,A_1,\dots,A_p\in\mathbf{S}^n$。（回忆 $\mathbf{tr}(CX)=\sum_{i,j=1}^nC_{ij}X_{ij}$ 是 $\mathbf{S}^n$ 上一般实值线性函数的形式。）这一形式应与标准形式线性规划 (4.28) 比较：LP 与 SDP 的标准形式都是最小化变量的线性函数，受变量的 $p$ 个线性等式约束与变量的非负性约束。

<!-- p.183 -->

与不等式形式 LP (4.29) 类似的 inequality form SDP（不等式形式 SDP）没有等式约束、只有一个 LMI：

```math
\begin{aligned}
\text{minimize}\quad & c^Tx\\
\text{subject to}\quad & x_1A_1+\cdots+x_nA_n\preceq B,
\end{aligned}
```

变量 $x\in\mathbb{R}^n$，参数 $B,A_1,\dots,A_n\in\mathbf{S}^k$，$c\in\mathbb{R}^n$。

**多个 LMI 与线性不等式。** 人们也常把带线性目标、线性等式与不等式约束、以及若干 LMI 约束的问题

```math
\begin{aligned}
\text{minimize}\quad & c^Tx\\
\text{subject to}\quad & F^{(i)}(x)=x_1F_1^{(i)}+\cdots+x_nF_n^{(i)}+G^{(i)}\preceq0,\quad i=1,\dots,K\\
& Gx\preceq h,\quad Ax=b
\end{aligned}
```

也称为 SDP。通过把各个 LMI 与线性不等式组成一个大的块对角 LMI，这类问题很容易变换为 SDP：

```math
\begin{aligned}
\text{minimize}\quad & c^Tx\\
\text{subject to}\quad & \mathbf{diag}(Gx-h,\ F^{(1)}(x),\dots,F^{(K)}(x))\preceq0\\
& Ax=b.
\end{aligned}
```

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">4.6.3 Examples（例子）</span>

**二阶锥规划。** SOCP (4.36) 可以表示为锥形式问题：

```math
\begin{aligned}
\text{minimize}\quad & c^Tx\\
\text{subject to}\quad & -(A_ix+b_i,\ c_i^Tx+d_i)\preceq_{K_i}0,\quad i=1,\dots,m\\
& Fx=g,
\end{aligned}
```

其中

```math
K_i=\{(y,t)\in\mathbb{R}^{n_i+1} \mid \|y\|_2\le t\},
```

即 $\mathbb{R}^{n_i+1}$ 中的二阶锥。这解释了优化问题 (4.36) 名为 second-order cone program（二阶锥规划）的原因。

<!-- p.183/184 -->

**矩阵范数最小化。** 设 $A(x)=A_0+x_1A_1+\cdots+x_nA_n$（$A_i\in\mathbb{R}^{p\times q}$）。考虑无约束问题

```math
\text{minimize}\quad \|A(x)\|_2,
```

其中 $\|\cdot\|_2$ 表示谱范数（最大奇异值），$x\in\mathbb{R}^n$ 是变量。这是凸问题，因为 $\|A(x)\|_2$ 是 $x$ 的凸函数。利用事实「$\|A\|_2\le s$ 当且仅当 $A^TA\preceq s^2I$（且 $s\ge0$）」，可以把问题表达为

```math
\begin{aligned}
\text{minimize}\quad & s\\
\text{subject to}\quad & A(x)^TA(x)\preceq sI,
\end{aligned}
```

变量 $x$ 与 $s$。由于函数 $A(x)^TA(x)-sI$ 关于 $(x,s)$ 矩阵凸，这是一个带单个 $q\times q$ 矩阵不等式约束的凸优化问题。

我们也可以用单个 $(p+q)\times(p+q)$ 的线性矩阵不等式来表述该问题，利用事实

```math
A^TA\preceq t^2I\ (\text{且 } t\ge0) \iff \begin{bmatrix}tI&A\\A^T&tI\end{bmatrix}\succeq0
```

<!-- p.184 -->

（见 §A.5.5）。这得到 SDP：

```math
\begin{aligned}
\text{minimize}\quad & t\\
\text{subject to}\quad & \begin{bmatrix}tI&A(x)\\A(x)^T&tI\end{bmatrix}\succeq0
\end{aligned}
```

变量 $x,t$。

**矩问题（Moment problems）。** 设 $t$ 是 $\mathbb{R}$ 上的随机变量。期望 $\mathbb{E}\,t^k$（若存在）称为 $t$ 分布的（power）moments（（幂）矩）。以下经典结果给出矩序列的刻画：若 $\mathbb{R}$ 上存在概率分布使 $x_k=\mathbb{E}\,t^k$（$k=0,\dots,2n$），则 $x_0=1$ 且

```math
H(x_0,\dots,x_{2n})=
\begin{bmatrix}
x_0&x_1&x_2&\dots&x_{n-1}&x_n\\
x_1&x_2&x_3&\dots&x_n&x_{n+1}\\
x_2&x_3&x_4&\dots&x_{n+1}&x_{n+2}\\
\vdots&\vdots&\vdots&&\vdots&\vdots\\
x_{n-1}&x_n&x_{n+1}&\dots&x_{2n-2}&x_{2n-1}\\
x_n&x_{n+1}&x_{n+2}&\dots&x_{2n-1}&x_{2n}
\end{bmatrix}\succeq0. \tag{4.52}
```

（矩阵 $H$ 称为与 $x_0,\dots,x_{2n}$ 相关联的 Hankel matrix（Hankel 矩阵）。）这很容易看出：设 $x_i=\mathbb{E}\,t^i$（$i=0,\dots,2n$）是某分布的矩，$y=(y_0,y_1,\dots,y_n)\in\mathbb{R}^{n+1}$，则

```math
y^TH(x_0,\dots,x_{2n})y=\sum_{i,j=0}^ny_iy_j\,\mathbb{E}\,t^{i+j}=\mathbb{E}(y_0+y_1t+\cdots+y_nt^n)^2\ge0.
```

<!-- p.184/185 -->

下面的部分逆命题不那么明显：若 $x_0=1$ 且 $H(x)\succ0$，则 $\mathbb{R}$ 上存在概率分布使 $x_i=\mathbb{E}\,t^i$（$i=0,\dots,2n$）。（证明见习题 2.37。）现在设 $x_0=1$ 且 $H(x)\succeq0$（但可能 $H(x)\not\succ0$），即线性矩阵不等式 (4.52) 成立但可能不严格。此时存在 $\mathbb{R}$ 上的一列分布，其矩收敛到 $x$。总之：「$x_0,\dots,x_{2n}$ 是 $\mathbb{R}$ 上某个分布的矩（或一列分布之矩的极限）」这一条件可以表达为变量 $x$ 的线性矩阵不等式 (4.52) 加上线性等式 $x_0=1$。利用这一点，我们可以把一些涉及矩的有趣问题表述为 SDP。

<!-- p.185 -->

设 $t$ 是 $\mathbb{R}$ 上的随机变量。我们不知道其分布，但知道一些矩的界：

```math
\underline{\mu}_k\le\mathbb{E}\,t^k\le\overline{\mu}_k,\qquad k=1,\dots,2n
```

（这包括知道某些矩的精确值作为特例）。设 $p(t)=c_0+c_1t+\cdots+c_{2n}t^{2n}$ 是给定的多项式。$p(t)$ 的期望关于矩 $\mathbb{E}\,t^i$ 是线性的：

```math
\mathbb{E}\,p(t)=\sum_{i=0}^{2n}c_i\,\mathbb{E}\,t^i=\sum_{i=0}^{2n}c_ix_i.
```

我们可以计算 $\mathbb{E}\,p(t)$ 的上下界——在所有满足给定矩界的概率分布上——通过求解 SDP：

```math
\begin{aligned}
\text{minimize (maximize)}\quad & c_1x_1+\cdots+c_{2n}x_{2n}\\
\text{subject to}\quad & \underline{\mu}_k\le x_k\le\overline{\mu}_k,\quad k=1,\dots,2n\\
& H(1,x_1,\dots,x_{2n})\succeq0,
\end{aligned}
```

变量 $x_1,\dots,x_{2n}$。这给出在所有满足已知矩约束的分布上 $\mathbb{E}\,p(t)$ 的界。这些界是 sharp（尖锐）的：存在一列分布（其矩满足给定矩界），使 $\mathbb{E}\,p(t)$ 收敛到这些 SDP 找到的上下界。

<!-- p.185/186 -->

**用不完整的协方差信息为组合风险定界。** 再次考虑经典 Markowitz 组合问题的设定（见第 155 页）：$n$ 种资产或股票的组合，$x_i$ 表示某投资期持有资产 $i$ 的数量，$p_i$ 表示资产 $i$ 在该期的相对价格变化。组合总价值的变化为 $p^Tx$。价格变化向量 $p$ 建模为随机向量，均值与协方差为

```math
\bar{p}=\mathbb{E}\,p,\qquad \Sigma=\mathbb{E}(p-\bar{p})(p-\bar{p})^T.
```

组合价值变化因此是随机变量，均值 $\bar{p}^Tx$，标准差 $\sigma=(x^T\Sigma x)^{1/2}$。大损失的风险（即组合价值大幅低于期望值的变化）与标准差 $\sigma$ 直接相关并随其增大。因此标准差 $\sigma$（或方差 $\sigma^2$）用作组合风险的度量。

<!-- p.186 -->

经典组合优化问题中，组合 $x$ 是优化变量，我们在最小平均回报等约束下最小化风险。价格变化统计量 $\bar{p}$ 与 $\Sigma$ 是已知的问题参数。在本文考虑的 risk bounding problem（风险定界问题）中，我们把问题反过来：假设组合 $x$ 已知，但关于协方差矩阵 $\Sigma$ 只有部分信息。例如可能有每个元素的上下界：

```math
L_{ij}\le\Sigma_{ij}\le U_{ij},\qquad i,j=1,\dots,n,
```

其中 $L,U$ 给定。现在提出问题：在所有与给定界一致的协方差矩阵上，组合的最大风险是多少？我们定义组合的 worst-case variance（最坏情况方差）为

```math
\sigma_{\mathrm{wc}}^2=\sup\{x^T\Sigma x \mid L_{ij}\le\Sigma_{ij}\le U_{ij},\ i,j=1,\dots,n,\ \Sigma\succeq0\}.
```

我们加上了 $\Sigma\succeq0$（协方差矩阵当然必须满足）。可以通过求解 SDP 找 $\sigma_{\mathrm{wc}}$：

```math
\begin{aligned}
\text{maximize}\quad & x^T\Sigma x\\
\text{subject to}\quad & L_{ij}\le\Sigma_{ij}\le U_{ij},\quad i,j=1,\dots,n\\
& \Sigma\succeq0
\end{aligned}
```

变量 $\Sigma\in\mathbf{S}^n$（问题参数 $x,L,U$）。最优 $\Sigma$ 是与给定元素界一致的最坏协方差矩阵——「最坏」指对（给定的）组合 $x$ 风险最大。可以从 SDP 的最优 $\Sigma$ 容易地构造一个与给定界一致、达到最坏情况方差的 $p$ 的分布：例如取 $p=\bar{p}+\Sigma^{1/2}v$，$v$ 是任何满足 $\mathbb{E}\,v=0$、$\mathbb{E}\,vv^T=I$ 的随机向量。

显然，同样的方法可以确定关于 $\Sigma$ 的任何 convex（凸）先验信息下的 $\sigma_{\mathrm{wc}}$。这里列出一些例子：

<!-- p.186/187 -->

- **某些组合的方差已知。** 可能有等式约束如

```math
u_k^T\Sigma u_k=\sigma_k^2,
```

其中 $u_k$ 与 $\sigma_k$ 给定。这对应先验知识：某些已知组合（由 $u_k$ 给出）具有已知（或估计得非常精确）的方差。
- **包含估计误差的影响。** 若协方差 $\Sigma$ 由经验数据估计，估计方法会给出估计值 $\hat{\Sigma}$ 以及关于估计可靠性的某些信息，如一个 confidence ellipsoid（置信椭球）。这可表达为

```math
C(\Sigma-\hat{\Sigma})\le\alpha,
```

其中 $C$ 是 $\mathbf{S}^n$ 上的正定二次型，常数 $\alpha$ 决定置信水平。
- **因子模型。** 协方差可能形如

```math
\Sigma=F\Sigma_{\mathrm{factor}}F^T+D,
```

其中 $F\in\mathbb{R}^{n\times k}$，$\Sigma_{\mathrm{factor}}\in\mathbf{S}^k$，$D$ 对角。这对应形如 $p=Fz+d$ 的价格变化模型，其中 $z$ 是随机变量（影响价格变化的底层 factors（因子）），$d_i$ 独立（每种资产价格的额外波动）。假设因子已知。由于 $\Sigma$ 与 $\Sigma_{\mathrm{factor}}$、$D$ 线性相关，我们可以对它们施加任何（表示先验信息的）凸约束，仍然能用凸优化计算 $\sigma_{\mathrm{wc}}$。
- **相关系数的信息。** 最简单的情形：$\Sigma$ 的对角元（即每种资产价格的 volatilities（波动率））已知，且价格变化间相关系数的界已知：

```math
l_{ij}\le\rho_{ij}=\frac{\Sigma_{ij}}{\Sigma_{ii}^{1/2}\Sigma_{jj}^{1/2}}\le u_{ij},\qquad i,j=1,\dots,n.
```

由于 $\Sigma_{ii}$ 已知而 $\Sigma_{ij}$（$i\neq j$）未知，这些是线性不等式。

<!-- p.187 -->

**图上最快混合马尔可夫链。** 考虑一个 undirected graph（无向图）：节点 $1,\dots,n$，边集 $\mathcal{E}\subseteq\{1,\dots,n\}\times\{1,\dots,n\}$。$(i,j)\in\mathcal{E}$ 表示节点 $i$ 与 $j$ 有一条边相连。由于图无向，$\mathcal{E}$ 对称：$(i,j)\in\mathcal{E}$ 当且仅当 $(j,i)\in\mathcal{E}$。允许 self-loops（自环），即可以有 $(i,i)\in\mathcal{E}$。

我们定义一个 Markov chain（马尔可夫链），状态 $X(t)\in\{1,\dots,n\}$（$t\in\mathbb{Z}_+$，非负整数集）如下：对每条边 $(i,j)\in\mathcal{E}$ 关联一个概率 $P_{ij}$，即 $X$ 在节点 $i$ 与 $j$ 之间转移的概率。状态转移只能沿边发生：对 $(i,j)\notin\mathcal{E}$ 有 $P_{ij}=0$。与边相关联的概率必须非负，且对每个节点，与该节点相连的链路概率之和（含自环，若有）必须等于 1。

马尔可夫链的 transition probability matrix（转移概率矩阵）为

```math
P_{ij}=\mathrm{prob}(X(t+1)=i \mid X(t)=j),\qquad i,j=1,\dots,n.
```

此矩阵必须满足

```math
P_{ij}\ge0,\ i,j=1,\dots,n;\qquad \mathbf{1}^TP=\mathbf{1}^T,\qquad P=P^T, \tag{4.53}
```

且

```math
P_{ij}=0 \text{ 对 } (i,j)\notin\mathcal{E}. \tag{4.54}
```

<!-- p.188 -->

由于 $P$ 对称且 $\mathbf{1}^TP=\mathbf{1}^T$，得 $P\mathbf{1}=\mathbf{1}$，所以均匀分布 $(1/n)\mathbf{1}$ 是马尔可夫链的一个 equilibrium distribution（平衡分布）。$X(t)$ 的分布向 $(1/n)\mathbf{1}$ 的收敛由 $P$ 的（按模）第二大特征值决定，即由 $r=\max\{\lambda_2,-\lambda_n\}$ 决定，其中

```math
1=\lambda_1\ge\lambda_2\ge\cdots\ge\lambda_n
```

是 $P$ 的特征值。我们称 $r$ 为马尔可夫链的 mixing rate（混合速率）。若 $r=1$，$X(t)$ 的分布未必收敛到 $(1/n)\mathbf{1}$（即马尔可夫链不混合）。当 $r<1$，$X(t)$ 的分布当 $t\to\infty$ 时按 $r^t$ 渐近趋近 $(1/n)\mathbf{1}$。因此 $r$ 越小，马尔可夫链混合越快。

fastest mixing Markov chain problem（最快混合马尔可夫链问题）是找满足约束 (4.53)(4.54) 且最小化 $r$ 的 $P$。（问题数据是图，即 $\mathcal{E}$。）我们将证明此问题可以表述为 SDP。

由于特征值 $\lambda_1=1$ 对应特征向量 $\mathbf{1}$，可以把混合速率表示为 $P$ 限制在子空间 $\mathbf{1}^\perp$ 上的范数：$r=\|QPQ\|_2$，其中 $Q=I-(1/n)\mathbf{1}\mathbf{1}^T$ 是向 $\mathbf{1}^\perp$ 正交投影的矩阵。利用 $P\mathbf{1}=\mathbf{1}$：

```math
r=\|QPQ\|_2=\|(I-(1/n)\mathbf{1}\mathbf{1}^T)P(I-(1/n)\mathbf{1}\mathbf{1}^T)\|_2=\|P-(1/n)\mathbf{1}\mathbf{1}^T\|_2.
```

这表明混合速率 $r$ 是 $P$ 的凸函数，所以最快混合马尔可夫链问题可以表述为凸优化问题：

```math
\begin{aligned}
\text{minimize}\quad & \|P-(1/n)\mathbf{1}\mathbf{1}^T\|_2\\
\text{subject to}\quad & P\mathbf{1}=\mathbf{1}\\
& P_{ij}\ge0,\quad i,j=1,\dots,n\\
& P_{ij}=0 \text{ 对 } (i,j)\notin\mathcal{E},
\end{aligned}
```

变量 $P\in\mathbf{S}^n$。通过引入标量变量 $t$ 约束 $P-(1/n)\mathbf{1}\mathbf{1}^T$ 的范数，可以把问题表达为 SDP：

```math
\begin{aligned}
\text{minimize}\quad & t\\
\text{subject to}\quad & -tI\preceq P-(1/n)\mathbf{1}\mathbf{1}^T\preceq tI\\
& P\mathbf{1}=\mathbf{1}\\
& P_{ij}\ge0,\quad i,j=1,\dots,n\\
& P_{ij}=0 \text{ 对 } (i,j)\notin\mathcal{E}. \tag{4.55}
\end{aligned}
```

> <span style="color:#2471a3;">**[译者注｜跨课程连接]** 最快混合马尔可夫链与随机算法（CLRS 第 5 章的随机化思想、Metropolis 算法）相接：链混合得越快，采样越接近独立。这个 SDP 是「把谱隙（spectral gap）最大化」的凸化范例。</span>

## <span style="color:#c0392b;">4.7 Vector optimization（向量优化）</span>

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">4.7.1 General and convex vector optimization problems（一般与凸向量优化问题）</span>

<!-- p.188/189 -->

§4.6 中我们把标准形式问题 (4.1) 扩展到向量值的约束函数。本节研究向量值目标函数的含义。我们把一般的 vector optimization problem（向量优化问题）记为

```math
\begin{aligned}
\text{minimize (with respect to } K)\quad & f_0(x)\\
\text{subject to}\quad & f_i(x)\le0,\quad i=1,\dots,m\\
& h_i(x)=0,\quad i=1,\dots,p.
\end{aligned} \tag{4.56}
```

这里 $x\in\mathbb{R}^n$ 是优化变量，$K\subseteq\mathbb{R}^q$ 是真锥，$f_0:\mathbb{R}^n\to\mathbb{R}^q$ 是 objective function（目标函数），$f_i:\mathbb{R}^n\to\mathbb{R}$ 是不等式约束函数，$h_i:\mathbb{R}^n\to\mathbb{R}$ 是等式约束函数。此问题与标准优化问题 (4.1) 的唯一差别是：这里目标函数取值于 $\mathbb{R}^q$，且问题规定包含一个真锥 $K$，用于比较目标值。在向量优化语境下，标准优化问题 (4.1) 有时称为 scalar optimization problem（标量优化问题）。

<!-- p.189 -->

我们说向量优化问题 (4.56) 是 convex vector optimization problem（凸向量优化问题），如果目标函数 $f_0$ 是 $K$-凸的，不等式约束函数 $f_1,\dots,f_m$ 凸，等式约束函数 $h_1,\dots,h_p$ 仿射。（与标量情形一样，我们通常把等式约束写作 $Ax=b$，$A\in\mathbb{R}^{p\times n}$。）

我们能给向量优化问题 (4.56) 什么含义？设 $x$ 与 $y$ 是两个可行点（即满足约束）。它们对应的目标值 $f_0(x)$ 与 $f_0(y)$ 要用广义不等式 $\preceq_K$ 比较。我们把 $f_0(x)\preceq_K f_0(y)$ 解释为「按目标 $f_0$（关于 $K$）判断，$x$ 的价值不劣于 $y$」。向量优化令人困惑之处在于：两个目标值 $f_0(x)$ 与 $f_0(y)$ 未必可比较——可能既没有 $f_0(x)\preceq_K f_0(y)$ 也没有 $f_0(y)\preceq_K f_0(x)$，即两者谁也不比谁好。这在标量目标优化问题中不可能发生。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">4.7.2 Optimal points and values（最优点与最优值）</span>

先考虑一个特殊情形，此时向量优化问题的含义是清楚的。考虑可行点的目标值集合

```math
\mathcal{O}=\{f_0(x) \mid \exists x\in\mathcal{D},\ f_i(x)\le0,\ i=1,\dots,m,\ h_i(x)=0,\ i=1,\dots,p\}\subseteq\mathbb{R}^q,
```

称为 set of achievable objective values（可达目标值集合）。若这个集合有 minimum element（最小元）（见 §2.4.2），即存在可行 $x$ 使 $f_0(x)\preceq_K f_0(y)$ 对所有可行 $y$，则称 $x$ 对问题 (4.56) 是 optimal（最优）的，并称 $f_0(x)$ 为问题的 optimal value（最优值）。（当向量优化问题有最优值时，它是唯一的。）若 $x^\star$ 是最优点，则 $f_0(x^\star)$ 可以与每个其他可行点的目标比较，且不劣于它们。粗略地说，$x^\star$ 无疑是可行点中 $x$ 的最佳选择。

<!-- p.189/190 -->

点 $x^\star$ 最优当且仅当它可行且

```math
\mathcal{O}\subseteq f_0(x^\star)+K. \tag{4.57}
```

<!-- p.190 -->

（见 §2.4.2。）集合 $f_0(x^\star)+K$ 可解释为「不优于 $f_0(x^\star)$ 的值」的集合，所以条件 (4.57) 陈述：每个可达值都落入这个集合。（见图 4.7。）大多数向量优化问题没有最优点与最优值，但在某些特殊情形会发生。

<span style="color:#00838f;">**[例 4.9]** **最佳线性无偏估计（Best linear unbiased estimator）。** 设 $y=Ax+v$，$v\in\mathbb{R}^m$ 是测量噪声，$y\in\mathbb{R}^m$ 是测量向量，$x\in\mathbb{R}^n$ 是给定测量 $y$ 时要估计的向量。假设 $A$ 的秩为 $n$，测量噪声满足 $\mathbb{E}\,v=0$、$\mathbb{E}\,vv^T=I$，即其分量零均值且不相关。

$x$ 的线性估计器形如 $\hat{x}=Fy$。估计器称为 unbiased（无偏）的，如果对所有 $x$ 有 $\mathbb{E}\,\hat{x}=x$，即 $FA=I$。无偏估计器的 error covariance（误差协方差）为

```math
\mathbb{E}(\hat{x}-x)(\hat{x}-x)^T=\mathbb{E}\,Fvv^TF^T=FF^T.
```

我们的目标是找误差协方差矩阵「小」的无偏估计器。我们可以用 matrix inequality（矩阵不等式，即关于 $\mathbf{S}^n_+$）比较误差协方差。其解释：设 $\hat{x}_1=F_1y$、$\hat{x}_2=F_2y$ 是两个无偏估计器，则第一个估计器不劣于第二个（$F_1F_1^T\preceq F_2F_2^T$）当且仅当对所有 $c$，

```math
\mathbb{E}(c^T\hat{x}_1-c^Tx)^2\le\mathbb{E}(c^T\hat{x}_2-c^Tx)^2.
```

换言之，对 $x$ 的任何线性函数，估计器 $F_1$ 给出不劣于 $F_2$ 的估计。

<!-- p.191 -->

我们可以把「找 $x$ 的无偏估计器」表达为向量优化问题：

```math
\begin{aligned}
\text{minimize (w.r.t. }\mathbf{S}^n_+)\quad & FF^T\\
\text{subject to}\quad & FA=I,
\end{aligned} \tag{4.58}
```

变量 $F\in\mathbb{R}^{n\times m}$。目标 $FF^T$ 关于 $\mathbf{S}^n_+$ 凸，所以问题 (4.58) 是凸向量优化问题。一种容易的验证：$v^TFF^Tv=\|F^Tv\|_2^2$ 对任何固定的 $v$ 是 $F$ 的凸函数。

一个著名结果是：问题 (4.58) 有最优解——least-squares estimator（最小二乘估计器）或 pseudo-inverse（伪逆）

```math
F^\star=A^\dagger=(A^TA)^{-1}A^T.
```

对任何满足 $FA=I$ 的 $F$，有 $FF^T\succeq F^\star F^{\star T}$。矩阵

```math
F^\star F^{\star T}=A^\dagger A^{\dagger T}=(A^TA)^{-1}
```

是问题 (4.58) 的最优值。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">4.7.3 Pareto optimal points and values（Pareto 最优点与最优值）</span>

<!-- p.191 -->

现在考虑（发生在大多数令人感兴趣的向量优化问题中的）情形：可达目标值集合没有最小元，问题没有最优点或最优值。此时可达值的 minimal elements（极小元）扮演重要角色。我们说可行点 $x$ 是 Pareto optimal（Pareto 最优，或 efficient，有效）的，如果 $f_0(x)$ 是可达值集合 $\mathcal{O}$ 的极小元。此时称 $f_0(x)$ 为向量优化问题 (4.56) 的 Pareto optimal value（Pareto 最优值）。于是：点 $x$ Pareto 最优，指它可行，且对任何可行 $y$，$f_0(y)\preceq_K f_0(x)$ 蕴含 $f_0(y)=f_0(x)$。换言之：任何不劣于 $x$ 的可行点 $y$（即 $f_0(y)\preceq_K f_0(x)$）的目标值与 $x$ 完全相同。

点 $x$ 是 Pareto 最优的当且仅当它可行且

```math
(f_0(x)-K)\cap\mathcal{O}=\{f_0(x)\}. \tag{4.59}
```

<!-- p.191/192 -->

（见 §2.4.2。）集合 $f_0(x)-K$ 可解释为「不劣于 $f_0(x)$ 的值」的集合，所以条件 (4.59) 陈述：可达值中不劣于 $f_0(x)$ 的只有 $f_0(x)$ 本身。（见图 4.8。）

一个向量优化问题可以有许多 Pareto 最优值（与点）。Pareto 最优值的集合记为 $\mathcal{P}$，满足

```math
\mathcal{P}\subseteq\mathcal{O}\cap\mathbf{bd}\,\mathcal{O},
```

即每个 Pareto 最优值都是位于可达目标值集合边界上的可达目标值（见习题 4.52）。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">4.7.4 Scalarization（标量化）</span>

<!-- p.192 -->

Scalarization（标量化）是找向量优化问题的 Pareto 最优（或最优）点的标准技术，基于 §2.6.3 通过对偶广义不等式对最小元与极小元的刻画。选任何 $\lambda\succ_{K^*}0$（即在对偶广义不等式下为正的向量）。考虑标量优化问题

```math
\begin{aligned}
\text{minimize}\quad & \lambda^Tf_0(x)\\
\text{subject to}\quad & f_i(x)\le0,\quad i=1,\dots,m\\
& h_i(x)=0,\quad i=1,\dots,p,
\end{aligned} \tag{4.60}
```

设 $x$ 是其最优点。则 $x$ 对向量优化问题 (4.56) 是 Pareto 最优的。这由 §2.6.3 的极小元对偶不等式刻画得出，也可直接证明：若 $x$ 不是 Pareto 最优，则存在可行 $y$ 满足 $f_0(y)\preceq_K f_0(x)$ 且 $f_0(x)\neq f_0(y)$。由于 $f_0(x)-f_0(y)\succeq_K0$ 且非零，有 $\lambda^T(f_0(x)-f_0(y))>0$，即 $\lambda^Tf_0(x)>\lambda^Tf_0(y)$，与「$x$ 对标量问题 (4.60) 最优」矛盾。

<!-- p.192/193 -->

使用标量化，我们可以通过求解普通标量优化问题 (4.60) 找任何向量优化问题的 Pareto 最优点。向量 $\lambda$ 有时称为 weight vector（权重向量），必须满足 $\lambda\succ_{K^*}0$。权重向量是 free parameter（自由参数）；变化它就得到向量优化问题 (4.56) 的（可能不同的）Pareto 最优解。（见图 4.9。该图也展示了一个任何权重向量 $\lambda\succ_{K^*}0$ 都无法通过标量化得到的 Pareto 最优点的例子。）

<!-- p.193 -->

标量化方法可以几何化解释。点 $x$ 对标量化问题最优（即在可行集上最小化 $\lambda^Tf_0$）当且仅当 $\lambda^T(f_0(y)-f_0(x))\ge0$ 对所有可行 $y$。但这等于说：$\{u \mid -\lambda^T(u-f_0(x))=0\}$ 是可达目标值集合 $\mathcal{O}$ 在点 $f_0(x)$ 处的支撑超平面；特别地

```math
\{u \mid \lambda^T(u-f_0(x))<0\}\cap\mathcal{O}=\varnothing. \tag{4.61}
```

（见图 4.9。）因此，当我们找到标量化问题的最优点时，不仅找到了原向量优化问题的一个 Pareto 最优点，还找到了 $\mathbb{R}^q$ 中由 (4.61) 给出的整个半空间——其中的目标值不可达。

**凸向量优化问题的标量化。** 现在设向量优化问题 (4.56) 是凸的。则标量化问题 (4.60) 也凸，因为 $\lambda^Tf_0$ 是（标量值）凸函数（由 §3.6 的结果）。这意味着我们可以通过求解凸的标量优化问题找凸向量优化问题的 Pareto 最优点。对每个权重向量 $\lambda\succ_{K^*}0$ 的选择得到一个（通常不同的）Pareto 最优点。

<!-- p.193/194 -->

对凸向量优化问题我们有部分逆命题：对每个 Pareto 最优点 $x_{\mathrm{po}}$，存在某个非零 $\lambda\succeq_{K^*}0$ 使 $x_{\mathrm{po}}$ 是标量化问题 (4.60) 的解。粗略地说，对凸问题，标量化方法随权重向量 $\lambda$ 取遍 $K^*$-非负非零值而给出所有 Pareto 最优点。这里要小心：并非标量化问题在 $\lambda\succeq_{K^*}0$、$\lambda\neq0$ 下的每个解都是向量问题的 Pareto 最优点。（相比之下，$\lambda\succ_{K^*}0$ 时标量化问题的每个解都是 Pareto 最优的。）

<!-- p.194 -->

某些情形可以用这个部分逆命题找凸向量优化问题的所有 Pareto 最优点。$\lambda\succ_{K^*}0$ 的标量化给出一组 Pareto 最优点（如同在非凸向量优化问题中一样）。为找其余的 Pareto 最优解，须考虑满足 $\lambda\succeq_{K^*}0$ 的非零权重向量。对每个这样的权重向量，先找出标量化问题的所有解，然后检查这些解中哪些确实对向量优化问题 Pareto 最优。这些「极端的」Pareto 最优点也可以作为正权重向量得到的 Pareto 最优点的极限来找到。

为确立这个部分逆命题，考虑集合

```math
\mathcal{A}=\mathcal{O}+K=\{t\in\mathbb{R}^q \mid f_0(x)\preceq_K t \text{ 对某个可行 } x\}, \tag{4.62}
```

它由所有「（关于 $\preceq_K$）劣于或等于某个可达目标值」的值组成。可达目标值集合 $\mathcal{O}$ 未必凸，但当问题凸时集合 $\mathcal{A}$ 凸。而且 $\mathcal{A}$ 的极小元恰好与可达值集合 $\mathcal{O}$ 的极小元相同，即就是 Pareto 最优值。（见习题 4.53。）现在用 §2.6.3 的结果得出：$\mathcal{A}$ 的任何极小元对某个非零 $\lambda\succeq_{K^*}0$ 最小化 $\lambda^Tz$（$z\in\mathcal{A}$）。这意味着向量优化问题的每个 Pareto 最优点对某个非零权重 $\lambda\succeq_{K^*}0$ 是标量化问题的最优解。

<span style="color:#00838f;">**[例 4.10]** **矩阵集合的最小上界。** 考虑（关于半正定锥的）（凸）向量优化问题：

```math
\begin{aligned}
\text{minimize (w.r.t. }\mathbf{S}^n_+)\quad & X\\
\text{subject to}\quad & X\succeq A_i,\quad i=1,\dots,m,
\end{aligned} \tag{4.63}
```

其中 $A_i\in\mathbf{S}^n$（$i=1,\dots,m$）给定。约束意味着 $X$ 是给定矩阵 $A_1,\dots,A_m$ 的 upper bound（上界）；(4.63) 的 Pareto 最优解是这些矩阵的 minimal upper bound（最小上界）。

<!-- p.194/195 -->

为找 Pareto 最优点，应用标量化：选任何 $W\in\mathbf{S}^n_{++}$，构造问题

```math
\begin{aligned}
\text{minimize}\quad & \mathbf{tr}(WX)\\
\text{subject to}\quad & X\succeq A_i,\quad i=1,\dots,m,
\end{aligned} \tag{4.64}
```

这是一个 SDP。不同的 $W$ 选择一般给出不同的极小解。部分逆命题告诉我们：若 $X$ 对向量问题 (4.63) Pareto 最优，则它对某个非零权重矩阵 $W\succeq0$ 是 SDP (4.64) 的最优解。（不过此情形并非 (4.64) 的每个解都对向量优化问题 Pareto 最优。）

<!-- p.195 -->

这个问题可以给出简单的几何解释。给每个 $A\in\mathbf{S}^n_{++}$ 关联以原点为中心的椭球

```math
\mathcal{E}_A=\{u \mid u^TA^{-1}u\le1\},
```

于是 $A\preceq B$ 当且仅当 $\mathcal{E}_A\subseteq\mathcal{E}_B$。问题 (4.63) 的 Pareto 最优点 $X$ 对应包含与 $A_1,\dots,A_m$ 相关联的椭球的 minimal ellipsoid（最小椭球）。例子见图 4.10。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">4.7.5 Multicriterion optimization（多准则优化）</span>

<!-- p.195 -->

当向量优化问题涉及锥 $K=\mathbb{R}^q_+$ 时，称为 multicriterion（多准则）或 multi-objective（多目标）优化问题。$f_0$ 的分量（记 $F_1,\dots,F_q$）可解释为 $q$ 个不同的标量目标，每个都想最小化。我们称 $F_i$ 为问题的第 $i$ 个 objective（目标）。multicriterion optimization problem（多准则优化问题）是凸的，如果 $f_1,\dots,f_m$ 凸、$h_1,\dots,h_p$ 仿射、且各目标 $F_1,\dots,F_q$ 凸。

由于多准则问题是向量优化问题，§4.7.1–4.7.4 的所有材料都适用。不过对多准则问题可以把解释说得更具体。若 $x$ 可行，可以把 $F_i(x)$ 看作它按第 $i$ 个目标的 score（得分）或 value（价值）。若 $x,y$ 都可行，$F_i(x)\le F_i(y)$ 意味着按第 $i$ 个目标 $x$ 不劣于 $y$；$F_i(x)\lt F_i(y)$ 意味着按第 $i$ 个目标 $x$ 好于 $y$（$x$ beats $y$，胜过 $y$）。若 $x,y$ 都可行，我们说 $x$ 好于 $y$，或 $x$ dominates $y$（支配 $y$），如果 $F_i(x)\le F_i(y)$（$i=1,\dots,q$）且至少对某个 $j$ 有 $F_j(x)\lt F_j(y)$。粗略地说：$x$ 好于 $y$，指 $x$ 在所有目标上持平或胜过 $y$，且至少在一个目标上胜过它。

<!-- p.195/196 -->

在多准则问题中，最优点 $x^\star$ 满足

```math
F_i(x^\star)\le F_i(y),\quad i=1,\dots,q
```

<!-- p.196 -->

对每个可行 $y$。换言之，$x^\star$ 同时对每个标量问题

```math
\begin{aligned}
\text{minimize}\quad & F_j(x)\\
\text{subject to}\quad & f_i(x)\le0,\quad i=1,\dots,m\\
& h_i(x)=0,\quad i=1,\dots,p
\end{aligned}
```

（$j=1,\dots,q$）最优。当存在最优点时，我们说各目标是 noncompeting（不竞争）的：目标之间无需妥协，每个目标都达到即使忽略其他目标也能达到的最小值。

Pareto 最优点 $x_{\mathrm{po}}$ 满足：若 $y$ 可行且 $F_i(y)\le F_i(x_{\mathrm{po}})$（$i=1,\dots,q$），则 $F_i(x_{\mathrm{po}})=F_i(y)$（$i=1,\dots,q$）。可以重述为：点 Pareto 最优当且仅当它可行且没有更好的可行点。特别地，若可行点不 Pareto 最优，则至少存在另一个更好的可行点。因此在搜索好点时，显然可以把搜索限制在 Pareto 最优点。

**权衡分析（Trade-off analysis）。** 设 $x,y$ 是 Pareto 最优点，且

```math
\begin{aligned}
F_i(x)&<F_i(y),&& i\in\mathcal{A}\\
F_i(x)&=F_i(y),&& i\in\mathcal{B}\\
F_i(x)&>F_i(y),&& i\in\mathcal{C},
\end{aligned}
```

$\mathcal{A}\cup\mathcal{B}\cup\mathcal{C}=\{1,\dots,q\}$。换言之，$\mathcal{A}$ 是 $x$ 胜过 $y$ 的目标（下标）集，$\mathcal{B}$ 是 $x,y$ 打平的目标集，$\mathcal{C}$ 是 $y$ 胜过 $x$ 的目标集。若 $\mathcal{A}$ 与 $\mathcal{C}$ 皆空，则两点目标值完全相同。否则 $\mathcal{A}$ 与 $\mathcal{C}$ 必都非空。换言之：比较两个 Pareto 最优点时，要么表现完全相同（所有目标相等），要么各在至少一个目标上胜过对方。

比较 $x$ 与 $y$ 时，我们说用 $i\in\mathcal{A}$ 上更好的目标值 traded（交换）或 traded off（权衡）了 $i\in\mathcal{C}$ 上更差的目标值。Optimal trade-off analysis（最优权衡分析，或简称 trade-off analysis）研究：为在其他目标上做得更好，必须在一个或多个目标上做得多差；或更一般地，研究哪些目标值集合是可达的。

<!-- p.196/197 -->

例如考虑 bi-criterion（双准则）问题。设 $x$ 是 Pareto 最优点，目标 $F_1(x)$、$F_2(x)$。我们可能问：要多大的 $F_2(z)$ 才能得到满足 $F_1(z)\le F_1(x)-a$ 的可行点 $z$（$a>0$ 常数）？粗略地说，我们在问为换取第一个目标上 $a$ 的改进，必须在第二个目标上付出多少。若必须接受 $F_2$ 的大幅增加才能实现 $F_1$ 的小幅减小，我们说在 Pareto 最优值 $(F_1(x),F_2(x))$ 附近目标之间有 strong trade-off（强权衡）。反之若 $F_1$ 的大幅减小只需 $F_2$ 的小幅增加，我们说（在该 Pareto 最优值附近）权衡是 weak（弱）的。

也可以考虑在第一个目标上变差换取第二个目标改进的情形：此时找能把 $F_2(z)$ 降到多小，以获得满足 $F_1(z)\le F_1(x)+a$ 的可行点 $z$（$a>0$）。此情形我们在第二个目标上获得 benefit（收益），即 $F_2$ 相对 $F_2(x)$ 的下降。若这个收益大（即 $F_1$ 增加少量获得 $F_2$ 的大幅下降），我们说目标表现出强权衡；若小，我们说目标（在该 Pareto 最优值附近）弱权衡。

<!-- p.197/198 -->

**最优权衡面。** 多准则问题的 Pareto 最优值集合称为 optimal trade-off surface（最优权衡面；一般 $q>2$ 时）或 optimal trade-off curve（最优权衡曲线；$q=2$ 时）。（接受任何非 Pareto 最优的点都是愚蠢的，所以可以把权衡分析限制在 Pareto 最优点。）权衡分析有时也叫 exploring the optimal trade-off surface（探索最优权衡面）。（最优权衡面通常但并非总是通常意义上的曲面：例如问题有最优点时，最优权衡面由单点——最优值——组成。）

最优权衡曲线很容易解释。图 4.11（第 185 页）展示（凸）双准则问题的例子。从曲线可以直观地看到并理解两个目标间的权衡：

- 右端点显示不考虑 $F_1$ 时 $F_2$ 的最小可能值。
- 左端点显示不考虑 $F_2$ 时 $F_1$ 的最小可能值。
- 找曲线与竖线 $F_1=\alpha$ 的交点，可以看到要达到 $F_1\le\alpha$，$F_2$ 必须多大。
- 找曲线与横线 $F_2=\beta$ 的交点，可以看到要达到 $F_2\le\beta$，$F_1$ 必须多大。
- 曲线在一点（即一个 Pareto 最优值）处的斜率显示两个目标间的局部最优权衡。斜率陡的地方，$F_1$ 的小变化伴随 $F_2$ 的大变化。
- 曲率大（large curvature）的点是「一个目标的小下降只能通过另一个目标的大增加来实现」的地方。这就是权衡曲线著名的 knee（拐点/膝点），在许多应用中代表一个好的折中解。

所有这些都可以简单推广到权衡面，尽管可视化超过三个目标的曲面很困难。

<!-- p.197/198 -->

**多准则问题的标量化。** 用加权和目标

```math
\lambda^Tf_0(x)=\sum_{i=1}^q\lambda_iF_i(x)
```

<!-- p.198 -->

（$\lambda\succ0$）对多准则问题标量化时，可以把 $\lambda_i$ 解释为我们附在第 $i$ 个目标上的 weight（权重）。权重 $\lambda_i$ 可以看作量化我们想使 $F_i$ 变小（或对 $F_i$ 大的反对）。特别地，想让 $F_i$ 小就取大的 $\lambda_i$；若对 $F_i$ 关心得少，可取小的 $\lambda_i$。可以把比值 $\lambda_i/\lambda_j$ 解释为第 $i$ 个目标相对第 $j$ 个目标的 relative weight（相对权重）或 relative importance（相对重要性）。或者把 $\lambda_i/\lambda_j$ 看作两个目标之间的 exchange rate（兑换率）：在加权和目标中，$F_i$ 减少 $\alpha$ 被视为与 $F_j$ 增加 $(\lambda_i/\lambda_j)\alpha$ 相同。

这些解释给出探索最优权衡面时如何设置或改变权重的一些直觉。例如设权重向量 $\lambda\succ0$ 给出 Pareto 最优点 $x_{\mathrm{po}}$（目标值 $F_1(x_{\mathrm{po}}),\dots,F_q(x_{\mathrm{po}})$）。要找（可能）新的 Pareto 最优点——用更好的第 $k$ 个目标值换取（可能）更差的其他目标值——构造新权重向量 $\tilde{\lambda}$：

```math
\tilde{\lambda}_k>\lambda_k,\qquad \tilde{\lambda}_j=\lambda_j,\ j\neq k,\ j=1,\dots,q,
```

即增加第 $k$ 个目标的权重。这产生新的 Pareto 最优点 $\tilde{x}_{\mathrm{po}}$，满足 $F_k(\tilde{x}_{\mathrm{po}})\le F_k(x_{\mathrm{po}})$（通常严格小于），即第 $k$ 个目标改进的新 Pareto 最优点。

还可以看出：在最优权衡面光滑的任何点，$\lambda$ 给出该面在相应 Pareto 最优点的 inward normal（内法向）。特别地，当我们选择权重向量 $\lambda$ 并应用标量化时，得到一个 Pareto 最优点，$\lambda$ 给出目标间的局部权衡。

实践中，最优权衡面通过基于上述直觉的 ad hoc（临时）权重调整来探索。后面（第 5 章）会看到：标量化——最小化目标的加权和、再调整权重以获得合适的解——的基本思想正是 duality（对偶）的精髓。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">4.7.6 Examples（例子）</span>

<!-- p.198 -->

**正则化最小二乘。** 给定 $A\in\mathbb{R}^{m\times n}$ 与 $b\in\mathbb{R}^m$，要选择 $x\in\mathbb{R}^n$，兼顾两个二次目标：

- $F_1(x)=\|Ax-b\|_2^2=x^TA^TAx-2b^TAx+b^Tb$ 度量 $Ax$ 与 $b$ 的 misfit（失配）；
- $F_2(x)=\|x\|_2^2=x^Tx$ 度量 $x$ 的大小。

我们的目标是找拟合好（$F_1$ 小）且不大（$F_2$ 小）的 $x$。可以把它表述为关于锥 $\mathbb{R}^2_+$ 的向量优化问题，即 bi-criterion problem（双准则问题，无约束）：

```math
\text{minimize (w.r.t. }\mathbb{R}^2_+)\quad f_0(x)=(F_1(x),F_2(x)).
```

<!-- p.198/199 -->

取 $\lambda_1>0$、$\lambda_2>0$ 并最小化标量加权和目标

```math
\lambda^Tf_0(x)=\lambda_1F_1(x)+\lambda_2F_2(x)=x^T(\lambda_1A^TA+\lambda_2I)x-2\lambda_1b^TAx+\lambda_1b^Tb
```

进行标量化，得

```math
x(\mu)=(\lambda_1A^TA+\lambda_2I)^{-1}\lambda_1A^Tb=(A^TA+\mu I)^{-1}A^Tb,
```

其中 $\mu=\lambda_2/\lambda_1$。对任何 $\mu>0$，该点是双准则问题的 Pareto 最优点。可以把 $\mu=\lambda_2/\lambda_1$ 解释为给 $F_2$ 相对 $F_1$ 的相对权重。

<!-- p.199 -->

这个方法产生除两个极端 $\mu\to\infty$ 与 $\mu\to0$ 外的所有 Pareto 最优点。前一情形有 Pareto 最优解 $x=0$（用 $\lambda=(0,1)$ 标量化得到）；另一极端有 Pareto 最优解 $A^\dagger b$（$A^\dagger$ 是 $A$ 的伪逆），它是标量化问题最优解当 $\mu\to0$（即 $\lambda\to(1,0)$）的极限。（§6.3.2 会再遇到正则化最小二乘问题。）

图 4.11 展示问题数据 $A\in\mathbb{R}^{100\times10}$、$b\in\mathbb{R}^{100}$ 的正则化最小二乘问题的最优权衡曲线与可达值集合。（更多讨论见习题 4.50。）

**组合优化中的风险-回报权衡。** 第 155 页描述的经典 Markowitz 组合优化问题自然地表述为双准则问题：目标是负平均回报（因为想最大化平均回报）与回报方差：

```math
\begin{aligned}
\text{minimize (w.r.t. }\mathbb{R}^2_+)\quad & (F_1(x),F_2(x))=(-\bar{p}^Tx,\ x^T\Sigma x)\\
\text{subject to}\quad & \mathbf{1}^Tx=1,\quad x\succeq0.
\end{aligned}
```

构造相应的标量化问题时，可以（不失一般性）取 $\lambda_1=1$、$\lambda_2=\mu>0$：

```math
\begin{aligned}
\text{minimize}\quad & -\bar{p}^Tx+\mu x^T\Sigma x\\
\text{subject to}\quad & \mathbf{1}^Tx=1,\quad x\succeq0,
\end{aligned}
```

这是一个 QP。这个例子同样得到除 $\mu\to0$ 与 $\mu\to\infty$ 两个极限情形外的所有 Pareto 最优组合。粗略地说：前一情形得到不考虑回报方差的最大平均回报；后一情形得到不考虑平均回报的最小方差回报。设 $\bar{p}_k>\bar{p}_i$（$i\neq k$），即资产 $k$ 是唯一有最大平均回报的资产，则组合配置 $x=e_k$ 是对应 $\mu\to0$ 的唯一解（即把组合全部集中于平均回报最大的资产）。许多组合问题中资产 $n$ 对应 risk-free investment（无风险投资），回报（确定性）为 $r_{\mathrm{rf}}$。假设 $\Sigma$（去掉全零的最后一行与最后一列）满秩，则另一极端的 Pareto 最优组合是 $x=e_n$，即组合全部集中于无风险资产。

<!-- p.199/200 -->

作为具体例子，考虑 4 种资产的简单组合优化问题，价格变化的均值与标准差如下表：

| 资产 | $\bar{p}_i$ | $(\Sigma^{1/2})_{ii}$ |
|------|------------|----------------------|
| 1 | 12% | 20% |
| 2 | 10% | 10% |
| 3 | 7% | 5% |
| 4 | 3% | 0% |

资产 4 是无风险资产，回报（确定）3%。资产 3、2、1 的平均回报递增（7% 到 12%），标准差也递增（5% 到 20%）。资产间的相关系数为 $\rho_{12}=30\%$、$\rho_{13}=-40\%$、$\rho_{23}=0\%$。

<!-- p.200/201 -->

图 4.12 展示该组合优化问题的最优风险-回报权衡曲线。图按惯例绘制：横轴是标准差（方差的平方根），纵轴是期望回报。下方的图显示每个 Pareto 最优点的最优资产配置向量 $x$。这个简单例子的结果与直觉一致：小风险时，最优配置主要由无风险资产构成，混有少量其他资产。注意资产 3 与资产 1（负相关）的混合提供了一些 hedging（对冲），即在给定平均回报水平下降低方差。在权衡曲线另一端，aggressive growth portfolios（激进增长组合，即平均回报大的组合）把配置集中于资产 1 与 2——平均回报（与方差）最大的资产。

<!-- p.202 -->

## <span style="color:#c0392b;">Bibliography（文献注释）</span>

**Linear programming（线性规划）** 自 1940 年代以来被广泛研究，是许多优秀书籍的主题，包括 Dantzig [Dan63]、Luenberger [Lue84]、Schrijver [Sch86]、Papadimitriou 与 Steiglitz [PS98]、Bertsimas 与 Tsitsiklis [BT97]、Vanderbei [Van96]、Roos, Terlaky 与 Vial [RTV97]。Dantzig 与 Schrijver 还提供了线性规划的详细历史。近期综述见 Todd [Tod02]。

Schaible [Sch82, Sch83] 给出 fractional programming（分式规划，含线性分式问题及凸-凹分式问题等扩展，见习题 4.7）的概述。例 4.7 的增长经济模型出自 von Neumann [vN46]。

**Quadratic programming（二次规划）** 的研究始于 1950 年代（如 Frank 与 Wolfe [FW56]、Markowitz [Mar56]、Hildreth [Hil57]），部分动机是第 155 页讨论的组合优化问题（Markowitz [Mar52]）与第 154 页讨论的随机成本 LP（见 Freund [Fre56]）。

对 **second-order cone programming（二阶锥规划）** 的兴趣更晚近，始于 Nesterov 与 Nemirovski [NN94, §6.2.3]。SOCP 的理论与应用综述见 Alizadeh 与 Goldfarb [AG03]、Ben-Tal 与 Nemirovski [BTN01, lecture 3]（那里称此问题为 conic quadratic programming）、Lobo, Vandenberghe, Boyd 与 Lebret [LVBL98]。

**Robust linear programming（鲁棒线性规划）** 与一般的鲁棒凸优化起源于 Ben-Tal 与 Nemirovski [BTN98, BTN99] 与 El Ghaoui 与 Lebret [EL97]。Goldfarb 与 Iyengar [GI03a, GI03b] 讨论鲁棒 QCQP 及其在组合优化中的应用。El Ghaoui, Oustry 与 Lebret [EOL98] 聚焦鲁棒半定规划。

**Geometric programming（几何规划）** 自 1960 年代已知。Duffin, Peterson 与 Zener [DPZ67] 与 Zener [Zen71] 最先倡导在工程设计中使用它。Peterson [Pet76] 与 Ecker [Eck80] 描述 1970 年代的进展。这些文章与书还包括工程应用（尤其是化工与土木工程）的例子。Fishburn 与 Dunlop [FD85]、Sapatnekar, Rao, Vaidya 与 Kang [SRVK93]、Hershenson, Boyd 与 Lee [HBL01] 把几何规划应用于集成电路设计问题。悬臂梁设计例子（第 163 页）出自 Vanderplaats [Van84, page 147]。Perron-Frobenius 特征值的变分刻画（第 165 页）的证明见 Berman 与 Plemmons [BP94, page 31]。

Nesterov 与 Nemirovski [NN94, chapter 4] 把锥形式问题 (4.49) 引入为非线性凸优化的标准问题格式。锥规划方法在 Ben-Tal 与 Nemirovski [BTN01] 中进一步发展，他们还描述了大量应用。

Alizadeh [Ali91] 与 Nesterov 与 Nemirovski [NN94, §6.4] 最先系统研究 semidefinite programming（半定规划），并指出其在凸优化中的广泛应用。1990 年代 SDP 的后续研究受组合优化（Goemans 与 Williamson [GW95]）、控制（Boyd, El Ghaoui, Feron 与 Balakrishnan [BEFB94]、Scherer, Gahinet 与 Chilali [SGC97]、Dullerud 与 Paganini [DP00]）、通信与信号处理（Luo [Luo03]、Davidson, Luo, Wong 与 Ma [DLW00, MDW+02]）及其他工程领域的应用推动。Wolkowicz, Saigal 与 Vandenberghe 主编的书 [WSV00] 及 Todd [Tod01]、Lewis 与 Overton [LO96]、Vandenberghe 与 Boyd [VB95] 的文章提供综述与详尽文献。SDP 与矩问题的联系（我们在第 170 页给出一个简单例子）由 Bertsimas 与 Sethuraman [BS00]、Nesterov [Nes00]、Lasserre [Las02] 详细探讨。最快混合马尔可夫链问题出自 Boyd, Diaconis 与 Xiao [BDX04]。

**Multicriterion optimization（多准则优化）与 Pareto 最优性** 是经济学的基本工具；见 Pareto [Par71]、Debreu [Deb59] 与 Luenberger [Lue95]。例 4.9 的结果称为 Gauss-Markov theorem（高斯-马尔可夫定理）（Kailath, Sayed 与 Hassibi [KSH00, page 97]）。

---

> <span style="color:#7f8c8d;">第 4 章正文（§4.1–4.7）与文献注释全部翻译完毕。习题（PDF p.203–228）暂未翻译。下一章：boyd_ch05（Duality，对偶，PDF p.229–291）。</span>
