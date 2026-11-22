# EE364A 补充材料双语译注 —— CVX Tutorial

> Stanford University · January 22, 2015
> 本材料为 CVX 建模系统入门讲义，译注保留 slide 分页结构，中文为主体，英文术语行内嵌入。

---

## Slide 1 — 封面

- **CVX tutorial session**（CVX 教程课）
- **EE364A**（课程编号）
- **Stanford University**（斯坦福大学）
- **January 22, 2015**（2015 年 1 月 22 日）

---

## Slide 2 — 什么是 CVX？

- CVX 是一个面向 **convex optimization**（凸优化）问题的**建模系统**（modeling system）。
  - *convex optimization*：目标函数为凸函数、约束集为凸集的最优化问题——若函数 f 满足 `f(θx+(1−θ)y) ≤ θf(x)+(1−θ)f(y)`（对任意 θ∈[0,1]），则称 f 为 convex function（凸函数）。
- 官网：http://cvxr.com/cvx

---

## Slide 3 — 在 Corn 服务器上的安装

**在终端（terminal）中：**

```text
$ ssh -Y SUNETID@corn.stanford.edu
$ wget http://web.cvxr.com/cvx/cvx-a64.tar.gz
$ tar -xvf cvx-a64.tar.gz
$ module load matlab
$ matlab &
```

**在 MATLAB 中：**

```text
>> cd cvx
>> cvx setup
```

- `ssh`：远程登录命令；`-Y` 启用 X11 转发以显示 MATLAB 图形界面。
- `wget`：下载压缩包；`tar -xvf`：解压。
- `module load matlab`：加载 MATLAB 运行环境模块。
- `cvx setup`：在 MATLAB 内完成 CVX 的初始化配置。

---

## Slide 4 — 凸问题的结构

**数学形式：**

```math
\begin{aligned}
\text{minimize} \quad & f_0(x) \\
\text{subject to} \quad & f_i(x) \le 0, \quad i = 1, \dots, m \\
& h_i(x) = 0, \quad i = 1, \dots, p
\end{aligned}
```

**在 CVX 中：**

```text
cvx begin
variables x(n)
minimize(f0(x))
subject to
f(x) <= 0
A * x - b == 0
cvx end
```

- **NB（注意）**：`f0` 与 `fi` 必须是 **convex**（凸函数），而 `hi` 必须是 **affine**（仿射函数，即线性函数加常数项）。
- 左侧为标准形式：目标 `f0(x)` 凸、不等式约束 `fi(x) ≤ 0` 中每个 `fi` 凸、等式约束 `hi(x) = 0` 中每个 `hi` 仿射。

---

## Slide 5 — 返回值

求解结束后，CVX 会设置以下变量：

- `x` —— 最优解变量 `x⋆`（solution variable）
- `cvx optval` —— 最优值 `p⋆`（optimal value）
- `cvx status` —— 求解器状态（solver status），可取 `Solved`（已求解）、`Unbounded`（无界）、`Infeasible`（不可行）等
- `...`（其他变量略）

---

## Slide 6 — 例子 · 基础

**优化问题：**

```math
\begin{aligned}
\text{minimize} \quad & x + y \\
\text{subject to} \quad & x \ge 1, \\
& y = 2.
\end{aligned}
```

**在 CVX 中：**

```text
cvx begin
variables x(1) y(1)
minimize(x + y)
subject to
x >= 1
y == 2
cvx end
```

- 该问题目标与约束均为仿射，属线性规划（LP）的特例。

---

## Slide 7 — 例子 · 基础（结果）

CVX 返回解与状态：

```text
>> x
1
>> y
2
>> cvx optval
3
>> cvx status
Solved
```

- 最优解 `x = 1`、`y = 2`，最优值 `p⋆ = 3`，状态为 `Solved`（已求解）。

---

## Slide 8 — 例子 · 线性规划（LP）

**优化问题：**

```math
\begin{aligned}
\text{maximize} \quad & c^T x \\
\text{subject to} \quad & Ax = b \\
& x \succeq 0
\end{aligned}
```

**在 CVX 中：**

```text
cvx begin
variables x(n)
maximize(c' * x)
subject to
A * x == b
x >= 0
cvx end
```

- **LP**（linear programming，线性规划）：目标与约束均为仿射（线性）的最优化问题。
- `x ⪰ 0`（即 `x >= 0`）表示分量非负约束；`c' * x` 为向量内积。

---

## Slide 9 — 例子 · 半定规划（SDP）

**优化问题：**

```math
\begin{aligned}
\text{minimize} \quad & \| X - A \|_2 \\
\text{subject to} \quad & X \succeq 0
\end{aligned}
```

**在 CVX 中：**

```text
cvx begin sdp
variable X(n, n)
minimize(norm(A - X))
subject to
X >= 0
cvx end
```

- **SDP**（semidefinite programming，半定规划）：决策变量为对称矩阵、并受半正定锥约束的凸优化问题。
- `X ⪰ 0`（`X >= 0`）表示矩阵半正定；`\|·\|_2` 为谱范数（spectral norm，最大奇异值）。

---

## Slide 10 — 例子 · 指派类问题

**优化问题：**

```math
\text{minimize} \quad \sum_i \| x - a_i \|_2
```

**在 CVX 中：**

```text
cvx begin
variable x(n)
OBJ = 0
for i = 1:n
OBJ = OBJ + norm(x - A(:,i));
end
minimize(OBJ)
cvx end
```

- **NB（注意）**：循环规模较大时，这种写法可能非常慢！

---

## Slide 11 — CVX 的常见陷阱

- 等式约束要用 `==`，而不是 `=`。
- 用 `>=` 和 `<=`，而不是 `>` 和 `<`。
- 每个函数必须在其**定义域（domain）上是凸函数**——而不仅仅在可行域内是凸的。

---

## Slide 12 — 资源

- CVX 文档：http://web.cvxr.com/cvx/doc/
- **DCP**（disciplined convex optimization，规范化凸优化）规则：http://dcp.stanford.edu/
  - *disciplined convex optimization*：通过一组受限的原子函数与组合规则（如非负加权和、仿射复合等）来构造并自动判定凸性的建模规范。

---

## Slide 13 — 提问环节

- Questions?（如有疑问，请提问。）
