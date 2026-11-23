# EE364a Homework 4 additional problems（EE364a 作业 4 附加题）

> 原文：`HW4 附加题.pdf`（4 页），EE364a, Winter 2007–08, Prof. S. Boyd。这是题面；解答见 `HW4 解答.pdf` 第 10 页起的 "Solutions to additional exercises"。

## 1. Minimizing a function over the probability simplex（在概率单纯形上极小化函数）

给出 $x \in \mathbf{R}^n$ 在 probability simplex（概率单纯形）$\{x \mid \mathbf{1}^T x = 1,\ x \succeq 0\}$ 上极小化可微 convex function $f$ 的简单 necessary and sufficient conditions（充要条件）。

## 2. Complex least-norm problem（复最小范数问题）

考虑 complex least $\ell_p$-norm problem

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

**(a)** 把 complex least $\ell_2$-norm problem 表述为一个实数据、实变量的 least $\ell_2$-norm problem，变量维度为 $2n$。提示：用 $z = (\Re x, \Im x) \in \mathbf{R}^{2n}$ 作为变量。

**(b)** 把 complex least $\ell_\infty$-norm problem 表述为一个 SOCP。

**(c)** 求解 $m = 30$、$n = 100$ 的随机实例（两个问题都求）。生成矩阵 $A$ 可以用 Matlab 命令 `A = randn(m,n) + i*randn(m,n)`。类似地，用 `b = randn(m,1) + i*randn(m,1)` 生成向量 $b$。用 Matlab 命令 `scatter` 把两个问题的最优解画在复平面上，并（简要）评述你观察到的现象。可以用 cvx 函数 `norm(x,2)` 与 `norm(x,inf)` 求解这两个问题——它们被重载以处理复数参数。要使用这一特性，需要在 `variable` 语句中把变量声明为 complex。（特别地，你不必手工构造或求解 (b) 中的 SOCP。）

## 3. Numerical perturbation analysis example（数值扰动分析实例）

考虑 quadratic program（二次规划）

```math
\begin{aligned}
\text{minimize} \quad & x_1^2 + 2x_2^2 - x_1 x_2 - x_1 \\
\text{subject to} \quad & x_1 + 2x_2 \le u_1 \\
& x_1 - 4x_2 \le u_2, \\
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

其中 $\delta_1$ 与 $\delta_2$ 各取 $\{-0.1, 0, 0.1\}$ 中的值。（含 $\delta_1 = \delta_2 = 0$ 的原问题在内，共九种组合。）对每组 $(\delta_1,\delta_2)$，对扰动后 QP 的 optimal value 作预测 $p^\star_{\mathrm{pred}}$，并与 perturbed QP 的 exact optimal value $p^\star_{\mathrm{exact}}$（通过求解扰动 QP 得到）比较。把结果填入下表形式的右两列。检查不等式 $p^\star_{\mathrm{pred}} \le p^\star_{\mathrm{exact}}$ 成立。

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

## 4. FIR filter design（FIR 滤波器设计）

考虑（对称、linear phase 的）FIR filter，由

```math
H(\omega) = a_0 + \sum_{k=1}^N a_k \cos k\omega
```

描述。设计变量是实系数 $a = (a_0,\dots,a_N) \in \mathbf{R}^{N+1}$。本题我们探索 low-pass filter（低通滤波器）的设计，specifications（指标）为：

- 对 $0 \le \omega \le \pi/3$，$0.89 \le H(\omega) \le 1.12$，即该滤波器在 passband（通带）$[0,\pi/3]$ 内约有 $\pm 1$ dB 的 ripple（纹波）。
- 对 $\omega_c \le \omega \le \pi$，$|H(\omega)| \le \alpha$。换句话说，该滤波器在 stopband（阻带）$[\omega_c,\pi]$ 内达到由 $\alpha$ 给出的 attenuation（衰减）。$\omega_c$ 称为 cutoff frequency（截止频率）。

这些 specifications 如下图所示。

[Figure 1：滤波器指标示意——横轴 $\omega$ 从 $0$ 经 $\pi/3$、$\omega_c$ 到 $\pi$，纵轴 $H(\omega)$；在 $[0,\pi/3]$ 上画出高度 $0.89$ 到 $1.12$ 的水平容差带（中心线 $1.00$，即 passband）；在 $[\omega_c,\pi]$ 上画出对称的水平线 $\pm\alpha$（stopband 上限），中间为 $0$；$\pi/3$ 与 $\omega_c$ 之间是 transition band]

**(a)** 假设固定 $\omega_c$ 与 $N$，希望最大化 stop-band attenuation，即极小化 $\alpha$ 使上述 specifications 可以被满足。说明如何把它表述为 convex optimization problem。

**(b)** 假设固定 $N$ 与 $\alpha$，希望极小化 $\omega_c$，即固定 stopband attenuation 与滤波器长度，希望极小化 'transition' band（$\pi/3$ 与 $\omega_c$ 之间）。说明如何把该问题表述为 quasiconvex optimization problem。

**(c)** 现在假设固定 $\omega_c$ 与 $\alpha$，希望找出能满足 specifications 的最小 $N$，即寻找能满足 specifications 的最短 FIR 滤波器。这个问题可以表述为 convex 或 quasiconvex problem 吗？如果可以，说明如何；如果你认为不行，简要且非形式地说明为什么。

**(d)** 对 $N = 7$，画出 attenuation（$\alpha$）对 cutoff frequency（$\omega_c$）的 optimal tradeoff curve（最优折中曲线）。可达到的 specifications 集合是 convex 的吗？简要解释 optimal tradeoff curve 的任何有趣特征，例如平坦段。

本小问中你可以对 frequency（频率）离散化约束，意思如下。取 $K \gg N$（也许是 $K \approx 10N$），令 $\omega_k = k\pi/K$，$k = 0,\dots,K$。然后把 specifications 换成

- 对满足 $0 \le \omega_k \le \pi/3$ 的 $k$，$0.89 \le H(\omega_k) \le 1.12$。
- 对满足 $\omega_c \le \omega_k \le \pi$ 的 $k$，$|H(\omega_k)| \le \alpha$。

有了这个近似，(a) 中的问题变成一个 LP，你就可以数值求解 (d)。

## 5. Minimum fuel optimal control（最小燃料最优控制）

求解 Convex Optimization 习题 4.16 描述的 minimum fuel optimal control problem，实例数据为

```math
A = \begin{bmatrix} -1 & 0.4 & 0.8 \\ 1 & 0 & 0 \\ 0 & 1 & 0 \end{bmatrix}, \quad
b = \begin{bmatrix} 1 \\ 0 \\ 0.3 \end{bmatrix}, \quad
x^{\mathrm{des}} = \begin{bmatrix} 7 \\ 2 \\ -6 \end{bmatrix}, \quad N = 30 .
```

你可以构造在习题 4.16 解答中得到的 LP，也可以更直接地用 cvx。画出 actuator signal $u(t)$ 随时间 $t$ 的变化。
