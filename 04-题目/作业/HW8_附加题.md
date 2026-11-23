# EE364a HW8_bonus 解答（作业 8 附加题 解答）

> 本作业为 Stanford EE364a《Convex Optimization》标准形式线性规划（standard form LP）的**障碍方法（barrier method）**编程练习，分三部分：①牛顿定心（centering）步；②给定严格可行初始点的 LP 求解器；③带阶段 I（phase I）的通用 LP 求解器。

---

## 题目 1（Problem 1）：定心步 Centering step

**题干：**

Implement Newton's method for solving the centering problem

```math
\begin{aligned}
\operatorname{minimize} \quad & c^T x - \sum_{i=1}^{n} \log x_i \\
\operatorname{subject\;to} \quad & Ax = b,
\end{aligned}
```

with variable $x$, given a strictly feasible starting point $x_0$. Your code should accept $A, b, c,$ and $x_0$, and return $x^{\star}$ (the primal optimal point), $\nu^{\star}$ (a dual optimal point), and the number of Newton steps executed. Use the block elimination method to compute the Newton step. Plot $\lambda^2/2$ versus iteration $k$ to verify asymptotic quadratic convergence, using the stopping criterion $\lambda^2/2 \le 10^{-6}$.

**中文：**

用牛顿法（Newton's method）求解如下的定心问题（centering problem），变量为 $x$，给定严格可行（strictly feasible）的初始点 $x_0$。你的代码应接收 $A, b, c, x_0$，并返回原始最优解（primal optimal point）$x^{\star}$、一个对偶最优解（dual optimal point）$\nu^{\star}$，以及执行的牛顿步数（number of Newton steps）。用分块消元法（block elimination method）计算牛顿步（Newton step）。画出 $\lambda^2/2$ 对迭代次数 $k$ 的曲线，验证渐近二次收敛（asymptotic quadratic convergence），停止准则（stopping criterion）取 $\lambda^2/2 \le 10^{-6}$。

---

**解答：**

**第一步：写出问题结构。** 定心问题的目标函数是

```math
f_0(x) = c^T x - \sum_{i=1}^{n} \log x_i,
```

其中 $-\log x_i$ 是对非负约束 $x_i \ge 0$ 的对数障碍（logarithmic barrier）。变量是 $x\in\mathbb{R}^{n}$，等式约束（equality constraint）为 $Ax = b$，其中 $A\in\mathbb{R}^{m\times n}$ 且 $m < n$，并假定 $A$ 行满秩（full rank）。定义域（domain）为 $\operatorname{dom} f_0 = \{x \mid x \succ 0\}$（严格正）。

**第二步：计算梯度与 Hessian。** 目标函数逐项求导：

```math
\nabla f_0(x) = c - \begin{bmatrix} 1/x_1 \\ \vdots \\ 1/x_n \end{bmatrix}
= c - \operatorname{diag}(x)^{-1}\mathbf{1},
```

其中 $\mathbf{1}$ 是全 1 向量。记 $d_i = 1/x_i$，则 $\nabla f_0(x) = c - d$，$d = (1/x_1,\dots,1/x_n)$。

对梯度再求导，得 Hessian（注意 $\frac{\partial}{\partial x_j}(1/x_i)=0$ 当 $i\ne j$，$\frac{\partial}{\partial x_i}(1/x_i) = -1/x_i^2$）：

```math
\nabla^2 f_0(x) = \operatorname{diag}\!\big(1/x_1^2,\dots,1/x_n^2\big) = \operatorname{diag}(x)^{-2}.
```

这是一个对角（diagonal）且**正定（positive definite）**的矩阵（因为 $x_i>0$），故 $f_0$ 在定义域内严格凸。

**第三步：写出 Newton 步所满足的 KKT 系统。** 对于等式约束优化 $\min f_0(x)$ s.t. $Ax=b$，Newton 步 $\Delta x_{\mathrm{nt}}$ 与对偶变量增量 $\Delta\nu$ 满足如下 KKT 系统（对 $f_0$ 在 $x$ 处做二阶近似后的一阶最优条件）：

```math
\begin{bmatrix} H & A^T \\ A & 0 \end{bmatrix}
\begin{bmatrix} \Delta x_{\mathrm{nt}} \\ w \end{bmatrix}
=
\begin{bmatrix} -\nabla f_0(x) \\ 0 \end{bmatrix},
```

其中 $H = \nabla^2 f_0(x) = \operatorname{diag}(x)^{-2}$，$w$ 是新的对偶变量（与 $\nu$ 相关）。

**第四步：分块消元法（block elimination）求解。** 由于 $H$ 是对角且正定，先从上块解出：

```math
\Delta x_{\mathrm{nt}} = -H^{-1}\big(\nabla f_0(x) + A^T w\big).
```

把它代入下块 $A\Delta x_{\mathrm{nt}}=0$：

```math
-A H^{-1}\big(\nabla f_0(x) + A^T w\big) = 0
\;\Longrightarrow\;
A H^{-1} A^T \, w = - A H^{-1} \nabla f_0(x).
```

这里 $A H^{-1} A^T$ 是 $m\times m$ 的对称矩阵；因为 $A$ 行满秩且 $H\succ 0$，它是**正定**的，可逆。于是：

```math
w = -\big(A H^{-1} A^T\big)^{-1} A H^{-1} \nabla f_0(x),
```

再回代得到 Newton 步：

```math
\Delta x_{\mathrm{nt}} = -H^{-1}\nabla f_0(x) - H^{-1} A^T w.
```

由于 $H^{-1}=\operatorname{diag}(x)^2$ 是对角阵，$A H^{-1} A^T$ 的每个元素是 $\sum_{k} A_{ik}A_{jk}x_k^2$，可直接构造。这正是**分块消元法**：先消 $\Delta x$ 求小规模 $m\times m$ 系统，再回代。

**第五步：牛顿减量（Newton decrement）的简化计算。** 牛顿减量 $\lambda$ 定义为

```math
\lambda^2 = \Delta x_{\mathrm{nt}}^T H \Delta x_{\mathrm{nt}}.
```

利用 KKT 系统第一行 $\nabla f_0(x)+A^T w = -H\Delta x_{\mathrm{nt}}$，且 $\Delta x_{\mathrm{nt}}$ 满足 $A\Delta x_{\mathrm{nt}}=0$，有：

```math
\lambda^2 = \Delta x_{\mathrm{nt}}^T H \Delta x_{\mathrm{nt}}
= -\Delta x_{\mathrm{nt}}^T \nabla f_0(x) - \Delta x_{\mathrm{nt}}^T A^T w
= -\Delta x_{\mathrm{nt}}^T \nabla f_0(x) - (A\Delta x_{\mathrm{nt}})^T w
= -\Delta x_{\mathrm{nt}}^T \nabla f_0(x).
```

所以可用 $\lambda^2 = -\Delta x_{\mathrm{nt}}^T \nabla f_0(x)$ 直接计算，**无需**求 $\lambda$ 本身（避免开平方，见题目提示：近最优点处数值误差可能使 $\lambda^2$ 略为负，开方会得复数）。

**第六步：回溯线搜索（backtracking line search）。** 参数 $\alpha\in(0,0.5)$、$\beta\in(0,1)$。先令 $t=1$，然后只要 $x+t\Delta x_{\mathrm{nt}}$ 不可行（即存在 $x_i + t\Delta x_{i}\le 0$），就令 $t\leftarrow \beta t$，直到严格正。**必须先做可行性收缩**，否则 $\log$ 会取到非正数。之后再按 Armijo 准则收缩：

```math
f_0(x+t\Delta x_{\mathrm{nt}}) > f_0(x) + \alpha\, t\, \nabla f_0(x)^T \Delta x_{\mathrm{nt}}
```

时令 $t\leftarrow \beta t$。更新 $x \leftarrow x + t\Delta x_{\mathrm{nt}}$。

**第七步：停止准则与收敛。** 当 $\lambda^2/2 \le 10^{-6}$ 时停止。返回 $x^{\star}=x$。对偶变量 $\nu^{\star}$ 由最优条件 $\nabla f_0(x^{\star}) + A^T\nu^{\star}=0$ 给出，即：

```math
\nu^{\star} = -\big(A H^{-1} A^T\big)^{-1} A H^{-1} \nabla f_0(x^{\star})
= w \;\text{（在收敛点的 } w）.
```

**关键判据（KKT 条件）——必须显式写出。** 定心问题的最优性条件是：

```math
\nabla f_0(x^{\star}) + A^T \nu^{\star} = 0, \qquad A x^{\star} = b, \qquad x^{\star} \succ 0.
```

收敛后应验证 $x^{\star}\succ 0$、$Ax^{\star}=b$ 近似成立，且 $\nabla f_0(x^{\star})+A^T\nu^{\star}\approx 0$。因为 Hessian 正定、等式约束线性（满足 Slater 条件），这是全局最优的充要条件。

**收敛性验证。** 牛顿法在 $H\succ 0$ 下**局部二次收敛**：存在常数 $\gamma$ 使 $\lambda(x_{k+1}) \le \big(\frac{\gamma}{2(1-2\alpha)}\big)\lambda(x_k)^2$，故 $\log(\lambda^2/2)$ 对 $k$ 的曲线应呈二次下降（斜率逐渐变陡）。实验中调整 $\alpha,\beta$：$\alpha$ 越大、$\beta$ 越小，线搜索更保守，单步迭代更多但总步数可能更少，需在固定实例上观察。

[[FIGURE: Page 1, 牛顿减量 $\lambda^2/2$ 对迭代次数 $k$ 的收敛曲线（log 尺度，呈二次收敛）]]

---

## 题目 2（Problem 2）：给定严格可行初始点的 LP 求解器

**题干：**

Using the centering code from part (1), implement a barrier method to solve the standard form LP

```math
\begin{aligned}
\operatorname{minimize} \quad & c^T x \\
\operatorname{subject\;to} \quad & Ax = b, \\
& x \succeq 0,
\end{aligned}
```

given a strictly feasible starting point $x_0$. Return $x^{\star}$. Terminate when the duality gap $n/t$ is smaller than $10^{-3}$. Return a $2\times k$ matrix `history` whose first row contains the number of Newton steps per centering step, and whose second row shows the duality gap at the end of each centering step.

**中文：**

用第 (1) 部分的定心代码，实现障碍方法（barrier method）求解上述标准形式 LP，给定严格可行初始点 $x_0$，返回 $x^{\star}$。当对偶间隙（duality gap）$n/t$ 小于 $10^{-3}$ 时终止。返回一个 $2\times k$ 矩阵 `history`：第一行是每次定心步所需的牛顿步数，第二行是每次定心步结束时的对偶间隙。

---

**解答：**

**第一步：障碍问题族。** 标准形式 LP 的障碍方法引入参数 $t>0$，求解一族定心问题：

```math
\operatorname{minimize} \quad t\, c^T x - \sum_{i=1}^{n}\log x_i \quad \operatorname{subject\;to} \quad Ax = b.
```

其目标等价于第 (1) 部分中取 $c \leftarrow t\,c$。记最优解为 $x^{\star}(t)$，称为**中心路径（central path）**上的点。

**第二步：对偶间隙与次优性。** 由第 (1) 部分的 KKT 条件 $\nabla f_0 + A^T\nu = t c - d + A^T\nu = 0$，即

```math
d = t c + A^T \nu, \qquad d_i = 1/x_i.
```

构造对偶可行解：令 $\tilde\nu = \nu/t$，则 $A^T\tilde\nu = \frac{1}{t}d - c = \operatorname{diag}(x)^{-1}\mathbf{1}/t - c$。定义 $\tilde s = \frac{1}{t}\operatorname{diag}(x)^{-1}\mathbf{1}$，则 $A^T\tilde\nu + \tilde s = c$ 且 $\tilde s \succ 0$，故 $(\tilde\nu, \tilde s)$ 是**对偶可行**的。对偶目标为 $b^T\tilde\nu$。

**关键判据——对偶间隙（duality gap）。** 原始目标 $c^T x$ 与对偶目标 $b^T\tilde\nu$ 的差：

```math
c^T x - b^T\tilde\nu
= (A^T\tilde\nu + \tilde s)^T x - (Ax)^T\tilde\nu
= \tilde s^T x
= \sum_{i=1}^{n} \tilde s_i x_i
= \sum_{i=1}^{n}\frac{1}{t} = \frac{n}{t}.
```

因此对偶间隙恰好是 $n/t$。当 $n/t \le 10^{-3}$ 时，$x$ 是 $\epsilon$-次优解（$\epsilon = 10^{-3}$），停止。

**第三步：障碍方法主循环。** 从 $x=x_0$、$t=t^{(0)}$（如 $t^{(0)}=1$）开始，反复执行：

1. 用第 (1) 部分牛顿法求 $\operatorname{minimize}\, t c^T x - \sum_i\log x_i$ 的最优解（**中心步**），记录本次牛顿步数；
2. 计算对偶间隙 $n/t$ 并记录；
3. 更新 $t \leftarrow \mu t$（参数 $\mu>1$，通常取 $\mu\approx 10$–$20$）。

**第四步：终止条件。** 当 $n/t \le 10^{-3}$ 停止，返回 $x^{\star}=x$。若容差取得过小（如 $10^{-6}$ 以下），$t$ 过大导致 $H=\operatorname{diag}(x)^{-2}$ 条件数变差，可能产生数值困难。

**第五步：`history` 矩阵。** 设共 $k$ 次定心步：

```math
\verb|history| = \begin{bmatrix} n_1 & n_2 & \cdots & n_k \\ \eta_1 & \eta_2 & \cdots & \eta_k \end{bmatrix},
```

其中 $n_j$ 是第 $j$ 次定心步的牛顿步数，$\eta_j$ 是该步结束时的对偶间隙 $n/t^{(j)}$。

**第六步：绘图。** 按书中图 11.4（p.572）风格，横轴为累计牛顿步数，纵轴为对偶间隙（log 尺度），用 MATLAB 代码：

```matlab
[xx, yy] = stairs(cumsum(history(1,:)), history(2,:));
semilogy(xx, yy);
```

`cumsum` 得到累计牛顿步数（横轴），`stairs` 使间隙呈阶梯状（每次定心步内保持不变，结束时跳变）。

**实验观察。** 增大 $\mu$ 会减少定心步数但每次定心步需要更多牛顿步；总牛顿步数通常在某中间 $\mu$ 处最优。对 $n=500, m=100$ 的实例绘图验证整体进度。

[[FIGURE: Page 572, 对偶间隙（log 尺度）对累计牛顿步数的阶梯下降曲线]]

---

## 题目 3（Problem 3）：通用 LP 求解器（含阶段 I）

**题干：**

Using the code from part (2), implement a general standard form LP solver, that takes arguments $A, b, c$, determines (strict) feasibility, and returns an optimal point if the problem is (strictly) feasible. Implement a phase I method that determines whether the problem is strictly feasible, and if so, finds a strictly feasible point to feed to part (2).

**中文：**

用第 (2) 部分代码实现一个通用的标准形式 LP 求解器，输入 $A, b, c$，判断（严格）可行性（feasibility），若问题（严格）可行则返回一个最优点。实现阶段 I 方法（phase I method）判断问题是否严格可行，若可行则找到一个严格可行点，再交给第 (2) 部分。

---

**解答：**

**第一步：阶段 I 问题的构造。** 为找到严格可行初始点，求解如下阶段 I 问题：

```math
\begin{aligned}
\operatorname{minimize} \quad & t \\
\operatorname{subject\;to} \quad & Ax = b, \\
& x \succeq (1-t)\mathbf{1}, \\
& t \ge 0,
\end{aligned}
```

变量为 $x$ 和 $t$。**关键判据：** 若存在可行解满足 $t<1$，则 $x\succeq(1-t)\mathbf{1}\succ 0$，故 $x$ 是原问题的严格可行点。反之，若 $x$ 严格可行，取 $t=0$ 即得 $x\succeq\mathbf{1}$？——需修正：严格可行只保证 $x\succ 0$，不一定 $x\succeq\mathbf{1}$。但可取 $t=1-\min_i x_i$（因 $\min_i x_i>0$，故 $t<1$），此时 $x_i \ge \min_i x_i = 1-t$ 对每个 $i$ 成立，即 $x\succeq(1-t)\mathbf{1}$。因此**原问题严格可行当且仅当 $t^{\star}<1$**，其中 $t^{\star}$ 是阶段 I 问题的最优值。

**第二步：阶段 I 的初始化。** 取任意满足 $Ax_0=b$ 的 $x_0$（例如 $x_0 = A^T(AA^T)^{-1}b$，即最小范数解），令

```math
t_0 = 2 - \min_i x_{0,i}.
```

（这里可假设 $\min_i x_{0,i}\le 0$，否则 $x_0$ 本身已严格可行，直接结束阶段 I。）这样选取保证初始点严格可行：由 $t_0 \ge 2 - \min_i x_{0,i} > 1-\min_i x_{0,i}$，得 $\min_i x_{0,i} > 1 - t_0$，即 $x_0 \succ (1-t_0)\mathbf{1}$，且 $t_0\ge 2>0$。

**第三步：变量代换化为第 (2) 部分形式。** 令 $z = x + (t-1)\mathbf{1}$，即 $x = z - (t-1)\mathbf{1}$。约束 $x \succeq (1-t)\mathbf{1}$ 变为：

```math
z - (t-1)\mathbf{1} \succeq (1-t)\mathbf{1}
\;\Longleftrightarrow\; z \succeq 0.
```

等式约束变为：

```math
A\big(z - (t-1)\mathbf{1}\big) = b
\;\Longleftrightarrow\; A z = b + (t-1) A\mathbf{1}.
```

记 $s = t-1$，则变量为 $(z,s)$，$z\succeq 0$、$s\ge -1$（即 $t\ge 0 \Leftrightarrow s\ge -1$），约束写成标准形：

```math
\begin{bmatrix} A & -A\mathbf{1} \end{bmatrix}
\begin{bmatrix} z \\ s \end{bmatrix}
= b - A\mathbf{1}, \qquad z\succeq 0,\; s\ge -1.
```

把 $s\ge -1$ 再平移（例如令 $\bar s = s+1 \ge 0$）可进一步吸收进非负变量。目标 $\min t = \min(s+1)$，等价于 $\min s$（线性目标）。这样就得到第 (2) 部分能处理的标准形式，可调用其障碍方法求解。

**第四步：主算法。** 完整流程：

1. 求任意 $x_0$ 满足 $Ax_0=b$；若 $\min_i x_{0,i}>0$，则 $x_0$ 已严格可行，跳到第 3 步。
2. 否则解阶段 I 问题；若 $t^{\star}\ge 1$，判定**原问题不可行（或非严格可行）**，返回不可行标志。
3. 若 $t^{\star}<1$，取严格可行点 $x = z^{\star}-(t^{\star}-1)\mathbf{1}$，作为第 (2) 部分求解器的初始点，解得原 LP 最优解 $x^{\star}$。

**为什么阶段 I 也能用第 (2) 部分代码：** 阶段 I 问题本身是标准形式 LP（目标线性、等式约束、非负变量），且我们已构造出它的严格可行初始点（第三步的 $(z_0,s_0)$ 由 $x_0,t_0$ 平移得到且满足 $z_0\succ 0$），故可直接复用同一障碍方法求解器。

**验证。** 对若干数值实例（含可行与不可行）用 cvx 对照检查：可行实例返回的 $x^{\star}$ 应与 cvx 结果一致；不可行实例应正确报告不可行而非返回错误解。

[[FIGURE: 无图（本部分为算法流程描述）]]
