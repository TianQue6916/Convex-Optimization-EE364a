# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

> <span style="color:#7f8c8d;">Source: 补充材料/CVX Lecture Slides.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

## <span style="color:#c0392b;">Disciplined Convex Programming and CVX（规则化凸规划与 CVX）</span>

> <span style="color:#7f8c8d;">元信息：原 PDF 为 Stanford SEE 课程幻灯片，每页页脚为 “Convex Optimization, Boyd & Vandenberghe” 与 slide 编号；译文以页标记逐页对应原 PDF，标记编号 N 即原 PDF 第 N 页。</span>

<!-- p.1 -->

<span style="color:#2471a3;">**[lecture]**</span> 本讲主题：Disciplined Convex Programming and CVX（规则化凸规划与 CVX）。

- 主讲：Stephen Boyd
- 单位：Electrical Engineering Department（电气工程系），Stanford University（斯坦福大学）
- 配套教材：Convex Optimization, Boyd & Vandenberghe

<!-- p.2 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Outline（大纲）</span>

- cone program solver（锥规划求解器）
- modeling system（建模系统）
- disciplined convex programming（规则化凸规划，缩写 DCP）
- CVX（及其同类 CVXPY、Convex.jl）

<!-- p.3 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Cone program solvers（锥规划求解器）</span>

- LP（linear program，线性规划）求解器：数量众多，开源与商业的都有
- cone solver（锥求解器）：每个求解器能处理 LP、SOCP（second-order cone program，二阶锥规划）、SDP（semidefinite program，半定规划）、EXP（指数锥）这几类锥中某个子集的组合
  - 开源：SDPT3、SeDuMi、CVXOPT、CSDP、ECOS、SCS……
  - 商业：Mosek、Gurobi、Cplex……
- 本课程后面你会亲手写一个基础锥求解器

<!-- p.4 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Transforming problems to cone form（把问题转化为锥形式）</span>

- 把一个问题变成等价的锥规划（cone program）有大量技巧：
  - 引入 slack variable（松弛变量）
  - 引入新变量去 upper bound（上界）某些表达式
- 这些技巧极大扩展了锥求解器的适用范围
- 手写代码执行这种变换非常痛苦
- modeling system（建模系统）把这一步自动化

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 为什么要把千奇百怪的问题都“变形”成锥规划这一种标准形？这和 6.006 里 reduction（归约）的思想完全同源：只要能把新问题归约到一个已知存在多项式时间求解器的标准形式（如最短路、网络流、LP），求解器本身只需一个，问题的千变万化全部交给“翻译”环节。引入松弛变量、给复杂表达式配一个上界变量，本质上都是在构造这样的归约。代价是“翻译层”必须严格无错——变换后问题的最优值要能一一映射回原问题，这正是建模系统存在的意义：把容易出错的手工变换自动化、可复用化。另外注意 EXP（指数锥）的加入非常关键：log、exp、熵这类函数没法用 LP/SOCP/SDP 直接表达，却在极大似然、相对熵优化等场景里到处出现。

<!-- p.5 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Modeling systems（建模系统）</span>

一个典型的建模系统：

- 自动完成到锥形式的变换；支持
  - 声明 optimization variable（优化变量）
  - 描述 objective function（目标函数）
  - 描述 constraint（约束）
  - 选择（并配置）solver（求解器）
- 给定一个问题实例后，调用求解器
- 解释并返回求解器的状态（optimal、infeasible……）
- （求解成功后）把解变换回原始形式

<!-- p.6 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Some current modeling systems（现有的一些建模系统）</span>

- AMPL 与 GAMS（专有商业软件）
  - 开发于 1980 年代，至今仍广泛用于传统 OR（operations research，运筹学）
  - 不支持凸优化
- YALMIP（“Yet Another LMI Parser”，MATLAB）
  - 第一个面向对象的凸优化建模系统
- CVX（MATLAB）
- CVXPY（Python，GPL）
- Convex.jl（Julia，GPL，正并入 JUMP）
- CVX、CVXPY、Convex.jl 三者合称 CVX*

<!-- p.7 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Disciplined convex programming（规则化凸规划）</span>

- 目标与约束都用表达式来描述，表达式由以下元素构成：
  - 一组基本 atom（原子）：仿射、凸、凹函数
  - 一组受限的运算或规则（即保持凸性的运算规则）
- 建模系统持续跟踪每个表达式是仿射、凸还是凹
- 这些规则保证：
  - 被识别为凸的表达式确实是凸的
  - 但有些凸表达式不会被识别为凸
- <span style="color:#00838f;">用 DCP 描述的问题“按构造”（by construction）就是凸的</span>
- 所有凸优化建模系统都采用 DCP

<!-- p.8 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">CVX</span>

- CVX 使用 DCP
- 运行于 MATLAB，介于 cvx_begin 与 cvx_end 两条命令之间
- 依赖 SDPT3 或 SeDuMi 求解器（覆盖 LP/SOCP/SDP）
- 更多信息参见用户指南（user guide）与在线帮助
- CVX 示例库（example library）含一百多个例子

<!-- p.9 -->

### <span style="color:#2471a3;">**[example]**</span> <span style="color:#c0392b;">Example: Constrained norm minimization（例：带约束的范数最小化）</span>

```matlab
A = randn(5, 3);
b = randn(5, 1);
cvx_begin
    variable x(3);
    minimize(norm(A*x - b, 1))
    subject to
        -0.5 <= x;
        x <= 0.3;
cvx_end
```

- 在 cvx_begin 与 cvx_end 之间，x 是 CVX 变量
- `subject to` 这条语句什么也不做，但写上可读性更好
- 不等式按 elementwise（按元素）方式解释

<!-- p.10 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">What CVX does（CVX 做了什么）</span>

执行 cvx_end 之后，CVX 会：

- 把问题变换成一个 LP
- 调用求解器 SDPT3
- 用（数值的）最优解覆盖（对象）x
- 把问题最优值赋给 cvx_optval
- 把问题状态（此处为 Solved）赋给 cvx_status
- （若问题不可行，则 cvx_status 为 Infeasible，且 x 为 NaN）

<!-- p.11 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Variables and affine expressions（变量与仿射表达式）</span>

- 用 `variable name[(dims)] [attributes]` 语法声明变量：

```matlab
variable x(3);
variable C(4,3);
variable S(3,3) symmetric;
variable D(3,3) diagonal;
variables y z;
```

- 构造 affine expression（仿射表达式）：

```matlab
A = randn(4, 3);
variables x(3) y(4);
% 以下是仿射表达式的例子：
3*x + 4
A*x - y
x(2:3)
sum(x)
```

<!-- p.12 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Some functions（一些函数）</span>

| function（函数） | meaning（含义） | attributes（属性） |
|---|---|---|
| `norm(x, p)` | $\lVert x \rVert_p$ | `cvx`（凸） |
| `square(x)` | $x^2$ | `cvx`（凸） |
| `square_pos(x)` | $(x_+)^2$ | `cvx, nondecr`（凸，非递减） |
| `pos(x)` | $x_+$ | `cvx, nondecr`（凸，非递减） |
| `sum_largest(x,k)` | $x_{[1]} + \cdots + x_{[k]}$（最大的 k 个分量之和） | `cvx, nondecr`（凸，非递减） |
| `sqrt(x)` | $\sqrt{x}$（$x \ge 0$） | `ccv, nondecr`（凹，非递减） |
| `inv_pos(x)` | $1/x$（$x > 0$） | `cvx, nonincr`（凸，非递增） |
| `max(x)` | $\max\{x_1, \ldots, x_n\}$ | `cvx`（凸） |
| `quad_over_lin(x,y)` | $x^2/y$（$y > 0$） | `cvx, nonincr in y`（凸，关于 y 非递增） |
| `lambda_max(X)` | $\lambda_{\max}(X)$（$X = X^T$） | `cvx`（凸） |
| `huber(x)` | $x^2$（$\lvert x \rvert \le 1$）；$2\lvert x \rvert - 1$（$\lvert x \rvert > 1$） | `cvx`（凸） |

属性关键字的含义：`cvx` 表示凸、`ccv` 表示凹、`nondecr` 表示非递减（nondecreasing）、`nonincr` 表示非递增（nonincreasing）。

<!-- p.13 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Composition rules（复合规则）</span>

- 可以按合法的 composition rule（复合规则）组合原子，例如：
  - 仿射函数的凸函数是凸的
  - 凸函数取负号得到凹函数
  - 凸且非递减的函数作用在凸函数上仍是凸的
  - 凹且非递减的函数作用在凹函数上仍是凹的

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 复合规则看起来像死记硬背，其实一条 Jensen 不等式（Jensen's inequality，概率论里的期望工具）就能讲清大部分。以“凸且非递减的 h 作用在凸函数 g 上仍凸”为例：设随机变量 $Z$，由 h 凸得 $\mathbb{E}[h(g(Z))] \ge h(\mathbb{E}[g(Z)])$，这一步只用了 h 的凸性；再由 g 凸得 $\mathbb{E}[g(Z)] \ge g(\mathbb{E}[Z])$，又因 h 非递减，不等号方向可以“原样穿过” h，得到 $h(\mathbb{E}[g(Z)]) \ge h(g(\mathbb{E}[Z]))$。两步串起来正是 $\mathbb{E}[h(g(Z))] \ge h(g(\mathbb{E}[Z]))$，即复合后仍满足凸函数的 Jensen 定义。可见单调性在这里的作用非常明确：它是让内层不等式“穿过”外层函数的通行证；若 h 非递增，穿过时要反向，于是规则要求内层换成凹函数。这就是表格里 nondecr/nonincr 标签的全部秘密。

<!-- p.14 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Composition rules — multiple arguments（复合规则：多参数情形）</span>

- 对凸函数 $h$：只要对每个 $i$ 满足下列条件之一，$h(g_1, \ldots, g_k)$ 就被识别为凸：
  - $g_i$ 是仿射的；或
  - $g_i$ 是凸的，且 $h$ 关于第 $i$ 个参数非递减；或
  - $g_i$ 是凹的，且 $h$ 关于第 $i$ 个参数非递增
- 对凹函数 $h$：对称地，只要对每个 $i$ 满足下列条件之一，$h(g_1, \ldots, g_k)$ 就被识别为凹：
  - $g_i$ 是仿射的；或
  - $g_i$ 是凸的，且 $h$ 关于第 $i$ 个参数非递增；或
  - $g_i$ 是凹的，且 $h$ 关于第 $i$ 个参数非递减

<!-- p.15 -->

### <span style="color:#2471a3;">**[example]**</span> <span style="color:#c0392b;">Valid (recognized) examples（合法且被识别的表达式示例）</span>

设 u、v、x、y 为标量变量；X 为 $3 \times 3$ 对称变量。

- 凸（convex）：

```matlab
norm(A*x - y) + 0.1*norm(x, 1)
quad_over_lin(u - v, 1 - square(v))
lambda_max(2*X - 4*eye(3))
norm(2*X - 3, 'fro')
```

- 凹（concave）：

```matlab
min(1 + 2*u, 1 - max(2, v))
sqrt(v) - 4.55*inv_pos(u - v)
```

<!-- p.16 -->

### <span style="color:#2471a3;">**[example]**</span> <span style="color:#c0392b;">Rejected examples（被拒绝的表达式示例）</span>

设 u、v、x、y 为标量变量。

- 既非凸也非凹：

```matlab
square(x) - square(y)
norm(A*x - y) - 0.1*norm(x, 1)
```

- 因 DCP 规则集有限而被拒绝：

```matlab
sqrt(sum(square(x)))   % 确实是凸的；可直接改用 norm(x)
square(1 + x^2)        % 确实是凸的；可改用 square_pos(1 + x^2)，
                       % 或 1 + 2*pow_pos(x, 2) + pow_pos(x, 4)
```

<!-- p.17 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Sets（集合）</span>

- 有些约束用凸集（convex set）来表达更自然
- CVX 中集合的工作方式：创建一个被约束到该集合的匿名变量（unnamed variable）
- 例子：

```matlab
semidefinite(n)
nonnegative(n)
simplex(n)
lorentz(n)
```

- 比如 `semidefinite(n)` 返回一个匿名的（对称矩阵）变量，并被约束为 positive semidefinite（半正定）

<!-- p.18 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Using the semidefinite cone（使用半定锥）</span>

变量：X（对称矩阵）、z（向量）、t（标量）；常数：A、B（矩阵）。

- `X == semidefinite(n)`
  - 表示 $X \in \mathbf{S}^n_+$（即 $X \succeq 0$）
- `A*X*A' - X == B*semidefinite(n)*B'`
  - 表示存在 $Z \succeq 0$，使得 $AXA^T - X = BZB^T$
- `[X z; z' t] == semidefinite(n+1)`
  - 表示

```math
\begin{bmatrix}
X & z \\
z^T & t
\end{bmatrix} \succeq 0
```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> `[X z; z' t] == semidefinite(n+1)` 这一行值得用 Strang 线代的语言重读一遍。分块对称矩阵半正定，等价于它的所有特征值非负（谱定理），也等价于对任意向量 $v$ 都有二次型 $v^T M v \ge 0$。当块 $X$ 可逆时，这个条件可以用 Schur 补化简为：$X \succeq 0$ 且 $t - z^T X^{-1} z \ge 0$，后者正是“以 $X^{-1}$ 为度量的二次型不超过 $t$”。这就解释了为什么大量含二次型或范数的约束（如 $x^T P x \le 1$）都能藏进一个 SDP：把二次型写成块矩阵的一块，半正定性就自动替你编码了整条不等式。你在 Strang 线代（18.065）里学的正定性与 SVD，在这里直接变成了可以写进求解器的约束语言。

<!-- p.19 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Objectives and constraints（目标与约束）</span>

- 目标函数可以是：
  - `minimize(凸表达式)`
  - `maximize(凹表达式)`
  - 省略（即 feasibility problem，可行性问题）
- 约束可以是：
  - 凸表达式 `<=` 凹表达式
  - 凹表达式 `>=` 凸表达式
  - 仿射表达式 `==` 仿射表达式
  - 省略（即无约束问题）

<!-- p.20 -->

### <span style="color:#2471a3;">**[example]**</span> <span style="color:#c0392b;">More involved example（一个更复杂的例子）</span>

```matlab
A = randn(5);
A = A'*A;
cvx_begin
    variable X(5, 5) symmetric;
    variable y;
    minimize(norm(X) - 10*sqrt(y))
    subject to
        X - A == semidefinite(5);
        X(2,5) == 2*y;
        X(3,1) >= 0.8;
        y <= 4;
cvx_end
```

<!-- p.21 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Defining new functions（定义新函数）</span>

- 可以用已有原子构造新函数
- 例子：凸的 deadzone function（死区函数）

```math
f(x) = \max\{|x| - 1,\; 0\} =
\begin{cases}
0, & |x| \le 1 \\
x - 1, & x > 1 \\
1 - x, & x < -1
\end{cases}
```

- 创建文件 deadzone.m，内容如下：

```matlab
function y = deadzone(x)
    y = max(abs(x) - 1, 0)
```

- deadzone 在 CVX 之内与之外都能用（都给出合理含义）

<!-- p.22 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">Defining functions via incompletely specified problems（借助不完全指定问题定义函数）</span>

- 设 $f_0, \ldots, f_m$ 关于 $(x, z)$ 是凸的
- 令 $\phi(x)$ 为下述凸问题的 optimal value（最优值），其中 $z$ 是变量、$x$ 是参数：

```math
\begin{array}{ll}
\text{minimize} & f_0(x, z) \\
\text{subject to} & f_i(x, z) \le 0, \quad i = 1, \ldots, m \\
& A_1 x + A_2 z = b
\end{array}
```

- <span style="color:#00838f;">$\phi$ 是一个凸函数</span>
- 上面的问题有时称为 incompletely specified problem（不完全指定问题），因为 $x$ 还没有给定
- 不完全指定的凹最大化问题则定义一个凹函数

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> “参数化最优值函数 $\phi(x) = \min_z f_0(x,z)$ 仍是凸函数”这件事初看很魔法，其实和两个你熟悉的东西同构。其一是 6.006 动态规划里的“子问题最优值”：DP 的状态值函数就是关于状态参数的最优值函数，我们从不显式写出它的闭式，而是靠最优子结构推理它的性质——这里的 $\phi$ 同样往往没有闭式，但凸性保证了对它继续做外层优化是安全的。其二是概率论里的边缘化：联合高斯分布对一部分变量积分后仍是高斯；联合凸函数对一部分变量取最小（部分最小化）后仍是凸。两者都是“对局部变量求和或求最优，整体结构保持不变”的模式。理解了这一点，CVX 允许把一个内层 cvx_begin…cvx_end 封装成函数再嵌进外层问题，就顺理成章：它只是把“结构保持”操作做成了可复用的积木。

<!-- p.23 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">CVX functions via incompletely specified problems（用不完全指定问题实现 CVX 函数）</span>

在 CVX 中这样实现：

```matlab
function cvx_optval = phi(x)
    cvx_begin
        variable z;
        minimize(f0(x, z))
        subject to
            f1(x, z) <= 0; ...
            A1*x + A2*z == b;
    cvx_end
```

- 函数 phi 对数值型的 x 可用（通过求解问题来求值）
- phi 也可以用在 CVX 声明内部——凡是凸函数能出现的地方都可以

<!-- p.24 -->

### <span style="color:#2471a3;">**[example]**</span> <span style="color:#c0392b;">Simple example: Two element max（简单例子：两元素的 max）</span>

- 创建文件 max2.m，内容如下：

```matlab
function cvx_optval = max2(x, y)
    cvx_begin
        variable t;
        minimize(t)
        subject to
            x <= t;
            y <= t;
    cvx_end
```

- 这些约束定义了 max 函数的 epigraph（上图）
- 可以加一点逻辑：当 x、y 是数值时直接返回 max(x,y)（否则，为了算两个数的 max 竟要解一个 LP！）

<!-- p.25 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">A more complex example（更复杂一点的例子）</span>

- $f(x) = x + x^{1.5} + x^{2.5}$，定义域 $\operatorname{dom} f = \mathbf{R}_+$，是凸的、单调递增的函数
- 它的反函数 $g = f^{-1}$ 是凹的、单调递增的，定义域 $\operatorname{dom} g = \mathbf{R}_+$
- $g$ 没有闭式（closed form）表达式
- $g(y)$ 是下述问题的最优值：

```math
\begin{array}{ll}
\text{maximize} & t \\
\text{subject to} & t_+ + t_+^{1.5} + t_+^{2.5} \le y
\end{array}
```

（当 $y < 0$ 时此问题不可行，于是最优值为 $-\infty$）

<!-- p.26 -->

### <span style="color:#2471a3;">**[example]**</span> <span style="color:#c0392b;">函数 g 的实现与使用</span>

实现为：

```matlab
function cvx_optval = g(y)
    cvx_begin
        variable t;
        maximize(t)
        subject to
            pos(t) + pow_pos(t, 1.5) + pow_pos(t, 2.5) <= y;
    cvx_end
```

- 可以当普通函数用，比如 g(14.3)；也可以在 CVX 内部当凹函数用：

```matlab
cvx_begin
    variables x y;
    minimize(quad_over_lin(x, y) + 4*x + 5*y)
    subject to
        g(x) + 2*g(y) >= 2;
cvx_end
```

<!-- p.27 -->

### <span style="color:#2471a3;">**[example]**</span> <span style="color:#c0392b;">Example（例子：LP 最优值函数）</span>

- LP 的最优值

```math
f(c) = \inf\{c^T x \mid Ax \preceq b\}
```

<span style="color:#00838f;">是关于 $c$ 的凹函数（concave function）。</span>

- 由对偶性（duality）（并设 $Ax \preceq b$ 可行）：

```math
f(c) = \sup\{-\lambda^T b \mid A^T \lambda + c = 0,\; \lambda \succeq 0\}
```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> “LP 最优值关于成本向量 $c$ 是凹函数”是对偶性最实用的推论之一。原因很干净：$f(c) = \inf\{c^T x\}$ 是一族关于 $c$ 的线性函数取下确界，而仿射函数的逐点下确界必凹（对称地，逐点上确界必凸）。6.006 里学过的最短路问题就是现成的例子：最短路长度作为边成本向量 $c$ 的值函数，其对偶用一组“势标号”去下界每一条 s–t 路径——这正是 LP 与网络流/最短路对偶关系的具体化身。上面 $f(c) = \sup\{-\lambda^T b\}$ 的表达式中，拉格朗日乘子（Lagrange multiplier）$\lambda$ 扮演的正是“势”的角色。这凹性还是灵敏度分析（perturbation analysis）的基石：它保证最优值随成本参数的变化是“良性”的，为后面“对偶变量即影子价格”的解释铺路。

<!-- p.28 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">在 CVX 中定义 lp_opt_val</span>

在 CVX 中把 f 定义为：

```matlab
function cvx_optval = lp_opt_val(A,b,c)
    cvx_begin
        variable lambda(length(b));
        maximize(-lambda'*b);
        subject to
            A'*lambda + c == 0; lambda >= 0;
    cvx_end
```

- 在 lp_opt_val(A,b,c) 中，A、b 必须是常数；c 可以是仿射表达式

<!-- p.29 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">CVX hints/warnings（CVX 提示与警告）</span>

- 当心 =（赋值）与 ==（等式约束）的区别
- 当 X 是矩阵时，`X >= 0` 是按元素的不等式
- `X >= semidefinite(n)` 的意思是：X 按元素大于某个半正定矩阵（这多半不是你想要的）
- 写 `subject to` 并非必需（但看起来更整洁）
- 许多传统上用凸二次型表述的问题，都可以改写成范数问题（norm problem，后者的数值性质往往更好）：
  `x'*P*x <= 1` 可以替换为 `norm(chol(P)*x) <= 1`
