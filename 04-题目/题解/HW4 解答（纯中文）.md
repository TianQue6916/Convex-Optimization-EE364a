# EE364a Homework 4 solutions（EE364a 作业 4 解答）

> 原文：`HW4 解答.pdf`（21 页），EE364a, Winter 2007-08, Prof. S. Boyd。前 9 页是教材习题解答（4.11, 4.16, 4.29, 4.30, 5.1），第 10 页起是 "Solutions to additional exercises"（附加题：probability simplex 上的最优性条件、复 least-norm 问题、数值扰动分析、FIR filter design、最小燃料最优控制）。

## 4.11 Problems involving $\ell_1$- and $\ell_\infty$-norms（涉及 $\ell_1$ 与 $\ell_\infty$ 范数的问题）

把下列问题表述为 LP。详细说明每个问题的最优解与其等价 LP 的解之间的关系。

**(a)** Minimize $\|Ax - b\|_\infty$（$\ell_\infty$-norm approximation（$\ell_\infty$ 范数逼近））。

**(b)** Minimize $\|Ax - b\|_1$（$\ell_1$-norm approximation）。

**(c)** Minimize $\|Ax - b\|_1$ subject to $\|x\|_\infty \le 1$。

**(d)** Minimize $\|x\|_1$ subject to $\|Ax - b\|_\infty \le 1$。

**(e)** Minimize $\|Ax - b\|_1 + \|x\|_\infty$。

每个问题中 $A \in \mathbf{R}^{m \times n}$ 与 $b \in \mathbf{R}^m$ 给定。（涉及 approximation 与 constrained approximation（约束逼近）的更多问题见 §6.1。）

**Solution**。

**(a)** 等价于 LP

```math
\begin{aligned}
\text{minimize} \quad & t \\
\text{subject to} \quad & Ax - b \preceq t\mathbf{1} \\
& Ax - b \succeq -t\mathbf{1},
\end{aligned}
```

变量为 $x \in \mathbf{R}^n$、$t \in \mathbf{R}$。为看清等价性，假设在该问题中 $x$ 固定、只对 $t$ 优化。约束说的是

```math
-t \le a_k^T x - b_k \le t
```

对每个 $k$ 成立，即 $t \ge |a_k^T x - b_k|$，即

```math
t \ge \max_k |a_k^T x - b_k| = \|Ax - b\|_\infty .
```

显然，若 $x$ 固定，该 LP 的 optimal value 为 $p^\star(x) = \|Ax - b\|_\infty$。因此同时对 $t$ 与 $x$ 优化，等价于原问题。

**(b)** 等价于 LP

```math
\begin{aligned}
\text{minimize} \quad & \mathbf{1}^T s \\
\text{subject to} \quad & Ax - b \preceq s \\
& Ax - b \succeq -s,
\end{aligned}
```

变量为 $x \in \mathbf{R}^n$ 与 $s \in \mathbf{R}^m$。假设在该问题中 $x$ 固定、只对 $s$ 优化。约束说的是

```math
-s_k \le a_k^T x - b_k \le s_k
```

对每个 $k$ 成立，即 $s_k \ge |a_k^T x - b_k|$。该 LP 的 objective function 是 separable 的，所以对 $s$ 的最优解取

```math
s_k = |a_k^T x - b_k|,
```

得到 optimal value $p^\star(x) = \|Ax - b\|_1$。因此同时对 $x$ 与 $s$ 优化，等价于原问题。

**(c)** 等价于 LP

```math
\begin{aligned}
\text{minimize} \quad & \mathbf{1}^T y \\
\text{subject to} \quad & -y \preceq Ax - b \preceq y \\
& -\mathbf{1} \preceq x \preceq \mathbf{1},
\end{aligned}
```

变量为 $x \in \mathbf{R}^n$ 与 $y \in \mathbf{R}^m$。

**(d)** 等价于 LP

```math
\begin{aligned}
\text{minimize} \quad & \mathbf{1}^T y \\
\text{subject to} \quad & -y \preceq x \preceq y \\
& -\mathbf{1} \preceq Ax - b \preceq \mathbf{1},
\end{aligned}
```

变量为 $x \in \mathbf{R}^n$ 与 $y \in \mathbf{R}^n$。

另一种改写是把 $x$ 写成两个非负向量之差

```math
x = x^+ - x^-,
```

并把问题表述为

```math
\begin{aligned}
\text{minimize} \quad & \mathbf{1}^T x^+ + \mathbf{1}^T x^- \\
\text{subject to} \quad & -\mathbf{1} \preceq A x^+ - A x^- - b \preceq \mathbf{1} \\
& x^+ \succeq 0, \quad x^- \succeq 0,
\end{aligned}
```

变量为 $x^+ \in \mathbf{R}^n$ 与 $x^- \in \mathbf{R}^n$。

**(e)** 等价于

```math
\begin{aligned}
\text{minimize} \quad & \mathbf{1}^T y + t \\
\text{subject to} \quad & -y \preceq Ax - b \preceq y \\
& -t\mathbf{1} \preceq x \preceq t\mathbf{1},
\end{aligned}
```

变量为 $x \in \mathbf{R}^n$、$y \in \mathbf{R}^m$ 与 $t \in \mathbf{R}$。

## 4.16 Minimum fuel optimal control（最小燃料最优控制）

考虑一个 linear dynamical system（线性动力系统），状态 $x(t) \in \mathbf{R}^n$，$t = 0,\dots,N$，actuator 或 input signal（输入信号）$u(t) \in \mathbf{R}$，$t = 0,\dots,N-1$。系统 dynamics（动力学）由 linear recurrence（线性递推）给出

```math
x(t+1) = Ax(t) + bu(t), \qquad t = 0,\dots,N-1,
```

其中 $A \in \mathbf{R}^{n \times n}$ 与 $b \in \mathbf{R}^n$ 给定。假设初始状态为零，即 $x(0) = 0$。

minimum fuel optimal control problem（最小燃料最优控制问题）是选择输入 $u(0),\dots,u(N-1)$，使总燃料消耗最小，总燃料为

```math
F = \sum_{t=0}^{N-1} f(u(t)),
```

并满足约束 $x(N) = x^{\mathrm{des}}$，其中 $N$ 是（给定的）time horizon（时间跨度），$x^{\mathrm{des}} \in \mathbf{R}^n$ 是（给定的）desired final or target state（期望终态或目标状态）。函数 $f : \mathbf{R} \to \mathbf{R}$ 是 actuator 的 fuel use map（燃料消耗映射），给出燃料用量作为 actuator signal amplitude（执行器信号幅值）的函数。本题中取

```math
f(a) = \begin{cases} |a| & |a| \le 1 \\ 2|a| - 1 & |a| > 1 . \end{cases}
```

意思是：对 $-1$ 到 $1$ 之间的 actuator signal，燃料用量与 actuator signal 的绝对值成正比；对更大的 actuator signal，marginal fuel efficiency（边际燃料效率）为一半。

把 minimum fuel optimal control problem 表述为 LP。

**Solution**。该问题等价于 LP

```math
\begin{aligned}
\text{minimize} \quad & \mathbf{1}^T t \\
\text{subject to} \quad & Hu = x^{\mathrm{des}} \\
& -y \preceq u \preceq y \\
& t \succeq y \\
& t \succeq 2y - \mathbf{1},
\end{aligned}
```

变量为 $u \in \mathbf{R}^N$、$y \in \mathbf{R}^N$ 与 $t \in \mathbf{R}^N$，其中

```math
H = \big[\, A^{N-1}b \quad A^{N-2}b \quad \cdots \quad Ab \quad b \,\big].
```

（译者补推导：把 $f$ 看成 $f(a) = \max\{|a|,\, 2|a|-1\}$，用 $y \succeq |u|$ 与 $t \succeq \max\{y, 2y-1\}$ 的 epigraph 形式即得。$H$ 的列序反映「$u(t)$ 经过 $N-1-t$ 步传播到 $x(N)$」：由递推展开得 $x(N) = \sum_{t=0}^{N-1} A^{N-1-t} b\, u(t)$，故 $H$ 的第 $(t+1)$ 列是 $A^{N-1-t}b$。）

还有其他几种可能的 LP 表述。例如，我们可以把状态轨迹 $x(0),\dots,x(N)$ 保留为优化变量，把上面的等式约束 $Hu = x^{\mathrm{des}}$ 换成一组等式约束

```math
x(t+1) = Ax(t) + bu(t), \quad t = 0,\dots,N-1, \quad x(0) = 0, \quad x(N) = x^{\mathrm{des}} .
```

在这种表述下，变量为 $u \in \mathbf{R}^N$、$x(0),\dots,x(N) \in \mathbf{R}^n$，以及 $y \in \mathbf{R}^N$ 与 $t \in \mathbf{R}^N$。

还有一种变体是不引入上面那个中间变量 $y$，只用变量 $t$（与 $u$）表述问题：

```math
-t \preceq u \preceq t, \qquad 2u - \mathbf{1} \preceq t, \qquad -2u - \mathbf{1} \preceq t,
```

变量为 $u \in \mathbf{R}^N$ 与 $t \in \mathbf{R}^N$。

（第三种写法的推导：对每个分量，$f(u_i) \le t_i$ 当且仅当 $|u_i| \le t_i$ 且 $2|u_i| - 1 \le t_i$；后者等价于 $2u_i - 1 \le t_i$ 与 $-2u_i - 1 \le t_i$。）

## 4.29 Maximizing probability of satisfying a linear inequality（最大化满足某个线性不等式的概率）

设 $c$ 是 $\mathbf{R}^n$ 中的 random variable（随机变量），服从 normal distribution（正态分布），均值为 $\bar{c}$、covariance matrix（协方差矩阵）为 $R$。考虑问题

```math
\begin{aligned}
\text{maximize} \quad & \mathrm{prob}(c^T x \ge \alpha) \\
\text{subject to} \quad & Fx \preceq g, \quad Ax = b .
\end{aligned}
```

找出该问题等价于 convex 或 quasiconvex optimization problem 的条件。当这些条件成立时，把问题表述为一个 QP、QCQP 或 SOCP（若问题 convex）；若问题 quasiconvex，则解释如何通过求解一系列 QP/QCQP/SOCP feasibility problem（可行性问题）来解它。

**Solution**。定义 $u = c^T x$，这是标量 random variable，服从 normal distribution，其均值为

```math
\mathbf{E}\,u = \bar{c}^T x,
```

方差为

```math
\mathbf{E}(u - \mathbf{E}\,u)^2 = x^T R x .
```

random variable

```math
\frac{u - \bar{c}^T x}{\sqrt{x^T R x}}
```

服从均值为零、方差为一的 normal distribution，所以

```math
\mathrm{prob}(u \ge \alpha) = \mathrm{prob}\!\left( \frac{u - \bar{c}^T x}{\sqrt{x^T R x}} \ge \frac{\alpha - \bar{c}^T x}{\sqrt{x^T R x}} \right) = 1 - \Phi\!\left( \frac{\alpha - \bar{c}^T x}{\sqrt{x^T R x}} \right),
```

其中

```math
\Phi(z) = \frac{1}{\sqrt{2\pi}} \int_{-\infty}^{z} e^{-t^2/2}\, dt
```

是 standard normal CDF（标准正态累积分布函数）。

要最大化 $\mathrm{prob}(u \ge \alpha)$，我们可以最小化 $(\alpha - \bar{c}^T x)/\sqrt{x^T R x}$（因为 $\Phi$ 递增），即求解问题

```math
\begin{aligned}
\text{maximize} \quad & (\bar{c}^T x - \alpha)/\sqrt{x^T R x} \\
\text{subject to} \quad & Fx \preceq g \\
& Ax = b .
\end{aligned}
```

（式 (1)）这**不是** convex optimization problem，因为 objective 不是 concave 的。

然而，只要某个条件成立，该问题就可用 quasiconvex optimization 求解。（该条件下面给出。）objective 超过某个值 $t$ 当且仅当

```math
\bar{c}^T x - \alpha \ge t \sqrt{x^T R x}
```

成立。只要 $t \ge 0$，最后这个不等式就是 convex 的——事实上它是 second-order cone constraint（二阶锥约束）。于是我们可以写出条件：**存在满足 $\bar{c}^T x \ge \alpha$ 的 feasible $x$**。（这个条件很容易作为 LP feasibility problem 检验。）顺便说，这个条件也可以表述为：存在 feasible $x$ 使 $\mathrm{prob}(u \ge \alpha) \ge 1/2$。

假设该条件成立。这意味着原问题的 optimal value 至少为 $0.5$，并且问题 (1) 的 optimal value 至少为 $0$。于是可以把问题写成

```math
\begin{aligned}
\text{maximize} \quad & t \\
\text{subject to} \quad & Fx \preceq g, \quad Ax = b \\
& \bar{c}^T x - \alpha \ge t\sqrt{x^T R x},
\end{aligned}
```

这里可以假定 $t \ge 0$。这可以对 $t$ 用 bisection（二分）来解：每步解一个 SOCP feasibility problem。换句话说：函数 $(\bar{c}^T x - \alpha)/\sqrt{x^T R x}$ 是 quasiconcave 的，前提是它非负。

事实上，只要上面那个条件成立（即存在 feasible $x$ 满足 $\bar{c}^T x \ge \alpha$），我们就可以用 convex optimization 求解问题 (1)。作 change of variables（变量替换）

```math
y = \frac{x}{\bar{c}^T x - \alpha}, \qquad s = \frac{1}{\bar{c}^T x - \alpha},
```

于是 $x = y/s$。这给出问题

```math
\begin{aligned}
\text{minimize} \quad & y^T R y \\
\text{subject to} \quad & Fy \preceq gs \\
& Ay = bs \\
& \bar{c}^T y - \alpha s = 1 \\
& s \ge 0 .
\end{aligned}
```

（推导：把 $x = y/s$ 代入 (1) 的 objective，得 $(\bar{c}^T y/s - \alpha)/\sqrt{y^T R y/s^2} = (\bar{c}^T y - \alpha s)/\sqrt{y^T R y}$；再用等式约束 $\bar{c}^T y - \alpha s = 1$，最大化它等价于最小化 $y^T R y$。约束两边乘以 $s > 0$，不等号方向不变。）

## 4.30

设温度为 $T$（比环境温度高出的度数）的热流体在管中流动；管长固定，圆形截面半径为 $r$。管外包一层厚度为 $w \ll r$ 的 insulation（保温层），以减少通过管壁的热损失。本题的设计变量是 $T$、$r$ 与 $w$。

热损失（近似）与 $T r/w$ 成正比，因此在固定寿命期内，由热损失造成的 energy cost（能源成本）为 $\alpha_1 T r/w$。管（壁厚固定）的成本近似与总材料量成正比，即 $\alpha_2 r$。insulation 的成本也近似与总绝缘材料量成正比，即 $\alpha_3 r w$（用了 $w \ll r$）。总成本是这三项之和。

沿管向下的热流完全来自流体的流动，流体速度固定，即热流为 $\alpha_4 T r^2$。常数 $\alpha_i$ 全为正，变量 $T$、$r$、$w$ 也全为正。

现在的问题：在总成本上限 $C^{\max}$ 以及约束

```math
T^{\min} \le T \le T^{\max}, \quad r^{\min} \le r \le r^{\max}, \quad w^{\min} \le w \le w^{\max}, \quad w \le 0.1 r
```

之下，最大化沿管的热流。把该问题表述为 geometric program（几何规划）。

**Solution**。问题为

```math
\begin{aligned}
\text{maximize} \quad & \alpha_4 T r^2 \\
\text{subject to} \quad & \alpha_1 T w^{-1} + \alpha_2 r + \alpha_3 r w \le C^{\max} \\
& T^{\min} \le T \le T^{\max} \\
& r^{\min} \le r \le r^{\max} \\
& w^{\min} \le w \le w^{\max} \\
& w \le 0.1 r .
\end{aligned}
```

这等价于 GP

```math
\begin{aligned}
\text{minimize} \quad & (1/\alpha_4) T^{-1} r^{-2} \\
\text{subject to} \quad & (\alpha_1/C^{\max}) T w^{-1} + (\alpha_2/C^{\max}) r + (\alpha_3/C^{\max}) r w \le 1 \\
& (1/T^{\max}) T \le 1, \quad T^{\min} T^{-1} \le 1 \\
& (1/r^{\max}) r \le 1, \quad r^{\min} r^{-1} \le 1 \\
& (1/w^{\max}) w \le 1, \quad w^{\min} w^{-1} \le 1 \\
& 10\, w r^{-1} \le 1
\end{aligned}
```

（变量为 $T$、$r$、$w$）。

（改写规则：GP 要求 objective 为 posynomial（正项式）的极小化、约束为 posynomial ≤ 1。maximize 改 minimize 需取倒数；上界 $T \le T^{\max}$ 化为 $(1/T^{\max})T \le 1$，下界 $T \ge T^{\min}$ 化为 $T^{\min}T^{-1} \le 1$；$w \le 0.1r$ 化为 $10 w r^{-1} \le 1$。）

## 5.1 A simple example（一个简单的例子）

考虑优化问题

```math
\begin{aligned}
\text{minimize} \quad & x^2 + 1 \\
\text{subject to} \quad & (x-2)(x-4) \le 0,
\end{aligned}
```

变量为 $x \in \mathbf{R}$。

**(a) Analysis of primal problem（原问题分析）**。给出 feasible set、optimal value 与 optimal solution。

**(b) Lagrangian and dual function（Lagrangian 与 dual function）**。画出 objective $x^2 + 1$ 随 $x$ 的图像。在同一图上标出 feasible set、optimal point 与 optimal value，并对几个正的 $\lambda$ 值画出 Lagrangian $L(x,\lambda)$ 随 $x$ 的图像。验证 lower bound property（下界性质，$p^\star \ge \inf_x L(x,\lambda)$ 对 $\lambda \ge 0$）。推导并画出 Lagrange dual function $g$。

**(c) Lagrange dual problem（Lagrange 对偶问题）**。写出 dual problem，并验证它是 concave maximization problem。求 dual optimal value 与 dual optimal solution $\lambda^\star$。strong duality 成立吗？

**(d) Sensitivity analysis（灵敏度分析）**。设 $p^\star(u)$ 表示问题

```math
\begin{aligned}
\text{minimize} \quad & x^2 + 1 \\
\text{subject to} \quad & (x-2)(x-4) \le u
\end{aligned}
```

的 optimal value，作为参数 $u$ 的函数。画出 $p^\star(u)$。验证 $dp^\star(0)/du = -\lambda^\star$。

**Solution**。

**(a)** feasible set 是区间 $[2,4]$。（唯一的）optimal point 是 $x^\star = 2$，optimal value 为 $p^\star = 5$。

下图画出 $f_0$ 与 $f_1$。

[Figure 1：$f_0(x) = x^2+1$（抛物线，顶点 $(0,1)$）与 $f_1(x) = (x-2)(x-4) = x^2 - 6x + 8$（零点在 $x=2$ 与 $x=4$，顶点 $(3,-1)$）在 $x \in [-1,5]$ 上的图像；纵轴 $-5$ 到 $30$。feasible set $[2,4]$ 即 $f_1 \le 0$ 的那段]

**(b)** Lagrangian 为

```math
L(x,\lambda) = (1+\lambda)x^2 - 6\lambda x + (1+8\lambda).
```

下图画出 Lagrangian $L(x,\lambda) = f_0 + \lambda f_1$ 作为 $x$ 的函数，取不同的 $\lambda \ge 0$。注意 $L(x,\lambda)$ 关于 $x$ 的最小值（即 $g(\lambda)$）总是小于 $p^\star$。当 $\lambda$ 从 $0$ 增大到 $2$ 时它增大，在 $\lambda = 2$ 处达到最大，随后当 $\lambda$ 超过 $2$ 又下降。在 $\lambda = 2$ 处有 $p^\star = g(\lambda)$。

[Figure 2：$f_0$ 与 $f_0 + 1.0f_1$、$f_0 + 2.0f_1$、$f_0 + 3.0f_1$ 四条抛物线在 $x \in [-1,5]$ 上的图像；随 $\lambda$ 增大，抛物线顶点沿一条包络（图中虚线标注）移动并抬升；$\lambda = 2$ 那条恰在 $x = 2$ 处与 $f_0$ 相切于 $y = 5$]

当 $\lambda > -1$ 时，Lagrangian 在 $\tilde{x} = 3\lambda/(1+\lambda)$ 处取到其最小值；当 $\lambda \le -1$ 时，它下界无界。因此

```math
g(\lambda) = \begin{cases} -9\lambda^2/(1+\lambda) + 1 + 8\lambda & \lambda > -1 \\ -\infty & \lambda \le -1, \end{cases}
```

其图像见下。

（推导：$L$ 关于 $x$ 的二次项系数为 $1+\lambda$。若 $1+\lambda > 0$，令 $\partial L/\partial x = 2(1+\lambda)x - 6\lambda = 0$ 得 $\tilde{x} = 3\lambda/(1+\lambda)$，代入得 $g(\lambda) = (1+\lambda)\tilde{x}^2 - 6\lambda\tilde{x} + 1 + 8\lambda = 9\lambda^2/(1+\lambda) - 18\lambda^2/(1+\lambda) + 1 + 8\lambda = -9\lambda^2/(1+\lambda) + 1 + 8\lambda$。若 $1+\lambda \le 0$，二次项非正，$x \to \pm\infty$ 时 $L \to -\infty$。）

[Figure 3：dual function $g(\lambda)$ 的图像——$\lambda \in [-2,4]$，纵轴 $-10$ 到 $6$；曲线在 $\lambda = -1$ 处从 $-\infty$ 升起，单调上升至 $\lambda = 2$ 处的峰值 $g(2) = 5$，之后单调下降]

我们可以验证 dual function 是 concave 的，其值在 $\lambda^\star = 2$ 处等于 $p^\star = 5$，对其它 $\lambda$ 值小于 $p^\star$。

**(c)** Lagrange dual problem 为

```math
\begin{aligned}
\text{maximize} \quad & -9\lambda^2/(1+\lambda) + 1 + 8\lambda \\
\text{subject to} \quad & \lambda \ge 0 .
\end{aligned}
```

dual optimum 出现在 $\lambda^\star = 2$，$d^\star = 5$。因此对这个例子我们可以直接观察到 strong duality 成立（它必然成立——Slater's condition 满足）。

**(d)** 扰动后的问题对 $u < -1$ 不可行，因为 $\inf_x (x^2 - 6x + 8) = -1$。对 $u \ge -1$，feasible set 是区间

```math
[3 - \sqrt{1+u},\ 3 + \sqrt{1+u}],
```

它由 $x^2 - 6x + 8 = u$ 的两个根给出。对 $-1 \le u \le 8$，optimum 是 $x^\star(u) = 3 - \sqrt{1+u}$。对 $u \ge 8$，optimum 是 $f_0$ 的 unconstrained minimum，即 $x^\star(u) = 0$。汇总：

```math
p^\star(u) = \begin{cases} \infty & u < -1 \\ 11 + u - 6\sqrt{1+u} & -1 \le u \le 8 \\ 1 & u \ge 8 . \end{cases}
```

（验算：$x^\star = 3 - \sqrt{1+u}$ 时 $p^\star = (3-\sqrt{1+u})^2 + 1 = 9 - 6\sqrt{1+u} + (1+u) + 1 = 11 + u - 6\sqrt{1+u}$。在 $u = 8$、$\sqrt{1+u} = 3$ 处得 $11 + 8 - 18 = 1$，与 $x^\star = 0$ 时的 $p^\star = 1$ 衔接；在 $u = 0$ 处得 $11 - 6 = 5$，与 (a) 一致。）

下图画出 optimal value function $p^\star(u)$ 及其 epigraph。

[Figure 4：$p^\star(u)$ 曲线及其 epigraph（曲线上方的阴影区）——纵轴 $p^\star(u)$ 从 $-2$ 到 $10$，横轴 $u$ 从 $-2$ 到 $10$；$u < -1$ 时 $p^\star = +\infty$；$u \in [-1,8]$ 上曲线单调下降且 convex（到 $u=8$ 降到 $1$），$u > 8$ 后恒为 $1$；图中标出切线 $p^\star(0) - \lambda^\star u$，它在 $u=0$ 处与曲线相切且整体位于 epigraph 下方]

最后我们注意 $p^\star(u)$ 是 $u$ 的可微函数，并且

```math
\frac{dp^\star(0)}{du} = -2 = -\lambda^\star .
```

（核对：$dp^\star/du = 1 - 6/(2\sqrt{1+u})$，在 $u=0$ 处得 $1 - 3 = -2$，而 $\lambda^\star = 2$。）

# Solutions to additional exercises（附加题解答）

## 附加题 1 · Minimizing a function over the probability simplex（在概率单纯形上极小化函数）

给出 $x \in \mathbf{R}^n$ 在 probability simplex

```math
\{x \mid \mathbf{1}^T x = 1,\ x \succeq 0\}
```

上极小化可微 convex function $f$ 的简单 necessary and sufficient conditions（充要条件）。

**Solution**。最朴素的最优性条件是：$x$ feasible，即 $x \succeq 0$、$\mathbf{1}^T x = 1$，并且对所有 feasible $y$ 有

```math
\nabla f(x)^T (y - x) \ge 0 .
```

我们先证明这等价于

```math
\min_{i=1,\dots,n} \nabla f(x)_i \ge \nabla f(x)^T x .
```

为看清这一点，假设 $\nabla f(x)^T (y - x) \ge 0$ 对所有 feasible $y$ 成立。那么特别地取 $y = e_i$，得 $\nabla f(x)_i \ge \nabla f(x)^T x$，这就是上面那个式子。反方向：假设 $\nabla f(x)_i \ge \nabla f(x)^T x$ 对 $i = 1,\dots,n$ 成立。设 $y$ feasible，即 $y \succeq 0$、$\mathbf{1}^T y = 1$。把 $\nabla f(x)_i \ge \nabla f(x)^T x$ 乘以 $y_i$ 再求和，得

```math
\sum_{i=1}^n y_i \nabla f(x)_i \ge \Big( \sum_{i=1}^n y_i \Big) \nabla f(x)^T x = \nabla f(x)^T x .
```

左边就是 $y^T \nabla f(x)$，于是 $\nabla f(x)^T (y - x) \ge 0$。

现在可以进一步化简。上面的条件可写成

```math
\min_{i=1,\dots,n} \frac{\partial f}{\partial x_i} \ge \sum_{i=1}^n x_i \frac{\partial f}{\partial x_i} .
```

但由于 $\mathbf{1}^T x = 1$、$x \succeq 0$，有

```math
\min_{i=1,\dots,n} \frac{\partial f}{\partial x_i} \le \sum_{i=1}^n x_i \frac{\partial f}{\partial x_i},
```

由此得

```math
\min_{i=1,\dots,n} \frac{\partial f}{\partial x_i} = \sum_{i=1}^n x_i \frac{\partial f}{\partial x_i} .
```

右端是各 $\partial f/\partial x_i$ 的 convex combination（凸组合），等于所有项的最小值。这只有在「凡 $\partial f/\partial x_k > \min_i \partial f/\partial x_i$ 就有 $x_k = 0$」时才可能。

（† 译注：从 $\min \ge \sum x_i \partial_i f$ 与「convex combination 介于 min 与 max 之间，故 $\sum x_i \partial_i f \ge \min$」两条夹出等号——原文把后一半（组合的界）省略了，直接跳到等号，属于跳步。）

因此可以写出（必要的与充分的）optimality condition：$\mathbf{1}^T x = 1$，$x \succeq 0$，且对每个 $k$，

```math
x_k > 0 \ \Longrightarrow\ \frac{\partial f}{\partial x_k} = \min_{i=1,\dots,n} \frac{\partial f}{\partial x_i} .
```

特别地，对 $x_k > 0$ 的那些 $k$，$\partial f/\partial x_k$ 全都相等。

## 附加题 2 · Complex least-norm problem（复最小范数问题）

考虑 complex least $\ell_p$-norm problem（复 $\ell_p$ 最小范数问题）

```math
\begin{aligned}
\text{minimize} \quad & \|x\|_p \\
\text{subject to} \quad & Ax = b,
\end{aligned}
```

其中 $A \in \mathbf{C}^{m \times n}$、$b \in \mathbf{C}^m$，变量为 $x \in \mathbf{C}^n$。这里 $\|\cdot\|_p$ 表示 $\mathbf{C}^n$ 上的 $\ell_p$-norm，定义为

```math
\|x\|_p = \left( \sum_{i=1}^n |x_i|^p \right)^{1/p}
```

（$p \ge 1$），以及 $\|x\|_\infty = \max_{i=1,\dots,n} |x_i|$。假设 $A$ 满秩，且 $m < n$。

**(a)** 把 complex least $\ell_2$-norm problem 表述为一个实数据、实变量的 least $\ell_2$-norm problem。提示：用 $z = (\Re x, \Im x) \in \mathbf{R}^{2n}$ 作为变量。

**(b)** 把 complex least $\ell_\infty$-norm problem 表述为一个 SOCP。

**(c)** 求解 $m = 30$、$n = 100$ 的随机实例（两个问题都求）。生成矩阵 $A$ 可以用 Matlab 命令 `A = randn(m,n) + i*randn(m,n)`。类似地，用 `b = randn(m,1) + i*randn(m,1)` 生成向量 $b$。用 Matlab 命令 `scatter` 把两个问题的最优解画在复平面上，并（简要）评述你观察到的现象。可以用 cvx 函数 `norm(x,2)` 与 `norm(x,inf)` 求解这两个问题——它们被重载以处理复数参数。要使用这一特性，需要在 `variable` 语句中把变量声明为 complex。（特别地，你不必手工构造或求解 (b) 中的 SOCP。）

**Solution**。

**(a)** 定义 $z = (\Re x, \Im x) \in \mathbf{R}^{2n}$，则 $\|x\|_2^2 = \|z\|_2^2$。复线性方程组 $Ax = b$ 等价于 $\Re(Ax) = \Re b$ 且 $\Im(Ax) = \Im b$，而后者又可以表示成线性方程组

```math
\begin{bmatrix} \Re A & -\Im A \\ \Im A & \Re A \end{bmatrix} z = \begin{bmatrix} \Re b \\ \Im b \end{bmatrix} .
```

因此 complex least $\ell_2$-norm problem 可表述为

```math
\begin{aligned}
\text{minimize} \quad & \|z\|_2 \\
\text{subject to} \quad & \begin{bmatrix} \Re A & -\Im A \\ \Im A & \Re A \end{bmatrix} z = \begin{bmatrix} \Re b \\ \Im b \end{bmatrix} .
\end{aligned}
```

（这可以解析求解。）

（推导：若 $A = A_R + iA_I$、$x = x_R + ix_I$，则 $Ax = (A_R x_R - A_I x_I) + i(A_I x_R + A_R x_I)$，按实部/虚部拆开正是上面那个 $2m \times 2n$ 的块矩阵。）

**(b)** 用 epigraph 表述，引入新变量 $t$，把问题写成

```math
\begin{aligned}
\text{minimize} \quad & t \\
\text{subject to} \quad & \left\| \begin{bmatrix} z_i \\ z_{n+i} \end{bmatrix} \right\|_2 \le t, \quad i = 1,\dots,n \\
& \begin{bmatrix} \Re A & -\Im A \\ \Im A & \Re A \end{bmatrix} z = \begin{bmatrix} \Re b \\ \Im b \end{bmatrix} .
\end{aligned}
```

这是一个含 $n$ 个 second-order cone constraint（$\mathbf{R}^3$ 中的）的 SOCP。

（关键：$|x_i| = \sqrt{(\Re x_i)^2 + (\Im x_i)^2} = \|(z_i, z_{n+i})\|_2$，故 $\|x\|_\infty \le t$ 恰好是 $n$ 条二阶锥约束。）

**(c)**

```matlab
% complex minimum norm problem
%
randn('state',0);
m = 30; n = 100;
% generate matrix A
Are = randn(m,n); Aim = randn(m,n);
bre = randn(m,1); bim = randn(m,1);
A = Are + i*Aim;
b = bre + i*bim;
% 2-norm problem (analytical solution)
Atot = [Are -Aim; Aim Are];
btot = [bre; bim];
z_2 = Atot'*inv(Atot*Atot')*btot;
x_2 = z_2(1:100) + i*z_2(101:200);
% 2-norm problem solution with cvx
cvx_begin
variable x(n) complex
minimize( norm(x) )
subject to
A*x == b;
cvx_end
% inf-norm problem solution with cvx
cvx_begin
variable xinf(n) complex
minimize( norm(xinf,Inf) )
subject to
A*xinf == b;
cvx_end
% scatter plot
figure(1)
scatter(real(x),imag(x)), hold on,
scatter(real(xinf),imag(xinf),[],'filled'), hold off,
axis([-0.2 0.2 -0.2 0.2]), axis square,
xlabel('Re x'); ylabel('Im x');
```

$p = 2$（空心圆）与 $p = \infty$（实心圆）两种最优解各分量的图见下。$p = \infty$ 的最优解在 $Ax = b$ 约束下极小化 objective $\max_{i=1,\dots,n} |x_i|$，$x_i$ 的散点图显示它们几乎全部集中在复平面上的一个圆周围。这在意料之中——我们极小化的是 $x_i$ 的最大模，因此几乎所有的 $x_i$ 应当有大致相等的模 $|x_i|$。

[Figure 5：复平面散点图——横轴 $\Re x$、纵轴 $\Im x$，两者都取 $[-0.2, 0.2]$，`axis square`；空心圆（$p=2$ 解）散布在圆盘内部，呈各向不均匀的云状；实心圆（$p=\infty$ 解）密集贴在一个半径约为 $0.14$ 的圆周上]

## 附加题 3 · Numerical perturbation analysis example（数值扰动分析实例）

考虑 quadratic program（二次规划）

```math
\begin{aligned}
\text{minimize} \quad & x_1^2 + 2x_2^2 - x_1 x_2 - x_1 \\
\text{subject to} \quad & x_1 + 2x_2 \le u_1 \\
& x_1 - 4x_2 \le u_2 \\
& 5x_1 + 76x_2 \le 1,
\end{aligned}
```

变量为 $x_1$、$x_2$，参数为 $u_1$、$u_2$。

**(a)** 取参数值 $u_1 = -2$、$u_2 = -3$ 求解该 QP，求 optimal primal variable values（最优原始变量值）$x_1^\star$、$x_2^\star$ 与 optimal dual variable values（最优对偶变量值）$\lambda_1^\star$、$\lambda_2^\star$、$\lambda_3^\star$。记 optimal objective value 为 $p^\star$。验证你求得的 optimal primal 与 dual variables 满足 KKT conditions（在合理的数值精度内）。

提示：见 CVX 用户指南 §3.6，了解如何取出 optimal dual variables。指定 quadratic objective 用 `quad_form()`。

**(b)** 现在我们求解该 QP 的一些扰动版本，取

```math
u_1 = -2 + \delta_1, \qquad u_2 = -3 + \delta_2,
```

其中 $\delta_1$ 与 $\delta_2$ 各取 $\{-0.1, 0, 0.1\}$ 中的值。（含 $\delta_1 = \delta_2 = 0$ 的原问题在内，共九种组合。）对每组 $(\delta_1,\delta_2)$，对扰动后 QP 的 optimal value 作预测 $p^{\star}_{\mathrm{pred}}$，并与 perturbation 后 QP 的 exact optimal value $p^\star_{\mathrm{exact}}$（通过求解扰动 QP 得到）比较。把结果填入下表形式的右两列。检查不等式 $p^\star_{\mathrm{pred}} \le p^\star_{\mathrm{exact}}$ 成立。

| $\delta_1$ | $\delta_2$ | $p^\star_{\mathrm{pred}}$ | $p^\star_{\mathrm{exact}}$ |
|---|---|---|---|
| 0 | 0 | | |
| 0 | $-0.1$ | | |
| 0 | $0.1$ | | |
| $-0.1$ | 0 | | |
| $-0.1$ | $-0.1$ | | |
| $-0.1$ | $0.1$ | | |
| $0.1$ | 0 | | |
| $0.1$ | $-0.1$ | | |
| $0.1$ | $0.1$ | | |

**Solution**。

**(a)** 下面的 Matlab 代码建立这个简单 QP 并用 CVX 求解：

```matlab
Q = [1 -1/2; -1/2 2];
f = [-1 0]';
A = [1 2; 1 -4; 5 76];
b = [-2 -3 1]';
cvx_begin
variable x(2)
dual variable lambda
minimize(quad_form(x,Q)+f'*x)
subject to
lambda: A*x <= b
cvx_end
p_star = cvx_optval
```

运行后，optimal objective value 为 $p^\star = 8.22$，optimal point 为 $x_1^\star = -2.33$、$x_2^\star = 0.17$。（该 optimal point 唯一，因为 objective 是 strictly convex 的。）一组 optimal dual variables 为 $\lambda_1^\star = 1.46$、$\lambda_2^\star = 3.77$、$\lambda_3^\star = 0.12$。（dual optimal point 也唯一，但要证明这点更难，而且无关紧要。）

KKT conditions 为

```math
\begin{aligned}
& x_1^\star + 2x_2^\star \le u_1, \quad x_1^\star - 4x_2^\star \le u_2, \quad 5x_1^\star + 76x_2^\star \le 1 \\
& \lambda_1^\star \ge 0, \quad \lambda_2^\star \ge 0, \quad \lambda_3^\star \ge 0 \\
& \lambda_1^\star (x_1^\star + 2x_2^\star - u_1) = 0, \quad \lambda_2^\star (x_1^\star - 4x_2^\star - u_2) = 0, \\
& \lambda_3^\star (5x_1^\star + 76x_2^\star - 1) = 0, \\
& 2x_1^\star - x_2^\star - 1 + \lambda_1^\star + \lambda_2^\star + 5\lambda_3^\star = 0, \\
& 4x_2^\star - x_1^\star + 2\lambda_1^\star - 4\lambda_2^\star + 76\lambda_3^\star = 0 .
\end{aligned}
```

我们用数值检验这些条件。dual variable $\lambda_1^\star$、$\lambda_2^\star$、$\lambda_3^\star$ 全都大于零，而量

```matlab
A*x-b
2*Q*x+f+A'*lambda
```

都非常小。因此 KKT conditions 得到验证。

（注意两个等式正是 $\nabla f_0 + A^T\lambda = 0$ 的两行：$\partial f_0/\partial x_1 = 2x_1 - x_2 - 1$、$\partial f_0/\partial x_2 = 4x_2 - x_1$。若三个乘子全为正，则三条 constraint 全 active——可以用这组数据自检。）

**(b)** 预测的 optimal value 为

```math
p^\star_{\mathrm{pred}} = p^\star - \lambda_1^\star \delta_1 - \lambda_2^\star \delta_2 .
```

下面的 matlab 代码填出表格

```matlab
arr_i = [0 -1 1];
delta = 0.1;
pa_table = [];
for i = arr_i
for j = arr_i
p_pred = p_star - [lambda(1) lambda(2)]*[i; j]*delta;
cvx_begin
variable x(2)
minimize(quad_form(x,Q)+f'*x)
subject to
A*x <= b+[i;j;0]*delta
cvx_end
p_exact = cvx_optval;
pa_table = [pa_table; i*delta j*delta p_pred p_exact]
end
end
```

得到的数值为

| $\delta_1$ | $\delta_2$ | $p^\star_{\mathrm{pred}}$ | $p^\star_{\mathrm{exact}}$ |
|---|---|---|---|
| 0 | 0 | 8.22 | 8.22 |
| 0 | $-0.1$ | 8.60 | 8.70 |
| 0 | $0.1$ | 7.85 | 7.98 |
| $-0.1$ | 0 | 8.34 | 8.57 |
| $-0.1$ | $-0.1$ | 8.75 | 8.82 |
| $-0.1$ | $0.1$ | 7.99 | 8.32 |
| $0.1$ | 0 | 8.08 | 8.22 |
| $0.1$ | $-0.1$ | 8.45 | 8.71 |
| $0.1$ | $0.1$ | 7.70 | 7.75 |

可以验证不等式 $p^\star_{\mathrm{pred}} \le p^\star_{\mathrm{exact}}$ 在所有情形下都成立。

（这就是 first-order sensitivity（一阶灵敏度）的全局 lower bound：$p^\star_{\mathrm{pred}}$ 是 $p^\star(u)$ 在 $u = (-2,-3)$ 处的支撑超平面，convex function 的超平面永远在图像下方，故必有 $\le$。差距大小反映二阶项——$\delta$ 越大、曲率越大，差距越大。）

## 附加题 4 · FIR filter design（FIR 滤波器设计）

考虑（对称、linear phase（线性相位）的）FIR filter，由

```math
H(\omega) = a_0 + \sum_{k=1}^N a_k \cos k\omega
```

描述。设计变量是实系数 $a = (a_0,\dots,a_N) \in \mathbf{R}^{N+1}$。本题我们探索 low-pass filter（低通滤波器）的设计，specifications（指标）为：

- 对 $0 \le \omega \le \pi/3$，$0.89 \le H(\omega) \le 1.12$，即该滤波器在 passband（通带）$[0,\pi/3]$ 内约有 $\pm 1$ dB 的 ripple（纹波）。
- 对 $\omega_c \le \omega \le \pi$，$|H(\omega)| \le \alpha$。换句话说，该滤波器在 stopband（阻带）$[\omega_c,\pi]$ 内达到由 $\alpha$ 给出的 attenuation（衰减）。$\omega_c$ 称为 cutoff frequency（截止频率）。

这些 specifications 如下图所示。

[Figure 6：滤波器指标示意——横轴 $\omega$ 从 $0$ 到 $\pi$，纵轴 $H(\omega)$；在 $[0,\pi/3]$ 区间画出一个高度为 $0.89$ 到 $1.12$ 的水平带（passband 容差带，中心线 $1.00$）；在 $[\omega_c,\pi]$ 区间画出上下对称的两条水平线 $\pm\alpha$（stopband 上限），中间是 $0$；$\pi/3$ 与 $\omega_c$ 之间为 transition band（过渡带）]

**(a)** 假设固定 $\omega_c$ 与 $N$，希望最大化 stop-band attenuation（阻带衰减），即极小化 $\alpha$ 使上述 specifications 可以被满足。说明如何把它表述为 convex optimization problem。

**(b)** 假设固定 $N$ 与 $\alpha$，希望极小化 $\omega_c$，即固定 stopband attenuation 与滤波器长度，希望极小化 'transition' band（$\pi/3$ 与 $\omega_c$ 之间）。说明如何把该问题表述为 quasiconvex optimization problem。

**(c)** 现在假设固定 $\omega_c$ 与 $\alpha$，希望找出能满足 specifications 的最小 $N$，即寻找能满足 specifications 的最短 FIR 滤波器。这个问题可以表述为 convex 或 quasiconvex problem 吗？如果可以，说明如何；如果你认为不行，简要且非形式地说明为什么。

**(d)** 对 $N = 7$，画出 attenuation（$\alpha$）对 cutoff frequency（$\omega_c$）的 optimal tradeoff curve（最优折中曲线）。可达到的 specifications 集合是 convex 的吗？简要解释 optimal tradeoff curve 的任何有趣特征，例如平坦段。

本小问中你可以对 frequency（频率）离散化约束，意思如下。取 $K \gg N$（也许是 $K \approx 10N$），令 $\omega_k = k\pi/K$，$k = 0,\dots,K$。然后把 specifications 换成

- 对满足 $0 \le \omega_k \le \pi/3$ 的 $k$，$0.89 \le H(\omega_k) \le 1.12$。
- 对满足 $\omega_c \le \omega_k \le \pi$ 的 $k$，$|H(\omega_k)| \le \alpha$。

有了这个近似，(a) 中的问题变成一个 LP，你就可以数值求解 (d)。

**Solution**。

**(a)** 第一个问题可以表述为

```math
\begin{aligned}
\text{minimize} \quad & \alpha \\
\text{subject to} \quad & f_1(a) \le 1.12 \\
& f_2(a) \ge 0.89 \\
& f_3(a) \le \alpha \\
& f_4(a) \ge -\alpha,
\end{aligned}
```

（式 (2)）其中

```math
\begin{aligned}
f_1(a) &= \sup_{0 \le \omega \le \pi/3} H(\omega), & f_2(a) &= \inf_{0 \le \omega \le \pi/3} H(\omega), \\
f_3(a) &= \sup_{\omega_c \le \omega \le \pi} H(\omega), & f_4(a) &= \inf_{\omega_c \le \omega \le \pi} H(\omega) .
\end{aligned}
```

问题 (2) 关于变量 $a$、$\alpha$ 是 convex 的，因为 $f_1$ 与 $f_3$ 是 convex function（一组 affine function 的 pointwise supremum），而 $f_2$ 与 $f_4$ 是 concave function（一组 affine function 的 pointwise infimum）。

（† 译注：原文此处印作 "f4 and f5 are concave functions"，其中 $f_5$ 在 (a) 中并未定义（它到 (b) 才出现），应为 $f_2$ 与 $f_4$。这是原文笔误。）

**(b)** 该问题可以表述为

```math
\begin{aligned}
\text{minimize} \quad & f_5(a) \\
\text{subject to} \quad & f_1(a) \le 1.12 \\
& f_2(a) \ge 0.89,
\end{aligned}
```

其中 $f_1$、$f_2$ 与上面相同，而

```math
f_5(a) = \inf\{\Omega \mid -\alpha \le H(\omega) \le \alpha \ \text{for} \ \Omega \le \omega \le \pi\} .
```

这是关于变量 $a$ 的 quasiconvex optimization problem，因为 $f_1$ convex、$f_2$ concave，而 $f_5$ 是 quasiconvex 的：它的 sublevel sets（下水平集）是

```math
\{a \mid f_5(a) \le \Omega\} = \{a \mid -\alpha \le H(\omega) \le \alpha \ \text{for} \ \Omega \le \omega \le \pi\},
```

即无穷多个 halfspace 的交。

**(c)** 该问题可以表述为

```math
\begin{aligned}
\text{minimize} \quad & f_6(a) \\
\text{subject to} \quad & f_1(a) \le 1.12 \\
& f_2(a) \ge 0.89 \\
& f_3(a) \le \alpha \\
& f_4(a) \ge -\alpha,
\end{aligned}
```

其中 $f_1$、$f_2$、$f_3$、$f_4$ 如上定义，且

```math
f_6(a) = \min\{k \mid a_{k+1} = \cdots = a_N = 0\} .
```

$f_6$ 的 sublevel sets 是 affine sets：

```math
\{a \mid f_6(a) \le k\} = \{a \mid a_{k+1} = \cdots = a_N = 0\} .
```

这意味着 $f_6$ 是 quasiconvex function，于是我们又一次得到 quasiconvex optimization problem。

（要点：$f_6(a) \le k$ 的集合「后 $N-k$ 个系数全为零」是 $\mathbf{R}^{N+1}$ 中的子空间，子空间是 affine 的、更是 convex 的，故所有下水平集 convex ⇒ $f_6$ quasiconvex。注意这里 $N$ 是固定的上界，$f_6$ 度量的是「实际用到多长」。）

**(d)** 离散化后，(a) 中的问题可表述为 LP

```math
\begin{aligned}
\text{minimize} \quad & \alpha \\
\text{subject to} \quad & 0.89 \le H(\omega_i) \le 1.12 \quad \text{for} \ 0 \le \omega_i \le \pi/3 \\
& -\alpha \le H(\omega_i) \le \alpha \quad \text{for} \ \omega_c \le \omega_i \le \pi,
\end{aligned}
```

（式 (3)）变量为 $\alpha$ 与 $a$。（对固定的 $\omega_i$，$H(\omega_i)$ 是 $a$ 的 affine function，因此该问题中所有约束都是 $a$ 与 $\alpha$ 的 linear inequality。）我们对区间 $(\pi/3,\pi]$ 中一系列 $\omega_c$ 值求解该 LP，从而得到 $\alpha$ 对 $\omega_c$ 的 tradeoff curve。

图 (1) 由下面的 matlab 代码生成。

```matlab
clear all
N = 7;
K = 10*N;
k = [0:N]';
w = [0:K]'/K*pi;
idx= max(find(w<=pi/3));
alphas = [];
for i=idx:length(w)
cvx_begin
variables a(N+1,1)
minimize( norm(cos(w(i:end)*k')*a,inf) )
subject to
cos(w(1:idx)*k')*a >= 0.89
cos(w(1:idx)*k')*a <= 1.12
cvx_end
alphas = [alphas; cvx_optval];
end;
plot(w(idx:end),alphas,'-');
xlabel('wc');
ylabel('alpha');
```

[Figure 7（原文 Figure 1）：problem 4d 的 tradeoff curve——横轴 $\omega_c$ 从 $\pi/3$ 到 $\pi$，纵轴 $\alpha$ 从 $0$ 到 $1$；曲线在 $\omega_c = \pi/3$ 附近（transition band 极窄）$\alpha$ 接近 $1$（等于没有衰减），随 $\omega_c$ 增大 $\alpha$ 单调快速下降，在 $\omega_c$ 接近 $\pi$ 时 $\alpha$ 趋近于 $0$；曲线整体单调且呈凸状]

（译注：可达到的 $(\omega_c,\alpha)$ 集合在这个意义上是 convex 的——因为 $\alpha^\star(\omega_c)$ 是「对每个 $\omega_c$ 求 LP 最优值」得到的，而 LP 值是 $(\omega_c)$ 的 convex function（由 (3) 的形式可见，$\omega_c$ 变大放松了约束，故值单调不增；曲率则由 $H$ 的具体形状决定）。（d）中曲线的近 $\pi/3$ 端斜率很陡，反映 transition band 从零展宽时衰减改善最快。）

## 附加题 5 · Minimum fuel optimal control（最小燃料最优控制，数值实例）

求解 Convex Optimization 习题 4.16 描述的 minimum fuel optimal control problem，实例数据为

```math
A = \begin{bmatrix} -1 & 0.4 & 0.8 \\ 1 & 0 & 0 \\ 0 & 1 & 0 \end{bmatrix}, \quad
b = \begin{bmatrix} 1 \\ 0 \\ 0.3 \end{bmatrix}, \quad
x^{\mathrm{des}} = \begin{bmatrix} 7 \\ 2 \\ -6 \end{bmatrix}, \quad N = 30 .
```

你可以构造在习题 4.16 解答中得到的 LP，也可以更直接地用 cvx。画出 actuator signal $u(t)$ 随时间 $t$ 的变化。

**Solution**。下面的 Matlab 代码求出解

```matlab
close all
clear all
n=3; % state dimension
N=30; % time horizon
A=[ -1 0.4 0.8; 1 0 0 ; 0 1 0];
b=[ 1 0 0.3]';
x0 = zeros(n,1);
xdes = [ 7 2 -6]';
cvx_begin
variable X(n,N+1);
variable u(1,N);
minimize (sum(max(abs(u),2*abs(u)-1)))
subject to
X(:,2:N+1) == A*X(:,1:N)+b*u; % dynamics
X(:,1)== x0;
X(:,N+1)== xdes;
cvx_end
stairs(0:N-1,u,'linewidth',2)
axis tight
xlabel('t')
ylabel('u')
```

optimal actuator signal 见图 2。

[Figure 8（原文 Figure 2）：minimum fuel actuator signal 的阶梯图——横轴 $t$ 从 $0$ 到 $30$，纵轴 $u(t)$ 从 $-0.5$ 到 $3$；$t \in [0,5]$ 附近 $u \approx 3$（饱和高值），随后逐步下降到 $t \approx 12$ 时约 $0.2$，$t \in [15,24]$ 附近出现一段约 $0$ 的平坦区，$t \approx 25$ 之后略为负值（约 $-0.1$）；整体像一个「先大推力、再滑行、最后微调」的 bang-bang 型轮廓]

（读图要点：$f$ 在 $|a| \le 1$ 段是 $|a|$、在 $|a| > 1$ 段斜率减半，所以 LP 的解自然倾向于「先用到高幅值（成本仍是线性的），再用很小的幅值做精确修正」。$u \approx 1$ 处的拐点就是两个线性片段的接缝，会出现一段贴着 $|u| = 1$ 的折线特征。）
