> <span style="color:#7f8c8d;">Source: 作业解答/HW6 解答.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

## <span style="color:#c0392b;">EE364a Homework 6 Solutions（EE364a 作业 6 解答）</span>

<span style="color:#2471a3;">**[lecture]**</span> <span style="color:#7f8c8d;">EE364a, Winter 2007-08 · Prof. S. Boyd</span> —— 本文件是斯坦福 EE364a（凸优化）2007–08 学年冬季学期第 6 次作业的官方解答，包含书后习题 6.9 的解答与 5 道附加习题（additional exercises）的解答，主题依次为：拟凸的有理函数拟合、指数函数的极小极大有理拟合、团队能力的最大似然预测、分段线性拟合、区间矩阵的鲁棒最小二乘、全变差图像插值、A-最优实验设计。

<!-- p.1 -->

### <span style="color:#c0392b;">习题 6.9 极小极大有理函数拟合（Minimax rational function fitting）</span>

<span style="color:#2471a3;">**[exercise]**</span> 6.9 Minimax rational function fitting. Show that the following problem is quasiconvex:

```math
\begin{array}{ll}
\text{minimize} & \displaystyle\max_{i=1,\ldots,k} \left| \frac{p(t_i)}{q(t_i)} - y_i \right|
\end{array}
```

where

```math
\begin{aligned}
p(t) &= a_0 + a_1 t + a_2 t^2 + \cdots + a_m t^m, \\
q(t) &= 1 + b_1 t + \cdots + b_n t^n,
\end{aligned}
```

and the domain of the objective function is defined as

```math
D = \{ (a, b) \in \mathbb{R}^{m+1} \times \mathbb{R}^n \mid q(t) > 0,\ \alpha \le t \le \beta \}.
```

In this problem we fit a rational function p(t)/q(t) to given data, while constraining the denominator polynomial to be positive on the interval [α, β]. The optimization variables are the numerator and denominator coefficients ai, bi. The interpolation points ti ∈ [α, β], and desired function values yi, i = 1, . . . , k, are given.

> <span style="color:#2471a3;">**[solution]**</span> 我们来证明该目标函数是 quasiconvex（拟凸）的。它的定义域（domain）是凸集（convex set）。由于对 $i = 1, \ldots, k$ 均有 $q(t_i) > 0$，我们有
>
> ```math
> \max_{i=1,\ldots,k} \left| \frac{p(t_i)}{q(t_i)} - y_i \right| \le \gamma
> ```
>
> 当且仅当（if and only if）
>
> ```math
> \left| p(t_i) - y_i q(t_i) \right| \le \gamma q(t_i), \qquad i = 1, \ldots, k,
> ```
>
> 这在变量 $a$ 与 $b$ 下定义了一个凸集，因为左端是凸的，而右端是线性的。我们还可以进一步把这组不等式写成 $2k$ 个 linear inequalities（线性不等式）：
>
> ```math
> -\gamma q(t_i) \le p(t_i) - y_i q(t_i) \le \gamma q(t_i), \qquad i = 1, \ldots, k.
> ```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这段解答演示了判定拟凸（quasiconvex）最核心的手法：检查每一个下水平集（sublevel set）$\{x : f(x) \le \gamma\}$ 是否为凸集。回想凸函数的定义——所有下水平集都是凸集；拟凸函数恰好保留了这条几何性质，只放松了"函数值凸组合不增"的代数要求。也正因为下水平集是凸的，任何拟凸问题都能用二分法（bisection）求解：固定一个 $\gamma$，解一个凸的可行性判定问题（本题把不等式拆开后得到 $2k$ 个线性不等式，就是一个线性规划可行性问题），再根据"可行/不可行"收缩区间——这正是后面附加题 1 采用的算法。你在 6.006 里熟悉的二分搜索在这里原样复活：迭代次数只取决于精度，约为 $\log_2((u-l)/\varepsilon)$ 次，与变量维数无关，每步的代价是一次凸可行性求解。最后那句"广义线性分式函数"对应教材第 3.2.5 节的标准结论：线性分式函数（linear-fractional function）经逐点取最大值等运算后仍保持拟凸性，所以本题是"非凸却易解"的典型代表。

<!-- p.2 -->

<span style="color:#7f8c8d;">Solutions to additional exercises（附加习题解答）</span>

### <span style="color:#c0392b;">附加习题 1 指数函数的极小极大有理拟合（Minimax rational fit to the exponential）</span>

<span style="color:#2471a3;">**[exercise]**</span> 1. Minimax rational fit to the exponential. (See exercise 6.9.) We consider the specific problem instance with data

```math
t_i = -3 + 6(i-1)/(k-1), \qquad y_i = e^{t_i}, \qquad i = 1, \ldots, k,
```

where k = 201. (In other words, the data are obtained by uniformly sampling the exponential function over the interval [−3, 3].) Find a function of the form

```math
f(t) = \frac{a_0 + a_1 t + a_2 t^2}{1 + b_1 t + b_2 t^2}
```

that minimizes $\max_{i=1,\ldots,k} |f(t_i) - y_i|$. (We require that $1 + b_1 t_i + b_2 t_i^2 > 0$ for $i = 1, \ldots, k$.) Find optimal values of $a_0, a_1, a_2, b_1, b_2$, and give the optimal objective value, computed to an accuracy of 0.001. Plot the data and the optimal rational function fit on the same plot. On a different plot, give the fitting error, i.e., $f(t_i) - y_i$.

Hint. You can use `strcmp(cvx_status,'Solved')`, after `cvx_end`, to check if a feasibility problem is feasible.

> <span style="color:#2471a3;">**[solution]**</span> <span style="color:#00838f;">目标函数（因而整个问题）不是 convex（凸）的，但它是 quasiconvex（拟凸）的。</span>我们有 $\max_{i=1,\ldots,k} |f(t_i) - y_i| \le \gamma$ 当且仅当
>
> ```math
> \left| \frac{a_0 + a_1 t_i + a_2 t_i^2}{1 + b_1 t_i + b_2 t_i^2} - y_i \right| \le \gamma, \qquad i = 1, \ldots, k.
> ```
>
> 由于分母为正，这等价于
>
> ```math
> \left| a_0 + a_1 t_i + a_2 t_i^2 - y_i \left( 1 + b_1 t_i + b_2 t_i^2 \right) \right| \le \gamma \left( 1 + b_1 t_i + b_2 t_i^2 \right), \qquad i = 1, \ldots, k,
> ```
>
> 对固定的 $\gamma$，这是关于变量 $a$ 与 $b$ 的 $2k$ 个线性不等式。特别地，这说明目标函数是拟凸的。（事实上，它是一个 generalized linear fractional function（广义线性分式函数）。）
>
> 为求解该问题，可以使用 bisection method（二分法），每一步求解一个 LP feasibility problem（线性规划可行性问题）。每一步取定一个 $\gamma$ 值，求解可行性问题
>
> ```math
> \begin{array}{ll}
> \text{find} & a,\ b \\
> \text{subject to} & \left| a_0 + a_1 t_i + a_2 t_i^2 - y_i \left( 1 + b_1 t_i + b_2 t_i^2 \right) \right| \le \gamma \left( 1 + b_1 t_i + b_2 t_i^2 \right), \quad i = 1, \ldots, k,
> \end{array}
> ```
>
> 其中变量为 $a$ 与 $b$。（注意：只要 $\gamma > 0$，"分母为正"这一条件会自动得到满足。）这可以转化为如下 LP 可行性问题：
>
> ```math
> \begin{array}{ll}
> \text{find} & a,\ b \\
> \text{subject to} & a_0 + a_1 t_i + a_2 t_i^2 - y_i \left( 1 + b_1 t_i + b_2 t_i^2 \right) \le \gamma \left( 1 + b_1 t_i + b_2 t_i^2 \right), \quad i = 1, \ldots, k \\
> & a_0 + a_1 t_i + a_2 t_i^2 - y_i \left( 1 + b_1 t_i + b_2 t_i^2 \right) \ge -\gamma \left( 1 + b_1 t_i + b_2 t_i^2 \right), \quad i = 1, \ldots, k.
> \end{array}
> ```
>
> 下面的 Matlab 代码针对该具体问题实例求解：
>
> <!-- p.3 -->
>
> ```matlab
> k=201;
> t=(-3:6/(k-1):3)';
> y=exp(t);
> Tpowers=[ones(k,1) t t.^2];
> u=exp(3); l=0; % initial upper and lower bounds
> bisection_tol=1e-3; % bisection tolerance
> while u-l>= bisection_tol
> gamma=(l+u)/2;
> cvx_begin % solve the feasibility problem
> cvx_quiet(true);
> variable a(3);
> variable b(2);
> subject to
> abs(Tpowers*a-y.*(Tpowers*[1;b])) <= gamma*Tpowers*[1;b];
> cvx_end
> if strcmp(cvx_status,'Solved')
> u=gamma;
> a_opt=a;
> b_opt=b;
> objval_opt=gamma;
> else
> l=gamma;
> end
> end
> y_fit=Tpowers*a_opt./(Tpowers*[1;b_opt]);
> figure(1);
> plot(t,y,'b', t,y_fit,'r+');
> xlabel('t');
> ylabel('y');
> figure(2);
> plot(t, y_fit-y);
> xlabel('t');
> ylabel('err');
> ```
>
> 最优值为 $a_0 = 1.0099$、$a_1 = 0.6117$、$a_2 = 0.1134$、$b_1 = -0.4147$、$b_2 = 0.0485$，
>
> <!-- p.4 -->
>
> <span style="color:#7f8c8d;">（图，第 4 页：横轴 $x$ 范围 $-3$ 到 $3$，纵轴 $y$ 范围 $0$ 到 $25$。）</span>
>
> <span style="color:#7f8c8d;">图 1（Figure 1）Chebyshev fit with rational function. The line represents the data and the crosses the fitted points. —— 用有理函数作 Chebyshev（切比雪夫）拟合：曲线表示数据，叉号表示拟合点。</span>
>
> 而最优目标值为 0.0233。我们还得到了下面的图像。

### <span style="color:#c0392b;">附加习题 2 团队能力的最大似然预测（Maximum likelihood prediction of team ability）</span>

<span style="color:#2471a3;">**[exercise]**</span> 2. Maximum likelihood prediction of team ability. A set of n teams compete in a tournament. We model each team's ability by a number $a_j \in [0, 1]$, j = 1, . . . , n. When teams j and k play each other, the probability that team j wins is equal to prob($a_j - a_k + v > 0$), where $v \sim N(0, \sigma^2)$.

You are given the outcome of m past games. These are organized as $(j^{(i)}, k^{(i)}, y^{(i)})$, i = 1, . . . , m, meaning that game i was played between teams j(i) and k(i); $y^{(i)} = 1$ means that team j(i) won, while $y^{(i)} = -1$ means that team k(i) won. (We assume there are no ties.)

(a) Formulate the problem of finding the maximum likelihood estimate of team abilities, $\hat{a} \in \mathbb{R}^n$, given the outcomes, as a convex optimization problem. You will find the game incidence matrix $A \in \mathbb{R}^{m \times n}$, defined as

```math
A_{il} = \begin{cases}
y^{(i)} & l = j(i) \\
-y^{(i)} & l = k(i) \\
0 & \text{otherwise},
\end{cases}
```

useful. The prior constraints $\hat{a}_i \in [0, 1]$ should be included in the problem formulation. Also, we note that if a constant is added to all team abilities, there is no change in

<!-- p.5 -->

<span style="color:#7f8c8d;">（图，第 5 页：横轴 $x$ 范围 $-3$ 到 $3$，纵轴 Error 范围约 $-0.025$ 到 $0.025$。）</span>

<span style="color:#7f8c8d;">图 2（Figure 2）Fitting error for Chebyshev fit of exponential with rational function. —— 用有理函数对指数函数作 Chebyshev 拟合的拟合误差（属附加习题 1）。</span>

the probabilities of game outcomes. This means that $\hat{a}$ is determined only up to a constant, like a potential. But this doesn't affect the ML estimation problem, or any subsequent predictions made using the estimated parameters.

(b) Find $\hat{a}$ for the team data given in team_data.m, in the matrix train. (This matrix gives the outcomes for a tournament in which each team plays each other team once.) CVX does not support the concave function log Φ, where Φ is the cumulative distribution of a unit Gaussian, but we have provided a good enough approximation, log_normcdf, on the course web site. This function is overloaded to handle vector inputs (elementwise). You can form A using the commands

```matlab
A = sparse(1:m,train(:,1),train(:,3),m,n) + ...
sparse(1:m,train(:,2),-train(:,3),m,n);
```

(c) Use the maximum likelihood estimate $\hat{a}$ found in part (b) to predict the outcomes of next year's tournament games, given in the matrix test, using $\hat{y}^{(i)} = \mathrm{sign}(\hat{a}_{j(i)} - \hat{a}_{k(i)})$. Compare these predictions with the actual outcomes, given in the third column of test. Given the fraction of correctly predicted outcomes. The games played in train and test are the same, so another, simpler method for predicting the outcomes in test it to just assume the team that won last year's match will also win this year's match. Give the percentage of correctly predicted outcomes using this simple method.

<!-- p.6 -->

> <span style="color:#2471a3;">**[solution]**</span> **(a)** 给定 $a$ 时比赛结果 $y$ 的 likelihood（似然）为
>
> ```math
> p(y|a) = \prod_{i=1,\ldots,n} \Phi\left( \frac{1}{\sigma}\, y^{(i)} \left( a_{j(i)} - a_{k(i)} \right) \right),
> ```
>
> 其中 $\Phi$ 是标准正态分布（standard normal）的 cumulative distribution（累积分布函数）。因此 log-likelihood（对数似然函数）为
>
> ```math
> l(a) = \log p(y|a) = \sum_i \log \Phi\left( (1/\sigma)(Aa)_i \right).
> ```
>
> 这是一个 concave（凹）函数。maximum likelihood estimate（最大似然估计）$\hat{a}$ 是下面问题的任意一个解：
>
> ```math
> \begin{array}{ll}
> \text{maximize} & l(a) \\
> \text{subject to} & 0 \preceq a \preceq 1.
> \end{array}
> ```
>
> 这是一个 convex optimization problem（凸优化问题）：被最大化的目标是凹的，且约束是 $2n$ 个线性不等式。
>
> **(b)** 下面的代码求解该问题：
>
> ```matlab
> % Form adjacency matrix
> A1 = sparse(1:m,train(:,1),train(:,3),m,n);
> A2 = sparse(1:m,train(:,2),-train(:,3),m,n);
> A = A1+A2;
> % Estimate abilities
> cvx_begin
> variable a_hat(n)
> minimize(-sum(log_normcdf(A*a_hat/sigma)))
> subject to
> a_hat >= 0
> a_hat <= 1
> cvx_end
> ```
>
> 用这段代码我们得到 $\hat{a} = (1.0,\ 0.0,\ 0.68,\ 0.37,\ 0.79,\ 0.58,\ 0.38,\ 0.09,\ 0.67,\ 0.58)$。
>
> **(c)** 下面的代码用于预测 test 集中的比赛结果：
>
> ```matlab
> % Estimate errors in test set
> res = sign(a_hat(test(:,1))-a_hat(test(:,2)));
> Pml = 1-length(find(res-test(:,3)))/m_test
> Ply = 1-length(find(train(:,3)-test(:,3)))/m_test
> ```
>
> 最大似然估计对 test 中比赛的预测正确率为 86.7%。另一方面，test 中 75.6% 的比赛与 train 中比赛的胜负结果相同。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这道题展示了"概率模型 → 凸优化"的完整流水线。关键一步是：标准正态分布的累积分布函数 $\Phi$ 是对数凹（log-concave）的，于是 $\log\Phi$ 是凹函数，对数似然作为一串凹项之和仍是凹的——"最大化似然"就自动变成了凸优化问题。你在 Bertsekas《概率论》里学的最大似然估计（MLE）在这里露出了算法真面目：只要参数化得当（本题中胜负概率只通过能力差 $a_j - a_k$ 进入模型），MLE 就可解、可算；这个"能力差决定胜率"的模型正是 Logistic 回归的近亲（Bradley–Terry 模型）。两点提醒：其一，原文似然函数的乘积上下标写作 $\prod_{i=1,\ldots,n}$，但 $n$ 是队伍数、$m$ 才是比赛场数，按题意应为 $m$，阅读时请自行更正；其二，"给所有能力值加同一常数不改变任何胜率"说明 $\hat{a}$ 只能确定到一个常数（类似电势），而预测只用差值的符号，所以这不影响估计问题本身，也不影响任何下游预测。

<!-- p.7 -->

### <span style="color:#c0392b;">附加习题 3 分段线性拟合（Piecewise-linear fitting）</span>

<span style="color:#2471a3;">**[exercise]**</span> 3. Piecewise-linear fitting. In many applications some function in the model is not given by a formula, but instead as tabulated data. The tabulated data could come from empirical measurements, historical data, numerically evaluating some complex expression or solving some problem, for a set of values of the argument. For use in a convex optimization model, we then have to fit these data with a convex function that is compatible with the solver or other system that we use. In this problem we explore a very simple problem of this general type.

Suppose we are given the data $(x_i, y_i)$, i = 1, . . . , m, with $x_i, y_i \in \mathbb{R}$. We will assume that $x_i$ are sorted, i.e., $x_1 < x_2 < \cdots < x_m$. Let $a_0 < a_1 < a_2 < \cdots < a_K$ be a set of fixed knot points, with $a_0 \le x_1$ and $a_K \ge x_m$. Explain how to find the convex piecewise linear function f, defined over $[a_0, a_K]$, with knot points $a_i$, that minimizes the least-squares fitting criterion

```math
\sum_{i=1}^{m} \left( f(x_i) - y_i \right)^2.
```

You must explain what the variables are and how they parametrize f, and how you ensure convexity of f.

Hints. One method to solve this problem is based on the Lagrange basis, $f_0, \ldots, f_K$, which are the piecewise linear functions that satisfy

```math
f_j(a_i) = \delta_{ij}, \qquad i, j = 0, \ldots, K.
```

Another method is based on defining $f(x) = \alpha_i x + \beta_i$, for $x \in (a_{i-1}, a_i]$. You then have to add conditions on the parameters $\alpha_i$ and $\beta_i$ to ensure that f is continuous and convex.

Apply your method to the data in the file pwl_fit_data.m, which contains data with $x_j \in [0, 1]$. Find the best affine fit (which corresponds to a = (0, 1)), and the best piecewise-linear convex function fit for 1, 2, and 3 internal knot points, evenly spaced in [0, 1]. (For example, for 3 internal knot points we have $a_0 = 0$, $a_1 = 0.25$, $a_2 = 0.50$, $a_3 = 0.75$, $a_4 = 1$.) Give the least-squares fitting cost for each one. Plot the data and the piecewise-linear fits found. Express each function in the form

```math
f(x) = \max_{i=1\ldots,K} \left( \alpha_i x + \beta_i \right).
```

(In this form the function is easily incorporated into an optimization problem.)

> <span style="color:#2471a3;">**[solution]**</span> 按照提示，我们使用 Lagrange 基函数（Lagrange basis functions）$f_0, \ldots, f_K$。它们可以表示为
>
> ```math
> f_0(x) = \left( \frac{a_1 - x}{a_1 - a_0} \right)_{+},
> ```
>
> ```math
> f_i(x) = \left( \min \left\{ \frac{x - a_{i-1}}{a_i - a_{i-1}},\ \frac{a_{i+1} - x}{a_i - a_{i+1}} \right\} \right)_{+}, \qquad i = 1, \ldots, K-1,
> ```
>
> <!-- p.8 -->
>
> 以及
>
> ```math
> f_K(x) = \left( \frac{x - a_{K-1}}{a_K - a_{K-1}} \right)_{+}.
> ```
>
> 函数 $f$ 可以参数化为
>
> ```math
> f(x) = \sum_{i=0}^{K} z_i f_i(x),
> ```
>
> 其中 $z_i = f(a_i)$，$i = 0, \ldots, K$。我们用 $z = (z_0, \ldots, z_K)$ 来参数化 $f$。最小二乘拟合准则（least-squares fitting criterion）于是为
>
> ```math
> J = \sum_{i=1}^{m} \left( f(x_i) - y_i \right)^2 = \left\| F z - y \right\|_2^2,
> ```
>
> 其中 $F \in \mathbb{R}^{m \times (K+1)}$ 是矩阵
>
> ```math
> F_{ij} = f_j(x_i), \qquad i = 1, \ldots, m, \quad j = 0, \ldots, K.
> ```
>
> （这里我们把 $F$ 的列从 0 编号到 $K$。）我们必须加上"$f$ 是凸的"这一约束。<span style="color:#00838f;">这与"各段斜率非降（nondecreasing）"是同一回事，即</span>
>
> ```math
> \frac{z_{i+1} - z_i}{a_{i+1} - a_i} \ge \frac{z_i - z_{i-1}}{a_i - a_{i-1}}, \qquad i = 1, \ldots, K-1.
> ```
>
> 这是关于 $z$ 的一组线性不等式。因此，最优的凸 PWL（piecewise-linear，分段线性）拟合可以通过求解如下 QP（quadratic program，二次规划）得到：
>
> ```math
> \begin{array}{ll}
> \text{minimize} & \left\| F z - y \right\|_2^2 \\
> \text{subject to} & \dfrac{z_{i+1} - z_i}{a_{i+1} - a_i} \ge \dfrac{z_i - z_{i-1}}{a_i - a_{i-1}}, \qquad i = 1, \ldots, K-1.
> \end{array}
> ```
>
> 下面的代码对 pwl_fit_data 中的数据求解该问题：
>
> ```matlab
> cvx_quiet('true')
> figure
> plot(x,y,'k:','linewidth',2)
> hold on
> % Single line
> p = [x ones(100,1)]\y;
> alpha = p(1)
> beta = p(2)
> plot(x,alpha*x+beta,'b','linewidth',2)
> mse = norm(alpha*x+beta-y)^2
> ```
>
> <!-- p.9 -->
>
> ```matlab
> for K = 2:4
> % Generate Lagrange basis
> a = (0:(1/K):1)';
> F = max((a(2)-x)/(a(2)-a(1)),0);
> for k = 2:K
> a_1 = a(k-1);
> a_2 = a(k);
> a_3 = a(k+1);
> f = max(0,min((x-a_1)/(a_2-a_1),(a_3-x)/(a_3-a_2)));
> F = [F f];
> end
> f = max(0,(x-a(K))/(a(K+1)-a(K)));
> F = [F f];
> % Solve problem
> cvx_begin
> variable z(K+1)
> minimize(norm(F*z-y))
> subject to
> (z(3:end)-z(2:end-1))./(a(3:end)-a(2:end-1)) >=...
> (z(2:end-1)-z(1:end-2))./(a(2:end-1)-a(1:end-2))
> cvx_end
> % Calculate alpha and beta
> alpha = (z(2:end)-z(1:end-1))./(a(2:end)-a(1:end-1))
> beta = z(2:end)-alpha(1:end).*a(2:end)
> % Plot solution
> y2 = F*z;
> mse = norm(y2-y)^2
> if K==2
> plot(x,y2,'r','linewidth',2)
> elseif K==3
> plot(x,y2,'g','linewidth',2)
> else
> plot(x,y2,'m','linewidth',2)
> end
> end
> xlabel('x')
> ylabel('y')
> ```
>
> <!-- p.10 -->
>
> <span style="color:#7f8c8d;">（图，第 10 页：横轴 $x$ 范围 $0$ 到 $1$，纵轴 $y$ 范围 $-1$ 到 $2$。）</span>
>
> <span style="color:#7f8c8d;">图 3（Figure 3）Piecewise-linear approximations for K = 1, 2, 3, 4 —— K = 1, 2, 3, 4 时的分段线性近似。</span>
>
> 这段代码生成了图 3。可以看出，近似随 $K$ 增大而改善。下表给出该近似的结果：
>
> | $K$ | $\alpha_1, \ldots, \alpha_K$ | $\beta_1, \ldots, \beta_K$ | $J$ |
> |---|---|---|---|
> | 1 | 1.91 | −0.87 | 12.73 |
> | 2 | −0.27, 4.09 | −0.33, −2.51 | 2.62 |
> | 3 | −1.80, 2.67, 4.25 | −0.10, −1.59, −2.65 | 0.60 |
> | 4 | −3.15, 2.11, 2.68, 4.90 | 0.03, −1.29, −1.57, −3.23 | 0.22 |
>
> 还有另一种解法。我们要找的是一条分段线性函数。若至少有一个内部节点（$K \ge 2$），函数应满足以下两个约束：
>
> - 凸性（convexity）：$\alpha_1 \le \alpha_2 \le \cdots \le \alpha_K$
> - 连续性（continuity）：$\alpha_i a_i + \beta_i = \alpha_{i+1} a_i + \beta_{i+1}$，$i = 1, \ldots, K-1$。
>
> 因此，优化问题（opimization problem，原文如此）为
>
> ```math
> \begin{array}{ll}
> \text{minimize} & \displaystyle\sum_{i=1}^{m} \left( f(x_i) - y_i \right)^2 \\
> \text{subject to} & \alpha_i \le \alpha_{i+1}, \qquad i = 1, \ldots, K-1 \\
> & \alpha_i a_i + \beta_i = \alpha_{i+1} a_i + \beta_{i+1}, \qquad i = 1, \ldots, K-1
> \end{array}
> ```
>
> 通过把 $f(x_i)$ 写成矩阵形式来重新表述该问题，我们得到
>
> ```math
> \begin{array}{ll}
> \text{minimize} & \left\| \mathrm{diag}(x) F \alpha + F \beta - y \right\|_2 \\
> \text{subject to} & \alpha_i \le \alpha_{i+1}, \qquad i = 1, \ldots, K-1 \\
> & \alpha_i a_i + \beta_i = \alpha_{i+1} a_i + \beta_{i+1}, \qquad i = 1, \ldots, K-1
> \end{array}
> ```
>
> <!-- p.11 -->
>
> 其中变量为 $\alpha \in \mathbb{R}^K$ 与 $\beta \in \mathbb{R}^K$，问题数据为 $x \in \mathbb{R}^m$、$y \in \mathbb{R}^m$，以及
>
> ```math
> F_{ij} = \begin{cases}
> 1 & \text{若 } a_{j-1} = x_i,\ j = 1 \\
> 1 & \text{若 } a_{j-1} < x_i \le a_j \\
> 0 & \text{其他}
> \end{cases}
> ```
>
> ```matlab
> % another approach for PWL fitting problem
> clear all;
> pwl_fit_data;
> m    = length(x);
> xp = 0:0.001:1;
> % for fine-grained pwl function plot
> mp = length(xp);
> yp = [];
> for K = 1:4 % internal knot 1,2,3
> a = [0:1/K:1]'; % a_0,...,a_K
> % matrix for sum f(x_i)
> F = sparse(1:m,max(1,ceil(x*K)),1,m,K);
> % solve problem
> cvx_begin
> variables alpha(K) beta(K)
> minimize( norm(diag(x)*F*alpha+F*beta-y) )
> subject to
> if (K>=2)
> alpha(1:K-1).*a(2:K)+beta(1:K-1) == alpha(2:K).*a(2:K)+beta(2:K)
> a(1:K-1) <= a(2:K)
> end
> cvx_end
> fp = sparse(1:mp,max(1,ceil(xp*K)),1,mp,K);
> yp = [yp diag(xp)*fp*alpha+fp*beta];
> end
> plot(x,y,'b.',xp,yp);
> ```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这一题的两种解法值得对照记忆。第一种用 Lagrange 基把整条折线压缩成一个向量 $z$（节点上的函数值），于是"凸性"化为斜率非降的线性不等式——回想 Strang 线代里的判据：可微凸函数的导数单调不减；分段线性函数不可微，就把"导数"换成每段斜率，条件原封不动。第二种解法直接以每段的 $\alpha_i, \beta_i$ 为变量，凸性变成 $\alpha_1 \le \cdots \le \alpha_K$，连续性变成节点处两段函数值相等的等式约束。两种方法最后都要把答案整理成 $f(x) = \max_i (\alpha_i x + \beta_i)$："仿射函数的逐点最大值"是凸函数最基本的封闭运算，而且 max 形式可以直接嵌进任何优化问题里当目标或约束。另外提醒原文代码的一处笔误：第二个程序中凸性约束写成了 `a(1:K-1) <= a(2:K)`（只约束节点本身递增，而节点是生成的、恒为真），按文字推导应为 `alpha(1:K-1) <= alpha(2:K)`；注释 `internal knot 1,2,3` 也把"段数"当成了"内部节点数"——循环 `K = 1:4` 中 K 是段数，实际对应 0–3 个内部节点。

### <span style="color:#c0392b;">附加习题 4 区间系数矩阵的鲁棒最小二乘（Robust least-squares with interval coefficient matrix）</span>

<span style="color:#2471a3;">**[exercise]**</span> 4. Robust least-squares with interval coefficient matrix. An interval matrix in $\mathbb{R}^{m \times n}$ is a matrix whose entries are intervals:

```math
\mathcal{A} = \{ A \in \mathbb{R}^{m \times n} \mid |A_{ij} - \bar{A}_{ij}| \le R_{ij},\ i = 1, \ldots, m,\ j = 1, \ldots, n \}.
```

The matrix $\bar{A} \in \mathbb{R}^{m \times n}$ is called the nominal value or center value, and $R \in \mathbb{R}^{m \times n}$, which is elementwise nonnegative, is called the radius.

<!-- p.12 -->

The robust least-squares problem, with interval matrix, is

```math
\begin{array}{ll}
\text{minimize} & \displaystyle\sup_{A \in \mathcal{A}} \left\| A x - b \right\|_2,
\end{array}
```

with optimization variable $x \in \mathbb{R}^n$. The problem data are $\mathcal{A}$ (i.e., $\bar{A}$ and $R$) and $b \in \mathbb{R}^m$. The objective, as a function of x, is called the worst-case residual norm. The robust least-squares problem is evidently a convex optimization problem.

(a) Formulate the interval matrix robust least-squares problem as a standard optimization problem, e.g., a QP, SOCP, or SDP. You can introduce new variables if needed. Your reformulation should have a number of variables and constraints that grows linearly with m and n, and not exponentially.

(b) Consider the specific problem instance with m = 4, n = 3,

```math
A = \begin{bmatrix}
60 \pm 0.05 & 45 \pm 0.05 & -8 \pm 0.05 \\
90 \pm 0.05 & 30 \pm 0.05 & -30 \pm 0.05 \\
0 \pm 0.05 & -8 \pm 0.05 & -4 \pm 0.05 \\
30 \pm 0.05 & 10 \pm 0.05 & -10 \pm 0.05
\end{bmatrix},
\qquad
b = \begin{bmatrix}
-6 \\ -3 \\ 18 \\ -9
\end{bmatrix}.
```

(The first part of each entry in A gives $\bar{A}_{ij}$; the second gives $R_{ij}$, which are all 0.05 here.) Find the solution xls of the nominal problem (i.e., minimize $\|\bar{A}x - b\|_2$), and robust least-squares solution xrls. For each of these, find the nominal residual norm, and also the worst-case residual norm. Make sure the results make sense.

> <span style="color:#2471a3;">**[solution]**</span> **(a)** 该问题等价于
>
> ```math
> \begin{array}{ll}
> \text{minimize} & \displaystyle\sup_{A \in \mathcal{A}} \left\| A x - b \right\|_2^2,
> \end{array}
> ```
>
> 后者可以重新表述为
>
> ```math
> \begin{array}{ll}
> \text{minimize} & y^T y \\
> \text{subject to} & -y \preceq A x - b \preceq y, \qquad \text{对所有 } A \in \mathcal{A}.
> \end{array}
> ```
>
> 我们有
>
> ```math
> \begin{aligned}
> \sup_{A \in \mathcal{A}} (Ax - b)_i &= \sum_{j=1}^{n} \left( \bar{A}_{ij} x_j + R_{ij} |x_j| \right) - b_i \\
> \inf_{A \in \mathcal{A}} (Ax - b)_i &= \sum_{j=1}^{n} \left( \bar{A}_{ij} x_j - R_{ij} |x_j| \right) - b_i.
> \end{aligned}
> ```
>
> 因此可以把问题写成
>
> ```math
> \begin{array}{ll}
> \text{minimize} & y^T y \\
> \text{subject to} & \bar{A} x + R |x| - b \preceq y \\
> & \bar{A} x - R |x| - b \succeq -y
> \end{array}
> ```
>
> <!-- p.13 -->
>
> 其中 $|x| \in \mathbb{R}^n$ 是元素为 $|x|_i = |x_i|$ 的向量；或者等价地写成如下 QP：
>
> ```math
> \begin{array}{ll}
> \text{minimize} & y^T y \\
> \text{subject to} & \bar{A} x + R z - b \preceq y \\
> & \bar{A} x - R z - b \succeq -y \\
> & -z \preceq x \preceq z.
> \end{array}
> ```
>
> 变量为 $x \in \mathbb{R}^n$、$y \in \mathbb{R}^m$、$z \in \mathbb{R}^n$。
>
> 该问题还有一种替代表述：技巧在于为最坏情况残差范数找一个作为 $x$ 的函数的替代表达式。令
>
> ```math
> f(x) = \sup_{A \in \mathcal{A}} \left\| A x - b \right\|_2^2.
> ```
>
> ```math
> \begin{aligned}
> f(x) &= \sup \left\{ \left\| A x - b \right\|_2^2 \mid A = \bar{A} + \Delta,\ |\Delta_{ij}| \le R_{ij},\ i = 1, \ldots, m,\ j = 1, \ldots, n \right\} \\
> &= \sup \left\{ \left\| \bar{A} x + \Delta x - b \right\|_2^2 \mid |\Delta_{ij}| \le R_{ij},\ i = 1, \ldots, m,\ j = 1, \ldots, n \right\} \\
> &= \sup \left\{ \left\| r + \Delta x \right\|_2^2 \mid |\Delta_{ij}| \le R_{ij},\ i = 1, \ldots, m,\ j = 1, \ldots, n \right\},
> \end{aligned}
> ```
>
> 其中 $r = \bar{A} x - b$。由于
>
> ```math
> \left\| r + \Delta x \right\|_2^2 = \sum_{i=1}^{m} \left( r_i + \sum_{j=1}^{n} \Delta_{ij} x_j \right)^2 = \sum_{i=1}^{m} \left| r_i + \sum_{j=1}^{n} \Delta_{ij} x_j \right|^2,
> ```
>
> 容易看出该表达式关于 $\Delta$ 的各行是 separable（可分离）的。因此，为求出 $f(x)$ 的表达式，我们只需求出
>
> ```math
> \sup \left\{ \left| r_i + \sum_{j=1}^{n} \Delta_{ij} x_j \right| \ \middle|\ |\Delta_{ij}| \le R_{ij},\ j = 1, \ldots, n \right\}
> ```
>
> 的表达式。该上确界在 $r_i \ge 0$ 时取 $\Delta_{ij} = R_{ij}\, \mathrm{sign}(x_j)$、在 $r_i < 0$ 时取 $\Delta_{ij} = -R_{ij}\, \mathrm{sign}(x_j)$ 达到，且上确界等于 $|r_i| + \sum_{j=1}^{n} R_{ij} |x_j|$。于是
>
> ```math
> \begin{aligned}
> f(x) &= \sum_{i=1}^{m} \left( |r_i| + \sum_{j=1}^{n} R_{ij} |x_j| \right)^2 \\
> &= \left\| \left| \bar{A} x - b \right| + R |x| \right\|_2^2
> \end{aligned}
> ```
>
> 鲁棒最小二乘问题于是可以重新表述为
>
> ```math
> \begin{array}{ll}
> \text{minimize} & \left\| \left| \bar{A} x - b \right| + R |x| \right\|_2
> \end{array}
> ```
>
> <span style="color:#00838f;">注意该目标函数是凸的：因为欧氏范数（Euclidean norm）在 $\mathbb{R}^m_+$ 上是凸且递增的，而 $|\bar{A} x - b| + R|x|$ 凸且非负。</span>
>
> **(b)** 下面的脚本计算最小二乘解与鲁棒解，并对每一个分别计算标称残差范数（nominal residual norm）与最坏情况残差范数（worst-case residual norm）：
>
> ```matlab
> % input data
> A_bar = [ 60   45   -8; ...
>           90   30   -30; ...
>            0   -8   -4; ...
>           30   10   -10];
> d = .05;
> R = d*ones(4,3);
> ```
>
> <!-- p.14 -->
>
> ```matlab
> b = [ -6; -3; 18; -9];
> % least-squares solution
> x_ls = A_bar\b;
> % robust least-squares solution
> cvx_begin
> variables x(3) y(4) z(3)
> minimize ( norm( y ) )
> A_bar*x + R*z - b <= y
> A_bar*x - R*z - b >= -y
> x <= z
> x + z >= 0
> cvx_end
> % computing nominal residual norms
> nom_res_ls = norm(A_bar*x_ls - b);
> nom_res_rls = norm(A_bar*x - b);
> % computing worst-case nominal norms
> r = A_bar*x_ls - b;
> Delta = zeros(4,3);
> for i=1:length(r)
> if r(i) < 0
> Delta(i,:) = -d*sign(x_ls');
> else
> Delta(i,:) = d*sign(x_ls');
> end
> end
> wc_res_ls = norm(r + Delta*x_ls);
> wc_res_rls = cvx_optval;
> % display
> disp('Residual norms for the nominal problem when using LS solution: ');
> disp(nom_res_ls);
> disp('Residual norms for the nominal problem when using robust solution: ');
> disp(nom_res_rls);
> disp('Residual norms for the worst-case problem when using LS solution: ');
> disp(wc_res_ls);
> disp('Residual norms for the worst-case problem when using robust solution: ');
> disp(wc_res_rls);
> ```
>
> 鲁棒最小二乘解也可以用下面的脚本求得：
>
> <!-- p.15 -->
>
> ```matlab
> cvx_begin
> variables x(3) t(4)
> minimize ( norm ( t ) )
> abs(A_bar*x - b) + R*abs(x) <= t
> cvx_end
> ```
>
> 该脚本返回如下结果：
>
> - 使用 LS 解时，标称问题（nominal problem）的残差范数：7.5895
> - 使用鲁棒解时，标称问题的残差范数：17.7106
> - 使用 LS 解时，最坏情况问题（worst-case problem）的残差范数：26.7012
> - 使用鲁棒解时，最坏情况问题的残差范数：17.7940
>
> 我们还顺带生成了下面的直方图，展示 $x = x_{ls}$ 与 $x = x_{rls}$ 两种情形下残差范数的分布。做法是：让 $A_{ij}$ 在 $\bar{A}_{ij} - R_{ij}$ 与 $\bar{A}_{ij} + R_{ij}$ 之间均匀采样，生成 1000 个矩阵 $A$ 的实例，然后对每个 $A$ 以及这两个解分别计算残差范数。
>
> <span style="color:#7f8c8d;">（图，第 15 页：横轴范围 0 到 20。）Residual norm distribution for x = xls —— $x = x_{ls}$ 的残差范数分布。</span>
>
> <!-- p.16 -->
>
> <span style="color:#7f8c8d;">（图，第 16 页：横轴范围 17.64 到 17.8。）Residual norm distribution for x = xrls —— $x = x_{rls}$ 的残差范数分布。</span>
>
> 下面的脚本生成这些直方图：
>
> ```matlab
> % Monte-Carlo simulation
> N = 1000;
> res_ls = zeros(N,1);
> res_rls = zeros(N,1);
> for k=1:N
> Delta = d*(2*rand(4,3)-1);
> A = A_bar + Delta;
> res_ls(k) = norm(A*x_ls - b);
> res_rls(k) = norm(A*x - b);
> end
> figure;
> hist(res_ls,50);
> figure;
> hist(res_rls,50);
> ```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 区间矩阵鲁棒最小二乘是鲁棒优化（robust optimization）最干净的教学样例，推导骨架值得记住：目标是对不确定集 $\mathcal{A}$ 取最坏情况（sup），而矩形不确定集让这个 sup 逐行、逐元素地解耦——每行残差的最坏值就是 $|r_i| + \sum_j R_{ij} |x_j|$，恰好是"标称残差 + 半径 × |x|"。最终目标 $\||\bar{A}x - b| + R|x|\|_2$ 只比普通最小二乘多出一项 $R|x|$：它相当于对 $x$ 施加一种数据驱动的收缩（shrinkage），哪个系数的不确定度贡献大，就在哪个方向上压。数值对比非常直观：LS 解的标称残差 7.5895 更小，但最坏情况 26.7012 灾难性地变差；鲁棒解标称残差 17.7106 略差，最坏情况 17.7940 却几乎不变——这就是"用平均性能换最坏性能"的保险费。两条直方图更妙：LS 解的残差分布又宽又散，鲁棒解挤在 17.8 附近一条极窄的带里。这与你今后在数值线性代数里关心的稳定性一脉相承：CSAPP 讲过浮点表示误差如何在计算中被放大，这里演示的则是系数摄动如何进入解的质量——两者都是"扰动分析"思维。

<!-- p.17 -->

### <span style="color:#c0392b;">附加习题 5 全变差图像插值（Total variation image interpolation）</span>

<span style="color:#2471a3;">**[exercise]**</span> 5. Total variation image interpolation. A grayscale image is represented as an $m \times n$ matrix of intensities $U^{\mathrm{orig}}$. You are given the values $U^{\mathrm{orig}}_{ij}$, for $(i, j) \in K$, where $K \subset \{1, \ldots, m\} \times \{1, \ldots, n\}$. Your job is to interpolate the image, by guessing the missing values. The reconstructed image will be represented by $U \in \mathbb{R}^{m \times n}$, where U satisfies the interpolation conditions $U_{ij} = U^{\mathrm{orig}}_{ij}$ for $(i, j) \in K$.

The reconstruction is found by minimizing a roughness measure subject to the interpolation conditions. One common roughness measure is the ℓ2 variation (squared),

```math
\sum_{i=2}^{m} \sum_{j=2}^{n} \left( \left( U_{ij} - U_{i-1,j} \right)^2 + \left( U_{ij} - U_{i,j-1} \right)^2 \right).
```

Another method minimizes instead the total variation,

```math
\sum_{i=2}^{m} \sum_{j=2}^{n} \left( \left| U_{ij} - U_{i-1,j} \right| + \left| U_{ij} - U_{i,j-1} \right| \right).
```

Evidently both methods lead to convex optimization problems.

Carry out ℓ2 and total variation interpolation on the problem instance with data given in tv_img_interp.m. This will define m, n, and matrices Uorig and Known. The matrix Known is m × n, with (i, j) entry one if $(i, j) \in K$, and zero otherwise. The mfile also has skeleton plotting code. (We give you the entire original image so you can compare your reconstruction to the original; obviously your solution cannot access $U^{\mathrm{orig}}_{ij}$ for $(i, j) \notin K$.)

> <span style="color:#2471a3;">**[solution]**</span> 插值的代码非常简单。对 ℓ2 插值，代码如下：
>
> ```matlab
> cvx_begin
> variable Ul2(m, n);
> Ul2(Known) == Uorig(Known); % Fix known pixel values.
> Ux = Ul2(2:end,2:end) - Ul2(2:end,1:end-1); % x (horiz) differences
> Uy = Ul2(2:end,2:end) - Ul2(1:end-1,2:end); % y (vert) differences
> minimize(norm([Ux(:); Uy(:)], 2)); % l2 roughness measure
> cvx_end
> ```
>
> 对全变差（total variation）插值，我们使用如下代码：
>
> ```matlab
> cvx_begin
> variable Utv(m, n);
> Utv(Known) == Uorig(Known); % Fix known pixel values.
> Ux = Utv(2:end,2:end) - Utv(2:end,1:end-1); % x (horiz) differences
> Uy = Utv(2:end,2:end) - Utv(1:end-1,2:end); % y (vert) differences
> minimize(norm([Ux(:); Uy(:)], 1)); % tv roughness measure
> cvx_end
> ```
>
> 我们得到下列图像：
>
> <!-- p.18 -->
>
> <span style="color:#7f8c8d;">（图，第 18 页：四幅 $50 \times 50$ 左右的灰度子图，坐标刻度均为 10–50。）</span>
>
> <span style="color:#7f8c8d;">Original image（原始图像）· Obscured image（被遮挡图像）· ℓ2 reconstructed image（ℓ2 重建图像）· Total variation reconstructed image（全变差重建图像）。</span>

### <span style="color:#c0392b;">附加习题 6 松弛与离散的 A-最优实验设计（Relaxed and discrete A-optimal experiment design）</span>

<span style="color:#2471a3;">**[exercise]**</span> 6. Relaxed and discrete A-optimal experiment design. This problem concerns the A-optimal experiment design problem, described on page 387, with data generated as follows.

```matlab
n = 5; % dimension of parameters to be estimated
p = 20; % number of available types of measurements
m = 30; % total number of measurements to be carried out
randn('state', 0);
V=randn(n,p); % columns are vi, the possible measurement vectors
```

Solve the relaxed A-optimal experiment design problem,

```math
\begin{array}{ll}
\text{minimize} & (1/m)\, \mathrm{tr}\left( \sum_{i=1}^{p} \lambda_i v_i v_i^T \right)^{-1} \\
\text{subject to} & \mathbf{1}^T \lambda = 1, \qquad \lambda \succeq 0,
\end{array}
```

with variable $\lambda \in \mathbb{R}^p$. Find the optimal point $\lambda^\star$ and the associated optimal value of the relaxed problem. This optimal value is a lower bound on the optimal value of the discrete A-optimal experiment design problem,

```math
\begin{array}{ll}
\text{minimize} & \mathrm{tr}\left( \sum_{i=1}^{p} m_i v_i v_i^T \right)^{-1} \\
\text{subject to} & m_1 + \cdots + m_p = m, \\
& m_i \in \{0, \ldots, m\}, \quad i = 1, \ldots, p,
\end{array}
```

<!-- p.19 -->

with variables $m_1, \ldots, m_p$. To get a suboptimal point for this discrete problem, round the entries in $m \lambda^\star$ to obtain integers $\hat{m}_i$. If needed, adjust these by hand or some other method to ensure that they sum to m, and compute the objective value obtained. This is, of course, an upper bound on the optimal value of the discrete problem. Give the gap between this upper bound and the lower bound obtained from the relaxed problem. Note that the two objective values can be interpreted as mean-square estimation error $E \|\hat{x} - x\|_2^2$.

> <span style="color:#2471a3;">**[solution]**</span> 松弛问题的目标函数是凸的，因此它是凸问题。把它写进 cvx 需要一点小技巧。我们想直接把目标写成 `minimize ((1/m)*trace(inv(V*diag(lambda)*V')))`，但这样行不通，因为 cvx 并不认识矩阵凸函数（matrix convex functions）。取而代之，我们可以把目标表示为若干 matrix fractional functions（矩阵分式函数）之和：
>
> ```math
> \begin{array}{ll}
> \text{minimize} & (1/m) \displaystyle\sum_{k=1}^{n} e_k^T \left( \sum_{i=1}^{p} \lambda_i v_i v_i^T \right)^{-1} e_k \\
> \text{subject to} & \mathbf{1}^T \lambda = 1, \qquad \lambda \succeq 0.
> \end{array}
> ```
>
> 其中 $e_k \in \mathbb{R}^n$ 是第 $k$ 个单位向量（unit vector）。（注意：$e$ 在习题 6.9 中被定义为估计误差向量（estimation error vector），因此 $e_k$ 也可能指误差向量的第 $k$ 个分量；但在这里，$e_k$ 显然是指第 $k$ 个单位向量。）
>
> 在 cvx 中可以用函数 `matrix_frac` 来表达它。下面的代码求解该问题：
>
> ```matlab
> n = 5; % dimension
> p = 20; % number of available types of measurements
> m = 30; % total number of measurements to be carried out
> randn('state', 0);
> V=randn(n,p); % columns are vi, the possible measurement vectors
> cvx_begin
> variable lambda(p)
> obj = 0;
> for k=1:n
> ek = zeros(n,1);
> ek(k)=1;
> obj = obj + (1/m)*matrix_frac(ek,V*diag(lambda)*V');
> end
> minimize( obj )
> subject to
> sum(lambda) == 1
> lambda >= 0
> ```
>
> <!-- p.20 -->
>
> ```matlab
> cvx_end
> lower_bound = cvx_optval
> t = -0.00; % small offset chosen by hand to make rounding work out.
> % for this problem data, none is needed!
> m_rnd = pos(round(m*lambda+t));
> sum(m_rnd) % should be == m
> % now find objective value of rounded experiment design
> upper_bound = trace(inv(V*diag(m_rnd/m)*V'))/m
> gap = upper_bound-lower_bound
> rel_gap = gap/lower_bound
> ```
>
> 对该问题实例，简单取整（rounding）得到的 $\hat{m}_i$ 恰好之和为 $m = 30$，因此无需再对取整值作任何调整。下界（lower bound）为 0.2481；上界（upper bound）为 0.2483。两者之间的间隔（gap）为 0.00023，约 0.1%。
>
> 这意味着：我们找到了一种从 20 种可选测量中挑选 30 次测量的方案，其均方估计误差 $E\|\hat{x} - x\|_2^2 = 0.2483$。我们并不知道这是否就是 30 次测量的最优选择，但我们确知：该方案至多次优 0.1%；最优方案所能达到的均方误差不会低于 0.2481。我们的实验设计即使不是最优的，也已经非常接近最优。（事实上，它极可能就是最优的。）

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 实验设计这题完整演示了"凸松弛 + 取整"的标准套路，与 6.006 里解整数规划用的 LP 松弛—舍入（rounding）是同一个思想：把整数变量 $m_i \in \{0, \ldots, m\}$ 松弛成比例 $\lambda_i \in [0,1]$（约束 $\mathbf{1}^T \lambda = 1$），松弛问题凸、可解，其最优值给离散问题提供下界；再把 $m\lambda^\star$ 逐项取整、微调到和为 $m$，得到可行的整数方案，其目标值是上界；两个界一夹，就能量出当前方案离最优有多远——本题只差 0.1%，基本可以断定已是最优。目标函数取 $\mathrm{tr}(\sum_i m_i v_i v_i^T)^{-1}$ 也不是随意的：A-最优性（A-optimality）最小化的正是估计误差协方差的迹，即均方估计误差 $E\|\hat{x} - x\|_2^2$——直接对接你正在学的 Bertsekas 概率论里 MSE 的概念（无偏估计下它就是各参数估计方差之和），测量向量 $v_i$ 扮演"信息"的角色，选得越分散、协方差越小。CVX 实现里把矩阵求逆的迹拆成 $e_k^T (\cdot)^{-1} e_k$ 的矩阵分式函数（`matrix_frac`）之和，是因为矩阵凸性不在 CVX 的 DCP 规则集内——这种"等价改写"正是用 CVX 建模的基本功。
