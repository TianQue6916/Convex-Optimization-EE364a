# EE364a 补充材料双语译注 —— Chance Constrained Optimization（机会约束优化）

> 来源：Stanford EE364a Supplementary Material；原文以 PDF 提取，公式有损处已用 `〔原书此处模糊，疑似 X，待核〕` 标注。

---

## Slide 1 — 总览

- Chance constrained optimization（机会约束优化）
  - chance constraints and percentile optimization（机会约束 与 百分位优化）
  - chance constraints for log-concave distributions（对数凹分布下的机会约束）
  - convex approximation of chance constraints（机会约束的凸近似）
- sources（参考来源）：Rockafellar & Uryasev, Nemirovsky & Shapiro

---

## Slide 2 — Chance constraints and percentile optimization（机会约束与百分位优化）

- "chance constraints"（机会约束；η 是 "confidence level"，置信水平）：
  ```math
  \mathrm{Prob}\big(f_i(x,\omega)\le 0\big)\ge \eta
  ```
  - convex in some cases (later)（在某些情况下是凸的，后文详述）
  - 一般关注的取值：η = 0.9, 0.95, 0.99
  - η = 0.999 无意义（除非你确定分布尾部可靠）
- percentile optimization（百分位优化；γ 是 "η-percentile"，η 分位数）：
  ```math
  \begin{aligned}
  &\text{minimize}\quad \gamma\\
  &\text{subject to}\quad \mathrm{Prob}\big(f_0(x,\omega)\le \gamma\big)\ge \eta
  \end{aligned}
  ```
  - convex or quasi-convex in some cases (later)（在某些情况下是凸的或拟凸的，后文详述）

---

## Slide 3 — Value-at-risk and conditional value-at-risk（在险价值 与 条件在险价值）

- value-at-risk（VaR，在险价值）：随机变量 z 在水平 η 下的定义为
  ```math
  \mathrm{VaR}(z;\eta)=\inf\{\gamma \mid \mathrm{Prob}(z\le \gamma)\ge \eta\}
  ```
  - 机会约束 $\mathrm{Prob}(f_i(x,\omega)\le 0)\ge\eta$ 等价于
    $\mathrm{VaR}(f_i(x,\omega);\eta)\le 0$
- conditional value-at-risk（CVaR，条件在险价值）：
  ```math
  \mathrm{CVaR}(z;\eta)=\inf_{\beta}\Big(\beta+\frac{1}{1-\eta}\,\mathbb{E}\,(z-\beta)_+\Big)
  ```
  - $\mathrm{CVaR}(z;\eta)\ge \mathrm{VaR}(z;\eta)$（more on this later，后文进一步说明）

---

## Slide 4 — CVaR interpretation（CVaR 的解读，针对连续分布）

- 在 CVaR 定义中，$\beta^\star=\mathrm{VaR}(z;\eta)$，由一阶条件：
  ```math
  0=\frac{d}{d\beta}\Big(\beta+\frac{1}{1-\eta}\,\mathbb{E}\,(z-\beta)_+\Big)
  =1-\frac{1}{1-\eta}\,\mathrm{Prob}(z\ge \beta)
  ```
  因此 $\mathrm{Prob}(z\ge \beta^\star)=1-\eta$
- conditional tail expectation（条件尾部期望，又称 expected shortfall，期望损失）：
  ```math
  \begin{aligned}
  \mathbb{E}(z\mid z\ge \beta^\star)
  &=\mathbb{E}\big(\beta^\star+(z-\beta^\star)\mid z\ge \beta^\star\big)\\
  &=\beta^\star+\frac{\mathbb{E}\big((z-\beta^\star)_+\big)}{\mathrm{Prob}(z\ge \beta^\star)}\\
  &=\mathrm{CVaR}(z;\eta)
  \end{aligned}
  ```

---

## Slide 5 — Chance constraints for log-concave distributions（对数凹分布下的机会约束）

- 假设
  - ω 具有 log-concave density（对数凹密度）$p(\omega)$
  - $C=\{(x,\omega)\mid f(x,\omega)\le 0\}$ 在 $(x,\omega)$ 上是 convex（凸）的
- 则
  ```math
  \mathrm{Prob}(f(x,\omega)\le 0)=\int \mathbf{1}\big((x,\omega)\in C\big)\,p(\omega)\,d\omega
  ```
  是 log-concave（对数凹）的，因为被积函数是对数凹的
- 因此机会约束 $\mathrm{Prob}(f(x,\omega)\le 0)\ge\eta$ 可写成一个 convex constraint（凸约束）：
  ```math
  \log \mathrm{Prob}(f(x,\omega)\le 0)\ge \log \eta
  ```

---

## Slide 6 — Linear inequality with normally distributed parameter（参数服从正态分布的线性不等式）

- 考虑 $a^Tx\le b$，其中 $a\sim \mathcal{N}(\bar a,\Sigma)$
- 则 $a^Tx-b\sim \mathcal{N}(\bar a^Tx-b,\; x^T\Sigma x)$
- 因而
  ```math
  \mathrm{Prob}(a^Tx\le b)=\Phi\!\left(\frac{b-\bar a^Tx}{\sqrt{x^T\Sigma x}}\right)
  ```
- 于是
  ```math
  \mathrm{Prob}(a^Tx\le b)\ge \eta \iff b-\bar a^Tx \ge \Phi^{-1}(\eta)\,\lVert\Sigma^{1/2}x\rVert_2
  ```
  当 η ≥ 0.5（即 $\Phi^{-1}(\eta)\ge 0$）时，这是一个 second-order cone constraint（二阶锥约束，SOC）

---

## Slide 7 — Portfolio optimization example（投资组合优化示例）

- $x\in\mathbb{R}^n$ 表示组合配置；$x_i$ 是资产 i 的（分数）头寸
- x 须满足 $1^Tx=1$，$x\in C$（convex portfolio constraint set，凸组合约束集）
- 组合收益率（例如以百分比计）为 $p^Tx$，其中 $p\sim\mathcal{N}(\bar p,\Sigma)$
  （更现实的模型是 p 服从 log-normal，对数正态）
- 目标：在损失概率受限的前提下最大化期望收益

---

## Slide 8 — 问题表述与凸化

- 问题为
  ```math
  \begin{aligned}
  &\text{maximize}\quad \mathbb{E}\,p^Tx\\
  &\text{subject to}\quad \mathrm{Prob}(p^Tx\le 0)\le \beta\\
  &\phantom{\text{subject to}}\quad 1^Tx=1,\; x\in C
  \end{aligned}
  ```
- 可写成一个 convex problem（凸问题，当 β ≤ 1/2 时）：
  ```math
  \begin{aligned}
  &\text{maximize}\quad \bar p^Tx\\
  &\text{subject to}\quad \bar p^Tx\ge \Phi^{-1}(1-\beta)\,\lVert\Sigma^{1/2}x\rVert_2\\
  &\phantom{\text{subject to}}\quad 1^Tx=1,\; x\in C
  \end{aligned}
  ```
  （当 C 为 polyhedron，多面体时，这是一个 SOCP，二阶锥规划）

---

## Slide 9 — Example（示例）

- n = 10 个资产，β = 0.05，$C=\{x\mid x\succeq -0.1\}$
- 对比
  - optimal portfolio（最优组合）
  - optimal portfolio w/o loss risk constraint（无损失风险约束的最优组合）
  - uniform portfolio（等权组合）$(1/n)\,\mathbf{1}$

| portfolio（组合） | $\mathbb{E}\,p^Tx$ | $\mathrm{Prob}(p^Tx\le 0)$ |
| --- | --- | --- |
| optimal（最优） | 7.51 | 5.0% |
| w/o loss constraint（无损失约束） | 10.66 | 20.3% |
| uniform（等权） | 3.41 | 18.9% |

---

## Slide 10 — return distributions（收益率分布图）

- 三幅图依次展示 optimal（最优）、w/o loss constraint（无损失约束）、uniform（等权）组合的收益率分布
- 横纵轴范围均为 −20 至 +30〔原书此处为坐标轴刻度数字，无文字，待核〕

---

## Slide 11 — Convex approximation of chance constraint bound（机会约束上界的凸近似）

- 假设 $f_i(x,\omega)$ 关于 x 是 convex（凸）的
- 设 $\varphi:\mathbb{R}\to\mathbb{R}$ 非负、凸、单调不减，且 $\varphi(0)=1$
- 对任意 $\alpha_i>0$，有 $\varphi(z/\alpha_i)\ge \mathbf{1}(z>0)$ 对所有 z 成立，故
  ```math
  \mathbb{E}\,\varphi\big(f_i(x,\omega)/\alpha_i\big)\ge \mathrm{Prob}\big(f_i(x,\omega)>0\big)
  ```
- 因此（凸）约束
  ```math
  \mathbb{E}\,\varphi\big(f_i(x,\omega)/\alpha_i\big)\le 1-\eta
  ```
  能保证机会约束 $\mathrm{Prob}(f_i(x,\omega)\le 0)\ge\eta$ 成立
- 这对任意 $\alpha_i>0$ 都成立；下面展示如何对 $\alpha_i$ 进行优化

---

## Slide 12 — 对 αᵢ 的优化

- 把约束改写为
  ```math
  \mathbb{E}\,\alpha_i\,\varphi\big(f_i(x,\omega)/\alpha_i\big)\le \alpha_i(1-\eta)
  ```
  - （perspective function，透视函数）$v\,\varphi(u/v)$ 在 $v>0$ 时关于 $(u,v)$ 是凸的，且关于 u 单调不减
  - 所以复合式 $\alpha_i\,\varphi(f_i(x,\omega)/\alpha_i)$ 在 $\alpha_i>0$ 时关于 $(x,\alpha_i)$ 是凸的
  - 因而上述约束关于 x 和 $\alpha_i$ 是凸的
  - 因此我们可以通过 convex optimization（凸优化）对 x 和 $\alpha_i>0$ 同时优化
- 这得到一个凸的随机优化问题，它是原 chance-constrained problem（机会约束问题）的一个 conservative approximation（保守近似）
- 下面看几个特殊情形

---

## Slide 13 — Markov chance constraint bound（马尔可夫机会约束上界）

- 取 $\varphi(u)=(u+1)_+$ 得到 Markov bound（马尔可夫界）：对任意 $\alpha_i>0$，
  ```math
  \mathrm{Prob}\big(f_i(x,\omega)>0\big)\le \mathbb{E}\,\big(f_i(x,\omega)/\alpha_i+1\big)_+
  ```
- 凸近似约束
  ```math
  \mathbb{E}\,\alpha_i\big(f_i(x,\omega)/\alpha_i+1\big)_+\le \alpha_i(1-\eta)
  ```
  可写为
  ```math
  \mathbb{E}\,\big(f_i(x,\omega)+\alpha_i\big)_+\le \alpha_i(1-\eta)
  ```
- 我们可以对 x 和 $\alpha_i\ge 0$ 进行优化

---

## Slide 14 — Interpretation via conditional value-at-risk（用条件在险价值解读）

- 把保守近似写成
  ```math
  \frac{\mathbb{E}\,\big(f_i(x,\omega)+\alpha_i\big)_+}{1-\eta}-\alpha_i\le 0
  ```
- 左端关于 $(x,\alpha_i)$ 是凸的，因此对 $\alpha_i$ 取最小
  ```math
  \inf_{\alpha_i>0}\left(\frac{\mathbb{E}\,\big(f_i(x,\omega)+\alpha_i\big)_+}{1-\eta}-\alpha_i\right)
  ```
  关于 x 是凸的
- 这正是 $\mathrm{CVaR}(f_i(x,\omega);\eta)$（可以证明 $\alpha_i>0$ 可放宽为 $\alpha_i\ge 0$）
- 因此凸近似把 $\mathrm{VaR}(f_i(x,\omega);\eta)\le 0$ 替换为 $\mathrm{CVaR}(f_i(x,\omega);\eta)\le 0$，后者关于 x 是凸的

---

## Slide 15 — Chebyshev chance constraint bound（切比雪夫机会约束上界）

- 取 $\varphi(u)=(u+1)^2_+$ 得到 Chebyshev bound（切比雪夫界）：对任意 $\alpha_i>0$，
  ```math
  \mathrm{Prob}\big(f_i(x,\omega)>0\big)\le \mathbb{E}\,\big(f_i(x,\omega)/\alpha_i+1\big)^2_+
  ```
- 凸近似约束
  ```math
  \mathbb{E}\,\alpha_i\big(f_i(x,\omega)/\alpha_i+1\big)^2_+\le \alpha_i(1-\eta)
  ```
  可写为
  ```math
  \mathbb{E}\,\big(f_i(x,\omega)+\alpha_i\big)^2_+\big/\alpha_i\le \alpha_i(1-\eta)
  ```

---

## Slide 16 — Traditional Chebyshev bound（传统切比雪夫界）

- 去掉下标 + 得到更保守的约束
  ```math
  \mathbb{E}\,\alpha_i\big(f_i(x,\omega)/\alpha_i+1\big)^2\le \alpha_i(1-\eta)
  ```
  可写为
  ```math
  2\,\mathbb{E}\,f_i(x,\omega)+\frac{1}{\alpha_i}\,\mathbb{E}\,f_i(x,\omega)^2+\alpha_i\,\eta\le 0
  ```
- 对 $\alpha_i$ 求最小得 $\alpha_i=$ 〔原文此处截断，后文缺失，待核〕
