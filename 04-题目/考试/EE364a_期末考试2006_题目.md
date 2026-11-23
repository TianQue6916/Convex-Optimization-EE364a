# EE364a《Convex Optimization》期末考试 2006 题目 双语译注

> 本文件为题目原文的**行内双语译注**，覆盖全部 12 页（Problem 1–8）。中文为主体，关键术语保留英文原词并首次给出 `English（中文）` 形式，之后直接使用 `English`。题目层级（Problem / (a) / (i)）与题号严格照原文，不重排、不合并。

## 试卷说明（第 1 页）

**EE364 Convex Optimization**（凸优化）
Prof. S. Boyd（Stephen Boyd 教授）
2006 年 6 月 7–8 日 或 6 月 8–9 日。
**Final exam**（期末考试）

你可以使用任何书籍、笔记或计算机程序（例如 Matlab、cvx），但在 6 月 10 日、也就是所有人都考完之前，不得与任何人讨论试卷内容。唯一的例外是你可以向助教（TAs）或 Stephen Boyd 本人寻求澄清。我们已经尽了很大努力让试卷没有歧义、表述清晰，所以很可能不会再多说。请把邮件咨询发送到 `ee364-spr0506-staff@lists.stanford.edu`。

请在交卷前复制一份你的答卷。
请把封面页（cover page）附在答卷最前面。将你的解答按顺序装订（problem 1、problem 2、problem 3、……），每道题另起一页。

由于这是一份 take home exam（带回家完成的考试），你的得分既看准确性、正确性，也看清晰度与简洁度。对于冗长、不必要地复杂的解答——即便答案正确——我们也会扣分。我们的参考答案都不长，所以如果你发现某道题的解答写了好几页，就应该试着找一个更简单的做法。我们期望每个人交上整洁、清晰可读的答卷，包括以 Cr/N（credit/no credit，及格制）注册的同学。

当一道题涉及计算时，你必须给出以下全部内容：对你具体所做事情的清晰讨论与论证、产生结果的 Matlab（或其他）源代码，以及最终的数值结果或图像。

有些题要求你下载一个包含题目数据的 Matlab 文件。你必须把题目中给出的完整 URL 手动输入浏览器；课程网页上没有指向这些文件的链接。例如，要获取名为 `data.m` 的文件，你需要在浏览器里输入 `www.stanford.edu/class/ee364/data.m`。

请遵守 honor code（荣誉准则）。虽然我们允许你在作业上以小组形式合作，但你不能与任何人讨论期末考试，至少在所有人都考完之前不能。

所有题目权重相同。有些题比它们乍看起来要简单（所以今年有 8 道题……）。

**非常重要**：考试中你会用到 cvx。务必确保你已经下载并安装了最新版本的 cvx。我们已经尽力保证期末考试期间不会冒出任何 cvx 的 bug，但这种事情当然仍可能发生。如果你写了一段自认为应该能跑通的 cvx 代码却跑不通，不要犹豫，直接邮件联系我们核实。另外，考试期间请经常查收邮件，以防我们确实需要发出重要通知。

（页码：1）

---

## Problem 1 — Optimizing processor speed（优化处理器速度）

一组 $n$ 个任务（tasks）要由 $n$ 个处理器（processors）完成。需要选取的变量是处理器速度 $s_1,\dots,s_n$，它们必须介于给定的最小值 $s_{\min}$ 与最大值 $s_{\max}$ 之间。任务 $i$ 的计算负载（computational load）是 $\alpha_i$，因此完成任务 $i$ 所需的时间是 $\tau_i = \alpha_i / s_i$。

处理器 $i$ 消耗的功率（power）由 $p_i = f(s_i)$ 给出，其中 $f : \mathbb{R} \to \mathbb{R}$ 是正值、递增（increasing）且凸（convex）的函数。因此，消耗的总能量（total energy）为

```math
E = \sum_{i=1}^{n} \frac{\alpha_i}{s_i} \, f(s_i).
```

（这里我们忽略了在处理器之间传输数据所消耗的能量，并假设处理器在不活跃时会被断电。）

任务之间存在一组 precedence constraints（优先约束/前驱约束），它是一个由 $m$ 个有序对（ordered pairs）组成的集合 $P \subseteq \{1,\dots,n\}\times\{1,\dots,n\}$。若 $(i,j)\in P$，则任务 $j$ 在任务 $i$ 完成之前不能开始。（例如，当任务 $j$ 需要用到任务 $i$ 中计算出的数据时，就会出现这种情况。）当 $(i,j)\in P$ 时，我们称任务 $i$ 是任务 $j$ 的一个 precedent（前驱），因为它必须先于任务 $j$。我们假设这些优先约束定义了一个 directed acyclic graph（DAG，有向无环图），当 $(i,j)\in P$ 时，就有一条从 $i$ 到 $j$ 的边。

如果一个任务没有前驱，那么它在时刻 $t=0$ 开始。否则，每个任务在其所有前驱都完成之后立即开始。我们用 $T$ 表示所有任务全部完成所需的时间。

为了把优先约束说清楚，我们看下面这个很小的例子：$n=6$ 个任务、$m=6$ 条优先约束。

```math
P = \{(1,4),\ (1,3),\ (2,3),\ (3,6),\ (4,6),\ (5,6)\}.
```

（此处原图为 6 个节点的 DAG：节点 1→3、1→4、2→3、3→6、4→6、5→6。）

在这个例子里，任务 1、2、5 在时刻 $t=0$ 开始（因为它们没有前驱）。任务 1 在 $t=\tau_1$ 完成，任务 2 在 $t=\tau_2$ 完成，任务 5 在 $t=\tau_5$ 完成。任务 3 以任务 1 和 2 为前驱，所以它在时刻 $t=\max\{\tau_1,\tau_2\}$ 开始，并在 $\tau_3$ 秒后、即 $t=\max\{\tau_1,\tau_2\}+\tau_3$ 时刻结束。任务 4 在时刻 $t=\tau_1+\tau_4$ 完成。任务 6 在任务 3、4、5 都完成时开始，即时刻 $t=\max\{\max\{\tau_1,\tau_2\}+\tau_3,\ \tau_1+\tau_4,\ \tau_5\}$，并在 $\tau_6$ 秒后完成。在这个例子里，任务 6 是最后一个完成的任务，所以我们有

```math
T = \max\{\max\{\tau_1,\tau_2\}+\tau_3,\ \tau_1+\tau_4,\ \tau_5\} + \tau_6.
```

（页码：2）

---

## Problem 1 (a)

在给定速度上下限之间选取处理器速度，以在能量约束 $E \le E_{\max}$ 下最小化完成时间 $T$，把这个问题表述（formulate）成一个凸优化问题（convex optimization problem）。本题的数据是 $P,\ s_{\min},\ s_{\max},\ \alpha_1,\dots,\alpha_n,\ E_{\max}$ 以及函数 $f$；变量是 $s_1,\dots,s_n$。

你可以自由地变换变量或引入新变量。务必清楚地说明为什么你的表述是凸的，以及为什么它与上面的问题陈述等价（equivalent）。

**重要**：

- 你的表述必须对**任何**正值、递增且凸的函数 $f$ 都成立为凸问题，不能再对 $f$ 附加任何进一步假设。
- 本题针对一般情形，而非上面描述的小例子。

## Problem 1 (b)

考虑由 `ps_data.m` 给出数据的具体实例，处理器功率为

```math
f(s) = 1 + s + s^2 + s^3.
```

优先约束由一个 $m\times 2$ 矩阵 `prec` 给出，其中 $m$ 是优先约束的条数，每一行给出一条优先约束（第一列给出前驱）。

画出能量 $E$ 对时间 $T$ 的最优权衡曲线（optimal trade-off curve），$T$ 的范围从其最小值延伸到其最大可能值。（这两个端点分别对应所有处理器都以 $s_{\max}$ 和 $s_{\min}$ 运行的情形，因为 $T$ 关于 $s$ 是单调非增的（monotone nonincreasing）。）在同一张图上，画出当所有处理器以相同速度 $\bar{s}$ 运行、且 $\bar{s}$ 从 $s_{\min}$ 变化到 $s_{\max}$ 时所得到的能量—时间权衡。

**注**：本题这一部分不像 (a) 那样对 $E$ 有 $E_{\max}$ 的上限；你要找的是 $E$ 对 $T$ 的最优权衡。

（页码：3）

---

## 关键概念术语表（首现定义）

- **convex function（凸函数）**：函数 $f$ 满足对任意 $x,y$ 与 $\lambda\in[0,1]$ 有 $f(\lambda x+(1-\lambda)y)\le \lambda f(x)+(1-\lambda)f(y)$；等价地，其 epigraph（上方图）是凸集。本题中功率函数 $f$ 即要求为凸。
- **convex optimization problem（凸优化问题）**：目标函数与不等式约束函数均为凸、等式约束为仿射的最小化问题，其任一局部最优即全局最优。
- **directed acyclic graph（DAG，有向无环图）**：不含环的有向图，用于刻画优先约束的偏序结构，保证任务调度可行且 $T$ 良定义。
- **precedence constraint（优先约束/前驱约束）**：任务之间“必须先于”的先后关系，$(i,j)\in P$ 表示 $i$ 必须先于 $j$ 完成。
- **trade-off curve（权衡曲线）**：以某个参数（此处为时间 $T$）为横轴、目标（能量 $E$）最优值为纵轴的曲线，刻画两个目标之间的取舍关系。

---

## Problem 2 — Exploring nearly optimal points（探索近最优点的集合）

一个优化算法在问题 feasible（可行）的前提下会找到该问题的一个 optimal point（最优点）。探索 nearly optimal points（近最优点）的集合往往很有用。当一个问题具有 strong minimum（强最小值）时，其近最优点集合很小，所有这样的点都靠近最初找到的那个最优点。另一个极端是，一个问题可以具有 soft minimum（软最小值），这意味着存在许多 feasible 且 objective value（目标值）近乎最优的点，其中有些点离最初找到的最优点相当远。本题中你将用一个典型方法来探索近最优点集合。

我们先求给定问题的最优值 $p^{\star}$：

```math
\begin{array}{ll}
\text{minimize} & f_0(x) \\
\text{subject to} & f_i(x) \le 0,\quad i = 1,\dots,m \\
& h_i(x) = 0,\quad i = 1,\dots,p,
\end{array}
```

以及一个 optimal point $x^{\star} \in \mathbb{R}^n$。然后我们取一个小的正数 $\epsilon$ 和一个向量 $c \in \mathbb{R}^n$，求解问题

```math
\begin{array}{ll}
\text{minimize} & c^T x \\
\text{subject to} & f_i(x) \le 0,\quad i = 1,\dots,m \\
& h_i(x) = 0,\quad i = 1,\dots,p \\
& f_0(x) \le p^{\star} + \epsilon.
\end{array}
```

注意，该问题的任何 feasible point 对原问题而言都是 $\epsilon$-suboptimal（$\epsilon$-次优）的。用不同的 $c$ 多次求解该问题，会产生（可能不同的）$\epsilon$-suboptimal 点。如果问题有 strong minimum，这些点会彼此靠近；如果问题有 weak minimum，它们可能差异很大。

在这些实验中，选择 $c$ 有不同的策略。最简单的是随机选择 $c$；另一种方法是选择形如 $c = \pm e_i$（$i = 1,\dots,n$）的 $c$。（这一方法给出 $x$ 的每个分量在 $\epsilon$-suboptimal 集合上的 range（取值范围）。）

你将对下面这个问题执行这一方法，以判断它具有 strong minimum 还是 weak minimum。你可以随机生成向量 $c$，采样数量要足够多以得出结论。你可以取 $\epsilon = 0.01\, p^{\star}$，这意味着我们在考虑 1% suboptimal 点的集合。

该问题是一个在 $\mathbb{R}^2$ 中运动的运载工具（vehicle）的 minimum fuel optimal control（最小燃料最优控制）问题。在时刻 $kh$ 的位置记为 $p(k) \in \mathbb{R}^2$，速度记为 $v(k) \in \mathbb{R}^2$，$k = 1,\dots,K$。这里 $h > 0$ 是采样周期（sampling period）。它们满足方程

```math
p(k+1) = p(k) + h\, v(k),
```

```math
v(k+1) = (1-\alpha)\, v(k) + (h/m)\, f(k),
\qquad k = 1,\dots,K-1,
```

其中 $f(k) \in \mathbb{R}^2$ 是在时刻 $kh$ 施加到运载工具上的力，$m > 0$ 是运载工具质量，$\alpha \in (0,1)$ 用于刻画运载工具受到的 drag（阻力）：在没有任何其他力时，运载工具的速度在每个离散时间区间内按因子 $1-\alpha$ 衰减。

（页码：4）

---

## Problem 2（续）

（这些公式是对那些涉及矩阵指数（matrix exponentials）的更精确公式的近似。）

力来自两个 thruster（推进器）以及重力：

```math
f(k) =
\begin{bmatrix} \cos\theta_1 \\ \sin\theta_1 \end{bmatrix} u_1(k)
+
\begin{bmatrix} \cos\theta_2 \\ \sin\theta_2 \end{bmatrix} u_2(k)
+
\begin{bmatrix} 0 \\ -mg \end{bmatrix},
\qquad k = 1,\dots,K-1.
```

这里 $u_1(k) \in \mathbb{R}$ 和 $u_2(k) \in \mathbb{R}$ 是（非负的）thruster 力的大小，$\theta_1$ 与 $\theta_2$ 是 thrust 力的方向，$g = 10$ 是重力加速度常数。

总燃料消耗为

```math
F = \sum_{k=1}^{K-1} \big(u_1(k) + u_2(k)\big).
```

（回忆 $u_1(k) \ge 0$，$u_2(k) \ge 0$。）

问题是在初始条件 $p(1) = 0$、$v(1) = 0$ 以及 way-point constraints（航路点约束）

```math
p(k_i) = w_i,
\qquad i = 1,\dots,M
```

（它们表述：在时刻 $h k_i$，运载工具必须经过位置 $w_i \in \mathbb{R}^2$）之下最小化燃料消耗。

此外，我们要求运载工具始终停留在一个方形工作区域内：

```math
\| p(k) \|_{\infty} \le P^{\max},
\qquad k = 1,\dots,K.
```

本题两部分都涉及由 `thrusters_data.m` 给出数据的具体问题实例。

**Problem 2 (a)**　找到一条 optimal trajectory（最优轨迹）以及相应的最小燃料消耗 $p^{\star}$。在 $\mathbb{R}^2$（即 $p_1, p_2$ 平面）中画出轨迹 $p(k)$。验证它经过了各个航路点。

**Problem 2 (b)**　用上面描述的一般方法生成若干条 1% suboptimal 的轨迹，并在 $\mathbb{R}^2$ 中画出相应的轨迹。你会认为该问题具有 strong minimum 还是 weak minimum？

（页码：5）

---

## Problem 3 — Estimating a vector with unknown nonlinear measurement nonlinearity（在测量非线性未知的情形下估计一个向量）

我们想估计一个向量 $x \in \mathbb{R}^n$，给定若干测量

```math
y_i = \varphi(a_i^T x + v_i),
\qquad i = 1,\dots,m.
```

这里 $a_i \in \mathbb{R}^n$ 已知，$v_i$ 是 IID（独立同分布）的 $\mathcal{N}(0,\sigma^2)$ 随机噪声，$\varphi : \mathbb{R} \to \mathbb{R}$ 是一个未知的单调递增函数，已知对所有 $u$ 满足

```math
\alpha \le \varphi'(u) \le \beta,
```

（这里 $\alpha$ 和 $\beta$ 是已知的正常数，且 $\alpha < \beta$。）给定 $y_i$，我们想求 $x$ 与 $\varphi$ 的 maximum likelihood estimate（最大似然估计，MLE）。（我们还知道 $a_i$、$\sigma$、$\alpha$ 与 $\beta$。）

这听起来像一个无限维问题，因为我们要估计的参数中有一个是函数。事实上，我们只需要知道 $m$ 个数

```math
z_i = \varphi^{-1}(y_i),
\qquad i = 1,\dots,m.
```

所以，所谓估计 $\varphi$，其实就是估计这 $m$ 个数 $z_1,\dots,z_m$。（这些数不是任意的；它们必须与先验信息“对所有 $u$ 都有 $\alpha \le \varphi'(u) \le \beta$”相容。）

**Problem 3 (a)**　解释如何用 convex optimization 求出 $x$ 与 $\varphi$（即 $z_1,\dots,z_m$）的最大似然估计。

**Problem 3 (b)**　在 `nonlin_meas_data.m` 给出的数据上执行你的方法，该数据包含一个矩阵 $A \in \mathbb{R}^{m \times n}$，其行为 $a_1^T,\dots,a_m^T$。给出 $\hat{x}^{\text{ml}}$，即 $x$ 的最大似然估计。画出你估计出的函数 $\hat{\varphi}^{\text{ml}}$。（你可以通过把 $(\hat{z}^{\text{ml}})_i$ 对 $y_i$ 作图来完成，其中 $y_i$ 画在纵轴上、$(\hat{z}^{\text{ml}})_i$ 画在横轴上。）

**Hint（提示）**　你可以假设测量已按 $y_i$ 非递减排序编号，即 $y_1 \le y_2 \le \cdots \le y_m$。（第 (b) 部分问题实例中给出的数据就按此顺序排列。）

（页码：6）

---

## Problem 4 — Optimizing rates and time slot fractions（优化速率与时间片比例）

我们考虑一个使用 time-domain multiple access（TDMA，时分多址）来支持 $n$ 条通信流（communication flows）的无线系统。这些流具有（非负的）速率 $r_1,\dots,r_n$，单位为 bits/sec（比特/秒）。要在流 $i$ 上支持速率 $r_i$，需要发射功率（transmitter power）

```math
p = a_i \big(e^{b r} - 1\big),
```

其中 $b$ 是一个（已知的）正常数，$a_i$ 是与接收机 $i$ 的噪声功率和增益相关的（已知）正常数。

TDMA 的工作原理如下。时间被划分为若干固定时长 $T$（秒）的周期（periods）。每个这样长度为 $T$ 的周期又被划分为 $n$ 个时间片（time-slots），时长分别为 $t_1,\dots,t_n$，且必须满足 $t_1 + \cdots + t_n = T$、$t_i \ge 0$。在时间片 $i$ 内，通信流 $i$ 以瞬时速率 $r = T r_i / t_i$ 发射，从而在每个长度为 $T$ 的周期内，流 $i$ 有 $T r_i$ 比特被发射出去。时间片 $i$ 内所需功率为 $a_i (e^{b T r_i / t_i} - 1)$，因此每个长度为 $T$ 的周期内的平均发射功率为

```math
P = \frac{1}{T} \sum_{i=1}^{n} a_i\, t_i \big(e^{b T r_i / t_i} - 1\big).
```

当 $t_i = 0$ 时，我们约定：若 $r_i > 0$ 则 $P = \infty$，若 $r_i = 0$ 则 $P = 0$。（后者对应“零流量、同时也为零时间分配”的情形。）

问题是求速率 $r \in \mathbb{R}^n$ 与时间片时长 $t \in \mathbb{R}^n$，在 $P \le P^{\max}$ 的约束下最大化 log utility function（对数效用函数）

```math
U(r) = \sum_{i=1}^{n} \log r_i.
```

（这一效用函数常被用来保证 fairness（公平性）：每条通信流至少获得某个正的速率。）问题数据是 $a_i$、$b$、$T$ 和 $P^{\max}$；变量是 $t_i$ 和 $r_i$。

**Problem 4 (a)**　把该问题 formulate（表述）成一个 convex optimization problem。如有需要，可以自由引入新变量或变换变量。务必论证你的表述中目标函数与约束函数的凸性。

**Problem 4 (b)**　给出你表述的 optimality conditions（最优性条件）。我们当然更偏好简单的最优性条件而非复杂的。**注**：我们不期望你求解这些最优性条件；你可以把它们给成一组方程（以及可能的不等式）。

**Hint（提示）**　采用 log utility function 时，我们不可能有 $r_i = 0$，因此也不可能有 $t_i = 0$；因此约束 $r_i \ge 0$ 与 $t_i \ge 0$ 不会是 active（起作用的）或 tight（紧的）。这将允许你简化最优性条件。

（页码：7）

---

## 关键概念术语表（续）

- **feasible（可行）**：一个点若满足问题的全部约束（不等式与等式），则称其为 feasible。
- **optimal point（最优点）**：在所有 feasible 点中使目标函数取得最优值的点；本卷中即最小化问题的最优解 $x^{\star}$。
- **strong / soft minimum（强 / 软最小值）**：近最优点集合小、且都靠近最优解时称为 strong minimum；存在大量远离最优解却仍近乎最优的可行点时称为 soft minimum。
- **$\epsilon$-suboptimal（$\epsilon$-次优）**：一个可行点若其目标值 $f_0(x) \le p^{\star} + \epsilon$（即与最优值相差不超过 $\epsilon$），则称为 $\epsilon$-suboptimal。
- **convex optimization problem（凸优化问题）**：目标函数与不等式约束函数均为 convex（凸）、等式约束为 affine（仿射）的最小化问题；其任一局部最优即全局最优。
- **maximum likelihood estimate（MLE，最大似然估计）**：使观测数据出现的似然函数取得最大值的参数估计。
- **TDMA（time-domain multiple access，时分多址）**：把时间划分为周期、再把每个周期切分为时间片，各通信流在各自时间片内独占信道进行传输的多址接入方式。

---

## Problem 5 — Feature selection and sparse linear separation（特征选择与稀疏线性可分）

设 $x^{(1)},\dots,x^{(N)}$ 与 $y^{(1)},\dots,y^{(M)}$ 是 $\mathbb{R}^n$ 中给定的两个非空向量集合（或类别），并且它们能被一个超平面（hyperplane）严格分离（strictly separated），即存在 $a\in\mathbb{R}^n$ 与 $b\in\mathbb{R}$ 使得

```math
a^T x^{(i)} - b \ge 1, \qquad i = 1,\dots,N,
```

```math
a^T y^{(i)} - b \le -1, \qquad i = 1,\dots,M.
```

这意味着这两个类别被 slab（板片/带层）

```math
S = \{ z \mid |a^T z - b| \le 1 \}
```

（弱）分离（weakly separated），该 slab 的厚度（thickness）为 $2/\|a\|_2$。你可以把 $x^{(i)}$ 与 $y^{(i)}$ 的分量看作特征（features）；$a$ 与 $b$ 定义一个仿射函数（affine function），它把这些特征组合起来，使我们得以区分这两个类别。

为了找到分离这两个类别的最厚的 slab，我们可以求解 QP（quadratic program，二次规划）

```math
\begin{aligned}
&\text{minimize} && \|a\|_2 \\
&\text{subject to} && a^T x^{(i)} - b \ge 1, \qquad i = 1,\dots,N \\
&&& a^T y^{(i)} - b \le -1, \qquad i = 1,\dots,M,
\end{aligned}
```

其中变量为 $a\in\mathbb{R}^n$ 与 $b\in\mathbb{R}$。（这与 (8.23) 式、第 424 页 §8.6.1 给出的问题等价；另见 exercise 8.23。）

在本题中，我们要寻找 $(a,b)$，它用一个较厚的 slab 分离两个类别，同时又是稀疏（sparse）的，即存在许多 $j$ 使得 $a_j = 0$。注意若 $a_j = 0$，则仿射函数 $a^T z - b$ 不依赖于 $z_j$，也就是说第 $j$ 个特征没有被用于完成分类。因此，一个稀疏的 $a$ 对应一个简约（parsimonious）的分类函数：它只依赖少数几个特征。所以我们的目标是找到一个仿射分类函数，它给出一个较厚的分离 slab，同时又使用尽可能少的特征来完成分类。

这通常是一个困难的组合式（combinatorial）双准则（bi-criterion）优化问题，所以我们采用标准的启发式（heuristic）方法，求解

```math
\begin{aligned}
&\text{minimize} && \|a\|_2 + \lambda \|a\|_1 \\
&\text{subject to} && a^T x^{(i)} - b \ge 1, \qquad i = 1,\dots,N \\
&&& a^T y^{(i)} - b \le -1, \qquad i = 1,\dots,M,
\end{aligned}
```

其中 $\lambda \ge 0$ 是一个权重（weight），它控制分离 slab 厚度与（通过 $\ell_1$ 范数间接体现的）$a$ 的稀疏性之间的权衡（trade-off）。

在 `sp_ln_sp_data.m` 中获取数据，它分别以矩阵 $X$ 与 $Y$ 的列给出 $x_i$ 与 $y_i$。求最大厚度分离 slab 的厚度。在合适的范围内（我们推荐用对数间隔 log spacing）对大约 100 个 $\lambda$ 值求解上面的问题。对每个值，记录分离 slab 厚度 $2/\|a\|_2$ 以及 $\operatorname{card}(a)$，即 $a$ 的基数（cardinality，非零元素的个数）。在计算基数时，若某个元素 $a_j$ 满足 $|a_j| \le 10^{-4}$，则可把它计为零。将这些数据绘成图，纵轴为 slab 厚度、横轴为基数。

（页码：8）

---

## Problem 5（续，第 9 页）

用这些数据从数据中的 50 个特征里选出一组 10 个特征。给出你所选特征的索引（indices）。

在这里你可以有几组不同的特征可选；你只需选一组即可。然后求只使用所选特征的最大厚度分离 slab。（这是标准做法：一旦选定了你要使用的特征，就只用这些特征、并且不再带 $\ell_1$ 正则化（regularization）地重新优化一次。）

（页码：9）

---

## Problem 6 — Bounding object position from multiple camera views（从多相机视图界定物体位置）

一个小物体位于未知位置 $x\in\mathbb{R}^3$，被一组 $m$ 个相机（cameras）观测。我们的目标是找到 $\mathbb{R}^3$ 中的一个盒子（box）

```math
B = \{ z \in \mathbb{R}^3 \mid l \preceq z \preceq u \},
```

使得我们能够保证 $x \in B$。我们希望这个 bounding box（包围盒）尽可能小。（虽然无关紧要，我们可以在各盒子之间用体积 volume 来评判“最小”。）

现在描述这些相机。位于位置 $x\in\mathbb{R}^3$ 的物体在相机 $i$ 的像平面（image plane）上于位置

```math
v_i = \frac{1}{c_i^T x + d_i} (A_i x + b_i) \in \mathbb{R}^2
```

处成像。矩阵 $A_i \in \mathbb{R}^{2\times 3}$、向量 $b_i\in\mathbb{R}^2$ 与 $c_i\in\mathbb{R}^3$、以及实数 $d_i\in\mathbb{R}$ 都是已知的，它们依赖于相机的位置与朝向（orientations）。我们假设 $c_i^T x + d_i > 0$。$3\times 4$ 矩阵

```math
P_i =
\begin{bmatrix}
A_i & b_i \\
c_i^T & d_i
\end{bmatrix}
```

称为（相机 $i$ 的）camera matrix（相机矩阵）。经常（但并不总是）有这种情况：$P_i$ 的前 3 列（即 $A_i$ 叠放在 $c_i^T$ 之上）构成一个正交矩阵（orthogonal matrix），此时该相机称为 orthographic（正交投影/正交的）。

我们并不能直接得到像点 $v_i$；我们只知道它所在的（方形）像素（pixel）。换句话说，相机给我们一个测量值 $\hat{v}_i$（像点所在像素的中心），并且我们保证

```math
\| v_i - \hat{v}_i \|_{\infty} \le \rho_i / 2,
```

其中 $\rho_i$ 是相机 $i$ 的像素宽度（也是高度）。（关于 $v_i$ 我们一无所知；它可以是这个像素中的任意一点。）

给定数据 $A_i, b_i, c_i, d_i, \hat{v}_i, \rho_i$，我们要找到保证包含 $x$ 的最小盒子 $B$（即求向量 $l$ 与 $u$）。换句话说，找到 $\mathbb{R}^3$ 中包含所有与相机观测相一致的点的最小盒子。

**(a)** 解释如何用凸（convex）或拟凸（quasiconvex）优化来求解此问题。你必须解释你所用的任何变换、所引入的任何新变量等。如果你表述中任何函数的凸性或拟凸性不是显然的，务必加以论证。

**(b)** 求解文件 `camera_data.m` 中给出的具体问题实例。务必让你的最终数值答案（即 $l$ 与 $u$）突出显示。

（页码：10）

---

## Problem 7 — $\ell_{1.5}$ optimization（$\ell_{1.5}$ 优化）

同时使用 $\ell_2$ 范数（或其平方）与 $\ell_1$ 范数的优化与逼近方法，目前在统计、机器学习以及信号与图像处理中非常流行。例子包括 Huber estimation（Huber 估计）、LASSO、basis pursuit（基追踪）、SVM、各种 $\ell_1$ 正则化分类方法、total variation denoising（全变分去噪）等。粗略地说，$\ell_2$ 范数对应（平方的）欧氏距离，或对应 Gaussian（高斯）分布的负对数似然（negative log-likelihood）；相比之下，$\ell_1$ 范数给出“鲁棒”（robust）逼近，即降低对离群点（outliers）的敏感度，并且也倾向于产生稀疏解（稀疏性是针对范数的自变量而言的）。（以上都只是背景；你不必了解这些也能解这道题。）

在本题中，我们研究一种混合这两个范数的自然方法，即使用 $\ell_{1.5}$ 范数，其定义为

```math
\| z \|_{1.5} = \left( \sum_{i=1}^{k} |z_i|^{3/2} \right)^{2/3}
```

对 $z\in\mathbb{R}^k$ 成立。我们将考虑最简单的逼近或回归（regression）问题：

```math
\text{minimize} \quad \| A x - b \|_{1.5},
```

其中变量为 $x\in\mathbb{R}^n$，问题数据为 $A\in\mathbb{R}^{m\times n}$ 与 $b\in\mathbb{R}^m$。我们假设 $m > n$ 且 $A$ 列满秩（full rank，即秩为 $n$）。期望这个 $\ell_{1.5}$ 最优逼近问题能兼具 $\ell_2$ 逼近与 $\ell_1$ 逼近的一些优良性质。

**(a)** 给出该问题的最优性条件（optimality conditions）。尽量使它们简单。你的解应具有“$x$ 是 $\ell_{1.5}$ 范数逼近问题的最优解，当且仅当……”的形式。

**(b)** 解释如何把 $\ell_{1.5}$ 范数逼近问题表述（formulate）为一个 SDP（semidefinite program，半定规划）。（你的 SDP 可以包含线性等式与不等式约束。）

**(c)** 求解由下列代码生成的具体数值实例：

```matlab
randn('state',0);
A=randn(100,30);
b=randn(100,1);
```

数值地验证最优性条件。给出残差（residuals）的直方图（histogram），并对 $\ell_2$ 范数与 $\ell_1$ 范数逼近重复此操作。你可以用任何你喜欢的方法求解该问题（但当然必须解释你是如何做的）；特别地，你不必使用 (b) 部分得到的 SDP 表述。

（页码：11）

---

## Problem 8 — Three-way linear classification（三路线性分类）

给定数据

```math
x^{(1)},\dots,x^{(N)}, \qquad y^{(1)},\dots,y^{(M)}, \qquad z^{(1)},\dots,z^{(P)},
```

它们是 $\mathbb{R}^n$ 中三个非空向量集合。我们希望在 $\mathbb{R}^n$ 上找到三个仿射函数

```math
f_i(z) = a_i^T z - b_i, \qquad i = 1,2,3,
```

它们满足以下性质：

```math
f_1(x^{(j)}) > \max\{ f_2(x^{(j)}), f_3(x^{(j)}) \}, \qquad j = 1,\dots,N,
```

```math
f_2(y^{(j)}) > \max\{ f_1(y^{(j)}), f_3(y^{(j)}) \}, \qquad j = 1,\dots,M,
```

```math
f_3(z^{(j)}) > \max\{ f_1(z^{(j)}), f_2(z^{(j)}) \}, \qquad j = 1,\dots,P.
```

用文字表述：$f_1$ 在 $x$ 数据点上是三个函数中最大的，$f_2$ 在 $y$ 数据点上是三个函数中最大的，$f_3$ 在 $z$ 数据点上是三个函数中最大的。我们可以给出一个简单的几何解释：函数 $f_1$、$f_2$、$f_3$ 把 $\mathbb{R}^n$ 划分（partition）成三个区域，

```math
R_1 = \{ z \mid f_1(z) > \max\{ f_2(z), f_3(z) \} \},
```

```math
R_2 = \{ z \mid f_2(z) > \max\{ f_1(z), f_3(z) \} \},
```

```math
R_3 = \{ z \mid f_3(z) > \max\{ f_1(z), f_2(z) \} \},
```

它们由“每个函数在何处是三个中最大者”来定义。我们的目标是找到满足 $x^{(j)}\in R_1$、$y^{(j)}\in R_2$、$z^{(j)}\in R_3$ 的函数。

把这个问题表述（pose）成一个凸优化问题（convex optimization problem）。在你的表述中**不得使用严格不等式**（strict inequalities）。

（页码：12）

---

## 关键概念术语表（续）

- **hyperplane（超平面）**：$\mathbb{R}^n$ 中形如 $\{z \mid a^T z = b\}$ 的 $n-1$ 维仿射集合，其中 $a\neq 0$；$a$ 是其法向量。本题中 $a^T z - b = \pm 1$ 给出分离 slab 的两个边界超平面。
- **slab（板片/带层）**：夹在两个平行超平面 $\{z \mid a^T z - b = -1\}$ 与 $\{z \mid a^T z - b = 1\}$ 之间的区域 $\{z \mid |a^T z - b| \le 1\}$；其厚度为两平面间距离 $2/\|a\|_2$。
- **QP（quadratic program，二次规划）**：目标函数为（凸）二次、约束为线性的凸优化问题。本题求最厚 slab 即求解以 $\|a\|_2$ 为目标（等价于 $\|a\|_2^2$）的 QP。
- **affine function（仿射函数）**：形如 $f(z)=a^T z - b$ 的函数，它既凸又凹；其图像是超平面，其水平集为超平面，构成线性分类器的基本形式。
- **sparse（稀疏）**：向量中大多数分量为零的性质；$a$ 稀疏意味着分类函数只用少数特征。
- **cardinality（基数）**：向量中非零分量的个数，记作 $\operatorname{card}(a)$；它是稀疏性的直接度量，但作为函数是非凸的，故常用 $\ell_1$ 范数作其凸代理（convex surrogate）。
- **$\ell_1$ norm（$\ell_1$ 范数）**：$\|a\|_1=\sum_j |a_j|$；它是凸函数，且作为基数的最小化代理，倾向于诱导稀疏解。
- **orthogonal matrix（正交矩阵）**：满足 $Q^T Q = I$ 的方阵 $Q$，其各列两两正交且模长为 1，保持欧氏距离（等距）。
- **convex（凸）**：一个集合 $C$ 是凸集，若对任意 $x,y\in C$ 与 $\theta\in[0,1]$ 有 $\theta x+(1-\theta)y\in C$；一个函数 $f$ 是凸函数，若其定义域为凸集且对任意 $x,y$、$\theta\in[0,1]$ 有 $f(\theta x+(1-\theta)y)\le \theta f(x)+(1-\theta)f(y)$。凸优化问题中任一局部最优即全局最优。
- **quasiconvex（拟凸）**：函数 $f$ 满足其所有子水平集（sublevel sets）$\{z \mid f(z)\le \alpha\}$ 均为凸集；比凸性弱，但许多分式（如相机投影的分式线性形式）是拟凸的，可用二分法（bisection）求解其最优化。
- **SDP（semidefinite program，半定规划）**：在“矩阵变量半正定”这一线性矩阵不等式（LMI）约束下最小化线性目标的凸优化问题；其可行集是半正定锥与仿射空间的交，是凸集。
- **optimality condition（最优性条件）**：判别可行解为最优解的充要条件；对光滑凸问题通常表现为梯度为零（无约束）或满足 KKT 条件（有约束），对不可微凸问题则表现为“$0$ 属于目标函数的 subdifferential（次微分）”。
- **residual（残差）**：观测值与拟合值之差 $Ax-b$ 的各分量；其范数即为逼近的误差度量，直方图用于观察误差分布与对离群点的鲁棒性。
- **strict inequality（严格不等式）**：形如 $>$ 或 $<$ 的不等式；凸优化问题中不允许使用严格不等式，因为严格不等式界定的集合不是闭集、最优值可能不可达，需改写为带 $\epsilon$ 余量的非严格形式（如 $\ge$）。
