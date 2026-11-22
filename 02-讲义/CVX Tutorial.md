# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

> <span style="color:#7f8c8d;">Source: 补充材料/CVX Tutorial.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

## <span style="color:#c0392b;">CVX tutorial session（CVX 教程课）</span>

> <span style="color:#7f8c8d;">EE364A · Stanford University（斯坦福大学）· January 22, 2015（2015 年 1 月 22 日）</span>

<!-- p.1 -->

### <span style="color:#c0392b;">CVX tutorial session（CVX 教程课）</span>

<span style="color:#2471a3;">**[lecture]**</span> 本讲义是 Stanford EE364A《Convex Optimization（凸优化）》课程的 CVX 上机教程（tutorial session），主题是如何使用 CVX 建模系统描述并求解凸优化问题（convex optimization problem）。

<!-- p.2 -->

### <span style="color:#c0392b;">What is CVX?（CVX 是什么？）</span>

<span style="color:#2471a3;">**[section]**</span> CVX 是一个面向凸优化问题（convex optimization problem）的**建模系统（modeling system）**：你用接近数学书写的语法声明目标函数（objective function）与约束（constraint），CVX 负责验证问题的凸性，并调用底层求解器（solver）完成数值计算。

- 官网：<http://cvxr.com/cvx>

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> CVX 的核心设计思路是把"建模"与"求解"解耦：建模层用贴近数学教材的语法接收问题，先验证它确实是凸优化问题，再自动改写为底层锥求解器（如 SeDuMi、SDPT3）可执行的标准形式。为什么非要先验证"凸"？回忆 6.006 里的复杂度视角：一般非凸优化问题和许多 NP 难问题一样棘手（例如在非凸约束中可以嵌入子集和这类组合难题），而凸问题存在多项式时间算法，内点法（interior-point method）在实践中几乎总能稳定、高效地收敛。换言之，"凸"正是"可在多项式时间内求解"的结构证书，CVX 的职责就是在你按下回车之前，替你检查这张证书是否有效。它采用的验证框架叫规范凸规划（disciplined convex programming, DCP）：把表达式拆解为已知的凸/凹/仿射原子函数，再按合成规则逐层检查，思想与编译器的静态类型检查同源。代价是框架偏保守——某些整体上凸但写法"不规范"的表达式会被拒收，需要你手动改写成等价的凸表示。

> <span style="color:#7f8c8d;">（原 slides 页码：2）</span>

<!-- p.3 -->

### <span style="color:#c0392b;">Installation on Corn（在 Corn 服务器上安装）</span>

<span style="color:#2471a3;">**[section]**</span> Corn 是 Stanford 的共享计算服务器。在终端（terminal）中依次执行：

```bash
$ ssh -Y SUNETID@corn.stanford.edu
$ wget http://web.cvxr.com/cvx/cvx-a64.tar.gz
$ tar -xvf cvx-a64.tar.gz
$ module load matlab
$ matlab &
```

进入 MATLAB 后执行：

```matlab
>> cd cvx
>> cvx_setup
```

> <span style="color:#7f8c8d;">（原 slides 页码：4）</span>

<!-- p.4 -->

### <span style="color:#c0392b;">Structure of convex problem（凸优化问题的结构）</span>

<span style="color:#2471a3;">**[section]**</span> 数学形式（mathematically）：

<span style="color:#2471a3;">**[definition]**</span>

```math
\begin{array}{ll}
\text{minimize} & f_0(x) \\
\text{subject to} & f_i(x) \le 0, \quad i = 1, \ldots, m \\
& h_i(x) = 0, \quad i = 1, \ldots, p
\end{array}
```

即：极小化目标函数 $f_0(x)$，受 $m$ 个不等式约束（inequality constraint）$f_i(x)\le 0$ 与 $p$ 个等式约束（equality constraint）$h_i(x)=0$ 限制（minimize 意为"极小化"，subject to 意为"受约束于"）。

对应的 CVX 代码：

```matlab
cvx_begin
    variables x(n)
    minimize(f0(x))
    subject to
        f(x) <= 0
        A * x - b == 0
cvx_end
```

<span style="color:#00838f;">注意（NB）：$f_0$ 与 $f_i$ 必须是凸的（convex），$h_i$ 必须是仿射的（affine）。</span>

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 为什么标准形式要求目标 $f_0$ 与不等式约束 $f_i$ 是凸函数、等式约束 $h_i$ 是仿射函数？从第一性原理看，凸性的价值在于"局部极小即全局极小"：假如存在两个不同的极小点，凸函数定义 $f(\theta x+(1-\theta)y)\le \theta f(x)+(1-\theta)f(y)$（Jensen 不等式）立刻说明两点连线上的函数值不会翘起来，全局最优值（optimal value）因此不会被局部陷阱隐藏。等式约束限定为仿射同样关键：仿射函数既凸又凹，所以可行集是若干超平面（hyperplane）的交。用 Strang 线代的语言说，$Ax=b$ 的解集正是零空间 $\mathcal{N}(A)$ 平移一个特解得到的仿射集（affine set）——线性子空间"不过原点的版本"；若允许一般非线性等式约束，可行集会弯曲，凸性立刻被破坏。这些要求合起来保证可行集是凸集（convex set），进而 KKT 条件（KKT conditions）不仅必要而且充分，后面学的对偶理论、中心路径（central path）与内点法（interior-point method）整套大厦都奠基于这块地基。

> <span style="color:#7f8c8d;">（原 slides 页码：6）</span>

<!-- p.5 -->

### <span style="color:#c0392b;">Return values（返回值）</span>

<span style="color:#2471a3;">**[section]**</span> 求解结束后（upon exit），CVX 会设置以下变量：

- `x` — 解变量（solution variable）$x^\star$；
- `cvx_optval` — 最优值（optimal value）$p^\star$；
- `cvx_status` — 求解器状态（solver status）：Solved（已求解）、Unbounded（无界）、Infeasible（不可行）等；
- ……

> <span style="color:#7f8c8d;">（原 slides 页码：7）</span>

<!-- p.6 -->

### <span style="color:#c0392b;">Examples – Basic（例题——基础）</span>

<span style="color:#2471a3;">**[section]**</span> <span style="color:#2471a3;">**[example]**</span> 优化问题（optimization problem）：

```math
\begin{array}{ll}
\text{minimize} & x + y \\
\text{subject to} & x \ge 1 \\
& y = 2
\end{array}
```

CVX 代码：

```matlab
cvx_begin
    variables x(1) y(1)
    minimize(x + y)
    subject to
        x >= 1
        y == 2
cvx_end
```

> <span style="color:#7f8c8d;">（原 slides 页码：9）</span>

<!-- p.7 -->

### <span style="color:#c0392b;">Examples – Basic（例题——基础：返回结果）</span>

<span style="color:#2471a3;">**[section]**</span> <span style="color:#2471a3;">**[example]**</span> CVX 返回一个解（solution）与求解状态（status）：

```matlab
>> x
1
>> y
2
>> cvx_optval
3
>> cvx_status
Solved
```

即最优解为 $x=1$、$y=2$，最优值为 $3$，状态为 Solved（已求解）。

> <span style="color:#7f8c8d;">（原 slides 页码：10）</span>

<!-- p.8 -->

### <span style="color:#c0392b;">Examples – LP（例题——线性规划）</span>

<span style="color:#2471a3;">**[section]**</span> <span style="color:#2471a3;">**[example]**</span> 优化问题：

```math
\begin{array}{ll}
\text{maximize} & c^T x \\
\text{subject to} & A x = b \\
& x \succeq 0
\end{array}
```

这是一个线性规划（linear program, LP）：线性目标 $c^T x$，等式约束 $Ax=b$ 与向量分量非负约束 $x\succeq 0$。

CVX 代码：

```matlab
cvx_begin
    variables x(n)
    maximize(c' * x)
    subject to
        A * x == b
        x >= 0
cvx_end
```

> <span style="color:#7f8c8d;">（原 slides 页码：11）</span>

<!-- p.9 -->

### <span style="color:#c0392b;">Examples – SDP（例题——半定规划）</span>

<span style="color:#2471a3;">**[section]**</span> <span style="color:#2471a3;">**[example]**</span> 优化问题：

```math
\begin{array}{ll}
\text{minimize} & \|X - A\|_2 \\
\text{subject to} & X \succeq 0
\end{array}
```

这是一个半定规划（semidefinite program, SDP）：决策变量是矩阵 $X$，目标是最小化 $X$ 与给定矩阵 $A$ 之间的范数（norm）距离，约束 $X\succeq 0$ 要求 $X$ 半正定（positive semidefinite）。

CVX 代码（注意 `sdp` 模式）：

```matlab
cvx_begin sdp
    variable X(n, n)
    minimize(norm(A - X))
    subject to
        X >= 0
cvx_end
```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这个 SDP 例子的几何图像是：在全体 $n\times n$ 对称矩阵构成的空间中，把矩阵 $A$ 投影到半正定锥上（凸锥（convex cone）的一种），得到离 $A$ 最近的半正定矩阵——协方差矩阵估计、控制系统分析中经常需要这种"把一个不合法的矩阵修正为合法半正定"的操作。注意这里的 $X\succeq 0$ 与上一节 LP 中的向量 $x\succeq 0$ 含义不同：向量情形指逐分量非负，矩阵情形指对一切 $v$ 都有 $v^T X v\ge 0$。这正是 Strang 线代里正定性（positive definiteness）的等价判据：所有特征值非负、所有主子式非负、存在 Cholesky 分解（Cholesky factorization）$X=LL^T$，三者互相等价。连接路径是：Strang 教你如何**验证**一个给定矩阵是否半正定，而半定规划把"半正定"升级为决策变量身上的**可优化约束**——从验证性质，跃迁到搜索满足性质且离目标最近的矩阵。这也解释了 CVX 为何专门提供 `cvx_begin sdp` 模式：同一句 `X >= 0` 在 sdp 模式下会被解释成半正定约束，而不是逐分量不等式。

> <span style="color:#7f8c8d;">（原 slides 页码：12）</span>

<!-- p.10 -->

### <span style="color:#c0392b;">Examples – Assignments（例题——练习作业）</span>

<span style="color:#2471a3;">**[section]**</span> <span style="color:#2471a3;">**[example]**</span> 优化问题：

```math
\text{minimize} \quad \sum_{i=1}^{n} \|x - a_i\|_2
```

目标是最小化 $x$ 到各给定点 $a_i$ 的欧氏距离之和；由于有 $n$ 个范数项，需要用循环逐项累加。

CVX 代码：

```matlab
cvx_begin
    variable x(n)
    OBJ = 0
    for i = 1:n
        OBJ = OBJ + norm(x - A(:,i));
    end
    minimize(OBJ)
cvx_end
```

<span style="color:#00838f;">注意（NB）：当循环规模很大时，这种写法会非常慢！</span>

> <span style="color:#7f8c8d;">（原 slides 页码：13）</span>

<!-- p.11 -->

### <span style="color:#c0392b;">CVX Pitfalls（CVX 常见陷阱）</span>

<span style="color:#2471a3;">**[section]**</span>

- 声明等式约束（equality constraint）用 `==`，不要用 `=`；
- 使用 `>=` 与 `<=`，而不是 `>` 与 `<`；
- 每个函数必须在其整个定义域（domain）上是凸的（convex），而不仅仅在可行区域（feasible region）内凸。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 第三条陷阱最值得展开：为什么要求每个函数在**整个定义域**上凸，而不仅仅在可行区域内凸？根源在于 DCP 验证是纯语法层面的：CVX 只看表达式由哪些原子函数、经哪些运算复合而成，并不知道变量最终会被赋什么值，更不知道哪些点真正落入可行集（feasible set）。数学上确实存在"只在某片区域凸"的函数，例如 $1/x$ 在 $x>0$ 上是凸的，但它的定义域 $x\ne 0$ 甚至不是凸集（而凸函数的定义域必须是凸集），这类写法 CVX 一律拒收，你必须显式声明定义域，或改用语义明确的凸原子函数（如 `pos`、`quad_over_lin`）来表达。这和 CSAPP 里静态类型检查的取舍同构：编译器保证的是语法层面的安全性质，运行期性质交还程序员自己负责——宁可错杀（拒收某些等价改写后其实合法的表达式），也不放过（接受一个可能非凸的模型）。工程上的好习惯是：把定义域信息显式写进 CVX 代码，让"验证区域"与"可行区域"重新对齐，而不是指望求解器替你兜底。

> <span style="color:#7f8c8d;">（原 slides 页码：15）</span>

<!-- p.12 -->

### <span style="color:#c0392b;">Resources（学习资源）</span>

<span style="color:#2471a3;">**[section]**</span>

- CVX 文档：<http://web.cvxr.com/cvx/doc/>
- 规范凸优化（disciplined convex optimization, DCP）：<http://dcp.stanford.edu/>

> <span style="color:#7f8c8d;">（原 slides 页码：16）</span>

<!-- p.13 -->

### <span style="color:#c0392b;">Questions?（有问题吗？）</span>

<span style="color:#2471a3;">**[section]**</span> Questions?（有问题吗？）

> <span style="color:#7f8c8d;">（原 slides 页码：17）</span>
