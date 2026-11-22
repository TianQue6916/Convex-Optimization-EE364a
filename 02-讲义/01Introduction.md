# <span style="color:#c0392b;">凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）</span>

> <span style="color:#7f8c8d;">Source: SEE官方讲义/01Introduction.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

## <span style="color:#c0392b;">1. Introduction（引言）</span>

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">本讲内容（Lecture Outline）</span>

<!-- p.1 -->

<span style="color:#2471a3;">**[lecture]**</span> **Convex Optimization — Boyd & Vandenberghe（凸优化）**
**1. Introduction（1. 引言）**

本讲涵盖以下主题：

- mathematical optimization（数学优化）
- least-squares and linear programming（最小二乘与线性规划）
- convex optimization（凸优化）
- example（实例）
- course goals and topics（课程目标与主题）
- nonlinear optimization（非线性优化）
- brief history of convex optimization（凸优化简史）

---

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">数学优化（Mathematical Optimization）</span>

<!-- p.2 -->

<span style="color:#00838f;">**[definition]**</span> <span style="color:#00838f;">（数学）优化问题 ((mathematical) optimization problem) 的标准形式：</span>

```math
\begin{array}{ll}
\text{minimize} & f_0(x) \\
\text{subject to} & f_i(x) \le b_i, \quad i = 1, \ldots, m
\end{array}
```

- $x = (x_1, \ldots, x_n)$：optimization variables（优化变量）
- $f_0 : \mathbb{R}^n \to \mathbb{R}$：objective function（目标函数）
- $f_i : \mathbb{R}^n \to \mathbb{R}$，$i = 1, \ldots, m$：constraint functions（约束函数）

optimal solution（最优解）$x^\star$ 是在所有满足约束的向量中使 $f_0$ 取值最小的向量。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 把所有优化问题统一写成"最小化 $f_0$ + 一组不等式 $f_i(x) \le b_i$"这个标准形式，表面上看只是记号统一，实际上是在为后续的算法设计与复杂度分析铺路。你在 6.006 里已经体会过：同一个问题的不同等价表述，可解性可能天差地别——优化领域更是如此，一个问题的"难度"并不取决于它文字上多复杂，而取决于 $f_0$ 与 $f_i$ 的结构性质（凸性、光滑性、可分解性）。注意这里的约束方向统一写成 $\le$：任何 $\ge$ 型不等式乘以 $-1$、等式约束拆成两条不等式后都能化成这种形状，所以该标准形式具有完备的表达能力。还要提醒一句："最优解 $x^\star$ 存在"本身并非常事——目标函数可能无下界，可行集可能为空，这些退化情形会在后面章节被精确刻画。Boyd 全课程固定沿用本页符号：$f_0$、$f_i$、$b_i$、$x^\star$ 会贯穿整个学期，现在花一分钟记牢，后面读 slide 会顺畅很多。

---

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">应用举例（Examples）</span>

<!-- p.3 -->

<span style="color:#2471a3;">**[example]**</span> 三个经典应用领域，结构完全同型（变量 / 约束 / 目标三件套）：

**portfolio optimization（投资组合优化）**

- variables（变量）：投资于不同资产的资金数额
- constraints（约束）：预算、每种资产的最大/最小投资额、最低回报
- objective（目标）：整体风险（overall risk）或回报方差（return variance）

**device sizing in electronic circuits（电子电路中的器件尺寸设计）**

- variables：器件的宽度与长度
- constraints：制造工艺限制（manufacturing limits）、时序要求（timing requirements）、最大面积
- objective：功耗（power consumption）

**data fitting（数据拟合）**

- variables：模型参数（model parameters）
- constraints：先验信息（prior information）、参数取值限制
- objective：失配程度或预测误差的某种度量（measure of misfit or prediction error）

---

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">求解优化问题（Solving Optimization Problems）</span>

<!-- p.4 -->

一般优化问题（general optimization problem）：

- 非常难以求解（very difficult to solve）
- 现有方法都要付出某种妥协（compromise），例如：计算时间极长，或者不能保证每次都找到解

exceptions（例外）：某些特定问题类可以被高效且可靠地求解：

- least-squares problems（最小二乘问题）
- linear programming problems（线性规划问题）
- convex optimization problems（凸优化问题）

---

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">最小二乘（Least-Squares）</span>

<!-- p.5 -->

```math
\text{minimize} \quad \|Ax - b\|_2^2
```

**solving least-squares problems（求解最小二乘问题）：**

- analytical solution（解析解）：$x^\star = (A^T A)^{-1} A^T b$
- 可靠、高效的算法与软件
- 计算时间正比于 $n^2 k$（其中 $A \in \mathbb{R}^{k \times n}$）；若矩阵有结构（structured）则更快
- 一项成熟的技术（a mature technology）

**using least-squares（使用最小二乘）：**

- 最小二乘问题很容易从实际问题中识别出来（easy to recognize）
- 少数几种标准技巧即可大幅增加灵活性（例如：引入权重 weights、添加 regularization terms（正则化项））

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 解析解 $x^\star = (A^T A)^{-1} A^T b$ 正是 Strang 线代里的正规方程（normal equations）：把 $\|Ax-b\|_2^2 = (Ax-b)^T(Ax-b)$ 展开后对 $x$ 求梯度并置零即得。它为什么"可靠"？关键在矩阵 $A^T A$：只要 $A$ 列满秩，$A^T A$ 就是对称正定（positive definite）矩阵——这是 Strang 18.06 中正定性的核心应用场景，正定性保证逆矩阵存在、解唯一，而且从任何初始点出发都只有一个极小值点，不存在"卡在局部极小"的问题。但正定不等于好算：18.065 提醒过，当 $A$ 的条件数（condition number）很大时，显式构造 $A^T A$ 会把条件数平方放大，浮点舍入误差（可以回忆 CSAPP 讲浮点表示时"有效位有限"的教训）足以毁掉解的精度，因此成熟的数值软件改用 QR 分解或 Cholesky 分解路线而非真的求逆。这也正是"有结构时更快"的含义：若 $A$ 稀疏或呈带状，$n^2k$ 的复杂度还能进一步压缩。"成熟技术"四个字背后是 LAPACK 级别的数十年工程积累——后面学凸优化时，能把问题化成最小二乘，基本就等于已经解决了。

---

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">线性规划（Linear Programming）</span>

<!-- p.6 -->

```math
\begin{array}{ll}
\text{minimize} & c^T x \\
\text{subject to} & a_i^T x \le b_i, \quad i = 1, \ldots, m
\end{array}
```

**solving linear programs（求解线性规划）：**

- 没有解的解析公式（no analytical formula for solution）
- 可靠、高效的算法与软件
- 当 $m \ge n$ 时，计算时间正比于 $n^2 m$；有结构时更快
- 一项成熟的技术

**using linear programming（使用线性规划）：**

- 不像最小二乘问题那样容易识别
- 有一些把问题转化为线性规划的标准技巧（standard tricks），例如：涉及 $\ell_1$-norm（$\ell_1$ 范数）或 $\ell_\infty$-norm（$\ell_\infty$ 范数）的问题、piecewise-linear（分段线性）函数

---

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">凸优化问题（Convex Optimization Problem）</span>

<!-- p.7 -->

```math
\begin{array}{ll}
\text{minimize} & f_0(x) \\
\text{subject to} & f_i(x) \le b_i, \quad i = 1, \ldots, m
\end{array}
```

<span style="color:#00838f;">**[definition]**</span> <span style="color:#00838f;">目标函数与约束函数都是 convex（凸）函数：对一切满足 $\alpha + \beta = 1$、$\alpha \ge 0$、$\beta \ge 0$ 的 $\alpha, \beta$，都有</span>

```math
f_i(\alpha x + \beta y) \le \alpha f_i(x) + \beta f_i(y)
```

- 最小二乘问题与线性规划都是凸优化问题的特例（special cases）

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这一页的不等式就是凸函数（convex function）的定义本体：$\alpha+\beta=1$、$\alpha,\beta\ge0$ 意味着点 $\alpha x + \beta y$ 落在连接 $x$ 与 $y$ 的线段上，于是"函数在弦上任意点的值不超过两端点值的同权重插值"——几何图像是：函数图像上方任何一条弦都悬在曲线（面）之上，把 $f$ 想成一只碗，碗口总比架在碗沿的橡皮筋低。它与概率论里的 Jensen 不等式只差一步推广：从"两点凸组合"换成"随机变量的期望"，所以见到 Jensen 时不必当新知识学。与线代的连接尤其要紧：二次函数 $x^T P x + q^T x + r$ 是凸函数当且仅当 Hessian 矩阵 $P$ 半正定（positive semidefinite）——Strang 讲的半正定性在此从"矩阵性质"升格为"函数形状"的判据；而半定规划（SDP）恰好把这个判据反过来用，把"找一个凸函数"转化为"找一个半正定的矩阵变量"。凸性之所以是整个学科的分水岭，是因为它带来两条黄金性质：任何局部最优解就是全局最优解；温和条件下对偶间隙（duality gap）为零——这两点分别在本书第 4、5 章严格化。

---

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">求解凸优化问题（Solving Convex Optimization Problems）</span>

<!-- p.8 -->

**solving convex optimization problems（求解凸优化问题）：**

- 没有解析解（no analytical solution）
- 可靠、高效的算法（reliable and efficient algorithms）
- 计算时间（大致）正比于 $\max\{n^3,\, n^2 m,\, F\}$，其中 $F$ 是计算各 $f_i$ 及其一阶、二阶导数的代价
- 几乎成为一门技术（almost a technology）

**using convex optimization（使用凸优化）：**

- 往往难以识别（often difficult to recognize）
- 有许多把问题转化为凸形式（convex form）的技巧
- 令人惊讶地多的问题（surprisingly many problems）都能通过凸优化求解

---

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">实例：灯光照明问题（Example: Illumination）</span>

<!-- p.9 -->

<span style="color:#2471a3;">**[example]**</span> $m$ 盏灯（lamps）照亮 $n$ 块小而平坦的区域（small, flat patches）。原 slide 配有示意图：灯 $j$ 到区域 $k$ 的距离为 $r_{kj}$、光线入射角为 $\theta_{kj}$，各灯功率为 $p_j$，各区域接收的照度为 $I_k$。

区域 $k$ 上的 intensity（照度）$I_k$ 随灯功率 $p_j$ 线性变化（depends linearly on）：

```math
I_k = \sum_{j=1}^{m} a_{kj} p_j, \qquad a_{kj} = r_{kj}^{-2} \max\{\cos\theta_{kj},\, 0\}
```

problem（问题）：在灯功率有界的条件下，达到期望照度 $I_{\text{des}}$：

```math
\begin{array}{ll}
\text{minimize} & \displaystyle\max_{k=1,\ldots,n} \left| \log I_k - \log I_{\text{des}} \right| \\
\text{subject to} & 0 \le p_j \le p_{\max}, \quad j = 1, \ldots, m
\end{array}
```

（目标取对数差意味着追求的是相对误差——各区域照度不偏多也不偏少——并用最大值刻画"最差的那个区域"。）

---

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">如何求解？（How to Solve?）</span>

<!-- p.10 -->

how to solve（如何求解）？前四种都是"近似"思路：

1. **uniform power（均匀功率）**：取 $p_j = p$，扫描（vary）不同的 $p$ 值。

2. **least-squares（最小二乘）**：

   ```math
   \text{minimize} \quad \sum_{k=1}^{n} (I_k - I_{\text{des}})^2
   ```

   之后若 $p_j > p_{\max}$ 或 $p_j < 0$，就把 $p_j$ 修整（round）回边界内。

3. **weighted least-squares（加权最小二乘）**：

   ```math
   \text{minimize} \quad \sum_{k=1}^{n} (I_k - I_{\text{des}})^2 + \sum_{j=1}^{m} w_j (p_j - p_{\max}/2)^2
   ```

   迭代调整权重（weights）$w_j$，直到 $0 \le p_j \le p_{\max}$ 成立。

4. **linear programming（线性规划）**：

   ```math
   \begin{array}{ll}
   \text{minimize} & \displaystyle\max_{k=1,\ldots,n} \left| I_k - I_{\text{des}} \right| \\
   \text{subject to} & 0 \le p_j \le p_{\max}, \quad j = 1, \ldots, m
   \end{array}
   ```

   该问题可以经由线性规划精确求解。

当然，以上都是近似的（suboptimal，次优的）"解"（'solutions'）。

---

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">方法 5：凸优化（Approach 5: Convex Optimization）</span>

<!-- p.11 -->

5. **convex optimization（凸优化）**：原问题等价于（equivalent to）

   ```math
   \begin{array}{ll}
   \text{minimize} & f_0(p) = \displaystyle\max_{k=1,\ldots,n} h(I_k / I_{\text{des}}) \\
   \text{subject to} & 0 \le p_j \le p_{\max}, \quad j = 1, \ldots, m
   \end{array}
   ```

   其中 $h(u) = \max\{u,\, 1/u\}$。

   （原 slide 配有 $h(u)$ 的函数图像：横轴 $u$ 从 0 到 4，纵轴 $h(u)$ 从 0 到 5，曲线呈"对勾"形——左侧是 $1/u$ 分支、右侧是 $u$ 分支，在 $u=1$ 处取到最小值 1。）

   $f_0$ 是凸的，因为凸函数的逐点最大值（pointwise maximum of convex functions）仍是凸函数。

   exact solution（精确解）：所需工作量 ≈ 一个适中的倍数（modest factor）× 最小二乘方法的工作量。

---

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">附加约束：直觉的边界（Additional Constraints）</span>

<!-- p.12 -->

additional constraints（附加约束）：在原问题上再加入下面 1 或 2 中的约束，会让问题变复杂吗？

1. 任意 10 盏灯消耗的功率不超过总功率的一半（no more than half of total power is in any 10 lamps）
2. 点亮的灯不超过总数的一半（no more than half of the lamps are on，即 $p_j > 0$ 的灯数不超过一半）

- answer（答案）：加入 (1)，问题仍然容易求解；加入 (2)，问题变得极其困难（extremely difficult）
- moral（寓意）：（未经训练的）直觉并不总是管用；缺乏相应背景时，非常容易的问题可能看起来与非常困难的问题极为相似

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 为什么约束 (1)"任意 10 盏灯功率不超过总功率一半"不破坏凸性，而约束 (2)"点亮灯数不超过一半"却让问题变得极难？判据只有一个：约束对应的可行集是不是凸集。(1) 可以写成一组关于 $p$ 的线性不等式——"任意 10 盏"无非枚举出 $\binom{m}{10}$ 条形如 $\sum_{j \in S} p_j \le \tfrac{1}{2}\sum_j p_j$ 的线性约束——线性不等式的交集仍是凸集，问题还留在凸优化的舒适区内，只是约束条数变多而已。(2) 本质上是在对布尔事件 $p_j > 0$ 计数："至多一半的分量非零"对应的集合在 $\mathbb{R}^m$ 中裂成不连通的若干块，计数函数是整值的、非凸的，可行集瞬间失去凸性，问题滑回组合优化（combinatorial optimization）领域，最坏情况只能逼近指数枚举——这正是 6.006 里"NP 难问题没有已知多项式时间算法"的那一类。这就是"识别凸性"的第一课：看约束不能只看文字多简单，要看它在 $\mathbb{R}^m$ 里的几何形状；后续章节会系统整理哪些运算保持凸性、哪些一碰就毁。这页的"寓意"其实是整门课的动机：直觉需要被凸分析重新训练。

---

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">课程目标与主题（Course Goals and Topics）</span>

<!-- p.13 -->

**goals（课程目标）：**

1. recognize/formulate problems（识别/建模问题）：把诸如照明问题这类问题表述（formulate）为凸优化问题
2. 为中等规模（moderate size）的问题开发代码——例如 1000 盏灯、5000 块区域
3. characterize optimal solution（刻画最优解）：给出最优功率分布、性能的极限（limits of performance）等

**topics（课程主题）：**

1. 凸集（convex sets）、凸函数（convex functions）、凸优化问题
2. 例子与应用（examples and applications）
3. 算法（algorithms）

---

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">非线性优化（Nonlinear Optimization）</span>

<!-- p.14 -->

针对一般非凸（nonconvex）问题，传统技术都需要某种妥协（compromise）。

**local optimization methods（局部优化方法，即 nonlinear programming（非线性规划））：**

- 在其附近的可行点（feasible points）中寻找使 $f_0$ 最小的点
- 速度快，能处理大规模问题
- 需要初始猜测（initial guess）
- 无法提供任何关于"距离（全局）最优值还有多远"的信息

**global optimization methods（全局优化方法）：**

- 能找到（全局）解
- 最坏情况复杂度（worst-case complexity）随问题规模指数增长（grows exponentially）

这些算法通常基于求解凸子问题（convex subproblems）。

---

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">凸优化简史（Brief History of Convex Optimization）</span>

<!-- p.15 -->

**theory（理论）**，即 convex analysis（凸分析）：约 1900–1970 年

**algorithms（算法）：**

- 1947 年：线性规划的 simplex algorithm（单纯形算法）（Dantzig）
- 1960 年代：早期的 interior-point methods（内点法）（Fiacco & McCormick、Dikin 等）
- 1970 年代：ellipsoid method（椭球法）与其他 subgradient methods（次梯度方法）
- 1980 年代：线性规划的多项式时间（polynomial-time）内点法（Karmarkar 1984）
- 1980 年代末至今：非线性凸优化的多项式时间内点法（Nesterov & Nemirovski 1994）

**applications（应用）：**

- 1990 年以前：主要集中于运筹学（operations research），工程界很少
- 1990 年以来：工程领域涌现大量新应用（控制、信号处理、通信、电路设计等）；并出现新的问题类（problem classes）：semidefinite programming（半定规划，SDP）与 second-order cone programming（二阶锥规划，SOCP）、robust optimization（鲁棒优化）

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这条时间线值得逐个记住，因为后续章节会反复回到这些锚点。1947 年 Dantzig 提出单纯形算法时，"凸优化"作为学科尚未成形；单纯形法实务上极快，但它的指数复杂度反例直到 1972 年才由 Klee 与 Minty 构造出来——"实践快但理论差"这道裂缝，正是 Khachiyan 1979 年椭球法（多项式时间却实用缓慢）与 Karmarkar 1984 年内点法（多项式时间且实用快速）相继登场的舞台。用 6.006 的语言说，这是"复杂度理论反过来重塑一个应用学科"的经典案例：多项式时间可解性（polynomial-time solvability）把"能解"从经验升级为可证明的保证。1994 年 Nesterov 与 Nemirovski 借助 self-concordance（自和谐）理论把内点法推广到所有凸优化问题，而工程界恰好从 1990 年起涌入控制、信号处理、电路设计等新应用——应用大爆发踩在理论成熟点之后，绝非巧合。本课程只讲"现代"部分：如何把问题建模成凸问题、以及内点法能高效求解的这一大类问题；历史脉络知道这条主线即可。
