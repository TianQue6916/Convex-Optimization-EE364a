# EE364a Homework 7 solutions（EE364a 作业 7 解答）

> 原文：`HW7 解答.pdf`（16 页），EE364a, Winter 2007-08, Prof. S. Boyd。前 9 页是教材习题解答（8.16, 9.30, 9.31），第 10 页起是 "Solutions to additional exercises"，先给 9.30/9.31 的实现提示，然后是两道附加题（三维线性分类、正则化 least-squares 的高效数值解法）。

## 8.16 Maximum volume rectangle inside a polyhedron（多面体内最大体积的盒子）

把下列问题表述为 convex optimization problem。求内含于 polyhedron（多面体）$P = \{x \mid Ax \preceq b\}$ 的 rectangle（盒子）

```math
R = \{x \in \mathbf{R}^n \mid l \preceq x \preceq u\}
```

使其 volume（体积）最大。变量为 $l, u \in \mathbf{R}^n$。你的表述不应涉及指数数目的约束。

**Solution**。表达约束 $R \subseteq P$ 的一个直接但非常低效的方法，是用 $R$ 的 $2^n$ 个顶点 $v^i$ 处的 $m2^n$ 条不等式 $Av^i \preceq b$。（若盒子的所有顶点都在 polyhedron 内，则整个盒子在它内。）幸运的是，可以用远为高效的方式表达该约束。定义

```math
a_{ij}^+ = \max\{a_{ij}, 0\}, \qquad a_{ij}^- = \max\{-a_{ij}, 0\} .
```

则 $R \subseteq P$ 当且仅当

```math
\sum_{j=1}^n \big( a_{ij}^+ u_j - a_{ij}^- l_j \big) \le b_i, \qquad i = 1,\dots,m .
```

† 原文此式与下一式的求和下限/下标印作 $i = 1$（即 $\sum_{i=1}^n$），应为 $\sum_{j=1}^n$——求和的指标是坐标下标 $j$，$i$ 是约束（行）下标。

maximum volume rectangle 是

```math
\begin{aligned}
\text{maximize} \quad & \Big( \prod_{i=1}^n (u_i - l_i) \Big)^{1/n} \\
\text{subject to} \quad & \sum_{j=1}^n \big( a_{ij}^+ u_j - a_{ij}^- l_j \big) \le b_i, \quad i = 1,\dots,m,
\end{aligned}
```

的解，带隐式约束 $u \succeq l$。另一种表述是对 objective 取对数，得到

```math
\begin{aligned}
\text{maximize} \quad & \sum_{i=1}^n \log(u_i - l_i) \\
\text{subject to} \quad & \sum_{j=1}^n \big( a_{ij}^+ u_j - a_{ij}^- l_j \big) \le b_i, \quad i = 1,\dots,m .
\end{aligned}
```

（推导要点（译者补）：约束 $a_i^Tx \le b_i$ 在盒子上的最大值在「正系数取 $u_j$、负系数取 $l_j$」处达到，即 $\max_{x \in R} a_i^Tx = \sum_j (a_{ij}^+ u_j - a_{ij}^- l_j)$。这只用 $n$ 项求和，而顶点枚举要 $2^n$ 条约束——这就是「不涉及指数数目约束」的意思。取 log 后 objective 是 concave 的（$\log$ 递增且 concave，复合 affine 差），约束是 linear 的，所以是 convex problem；而几何平均形式则是它的等价写法。）

## 9.30 Gradient and Newton methods（梯度法与 Newton 法）

考虑无约束问题

```math
\text{minimize} \quad f(x) = -\sum_{i=1}^m \log(1 - a_i^T x) - \sum_{i=1}^n \log(1 - x_i^2),
```

变量为 $x \in \mathbf{R}^n$，且

```math
\mathbf{dom}\,f = \{x \mid a_i^T x < 1,\ i = 1,\dots,m,\ |x_i| < 1,\ i = 1,\dots,n\} .
```

这是计算 linear inequalities 集合

```math
a_i^T x \le 1,\ i = 1,\dots,m, \qquad |x_i| \le 1,\ i = 1,\dots,n
```

的 analytic center（解析中心）的问题。注意我们可以取 $x^{(0)} = 0$ 作为初始点。你可以通过从 $\mathbf{R}^n$ 上某个分布中选取 $a_i$ 来生成该问题的实例。

**(a)** 用 gradient method 求解该问题，backtracking 参数取合理值，stopping criterion 取 $\|\nabla f(x)\|_2 \le \eta$ 的形式。画出 objective function 与 step length 随迭代次数的变化。（一旦把 $p^\star$ 算到很高精度，也可以画 $f - p^\star$ 随迭代次数的变化。）试验 backtracking 参数 $\alpha$ 与 $\beta$，观察它们对所需总迭代次数的影响。对若干不同规模的问题实例做这些实验。

**(b)** 改用 Newton's method，stopping criterion 基于 Newton decrement（Newton 减量）$\lambda^2$。观察 quadratic convergence（二次收敛）。你不必像习题 9.27 那样用高效方法计算 Newton step；可以用通用 dense solver（稠密求解器），虽然用基于 Cholesky factorization 的更好。

**Hint（提示）**。用 chain rule（链式法则）求 $\nabla f(x)$ 与 $\nabla^2 f(x)$ 的表达式。

**Solution**。

**(a) Gradient method**。下图给出 $m = 200$、$n = 100$ 的一个例子的函数值与步长随迭代次数的变化。我们取 $\alpha = 0.01$、$\beta = 0.5$，退出条件为 $\|\nabla f(x^{(k)})\|_2 \le 10^{-3}$。

[Figure 1：左图 objective function 值 $f(x^{(k)}) - p^\star$ 随 $k$ 变化（对数纵轴 $10^{-8}$ 到 $10^4$，横轴 $k \in [0,500]$），单调下降；右图 step length $t^{(k)}$ 随 $k$ 变化（纵轴 $0$ 到 $0.016$，横轴 $k \in [0,600]$），从 $0.014$ 附近起伏下降到 $0.006$ 左右，呈锯齿状]

下面是一个 Matlab 实现。

```matlab
randn('state',1);
m=200;
n=100;
ALPHA = 0.01;
BETA = 0.5;
MAXITERS = 1000;
NTTOL = 1e-8;
GRADTOL = 1e-3;
% generate random problem
A = randn(m,n);
% gradient method
vals = []; steps = [];
x = zeros(n,1);
for iter = 1:MAXITERS
val = -sum(log(1-A*x)) - sum(log(1+x)) - sum(log(1-x));
vals = [vals, val];
d = 1./(1-A*x);
grad = A'*d - 1./(1+x) + 1./(1-x);
v = -grad;
fprime = grad'*v;
norm(grad)
if norm(grad) < GRADTOL, break; end;
t = 1;
while ((max(A*(x+t*v)) >= 1) | (max(abs(x+t*v)) >= 1)),
t = BETA*t;
end;
while ( -sum(log(1-A*(x+t*v))) - sum(log(1-(x+t*v).^2)) > ...
val + ALPHA*t*fprime )
t = BETA*t;
end;
x = x+t*v;
steps = [steps,t];
end;
figure(1)
semilogy([0:(length(vals)-2)], vals(1:length(vals)-1)-optval, '-');
xlabel('x'); ylabel('z');
figure(2)
plot([1:length(steps)], steps, ':',[1:length(steps)], steps, 'o');
xlabel('x'); ylabel('z');
```

（推导 $\nabla f$（译者补）：由 §A.4 的链式法则，$\nabla f(x) = \sum_i \frac{a_i}{1 - a_i^Tx} + \sum_i \frac{2x_i}{1-x_i^2} = A^Td + \frac{2x}{1-x^2}$，其中 $d_i = 1/(1-a_i^Tx)$。注意原文代码把后一半写成 $-1/(1+x) + 1/(1-x)$，因为 $\frac{2x}{1-x^2} = \frac{1}{1-x} - \frac{1}{1+x}$——这是同一个量，拆成两项是为了 Hessian 好写。）

**(b) Newton method**。下图给出同一例子的函数值与步长随迭代次数的变化。我们取 $\alpha = 0.01$、$\beta = 0.5$，退出条件为 $\lambda(x^{(k)})^2 \le 10^{-8}$。

[Figure 2：左图 $f(x^{(k)}) - p^\star$ 随 $k$ 变化（对数纵轴 $10^{-10}$ 到 $10^5$，横轴 $k \in [0,7]$），前几步下降缓慢、之后急剧下跌到 $10^{-10}$（典型的 quadratic convergence 尾部）；右图 step length $t^{(k)}$ 随 $k$ 变化（纵轴 $0$ 到 $1$，横轴 $k \in [0,8]$），从接近 $1$ 后逐步降到 $0.2$ 以下]

下面是一个 Matlab 实现。

```matlab
% Newton method
vals = []; steps = [];
x = zeros(n,1);
for iter = 1:MAXITERS
val = -sum(log(1-A*x)) - sum(log(1+x)) - sum(log(1-x));
vals = [vals, val];
d = 1./(1-A*x);
grad = A'*d - 1./(1+x) + 1./(1-x);
hess = A'*diag(d.^2)*A + diag(1./(1+x).^2 + 1./(1-x).^2);
v = -hess\grad;
fprime = grad'*v
if abs(fprime) < NTTOL, break; end;
t = 1;
while ((max(A*(x+t*v)) >= 1) | (max(abs(x+t*v)) >= 1)),
t = BETA*t;
end;
while ( -sum(log(1-A*(x+t*v))) - sum(log(1-(x+t*v).^2)) > ...
val + ALPHA*t*fprime )
t = BETA*t;
end;
x = x+t*v;
steps = [steps,t];
end;
optval = vals(length(vals));
figure(3)
semilogy([0:(length(vals)-2)], vals(1:length(vals)-1)-optval, '-', ...
[0:(length(vals)-2)], vals(1:length(vals)-1)-optval, 'o');
xlabel('x'); ylabel('z');
figure(4)
plot([1:length(steps)], steps, '-', [1:length(steps)], steps, 'o');
axis([0, length(steps), 0, 1.1]);
xlabel('x'); ylabel('z');
```

（Hessian（译者补）：$\nabla^2f(x) = A^T\mathbf{diag}(d^2)A + \mathbf{diag}\big(1/(1+x)^2 + 1/(1-x)^2\big)$。第一项来自 $\sum_i a_ia_i^T/(1-a_i^Tx)^2$；第二项来自 $\frac{d}{dx}\big(\frac{2x}{1-x^2}\big) = \frac{2(1+x^2)}{(1-x^2)^2} = \frac{1}{(1+x)^2} + \frac{1}{(1-x)^2}$。这里 $\mathbf{diag}(\cdot)$ 是对角矩阵技巧的关键——不要逐项求 $\partial^2 f/\partial x_i\partial x_j$（原文提示里说「you will be sorry」正是此意）。）

## 9.31 Some approximate Newton methods（几种近似 Newton 法）

Newton's method 的成本主要来自计算 Hessian $\nabla^2f(x)$ 与求解 Newton system。对大规模问题，有时把 Hessian 换成一个正定近似，使构造与求 search step 都更容易。本题我们探索这一想法的一些常见例子。

对下面描述的每种 approximate Newton method，在一些 analytic centering problem（见习题 9.30）的实例上测试它，并把结果与用 Newton's method 及 gradient method 得到的结果比较。

**(a) Re-using the Hessian（复用 Hessian）**。我们只每 $N$ 次迭代计算并分解一次 Hessian（$N > 1$），并使用 search step $\Delta x = -H^{-1}\nabla f(x)$，其中 $H$ 是最近一次计算的 Hessian。（每 $N$ 步需要计算并分解一次 Hessian；其它步用前代与回代计算 search direction。）

**(b) Diagonal approximation（对角近似）**。我们把 Hessian 换成它的对角部分，于是只需计算 $n$ 个二阶导数 $\partial^2 f(x)/\partial x_i^2$，计算 search step 也非常容易。

**Solution**。

**(a)** 下图画出（与习题 9.30 解答中同一例子）函数值随大约所需总 flops 数的变化，取 $N = 1$（即 Newton's method）、$N = 15$ 与 $N = 30$。

[Figure 3：$f(x^{(k)}) - p^\star$ 随累计 flops 数变化（对数纵轴 $10^{-10}$ 到 $10^5$，横轴 $0$ 到 $2.5\times10^6$）——Newton（$N=1$）的曲线最陡，$N=15$ 次之，$N=30$ 最缓，但三者在相同的 flops 预算下达到的精度差别不大，说明「复用因式分解」在真实计算量上确有收益]

我们看到，用「一个分解好的 Hessian 走若干步」的方法，按真实计算量（即所需 flops 数）衡量，收敛速度提高了。当然按迭代次数衡量，该方法比基本 Newton's method 差。

下面是一个 Matlab 实现。

```matlab
randn('state',1);
m=200;
n=100;
ALPHA = 0.01;
BETA = 0.5;
MAXITERS = 1000;
NTTOL = 1e-9;
GRADTOL = 1e-3;
% generate random problem
A = randn(m,n);
% Newton method with periodically updated Hessian
for N = [1,15,30]; % re-compute Hessian every N iterations
vals = [];
flops = [];
flop = 0;
x = zeros(n,1);
for iter = 1:MAXITERS
val = -sum(log(1-A*x))-sum(log(1+x))-sum(log(1-x));
vals = [vals,val];
flops = [flops,flop];
d = 1./(1-A*x);
grad = A'*d-1./(1+x)+1./(1-x);
if (rem(iter-1,N) == 0)
H = A'*diag(d.^2)*A+diag(1./(1+x).^2+1./(1-x).^2);
L = chol(H,'lower');
flop = (1/3)*n^3; % add flop for Cholesky factorization
else
flop = 0;
end
v = -L'\(L\grad);
flop = flop+2*n^2; % add flop for fwd/bwd substitution
fprime = grad'*v
if (abs(fprime) < NTTOL) break; end
t = 1;
while ((max(A*(x+t*v))>=1) | (max(abs(x+t*v))> 1)),
t = BETA*t;
end
while (-sum(log(1-A*(x+t*v)))-sum(log(1-(x+t*v).^2)) > ...
val + ALPHA*t*fprime )
t = BETA*t;
end
x = x+t*v;
end
if (N==1), optval = vals(length(vals)); end
figure(1)
cflops = cumsum(flops(1:end-1));
perror = vals(1:end-1)-optval;
semilogy(cflops,perror,'-',cflops,perror,'o');
hold on;
semilogy(cflops(1:N:end-1),perror(1:N:end-1),...
'mo','MarkerEdgeColor','k','MarkerFaceColor','b','MarkerSize',8);
text(cflops(end),perror(end),['N',num2str(N)]);
hold on;
end
xlabel('x'); ylabel('z');
```

（flop 记账（译者补）：一次 Cholesky 分解 $n \times n$ 对称正定矩阵约 $\frac{1}{3}n^3$ flops；一次前代+回代约 $2n^2$ flops。所以「每 $N$ 步分解一次」把每次迭代的平均成本从 $\frac{1}{3}n^3 + 2n^2$ 降到 $\frac{1}{3N}n^3 + 2n^2$，但迭代次数会增加。总代价的比较必须按 flops 而不能按迭代数——这就是横轴取累计 flops 的原因。）

**(b)** 下图画出（与习题 9.30 解答中同一例子）函数值随迭代次数的变化，用的是 Hessian 的对角近似。实验表明该算法的收敛行为与 gradient method 非常相似。

[Figure 4：$f(x^{(k)}) - p^\star$ 随 $k$ 变化（对数纵轴 $10^{-8}$ 到 $10^4$，横轴 $k \in [0,800]$）——曲线单调下降但斜率远不如 Newton 法，800 步才降到 $10^{-8}$ 附近，形态上更像 gradient method 的线性收敛]

下面是一个 Matlab 实现。

```matlab
% Newton method with diagonal approximation of Hessian
vals = [];
x = zeros(n,1);
for iter = 1:MAXITERS
val = -sum(log(1-A*x)) - sum(log(1+x)) - sum(log(1-x));
vals = [vals, val];
d = 1./(1-A*x);
grad = A'*d - 1./(1+x) + 1./(1-x);
hess = A'*diag(d.^2)*A + diag(1./(1+x).^2 + 1./(1-x).^2);
H = diag(diag(hess));
norm(grad)
if norm(grad) < GRADTOL, break; end;
v = -H\grad; fprime = grad'*v;
if (fprime > 0), keyboard; end;
t = 1;
while ((max(A*(x+t*v)) >= 1) | (max(abs(x+t*v)) >= 1)),
t = BETA*t;
end
while ( -sum(log(1-A*(x+t*v))) - sum(log(1-(x+t*v).^2)) > ...
val + ALPHA*t*fprime )
t = BETA*t;
end
x = x+t*v;
end
figure(2)
semilogy([0:(length(vals)-2)], vals(1:length(vals)-1)-optval, '-');
xlabel('x'); ylabel('z');
```

（解释（译者补）：丢掉 Hessian 的非对角项后，search direction 只剩坐标方向上的缩放，于是每步实际上退化成「坐标缩放的梯度步」，只能线性收敛。这条实验的意义是：Newton 法的威力来自 Hessian 的非对角耦合信息，而不只是「按二阶导数缩放」。）

# Solutions to additional exercises（附加题解答）

## Suggestions for exercise 9.30（对习题 9.30 的建议）

生成一个测试实例，我们推荐如下：

```matlab
n = 100;
m = 200;
randn('state',1);
A=randn(m,n);
```

当然，你也应当用不同的维度、不同的数据来测试你的代码。

在所有情形下，务必让你的 line search 首先找到一个使试探点落在 $\mathbf{dom}\,f$ 内的步长；如果你试图在定义域外计算 $f$，你会得到复数，而且永远回不来。

为求 $\nabla f(x)$ 与 $\nabla^2 f(x)$ 的表达式，请用 chain rule（见附录 A.4）；若你试图逐项计算 $\partial^2 f(x)/\partial x_i \partial x_j$，你会后悔的。

计算 Newton step 可以用 `vnt=-H\g`。

## Suggestions for exercise 9.31（对习题 9.31 的建议）

对 9.31a，你应当试 $N = 1$、$N = 15$ 与 $N = 30$。你不妨计算并存储 Hessian 的 Cholesky factorization，然后回代得到 search direction——尽管对这么小的问题在 Matlab 里你其实看不到什么加速。算出 Hessian 后，可以取 Cholesky 分解为 `L=chol(H,'lower')`。然后可以按 `-L'\(L\g)` 计算 search step，其中 `g` 是当前点的 gradient。Matlab 会做正确的事：先用前代解 `L\g`，再用回代解 `-L'\(L\g)`。每次替换是 $n^2$ 阶。

为公平比较三种方法（即 $N = 1$、$N = 15$、$N = 30$）的收敛性，横轴应当显示大约所需的总 flops 数，而不是迭代次数。你可以用每次分解 $n^3/3$、每次求解 $2n^2$ 来算大约的 flops 数（这里每次「求解」包含一次前代与一次回代）。

# Additional exercises（附加题）

## 附加题 1 · Three-way linear classification（三类线性分类）

给定数据

```math
x^{(1)},\dots,x^{(N)}, \qquad y^{(1)},\dots,y^{(M)}, \qquad z^{(1)},\dots,z^{(P)},
```

它们是 $\mathbf{R}^n$ 中三个非空向量集合。我们希望找出 $\mathbf{R}^n$ 上的三个 affine function（仿射函数）

```math
f_i(z) = a_i^T z - b_i, \qquad i = 1,2,3,
```

满足下列性质：

```math
\begin{aligned}
f_1(x^{(j)}) &> \max\{f_2(x^{(j)}), f_3(x^{(j)})\}, \quad j = 1,\dots,N, \\
f_2(y^{(j)}) &> \max\{f_1(y^{(j)}), f_3(y^{(j)})\}, \quad j = 1,\dots,M, \\
f_3(z^{(j)}) &> \max\{f_1(z^{(j)}), f_2(z^{(j)})\}, \quad j = 1,\dots,P .
\end{aligned}
```

用文字说：$f_1$ 在 $x$ 数据点上三者中最大，$f_2$ 在 $y$ 数据点上三者中最大，$f_3$ 在 $z$ 数据点上三者中最大。可以给出一个简单的几何解释：这三个函数把 $\mathbf{R}^n$ 分成三个区域

```math
\begin{aligned}
R_1 &= \{z \mid f_1(z) > \max\{f_2(z), f_3(z)\}\}, \\
R_2 &= \{z \mid f_2(z) > \max\{f_1(z), f_3(z)\}\}, \\
R_3 &= \{z \mid f_3(z) > \max\{f_1(z), f_2(z)\}\},
\end{aligned}
```

它们按「各函数是三者的最大值」来划分。我们的目标是找出满足 $x^{(j)} \in R_1$、$y^{(j)} \in R_2$、$z^{(j)} \in R_3$ 的函数。

把它表述为 convex optimization problem。你的表述中不得使用 strict inequality（严格不等式）。

求解 `sep3way_data.m` 中给出的三维分离问题的具体实例，其中矩阵 `X`、`Y`、`Z` 的各列分别给出 $x^{(j)}$（$j = 1,\dots,N$）、$y^{(j)}$（$j = 1,\dots,M$）与 $z^{(j)}$（$j = 1,\dots,P$）。为省去你画数据点与分离边界的麻烦，我们已在 `sep3way_data.m` 中包含了绘图代码。（注意 `a1`、`a2`、`a3`、`b1`、`b2` 含任意数值；你应当用 cvx 算出正确的值。）

**Solution**。不等式

```math
\begin{aligned}
f_1(x^{(j)}) &> \max\{f_2(x^{(j)}), f_3(x^{(j)})\}, \quad j = 1,\dots,N, \\
f_2(y^{(j)}) &> \max\{f_1(y^{(j)}), f_3(y^{(j)})\}, \quad j = 1,\dots,M, \\
f_3(z^{(j)}) &> \max\{f_1(z^{(j)}), f_2(z^{(j)})\}, \quad j = 1,\dots,P
\end{aligned}
```

关于 $a_i$ 与 $b_i$ 是齐次的，因此可以把它们写成

```math
\begin{aligned}
f_1(x^{(j)}) &\ge \max\{f_2(x^{(j)}), f_3(x^{(j)})\} + 1, \quad j = 1,\dots,N, \\
f_2(y^{(j)}) &\ge \max\{f_1(y^{(j)}), f_3(y^{(j)})\} + 1, \quad j = 1,\dots,M, \\
f_3(z^{(j)}) &\ge \max\{f_1(z^{(j)}), f_2(z^{(j)})\} + 1, \quad j = 1,\dots,P .
\end{aligned}
```

注意我们可以给每个 $a_i$ 加上任意向量 $\alpha$，而不影响这些不等式（它们只涉及各 $a_i$ 之间的差）；出于同样理由，我们也可以给每个 $b_i$ 加上任意数 $\beta$。我们可以用这个观察来归一化或化简 $a_i$ 与 $b_i$。例如，可以不失一般性地假设 $a_1 + a_2 + a_3 = 0$ 且 $b_1 + b_2 + b_3 = 0$。

（为什么可以把严格不等式换成「$\ge$ 差 $1$」：原不等式对 $(a_i,b_i)$ 是齐次的，若某个解使左边严格大于右边，则把全部参数同时放大 $t$ 倍后左边差值也放大 $t$ 倍，取 $t$ 足够大就能让差 $\ge 1$。加上归一化条件后问题不再齐次，也不会因「无限放大」而退化。）

下面的脚本实现了该三维分类方法，并在一组小的可分数据上测试。

```matlab
clear all; close all;
% data for problem instance
M = 20;
N = 20;
P = 20;
X = [
3.5674 4.1253 2.8535 5.1892 4.3273 3.8133 3.4117 ...
3.8636 5.0668 3.9044 4.2944 4.7143 3.3082 5.2540 ...
2.5590 3.6001 4.8156 5.2902 5.1908 3.9802 ;...
-2.9981 0.5178 2.1436 -0.0677 0.3144 1.3064 3.9297 ...
0.2051 0.1067 -1.4982 -2.4051 2.9224 1.5444 -2.8687 ...
1.0281 1.2420 1.2814 1.2035 -2.1644 -0.2821];
Y = [
-4.5665 -3.6904 -3.2881 -1.6491 -5.4731 -3.6170 -1.1876 ...
-1.0539 -1.3915 -2.0312 -1.9999 -0.2480 -1.3149 -0.8305 ...
-1.9355 -1.0898 -2.6040 -4.3602 -1.8105 0.3096; ...
2.4117 4.2642 2.8460 0.5250 1.9053 2.9831 4.7079 ...
0.9702 0.3854 1.9228 1.4914 -0.9984 3.4330 2.9246 ...
3.0833 1.5910 1.5266 1.6256 2.5037 1.4384];
Z = [
1.7451 2.6345 0.5937 -2.8217 3.0304 1.0917 -1.7793 ...
1.2422 2.1873 -2.3008 -3.3258 2.7617 0.9166 0.0601 ...
-2.6520 -3.3205 4.1229 -3.4085 -3.1594 -0.7311; ...
-3.2010 -4.9921 -3.7621 -4.7420 -4.1315 -3.9120 -4.5596 ...
-4.9499 -3.4310 -4.2656 -6.2023 -4.5186 -3.7659 -5.0039 ...
-4.3744 -5.0559 -3.9443 -4.0412 -5.3493 -3.0465];
cvx_begin
variables a1(2) a2(2) a3(2) b1 b2 b3
a1'*X-b1 >= max(a2'*X-b2,a3'*X-b3)+1;
a2'*Y-b2 >= max(a1'*Y-b1,a3'*Y-b3)+1;
a3'*Z-b3 >= max(a1'*Z-b1,a2'*Z-b2)+1;
a1 + a2 + a3 == 0
b1 + b2 + b3 == 0
cvx_end
% now let's plot the three-way separation induced by
% a1,a2,a3,b1,b2,b3
% find maximally confusing point
p = [(a1-a2)';(a1-a3)'\]\[(b1-b2);(b1-b3)\];
% plot
t = [-7:0.01:7];
u1 = a1-a2; u2 = a2-a3; u3 = a3-a1;
v1 = b1-b2; v2 = b2-b3; v3 = b3-b1;
line1 = (-t*u1(1)+v1)/u1(2); idx1 = find(u2'*[t;line1]-v2>0);
line2 = (-t*u2(1)+v2)/u2(2); idx2 = find(u3'*[t;line2]-v3>0);
line3 = (-t*u3(1)+v3)/u3(2); idx3 = find(u1'*[t;line3]-v1>0);
plot(X(1,:),X(2,:),'*',Y(1,:),Y(2,:),'ro',Z(1,:),Z(2,:),'g+',...
t(idx1),line1(idx1),'k',t(idx2),line2(idx2),'k',t(idx3),line3(idx3),'k');
axis([-7 7 -7 7]);
```

生成了下面的图。

[Figure 5：三维分离的二维投影——横轴与纵轴都取 $[-6,6]$；`*`（星号）为 $X$ 数据、`o`（圆圈）为 $Y$ 数据、`+`（加号）为 $Z$ 数据；三条黑色直线两两相交围出一个三角区域，三类点分别落在这三条线划分出的三个扇形区域里]

（读图要点（译者补）：三条直线分别是 $f_1 = f_2$、$f_2 = f_3$、$f_3 = f_1$ 即三个两两相等的分界面。它们围成三角形，三块区域的「谁最大」关系在三角形内部和外部会互换——`idx` 那三行 `find` 正是在裁剪每条直线只保留真正构成分界的那一段。`p` 那个线性方程组求的是三条线的交点（「最混乱的点」）。

## 附加题 2 · Efficient numerical method for a regularized least-squares problem（正则化 least-squares 问题的高效数值解法）

考虑带 smoothing（平滑项）的 regularized least squares problem

```math
\text{minimize} \quad \sum_{i=1}^k (a_i^T x - b_i)^2 + \delta \sum_{i=1}^{n-1} (x_i - x_{i+1})^2 + \eta \sum_{i=1}^n x_i^2 ,
```

其中 $x \in \mathbf{R}^n$ 是变量，$\delta, \eta > 0$ 是参数。

**(a)** 把该问题的最优性条件表述为涉及 $x$ 的一组线性方程。（这称为 normal equations（正规方程）。）

**(b)** 现在假设 $k \ll n$。描述一种求解 (2a) 中 normal equations 的高效方法。给出不利用结构的通用方法的近似 flop 数，以及你这种高效方法的近似 flop 数。

**(c)** 数值实例。本小问你试用你的高效方法。取 $k = 100$、$n = 2000$、$\delta = \eta = 1$。首先按这些维度随机生成 $A$ 与 $b$。按 (2a) 构造 normal equations，用通用方法求解。接着写（简短的）代码实现你的高效方法，并在你的问题实例上运行。验证两种方法求得的解几乎相同，且你的高效方法比通用方法快得多。

注意：为确保从高效方法中得到加速，你需要了解一些 Matlab 的知识。你的方法应当涉及求解系数矩阵为 tridiagonal（三对角）的线性方程。在这种情况下，分解与回代都可以非常高效地进行。Matlab 文档说求解方程时会识别并利用 banded matrix（带状矩阵），但我们发现并非总是如此。为确保 Matlab 知道你的矩阵是三对角的，你可以用 `spdiags` 把矩阵声明为 sparse（稀疏），`spdiags` 可用来创建三对角矩阵。你也可以按常规方式创建三对角矩阵，然后用 `sparse` 把它转成稀疏矩阵。

还有一件你需要知道的事。假设你要解一组具有相同系数矩阵的线性方程，即你需要计算 $F^{-1}a_1,\dots,F^{-1}a_m$，其中 $F$ 可逆、$a_i$ 是列向量。把各列拼接起来，这可以表示成单个矩阵

```math
\big[ F^{-1}a_1\ \cdots\ F^{-1}a_m \big] = F^{-1}\big[ a_1\ \cdots\ a_m \big] .
```

要用 Matlab 计算这个矩阵，你应当把各右端项收集到一个矩阵中（如上），并用 Matlab 的反斜杠运算符：`F\A`。它会做正确的事：对矩阵 $F$ 只分解一次，并对各右端项做多次回代。

**Solution**。

**(a)** objective function 为

```math
x^T(A^TA + \delta\Delta + \eta I)x - 2b^TAx + b^Tb,
```

其中 $A \in \mathbf{R}^{k \times n}$ 是各行取 $a_i^T$ 的矩阵，$\Delta \in \mathbf{R}^{n \times n}$ 是三对角矩阵

```math
\Delta = \begin{bmatrix}
1 & -1 & 0 & \cdots & 0 & 0 & 0 \\
-1 & 2 & -1 & \cdots & 0 & 0 & 0 \\
0 & -1 & 2 & \cdots & 0 & 0 & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots & \vdots & \vdots \\
0 & 0 & 0 & \cdots & 2 & -1 & 0 \\
0 & 0 & 0 & \cdots & -1 & 2 & -1 \\
0 & 0 & 0 & \cdots & 0 & -1 & 1
\end{bmatrix}
```

由于问题无约束，最优性条件为

```math
(A^TA + \delta\Delta + \eta I)x^\star = A^Tb .
```

（式 (1)）（推导：$\delta\sum_{i=1}^{n-1}(x_i - x_{i+1})^2 = \delta x^T\Delta x$，其中 $\Delta$ 的每行是 $(x_i - x_{i+1})^2$ 展开后的系数——内点行是 $2$ 与两侧 $-1$，端点行只有 $1$ 与一个 $-1$。目标函数的梯度令为零即得 (1)。）

**(b)** 若完全不利用结构，解 (1) 约需 $(1/3)n^3$ flops。若 $k \ll n$，我们需要解 $Fx = g$，其中 $F$ 是三对角矩阵与一个（相对）低秩矩阵之和。可以用 Sherman-Morrison-Woodbury formula（SMW 公式）

```math
x^\star = (\delta\Delta + \eta I)^{-1}g - (\delta\Delta + \eta I)^{-1}A^T\big( I + A(\delta\Delta + \eta I)^{-1}A^T \big)^{-1}A(\delta\Delta + \eta I)^{-1}g
```

来高效地解 (1)，步骤如下：

**i.** 解 $(\delta\Delta + \eta I)z_1 = g$ 与 $(\delta\Delta + \eta I)Z_2 = A^T$，得到 $z_1$ 与 $Z_2$。由于 $\delta\Delta + \eta I$ 是三对角的，这总共约需 $4nk + 5n$ flops（$n$ 用于分解，$4n(k+1)$ 用于求解）。

**ii.** 构造 $Az_1$ 与 $AZ_2$（$2nk + 2nk^2$ flops）。

**iii.** 解 $(I + AZ_2)z_3 = Az_1$ 得到 $z_3$（$(1/3)k^3$ flops）。

**iv.** 构造 $x^\star = z_1 - Z_2z_3$（$2nk$ flops）。

总计 flop 数（只保留主项）为 $2nk^2$，当 $k \ll n$ 时远小于 $(1/3)n^3$。

（为什么能这样拆（译者补）：$F = P + A^TA$，其中 $P = \delta\Delta + \eta I$ 是三对角的、$A^TA$ 是秩至多 $k$ 的低秩项。SMW 把「解一个稠密的 $n \times n$ 系统」换成「解 $k+1$ 个三对角系统」+「解一个 $k \times k$ 系统」。`Z_0 = P\[g A']` 那一行就是用一次三对角分解处理 $k+1$ 个右端项，正是题目最后一段所讲的技巧。）

**(c)** 下面是 Matlab 代码：

```matlab
clear all; close all;
n = 2000;
k = 100;
delta = 1;
eta = 1;
A = rand(k,n);
b = rand(k,1);
e = ones(n,1);
D = spdiags([-e 2*e -e],[-1 0 1], n,n);
D(1,1) = 1; D(n,n) = 1;
I = speye(n);
F = A'*A + eta*I + delta*D;
P = eta*I + delta*D; %P is cheap to invert since it's tridiagonal
g = A'*b;
%Directly computing optimal solution
fprintf('\nComputing solution directly\n');
s1 = cputime;
x_gen = F\g;
s2 = cputime;
fprintf('Done (in %g sec)\n',s2-s1);
fprintf('\nComputing solution using efficient method\n');
%x_eff = P^{-1}g - P^{-1}A'(I +AP^{-1}A')^{-1}AP^{-1}g.
t1= cputime;
Z_0 = P\[g A'];
z_1 = Z_0(:,1);
%z_2 = A*z_1;
Z_2 = Z_0(:,2:k+1);
z_3 = (sparse(1:k,1:k,1) +A*Z_2)\(A*z_1);
x_eff = z_1 - Z_2*z_3;
t2 = cputime;
fprintf('Done (in %g sec)\n',t2-t1);
fprintf('\nrelative error = %e\n',norm(x_eff-x_gen)/norm(x_gen) );
```

（读代码要点（译者补）：`D(1,1) = 1; D(n,n) = 1;` 把端点从 $2$ 改成 $1$，正是 $\Delta$ 的首末两行；`sparse(1:k,1:k,1)` 是显式构造 $k \times k$ 单位矩阵（这样 `+A*Z_2` 之后仍是稀疏，避免稠密化）；`P\[g A']` 一次分解、$k+1$ 个右端项，这是 (b) 里 `i.` 那一步的实现；最后 `relative error` 应当在 $10^{-12}$ 量级，而两个 `cputime` 的差会相差一个量级以上——这就是 $\frac{1}{3}n^3 \approx 2.7\times10^9$ 与 $2nk^2 = 4\times10^7$ 的对比。）
