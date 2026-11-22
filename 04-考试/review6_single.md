> <span style="color:#7f8c8d;">Source: SEE复习课/review6_single.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

## EE364a Review Session 6（EE364a 复习课 6）

<!-- p.1 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**本讲主题（topics）**</span>

<span style="color:#2471a3;">**[lecture]**</span> 本讲为 EE364a 复习课第 6 讲（EE364a Review Session 6），主题（topics）：

- 高度量化测量下的最大似然预测（ML prediction with highly quantized measurements）
- 双向划分（two-way partitioning）

<!-- p.2 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**量化测量下的估计（Estimation with quantized measurements）**</span>

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">**已知（given）：**</span>

- 信号矩阵（signal matrix）$A \in \mathbb{R}^{m \times n}$；
- 测量值 $y = \phi(Ax + v)$（量化器 $\phi$ 按分量作用），其中噪声 $v \sim \mathcal{N}(0, \sigma^2 I)$，且每个 $\phi_i : \mathbb{R} \to \{1, \ldots, K\}$ 满足 $\phi_i^{-1}(k) = (t_k, t_{k+1}]$；
- 量化门限（quantization levels）$-\infty = t_1 < t_2 < t_3 < \cdots < t_K < t_{K+1} = \infty$。

<span style="color:#2471a3;">**[question]**</span> 任务：计算 $\hat{x}$——即在给定 $y$ 的条件下，$x$ 的最大似然估计（maximum likelihood estimate）。

<!-- p.3 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**量化测量下的估计（续）（Estimation with quantized measurements）**</span>

示意图给出信号链：$x$ 经信号矩阵 $A$ 线性映射、叠加噪声 $v$，再经量化器 $\phi$ 得到测量 $y$（图中依次标注 $A$、$x$、$v$、$y$）。

<span style="color:#2471a3;">**[question]**</span> 你会如何求 $\hat{x}$（how would you find $\hat{x}$）？

- 无噪声也无量化时（$v = 0$ 且 $\phi(z) = z$）？
- 有噪声但无量化时（$\phi(z) = z$）？
- 无噪声但有量化时（$v = 0$）？

<!-- p.4 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**似然与对数似然（Likelihood and log-likelihood）**</span>

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">**似然（likelihood）：**</span>

```math
p(y \mid x) = \prod_{i=1}^{m} \left( \Phi\!\left( \frac{t_{y_i+1} - (Ax)_i}{\sigma} \right) - \Phi\!\left( \frac{t_{y_i} - (Ax)_i}{\sigma} \right) \right)
```

<span style="color:#00838f;">**对数似然（log-likelihood）：**</span>

```math
l_y(x) = \sum_{i=1}^{m} \log\!\left( \Phi\!\left( \frac{t_{y_i+1} - (Ax)_i}{\sigma} \right) - \Phi\!\left( \frac{t_{y_i} - (Ax)_i}{\sigma} \right) \right)
```

其中 $\Phi$ 是标准正态分布（standard normal distribution）的累积分布函数（cdf）。

- $l_y(x)$ 是凹的（concave），且二阶可微（twice differentiable）。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 为什么对数似然 $l_y(x)$ 是凹函数？拆开看单项：测量 $y_i = k$ 的似然是高斯噪声落入区间 $(t_k, t_{k+1}]$ 的概率，即两个 cdf 之差 $\Phi((t_{k+1} - (Ax)_i)/\sigma) - \Phi((t_k - (Ax)_i)/\sigma)$。概率论中的经典结果（Prékopa 的对数凹性理论）说：高斯区间概率取对数后关于区间端点是联合凹的；再做仿射复合 $x \mapsto (Ax)_i$ 与逐项求和（凹函数之和仍凹），凹性就传递到整体——这正是你在 Strang 线代与 18.065 中见过的「仿射结构不破坏凹凸性」这一保凸运算思想的直接应用。这一步是从概率论（Bertsekas）的 cdf 与最大似然通往凸优化「最大化凹函数」的关键桥梁：最大化凹函数等价于最小化凸函数，估计问题由此落入凸优化框架。换个角度看，量化把连续测量压成 $K$ 个离散档位，与 CSAPP 中有限字长把实数压到可表示集合是同一类操作，只不过这里的「舍入区间」由门限 $t_k$ 显式给出。$\sigma$ 越小，区间概率越集中，下一页图中的对数似然曲面也越陡峭。

<!-- p.5 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**区间对数正态 cdf（Interval log-normal cdf）**</span>

<span style="color:#2471a3;">**[example]**</span> 图示为函数 $f(x) = \log(\Phi((x+1)/\sigma) - \Phi((x-1)/\sigma))$ 在 $\sigma = 0.3$ 下的图像：横轴 $x$ 约从 $-3$ 到 $3$，纵轴约从 $-30$ 到 $10$，曲线整体呈凹形，直观展示了「单区间对数 cdf」的凹性。

<!-- p.6 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**最大似然估计（ML estimation）**</span>

<span style="color:#2471a3;">**[algorithm]**</span> 求解以下优化问题：

```math
\text{maximize} \quad l_y(x)
```

- 这是一个凸的（convex）、无约束（unconstrained）优化问题（optimization problem）；
- 可以用牛顿法（Newton's method）高效求解（下一主题）。

扩展（extensions）：

- MAP（最大后验估计，maximum a posteriori）：给 $x$ 加先验分布（prior distribution）；
- 对 $x$ 的先验约束（prior constraints）。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 凸且无约束，为什么课上点名牛顿法（Newton's method）？从第一性原理看：最大化凹的 $l_y(x)$ 等价于最小化凸的 $-l_y(x)$，后者的 Hessian 处处正定，牛顿方程的系数矩阵良态，可以用 Cholesky 分解稳定求解——这正是 Strang 线代中「正定 ⇒ Cholesky 分解存在且数值稳定」的直接应用。与梯度法（gradient method）相比，牛顿法在最优解附近接近二阶收敛：误差每步近似平方，而梯度法每步只按固定比例收缩，因此牛顿法所需迭代步数少得多；代价是每步要组装并分解 Hessian。本例中 $n = 10$、$m = 200$，Hessian 只有 $10 \times 10$，每步代价可忽略，牛顿法是显然赢家。顺带一提，$\log(\Phi(\cdot) - \Phi(\cdot))$ 这类「对数 cdf 差」结构与障碍函数（barrier function）同属对数型凹函数，课件随后把牛顿法列为下一主题，正是为这类函数的自和谐（self-concordance）分析做铺垫。

<!-- p.7 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**数值例子（Numerical example）**</span>

<span style="color:#2471a3;">**[example]**</span> 问题实例（problem instance）：

- $n = 10$ 个变量，$m = 200$ 个测量值；
- 门限 $-\infty, -1, +1, \infty$（3 个区间 ≈ 每次测量 1.6 比特）；
- $A_{ij} \sim \mathcal{N}(0, 1)$。

仿真（simulation）步骤：

- 让 $\sigma$ 从 0.1 变到 3；
- 生成 100 组 $x, y$ 的取值，其中 $x \sim \mathcal{N}(0, I)$；
- 计算 $\hat{x}$；
- 评估相对估计误差 $\|\hat{x} - x\|_2 / \|x\|_2$。

<!-- p.8 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**结果（Results）**</span>

<span style="color:#2471a3;">**[example]**</span> 图示为相对估计误差 $\|\hat{x} - x\|_2 / \|x\|_2$ 随噪声标准差 $\sigma$（横轴，约 0.5 至 3）变化的仿真结果，纵轴范围约 0.1 至 0.6。虚线（dashed）为最大似然（ML）估计；实线（solid）为最小二乘（least-squares）估计，其中量化测量值直接取为 $y_i \in \{-2, 0, +2\}$。

<!-- p.9 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**双向划分问题（Two-way partitioning）**</span>

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">**问题描述：**</span>

- $n$ 个顶点，标号为 $\{1, \ldots, n\}$；
- 给定顶点对上的一组对称权重 $w_{ij} = w_{ji}$；
- 求顶点的一个划分（partition）$(Y, Z)$（即 $Y \cup Z = \{1, \ldots, n\}$，$Y \cap Z = \emptyset$），使割（cut）的总权重最大：

```math
J(Y, Z) = \sum_{i \in Y} \sum_{j \in Z} w_{ij}
```

- 用 $x \in \{-1, 1\}^n$ 编码划分；$x_i = -1$ 表示顶点 $i \in Y$；
- 此时 $J(x) = \mathbf{1}^T W \mathbf{1} - x^T W x$。

<!-- p.10 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**双向划分的优化形式（Two-way partitioning）**</span>

该问题可以写成（cast as）：

```math
\begin{array}{ll}
\text{minimize}   & x^T W x \\
\text{subject to} & x_i^2 = 1, \quad i = 1, \ldots, n
\end{array}
```

或等价地（or equivalently）：

```math
\begin{array}{ll}
\text{minimize}   & \operatorname{tr}(WX) \\
\text{subject to} & X_{ii} = 1, \\
                  & X \succeq 0, \\
                  & \operatorname{rank}(X) = 1.
\end{array}
```

- 这是一个非凸（nonconvex）的组合问题（combinatorial problem）；
- 我们将推导它的半定规划（semidefinite program, SDP）松弛（relaxation）。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 把 $n$ 个顶点做二划分，可行赋值恰好 $2^n$ 种——这是 6.006 中最典型的指数爆炸形态：MAX-CUT 是 NP-hard 问题，至今没有已知的多项式时间精确算法，$n$ 稍大时穷举即不可行。注意第一步形式转换：最大化割 $J(x) = \mathbf{1}^T W \mathbf{1} - x^T W x$ 时，$\mathbf{1}^T W \mathbf{1}$ 是与 $x$ 无关的常数，于是「最大化割」等价于在 $x_i^2 = 1$（即 $x \in \{-1, 1\}^n$）约束下最小化二次型 $x^T W x$。第二步引入 $X = xx^T$：目标变成线性的 $\operatorname{tr}(WX)$，约束 $x_i^2 = 1$ 变成线性约束 $X_{ii} = 1$，全部非凸性被压缩到单一约束 $\operatorname{rank}(X) = 1$ 上。这与 6.006 里「整数规划 → LP 松弛」的路线同源：先松成一个多项式时间可解的凸问题（LP 对应网络流/最短路那类结构），再解决「取整」；区别在于这里的松弛是 SDP 而非 LP，且秩 1 结构丢掉后一般无法无损恢复，取整必须随机化——这正是下一页的主题。

<!-- p.11 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**SDP 松弛与随机化取整（SDP relaxation）**</span>

去掉（dropping）秩（rank）约束，得到：

```math
\begin{array}{ll}
\text{minimize}   & \operatorname{tr}(WX) \\
\text{subject to} & X_{ii} = 1, \quad X \succeq 0
\end{array}
```

<span style="color:#2471a3;">**[algorithm]**</span> 随机化方案（randomized scheme）：

- 求解该 SDP 得到 $X^\star$，它给出原问题最优值的下界（lower bound）；
- 从 $\mathcal{N}(0, X^\star)$ 中采样 $v$；
- 取 $x = \operatorname{sign}(v)$。

Goemans 与 Williamson 证明：对 MAX-CUT 问题（$W_{ii} = 0$，$W_{ij} \geq 0$），这一下界平均而言至多次优 14%。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> Goemans 与 Williamson 在 1995 年证明的这一保证是近似算法史上的里程碑：此前近二十年，MAX-CUT 的最好保证停留在「至少拿到最优值一半」的平凡水平，他们的随机化取整首次给出约 0.878 的期望比（对应本页「平均至多次优约 14%」的表述）。机制可以用概率论拆解：取 $v \sim \mathcal{N}(0, X^\star)$ 后逐分量取符号，两顶点同号的概率由联合高斯的相关结构决定，有恒等式 $\mathbb{E}[\operatorname{sign}(v_i)\operatorname{sign}(v_j)] = \frac{2}{\pi}\arcsin\!\big(X^\star_{ij}/\sqrt{X^\star_{ii}X^\star_{jj}}\big)$——即「二维联合高斯的相关系数与反正弦函数」的经典结果。这正是用期望（expectation）刻画随机算法性能的标准范式，再配合集中不等式即可升级为高概率保证。此外，$X^\star \succeq 0$ 保证它确实是合法的协方差矩阵，数值上对其做 Cholesky 分解即可完成采样——又一次回到 Strang 的正定矩阵工具箱。

<!-- p.12 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**经对偶路径的 SDP 松弛：拉格朗日函数（SDP relaxation via dual）**</span>

<span style="color:#2471a3;">**[definition]**</span> 原问题（primal problem）的拉格朗日函数（Lagrangian）：

```math
\begin{aligned}
L(x, \nu) &= x^T W x + \sum_i \nu_i (x_i^2 - 1) \\
          &= \operatorname{tr}\!\left( (W + \operatorname{diag}(\nu))\, x x^T \right) - \mathbf{1}^T \nu
\end{aligned}
```

对偶函数（dual function）：

```math
g(\nu) =
\begin{cases}
-\mathbf{1}^T \nu, & \text{if } W + \operatorname{diag}(\nu) \succeq 0, \\
-\infty,           & \text{otherwise.}
\end{cases}
```

（即当 $W + \operatorname{diag}(\nu) \succeq 0$ 时 $g(\nu) = -\mathbf{1}^T \nu$，否则 $g(\nu) = -\infty$。）

<!-- p.13 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**经对偶路径的 SDP 松弛：对偶问题（SDP relaxation via dual）**</span>

<span style="color:#2471a3;">**[definition]**</span> 对偶问题（dual problem）：

```math
\begin{array}{ll}
\text{maximize}   & -\mathbf{1}^T \nu \\
\text{subject to} & W + \operatorname{diag}(\nu) \succeq 0
\end{array}
```

对偶的对偶（dual of dual）：

```math
\begin{array}{ll}
\text{minimize}   & \operatorname{tr}(WX) \\
\text{subject to} & X_{ii} = 1, \quad X \succeq 0
\end{array}
```

与直接去掉秩约束得到的松弛完全相同！

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 本页是全场对偶思想的收束：对偶的对偶恰好还原出「直接丢掉秩约束」的 SDP，说明该松弛不是临时起意的近似，而是拉格朗日对偶（Lagrange duality）框架的必然产物。从第一性原理回顾：对 $x$ 取下确界得到对偶函数 $g(\nu)$，它给出原问题最优值的下界，这就是弱对偶性（weak duality）；对 $\nu$ 最大化便得到最好的下界。再对这个对偶问题做一次对偶，变量以 $X$（扮演 $xx^T$ 的凸松弛角色）回归，就得到 $\min \operatorname{tr}(WX)$、$X_{ii} = 1$、$X \succeq 0$。该 SDP 存在严格可行点 $X = I$（正定且对角线全为 1），Slater 条件成立，故对偶间隙（duality gap）为零、对偶最优值可以达到；松弛界是紧的当且仅当最优解 $X^\star$ 恰好秩 1——此时连取整都不需要。用 Strang 的语言说，$X \succeq 0$ 就是「特征值全部非负」，这正是半定规划与对称矩阵特征值理论、Cholesky 分解深度绑定的原因。
