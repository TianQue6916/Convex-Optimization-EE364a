> <span style="color:#7f8c8d;">Source: 补充材料/Convex Optimization Examples 凸优化示例.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

<!-- p.1 -->
## <span style="color:#2471a3;">**[lecture]**</span> <span style="color:#c0392b;">Convex Optimization Examples（凸优化示例）</span>

本讲用六个来自不同工程领域的实例，演示如何把实际问题建模为凸优化（convex optimization）问题：

- multi-period processor speed scheduling（多时段处理器速度调度）
- minimum time optimal control（最小时间最优控制）
- grasp force optimization（抓取力优化）
- optimal broadcast transmitter power allocation（最优广播发射机功率分配）
- phased-array antenna beamforming（相控阵天线波束成形）
- optimal receiver location（最优接收机位置）

<!-- p.2 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Multi-Period Processor Speed Scheduling（多时段处理器速度调度）</span>

- 处理器在 $T$ 个时段（time period）中的每个时段调整其运行速度 $s_t \in [s_{\min}, s_{\max}]$
- 时段 $t$ 消耗的能量为 $\phi(s_t)$；总能量为 $E = \sum_{t=1}^{T} \phi(s_t)$
- 共有 $n$ 个 job（作业）：
  - 作业 $i$ 在时刻 $t = A_i$ 就绪（available），必须在 deadline（截止期限）$t = D_i$ 前完成
  - 作业 $i$ 需要的总工作量为 $W_i \ge 0$
- $\theta^i_t \ge 0$ 是时段 $t$ 分配给作业 $i$ 的处理器算力（processor effort）比例

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">约束条件：每个时段的分配比例之和为 1，且每个作业在其时间窗 $[A_i, D_i]$ 内累计获得的工作量不少于 $W_i$：</span>

```math
\mathbf{1}^T \theta^t = 1, \qquad \sum_{t=A_i}^{D_i} \theta^i_t s_t \ge W_i
```

- 问题：选择速度 $s_t$ 与分配比例 $\theta^i_t$，使总能量 $E$ 最小

<!-- p.3 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Minimum Energy Processor Speed Scheduling（最小能量处理器速度调度）</span>

- 改用变量 $S_{ti} = \theta^i_t s_t$（时段 $t$ 投入作业 $i$ 的等效速度）来改写约束：

```math
s_t = \sum_{i=1}^{n} S_{ti}, \qquad \sum_{t=A_i}^{D_i} S_{ti} \ge W_i
```

- 求解如下凸优化问题：

```math
\begin{array}{ll}
\text{minimize}   & \displaystyle E = \sum_{t=1}^{T} \phi(s_t) \\
\text{subject to} & s_{\min} \le s_t \le s_{\max}, \quad t = 1, \ldots, T \\
                  & \displaystyle s_t = \sum_{i=1}^{n} S_{ti}, \quad t = 1, \ldots, T \\
                  & \displaystyle \sum_{t=A_i}^{D_i} S_{ti} \ge W_i, \quad i = 1, \ldots, n
\end{array}
```

- 当 $\phi$ 为凸函数时，这是一个凸问题
- 可恢复出最优分配比例：$\theta^{\star i}_t = (1/s^{\star}_t)\, S^{\star}_{ti}$

<!-- p.4 -->
### <span style="color:#2471a3;">**[example]**</span> <span style="color:#c0392b;">Example（示例）</span>

- $T = 16$ 个时段，$n = 12$ 个作业
- $s_{\min} = 1$，$s_{\max} = 6$，$\phi(s_t) = s_t^2$
- 作业画为区间 $[A_i, D_i]$ 上的横条，面积正比于 $W_i$

图示：一图绘出能耗函数 $\phi(s_t) = s_t^2$ 的曲线；另一图以横条展示各作业在时间轴上的时间窗与工作量（原 slide 为纯图示）。

<!-- p.5 -->
### <span style="color:#2471a3;">**[example]**</span> <span style="color:#c0392b;">Optimal and Uniform Schedules（最优调度与均匀调度）</span>

- uniform schedule（均匀调度）：$S_{ti} = W_i / (D_i - A_i + 1)$，对应总能量 $E_{\mathrm{unif}} = 204.3$
- optimal schedule（最优调度）：取 $S^{\star}_{ti}$，对应 $E^{\star} = 167.1$

图示：两幅图分别展示最优调度与均匀调度下各时段速度 $s_t$ 的曲线（纵轴 0–6，横轴 $t$ 到 18）。

<!-- p.6 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Minimum-Time Optimal Control（最小时间最优控制）</span>

- linear dynamical system（线性动态系统）：

```math
x_{t+1} = A x_t + B u_t, \quad t = 0, 1, \ldots, K, \qquad x_0 = x_{\mathrm{init}}
```

- 输入约束（按分量成立）：

```math
u_{\min} \preceq u_t \preceq u_{\max}, \quad t = 0, 1, \ldots, K
```

- 到达状态 $x_{\mathrm{des}}$ 所需的最短时间：

```math
f(u_0, \ldots, u_K) = \min \left\{\, T \mid x_t = x_{\mathrm{des}}, \ T \le t \le K+1 \,\right\}
```

<!-- p.7 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">State Transfer Time Is Quasiconvex（状态转移时间是拟凸函数）</span>

<span style="color:#2471a3;">**[theorem]**</span> <span style="color:#00838f;">状态转移时间 $f$ 是 $(u_0, \ldots, u_K)$ 的 quasiconvex（拟凸）函数：$f(u_0, u_1, \ldots, u_K) \le T$</span>

当且仅当对一切 $t = T, \ldots, K+1$：

```math
x_t = A^t x_{\mathrm{init}} + A^{t-1} B u_0 + \cdots + B u_{t-1} = x_{\mathrm{des}}
```

也就是说，$f$ 的 sublevel set（下水平集）是仿射集。

由此，最小时间最优控制问题为：

```math
\begin{array}{ll}
\text{minimize}   & f(u_0, u_1, \ldots, u_K) \\
\text{subject to} & u_{\min} \preceq u_t \preceq u_{\max}, \quad t = 0, \ldots, K
\end{array}
```

变量为 $u_0, \ldots, u_K$。这是一个 quasiconvex problem（拟凸问题），可以通过 bisection（二分法）求解。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这条命题是整份讲义的枢纽，值得放慢速度读。证明思路就藏在陈述里：$f(u_0, \ldots, u_K) \le T$ 当且仅当一组关于 $u$ 的线性等式有解，因此 $f$ 的下水平集是仿射集（当然是凸集），这正是 quasiconvex（拟凸）的定义特征——保留「下水平集必凸」，却不要求函数本身凸。你在 6.006 学过二分查找：只要对候选阈值存在单调的「可行/不可行」判定，就能用 $O(\log(1/\varepsilon))$ 次判定把最优值逼到任意精度。这里每次判定「$T$ 个时段内能否到达目标」都是凸可行性问题，多项式时间可解，于是非凸的目标优化被归约成一串凸判定——与 6.006 把新问题 reduction（归约）到已知可解问题是同一种思维方式。代价也和二分查找一样：只能得到 $\varepsilon$-精度的解，且每缩小一次区间都要重解一个凸问题。

<!-- p.8 -->
### <span style="color:#2471a3;">**[example]**</span> <span style="color:#c0392b;">Minimum-Time Control Example（最小时间控制示例）</span>

- 力 $(u_t)_1$ 驱动一个建模为 3 个质量块（即 2 个振动模态）的物体运动
- 力 $(u_t)_2$ 用于主动振动抑制（active vibration suppression）
- 目标：尽快把物体移动到指令位置，约束为 $|(u_t)_1| \le 1$、$|(u_t)_2| \le 0.1$，$t = 0, \ldots, K$

<!-- p.9 -->
### <span style="color:#2471a3;">**[example]**</span> <span style="color:#c0392b;">Ignoring Vibration Modes（忽略振动模态）</span>

- 把物体当作单一质量块处理，只施加 $u_1$
- 存在解析解（即 bang-bang（砰砰）控制解）

图示：三幅图分别为物体位置分量 $(x_t)_3$ 随 $t$ 的轨迹，以及输入 $(u_t)_1$、$(u_t)_2$ 随 $t$ 的波形；$(u_t)_1$ 呈现典型的「先满力正推、再满力反推」的 bang-bang 形状。

<!-- p.10 -->
### <span style="color:#2471a3;">**[example]**</span> <span style="color:#c0392b;">With Vibration Modes（计入振动模态）</span>

- 此时没有解析解
- 问题为拟凸问题，用二分法求解

图示：与上一页对应的三幅图，显示计入振动模态后最优轨迹 $(x_t)_3$ 与输入 $(u_t)_1$、$(u_t)_2$ 的波形。

<!-- p.11 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Grasp Force Optimization（抓取力优化）</span>

- 在物体上选择 $K$ 个抓取力（grasping force），使得：
  - 抵抗外部 wrench（力旋量）
  - 满足 friction cone（摩擦锥）约束
  - 最小化最大抓取力
- 这是一个凸问题（second-order cone program（二阶锥规划，SOCP））：

```math
\begin{array}{ll}
\text{minimize}   & \displaystyle \max_i \| f^{(i)} \|_2 \\
\text{subject to} & \displaystyle \sum_i Q^{(i)} f^{(i)} = f^{\mathrm{ext}} \\
                  & \displaystyle \sum_i p^{(i)} \times \left( Q^{(i)} f^{(i)} \right) = \tau^{\mathrm{ext}} \\
                  & \displaystyle \mu_i f^{(i)}_3 \ge \left( \left( f^{(i)}_1 \right)^2 + \left( f^{(i)}_2 \right)^2 \right)^{1/2}
\end{array}
```

- 各行含义：目标为最小化最大接触力（max contact force）；第一条约束为力平衡（force equilibrium）；第二条为力矩平衡（torque equilibrium）；第三条为摩擦锥约束（friction cone constraints）
- 变量为接触力 $f^{(i)} \in \mathbb{R}^3$，$i = 1, \ldots, K$

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 摩擦锥约束 $\mu_i f^{(i)}_3 \ge \| (f^{(i)}_1, f^{(i)}_2) \|_2$ 的几何形状是一个圆锥：切向分量越大，所需的法向压力就越大，否则接触点会打滑——这正是库仑摩擦定律的代数表达，物理结构恰好与 second-order cone（二阶锥，又称 Lorentz 锥）的几何重合。用 Strang 线代的视角看，右端就是二维向量的欧氏范数，而「向量范数不超过线性函数」正是把二次规划升级为 SOCP 的关键一步。这条锥约束还等价于某个 $2 \times 2$ 对称矩阵的半正定性，因此 SOCP 可视为半定规划（SDP）的特例，这是范数理论与半定规划之间的桥梁。目标 $\max_i \|f^{(i)}\|_2$ 本身不凸，靠引入 epigraph（上图）变量凸化，与 p.19 处理 $\max_i |y(\theta_i)|$ 的手法一致。「最小化最大接触力」即在各接触点之间做负载均衡，与本讲「最大化最坏 SINR」的 max-min 思想如出一辙。

<!-- p.12 -->
### <span style="color:#2471a3;">**[example]**</span> <span style="color:#c0392b;">Example（示例）</span>

本页为抓取力优化的数值示例图（原 slide 仅含图形，无可提取文字）。

<!-- p.13 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Optimal Broadcast Transmitter Power Allocation（最优广播发射机功率分配）</span>

- $m$ 个 transmitter（发射机），$mn$ 个 receiver（接收机），全部工作在同一频率
- 发射机 $i$ 想向标号为 $(i, j)$、$j = 1, \ldots, n$ 的 $n$ 个接收机发送信号
- $A_{ijk}$ 是从发射机 $k$ 到接收机 $(i, j)$ 的 path gain（路径增益）
- $N_{ij}$ 是接收机 $(i, j)$ 的（自身）噪声功率
- 变量：发射机功率 $p_k$，$k = 1, \ldots, m$

图示：发射机 $i$、发射机 $k$ 与接收机 $(i, j)$ 相对位置的示意（原 slide 含简单示意图）。

<!-- p.14 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Signal Model and SINR Maximization（信号模型与 SINR 最大化）</span>

<span style="color:#2471a3;">**[definition]**</span> 在接收机 $(i, j)$ 处：

- signal power（信号功率）：$S_{ij} = A_{iji} p_i$
- noise plus interference power（噪声加干扰功率）：

```math
I_{ij} = \sum_{k \ne i} A_{ijk} p_k + N_{ij}
```

- signal to interference/noise ratio（信干噪比，SINR）：$S_{ij} / I_{ij}$

<span style="color:#00838f;">问题：选择 $p_i$，使最小的 SINR 最大化：</span>

```math
\begin{array}{ll}
\text{maximize}   & \displaystyle \min_{i,j} \ \frac{A_{iji} p_i}{\sum_{k \ne i} A_{ijk} p_k + N_{ij}} \\
\text{subject to} & 0 \le p_i \le p_{\max}
\end{array}
```

……这是一个 generalized linear fractional program（广义线性分式规划）。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 「最大化最坏情况 SINR」是 generalized linear fractional program（广义线性分式规划）的典型样本：目标是功率的线性函数之比，比值本身既非线性也非凸，但具有拟凸性。固定门限 $t$ 后，「对一切 $(i,j)$ 都有 $\mathrm{SINR}_{ij} \ge t$」可交叉相乘改写为 $A_{iji} p_i \ge t ( \sum_{k \ne i} A_{ijk} p_k + N_{ij} )$——关于 $p$ 的线性不等式组，加上 $0 \le p_i \le p_{\max}$ 仍是线性系统。于是问题落回二分框架：外层对 $t$ 二分，内层解一个 LP 可行性问题。你在概率论里见过许多比值型泛函（似然比、变异系数等），它们普遍不凸却常具拟凸/拟凹性。本页示范一条通用流水线：识别拟凸性 → 参数化门限 → 化归为凸可行性判定，max-min 型鲁棒设计大多由此收编进凸优化的工具箱。值得注意的是，这条路线不需要对原问题做任何凸化近似，解的始终是精确的拟凸问题，而非它的替代版本。

<!-- p.15 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Phased-Array Antenna Beamforming（相控阵天线波束成形）</span>

- 位于 $(x_1, y_1), \ldots, (x_n, y_n)$ 的 omnidirectional（全向）天线单元
- 从角度 $\theta$ 入射的单位平面波在第 $i$ 个单元中感应出信号

```math
e^{j (x_i \cos\theta + y_i \sin\theta - \omega t)}
```

（其中 $j = \sqrt{-1}$，频率为 $\omega$，波长为 $2\pi$）

图示：天线单元位置 $(x_i, y_i)$ 与入射角 $\theta$ 的示意（原 slide 含简单示意图）。

<!-- p.16 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Antenna Array Gain Pattern（天线阵增益方向图）</span>

- 经解调（demodulate）后得到输出 $e^{j (x_i \cos\theta + y_i \sin\theta)} \in \mathbb{C}$
- 用复权重 $w_i$ 线性组合：

```math
y(\theta) = \sum_{i=1}^{n} w_i e^{j (x_i \cos\theta + y_i \sin\theta)}
```

- $y(\theta)$ 称为（复值的）antenna array gain pattern（天线阵增益方向图）
- $|y(\theta)|$ 给出阵列灵敏度随入射角 $\theta$ 变化的函数
- 它取决于设计变量 $\operatorname{Re} w$ 与 $\operatorname{Im} w$（称为 antenna array weights（天线阵权重）或 shading coefficients（加权系数））

设计问题：选择 $w$ 以实现期望的增益方向图。

<!-- p.17 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Sidelobe Level Minimization（旁瓣电平最小化）</span>

- 使 $|y(\theta)|$ 在 $|\theta - \theta_{\mathrm{tar}}| > \alpha$ 的范围内尽量小（$\theta_{\mathrm{tar}}$：target direction（目标方向）；$2\alpha$：beamwidth（波束宽度））
- 通过 least-squares（最小二乘）实现（将角度离散化）：

```math
\begin{array}{ll}
\text{minimize}   & \displaystyle \sum_i |y(\theta_i)|^2 \\
\text{subject to} & y(\theta_{\mathrm{tar}}) = 1
\end{array}
```

（求和遍历波束以外的角度）

这是一个带两个（实数）线性等式约束的 least-squares problem（最小二乘问题）。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 把角度离散化后，每个 $y(\theta_i)$ 都是设计变量 $w$ 的（复）线性函数：$y(\theta_i) = \sum_k w_k e^{j(x_k \cos\theta_i + y_k \sin\theta_i)}$。于是 $\sum_i |y(\theta_i)|^2$ 恰是 $\|Aw\|_2^2$ 型的二次型，$A$ 的第 $i$ 行由各天线单元在角度 $\theta_i$ 处的相位因子排成，称为导向向量（steering vector）。约束 $y(\theta_{\mathrm{tar}}) = 1$ 是复等式，拆成实部与虚部后正是 slide 说的「两个实线性等式约束」。这就是带线性等式约束的最小二乘，最优解由正规方程/KKT 条件给出，几何上对应 Strang 的「正交投影到列空间、残差落在零空间」图像。天线加权设计因此把最小二乘的投影几何搬到了复数域与波束方向图上，课堂理论直接变成工程工具。

<!-- p.18 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Gain Pattern（增益方向图）</span>

图示：$\theta_{\mathrm{tar}} = 30^\circ$ 时 $|y(\theta)|$ 的方向图，图中标注了波束边缘 $10^\circ$、$50^\circ$ 与 sidelobe level（旁瓣电平）（原 slide 为纯图示）。

<!-- p.19 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Minimize Sidelobe Level（最小化旁瓣电平）</span>

- 最小化旁瓣电平（将角度离散化）：

```math
\begin{array}{ll}
\text{minimize}   & \displaystyle \max_i |y(\theta_i)| \\
\text{subject to} & y(\theta_{\mathrm{tar}}) = 1
\end{array}
```

（max 取遍波束以外的角度）

- 可以 cast（转化）为 SOCP：

```math
\begin{array}{ll}
\text{minimize}   & t \\
\text{subject to} & |y(\theta_i)| \le t \\
                  & y(\theta_{\mathrm{tar}}) = 1
\end{array}
```

<!-- p.20 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Gain Pattern after Sidelobe Minimization（旁瓣最小化后的增益方向图）</span>

图示：按旁瓣电平最小化设计权重后，$\theta_{\mathrm{tar}} = 30^\circ$ 的 $|y(\theta)|$ 方向图与旁瓣电平（原 slide 为纯图示，版式与 p.18 相同）。

<!-- p.21 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Extensions（扩展形式）</span>

convex（凸）与 quasiconvex（拟凸）扩展：

- $y(\theta_0) = 0$（在方向 $\theta_0$ 处设置 null（零陷））
- $w$ 取实数（仅幅度加权，amplitude only shading）
- $|w_i| \le 1$（仅衰减加权，attenuation only shading）
- 最小化 $\sigma^2 \sum_{i=1}^{n} |w_i|^2$（$y$ 中的 thermal noise（热噪声）功率）
- 在给定最大旁瓣电平的约束下最小化波束宽度

nonconvex（非凸）扩展：

- 最大化零权重（zero weights）的个数

<!-- p.22 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Optimal Receiver Location（最优接收机位置）</span>

- $N$ 个发射机频率 $1, \ldots, N$
- 位于 $a_i, b_i \in \mathbb{R}^2$ 的发射机使用频率 $i$
- 位于 $a_1, a_2, \ldots, a_N$ 的发射机是有用（wanted）信号源
- 位于 $b_1, b_2, \ldots, b_N$ 的发射机是干扰（interfering）源
- 接收机位于 $x \in \mathbb{R}^2$

图示：平面上有用发射机 $a_1, a_2, a_3$、干扰发射机 $b_1, b_2, b_3$ 与接收机位置 $x$ 的示意（原 slide 含简单示意图）。

<!-- p.23 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Received Power and Worst-Case S/I（接收功率与最坏信干比）</span>

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">信号与干扰的接收功率由路径损耗模型刻画，最坏信干比定义为对频率取最小：</span>

- 来自 $a_i$ 的（信号）接收功率：$\|x - a_i\|_2^{-\alpha}$（$\alpha \approx 2.1$）
- 来自 $b_i$ 的（干扰）接收功率：$\|x - b_i\|_2^{-\alpha}$（$\alpha \approx 2.1$）
- 对所有频率而言最坏的 signal to interference ratio（信干比）：

```math
S/I = \min_i \ \frac{\|x - a_i\|_2^{-\alpha}}{\|x - b_i\|_2^{-\alpha}}
```

- 什么样的接收机位置 $x$ 能使 $S/I$ 最大？

<!-- p.24 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">S/I Is Quasiconcave（信干比的拟凹性）</span>

<span style="color:#2471a3;">**[theorem]**</span> <span style="color:#00838f;">$S/I$ 在集合 $\{x \mid S/I \ge 1\}$ 上是 quasiconcave（拟凹）的，即在下述集合上拟凹：</span>

```math
\{x \mid \|x - a_i\|_2 \le \|x - b_i\|_2, \ i = 1, \ldots, N\}
```

图示：与 p.22 相同的发射机布局示意（原 slide 为纯图示）。可用二分法求解；每一次迭代都是一个 convex quadratic feasibility problem（凸二次可行性问题）。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 为什么 $\{x \mid \|x - a_i\|_2 \le \|x - b_i\|_2\}$ 是凸集？把不等式两边平方，二次项 $x^T x$ 恰好相消，剩下 $2(b_i - a_i)^T x \le \|b_i\|_2^2 - \|a_i\|_2^2$——一个半空间。因此「信干比不劣于 1」的区域是 $N$ 个半空间的交，这也是 slide 中集合写成 $N$ 条约束之交的原因。几何上这正是以点对 $(a_i, b_i)$ 定义的广义 Voronoi 图：6.006 里 Voronoi 图按「离哪个站点更近」划分平面，而这里信号与干扰的路径损耗指数相同，功率比 $\ge 1$ 恰好等价于「离信号源不比离干扰源远」。这也是 quasiconcave（拟凹）的第一性原理：函数拟凹当且仅当其上水平集（superlevel set）是凸集，与拟凸函数「下水平集凸」完全对偶。对一般门限 $c$ 的二分迭代，约束形如 $\|x - a_i\|_2 \le c^{-1/\alpha} \|x - b_i\|_2$，仍是凸二次约束，故每次迭代解一个凸二次可行性问题。
