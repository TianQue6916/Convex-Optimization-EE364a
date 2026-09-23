# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

> <span style="color:#7f8c8d;">Source: SEE复习课/review4_single.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

<!-- p.1 -->
## <span style="color:#2471a3;">**[lecture]**</span> <span style="color:#c0392b;">EE364a Review Session 4（EE364a 复习课第 4 次）</span>

本次复习课的大纲（session outline）共三个主题：

- **Transformations（变换）**
- **Dual problem（对偶问题）**
- **Homework hints（作业提示）**

<!-- p.2 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">变换（Transformations）</span>

<span style="color:#2471a3;">**[section]**</span> 第一部分复习**等价变换**（transformation）：在不改变问题本质的前提下，把目标或约束改写成凸优化更容易处理的形式。两条路线分别是：

- **transformation of objective（目标函数的变换）**；
- **transformation of constraints（约束的变换）**。

下面用几个例子依次演示。

<!-- p.3 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">例：目标函数的变换（Objective Transformation）</span>

<span style="color:#2471a3;">**[example]**</span> **例：目标函数的变换。** 考虑如下优化问题：

```math
\begin{array}{ll}
\text{minimize} & \displaystyle\int_{-\infty}^{c^T x} \frac{1}{\sqrt{2\pi}}\, e^{t^2/2}\, \mathrm{d}t \\[6pt]
\text{subject to} & Ax \preceq b \\
& Hx = g
\end{array}
```

- **Is it a convex problem?（它是凸问题吗？）**
- **Is it a quasiconvex problem?（它是拟凸问题吗？）**

<!-- p.4 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">例（续）：目标函数变换的解答</span>

> <span style="color:#2471a3;">**[solution]**</span> **解答（solution）：**
>
> - **非凸（nonconvex）**：目标函数不是凸函数；
>
> - **拟凸（quasiconvex）**：下水平集（sublevel sets）是凸集。<span style="color:#7f8c8d;">（原 slide 印作 "quasiconcave"，但"下水平集为凸"刻画的正是拟凸性 quasiconvex；事实上 $\Phi(c^Tx)$ 关于 $x$ 单调、既拟凸也拟凹，此处按论证逻辑取拟凸，按笔误处理。）</span>
>
> - 记 $\Phi$ 为标准正态分布的累积分布函数，则 $\int_{-\infty}^{c^T x}\frac{1}{\sqrt{2\pi}}\,e^{t^2/2}\,\mathrm{d}t \;=\; \Phi(c^T x)$，其中 $\Phi(u)$ 关于 $u$ 单调递增（monotone increasing），所以最小化 $\Phi(c^T x)$ 与最小化 $c^T x$ 是同一个问题；
>
> - 因此，等价问题（equivalent problem）是一个线性规划（LP）：
>
> ```math
> \begin{array}{ll}
> \text{minimize} & c^T x \\
> \text{subject to} & Ax \preceq b \\
> & Hx = g
> \end{array}
> ```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这道题的全部机关在于"单调复合不改变最优解集"这一保序事实。$\Phi$ 正是你在 Bertsekas 概率论里熟悉的标准正态累积分布函数（CDF）：$Z\sim N(0,1)$ 时 $\Phi(u)=P(Z\le u)$，严格递增、值域 $(0,1)$。严格递增意味着大小关系被原样保留：$c^Tx\le c^Ty\iff\Phi(c^Tx)\le\Phi(c^Ty)$，于是两个目标函数的 argmin 完全相同，只是"纵轴刻度"被非线性压扁。从第一性原理看，优化问题里"解"比"值"更本质：把目标复合任何严格递增函数，可行集与最优解集都不动，但最优值会变，凸性也通常被破坏——S 形的 $\Phi$ 复合仿射映射后不凸不凹，所以"最小化"方向非凸。而拟凸性按定义只问下水平集 $\{x:\phi(x)\le\alpha\}$ 是否为凸集：单调递增复合把下水平集变成半空间 $\{x:c^Tx\le\Phi^{-1}(\alpha)\}$，天然是凸的。两问于是同时有答案：非凸、但拟凸，且等价于一个 LP。以后拿到变换题，先问"目标里藏没藏单调复合"，这条检查应成为肌肉记忆。

<!-- p.5 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">例：把 max 约束变为线性约束（Linearize a max Constraint）</span>

<span style="color:#2471a3;">**[example]**</span> **例：把下面的约束变换成一组线性约束（a set of linear constraints）：**

```math
a_1^T x + b_1 + \max\left(a_2^T x + b_2,\; a_3^T x + b_3\right) \le 0
```

<!-- p.6 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">例（续）：两种线性化解法</span>

> <span style="color:#2471a3;">**[solution]**</span> **解法一（solution 1）：**
>
> - 引入新变量 $t$；
> - 于是原约束等价于：
>
> ```math
> \begin{aligned}
> a_1^T x + b_1 + t &\le 0 \\
> a_2^T x + b_2 - t &\le 0 \\
> a_3^T x + b_3 - t &\le 0
> \end{aligned}
> ```
>
> <span style="color:#2471a3;">**[solution]**</span> **解法二（solution 2）：**
>
> - 把 $a_1^T x + b_1$ 放进 $\max$ 函数内部；
> - 于是得到：
>
> ```math
> \begin{aligned}
> (a_1 + a_2)^T x + (b_1 + b_2) &\le 0 \\
> (a_1 + a_3)^T x + (b_1 + b_3) &\le 0
> \end{aligned}
> ```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 为什么带加号的 $\max$ 能线性化、带减号的不行？关键在凸性的"方向"。逐点最大值 $\max(g_1,g_2)$（$g_i$ 为仿射函数）是凸函数，其**上图（epigraph）** $\{(x,t): t\ge g_1(x),\ t\ge g_2(x)\}$ 是几个半空间的交，天然是凸集；所以 "$a_1^Tx+b_1+\max\le 0$" 可无损改写为 "$a_1^Tx+b_1+t\le 0$ 且 $t\ge a_i^Tx+b_i$"——解法一引入辅助变量 $t$，本质就是给上图立一个坐标替身。解法二则利用 $\max$ 对加法的分配律 $c+\max(u,v)=\max(c+u,\,c+v)$，把常数项吸进 $\max$ 后直接拆成两个不等式，连辅助变量都省掉。反过来，下一页的 "$-\max$" 要求 $a_1^Tx+b_1\ge\max_i(a_i^Tx+b_i)$，等价于把 $\max$ 的**下图（hypograph）**压进凸集约束，而逐点最大值的下图一般不是凸集，凸优化的语言表达不了。这与课本里"逐点 max 保凸、逐点 min 不保凸"的不对称一致；你以后在半定规划里见到的 $\lambda_{\max}(A)=\sup_{\|u\|_2=1}u^TAu$ 也是同一构造——取大产生凸性，取小产生凹性，方向感是凸分析的第一直觉。

<!-- p.7 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">例：带 −max 的约束（A Constraint with −max）</span>

<span style="color:#2471a3;">**[example]**</span> **例：那下面这个约束呢（what about the following constraint）？**

```math
a_1^T x + b_1 - \max\left(a_2^T x + b_2,\; a_3^T x + b_3\right) \le 0
```

> <span style="color:#2471a3;">**[solution]**</span> **解答（solution）：**
>
> - 这是一个**非凸约束（non-convex constraint）**；
> - 它不能被变换为一组线性不等式；
> - 考虑如下问题：
>
> ```math
> \begin{array}{ll}
> \text{minimize} & c^T x \\
> \text{subject to} & \bar{A}x - \bar{b} \preceq 0 \\
> & Hx - g = 0 \\
> & a_1^T x + b_1 - \max\left(a_2^T x + b_2,\; a_3^T x + b_3\right) \le 0
> \end{array}
> ```
>
> 这不是一个 LP，但可以通过求解两个 LP 轻松解决（solved easily by solving two LPs）。

<!-- p.8 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">例（续）：拆成两个 LP 求解</span>

> <span style="color:#2471a3;">**[solution]**</span> **解答（续）：** 考虑最后一个不等式（the last inequality）：
>
> - 若 $a_2^T x + b_2 \ge a_3^T x + b_3$，则该不等式变成 $a_1^T x + b_1 - a_2^T x - b_2 \le 0$；
> - 若 $a_2^T x + b_2 \le a_3^T x + b_3$，则该不等式变成 $a_1^T x + b_1 - a_3^T x - b_3 \le 0$。
>
> 因此，最优解可以通过求解下面两个 LP 找到：
>
> ```math
> \begin{array}{ll}
> \text{minimize} & c^T x \\
> \text{subject to} & \bar{A}x - \bar{b} \preceq 0 \\
> & Hx - g = 0 \\
> & a_2^T x + b_2 \ge a_3^T x + b_3 \\
> & a_1^T x + b_1 - a_2^T x - b_2 \le 0
> \end{array}
> ```
>
> 以及
>
> ```math
> \begin{array}{ll}
> \text{minimize} & c^T x \\
> \text{subject to} & \bar{A}x - \bar{b} \preceq 0 \\
> & Hx - g = 0 \\
> & a_2^T x + b_2 \le a_3^T x + b_3 \\
> & a_1^T x + b_1 - a_3^T x - b_3 \le 0
> \end{array}
> ```
>
> 最后，在两个最优解中取目标函数值更小者即可。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> "解两个 LP 再取较小目标值"是精确算法而非启发式，理由是一个干净的覆盖论证：任何可行 $x$ 都满足 $a_2^Tx+b_2\ge a_3^Tx+b_3$ 与 $a_2^Tx+b_2\le a_3^Tx+b_3$ 之一，故至少落在某个 LP 的可行集内；在所属分支上，非凸约束恰好等价于对应的线性不等式，而两个 LP 的目标函数与原问题相同，因此两个 LP 最优值取 $\min$ 就是原问题最优值，取到的解对原问题也可行。几何上，这是用超平面 $a_2^Tx+b_2=a_3^Tx+b_3$ 把可行域切成两半、分而治之。从 6.006 的视角看，这是把一个（当前形式下）没有多项式时间全局解法保证的非凸问题，拆成有限个多项式时间可解的子问题（LP 可用内点法在多项式时间内求解），与分支定界（branch and bound）的骨架同构。代价也要心里有数：$\max$ 里若换成 $k$ 个仿射项取最大，分支数就是 $2^{k-1}$，指数爆炸；所以此技巧只适合"段数极少"的分段结构，段一多就该改用混合整数规划或凸松弛。

<!-- p.9 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">求对偶问题（Finding the Dual Problem）</span>

<span style="color:#2471a3;">**[section]**</span> 第二部分复习"从原问题到对偶问题"的标准流程：

- **primal problem（原问题）**：

```math
\begin{array}{ll}
\text{minimize} & f_0(x) \\
\text{subject to} & f(x) \preceq 0 \\
& h(x) = 0
\end{array}
```

<span style="color:#7f8c8d;">（原 slide 此行印作 maximize；但下文拉格朗日函数取 $L=f_0+\lambda^Tf+\nu^Th$、对偶函数对 $x$ 取 $\inf$、且乘子约定 $\lambda\succeq 0$，这些都与最小化形式的符号约定配套，故按笔误处理为 minimize。）</span>

- **Lagrangian（拉格朗日函数）**：$L(x,\lambda,\nu) = f_0(x) + \lambda^T f(x) + \nu^T h(x)$；
- **Lagrange dual function（拉格朗日对偶函数）**：<span style="color:#00838f;">$g(\lambda,\nu) = \inf_x\, L(x,\lambda,\nu)$</span>；
- **dual problem（对偶问题）**：

```math
\begin{array}{ll}
\text{maximize} & g(\lambda, \nu) \\
\text{subject to} & \lambda \succeq 0
\end{array}
```

<!-- p.10 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">例：熵最大化（Entropy Maximization）</span>

<span style="color:#2471a3;">**[example]**</span> **例：熵最大化（entropy maximization）。**

```math
\begin{array}{ll}
\text{minimize} & \displaystyle f_0(x) = \sum_{i=1}^n x_i \log x_i \\[4pt]
\text{subject to} & Ax \preceq b \\
& \mathbf{1}^T x = 1
\end{array}
```

> <span style="color:#2471a3;">**[solution]**</span> **解答（solution）：**
>
> - 写下拉格朗日函数（Lagrangian）：
>
> $L(x,\lambda,\nu) = f_0(x) + \lambda^T(Ax - b) + \nu(\mathbf{1}^T x - 1)$
>
> - 求拉格朗日对偶函数（Lagrange dual function）：
>
> ```math
> \begin{aligned}
> g(\lambda,\nu) &= \inf_x\, \Big\{ f_0(x) + \lambda^T(Ax - b) + \nu(\mathbf{1}^T x - 1) \Big\} \\
> &= -b^T\lambda - \nu - \sup_x\, \Big\{ (-A^T\lambda - \nu\mathbf{1})^T x - f_0(x) \Big\} \\
> &= -b^T\lambda - \nu - f_0^*\!\left(-A^T\lambda - \nu\mathbf{1}\right),
> \end{aligned}
> ```
>
> 其中 $f_0^*(y) = \sup_x\,\left(y^T x - f_0(x)\right) = \sum_{i=1}^n e^{y_i - 1}$（$f_0$ 的共轭函数）。

<!-- p.11 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">例（续）：对偶问题及其化简</span>

> <span style="color:#2471a3;">**[solution]**</span> **解答（续）：**
>
> - 于是我们得到对偶问题（dual problem）：
>
> ```math
> \begin{array}{ll}
> \text{maximize} & \displaystyle -b^T\lambda - \nu - \sum_{i=1}^n e^{-a_i^T\lambda - \nu - 1} \\[6pt]
> \text{subject to} & \lambda \succeq 0
> \end{array}
> ```
>
> - 为化简，在 $\nu$ 上最小化 $g(\lambda,\nu)$（即取 $\nu^\star = \log\!\left(\sum_{i=1}^n e^{-a_i^T\lambda}\right) - 1$）：
>
> ```math
> \begin{array}{ll}
> \text{maximize} & \displaystyle -\log\!\left(\sum_{i=1}^n e^{-a_i^T\lambda}\right) - b^T\lambda \\[6pt]
> \text{subject to} & \lambda \succeq 0
> \end{array}
> ```
>
> - 最终我们得到**凸形式（convex form）的几何规划（GP）**。（为什么？why?）

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这两页演示了求对偶最常用的"共轭函数套路"，值得背成模板：把 $g=\inf_x L$ 中所有含 $x$ 的项整理成 $\sup_x\{y^Tx-f_0(x)\}$，这正是**共轭函数（conjugate function）** $f_0^*(y)$ 的定义取负，于是 $g=-b^T\lambda-\nu-f_0^*(-A^T\lambda-\nu\mathbf{1})$——对偶函数就是"共轭函数的平移加线性项"。历史上，Legendre 于 1788 年在分析力学中引入 Legendre 变换，Fenchel 在 1949 年把它推广到一般凸函数，合称 Legendre–Fenchel 变换；对偶函数恒为凹函数，根源在于它是一族仿射函数对 $x$ 取 inf。熵函数的共轭 $f_0^*(y)=\sum_i e^{y_i-1}$ 里那个 $-1$ 来自一阶条件 $\log x_i+1=y_i\Rightarrow x_i=e^{y_i-1}$，并非魔法常数。概率论连接：$\sum_i e^{y_i}$ 这种"exp 乘线性再求和"的结构与矩母函数 $M_X(t)=E[e^{tX}]$ 同源，这正是指数族分布的最大似然估计与凸对偶互为表里的原因。最后"对 $\nu$ 先最小化消元"是在乘子层面做分块消元：等式约束只通过 $\nu$ 进入对偶函数，解析求出 $\nu^\star$ 后双变量降为单变量——处理对偶的常用顺序是先易后难、逐个消元。

<!-- p.12 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">作业提示：P4.29（Homework Hints: P4.29）</span>

<span style="color:#2471a3;">**[exercise]**</span> **Homework hints（作业提示）· P4.29**

- 如何处理 $\mathrm{prob}(c^T x \ge \alpha)$（机会约束，chance constraint）？
  参见鲁棒 LP 例题（robust LP example）中的做法——经由 SOCP（二阶锥规划）的随机方法（stochastic approach via SOCP）：

```math
\mathrm{prob}(c^T x \ge \alpha) \;=\; 1 - \Phi\!\left(\frac{\alpha - \bar{c}^T x}{\sqrt{x^T \Sigma x}}\right)
```

- 如何处理非凸/非凹（nonconvex/nonconcave）的目标函数？
  $\Phi(u)$ 关于 $u$ 单调递增，所以对目标函数做变换，即可把问题变成拟凸问题（quasiconvex problem）。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这条公式是"把随机问题翻译成二阶锥"的标准样本，值得逐步拆开。设 $c=\bar c+\Sigma^{1/2}u$、$u\sim N(0,I)$，则 $c^Tx$ 服从均值 $\bar c^Tx$、方差 $x^T\Sigma x$ 的高斯分布；对高斯随机变量做标准化 $Z=(X-\mu)/\sigma$（概率论最早学的基本操作），尾概率即 $P(c^Tx\ge\alpha)=1-\Phi\!\big((\alpha-\bar c^Tx)/\sqrt{x^T\Sigma x}\big)$。要求该概率不超过 $\eta$，利用 $\Phi$ 严格递增可反解出 $\bar c^Tx+\Phi^{-1}(1-\eta)\sqrt{x^T\Sigma x}\le\alpha$；注意 $\sqrt{x^T\Sigma x}=\|\Sigma^{1/2}x\|_2$ 是范数，且 $\Phi^{-1}(1-\eta)>0$，于是约束恰为二阶锥（SOC）约束——这正是提示中 "via SOCP" 的全部含义。目标函数一侧同理：$\Phi$ 单调递增，换成其单调变形即得拟凸问题，配合二分法调用 SOCP 可行性判定即可求解。整个流程把"概率论的高斯尾概率"译成"凸优化的锥约束"，是机会约束规划的入口，也再次呼应本讲主线：单调复合只换外衣、不换最优解。

<!-- p.13 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">作业提示：FIR 滤波器设计（FIR Filter Design）</span>

<span style="color:#2471a3;">**[exercise]**</span> **FIR filter design（FIR 滤波器设计；FIR 即 finite impulse response，有限脉冲响应）**

- 如何表示 (b) 问中的目标 $\omega_c$（截止频率）？
  回忆 HW3 中的**近似宽度问题（approximation width problem）**：

```math
W(x) = \sup\Big\{\, T \;\Big|\; \big| x_1 f_1(t) + \cdots + x_n f_n(t) - f_0(t) \big| \le \epsilon \ \text{ for } \ 0 \le t \le T \,\Big\}
```

- 如何表示 (c) 问中的目标 $N$（滤波器长度）？
  先用系数 $a_i$ 把长度为 $N$ 的滤波器表达出来（express the filter of length $N$ in terms of coefficients $a_i$），然后套用上面关于 $\omega_c$ 的提示（apply the hint for $\omega_c$）。
