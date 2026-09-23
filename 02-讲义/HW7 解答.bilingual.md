> <span style="color:#7f8c8d;">Source: 作业解答/HW7 解答.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

<!-- p.1 -->

> <span style="color:#7f8c8d;">EE364a, Winter 2007-08 · Prof. S. Boyd（2007–08 学年冬季学期 · S. Boyd 教授）</span>

## <span style="color:#c0392b;">EE364a Homework 7 solutions（EE364a 作业 7 解答）</span>

### <span style="color:#c0392b;">8.16 Maximum volume rectangle inside a polyhedron（多面体内最大体积矩形）</span>

<span style="color:#2471a3;">**[exercise]**</span>

**8.16 Maximum volume rectangle inside a polyhedron.** Formulate the following problem as a convex optimization problem. Find the rectangle

```math
R = \{x \in \mathbb{R}^n \mid l \preceq x \preceq u\}
```

of maximum volume, enclosed in a polyhedron $P = \{x \mid Ax \preceq b\}$. The variables are $l, u \in \mathbb{R}^n$. Your formulation should not involve an exponential number of constraints.

<span style="color:#2471a3;">**[solution]**</span>

> 一种直接、但效率极低的表达约束 $R \subseteq P$ 的方法，是使用由 $m\,2^n$ 个不等式 $A v_i \preceq b$ 构成的集合，其中 $v_i$ 是 $R$ 的 $2^n$ 个顶点（corner）。（如果长方体（box）的所有顶点都在多面体内，那么整个长方体也一定在。）幸运的是，这个约束存在高效得多的表达方式。<span style="color:#00838f;">定义 $a^+_{ij} = \max\{a_{ij}, 0\}$，$a^-_{ij} = \max\{-a_{ij}, 0\}$，即取 $a_{ij}$ 的正部与负部。</span>那么，$R \subseteq P$ 当且仅当
>
> ```math
> \sum_{j=1}^{n}\left(a^+_{ij}u_j - a^-_{ij}l_j\right) \le b_i, \qquad i = 1, \ldots, m
> ```
>
> 最大体积矩形即为如下优化问题的解：
>
> ```math
> \begin{array}{ll}
> \text{maximize} & \left(\prod_{i=1}^{n}(u_i - l_i)\right)^{1/n} \\
> \text{subject to} & \sum_{j=1}^{n}\left(a^+_{ij}u_j - a^-_{ij}l_j\right) \le b_i, \qquad i = 1, \ldots, m,
> \end{array}
> ```
>
> 并带有隐式约束（implicit constraint）$u \succeq l$。对目标函数取对数，可得另一种表述：
>
> ```math
> \begin{array}{ll}
> \text{maximize} & \sum_{i=1}^{n}\log(u_i - l_i) \\
> \text{subject to} & \sum_{j=1}^{n}\left(a^+_{ij}u_j - a^-_{ij}l_j\right) \le b_i, \qquad i = 1, \ldots, m.
> \end{array}
> ```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这道题最值得咀嚼的是"逐坐标取最值"的分解技巧。线性函数 $\sum_j a_{ij}x_j$ 在长方体 $[l,u]$ 上取最大值时，由于可行集是各区间 $[l_j,u_j]$ 的笛卡尔积、目标函数在坐标之间不耦合，最大值可以逐坐标独立取得：系数为正就取上界 $u_j$，为负就取下界 $l_j$，这正是 $a^+_{ij}u_j-a^-_{ij}l_j$ 的来历。从第一性原理看，这一步把"验证 $2^n$ 个顶点"的指数级任务压缩成 $m$ 个线性不等式，与 6.006 的主线——利用问题结构把指数算法压成多项式算法——一脉相承。几何上，不等式左端恰是长方体在方向 $a_i$ 上的支撑函数（support function）取值，而"凸集包含 $\Leftrightarrow$ 各方向支撑函数不超过"本身就是分离判据。最后注意，目标 $(\prod_i(u_i-l_i))^{1/n}$ 取对数后变成可分的 $\sum_i\log(u_i-l_i)$：$\log$ 复合正的仿射函数是凹函数，约束又全是线性的，所以取对数后的表述是标准凸优化问题——这种"先取 log 再验证凹性"的手法在几何规划（geometric programming）里也是标配。

### <span style="color:#c0392b;">9.30 Gradient and Newton methods（梯度法与牛顿法）</span>

<span style="color:#2471a3;">**[exercise]**</span>

**9.30 Gradient and Newton methods.** Consider the unconstrained problem

```math
\text{minimize} \quad f(x) = -\sum_{i=1}^{m}\log(1 - a_i^T x) - \sum_{i=1}^{n}\log(1 - x_i^2),
```

with variable $x \in \mathbb{R}^n$, and $\mathbf{dom}\, f = \{x \mid a_i^T x < 1,\ i = 1, \ldots, m,\ |x_i| < 1,\ i = 1, \ldots, n\}$. This is the problem of computing the analytic center of the set of linear inequalities

```math
a_i^T x \le 1, \quad i = 1, \ldots, m, \qquad |x_i| \le 1, \quad i = 1, \ldots, n.
```

Note that we can choose $x^{(0)} = 0$ as our initial point. You can generate instances of this problem by choosing $a_i$ from some distribution on $\mathbb{R}^n$.

<!-- p.2 -->

**(a)** Use the gradient method to solve the problem, using reasonable choices for the backtracking parameters, and a stopping criterion of the form $\|\nabla f(x)\|_2 \le \eta$. Plot the objective function and step length versus iteration number. (Once you have determined $p^\star$ to high accuracy, you can also plot $f - p^\star$ versus iteration.) Experiment with the backtracking parameters $\alpha$ and $\beta$ to see their effect on the total number of iterations required. Carry these experiments out for several instances of the problem, of different sizes.

**(b)** Repeat using Newton's method, with stopping criterion based on the Newton decrement $\lambda^2$. Look for quadratic convergence. You do not have to use an efficient method to compute the Newton step, as in exercise 9.27; you can use a general purpose dense solver, although it is better to use one that is based on a Cholesky factorization.

**Hint.** Use the chain rule to find expressions for $\nabla f(x)$ and $\nabla^2 f(x)$.

<span style="color:#2471a3;">**[solution]**</span>

> **(a) 梯度法（gradient method）。** 图示给出了一个 $m = 200$、$n = 100$ 的算例中，函数值与步长（step length）随迭代次数变化的曲线。我们使用 $\alpha = 0.01$、$\beta = 0.5$，以及终止条件 $\|\nabla f(x^{(k)})\|_2 \le 10^{-3}$。
>
> <span style="color:#7f8c8d;">*图示说明：左图横轴为迭代数 $k$（0–500），纵轴为 $f(x^{(k)}) - p^\star$（对数刻度，$10^{-8}$ 至 $10^{4}$）；右图横轴为 $k$（0–600），纵轴为步长 $t^{(k)}$（0 至约 $0.016$）。*</span>
>
> Matlab 实现如下。
>
> ```matlab
> randn('state',1);
> m=200;
> n=100;
> ALPHA = 0.01;
> BETA = 0.5;
> MAXITERS = 1000;
> NTTOL    = 1e-8;
> GRADTOL  = 1e-3;
> ```
>
> <!-- p.3 -->
>
> ```matlab
> % generate random problem
> A = randn(m,n);
> % gradient method
> vals = [];
> steps = [];
> x = zeros(n,1);
> for iter = 1:MAXITERS
>     val = -sum(log(1-A*x)) - sum(log(1+x)) - sum(log(1-x));
>     vals = [vals, val];
>     d = 1./(1-A*x);
>     grad = A'*d - 1./(1+x) + 1./(1-x);
>     v = -grad;
>     fprime = grad'*v;
>     norm(grad)
>     if norm(grad) < GRADTOL, break; end;
>     t = 1;
>     while ((max(A*(x+t*v)) >= 1) | (max(abs(x+t*v)) >= 1)),
>         t = BETA*t;
>     end;
>     while ( -sum(log(1-A*(x+t*v))) - sum(log(1-(x+t*v).^2)) > ...
>             val + ALPHA*t*fprime )
>         t = BETA*t;
>     end;
>     x = x+t*v;
>     steps = [steps,t];
> end;
> figure(1)
> semilogy([0:(length(vals)-2)], vals(1:length(vals)-1)-optval, '-');
> xlabel('x');
> ylabel('z');
> figure(2)
> plot([1:length(steps)], steps, ':',[1:length(steps)], steps, 'o');
> xlabel('x');
> ylabel('z');
> ```
>
> **(b) 牛顿法（Newton's method）。** 图示给出了同一算例中函数值与步长随迭代次数变化的曲线。我们使用 $\alpha = 0.01$、$\beta = 0.5$，以及终止条件 $\lambda(x^{(k)})^2 \le 10^{-8}$。
>
> <!-- p.4 -->
>
> <span style="color:#7f8c8d;">*图示说明：左图横轴为 $k$（0–7），纵轴为 $f(x^{(k)}) - p^\star$（对数刻度，$10^{-10}$ 至 $10^{5}$）；右图横轴为 $k$（0–8），纵轴为步长 $t^{(k)}$（0 至 1）。仅约 7 次迭代就收敛，与题目要求的"观察二次收敛（quadratic convergence）"相符。*</span>
>
> Matlab 实现如下。
>
> ```matlab
> % Newton method
> vals = [];
> steps = [];
> x = zeros(n,1);
> for iter = 1:MAXITERS
>     val = -sum(log(1-A*x)) - sum(log(1+x)) - sum(log(1-x));
>     vals = [vals, val];
>     d = 1./(1-A*x);
>     grad = A'*d - 1./(1+x) + 1./(1-x);
>     hess = A'*diag(d.^2)*A + diag(1./(1+x).^2 + 1./(1-x).^2);
>     v = -hess\grad;
>     fprime = grad'*v
>     if abs(fprime) < NTTOL, break; end;
>     t = 1;
>     while ((max(A*(x+t*v)) >= 1) | (max(abs(x+t*v)) >= 1)),
>         t = BETA*t;
>     end;
>     while ( -sum(log(1-A*(x+t*v))) - sum(log(1-(x+t*v).^2)) > ...
>             val + ALPHA*t*fprime )
>         t = BETA*t;
>     end;
>     x = x+t*v;
> ```
>
> <!-- p.5 -->
>
> ```matlab
>     steps = [steps,t];
> end;
> optval = vals(length(vals));
> figure(3)
> semilogy([0:(length(vals)-2)], vals(1:length(vals)-1)-optval, '-', ...
> [0:(length(vals)-2)], vals(1:length(vals)-1)-optval, 'o');
> xlabel('x');
> ylabel('z');
> figure(4)
> plot([1:length(steps)], steps, '-', [1:length(steps)], steps, 'o');
> axis([0, length(steps), 0, 1.1]);
> xlabel('x');
> ylabel('z');
> ```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 目标函数中的 $-\log(1-u)$ 值得多看一眼。它一方面是标准的对数障碍（log barrier），把约束 $u<1$ "软化"成定义域边界；另一方面，若 $U\sim\mathrm{Exp}(1)$，则其矩母函数（moment generating function）为 $\mathbb{E}[e^{tU}]=1/(1-t)$，于是 $-\log(1-t)=\log\mathbb{E}[e^{tU}]$ 恰是矩母函数取对数（即累积量生成函数）——这正是你正在学的 Bertsekas 概率论里"矩母函数在原点附近完全刻画分布"这一结论的凸优化变装。整个 $f$ 的凸性可以直接验证：$-\log$ 是凸的且递减，复合正的仿射函数 $1-a_i^Tx$ 后仍凸；$-\log(1-x_i^2)$ 拆成 $-\log(1-x_i)-\log(1+x_i)$ 后同理。"解析中心"（analytic center）这个名字的由来，是它等价于在一组不等式围成的区域内做某种对数似然最大化，得到的点"离每条边界都尽量远"，与取在极点上的顶点解形成鲜明对比。也正因为定义域包含原点，$x^{(0)}=0$ 是天然安全的初始点。

### <span style="color:#c0392b;">9.31 Some approximate Newton methods（若干近似牛顿方法）</span>

<span style="color:#2471a3;">**[exercise]**</span>

**9.31 Some approximate Newton methods.** The cost of Newton's method is dominated by the cost of evaluating the Hessian $\nabla^2 f(x)$ and the cost of solving the Newton system. For large problems, it is sometimes useful to replace the Hessian by a positive definite approximation that makes it easier to form and solve for the search step. In this problem we explore some common examples of this idea.

For each of the approximate Newton methods described below, test the method on some instances of the analytic centering problem described in exercise 9.30, and compare the results to those obtained using the Newton method and gradient method.

**(a) Re-using the Hessian.** We evaluate and factor the Hessian only every $N$ iterations, where $N > 1$, and use the search step $\Delta x = -H^{-1}\nabla f(x)$, where $H$ is the last Hessian evaluated. (We need to evaluate and factor the Hessian once every $N$ steps; for the other steps, we compute the search direction using back and forward substitution.)

**(b) Diagonal approximation.** We replace the Hessian by its diagonal, so we only have to evaluate the $n$ second derivatives $\partial^2 f(x)/\partial x_i^2$, and computing the search step is very easy.

<span style="color:#2471a3;">**[solution]**</span>

> **(a)** 图示给出了函数值与近似总浮点运算次数（flops）之间的关系（算例与习题 9.30 解答中的相同），分别对应 $N = 1$（即牛顿法）、$N = 15$ 和 $N = 30$。
>
> <!-- p.6 -->
>
> <span style="color:#7f8c8d;">*图示说明：横轴为浮点运算次数（# of flops，0 至约 $2.5\times 10^{6}$），纵轴为 $f(x^{(k)}) - p^\star$（对数刻度，$10^{-10}$ 至 $10^{5}$），三条曲线分别标注 Newton、$N=15$、$N=30$。*</span>
>
> 可以看出，若以真实代价（即所需浮点运算次数）来度量，"每隔 $N$ 步复用一次已分解的 Hessian（海塞矩阵）"的方法确实提高了收敛速度。当然，按迭代次数衡量，该方法比基本牛顿法更差。
>
> Matlab 实现如下。
>
> ```matlab
> randn('state',1);
> m=200;
> n=100;
> ALPHA    = 0.01;
> BETA     = 0.5;
> MAXITERS = 1000;
> NTTOL    = 1e-9;
> GRADTOL  = 1e-3;
> % generate random problem
> A = randn(m,n);
> % Newton method with periodically updated Hessian
> for N = [1,15,30];
>     % re-compute Hessian every N iterations
>     vals  = [];
>     flops = [];
>     flop  = 0;
>     x = zeros(n,1);
>     for iter = 1:MAXITERS
>         val = -sum(log(1-A*x))-sum(log(1+x))-sum(log(1-x));
>         vals  = [vals,val];
> ```
>
> <!-- p.7 -->
>
> ```matlab
>         flops = [flops,flop];
>         d = 1./(1-A*x);
>         grad = A'*d-1./(1+x)+1./(1-x);
>         if (rem(iter-1,N) == 0)
>             H = A'*diag(d.^2)*A+diag(1./(1+x).^2+1./(1-x).^2);
>             L = chol(H,'lower');
>             flop = (1/3)*n^3; % add flop for Cholesky factorization
>         else
>             flop = 0;
>         end
>         v = -L'\(L\grad);
>         flop = flop+2*n^2; % add flop for fwd/bwd substitution
>         fprime = grad'*v
>         if (abs(fprime) < NTTOL) break; end
>         t = 1;
>         while ((max(A*(x+t*v))>=1) | (max(abs(x+t*v))> 1)),
>             t = BETA*t;
>         end
>         while (-sum(log(1-A*(x+t*v)))-sum(log(1-(x+t*v).^2)) > ...
>                val + ALPHA*t*fprime )
>             t = BETA*t;
>         end
>         x = x+t*v;
>     end
>     if (N==1), optval = vals(length(vals)); end
>     figure(1)
>     cflops = cumsum(flops(1:end-1));
>     perror = vals(1:end-1)-optval;
>     semilogy(cflops,perror,'-',cflops,perror,'o');
>     hold on;
>     semilogy(cflops(1:N:end-1),perror(1:N:end-1),...
>     'mo','MarkerEdgeColor','k','MarkerFaceColor','b','MarkerSize',8);
>     text(cflops(end),perror(end),['N',num2str(N)]);
>     hold on;
> end
> ```
>
> <!-- p.8 -->
>
> ```matlab
> xlabel('x');
> ylabel('z');
> ```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 按浮点运算次数（flops）而非迭代次数画收敛曲线，是数值线性代数里衡量"真实代价"的基本功。回忆 Strang 线代里的一组数：稠密对称正定矩阵的 Cholesky 分解约需 $(1/3)n^3$ 次浮点运算，而三角前代/回代各约 $n^2$ 次——"分解贵、复用便宜"，这正是每 $N$ 步才重新分解一次 Hessian 能省时间的根本原因；代码中的 `flop = (1/3)*n^3` 与 `flop = flop+2*n^2` 就是在手工维护这个代价模型。从 CSAPP 的视角看，flops 只是运行时间的代理指标：缓存命中率、Matlab 按列主序存储矩阵等系统因素同样影响实测速度，理论计数与实测时间出现偏差时不必惊讶。"花一次大代价做分解，换后续多次便宜的代换"的权衡，与 6.006 里"预处理换查询"的思想完全同构。

> **(b)** 图示给出了（与习题 9.30 解答相同算例下）对 Hessian 取对角近似（diagonal approximation）时，函数值随迭代次数的变化。实验表明，该算法的收敛行为与梯度法非常相似。
>
> <span style="color:#7f8c8d;">*图示说明：横轴为 $k$（0–800），纵轴为 $f(x^{(k)}) - p^\star$（对数刻度，$10^{-8}$ 至 $10^{4}$），收敛轨迹与梯度法接近。*</span>
>
> Matlab 实现如下。
>
> ```matlab
> % Newton method with diagonal approximation of Hessian
> vals = [];
> x = zeros(n,1);
> for iter = 1:MAXITERS
>     val = -sum(log(1-A*x)) - sum(log(1+x)) - sum(log(1-x));
>     vals = [vals, val];
>     d = 1./(1-A*x);
>     grad = A'*d - 1./(1+x) + 1./(1-x);
>     hess = A'*diag(d.^2)*A + diag(1./(1+x).^2 + 1./(1-x).^2);
>     H = diag(diag(hess));
>     norm(grad)
>     if norm(grad) < GRADTOL, break; end;
>     v = -H\grad;
>     fprime = grad'*v;
>     if (fprime > 0), keyboard; end;
>     t = 1;
>     while ((max(A*(x+t*v)) >= 1) | (max(abs(x+t*v)) >= 1)),
>         t = BETA*t;
>     end
>     while ( -sum(log(1-A*(x+t*v))) - sum(log(1-(x+t*v).^2)) > ...
>             val + ALPHA*t*fprime )
> ```
>
> <!-- p.9 -->
>
> ```matlab
>         t = BETA*t;
>     end
>     x = x+t*v;
> end
> figure(2)
> semilogy([0:(length(vals)-2)], vals(1:length(vals)-1)-optval, '-');
> xlabel('x');
> ylabel('z');
> ```

<!-- p.10 -->

### <span style="color:#c0392b;">Solutions to additional exercises（附加习题解答）</span>

<span style="color:#2471a3;">**[section]**</span>

#### <span style="color:#c0392b;">Suggestions for exercise 9.30（习题 9.30 的建议）</span>

<span style="color:#2471a3;">**[section]**</span>

> 我们建议用如下方式生成问题实例：
>
> ```matlab
> n = 100;
> m = 200;
> randn('state',1);
> A=randn(m,n);
> ```
>
> 当然，你也应当用不同的维数和不同的数据来测试你的代码。在任何情况下，都务必保证你的线搜索（line search）首先找到一个使试探点落在 $\mathbf{dom}\, f$ 内的步长；如果你在 $f$ 的定义域之外求值，会得到复数，而且再也无法恢复。
>
> 要求出 $\nabla f(x)$ 与 $\nabla^2 f(x)$ 的表达式，请使用链式法则（chain rule，见附录 A.4）；如果你试图直接计算 $\partial^2 f(x)/\partial x_i \partial x_j$，你会后悔的。
>
> 计算牛顿步时，可以使用 `vnt=-H\g`。

#### <span style="color:#c0392b;">Suggestions for exercise 9.31（习题 9.31 的建议）</span>

<span style="color:#2471a3;">**[section]**</span>

> 对于 9.31a，你应当尝试 $N = 1$、$N = 15$ 和 $N = 30$。你不妨计算并保存 Hessian 的 Cholesky 分解（Cholesky factorization），然后通过回代（back solve）得到搜索方向——尽管对这么小的问题，你在 Matlab 里并不会看到明显的加速。求出 Hessian 后，可以用 `L=chol(H,'lower')` 得到其 Cholesky 分解；随后用 `-L'\(L\g)` 计算搜索步，其中 $g$ 是当前点的梯度。Matlab 会做正确的事：先用前代（forward substitution）解 `L\g`，再用回代（backward substitution）解 `-L'\(L\g)`。每次代换的代价是 $n^2$ 量级。
>
> 要公平地比较三种方法（即 $N = 1$、$N = 15$、$N = 30$）的收敛性，横轴应当显示近似的总浮点运算次数，而不是迭代次数。你可以按每次分解 $n^3/3$、每次求解 $2n^2$ 来估算浮点次数（其中每次"求解"包含一步前代与一步回代）。

#### <span style="color:#c0392b;">Additional exercises（附加习题）</span>

<span style="color:#2471a3;">**[section]**</span>

#### <span style="color:#c0392b;">1. Three-way linear classification（三路线性分类）</span>

<span style="color:#2471a3;">**[exercise]**</span>

**1. Three-way linear classification.** We are given data

```math
x^{(1)}, \ldots, x^{(N)}, \qquad y^{(1)}, \ldots, y^{(M)}, \qquad z^{(1)}, \ldots, z^{(P)},
```

three nonempty sets of vectors in $\mathbb{R}^n$. We wish to find three affine functions on $\mathbb{R}^n$,

```math
f_i(z) = a_i^T z - b_i, \qquad i = 1, 2, 3,
```

<!-- p.11 -->

that satisfy the following properties:

```math
\begin{aligned}
f_1(x^{(j)}) &> \max\{f_2(x^{(j)}),\, f_3(x^{(j)})\}, \qquad j = 1, \ldots, N,\\
f_2(y^{(j)}) &> \max\{f_1(y^{(j)}),\, f_3(y^{(j)})\}, \qquad j = 1, \ldots, M,\\
f_3(z^{(j)}) &> \max\{f_1(z^{(j)}),\, f_2(z^{(j)})\}, \qquad j = 1, \ldots, P.
\end{aligned}
```

In words: $f_1$ is the largest of the three functions on the $x$ data points, $f_2$ is the largest of the three functions on the $y$ data points, $f_3$ is the largest of the three functions on the $z$ data points. We can give a simple geometric interpretation: The functions $f_1$, $f_2$, and $f_3$ partition $\mathbb{R}^n$ into three regions,

```math
\begin{aligned}
R_1 &= \{z \mid f_1(z) > \max\{f_2(z),\, f_3(z)\}\},\\
R_2 &= \{z \mid f_2(z) > \max\{f_1(z),\, f_3(z)\}\},\\
R_3 &= \{z \mid f_3(z) > \max\{f_1(z),\, f_2(z)\}\},
\end{aligned}
```

defined by where each function is the largest of the three. Our goal is to find functions with $x^{(j)} \in R_1$, $y^{(j)} \in R_2$, and $z^{(j)} \in R_3$.

Pose this as a convex optimization problem. You may not use strict inequalities in your formulation.

Solve the specific instance of the 3-way separation problem given in `sep3way_data.m`, with the columns of the matrices X, Y and Z giving the $x^{(j)}$, $j = 1, \ldots, N$, $y^{(j)}$, $j = 1, \ldots, M$ and $z^{(j)}$, $j = 1, \ldots, P$. To save you the trouble of plotting data points and separation boundaries, we have included the plotting code in `sep3way_data.m`. (Note that `a1`, `a2`, `a3`, `b1` and `b2` contain arbitrary numbers; you should compute the correct values using cvx.)

<span style="color:#2471a3;">**[solution]**</span>

> 不等式组
>
> ```math
> \begin{aligned}
> f_1(x^{(j)}) &> \max\{f_2(x^{(j)}),\, f_3(x^{(j)})\}, \qquad j = 1, \ldots, N,\\
> f_2(y^{(j)}) &> \max\{f_1(y^{(j)}),\, f_3(y^{(j)})\}, \qquad j = 1, \ldots, M,\\
> f_3(z^{(j)}) &> \max\{f_1(z^{(j)}),\, f_2(z^{(j)})\}, \qquad j = 1, \ldots, P
> \end{aligned}
> ```
>
> 关于 $a_i$ 与 $b_i$ 是齐次的（homogeneous），因此可以把它们表达为
>
> ```math
> \begin{aligned}
> f_1(x^{(j)}) &\ge \max\{f_2(x^{(j)}),\, f_3(x^{(j)})\} + 1, \qquad j = 1, \ldots, N,\\
> f_2(y^{(j)}) &\ge \max\{f_1(y^{(j)}),\, f_3(y^{(j)})\} + 1, \qquad j = 1, \ldots, M,\\
> f_3(z^{(j)}) &\ge \max\{f_1(z^{(j)}),\, f_2(z^{(j)})\} + 1, \qquad j = 1, \ldots, P.
> \end{aligned}
> ```
>
> 注意，我们可以给每个 $a_i$ 加上任一向量 $\alpha$ 而不影响这些不等式（它们只涉及各个 $a_i$ 之间的差），同理也可以给每个 $b_i$ 加上任一数 $\beta$。利用这一观察，可以对 $a_i$ 与 $b_i$ 作归一化或化简。例如，可以不失一般性地假设 $a_1 + a_2 + a_3 = 0$ 且 $b_1 + b_2 + b_3 = 0$。
>
> 下面的脚本实现了这一三方分类方法，并在一个小型可分数据集上进行了测试。
>
> <!-- p.12 -->
>
> ```matlab
> clear all; close all;
> % data for problem instance
> M = 20;
> N = 20;
> P = 20;
> X = [
>     3.5674  4.1253  2.8535  5.1892  4.3273  3.8133  3.4117 ...
>     3.8636  5.0668  3.9044  4.2944  4.7143  3.3082  5.2540 ...
>     2.5590  3.6001  4.8156  5.2902  5.1908  3.9802 ;...
>    -2.9981  0.5178  2.1436 -0.0677  0.3144  1.3064  3.9297 ...
>     0.2051  0.1067 -1.4982 -2.4051  2.9224  1.5444 -2.8687 ...
>     1.0281  1.2420  1.2814  1.2035 -2.1644 -0.2821];
> Y = [
>    -4.5665 -3.6904 -3.2881 -1.6491 -5.4731 -3.6170 -1.1876 ...
>    -1.0539 -1.3915 -2.0312 -1.9999 -0.2480 -1.3149 -0.8305 ...
>    -1.9355 -1.0898 -2.6040 -4.3602 -1.8105  0.3096; ...
>     2.4117  4.2642  2.8460  0.5250  1.9053  2.9831  4.7079 ...
>     0.9702  0.3854  1.9228  1.4914 -0.9984  3.4330  2.9246 ...
>     3.0833  1.5910  1.5266  1.6256  2.5037  1.4384];
> Z = [
>     1.7451  2.6345  0.5937 -2.8217  3.0304  1.0917 -1.7793 ...
>     1.2422  2.1873 -2.3008 -3.3258  2.7617  0.9166  0.0601 ...
>    -2.6520 -3.3205  4.1229 -3.4085 -3.1594 -0.7311; ...
>    -3.2010 -4.9921 -3.7621 -4.7420 -4.1315 -3.9120 -4.5596 ...
>    -4.9499 -3.4310 -4.2656 -6.2023 -4.5186 -3.7659 -5.0039 ...
>    -4.3744 -5.0559 -3.9443 -4.0412 -5.3493 -3.0465];
> cvx_begin
>     variables a1(2) a2(2) a3(2) b1 b2 b3
>     a1'*X-b1 >= max(a2'*X-b2,a3'*X-b3)+1;
>     a2'*Y-b2 >= max(a1'*Y-b1,a3'*Y-b3)+1;
>     a3'*Z-b3 >= max(a1'*Z-b1,a2'*Z-b2)+1;
>     a1 + a2 + a3 == 0
>     b1 + b2 + b3 == 0
> cvx_end
> % now let's plot the three-way separation induced by
> % a1,a2,a3,b1,b2,b3
> ```
>
> <!-- p.13 -->
>
> ```matlab
> % find maximally confusing point
> p = [(a1-a2)';(a1-a3)']\[(b1-b2);(b1-b3)];
> % plot
> t = [-7:0.01:7];
> u1 = a1-a2; u2 = a2-a3; u3 = a3-a1;
> v1 = b1-b2; v2 = b2-b3; v3 = b3-b1;
> line1 = (-t*u1(1)+v1)/u1(2); idx1 = find(u2'*[t;line1]-v2>0);
> line2 = (-t*u2(1)+v2)/u2(2); idx2 = find(u3'*[t;line2]-v3>0);
> line3 = (-t*u3(1)+v3)/u3(2); idx3 = find(u1'*[t;line3]-v1>0);
> plot(X(1,:),X(2,:),'*',Y(1,:),Y(2,:),'ro',Z(1,:),Z(2,:),'g+',...
> t(idx1),line1(idx1),'k',t(idx2),line2(idx2),'k',t(idx3),line3(idx3),'k');
> axis([-7 7 -7 7]);
> ```
>
> <span style="color:#7f8c8d;">*图示说明：运行脚本生成下图，横纵坐标范围均为 $[-7,7]$（刻度从 $-6$ 到 $6$）；`X` 数据用星号、`Y` 数据用红色圆圈、`Z` 数据用绿色加号绘制，黑色线段为 $f_1{=}f_2$、$f_2{=}f_3$、$f_3{=}f_1$ 的两两边界在各区域内的可见部分。*</span>

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 把严格不等式改成"不小于对方加 1"，是支持向量机（support vector machine）里经典的间隔（margin）归一化手法。关键在于齐次性：把每个 $(a_i,b_i)$ 整体放大同一个 $t>0$，所有 $f_i$ 同步放大，"谁最大"的序关系不变，因此间隔可以任意伸缩——固定为 1 不失一般性，却把开集条件 $\{>\}$ 换成了闭集约束 $\{\ge\}$；凸优化的可行集需要闭性，这一步正是让问题"能进求解器"的临门一脚，题目禁止严格不等式的原因也在于此。另外，"给每个 $a_i$ 加同一向量、每个 $b_i$ 加同一数不影响不等式"说明解带有整体平移自由度（不等式只依赖 $a_i$ 之差与 $b_i$ 之差），固定 $a_1+a_2+a_3=0$、$b_1+b_2+b_3=0$ 就是把这个自由度钉死，与线性代数里给解空间挑一个规范化代表元是同一件事；不加这个约束，求解器面对一族等价解可能出现数值上的任意漂移。

#### <span style="color:#c0392b;">2. Efficient numerical method for a regularized least-squares problem（正则化最小二乘问题的高效数值方法）</span>

<span style="color:#2471a3;">**[exercise]**</span>

**2. Efficient numerical method for a regularized least-squares problem.** We consider a regularized least squares problem with smoothing,

```math
\text{minimize} \quad \sum_{i=1}^{k}(a_i^T x - b_i)^2 + \delta\sum_{i=1}^{n-1}(x_i - x_{i+1})^2 + \eta\sum_{i=1}^{n}x_i^2,
```

where $x \in \mathbb{R}^n$ is the variable, and $\delta, \eta > 0$ are parameters.

**(a)** Express the optimality conditions for this problem as a set of linear equations involving $x$. (These are called the normal equations.)

<!-- p.14 -->

**(b)** Now assume that $k \ll n$. Describe an efficient method to solve the normal equations found in (2a). Give an approximate flop count for a general method that does not exploit structure, and also for your efficient method.

**(c) A numerical instance.** In this part you will try out your efficient method. We'll choose $k = 100$ and $n = 2000$, and $\delta = \eta = 1$. First, randomly generate A and b with these dimensions. Form the normal equations as in (2a), and solve them using a generic method. Next, write (short) code implementing your efficient method, and run it on your problem instance. Verify that the solutions found by the two methods are nearly the same, and also that your efficient method is much faster than the generic one.

**Note:** You'll need to know some things about Matlab to be sure you get the speedup from the efficient method. Your method should involve solving linear equations with tridiagonal coefficient matrix. In this case, both the factorization and the back substitution can be carried out very efficiently. The Matlab documentation says that banded matrices are recognized and exploited, when solving equations, but we found this wasn't always the case. To be sure Matlab knows your matrix is tridiagonal, you can declare the matrix as sparse, using `spdiags`, which can be used to create a tridiagonal matrix. You could also create the tridiagonal matrix conventionally, and then convert the resulting matrix to a sparse one using `sparse`.

One other thing you need to know. Suppose you need to solve a group of linear equations with the same coefficient matrix, i.e., you need to compute $F^{-1}a_1, \ldots, F^{-1}a_m$, where F is invertible and $a_i$ are column vectors. By concatenating columns, this can be expressed as a single matrix

```math
\begin{bmatrix} F^{-1}a_1 & \cdots & F^{-1}a_m \end{bmatrix} = F^{-1}\begin{bmatrix} a_1 & \cdots & a_m \end{bmatrix}.
```

To compute this matrix using Matlab, you should collect the righthand sides into one matrix (as above) and use Matlab's backslash operator: `F\A`. This will do the right thing: factor the matrix F once, and carry out multiple back substitutions for the righthand sides.

<span style="color:#2471a3;">**[solution]**</span>

> **(a)** 目标函数为
>
> ```math
> x^T(A^TA + \delta\Delta + \eta I)x - 2b^TAx + b^Tb,
> ```
>
> 其中 $A \in \mathbb{R}^{k\times n}$ 是以 $a_i$ 为行向量的矩阵，$\Delta \in \mathbb{R}^{n\times n}$ 是如下三对角矩阵（tridiagonal matrix）
>
> <!-- p.15 -->
>
> ```math
> \Delta =
> \left[\begin{array}{ccccccc}
> 1 & -1 & 0 & \cdots & 0 & 0 & 0\\
> -1 & 2 & -1 & \cdots & 0 & 0 & 0\\
> 0 & -1 & 2 & \cdots & 0 & 0 & 0\\
> \vdots & \vdots & \vdots & \ddots & \vdots & \vdots & \vdots\\
> 0 & 0 & 0 & \cdots & 2 & -1 & 0\\
> 0 & 0 & 0 & \cdots & -1 & 2 & -1\\
> 0 & 0 & 0 & \cdots & 0 & -1 & 1
> \end{array}\right].
> ```
>
> 由于问题是无约束的，最优性条件（optimality conditions）为
>
> ```math
> (A^TA + \delta\Delta + \eta I)\,x^\star = A^Tb. \qquad (1)
> ```
>
> **(b)** 若不利用任何结构，求解 (1) 大约需要 $(1/3)n^3$ 次浮点运算。若 $k \ll n$，我们需要求解方程组 $Fx = g$，其中 $F$ 是一个三对角矩阵与一个（相对）低秩（low-rank）矩阵之和。此时可以用 Sherman-Morrison-Woodbury 公式（Sherman-Morrison-Woodbury formula）
>
> ```math
> x^\star = (\delta\Delta + \eta I)^{-1}g \;-\; (\delta\Delta + \eta I)^{-1}A^T\left(I + A(\delta\Delta + \eta I)^{-1}A^T\right)^{-1}A(\delta\Delta + \eta I)^{-1}g
> ```
>
> 来高效地求解 (1)，具体步骤如下：
>
> i. 求解 $(\delta\Delta + \eta I)z_1 = g$ 与 $(\delta\Delta + \eta I)Z_2 = A^T$，得到 $z_1$ 与 $Z_2$。由于 $\delta\Delta + \eta I$ 是三对角的，这一步的总代价约为 $4nk + 5n$ 次浮点运算（$n$ 用于分解，$4n(k+1)$ 用于求解）。
>
> ii. 计算 $Az_1$ 与 $AZ_2$（$2nk + 2nk^2$ 次浮点运算）。
>
> iii. 求解 $(I + AZ_2)z_3 = Az_1$ 得到 $z_3$（$(1/3)k^3$ 次浮点运算）。
>
> iv. 计算 $x^\star = z_1 - Z_2z_3$（$2nk$ 次浮点运算）。
>
> 只保留主导项，总浮点运算次数为 $2nk^2$，当 $k \ll n$ 时远小于 $(1/3)n^3$。
>
> **(c)** Matlab 代码如下：
>
> ```matlab
> clear all; close all;
> n = 2000;
> k = 100;
> delta = 1;
> eta = 1;
> A = rand(k,n);
> b = rand(k,1);
> e = ones(n,1);
> D = spdiags([-e 2*e -e],[-1 0 1], n,n);
> ```
>
> <!-- p.16 -->
>
> ```matlab
> D(1,1) = 1; D(n,n) = 1;
> I = speye(n);
> F = A'*A + eta*I + delta*D;
> P = eta*I + delta*D; %P is cheap to invert since it's tridiagonal
> g = A'*b;
> %Directly computing optimal solution
> fprintf('\nComputing solution directly\n');
> s1 = cputime;
> x_gen = F\g;
> s2 = cputime;
> fprintf('Done (in %g sec)\n',s2-s1);
> fprintf('\nComputing solution using efficient method\n');
> %x_eff = P^{-1}g - P^{-1}A'(I +AP^{-1}A')^{-1}AP^{-1}g.
> t1= cputime;
> Z_0 = P\[g A'];
> z_1 = Z_0(:,1);
> %z_2 = A*z_1;
> Z_2 = Z_0(:,2:k+1);
> z_3 = (sparse(1:k,1:k,1) +A*Z_2)\(A*z_1);
> x_eff = z_1 - Z_2*z_3;
> t2 = cputime;
> fprintf('Done (in %g sec)\n',t2-t1);
> fprintf('\nrelative error = %e\n',norm(x_eff-x_gen)/norm(x_gen) );
> ```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> Sherman-Morrison-Woodbury 公式是"低秩修正不必重算逆"的代数化身，也是 Strang 线代（18.065）反复强调的核心思想：$F = P + A^TA$ 中三对角的 $P$ 分解只需 $O(n)$ 代价，而低秩部分 $A^TA$ 携带的信息本质上只有 $k$ 维，于是把 $k\times k$ 的小系统交给稠密方法处理即可。总代价 $2nk^2$ 对 $(1/3)n^3$ 的碾压式优势，就是"利用结构"四个字的量化版本，与 6.006 中"稠密图 $O(V^2)$、稀疏图用堆优化到 $O(E\log V)$"的结构性差距同源。代码里两个工程细节值得记住：其一，用 `spdiags` 显式声明稀疏三对角矩阵，否则 Matlab 不一定自动利用带状结构（题目 Note 里抱怨的正是这一点）；其二，多右端项拼成矩阵一次求解 `P\[g A']`，让分解只做一次、代换服务多组右端——这正是 18.065 里"分解一次、多次求解"的 LU 用法。另外注意正规方程 $A^TA$ 的构造会把条件数平方放大，本例数值实验能通过，靠的是 $\delta\Delta+\eta I$ 提供的正则化。
