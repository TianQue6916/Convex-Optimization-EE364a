> <span style="color:#7f8c8d;">Source: SEE复习课/review3_single.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

## <span style="color:#2471a3;">**[lecture]**</span> <span style="color:#c0392b;">EE364a Review — Disciplined Convex Programming and CVX（EE364a 复习：规范凸规划与 CVX）</span>

<!-- p.1 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">EE364a 复习：规范凸规划与 CVX（Disciplined Convex Programming and CVX）</span>

本讲复习凸优化的「前端」工具链——从求解器到建模语言，围绕以下四个要点展开：

- 凸优化求解器（convex optimization solvers）
- 建模系统（modeling systems）
- 规范凸规划（disciplined convex programming，DCP）
- CVX

<!-- p.2 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">凸优化求解器（Convex optimization solvers）</span>

- **LP 求解器**：可选的非常多（GLPK、Excel、Matlab 的 `linprog` 等）
- **锥求解器（cone solvers）**：通常能处理 LP、SOCP、SDP 锥（或它们的组合）
  - 已有若干可用（SDPT3、SeDuMi、CSDP 等）
- **一般凸求解器**：有一些可用（CVXOPT、MOSEK 等）
- 此外还有大量专用（special purpose）或面向特定应用的求解器
- 也可以自己动手写一个
  （本学季稍后我们会研究、并且亲手编写求解器）

<!-- p.3 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">把问题变换为标准形式（Transforming problems to standard form）</span>

- 你已经见过许多变换技巧，能把一个问题变成等价的、具有标准形式（standard form，如 LP、SDP）的问题
- 这些技巧大大扩展了标准求解器的适用范围
- 但手写代码来完成这类变换往往很痛苦
- 建模系统（modeling system）可以部分自动化这一步

<!-- p.4 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">建模系统（Modeling systems）</span>

一个典型的建模系统：

- 自动完成大部分「变换到标准形式」的工作；支持：
  - 声明优化变量
  - 描述目标函数
  - 描述约束
  - 选择（并配置）求解器
- 拿到一个问题实例后，调用求解器
- 解释并返回求解器的状态（optimal 最优、infeasible 不可行……）
- （求解成功时）把解再变换回原始形式

<!-- p.5 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">现有的一些建模系统（Some current modeling systems）</span>

- **AMPL 与 GAMS**（专有商业软件）
  - 开发于 1980 年代，至今仍在传统运筹学（OR）界广泛使用
  - 不支持凸优化
- **YALMIP**（"Yet Another LMI Parser"）
  - 第一个基于 Matlab 的面向对象建模系统，对凸优化提供专门支持
  - 可调用多种不同求解器；也能处理一些非凸问题
- **CVXMOD/CVXOPT**（尚处 alpha 阶段）
  - 基于 Python，完全采用 GPL 许可
  - 支持锥求解器与自定义求解器
- **CVX**
  - 基于 Matlab，GPL 许可，使用 SDPT3/SeDuMi 求解器

<!-- p.6 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">规范凸规划（Disciplined convex programming）</span>

- 用下面两类元素构成的表达式来描述目标函数与约束：
  - 一组基本原子（atoms，即凸、凹函数）
  - 一组受限的运算/规则（能保持凸性的规则）
- 建模系统持续跟踪记录每个表达式是仿射、凸还是凹
- 规则保证：
  - 被识别为凸（凹）的表达式确实凸（凹）
  - 但某些确实是凸（凹）的表达式不会被识别为凸（凹）
- 用 DCP 描述的问题**按构造即为凸**（convex by construction）

<!-- p.7 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">CVX</span>

- 使用 DCP
- 运行于 Matlab，代码位于 `cvx_begin` 与 `cvx_end` 命令之间
- 底层依赖 SDPT3 或 SeDuMi（LP/SOCP/SDP）求解器
- 更多信息请查阅用户手册（user guide）与在线帮助
- CVX 示例库（example library）收录了一百多个例子

<!-- p.8 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">例：约束范数最小化（Constrained norm minimization）</span>

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

- 在 `cvx_begin` 与 `cvx_end` 之间，x 是一个 CVX 变量
- `subject to` 这一句什么都不做，写上只是为了可读性
- 不等式按逐元素（elementwise）方式解释

<!-- p.9 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">CVX 做了什么（What CVX does）</span>

`cvx_end` 之后，CVX 会：

- 把问题变换成一个 LP
- 调用求解器 SDPT3
- 用（数值形式的）最优解覆盖（对象）x
- 把问题的最优值赋给 `cvx_optval`
- 把问题的状态（此处为 `Solved`）赋给 `cvx_status`

（若问题不可行，`cvx_status` 会是 `Infeasible`，而 x 会是 `NaN`。）

<!-- p.10 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">变量与仿射表达式（Variables and affine expressions）</span>

- 用 `variable name[(dims)] [attributes]`（变量名[(维度)] [属性]）声明变量：
  - `variable x(3);`
  - `variable C(4,3);`
  - `variable S(3,3) symmetric;`（对称）
  - `variable D(3,3) diagonal;`（对角）
  - `variables y z;`
- 构造仿射表达式（affine expressions）：
  - `A = randn(4, 3);`
  - `variables x(3) y(4);`
  - `3*x + 4`
  - `A*x - y`
  - `x(2:3)`
  - `sum(x)`

<!-- p.11 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">一些函数（Some functions）</span>

下表中「凸」即 CVX 文档标注的 cvx，「凹」即 ccv；「非减/非增」（nondecr/nonincr）指该函数对相应参数单调不减/不增；$x_+ = \max\{x, 0\}$。

| 函数 | 含义 | 属性 |
|---|---|---|
| `norm(x, p)` | $\|x\|_p$ | 凸 |
| `square(x)` | $x^2$ | 凸 |
| `square_pos(x)` | $(x_+)^2$ | 凸，非减 |
| `pos(x)` | $x_+$ | 凸，非减 |
| `sum_largest(x,k)` | $x_{[1]} + \cdots + x_{[k]}$ | 凸，非减 |
| `sqrt(x)` | $\sqrt{x}$（$x \ge 0$） | 凹，非减 |
| `inv_pos(x)` | $1/x$（$x > 0$） | 凸，非增 |
| `max(x)` | $\max\{x_1, \ldots, x_n\}$ | 凸，非减 |
| `quad_over_lin(x,y)` | $x^2/y$（$y > 0$） | 凸，对 y 非增 |
| `lambda_max(X)` | $\lambda_{\max}(X)$（$X = X^T$） | 凸 |
| `huber(x)` | $\begin{cases} x^2, & \lvert x \rvert \le 1 \\ 2\lvert x \rvert - 1, & \lvert x \rvert > 1 \end{cases}$ | 凸 |

<!-- p.12 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">组合规则（Composition rules）</span>

- 可以用合法的组合规则（composition rules）来组合原子，例如：
  - 仿射函数的凸函数是凸的
  - 凸函数取负得到凹函数
  - 凸且非减的函数作用于凸函数，结果为凸
  - 凹且非减的函数作用于凹函数，结果为凹
- 对凸函数 $h$：$h(g_1, \ldots, g_k)$ 被识别为凸，若对每个 $i$：
  - $g_i$ 是仿射的；或
  - $g_i$ 是凸的且 $h$ 对第 $i$ 个参数非减；或
  - $g_i$ 是凹的且 $h$ 对第 $i$ 个参数非增
- 对凹函数 $h$：$h(g_1, \ldots, g_k)$ 被识别为凹，若对每个 $i$：
  - $g_i$ 是仿射的；或
  - $g_i$ 是凸的且 $h$ 对第 $i$ 个参数非增；或
  - $g_i$ 是凹的且 $h$ 对第 $i$ 个参数非减

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> DCP 组合规则本质上是为数学表达式设计的一套「类型系统」：每个表达式携带「凸/凹/仿射 + 单调性」的类型标签，规则像编译器做类型检查那样在语法层面传播这些标签。你在 CSAPP 里写过 C 代码就会体会到，类型系统是保守的——它会拒绝一些实际安全的程序，以此换取「凡通过检查必安全」的可靠保证。DCP 完全同理：`sqrt(sum(square(x)))` 在数学上确实是凸的，但规则库中查不到这条推导路径，于是被拒绝；改写成 `norm(x)` 就能通过。这种「不完备但可靠」的取舍是自动验证领域的经典设计：要么牺牲完备性、要么牺牲可靠性，CVX 选择了后者，因为它绝不能向求解器谎报一个非凸问题是凸的。再深一层：凸性是关于整条函数曲线的全局性质，单看若干数值样本无法判定，所以建模系统必须像编译器解析语法树那样「读出」表达式的结构，而不是只做数值求值。

<!-- p.13 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">被识别为合法的例子（Valid examples）</span>

设 $u, v, x, y$ 为标量变量；$X$ 是 $3 \times 3$ 对称矩阵变量。

- 凸：
  - `norm(A*x - y) + 0.1*norm(x, 1)`
  - `quad_over_lin(u - v, 1 - square(v))`
  - `lambda_max(2*X - 4*eye(3))`
  - `norm(2*X - 3, 'fro')`
- 凹：
  - `min(1 + 2*u, 1 - max(2, v))`
  - `sqrt(v) - 4.55*inv_pos(u - v)`

<!-- p.14 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">被拒绝的例子（Rejected examples）</span>

设 $u, v, x, y$ 为标量变量。

- 既不凸也不凹：
  - `square(x) - square(y)`
  - `norm(A*x - y) - 0.1*norm(x, 1)`
- 因 DCP 规则集有限而被拒绝（表达式本身实际是凸的）：
  - `sqrt(sum(square(x)))`（可改用 `norm(x)`）
  - `square(1 + x^2)`（可改用 `square_pos(1 + x^2)`，或 `1 + 2*pow_pos(x, 2) + pow_pos(x, 4)`）

<!-- p.15 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">集合（Sets）</span>

- 有些约束用凸集来表达更自然
- CVX 中集合的实现方式：创建一个被约束到该集合上的无名（unnamed）变量
- 例子：
  - `semidefinite(n)`（半正定锥）
  - `nonnegative(n)`（非负象限）
  - `simplex(n)`（单纯形）
  - `lorentz(n)`（洛伦兹锥，即二阶锥 second-order cone）
- 例如 `semidefinite(n)` 返回一个无名的（对称矩阵）变量，且被约束为半正定（positive semidefinite）

<!-- p.16 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">使用半正定锥（Using the semidefinite cone）</span>

变量：$X$（对称矩阵）、$z$（向量）、$t$（标量）；常数：$A$、$B$（矩阵）。

- `X == semidefinite(n)`
  - 表示 $X \in \mathbf{S}^n_+$（即 $X \succeq 0$）
- `A*X*A' - X == B*semidefinite(n)*B'`
  - 表示存在 $Z \succeq 0$ 使得 $AXA^T - X = BZB^T$
- `[X z; z' t] == semidefinite(n+1)`
  - 表示

```math
\begin{bmatrix} X & z \\ z^T & t \end{bmatrix} \succeq 0
```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 半正定锥这一节值得用 Strang 线性代数的语言重述一遍：$X \succeq 0$ 就是「对称矩阵的所有特征值非负」。这正是 18.065 中正定性（positive definiteness）概念从单个矩阵的对角化推广为矩阵不等式的形式。`lambda_max(X)` 之所以是凸原子，可以用 Rayleigh 商 $\lambda_{\max}(X) = \max_{x \ne 0} \frac{x^T X x}{x^T x}$ 来看：它是无穷多个关于 $X$ 的线性函数 $x^T X x = \operatorname{tr}(x x^T X)$ 的逐点最大值，而「逐点取最大保持凸性」正是第 11 页 `max(x)` 是凸原子的同一原理，只是从向量升级到了矩阵。若把对称矩阵空间看作带 Frobenius 内积 $\operatorname{tr}(A^T B)$ 的「广义欧氏空间」，半正定锥便扮演非负象限的角色：LP 中的 $x \ge 0$ 升级为 SDP 中的 $X \succeq 0$。想通这一点，`[X z; z' t] == semidefinite(n+1)` 这类分块拼大矩阵的写法就不再神秘——它把一个分块矩阵不等式（其背后是 Schur 补条件）整体交给求解器处理。

<!-- p.17 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">目标函数与约束（Objectives and constraints）</span>

- 目标函数可以是：
  - `minimize(凸表达式)`
  - `maximize(凹表达式)`
  - 省略（即可行性问题 feasibility problem）
- 约束可以是：
  - 凸表达式 `<=` 凹表达式
  - 凹表达式 `>=` 凸表达式
  - 仿射表达式 `==` 仿射表达式
  - 省略（即无约束问题）

<!-- p.18 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">一个更复杂些的例子（More involved example）</span>

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

按 DCP 检查：目标中 `norm(X)` 是凸的，`-10*sqrt(y)` 是凹函数取负、也为凸，两者相加仍凸，故 `minimize` 合法；`X - A == semidefinite(5)` 是半正定锥集合约束，其余约束均为仿射。

<!-- p.19 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">定义新函数（Defining new functions）</span>

- 可以用已有原子构造新函数
- 例：凸的死区函数（deadzone function）

```math
f(x) = \max\{|x| - 1,\; 0\} =
\begin{cases}
0, & |x| \le 1\\
x - 1, & x > 1\\
1 - x, & x < -1
\end{cases}
```

- 创建文件 `deadzone.m`，内容为：

```matlab
function y = deadzone(x)
y = max(abs(x) - 1, 0)
```

- `deadzone` 在 CVX 之内与之外都同样可用（都给出正确的含义）

<!-- p.20 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">通过不完备指定的问题定义函数（Defining functions via incompletely specified problems）</span>

- 设 $f_0, \ldots, f_m$ 关于 $(x, z)$ 是凸的
- 令 $\varphi(x)$ 为下述凸问题的最优值，其中 $z$ 为变量、$x$ 为参数：

```math
\begin{array}{ll}
\text{minimize} & f_0(x, z)\\
\text{subject to} & f_i(x, z) \le 0, \quad i = 1, \ldots, m\\
& A_1 x + A_2 z = b
\end{array}
```

- $\varphi$ 是凸函数
- 上面的问题有时被称为不完备指定（incompletely specified），因为 $x$ 尚未给定
- 一个不完备指定的凹最大化问题则定义一个凹函数

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 「不完备指定的问题」背后是一个极值得记住的原理：凸函数对一部分变量取最小值（partial minimization）后仍是凸函数。用上图（epigraph）看最直观：$\varphi$ 的上图恰好是 $f_0$ 的上图与约束集合「拼接」后在 $x$ 坐标方向上的投影，而凸集的线性投影仍是凸集，于是 $\varphi$ 的上图是凸集，$\varphi$ 自然是凸函数。你其实早就见过结构完全平行的论证：概率论里对联合分布做边缘化（marginalization）——「对消去的变量求和后，边缘分布仍是合法分布」，与这里的「对消去的变量最小化后，凸性保持」是同一种投影思想。接下来两页把这个原理工程化：$g(y)$ 定义为「最大化 $t$ 使得 $t_+ + t_+^{1.5} + t_+^{2.5} \le y$」的最优值，虽无闭式公式，却完全合法地充当一个凹原子。这也说明：在凸优化中，函数不必由显式公式定义，「某个凸优化问题的最优值」本身就是一种可计算、可组合的函数表示。

<!-- p.21 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">用不完备指定的问题定义 CVX 函数（CVX functions via incompletely specified problems）</span>

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

- 当 $x$ 是数值时，函数 `phi` 通过求解上述问题来求值
- 函数 `phi` 也可以用在 CVX 描述内部——凡是能使用凸函数的地方都可以使用它

<!-- p.22 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">简单例子：两元素取最大（Simple example: Two element max）</span>

- 创建文件 `max2.m`，内容为：

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

- 这些约束定义的正是 max 函数的上图（epigraph）
- 可以再加一段逻辑：当 $x, y$ 是数值时直接返回 `max(x,y)`
  （否则，为了求两个数的最大值居然要解一个 LP！）

<!-- p.23 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">一个更复杂的例子（A more complex example）</span>

- $f(x) = x + x^{1.5} + x^{2.5}$，定义域 $\operatorname{dom} f = \mathbf{R}_+$，是凸的、单调递增（monotone increasing）函数
- 它的反函数 $g = f^{-1}$ 是凹的、单调递增的，定义域 $\operatorname{dom} g = \mathbf{R}_+$
- $g$ 没有闭式（closed-form）表达式
- $g(y)$ 是下述问题的最优值：

```math
\begin{array}{ll}
\text{maximize} & t\\
\text{subject to} & t_+ + t_+^{1.5} + t_+^{2.5} \le y
\end{array}
```

（当 $y < 0$ 时该问题不可行，故最优值为 $-\infty$。）

<!-- p.24 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">实现：反函数 g 的 CVX 函数（Implementation of g）</span>

实现如下：

```matlab
function cvx_optval = g(y)
cvx_begin
variable t;
maximize(t)
subject to
pos(t) + pow_pos(t, 1.5) + pow_pos(t, 2.5) <= y;
cvx_end
```

可以把它当普通函数使用（如 `g(14.3)`），也可以在 CVX 内部把它当凹函数使用：

```matlab
cvx_begin
variables x y;
minimize(quad_over_lin(x, y) + 4*x + 5*y)
subject to
g(x) + 2*g(y) >= 2;
cvx_end
```

<!-- p.25 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">例：LP 最优值作为参数的函数（Example）</span>

- LP 的最优值 $f(c) = \inf\{c^T x \mid Ax \preceq b\}$ 是关于 $c$ 的凹函数
- 由对偶性（假设 $Ax \preceq b$ 可行）可得

```math
f(c) = \sup\{-\lambda^T b \mid A^T \lambda + c = 0, \; \lambda \succeq 0\}
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

- 在 `lp_opt_val(A,b,c)` 中，$A$、$b$ 必须是常数；$c$ 可以是仿射表达式

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这一页把弱对偶性（weak duality）与凹性接在了一起：$f(c) = \inf\{c^T x \mid Ax \preceq b\}$ 是一族关于 $c$ 的线性函数 $c^T x$ 在可行集上取下确界，而「线性函数逐点取下确界得到凹函数」（取上确界则得到凸函数），这解释了 LP 最优值为何随成本向量 $c$ 凹变化。右端用拉格朗日乘子 $\lambda$ 把它改写为对 $-\lambda^T b$ 取上确界，是教材第 5 章对偶理论的直接应用。你在 6.006 接触过的最大流-最小割定理正是它的特例：网络流 LP 的对偶给出最小割，「原问题最优值 = 对偶问题最优值」在那里表现为 max-flow = min-cut。乘子 $\lambda$ 还有个经济学名字——影子价格（shadow price）：它度量「把某个约束松弛一个单位，最优值能改善多少」，与灵敏度分析中的解释一脉相承。建立起「问题的最优值本身是参数的函数」这一视角后，对偶就不再是应试技巧，而是同一对象的另一种（往往更好算的）描述。

<!-- p.26 -->
### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">CVX 提示与警告（CVX hints/warnings）</span>

- 当心 `=`（赋值）与 `==`（等式约束）的区别
- 若 $X$ 是矩阵，`X >= 0` 表示逐元素（elementwise）不等式
- `X >= semidefinite(n)` 的含义是：$X$ 逐元素大于某个半正定矩阵（这多半不是你想要的）
- 写 `subject to` 并非必需（但看起来更整洁）
- 确保目标函数用括号包起来：
  - 正确：`minimize(c'*x)`
  - 错误：`minimize c'*x`
- 像 `0 <= x <= 1` 这样的双重不等式行不通；请改写成 `0 <= x; x <= 1`
- log、exp、熵（entropy）类函数在 CVX 中尚未实现
