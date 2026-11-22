# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

> <span style="color:#7f8c8d;">Source: SEE官方讲义/AdditionalNotes1ConvexOptExamples.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

## <span style="color:#2471a3;">**[lecture]**</span> <span style="color:#c0392b">Convex optimization examples（凸优化示例）</span>

本份附加讲义（Additional Notes 1）用七个工程问题演示 convex optimization（凸优化）的落地方式：同一套"建模—变换—求解"的配方在不同物理场景中反复出现，先后用到了 linear program（线性规划，LP）、quasiconvex optimization（拟凸优化）、least-squares（最小二乘）、second-order cone programming（二阶锥规划，SOCP）、geometric programming（几何规划）与 semidefinite program（半定规划，SDP）。

<!-- p.1 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">示例总览</span>

- force/moment generation with thrusters（推进器的力/力矩生成）
- minimum-time optimal control（最短时间最优控制）
- optimal transmitter power allocation（发射机最优功率分配）
- phased-array antenna beamforming（相控阵天线波束成形）
- optimal receiver location（接收机最优选址）
- power allocation in FDM system（FDM 系统中的功率分配）
- optimizing structural dynamics（结构动力学优化）

---

<!-- p.2 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Force/moment generation with thrusters（推进器的力/力矩生成）</span>

考虑一个 rigid body（刚体），其 center of mass（质心）位于原点 $p = 0 \in \mathbb{R}^2$；另有 $n$ 个 thruster（推进器）：第 $i$ 个推进器以 magnitude（大小）$u_i$、direction（方向）$\theta_i$ 作用于点 $p_i = (p_{ix}, p_{iy})$。

（图示：在位置 $(p_{ix}, p_{iy})$ 处、沿方向 $\theta_i$ 施加大小为 $u_i$ 的推力。）

<!-- p.3 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">合力、合力矩与燃料消耗</span>

各推进器叠加后的总效果：

- resulting horizontal force（合成的水平力）：$F_x = \sum_{i=1}^n u_i \cos\theta_i$
- resulting vertical force（合成的竖直力）：$F_y = \sum_{i=1}^n u_i \sin\theta_i$
- resulting torque（合成的力矩）：$T = \sum_{i=1}^n \left(p_{iy} u_i \cos\theta_i - p_{ix} u_i \sin\theta_i\right)$

约束与代价：

- force limits（推力限制）：$0 \le u_i \le 1$（thruster 推进器的物理上限）
- fuel usage（燃料消耗）：$u_1 + \cdots + u_n$

**Problem（问题）：** 求一组推进器推力 $u_i$，使合成力与力矩达到给定的期望值 $F_x^{\mathrm{des}}$、$F_y^{\mathrm{des}}$、$T^{\mathrm{des}}$，并在问题 feasible（可行）的前提下最小化燃料消耗。

<!-- p.4 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">可表达为 linear program（线性规划，LP）</span>

上述问题可以表达为一个 LP：

```math
\begin{array}{ll}
\text{minimize} & \mathbf{1}^T u \\
\text{subject to} & F u = f^{\mathrm{des}} \\
& 0 \le u_i \le 1, \quad i = 1, \ldots, n
\end{array}
```

其中

```math
F = \begin{bmatrix}
\cos\theta_1 & \cdots & \cos\theta_n \\
\sin\theta_1 & \cdots & \sin\theta_n \\
p_{1y}\cos\theta_1 - p_{1x}\sin\theta_1 & \cdots & p_{ny}\cos\theta_n - p_{nx}\sin\theta_n
\end{bmatrix},
\quad
f^{\mathrm{des}} = \begin{bmatrix} F_x^{\mathrm{des}} \\ F_y^{\mathrm{des}} \\ T^{\mathrm{des}} \end{bmatrix},
\quad
\mathbf{1} = (1, 1, \ldots, 1)
```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这个例子的深层价值在于"建模变换"：只要目标是线性成本、约束是线性等式加箱式界，问题就自动落入 LP 的表达范围，而 LP 可以在多项式时间内可靠求解——你在 6.006 算法导论里见过的最短路/网络流问题本质上就是 LP 的特例，这里只是把"图上的流量"换成了"推进器的推力"。矩阵 $F$ 的构造思路也值得记住：把 $n$ 个决策变量对系统的线性响应逐列排开，就得到把变量映射为输出的传递矩阵，这与 Strang 线性代数中 $y = Ax$ 的视角完全一致——$F$ 的第 $i$ 列正是单位推力 $u_i = 1$ 单独作用时产生的 $(F_x, F_y, T)$ 三元组。另外注意 $0 \le u_i \le 1$ 属于 box constraint（箱式约束），不破坏 LP 结构。后面几页会把 $\ell_1$ 目标、分段线性燃料模型都塞进同一框架，这正是凸优化工程应用的核心套路：不换算法，只换建模。

<!-- p.5 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Extensions of thruster problem（推进器问题的扩展）</span>

**opposing thruster pairs（对置推进器对）**——允许推力反向（成对安装、差动出力），改用 $\ell_1$ 目标：

```math
\begin{array}{ll}
\text{minimize} & \|u\|_1 = \sum_{i=1}^n |u_i| \\
\text{subject to} & F u = f^{\mathrm{des}} \\
& |u_i| \le 1, \quad i = 1, \ldots, n
\end{array}
```

可以表达为 LP。

**more accurate fuel use model（更精确的燃料使用模型）**：

```math
\begin{array}{ll}
\text{minimize} & \sum_{i=1}^n \phi_i(u_i) \\
\text{subject to} & F u = f^{\mathrm{des}} \\
& 0 \le u_i \le 1, \quad i = 1, \ldots, n
\end{array}
```

其中 $\phi_i$ 为 piecewise linear increasing convex functions（分段线性递增凸函数），可以表达为 LP。

<!-- p.6 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">最小化最大误差与推进器数量</span>

**minimize maximum force/moment error（最小化最大力/力矩误差）**：

```math
\begin{array}{ll}
\text{minimize} & \|F u - f^{\mathrm{des}}\|_\infty \\
\text{subject to} & 0 \le u_i \le 1, \quad i = 1, \ldots, n
\end{array}
```

可以表达为 LP。

**minimize number of thrusters used（最小化使用的推进器数量）**：

```math
\begin{array}{ll}
\text{minimize} & \text{\# thrusters on} \\
\text{subject to} & F u = f^{\mathrm{des}} \\
& 0 \le u_i \le 1, \quad i = 1, \ldots, n
\end{array}
```

**不能**表达为 LP（计数型目标非凸；不过可以逐一检验 $2^n$ 个推进器子集的 feasibility（可行性））。

---

<!-- p.7 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Minimum-time optimal control（最短时间最优控制）</span>

- linear dynamical system（线性动力学系统）：$x(t+1) = A x(t) + B u(t)$，$t = 0, 1, \ldots, K$，初始状态 $x(0) = x_0$
- inputs limited to range $[-1, 1]$（输入限制在该区间内）：$\|u(t)\|_\infty \le 1$，$t = 0, 1, \ldots, K$
- settling time（稳定时间）：

```math
f(u(0), \ldots, u(K)) = \min \{ T \mid x(t) = 0 \ \mathrm{for}\ T \le t \le K+1 \}
```

<!-- p.8 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">稳定时间是 quasiconvex function（拟凸函数）</span>

settling time $f$ 作为 $(u(0), \ldots, u(K))$ 的函数是 quasiconvex function（拟凸函数）：

$f(u(0), u(1), \ldots, u(K)) \le T$ **当且仅当**对所有 $t = T, \ldots, K+1$，有

```math
x(t) = A^t x_0 + A^{t-1} B u(0) + \cdots + B u(t-1) = 0
```

也就是说，sublevel sets（下水平集）是 affine sets（仿射集）。

minimum-time optimal control problem（最短时间最优控制问题）：

```math
\begin{array}{ll}
\text{minimize} & f(u(0), u(1), \ldots, u(K)) \\
\text{subject to} & \|u(t)\|_\infty \le 1, \quad t = 0, \ldots, K
\end{array}
```

variables（变量）为 $u(0), \ldots, u(K)$。这是一个 quasiconvex problem（拟凸问题），可以通过 bisection（二分法）配合一系列 LP 求解。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这一页是"拟凸问题为何也能可靠求解"的最干净展示，可以把它抽象为一个思维模板——"下水平集二分法"。第一性原理链条是：二分搜索能工作的唯一前提，是"判定 $f(u) \le T$ 是否成立"可以被准确回答；而这里 $f(u) \le T$ 当且仅当一组线性等式同时成立，即下水平集是仿射集（线性方程组的解集）。于是每轮二分迭代只需解一个 LP 可行性子问题：可行则上界减半，不可行则下界升半——这与你在 6.006 学过的 binary search（二分搜索）在有序数组上收缩区间的逻辑完全同构，区别只是"比较一次"被替换成"解一次 LP"。反观上一页"推进器数量最少"的计数目标，其下水平集是离散破碎的集合，二分无从谈起；而拟凸函数的每个下水平集都是凸集，保证了区间的单调可分性。这正是拟凸性的价值：它既不 convex 也不 smooth，却保住了多项式时间可判定性，是 Boyd 正课 quasiconvex optimization 一章的核心技巧。

<!-- p.9 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Minimum-time control example（最短时间控制示例）</span>

三个 unit mass（单位质量）由两个 unit spring（单位弹簧）连接，弹簧的 equilibrium length（平衡长度）为 1。$u(t) \in \mathbb{R}^2$ 是在第 $t$ 个时间区间 $(0.15t,\ 0.15(t+1)]$ 内作用在左、右两个质量块上的力。

**Problem（问题）：** 选择 $u(0), \ldots, u(K)$，使三个质量块尽可能快地从初始条件 $(-3, 0, 2)$（处于静止）移动到目标位置 $(-1, 0, 1)$（并保持静止）。

<!-- p.10 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Optimal solution（最优解）图示</span>

本页为纯图示 slide：展示最优解的数值结果——输入 $u_1$、$u_2$ 与三个质量块的 position（位置）随时间 $t$（约 $0$ 到 $5$）的演化曲线，图例区分 right mass（右质量块）、center mass（中间质量块）、left mass（左质量块）。

---

<!-- p.11 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Optimal transmitter power allocation（发射机最优功率分配）</span>

- $m$ 个 transmitter（发射机）与 $mn$ 个 receiver（接收机）全部工作在同一频率
- 发射机 $i$ 想向标记为 $(i, j)$、$j = 1, \ldots, n$ 的 $n$ 个接收机发送信号
- $A_{ijk}$ 是从发射机 $k$ 到接收机 $(i, j)$ 的 path gain（路径增益）
- $N_{ij}$ 是接收机 $(i, j)$ 的 (self) noise power（自身噪声功率）
- variables（变量）：各发射机的发射功率 $p_k$，$k = 1, \ldots, m$

<!-- p.12 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">SINR 最大化问题</span>

在接收机 $(i, j)$ 处：

- signal power（信号功率）：$S_{ij} = A_{iji} p_i$
- noise plus interference power（噪声加干扰功率）：$I_{ij} = \sum_{k \ne i} A_{ijk} p_k + N_{ij}$
- signal to interference/noise ratio（信干噪比，SINR）：$S_{ij}/I_{ij}$

**Problem（问题）**：选择 $p_i$ 使最小的 SINR 最大化：

```math
\begin{array}{ll}
\text{maximize} & \min_{i,j} \dfrac{A_{iji}\, p_i}{\sum_{k \ne i} A_{ijk}\, p_k + N_{ij}} \\
\text{subject to} & 0 \le p_i \le p^{\max}
\end{array}
```

……这是一个 (generalized) linear fractional program（广义线性分式规划）。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> "分式目标"看似棘手，其实有教科书级的标准解法——变量替换（即 Charnes–Cooper 变换，由 A. Charnes 与 W. W. Cooper 于 1962 年提出）：引入"分母归一"的缩放变量，把每个比值改写成"分子 × 缩放因子"的线性函数，分式规划就退化为 LP；广义分式情形（目标里套 $\min$）则把"最小 SINR $\ge \gamma$"改写为一组不等式约束，再对阈值 $\gamma$ 二分。Boyd & Vandenberghe 教材 §4.3.2 对此有完整推导。用你已经熟悉的语言说，这与概率论中"用归一化常数把似然函数变成概率分布"是同一个动作：比值结构不怕整体缩放，怕的是异分母相加；而这里的分子、分母都是 affine（仿射）的，恰好允许归一化操作。本例也是后面 FDM 系统一节的热身：那里分母升级为 posynomial（正项式），"先变换、再凸化"的思想会再次登场。

<!-- p.13 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Phased-array antenna beamforming（相控阵天线波束成形）</span>

- 位于 $(x_1, y_1), \ldots, (x_n, y_n)$ 处的 omnidirectional antenna elements（全向天线单元）
- 来自角度 $\theta$ 的 unit plane wave（单位平面波）在第 $i$ 个单元中感应出信号

```math
e^{\,j(x_i \cos\theta + y_i \sin\theta - \omega t)}
```

（其中 $j = \sqrt{-1}$，frequency（频率）为 $\omega$，wavelength（波长）为 $2\pi$。）

<!-- p.14 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">阵列增益方向图</span>

- 解调（demodulate）后得到输出 $e^{\,j(x_i \cos\theta + y_i \sin\theta)} \in \mathbb{C}$
- 用复权重 $w_i$ 线性组合：

```math
y(\theta) = \sum_{i=1}^n w_i e^{\,j(x_i \cos\theta + y_i \sin\theta)}
```

- $y(\theta)$ 是 (complex) antenna array gain pattern（复天线阵列增益方向图）
- $|y(\theta)|$ 给出阵列灵敏度随入射角 $\theta$ 的变化
- 方向图依赖于设计变量 $\mathrm{Re}\, w$ 与 $\mathrm{Im}\, w$（称为 antenna array weights（阵列权重）或 shading coefficients（加权系数））

**Design problem（设计问题）**：选择 $w$ 以实现期望的增益方向图。

<!-- p.15 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Sidelobe level minimization（旁瓣电平最小化）：最小二乘</span>

目标：使 $|y(\theta)|$ 在 $|\theta - \theta_{\mathrm{tar}}| > \alpha$ 的角度上足够小（$\theta_{\mathrm{tar}}$ 为 target direction（目标方向）；$2\alpha$ 为 beamwidth（波束宽度））。

用 least-squares（最小二乘）实现（将角度离散化）：

```math
\begin{array}{ll}
\text{minimize} & \sum_i |y(\theta_i)|^2 \\
\text{subject to} & y(\theta_{\mathrm{tar}}) = 1
\end{array}
```

（求和遍及波束之外的角度。）这是一个带两个 (real) linear equality constraints（实线性等式约束）的 least-squares problem（最小二乘问题）。

<!-- p.16 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">最小二乘设计的方向图（图示）</span>

纯图示 slide：最小二乘波束成形设计得到的增益方向图 $|y(\theta)|$——$\theta_{\mathrm{tar}} = 30^\circ$，主瓣覆盖 $10^\circ$ 至 $50^\circ$，主瓣之外的区域图中标注了 sidelobe level（旁瓣电平）。

<!-- p.17 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">最小化旁瓣电平：表达为 SOCP</span>

直接以旁瓣电平为目标（将角度离散化）：

```math
\begin{array}{ll}
\text{minimize} & \max_i |y(\theta_i)| \\
\text{subject to} & y(\theta_{\mathrm{tar}}) = 1
\end{array}
```

（$\max$ 遍及波束之外的角度。）

可以表达为 SOCP（二阶锥规划）——引入 epigraph（上图）变量 $t$：

```math
\begin{array}{ll}
\text{minimize} & t \\
\text{subject to} & |y(\theta_i)| \le t \\
& y(\theta_{\mathrm{tar}}) = 1
\end{array}
```

<!-- p.18 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">旁瓣最小化设计的方向图（图示）</span>

纯图示 slide：与最小二乘设计同样取 $\theta_{\mathrm{tar}} = 30^\circ$、波束 $10^\circ$ 至 $50^\circ$，此图对应直接最小化主瓣外最大幅度 $|y(\theta)|$ 的 SOCP 公式，图中标注 sidelobe level（旁瓣电平）。

<!-- p.19 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Extensions（扩展）</span>

convex（凸）与 quasiconvex（拟凸）扩展：

- $y(\theta_0) = 0$（在方向 $\theta_0$ 处制造 null（零陷））
- $w$ 为实向量（amplitude only shading（仅幅度加权））
- $|w_i| \le 1$（attenuation only shading（仅衰减加权））
- 最小化 $\sigma^2 \sum_{i=1}^n |w_i|^2$（$y$ 中的 thermal noise power（热噪声功率））
- 给定最大旁瓣电平，最小化 beamwidth（波束宽度）

nonconvex（非凸）扩展：

- 最大化 zero weights（零权重）的数量

---

<!-- p.20 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Optimal receiver location（接收机最优选址）</span>

- $N$ 个发射机频率 $1, \ldots, N$
- 位于 $a_i$ 与 $b_i \in \mathbb{R}^2$ 处的发射机使用频率 $i$
- 位于 $a_1, a_2, \ldots, a_N$ 的发射机是 wanted（有用）发射机
- 位于 $b_1, b_2, \ldots, b_N$ 的发射机是 interfering（干扰）发射机
- receiver（接收机）位于位置 $x \in \mathbb{R}^2$

（图示：有用发射机 $a_1, a_2, a_3$、干扰发射机 $b_1, b_2, b_3$，以及待定的接收机位置 $x$。）

<!-- p.21 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">信干比 S/I</span>

- 来自 $a_i$ 的 (signal) receiver power（接收信号功率）：$\|x - a_i\|^{-\alpha}$（$\alpha \approx 2.1$）
- 来自 $b_i$ 的 (interfering) receiver power（接收干扰功率）：$\|x - b_i\|^{-\alpha}$（$\alpha \approx 2.1$）
- 对所有频率取最坏情况的 signal to interference ratio（信干比）：

```math
S/I = \min_i \frac{\|x - a_i\|^{-\alpha}}{\|x - b_i\|^{-\alpha}}
```

- 问题：哪个接收机位置 $x$ 能最大化 $S/I$？

<!-- p.22 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">S/I 的 quasiconcavity（拟凹性）</span>

$S/I$ 在集合 $\{x \mid S/I \ge 1\}$ 上是 quasiconcave（拟凹的），也就是说在区域

```math
\{x \mid \|x - a_i\| \le \|x - b_i\|,\ i = 1, \ldots, N\}
```

上是拟凹的。可以采用 bisection（二分法）求解；每一次迭代都是一个 convex quadratic feasibility problem（凸二次可行性问题）。

---

<!-- p.23 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Power allocation in FDM system（FDM 系统中的功率分配）</span>

frequency division multiplex (FDM) system（频分复用系统）：

- 信号 $u_i$ 以功率 $p_i$ 调制载波频率 $f_i$
- channel（信道）存在轻微的 nonlinearity（非线性）
- 各路功率影响每个 $y_i$ 处的信号功率与干扰功率
- **Problem（问题）**：选择各路功率，使最小 SINR（signal to noise & interference ratio（信干噪比））最大化

（图示：$u_1, \ldots, u_n$ 经 mod.（调制）进入信道，demod.（解调）后得到 $y_1, \ldots, y_n$。）

<!-- p.24 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">噪声与串扰模型</span>

- $y_i$ 中 demodulated signal power（解调后信号功率）与 $p_i$ 成正比
- $y_i$ 中 noise power（噪声功率）为 $\sigma_i^2$
- $y_i$ 中 interference power（干扰功率）是非线性产生的 crosstalk（串扰）与 intermodulation products（互调产物）之和
- crosstalk power（串扰功率）$c_i$ 对功率向量是线性的：$c = C p$，$C_{ij} \ge 0$；$C$ 通常是 tridiagonal（三对角的），即只有相邻信道之间才存在串扰

<!-- p.25 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Intermodulation（互调）产物</span>

- intermodulation power（互调功率）：$k$ 阶 IM products（互调产物）的频率为 $\pm f_{i_1} \pm f_{i_2} \pm \cdots \pm f_{i_k}$，功率与 $p_{i_1} p_{i_2} \cdots p_{i_k}$ 成正比

例如对频率 $1, 2, 3$：

| frequency（频率） | IM product（互调产物） | pwr. prop. to（功率正比于） |
|:---:|:---:|:---:|
| 2 | $1 + 1$ | $p_1^2$ |
| 3 | $1 + 2$ | $p_1 p_2$ |
| 1 | $2 - 1$ | $p_2 p_1$ |
| 1 | $3 - 2$ | $p_3 p_2$ |
| 2 | $3 - 1$ | $p_3 p_1$ |
| 3 | $1 + 1 + 1$ | $p_1^3$ |
| 1 | $1 + 1 - 1$ | $p_1^3$ |
| 2 | $2 + 1 - 1$ | $p_1^2 p_2$ |
| … | … | … |

- 频率 $f_i$ 处的总互调功率是 $p_1, \ldots, p_n$ 的 (complicated) polynomial（复杂多项式），且 coefficients（系数）全部非负

<!-- p.26 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">geometric program（几何规划）</span>

频率 $i$ 处的 inverse SINR（逆信干噪比）

```math
\frac{\text{noise + crosstalk + IM power}}{\text{signal power}}
```

是 $p_1, \ldots, p_n$ 的 posynomial function（正项式函数）。因此，形如下式的问题

```math
\begin{array}{ll}
\text{maximize} & \min_i \ \mathrm{SINR}_i \\
\text{subject to} & 0 < p_i \le P^{\max}
\end{array}
```

是 geometric program（几何规划）。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这一页的关键词是 posynomial（正项式）：系数非负的 monomial（单项式）之和，形如 $\sum_k c_k\, p_1^{\alpha_{1k}} \cdots p_n^{\alpha_{nk}}$，其中 $c_k \ge 0$。它一般不是凸函数，但取对数后立刻"显形"——对数把乘法变加法、把幂变系数：$\log(p_1 p_2) = \log p_1 + \log p_2$，$\log p_1^2 = 2 \log p_1$。这与 CSAPP 中浮点数的设计思想同源：浮点数用"阶码"把指数部分线性化，使乘除法化为阶码的加减，只是这里在对数域中精确成立、不涉及任何舍入误差。于是"对 GP 做对数换元 $y_i = \log p_i$"就能把整个问题化为凸问题，约束 $0 < p_i \le P^{\max}$ 也顺势变成线性约束——注意下界是严格大于零，因为 $p_i = 0$ 处正项式与对数都会失效。这种"换坐标系让问题变凸"的设计思路，与线性分式规划经缩放变换化为 LP 一脉相承，Boyd 教材 §4.5 给出了 GP 的完整理论。

<!-- p.27 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Optimizing structural dynamics（结构动力学优化）</span>

考虑 linear elastic structure（线性弹性结构），其 dynamics（动力学）方程（忽略 damping（阻尼））为 $M \ddot{d} + K d = 0$：

- $d(t) \in \mathbb{R}^k$：displacements（位移）向量
- $M = M^T \succ 0$ 是 mass matrix（质量矩阵）；$K = K^T \succ 0$ 是 stiffness matrix（刚度矩阵）

<!-- p.28 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Fundamental frequency（基频）</span>

- 解具有形式

```math
d_i(t) = \sum_{j=1}^k \alpha_{ij} \cos(\omega_j t - \phi_j)
```

其中 $0 \le \omega_1 \le \omega_2 \le \cdots \le \omega_k$ 是 modal frequencies（模态频率），即 $\det(\omega^2 M - K) = 0$ 的正解。

- fundamental frequency（基频）：

```math
\omega_1 = \lambda_{\min}(K, M)^{1/2} = \lambda_{\min}\!\left(M^{-1/2} K M^{-1/2}\right)^{1/2}
```

- 当频率低于 $\omega_1$ 时，结构表现得如同一个质量块
- $\omega_1$ 给出 stiffness measure（刚度度量）：$\omega_1$ 越大，结构越 stiff（刚硬）
- $\omega_1 \ge \Omega \iff \Omega^2 M - K \preceq 0$，因此 $\omega_1$ 是 $M$、$K$ 的 quasiconcave function（拟凹函数）

<!-- p.29 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">结构优化的 SDP 表达</span>

- design variables（设计变量）：$x_i$ 为第 $i$ 个 structural member（结构杆件）的 cross-sectional area（横截面积）（结构 geometry（几何）固定）
- $M(x) = M_0 + \sum_i x_i M_i$，$K(x) = K_0 + \sum_i x_i K_i$
- 结构重量 $w = w_0 + \sum_i x_i w_i$
- **Problem（问题）**：在 $\omega_1 \ge \Omega$ 及横截面积上下界的约束下最小化重量

写成 SDP（半定规划）：

```math
\begin{array}{ll}
\text{minimize} & w_0 + \sum_i x_i w_i \\
\text{subject to} & \Omega^2 M(x) - K(x) \preceq 0 \\
& l_i \le x_i \le u_i
\end{array}
```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 基频公式是 Strang 线性代数中特征值理论的一次"加权升级"。你在 Strang 课里熟悉的 eigenvalue problem（特征值问题）$Av = \lambda v$ 与 Rayleigh quotient（瑞利商）$v^T A v / v^T v$，在质量-刚度系统中推广为 generalized eigenvalue problem（广义特征值问题）$K v = \omega^2 M v$：分母的 $v^T v$ 换成了 $v^T M v$，即改用质量矩阵 $M$ 定义的内积来度量长度，而 $\omega_1^2$ 正是瑞利商 $v^T K v / v^T M v$ 的最小值。同样，Strang 讲的正定性 $x^T A x > 0$ 在这里升级为 linear matrix inequality（线性矩阵不等式，LMI）$K(x) - \Omega^2 M(x) \succeq 0$：约束不再是一组标量不等式，而是"整个矩阵半正定"，这正是 semidefinite program（半定规划）的标志性结构。妙处在于：拟凹的基频约束 $\omega_1 \ge \Omega$ 经过矩阵不等式改写后变成凸约束，重量最小化随之成为可用内点法高效求解的 SDP——把"物理性能要求"翻译成"矩阵不等式"，正是现代结构优化的标准建模动作。

---

> <span style="color:#7f8c8d;">全文完 · 共 29 页 · 译者注 5 条分散于 p.4 / p.8 / p.12 / p.26 / p.29 对应内容之后</span>
