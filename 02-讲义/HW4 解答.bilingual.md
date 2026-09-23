> <span style="color:#7f8c8d;">Source: 作业解答/HW4 解答.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

## EE364a Homework 4 Solutions（EE364a 作业 4 解答）

> <span style="color:#7f8c8d;">EE364a, Winter 2007-08 · Prof. S. Boyd · 本文档为作业 4 的官方解答，覆盖教材习题 4.11、4.16、4.29、4.30、5.1 及附加题 1–5</span>

<!-- p.1 -->

### <span style="color:#c0392b;">4.11 涉及 ℓ1 范数与 ℓ∞ 范数的问题（Problems involving ℓ1- and ℓ∞-norms）</span>

<span style="color:#2471a3;">**[question]**</span>

**4.11 Problems involving ℓ1- and ℓ∞-norms.** Formulate the following problems as LPs. Explain in detail the relation between the optimal solution of each problem and the solution of its equivalent LP.

(a) Minimize $\|Ax-b\|_\infty$ (ℓ∞-norm approximation).

(b) Minimize $\|Ax-b\|_1$ (ℓ1-norm approximation).

(c) Minimize $\|Ax-b\|_1$ subject to $\|x\|_\infty \le 1$.

(d) Minimize $\|x\|_1$ subject to $\|Ax-b\|_\infty \le 1$.

(e) Minimize $\|Ax-b\|_1 + \|x\|_\infty$.

In each problem, $A \in \mathbb{R}^{m\times n}$ and $b \in \mathbb{R}^m$ are given. (See §6.1 for more problems involving approximation and constrained approximation.)

<span style="color:#2471a3;">**[solution]**</span>

> **解答（Solution）.** (a) 等价于如下 LP（linear program，线性规划）：
>
> ```math
> \begin{array}{ll}
> \text{minimize} & t \\
> \text{subject to} & Ax - b \preceq t\mathbf{1} \\
> & Ax - b \succeq -t\mathbf{1}
> \end{array}
> ```
>
> 变量为 $x \in \mathbb{R}^n$ 与 $t \in \mathbb{R}$。为看出等价性，固定 $x$，只对 $t$ 优化。约束条件说明：对每个 $k$ 都有 $-t \le a_k^T x - b_k \le t$，即 $t \ge |a_k^T x - b_k|$，也就是
>
> ```math
> t \;\ge\; \max_{k}\, |a_k^T x - b_k| \;=\; \|Ax - b\|_\infty .
> ```
>
> 显然，若固定 $x$，该 LP 的最优值为 $p^\star(x) = \|Ax - b\|_\infty$。因此同时对 $t$ 与 $x$ 优化等价于原问题。

<!-- p.2 -->

> (b) 等价于如下 LP：
>
> ```math
> \begin{array}{ll}
> \text{minimize} & \mathbf{1}^T s \\
> \text{subject to} & Ax - b \preceq s \\
> & Ax - b \succeq -s
> \end{array}
> ```
>
> 变量为 $x \in \mathbb{R}^n$ 与 $s \in \mathbb{R}^m$。假设 $x$ 固定，只对 $s$ 优化。约束条件说明：对每个 $k$ 都有 $-s_k \le a_k^T x - b_k \le s_k$，即 $s_k \ge |a_k^T x - b_k|$。LP 的目标函数是可分的（separable），所以对 $s$ 取到最优时选择
>
> ```math
> s_k = |a_k^T x - b_k|,
> ```
>
> 从而得到最优值 $p^\star(x) = \|Ax - b\|_1$。因此同时对 $x$ 与 $s$ 优化等价于原问题。
>
> (c) 等价于如下 LP：
>
> ```math
> \begin{array}{ll}
> \text{minimize} & \mathbf{1}^T y \\
> \text{subject to} & -y \preceq Ax - b \preceq y \\
> & -\mathbf{1} \preceq x \preceq \mathbf{1}
> \end{array}
> ```
>
> 变量为 $x \in \mathbb{R}^n$ 与 $y \in \mathbb{R}^m$。
>
> (d) 等价于如下 LP：
>
> ```math
> \begin{array}{ll}
> \text{minimize} & \mathbf{1}^T y \\
> \text{subject to} & -y \preceq x \preceq y \\
> & -\mathbf{1} \preceq Ax - b \preceq \mathbf{1}
> \end{array}
> ```
>
> 变量为 $x \in \mathbb{R}^n$ 与 $y \in \mathbb{R}^n$。
>
> 另一种改写方式是把 $x$ 写成两个非负向量之差 $x = x^+ - x^-$，并把问题表达为
>
> ```math
> \begin{array}{ll}
> \text{minimize} & \mathbf{1}^T x^+ + \mathbf{1}^T x^- \\
> \text{subject to} & -\mathbf{1} \preceq Ax^+ - Ax^- - b \preceq \mathbf{1} \\
> & x^+ \succeq 0, \qquad x^- \succeq 0
> \end{array}
> ```
>
> 变量为 $x^+ \in \mathbb{R}^n$ 与 $x^- \in \mathbb{R}^n$。
>
> (e) 等价于
>
> ```math
> \begin{array}{ll}
> \text{minimize} & \mathbf{1}^T y + t \\
> \text{subject to} & -y \preceq Ax - b \preceq y \\
> & -t\mathbf{1} \preceq x \preceq t\mathbf{1}
> \end{array}
> ```
>
> 变量为 $x \in \mathbb{R}^n$、$y \in \mathbb{R}^m$ 与 $t \in \mathbb{R}$。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这一组小题从头到尾只用了一个核心技巧：为范数型目标引进辅助变量（上图 epigraph 技巧），把「最小化某个范数」改写成「最小化一个辅助标量 + 一组线性不等式」。背后的第一性原理是：$\|Ax-b\|_\infty \le t$ 恰好等价于逐分量的线性不等式 $-t \le a_k^T x - b_k \le t$，而 $\|\cdot\|_1$ 可以拆成绝对值之和，所以这些目标都能被 LP 精确表达，不需要近似或采样。换句话说，辅助变量 $t$（或向量 $s,y$）在最优解处会自动收紧到当前点的范数值，这正是「用约束挤压出目标」的机制。更抽象地看，这是把「优化问题」归约为「可行性问题」：先固定一个目标水平，再问「是否存在目标不超过该水平的可行点」——这与 6.006 中把优化问题归约为判定问题（decision problem）、再用二分搜索逼近最优值的套路同构，只是凸优化里的二分常常已内嵌在求解器内部。一个实用口诀：目标里出现 max、绝对值或范数，第一反应就是加 epigraph 变量；而 $\ell_1$ 范数此后还会与稀疏性、正则化（regularization）深度绑定，是本书后几章多条主线的种子。

<!-- p.3 -->

### <span style="color:#c0392b;">4.16 最小燃料最优控制（Minimum fuel optimal control）</span>

<span style="color:#2471a3;">**[question]**</span>

**4.16 Minimum fuel optimal control.** We consider a linear dynamical system with state $x(t) \in \mathbb{R}^n$, $t = 0, \ldots, N$, and actuator or input signal $u(t) \in \mathbb{R}$, for $t = 0, \ldots, N-1$. The dynamics of the system is given by the linear recurrence

```math
x(t+1) = Ax(t) + bu(t), \qquad t = 0, \ldots, N-1,
```

where $A \in \mathbb{R}^{n\times n}$ and $b \in \mathbb{R}^n$ are given. We assume that the initial state is zero, i.e., $x(0) = 0$.

The minimum fuel optimal control problem is to choose the inputs $u(0), \ldots, u(N-1)$ so as to minimize the total fuel consumed, which is given by

```math
F = \sum_{t=0}^{N-1} f(u(t)),
```

subject to the constraint that $x(N) = x_{\mathrm{des}}$, where $N$ is the (given) time horizon, and $x_{\mathrm{des}} \in \mathbb{R}^n$ is the (given) desired final or target state. The function $f : \mathbb{R} \to \mathbb{R}$ is the fuel use map for the actuator, and gives the amount of fuel used as a function of the actuator signal amplitude. In this problem we use

```math
f(a) = \begin{cases} |a|, & |a| \le 1, \\ 2|a| - 1, & |a| > 1. \end{cases}
```

f is proportional to $|a|$ for actuator signals between $-1$ and $1$; for larger actuator signals the marginal fuel efficiency is half.

Formulate the minimum fuel optimal control problem as an LP.

<span style="color:#2471a3;">**[solution]**</span>

> **解答（Solution）.** 最小燃料最优控制问题等价于如下 LP：
>
> ```math
> \begin{array}{ll}
> \text{minimize} & \mathbf{1}^T t \\
> \text{subject to} & Hu = x_{\mathrm{des}} \\
> & -y \preceq u \preceq y \\
> & t \succeq y \\
> & t \succeq 2y - \mathbf{1}
> \end{array}
> ```
>
> 变量为 $u \in \mathbb{R}^N$、$y \in \mathbb{R}^N$ 与 $t \in \mathbb{R}^N$，其中
>
> ```math
> H = \left[\; A^{N-1}b \quad A^{N-2}b \quad \cdots \quad Ab \quad b \;\right].
> ```
>
> 还有若干种其他可行的 LP 表述。例如，可以把状态轨迹 $x(0), \ldots, x(N)$ 保留为优化变量，并把上面的等式约束 $Hu = x_{\mathrm{des}}$ 换成如下等式约束组：
>
> ```math
> x(t+1) = Ax(t) + bu(t), \quad t = 0, \ldots, N-1, \qquad x(0) = 0, \qquad x(N) = x_{\mathrm{des}}.
> ```
>
> 在这一表述下，变量是 $u \in \mathbb{R}^N$、$x(0), \ldots, x(N) \in \mathbb{R}^n$，以及 $y \in \mathbb{R}^N$ 与 $t \in \mathbb{R}^N$。
>
> 再一种变体是不使用上面引入的中间变量 $y$，只用变量 $t$（与 $u$）来表达问题：
>
> ```math
> -t \preceq u \preceq t, \qquad 2u - \mathbf{1} \preceq t, \qquad -2u - \mathbf{1} \preceq t,
> ```
>
> 变量为 $u \in \mathbb{R}^N$ 与 $t \in \mathbb{R}^N$。

<!-- p.4 -->

### <span style="color:#c0392b;">4.29 最大化线性不等式被满足的概率（Maximizing probability of satisfying a linear inequality）</span>

<span style="color:#2471a3;">**[question]**</span>

**4.29 Maximizing probability of satisfying a linear inequality.** Let $c$ be a random variable in $\mathbb{R}^n$, normally distributed with mean $\bar{c}$ and covariance matrix $R$. Consider the problem

```math
\begin{array}{ll}
\text{maximize} & \operatorname{prob}(c^T x \ge \alpha) \\
\text{subject to} & Fx \preceq g, \\
& Ax = b.
\end{array}
```

Find the conditions under which this is equivalent to a convex or quasiconvex optimization problem. When these conditions hold, formulate the problem as a QP, QCQP, or SOCP (if the problem is convex), or explain how you can solve it by solving a sequence of QP, QCQP, or SOCP feasibility problems (if the problem is quasiconvex).

<span style="color:#2471a3;">**[solution]**</span>

> **解答（Solution）.** 定义 $u = c^T x$，它是一个标量随机变量，服从正态分布（normal distribution），均值为 $\mathbb{E}\,u = \bar{c}^T x$，方差为 $\mathbb{E}(u - \mathbb{E}\,u)^2 = x^T R x$。随机变量
>
> ```math
> \frac{u - \bar{c}^T x}{\sqrt{x^T R x}}
> ```
>
> 服从均值为零、方差为 1 的正态分布，因此
>
> ```math
> \operatorname{prob}(u \ge \alpha) = \operatorname{prob}\!\left( \frac{u - \bar{c}^T x}{\sqrt{x^T R x}} \ge \frac{\alpha - \bar{c}^T x}{\sqrt{x^T R x}} \right) = 1 - \Phi\!\left( \frac{\alpha - \bar{c}^T x}{\sqrt{x^T R x}} \right),
> ```
>
> 其中 <span style="color:#00838f;">$\Phi(z) = \frac{1}{\sqrt{2\pi}} \int_{-\infty}^{z} e^{-t^2/2}\,dt$ 是标准正态分布的 CDF（cumulative distribution function，累积分布函数）。</span>
>
> 要最大化 $\operatorname{prob}(u \ge \alpha)$，只需最小化 $(\alpha - \bar{c}^T x)/\sqrt{x^T R x}$（因为 $\Phi$ 单调递增），即求解问题
>
> ```math
> \begin{array}{ll}
> \text{maximize} & (\bar{c}^T x - \alpha)/\sqrt{x^T R x} \\
> \text{subject to} & Fx \preceq g \\
> & Ax = b
> \end{array}
> \tag{1}
> ```
>
> 这不是凸优化问题，因为目标函数不是凹的。
>
> 不过，只要某个条件成立，它就可以用拟凸优化（quasiconvex optimization）来求解（条件在下面推导）。目标函数超过某个值 $t$，当且仅当
>
> ```math
> \bar{c}^T x - \alpha \;\ge\; t\sqrt{x^T R x}
> ```
>
> 成立。只要 $t \ge 0$，后面这个不等式就是凸的——事实上是一个二阶锥约束（second-order cone constraint）。于是可以陈述该条件：<span style="color:#00838f;">存在可行点 $x$ 使得 $\bar{c}^T x \ge \alpha$。</span>（这一条件很容易作为一个 LP 可行性问题来检验。）顺便说一句，该条件也可以等价地表述为：存在可行点 $x$ 使得 $\operatorname{prob}(u \ge \alpha) \ge 1/2$。
>
> 假设该条件成立。这意味着原问题的最优值至少是 0.5，且问题 (1) 的最优值至少是 0。于是可以把我们的问题表述为
>
> ```math
> \begin{array}{ll}
> \text{maximize} & t \\
> \text{subject to} & Fx \preceq g, \quad Ax = b \\
> & \bar{c}^T x - \alpha \ge t\sqrt{x^T R x},
> \end{array}
> ```

<!-- p.5 -->

> 其中可以假定 $t \ge 0$。它可以通过对 $t$ 做二分法（bisection）来求解：每一步解一个 SOCP 可行性问题。换句话说：函数 $(\bar{c}^T x - \alpha)/\sqrt{x^T R x}$ 在其为非负时是拟凹的（quasiconcave）。
>
> 事实上，只要上述条件成立（即存在满足 $\bar{c}^T x \ge \alpha$ 的可行点 $x$），我们甚至可以用凸优化来求解问题 (1)。做变量替换
>
> ```math
> y = \frac{x}{\bar{c}^T x - \alpha}, \qquad s = \frac{1}{\bar{c}^T x - \alpha},
> ```
>
> 即 $x = y/s$。这就得到问题
>
> ```math
> \begin{array}{ll}
> \text{minimize} & \sqrt{y^T R y} \\
> \text{subject to} & Fy \preceq gs \\
> & Ay = bs \\
> & \bar{c}^T y - \alpha s = 1 \\
> & s \ge 0.
> \end{array}
> ```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这一题展示了一条完整的「概率 → 几何」转化链。第一步靠概率论的封闭性：高斯随机变量的线性组合 $c^T x$ 仍服从高斯分布，于是 $n$ 维问题坍缩成一维 CDF 问题。第二步用 $\Phi$ 的单调性把「最大化概率」变成「最大化比值 $(\bar{c}^T x - \alpha)/\sqrt{x^T R x}$」；第三步发现该比值虽不凹、但在非负区域上是拟凹的，因为「目标 $\ge t$」恰好等价于一个二阶锥约束，即其上水平集是凸集。求解用对 $t$ 的二分法：每步只解一个 SOCP 可行性问题，$O(\log(1/\varepsilon))$ 轮就能达到精度 $\varepsilon$。这与 6.006 中二分搜索的前提完全同源：二分要求「可判定且单调」，而拟凸性（quasiconvexity）正是把判定问题变成凸可行性问题的那份结构保证。最后的齐次化替换 $y = x/(\bar{c}^T x - \alpha)$、$s = 1/(\bar{c}^T x - \alpha)$ 更是把拟凸问题精确地凸化，是处理「比值型目标」（如信噪比最大化）时的标准重武器。

### <span style="color:#c0392b;">4.30 热流体管道设计（A heated fluid）——几何规划建模</span>

<span style="color:#2471a3;">**[question]**</span>

**4.30** A heated fluid at temperature $T$ (degrees above ambient temperature) flows in a pipe with fixed length and circular cross section with radius $r$. A layer of insulation, with thickness $w \ll r$, surrounds the pipe to reduce heat loss through the pipe walls. The design variables in this problem are $T$, $r$, and $w$.

The heat loss is (approximately) proportional to $T r / w$, so over a fixed lifetime, the energy cost due to heat loss is given by $\alpha_1 T r / w$. The cost of the pipe, which has a fixed wall thickness, is approximately proportional to the total material, i.e., it is given by $\alpha_2 r$. The cost of the insulation is also approximately proportional to the total insulation material, i.e., $\alpha_3 r w$ (using $w \ll r$). The total cost is the sum of these three costs.

The heat flow down the pipe is entirely due to the flow of the fluid, which has a fixed velocity, i.e., it is given by $\alpha_4 T r^2$. The constants $\alpha_i$ are all positive, as are the variables $T$, $r$, and $w$.

Now the problem: maximize the total heat flow down the pipe, subject to an upper limit $C_{\max}$ on total cost, and the constraints

```math
T_{\min} \le T \le T_{\max}, \qquad r_{\min} \le r \le r_{\max}, \qquad w_{\min} \le w \le w_{\max}, \qquad w \le 0.1 r.
```

Express this problem as a geometric program.

<span style="color:#2471a3;">**[solution]**</span>

> **解答（Solution）.** 该问题为
>
> ```math
> \begin{array}{ll}
> \text{maximize} & \alpha_4 T r^2 \\
> \text{subject to} & \alpha_1 T w^{-1} + \alpha_2 r + \alpha_3 r w \le C_{\max} \\
> & T_{\min} \le T \le T_{\max} \\
> & r_{\min} \le r \le r_{\max} \\
> & w_{\min} \le w \le w_{\max} \\
> & w \le 0.1 r
> \end{array}
> ```

<!-- p.6 -->

> 它等价于如下 GP（geometric program，几何规划）：
>
> ```math
> \begin{array}{ll}
> \text{minimize} & (1/\alpha_4)\, T^{-1} r^{-2} \\
> \text{subject to} & (\alpha_1/C_{\max})\, T w^{-1} + (\alpha_2/C_{\max})\, r + (\alpha_3/C_{\max})\, r w \le 1 \\
> & (1/T_{\max})\, T \le 1, \qquad T_{\min} T^{-1} \le 1 \\
> & (1/r_{\max})\, r \le 1, \qquad r_{\min} r^{-1} \le 1 \\
> & (1/w_{\max})\, w \le 1, \qquad w_{\min} w^{-1} \le 1 \\
> & 10 w r^{-1} \le 1
> \end{array}
> ```
>
> （变量为 $T$、$r$、$w$。）

### <span style="color:#c0392b;">5.1 一个简单的例子（A simple example）——拉格朗日对偶全流程</span>

<span style="color:#2471a3;">**[question]**</span>

**5.1 A simple example.** Consider the optimization problem

```math
\begin{array}{ll}
\text{minimize} & x^2 + 1 \\
\text{subject to} & (x-2)(x-4) \le 0
\end{array}
```

with variable $x \in \mathbb{R}$.

**(a) Analysis of primal problem.** Give the feasible set, the optimal value, and the optimal solution.

**(b) Lagrangian and dual function.** Plot the objective $x^2 + 1$ versus $x$. On the same plot, show the feasible set, optimal point and value, and plot the Lagrangian $L(x, \lambda)$ versus $x$ for a few positive values of $\lambda$. Verify the lower bound property ($p^\star \ge \inf_x L(x, \lambda)$ for $\lambda \ge 0$). Derive and sketch the Lagrange dual function $g$.

**(c) Lagrange dual problem.** State the dual problem, and verify that it is a concave maximization problem. Find the dual optimal value and dual optimal solution $\lambda^\star$. Does strong duality hold?

**(d) Sensitivity analysis.** Let $p^\star(u)$ denote the optimal value of the problem

```math
\begin{array}{ll}
\text{minimize} & x^2 + 1 \\
\text{subject to} & (x-2)(x-4) \le u
\end{array}
```

as a function of the parameter $u$. Plot $p^\star(u)$. Verify that $dp^\star(0)/du = -\lambda^\star$.

<span style="color:#2471a3;">**[solution]**</span>

> **解答（Solution）.**
>
> (a) 可行集是区间 $[2, 4]$。（唯一的）最优解为 $x^\star = 2$，最优值为 $p^\star = 5$。
>
> 图中画出了 $f_0$ 与 $f_1$。（图：横轴 $x$ 范围 $[-1, 5]$，纵轴范围 $[-5, 30]$；$f_0(x) = x^2 + 1$ 是开口向上的抛物线，$f_1(x) = (x-2)(x-4)$ 在 $[2,4]$ 内非正、在其外为正，可行集正是 $f_1 \le 0$ 对应的区间。）

<!-- p.7 -->

> (b) 拉格朗日函数（Lagrangian）为
>
> ```math
> L(x, \lambda) = (1 + \lambda)x^2 - 6\lambda x + (1 + 8\lambda).
> ```
>
> 图中对若干个 $\lambda \ge 0$ 画出了 $L(x, \lambda) = f_0 + \lambda f_1$ 关于 $x$ 的曲线。注意：$L(x, \lambda)$ 关于 $x$ 的最小值（即 $g(\lambda)$）总是小于 $p^\star$；当 $\lambda$ 从 0 增大到 2 时它上升，在 $\lambda = 2$ 处达到最大，$\lambda$ 超过 2 后再次下降。在 $\lambda = 2$ 处有等式 $p^\star = g(\lambda)$ 成立。
>
> 当 $\lambda > -1$ 时，拉格朗日函数在 $\tilde{x} = 3\lambda/(1 + \lambda)$ 处取得最小值；当 $\lambda \le -1$ 时它下无界。因此
>
> ```math
> g(\lambda) = \begin{cases} -9\lambda^2/(1+\lambda) + 1 + 8\lambda, & \lambda > -1, \\ -\infty, & \lambda \le -1, \end{cases}
> ```
>
> 其图像如下。（图：横轴 $\lambda$ 范围 $[-2, 4]$，纵轴 $g(\lambda)$ 范围 $[-10, 6]$ 的曲线。）

<!-- p.8 -->

> 可以验证：对偶函数是凹的，且它在 $\lambda = 2$ 处的值等于 $p^\star = 5$，而在其他 $\lambda$ 值处都小于 $p^\star$。
>
> (c) 拉格朗日对偶问题（Lagrange dual problem）为
>
> ```math
> \begin{array}{ll}
> \text{maximize} & -9\lambda^2/(1+\lambda) + 1 + 8\lambda \\
> \text{subject to} & \lambda \ge 0.
> \end{array}
> ```
>
> 对偶最优点在 $\lambda = 2$ 处，$d^\star = 5$。所以对本例可以直接观察到强对偶性（strong duality）成立（它本来也必须成立——Slater 约束规范（constraint qualification）是满足的）。
>
> (d) 当 $u < -1$ 时，扰动问题不可行，因为 $\inf_x (x^2 - 6x + 8) = -1$。当 $u \ge -1$ 时，可行集是区间
>
> ```math
> \left[\, 3 - \sqrt{1+u}, \; 3 + \sqrt{1+u} \,\right],
> ```
>
> 它由 $x^2 - 6x + 8 = u$ 的两个根给出。当 $-1 \le u \le 8$ 时，最优解为 $x^\star(u) = 3 - \sqrt{1+u}$；当 $u \ge 8$ 时，最优解就是 $f_0$ 的无约束最小点，即 $x^\star(u) = 0$。综上，
>
> ```math
> p^\star(u) = \begin{cases} \infty, & u < -1, \\ 11 + u - 6\sqrt{1+u}, & -1 \le u \le 8, \\ 1, & u \ge 8. \end{cases}
> ```
>
> 图中画出了最优值函数 $p^\star(u)$ 及其上图（epigraph）。

<!-- p.9 -->

> （图：横轴 $u$ 与纵轴范围均为 $[-2, 10]$；曲线 $p^\star(u)$、其上图区域 $\operatorname{epi} p^\star$，以及在 $u = 0$ 处与曲线相切的直线 $p^\star(0) - \lambda^\star u$。）
>
> 最后我们指出：$p^\star(u)$ 是 $u$ 的可微函数，且
>
> ```math
> \frac{dp^\star(0)}{du} = -2 = -\lambda^\star.
> ```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这个一维小例子把 Lagrange 对偶的整套骨架演示了一遍。对偶函数 $g(\lambda) = \inf_x L(x, \lambda)$ 对任何原问题都是凹的（对 $\lambda$ 逐点取下确界保持凹性），所以对偶问题永远是一个凸优化问题——这就是「对偶性不依赖原问题凸性」的第一性原理。弱对偶性 $g(\lambda) \le p^\star$ 的几何意义在图中一目了然：每条加权曲线 $f_0 + \lambda f_1$ 的最低点都落在最优值 $p^\star$ 之下，$\lambda$ 相当于给「违反约束」买保险的价格。本例在 $\lambda^\star = 2$ 处 $g(\lambda^\star) = p^\star = 5$，强对偶性成立，原因正是 Slater 条件（例如 $x = 3$ 严格可行）——凸问题加 Slater 条件保证对偶间隙（duality gap）为零。从 Strang 线代的角度看：$L(x, \lambda)$ 的二次项系数 $1 + \lambda$ 在 $\lambda \ge 0$ 时恒正，最小化它就是解一个正定二次型的驻点方程（求导后得到良态线性方程）；而 $\lambda < -1$ 时抛物线开口向下、$g = -\infty$，正定性崩塌恰好对应对偶函数失去下界能力。

<!-- p.10 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">附加题解答（Solutions to additional exercises）</span>

#### <span style="color:#c0392b;">1. 在概率单纯形上最小化函数（Minimizing a function over the probability simplex）</span>

<span style="color:#2471a3;">**[exercise]**</span>

**1. Minimizing a function over the probability simplex.** Find simple necessary and sufficient conditions for $x \in \mathbb{R}^n$ to minimize a differentiable convex function $f$ over the probability simplex, $\{x \mid \mathbf{1}^T x = 1,\ x \succeq 0\}$.

<span style="color:#2471a3;">**[solution]**</span>

> **解答（Solution）.** <span style="color:#00838f;">最优性的基本条件是：$x$ 可行（即 $x \succeq 0$，$\mathbf{1}^T x = 1$），且对所有可行 $y$ 都有 $\nabla f(x)^T (y - x) \ge 0$。</span>我们首先证明这等价于
>
> ```math
> \min_{i=1,\ldots,n} \nabla f(x)_i \;\ge\; \nabla f(x)^T x.
> ```
>
> 为看清这一点，假设 $\nabla f(x)^T (y - x) \ge 0$ 对所有可行 $y$ 成立。那么特别地，取 $y = e_i$，就有 $\nabla f(x)_i \ge \nabla f(x)^T x$，这正是上式。反过来，假设 $\nabla f(x)_i \ge \nabla f(x)^T x$ 对 $i = 1, \ldots, n$ 都成立。设 $y$ 可行，即 $y \succeq 0$，$\mathbf{1}^T y = 1$。把 $\nabla f(x)_i \ge \nabla f(x)^T x$ 两边乘以 $y_i$ 并求和，得到
>
> ```math
> \sum_{i=1}^{n} y_i \nabla f(x)_i \;\ge\; \left( \sum_{i=1}^{n} y_i \right) \nabla f(x)^T x \;=\; \nabla f(x)^T x.
> ```
>
> 左边正是 $y^T \nabla f(x)$，因此 $\nabla f(x)^T (y - x) \ge 0$。
>
> 现在还可以进一步简化。上面的条件可以写成
>
> ```math
> \min_{i=1,\ldots,n} \frac{\partial f}{\partial x_i} \;\ge\; \sum_{i=1}^{n} x_i \frac{\partial f}{\partial x_i}.
> ```
>
> 但由于 $\mathbf{1}^T x = 1$、$x \succeq 0$，又有
>
> ```math
> \min_{i=1,\ldots,n} \frac{\partial f}{\partial x_i} \;\le\; \sum_{i=1}^{n} x_i \frac{\partial f}{\partial x_i},
> ```
>
> 于是
>
> ```math
> \min_{i=1,\ldots,n} \frac{\partial f}{\partial x_i} \;=\; \sum_{i=1}^{n} x_i \frac{\partial f}{\partial x_i}.
> ```
>
> 右边是 $\partial f / \partial x_i$ 各项的一个凸组合，却等于所有项中的最小值。这只可能在如下情形发生：只要 $\partial f / \partial x_k > \min_i \partial f / \partial x_i$，就有 $x_k = 0$。
>
> 因此（充要的）最优性条件可以写成：$\mathbf{1}^T x = 1$，$x \succeq 0$，且对每个 $k$，
>
> ```math
> x_k > 0 \;\Longrightarrow\; \frac{\partial f}{\partial x_k} = \min_{i=1,\ldots,n} \frac{\partial f}{\partial x_i}.
> ```
>
> 特别地，对所有 $x_k > 0$ 的 $k$，各个 $\partial f / \partial x_k$ 彼此相等。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 概率单纯形 $\{x \mid \mathbf{1}^T x = 1,\ x \succeq 0\}$ 上的每个点恰好是一个离散概率分布，所以本题等价于在问：「最小化凸期望成本的分布长什么样？」答案是最优性条件 $\nabla f(x)^T (y - x) \ge 0$ 在单纯形上的特化：凡是被赋了正质量的坐标，其偏导数必须并列全场最小，$\partial f / \partial x_k = \min_i \partial f / \partial x_i$。直观解释：如果某个已选坐标的边际成本不是最低，把质量从它挪到最低成本坐标就能继续下降——这正是概率论里「期望对分布求导」之后显露出的贪心结构。这个「正分量处梯度相等」的形态与 KKT 条件中的互补松弛（complementary slackness）同源，也是水位填充（water-filling）类算法的逻辑内核：资源只分给边际收益并列第一的格子。证明里「用可行点的分量做凸组合系数」这一步，是处理单纯形约束时值得反复复用的标准手法。

<!-- p.11 -->

#### <span style="color:#c0392b;">2. 复最小范数问题（Complex least-norm problem）</span>

<span style="color:#2471a3;">**[exercise]**</span>

**2. Complex least-norm problem.** We consider the complex least ℓp-norm problem

```math
\begin{array}{ll}
\text{minimize} & \|x\|_p \\
\text{subject to} & Ax = b
\end{array}
```

where $A \in \mathbb{C}^{m\times n}$, $b \in \mathbb{C}^m$, and the variable is $x \in \mathbb{C}^n$. Here $\|\cdot\|_p$ denotes the ℓp-norm on $\mathbb{C}^n$, defined as

```math
\|x\|_p = \left( \sum_{i=1}^{n} |x_i|^p \right)^{1/p} \quad \text{for } p \ge 1, \qquad \|x\|_\infty = \max_{i=1,\ldots,n} |x_i|.
```

We assume $A$ is full rank, and $m < n$.

(a) Formulate the complex least ℓ2-norm problem as a least ℓ2-norm problem with real problem data and variable. Hint. Use $z = (\Re x, \Im x) \in \mathbb{R}^{2n}$ as the variable.

(b) Formulate the complex least ℓ∞-norm problem as an SOCP.

(c) Solve a random instance of both problems with $m = 30$ and $n = 100$. To generate the matrix $A$, you can use the Matlab command `A = randn(m,n) + i*randn(m,n)`. Similarly, use `b = randn(m,1) + i*randn(m,1)` to generate the vector $b$. Use the Matlab command `scatter` to plot the optimal solutions of the two problems on the complex plane, and comment (briefly) on what you observe. You can solve the problems using the cvx functions `norm(x,2)` and `norm(x,inf)`, which are overloaded to handle complex arguments. To utilize this feature, you will need to declare variables to be complex in the variable statement. (In particular, you do not have to manually form or solve the SOCP from part (b).)

<span style="color:#2471a3;">**[solution]**</span>

> **解答（Solution）.**
>
> (a) 定义 $z = (\Re x, \Im x) \in \mathbb{R}^{2n}$，则 $\|x\|_2^2 = \|z\|_2^2$。复线性方程 $Ax = b$ 等价于 $\Re(Ax) = \Re b$ 与 $\Im(Ax) = \Im b$，而后者又可以表达为线性方程组
>
> ```math
> \begin{bmatrix} \Re A & -\Im A \\ \Im A & \Re A \end{bmatrix} z = \begin{bmatrix} \Re b \\ \Im b \end{bmatrix}.
> ```
>
> 于是，复最小 ℓ2 范数问题可以表达为
>
> ```math
> \begin{array}{ll}
> \text{minimize} & \|z\|_2 \\
> \text{subject to} & \begin{bmatrix} \Re A & -\Im A \\ \Im A & \Re A \end{bmatrix} z = \begin{bmatrix} \Re b \\ \Im b \end{bmatrix}.
> \end{array}
> ```
>
> （这个形式可以直接解析求解。）
>
> (b) 采用上图（epigraph）表述，引入新变量 $t$，把问题写成
>
> ```math
> \begin{array}{ll}
> \text{minimize} & t \\
> \text{subject to} & \left\| \begin{bmatrix} z_i \\ z_{n+i} \end{bmatrix} \right\|_2 \le t, \quad i = 1, \ldots, n \\
> & \begin{bmatrix} \Re A & -\Im A \\ \Im A & \Re A \end{bmatrix} z = \begin{bmatrix} \Re b \\ \Im b \end{bmatrix}.
> \end{array}
> ```
>
> 这是一个含 $n$ 个（$\mathbb{R}^3$ 中的）二阶锥约束（second-order cone constraint）的 SOCP（second-order cone program，二阶锥规划）。

<!-- p.12 -->

> (c) 求解代码如下（Matlab）：
>
> ```matlab
> % complex minimum norm problem
> %
> randn(’state’,0);
> m = 30; n = 100;
> % generate matrix A
> Are = randn(m,n); Aim = randn(m,n);
> bre = randn(m,1); bim = randn(m,1);
> A = Are + i*Aim;
> b = bre + i*bim;
> % 2-norm problem (analytical solution)
> Atot = [Are -Aim; Aim Are];
> btot = [bre; bim];
> z_2 = Atot’*inv(Atot*Atot’)*btot;
> x_2 = z_2(1:100) + i*z_2(101:200);
> % 2-norm problem solution with cvx
> cvx_begin
> variable x(n) complex
> minimize( norm(x) )
> subject to
> A*x == b;
> cvx_end
> % inf-norm problem solution with cvx
> cvx_begin
> variable xinf(n) complex
> minimize( norm(xinf,Inf) )
> subject to
> A*xinf == b;
> cvx_end
> % scatter plot
> figure(1)
> scatter(real(x),imag(x)), hold on,
> scatter(real(xinf),imag(xinf),[],’filled’), hold off,
> axis([-0.2 0.2 -0.2 0.2]), axis square,
> xlabel(’Re x’); ylabel(’Im x’);
> ```
>
> 下图给出了 $p = 2$（空心圆圈）与 $p = \infty$（实心圆圈）两组最优解分量的散点图。$p = \infty$ 的最优解在约束 $Ax = b$ 下最小化目标 $\max_{i=1,\ldots,n} |x_i|$，而 $x_i$ 的散点图显示它们几乎全部集中在复平面上一个圆的附近。这应当在意料之中：既然最小化的是各分量的最大幅值，那么几乎所有 $x_i$ 的幅值 $|x_i|$ 就应当大致相等。

<!-- p.13 -->

> （图：横轴 $\Re x$、纵轴 $\Im x$，范围均为 $[-0.2, 0.2]$；空心点为 $\ell_2$ 解，实心点为 $\ell_\infty$ 解，后者近似落在同一个圆上。）

#### <span style="color:#c0392b;">3. 数值扰动分析例子（Numerical perturbation analysis example）</span>

<span style="color:#2471a3;">**[exercise]**</span>

**3. Numerical perturbation analysis example.** Consider the quadratic program

```math
\begin{array}{ll}
\text{minimize} & x_1^2 + 2x_2^2 - x_1 x_2 - x_1 \\
\text{subject to} & x_1 + 2x_2 \le u_1 \\
& x_1 - 4x_2 \le u_2, \\
& 5x_1 + 76x_2 \le 1,
\end{array}
```

with variables $x_1$, $x_2$, and parameters $u_1$, $u_2$.

(a) Solve this QP, for parameter values $u_1 = -2$, $u_2 = -3$, to find optimal primal variable values $x_1^\star$ and $x_2^\star$, and optimal dual variable values $\lambda_1^\star$, $\lambda_2^\star$ and $\lambda_3^\star$. Let $p^\star$ denote the optimal objective value. Verify that the KKT conditions hold for the optimal primal and dual variables you found (within reasonable numerical accuracy). Hint: See §3.6 of the CVX users' guide to find out how to retrieve optimal dual variables. To specify the quadratic objective, use `quad_form()`.

(b) We will now solve some perturbed versions of the QP, with

```math
u_1 = -2 + \delta_1, \qquad u_2 = -3 + \delta_2,
```

where $\delta_1$ and $\delta_2$ each take values from $\{-0.1, 0, 0.1\}$. (There are a total of nine such combinations, including the original problem with $\delta_1 = \delta_2 = 0$.) For each combination of $\delta_1$ and $\delta_2$, make a prediction $p^\star_{\mathrm{pred}}$ of the optimal value of the perturbed QP, and compare it to $p^\star_{\mathrm{exact}}$, the exact optimal value of the perturbed QP (obtained by solving the perturbed QP). Put your results in the two righthand columns in a table with the form shown below. Check that the inequality $p^\star_{\mathrm{pred}} \le p^\star_{\mathrm{exact}}$ holds.

| $\delta_1$ | $\delta_2$ | $p^\star_{\mathrm{pred}}$ | $p^\star_{\mathrm{exact}}$ |
|:---:|:---:|:---:|:---:|
| 0 | 0 | | |
| 0 | $-0.1$ | | |
| 0 | $0.1$ | | |
| $-0.1$ | 0 | | |
| $-0.1$ | $-0.1$ | | |
| $-0.1$ | $0.1$ | | |
| $0.1$ | 0 | | |
| $0.1$ | $-0.1$ | | |
| $0.1$ | $0.1$ | | |

<!-- p.14 -->

<span style="color:#2471a3;">**[solution]**</span>

> **解答（Solution）.**
>
> (a) 下面的 Matlab 代码建立这个简单的 QP 并用 CVX 求解：
>
> ```matlab
> Q = [1 -1/2; -1/2 2];
> f = [-1 0]’;
> A = [1 2; 1 -4; 5 76];
> b = [-2 -3 1]’;
> cvx_begin
> variable x(2)
> dual variable lambda
> minimize(quad_form(x,Q)+f’*x)
> subject to
> lambda: A*x <= b
> cvx_end
> p_star = cvx_optval
> ```
>
> 运行后得到最优目标值 $p^\star = 8.22$，最优点为 $x_1^\star = -2.33$、$x_2^\star = 0.17$。（由于目标函数严格凸，这个最优点是唯一的。）一组最优对偶变量为 $\lambda_1^\star = 1.46$、$\lambda_2^\star = 3.77$、$\lambda_3^\star = 0.12$。（对偶最优点也是唯一的，不过证明起来更麻烦，而且反正也无关紧要。）

<!-- p.15 -->

> KKT 条件为
>
> ```math
> \begin{aligned}
> & x_1^\star + 2x_2^\star \le u_1, \qquad x_1^\star - 4x_2^\star \le u_2, \qquad 5x_1^\star + 76x_2^\star \le 1 \\
> & \lambda_1^\star \ge 0, \qquad \lambda_2^\star \ge 0, \qquad \lambda_3^\star \ge 0 \\
> & \lambda_1^\star (x_1^\star + 2x_2^\star - u_1) = 0, \quad \lambda_2^\star (x_1^\star - 4x_2^\star - u_2) = 0, \quad \lambda_3^\star (5x_1^\star + 76x_2^\star - 1) = 0, \\
> & 2x_1^\star - x_2^\star - 1 + \lambda_1^\star + \lambda_2^\star + 5\lambda_3^\star = 0, \\
> & 4x_2^\star - x_1^\star + 2\lambda_1^\star - 4\lambda_2^\star + 76\lambda_3^\star = 0.
> \end{aligned}
> ```
>
> 我们对这些条件做数值检验。对偶变量 $\lambda_1^\star$、$\lambda_2^\star$、$\lambda_3^\star$ 都大于零，而如下两个量
>
> ```matlab
> A*x-b
> 2*Q*x+f+A’*lambda
> ```
>
> 被发现非常小。因此 KKT 条件得到验证。
>
> (b) 预测的最优值为
>
> ```math
> p^\star_{\mathrm{pred}} = p^\star - \lambda_1^\star \delta_1 - \lambda_2^\star \delta_2.
> ```
>
> 下面的 Matlab 代码填出表格：
>
> ```matlab
> arr_i = [0 -1 1];
> delta = 0.1;
> pa_table = [];
> for i = arr_i
> for j = arr_i
> p_pred = p_star - [lambda(1) lambda(2)]*[i; j]*delta;
> cvx_begin
> variable x(2)
> minimize(quad_form(x,Q)+f’*x)
> subject to
> A*x <= b+[i;j;0]*delta
> cvx_end
> p_exact = cvx_optval;
> pa_table = [pa_table; i*delta j*delta p_pred p_exact]
> end
> end
> ```
>
> 得到的数值为

<!-- p.16 -->

> | $\delta_1$ | $\delta_2$ | $p^\star_{\mathrm{pred}}$ | $p^\star_{\mathrm{exact}}$ |
> |:---:|:---:|:---:|:---:|
> | 0 | 0 | 8.22 | 8.22 |
> | 0 | $-0.1$ | 8.60 | 8.70 |
> | 0 | $0.1$ | 7.85 | 7.98 |
> | $-0.1$ | 0 | 8.34 | 8.57 |
> | $-0.1$ | $-0.1$ | 8.75 | 8.82 |
> | $-0.1$ | $0.1$ | 7.99 | 8.32 |
> | $0.1$ | 0 | 8.08 | 8.22 |
> | $0.1$ | $-0.1$ | 8.45 | 8.71 |
> | $0.1$ | $0.1$ | 7.70 | 7.75 |
>
> 不等式 $p^\star_{\mathrm{pred}} \le p^\star_{\mathrm{exact}}$ 在所有情形下都被验证成立。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 本题把 KKT 条件从「背定义」推进到「数值验证 + 预测」两个实战环节。四组条件——原始可行、对偶可行、互补松弛、驻点——在代码里对应两个残差向量 `A*x-b` 与 `2*Q*x+f+A'*lambda` 足够接近零：工程上验证 KKT 就是算残差范数，这与 CSAPP 中「用数值实验确认理论界」的思路一致。预测公式 $p^\star_{\mathrm{pred}} = p^\star - \lambda_1^\star \delta_1 - \lambda_2^\star \delta_2$ 是灵敏度分析（perturbation analysis）的一阶展开：对偶乘子就是「影子价格」，告诉你第 $i$ 个约束的右端放松一单位、最优值大约改善多少。表中 9 组数据全部满足 $p^\star_{\mathrm{pred}} \le p^\star_{\mathrm{exact}}$——这是凸性给出的全局下界保证；而误差最大的一组（$\delta_1 = -0.1, \delta_2 = +0.1$，相差 0.33）说明曲率会让线性预测偏保守。这与概率论中用一阶泰勒展开近似非线性函数（如 Delta 近似 $\mathbb{E}[g(X)] \approx g(\mathbb{E}X)$）是同一个思想：局部线性化加上已知的方向导数。

#### <span style="color:#c0392b;">4. FIR 滤波器设计（FIR filter design）</span>

<span style="color:#2471a3;">**[exercise]**</span>

**4. FIR filter design.** Consider the (symmetric, linear phase) FIR filter described by

```math
H(\omega) = a_0 + \sum_{k=1}^{N} a_k \cos k\omega.
```

The design variables are the real coefficients $a = (a_0, \ldots, a_N) \in \mathbb{R}^{N+1}$. In this problem we will explore the design of a low-pass filter, with specifications:

• For $0 \le \omega \le \pi/3$, $0.89 \le H(\omega) \le 1.12$, i.e., the filter has about ±1dB ripple in the ’passband’ $[0, \pi/3]$.
• For $\omega_c \le \omega \le \pi$, $|H(\omega)| \le \alpha$. In other words, the filter achieves an attenuation given by $\alpha$ in the ’stopband’ $[\omega_c, \pi]$. $\omega_c$ is called the ’cutoff frequency’.

These specifications are depicted graphically in the figure below.

（图：横轴 $\omega$ 标出 $0$、$\pi/3$、$\omega_c$、$\pi$；纵轴标出 $-\alpha$、$0$、$\alpha$ 与 $0.89$、$1.00$、$1.12$；通带 $[0, \pi/3]$ 内 $H(\omega)$ 介于 $0.89$ 与 $1.12$ 之间，阻带 $[\omega_c, \pi]$ 内 $|H(\omega)|$ 不超过 $\alpha$。）

<!-- p.17 -->

(a) Suppose we fix $\omega_c$ and $N$, and wish to maximize the stop-band attenuation, i.e., minimize $\alpha$ such that the specifications above can be met. Explain how to pose this as a convex optimization problem.

(b) Suppose we fix $N$ and $\alpha$, and want to minimize $\omega_c$, i.e., we set the stopband attenuation and filter length, and wish to minimize the ’transition’ band (between $\pi/3$ and $\omega_c$). Explain how to pose this problem as a quasiconvex optimization problem.

(c) Now suppose we fix $\omega_c$ and $\alpha$, and wish to find the smallest $N$ that can meet the specifications, i.e., we seek the shortest length FIR filter that can meet the specifications. Can this problem be posed as a convex or quasiconvex problem? If so, explain how. If you think it cannot be, briefly and informally explain why.

(d) Plot the optimal tradeoff curve of attenuation ($\alpha$) versus cutoff frequency ($\omega_c$) for $N = 7$. Is the set of achievable specifications convex? Briefly explain any interesting features, e.g., flat portions, of the optimal tradeoff curve.

For this subproblem, you may sample the constraints in frequency, which means the following. Choose $K \gg N$ (perhaps $K \approx 10N$), and set $\omega_k = k\pi/K$, $k = 0, \ldots, K$. Then replace the specifications with

• For $k$ with $0 \le \omega_k \le \pi/3$, $0.89 \le H(\omega_k) \le 1.12$.
• For $k$ with $\omega_c \le \omega_k \le \pi$, $|H(\omega_k)| \le \alpha$.

With this approximation, the problem in part (a) becomes an LP, which allows you to solve part (d) numerically.

<span style="color:#2471a3;">**[solution]**</span>

> **解答（Solution）.**
>
> (a) 第一个问题可以表达为
>
> ```math
> \begin{array}{ll}
> \text{minimize} & \alpha \\
> \text{subject to} & f_1(a) \le 1.12 \\
> & f_2(a) \ge 0.89 \\
> & f_3(a) \le \alpha \\
> & f_4(a) \ge -\alpha
> \end{array}
> \tag{2}
> ```
>
> 其中
>
> ```math
> f_1(a) = \sup_{0 \le \omega \le \pi/3} H(\omega), \quad f_2(a) = \inf_{0 \le \omega \le \pi/3} H(\omega), \quad f_3(a) = \sup_{\omega_c \le \omega \le \pi} H(\omega), \quad f_4(a) = \inf_{\omega_c \le \omega \le \pi} H(\omega).
> ```
>
> 问题 (2) 在变量 $a$、$\alpha$ 下是凸的，因为 $f_1$ 与 $f_3$ 是凸函数（仿射函数的逐点上确界），而 $f_2$ 与 $f_4$ 是凹函数（仿射函数的逐点下确界）。

<!-- p.18 -->

> (b) 这个问题可以表达为
>
> ```math
> \begin{array}{ll}
> \text{minimize} & f_5(a) \\
> \text{subject to} & f_1(a) \le 1.12 \\
> & f_2(a) \ge 0.89
> \end{array}
> ```
>
> 其中 $f_1$、$f_2$ 与上面相同，而
>
> ```math
> f_5(a) = \inf \{ \Omega \mid -\alpha \le H(\omega) \le \alpha \text{ 对 } \Omega \le \omega \le \pi \}.
> ```
>
> 这是变量 $a$ 下的拟凸优化问题，因为 $f_1$ 是凸的、$f_2$ 是凹的，而 $f_5$ 是拟凸的：其下水平集（sublevel set）为
>
> ```math
> \{ a \mid f_5(a) \le \Omega \} = \{ a \mid -\alpha \le H(\omega) \le \alpha \text{ 对 } \Omega \le \omega \le \pi \},
> ```
>
> 即无穷多个半空间（halfspace）的交。
>
> (c) 这个问题可以表达为
>
> ```math
> \begin{array}{ll}
> \text{minimize} & f_6(a) \\
> \text{subject to} & f_1(a) \le 1.12 \\
> & f_2(a) \ge 0.89 \\
> & f_3(a) \le \alpha \\
> & f_4(a) \ge -\alpha
> \end{array}
> ```
>
> 其中 $f_1, f_2, f_3, f_4$ 定义如上，而
>
> ```math
> f_6(a) = \min \{ k \mid a_{k+1} = \cdots = a_N = 0 \}.
> ```
>
> $f_6$ 的下水平集是仿射集（affine set）：
>
> ```math
> \{ a \mid f_6(a) \le k \} = \{ a \mid a_{k+1} = \cdots = a_N = 0 \}.
> ```
>
> 这意味着 $f_6$ 是拟凸函数，于是我们再次得到一个拟凸优化问题。
>
> (d) 离散化之后，可以把 (a) 中的问题表达为 LP
>
> ```math
> \begin{array}{ll}
> \text{minimize} & \alpha \\
> \text{subject to} & 0.89 \le H(\omega_i) \le 1.12 \quad \text{对 } 0 \le \omega_i \le \pi/3 \\
> & -\alpha \le H(\omega_i) \le \alpha \quad \text{对 } \omega_c \le \omega_i \le \pi
> \end{array}
> \tag{3}
> ```
>
> 变量为 $\alpha$ 与 $a$。（对固定的 $\omega_i$，$H(\omega_i)$ 是 $a$ 的仿射函数，因此本问题中的所有约束都是关于 $a$ 与 $\alpha$ 的线性不等式。）对 $(\pi/3, \pi]$ 中的一系列 $\omega_c$ 值逐个求解该 LP，就得到 $\alpha$ 对 $\omega_c$ 的权衡曲线（tradeoff curve）。
>
> 图 1 由下面的 Matlab 代码生成：

<!-- p.19 -->

> ```matlab
> clear all
> N = 7;
> K = 10*N;
> k = [0:N]’;
> w = [0:K]’/K*pi;
> idx= max(find(w<=pi/3));
> alphas = [];
> for i=idx:length(w)
> cvx_begin
> variables a(N+1,1)
> minimize( norm(cos(w(i:end)*k’)*a,inf) )
> subject to
> cos(w(1:idx)*k’)*a >= 0.89
> cos(w(1:idx)*k’)*a <= 1.12
> cvx_end
> alphas = [alphas; cvx_optval];
> end;
> plot(w(idx:end),alphas,’-’);
> xlabel(’wc’);
> ylabel(’alpha’);
> ```

> <span style="color:#7f8c8d;">Figure 1：问题 4d 的权衡曲线（Tradeoff curve for problem 4d）</span>
>
> （图：横轴 $\omega_c$ 从 $\pi/3$ 到 $\pi$，纵轴 $\alpha$ 从 0 到 1。）

#### <span style="color:#c0392b;">5. 最小燃料最优控制（Minimum fuel optimal control）</span>

<span style="color:#2471a3;">**[exercise]**</span>

**5. Minimum fuel optimal control.** Solve the minimum fuel optimal control problem described in exercise 4.16 of Convex Optimization, for the instance with problem data

```math
A = \begin{bmatrix} -1 & 0.4 & 0.8 \\ 1 & 0 & 0 \\ 0 & 1 & 0 \end{bmatrix}, \quad b = \begin{bmatrix} 1 \\ 0 \\ 0.3 \end{bmatrix}, \quad x_{\mathrm{des}} = \begin{bmatrix} 7 \\ 2 \\ -6 \end{bmatrix}, \quad N = 30.
```

You can do this by forming the LP you found in your solution of exercise 4.16, or more directly using cvx. Plot the actuator signal $u(t)$ as a function of time $t$.

<span style="color:#2471a3;">**[solution]**</span>

> **解答（Solution）.** 下面的 Matlab 代码求出该解：

<!-- p.20 -->

> ```matlab
> close all
> clear all
> n=3;
> % state dimension
> N=30; % time horizon
> A=[ -1 0.4 0.8; 1 0 0 ; 0 1 0];
> b=[ 1 0 0.3]’;
> x0 = zeros(n,1);
> xdes = [ 7 2 -6]’;
> cvx_begin
> variable X(n,N+1);
> variable u(1,N);
> minimize (sum(max(abs(u),2*abs(u)-1)))
> subject to
> X(:,2:N+1) == A*X(:,1:N)+b*u;
> % dynamics
> X(:,1)== x0;
> X(:,N+1)== xdes;
> cvx_end
> stairs(0:N-1,u,’linewidth’,2)
> axis tight
> xlabel(’t’)
> ylabel(’u’)
> ```
>
> 最优执行器信号如图 2 所示。

<!-- p.21 -->

> <span style="color:#7f8c8d;">Figure 2：最小燃料最优控制的执行器信号（Minimum fuel actuator signal）</span>
>
> （图：横轴 $t$ 标出 0、5、10、15、20、25；纵轴 $u(t)$ 标出 $-0.5$、0、0.5、1；信号按 `stairs` 指令呈阶梯状。）


