> <span style="color:#7f8c8d;">Source: SEE官方讲义/11EqualityMin.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

## <span style="color:#2471a3;">**[lecture]**</span> 11. Equality constrained minimization（等式约束最小化）

<!-- p.1 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**第 11 讲总览**</span>

本讲内容（outline）：

- 等式约束最小化（equality constrained minimization）
- 消去等式约束（eliminating equality constraints）
- 带等式约束的牛顿法（Newton's method with equality constraints）
- 不可行初始点牛顿法（infeasible start Newton method）
- 数值实现（implementation）

<!-- p.2 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**等式约束最小化问题（Equality constrained minimization）**</span>

<span style="color:#2471a3;">**[definition]**</span>
<span style="color:#00838f;">**等式约束最小化问题**：目标函数为 $f$、约束为线性等式组 $Ax=b$ 的优化问题（optimization problem）。</span>

```math
\begin{array}{ll}
\text{minimize} & f(x) \\
\text{subject to} & Ax = b
\end{array}
```

- $f$ 为凸函数（convex），且二阶连续可微（twice continuously differentiable）
- $A \in \mathbb{R}^{p\times n}$，且 $\operatorname{rank} A = p$（行满秩）
- 假设最优值（optimal value）$p^\star$ 有限且可达（attained）

<span style="color:#2471a3;">**[theorem]**</span> <span style="color:#c0392b;">**最优性条件（optimality conditions）**</span>：$x^\star$ 是最优解，当且仅当存在 $\nu^\star$ 使得

```math
\nabla f(x^\star) + A^T\nu^\star = 0, \qquad Ax^\star = b
```

<!-- p.3 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**等式约束二次最小化与 KKT 矩阵**</span>

取 $P \in S^n_{+}$（半正定）时的等式约束二次规划（quadratic program, QP）：

```math
\begin{array}{ll}
\text{minimize} & (1/2)x^TPx + q^Tx + r \\
\text{subject to} & Ax = b
\end{array}
```

最优性条件（optimality condition）：

```math
\begin{bmatrix} P & A^T \\ A & 0 \end{bmatrix}
\begin{bmatrix} x^\star \\ \nu^\star \end{bmatrix}
=
\begin{bmatrix} -q \\ b \end{bmatrix}
```

- <span style="color:#00838f;">此处的系数矩阵称为 **KKT 矩阵（KKT matrix）**。</span>
- KKT 矩阵非奇异（nonsingular）当且仅当

```math
Ax = 0,\; x \neq 0 \implies x^TPx > 0
```

- 非奇异性的等价条件：$P + A^TA \succ 0$

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这个分块矩阵在数值线性代数里被称为鞍点系统（saddle-point system）：左上块 $P$ 半正定、右下块是 $0$，整个矩阵必然不定，它的可逆性完全取决于 $P$ 限制在 $A$ 的零空间上是否正定——这正是上一行条件的含义。用 Strang 四基本定理的语言说：$A$ 行满秩时零空间维数为 $n-p$，该条件要求二次型在这块低维子空间上没有非零的"平坦方向"；至于零空间之外的正性则无关紧要，因为可行位移只能落在零空间里。同时，第一行方程 $\nabla f(x^\star)+A^T\nu^\star=0$ 恰是 05 讲拉格朗日函数（Lagrangian）$L(x,\nu)=f(x)+\nu^T(Ax-b)$ 的平稳条件，$\nu^\star$ 就是对偶变量（Lagrange multiplier）：解一个 KKT 系统等于同时求出原、对偶两组变量。等价条件 $P+A^TA\succ 0$ 之所以成立，是因为 $x^T(P+A^TA)x=x^TPx+\|Ax\|_2^2$，把仅在零空间上成立的正定性"抬升"为全空间正定性——这也是 18.065 中正则化思想的一个雏形。

<!-- p.4 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**消去等式约束（Eliminating equality constraints）**</span>

把解集 $\{x \mid Ax = b\}$ 表示为

```math
\{x \mid Ax = b\} = \{\, Fz + \hat{x} \mid z \in \mathbb{R}^{n-p} \,\}
```

- $\hat{x}$ 是（任意）一个特解（particular solution）
- $F \in \mathbb{R}^{n\times(n-p)}$ 的列空间（range）等于 $A$ 的零空间（nullspace），即 $\operatorname{rank} F = n-p$ 且 $AF = 0$

由此得到约化/消去问题（reduced or eliminated problem）：

```math
\text{minimize } f(Fz + \hat{x})
```

- 这是一个以 $z \in \mathbb{R}^{n-p}$ 为变量的无约束问题（unconstrained problem）
- 由其解 $z^\star$ 可复原 $x^\star$ 与对偶变量 $\nu^\star$：

```math
x^\star = Fz^\star + \hat{x}, \qquad \nu^\star = -(AA^T)^{-1}A\nabla f(x^\star)
```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这一表示就是 Strang 解 $Ax=b$ 的标准套路——"特解 + 零空间齐次解"——的优化版：任取一个特解 $\hat{x}$，再让 $F$ 的列撑起零空间（四基本定理中维数为 $n-p$ 的那块拼图），任何可行点都能写成 $\hat{x}+Fz$，优化自由度随之从 $n$ 压缩到 $n-p$。链式法则给出约化问题的梯度 $F^T\nabla f$ 与 Hessian $F^T\nabla^2 f\,F$，因此 11–9 页会证明：沿 $F$ 消元后做牛顿法与带等式约束的牛顿法逐点等价，两种视角可以自由切换。对偶变量也能反解出来：把平稳条件 $\nabla f+A^T\nu=0$ 左乘 $A$，利用行满秩保证 $AA^T$ 可逆（它是行向量组的 Gram 矩阵，Strang 教材中的经典结论），立即得到 $\nu^\star=-(AA^T)^{-1}A\nabla f(x^\star)$。数值实现上，$F$ 通常由 QR 分解或 SVD 提取标准正交零空间基（18.065 强调的稳定做法），而不是显式解方程求零空间。

<!-- p.5 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**例：资源约束下的最优分配**</span>

<span style="color:#2471a3;">**[example]**</span>

```math
\begin{array}{ll}
\text{minimize} & f_1(x_1) + f_2(x_2) + \cdots + f_n(x_n) \\
\text{subject to} & x_1 + x_2 + \cdots + x_n = b
\end{array}
```

消去 $x_n = b - x_1 - \cdots - x_{n-1}$，即取

```math
\hat{x} = be_n, \qquad F = \begin{bmatrix} I \\ -1^T \end{bmatrix} \in \mathbb{R}^{n\times(n-1)}
```

约化问题（reduced problem）：

```math
\text{minimize } f_1(x_1) + \cdots + f_{n-1}(x_{n-1}) + f_n(b - x_1 - \cdots - x_{n-1})
```

（变量为 $x_1, \ldots, x_{n-1}$。）

<!-- p.6 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**牛顿步（Newton step）**</span>

$f$ 在可行点 $x$ 处的牛顿步（Newton step），由下述 KKT 系统（KKT system）之解的第一个分块给出：

```math
\begin{bmatrix} \nabla^2 f(x) & A^T \\ A & 0 \end{bmatrix}
\begin{bmatrix} \Delta x_{\mathrm{nt}} \\ w \end{bmatrix}
=
\begin{bmatrix} -\nabla f(x) \\ 0 \end{bmatrix}
```

两种解释（interpretations）：

- $\Delta x_{\mathrm{nt}}$ 是二阶近似问题（变量取 $v$）的解：

```math
\begin{array}{ll}
\text{minimize} & \hat{f}(x+v) = f(x) + \nabla f(x)^Tv + (1/2)v^T\nabla^2 f(x)\,v \\
\text{subject to} & A(x+v) = b
\end{array}
```

- 该方程组也可由对最优性条件线性化（linearizing）得到：

```math
\nabla f(x + \Delta x_{\mathrm{nt}}) + A^Tw = 0, \qquad A(x + \Delta x_{\mathrm{nt}}) = b
```

<!-- p.7 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**牛顿减量（Newton decrement）**</span>

<span style="color:#2471a3;">**[definition]**</span>
<span style="color:#00838f;">**牛顿减量（Newton decrement）**定义如下：</span>

```math
\lambda(x) = \left(\Delta x_{\mathrm{nt}}^T \nabla^2 f(x)\, \Delta x_{\mathrm{nt}}\right)^{1/2} = \left(-\nabla f(x)^T \Delta x_{\mathrm{nt}}\right)^{1/2}
```

性质（properties）：

- 借助二次近似 $\hat{f}$ 估计 $f(x) - p^\star$：

```math
f(x) - \inf_{Ay=b} \hat{f}(y) = (1/2)\lambda(x)^2
```

- 沿牛顿方向的方向导数（directional derivative）：

```math
\left. \frac{d}{dt} f(x + t\Delta x_{\mathrm{nt}}) \right|_{t=0} = -\lambda(x)^2
```

- 一般地，$\lambda(x) \neq \left(\nabla f(x)^T \nabla^2 f(x)^{-1} \nabla f(x)\right)^{1/2}$

<!-- p.8 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**带等式约束的牛顿法（Newton's method with equality constraints）**</span>

<span style="color:#2471a3;">**[algorithm]**</span>
给定起点 $x \in \operatorname{dom} f$（满足 $Ax = b$）与容许误差（tolerance）$\epsilon > 0$。

**重复（repeat）：**

1. 计算牛顿步与牛顿减量 $\Delta x_{\mathrm{nt}},\ \lambda(x)$。
2. **停止准则（stopping criterion）**：若 $\lambda^2/2 \le \epsilon$，退出。
3. **线搜索（line search）**：用回溯线搜索（backtracking line search）选取步长 $t$。
4. **更新（update）**：$x := x + t\Delta x_{\mathrm{nt}}$。

- 这是一个**可行下降方法**（feasible descent method）：$x^{(k)}$ 始终可行，且 $f(x^{(k+1)}) < f(x^{(k)})$
- 具有**仿射不变性**（affine invariant）

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 读牛顿减量要抓住两点。其一，$\lambda(x)^2/2$ 度量的是"当前点到约束下二次模型最优值的距离"：$f(x)-\inf_{Ay=b}\hat{f}(y)=(1/2)\lambda^2$，而那个 $\inf$ 正是 11–6 页二阶近似问题的最优值，所以它是 $f(x)-p^\star$ 的局部估计，停止准则 $\lambda^2/2\le\epsilon$ 才显得有理有据。其二，最后一条强调 $\lambda(x)$ 一般不等于无约束版本 $(\nabla f^T\nabla^2f^{-1}\nabla f)^{1/2}$：因为 $\Delta x_{\mathrm{nt}}$ 被迫满足 $A\Delta x_{\mathrm{nt}}=0$，方向只能落在零空间里，减量只统计了"可行方向上的曲率"。仿射不变性可用 Strang 的换基语言理解：对可逆仿射替换 $x\mapsto Tx+c$，KKT 系统整体作相合变换，迭代序列在本质上是同一个，这与梯度法依赖坐标缩放形成鲜明对比。再加上回溯线搜索利用 $t$ 很小时斜率为 $-\lambda^2<0$ 保证初始下降，最终换来二次收敛。

<!-- p.9 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**牛顿法与消元（Newton's method and elimination）**</span>

对约化问题施行牛顿法：

```math
\text{minimize } \tilde{f}(z) = f(Fz + \hat{x})
```

- 变量为 $z \in \mathbb{R}^{n-p}$
- $\hat{x}$ 满足 $A\hat{x} = b$；$\operatorname{rank} F = n-p$ 且 $AF = 0$
- 对 $\tilde{f}$ 施行牛顿法，从 $z^{(0)}$ 出发，生成迭代点列 $z^{(k)}$

**带等式约束的牛顿法**：当从 $x^{(0)} = Fz^{(0)} + \hat{x}$ 出发时，迭代点满足

```math
x^{(k+1)} = Fz^{(k)} + \hat{x}
```

> <span style="color:#7f8c8d;">原文如此。由初始重合 $x^{(0)} = Fz^{(0)}+\hat{x}$ 逐层归纳，两侧下标一致应为 $x^{(k)} = Fz^{(k)} + \hat{x}$；本页结论——两法迭代点逐点重合——不受此排印影响。</span>

因此，不需要单独进行收敛性分析（convergence analysis）。

<!-- p.10 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**不可行点处的牛顿步（Newton step at infeasible points）**</span>

11–6 页的第二种解释可推广到不可行的 $x$（即 $Ax \neq b$）。在不可行点 $x$（要求 $x \in \operatorname{dom} f$）处对最优性条件线性化，得

```math
\begin{bmatrix} \nabla^2 f(x) & A^T \\ A & 0 \end{bmatrix}
\begin{bmatrix} \Delta x_{\mathrm{nt}} \\ w \end{bmatrix}
= -\begin{bmatrix} \nabla f(x) \\ Ax - b \end{bmatrix}
\tag{1}
```

**原对偶解释（primal-dual interpretation）**：

- 把最优性条件写成残差（residual）方程 $r(y) = 0$，其中

```math
y = (x, \nu), \qquad r(y) = \big(\nabla f(x) + A^T\nu,\; Ax - b\big)
```

- 对 $r(y)=0$ 线性化，得 $r(y+\Delta y) \approx r(y) + Dr(y)\Delta y = 0$：

```math
\begin{bmatrix} \nabla^2 f(x) & A^T \\ A & 0 \end{bmatrix}
\begin{bmatrix} \Delta x_{\mathrm{nt}} \\ \Delta\nu_{\mathrm{nt}} \end{bmatrix}
= -\begin{bmatrix} \nabla f(x) + A^T\nu \\ Ax - b \end{bmatrix}
```

取 $w = \nu + \Delta\nu_{\mathrm{nt}}$ 时，与式 (1) 相同。

<!-- p.11 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**不可行初始点牛顿法（Infeasible start Newton method）**</span>

<span style="color:#2471a3;">**[algorithm]**</span>
给定起点 $x \in \operatorname{dom} f$、$\nu$，容许误差 $\epsilon > 0$，参数 $\alpha \in (0, 1/2)$，$\beta \in (0, 1)$。

**重复（repeat）：**

1. 计算原（primal）、对偶（dual）牛顿步 $\Delta x_{\mathrm{nt}},\ \Delta\nu_{\mathrm{nt}}$。
2. 对 $\|r\|_2$ 做回溯线搜索：令 $t := 1$；当 $\|r(x+t\Delta x_{\mathrm{nt}},\ \nu+t\Delta\nu_{\mathrm{nt}})\|_2 > (1-\alpha t)\,\|r(x, \nu)\|_2$ 时，令 $t := \beta t$。
3. **更新（update）**：$x := x + t\Delta x_{\mathrm{nt}}$，$\nu := \nu + t\Delta\nu_{\mathrm{nt}}$。

直到 $Ax = b$ 且 $\|r(x, \nu)\|_2 \le \epsilon$ 时终止。

- 这**不是下降方法**：可能出现 $f(x^{(k+1)}) > f(x^{(k)})$
- $\|r(y)\|_2$ 沿方向 $\Delta y = (\Delta x_{\mathrm{nt}}, \Delta\nu_{\mathrm{nt}})$ 的方向导数为

```math
\left. \frac{d}{dt}\, \big\| r(y + t\Delta y) \big\|_2 \right|_{t=0} = -\|r(y)\|_2
```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 残差 $r(y)=(\nabla f+A^T\nu,\ Ax-b)$ 把 05 讲 KKT 条件拆成的两半重新打包：前一分量是对偶残差，度量平稳条件被破坏的程度；后一分量是原残差，度量可行性被破坏的程度。不可行初始点牛顿法本质上是对 KKT 方程组整体做牛顿求根，雅可比 $Dr(y)$ 恰好就是 KKT 矩阵，线搜索的对象从目标函数换成 $\|r\|_2$，而方向导数 $-\|r\|_2$ 保证残差范数每步必有下降。$f$ 可能暂时上升并不奇怪：迭代初期的主要任务是偿还可行性的"欠账"，步长要同时服务"走向可行"与"走向最优"两个目标。这个原对偶（primal-dual）视角是下一讲内点法（interior-point method）的直接伏笔——原对偶内点法就是把残差换成"对偶残差 + 互补松弛（complementary slackness）残差"继续做牛顿迭代，看懂本页就等于预演了内点法的骨架。

<!-- p.12 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**求解 KKT 系统（Solving KKT systems）**</span>

考察一般形式的 KKT 系统：

```math
\begin{bmatrix} H & A^T \\ A & 0 \end{bmatrix}
\begin{bmatrix} v \\ w \end{bmatrix}
= -\begin{bmatrix} g \\ h \end{bmatrix}
```

求解方法（solution methods）：

- $\mathrm{LDL}^T$ 分解（$\mathrm{LDL}^T$ factorization）
- 消元（elimination）（当 $H$ 非奇异时）：

```math
AH^{-1}A^T w = h - AH^{-1}g, \qquad Hv = -(g + A^Tw)
```

- $H$ 奇异（singular）时的消元：先将系统改写为

```math
\begin{bmatrix} H + A^TQA & A^T \\ A & 0 \end{bmatrix}
\begin{bmatrix} v \\ w \end{bmatrix}
= -\begin{bmatrix} g + A^TQh \\ h \end{bmatrix}
```

其中取 $Q \succeq 0$ 使得 $H + A^TQA \succ 0$，再应用消元法。

<!-- p.13 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**等式约束解析中心（Equality constrained analytic centering）**</span>

<span style="color:#2471a3;">**[example]**</span>

原问题（primal problem）：

```math
\text{minimize } -\sum_{i=1}^n \log x_i \quad \text{subject to } Ax = b
```

对偶问题（dual problem）：

```math
\text{maximize } -b^T\nu + \sum_{i=1}^n \log (A^T\nu)_i + n
```

以 $A \in \mathbb{R}^{100\times 500}$ 的算例、从不同初始点出发，比较三种方法。

**方法 1：带等式约束的牛顿法**（要求 $x^{(0)} \succ 0$ 且 $Ax^{(0)} = b$）。图中横轴为迭代次数 $k$（0–20），纵轴为 $f(x^{(k)}) - p^\star$（对数刻度，刻度自 $10^5$ 到 $10^{-10}$）。

<!-- p.14 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**等式约束解析中心：三种方法的收敛曲线（续）**</span>

**方法 2：牛顿法应用于对偶问题**（要求 $A^T\nu^{(0)} \succ 0$）。图中横轴为迭代次数 $k$（0–10），纵轴为 $p^\star - g(\nu^{(k)})$，即对偶间隙（duality gap）（对数刻度，$10^5$–$10^{-10}$）。

**方法 3：不可行初始点牛顿法**（要求 $x^{(0)} \succ 0$）。图中横轴为迭代次数 $k$（0–25），纵轴为残差范数 $\|r(x^{(k)}, \nu^{(k)})\|_2$（对数刻度，$10^{10}$–$10^{-15}$）。

<!-- p.15 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**三种方法每次迭代的计算量**</span>

三种方法每次迭代的计算量（complexity per iteration）相同：

1. 用块消元（block elimination）求解 KKT 系统：

```math
\begin{bmatrix} \operatorname{diag}(x)^{-2} & A^T \\ A & 0 \end{bmatrix}
\begin{bmatrix} \Delta x \\ w \end{bmatrix}
= \begin{bmatrix} \operatorname{diag}(x)^{-1}\mathbf{1} \\ 0 \end{bmatrix}
```

   归结为求解 $A\,\operatorname{diag}(x)^2A^T w = b$。

2. 求解牛顿方程组

```math
A\,\operatorname{diag}(A^T\nu)^{-2}A^T\,\Delta\nu = -b + A\,\operatorname{diag}(A^T\nu)^{-1}\mathbf{1}
```

3. 用块消元求解 KKT 系统：

```math
\begin{bmatrix} \operatorname{diag}(x)^{-2} & A^T \\ A & 0 \end{bmatrix}
\begin{bmatrix} \Delta x \\ \Delta\nu \end{bmatrix}
= \begin{bmatrix} \operatorname{diag}(x)^{-1}\mathbf{1} \\ Ax - b \end{bmatrix}
```

   归结为求解 $A\,\operatorname{diag}(x)^2A^T w = 2Ax - b$。

**结论**：三种情形最终都归结为求解 $ADA^Tw = h$，其中 $D$ 为正对角阵（positive diagonal）。

<!-- p.16 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**网络流优化（Network flow optimization）**</span>

<span style="color:#2471a3;">**[example]**</span>

```math
\begin{array}{ll}
\text{minimize} & \sum_{i=1}^n \phi_i(x_i) \\
\text{subject to} & Ax = b
\end{array}
```

- 有向图（directed graph）：$n$ 条弧（arc）、$p+1$ 个节点（node）
- $x_i$：弧 $i$ 上的流量；$\phi_i$：弧 $i$ 的成本流函数（cost flow function），满足 $\phi_i''(x) > 0$
- <span style="color:#00838f;">节点—弧关联矩阵（node-incidence matrix）$\tilde{A} \in \mathbb{R}^{(p+1)\times n}$ 定义为</span>

```math
\tilde{A}_{ij} = \begin{cases}
1, & \text{若弧 } j \text{ 离开节点 } i \\
-1, & \text{若弧 } j \text{ 进入节点 } i \\
0, & \text{其他情形}
\end{cases}
```

- 缩减的节点—弧关联矩阵（reduced node-incidence matrix）$A \in \mathbb{R}^{p\times n}$：去掉 $\tilde{A}$ 的最后一行
- $b \in \mathbb{R}^p$ 为（缩减的）源向量（source vector）
- 若图连通（connected），则 $\operatorname{rank} A = p$

<!-- p.17 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**KKT 系统（网络流情形）**</span>

```math
\begin{bmatrix} H & A^T \\ A & 0 \end{bmatrix}
\begin{bmatrix} v \\ w \end{bmatrix}
= -\begin{bmatrix} g \\ h \end{bmatrix}
```

- $H = \operatorname{diag}\big(\phi_1''(x_1), \ldots, \phi_n''(x_n)\big)$，为正对角阵
- 用消元法求解：

```math
AH^{-1}A^T w = h - AH^{-1}g, \qquad Hv = -(g + A^Tw)
```

系数矩阵的稀疏模式（sparsity pattern）由图的连通性决定：

```math
(AH^{-1}A^T)_{ij} \neq 0 \iff (AA^T)_{ij} \neq 0
```

且这等价于：节点 $i$ 与节点 $j$ 之间有一条弧相连。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这一页把"图结构决定代数结构"展示得非常清楚：$(AA^T)_{ij}\neq 0$ 当且仅当节点 $i$ 与节点 $j$ 共享一条弧，因此 $AA^T$ 就是邻接结构在代数上的化身，正是 6.006 里邻接矩阵表示法的线性代数对应物。又因为 $H$ 是正对角阵，$H^{-1}$ 不改变任何零元的位置，消元后的 $AH^{-1}A^T$ 完整继承同样的稀疏模式——于是稀疏 Cholesky 分解的实际代价取决于非零元个数与消元时产生的填充（fill-in），而不是矩阵的名义尺寸 $p$。这也顺带解释了 11–15 页的结论：三种方法每次迭代都归结为 $ADA^Tw=h$（$D$ 为正对角阵），成本自然相同。工程上一个著名细节是节点编号顺序会显著影响填充量，"好的排序"本身就是一门学问，这正是 18.065 与稀疏求解器实践都反复强调的点。

<!-- p.18 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**线性矩阵不等式的解析中心（Analytic center of linear matrix inequality）**</span>

<span style="color:#2471a3;">**[example]**</span>

```math
\begin{array}{ll}
\text{minimize} & -\log\det X \\
\text{subject to} & \operatorname{tr}(A_iX) = b_i, \quad i = 1, \ldots, p
\end{array}
```

变量为 $X \in S^n$（$n \times n$ 对称矩阵）。

最优性条件（optimality conditions）：

```math
X^\star \succ 0, \qquad -(X^\star)^{-1} + \sum_{j=1}^p \nu_j^\star A_j = 0, \qquad \operatorname{tr}(A_iX^\star) = b_i,\; i = 1, \ldots, p
```

> <span style="color:#7f8c8d;">原文求和号后印作 $A_i$（下标 $i$ 与求和指标 $j$ 不匹配）；按 11–19 页消元式反推应为 $A_j$，译文已径改。</span>

在可行点 $X$ 处的牛顿方程（Newton equation）：

```math
X^{-1}\Delta XX^{-1} + \sum_{j=1}^p w_j A_j = X^{-1}, \qquad \operatorname{tr}(A_i\Delta X) = 0,\; i = 1, \ldots, p
```

> <span style="color:#7f8c8d;">此处同上：原文印作 $\sum_j w_jA_i$，应为 $\sum_j w_jA_j$。</span>

- 该方程由线性近似 $(X+\Delta X)^{-1} \approx X^{-1} - X^{-1}\Delta XX^{-1}$ 得到
- 变量 $\Delta X, w$ 共 $n(n+1)/2 + p$ 个

<!-- p.19 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**用块消元求解（Solution by block elimination）**</span>

- 从第一个方程中消去 $\Delta X$：

```math
\Delta X = X - \sum_{j=1}^p w_j\,XA_jX
```

- 把 $\Delta X$ 代入第二个方程：

```math
\sum_{j=1}^p \operatorname{tr}(A_iXA_jX)\, w_j = b_i, \quad i = 1, \ldots, p
\tag{2}
```

这是以 $w \in \mathbb{R}^p$ 为变量的稠密（dense）正定线性方程组。

采用 Cholesky 分解（Cholesky factorization）$X = LL^T$ 时的浮点运算次数（flop count，仅列主导项）：

- 形成 $p$ 个乘积 $L^TA_jL$：$(3/2)pn^3$
- 形成 $p(p+1)/2$ 个内积 $\operatorname{tr}\big((L^TA_iL)(L^TA_jL)\big)$：$(1/2)p^2n^2$
- 用 Cholesky 分解求解式 (2)：$(1/3)p^3$
