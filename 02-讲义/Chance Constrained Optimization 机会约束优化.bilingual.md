# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

## 机会约束优化 Chance Constrained Optimization

> <span style="color:#7f8c8d;">Source: 补充材料/Chance Constrained Optimization 机会约束优化.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Slide 1 · 讲次封面：机会约束优化（Chance Constrained Optimization）</span>

<!-- p.1 -->

<span style="color:#2471a3;">**[lecture]**</span> 本讲主题：

- 机会约束（chance constraints）与分位数优化（percentile optimization）
- 对数凹分布（log-concave distributions）下的机会约束
- 机会约束的凸近似（convex approximation of chance constraints）

参考资料来源：Rockafellar & Uryasev、Nemirovsky & Shapiro。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Slide 2 · 机会约束与分位数优化（Chance Constraints and Percentile Optimization）</span>

<!-- p.2 -->

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">机会约束（chance constraints），其中 η 称为"置信水平"（confidence level）：</span>

```math
\mathrm{Prob}(f_i(x, \omega) \le 0) \ge \eta
```

- 在某些情形下是凸的（见后文）
- 通常感兴趣的取值是 η = 0.9、0.95、0.99
- η = 0.999 没有意义（除非你确实掌握分布尾部的信息）

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">分位数优化（percentile optimization），其中 γ 称为"η-分位数"（η-percentile）：</span>

```math
\begin{aligned}
\text{minimize} \quad & \gamma \\
\text{subject to} \quad & \mathrm{Prob}(f_0(x, \omega) \le \gamma) \ge \eta
\end{aligned}
```

- 在某些情形下是凸的或拟凸（quasiconvex）的（见后文）

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Slide 3 · 风险价值与条件风险价值（Value-at-Risk and Conditional Value-at-Risk）</span>

<!-- p.3 -->

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">随机变量 z 在水平 η 下的风险价值（value-at-risk，VaR）：</span>

```math
\mathrm{VaR}(z; \eta) = \inf\{\gamma \mid \mathrm{Prob}(z \le \gamma) \ge \eta\}
```

- 机会约束 $\mathrm{Prob}(f_i(x, \omega) \le 0) \ge \eta$ 等价于 $\mathrm{VaR}(f_i(x, \omega); \eta) \le 0$

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">条件风险价值（conditional value-at-risk，CVaR）：</span>

```math
\mathrm{CVaR}(z; \eta) = \inf_\beta \left( \beta + \frac{1}{1-\eta}\, \mathrm{E}(z - \beta)^+ \right)
```

- $\mathrm{CVaR}(z; \eta) \ge \mathrm{VaR}(z; \eta)$（后文详述）

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> VaR 其实就是你在概率论（Bertsekas）里学过的分位数：把随机损失 z 的累积分布函数记为 F，VaR(z; η) 是使 Prob(z ≤ γ) ≥ η 成立的最小 γ，也就是损失分布的 η-分位数。CVaR 则把目光从"分位点"移到"分位点之外的尾部"：它是最坏 1−η 比例情形的平均损失，金融里称为预期短缺（expected shortfall）。两者的大小关系 CVaR ≥ VaR 直观上很清楚：尾部的平均损失不会小于尾部的起点。为什么风险管理界最终偏爱 CVaR？因为 VaR 作为约束一般非凸、难以优化，而 CVaR 有一个凸的极小化表述，这正是 Rockafellar 与 Uryasev 在 2000 年前后的工作带来的关键转折。本讲后面所有的凸近似方法，本质上都是在用 CVaR 这类凸的尾部度量去替代非凸的 VaR。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Slide 4 · CVaR 的解读（CVaR Interpretation，针对连续分布）</span>

<!-- p.4 -->

- 在 CVaR 定义中，最优的 β 恰为 $\beta^\star = \mathrm{VaR}(z; \eta)$：对 β 求导得

```math
0 = \frac{d}{d\beta}\left( \beta + \frac{1}{1-\eta}\, \mathrm{E}(z - \beta)^+ \right) = 1 - \frac{1}{1-\eta}\, \mathrm{Prob}(z \ge \beta)
```

于是 $\mathrm{Prob}(z \ge \beta^\star) = 1 - \eta$。

- 条件尾部期望（conditional tail expectation，或称预期短缺 expected shortfall）：

```math
\begin{aligned}
\mathrm{E}(z \mid z \ge \beta^\star)
&= \mathrm{E}(\beta^\star + (z - \beta^\star) \mid z \ge \beta^\star) \\
&= \beta^\star + \frac{\mathrm{E}(z - \beta^\star)^+}{\mathrm{Prob}(z \ge \beta^\star)} \\
&= \mathrm{CVaR}(z; \eta)
\end{aligned}
```

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Slide 5 · 对数凹分布下的机会约束（Chance Constraints for Log-Concave Distributions）</span>

<!-- p.5 -->

- 假设：
  - ω 具有对数凹（log-concave）密度 $p(\omega)$
  - $C = \{(x, \omega) \mid f(x, \omega) \le 0\}$ 关于 $(x, \omega)$ 是凸的
- 则概率

```math
\mathrm{Prob}(f(x, \omega) \le 0) = \int \mathbf{1}((x, \omega) \in C)\, p(\omega)\, d\omega
```

是对数凹的，因为被积函数（integrand）是对数凹的。

- 因此机会约束 $\mathrm{Prob}(f(x, \omega) \le 0) \ge \eta$ 可以表示为凸约束

```math
\log \mathrm{Prob}(f(x, \omega) \le 0) \ge \log \eta
```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这一页的背后是概率论中的 Prékopa 定理（1973）：对数凹密度经过积分（例如边际化）之后仍然是对数凹的——这正是它比"凸密度"更有用的地方，因为积分是计算概率的基本操作，而普通凸性在积分下一般会丢失。从第一性原理看：概率 Prob(f(x,ω) ≤ 0) 等于指标函数 1((x,ω)∈C) 与密度 p(ω) 乘积的积分；C 关于 (x,ω) 凸保证指标函数（取值 0 或 1）是对数凹的，密度对数凹是前提，两者相乘仍对数凹，于是积分结果对数凹。约束 log Prob ≥ log η 的左端是凹函数，"凹函数 ≥ 常数"形式的约束定义的是凸集。你在 18.065 里熟悉的多元高斯密度 exp(−(二次型)/2) 就是最典型的对数凹密度，因此高斯噪声下的这类机会约束可以精确化为凸约束，而不需要任何近似。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Slide 6 · 参数服从正态分布的线性不等式（Linear Inequality with Normally Distributed Parameter）</span>

<!-- p.6 -->

- 考虑 $a^T x \le b$，其中 $a \sim \mathcal{N}(\bar a, \Sigma)$
- 则 $a^T x - b \sim \mathcal{N}(\bar a^T x - b,\; x^T \Sigma x)$
- 于是

```math
\mathrm{Prob}(a^T x \le b) = \Phi\!\left( \frac{b - \bar a^T x}{\sqrt{x^T \Sigma x}} \right)
```

- 因此

```math
\mathrm{Prob}(a^T x \le b) \ge \eta
\;\Longleftrightarrow\;
b - \bar a^T x \ge \Phi^{-1}(\eta)\, \|\Sigma^{1/2} x\|_2
```

当 η ≥ 0.5（即 $\Phi^{-1}(\eta) \ge 0$）时，这是一个二阶锥约束（second-order cone constraint）。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Slide 7 · 投资组合优化示例（Portfolio Optimization Example）</span>

<!-- p.7 -->

<span style="color:#2471a3;">**[example]**</span>

- $x \in \mathbb{R}^n$ 给出投资组合配置（portfolio allocation）；$x_i$ 是资产 i 的（分数）头寸（position）
- x 必须满足 $\mathbf{1}^T x = 1$，$x \in C$（凸的投资组合约束集）
- 组合收益（以百分比计）为 $p^T x$，其中 $p \sim \mathcal{N}(\bar p, \Sigma)$（更符合实际的模型是 p 服从对数正态分布 log-normal）
- 目标：在亏损概率受限的约束下最大化期望收益

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Slide 8 · 问题的凸表述</span>

<!-- p.8 -->

<span style="color:#2471a3;">**[example]**</span> 问题为

```math
\begin{aligned}
\text{maximize} \quad & \mathrm{E}\, p^T x \\
\text{subject to} \quad & \mathrm{Prob}(p^T x \le 0) \le \beta \\
& \mathbf{1}^T x = 1, \quad x \in C
\end{aligned}
```

当 β ≤ 1/2 时，可以表示为凸问题：

```math
\begin{aligned}
\text{maximize} \quad & \bar p^T x \\
\text{subject to} \quad & \bar p^T x \ge \Phi^{-1}(1-\beta)\, \|\Sigma^{1/2} x\|_2 \\
& \mathbf{1}^T x = 1, \quad x \in C
\end{aligned}
```

（当 C 是多面体（polyhedron）时，这是一个 SOCP。）

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Slide 9 · 数值例子</span>

<!-- p.9 -->

<span style="color:#2471a3;">**[example]**</span>

- n = 10 个资产，β = 0.05，$C = \{x \mid x \succeq -0.1\}$（分量不等式，限制卖空幅度）
- 比较三种组合：最优组合、去掉亏损风险约束的最优组合、均匀组合 $(1/n)\mathbf{1}$

| 组合 | $\mathrm{E}\, p^T x$ | $\mathrm{Prob}(p^T x \le 0)$ |
|---|---|---|
| 最优（optimal） | 7.51 | 5.0% |
| 无亏损约束（w/o loss constraint） | 10.66 | 20.3% |
| 均匀（uniform） | 3.41 | 18.9% |

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Slide 10 · 收益分布图（Return Distributions）</span>

<!-- p.10 -->

<span style="color:#2471a3;">**[example]**</span> 纯图示页：三幅收益分布直方图，横轴刻度均为 −20 至 30，从左到右依次为最优组合（optimal）、无亏损约束组合（w/o loss constraint）、均匀组合（uniform）。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Slide 11 · 机会约束界的凸近似（Convex Approximation of Chance Constraint Bound）</span>

<!-- p.11 -->

<span style="color:#2471a3;">**[definition]**</span>

- 假设 $f_i(x, \omega)$ 关于 x 是凸的
- 设 $\phi : \mathbb{R} \to \mathbb{R}$ 非负、凸、非减，且 $\phi(0) = 1$
- 对任意 $\alpha_i > 0$，有 $\phi(z/\alpha_i) \ge \mathbf{1}(z > 0)$ 对所有 z 成立，因此

```math
\mathrm{E}\, \phi(f_i(x, \omega)/\alpha_i) \ge \mathrm{Prob}(f_i(x, \omega) > 0)
```

- 因此（凸）约束

```math
\mathrm{E}\, \phi(f_i(x, \omega)/\alpha_i) \le 1 - \eta
```

保证机会约束 $\mathrm{Prob}(f_i(x, \omega) \le 0) \ge \eta$ 成立

- 这对任意 $\alpha_i > 0$ 都成立；下面说明如何对 $\alpha_i$ 进行优化

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Slide 12 · 通过透视函数化为凸约束</span>

<!-- p.12 -->

- 把约束写成

```math
\mathrm{E}\, \alpha_i \phi(f_i(x, \omega)/\alpha_i) \le \alpha_i (1 - \eta)
```

- （透视函数 perspective function）$v\, \phi(u/v)$ 在 $(u, v)$（v > 0）上是凸的，且关于 u 非减
- 因此复合函数 $\alpha_i \phi(f_i(x, \omega)/\alpha_i)$ 在 $(x, \alpha_i)$（α_i > 0）上是凸的
- 故上述约束关于 x 和 α_i 是凸的
- 于是可以通过凸优化对 x 和 $\alpha_i > 0$ 联合优化
- 这给出一个凸随机优化问题，它是机会约束问题的保守近似（conservative approximation）
- 下面考察几个特殊情形

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 为什么要引入辅助尺度 α_i 并写成透视函数（perspective function）的形式？凸优化书第 3 章给出结论：凸函数 φ 的透视 v φ(u/v) 在 v > 0 时关于 (u, v) 凸，且 α_i φ(f_i/α_i) 关于 α_i 一次齐次，这使 α_i 从"任意固定常数"升级为"可与 x 联合优化的变量"。逐点不等式 φ(z/α_i) ≥ 1(z>0) 是整个构造的根基：指示函数 1(z>0) 本身既不凸也不连续，无法直接进期望约束，于是用一族凸的"上包络"控制它，再借期望保不等号（这正是 Jensen 不等式一侧的思想）得到概率上界。所谓"保守近似"，指满足凸约束的点必然满足原机会约束，即可行集是原问题可行集的子集；对最大化问题而言，近似问题的最优值因此是原问题最优值的下界，第 20 页会正式使用这一事实。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Slide 13 · Markov 机会约束界（Markov Chance Constraint Bound）</span>

<!-- p.13 -->

<span style="color:#2471a3;">**[theorem]**</span>

- 取 $\phi(u) = (u + 1)^+$ 得到 Markov 界（Markov bound）：对任意 $\alpha_i > 0$，

```math
\mathrm{Prob}(f_i(x, \omega) > 0) \le \mathrm{E}\, (f_i(x, \omega)/\alpha_i + 1)^+
```

- 凸近似约束

```math
\mathrm{E}\, \alpha_i (f_i(x, \omega)/\alpha_i + 1)^+ \le \alpha_i (1 - \eta)
```

可以写成

```math
\mathrm{E}\, (f_i(x, \omega) + \alpha_i)^+ \le \alpha_i (1 - \eta)
```

- 我们可以对 x 和 $\alpha_i \ge 0$ 进行优化

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Slide 14 · 通过条件风险价值（CVaR）解读</span>

<!-- p.14 -->

<span style="color:#2471a3;">**[theorem]**</span>

- 把保守近似写成

```math
\frac{\mathrm{E}\, (f_i(x, \omega) + \alpha_i)^+}{1 - \eta} - \alpha_i \le 0
```

- 左端关于 $(x, \alpha_i)$ 是凸的，因此对 α_i 取下确界

```math
\inf_{\alpha_i > 0} \left( \frac{\mathrm{E}\, (f_i(x, \omega) + \alpha_i)^+}{1 - \eta} - \alpha_i \right)
```

后关于 x 是凸的

- 这正是 $\mathrm{CVaR}(f_i(x, \omega); \eta)$（可以证明 α_i > 0 的限制可以去掉）
- 所以凸近似把 $\mathrm{VaR}(f_i(x, \omega); \eta) \le 0$ 替换为 $\mathrm{CVaR}(f_i(x, \omega); \eta) \le 0$，后者关于 x 是凸的

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这一页把整条技术路线收拢成一句话：用凸的 CVaR(f_i(x,ω); η) ≤ 0 替代非凸的 VaR(f_i(x,ω); η) ≤ 0，而 CVaR ≥ VaR 保证了这是保守替代。从凸分析看 CVaR 为什么凸：被极小化的函数 β + (1/(1−η))E(z−β)+ 中，(z−β)+ 是 (z, β) 的联合凸函数，期望保凸，而凸函数对部分变量 β 取下确界后关于剩下的变量 x 仍凸——这是"部分极小化保凸"的标准结论。VaR 则是分布函数的反函数（分位数），既不凸也不凹，所以没法直接进凸优化。Rockafellar 与 Uryasev 2000 年的论文正是靠这个极小化表述把尾部风险优化变成凸问题，如今已是随机规划与金融风险管理的标准工具。顺带一提，(z−β)+ 的形状与机器学习里的铰链损失（hinge loss）完全一致。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Slide 15 · Chebyshev 机会约束界（Chebyshev Chance Constraint Bound）</span>

<!-- p.15 -->

<span style="color:#2471a3;">**[theorem]**</span>

- 取 $\phi(u) = (u + 1)^{2+}$（即 $\max(u+1, 0)^2$）得到 Chebyshev 界（Chebyshev bound）：对任意 $\alpha_i > 0$，

```math
\mathrm{Prob}(f_i(x, \omega) > 0) \le \mathrm{E}\, (f_i(x, \omega)/\alpha_i + 1)^{2+}
```

- 凸近似约束

```math
\mathrm{E}\, \alpha_i (f_i(x, \omega)/\alpha_i + 1)^{2+} \le \alpha_i (1 - \eta)
```

可以写成

```math
\mathrm{E}\, (f_i(x, \omega) + \alpha_i)^{2+} / \alpha_i \le \alpha_i (1 - \eta)
```

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Slide 16 · 传统 Chebyshev 界（Traditional Chebyshev Bound）</span>

<!-- p.16 -->

<span style="color:#2471a3;">**[theorem]**</span>

- 去掉下标 $+$（即不再截断负部），得到更保守的约束

```math
\mathrm{E}\, \alpha_i (f_i(x, \omega)/\alpha_i + 1)^{2} \le \alpha_i (1 - \eta)
```

它可以写成

```math
2\, \mathrm{E}\, f_i(x, \omega) + (1/\alpha_i)\, \mathrm{E}\, f_i(x, \omega)^2 + \alpha_i \eta \le 0
```

- 对 α_i 极小化给出 $\alpha_i = \left( \mathrm{E}\, f_i(x, \omega)^2 / \eta \right)^{1/2}$；得到约束

```math
\mathrm{E}\, f_i(x, \omega) + \left( \eta\, \mathrm{E}\, f_i(x, \omega)^2 \right)^{1/2} \le 0
```

它只依赖于 $f_i$ 的一阶矩和二阶矩。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Slide 17 · 例子：随机线性约束的传统 Chebyshev 近似</span>

<!-- p.17 -->

<span style="color:#2471a3;">**[example]**</span>

- 设 $f_i(x) = a^T x - b$，其中 a 是随机的，$\mathrm{E}\, a = \bar a$，$\mathrm{E}\, aa^T = \Sigma$
- 机会约束的传统 Chebyshev 近似为

```math
\bar a^T x - b + \eta^{1/2} \left( x^T \Sigma x - 2b\, \bar a^T x + b^2 \right)^{1/2} \le 0
```

- 可以写成二阶锥约束（second-order cone constraint）

```math
\bar a^T x - b + \eta^{1/2}\, \| (z, y) \|_2 \le 0
```

其中

```math
z = \Sigma^{1/2} x - b\, \Sigma^{-1/2} \bar a, \qquad
y = b \left( 1 - \bar a^T \Sigma^{-1} \bar a \right)^{1/2}
```

- 可以解释为确定性等价（certainty-equivalent）约束，其中的范数项是"额外裕量"（extra margin）

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Slide 18 · Chernoff 机会约束界（Chernoff Chance Constraint Bound）</span>

<!-- p.18 -->

<span style="color:#2471a3;">**[theorem]**</span>

- 取 $\phi(u) = \exp u$ 得到 Chernoff 界（Chernoff bound）：对任意 $\alpha_i > 0$，

```math
\mathrm{Prob}(f_i(x, \omega) > 0) \le \mathrm{E}\, \exp(f_i(x, \omega)/\alpha_i)
```

- 凸近似约束

```math
\mathrm{E}\, \alpha_i \exp(f_i(x, \omega)/\alpha_i) \le \alpha_i (1 - \eta)
```

可以写成

```math
\log \mathrm{E}\, \exp(f_i(x, \omega)/\alpha_i) \le \log(1 - \eta)
```

（左端是 $f_i(x, \omega)$ 的累积量生成函数（cumulant generating function）在 $1/\alpha_i$ 处的取值。）

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 把三种界排在一起，能看到概率论（Bertsekas）里那条熟悉的递进链：Markov 不等式只用非负性与支撑信息，Chebyshev 不等式用到二阶矩，Chernoff 界通过指数函数用上全部矩——使用的矩信息越多，界越紧，对应的保守近似越不保守。log E exp(·) 是 f_i(x,ω) 的累积量生成函数（cumulant generating function），即矩母函数 M(s) = E exp(s z) 取对数：概率课上它用来由矩恢复分布，而在这里它更关键的性质是凸性——log-sum-exp 是凸函数（凸优化书第 3 章），期望保持凸性，所以左端关于 (x, α_i) 仍是凸的，约束可以进凸优化求解。你在 18.065 与机器学习中常见的 softmax、log-partition function，本质上都是同一个 log E exp 结构。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Slide 19 · 例子：随机线性约束</span>

<!-- p.19 -->

<span style="color:#2471a3;">**[example]**</span>

- （比如说）在最大化线性收益函数的同时，要求随机线性约束以概率 η 成立：

```math
\begin{aligned}
\text{maximize} \quad & c^T x \\
\text{subject to} \quad & \mathrm{Prob}(\max(Ax - b) \le 0) \ge \eta
\end{aligned}
```

变量为 $x \in \mathbb{R}^n$；$A \in \mathbb{R}^{m \times n}$，$b \in \mathbb{R}^m$ 是随机的（高斯）。

- Markov/CVaR 近似：

```math
\begin{aligned}
\text{maximize} \quad & c^T x \\
\text{subject to} \quad & \mathrm{E}\, (\max(Ax - b) + \alpha)^+ \le \alpha (1 - \eta)
\end{aligned}
```

变量为 $x \in \mathbb{R}^n$，$\alpha \in \mathbb{R}$。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Slide 20 · Chebyshev 近似与下界性质</span>

<!-- p.20 -->

<span style="color:#2471a3;">**[example]**</span>

- Chebyshev 近似：

```math
\begin{aligned}
\text{maximize} \quad & c^T x \\
\text{subject to} \quad & \mathrm{E}\, (\max(Ax - b) + \alpha)^{2+} / \alpha \le \alpha (1 - \eta)
\end{aligned}
```

变量为 $x \in \mathbb{R}^n$，$\alpha \in \mathbb{R}$。

- 这些近似问题的最优值是原问题最优值的下界（lower bounds）

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Slide 21 · 数值实验</span>

<!-- p.21 -->

<span style="color:#2471a3;">**[example]**</span>

- 实例：n = 5，m = 10，η = 0.9
- 用采样法求解各近似问题：N = 1000 个训练样本，再用 M = 10000 个样本验证
- 与确定性问题的解进行比较：

```math
\begin{aligned}
\text{maximize} \quad & c^T x \\
\text{subject to} \quad & \mathrm{E}\, A x \le \mathrm{E}\, b
\end{aligned}
```

- 在训练/验证数据上对 $\mathrm{Prob}(\max(Ax - b) \le 0)$ 的估计：

| 方法 | $c^T x$ | 训练集 | 验证集 |
|---|---|---|---|
| Markov | 3.60 | 0.97 | 0.96 |
| Chebyshev | 3.43 | 0.97 | 0.96 |
| 确定性（deterministic） | 7.98 | 0.04 | 0.03 |

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Slide 22 · Markov 近似解的 PDF 图</span>

<!-- p.22 -->

<span style="color:#2471a3;">**[example]**</span> 纯图示页：Markov 近似解对应的 $\max(Ax - b)$ 的概率密度函数（PDF）曲线，横轴范围约为 −25 至 15，密度峰值位于 0 左侧（即约束大概率以正裕量满足）。
