> <span style="color:#7f8c8d;">Source: 补充材料/Convex.jl Tutorial.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

# <span style="color:#c0392b;">凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）</span>

<!-- p.1 -->

## <span style="color:#c0392b;">Convex.jl Tutorial（Convex.jl 教程）</span>

<span style="color:#7f8c8d;">David Zeng · Karanveer Mohan · EE364A · Stanford University（斯坦福大学）· January 20, 2015（2015 年 1 月 20 日）</span>

<!-- p.2 -->

### <span style="color:#c0392b;">The Julia programming language（Julia 编程语言）</span> <span style="color:#2471a3;">**[section]**</span>

- 一种面向科学计算（scientific computing）的新编程语言
  - 由一个以 MIT 成员为主的团队开发
  - 完全开源（open source），也就是免费
  - 提供便捷的语法来构造向量、矩阵等数学对象（math constructs）
  - 速度极快

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> Julia 于 2012 年由 MIT 的 Jeff Bezanson、Stefan Karpinski、Viral Shah 与 Alan Edelman 等人正式发布，设计口号是 "walks like Python, runs like C"（写起来像 Python，跑起来像 C）。在此之前，科学计算圈长期存在"两语言困境"：用 Python 或 MATLAB 做原型易读，但数值循环慢到无法接受，性能关键部分又得用 C/Fortran 重写一遍——正如你在 CSAPP 中看到的，解释器层的高级抽象与缓存友好的底层执行之间隔着巨大的性能鸿沟。Julia 的解法是即时编译（JIT，just-in-time compilation）加参数类型系统：每种参数类型组合在首次调用时生成专门的本机代码，再配合多重派发（multiple dispatch）按参数类型选择方法。对本课程的实际意义在于：EE364A 的作业此前多用 MATLAB + CVX 完成，Convex.jl 正是 CVX 在 Julia 生态中的对应物；而本教程基于 2015 年初的 Julia v0.3，其 API（如 `Pkg.add`）与今天 Julia 1.x 的包管理模式差异很大，动手复现时务必注意版本。

<!-- p.3 -->

### <span style="color:#c0392b;">Installing Julia（安装 Julia）</span> <span style="color:#2471a3;">**[section]**</span>

- 从 http://julialang.org/downloads/ 下载 Julia v0.3
- 按照屏幕上的安装说明逐步操作

<!-- p.4 -->

### <span style="color:#c0392b;">Resources for learning Julia（学习 Julia 的资源）</span> <span style="color:#2471a3;">**[section]**</span>

- Julia 官方文档（official documentation）：http://docs.julialang.org/en/release-0.3/
- Julia 基础教程：http://learnxinyminutes.com/docs/julia/
- 搭建 IJulia 环境（environment）：https://github.com/stevengj/julia-mit

<!-- p.5 -->

### <span style="color:#c0392b;">Disciplined Convex Programming（规范凸规划，DCP）</span> <span style="color:#2471a3;">**[section]**</span>

- <span style="color:#00838f;">**DCP（Disciplined Convex Programming，规范凸规划）是一套用于构造具有确定曲率（curvature）的数学表达式的规则体系**</span>
- 它覆盖了形形色色的凸优化问题（convex optimization problems）——但并非全部！
- 它让软件能够轻松判定一个优化问题是否为凸
- 更多关于 DCP 的内容见：http://dcp.stanford.edu

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> DCP 的核心思想值得从第一性原理拆解：一个优化问题之所以"可解"，前提是我们能机械地验证"目标函数是凸的、约束构成凸集"，而 DCP 就是把这种验证彻底规则化、可计算化。它维护一张原子函数（atom）表，每个原子都带有已被证明的曲率属性（凸/凹/仿射）与单调性，再通过复合规则（例如凸函数复合凸且不减的函数仍为凸，这是 Jensen 不等式方向的推广）逐层判定整个表达式的曲率。这与 Strang 线代中判定正定性的思路一脉相承：$x^\top A x \ge 0$ 对一切 $x$ 成立无法靠枚举验证，但借助特征值符号、Cholesky 分解等结构性工具即可判定——DCP 对"曲率"做的事情完全同构，把一个分析性质转化为可机械检验的语法性质。代价是保守性：许多确实凸的表达式（如几何均值 $\sqrt{x_1 x_2}$ 型）不在规则覆盖范围内，所以课件强调 "covers a wide variety… but not all of them"。这也是 6.006 中"问题的可解性取决于表示方式"这一思想在优化领域的镜像：DCP 用牺牲表达能力，换来了自动验证与自动锥化，进而保证多项式时间可解。

<!-- p.6 -->

### <span style="color:#c0392b;">Convex.jl</span> <span style="color:#2471a3;">**[section]**</span>

- 一个面向规范凸规划（DCP）的 Julia 软件包（package）
  - 充当"纸面上的凸优化问题"与后端求解器（backend solver）之间的接口（interface）
  - 让许多凸优化问题都能用自然的数学语法来描述

<!-- p.7 -->

### <span style="color:#c0392b;">Installing Convex.jl（安装 Convex.jl）</span> <span style="color:#2471a3;">**[section]**</span>

1. 打开一个 Julia 终端（terminal）
2. 更新到最新的软件包（package）列表

```julia
Pkg.update()
```

3. 安装 SCS——Convex.jl 的默认后端求解器

```julia
Pkg.add("SCS")
```

4. 安装 Convex.jl

```julia
Pkg.add("Convex")
```

<!-- p.8 -->

### <span style="color:#c0392b;">Basic Convex.jl program（基本的 Convex.jl 程序）</span> <span style="color:#2471a3;">**[section]**</span>

- 先试跑一下，确保一切安装正确

```julia
using Convex
x = Variable()
p = minimize(x, x >= 0)
solve!(p)
```

这四行代码依次完成：导入 Convex 包；声明一个标量决策变量 $x$；构造"在约束 $x \ge 0$ 下最小化 $x$"的优化问题（第一个参数是目标函数，其余参数是约束）；最后求解问题并把数值解写回变量。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这份仅 9 页的短教程浓缩了一个重要的软件工程分层思想：建模层（Convex.jl）与求解层（SCS）彻底解耦。Convex.jl 只负责把用户写出的 DCP 合法表达式改写成等价的锥规划标准形（最终落到二阶锥规划 SOCP / 半定规划 SDP 级别的锥形式），再把标准形交给 SCS 这类后端求解器数值求解；两层之间用标准问题格式通信，因此可以随时更换求解器而模型代码一行不改——CVX 在 MATLAB 中也是同样的架构。代码里的 `x >= 0` 尤其值得玩味：它并非布尔比较（结果不是 true/false），而是通过运算符重载（operator overloading）构造出一个约束对象，`minimize(x, x >= 0)` 再把目标与约束组装成问题实例；`solve!` 末尾的感叹号是 Julia 惯例，表示该函数会修改其参数（原地写入解）。另一个彩蛋：默认求解器 SCS 用的不是课文中讲的内点法，而是一阶算子分裂（ADMM 类）算法——用中等精度换取对大规模稀疏问题的稳健性，与课件内点法一章形成互补：小规模求高精度用内点法，大规模容许中等精度时一阶方法更划算。

<!-- p.9 -->

### <span style="color:#c0392b;">Convex.jl full documentation（Convex.jl 完整文档）</span> <span style="color:#2471a3;">**[section]**</span>

- 关于如何在 Convex.jl 中构造并求解凸优化问题的完整文档，见 http://convexjl.readthedocs.org
