# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

## Stochastic Programming（随机规划）

> <span style="color:#7f8c8d;">Source: 补充材料/Stochastic Programming 随机规划.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

<!-- p.1 -->

### <span style="color:#2471a3;">**[lecture]**</span> <span style="color:#c0392b;">**随机规划：本讲导览**</span>

本讲主题（Stochastic Programming，随机规划）：

- 随机规划（stochastic programming）
- 「确定性等价」（'certainty equivalent'）问题
- 违约/短缺（violation/shortfall）约束与惩罚
- 蒙特卡洛采样方法（Monte Carlo sampling methods）
- 验证（validation）

资料来源：Nemirovsky & Shapiro。

<!-- p.2 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**随机规划的问题设定**</span>

<span style="color:#00838f;">**随机规划中，目标函数与约束函数 $f_i(x, \omega)$ 同时依赖优化变量 $x$ 与一个随机变量 $\omega$（random variable）。**</span>

- $\omega$ 刻画的对象：

  - 参数变化与不确定性（parameter variation and uncertainty）
  - 实现、制造、运行过程中的随机波动（random variation）

- $\omega$ 的具体取值未知，但其分布（distribution）已知
- 目标：选取 $x$，使得

  - 约束在平均意义上满足，或以高概率满足
  - 目标函数在平均意义上小，或以高概率小

<!-- p.3 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**基本的随机规划问题**</span>

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">**基本随机规划问题（basic stochastic programming problem）：**</span>

```math
\begin{array}{ll}
\text{minimize} & F_0(x) = \mathbb{E}\, f_0(x, \omega) \\
\text{subject to} & F_i(x) = \mathbb{E}\, f_i(x, \omega) \le 0, \quad i = 1, \ldots, m
\end{array}
```

- 决策变量（variable）是 $x$
- 问题数据是 $f_i$ 以及 $\omega$ 的分布
- 若对每个固定的 $\omega$，$f_i(x, \omega)$ 关于 $x$ 是凸的（convex），则

  - $F_i$ 是凸的
  - 从而该随机规划问题是凸问题

- 只有在少数情形下 $F_i$ 才有解析表达式；其余情形我们将近似求解该问题

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 为什么「取期望」不会破坏凸性？从你在概率论（Bertsekas）中学过的事实出发：期望是按分布加权的求和/积分，本质上是线性算子，满足 $\mathbb{E}(aX+bY)=a\,\mathbb{E}X+b\,\mathbb{E}Y$。凸性说的是「函数图像在弦的下方」，把一族凸函数 $f_i(x,\omega)$ 按概率权重逐点平均，相当于做非负加权的函数混合，而「凸函数的非负加权组合仍是凸函数」与 Strang 线代里「凸集的凸组合仍是凸集」是同一原理在函数与集合两个层面的镜像。于是 $F_i(x)=\mathbb{E}f_i(x,\omega)$ 自动继承凸性，问题仍是凸优化——这正是随机规划能全套沿用本课程凸工具链的根本原因。代价是 $F_i$ 一般没有闭式表达（除非 $\omega$ 只取有限个值、或以二次形式进入 $f_i$），「结构保持」与「可计算性」由此产生张力，这也是本讲后半部分转向 Monte Carlo 近似的直接动机。

<!-- p.4 -->

### <span style="color:#2471a3;">**[example]**</span> <span style="color:#c0392b;">**具有解析形式的例子**</span>

- 取 $f(x) = \|Ax - b\|_2^2$，其中 $A$、$b$ 是随机的
- 则 $F(x) = \mathbb{E} f(x) = x^T P x - 2q^T x + r$，其中

```math
P = \mathbb{E}(A^T A), \qquad q = \mathbb{E}(A^T b), \qquad r = \mathbb{E}(\|b\|_2^2)
```

- 只需要知道 $(A, b)$ 的二阶矩（second moments）
- 于是随机约束 $\mathbb{E} f(x) \le 0$ 可表示为一个标准的二次不等式（quadratic inequality）

<!-- p.5 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**「确定性等价」问题**</span>

- 「确定性等价」（'certainty-equivalent'，又称「均场」'mean field'）问题：

```math
\begin{array}{ll}
\text{minimize} & f_0(x, \mathbb{E}\,\omega) \\
\text{subject to} & f_i(x, \mathbb{E}\,\omega) \le 0, \quad i = 1, \ldots, m
\end{array}
```

- 粗略地说：直接忽略参数的随机波动，只用其均值代替
- 若对每个固定的 $x$，$f_i$ 关于 $\omega$ 是凸的，则

  - $f_i(x, \mathbb{E}\,\omega) \le \mathbb{E} f_i(x, \omega)$
  - 因此确定性等价问题的最优值是随机问题最优值的一个下界（lower bound）

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这条下界恰是你概率论中 Jensen 不等式（Jensen's inequality）的直接应用：凸函数满足 $f(\mathbb{E}\,\omega) \le \mathbb{E} f(\omega)$，直觉是「波动越大，凸函数平均下来越吃亏」。把不等式放进约束里读：$f_i(x, \mathbb{E}\,\omega) \le \mathbb{E} f_i(x, \omega)$ 说明确定性等价问题把每个约束都放松了（可行集变大），于是它的最优值只会更小或相等——这就是「下界」的来源，也解释了为何本讲所有 CE 数值都只当作 bound 而非可实现的性能。「均场」（mean field）一词借自统计物理：用全体粒子的平均效应替代个体涨落来近似整个系统，对应这里「用 $\mathbb{E}\,\omega$ 替代 $\omega$」。工程上的教训很实在：CE 问题便宜，一次确定性求解就能给出底价，适合做基准或粗筛；但若照着它做决策，等于假装波动不存在，而波动在真实系统里是要付费的。

<!-- p.6 -->

### <span style="color:#2471a3;">**[example]**</span> <span style="color:#c0392b;">**随机规划示例：目标值分布直方图**</span>

- 目标为 minimize $\mathbb{E}\|Ax - b\|_1$；其中 $A_{ij}$ 服从 $\bar{A}_{ij} \pm \gamma_{ij}$ 上的均匀分布（uniform），$b_i$ 服从 $\bar{b}_i \pm \delta_i$ 上的均匀分布
- 下图为随机最优解（stochastic optimal）与确定性等价解（certainty-equivalent solution）所对应目标值的概率密度（PDF）直方图
- 由确定性等价（CE）问题得到的下界：5.96

（纯图示 slide 说明：图中横、纵坐标范围均为 0–18，两条经验分布曲线分别标注为 stochastic solution（随机解）与 certainty equivalent solution（确定性等价解），用于直观对比两种建模方式所得目标值的分布差异。）

<!-- p.7 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**期望违约/短缺约束与惩罚**</span>

- 把 $\mathbb{E} f_i(x, \omega) \le 0$ 替换为下列两者之一：

  - $\mathbb{E}\, f_i(x, \omega)^+ \le \epsilon$（左边是期望违约 expected violation；记正部 $u^+ = \max\{u, 0\}$）
  - $\mathbb{E}\big(\max_i f_i(x, \omega)^+\big) \le \epsilon$（左边是期望最坏违约 expected worst violation）

- 变体：把违约/短缺惩罚（violation/shortfall penalty）加进目标函数：

```math
\text{minimize} \quad \mathbb{E}\Big(f_0(x, \omega) + \sum_{i=1}^m c_i f_i(x, \omega)^+\Big)
```

  其中 $c_i > 0$ 是违反各约束的惩罚率（penalty rates）。
- 只要 $f_i$ 关于 $x$ 凸，这些都是凸问题

<!-- p.8 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**机会约束与分位数优化**</span>

- 「机会约束」（'chance constraints'，$\eta$ 称为「置信水平」confidence level）：

```math
\mathrm{Prob}(f_i(x, \omega) \le 0) \ge \eta
```

  - 在某些情形下是凸的
  - 通常关心 $\eta = 0.9,\ 0.95,\ 0.99$
  - $\eta = 0.999$ 没有意义（除非你对分布的尾部很有把握）

- 分位数优化（percentile optimization，$\gamma$ 为「$\eta$ 分位数」percentile）：

```math
\begin{array}{ll}
\text{minimize} & \gamma \\
\text{subject to} & \mathrm{Prob}(f_0(x, \omega) \le \gamma) \ge \eta
\end{array}
```

  - 在某些情形下是凸的或拟凸的（quasiconvex）

- 这些主题将在下一讲展开

<!-- p.9 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**如何求解随机规划问题**</span>

- 特殊情形下有解析解，例如期望可以解析求出时：

  - $\omega$ 以二次（quadratic）形式进入 $f_i$
  - $\omega$ 只取有限多个值

- 一般情形：通过（Monte Carlo）采样近似求解

<!-- p.10 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**有限事件集**</span>

- 设 $\omega \in \{\omega_1, \ldots, \omega_N\}$，概率 $\pi_j = \mathrm{Prob}(\omega = \omega_j)$
- 有时把这些取值称为「情景」（'scenarios'）；常见情形 $\pi_j = 1/N$
- 随机规划问题变为：

```math
\begin{array}{ll}
\text{minimize} & F_0(x) = \sum_{j=1}^N \pi_j f_0(x, \omega_j) \\
\text{subject to} & F_i(x) = \sum_{j=1}^N \pi_j f_i(x, \omega_j) \le 0, \quad i = 1, \ldots, m
\end{array}
```

- 只要 $f_i$ 关于 $x$ 凸，这就是一个（标准形式的）凸问题
- 计算复杂度随情景数 $N$ 线性（linearly）增长

<!-- p.11 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Monte Carlo 采样方法**</span>

- 这是（近似）求解随机规划问题的一般方法
- 生成 $N$ 个样本（实现，realizations）$\omega_1, \ldots, \omega_N$，并赋予概率 $\pi_1, \ldots, \pi_N$（通常 $\pi_j = 1/N$）
- 构造样本平均近似（sample average approximation）：

```math
\hat{F}_i(x) = \sum_{j=1}^N \pi_j f_i(x, \omega_j), \qquad i = 0, \ldots, m
```

- 经由 $\omega_1, \ldots, \omega_N$，它们是随机变量（RVs），且均值为 $\mathbb{E} f_i(x, \omega) = F_i(x)$

<!-- p.12 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**求解有限事件问题：解的性质**</span>

- 接着求解有限事件问题：

```math
\begin{array}{ll}
\text{minimize} & \hat{F}_0(x) \\
\text{subject to} & \hat{F}_i(x) \le 0, \quad i = 1, \ldots, m
\end{array}
```

- 解 $x^\star_{\mathrm{mcs}}$ 与最优值 $\hat{F}_0(x^\star_{\mathrm{mcs}})$ 都是随机变量（希望它们接近原问题的解 $x^\star$ 与最优值 $p^\star$）
- 理论结论：

  - （在若干技术性条件下）当 $N \to \infty$ 时，$x^\star_{\mathrm{mcs}} \to x^\star$
  - $\mathbb{E}\, \hat{F}_0(x^\star_{\mathrm{mcs}}) \le p^\star$

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这两条理论结论值得拆开读。第一条 $x^\star_{\mathrm{mcs}} \to x^\star$ 的引擎是大数定律（Bertsekas 概率论中的弱大数定律）：样本均值 $\hat F_i$ 逐点收敛到期望 $F_i$，在若干技术条件下（如解集稳定、一致收敛）最优解也随之收敛；从 6.006 的复杂性视角看，情景数 $N$ 只让问题规模线性增长，而凸问题可在多项式时间求解，所以「加大样本」是付得起代价的。第二条 $\mathbb{E}\hat F_0(x^\star_{\mathrm{mcs}}) \le p^\star$ 揭示「优化者的乐观偏差」：由于最小化操作对样本噪声有系统性偏好，在同一批样本上挑出的解，其样本内成绩的期望必然不超过真实最优值——这不是 bug 而是数学事实，也正因如此，下一页才需要独立验证集来做无偏评估。两条合起来给出实用心法：采样解「渐近正确、样本内乐观」。

<!-- p.13 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**样本外验证（out-of-sample validation）**</span>

- 这是检验 $N$ 是否「足够大」的实用方法
- 取另一组样本（「验证集」validation set）$\omega^{val}_1, \ldots, \omega^{val}_M$ 及概率 $\pi^{val}_1, \ldots, \pi^{val}_M$（通常 $M \gg N$）（原来的样本集称为「训练集」training set）
- 计算：

```math
\hat{F}^{val}_i(x^\star_{\mathrm{mcs}}) = \sum_{j=1}^M \pi^{val}_j f_i(x^\star_{\mathrm{mcs}}, \omega^{val}_j), \qquad i = 0, \ldots, m
```

- 若 $\hat{F}_i(x^\star_{\mathrm{mcs}}) \approx \hat{F}^{val}_i(x^\star_{\mathrm{mcs}})$，则我们对 $x^\star_{\mathrm{mcs}} \approx x^\star$ 的信心增强
- 若不接近，则增大 $N$ 并重新计算 $x^\star_{\mathrm{mcs}}$

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 训练集/验证集二分法与统计学习中的做法完全同源：在同一批样本上既求解决策又汇报成绩，读数必然偏乐观（即上一页的 $\mathbb{E}\hat F_0(x^\star_{\mathrm{mcs}}) \le p^\star$），而换一批独立样本重测，相当于做一次重复实验来交叉核对。其概率论根基是独立同分布：验证集上的 $\hat F^{val}_i$ 与训练集的 $\hat F_i$ 都是同一期望 $F_i$ 的无偏估计，两者接近说明「样本内优势」来自真实性质而非过拟合噪声。$M \gg N$ 的设计同样有讲究——验证读数自身的随机波动必须小于待检测的偏差，否则检验失去分辨力，这与 6.006 中「测试规模要覆盖待验证性质」的思想一致。若验证不过关，处方只有一张：增大 $N$ 重新求解，因为误差源头是样本量而非求解算法。

<!-- p.14 -->

### <span style="color:#2471a3;">**[example]**</span> <span style="color:#c0392b;">**例子**</span>

- 考虑问题：

```math
\begin{array}{ll}
\text{minimize} & F_0(x) = \mathbb{E} \max_i (Ax + b)_i \\
\text{subject to} & F_1(x) = \mathbb{E} \max_i (Cx + d)_i \le 0
\end{array}
```

  其中优化变量 $x \in \mathbb{R}^n$；$A \in \mathbb{R}^{m \times n}$，$b \in \mathbb{R}^m$，$C \in \mathbb{R}^{k \times n}$，$d \in \mathbb{R}^k$ 均为随机的。
- 具体算例取 $n = 10$，$m = 20$，$k = 5$
- 确定性等价最优值给出下界 19.1
- 采用 Monte Carlo 采样，$N = 10,\ 100,\ 1000$
- 验证集规模 $M = 10000$

<!-- p.15 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**结果：不同训练样本量下的训练/验证对比**</span>

| 指标 | $N = 10$ | $N = 100$ | $N = 1000$ |
|---|---|---|---|
| $F_0$（训练集 training） | 51.8 | 54.0 | 55.4 |
| $F_0$（验证集 validation） | 56.0 | 54.8 | 55.2 |
| $F_1$（训练集 training） | 0 | 0 | 0 |
| $F_1$（验证集 validation） | 1.3 | 0.7 | $-0.03$ |

由此我们得出结论：

- $N = 10$ 样本太少
- $N = 100$ 好一些，但仍不够
- $N = 1000$ 大概就够用了

<!-- p.16 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**需求不确定的生产计划**</span>

- 制造 $m$ 种成品，数量为 $q = (q_1, \ldots, q_m)$
- 采购原材料数量为 $r = (r_1, \ldots, r_n)$，成本向量为 $c = (c_1, \ldots, c_n)$，故总成本为 $c^T r$
- 制造过程要求 $r \succeq Aq$，其中 $A_{ij}$ 是生产一单位成品 $j$ 所需原材料 $i$ 的数量
- 产品需求 $d = (d_1, \ldots, d_m)$ 是随机的，分布已知
- 产品价格为 $p = (p_1, \ldots, p_m)$，故总收入为 $p^T \min(d, q)$（销量取需求与产量的较小者）
- 在优化变量 $q, r$ 上最大化（期望）净收入：

```math
\begin{array}{ll}
\text{maximize} & \mathbb{E}\, p^T \min(d, q) - c^T r \\
\text{subject to} & r \succeq Aq, \qquad q \succeq 0, \qquad r \succeq 0
\end{array}
```

<!-- p.17 -->

### <span style="color:#2471a3;">**[example]**</span> <span style="color:#c0392b;">**算例**</span>

- 算例规模 $n = 10$，$m = 5$，需求 $d$ 服从对数正态分布（log-normal）
- 确定性等价问题给出上界 170.7
- 采用 Monte Carlo 采样，训练样本数 $N = 2000$
- 用 $M = 10000$ 个验证样本进行验证

| 指标 | 数值 |
|---|---|
| $F_0$（训练集） | 155.7 |
| $F_0$（验证集） | 155.1 |
| CE（用 $\bar{d}$ 的确定性等价） | 170.7 |
| CE（验证集） | 141.1 |

<!-- p.18 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**净收入分布对比（图示）**</span>

（纯图示 slide 说明：图中为三张横轴范围约 100–300 的直方图，分别对应 training set stochastic solution（训练集随机解）、validation set stochastic solution（验证集随机解）与 validation set CE solution（验证集上的 CE 解），用于对比三种解在大量需求样本下净收入的经验分布；结合上一页数据，CE 解在真实分布上的取值（141.1）明显低于随机解（约 155）。）

<!-- p.19 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**最小平均损失预测**</span>

- $(x, y) \in \mathbb{R}^n \times \mathbb{R}$ 服从某一联合分布（joint distribution）
- 寻找权重向量 $w \in \mathbb{R}^n$，使 $w^T x$ 成为 $y$ 的良好估计量（estimator）
- 选取 $w$ 以最小化凸损失函数（convex loss function）$l$ 的期望：

```math
J(w) = \mathbb{E}\, l(w^T x - y)
```

  - $l(u) = u^2$：均方误差（mean-square error）
  - $l(u) = |u|$：平均绝对误差（mean-absolute error）

- 我们并不知道该联合分布，但手头有独立样本（「训练数据」training data）：

```math
(x_i, y_i), \qquad i = 1, \ldots, N
```

<!-- p.20 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**训练与泛化**</span>

- Monte Carlo 采样方法（此处称为「训练」training）：选取 $w$ 以最小化样本平均损失：

```math
w^{sa} = \arg\min_w \Big( \frac{1}{N} \sum_{i=1}^N l(w^T x_i - y_i) \Big)
```

  并记相应的样本平均损失为 $J^{sa}$。
- 在另一组 $M$ 个样本上验证预测器 $y \approx w_{sa}^T x$：

```math
J^{val} = \frac{1}{M} \sum_{i=1}^M l\big(w_{sa}^T x^{val}_i - y^{val}_i\big)
```

- 若 $J^{sa} \approx J^{val}$（且 $M$ 足够大），我们称该预测器能够泛化（generalizes）

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 把损失函数一换，这个框架就把你学过的多种回归统一起来了。取 $l(u)=u^2$ 时，最小化样本平均损失正是 Strang 线代中的（线性）最小二乘（least-squares）：正规方程 $(X^TX)w=X^Ty$ 给出闭式解，几何上是把 $y$ 正交投影到 $X$ 的列空间；取 $l(u)=|u|$ 时问题变成线性规划，最优性由次梯度条件刻画，估计的是条件中位数；而下一页的非对称损失 $l(u)=(u)_+ + 4(u)_-$ 相当于对低估四倍收费，典型动机是备货不足远比库存积压昂贵（报童式权衡），其最优解从条件均值移向条件高分位数——这正是下一讲「分位数优化」的伏笔。整页结构与前面的随机规划完全同构：$J(w)$ 对应 $F_i$，$J^{sa}$ 对应 $\hat F_i$，$J^{val}$ 对应样本外验证，「泛化」即训练/验证读数吻合——统计学习与随机规划在此合流，共享大数定律这同一套基础设施。

<!-- p.21 -->

### <span style="color:#2471a3;">**[example]**</span> <span style="color:#c0392b;">**例子：非对称损失**</span>

- 取 $n = 10$；训练样本 $N = 1000$；验证样本 $M = 10000$
- 损失取 $l(u) = (u)_+ + 4(u)_-$（其中 $u^+ = \max\{u, 0\}$、$u^- = \max\{-u, 0\}$；低估要付出 4 倍的代价）

（纯图示 slide 说明：两张直方图分别给出 training set prediction errors（训练集预测误差）与 validation set prediction errors（验证集预测误差），横轴为预测误差（约 $-0.2$ 至 $0.4$），纵轴为频数；对比两组误差分布是否接近，即可检验预测器是否泛化。）
