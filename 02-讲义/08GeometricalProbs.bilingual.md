# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

> <span style="color:#7f8c8d;">Source: SEE官方讲义/08GeometricalProbs.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

<!-- p.1 -->

## <span style="color:#2471a3;">**[lecture]**</span> <span style="color:#c0392b">8. Geometric problems（几何问题）</span>

本讲围绕四类以几何结构为核心的凸优化应用展开：

- **extremal volume ellipsoids（极值体积椭球）**：围绕一个集合求最小体积椭球，或在集合内部求最大体积内切椭球；
- **centering（中心化）**：为凸集定义并计算各种意义上的"中心"；
- **classification（分类）**：用线性或非线性判别函数把两组点分开；
- **placement and facility location（布局与设施选址）**：在成对代价函数下优化一组点的位置。

> <span style="color:#7f8c8d;">Slide 8–1</span>

---

<!-- p.2 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Minimum volume ellipsoid around a set（围绕集合的最小体积椭球）</span>

<span style="color:#00838f;">集合 $C$ 的 **Löwner-John ellipsoid（Löwner-John 椭球）**：满足 $C \subseteq E$ 的所有椭球中体积最小的那个 **minimum volume ellipsoid（最小体积椭球，MVE）**。</span>

- 把 $E$ 参数化为 $E = \{v \mid \|Av+b\|_2 \le 1\}$；不失一般性（w.l.o.g.）可设 $A \in \mathbf{S}^n_{++}$（对称正定矩阵集合）；
- 椭球体积 $\operatorname{vol} E$ 与 $\det A^{-1}$ 成正比；于是计算最小体积椭球即在 $(A, b)$ 上求解：

```math
\begin{array}{ll}
\text{minimize} & \log\det A^{-1} \\
\text{subject to} & \sup_{v\in C}\|Av+b\|_2 \le 1
\end{array}
```

该问题是凸的，但对一般的集合 $C$，约束中的上确界（supremum）可能难以求值。

当 $C = \{x_1, \ldots, x_m\}$ 是有限点集时：

```math
\begin{array}{ll}
\text{minimize} & \log\det A^{-1} \\
\text{subject to} & \|Ax_i+b\|_2 \le 1, \quad i=1,\ldots,m
\end{array}
```

这一形式同时也给出了多面体（polyhedron）$\operatorname{conv}\{x_1,\ldots,x_m\}$ 的 Löwner-John 椭球。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这个名字纪念两位数学家：Karel Löwner（移居美国后改名 Charles Loewner）在 1930 年代的布拉格讲座中证明了最小体积外包椭球的唯一性但未发表；Fritz John 在 1948 年把这类含不等式约束的极值问题系统化，并给出下一页将出现的按因子 $1/n$ 收缩的结论。Löwner 1939 年因纳粹占领捷克而流亡美国，1951 年起任 Stanford 教授直至 1968 年去世——你读的正是 Stanford 讲义；他还是 Loewner 微分方程（1923）的创立者，该方程在 2000 年被 Schramm 随机化为 SLE。目标函数为何取 $\log\det A^{-1}$：椭球各半轴长是 $A$ 的特征值的倒数，体积正比于 $1/\det A$（把 $A=Q\Lambda Q^T$ 分解，$\det$ 就是特征值之积——Strang 线性代数里的老朋友），故最小化体积等价于最大化 $\det A$。而真正必须取对数的原因是凸性：$\det$ 在 $\mathbf{S}^n_{++}$ 上既不凸也不凹，$\log\det$ 才是凹的，再取负号才得到凸目标——"取对数换凸性"与最大似然估计中对数化似然是同一手法。

> <span style="color:#7f8c8d;">Slide 8–2</span>

---

<!-- p.3 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Maximum volume inscribed ellipsoid（最大体积内切椭球）</span>

在凸集 $C \subseteq \mathbb{R}^n$ 内部求体积最大的椭球 $E$。

- 把 $E$ 参数化为 $E = \{Bu + d \mid \|u\|_2 \le 1\}$；不失一般性可设 $B \in \mathbf{S}^n_{++}$；
- $\operatorname{vol} E$ 与 $\det B$ 成正比；可通过求解下述问题得到 $E$：

```math
\begin{array}{ll}
\text{maximize} & \log\det B \\
\text{subject to} & \sup_{\|u\|_2\le 1} I_C(Bu+d) \le 0
\end{array}
```

其中 $I_C$ 是 $C$ 的 indicator function（示性函数）：$I_C(x) = 0$（当 $x \in C$），$I_C(x) = +\infty$（当 $x \notin C$）。

问题同样是凸的，但对一般的 $C$，约束求值可能很困难。

当 $C$ 是多面体 $\{x \mid a_i^T x \le b_i,\ i=1,\ldots,m\}$ 时：

```math
\begin{array}{ll}
\text{maximize} & \log\det B \\
\text{subject to} & \|Ba_i\|_2 + a_i^T d \le b_i, \quad i=1,\ldots,m
\end{array}
```

（约束由恒等式 $\sup_{\|u\|_2\le 1} a_i^T(Bu+d) = \|Ba_i\|_2 + a_i^T d$ 得到。）

> <span style="color:#7f8c8d;">Slide 8–3</span>

---

<!-- p.4 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Efficiency of ellipsoidal approximations（椭球近似的效率）</span>

设 $C \subseteq \mathbb{R}^n$ 为凸、有界、且具有非空内部（nonempty interior）的集合：

- Löwner-John 椭球按因子 $n$ 收缩后落在 $C$ 之内；
- 最大体积内切椭球按因子 $n$ 扩张后覆盖整个 $C$。

原 slide 以 $\mathbb{R}^2$ 中的两个多面体为例用图示展示了这一收缩/扩张关系。若 $C$ 是对称的（symmetric，即关于原点中心对称），因子 $n$ 可以改进为 $\sqrt{n}$。

> <span style="color:#7f8c8d;">Slide 8–4</span>

---

<!-- p.5 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Centering（中心化）</span>

凸集 $C$ 的"中心"有若干种可能的定义：

- **最大内切球（largest inscribed ball）的球心**——即 **Chebyshev center（切比雪夫中心）**：对多面体可以用线性规划（LP）计算（见第 4 讲 page 4–19）；
- **最大体积内切椭球（MVE）的中心**（原文注 page 1–3，所指即本讲 Slide 8–3）。

原 slide 用图示对比了同一集合的 $x_{\text{cheb}}$ 与 $x_{\text{mve}}$，两者一般并不重合。

**MVE 中心在仿射坐标变换（affine coordinate transformation）下保持不变。**

> <span style="color:#7f8c8d;">Slide 8–5</span>

---

<!-- p.6 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Analytic center of a set of inequalities（一组不等式的解析中心）</span>

对一组凸不等式与线性方程 $f_i(x) \le 0,\ i = 1,\ldots,m$，$Fx = g$：

<span style="color:#00838f;">其 **analytic center（解析中心）** 定义为下述问题的最优点：</span>

```math
\begin{array}{ll}
\text{minimize} & -\sum_{i=1}^m \log(-f_i(x)) \\
\text{subject to} & Fx = g
\end{array}
```

- 比 MVE 中心或切比雪夫中心更容易计算（见后文）；
- 它不只是可行集（feasible set）的属性：两组不同的不等式可以描述同一个集合，但解析中心不同。

> <span style="color:#7f8c8d;">Slide 8–6</span>

---

<!-- p.7 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">线性不等式的解析中心及其内、外椭球</span>

对线性不等式组 $a_i^T x \le b_i,\ i = 1,\ldots,m$，解析中心 $x_{\text{ac}}$ 是

```math
\varphi(x) = -\sum_{i=1}^m \log(b_i - a_i^T x)
```

的极小点。

由解析中心可给出内、外两个椭球：

```math
E_{\text{inner}} \subseteq \{x \mid a_i^T x \le b_i,\ i = 1,\ldots,m\} \subseteq E_{\text{outer}}
```

其中

```math
\begin{aligned}
E_{\text{inner}} &= \{x \mid (x - x_{\text{ac}})^T \nabla^2\varphi(x_{\text{ac}})\,(x - x_{\text{ac}}) \le 1\} \\
E_{\text{outer}} &= \{x \mid (x - x_{\text{ac}})^T \nabla^2\varphi(x_{\text{ac}})\,(x - x_{\text{ac}}) \le m(m-1)\}
\end{aligned}
```

两个椭球由同一个 Hessian（海森矩阵）$\nabla^2\varphi(x_{\text{ac}})$ 确定，仅半径因子不同（$1$ 与 $m(m-1)$）。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 解析中心最反直觉之处：它甚至不由集合唯一决定——把某条约束重复写两遍，中心就会被拉向那面"墙"（而等价的缩放 $2a_i^Tx \le 2b_i$ 毫无影响，因为 $-\log(2s)=-\log s-\log 2$ 只差常数）。Slide 8–6 承诺的"更容易计算"，靠的正是 $\varphi(x)=-\sum_i \log(b_i-a_i^Tx)$ 这种 logarithmic barrier（对数障碍）结构：松弛量越小惩罚越猛，最优点因此天然远离每张约束墙面。它与后面内点法一讲的 central path（中心路径）是同一套数学：对线性规划可以证明，中心路径在障碍参数 $t\to\infty$ 时的极限恰好落在最优面（optimal face）的解析中心上。"好算"的深层原因属于 Nesterov 与 Nemirovski（1994）的 self-concordance（自和谐）理论：对数障碍属于该函数类，牛顿法由此获得干净的多项式时间收敛分析（本课程后面专讲）。最后看代数：$\nabla^2\varphi(x)=\sum_i \frac{a_ia_i^T}{(b_i-a_i^Tx)^2}$ 是外积的加权和、恒为半正定——这正是本页两个椭球能共用同一个 Hessian 表达式的原因。

> <span style="color:#7f8c8d;">Slide 8–7</span>

---

<!-- p.8 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Linear discrimination（线性判别）</span>

用一个超平面（hyperplane）分开两组点 $\{x_1, \ldots, x_N\}$ 与 $\{y_1, \ldots, y_M\}$：

```math
a^T x_i + b > 0,\quad i = 1,\ldots,N; \qquad a^T y_i + b < 0,\quad i = 1,\ldots,M
```

问题关于 $a, b$ 是齐次的（homogeneous），因此可规范化为等价形式：

```math
a^T x_i + b \ge 1,\quad i = 1,\ldots,N; \qquad a^T y_i + b \le -1,\quad i = 1,\ldots,M
```

这是关于变量 $a, b$ 的一组线性不等式。

> <span style="color:#7f8c8d;">Slide 8–8</span>

---

<!-- p.9 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Robust linear discrimination（鲁棒线性判别）</span>

两个超平面

```math
\mathcal{H}_1 = \{z \mid a^T z + b = 1\}, \qquad \mathcal{H}_2 = \{z \mid a^T z + b = -1\}
```

之间的（欧氏）距离为 $\operatorname{dist}(\mathcal{H}_1, \mathcal{H}_2) = 2/\|a\|_2$。

要以最大间隔（maximum margin）分开两组点，求解

```math
\begin{array}{ll}
\text{minimize} & (1/2)\|a\|_2 \\
\text{subject to} & a^T x_i + b \ge 1, \quad i=1,\ldots,N \\
& a^T y_i + b \le -1, \quad i=1,\ldots,M
\end{array}
```

（1）

（把目标函数平方后）它是关于 $a, b$ 的一个二次规划（QP）。

> <span style="color:#7f8c8d;">Slide 8–9</span>

---

<!-- p.10 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">最大间隔分离问题 (1) 的 Lagrange dual（拉格朗日对偶）</span>

问题 (1) 的拉格朗日对偶问题为

```math
\begin{array}{ll}
\text{maximize} & \mathbf{1}^T\lambda + \mathbf{1}^T\mu \\
\text{subject to} & 2\left\|\sum_{i=1}^N \lambda_i x_i - \sum_{i=1}^M \mu_i y_i\right\|_2 \le 1 \\
& \mathbf{1}^T\lambda = \mathbf{1}^T\mu, \qquad \lambda \succeq 0,\ \mu \succeq 0
\end{array}
```

（2）

由对偶性（duality），对偶最优值等于最大间隔的倒数（inverse）。

**解释（interpretation）：**

- 换变量 $\theta_i = \lambda_i / \mathbf{1}^T\lambda$，$\gamma_i = \mu_i / \mathbf{1}^T\mu$，$t = 1/(\mathbf{1}^T\lambda + \mathbf{1}^T\mu)$；
- 把目标取倒数，改为极小化 $1/(\mathbf{1}^T\lambda + \mathbf{1}^T\mu) = t$：

```math
\begin{array}{ll}
\text{minimize} & t \\
\text{subject to} & \left\|\sum_{i=1}^N \theta_i x_i - \sum_{i=1}^M \gamma_i y_i\right\|_2 \le t \\
& \theta \succeq 0,\ \mathbf{1}^T\theta = 1, \qquad \gamma \succeq 0,\ \mathbf{1}^T\gamma = 1
\end{array}
```

其最优值恰为两个凸包（convex hulls）之间的距离。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 对偶问题的最优值恰是两个凸包之间的欧氏距离——换元后的 $\theta, \gamma$ 是非负、和为一的权重，$\sum_i \theta_i x_i$ 与 $\sum_i \gamma_i y_i$ 正是两个凸包中的点，"极小化 $t$ 使两凸包中存在距离不超过 $t$ 的点对"就是凸包间最近距离的定义本身，这也是"对偶最优值 = 最大间隔的倒数"的几何对偶面。对偶变量还藏着漂亮结构：由第 5 讲的 complementary slackness（互补松弛），只有恰落在间隔边界 $a^Tx_i + b = 1$ 上的样本才可能有 $\lambda_i > 0$——这些点就是 support vectors（支持向量），删掉其余任何样本都不改变分类器，"支持向量机"由此得名。再看下一页的基函数 $F(z)$：把 $x$ 换成特征映射后，对偶形式只出现内积 $F(x_i)^T F(x_j)$，于是可以不显式构造 $F$、直接用 kernel（核函数）$K(x_i, x_j)$ 代替——Boser、Guyon 与 Vapnik 在 1992 年的 COLT 会议上据此提出 kernel trick（核技巧），把线性判别一举推广到非线性而计算量不变。

> <span style="color:#7f8c8d;">Slide 8–10</span>

---

<!-- p.11 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Approximate linear separation of non-separable sets（不可分集合的近似线性分离）</span>

当两组点无法被超平面严格分开时，引入松弛变量（slack variables）$u, v$：

```math
\begin{array}{ll}
\text{minimize} & \mathbf{1}^T u + \mathbf{1}^T v \\
\text{subject to} & a^T x_i + b \ge 1 - u_i, \quad i=1,\ldots,N \\
& a^T y_i + b \le -1 + v_i, \quad i=1,\ldots,M \\
& u \succeq 0, \quad v \succeq 0
\end{array}
```

- 这是关于 $a, b, u, v$ 的一个线性规划（LP）；
- 在最优点处，$u_i = \max\{0,\ 1 - a^T x_i - b\}$，$v_i = \max\{0,\ 1 + a^T y_i + b\}$；
- 可以解释为一种极小化误分类点数（#misclassified points）的启发式（heuristic）方法。

> <span style="color:#7f8c8d;">Slide 8–11</span>

---

<!-- p.12 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Support vector classifier（支持向量分类器）</span>

```math
\begin{array}{ll}
\text{minimize} & \|a\|_2 + \gamma(\mathbf{1}^T u + \mathbf{1}^T v) \\
\text{subject to} & a^T x_i + b \ge 1 - u_i, \quad i=1,\ldots,N \\
& a^T y_i + b \le -1 + v_i, \quad i=1,\ldots,M \\
& u \succeq 0, \quad v \succeq 0
\end{array}
```

它在"间隔的倒数（inverse of margin）"（间隔为 $2/\|a\|_2$）与"分类误差（classification error，以总松弛量 total slack $\mathbf{1}^T u + \mathbf{1}^T v$ 度量）"之间的 trade-off curve（权衡曲线）上给出一个点。

原 slide 展示了与上一页相同的例子在 $\gamma = 0.1$ 时的分类结果。

> <span style="color:#7f8c8d;">Slide 8–12</span>

---

<!-- p.13 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Nonlinear discrimination（非线性判别）</span>

用一个非线性函数 $f$ 分开两组点：

```math
f(x_i) > 0,\quad i = 1,\ldots,N; \qquad f(y_i) < 0,\quad i = 1,\ldots,M
```

- 选取一个线性参数化（linearly parametrized）的函数族

```math
f(z) = \theta^T F(z)
```

其中 $F = (F_1, \ldots, F_k): \mathbb{R}^n \to \mathbb{R}^k$ 是 basis functions（基函数）；
- 于是只需解关于 $\theta$ 的一组线性不等式：

```math
\theta^T F(x_i) \ge 1,\quad i = 1,\ldots,N; \qquad \theta^T F(y_i) \le -1,\quad i = 1,\ldots,M
```

> <span style="color:#7f8c8d;">Slide 8–13</span>

---

<!-- p.14 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">二次判别与多项式判别</span>

**Quadratic discrimination（二次判别）**：取 $f(z) = z^T P z + q^T z + r$，判别条件为

```math
x_i^T P x_i + q^T x_i + r \ge 1, \qquad y_i^T P y_i + q^T y_i + r \le -1
```

还可以添加附加约束（例如 $P \preceq -I$，此时分离边界是一个椭球）。

**Polynomial discrimination（多项式判别）**：取基函数 $F(z)$ 为不超过给定次数的全部 monomials（单项式）。

原 slide 给出两幅图：用椭球分离（separation by ellipsoid）与用 4 次多项式分离（separation by 4th degree polynomial）。

> <span style="color:#7f8c8d;">Slide 8–14</span>

---

<!-- p.15 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">Placement and facility location（布局与设施选址）</span>

- $N$ 个点，坐标为 $x_i \in \mathbb{R}^2$（或 $\mathbb{R}^3$）；
- 部分位置 $x_i$ 是给定的，其余 $x_i$ 是变量；
- 对每一对点，给定一个代价函数（cost function）$f_{ij}(x_i, x_j)$。

**Placement problem（布局问题）**：

```math
\text{minimize} \quad \sum_{i \ne j} f_{ij}(x_i, x_j)
```

变量是各自由点的位置。

**解释（interpretations）：**

- 点代表工厂或仓库（plants / warehouses），$f_{ij}$ 是设施 $i$ 与 $j$ 之间的运输成本（transportation cost）；
- 点代表集成电路（IC）上的单元（cells），$f_{ij}$ 代表连线长度（wirelength）。

> <span style="color:#7f8c8d;">Slide 8–15</span>

---

<!-- p.16 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b">示例：三种代价函数下的最优布局</span>

例子：极小化

```math
\sum_{(i,j) \in A} h(\|x_i - x_j\|_2)
```

其中含 6 个自由点、27 条连接（links），$A$ 为连接集合。

原 slide 给出 $h(z) = z$、$h(z) = z^2$、$h(z) = z^4$ 三种代价下的最优布局图，以及对应的连接长度 $\|x_i - x_j\|_2$ 的直方图（histograms of connection lengths）。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 把三种代价函数排成一条"升级链"来看：$h(z)=z$ 时目标是欧氏范数之和，可以写成二阶锥规划（SOCP）；$h(z)=z^2$ 时是二次规划（QP）；$h(z)=z^4$ 则是惩罚更陡的凸问题——次数越高，长连接的代价被放大得越狠，slide 下方三张直方图显示连接长度分布整体向短端集中。IC 布线偏爱高次惩罚有物理依据：连线的 RC 延迟近似随长度平方增长（电阻、电容都与长度成正比，乘积即平方），长线是时序瓶颈，$z^4$ 以陡惩罚换取更短的右尾。从 6.006 的图论视角看，这是把一个 27 条边的图嵌入平面、极小化边代价的问题——网络可视化中的 force-directed layout（力导向布局，把边视作弹簧、按弹性能量松弛节点位置）是同一思想的启发式版本；区别在于这里的代价是凸的、有全局最优解，而一般图布局问题的能量面非凸、只能得到局部解。

> <span style="color:#7f8c8d;">Slide 8–16</span>
