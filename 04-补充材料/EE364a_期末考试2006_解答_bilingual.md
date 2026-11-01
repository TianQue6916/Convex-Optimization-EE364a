# EE364a《Convex Optimization》期末考试 2006 解答 双语译注

> 说明：本文为 Stanford EE364a 凸优化课程 2006 年期末考试（Final exam）**解答**的中文双语译注，覆盖全部 34 页（Problem 1–8）。中文为主体，英文术语以 `English（中文）` 形式首次行内嵌入，之后仅保留英文；公式使用 `$...$` 行内与独立 ```math 围栏，不使用美元双符号 display math，不使用 Unicode 上下标。题目层级（Problem / (a) / (i)）严格照原文保留。

## Problem 1（第 1 题）

**Optimizing processor speed（处理器速度优化）。** 有 $n$ 个任务（tasks），要由 $n$ 个处理器（processors）完成。需要选择的变量是处理器速度 $s_1,\dots,s_n$，它们必须落在给定的最小值 $s_{\min}$ 与最大值 $s_{\max}$ 之间。任务 $i$ 的计算负载（computational load）为 $\alpha_i$，因此完成任务 $i$ 所需的时间为 $\tau_i=\alpha_i/s_i$。

处理器 $i$ 消耗的功率（power）为 $p_i=f(s_i)$，其中 $f:\mathbb{R}\to\mathbb{R}$ 是正的、递增的、凸的函数（positive, increasing, and convex）。因此，总能量（total energy）为

```math
E=\sum_{i=1}^{n}\frac{\alpha_i}{s_i}f(s_i).
```

（这里我们忽略处理器之间传输数据所消耗的能量，并假设处理器在不活跃时会被断电。）

任务之间有一组优先级约束（precedence constraints），它是 $m$ 个有序对的集合

```math
\mathcal{P}\subseteq\{1,\dots,n\}\times\{1,\dots,n\}.
```

若 $(i,j)\in\mathcal{P}$，则任务 $j$ 必须等到任务 $i$ 完成后才能开始。（例如，当任务 $j$ 需要任务 $i$ 计算出的数据时就是这种情况。）当 $(i,j)\in\mathcal{P}$ 时，我们称任务 $i$ 是任务 $j$ 的先行任务（a precedent of task $j$），因为它必须先于任务 $j$ 完成。我们假设优先级约束定义了一个有向无环图（directed acyclic graph, DAG），当 $(i,j)\in\mathcal{P}$ 时存在一条从 $i$ 到 $j$ 的边。

若一个任务没有先行任务，则它在 $t=0$ 时刻开始。否则，每个任务在其所有先行任务都完成后立即开始。我们用 $T$ 表示所有任务全部完成所需的时间。

为确保优先级约束清晰，我们考虑下面这个很小的例子：$n=6$ 个任务，$m=6$ 条优先级约束。

```math
\mathcal{P}=\{(1,4),\,(1,3),\,(2,3),\,(3,6),\,(4,6),\,(5,6)\}.
```

```
1 → 3 → 6
  ↘     ↑
    → 4 ↗
2 → 3
5 → 6
```

在这个例子中，任务 1、2、5 在 $t=0$ 时刻开始（因为它们没有先行任务）。任务 1 在 $t=\tau_1$ 完成，任务 2 在 $t=\tau_2$ 完成，任务 5 在 $t=\tau_5$ 完成。任务 3 以任务 1 和 2 为先行任务，所以它在 $t=\max\{\tau_1,\tau_2\}$ 时刻开始，并在 $\tau_3$ 秒之后、即 $t=\max\{\tau_1,\tau_2\}+\tau_3$ 时刻结束。任务 4 在 $t=\tau_1+\tau_4$ 时刻完成。任务 6 在任务 3、4、5 都完成后开始，即 $t=\max\{\max\{\tau_1,\tau_2\}+\tau_3,\ \tau_1+\tau_4,\ \tau_5\}$ 时刻。它在 $\tau_6$ 秒之后完成。在这个例子中，任务 6 是最后一个完成的任务，因此有

```math
T=\max\{\max\{\tau_1,\tau_2\}+\tau_3,\ \tau_1+\tau_4,\ \tau_5\}+\tau_6.
```

**(a)** 将“在给定界限内选择处理器速度、以最小化完成时间 $T$、并受能量约束 $E\le E_{\max}$ 限制”的问题，表述为一个凸优化问题（convex optimization problem）。本题的数据是 $\mathcal{P}$、$s_{\min}$、$s_{\max}$、$\alpha_1,\dots,\alpha_n$、$E_{\max}$ 以及函数 $f$；变量是 $s_1,\dots,s_n$。

你可以自由地换变量或引入新变量。务必清楚地解释为什么你的表述是凸的，以及为什么它与上面的问题陈述等价。

**重要说明：**
- 你的表述必须对**任何**正的、递增的、凸的函数 $f$ 都是凸的。你**不能**对 $f$ 做进一步的假设。
- 本题针对一般情形，而不是上面描述的那个小例子。

**(b)** 考虑由 `ps_data.m` 给出的具体数据实例，以及处理器功率

```math
f(s)=1+s+s^2+s^3.
```

优先级约束由一个 $m\times 2$ 的矩阵 `prec` 给出，其中 $m$ 是优先级约束的条数，每一行给出一条优先级约束（第一列给出先行任务）。

画出能量 $E$ 对时间 $T$ 的最优权衡曲线（optimal trade-off curve），其中 $T$ 的取值范围从其最小值延伸到其最大可能值。（这两个极值分别出现在所有处理器都以 $s_{\max}$ 和 $s_{\min}$ 运行时，因为 $T$ 关于 $s$ 是单调不增的。）在同一张图上，画出所有处理器以相同速度 $\bar{s}$ 运行时得到的能量—时间权衡曲线，其中 $\bar{s}$ 从 $s_{\min}$ 变化到 $s_{\max}$。

注意：在本题的 (b) 部分，不像 (a) 部分那样对 $E$ 有 $E_{\max}$ 的限制；你要找的是 $E$ 对 $T$ 的最优权衡。

---

### Solution（解答）

**(a)** 首先考察能量 $E$。一般而言，它并不是 $s$ 的凸函数（convex function）。例如考虑 $f(s)=s^{1.5}$，它是递增且凸的；但 $(1/s)f(s)=\sqrt{s}$，后者不是凸的。（译者注：$\sqrt{s}$ 在 $s\ge 0$ 上其实是凹函数，其 Hessian 为负，不满足凸函数的二阶条件。）所以我们无论如何都需要对问题做某种重新表述（reformulate）。

我们引入变量 $\tau\in\mathbb{R}^{n}$，定义为

```math
\tau_i=\frac{\alpha_i}{s_i}.
```

变量 $\tau_i$ 是完成任务 $i$ 所需的时间。我们可以由 $\tau_i$ 反解出 $s_i$ 为 $s_i=\alpha_i/\tau_i$。后面我们将用 $\tau_i$ 替代 $s_i$。

能量 $E$ 作为 $\tau$ 的函数为

```math
E=\sum_{i=1}^{n}\tau_i\,f\!\left(\frac{\alpha_i}{\tau_i}\right).
```

这是关于 $\tau$ 的凸函数，因为每一项都是 $f$ 的透视函数（perspective）$yf(x/y)$ 在 $y=\tau_i$、$x=\alpha_i$ 处的取值。（这表明 $E$ 关于 $\tau$ 和 $\alpha$ 是联合凸的（jointly convex），不过这里我们把 $\alpha$ 视为常数。）

> **术语 · perspective（透视函数）**：对函数 $f$，其透视函数定义为 $g(x,t)=t\,f(x/t)$，其中 $t>0$。透视运算保持凸性：若 $f$ 是凸函数，则 $g$ 关于 $(x,t)$ 联合凸。这里利用的正是这一性质——把原来非凸的 $(\alpha_i/s_i)f(s_i)$ 变换成凸的 $\tau_i f(\alpha_i/\tau_i)$。

处理器速度限制 $s_{\min}\le s_i\le s_{\max}$ 等价于

```math
\frac{\alpha_i}{s_{\max}}\le\tau_i\le\frac{\alpha_i}{s_{\min}},\qquad i=1,\dots,n.
```

（译者注：这是因为 $\tau_i=\alpha_i/s_i$，而 $s_i$ 越大则 $\tau_i$ 越小，故上界 $s_{\max}$ 对应 $\tau_i$ 的下界，下界 $s_{\min}$ 对应 $\tau_i$ 的上界，不等式方向要颠倒。）

现在考察优先级约束。为处理它们，我们引入变量 $t\in\mathbb{R}^{n}$，其中 $t_i$ 是任务 $i$ 完成时间（completion time）的一个上界。于是有

```math
T\le\max_{i}t_i.
```

任务 $i$ 不能在其所有先行任务完成之前开始；在那之后，它还需要至少 $\tau_i$ 的额外时间。因此有

```math
t_j\ge t_i+\tau_j,\qquad (i,j)\in\mathcal{P}.
```

没有先行任务的任务必须满足 $t_i\ge\tau_i$。事实上，这对所有任务都成立，因此有

```math
t_i\ge\tau_i,\qquad i=1,\dots,n.
```

我们把问题表述为

```math
\begin{aligned}
&\text{minimize} && \max_i t_i \\
&\text{subject to} && \sum_{i=1}^{n}\tau_i f(\alpha_i/\tau_i)\le E_{\max} \\
&&& \alpha_i/s_{\max}\le\tau_i\le\alpha_i/s_{\min},\qquad i=1,\dots,n \\
&&& t_i\ge\tau_i,\qquad i=1,\dots,n \\
&&& t_j\ge t_i+\tau_j,\qquad (i,j)\in\mathcal{P},
\end{aligned}
```

其中变量为 $t$ 和 $\tau$。能量约束是凸的（因为它是透视函数的和，而凸函数的非负求和保持凸性），其余约束都是线性的（linear）；目标函数 $\max_i t_i$ 是凸的（它是逐点最大值，而凸函数的逐点最大仍是凸函数）。

> **为什么等价（equivalence）**：约束 $t_j\ge t_i+\tau_j$ 与 $t_i\ge\tau_i$ 一起，精确刻画了“任务 $j$ 在它所有先行任务完成后还要 $\tau_j$ 时间”这一事实——沿 DAG 的每条路径反复累加，$t_j$ 会成为任务 $j$ 真实完成时间的一个下界；最小化 $\max_i t_i$ 时，最优的 $t_i$ 会恰好等于真实完成时间，因此目标值就是真实的 $T$。同时 $\tau_i$ 与 $s_i$ 一一对应，所以变量替换不改变可行域。

> **为什么凸（convexity）**：目标 $\max_i t_i$ 凸；能量约束中每一项 $\tau_i f(\alpha_i/\tau_i)$ 是凸函数 $f$ 的透视，故凸，其和亦凸；其余约束是仿射（affine）的，故凸。由于对任意正的、递增、凸的 $f$ 都成立（未对 $f$ 做额外假设），整个问题是一个凸优化问题。

**(b)** 对于这个具体问题，有

```math
\tau_i f(\alpha_i/\tau_i)=\tau_i+\alpha_i+\frac{\alpha_i^{2}}{\tau_i}+\frac{\alpha_i^{3}}{\tau_i^{2}}.
```

> **推导**：将 $f(s)=1+s+s^2+s^3$ 代入透视形式：

```math
\tau_i f\!\left(\frac{\alpha_i}{\tau_i}\right)
=\tau_i\left[1+\frac{\alpha_i}{\tau_i}+\left(\frac{\alpha_i}{\tau_i}\right)^{2}+\left(\frac{\alpha_i}{\tau_i}\right)^{3}\right]
=\tau_i+\alpha_i+\frac{\alpha_i^{2}}{\tau_i}+\frac{\alpha_i^{3}}{\tau_i^{2}}.
```

> 由于 $\tau_i>0$，后两项 $1/\tau_i$ 与 $1/\tau_i^{2}$ 均为凸函数（在正半轴上），故整体为凸。

为生成最优权衡曲线，我们做标量化（scalarize）：对 $\lambda$ 在某个范围内取值（该范围能覆盖 $T$ 的整个取值范围），最小化 $T+\lambda E$。于是我们求解问题

```math
\begin{aligned}
&\text{minimize} && \max_i t_i+\lambda\sum_{i=1}^{n}\left(\tau_i+\alpha_i+\frac{\alpha_i^{2}}{\tau_i}+\frac{\alpha_i^{3}}{\tau_i^{2}}\right) \\
&\text{subject to} && \alpha_i/s_{\max}\le\tau_i\le\alpha_i/s_{\min},\qquad i=1,\dots,n \\
&&& t_i\ge\tau_i,\qquad i=1,\dots,n \\
&&& t_j\ge t_i+\tau_j,\qquad (i,j)\in\mathcal{P},
\end{aligned}
```

其中 $\lambda$ 在某个范围内取值。

> **术语 · scalarization（标量化）**：多目标（这里是“同时最小化 $T$ 和 $E$”两个目标）的一种处理方法——把多个目标加权合并成单一目标 $T+\lambda E$，通过改变权重 $\lambda$ 扫出 Pareto 前沿（Pareto frontier），即权衡曲线。$\lambda$ 越大越看重节能，$\lambda$ 越小越看重省时。

如果我们约束所有处理器必须以相同速度 $\bar{s}$ 运行，则实际上等于加上了约束 $\tau=(1/\bar{s})\alpha$。此时，我们可以通过求解下面的问题来求出完成所有进程所需的时间：

```math
\begin{aligned}
&\text{minimize} && \max_i t_i \\
&\text{subject to} && t_i\ge\alpha_i/\bar{s},\qquad i=1,\dots,n \\
&&& t_j\ge t_i+\alpha_j/\bar{s},\qquad (i,j)\in\mathcal{P}.
\end{aligned}
```

（这里我们其实并不真的需要解一个优化问题；但解它比写代码去直接计算 $T$ 更容易。）为生成“所有处理器以相同速度运行”情形下的权衡曲线，我们对 $\bar{s}$ 在 $s_{\min}=1$ 与 $s_{\max}=5$ 之间取值并求解上述问题。这给出 $T$ 的全部可能取值范围：当 $\bar{s}=s_{\max}$ 时得 $T=3.243$；当 $\bar{s}=s_{\min}$ 时得 $T=16.212$。

下面这段 MATLAB 代码用来画出这两条权衡曲线：

```matlab
cvx_quiet(true);
ps_data
% Optimal power-time tradeoff curve
Eopt = []; Topt = [];
fprintf(1,'Optimal tradeoff curve\n')
for lambda = logspace(0,-3,30);
fprintf(1,'Solving for lambda = %1.3f\n',lambda);
cvx_begin
variables t(n) tau(n)
E = sum(tau+alpha+alpha.^2.*inv_pos(tau)+...
alpha.^3.*square_pos(inv_pos(tau)));
minimize(lambda*E+max(t))
subject to
t(prec(:,2)) >= t(prec(:,1))+tau(prec(:,2))
t >= tau
tau >= alpha/s_max
tau <= alpha/s_min
cvx_end
E = sum(tau+alpha+alpha.^2./tau+alpha.^3./(tau.^2));
T = max(t);
Eopt = [Eopt E];
Topt = [Topt T];
end
% Tradeoff-curve for constant speed
fprintf(1,'\nConstant speed tradeoff curve\n')
Econst = []; Tconst = [];
for s_const = linspace(s_min,s_max,30);
fprintf(1,'Solving for s = %1.3f\n',s_const);
cvx_begin
variables t(n)
minimize(max(t))
subject to
t(prec(:,2)) >= t(prec(:,1))+alpha(prec(:,2))/s_const
t >= alpha/s_const
cvx_end
E = sum(alpha*(1/s_const+1+s_const+s_const^2));
T = max(t);
Econst = [Econst E];
Tconst = [Tconst T];
end
plot(Tconst,Econst,'r--')
hold on
plot(Topt,Eopt,'b-')
xlabel('Time')
ylabel('Energy')
grid on
axis([0 20 0 4000])
print -depsc processor_speed.eps
```

> **代码要点说明**：`inv_pos` 是 CVX 中用于凸表达式 `1/x`（$x>0$）的函数；`square_pos` 是 `x^2`（$x\ge 0$）的凸版本。由于 CVX 要求目标与约束都写成凸形式，$1/\tau_i$ 与 $1/\tau_i^2$ 必须用 `inv_pos` 而非普通的 `/`（后者对 CVX 不合法），$1/\tau_i^2$ 写作 `square_pos(inv_pos(tau))`。第一段循环通过 `lambda` 从 $10^0$ 到 $10^{-3}$ 对数等距取 30 个值，扫出最优权衡曲线；第二段循环通过 `s_const` 从 `s_min` 到 `s_max` 线性取 30 个值，得到恒定速度下的权衡曲线。

两条权衡曲线如下图所示。实线（solid line）对应最优权衡曲线，虚线（dotted line）对应恒定处理器速度下的权衡曲线。

```
（图：横轴 Time，纵轴 Energy；横轴 0–20，纵轴 0–4000；
  蓝色实线为最优权衡曲线，红色虚线为恒定速度权衡曲线。）
```

我们看到，在调整到相同 $T$ 的前提下，最优处理器速度所消耗的能量，显著低于所有处理器以相同速度运行时消耗的能量——尤其是当 $T$ 很小时。

我们注意到，这个特定问题其实可以不使用 (a) 部分给出的表述来求解。对于这个特定的功率函数，我们实际上可以直接用 $s$ 作为优化变量，而不必换坐标到 $\tau$。这是因为此时 $E$ 是 $s$ 的凸函数，其形式为

```math
E=\sum_{i=1}^{n}\alpha_i\left(\frac{1}{s_i}+1+s_i+s_i^{2}\right).
```

> **推导**：由 $E=\sum_i (\alpha_i/s_i)f(s_i)$，代入 $f(s_i)=1+s_i+s_i^2+s_i^3$：

```math
\frac{\alpha_i}{s_i}(1+s_i+s_i^2+s_i^3)
=\alpha_i\left(\frac{1}{s_i}+1+s_i+s_i^2\right).
```

> 各项中 $1/s_i$（$s_i>0$ 时凸）、$1$、$s_i$（仿射）、$s_i^2$（凸）均为凸，故 $E$ 关于 $s$ 凸。

为得到权衡曲线，我们可以求解问题

```math
\begin{aligned}
&\text{minimize} && \max_i t_i+\lambda\sum_{i=1}^{n}\alpha_i\left(\frac{1}{s_i}+1+s_i+s_i^{2}\right) \\
&\text{subject to} && s_{\min}\le s_i\le s_{\max},\qquad i=1,\dots,n \\
&&& t_i\ge\alpha_i/s_i,\qquad i=1,\dots,n \\
&&& t_j\ge t_i+\alpha_j/s_j,\qquad (i,j)\in\mathcal{P},
\end{aligned}
```

其中变量为 $t$ 和 $s$，$\lambda$ 在一组正值上取值。

```matlab
cvx_begin
variables s(n) t(n)
E = alpha'*(inv_pos(s)+1+s+square_pos(s));
minimize(lambda*E+max(t))
subject to
t(prec(:,2)) >= t(prec(:,1))+alpha(prec(:,2)).*...
inv_pos(s(prec(:,2)))
t >= alpha.*inv_pos(s)
```

> **为什么 $f(s)=1+s+s^2+s^3$ 情形能直接用 $s$**：关键在于此时 $E(s)=\sum_i \alpha_i(1/s_i+1+s_i+s_i^2)$ 本身已是凸的，无需透视变换。而 (a) 中一般情形下 $(\alpha_i/s_i)f(s_i)$ 对任意凸 $f$ 未必凸（反例 $f(s)=s^{1.5}$ 得到 $\sqrt{s}$），所以才必须换成 $\tau$ 变量、借助透视函数来保证凸性。这正是 (a) 部分强调“不能对 $f$ 做额外假设”的原因。

---

## Problem 1（续）：最小化最坏情形下违反航点约束（worst-case way-point violation）的轨迹

### （第 7 页，承接上一部分）

```math
s \ge s_{\max}
```

```math
s \le s_{\min}
```

cvx_end

> 译注：上述 `s >= s_max`、`s <= s_min` 是 CVX 中关于松弛变量 $s$ 的上、下界约束的代码片段，用于界定 $s$ 的取值范围。

最后，我们指出：这个具体问题也可以化为一个几何规划（geometric program, GP），因为 $E$（能耗/代价表达式）是速度的一个 posynomial 函数（正项式函数，即各系数为正、指数可为任意实数的单项式之和），而所有约束也都能写成 posynomial 不等式（posynomial inequalities）的形式。

---

## Problem 2：探索近最优解集（nearly optimal points）

一个优化算法只要问题可行（feasible），就能为问题找到一个最优解。很多时候，探究「近最优解」构成的集合是很有用的。当问题有一个「强最小值」（strong minimum）时，近最优解集很小——所有这些点都紧挨着最初找到的那个最优解。而在另一个极端，问题可能有一个「弱最小值」（soft/weak minimum），这意味着存在很多可行点，其中有些点离最初找到的最优解相当远，但其目标函数值却与最优值几乎相等。本题要求你用一种典型方法来探索近最优解集。

我们先求出给定问题的最优值 $p^\star$：

```math
\begin{aligned}
& \text{minimize} & & f_0(x) \\
& \text{subject to} & & f_i(x) \le 0, & i = 1, \ldots, m \\
& & & h_i(x) = 0, & i = 1, \ldots, p,
\end{aligned}
```

以及一个最优解 $x^\star \in \mathbb{R}^n$。然后我们取一个小的正数 $\epsilon$，以及一个向量 $c \in \mathbb{R}^n$，并求解下列问题：

```math
\begin{aligned}
& \text{minimize} & & c^T x \\
& \text{subject to} & & f_i(x) \le 0, & i = 1, \ldots, m \\
& & & h_i(x) = 0, & i = 1, \ldots, p \\
& & & f_0(x) \le p^\star + \epsilon.
\end{aligned}
```

> 译注：注意该问题的任何一个可行点，对原问题而言都是 $\epsilon$-次优（$\epsilon$-suboptimal）的——因为新增约束 $f_0(x) \le p^\star + \epsilon$ 恰好限制了目标函数值不能超过最优值加上 $\epsilon$。用不同的 $c$ 多次求解这个问题，就能生成（可能是不同的）$\epsilon$-次优点。如果问题有强最小值，这些点会彼此靠得很近；如果问题有弱最小值，它们则可能差异很大。

在这些实验中，选择 $c$ 有不同的策略。最简单的是随机地选取各个 $c$；另一种方法是令 $c$ 形如 $\pm e_i$（其中 $i = 1, \ldots, n$，$e_i$ 是第 $i$ 个单位坐标向量）。后一种方法给出的是 $x$ 的每一个分量在整个 $\epsilon$-次优集合上的「取值范围」（range）。

你将针对下面这个问题执行该方法，以判断它到底有强最小值还是弱最小值。你可以随机生成向量 $c$，样本数量以你能得出结论为准。你可以取 $\epsilon = 0.01\, p^\star$，这意味着我们考虑的是 $1\%$ 次优点构成的集合。

该问题是一个在 $\mathbb{R}^2$ 中运动的车辆的最小燃料最优控制（minimum fuel optimal control）问题。在时刻 $kh$ 处的位置记为 $p(k) \in \mathbb{R}^2$，速度记为 $v(k) \in \mathbb{R}^2$，其中 $k = 1, \ldots, K$。这里 $h > 0$ 是采样周期（sampling period）。它们由下列方程联系：

```math
p(k+1) = p(k) + h v(k),
```

```math
v(k+1) = (1 - \alpha) v(k) + \frac{h}{m} f(k), \qquad k = 1, \ldots, K-1,
```

其中 $f(k) \in \mathbb{R}^2$ 是在时刻 $kh$ 施加到车辆上的力，$m > 0$ 是车辆质量，而 $\alpha \in (0, 1)$ 用来刻画车辆所受的阻力（drag）：在没有任何其他力作用时，每个离散时间区间内车速会按因子 $1 - \alpha$ 衰减。

> 译注：这两个式子就是状态方程（state equations），它把「当前位置 + 当前速度」递推到「下一时刻的位置与速度」。第一个是标准的运动学积分（位置 = 旧位置 + 速度 × 时间步长）；第二个是含阻力的速度更新（旧速度按 $1-\alpha$ 缩放，再加上由力 $f(k)$ 产生的加速度 $(h/m) f(k)$ 的贡献）。

（这些公式是对涉及矩阵指数（matrix exponentials）的更精确公式的一种近似。）

力来自两个推进器（thrusters）以及重力（gravity）：

```math
f(k) =
\begin{bmatrix}
\cos\theta_1 \\ \sin\theta_1
\end{bmatrix}
u_1(k)
+
\begin{bmatrix}
\cos\theta_2 \\ \sin\theta_2
\end{bmatrix}
u_2(k)
+
\begin{bmatrix}
0 \\ -mg
\end{bmatrix},
\qquad k = 1, \ldots, K-1.
```

这里 $u_1(k) \in \mathbb{R}$ 和 $u_2(k) \in \mathbb{R}$ 是两个推进器的（非负的）推力大小，$\theta_1$ 和 $\theta_2$ 是推力方向，$g = 10$ 是重力加速度常数。

> 译注：这个式子把总力分解成三个来源：第一个推进器沿方向 $(\cos\theta_1, \sin\theta_1)$ 贡献 $u_1(k)$，第二个推进器沿方向 $(\cos\theta_2, \sin\theta_2)$ 贡献 $u_2(k)$，再加上竖直向下的重力 $(0, -mg)$。

总燃料消耗为

```math
F = \sum_{k=1}^{K-1} \bigl( u_1(k) + u_2(k) \bigr).
```

（回忆一下 $u_1(k) \ge 0$，$u_2(k) \ge 0$。）

问题是在满足初始条件 $p(1) = 0$、$v(1) = 0$ 以及航点约束（way-point constraints）

```math
p(k_i) = w_i, \qquad i = 1, \ldots, M,
```

的前提下最小化燃料消耗。（这些条件说的是：在时刻 $h k_i$，车辆必须经过位置 $w_i \in \mathbb{R}^2$。）

此外，我们还要求车辆始终停留在一个方形工作区域（square operating region）内：

```math
\|p(k)\|_{\infty} \le P^{\max}, \qquad k = 1, \ldots, K.
```

> 译注：$\|p(k)\|_{\infty}$ 是向量 $p(k)$ 的无穷范数（$\infty$-norm），即两个坐标分量绝对值的最大值 $\max\{|p_1(k)|, |p_2(k)|\}$。这个约束等价于要求每一时刻的 $p_1(k)$、$p_2(k)$ 都落在 $[-P^{\max}, P^{\max}]$ 内，从而形成一个以原点为中心的方形区域。

本题的两部分都针对 `thrusters_data.m` 中给出的那个具体问题实例。

**(a)** 找出一条最优轨迹，以及相应的最小燃料消耗 $p^\star$。在 $\mathbb{R}^2$ 中（即 $p_1, p_2$ 平面上）画出轨迹 $p(k)$。验证它确实经过了各个航点。

**(b)** 用上述通用方法生成若干条 $1\%$ 次优轨迹，并在 $\mathbb{R}^2$ 中画出相应的轨迹。你认为这个问题是有强最小值，还是弱最小值？

### Solution（解答）

**(a)** 下面的 Matlab 脚本找出最优解。

```
cvx_quiet(true);
thrusters_data;
F = [ cos(theta1) cos(theta2);...
sin(theta1) sin(theta2)];
```

```matlab
% finding optimal solution
cvx_begin
variables u(2,K-1) p(2,K) v(2,K)
minimize ( sum(sum(u)))
p(:,1) == 0;
% initial position
v(:,1) == 0;
% initial velocity
% way-point constraints
p(:,k1) == w1;
p(:,k2) == w2;
p(:,k3) == w3;
p(:,k4) == w4;
for i=1:K-1
p(:,i+1) == p(:,i) + h*v(:,i);
v(:,i+1) == (1-alpha)*v(:,i) + h*F*u(:,i)/m + [0; -g*h];
end
u >= 0;
% constaints on positions (x,y)
p <= pmax;
p >= -pmax;
cvx_end
display('The optimal fuel use is: ');
optval = cvx_optval
plot(p(1,:),p(2,:));
hold on
ps = [zeros(2,1) w1 w2 w3 w4];
plot(ps(1,:),ps(2,:),'*');
xlabel('x'); ylabel('y'); title('optimal');
axis([-6 6 -6 6]);
```

> 译注：脚本结构对应前述建模——`p(:,i+1) == p(:,i) + h*v(:,i)` 是位置更新，`v(:,i+1) == (1-alpha)*v(:,i) + h*F*u(:,i)/m + [0; -g*h]` 是速度更新（注意重力项 $[0;-gh]$ 等价于把 $f(k)$ 中的 $(0,-mg)$ 乘上 $h/m$）。`F` 是由两个推力方向向量拼成的 $2\times 2$ 矩阵，所以 `F*u(:,i)` 就是两个推进器推力合成的水平力与竖直力。`u >= 0` 保证推力非负，`p <= pmax; p >= -pmax` 保证位置落在方形区域内。目标 `sum(sum(u))` 即 $\sum_{k=1}^{K-1}(u_1(k)+u_2(k))$，正是燃料消耗 $F$。

这个 Matlab 脚本生成如下最优轨迹。

> 〔原书此处为一幅轨迹图：横轴 $x$、纵轴 $y$，坐标范围均为 $-6$ 到 $6$，图中展示最优轨迹 $p(k)$ 与航点标记。〕

最优燃料消耗值求得为 **1055.3**。

**(b)** 下面的脚本找出 $1\%$ 次优解。

```matlab
% finding nearly optimal solutions
cvx_begin
variables u(2,K-1) p(2,K) v(2,K)
minimize ( sum ( sum ( randn(2,K-1).*u ) ) + ...
sum ( sum ( randn(2,K).*p ) ) + ...
sum ( sum ( randn(2,K).*v ) ) )
p(:,1) == 0;
% initial position
v(:,1) == 0;
% initial velocity
% way-point constraints
p(:,k1) == w1;
p(:,k2) == w2;
p(:,k3) == w3;
p(:,k4) == w4;
for i=1:K-1
p(:,i+1) == p(:,i) + h*v(:,i);
v(:,i+1) == (1-alpha)*v(:,i) + F*u(:,i) + [0; -g*h];
end
u >= 0;
sum(sum(u))<=1.01*optval;
% constaints on positions (x,y)
p <= pmax;
p >= -pmax;
cvx_end
figure;
plot(p(1,:),p(2,:));
hold on
ps = [zeros(2,1) w1 w2 w3 w4];
plot(ps(1,:),ps(2,:),'*');
xlabel('x'); ylabel('y'); title('suboptimal');
axis([-6 6 -6 6]);
```

> 译注：与 (a) 的关键差异在于两处——其一，目标函数由 `sum(sum(u))` 换成了一个随机线性目标：把随机矩阵 `randn(2,K-1)`、`randn(2,K)`、`randn(2,K)` 分别逐元素乘上 `u`、`p`、`v` 再求和，这相当于取了某个随机向量 $c$ 的线性目标 $c^T x$（其中 $x$ 是把 $u,p,v$ 堆叠起来得到的决策向量）；其二，新增约束 `sum(sum(u)) <= 1.01*optval` 正是 $f_0(x) \le p^\star + \epsilon$，即把燃料限制在最优值的 $1.01$ 倍以内（$\epsilon = 0.01\,p^\star$）。每次运行脚本都会重新抽样随机矩阵，从而得到不同的 $\epsilon$-次优点。

MATLAB 脚本返回 4 条随机生成的近最优轨迹。

> 〔原书此处为四幅轨迹子图，均以 $x$ 为横轴、$y$ 为纵轴、坐标范围 $-6$ 到 $6$，展示 4 条不同的近最优轨迹及其航点标记。〕

我们看到，这些近最优轨迹彼此差异非常大。所以在这个问题中存在着一个**弱最小值（weak minimum）**，即一个非常大的 $1\%$-次优集合。

> 译注：这正是题首所述「soft/weak minimum」的判定依据——若最小燃料问题有强最小值，各条近最优轨迹应当相互重合；但实际得到的 4 条轨迹差别巨大，说明存在大量相互远离、却几乎同样省油的可行轨迹，因此结论是弱最小值。

---

## Problem 3：在未知非线性测量函数下估计向量

我们想要估计一个向量 $x \in \mathbb{R}^n$，已知若干测量值

```math
y_i = \phi\bigl( a_i^T x + v_i \bigr), \qquad i = 1, \ldots, m.
```

这里 $a_i \in \mathbb{R}^n$ 已知，$v_i$ 是独立同分布（IID）的 $N(0, \sigma^2)$ 随机噪声，而 $\phi : \mathbb{R} \to \mathbb{R}$ 是一个未知的单调递增（monotonic increasing）函数，已知对所有 $u$ 都满足

```math
\alpha \le \phi'(u) \le \beta.
```

（这里 $\alpha$ 和 $\beta$ 是已知的正常数，且 $\alpha < \beta$。）我们希望在给定 $y_i$ 的前提下，求出 $x$ 和 $\phi$ 的最大似然估计（maximum likelihood estimate, MLE）。（此外我们还已知 $a_i$、$\sigma$、$\alpha$ 和 $\beta$。）

这听起来像是一个无穷维（infinite-dimensional）问题，因为我们要估计的参数中有一个是函数。事实上，我们只需要知道 $m$ 个数 $z_i = \phi^{-1}(y_i)$（$i = 1, \ldots, m$）就够了。因此，所谓「估计 $\phi$」其实是指估计这 $m$ 个数 $z_1, \ldots, z_m$。（这些数并非任意取值；它们必须与先验信息「对所有 $u$ 都有 $\alpha \le \phi'(u) \le \beta$」保持一致。）

**(a)** 解释如何用凸优化（convex optimization）求出 $x$ 和 $\phi$（即 $z_1, \ldots, z_m$）的最大似然估计。

**(b)** 在 `nonlin_meas_data.m` 给出的数据上执行你的方法，该文件包含一个矩阵 $A \in \mathbb{R}^{m \times n}$，其各行分别为 $a_1^T, \ldots, a_m^T$。给出 $\hat{x}_{\text{ml}}$，即 $x$ 的最大似然估计。画出你估计出的函数 $\hat\phi_{\text{ml}}$。（你可以把 $(\hat z_{\text{ml}})_i$ 对 $y_i$ 作图来实现，其中 $y_i$ 在纵轴上，$(\hat z_{\text{ml}})_i$ 在横轴上。）

**提示（Hint）。** 你可以假设测量值已按编号排序，使得 $y_i$ 非递减排列，即 $y_1 \le y_2 \le \cdots \le y_m$。（第 (b) 部分问题实例给出的数据正是按这个顺序排列的。）

### Solution（解答）

**(a)** 测量方程可以写成

```math
z_i = \phi^{-1}(y_i), \qquad i = 1, \ldots, m.
```

函数 $\phi^{-1}$ 未知（事实上它正是待估计的对象），但它的导数介于 $1/\beta$ 与 $1/\alpha$ 之间。用 $z_i$ 来表示，这意味着（假设数据中的 $y_i$ 按非递减顺序给出）

```math
\frac{1}{\beta}\,(y_{i+1} - y_i) \le z_{i+1} - z_i \le \frac{1}{\alpha}\,(y_{i+1} - y_i), \qquad i = 1, \ldots, m-1.
```

> 译注：这一推导的关键依据是反函数求导与单调性的关系。由于 $\phi$ 单调递增，其反函数 $\phi^{-1}$ 也单调递增，且 $(\phi^{-1})'(y) = 1 / \phi'(u)$（其中 $u = \phi^{-1}(y)$）。由已知 $\alpha \le \phi'(u) \le \beta$，两边取倒数（注意正数取倒数会反转不等式方向）得到 $\dfrac{1}{\beta} \le (\phi^{-1})'(y) \le \dfrac{1}{\alpha}$。也就是说，$\phi^{-1}$ 的斜率被夹在 $\dfrac{1}{\beta}$ 和 $\dfrac{1}{\alpha}$ 之间。因此，对任意相邻的 $y_i \le y_{i+1}$，反函数的差值 $z_{i+1} - z_i$ 必然满足「最小斜率 × 自变量差 ≤ 函数值差 ≤ 最大斜率 × 自变量差」，即上面这对不等式。这一约束把「$\phi$ 未知但其导数有界」这一先验，转化成了关于有限个 $z_i$ 的线性约束。

对数似然函数（log-likelihood function）具有如下形式：

```math
l(z, x) = -\frac{1}{\sigma^2} \sum_{i=1}^{m} \bigl( z_i - a_i^T x \bigr)^2
```

> 译注：这一形式的由来如下。因为 $\phi$ 单调递增，故 $\phi^{-1}$ 也单调递增，于是测量关系 $y_i = \phi(a_i^T x + v_i)$ 等价于 $z_i = \phi^{-1}(y_i) = a_i^T x + v_i$。噪声 $v_i \sim N(0, \sigma^2)$ 独立同分布，所以在给定 $x$ 和 $z_i$ 下观测 $y_i$ 的联合密度正比于 $\prod_i \exp\!\bigl(-\frac{(z_i - a_i^T x)^2}{2\sigma^2}\bigr)$，取对数即得上述 $l(z,x)$（略去了与 $x, z$ 无关的常数项与 $\frac{1}{2}$ 因子）。最大化 $l(z,x)$ 等价于最小化 $\sum_{i=1}^m (z_i - a_i^T x)^2$。

---

## （续）Problem 3 —— 非单调非线性测量

（接上一段。前面已建立最大似然估计 Maximum Likelihood Estimate 的目标，这里补全推导。）

（加上一个无关的常数）。因此，为了求 $x$ 和 $z$ 的最大似然估计 Maximum Likelihood (ML) estimate，我们求解如下问题：

```math
\text{maximize}\quad -\frac{1}{\sigma^2}\sum_{i=1}^{m}\bigl(z_i - a_i^T x\bigr)^2
```

```math
\text{subject to}\quad \frac{1}{\beta}(y_{i+1}-y_i) \le z_{i+1}-z_i \le \frac{1}{\alpha}(y_{i+1}-y_i),\quad i=1,\dots,m-1,
```

其中变量为 $z\in\mathbb{R}^m$ 与 $x\in\mathbb{R}^n$。这是一个二次规划 Quadratic Program (QP)。

> 译注（为何这是 QP）：目标函数是 $z$ 的二次型（负号系数，对最大化而言是凹 concave 函数，等价于最小化一个凸二次型），约束是 $z$ 的分量差分上的线性（仿射 affine）不等式。目标为二次、约束为线性，故整体是 QP。

**(b)** 下面的 Matlab 代码求解给定问题：

```matlab
nonlin_meas_data
row = zeros(1,m);
row(1) = -1;
row(2) = 1;
col = zeros(1,m-1);
col(1) = -1;
B = toeplitz(col,row);
cvx_begin
  variable x(n);
  variable z(m);
  minimize(norm(z - A*x));
  subject to
    1/beta*B*y <= B*z;
    B*z <= 1/alpha*B*y;
cvx_end
disp('estimated x:'); disp(x);
plot(z,y)
ylabel('y')
xlabel('z')
title('ML estimate of \phi')
```

估计得到的 $x$ 为 $x = (0.4819,\,-0.4657,\,0.9364,\,0.9297)$。图 1 画出了估计的 $z$ 相对于测量值 $y$ 的关系。

> 译注（代码怎么对应数学）：矩阵 $B$ 是差分算子差分矩阵 difference matrix，第 $i$ 行是 $(-1, 1)$（在位置 $i$ 和 $i+1$），所以 $B z$ 的第 $i$ 个分量就是 $z_{i+1}-z_i$；同理 $B y$ 的分量是 $y_{i+1}-y_i$。于是约束 `1/beta*B*y <= B*z` 与 `B*z <= 1/alpha*B*y` 逐分量等价于 $\frac{1}{\beta}(y_{i+1}-y_i)\le z_{i+1}-z_i\le \frac{1}{\alpha}(y_{i+1}-y_i)$。目标 `norm(z - A*x)` 是 $\|z-Ax\|_2$，其平方正比于上面最大化目标中的负二次型（忽略 $\sigma^2$ 常数），故二者等价。

---

## Figure 1（第 16 页，仅图，无文字）

> 图 1：最大似然估计得到的 $\hat\varphi(z)$（纵轴）关于 $z$（横轴）的函数图。图中数据点沿一条近似单调递增的曲线分布，$\hat\varphi$ 是 $\varphi$ 的 ML 估计。数据取自 `nonlin_meas_data`，横轴范围约 $[-2.5, 2.5]$，纵轴约 $[-6, 4]$。

〔原书此处为插图，无正文文字。〕

---

## Problem 4 —— 优化速率与时隙占比

**4. Optimizing rates and time slot fractions.（优化速率与时隙占比。）**

我们考虑一个无线系统，它使用时分多址 Time-Division Multiple Access (TDMA) 来支持 $n$ 条通信流 communication flows。这些流有（非负的）速率 rates $r_1,\dots,r_n$，单位为比特/秒 (bits/sec)。要在流 $i$ 上支持速率 $r_i$，需要发射功率 transmitter power

```math
p = a_i\bigl(e^{br_i} - 1\bigr),
```

其中 $b$ 是一个（已知的）正常数，$a_i$ 是（已知的）正常数，与接收机 $i$ 的噪声功率和增益有关。

TDMA 的工作方式如下。时间被划分成若干周期，每个周期有某个固定时长 $T$（秒）。每个 $T$ 长的周期被分成 $n$ 个时隙 time-slots，时长分别为 $t_1,\dots,t_n$，必须满足 $t_1+\cdots+t_n=T$，$t_i\ge 0$。在时隙 $i$ 内，通信流 $i$ 以瞬时速率 instantaneous rate $r = T r_i / t_i$ 传输，从而在每个 $T$ 长周期内，流 $i$ 传输了 $T r_i$ 比特。时隙 $i$ 期间所需的功率为 $a_i\bigl(e^{b T r_i/t_i}-1\bigr)$，因此每个 $T$ 长周期内的平均发射功率为

```math
P = \frac{1}{T}\sum_{i=1}^{n} a_i t_i \bigl(e^{b T r_i/t_i} - 1\bigr).
```

当 $t_i=0$ 时，若 $r_i>0$ 则取 $P=\infty$，若 $r_i=0$ 则取 $P=0$。（后者对应于流量为零、且分配给该流的时隙也为零的情形。）

问题是要找到速率 $r\in\mathbb{R}^n$ 和时隙时长 $t\in\mathbb{R}^n$，使对数效用函数 log utility function

```math
U(r) = \sum_{i=1}^{n} \log r_i
```

最大化，约束为 $P\le P^{\max}$。（这个效用函数常被用来保证「公平性」fairness：每条通信流至少得到某个正的速率。）问题数据是 $a_i, b, T$ 和 $P^{\max}$；变量是 $t_i$ 和 $r_i$。

**(a)** 把这个问题表述成一个凸优化问题 convex optimization problem。如有需要，可以自由引入新变量或作变量替换。请务必在表述中论证目标函数或约束函数的凸性 convexity。

**(b)** 给出你的表述的最优性条件 optimality conditions。当然，我们更希望得到简单的最优性条件而不是复杂的。注意：我们不要求你解这些最优性条件；你可以把它们给成一组方程（可能还带不等式）。

> 提示 Hint：对于对数效用函数，我们不能有 $r_i=0$，因此也不能有 $t_i=0$；因此约束 $r_i\ge 0$ 与 $t_i\ge 0$ 不可能在边界上激活 active 或起紧作用 tight。这将使你能简化最优性条件。

**Solution.（解答。）**

问题为

```math
\text{maximize}\quad \sum_{i=1}^{n}\log r_i
```

```math
\text{subject to}\quad \mathbf{1}^T t = T,
```

```math
P = \frac{1}{T}\sum_{i=1}^{n} a_i t_i\bigl(e^{b T r_i/t_i}-1\bigr) \le P^{\max},
```

其中变量为 $r\in\mathbb{R}^n$ 与 $t\in\mathbb{R}^n$。这里有一个隐式约束 implicit constraint $r_i>0$，以及 $t_i>0$。

事实上，我们不需要引入任何新变量，也不需要做任何变量替换。这个问题本身就是一个凸优化问题。目标函数显然是凹 concave 的，因此可以被最大化。唯一的问题是函数 $P$ 是否关于 $r$ 和 $t$ 是凸的。要证明这一点，我们需要证明函数 $f(x,y)=x\,e^{x/y}$ 在 $y>0$ 时关于 $x$ 和 $y$ 是凸的。但这不过是指数函数的透视函数 perspective（透视变换 perspective transformation 保持凸性：若 $g$ 为凸函数，则其透视 $y g(x/y)$ 在 $y>0$ 时也凸）。所以它是凸的。函数 $P$ 只是这类函数的正加权和（再加上一个仿射项），所以它也是凸的。

> 译注（为何 $x e^{x/y}$ 是透视）：对凸函数 $g(u)=e^u$，其透视 perspective 定义为 $y\,g(x/y)=y\,e^{x/y}$。但 $f(x,y)=x\,e^{x/y}$ 不是 $y\,e^{x/y}$，而是 $x$ 乘 $e^{x/y}$。严格的写法是：令 $u=x/y$，则 $x e^{x/y}=y\,(x/y)\,e^{x/y}=y\,h(x/y)$，其中 $h(u)=u e^u$。因为 $h(u)=u e^u$ 是凸函数（$h''(u)=(u+2)e^u>0$），其透视 $y\,h(x/y)=x\,e^{x/y}$ 在 $y>0$ 时是凸的。因此 $P$ 是若干凸函数之和（系数 $a_i/T>0$）加常数，故凸。这样约束 $P\le P^{\max}$ 是凸集 convex set（凸函数的子水平集 sublevel set 是凸集），目标 $\sum_i\log r_i$ 是凹的，于是这是凸优化问题。

我们为等式约束引入一个 Lagrange 乘子 Lagrange multiplier $\nu\in\mathbb{R}$，为不等式约束引入 $\lambda\in\mathbb{R}_+$。隐式约束 $t\succeq 0$、$r\succeq 0$ 不需要 Lagrange 乘子；即便引入，它们在最优解处也应为零，因为这些约束不可能起紧作用。

KKT 条件 Karush–Kuhn–Tucker conditions 如下：原始可行性 primal feasibility，

```math
\mathbf{1}^T t = T,
```

```math
\frac{1}{T}\sum_{i=1}^{n} a_i t_i\bigl(e^{b T r_i/t_i}-1\bigr) \le P^{\max},
```

对偶可行性 dual feasibility，$\lambda\ge 0$，

```math
\frac{\partial L}{\partial r_i} = -\frac{1}{r_i} + \lambda a_i b\, e^{b T r_i/t_i} = 0,\quad i=1,\dots,n,
```

```math
\frac{\partial L}{\partial t_i} = \lambda\frac{a_i}{T}\Bigl(e^{b T r_i/t_i}-1-\bigl(b T r_i/t_i\bigr)e^{b T r_i/t_i}\Bigr) + \nu = 0,\quad i=1,\dots,n,
```

以及互补松弛条件 complementarity condition $\lambda\bigl(P-P^{\max}\bigr)=0$。

> 译注（KKT 条件是怎么写出来的）：设 Lagrange 函数 $L(r,t,\nu,\lambda)=-\sum_i\log r_i+\nu(\mathbf{1}^T t-T)+\lambda(P-P^{\max})$。对 $r_i$ 求偏导，$\partial(-\log r_i)/\partial r_i=-1/r_i$，而 $P$ 对 $r_i$ 的偏导为 $\frac{a_i}{T}\cdot t_i\cdot e^{b T r_i/t_i}\cdot\frac{b T}{t_i}=a_i b\, e^{b T r_i/t_i}$，乘上 $\lambda$ 得 $\lambda a_i b\, e^{b T r_i/t_i}$。对 $t_i$ 求偏导：$\mathbf{1}^T t$ 对 $t_i$ 偏导为 $1$（乘 $\nu$ 得 $\nu$）；$P$ 对 $t_i$ 的偏导需要用乘积法则，对 $a_i t_i(e^{b T r_i/t_i}-1)$ 求导得 $a_i(e^{bTr_i/t_i}-1)+a_i t_i\cdot e^{bTr_i/t_i}\cdot(-b T r_i/t_i^2)=a_i(e^{bTr_i/t_i}-1-(bTr_i/t_i)e^{bTr_i/t_i})$，整体乘 $\lambda/T$ 得 $\frac{\lambda a_i}{T}(e^{bTr_i/t_i}-1-(bTr_i/t_i)e^{bTr_i/t_i})$。所以驻点条件正是上面两式。

事实上，约束 $P\le P^{\max}$ 在最优解处必然起紧作用，因为效用函数关于 $r$ 是单调递增的：如果功率约束是松弛的 slack，我们就可以在不违反功率上限的前提下略微增大速率，从而得到更高的效用。换句话说，我们可以把 $P\le P^{\max}$ 替换成 $P=P^{\max}$。这意味着我们可以把第二个原始可行性条件换成等式，同时也可以断言互补松弛条件恒成立。

于是，KKT 条件为

```math
\mathbf{1}^T t = T,
```

```math
\frac{1}{T}\sum_{i=1}^{n} a_i t_i\bigl(e^{b T r_i/t_i}-1\bigr) = P^{\max},
```

```math
-\frac{1}{r_i} + \lambda a_i b\, e^{b T r_i/t_i} = 0,\quad i=1,\dots,n,
```

```math
\frac{\lambda a_i}{T}\Bigl(e^{b T r_i/t_i}-1-\bigl(b T r_i/t_i\bigr)e^{b T r_i/t_i}\Bigr) + \nu = 0,\quad i=1,\dots,n,
```

```math
\lambda \ge 0.
```

我们不要求你解这些方程。据我们所知，它没有解析解 analytic solution。但是，经过一场浩大而「血腥」的代数搏斗之后，可以用单参数搜索 one-parameter search 来解这些 KKT 条件，就像注水 water-filling 那样。尽管这看起来是个很好的解法，实际上它的计算复杂度并不比标准方法（例如解 KKT 条件的 Newton 方法 Newton's method）更好，前提是恰当地利用了 Newton 步方程中的特殊结构。无论哪种方式，你最终得到的都是一个大约需要几十次迭代、每次迭代需要 $O(n)$ 次浮点运算 flops 的方法。

记住，我们没有要求你解 KKT 方程。你应该感到庆幸，因为我们完全有可能会要求你解。

---

## Problem 5 —— 特征选择与稀疏线性分离

**5. Feature selection and sparse linear separation.（特征选择与稀疏线性分离。）**

假设 $x(1),\dots,x(N)$ 与 $y(1),\dots,y(M)$ 是 $\mathbb{R}^n$ 中两个给定的非空向量集合（类别 classes），它们可以被一个超平面 hyperplane（严格）分离 strictly separated，即存在 $a\in\mathbb{R}^n$ 与 $b\in\mathbb{R}$，使得

```math
a^T x(i) - b \ge 1,\quad i=1,\dots,N,
```

```math
a^T y(i) - b \le -1,\quad i=1,\dots,M.
```

这意味着这两个类别被「板层」slab

```math
S = \{\,z \mid |a^T z - b| \le 1\,\}
```

（弱）分离 weakly separated，其厚度 thickness 为 $2/\|a\|_2$。

你可以把 $x(i)$ 和 $y(i)$ 的分量看作特征 features；$a$ 和 $b$ 定义了一个仿射函数 affine function，它把这些特征组合起来，使我们能区分这两个类别。

为了找到分离这两个类别的最厚板层，我们可以求解 QP

```math
\text{minimize}\quad \|a\|_2
```

```math
\text{subject to}\quad a^T x_i - b \ge 1,\quad i=1,\dots,N,
```

```math
a^T y_i - b \le -1,\quad i=1,\dots,M,
```

其中变量为 $a\in\mathbb{R}^n$ 与 $b\in\mathbb{R}$。（这等价于书中 §8.6.1 第 424 页 (8.23) 给出的问题；另见习题 8.23。）

> 译注（为何厚度是 $2/\|a\|_2$）：板层由 $|a^T z-b|\le 1$ 定义，两条边界超平面是 $a^T z-b=\pm 1$。两条平行超平面 $a^T z-b=1$ 与 $a^T z-b=-1$ 之间的距离为 $\frac{|1-(-1)|}{\|a\|_2}=\frac{2}{\|a\|_2}$（平行超平面距离公式 $|c_1-c_2|/\|a\|$）。所以最小化 $\|a\|_2$ 等价于最大化厚度 $2/\|a\|_2$。

在这个问题中，我们寻找能用一个厚板层分离两个类别的 $(a,b)$，并且 $a$ 还要是稀疏的 sparse，也就是说存在许多 $j$ 使得 $a_j=0$。注意，若 $a_j=0$，则仿射函数 $a^T z-b$ 不依赖于 $z_j$，即第 $j$ 个特征不参与分类。所以稀疏的 $a$ 对应一个「简约」parsimonious 的分类函数，它只依赖少数几个特征。因此我们的目标是找到一个仿射分类函数，它既能给出厚的分离板层，又尽可能少地用特征来进行分类。

这通常是一个困难的组合（双准则）优化问题 combinatorial (bi-criterion) optimization problem，所以我们采用标准启发式 heuristic，求解

```math
\text{minimize}\quad \|a\|_2 + \lambda \|a\|_1
```

```math
\text{subject to}\quad a^T x_i - b \ge 1,\quad i=1,\dots,N,
```

```math
a^T y_i - b \le -1,\quad i=1,\dots,M,
```

其中 $\lambda\ge 0$ 是一个权重参数，用来控制分离板层厚度与（通过 $\ell_1$ 范数 $\ell_1$ norm 间接实现的）$a$ 的稀疏性之间的权衡 trade-off。

> 译注（为何用 $\ell_1$ 范数诱导稀疏）：$\ell_0$ 伪范数 $\|a\|_0=\text{card}(a)$（非零分量个数）直接度量稀疏性，但它非凸、导致组合困难。$\ell_1$ 范数 $\|a\|_1=\sum_j|a_j|$ 是 $\|a\|_0$ 的凸松驰 convex relaxation，在极小化时倾向于把许多分量压到零（因为绝对值函数在零处不可微、且是最紧的凸近似），从而间接诱导稀疏。$\lambda$ 越大，稀疏性越被强调、厚度越被牺牲。

获取数据文件 `sp_ln_sp_data.m`，它把 $x_i$ 和 $y_i$ 分别作为矩阵 $X$ 和 $Y$ 的列给出。求最大厚度分离板层的厚度。对大约 100 个 $\lambda$ 值（在一个合适的范围内取值，我们建议用对数间隔 log spacing）求解上述问题。对每个值，记录分离板层厚度 $2/\|a\|_2$ 以及 $\text{card}(a)$，即 $a$ 的基数 cardinality（也就是非零项的个数）。在计算基数时，若 $a_j$ 满足 $|a_j|\le 10^{-4}$，就把它计为零。把这些数据画成图，纵轴为板层厚度，横轴为基数。

> 译注（$\text{card}(a)$ 的容差意义）：由于数值优化不会得到严格为零的分量，而是得到很小的数，所以用阈值 $10^{-4}$ 把「足够接近零」的分量视为零，从而得到离散的稀疏度计数，画出一条「厚度 vs 特征数」的权衡曲线（Pareto 前沿 Pareto frontier 的近似）。

---

## Problem 5（续）：feature selection 解答 —— MATLAB 脚本与结果

### 完整题目回顾（承接前文）

> 使用该数据在 50 个特征中选出 10 个特征，给出所选特征的索引。此处对特征集合可以有多种选法，任选其一即可。然后找出**只使用所选特征**的最大厚度 separating slab（分离厚板）。这是标准做法：一旦选定将要使用的特征，就只用这些特征、且**不带 ℓ1 正则化**重新优化一次。

### 解答（Solution）

> 注：此题的完整题目与建模在原文第 21 页之前给出。这里从第 21 页的解答脚本开始译注。

**所用 MATLAB 脚本如下：**

```matlab
cvx_quiet(true);
sp_ln_sp_data;
% thickest slab（最厚的厚板）
cvx_begin
variables a(n) b
minimize ( norm(a) )
a'*X - b >= 1
a'*Y - b <= -1
cvx_end
w_thickest = 2./norm(a);
disp('The thickness of the maximum thickness separating slab is: ');
disp(w_thickest);
```

**逐段解释**：

- `cvx_quiet(true)`：让 CVX 求解器安静运行，不输出冗长的迭代日志。
- `sp_ln_sp_data`：载入数据，其中 `X`、`Y` 是两类样本点的数据矩阵（`n` 为特征维数）。
- 第一个 `cvx_begin … cvx_end` 块求解的是**不带正则化**的 thickest slab 问题：
  目标为 minimize $\|a\|$，约束为
  ```math
  a^\top x - b \ge 1 \quad (\forall x \in X), \qquad a^\top y - b \le -1 \quad (\forall y \in Y),
  ```

  即要求一个分离两类的超平面 $\{z \mid a^\top z = b\}$，两类点分别落在其两侧、且至少离它 1 个单位的距离（这里的 1 是任意给定的归一化尺度）。
- 由于 slab 的厚度等于 $\dfrac{2}{\|a\|}$（见下面对 slab 的定义），所以脚本用 `w_thickest = 2./norm(a)` 把最优 `a` 的范数换算成 slab 宽度。

**背景说明**：这里的 separating slab 是指形如

```math
\{ z \mid |a^\top z - b| \le 1 \}
```

的「板」区域——即到超平面 $a^\top z = b$ 的（带符号）距离绝对值不超过 1 的点集。它的**厚度**（垂直于超平面方向上的宽度）为 $\dfrac{2}{\|a\|}$。于是「最大化厚度」等价于「最小化 $\|a\|$」，这正是一个 convex optimization 问题（目标 $\|a\|$ 是凸的，约束是仿射的）。

**生成 trade-off curve（权衡曲线）的脚本：**

```matlab
% generating the trade-off curve
lambdas = logspace(-2,5);
A = zeros(n,length(lambdas));
for i=1:length(lambdas)
  cvx_begin
  variables a(n) b
  minimize ( norm(a) + lambdas(i)*norm(a,1) )
  a'*X - b >= 1
  a'*Y - b <= -1
  cvx_end
  A(:,i) = a;
end
w = 2./norms(A);
% width of the slab
card = sum((abs(A) > 1e-4));
plot(card,w)
hold on;
plot(card,w,'*')
xlabel('card(a)');
ylabel('w');
title('width of the slab versus cardinality of a');
```

**逐段解释**：

- `lambdas = logspace(-2,5)`：在对数尺度上从 $10^{-2}$ 到 $10^{5}$ 等距取一串正则化权重 $\lambda$。
- 对每个 $\lambda$，求解一个带 **ℓ1 正则化** 的问题：
  目标为 minimize $\|a\| + \lambda\,\|a\|_1$，约束与前面相同。
  其中 $\|a\|_1 = \sum_{j=1}^{n}|a_j|$ 是 ℓ1-norm（ℓ1 范数），$\lambda$ 越大，就越惩罚 $a$ 中各分量绝对值之和，从而**促使 $a$ 稀疏**（很多分量被压到 0）。
- `card = sum(abs(A) > 1e-4)`：对每个 $\lambda$ 下的最优 `a`，统计其分量中绝对值大于阈值 $10^{-4}$ 的个数，作为该解的有效特征数（cardinality，基数/非零分量数）。这里用阈值而非严格 `~=0`，是因为数值求解器只会把分量压到很小的数而非严格 0。
- 最后画出横轴 `card(a)`、纵轴 `w`（slab 宽度）的权衡曲线：稀疏度（特征数）与 slab 宽度之间的 trade-off——特征越少，能达到的最大厚度通常越小。

---

## Page 22：特征选择（固定 card(a) = 10）

```matlab
% feature selection (fixing card(a) to 10)
indices = find(card == 10);
idx = indices(end);
w_before = w(idx);
a_selected = A(:,idx);
features = find(abs(a_selected) > 1e-4);
num_feat = length(features);
X_sub = X(features,:);
Y_sub = Y(features,:);
cvx_begin
variables a(num_feat) b
minimize ( norm(a) )
a'*X_sub - b >= 1
a'*Y_sub - b <= -1
cvx_end
w_after = 2/norm(a);
disp('Using only the following 10 features');
disp(features');
disp('the width of the thickest slab returned by the regularized');
disp('optimization problem was: ');
disp(w_before);
disp('after reoptimizing, the width of the thickest slab is: ');
disp(w_after)
```

**逐段解释**：

- `indices = find(card == 10)`：在权衡曲线上找出恰好用了 10 个特征的解对应的位置。
- `idx = indices(end)`：取这些位置中的**最后一个**（对应 $\lambda$ 较大的那一端，即正则化较强、稀疏度恰好降到 10 的那个解）。
- `w_before = w(idx)`：记下这个带正则化解的 slab 宽度。
- `features = find(abs(a_selected) > 1e-4)`：从这个解的非零分量位置读出被选中的特征索引。
- `X_sub = X(features,:)`、`Y_sub = Y(features,:)`：只保留被选中特征对应的数据行，得到降维后的子数据。
- 随后在**只含这 10 个特征**的子空间里重新求解 thickest slab 问题（目标仍是 minimize $\|a\|$，但变量只有 `num_feat` 个），得到重优化后的宽度 `w_after`。
- 最后打印：带正则化时返回的宽度 `w_before`，以及重优化后的宽度 `w_after`。

**这一步的意义**：ℓ1 正则化会为了稀疏性而「牺牲」一部分厚度（因为 $\lambda\|a\|_1$ 这一项会轻微拉扯 $a$）。一旦特征已经选定，就应**去掉正则项**、只在这些特征上重新求解，从而得到用这 10 个特征所能达到的**真正最大** slab 厚度。这是特征选择后「重新优化」（reoptimize）的标准流程。

---

## Page 23：数值结果

最大厚度 separating slab 的厚度为 **116.4244**。脚本还生成了如下 trade-off curve：

（此处为原图：横轴 `card(a)`，纵轴 $2/\lVert a\rVert_2$，即 slab 宽度；数据点大致分布为——`card(a)` 取 10、15、20、25、30、35、40、45、50 时，宽度 $2/\lVert a\rVert_2$ 对应从约 75 递增到约 120，标题为 `width of the slab versus card(a)`。）

我们得到：只使用特征

```math
\{1,\ 7,\ 8,\ 18,\ 19,\ 21,\ 23,\ 26,\ 27,\ 46\}
```

时，由**正则化**优化问题得到的最大厚度 slab 的宽度是 **77.0246**。而在这个子集上**重新优化**之后，最大厚度 slab 的宽度增加到 **78.4697**。

**解释**：这印证了前面第 22 页的说明——去掉 ℓ1 正则项后，宽度从 77.0246 提升到 78.4697，说明正则项确实以略降厚度为代价换取了稀疏性。二者数值相近，说明选出的这 10 个特征已经能很好地区分两类数据。

---

## Problem 6. Bounding object position from multiple camera views（由多个相机视角界定物体位置）

**题目**：一个小物体位于未知位置 $x \in \mathbb{R}^3$，被 $m$ 台相机观测。我们的目标是在 $\mathbb{R}^3$ 中找一个 box（盒子/长方体）

```math
B = \{ z \in \mathbb{R}^3 \mid l \preceq z \preceq u \},
```

使得我们能保证 $x \in B$。我们希望这样的 bounding box（包围盒）尽可能小。（虽然用哪种标准判断「最小」并不影响本质，但我们可以用体积来判断盒子之间的「大小」。）

现在描述相机。位于 $x \in \mathbb{R}^3$ 的物体在第 $i$ 台相机的 image plane（像平面）上成像于点

```math
v_i = \frac{1}{c_i^\top x + d_i}\big(A_i x + b_i\big) \in \mathbb{R}^2.
```

矩阵 $A_i \in \mathbb{R}^{2\times 3}$、向量 $b_i \in \mathbb{R}^2$ 与 $c_i \in \mathbb{R}^3$、实数 $d_i \in \mathbb{R}$ 都是已知的，且取决于相机的位置和朝向。我们假设 $c_i^\top x + d_i > 0$。这个 $3 \times 4$ 矩阵

```math
P_i =
\begin{bmatrix}
A_i & b_i \\
c_i^\top & d_i
\end{bmatrix}
```

称为（第 $i$ 台相机的）camera matrix（相机矩阵）。通常（但不总是）$P_i$ 的前 3 列（即 $A_i$ 叠在 $c_i^\top$ 之上）构成一个正交矩阵，此时该相机称为 orthographic（正交的）。

我们并不能直接得到像点 $v_i$；只知道它所在的（正方形）pixel（像素）。换句话说，相机给我们一个测量值 $\hat v_i$（像点所在像素的中心），并且保证

```math
\| v_i - \hat v_i \|_{\infty} \le \rho_i / 2,
```

其中 $\rho_i$ 是第 $i$ 台相机的 pixel width（像素宽度，也等于高度）。除此之外我们对 $v_i$ 一无所知；它可能是该像素内的任意一点。

给定数据 $A_i, b_i, c_i, d_i, \hat v_i, \rho_i$，我们要找出保证包含 $x$ 的最小 box $B$（即求出向量 $l$ 和 $u$）。换句话说，求 $\mathbb{R}^3$ 中包含所有与这些相机观测**一致**的点的最小 box。

- **(a)** 解释如何用 convex 或 quasiconvex optimization（凸或拟凸优化）求解此题。你必须解释所用的任何变换、引入的任何新变量等。如果公式中任何函数的凸性或拟凸性不是显然的，务必给出论证。
- **(b)** 求解文件 `camera_data.m` 中给出的具体问题实例。务必让最终的数值答案（即 $l$ 和 $u$）突出显示。

### 术语定义（首次出现）

- **convex optimization（凸优化）**：目标函数与可行域都是凸的优化问题；其中 feasible set（可行集）由凸不等式/等式约束定义，其局部最优必为全局最优。
- **quasiconvex optimization（拟凸优化）**：目标函数为 quasiconvex function（拟凸函数，其所有下水平集 $\{x \mid f(x) \le t\}$ 都是凸集）的优化问题。
- **polyhedron（多面体）**：有限个线性等式与线性不等式的解集，形如 $\{x \mid Ax \preceq b,\ Cx = d\}$ 的集合。
- **LP（Linear Program，线性规划）**：在线性等式/不等式约束下最小化或最大化一个线性目标函数的凸优化问题。
- **componentwise inequality（分量不等式）$\preceq$**：向量之间的逐分量 $\le$，即 $l \preceq z$ 表示对每个坐标 $l_j \le z_j$。

---

## Page 25：解答（Solution）

### (a) 建模为 6 个 LP

我们得到一个位置集合 $P \subseteq \mathbb{R}^3$（很快会看到它是一个 polyhedron），它由所有与相机测量一致的位置 $x$ 组成。要在 $\mathbb{R}^3$ 中找出覆盖任意子集的最小 box，只需对（线性）函数 $x_1, x_2, x_3$ 分别求最小值和最大值，从而得到 $l$ 和 $u$。这里 $P$ 是 polyhedron，所以最终要解 **6 个 LP**，每个分别求出 $l_1, l_2, l_3, u_1, u_2, u_3$ 中的一个。

现在更仔细地看 $P$。我们的测量告诉我们：

```math
\hat v_i - \rho_i/2 \;\le\; \frac{1}{c_i^\top x + d_i}\big(A_i x + b_i\big) \;\le\; \hat v_i + \rho_i/2,
\qquad i = 1,\dots,m.
```

> 这里的 $\le$ 都是**逐分量**（componentwise）的：左、中、右都是 $\mathbb{R}^2$ 中的向量，不等式分别对两个坐标成立。$\hat v_i - \rho_i/2$ 中 $\rho_i/2$ 是一个标量，$\hat v_i$ 是二维向量，二者相减指每个分量都减 $\rho_i/2$。

由于 $c_i^\top x + d_i > 0$（题目已假设），我们可以用它乘遍不等式，**不改变不等号方向**，得到：

```math
(\hat v_i - \rho_i/2)\,(c_i^\top x + d_i) \;\le\; A_i x + b_i \;\le\; (\hat v_i + \rho_i/2)\,(c_i^\top x + d_i),
\qquad i = 1,\dots,m,
```

这是关于 $x$ 的 $2m$ 个**线性**不等式的集合（每台相机贡献 2 个，对应 $\mathbb{R}^2$ 的两个坐标）。特别地，它定义了一个 polyhedron。

**为什么是线性的**：$\hat v_i - \rho_i/2$ 是已知向量，$c_i^\top x + d_i$ 是 $x$ 的仿射（线性 + 常数）函数，$A_i x + b_i$ 也是 $x$ 的仿射函数。因此每一项都关于 $x$ 仿射，整个不等式是 linear inequality（线性不等式）。

**为什么「最小 box」只需对三个坐标分别求 min/max**：box 的约束 $l \preceq z \preceq u$ 是逐分量解耦的。要使 box 覆盖 $P$ 且每个坐标方向的区间 $[l_j, u_j]$ 尽可能小，就必须取

```math
l_j = \min_{x \in P} x_j, \qquad u_j = \max_{x \in P} x_j.
```

因为 $x_j$ 是线性函数、$P$ 是 polyhedron，求 $\min x_j$ 与 $\max x_j$ 各自是一个 LP。

为得到 $l_k$，解如下 LP：

```math
\begin{aligned}
&\text{minimize} && x_k \\
&\text{subject to} && (\hat v_i - \rho_i/2)\,(c_i^\top x + d_i) \le A_i x + b_i, && i = 1,\dots,m,\\
&&& A_i x + b_i \le (\hat v_i + \rho_i/2)\,(c_i^\top x + d_i), && i = 1,\dots,m,
\end{aligned}
```

对 $k = 1, 2, 3$ 分别求解；为得到 $u_k$，则对同一个目标**求最大值**（maximize $x_k$）。

> 也就是说：$l_1$ 由 minimize $x_1$ 得到，$u_1$ 由 maximize $x_1$ 得到，其余坐标同理。共 $3 \times 2 = 6$ 个 LP。

### (b) 求解给定实例的 MATLAB 脚本

```matlab
% load the data
camera_data;
A1 = P1(1:2,1:3); b1 = P1(1:2,4); c1 = P1(3,1:3); d1 = P1(3,4);
A2 = P2(1:2,1:3); b2 = P2(1:2,4); c2 = P2(3,1:3); d2 = P2(3,4);
A3 = P3(1:2,1:3); b3 = P3(1:2,4); c3 = P3(3,1:3); d3 = P3(3,4);
A4 = P4(1:2,1:3); b4 = P4(1:2,4); c4 = P4(3,1:3); d4 = P4(3,4);
cvx_quiet(true);
for bounds = 1:6
  cvx_begin
  variable x(3)
  switch bounds
    case 1
      minimize x(1)
    case 2
      maximize x(1)
    case 3
      minimize x(2)
    case 4
      maximize x(2)
    case 5
      minimize x(3)
    case 6
      maximize x(3)
  end
  % constraints for 1st camera
  (vhat(:,1)-rho(1)/2)*(c1*x + d1) <= A1*x + b1;
  A1*x + b1 <= (vhat(:,1)+rho(1)/2)*(c1*x + d1);
  % constraints for 2nd camera
  (vhat(:,2)-rho(2)/2)*(c2*x + d2) <= A2*x + b2;
  A2*x + b2 <= (vhat(:,2)+rho(2)/2)*(c2*x + d2);
  % constraints for 3rd camera
  (vhat(:,3)-rho(3)/2)*(c3*x + d3) <= A3*x + b3;
  A3*x + b3 <= (vhat(:,3)+rho(3)/2)*(c3*x + d3);
  % constraints for 4th camera
  (vhat(:,4)-rho(4)/2)*(c4*x + d4) <= A4*x + b4;
  A4*x + b4 <= (vhat(:,4)+rho(4)/2)*(c4*x + d4);
  cvx_end
  val(bounds) = cvx_optval;
end
disp(['l1 = ' num2str(val(1))]);
disp(['l2 = ' num2str(val(3))]);
disp(['l3 = ' num2str(val(5))]);
disp(['u1 = ' num2str(val(2))]);
disp(['u2 = ' num2str(val(4))]);
disp(['u3 = ' num2str(val(6))]);
```

**逐段解释**：

- `camera_data`：载入数据，其中 `P1`–`P4` 是 4 台相机的 $3\times 4$ camera matrix（`vhat`、`rho` 为像素中心与像素宽度）。
- 前四行把每台相机的矩阵 `Pk` 按行切分：`A_k = Pk(1:2, 1:3)`（前两行的前三列，即 $A_k \in \mathbb{R}^{2\times 3}$）、`b_k = Pk(1:2, 4)`（前两行的第四列，即 $b_k$）、`c_k = Pk(3, 1:3)`（第三行的前三列，即 $c_k^\top$ 的转置行）、`d_k = Pk(3, 4)`（右下角标量，即 $d_k$）。
- `for bounds = 1:6` 循环：依次对 $x_1, x_1, x_2, x_2, x_3, x_3$ 做 minimize / maximize，正好覆盖 6 个 LP。
- 每个 LP 的约束就是 (a) 中导出的 $2m$ 个线性不等式；这里 `m = 4`，所以是 8 个约束（每台相机 2 个）。
- `cvx_optval` 记录当前 LP 的最优值，存入 `val(bounds)`。
- 最后打印：`l1` 对应 minimize $x_1$（`val(1)`），`u1` 对应 maximize $x_1$（`val(2)`），`l2`/`u2` 对应 `val(3)`/`val(4)`，`l3`/`u3` 对应 `val(5)`/`val(6)`。

**MATLAB 脚本返回如下结果：**

```
l1 = -0.99561
l2 =  0.27531
l3 = -0.67899
u1 = -0.82450
u2 =  0.37837
u3 = -0.57352
```

**解读**：因此能保证包含物体位置 $x$ 的最小 bounding box 为

```math
B = \{ z \in \mathbb{R}^3 \mid l \preceq z \preceq u \},
```

其中

```math
l = (-0.99561,\ 0.27531,\ -0.67899), \qquad
u = (-0.82450,\ 0.37837,\ -0.57352).
```

每个坐标方向上的区间为：

```math
x_1 \in [-0.99561,\ -0.82450],\quad
x_2 \in [0.27531,\ 0.37837],\quad
x_3 \in [-0.67899,\ -0.57352].
```

> 注意：对每个坐标 $k$ 都有 $l_k \le u_k$（例如 $l_1 = -0.99561 \le -0.82450 = u_1$），符合 box 的定义，说明这 6 个 LP 的解是一致的。

---

## Problem 7. ℓ1.5 optimization（ℓ1.5 优化）

**题目**：同时使用 ℓ2-norm（或其平方）与 ℓ1-norm 的优化与逼近方法目前在统计、机器学习、信号与图像处理中非常流行。例子包括 Huber estimation（Huber 估计）、LASSO、basis pursuit（基追踪）、SVM、各种 ℓ1-regularized classification（ℓ1 正则化分类）方法、total variation denoising（全变差去噪）等。粗略地说，ℓ2-norm 对应于欧氏距离（的平方），或高斯分布的 negative log-likelihood function（负对数似然函数）；相比之下，ℓ1-norm 给出「稳健」逼近，即对 outlier（离群点）的敏感度更低，并且往往产生稀疏解（对范数自变量而言）。（以上都只是背景；解本题不需要了解这些。）

本题研究一种把两种范数混合的自然方法，即使用 ℓ1.5-norm，定义为

```math
\| z \|_{1.5} = \left( \sum_{i=1}^{k} |z_i|^{3/2} \right)^{2/3}
```

对 $z \in \mathbb{R}^k$ 成立。我们考虑最简单的逼近（回归）问题：

```math
\text{minimize} \quad \| Ax - b \|_{1.5},
```

变量为 $x \in \mathbb{R}^n$，问题数据为 $A \in \mathbb{R}^{m \times n}$、$b \in \mathbb{R}^m$。假设 $m > n$ 且 $A$ 满秩（即 $\mathrm{rank}\, A = n$）。希望这个 ℓ1.5-optimal approximation（ℓ1.5 最优逼近）问题能兼具 ℓ2 逼近与 ℓ1 逼近的一些优点。

- **(a)** 给出此问题的最优性条件（optimality conditions），并尽量写简单。你的解答应形如「$x$ 是 ℓ1.5-norm 逼近问题的最优解当且仅当 ……」。
- **(b)** 解释如何把 ℓ1.5-norm 逼近问题表述为一个 SDP（半定规划）。你的 SDP 可以包含线性等式与不等式约束。
- **(c)** 求解由如下代码生成的特定数值实例：

```matlab
randn('state',0);
A = randn(100,30);
b = randn(100,1);
```

数值验证最优性条件。给出 residual（残差）的直方图，并对 ℓ2-norm 与 ℓ1-norm 逼近各重复一次。可以用任何方法求解（但当然要说明是怎么做的）；特别地，你**不必**使用 (b) 中的 SDP 表述。

### 术语定义（首次出现）

- **ℓp-norm（ℓp 范数）**：$\|z\|_p = \left(\sum_{i}|z_i|^{p}\right)^{1/p}$，当 $p \ge 1$ 时是凸函数；$p=2$ 为欧氏范数，$p=1$ 为绝对值之和，$p=\infty$ 为最大绝对值分量。
- **sparse solution（稀疏解）**：很多分量精确等于 0 的解向量。
- **outlier（离群点）**：数据中明显偏离主体分布、可能由误差或异常引起的点。
- **optimality conditions（最优性条件）**：一个点成为（凸）问题最优解的充分必要条件，通常用梯度/次梯度、KKT 或一阶最优性来表达。
- **SDP（Semidefinite Program，半定规划）**：在 linear matrix inequality（线性矩阵不等式，即约束某个仿射对称矩阵半正定）与线性约束下优化线性目标函数的凸优化问题。
- **residual（残差）**：观测值与模型拟合值之差；这里指 $r_i = a_i^\top x - b_i$。

---

### 解答（Solution）

### (a) 最优性条件

我们完全可以把目标函数取 $3/2$ 次幂来最小化，即求解问题

```math
\text{minimize} \quad f(x) = \sum_{i=1}^{m} |a_i^\top x - b_i|^{3/2},
```

其中 $a_i^\top$ 表示矩阵 $A$ 的第 $i$ 行。

**这一步的推导思路**：目标原为 $\|Ax - b\|_{1.5} = \left(\sum_{i=1}^m |a_i^\top x - b_i|^{3/2}\right)^{2/3}$。外层函数 $t \mapsto t^{2/3}$ 在 $t \ge 0$ 上是**单调递增**的，因此「最小化 $\|Ax-b\|_{1.5}$」与「最小化 $\|Ax-b\|_{1.5}^{3/2}$」有**完全相同的最优解集**。而

```math
\|Ax - b\|_{1.5}^{3/2} = \left[\left(\sum_i |a_i^\top x - b_i|^{3/2}\right)^{2/3}\right]^{3/2}
= \sum_{i=1}^{m} |a_i^\top x - b_i|^{3/2}.
```

这样就**去掉了外层的 $2/3$ 次幂**，得到一个更简单、逐项可导（除残差为零处）的目标

```math
f(x) = \sum_{i=1}^{m} |a_i^\top x - b_i|^{3/2}.
```

**为什么这样做是安全的**：单调递增变换不改变极小值点的位置（只会改变目标值本身）。这正是后续求最优性条件（一阶条件，即梯度或次梯度为 0）时希望面对的简洁形式。

> 说明：此页（Page 27）原文到此结束。第 7 题 (a) 的最优性条件的完整推导、以及 (b) 的 SDP 表述、(c) 的数值结果与残差直方图，均位于原书第 28 页及之后，不属于本次给定的第 21–27 页范围。
>
---

---

## Problem 7（续）：ℓ1.5 范数逼近（ℓ1.5-norm approximation）——(a) 的最优化条件与 (b)(c)

### （第 28 页，承接上一部分）

这个目标函数其实是可微的（differentiable），因此最优化条件（optimality condition）就简化为「梯度（gradient）应等于零」。（但它不是二次可微（twice differentiable）的。）梯度为

```math
\nabla f(x) = \sum_{i=1}^{m} \frac{3}{2}\,\operatorname{sign}\bigl(a_i^T x - b_i\bigr)\,\bigl|a_i^T x - b_i\bigr|^{1/2}\,a_i,
```

所以最优化条件就是

```math
\sum_{i=1}^{m} \frac{3}{2}\,\operatorname{sign}(r_i)\,|r_i|^{1/2}\,a_i = 0,
```

其中 $r_i = a_i^T x - b_i$ 是第 $i$ 个残差（residual）。我们当然可以丢掉因子 $\tfrac{3}{2}$。

> 译注：这一步用到的是「无约束可微凸函数最优点处梯度为零」的一阶最优性判据（first-order optimality condition）。因为目标函数 $\sum_{i=1}^m |a_i^T x - b_i|^{3/2}$ 是 $m$ 个 $\tfrac32$ 次幂函数的和，而每一项都是凸函数（绝对值函数是凸的、幂次 $p \ge 1$ 的复合仍凸），因此整个目标是无约束的凸函数；又因为每一项对 $x$ 可微（尽管在 $r_i=0$ 处二阶不可微），故极小点当且仅当梯度为零。逐项对 $x$ 求导时，由链式法则 $\dfrac{d}{dx}|a_i^T x - b_i|^{3/2} = \dfrac{3}{2}\,|r_i|^{1/2}\,\operatorname{sign}(r_i)\,a_i$，其中 $\operatorname{sign}(r_i)$ 是 $r_i$ 的符号函数，$|r_i|^{1/2}$ 保证在 $r_i \to 0$ 时该项仍连续（这正是「一阶可微但二阶不可微」的体现）。

**(b)** 我们可以写出一个等价问题（equivalent problem）：

```math
\begin{aligned}
& \text{minimize} & & \mathbf{1}^T t \\
& \text{subject to} & & s^{3/2} \preceq t, \\
& & & -s_i \preceq a_i^T x - b_i \preceq s_i, & i = 1, \ldots, m,
\end{aligned}
```

其中引入了新的变量 $t, s \in \mathbb{R}^m$。

> 译注：这里的 $\mathbf{1}^T t$ 表示分量求和 $\sum_{i=1}^m t_i$。引入松弛变量（slack variable）$s_i$ 与 $t_i$ 后，约束 $-s_i \le a_i^T x - b_i \le s_i$ 等价于 $s_i \ge |a_i^T x - b_i| = |r_i|$；而 $s^{3/2} \preceq t$ 表示逐分量（componentwise）的 $s_i^{3/2} \le t_i$。于是最小化 $\sum_i t_i$ 时，$t_i$ 会被压到尽可能小，即 $t_i = s_i^{3/2} = |r_i|^{3/2}$，从而与原来的 $\sum_i |r_i|^{3/2}$ 等价。这就是把不可直接写出的 $s_i^{3/2}$ 用「上界变量 $t_i$」替换的标准松弛技巧。

我们需要一种用 LMI（linear matrix inequality，线性矩阵不等式）来表达 $s_i^{3/2} \le t_i$ 的方法。我们先把它写成 $s_i^2 \le t_i \sqrt{s_i}$。

> 译注：这一步是一个纯代数恒等变形：$s_i^{3/2} \le t_i$ 两边同乘 $\sqrt{s_i} > 0$（这里默认 $s_i > 0$；$s_i = 0$ 时不等式自然成立，可作极限处理）即得 $s_i^2 \le t_i \sqrt{s_i}$。之所以要化成这个形式，是因为 $\sqrt{s_i}$ 是凹函数（concave function），而后面会用到「二次函数除以一个凹正函数仍凸」这条复合规则。

我们将用一些 LMI 来表达它。回忆一般的 $2\times 2$ LMI：

```math
\begin{bmatrix}
u & v \\
v & w
\end{bmatrix} \succeq 0
```

等价于 $u \ge 0$，$u w \ge v^2$。所以我们可以把 $s_i^2 \le t_i \sqrt{s_i}$ 写成

```math
\begin{bmatrix}
\sqrt{s_i} & s_i \\
s_i & t_i
\end{bmatrix} \succeq 0.
```

> 译注：这里是「二阶对称矩阵半正定（positive semidefinite, PSD）」的充要条件：一个对称 $2\times 2$ 矩阵 $\begin{bmatrix} u & v \\ v & w \end{bmatrix} \succeq 0$ 当且仅当其顺序主子式（leading principal minors）非负，即 $u \ge 0$ 且 $u w - v^2 \ge 0$。把 $u = \sqrt{s_i}$、$v = s_i$、$w = t_i$ 代入，得到 $\sqrt{s_i} \ge 0$ 且 $\sqrt{s_i}\,t_i \ge s_i^2$，后者正是 $s_i^2 \le t_i \sqrt{s_i}$。

不过这还不是 LMI，因为 $(1,1)$ 位置的元素（即 $\sqrt{s_i}$）并不是变量的仿射（affine）函数。为了处理这一点，我们引入一个新变量 $y \in \mathbb{R}^m$，它满足 $0 \preceq y \preceq \sqrt{s}$：

```math
\begin{bmatrix}
y_i & s_i \\
s_i & t_i
\end{bmatrix} \succeq 0,
\qquad
\begin{bmatrix}
s_i & y_i \\
y_i & 1
\end{bmatrix} \succeq 0.
```

这些是关于各变量的 LMI。第一个 LMI 等价于 $y_i \ge 0$，$y_i t_i \ge s_i^2$。第二个 LMI 等价于 $s_i \ge y_i^2$，即 $\sqrt{s_i} \ge |y_i|$。这两者合起来等价于 $s_i^2 \ge t_i \sqrt{s_i}$。

> 译注：这里的关键是用一个「自由」的变量 $y_i$ 去替代原本那个非仿射的 $\sqrt{s_i}$，从而把非线性项拆成两个 LMI。逐个分析：
>
> - 第一个 LMI $\begin{bmatrix} y_i & s_i \\ s_i & t_i \end{bmatrix} \succeq 0$ 由 $2\times 2$ PSD 判据等价于 $y_i \ge 0$ 且 $y_i t_i \ge s_i^2$。由于 $y_i \ge 0$，$y_i t_i \ge s_i^2$ 蕴含了 $t_i \ge s_i^2 / y_i$。
> - 第二个 LMI $\begin{bmatrix} s_i & y_i \\ y_i & 1 \end{bmatrix} \succeq 0$ 等价于 $s_i \ge 0$ 且 $s_i \cdot 1 \ge y_i^2$，即 $s_i \ge y_i^2$，也就是 $\sqrt{s_i} \ge |y_i|$。又因为 $y_i \ge 0$，故 $\sqrt{s_i} \ge y_i$。
>
> 合起来：我们想要的是 $s_i^2 \le t_i \sqrt{s_i}$（即 $t_i \ge s_i^2 / \sqrt{s_i}$）。两个 LMI 只给出 $t_i \ge s_i^2 / y_i$ 与 $\sqrt{s_i} \ge y_i$，所以只需证明「存在 $y_i$ 满足 $0 \le y_i \le \sqrt{s_i}$ 且 $t_i \ge s_i^2/y_i$」等价于「$t_i \ge s_i^2/\sqrt{s_i}$」。（这里用到一个事实：增大一个矩阵的 $(1,1)$ 元素会让它「更加」半正定。这个说法虽然不太正式，但你应该明白我们的意思。）

现在我们可以组装出一个 SDP（semidefinite program，半定规划）来求解我们的 ℓ1.5 范数逼近问题：

```math
\begin{aligned}
& \text{minimize} & & \mathbf{1}^T t \\
& \text{subject to} & & -s_i \preceq a_i^T x - b_i \preceq s_i, & i = 1, \ldots, m \\
& & &
\begin{bmatrix}
y_i & s_i \\
s_i & t_i
\end{bmatrix} \succeq 0,
& i = 1, \ldots, m \\
& & &
\begin{bmatrix}
s_i & y_i \\
y_i & 1
\end{bmatrix} \succeq 0,
& i = 1, \ldots, m,
\end{aligned}
```

### （第 29 页）

其中变量为 $x \in \mathbb{R}^n$，$t \in \mathbb{R}^m$，$y \in \mathbb{R}^m$，$s \in \mathbb{R}^m$。

下面是你们当中好几位同学使用的另一种解法，我们也很喜欢。最终的 SDP 是：

```math
\begin{aligned}
& \text{minimize} & & z \\
& \text{subject to} & & -s_i \preceq a_i^T x - b_i \preceq s_i, & i = 1, \ldots, m \\
& & &
\begin{bmatrix}
s_i & y_i \\
y_i & 1
\end{bmatrix} \succeq 0,
& i = 1, \ldots, m \\
& & &
\begin{bmatrix}
z & s^T \\
s & \operatorname{diag}(y)
\end{bmatrix} \succeq 0,
\end{aligned}
```

其中变量为 $x \in \mathbb{R}^n$，$y \in \mathbb{R}^m$，$s \in \mathbb{R}^m$，以及 $z \in \mathbb{R}$。

> 译注：这一版与上一版的区别在于，用单个标量 $z$ 取代了向量 $t$，并把「所有 $s_i^{3/2} \le t_i$」的 $m$ 个约束合并成一个大 LMI。思路是：先由 $2\times 2$ LMI 得到 $y_i \ge 0$ 且 $s_i \ge y_i^2$（即 $y_i$ 是 $\sqrt{s_i}$ 的下界），再用最后一个 $(2m+1)\times(2m+1)$ 的 LMI 一次性表达 $z \ge \sum_i s_i^2 / y_i$。

第一组不等式等价于 $|a_i^T x - b_i| \le s_i$；这组 $2\times 2$ LMI 等价于 $s_i \ge y_i^2$；而最后一个 $(2m+1)\times(2m+1)$ LMI 等价于

```math
z \ge s^T \bigl(\operatorname{diag}(y)^{-1}\bigr) s = \sum_{i=1}^{m} \frac{s_i^2}{y_i}.
```

> 译注：最后一个大 LMI 用到的是「分块矩阵半正定」的 Schur 补（Schur complement）判据：对于分块对称矩阵 $\begin{bmatrix} A & B^T \\ B & C \end{bmatrix}$，当 $C \succ 0$ 时，它半正定当且仅当 Schur 补 $A - B^T C^{-1} B \succeq 0$。这里 $A = z$（$1\times 1$），$B = s$，$C = \operatorname{diag}(y)$。因为 $y_i \ge 0$ 且（在有效解处）$y_i > 0$，故 $\operatorname{diag}(y)$ 可逆，Schur 补为 $z - s^T \operatorname{diag}(y)^{-1} s \ge 0$，即 $z \ge \sum_i s_i^2 / y_i$。

显然我们是在最小化 $z$，因此也就是在最小化上式右端。对于固定的 $s_i$，选择 $y_i = \sqrt{s_i}$ 能使目标函数最小化，所以我们在最小化的正是

```math
\sum_{i=1}^{m} \frac{s_i^2}{y_i} = \sum_{i=1}^{m} s_i^{3/2}.
```

> 译注：这一段的逻辑链条是「逐分量放缩 + 边界取等」。因为约束要求 $s_i \ge y_i^2$（即 $y_i \le \sqrt{s_i}$），而目标项 $s_i^2 / y_i$ 关于 $y_i$ 单调递减，所以在固定 $s_i$ 时，让 $y_i$ 取最大允许值 $y_i = \sqrt{s_i}$ 才能把目标压到最小；代入即得 $s_i^2 / \sqrt{s_i} = s_i^{3/2}$。这正是上一版「增大 $(1,1)$ 元素使矩阵更半正定」这一直觉的严格化：$y_i$ 越接近 $\sqrt{s_i}$，$z$ 的下界就越紧，最优时二者相等。

**(c)** 我们将用 cvx 来求解这个问题。函数 `norm(r,1.5)` 目前还没有实现，所以我们必须自己动手。一个简单的方法是注意到 $|r|^{3/2} = r^2 / \sqrt{r}$，这是二次除以线性函数（quadratic-over-linear function）$x_1^2 / x_2$ 在 $x_1 = r$、$x_2 = \sqrt{r}$ 处的复合。幸运的是，这个复合的结果是凸的，因为二次除以线性函数是凸的，并且关于它的第二个自变量是递减的，所以可以在第二个自变量位置上接受一个凹的正函数。换句话说，cvx 会接受 `quad_over_lin(s,sqrt(s))` 并把它识别为凸的。于是我们就有了一个干脆利落的方法，用来在 $s > 0$ 时表达 $s^{3/2}$。现在我们必须把这个凸函数与凸函数 $r_i = a_i^T x - b_i$ 复合起来。下面是一种做法。

> 译注：这一段用到凸函数的复合规则（composition rule）：二次除以线性函数 $f(x_1, x_2) = x_1^2 / x_2$（定义域 $x_2 > 0$）是凸函数，且它对 $x_2$ 单调递减；而 $\sqrt{s}$ 是凹函数（且为正）。凸函数在一个单调递减的自变量位置上复合一个凹函数，结果仍是凸的。因此 $s^{3/2} = s^2 / \sqrt{s}$ 对 $s > 0$ 是凸的。CVX 的 DCP（disciplined convex programming，规范凸编程）规则正是依据这条复合规则来自动验证凸性的，所以它能识别出 `quad_over_lin(s, sqrt(s))` 是凸的。

```matlab
cvx_begin
    variables x(n) s(m);
    s >= abs(A*x-b);
    minimize(sum(quad_over_lin(s,sqrt(s),0)));
cvx_end
```

> 译注：`s >= abs(A*x-b)` 对应约束 $s_i \ge |a_i^T x - b_i|$（即 $s_i \ge |r_i|$）；`quad_over_lin(s,sqrt(s),0)` 计算的就是 $\sum_i s_i^2 / \sqrt{s_i} = \sum_i s_i^{3/2}$，其中第三个参数 `0` 是 CVX 中该函数的可选参数（用于指定分母的某个平移，这里取 0 表示不平移）。

下面的代码针对不同的范数求解该问题，并绘制残差的直方图（histogram）。

```matlab
n=30;
m=100;
randn('state',0);
```

### （第 30 页）

```matlab
A=randn(m,n);
b=randn(m,1);
% l1.5 solution
cvx_begin
    variables x(n) s(m);
    s >= abs(A*x-b);
    minimize(sum(quad_over_lin(s,sqrt(s),0)));
cvx_end
% l2 solution
xl2=A\b;
% l1 solution
cvx_begin
    variables xl1(n);
    minimize(norm(A*xl1-b,1));
cvx_end
r=A*x-b;      % residuals
rl2=A*xl2-b;
rl1=A*xl1-b;
% check optimality condition
A'*(3/2*sign(r).*sqrt(abs(r)))
subplot(3,1,1)
hist(r)
axis([-2.5 2.5 0 50])
xlabel('r')
subplot(3,1,2)
hist(rl2)
axis([-2.5 2.5 0 50])
xlabel('r2')
subplot(3,1,3)
hist(rl1)
axis([-2.5 2.5 0 50])
xlabel('r1')
% solution using SDP
cvx_begin sdp
```

### （第 31 页）

```matlab
variables xdf(n) r(m) y(m) t(m);
A*xdf-b<=r;
-r<=A*xdf-b;
minimize(sum(t));
for i=1:m
    [y(i), r(i); r(i), t(i)]>=0;
    [r(i), y(i); y(i), 1]>=0;
end
cvx_end
```

> 译注：这是 (b) 部分第一个 SDP 建模的 CVX 实现。`A*xdf-b<=r; -r<=A*xdf-b;` 对应 $-s_i \le a_i^T x - b_i \le s_i$（这里变量名 $r$ 即上一版中的 $s$）；循环体内两个 `[...]>=0` 正是那两组 $2\times 2$ LMI：`[y(i), r(i); r(i), t(i)]>=0` 即 $\begin{bmatrix} y_i & s_i \\ s_i & t_i \end{bmatrix} \succeq 0$，`[r(i), y(i); y(i), 1]>=0` 即 $\begin{bmatrix} s_i & y_i \\ y_i & 1 \end{bmatrix} \succeq 0$。目标 `sum(t)` 即 $\mathbf{1}^T t$。前面的 `%check optimality condition` 一行 `A'*(3/2*sign(r).*sqrt(abs(r)))` 是检验 (a) 中的一阶最优性条件 $\sum_i \tfrac32 \operatorname{sign}(r_i)|r_i|^{1/2} a_i = 0$ 是否被满足（结果应接近零向量）。

图 2 展示了三种范数下残差的直方图。

> 〔原书此处为 Figure 2：三幅并排的直方图，横轴均为残差 $r$（范围 $-2.5$ 到 $2.5$），纵轴为频数（范围 $0$ 到 $50$），自上而下依次标注为 ℓ1.5-norm、ℓ2-norm、ℓ1-norm，分别对应 ℓ1.5 范数、ℓ2 范数（最小二乘）、ℓ1 范数解的残差分布。〕

---

## Problem 8：三路线性分类（three-way linear classification）

### （第 32 页）

给定数据

```math
x^{(1)}, \ldots, x^{(N)}, \qquad
y^{(1)}, \ldots, y^{(M)}, \qquad
z^{(1)}, \ldots, z^{(P)},
```

即 $\mathbb{R}^n$ 中的三组非空向量集合。我们希望找到 $\mathbb{R}^n$ 上的三个仿射函数（affine function），

```math
f_i(u) = a_i^T u - b_i, \qquad i = 1, 2, 3,
```

它们满足如下性质：

```math
f_1\bigl(x^{(j)}\bigr) > \max\bigl\{ f_2\bigl(x^{(j)}\bigr), f_3\bigl(x^{(j)}\bigr) \bigr\}, \qquad j = 1, \ldots, N,
```

```math
f_2\bigl(y^{(j)}\bigr) > \max\bigl\{ f_1\bigl(y^{(j)}\bigr), f_3\bigl(y^{(j)}\bigr) \bigr\}, \qquad j = 1, \ldots, M,
```

```math
f_3\bigl(z^{(j)}\bigr) > \max\bigl\{ f_1\bigl(z^{(j)}\bigr), f_2\bigl(z^{(j)}\bigr) \bigr\}, \qquad j = 1, \ldots, P.
```

用语言来说：$f_1$ 在 $x$ 数据点上是三个函数中最大的，$f_2$ 在 $y$ 数据点上是三个函数中最大的，$f_3$ 在 $z$ 数据点上是三个函数中最大的。我们可以给出一个简单的几何解释：函数 $f_1, f_2, f_3$ 把 $\mathbb{R}^n$ 划分成三个区域，

```math
R_1 = \bigl\{ u \mid f_1(u) > \max\{f_2(u), f_3(u)\} \bigr\},
```

```math
R_2 = \bigl\{ u \mid f_2(u) > \max\{f_1(u), f_3(u)\} \bigr\},
```

```math
R_3 = \bigl\{ u \mid f_3(u) > \max\{f_1(u), f_2(u)\} \bigr\},
```

其中每个区域由「哪个函数是三者中最大的」来定义。我们的目标是找到满足 $x^{(j)} \in R_1$、$y^{(j)} \in R_2$、$z^{(j)} \in R_3$ 的函数。

请把这个问题建模成一个凸优化问题（convex optimization problem）。在你的建模中不得使用严格不等式（strict inequalities）。

> 译注：本题的核心是把「三选一的最大」这一逻辑条件，拆解成若干个线性不等式。注意原文给出的 $R_i$ 定义里用的是记号 $z$ 表示一般的点，但前面 $z^{(j)}$ 又是第三组数据，这里为避免混淆，我把区域定义中的自变量统一写成 $u$（原文此处用 $z$ 仅作为区域内的通用点记号，与数据点 $z^{(j)}$ 无关）。凸优化问题的标准形式要求所有约束是「非严格不等式」$(\le)$ 或等式，因此后面必须把 $>$ 严格不等式化为 $\ge$（见解答的「齐次性 + 归一化」技巧）。

### Solution（解答）

我们需要

```math
f_1\bigl(x^{(j)}\bigr) > f_2\bigl(x^{(j)}\bigr), \qquad j = 1, \ldots, N,
```

```math
f_1\bigl(x^{(j)}\bigr) > f_3\bigl(x^{(j)}\bigr), \qquad j = 1, \ldots, N,
```

```math
f_2\bigl(y^{(j)}\bigr) > f_1\bigl(y^{(j)}\bigr), \qquad j = 1, \ldots, M,
```

```math
f_2\bigl(y^{(j)}\bigr) > f_3\bigl(y^{(j)}\bigr), \qquad j = 1, \ldots, M,
```

```math
f_3\bigl(z^{(j)}\bigr) > f_1\bigl(z^{(j)}\bigr), \qquad j = 1, \ldots, P,
```

```math
f_3\bigl(z^{(j)}\bigr) > f_2\bigl(z^{(j)}\bigr), \qquad j = 1, \ldots, P,
```

这是关于变量 $a_i$ 和 $b_i$ 的一组共 $2(M+N+P)$ 个严格线性不等式。它们在 $a_i$ 和 $b_i$ 上是齐次的（homogeneous），所以我们可以把它们表达为

```math
f_1\bigl(x^{(j)}\bigr) \ge f_2\bigl(x^{(j)}\bigr) + 1, \qquad j = 1, \ldots, N,
```

```math
f_1\bigl(x^{(j)}\bigr) \ge f_3\bigl(x^{(j)}\bigr) + 1, \qquad j = 1, \ldots, N,
```

```math
f_2\bigl(y^{(j)}\bigr) \ge f_1\bigl(y^{(j)}\bigr) + 1, \qquad j = 1, \ldots, M,
```

```math
f_2\bigl(y^{(j)}\bigr) \ge f_3\bigl(y^{(j)}\bigr) + 1, \qquad j = 1, \ldots, M,
```

```math
f_3\bigl(z^{(j)}\bigr) \ge f_1\bigl(z^{(j)}\bigr) + 1, \qquad j = 1, \ldots, P,
```

```math
f_3\bigl(z^{(j)}\bigr) \ge f_2\bigl(z^{(j)}\bigr) + 1, \qquad j = 1, \ldots, P,
```

这是一个 LP（linear program，线性规划）可行性问题（feasibility problem）。

> 译注：这一步是本题的关键技巧，分两点说明：
>
> 1. **为什么 $>$ 能换成 $\ge +1$？** 因为所有不等式关于 $(a_i, b_i)$ 是齐次的——也就是说，如果 $(a_i, b_i)$ 满足某组严格不等式，那么把它们整体乘以任意正数 $\lambda > 0$ 后仍然满足，并且不等号两边同乘 $\lambda$，差也会被放大 $\lambda$ 倍。于是只要存在一个满足 $>$ 的解，就可以把它整体放大到使每个差值都至少达到 $1$（甚至任意大的常数），从而满足 $\ge +1$ 的形式。反之，满足 $\ge +1$ 的解当然也满足原来的 $>$。所以两者在「是否可行」这个意义上是等价的。
>
> 2. **为什么这是凸（其实是线性）的？** 每条不等式 $f_1(x^{(j)}) - f_2(x^{(j)}) \ge 1$ 展开后是 $(a_1 - a_2)^T x^{(j)} - (b_1 - b_2) \ge 1$，它关于变量 $(a_1, a_2, b_1, b_2)$ 是线性的，因此整个问题是线性可行性问题，自然是凸优化问题。注意这里没有任何目标函数（或者说目标恒为常数 0），所以它是一个「可行性问题」而非一般的「优化问题」。

### （第 33 页）

更明确地写出来，我们有

```math
(a_1 - a_2)^T x^{(j)} - (b_1 - b_2) \ge 1, \qquad j = 1, \ldots, N,
```

```math
(a_1 - a_3)^T x^{(j)} - (b_1 - b_3) \ge 1, \qquad j = 1, \ldots, N,
```

```math
(a_2 - a_1)^T y^{(j)} - (b_2 - b_1) \ge 1, \qquad j = 1, \ldots, M,
```

```math
(a_2 - a_3)^T y^{(j)} - (b_2 - b_3) \ge 1, \qquad j = 1, \ldots, M,
```

```math
(a_3 - a_1)^T z^{(j)} - (b_3 - b_1) \ge 1, \qquad j = 1, \ldots, P,
```

```math
(a_3 - a_2)^T z^{(j)} - (b_3 - b_2) \ge 1, \qquad j = 1, \ldots, P.
```

> 译注：这六组式子就是把上一步的 $f_i(u) = a_i^T u - b_i$ 直接代入并展开。以第一组为例，$f_1(x^{(j)}) - f_2(x^{(j)}) = (a_1^T x^{(j)} - b_1) - (a_2^T x^{(j)} - b_2) = (a_1 - a_2)^T x^{(j)} - (b_1 - b_2)$。其余各组同理。

第一组和第三组说的是：仿射函数 $(a_1 - a_2)^T u - (b_1 - b_2)$ 严格分离（separate）了 $x^{(j)}$ 和 $y^{(j)}$。第二组和第五组说的是：仿射函数 $(a_1 - a_3)^T u - (b_1 - b_3)$ 严格分离了 $x^{(j)}$ 和 $z^{(j)}$。第四组和第六组说的是：仿射函数 $(a_2 - a_3)^T u - (b_2 - b_3)$ 严格分离了 $y^{(j)}$ 和 $z^{(j)}$。

> 译注：这里揭示了三路分类的一个深层结构——它等价于「三对分离超平面（separating hyperplane）」。对每一对数据类（$x$ 与 $y$、$x$ 与 $z$、$y$ 与 $z$），都存在一个由相应仿射函数给出的分离超平面，把其中一类严格分在正侧、另一类严格分在负侧。因此「三路分类」可归结为「三组二分类线性可分」的组合。

注意，我们可以给每个 $a_i$ 都加上任意向量 $\alpha$，而不影响这些不等式（因为它们只涉及 $a_i$ 之间的差）；同理，我们也可以给每个 $b_i$ 都加上任意数 $\beta$。我们可以利用这个观察来对 $a_i$ 和 $b_i$ 做归一化（normalize）或简化。例如，我们可以不失一般性（without loss of generality）地假设 $a_1 = 0$，$b_1 = 0$。或者，用一种更对称的方式，我们可以要求 $a_1 + a_2 + a_3 = 0$ 且 $b_1 + b_2 + b_3 = 0$。

> 译注：这一步说的是「平移不变性」。所有不等式都只包含 $a_i - a_j$ 与 $b_i - b_j$ 这样的差，因此同时给三个 $a_i$ 加同一个 $\alpha$、给三个 $b_i$ 加同一个 $\beta$，所有差都不变，可行性不受影响。于是我们可以利用这个自由度（共 $n + 1$ 个：$\alpha$ 有 $n$ 个分量、$\beta$ 有 1 个）来固定某些变量，例如令 $a_1 = 0, b_1 = 0$（固定掉 $n+1$ 个自由变量），或采用对称形式 $a_1+a_2+a_3=0$、$b_1+b_2+b_3=0$（这是把「重心」平移到原点）。这样做的目的是消除解的非唯一性，使问题确定化。

我们没有要求你在数值数据上试验它。但我们自己试了一下，纯粹为了好玩，也为了确认我们的理论确实是对的。

下面的 MATLAB 脚本在三个小型的可分离数据集上实现了这个三路分类方法并加以测试：

```matlab
sep3way_data;
cvx_begin
    variables a1(2) a2(2) a3(2) b1 b2 b3
    (a1 - a2)'*X - (b1 - b2) >= 1
    (a1 - a3)'*X - (b1 - b3) >= 1
    (a2 - a1)'*Y - (b2 - b1) >= 1
    (a2 - a3)'*Y - (b2 - b3) >= 1
    (a3 - a1)'*Z - (b3 - b1) >= 1
    (a3 - a2)'*Z - (b3 - b2) >= 1
    a1 + a2 + a3 == 0
    b1 + b2 + b3 == 0
cvx_end
% maximally confusing point
p = [(a1-a2)';(a1-a3)'] \ [(b1-b2);(b1-b3)];
% plot
```

### （第 34 页）

```matlab
plot(X(1,:),X(2,:),'*');
hold on;
plot(Y(1,:),Y(2,:),'ro');
plot(Z(1,:),Z(2,:),'g+');
plot(p(1),p(2),'ks');
t = [-5:0.01:8];
u1 = a1 - a2; v1 = b1 - b2;
u2 = a2 - a3; v2 = b2 - b3;
u3 = a3 - a1; v3 = b3 - b1;
line1 = (-t*u1(1) + v1)/u1(2);
idx1 = find(u2'*[t;line1] - v2 > 0);
plot(t(idx1),line1(idx1));
line2 = (-t*u2(1) + v2)/u2(2);
idx2 = find(u3'*[t; line2] - v3 > 0);
plot(t(idx2),line2(idx2),'r');
line3 = (-t*u3(1) + v3)/u3(2);
idx3 = find(u1'*[t; line3] - v1 > 0);
plot(t(idx3),line3(idx3),'g');
```

> 译注：这段绘图脚本的几何含义如下。
>
> - 前四行把三组二维数据分别用 `*`（$X$，星号）、`ro`（$Y$，红色圆圈）、`g+`（$Z$，绿色加号）画出来，并用 `ks`（黑色方块）标出「最容易混淆的点」$p$。
> - `p` 的计算 `[(a1-a2)';(a1-a3)'] \ [(b1-b2);(b1-b3)]` 解的是线性方程组 $(a_1-a_2)^T p = b_1-b_2$ 与 $(a_1-a_3)^T p = b_1-b_3$，即三个区域边界中「$R_1$ 与 $R_2$ 的边界线」和「$R_1$ 与 $R_3$ 的边界线」的交点——它是三路分类最难判定的点（三个区域的公共交点）。
> - 三对 `line1/line2/line3` 分别画出三条分离直线的方程：以 `line1` 为例，由 $(a_1 - a_2)^T u - (b_1 - b_2) = 0$，令 $u = (t, \text{line1})$，解得 $\text{line1} = \dfrac{-t\,u_1(1) + v_1}{u_1(2)}$（即 $u_1(1) t + u_1(2)\,\text{line1} - v_1 = 0$ 的显式解）。`idx1` 用 `u2'*[t;line1] - v2 > 0` 筛选出位于区域 $R_1$（相对 $R_2$ 的边界）正侧的那段线段再画出，从而只显示真正构成区域边界的那一段。

生成的图形如下。

> 〔原书此处为一幅二维散点图：横轴范围约 $-4$ 到 $8$，纵轴范围约 $-6$ 到 $6$，图中以星号、红色圆圈、绿色加号分别表示三组数据，三条分离直线把平面划分为三个区域，黑色方块标出最易混淆的交点 $p$。〕

---

## 全卷小结

全卷（第 1–34 页）的收尾部分收束了 Problem 7（ℓ1.5 范数逼近）并完整解答了 Problem 8（三路线性分类）：

- **Problem 7** 通过一阶最优性条件给出了 $\sum_i \tfrac32 \operatorname{sign}(r_i)|r_i|^{1/2} a_i = 0$ 的解析刻画，进而用两组 $2\times 2$ LMI（借助 Schur 补与「增大 $(1,1)$ 元素使矩阵更半正定」的技巧）把非线性的 $s_i^{3/2}$ 项转化为半定规划（SDP），并给出了两种等价的 SDP 建模与对应的 CVX 代码。
- **Problem 8** 把三路分类拆成 $2(M+N+P)$ 个线性不等式，利用齐次性把严格不等式 $>$ 化为 $\ge +1$，得到一个 LP 可行性问题，并揭示了它与「三对分离超平面」的等价关系；最后用 MATLAB/CVX 在二维数据上实现了分类并画出三个区域。
