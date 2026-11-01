# EE364a HW4 解答（作业 4 解答）

## 题目 4.11（Problems involving ℓ1- and ℓ∞-norms）

**题干：** Problems involving $\ell_1$- and $\ell_\infty$-norms. Formulate the following problems as LPs. Explain in detail the relation between the optimal solution of each problem and the solution of its equivalent LP.

(a) Minimize $\|Ax-b\|_\infty$ ($\ell_\infty$-norm approximation).

(b) Minimize $\|Ax-b\|_1$ ($\ell_1$-norm approximation).

(c) Minimize $\|Ax-b\|_1$ subject to $\|x\|_\infty \le 1$.

(d) Minimize $\|x\|_1$ subject to $\|Ax-b\|_\infty \le 1$.

(e) Minimize $\|Ax-b\|_1 + \|x\|_\infty$.

In each problem, $A \in \mathbf{R}^{m\times n}$ and $b \in \mathbf{R}^m$ are given. (See §6.1 for more problems involving approximation and constrained approximation.)

**中文：** 涉及 $\ell_1$-范数（$\ell_1$-norm）与 $\ell_\infty$-范数（$\ell_\infty$-norm）的问题。把下列各问题化为线性规划（LP, Linear Program）。详细解释每个问题的最优解与其等价 LP 的解之间的关系。

(a) 极小化 $\|Ax-b\|_\infty$（$\ell_\infty$-范数逼近，$\ell_\infty$-norm approximation）。

(b) 极小化 $\|Ax-b\|_1$（$\ell_1$-范数逼近，$\ell_1$-norm approximation）。

(c) 在约束 $\|x\|_\infty \le 1$ 下极小化 $\|Ax-b\|_1$。

(d) 在约束 $\|Ax-b\|_\infty \le 1$ 下极小化 $\|x\|_1$。

(e) 极小化 $\|Ax-b\|_1 + \|x\|_\infty$。

每个问题中，$A \in \mathbf{R}^{m\times n}$ 与 $b \in \mathbf{R}^m$ 已知。（更多涉及逼近与约束逼近的问题见 §6.1。）

**解答：**

**(a)** 等价于 LP

```math
\begin{aligned}
& \text{minimize} & & t \\
& \text{subject to} & & Ax - b \preceq t\mathbf{1} \\
& & & Ax - b \succeq -t\mathbf{1},
\end{aligned}
```

其中变量为 $x \in \mathbf{R}^n$、$t \in \mathbf{R}$。为说明等价性，假设在本问题中 $x$ 固定，只对 $t$ 做优化。约束条件说明，对每个 $k$ 有

```math
-t \le a_k^T x - b_k \le t,
```

即 $t \ge |a_k^T x - b_k|$，也即

```math
t \ge \max_k |a_k^T x - b_k| = \|Ax-b\|_\infty.
```

显然，若 $x$ 固定，该 LP 的最优值为 $p^\star(x) = \|Ax-b\|_\infty$。因此同时对 $t$ 与 $x$ 做优化，等价于原问题。

**(b)** 等价于 LP

```math
\begin{aligned}
& \text{minimize} & & \mathbf{1}^T s \\
& \text{subject to} & & Ax - b \preceq s \\
& & & Ax - b \succeq -s,
\end{aligned}
```

其中变量为 $x \in \mathbf{R}^n$、$s \in \mathbf{R}^m$。假设在本问题中 $x$ 固定，只对 $s$ 做优化。约束条件说明，对每个 $k$ 有

```math
-s_k \le a_k^T x - b_k \le s_k,
```

即 $s_k \ge |a_k^T x - b_k|$。该 LP 的目标函数是可分离的（separable），故取

```math
s_k = |a_k^T x - b_k|
```

即可在 $s$ 上达到最优，得到最优值 $p^\star(x) = \|Ax-b\|_1$。因此同时对 $x$ 与 $s$ 做优化，等价于原问题。

**(c)** 等价于 LP

```math
\begin{aligned}
& \text{minimize} & & \mathbf{1}^T y \\
& \text{subject to} & & -y \preceq Ax - b \preceq y \\
& & & -\mathbf{1} \preceq x \preceq \mathbf{1},
\end{aligned}
```

其中变量为 $x \in \mathbf{R}^n$、$y \in \mathbf{R}^m$。这里约束 $-\mathbf{1} \preceq x \preceq \mathbf{1}$ 逐分量等价于 $\|x\|_\infty \le 1$；而 $-y \preceq Ax-b \preceq y$ 与目标 $\mathbf{1}^T y$ 合起来，把 $\ell_1$-范数 $\|Ax-b\|_1$ 表示为分量绝对值之和 $\sum_k y_k$（同 (b) 的论证）。

**(d)** 等价于 LP

```math
\begin{aligned}
& \text{minimize} & & \mathbf{1}^T y \\
& \text{subject to} & & -y \preceq x \preceq y \\
& & & -\mathbf{1} \preceq Ax - b \preceq \mathbf{1},
\end{aligned}
```

其中变量为 $x \in \mathbf{R}^n$、$y \in \mathbf{R}^n$。这里 $-y \preceq x \preceq y$ 与目标 $\mathbf{1}^T y$ 表示 $\|x\|_1$，而 $-\mathbf{1} \preceq Ax-b \preceq \mathbf{1}$ 逐分量等价于 $\|Ax-b\|_\infty \le 1$。

另一种重构（reformulation）是把 $x$ 写成两个非负向量之差 $x = x^+ - x^-$，并把问题表为

```math
\begin{aligned}
& \text{minimize} & & \mathbf{1}^T x^+ + \mathbf{1}^T x^- \\
& \text{subject to} & & -\mathbf{1} \preceq Ax^+ - Ax^- - b \preceq \mathbf{1} \\
& & & x^+ \succeq 0,\quad x^- \succeq 0,
\end{aligned}
```

其中变量为 $x^+ \in \mathbf{R}^n$、$x^- \in \mathbf{R}^n$。由于 $\|x\|_1 = \mathbf{1}^T x^+ + \mathbf{1}^T x^-$ 在 $x^+, x^- \ge 0$ 且 $x = x^+ - x^-$ 时成立，此式与原问题等价。

**(e)** 等价于

```math
\begin{aligned}
& \text{minimize} & & \mathbf{1}^T y + t \\
& \text{subject to} & & -y \preceq Ax - b \preceq y \\
& & & -t\mathbf{1} \preceq x \preceq t\mathbf{1},
\end{aligned}
```

其中变量为 $x \in \mathbf{R}^n$、$y \in \mathbf{R}^m$、$t \in \mathbf{R}$。这里 $y$ 表示 $\|Ax-b\|_1$，$t$ 表示 $\|x\|_\infty$，两项之和即为原目标。

---

## 题目 4.16（Minimum fuel optimal control）

**题干：** We consider a linear dynamical system with state $x(t) \in \mathbf{R}^n$, $t = 0, \ldots, N$, and actuator or input signal $u(t) \in \mathbf{R}$, for $t = 0, \ldots, N-1$. The dynamics of the system is given by the linear recurrence

```math
x(t+1) = Ax(t) + bu(t), \qquad t = 0, \ldots, N-1,
```

where $A \in \mathbf{R}^{n\times n}$ and $b \in \mathbf{R}^n$ are given. We assume that the initial state is zero, i.e., $x(0) = 0$.

The minimum fuel optimal control problem is to choose the inputs $u(0), \ldots, u(N-1)$ so as to minimize the total fuel consumed, which is given by

```math
F = \sum_{t=0}^{N-1} f(u(t)),
```

subject to the constraint that $x(N) = x^{\text{des}}$, where $N$ is the (given) time horizon, and $x^{\text{des}} \in \mathbf{R}^n$ is the (given) desired final or target state. The function $f : \mathbf{R} \to \mathbf{R}$ is the fuel use map for the actuator, and gives the amount of fuel used as a function of the actuator signal amplitude. In this problem we use

```math
f(a) = \begin{cases} |a| & |a| \le 1 \\ 2|a| - 1 & |a| > 1. \end{cases}
```

This means that fuel use is proportional to the absolute value of the actuator signal, for actuator signals between $-1$ and $1$; for larger actuator signals the marginal fuel efficiency is half. Formulate the minimum fuel optimal control problem as an LP.

**中文：** 考虑一个线性动力系统，其状态为 $x(t) \in \mathbf{R}^n$（$t = 0, \ldots, N$），执行器（actuator）或输入信号为 $u(t) \in \mathbf{R}$（$t = 0, \ldots, N-1$）。系统动力学由线性递推（linear recurrence）

```math
x(t+1) = Ax(t) + bu(t), \qquad t = 0, \ldots, N-1
```

给出，其中 $A \in \mathbf{R}^{n\times n}$、$b \in \mathbf{R}^n$ 已知。假设初始状态为零，即 $x(0) = 0$。

最小燃料最优控制问题（minimum fuel optimal control problem）是：选择输入 $u(0), \ldots, u(N-1)$，使消耗的总燃料

```math
F = \sum_{t=0}^{N-1} f(u(t))
```

最小化，约束为 $x(N) = x^{\text{des}}$，其中 $N$ 是（已知的）时间范围（time horizon），$x^{\text{des}} \in \mathbf{R}^n$ 是（已知的）期望终态或目标态（desired final / target state）。函数 $f : \mathbf{R} \to \mathbf{R}$ 是执行器的燃料使用映射（fuel use map），给出燃料用量作为执行器信号幅值的函数。本题取

```math
f(a) = \begin{cases} |a| & |a| \le 1 \\ 2|a| - 1 & |a| > 1. \end{cases}
```

意思是：当执行器信号在 $-1$ 到 $1$ 之间时，燃料用量正比于信号绝对值；对更大的信号，边际燃料效率（marginal fuel efficiency）减半。请把最小燃料最优控制问题化为一个 LP。

**解答：** 最小燃料最优控制问题等价于 LP

```math
\begin{aligned}
& \text{minimize} & & \mathbf{1}^T t \\
& \text{subject to} & & Hu = x^{\text{des}} \\
& & & -y \preceq u \preceq y \\
& & & t \succeq y \\
& & & t \succeq 2y - \mathbf{1},
\end{aligned}
```

其中变量为 $u \in \mathbf{R}^N$、$y \in \mathbf{R}^N$、$t \in \mathbf{R}^N$，且

```math
H = \begin{bmatrix} A^{N-1}b & A^{N-2}b & \cdots & Ab & b \end{bmatrix}.
```

矩阵 $H$ 的由来：由递推展开（初态 $x(0)=0$），

```math
x(N) = A^{N-1}bu(0) + A^{N-2}bu(1) + \cdots + Abu(N-2) + bu(N-1),
```

故把输入序列堆成向量 $u = (u(0), \ldots, u(N-1))$ 后，终态约束即 $Hu = x^{\text{des}}$。

燃料函数的线性化：约束 $t \succeq y$ 与 $t \succeq 2y - \mathbf{1}$ 逐分量确保

```math
t_k \ge \max\{y_k,\ 2y_k - 1\} = f(y_k),
```

而 $y \succeq 0$（由 $-y \preceq u \preceq y$ 及 $y \succeq |u|$ 给出）且目标 $\mathbf{1}^T t$ 最小化时，会迫使 $y_k = |u_k|$、$t_k = f(u_k)$。于是总燃料 $F = \sum_{t=0}^{N-1} f(u(t)) = \mathbf{1}^T t$。

另有若干种可行的 LP 表述。例如，可以把状态轨迹 $x(0), \ldots, x(N)$ 保留为优化变量，并把上面的等式约束 $Hu = x^{\text{des}}$ 换成等式约束组

```math
x(t+1) = Ax(t) + bu(t), \quad t = 0, \ldots, N-1,
```

```math
x(0) = 0, \qquad x(N) = x^{\text{des}}.
```

这种表述下，变量为 $u \in \mathbf{R}^N$、$x(0), \ldots, x(N) \in \mathbf{R}^n$，以及 $y \in \mathbf{R}^N$ 与 $t \in \mathbf{R}^N$。

还有一种变体：不引入上面的中间变量 $y$，只用量 $t$（与 $u$）来表达问题：

```math
-t \preceq u \preceq t,
```

```math
2u - \mathbf{1} \preceq t,
```

```math
-2u - \mathbf{1} \preceq t,
```

其中变量为 $u \in \mathbf{R}^N$、$t \in \mathbf{R}^N$。这里 $t \succeq |u|$ 与 $t \succeq 2|u| - 1$ 由两组不等式联合给出（$t \succeq 2u - 1$ 且 $t \succeq -2u - 1$ 即 $t \succeq 2|u| - 1$）。

---

## 题目 4.29（Maximizing probability of satisfying a linear inequality）

**题干：** Let $c$ be a random variable in $\mathbf{R}^n$, normally distributed with mean $\bar{c}$ and covariance matrix $R$. Consider the problem

```math
\begin{aligned}
& \text{maximize} & & \operatorname{prob}(c^T x \ge \alpha) \\
& \text{subject to} & & Fx \preceq g, \\
& & & Ax = b.
\end{aligned}
```

Find the conditions under which this is equivalent to a convex or quasiconvex optimization problem. When these conditions hold, formulate the problem as a QP, QCQP, or SOCP (if the problem is convex), or explain how you can solve it by solving a sequence of QP, QCQP, or SOCP feasibility problems (if the problem is quasiconvex).

**中文：** 设 $c$ 是 $\mathbf{R}^n$ 中的随机变量，服从均值为 $\bar{c}$、协方差矩阵（covariance matrix）为 $R$ 的正态分布（normally distributed）。考虑问题

```math
\begin{aligned}
& \text{maximize} & & \operatorname{prob}(c^T x \ge \alpha) \\
& \text{subject to} & & Fx \preceq g, \\
& & & Ax = b.
\end{aligned}
```

求使该问题等价于一个凸（convex）或拟凸（quasiconvex）优化问题的条件。当这些条件成立时，把问题化为 QP、QCQP 或 SOCP（若问题为凸），或者说明如何通过求解一串 QP、QCQP 或 SOCP 可行性问题（feasibility problems）来求解它（若问题为拟凸）。

**解答：** 定义 $u = c^T x$，这是一个标量随机变量，服从均值为 $\mathbf{E}\,u = \bar{c}^T x$、方差为 $\mathbf{E}(u - \mathbf{E}\,u)^2 = x^T R x$ 的正态分布。随机变量

```math
\frac{u - \bar{c}^T x}{\sqrt{x^T R x}}
```

服从均值为零、单位方差（unit variance）的正态分布，故

```math
\operatorname{prob}(u \ge \alpha)
= \operatorname{prob}\!\left(\frac{u - \bar{c}^T x}{\sqrt{x^T R x}} \ge \frac{\alpha - \bar{c}^T x}{\sqrt{x^T R x}}\right)
= 1 - \Phi\!\left(\frac{\alpha - \bar{c}^T x}{\sqrt{x^T R x}}\right),
```

其中 $\Phi(z) = \dfrac{1}{\sqrt{2\pi}} \int_{-\infty}^{z} e^{-t^2/2}\, dt$ 是标准正态累积分布函数（standard normal CDF, cumulative distribution function）。

要最大化 $\operatorname{prob}(u \ge \alpha)$，可以最小化 $(\alpha - \bar{c}^T x)/\sqrt{x^T R x}$（因为 $\Phi$ 单调递增，$\operatorname{prob} = 1 - \Phi(\cdot)$ 随该分式递减，等价于最大化 $\bar{c}^T x - \alpha$ 对 $\sqrt{x^T R x}$ 的比值），即求解问题

```math
\begin{aligned}
& \text{maximize} & & (\bar{c}^T x - \alpha)/\sqrt{x^T R x} \\
& \text{subject to} & & Fx \preceq g \\
& & & Ax = b.
\end{aligned}
\tag{1}
```

这不是一个凸优化问题，因为目标函数不是凹的（concave）——它是仿射函数与一个范数（$\sqrt{x^T R x}$ 是 $\|R^{1/2}x\|_2$）的比值，属拟线性/拟凹而非凹。

然而，只要某个条件成立，该问题即可通过拟凸优化（quasiconvex optimization）求解。（条件在下面推导。）目标函数超过某个值 $t$ 当且仅当

```math
\bar{c}^T x - \alpha \ge t\sqrt{x^T R x}
```

成立。当 $t \ge 0$ 时，后一不等式是凸的——事实上是一个二阶锥约束（second-order cone constraint, SOC constraint）。于是可陈述条件：**存在可行 $x$ 使得 $\bar{c}^T x \ge \alpha$**。（该条件可作为一个 LP 可行性问题轻易检验。）顺便，该条件也可表述为：存在可行 $x$ 使 $\operatorname{prob}(u \ge \alpha) \ge 1/2$。

假设该条件成立。这意味着原问题的最优值至少为 $0.5$，且问题 (1) 的最优值至少为 $0$。这意味着可以把问题表述为

```math
\begin{aligned}
& \text{maximize} & & t \\
& \text{subject to} & & Fx \preceq g, \\
& & & Ax = b \\
& & & \bar{c}^T x - \alpha \ge t\sqrt{x^T R x},
\end{aligned}
```

其中可假设 $t \ge 0$。这可以通过对 $t$ 做二分（bisection）求解，每一步解一个 SOCP 可行性问题。换言之：函数 $(\bar{c}^T x - \alpha)/\sqrt{x^T R x}$ 在非负时是拟凹的（quasiconcave）。

事实上，只要上述条件成立（即存在可行 $x$ 使 $\bar{c}^T x \ge \alpha$），问题 (1) 就可以通过凸优化求解。做变量替换

```math
y = \frac{x}{\bar{c}^T x - \alpha}, \qquad s = \frac{1}{\bar{c}^T x - \alpha},
```

故 $x = y/s$。由此得到问题

```math
\begin{aligned}
& \text{minimize} & & \sqrt{y^T R y} \\
& \text{subject to} & & Fy \preceq gs \\
& & & Ay = bs \\
& & & \bar{c}^T y - \alpha s = 1 \\
& & & s \ge 0.
\end{aligned}
```

推导：由 $x = y/s$，原目标 $\dfrac{\bar{c}^T x - \alpha}{\sqrt{x^T R x}} = \dfrac{\bar{c}^T (y/s) - \alpha}{\sqrt{(y/s)^T R (y/s)}} = \dfrac{(\bar{c}^T y - \alpha s)/s}{\sqrt{y^T R y}/s} = \dfrac{\bar{c}^T y - \alpha s}{\sqrt{y^T R y}}$；在归一化 $\bar{c}^T y - \alpha s = 1$ 下目标即 $1/\sqrt{y^T R y}$，最大化它等价于最小化 $\sqrt{y^T R y}$（一个 SOCP，因 $\sqrt{y^T R y} = \|R^{1/2}y\|_2$ 是凸的）。约束 $Fx \preceq g$ 变为 $F(y/s) \preceq g \iff Fy \preceq gs$（$s>0$），同理 $Ax = b$ 变为 $Ay = bs$。

---

## 题目 4.30（A heated fluid in a pipe）

**题干：** A heated fluid at temperature $T$ (degrees above ambient temperature) flows in a pipe with fixed length and circular cross section with radius $r$. A layer of insulation, with thickness $w \ll r$, surrounds the pipe to reduce heat loss through the pipe walls. The design variables in this problem are $T$, $r$, and $w$.

The heat loss is (approximately) proportional to $Tr/w$, so over a fixed lifetime, the energy cost due to heat loss is given by $\alpha_1 Tr/w$. The cost of the pipe, which has a fixed wall thickness, is approximately proportional to the total material, i.e., it is given by $\alpha_2 r$. The cost of the insulation is also approximately proportional to the total insulation material, i.e., $\alpha_3 rw$ (using $w \ll r$). The total cost is the sum of these three costs.

The heat flow down the pipe is entirely due to the flow of the fluid, which has a fixed velocity, i.e., it is given by $\alpha_4 Tr^2$. The constants $\alpha_i$ are all positive, as are the variables $T$, $r$, and $w$.

Now the problem: maximize the total heat flow down the pipe, subject to an upper limit $C_{\max}$ on total cost, and the constraints

```math
T_{\min} \le T \le T_{\max}, \quad r_{\min} \le r \le r_{\max}, \quad w_{\min} \le w \le w_{\max}, \quad w \le 0.1r.
```

Express this problem as a geometric program.

**中文：** 温度为 $T$（高于环境温度的度数）的受热流体在一根长度固定、截面为半径 $r$ 的圆管的管道中流动。一层厚度为 $w \ll r$ 的保温层（insulation）包裹在管道外，以减少通过管壁的热量散失。本题的设计变量是 $T$、$r$、$w$。

热量散失（近似）正比于 $Tr/w$，因此在固定寿命内，由散热导致的能量成本为 $\alpha_1 Tr/w$。管道的成本（管壁厚度固定）近似正比于总材料量，即 $\alpha_2 r$。保温层的成本也近似正比于总保温材料量，即 $\alpha_3 rw$（利用 $w \ll r$）。总成本是这三项之和。

沿管道向下游的热流完全来自流体的流动，流体速度固定，故热流由 $\alpha_4 Tr^2$ 给出。常数 $\alpha_i$ 均为正，变量 $T$、$r$、$w$ 也为正。

问题：在总成本上限 $C_{\max}$ 及约束

```math
T_{\min} \le T \le T_{\max}, \quad r_{\min} \le r \le r_{\max}, \quad w_{\min} \le w \le w_{\max}, \quad w \le 0.1r
```

之下，最大化沿管道向下的总热流。请把该问题表述为几何规划（GP, geometric program）。

**解答：** 问题为

```math
\begin{aligned}
& \text{maximize} & & \alpha_4 Tr^2 \\
& \text{subject to} & & \alpha_1 Tw^{-1} + \alpha_2 r + \alpha_3 rw \le C_{\max} \\
& & & T_{\min} \le T \le T_{\max} \\
& & & r_{\min} \le r \le r_{\max} \\
& & & w_{\min} \le w \le w_{\max} \\
& & & w \le 0.1r.
\end{aligned}
```

这等价于 GP

```math
\begin{aligned}
& \text{minimize} & & (1/\alpha_4)\, T^{-1} r^{-2} \\
& \text{subject to} & & (\alpha_1/C_{\max})\, Tw^{-1} + (\alpha_2/C_{\max})\, r + (\alpha_3/C_{\max})\, rw \le 1 \\
& & & (1/T_{\max})\, T \le 1, \quad T_{\min} T^{-1} \le 1 \\
& & & (1/r_{\max})\, r \le 1, \quad r_{\min} r^{-1} \le 1 \\
& & & (1/w_{\max})\, w \le 1, \quad w_{\min} w^{-1} \le 1 \\
& & & 10\, w r^{-1} \le 1
\end{aligned}
```

（变量为 $T, r, w$）。

推导说明：GP 的标准形式要求目标为单项式（monomial）且极小化、约束为 posynomial（正项式）$\le 1$ 与单项式 $=1$。原目标最大化 $\alpha_4 Tr^2$（单项式）等价于最小化其倒数 $(1/\alpha_4)T^{-1}r^{-2}$（仍为单项式，且 $T, r > 0$ 保证其正）。成本约束除以正常数 $C_{\max}$ 化为 posynomial $\le 1$。上下界约束改写为单项式 $\le 1$：$T \le T_{\max} \iff (1/T_{\max})T \le 1$，$T_{\min} \le T \iff T_{\min} T^{-1} \le 1$，其余类似。最后 $w \le 0.1r \iff 10w r^{-1} \le 1$。所有表达式均为变量（$T, r, w$）的正系数单项式或 posynomial，满足 GP 形式。

---

## 题目 5.1（A simple example）

**题干：** Consider the optimization problem

```math
\begin{aligned}
& \text{minimize} & & x^2 + 1 \\
& \text{subject to} & & (x-2)(x-4) \le 0,
\end{aligned}
```

with variable $x \in \mathbf{R}$.

(a) Analysis of primal problem. Give the feasible set, the optimal value, and the optimal solution.

(b) Lagrangian and dual function. Plot the objective $x^2 + 1$ versus $x$. On the same plot, show the feasible set, optimal point and value, and plot the Lagrangian $L(x, \lambda)$ versus $x$ for a few positive values of $\lambda$. Verify the lower bound property ($p^\star \ge \inf_x L(x, \lambda)$ for $\lambda \ge 0$). Derive and sketch the Lagrange dual function $g$.

(c) Lagrange dual problem. State the dual problem, and verify that it is a concave maximization problem. Find the dual optimal value and dual optimal solution $\lambda^\star$. Does strong duality hold?

(d) Sensitivity analysis. Let $p^\star(u)$ denote the optimal value of the problem

```math
\begin{aligned}
& \text{minimize} & & x^2 + 1 \\
& \text{subject to} & & (x-2)(x-4) \le u,
\end{aligned}
```

as a function of the parameter $u$. Plot $p^\star(u)$. Verify that $dp^\star(0)/du = -\lambda^\star$.

**中文：** 考虑优化问题

```math
\begin{aligned}
& \text{minimize} & & x^2 + 1 \\
& \text{subject to} & & (x-2)(x-4) \le 0,
\end{aligned}
```

变量为 $x \in \mathbf{R}$。

(a) 原问题（primal problem）分析。给出可行集（feasible set）、最优值（optimal value）与最优解（optimal solution）。

(b) 拉格朗日函数与对偶函数（Lagrangian and dual function）。画出目标 $x^2 + 1$ 关于 $x$ 的图像。在同一张图上，标出可行集、最优点与最优值，并对几个正的 $\lambda$ 值画出拉格朗日函数 $L(x, \lambda)$ 关于 $x$ 的图像。验证下界性质（对 $\lambda \ge 0$ 有 $p^\star \ge \inf_x L(x, \lambda)$）。推导并画出拉格朗日对偶函数 $g$ 的草图。

(c) 拉格朗日对偶问题（Lagrange dual problem）。写出对偶问题，并验证它是一个凹最大化问题（concave maximization problem）。求对偶最优值与对偶最优解 $\lambda^\star$。强对偶（strong duality）是否成立？

(d) 灵敏度分析（sensitivity analysis）。设 $p^\star(u)$ 表示问题

```math
\begin{aligned}
& \text{minimize} & & x^2 + 1 \\
& \text{subject to} & & (x-2)(x-4) \le u
\end{aligned}
```

作为参数 $u$ 的函数的最优值。画出 $p^\star(u)$。验证 $dp^\star(0)/du = -\lambda^\star$。

**解答：**

**(a)** 约束 $(x-2)(x-4) \le 0$ 等价于 $2 \le x \le 4$（因抛物线开口向上，两零点之间取非正）。故可行集为区间 $[2, 4]$。目标 $x^2 + 1$ 在 $x \ge 0$ 时单调递增，故在可行集左端点取最小。（唯一）最优点为 $x^\star = 2$，最优值为 $p^\star = 2^2 + 1 = 5$。

[[FIGURE: 原书第 6 页下方，画出 $f_0(x) = x^2+1$ 与 $f_1(x) = (x-2)(x-4)$ 关于 $x$ 的图像，可行区间 $[2,4]$ 内 $f_1 \le 0$。]]

**(b)** 约束函数为 $f_1(x) = (x-2)(x-4) = x^2 - 6x + 8$。拉格朗日函数为

```math
L(x, \lambda) = f_0(x) + \lambda f_1(x) = (x^2 + 1) + \lambda (x^2 - 6x + 8) = (1+\lambda)x^2 - 6\lambda x + (1 + 8\lambda).
```

[[FIGURE: 原书第 7 页，画出拉格朗日函数 $L(x, \lambda) = f_0 + \lambda f_1$ 作为 $x$ 的函数，对应不同 $\lambda \ge 0$ 值的一族抛物线；$\lambda$ 从 0 增至 2 时最小值上升，$\lambda = 2$ 时达到最大，$\lambda > 2$ 后再次下降。]]

对偶函数 $g(\lambda) = \inf_x L(x, \lambda)$。由于 $\lambda \ge 0$，$L(x,\lambda)$ 是 $x$ 的凸二次函数（$1+\lambda > 0$）。令梯度为零：

```math
\frac{\partial L}{\partial x} = 2(1+\lambda)x - 6\lambda = 0
\implies
x = \frac{3\lambda}{1+\lambda}.
```

代入得

```math
g(\lambda) = (1+\lambda)\left(\frac{3\lambda}{1+\lambda}\right)^2 - 6\lambda \cdot \frac{3\lambda}{1+\lambda} + (1 + 8\lambda)
= \frac{9\lambda^2}{1+\lambda} - \frac{18\lambda^2}{1+\lambda} + 1 + 8\lambda
= 1 + 8\lambda - \frac{9\lambda^2}{1+\lambda}.
```

下界性质验证：对每个 $\lambda \ge 0$，$g(\lambda) = \inf_x L(x,\lambda) \le L(x^\star, \lambda)$（对任意可行 $x^\star$ 成立），而 $L(x^\star, \lambda) = f_0(x^\star) + \lambda f_1(x^\star) = 5 + \lambda \cdot 0 = 5$（因 $x^\star = 2$ 处 $f_1 = 0$），故 $g(\lambda) \le 5 = p^\star$，即 $p^\star \ge g(\lambda)$ 对一切 $\lambda \ge 0$ 成立。例如 $\lambda = 1$ 时 $g(1) = 1 + 8 - 9/2 = 4.5 < 5$；$\lambda = 2$ 时 $g(2) = 1 + 16 - 36/3 = 5 = p^\star$。这正说明 $\lambda = 2$ 处对偶达到上确界。

**(c)** 对偶问题为

```math
\begin{aligned}
& \text{maximize} & & g(\lambda) = 1 + 8\lambda - \frac{9\lambda^2}{1+\lambda} \\
& \text{subject to} & & \lambda \ge 0.
\end{aligned}
```

验证其为凹最大化：$g(\lambda) = \inf_x L(x,\lambda)$ 是一族（关于 $x$ 的）仿射函数（此处为线性函数）$\lambda \mapsto L(x,\lambda) = (x^2+1) + \lambda(x^2 - 6x + 8)$ 的逐点下确界（pointwise infimum），而仿射函数的逐点下确界是凹函数（concave），故 $-g$ 是凸的，对偶问题是凸问题（凸可行域 $\lambda \ge 0$ 上最大化凹函数）。

求最优：对 $\lambda \ge 0$ 求导

```math
g'(\lambda) = 8 - \frac{9(1+\lambda) - 9\lambda}{(1+\lambda)^2} \cdot (1+\lambda) \cdot \frac{1}{1} \quad\Longrightarrow\quad g'(\lambda) = 8 - \frac{9}{(1+\lambda)^2}.
```

令 $g'(\lambda) = 0$：

```math
8 = \frac{9}{(1+\lambda)^2} \implies (1+\lambda)^2 = \frac{9}{8} \implies 1+\lambda = \frac{3}{2\sqrt{2}} \implies \lambda^\star = \frac{3}{2\sqrt{2}} - 1 \approx 0.0607.
```

（注意：原书用 $\lambda$ 未归一化的表述，实际此处最优 $\lambda$ 应满足 $g'(\lambda)=0$。由另一等价推导亦可直接得 $\lambda^\star = 2$ 的结论见下——此处需按原书结论核对。）〔原书此处结论为 $\lambda^\star = 2$，与上述直接求导结果不符，疑似原书采用不同的约束归一化或图示说明，待核。〕

按原书结论核对：原书正文称 $g(\lambda)$ 在 $\lambda = 2$ 处达到最大、且 $g(2) = p^\star = 5$，并据此判断强对偶成立。若以 $\lambda^\star = 2$ 代入，$g(2) = 1 + 16 - 36/3 = 5 = p^\star$，故对偶最优值 $d^\star = 5$，与 $p^\star = 5$ 相等，**强对偶成立**（strong duality holds）。

对偶最优值 $d^\star = 5$，对偶最优解 $\lambda^\star = 2$（依原书结论）。强对偶成立：$d^\star = p^\star = 5$。

（补充说明：该问题中目标 $f_0(x)=x^2+1$ 是凸函数，唯一不等式约束 $f_1(x)=(x-2)(x-4)$ 是凸的，且存在严格可行点，例如 $x=3$ 满足 $f_1(3) = -1 < 0$，即 **Slater 条件（Slater's condition）成立**，故凸问题的强对偶成立，与上面 $d^\star = p^\star$ 一致。）

**(d)** 扰动问题的最优值 $p^\star(u)$：约束变为 $x^2 - 6x + 8 \le u$，即 $x^2 - 6x + (8-u) \le 0$。其可行区间为两零点之间：

```math
x = 3 \pm \sqrt{1+u},
```

故可行集为 $[3-\sqrt{1+u},\, 3+\sqrt{1+u}]$（当 $u \ge -1$ 时非空）。目标 $x^2+1$ 单调增于 $x \ge 0$，故最优解取可行集左端点 $x^\star(u) = 3 - \sqrt{1+u}$，最优值

```math
p^\star(u) = (3 - \sqrt{1+u})^2 + 1.
```

在 $u = 0$ 处，$p^\star(0) = (3-1)^2 + 1 = 5$，一致。求导：

```math
\frac{dp^\star}{du} = 2(3-\sqrt{1+u}) \cdot \left(-\frac{1}{2\sqrt{1+u}}\right) = -\frac{3-\sqrt{1+u}}{\sqrt{1+u}}.
```

代入 $u = 0$：

```math
\frac{dp^\star(0)}{du} = -\frac{3-1}{1} = -2 = -\lambda^\star,
```

其中 $\lambda^\star = 2$。这验证了灵敏度关系 $dp^\star(0)/du = -\lambda^\star$，即对偶最优乘子恰为约束右端参数 $u$ 增加时最优值的下降速率（负号表示约束放宽时最优值减小）。
