> <span style="color:#7f8c8d;">Source: 作业解答/HW8 解答.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

## EE364a Homework 8 Solutions（EE364a 作业 8 解答）

> <span style="color:#7f8c8d;">EE364a, Winter 2007-08 · Prof. S. Boyd · 本文档为作业 8 的官方解答，覆盖教材习题 9.8、10.1、11.13，以及附加习题（标准形式 LP 的障碍法）1–3</span>

<!-- p.1 -->

### <span style="color:#c0392b;">9.8 ℓ∞-范数下的最速下降法（Steepest descent method in ℓ∞-norm）</span>

<span style="color:#2471a3;">**[question]**</span>

**9.8 Steepest descent method in ℓ∞-norm.** Explain how to find a steepest descent direction in the ℓ∞-norm, and give a simple interpretation.

<span style="color:#2471a3;">**[solution]**</span>

> **解答（Solution）.** 归一化最速下降方向（normalized steepest descent direction）为
>
> ```math
> \Delta x_{\mathrm{nsd}} = -\operatorname{sign}(\nabla f(x)),
> ```
>
> 其中 $\operatorname{sign}$ 按分量（componentwise）取值。直观解释（interpretation）：若 $f$ 对 $x_k$ 的偏导数（partial derivative）为正，我们就取一个使 $x_k$ 减小的步；若它为负，就取一个使 $x_k$ 增大的步。
>
> 未归一化最速下降方向（unnormalized steepest descent direction）为
>
> ```math
> \Delta x_{\mathrm{sd}} = -\|\nabla f(x)\|_1\, \operatorname{sign}(\nabla f(x)).
> ```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 「最速下降」为什么必须指明范数？因为它的定义是在范数单位球上极小化 $\nabla f(x)^T \Delta x$：单位球是什么形状，方向就往哪里指。$\ell_\infty$ 范数的单位球是立方体，其对偶范数（dual norm）是 $\ell_1$ 范数，因此归一化方向 $\Delta x_{\mathrm{nsd}} = -\operatorname{sign}(\nabla f(x))$ 的最优一阶下降量恰为 $-\|\nabla f(x)\|_1$——这正是「$\ell_\infty$ 的对偶是 $\ell_1$」的直接应用，与你在 Strang 线性代数中见过的对偶思想（$\max_{\|u\| \le 1} u^T v$ 型极值问题）一脉相承；而未归一化方向保证 $\nabla f(x)^T \Delta x_{\mathrm{sd}} = -\|\nabla f(x)\|_1^2$。这种「逐分量取符号」的步法与坐标下降（coordinate descent）的 Gauss–Southwell 规则（选最大偏导数对应的坐标）精神相通：都是把下降翻译到坐标层面，区别在于坐标下降每次只动一个坐标，而这里所有坐标同幅同向同动。Boyd 书 9.4 节还证明：若改用二次范数 $\|z\|_P = (z^T P z)^{1/2}$，最速下降方向就是 $-P^{-1}\nabla f(x)$；取 $P = \nabla^2 f(x)$ 便退化为牛顿方向——三种范数下的最速下降由此统一成「换一个度量」这一个动作。

### <span style="color:#c0392b;">10.1 KKT 矩阵的非奇异性（Nonsingularity of the KKT matrix）</span>

<span style="color:#2471a3;">**[question]**</span>

**10.1 Nonsingularity of the KKT matrix.** Consider the KKT matrix

```math
\begin{bmatrix}
P & A^T \\
A & 0
\end{bmatrix},
```

where $P \in \mathbf{S}^n_+$, $A \in \mathbb{R}^{p \times n}$, and $\operatorname{rank} A = p < n$.

(a) Show that each of the following statements is equivalent to nonsingularity of the KKT matrix.

- $\mathcal{N}(P) \cap \mathcal{N}(A) = \{0\}$
- $Ax = 0,\; x \neq 0 \implies x^T P x > 0$
- $F^T P F \succ 0$, where $F \in \mathbb{R}^{n \times (n-p)}$ is a matrix for which $\mathcal{R}(F) = \mathcal{N}(A)$
- $P + A^T Q A \succ 0$ for some $Q \succeq 0$

(b) Show that if the KKT matrix is nonsingular, then it has exactly $n$ positive and $p$ negative eigenvalues.

<span style="color:#2471a3;">**[solution]**</span>

> **解答（Solution）.**
>
> **(a)**
>
> - **条件 1 与条件 2（Conditions 1 and 2）.** 若 $x \in \mathcal{N}(A) \cap \mathcal{N}(P)$（两个零空间 nullspace 之交）且 $x \neq 0$，则 $Ax = 0$、$x \neq 0$，但 $x^T P x = 0$，这与第二个命题矛盾。反过来，假设第二个命题不成立，即存在 $x$ 满足 $Ax = 0$、$x \neq 0$，但 $x^T P x = 0$。由于 $P \succeq 0$，我们得出 $Px = 0$，即 $x \in \mathcal{N}(P)$，这与第一个命题矛盾。
>
> - **条件 2 与条件 3（Conditions 2 and 3）.** 若 $Ax = 0$、$x \neq 0$，则 $x$ 必定形如 $x = Fz$，其中 $z \neq 0$（因为 $\operatorname{rank}(F) = n - p$）。于是我们有 $x^T P x = z^T F^T P F z > 0$。

<!-- p.2 -->

> - **条件 2 与条件 4（Conditions 2 and 4）.** 若第二个条件成立，则对一切非零 $x$ 有
>
> ```math
> x^T (P + A^T A) x = x^T P x + \|Ax\|_2^2 > 0,
> ```
>
> 因此最后一个命题在 $Q = I$ 时成立。若最后一个命题对某个 $Q \succeq 0$ 成立，则对一切非零 $x$ 有
>
> ```math
> x^T (P + A^T Q A) x = x^T P x + x^T A^T Q A x > 0.
> ```
>
> 因此，若 $Ax = 0$ 且 $x \neq 0$，则必有 $x^T P x > 0$。
>
> 接下来证明这四个命题都等价于 KKT 矩阵的非奇异性（nonsingularity）。先设 $x$ 满足 $Ax = 0$、$Px = 0$ 且 $x \neq 0$，则
>
> ```math
> \begin{bmatrix}
> P & A^T \\
> A & 0
> \end{bmatrix}
> \begin{bmatrix}
> x \\
> 0
> \end{bmatrix} = 0,
> ```
>
> 这说明 KKT 矩阵是奇异的（singular）。
>
> 再设 KKT 矩阵奇异，即存在不全为零的 $x, z$ 使得
>
> ```math
> \begin{bmatrix}
> P & A^T \\
> A & 0
> \end{bmatrix}
> \begin{bmatrix}
> x \\
> z
> \end{bmatrix} = 0.
> ```
>
> 这意味着 $Px + A^T z = 0$ 且 $Ax = 0$。用 $x^T$ 左乘第一个等式，得 $x^T P x + x^T A^T z = 0$；利用 $Ax = 0$，它化简为 $x^T P x = 0$，于是（由 $P \succeq 0$）有 $Px = 0$。除非 $x = 0$，否则这与 (a) 矛盾。在这种情况下必有 $z \neq 0$，但那样一来 $A^T z = 0$ 又与 $\operatorname{rank} A = p$ 矛盾。
>
> **(b)** 由 (a) 知 $P + A^T A \succ 0$。因此存在非奇异矩阵 $R \in \mathbb{R}^{n \times n}$ 使得
>
> ```math
> R^T (P + A^T A) R = I.
> ```
>
> 设 $AR = U \Sigma V_1^T$ 为 $AR$ 的奇异值分解（singular value decomposition），其中 $U \in \mathbb{R}^{p \times p}$，$\Sigma = \operatorname{diag}(\sigma_1, \ldots, \sigma_p) \in \mathbb{R}^{p \times p}$，$V_1 \in \mathbb{R}^{n \times p}$。取 $V_2 \in \mathbb{R}^{n \times (n-p)}$ 使得 $V = \begin{bmatrix} V_1 & V_2 \end{bmatrix}$ 为正交矩阵（orthogonal），并定义 $S = \begin{bmatrix} \Sigma & 0 \end{bmatrix} \in \mathbb{R}^{p \times n}$。我们有 $AR = U S V^T$，于是
>
> ```math
> V^T R^T (P + A^T A) R V = V^T R^T P R V + S^T S = I.
> ```
>
> 因此 $V^T R^T P R V = I - S^T S$ 是对角矩阵。把它记作 $\Lambda$：
>
> ```math
> \Lambda = V^T R^T P R V = \operatorname{diag}(1 - \sigma_1^2, \ldots, 1 - \sigma_p^2, 1, \ldots, 1).
> ```

<!-- p.3 -->

> 对 KKT 矩阵施加合同变换（congruence transformation）得
>
> ```math
> \begin{bmatrix}
> V^T R^T & 0 \\
> 0 & U^T
> \end{bmatrix}
> \begin{bmatrix}
> P & A^T \\
> A & 0
> \end{bmatrix}
> \begin{bmatrix}
> RV & 0 \\
> 0 & U
> \end{bmatrix}
> =
> \begin{bmatrix}
> \Lambda & S^T \\
> S & 0
> \end{bmatrix},
> ```
>
> 且 KKT 矩阵的惯性（inertia）等于右边矩阵的惯性。对右边矩阵施加一个置换（permutation），得到一个具有 $n$ 个对角块的分块对角（block diagonal）矩阵：
>
> ```math
> \begin{bmatrix}
> \lambda_i & \sigma_i \\
> \sigma_i & 0
> \end{bmatrix}, \quad i = 1, \ldots, p; \qquad \lambda_i = 1, \quad i = p + 1, \ldots, n.
> ```
>
> 这些 $2 \times 2$ 块的特征值（eigenvalue）为
>
> ```math
> \frac{\lambda_i \pm \sqrt{\lambda_i^2 + 4 \sigma_i^2}}{2},
> ```
>
> 即一正一负。我们得出结论：共有 $p + (n - p) = n$ 个正特征值和 $p$ 个负特征值。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这道题的骨架是 Sylvester 惯性定律（Sylvester's law of inertia，Sylvester, 1852）：可逆合同变换 $M \mapsto S^T M S$ 不改变矩阵正、负、零特征值的个数。你在 Strang 线性代数中见过它的特例——二次型 $x^T M x$ 在可逆代换 $x = Sy$ 下符号结构不变，这正是本题敢用合同变换把 KKT 矩阵化成小方块来「数」特征值的合法性来源。四个等价条件本质上是同一件事的四种说法：$P$ 在零空间 $\mathcal{N}(A)$ 上正定——这与 Strang 教材「约束子空间上的正定性」完全同构；条件 4 取 $Q = I$ 还顺带给出一个实用构造：给半正定的 $P$ 加上 $A^T A$ 就被「扶正」为正定矩阵。为什么要在意非奇异性？因为第 10 章的等式约束牛顿法每一步都要解以 KKT 矩阵为系数的线性系统，解存在唯一当且仅当它非奇异；本文件附加题代码 lp_acent 中的块消元（block elimination）先解 $A H^{-1} A^T w = \cdots$ 再回代，正是「$H \succ 0$ 且 $\operatorname{rank} A = p$」这一本题框架在具体问题里的化身。当某个 $x_i$ 极小、$H$ 变得病态时，条件数（condition number）警告就会出现——附加题题干里那句提醒的理论根源就是这里的线性代数。

### <span style="color:#c0392b;">11.13 自和谐与负熵（Self-concordance and negative entropy）</span>

<span style="color:#2471a3;">**[question]**</span>

**11.13 Self-concordance and negative entropy.**
(a) Show that the negative entropy function $x \log x$ (on $\mathbb{R}_{++}$) is not self-concordant.
(b) Show that for any $t > 0$, $t x \log x - \log x$ is self-concordant (on $\mathbb{R}_{++}$).

<span style="color:#2471a3;">**[solution]**</span>

> **解答（Solution）.**
>
> **(a)** 先考虑 $f(x) = x \log x$，对其有
>
> ```math
> f'(x) = 1 + \log x, \qquad f''(x) = \frac{1}{x}, \qquad f'''(x) = -\frac{1}{x^2}.
> ```
>
> 于是
>
> ```math
> \frac{|f'''(x)|}{f''(x)^{3/2}} = \frac{1/x^2}{1/x^{3/2}} = \frac{1}{\sqrt{x}},
> ```
>
> 它在上方无界（unbounded above，当 $x \to 0^+$ 时）。特别地，自和谐不等式（self-concordance inequality）$|f'''(x)| \le 2 f''(x)^{3/2}$ 在 $x = 1/5$ 处不成立，故 $f$ 不是自和谐的（self-concordant）。
>
> **(b)** 再考虑 $g(x) = t x \log x - \log x$，对其有
>
> ```math
> g'(x) = -\frac{1}{x} + t + t \log x, \qquad g''(x) = \frac{1}{x^2} + \frac{t}{x}, \qquad g'''(x) = -\frac{2}{x^3} - \frac{t}{x^2}.
> ```
>
> 因此
>
> ```math
> \frac{|g'''(x)|}{g''(x)^{3/2}} = \frac{2/x^3 + t/x^2}{\left(1/x^2 + t/x\right)^{3/2}} = \frac{2 + t x}{(1 + t x)^{3/2}}.
> ```

<!-- p.4 -->

> 定义
>
> ```math
> h(a) = \frac{2 + a}{(1 + a)^{3/2}},
> ```
>
> 使得 $h(t x) = \dfrac{|g'''(x)|}{g''(x)^{3/2}}$。我们有 $h(0) = 2$，下面将证明当 $a > 0$ 时 $h'(a) < 0$，即 $h$ 在 $a > 0$ 上单调递减。这就证明了 $h(a) \le h(0) = 2$，从而
>
> ```math
> \frac{|g'''(x)|}{g''(x)^{3/2}} \le 2.
> ```
>
> 我们有
>
> ```math
> h'(a) = \frac{(1+a)^{3/2} - (3/2)(1+a)^{1/2}(2+a)}{(1+a)^3} = \frac{(1+a)^{1/2}\left((1+a) - (3/2)(2+a)\right)}{(1+a)^3} = -\frac{2 + a/2}{(1+a)^{5/2}} < 0,
> ```
>
> 对 $a > 0$ 成立，证毕。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 自和谐（self-concordance）是 Nesterov 与 Nemirovski 在 1994 年提出的概念，目的是为牛顿法建立一套不依赖坐标、对可逆仿射变换不变（affine invariant）的收敛性理论：条件 $|f'''(x)| \le 2 f''(x)^{3/2}$ 恰好让三阶泰勒项被二阶项的 $3/2$ 次幂控制住，使「牛顿减量 $\lambda$ 小 $\Rightarrow$ 已接近最优」这条推理链完全用 $f''$ 自身作尺度。本题演示了两个关键事实。其一，自和谐性只在 $t \ge 1$ 的正数缩放下保持（比较式两边分别按 $t$ 与 $t^{3/2}$ 缩放，$t < 1$ 时可能失效），所以「缩放一个函数」并不是随随便便就能做的操作。其二，(b) 的妙处在于：$x \log x$ 在 $x \to 0^+$ 处的病态被 $-\log x$ 这个对数障碍（log barrier）吸收——$-\log x$ 自身自和谐、且恰在边界附近把比值顶到 $2$，而 $t x \log x$ 的贡献被 $(1 + tx)^{3/2}$ 的增长压制，于是无论 $t > 0$ 多小，$t x \log x - \log x$ 的比值都不超过 $2$。这条「障碍项治愈边界病态」的性质正是中心路径（central path）上的函数 $t f_0 + \varphi$（$\varphi$ 为对数障碍）始终自和谐的缩影，也是第 11 章内点法（interior-point method）多项式复杂性分析站得住的根基。

<!-- p.5 -->

### <span style="color:#c0392b;">附加习题解答（Solutions to additional exercises）</span>

#### <span style="color:#c0392b;">标准形式 LP 的障碍法（Standard form LP barrier method）</span>

<span style="color:#2471a3;">**[question]**</span>

**Standard form LP barrier method.** In the following three exercises, you will implement a barrier method for solving the standard form LP

```math
\begin{array}{ll}
\text{minimize} & c^T x \\
\text{subject to} & Ax = b, \\
& x \succeq 0,
\end{array}
```

with variable $x \in \mathbb{R}^n$, where $A \in \mathbb{R}^{m \times n}$, with $m < n$. Throughout this exercise we will assume that $A$ is full rank, and the sublevel sets $\{x \mid Ax = b,\; x \succeq 0,\; c^T x \le \gamma\}$ are all bounded. (If this is not the case, the centering problem is unbounded below.)

**1. Centering step.** Implement Newton's method for solving the centering problem

```math
\begin{array}{ll}
\text{minimize} & c^T x - \displaystyle\sum_{i=1}^n \log x_i \\
\text{subject to} & Ax = b,
\end{array}
```

with variable $x$, given a strictly feasible starting point $x_0$.
Your code should accept $A$, $b$, $c$, and $x_0$, and return $x^\star$, the primal optimal point, $\nu^\star$, a dual optimal point, and the number of Newton steps executed.
Use the block elimination method to compute the Newton step. (You can also compute the Newton step via the KKT system, and compare the result to the Newton step computed via block elimination. The two steps should be close, but if any $x_i$ is very small, you might get a warning about the condition number of the KKT matrix.)
Plot $\lambda^2/2$ versus iteration $k$, for various problem data and initial points, to verify that your implementation gives asymptotic quadratic convergence. As stopping criterion, you can use $\lambda^2/2 \le 10^{-6}$. Experiment with varying the algorithm parameters $\alpha$ and $\beta$, observing the effect on the total number of Newton steps required, for a fixed problem instance. Check that your computed $x^\star$ and $\nu^\star$ (nearly) satisfy the KKT conditions.
To generate some random problem data (i.e., $A$, $b$, $c$, $x_0$), we recommend the following approach. First, generate $A$ randomly. (You might want to check that it has full rank.) Then generate a random positive vector $x_0$, and take $b = A x_0$. (This ensures that $x_0$ is strictly feasible.) The parameter $c$ can be chosen randomly. To be sure the sublevel sets are bounded, you can add a row to $A$ with all positive elements. If you want to be able to repeat a run with the same problem data, be sure to set the state for the uniform and normal random number generators.
Here are some hints that may be useful.

- We recommend computing $\lambda^2$ using the formula $\lambda^2 = -\Delta x_{\mathrm{nt}}^T \nabla f(x)$. You don't really need $\lambda$ for anything; you can work with $\lambda^2$ instead. (This is important for reasons described below.)

<!-- p.6 -->

- There can be small numerical errors in the Newton step $\Delta x_{\mathrm{nt}}$ that you compute. When $x$ is nearly optimal, the computed value of $\lambda^2$, i.e., $\lambda^2 = -\Delta x_{\mathrm{nt}}^T \nabla f(x)$, can actually be (slightly) negative. If you take the squareroot to get $\lambda$, you'll get a complex number, and you'll never recover. Moreover, your line search will never exit. However, this only happens when $x$ is nearly optimal. So if you exit on the condition $\lambda^2/2 \le 10^{-6}$, everything will be fine, even when the computed value of $\lambda^2$ is negative.
- For the line search, you must first multiply the step size $t$ by $\beta$ until $x + t \Delta x_{\mathrm{nt}}$ is feasible (i.e., strictly positive). If you don't, when you evaluate $f$ you'll be taking the logarithm of negative numbers, and you'll never recover.

**2. LP solver with strictly feasible starting point.** Using the centering code from part (1), implement a barrier method to solve the standard form LP

```math
\begin{array}{ll}
\text{minimize} & c^T x \\
\text{subject to} & Ax = b, \\
& x \succeq 0,
\end{array}
```

with variable $x \in \mathbb{R}^n$, given a strictly feasible starting point $x_0$. Your LP solver should take as argument $A$, $b$, $c$, and $x_0$, and return $x^\star$.
You can terminate your barrier method when the duality gap, as measured by $n/t$, is smaller than $10^{-3}$. (If you make the tolerance much smaller, you might run into some numerical trouble.) Check your LP solver against the solution found by cvx, for several problem instances.
The comments in part (1) on how to generate random data hold here too.
Experiment with the parameter $\mu$ to see the effect on the number of Newton steps per centering step, and the total number of Newton steps required to solve the problem.
Plot the progress of the algorithm, for a problem instance with $n = 500$ and $m = 100$, showing duality gap (on a log scale) on the vertical axis, versus the cumulative total number of Newton steps (on a linear scale) on the horizontal axis.
Your algorithm should return a $2 \times k$ matrix `history`, (where $k$ is the total number of centering steps), whose first row contains the number of Newton steps required for each centering step, and whose second row shows the duality gap at the end of each centering step. In order to get a plot that looks like the ones in the book (e.g., figure 11.4, page 572), you should use the following code:

```
[xx, yy] = stairs(cumsum(history(1,:)),history(2,:));
semilogy(xx,yy);
```

**3. LP solver.**
Using the code from part (2), implement a general standard form LP solver, that takes arguments $A$, $b$, $c$, determines (strict) feasibility, and returns an optimal point if the problem is (strictly) feasible.

<!-- p.7 -->

You will need to implement a phase I method, that determines whether the problem is strictly feasible, and if so, finds a strictly feasible point, which can then be fed to the code from part (2). In fact, you can use the code from part (2) to implement the phase I method.
To find a strictly feasible initial point $x_0$, we solve the phase I problem

```math
\begin{array}{ll}
\text{minimize} & t \\
\text{subject to} & Ax = b \\
& x \succeq (1-t)\mathbf{1}, \\
& t \ge 0,
\end{array}
```

with variables $x$ and $t$. If we can find a feasible $(x, t)$, with $t < 1$, then $x$ is strictly feasible for the original problem. The converse is also true, so the original LP is strictly feasible if and only if $t^\star < 1$, where $t^\star$ is the optimal value of the phase I problem.
We can initialize $x$ and $t$ for the phase I problem with any $x_0$ satisfying $A x_0 = b$, and $t_0 = 2 - \min_i x_{0,i}$. (Here we can assume that $\min_i x_{0,i} \le 0$; otherwise $x_0$ is already a strictly feasible point, and we are done.) You can use a change of variable $z = x + (t-1)\mathbf{1}$ to transform the phase I problem into the form in part (2).
Check your LP solver against cvx on several numerical examples, including both feasible and infeasible instances.

<span style="color:#2471a3;">**[solution]**</span>

> **解答（Solution）.**
>
> **1.** 牛顿步（Newton step）$\Delta x_{\mathrm{nt}}$ 由 KKT 系统（KKT system）定义：
>
> ```math
> \begin{bmatrix}
> H & A^T \\
> A & 0
> \end{bmatrix}
> \begin{bmatrix}
> \Delta x_{\mathrm{nt}} \\
> w
> \end{bmatrix}
> =
> \begin{bmatrix}
> -g \\
> 0
> \end{bmatrix},
> ```
>
> 其中 $H = \operatorname{diag}(1/x_1^2, \ldots, 1/x_n^2)$，$g = c - (1/x_1, \ldots, 1/x_n)$。该 KKT 系统可以用块消元（block elimination）高效求解：先解
>
> ```math
> A H^{-1} A^T w = -A H^{-1} g,
> ```
>
> 再取 $\Delta x_{\mathrm{nt}} = -H^{-1}(A^T w + g)$。KKT 最优性条件（KKT optimality condition）为
>
> ```math
> A^T \nu^\star + c - (1/x_1^\star, \ldots, 1/x_n^\star) = 0.
> ```
>
> 当牛顿法收敛（即 $\Delta x_{\mathrm{nt}} \approx 0$）时，$w$ 就是对偶最优点（dual optimal point）$\nu^\star$。
>
> 下面的函数用牛顿法计算解析中心（analytic center）：
>
> ```matlab
> function [x_star, nu_star, lambda_hist] = lp_acent(A,b,c,x_0)
> % solves problem
> % minimize c'*x - sum(log(x))
> % subject to A*x = b
> % using Newton's method, given strictly feasible starting point x0
> ```

<!-- p.8 -->

> ```matlab
> % input (A, b, c, x_0)
> % returns primal and dual optimal points
> % lambda_hist is a vector showing lambda^2/2 for each newton step
> % returns [], [] if MAXITERS reached, or x_0 not feasible
> % algorithm parameters
> ALPHA = 0.01;
> BETA = 0.5;
> EPSILON = 1e-6;
> MAXITERS = 100;
> if (min(x_0) <= 0) || (norm(A*x_0 - b) > 1e-3) % x0 not feasible
>     fprintf('FAILED');
>     nu_star = []; x_star = []; lambda_hist=[];
>     return;
> end
> m = length(b);
> n = length(x_0);
> x = x_0; lambda_hist = [];
> for iter = 1:MAXITERS
>     H = diag(x.^(-2));
>     g = c - x.^(-1);
>     % lines below compute newton step via whole KKT system
>     % M = [ H A'; A zeros(m,m)];
>     % d = M\[-g; zeros(m,1)];
>     % dx = d(1:n);
>     % w = d(n+1:end);
>     % newton step by elimination method
>     w = (A*diag(x.^2)*A')\(-A*diag(x.^2)*g);
>     dx = -diag(x.^2)*(A'*w + g);
>     lambdasqr = -g'*dx; % dx'*H*dx;
>     lambda_hist = [lambda_hist lambdasqr/2];
>     if lambdasqr/2 <= EPSILON break; end
>     % backtracking line search
>     % first bring the point inside the domain
>     t = 1; while min(x+t*dx) <= 0 t = BETA*t; end
>     % now do backtracking line search
> ```

<!-- p.9 -->

> ```matlab
> while c'*(t*dx)-sum(log(x+t*dx))+sum(log(x))-ALPHA*t*g'*dx > 0
>     t = BETA*t;
> end
> x = x + t*dx;
> end
> if iter == MAXITERS % MAXITERS reached
>     fprintf('ERROR: MAXITERS reached.\n');
>     x_star = []; nu_star = [];
> else
>     x_star = x;
>     nu_star = w;
> end
> ```
>
> 随机数据按题目陈述中给出的方式生成，取 $A \in \mathbb{R}^{100 \times 500}$。下图画出牛顿减量（Newton decrement）$\lambda^2/2$ 随牛顿步数的变化：二次收敛（quadratic convergence）一目了然；两种方法算出的牛顿方向非常接近；对函数返回的点验证了 KKT 最优性条件。
>
> <span style="color:#7f8c8d;">**图（原 PDF 插图）:** $\lambda^2/2$ 随 Newton steps（牛顿步数，横轴 1–11，线性刻度）的变化，纵轴为对数刻度（$10^{-12}$ 至 $10^4$），曲线呈典型的二次收敛下坠形态。</span>
>
> **2.** 下面的函数用障碍法（barrier method）求解该 LP：
>
> ```matlab
> function [x_star, history, gap] = lp_barrier(A,b,c,x_0)
> ```

<!-- p.10 -->

> ```matlab
> % solves standard form LP
> % minimize     c^T x
> % subject to   Ax = b, x >=0;
> % using barrier method, given strictly feasible x0
> % uses function std_form_LP_acent() to carry out centering steps
> % returns:
> %   - primal optimal point x_star
> %   - history, a 2xk matrix that returns number of newton steps
> %     in each centering step (top row) and duality gap (bottom row)
> %     (k is total number of centering steps)
> %   - gap, optimal duality gap
> % barrier method parameters
> T_0 = 1;
> MU = 20;
> EPSILON = 1e-3; % duality gap stopping criterion
> n = length(x_0);
> t = T_0;
> x = x_0;
> history = [];
> while(1)
>     [x_star, nu_star, lambda_hist] = lp_acent(A,b,t*c,x);
>     x = x_star;
>     gap = n/t;
>     history = [history [length(lambda_hist); gap]];
>     if gap < EPSILON break; end
>     t = MU*t;
> end
> ```
>
> 下面的脚本生成测试数据、绘制障碍法的求解进展，并把计算出的解与 cvx 的结果进行对比：
>
> ```matlab
> % script that generates data and tests the functions
> % std_form_LP_acent
> % std_form_LP_barrier
> clear all;
> m = 100;
> n = 500;
> rand('seed',0);
> ```

<!-- p.11 -->

> ```matlab
> randn('seed',0);
> A = [randn(m-1,n); ones(1,n)];
> x_0 = rand(n,1) + 0.1;
> b = A*x_0;
> c = randn(n,1);
> % analytic centering
> figure
> [x_star, nu_star, lambda_hist] = lp_acent(A,b,c,x_0);
> semilogy(lambda_hist,'bo-')
> xlabel('iters')
> ylabel('lambdasqr/2')
> % solve the LP with barrier
> figure
> [x_star, history, gap] = lp_barrier(A,b,c,x_0);
> [xx, yy] = stairs(cumsum(history(1,:)),history(2,:));
> semilogy(xx,yy,'bo-');
> xlabel('iters')
> ylabel('gap')
> p_star = c'*x_star;
> % solve LP using cvx for comparison
> cvx_begin
>     variable x(n)
>     minimize(c'*x)
>     subject to
>         A*x == b
>         x >= 0
> cvx_end
> fprintf('\n\nOptimal value found by barrier method:\n');
> p_star
> fprintf('Optimal value found by CVX:\n');
> cvx_optval
> fprintf('Duality gap from barrier method:\n');
> gap
> ```

<!-- p.12 -->

> <span style="color:#7f8c8d;">**图（原 PDF 插图）:** Duality gap（对偶间隙，纵轴 $10^{-4}$ 至 $10^3$，对数刻度）随累计 Newton steps（牛顿步数，横轴 10–55，线性刻度）的变化，呈分段线性下降的阶梯形态。</span>

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这里的对偶间隙（duality gap）估计是 $n/t$ 而非书里一般形式的 $m/t$，值得亲手算一遍：标准形式 LP 的不等式约束就是 $n$ 个分量 $x_i \ge 0$，由中心化问题 $t c^T x - \sum_i \log x_i$ 的 KKT 条件可以读出一个对偶可行点 $y$（按分量满足 $c - A^T y = 1/(t x^\star)$），于是原始目标与对偶目标之差恰为 $\sum_i x_i^\star \cdot \frac{1}{t x_i^\star} = n/t$——这是互补松弛（complementary slackness）在「每分量障碍权重相同」时的直接推论。若把 $t$ 显式乘在障碍上（书 11.3 节的参数化 $t c^T x + \phi(x)$），同一估计就写成 $m/t$，两套记号实为一回事。从 6.006 的视角看，内点法的历史意义在于把线性规划拉进多项式时间可解类：单纯形法存在指数最坏情形（Klee–Minty 立方体，1972），而 Khachiyan 1979 的椭球法与 Karmarkar 1984 的内点法先后给出了多项式时间算法。本题代码里每次中心化只需个位数到十几次牛顿步、外层用 $\mu = 20$ 做几何加速，总牛顿步数几乎不随问题规模增长——这正是这套理论在数值上的样子。

> **3.** 下面的函数实现了完整的 LP 求解器（Phase I 与 Phase II）：
>
> ```matlab
> function [x_star,p_star,gap,status,nsteps] = lp_solve(A,b,c);
> % solves the LP
> % minimize     c^T x
> % subject to   Ax = b, x >= 0;
> % using a barrier method
> % computes a strictly feasible point by carrying out
> % a phase I method
> % returns:
> %   - a primal optimal point x_star
> %   - the primal optimal value p_star
> %   - status: either 'Infeasible' or 'Solved'
> %   - nsteps(1): number of newton steps for phase I
> %   - nsteps(2): number of newton steps for phase I
> [m,n] = size(A);
> nsteps = zeros(2,1);
> % phase I
> x0 = A\b; t0 = 2+max(0,-min(x0));
> A1 = [A,-A*ones(n,1)];
> b1 = b-A*ones(n,1);
> ```

<!-- p.13 -->

> ```matlab
> z0 = x0+t0*ones(n,1)-ones(n,1);
> c1 = [zeros(n,1);1];
> [z_star, history, gap] = lp_barrier(A1,b1,c1,[z0;t0]);
> if (z_star(n+1) >= 1)
>     fprintf('\nProblem is infeasible\n');
>     x_star = []; p_star = Inf; status = 'Infeasible';
>     nsteps(1) = sum(history(1,:)); gap = [];
>     return;
> end
> fprintf('\nFeasible point found\n');
> nsteps(1) = sum(history(1,:));
> x_0 = z_star(1:n)-z_star(n+1)*ones(n,1)+ones(n,1);
> % phase II
> [x_star, history, gap] = lp_barrier(A,b,c,x_0);
> status = 'Solved'; p_star = c'*x_star;
> nsteps(2) = sum(history(1,:));
> ```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> Phase I 方法解决的是一切迭代法的「先有鸡还是先有蛋」难题：牛顿法与障碍法都需要一个严格可行初始点，而可行点本身可能很难找。这里的技巧与你在 6.006 最大流（max-flow）问题中学到的超级源点（super-source）同构：那里为了给算法一个初始可行流，虚构一个源点向所有节点供流；这里为了把「找可行点」变成可优化的问题，引入标量 $t$ 并最小化它，用 $x \succeq (1-t)\mathbf{1}$ 允许每个 $x_i$ 暂时「欠账」不超过 $t$。判据 $t^\star < 1$ 当且仅当原问题严格可行，而 $t^\star \ge 1$ 时 Phase I 的对偶解还能充当原问题不可行的证书（certificate）——这与 LP 对偶理论中「不可行性由对偶射线证明」的思想完全一致。变量替换 $z = x + (t-1)\mathbf{1}$ 把 Phase I 问题化归为第 2 问已实现的标准形式，于是 Phase I 与 Phase II 共用同一个 lp_barrier 函数；代码里 `A1 = [A,-A*ones(n,1)]`、`b1 = b-A*ones(n,1)` 两行正是这个替换的矩阵化表达。

> 我们在两个问题实例上测试该 LP 求解器（一个不可行、一个可行），并把结果与 cvx 的输出对比：
>
> ```matlab
> % solves standard form LP for two problem instances
> clear all;
> m = 100;
> n = 500;
> % infeasible problem instance
> rand('seed',0);
> randn('seed',0);
> A = [rand(m-1,n); ones(1,n)];
> b = randn(m,1);
> c = randn(n,1);
> [x_star,p_star,gap,status,nsteps] = lp_solve(A,b,c);
> % solve LP using cvx for comparison
> cvx_begin
>     variable x(n)
>     minimize(c'*x)
>     subject to
>         A*x == b
>         x >= 0
> cvx_end
> ```

<!-- p.14 -->

> ```matlab
> % feasible problem instance
> A = [randn(m-1,n); ones(1,n)];
> v = rand(n,1) + 0.1;
> b = A*v;
> c = randn(n,1);
> [x_star,p_star,gap,status,nsteps] = lp_solve(A,b,c);
> % solve LP using cvx for comparison
> cvx_begin
>     variable x(n)
>     minimize(c'*x)
>     subject to
>         A*x == b
>         x >= 0
> cvx_end
> fprintf('\n\nOptimal value found by barrier method:\n');
> p_star
> fprintf('Optimal value found by CVX:\n');
> cvx_optval
> fprintf('Duality gap from barrier method:\n');
> gap
> ```
