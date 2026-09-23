# <span style="color:#c0392b;">Boyd《凸优化》第 1 章 引言（Convex Optimization, Boyd & Vandenberghe）</span>

> <span style="color:#7f8c8d;">Source: 教材/Convex Optimization (Boyd, Vandenberghe).pdf · Chapter 1, pp.1–17 (PDF p.15–32) · 行内双语翻译版 · 译者：ZCode（规范：bilingual-translator 铁律，禁止英中分段对照）</span>

在本章引言中，我们对 mathematical optimization（数学优化）做一个概览，重点是 convex optimization（凸优化）的特殊地位。这里非正式引入的概念将在后续章节中以更严谨、更细致的方式展开。

## <span style="color:#c0392b;">1.1 Mathematical optimization（数学优化）</span>

<!-- p.15 -->

一个 mathematical optimization problem（数学优化问题，或简称 optimization problem，优化问题）具有如下形式：

$$
\text{minimize}\quad f_0(x) \qquad \text{subject to}\quad f_i(x) \le b_i,\; i = 1,\dots,m. \tag{1.1}
$$

这里向量 $x = (x_1,\dots,x_n)$ 是问题的 optimization variable（优化变量），函数 $f_0:\mathbb{R}^n \to \mathbb{R}$ 是 objective function（目标函数），函数 $f_i:\mathbb{R}^n\to\mathbb{R},\; i=1,\dots,m$ 是（不等式）constraint functions（约束函数），常数 $b_1,\dots,b_m$ 是约束的 limits（限制）或 bounds（界）。如果一个向量 $x^\star$ 在所有满足约束的向量中具有最小的目标值，就称它为 optimal（最优的），或问题 (1.1) 的 solution（解）：即对任意满足 $f_1(z)\le b_1,\dots,f_m(z)\le b_m$ 的 $z$，都有 $f_0(z)\ge f_0(x^\star)$。

我们通常研究的是以目标函数和约束函数的特定形式为特征的一族或一类优化问题。一个重要的例子：若 (1.1) 中目标函数与约束函数 $f_0,\dots,f_m$ 都是 linear（线性的），即满足

$$
f_i(\alpha x + \beta y) = \alpha f_i(x) + \beta f_i(y) \tag{1.2}
$$

对所有 $x, y\in\mathbb{R}^n$ 与所有 $\alpha,\beta\in\mathbb{R}$ 成立，则该优化问题称为 linear program（线性规划）。若优化问题不是线性的，则称为 nonlinear program（非线性规划）。

<!-- p.16 -->

本书讨论的是一类称为 convex optimization problems（凸优化问题）的优化问题。凸优化问题是指目标函数与约束函数都是 convex（凸）的问题，也就是说它们满足不等式

$$
f_i(\alpha x + \beta y) \le \alpha f_i(x) + \beta f_i(y)
$$

对所有 $x, y\in\mathbb{R}^n$ 以及满足 $\alpha+\beta=1,\;\alpha\ge 0,\;\beta\ge 0$ 的 $\alpha,\beta\in\mathbb{R}$ 成立。比较 (1.3) 与 (1.2) 可以看到：convexity（凸性）比 linearity（线性）更一般——不等式取代了限制更强的等式，而且不等式只须对特定的 $\alpha,\beta$ 取值成立。既然任何 linear program 都是凸优化问题，我们可以把 convex optimization（凸优化）看作 linear programming（线性规划）的推广。

> <span style="color:#2471a3;">**[译者注｜跨课程连接]** 这里的 (1.2) 正是你在 18.065/线性代数中见过的「线性映射」定义；而 (1.3) 的凸组合不等式与 CLRS 中 J(u) 对应 CSAPP 中「一次编写随处运行」的可靠性思想同构：两者都是用「可预期的性质」换取「可自动化」。把「凸 = 可以放心交给算法」记在心里，后面每一章都在为此背书。</span>

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">1.1.1 Applications（应用）</span>

优化问题 (1.1) 是「从 $\mathbb{R}^n$ 中一组候选选择里做出最优决策」这一问题的抽象。变量 $x$ 代表所做的选择；约束 $f_i(x)\le b_i$ 代表限制可能选择的硬性要求或规格；目标值 $f_0(x)$ 代表选择 $x$ 的 cost（代价）。（我们也可以把 $-f_0(x)$ 看作选择 $x$ 的 value（价值）或 utility（效用）。）优化问题 (1.1) 的解，对应的就是在所有满足硬性要求的选择中代价最小（或效用最大）的那个选择。

例如在 portfolio optimization（投资组合优化）中，我们要把一笔资金以最佳方式投资到 $n$ 种资产上。变量 $x_i$ 表示对第 $i$ 种资产的投资额，于是向量 $x\in\mathbb{R}^n$ 描述了整个投资组合在资产集合上的 allocation（配置）。约束可以表示：预算上限（即总投资额的限制）、投资额非负的要求（假设不允许 short positions（空头头寸））、以及整个组合 expected return（期望收益）的最低可接受值。目标函数（成本函数）可以是组合收益的整体 risk（风险）或 variance（方差）的度量。此时优化问题 (1.1) 对应的就是：在所有满足硬性要求的配置中，选择风险最小的投资组合配置。

另一个例子是电子设计中的 device sizing（器件尺寸设计），即为电路中每个器件选择宽度与长度的任务。这里变量表示各器件的宽度和长度；约束表示各种工程要求，例如制造工艺对器件尺寸的限制、保证电路能在指定速度下可靠运行的 timing requirements（时序要求），以及电路总面积的上限。器件尺寸设计中一个常见目标函数是电路消耗的 total power（总功耗）。于是优化问题 (1.1) 就是：找到既满足设计要求（可制造性、时序、面积），又最省电的器件尺寸。

在 data fitting（数据拟合）中，任务是从一族候选模型中找到最能拟合观测数据与先验信息的模型。这里变量是模型中的 parameters（参数），约束可以表示先验信息或对参数的必要限制（如非负性）。目标函数可以是观测数据与模型预测值之间的 misfit（失配）或 prediction error（预测误差）的度量，也可以是对参数值不可能性/不合理性的统计度量。优化问题 (1.1) 就是：找到与先验信息一致、且对观测数据失配或预测误差最小（或在统计框架下最有可能）的模型参数值。

<!-- p.17 -->

大量涉及决策（或系统设计、分析与运行）的实际问题，都可以归结为数学优化问题或其某种变体（例如多准则优化问题）。事实上，数学优化已成为许多领域的重要工具：它广泛用于工程领域——电子设计自动化、自动控制系统，以及土木、化工、机械与航空航天工程中的最优设计问题；也用于网络设计与运行、金融、供应链管理、调度等许多其他领域的问题。应用清单仍在稳步增长。

对上述大多数应用而言，数学优化是作为人类决策者、系统设计者或系统操作者的辅助工具：由人来监督过程、检查结果，并在必要时修改问题（或求解方法）。这位人类决策者还执行优化问题所建议的行动，例如买卖资产以实现最优投资组合。

一个较新的现象为数学优化开辟了许多其他应用可能：随着嵌入产品的计算机的激增，embedded optimization（嵌入式优化）快速增长。在这类嵌入式应用中，优化被用来自动做出实时选择，甚至执行相应行动，几乎没有（或完全没有）人工干预或监督。在某些应用领域，传统自动控制系统与嵌入式优化的融合已在顺利进行；在另一些领域才刚刚起步。嵌入式实时优化带来一些新挑战：特别是它要求求解方法极其可靠，并且能在可预测的时间（和内存）内解决问题。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">1.1.2 Solving optimization problems（求解优化问题）</span>

<!-- p.18 -->

某一类优化问题的 solution method（求解方法）是一个 algorithm（算法）：给定该类中的一个具体问题（即问题的一个 instance（实例）），它计算出该问题的（在给定精度下的）解。自 1940 年代末以来，人们投入大量精力去开发求解各类优化问题的算法、分析其性质、并给出良好的软件实现。这些算法的有效性——即我们求解优化问题 (1.1) 的能力——差异很大，取决于诸如目标函数与约束函数的具体形式、变量与约束的数量，以及特殊结构（例如 sparsity（稀疏性）：若每个约束函数只依赖于少量变量，则称问题是稀疏的）。

即使目标函数和约束函数都是 smooth（光滑）的（例如多项式），一般优化问题 (1.1) 的求解也出奇地困难。因此求解一般问题的方法都要做某种妥协：要么计算时间极长，要么可能找不到解。其中一些方法将在 §1.4 讨论。

然而，「大多数优化问题都难解」这条一般规律存在一些重要的例外。对少数问题类，我们拥有

<!-- p.18/19 -->

即使面对有数百甚至数千个变量和约束的大型问题也能可靠求解的有效算法。§1.2 描述的两个著名例子——least-squares problems（最小二乘问题）与 linear programs（线性规划）——就是如此。而一个鲜为人知的事实是：凸优化是这条规律的另一个例外。与最小二乘或线性规划一样，存在非常有效的算法，能够可靠而高效地求解即使是大型凸问题。

## <span style="color:#c0392b;">1.2 Least-squares and linear programming（最小二乘与线性规划）</span>

本节介绍两个广为人知、广泛使用的凸优化子类：least-squares（最小二乘）与 linear programming（线性规划）。（对这些问题的完整技术处理将在第 4 章给出。）

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">1.2.1 Least-squares problems（最小二乘问题）</span>

<!-- p.19 -->

least-squares problem（最小二乘问题）是一个无约束（即 $m=0$）的优化问题，其目标是形如 $a_i^Tx-b_i$ 的各term（项）的平方和：

$$
\text{minimize}\quad f_0(x) = \|Ax-b\|_2^2 = \sum_{i=1}^k (a_i^T x - b_i)^2. \tag{1.4}
$$

这里 $A\in\mathbb{R}^{k\times n}$（$k\ge n$），$a_i^T$ 是 $A$ 的各行，向量 $x\in\mathbb{R}^n$ 是优化变量。

**求解最小二乘问题。** 最小二乘问题 (1.4) 的求解可以归结为解一组线性方程

$$
(A^TA)x = A^Tb,
$$

于是有解析解 $x = (A^TA)^{-1}A^Tb$。对最小二乘问题，我们有能把问题解到高精度、可靠性非常高的好算法（和软件实现）。求解时间近似正比于 $n^2k$，且比例常数已知。当前的台式计算机可以在几秒内求解数百个变量、数千个项的最小二乘问题；更强大的计算机当然能更快地求解更大的问题或同样规模的问题。（而且按 Moore's law（摩尔定律），这些求解时间在未来会呈指数下降。）最小二乘的算法与软件已经可靠到可以用于嵌入式优化。

<!-- p.19/20 -->

许多情况下，通过利用系数矩阵 $A$ 的某种特殊结构，我们能求解甚至更大的最小二乘问题。例如设矩阵 $A$ 是 sparse（稀疏）的，即它的非零元远少于 $kn$ 个。利用稀疏性，通常能以远低于 $n^2k$ 阶的时间求解。当前台式计算机可以在一分钟左右求解数万个变量、数十万个项的稀疏最小二乘问题（尽管这取决于具体的稀疏模式）。

对于极大规模的问题（比如数百万个变量）或有苛刻实时计算要求的问题，求解最小二乘仍具挑战。但在绝大多数情况下，可以说现有方法非常有效且极其可靠。事实上，我们可以说求解最小二乘问题（未触及当前能力边界时）是一项 (mature) technology（「成熟」的技术），可以被许多不了解、也不需要了解细节的人可靠地使用。

**使用最小二乘。** 最小二乘问题是 regression analysis（回归分析）、optimal control（最优控制）以及许多 parameter estimation（参数估计）与数据拟合方法的基础。它有多种统计解释，例如：在受 Gaussian measurement errors（高斯测量误差）污染的线性测量下，对向量 $x$ 做 maximum likelihood estimation（最大似然估计）。

识别一个优化问题是不是最小二乘问题很直接：只须验证目标是 quadratic function（二次函数），再检验相应的 quadratic form（二次型）是否 positive semidefinite（半正定）。

虽然基本最小二乘问题有简单固定的形式，但应用中有若干标准技术可以增加其灵活性。在 weighted least-squares（加权最小二乘）中，最小化的是加权代价

$$
\sum_{i=1}^k w_i (a_i^T x - b_i)^2,
$$

其中 $w_1,\dots,w_k$ 为正数。（这个问题很容易转化并作为标准最小二乘问题求解。）权重 $w_i$ 的选取用来反映对各项 $a_i^Tx-b_i$ 大小的不同关注程度，或者仅仅是为了影响解。在统计背景下，当线性测量的误差具有不相等的 variances（方差）时，加权最小二乘自然出现。

<!-- p.20 -->

最小二乘的另一项技术是 regularization（正则化），即在代价函数中加入额外的项。最简单的情形是把变量的平方和乘以一个正常数后加入代价函数：

$$
\sum_{i=1}^k (a_i^T x - b_i)^2 + \rho \sum_{i=1}^n x_i^2,
$$

其中 $\rho > 0$。（这个问题同样可以表述为标准最小二乘问题。）额外的项惩罚 $x$ 的过大取值，使得在只最小化第一个和会得到不合理结果的情形下，仍能得到 sensible solution（合乎情理的解）。参数 $\rho$ 由使用者选择，以便在「让原目标函数 $\sum_{i=1}^k(a_i^Tx-b_i)^2$ 小」与「让 $\sum_{i=1}^n x_i^2$ 不至于太大」之间取得正确的 trade-off（权衡）。当被估计的向量 $x$ 具有先验分布时，正则化就会在统计估计中出现。

加权最小二乘与正则化将在第 6 章讲述；它们的统计解释在第 7 章给出。

> <span style="color:#2471a3;">**[译者注｜跨课程连接]** 正则化项 $\rho\sum x_i^2$ 与你在大二概率论（6.431）里会遇到的 MAP 估计直接对应：加上 Gaussian 先验的最小二乘就是 ridge regression（岭回归）。「先验分布 → 代价函数中的额外项」这一对应是贝叶斯视角与优化视角的第一座桥。</span>

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">1.2.2 Linear programming（线性规划）</span>

<!-- p.20/21 -->

另一类重要的优化问题是 linear programming（线性规划），其目标函数与所有约束函数都是线性的：

$$
\text{minimize}\quad c^T x \qquad \text{subject to}\quad a_i^T x \le b_i,\; i=1,\dots,m. \tag{1.5}
$$

这里向量 $c, a_1,\dots,a_m\in\mathbb{R}^n$ 与标量 $b_1,\dots,b_m\in\mathbb{R}$ 是规定目标函数与约束函数的 problem parameters（问题参数）。

**求解线性规划。** 线性规划的解没有简单的解析公式（不像最小二乘），但有很多非常有效的求解方法，包括 Dantzig 的 simplex method（单纯形法），以及本书后面讲述的较新的 interior-point methods（内点法）。虽然我们无法像最小二乘那样给出求解所需的精确算术运算数，但可以建立严格的 bounds（界）：用内点法把线性规划解到给定精度所需的运算次数界。实际复杂度约为 $n^2m$ 阶（设 $m\ge n$），但其常数不如最小二乘刻画得那么清楚。这些算法相当可靠，尽管可能不如最小二乘的方法。我们可以在小型台式计算机上、几秒内轻松求解数百个变量、数千个约束的问题。如果问题稀疏或有其他可利用的结构，常常能求解数万乃至数十万个变量与约束的问题。

与最小二乘一样，求解极大规模的线性规划、或满足苛刻实时计算要求的线性规划仍具挑战。但和最小二乘一样，我们可以说求解（大多数）线性规划是一项成熟技术。Linear programming solvers（线性规划求解器）可以（并且已经）被嵌入许多工具与应用中。

**使用线性规划。** 有些应用直接导出形如 (1.5) 的线性规划或其几个标准变体。在许多其他情形，原始优化问题并不具有标准线性规划形式，但可以用第 4 章详述的技术把它变换成一个等价的线性规划（然后当然可以求解）。

<!-- p.21 -->

举一个简单例子：Chebyshev approximation problem（切比雪夫逼近问题）：

$$
\text{minimize}\quad \max_{i=1,\dots,k} |a_i^T x - b_i|. \tag{1.6}
$$

这里 $x\in\mathbb{R}^n$ 是变量，$a_1,\dots,a_k\in\mathbb{R}^n$、$b_1,\dots,b_k\in\mathbb{R}$ 是规定问题实例的参数。注意它与最小二乘问题 (1.4) 的相似之处：两个问题的目标都是度量 $a_i^Tx-b_i$ 这些项的大小。最小二乘用各项的平方和作目标，而切比雪夫逼近用绝对值的 maximum（最大值）。

另一个重要区别是：切比雪夫逼近问题 (1.6) 的目标函数不是 differentiable（可微）的；而最小二乘问题 (1.4) 的目标是二次的，因而可微。

切比雪夫逼近问题 (1.6) 可以通过求解线性规划

$$
\begin{aligned}
\text{minimize}\quad & t\\
\text{subject to}\quad & a_i^T x - t \le b_i, && i = 1,\dots,k\\
& -a_i^T x - t \le -b_i, && i = 1,\dots,k,
\end{aligned}
\tag{1.7}
$$

来求解，其中变量为 $x\in\mathbb{R}^n$ 与 $t\in\mathbb{R}$。（细节将在第 6 章给出。）由于线性规划很容易求解，切比雪夫逼近问题也就容易求解了。

任何具备线性规划实用知识的人都会看出切比雪夫逼近问题 (1.6) 可以归结为线性规划。但对没有这种背景的人来说，带不可微目标的 (1.6) 能被表述并求解为线性规划，可能并不显然。

<!-- p.22 -->

识别可归结为线性规划的问题，比识别最小二乘问题要复杂，但这是一项很容易习得的技能，因为用到的只是少数几个标准技巧。这项任务甚至可以部分自动化：一些用于描述和求解优化问题的软件系统能自动识别（某些）可以重新表述为线性规划的问题。

## <span style="color:#c0392b;">1.3 Convex optimization（凸优化）</span>

convex optimization problem（凸优化问题）是如下形式的问题：

$$
\text{minimize}\quad f_0(x) \qquad \text{subject to}\quad f_i(x)\le b_i,\; i=1,\dots,m, \tag{1.8}
$$

其中函数 $f_0,\dots,f_m:\mathbb{R}^n\to\mathbb{R}$ 都是 convex（凸）的，即满足

$$
f_i(\alpha x + \beta y) \le \alpha f_i(x) + \beta f_i(y)
$$

对所有 $x,y\in\mathbb{R}^n$ 与满足 $\alpha+\beta=1,\;\alpha\ge0,\;\beta\ge0$ 的 $\alpha,\beta\in\mathbb{R}$ 成立。最小二乘问题 (1.4) 与线性规划问题 (1.5) 都是一般凸优化问题 (1.8) 的特例。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">1.3.1 Solving convex optimization problems（求解凸优化问题）</span>

<!-- p.22/23 -->

凸优化问题的解一般没有解析公式，但（与线性规划一样）有非常有效的求解方法。Interior-point methods（内点法）在实践中表现非常好，某些情形下还能证明：可以用不超过问题维度多项式数量级的运算次数把问题解到指定精度。（这将在第 11 章讲述。）

我们将会看到，内点法求解问题 (1.8) 所需的步数或 iteration（迭代）次数几乎总在 10 到 100 之间。若忽略问题中的任何结构（如稀疏性），每一步需要

$$
\max\{n^3,\; n^2 m,\; F\}
$$

阶的运算，其中 $F$ 是计算目标与约束函数 $f_0,\dots,f_m$ 的一阶与二阶导数的代价。

<!-- p.23 -->

与求解线性规划的方法一样，这些内点法相当可靠。当前台式计算机上，我们可以在至多几十秒内轻松求解数百个变量、数千个约束的问题。利用问题结构（如稀疏性），可以求解远更大的、有成千上万个变量与约束的问题。

我们还不能宣称求解一般凸优化问题已是像求解最小二乘或线性规划那样的成熟技术。针对一般非线性凸优化的内点法研究仍是非常活跃的领域，关于什么是最佳方法尚无共识。但可以合理预期，求解一般凸优化问题将在几年内成为一种技术。而对某些凸优化子类——例如 second-order cone programming（二阶锥规划）或 geometric programming（几何规划）（第 4 章详述）——可以说内点法已接近成为技术。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">1.3.2 Using convex optimization（使用凸优化）</span>

<!-- p.23/24 -->

使用凸优化（至少在概念上）非常像使用最小二乘或线性规划。如果我们能把一个问题表述为凸优化问题，就能高效地求解它，正如我们能高效求解最小二乘问题一样。稍微夸张一点说：如果你把一个实际问题表述成了凸优化问题，那么你已经解决了原来的问题。

当然也存在一些重要差异。识别最小二乘问题很容易，但识别 convex function（凸函数）可能很难。此外，变换凸问题的技巧比变换线性规划的技巧多得多。因此，识别凸优化问题（或可变换为凸优化问题的问题）可能很有挑战性。本书的主要目标，正是让读者具备完成这件事所需的背景知识。

<!-- p.24 -->

一旦发展出识别或表述凸优化问题的技能，你会发现多得惊人的问题都可以通过凸优化求解。

使用凸优化的挑战与艺术，正在于识别与表述问题。表述一旦完成，求解问题就像最小二乘或线性规划一样，（几乎）成了技术。

> <span style="color:#2471a3;">**[译者注｜第一性原理]** 本章最重要的主线是「技术 vs 艺术」的分界线：最小二乘/线性规划 = 技术（机械流程可复用）；局部优化 = 艺术（依赖初始点、调参、运气）；凸优化 = 正从艺术走向技术，其难点唯一地集中在 formulation（建模）环节。这也是本书后面 400 多页内容的定位——把「识别凸性」从艺术变成可教学的技能。</span>

## <span style="color:#c0392b;">1.4 Nonlinear optimization（非线性优化）</span>

<!-- p.24 -->

Nonlinear optimization（非线性优化，或 nonlinear programming，非线性规划）一词描述的是目标函数或约束函数不是线性、但也不已知为凸的优化问题。遗憾的是，求解一般非线性规划问题 (1.1) 并没有有效方法。哪怕只有十个变量的、看起来简单的问题也可能极具挑战，而有几百个变量的问题可能完全 intractable（难以处理）。因此，一般非线性规划的方法采取了几种不同的路线，每种都包含某种妥协。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">1.4.1 Local optimization（局部优化）</span>

<!-- p.24/25 -->

在 local optimization（局部优化）中，妥协是放弃寻找在所有可行点上最小化目标的 optimal $x$（最优解），转而寻找一个 merely locally optimal（仅局部最优）的点：它在「邻近的可行点」中使目标函数最小，但不保证比所有其他可行点的目标值更低。一般非线性规划研究中有很大比例集中在局部优化方法上，因此这些方法发展得很成熟。

局部优化方法可以很快、能处理大规模问题、并且适用面广，因为它们只要求目标与约束函数可微。因此，在「找到一个好点（即使不是最好的点）也有价值」的应用中，局部优化方法被广泛使用。例如在工程设计应用中，局部优化可用来改进最初由手工（或其他）设计方法得到的设计。

<!-- p.25 -->

局部优化方法除（可能）找不到真正的 globally optimal solution（全局最优解）之外，还有若干缺点：方法需要对优化变量有 initial guess（初始猜测）；这个初始点或 starting point（起始点）至关重要，会极大影响所得局部解的目标值；对于局部解距离（全局）最优有多远，它几乎不提供信息；局部优化方法常常对算法参数值敏感，参数可能需要针对具体问题或问题族来调整。

使用局部优化方法比求解最小二乘问题、线性规划或凸优化问题更棘手：它包括尝试选择算法、调整算法参数、以及（在求解单个实例时）找到一个足够好的初始点，或（在求解一族问题时）设计产生足够好初始点的方法。粗略地说，局部优化方法与其说是技术，不如说是艺术（more art than technology）。局部优化是一门发展成熟的「艺术」，常常非常有效，但它终究是艺术。相比之下，求解最小二乘问题或线性规划几乎不涉及「艺术」（当然，触及当前能力边界的问题除外）。

<!-- p.25/26 -->

可以在非线性规划的局部优化方法与凸优化之间做一个有趣的比较。既然目标与约束函数的可微性是大多数局部优化方法的唯一要求，把实际问题表述为非线性优化问题相对直接：局部优化中的「艺术」在于表述完成之后去求解问题（以「找到局部最优点」这一弱化的意义）。而在凸优化中这两者是颠倒的：艺术与挑战在于问题表述；一旦把问题表述为凸优化问题，求解它就相对直接了。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">1.4.2 Global optimization（全局优化）</span>

在 global optimization（全局优化）中，找到的是优化问题 (1.1) 的真正的全局解；妥协是效率。全局优化方法的 worst-case complexity（最坏情况复杂度）随问题规模 $n$ 与 $m$ 呈指数增长；人们希望在实际中，对遇到的特定问题实例，方法会快得多。这种有利情形确实会出现，但并非典型。即使有几十个变量的小问题，也可能需要很长时间（例如几小时或几天）来求解。

<!-- p.26 -->

全局优化用于变量少、计算时间不关键、而找到真正全局解的价值很高的那些问题。工程设计中的一个例子是对高价值或安全攸关系统做 worst-case analysis（最坏情况分析）或验证。这里变量表示不确定的 parameters（参数），它们会随制造、环境或运行条件而变化；目标函数是一个 utility function（效用函数），即值越小越糟的函数；约束表示关于参数可能取值的先验知识。优化问题 (1.1) 就是寻找参数的最坏取值。如果最坏值尚可接受，就能认证该系统（关于这些参数变化）是安全或可靠的。

局部优化方法能快速找到一组「很糟、但不保证是绝对最坏」的参数值。如果局部优化方法找到了导致不可接受性能的参数值，它成功地判定了系统不可靠。但局部优化方法无法认证系统可靠——它只能「没有找到坏参数」。全局优化方法则相反，会找到参数的绝对最坏值，若相应性能可以接受，就能认证系统安全。代价是计算时间，即使参数相对较少，也可能非常大。但在「认证性能的价值很高」或「对可靠性/安全性判断错误的代价很高」的情形下，这可能是值得的。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">1.4.3 Role of convex optimization in nonconvex problems（凸优化在非凸问题中的作用）</span>

本书主要讨论凸优化问题以及可归结为凸优化问题的应用。但凸优化在非凸问题中也扮演着重要角色。

<!-- p.27 -->

**为局部优化提供初始化（Initialization for local optimization）。** 一个明显的用法是把凸优化与局部优化方法结合。从非凸问题出发，先找到问题的一个近似但 convex（凸）的表述。求解这个近似问题（这很容易做，且不需要初始点），得到近似凸问题的精确解，然后把这一点用作局部优化方法的 starting point（起始点），作用于原始非凸问题。

**非凸优化的凸启发式（Convex heuristics for nonconvex optimization）。** 凸优化是若干求解非凸问题的 heuristics（启发式方法）的基础。我们后面会看到一个有趣的例子：寻找满足某些约束的 sparse vector（稀疏向量，即非零元很少的向量）。这是一个困难的组合问题，但存在一些基于凸优化的简单启发式，常常能找到相当稀疏的解。（这些将在第 6 章描述。）

另一个宽泛的例子是 randomized algorithms（随机化算法）：从某个 probability distribution（概率分布）抽取若干候选解，把其中最好的作为非凸问题的近似解。现在假设抽样的分布族被参数化（例如由均值与 covariance（协方差）参数化），于是可以提出这样的问题：这些分布中哪个给出目标函数最小的 expected value（期望值）？事实证明这个问题有时本身就是凸问题，因而可以高效求解。（见习题 11.23。）

<!-- p.27/28 -->

**为全局优化提供界（Bounds for global optimization）。** 许多全局优化方法需要一个关于非凸问题最优值的、计算代价低的 lower bound（下界）。两个标准方法都基于凸优化。在 relaxation（松弛）中，每个非凸约束被替换为更宽松但 convex（凸）的约束。在 Lagrangian relaxation（拉格朗日松弛）中，求解 Lagrangian dual problem（拉格朗日对偶问题）（第 5 章描述）。这个问题是凸的，并给出非凸问题最优值的下界。

> <span style="color:#2471a3;">**[译者注｜跨课程连接]** 「用松弛取下界」与你已经熟悉的算法思路同构：CLRS 中用决策树模型给比较排序取 $\Omega(n\log n)$ 下界，这里用 Lagrangian 对偶给非凸优化取下界——都是「构造一个更容易分析、且不劣于（不优于）原问题的替身」。</span>

## <span style="color:#c0392b;">1.5 Outline（本书概要）</span>

<!-- p.28 -->

本书分为三个主要部分：Theory（理论）、Applications（应用）与 Algorithms（算法）。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">1.5.1 Part I: Theory（第一部分：理论）</span>

第一部分 Theory 讲述凸分析与凸优化的基本定义、概念与结果。我们不求面面俱到，选题偏向我们认为有助于识别与表述凸优化问题的那些内容。这些是 classical material（经典材料），几乎全部可以在其他凸分析与优化的教科书中找到。我们不试图给出结果的最一般形式；需要的话读者可参考任何标准凸分析教材。

<!-- p.28/29 -->

第 2 章与第 3 章分别讲 convex sets（凸集）与 convex functions（凸函数）。我们给出一些常见的凸集与凸函数例子，以及一批 convex calculus rules（凸微积分规则），即保持凸性的对集合与函数的运算。把基本例子与凸微积分规则结合起来，就能构造（或者更重要的，识别）一些相当复杂的凸集与凸函数。

第 4 章 Convex optimization problems（凸优化问题）细致地处理优化问题，并描述一系列可用于重新表述问题的 transformations（变换）。我们还引入凸优化的一些常见子类，如 linear programming（线性规划）与 geometric programming（几何规划），以及较新发展的 second-order cone programming（二阶锥规划）与 semidefinite programming（半定规划）。

第 5 章讲 Lagrangian duality（拉格朗日对偶），它在凸优化中处于中心地位。这里我们给出经典的 Karush-Kuhn-Tucker optimality conditions（KKT 最优性条件），以及凸优化问题的 local 与 global sensitivity analysis（局部与全局敏感性分析）。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">1.5.2 Part II: Applications（第二部分：应用）</span>

<!-- p.29 -->

第二部分 Applications 描述凸优化在概率与统计、computational geometry（计算几何）、数据拟合等领域的各种应用。我们以希望对广泛读者都易于理解的方式来描述这些应用。为了让每个应用保持简短，我们只考虑简单情形，有时加上关于可能推广的评论。我们确信对其中某些应用的处理会让专家们皱眉，在此预先致歉。但我们的目标是快速地向广泛读者传达应用的「风味」，而不是给出优雅、理论完备或完整的处理。我们的背景是 electrical engineering（电气工程）中的控制系统、signal processing（信号处理）、电路分析与设计等领域。虽然我们在教学中（以本书为主要教材）包括这些主题，但其中只有少数应用足够普适而收入本书。

第二部分的目标是通过例子向读者展示凸优化在实践中如何被应用。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">1.5.3 Part III: Algorithms（第三部分：算法）</span>

<!-- p.29/30 -->

第三部分 Algorithms 描述求解凸优化问题的数值方法，重点是 Newton's algorithm（牛顿算法）与 interior-point methods（内点法）。第三部分按三章组织，分别讲述 unconstrained optimization（无约束优化）、equality constrained optimization（等式约束优化）与 inequality constrained optimization（不等式约束优化）。这三章遵循一个自然的 hierarchy（层级结构）：把求解一个问题约化为求解一列更简单的问题。Quadratic optimization problems（二次优化问题，包括例如最小二乘）位于该层级的基础，它们可以通过解一组线性方程精确求解。在第 9、10 章发展的 Newton's method（牛顿法）是层级的下一层：用牛顿法求解无约束或等式约束问题被约化为求解一列二次问题。第 11 章描述的 interior-point methods（内点法）构成层级的顶层：这些方法通过求解一列无约束或等式约束问题来求解不等式约束问题。

<!-- p.30 -->

总体上我们只讲少数几个算法，并省略了整类好方法，例如 quasi-Newton（拟牛顿）、conjugate-gradient（共轭梯度）、bundle（束方法）与 cutting-plane（割平面）算法。对我们讲述的方法，给的是简化变体，而非最新、最精巧的版本。选择算法的标准有几点：我们选择 simple（简单，易于描述与实现）、而且 reliable（可靠）、robust（稳健）、对大多数问题足够有效且快的方法。

许多凸优化使用者最终只是使用（而非开发）标准软件，例如线性规划或半定规划求解器。对这些使用者，第三部分意在传达这些方法的基本风味，并给出其基本属性的一些概念。对少数将要开发新算法的人，我们认为第三部分是一个很好的入门。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">1.5.4 Appendices（附录）</span>

<!-- p.30/31 -->

书末有三个 appendices（附录）。第一个列出我们用到的一些基本数学事实，并顺带给出记号总表。第二个附录覆盖一个相当特别的主题：目标为二次函数、约束含一个二次约束的优化问题。它们是 nonconvex problems（非凸问题），但仍能被有效求解，我们在第二部分的若干应用中会用到相关结果。

最后一个附录给出 numerical linear algebra（数值线性代数）的简短介绍，聚焦于能利用问题结构（如稀疏性）来获得效率的那些方法。我们不涉及若干重要主题，包括 roundoff analysis（舍入误差分析），也不给出执行所需 factorizations（分解）的方法细节。这些主题由许多优秀教材覆盖。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">1.5.5 Comments on examples（关于例子的说明）</span>

<!-- p.31 -->

书中许多地方（特别是在分别讲应用与算法的第二、三部分）我们用具体例子来阐述思想。有些例子是专门为阐明论点而选择（或设计）的；另一些例子则选得比较「typical（典型）」——即从某个显而易见或简单的概率分布中抽取的样本。从几十上百个随机生成的例子对算法性能下结论的风险众所周知，我们不再重复。这些例子只是为了给出算法性能的大致印象，或计算量随问题维度变化的大致印象，而不是算法性能的精确预测器。特别地，你的结果可能与我们的不同。

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">1.5.6 Comments on exercises（关于习题的说明）</span>

<!-- p.31/32 -->

每章末尾都有一组 exercises（习题）。有些要求补全正文中论证或论断的细节；另一些专注于判定或确立给定集合、函数或问题的凸性，或者更一般地，凸优化问题的表述。一些章节包含 numerical exercises（数值习题），要求用适当的高级语言做一些（但不多的）编程。习题难度参差，而且会毫无预警地从相当直白变为相当刁钻。

## <span style="color:#c0392b;">1.6 Notation（记号）</span>

<!-- p.32 -->

我们的记号大体标准，只有少量例外。本节描述基本记号；更完整的列表见第 697 页。

我们用 $\mathbb{R}$ 表示 real numbers（实数）集合，$\mathbb{R}_+$ 表示 nonnegative real numbers（非负实数）集合，$\mathbb{R}_{++}$ 表示 positive real numbers（正实数）集合。$n$ 维实向量的集合记作 $\mathbb{R}^n$，$m\times n$ 实矩阵的集合记作 $\mathbb{R}^{m\times n}$。我们用方括号定界向量与矩阵，分量之间用空格分隔；用圆括号从逗号分隔的列表构造 column vectors（列向量）。例如，若 $a,b,c\in\mathbb{R}$，则

$$
(a,b,c) = \begin{bmatrix} a\\ b\\ c\end{bmatrix} = \left[\,a\;\;b\;\;c\,\right]^T,
$$

它是 $\mathbb{R}^3$ 的元素。符号 $\mathbf{1}$ 表示所有分量均为 1 的向量（维度由上下文确定）。记号 $x_i$ 可以指向量 $x$ 的第 $i$ 个 component（分量），也可以指向量序列 $x_1,x_2,\dots$ 中第 $i$ 个元素。上下文或正文会明确所指。

我们用 $\mathbf{S}^k$ 表示 $k\times k$ 对称矩阵的集合，$\mathbf{S}^k_+$ 表示对称 positive semidefinite（半正定）的 $k\times k$ 矩阵集合，$\mathbf{S}^k_{++}$ 表示对称 positive definite（正定）的 $k\times k$ 矩阵集合。

卷曲不等号 $\succeq$（及其严格形式 $\succ$）用于表示 generalized inequality（广义不等式）：在向量之间表示 componentwise inequality（逐分量不等式）；在对称矩阵之间表示 matrix inequality（矩阵不等式）。带下标时，符号 $\preceq_K$（或 $\prec_K$）表示关于 cone（锥）$K$ 的广义不等式（在 §2.4.1 解释）。

我们对函数的记号与标准记号略有偏离，但希望不会引起混淆。记号 $f:\mathbb{R}^p\to\mathbb{R}^q$ 表示 $f$ 是 $\mathbb{R}^p$ 某个子集（即其 domain（定义域），记作 $\mathbf{dom}\, f$）上的 $\mathbb{R}^q$ 值函数。可以把 $f:\mathbb{R}^p\to\mathbb{R}^q$ 的用法理解为如计算机语言般的函数类型声明：$f:\mathbb{R}^p\to\mathbb{R}^q$ 表示函数 $f$ 以实 $p$-向量为参数，返回实 $q$-向量。集合 $\mathbf{dom}\, f$（函数 $f$ 的定义域）指明 $\mathbb{R}^p$ 中使 $f(x)$ 有定义的点的子集。例如，我们把对数函数描述为 $\log:\mathbb{R}\to\mathbb{R}$，且 $\mathbf{dom}\,\log = \mathbb{R}_{++}$。记号 $\log:\mathbb{R}\to\mathbb{R}$ 表示对数函数接受并返回一个实数；$\mathbf{dom}\,\log = \mathbb{R}_{++}$ 表示对数仅对正数有定义。

<!-- p.33 -->

我们把 $\mathbb{R}^n$ 用作 generic finite-dimensional vector space（一般有限维向量空间）。我们还会遇到其他有限维向量空间，例如具有给定最大次数的 variable（变量）的多项式空间，或对称 $k\times k$ 矩阵空间 $\mathbf{S}^k$。通过为向量空间选定一组 basis（基），总能把它与 $\mathbb{R}^n$（$n$ 为其维度）等同起来，因此针对向量空间 $\mathbb{R}^n$ 陈述的一般结果都可以应用。我们通常把「把一般结果翻译到其他向量空间」留给读者。例如，任何 linear function（线性函数）$f:\mathbb{R}^n\to\mathbb{R}$ 都可以表示为 $f(x)=c^Tx$（$c\in\mathbb{R}^n$）。对于空间 $\mathbf{S}^k$，选一组基并翻译，即得相应陈述：任何线性函数 $f:\mathbf{S}^k\to\mathbb{R}$ 都可以表示为 $f(X) = \mathbf{tr}(CX)$，其中 $C\in\mathbf{S}^k$。

## <span style="color:#c0392b;">Bibliography（文献注释）</span>

**最小二乘** 是一个非常古老的学科：可参见 Gauss 在 1820 年代用拉丁文写成、近来由 Stewart 翻译的论著 [Gau95]；较新的工作包括 Lawson 与 Hanson [LH95] 及 Björck [Bjö96] 的书。linear programming（线性规划）的参考文献见第 4 章。

**非线性规划的局部方法** 有许多好教材：Gill, Murray 与 Wright [GMW81]、Nocedal 与 Wright [NW99]、Luenberger [Lue84]、Bertsekas [Ber99]。

**全局优化** 见 Horst 与 Pardalos [HP94]、Pinter [Pin95]、Tuy [Tuy98] 的书。用凸优化为非凸问题找界是一个活跃的研究方向，可见上述全局优化教材、Ben-Tal 与 Nemirovski [BTN01, §4.3] 的书，以及 Nesterov, Wolkowicz 与 Ye 的综述 [NWY00]。该主题的著名论文有 Goemans 与 Williamson [GW95]、Nesterov [Nes00, Nes98]、Ye [Ye99]、Parrilo [Par03]。随机化方法见 Motwani 与 Raghavan [MR95]。

**Convex analysis（凸分析）**——凸集、凸函数与优化问题的数学——是数学中发展成熟的分支。基本参考包括 Rockafellar [Roc70]、Hiriart-Urruty 与 Lemaréchal [HUL93, HUL01]、Borwein 与 Lewis [BL00]、Bertsekas, Nedić 与 Ozdaglar [Ber03] 的书。更多凸分析参考见第 2–5 章。

Nesterov 与 Nemirovski [NN94] 首先指出 interior-point methods（内点法）可以求解许多凸优化问题；另见第 11 章的参考文献。Ben-Tal 与 Nemirovski [BTN01] 的书覆盖现代凸优化、内点法及其应用。

本书未涵盖的凸优化求解方法包括 subgradient methods（次梯度方法）[Sho85]、bundle methods（束方法）[HUL93]、cutting-plane methods（割平面方法）[Kel60, EM75, GLY96] 与 ellipsoid method（椭球法）[Sho91, BGT81]。

「凸优化问题 tractable（易于处理）」的思想并不新鲜。人们早就认识到，凸优化理论远比一般非线性优化理论来得直截了当（而且完备）。在此背景下，Rockafellar 在其 1993 年 SIAM Review 综述 [Roc93] 中陈述道：

> In fact the great watershed in optimization isn't between linearity and nonlinearity, but convexity and nonconvexity.
> （事实上，优化中真正的分水岭不在于线性与非线性，而在于凸与非凸。）

<!-- p.34 -->

第一个正式论证「凸优化问题比一般非线性优化问题易解」的是 Nemirovski 与 Yudin 在其 1983 年的书《Problem Complexity and Method Efficiency in Optimization》[NY83] 中：他们证明凸优化问题的 information-based complexity（基于信息的复杂度）远低于一般非线性优化问题。关于该主题较新的书是 Vavasis [Vav91]。

内点法低（理论）复杂度是该领域现代研究的核心。许多研究致力于证明：某个内点法（或其他方法）求解某类凸优化问题所需的运算次数增长不超过问题维数的多项式与 $\log(1/\epsilon)$（其中 $\epsilon>0$ 是所需精度）。（第 11 章会看到一些这类简单结果。）该主题第一部全面的著作是 Nesterov 与 Nemirovski [NN94]。其他书包括 Ben-Tal 与 Nemirovski [BTN01, lecture 5] 与 Renegar [Ren01]。内点法对各种凸优化问题的多项式时间复杂度，与许多非凸优化问题（其所有已知算法在最坏情况下都需要问题维数的指数级运算次数）形成鲜明对照。

<!-- p.35 -->

凸优化已被用于太多应用领域，无法在此一一引用。凸分析在经济学与金融学中处于核心地位，是许多结果的基础。例如：separating hyperplane theorem（分离超平面定理）加上 no-arbitrage assumption（无套利假设）可用来推出 prices（价格）与 risk-neutral probabilities（风险中性概率）的存在性（见 Luenberger [Lue95, Lue98] 与 Ross [Ros99]）。凸优化（尤其是求解 semidefinite programs，半定规划的能力）近来在自动控制理论中受到特别关注。凸优化在控制理论中的应用可见 Boyd 与 Barratt [BB91]、Boyd, El Ghaoui, Feron 与 Balakrishnan [BEFB94]、Dahleh 与 Diaz-Bobillo [DDB95]、El Ghaoui 与 Niculescu [EN00]、Dullerud 与 Paganini [DP00] 的书。embedded (convex) optimization（嵌入式凸优化）的一个好例子是 model predictive control（模型预测控制）：这种自动控制技术要求每一步求解一个（凸）二次规划。模型预测控制现在广泛用于化工过程控制工业；见 Morari 与 Zafiriou [MZ89]。另一个凸优化（尤其是 geometric programming，几何规划）有悠久历史的应用领域是 electronic circuit design（电子电路设计）。该方向的研究论文包括 Fishburn 与 Dunlop [FD85]、Sapatnekar, Rao, Vaidya 与 Kang [SRVK93]、Hershenson, Boyd 与 Lee [HBL01]。Luo [Luo03] 给出了在 signal processing（信号处理）与 communications（通信）中应用的综述。更多凸优化应用的参考见第 4 章与第 6–8 章。

<!-- p.36 -->

求解凸优化问题的近期内点法的高质量实现，可在 LOQO [Van97] 与 MOSEK [MOS02] 软件包以及第 11 章所列代码中获得。用于描述优化问题的软件系统包括 AMPL [FGK99] 与 GAMS [BKMR98]；两者都为识别「可变换为线性规划的问题」提供了一定支持。

---

> <span style="color:#7f8c8d;">本章完。下一章：boyd_ch02（Convex sets，凸集，PDF p.35–80）。</span>
