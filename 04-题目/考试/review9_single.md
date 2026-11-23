# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

> <span style="color:#7f8c8d;">Source: SEE复习课/review9_single.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

## <span style="color:#c0392b;">**EE364a 期末复习课（EE364a Final Review Session）**</span>

### <span style="color:#c0392b;">**封面与大纲 Cover and outline**</span>

<!-- p.1 -->

<span style="color:#2471a3;">**[lecture]**</span> **EE364a Review**：EE364a 期末复习课（EE364a Final Review Session）。

本次复习课的大纲 session outline：

- optimizing processor speed（优化处理器速度）
- ℓ1.5 optimization（$\ell_{1.5}$ 优化）
- brief course overview（课程简要回顾）

### <span style="color:#c0392b;">**优化处理器速度 Optimizing processor speed**</span>

<!-- p.2 -->

<span style="color:#2471a3;">**[section]**</span> 问题设定：

- 一组共 $n$ 个任务（tasks）要交给 $n$ 个处理器（processors）计算；
- 处理器的功耗为 $f(s_i)$，其中 $s_{\min}\le s_i\le s_{\max}$ 是处理器的速度（speed）；
- 任务 $i$ 在时间 $\tau_i=\alpha_i/s_i$ 内完成；
- 处理器总能量为

```math
E=\sum_i (\alpha_i/s_i)\,f(s_i)
```

- 先后约束集（precedence constraint set）$P\subseteq\{1,\dots,n\}\times\{1,\dots,n\}$，用一个有向无环图（directed acyclic graph, DAG）描述，例如：图中 6 个节点对应 6 个任务，有向边表示"前一任务完成后一任务才能开始"的依赖关系（具体连边见原 slide 图示）。

### <span style="color:#c0392b;">**问题陈述 Problem statement**</span>

<!-- p.3 -->

<span style="color:#2471a3;">**[question]**</span>（按规范，题目部分保留英文原文）

problem:

1. formulate the problem of minimizing completion time T subject to E ≤ Emax as a convex optimization problem
2. generate the optimal tradeoff curve of E versus T for f(s) = 1 + s + s² + s³

issues:

- how do we deal with E ≤ Emax?
- how do we deal with precedence constraints?

### <span style="color:#c0392b;">**能量约束 Energy constraint**</span>

<!-- p.4 -->

<span style="color:#2471a3;">**[solution]**</span>

> 能量函数（energy function）$E=\sum_i (\alpha_i/s_i)f(s_i)$ 一般情况下关于速度 $s$ 并不是凸的（not convex in general）。
>
> 改用完成时间 $\tau_i=\alpha_i/s_i$ 把 $E$ 改写为
>
> ```math
> E=\sum_{i=1}^{n}\tau_i f(\alpha_i/\tau_i)
> ```
>
> 它是凸的——因为它是 $f$ 的 perspective function（透视函数）；而速度约束也随之变成时间约束：
>
> ```math
> \alpha_i/s_{\max}\le \tau_i\le \alpha_i/s_{\min},\qquad i=1,\dots,n
> ```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 透视函数是这一页真正的"魔法"。直观地看，$t f(x/t)$ 把 $f$ 的上图（epigraph）沿从原点出发的射线做缩放：当 $t>0$ 时，$(x,t)$ 落在 $g$ 的上图当且仅当 $(x/t,1)$ 落在 $f$ 的上图，因此凸性在换元后自动保留。其实你在 Bertsekas 的概率论里早见过它：相对熵 $t\log(t/x)$ 正是 $\log$ 的透视函数，KL 散度与交叉熵损失都由这个运算生成；最大似然估计中"$n$ 乘以平均负对数似然"也是同一结构。本题还有一层建模方法论：直接看 $(\alpha_i/s_i)f(s_i)$ 凸性不明，换成 $\tau_i$ 后凸性立刻"显形"——先把变量选对，再谈求解算法。这是凸建模的第一性原理：凸性往往不是问题固有的，而是你表述它的方式赋予的。

### <span style="color:#c0392b;">**完工时间 Completion time**</span>

<!-- p.5 -->

<span style="color:#2471a3;">**[solution]**</span>

> 引入变量 $t$：让 $t_i$ 充当任务 $i$ 完工时间（completion time）的上界（upper bound）；按此构造，整体完工时间满足 $T\le \max_i t_i$。
>
> 于是先后约束（precedence constraints）可以表示为
>
> ```math
> t_j\ge t_i+\tau_j,\qquad (i,j)\in P
> ```
>
> 以及
>
> ```math
> t_i\ge \tau_i,\qquad i=1,\dots,n
> ```

### <span style="color:#c0392b;">**凸形式 Convex formulation**</span>

<!-- p.6 -->

<span style="color:#2471a3;">**[solution]**</span>

> 汇总成一个凸优化问题（convex optimization problem）：
>
> ```math
> \begin{array}{ll}
> \text{minimize} & \max_i t_i\\
> \text{subject to} & \displaystyle\sum_{i=1}^{n}\tau_i f(\alpha_i/\tau_i)\le E_{\max}\\
> & \alpha_i/s_{\max}\le \tau_i\le \alpha_i/s_{\min},\quad i=1,\dots,n\\
> & t_i\ge \tau_i,\quad i=1,\dots,n\\
> & t_j\ge t_i+\tau_j,\quad (i,j)\in P
> \end{array}
> ```
>
> - 变量（variables）是 $t$ 与 $\tau$；
> - 能量约束是凸的（convex）；
> - 先后约束是仿射的（affine）；
> - 所以整个问题是凸的（convex）。

### <span style="color:#c0392b;">**最优权衡曲线 Optimal tradeoff curve**</span>

<!-- p.7 -->

<span style="color:#2471a3;">**[section]**</span> 纯图示 slide：横轴为完工时间 Time（范围 0–20），纵轴为能量 Energy（范围 0–4000），图中给出 $f(s)=1+s+s^2+s^3$ 情形下 $E$ 与 $T$ 的最优权衡曲线（optimal tradeoff curve），曲线具体形状见原 slide 图示。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这条曲线就是 Pareto 意义下的最优权衡曲线：曲线上每个点表示"在能量不超过 $E$ 的前提下可达到的最小完工时间"，任何一点都无法同时改进两个目标。生成它的标准武器是拉格朗日标量化（Lagrangian scalarization）：对 $\min\; T+\lambda E$ 扫描乘子 $\lambda\ge 0$，只要强对偶性（strong duality）成立，就能扫出整条曲线——这正是课程对偶一章最重要的应用之一。你在概率论里见过同构的折中结构：估计量的均方误差可分解为偏差²加方差，调节正则化强度就是在两项代价之间选点；Strang 在 18.065 中讲的岭回归 $\|Ax-b\|_2^2+\lambda\|x\|_2^2$ 是同一件事的最简实例。掌握"多目标 → 单目标 + 乘子"这一步转换，是读懂几乎所有工程权衡分析的钥匙。

### <span style="color:#c0392b;">**ℓ1.5 优化 ℓ1.5 optimization**</span>

<!-- p.8 -->

<span style="color:#2471a3;">**[question]**</span>（按规范，题目部分保留英文原文）考虑最小化 $\ell_{1.5}$ 范数误差：

```math
\text{minimize}\qquad \|Ax-b\|_{1.5}=\Big(\sum_{i=1}^{m}\left|a_i^T x-b_i\right|^{3/2}\Big)^{2/3}
```

problem:

1. give simple optimality conditions for this problem
2. formulate this problem as an SDP

### <span style="color:#c0392b;">**最优性条件 Optimality conditions**</span>

<!-- p.9 -->

<span style="color:#2471a3;">**[solution]**</span>

> 等价问题（equivalent problem）：最小化
>
> ```math
> f(x)=\sum_{i=1}^{m}\left|a_i^T x-b_i\right|^{3/2}
> ```
>
> （与原问题只差一个单调的 $2/3$ 次幂，最优点相同。）
>
> - 目标函数可微（differentiable）；
> - 因此可以使用一阶最优性条件（first order optimality conditions）：
>
> ```math
> \nabla f(x)=\sum_{i=1}^{m}\frac{3}{2}\,\operatorname{sgn}\!\left(a_i^T x-b_i\right)\left|a_i^T x-b_i\right|^{1/2}a_i=0
> ```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> $\ell_{1.5}$ 恰好卡在平滑度的微妙边界上。单项 $|u|^{3/2}$ 的一阶导数 $\tfrac{3}{2}\operatorname{sgn}(u)|u|^{1/2}$ 在 $u=0$ 处连续，所以目标是 $C^1$ 的、一阶条件 $\nabla f=0$ 名正言顺；但二阶导数 $\tfrac{3}{4}|u|^{-1/2}$ 在 $u=0$ 附近爆炸，而最优解附近恰恰常有大量残差为零的分量，海森矩阵（Hessian）在那里无界。这意味着把牛顿法（Newton's method）直接套上去，步长会在零残差区域病态化——这正是教材专门引入自和谐（self-concordance）来分析牛顿法真实收敛行为的动机之一。这一点与 CSAPP 的经验遥相呼应：浮点表示误差会在病态问题上被放大；光滑性差、数值条件差、数值稳定性随之下降，所以连续优化的算法选择必须先看问题的微分结构。

### <span style="color:#c0392b;">**SDP 形式 SDP formulation**</span>

<!-- p.10 -->

<span style="color:#2471a3;">**[solution]**</span>

> 等价问题（equivalent problem）：
>
> ```math
> \begin{array}{ll}
> \text{minimize} & \mathbf{1}^T t\\
> \text{subject to} & s_i^{3/2}\preceq t_i,\\
> & -s_i\preceq a_i^T x-b_i\preceq s_i,\qquad i=1,\dots,m
> \end{array}
> ```
>
> - 变量（variables）为 $x\in\mathbf{R}^n$ 以及 $s,\,t\in\mathbf{R}^m$；
> - 问题是凸的（convex），但还不是 SDP；
> - 需要把约束 $s_i^{3/2}\preceq t_i$ 变换成 LMI（linear matrix inequality，线性矩阵不等式）。

### <span style="color:#c0392b;">**LMI 变换 LMI transformation**</span>

<!-- p.11 -->

<span style="color:#2471a3;">**[solution]**</span>

> 利用如下事实：
>
> ```math
> \begin{bmatrix}u&v\\ v&w\end{bmatrix}\succeq 0
> \quad\Longleftrightarrow\quad u\ge 0,\ \ uw\ge v^2
> ```
>
> 约束 $s_i^{3/2}\le t_i$ 等价于
>
> ```math
> \begin{bmatrix}\sqrt{s_i}&s_i\\ s_i&t_i\end{bmatrix}\succeq 0
> ```
>
> 而后者又等价于下面这对 LMI：
>
> ```math
> \begin{bmatrix}y_i&s_i\\ s_i&t_i\end{bmatrix}\succeq 0,
> \qquad
> \begin{bmatrix}s_i&y_i\\ y_i&1\end{bmatrix}\succeq 0
> ```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这页的判据你在 Strang 线代里早就握有钥匙：$2\times 2$ 对称矩阵 $\begin{bmatrix}u&v\\ v&w\end{bmatrix}$ 半正定当且仅当顺序主子式 $u\ge 0$ 与行列式 $uw-v^2\ge 0$ 同时成立——这正是正定性主子式判别法的半正定版本。用 Schur complement（舒尔补）的语言说：当 $u>0$ 时它等价于 $w-v^2/u\ge 0$，代回 $u=\sqrt{s_i}$ 恰好还原 $t_i\ge s_i^{3/2}$。第二个矩阵则是"提升（lifting）"技巧：不直接处理根号，而是引入新变量 $y_i$，用 $\begin{bmatrix}s_i&y_i\\ y_i&1\end{bmatrix}\succeq 0$ 强制 $s_i\ge y_i^2\ge 0$，再由 $\begin{bmatrix}y_i&s_i\\ s_i&t_i\end{bmatrix}\succeq 0$ 得到 $y_i t_i\ge s_i^2$，两式联立恰好等价于 $t_i\ge s_i^{3/2}$。把"非线性约束"升维成"更高维空间里的线性矩阵约束"，是半定规划建模的核心思想，SOS（sum of squares，平方和）方法把这一思想推广到了任意多项式。

### <span style="color:#c0392b;">**SDP 形式（汇总）SDP formulation**</span>

<!-- p.12 -->

<span style="color:#2471a3;">**[solution]**</span>

> 把所有部分合到一起（putting it all together）：
>
> ```math
> \begin{array}{ll}
> \text{minimize} & \mathbf{1}^T t\\
> \text{subject to} & -s_i\preceq a_i^T x-b_i\preceq s_i,\qquad i=1,\dots,m
> \end{array}
> ```
>
> 以及
>
> ```math
> \begin{bmatrix}y_i&s_i\\ s_i&t_i\end{bmatrix}\succeq 0,
> \qquad
> \begin{bmatrix}s_i&y_i\\ y_i&1\end{bmatrix}\succeq 0,
> \qquad i=1,\dots,m
> ```
>
> - 这是一个以 $x,\ s,\ t,\ y$ 为变量的 SDP（semidefinite program，半定规划）；
> - 同样的技巧可以用于其他涉及多项式（polynomials）的问题；
> - 可参见关于 SOS（sum of squares，平方和）方法的文献。

### <span style="color:#c0392b;">**课程简要回顾 Brief course overview**</span>

<!-- p.13 -->

<span style="color:#2471a3;">**[section]**</span> 这门课你学到了什么（what have you learned）？

- theory（理论）
- applications（应用）
- algorithms（算法）

### <span style="color:#c0392b;">**理论 Theory**</span>

<!-- p.14 -->

<span style="color:#2471a3;">**[section]**</span>

- convex sets（凸集）与 convex functions（凸函数）
- operations that preserve convexity（保持凸性的运算）
- convex optimization problems（凸优化问题）
- duality（对偶性）

### <span style="color:#c0392b;">**应用 Applications**</span>

<!-- p.15 -->

<span style="color:#2471a3;">**[section]**</span>

- approximation and fitting（逼近与拟合）
  - least-norm problems（最小范数问题）
  - robust approximation（鲁棒逼近）
  - function fitting（函数拟合）
- statistical estimation（统计估计）
  - parametric estimation（参数估计）
  - optimal detector design（最优检测器设计）
  - experiment design（实验设计）
- geometric problems（几何问题）
  - classification（分类）
  - placement problems（布局问题）
  - floor planning（楼层规划）

此外还有许多其他应用（many others）。

### <span style="color:#c0392b;">**算法 Algorithms**</span>

<!-- p.16 -->

<span style="color:#2471a3;">**[section]**</span>

- exploiting structure（利用结构）
- unconstrained minimization（无约束极小化）
  - gradient descent（梯度下降）
  - steepest descent（最速下降）
  - Newton's method（牛顿法）
- equality constrained Newton's method（等式约束牛顿法）
- interior-point methods（内点法）

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这页清单背后，是一段与复杂度理论交织的历史。1979 年 Khachiyan 用椭球法（ellipsoid method）证明线性规划可在多项式时间内求解，回答了"连续优化是否像 6.006 里的图问题一样属于 P 类"；1984 年 Karmarkar 提出内点法，实测速度可与单纯形法抗衡甚至超越，掀起了"内点法热潮"；1990 年代初 Nesterov 与 Nemirovski 建立自和谐（self-concordance）理论，把内点法推广到含半定规划在内的一大类凸问题，并给出多项式迭代次数界。以 6.006 的视角看，区别在于：那边衡量的是组合对象上的离散操作数，这边按"迭代次数 × 单次迭代的数值代价"（如 Cholesky 分解的 $O(n^3)$）来计复杂度——但"多项式时间可解"这条底线，用的是同一种语言。

### <span style="color:#c0392b;">**结语 Final comment**</span>

<!-- p.17 -->

<span style="color:#2471a3;">**[section]**</span> 祝你考试顺利（good luck on your exam）！
