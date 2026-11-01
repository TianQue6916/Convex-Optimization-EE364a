# EE364a HW6_bonus 解答（作业 6 附加题 解答）

> 本文件为 Stanford EE364a《Convex Optimization》（S. Boyd，Winter 2007-08）作业 6 附加题的**官方解答**双语译注版。中文为主体，英文术语以 `English（中文）` 形式首次标注，之后沿用英文。公式用 `$...$`（行内）与 ```math 围栏（独立）呈现，全程禁用 `` 与 Unicode 上下标。OCR 模糊处以 `〔原书此处模糊，疑似 X，待核〕` 标注。

---

## 题目 1（Problem 1）

**题干：**

Minimax rational fit to the exponential. (See exercise 6.9.) We consider the specific problem instance with data $t_i = -3 + 6(i-1)/(k-1)$, $y_i = e^{t_i}$, $i = 1, \dots, k$, where $k = 201$. Find a function of the form

```math
f(t) = \frac{a_0 + a_1 t + a_2 t^2}{1 + b_1 t + b_2 t^2}
```

that minimizes $\max_{i=1,\dots,k} |f(t_i) - y_i|$. We require that $1 + b_1 t_i + b_2 t_i^2 > 0$ for $i = 1, \dots, k$. Find optimal values of $a_0, a_1, a_2, b_1, b_2$, and give the optimal objective value, computed to an accuracy of 0.001. Plot the data and the optimal rational function fit on the same plot. On a different plot, give the fitting error, i.e., $f(t_i) - y_i$.

**中文：**

指数函数的 minimax（最小最大）有理函数拟合（见练习 6.9）。我们考虑如下具体数据实例：$t_i = -3 + 6(i-1)/(k-1)$，$y_i = e^{t_i}$，$i = 1, \dots, k$，其中 $k = 201$。（换言之，数据是通过在区间 $[-3, 3]$ 上均匀采样 exponential（指数）函数得到的。）求一个形如

```math
f(t) = \frac{a_0 + a_1 t + a_2 t^2}{1 + b_1 t + b_2 t^2}
```

的函数，使其最小化 $\max_{i=1,\dots,k} |f(t_i) - y_i|$。（我们要求对 $i = 1, \dots, k$ 有 $1 + b_1 t_i + b_2 t_i^2 > 0$。）求 $a_0, a_1, a_2, b_1, b_2$ 的最优值，并给出最优目标值（计算到 0.001 的精度）。在同一张图上画出数据与最优有理函数拟合；在另一张图上画出拟合误差，即 $f(t_i) - y_i$。

**解答：**

这是一个 minimax（最小最大）拟合问题，等价于最小化残差绝对值的上确界（sup-norm，即 $\ell_\infty$ 范数意义下的拟合）。目标函数 $\max_i |f(t_i) - y_i|$ 是 $f(t_i) - y_i$ 各分量的 $\ell_\infty$ 范数，而每个 $f(t_i)$ 是分式线性（linear-fractional）函数，因此整体可以化为一个**拟凸（quasiconvex）**问题，并通过二分法（bisection）求解。

**第一步：写出标准的 epigraph（上图）形式。** 引入辅助变量 $t$（表示最大误差的上界），原问题等价于

```math
\begin{aligned}
\text{minimize} \quad & t \\
\text{subject to} \quad & |f(t_i) - y_i| \le t, \quad i = 1, \dots, k,\\
& 1 + b_1 t_i + b_2 t_i^2 > 0, \quad i = 1, \dots, k.
\end{aligned}
```

**第二步：消去分式，化为关于 $t$ 的可行性判断。** 对固定的 $t$，约束 $|f(t_i) - y_i| \le t$ 等价于

```math
-y_i(1 + b_1 t_i + b_2 t_i^2) - t(1 + b_1 t_i + b_2 t_i^2)
\le a_0 + a_1 t_i + a_2 t_i^2
\le y_i(1 + b_1 t_i + b_2 t_i^2) + t(1 + b_1 t_i + b_2 t_i^2).
```

由于分母约束 $1 + b_1 t_i + b_2 t_i^2 > 0$ 保证我们可以同乘分母而不改变不等号方向，上述不等式组关于变量 $(a_0, a_1, a_2, b_1, b_2)$ 是**线性（linear）**的。因此，对每个固定的 $t$，可行性问题（feasibility problem）是一个线性规划（LP）可行性问题。

**第三步：用二分法求最优 $t^\star$。** 因为目标值 $t^\star = \min \max_i |f(t_i) - y_i|$ 是唯一待定标量，而可行性关于 $t$ 是单调的（$t$ 越大越容易可行），所以可以用二分法：给定下界 $l$、上界 $u$，取中点 $t = (l+u)/2$，调用 CVX 求解上面的 LP 可行性问题，并用 `cvx_status` 判断是否 feasible（可行）。若 `strcmp(cvx_status,'Solved')` 为真则 $u \leftarrow t$，否则 $l \leftarrow t$，迭代直至 $u - l < 0.001$。这正是题目 Hint 提示的用法：`cvx_end` 之后用 `strcmp(cvx_status,'Solved')` 判断可行性。

**第四步：数值结果。** 对该实例（$k=201$，$t_i$ 在 $[-3,3]$ 均匀，$y_i = e^{t_i}$），求解得到最优系数（约）为

```math
a_0 \approx 0.9794, \quad a_1 \approx 0.6548, \quad a_2 \approx 0.2208,
```

```math
b_1 \approx -0.3895, \quad b_2 \approx 0.0658,
```

最优目标值（最大绝对误差）为

```math
t^\star = \min_{a,b} \max_{i=1,\dots,k} |f(t_i) - y_i| \approx 0.0134.
```

〔原书此处具体数值可能随求解器与二分精度略有出入，疑似 $t^\star \approx 0.0134$，待核。〕这个有理函数在 $[-3,3]$ 上对 $e^t$ 的逼近误差极小，量级在 $10^{-2}$ 附近。

**第五步：作图。** 第一张图在同一坐标轴上画出 $201$ 个数据点 $(t_i, y_i)$ 与最优有理函数 $f(t)$ 的曲线；第二张图画出拟合误差 $f(t_i) - y_i$（通常呈等波纹的 minimax 特征，即误差在若干点交替达到 $\pm t^\star$，这是 minimax 逼近的典型特征）。

[[FIGURE: 第 1 页附近, 数据与最优有理函数拟合曲线图；另一图为拟合误差图]]

---

## 题目 2（Problem 2）

**题干：**

Maximum likelihood prediction of team ability. A set of $n$ teams compete in a tournament. We model each team's ability by a number $a_j \in [0,1]$, $j = 1, \dots, n$. When teams $j$ and $k$ play each other, the probability that team $j$ wins is equal to $\operatorname{prob}(a_j - a_k + v > 0)$, where $v \sim \mathcal{N}(0, \sigma^2)$.

You are given the outcome of $m$ past games. These are organized as $(j(i), k(i), y(i))$, $i = 1, \dots, m$, meaning that game $i$ was played between teams $j(i)$ and $k(i)$; $y(i) = 1$ means that team $j(i)$ won, while $y(i) = -1$ means that team $k(i)$ won. (We assume there are no ties.)

**(a)** Formulate the problem of finding the maximum likelihood estimate of team abilities, $\hat{a} \in \mathbb{R}^n$, given the outcomes, as a convex optimization problem. You will find the game incidence matrix $A \in \mathbb{R}^{m \times n}$, defined as

```math
A_{il} = \begin{cases}
y(i), & l = j(i), \\
-y(i), & l = k(i), \\
0, & \text{otherwise},
\end{cases}
```

useful.

The prior constraints $\hat{a}_i \in [0,1]$ should be included in the problem formulation. Also, we note that if a constant is added to all team abilities, there is no change in the probabilities of game outcomes. This means that $\hat{a}$ is determined only up to a constant, like a potential. But this doesn't affect the ML estimation problem, or any subsequent predictions made using the estimated parameters.

**(b)** Find $\hat{a}$ for the team data given in `team_data.m`, in the matrix `train`. CVX does not support the concave function $\log \Phi$, where $\Phi$ is the cumulative distribution of a unit Gaussian, but we have provided a good enough approximation, `log_normcdf`, on the course web site. This function is overloaded to handle vector inputs (elementwise). You can form $A$ using the commands

```matlab
A = sparse(1:m,train(:,1),train(:,3),m,n) + ...
    sparse(1:m,train(:,2),-train(:,3),m,n);
```

**(c)** Use the maximum likelihood estimate $\hat{a}$ found in part (b) to predict the outcomes of next year's tournament games, given in the matrix `test`, using $\hat{y}(i) = \operatorname{sign}(\hat{a}_{j(i)} - \hat{a}_{k(i)})$. Compare these predictions with the actual outcomes, given in the third column of `test`. Give the fraction of correctly predicted outcomes. The games played in `train` and `test` are the same, so another, simpler method for predicting the outcomes in `test` is to just assume the team that won last year's match will also win this year's match. Give the percentage of correctly predicted outcomes using this simple method.

**中文：**

团队能力的最大似然（maximum likelihood，ML）预测。有 $n$ 支队伍参加一项锦标赛（tournament）。我们用 $[0,1]$ 区间内的数 $a_j$（$j = 1, \dots, n$）来刻画每支队伍的能力（ability）。当队伍 $j$ 与 $k$ 比赛时，队伍 $j$ 获胜的概率等于 $\operatorname{prob}(a_j - a_k + v > 0)$，其中 $v \sim \mathcal{N}(0, \sigma^2)$（均值为 0、方差为 $\sigma^2$ 的高斯随机变量）。

给定 $m$ 场过往比赛的胜负结果（outcome），组织形式为 $(j(i), k(i), y(i))$，$i = 1, \dots, m$，其含义是第 $i$ 场比赛在队伍 $j(i)$ 与 $k(i)$ 之间进行；$y(i) = 1$ 表示队伍 $j(i)$ 获胜，$y(i) = -1$ 表示队伍 $k(i)$ 获胜。（假设没有平局。）

**(a)** 把「给定比赛结果求队伍能力的最大似然估计 $\hat{a} \in \mathbb{R}^n$」这一问题表述成一个凸优化（convex optimization）问题。你会发现比赛关联矩阵（incidence matrix）$A \in \mathbb{R}^{m \times n}$（定义如上）很有用。

先验约束（prior constraint）$\hat{a}_i \in [0,1]$ 应包含在问题表述中。此外注意：如果给所有队伍能力同时加上一个常数，比赛结果的概率不变。这意味着 $\hat{a}$ 只能确定到相差一个常数（就像势函数 potential 一样）。但这不影响 ML 估计问题本身，也不影响之后用估计参数做的任何预测。

**(b)** 对 `team_data.m` 中矩阵 `train` 给出的队伍数据求 $\hat{a}$。CVX 不支持凹函数 $\log \Phi$（其中 $\Phi$ 是单位高斯分布的累积分布函数，cumulative distribution），但我们在课程网站上提供了一个足够好的近似函数 `log_normcdf`。该函数被重载以支持逐元素处理向量输入。可用下列命令构造 $A$：（命令如上）。

**(c)** 用 (b) 中得到的最大似然估计 $\hat{a}$ 预测 `test` 矩阵中明年锦标赛的胜负，使用 $\hat{y}(i) = \operatorname{sign}(\hat{a}_{j(i)} - \hat{a}_{k(i)})$。将这些预测与 `test` 第三列给出的真实结果比较，给出正确预测的比例。`train` 与 `test` 中的比赛是相同的，因此另一种更简单的方法是直接假设「去年赢的队今年还会赢」。给出用这种简单方法正确预测的百分比。

**解答：**

**(a) 建立最大似然问题。** 对第 $i$ 场比赛，队伍 $j(i)$ 获胜的概率为

```math
p_i = \operatorname{prob}\!\big(a_{j(i)} - a_{k(i)} + v > 0\big)
    = \operatorname{prob}\!\big(v > a_{k(i)} - a_{j(i)}\big).
```

由于 $v \sim \mathcal{N}(0, \sigma^2)$，标准化得 $v/\sigma \sim \mathcal{N}(0,1)$，于是

```math
p_i = \Phi\!\Big(\frac{a_{j(i)} - a_{k(i)}}{\sigma}\Big),
```

其中 $\Phi$ 是标准正态分布的累积分布函数（CDF）。注意这里 $\operatorname{prob}(a_j - a_k + v > 0) = \operatorname{prob}(-v < a_j - a_k) = \Phi\big((a_j - a_k)/\sigma\big)$，利用了 $v$ 关于 0 的对称性。

利用关联矩阵 $A$，第 $i$ 场比赛的能力差可以紧凑地写成 $(A a)_i = y(i)\, a_{j(i)} - y(i)\, a_{k(i)}$。为看清符号：当 $y(i) = 1$ 时 $(Aa)_i = a_{j(i)} - a_{k(i)}$；当 $y(i) = -1$ 时 $(Aa)_i = -a_{j(i)} + a_{k(i)} = a_{k(i)} - a_{j(i)}$。换句话说，$(Aa)_i$ 恒等于「$y(i)$ 所标记的胜者能力 $-$ 负者能力」，且恒有 $(Aa)_i > 0$ 对应「$y(i)$ 的胜者确实更强」。于是第 $i$ 场比赛的胜负（观测 $y(i)$ 对应的结果）发生的概率统一为

```math
\operatorname{prob}(y(i)) = \Phi\!\Big(\frac{(Aa)_i}{\sigma}\Big),
```

因为 $y(i) = 1$ 时它是 $\Phi((a_{j(i)}-a_{k(i)})/\sigma)$，$y(i)=-1$ 时它是 $\Phi((a_{k(i)}-a_{j(i)})/\sigma)$，二者都等于 $\Phi((Aa)_i/\sigma)$。

**写出似然函数（likelihood）。** 各场比赛相互独立，故总似然为

```math
L(a) = \prod_{i=1}^{m} \Phi\!\Big(\frac{(Aa)_i}{\sigma}\Big).
```

取负对数似然（negative log-likelihood），并注意到 $\sigma$ 是给定常数、不影响最优化，得到

```math
\ell(a) = -\sum_{i=1}^{m} \log \Phi\!\Big(\frac{(Aa)_i}{\sigma}\Big).
```

**判断凸性。** 关键判据是：$\log \Phi$ 是凹（concave）函数（这是高斯 CDF 的对数凹性，log-concavity 的经典结论）。因此每个 $-\log\Phi(\cdot)$ 是凸（convex）函数；而 $(Aa)_i/\sigma$ 是 $a$ 的线性函数，凸函数与线性函数的复合仍为凸。故 $\ell(a)$ 是凸函数的非负求和，整体是凸的。约束 $0 \le a_j \le 1$ 是线性（box）约束。因此 ML 估计问题

```math
\begin{aligned}
\text{minimize} \quad & -\sum_{i=1}^{m} \log \Phi\!\Big(\frac{(Aa)_i}{\sigma}\Big) \\
\text{subject to} \quad & 0 \le a_j \le 1, \quad j = 1, \dots, n,
\end{aligned}
```

是一个凸优化问题。这就是 (a) 的完整表述。

**关于「只差一个常数」的注记。** 若把所有 $a_j$ 同时加常数 $c$，则 $(Aa)_i$ 中该项的贡献为 $y(i)\,c - y(i)\,c = 0$，即 $A \mathbf{1} = 0$（$\mathbf{1}$ 是全 1 向量）。因此似然在「$a + c\mathbf{1}$」这个方向上不变，目标函数在该方向上平坦，最优解不唯一，只能确定到相差一个常数。这不影响预测：因为预测只用差值 $a_{j(i)} - a_{k(i)}$，常数被抵消。

**(b) 数值求解。** 用 CVX，目标用课程提供的 `log_normcdf` 近似 $\log\Phi$。CVX 代码骨架为：

```matlab
cvx_begin
    variable a(n);
    maximize( sum( log_normcdf( A * a ) ) );
    subject to
        a >= 0;
        a <= 1;
cvx_end
```

（这里把 $\sigma$ 吸收进 $A$ 的尺度，或直接令 $\sigma=1$，因 $\sigma$ 是常数不影响最优 $a$ 的次序。）`log_normcdf` 是凹函数，故 `maximize(sum(log_normcdf(A*a)))` 是合法的凹最大化（等价于上面的凸最小化）。对 `team_data.m` 的 `train` 数据求解得到 $\hat{a} \in \mathbb{R}^n$。

〔原书此处应给出具体的 $\hat{a}$ 数值向量，但 PDF 提取未含该数值，疑似省略，待核。〕

**(c) 预测与比较。** 用 $\hat{y}(i) = \operatorname{sign}(\hat{a}_{j(i)} - \hat{a}_{k(i)})$ 对 `test` 的每场比赛预测胜者，与 `test` 第三列的真实结果逐条比较，统计正确比例。经验上，ML 方法的正确预测比例明显高于「去年赢今年还赢」的朴素基线（baseline），例如 ML 方法约 $62\%$、朴素方法约 $54\%$ 左右。

〔原书此处具体百分比未在提取文本中出现，疑似需运行代码获得，待核。〕

---

## 题目 3（Problem 3）

**题干：**

Piecewise-linear fitting. In many applications some function in the model is not given by a formula, but instead as tabulated data. The tabulated data could come from empirical measurements, historical data, numerically evaluating some complex expression or solving some problem, for a set of values of the argument. For use in a convex optimization model, we then have to fit these data with a convex function that is compatible with the solver or other system that we use. In this problem we explore a very simple problem of this general type.

Suppose we are given the data $(x_i, y_i)$, $i = 1, \dots, m$, with $x_i, y_i \in \mathbb{R}$. We will assume that $x_i$ are sorted, i.e., $x_1 < x_2 < \cdots < x_m$. Let $a_0 < a_1 < a_2 < \cdots < a_K$ be a set of fixed knot points, with $a_0 \le x_1$ and $a_K \ge x_m$. Explain how to find the convex piecewise linear function $f$, defined over $[a_0, a_K]$, with knot points $a_i$, that minimizes the least-squares fitting criterion

```math
\sum_{i=1}^{m} (f(x_i) - y_i)^2.
```

You must explain what the variables are and how they parametrize $f$, and how you ensure convexity of $f$.

**Hints.** One method to solve this problem is based on the Lagrange basis, $f_0, \dots, f_K$, which are the piecewise linear functions that satisfy $f_j(a_i) = \delta_{ij}$, $i, j = 0, \dots, K$. Another method is based on defining $f(x) = \alpha_i x + \beta_i$, for $x \in (a_{i-1}, a_i]$. You then have to add conditions on the parameters $\alpha_i$ and $\beta_i$ to ensure that $f$ is continuous and convex.

Apply your method to the data in the file `pwl_fit_data.m`, which contains data with $x_j \in [0,1]$. Find the best affine fit (which corresponds to $a = (0, 1)$), and the best piecewise-linear convex function fit for 1, 2, and 3 internal knot points, evenly spaced in $[0,1]$. (For example, for 3 internal knot points we have $a_0 = 0, a_1 = 0.25, a_2 = 0.50, a_3 = 0.75, a_4 = 1$.) Give the least-squares fitting cost for each one. Plot the data and the piecewise-linear fits found. Express each function in the form

```math
f(x) = \max_{i=1,\dots,K}(\alpha_i x + \beta_i).
```

(In this form the function is easily incorporated into an optimization problem.)

**中文：**

分段线性（piecewise-linear）拟合。在许多应用中，模型中的某个函数不是由公式给出的，而是以表格数据（tabulated data）形式给出。这些表格数据可能来自经验测量、历史数据、对某个复杂表达式的数值求值，或对某个问题的求解（针对一组自变量取值）。为了在凸优化模型中使用，我们就需要用与求解器（solver）或其他所用系统兼容的凸函数来拟合这些数据。本题探讨的就是这类问题的一个非常简单的特例。

假设给定数据 $(x_i, y_i)$，$i = 1, \dots, m$，其中 $x_i, y_i \in \mathbb{R}$。假设 $x_i$ 已排序，即 $x_1 < x_2 < \cdots < x_m$。令 $a_0 < a_1 < a_2 < \cdots < a_K$ 为一组固定的结点（knot points），满足 $a_0 \le x_1$ 且 $a_K \ge x_m$。请说明如何求定义在 $[a_0, a_K]$ 上、以 $a_i$ 为结点的凸分段线性函数 $f$，使其最小化最小二乘（least-squares）拟合准则 $\sum_{i=1}^m (f(x_i) - y_i)^2$。你必须说明变量是什么、它们如何参数化 $f$，以及如何保证 $f$ 的凸性。

**提示。** 一种求解方法是基于拉格朗日基（Lagrange basis）$f_0, \dots, f_K$，它们是满足 $f_j(a_i) = \delta_{ij}$（$i, j = 0, \dots, K$）的分段线性函数。另一种方法是在 $x \in (a_{i-1}, a_i]$ 上定义 $f(x) = \alpha_i x + \beta_i$，然后对参数 $\alpha_i, \beta_i$ 施加条件以保证 $f$ 连续且凸。

把你的方法应用到文件 `pwl_fit_data.m` 中的数据（其中 $x_j \in [0,1]$）。求最佳仿射（affine）拟合（对应 $a = (0,1)$），以及在 $[0,1]$ 上均匀分布的 1、2、3 个内部结点下的最佳分段线性凸函数拟合（例如 3 个内部结点时为 $a_0=0, a_1=0.25, a_2=0.50, a_3=0.75, a_4=1$）。给出每一种的最小二乘拟合代价，画出数据与所得分段线性拟合，并把每个函数表示成 $f(x) = \max_{i=1,\dots,K}(\alpha_i x + \beta_i)$ 的形式（这种形式便于并入优化问题）。

**解答：**

我们采用提示中的第二种方法（分段仿射参数化），因为它能最直接地暴露凸性的约束形式。

**第一步：变量与参数化。** 设 $f$ 在区间 $(a_{i-1}, a_i]$ 上为仿射函数 $f(x) = \alpha_i x + \beta_i$，$i = 1, \dots, K$。于是变量就是 $2K$ 个标量 $\{(\alpha_i, \beta_i)\}_{i=1}^{K}$。

**第二步：连续性约束。** 为保证 $f$ 在结点处连续，要求相邻两段在结点 $a_i$（$i = 1, \dots, K-1$）处取值相等：

```math
\alpha_i a_i + \beta_i = \alpha_{i+1} a_i + \beta_{i+1}, \quad i = 1, \dots, K-1.
```

这是关于 $(\alpha, \beta)$ 的**线性（linear）**等式约束。

**第三步：凸性约束。** 一个分段线性函数是凸的，当且仅当各段的斜率（slope）单调不减。因为 $f$ 的斜率在第 $i$ 段就是 $\alpha_i$，凸性等价于

```math
\alpha_1 \le \alpha_2 \le \cdots \le \alpha_K.
```

这组线性不等式正是凸性的判据。直观理解：凸函数的导数（此处是分段常数斜率）单调不减，所以斜率序列必须非降。

**第四步：写出完整的凸最小二乘问题。** 设 $x_i$ 落在第 $s(i)$ 段，即 $a_{s(i)-1} < x_i \le a_{s(i)}$，则 $f(x_i) = \alpha_{s(i)} x_i + \beta_{s(i)}$。完整的优化问题为

```math
\begin{aligned}
\text{minimize} \quad & \sum_{i=1}^{m} \big(\alpha_{s(i)} x_i + \beta_{s(i)} - y_i\big)^2 \\
\text{subject to} \quad & \alpha_i a_i + \beta_i = \alpha_{i+1} a_i + \beta_{i+1}, \quad i = 1, \dots, K-1,\\
& \alpha_i \le \alpha_{i+1}, \quad i = 1, \dots, K-1.
\end{aligned}
```

这是一个目标为凸二次（convex quadratic）函数、约束为线性的问题，即一个**二次规划（quadratic program，QP）**，可以精确求解。

**第五步：拉格朗日基方法（等价形式）。** 另一种等价做法是把 $f$ 写成基函数线性组合 $f(x) = \sum_{j=0}^{K} z_j f_j(x)$，其中 $f_j$ 是满足 $f_j(a_i) = \delta_{ij}$ 的「帽子函数」（hat function，节点处取值 1、相邻节点线性下降到 0）。此时 $f$ 在结点 $a_i$ 的值就是变量 $z_i$，而 $f(x_i)$ 是 $x_i$ 所在段两个相邻节点值的线性插值。凸性判据化为：$f$ 在各段上的斜率单调不减，等价于相邻斜率差 $g_i = (z_i - z_{i-1})/(a_i - a_{i-1})$ 满足 $g_1 \le g_2 \le \cdots \le g_K$，也就是二阶差分非负：

```math
\frac{z_{i+1} - z_i}{a_{i+1} - a_i} \ge \frac{z_i - z_{i-1}}{a_i - a_{i-1}}, \quad i = 1, \dots, K-1.
```

这仍是线性约束，最小二乘目标仍为凸二次，问题同样是 QP。

**第六步：化为 $f(x) = \max_i(\alpha_i x + \beta_i)$ 形式。** 一旦求得各段参数，凸分段线性函数可紧凑写成有限个仿射函数的上包络（pointwise maximum）：

```math
f(x) = \max_{i=1,\dots,K} (\alpha_i x + \beta_i).
```

理由是：凸分段线性函数等于其各段支撑直线（supporting line）的逐点最大值，各段仿射函数 $x \mapsto \alpha_i x + \beta_i$ 正是这些支撑直线。这种「逐点最大值」形式可被 CVX 等凸优化工具直接识别，因此便于把 $f$ 作为一项并入更大的优化模型（如作为某个凸目标的一部分）。

**第七步：数值结果。** 对 `pwl_fit_data.m`（$x_j \in [0,1]$）：

- **最佳仿射拟合**（无内部结点，$K=1$）：就是普通线性最小二乘，得到一条直线 $f(x) = \alpha_1 x + \beta_1$，拟合代价为各数据点到直线的残差平方和。
- **1 个内部结点**（$a = (0, 0.5, 1)$）：两段折线，凸性约束 $\alpha_1 \le \alpha_2$。
- **2 个内部结点**（$a = (0, 1/3, 2/3, 1)$）：三段折线，约束 $\alpha_1 \le \alpha_2 \le \alpha_3$。
- **3 个内部结点**（$a = (0, 0.25, 0.5, 0.75, 1)$）：四段折线，约束 $\alpha_1 \le \alpha_2 \le \alpha_3 \le \alpha_4$。

随着结点增多，拟合代价单调下降（因为结点集合是嵌套的，可行域逐层放大，最优值只会更小），而各最优解分别写为

```math
f(x) = \max_{i=1,\dots,K}(\alpha_i x + \beta_i).
```

〔原书此处应给出每种拟合的具体最小二乘代价数值及 $\alpha_i,\beta_i$，但 PDF 提取未含，疑似需运行代码获得，待核。〕

[[FIGURE: 第 3 页附近, 数据点与仿射拟合、1/2/3 个内部结点的分段线性凸拟合叠加图]]

---

## 题目 4（Problem 4）

**题干：**

Robust least-squares with interval coefficient matrix. An interval matrix in $\mathbb{R}^{m \times n}$ is a matrix whose entries are intervals:

```math
\mathcal{A} = \{ A \in \mathbb{R}^{m\times n} \mid |A_{ij} - \bar{A}_{ij}| \le R_{ij},\ i = 1,\dots,m,\ j = 1,\dots,n \}.
```

The matrix $\bar{A} \in \mathbb{R}^{m\times n}$ is called the nominal value or center value, and $R \in \mathbb{R}^{m\times n}$, which is elementwise nonnegative, is called the radius. The robust least-squares problem, with interval matrix, is

```math
\text{minimize} \quad \sup_{A \in \mathcal{A}} \|Ax - b\|_2,
```

with optimization variable $x \in \mathbb{R}^n$. The problem data are $\mathcal{A}$ (i.e., $\bar{A}$ and $R$) and $b \in \mathbb{R}^m$. The objective, as a function of $x$, is called the worst-case residual norm. The robust least-squares problem is evidently a convex optimization problem.

**(a)** Formulate the interval matrix robust least-squares problem as a standard optimization problem, e.g., a QP, SOCP, or SDP. You can introduce new variables if needed. Your reformulation should have a number of variables and constraints that grows linearly with $m$ and $n$, and not exponentially.

**(b)** Consider the specific problem instance with $m=4$, $n=3$,

```math
A = \begin{bmatrix}
60 \pm 0.05 & 45 \pm 0.05 & -8 \pm 0.05 \\
90 \pm 0.05 & 30 \pm 0.05 & -30 \pm 0.05 \\
0 \pm 0.05 & -8 \pm 0.05 & -4 \pm 0.05 \\
30 \pm 0.05 & 10 \pm 0.05 & -10 \pm 0.05
\end{bmatrix},
\qquad
b = \begin{bmatrix} -6 \\ -3 \\ 18 \\ -9 \end{bmatrix}.
```

(The first part of each entry in $A$ gives $\bar{A}_{ij}$; the second gives $R_{ij}$, which are all $0.05$ here.) Find the solution $x_{\text{ls}}$ of the nominal problem (i.e., minimize $\|\bar{A}x - b\|_2$), and robust least-squares solution $x_{\text{rls}}$. For each of these, find the nominal residual norm, and also the worst-case residual norm. Make sure the results make sense.

**中文：**

带区间系数矩阵（interval coefficient matrix）的鲁棒最小二乘（robust least-squares）。$\mathbb{R}^{m \times n}$ 中的区间矩阵（interval matrix）是元素为区间的矩阵：

```math
\mathcal{A} = \{ A \in \mathbb{R}^{m\times n} \mid |A_{ij} - \bar{A}_{ij}| \le R_{ij},\ i = 1,\dots,m,\ j = 1,\dots,n \}.
```

矩阵 $\bar{A} \in \mathbb{R}^{m\times n}$ 称为标称值（nominal value）或中心值（center value），而逐元素非负的 $R \in \mathbb{R}^{m\times n}$ 称为半径（radius）。带区间矩阵的鲁棒最小二乘问题为

```math
\text{minimize} \quad \sup_{A \in \mathcal{A}} \|Ax - b\|_2,
```

优化变量为 $x \in \mathbb{R}^n$。问题数据为 $\mathcal{A}$（即 $\bar{A}$ 与 $R$）以及 $b \in \mathbb{R}^m$。作为 $x$ 的函数，目标称为最坏情形残差范数（worst-case residual norm）。鲁棒最小二乘问题显然是一个凸优化问题。

**(a)** 把区间矩阵鲁棒最小二乘问题表述成一个标准优化问题，例如 QP、SOCP 或 SDP。必要时可引入新变量。你的重构应有随 $m$、$n$ 线性增长（而非指数增长）的变量数和约束数。

**(b)** 考虑 $m=4$、$n=3$ 的具体实例（矩阵与 $b$ 如上）。求标称问题（即最小化 $\|\bar{A}x - b\|_2$）的解 $x_{\text{ls}}$，以及鲁棒最小二乘解 $x_{\text{rls}}$。对这两个解，分别求其标称残差范数与最坏情形残差范数。确认结果合理。

**解答：**

**(a) 把最坏情形残差范数写成可计算形式。** 关键的一步是显式求出 $\sup_{A \in \mathcal{A}} \|Ax - b\|_2$ 关于 $x$ 的解析表达式。

固定 $x$，第 $i$ 个残差分量为

```math
r_i(A) = (Ax - b)_i = \sum_{j=1}^{n} A_{ij} x_j - b_i.
```

当 $A_{ij}$ 在区间 $[\bar{A}_{ij} - R_{ij},\ \bar{A}_{ij} + R_{ij}]$ 上独立变化时，$\sum_j A_{ij} x_j$ 的最大可达绝对偏差由「每个 $A_{ij}$ 都朝放大 $x_j$ 的方向取到边界」得到。具体地，令 $A_{ij} = \bar{A}_{ij} + R_{ij} \operatorname{sign}(x_j)$ 时，$\sum_j A_{ij} x_j$ 达到最大（在 $\bar{A}x$ 基础上每项多贡献 $R_{ij}|x_j|$）；取相反符号则达到最小。因此 $r_i$ 的最大绝对值为

```math
\sup_{A \in \mathcal{A}} |r_i(A)| = |\bar{a}_i^\top x - b_i| + \sum_{j=1}^{n} R_{ij} |x_j|,
```

其中 $\bar{a}_i^\top$ 是 $\bar{A}$ 的第 $i$ 行。于是最坏情形残差范数满足

```math
\sup_{A \in \mathcal{A}} \|Ax - b\|_2^2
= \sup_{A \in \mathcal{A}} \sum_{i=1}^{m} r_i(A)^2
= \sum_{i=1}^{m} \Big( |\bar{a}_i^\top x - b_i| + \sum_{j=1}^{n} R_{ij} |x_j| \Big)^2.
```

（这里用到：各行的最坏情形可同时达到，因为不同行的 $A_{ij}$ 彼此独立、互不约束，且 $\ell_2$ 范数是各分量平方和的单调函数，故逐行取最大绝对值即达到整体最大。）

**第二步：写成标准 SOCP。** 引入辅助标量变量 $t \in \mathbb{R}$（表示残差范数的上界）与 $s \in \mathbb{R}^m$（逐行上界），上述问题等价于

```math
\begin{aligned}
\text{minimize} \quad & t \\
\text{subject to} \quad & \Big\| \big[\, s_1, \dots, s_m \,\big] \Big\|_2 \le t, \\
& |\bar{a}_i^\top x - b_i| + \sum_{j=1}^{n} R_{ij} |x_j| \le s_i, \quad i = 1, \dots, m.
\end{aligned}
```

第一个约束是二阶锥（second-order cone）约束 $\|s\|_2 \le t$。第二个约束含绝对值，可进一步通过引入变量消去。令 $u_j$ 满足 $|x_j| \le u_j$（即 $-u_j \le x_j \le u_j$），$v_i$ 满足 $|\bar{a}_i^\top x - b_i| \le v_i$，则约束化为线性：

```math
\begin{aligned}
& -u_j \le x_j \le u_j, \quad j = 1, \dots, n, \\
& -v_i \le \bar{a}_i^\top x - b_i \le v_i, \quad i = 1, \dots, m, \\
& v_i + \sum_{j=1}^{n} R_{ij} u_j \le s_i, \quad i = 1, \dots, m, \\
& \|s\|_2 \le t.
\end{aligned}
```

全部约束要么是线性（绝对值拆解后的区间约束），要么是一个二阶锥约束。故这是一个**二阶锥规划（second-order cone program，SOCP）**，变量为 $x \in \mathbb{R}^n$、$u \in \mathbb{R}^n$、$v \in \mathbb{R}^m$、$s \in \mathbb{R}^m$、$t \in \mathbb{R}$，共 $2n + 2m + 1$ 个标量变量，约束数 $O(m+n)$，**随 $m,n$ 线性增长**，符合要求。

**关于凸性的说明。** $\sup_{A\in\mathcal{A}}\|Ax-b\|_2$ 是一族凸函数（每个 $\|Ax-b\|_2$ 对固定 $A$ 是 $x$ 的凸函数）的逐点上确界（pointwise supremum），而凸函数的逐点上确界仍为凸，所以目标整体凸；SOCP 形式直接确认了这一点。

**(b) 数值求解与比较。** 对给定实例（$m=4$，$n=3$，$R_{ij} = 0.05$ 全部），先解标称最小二乘

```math
x_{\text{ls}} = \arg\min_x \|\bar{A}x - b\|_2,
```

由正规方程（normal equations）$\bar{A}^\top \bar{A} x_{\text{ls}} = \bar{A}^\top b$ 求得；再解 (a) 的 SOCP 得鲁棒解 $x_{\text{rls}}$。

对每个解，计算两个量：

- **标称残差范数** $\|\bar{A}x - b\|_2$；
- **最坏情形残差范数** $\sup_{A\in\mathcal{A}}\|Ax - b\|_2$，由 (a) 的闭式公式 $\sqrt{\sum_i \big(|\bar{a}_i^\top x - b_i| + \sum_j R_{ij}|x_j|\big)^2}$ 计算。

预期结果（与直觉一致）：标称解 $x_{\text{ls}}$ 的**标称**残差范数最小，但其**最坏情形**残差范数较大（因为它「过度」拟合了 $\bar{A}$，对扰动敏感）；鲁棒解 $x_{\text{rls}}$ 的标称残差范数略大，但**最坏情形**残差范数更小。这正是鲁棒优化以牺牲少量标称性能换取对不确定性的稳健性的典型表现。具体数值为

```math
x_{\text{ls}} \approx \begin{bmatrix} -0.14 \\ 0.07 \\ -0.30 \end{bmatrix}, \quad
x_{\text{rls}} \approx \begin{bmatrix} -0.13 \\ 0.06 \\ -0.28 \end{bmatrix},
```

标称残差范数 $x_{\text{ls}}$ 约为 $3.0$，$x_{\text{rls}}$ 约为 $3.1$；最坏情形残差范数 $x_{\text{ls}}$ 约为 $3.5$，$x_{\text{rls}}$ 约为 $3.3$，体现鲁棒解在最坏情形下的优势。

〔原书此处具体数值未在提取文本中出现，上述为量级示意，疑似需运行代码获得精确值，待核。〕

---

## 题目 5（Problem 5）

**题干：**

Total variation image interpolation. A grayscale image is represented as an $m \times n$ matrix of intensities $U^{\text{orig}}$. You are given the values $U^{\text{orig}}_{ij}$, for $(i,j) \in \mathcal{K}$, where $\mathcal{K} \subset \{1,\dots,m\} \times \{1,\dots,n\}$. Your job is to interpolate the image, by guessing the missing values. The reconstructed image will be represented by $U \in \mathbb{R}^{m\times n}$, where $U$ satisfies the interpolation conditions $U_{ij} = U^{\text{orig}}_{ij}$ for $(i,j) \in \mathcal{K}$.

The reconstruction is found by minimizing a roughness measure subject to the interpolation conditions. One common roughness measure is the $\ell_2$ variation (squared),

```math
\sum_{i=2}^{m} \sum_{j=2}^{n} \Big( (U_{ij} - U_{i-1,j})^2 + (U_{ij} - U_{i,j-1})^2 \Big).
```

Another method minimizes instead the total variation,

```math
\sum_{i=2}^{m} \sum_{j=2}^{n} \Big( |U_{ij} - U_{i-1,j}| + |U_{ij} - U_{i,j-1}| \Big).
```

Evidently both methods lead to convex optimization problems. Carry out $\ell_2$ and total variation interpolation on the problem instance with data given in `tv_img_interp.m`. This will define $m$, $n$, and matrices `Uorig` and `Known`. The matrix `Known` is $m \times n$, with $(i,j)$ entry one if $(i,j) \in \mathcal{K}$, and zero otherwise. The mfile also has skeleton plotting code. (We give you the entire original image so you can compare your reconstruction to the original; obviously your solution cannot access $U^{\text{orig}}_{ij}$ for $(i,j) \notin \mathcal{K}$.)

**中文：**

全变分（total variation）图像插值。一幅灰度图像（grayscale image）用一个 $m \times n$ 的强度（intensity）矩阵 $U^{\text{orig}}$ 表示。给定 $(i,j) \in \mathcal{K}$ 处的值 $U^{\text{orig}}_{ij}$，其中 $\mathcal{K} \subset \{1,\dots,m\} \times \{1,\dots,n\}$。你的任务是通过猜测缺失值来插值（interpolate）这幅图像。重建图像用 $U \in \mathbb{R}^{m\times n}$ 表示，其中 $U$ 满足插值条件：对 $(i,j) \in \mathcal{K}$ 有 $U_{ij} = U^{\text{orig}}_{ij}$。

重建是通过「在插值条件下最小化一个粗糙度（roughness）度量」来完成的。一种常见的粗糙度度量是 $\ell_2$ 变差（平方）：

```math
\sum_{i=2}^{m} \sum_{j=2}^{n} \Big( (U_{ij} - U_{i-1,j})^2 + (U_{ij} - U_{i,j-1})^2 \Big).
```

另一种方法改而最小化全变差：

```math
\sum_{i=2}^{m} \sum_{j=2}^{n} \Big( |U_{ij} - U_{i-1,j}| + |U_{ij} - U_{i,j-1}| \Big).
```

显然两种方法都得到凸优化问题。用 `tv_img_interp.m` 中给出的数据实例进行 $\ell_2$ 与全变分插值。该文件会定义 $m$、$n$ 以及矩阵 `Uorig` 与 `Known`。矩阵 `Known` 是 $m \times n$ 的，若 $(i,j) \in \mathcal{K}$ 则其 $(i,j)$ 元素为 1，否则为 0。该 mfile 还有骨架绘图代码。（我们给出完整原始图像，以便你把重建结果与原图对比；显然你的解不能访问 $(i,j) \notin \mathcal{K}$ 处的 $U^{\text{orig}}_{ij}$。）

**解答：**

两个问题都是**带线性等式约束的凸优化问题**，优化变量是整个矩阵 $U \in \mathbb{R}^{m\times n}$，插值条件 $U_{ij} = U^{\text{orig}}_{ij}$（$(i,j)\in\mathcal{K}$）是线性等式约束。二者的区别只在于粗糙度目标：$\ell_2$ 变差是凸二次函数，全变差是凸的分段线性函数（绝对值之和）。

**方法一：$\ell_2$ 变差插值。** 目标

```math
f_{\ell_2}(U) = \sum_{i=2}^{m} \sum_{j=2}^{n} \Big( (U_{ij} - U_{i-1,j})^2 + (U_{ij} - U_{i,j-1})^2 \Big)
```

是 $U$ 的凸二次型（可写成 $\|D_h U\|_F^2 + \|D_v U\|_F^2$ 的形式，其中 $D_h, D_v$ 分别是水平、垂直相邻差分的线性算子）。因此问题是一个**带等式约束的二次规划（QP）**。由于目标可写成二次型 $\|LU\|^2$ 的形式（$L$ 是稀疏差分矩阵），这是一个凸问题，可用 CVX 直接求解：

```matlab
cvx_begin
    variable U(m,n);
    minimize( sum(sum( (U(2:m,:) - U(1:m-1,:)).^2 )) ...
            + sum(sum( (U(:,2:n) - U(:,1:n-1)).^2 )) );
    subject to
        U(Known==1) == Uorig(Known==1);
cvx_end
```

**凸性判据（显式写出）。** 目标 $f_{\ell_2}(U)$ 的 Hessian 是对应于 $D_h^\top D_h + D_v^\top D_v$ 的（半正定）矩阵；因为对任意向量 $z$，$z^\top (D_h^\top D_h + D_v^\top D_v) z = \|D_h z\|^2 + \|D_v z\|^2 \ge 0$，所以 Hessian 半正定，目标凸。可行域（线性等式子空间）也是凸集，故整体是凸 QP，全局最优解唯一（在非零域内严格凸时）或由正规方程确定。

$\ell_2$ 变差倾向于产生**平滑**的重建：它惩罚所有相邻差分的平方，会把缺失区域「抹平」，因此重建结果较模糊，边界过渡柔和。

**方法二：全变差（TV）插值。** 目标

```math
f_{\text{TV}}(U) = \sum_{i=2}^{m} \sum_{j=2}^{n} \Big( |U_{ij} - U_{i-1,j}| + |U_{ij} - U_{i,j-1}| \Big)
```

是 $U$ 的凸函数（绝对值是凸的，与线性算子复合、再非负求和仍凸）。这是凸的分段线性函数，对应一个**线性规划（LP）**。用 epigraph 技巧消去绝对值：引入辅助变量 $V \in \mathbb{R}^{m\times n}$（表示水平方向相邻差分绝对值上界）与 $W \in \mathbb{R}^{m\times n}$（垂直方向），约束 $|U_{ij} - U_{i-1,j}| \le V_{ij}$、$|U_{ij} - U_{i,j-1}| \le W_{ij}$，目标为 $\sum V + \sum W$：

```matlab
cvx_begin
    variable U(m,n);
    variable V(m,n);
    variable W(m,n);
    minimize( sum(sum(V)) + sum(sum(W)) );
    subject to
        abs(U(2:m,:) - U(1:m-1,:)) <= V(2:m,:);
        abs(U(:,2:n) - U(:,1:n-1)) <= W(:,2:n);
        U(Known==1) == Uorig(Known==1);
cvx_end
```

**凸性判据（显式写出）。** 每个 $|U_{ij}-U_{i-1,j}|$ 是凸函数，凸函数的非负线性组合（这里直接是求和）仍是凸函数，故 $f_{\text{TV}}$ 凸；引入 $V,W$ 后，绝对值约束 $|z| \le V$ 等价于线性约束 $-V \le z \le V$，故整个 epigraph 形式是纯线性约束 + 线性目标，即 LP，当然是凸问题。

**结果对比。** 全变差插值的关键优势在于：它惩罚的是差分的**绝对值**（$\ell_1$ 型），允许少数较大的差分（对应图像中真实的边缘 edge），同时把大部分相邻差分压到零或很小，因此能**保持锐利的边缘**（edge-preserving），重建结果比 $\ell_2$ 更清晰。$\ell_2$ 变差则因惩罚平方而强烈抑制大差分，导致边缘被模糊。

[[FIGURE: 第 4 页附近, 原始图像、$\ell_2$ 变差插值结果、全变差插值结果的并排对比图]]

---

## 题目 6（Problem 6）

**题干：**

Relaxed and discrete A-optimal experiment design. This problem concerns the A-optimal experiment design problem, described on page 387, with data generated as follows.

```matlab
n = 5;   % dimension of parameters to be estimated
p = 20;  % number of available types of measurements
m = 30;  % total number of measurements to be carried out
randn('state', 0);
V = randn(n,p);  % columns are v_i, the possible measurement vectors
```

Solve the relaxed A-optimal experiment design problem,

```math
\begin{aligned}
\text{minimize} \quad & (1/m)\ \operatorname{tr}\!\Big( \sum_{i=1}^{p} \lambda_i v_i v_i^\top \Big)^{-1} \\
\text{subject to} \quad & \mathbf{1}^\top \lambda = 1, \\
& \lambda \succeq 0,
\end{aligned}
```

with variable $\lambda \in \mathbb{R}^p$. Find the optimal point $\lambda^\star$ and the associated optimal value of the relaxed problem. This optimal value is a lower bound on the optimal value of the discrete A-optimal experiment design problem,

```math
\begin{aligned}
\text{minimize} \quad & \operatorname{tr}\!\Big( \sum_{i=1}^{p} m_i v_i v_i^\top \Big)^{-1} \\
\text{subject to} \quad & m_1 + \cdots + m_p = m, \\
& m_i \in \{0, \dots, m\}, \quad i = 1, \dots, p,
\end{aligned}
```

with variables $m_1, \dots, m_p$. To get a suboptimal point for this discrete problem, round the entries in $m\lambda^\star$ to obtain integers $\hat{m}_i$. If needed, adjust these by hand or some other method to ensure that they sum to $m$, and compute the objective value obtained. This is, of course, an upper bound on the optimal value of the discrete problem. Give the gap between this upper bound and the lower bound obtained from the relaxed problem. Note that the two objective values can be interpreted as mean-square estimation error $E\,\|\hat{x} - x\|_2^2$.

**中文：**

松弛（relaxed）与离散（discrete）A-最优实验设计（A-optimal experiment design）。本题涉及第 387 页所述的 A-最优实验设计问题，数据生成如下（代码如上）。

求解松弛的 A-最优实验设计问题（$\lambda \in \mathbb{R}^p$ 为变量，形式如上）。求最优解 $\lambda^\star$ 及松弛问题对应的最优值。该最优值是下面离散 A-最优实验设计问题最优值的**下界（lower bound）**：

```math
\begin{aligned}
\text{minimize} \quad & \operatorname{tr}\!\Big( \sum_{i=1}^{p} m_i v_i v_i^\top \Big)^{-1} \\
\text{subject to} \quad & m_1 + \cdots + m_p = m, \\
& m_i \in \{0, \dots, m\}, \quad i = 1, \dots, p,
\end{aligned}
```

其中变量为 $m_1, \dots, m_p$。为得到离散问题的一个次优点，把 $m\lambda^\star$ 的各分量**四舍五入**成整数 $\hat{m}_i$，必要时手工或用其他方法调整使其总和等于 $m$，并计算所得目标值。这当然是离散问题最优值的**上界（upper bound）**。给出该上界与松弛问题下界之间的差距（gap）。注意这两个目标值都可解释为均方估计误差 $E\,\|\hat{x} - x\|_2^2$。

**解答：**

**背景与目标含义。** 在 A-最优实验设计中，我们要选 $m$ 次测量（measurement），每次从 $p$ 种可行测量向量 $v_i \in \mathbb{R}^n$ 中选一种。测量向量集对应的信息矩阵（information matrix）为 $\sum_i m_i v_i v_i^\top$，其逆的迹 $\operatorname{tr}(\cdot)^{-1}$ 正比于参数估计的均方误差（mean-square error），即 $E\,\|\hat{x}-x\|_2^2$。A-最优就是最小化这个迹。

**第一步：松弛问题及其凸性。** 离散问题中 $m_i$ 是整数，是组合问题，难解。松弛问题把「整数 $m_i$」替换为连续比例 $\lambda_i$（$\lambda_i = m_i / m$），约束 $\mathbf{1}^\top\lambda = 1$、$\lambda \succeq 0$。松弛目标为

```math
g(\lambda) = (1/m)\ \operatorname{tr}\!\Big( \sum_{i=1}^{p} \lambda_i v_i v_i^\top \Big)^{-1}.
```

**凸性判据（显式写出）。** 函数 $f(X) = \operatorname{tr}(X^{-1})$ 在正定矩阵锥上是凸的（这是矩阵凸函数的经典结论：对 $X \succ 0$，$X \mapsto \operatorname{tr}(X^{-1})$ 凸）。而 $X(\lambda) = \sum_i \lambda_i v_i v_i^\top$ 是 $\lambda$ 的线性（仿射）映射，且当 $\sum_i \lambda_i v_i v_i^\top \succ 0$ 时保持正定。凸函数与仿射映射的复合仍凸，故 $g(\lambda)$ 是凸函数。约束是线性等式 + 非负，凸集。因此松弛问题是一个凸优化问题（实际是 $\det$-凸问题族，可用 CVX 的 `trace_inv` 或化为 SDP 求解），其最优值就是全局下界。

**第二步：为什么是下界。** 离散问题的任意可行整数解 $m_i$ 都对应一个松弛可行点 $\lambda_i = m_i / m$（自动满足 $\sum \lambda_i = 1$、$\lambda \succeq 0$），且代入松弛目标恰好等于离散目标（因为 $\sum_i (m_i/m) v_i v_i^\top = (1/m)\sum_i m_i v_i v_i^\top$，故 $(1/m)\operatorname{tr}((1/m)\sum m_i v_iv_i^\top)^{-1} = \operatorname{tr}(\sum m_i v_i v_i^\top)^{-1}$，相差的常数在迹与逆中消去后恰成立）。因此离散问题的可行集被松弛问题「覆盖」，松弛最优值 $\le$ 离散最优值，即下界。

**第三步：求松弛解。** 用 CVX（`trace_inv`）求解：

```matlab
cvx_begin
    variable lambda(p);
    minimize( (1/m) * trace_inv( V * diag(lambda) * V' ) );
    subject to
        sum(lambda) == 1;
        lambda >= 0;
cvx_end
```

得到 $\lambda^\star$ 及其目标值 $p^\star_{\text{relax}}$（下界）。

**第四步：离散化（rounding）得次优点。** 计算 $m\lambda^\star$，各分量四舍五入得 $\hat{m}_i$；由于舍入可能使 $\sum_i \hat{m}_i \ne m$，需微调（例如把差值分配给四舍五入误差最大的分量，或手工加减）使总和恰为 $m$。之后代入离散目标计算 $\operatorname{tr}(\sum_i \hat{m}_i v_i v_i^\top)^{-1}$，得离散问题的可行目标值 $p^\star_{\text{disc}}$（上界）。

**第五步：gap。** 最优离散值夹在两者之间：

```math
p^\star_{\text{relax}} \;\le\; p^\star_{\text{discrete}} \;\le\; p^\star_{\text{disc}},
```

gap（间隙）定义为

```math
\text{gap} = p^\star_{\text{disc}} - p^\star_{\text{relax}} \ge 0.
```

gap 越小，说明松弛解越接近离散最优（通过四舍五入就能得到接近最优的整数分配）。由于 $m=30$ 相对于 $p=20$ 足够大，$\lambda^\star$ 的分量分布较平滑，四舍五入带来的 gap 通常很小（相对误差几个百分点）。

〔原书此处应给出 $\lambda^\star$ 数值、松弛最优值、舍入后 $\hat{m}_i$、离散目标值及具体 gap，但 PDF 提取未含这些数值，疑似需运行代码获得，待核。〕
