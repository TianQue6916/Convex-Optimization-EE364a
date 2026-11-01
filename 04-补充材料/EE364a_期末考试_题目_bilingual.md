# EE364a《Convex Optimization》期末考试 2008 题目 双语译注

> 本文件为 Stanford EE364a 凸优化课程 2008 年期末考试（Final exam）**题目**部分的中英双语译注，覆盖第 1–9 页。中文为主体，英文术语行内嵌入，首次给出 `English（中文）`，之后仅用 `English`。公式使用 `$...$` 行内与独立 ```math 围栏，不使用美元双符号 display math，不使用 Unicode 上下标。题目层级（Problem / (a) / (b)）与题号严格照原文，不重排、不合并。

## 试卷说明（第 1 页）

**EE364a Convex Optimization I**（凸优化 I），S. Boyd 教授，2008 年 3 月 14–15 日或 3 月 15–16 日，期末考试。

你可以使用任何书籍、笔记或计算机程序（如 Matlab、cvx），但在 3 月 18 日之前、所有人都考完之前，不得与任何人讨论本试卷。唯一例外是可以就疑问通过课程助教邮箱向我们提问。我们已经尽力让试卷无歧义、表述清晰，所以大概率不会多说。

请在交卷前复印一份你的试卷。请把封面页附在试卷最前面。按题目顺序（第 1 题、第 2 题、第 3 题……）装订你的解答，每题另起一页。把与每题相关的所有内容（如文字、代码、绘图）放在一起；不要把代码或绘图附在试卷最后。

我们会扣除那些冗长且不必要复杂、即便答案正确的解法的分数。我们的解答都不长，所以如果你发现某题的解答一页接一页写个没完，应当设法想一个更简单的。我们希望每个人（包括以 Cr/N 计分注册的同学）交上整洁清晰的试卷。

当题目涉及计算时，你必须给出以下全部内容：对你究竟做了什么给出清楚的讨论与论证（justification），产生结果的 Matlab（或其他）源代码，以及最终的数值结果或绘图。

要下载包含题目数据的 Matlab 文件，你需要把题目中给出的完整 URL 键入浏览器；课程网页上没有指向这些文件的链接。例如，要取得名为 `filename.m` 的文件，你要用浏览器获取
`http://www.stanford.edu/class/ee364a/final-data/filename.m`。

请遵守荣誉守则。虽然我们允许你以小组形式完成作业，但在所有人都考完之前，你不能与任何人讨论期末试卷。

所有题目权重相同。有些题比乍一看要简单。请下载最新版本的 cvx。

考试期间务必经常查收邮件，以防我们需要发出重要通知。

---

## Problem 1（第 2 页）

### 1. Optimal investment to fund an expense stream（为一条费用流融资的最优投资）

某组织（如一个市政机构）知道它在未来 $T$ 个周期（period）内的运营费用，记为 $E_1,\dots,E_T$。（通常这些值为正；但也可能有负的 $E_t$，负值对应收入。）这些费用将由两部分的组合来融资：一部分是 $t=0$ 时购入的一篮子债券（bonds）产生的投资收入，另一部分是一个现金账户（cash account）。

债券产生投资收入，记为 $I_1,\dots,I_T$。现金余额记为 $B_0,\dots,B_T$，其中 $B_0\ge 0$ 是初始存入现金账户的金额。对于 $t=1,\dots,T$，我们可以有 $B_t<0$，它表示借款（borrowing）。

在第 $t$ 个周期，用投资收入和现金支付完费用之后，我们手上剩 $B_t-E_t+I_t$ 的现金。若此量为正，则以利率 $r_+>0$ 赚取利息；若为负，则须以利率 $r_-$ 支付利息，其中 $r_-\ge r_+$。于是费用、投资收入和现金余额的递推关系如下：

```math
B_{t+1} =
\begin{cases}
(1+r_+)(B_t - E_t + I_t), & B_t - E_t + I_t \ge 0, \\
(1+r_-)(B_t - E_t + I_t), & B_t - E_t + I_t < 0,
\end{cases}
\quad t = 1,\dots,T-1.
```

我们取 $B_1=(1+r_+)B_0$，并要求 $B_T-E_T+I_T=0$，其含义是最终的现金余额加上收入，恰好覆盖最后一笔费用。

初始投资将是标号为 $1,\dots,n$ 的一篮子债券。债券 $i$ 有价格 $P_i>0$、息票（coupon）支付 $C_i>0$，以及到期日 $M_i\in\{1,\dots,T\}$。债券 $i$ 产生的收入流由下式给出：

```math
a^{(i)}_t =
\begin{cases}
C_i, & t < M_i, \\
C_i + 1, & t = M_i, \\
0, & t > M_i,
\end{cases}
\quad t = 1,\dots,T.
```

若 $x_i$ 是（在 $t=0$ 时）购入债券 $i$ 的份数，则总投资现金流为

```math
I_t = x_1 a^{(1)}_t + \cdots + x_n a^{(n)}_t, \quad t = 1,\dots,T.
```

我们要求 $x_i\ge 0$。（$x_i$ 可以是小数；不必是整数。）

购入债券所需的总初始投资，加上在 $t=0$ 时补足初始现金余额的金额，为 $x_1P_1+\cdots+x_nP_n+B_0$。

**(a)** 解释如何选择 $x$ 和 $B_0$ 来最小化为这条费用流融资所需的总初始投资。

**(b)** 求解 `opt_funding_data.m` 中给定的问题实例。给出 $x$ 和 $B_0$ 的最优值。给出最优的总初始投资，并将其与不购债券（即所有费用全部由现金账户融资）时所需的初始投资作比较。分别画出（最优债券投资下、以及不购债券投资下）现金余额随周期的变化曲线。

---

## Problem 2（第 3 页）

### 2. Utility versus latency trade-off in a network（网络中的效用与延迟权衡）

我们考虑一个有 $m$ 条边（edge，标号 $1,\dots,m$）和 $n$ 条流（flow，标号 $1,\dots,n$）的网络。每条流有一个非负的流速率（flow rate）$f_j$；每条边/链路有一个正的容量（capacity）$c_i$。每条流经过一个固定的链路集合（即其路由 route）；链路 $i$ 上的总流量（traffic）$t_i$ 是所有经过链路 $i$ 的流的流速率之和。流的路由由路由矩阵（routing matrix）$R\in\mathbb{R}^{m\times n}$ 描述，定义为

```math
R_{ij} =
\begin{cases}
1, & \text{流 } j \text{ 经过链路 } i, \\
0, & \text{否则}.
\end{cases}
```

于是链路流量向量 $t\in\mathbb{R}^m$ 由 $t=Rf$ 给出。链路容量约束可表示为 $Rf\preceq c$。（对数）网络效用（utility）定义为

```math
U(f)=\sum_{j=1}^{n}\log f_j.
```

链路 $i$ 上的（平均排队）延迟为

```math
d_i = \frac{1}{c_i - t_i}
```

（再乘一个对我们无影响的常数）。当 $t_i=c_i$ 时，我们取 $d_i=\infty$。流 $j$ 的延迟或时延（latency），记为 $l_j$，是流 $j$ 所经过的所有链路的链路延迟之和。我们定义最大流延迟为

```math
L = \max\{l_1,\dots,l_n\}.
```

给定 $R$ 和 $c$；我们要选择 $f$。

**(a)** 忽略流延迟，你会如何求出使效用 $U$ 最大的流速率？（特别地，我们允许 $L=\infty$。）我们把这一可达到的最大效用记为 $U_{\max}$。

**(b)** 忽略效用，你会如何求出使最大流延迟 $L$ 最小的流速率？（特别地，我们允许 $U=-\infty$。）我们把这一可达到的最小延迟记为 $L_{\min}$。

**(c)** 解释如何求出效用 $U$（我们希望最大化）与延迟 $L$（我们希望最小化）之间的最优权衡（trade-off）。

**(d)** 对 `net_util_data.m` 中数据给出的网络，求出 $U_{\max}$、$L_{\min}$，并画出效用对延迟的最优权衡曲线，在同一张图上标出 $L_{\min}$ 和 $U_{\max}$。你的图应覆盖从 $L=1.1L_{\min}$ 到 $L=11L_{\min}$ 的范围。$U$ 画在纵轴上、用线性刻度（linear scale），$L$ 画在横轴上、用对数刻度（log scale）。

**注。** 对于 (a)、(b)、(c) 三小问，你的回答可以涉及求解一个或多个凸优化问题。但如果存在更简单的解法，你应当说明。

---

## Problem 3（第 4 页）

### 3. Optimal design of a tensile structure（张拉结构的最优设计）

一个张拉结构（tensile structure）被建模为 $\mathbb{R}^2$ 中的 $n$ 个质量块（masses），其中一部分被固定，各质量块由 $N$ 根弹簧（springs）连接。各质量块处于平衡状态，弹簧力、固定质量块的连接力与重力相互平衡。（当质量块的位置使下面定义的总能量最小时，就达到这一平衡。）

我们令 $(x_i,y_i)\in\mathbb{R}^2$ 表示质量块 $i$ 的位置，$m_i>0$ 为其质量值。前 $p$ 个质量块是固定的，即 $x_i=x_i^{\mathrm{fixed}}$ 且 $y_i=y_i^{\mathrm{fixed}}$，对 $i=1,\dots,p$。质量块 $i$ 的重力势能是 $g m_i y_i$，其中 $g\approx 9.8$ 是重力加速度。

假设弹簧 $j$ 连接质量块 $r$ 和 $s$。其弹性势能为

```math
\frac{1}{2}k_j\left((x_r-x_s)^2+(y_r-y_s)^2\right),
```

其中 $k_j\ge 0$ 是弹簧 $j$ 的刚度（stiffness）。

为描述拓扑（topology），即哪些弹簧连接哪些质量块，我们使用关联矩阵（incidence matrix）$A\in\mathbb{R}^{n\times N}$，定义为

```math
A_{ij} =
\begin{cases}
1, & \text{弹簧 } j \text{ 的 head 端连接到质量块 } i, \\
-1, & \text{弹簧 } j \text{ 的 tail 端连接到质量块 } i, \\
0, & \text{否则}.
\end{cases}
```

这里我们为每根弹簧任意选定一个 head 端和一个 tail 端，但实际上弹簧是完全对称的，这种选取可以反过来而不产生任何影响。（希望你会明白为什么用关联矩阵 $A$ 来描述系统的拓扑会很方便。）

总能量是全部质量块的重力势能之和，加上全部弹簧的弹性势能之和。各质量块的平衡位置是使总能量最小、且满足前 $p$ 个位置固定的约束的那个点。（在平衡位置处，每个质量块上的合力为零。）我们令 $E_{\min}$ 表示系统在其平衡位置处的总能量。（我们假设能量是有下界的；这当且仅当每个质量块都通过某个具有正刚度的弹簧集合连接到某个固定质量块时成立。）

总能量 $E_{\min}$ 是结构刚度的一个度量，$E_{\min}$ 越大对应结构越刚。（我们可以把 $E_{\min}=-\infty$ 理解为无限不刚的结构；此时至少有一个质量块甚至无法被支撑以抵抗重力。）

**(a)** 假设我们知道固定位置 $x_1^{\mathrm{fixed}},\dots,x_p^{\mathrm{fixed}}$、$y_1^{\mathrm{fixed}},\dots,y_p^{\mathrm{fixed}}$，质量值 $m_1,\dots,m_n$，弹簧拓扑 $A$，以及常数 $g$。你要选择非负的 $k_1,\dots,k_N$，并满足预算约束 $\mathbf{1}^T k = k_1+\cdots+k_N = k_{\mathrm{tot}}$，其中 $k_{\mathrm{tot}}$ 是给定的。你的目标是最大化 $E_{\min}$。

解释如何用凸优化来做这件事。

---

## Problem 3（续）

### (b)

Carry out your method for the problem data given in `tens_struct_data.m`. This file defines all the needed data, and also plots the equilibrium configuration when the stiffness is evenly distributed across the springs (i.e., $k = (k_{\rm tot}/N)\mathbf{1}$).

> 针对 `tens_struct_data.m` 中给出的问题数据，执行你在 (a) 部分提出的方法。该文件定义了全部所需数据，并且还会画出当 stiffness（刚度）均匀分配到各 springs（弹簧）上（即 $k = (k_{\rm tot}/N)\mathbf{1}$）时的 equilibrium configuration（平衡构型）。

Report the optimal value of $E_{\rm min}$. Plot the optimized equilibrium configuration, and compare it to the equilibrium configuration with evenly distributed stiffness. (The code for doing this is in the file `tens_struct_data.m`, but commented out.)

> 报告 $E_{\rm min}$ 的最优值。画出优化后的 equilibrium configuration（平衡构型），并将它与刚度均匀分布时的平衡构型进行对比。（执行此操作的代码已包含在 `tens_struct_data.m` 文件中，但被注释掉了。）

---

## Problem 4. Identifying a sparse linear dynamical system

> **Problem 4. 识别一个 sparse（稀疏的）linear dynamical system（线性动力系统）**

A linear dynamical system has the form

> 一个 linear dynamical system（线性动力系统）具有如下形式：

```math
x(t+1) = Ax(t) + Bu(t) + w(t), \qquad t = 1, \dots, T-1,
```

where $x(t) \in \mathbb{R}^n$ is the state, $u(t) \in \mathbb{R}^m$ is the input signal, and $w(t) \in \mathbb{R}^n$ is the process noise, at time $t$.

> 其中 $x(t)\in\mathbb{R}^n$ 是 $t$ 时刻的 state（状态），$u(t)\in\mathbb{R}^m$ 是 input signal（输入信号），$w(t)\in\mathbb{R}^n$ 是 process noise（过程噪声）。

We assume the process noises are IID $\mathcal{N}(0, W)$, where $W \succ 0$ is the covariance matrix.

> 我们假设各 process noise（过程噪声）是独立同分布（IID）的 $\mathcal{N}(0, W)$，其中 $W \succ 0$ 是 covariance matrix（协方差矩阵，满足正定）。

The matrix $A \in \mathbb{R}^{n\times n}$ is called the dynamics matrix or the state transition matrix, and the matrix $B \in \mathbb{R}^{n\times m}$ is called the input matrix.

> 矩阵 $A\in\mathbb{R}^{n\times n}$ 称为 dynamics matrix（动力学矩阵）或 state transition matrix（状态转移矩阵），矩阵 $B\in\mathbb{R}^{n\times m}$ 称为 input matrix（输入矩阵）。

You are given accurate measurements of the state and input signal, i.e., $x(1), \dots, x(T)$, $u(1), \dots, u(T-1)$, and $W$ is known. Your job is to find a state transition matrix $\hat{A}$ and input matrix $\hat{B}$ from these data, that are plausible, and in addition are sparse, i.e., have many zero entries. (The sparser the better.)

> 你获得了 state（状态）与 input signal（输入信号）的精确测量值，即 $x(1),\dots,x(T)$、$u(1),\dots,u(T-1)$，且 $W$ 已知。你的任务是从这些数据中找出一个 state transition matrix（状态转移矩阵）$\hat{A}$ 和一个 input matrix（输入矩阵）$\hat{B}$，要求它们 plausible（合理、可信的），并且额外要求它们是 sparse（稀疏）的，即含有许多零元素。（越稀疏越好。）

By doing this, you are effectively estimating the structure of the dynamical system, i.e., you are determining which components of $x(t)$ and $u(t)$ affect which components of $x(t+1)$. In some applications, this structure might be more interesting than the actual values of the (nonzero) coefficients in $\hat{A}$ and $\hat{B}$.

> 通过这样做，你实际上是在估计这个 dynamical system（动力系统）的 structure（结构），也就是在确定 $x(t)$ 与 $u(t)$ 的哪些分量会影响 $x(t+1)$ 的哪些分量。在某些应用中，这种结构可能比 $\hat{A}$、$\hat{B}$ 中（非零）系数的具体数值更有价值。

By plausible, we mean that

> 所谓 plausible（合理、可信），我们指的是：

```math
\sum_{t=1}^{T-1} \left\| W^{-1/2}\bigl(x(t+1) - \hat{A}x(t) - \hat{B}u(t)\bigr) \right\|_2^2
\ \in\ n(T-1) \pm 2\sqrt{2n(T-1)},
```

where $a \pm b$ means the interval $[a-b, a+b]$. (You can just take this as our definition of plausible. But to explain this choice, we note that when $\hat{A} = A$ and $\hat{B} = B$, the left-hand side is $\chi^2$, with $n(T-1)$ degrees of freedom, and so has mean $n(T-1)$ and standard deviation $\sqrt{2n(T-1)}$.)

> 其中 $a\pm b$ 表示区间 $[a-b, a+b]$。（你可以直接把它当作我们对 plausible 的定义。不过为了解释这一选取，我们指出：当 $\hat{A}=A$ 且 $\hat{B}=B$ 时，左边是一个自由度为 $n(T-1)$ 的 $\chi^2$ 分布随机变量，因此其均值为 $n(T-1)$、标准差为 $\sqrt{2n(T-1)}$。）

### (a)

Describe a method for finding $\hat{A}$ and $\hat{B}$, based on convex optimization. We are looking for a very simple method, that involves solving one convex optimization problem. (There are many extensions of this basic method, that would improve the simple method, i.e., yield sparser $\hat{A}$ and $\hat{B}$ that are still plausible. We're not asking you to describe or implement any of these.)

> 描述一种基于 convex optimization（凸优化）来求解 $\hat{A}$ 和 $\hat{B}$ 的方法。我们想要一个非常简单的方法，即只需求解**一个** convex optimization（凸优化）问题即可。（这个基本方法有许多扩展，可以改进这个简单方法，例如在仍保持 plausible 的前提下得到更稀疏的 $\hat{A}$ 和 $\hat{B}$。我们不要求你描述或实现其中任何一个。）

### (b)

Carry out your method on the data found in `sparse_lds_data.m`. Give the values of $\hat{A}$ and $\hat{B}$ that you find, and verify that they are plausible.

> 对 `sparse_lds_data.m` 中的数据执行你的方法。给出你求得的 $\hat{A}$ 和 $\hat{B}$ 的数值，并验证它们是 plausible 的。

In the data file, we give you the true values of $A$ and $B$, so you can evaluate the performance of your method. (Needless to say, you are not allowed to use these values when forming $\hat{A}$ and $\hat{B}$.) Using these true values, give the number of false positives and false negatives in both $\hat{A}$ and $\hat{B}$. A false positive in $\hat{A}$, for example, is an entry that is nonzero, while the corresponding entry in $A$ is zero. A false negative is an entry of $\hat{A}$ that is zero, while the corresponding entry of $A$ is nonzero. To judge whether an entry of $\hat{A}$ (or $\hat{B}$) is nonzero, you can use the test $|\hat{A}_{ij}| \ge 0.01$ (or $|\hat{B}_{ij}| \ge 0.01$).

> 数据文件中给出了 $A$ 和 $B$ 的真实值，因此你可以评估自己方法的性能。（不必多说，在构造 $\hat{A}$ 和 $\hat{B}$ 时不允许使用这些真实值。）利用这些真实值，给出 $\hat{A}$ 和 $\hat{B}$ 中 false positive（假阳性）与 false negative（假阴性）的个数。例如，$\hat{A}$ 中的一个 false positive 是指：该元素非零，而 $A$ 中对应元素为零。一个 false negative 是指：$\hat{A}$ 中某元素为零，而 $A$ 中对应元素非零。判断 $\hat{A}$（或 $\hat{B}$）的某个元素是否非零时，可以用判据 $|\hat{A}_{ij}|\ge 0.01$（或 $|\hat{B}_{ij}|\ge 0.01$）。

---

## Problem 5. Minimum energy processor speed scheduling

> **Problem 5. 最小能量处理器速度调度**

A single processor can adjust its speed in each of $T$ time periods, labeled $1, \dots, T$. Its speed in period $t$ will be denoted $s_t$, $t = 1, \dots, T$. The speeds must lie between given (positive) minimum and maximum values, $S_{\min}$ and $S_{\max}$, respectively, and must satisfy a slew-rate limit, $|s_{t+1} - s_t| \le R$, $t = 1, \dots, T-1$. (That is, $R$ is the maximum allowed period-to-period change in speed.) The energy consumed by the processor in period $t$ is given by $\varphi(s_t)$, where $\varphi : \mathbb{R} \to \mathbb{R}$ is increasing and convex. The total energy consumed over all the periods is $E = \sum_{t=1}^{T} \varphi(s_t)$.

> 一个 single processor（单处理器）可以在 $T$ 个 time period（时间段）中调节自己的 speed（速度），这些时间段记为 $1,\dots,T$。它在第 $t$ 个时间段的速度记为 $s_t$，$t=1,\dots,T$。速度必须落在给定的（正的）最小值与最大值 $S_{\min}$、$S_{\max}$ 之间，并且必须满足 slew-rate limit（摆率限制）$|s_{t+1}-s_t|\le R$，$t=1,\dots,T-1$。（也就是说，$R$ 是相邻两个时间段之间速度允许变化的上限。）处理器在第 $t$ 个时间段消耗的 energy（能量）由 $\varphi(s_t)$ 给出，其中 $\varphi:\mathbb{R}\to\mathbb{R}$ 是 increasing（单调递增）且 convex（凸）的函数。所有时间段消耗的总能量为 $E=\sum_{t=1}^{T}\varphi(s_t)$。

The processor must handle $n$ jobs, labeled $1, \dots, n$. Each job has an availability time $A_i \in \{1, \dots, T\}$, and a deadline $D_i \in \{1, \dots, T\}$, with $D_i \ge A_i$. The processor cannot start work on job $i$ until period $t = A_i$, and must complete the job by the end of period $D_i$. Job $i$ involves a (nonnegative) total work $W_i$. You can assume that in each time period, there is at least one job available, i.e., for each $t$, there is at least one $i$ with $A_i \le t$ and $D_i \ge t$.

> 处理器必须处理 $n$ 个 job（作业），记为 $1,\dots,n$。每个作业有一个 availability time（可用时刻，即最早可开始时间）$A_i\in\{1,\dots,T\}$ 和一个 deadline（截止时刻）$D_i\in\{1,\dots,T\}$，且满足 $D_i\ge A_i$。处理器在时间段 $t=A_i$ 之前不能开始处理作业 $i$，并且必须在第 $D_i$ 个时间段结束前完成该作业。作业 $i$ 涉及（非负的）总工作量 $W_i$。你可以假设每个时间段内至少有一个作业是可用的，即对每个 $t$，至少存在一个 $i$ 满足 $A_i\le t$ 且 $D_i\ge t$。

In period $t$, the processor allocates its effort across the $n$ jobs as $\theta_t$, where $\mathbf{1}^T \theta_t = 1$, $\theta_t \succeq 0$. Here $\theta_{ti}$ (the $i$th component of $\theta_t$) gives the fraction of the processor effort devoted to job $i$ in period $t$.

> 在第 $t$ 个时间段，处理器将其 effort（处理能力/努力）以 $\theta_t$ 分配到 $n$ 个作业上，其中 $\mathbf{1}^T\theta_t = 1$、$\theta_t\succeq 0$。这里 $\theta_{ti}$（$\theta_t$ 的第 $i$ 个分量）给出第 $t$ 个时间段内投入到作业 $i$ 的处理能力所占的比例。

Respecting the availability and deadline constraints requires that $\theta_{ti} = 0$ for $t < A_i$ or $t > D_i$. To complete the jobs we must have

> 遵守 availability（可用性）与 deadline（截止）约束要求：当 $t < A_i$ 或 $t > D_i$ 时，$\theta_{ti}=0$。为完成所有作业，必须满足

```math
\sum_{t=A_i}^{D_i} \theta_{ti} s_t \ge W_i, \qquad i = 1, \dots, n.
```

### (a)

Formulate the problem of choosing the speeds $s_1, \dots, s_T$, and the allocations $\theta_1, \dots, \theta_T$, in order to minimize the total energy $E$, as a convex optimization problem. The problem data are $S_{\min}$, $S_{\max}$, $R$, $\varphi$, and the job data, $A_i$, $D_i$, $W_i$, $i = 1, \dots, n$. Be sure to justify any change of variables, or introduction of new variables, that you use in your formulation.

> 将「选择速度 $s_1,\dots,s_T$ 与分配方案 $\theta_1,\dots,\theta_T$ 以最小化总能量 $E$」这一问题 formulate（表述）为一个 convex optimization（凸优化）问题。问题数据包括 $S_{\min}$、$S_{\max}$、$R$、$\varphi$ 以及作业数据 $A_i$、$D_i$、$W_i$，$i=1,\dots,n$。务必对你所使用的任何 change of variables（变量替换）或 introduction of new variables（引入新变量）给出 justification（论证）。

### (b)

Carry out your method on the problem instance described in `proc_sched_data.m`, with quadratic energy function $\varphi(s_t) = \alpha + \beta s_t + \gamma s_t^2$. (The parameters $\alpha$, $\beta$, and $\gamma$ are given in the data file.) Executing this file will also give a plot showing the availability times and deadlines for the jobs.

> 对 `proc_sched_data.m` 中描述的问题实例执行你的方法，其中 energy function（能量函数）为 quadratic（二次的）$\varphi(s_t)=\alpha+\beta s_t+\gamma s_t^2$。（参数 $\alpha$、$\beta$、$\gamma$ 在数据文件中给出。）执行该文件还会给出一个图，展示各作业的 availability time（可用时刻）与 deadline（截止时刻）。

Give the energy obtained by your speed profile and allocations. Plot these using the command `bar((s*ones(1,n)).*theta,1,'stacked')`, where $s$ is the $T \times 1$ vector of speeds, and $\theta$ is the $T \times n$ matrix of allocations with components $\theta_{ti}$. This will show, at each time period, how much effective speed is allocated to each job. The top of the plot will show the speed $s_t$. (You don't need to turn in a color version of this plot; B&W is fine.)

> 给出你的速度曲线（speed profile）与分配方案所得到的能量值。用命令 `bar((s*ones(1,n)).*theta,1,'stacked')` 绘制这些结果，其中 $s$ 是 $T\times 1$ 的速度向量，$\theta$ 是 $T\times n$ 的分配矩阵、其分量为 $\theta_{ti}$。这将展示在每个时间段内，有多少 effective speed（有效速度）被分配给了每个作业。柱状图顶端将显示速度 $s_t$。（你不必提交这张图的彩色版本；黑白即可。）

---

## Problem 6. Planning production with uncertain demand

> **Problem 6. 需求不确定情形下的生产计划**

You must order (nonnegative) amounts $r_1, \dots, r_m$ of raw materials, which are needed to manufacture (nonnegative) quantities $q_1, \dots, q_n$ of $n$ different products. To manufacture one unit of product $j$ requires at least $A_{ij}$ units of raw material $i$, so we must have $r \succeq Aq$. (We will assume that $A_{ij}$ are nonnegative.) The per-unit cost of the raw materials is given by $c \in \mathbb{R}^m_{+}$, so the total raw material cost is $c^T r$.

> 你必须订购（非负数量的）raw materials（原材料）$r_1,\dots,r_m$，这些原材料用于生产 $n$ 种不同产品的（非负）数量 $q_1,\dots,q_n$。生产一单位产品 $j$ 至少需要 $A_{ij}$ 单位的原材料 $i$，因此必须有 $r\succeq Aq$。（我们假设 $A_{ij}$ 均为非负。）原材料的单位成本由 $c\in\mathbb{R}^m_{+}$ 给出，因此原材料总成本为 $c^T r$。

The (nonnegative) demand for product $j$ is denoted $d_j$; the number of units of product $j$ sold is $s_j = \min\{q_j, d_j\}$. (When $q_j > d_j$, $q_j - d_j$ is the amount of product $j$ produced, but not sold; when $d_j > q_j$, $d_j - q_j$ is the amount of unmet demand.) The revenue from selling the products is $p^T s$, where $p \in \mathbb{R}^n_{+}$ is the vector of product prices. The profit is $p^T s - c^T r$. (Both $d$ and $q$ are real vectors; their entries need not be integers.)

> 产品 $j$ 的（非负）demand（需求量）记为 $d_j$；产品 $j$ 售出的数量为 $s_j = \min\{q_j, d_j\}$。（当 $q_j > d_j$ 时，$q_j - d_j$ 是产品 $j$ 生产了但未售出的数量；当 $d_j > q_j$ 时，$d_j - q_j$ 是未满足的需求量。）销售产品所得 revenue（收入）为 $p^T s$，其中 $p\in\mathbb{R}^n_{+}$ 是产品价格向量。profit（利润）为 $p^T s - c^T r$。（$d$ 与 $q$ 都是实向量；其分量不必是整数。）

You are given $A$, $c$, and $p$. The product demand, however, is not known. Instead, a set of $K$ possible demand vectors, $d^{(1)}, \dots, d^{(K)}$, with associated probabilities $\pi_1, \dots, \pi_K$, is given. (These satisfy $\mathbf{1}^T \pi = 1$, $\pi \succeq 0$.)

> 你已知 $A$、$c$ 和 $p$。然而产品需求是未知的。取而代之，给定了 $K$ 个可能的需求向量 $d^{(1)},\dots,d^{(K)}$ 以及相应的概率 $\pi_1,\dots,\pi_K$。（它们满足 $\mathbf{1}^T\pi = 1$、$\pi\succeq 0$。）

You will explore two different optimization problems that arise in choosing $r$ and $q$ (the variables).

> 你将探讨在选择 $r$ 与 $q$（这些是变量）时出现的两个不同的优化问题。

**I. Choose $r$ and $q$ ahead of time.** You must choose $r$ and $q$, knowing only the data listed above. (In other words, you must order the raw materials, and commit to producing the chosen quantities of products, before you know the product demand.) The objective is to maximize the expected profit.

> **情形 I. 提前选择 $r$ 与 $q$。** 你必须在仅知道上述数据的情况下选择 $r$ 和 $q$。（换句话说，你必须先订购原材料，并承诺生产选定数量的产品，之后才知晓产品需求。）目标是最大化 expected profit（期望利润）。

**II. Choose $r$ ahead of time, and $q$ after $d$ is known.** You must choose $r$, knowing only the data listed above. Some time after you have chosen $r$, the demand will become known to you. This means that you will find out which of the $K$ demand vectors is the true demand. Once you know this, you must choose the quantities to be manufactured. (In other words, you must order the raw materials before the product demand is known; but you can choose the mix of products to manufacture after you have learned the true product demand.) The objective is to maximize the expected profit.

> **情形 II. 提前选择 $r$，在 $d$ 已知后再选择 $q$。** 你必须在仅知道上述数据的情况下选择 $r$。在选择 $r$ 之后一段时间，需求将变得可知，即你会知道 $K$ 个需求向量中哪一个才是真实需求。一旦知晓，就必须选择要生产的数量。（换句话说，你必须在产品需求已知之前订购原材料；但可以在得知真实产品需求之后再选择生产的产品组合。）目标同样是最大化 expected profit（期望利润）。

### (a)

Explain how to formulate each of these problems as a convex optimization problem. Clearly state what the variables are in the problem, what the constraints are, and describe the roles of any auxiliary variables or constraints you introduce.

> 解释如何将这两个问题各自 formulate（表述）为 convex optimization（凸优化）问题。清楚说明问题中的 variables（变量）是什么、constraints（约束）是什么，并描述你所引入的任何 auxiliary variables（辅助变量）或辅助约束的作用。

### (b)

Carry out the methods from part (a) on the problem instance with numerical data given in `planning_data.m`. This file will define $A$, $D$, $K$, $c$, $m$, $n$, $p$ and $\pi$. The $K$ columns of $D$ are the possible demand vectors. For both of the problems described above, give the optimal value of $r$, and the expected profit.

> 对 `planning_data.m` 中给出的、带数值数据的问题实例执行 (a) 部分的方法。该文件将定义 $A$、$D$、$K$、$c$、$m$、$n$、$p$ 和 $\pi$。$D$ 的 $K$ 个列就是所有可能的需求向量。对于上述两个问题，分别给出 $r$ 的最优值以及 expected profit（期望利润）。

---

## Problem 7. Dual of exponential cone

> **Problem 7. exponential cone（指数锥）的对偶**

The exponential cone $K_{\rm exp} \subseteq \mathbb{R}^3$ is defined as

> exponential cone（指数锥）$K_{\rm exp}\subseteq\mathbb{R}^3$ 定义为：

```math
K_{\rm exp} = \left\{ (x, y, z) \mid y > 0,\; y e^{x/y} \le z \right\}.
```

Express the dual cone in the form

> 将 dual cone（对偶锥）表示成如下形式：

```math
K_{\rm exp}^{*} = \left\{ (u, v, w) \mid \text{... your conditions here ...} \right\}.
```

Your conditions must be short (certainly no more than one line) and cannot involve any variables other than $u$, $v$, and $w$.

> 你给出的条件必须简短（一定不超过一行），并且不能涉及除 $u$、$v$、$w$ 之外的任何变量。

We are not worried here about the fine details of what happens on the boundaries of these cones, so you really needn't worry about it. But we make some comments here for those who do care about such things.

> 这里我们并不关心这些锥的边界上发生的细节，所以你实在不必为此担心。但为了那些在意此类细节的人，我们在此作一些说明。

The cone $K_{\rm exp}$ as defined above is not closed. To obtain its closure, we need to add the points

> 上面定义的锥 $K_{\rm exp}$ 不是 closed（闭）的。为了得到它的 closure（闭包），需要加入以下点集：

```math
\left\{ (x, y, z) \mid x \le 0,\; y = 0,\; z \ge 0 \right\}.
```

(This makes no difference, since the dual of a cone is equal to the dual of its closure.)

> （这不会造成任何差别，因为一个锥的 dual（对偶）等于其 closure（闭包）的对偶。）
