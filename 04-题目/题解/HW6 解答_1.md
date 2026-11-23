# EE364a Homework 6 solutions（EE364a 作业 6 解答）

> 原文：`HW6 解答.pdf`（20 页），EE364a, Winter 2007-08, Prof. S. Boyd。第 1 页是教材习题解答（6.9），第 2 页起是 "Solutions to additional exercises"（附加题 6 道：指数函数的 minimax 有理逼近、球队实力的 maximum likelihood 预测、piecewise-linear 拟合、区间系数矩阵的 robust least-squares、total variation 图像插值、松弛与离散 A-optimal 实验设计）。

## 6.9 Minimax rational function fitting（minimax 有理函数拟合）

证明下列问题为 quasiconvex：

```math
\text{minimize} \quad \max_{i=1,\dots,k} \left| \frac{p(t_i)}{q(t_i)} - y_i \right|
```

其中

```math
p(t) = a_0 + a_1 t + a_2 t^2 + \cdots + a_m t^m, \qquad q(t) = 1 + b_1 t + \cdots + b_n t^n,
```

且 objective function 的定义域为

```math
\mathcal{D} = \{(a,b) \in \mathbf{R}^{m+1} \times \mathbf{R}^n \mid q(t) > 0,\ \alpha \le t \le \beta\} .
```

本题中我们把有理函数 $p(t)/q(t)$ 拟合到给定数据，同时约束分母多项式在区间 $[\alpha,\beta]$ 上为正。优化变量是分子与分母的系数 $a_i$、$b_i$。插值点 $t_i \in [\alpha,\beta]$ 与目标函数值 $y_i$（$i = 1,\dots,k$）给定。

**Solution**。我们来证明 objective 是 quasiconvex 的。它的定义域是 convex 的。由于 $q(t_i) > 0$（$i = 1,\dots,k$），有

```math
\max_{i=1,\dots,k} |p(t_i)/q(t_i) - y_i| \le \gamma
```

当且仅当

```math
|p(t_i) - y_i q(t_i)| \le \gamma q(t_i), \qquad i = 1,\dots,k,
```

而这在变量 $a$ 与 $b$ 中定义了一个 convex set：左边是 convex 的，右边是 linear 的。我们可以把这些不等式进一步写成 $2k$ 条 linear inequality：

```math
-\gamma q(t_i) \le p(t_i) - y_i q(t_i) \le \gamma q(t_i), \qquad i = 1,\dots,k .
```

（这就是 quasiconvexity 的定义：所有 sublevel set 都是 convex set。注意这里 $q(t_i) > 0$ 是关键——不等号两边乘以正数才不翻转。）

# Solutions to additional exercises（附加题解答）

## 附加题 1 · Minimax rational fit to the exponential（指数函数的 minimax 有理拟合）

（见习题 6.9。）考虑具体实例，数据为

```math
t_i = -3 + 6(i-1)/(k-1), \qquad y_i = e^{t_i}, \qquad i = 1,\dots,k,
```

其中 $k = 201$。（换句话说，数据是把指数函数在区间 $[-3,3]$ 上均匀采样得到的。）求形如

```math
f(t) = \frac{a_0 + a_1 t + a_2 t^2}{1 + b_1 t + b_2 t^2}
```

的函数，使 $\max_{i=1,\dots,k} |f(t_i) - y_i|$ 最小。（我们要求 $1 + b_1 t_i + b_2 t_i^2 > 0$ 对 $i = 1,\dots,k$ 成立。）

求出 $a_0$、$a_1$、$a_2$、$b_1$、$b_2$ 的 optimal values，并给出 optimal objective value，计算精度 $0.001$。在同一张图上画出数据与 optimal rational function fit。另画一张图给出 fitting error，即 $f(t_i) - y_i$。

提示：可以在 `cvx_end` 之后用 `strcmp(cvx_status,'Solved')` 检查一个 feasibility problem 是否可行。

**Solution**。objective function（因而整个问题）不是 convex 的，但它是 quasiconvex 的。有 $\max_{i=1,\dots,k} |f(t_i) - y_i| \le \gamma$ 当且仅当

```math
\left| \frac{a_0 + a_1 t_i + a_2 t_i^2}{1 + b_1 t_i + b_2 t_i^2} - y_i \right| \le \gamma, \qquad i = 1,\dots,k .
```

（由于分母为正，）这等价于

```math
|a_0 + a_1 t_i + a_2 t_i^2 - y_i(1 + b_1 t_i + b_2 t_i^2)| \le \gamma(1 + b_1 t_i + b_2 t_i^2), \qquad i = 1,\dots,k,
```

这是变量 $a$、$b$ 上的 $2k$ 条 linear inequality（对固定 $\gamma$ 而言）。特别地，这说明 objective 是 quasiconvex 的。（事实上它是 generalized linear fractional function。）

为求解该问题，我们可以用 bisection method（二分法），每步解一个 LP feasibility problem。每步选定某个 $\gamma$ 值，求解 feasibility problem

```math
\begin{aligned}
\text{find} \quad & a,\ b \\
\text{subject to} \quad & |a_0 + a_1 t_i + a_2 t_i^2 - y_i(1 + b_1 t_i + b_2 t_i^2)| \le \gamma(1 + b_1 t_i + b_2 t_i^2), \quad i = 1,\dots,k,
\end{aligned}
```

变量为 $a$ 与 $b$。（注意只要 $\gamma > 0$，「分母为正」这一条件就被自动强制满足。）这可以转成 LP feasibility problem

```math
\begin{aligned}
\text{find} \quad & a,\ b \\
\text{subject to} \quad & a_0 + a_1 t_i + a_2 t_i^2 - y_i(1 + b_1 t_i + b_2 t_i^2) \le \gamma(1 + b_1 t_i + b_2 t_i^2), \quad i = 1,\dots,k \\
& a_0 + a_1 t_i + a_2 t_i^2 - y_i(1 + b_1 t_i + b_2 t_i^2) \ge -\gamma(1 + b_1 t_i + b_2 t_i^2), \quad i = 1,\dots,k .
\end{aligned}
```

下面的 Matlab 代码求解该具体实例。

```matlab
k=201;
t=(-3:6/(k-1):3)';
y=exp(t);
Tpowers=[ones(k,1) t t.^2];
u=exp(3); l=0; % initial upper and lower bounds
bisection_tol=1e-3; % bisection tolerance
while u-l>= bisection_tol
gamma=(l+u)/2;
cvx_begin % solve the feasibility problem
cvx_quiet(true);
variable a(3);
variable b(2);
subject to
abs(Tpowers*a-y.*(Tpowers*[1;b])) <= gamma*Tpowers*[1;b];
cvx_end
if strcmp(cvx_status,'Solved')
u=gamma;
a_opt=a;
b_opt=b;
objval_opt=gamma;
else
l=gamma;
end
end
y_fit=Tpowers*a_opt./(Tpowers*[1;b_opt]);
figure(1);
plot(t,y,'b', t,y_fit,'r+');
xlabel('t');
ylabel('y');
figure(2);
plot(t, y_fit-y);
xlabel('t');
ylabel('err');
```

optimal values 为

```math
a_0 = 1.0099,\ a_1 = 0.6117,\ a_2 = 0.1134,\ b_1 = -0.4147,\ b_2 = 0.0485,
```

optimal objective value 为 $0.0233$。我们还得到下面两张图。

[Figure 1：Chebyshev fit with rational function（有理函数的 Chebyshev 拟合）——横轴 $t \in [-3,3]$、纵轴 $y \in [0,25]$；蓝线为数据 $y = e^t$（从约 $0.05$ 升到约 $20$），红色十字符号标出拟合值，两者几乎无法区分]

[Figure 2：指数函数有理 Chebyshev 拟合的 fitting error——横轴 $t \in [-3,3]$、纵轴 err；误差呈等波纹（equioscillating）形态：在 $-3$ 附近约 $-0.02$、在 $-1.5$ 附近约 $+0.023$、在 $0$ 附近约 $-0.02$、在 $1.5$ 附近约 $+0.02$、在 $3$ 附近约 $-0.02$，最大绝对值约 $0.0233$，正是 optimal objective value]

（读图要点（译者补）：误差曲线的等波纹形态是 minimax（Chebyshev）准则的标志——最优解把误差在若干点上「压平」到同一幅值，交替变号。若某个区间的误差绝对值明显小于 $\gamma$，说明那里不是 active constraint，可以通过重新分配系数再降一点 $\gamma$。）

## 附加题 2 · Maximum likelihood prediction of team ability（球队实力的 maximum likelihood 预测）

有 $n$ 支球队参加一个 tournament（联赛）。我们把每支球队的实力用一个数 $a_j \in [0,1]$（$j = 1,\dots,n$）建模。当球队 $j$ 与 $k$ 交手时，球队 $j$ 获胜的概率等于 $\mathrm{prob}(a_j - a_k + v > 0)$，其中 $v \sim \mathcal{N}(0,\sigma^2)$。

给定 $m$ 场已结束比赛的胜负结果，表示为

```math
(j^{(i)},\ k^{(i)},\ y^{(i)}), \qquad i = 1,\dots,m,
```

意思是第 $i$ 场由球队 $j^{(i)}$ 对球队 $k^{(i)}$；$y^{(i)} = 1$ 表示球队 $j^{(i)}$ 获胜，$y^{(i)} = -1$ 表示球队 $k^{(i)}$ 获胜。（我们假设没有平局。）

**(a)** 把「给定这些结果，求球队实力的 maximum likelihood estimate（最大似然估计）$a^\star \in \mathbf{R}^n$」表述为一个 convex optimization problem。你会发现 game incidence matrix（比赛关联矩阵）$A \in \mathbf{R}^{m \times n}$ 很有用，其定义为

```math
A_{il} = \begin{cases} y^{(i)} & l = j^{(i)} \\ -y^{(i)} & l = k^{(i)} \\ 0 & \text{otherwise} . \end{cases}
```

prior constraints（先验约束）$a_i \in [0,1]$ 应当包含在问题表述中。另外注意：若给所有球队实力加上同一个常数，比赛结果的概率不变。这意味着 $a^\star$ 只能确定到一个常数（像势能那样）。但这不影响 ML 估计问题，也不影响用估计出的参数做出的任何后续预测。

**(b)** 对 `team_data.m` 中给出的球队数据（矩阵 `train`）求 $a^\star$。（该矩阵给出的是一个每两队各交手一次的联赛的结果。）

CVX 不支持 concave function $\log\Phi$（$\Phi$ 是单位 Gaussian 的累积分布），但我们在课程网站上提供了一个足够好的近似 `log_normcdf`。该函数被重载以处理向量输入（逐元素）。

可以用下面的命令构造 $A$：

```matlab
A = sparse(1:m,train(:,1),train(:,3),m,n) + ...
sparse(1:m,train(:,2),-train(:,3),m,n);
```

**(c)** 用 (b) 中求得的 maximum likelihood estimate $a^\star$ 预测下一年联赛各场比赛的结果（见矩阵 `test`），预测式为 $\hat{y}^{(i)} = \mathrm{sign}(a^\star_{j^{(i)}} - a^\star_{k^{(i)}})$。把这些预测与 `test` 第三列给出的实际结果比较，给出正确预测的比例。

`train` 与 `test` 中的比赛是同一批，因此另一种更简单的预测方法是「假定去年赢的那支队今年还会赢」。给出这种简单方法的正确预测百分比。

**Solution**。

**(a)** 给定 $a$ 时结果 $y$ 的 likelihood 为

```math
p(y \mid a) = \prod_{i=1,\dots,m} \Phi\!\left( \frac{1}{\sigma} y^{(i)} (a_{j^{(i)}} - a_{k^{(i)}}) \right),
```

其中 $\Phi$ 是标准 normal distribution 的累积分布函数。因此 log-likelihood function 为

```math
l(a) = \log p(y \mid a) = \sum_{i} \log \Phi\big( (1/\sigma)(Aa)_i \big) .
```

这是 concave function。（因为 $\log\Phi$ 是 concave 且非减的，$(1/\sigma)Aa$ 是 $a$ 的 affine 函数，故复合仍 concave。）

maximum likelihood estimate $a^\star$ 是

```math
\begin{aligned}
\text{maximize} \quad & l(a) \\
\text{subject to} \quad & 0 \preceq a \preceq \mathbf{1}
\end{aligned}
```

的任一解。这是 convex optimization problem，因为被极大化的 objective 是 concave 的，约束是 $2n$ 条 linear inequality。

**(b)** 下面的代码求解该问题

```matlab
% Form adjacency matrix
A1 = sparse(1:m,train(:,1),train(:,3),m,n);
A2 = sparse(1:m,train(:,2),-train(:,3),m,n);
A = A1+A2;
% Estimate abilities
cvx_begin
variable a_hat(n)
minimize(-sum(log_normcdf(A*a_hat/sigma)))
subject to
a_hat >= 0
a_hat <= 1
cvx_end
```

用这段代码得到

```math
a^\star = (1.0,\ 0.0,\ 0.68,\ 0.37,\ 0.79,\ 0.58,\ 0.38,\ 0.09,\ 0.67,\ 0.58).
```

**(c)** 下面的代码用于预测 `test` 集中的结果

```matlab
% Estimate errors in test set
res = sign(a_hat(test(:,1))-a_hat(test(:,2)));
Pml = 1-length(find(res-test(:,3)))/m_test
Ply = 1-length(find(train(:,3)-test(:,3)))/m_test
```

maximum likelihood estimate 对 `test` 中 $86.7\%$ 的比赛给出了正确预测。另一方面，`test` 中有 $75.6\%$ 的比赛与 `train` 中的比赛结果相同。

（读法：`Pml` 是 ML 预测正确率，`Ply` 是「去年赢家今年还赢」的正确率。差 11 个百分点，说明用实力差模型确实抓到了比「重复去年结果」更多的信息。）

## 附加题 3 · Piecewise-linear fitting（分段线性拟合）

在许多应用中，模型中的某个函数不是由公式给出，而是以表格数据（tabulated data）的形式给出。这种表格数据可能来自经验测量、历史数据、对某个复杂表达式的数值求值，或对某个问题在若干自变量取值上的求解。为了在 convex optimization model 中使用它，我们必须用一个与求解器或其它系统兼容的 convex function 去拟合这些数据。本题探索这类一般问题中一个非常简单的例子。

假设给定数据 $(x_i, y_i)$，$i = 1,\dots,m$，其中 $x_i, y_i \in \mathbf{R}$。我们假设 $x_i$ 已排序，即 $x_1 < x_2 < \cdots < x_m$。设 $a_0 < a_1 < a_2 < \cdots < a_K$ 是一组固定的 knot points（节点），且 $a_0 \le x_1$、$a_K \ge x_m$。说明如何找出定义在 $[a_0,a_K]$ 上、以 $a_i$ 为节点的 convex piecewise linear function $f$，使 least-squares fitting criterion（最小二乘拟合准则）

```math
\sum_{i=1}^m (f(x_i) - y_i)^2
```

最小。你必须说明变量是什么、它们如何参数化 $f$，以及如何保证 $f$ 的 convexity。

**Hints（提示）**。一种方法是基于 Lagrange basis（Lagrange 基）$f_0,\dots,f_K$，它们是满足

```math
f_j(a_i) = \delta_{ij}, \qquad i,j = 0,\dots,K
```

的 piecewise linear function。另一种方法是定义 $f(x) = \alpha_i x + \beta_i$（$x \in (a_{i-1},a_i]$），然后对参数 $\alpha_i$ 与 $\beta_i$ 加上条件以保证 $f$ 连续且 convex。

把你的方法应用到文件 `pwl_fit_data.m` 中的数据（其中 $x_j \in [0,1]$）。求出最好的 affine fit（对应 $a = (0,1)$），以及 1、2、3 个 internal knot point（在 $[0,1]$ 内均匀分布）时最好的 piecewise-linear convex function fit。（例如，3 个 internal knot point 时 $a_0 = 0$、$a_1 = 0.25$、$a_2 = 0.50$、$a_3 = 0.75$、$a_4 = 1$。）给出每种的 least-squares fitting cost。画出数据与求得的分段线性拟合。把每个函数表示成

```math
f(x) = \max_{i=1\dots,K} (\alpha_i x + \beta_i)
```

的形式。（在这种形式下，函数很容易被纳入优化问题。）

**Solution**。按提示，我们使用 Lagrange basis functions $f_0,\dots,f_K$。它们可以写成

```math
f_0(x) = \left( \frac{a_1 - x}{a_1 - a_0} \right)_+,
```

```math
f_i(x) = \min\left\{ \left( \frac{x - a_{i-1}}{a_i - a_{i-1}} \right)_+,\ \left( \frac{a_{i+1} - x}{a_i - a_{i+1}} \right)_+ \right\}, \qquad i = 1,\dots,K-1,
```

以及

```math
f_K(x) = \left( \frac{x - a_{K-1}}{a_K - a_{K-1}} \right)_+ .
```

（记号 $(\cdot)_+ = \max\{0,\ \cdot\}$：单侧截断的「帽函数」。$f_i$ 在节点 $a_i$ 处取 $1$、在其它节点处取 $0$，正是分段线性的 Lagrange 基。）

函数 $f$ 可以参数化为

```math
f(x) = \sum_{i=0}^K z_i f_i(x),
```

其中 $z_i = f(a_i)$，$i = 0,\dots,K$。我们用 $z = (z_0,\dots,z_K)$ 参数化 $f$。least-squares fitting criterion 于是为

```math
J = \sum_{i=1}^m (f(x_i) - y_i)^2 = \|Fz - y\|_2^2,
```

其中 $F \in \mathbf{R}^{m \times (K+1)}$ 是矩阵

```math
F_{ij} = f_j(x_i), \qquad i = 1,\dots,m,\quad j = 0,\dots,K .
```

（这里把 $F$ 的列下标取为 $0$ 到 $K$。）

我们必须加上 $f$ 为 convex 的约束。这等同于各段斜率非减，即

```math
\frac{z_{i+1} - z_i}{a_{i+1} - a_i} \ge \frac{z_i - z_{i-1}}{a_i - a_{i-1}}, \qquad i = 1,\dots,K-1 .
```

这是 $z$ 上的一组 linear inequality。因此，最好的 PWL convex fit 可以通过求解 QP

```math
\begin{aligned}
\text{minimize} \quad & \|Fz - y\|_2^2 \\
\text{subject to} \quad & \frac{z_{i+1} - z_i}{a_{i+1} - a_i} \ge \frac{z_i - z_{i-1}}{a_i - a_{i-1}}, \quad i = 1,\dots,K-1
\end{aligned}
```

得到。（关键：convexity 在这里化为一组**线性**约束，因为节点 $a_i$ 固定，分母是常数，约束对 $z$ 是线性的。这就是「用 Lagrange 基参数化」相对「用 $\alpha_i,\beta_i$ 参数化」的优势——后者还要额外处理连续性。）

下面的代码对 `pwl_fit_data` 中的数据求解该问题。

```matlab
cvx_quiet('true')
figure
plot(x,y,'k:','linewidth',2)
hold on
% Single line
p = [x ones(100,1)]\y;
alpha = p(1)
beta = p(2)
plot(x,alpha*x+beta,'b','linewidth',2)
mse = norm(alpha*x+beta-y)^2
for K = 2:4
% Generate Lagrange basis
a = (0:(1/K):1)';
F = max((a(2)-x)/(a(2)-a(1)),0);
for k = 2:K
a_1 = a(k-1);
a_2 = a(k);
a_3 = a(k+1);
f = max(0,min((x-a_1)/(a_2-a_1),(a_3-x)/(a_3-a_2)));
F = [F f];
end
f = max(0,(x-a(K))/(a(K+1)-a(K)));
F = [F f];
% Solve problem
cvx_begin
variable z(K+1)
minimize(norm(F*z-y))
subject to
(z(3:end)-z(2:end-1))./(a(3:end)-a(2:end-1)) >=...
(z(2:end-1)-z(1:end-2))./(a(2:end-1)-a(1:end-2))
cvx_end
% Calculate alpha and beta
alpha = (z(2:end)-z(1:end-1))./(a(2:end)-a(1:end-1))
beta = z(2:end)-alpha(1:end).*a(2:end)
% Plot solution
y2 = F*z;
mse = norm(y2-y)^2
if K==2
plot(x,y2,'r','linewidth',2)
elseif K==3
plot(x,y2,'g','linewidth',2)
else
plot(x,y2,'m','linewidth',2)
end
end
xlabel('x')
ylabel('y')
```

[Figure 3：$K = 1,2,3,4$ 的分段线性近似——横轴 $x \in [0,1]$、纵轴 $y \in [-1,2]$；黑色点线为数据（先降后升、末端上翘），蓝线（$K=1$）为一条直线，红线（$K=2$）、绿线（$K=3$）、品红线（$K=4$）依次更贴合数据，且都保持 convex（斜率单调不减）]

这生成图 3。可以看到近似随 $K$ 增大而改善。下表给出该近似的结果。

| $K$ | $\alpha_1,\dots,\alpha_K$ | $\beta_1,\dots,\beta_K$ | $J$ |
|---|---|---|---|
| 1 | 1.91 | $-0.87$ | 12.73 |
| 2 | $-0.27$, 4.09 | $-0.33$, $-2.51$ | 2.62 |
| 3 | $-1.80$, 2.67, 4.25 | $-0.10$, $-1.59$, $-2.65$ | 0.60 |
| 4 | $-3.15$, 2.11, 2.68, 4.90 | 0.03, $-1.29$, $-1.57$, $-3.23$ | 0.22 |

还有另一种解法。我们找的是一个 piecewise linear function。若至少有一个 internal knot（$K \ge 2$），该函数应满足下面两个约束：

- convexity：$\alpha_1 \le \alpha_2 \le \cdots \le \alpha_K$
- continuity：$\alpha_i a_i + \beta_i = \alpha_{i+1}a_i + \beta_{i+1}$，$i = 1,\dots,K-1$。

于是优化问题为

```math
\begin{aligned}
\text{minimize} \quad & \sum_{i=1}^m (f(x_i) - y_i)^2 \\
\text{subject to} \quad & \alpha_i \le \alpha_{i+1}, \quad i = 1,\dots,K-1 \\
& \alpha_i a_i + \beta_i = \alpha_{i+1}a_i + \beta_{i+1}, \quad i = 1,\dots,K-1 .
\end{aligned}
```

把 $f(x_i)$ 用矩阵形式表示来改写该问题，得

```math
\begin{aligned}
\text{minimize} \quad & \|\mathbf{diag}(x) F \alpha + F \beta - y\|_2^2 \\
\text{subject to} \quad & \alpha_i \le \alpha_{i+1}, \quad i = 1,\dots,K-1 \\
& \alpha_i a_i + \beta_i = \alpha_{i+1}a_i + \beta_{i+1}, \quad i = 1,\dots,K-1,
\end{aligned}
```

其中变量为 $\alpha \in \mathbf{R}^K$ 与 $\beta \in \mathbf{R}^K$，问题数据为 $x \in \mathbf{R}^m$、$y \in \mathbf{R}^m$，且

```math
F_{ij} = \begin{cases} 1 & a_{j-1} = x_i,\ j = 1 \\ 1 & a_{j-1} < x_i \le a_j \\ 0 & \text{otherwise} . \end{cases}
```

（这个 $F$ 是「$x_i$ 落在第 $j$ 段」的指示矩阵，所以第 $i$ 个拟合值就是 $\alpha_j x_i + \beta_j$，其中 $j$ 是 $x_i$ 所在的段。）

```matlab
% another approach for PWL fitting problem
clear all;
pwl_fit_data;
m = length(x);
xp = 0:0.001:1; % for fine-grained pwl function plot
mp = length(xp);
yp = [];
for K = 1:4 % internal knot 1,2,3
a = [0:1/K:1]'; % a_0,...,a_K
% matrix for sum f(x_i)
F = sparse(1:m,max(1,ceil(x*K)),1,m,K);
% solve problem
cvx_begin
variables alpha(K) beta(K)
minimize( norm(diag(x)*F*alpha+F*beta-y) )
subject to
if (K>=2)
alpha(1:K-1).*a(2:K)+beta(1:K-1) == alpha(2:K).*a(2:K)+beta(2:K)
a(1:K-1) <= a(2:K)
end
cvx_end
fp = sparse(1:mp,max(1,ceil(xp*K)),1,mp,K);
yp = [yp diag(xp)*fp*alpha+fp*beta];
end
plot(x,y,'b.',xp,yp);
```

（† 译注：上面代码中 `a(1:K-1) <= a(2:K)` 这一行是无效的——`a` 是固定的单调节点序列，这个不等式恒成立，写在这里当作约束只是 cvx 会接受并忽略的常数约束。它显然不是作者想写的 convexity 约束（convexity 应在 `alpha` 上）。第一段循环注释 `% internal knot 1,2,3` 也与 `K = 1:4` 不完全对应（$K$ 是段数，internal knot 数是 $K-1$）。这些是原文代码的瑕疵，不影响结果。）

## 附加题 4 · Robust least-squares with interval coefficient matrix（区间系数矩阵的 robust least-squares）

$\mathbf{R}^{m \times n}$ 中的 interval matrix（区间矩阵）是元素为区间的矩阵：

```math
\mathcal{A} = \{A \in \mathbf{R}^{m \times n} \mid |A_{ij} - \bar{A}_{ij}| \le R_{ij},\ i = 1,\dots,m,\ j = 1,\dots,n\} .
```

矩阵 $\bar{A} \in \mathbf{R}^{m \times n}$ 称为 nominal value 或 center value，$R \in \mathbf{R}^{m \times n}$（逐元素非负）称为 radius（半径）。

带 interval matrix 的 robust least-squares problem 为

```math
\text{minimize} \quad \sup_{A \in \mathcal{A}} \|Ax - b\|_2 ,
```

优化变量为 $x \in \mathbf{R}^n$。问题数据为 $\bar{A}$（即 $\mathcal{A}$ 与 $R$）以及 $b \in \mathbf{R}^m$。objective 作为 $x$ 的函数称为 worst-case residual norm（最坏情形残差范数）。robust least-squares problem 显然是一个 convex optimization problem。

**(a)** 把 interval matrix robust least-squares problem 表述为一个标准优化问题，例如 QP、SOCP 或 SDP。需要时可以引入新变量。你的改写中变量与约束的数目应当随 $m,n$ 线性增长，而**不是**指数增长。

**(b)** 考虑 $m = 4$、$n = 3$ 的具体实例：

```math
A = \begin{bmatrix}
60 \pm 0.05 & 45 \pm 0.05 & -8 \pm 0.05 \\
90 \pm 0.05 & 30 \pm 0.05 & -30 \pm 0.05 \\
0 \pm 0.05 & -8 \pm 0.05 & -4 \pm 0.05 \\
30 \pm 0.05 & 10 \pm 0.05 & -10 \pm 0.05
\end{bmatrix}, \quad
b = \begin{bmatrix} -6 \\ -3 \\ 18 \\ -9 \end{bmatrix} .
```

（$A$ 中每个条目的第一部分给出 $\bar{A}_{ij}$，第二部分给出 $R_{ij}$，这里全都是 $0.05$。）求 nominal problem（即 minimize $\|\bar{A}x - b\|_2$）的解 $x_{\mathrm{ls}}$，以及 robust least-squares 解 $x_{\mathrm{rls}}$。对这两个解，分别求 nominal residual norm 与 worst-case residual norm。确保结果说得通。

**Solution**：

**(a)** 问题等价于

```math
\text{minimize} \quad \sup_{A \in \mathcal{A}} \|Ax - b\|_2^2 ,
```

它可以改写为

```math
\begin{aligned}
\text{minimize} \quad & y^T y \\
\text{subject to} \quad & -y \preceq Ax - b \preceq y, \quad \text{for all} \ A \in \mathcal{A} .
\end{aligned}
```

我们有

```math
\sup_{A \in \mathcal{A}} (Ax - b)_i = \sum_{j=1}^n (\bar{A}_{ij} x_j + R_{ij}|x_j|) - b_i,
```

```math
\inf_{A \in \mathcal{A}} (Ax - b)_i = \sum_{j=1}^n (\bar{A}_{ij} x_j - R_{ij}|x_j|) - b_i .
```

（推导：固定 $x$，每一项 $\bar{A}_{ij}x_j + \Delta_{ij}x_j$ 在 $|\Delta_{ij}| \le R_{ij}$ 下要取最大，就取 $\Delta_{ij} = R_{ij}\mathrm{sign}(x_j)$，得 $\bar{A}_{ij}x_j + R_{ij}|x_j|$；取最小则取相反符号。**关键：$x_j$ 的符号决定了取哪个端点**，这正是 $|x_j|$ 出现的原因。）

因此可以把问题写成

```math
\begin{aligned}
\text{minimize} \quad & y^T y \\
\text{subject to} \quad & \bar{A}x + R|x| - b \preceq y \\
& \bar{A}x - R|x| - b \succeq -y,
\end{aligned}
```

其中 $|x| \in \mathbf{R}^n$ 是逐元素取绝对值的向量（$|x|_i = |x_i|$）；或者等价地写成 QP

```math
\begin{aligned}
\text{minimize} \quad & y^T y \\
\text{subject to} \quad & \bar{A}x + Rz - b \preceq y \\
& \bar{A}x - Rz - b \succeq -y \\
& -z \preceq x \preceq z .
\end{aligned}
```

变量为 $x \in \mathbf{R}^n$、$y \in \mathbf{R}^m$、$z \in \mathbf{R}^n$。

（$|x|$ 是 convex 但非线性的，用 $z \succeq |x|$（即 $-z \preceq x \preceq z$）松开会得到更大的可行域；由于 objective 关于 $y$ 单调，最优时会自动取 $z = |x|$，故等价。）

该问题还有另一种表述：诀窍是为 worst-case residual norm 作为 $x$ 的函数找一个替代表达式。令

```math
f(x) = \sup_{A \in \mathcal{A}} \|Ax - b\|_2^2 .
```

则

```math
\begin{aligned}
f(x) &= \sup\{ \|Ax - b\|_2^2 \mid A = \bar{A} + \Delta,\ |\Delta_{ij}| \le R_{ij},\ i = 1,\dots,m,\ j = 1,\dots,n \} \\
&= \sup\{ \|\bar{A}x + \Delta x - b\|_2^2 \mid |\Delta_{ij}| \le R_{ij},\ i = 1,\dots,m,\ j = 1,\dots,n \} \\
&= \sup\{ \|r + \Delta x\|_2^2 \mid |\Delta_{ij}| \le R_{ij},\ i = 1,\dots,m,\ j = 1,\dots,n \},
\end{aligned}
```

其中 $r = \bar{A}x - b$。

由于

```math
\|r + \Delta x\|_2^2 = \sum_{i=1}^m \Big( r_i + \sum_{j=1}^n \Delta_{ij} x_j \Big)^2 = \sum_{i=1}^m \Big| r_i + \sum_{j=1}^n \Delta_{ij} x_j \Big|^2,
```

容易看出该表达式在 $\Delta$ 的行之间是可分离的。因此为得到 $f(x)$ 的表达式，我们只需为

```math
\sup\left\{ \Big| r_i + \sum_{j=1}^n \Delta_{ij} x_j \Big| \ \Big| \ |\Delta_{ij}| \le R_{ij},\ j = 1,\dots,n \right\}
```

找表达式。该 supremum 由「$r_i \ge 0$ 时取 $\Delta_{ij} = R_{ij}\mathrm{sign}(x_j)$，$r_i < 0$ 时取 $\Delta_{ij} = -R_{ij}\mathrm{sign}(x_j)$」达到。supremum 等于

```math
|r_i| + \sum_{j=1}^n R_{ij}|x_j| .
```

所以

```math
f(x) = \sum_{i=1}^m \Big( |r_i| + \sum_{j=1}^n R_{ij}|x_j| \Big)^2 = \big\| |\bar{A}x - b| + R|x| \big\|_2^2 .
```

于是 robust least-squares problem 可以改写为

```math
\text{minimize} \quad \big\| |\bar{A}x - b| + R|x| \big\|_2 .
```

注意 objective function 是 convex 的，因为 Euclidean norm 是 convex 且在 $\mathbf{R}^m_+$ 上 increasing，而 $|\bar{A}x - b| + R|x|$ 是 convex 且非负的。

（这一步是 composition rule 的典型用法：外层范数 increasing 才能吃进内层的 convex 结果，且内层必须落在 $\mathbf{R}^m_+$ 中——$|\cdot|$ 与 $R|x|$ 保证了这点。）

**(b)** 下面的脚本计算 least-squares 解与 robust 解，并为每个解计算 nominal 与 worst-case residual norm。

```matlab
% input data
A_bar = [ 60 45 -8; ...
90 30 -30; ...
0 -8 -4; ...
30 10 -10];
d = .05;
R = d*ones(4,3);
b = [ -6; -3; 18; -9];
% least-squares solution
x_ls = A_bar\b;
% robust least-squares solution
cvx_begin
variables x(3) y(4) z(3)
minimize ( norm( y ) )
A_bar*x + R*z - b <= y
A_bar*x - R*z - b >= -y
x <= z
x + z >= 0
cvx_end
% computing nominal residual norms
nom_res_ls = norm(A_bar*x_ls - b);
nom_res_rls = norm(A_bar*x - b);
% computing worst-case nominal norms
r = A_bar*x_ls - b;
Delta = zeros(4,3);
for i=1:length(r)
if r(i) < 0
Delta(i,:) = -d*sign(x_ls');
else
Delta(i,:) = d*sign(x_ls');
end
end
wc_res_ls = norm(r + Delta*x_ls);
wc_res_rls = cvx_optval;
% display
disp('Residual norms for the nominal problem when using LS solution: ');
disp(nom_res_ls);
disp('Residual norms for the nominal problem when using robust solution: ');
disp(nom_res_rls);
disp('Residual norms for the worst-case problem when using LS solution: ');
disp(wc_res_ls);
disp('Residual norms for the worst-case problem when using robust solution: ');
disp(wc_res_rls);
```

robust least-squares 解也可以用下面的脚本求出：

```matlab
cvx_begin
variables x(3) t(4)
minimize ( norm ( t ) )
abs(A_bar*x - b) + R*abs(x) <= t
cvx_end
```

该脚本返回如下结果：

```
Residual norms for the nominal problem when using LS solution:
7.5895
Residual norms for the nominal problem when using robust solution:
17.7106
Residual norms for the worst-case problem when using LS solution:
26.7012
Residual norms for the worst-case problem when using robust solution:
17.7940
```

我们还出于好玩生成了下面的直方图，展示 $x = x_{\mathrm{ls}}$ 与 $x = x_{\mathrm{rls}}$ 两种情形下 residual norm 的分布。这些图是通过生成 $1000$ 个 $A$ 的实例、对每个 $A$ 与两个解分别计算 residual norm 得到的（$A$ 的元素在 $\bar{A}_{ij} - R_{ij}$ 与 $\bar{A}_{ij} + R_{ij}$ 之间均匀采样）。

（† 原文此处印作 "$x = x_{\mathrm{ks}}$"，应为 $x_{\mathrm{ls}}$，笔误。）

[Figure 4：$x = x_{\mathrm{ls}}$ 时 residual norm 的分布直方图——横轴 $0$ 到 $20$、纵轴频数 $0$ 到 $60$；分布很宽，峰值在 $6$–$8$ 附近（约 $55$ 次），长尾一直延伸到 $18$ 以上]

[Figure 5：$x = x_{\mathrm{rls}}$ 时 residual norm 的分布直方图——横轴 $17.64$ 到 $17.80$、纵轴频数 $0$ 到 $60$；分布极窄，几乎全部集中在 $17.73$–$17.76$ 这一段（这与 worst-case $17.7940$ 相当接近），说明 robust 解把「最坏情形」压成了一个几乎确定的水平]

下面的脚本生成这些直方图：

```matlab
% Monte-Carlo simulation
N = 1000;
res_ls = zeros(N,1);
res_rls = zeros(N,1);
for k=1:N
Delta = d*(2*rand(4,3)-1);
A = A_bar + Delta;
res_ls(k) = norm(A*x_ls - b);
res_rls(k) = norm(A*x - b);
end
figure;
hist(res_ls,50);
figure;
hist(res_rls,50);
```

（读图要点（译者补）：LS 解的 nominal residual 小（7.59）但 worst-case 很大（26.70），且在实际扰动下分布很散；robust 解牺牲 nominal（17.71）换取 worst-case 几乎不随扰动变化（分布宽度约 $0.05$）。这就是 robust optimization 的 trade-off 的直观图像。）

## 附加题 5 · Total variation image interpolation（total variation 图像插值）

灰度图像表示为 $m \times n$ 的强度矩阵 $U^{\mathrm{orig}}$。已知 $U^{\mathrm{orig}}_{ij}$ 在 $(i,j) \in K$ 上的取值，其中 $K \subset \{1,\dots,m\} \times \{1,\dots,n\}$。你的任务是插值整幅图像，即猜测缺失的值。重建图像表示为 $U \in \mathbf{R}^{m \times n}$，其中 $U$ 满足插值条件 $U_{ij} = U^{\mathrm{orig}}_{ij}$（$(i,j) \in K$）。

重建通过「在插值条件约束下极小化某个 roughness measure（粗糙度度量）」来得到。一种常见的 roughness measure 是（平方）$\ell_2$ variation：

```math
\sum_{i=2}^m \sum_{j=2}^n \big( (U_{ij} - U_{i-1,j})^2 + (U_{ij} - U_{i,j-1})^2 \big) .
```

另一种方法改为极小化 total variation（全变差）：

```math
\sum_{i=2}^m \sum_{j=2}^n \big( |U_{ij} - U_{i-1,j}| + |U_{ij} - U_{i,j-1}| \big) .
```

显然两种方法都导致 convex optimization problem。

对 `tv_img_interp.m` 中给出的数据实例分别做 $\ell_2$ 与 total variation 插值。该脚本会定义 $m$、$n$ 以及矩阵 `Uorig` 与 `Known`。`Known` 是 $m \times n$ 矩阵，若 $(i,j) \in K$ 则 $(i,j)$ 项为一，否则为零。该 m 文件还有画图的骨架代码。（我们给出整幅原图，以便你把重建结果与原图比较；显然你的解不能访问 $(i,j) \notin K$ 的 $U^{\mathrm{orig}}_{ij}$。）

† 译注：原文的求和范围 $i = 2,\dots,m$、$j = 2,\dots,n$ 只覆盖了内部的相邻差分，漏掉了第一行与第一列的差分（正确的全变差应遍历所有相邻像素对，即 $i = 1,\dots,m-1$ 与 $j = 1,\dots,n-1$ 之类的范围）。下面的代码用 `2:end` 与 `1:end-1`，与上面的公式一致，所以是「题面的范围写法」与「通常意义下的 TV」有出入，不影响本解的自洽性。

**Solution**。插值的代码非常简单。对 $\ell_2$ 插值，代码如下。

```matlab
cvx_begin
variable Ul2(m, n);
Ul2(Known) == Uorig(Known); % Fix known pixel values.
Ux = Ul2(2:end,2:end) - Ul2(2:end,1:end-1); % x (horiz) differences
Uy = Ul2(2:end,2:end) - Ul2(1:end-1,2:end); % y (vert) differences
minimize(norm([Ux(:); Uy(:)], 2)); % l2 roughness measure
cvx_end
```

对 total variation 插值，使用如下代码。

```matlab
cvx_begin
variable Utv(m, n);
Utv(Known) == Uorig(Known); % Fix known pixel values.
Ux = Utv(2:end,2:end) - Utv(2:end,1:end-1); % x (horiz) differences
Uy = Utv(2:end,2:end) - Utv(1:end-1,2:end); % y (vert) differences
minimize(norm([Ux(:); Uy(:)], 1)); % tv roughness measure
cvx_end
```

我们得到下面这些图像。

[Figure 6：四个 $50 \times 50$ 量级的灰度图像拼图，坐标轴标出 $10,20,30,40,50$。左上 "Original image"（原图，有清晰的条纹/方波图案）；右上 "Obscured image"（遮挡图，大部分像素被置为同一灰度，只见少量已知像素的形状）；左下 "$\ell_2$ reconstructed image"（$\ell_2$ 重建，边缘和条纹明显被「抹匀」、有振铃和模糊）；右下 "Total variation reconstructed image"（TV 重建，边缘锐利、条纹边界保持清晰，但平坦区呈块状）]

（读图要点（译者补）：$\ell_2$ 粗糙度惩罚的是差分的平方，因此对「大的跳变」惩罚极重，最优解倾向于把所有跳变抹平——图像变模糊；TV 惩罚差分的绝对值，对少量大跳变只线性惩罚，因此允许「边缘」存在而抹平「平坦区的小噪声」——这就是 TV 在去噪/插值里保边的机理。）

## 附加题 6 · Relaxed and discrete A-optimal experiment design（松弛与离散 A-optimal 实验设计）

本题讨论第 387 页描述的 A-optimal experiment design problem，数据生成如下。

```matlab
n = 5; % dimension of parameters to be estimated
p = 20; % number of available types of measurements
m = 30; % total number of measurements to be carried out
randn('state', 0);
V=randn(n,p); % columns are vi, the possible measurement vectors
```

求解 relaxed A-optimal experiment design problem

```math
\begin{aligned}
\text{minimize} \quad & (1/m)\, \mathbf{tr}\left( \sum_{i=1}^p \lambda_i v_i v_i^T \right)^{-1} \\
\text{subject to} \quad & \mathbf{1}^T \lambda = 1, \quad \lambda \succeq 0,
\end{aligned}
```

变量为 $\lambda \in \mathbf{R}^p$。求 optimal point $\lambda^\star$ 与 relaxed problem 相应的 optimal value。该 optimal value 是 discrete A-optimal experiment design problem

```math
\begin{aligned}
\text{minimize} \quad & \mathbf{tr}\left( \sum_{i=1}^p m_i v_i v_i^T \right)^{-1} \\
\text{subject to} \quad & m_1 + \cdots + m_p = m, \quad m_i \in \{0,\dots,m\},\ i = 1,\dots,p,
\end{aligned}
```

的 optimal value 的一个 lower bound，后者的变量为 $m_1,\dots,m_p$。为得到该离散问题的一个 suboptimal point，把 $m\lambda^\star$ 的各分量取整得到整数 $\hat{m}_i$。若需要，用手工或其它方法调整它们使其和为 $m$，并计算得到的 objective value。这当然是离散问题 optimal value 的一个 upper bound。给出该 upper bound 与 relaxed problem 所得 lower bound 之间的 gap。

注意两个 objective value 都可以解释为 mean-square estimation error（均方估计误差）$\mathbf{E}\|\hat{x} - x\|_2^2$。

**Solution**。relaxed problem 的 objective 是 convex 的，所以它是 convex problem。在 cvx 中表述它需要一点功夫。我们想写成

```matlab
minimize ((1/m)*trace(inv(V*diag(lambda)*V')))
```

但这不行，因为 cvx 不认识 matrix convex function（矩阵凸函数）。作为替代，我们可以把 objective 表达成 matrix fractional function 之和：

```math
\begin{aligned}
\text{minimize} \quad & (1/m) \sum_{k=1}^n e_k^T \left( \sum_{i=1}^p \lambda_i v_i v_i^T \right)^{-1} e_k \\
\text{subject to} \quad & \mathbf{1}^T \lambda = 1, \quad \lambda \succeq 0,
\end{aligned}
```

其中 $e_k \in \mathbf{R}^n$ 是第 $k$ 个单位向量。（注意在习题 6.9 中 $e$ 被定义为 estimation error vector，所以 $e_k$ 也可能指误差向量的第 $k$ 个分量。但在这里，显然 $e_k$ 是第 $k$ 个单位向量。）

（为什么可以这样拆：对任意正定矩阵 $M$，$\mathbf{tr}(M^{-1}) = \sum_k e_k^TM^{-1}e_k$。逐项写成 $e_k^T(\cdot)^{-1}e_k$ 后，每项都是 cvx 认识的 `matrix_frac` 形式，加起来仍是 convex。）

我们可以用 `matrix_frac` 函数在 cvx 中表达它。下面的代码求解该问题。

```matlab
n = 5; % dimension
p = 20; % number of available types of measurements
m = 30; % total number of measurements to be carried out
randn('state', 0);
V=randn(n,p); % columns are vi, the possible measurement vectors
cvx_begin
variable lambda(p)
obj = 0;
for k=1:n
ek = zeros(n,1);
ek(k)=1;
obj = obj + (1/m)*matrix_frac(ek,V*diag(lambda)*V');
end
minimize( obj )
subject to
sum(lambda) == 1
lambda >= 0
cvx_end
lower_bound = cvx_optval
t = -0.00; % small offset chosen by hand to make rounding work out.
% for this problem data, none is needed!
m_rnd = pos(round(m*lambda+t));
sum(m_rnd) % should be == m
% now find objective value of rounded experiment design
upper_bound = trace(inv(V*diag(m_rnd/m)*V'))/m
gap = upper_bound-lower_bound
rel_gap = gap/lower_bound
```

对这个实例，简单取整得到的 $\hat{m}_i$ 之和恰好为 $m = 30$，因此不需调整取整后的值。lower bound 为 $0.2481$，upper bound 为 $0.2483$。gap 为 $0.00023$，约 $0.1\%$。

这意味着：我们找到了 30 次测量的一个选择（每次从 20 种可能测量中挑一种），它给出 mean-square estimation error $\mathbf{E}\|\hat{x} - x\|_2^2 = 0.2483$。我们不知道这是否就是 30 次测量的最优选择。但我们确实知道：这个选择是次优不超过 $0.1\%$ 的；最优选择能达到的 mean-square error 不小于 $0.2481$。我们的实验设计即使不是最优的，也非常接近最优。（事实上它很可能就是最优的。）

（读法：这是一个「先松后圆」的经典套路——relaxed problem 给出 $L = 0.2481$，把分数 $\lambda^\star$ 直接乘 $m$ 再取整得到整数方案，其目标值 $U = 0.2483$ 是离散问题的可行解；由于 $L \le p^\star \le U$，相对 gap $0.1\%$ 就把离散问题的最优值夹住了。注意 `m_rnd/m` 是归一化后的权重，恰好是离散方案对应的 $\lambda$。）
