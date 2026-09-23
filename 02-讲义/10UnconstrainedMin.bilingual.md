# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

> <span style="color:#7f8c8d;">Source: SEE官方讲义/10UnconstrainedMin.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

<!-- p.1 -->

## <span style="color:#c0392b;">10. 无约束最小化（Unconstrained Minimization）</span>

<span style="color:#2471a3;">**[lecture]**</span> 本讲提纲（outline）：

- 术语与假设（terminology and assumptions）
- 梯度下降法（gradient descent method）
- 最速下降法（steepest descent method）
- 牛顿法（Newton's method）
- 自和谐函数（self-concordant functions）
- 实现（implementation）

<!-- p.2 -->

### <span style="color:#2471a3;">**[section]**</span> 无约束最小化（Unconstrained minimization）（10–2）

考虑无约束最小化问题：

```math
\operatorname{minimize}\quad f(x)
```

- $f$ 凸，且 twice continuously differentiable（二阶连续可微，因此 $\operatorname{dom} f$ 为开集）
- 假设 optimal value（最优值）$p^\star = \inf_x f(x)$ 能够 attained（达到，且有限）

无约束最小化方法（unconstrained minimization methods）：

- 产生点列 $x^{(k)} \in \operatorname{dom} f$（$k = 0, 1, \ldots$），满足 $f(x^{(k)}) \to p^\star$
- 可以解释为求解 optimality condition（最优性条件）$\nabla f(x^\star) = 0$ 的迭代方法

<!-- p.3 -->

### <span style="color:#2471a3;">**[section]**</span> 初始点与下水平集（Initial point and sublevel set）（10–3）

本章算法要求起点 $x^{(0)}$ 满足：

- $x^{(0)} \in \operatorname{dom} f$
- sublevel set（下水平集）$S = \{x \mid f(x) \le f(x^{(0)})\}$ 为 closed（闭集）

第 2 个条件很难验证，除非所有下水平集都是闭的：

- 等价于 epigraph（上图）$\operatorname{epi} f$ 为闭集
- 若 $\operatorname{dom} f = \mathbb{R}^n$，该条件成立
- 若当 $x \to \operatorname{bd}\,\operatorname{dom} f$ 时 $f(x) \to \infty$，该条件成立

具有闭下水平集的可微函数例子：

```math
f(x) = \log\Big(\sum_{i=1}^{m}\exp(a_i^T x + b_i)\Big),\qquad
f(x) = -\sum_{i=1}^{m}\log(b_i - a_i^T x)
```

<!-- p.4 -->

### <span style="color:#2471a3;">**[section]**</span> 强凸性及其推论（Strong convexity and implications）（10–4）

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">若存在 $m > 0$ 使得 $\nabla^2 f(x) \succeq mI$ 对所有 $x \in S$ 成立，则称 $f$ 在 $S$ 上 strongly convex（强凸）。</span>

推论（implications）：

- 对 $x, y \in S$，有

```math
f(y) \ge f(x) + \nabla f(x)^T (y - x) + \frac{m}{2}\|x - y\|_2^2
```

  因此 $S$ bounded（有界）。

- $p^\star > -\infty$，且对 $x \in S$，有

```math
f(x) - p^\star \le \frac{1}{2m}\|\nabla f(x)\|_2^2
```

  若已知 $m$，这条不等式可用作 stopping criterion（停止准则）。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 强凸性本质上是把 Strang 线代里的正定性从"单点成立"升级为"全域一致成立"。$\nabla^2 f(x) \succeq mI$ 的含义是：Hessian 这个对称矩阵的最小特征值处处不小于 $m$，这正是 18.06 中"对称矩阵半正定 ⟺ 特征值全非负"的加一致下界版本。第一条不等式可由泰勒展开加特征值下界直接推出：展开的二阶余项被 $\frac{m}{2}\|x-y\|_2^2$ 从下方托住，函数在任何两点之间都坐在一口"抛物线碗"之上。两个推论分工明确：下水平集有界保证"闭集 + 有界 = 紧集"，最优值因此确实能够达到，恰好补上了上一页那个难以验证的条件。第二条不等式则把"离最优值多远"折算成"梯度范数多大"，梯度范数因此有资格充当停止准则。请记住这个 $m$：后面经典收敛分析中的全部常数、以及 condition number（条件数）$\kappa = L/m$，都是从这条特征值下界长出来的。

<!-- p.5 -->

### <span style="color:#2471a3;">**[section]**</span> 下降方法（Descent methods）（10–5）

迭代形式：

```math
x^{(k+1)} = x^{(k)} + t^{(k)}\Delta x^{(k)}
```

并要求 $f(x^{(k+1)}) < f(x^{(k)})$。

- 其他记号：$x^+ = x + t\Delta x$，或 $x := x + t\Delta x$
- $\Delta x$ 称为 step（步）或 search direction（搜索方向）；$t$ 称为步长（step size，或称 step length）
- 由凸性，$f(x^+) < f(x)$ 蕴含 $\nabla f(x)^T \Delta x < 0$（即 $\Delta x$ 是 descent direction（下降方向））

<span style="color:#2471a3;">**[algorithm]**</span> **一般下降方法（general descent method）**

- 给定起点 $x \in \operatorname{dom} f$。
- 重复：
  1. 确定一个下降方向 $\Delta x$。
  2. **线搜索（line search）**：选择步长 $t > 0$。
  3. **更新**：$x := x + t\Delta x$。
- 直至满足停止准则。

<!-- p.6 -->

### <span style="color:#2471a3;">**[section]**</span> 线搜索的类型（Line search types）（10–6）

**exact line search（精确线搜索）**：

```math
t = \operatorname*{argmin}_{t>0} f(x + t\Delta x)
```

**backtracking line search（回溯线搜索）**（参数 $\alpha \in (0, 1/2)$，$\beta \in (0, 1)$）：

- 从 $t = 1$ 开始，重复 $t := \beta t$，直到

```math
f(x + t\Delta x) < f(x) + \alpha t \nabla f(x)^T \Delta x
```

- 图形解释：不断回溯，直到 $t \le t_0$。

图：横轴为 $t$，曲线为 $f(x + t\Delta x)$；两条直线分别是一阶泰勒近似 $f(x) + t\nabla f(x)^T\Delta x$ 与打折后的 $f(x) + \alpha t\nabla f(x)^T\Delta x$；从 $t = 1$ 出发按 $\beta$ 逐步缩短 $t$，直到落入 $t \le t_0$ 的可接受区间。

<!-- p.7 -->

### <span style="color:#2471a3;">**[section]**</span> 梯度下降法（Gradient descent method）（10–7）

在一般下降方法中取 $\Delta x = -\nabla f(x)$：

<span style="color:#2471a3;">**[algorithm]**</span> **梯度下降法**

- 给定起点 $x \in \operatorname{dom} f$。
- 重复：
  1. $\Delta x := -\nabla f(x)$。
  2. **线搜索**：用精确或回溯线搜索选择步长 $t$。
  3. **更新**：$x := x + t\Delta x$。
- 直至满足停止准则。

- 停止准则通常取 $\|\nabla f(x)\|_2 \le \epsilon$ 的形式。
- 收敛结果：对强凸的 $f$，

```math
f(x^{(k)}) - p^\star \le c^k\left(f(x^{(0)}) - p^\star\right)
```

  其中 $c \in (0,1)$ 依赖于 $m$、$x^{(0)}$ 与线搜索类型。
- 非常简单，但常常非常慢；实践中很少使用。

<!-- p.8 -->

### <span style="color:#2471a3;">**[section]**</span> $\mathbb{R}^2$ 中的二次问题（quadratic problem in $\mathbb{R}^2$）（10–8）

<span style="color:#2471a3;">**[example]**</span>

```math
f(x) = \tfrac{1}{2}\left(x_1^2 + \gamma x_2^2\right)\qquad (\gamma > 0)
```

采用精确线搜索，起点 $x^{(0)} = (\gamma, 1)$：

```math
x_1^{(k)} = \gamma\left(\frac{\gamma - 1}{\gamma + 1}\right)^{k},\qquad
x_2^{(k)} = \left(\frac{-\gamma + 1}{\gamma + 1}\right)^{k}
```

- 当 $\gamma \gg 1$ 或 $\gamma \ll 1$ 时非常慢。
- 下图是 $\gamma = 10$ 的例子（横轴 $x_1$、纵轴 $x_2$，范围约 $[-10,10]\times[-4,4]$，图中标出 $x^{(0)}$ 与 $x^{(1)}$）。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这个 $\mathbb{R}^2$ 的小例子值得用 Strang 的谱观点亲手算一遍。目标函数的 Hessian 是对角阵 $\operatorname{diag}(1, \gamma)$，两个特征值是 $1$ 与 $\gamma$，条件数恰为 $\kappa = \gamma$。梯度下降每一步会把各特征向量方向上的误差分量乘上因子 $(1 - t\lambda_i)$；精确线搜索挑了最优的 $t$ 之后，最坏方向的收缩比正好是 $(\gamma - 1)/(\gamma + 1)$，也就是上面闭式解的公比。所以 $\gamma \gg 1$ 时每步几乎削不掉误差：这不是梯度法"笨"，而是病态问题在欧氏度量下本来就走不动，与 18.065 里条件数放大误差的讨论一脉相承。换成 6.006 的视角：这种"线性收敛"意味着误差每少一个数量级只需常数次迭代，总迭代次数是 $O(\log(1/\epsilon))$，对精度是对数的，和二分搜索同量级。真正的痛点是迭代次数对 $\kappa$ 线性增长——这正是后面共轭梯度法与牛顿法要解决的对象。

<!-- p.9 -->

### <span style="color:#2471a3;">**[section]**</span> 非二次例子（nonquadratic example）（10–9）

<span style="color:#2471a3;">**[example]**</span>

```math
f(x_1, x_2) = e^{x_1 + 3x_2 - 0.1} + e^{x_1 - 3x_2 - 0.1} + e^{-x_1 - 0.1}
```

图：两个子图分别给出 backtracking line search（回溯线搜索）与 exact line search（精确线搜索）的迭代轨迹，并标出 $x^{(0)}$、$x^{(1)}$、$x^{(2)}$：可见两种线搜索得到的迭代路径明显不同。

<!-- p.10 -->

### <span style="color:#2471a3;">**[section]**</span> $\mathbb{R}^{100}$ 中的问题（a problem in $\mathbb{R}^{100}$）（10–10）

<span style="color:#2471a3;">**[example]**</span>

```math
f(x) = c^T x - \sum_{i=1}^{500} \log(b_i - a_i^T x)
```

图：横轴为迭代次数 $k$（0–200），纵轴为半对数刻度的 $f(x^{(k)}) - p^\star$（$10^{-4}$ 至 $10^4$），画出 exact l.s.（精确线搜索）与 backtracking l.s.（回溯线搜索）两条曲线。

- 『线性』收敛（linear convergence），即在半对数（semilog）图上是一条直线。

<!-- p.11 -->

### <span style="color:#2471a3;">**[section]**</span> 最速下降法（Steepest descent method）（10–11）

<span style="color:#2471a3;">**[definition]**</span> **normalized steepest descent direction（归一化最速下降方向）**（在 $x$ 处，关于范数 $\|\cdot\|$）：

```math
\Delta x_{\mathrm{nsd}} = \operatorname*{argmin}\{\nabla f(x)^T v \mid \|v\| = 1\}
```

解释：对小的 $v$，$f(x + v) \approx f(x) + \nabla f(x)^T v$；方向 $\Delta x_{\mathrm{nsd}}$ 是 directional derivative（方向导数）最负的 unit-norm step（单位范数步）。

**（未归一化）最速下降方向（(unnormalized) steepest descent direction）**：

```math
\Delta x_{\mathrm{sd}} = \|\nabla f(x)\|_* \Delta x_{\mathrm{nsd}}
```

满足 $\nabla f(x)^T \Delta x_{\mathrm{sd}} = -\|\nabla f(x)\|_*^2$（$\|\cdot\|_*$ 为 dual norm（对偶范数））。

**最速下降方法（steepest descent method）**：

- 一般下降方法中取 $\Delta x = \Delta x_{\mathrm{sd}}$。
- 收敛性质与梯度下降法类似。

<!-- p.12 -->

### <span style="color:#2471a3;">**[section]**</span> 例子（examples）（10–12）

- Euclidean norm（欧氏范数）：$\Delta x_{\mathrm{sd}} = -\nabla f(x)$
- quadratic norm（二次范数）$\|x\|_P = (x^T P x)^{1/2}$（$P \in S_{++}^n$）：$\Delta x_{\mathrm{sd}} = -P^{-1}\nabla f(x)$
- $\ell_1$ 范数：$\Delta x_{\mathrm{sd}} = -(\partial f(x)/\partial x_i)\, e_i$，其中 $|\partial f(x)/\partial x_i| = \|\nabla f(x)\|_\infty$

图：二次范数与 $\ell_1$ 范数的单位球（unit balls）及归一化最速下降方向，箭头分别标出 $-\nabla f(x)$ 与 $\Delta x_{\mathrm{nsd}}$：在 $\ell_1$ 单位球（菱形）上，最速下降方向总是取某个坐标轴方向。

<!-- p.13 -->

### <span style="color:#2471a3;">**[section]**</span> 最速下降中范数的选择（choice of norm for steepest descent）（10–13）

图：两个二次范数下带回溯线搜索的最速下降迭代轨迹（标出 $x^{(0)}$、$x^{(1)}$、$x^{(2)}$），椭圆为 $\{x \mid \|x - x^{(k)}\|_P = 1\}$。

- 二次范数下最速下降的等价解释：先做 change of variables（变量替换）$\bar{x} = P^{1/2}x$，再做梯度下降。
- 这说明范数（即 $P$）的选择对收敛速度有很强的影响。

<!-- p.14 -->

### <span style="color:#2471a3;">**[section]**</span> 牛顿步（Newton step）（10–14）

<span style="color:#2471a3;">**[definition]**</span> **Newton step（牛顿步）**：

```math
\Delta x_{\mathrm{nt}} = -\nabla^2 f(x)^{-1}\nabla f(x)
```

解释（interpretations）：

- $x + \Delta x_{\mathrm{nt}}$ 最小化 second order approximation（二阶近似）

```math
\hat{f}(x + v) = f(x) + \nabla f(x)^T v + \tfrac{1}{2} v^T \nabla^2 f(x) v
```

- $x + \Delta x_{\mathrm{nt}}$ 求解 linearized（线性化）的最优性条件：

```math
\nabla f(x + v) \approx \nabla \hat{f}(x + v) = \nabla f(x) + \nabla^2 f(x) v = 0
```

图：上图为 $f$ 与其近似 $\hat{f}$ 在点 $(x, f(x))$ 与 $(x + \Delta x_{\mathrm{nt}}, f(x + \Delta x_{\mathrm{nt}}))$ 处的比较；下图为导数 $f'$ 与 $\hat{f}'$ 的对应关系。

<!-- p.15 -->

### <span style="color:#2471a3;">**[section]**</span> 牛顿步是局部 Hessian 范数下的最速下降方向（10–15）

$\Delta x_{\mathrm{nt}}$ 是 $x$ 处关于 local Hessian norm（局部 Hessian 范数）

```math
\|u\|_{\nabla^2 f(x)} = \left(u^T \nabla^2 f(x) u\right)^{1/2}
```

的最速下降方向。

图：虚线为 $f$ 的等高线（contour lines）；椭圆为 $\{x + v \mid v^T \nabla^2 f(x) v = 1\}$；箭头为 $-\nabla f(x)$；标出 $x$、$x + \Delta x_{\mathrm{nt}}$ 与 $x + \Delta x_{\mathrm{nsd}}$：在 Hessian 范数的椭圆度量下，牛顿方向正是最陡的下降方向。

<!-- p.16 -->

### <span style="color:#2471a3;">**[section]**</span> 牛顿减量（Newton decrement）（10–16）

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">**Newton decrement（牛顿减量）**

```math
\lambda(x) = \left(\nabla f(x)^T \nabla^2 f(x)^{-1} \nabla f(x)\right)^{1/2}
```

它是 $x$ 接近 $x^\star$ 程度的一种度量。</span>

性质（properties）：

- 用二次近似 $\hat{f}$ 给出 $f(x) - p^\star$ 的估计：

```math
f(x) - \inf_y \hat{f}(y) = \tfrac{1}{2}\lambda(x)^2
```

- 等于牛顿步在二次 Hessian 范数下的范数：

```math
\lambda(x) = \left(\Delta x_{\mathrm{nt}}^T \nabla^2 f(x) \Delta x_{\mathrm{nt}}\right)^{1/2}
```

- 沿牛顿方向的方向导数：$\nabla f(x)^T \Delta x_{\mathrm{nt}} = -\lambda(x)^2$
- affine invariant（仿射不变）（与 $\|\nabla f(x)\|_2$ 不同）。

<!-- p.17 -->

### <span style="color:#2471a3;">**[section]**</span> 牛顿法（Newton's method）（10–17）

<span style="color:#2471a3;">**[algorithm]**</span> **牛顿法**

- 给定起点 $x \in \operatorname{dom} f$，tolerance（容差）$\epsilon > 0$。
- 重复：
  1. **计算牛顿步与牛顿减量**：
     $\Delta x_{\mathrm{nt}} := -\nabla^2 f(x)^{-1}\nabla f(x)$；$\lambda^2 := \nabla f(x)^T \nabla^2 f(x)^{-1}\nabla f(x)$。
  2. **停止准则**：若 $\lambda^2/2 \le \epsilon$，退出。
  3. **线搜索**：用回溯线搜索选择步长 $t$。
  4. **更新**：$x := x + t\,\Delta x_{\mathrm{nt}}$。

仿射不变（affine invariant），即与坐标的线性变换无关：对 $\tilde{f}(y) = f(Ty)$，起点取 $y^{(0)} = T^{-1}x^{(0)}$，则牛顿迭代满足

```math
y^{(k)} = T^{-1}x^{(k)}
```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 仿射不变性是牛顿法最深刻的性质，值得用 Strang 的坐标变换语言说透。在线代中，二次型在坐标替换 $x = Ty$ 下按 $T^T A T$（合同变换）变化，而正定性在合同变换下保持——牛顿法把这一点用到了极致：整条迭代序列 $y^{(k)} = T^{-1}x^{(k)}$ 只是同一组迭代换了个坐标系记录而已，一步都不多走。对比之下，梯度法依赖欧氏度量 $\|\nabla f\|_2$，而"哪个方向算最陡"完全取决于用哪把尺子量距离：把某个坐标拉长十倍，迭代路径立刻变形。牛顿法用 Hessian 定义局部范数 $\|u\|_{\nabla^2 f(x)}$，相当于让问题自身的曲率来提供几何，因此对线性坐标变换"视而不见"。这也解释了牛顿减量 $\lambda(x)$ 为什么能充当停止准则：它在线性变换下不变，而 $\|\nabla f(x)\|_2$ 不然。第 13 页"给最速下降选哪个范数"的那个自由度，在牛顿法里被曲率自动敲定了。

<!-- p.18 -->

### <span style="color:#2471a3;">**[section]**</span> 经典收敛分析（Classical convergence analysis）（10–18）

<span style="color:#2471a3;">**[theorem]**</span> <span style="color:#c0392b;">**假设（assumptions）**</span>

- $f$ 在 $S$ 上强凸，常数为 $m$；
- $\nabla^2 f$ 在 $S$ 上 Lipschitz continuous（Lipschitz 连续），常数为 $L > 0$：

```math
\|\nabla^2 f(x) - \nabla^2 f(y)\|_2 \le L\|x - y\|_2
```

  （$L$ 度量 $f$ 能被二次函数近似的好坏）。

**梗概（outline）**：存在常数 $\eta \in (0, m^2/L)$ 与 $\gamma > 0$，使得：

- 若 $\|\nabla f(x)\|_2 \ge \eta$，则 $f(x^{(k+1)}) - f(x^{(k)}) \le -\gamma$；
- 若 $\|\nabla f(x)\|_2 < \eta$，则

```math
\frac{L}{2m^2}\|\nabla f(x^{(k+1)})\|_2 \le \left(\frac{L}{2m^2}\|\nabla f(x^{(k)})\|_2\right)^2
```

<!-- p.19 -->

### <span style="color:#2471a3;">**[section]**</span> 算法的两个阶段（10–19）

**damped Newton phase（阻尼牛顿阶段）**（$\|\nabla f(x)\|_2 \ge \eta$）：

- 大多数迭代需要 backtracking steps（回溯步）；
- 函数值每次至少下降 $\gamma$；
- 若 $p^\star > -\infty$，该阶段至多 $(f(x^{(0)}) - p^\star)/\gamma$ 次迭代后结束。

**quadratically convergent phase（二次收敛阶段）**（$\|\nabla f(x)\|_2 < \eta$）：

- 所有迭代步长均为 $t = 1$；
- $\|\nabla f(x)\|_2$ 二次收敛到零：若 $\|\nabla f(x^{(k)})\|_2 < \eta$，则对 $l \ge k$：

```math
\frac{L}{2m^2}\|\nabla f(x^{(l)})\|_2 \le \left(\frac{L}{2m^2}\|\nabla f(x^{(k)})\|_2\right)^{2^{l-k}} \le \left(\frac{1}{2}\right)^{2^{l-k}}
```

<!-- p.20 -->

### <span style="color:#2471a3;">**[section]**</span> 结论：迭代次数上界（10–20）

使 $f(x) - p^\star \le \epsilon$ 成立所需的迭代次数上界为：

```math
\frac{f(x^{(0)}) - p^\star}{\gamma} + \log_2 \log_2(\epsilon_0/\epsilon)
```

- $\gamma$、$\epsilon_0$ 是依赖于 $m$、$L$、$x^{(0)}$ 的常数；
- 第二项很小（量级约 6），实际应用中几乎恒定；
- 实践中常数 $m$、$L$（从而 $\gamma$、$\epsilon_0$）通常未知；
- 该分析提供对收敛性质的定性洞察（即解释了算法的两个阶段）。

<!-- p.21 -->

### <span style="color:#2471a3;">**[section]**</span> 例子（Examples）：$\mathbb{R}^2$（见 1–9 页）（10–21）

<span style="color:#2471a3;">**[example]**</span> 图：横轴 $k$（0–5），纵轴为半对数刻度的 $f(x^{(k)}) - p^\star$（$10^{-15}$ 至 $10^5$）。

- 回溯参数 $\alpha = 0.1$，$\beta = 0.7$；
- 仅 5 步即收敛；
- 局部二次收敛（quadratic local convergence）。

<!-- p.22 -->

### <span style="color:#2471a3;">**[section]**</span> 例子：$\mathbb{R}^{100}$（见 1–10 页）（10–22）

<span style="color:#2471a3;">**[example]**</span> 图：上图横轴 $k$（0–10），纵轴为半对数刻度的 $f(x^{(k)}) - p^\star$（$10^{-15}$ 至 $10^5$），比较精确线搜索与回溯两条曲线；下图给出步长 $t^{(k)}$ 随 $k$ 的变化（0–2），同样比较两种线搜索。

- 回溯参数 $\alpha = 0.01$，$\beta = 0.5$；
- 回溯线搜索几乎与精确线搜索一样快（且简单得多）；
- 清楚地展示了算法的两个阶段。

<!-- p.23 -->

### <span style="color:#2471a3;">**[section]**</span> 例子：$\mathbb{R}^{10000}$（$a_i$ 稀疏）（10–23）

<span style="color:#2471a3;">**[example]**</span>

```math
f(x) = -\sum_{i=1}^{10000}\log(1 - x_i^2) - \sum_{i=1}^{100000}\log(b_i - a_i^T x)
```

图：横轴 $k$（0–20），纵轴为半对数刻度的 $f(x^{(k)}) - p^\star$（$10^{-5}$ 至 $10^5$）。

- 回溯参数 $\alpha = 0.01$，$\beta = 0.5$；
- 表现与小规模例子类似。

<!-- p.24 -->

### <span style="color:#2471a3;">**[section]**</span> 自和谐（Self-concordance）（10–24）

经典收敛分析的缺点（shortcomings）：

- 依赖未知常数（$m$、$L$ 等）；
- 界不是仿射不变的，尽管牛顿法本身是。

通过 self-concordance（自和谐）的收敛分析（Nesterov 与 Nemirovski）：

- 不依赖任何未知常数；
- 给出仿射不变的界；
- 只适用于一类特殊的凸函数（『自和谐』函数）；
- 最初为分析凸优化的 polynomial-time interior-point methods（多项式时间内点法）而发展。

<!-- p.25 -->

### <span style="color:#2471a3;">**[section]**</span> 自和谐函数（Self-concordant functions）（10–25）

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">**定义**：凸函数 $f : \mathbb{R} \to \mathbb{R}$ 称为 self-concordant（自和谐），若 $|f'''(x)| \le 2 f''(x)^{3/2}$ 对所有 $x \in \operatorname{dom} f$ 成立；$f : \mathbb{R}^n \to \mathbb{R}$ 称为自和谐，若 $g(t) = f(x + tv)$ 对所有 $x \in \operatorname{dom} f$、$v \in \mathbb{R}^n$ 都是自和谐的。</span>

$\mathbb{R}$ 上的例子（examples on $\mathbb{R}$）：

- 线性函数与二次函数；
- 负对数（negative logarithm）$f(x) = -\log x$；
- 负熵加负对数：$f(x) = x\log x - \log x$。

仿射不变性（affine invariance）：若 $f : \mathbb{R} \to \mathbb{R}$ 自和谐，则 $\tilde{f}(y) = f(ay + b)$ 也自和谐：

```math
\tilde{f}'''(y) = a^3 f'''(ay + b),\qquad \tilde{f}''(y) = a^2 f''(ay + b)
```

<!-- p.26 -->

### <span style="color:#2471a3;">**[section]**</span> 自和谐函数的运算（Self-concordant calculus）（10–26）

性质（properties）：

- 在 positive scaling（正缩放）$\alpha \ge 1$ 与求和下保持；
- 在 composition with an affine function（与仿射函数复合）下保持；
- 若 $g$ 凸且 $\operatorname{dom} g = \mathbb{R}_{++}$，且 $|g'''(x)| \le 3g''(x)/x$，则

```math
f(x) = \log(-g(x)) - \log x
```

  是自和谐的。

例子：用上述性质可证明以下函数自和谐：

- $f(x) = -\sum_{i=1}^{m}\log(b_i - a_i^T x)$，定义域为 $\{x \mid a_i^T x < b_i,\ i = 1, \ldots, m\}$；
- $f(X) = -\log\det X$，定义域为 $S_{++}^n$；
- $f(x) = -\log(y^2 - x^T x)$，定义域为 $\{(x, y) \mid \|x\|_2 < y\}$。

<!-- p.27 -->

### <span style="color:#2471a3;">**[section]**</span> 自和谐函数的收敛分析（10–27）

<span style="color:#2471a3;">**[theorem]**</span> <span style="color:#c0392b;">**总结（summary）**</span>：存在常数 $\eta \in (0, 1/4]$ 与 $\gamma > 0$，使得：

- 若 $\lambda(x) > \eta$，则 $f(x^{(k+1)}) - f(x^{(k)}) \le -\gamma$；
- 若 $\lambda(x) \le \eta$，则

```math
2\lambda(x^{(k+1)}) \le \left(2\lambda(x^{(k)})\right)^2
```

（$\eta$ 与 $\gamma$ 只依赖回溯参数 $\alpha$、$\beta$。）

**complexity bound（复杂度界）**：牛顿迭代次数有上界：

```math
\frac{f(x^{(0)}) - p^\star}{\gamma} + \log_2 \log_2(1/\epsilon)
```

取 $\alpha = 0.1$、$\beta = 0.8$、$\epsilon = 10^{-10}$，该界的值为 $375\,(f(x^{(0)}) - p^\star) + 6$。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 把这一页与第 20 页的经典界对照着看，才能体会 Nesterov 与 Nemirovski（1988 年前后）这项工作的分量。经典分析给出的界形式漂亮，但 $\gamma$、$\epsilon_0$ 依赖未知的 $m$、$L$；用 6.006 的标准衡量，一个含未知常数的界根本算不上合格的复杂度证明——你无法事先预测算法要跑多久。自和谐分析把所有常数压缩到只依赖回溯参数 $\alpha, \beta$：代入 $\alpha = 0.1$、$\beta = 0.8$、$\epsilon = 10^{-10}$ 得 $375(f(x^{(0)}) - p^\star) + 6$，这是一个随输入可算、可验证的迭代次数上界。双对数 $\log_2\log_2$ 项意味着从 $10^{-10}$ 一路追到机器精度只多花约 6 步，提高精度几乎"白送"。正是这套技术，构成了内点法多项式时间复杂度证明的内核——也就是"线性规划可多项式时间求解"这条 6.006 教科书事实背后的分析引擎。

<!-- p.28 -->

### <span style="color:#2471a3;">**[section]**</span> 数值例子（numerical example）（10–28）

<span style="color:#2471a3;">**[example]**</span> 150 个随机生成的 instances（实例），求解

```math
\operatorname{minimize}\quad f(x) = -\sum_{i=1}^{m}\log(b_i - a_i^T x)
```

标记：○：$m = 100$，$n = 50$；□：$m = 1000$，$n = 500$；♦：$m = 1000$，$n = 50$。

图：横轴为迭代次数（iterations，0–25），纵轴为 $f(x^{(0)}) - p^\star$（0–35）的散点图。

- 实际迭代次数远小于 $375(f(x^{(0)}) - p^\star) + 6$；
- 经验上，形如 $c\,(f(x^{(0)}) - p^\star) + 6$ 的界以更小的 $c$ 成立。

<!-- p.29 -->

### <span style="color:#2471a3;">**[section]**</span> 实现（Implementation）（10–29）

每次迭代的主要计算量：计算各阶导数，并求解 Newton system（牛顿方程组）

```math
H\Delta x = g,\qquad H = \nabla^2 f(x),\quad g = -\nabla f(x)
```

通过 Cholesky factorization（Cholesky 分解）求解：

```math
H = LL^T,\qquad \Delta x_{\mathrm{nt}} = L^{-T}L^{-1}g,\qquad \lambda(x) = \|L^{-1}g\|_2
```

- unstructured（非结构化）方程组的代价为 $(1/3)n^3$ flops；
- 若 $H$ sparse（稀疏）、banded（带状），代价 $\ll (1/3)n^3$。

<!-- p.30 -->

### <span style="color:#2471a3;">**[section]**</span> 有结构的稠密牛顿方程组示例（10–30）

<span style="color:#2471a3;">**[example]**</span>

```math
f(x) = \sum_{i=1}^{n}\psi_i(x_i) + \psi_0(Ax + b),\qquad H = D + A^T H_0 A
```

- 设 $A \in \mathbb{R}^{p \times n}$ 稠密，且 $p \ll n$；
- $D$ 为对角阵，对角元为 $\psi_i''(x_i)$；$H_0 = \nabla^2\psi_0(Ax + b)$。

**方法 1**：显式构造 $H$，用稠密 Cholesky 分解求解：代价 $(1/3)n^3$。

**方法 2**（见 9–15 页）：分解 $H_0 = L_0 L_0^T$；把牛顿方程组写成

```math
D\Delta x + A^T L_0 w = -g,\qquad L_0^T A \Delta x - w = 0
```

从第一个方程消去 $\Delta x$；由下式计算 $w$ 与 $\Delta x$：

```math
\left(I + L_0^T A D^{-1} A^T L_0\right)w = -L_0^T A D^{-1} g,\qquad D\Delta x = -g - A^T L_0 w
```

代价：$2p^2 n$（由 $L_0^T A D^{-1} A^T L_0$ 的计算主导）。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这一页是全课程"利用结构省算力"思想的缩影。牛顿方程 $H\Delta x = g$ 若当稠密系统硬解，Cholesky 分解要 $(1/3)n^3$ flops；而当 $p \ll n$ 时，方法 2 通过消去 $\Delta x$（本质是 Schur 补，与 9–15 页的块消去是同一招）把问题降到 $p$ 维，代价 $2p^2 n$——若 $n = 10^4$、$p = 10^2$，两者相差约五个数量级。Strang 在 18.06 讲 LU 分解时反复强调"同一个矩阵别重复分解"，在这里兑现成了真金白银。从 6.006 的角度看，这正是"图的稀疏性决定算法选择"的矩阵版：$H$ 稀疏对应问题图稀疏，消元顺序与填充（fill-in）对应图上的搜索策略。工程上一个凸优化问题解得快不快，往往不取决于理论迭代次数，而取决于这一页这种线性代数层面的调度。
