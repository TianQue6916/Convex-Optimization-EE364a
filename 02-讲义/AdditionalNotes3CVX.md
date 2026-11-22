> <span style="color:#7f8c8d;">Source: SEE官方讲义/AdditionalNotes3CVX.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

## <span style="color:#c0392b;">EE364a 复习：规范凸编程与 CVX（EE364a Review: Disciplined Convex Programming and CVX）</span>

<!-- p.1 -->

<span style="color:#2471a3;">**[lecture]**</span>

本讲是 EE364a 的复习课，主题为规范凸编程（Disciplined Convex Programming, DCP）与 CVX 建模系统，将依次介绍：

- 凸优化求解器（convex optimization solvers）
- 建模系统（modeling systems）
- 规范凸编程（disciplined convex programming）
- CVX

---

<!-- p.2 -->

### <span style="color:#c0392b;">凸优化求解器 Convex optimization solvers</span>

<span style="color:#2471a3;">**[section]**</span>

- **LP 求解器（LP solvers）**：可选择的很多（GLPK、Excel、MATLAB 的 `linprog` 等）
- **锥求解器（cone solvers）**：通常处理 LP、SOCP、SDP 锥（cone）及其组合；已有多种可用（SDPT3、SeDuMi、CSDP 等）
- **一般凸求解器（general convex solvers）**：有一些可用（CVXOPT、MOSEK 等）
- 此外还有大量专用（special purpose）或面向特定应用的（application specific）求解器
- 也可以自己动手写一个（本学期后面我们会学习并亲自编写求解器）

---

<!-- p.3 -->

### <span style="color:#c0392b;">把问题变换为标准形式 Transforming problems to standard form</span>

<span style="color:#2471a3;">**[section]**</span>

- 你已经见过大量技巧，可以把一个问题变换为等价的、具有标准形式（standard form）的问题（例如 LP、SDP）
- 这些技巧极大扩展了标准求解器的适用范围
- 但亲手写代码来实现这些变换往往相当痛苦
- 建模系统可以把这一步部分自动化

---

<!-- p.4 -->

### <span style="color:#c0392b;">建模系统 Modeling systems</span>

<span style="color:#2471a3;">**[section]**</span>

一个典型的建模系统（modeling system）：

- 自动完成大部分到标准形式的变换；支持：
  - 声明优化变量（optimization variables）
  - 描述目标函数（objective function）
  - 描述约束（constraints）
  - 选择（并配置）求解器
- 给定问题实例后，调用求解器
- 解读并返回求解器的状态（optimal 最优、infeasible 不可行……）
- （求解成功时）把解变换回原始形式

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> CVX 这类建模系统的分层思路，和编译器把「高级语言源码」翻译成「机器指令」几乎是同构的：用户写的是数学层面的目标函数与约束（相当于前端），系统负责等价变形与锥规划标准型转换（相当于中间表示与优化 pass），数值计算则交给 SDPT3/SeDuMi 这类求解器（相当于后端）。在没有这一层的年代，同一个凸问题在不同论文里可能有十种等价写法，手工转成 LP/SOCP/SDP 标准型既繁琐，又极易在符号、转置和不等号方向上出错。建模系统把「问题描述」与「算法执行」解耦：换求解器不必改模型代码，这就是它真正的工程价值。你在 CSAPP 里见过抽象层的收益（高级语言屏蔽 ISA 细节），同样的逻辑在这里成立——抽象的代价是失去对底层的精细控制，收益是正确性与可移植性。后面你会看到，这份「正确性」正是由 DCP 规则系统来保证的。

---

<!-- p.5 -->

### <span style="color:#c0392b;">现有的一些建模系统 Some current modeling systems</span>

<span style="color:#2471a3;">**[section]**</span>

- **AMPL 与 GAMS**（专有软件 proprietary）
  - 开发于 20 世纪 80 年代，至今仍广泛用于传统运筹学（operations research, OR）
  - 不支持凸优化
- **YALMIP**（「Yet Another LMI Parser」）
  - 第一个基于 MATLAB、对凸优化有专门支持的面向对象建模系统
  - 可使用多种不同的求解器；能处理一些非凸问题
- **CVXMOD/CVXOPT**（alpha 阶段）
  - 基于 Python，完全遵循 GPL
  - 支持锥求解器与自定义求解器
- **CVX**
  - 基于 MATLAB，遵循 GPL，使用 SDPT3/SeDuMi

---

<!-- p.6 -->

### <span style="color:#c0392b;">规范凸编程 Disciplined convex programming</span>

<span style="color:#2471a3;">**[definition]**</span>

<span style="color:#00838f;">**规范凸编程（DCP）**：只用一组基本原子（atoms，即凸、凹函数）和一组受限的、保持凸性的运算规则来构造目标函数与约束表达式；凡通过规则检查的问题，在构造上就是凸问题（convex by construction）。</span>

- 用由以下元素构成的表达式来描述目标函数与约束：
  - 一组基本原子（凸、凹函数）
  - 一组受限的运算或规则（保持凸性 convexity-preserving）
- 建模系统持续追踪每个表达式是仿射（affine）、凸（convex）还是凹（concave）的
- 规则保证：
  - 被识别为凸（凹）的表达式确实一定是凸（凹）的
  - 但某些凸（凹）表达式不会被识别为凸（凹）
- 用 DCP 描述的问题是**构造上即为凸的（convex by construction）**

---

<!-- p.7 -->

### <span style="color:#c0392b;">CVX</span>

<span style="color:#2471a3;">**[section]**</span>

- CVX 使用 DCP
- 运行于 MATLAB，位于 `cvx_begin` 与 `cvx_end` 命令之间
- 依赖 SDPT3 或 SeDuMi 求解器（LP/SOCP/SDP）
- 更多信息请参阅用户指南（user guide）与在线帮助
- CVX 示例库（example library）收录了一百多个例子

---

<!-- p.8 -->

### <span style="color:#c0392b;">例：约束范数最小化 Example: Constrained norm minimization</span>

<span style="color:#2471a3;">**[example]**</span>

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

- 在 `cvx_begin` 与 `cvx_end` 之间，`x` 是一个 CVX 变量
- `subject to` 语句本身不做任何事情，加它只是为了可读性
- 不等式按元素（elementwise）逐项解释

---

<!-- p.9 -->

### <span style="color:#c0392b;">CVX 做了什么 What CVX does</span>

<span style="color:#2471a3;">**[section]**</span>

`cvx_end` 之后，CVX 会：

- 把问题变换为一个 LP
- 调用求解器 SDPT3
- 用（数值的）最优解覆盖（对象）`x`
- 把问题的最优值赋给 `cvx_optval`
- 把问题的状态（这里是 Solved，已求解）赋给 `cvx_status`

（若问题不可行，`cvx_status` 将为 Infeasible，且 `x` 为 NaN）

---

<!-- p.10 -->

### <span style="color:#c0392b;">变量与仿射表达式 Variables and affine expressions</span>

<span style="color:#2471a3;">**[section]**</span>

- 用 `variable name[(dims)] [attributes]` 声明变量：
  - `variable x(3);`
  - `variable C(4,3);`
  - `variable S(3,3) symmetric;`
  - `variable D(3,3) diagonal;`
  - `variables y z;`
- 构造仿射表达式（affine expressions）：
  - `A = randn(4, 3);`
  - `variables x(3) y(4);`
  - `3*x + 4`
  - `A*x - y`
  - `x(2:3)`
  - `sum(x)`

---

<!-- p.11 -->

### <span style="color:#c0392b;">一些函数 Some functions</span>

<span style="color:#2471a3;">**[section]**</span>

| 函数 function | 含义 meaning | 属性 attributes |
|---|---|---|
| `norm(x, p)` | $\Vert x\Vert_p$ | cvx（凸） |
| `square(x)` | $x^2$ | cvx（凸） |
| `square_pos(x)` | $(x_+)^2$ | cvx（凸），nondecr（非减） |
| `pos(x)` | $x_+$ | cvx（凸），nondecr（非减） |
| `sum_largest(x,k)` | $x_{[1]}+\cdots+x_{[k]}$（即 $k$ 个最大分量之和） | cvx（凸），nondecr（非减） |
| `sqrt(x)` | $\sqrt{x}$（要求 $x \ge 0$） | ccv（凹），nondecr（非减） |
| `inv_pos(x)` | $1/x$（要求 $x > 0$） | cvx（凸），nonincr（非增） |
| `max(x)` | $\max\{x_1,\ldots,x_n\}$ | cvx（凸），nondecr（非减） |
| `quad_over_lin(x,y)` | $x^2/y$（要求 $y > 0$） | cvx（凸），关于 $y$ nonincr（非增） |
| `lambda_max(X)` | $\lambda_{\max}(X)$（要求 $X = X^T$） | cvx（凸） |
| `huber(x)` | $x^2$（当 $\lvert x\rvert \le 1$）；$2\lvert x\rvert-1$（当 $\lvert x\rvert > 1$） | cvx（凸） |

表中属性缩写：cvx = convex（凸）、ccv = concave（凹）、nondecr = nondecreasing（非减）、nonincr = nonincreasing（非增）；$x_+$ 表示 $\max\{x,0\}$，即正部（positive part）。

---

<!-- p.12 -->

### <span style="color:#c0392b;">复合规则 Composition rules</span>

<span style="color:#2471a3;">**[section]**</span>

- 可以用合法的复合规则（composition rules）组合原子，例如：
  - 仿射函数的凸函数仍是凸函数
  - 凸函数取负号得到凹函数
  - 凸且非减的函数作用于凸函数，结果为凸
  - 凹且非减的函数作用于凹函数，结果为凹
- 对凸函数 $h$：$h(g_1,\ldots,g_k)$ 被识别为凸，若对每个 $i$：
  - $g_i$ 是仿射的；或
  - $g_i$ 是凸的，且 $h$ 对第 $i$ 个参数非减（nondecreasing）；或
  - $g_i$ 是凹的，且 $h$ 对第 $i$ 个参数非增（nonincreasing）
- 对凹函数 $h$：$h(g_1,\ldots,g_k)$ 被识别为凹，若对每个 $i$：
  - $g_i$ 是仿射的；或
  - $g_i$ 是凸的，且 $h$ 对第 $i$ 个参数非增；或
  - $g_i$ 是凹的，且 $h$ 对第 $i$ 个参数非减

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 复合规则看起来像人为规定，其实是一元链式法则在凸性上的推广。对复合 $h(g(x))$ 求导得 $h'(g(x))\,g'(x)$：若 $h$ 凸且非减，则 $h'\ge 0$，外层不会反转内层的「符号结构」，于是凸复合凸仍是凸；若 $h$ 非增，符号被反转，这时就需要内层是凹的才能重新配平。这正是上一页函数表中 nondecr/nonincr 属性存在的原因——单调性决定外层函数沿哪个方向「放大」内层。要强调的是，DCP 规则集只是凸性的充分条件而非必要条件：$\sqrt{x^2+y^2}$ 明明是凸的（它就是欧氏范数），但按规则写 $\sqrt{\mathrm{square}(x)+\mathrm{square}(y)}$ 会被 CVX 拒收。这很像编程语言的类型系统：通过类型检查的程序必然正确，但被拒收不代表真的错，只是系统无法自动给出正确性证明，你得改写成一个等价且能通过检查的形式（例如直接用 `norm(x)`）。

---

<!-- p.13 -->

### <span style="color:#c0392b;">合法（被识别）的例子 Valid (recognized) examples</span>

<span style="color:#2471a3;">**[example]**</span>

设 $u, v, x, y$ 为标量变量；$X$ 为 $3\times 3$ 对称变量。

- 凸表达式（convex）：
  - `norm(A*x - y) + 0.1*norm(x, 1)`
  - `quad_over_lin(u - v, 1 - square(v))`
  - `lambda_max(2*X - 4*eye(3))`
  - `norm(2*X - 3, 'fro')`
- 凹表达式（concave）：
  - `min(1 + 2*u, 1 - max(2, v))`
  - `sqrt(v) - 4.55*inv_pos(u - v)`

---

<!-- p.14 -->

### <span style="color:#c0392b;">被拒收的例子 Rejected examples</span>

<span style="color:#2471a3;">**[example]**</span>

设 $u, v, x, y$ 为标量变量。

- 既非凸也非凹：
  - `square(x) - square(y)`
  - `norm(A*x - y) - 0.1*norm(x, 1)`
- 因 DCP 规则集有限而被拒收（尽管实际上是凸的）：
  - `sqrt(sum(square(x)))`（是凸的；可以改用 `norm(x)`）
  - `square(1 + x^2)`（是凸的；可以改用 `square_pos(1 + x^2)`，或 `1 + 2*pow_pos(x, 2) + pow_pos(x, 4)`）

---

<!-- p.15 -->

### <span style="color:#c0392b;">集合 Sets</span>

<span style="color:#2471a3;">**[section]**</span>

- 某些约束用凸集（convex sets）来表达更自然
- CVX 中集合的实现方式：创建一个被约束到该集合的匿名（unnamed）变量
- 例子：
  - `semidefinite(n)`
  - `nonnegative(n)`
  - `simplex(n)`
  - `lorentz(n)`
- 例如 `semidefinite(n)` 返回一个匿名（对称矩阵）变量，并被约束为半正定（positive semidefinite）

---

<!-- p.16 -->

### <span style="color:#c0392b;">使用半定锥 Using the semidefinite cone</span>

<span style="color:#2471a3;">**[section]**</span>

变量：$X$（对称矩阵）、$z$（向量）、$t$（标量）；常数：$A$ 与 $B$（矩阵）。

- `X == semidefinite(n)`
  - 表示 $X \in \mathbf{S}^n_+$（即 $X \succeq 0$）
- `A*X*A' - X == B*semidefinite(n)*B'`
  - 表示存在 $Z \succeq 0$，使得 $AXA^T - X = BZB^T$
- `[X z; z' t] == semidefinite(n+1)`
  - 表示

```math
\begin{bmatrix}
X & z\\
z^T & t
\end{bmatrix} \succeq 0
```

---

<!-- p.17 -->

### <span style="color:#c0392b;">目标函数与约束 Objectives and constraints</span>

<span style="color:#2471a3;">**[section]**</span>

- 目标函数可以是：
  - `minimize(convex expression)`（最小化凸表达式）
  - `maximize(concave expression)`（最大化凹表达式）
  - 省略（即可行性问题 feasibility problem）
- 约束可以是：
  - 凸表达式 `<=` 凹表达式
  - 凹表达式 `>=` 凸表达式
  - 仿射表达式 `==` 仿射表达式
  - 省略（即无约束问题）

---

<!-- p.18 -->

### <span style="color:#c0392b;">一个更复杂的例子 More involved example</span>

<span style="color:#2471a3;">**[example]**</span>

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

---

<!-- p.19 -->

### <span style="color:#c0392b;">定义新函数 Defining new functions</span>

<span style="color:#2471a3;">**[section]**</span>

- 可以用现有原子构造新函数
- 例：凸的死区函数（deadzone function）

```math
f(x)=\max\{\lvert x\rvert-1,\;0\}=
\begin{cases}
0, & \lvert x\rvert \le 1\\
x-1, & x > 1\\
1-x, & x < -1
\end{cases}
```

- 创建文件 `deadzone.m`，内容如下：

```matlab
function y = deadzone(x)
    y = max(abs(x) - 1, 0)
```

- `deadzone` 无论在 CVX 之内还是之外都能正常使用

---

<!-- p.20 -->

### <span style="color:#c0392b;">通过不完全指定的问题定义函数 Defining functions via incompletely specified problems</span>

<span style="color:#2471a3;">**[section]**</span>

- 设 $f_0,\ldots,f_m$ 关于 $(x,z)$ 是凸的
- 令 $\phi(x)$ 为下述凸问题的最优值，其中 $z$ 是变量，$x$ 是参数：

```math
\begin{array}{ll}
\text{minimize} & f_0(x,z)\\
\text{subject to} & f_i(x,z) \le 0, \quad i=1,\ldots,m\\
& A_1 x + A_2 z = b
\end{array}
```

- 则 $\phi$ 是一个凸函数
- 上述问题有时被称为**不完全指定的（incompletely specified）**，因为 $x$ 尚未给定
- 不完全指定的凹最大化问题则定义一个凹函数

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 用「部分变量未定的优化问题」定义新函数，本质是对 $z$ 做部分最小化（partial minimization），这是凸分析里保凸的基本运算之一：$\phi(x)=\inf_z f_0(x,z)$ 的上图（epigraph）恰好是 $f_0$ 的上图沿 $z$ 方向的投影，而凸集的投影仍是凸集，所以 $\phi$ 必然是凸函数。这个手法你在最小二乘里其实已经用过：把一部分变量解析消去（profiling）之后，剩下的目标函数对其余变量依然是凸的。它与对偶理论的联动尤其重要——第 5 章里原问题最优值作为参数的函数、以及对偶函数本身，都是「对一部分变量取极值」构造出来的。概率论里全期望公式 $E[X]=E[E[X\mid Y]]$ 做的也是同类的「先对内层变量积分，再研究外层函数性质」的操作。在 CVX 中，这允许你把一个子问题的最优值直接当作新的原子来使用，而不必手工推导它的解析形式。

---

<!-- p.21 -->

### <span style="color:#c0392b;">用不完全指定问题实现 CVX 函数 CVX functions via incompletely specified problems</span>

<span style="color:#2471a3;">**[section]**</span>

在 CVX 中实现为：

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

- 函数 `phi` 对数值型的 `x` 可以直接运行（通过求解该问题）
- `phi` 也可以在 CVX 规范（specification）内部、任何可用凸函数的地方使用

---

<!-- p.22 -->

### <span style="color:#c0392b;">简单例子：两元素最大值 Simple example: Two element max</span>

<span style="color:#2471a3;">**[example]**</span>

- 创建文件 `max2.m`，内容如下：

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

- 这些约束定义了 max 函数的上图（epigraph）
- 可以加入逻辑判断：当 `x`、`y` 是数值时直接返回 `max(x,y)`
  （否则，为了求两个数的最大值竟然要解一个 LP！）

---

<!-- p.23 -->

### <span style="color:#c0392b;">一个更复杂的例子 A more complex example</span>

<span style="color:#2471a3;">**[example]**</span>

- $f(x) = x + x^{1.5} + x^{2.5}$，定义域（domain）为 $\mathbf{R}_+$，是一个凸的、单调递增函数
- 它的反函数 $g = f^{-1}$ 是凹的、单调递增函数，定义域为 $\mathbf{R}_+$
- $g$ 没有闭式（closed form）表达式
- $g(y)$ 是下述问题的最优值：

```math
\begin{array}{ll}
\text{maximize} & t\\
\text{subject to} & t_+ + t_+^{1.5} + t_+^{2.5} \le y
\end{array}
```

（当 $y < 0$ 时该问题不可行，故最优值为 $-\infty$）

---

<!-- p.24 -->

### <span style="color:#c0392b;">实现 Implementation</span>

<span style="color:#2471a3;">**[example]**</span>

- 实现为：

```matlab
function cvx_optval = g(y)
    cvx_begin
        variable t;
        maximize(t)
        subject to
            pos(t) + pow_pos(t, 1.5) + pow_pos(t, 2.5) <= y;
    cvx_end
```

- 可以把它当普通函数用，如 `g(14.3)`；也可以在 CVX 内部把它当凹函数用：

```matlab
cvx_begin
    variables x y;
    minimize(quad_over_lin(x, y) + 4*x + 5*y)
    subject to
        g(x) + 2*g(y) >= 2;
cvx_end
```

---

<!-- p.25 -->

### <span style="color:#c0392b;">例：LP 最优值函数 Example</span>

<span style="color:#2471a3;">**[example]**</span>

- LP 的最优值 $f(c) = \inf\{c^T x \mid Ax \preceq b\}$ 是关于 $c$ 的凹函数
- 由对偶性（duality，假设 $Ax \preceq b$ 可行）可得：

```math
f(c)=\sup\{-\lambda^T b \mid A^T\lambda + c = 0,\ \lambda \succeq 0\}
```

- 在 CVX 中把 $f$ 定义为：

```matlab
function cvx_optval = lp_opt_val(A,b,c)
    cvx_begin
        variable lambda(length(b));
        maximize(-lambda'*b);
        subject to
            A'*lambda + c == 0; lambda >= 0;
    cvx_end
```

- 在 `lp_opt_val(A,b,c)` 中，`A`、`b` 必须是常数；`c` 可以是仿射表达式

---

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> LP 最优值 $f(c)=\inf\{c^Tx \mid Ax\preceq b\}$ 关于成本向量 $c$ 是凹函数，这是第 5 章对偶性理论最干净的应用实例。看它的对偶表示 $\sup\{-\lambda^Tb \mid A^T\lambda+c=0,\ \lambda\succeq 0\}$：它是关于 $c$ 的一族仿射函数的逐点上确界（pointwise supremum），而逐点取上确界保持凹性——正如凸函数的逐点最大值仍是凸函数。弱对偶性（weak duality）保证这个 sup 不超过 $f(c)$；而 LP 的强对偶性（strong duality）在可行时总成立（Slater 条件自动满足），于是等号成立，我们就得到了 $f(c)$ 的凹性刻画。经济学里的影子价格（shadow price）说的正是这件事：乘子 $\lambda$ 度量「放松一单位约束能省多少钱」，即最优值对参数的灵敏度。在 CVX 里把 $c$ 换成仿射表达式嵌进更大的问题仍整体保凸，这正是上一页「不完全指定问题」定义函数的威力所在。

---

<!-- p.26 -->

### <span style="color:#c0392b;">CVX 提示与警告 CVX hints/warnings</span>

<span style="color:#2471a3;">**[section]**</span>

- 当心 `=`（赋值）与 `==`（等式约束）的区别
- 若 `X` 是矩阵，`X >= 0` 是按元素的不等式
- `X >= semidefinite(n)` 的含义是：`X` 按元素大于某个半正定矩阵（这很可能不是你想要的）
- 写 `subject to` 并非必要（但看起来更整洁）
- 确保给目标函数加上括号：
  - 正确：`minimize(c'*x)`
  - 错误：`minimize c'*x`
- 像 `0 <= x <= 1` 这样的双重不等式不能用；应写成 `0 <= x; x <= 1`
- log、exp、熵（entropy）类函数在 CVX 中尚未实现

