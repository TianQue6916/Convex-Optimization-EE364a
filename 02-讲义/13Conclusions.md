> <span style="color:#7f8c8d;">Source: SEE官方讲义/13Conclusions.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

## <span style="color:#c0392b;">13. Conclusions（结论）</span>

<!-- p.1 -->

### <span style="color:#c0392b;">13–1 · Conclusions（结论）：全讲导览</span>

<span style="color:#2471a3;">**[section]**</span> 这是全课程的最后一讲（讲次封面页），共 6 页。本讲将总结：

- 课程的主要思想（main ideas of the course，课程主线思想）
- 建模（modeling）在优化中的重要性

<!-- p.2 -->

### <span style="color:#c0392b;">13–2 · Modeling（建模）</span>

<span style="color:#2471a3;">**[section]**</span> <span style="color:#00838f;">**数学优化**（mathematical optimization）</span>

- 工程设计、数据分析与统计（statistics，统计学）、经济学（economics）、管理学（management）等领域中的问题，往往可以表述为数学优化问题（optimization problem）
- 已有相关技术可以处理**多目标**（multiple objectives）或数据中的**不确定性**（uncertainty）

<span style="color:#2471a3;">**[section]**</span> <span style="color:#00838f;">**可解性**（tractability）</span>

- 粗略地说，优化中的可解性（tractability）要求**凸性**（convexity）
- 非凸优化（nonconvex optimization）的算法要么只能找到**局部（次优）解**（local, suboptimal solutions），要么计算代价极高
- 令人惊讶的是，数量惊人的应用都可以**表述为凸问题**（convex problems）

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 「粗略地说，可解性要求凸性」这句话值得用你在 6.006 里建立的复杂度直觉来精确化。严格的陈述是：若希望一族优化问题在多项式时间内解到任意精度，凸性是最自然的充分条件；而一般形式的非凸问题（例如含整数变量的规划）是 NP-难的，不存在已知的多项式算法。你在 6.006 学过的最大流与最短路本质上都是 LP（线性规划）的特例——「LP↔网络流/最短路」正是这条可解性链条上的具体环节。更实用的是第三条：很多看似非凸的工程问题（滤波器设计、投资组合、资源分配）经过变量替换或松弛之后其实是凸的。所以动手求解之前应当先问「这个问题能不能写成凸的」，而不是直接套用梯度下降或启发式搜索——这正是本讲把「建模」放在「算法」之前讲的原因。

<!-- p.3 -->

### <span style="color:#c0392b;">13–3 · 凸性的理论后果（Theoretical consequences of convexity）</span>

<span style="color:#2471a3;">**[section]**</span> 凸性带来的理论后果：

- **局部最优即全局最优**（local optima are global）
- **内容丰富的对偶理论**（duality theory）：
  - 系统地推导**最优值下界**（lower bounds on optimal value）的方法
  - 必要且充分的**最优性条件**（optimality conditions）
  - **不可行性证书**（certificates of infeasibility）
  - **灵敏度分析**（sensitivity analysis）
- 具有**多项式最坏情形复杂度**（polynomial worst-case complexity）的求解方法理论（借助自和谐 self-concordance 建立）

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 「局部最优即全局最优」可以从第一性原理看清楚：凸函数满足 $f(\theta x+(1-\theta)y)\le\theta f(x)+(1-\theta)f(y)$，于是若存在比局部最优点 $x^\star$ 更好的点 $y$，沿两点连线的整条线段上函数值都不劣于线性插值，在靠近 $x^\star$ 处就会严格更优，这与 $x^\star$ 的局部最优性矛盾，故这样的 $y$ 不存在。对偶理论你其实已经见过具体实例：Strang 讲 SVD 与 Rayleigh 商时，最大特征值写成 $\max\, x^T A x$ 在 $\|x\|=1$ 上的最紧上界，这正是「对偶给出最优值下界」思想的雏形；6.006 学过的最大流–最小割定理则是强对偶（strong duality）在 LP 上的典型体现。不可行性证书对应 Farkas 引理式的分离证据：不仅判定「无解」，还附上一张可供验证的凭据，这在工程评审与算法验证中都极有价值。

<!-- p.4 -->

### <span style="color:#c0392b;">13–4 · 凸性的实践后果（Practical consequences of convexity）</span>

<span style="color:#2471a3;">**[section]**</span> （绝大多数）凸问题可以**全局且高效**地求解：

- **内点法**（interior-point method）在实践中只需 20–80 步迭代
- 基本算法（例如牛顿法 Newton、障碍法 barrier method 等）易于实现，对中小规模问题效果良好（若利用问题的结构，可处理更大规模的问题）
- 先进算法与建模工具的高质量实现正变得越来越多
- cvx 这类高级建模工具让建模与问题表述更加容易

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 「内点法实践中 20–80 步收敛」这个数字，对学过 CSAPP 的你还有数值层面的含义。每一步原对偶（primal-dual）迭代都要解一个 KKT 线性方程组，随着逼近中心路径（central path）末端，系数矩阵逐渐病态、条件数（condition number）急剧增大，求解器通常用 Cholesky 分解加正则化来稳住——这正是 CSAPP 讲浮点表示误差时埋下的伏笔：$10^{-16}$ 量级的舍入误差经病态矩阵放大会毁掉全部精度。另外，牛顿步要求 Hessian（或其近似）正定，这一前提完全由凸性兜底；Strang 线代里的正定判据（$x^T A x>0$、各阶主子式为正、特征值全为正）就是它背后的代数地基。历史上，Karmarkar 在 1984 年提出内点法，Nesterov 与 Nemirovski 在 1988–1994 年间建立自和谐（self-concordance）理论，二者共同解释了「步数几乎与问题规模无关」这一经验事实。

<!-- p.5 -->

### <span style="color:#c0392b;">13–5 · 如何使用凸优化（How to use convex optimization）</span>

<span style="color:#2471a3;">**[section]**</span> 要在某个应用场景中使用凸优化：

- **快速原型与近似建模**（rapid prototyping, approximate modeling）：
  - 从简单模型、小规模问题实例、低效的求解方法起步
  - 如果你对结果不满意，就不必再为更精确的模型或更高效的算法投入精力
- 推导、化简并解读**最优性条件**与**对偶**（dual）
- 即使问题相当**非凸**（nonconvex），你也可以使用凸优化：
  - 用于**子问题**（subproblems），例如寻找搜索方向（search direction）
  - 在当前点反复构造并求解一个**凸近似**（convex approximation）

<!-- p.6 -->

### <span style="color:#c0392b;">13–6 · 进阶主题（Further topics）</span>

<span style="color:#2471a3;">**[section]**</span> 本课程未涵盖的一些主题：

- 超大规模问题的求解方法
- 次梯度演算（subgradient calculus）、凸分析（convex analysis）
- 局部化（localization）、次梯度（subgradient）及相关方法
- 分布式凸优化（distributed convex optimization）
- 建立在凸优化之上、或使用凸优化的各类应用

……这些内容将在下学期的 EE364B 中讲授。

> <span style="color:#7f8c8d;">Slide 页脚「Conclusions 13–1 至 13–6」为原 PDF 页脚编号，已并入各节标题；本讲为课程总结，共 6 页，至此 EE364A 讲义全部结束。</span>
