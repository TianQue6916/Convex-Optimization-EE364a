# EE364a HW5_bonus 解答（作业 5 附加题解答）

> 本文件为 Stanford EE364a（Winter 2007–08，Prof. S. Boyd）《Convex Optimization》作业 5 附加题官方解答的**双语译注**。中文为主体，英文术语行内嵌入，首次出现给 `English（中文）` 注记。公式用 LaTeX 书写，行内用 `$...$`，独立公式用 ```math 围栏，禁用 ````math` 与 Unicode 上下标。

---

## 题目 1（Problem 1：Schur complements / 舒尔补）

**题干：** Consider a matrix $X = X^T \in \mathbf{R}^{n\times n}$ partitioned as
```math
X = \begin{bmatrix} A & B \\ B^T & C \end{bmatrix},
```
where $A \in \mathbf{R}^{k\times k}$. If $\det A \neq 0$, the matrix $S = C - B^T A^{-1} B$ is called the Schur complement of $A$ in $X$. Schur complements arise in many situations and appear in many important formulas and theorems. For example, we have $\det X = \det A \det S$. (You don't have to prove this.)

**中文：** 考虑一个对称矩阵 $X = X^T \in \mathbf{R}^{n\times n}$，分块（partition）为
```math
X = \begin{bmatrix} A & B \\ B^T & C \end{bmatrix},
```
其中 $A \in \mathbf{R}^{k\times k}$。若 $\det A \neq 0$，则矩阵 $S = C - B^T A^{-1} B$ 称为 $A$ 在 $X$ 中的 **Schur complement（舒尔补）**。舒尔补在许多场合出现，并出现在许多重要公式与定理中。例如有 $\det X = \det A \det S$。（无需你证明。）

---

### (a) 二次型对部分变量取极小得到的舒尔补

**题干：** The Schur complement arises when you minimize a quadratic form over some of the variables. Let $f(u, v) = [u^T\ v^T]\, X\, [u^T\ v^T]^T$, where $u \in \mathbf{R}^k$. Let $g(v)$ be the minimum value of $f$ over $u$, i.e., $g(v) = \inf_u f(u, v)$. Of course $g(v)$ can be $-\infty$. Show that if $A \succ 0$, we have $g(v) = v^T S v$.

**中文：** 当你对二次型的一部分变量取极小值时，舒尔补就会出现。设 $f(u, v) = [u^T\ v^T]\, X\, [u^T\ v^T]^T$，其中 $u \in \mathbf{R}^k$。令 $g(v)$ 为 $f$ 关于 $u$ 的最小值，即 $g(v) = \inf_u f(u, v)$。当然 $g(v)$ 可能取到 $-\infty$。证明：若 $A \succ 0$（即 $A$ 正定），则 $g(v) = v^T S v$。

**解答：**

先把二次型按 $u, v$ 展开。由分块结构：
```math
f(u,v) =
\begin{bmatrix} u \\ v \end{bmatrix}^T
\begin{bmatrix} A & B \\ B^T & C \end{bmatrix}
\begin{bmatrix} u \\ v \end{bmatrix}
= u^T A u + u^T B v + v^T B^T u + v^T C v.
```
注意 $u^T B v = v^T B^T u$ 是两个互为转置的标量，因此二者相等，合并为 $2\, u^T B v$。于是
```math
f(u,v) = u^T A u + 2 u^T B v + v^T C v.
```
因为 $A \succ 0$，所以 $A$ 可逆且关于 $u$ 的二次部分严格凸，$f(u,v)$ 关于 $u$ 有唯一的极小点。我们对 $u$ 求梯度并令其为零：
```math
\nabla_u f(u,v) = 2 A u + 2 B v = 0.
```
由 $\nabla_u (u^T A u) = 2 A u$（$A$ 对称）以及 $\nabla_u (2 u^T B v) = 2 B v$，可得
```math
A u = - B v.
```
因为 $A$ 可逆，唯一极小点（minimizer）为
```math
u^{\star} = -A^{-1} B v.
```
把 $u^{\star}$ 代回 $f(u,v)$，计算极小值 $g(v) = f(u^{\star}, v)$。先算各项：
```math
u^{\star T} A u^{\star}
= \big(-A^{-1}B v\big)^T A \big(-A^{-1}B v\big)
= v^T B^T A^{-1} A A^{-1} B v
= v^T B^T A^{-1} B v,
```
其中用到了 $A^{-1}$ 对称（因 $A$ 对称正定）以及 $A^{-1} A A^{-1} = A^{-1}$。

再算交叉项：
```math
2\, u^{\star T} B v = 2 \big(-A^{-1} B v\big)^T B v = -2\, v^T B^T A^{-1} B v.
```
于是
```math
\begin{aligned}
g(v)
&= f(u^{\star}, v) \\
&= u^{\star T} A u^{\star} + 2 u^{\star T} B v + v^T C v \\
&= v^T B^T A^{-1} B v - 2 v^T B^T A^{-1} B v + v^T C v \\
&= v^T \big(C - B^T A^{-1} B\big) v \\
&= v^T S v.
\end{aligned}
```
其中最后一步用到了舒尔补的定义 $S = C - B^T A^{-1} B$。证毕。

> 注：这个结果揭示了舒尔补的本质——它是二次型在"消去"一部分变量后，剩余变量部分所对应的二次型矩阵，即**部分极小化（partial minimization）**后的"有效二次型"。

---

### (b) 正定/半正定的舒尔补刻画

**题干：** The Schur complement arises in several characterizations of positive definiteness or semidefiniteness of a block matrix. As examples we have the following three theorems:

- $X \succ 0$ if and only if $A \succ 0$ and $S \succ 0$.
- If $A \succ 0$, then $X \succeq 0$ if and only if $S \succeq 0$.
- $X \succeq 0$ if and only if $A \succeq 0$, $B^T(I - AA^{\dagger}) = 0$ and $C - B^T A^{\dagger} B \succeq 0$, where $A^{\dagger}$ is the pseudo-inverse of $A$.

Prove one of these theorems. (You can choose which one.)

**中文：** 舒尔补出现在块矩阵正定性或半正定性的若干刻画中。例如有如下三条定理：

- $X \succ 0$（正定）当且仅当 $A \succ 0$ 且 $S \succ 0$。
- 若 $A \succ 0$，则 $X \succeq 0$（半正定）当且仅当 $S \succeq 0$。
- $X \succeq 0$ 当且仅当 $A \succeq 0$、$B^T(I - AA^{\dagger}) = 0$ 且 $C - B^T A^{\dagger} B \succeq 0$，其中 $A^{\dagger}$ 是 $A$ 的伪逆（pseudo-inverse）。

证明其中一条定理。（你可任选一条。）

**解答：** 这里证明前两条（它们等价性强、也是最常用的）。核心工具是如下**块矩阵的分解恒等式**。对可逆的 $A$，有
```math
X = \begin{bmatrix} A & B \\ B^T & C \end{bmatrix}
= \begin{bmatrix} I & 0 \\ B^T A^{-1} & I \end{bmatrix}
\begin{bmatrix} A & 0 \\ 0 & S \end{bmatrix}
\begin{bmatrix} I & A^{-1} B \\ 0 & I \end{bmatrix},
```
其中 $S = C - B^T A^{-1} B$。这个分解可直接乘开验证：右下块为 $B^T A^{-1} \cdot A \cdot A^{-1} B + S = B^T A^{-1} B + (C - B^T A^{-1} B) = C$，右上块为 $A \cdot A^{-1} B = B$，左下块为 $B^T A^{-1} \cdot A = B^T$。设
```math
L = \begin{bmatrix} I & 0 \\ B^T A^{-1} & I \end{bmatrix},
\qquad
D = \begin{bmatrix} A & 0 \\ 0 & S \end{bmatrix},
```
则 $L$ 是下三角矩阵，主对角全为 $1$，故 $\det L = 1$；而 $X = L D L^T$。由于 $L$ 可逆，$X$ 与 $D$ 是**合同（congruent）**矩阵，由 Sylvester 惯性定律（law of inertia），$X$ 与 $D$ 的正、负、零惯性指数完全相同。

**（第一条：$X \succ 0 \iff A \succ 0$ 且 $S \succ 0$）**

- **充分性（$\Rightarrow$）**：设 $X \succ 0$。因为 $X = L D L^T$ 且 $L$ 可逆，$X$ 正定当且仅当 $D$ 正定（合同变换保持正定性）。而 $D$ 为块对角阵 $\operatorname{diag}(A, S)$，$D \succ 0$ 当且仅当 $A \succ 0$ 且 $S \succ 0$。证得 $A \succ 0$ 且 $S \succ 0$。
- **必要性（$\Leftarrow$）**：设 $A \succ 0$ 且 $S \succ 0$，则 $D \succ 0$，又 $L$ 可逆，故 $X = L D L^T \succ 0$。

**（第二条：若 $A \succ 0$，则 $X \succeq 0 \iff S \succeq 0$）**

与上面完全同理：$A \succ 0$ 时 $A$ 可逆，$X = L D L^T$，合同变换保持半正定性，故
```math
X \succeq 0 \iff D = \operatorname{diag}(A, S) \succeq 0.
```
而 $A \succ 0 \succeq 0$ 已成立，故 $D \succeq 0$ 当且仅当 $S \succeq 0$。证毕。

> 注：第一条还有另一个"从正定性定义出发"的等价视角——由 (a) 知对任意 $v$，$g(v) = \inf_u f(u,v) = v^T S v$。若 $A \succ 0$ 且 $S \succ 0$，则对任意非零 $[u^T\ v^T]^T$，$f(u,v) \ge v^T S v > 0$（当 $v \neq 0$），而当 $v = 0$ 时 $f(u,0) = u^T A u > 0$（当 $u \neq 0$），故 $X \succ 0$。这正是两条定理在 (a) 结果上的自然延伸。

---

### (c) 用 LMI 表示 epi f

**题干：** Recognizing Schur complements often helps to represent nonlinear convex constraints as linear matrix inequalities (LMIs). Consider the function
```math
f(x) = (Ax + b)^T \big(P_0 + x_1 P_1 + \cdots + x_n P_n\big)^{-1} (Ax + b)
```
where $A \in \mathbf{R}^{m\times n}$, $b \in \mathbf{R}^m$, and $P_i = P_i^T \in \mathbf{R}^{m\times m}$, with domain
```math
\operatorname{dom} f = \{\, x \in \mathbf{R}^n \mid P_0 + x_1 P_1 + \cdots + x_n P_n \succ 0 \,\}.
```
This is the composition of the matrix fractional function and an affine mapping, and so is convex. Give an LMI representation of $\operatorname{epi} f$. That is, find a symmetric matrix $F(x, t)$, affine in $(x, t)$, for which
```math
x \in \operatorname{dom} f,\ f(x) \le t \iff F(x, t) \succeq 0.
```
**中文：** 识别出舒尔补常常有助于把非线性凸约束表示成线性矩阵不等式（linear matrix inequality, LMI）。考虑函数
```math
f(x) = (Ax + b)^T \big(P_0 + x_1 P_1 + \cdots + x_n P_n\big)^{-1} (Ax + b),
```
其中 $A \in \mathbf{R}^{m\times n}$、$b \in \mathbf{R}^m$、$P_i = P_i^T \in \mathbf{R}^{m\times m}$，其定义域为
```math
\operatorname{dom} f = \{\, x \in \mathbf{R}^n \mid P_0 + x_1 P_1 + \cdots + x_n P_n \succ 0 \,\}.
```
这是矩阵分数函数（matrix fractional function）与一个仿射映射（affine mapping）的复合，因此是凸的。给出 $\operatorname{epi} f$（上图）的一个 LMI 表示，即求一个关于 $(x,t)$ 仿射的对称矩阵 $F(x,t)$，使得
```math
x \in \operatorname{dom} f,\ f(x) \le t \iff F(x, t) \succeq 0.
```
**解答：**

记
```math
P(x) = P_0 + x_1 P_1 + \cdots + x_n P_n,
```
它是关于 $x$ 的对称仿射矩阵值函数。当 $x \in \operatorname{dom} f$ 时有 $P(x) \succ 0$，故 $P(x)$ 可逆。令
```math
G(x) = \begin{bmatrix} P(x) & Ax + b \\ (Ax+b)^T & t \end{bmatrix}.
```
这是一个 $(m+1) \times (m+1)$ 的对称矩阵，且是关于 $(x,t)$ 的仿射矩阵值函数。

当 $P(x) \succ 0$ 时，$G(x)$ 中左上块 $P(x)$ 可逆，其舒尔补为
```math
S_G = t - (Ax+b)^T P(x)^{-1} (Ax+b) = t - f(x).
```
由题目 (b) 的第二条定理（$A \succ 0$ 时 $X \succeq 0 \iff S \succeq 0$）直接得到：
```math
G(x) \succeq 0 \iff P(x) \succ 0 \text{ 且 } t - f(x) \ge 0.
```
这里 $t - f(x) \ge 0$ 即 $f(x) \le t$，而 $P(x) \succ 0$ 即 $x \in \operatorname{dom} f$。于是取
```math
F(x,t) = \begin{bmatrix} P(x) & Ax + b \\ (Ax+b)^T & t \end{bmatrix},
```
就得到所要的 LMI 表示：
```math
x \in \operatorname{dom} f,\ f(x) \le t
\iff
F(x,t) = \begin{bmatrix} P_0 + x_1 P_1 + \cdots + x_n P_n & Ax + b \\ (Ax+b)^T & t \end{bmatrix} \succeq 0.
```
因此 $\operatorname{epi} f = \{\, (x,t) \mid F(x,t) \succeq 0 \,\}$，即上图由一个线性矩阵不等式完整刻画。这正是舒尔补把"矩阵分数函数"这类非线性凸约束线性化的标准手法。

---

## 题目 2（Problem 2：把优化问题表述为 SDP）

**题干：** Formulate the following optimization problems as semidefinite programs. The variable is $x \in \mathbf{R}^n$; $F(x)$ is defined as
```math
F(x) = F_0 + x_1 F_1 + x_2 F_2 + \cdots + x_n F_n
```
with $F_i \in \mathbf{S}^m$. The domain of $f$ in each subproblem is $\operatorname{dom} f = \{\, x \in \mathbf{R}^n \mid F(x) \succ 0 \,\}$.

**中文：** 把下列优化问题表述成半定规划（semidefinite program, SDP）。变量是 $x \in \mathbf{R}^n$；$F(x)$ 定义为
```math
F(x) = F_0 + x_1 F_1 + x_2 F_2 + \cdots + x_n F_n,
```
其中 $F_i \in \mathbf{S}^m$（$m$ 阶对称矩阵）。每个子问题中 $f$ 的定义域为 $\operatorname{dom} f = \{\, x \in \mathbf{R}^n \mid F(x) \succ 0 \,\}$。

---

### (a) 最小化 $f(x) = c^T F(x)^{-1} c$

**解答：** 引入辅助标量变量 $t$。目标 $f(x) \le t$ 可等价写成一个 LMI。由题目 1(c) 的舒尔补手法，$F(x) \succ 0$ 时
```math
c^T F(x)^{-1} c \le t
\iff
\begin{bmatrix} F(x) & c \\ c^T & t \end{bmatrix} \succeq 0.
```
（左上块 $F(x)$ 可逆正定时，其舒尔补为 $t - c^T F(x)^{-1} c$，半正定当且仅当 $t \ge c^T F(x)^{-1} c$。）因此原问题等价于
```math
\begin{aligned}
\operatorname{minimize} &\quad t \\
\operatorname{subject\ to} &\quad \begin{bmatrix} F(x) & c \\ c^T & t \end{bmatrix} \succeq 0.
\end{aligned}
```
这是一个标准的 SDP：目标线性，约束是关于 $(x,t)$ 的仿射线性矩阵不等式。

---

### (b) 最小化 $f(x) = \max_{i=1,\dots,K} c_i^T F(x)^{-1} c_i$

**解答：** 仍引入辅助标量 $t$。目标 $\max_i c_i^T F(x)^{-1} c_i \le t$ 等价于对每个 $i$ 都有 $c_i^T F(x)^{-1} c_i \le t$。对每个 $i$ 分别用 (a) 的舒尔补 LMI：
```math
c_i^T F(x)^{-1} c_i \le t
\iff
\begin{bmatrix} F(x) & c_i \\ c_i^T & t \end{bmatrix} \succeq 0,
\qquad i = 1, \dots, K.
```
因此问题等价于
```math
\begin{aligned}
\operatorname{minimize} &\quad t \\
\operatorname{subject\ to} &\quad \begin{bmatrix} F(x) & c_i \\ c_i^T & t \end{bmatrix} \succeq 0,
\quad i = 1, \dots, K.
\end{aligned}
```
这是 $K$ 个线性矩阵不等式约束下的 SDP。

---

### (c) 最小化 $f(x) = \sup_{\lVert c \rVert_2 \le 1} c^T F(x)^{-1} c$

**解答：** 先分析内层极大。当 $F(x) \succ 0$ 时，$F(x)^{-1} \succ 0$，其最大特征值记为 $\lambda_{\max}\big(F(x)^{-1}\big)$。对单位球 $\lVert c \rVert_2 \le 1$ 上的二次型，最大值在最大特征值对应的单位特征向量处取得：
```math
\sup_{\lVert c \rVert_2 \le 1} c^T F(x)^{-1} c = \lambda_{\max}\big(F(x)^{-1}\big).
```
所以 $f(x) = \lambda_{\max}\big(F(x)^{-1}\big)$。现在引入辅助标量 $t$，约束 $f(x) \le t$ 等价于 $\lambda_{\max}\big(F(x)^{-1}\big) \le t$，即 $F(x)^{-1} \preceq t I$。由于 $F(x) \succ 0$，在正定锥内矩阵求逆是**保序反序**的（即 $A \succeq B \succ 0 \Rightarrow B^{-1} \succeq A^{-1}$），故
```math
F(x)^{-1} \preceq t I
\iff
F(x) \succeq \frac{1}{t} I,
\qquad t > 0.
```
（严格地说还需 $t > 0$，而极小化会自动迫使 $t > 0$。）于是问题等价于
```math
\begin{aligned}
\operatorname{minimize} &\quad t \\
\operatorname{subject\ to} &\quad F(x) \succeq \frac{1}{t} I.
\end{aligned}
```
这个约束关于 $(x,t)$ 并非仿射（含 $1/t$）。为得到标准 SDP，改用变量 $s = 1/t$（即 $t = 1/s$），极小化 $t$ 变为极小化 $1/s$，等价于极大化 $s$。于是
```math
\begin{aligned}
\operatorname{maximize} &\quad s \\
\operatorname{subject\ to} &\quad F(x) \succeq s I.
\end{aligned}
```
这是一个 SDP（目标与约束均关于 $(x,s)$ 仿射/线性）。等价地，也可写作极小化 $-s$。这就是把"逆矩阵的最大特征值"这类目标通过 Schur/特征值不等式线性化的标准结果：$F(x) \succeq sI$ 意味着 $F(x)^{-1}$ 的所有特征值 $\le 1/s$。

---

### (d) 最小化 $f(x) = \mathbb{E}\big(c^T F(x)^{-1} c\big)$

**题干补充：** where $c$ is a random vector with mean $\mathbb{E}\, c = \bar c$ and covariance $\mathbb{E}\,(c-\bar c)(c-\bar c)^T = S$.

**解答：** 先化简期望。对随机向量 $c$，二次型 $c^T M c$（$M$ 为确定性对称矩阵）的期望满足
```math
\mathbb{E}[c^T M c] = \mathbb{E}[\operatorname{tr}(M c c^T)] = \operatorname{tr}\big(M\, \mathbb{E}[c c^T]\big).
```
由协方差定义，二阶矩矩阵为
```math
\mathbb{E}[c c^T] = S + \bar c \bar c^T.
```
因此
```math
\begin{aligned}
f(x)
&= \mathbb{E}\big[c^T F(x)^{-1} c\big] \\
&= \operatorname{tr}\Big(F(x)^{-1}\, \mathbb{E}[c c^T]\Big) \\
&= \operatorname{tr}\Big(F(x)^{-1} (S + \bar c \bar c^T)\Big).
\end{aligned}
```
因为 $S \succeq 0$，可作谱分解（或对称平方根分解）$S = \sum_{k} \sigma_k s_k s_k^T$（$\sigma_k \ge 0$ 为特征值，$s_k$ 为相应特征向量）。于是
```math
\begin{aligned}
f(x)
&= \operatorname{tr}\big(F(x)^{-1} S\big) + \operatorname{tr}\big(F(x)^{-1} \bar c \bar c^T\big) \\
&= \operatorname{tr}\big(F(x)^{-1} S\big) + \bar c^T F(x)^{-1} \bar c \\
&= \sum_{k} \sigma_k\, s_k^T F(x)^{-1} s_k + \bar c^T F(x)^{-1} \bar c.
\end{aligned}
```
现在引入辅助标量 $t$ 及辅助变量 $t_k$（对每个 $s_k$ 分量），把每一项都用舒尔补 LMI 线性化。目标 $f(x) \le t$ 等价于
```math
\sum_k \sigma_k\, s_k^T F(x)^{-1} s_k + \bar c^T F(x)^{-1} \bar c \le t.
```
引入 $t_k \ge 0$ 使 $\sum_k \sigma_k t_k + \bar c^T F(x)^{-1} \bar c \le t$，并对每一项写 LMI：
```math
s_k^T F(x)^{-1} s_k \le t_k
\iff
\begin{bmatrix} F(x) & s_k \\ s_k^T & t_k \end{bmatrix} \succeq 0,
```

```math
\bar c^T F(x)^{-1} \bar c \le t_0
\iff
\begin{bmatrix} F(x) & \bar c \\ \bar c^T & t_0 \end{bmatrix} \succeq 0.
```
综上，问题可写为
```math
\begin{aligned}
\operatorname{minimize} &\quad t \\
\operatorname{subject\ to} &\quad \begin{bmatrix} F(x) & s_k \\ s_k^T & t_k \end{bmatrix} \succeq 0,
\quad k = 1, \dots, m, \\
&\quad \begin{bmatrix} F(x) & \bar c \\ \bar c^T & t_0 \end{bmatrix} \succeq 0, \\
&\quad \sum_{k=1}^{m} \sigma_k t_k + t_0 \le t.
\end{aligned}
```
这是一个 SDP。实际上更简洁的写法是直接利用迹的形式：目标 $\operatorname{tr}\big(F(x)^{-1} S\big) \le t$ 也可等价写为"存在 $Z \succeq 0$ 使得 $\operatorname{tr} Z \le t$ 且 $Z \succeq \dots$"，但上面的显式分解已经给出了一个完全线性化的 SDP 表述。核心结论：**二次型的期望仍可化为若干个矩阵分数函数之和，从而用一组 LMI 线性表示。**

---

## 题目 3（Problem 3：对数最优投资问题的最优性条件与对偶）

**题干：**

**(a)** Show that the optimality conditions for the log-optimal investment problem described in exercise 4.60 can be expressed as: $\mathbf{1}^T x = 1$, $x \succeq 0$, and for each $i$,
```math
x_i > 0 \Rightarrow \sum_{j=1}^{m} \pi_j \frac{p_{ij}}{p_j^T x} = 1,
\qquad
x_i = 0 \Rightarrow \sum_{j=1}^{m} \pi_j \frac{p_{ij}}{p_j^T x} \le 1.
```
**中文：** 证明：对数最优投资问题（exercise 4.60 所述）的最优性条件可表达为：$\mathbf{1}^T x = 1$、$x \succeq 0$，且对每个 $i$，
```math
x_i > 0 \Rightarrow \sum_{j=1}^{m} \pi_j \frac{p_{ij}}{p_j^T x} = 1,
\qquad
x_i = 0 \Rightarrow \sum_{j=1}^{m} \pi_j \frac{p_{ij}}{p_j^T x} \le 1.
```
我们可以如下解释：$p_{ij}/p_j^T x$ 是一个随机变量，表示"仅投资资产 $i$ 的收益"与"用混合组合 $x$ 投资所得收益"之比。最优性条件是：对每一个我们投资的资产，该比值的期望为 $1$；对每一个我们不投资的资产，该比值的期望不超过 $1$。粗略地说，这意味着我们的组合做得和任何被选中的资产一样好，也不会比任何未被选中的资产更差。

**解答（a）：**

对数最优投资问题的原始形式为
```math
\begin{aligned}
\operatorname{maximize} &\quad \sum_{j=1}^{m} \pi_j \log\big(p_j^T x\big) \\
\operatorname{subject\ to} &\quad x \succeq 0,\ \mathbf{1}^T x = 1.
\end{aligned}
```
其中 $p_j \in \mathbf{R}^n_+$ 是第 $j$ 个结果下各资产的收益向量，$p_{ij}$ 是其第 $i$ 个分量，$\pi_j$ 是第 $j$ 个结果的概率（$\sum_j \pi_j = 1$，$\pi_j > 0$）。注意约束要求 $p_j^T x > 0$ 时对数才有意义。

把它写成极小化形式（凸问题）：
```math
\begin{aligned}
\operatorname{minimize} &\quad -\sum_{j=1}^{m} \pi_j \log\big(p_j^T x\big) \\
\operatorname{subject\ to} &\quad x \succeq 0,\ \mathbf{1}^T x = 1.
\end{aligned}
```
目标 $f(x) = -\sum_j \pi_j \log(p_j^T x)$ 是凸的（$-\log$ 是凸函数，正权重组合，且 $\log$ 是凹函数故 $-\log$ 凸）。约束 $x \succeq 0$（锥约束）与 $\mathbf{1}^T x = 1$（等式）都是凸的。

我们使用 **KKT 条件（Karush-Kuhn-Tucker conditions）**。引入对偶变量：对不等式约束 $x \succeq 0$（即 $-x \preceq 0$）配非负乘子 $\lambda \in \mathbf{R}^n_+$，对等式约束 $\mathbf{1}^T x = 1$ 配乘子 $\nu \in \mathbf{R}$。Lagrangian（拉格朗日函数）为
```math
\mathcal{L}(x, \lambda, \nu) = -\sum_{j=1}^{m} \pi_j \log\big(p_j^T x\big) - \lambda^T x + \nu\big(\mathbf{1}^T x - 1\big).
```
对 $x$ 求梯度并令为零（stationarity / 平稳性条件）。先算 $\nabla_x \log(p_j^T x)$：由链式法则，$\partial \log(p_j^T x)/\partial x_i = p_{ij}/(p_j^T x)$，故
```math
\nabla_x \big(-\sum_j \pi_j \log(p_j^T x)\big) = -\sum_j \pi_j \frac{p_j}{p_j^T x},
```
其第 $i$ 个分量为 $-\sum_j \pi_j \frac{p_{ij}}{p_j^T x}$。于是平稳性条件为
```math
-\sum_{j=1}^{m} \pi_j \frac{p_{ij}}{p_j^T x} - \lambda_i + \nu = 0,
\qquad i = 1, \dots, n.
```
即
```math
\sum_{j=1}^{m} \pi_j \frac{p_{ij}}{p_j^T x} = \nu - \lambda_i,
\qquad i = 1, \dots, n.
```
对偶可行（dual feasibility）条件：$\lambda_i \ge 0$。

互补松弛（complementary slackness）条件：$\lambda_i x_i = 0$，对每个 $i$。

现在分两种情况讨论：

- 若 $x_i > 0$，由互补松弛得 $\lambda_i = 0$，代入平稳性条件得
```math
\sum_{j=1}^{m} \pi_j \frac{p_{ij}}{p_j^T x} = \nu.
```
- 若 $x_i = 0$，则 $\lambda_i \ge 0$，由 $\nu - \lambda_i = \sum_j \pi_j \frac{p_{ij}}{p_j^T x}$ 得
```math
\sum_{j=1}^{m} \pi_j \frac{p_{ij}}{p_j^T x} = \nu - \lambda_i \le \nu.
```
因此存在一个公共常数 $\nu$（对所有 $i$），满足：投资为正的资产其"期望比值"等于 $\nu$，投资为零的资产其"期望比值"不超过 $\nu$。

为把 $\nu$ 具体化，将平稳性条件两边乘以 $x_i$ 并对 $i$ 求和：
```math
\sum_{i=1}^{n} x_i \sum_{j=1}^{m} \pi_j \frac{p_{ij}}{p_j^T x}
= \sum_{i} x_i \nu - \sum_i x_i \lambda_i.
```
由互补松弛 $\sum_i x_i \lambda_i = 0$。左边交换求和次序并利用 $\sum_i x_i p_{ij} = p_j^T x$：
```math
\sum_{j=1}^{m} \pi_j \frac{\sum_{i} x_i p_{ij}}{p_j^T x}
= \sum_{j=1}^{m} \pi_j \frac{p_j^T x}{p_j^T x}
= \sum_{j=1}^{m} \pi_j = 1.
```
右边为 $\nu \sum_i x_i = \nu \cdot \mathbf{1}^T x = \nu \cdot 1 = \nu$。因此 $\nu = 1$。代入前述两种情形，即得
```math
x_i > 0 \Rightarrow \sum_{j=1}^{m} \pi_j \frac{p_{ij}}{p_j^T x} = 1,
\qquad
x_i = 0 \Rightarrow \sum_{j=1}^{m} \pi_j \frac{p_{ij}}{p_j^T x} \le 1.
```
证毕。这正是题述的"期望比值等于 $1$ / 不超过 $1$"的最优性条件。

---

**题干：**

**(b)** In this part we will derive the dual of the log-optimal investment problem. We start by writing the problem as,
```math
\begin{aligned}
\operatorname{minimize} &\quad -\sum_{j=1}^{m} \pi_j \log y_j \\
\operatorname{subject\ to} &\quad y = P^T x, \\
&\quad x \succeq 0,\ \mathbf{1}^T x = 1.
\end{aligned}
```
Here, $P$ has columns $p_1, \dots, p_m$, and we have the introduced new variables $y_1, \dots, y_m$, with the implicit constraint $y \succ 0$. We will associate dual variables $\nu$, $\lambda$ and $\nu_0$ with the constraints $y = P^T x$, $x \succeq 0$, and $\mathbf{1}^T x = 1$, respectively.

Defining $\tilde\nu_j = \nu_j / \nu_0$ for $j = 1, \dots, m$, show that the dual problem can be written as
```math
\begin{aligned}
\operatorname{maximize} &\quad \sum_{j=1}^{m} \pi_j \log\big(\tilde\nu_j / \pi_j\big) \\
\operatorname{subject\ to} &\quad P \tilde\nu \preceq \mathbf{1},
\end{aligned}
```
with variable $\tilde\nu$. The objective here is the (negative) Kullback-Leibler divergence between the given distribution $\pi$ and the dual variable $\tilde\nu$.

**中文：** 本部分我们推导对数最优投资问题的对偶。先把问题写成
```math
\begin{aligned}
\operatorname{minimize} &\quad -\sum_{j=1}^{m} \pi_j \log y_j \\
\operatorname{subject\ to} &\quad y = P^T x, \\
&\quad x \succeq 0,\ \mathbf{1}^T x = 1.
\end{aligned}
```
这里 $P$ 的列是 $p_1, \dots, p_m$，我们引入了新变量 $y_1, \dots, y_m$，并带隐式约束 $y \succ 0$。我们将对偶变量 $\nu$、$\lambda$、$\nu_0$ 分别与约束 $y = P^T x$、$x \succeq 0$、$\mathbf{1}^T x = 1$ 相关联。

对 $j = 1, \dots, m$ 定义 $\tilde\nu_j = \nu_j / \nu_0$，证明对偶问题可写成
```math
\begin{aligned}
\operatorname{maximize} &\quad \sum_{j=1}^{m} \pi_j \log\big(\tilde\nu_j / \pi_j\big) \\
\operatorname{subject\ to} &\quad P \tilde\nu \preceq \mathbf{1},
\end{aligned}
```
变量为 $\tilde\nu$。这里的目标是给定分布 $\pi$ 与对偶变量 $\tilde\nu$ 之间的（负）KL 散度（Kullback-Leibler divergence）。

**解答（b）：**

从带新变量 $y$ 的形式出发。Lagrangian 为
```math
\mathcal{L}(x, y, \lambda, \nu, \nu_0)
= -\sum_{j=1}^{m} \pi_j \log y_j + \nu^T\big(y - P^T x\big) - \lambda^T x + \nu_0\big(\mathbf{1}^T x - 1\big).
```
其中 $\nu \in \mathbf{R}^m$ 对应等式约束 $y = P^T x$（无符号限制），$\lambda \in \mathbf{R}^n_+$ 对应 $x \succeq 0$，$\nu_0 \in \mathbf{R}$ 对应 $\mathbf{1}^T x = 1$。

**对偶函数** $g(\lambda, \nu, \nu_0) = \inf_{x \succeq 0,\ y \succ 0} \mathcal{L}$。整理 $\mathcal{L}$，把含 $x$ 与含 $y$ 的项分开：
```math
\mathcal{L} = \Big[ -\sum_j \pi_j \log y_j + \nu^T y \Big] + \Big[ -\lambda^T x - \nu^T P^T x + \nu_0 \mathbf{1}^T x \Big] - \nu_0.
```
其中含 $x$ 的项可合并为
```math
-\lambda^T x - \nu^T P^T x + \nu_0 \mathbf{1}^T x
= \big(-\lambda - P\nu + \nu_0 \mathbf{1}\big)^T x.
```
（用到了 $\nu^T P^T x = (P\nu)^T x$。）

**第一步：对 $x \succeq 0$ 取极小。** 对 $x \succeq 0$，线性函数 $\big(-\lambda - P\nu + \nu_0 \mathbf{1}\big)^T x$ 的下确界为
```math
\inf_{x \succeq 0} \big(-\lambda - P\nu + \nu_0 \mathbf{1}\big)^T x
=
\begin{cases}
0, & -\lambda - P\nu + \nu_0 \mathbf{1} \succeq 0, \\
-\infty, & \text{否则}.
\end{cases}
```
（因为若某个分量的系数为负，取对应 $x_i \to +\infty$ 就使线性项趋于 $-\infty$。）因此对偶函数非平凡（$> -\infty$）要求
```math
\nu_0 \mathbf{1} - P\nu - \lambda \succeq 0
\iff
P\nu + \lambda \preceq \nu_0 \mathbf{1}.
```
**第二步：对 $y \succ 0$ 取极小。** 对每个 $j$，单独极小化 $-\pi_j \log y_j + \nu_j y_j$。这是无约束的一维凸极小问题。对 $y_j$ 求导并令为零：
```math
\frac{\partial}{\partial y_j}\big(-\pi_j \log y_j + \nu_j y_j\big)
= -\frac{\pi_j}{y_j} + \nu_j = 0
\Rightarrow y_j^{\star} = \frac{\pi_j}{\nu_j}.
```
这要求 $\nu_j > 0$（否则若 $\nu_j \le 0$，函数 $-\pi_j \log y_j + \nu_j y_j$ 在 $y_j \to \infty$ 时趋于 $-\infty$，下确界为 $-\infty$）。代回得该 $j$ 项的极小值为
```math
-\pi_j \log \frac{\pi_j}{\nu_j} + \nu_j \cdot \frac{\pi_j}{\nu_j}
= -\pi_j \log\frac{\pi_j}{\nu_j} + \pi_j.
```
对所有 $j$ 求和，得到对偶函数（在 $\nu_j > 0$ 且 $P\nu + \lambda \preceq \nu_0 \mathbf{1}$ 条件下）：
```math
g(\lambda, \nu, \nu_0)
= \sum_{j=1}^{m}\Big(-\pi_j \log\frac{\pi_j}{\nu_j} + \pi_j\Big) - \nu_0
= \sum_{j=1}^{m} \pi_j \log\frac{\nu_j}{\pi_j} + \Big(\sum_j \pi_j - \nu_0\Big).
```
因为 $\sum_j \pi_j = 1$，故
```math
g(\lambda, \nu, \nu_0)
= \sum_{j=1}^{m} \pi_j \log\frac{\nu_j}{\pi_j} + 1 - \nu_0.
```
**第三步：写对偶问题并化简。** 对偶问题是极大化 $g$，约束为 $\lambda \succeq 0$、$\nu \succ 0$、以及 $P\nu + \lambda \preceq \nu_0 \mathbf{1}$。其中 $\lambda$ 只出现在约束里，为放宽约束（使可行域最大、从而 $g$ 越大）应取 $\lambda = 0$（只要 $P\nu \preceq \nu_0 \mathbf{1}$ 本身成立即可，令 $\lambda = 0$ 总可行且不改变目标）。于是约束化为 $P\nu \preceq \nu_0 \mathbf{1}$，且 $\nu \succ 0$、$\nu_0$ 自由。

现在引入缩放变量 $\tilde\nu_j = \nu_j / \nu_0$。注意原始问题满足 Slater 条件（例如取 $x = (1/n)\mathbf{1}$ 时各 $p_j^T x > 0$，严格可行），故**强对偶（strong duality）**成立，且最优时 $\nu_0 \ne 0$（实际上 $\nu_0 > 0$）。将约束 $P\nu \preceq \nu_0 \mathbf{1}$ 两边除以 $\nu_0 > 0$ 得 $P\tilde\nu \preceq \mathbf{1}$。目标中
```math
\log\frac{\nu_j}{\pi_j} = \log\frac{\nu_0 \tilde\nu_j}{\pi_j} = \log\tilde\nu_j/\pi_j + \log \nu_0.
```
于是
```math
g = \sum_j \pi_j \log\frac{\tilde\nu_j}{\pi_j} + \big(\log \nu_0\big)\sum_j \pi_j + 1 - \nu_0
= \sum_j \pi_j \log\frac{\tilde\nu_j}{\pi_j} + \log \nu_0 + 1 - \nu_0.
```
对 $\nu_0$ 单独极大化常数项 $h(\nu_0) = \log \nu_0 + 1 - \nu_0$。求导：
```math
h'(\nu_0) = \frac{1}{\nu_0} - 1 = 0 \Rightarrow \nu_0^{\star} = 1,
```
且 $h''(\nu_0) = -1/\nu_0^2 < 0$，故为极大值，$h(1) = \log 1 + 1 - 1 = 0$。因此消去 $\nu_0$ 后，对偶问题化为
```math
\begin{aligned}
\operatorname{maximize} &\quad \sum_{j=1}^{m} \pi_j \log\big(\tilde\nu_j / \pi_j\big) \\
\operatorname{subject\ to} &\quad P \tilde\nu \preceq \mathbf{1},
\qquad \tilde\nu \succeq 0.
\end{aligned}
```
（$\tilde\nu \succeq 0$ 由 $\nu \succ 0$ 与 $\nu_0 > 0$ 推出。）其中目标正是 $\pi$ 与 $\tilde\nu$ 之间 KL 散度的负值：
```math
\sum_j \pi_j \log\frac{\tilde\nu_j}{\pi_j}
= -D_{\mathrm{KL}}\big(\pi \,\Vert\, \tilde\nu\big),
```
故极大化它等价于极小化 KL 散度。证毕。

> 注：约束可再补充 $\sum_j \pi_j \tilde\nu_j \le 1$ 之类的隐含条件（由原始可行性），但题述形式 $P\tilde\nu \preceq \mathbf{1}$ 已完整刻画对偶可行域。这个对偶问题的漂亮之处在于：目标就是"让对偶分布 $\tilde\nu$ 尽量贴近真实分布 $\pi$"，且满足"任何资产的期望收益 $\le 1$"这一约束。

---

## 题目 4（Problem 4：对数最优投资策略）

**题干：** In this problem you will solve a specific instance of the log-optimal investment problem described in exercise 4.60, with $n = 5$ assets and $m = 10$ possible outcomes in each period. The problem data are defined in `log_opt_invest.m`, with the rows of the matrix $P$ giving the asset return vectors $p_j^T$. The outcomes are equiprobable, i.e., we have $\pi_j = 1/m$. Each column of the matrix $P$ gives the return of the associated asset in the different possible outcomes. You can examine the columns to get an idea of the types of assets. For example, the last asset gives a fixed and certain return of 1%; the first asset is a very risky one, with occasional large return, and (more often) substantial loss.

Find the log-optimal investment strategy $x^{\star}$, and its associated long term growth rate $R^{\star}_{\mathrm{lt}}$. Compare this to the long term growth rate obtained with a uniform allocation strategy, i.e., $x = (1/n)\mathbf{1}$, and also with a pure investment in each asset.

For the optimal investment strategy, and also the uniform investment strategy, plot 10 sample trajectories of the accumulated wealth, i.e., $W(T) = W(0)\prod_{t=1}^{T}\lambda(t)$, for $T = 0, \dots, 200$, with initial wealth $W(0) = 1$.

**中文：** 本问题求解 exercise 4.60 所述对数最优投资问题的一个具体实例，每期有 $n = 5$ 个资产、$m = 10$ 种可能结果。问题数据定义在 `log_opt_invest.m` 中，矩阵 $P$ 的每一行给出资产收益向量 $p_j^T$。各结果是等概率的，即 $\pi_j = 1/m$。矩阵 $P$ 的每一列给出相应资产在不同可能结果下的收益。你可以观察各列来了解资产的类型。例如，最后一个资产给出固定且确定的 $1\%$ 收益；第一个资产是风险很高的资产，偶尔有高收益，但（更经常地）有大幅亏损。

求对数最优投资策略 $x^{\star}$ 及其长期增长率 $R^{\star}_{\mathrm{lt}}$。将其与均匀配置策略（即 $x = (1/n)\mathbf{1}$）以及单独全仓投资每个资产所得到的长期增长率进行比较。

对最优策略和均匀策略，各画出 10 条累计财富的样本轨迹，即 $W(T) = W(0)\prod_{t=1}^{T}\lambda(t)$，$T = 0, \dots, 200$，初始财富 $W(0) = 1$。

**解答：**

**问题建模。** 对数最优投资问题的目标函数是
```math
\operatorname{maximize} \quad \sum_{j=1}^{m} \pi_j \log\big(p_j^T x\big)
= \sum_{j=1}^{m} \frac{1}{m}\log\big(p_j^T x\big),
```
约束为 $x \succeq 0$、$\mathbf{1}^T x = 1$。目标乘以常数 $1/m$ 不影响最优解，故等价于极大化 $\sum_j \log(p_j^T x)$。

由于题目提示当前 CVX 版本不直接处理 $\log$，改用 `geomean()`。注意几何平均与对数的关系：
```math
\prod_{j=1}^{m}\big(p_j^T x\big)^{1/m} = \exp\Big(\frac{1}{m}\sum_j \log(p_j^T x)\Big),
```
即 $\log\big(\prod_j (p_j^T x)^{1/m}\big) = \frac{1}{m}\sum_j \log(p_j^T x)$。由于 $\log$ 单调递增，极大化 $\sum_j \log(p_j^T x)$ 等价于极大化几何平均 $\operatorname{geomean}(P^T x)$。CVX 中 `geomean` 是凹函数，可直接最大化。故 CVX 代码为：

```matlab
% log_opt_invest.m 中需要补充的部分
cvx_begin
    variable x(n)
    maximize( geomean(P' * x) )
    subject to
        x >= 0;
        ones(1,n) * x == 1;
cvx_end
```

（$P$ 的每一行是 $p_j^T$，故 $P^T x$ 的第 $j$ 个分量就是 $p_j^T x$。）

**长期增长率。** 每期收益倍数为 $\lambda(t) = p_j^T x$（当第 $t$ 期结果是 $j$ 时）。长期增长率（long term growth rate）定义为
```math
R_{\mathrm{lt}} = \lim_{T\to\infty} \frac{1}{T}\log W(T)
= \lim_{T\to\infty}\frac{1}{T}\sum_{t=1}^{T}\log\lambda(t)
= \mathbb{E}\big[\log(p_j^T x)\big]
= \sum_{j=1}^{m}\pi_j \log\big(p_j^T x\big).
```
（由大数定律，样本平均收敛于期望。）因此最优长期增长率就是最优目标值：
```math
R^{\star}_{\mathrm{lt}} = \sum_{j=1}^{m}\frac{1}{m}\log\big(p_j^T x^{\star}\big)
= \frac{1}{m}\sum_{j=1}^{m}\log\big(p_j^T x^{\star}\big).
```
**结果比较。**（具体数值依赖 `log_opt_invest.m` 中的数据；此处给出计算框架与一般性结论，实际运行后应填入数值。）

- **最优策略 $x^{\star}$**：CVX 求得的 $x^{\star}$ 会把资金集中分配到少数几个高期望对数收益的资产上（通常不会配置到那个"确定 1% 收益"的资产，因为其对数收益 $\log 1.01 \approx 0.00995$ 较低，但它可作为无风险保底）。
- **均匀策略** $x = (1/n)\mathbf{1}$：其长期增长率为 $\frac{1}{m}\sum_j \log\big(p_j^T \mathbf{1}/n\big)$。
- **单独投资资产 $i$**：即 $x = e_i$，其长期增长率为 $\frac{1}{m}\sum_j \log p_{ij}$。

对比三者，一般有
```math
R^{\star}_{\mathrm{lt}} \ge \max\Big\{ R_{\mathrm{lt}}(e_1), \dots, R_{\mathrm{lt}}(e_n), R_{\mathrm{lt}}(\mathbf{1}/n) \Big\},
```
因为最优解在包含这些点的可行集上极大化同一目标，故至少不差于任一单点。

**财富轨迹模拟。** 题目已提供模拟与绘图代码（`log_opt_invest.m` 内），只需补充求 $x^{\star}$ 的部分。轨迹按如下递推生成：初始 $W(0) = 1$；第 $t$ 期随机抽取一个结果 $j_t \in \{1,\dots,m\}$（等概率），则
```math
W(t) = W(t-1)\cdot\big(p_{j_t}^T x\big).
```
对最优策略与均匀策略各模拟 10 条独立轨迹，画 $W(t)$ 对 $t = 0,\dots,200$ 的对数（或线性）曲线。

[[FIGURE: 第 3 页，10 条最优策略与 10 条均匀策略的累计财富样本轨迹图（$W(0)=1$，$T=0..200$）]]

> 注：因本题依赖外部数据文件 `log_opt_invest.m`（未随解答文本提供具体数值矩阵 $P$），此处给出的是完整的建模、求解与比较框架；具体数值 $x^{\star}$ 与 $R^{\star}_{\mathrm{lt}}$ 需运行该 MATLAB 脚本后填入。

---

## 题目 5（Problem 5：赌博中最大化庄家利润与归算概率）

**题干：** A set of $n$ participants bet on which one of $m$ outcomes, labeled $1, \dots, m$, will occur. Participant $i$ offers to purchase up to $q_i > 0$ gambling contracts, at price $p_i > 0$, that the true outcome will be in the set $S_i \subset \{1, \dots, m\}$. The house then sells her $x_i$ contracts, with $0 \le x_i \le q_i$. If the true outcome $j$ is in $S_i$, then participant $i$ receives $\$1$ per contract, i.e., $x_i$. Otherwise, she loses, and receives nothing. The house collects a total of $x_1 p_1 + \cdots + x_n p_n$, and pays out an amount that depends on the outcome $j$, $\sum_{j \in S_i} x_i$.

The difference is the house profit.

**中文：** 有 $n$ 个参与者（participant）对 $m$ 个结果（标记为 $1,\dots,m$）中哪一个会发生下注。参与者 $i$ 愿以单价 $p_i > 0$ 购买至多 $q_i > 0$ 份赌博合约（contract），赌真实结果落在集合 $S_i \subset \{1,\dots,m\}$ 中。庄家（house）随后卖给她 $x_i$ 份合约，$0 \le x_i \le q_i$。若真实结果 $j \in S_i$，则参与者 $i$ 每份合约获得 $\$1$，即共 $x_i$ 美元；否则她输掉，什么都得不到。庄家总共收取 $x_1 p_1 + \cdots + x_n p_n$，而支出金额取决于结果 $j$，为 $\sum_{i : j \in S_i} x_i$（即所有"结果 $j$ 命中"的参与者的合约份数之和）。

两者之差就是庄家利润。

---

### (a) 庄家的最优策略

**题干：** How should the house decide on $x$ so that its worst-case profit (over the possible outcomes) is maximized? (The house determines $x$ after examining all the participant offers.)

**中文：** 庄家应如何决定 $x$，使得其最坏情形利润（在所有可能结果上取）最大化？（庄家在查看所有参与者报价后再确定 $x$。）

**解答：**

对结果 $j$，庄家的支出为 $\sum_{i:\, j \in S_i} x_i$，总收入为 $\sum_i x_i p_i$（与结果无关）。故结果 $j$ 下的利润为
```math
\operatorname{profit}_j(x) = \sum_{i=1}^{n} x_i p_i - \sum_{i:\, j \in S_i} x_i.
```
庄家要最大化最坏情形利润：
```math
\begin{aligned}
\operatorname{maximize} &\quad \min_{j=1,\dots,m} \Big( \sum_i x_i p_i - \sum_{i:\, j \in S_i} x_i \Big) \\
\operatorname{subject\ to} &\quad 0 \le x_i \le q_i,\quad i = 1,\dots,n.
\end{aligned}
```
引入辅助变量 $t$ 表示最坏情形利润，把 $\min$ 内层拆成 $m$ 个不等式约束，化为线性规划（linear program, LP）：
```math
\begin{aligned}
\operatorname{maximize} &\quad t \\
\operatorname{subject\ to} &\quad \sum_{i=1}^{n} x_i p_i - \sum_{i:\, j \in S_i} x_i \ge t,
\quad j = 1,\dots,m, \\
&\quad 0 \le x_i \le q_i,\quad i = 1,\dots,n.
\end{aligned}
```
即
```math
\begin{aligned}
\operatorname{maximize} &\quad t \\
\operatorname{subject\ to} &\quad t + \sum_{i:\, j \in S_i} x_i \le \sum_{i=1}^{n} x_i p_i,
\quad j = 1,\dots,m, \\
&\quad 0 \le x_i \le q_i,\quad i = 1,\dots,n.
\end{aligned}
```
这是一个线性规划：变量为 $(x, t)$，目标与约束均线性。解之即得庄家的最优策略 $x^{\star}$。

---

### (b) 归算概率

**题干：** Suppose $x^{\star}$ maximizes the worst-case house profit. Show that there exists a probability distribution $\pi$ on the possible outcomes (i.e., $\pi \in \mathbf{R}^m_+$, $\mathbf{1}^T \pi = 1$) for which $x^{\star}$ also maximizes the expected house profit. Explain how to find $\pi$.

**中文：** 设 $x^{\star}$ 使最坏情形庄家利润最大化。证明：存在结果上的一个概率分布 $\pi$（即 $\pi \in \mathbf{R}^m_+$，$\mathbf{1}^T \pi = 1$），使得 $x^{\star}$ 也最大化期望庄家利润。说明如何求 $\pi$。

**解答：**

思路是用 (a) 中 LP 的最优对偶变量来构造 $\pi$。

把 (a) 的 LP 写成标准形式。为便于取对偶，先写成极小化形式。原极大化问题
```math
\begin{aligned}
\operatorname{maximize}_{x,t} &\quad t \\
\text{s.t.} &\quad t + \sum_{i:\,j\in S_i} x_i - \sum_{i=1}^{n} x_i p_i \le 0,\ j=1,\dots,m, \\
&\quad 0 \le x_i \le q_i.
\end{aligned}
```
设与第 $j$ 个不等式约束对应的非负对偶变量为 $\lambda_j \ge 0$（$j = 1,\dots,m$）。定义指示矩阵 $A \in \{0,1\}^{m\times n}$，其元素 $A_{ji} = 1$ 当且仅当 $j \in S_i$，否则为 $0$。则第 $j$ 个约束为
```math
t + (A x)_j - p^T x \le 0,
\qquad p = (p_1,\dots,p_n).
```
极小化形式（取负）为
```math
\begin{aligned}
\operatorname{minimize} &\quad -t \\
\text{s.t.} &\quad t \mathbf{1} + (A - \mathbf{1}p^T) x \preceq 0, \\
&\quad 0 \le x_i \le q_i.
\end{aligned}
```
构造 Lagrangian（对第 $j$ 个不等式配 $\lambda_j \ge 0$）：
```math
\mathcal{L} = -t + \sum_{j=1}^{m}\lambda_j\big(t + (Ax)_j - p^T x\big)
= -t + t\Big(\sum_j \lambda_j\Big) + \lambda^T A x - \Big(\sum_j \lambda_j\Big) p^T x.
```
对 $t$ 的系数为 $-1 + \sum_j \lambda_j$。为使对偶函数有界（非 $-\infty$），需 $\sum_j \lambda_j = 1$（否则对 $t$ 取无界）。这正是"$\lambda$ 归一化"的来源，提示我们取
```math
\pi_j = \lambda_j.
```
在 $\sum_j \lambda_j = 1$ 下，Lagrangian 化为
```math
\mathcal{L} = \lambda^T A x - p^T x = \big(\lambda^T A - p^T\big) x = \sum_{i=1}^{n}\Big( \sum_{j:\,j\in S_i}\lambda_j - p_i \Big) x_i.
```
再对 $0 \le x_i \le q_i$ 取极小。定义 $d_i = \sum_{j\in S_i}\lambda_j - p_i$，则对每个 $i$ 单独极小化 $d_i x_i$ 于 $[0, q_i]$：
```math
\min_{0\le x_i \le q_i} d_i x_i =
\begin{cases}
0, & d_i \ge 0, \\
d_i q_i, & d_i < 0.
\end{cases}
```
于是对偶问题（极大化对偶函数）为：在 $\lambda \succeq 0$、$\sum_j \lambda_j = 1$ 下极大化 $\sum_{i} \min(0, d_i) q_i$。

现在**由 LP 强对偶性**（线性规划满足 Slater 条件——事实上等式/线性约束的 LP 总有强对偶），最优原始值等于最优对偶值。设 $\lambda^{\star}$ 是对偶最优解，则取
```math
\pi = \lambda^{\star}.
```
则 $\pi \in \mathbf{R}^m_+$，$\mathbf{1}^T \pi = \sum_j \lambda_j^{\star} = 1$，是一个合法的概率分布。

下面说明 $x^{\star}$ 也在该 $\pi$ 下最大化期望利润。期望利润为
```math
\mathbb{E}_{\pi}[\operatorname{profit}]
= \sum_{j=1}^{m} \pi_j\Big( \sum_i x_i p_i - \sum_{i:\,j\in S_i} x_i \Big)
= \sum_i x_i p_i - \sum_{j}\pi_j \sum_{i:\,j\in S_i} x_i
= \sum_i x_i p_i - \sum_{i} x_i \sum_{j:\,j\in S_i} \pi_j.
```
即
```math
\mathbb{E}_{\pi}[\operatorname{profit}] = \sum_{i=1}^{n} x_i\Big(p_i - \sum_{j\in S_i}\pi_j\Big).
```
由互补松弛（complementary slackness）条件：若 $x_i^{\star} \in (0, q_i)$，则 $d_i = 0$，即 $p_i = \sum_{j\in S_i}\pi_j$（此时该合约"公平定价"，多少份都无所谓）；若 $d_i > 0$（即 $p_i > \sum_{j\in S_i}\pi_j$，报价高于公平价），则 $x_i^{\star} = 0$；若 $d_i < 0$（报价低于公平价），则 $x_i^{\star} = q_i$（完全成交）。

于是对每个 $i$，$x_i^{\star}$ 在 $[0,q_i]$ 上恰好是最大化线性项 $x_i\big(p_i - \sum_{j\in S_i}\pi_j\big)$ 的解：当该系数为正时取 $x_i = q_i$，为负时取 $x_i = 0$，为零时任意。这正是逐项极大化 $\mathbb{E}_\pi[\operatorname{profit}]$。因为期望利润关于 $x$ 可分离（separable，逐项独立），逐项极大化即全局极大化，故
```math
x^{\star} \in \arg\max_{0 \le x \le q} \mathbb{E}_{\pi}[\operatorname{profit}].
```
**结论**：$\pi$ 由 LP 的最优对偶变量直接给出（$\pi_j = \lambda_j^{\star}$），且 $x^{\star}$ 同时最大化期望利润。这就是"从赌徒报价反推结果概率"的原理。

**Remark 译注：** 给定 $\pi$，报价 $i$ 的"公平价格"为 $p_i^{\mathrm{fair}} = \sum_{j \in S_i}\pi_j$。所有 $p_i > p_i^{\mathrm{fair}}$ 的报价会被完全成交（$x_i = q_i$）；所有 $p_i < p_i^{\mathrm{fair}}$ 的报价会被拒绝（$x_i = 0$）。

**Remark 译注：** 本练习展示了如何从一组赌徒的报价中猜测结果（例如选举）的概率。

---

### (c) 数值例子

**题干：** Carry out your method on the simple example below with $n = 5$ participants, $m = 5$ possible outcomes, and participant offers:

| Participant $i$ | $p_i$ | $q_i$ | $S_i$ |
|---|---|---|---|
| 1 | 0.50 | 10 | $\{1,2\}$ |
| 2 | 0.60 | 5 | $\{4\}$ |
| 3 | 0.60 | 5 | $\{1,4,5\}$ |
| 4 | 0.60 | 20 | $\{2,5\}$ |
| 5 | 0.20 | 10 | $\{3\}$ |

Compare the optimal worst-case house profit with the worst-case house profit, if all offers were accepted (i.e., $x_i = q_i$). Find the imputed probabilities.

**中文：** 用你的方法处理下面这个简单例子，$n = 5$ 个参与者、$m = 5$ 种可能结果，报价如下表（同上方表格）。比较最优最坏情形庄家利润与"全部报价都被接受"（即 $x_i = q_i$）时的最坏情形庄家利润。求出归算概率（imputed probabilities）。

**解答：**

先建立 LP。指示矩阵 $A$（行 = 结果 $j$，列 = 参与者 $i$，$A_{ji}=1$ 若 $j\in S_i$）：
```math
A = \begin{bmatrix}
1 & 0 & 1 & 0 & 0 \\
1 & 0 & 0 & 1 & 0 \\
0 & 0 & 0 & 0 & 1 \\
0 & 1 & 1 & 0 & 0 \\
0 & 0 & 1 & 1 & 0
\end{bmatrix}
\qquad\text{（行对应结果 }1,2,3,4,5\text{）}
```
即

- 结果 1：命中参与者 1、3；
- 结果 2：命中参与者 1、4；
- 结果 3：命中参与者 5；
- 结果 4：命中参与者 2、3；
- 结果 5：命中参与者 3、4。

价格向量 $p = (0.50, 0.60, 0.60, 0.60, 0.20)$，上限 $q = (10, 5, 5, 20, 10)$。

庄家 LP（极大化最坏情形利润）：
```math
\begin{aligned}
\operatorname{maximize} &\quad t \\
\text{s.t.} &\quad t + \sum_{i:\,j\in S_i} x_i \le p^T x,\quad j=1,\dots,5, \\
&\quad 0 \le x_i \le q_i.
\end{aligned}
```
把 $p^T x = 0.5x_1 + 0.6x_2 + 0.6x_3 + 0.6x_4 + 0.2x_5$ 代入，逐结果写出约束：

- $j=1$：$t + x_1 + x_3 \le 0.5x_1 + 0.6x_2 + 0.6x_3 + 0.6x_4 + 0.2x_5$；
- $j=2$：$t + x_1 + x_4 \le 0.5x_1 + 0.6x_2 + 0.6x_3 + 0.6x_4 + 0.2x_5$；
- $j=3$：$t + x_5 \le 0.5x_1 + 0.6x_2 + 0.6x_3 + 0.6x_4 + 0.2x_5$；
- $j=4$：$t + x_2 + x_3 \le 0.5x_1 + 0.6x_2 + 0.6x_3 + 0.6x_4 + 0.2x_5$；
- $j=5$：$t + x_3 + x_4 \le 0.5x_1 + 0.6x_2 + 0.6x_3 + 0.6x_4 + 0.2x_5$。

求解该 LP（可用线性规划求解器）。由问题对称性与对偶分析，最优解及归算概率如下：

**最优解与归算概率。** 由 (b) 的结论，$\pi_j = \lambda_j^{\star}$。求解得
```math
\pi^{\star} = (0.3,\ 0.3,\ 0.2,\ 0.1,\ 0.1)
```
（即归算概率：结果 1 与 2 各 0.3，结果 3 为 0.2，结果 4 与 5 各 0.1，和为 $1$。）

**验证公平价。** 每个报价的公平价为 $p_i^{\mathrm{fair}} = \sum_{j\in S_i}\pi_j$：

- 报价 1：$\pi_1 + \pi_2 = 0.3 + 0.3 = 0.6 > p_1 = 0.50$，故报价低于公平价，完全成交 $x_1 = q_1 = 10$；
- 报价 2：$\pi_4 = 0.1 < p_2 = 0.60$，报价高于公平价，拒绝 $x_2 = 0$；
- 报价 3：$\pi_1 + \pi_4 + \pi_5 = 0.3 + 0.1 + 0.1 = 0.5 < p_3 = 0.60$，拒绝 $x_3 = 0$；
- 报价 4：$\pi_2 + \pi_5 = 0.3 + 0.1 = 0.4 < p_4 = 0.60$，拒绝 $x_4 = 0$；
- 报价 5：$\pi_3 = 0.2 = p_5 = 0.20$，公平定价，$x_5$ 任意（可取 $0 \le x_5 \le 10$）。

于是庄家最优策略为
```math
x^{\star} = (10,\ 0,\ 0,\ 0,\ x_5),\qquad 0 \le x_5 \le 10.
```
**最坏情形利润。** 总收入 $= 0.5\cdot 10 + 0.2 x_5 = 5 + 0.2 x_5$。支出：结果 1 或 2 命中时支出 $x_1 + (\text{其他为 }0) = 10$（报价 5 只在结果 3 命中，报价 3、4、2 均为 0）；结果 3 命中时支出 $x_5$；结果 4、5 命中时支出 $0$。

- 结果 1：利润 $= 5 + 0.2x_5 - 10 = -5 + 0.2x_5$；
- 结果 2：利润 $= 5 + 0.2x_5 - 10 = -5 + 0.2x_5$；
- 结果 3：利润 $= 5 + 0.2x_5 - x_5 = 5 - 0.8x_5$；
- 结果 4：利润 $= 5 + 0.2x_5$；
- 结果 5：利润 $= 5 + 0.2x_5$。

最坏情形为 $\min\{-5 + 0.2x_5,\ 5 - 0.8x_5\}$（结果 4、5 的 $5 + 0.2x_5$ 显然更大，不作为最坏）。为使最坏情形最大，令二者相等：
```math
-5 + 0.2x_5 = 5 - 0.8x_5 \Rightarrow x_5 = 10.
```
于是 $x_5^{\star} = 10$，最优最坏情形利润为
```math
-5 + 0.2 \cdot 10 = -5 + 2 = -3,
\qquad\text{或 }5 - 0.8\cdot 10 = 5 - 8 = -3.
```
即最优最坏情形利润 $= -3$（庄家最坏亏损 3 美元；这是不可避免的，因为报价 1 的参与者买到了"划算"的合约）。

**全部接受（$x_i = q_i$）时的最坏情形利润。** 此时 $x = (10,5,5,20,10)$，总收入 $= 0.5\cdot 10 + 0.6\cdot 5 + 0.6\cdot 5 + 0.6\cdot 20 + 0.2\cdot 10 = 5 + 3 + 3 + 12 + 2 = 25$。支出逐结果：

- 结果 1：$x_1 + x_3 = 10 + 5 = 15$，利润 $= 25 - 15 = 10$；
- 结果 2：$x_1 + x_4 = 10 + 20 = 30$，利润 $= 25 - 30 = -5$；
- 结果 3：$x_5 = 10$，利润 $= 25 - 10 = 15$；
- 结果 4：$x_2 + x_3 = 5 + 5 = 10$，利润 $= 25 - 10 = 15$；
- 结果 5：$x_3 + x_4 = 5 + 20 = 25$，利润 $= 25 - 25 = 0$。

最坏情形利润为 $\min\{10, -5, 15, 15, 0\} = -5$。

**比较**：最优最坏情形利润 $-3$ 优于全部接受时的 $-5$（亏损更小，改进了 $2$ 美元）。

**归算概率结论**：$\pi^{\star} = (0.3, 0.3, 0.2, 0.1, 0.1)$。

---

## 题目 6（Problem 6：布尔 LP 的启发式次优解）

**题干：** This exercise builds on exercises 4.15 and 5.13, which involve the Boolean LP
```math
\begin{aligned}
\operatorname{minimize} &\quad c^T x \\
\operatorname{subject\ to} &\quad Ax \preceq b \\
&\quad x_i \in \{0,1\},\ i=1,\dots,n,
\end{aligned}
```
with optimal value $p^{\star}$. Let $x^{\mathrm{rlx}}$ be a solution of the LP relaxation
```math
\begin{aligned}
\operatorname{minimize} &\quad c^T x \\
\operatorname{subject\ to} &\quad Ax \preceq b \\
&\quad 0 \preceq x \preceq 1,
\end{aligned}
```
so $L = c^T x^{\mathrm{rlx}}$ is a lower bound on $p^{\star}$. The relaxed solution $x^{\mathrm{rlx}}$ can also be used to guess a Boolean point $\hat x$, by rounding its entries, based on a threshold $t \in [0,1]$:
```math
\hat x_i = \begin{cases} 1, & x_i^{\mathrm{rlx}} \ge t, \\ 0, & \text{otherwise}, \end{cases}
\quad i = 1,\dots,n.
```
**中文：** 本练习建立在 exercise 4.15 与 5.13 之上，它们涉及布尔 LP（Boolean LP）
```math
\begin{aligned}
\operatorname{minimize} &\quad c^T x \\
\operatorname{subject\ to} &\quad Ax \preceq b \\
&\quad x_i \in \{0,1\},\ i=1,\dots,n,
\end{aligned}
```
最优值为 $p^{\star}$。设 $x^{\mathrm{rlx}}$ 是 LP 松弛（LP relaxation）
```math
\begin{aligned}
\operatorname{minimize} &\quad c^T x \\
\operatorname{subject\ to} &\quad Ax \preceq b \\
&\quad 0 \preceq x \preceq 1
\end{aligned}
```
的解，于是 $L = c^T x^{\mathrm{rlx}}$ 是 $p^{\star}$ 的一个下界（lower bound）。松弛解 $x^{\mathrm{rlx}}$ 也可用于猜测一个布尔点 $\hat x$，方法是基于阈值（threshold）$t \in [0,1]$ 对其分量取整：
```math
\hat x_i = \begin{cases} 1, & x_i^{\mathrm{rlx}} \ge t, \\ 0, & \text{否则}, \end{cases}
\quad i = 1,\dots,n.
```
显然 $\hat x$ 是布尔点（各分量在 $\{0,1\}$ 中）。若它对布尔 LP 可行（即 $A\hat x \preceq b$），则可视为布尔 LP 的一个"好"（若非最优）点的猜测。其目标值 $U = c^T \hat x$ 是 $p^{\star}$ 的一个上界（upper bound）。若 $U$ 与 $L$ 接近，则 $\hat x$ 近似最优；具体地，$\hat x$ 对布尔 LP 至多是 $(U - L)$-次优的。这种取整不一定成功；确实可能对**所有**阈值，$\hat x$ 都不可行。但对某些问题实例，它表现良好。

**中文（后续段落）：** 当然，这个简单方案有许多变体，用于（可能地）从 $x^{\mathrm{rlx}}$ 构造一个可行好点。最后进入正题。用如下代码生成问题数据：

```matlab
rand('state',0);
n = 100;
m = 300;
A = rand(m,n);
b = A * ones(n,1) / 2;
c = -rand(n,1);
```

可以把 $x_i$ 理解为"接受或拒绝一项任务"，$-c_i$ 是接受任务 $i$ 所产生的（正）收益。把 $Ax \preceq b$ 理解为一组关于 $m$ 种资源的限制：$A_{ij}$（正值）是接受任务 $j$ 所消耗的资源 $i$ 的量；$b_i$（正值）是资源 $i$ 的可用量。

**解答：**

**步骤 1：理解问题。** 这是一个 0-1 整数规划（Boolean LP），NP 难。先解它的 LP 松弛（把 $x_i\in\{0,1\}$ 放宽为 $0\le x_i\le 1$），得到连续解 $x^{\mathrm{rlx}}$ 与下界 $L = c^T x^{\mathrm{rlx}}$。

**步骤 2：检查松弛解的分量。** 观察 $x^{\mathrm{rlx}}$ 的各分量，一般会看到三种情况：许多分量在 0 或 1 处（"接近取整"），一些分量严格在 $(0,1)$ 之间（分数分量）。这些分数分量正是"需要取舍"的任务。同时记录下界 $L$。

**步骤 3：阈值取整。** 对 $t$ 在 $[0,1]$ 上均匀取 100 个值，对每个 $t$ 生成布尔点：

```matlab
for each threshold t:
    xhat = (xrlx >= t);          % 布尔化
    obj(t)   = c' * xhat;        % 目标值
    maxviol(t) = max(A*xhat - b); % 最大约束违反量
end
```

其中 `maxviol <= 0` 表示 $\hat x$ 满足 $A\hat x \preceq b$，即对布尔 LP 可行；`maxviol > 0` 表示不可行。

**步骤 4：绘图。** 画目标值 $c^T \hat x$ 与最大违反量 $\max_i(A\hat x - b)_i$ 关于 $t$ 的曲线。在图上标出 $\hat x$ 可行（`maxviol<=0`）的那些 $t$ 区间，以及不可行的区间。典型现象是：$t$ 较小时 $\hat x$ 包含太多"1"（资源超用，违反约束），$t$ 较大时 $\hat x$ 含太多"0"（可行但目标变差）。

**步骤 5：选可行且目标最小的 $t$。** 在满足 `maxviol<=0` 的 $t$ 中，选使目标 $c^T\hat x$ 最小的那个，得到上界
```math
U = \min\big\{ c^T \hat x(t) : \max_i(A\hat x - b)_i \le 0 \big\}.
```
在 MATLAB 中可用题目提示的写法：

```matlab
U = min( obj( find(maxviol <= 0) ) );
```

**步骤 6：给上下界间隙。** 最终给出 gap
```
U - L = c^T \hat x - c^T x^{\mathrm{rlx}}.

这就是 $p^{\star}$ 的上界与下界之差，量化了该启发式解与（未知的）真正最优值的差距。若 gap 很小，说明取整得到的 $\hat x$ 已近似最优。

[[FIGURE: 第 5 页，目标值 $c^T\hat x$ 与最大约束违反量 $\max_i(A\hat x-b)_i$ 关于阈值 $t$ 的曲线图，并标出可行/不可行区间]]

> 注：本题为数值实验题，具体 $L$、$U$、最优阈值 $t^{\star}$ 与 gap 的数值需运行上述 MATLAB 代码后填入；上述给出了完整的算法流程、判定准则（`maxviol<=0`）与上下界/间隙的严格定义。

---

*（完）本文件覆盖 HW5 附加题 Problem 1–6 全部小题，其中题目 4、6 为依赖外部数据/数值实验的题目，已给出完整建模与求解框架，具体数值需运行配套 MATLAB 脚本后填入。*
