# <span style="color:#c0392b;">Boyd《凸优化》第 5 章 对偶（中：§5.5–5.7）</span>

> <span style="color:#7f8c8d;">Source: 教材/Convex Optimization (Boyd, Vandenberghe).pdf · Chapter 5 §5.5–5.7, pp.241–258 (PDF p.255–272) · 行内双语翻译版 · 译者：ZCode（bilingual-translator 铁律）</span>

## <span style="color:#c0392b;">5.5 Optimality conditions（最优性条件）</span>

提醒读者：除非明确说明，我们不假设问题 (5.1) 凸。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.5.1 Certificate of suboptimality and stopping criteria（次优性证书与停止准则）</span>

<!-- p.255 -->

若能找到对偶可行的 $(\lambda,\nu)$，就确立了原始问题最优值的下界：$p^\star\ge g(\lambda,\nu)$。因此对偶可行点 $(\lambda,\nu)$ 提供 $p^\star\ge g(\lambda,\nu)$ 的 proof（证明）或 certificate（证书）。强对偶性意味着存在任意好的证书。

对偶可行点使我们能在不知道 $p^\star$ 精确值的情况下，界定给定可行点的次优程度。事实上若 $x$ 原始可行、$(\lambda,\nu)$ 对偶可行，则

$$
f_0(x)-p^\star\le f_0(x)-g(\lambda,\nu).
$$

特别地，这确立 $x$ 是 $\epsilon$-次优的，$\epsilon=f_0(x)-g(\lambda,\nu)$。（也确立 $(\lambda,\nu)$ 对对偶问题 $\epsilon$-次优。）

我们把原始与对偶目标之间的 gap（间隙）

$$
f_0(x)-g(\lambda,\nu)
$$

称为与原始可行点 $x$、对偶可行点 $(\lambda,\nu)$ 相关联的 duality gap（对偶间隙）。原始-对偶可行对 $x,(\lambda,\nu)$ 把原始（与对偶）问题的最优值限定在一个区间内：

$$
p^\star\in[g(\lambda,\nu),f_0(x)],\qquad d^\star\in[g(\lambda,\nu),f_0(x)],
$$

区间宽度就是对偶间隙。

若原始-对偶可行对 $x,(\lambda,\nu)$ 的对偶间隙为零，即 $f_0(x)=g(\lambda,\nu)$，则 $x$ 原始最优、$(\lambda,\nu)$ 对偶最优。可以把 $(\lambda,\nu)$ 想成证明 $x$ 最优的证书（类似地可以把 $x$ 想成证明 $(\lambda,\nu)$ 对偶最优的证书）。

<!-- p.255/256 -->

这些观察可用于优化算法以提供 nonheuristic stopping criteria（非启发式停止准则）。设算法产生原始可行的 $x^{(k)}$ 与对偶可行的 $(\lambda^{(k)},\nu^{(k)})$ 序列（$k=1,2,\dots$），$\epsilon_{\mathrm{abs}}>0$ 是给定的要求绝对精度。则停止准则（即算法终止条件）

$$
f_0(x^{(k)})-g(\lambda^{(k)},\nu^{(k)})\le\epsilon_{\mathrm{abs}}
$$

<!-- p.256 -->

保证算法终止时 $x^{(k)}$ 是 $\epsilon_{\mathrm{abs}}$-次优的。事实上 $(\lambda^{(k)},\nu^{(k)})$ 是证明它的证书。（当然，若此方法要用于任意小的容差 $\epsilon_{\mathrm{abs}}$，强对偶性必须成立。）

类似的条件可用于保证给定的相对精度 $\epsilon_{\mathrm{rel}}>0$。若

$$
g(\lambda^{(k)},\nu^{(k)})>0,\qquad \frac{f_0(x^{(k)})-g(\lambda^{(k)},\nu^{(k)})}{g(\lambda^{(k)},\nu^{(k)})}\le\epsilon_{\mathrm{rel}}
$$

成立，或

$$
f_0(x^{(k)})<0,\qquad \frac{f_0(x^{(k)})-g(\lambda^{(k)},\nu^{(k)})}{-f_0(x^{(k)})}\le\epsilon_{\mathrm{rel}}
$$

成立，则 $p^\star\neq0$ 且相对误差 $\dfrac{f_0(x^{(k)})-p^\star}{|p^\star|}$ 保证小于等于 $\epsilon_{\mathrm{rel}}$。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.5.2 Complementary slackness（互补松弛性）</span>

<!-- p.256/257 -->

设原始与对偶最优值被达到且相等（特别地，强对偶性成立）。设 $x^\star$ 是原始最优、$(\lambda^\star,\nu^\star)$ 是对偶最优点。这意味着

$$
\begin{aligned}
f_0(x^\star)&=g(\lambda^\star,\nu^\star)\\
&=\inf_x\Bigl(f_0(x)+\sum_{i=1}^m\lambda_i^\star f_i(x)+\sum_{i=1}^p\nu_i^\star h_i(x)\Bigr)\\
&\le f_0(x^\star)+\sum_{i=1}^m\lambda_i^\star f_i(x^\star)+\sum_{i=1}^p\nu_i^\star h_i(x^\star)\\
&\le f_0(x^\star).
\end{aligned}
$$

第一行陈述最优对偶间隙为零，第二行是对偶函数的定义。第三行成立因为拉格朗日函数关于 $x$ 的下确界小于等于其在 $x=x^\star$ 处的值。最后一个不等式由 $\lambda_i^\star\ge0$、$f_i(x^\star)\le0$（$i=1,\dots,m$）与 $h_i(x^\star)=0$（$i=1,\dots,p$）得出。我们得出：这个链中的两个不等式都取等号。

<!-- p.257 -->

由此可以得出几个有趣的结论。例如，由于第三行的等式成立，$x^\star$ 在 $x$ 上最小化 $L(x,\lambda^\star,\nu^\star)$。（拉格朗日函数 $L(x,\lambda^\star,\nu^\star)$ 可以有其他最小化点；$x^\star$ 只是一个最小化点。）

另一个重要结论是

$$
\sum_{i=1}^m\lambda_i^\star f_i(x^\star)=0.
$$

由于和中每项非正，得出

$$
\lambda_i^\star f_i(x^\star)=0,\qquad i=1,\dots,m. \tag{5.48}
$$

这个条件称为 complementary slackness（互补松弛性）；它对任何原始最优 $x^\star$ 与任何对偶最优 $(\lambda^\star,\nu^\star)$ 成立（强对偶性成立时）。可以把互补松弛条件表达为

$$
\lambda_i^\star>0 \implies f_i(x^\star)=0,
$$

或等价地

$$
f_i(x^\star)<0 \implies \lambda_i^\star=0.
$$

粗略地说：第 $i$ 个最优拉格朗日乘子为零，除非第 $i$ 个约束在最优点处起作用。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.5.3 KKT optimality conditions（KKT 最优性条件）</span>

<!-- p.257 -->

现在假设函数 $f_0,\dots,f_m,h_1,\dots,h_p$ 可微（从而定义域开），但暂不对凸性做假设。

**非凸问题的 KKT 条件。** 如上，设 $x^\star$ 与 $(\lambda^\star,\nu^\star)$ 是任何对偶间隙为零的原始与对偶最优点。由于 $x^\star$ 在 $x$ 上最小化 $L(x,\lambda^\star,\nu^\star)$，其梯度在 $x^\star$ 必为零：

$$
\nabla f_0(x^\star)+\sum_{i=1}^m\lambda_i^\star\nabla f_i(x^\star)+\sum_{i=1}^p\nu_i^\star\nabla h_i(x^\star)=0.
$$

于是有

$$
\begin{aligned}
f_i(x^\star)&\le0,&& i=1,\dots,m\\
h_i(x^\star)&=0,&& i=1,\dots,p\\
\lambda_i^\star&\ge0,&& i=1,\dots,m\\
\lambda_i^\star f_i(x^\star)&=0,&& i=1,\dots,m\\
\nabla f_0(x^\star)+\sum_{i=1}^m\lambda_i^\star\nabla f_i(x^\star)+\sum_{i=1}^p\nu_i^\star\nabla h_i(x^\star)&=0,
\end{aligned} \tag{5.49}
$$

称为 Karush-Kuhn-Tucker (KKT) conditions（KKT 条件）。总结：对任何目标与约束函数可微、且强对偶性成立的优化问题，任何原始与对偶最优点对必须满足 KKT 条件 (5.49)。

<!-- p.258 -->

**凸问题的 KKT 条件。** 当原始问题凸时，KKT 条件对「点是原始与对偶最优」也是 sufficient（充分）的。换言之，若 $f_i$ 凸、$h_i$ 仿射，且 $\tilde{x},\tilde{\lambda},\tilde{\nu}$ 是满足 KKT 条件

$$
\begin{aligned}
f_i(\tilde{x})&\le0,&& i=1,\dots,m\\
h_i(\tilde{x})&=0,&& i=1,\dots,p\\
\tilde{\lambda}_i&\ge0,&& i=1,\dots,m\\
\tilde{\lambda}_if_i(\tilde{x})&=0,&& i=1,\dots,m\\
\nabla f_0(\tilde{x})+\sum_{i=1}^m\tilde{\lambda}_i\nabla f_i(\tilde{x})+\sum_{i=1}^p\tilde{\nu}_i\nabla h_i(\tilde{x})&=0
\end{aligned}
$$

的任何点，则 $\tilde{x}$ 与 $(\tilde{\lambda},\tilde{\nu})$ 原始与对偶最优，对偶间隙为零。

为看出这一点，注意前两个条件说明 $\tilde{x}$ 原始可行。由于 $\tilde{\lambda}_i\ge0$，$L(x,\tilde{\lambda},\tilde{\nu})$ 关于 $x$ 凸；最后一个 KKT 条件说明其关于 $x$ 的梯度在 $x=\tilde{x}$ 为零，故 $\tilde{x}$ 在 $x$ 上最小化 $L(x,\tilde{\lambda},\tilde{\nu})$。由此得

$$
g(\tilde{\lambda},\tilde{\nu})=L(\tilde{x},\tilde{\lambda},\tilde{\nu})=f_0(\tilde{x})+\sum_{i=1}^m\tilde{\lambda}_if_i(\tilde{x})+\sum_{i=1}^p\tilde{\nu}_ih_i(\tilde{x})=f_0(\tilde{x}),
$$

最后一行用 $h_i(\tilde{x})=0$ 与 $\tilde{\lambda}_if_i(\tilde{x})=0$。这表明 $\tilde{x}$ 与 $(\tilde{\lambda},\tilde{\nu})$ 对偶间隙为零，因此原始与对偶最优。总结：对任何目标与约束函数可微的凸优化问题，满足 KKT 条件的任何点都原始与对偶最优、对偶间隙为零。

若带可微目标与约束函数的凸优化问题满足 Slater 条件，则 KKT 条件提供最优性的充要条件：Slater 条件蕴含最优对偶间隙为零且对偶最优达到，所以 $x$ 最优当且仅当存在 $(\lambda,\nu)$ 与 $x$ 一起满足 KKT 条件。

KKT 条件在优化中扮演重要角色。少数特殊情形可以解析地求解 KKT 条件（从而求解优化问题）。更一般地，许多凸优化算法被构思为（或可解释为）求解 KKT 条件的方法。

<span style="color:#00838f;">**[例 5.1]** **等式约束的凸二次最小化。** 考虑问题

$$
\begin{aligned}
\text{minimize}\quad & (1/2)x^TPx+q^Tx+r\\
\text{subject to}\quad & Ax=b,
\end{aligned} \tag{5.50}
$$

$P\in\mathbf{S}^n_+$。此问题的 KKT 条件为

$$
Ax^\star=b,\qquad Px^\star+q+A^T\nu^\star=0,
$$

可以写成

$$
\begin{bmatrix}P&A^T\\A&0\end{bmatrix}\begin{bmatrix}x^\star\\\nu^\star\end{bmatrix}=\begin{bmatrix}-q\\b\end{bmatrix}.
$$

<!-- p.259 -->

解这组关于 $m+n$ 个变量 $x^\star,\nu^\star$ 的 $m+n$ 个方程，给出 (5.50) 的最优原始与对偶变量。

<span style="color:#00838f;">**[例 5.2]** **注水（water-filling）。** 考虑凸优化问题

$$
\begin{aligned}
\text{minimize}\quad & -\sum_{i=1}^n\log(\alpha_i+x_i)\\
\text{subject to}\quad & x\succeq0,\quad \mathbf{1}^Tx=1,
\end{aligned}
$$

$\alpha_i>0$。此问题出现在 information theory（信息论）中：给 $n$ 个通信信道分配功率。变量 $x_i$ 表示分配给第 $i$ 个信道的发射功率，$\log(\alpha_i+x_i)$ 给出信道的 capacity（容量）或通信速率，所以问题是把总功率 1 分配给各信道以最大化总通信速率。

为不等式约束 $x^\star\succeq0$ 引入拉格朗日乘子 $\lambda^\star\in\mathbb{R}^n$、为等式约束 $\mathbf{1}^Tx=1$ 引入乘子 $\nu^\star\in\mathbb{R}$，得 KKT 条件：

$$
\begin{aligned}
&x^\star\succeq0,\qquad \mathbf{1}^Tx^\star=1,\\
&\lambda^\star\succeq0,\qquad \lambda_i^\star x_i^\star=0,\quad i=1,\dots,n,\\
&-1/(\alpha_i+x_i^\star)-\lambda_i^\star+\nu^\star=0,\quad i=1,\dots,n.
\end{aligned}
$$

我们可以直接解这些方程求 $x^\star,\lambda^\star,\nu^\star$。先注意 $\lambda^\star$ 在最后一个方程中充当 slack variable（松弛变量），可以消去，留下

$$
\begin{aligned}
&x^\star\succeq0,\qquad \mathbf{1}^Tx^\star=1,\\
&x_i^\star(\nu^\star-1/(\alpha_i+x_i^\star))=0,\quad i=1,\dots,n,\\
&\nu^\star\ge1/(\alpha_i+x_i^\star),\quad i=1,\dots,n.
\end{aligned}
$$

若 $\nu^\star<1/\alpha_i$，最后这个条件只有 $x_i^\star>0$ 才能成立，而这由第三个条件蕴含 $\nu^\star=1/(\alpha_i+x_i^\star)$。解 $x_i^\star$ 得：$\nu^\star<1/\alpha_i$ 时 $x_i^\star=1/\nu^\star-\alpha_i$。若 $\nu^\star\ge1/\alpha_i$，则 $x_i^\star>0$ 不可能，因为它会蕴含 $\nu^\star\ge1/\alpha_i>1/(\alpha_i+x_i^\star)$，违反互补松弛条件。因此 $\nu^\star\ge1/\alpha_i$ 时 $x_i^\star=0$。于是

$$
x_i^\star=\begin{cases}1/\nu^\star-\alpha_i & \nu^\star<1/\alpha_i\\ 0 & \nu^\star\ge1/\alpha_i,\end{cases}
$$

<!-- p.259/260 -->

更简单地：$x_i^\star=\max\{0,\ 1/\nu^\star-\alpha_i\}$。把这个 $x_i^\star$ 表达式代入条件 $\mathbf{1}^Tx^\star=1$ 得

$$
\sum_{i=1}^n\max\{0,\ 1/\nu^\star-\alpha_i\}=1.
$$

左边是 $1/\nu^\star$ 的分段线性递增函数，breakpoints（断点）在 $\alpha_i$，所以方程有唯一解，容易确定。

<!-- p.260 -->

这个解法称为 water-filling（注水），原因如下：把 $\alpha_i$ 想成地块 $i$ 上方的地面高度，然后把区域注水到深度 $1/\nu$（图 5.7）。用水总量为 $\sum_{i=1}^n\max\{0,1/\nu^\star-\alpha_i\}$。然后提高水位直到用水总量等于 1。地块 $i$ 上方的水深就是最优值 $x_i^\star$。

> <span style="color:#2471a3;">**[译者注｜跨课程连接]** 例 5.2 是信息论的正门！注水解就是平行高斯信道的容量分配（Cover & Thomas / MacKay 书的内容），「注水算法」将出现在你 MacKay 信息论讲义 L2-L3 与通信课中。KKT 条件在此把凸优化与 Shannon 容量直接连了起来。</span>

<!-- p.260/261 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.5.4 Mechanics interpretation of KKT conditions（KKT 条件的力学解释）</span>

KKT 条件在力学中有漂亮的解释（这确实是拉格朗日的主要动机之一）。用简单例子说明。图 5.8 的系统由两个块组成，它们彼此相连、并通过三个弹簧连接到左右墙壁。块的位置由 $x\in\mathbb{R}^2$ 给出，$x_1$ 是左块（中部）的位移，$x_2$ 是右块的位移。左墙位于位置 0，右墙位于位置 $l$。

<!-- p.261 -->

弹簧的势能作为块位置的函数为

$$
f_0(x_1,x_2)=\frac{1}{2}k_1x_1^2+\frac{1}{2}k_2(x_2-x_1)^2+\frac{1}{2}k_3(l-x_2)^2,
$$

其中 $k_i>0$ 是三个弹簧的 stiffness constants（刚度常数）。平衡位置 $x^\star$ 是在不等式约束

$$
w/2-x_1\le0,\qquad w+x_1-x_2\le0,\qquad w/2-l+x_2\le0 \tag{5.51}
$$

下最小化势能的位置。这些约束称为 kinematic constraints（运动学约束），表达块有宽度 $w>0$、不能彼此穿透也不能穿墙的事实。

<!-- p.261/262 -->

平衡位置因此由优化问题

$$
\begin{aligned}
\text{minimize}\quad & (1/2)\bigl(k_1x_1^2+k_2(x_2-x_1)^2+k_3(l-x_2)^2\bigr)\\
\text{subject to}\quad & w/2-x_1\le0\\
& w+x_1-x_2\le0\\
& w/2-l+x_2\le0
\end{aligned} \tag{5.52}
$$

的解给出，它是一个 QP。

以 $\lambda_1,\lambda_2,\lambda_3$ 为拉格朗日乘子，此问题的 KKT 条件由运动学约束 (5.51)、非负性约束 $\lambda_i\ge0$、互补松弛条件

$$
\lambda_1(w/2-x_1)=0,\qquad \lambda_2(w-x_2+x_1)=0,\qquad \lambda_3(w/2-l+x_2)=0, \tag{5.53}
$$

以及零梯度条件

$$
\begin{bmatrix}k_1x_1-k_2(x_2-x_1)\\ k_2(x_2-x_1)-k_3(l-x_2)\end{bmatrix}+\lambda_1\begin{bmatrix}-1\\0\end{bmatrix}+\lambda_2\begin{bmatrix}1\\-1\end{bmatrix}+\lambda_3\begin{bmatrix}0\\1\end{bmatrix}=0 \tag{5.54}
$$

<!-- p.262 -->

组成。方程 (5.54) 可以解释为两个块的 force balance equations（力平衡方程），只要把拉格朗日乘子解释为作用在墙壁与块之间的 contact forces（接触力）（图 5.9）。第一个方程说明第一个块上的合力为零：$-k_1x_1$ 是左弹簧施加在左块上的力，$k_2(x_2-x_1)$ 是中间弹簧施加的力，$\lambda_1$ 是左墙施加的力，$-\lambda_2$ 是右块施加的力。接触力必须背离接触面（由约束 $\lambda_1\ge0$ 与 $-\lambda_2\le0$ 表达），且只在有接触时非零（由前两个互补松弛条件 (5.53) 表达）。类似地，(5.54) 的第二个方程是第二个块的力平衡，(5.53) 的最后一个条件说明除非右块接触墙壁，$\lambda_3$ 为零。

此例中势能与运动学约束函数凸，且（精化形式的）Slater 约束规格在 $2w\le l$（即墙之间有足够空间容纳两个块）时成立，所以我们可以断定：由 (5.52) 给出的平衡的能量表述与由 KKT 条件给出的力平衡表述给出相同结果。

<!-- p.262/263 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.5.5 Solving the primal problem via the dual（借助对偶求解原始问题）</span>

§5.5.3 开头提到过：若强对偶性成立且存在对偶最优解 $(\lambda^\star,\nu^\star)$，则任何原始最优点也是 $L(x,\lambda^\star,\nu^\star)$ 的最小化点。这一事实有时允许我们从对偶最优解计算原始最优解。

更精确地：设我们有强对偶性且已知最优的 $(\lambda^\star,\nu^\star)$。设 $L(x,\lambda^\star,\nu^\star)$ 的最小化点（即问题

$$
\text{minimize}\quad f_0(x)+\sum_{i=1}^m\lambda_i^\star f_i(x)+\sum_{i=1}^p\nu_i^\star h_i(x), \tag{5.55}
$$

的解）是唯一的。（对凸问题，例如当 $L(x,\lambda^\star,\nu^\star)$ 关于 $x$ 严格凸时会发生。）那么若 (5.55) 的解原始可行，它必是原始最优的；若它不原始可行，则不可能存在原始最优点，即可以断定原始最优未被达到。当对偶问题比原始问题更容易求解时（例如可解析求解，或有可利用的特殊结构），这个观察很有意思。

<span style="color:#00838f;">**[例 5.3]** **熵最大化。** 考虑熵最大化问题

$$
\begin{aligned}
\text{minimize}\quad & f_0(x)=\sum_{i=1}^nx_i\log x_i\\
\text{subject to}\quad & Ax\preceq b,\quad \mathbf{1}^Tx=1
\end{aligned}
$$

（$\mathbb{R}^n_{++}$ 上）及其对偶问题

$$
\begin{aligned}
\text{maximize}\quad & -b^T\lambda-\nu-e^{-\nu-1}\sum_{i=1}^ne^{-a_i^T\lambda}\\
\text{subject to}\quad & \lambda\succeq0
\end{aligned}
$$

（$a_i$ 是 $A$ 的列；见第 222、228 页）。设（弱形式的）Slater 条件成立，即存在 $x\succ0$ 满足 $Ax\preceq b$ 与 $\mathbf{1}^Tx=1$，故强对偶性成立且存在最优解 $(\lambda^\star,\nu^\star)$。

<!-- p.263 -->

设我们已解出对偶问题。$(\lambda^\star,\nu^\star)$ 处的拉格朗日函数为

$$
L(x,\lambda^\star,\nu^\star)=\sum_{i=1}^nx_i\log x_i+\lambda^{\star T}(Ax-b)+\nu^\star(\mathbf{1}^Tx-1),
$$

它在 $\mathcal{D}$ 上严格凸且有下界，所以有唯一解 $x^\star$：

$$
x_i^\star=1/\exp(a_i^T\lambda^\star+\nu^\star+1),\qquad i=1,\dots,n.
$$

若 $x^\star$ 原始可行，它必是原始问题 (5.13) 的最优解。若 $x^\star$ 不原始可行，则可断定原始最优未被达到。

<span style="color:#00838f;">**[例 5.4]** **等式约束下最小化可分函数。** 考虑问题

$$
\begin{aligned}
\text{minimize}\quad & f_0(x)=\sum_{i=1}^nf_i(x_i)\\
\text{subject to}\quad & a^Tx=b,
\end{aligned}
$$

$a\in\mathbb{R}^n$、$b\in\mathbb{R}$、$f_i:\mathbb{R}\to\mathbb{R}$ 可微且严格凸。目标函数称为 separable（可分）的，因为它是单个变量 $x_1,\dots,x_n$ 的函数之和。设 $f_0$ 的定义域与约束集相交，即存在 $x_0\in\mathbf{dom}\,f_0$ 满足 $a^Tx_0=b$。这蕴含问题有唯一最优点 $x^\star$。

拉格朗日函数为

$$
L(x,\nu)=\sum_{i=1}^nf_i(x_i)+\nu(a^Tx-b)=-b\nu+\sum_{i=1}^n(f_i(x_i)+\nu a_ix_i),
$$

也可分，所以对偶函数为

$$
\begin{aligned}
g(\nu)&=-b\nu+\inf_x\Bigl(\sum_{i=1}^n(f_i(x_i)+\nu a_ix_i)\Bigr)\\
&=-b\nu+\sum_{i=1}^n\inf_{x_i}(f_i(x_i)+\nu a_ix_i)\\
&=-b\nu-\sum_{i=1}^nf_i^*(-\nu a_i).
\end{aligned}
$$

对偶问题为

$$
\text{maximize}\quad -b\nu-\sum_{i=1}^nf_i^*(-\nu a_i),
$$

（标量）变量 $\nu\in\mathbb{R}$。

现在设我们找到了最优对偶变量 $\nu^\star$。（有几种求解单标量变量凸问题的简单方法，如二分法。）由于每个 $f_i$ 严格凸，$L(x,\nu^\star)$ 关于 $x$ 严格凸，有唯一最小化点 $\tilde{x}$。但我们还知道 $x^\star$ 最小化 $L(x,\nu^\star)$，所以必有 $\tilde{x}=x^\star$。可以从 $\nabla_xL(x,\nu^\star)=0$ 恢复 $x^\star$，即解方程 $f_i'(x_i^\star)=-\nu^\star a_i$。

<!-- p.263/264 -->

## <span style="color:#c0392b;">5.6 Perturbation and sensitivity analysis（扰动与敏感性分析）</span>

当强对偶性成立时，最优对偶变量给出关于最优值对约束扰动的敏感性的非常有用的信息。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.6.1 The perturbed problem（扰动问题）</span>

考虑原优化问题 (5.1) 的如下 perturbed（扰动）版本：

$$
\begin{aligned}
\text{minimize}\quad & f_0(x)\\
\text{subject to}\quad & f_i(x)\le u_i,\quad i=1,\dots,m\\
& h_i(x)=v_i,\quad i=1,\dots,p,
\end{aligned} \tag{5.56}
$$

<!-- p.264 -->

变量 $x\in\mathbb{R}^n$。当 $u=0,v=0$ 时此问题与原问题 (5.1) 一致。$u_i$ 为正表示放宽了第 $i$ 个不等式约束；$u_i$ 为负表示收紧了约束。因此扰动问题 (5.56) 是把每个不等式约束收紧或放宽 $u_i$、把等式约束右端改变 $v_i$ 后的原问题 (5.1)。

我们定义 $p^\star(u,v)$ 为扰动问题 (5.56) 的最优值：

$$
p^\star(u,v)=\inf\{f_0(x) \mid \exists x\in\mathcal{D},\ f_i(x)\le u_i,\ i=1,\dots,m,\ h_i(x)=v_i,\ i=1,\dots,p\}.
$$

可能有 $p^\star(u,v)=\infty$，对应导致不可行的约束扰动。注意 $p^\star(0,0)=p^\star$（未扰动问题 (5.1) 的最优值）。（希望这点轻微的记号滥用不会引起混淆。）粗略地说，函数 $p^\star:\mathbb{R}^m\times\mathbb{R}^p\to\mathbb{R}$ 给出问题最优值作为约束右端扰动的函数。

原问题凸时，函数 $p^\star$ 是 $u,v$ 的凸函数；事实上它的上图正是 (5.37) 定义的集合 $\mathcal{A}$ 的闭包（见习题 5.32）。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.6.2 A global inequality（全局不等式）</span>

<!-- p.264/265 -->

现在设强对偶性成立且对偶最优达到。（原问题凸且满足 Slater 条件时即如此。）设 $(\lambda^\star,\nu^\star)$ 对未扰动问题的对偶 (5.16) 最优。则对所有 $u,v$：

$$
p^\star(u,v)\ge p^\star(0,0)-\lambda^{\star T}u-\nu^{\star T}v. \tag{5.57}
$$

为确立此不等式，设 $x$ 是扰动问题的任何可行点（即 $f_i(x)\le u_i$（$i=1,\dots,m$）、$h_i(x)=v_i$（$i=1,\dots,p$））。则由强对偶性：

$$
\begin{aligned}
p^\star(0,0)=g(\lambda^\star,\nu^\star)&\le f_0(x)+\sum_{i=1}^m\lambda_i^\star f_i(x)+\sum_{i=1}^p\nu_i^\star h_i(x)\\
&\le f_0(x)+\lambda^{\star T}u+\nu^{\star T}v.
\end{aligned}
$$

（第一个不等式由 $g(\lambda^\star,\nu^\star)$ 的定义；第二个由 $\lambda^\star\succeq0$。）我们得出：对扰动问题的任何可行 $x$，

$$
f_0(x)\ge p^\star(0,0)-\lambda^{\star T}u-\nu^{\star T}v,
$$

由得 (5.57)。

<!-- p.265 -->

**敏感性解释（Sensitivity interpretations）。** 强对偶性成立时，最优拉格朗日变量的各种敏感性解释直接由不等式 (5.57) 得出。一些结论：

- 若 $\lambda_i^\star$ 大，而我们收紧第 $i$ 个约束（取 $u_i<0$），则最优值 $p^\star(u,v)$ 保证大幅增加。
- 若 $\nu_i^\star$ 大且为正而取 $v_i<0$，或 $\nu_i^\star$ 大且为负而取 $v_i>0$，则最优值 $p^\star(u,v)$ 保证大幅增加。
- 若 $\lambda_i^\star$ 小，而我们放宽第 $i$ 个约束（$u_i>0$），则最优值 $p^\star(u,v)$ 不会下降太多。
- 若 $\nu_i^\star$ 小且为正而 $v_i>0$，或 $\nu_i^\star$ 小且为负而 $v_i<0$，则最优值 $p^\star(u,v)$ 不会下降太多。

不等式 (5.57) 与上面列出的结论给出扰动最优值的下界，但没有上界。因此这些结果对放宽或收紧约束是不对称的。例如设 $\lambda_i^\star$ 大，而我们稍微放宽第 $i$ 个约束（$u_i$ 小且正）。此时不等式 (5.57) 没有用：它并不蕴含最优值会大幅下降。

不等式 (5.57) 见图 5.10（单不等式约束的凸问题）。不等式陈述：仿射函数 $p^\star(0)-\lambda^\star u$ 是凸函数 $p^\star$ 的下界。

<!-- p.265/266 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.6.3 Local sensitivity analysis（局部敏感性分析）</span>

<!-- p.266 -->

现在设 $p^\star(u,v)$ 在 $u=0,v=0$ 处可微。则只要强对偶性成立，最优对偶变量 $\lambda^\star,\nu^\star$ 与 $p^\star$ 在 $u=0,v=0$ 处的梯度相关：

$$
\lambda_i^\star=-\frac{\partial p^\star(0,0)}{\partial u_i},\qquad \nu_i^\star=-\frac{\partial p^\star(0,0)}{\partial v_i}. \tag{5.58}
$$

这个性质可以从图 5.10 的例子看出：$-\lambda^\star$ 是 $p^\star$ 在 $u=0$ 附近的斜率。

因此当 $p^\star(u,v)$ 在 $u=0,v=0$ 可微且强对偶性成立时，最优拉格朗日乘子恰是最优值对约束扰动的 local sensitivities（局部敏感性）。与不可微情形相比，这个解释是对称的：收紧第 $i$ 个不等式约束少量（$u_i$ 小且负）产生 $p^\star$ 约 $-\lambda_i^\star u_i$ 的增加；放宽第 $i$ 个约束少量（$u_i$ 小且正）产生 $p^\star$ 约 $\lambda_i^\star u_i$ 的下降。

为证 (5.58)，设 $p^\star(u,v)$ 可微且强对偶性成立。对扰动 $u=te_i$、$v=0$（$e_i$ 是第 $i$ 个单位向量）：

$$
\lim_{t\to0}\frac{p^\star(te_i,0)-p^\star}{t}=\frac{\partial p^\star(0,0)}{\partial u_i}.
$$

不等式 (5.57) 陈述：对 $t>0$，

$$
\frac{p^\star(te_i,0)-p^\star}{t}\ge-\lambda_i^\star,
$$

而对 $t<0$ 有相反的不等式。取 $t\to0^+$ 极限得

$$
\frac{\partial p^\star(0,0)}{\partial u_i}\ge-\lambda_i^\star,
$$

取 $t<0$ 极限得相反不等式，于是

$$
\frac{\partial p^\star(0,0)}{\partial u_i}=-\lambda_i^\star.
$$

同样的方法可以确立 $\dfrac{\partial p^\star(0,0)}{\partial v_i}=-\nu_i^\star$。

<!-- p.266/267 -->

局部敏感性结果 (5.58) 给出约束在最优点 $x^\star$ 处「多活跃」的定量度量。若 $f_i(x^\star)<0$，则约束不起作用，于是约束可以少量收紧或放宽而不影响最优值。由互补松弛性，相应的最优拉格朗日乘子必为零。但若 $f_i(x^\star)=0$，即第 $i$ 个约束在最优点处活跃，第 $i$ 个最优拉格朗日乘子告诉我们约束多活跃：$\lambda_i^\star$ 小意味着约束可以少量放宽或收紧而对最优值影响不大；$\lambda_i^\star$ 大意味着约束稍加放宽或收紧就会对最优值产生很大影响。

<!-- p.267 -->

**影子价格解释。** 结果 (5.58) 也可以在经济学框架下给出简单几何解释。（为简单）考虑没有等式约束、满足 Slater 条件的凸问题。变量 $x\in\mathbb{R}^m$ 决定 firm（企业）如何运营，目标 $f_0$ 是成本，即 $-f_0$ 是利润。每个约束 $f_i(x)\le0$ 表示对某种 resource（资源）（如劳动力、钢材、仓库空间）的限制。（负的）扰动最优成本函数 $-p^\star(u)$ 告诉我们：如果每种资源对企业的供给增加或减少，利润能多赚或少赚多少。若它在 $u=0$ 附近可微，则

$$
\lambda_i^\star=-\frac{\partial p^\star(0)}{\partial u_i}.
$$

换言之，$\lambda_i^\star$ 告诉我们：资源 $i$ 的可得性少量增加，企业大约能多赚多少利润。

由此：$\lambda_i^\star$ 将是资源 $i$ 的 natural or equilibrium price（自然或均衡价格），如果企业可以买卖它的话。举例：设企业能以低于 $\lambda_i^\star$ 的价格买卖资源 $i$，此时它肯定会买入一些资源，使其能以提高超过买入成本的方式运营。反之若价格超过 $\lambda_i^\star$，企业会卖出一部分资源 $i$ 的配额并获得净收益，因为卖出收入大于资源可得性减少带来的利润下降。

<!-- p.267/268 -->

## <span style="color:#c0392b;">5.7 Examples（例子）</span>

本节用例子展示：问题的简单等价重新表述可能导致非常不同的对偶问题。考虑以下类型的重新表述：

- 引入新变量及相应的等式约束。
- 用原目标的递增函数替换目标。
- 把显式约束变为隐式，即把它们并入目标的定义域。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.7.1 Introducing new variables and equality constraints（引入新变量与等式约束）</span>

考虑形如

$$
\text{minimize}\quad f_0(Ax+b) \tag{5.59}
$$

的无约束问题。其拉格朗日对偶函数是常数 $p^\star$。所以虽然强对偶性成立（$p^\star=d^\star$），拉格朗日对偶既无用也不有趣。

<!-- p.268 -->

现在把问题 (5.59) 重新表述为

$$
\begin{aligned}
\text{minimize}\quad & f_0(y)\\
\text{subject to}\quad & Ax+b=y. \tag{5.60}
\end{aligned}
$$

这里引入了新变量 $y$ 以及新等式约束 $Ax+b=y$。问题 (5.59) 与 (5.60) 显然等价。重新表述的问题的拉格朗日函数为

$$
L(x,y,\nu)=f_0(y)+\nu^T(Ax+b-y).
$$

为找对偶函数，对 $x$ 与 $y$ 最小化 $L$。对 $x$ 最小化发现：除非 $A^T\nu=0$，$g(\nu)=-\infty$；此时留下

$$
g(\nu)=b^T\nu+\inf_y(f_0(y)-\nu^Ty)=b^T\nu-f_0^*(\nu),
$$

其中 $f_0^*$ 是 $f_0$ 的共轭。(5.60) 的对偶问题因此可表达为

$$
\begin{aligned}
\text{maximize}\quad & b^T\nu-f_0^*(\nu)\\
\text{subject to}\quad & A^T\nu=0. \tag{5.61}
\end{aligned}
$$

于是重新表述的问题 (5.60) 的对偶远比原问题 (5.59) 的对偶有用。

<span style="color:#00838f;">**[例 5.5]** **无约束几何规划。** 考虑无约束几何规划

$$
\text{minimize}\quad \log\Bigl(\sum_{i=1}^m\exp(a_i^Tx+b_i)\Bigr).
$$

先通过引入新变量与等式约束重新表述：

$$
\begin{aligned}
\text{minimize}\quad & f_0(y)=\log\Bigl(\sum_{i=1}^m\exp y_i\Bigr)\\
\text{subject to}\quad & Ax+b=y,
\end{aligned}
$$

$a_i^T$ 是 $A$ 的行。log-sum-exp 函数的共轭是

$$
f_0^*(\nu)=\begin{cases}\sum_{i=1}^m\nu_i\log\nu_i & \nu\succeq0,\ \mathbf{1}^T\nu=1\\ \infty & \text{otherwise}\end{cases}
$$

（例 3.25，第 93 页），所以重新表述的问题的对偶可表达为

$$
\begin{aligned}
\text{maximize}\quad & b^T\nu-\sum_{i=1}^m\nu_i\log\nu_i\\
\text{subject to}\quad & \mathbf{1}^T\nu=1\\
& A^T\nu=0\\
& \nu\succeq0,
\end{aligned} \tag{5.62}
$$

它是一个熵最大化问题。

<!-- p.268/269 -->

<span style="color:#00838f;">**[例 5.6]** **范数逼近问题。** 考虑无约束范数逼近问题

$$
\text{minimize}\quad \|Ax-b\|, \tag{5.63}
$$

$\|\cdot\|$ 是任意范数。这里拉格朗日对偶函数同样是常数（等于 (5.63) 的最优值），因此无用。

<!-- p.269 -->

再次重新表述问题为

$$
\begin{aligned}
\text{minimize}\quad & \|y\|\\
\text{subject to}\quad & Ax-b=y.
\end{aligned}
$$

按 (5.61)，拉格朗日对偶问题为

$$
\begin{aligned}
\text{maximize}\quad & b^T\nu\\
\text{subject to}\quad & \|\nu\|_*\le1,\quad A^T\nu=0, \tag{5.64}
\end{aligned}
$$

这里用了「范数的共轭是对偶范数单位球的示性函数」（例 3.26，第 93 页）。

引入新等式约束的想法也可以应用于约束函数。例如考虑问题

$$
\begin{aligned}
\text{minimize}\quad & f_0(A_0x+b_0)\\
\text{subject to}\quad & f_i(A_ix+b_i)\le0,\quad i=1,\dots,m,
\end{aligned} \tag{5.65}
$$

$A_i\in\mathbb{R}^{k_i\times n}$、$f_i:\mathbb{R}^{k_i}\to\mathbb{R}$ 凸。（为简单这里不含等式约束。）引入新变量 $y_i\in\mathbb{R}^{k_i}$（$i=0,\dots,m$），把问题重新表述为

$$
\begin{aligned}
\text{minimize}\quad & f_0(y_0)\\
\text{subject to}\quad & f_i(y_i)\le0,\quad i=1,\dots,m\\
& A_ix+b_i=y_i,\quad i=0,\dots,m.
\end{aligned} \tag{5.66}
$$

此问题的拉格朗日函数为

$$
L(x,y_0,\dots,y_m,\lambda,\nu_0,\dots,\nu_m)=f_0(y_0)+\sum_{i=1}^m\lambda_if_i(y_i)+\sum_{i=0}^m\nu_i^T(A_ix+b_i-y_i).
$$

为找对偶函数，对 $x$ 与 $y_i$ 最小化。对 $x$ 的最小值为 $-\infty$，除非

$$
\sum_{i=0}^mA_i^T\nu_i=0,
$$

<!-- p.270 -->

此时对 $\lambda\succ0$ 有

$$
\begin{aligned}
g(\lambda,\nu_0,\dots,\nu_m)&=\sum_{i=0}^m\nu_i^Tb_i+\inf_{y_0,\dots,y_m}\Bigl(f_0(y_0)+\sum_{i=1}^m\lambda_if_i(y_i)-\sum_{i=0}^m\nu_i^Ty_i\Bigr)\\
&=\sum_{i=0}^m\nu_i^Tb_i+\inf_{y_0}\bigl(f_0(y_0)-\nu_0^Ty_0\bigr)+\sum_{i=1}^m\lambda_i\inf_{y_i}\bigl(f_i(y_i)-(\nu_i/\lambda_i)^Ty_i\bigr)\\
&=\sum_{i=0}^m\nu_i^Tb_i-f_0^*(\nu_0)-\sum_{i=1}^m\lambda_if_i^*(\nu_i/\lambda_i).
\end{aligned}
$$

最后的表达式涉及共轭函数的 perspective（透视），因此关于对偶变量凹。最后处理 $\lambda\succeq0$ 但某些 $\lambda_i$ 为零时会发生什么：若 $\lambda_i=0$ 且 $\nu_i\neq0$，则对偶函数为 $-\infty$；若 $\lambda_i=0$ 且 $\nu_i=0$，则涉及 $y_i,\nu_i,\lambda_i$ 的项全为零。因此只要约定 $\lambda_i=0,\nu_i=0$ 时取 $\lambda_if_i^*(\nu_i/\lambda_i)=0$、$\lambda_i=0,\nu_i\neq0$ 时取 $\infty$，上面的 $g$ 表达式对一切 $\lambda\succeq0$ 有效。于是可以把问题 (5.66) 的对偶表达为

$$
\begin{aligned}
\text{maximize}\quad & \sum_{i=0}^m\nu_i^Tb_i-f_0^*(\nu_0)-\sum_{i=1}^m\lambda_if_i^*(\nu_i/\lambda_i)\\
\text{subject to}\quad & \lambda\succeq0\\
& \sum_{i=0}^mA_i^T\nu_i=0. \tag{5.67}
\end{aligned}
$$

<span style="color:#00838f;">**[例 5.7]** **不等式约束的几何规划。** 不等式约束的几何规划

$$
\begin{aligned}
\text{minimize}\quad & \log\Bigl(\sum_{k=1}^{K_0}e^{a_{0k}^Tx+b_{0k}}\Bigr)\\
\text{subject to}\quad & \log\Bigl(\sum_{k=1}^{K_i}e^{a_{ik}^Tx+b_{ik}}\Bigr)\le0,\quad i=1,\dots,m
\end{aligned}
$$

具有 (5.65) 的形式，$f_i:\mathbb{R}^{K_i}\to\mathbb{R}$ 由 $f_i(y)=\log\bigl(\sum_{k=1}^{K_i}e^{y_k}\bigr)$ 给出。此函数的共轭为

$$
f_i^*(\nu)=\begin{cases}\sum_{k=1}^{K_i}\nu_k\log\nu_k & \nu\succeq0,\ \mathbf{1}^T\nu=1\\ \infty & \text{otherwise}.\end{cases}
$$

用 (5.67) 可以立即写出对偶问题：

$$
\begin{aligned}
\text{maximize}\quad & b_0^T\nu_0-\sum_{k=1}^{K_0}\nu_{0k}\log\nu_{0k}+\sum_{i=1}^m\Bigl(b_i^T\nu_i-\sum_{k=1}^{K_i}\nu_{ik}\log(\nu_{ik}/\lambda_i)\Bigr)\\
\text{subject to}\quad & \nu_0\succeq0,\ \mathbf{1}^T\nu_0=1\\
& \nu_i\succeq0,\ \mathbf{1}^T\nu_i=\lambda_i,\quad i=1,\dots,m\\
& \lambda_i\ge0,\quad i=1,\dots,m\\
& \sum_{i=0}^mA_i^T\nu_i=0,
\end{aligned}
$$

进一步化简为

$$
\begin{aligned}
\text{maximize}\quad & b_0^T\nu_0-\sum_{k=1}^{K_0}\nu_{0k}\log\nu_{0k}+\sum_{i=1}^m\Bigl(b_i^T\nu_i-\sum_{k=1}^{K_i}\nu_{ik}\log(\nu_{ik}/\mathbf{1}^T\nu_i)\Bigr)\\
\text{subject to}\quad & \nu_i\succeq0,\quad i=0,\dots,m\\
& \mathbf{1}^T\nu_0=1\\
& \sum_{i=0}^mA_i^T\nu_i=0.
\end{aligned}
$$

<!-- p.271 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.7.2 Transforming the objective（变换目标）</span>

若用 $f_0$ 的递增函数替换目标 $f_0$，所得问题显然等价（见 §4.1.3）。但这个等价问题的对偶可能与原问题的对偶非常不同。

<span style="color:#00838f;">**[例 5.8]** 再次考虑最小范数问题

$$
\text{minimize}\quad \|Ax-b\|,
$$

$\|\cdot\|$ 是某范数。重新表述为

$$
\begin{aligned}
\text{minimize}\quad & (1/2)\|y\|_2^2\\
\text{subject to}\quad & Ax-b=y.
\end{aligned}
$$

这里引入了新变量，并把目标替换为其平方的一半。显然与原问题等价。重新表述的问题的对偶为

$$
\begin{aligned}
\text{maximize}\quad & -(1/2)\|\nu\|_*^2+b^T\nu\\
\text{subject to}\quad & A^T\nu=0,
\end{aligned}
$$

用了事实「$(1/2)\|\cdot\|_2^2$ 的共轭是 $(1/2)\|\cdot\|_*^2$」（例 3.27，第 93 页）。注意这个对偶问题与前面推导的对偶问题 (5.64) 不同。

<!-- p.271/272 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">5.7.3 Implicit constraints（隐式约束）</span>

我们研究的下一个简单重新表述是：把一些约束并入目标函数——修改目标函数使其在约束被违反时为无穷。

<span style="color:#00838f;">**[例 5.9]** **带箱约束的线性规划。** 考虑线性规划

$$
\begin{aligned}
\text{minimize}\quad & c^Tx\\
\text{subject to}\quad & Ax=b\\
& l\preceq x\preceq u
\end{aligned} \tag{5.68}
$$

$A\in\mathbb{R}^{p\times n}$、$l\prec u$。约束 $l\preceq x\preceq u$ 有时称为 box constraints（箱约束）或 variable bounds（变量界）。

当然可以直接推导此线性规划的对偶。对偶将有与等式约束相关联的拉格朗日乘子 $\nu$、与不等式约束 $x\preceq u$ 相关联的 $\lambda_1$、与不等式约束 $l\preceq x$ 相关联的 $\lambda_2$。对偶为

$$
\begin{aligned}
\text{maximize}\quad & -b^T\nu-\lambda_1^Tu+\lambda_2^Tl\\
\text{subject to}\quad & A^T\nu+\lambda_1-\lambda_2+c=0\\
& \lambda_1\succeq0,\quad \lambda_2\succeq0.
\end{aligned} \tag{5.69}
$$

另一种做法：先把问题 (5.68) 重新表述为

$$
\begin{aligned}
\text{minimize}\quad & f_0(x)\\
\text{subject to}\quad & Ax=b, \tag{5.70}
\end{aligned}
$$

其中定义

$$
f_0(x)=\begin{cases}c^Tx & l\preceq x\preceq u\\ \infty & \text{否则}.\end{cases}
$$

<!-- p.272 -->

问题 (5.70) 显然与 (5.68) 等价：我们只是把显式的箱约束变为隐式。

问题 (5.70) 的对偶函数为

$$
g(\nu)=\inf_{l\preceq x\preceq u}\bigl(c^Tx+\nu^T(Ax-b)\bigr)=-b^T\nu-u^T(A^T\nu+c)^-+l^T(A^T\nu+c)^+,
$$

其中 $(y^+)_i=\max\{y_i,0\}$、$(y^-)_i=\max\{-y_i,0\}$。所以这里我们能推导出 $g$ 的解析公式——一个凹的分段线性函数。对偶问题是无约束问题

$$
\text{maximize}\quad -b^T\nu-u^T(A^T\nu+c)^-+l^T(A^T\nu+c)^+,\tag{5.71}
$$

它与原问题的对偶形式相当不同。（问题 (5.69) 与 (5.71) 密切相关，事实上等价；见习题 5.8。）

> <span style="color:#2471a3;">**[译者注｜第一性原理]** §5.7 的教益：**对偶不是问题本身的属性，而是问题表述的属性。** 同一个优化问题，换个等价写法，对偶问题可以一文不值（常数）也可以优雅异常（熵最大化）。建模时要为对偶而建模——这是把凸优化用好的核心技能。</span>

---

> <span style="color:#7f8c8d;">§5.5–5.7 翻译完毕。§5.8–5.9 见 boyd_ch05c.bilingual.md。</span>
