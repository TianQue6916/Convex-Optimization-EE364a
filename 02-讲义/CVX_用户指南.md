# CVX 用户指南 · 双语译注

> 本文件由 WorkBuddy 分片产出后合并；分片边界已平滑。

**作者（Authors）**
- Michael Grant —— mcgrant@stanford.edu
- Stephen Boyd —— boyd@stanford.edu
- Yinyu Ye —— yyye@stanford.edu

**日期（Date）**：June 24, 2008（2008 年 6 月 24 日）

---

## 目录（Contents）

| 章 | 标题（Title） | 页码 |
|----|--------------|------|
| 1 | Introduction（引言） | 4 |
| 1.1 | What is cvx?（什么是 CVX？） | 4 |
| 1.2 | What is disciplined convex programming?（什么是 disciplined convex programming？） | 5 |
| 1.3 | About this version（关于本版本） | 5 |
| 1.4 | Feedback（反馈） | 6 |
| 1.5 | What cvx is not（CVX 不是什么） | 6 |
| 2 | A quick start（快速上手） | 8 |
| 2.1 | Least-squares（最小二乘） | 8 |
| 2.2 | Bound-constrained least-squares（带界约束的最小二乘） | 10 |
| 2.3 | Other norms and functions（其他范数与函数） | 11 |
| 2.4 | Other constraints（其他约束） | 13 |
| 2.5 | An optimal trade-off curve（最优权衡曲线） | 15 |
| 3 | The basics（基础知识） | 17 |
| 3.1 | Data types for variables（变量的数据类型） | 17 |
| 3.2 | Objective functions（目标函数） | 18 |
| 3.3 | Constraints（约束） | 18 |
| 3.4 | Functions（函数） | 19 |
| 3.5 | Sets（集合） | 19 |
| 3.6 | Dual variables（对偶变量） | 21 |
| 3.7 | Expression holders（表达式容器） | 23 |
| 4 | The DCP ruleset（DCP 规则集） | 25 |
| 4.1 | A taxonomy of curvature（曲率的分类） | 25 |
| 4.2 | Top-level rules（顶层规则） | 26 |
| 4.3 | Constraints（约束） | 26 |
| 4.4 | Expression rules（表达式规则） | 27 |
| 4.5 | Functions（函数） | 28 |
| 4.6 | Compositions（复合） | 29 |
| 4.7 | Monotonicity in nonlinear compositions（非线性复合中的单调性） | 31 |
| 4.8 | Scalar quadratic forms（标量二次型） | 32 |
| 5 | Adding new functions to the cvx atom library（向 CVX 的 atom 库添加新函数） | 34 |
| 5.1 | New functions via the DCP ruleset（通过 DCP 规则集添加新函数） | 34 |
| 5.2 | New functions via partially specified problems（通过部分指定问题添加新函数） | 35 |
| 6 | Semidefinite programming using cvx（用 CVX 求解半定规划） | 39 |
| 7 | Geometric programming using cvx（用 CVX 求解几何规划） | 41 |
| 7.1 | Top-level rules（顶层规则） | 41 |
| 7.2 | Constraints（约束） | 42 |
| 7.3 | Expressions（表达式） | 42 |
| 8 | Advanced topics（高级主题） | 44 |
| 8.1 | Solver selection（solver 选择） | 44 |
| 8.2 | Controlling solver precision（控制 solver 精度） | 44 |
| 8.3 | Miscellaneous cvx commands（其他 CVX 命令） | 46 |
| 8.4 | Assignments versus equality constraints（赋值与等式约束） | 46 |
| 8.5 | Indexed dual variables（带索引的对偶变量） | 48 |
| A | Installation and compatability（安装与兼容性） | 50 |
| A.1 | Basic instructions（基本说明） | 50 |
| A.2 | MEX file compilation（MEX 文件编译） | 51 |
| A.3 | About SeDuMi and SDPT3（关于 SeDuMi 与 SDPT3） | 51 |
| A.4 | A Matlab 7.0 issue（一个 Matlab 7.0 的问题） | 52 |
| B | Operators, functions, and sets（运算符、函数与集合） | 53 |
| B.1 | Basic operators and linear functions（基本运算符与线性函数） | 53 |
| B.2 | Nonlinear functions（非线性函数） | 54 |
| B.3 | Sets（集合） | 59 |
| C | cvx status messages（CVX 状态消息） | 61 |
| D | Advanced solver topics（高级 solver 主题） | 63 |
| D.1 | The successive approximation method（逐次逼近法） | 63 |
| D.2 | Irrational powers（无理数次幂） | 63 |
| D.3 | Overdetermined problems（超定问题） | 64 |
| E | Acknowledgements（致谢） | 65 |

---

# 1　引言（Introduction）

## 1.1　什么是 CVX？（What is cvx?）

**cvx** is a modeling system for **disciplined convex programming**（CVX 是一套用于 **disciplined convex programming（规范化凸规划）** 的建模系统）。

**Disciplined convex programs**, or **DCPs**, are convex optimization problems that are described using a limited set of construction rules, which enables them to be analyzed and solved efficiently.
**Disciplined convex programs（规范化凸规划）**，简称 **DCP**，是指使用一组受限的构造规则来描述的凸优化问题；正因为有这些规则约束，这类问题才能被高效地分析并求解。

cvx can solve standard problems such as **linear programs (LPs)**（线性规划）、**quadratic programs (QPs)**（二次规划）、**second-order cone programs (SOCPs)**（二阶锥规划），以及 **semidefinite programs (SDPs)**（半定规划）；但相比于直接针对其中某一类问题调用 solver（求解器），cvx 能极大简化「描述问题」这一步的工作量。cvx 还能求解远比上述复杂得多的凸优化问题，其中许多问题涉及**不可微函数（nondifferentiable functions）**，例如 $\ell_1$ 范数。你可以用 cvx 方便地建立并求解**带约束的范数最小化（constrained norm minimization）**、**熵最大化（entropy maximization）**、**行列式最大化（determinant maximization）** 以及许多其他问题。

要有效地使用 cvx，你至少需要了解一点凸优化知识。关于凸优化的背景资料，可参阅《Convex Optimization》一书 [BV04]，在线地址为 `www.stanford.edu/~boyd/cvxbook/`；也可参考斯坦福课程 EE364A，地址为 `www.stanford.edu/class/ee364a/`。

cvx 在 **Matlab** [Mat04] 中实现，实际上是把 Matlab 变成了一门**优化建模语言（optimization modeling language）**。模型规格（model specification）用常见的 Matlab 运算和函数来构建，而且标准的 Matlab 代码可以与这些规格自由混合使用。这种结合使得「构造优化问题所需的计算」以及「处理求解结果」都变得简单。例如，通过不断改变约束、构造并求解一系列优化问题，就能轻松地算出**最优权衡曲线（optimal trade-off curve）**。再举一个例子：cvx 可以作为某个更大系统的组件来使用，该系统本身要用到凸优化，比如**分支定界法（branch and bound method）**或某个工程设计框架。

cvx 还提供了特殊模式，以简化来自两个特定问题类别的问题构造：

- 在 **SDP 模式（SDP mode）** 下，cvx 对不等式运算符采用**矩阵解释（matrix interpretation）**，从而使**线性矩阵不等式（linear matrix inequalities, LMIs）** 与 SDP 能以更自然的形式表达。
- 在 **GP 模式（GP mode）** 下，cvx 接受**几何规划（geometric programming）** 的所有特殊函数与组合规则，包括**单项式（monomials）**、**正项式（posynomials）** 与**广义正项式（generalized posynomials）**，并把这类问题变换成凸形式，从而得以高效求解。关于几何规划的背景资料，可参阅教程论文 [BKVH05]，地址为 `www.stanford.edu/~boyd/papers/gp_tutorial.html`。

cvx 由 Michael Grant 设计与实现，并得到 Stephen Boyd 与 Yinyu Ye 的协助 [GBY06]。它吸收了早期工作的思想，包括 L¨ofberg [L¨of05]、Dahl 与 Vandenberghe [DV05]、Crusius [Cru02]、Wu 与 Boyd [WB00]，以及其他许多人的成果。该建模语言遵循 AMPL [FGK99] 或 GAMS [BKMR98] 的精神；然而与这些软件包不同的是，cvx 从一开始就为**充分利用凸性（fully exploit convexity）** 而设计。在 Matlab 中实现 cvx 的具体方法大量借鉴了 YALMIP [L¨of05]。我们也希望今后能为其他平台开发 cvx 的版本。

---

> **译注说明**
> 本文件为 CVX 用户指南第 1–4 页（封面、目录、第 1 章 1.1 节）的双语译注。原文此处仅覆盖到 1.1 节「What is cvx?」末尾，后续章节（1.2 起）将在后续页中处理。

---

## 1.2 什么是 disciplined convex programming？

**disciplined convex programming（DCP，规范化凸规划）**：一种由 Michael Grant、Stephen Boyd 与 Yinyu Ye 在 [GBY06, Gra04] 中提出的、用于**构造凸优化问题**的方法论。判断标准是：符合它规则的问题可以被**快速且自动地判定为凸**并转换为可求解形式。

Disciplined convex programming 是一种构造凸优化问题的方法论，由 Michael Grant、Stephen Boyd 和 Yinyu Ye 提出 [GBY06, Gra04]。它的目的，是支持用户从一开始就想要构造的、**凸的**优化问题的表达（formulation）与构建（construction）。

Disciplined convex programming 施加了一组约定或规则，我们称之为 **DCP ruleset（DCP 规则集）**。遵守该规则集的问题可以被快速、自动地判定为凸，并转换成可求解的形式。违反该规则集的问题会被**拒绝**，哪怕这个问题本身是凸的。这并不是说这类问题不能用 DCP 求解；它们只是需要被重写为符合 DCP 规则集的形式。

关于 DCP 规则集的详细描述见 §4，凡是打算真正使用 cvx 的人都应当理解它，这一点很重要。该规则集简单易学，且源自凸分析（convex analysis）的基本原理。作为接受规则集所施加限制的回报，我们能获得相当大的好处，例如：问题被自动转换为可求解形式，以及对不可微函数（nondiﬀerentiable functions）的完全支持。实践中我们发现，disciplined convex program（规范化凸规划）与其自然的数学形式非常接近。

## 1.3 关于本版本

**Supported solvers（所支持的求解器）。** 本版本的 cvx 支持两个核心求解器（solver）：SeDuMi [Stu99] 与 SDPT3 [TTT06]，其中 SDPT3 为默认求解器。未来的 cvx 版本可能会支持其他求解器，例如 MOSEK [MOS05] 或 CVXOPT [DV05]。SeDuMi 与 SDPT3 是用 Matlab 编写的、开源的**内点法（interior-point）**求解器，可处理 LP（线性规划）、SOCP（二阶锥规划）、SDP（半定规划）以及它们的组合。

**Problems handled exactly（精确处理的问题）。** 当问题说明中的所有函数都能表示为 LP、SOCP 或 SDP 形式时，cvx 会把指定问题转换为 LP、SOCP 或 SDP。这涵盖了相当广泛的函数，例如最小值与最大值、绝对值、二次型（quadratic forms）、对称矩阵的最小与最大特征值、幂函数 $x^p$，以及 $\ell_p$ 范数（当 $p$ 为有理数时）。

**Problems handled with (good) approximations（用（良好的）近似处理的问题）。** 对少数几个函数，cvx 会做一个（良好的）近似，把指定问题转换成可由 LP、SOCP、SDP 组合求解器处理的形式。例如，当使用幂函数或 $\ell_p$ 范数、且指数 $p$ 为无理数时，cvx 会用附近的一个有理数替换 $p$。正态累积分布的对数 $\log\Phi(x)$ 会被替换为一个与 SDP 兼容的近似。

**Problems handled with successive approximation（用逐次近似处理的问题）。** 本版本 cvx 新增了对若干无法通过 LP、SOCP 或 SDP 精确表示的函数支持，包括 log、exp、log-sum-exp $\log(\exp x_1 + \cdots + \exp x_n)$、entropy（熵），以及 Kullback-Leibler divergence（KL 散度）。这些问题通过求解一列（通常只有少量几个）SDP 来求解，其解能达到核心求解器的完整精度。但另一方面，这种技术可能比核心求解器直接处理这些函数要慢得多。

逐次近似方法在附录 D.1 中简要描述。几何规划（geometric problems）现在也用这种方式求解；而在以前的版本中，是用近似来处理的。

最终，我们将把 cvx 对接到一个**原生支持这些函数**的求解器上，这会给含这些函数的问题求解带来大幅加速。在那之前，用户应当知道，涉及这些函数的问题用当前版本的 cvx 求解可能会很慢。出于这个原因，当用到其中某个函数时，会警告用户将采用逐次近似技术。

我们要强调，**大多数用户并不需要知道 cvx 如何处理他们的问题**；重要的是哪些函数和运算能被处理。关于 cvx 支持函数的完整列表，参见附录 B，或者输入以下在线帮助命令来查看：`help cvx/builtins`（用于 Matlab 中已有的函数，如 `sqrt` 或 `log`），或 `help cvx/functions`（用于 Matlab 中没有的函数，如 `lambda_max`）。

## 1.4 反馈（Feedback）

请将您的意见发送给 Michael Grant（mcgrant@stanford.edu）或 Stephen Boyd（boyd@stanford.edu）。如果您发现您认为是 bug 的问题，请在来信中包含以下内容，以便我们重现并修复该问题：

- 引发错误的 cvx 模型及其支撑数据
- 它所产生的任何错误信息（error message）的副本
- cvx 的版本号与构建号（build number）
- 您正在运行的 Matlab 的版本号
- 您所使用的操作系统的名称与版本

后三项信息都可以通过在 MATLAB 命令提示符下输入以下命令获得：

```matlab
cvx_version
```

只需把它的输出复制到您的电子邮件中即可。

## 1.5 cvx 不是什么

cvx 并不是用来检查您的问题是否凸的工具。要有效地使用 cvx，您需要对凸优化有所了解；否则，您就像是谚语里那只在打字机前的猴子，指望（碰巧）敲出一个有效的规范化凸规划。

另一方面，如果 cvx 接受了您的问题，您就可以确定它是凸的。结合凸优化课程（或自学），cvx（尤其是它的错误信息）在帮助学习一些基本的凸分析时非常有用。虽然 cvx 在您违反 DCP 规则集时会尽量给出有帮助的错误信息，但有时它也会给出相当晦涩难懂的错误信息。

cvx 不适用于非常大的问题，所以如果您的问题非常大（例如，一个大型图像处理问题），cvx 很可能无法很好地工作（甚至根本无法工作）。对于这类问题，您很可能需要直接调用某个求解器，或者自行开发方法，才能获得所需的效率。

然而，对于这类问题，cvx 仍能发挥重要作用。在着手开发专门的大规模方法之前，您可以先用 cvx 求解问题的**缩小版**或**简化版**，以便快速试验您究竟想求解什么样的问题。

以图像重建（image reconstruction）为例，您可以用 cvx 在 $50 \times 50$ 像素的图像上试验不同的问题表达。

cvx 可以求解许多中、大规模问题，只要它们具有可利用的结构（例如稀疏性 sparsity），并且您避免使用 `for` 循环（在 Matlab 中 `for` 循环可能很慢）以及像 `log`、`exp` 这样需要逐次近似的函数。如果您在求解大规模问题实例时遇到困难，请务必联系我们；我们也许能建议一个 cvx 可以更高效处理的等价表达。

---

## 2 快速入门（A quick start）

安装好 cvx 之后（见 §A），您就可以开始使用它了：把一段 cvx 说明（cvx speciﬁcation）输入到 Matlab 脚本或函数中，或者直接在命令提示符下输入。为了把 cvx 说明与周围的 Matlab 代码区分开来，它们以语句 `cvx_begin` 开头，以语句 `cvx_end` 结尾。一段 cvx 说明可以包含任何普通的 Matlab 语句，以及用于声明**原始（primal）优化变量**和**对偶（dual）优化变量**、指定约束和目标函数的 cvx 专用命令。

在一段 cvx 说明中，优化变量**没有数值**；相反，它们是特殊的 Matlab 对象。这使得 Matlab 能够区分普通命令与 cvx 的目标函数和约束。当 Matlab 读取一段 cvx 说明时，它会构建该优化问题的一个内部表示。如果它遇到违反 disciplined convex programming 规则的情况（例如无效地使用了某条复合规则 composition rule，或一个无效的约束），就会生成一条错误信息。当 Matlab 到达 `cvx_end` 命令时，它就完成了把 cvx 说明转换成**规范形式（canonical form）**的过程，并调用底层核心求解器来求解。

如果优化成功，cvx 说明中声明的优化变量就会从对象转换成普通的 Matlab 数值，可用于任何后续的 Matlab 计算。此外，cvx 还会再赋值几个相关的 Matlab 变量。例如，其中一个给出问题的**状态（status）**（即是否找到了最优解，还是判定问题不可行 infeasible 或无界 unbounded）。另一个给出问题的最优值（optimal value）。还可以赋给对偶变量（dual variables）。

随着我们引入一些简单的例子，这一处理流程会变得更加清晰。我们邀请读者在 Matlab 中真正跟着这些例子操作一遍，方法是运行 cvx 发行版 `examples` 子目录中的 `quickstart` 脚本。例如，如果您用的是 Windows，并且把 cvx 发行版安装在了目录 `D:\Matlab\cvx` 中，那么您可以在 Matlab 命令提示符下输入：

```matlab
cd D:\Matlab\cvx\examples
quickstart
```

该脚本会自动打印其代码的关键摘录，并周期性地暂停，以便您检查它的输出。（按 "Enter" 或 "Return" 键继续。）本文档中伴随代码摘录出现的行号，与文件 `quickstart.m` 中的行号一一对应。

## 2.1 最小二乘（Least-squares）

我们首先考虑最基本的凸优化问题——最小二乘。在最小二乘问题中，我们寻找 $x \in \mathbb{R}^n$，使 $\|Ax - b\|_2$ 最小，其中 $A \in \mathbb{R}^{m \times n}$ 是瘦高且满秩的（即 $m \geq n$ 且 $\mathrm{Rank}(A) = n$）。让我们在 Matlab 中为 $m$、$n$、$A$ 和 $b$ 创建一些测试问题数据：

（原文在此页结束，后续代码示例见下一页。）

---

## 2 快速开始（Quick Start，续）

### 2.1 最小二乘（Least-squares）

首先为问题设定数据。令 $m=16$、$n=8$，并随机生成矩阵 $A$ 与向量 $b$：

```matlab
m = 16; n = 8;
A = randn(m,n);
b = randn(m,1);
```

（我们刻意选取较小的 $m$、$n$ 值，好让输出结果可读。）那么，最小二乘解 $x = (A^TA)^{-1}A^Tb$ 可以用反斜杠运算符（backslash operator）轻松算出：

```matlab
x_ls = A \ b;
```

用 CVX，同一个问题可以这样求解：

```matlab
cvx_begin
    variable x(n);
    minimize( norm(A*x-b) );
cvx_end
```

（这里的缩进纯粹是为了排版美观，是可有可无的。）我们逐行来审视这段 specification（问题规格说明）：

- 第 23 行 `cvx_begin` 为新的 CVX specification 创建一个占位符，并让 MATLAB 准备好接受变量声明（variable declarations）、约束（constraints）、目标函数（objective function）等等。
- 第 24 行 `variable x(n);` 声明 $x$ 是一个维数为 $n$ 的优化变量（optimization variable）。CVX 要求：所有问题变量都必须先在目标函数或约束中被使用之前完成声明。
- 第 25 行 `minimize( norm(A*x-b) );` 指定一个要最小化的目标函数；本例中即 $Ax - b$ 的欧几里得范数（Euclidean norm），也就是 $\ell_2$ 范数（ℓ2-norm）。
- 第 26 行 `cvx_end` 标志这段 CVX specification 结束，并触发问题求解。

反斜杠的写法显然更简单——解一个简单的最小二乘问题完全没必要动用 CVX。但这个例子相当于 CVX 里的 “Hello world!” 程序，即：一段真正能干活、却又最简单的代码段。

如果在第 24 行之后、`cvx_end` 命令之前，在 MATLAB 提示符下输入 `x`，你会看到类似这样的输出：

```matlab
x =
    cvx affine expression (8x1 vector)
```

这是因为在一段 specification 内部，变量还没有数值；它们是被设计用来表示问题变量及含变量的表达式（expressions）的 MATLAB 对象（objects）。同理，由于目标函数 `norm(A*x-b)` 中含有一个 CVX 变量，它同样没有数值，也是用一个 MATLAB 对象来表示的。

当 MATLAB 执行到 `cvx_end` 命令时，最小二乘问题即被求解，MATLAB 变量 `x` 被覆盖为最小二乘问题的解，即 $(A^TA)^{-1}A^Tb$。此时 `x` 是一个普通的、长度为 $n$ 的数值向量，与传统方法得到的结果一致，至少在 solver（求解器）的精度范围内如此。此外，还会额外创建两个 MATLAB 变量：

- `cvx_optval`：存放目标函数的值，即 $\|Ax-b\|_2$；
- `cvx_status`：存放一个描述计算状态的字符串。本例中 `cvx_status` 会存放字符串 `Solved`。`cvx_status` 可能的取值及其含义，参见附录 C。

这三个量——`x`、`cvx_optval` 和 `cvx_status`——现在都可以自由地在其他 MATLAB 语句中使用，就跟任何普通的数值或字符串值一样。¹

> ¹ 如果你在命令提示符下输入 `who` 或 `whos`，可能还会看到其他不熟悉的变量。任何以 `cvx` 前缀开头的变量都是 CVX 保留作内部使用的，不应修改。

设定一个简单的最小二乘问题，几乎没什么出错的空间；但万一出错，你会收到一条 error（错误）或 warning（警告）消息。例如，如果你把第 25 行替换为

```matlab
maximize( norm(A*x-b) );
```

也就是要求最大化这个范数，你会得到一条 error message（错误信息），指出（至少在 disciplined convex programming（disciplined convex programming，规则化凸规划，DCP）中）凸函数不能被最大化：

```
??? Error using ==> maximize
Disciplined convex programming error:
Objective function in a maximization must be concave.
```

> 中文解释：这条错误信息的含义是——在一个最大化问题（maximization）里，目标函数必须是 concave（凹）函数；而范数 $\|Ax-b\|$ 是 convex（凸）函数，因此不能作为最大化目标。

---

### 2.2 带界约束的最小二乘（Bound-constrained least-squares）

假设我们想给上面的最小二乘问题加上一些简单的上下界约束，即我们要解：

```math
\begin{aligned}
&\text{minimize} && \|Ax - b\|_2 \\
&\text{subject to} && l \preceq x \preceq u,
\end{aligned}
```

其中 $l$ 与 $u$ 是给定的数据，是与变量 $x$ 同维的向量。向量不等式 $u \preceq v$ 表示分量逐项成立，即对所有 $i$ 都有 $u_i \le v_i$。这个问题不能再套用简单的反斜杠写法来解，但可以转化为一个 quadratic program（QP，二次规划），只要你手头有某种 QP 软件，就不难求解。

让我们给 $l$ 和 $u$ 赋一些数值：

```matlab
bnds = randn(n,2);
l = min( bnds, [], 2 );
u = max( bnds, [], 2 );
```

那么，如果你有 MATLAB 的 Optimization Toolbox（优化工具箱）[Mat05]，就可以用 `quadprog` 函数这样求解：

```matlab
x_qp = quadprog( 2*A'*A, -2*A'*b, [], [], [], [], l, u );
```

实际上这里最小化的是范数的平方，与最小化范数本身是等价的。相比之下，对应的 CVX specification 写作：

```matlab
cvx_begin
    variable x(n);
    minimize( norm(A*x-b) );
    subject to
        x >= l;
        x <= u;
cvx_end
```

这段 CVX specification 新加了三行 CVX 代码：

- 第 62 行的 `subject to` 语句本身什么都不做——CVX 提供这个语句只是为了提高 specification 的可读性，完全可以省略。
- 第 63、64 行 `x >= l;` 和 `x <= u;` 表示 $2n$ 个不等式约束 $l \preceq x \preceq u$。

和前面一样，当执行到 `cvx_end` 命令时问题即被求解，数值解赋给变量 `x`。顺带一提，CVX 不会通过给目标取平方来把这个问题转成 QP；相反，它会把它转成一个 SOCP（second-order cone program，二阶锥规划）。结果是一样的，而这个转化是自动完成的。

在这个例子里，和第一个例子一样，CVX 的 specification 比 MATLAB 的替代写法要长。但另一方面，CVX 版本更易读，也更容易与原问题对应起来。相比之下，`quadprog` 版本要求我们事先知道向 QP 形式的转化，包括 `2*A'*A` 和 `-2*A'*b` 这类计算。除了最简单的情形之外，求解同一问题时，CVX 的 specification 都要比等价的 MATLAB 代码更简单、更可读、也更紧凑。

---

### 2.3 其他范数与函数（Other norms and functions）

现在来看看最小二乘问题的一些替代形式。涉及 $\ell_{\infty}$ 或 $\ell_1$ 范数的范数最小化问题，可以改写为 LP（linear program，线性规划），并用线性规划求解器（linear programming solver）求解，例如 MATLAB Optimization Toolbox 中的 `linprog`（参见，如 [BV04, §6.1]）。不过，由于这些范数属于 CVX 的基础函数库（base library of functions），CVX 可以直接处理这些问题。

例如，要找出使切比雪夫范数（Chebyshev norm）$\|Ax-b\|_{\infty}$ 最小的 $x$，我们可以调用 MATLAB Optimization Toolbox 的 `linprog` 命令：

```matlab
f    = [ zeros(n,1); 1 ];
Ane  = [ +A,         -ones(m,1) ; ...
         -A,         -ones(m,1) ];
bne  = [ +b;         -b ];
xt   = linprog(f,Ane,bne);
x_cheb = xt(1:n,:);
```

用 CVX，同一个问题这样写：

```matlab
cvx_begin
    variable x(n);
    minimize( norm(A*x-b,Inf) );
cvx_end
```

基于 `linprog` 的代码和上面的 CVX specification 都能解这个切比雪夫范数最小化问题，也就是说，两者都会产生一个使 $\|Ax-b\|_{\infty}$ 最小的 $x$。不过，切比雪夫范数最小化问题可能有多个最优点（optimal points），所以两种方法得到的具体的 $x$ 可能不同；但这两个点对应的 $\|Ax-b\|_{\infty}$ 的值必须相同。

类似地，要最小化 $\ell_1$ 范数 $\|\cdot\|_1$，我们可以这样用 `linprog`：

```matlab
f    = [ zeros(n,1); ones(m,1); ones(m,1) ];
Aeq  = [ A,           eye(m),    +eye(m) ];
lb   = [ -Inf(n,1);   zeros(m,1); zeros(m,1) ];
xzz  = linprog(f,[],[],Aeq,b,lb,[]);
x_l1 = xzz(1:n,:);
```

CVX 版本则毫不意外地是：

```matlab
cvx_begin
    variable x(n);
    minimize( norm(A*x-b,1) );
cvx_end
```

CVX 会自动把这两个问题都转成 LP，跟手工为 `linprog` 构造的形式差不多。

如果我们考虑比 $\ell_{\infty}$ 和 $\ell_1$ 范数更不为人知的函数（以及它们所对应的转化），这种自动转化的优势就更加明显了。例如，考虑范数

```math
\|Ax - b\|_{\mathrm{lgst},k} = |Ax - b|_{[1]} + \cdots + |Ax - b|_{[k]},
```

其中 $|Ax-b|_{[i]}$ 表示 $Ax-b$ 各分量的绝对值中第 $i$ 大的那个元素。这确实是一个范数，尽管相当冷门。（当 $k=1$ 时它退化为 $\ell_{\infty}$ 范数；当 $k=m$（即 $Ax-b$ 的维数）时，它退化为 $\ell_1$ 范数。）在 $x$ 上最小化 $\|Ax-b\|_{\mathrm{lgst},k}$ 的问题可以写成 LP，但其转化绝非显而易见，这里就省略了。不过这个范数在 CVX 的基础库中已经提供，名为 `norm_largest`，所以用 CVX 来写出并求解这个问题很容易：

```matlab
k = 5;
cvx_begin
    variable x(n);
    minimize( norm_largest(A*x-b,k) );
cvx_end
```

---

## 本段关键概念速查

| 概念 | 英文原名 | 中文 | 一句定义 |
|------|----------|------|----------|
| Disciplined convex programming | DCP | 规则化凸规划 | 一种只允许用可判定的规则组合凸/凹函数来构造优化问题的方法论，以保证问题能被高效求解。 |
| Optimization variable | variable | 优化变量 | 在建模中被声明、由求解器确定其最优数值的未知量。 |
| Objective function | objective | 目标函数 | 需要被最小化或最大化的那个（标量）表达式。 |
| Constraint | constraint | 约束 | 对变量取值范围施加的等式或不等式条件。 |
| Solver | solver | 求解器 | 实际执行数值优化算法、求出最优解的程序。 |
| Quadratic program | QP | 二次规划 | 目标为二次函数、约束为线性的优化问题。 |
| Second-order cone program | SOCP | 二阶锥规划 | 含二阶锥约束的凸优化问题，是 QP 的推广。 |
| Linear program | LP | 线性规划 | 目标与约束均为线性的优化问题。 |
| Chebyshev norm | $\ell_{\infty}$-norm | 切比雪夫范数 | 向量各分量绝对值中的最大值。 |
| $\ell_1$ norm | $\ell_1$-norm | $\ell_1$ 范数 | 向量各分量绝对值之和。 |
| Backslash operator | `\` | 反斜杠运算符 | MATLAB 中 `A\b` 求解线性方程组/最小二乘的运算符。 |

---

## 2.3 续：norm_largest 与 Huber 惩罚函数

与 $\ell_1$、$\ell_2$ 或 $\ell_\infty$ 范数不同，这个范数（`norm_largest`）并不是 Matlab 标准发行版的一部分。不过，一旦你安装了 cvx，该范数就可以在 cvx 规格（cvx specification）之外，作为普通的 Matlab 函数来使用。例如，当上面的代码处理完之后，`x` 已经是一个数值向量了，于是我们可以输入：

```matlab
cvx_optval
norm_largest(A*x-b,k)
```

第一行显示由 cvx 确定的最优值；第二行则由 cvx 确定的最优向量 `x` 重新计算同一个值。

cvx 中受支持的非线性函数远不止 `norm` 和 `norm_largest`。例如，考虑 Huber 惩罚（Huber penalty）最小化问题

```math
\text{minimize} \quad \sum_{i=1}^{m} \varphi\big((Ax-b)_i\big)
```

其中变量 $x \in \mathbb{R}^n$，$\varphi$ 是 Huber 惩罚函数

```math
\varphi(z) =
\begin{cases}
|z|^2 & |z| \le 1 \\
2|z| - 1 & |z| \ge 1
\end{cases}
```

Huber 惩罚函数是凸的，并且已经内置在 cvx 的函数库中。所以在 cvx 中求解 Huber 惩罚最小化问题很简单：

```matlab
204  cvx_begin
205      variable x(n);
206      minimize( sum(huber(A*x-b)) );
207  cvx_end
```

cvx 会自动把这个问题转化成 SOCP（second-order cone program，二阶锥规划），再由核心 solver（求解器）来求解。（不过 cvx 用户并不需要知道这个转化是如何进行的。）

## 2.4 其他约束（Other constraints）

到目前为止，你大概不会感到意外：给 §2.3 中的那些问题加上简单界约束 $l \preceq x \preceq u$，做法简单到只需在每个 cvx 规格的 `cvx_end` 语句之前插入下面两行：

```matlab
x >= l;
x <= u;
```

事实上，cvx 还支持更复杂的约束。

例如，先在 Matlab 中定义新的矩阵 `C` 和向量 `d`：

```matlab
227  p = 4;
228  C = randn(p,n);
229  d = randn(p,1);
```

现在，给最初的最小二乘问题加上一个等式约束和一个非线性不等式约束：

```matlab
232  cvx_begin
233      variable x(n);
234      minimize( norm(A*x-b) );
235      subject to
236          C*x == d;
237          norm(x,Inf) <= 1;
238  cvx_end
```

这两个新增的约束都符合 DCP（disciplined convex programming，规范化凸规划）规则，因此都能被 cvx 接受。在 `cvx_end` 命令之后，cvx 会把这个问题转化为一个 SOCP 并求解。

当比较运算符（`==`、`>=` 等）涉及 cvx 优化变量、或由 cvx 优化变量构成的表达式时，它们的行为与涉及普通数值时的行为截然不同。例如，因为 `x` 是一个已声明的变量，第 236 行的表达式 `C*x==d` 会在 cvx 规格中加入一条约束，而它本身**不返回任何值**。另一方面，在 cvx 规格之外，如果 `x` 具有某个合适的数值——比如就在 `cvx_end` 命令之后——同样的表达式则会返回一个由 1 和 0 组成的向量，对应每个等式成立与否的真假。[^2]

[^2]: 事实上，在上面那段代码的 `cvx_end` 命令之后，你很可能会发现返回的值大多（甚至全部）是 0。这是因为，与许多数值算法一样，解只被确定到某个非零数值容差以内。所以等式约束会被“接近”满足，但往往不是精确满足。

同理，在 cvx 规格之内，语句 `norm(x,Inf)<=1` 会向规格中加入一个非线性约束；而在规格之外，它则根据 `x` 的数值返回 1 或 0（具体来说，取决于其 $\ell_\infty$-范数是小于等于 1，还是大于 1）。

由于 cvx 是为支持凸优化而设计的，它必须能够验证问题是凸的。为此，cvx 采用了一套构造规则（construction rules），来约束约束表达式和目标表达式的构造方式。例如，cvx 要求等式约束的左右两边都必须是仿射（affine）的。因此，像这样的约束

```matlab
norm(x,Inf) == 1;
```

会导致如下错误：

```
??? Error using ==> cvx.eq
Disciplined convex programming error:
Both sides of an equality constraint must be affine.
```

> 中文解释：等式约束的两边都必须是仿射的（affine）。`norm(x,Inf)` 不是仿射表达式，所以报错。

形如 $f(x) \le g(x)$ 或 $g(x) \ge f(x)$ 的不等式约束，只有当 $f$ 能被验证为凸（convex）、$g$ 能被验证为凹（concave）时，才会被接受。因此，像这样的约束

```matlab
norm(x,Inf) >= 1;
```

会导致如下错误：

```
??? Error using ==> cvx.ge
Disciplined convex programming error:
The left-hand side of a ">=" inequality must be concave.
```

> 中文解释：`>=` 不等式的左边必须是凹函数（concave）。`norm(x,Inf)` 是凸函数而非凹函数，所以报错。

构造规则的具体细节将在下文 §4 中更详细地讨论。如果你了解凸分析（convex analysis）和凸优化的基本知识，这些规则是相当直观的。

## 2.5 最优权衡曲线（An optimal trade-off curve）

作为本节的最后一个例子，我们来展示如何把传统 Matlab 代码和 cvx 规格混合起来，构建并求解多个优化问题。下面的代码求解了最小化 $\|Ax-b\|_2 + \gamma\|x\|_1$ 的问题，其中 $\gamma$ 取一列按对数间隔分布的（正）值。这样我们就得到了 $\|Ax-b\|_2$ 与 $\|x\|_1$ 之间最优权衡曲线上的点。这条曲线的一个例子见图 1。

```matlab
268  gamma = logspace( -2, 2, 20 );
269  l2norm = zeros(size(gamma));
270  l1norm = zeros(size(gamma));
271  fprintf( 1, '   gamma     norm(x,1)    norm(A*x-b)\n' );
272  fprintf( 1, '---------------------------------------\n' );
273  for k = 1:length(gamma),
274      fprintf( 1, '%8.4e', gamma(k) );
275      cvx_begin
276          variable x(n);
277          minimize( norm(A*x-b)+gamma(k)*norm(x,1) );
278      cvx_end
279      l1norm(k) = norm(x,1);
280      l2norm(k) = norm(A*x-b);
281      fprintf( 1, '  %8.4e  %8.4e\n', l1norm(k), l2norm(k) );
282  end
283  plot( l1norm, l2norm );
284  xlabel( 'norm(x,1)' );
285  ylabel( 'norm(A*x-b)' );
286  grid
```

这段代码的第 277 行展示了将在下文 §4 中讨论的一条构造规则。凸分析的一个基本原理是：一个凸函数可以乘以一个非负标量，也可以与另一个凸函数相加，其结果仍然是凸的。cvx 能够识别这类组合，并允许它们在任意可以用简单凸函数的地方使用——比如作为要最小化的目标函数，或者放在不等式约束的适当一侧。因此，在我们的例子中，表达式

```matlab
norm(A*x-b)+gamma(k)*norm(x,1)
```

在第 277 行上会被 cvx 识别为凸的，只要 `gamma(k)` 为正或为零。如果 `gamma(k)` 为负，那么这个表达式就变成了一个凸项与一个凹项的和，cvx 会因此产生如下错误：

```
??? Error using ==> cvx.plus
Disciplined convex programming error:
Addition of convex and concave terms is forbidden.
```

> 中文解释：禁止将凸项（convex）与凹项（concave）相加。当 `gamma(k)<0` 时，`gamma(k)*norm(x,1)` 就变成了凹项，与凸项 `norm(A*x-b)` 相加违反规则。

**图 1（Figure 1）**：quickstart 演示中第 268–286 行生成的权衡曲线示例。横轴为 `norm(x,1)`，纵轴为 `norm(A*x-b)`。

## 3 基础（The basics）

### 3.1 变量的数据类型（Data types for variables）

如上所述，所有变量都必须先用 `variable` 命令（或 `variables` 命令；见下文）声明，然后才能在约束或目标函数中使用。

变量可以是实数的或复数的；可以是标量、向量、矩阵，或 n 维数组。此外，矩阵还可以带有结构（structure），例如对称性（symmetry）或带状性（bandedness）。变量的结构是在变量名和尺寸之后，通过提供一组描述性关键字来指定的。例如，下面这段代码

```matlab
variable w(50) complex;
variable X(20,10);
variable Y(50,50) symmetric;
variable Z(100,100) hermitian toeplitz;
```

（位于一个 cvx 规格内部）声明：`w` 是一个 50 元素的复向量变量；`X` 是一个实数 $20\times10$ 矩阵变量；`Y` 是一个实数 $50\times50$ 对称矩阵变量；`Z` 是一个复数 $100\times100$ Hermitian（埃尔米特）Toeplitz 矩阵变量。结构关键字同样可以应用于 n 维数组：数组的每一个二维“切片”都会获得所指定的结构。当前支持的结构关键字如下：

| 关键字 | 含义 |
|---|---|
| `banded(lb,ub)` | 带状矩阵（下带宽 lb、上带宽 ub） |
| `complex` | 复数 |
| `diagonal` | 对角 |
| `hankel` | Hankel 矩阵 |
| `hermitian` | Hermitian（埃尔米特）矩阵 |
| `lower_bidiagonal` | 下双对角 |
| `lower_hessenberg` | 下 Hessenberg 矩阵 |
| `lower_triangular` | 下三角 |
| `scaled_identity` | 单位矩阵的（变量）倍 |
| `skew_symmetric` | 反对称 |
| `symmetric` | 对称 |
| `toeplitz` | Toeplitz 矩阵 |
| `tridiagonal` | 三对角 |
| `upper_bidiagonal` | 上双对角 |
| `upper_hankel` | 上 Hankel 矩阵 |
| `upper_hessenberg` | 上 Hessenberg 矩阵 |
| `upper_triangular` | 上三角 |

除少数几个例外，这些结构关键字都是自解释的：

- **`banded(lb,ub)`**：矩阵是带状的，下带宽为 `lb`，上带宽为 `ub`。如果 `lb` 和 `ub` 都为零，则得到对角矩阵。`ub` 可以省略，此时它被设为与 `lb` 相等。例如，`banded(1,1)`（或 `banded(1)`）就是三对角矩阵。
- **`scaled_identity`**：矩阵是单位矩阵的（变量）倍数。这等价于把它声明为对角且 Toeplitz。
- **`upper_hankel`**：矩阵是 Hankel 矩阵（即沿副对角线为常数），且在中央副对角线以下为零，即当 $i + j > n + 1$ 时元素为零。

当提供多个关键字时，最终的矩阵结构由它们的交集决定；如果关键字相互冲突，则会产生错误。

一条 `variable` 语句只能声明单个变量，当你需要声明大量变量时，这可能会有点不便。为此提供了 `variables` 语句，允许你一次声明多个变量；例如：

```matlab
variables x1 x2 x3 y1(10) y2(10,10,10);
```

`variables` 命令的一个局限是，它不能声明复数或结构化数组（例如 `symmetric` 等）。这些必须使用单数形式的 `variable` 命令，一次声明一个。

---

## 3.2 Objective functions（目标函数）

声明一个目标函数（objective function）需要使用 `minimize` 或 `maximize` 函数，视情况而定。在调用 `minimize` 时，其目标函数必须是凸的（convex）；在调用 `maximize` 时，其目标函数必须是凹的（concave）。在给定的 CVX 规约（specification，即 `cvx_begin` 与 `cvx_end` 之间的一段建模代码）中，最多只能声明一个目标函数，且该目标函数必须具有标量（scalar）值。（关于此规则的唯一例外，请参见第 §5 节关于定义新函数的部分。）

如果没有指定目标函数，则该问题被解释为一个可行性问题（feasibility problem），这等价于把目标函数设为零再做一次最小化。在这种情况下，`cvx_optval`（CVX 记录最优目标值的变量）的取值要么是 0（找到一个可行点时），要么是 `+Inf`（约束不可行时）。

---

## 3.3 Constraints（约束）

CVX 支持以下约束类型：

- 等式 `==` 约束（equality constraints），其左右两边都是优化变量的仿射（affine）函数。
- 小于号 `<=`、`<` 不等式约束（inequality constraints），其左边表达式是凸的，右边表达式是凹的。
- 大于号 `>=`、`>` 约束，其左边表达式是凹的，右边表达式是凸的。

在 CVX 中，严格不等式 `<` 和 `>` 会被接受，但被解释为对应的非严格不等式 `<=` 和 `>=`。我们鼓励你使用非严格形式 `<=` 和 `>=`，因为它们在数学上是正确的。（CVX 的未来版本可能会给严格不等式赋予略有不同的含义。）

这些等式和不等式运算符可用于数组（array）。当约束两边都是大小相同的数组时，约束按逐元素（elementwise）方式施加。例如，若 `a` 和 `b` 是 m×n 矩阵，那么 `a<=b` 会被 CVX 解释为 mn 个（标量）不等式，即 `a` 的每个元素都必须小于或等于 `b` 中对应位置的元素。CVX 也能处理一边是标量、另一边是数组的情形：这被解释为对数组的每个元素施加一个约束，其中（同一个）标量出现在另一边。例如，若 `a` 是 m×n 矩阵，那么 `a>=0` 被解释为 mn 个不等式：矩阵的每个元素都必须非负。

还要注意 `=` 与 `==` 之间的重要区别：`=` 是赋值（assignment），而 `==` 是在 CVX 规约内部施加等式约束；关于这一区别的更多内容，请参见第 §8.4 节。此外还要注意，不等运算符 `~=`（not-equal，不等于）不能在约束中使用；无论如何，这类约束很少是凸的。当任一边是复数（complex）时，不等式不能使用。

CVX 还支持集合成员（set membership）约束；请参见第 §3.5 节。

---

## 3.4 Functions（函数）

CVX 的基础函数库（base library）包含各种凸函数、凹函数和仿射函数，它们接受 CVX 变量或表达式（expression）作为参数。其中许多是常见的 MATLAB 函数，如 `sum`、`trace`、`diag`、`sqrt`、`max` 和 `min`，为了支持 CVX 而按需重新实现；另一些则是 MATLAB 中没有的新函数。基础库中函数的完整列表见第 §B 节。你也可以添加自己的新函数，参见第 §5 节。

基础库中的一个函数示例是 `quad_over_lin`，它表示二次比线性（quadratic-over-linear）函数，定义为 $f(x,y)=x^T x / y$，其定义域为 $\mathbb{R}^n \times \mathbb{R}_{++}$，即 $x$ 是 $\mathbb{R}^n$ 中的任意向量，$y$ 是正标量。（该函数也接受复的 $x$，但为简单起见我们只考虑实的 $x$。）二次比线性函数关于 $x$ 和 $y$ 是凸的，因此可以用作目标函数、放在合适的约束中，或用于更复杂的表达式。例如，我们可以用 CVX 规约内的如下语句来最小化 $(Ax-b,\, c^T x + d)$ 的二次比线性函数：

```matlab
minimize( quad_over_lin( A*x-b, c'*x+d ) );
```

这里假设 `x` 是向量优化变量，`A` 是矩阵，`b` 和 `c` 是向量，`d` 是标量。CVX 将这个目标表达式识别为凸函数，因为它是凸函数（二次比线性函数）与仿射函数的复合（composition）。

你也可以在 CVX 规约之外使用 `quad_over_lin` 函数。在这种情况下，给定（数值）参数后，它只是计算其（数值）值。不过，它与表达式 `((A*x-b)'*(A*x-b))/(c'*x+d)` 并不完全相同。当 $c^T x + d$ 为负时，后一个表达式仍然有意义并返回一个实数；但当 $c^T x + d \not> 0$ 时，`quad_over_lin(A*x-b,c'*x+d)` 返回 `+Inf`。

---

## 3.5 Sets（集合）

CVX 支持凸集合（convex set）的定义与使用。基础库包括 n×n 正半定矩阵锥（cone）、二阶锥或洛伦兹锥（second-order / Lorentz cone），以及各种范数球（norm ball）。基础库提供的集合完整列表见第 §B 节。

遗憾的是，MATLAB 语言没有集合成员运算符——比如用 `x in S` 来表示 $x \in S$。所以在 CVX 中，我们使用略有不同的语法来要求某个表达式属于某个集合。我们用“一个返回无名变量（unnamed variable）、并要求该变量属于该集合的函数”来表示一个集合。例如，考虑 $\mathcal{S}^n_{+}$，即 n×n 对称正半定矩阵锥。在 CVX 中，我们用函数 `semidefinite(n)` 来表示它，该函数返回一个新的无名变量，并约束它为正半定的（positive semidefinite）。若要求矩阵表达式 `X` 对称正半定，我们使用语法 `X == semidefinite(n)`。它的字面含义是：约束 `X` 等于某个无名变量，而该无名变量被要求是一个 n×n 对称正半定矩阵。当然，这等价于说 `X` 必须是对称正半定的。

作为一个例子，考虑约束：一个（矩阵）变量 `X` 是一个相关矩阵（correlation matrix），即它是对称的、对角线元素全为 1、且正半定。在 CVX 中，我们可以这样声明变量并施加这些约束：

```matlab
variable X(n,n) symmetric;
X == semidefinite(n);
diag(X) == ones(n,1);
```

这里第二行施加了“`X` 正半定”的约束。（你可以把这里的 `==` 读作“是（is）”，于是第二行可读作“`X` 是正半定的”。）第三行的左边是一个向量，它包含 `X` 的对角线元素，我们要求这些元素都等于 1。顺便说明，CVX 允许我们把第三行简化为：

```matlab
diag(X) == 1;
```

因为 CVX 遵循 MATLAB 处理数组/标量比较的惯例，即把数组的每个元素分别与标量比较。

集合可以组合在仿射表达式中，我们可以约束一个仿射表达式属于某个凸集合。例如，我们可以施加如下形式的约束：

```matlab
A*X*A'-X == B*semidefinite(n)*B';
```

其中 `X` 是一个 n×n 对称变量矩阵，`A` 和 `B` 是 n×n 常数矩阵。该约束要求 $AXA^T - X = BYB^T$ 对某个 $Y \in \mathcal{S}^n_{+}$ 成立。

CVX 也支持元素为“有序列表（ordered list）的若干量”的集合。例如，考虑二阶锥或洛伦兹锥：

```math
\mathcal{Q}^m = \{\, (x,y) \in \mathbb{R}^m \times \mathbb{R} \;|\; \|x\|_2 \le y \,\} = \mathrm{epi}\,\|\cdot\|_2,
\tag{2}
```

其中 `epi` 表示一个函数的上图（epigraph）。$\mathcal{Q}^m$ 的一个元素是一个有序列表，含有两个元素：第一个是 m 维向量，第二个是标量。我们可以用这个锥来表达第 §2.1 节中的简单最小二乘问题（用一种相当复杂的方式），如下所示：

```math
\begin{aligned}
& \text{minimize} && y \\
& \text{subject to} && (Ax - b, y) \in \mathcal{Q}^m .
\end{aligned}
\tag{3}
```

CVX 使用 MATLAB 的元胞数组（cell array）机制来模拟这种记法：

```matlab
cvx_begin
variables x(n) y;
minimize( y );
subject to
{ A*x-b, y } == lorentz(m);
cvx_end
```

函数调用 `lorentz(m)` 返回一个无名变量（即由“一个向量和一个标量变量”组成的一对），并被约束为落在长度为 m 的洛伦兹锥中。因此，这段规约中的约束要求：对 `{ A*x-b, y }` 位于大小合适的洛伦兹锥中。

---

## 3.6 Dual variables（对偶变量）

当一个规范化凸规划（disciplined convex program，DCP）被求解时，其关联的对偶问题（dual problem）也会被一并求解。（在这种语境下，原问题被称为原始问题（primal problem）。）最优对偶变量（dual variable）中的每一个都与原问题中的一个约束相关联，它们提供了关于原问题的宝贵信息，例如对约束做扰动时的灵敏度（sensitivity）[BV04, Ch.5]。要在 CVX 中获取最优对偶变量，你只需声明它们，并把它们与相应的约束关联起来。例如，考虑如下的 LP（线性规划，linear program）：

```math
\begin{aligned}
& \text{minimize} && c^T x \\
& \text{subject to} && Ax \preceq b ,
\end{aligned}
```

其中变量 $x \in \mathbb{R}^n$，共有 m 个不等式约束。该问题的对偶是：

```math
\begin{aligned}
& \text{maximize} && -b^T y \\
& \text{subject to} && c + A^T y = 0 \\
& && y \succeq 0 ,
\end{aligned}
```

其中对偶变量 $y$ 与原 LP 中的不等式约束 $Ax \preceq b$ 相关联。要在 CVX 中表示原始问题及这个对偶变量，我们使用如下语法：

```matlab
n = size(A,2);
cvx_begin
variable x(n);
dual variable y;
minimize( c' * x );
subject to
y : A * x <= b;
cvx_end
```

其中这行：

```matlab
dual variable y
```

告诉 CVX：`y` 将表示对偶变量；而这行：

```matlab
y : A * x <= b;
```

把它与不等式约束关联起来。注意，冒号 `:` 运算符在这里的用法不同于标准 MATLAB 中构造数值序列（如 `1:10`）的用法。这种新行为仅在存在对偶变量时生效，因此不会产生混淆或冲突。没有为 `y` 指定任何维度；它的维度由与之关联的约束自动确定。例如，若 `m = 20`，在 `cvx_end` 之前、紧邻的 MATLAB 命令行提示符处键入 `y`，会得到：

```
y =

cvx dual variable (20x1 vector)
```

（译文：`y` 是一个 CVX 对偶变量，为 20×1 的向量。）

---

## 第 22 页

> 原文说明：dual variable（对偶变量）不必放在约束的左侧。

将 dual variable（对偶变量，即在约束上声明、用于提取最优对偶解/影子价格的符号）放在约束的左侧并不是必需的；例如，上面那行也可以写成这样：

```matlab
A * x <= b : y;
```

此外，不等式约束的对偶变量永远是**非负**的，这意味着可以反转不等号的方向而不改变对偶变量的取值；也就是说：

```matlab
b >= A * x : y;
```

会得到完全相同的结果。另一方面，对于等式约束，交换等号左右两边会使该对偶变量的最优值**变号**（取相反数）。

在 `cvx_end` 语句处理完毕、并且假设优化成功后，cvx 会为 `x` 和 `y` 赋上数值——它们分别是**原变量（primal variable）**与**对偶变量（dual variable）**的最优值。对于这个 LP（线性规划，Linear Program），最优的原变量与对偶变量必须满足 complementary slackness conditions（互补松弛条件，即每条约束的松弛量与对应对偶变量之积为零）：

```math
y_i (b - Ax)_i = 0, \quad i = 1, \ldots, m \tag{4}
```

你可以在 Matlab 里用下面这行来检验：

```matlab
y .* (b-A*x)
```

它会打印出 `y` 与 `b-A*x` 逐元素相乘的积，结果应当几乎为零。这一行必须在 `cvx_end` 命令**之后**执行（因为 `cvx_end` 才会给 `x` 和 `y` 赋数值）；如果把它放在 cvx 规约（`cvx_begin` … `cvx_end` 之间的建模区）内部执行，就会报错，因为在那里 `y` 和 `b-A*x` 还只是抽象表达式（abstract expression，尚未赋数值的符号量）。

如果优化不成功——要么问题不可行（infeasible），要么无界（unbounded）——那么 `x` 和 `y` 会取不同的值。在**无界**的情形下，`x` 会包含一个无界方向（unbounded direction），也就是一个满足以下条件的点 `x`：

```math
c^T x = -1, \quad Ax \preceq 0 \tag{5}
```

而 `y` 会被填满 `NaN` 值，这反映了一个事实：对应的对偶问题不可行。

在**不可行**的情形下，`x` 被填满 `NaN` 值，而 `y` 则包含一个无界的对偶方向（unbounded dual direction），也就是满足以下条件的点 `y`：

```math
b^T y = -1, \quad A^T y = 0, \quad y \succeq 0 \tag{6}
```

当然，原问题与对偶问题的最优点和/或方向的精确含义取决于问题本身的结构。关于对偶信息的解释，可参考 [BV04] 等文献。

cvx 还支持声明**带索引的对偶变量（indexed dual variables）**。当模型中的约束数量（从而对偶变量的数量）本身依赖于参数时，这很有用。关于 indexed dual variables 的更多信息，见 §8.5。

---

## 第 23 页

### 3.7 表达式容器（Expression holders）

有时把某个 cvx 表达式存到一个 Matlab 变量里、供以后使用会很有用。例如，考虑下面这个 cvx 脚本：

```matlab
variables x y
z = 2 * x - y;
square( z ) <= 3;
quad_over_lin( x, z ) <= 1;
```

这里的构造 `z = 2 * x - y` **不是**一个等式约束，而是一个赋值（assignment）。它把中间计算结果 `2 * x - y`（这是一个仿射表达式，affine expression）存起来，然后在后面的两个不同约束里重复使用。我们称 `z` 为一个 **expression holder**（表达式容器，一个用来暂存表达式、供后续复用的 Matlab 变量），以区别于正式声明的 cvx 变量。关于**赋值与等式**之间的关键区别，详见 §8.4。

经常需要把一串表达式累积（accumulate）到一个 Matlab 变量里。遗憾的是，Matlab 对象模型的一个技术细节会在这种情况下带来麻烦。考虑下面这段构造：

```matlab
variable u(9);
x(1) = 1;
for k = 1 : 9,
x(k+1) = sqrt( x(k) + u(k) );
end
```

这看起来挺合理：`x` 应该是一个向量，第一个值是 `1`，后面的值都是凹的（concave）cvx 表达式。但如果你在 cvx 模型里这样写，Matlab 会给出一个相当难懂的报错：

```
??? The following error occurred converting from cvx to double:
Error using ==> double
Conversion to double from cvx is not possible.
```

> 报错中文解释：Matlab 试图把 cvx 对象转换成 double 数值时失败——即「无法将 cvx 对象转换为 double 类型」。

出现这个问题的原因是：当执行 `x(1)=1` 这个赋值时，Matlab 变量 `x` 被初始化成了一个**数值数组**（numeric array）；而 Matlab 不允许随后把 cvx 对象插入到数值数组中去。

解决办法是：在给 `x` 赋值之前，先把它**显式声明为表达式容器**。为此我们提供了关键字 `expression` 和 `expressions`，分别用于声明**单个**或**多个**表达式容器，供后续赋值使用。一旦声明了表达式容器，就可以自由地把数值和 cvx 表达式都插入进去。例如，上面那个例子可以改正为：

```matlab
variable u(9);
expression x(10);
x(1) = 1;
for k = 1 : 9,
x(k+1) = sqrt( x(k) + u(k) );
end
```

---

## 第 24 页

cvx 会无报错地接受这段构造。之后你就可以以任何合适的方式使用这些凹表达式 `x(1), …, x(10)`；例如，你可以对 `x(10)` 做 maximize（最大化）。

**变量对象（variable object）**与**表达式对象（expression object）**之间的区别相当重要。一个变量对象持有一个优化变量，并且在 cvx 规约中**不能被覆盖或赋值**。（不过在求解问题之后，cvx 会用最优值覆盖这些优化变量。）另一方面，一个表达式对象初始化为零，应当被理解为存放 cvx 表达式的**临时位置**；在 cvx 规约中它可以被赋值、自由地重新赋值、甚至被覆盖。

当然，正如第一个例子所示，并不总需要在创建或使用表达式容器之前就声明它。但这样做能给模型带来额外的一层清晰度，因此我们强烈建议这样做。

---

## 第 25 页

## 4 DCP 规则集（The DCP ruleset）

cvx 强制遵循由 **disciplined convex programming**（规范凸规划，缩写 **DCP**，一种通过限制表达式构造方式、从而可机械判定凸性的一类凸优化建模规则）规则集——简称 DCP ruleset（DCP 规则集）——所规定的约定。每当遇到违反任何一条规则的情况，cvx 都会发出报错信息，因此在开始构建模型之前理解这些规则非常重要。这些规则源自凸分析（convex analysis）的基本原理，一旦你接触过凸分析与凸优化，就很容易学会。

DCP 规则集是凸性的一个**充分但非必要**条件集合。因此，有可能构造出违反规则集、但实际上是凸的表达式。例如熵函数 $-\sum_{i=1}^{n} x_i \log x_i$（定义在 $x > 0$ 上）是凹的。如果把它写成：

```matlab
- sum( x .* log( x ) )
```

cvx 会拒绝它，因为它的凹性无法由任何一条 composition rules（复合规则）推出。（具体地说，它违反了 §4.4 中描述的 no-product rule——「禁止乘积」规则。）

不过，涉及熵的问题可以通过显式使用熵函数来求解：

```matlab
sum(entr( x ))
```

它在 cvx 的基础库中，因此被 cvx 识别为凹函数。如果某个凸（或凹）函数没有被 cvx 识别为凸（或凹），可以把它作为一个新的 **atom**（原子函数，即 DCP 规则集中被 cvx 直接识别其凸性/凹性/曲率的基础函数，如 `norm`、`sqrt`、`entr` 等）加入；见 §5。

再看另一个例子，函数 $\sqrt{x^2 + 1} = \lVert [x \; 1] \rVert_2$ 是凸的。如果写成：

```matlab
norm([x 1])
```

（假设 `x` 是标量变量或仿射表达式）cvx 会把它识别为凸表达式，因此可以用在（合适的）约束和目标函数中。但如果写成：

```matlab
sqrt(x^2+1)
```

cvx 会拒绝它，因为这个函数的凸性无法由 cvx 规则集推出。

### 4.1 曲率分类（A taxonomy of curvature）

在 disciplined convex programming 中，一个标量表达式按其 **curvature**（曲率，即表达式的凸性类别）进行分类。曲率有四类：**constant（常数）**、**affine（仿射）**、**convex（凸）**和 **concave（凹）**。对于一个定义在整个 $\mathbb{R}^n$ 上的函数 $f : \mathbb{R}^n \to \mathbb{R}$，这几类的含义如下：

- **constant（常数）**：

```math
f(\alpha x + (1 - \alpha) y) = f(x) \quad \forall x, y \in \mathbb{R}^n, \ \alpha \in \mathbb{R}
```

- **affine（仿射）**：

```math
f(\alpha x + (1 - \alpha) y) = \alpha f(x) + (1 - \alpha) f(y) \quad \forall x, y \in \mathbb{R}^n, \ \alpha \in \mathbb{R}
```

- **convex（凸）**：

```math
f(\alpha x + (1 - \alpha) y) \le \alpha f(x) + (1 - \alpha) f(y) \quad \forall x, y \in \mathbb{R}^n, \ \alpha \in [0, 1]
```

- **concave（凹）**：

```math
f(\alpha x + (1 - \alpha) y) \ge \alpha f(x) + (1 - \alpha) f(y) \quad \forall x, y \in \mathbb{R}^n, \ \alpha \in [0, 1]
```

---

## 第 26 页

当然，这些类别之间有显著的重叠。例如，常数表达式同时也是仿射的，而（实的）仿射表达式既是凸的又是凹的。

凸表达式和凹表达式按定义都是**实**的。可以构造复的常数表达式和仿射表达式，但它们的用途更受限；例如，它们不能作为不等式约束的左侧或右侧。

### 4.2 顶层规则（Top-level rules）

cvx 支持三种不同类型的 disciplined convex programs（规范凸规划问题）：

- **最小化问题（minimization problem）**：由一个凸的目标函数（convex objective function）加上零个或多个约束组成。
- **最大化问题（maximization problem）**：由一个凹的目标函数（concave objective function）加上零个或多个约束组成。
- **可行性问题（feasibility problem）**：由一个或多个约束组成。

### 4.3 约束（Constraints）

在 disciplined convex programs 中可以指定三种类型的约束：

- **等式约束（equality constraint）**：用 `==` 构造，两侧都必须是仿射（affine）的。
- **小于等于不等式约束**：用 `<=` 或 `<`，左侧必须凸（convex）、右侧必须凹（concave）。
- **大于等于不等式约束**：用 `>=` 或 `>`，左侧必须凹（concave）、右侧必须凸（convex）。

用 `~=` 构造的**不相等约束**永远不被允许。（这类约束不是凸的。）

等式约束的一侧或两侧可以是复的；而不等式约束则必须是实的。一个复等式约束等价于两个实等式约束——一个对应实部，一个对应虚部。一个一侧为实、另一侧为复的等式约束，其效果是把复那一侧的虚部约束为零。

正如上文 §3.5 所讨论的，cvx 通过等式约束来强制实现**集合成员约束（set membership constraints）**，例如 $x \in S$。「等式约束两侧都必须仿射」这条规则同样适用于集合成员约束。事实上，诸如 `semidefinite()` 和 `lorentz()` 这类 set atoms（集合类原子）的返回值是仿射的，所以只需验证集合成员约束的剩余部分即可。对于诸如 `{ x, y }` 这样的复合值，其每个元素都必须是仿射的。

在当前版本中，严格不等式 `<`、`>` 与非严格不等式 `>=`、`<=` 被**完全等价地**解释。最终 cvx 会标记（flag）严格不等式，以便在优化完成之后再对其进行验证。

---

## 4.4 Expression rules（表达式规则）

到目前为止，前面所述的这些规则约束力并不强——因为所有凸规划（无论是否遵循 disciplined convex programming，纪律化凸规划/DCP）通常都天然满足它们。真正把 disciplined convex programming（纪律化凸规划，简称 DCP，指一类必须用受限的、可验证为凸的表达式来构建模型的凸优化问题）与更一般的凸规划区分开来的，是那些管辖目标函数与约束中所用表达式如何构造的规则。

Disciplined convex programming 通过递归地应用下面这些规则，来确定标量表达式的 curvature（曲率/凹凸性，即表达式是凸、凹还是仿射这一性质）。这份清单看起来可能很长，但它在很大程度上只是凸分析中关于如何组合 convex（凸）、concave（凹）与 affine（仿射/affine）形式的一些基本规则的枚举：加法、数乘，等等。

- 一个合法的 constant expression（常量表达式）是
  – 任意一个良构的、能求值为有限值的 MATLAB 表达式。
- 一个合法的 affine expression（仿射表达式）是
  – 一个合法的常量表达式；
  – 一个已声明的变量；
  – 对 atom library（原子库/atom 库，即 CVX 内置的一批基础凸/凹/仿射函数的集合）中结果为仿射的函数的合法调用；
  – 若干仿射表达式的和或差；
  – 一个仿射表达式与一个常量的乘积。
- 一个合法的 convex expression（凸表达式）是
  – 一个合法的常量或仿射表达式；
  – 对原子库中结果为凸的函数的合法调用；
  – 一个仿射标量的常量幂 $p \ge 1$，且 $p \ne 3,5,7,9,\dots$（即 $p$ 不能是大于 1 的奇数）；
  – 一个凸的标量 quadratic form（二次型，见 §4.8）；
  – 两个或更多凸表达式之和；
  – 一个凸表达式与一个凹表达式之差；
  – 一个凸表达式与一个非负常量的乘积；
  – 一个凹表达式与一个非正常量的乘积；
  – 一个凹表达式的取负。
- 一个合法的 concave expression（凹表达式）是
  – 一个合法的常量或仿射表达式；
  – 对原子库中结果为凹的函数的合法调用；
  – 一个凹标量的幂 $p \in (0,1)$；
  – 一个凹的标量二次型（见 §4.8）；
  – 两个或更多凹表达式之和；

（第 27 页完，续下页）

---

  – 一个凹表达式与一个凸表达式之差；
  – 一个凹表达式与一个非负常量的乘积；
  – 一个凸表达式与一个非正常量的乘积；
  – 一个凸表达式的取负。

如果某个表达式无法被这套规则归类，CVX 就会拒绝它。对于矩阵和数组表达式，这些规则按元素逐项（elementwise）应用。我们指出，上面列出的这套规则是冗余的——存在更小、彼此等价的规则集合。

尤其值得注意的一点是：这些表达式规则**一般禁止非常量表达式之间的乘积**，唯一的例外是标量二次型（见下文 §4.8）。例如，表达式 `x*sqrt(x)` 碰巧是 $x$ 的一个凸函数，但其凸性无法用 CVX 的规则集验证，因此会被拒绝。（不过它可以改写成 `x^(3/2)` 或 `pow_p(x,3/2)`。）我们称之为 no-product rule（无乘积规则），对它多加留意，将大大有助于确保你所构造的表达式是合法的。

## 4.5 Functions（函数）

在 CVX 中，函数按两个属性分类：curvature（曲率/凹凸性：constant 常量、affine 仿射、convex 凸、或 concave 凹）与 monotonicity（单调性：nondecreasing 非递减、nonincreasing 非递增、或 nonmonotonic 非单调）。曲率决定了它们依据 §4.4 的表达式规则能够出现在表达式中的条件；单调性则决定了它们在函数复合（composition）中的使用方式，这一点我们将在下文 §4.6 中看到。

对于只有一个参数的函数，分类是直接的。下表给出一些例子。

| 函数 | 含义 | 曲率 | 单调性 |
| --- | --- | --- | --- |
| `sum(x)` | $\sum_i x_i$ | affine | nondecreasing |
| `abs(x)` | $|x|$ | convex | nonmonotonic |
| `sqrt(x)` | $\sqrt{x}$ | concave | nondecreasing |

遵循凸分析的标准做法，当参数落在函数的定义域之外时，凸函数被解释为 $+\infty$，凹函数被解释为 $-\infty$。换言之，CVX 中的凸函数和凹函数都被解释为它们的 extended-valued extension（扩值延拓，即在定义域外取值 $+\infty$ 或 $-\infty$ 的延拓形式）。

这样做的效果是：**自动**把函数的参数约束在其定义域之内。例如，如果我们在 CVX 规格（specification）中构造 `sqrt(x+1)`，其中 `x` 是一个变量，那么 `x` 会自动被约束为大于等于 $-1$。无需再额外添加一条 `x>=-1` 的约束来强制这一点。

函数的单调性是在扩值意义下（extended sense）确定的，也就是说，把参数在定义域之外的取值也一并考虑进来。例如，`sqrt(x)` 被判定为非递减，因为对于参数的负值，其值恒为 $-\infty$；随后在参数为零处跳升到 $0$，并随着参数取正值而递增。

（第 28 页完，续下页）

---

CVX 不会把一个仅在其定义域的某一部分上才是凸或凹的函数认定为凸或凹，即便参数被约束在这些部分之中。例如，考虑函数 $1/x$：当 $x>0$ 时它是凸的，当 $x<0$ 时它是凹的。但你永远无法在 CVX 中写下 `1/x`（除非 `x` 是常量），哪怕你已经施加了诸如 `x>=1` 这样的、把 `x` 限制在 $1/x$ 的凸区间的约束。你可以使用 CVX 函数 `inv_pos(x)`——它定义为当 $x>0$ 时为 $1/x$、否则为 $\infty$——来表示 $1/x$ 的凸部分；CVX 把这个函数识别为凸且非递增。在 CVX 中，你可以用 `-inv_pos(-x)` 来表达 $1/x$ 的凹部分（此时 `x` 为负），它会被正确地识别为凹且非递增。

对于有多个参数的函数，曲率总是联合（jointly）考察，但单调性可以逐参数（argument-by-argument）考察。例如，

$$
\text{quad\_over\_lin}(x,y) =
\begin{cases}
|x|^2 / y & y > 0 \\
+\infty & y \le 0
\end{cases}
\quad\text{（关于 $y$ 凸、非递增）}
$$

它在两个参数上联合为凸，但只在第二个参数上单调。

此外，有些函数只对其参数的某个子集才是凸、凹或仿射的。例如，函数

$$
\text{norm}(x,p) = \|x\|_p \quad (1 \le p)
\quad\text{（关于 $x$ 凸、非单调）}
$$

只在第一个参数上是凸的。于是，每当这个函数用于 CVX 规格中时，其余参数必须是常量，否则 CVX 会发出错误消息。

这类参数在数学术语中对应函数的参数（parameter）；例如：

$$
f_p(x) : \mathbb{R}^n \to \mathbb{R}, \qquad f_p(x) \triangleq \|x\|_p
$$

所以，在这个语境下我们也把它们称为参数（parameter）似乎很贴切。自此以后，每当我们说某个 CVX 函数是凸、凹或仿射时，我们都假定它的参数是已知的、并已被赋予了适当的常量值。

## 4.6 Compositions（复合）

凸分析的一条基本规则是：凸性在与仿射映射复合时保持封闭。这也是 DCP 规则集的一部分：

- 一个凸、凹或仿射函数可以接受一个（尺寸兼容的）仿射表达式作为参数，其结果分别为凸、凹或仿射。

例如，考虑 CVX 原子库中提供的函数 `square(x)`。该函数对其参数求平方，即计算 `x.*x`。（对于数组参数，它逐元素独立地平方。）它位于 CVX 原子库中，且在其参数为实数时被认定为凸。

于是，若 `x` 是一个 $n$ 维实变量，`a` 是一个常量 $n$ 维向量，`b` 是一个常量，则表达式

```matlab
square( a' * x + b )
```

（第 29 页完，续下页）

---

会被 CVX 接受，并且 CVX 知道它是凸的。

上面的仿射复合规则，是下面将要描述的、更精细的复合规则的一个特例。我们考虑一个已知曲率与单调性、且接受多个参数的函数。对凸函数而言，规则如下：

- 若该函数在某个参数上非递减，则该参数必须是凸的。
- 若该函数在某个参数上非递增，则该参数必须是凹的。
- 若该函数在某个参数上既非非递减也非非递增，则该参数必须是仿射的。

如果函数的每个参数都满足这些规则，那么该表达式就被 CVX 接受，并被归类为凸。回想一下，常量或仿射表达式既是凸的也是凹的，所以任何参数都可以是仿射的（常量作为其特例）。

凹函数对应的规则如下：

- 若该函数在某个参数上非递减，则该参数必须是凹的。
- 若该函数在某个参数上非递增，则该参数必须是凸的。
- 若该函数在某个参数上既非非递减也非非递增，则该参数必须是仿射的。

在此情形下，该表达式被 CVX 接受，并被归类为凹。

关于这些复合规则的更多背景，参见 [BV04, §3.2.4]。事实上，除了标量二次型表达式之外，整个 DCP 规则集都可以视为这六条规则的特例。

让我们来看几个例子。maximum 函数（即 `max`）是凸的，且在每一个参数上都非递减，因此它可以接受任何凸表达式作为参数。例如，若 `x` 是一个向量变量，那么

```matlab
max( abs( x ) )
```

满足六条复合规则中的第一条，因此被 CVX 接受，并被归类为凸。

再看另一个例子：`sum` 函数既是凸的又是凹的（因为它是仿射的），且在每一个参数上都非递减。因此表达式

```matlab
sum( square( x ) )
sum( sqrt( x ) )
```

在 CVX 中被认定为合法，并分别被归类为凸和凹。前者由凸函数的第一条规则推出，后者由凹函数的第一条规则推出。

大多数懂一点基础凸分析的人都喜欢用更具体的规则来理解这些例子：凸函数的最大值仍是凸的，凸（凹）函数之和仍是凸（凹）的。但这些规则不过是更一般的复合规则的特例。

（第 30 页完，本节续下页）

---

## 4.6 复合规则的一些具体情形（承接上一节）

上一节给出的是一般复合规则的几个特例（cases of the general composition rules）。另有一些众所周知、由一般复合规则推导出来的基本规则，例如：凸（凹）函数的非负倍数仍是凸（凹）函数；凸（凹）函数的非正倍数则变成凹（凸）函数。

下面我们深入剖析一个更复杂的例子。设 $x$ 是一个向量变量，$A$、$b$、$f$ 是维数恰当的常量。CVX 会把表达式

```matlab
sqrt(f'*x) + min(4,1.3-norm(A*x-b))
```

识别为 concave（凹的）。先看第一项 `sqrt(f'*x)`：CVX 知道 `sqrt` 是 concave，而 `f'*x` 是 affine（仿射的），所以推断出 `sqrt(f'*x)` 是 concave。再看第二项 `min(4,1.3-norm(A*x-b))`：CVX 知道 `min` 是 concave 且 nondecreasing（非减的），因此它可以接受凹的参数。而 `1.3-norm(A*x-b)` 是一个常量减去一个凸函数，所以是 concave。于是 CVX 断定第二项也是 concave。整个表达式是两项凹函数之和，故也被识别为 concave。

复合规则对于正确分类是**充分但非必要**的，因此有些实际上确实是凸或凹的表达式，会因不满足这些规则而被 CVX 拒绝。例如，若 $x$ 是向量变量，表达式

```matlab
sqrt( sum( square( x ) ) )
```

会被 CVX 拒绝，因为没有任何规则能够处理「一个凹且非减的函数」与「一个凸函数」的复合。当然，这个例子的绕行办法很简单：改用 `norm( x )`，因为 `norm` 在 atom（原子函数，指 CVX 内建、其 curvature 与 sign 已被登记的基础函数）库里，且 CVX 已知它是凸的。

---

## 4.7 非线性复合中的单调性

单调性（monotonicity）是非线性复合规则中至关重要的一个方面，它带来一些不那么显而易见的结果，我们通过例子来演示。考虑表达式

```matlab
square( square( x ) + 1 )
```

其中 $x$ 是标量变量。这个表达式实际上是凸的，因为 $(x^2+1)^2 = x^4+2x^2+1$ 是凸的。但 CVX 会拒绝它，因为外层 `square` 不能接受一个凸的参数。事实上，凸函数的平方一般而言未必是凸的：例如 $(x^2-1)^2 = x^4-2x^2+1$ 就不是凸的。

有几种办法可以把上面的表达式改写为符合规则集的形式。一种办法是写成 `x^4 + 2*x^2 + 1`，CVX 会把它识别为凸的，因为 CVX 允许使用 `^` 运算符表达正的偶次幂。（注意，同样的手法套用到函数 $(x^2-1)^2$ 上会失败，因为它的第二项是凹的。）

另一种办法是改用外层函数 `square_pos`，它包含在 CVX 库中，表示函数 $(x_+)^2$，其中 $x_+=\max\{0,x\}$。显然，当参数非负时，`square` 与 `square_pos` 结果一致。但

```matlab
square_pos( square( x ) + 1 )
```

这个表达式与上面被拒绝的版本在数学上等价（因为外层函数的参数恒为正），然而它满足 DCP 规则集，因此能被 CVX 接受。

这正是 CVX atom 库中若干函数存在两种形式的原因：一种是「自然」形式，另一种是经过改造、使其单调、从而可用于复合的形式。其他这类「单调延拓（monotonic extensions）」还包括 `sum_square_pos` 和 `quad_pos_over_lin`。如果你自己要实现一个新函数，不妨想一想这个函数是否也需要一个单调延拓版本。

---

## 4.8 标量二次型

在其最初形式（见 [Gra04, GBY06]）中，DCP 规则集甚至禁止使用像 `x * x` 这样的简单二次表达式（假设 $x$ 是标量变量）。出于实用考虑，我们选择对规则集开一个例外，以识别若干特定的二次型（quadratic forms），它们能够直接映射到 CVX atom 库中的某个凸二次函数（或其凹的负形式）：

- `conj( x ) .* x` 被替换为 `square( x )`
- `y' * y` 被替换为 `sum_square( y )`
- `(A*x-b)'*Q*(A*x-b)` 被替换为 `quad_form( A * x - b, Q )`

CVX 会检测上述左侧这类二次表达式，判断它们究竟是凸还是凹；若是，则把它们翻译为右侧等价的函数调用。

CVX 会逐一检查每个仿射表达式的单次乘积、每个仿射表达式的单次平方，判断其凸性；但它**不会**去检查，例如，仿射表达式乘积的和。举例来说，给定标量变量 $x$ 和 $y$，表达式

```matlab
x ^ 2 + 2 * x * y + y ^ 2
```

会在 CVX 中报错，因为三项中的第二项 `2 * x * y` 既非凸也非凹。但等价的表达式

```matlab
( x + y ) ^ 2
( x + y ) * ( x + y )
```

会被接受。实际上，CVX 在遇到标量二次型时会自动「配方（complete the square）」，所以二次型不要求对称。例如，若 $z$ 是向量变量，$a$、$b$ 是常量，$Q$ 是正定的，则

```matlab
( z + a )' * Q * ( z + b )
```

会被识别为凸的。二次型一旦被 CVX 验证通过，就可以像普通的凸或凹表达式一样任意使用，如 §4.4 所述。

在 disciplined convex programming（disciplined convex programming / DCP，即「规范化凸规划」，指只允许符合一套组合规则、从而保证凸性可判定的一类凸优化建模范式）中，二次型的使用频率实际上应当比传统数学规划框架中更低——因为在传统框架里，二次型往往是对人们真正想要的那个非光滑形式的一种光滑替代。而在 CVX 中，由于它支持非光滑函数，这种替代很少是必需的。例如，约束

```matlab
sum( ( A * x - b ) .^ 2 ) <= 1
```

可以等价地用欧几里得范数表示为：

```matlab
norm( A * x - b ) <= 1
```

在现代 solver（求解器）中，第二种形式可以用二阶锥约束（second-order cone constraint）来表示——所以第二种形式实际上可能更高效。因此我们鼓励你，鉴于规范化凸规划带来的这些新能力，重新审视模型中二次型的使用。

---

## 5 向 CVX atom 库添加新函数

CVX 允许用户定义新的凸函数与凹函数并加入 atom 库，有两种方式，本节将分别介绍。第一种方法很简单，很多 CVX 用户都可以（也应该）使用，因为它只需掌握基本的 DCP 规则集即可。第二种方法功能非常强大，但略复杂，应视为一种高级技巧，只适合那些真正熟悉凸分析（convex analysis）、规范化凸规划以及 CVX 当前状态的人来尝试。

如果你实现了某个你认为对其他用户有用的凸函数或凹函数，请务必告诉我们；我们很乐意在后续版本中将其纳入。

### 5.1 通过 DCP 规则集构造新函数

在 CVX 中构造一个可用新函数的最简单方式，就是用完全符合 DCP 规则集的表达式来构造它。为说明这一点，考虑凸的死区函数（convex deadzone function），定义为

$$
f(x) = \max\{|x|-1,\,0\} =
\begin{cases}
0 & |x| \le 1 \\
x-1 & x > 1 \\
-1-x & x < -1
\end{cases}
$$

要在 CVX 中实现这个函数，只需创建一个文件 `deadzone.m`，内容为：

```matlab
function y = deadzone( x )
y = max( abs( x ) - 1, 0 )
```

这个函数在 CVX 之外的行为正如你所预期——即当它的参数是数值时正常计算。但得益于 Matlab 的运算符重载（operator overloading）能力，当它以仿射参数被调用时，在 CVX 内部也能正常工作。CVX 会正确地推断出该函数是凸的，因为它所执行的每个运算都符合 DCP 规则：`abs` 被识别为凸函数；我们可以从它减去一个常量；再对结果与 0 取最大值，最终得到凸函数。因此，凡是能用 `abs` 的地方，我们都能自由地使用 `deadzone`，因为 CVX 知道它是凸函数。

需要强调的是，用这种方式定义函数时，你所用到的表达式必须符合 DCP 规则集，正如它们被直接插入 CVX 模型时一样。例如，如果把上面的 `max` 换成 `min`：

```matlab
function y = deadzone_bad( x )
y = min( abs( x ) - 1, 0 )
```

那么这个修改后的函数就不满足 DCP 规则集了。在 CVX 规范之外，该函数能正常运作，对一个数值参数 $x$ 愉快地算出 $\min\{|x|-1,\,0\}$ 的值。但在 CVX 规范内部、以非常量参数调用时，它就无法工作，因为它没有遵循 DCP 的复合规则。

---

## 5.2 通过部分指定问题（partially specified problems）定义新函数

在 CVX 中定义新函数的一种更高级方法，依赖于凸分析（convex analysis）中如下一条基本结论。设 $S \subset \mathbb{R}^n \times \mathbb{R}^m$ 是一个凸集（convex set），$g:(\mathbb{R}^n \times \mathbb{R}^m) \to (\mathbb{R} \cup +\infty)$ 是一个凸函数。那么

```math
f:\mathbb{R}^n \to (\mathbb{R}\cup +\infty),\qquad
f(x) \triangleq \inf\{\, g(x,y) \mid \exists y,\ (x,y)\in S \,\}
\tag{7}
```

同样是一个凸函数。（这条规则有时被称为**部分最小化规则** partial minimization rule。）我们可以把凸函数 $f$ 看作一族凸优化问题的**最优值（optimal value）**，这族问题以 $x$ 为指标（index）或参数（parametrize），形式为

```math
\begin{aligned}
& \text{minimize} & & g(x,y) \\
& \text{subject to} & & (x,y) \in S
\end{aligned}
```

其中优化变量是 $y$。

有一个特殊情况应当非常熟悉：如果 $m=1$ 且 $g(x,y)\triangleq y$，那么

```math
f(x) \triangleq \inf\{\, y \mid \exists y,\ (x,y)\in S \,\}
```

给出了 $f$ 经典的**上图表示（epigraph representation）**：

```math
\operatorname{epi} f = S + (\{0\}\times \mathbb{R}_+),
```

其中 $0\in\mathbb{R}^n$。

在 CVX 中，你可以用同样的方式定义一个凸函数——即把它定义为一族参数化 disciplined convex program（DCP，凸规划）的最优值。这种情况下，我们把作为基础的凸规划称为一个**不完全指定（incomplete specification）**——之所以这样命名，是因为在构造该指定时，参数（也就是函数的输入）还是未知的。不完全指定的概念乍看可能有点复杂，但它是让 CVX 得以支持众多种类函数的一个非常强大的机制。

我们来看一个例子，看看它是如何工作的。考虑**单位半宽 Huber 罚函数（unit-halfwidth Huber penalty function）** $h(x)$：

```math
h:\mathbb{R}\to\mathbb{R},\qquad
h(x)\triangleq
\begin{cases}
x^2, & |x|\le 1,\\[2pt]
2|x|-1, & |x|\ge 1.
\end{cases}
\tag{8}
```

我们可以把 Huber 函数表示为如下一族以 $x$ 为参数的凸二次规划（QP，quadratic program）：

```math
\begin{aligned}
& \text{minimize} & & 2v + w^2 \\
& \text{subject to} & & |x| \le v + w, \\
& & & w \le 1,
\end{aligned}
\tag{9}
```

其中 $v$ 和 $w$ 为标量变量。这个简单 QP 的最优值恰好等于 $x$ 的 Huber 罚函数值。我们注意到，这个 QP 中的目标函数和约束函数（联合地）关于 $v$、$w$ 和 $x$ 是凸的。

> **数学问题 ↔ CVX 代码对应说明**：目标函数 $2v+w^2$ 对应 `minimize( w^2 + 2 * v )`；约束 $|x|\le v+w$ 对应 `abs( x ) <= w + v`；约束 $w\le 1$ 对应 `w <= 1`。两个标量变量 $v$、$w$ 由 `variables w v;` 声明。

我们可以在 CVX 中如下实现 Huber 罚函数：

```matlab
function cvx_optval = huber( x )
cvx_begin
variables w v;
minimize( w^2 + 2 * v );
subject to
abs( x ) <= w + v;
w <= 1;
cvx_end
```

如果以数值 $x$ 调用 `huber`，那么当执行到 `cvx_end` 语句时，CVX 会找到一个**完全指定（complete specification）**，并求解该问题以计算结果。CVX 把最优目标函数值放入变量 `cvx_optval` 中，函数将这个值作为输出返回。当然，通过求解一个 QP 来计算数值 $x$ 的 Huber 函数值是非常低效的，但它确实能给出正确的值（精确到核心 solver 的精度）。

然而最重要的是：如果 `huber` 被用在一个 CVX 指定内部，且其参数是一个**仿射（affine）CVX 表达式（expression）**，那么 CVX 会正确处理。特别地，CVX 会把以仿射参数调用的 Huber 函数识别为一个合法的凸表达式（convex expression）。

在这种情况下，函数 `huber` 内部会包含一个特殊的 Matlab 对象，用来在约束和目标中表示这次函数调用。因此，函数 `huber` 可以用在任何传统凸函数能够使用的地方——约束或目标函数中，符合 DCP（disciplined convex programming，规范凸规划）规则集。

对于凹函数也有对应的展开。给定如上所述的凸集 $S$，以及一个凹函数 $g:(\mathbb{R}^n\times\mathbb{R}^m)\to(\mathbb{R}\cup-\infty)$，函数

```math
f:\mathbb{R}\to(\mathbb{R}\cup-\infty),\qquad
f(x)\triangleq \sup\{\, g(x,y) \mid \exists y,\ (x,y)\in S \,\}
\tag{10}
```

是凹的。如果 $g(x,y)\triangleq y$，那么

```math
f(x)\triangleq \sup\{\, y \mid \exists y,\ (x,y)\in S \,\}
\tag{11}
```

给出了 $f$ 的**下图表表示（hypograph representation）**：

```math
\operatorname{hypo} f = S - \mathbb{R}^n_+.
```

在 CVX 中，一个**凹的不完全指定（concave incomplete specification）**只不过是把 `minimize` 目标换成 `maximize` 目标的那一种；只要构造得当，它就可以用在 CVX 指定内部任何传统凹函数能够使用的地方。

作为一个凹不完全指定的例子，考虑函数

```math
f:\mathbb{R}^{n\times n}\to\mathbb{R},\qquad
f(X)=\lambda_{\min}(X+X^T)
\tag{12}
```

它的下图表可以用一个线性矩阵不等式（LMI，linear matrix inequality）来表示：

```math
\operatorname{hypo} f = \{\,(X,t)\mid f(X)\ge t\,\}
= \left\{\, (X,t) \ \middle|\ X+X^T - tI \succeq 0 \,\right\}
\tag{13}
```

> 注：$\lambda_{\min}$ 表示最小特征值（minimum eigenvalue），$I$ 为单位矩阵，$\succeq 0$ 表示矩阵半正定（positive semidefinite）。于是我们在 CVX 中可以这样实现这个函数：

```matlab
function cvx_optval = lambda_min_symm( X )
n = size( X, 1 );
cvx_begin
variable y;
maximize( y );
subject to
X + X' - y * eye( n ) == semidefinite( n );
cvx_end
```

> **数学问题 ↔ CVX 代码对应说明**：目标 $\sup y$（最大化 $y$）对应 `maximize( y )`；LMI 约束 $X+X^T-tI\succeq 0$ 对应 `X + X' - y * eye( n ) == semidefinite( n )`，其中 $t$ 即变量 `y`，$I$ 即 `eye(n)`，$\succeq 0$ 用 `== semidefinite(n)` 表示。

如果提供了数值 $X$，这个函数将返回 `min(eig(X+X'))`（在数值容差范围内）。不过，这个函数也可以用在 CVX 的约束和目标中，就像 atom 库中任何其他凹函数一样。

使用不完全指定定义函数时，会遇到两个实际问题，下面都用上面的 `huber` 例子来说明。首先，如所写的那样，这个函数只能处理标量值。要把它（逐元素地）应用于一个向量，就需要在 for 循环中遍历各个元素——这在 CVX 中是一种非常低效的做法。好得多的办法是扩展 `huber` 函数以处理向量输入。这其实相当简单：我们只需创建一个**多目标（multiobjective）**版本的问题：

```matlab
function cvx_optval = huber( x )
sx = size( x );
cvx_begin
variables w( sx ) v( sx );
minimize( w .^ 2 + 2 * v );
subject to
abs( x ) <= w + v;
w <= 1;
cvx_end
```

这个版本的 `huber` 实际上会并行地创建 `sx` 个问题"实例"；当它被用在 CVX 指定中时，会被正确处理。

第二个问题是：如果 `huber` 的输入是数值，那么直接计算远比求解一个 QP 高效得多。（更重要的是，多目标版本不能用于数值输入。）一种解决方案是把两个版本放在同一个文件中，用一个适当的判断来选择使用哪个版本：

```matlab
function cvx_optval = huber( x )
if isnumeric( x ),
xa = abs( x );
flag = xa < 1;
cvx_optval = flag .* xa.^2 + (~flag) * (2*xa-1);
else,
sx = size( x );
cvx_begin
variables w( sx ) v( sx );
minimize( w .^ 2 + 2 * v );
subject to
abs( x ) <= w + v;
w <= 1;
cvx_end
end
```

或者，你也可以创建函数的两个独立版本，一个用于数值输入，一个用于 CVX 表达式，并把数值版本放在名为 `@double` 的子目录里。Matlab 只会在其参数为数值时调用 `@double` 版本，在其他情况下则调用你的 CVX 版本。CVX atom 库中的 `huber` 版本采取的就是这种做法。

进一步学习使用不完全指定的一个好方法，是去研究 CVX atom 库中已有的一些例子。不错的选择包括 `huber`、`inv_pos`、`lambda_min`、`lambda_max`、`matrix_frac`、`quad_over_lin`、`sum_largest` 等等。其中有些因为含有诊断或错误检查代码而稍难读懂，但这些相对简单。

---

## 6 使用 CVX 做半定规划（Semidefinite Programming，SDP）

熟悉半定规划（SDP）的人都知道，上面 §3.5 中用到的集合 `semidefinite(n)` 所表达的约束，在实际中通常是用**线性矩阵不等式（LMI）**记号来书写的。例如，给定 $X = X^T \in \mathbb{R}^{n\times n}$，约束 $X \succeq 0$ 表示 $X \in \mathbb{S}^n_+$，也就是说 $X$ 是**半正定（positive semidefinite）**的。

CVX 提供了一种特殊的 SDP 模式，允许在 CVX 模型内部使用 Matlab 标准的不等式运算符 `>=`、`<=` 等来采用这种 LMI 约定。要使用它，只需在建模开始时把语句写成 `cvx_begin sdp` 或 `cvx_begin SDP`，而不是简单地写 `cvx_begin`。

当 SDP 模式开启时，CVX 会以不同的方式解释某些不等式约束。具体来说：

- **等式约束**按相同方式解释（即逐元素地，elementwise）。
- **涉及向量和标量的不等式约束**按相同方式解释，即逐元素地。
- **涉及非方阵（non-square matrices）的不等式约束**是不被允许的；尝试使用它们会导致错误。

  如果你希望对矩阵 $X$ 和 $Y$ 做真正的逐元素比较，请使用向量化操作 `X(:) <= Y(:)` 或 `vec( X ) <= vec( Y )`。（`vec` 是 CVX 提供的一个函数，等价于冒号操作。）

- **涉及实数方阵的不等式约束**按如下方式解释：

  ```
  X >= Y
  以及
  X > Y
  ```
  变成
  ```
  X - Y == semidefinite(n)
  ```

  ```
  X <= Y
  以及
  X < Y
  ```
  变成
  ```
  Y - X == semidefinite(n)
  ```

  如果任一侧是复数，那么这些不等式按如下方式解释：

  ```
  X >= Y
  以及
  X > Y
  ```
  变成
  ```
  X - Y == hermitian_semidefinite(n)
  ```

  ```
  X <= Y
  以及
  X < Y
  ```
  变成
  ```
  Y - X == hermitian_semidefinite(n)
  ```

  在上面的式子中，$n=\max(\text{size}(X,1),\,\text{size}(Y,1))$。

- 还有一条额外的限制：$X$ 和 $Y$ 必须大小相同，或者其中一个必须是标量零。例如，如果 $X$ 和 $Y$ 是大小为 $n$ 的矩阵：

  | 写法 | 合法性 |
  | --- | --- |
  | `X >= 1` 或 `1 >= Y` | illegal（非法） |
  | `X >= ones(n,n)` 或 `ones(n,n) >= Y` | legal（合法） |
  | `X >= 0` 或 `0 >= Y` | legal（合法） |

  实际上，对于 LMI 约束，CVX 对不等式运算符实施了更严格的解释。

---

## 第 40 页（续 §6 半定规划模式）

- 注意，LMI（linear matrix inequality，线性矩阵不等式）约束会对它的输入强制施加对称性（视情形为实对称或 Hermitian，即埃尔米特对称）。与 SDPSOL [WB00] 不同，CVX 不会替你抽取对称部分（symmetric part）：你必须自行确保对称性。由于 CVX 支持声明对称矩阵（symmetric matrix），做到这一点是相当直接的。如果 CVX 无法判定某条 LMI 是对称的，它会发出警告。

- 一个 dual variable（对偶变量），如果被提供，将被施加到转换后的等式约束上。若找到了最优点，它会被赋予一个半正定（positive semidefinite）的值。

  例如，文件 `examples/closest_toeplitz_sdp.m` 中出现的 CVX 模型：

```matlab
cvx_begin
variable Z(n,n) hermitian toeplitz
dual variable Q
minimize( norm( Z - P, 'fro' ) )
Z == hermitian_semidefinite( n ) : Q;
cvx_end
```

  也可以等价地写成下面这样（改用 SDP 模式）：

```matlab
cvx_begin sdp
variable Z(n,n) hermitian toeplitz
dual variable Q
minimize( norm( Z - P, 'fro' ) )
Z >= 0 : Q;
cvx_end
```

  **对应关系说明**：两段代码的目标完全相同——极小化 `Z` 与给定矩阵 `P` 之差的 Frobenius 范数（`norm( Z - P, 'fro' )`）。差别只在约束的写法：第一段用显式调用 `hermitian_semidefinite(n)` 来声明「Z 为半正定」，并用 `==` 建立等式；第二段直接写 `Z >= 0`（在 `cvx_begin sdp` 模式下，`>= 0` 会被解释为「半正定」而非逐元素非负）。冒号 `: Q` 把对偶变量 `Q` 关联到这条约束上。

CVX 示例库（example library）中的许多其他示例都用到了半定约束（semidefinite constraint），而且它们全部采用 SDP 模式。要找到它们，只需用你惯用的文件搜索工具，在 `examples/` 子目录树中搜索文本 `cvx_begin sdp` 即可。其中一例将在 §8.5 中复现。

既然半定规划（semidefinite programming，SDP）如此常用，或许有人会问：为什么 SDP 模式不是默认行为？原因是：我们极其重视在 MATLAB 原生行为与 CVX 行为之间保持一致，而用 `>=`、`<=`、`>`、`<` 运算符来构造 LMI 恰恰偏离了这一理想。

例如，上面示例中的表达式 `Z >= 0` 把变量 `Z` 约束为半正定。但在模型求解完成、`Z` 已被替换为数值之后，表达式 `Z >= 0` 将转而检验 `Z` 是否逐元素（elementwise）非负。要验证 `Z` 的数值确实半正定，你必须执行类似 `min(eig(Z)) >= 0` 这样的检验（即所有特征值均非负）。

---

## 第 41 页

## 7 使用 CVX 进行几何规划

几何规划（Geometric programs，GPs）是一类特殊的数学规划，可以通过变量代换（change of variables）转化为凸形式（convex form）。GP 的凸形式可以表达为 DCP（disciplined convex program，纪律凸规划）问题，但 CVX 还额外提供了一种特殊模式，允许以 GP 的「原生形式」（native form）直接写出几何规划。CVX 会自动完成必要的转换、算出数值解，并把结果翻译回原始问题。关于几何规划的入门教程，我们推荐读者参考 [BKVH05]。

要使用 GP 模式，你必须用命令 `cvx_begin gp` 或 `cvx_begin GP` 来开启 CVX 规格，而不是简单地写 `cvx_begin`。例如，下面这段代码（位于示例库 `gp/max_volume_box.m`）求解的是：在若干面积约束和比例约束下，一个体积最大的箱子：

```matlab
cvx_begin gp
variables w h d
maximize( w * h * d )
subject to
2*(h*w+h*d) <= Awall;
w*d <= Afloor;
h/w >= alpha;
h/w <= beta;
d/w >= gamma;
d/w <= delta;
cvx_end
```

  **对应关系说明**：这里 `w`、`h`、`d` 分别是箱子的宽、高、深三个变量。目标 `maximize( w * h * d )` 即最大化体积。`subject to` 之后的六条约束依次为：箱子四个侧面的总面积 `2*(h*w + h*d)` 不超过墙体面积 `Awall`（墙面积约束）；底面面积 `w*d` 不超过地面面积 `Afloor`（地面面积约束）；高宽比 `h/w` 介于 `alpha` 与 `beta` 之间（两条约束）；深宽比 `d/w` 介于 `gamma` 与 `delta` 之间（两条约束）。

正如该示例所示，CVX 支持利用加法、乘法、（在适当时的）除法以及幂来构造 monomial（单项式）和 posynomial（正项式，即各项系数为正的多项式）。此外，CVX 还支持构造广义几何规划（generalized geometric programs，GGPs）——通过在标准 GP 中凡是允许 posynomial 的位置改用 generalized posynomial（广义正项式）即可 [BKVH05]。

本版本 CVX 所使用的求解器并不原生支持几何规划，而是改用附录 D.1 中描述的逐次逼近（successive approximation）技术来求解。这意味着求解 GP 可能较慢，但对于中小规模问题，该方法效果良好。

在本节剩余部分，我们将描述在 GP 模式下构造模型时适用的具体规则。

### 7.1 顶层规则

CVX 支持三种类型的几何规划：

- **最小化问题（minimization）**：由一个 generalized posynomial（广义正项式）目标函数和零个或多个约束组成。
- **最大化问题（maximization）**：由一个 monomial（单项式）目标函数和零个或多个约束组成。

---

## 第 42 页

- **可行性问题（feasibility）**：由一个或多个约束组成。

最小化与最大化之间的这种不对称——具体而言，即最大化问题只允许 monomial 目标函数——是 GP 与 GGP 几何特性所导致的、无法避免的产物。

### 7.2 约束

几何规划中可以指定三种类型的约束：

- **等式约束（equality constraint）**：用 `==` 构造，等式两边都是 monomial（单项式）。
- **小于不等式约束**（`<=`、`<`）：左侧是 generalized posynomial（广义正项式），右侧是 monomial。
- **大于不等式约束**（`>=`、`>`）：左侧是 monomial，右侧是 generalized posynomial。

与 DCP 一样，非等式约束（即范围限制之外的约束形式）是不允许的。

### 7.3 表达式

广义几何规划的基本构件（building block）是 monomial（单项式）、posynomial（正项式）和 generalized posynomial（广义正项式）。一个合法的 monomial 是：

- 一个已声明的变量；
- 两个或多个 monomial 的乘积；
- 两个 monomial 的比值；
- 一个 monomial 的实数次幂；
- 以下列函数之一、以 monomial 为参数的调用：`prod`、`cumprod`、`geo_mean`、`sqrt`。

一个合法的 posynomial 表达式是：

- 一个合法的 monomial；
- 两个或多个 posynomial 的和；
- 两个或多个 posynomial 的乘积；
- 一个 posynomial 与一个 monomial 的比值；
- 一个 posynomial 的正整数次幂；
- 以下列函数之一、以 posynomial 为参数的调用：`sum`、`cumsum`、`mean`、`prod`、`cumprod`。

---

## 第 43 页

一个合法的 generalized posynomial 表达式是：

- 一个合法的 posynomial；
- 两个或多个 generalized posynomial 的和；
- 两个或多个 generalized posynomial 的乘积；
- 一个 generalized posynomial 与一个 monomial 的比值；
- 一个 generalized posynomial 的正实数次幂；
- 以下列函数之一、以 generalized posynomial 为参数的调用：`sum`、`cumsum`、`mean`、`prod`、`cumprod`、`geo_mean`、`sqrt`、`norm`、`sum_largest`、`norm_largest`。

在 CVX 中完全可以创建并操作 monomial、posynomial 和/或 generalized posynomial 的数组，此时上述规则以显而易见的方式推广。例如，两个 monomial 矩阵的乘积，依据这两个矩阵的结构，会产生一个 posynomial 矩阵或一个 monomial 矩阵。

---

## 第 44 页

## 8 高级主题

在本节中，我们将介绍 CVX 若干更高级的能力。我们建议你在熟练掌握上面所描述的基本能力之前，先跳过本节。

### 8.1 求解器选择

CVX 目前支持两个求解器：SeDuMi 和 SDPT3（默认求解器）。要把 SeDuMi 设为你的默认求解器，只需在 CVX 模型之外（即命令行处）键入：

```matlab
cvx_solver sedumi
```

要恢复为 SDPT3，键入：

```matlab
cvx_solver sdpt3
```

要查看当前选中的是哪个求解器，只需键入：

```matlab
cvx_solver
```

我们发现，对于用到二阶锥（second-order cones）的问题，SDPT3 要可靠得多；这类问题包括涉及绝对值（absolute value）、二次型（quadratics）、幂函数（power functions）和范数（norms）的问题。SDPT3 目前正处于非常活跃的开发之中；因此，如果你遇到一个 SDPT3 无法求解、但 SeDuMi 可以求解的问题，请给我们发送 bug 报告，我们会把结果转发给 SDPT3 的作者。

### 8.2 控制求解精度

凸优化的数值方法并非精确方法；它们会把结果计算到某个预定义的数值精度（numerical precision）或容差（tolerance）以内。CVX 默认选择的精度（它继承自其求解器 SeDuMi 的默认值）对大多数应用来说应当是完全可以接受的。不过，在某些应用中你可能希望收紧或放宽该精度。

调用 `cvx_precision` 命令有几种方式。如果你不带任何参数调用它，它会简单地返回一个由两个元素组成的向量，表示当前的精度设置。该向量中的第一个元素是标准精度（standard precision），即求解器必须达到才能返回 `Solved`、`Unbounded` 或 `Infeasible` 的精度；向量中的第二个元素是「降低精度」（reduced precision），即求解器必须达到才能返回 `Solved/Inaccurate`、`Unbounded/Inaccurate`、`Infeasible/Inaccurate` 的精度。

带参数调用 `cvx_precision` 可以让你真正改变精度级别。一种方式是以字符串形式提供参数（无论命令模式还是函数模式均可），该字符串从以下五个值中选择其一：

- `cvx_precision low`：标准精度 = 降低精度 = $ϵ^{1/4} \approx 1.2 \times 10^{-4}$。
- `cvx_precision medium`：标准精度 = $ϵ^{3/8} \approx 1.3 \times 10^{-6}$，降低精度 = $ϵ^{1/4}$。

---

## 8.2 精度控制（cvx_precision，续）

- `cvx_precision` default（默认）时：standard（标准）精度 = $\epsilon^{1/2}\approx 1.5\times10^{-8}$，reduced（降档）精度 = $\epsilon^{1/4}$。
- `cvx_precision` high（高）时：standard = $\epsilon^{3/4}\approx 1.1\times10^{-11}$，reduced = $\epsilon^{3/8}$。
- `cvx_precision` best（最高）时：standard = $0$，"reduced" = $\epsilon^{1/2}$（见下文）。

在 function mode（函数调用模式）下，这些调用写作 `cvx_precision('low')` 等。其中 best 这个精度设置很特殊：它指示 solver 一直迭代，直到完全无法再取得任何进展为止。此后，只要它至少达到了 $\epsilon^{1/2}$ 的 "reduced" 精度，就可以宣告求解成功；否则，它返回一个 `cvx_status` 值为 `Failed`。在 best 模式下，`Inaccurate` 这个状态值是不可能出现的（在 low 模式下同样不会出现，因为 low 模式把 standard 精度和 reduced 精度设为完全相同）。

`cvx_precision` 命令也可以接受一个标量或一个长度为 2 的向量作为参数。如果你传入一个标量，它会把它当作 standard 精度，并自动为你算出一个默认的 reduced 精度值。粗略地说，这个 reduced 精度就是 standard 精度的平方根，同时会施加一些边界以保证它保持在合理范围内。如果你提供一个数值向量，那么其中较小的值会被选作 standard 精度，较大的值被选作 reduced 精度。

`cvx_precision` 命令既可以在一个 cvx model（模型）内部使用，也可以在模型外部使用；两种情况下它的行为不同。如果你在模型内部调用它，例如：

```matlab
cvx_begin
cvx_precision high
...
cvx_end
```

那么你选择的设置只会生效到 `cvx_end` 为止。如果你在模型外部调用它，例如：

```matlab
cvx_precision high
cvx_begin
...
cvx_end
```

那么你选择的设置将**全局生效**；也就是说，它会作用于之后创建并求解的任何模型。在那种需要以不同精度级别构建并求解多个模型的应用中，应当优先采用局部（在模型内部）的做法。

如果你以 function mode 调用 `cvx_precision`，无论是传入一个字符串还是数值，它都会把**之前的精度向量**作为输出返回——这正是你不带任何参数调用它时所能得到的结果。乍一看这似乎有点令人困惑，但这样做的目的是让你可以把之前的取值保存到一个变量里，并在计算结束时再恢复它；例如：

```matlab
cvxp = cvx_precision( 'high' );
cvx_begin
...
cvx_end
cvx_precision( cvxp );
```

在一个可能用到多个 cvx 模型、多个精度级别的大型应用中，这被视为良好的编码礼仪。当然，一种更简单、同样得体的做法是如上所述在 cvx model 内部调用 `cvx_precision`，这样它的效果只持续到该模型结束。

---

## 8.3 杂项 cvx 命令（Miscellaneous cvx commands）

- **`cvx_problem`**：在一个 cvx specification（规范）内键入此命令，可以给出当前问题（problem）的摘要。注意，其中会包含大量你可能不认识的信息——cvx 在问题输入的过程中会将其转换（conversion）为 canonical form（标准形/规范形），并在此过程中生成额外的临时变量和约束。
- **`cvx_clear`**：键入此命令会重置 cvx 系统，从内存中清除所有问题，但**不会**擦除你的任何数值数据。当你犯了一个错误、需要从头再来时，这会很有用。但要注意，在 cvx 的当前版本中，你完全可以直接用 `cvx_begin` 开始另一个模型，之前的模型就会被擦除（并给出一个警告）。
- **`cvx_quiet`**：键入 `cvx_quiet(true)` 会抑制 solver 的屏幕输出。键入 `cvx_quiet(false)` 会恢复屏幕输出。两种情况它都会返回一个表示该 quiet 标志之前状态的逻辑值，这样你之后就可以恢复那个状态——如果你希望把代码分享给他人使用，这是一种良好的做法。
- **`cvx_pause`**：键入 `cvx_pause(true)` 会让 cvx 在调用 solver 之前和之后暂停，等待键盘输入。主要用于演示（demo）目的。键入 `cvx_pause(false)` 会重置这一行为。两种情况它都会返回一个表示该 pause 标志之前状态的逻辑值，这样你之后就可以恢复那个状态。
- **`cvx_where`**：返回 cvx 发行版（distribution）所安装到的目录——前提是 Matlab 路径已设置为包含该发行版。当你想要找到某些有用的子目录（如 `doc`、`examples` 等）时，这很有用。
- **`cvx_version`**：返回对你当前正在使用的 cvx 版本号、构建号（build number）以及硬件平台（hardware platform）的描述。如果你在 cvx 中遇到了一个 bug，请运行此函数，并把它的输出连同你对问题的描述一起复制到你的电子邮件中。

---

## 8.4 赋值与等式约束（Assignments versus equality constraints）

任何在 C 或 Matlab 语言中用过足够长时间的人，都理解 assignment（赋值）与 equality（等式/相等）之间的区别：赋值使用单个等号 `=` 运算符，而相等使用双等号 `==` 运算符。在 cvx 中，这一区别尤为重要；混淆这两个运算符可能会带来严重的后果。

即使这一区别已经理解得很清楚，在 cvx 中使用赋值仍有一些重要的注意事项（caveats），我们在这里一并加以说明。

在 cvx specification 内无意中使用赋值，会导致一些微妙但关键的（subtle but critical）问题。例如，设 $A,C\in\mathbb{R}^{n\times n}$，$b\in\mathbb{R}$ 为给定数据，考虑如下简单的 SDP：

```math
\begin{aligned}
& \text{minimize} & & \operatorname{Tr}(CX) \\
& \text{subject to} & & \operatorname{Tr}(AX) = b \\
& & & X \succeq 0
\end{aligned}
\tag{14}
```

假设我们试图在 cvx 中如下表达这个问题：

```matlab
n = 5;
A = randn(n,n); C = randn(n,n); b = randn;
cvx_begin
variable X(n,n) symmetric;
minimize( trace( C * X ) );
subject to
trace( A * X ) == b;
X = semidefinite(n);
cvx_end
```

> **数学问题 ↔ CVX 代码对应说明**：目标 $\operatorname{Tr}(CX)$ 对应 `minimize( trace( C * X ) )`；约束 $\operatorname{Tr}(AX)=b$ 对应 `trace( A * X ) == b`；约束 $X\succeq 0$（$X$ 半正定）本应对应 `X == semidefinite(n)`，但这里第 8 行写成了单个等号 `X = semidefinite(n)`——这是一个赋值，而不是等式约束。

乍一看，第 8 行似乎是在把 $X$ 约束为半正定；但它其实是一个 assignment，而不是一个 equality constraint。因此，$X$ 实际上被一个匿名的半正定变量覆盖（overwrite）了，而原始的 $X$ 完全没有受到约束！

幸运的是，这个特定的错误很容易被 cvx 捕获并防止。当执行到 `cvx_end` 时，cvx 会检查每个已声明变量，验证它仍然指向声明时最初赋给它的那个变量对象。如果不是，它就会发出类似下面的错误：

```
??? Error using ==> cvx_end
The following cvx variable(s) have been overwritten:
X
This is often an indication that an equality constraint was
written with one equals '=' instead of two '=='. The model
must be rewritten before cvx can proceed.
```

> **报错信息中文解释**：`cvx_end` 报错——以下 cvx 变量已被覆盖：`X`。这通常表明某个等式约束写成了一个等号 `=` 而不是两个等号 `==`。必须重写该模型，cvx 才能继续执行。

我们希望这一检查能够至少防止一部分书写错误（typographical errors）给你的模型带来令人沮丧的后果。

当然，这一项检查并不会禁止你在 cvx specification 内使用**所有**赋值，它只禁止那些覆盖了正式声明变量（formally declared variables）的赋值。正如 §3.7 所讨论的，其他种类的赋值是允许的，而且可能是真正有用的。但在我们看来，它们应当尽量少用。例如，考虑 §3.7 的第一个例子：

```matlab
variables x y
z = 2 * x - y;
square( z ) <= 3;
quad_over_lin( x, z ) <= 1;
```

下面这种把 `z` 声明为正式变量的替代写法，在数值上是等价的：

```matlab
variables x y z
z == 2 * x - y;
square( z ) <= 3;
quad_over_lin( x, z ) <= 1;
```

> **数学问题 ↔ CVX 代码对应说明**：这里 `z = 2*x - y` 是一个中间计算（intermediate calculation）。第一种写法用赋值 `=` 生成表达式 `z`，第二种写法把 `z` 声明为变量并用等式约束 `==` 把关系 `z = 2x - y` 固定下来；其余两个约束 `square(z) <= 3` 与 `quad_over_lin(x,z) <= 1` 完全相同。两种写法表达的数学问题一致。

我们建议尽可能采用后一种做法。把中间计算声明为变量，可以为你的模型提供额外的一层清晰度，并且会把模型中更多的结构暴露给 solver，从而有可能提升性能。

---

## 8.5 带索引的对偶变量（Indexed dual variables）

在某些模型中，约束的数量取决于模型参数——而不仅仅取决于它们的尺寸。在 cvx 中构建这样的模型是直接了当的，比如使用一个 Matlab 的 for 循环。为了给这些约束中的每一个都赋一个独立的 dual variable（对偶变量），我们必须找到一种方式来同样调整对偶变量的数量。为此，cvx 支持**带索引的对偶变量**。实际上，它们不过是标准的 Matlab 元胞数组（cell arrays），其元素是 cvx 对偶变量对象。

我们用一个例子来说明如何声明和使用带索引的对偶变量。考虑如下半定规划：

```math
\begin{aligned}
& \text{minimize} & & \sum_{i=1}^{n}(n-i)X_{ii} \\
& \text{subject to} & & \sum_{i=1}^{n} X_{i,i+k} = b_k,\quad k = 1,2,\dots,n \\
& & & X \succeq 0
\end{aligned}
\tag{15}
```

（参见 [Stu99]）。这个问题在把每条对角线上的元素之和保持为常数的同时，最小化一个半正定矩阵主对角线元素的加权和。问题的参数是向量 $b\in\mathbb{R}^{n}$ 的元素，优化变量是一个对称矩阵 $X\in\mathbb{R}^{n\times n}$。这个模型的 cvx 版本是：

```matlab
cvx_begin
variable X( n, n ) symmetric
minimize( ( n - 1 : -1 : 0 ) * diag( X ) );
for k = 0 : n-1,
sum( diag( X, k ) ) == b( k+1 );
end
X == semidefinite(n);
cvx_end
```

> **数学问题 ↔ CVX 代码对应说明**：目标 $\sum_{i=1}^{n}(n-i)X_{ii}$（即对主对角线元素按权重 $n-1,n-2,\dots,0$ 加权求和）对应 `minimize( ( n - 1 : -1 : 0 ) * diag( X ) )`，其中 `diag(X)` 取出主对角线元素，行向量 `(n-1:-1:0)` 与其相乘完成加权；约束 $\sum_{i=1}^n X_{i,i+k} = b_k$（$k=1,\dots,n$）对应 for 循环里第 $k$ 条对角线上的元素求和等于 `b(k+1)`，即 `sum( diag( X, k ) ) == b( k+1 )`；约束 $X\succeq 0$ 对应 `X == semidefinite(n)`。

如果我们希望为这 $n$ 个简单等式约束获取对偶信息（dual information），就需要一种方法给 for 循环中的每个约束赋一个独立的对偶变量。这可以如下实现：

```matlab
cvx_begin
variable X( n, n ) symmetric
```

---

## 第 49 页（续：dual variables 用法示例）

```matlab
dual variables y{n}
minimize( ( n - 1 : -1 : 0 ) * diag( X ) );
for k = 0 : n-1,
    sum( diag( X, k ) ) == b( k+1 ) : y{k+1};
end
X == semidefinite(n);
cvx_end
```

上面这段代码的含义，逐行对应如下：

- `dual variables y{n}` —— 分配一个长度为 $n$ 的 **cell array（元胞数组）**，用于存放 $n$ 个 **dual variable（对偶变量）**，结果存放在 MATLAB 变量 `y` 中。对偶变量是凸优化问题中与原约束一一对应的乘子变量，其最优值刻画了约束对目标函数的影响程度（灵敏度）。
- `minimize( ( n - 1 : -1 : 0 ) * diag( X ) )` —— **目标函数（objective）**：向量 $(n-1,\,n-2,\,\dots,\,1,\,0)$ 与 `diag(X)`（矩阵 $X$ 主对角线元素构成的向量）的内积，作为最小化目标。
- `for k = 0 : n-1, ... end` —— 一个循环，产生 $n$ 条**等式约束（equality constraint）**。
- `sum( diag( X, k ) ) == b( k+1 ) : y{k+1};` —— 第 $k$ 条约束：$X$ 的第 $k$ 条对角线（`diag(X,k)` 取第 $k$ 条上对角线）上元素之和等于 $b(k+1)$。冒号后的 `: y{k+1}` 把这条约束与对偶变量 `y{k+1}` 绑定，使每条约束各有一个独立的对偶变量。
- `X == semidefinite(n);` —— 约束 $X$ 为 $n \times n$ 的**半正定矩阵（semidefinite）**。
- `cvx_end` —— 结束建模并求解。

当 `cvx_end` 命令执行时，CVX 会求出这些对偶变量的最优值，并把它们存入一个 $n$ 元胞数组 `y` 中。

这个例子确实略有些简化。稍加安排，就能把这个模型改写成把 $n$ 条等式约束合并为一条向量约束的形式，那样就只需要一个向量对偶变量了。[^3] 若要找一个无法如此简化的更复杂例子，请参阅 CVX 发行版中的文件：

```
examples/cvxbook/Ch07_statistical_estim/cheb.m
```

在那个问题里，`for` 循环中的每条约束都是一个**线性矩阵不等式（linear matrix inequality，LMI）**，而非标量线性方程；因此带下标索引的对偶变量是对称矩阵，而不是标量。

[^3]: 事实上，CVX 的未来版本将支持使用 MATLAB 函数 `spdiags`，届时整个 `for` 循环将可化简为单条约束 `spdiags(X,0:n-1)==b`。

---

## A　安装与兼容性（Installation and compatibility）

CVX 需要 MATLAB 6.1 或更高版本。[^4] CVX 的主要开发在一台运行 MATLAB 7.4 的 Intel 架构 Mac 上进行，另在以下平台与 MATLAB 版本上做了补充开发：

- Windows（32 位）：MATLAB 6.1、7.4
- Linux（32 位）：MATLAB 6.1、7.4
- Linux（64 位）：MATLAB 7.1、7.4

出于测试与支持目的，我们可访问 Windows、Linux 与 Mac（含 Intel 和 PowerPC）上绝大多数其他版本的 MATLAB。唯一的重要例外是版本 7.0.0，因此我们无法为其提供支持。

版本 7.0.0 至少对一位用户表现出了数值问题；这些问题在升级后即被消除。我们还发现了 CVX 与 MATLAB 7.0.1 和 7.0.4 搭配时的另一个问题；相关绕过办法见下文 §A.4。

已为 Windows（32 位）、Linux（32 位）与 Intel Mac OS X 提供了**预编译的 MEX 文件（precompiled MEX files）**。对于其他平台，MEX 文件将在安装过程中编译；见 §A.2。对于上述列表之外的平台（如 Solaris、HPUX），CVX 极有可能也能工作，但我们无法提供支持。

[^4]: 本手册此前版本曾称 CVX 需要 MATLAB 6.0 或更高版本；我们后来发现 6.0 并不够用。

---

### A.1　基本安装步骤（Basic instructions）

1. 从 http://www.stanford.edu/~boyd/cvx 获取最新版 CVX。你可以把软件包下载为 `.zip` 文件或 `.tar.gz` 文件。
2. 如果你此前一直在运行旧版 CVX，请先移除它或把它挪开——例如将其重命名为 `cvx_old`——再继续。**切勿**让新版 CVX 直接解压覆盖在旧版之上。
3. 把文件解压到任意位置；会生成一个名为 `cvx` 的目录。有一个重要例外：**不要**把 CVX 放到 MATLAB 自带的 toolbox（工具箱）目录中。（如果你此前的 CVX 安装在那个位置，需要把它移出来。）
4. 启动 MATLAB。
5. 把当前目录切换到 CVX 所在位置。例如，如果你在 Windows 机器上把 `cvx.zip` 解压到目录 `C:\Matlab\personal`，则在 MATLAB 命令提示符下输入：

```matlab
cd C:\Matlab\personal\cvx
```

  或者，如果你在 Linux 机器上把 `cvx.tar.gz` 解压到目录 `~/matlab`，则在 MATLAB 命令提示符下输入：

```matlab
cd ~/matlab/cvx
```

6. 在 MATLAB 提示符下输入命令：

```matlab
cvx_setup
```

  这条命令做两件事：设置 MATLAB 搜索路径以便找到所有 CVX 程序文件，并运行一个简单测试问题来验证安装。在某些平台上它还会编译 MEX 文件（见下文 §A.2）。如果一切顺利，命令会输出这样一行（另有其他输出）：

```text
No errors! cvx has been successfully installed.
```

  如果未显示这条消息，或产生了任何警告或错误，就说明 CVX 安装有问题。尝试重新安装软件包；若仍失败，请向我们发送 bug 报告并说明情况。

7. 如果你打算经常使用 CVX，需要保存当前 MATLAB 路径以供后续 MATLAB 会话使用。按 `cvx_setup` 提供的说明完成此操作。

---

### A.2　MEX 文件编译（MEX file compilation）

只有少数平台提供了预编译的 MEX 文件；其他平台必须由用户自行编译。`cvx_setup` 脚本会自动检测是否存在编译好的 MEX 文件，若缺失，则为 SeDuMi、SDPT3 和 CVX 编译它们。不过，只有当你的机器上 MEX 系统已正确配置时，`cvx_setup` 才会成功。如果你以前编译过 MEX 文件，大概率就是这种情况；否则，在 MATLAB 提示符下运行命令：

```matlab
mex -setup
```

并回答随后提出的问题。如有必要，查阅 MATLAB 文档以了解该过程的细节。

---

### A.3　关于 SeDuMi 与 SDPT3（About SeDuMi and SDPT3）

CVX 发行版在目录 `cvx/sedumi` 与 `cvx/sdpt3` 中分别包含了求解器 SeDuMi 与 SDPT3 的副本。我们**强烈建议**使用我们提供的这两个求解器版本，并移除 MATLAB 路径中任何其他版本。CVX 自带的 SeDuMi 版本集成了若干 bug 修复及一处轻微改动，这些目前尚未出现在作者发布的版本中。SDPT3 则没有这种情况；不过，SDPT3 的作者经常在**不更新版本号**（目前为 4.0 beta）的情况下发布 bug 修复。因此，务必确保你想使用的任何更新版 SDPT3 是在你获取 CVX **之后**下载的。

---

### A.4　一个 MATLAB 7.0 的问题（A Matlab 7.0 issue）

CVX 用来在 MATLAB 内部嵌入一种新建模语言的技术，似乎会给 MATLAB 7.0 带来一些困扰，具体是版本 7.0.4（R14SP2）。我们相当确信该问题源于 MATLAB 自身的一个 bug。它在 6.5.2 及更早版本、7.1 及更高版本中不会出现，而在更早的 MATLAB 7.0 版本（R14、R14SP1）中可能出现。

这个 bug 是这样的：在某些情况下，一个包含 CVX 模型的 `.m` 文件会触发一个如下所示的错误：

```text
??? Error: File: thrusters.m Line: 43 Column: 5
"p" was previously used as a variable,
conflicting with its use here as the name of a function.
```

错误解释：文件名、行/列号以及变量名可能不同，但错误信息本身保持不变。其含义是——变量 `p` 之前被当作变量使用过，现在却被当作函数名使用，二者发生冲突。

产生这一特定错误的例子是一条简单的不等式约束：

```matlab
p >= -5;
```

其中 `p` 是先前用 `variable` 语句声明过的一个 CVX 变量。有意思的是，同一模型中的另一条不等式约束

```matlab
u >= 0
```

却不会引起任何抱怨。因此我们无法给出一个能精确触发该错误的条件集合。所幸，绕过办法非常简单：只需删去约束中的全部多余空格。在本例中，把约束改成：

```matlab
p>=-5;
```

即可消除该错误。你仍可按需缩进这条约束——只要去掉中间的空格即可。我们也不清楚这个办法为何有效，但它确实可靠地有效。

---

## B　运算符、函数与集合（Operators, functions, and sets）

### B.1　基本运算符与线性函数（Basic operators and linear functions）

MATLAB 的标准算术运算——加法 `+`、减法 `-`、乘法 `*` `.*`、除法 `/` `\` `./` `.\`、幂运算 `^` `.^`——已被**重载（overloaded）**，只要其用法同时符合标准数学惯例、MATLAB 惯例以及 **DCP 规则集（disciplined convex programming ruleset）**，即可在 CVX 中工作。DCP（disciplined convex programming，规范化凸规划）是一套约束表达式构造方式的规则，保证按规则写出的模型一定能被凸优化求解器可靠求解。例如：

- 两个 CVX 表达式若维度相同（或其中一个为标量）且**曲率（curvature）**相同（即二者同为凸、凹或仿射），则可以相加。曲率指表达式作为变量的函数是凸、凹还是仿射的性质。
- 一个 CVX 表达式可以乘以或除以一个标量常数。若常数为正，曲率保持不变；若为负，曲率反转（凸变凹、凹变凸）。
- 一个仿射列向量 CVX 表达式可以乘以一个维度匹配的常数矩阵；也可以被一个维度匹配的**非奇异（non-singular）**常数矩阵左除。

当然还有众多其他可行组合。例如，幂运算 `^` `.^` 的使用较为受限；见下文 §B.2。

MATLAB 的基本矩阵操作与算术运算也已扩展至 CVX 表达式，包括：

- **拼接（Concatenation）**：`[ A, B ; C, D ]`
- **索引（Indexing）**：`x(n+1:end)`、`X([3,4],:)` 等
- **索引赋值（Indexed assignment）**，含删除操作：`y(2:4) = 1`、`Z(1:4,:) = []` 等
- **转置与共轭转置（Transpose and conjugate transpose）**：`Z.'`、`y'`

MATLAB 的若干基本函数也已扩展至 CVX 表达式：

| 函数 | 函数 | 函数 |
|------|------|------|
| `conj` | `conv` | `cumsum` |
| `diag` | `dot` | `find` |
| `fliplr` | `flipud` | `flipdim` |
| `horzcat` | `hankel` | `ipermute` |
| `kron` | `permute` | `repmat` |
| `reshape` | `rot90` | `sparse` |
| `sum` | `trace` | `tril` |
| `triu` | `toeplitz` | `vertcat` |

> 说明：原文中以竖排多列列出上述函数名。为便于对照，此处以 3 列表格完整列出全部 24 个函数名：`conj`、`conv`、`cumsum`、`diag`、`dot`、`find`、`fliplr`、`flipud`、`flipdim`、`horzcat`、`hankel`、`ipermute`、`kron`、`permute`、`repmat`、`reshape`、`rot90`、`sparse`、`sum`、`trace`、`tril`、`triu`、`toeplitz`、`vertcat`。

其中大多数函数对 CVX 表达式的行为应与对数值表达式一致。那些执行某种求和（如 `cumsum`、`sum`）或乘法（如 `conv`、`dot`、`kron`）的函数，只能按 DCP 规则使用。例如，`kron(X,Y)` 仅当 `X` 或 `Y` 之一是常数时才合法；而 `trace(Z)` 仅当对角线上的元素具有相同曲率时才合法。

---

下面给出 CVX 支持的**两份**非线性函数清单：第一份是**被扩展用于 CVX 的标准 MATLAB 函数**清单；第二份是**专门为 CVX 新创建的函数**清单。

在某些情况下，底层 solver（求解器）的局限性会对这些函数的使用施加特定限制或注意事项：

- 带有匕首标记（†）的函数，CVX 所用的 solver **原生并不支持**。它们通过一种**逐次逼近方法（successive approximation）**来处理——该方法会对底层 solver 进行多次调用，最终达到相同的精度（详见 §D.1）。如果你使用了这些函数之一，你会收到一条警告，提示将采用逐次逼近。
- 涉及幂运算（例如 `x^p`）与 p-范数（例如 `norm(x,p)`）的函数带有星号标记（⋆）。当 p 为**有理数**时，CVX 会**精确**表示这些函数；当 p 为无理数时，CVX 会用 MATLAB 的 `rat` 函数选取一个**邻近的有理数**来近似。详见 §D.2。

## B.2.1 内置函数（Built-in functions）

- `abs`：实数和复数数组的**绝对值（absolute value）**。凸（convex）。
- † `exp`：**指数函数（exponential）**。凸且非递减（nondecreasing）。
- † `log`：**对数（logarithm）**。凹（concave）且非递减。
- `max`：**最大值（maximum）**。凸且非递减。
- `min`：**最小值（minimum）**。凹且非递减。
- ⋆ `norm`：实数、复数向量和矩阵的**范数（norm）**。凸。单参数版本 `norm(x)` 对向量计算 **2-范数**，对矩阵计算**诱导 2-范数**（即**最大奇异值**）。双参数版本 `norm(x,p)` 的支持情况如下：
  - 对向量：接受所有 `p ≥ 1`，但 CVX 如何处理除 1、2 和 Inf 之外的值，详见附录 D.2。
  - 对矩阵：`p` 必须是 `1`、`2`、`Inf` 或 `'Fro'`。
- `polyval`：**多项式求值（polynomial evaluation）**。`polyval(p,x)` 中，`p` 是长度为 `n` 的向量，它计算：

```math
p(1) \cdot x^{n-1} + p(2) \cdot x^{n-2} + \cdots + p(n-1) \cdot x + p(n)
```

该函数在 CVX 中有两种使用方式：

- 如果 `p` 是**变量**而 `x` 是**常量**，则 `polyval(x,p)` 计算的是 `p` 各元素的**线性组合**。该组合必须满足 DCP（disciplined convex programming，规范化凸规划）规则中关于**加法**与**缩放（scaling）**的要求。
- 如果 `p` 是**常量**而 `x` 是**变量**，则 `polyval(x,p)` 构造一个关于变量 `x` 的**多项式函数**。该多项式必须是**仿射（affine）**、**凸**或**凹**的，且 `x` 必须是实数且仿射的。
- ⋆ `power`：`x^p` 与 `x.^p`，其中 `x` 是实变量、`p` 是实常量。对于 `x^p`，`x` 和 `p` 都必须是**标量**。只有那些能够合理且无歧义地被解释为凸或凹的 `p` 值才被接受：
  - `p = 0`：常量。`x.^p` 恒等于 1。
  - `0 < p < 1`：凹。参数 `x` 必须是凹的（或仿射的），并且被**隐式约束为非负**。
  - `p = 1`：仿射。此时 `x.^p` 就等于 `x`。
  - `p ∈ {2, 4, 6, 8, ...}`：凸。参数 `x` 必须是仿射的。
  - `p > 1`，且 `p ∉ {2, 3, 4, 5, ...}`：凸。参数 `x` 必须是仿射的，并且被**隐式约束为非负**。

  `p` 的**负值**与**奇整数**值是不被允许的，但可参见下一节的 `pow_p`、`pow_pos` 和 `pow_abs` 函数作为有用的替代。
- † `power`：`p.^x` 与 `p^x`，其中 `p` 是实常量、`x` 是实变量。对于 `p^x`，`p` 和 `x` 都必须是标量。`p` 的有效取值包括：
  - `p ∈ {0, 1}`：常量。
  - `0 < p < 1`：凸且非递增（nonincreasing）；`x` 必须是凹的。
  - `p > 1`：凸且非递减；`x` 必须是凸的。

  `p` 的负值不被允许。
- `sqrt`：**平方根（square root）**。隐式约束其参数为非负。凹且非递减。

## B.2.2 新的非线性函数（New nonlinear functions）

尽管这些函数是专门为 CVX 开发的，但在提供数值参数时，它们在 CVX 规范（specification）之外同样可以工作。

- `berhu(x,M)`：**反向 Huber 函数（reversed Huber function，故称 Berhu）**，定义为：当 `|x| ≤ M` 时为 `|x|`，当 `|x| ≥ M` 时为 `(|x|^2 + M^2)/2M`。凸。如果省略 `M`，则假定 `M = 1`；但如果提供了 `M`，它必须是**正的常量**。也可以以三个参数调用，写作 `berhu(x,M,t)`，它计算 `t + t*berhu(x/t,M)`，用于**伴随尺度估计（concomitant scale estimation）**（见 [Owe06]）。
- `det_inv`：对称（或 Hermitian）正定矩阵**逆矩阵的行列式**，即 `(det X)^{-1}`，它等价于**各特征值倒数之和**。在 CVX 规范内使用时，`det_inv` 会把矩阵约束为对称（实数情形）或 Hermitian（复数情形）且**半正定（positive semidefinite）**。以数值参数使用时，若这些约束不满足，`det_inv` 返回 `-Inf`。凹。
- `det_rootn`：半定矩阵**行列式的 n 次方根**，即 `(det X)^{1/n}`。在 CVX 规范内使用时，`det_rootn` 会把矩阵约束为对称（实数情形）或 Hermitian（复数情形）且半正定。以数值参数使用时，若这些约束不满足，`det_rootn` 返回 `-Inf`。凹。
- `det_root2n`：半定矩阵**行列式的 2n 次方根**，即 `det_root2n(X) = sqrt(det_rootn(X))`。凹。仅出于**向后兼容（back-compatibility）**目的而保留。
- † `entr`，**逐元素熵函数（elementwise entropy function）**：`entr(x) = x.*log(x)`。凹。当以含有负元素的常量参数调用时返回 `-Inf`。
- `geo_mean`：向量的**几何平均（geometric mean）**，即 $(\prod_{k=1}^{n} x_k)^{1/n}$。在 CVX 规范内使用时，`geo_mean` 会把向量的元素约束为非负。以数值参数使用时，若任一元素为负，`geo_mean` 返回 `-Inf`。凹且递增（increasing）。
- `huber(x,M)`：定义为：当 `|x| ≥ M` 时为 `2M|x| − M^2`，当 `|x| ≤ M` 时为 `|x|^2`。凸。如果省略 `M`，则假定 `M = 1`；但如果提供了 `M`，它必须是正的常量。也可以写作 `huber(x,M,t)`，它计算 `t + t*huber(x/t,M)`，用于伴随尺度估计（见 [Owe06]）。
- `huber_circ(x,M)`：**圆对称 Huber 函数（circularly symmetric Huber function）**，定义为：当 `‖x‖_2 ≤ M` 时为 `‖x‖_2`，当 `‖x‖_2 ≥ M` 时为 `2M‖x‖_2 − M^2`。与 `huber_pos(norm(x),M)` 相同（并按此实现）。凸。
- `huber_pos(x,M)`：对非负 `x` 与 Huber 函数相同；对负 `x` 为零。凸且非递减。
- `inv_pos`：**正部分的倒数（inverse of the positive portion）**，即 `1/max{x, 0}`。在 CVX 规范内，会施加其参数为正的约束。在 CVX 规范外，若 `x ≤ 0` 则返回 `+∞`。凸且递减（decreasing）。
- † `kl_div`：**逐元素 Kullback-Leibler 距离**，`kl_div(x,y) = x.*log(x./y) − x + y`，其中 `x`、`y` 非负，且当 `y(i)` 为零时 `x(i)` 也为零。凸。在 CVX 规范外，若参数不在定义域内则返回 `+∞`。
- `lambda_max`：实数对称或复数 Hermitian 矩阵的**最大特征值（maximum eigenvalue）**。在 CVX 内，会施加其参数为对称（实数情形）或 Hermitian（复数情形）的约束。凸。
- `lambda_min`：实数对称或复数 Hermitian 矩阵的**最小特征值（minimum eigenvalue）**。在 CVX 内，会施加其参数为对称（实数情形）或 Hermitian（复数情形）的约束。凹。
- `log_det`：正定矩阵**行列式的对数**，即 `log det(X)`。在 CVX 规范内使用时，`log_det` 会将其参数约束为对称（实数情形）或 Hermitian（复数情形）且**正定（positive definite）**。以数值参数使用时，若这些约束不满足，`log_det` 返回 `-Inf`。凹。
- ⋆ `log_norm_cdf(x)`：**标准正态随机变量的累积分布函数（cumulative distribution function）的对数**。凹且递增。当前实现是一个相当粗略的**SDP 可表示（SDP-representable）近似**，在区间 [−4, 4] 上精度一般；我们打算在某个时候用更好的近似来替换它。
- † `log_sum_exp(x)`：`x` 各元素**逐元素指数之和的对数**。凸且非递减。它在 **expert GP mode（专家 GP 模式）**下被内部使用，但也可以用于标准 DCP 中。
- `logsumexp_sdp`：对 log-sum-exp 函数的**多项式近似**，具有**全局绝对精度**。此近似用于**默认 GP mode（GP 模式）**下，但也可以用于标准 DCP 中。
- `matrix_frac(x,Y)`：**矩阵分数函数（matrix fractional function）**，即 `x^T Y^{-1} x`。在 CVX 中，会施加 `Y` 为对称（或 Hermitian）且正定的约束；在 CVX 之外，除非 `Y = Y^T ≻ 0`，否则返回 `+∞`。凸。
- `norm_largest(x, k)`：对实数和复数向量，返回向量 `x` 中**最大的 k 个幅值（magnitude）之和**。凸。
- `norm_nuc(X)`：实数或复数矩阵 `X` 的**奇异值之和**（它是通常的**谱矩阵范数（spectral matrix norm）**，即最大奇异值，的**对偶（dual）**）。凸。
- ⋆ `norms(x, p, dim)` 与 `norms_largest(x, k, dim)`：沿矩阵或 N 维数组的**指定维度**计算向量范数。用于**范数之和（sum-of-norms）**与**范数之最大（max-of-norms）**问题。凸。
- `poly_env(p, x)`：计算由 `p` 描述的（按 `polyval` 意义上的）多项式的**凸包络或凹包络（convex or concave envelope）**的值。`p` 必须是实常量向量，其长度 `n` 为 0、1、2、3 或其他**奇数**长度；`x` 必须是实数且仿射的。`p` 的**首个非零元素的正负号**决定了构造的是凸（正）包络还是凹（负）包络。例如，考虑函数 $p(x) \triangleq (x^2 - 1)^2 = x^4 - 2x^2 + 1$，它与它的凸包络一并绘于图 2 中。当 `|x| ≥ 1` 时二者重合，但当 `|x| < 1` 时二者偏离。若在 CVX 模型中尝试调用 `polyval([1,0,2,0,1],x)` 会产生错误，但调用 `poly_env([1,0,2,0,1],x)` 则会得到——

---

## Figure 2（图 2）说明

**Figure 2: The polynomial function $p(x) = x^4 - 2x^2 + 1$ and its convex envelope.**

图 2：多项式函数 $p(x) = x^4 - 2x^2 + 1$ 及其**凸包络（convex envelope，即不高于该函数的最大凸函数）**。

图 2 中绘出了 $p(x)$ 的曲线，以及它在 $[-1.5, 1.5]$ 区间上的 convex envelope。下面列出支持的多项式函数及相关 atom。

> **valid representation of the envelope.** For convex or concave polynomials, this function produces the same result as `polyval`.

上述函数给出了 convex envelope 的合法表示。对于**凸（convex）或凹（concave）多项式**，该函数产生的结果与 `polyval` 相同。

---

## B.2（续）向量/矩阵函数 atom 列表

以下是 CVX 函数库中支持的部分**原子函数（atom，即凸优化建模里可直接识别其 curvature 与 sign 的基本函数块）**：

- **`pos`**：$\max\{x, 0\}$，$x$ 为实数。**凸（convex）且递增（increasing）**。

- **`pow_abs(x,p)`**：$|x|^p$，其中 $x \in \mathbb{R}$ 或 $x \in \mathbb{C}$，且 $p \ge 1$。**凸（convex）**。若 $p$ 为无理数，则选取一个邻近的有理数值；详见附录 D.2。

- **`pow_pos(x,p)`**：$(\max\{x,0\})^p$，其中 $x \in \mathbb{R}$ 且 $p \ge 1$。**凸（convex）且非递减（nondecreasing）**。若 $p$ 为无理数，则选取一个邻近的有理数值；详见附录 D.2。

- **`pow_p(x,p)`**：对 $x \in \mathbb{R}$ 与实常数 $p$，计算幂函数的**非负凸分支与凹分支（convex and concave branches）**：

  ```math
  p \le 0:\quad
  f_p(x) \triangleq
  \begin{cases}
  x^p, & x > 0 \\
  +\infty, & x \le 0
  \end{cases}
  \qquad\text{凸（convex），非递增（nonincreasing）}
  ```

  ```math
  0 < p \le 1:\quad
  f_p(x) \triangleq
  \begin{cases}
  x^p, & x \ge 0 \\
  -\infty, & x < 0
  \end{cases}
  \qquad\text{凹（concave），非递减（nondecreasing）}
  ```

  ```math
  p \ge 1:\quad
  f_p(x) \triangleq
  \begin{cases}
  x^p, & x \ge 0 \\
  +\infty, & x < 0
  \end{cases}
  \qquad\text{凸（convex），非单调（nonmonotonic）}
  ```

- **`quad_form(x,P)`**：计算 $x^T P x$（$x$ 为实数、$P$ 为对称矩阵时），或 $x^H P x$（$x$ 为复数、$P$ 为 Hermitian 矩阵时）。当 $P$ 为常量且**半正定（positive semidefinite，PSD）**时，它是关于 $x$ 的凸函数；当 $P$ 为常量且**半负定（negative semidefinite）**时，是关于 $x$ 的凹函数。之所以提供这个函数，是因为 `cvx` 无法把 `x'*P*x` 识别为凸（即使 $P$ 是半正定的）。

---

## 第 59 页

- **`quad_over_lin`**：计算 $x^T x / y$（$x \in \mathbb{R}^n$、$y > 0$）；对 $x \in \mathbb{C}^n$、$y > 0$ 则为 $x^* x / y$。在 `cvx` 建模规范中，会额外添加约束 $y > 0$；在 `cvx` 规范之外，若 $y \le 0$ 则返回 $+\infty$。**凸（convex），且关于 $y$ 递减（decreasing in $y$）**。

- **`quad_pos_over_lin`**：计算 `sum_square_pos( x ) / y`，$x \in \mathbb{R}^n$、$y > 0$。**凸（convex），关于 $x$ 递增（increasing in $x$），关于 $y$ 递减（decreasing in $y$）**。

- **`rel_entr`**：标量**相对熵（relative entropy）**：`rel_entr(x,y) = x.*log(x/y)`。**凸（convex）**。

- **`sigma_max`**：实数或复数矩阵的**最大奇异值（maximum singular value）**，与 `norm` 相同。**凸（convex）**。

- **`square`**：$x^2$，$x \in \mathbb{R}$。**凸（convex）**。

- **`square_abs`**：$|x|^2$，$x \in \mathbb{R}$ 或 $x \in \mathbb{C}$。

- **`square_pos`**：$(\max\{x,0\})^2$，$x \in \mathbb{R}$。**凸（convex）且递增（increasing）**。

- **`sum_largest(x,k)`**：实数向量 $x$ 中最大的 $k$ 个元素之和。**凸（convex）且递增（increasing）**。

- **`sum_smallest(x,k)`**：实数向量 $x$ 中最小的 $k$ 个元素之和，即 `-sum_largest(-x,k)`（原文写作 `-sum_smallest(-x,k)`，按语义应为 `-sum_largest(-x,k)`，属原文档笔误）。**凹（concave）且递减（decreasing）**。

- **`sum_square`**：`sum( square( x ) )`。**凸（convex）**。

- **`sum_square_abs`**：`sum( square_abs( x ) )`。**凸（convex）**。

- **`sum_square_pos`**：`sum( square_pos( x ) )`；仅对实数值有效。**凸（convex）且递增（increasing）**。

- **`trace_inv(X)`**：对称正定（SPD）矩阵 $X$ 的**逆的迹（trace of the inverse）**，等于其特征值倒数之和。**凸（convex）**。在 `cvx` 之外，若参数不是正定的，则返回 `+Inf`。

- **`trace_sqrtm(X)`**：半正定矩阵 $X$ 的**矩阵平方根的迹（trace of the matrix square root）**，等于其特征值平方根之和。**凹（concave）**。在 `cvx` 之外，若参数不是半正定的，则返回 `+Inf`。

---

## B.3 集合（Sets）

`cvx` 目前支持以下集合；在各定义中，$n$ 为正整数常量。

- **`nonnegative(n)`**：非负锥

  ```math
  \mathbb{R}_+^n \triangleq \{ x \in \mathbb{R}^n \mid x_i \ge 0,\ i = 1, 2, \dots, n \}
  ```

- **`simplex(n)`**：单纯形（概率单纯形）

  ```math
  \mathbb{R}_{1+}^n \triangleq \{ x \in \mathbb{R}^n \mid x_i \ge 0,\ i = 1, 2, \dots, n,\ \sum_i x_i = 1 \}
  ```

---

## 第 60 页

- **`lorentz(n)`**：二阶锥（Lorentz cone / second-order cone）

  ```math
  \mathcal{Q}^n \triangleq \{ (x, y) \in \mathbb{R}^n \times \mathbb{R} \mid \lVert x \rVert_2 \le y \}
  ```

- **`rotated_lorentz(n)`**：旋转二阶锥（rotated Lorentz cone）

  ```math
  \mathcal{Q}_r^n \triangleq \{ (x, y, z) \in \mathbb{R}^n \times \mathbb{R} \times \mathbb{R} \mid \lVert x \rVert_2 \le yz,\ y, z \ge 0 \}
  ```

- **`complex_lorentz(n)`**：复二阶锥（complex Lorentz cone）

  ```math
  \mathcal{Q}_c^n \triangleq \{ (x, y) \in \mathbb{C}^n \times \mathbb{R} \mid \lVert x \rVert_2 \le y \}
  ```

- **`rotated_complex_lorentz(n)`**：旋转复二阶锥（rotated complex Lorentz cone）

  ```math
  \mathcal{Q}_{rc}^n \triangleq \{ (x, y, z) \in \mathbb{C}^n \times \mathbb{R} \times \mathbb{R} \mid \lVert x \rVert_2 \le yz,\ y, z \ge 0 \}
  ```

- **`semidefinite(n)`**：半正定矩阵锥（positive semidefinite cone）

  ```math
  \mathcal{S}_+^n \triangleq \{ X \in \mathbb{R}^{n \times n} \mid X = X^T,\ X \succeq 0 \}
  ```

- **`hermitian_semidefinite(n)`**：Hermitian 半正定矩阵锥（Hermitian positive semidefinite cone）

  ```math
  \mathcal{H}_+^n \triangleq \{ Z \in \mathbb{C}^{n \times n} \mid Z = Z^H,\ Z \succeq 0 \}
  ```

  > 原文此处右侧矩阵变量写作 $X \succeq 0$，按定义应为 $Z \succeq 0$（原文档笔误），上式为校正后的定义。

- **`nonneg_poly_coeffs(n)`**：次数为 $n$ 的**非负多项式系数锥（cone of coefficients of nonnegative polynomials）**；$n$ 必须为偶数：

  ```math
  \mathcal{P}_{+,n} \triangleq \left\{ p \in \mathbb{R}^{n+1} \;\middle|\; \sum_{i=0}^{n} p_{i+1} x^{n-i} \ge 0 \ \forall x \in \mathbb{R} \right\}
  ```

- **`convex_poly_coeffs(n)`**：次数为 $n$ 的**凸多项式系数锥（cone of coefficients of convex polynomials）**；$n$ 必须为偶数：

  ```math
  \mathcal{P}_{+,n} \triangleq \left\{ p \in \mathbb{R}^{n+1} \;\middle|\; \sum_{i=0}^{n-2} (n-i)(n-i-1)\,p_{i+1}\,x^{n-i-2} \ge 0 \ \forall x \in \mathbb{R} \right\}
  ```

- **`exponential_cone`**：指数锥（exponential cone）

  ```math
  \mathcal{E} \triangleq \operatorname{cl}\left\{ (x, y, z) \in \mathbb{R} \times \mathbb{R} \times \mathbb{R} \mid y > 0,\ y e^{x/y} \le z \right\}
  ```

- **`geo_mean_cone(n)`**：几何平均锥（geometric mean cone）

  ```math
  \mathcal{G}^n \triangleq \operatorname{cl}\left\{ (x, y) \in \mathbb{R}^n \times \mathbb{R}^n \times \mathbb{R}^n \mid x \ge 0,\ \left(\prod_{i=1}^{n} x_i\right)^{1/n} \ge y \right\}
  ```

---

## C　cvx 状态信息（cvx status messages）

在输入完整的 `cvx` 建模规范并发出 `cvx_end` 命令后，求解器被调用来生成数值结果。求解器会产生五种退出条件之一，它们由字符串变量 `cvx_status` 的取值来指示。`cvx_status` 的名义取值，以及其它变量的相应取值如下：

- **`Solved`（已求解）**：已找到**互补（complementary，即原始 + 对偶同时满足）解**。原始变量（primal variables）和对偶变量（dual variables）被替换为它们的计算值，问题的最优值被放入 `cvx_optval`（按约定，对可行性问题该值为 0）。

- **`Unbounded`（无界）**：通过发现一个**无界的原始方向（unbounded primal direction）**，已证明问题**下方无界（unbounded below）**。该方向存储在原始变量中。`cvx_optval` 的值对最小化问题设为 `-Inf`，对最大化问题也设为 `-Inf`。可行性问题在构造上不可能下方无界。

  需要理解的重要一点：这个无界的原始方向很可能**并不是一个可行点（feasible point）**。如果需要可行点，则应通过省略目标函数，把问题重新作为可行性问题来求解。

- **`Infeasible`（不可行）**：通过发现一个**无界的对偶方向（unbounded dual direction）**，已证明问题**不可行（infeasible）**。该方向的相应分量存储在对偶变量中。原始变量的值被填为 `NaN`。`cvx_optval` 的值对最小化问题和可行性问题设为 `+Inf`，对最大化问题设为 `-Inf`。

  在某些情况下，`SeDuMi` 无法达到它做出上述判断所需的数值确定性——但通过适当放宽这些容差，它能够得出一个较弱的结论。此时会返回以下结果之一：

  - **`Inaccurate/Solved`**：问题很可能存在互补解。
  - **`Inaccurate/Unbounded`**：问题很可能是无界的。
  - **`Inaccurate/Infeasible`**：问题很可能是不可行的。

  原始变量、对偶变量以及 `cvx_optval` 的取值，与「精确」情形完全一致地更新。此外还有两种最终结果：

- **`Failed`（失败）**：求解器未能朝解取得足够的进展。`cvx_optval` 以及原始变量、对偶变量的值都被填为 `NaN`。这一结果可能是由 `SeDuMi` 内部的数值问题导致的，通常是问题在某个方面特别「糟糕」（例如存在非零的对偶间隙 duality gap）。

---

## 第 62 页

- **`Overdetermined`（超定）**：预求解器（presolver）判定该问题的**等式约束数量多于变量数量**，这意味着等式约束的系数矩阵是奇异的（singular）。在实践中，这类问题常常（但并非总是）不可行。遗憾的是，求解器通常无法处理此类问题，因此无法得出精确结论。

  最常产生 `Overdetermined` 结果的几种情形，在下文 §D.3 中讨论。

---

## D　高级求解器专题（Advanced solver topics）

### D.1　逐次逼近法（The successive approximation method）

在 1.2 版本之前，用户最常要求加入 `cvx` 函数库的函数是**指数族（exponential family）**函数，包括 `exp`、`log` 以及各种熵函数（entropy functions）。遗憾的是，`cvx` 使用的是**对称原始/对偶求解器（symmetric primal/dual solvers）**，它们根本无法原生支持这些函数；而各种实际因素也推迟了在 `cvx` 中使用其它类型求解器的进程。

为此，我们构造了一种**逐次逼近法（successive approximation method）**，使对称原始/对偶求解器能够支持指数族函数。该方法的精确机理将另行发表，但我们可以在此给出一个高度简化的描述。首先，我们为 `exp`（或 `log` 等）构造一个全局近似，该近似在某个中心点 $x_0$ 的邻域内是精确的。求解该近似问题可得到一个近似最优点 $\bar{x}$。我们把中心点 $x_0$ 向 $\bar{x}$ 移动，构造新的近似，再次求解。重复这一过程，直到 $|\bar{x} - x_0|$ 足够小，从而断定我们的近似已足够精确以表示原模型。再次强调，这只是对该方法的高度简化描述；例如，我们同时也在监视对偶问题，以指导对 $x_0$ 的移动与终止的判断。

---

## 第 63 页

### D.2　无理数次幂（Irrational powers）

为了实现 $x^p$ 这样的幂表达式以及 $1 < p < \infty$ 时的 $p$-范数 $\lVert x \rVert_p$，`cvx` 使用了一种与 SDP 兼容的方法，该方法在 [AG01] 中给出描述，并由 `cvx` 的作者进行了增强。只要指数 $p$ 是**有理数（rational）**，这种方法就是**精确的（exact）**。为了确定满足 $p_n / p_d = p$ 的整数值 $p_n$、$p_d$，`cvx` 使用了 MATLAB 的 `rat` 函数，并采用其默认容差 $10^{-6}$。目前没有办法修改这一容差。详情请参见 `rat` 的文档。

SDP 实现的复杂度大致取决于 $p_n$ 和 $p_d$ 数值的大小。让我们引入一个更精确的复杂度度量。对于 $p = 2$，约束 $x^p \le y$ 可以恰好用一个 $2 \times 2$ 的 LMI（线性矩阵不等式）表示：

```math
x^2 \le y \implies
\begin{pmatrix} y & x \\ x & 1 \end{pmatrix} \succeq 0.
```

对于 $p = p_n / p_d$ 的其它取值，`cvx` 生成的 $2 \times 2$ LMI 数量取决于 $p_n$ 和 $p_d$ 两者；我们用 $k(p_n, p_d)$ 表示该数量。（同时也会生成若干内部变量与等式约束，但在此分析中我们忽略它们。）一项实证研究表明，对于 $p = p_n / p_d > 1$，`cvx` 满足

```math
k(p_n, p_d) \le \log_2 p_n + \alpha(p_n),
```

其中 $\alpha(p_n)$ 项相比 $\log_2$ 项增长得非常缓慢。事实上，对于 $p_n \le 4096$，我们已经验证 $\alpha(p_n)$ 通常为 1 或 2，偶尔为 0 或 3。对于 $0 < p < 1$ 和 $p < 0$，也能得到类似的结果。

---

## 第 64 页

对于几乎所有有实用价值的 $p$ 值而言，这种 SDP（semidefinite program，半定规划）表示法的代价都相对较小。尽管如此，只要 $k(pn,pd) > 10$，cvx 就会发出警告，以确保用户不会对任何意外的计算减速感到惊讶。如果这个阈值不适合你，你可以使用命令 `cvx_power_warning(thresh)` 来修改它，其中 `thresh` 就是你想要的截断值。把阈值设为 `Inf` 可以完全禁用该警告。

与命令 `cvx_precision` 一样，你可以在模型内部放置一次对 `cvx_power_warning` 的调用，从而只为单个模型修改阈值；也可以把调用放在模型之外，从而作出全局修改。该命令总是返回阈值的**前一个值**，因此你可以保存它，并在模型完成后（如你所愿地）恢复它。你可以通过不带参数地调用 `cvx_power_warning` 来查询当前阈值。

### D.3 Overdetermined problems（超定问题）

这个状态消息 `Overdetermined` 通常出现在变量或集合的某种结构没有被正确识别的时候。例如，考虑这样一个问题：给定矩阵 $W \in \mathbb{R}^{n\times n}$，找出能使它变为半正定的最小的对角加项：

```math
\begin{aligned}
& \text{minimize} && \operatorname{Trace}\, D \\
& \text{subject to} && W + D \succeq 0 \\
& && D \text{ diagonal}
\end{aligned}
\tag{16}
```

在 cvx 中，这个问题可以如下表达：

```matlab
n = size(W,1);
cvx_begin
    variable D(n,n) diagonal;
    minimize( trace( D ) );
    subject to
        W + D == semidefinite(n);
cvx_end
```

如果把这个设定应用到矩阵 `W=randn(5,5)` 上，就会发出如下警告：

```
Warning: Overdetermined equality constraints;
problem is likely infeasible.
```

（中文解释：警告——等式约束超定；问题很可能不可行。）同时变量 `cvx_status` 会被设为 `Overdetermined`。

这里发生的情况是：语句 `semidefinite(n)` 返回的那个无名变量是对称的，而 `W` 是固定的、非对称的。因此，按题面陈述，这个问题是不可行的（infeasible）。不过，这里还存在 $n^2$ 个等式约束，却只有 $n + n\cdot(n+1)/2$ 个独立的自由度——所以这个问题是超定的（overdetermined）。

下面这个修正版的设定通过提取 `W` 的对称部分来纠正该问题：

```matlab
n = size(W,1);
cvx_begin
    variable D(n,n) diagonal;
```

（第 64 页在此结束，代码于第 65 页续。）

---

## 第 65 页

```matlab
    minimize( trace( D ) );
    subject to
        0.5 * ( W + W' ) + D == semidefinite(n);
cvx_end
```

对应关系说明：`minimize( trace( D ) )` 对应目标函数 $\operatorname{Trace}\, D$；约束行 `0.5 * ( W + W' ) + D == semidefinite(n)` 对应数学约束 $W + D \succeq 0$——这里把 $W$ 替换成了它的对称部分 $\tfrac12(W + W^{\top})$，从而保证等式两侧都是对称矩阵，`== semidefinite(n)` 表示半正定约束。`D` 的 `diagonal` 结构在 `variable D(n,n) diagonal;` 中声明，对应约束“$D$ 为对角矩阵”。

## E Acknowledgements（致谢）

我们谨向以下为 cvx 的开发作出贡献的人士表示感谢：Toh Kim Chuan、Laurent El Ghaoui、Arpita Ghosh、Siddharth Joshi、Johan Löfberg、Almir Mutapcic、Michael Overton 及其学生、Art Owen、Rahul Panicker、Imre Polik、Joëlle Skaf、Lieven Vandenberghe、Argyris Zymnis。我们同样感谢多所大学中（也许是无意间）通过在课程作业里使用 cvx 而充当 beta 测试者的众多学生。我们感谢 Igal Sason 找出了本文档早期版本中的许多笔误，并总体上帮助提高了文档的清晰度。

---

## 第 66 页

## References（参考文献）

- **[AG01]** F. Alizadeh and D. Goldfarb. *Second-order cone programming.* Technical Report RRR 51-2001, RUTCOR, Rutgers University, November 2001. Available at http://rutcor.rutgers.edu/pub/rrr/reports2001/51.ps.
- **[BKMR98]** A. Brooke, D. Kendrick, A. Meeraus, and R. Raman. *GAMS: A User's Guide.* The Scientific Press, South San Francisco, 1998. Available at http://www.gams.com/docs/gams/GAMSUsersGuide.pdf.
- **[BKVH05]** S. Boyd, S. J. Kim, L. Vandenberghe, and A. Hassibi. *A tutorial on geometric programming.* Optimization and Engineering, 2005. Available at http://www.stanford.edu/~boyd/gp_tutorial.html.
- **[BV04]** S. Boyd and L. Vandenberghe. *Convex Optimization.* Cambridge University Press, 2004. Available at http://www.stanford.edu/~boyd/cvxbook.html.
- **[Cru02]** C. Crusius. *A Parser/Solver for Convex Optimization Problems.* PhD thesis, Stanford University, 2002.
- **[DV05]** J. Dahl and L. Vandenberghe. *CVXOPT: A Python Package for Convex Optimization.* Available at http://www.ee.ucla.edu/~vandenbe/cvxopt, 2005.
- **[FGK99]** R. Fourer, D. Gay, and B. Kernighan. *AMPL: A Modeling Language for Mathematical Programming.* Duxbury Press, December 1999.
- **[GBY06]** M. Grant, S. Boyd, and Y. Ye. *Disciplined convex programming.* In L. Liberti and N. Maculan, editors, *Global Optimization: from Theory to Implementation*, Nonconvex Optimization and Its Applications, pages 155–210. Springer, New York, 2006. Available at http://www.stanford.edu/~boyd/disc_cvx_prog.html.
- **[Gra04]** M. Grant. *Disciplined Convex Programming.* PhD thesis, Department of Electrical Engineering, Stanford University, December 2004. See http://www.stanford.edu/~boyd/disc_cvx_prog.html.
- **[Löf05]** J. Löfberg. *YALMIP version 3 (software package).* http://control.ee.ethz.ch/~joloef/yalmip.php, September 2005.
- **[Mat04]** The MathWorks, Inc. *MATLAB (software package).* http://www.mathworks.com, 2004.
- **[Mat05]** The MathWorks, Inc. *MATLAB optimization toolbox (software package).* http://www.mathworks.com/products/optimization/, 2005.
- **[MOS05]** MOSEK ApS. *Mosek (software package).* http://www.mosek.com, February 2005.

---

## 第 67 页

- **[Owe06]** A. Owen. *A robust hybrid of lasso and ridge regression.* Technical report, Department of Statistics, Stanford University, October 2006. Author's internal draft.
- **[Stu99]** J. Sturm. *Using SeDuMi 1.02, a MATLAB toolbox for optimization over symmetric cones.* Optimization Methods and Software, 11:625–653, 1999. Software available at http://sedumi.mcmaster.ca/.
- **[TTT06]** K. Toh, R. Tütüncü, and M. Todd. *SDPT3 4.0 (beta) (software package).* http://www.math.nus.edu.sg/~mattohkc/sdpt3.html, July 2006.
- **[WB00]** S.-P. Wu and S. Boyd. *SDPSOL: A parser/solver for semidefinite programs with matrix structure.* In L. El Ghaoui and S.-I. Niculescu, editors, *Recent Advances in LMI Methods for Control*, chapter 4, pages 79–91. SIAM, 2000. Available at http://www.stanford.edu/~boyd/sdpsol.html.

---

## 第 68 页

## Index（索引）

| 英文术语 | 中文译名 | 页码 |
|---|---|---|
| affine function | 仿射函数 | 28 |
| AMPL | AMPL | 4 |
| array variable | 数组变量 | 17 |
| assignment | 赋值 | 46 |
| banded matrix variable | 带状矩阵变量 | 17 |
| bug report | 缺陷报告 | 6 |
| complex variable | 复数变量 | 17 |
| composition | 复合 | 29 |
| concave function | 凹函数 | 28 |
| constraint | 约束 | 11, 13, 18 |
| constraint bounds | 约束边界 | 11, 13, 18 |
| equality constraint | 等式约束 | 14, 18, 46 |
| inequality constraint | 不等式约束 | 14, 18 |
| nonconvex constraint | 非凸约束 | 14, 16 |
| constraint set | 约束集 | 19 |
| convex function | 凸函数 | 28 |
| cvx | cvx | 4 |
| cvx functions | cvx 函数 | 53 |
| installing cvx | 安装 cvx | 50 |
| cvx operators | cvx 运算符 | 53 |
| cvx precision | cvx 精度 | 61 |
| cvx status messages | cvx 状态消息 | 61 |
| cvx begin | cvx begin | 8 |
| cvx clear | cvx clear | 46 |
| cvx end | cvx end | 8 |
| cvx expert | cvx expert | 63 |
| cvx optval | cvx optval | 10, 18 |
| cvx pause | cvx pause | 46 |
| cvx power warning | cvx power warning | 63 |
| cvx precision | cvx precision | 44 |
| cvx problem | cvx problem | 46 |
| cvx quiet | cvx quiet | 46 |
| cvx solver | cvx solver | 44 |
| cvx status | cvx status | 10 |
| cvx version | cvx version | 46 |
| cvx where | cvx where | 46 |
| CVXOPT | CVXOPT | 5 |
| DCP | DCP | 4, 5 |
| DCP ruleset | DCP 规则集 | 5, 25 |
| deadzone function | 死区函数 | 34 |
| defining new function | 定义新函数 | 34 |
| diagonal matrix variable | 对角矩阵变量 | 17 |
| disciplined convex programming | 规则化凸规划（见 DCP） | 4, 5 |
| dual variable | 对偶变量 | 21 |
| epigraph | 上镜图 | 35 |
| equality constraint | 等式约束 | 46 |
| examples | 示例 | 8 |
| expression | 表达式 | 23 |
| expression holder | 表达式持有者 | 23 |
| expressions | 表达式（复数） | 23 |
| feasibility | 可行性 | 18, 26 |
| feedback | 反馈 | 6 |
| function | 函数 | 19 |
| cvx library function | cvx 库函数 | 53 |
| affine function | 仿射函数 | 28 |
| function composition | 函数复合 | 29 |
| concave function | 凹函数 | 28 |
| convex function | 凸函数 | 28 |
| deadzone function | 死区函数 | 34 |
| defining new function | 定义新函数 | 34 |
| epigraph function | 上镜图函数 | 35 |
| generalized posynomial | 广义正项式 | 41 |
| Huber function | Huber 函数 | 35 |
| hypograph function | 下镜图函数 | 36 |
| monomial | 单项式 | 41 |
| monotonicity | 单调性 | 28 |
| objective function | 目标函数 | 9 |
| posynomial | 正项式 | 41 |
| quadratic function | 二次函数 | 32 |
| GAMS | GAMS | 4 |
| generalized geometric programming | 广义几何规划（见 GGP） | — |
| geometric program | 几何规划（见 GP） | — |
| geometric programming mode | 几何规划模式 | 41 |
| GGP | GGP | 41 |
| GP | GP | 4, 41 |
| GP mode | GP 模式 | 4, 41 |

---

## 第 69 页

| 英文术语 | 中文译名 | 页码 |
|---|---|---|
| Hermitian matrix variable | Hermitian 矩阵变量 | 17 |
| Huber function | Huber 函数 | 35 |
| hypograph | 下镜图 | 36 |
| inequality constraint | 不等式约束 | 18 |
| matrix inequality | 矩阵不等式 | 39, 40 |
| infeasible problem | 不可行问题 | 22 |
| installing cvx | 安装 cvx | 50 |
| Lagrange multiplier | 拉格朗日乘子 | 21 |
| least squares | 最小二乘 | 8 |
| linear matrix inequality | 线性矩阵不等式（见 LMI） | — |
| linear program | 线性规划（见 LP） | — |
| linprog | linprog | 11 |
| LMI | LMI | 39 |
| Lorentz cone | Lorentz 锥（见 second-order cone） | — |
| LP | LP | 4, 5 |
| Matlab | Matlab | 4 |
| maximize | maximize | 10, 18 |
| minimize | minimize | 9, 18 |
| mode GP | GP 模式 | 41 |
| mode SDP | SDP 模式 | 39 |
| monotonicity | 单调性 | 28 |
| MOSEK | MOSEK | 5 |
| nondecreasing | 非减 | 28 |
| nonincreasing | 非增 | 28 |
| norm | 范数 | 9 |
| norm(·,1) | 1-范数 | 12 |
| norm(·,Inf) | 无穷范数 | 11, 14 |
| objective function | 目标函数 | 9, 18 |
| nonconvex objective | 非凸目标 | 10 |
| operators | 运算符 | 53 |
| suppressing output | 抑制输出 | 46 |
| overdetermined problem | 超定问题 | 64 |
| platforms | 平台 | 50 |
| posynomial | 正项式 | 41 |
| powers | 幂 | 63 |
| precision | 精度 | 61 |
| changing precision | 修改精度 | 44 |
| dual problem | 对偶问题 | 21 |
| feasibility problem | 可行性问题 | 18, 26 |
| infeasible problem | 不可行问题 | 22 |
| overdetermined problem | 超定问题 | 64 |
| python | python | 4 |
| QP | QP | 4 |
| quadratic form | 二次型 | 32 |
| quadratic program | 二次规划（见 QP） | — |
| SDP | SDP | 4, 5, 19, 39 |
| SDP mode | SDP 模式 | 4, 39 |
| SDPT3 | SDPT3 | 5, 44 |
| second-order cone | 二阶锥 | 20 |
| second-order cone program | 二阶锥规划（见 SOCP） | — |
| SeDuMi | SeDuMi | 5, 44 |
| semidefinite | 半定 | 19 |
| semidefinite program | 半定规划（见 SDP） | — |
| semidefinite programming mode | 半定规划模式 | 39 |
| set | 集合 | 19 |
| SOCP | SOCP | 4, 5, 20 |
| changing solver | 修改求解器 | 44 |
| solver status messages | 求解器状态消息 | 61 |
| successive approximation | 逐次逼近 | 63 |
| symmetric matrix variable | 对称矩阵变量 | 17 |
| Toeplitz matrix variable | Toeplitz 矩阵变量 | 17 |
| trade-off curve | 折中曲线 | 15 |
| array variable | 数组变量 | 17 |
| banded matrix variable | 带状矩阵变量 | 17 |
| complex variable | 复数变量 | 17 |
| declaring variable | 声明变量 | 17 |
| diagonal matrix variable | 对角矩阵变量 | 17 |
| Hermitian matrix variable | Hermitian 矩阵变量 | 17 |
| variable structure | 变量结构 | 17 |
| symmetric matrix variable | 对称矩阵变量 | 17 |
| Toeplitz matrix variable | Toeplitz 矩阵变量 | 17 |
| variable | 变量 | 9, 17 |
| variables | 变量（复数） | 17 |
| version information | 版本信息 | 5 |
| YALMIP | YALMIP | 4 |
