# EE364a HW7_bonus 解答（作业 7 附加题 解答）

> 本文件为 Stanford EE364a（Convex Optimization，Prof. S. Boyd，Winter 2007-08）Homework 7 **附加题（additional problems）** 官方解答的**行内双语译注 + 完整解答**。
> 约定：中文为主体，英文术语以 `English（中文）` 形式在首次出现时标注，之后只保留 `English`。
> 原文 PDF 公式有损，均已按上下文还原为标准 LaTeX；OCR 模糊处以 `〔…待核〕` 标出，不臆造。
> 格式：`$...$` 用于行内公式，独立公式一律用 ```math 围栏，不使用 ``，不使用 Unicode 上下标。

---

## 0. 关于作业 9.30 的编程建议（Suggestions for exercise 9.30）

**题干：** We recommend the following to generate a problem instance:

```
n = 100;
m = 200;
randn('state',1);
A = randn(m,n);
```

**中文：** 我们推荐用以下方式生成一个问题实例（problem instance）：

```
n = 100;
m = 200;
randn('state',1);
A = randn(m,n);
```

> 说明：这里 `randn('state',1)` 固定了 MATLAB 随机数发生器（random number generator）的种子（seed），使 `A = randn(m,n)` 每次运行都产生同一个 $m\times n$ 的标准正态（standard normal）矩阵 $A$，从而结果可复现（reproducible）。当然，你应该用不同的维数（dimensions）以及不同的数据来测试你的代码。

**题干：** In all cases, be sure that your line search first finds a step length for which the tentative point is in dom f; if you attempt to evaluate f outside its domain, you'll get complex numbers, and you'll never recover.

**中文：** 在所有情形下，务必让你的线搜索（line search）**首先**找到一个步长（step length），使得试探点（tentative point）落在 $f$ 的定义域（domain，记作 $\operatorname{dom} f$）之内；如果你试图在定义域之外计算 $f$，你会得到复数（complex numbers），而且再也无法恢复（recover）。

> **要点：** 在含对数障碍（log barrier）$\log$ 项的问题里，一旦自变量越过定义域边界，`log` 会对负数取对数而返回复数，导致整个迭代崩溃。所以线搜索的**回退（backtracking）**必须先保证候选点在 $\operatorname{dom} f$ 内，再去比较函数值。

**题干：** To find expressions for ∇f(x) and ∇$^{2}$f(x), use the chain rule (see Appendix A.4); if you attempt to compute ∂$^{2}$f(x)/∂xᵢ∂xⱼ, you will be sorry.

**中文：** 求 $\nabla f(x)$（梯度，gradient）和 $\nabla^2 f(x)$（Hessian 矩阵，Hessian）的表达式时，请使用链式法则（chain rule，见附录 A.4）；如果你试图直接逐项去算 $\dfrac{\partial^2 f(x)}{\partial x_i\partial x_j}$，你会吃大亏。

> **要点：** 复合函数（composite function）$f(x)=g(h(x))$ 的梯度和 Hessian 应写成
> ```math
> \nabla f(x)=J_h(x)^{\top}\nabla g\bigl(h(x)\bigr),
> ```
> ```math
> \nabla^2 f(x)=J_h(x)^{\top}\nabla^2 g\bigl(h(x)\bigr)J_h(x)+\sum_k \frac{\partial g}{\partial h_k}\bigl(h(x)\bigr)\,\nabla^2 h_k(x),
> ```
> 其中 $J_h(x)$ 是 $h$ 的 Jacobian 矩阵。直接对每个 $(i,j)$ 分量求二阶偏导既繁琐又易错。

**题干：** To compute the Newton step, you can use `vnt = -H\g`.

**中文：** 计算牛顿步（Newton step）时，你可以用 MATLAB 的 `vnt = -H\g`。

> 即解线性方程组 $H\,\Delta x_{\mathrm{nt}} = -g$，其中 $g=\nabla f(x)$，$H=\nabla^2 f(x)$。MATLAB 的左除 `\` 会自动求解该线性方程组。

---

## 0′. 关于作业 9.31 的编程建议（Suggestions for exercise 9.31）

**题干：** For 9.31a, you should try out N = 1, N = 15, and N = 30. You might as well compute and store the Cholesky factorization of the Hessian, and then back solve to get the search directions, even though you won't really see any speedup in Matlab for such a small problem. After you evaluate the Hessian, you can find the Cholesky factorization as `L = chol(H,'lower')`.

**中文：** 对 9.31a，你应该分别尝试 $N=1$、$N=15$ 和 $N=30$。你最好先计算并存储 Hessian 的 Cholesky 分解（Cholesky factorization），然后通过回代（back solve）得到搜索方向（search directions），尽管对这么小的问题你在 MATLAB 里其实看不到什么加速。算出 Hessian 之后，可以用 `L = chol(H,'lower')` 得到它的 Cholesky 分解。

**题干：** You can then compute a search step as `-L'\(L\g)`, where g is the gradient at the current point. Matlab will do the right thing, i.e., it will first solve L\g using forward substitution, and then it will solve -L'\(L\g) using backward substitution. Each substitution is order n$^{2}$.

**中文：** 之后你可以用 `-L'\(L\g)` 计算一个搜索步（search step），其中 $g$ 是当前点的梯度。MATLAB 会自动做正确的事：它先用前向代入（forward substitution）解 `L\g`，再用后向代入（backward substitution）解 `-L'\(L\g)`。每一次代入都是 $O(n^2)$ 量级。

> **推导（为什么是这一步）：** 牛顿步满足 $H\Delta x = -g$，而 $H=LL^{\top}$（$L$ 为下三角，$L^{\top}$ 为上三角）。于是
> ```math
> LL^{\top}\Delta x = -g \quad\Longrightarrow\quad L^{\top}\Delta x = L^{-1}(-g) \quad\Longrightarrow\quad \Delta x = (L^{\top})^{-1}L^{-1}(-g).
> ```
> MATLAB 中 `L\g` 即 $L^{-1}g$（前向代入），`L'\(L\g)` 即 $(L^{\top})^{-1}(L^{-1}g)$（后向代入），再加负号即得 $\Delta x$。因为三角矩阵一次前向/后向代入只需 $O(n^2)$ 次浮点运算，所以总代价远小于直接对稠密 $H$ 求逆。

**题干：** To fairly compare the convergence of the three methods (i.e., N = 1, N = 15, N = 30), the horizontal axis should show the approximate total number of flops required, and not the number of iterations. You can compute the approximate number of flops using n$^{3}$/3 for each factorization, and 2n$^{2}$ for each solve (where each 'solve' involves a forward substitution step and a backward substitution step).

**中文：** 为了公平比较三种方法（即 $N=1,15,30$）的收敛性（convergence），横轴应该显示所需的**近似浮点运算总数（total number of flops）**，而不是迭代次数（number of iterations）。你可以用 $n^3/3$ 估算每次分解（factorization）的浮点运算数，用 $2n^2$ 估算每次求解（solve）的浮点运算数（这里每次 “solve” 包含一次前向代入和一次后向代入）。

> **要点：** 三种方法每次迭代代价不同，只看迭代次数会误导。横轴用累计 flops 能把三条收敛曲线放在同一尺度上比较。

---

## 题目 1（Problem 1）：Three-way linear classification（三向线性分类）

**题干：** Three-way linear classification. We are given data

```math
x^{(1)},\dots,x^{(N)},\qquad y^{(1)},\dots,y^{(M)},\qquad z^{(1)},\dots,z^{(P)},
```

three nonempty sets of vectors in $\mathbb{R}^n$. We wish to find three affine functions on $\mathbb{R}^n$,

```math
f_i(z)=a_i^{\top}z-b_i,\qquad i=1,2,3,
```

**中文：** 三向线性分类（three-way linear classification）。给定三组数据

```math
x^{(1)},\dots,x^{(N)},\qquad y^{(1)},\dots,y^{(M)},\qquad z^{(1)},\dots,z^{(P)},
```

它们是 $\mathbb{R}^n$ 中的三个非空向量集（nonempty sets of vectors）。我们希望找到 $\mathbb{R}^n$ 上的三个仿射函数（affine functions）

```math
f_i(z)=a_i^{\top}z-b_i,\qquad i=1,2,3,
```

**题干：** that satisfy the following properties:

```math
f_1(x^{(j)}) > \max\{f_2(x^{(j)}), f_3(x^{(j)})\},\qquad j=1,\dots,N,
```

```math
f_2(y^{(j)}) > \max\{f_1(y^{(j)}), f_3(y^{(j)})\},\qquad j=1,\dots,M,
```

```math
f_3(z^{(j)}) > \max\{f_1(z^{(j)}), f_2(z^{(j)})\},\qquad j=1,\dots,P.
```

**中文：** 且满足以下性质：

```math
f_1(x^{(j)}) > \max\{f_2(x^{(j)}), f_3(x^{(j)})\},\qquad j=1,\dots,N,
```

```math
f_2(y^{(j)}) > \max\{f_1(y^{(j)}), f_3(y^{(j)})\},\qquad j=1,\dots,M,
```

```math
f_3(z^{(j)}) > \max\{f_1(z^{(j)}), f_2(z^{(j)})\},\qquad j=1,\dots,P.
```

**题干：** In words: f₁ is the largest of the three functions on the x data points, f₂ is the largest of the three functions on the y data points, f₃ is the largest of the three functions on the z data points.

**中文：** 换句话说：在 $x$ 数据点上，$f_1$ 是三个函数中最大的；在 $y$ 数据点上，$f_2$ 是最大的；在 $z$ 数据点上，$f_3$ 是最大的。

**题干：** We can give a simple geometric interpretation: The functions f₁, f₂, and f₃ partition ℝⁿ into three regions,

```math
R_1=\{z \mid f_1(z)>\max\{f_2(z),f_3(z)\}\},
```

```math
R_2=\{z \mid f_2(z)>\max\{f_1(z),f_3(z)\}\},
```

```math
R_3=\{z \mid f_3(z)>\max\{f_1(z),f_2(z)\}\},
```

defined by where each function is the largest of the three. Our goal is to find functions with $x^{(j)}\in R_1$, $y^{(j)}\in R_2$, and $z^{(j)}\in R_3$.

**中文：** 我们可以给出一个简单的几何解释（geometric interpretation）：$f_1,f_2,f_3$ 把 $\mathbb{R}^n$ 划分成三个区域（regions）

```math
R_1=\{z \mid f_1(z)>\max\{f_2(z),f_3(z)\}\},
```

```math
R_2=\{z \mid f_2(z)>\max\{f_1(z),f_3(z)\}\},
```

```math
R_3=\{z \mid f_3(z)>\max\{f_1(z),f_2(z)\}\},
```

即按“每个函数在哪一块区域最大”来定义。我们的目标是找到这样的函数，使 $x^{(j)}\in R_1$、$y^{(j)}\in R_2$、$z^{(j)}\in R_3$。

**题干：** Pose this as a convex optimization problem. You may not use strict inequalities in your formulation.

**中文：** 把这个问题建模（pose）成一个凸优化问题（convex optimization problem）。在你的建模中**不允许使用严格不等式（strict inequalities）**。

**题干：** Solve the specific instance of the 3-way separation problem given in `sep3way_data.m`, with the columns of the matrices X, Y and Z giving the $x^{(j)}, j=1,\dots,N$, $y^{(j)}, j=1,\dots,M$ and $z^{(j)}, j=1,\dots,P$. To save you the trouble of plotting data points and separation boundaries, we have included the plotting code in `sep3way_data.m`. (Note that a₁, a₂, a₃, b₁ and b₂ contain arbitrary numbers; you should compute the correct values using cvx.)

**中文：** 求解 `sep3way_data.m` 中给出的三向分离问题（3-way separation problem）的具体实例（specific instance），其中矩阵 $X,Y,Z$ 的各列分别给出 $x^{(j)},\ j=1,\dots,N$、$y^{(j)},\ j=1,\dots,M$ 和 $z^{(j)},\ j=1,\dots,P$。为了省去你绘制数据点和分离边界（separation boundaries）的麻烦，我们已在 `sep3way_data.m` 中附上了绘图代码。（注意：文件里给出的 $a_1,a_2,a_3,b_1,b_2$ 是任意数〔原文此处只列了 $a_1,a_2,a_3,b_1,b_2$，疑漏 $b_3$，待核〕；你应该用 cvx 计算出正确的值。）

---

### 解答（Problem 1）

**目标：** 把“严格大于”的分离条件改写成**非严格**且**凸**的约束。关键是用**齐次（homogeneous）**处理规避仿射函数 $f_i=a_i^{\top}z-b_i$ 中截距带来的尺度自由度。

**Step 1 — 变量定义。** 设优化变量为 $a_1,a_2,a_3\in\mathbb{R}^n$ 与 $b_1,b_2,b_3\in\mathbb{R}$。条件

```math
f_1(x^{(j)})>\max\{f_2(x^{(j)}),f_3(x^{(j)})\}
```

等价于**两个**严格不等式：

```math
a_1^{\top}x^{(j)}-b_1 > a_2^{\top}x^{(j)}-b_2,
\qquad
a_1^{\top}x^{(j)}-b_1 > a_3^{\top}x^{(j)}-b_3 .
```

即

```math
(a_1-a_2)^{\top}x^{(j)} - (b_1-b_2) > 0,
\qquad
(a_1-a_3)^{\top}x^{(j)} - (b_1-b_3) > 0 .
```

**Step 2 — 去掉严格号（为什么可以）。** 题目禁止严格不等式。处理办法是给每个严格不等式右侧留一个**正的下界**：要求

```math
f_1(x^{(j)})-\max\{f_2(x^{(j)}),f_3(x^{(j)})\} \ge 1,
```

或更简洁地，要求

```math
(a_1-a_2)^{\top}x^{(j)}-(b_1-b_2)\ge 1,
\qquad
(a_1-a_3)^{\top}x^{(j)}-(b_1-b_3)\ge 1 .
```

> **为什么右侧能取 1 而不失一般性：** 观察仿射函数 $f_i=a_i^{\top}z-b_i$ 整体乘一个正数 $\alpha>0$ 不改变“哪个 $f_i$ 最大”的判定（因为 $\alpha f_1>\alpha\max\{f_2,f_3\}\iff f_1>\max\{f_2,f_3\}$，$\alpha>0$）。也就是说分离问题关于 $(a_i,b_i)$ 是**尺度不变（scale-invariant）**的。因此总可以把满足分离条件的解缩放到使间隔（margin）至少为 1。这样，严格不等式 “$>0$” 就能安全地替换为非严格不等式 “$\ge 1$”，得到一个凸约束。

**Step 3 — 写出完整凸问题。** 对所有 $j$ 施加三组对称的约束，即：

- 对 $x$ 数据（应落入 $R_1$）：
  ```math
  (a_1-a_2)^{\top}x^{(j)}-(b_1-b_2)\ge 1,\qquad
  (a_1-a_3)^{\top}x^{(j)}-(b_1-b_3)\ge 1,\quad j=1,\dots,N.
  ```
- 对 $y$ 数据（应落入 $R_2$）：
  ```math
  (a_2-a_1)^{\top}y^{(j)}-(b_2-b_1)\ge 1,\qquad
  (a_2-a_3)^{\top}y^{(j)}-(b_2-b_3)\ge 1,\quad j=1,\dots,M.
  ```
- 对 $z$ 数据（应落入 $R_3$）：
  ```math
  (a_3-a_1)^{\top}z^{(j)}-(b_3-b_1)\ge 1,\qquad
  (a_3-a_2)^{\top}z^{(j)}-(b_3-b_2)\ge 1,\quad j=1,\dots,P.
  ```

于是问题可以写成一个线性规划（linear program, LP）——例如最小化一个任意常数（或无目标函数，即 feasibility problem，可行性问题）：

```math
\begin{aligned}
\text{minimize} &\quad 0 \\
\text{subject to} &\quad (a_1-a_2)^{\top}x^{(j)}-(b_1-b_2)\ge 1,\quad j=1,\dots,N,\\
&\quad (a_1-a_3)^{\top}x^{(j)}-(b_1-b_3)\ge 1,\quad j=1,\dots,N,\\
&\quad (a_2-a_1)^{\top}y^{(j)}-(b_2-b_1)\ge 1,\quad j=1,\dots,M,\\
&\quad (a_2-a_3)^{\top}y^{(j)}-(b_2-b_3)\ge 1,\quad j=1,\dots,M,\\
&\quad (a_3-a_1)^{\top}z^{(j)}-(b_3-b_1)\ge 1,\quad j=1,\dots,P,\\
&\quad (a_3-a_2)^{\top}z^{(j)}-(b_3-b_2)\ge 1,\quad j=1,\dots,P,
\end{aligned}
```

变量为 $a_1,a_2,a_3\in\mathbb{R}^n$，$b_1,b_2,b_3\in\mathbb{R}$。

> **凸性判据（显式写出）：** 所有约束都是关于变量 $(a_i,b_i)$ 的**仿射不等式（affine inequalities）**，形如 $\langle c,\,(a,b)\rangle \ge d$。仿射不等式定义的是**半空间（halfspace）**，而半空间是凸集；有限多个凸集的交集仍是凸集。因此可行域（feasible set）是凸集（更具体地，是一个多面体 polyhedron），目标函数为常数 0（凸），故这是凸优化问题。

**Step 4 — cvx 求解。** 用 cvx 直接写（示意）：

```matlab
cvx_begin
    variables a1(n) a2(n) a3(n) b1 b2 b3
    minimize( 0 )
    subject to
        (a1 - a2)'*X - (b1 - b2) >= 1;
        (a1 - a3)'*X - (b1 - b3) >= 1;
        (a2 - a1)'*Y - (b2 - b1) >= 1;
        (a2 - a3)'*Y - (b2 - b3) >= 1;
        (a3 - a1)'*Z - (b3 - b1) >= 1;
        (a3 - a2)'*Z - (b3 - b2) >= 1;
cvx_end
```

这里 `X`、`Y`、`Z` 分别是 $n\times N$、$n\times M$、$n\times P$ 的数据矩阵。得到的 $a_i,b_i$ 代入 $f_i(z)=a_i^{\top}z-b_i$ 即为所求的三个仿射分类函数，且 $x^{(j)}\in R_1$、$y^{(j)}\in R_2$、$z^{(j)}\in R_3$。

[[FIGURE: 无图，本附加题原文未附分离结果图；具体实例的分离结果由 `sep3way_data.m` 内置绘图代码画出]]

---

## 题目 2（Problem 2）：Efficient numerical method for a regularized least-squares problem（正则化最小二乘问题的高效数值方法）

**题干：** We consider a regularized least squares problem with smoothing,

```math
\text{minimize}\quad \sum_{i=1}^{k}(a_i^{\top}x-b_i)^2 + \delta\sum_{i=1}^{n-1}(x_i-x_{i+1})^2 + \eta\sum_{i=1}^{n}x_i^2,
```

where $x\in\mathbb{R}^n$ is the variable, and $\delta,\eta>0$ are parameters.

**中文：** 考虑一个带平滑（smoothing）的正则化最小二乘问题（regularized least squares problem）：

```math
\text{minimize}\quad \sum_{i=1}^{k}(a_i^{\top}x-b_i)^2 + \delta\sum_{i=1}^{n-1}(x_i-x_{i+1})^2 + \eta\sum_{i=1}^{n}x_i^2,
```

其中 $x\in\mathbb{R}^n$ 是变量（variable），$\delta,\eta>0$ 是参数（parameters）。

> 说明：第一项是拟合项（data-fitting / least-squares term），$a_i\in\mathbb{R}^n$、$b_i\in\mathbb{R}$ 是给定数据；第二项 $\delta\sum(x_i-x_{i+1})^2$ 是平滑项（smoothing / regularization），惩罚相邻分量之间的差异；第三项 $\eta\sum x_i^2$ 是二次正则（quadratic regularization / Tikhonov），保证问题有唯一解。

### 2(a) — 最优性条件与正规方程

**题干：** (a) Express the optimality conditions for this problem as a set of linear equations involving x. (These are called the normal equations.)

**中文：** (a) 把该问题的最优性条件（optimality conditions）写成关于 $x$ 的一组线性方程。（这些方程称为正规方程，normal equations。）

**解答：** 设 $A\in\mathbb{R}^{k\times n}$ 的第 $i$ 行为 $a_i^{\top}$，$b=(b_1,\dots,b_k)\in\mathbb{R}^k$。则拟合项可写作 $\|Ax-b\|_2^2$。目标函数

```math
f(x)=\|Ax-b\|_2^2 + \delta\,\|Dx\|_2^2 + \eta\,\|x\|_2^2,
```

其中 $D\in\mathbb{R}^{(n-1)\times n}$ 是一阶差分矩阵（difference matrix）：

```math
D=\begin{bmatrix}
1&-1&0&\cdots&0\\
0&1&-1&\cdots&0\\
\vdots&\ddots&\ddots&\ddots&\vdots\\
0&\cdots&0&1&-1
\end{bmatrix},
\qquad
(Dx)_i = x_i - x_{i+1},\ i=1,\dots,n-1.
```

**梯度（gradient）计算（逐步写出）：**

- 对第一项：$\nabla_x\|Ax-b\|_2^2 = 2A^{\top}(Ax-b)$。
- 对第二项：$\nabla_x\|Dx\|_2^2 = 2D^{\top}Dx$。
- 对第三项：$\nabla_x\|x\|_2^2 = 2x$。

因此

```math
\nabla f(x)=2A^{\top}(Ax-b)+2\delta D^{\top}Dx+2\eta x .
```

**最优性条件（显式写出）：** 目标函数是强凸（strongly convex）的二次函数（因为 $\eta>0$ 使 Hessian 正定，见下），故最优解唯一，且由一阶必要条件（也是充分条件）$\nabla f(x)=0$ 刻画：

```math
\nabla f(x)=0 \iff 2A^{\top}(Ax-b)+2\delta D^{\top}Dx+2\eta x=0 .
```

除以 2 并整理，得**正规方程（normal equations）**：

```math
\bigl(A^{\top}A+\delta D^{\top}D+\eta I\bigr)\,x = A^{\top}b .
```

记系数矩阵 $G = A^{\top}A+\delta D^{\top}D+\eta I$（注意 $G$ 为 $n\times n$ 对称矩阵），则

```math
Gx = A^{\top}b .
```

> **Hessian 正定性判据（显式写出）：** 目标函数的 Hessian 为
> ```math
> \nabla^2 f(x)=2\bigl(A^{\top}A+\delta D^{\top}D+\eta I\bigr)=2G .
> ```
> 其中 $A^{\top}A$、$D^{\top}D$ 半正定（positive semidefinite, PSD），$\eta I$ 正定（positive definite, PD），故 $G\succ 0$（正定）。Hessian 处处正定 $\Rightarrow$ $f$ 强凸 $\Rightarrow$ 唯一全局最优解，且 $\nabla f(x)=0$ 是其充要条件（necessary and sufficient condition）。这与 KKT/无约束最优性完全一致（本问题无约束，KKT 条件退化为 $\nabla f(x)=0$）。

### 2(b) — 高效求解方法与浮点运算量分析

**题干：** (b) Now assume that k ≪ n. Describe an efficient method to solve the normal equations found in (2a). Give an approximate flop count for a general method that does not exploit structure, and also for your efficient method.

**中文：** (b) 现在假设 $k\ll n$。描述一个求解 (2a) 中正规方程的高效方法（efficient method）。给出不利用结构（exploit structure）的通用方法的近似浮点运算量（flop count），以及你的高效方法的近似浮点运算量。

**解答：**

**观察矩阵结构：** 正规方程的系数矩阵

```math
G = A^{\top}A + \delta D^{\top}D + \eta I .
```

- $D^{\top}D$ 是**三对角（tridiagonal）**矩阵：$D$ 是一阶差分算子，$D^{\top}D$ 的对角元为 $2$（端点处为 $1$），上下次对角元为 $-1$。即

```math
D^{\top}D=\begin{bmatrix}
1&-1&0&\cdots&0\\
-1&2&-1&\ddots&\vdots\\
0&-1&2&\ddots&0\\
\vdots&\ddots&\ddots&\ddots&-1\\
0&\cdots&0&-1&1
\end{bmatrix}.
```

- $I$ 是单位阵（对角）。
- $A^{\top}A$ 当 $k\ll n$ 时是**低秩（low-rank）**矩阵：$\operatorname{rank}(A^{\top}A)\le k\ll n$，且 $A^{\top}A=\sum_{i=1}^{k}a_i a_i^{\top}$。

因此 $G$ 等于“一个三对角矩阵（$T:=\delta D^{\top}D+\eta I$）加上一个低秩修正（$A^{\top}A$，秩至多 $k$）”。

**高效方法（Matrix Inversion Lemma / Woodbury 公式）：** 令

```math
T = \delta D^{\top}D + \eta I \quad(\text{三对角、且 } \eta>0 \text{ 保证 } T\succ 0),
```

则

```math
G = T + A^{\top}A = T + A^{\top}(I_k)A .
```

由 Woodbury 矩阵恒等式（matrix inversion lemma），对可逆的 $T$：

```math
\bigl(T+A^{\top}A\bigr)^{-1} = T^{-1} - T^{-1}A^{\top}\bigl(I_k + A\,T^{-1}A^{\top}\bigr)^{-1}A\,T^{-1} .
```

于是解 $Gx=A^{\top}b$ 可写为

```math
x = G^{-1}(A^{\top}b) = T^{-1}A^{\top}b - T^{-1}A^{\top}\bigl(I_k + A\,T^{-1}A^{\top}\bigr)^{-1}A\,T^{-1}A^{\top}b .
```

**具体算法步骤：**

1. 对三对角矩阵 $T$ 做 Cholesky 分解 $T=LL^{\top}$，或用 Thomas 算法；三对角矩阵的分解只需 $O(n)$ 次 flops。
2. 计算 $W=T^{-1}A^{\top}\in\mathbb{R}^{n\times k}$：这相当于对 $k$ 个右端列（$A^{\top}$ 的各列）分别解三对角方程组，共 $k$ 次 $O(n)$ 的回代，总 $O(kn)$。
3. 形成 $k\times k$ 小矩阵 $S=I_k + A\,W$（$A\,W=A\,T^{-1}A^{\top}$ 为 $k\times k$）。
4. 解小系统 $S\,y = A\,T^{-1}A^{\top}b$，代价 $O(k^3)$（对 $k\times k$ 矩阵做分解）。
5. 组合 $x = T^{-1}A^{\top}b - W\,y$。

**浮点运算量对比（flop count）：**

- **通用方法（不利用结构）：** 直接对 $n\times n$ 的 $G$ 做 Cholesky 分解并回代，代价约为 $n^3/3$ 次 flops（分解主导）。
- **高效方法：** 三对角 $T$ 的分解 $O(n)$，$k$ 次三对角回代 $O(kn)$，$k\times k$ 系统的分解 $O(k^3)$。总代价约为 $O(kn + k^3)$。

当 $k\ll n$ 时，$O(kn+k^3)\ll O(n^3)$，例如 $k$ 很小（如 $k\sim 100$）时，$k^3$ 项相对 $n^3$ 可忽略，加速显著。

> **关键判据回顾：** 这里利用了两点结构——$D^{\top}D$ 的**带状/三对角（banded/tridiagonal）**结构使 $T^{-1}(\cdot)$ 可 $O(n)$ 完成，$A^{\top}A$ 的**低秩（low-rank）**结构使 Woodbury 公式把 $n\times n$ 求逆转化为 $k\times k$ 求逆。二者叠加即得高效算法。

### 2(c) — 数值实例

**题干：** (c) A numerical instance. In this part you will try out your efficient method. We'll choose k = 100 and n = 2000, and δ = η = 1. First, randomly generate A and b with these dimensions. Form the normal equations as in (2a), and solve them using a generic method. Next, write (short) code implementing your efficient method, and run it on your problem instance. Verify that the solutions found by the two methods are nearly the same, and also that your efficient method is much faster than the generic one.

**中文：** (c) 一个数值实例（numerical instance）。本部分你将实际测试你的高效方法。取 $k=100$、$n=2000$、$\delta=\eta=1$。首先按这些维数随机生成 $A$ 和 $b$。按 (2a) 构造正规方程，并用通用方法（generic method）求解。接着写（简短的）代码实现你的高效方法，并在你的问题实例上运行。验证两种方法求出的解几乎相同（nearly the same），并且你的高效方法比通用方法快得多（much faster）。

**题干：** Note: You'll need to know some things about Matlab to be sure you get the speedup from the efficient method. Your method should involve solving linear equations with tridiagonal coefficient matrix. In this case, both the factorization and the back substitution can be carried out very efficiently. The Matlab documentation says that banded matrices are recognized and exploited, when solving equations, but we found this wasn't always the case. To be sure Matlab knows your matrix is tridiagonal, you can declare the matrix as sparse, using spdiags, which can be used to create a tridiagonal matrix. You could also create the tridiagonal matrix conventionally, and then convert the resulting matrix to a sparse one using sparse.

**中文：** 注意：你需要了解一些 MATLAB 的细节，才能确保从高效方法中得到加速。你的方法应涉及求解三对角系数矩阵（tridiagonal coefficient matrix）的线性方程。此时分解（factorization）与回代（back substitution）都能非常高效地完成。MATLAB 文档声称求解方程时会识别并利用带状矩阵（banded matrices），但我们发现并非总是如此。为确保 MATLAB 知道你的矩阵是三对角的，可以用 `spdiags` 把矩阵声明为稀疏（sparse）——`spdiags` 能用来构造三对角矩阵；也可以先按常规方式构造三对角矩阵，再用 `sparse` 把它转换为稀疏矩阵。

**题干：** One other thing you need to know. Suppose you need to solve a group of linear equations with the same coefficient matrix, i.e., you need to compute F⁻$^{1}$a₁, ..., F⁻$^{1}$aₘ, where F is invertible and aᵢ are column vectors. By concatenating columns, this can be expressed as a single matrix [F⁻$^{1}$a₁ ··· F⁻$^{1}$aₘ] = F⁻$^{1}$[a₁ ··· aₘ]. To compute this matrix using Matlab, you should collect the righthand sides into one matrix (as above) and use Matlab's backslash operator: F\A. This will do the right thing: factor the matrix F once, and carry out multiple back substitutions for the righthand sides.

**中文：** 你还需要知道一件事。假设你要解一组具有相同系数矩阵的线性方程，即要计算 $F^{-1}a_1,\dots,F^{-1}a_m$，其中 $F$ 可逆，$a_i$ 是列向量（column vectors）。通过把列拼接（concatenating columns），这可表示为单个矩阵

```math
\bigl[F^{-1}a_1\ \cdots\ F^{-1}a_m\bigr] = F^{-1}[a_1\ \cdots\ a_m].
```

要在 MATLAB 中计算这个矩阵，你应把各个右端项（righthand sides）收集到一个矩阵里（如上），然后使用 MATLAB 的左除运算符（backslash operator）`F\A`。这会做正确的事：只对矩阵 $F$ 分解一次（factor once），再对多个右端项做多次回代（multiple back substitutions）。

---

### 解答要点小结（Problem 2）

- **(2a)** 正规方程为 $Gx=A^{\top}b$，其中 $G=A^{\top}A+\delta D^{\top}D+\eta I$，由 $\nabla f(x)=0$ 得到；$G\succ 0$ 保证唯一解。
- **(2b)** 利用 $G=T+A^{\top}A$（$T=\delta D^{\top}D+\eta I$ 三对角，$A^{\top}A$ 低秩），通过 Woodbury 公式把 $n\times n$ 求逆转为 $k\times k$ 求逆：通用方法 $O(n^3)$，高效方法 $O(kn+k^3)$。
- **(2c)** $k=100$、$n=2000$ 下两法解几乎一致，高效法显著更快；MATLAB 中需用 `spdiags`/`sparse` 显式声明三对角稀疏性，并用 `F\A` 一次分解、多次回代。
