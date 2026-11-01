# EE364a HW6 解答（作业 6 解答）

> 本文件为 Stanford EE364a（Convex Optimization，Prof. S. Boyd，Winter 2007-08）Homework 6 官方解答的**行内双语译注**。
> 约定：中文为主体，英文术语以 `English（中文）` 形式在首次出现时标注，之后只保留 `English`。
> 原文 PDF 公式有损，均已按上下文还原为标准 LaTeX；OCR 模糊处以 `〔…待核〕` 标出，不臆造。

---

## 题目 1（Problem 1）

**题干：** 6.9 Minimax rational function fitting. Show that the following problem is quasiconvex:

```math
\text{minimize}\quad \max_{i=1,\dots,k}\left|\frac{p(t_i)}{q(t_i)}-y_i\right|
```

where

```math
p(t)=a_0+a_1t+a_2t^2+\cdots+a_mt^m,\qquad
q(t)=1+b_1t+\cdots+b_nt^n,
```

and the domain of the objective function is defined as

```math
\mathcal{D}=\{(a,b)\in\mathbb{R}^{m+1}\times\mathbb{R}^{n}\mid q(t)>0,\ \alpha\le t\le\beta\}.
```

**中文：** 6.9 极小极大有理函数拟合（minimax rational function fitting）。证明下列问题是拟凸的（quasiconvex）：

```math
\text{minimize}\quad \max_{i=1,\dots,k}\left|\frac{p(t_i)}{q(t_i)}-y_i\right|
```

其中

```math
p(t)=a_0+a_1t+a_2t^2+\cdots+a_mt^m,\qquad
q(t)=1+b_1t+\cdots+b_nt^n,
```

且目标函数（objective function）的定义域（domain）为

```math
\mathcal{D}=\{(a,b)\in\mathbb{R}^{m+1}\times\mathbb{R}^{n}\mid q(t)>0,\ \alpha\le t\le\beta\}.
```

本题是把一个有理函数（rational function）$p(t)/q(t)$ 拟合到给定数据，同时约束分母多项式（denominator polynomial）在区间 $[\alpha,\beta]$ 上为正。优化变量（optimization variables）是分子、分母系数 $a_i,b_i$。插值点（interpolation points）$t_i\in[\alpha,\beta]$ 以及期望的函数值 $y_i,\ i=1,\dots,k$ 是已知给定的。

**解答：** 下面证明目标函数是拟凸的。它的定义域是凸的（convex）。由于 $q(t_i)>0$ 对 $i=1,\dots,k$ 成立，我们有

```math
\max_{i=1,\dots,k}\left|p(t_i)/q(t_i)-y_i\right|\le\gamma
```

当且仅当（if and only if）

```math
|p(t_i)-y_i q(t_i)|\le\gamma\,q(t_i),\qquad i=1,\dots,k.
```

> **关键一步（为什么能去分母）：** 因为分母 $q(t_i)>0$，所以不等式两边同乘正数 $q(t_i)$ 不改变不等号方向：
> ```math
\left|\frac{p(t_i)}{q(t_i)}-y_i\right|=\frac{|p(t_i)-y_i q(t_i)|}{q(t_i)}\le\gamma \iff |p(t_i)-y_i q(t_i)|\le\gamma q(t_i).
> ```
> 注意左边 $|p(t_i)-y_i q(t_i)|$ 关于变量 $(a,b)$ 是凸的（convex，绝对值函数是凸函数，内部 $p(t_i)-y_i q(t_i)$ 是 $(a,b)$ 的仿射函数 affine function），右边 $\gamma q(t_i)$ 关于 $(a,b)$ 是线性的（linear/affine）。因此对每个固定的 $\gamma$，这 $k$ 组不等式定义了一个关于 $(a,b)$ 的凸集。

进一步，可以把这些不等式写成一组 $2k$ 个线性不等式（linear inequalities）：

```math
-\gamma q(t_i)\le p(t_i)-y_i q(t_i)\le\gamma q(t_i),\qquad i=1,\dots,k.
```

```math
-\gamma q(t_i)\le p(t_i)-y_i q(t_i)\le\gamma q(t_i),\qquad i=1,\dots,k.
```
**拟凸性判据（显式写出）：** 函数 $f$ 拟凸的定义是：其所有子水平集（sublevel set）
```math
\{x\mid f(x)\le\gamma\}
```
对任意 $\gamma$ 都是凸集。上面我们已经证明：$\{(a,b)\mid \max_i|p(t_i)/q(t_i)-y_i|\le\gamma\}$ 恰好等价于 $2k$ 个线性不等式（线性不等式定义的是半空间 halfspace，半空间是凸集，有限个凸集的交仍是凸集），故该子水平集是凸集，从而目标函数是拟凸的。证毕。

---

## 题目 2（Problem 2）——附加练习 1：指数函数的极小极大有理拟合

**题干：** Minimax rational fit to the exponential. (See exercise 6.9.) We consider the specific problem instance with data
```math
t_i=-3+6(i-1)/(k-1),\qquad y_i=e^{t_i},\qquad i=1,\dots,k,
```
where $k=201$. (In other words, the data are obtained by uniformly sampling the exponential function over the interval $[-3,3]$.) Find a function of the form
```math
f(t)=\frac{a_0+a_1t+a_2t^2}{1+b_1t+b_2t^2}
```
that minimizes $\max_{i=1,\dots,k}|f(t_i)-y_i|$. (We require that $1+b_1t_i+b_2t_i^2>0$ for $i=1,\dots,k$.) Find optimal values of $a_0,a_1,a_2,b_1,b_2$, and give the optimal objective value, computed to an accuracy of 0.001. Plot the data and the optimal rational function fit on the same plot. On a different plot, give the fitting error, i.e., $f(t_i)-y_i$.

**中文：** 对指数函数的极小极大有理拟合（minimax rational fit to the exponential）。考虑如下的具体问题实例，数据为

```math
t_i=-3+6(i-1)/(k-1),\qquad y_i=e^{t_i},\qquad i=1,\dots,k,
```

其中 $k=201$（也就是说，数据是在区间 $[-3,3]$ 上对指数函数做均匀采样 uniformly sampling 得到的）。寻找一个形如

```math
f(t)=\frac{a_0+a_1t+a_2t^2}{1+b_1t+b_2t^2}
```

的函数，使 $\max_{i=1,\dots,k}|f(t_i)-y_i|$ 最小（minimize）。要求 $1+b_1t_i+b_2t_i^2>0$ 对 $i=1,\dots,k$ 成立。求出 $a_0,a_1,a_2,b_1,b_2$ 的最优值，并给出精确到 0.001 的最优目标值（optimal objective value）。在同一张图上画出数据和最优有理拟合函数；在另一张图上画出拟合误差（fitting error），即 $f(t_i)-y_i$。

**提示（Hint）：** 在 `cvx_end` 之后，可以用 `strcmp(cvx_status,'Solved')` 来判断一个可行性问题（feasibility problem）是否可行。

**解答：** 目标函数（因而整个问题）不是凸的，但它是拟凸的（quasiconvex）。我们有 $\max_{i=1,\dots,k}|f(t_i)-y_i|\le\gamma$ 当且仅当
```math
\left|\frac{a_0+a_1t_i+a_2t_i^2}{1+b_1t_i+b_2t_i^2}-y_i\right|\le\gamma,\qquad i=1,\dots,k.
```
由于分母为正，上式等价于（equivalent to）
```math
|a_0+a_1t_i+a_2t_i^2-y_i(1+b_1t_i+b_2t_i^2)|\le\gamma(1+b_1t_i+b_2t_i^2),\qquad i=1,\dots,k.
```math
|a_0+a_1t_i+a_2t_i^2-y_i(1+b_1t_i+b_2t_i^2)|\le\gamma(1+b_1t_i+b_2t_i^2),\qquad i=1,\dots,k.
```

（原文 OCR 此处有一处 $a_0+a_1t+a_2t_i^2$ 的 $t$ 下标疑似缺失，按上下文应统一为 $t_i$。待核。）

这是关于变量 $a$、$b$ 的一组 $2k$ 个线性不等式（对固定的 $\gamma$ 而言）。特别地，这说明了目标函数是拟凸的（事实上它是一个广义线性分式函数 generalized linear fractional function）。

为求解该问题，采用二分法（bisection method），每一步求解一个 LP 可行性问题（LP feasibility problem）。每一步选定一个 $\gamma$ 值，求解如下可行性问题：

```math
\begin{aligned}
\text{find}\quad &a,b\\
\text{subject to}\quad &|a_0+a_1t_i+a_2t_i^2-y_i(1+b_1t_i+b_2t_i^2)|\le\gamma(1+b_1t_i+b_2t_i^2),\quad i=1,\dots,k,
\end{aligned}
```

变量为 $a$、$b$。（注意：只要 $\gamma>0$，分母为正这一条件就自动被满足——因为若分母 $1+b_1t_i+b_2t_i^2\le 0$，则右边 $\gamma(\text{分母})\le0$，而左边是绝对值 $\ge0$，两者无法同时成立，故可行性要求强制了分母为正。）上式可以转化为如下的 LP 可行性问题：

```math
\begin{aligned}
\text{find}\quad &a,b\\
\text{subject to}\quad
&a_0+a_1t_i+a_2t_i^2-y_i(1+b_1t_i+b_2t_i^2)\le\gamma(1+b_1t_i+b_2t_i^2),\quad i=1,\dots,k\\
&a_0+a_1t_i+a_2t_i^2-y_i(1+b_1t_i+b_2t_i^2)\ge-\gamma(1+b_1t_i+b_2t_i^2),\quad i=1,\dots,k.
\end{aligned}
```

下面的 Matlab 代码求解这个具体问题实例：

```matlab
k=201;
t=(-3:6/(k-1):3)';
y=exp(t);
Tpowers=[ones(k,1) t t.^2];
u=exp(3); l=0;            % initial upper and lower bounds  初始上下界
bisection_tol=1e-3;       % bisection tolerance             二分容差
while u-l>= bisection_tol
    gamma=(l+u)/2;
    cvx_begin             % solve the feasibility problem   求解可行性问题
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

> **二分法原理：** 目标函数是拟凸的，故 $f(x)\le\gamma$ 的可行性关于 $\gamma$ 单调（若 $\gamma_1\le\gamma_2$，则 $f\le\gamma_1$ 可行推出 $f\le\gamma_2$ 可行）。因此可用二分：取中点 $\gamma=(l+u)/2$ 求解 LP 可行性问题，可行则收缩上界 $u=\gamma$，否则收缩下界 $l=\gamma$，直到区间长度小于容差 $10^{-3}$。

最优值（optimal values）为
```math
a_0=1.0099,\quad a_1=0.6117,\quad a_2=0.1134,\quad b_1=-0.4147,\quad b_2=0.0485,
```
最优目标值（optimal objective value）为 $0.0233$。

[[FIGURE: 第 4 页, Figure 1：Chebyshev 有理拟合。实线为数据（指数函数采样），十字叉为拟合点。横轴 $t\in[-3,3]$，纵轴 $y\in[0,25]$ 左右。]]

[[FIGURE: 第 5 页, Figure 2：Chebyshev 指数有理拟合的拟合误差 $f(t_i)-y_i$，纵轴范围约 $[-0.025,0.025]$。]]

---

## 题目 3（Problem 3）——附加练习 2：团队实力的极大似然预测

**题干：** Maximum likelihood prediction of team ability. A set of $n$ teams compete in a tournament. We model each team's ability by a number $a_j\in[0,1]$, $j=1,\dots,n$. When teams $j$ and $k$ play each other, the probability that team $j$ wins is equal to $\operatorname{prob}(a_j-a_k+v>0)$, where $v\sim\mathcal{N}(0,\sigma^2)$. You are given the outcome of $m$ past games, organized as $(j(i),k(i),y(i))$, $i=1,\dots,m$, meaning that game $i$ was played between teams $j(i)$ and $k(i)$; $y(i)=1$ means team $j(i)$ won, while $y(i)=-1$ means team $k(i)$ won. (We assume there are no ties.)

(a) Formulate the problem of finding the maximum likelihood estimate of team abilities, $\hat a\in\mathbb{R}^n$, given the outcomes, as a convex optimization problem. You will find the game incidence matrix $A\in\mathbb{R}^{m\times n}$, defined as
```math
A_{il}=
\begin{cases}
y(i) & l=j(i)\\
-y(i) & l=k(i)\\
0 & \text{otherwise},
\end{cases}
```
useful. The prior constraints $\hat a_i\in[0,1]$ should be included in the problem formulation.

**中文：** 团队实力的极大似然预测（maximum likelihood prediction of team ability）。$n$ 支队伍参加锦标赛（tournament）。我们用数 $a_j\in[0,1]$（$j=1,\dots,n$）来刻画每支队伍的实力（ability）。当队伍 $j$ 与 $k$ 交手时，$j$ 获胜的概率等于 $\operatorname{prob}(a_j-a_k+v>0)$，其中 $v\sim\mathcal{N}(0,\sigma^2)$。给定过去 $m$ 场比赛的结果（outcome），组织成三元组 $(j(i),k(i),y(i))$（$i=1,\dots,m$），含义是第 $i$ 场由队伍 $j(i)$ 与 $k(i)$ 进行；$y(i)=1$ 表示 $j(i)$ 胜，$y(i)=-1$ 表示 $k(i)$ 胜（假设没有平局 ties）。

(a) 把「给定结果求队伍实力极大似然估计（maximum likelihood estimate）$\hat a\in\mathbb{R}^n$」的问题表述为一个凸优化问题（convex optimization problem）。你会发现比赛关联矩阵（game incidence matrix）$A\in\mathbb{R}^{m\times n}$（如上定义）很有用。先验约束（prior constraints）$\hat a_i\in[0,1]$ 应包含在问题表述中。

同时注意：若给所有队伍实力都加上同一个常数，比赛结果的概率不变。这意味着 $\hat a$ 只确定到相差一个常数（像一个势函数 potential 一样）。但这不影响 ML 估计问题，也不影响用估计参数所做的任何后续预测。

(b) 用 `team_data.m` 中矩阵 `train` 给出的队伍数据求 $\hat a$。（该矩阵给出的是「每两支队伍各赛一场」的锦标赛结果。）CVX 不支持凹函数（concave function）$\log\Phi$（$\Phi$ 是单位高斯的累积分布函数 cumulative distribution），但课程网站上提供了足够好的近似 `log_normcdf`，该函数已重载为支持向量输入（逐元素 elementwise）。可以用如下命令构造 $A$：

```matlab
A = sparse(1:m,train(:,1),train(:,3),m,n) + ...
    sparse(1:m,train(:,2),-train(:,3),m,n);
```

(c) 用 (b) 中得到的极大似然估计 $\hat a$ 来预测矩阵 `test` 中明年锦标赛的结果，用 $\hat y(i)=\operatorname{sign}(\hat a_{j(i)}-\hat a_{k(i)})$。把这些预测与 `test` 第三列的实际结果比较，给出正确预测的比例（fraction of correctly predicted outcomes）。`train` 与 `test` 中的比赛是相同的，所以另一种更简单的方法是直接假设去年赢的队伍今年也会赢。给出用这种简单方法的正确预测百分比。

**解答：**

**(a)** 给定 $a$ 时结果 $y$ 的似然（likelihood）为
```math
p(y\mid a)=\prod_{i=1,\dots,m}\Phi\!\left(\frac{1}{\sigma}y(i)\,(a_{j(i)}-a_{k(i)})\right),
```
其中 $\Phi$ 是标准正态的累积分布函数（cumulative distribution of the standard normal）。

> **推导（为什么是 $\Phi$）：** 队伍 $j$ 获胜当且仅当 $a_j-a_k+v>0$，即 $v>-(a_j-a_k)$。由于 $v\sim\mathcal{N}(0,\sigma^2)$，有
>
```math
\Pr(a_j-a_k+v>0)=\Pr\!\left(\frac{v}{\sigma}>\frac{-(a_j-a_k)}{\sigma}\right)=\Phi\!\left(\frac{a_j-a_k}{\sigma}\right).
```
> 而 $y(i)=\pm1$ 的符号用来统一两种情况：当 $y(i)=1$（$j$ 胜）时对应 $\Phi((a_j-a_k)/\sigma)$；当 $y(i)=-1$（$k$ 胜）时对应 $1-\Phi((a_j-a_k)/\sigma)=\Phi((a_k-a_j)/\sigma)$，两者统一为 $\Phi(y(i)(a_{j(i)}-a_{k(i)})/\sigma)$。又由矩阵 $A$ 的定义，$y(i)(a_{j(i)}-a_{k(i)})=(Aa)_i$，故似然可写成 $\prod_i\Phi((Aa)_i/\sigma)$。

对数似然函数（log-likelihood function）因此为
```math
l(a)=\log p(y\mid a)=\sum_{i}\log\Phi\!\left(\frac{1}{\sigma}(Aa)_i\right).
```math
l(a)=\log p(y\mid a)=\sum_{i}\log\Phi\left(\frac{1}{\sigma}(Aa)_i\right).
```

这是一个凹函数（concave function）。极大似然估计 $\hat a$ 是下列问题的任意解：

```math
\begin{aligned}
\text{maximize}\quad &l(a)\\
\text{subject to}\quad &0\preceq a\preceq 1.
\end{aligned}
```

这是一个凸优化问题：因为被极大化的目标函数是凹的，而约束是 $2n$ 个线性不等式。

> **凸性说明（显式）：** $\log\Phi$ 是凹函数（标准正态 CDF 是 log-concave 的），$x\mapsto\log\Phi((Ax)_i/\sigma)$ 是凹函数与仿射函数的复合，仍为凹函数；凹函数之和是凹函数，故 $l(a)$ 凹。极大化凹函数 + 盒约束（box constraints，$2n$ 个线性不等式）即标准凸问题。

**(b)** 下面的代码求解该问题：

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

用此代码得到 $\hat a=(1.0,\,0.0,\,0.68,\,0.37,\,0.79,\,0.58,\,0.38,\,0.09,\,0.67,\,0.58)$。

**(c)** 下面的代码用来预测 `test` 集的结果：

```matlab
% Estimate errors in test set
res = sign(a_hat(test(:,1))-a_hat(test(:,2)));
Pml = 1-length(find(res-test(:,3)))/m_test
Ply = 1-length(find(train(:,3)-test(:,3)))/m_test
```

极大似然估计正确预测了 `test` 中 $86.7\%$ 的比赛；另一方面，`test` 中有 $75.6\%$ 的比赛结果与 `train` 相同。

---

## 题目 4（Problem 4）——附加练习 3：分段线性拟合

**题干：** Piecewise-linear fitting. In many applications some function in the model is not given by a formula, but instead as tabulated data. The tabulated data could come from empirical measurements, historical data, numerically evaluating some complex expression or solving some problem, for a set of values of the argument. For use in a convex optimization model, we then have to fit these data with a convex function that is compatible with the solver or other system that we use.

Suppose we are given the data $(x_i,y_i)$, $i=1,\dots,m$, with $x_i,y_i\in\mathbb{R}$. We will assume that $x_i$ are sorted, i.e., $x_1<x_2<\cdots<x_m$. Let $a_0<a_1<\cdots<a_K$ be a set of fixed knot points, with $a_0\le x_1$ and $a_K\ge x_m$. Explain how to find the convex piecewise linear function $f$, defined over $[a_0,a_K]$, with knot points $a_i$, that minimizes the least-squares fitting criterion $\sum_{i=1}^m(f(x_i)-y_i)^2$. You must explain what the variables are and how they parametrize $f$, and how you ensure convexity of $f$.

**中文：** 分段线性拟合（piecewise-linear fitting）。在许多应用中，模型中的某个函数不是由公式给出，而是以表格数据（tabulated data）的形式给出。这些表格数据可能来自经验测量、历史数据、对某个复杂表达式的数值求值，或对某组自变量取值求解某个问题。为了在凸优化模型中使用，我们必须用一个与所用求解器（solver）或其他系统兼容的凸函数来拟合这些数据。

假设给定数据 $(x_i,y_i)$，$i=1,\dots,m$，$x_i,y_i\in\mathbb{R}$。假设 $x_i$ 已排序，即 $x_1<x_2<\cdots<x_m$。设 $a_0<a_1<\cdots<a_K$ 是一组固定的结点（knot points），满足 $a_0\le x_1$、$a_K\ge x_m$。解释如何求定义在 $[a_0,a_K]$ 上、以 $a_i$ 为结点、使最小二乘拟合准则（least-squares fitting criterion）$\sum_{i=1}^m(f(x_i)-y_i)^2$ 最小的凸分段线性函数 $f$。你必须说明变量是什么、它们如何参数化 $f$，以及如何保证 $f$ 的凸性。

**提示（Hints）：** 一种方法基于 Lagrange 基（Lagrange basis）$f_0,\dots,f_K$，它们是满足 $f_j(a_i)=\delta_{ij}$（$i,j=0,\dots,K$）的分段线性函数。另一种方法基于在 $x\in(a_{i-1},a_i]$ 上定义 $f(x)=\alpha_i x+\beta_i$，然后需对参数 $\alpha_i,\beta_i$ 加上条件以保证 $f$ 连续（continuous）且凸。

把方法应用于文件 `pwl_fit_data.m` 中的数据（含 $x_j\in[0,1]$）。求最佳仿射拟合（affine fit，对应 $a=(0,1)$），以及内部结点数为 1、2、3（在 $[0,1]$ 上等距）时的最佳分段线性凸函数拟合。给出每种情形的拟合代价（fitting cost）。画出数据和分段线性拟合。把每个函数表示成
```math
f(x)=\max_{i=1,\dots,K}(\alpha_i x+\beta_i)
```
的形式（这种形式便于把函数并入优化问题）。

**解答：** 按提示，采用 Lagrange 基函数 $f_0,\dots,f_K$。它们可表示为
```math
f_0(x)=\left(\frac{a_1-x}{a_1-a_0}\right)_+,
```

```math
f_i(x)=\left(\min\left\{\frac{x-a_{i-1}}{a_i-a_{i-1}},\frac{a_{i+1}-x}{a_{i+1}-a_i}\right\}\right)_+,\qquad i=1,\dots,K-1,
```
以及
```math
f_K(x)=\left(\frac{x-a_{K-1}}{a_K-a_{K-1}}\right)_+.
```
其中 $(\cdot)_+=\max\{0,\cdot\}$ 表示正部（positive part）。

> **Lagrange 基的性质：** 每个 $f_i$ 是以 $a_i$ 为「峰」的三角帐篷函数（tent function），满足 $f_i(a_j)=\delta_{ij}$。任意以 $a_i$ 为结点的分段线性函数都可唯一写成这些基函数的线性组合。

函数 $f$ 可参数化为
```math
f(x)=\sum_{i=0}^{K}z_i f_i(x),
```
其中 $z_i=f(a_i)$（$i=0,\dots,K$）。用 $z=(z_0,\dots,z_K)$ 参数化 $f$。最小二乘拟合准则于是为
```math
J=\sum_{i=1}^{m}(f(x_i)-y_i)^2=\|Fz-y\|_2^2,
```math
J=\sum_{i=1}^{m}(f(x_i)-y_i)^2=\|Fz-y\|_2^2,
```

其中 $F\in\mathbb{R}^{m\times(K+1)}$ 是元素为 $F_{ij}=f_j(x_i)$（$i=1,\dots,m$，$j=0,\dots,K$）的矩阵（这里 $F$ 的列下标从 0 到 $K$）。

还需加上 $f$ 凸的约束。这等价于各段斜率（slope）非递减（nondecreasing）的条件，即
```math
\frac{z_{i+1}-z_i}{a_{i+1}-a_i}\ge\frac{z_i-z_{i-1}}{a_i-a_{i-1}},\qquad i=1,\dots,K-1.
```math
\frac{z_{i+1}-z_i}{a_{i+1}-a_i}\ge\frac{z_i-z_{i-1}}{a_i-a_{i-1}},\qquad i=1,\dots,K-1.
```

> **凸性判据（显式）：** 一个分段线性函数是凸的，当且仅当其各段斜率单调非递减。第 $i$ 段（$[a_{i-1},a_i]$）的斜率为 $(z_i-z_{i-1})/(a_i-a_{i-1})$，第 $i+1$ 段斜率为 $(z_{i+1}-z_i)/(a_{i+1}-a_i)$，要求前者 $\le$ 后者即得上式。这是一组关于 $z$ 的线性不等式。

因此，最佳分段线性凸拟合可通过求解如下 QP 得到：

```math
\begin{aligned}
\text{minimize}\quad &\|Fz-y\|_2^2\\
\text{subject to}\quad &\frac{z_{i+1}-z_i}{a_{i+1}-a_i}\ge\frac{z_i-z_{i-1}}{a_i-a_{i-1}},\quad i=1,\dots,K-1.
\end{aligned}
```

下面的代码对 `pwl_fit_data` 中的数据求解该问题：

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
```

```matlab
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
(z(3:end)-z(2:end-1))./(a(3:end)-a(2:end-1)) >= ...
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

[[FIGURE: 第 10 页, Figure 3：$K=1,2,3,4$ 时的分段线性逼近。横轴 $x\in[0,1]$，纵轴 $y\in[-1,2]$ 左右。可见随 $K$ 增大逼近改善。]]

下表给出该逼近的结果（$J$ 为最小二乘代价）：

| $K$ | $\alpha_1,\dots,\alpha_K$ | $\beta_1,\dots,\beta_K$ | $J$ |
|-----|---------------------------|--------------------------|-----|
| 1 | 1.91 | −0.87 | 12.73 |
| 2 | −0.27, 4.09 | −0.33, −2.51 | 2.62 |
| 3 | −1.80, 2.67, 4.25 | −0.10, −1.59, −2.65 | 0.60 |
| 4 | −3.15, 2.11, 2.68, 4.90 | 0.03, −1.29, −1.57, −3.23 | 0.22 |

还有另一种方法求解此问题。我们要求一个分段线性函数。若至少有一个内部结点（$K\ge2$），函数应满足如下两个约束：

- **凸性（convexity）：** $\alpha_1\le\alpha_2\le\cdots\le\alpha_K$
- **连续性（continuity）：** $\alpha_i a_i+\beta_i=\alpha_{i+1}a_i+\beta_{i+1}$，$i=1,\dots,K-1$

因此优化问题为

```math
\begin{aligned}
\text{minimize}\quad &\sum_{i=1}^{m}\big(f(x_i)-y_i\big)^2\\
\text{subject to}\quad &\alpha_i\le\alpha_{i+1},\quad i=1,\dots,K-1\\
&\alpha_i a_i+\beta_i=\alpha_{i+1}a_i+\beta_{i+1},\quad i=1,\dots,K-1.
\end{aligned}
```

把 $f(x_i)$ 写成矩阵形式重新表述，得到

```math
\begin{aligned}
\text{minimize}\quad &\|\operatorname{diag}(x)F\alpha+F\beta-y\|_2\\
\text{subject to}\quad &\alpha_i\le\alpha_{i+1},\quad i=1,\dots,K-1\\
&\alpha_i a_i+\beta_i=\alpha_{i+1}a_i+\beta_{i+1},\quad i=1,\dots,K-1,
\end{aligned}
```

其中变量是 $\alpha\in\mathbb{R}^K$ 和 $\beta\in\mathbb{R}^K$，问题数据是 $x\in\mathbb{R}^m$、$y\in\mathbb{R}^m$，以及
```math
F_{ij}=
\begin{cases}
1 & \text{if } a_{j-1}=x_i,\ j=1\\
1 & \text{if } a_{j-1}<x_i\le a_j\\
0 & \text{otherwise}.
\end{cases}
```
```matlab
% another approach for PWL fitting problem
clear all;
pwl_fit_data;
m = length(x);
xp = 0:0.001:1;   % for fine-grained pwl function plot
mp = length(xp);
yp = [];
for K = 1:4       % internal knot 1,2,3
    a = [0:1/K:1]';   % a_0,...,a_K
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

> 注：原文代码中 `a(1:K-1) <= a(2:K)` 这一行按题意应为约束 $\alpha_i\le\alpha_{i+1}$，即 `alpha(1:K-1) <= alpha(2:K)`。原文此处写成了 `a`（结点），疑为 OCR/原书笔误，按上下文应为 `alpha`。待核。

---

## 题目 5（Problem 5）——附加练习 4：区间系数矩阵的鲁棒最小二乘

**题干：** Robust least-squares with interval coefficient matrix. An interval matrix in $\mathbb{R}^{m\times n}$ is a matrix whose entries are intervals:
```math
\mathcal{A}=\{A\in\mathbb{R}^{m\times n}\mid |A_{ij}-\bar A_{ij}|\le R_{ij},\ i=1,\dots,m,\ j=1,\dots,n\}.
```
The matrix $\bar A\in\mathbb{R}^{m\times n}$ is called the nominal value or center value, and $R\in\mathbb{R}^{m\times n}$, which is elementwise nonnegative, is called the radius.

The robust least-squares problem, with interval matrix, is
```math
\text{minimize}\quad \sup_{A\in\mathcal{A}}\|Ax-b\|_2,
```
with optimization variable $x\in\mathbb{R}^n$. The problem data are $\mathcal{A}$ (i.e., $\bar A$ and $R$) and $b\in\mathbb{R}^m$. The objective, as a function of $x$, is called the worst-case residual norm.

(a) Formulate the interval matrix robust least-squares problem as a standard optimization problem, e.g., a QP, SOCP, or SDP. You can introduce new variables if needed. Your reformulation should have a number of variables and constraints that grows linearly with $m$ and $n$, and not exponentially.

(b) Consider the specific problem instance with $m=4$, $n=3$, and the matrices given below. Find the solution $x_{ls}$ of the nominal problem (i.e., minimize $\|\bar Ax-b\|_2$), and robust least-squares solution $x_{rls}$. For each of these, find the nominal residual norm, and also the worst-case residual norm. Make sure the results make sense.

**中文：** 区间系数矩阵的鲁棒最小二乘（robust least-squares with interval coefficient matrix）。$\mathbb{R}^{m\times n}$ 中的区间矩阵（interval matrix）是元素为区间（intervals）的矩阵：

```math
\mathcal{A}=\{A\in\mathbb{R}^{m\times n}\mid |A_{ij}-\bar A_{ij}|\le R_{ij},\ i=1,\dots,m,\ j=1,\dots,n\}.
```

矩阵 $\bar A\in\mathbb{R}^{m\times n}$ 称为标称值（nominal value）或中心值（center value），元素非负的 $R\in\mathbb{R}^{m\times n}$ 称为半径（radius）。

带区间矩阵的鲁棒最小二乘问题为

```math
\text{minimize}\quad \sup_{A\in\mathcal{A}}\|Ax-b\|_2,
```

优化变量为 $x\in\mathbb{R}^n$。问题数据是 $\mathcal{A}$（即 $\bar A$ 与 $R$）以及 $b\in\mathbb{R}^m$。该目标（作为 $x$ 的函数）称为最坏情形残差范数（worst-case residual norm）。

(a) 把区间矩阵鲁棒最小二乘问题表述为一个标准优化问题，例如 QP、SOCP 或 SDP。需要时可引入新变量。你的重表述中变量和约束的数量应随 $m$ 和 $n$ 线性增长，而非指数增长。

(b) 考虑 $m=4$、$n=3$ 的具体问题实例，矩阵如下（每个元素前半部分是 $\bar A_{ij}$，后半部分是 $R_{ij}$，这里均为 0.05）：
```math
A=
\begin{bmatrix}
60\pm0.05 & 45\pm0.05 & -8\pm0.05\\
90\pm0.05 & 30\pm0.05 & -30\pm0.05\\
0\pm0.05 & -8\pm0.05 & -4\pm0.05\\
30\pm0.05 & 10\pm0.05 & -10\pm0.05
\end{bmatrix},
\qquad
b=
\begin{bmatrix}
-6\\-3\\18\\-9
\end{bmatrix}.
```
求标称问题（即 $\min_x\|\bar Ax-b\|_2$）的解 $x_{ls}$，以及鲁棒最小二乘解 $x_{rls}$。对每一个解，求标称残差范数（nominal residual norm）以及最坏情形残差范数（worst-case residual norm）。确保结果合理。

**解答：**

**(a)** 该问题等价于

```math
\text{minimize}\quad \sup_{A\in\mathcal{A}}\|Ax-b\|_2^2,
```

可重表述为

```math
\begin{aligned}
\text{minimize}\quad &y^T y\\
\text{subject to}\quad &-y\preceq Ax-b\preceq y,\quad \forall A\in\mathcal{A}.
\end{aligned}
```

> **思路：** 引入上界变量 $y\in\mathbb{R}^m$，约束 $|Ax-b|_i\le y_i$ 对所有 $A\in\mathcal{A}$ 成立，再最小化 $\|y\|_2$（或 $\|y\|_2^2$）。由于取上确界后最坏情形可逐行（rowwise）分离，我们逐行求 $\sup_A(Ax-b)_i$ 与 $\inf_A(Ax-b)_i$。

我们有

```math
\sup_{A\in\mathcal{A}}(Ax-b)_i=\sum_{j=1}^{n}\big(\bar A_{ij}x_j+R_{ij}|x_j|\big)-b_i,
```

```math
\inf_{A\in\mathcal{A}}(Ax-b)_i=\sum_{j=1}^{n}\big(\bar A_{ij}x_j-R_{ij}|x_j|\big)-b_i.
```

> **推导：** $(Ax)_i=\sum_j A_{ij}x_j$，其中 $A_{ij}\in[\bar A_{ij}-R_{ij},\bar A_{ij}+R_{ij}]$。为最大化 $\sum_j A_{ij}x_j$，当 $x_j\ge0$ 时取 $A_{ij}=\bar A_{ij}+R_{ij}$（贡献 $+\bar A_{ij}x_j+R_{ij}|x_j|$），当 $x_j<0$ 时取 $A_{ij}=\bar A_{ij}-R_{ij}$（此时 $R_{ij}x_j=R_{ij}(-|x_j|)=-R_{ij}|x_j|$，但仍写成 $+R_{ij}|x_j|$ 因为 $-R_{ij}x_j=R_{ij}|x_j|$）。统一地，$A_{ij}=\bar A_{ij}+R_{ij}\operatorname{sign}(x_j)$ 时取到最大值 $\sum_j(\bar A_{ij}x_j+R_{ij}|x_j|)$。同理取 $\operatorname{sign}$ 相反方向得最小值。

因此问题可写为

```math
\begin{aligned}
\text{minimize}\quad &y^T y\\
\text{subject to}\quad &\bar Ax+R|x|-b\preceq y\\
&\bar Ax-R|x|-b\succeq -y,
\end{aligned}
```

其中 $|x|\in\mathbb{R}^n$ 的元素为 $|x|_i=|x_i|$。等价地，引入变量 $z$ 作为 $|x|$ 的上界，写成 QP：

```math
\begin{aligned}
\text{minimize}\quad &y^T y\\
\text{subject to}\quad &\bar Ax+Rz-b\preceq y\\
&\bar Ax-Rz-b\succeq -y\\
&-z\preceq x\preceq z.
\end{aligned}
```

变量是 $x\in\mathbb{R}^n$、$y\in\mathbb{R}^m$、$z\in\mathbb{R}^n$。

> **说明：** 约束 $-z\preceq x\preceq z$（即 $|x_j|\le z_j$）保证 $z$ 是 $|x|$ 的逐元素上界；由于目标中 $y$ 被极小化，最优时 $z_j=|x_j|$ 会「贴紧」成立。变量总数 $2n+m$，约束数 $2m+2n$，均随 $m,n$ 线性增长。

该问题还有另一种表述：关键是找到最坏情形残差范数作为 $x$ 的函数的另一种表达式。令 $f(x)=\sup_{A\in\mathcal{A}}\|Ax-b\|_2^2$。

```math
f(x)=\sup\{\|Ax-b\|_2^2\mid A=\bar A+\Delta,\ |\Delta_{ij}|\le R_{ij}\}
```

```math
=\sup\{\|\bar Ax+\Delta x-b\|_2^2\mid |\Delta_{ij}|\le R_{ij}\}
```

```math
=\sup\{\|r+\Delta x\|_2^2\mid |\Delta_{ij}|\le R_{ij}\},
```

其中 $r=\bar Ax-b$。

由于
```math
\|r+\Delta x\|_2^2=\sum_{i=1}^{m}\Big(r_i+\sum_{j=1}^{n}\Delta_{ij}x_j\Big)^2
=\sum_{i=1}^{m}\Big|r_i+\sum_{j=1}^{n}\Delta_{ij}x_j\Big|^2,
```
易见此表达式关于 $\Delta$ 的各行是**可分离的（separable in the rows of $\Delta$）**。所以为求 $f(x)$，只需先求
```math
\sup\Big\{\Big|r_i+\sum_{j=1}^n\Delta_{ij}x_j\Big|:\ |\Delta_{ij}|\le R_{ij},\ j=1,\dots,n\Big\}.
```
该上确界通过取 $\Delta_{ij}=R_{ij}\operatorname{sign}(x_j)$（当 $r_i\ge0$ 时）或 $\Delta_{ij}=-R_{ij}\operatorname{sign}(x_j)$（当 $r_i<0$ 时）达到，等于
```math
|r_i|+\sum_{j=1}^{n}R_{ij}|x_j|.
```
> **推导：** 括号内为 $s=r_i+\sum_j\Delta_{ij}x_j$。要最大化 $|s|$：若 $r_i\ge0$，令每个 $\Delta_{ij}x_j$ 与 $r_i$ 同号，取 $\Delta_{ij}=R_{ij}\operatorname{sign}(x_j)$ 得 $s=r_i+\sum_j R_{ij}|x_j|\ge0$；若 $r_i<0$，取 $\Delta_{ij}=-R_{ij}\operatorname{sign}(x_j)$ 得 $s=r_i-\sum_j R_{ij}|x_j|<0$，其绝对值为 $|r_i|+\sum_j R_{ij}|x_j|$。两种情况统一为 $|r_i|+\sum_j R_{ij}|x_j|$。

因此
```math
f(x)=\sum_{i=1}^{m}\Big(|r_i|+\sum_{j=1}^{n}R_{ij}|x_j|\Big)^2
=\big\||\bar Ax-b|+R|x|\big\|_2^2.
```math
f(x)=\big\||\bar Ax-b|+R|x|\big\|_2^2.
```

于是鲁棒最小二乘问题可重表述为

```math
\text{minimize}\quad \big\||\bar Ax-b|+R|x|\big\|_2.
```

注意目标函数是凸的：因为欧氏范数（Euclidean norm）在 $\mathbb{R}^m_+$ 上是凸的且递增（increasing），而 $|\bar Ax-b|+R|x|$ 是凸的且非负的（convex and nonnegative）。

**(b)** 下面的脚本计算最小二乘解和鲁棒解，并对每个解计算标称残差范数和最坏情形残差范数：

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

鲁棒最小二乘解也可用如下脚本求得：

```matlab
cvx_begin
variables x(3) t(4)
minimize ( norm ( t ) )
abs(A_bar*x - b) + R*abs(x) <= t
cvx_end
```

该脚本返回如下结果：

```text
Residual norms for the nominal problem when using LS solution:   7.5895
Residual norms for the nominal problem when using robust solution: 17.7106
Residual norms for the worst-case problem when using LS solution:   26.7012
Residual norms for the worst-case problem when using robust solution: 17.7940
```

> **结果解读（为什么合理）：** 标称 LS 解在标称残差上最小（7.5895），但它对数据扰动非常敏感，最坏情形残差高达 26.7012（几乎是标称值的 3.5 倍）。鲁棒解牺牲了标称性能（标称残差升到 17.7106），却把最坏情形残差压到 17.7940，与标称残差几乎持平——这正是鲁棒优化的典型权衡：以更差的名义性能换取对不确定性的低敏感度。

我们还（作为趣味）生成了如下直方图，展示 $x=x_{ls}$ 与 $x=x_{rls}$ 两种情况下残差范数的分布。做法是生成 1000 个 $A$ 的实例（每个 $A_{ij}$ 在 $\bar A_{ij}-R_{ij}$ 与 $\bar A_{ij}+R_{ij}$ 之间均匀采样），再对每个 $A$ 和每个解求残差范数。

[[FIGURE: 第 15 页, 直方图：$x=x_{ls}$ 的残差范数分布，横轴约 $[0,20]$，纵轴为频数。分布较分散。]]

[[FIGURE: 第 16 页, 直方图：$x=x_{rls}$ 的残差范数分布，横轴约 $[17.64,17.80]$，纵轴为频数。分布高度集中。]]

生成这些直方图的脚本如下：

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

---

## 题目 6（Problem 6）——附加练习 5：全变分图像插值

**题干：** Total variation image interpolation. A grayscale image is represented as an $m\times n$ matrix of intensities $U^{\text{orig}}$. You are given the values $U^{\text{orig}}_{ij}$ for $(i,j)\in\mathcal{K}$, where $\mathcal{K}\subset\{1,\dots,m\}\times\{1,\dots,n\}$. Your job is to interpolate the image, by guessing the missing values. The reconstructed image will be represented by $U\in\mathbb{R}^{m\times n}$, where $U$ satisfies the interpolation conditions $U_{ij}=U^{\text{orig}}_{ij}$ for $(i,j)\in\mathcal{K}$. The reconstruction is found by minimizing a roughness measure subject to the interpolation conditions.

One common roughness measure is the $\ell_2$ variation (squared),
```math
\sum_{i=2}^{m}\sum_{j=2}^{n}\Big((U_{ij}-U_{i-1,j})^2+(U_{ij}-U_{i,j-1})^2\Big).
```
Another method minimizes instead the total variation,
```math
\sum_{i=2}^{m}\sum_{j=2}^{n}\Big(|U_{ij}-U_{i-1,j}|+|U_{ij}-U_{i,j-1}|\Big).
```
Evidently both methods lead to convex optimization problems. Carry out $\ell_2$ and total variation interpolation on the problem instance with data given in `tv_img_interp.m`. This will define $m$, $n$, and matrices `Uorig` and `Known`. The matrix `Known` is $m\times n$, with $(i,j)$ entry one if $(i,j)\in\mathcal{K}$, and zero otherwise.

**中文：** 全变分图像插值（total variation image interpolation）。一幅灰度图像（grayscale image）表示为强度（intensities）的 $m\times n$ 矩阵 $U^{\text{orig}}$。给定 $(i,j)\in\mathcal{K}$（其中 $\mathcal{K}\subset\{1,\dots,m\}\times\{1,\dots,n\}$）处的值 $U^{\text{orig}}_{ij}$。任务是插值（interpolate）图像，猜出缺失的值。重建图像表示为 $U\in\mathbb{R}^{m\times n}$，且 $U$ 满足插值条件（interpolation conditions）$U_{ij}=U^{\text{orig}}_{ij}$（对 $(i,j)\in\mathcal{K}$）。重建是通过在插值条件约束下最小化一个粗糙度度量（roughness measure）得到的。

一种常见的粗糙度度量是 $\ell_2$ 变分（平方，squared）：

```math
\sum_{i=2}^{m}\sum_{j=2}^{n}\Big((U_{ij}-U_{i-1,j})^2+(U_{ij}-U_{i,j-1})^2\Big).
```

另一种方法改为最小化全变分（total variation）：

```math
\sum_{i=2}^{m}\sum_{j=2}^{n}\Big(|U_{ij}-U_{i-1,j}|+|U_{ij}-U_{i,j-1}|\Big).
```

显然两种方法都导出凸优化问题。在 `tv_img_interp.m` 给出的问题实例上执行 $\ell_2$ 和全变分插值。该文件会定义 $m$、$n$ 以及矩阵 `Uorig` 和 `Known`。矩阵 `Known` 是 $m\times n$ 的，$(i,j)$ 处为 1 表示 $(i,j)\in\mathcal{K}$，否则为 0。（给出完整原始图像是为了让你把重建结果与原始图像对比；显然你的解不能访问 $(i,j)\notin\mathcal{K}$ 处的 $U^{\text{orig}}_{ij}$。）

**解答：** 插值代码非常简单。对 $\ell_2$ 插值，代码如下：

```matlab
cvx_begin
variable Ul2(m, n);
Ul2(Known) == Uorig(Known);   % Fix known pixel values.  固定已知像素值
Ux = Ul2(2:end,2:end) - Ul2(2:end,1:end-1);   % x (horiz) differences  水平差分
Uy = Ul2(2:end,2:end) - Ul2(1:end-1,2:end);   % y (vert) differences  垂直差分
minimize(norm([Ux(:); Uy(:)], 2));   % l2 roughness measure  ℓ2 粗糙度度量
cvx_end
```

对全变分插值，使用如下代码：

```matlab
cvx_begin
variable Utv(m, n);
Utv(Known) == Uorig(Known);   % Fix known pixel values.  固定已知像素值
Ux = Utv(2:end,2:end) - Utv(2:end,1:end-1);   % x (horiz) differences  水平差分
Uy = Utv(2:end,2:end) - Utv(1:end-1,2:end);   % y (vert) differences  垂直差分
minimize(norm([Ux(:); Uy(:)], 1));   % tv roughness measure  TV 粗糙度度量
cvx_end
```

> **说明：** 两者都是凸问题。$\ell_2$ 情形是约束最小二乘（约束为对已知像素的等式约束，目标为 $\|Ux\|_2^2+\|Uy\|_2^2$ 的平方根，等价于最小化其平方），是 QP。全变分情形目标为 $\ell_1$ 范数（凸），约束是线性等式，也是凸问题。全变分倾向于产生分片常数（piecewise-constant）的重建，因而能更好地保留边缘。

我们得到如下图像：

[[FIGURE: 第 18 页, 四幅 50×50 灰度图：原始图像（Original image）、遮挡图像（Obscured image）、ℓ2 重建图像（ℓ2 reconstructed image）、全变分重建图像（Total variation reconstructed image）。]]

---

## 题目 7（Problem 7）——附加练习 6：松弛与离散 A-最优实验设计

**题干：** Relaxed and discrete A-optimal experiment design. This problem concerns the A-optimal experiment design problem, described on page 387, with data generated as follows:

```matlab
n = 5;    % dimension of parameters to be estimated
p = 20;   % number of available types of measurements
m = 30;   % total number of measurements to be carried out
randn('state', 0);
V=randn(n,p);   % columns are vi, the possible measurement vectors
```

Solve the relaxed A-optimal experiment design problem,
```math
\text{minimize}\quad (1/m)\operatorname{tr}\!\left(\sum_{i=1}^{p}\lambda_i v_i v_i^T\right)^{-1}
\quad\text{subject to}\quad \mathbf{1}^T\lambda=1,\ \lambda\succeq0,
```
with variable $\lambda\in\mathbb{R}^p$. Find the optimal point $\lambda^\star$ and the associated optimal value of the relaxed problem. This optimal value is a lower bound on the optimal value of the discrete A-optimal experiment design problem,
```math
\text{minimize}\quad \operatorname{tr}\!\left(\sum_{i=1}^{p}m_i v_i v_i^T\right)^{-1}
\quad\text{subject to}\quad m_1+\cdots+m_p=m,\ m_i\in\{0,\dots,m\},\ i=1,\dots,p,
```
with variables $m_1,\dots,m_p$. To get a suboptimal point for this discrete problem, round the entries in $m\lambda^\star$ to obtain integers $\hat m_i$. If needed, adjust these by hand or some other method to ensure that they sum to $m$, and compute the objective value obtained. This is an upper bound on the optimal value of the discrete problem. Give the gap between this upper bound and the lower bound obtained from the relaxed problem.

**中文：** 松弛与离散 A-最优实验设计（relaxed and discrete A-optimal experiment design）。本题涉及 A-最优实验设计问题（见第 387 页），数据如下生成：

```matlab
n = 5;    % 待估计参数的维数
p = 20;   % 可用的测量类型数
m = 30;   % 要执行的测量总数
randn('state', 0);
V=randn(n,p);   % 列是 vi，即可能的测量向量
```

求解松弛（relaxed）A-最优实验设计问题：

```math
\text{minimize}\quad (1/m)\operatorname{tr}\left(\sum_{i=1}^{p}\lambda_i v_i v_i^T\right)^{-1}
\quad\text{subject to}\quad \mathbf{1}^T\lambda=1,\ \lambda\succeq0,
```

变量为 $\lambda\in\mathbb{R}^p$。求最优解 $\lambda^\star$ 及松弛问题的最优值。该最优值是离散 A-最优实验设计问题的**下界（lower bound）**：

```math
\text{minimize}\quad \operatorname{tr}\left(\sum_{i=1}^{p}m_i v_i v_i^T\right)^{-1}
\quad\text{subject to}\quad m_1+\cdots+m_p=m,\ m_i\in\{0,\dots,m\},\ i=1,\dots,p,
```

变量为 $m_1,\dots,m_p$。为得到该离散问题的一个次优解（suboptimal point），把 $m\lambda^\star$ 的各元素取整（round）得整数 $\hat m_i$；必要时手工或其他方法调整使它们之和等于 $m$，并计算所得目标值。这是离散问题最优值的**上界（upper bound）**。给出该上界与松弛问题所得下界之间的间隔（gap）。注意这两个目标值都可解释为均方估计误差（mean-square estimation error）$\mathbb{E}\|\hat x-x\|_2^2$。

**解答：** 松弛问题的目标是凸的，所以是凸问题。在 cvx 中表示它需要一点技巧。我们本想写成

```matlab
minimize ((1/m)*trace(inv(V*diag(lambda)*V')))
```

但这样不行，因为 cvx 不认识矩阵凸函数（matrix convex functions）。改为把目标写成若干矩阵分式函数（matrix fractional functions）之和：

```math
\text{minimize}\quad (1/m)\sum_{k=1}^{n}e_k^T\left(\sum_{i=1}^{p}\lambda_i v_i v_i^T\right)^{-1}e_k
\quad\text{subject to}\quad \mathbf{1}^T\lambda=1,\ \lambda\succeq0,
```

其中 $e_k\in\mathbb{R}^n$ 是第 $k$ 个单位向量（unit vector）。

> **说明（为什么能这样改写迹）：** 对对称正定矩阵 $M=\sum_i\lambda_i v_i v_i^T$，迹等于对角元之和：
>
```math
\operatorname{tr}(M^{-1})=\sum_{k=1}^{n}(M^{-1})_{kk}=\sum_{k=1}^{n}e_k^T M^{-1}e_k.
```
> 每一项 $e_k^T M^{-1}e_k$ 都是关于 $\lambda$ 的凸函数（矩阵分式函数 matrix fractional function 是凸的），故目标和是凸函数。cvx 中可用内置 `matrix_frac` 表达 $e_k^T M^{-1}e_k$。

（注：$e$ 在练习 6.9 中被定义为估计误差向量，所以 $e_k$ 也可能指误差向量第 $k$ 个分量；但此处显然 $e_k$ 是第 $k$ 个单位向量。）

可用 cvx 的 `matrix_frac` 函数表达。下面的代码求解该问题：

```matlab
n = 5;    % dimension
p = 20;   % number of available types of measurements
m = 30;   % total number of measurements to be carried out
randn('state', 0);
V=randn(n,p);   % columns are vi, the possible measurement vectors
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
t = -0.00;   % small offset chosen by hand to make rounding work out.
             % for this problem data, none is needed!
m_rnd = pos(round(m*lambda+t));
sum(m_rnd)   % should be == m
% now find objective value of rounded experiment design
upper_bound = trace(inv(V*diag(m_rnd/m)*V'))/m
gap = upper_bound-lower_bound
rel_gap = gap/lower_bound
```

对这个具体问题实例，简单取整得到的 $\hat m_i$ 之和恰好等于 $m=30$，因此无需调整取整值。下界为 0.2481，上界为 0.2483，间隔（gap）为 0.00023，约为 $0.1\%$。

其含义是：我们找到了一种 30 次测量的选择（每次从 20 种可能测量中选取），使得均方估计误差 $\mathbb{E}\|\hat x-x\|_2^2=0.2483$。我们不知道这是否是 30 次测量的最优选择，但我们知道这个选择至多比最优差 $0.1\%$；最优选择能达到的均方误差不小于 0.2481。因此我们的实验设计即使不是最优，也几乎是最优的（事实上它极可能就是最优的）。

---
