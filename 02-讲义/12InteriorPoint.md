# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

> <span style="color:#7f8c8d;">Source: SEE官方讲义/12InteriorPoint.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

<!-- p.1 -->

## <span style="color:#c0392b;">**12. 内点法 Interior-point methods**</span>

<span style="color:#2471a3;">**[lecture]**</span> 本讲讨论如何数值求解带不等式约束的凸优化问题，核心工具是 interior-point method（内点法）。内容提纲：

- inequality constrained minimization（不等式约束最小化）
- logarithmic barrier function and central path（对数障碍函数与中心路径）
- barrier method（障碍法）
- feasibility and phase I methods（可行性与第一阶段方法）
- complexity analysis via self-concordance（基于自和谐性的复杂度分析）
- generalized inequalities（广义不等式）

<!-- p.2 -->

### <span style="color:#c0392b;">**不等式约束最小化 Inequality constrained minimization**</span>

<span style="color:#2471a3;">**[section]**</span> 考虑优化问题

```math
\begin{array}{ll}
\text{minimize} & f_0(x) \\
\text{subject to} & f_i(x) \le 0, \quad i = 1, \dots, m \\
 & Ax = b
\end{array}
\tag{1}
```

- 各 $f_i$ 为凸函数（convex function），且二阶连续可微（twice continuously differentiable）；
- $A \in R^{p \times n}$，且 $\operatorname{rank} A = p$；
- 假设最优值（optimal value）$p^\star$ 有限且可达（attained）；
- 假设问题严格可行（strictly feasible）：存在 $\tilde{x}$ 使得 $\tilde{x} \in \operatorname{dom} f_0$、$f_i(\tilde{x}) < 0$（$i = 1, \dots, m$）、$A\tilde{x} = b$。

因此，强对偶性（strong duality）成立，且对偶最优解（dual optimum）可达。

<!-- p.3 -->

### <span style="color:#c0392b;">**例子 Examples**</span>

<span style="color:#2471a3;">**[section]**</span> 该框架覆盖的典型问题：

- 线性规划（linear program, LP）、二次规划（quadratic program, QP）、二次约束二次规划（QCQP）、几何规划（geometric program, GP）；
- 带线性不等式约束的熵最大化（entropy maximization）问题：

```math
\begin{array}{ll}
\text{minimize} & \sum_{i=1}^n x_i \log x_i \\
\text{subject to} & Fx \preceq g \\
 & Ax = b
\end{array}
```

其定义域为 $\operatorname{dom} f_0 = R^n_{++}$。

- 若目标函数不可微，可能需要先改写问题，例如把分段线性（piecewise-linear）最小化或 $\ell_\infty$-范数近似（$\ell_\infty$-norm approximation）问题改写成 LP；
- 半定规划 SDP 与二阶锥规划 SOCP 更适合用广义不等式（generalized inequalities）表述来处理（见后文）。

<!-- p.4 -->

### <span style="color:#c0392b;">**对数障碍 Logarithmic barrier**</span>

<span style="color:#2471a3;">**[definition]**</span> 用指示函数（indicator function）改写问题 (1)：

```math
\begin{array}{ll}
\text{minimize} & f_0(x) + \sum_{i=1}^m I_-(f_i(x)) \\
\text{subject to} & Ax = b
\end{array}
```

其中当 $u \le 0$ 时 $I_-(u) = 0$，否则 $I_-(u) = \infty$（即 $R_-$ 的指示函数）。

<span style="color:#00838f;">用对数障碍（logarithmic barrier）近似后，问题变为</span>

```math
\begin{array}{ll}
\text{minimize} & f_0(x) - (1/t) \sum_{i=1}^m \log(-f_i(x)) \\
\text{subject to} & Ax = b
\end{array}
```

- 这是一个只含等式约束的问题；
- 对 $t > 0$，$-(1/t)\log(-u)$ 是 $I_-$ 的光滑近似；
- 当 $t \to \infty$ 时近似越来越好。

> <span style="color:#7f8c8d;">*图示：函数 $-(1/t)\log(-u)$ 的曲线（横轴 $u$ 从 $-3$ 到 $1$，纵轴约 $-5$ 到 $10$）：在 $u = 0$ 左侧急剧升至 $+\infty$，右侧为 $0$，形如被平滑化的指示函数。*</span>

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 从第一性原理看，这一步做了两件事：先把硬约束「软化」成目标的一部分，再用参数 $t$ 控制软化的程度。$I_-(u)$ 在 $u = 0$ 处从 $0$ 跳到 $\infty$，任何梯度类算法都无从下手。对数障碍把它换成一条光滑、单调上升的曲线，离边界越近惩罚越大，迭代点因此始终被「关」在可行域内部——这正是内点法得名的原因。$1/t$ 的角色类似统计物理中的温度：$t$ 小时障碍平缓、解靠近可行域的「重心」，$t$ 大时障碍陡峭、解逼近原问题的最优解。为什么偏偏选对数而不是二次惩罚？因为对数障碍随后会展现出自和谐性（self-concordance）这一罕见的解析性质，使对偶间隙有显式上界 $m/t$，整套复杂度分析得以闭合，这是二次罚函数难以做到的。

<!-- p.5 -->

### <span style="color:#c0392b;">**对数障碍函数 Logarithmic barrier function**</span>

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">对数障碍函数定义为</span>

```math
\phi(x) = -\sum_{i=1}^m \log(-f_i(x)), \qquad \operatorname{dom} \phi = \{x \mid f_1(x) < 0,\ \dots,\ f_m(x) < 0\}
```

- 它是凸的（由复合规则（composition rules）推出）；
- 二阶连续可微，导数为：

```math
\nabla \phi(x) = \sum_{i=1}^m \frac{1}{-f_i(x)} \nabla f_i(x)
```

```math
\nabla^2 \phi(x) = \sum_{i=1}^m \frac{1}{f_i(x)^2} \nabla f_i(x) \nabla f_i(x)^T + \sum_{i=1}^m \frac{1}{-f_i(x)} \nabla^2 f_i(x)
```

从 Hessian 的结构看：第一项是秩一项 $\nabla f_i \nabla f_i^T$ 以 $1/f_i^2$ 加权之和（秩一项之和必半正定，正是 Strang 线代里 Gram 矩阵的老朋友）；当某 $f_i \to 0^-$ 时权重 $1/f_i^2$ 爆炸——离约束边界越近，$\phi$ 的曲率越大，这正是障碍把迭代点推离边界的力学来源。

<!-- p.6 -->

### <span style="color:#c0392b;">**中心路径 Central path**</span>

<span style="color:#2471a3;">**[definition]**</span> 对 $t > 0$，定义 $x^\star(t)$ 为如下问题的解：

```math
\begin{array}{ll}
\text{minimize} & t f_0(x) + \phi(x) \\
\text{subject to} & Ax = b
\end{array}
```

（目前先假设对每个 $t > 0$，$x^\star(t)$ 存在且唯一。）

<span style="color:#00838f;">中心路径（central path）即点集 $\{x^\star(t) \mid t > 0\}$。</span>

<span style="color:#2471a3;">**[example]**</span> 例：一个 LP 的中心路径。对问题 $\min c^T x$（约束 $a_i^T x \le b_i$，$i = 1, \dots, 6$），中心路径是可行多边形内部的一条曲线，$t \to \infty$ 时收敛到最优顶点；超平面 $c^T x = c^T x^\star(t)$ 与过 $x^\star(t)$ 的 $\phi$ 等值线相切。

> <span style="color:#7f8c8d;">*图示：可行多边形内画出的中心路径，标注目标方向 $c$、最优解 $x^\star$ 与 $t = 10$ 时的中心路径点 $x^\star(10)$。*</span>

<!-- p.7 -->

### <span style="color:#c0392b;">**中心路径上的对偶点 Dual points on central path**</span>

<span style="color:#2471a3;">**[section]**</span> 若存在 $w$ 使得

```math
t \nabla f_0(x) + \sum_{i=1}^m \frac{1}{-f_i(x)} \nabla f_i(x) + A^T w = 0, \qquad Ax = b
```

则 $x = x^\star(t)$。

- 因此，$x^\star(t)$ 最小化拉格朗日函数（Lagrangian）

```math
L(x, \lambda^\star(t), \nu^\star(t)) = f_0(x) + \sum_{i=1}^m \lambda_i^\star(t) f_i(x) + \nu^\star(t)^T (Ax - b)
```

其中定义 $\lambda_i^\star(t) = 1/(-t f_i(x^\star(t)))$，$\nu^\star(t) = w/t$。

- 这印证了直觉：当 $t \to \infty$ 时 $f_0(x^\star(t)) \to p^\star$：

```math
p^\star \ge g(\lambda^\star(t), \nu^\star(t)) = L(x^\star(t), \lambda^\star(t), \nu^\star(t)) = f_0(x^\star(t)) - \frac{m}{t}
```

<!-- p.8 -->

### <span style="color:#c0392b;">**用 KKT 条件解释 Interpretation via KKT conditions**</span>

<span style="color:#2471a3;">**[section]**</span> 取 $x = x^\star(t)$、$\lambda = \lambda^\star(t)$、$\nu = \nu^\star(t)$，则它们满足：

1. 原始约束（primal constraints）：$f_i(x) \le 0$（$i = 1, \dots, m$），$Ax = b$；
2. 对偶约束（dual constraints）：$\lambda \succeq 0$；
3. 近似互补松弛（approximate complementary slackness）：$-\lambda_i f_i(x) = 1/t$（$i = 1, \dots, m$）；
4. 拉格朗日函数对 $x$ 的梯度为零：$\nabla f_0(x) + \sum_{i=1}^m \lambda_i \nabla f_i(x) + A^T \nu = 0$。

与标准 KKT 条件（KKT conditions）的差别仅在于：第 3 条把 $\lambda_i f_i(x) = 0$ 替换成了 $-\lambda_i f_i(x) = 1/t$。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这条「近似互补松弛」是理解内点法的钥匙。回忆标准 KKT 条件中的互补松弛要求 $\lambda_i f_i(x) = 0$：每个不等式约束要么「顶死」（$f_i(x) = 0$）要么松弛（$\lambda_i = 0$），是一个非此即彼的离散选择。内点法把它替换成随 $t$ 增大而不断收紧的等式，让每个约束始终以 $1/t$ 的力度参与，等于把离散选择连续化了。当 $t \to \infty$ 时，这组条件连续变形为真正的 KKT 条件——数值分析里把这种「解随参数连续移动的轨道」称为同伦（homotopy）或路径跟踪（path-following）方法。附带的好处是：沿中心路径行进时，对偶可行点与对偶间隙估计 $m/t$ 是免费附赠的，无需另行求解对偶问题，这为算法提供了天然的停止准则。

<!-- p.9 -->

### <span style="color:#c0392b;">**力场解释 Force field interpretation**</span>

<span style="color:#2471a3;">**[section]**</span> 居中问题（centering problem，对不含等式约束的问题）：

```math
\text{minimize} \qquad t f_0(x) - \sum_{i=1}^m \log(-f_i(x))
```

力场（force field）解释：

- $t f_0(x)$ 是力场 $F_0(x) = -t \nabla f_0(x)$ 的势（potential）；
- $-\log(-f_i(x))$ 是力场 $F_i(x) = (1/f_i(x)) \nabla f_i(x)$ 的势。

在 $x^\star(t)$ 处各力恰好平衡：

```math
F_0(x^\star(t)) + \sum_{i=1}^m F_i(x^\star(t)) = 0
```

<!-- p.10 -->

### <span style="color:#c0392b;">**例：LP 的力场 Example**</span>

<span style="color:#2471a3;">**[example]**</span> 例：对 LP $\min c^T x$（约束 $a_i^T x \le b_i$，$i = 1, \dots, m$）：

- 目标力场是常力：$F_0(x) = -t c$；
- 约束力场随到约束超平面的距离按反比衰减：

```math
F_i(x) = \frac{-a_i}{b_i - a_i^T x}, \qquad \|F_i(x)\|_2 = \frac{1}{\operatorname{dist}(x, H_i)}
```

其中 $H_i = \{x \mid a_i^T x = b_i\}$。

> <span style="color:#7f8c8d;">*图示：$t = 1$ 与 $t = 3$ 两档下的合力场示意：$t$ 越大，目标常力（标注 $-c$、$-3c$）越强，边界附近的排斥力相对变弱，点被推向更贴近最优面的位置。*</span>

<!-- p.11 -->

### <span style="color:#c0392b;">**障碍法 Barrier method**</span>

<span style="color:#2471a3;">**[algorithm]**</span> **障碍法（barrier method）**。给定严格可行点 $x$，$t := t^{(0)} > 0$，$\mu > 1$，容差 $\epsilon > 0$。重复：

1. **居中步（Centering step）**：在约束 $Ax = b$ 下最小化 $t f_0 + \phi$，计算 $x^\star(t)$。
2. **更新（Update）**：$x := x^\star(t)$。
3. **停止准则（Stopping criterion）**：若 $m/t < \epsilon$ 则退出。
4. **增大 $t$（Increase $t$）**：$t := \mu t$。

- 算法终止时有 $f_0(x) - p^\star \le \epsilon$（停止准则的依据是 $f_0(x^\star(t)) - p^\star \le m/t$）；
- 居中步通常用牛顿法（Newton's method）完成，以上一轮的 $x$ 为起点；
- $\mu$ 的选取是一个权衡：$\mu$ 大意味着外层迭代更少、内层（牛顿）迭代更多；典型取值 $\mu = 10$–$20$；
- $t^{(0)}$ 的选取有若干启发式方法。

<!-- p.12 -->

### <span style="color:#c0392b;">**收敛性分析 Convergence analysis**</span>

<span style="color:#2471a3;">**[section]**</span> 外层（居中）迭代次数恰好为

```math
\left\lceil \frac{\log(m/(\epsilon t^{(0)}))}{\log \mu} \right\rceil
```

另加初始居中步（用于计算 $x^\star(t^{(0)})$）。

居中问题 $\min\ t f_0(x) + \phi(x)$ 的收敛性参见牛顿法的收敛性分析：

- 对 $t \ge t^{(0)}$，$t f_0 + \phi$ 必须具有闭下水平集（closed sublevel sets）；
- 经典分析要求强凸性（strong convexity）与 Lipschitz 条件；
- 基于自和谐性（self-concordance）的分析则要求 $t f_0 + \phi$ 是自和谐的。

<!-- p.13 -->

### <span style="color:#c0392b;">**例子：不等式形式 LP Examples**</span>

<span style="color:#2471a3;">**[example]**</span> 不等式形式 LP（$m = 100$ 个不等式，$n = 50$ 个变量）：

> <span style="color:#7f8c8d;">*图示：左图为对偶间隙（duality gap）随牛顿迭代次数下降的曲线（$\mu = 2, 50, 150$ 三条）；右图为牛顿迭代总数随 $\mu$ 的变化，横轴 $\mu \in [0, 100]$，纵轴迭代次数约 $0$–$140$。*</span>

- 起点取中心路径上的点（$t^{(0)} = 1$，对偶间隙为 $100$）；
- 当 $t = 10^8$ 时终止（对偶间隙 $10^{-6}$）；
- 居中使用带回溯（backtracking）线搜索的牛顿法；
- 当 $\mu \ge 10$ 时，牛顿迭代总数对 $\mu$ 不太敏感。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这组实验揭示了一个不太直观的现象：$\mu$ 在很宽的范围内（约 $10$–$100$）取值，对牛顿迭代总数影响甚微。原因在于两类迭代次数此消彼长：$\mu$ 大，外层居中次数 $\lceil \log(m/(\epsilon t^{(0)}))/\log \mu \rceil$ 变少，但每次居中要跨过更大的 $t$ 区间、所需牛顿步数变多；$\mu$ 小则相反。两项相乘后出现一个又宽又平的「谷底」，工程上最喜欢这种不敏感——超参数不必精调。这也呼应 6.006 的老经验：渐近复杂度与常数因子是两回事。理论上 $\mu \to 1^+$ 时上界最优，实践中 $\mu = 10$–$20$ 才是甜点，因为每次外层迭代还有矩阵分解等未计入的固定开销。

<!-- p.14 -->

### <span style="color:#c0392b;">**例子：几何规划 Geometric program**</span>

<span style="color:#2471a3;">**[example]**</span> 几何规划（$m = 100$ 个不等式，$n = 50$ 个变量）：

```math
\begin{array}{ll}
\text{minimize} & \log\Big(\sum_{k=1}^5 \exp(a_{0k}^T x + b_{0k})\Big) \\
\text{subject to} & \log\Big(\sum_{k=1}^5 \exp(a_{ik}^T x + b_{ik})\Big) \le 0, \quad i = 1, \dots, m
\end{array}
```

> <span style="color:#7f8c8d;">*图示：与 LP 情形同款的两张图（对偶间隙–牛顿迭代曲线；$\mu = 2, 50, 150$ 下的迭代总数），形态几乎一致。*</span>

<!-- p.15 -->

### <span style="color:#c0392b;">**例子：标准形式 LP 族 Examples**</span>

<span style="color:#2471a3;">**[example]**</span> 标准形式 LP 族（$A \in R^{m \times 2m}$）：

```math
\begin{array}{ll}
\text{minimize} & c^T x \\
\text{subject to} & Ax = b, \quad x \succeq 0
\end{array}
```

取 $m = 10, \dots, 1000$；对每个 $m$ 求解 100 个随机生成的实例。

> <span style="color:#7f8c8d;">*图示：横轴为 $m$（对数刻度 $10^1$–$10^3$），纵轴为牛顿迭代次数（约 15–35），点云近乎水平。*</span>

当 $m$ 在 100:1 的范围内变化时，迭代次数增长非常缓慢。

<!-- p.16 -->

### <span style="color:#c0392b;">**可行性与第一阶段方法 Feasibility and phase I methods**</span>

<span style="color:#2471a3;">**[section]**</span> 可行性问题（feasibility problem）：求 $x$ 使得

```math
f_i(x) \le 0, \quad i = 1, \dots, m, \qquad Ax = b
\tag{2}
```

第一阶段（phase I）方法：为障碍法计算严格可行的起点。

**基本第一阶段方法（basic phase I method）**：

```math
\begin{array}{ll}
\text{minimize (over } x, s\text{)} & s \\
\text{subject to} & f_i(x) \le s, \quad i = 1, \dots, m \\
 & Ax = b
\end{array}
\tag{3}
```

- 若 $x, s$ 可行且 $s < 0$，则 $x$ 对问题 (2) 严格可行；
- 若 (3) 的最优值 $\bar{p}^\star$ 为正，则问题 (2) 不可行（infeasible）；
- 若 $\bar{p}^\star = 0$ 且可达，则问题 (2) 可行（但非严格可行）；若 $\bar{p}^\star = 0$ 且不可达，则问题 (2) 不可行。

<!-- p.17 -->

### <span style="color:#c0392b;">**不可行性之和第一阶段方法 Sum of infeasibilities phase I method**</span>

<span style="color:#2471a3;">**[algorithm]**</span> **不可行性之和（sum of infeasibilities）第一阶段方法**：

```math
\begin{array}{ll}
\text{minimize} & \mathbf{1}^T s \\
\text{subject to} & s \succeq 0, \quad f_i(x) \le s_i, \quad i = 1, \dots, m \\
 & Ax = b
\end{array}
```

对不可行问题，它给出的解比基本第一阶段方法满足多得多的不等式。

例（50 个变量、100 条线性不等式构成的不可行集）：

> <span style="color:#7f8c8d;">*图示：两张按「违约余量」$b_i - a_i^T x$ 排序的条形图。左图（基本第一阶段法，$b_i - a_i^T x_{\max}$）满足 39 条不等式；右图（不可行性之和法，$b_i - a_i^T x_{\mathrm{sum}}$）满足 79 条不等式（原文误作 79 个 solutions，应为 inequalities）。*</span>

<!-- p.18 -->

### <span style="color:#c0392b;">**例：线性不等式族 Example**</span>

<span style="color:#2471a3;">**[example]**</span> 例：线性不等式族 $Ax \preceq b + \gamma \Delta b$。

- 数据选成：$\gamma > 0$ 时严格可行，$\gamma \le 0$ 时不可行；
- 用基本第一阶段方法，当 $s < 0$ 或对偶目标值为正时终止。

> <span style="color:#7f8c8d;">*图示：三张以 $\gamma \in [-1, 1]$ 为横轴、牛顿迭代次数（$0$–$100$）为纵轴的图（线性刻度与两种对数刻度），并标注 Infeasible（不可行）/ Feasible（可行）分界。*</span>

迭代次数大致正比于 $\log(1/|\gamma|)$。

<!-- p.19 -->

### <span style="color:#c0392b;">**基于自和谐性的复杂度分析 Complexity analysis via self-concordance**</span>

<span style="color:#2471a3;">**[section]**</span> 假设与本讲第 12–1、12–2 页相同，另加：

- （$f_0$ 在可行集上的）下水平集有界；
- $t f_0 + \phi$ 自和谐，且具有闭下水平集。

第二个条件：

- 对 LP、QP、QCQP 自动成立；
- 可能需要改写问题才能满足，例如：

```math
\begin{array}{ll}
\text{minimize} & \sum_{i=1}^n x_i \log x_i \\
\text{subject to} & Fx \preceq g
\end{array}
\;\longrightarrow\;
\begin{array}{ll}
\text{minimize} & \sum_{i=1}^n x_i \log x_i \\
\text{subject to} & Fx \preceq g, \quad x \succeq 0
\end{array}
```

- 复杂度分析需要这个条件；即便自和谐假设不成立，障碍法本身照样可用。

<!-- p.20 -->

### <span style="color:#c0392b;">**每次居中的牛顿迭代次数 Newton iterations per centering step**</span>

<span style="color:#2471a3;">**[section]**</span> 由自和谐理论（self-concordance theory）：

```math
\#\text{Newton iterations} \le \frac{\mu t f_0(x) + \phi(x) - \mu t f_0(x^+) - \phi(x^+)}{\gamma} + c
```

- 这是从 $x = x^\star(t)$ 出发计算 $x^+ = x^\star(\mu t)$ 所需工作量的上界；
- $\gamma, c$ 是常数（只依赖于牛顿算法的参数）；
- 由对偶性（取 $\lambda = \lambda^\star(t)$、$\nu = \nu^\star(t)$）：

```math
\begin{aligned}
&\mu t f_0(x) + \phi(x) - \mu t f_0(x^+) - \phi(x^+) \\
&\quad = \mu t f_0(x) - \mu t f_0(x^+) + \sum_{i=1}^m \log(-\mu t \lambda_i f_i(x^+)) - m \log \mu \\
&\quad \le \mu t f_0(x) - \mu t f_0(x^+) - \mu t \sum_{i=1}^m \lambda_i f_i(x^+) - m - m \log \mu \\
&\quad \le \mu t f_0(x) - \mu t g(\lambda, \nu) - m - m \log \mu \\
&\quad = m(\mu - 1 - \log \mu)
\end{aligned}
```

<!-- p.21 -->

### <span style="color:#c0392b;">**牛顿迭代总数 Total number of Newton iterations**</span>

<span style="color:#2471a3;">**[section]**</span> 牛顿迭代总数（不计第一次居中）：

```math
\#\text{Newton iterations} \le N = \left\lceil \frac{\log(m/(t^{(0)} \epsilon))}{\log \mu} \right\rceil \left( \frac{m(\mu - 1 - \log \mu)}{\gamma} + c \right)
```

> <span style="color:#7f8c8d;">*图示：$\mu \in [1, 1.2]$ 区间内 $N$ 的曲线，纵轴高达 $5 \times 10^4$：$\mu \to 1^+$ 时 $N$ 爆炸式增长。*</span>

图中取典型的 $\gamma, c$，$m = 100$，$m/(t^{(0)}\epsilon) = 10^5$：

- 印证了 $\mu$ 选择的权衡；
- 实践中迭代次数是几十次量级；$\mu \ge 10$ 时不太敏感。

<!-- p.22 -->

### <span style="color:#c0392b;">**障碍法的多项式时间复杂度 Polynomial-time complexity of barrier method**</span>

<span style="color:#2471a3;">**[section]**</span> 取 $\mu = 1 + 1/\sqrt{m}$ 时：

```math
N = O\!\left(\sqrt{m}\, \log\big(m/(\epsilon t^{(0)})\big)\right)
```

- 固定对偶间隙缩减比时，牛顿迭代次数为 $O(\sqrt{m})$；
- 再乘以单次牛顿迭代的代价（问题维度的多项式函数），即得浮点运算次数（flops）的上界。

这一取法优化的是最坏情况复杂度；实践中我们取固定的 $\mu$（$\mu = 10, \dots, 20$）。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 把这条复杂度翻译成 6.006 的语言：内点法把线性规划从「单纯形法最坏情况指数时间」变成迭代次数 $O(\sqrt{m}\log(m/\epsilon))$、每次迭代代价为维度多项式的算法，是复杂性意义上的里程碑。历史上，Khachiyan 在 1979 年用椭球法（ellipsoid method）首次证明 LP 多项式时间可解，但常数过大、实际跑不快。Karmarkar 于 1984 年提出投影式内点算法，宣称在大型结构化问题上远超单纯形法，掀起内点法研究热潮，AT&T 甚至为它申请了专利。今天的共识是两类算法各擅胜场；而「是否存在强多项式时间（strongly polynomial）的 LP 算法」——迭代次数只依赖 $m, n$、与数据位长无关——至今仍是公开难题。另注意 $\mu = 1 + 1/\sqrt{m}$ 只是优化最坏情况；实践中固定 $\mu \approx 10$–$20$ 通常更快。

<!-- p.23 -->

### <span style="color:#c0392b;">**广义不等式 Generalized inequalities**</span>

<span style="color:#2471a3;">**[section]**</span> 考虑广义不等式版本的优化问题：

```math
\begin{array}{ll}
\text{minimize} & f_0(x) \\
\text{subject to} & f_i(x) \preceq_{K_i} 0, \quad i = 1, \dots, m \\
 & Ax = b
\end{array}
```

- $f_0$ 凸；$f_i : R^n \to R^{k_i}$（$i = 1, \dots, m$）关于正常锥（proper cone）$K_i \subseteq R^{k_i}$ 凸；
- 各 $f_i$ 二阶连续可微；
- $A \in R^{p \times n}$，$\operatorname{rank} A = p$；
- 假设 $p^\star$ 有限且可达；
- 假设问题严格可行；因此强对偶性成立，且对偶最优解可达。

最令人感兴趣的例子：SOCP、SDP。

<!-- p.24 -->

### <span style="color:#c0392b;">**正常锥的广义对数函数 Generalized logarithm for proper cone**</span>

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">若 $\psi : R^q \to R$ 满足下面两条，就称 $\psi$ 是正常锥 $K \subseteq R^q$ 的广义对数函数（generalized logarithm）：</span>

- $\operatorname{dom} \psi = \operatorname{int} K$，且对 $y \succ_K 0$ 有 $\nabla^2 \psi(y) \prec 0$；
- 对 $y \succ_K 0$、$s > 0$，$\psi(sy) = \psi(y) + \theta \log s$（$\theta$ 称为 $\psi$ 的阶（degree））。

例子：

- 非负象限（nonnegative orthant）$K = R^n_+$：$\psi(y) = \sum_{i=1}^n \log y_i$，阶 $\theta = n$；
- 半正定锥（positive semidefinite cone）$K = S^n_+$：$\psi(Y) = \log \det Y$（$\theta = n$）；
- 二阶锥（second-order cone）$K = \{y \in R^{n+1} \mid (y_1^2 + \cdots + y_n^2)^{1/2} \le y_{n+1}\}$：$\psi(y) = \log(y_{n+1}^2 - y_1^2 - \cdots - y_n^2)$（$\theta = 2$）。

<!-- p.25 -->

### <span style="color:#c0392b;">**广义对数函数的性质 Properties**</span>

<span style="color:#2471a3;">**[section]**</span> 性质（不证）：对 $y \succ_K 0$，

```math
\nabla \psi(y) \succeq_{K^*} 0, \qquad y^T \nabla \psi(y) = \theta
```

- 非负象限 $R^n_+$：$\psi(y) = \sum_{i=1}^n \log y_i$，$\nabla \psi(y) = (1/y_1, \dots, 1/y_n)$，$y^T \nabla \psi(y) = n$；
- 半正定锥 $S^n_+$：$\psi(Y) = \log \det Y$，$\nabla \psi(Y) = Y^{-1}$，$\operatorname{tr}(Y \nabla \psi(Y)) = n$；
- 二阶锥 $K = \{y \in R^{n+1} \mid (y_1^2 + \cdots + y_n^2)^{1/2} \le y_{n+1}\}$，$\psi(y) = \log(y_{n+1}^2 - y_1^2 - \cdots - y_n^2)$：

```math
\nabla \psi(y) = \frac{2}{y_{n+1}^2 - y_1^2 - \cdots - y_n^2} \begin{bmatrix} -y_1 \\ \vdots \\ -y_n \\ y_{n+1} \end{bmatrix}, \qquad y^T \nabla \psi(y) = 2
```

<!-- p.26 -->

### <span style="color:#c0392b;">**对数障碍与中心路径 Logarithmic barrier and central path**</span>

<span style="color:#2471a3;">**[definition]**</span> 对约束 $f_1(x) \preceq_{K_1} 0, \dots, f_m(x) \preceq_{K_m} 0$ 的对数障碍：

```math
\phi(x) = -\sum_{i=1}^m \psi_i(-f_i(x)), \qquad \operatorname{dom} \phi = \{x \mid f_i(x) \prec_{K_i} 0,\ i = 1, \dots, m\}
```

- 其中 $\psi_i$ 是 $K_i$ 的广义对数函数，阶为 $\theta_i$；
- $\phi$ 凸且二阶连续可微。

中心路径：$\{x^\star(t) \mid t > 0\}$，其中 $x^\star(t)$ 求解 $\min\ t f_0(x) + \phi(x)$（约束 $Ax = b$）。

<!-- p.27 -->

### <span style="color:#c0392b;">**中心路径上的对偶点（广义不等式） Dual points on central path**</span>

<span style="color:#2471a3;">**[section]**</span> 若存在 $w \in R^p$ 使得

```math
t \nabla f_0(x) + \sum_{i=1}^m D f_i(x)^T \nabla \psi_i(-f_i(x)) + A^T w = 0
```

则 $x = x^\star(t)$（其中 $D f_i(x) \in R^{k_i \times n}$ 是 $f_i$ 的导数矩阵）。

- 因此 $x^\star(t)$ 最小化拉格朗日函数 $L(x, \lambda^\star(t), \nu^\star(t))$，其中

```math
\lambda_i^\star(t) = \frac{1}{t} \nabla \psi_i(-f_i(x^\star(t))), \qquad \nu^\star(t) = \frac{w}{t}
```

- 由 $\psi_i$ 的性质：$\lambda_i^\star(t) \succ_{K_i^*} 0$，且对偶间隙（duality gap）为

```math
f_0(x^\star(t)) - g(\lambda^\star(t), \nu^\star(t)) = \frac{1}{t} \sum_{i=1}^m \theta_i
```

<!-- p.28 -->

### <span style="color:#c0392b;">**例：半定规划 Example: semidefinite programming**</span>

<span style="color:#2471a3;">**[example]**</span> 半定规划（$F_i \in S^p$）：

```math
\begin{array}{ll}
\text{minimize} & c^T x \\
\text{subject to} & F(x) = \sum_{i=1}^n x_i F_i + G \preceq 0
\end{array}
```

- 对数障碍：$\phi(x) = \log \det(-F(x))^{-1}$；
- 中心路径：$x^\star(t)$ 最小化 $t c^T x - \log \det(-F(x))$；因此

```math
t c_i - \operatorname{tr}\big(F_i F(x^\star(t))^{-1}\big) = 0, \quad i = 1, \dots, n
```

- 中心路径上的对偶点 $Z^\star(t) = -(1/t) F(x^\star(t))^{-1}$ 对下面的问题可行：

```math
\begin{array}{ll}
\text{maximize} & \operatorname{tr}(G Z) \\
\text{subject to} & \operatorname{tr}(F_i Z) + c_i = 0, \quad i = 1, \dots, n \\
 & Z \succeq 0
\end{array}
```

- 中心路径上的对偶间隙：$c^T x^\star(t) - \operatorname{tr}(G Z^\star(t)) = p/t$。

<!-- p.29 -->

### <span style="color:#c0392b;">**障碍法（广义不等式版） Barrier method**</span>

<span style="color:#2471a3;">**[algorithm]**</span> 给定严格可行点 $x$，$t := t^{(0)} > 0$，$\mu > 1$，容差 $\epsilon > 0$。重复：

1. **居中步**：在约束 $Ax = b$ 下最小化 $t f_0 + \phi$，计算 $x^\star(t)$。
2. **更新**：$x := x^\star(t)$。
3. **停止准则**：若 $\left(\sum_i \theta_i\right)/t < \epsilon$ 则退出。
4. **增大 $t$**：$t := \mu t$。

- 唯一的改动：中心路径上的对偶间隙由 $m/t$ 换成 $\left(\sum_i \theta_i\right)/t$；
- 外层迭代次数：$\left\lceil \log\big((\sum_i \theta_i)/(\epsilon t^{(0)})\big) / \log \mu \right\rceil$；
- 基于自和谐性的复杂度分析适用于 SDP、SOCP。

<!-- p.30 -->

### <span style="color:#c0392b;">**例子 Examples**</span>

<span style="color:#2471a3;">**[example]**</span> 二阶锥规划（SOCP，50 个变量、$R^6$ 中 50 个二阶锥约束）：

> <span style="color:#7f8c8d;">*图示：对偶间隙–牛顿迭代曲线（$\mu = 2, 50, 200$）与迭代总数–$\mu$ 曲线，形态与 LP/GP 例子一致。*</span>

半定规划（SDP，100 个变量、$S^{100}$ 中的线性矩阵不等式（LMI）约束）：

> <span style="color:#7f8c8d;">*图示：对偶间隙–牛顿迭代曲线（$\mu = 2, 50, 150$）与迭代总数–$\mu$ 曲线，迭代总数约 $20$–$140$。*</span>

<!-- p.31 -->

### <span style="color:#c0392b;">**例：SDP 族 Example**</span>

<span style="color:#2471a3;">**[example]**</span> SDP 族（$A \in S^n$，$x \in R^n$）：

```math
\begin{array}{ll}
\text{minimize} & \mathbf{1}^T x \\
\text{subject to} & A + \operatorname{diag}(x) \succeq 0
\end{array}
```

取 $n = 10, \dots, 1000$；对每个 $n$ 求解 100 个随机生成的实例。

> <span style="color:#7f8c8d;">*图示：横轴 $n$（对数刻度 $10^1$–$10^3$），纵轴牛顿迭代次数（约 15–35），与 LP 族的图几乎同构。*</span>

<!-- p.32 -->

### <span style="color:#c0392b;">**原对偶内点法 Primal-dual interior-point methods**</span>

<span style="color:#2471a3;">**[section]**</span> 需要高精度解时，原对偶内点法（primal-dual interior-point methods）比障碍法更高效：

- 每次迭代同时更新原始（primal）变量与对偶（dual）变量；不区分内层与外层迭代；
- 常表现出超线性（superlinear）渐近收敛；
- 搜索方向可解释为对修正后的 KKT 条件施加的牛顿方向；
- 可以从不可行点出发；
- 单次迭代的代价与障碍法相同。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 为什么实用求解器（如 Mosek、SDPT3、Gurobi 的内点内核）几乎都采用原对偶形式？障碍法「先解一串子问题、再逐步逼近」的结构信息利用率偏低。原对偶法每一步同时挪动 $x$ 与 $(\lambda, \nu)$，用近似互补松弛 $-\lambda_i f_i(x) \approx 1/t$ 的残差构造牛顿方向。这样通常几十步就能把对偶间隙压到机器精度量级，且常呈超线性收敛。工程上真正的瓶颈转移到每步要解的 KKT 线性方程组：随着 $t$ 增大，矩阵条件数（condition number）会急剧恶化。这正是 CSAPP 中浮点舍入误差教训的用武之地——实际实现依赖稀疏 Cholesky 分解、对称化与正则化技巧，而不是天真地直接求逆。
