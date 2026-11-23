# EE364a Homework 8 solutions（EE364a 作业 8 解答）

> 原文：`HW8 解答.pdf`（约 8 页），EE364a, Winter 2007-08, Prof. S. Boyd。前 4 页是教材习题解答（9.8, 10.1, 11.13），第 5 页起是 "Solutions to additional exercises"（标准形 LP 的 barrier method 三道实现题：centering step、带严格可行初始点的 LP solver、通用 LP solver）。附加题部分是编程作业的任务说明，原文未给代码。

## 9.8 Steepest descent method in $\ell_\infty$-norm（$\ell_\infty$ 范数下的最速下降法）

说明如何求 $\ell_\infty$ 范数下的 steepest descent direction（最速下降方向），并给出一个简单的解释。

**Solution**。normalized steepest descent direction（归一化最速下降方向）为

```math
\Delta x_{\mathrm{nsd}} = -\nabla f(x)\ \text{的逐分量符号，即}\ \Delta x_{\mathrm{nsd}} = -\mathbf{sign}(\nabla f(x)),
```

其中 $\mathbf{sign}$ 逐分量作用。解释：若关于 $x_k$ 的偏导数为正，我们就取一个减小 $x_k$ 的步；若偏导数为负，我们就取一个增大 $x_k$ 的步。

† 原文此处印作 "if it is positive, we take a step that increases $x_k$"，第二个 "positive" 应为 **negative**——否则与前半句重复且自相矛盾。逐分量取 $\mathrm{sign}$ 的意义就是「往梯度相反的方向走」：$\nabla f$ 的第 $k$ 个分量为正说明 $f$ 在该坐标方向上升，故应减小 $x_k$。

unnormalized steepest descent direction（未归一化最速下降方向）为

```math
\Delta x_{\mathrm{sd}} = -\|\nabla f(x)\|_1 \mathbf{sign}(\nabla f(x)).
```

（为什么 $\ell_\infty$ 范数下最速下降方向是「符号向量」（译者补）：归一化最速下降方向定义为 $\arg\min\{\nabla f(x)^Tv \mid \|v\|_\infty \le 1\}$。要极小化线性函数 $\nabla f(x)^Tv$ 在箱约束 $\|v\|_\infty \le 1$ 上，每个分量都取端点 $v_k = -\mathrm{sign}(\nabla f(x)_k)$，此时 $\nabla f^Tv = -\|\nabla f\|_1$。取「未归一化」版本时乘上 $-\|\nabla f\|_1$，于是只有 ±1 与 0 三种分量──这就是 $\ell_1$ 范数下的「坐标方向」的对偶图像。）

## 10.1 Nonsingularity of the KKT matrix（KKT 矩阵的非奇异性）

考虑 KKT matrix

```math
\begin{bmatrix} P & A^T \\ A & 0 \end{bmatrix},
```

其中 $P \in \mathbf{S}^n_+$、$A \in \mathbf{R}^{p \times n}$，且 $\mathrm{rank}\,A = p < n$。

**(a)** 证明下列每条陈述都与 KKT matrix 的非奇异性等价。

- $\mathcal{N}(P) \cap \mathcal{N}(A) = \{0\}$。
- $Ax = 0,\ x \ne 0 \ \Longrightarrow\ x^TPx > 0$。
- $F^TPF \succ 0$，其中 $F \in \mathbf{R}^{n \times (n-p)}$ 是满足 $\mathcal{R}(F) = \mathcal{N}(A)$ 的矩阵。
- $P + A^TQA \succ 0$ 对某个 $Q \succeq 0$ 成立。

**(b)** 证明：若 KKT matrix 非奇异，则它恰有 $n$ 个正特征值与 $p$ 个负特征值。

**Solution**。

**(a)** 条件 1 与条件 2。若 $x \in \mathcal{N}(A) \cap \mathcal{N}(P)$、$x \ne 0$，则 $Ax = 0$、$x \ne 0$，但 $x^TPx = 0$，与第二条陈述矛盾。反之，设第二条陈述不成立，即存在 $x$ 使 $Ax = 0$、$x \ne 0$，但 $x^TPx = 0$。由于 $P \succeq 0$，可推出 $Px = 0$，即 $x \in \mathcal{N}(P)$，与第一条陈述矛盾。

条件 2 与条件 3。若 $Ax = 0$、$x \ne 0$，则 $x$ 必形如 $x = Fz$，其中 $z \ne 0$（因为 $\mathrm{rank}(F) = n-p$）。于是 $x^TPx = z^TF^TPFz > 0$。

（反向：若 $F^TPF \succ 0$，则对任意 $x = Fz \ne 0$（$z \ne 0$）有 $x^TPx = z^TF^TPFz > 0$，即条件 2。）

条件 2 与条件 4。若第二条件成立，则

```math
x^T(P + A^TA)x = x^TPx + \|Ax\|_2^2 > 0
```

对所有非零 $x$ 成立，故最后一条陈述以 $Q = I$ 成立。

（注意这里要用 $P \succeq 0$ 保证 $x^TPx \ge 0$：当 $Ax \ne 0$ 时由 $\|Ax\|_2^2 > 0$ 得正；当 $Ax = 0$ 时由条件 2 得正。）

若最后一条陈述对某个 $Q \succeq 0$ 成立，则

```math
x^T(P + A^TQA)x = x^TPx + x^TA^TQAx > 0
```

对所有非零 $x$ 成立。因此若 $Ax = 0$ 且 $x \ne 0$，必有 $x^TPx > 0$。

现在证明这四条陈述与 KKT matrix 的非奇异性等价。先假设存在 $x$ 满足 $Ax = 0$、$Px = 0$ 且 $x \ne 0$。则

```math
\begin{bmatrix} P & A^T \\ A & 0 \end{bmatrix} \begin{bmatrix} x \\ 0 \end{bmatrix} = 0,
```

这说明 KKT matrix 奇异。

（注意这里两个零同时出现：$Ax = 0$ 来自 $x \in \mathcal{N}(A)$，$Px = 0$ 来自 $x \in \mathcal{N}(P)$；下式的第一块是 $Px + A^T\cdot0 = 0$，第二块是 $Ax + 0 = 0$。）

反之，设 KKT matrix 奇异，即存在不全为零的 $x, z$ 使

```math
\begin{bmatrix} P & A^T \\ A & 0 \end{bmatrix} \begin{bmatrix} x \\ z \end{bmatrix} = 0 .
```

这意味着 $Px + A^Tz = 0$ 且 $Ax = 0$。把第一个方程左乘 $x^T$，得 $x^TPx + x^TA^Tz = 0$。用 $Ax = 0$ 化为 $x^TPx = 0$，于是（由 $P \succeq 0$）$Px = 0$。除非 $x = 0$，这与第一条陈述矛盾。若 $x = 0$，则必有 $z \ne 0$，而此时 $A^Tz = 0$ 与 $\mathrm{rank}\,A = p$（即 $A^T$ 列满秩、$\mathcal{N}(A^T) = \{0\}$）矛盾。

（† 译注：原文此处印作 "This contradicts (a)"，语义上应为「与第一条条件（$\mathcal{N}(P)\cap\mathcal{N}(A) = \{0\}$）矛盾」。原文的 "(a)" 指向不明，属笔误。）

**(b)** 由 (a)，$P + A^TA \succ 0$。因此存在非奇异矩阵 $R \in \mathbf{R}^{n \times n}$ 使

```math
R^T(P + A^TA)R = I .
```

设 $AR = U\Sigma V^T$ 是 $AR$ 的 singular value decomposition（奇异值分解），其中 $U \in \mathbf{R}^{p \times p}$、$\Sigma = \mathbf{diag}(\sigma_1,\dots,\sigma_p) \in \mathbf{R}^{p \times p}$、$V_1 \in \mathbf{R}^{n \times p}$。取 $V_2 \in \mathbf{R}^{n \times (n-p)}$ 使

```math
V = \begin{bmatrix} V_1 & V_2 \end{bmatrix}
```

为正交矩阵，并定义

```math
S = \begin{bmatrix} \Sigma & 0 \end{bmatrix} \in \mathbf{R}^{p \times n} .
```

（这里 $U$ 取 $p \times p$、$V$ 取 $n \times n$，故 $AR = USV^T$，其中 $S$ 是「$p \times n$ 的奇异值矩阵」。这样配齐维度才能让下式成立。）

我们有 $AR = USV^T$，于是

```math
V^T R^T(P + A^TA)RV = V^TR^TPRV + S^TS = I .
```

因此 $V^TR^TPRV = I - S^TS$ 是对角矩阵。我们把这个矩阵记为 $\Lambda$：

```math
\Lambda = V^TR^TPRV = \mathbf{diag}(1 - \sigma_1^2, \dots, 1 - \sigma_p^2, 1,\dots,1).
```

（为什么在后 $n-p$ 个位置是 $1$：$S^TS = \mathbf{diag}(\sigma_1^2,\dots,\sigma_p^2,0,\dots,0)$，所以 $I - S^TS$ 的那些位置为 $1$。又因 $\Lambda \succeq 0$（它是 $R^TPR$ 的合同变换，而 $P \succeq 0$），可知 $\sigma_i^2 \le 1$。）

对 KKT matrix 作 congruence transformation（合同变换）得

```math
\begin{bmatrix} V^TR^T & 0 \\ 0 & U^T \end{bmatrix} \begin{bmatrix} P & A^T \\ A & 0 \end{bmatrix} \begin{bmatrix} RV & 0 \\ 0 & U \end{bmatrix} = \begin{bmatrix} \Lambda & S^T \\ S & 0 \end{bmatrix},
```

而 KKT matrix 的 inertia（惯性指数，即正/负/零特征值的个数）等于右边矩阵的 inertia。

对右边矩阵作排列，得到一个分块对角矩阵，含 $n$ 个对角块

```math
\begin{bmatrix} \lambda_i & \sigma_i \\ \sigma_i & 0 \end{bmatrix}, \quad i = 1,\dots,p, \qquad \lambda_i = 1, \quad i = p+1,\dots,n .
```

（合同变换加行列置换不改变 inertia，所以只需数这些小块。）

这些 $2 \times 2$ 块的特征值为

```math
\frac{\lambda_i \pm \sqrt{\lambda_i^2 + 4\sigma_i^2}}{2},
```

即一个特征值为正、一个为负（因为 $\lambda_i^2 + 4\sigma_i^2 > \lambda_i^2$ 当 $\sigma_i \ne 0$；若 $\sigma_i = 0$ 则特征值为 $\lambda_i$ 与 $0$——而 $\sigma_i = 0$ 与 $\lambda_i = 0$ 同时出现会使 KKT 矩阵奇异，故在非奇异的前提下不会发生）。

由此得出结论：共有 $p + (n-p) = n$ 个正特征值与 $p$ 个负特征值。

## 11.13 Self-concordance and negative entropy（self-concordance 与负熵）

**(a)** 证明负熵函数 $x\log x$（在 $\mathbf{R}_{++}$ 上）不是 self-concordant 的。

**(b)** 证明对任意 $t > 0$，$tx\log x - \log x$ 是 self-concordant 的（在 $\mathbf{R}_{++}$ 上）。

**Solution**。

**(a)** 先考虑 $f(x) = x\log x$，其导数为

```math
f'(x) = 1 + \log x, \qquad f''(x) = \frac{1}{x}, \qquad f'''(x) = -\frac{1}{x^2}.
```

于是

```math
\frac{|f'''(x)|}{(f''(x))^{3/2}} = \frac{1/x^2}{(1/x)^{3/2}} = \frac{1}{\sqrt{x}},
```

它在 $x \to 0^+$ 时上界无界。特别地，self-concordance 不等式 $|f'''(x)| \le 2(f''(x))^{3/2}$ 在 $x = 1/5$ 处不成立（$1/\sqrt{1/5} = \sqrt5 \approx 2.24 > 2$），故 $f$ 不是 self-concordant 的。

**(b)** 再考虑 $g(x) = tx\log x - \log x$，其导数为

```math
g'(x) = -\frac{1}{x} + t + t\log x, \qquad g''(x) = \frac{1}{x^2} + \frac{t}{x}, \qquad g'''(x) = -\frac{2}{x^3} - \frac{t}{x^2}.
```

因此

```math
\frac{|g'''(x)|}{(g''(x))^{3/2}} = \frac{2/x^3 + t/x^2}{(1/x^2 + t/x)^{3/2}} = \frac{2 + tx}{(1 + tx)^{3/2}} .
```

（化简：分子 $= \frac{2 + tx}{x^3}$，分母 $= \left(\frac{1+tx}{x^2}\right)^{3/2} = \frac{(1+tx)^{3/2}}{x^3}$，相除即得上式。）

定义

```math
h(a) = \frac{2 + a}{(1 + a)^{3/2}},
```

于是

```math
h(tx) = \frac{|g'''(x)|}{(g''(x))^{3/2}} .
```

我们有 $h(0) = 2$，并将证明对 $a > 0$ 有 $h'(a) < 0$，即 $h$ 在 $a > 0$ 上递减。这就证明 $h(a) \le h(0) = 2$，从而

```math
\frac{|g'''(x)|}{(g''(x))^{3/2}} \le 2 .
```

我们有

```math
\begin{aligned}
h'(a) &= \frac{(1+a)^{3/2} - (3/2)(1+a)^{1/2}(2+a)}{(1+a)^3} \\
&= \frac{(1+a)^{1/2}\big( (1+a) - (3/2)(2+a) \big)}{(1+a)^3} \\
&= \frac{-\big( 2 + a/2 \big)}{(1+a)^{5/2}} \\
&< 0,
\end{aligned}
```

对 $a > 0$ 成立，于是证毕。

（验算最后一步：$(1+a) - \frac{3}{2}(2+a) = 1 + a - 3 - 1.5a = -2 - 0.5a = -(2 + a/2)$，指数 $(1+a)^{1/2}/(1+a)^3 = 1/(1+a)^{5/2}$。）

# Solutions to additional exercises（附加题解答）

## Standard form LP barrier method（标准形 LP 的 barrier method）

下面三道练习中，你将为求解 standard form LP（标准形线性规划）

```math
\begin{aligned}
\text{minimize} \quad & c^T x \\
\text{subject to} \quad & Ax = b, \quad x \succeq 0,
\end{aligned}
```

实现一个 barrier method（障碍法/内点法）。变量为 $x \in \mathbf{R}^n$，其中 $A \in \mathbf{R}^{m \times n}$，$m < n$。整个练习中我们假设 $A$ 行满秩，且 sublevel sets（下水平集）$\{x \mid Ax = b,\ x \succeq 0,\ c^Tx \le \gamma\}$ 都有界。（若不成立，centering problem 下界无界。）

## 1. Centering step（中心化步）

对 centering problem

```math
\begin{aligned}
\text{minimize} \quad & c^T x - \sum_{i=1}^n \log x_i \\
\text{subject to} \quad & Ax = b,
\end{aligned}
```

实现 Newton's method：变量为 $x$，给定严格可行初始点 $x_0$。

你的代码应当接受 $A$、$b$、$c$、$x_0$，并返回 primal optimal point $x^\star$、dual optimal point $\nu^\star$ 以及执行的 Newton 步数。

用 block elimination method（块消去法）计算 Newton step。（你也可以通过 KKT system 计算 Newton step，并与块消去法算出的结果比较。两种步应当接近，但若某个 $x_i$ 非常小，你可能会得到关于 KKT matrix 条件数的警告。）

对不同的数据与初始点画出 $\lambda^2/2$ 随迭代 $k$ 的变化，以验证你的实现具有渐近 quadratic convergence。stopping criterion 可用 $\lambda^2/2 \le 10^{-6}$。对固定的问题实例，试验算法参数 $\alpha$ 与 $\beta$ 的取值，观察它们对所需总 Newton 步数的影响。检查你算出的 $x^\star$ 与 $\nu^\star$（近似）满足 KKT conditions。

为生成随机问题数据（即 $A$、$b$、$c$、$x_0$），我们推荐如下做法。先随机生成 $A$。（你可能想检查它是否行满秩。）然后生成随机正向量 $x_0$，取 $b = Ax_0$。（这保证 $x_0$ 严格可行。）参数 $c$ 可以随机选取。为确保 sublevel sets 有界，可以在 $A$ 上加一行全正元素。若希望能用同一份数据重复运行，务必为 uniform 与 normal 随机数生成器设好状态。

下面这些提示可能有用。

- 我们推荐用公式 $\lambda^2 = -\Delta x_{\mathrm{nt}}^T \nabla^2 f(x)\, \Delta x_{\mathrm{nt}}$ 计算 $\lambda^2$。你其实并不需要 $\lambda$ 本身；用 $\lambda^2$ 就够了。（这一点很重要，原因见下。）
- 你算出的 Newton step $\Delta x_{\mathrm{nt}}$ 可能带有小的数值误差。当 $x$ 接近最优时，$\lambda^2$ 的计算值（即 $\lambda^2 = -\Delta x_{\mathrm{nt}}^T\nabla^2 f(x)\Delta x_{\mathrm{nt}}$）实际上可能（略微）为负。若你开平方求 $\lambda$，会得到复数，而且永远回不来。此外，你的 line search 将永不退出。但这只在 $x$ 接近最优时发生。所以如果你以 $\lambda^2/2 \le 10^{-6}$ 为退出条件，一切都会正常，即使 $\lambda^2$ 的计算值为负。
- 对 line search，你必须先把步长 $t$ 乘以 $\beta$，直到 $x + t\Delta x_{\mathrm{nt}}$ 可行（即严格正）。若不这样做，你在计算 $f$ 时就会对负数取对数，而且永远回不来。

## 2. LP solver with strictly feasible starting point（带严格可行初始点的 LP solver）

用 (1) 中的 centering 代码，实现一个 barrier method 来求解 standard form LP

```math
\begin{aligned}
\text{minimize} \quad & c^T x \\
\text{subject to} \quad & Ax = b, \quad x \succeq 0,
\end{aligned}
```

变量为 $x \in \mathbf{R}^n$，给定严格可行初始点 $x_0$。你的 LP solver 应当以 $A$、$b$、$c$、$x_0$ 为参数，返回 $x^\star$。

当由 $n/t$ 度量的 duality gap（对偶间隙）小于 $10^{-3}$ 时，可以终止你的 barrier method。（若把容差取得小得多，你可能会遇到数值问题。）用若干问题实例把你的 LP solver 与 cvx 求出的解做对比。

(1) 中关于如何生成随机数据的评论在这里同样适用。

试验参数 $\mu$，观察它对各 centering step 所需 Newton 步数、以及求解整个问题所需总 Newton 步数的影响。

对 $n = 500$、$m = 100$ 的问题实例画出算法进展：纵轴为 duality gap（对数刻度），横轴为累计总 Newton 步数（线性刻度）。

你的算法应当返回一个 $2 \times k$ 矩阵 `history`（$k$ 是 centering step 总数），其第一行含每个 centering step 所需的 Newton 步数，第二行给出每个 centering step 结束时的 duality gap。为得到与书中图（例如第 572 页图 11.4）相似的图，你应使用如下代码：

```matlab
[xx, yy] = stairs(cumsum(history(1,:)),history(2,:));
semilogy(xx,yy);
```

## 3. LP solver（通用 LP solver）

用 (2) 中的代码，实现一个通用的 standard form LP solver：它以 $A$、$b$、$c$ 为参数，判定（严格）可行性与否，并在问题（严格）可行时返回一个 optimal point。

你需要实现一个 phase I method（第一阶段方法），它判定问题是否严格可行，若严格可行则找出一个严格可行点，从而可以喂给后面阶段。

（读法：phase I 的典型做法是引入人工变量 $s$、求解「极大化最小分量」或「极小化 $\mathbf{1}^Ts$」的辅助 LP；其最优值 $< 0$ 说明原问题严格可行。原文只给任务要求，不给实现——这是本课程最后一次编程作业。）
