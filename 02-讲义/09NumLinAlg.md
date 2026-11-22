> <span style="color:#7f8c8d;">Source: SEE官方讲义/09NumLinAlg.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

## <span style="color:#c0392b;">**9. Numerical linear algebra background（数值线性代数背景）**</span>

<!-- p.1 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**第 9 讲：数值线性代数背景（Numerical linear algebra background）**</span>

本讲（Convex Optimization — Boyd & Vandenberghe）为后续各章的优化算法准备数值基础，主题包括：

- matrix structure and algorithm complexity（矩阵结构与算法复杂度）
- solving linear equations with factored matrices（用已分解的矩阵求解线性方程组）
- LU factorization（LU 分解）、Cholesky factorization（Cholesky 分解）与 $LDL^T$ factorization（$LDL^T$ 分解）
- block elimination and the matrix inversion lemma（块消元与矩阵求逆引理）
- solving underdetermined equations（求解欠定方程组）

<!-- p.2 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Matrix structure and algorithm complexity（矩阵结构与算法复杂度）**</span>

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">**求解 $Ax = b$（$A \in \mathbb{R}^{n\times n}$）的执行成本（cost, execution time）**</span>：对一般方法（general methods），成本按 $n^3$ 增长；若 $A$ 是 structured（有结构的）——banded（带状）、sparse（稀疏）、Toeplitz 等——则成本更低。

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">**flop 计数（flop counts）**</span>：

- flop（floating-point operation，浮点运算）：对两个浮点数做一次加法、减法、乘法或除法。
- 估计算法复杂度（complexity）的办法：把 flop 数表示为问题维度的（多项式）函数，再只保留 leading terms（首项）化简。
- flop 计数并不是现代计算机上运行时间的精确预测器，但作为复杂度的粗略估计很有用。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> flop 计数与你在 6.006 里学的渐进复杂度分析是同一套思想的两层皮：6.006 数的是基本操作的次数并用大 O 化简，这里把基本操作具体化为一次浮点四则运算，同样"只保留首项"——$(2/3)n^3 + 2n^2$ 直接写成 $(2/3)n^3$，只不过数值软件还会保留常数系数，因为要据此估算真实运行时间。但 CSAPP 提醒过我们纸面模型的边界：现代 CPU 有流水线、超标量发射与 SIMD 向量化，一次乘法并不对应固定的周期数，而一次缓存未命中就可能比几十次 flop 还贵，所以讲义强调 flop 计数"不是运行时间的精确预测器"。$2n-1$ 与 $2n$ 的差别也值得一提：内积是 $n$ 次乘法加 $n-1$ 次加法，$n$ 大时差一项无关紧要，这正是"首项思维"的日常版本。本讲后面所有算法的账本（LU 的 $(2/3)n^3$、Cholesky 的 $(1/3)n^3$）都用这个单位记账，读后续章节时你应能条件反射地把它翻译成大 O。

<!-- p.3 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**向量与矩阵运算的 flop 计数**</span>

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">**向量—向量运算（vector-vector operations，$x, y \in \mathbb{R}^n$）**</span>：

- 内积 $x^Ty$：$2n-1$ 次 flop（$n$ 大时记 $2n$）。
- 求和 $x + y$、标量乘法 $\alpha x$：各 $n$ 次 flop。

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">**矩阵—向量乘积 $y = Ax$（$A \in \mathbb{R}^{m\times n}$）**</span>：

- $m(2n-1)$ 次 flop（$n$ 大时记 $2mn$）。
- 若 $A$ 稀疏且非零元共 $N$ 个：$2N$ 次 flop。
- 若 $A$ 以 $A = UV^T$ 的形式给出（$U \in \mathbb{R}^{m\times p}$，$V \in \mathbb{R}^{n\times p}$）：$2p(n+m)$ 次 flop。

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">**矩阵—矩阵乘积 $C = AB$（$A \in \mathbb{R}^{m\times n}$，$B \in \mathbb{R}^{n\times p}$）**</span>：

- $mp(2n-1)$ 次 flop（$n$ 大时记 $2mnp$）。
- $A$、$B$ 之一或两者稀疏时更省。
- 当 $m = p$ 且 $C$ 对称：$(1/2)m(m+1)(2n-1) \approx m^2n$ 次 flop。

<!-- p.4 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Linear equations that are easy to solve（容易求解的线性方程组）**</span>

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">**对角矩阵（diagonal matrices，$i \neq j$ 时 $a_{ij} = 0$）：$n$ 次 flop。**</span>

```math
x = A^{-1}b = (b_1/a_{11},\ \ldots,\ b_n/a_{nn})
```

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">**下三角矩阵（lower triangular，$j > i$ 时 $a_{ij} = 0$）：$n^2$ 次 flop。**</span>

```math
\begin{aligned}
x_1 &:= b_1/a_{11} \\
x_2 &:= (b_2 - a_{21}x_1)/a_{22} \\
x_3 &:= (b_3 - a_{31}x_1 - a_{32}x_2)/a_{33} \\
&\ \ \vdots \\
x_n &:= (b_n - a_{n1}x_1 - a_{n2}x_2 - \cdots - a_{n,n-1}x_{n-1})/a_{nn}
\end{aligned}
```

这一过程称为 forward substitution（前向代入）。

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">**上三角矩阵（upper triangular，$j < i$ 时 $a_{ij} = 0$）：$n^2$ 次 flop**</span>，通过 backward substitution（回代）求解。

<!-- p.5 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Orthogonal and permutation matrices（正交矩阵与置换矩阵）**</span>

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">**正交矩阵（orthogonal matrices）：$A^{-1} = A^T$。**</span>

- 对一般的 $A$，计算 $x = A^Tb$ 需 $2n^2$ 次 flop。
- 有结构时更省：例如 $A = I - 2uu^T$（其中 $\|u\|_2 = 1$），可以在 $4n$ 次 flop 内算出 $x = A^Tb = b - 2(u^Tb)u$。

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">**置换矩阵（permutation matrices）**</span>：$a_{ij} = 1$（当 $j = \pi_i$），否则 $a_{ij} = 0$，其中 $\pi = (\pi_1, \pi_2, \ldots, \pi_n)$ 是 $(1, 2, \ldots, n)$ 的一个 permutation（置换）。

- 解释：$Ax = (x_{\pi_1}, \ldots, x_{\pi_n})$，即乘 $A$ 只是把 $x$ 的分量重新排列。
- 满足 $A^{-1} = A^T$，因此求解 $Ax = b$ 的成本为 $0$ 次 flop。

<span style="color:#2471a3;">**[example]**</span> 例子：

```math
A = \begin{bmatrix} 0 & 1 & 0 \\ 0 & 0 & 1 \\ 1 & 0 & 0 \end{bmatrix}, \qquad
A^{-1} = A^T = \begin{bmatrix} 0 & 0 & 1 \\ 1 & 0 & 0 \\ 0 & 1 & 0 \end{bmatrix}
```

<!-- p.6 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**The factor-solve method for solving $Ax = b$（求解 $Ax=b$ 的"分解—求解"方法）**</span>

<span style="color:#2471a3;">**[algorithm]**</span> 核心思想分两步：

1. **分解（factor）**：把 $A$ 分解为少数（通常 2 或 3 个）简单矩阵的乘积 $A = A_1A_2\cdots A_k$（各 $A_i$ 为对角阵、上三角或下三角阵等）；
2. **求解（solve）**：通过解 $k$ 个"容易"的方程计算 $x = A^{-1}b = A_k^{-1}\cdots A_2^{-1}A_1^{-1}b$，即依次解 $A_1x_1 = b$，$A_2x_2 = x_1$，…，$A_kx = x_{k-1}$。

分解步骤的成本通常主导（dominates）求解步骤的成本。

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">**多右端项（multiple righthand sides）**</span>：要解 $Ax_1 = b_1$，$Ax_2 = b_2$，…，$Ax_m = b_m$，成本为一次分解加 $m$ 次求解。

<!-- p.7 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**LU factorization（LU 分解）**</span>

<span style="color:#2471a3;">**[theorem]**</span> <span style="color:#00838f;">**每个 nonsingular（非奇异）矩阵 $A$ 都可以分解为 $A = PLU$，其中 $P$ 是置换矩阵，$L$ 是下三角矩阵，$U$ 是上三角矩阵。**</span> 成本：$(2/3)n^3$ 次 flop。

<span style="color:#2471a3;">**[algorithm]**</span> <span style="color:#c0392b;">**Solving linear equations by LU factorization（用 LU 分解求解线性方程组）**</span>：给定线性方程组 $Ax = b$，$A$ 非奇异。

1. **LU 分解**：把 $A$ 分解为 $A = PLU$（$(2/3)n^3$ 次 flop）。
2. **置换（Permutation）**：解 $Pz_1 = b$（$0$ 次 flop）。
3. **前向代入（Forward substitution）**：解 $Lz_2 = z_1$（$n^2$ 次 flop）。
4. **回代（Backward substitution）**：解 $Ux = z_2$（$n^2$ 次 flop）。

成本：$(2/3)n^3 + 2n^2 \approx (2/3)n^3$（$n$ 大时）。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> $A = PLU$ 就是 Strang 18.06 里 Gauss 消元的"记账版"：消元时每行乘的倍数收进 $L$，消完的上三角余量就是 $U$；课堂上为了叙述干净常假设"主元非零、不必换行"，而这里 $P$ 的出现说明实际算法要做行交换——即选主元（pivoting）。为什么必须选主元？纸面上只要非零就行，但 CSAPP 讲过浮点数是实数的离散近似，舍入误差无处不在：若某行主元绝对值极小，拿它当除数会把相对误差放大到灾难性的程度，甚至把本可解的方程组"解"得面目全非，所以 LAPACK 这类库默认做部分主元选取。这也划出了"纸上线代"与"数值线代"的分界：后者对每个算法同时问两个问题——flop 账本是否漂亮，误差是否被放大（数值稳定性）。后面 Newton 法解 KKT 系统时，走的正是"先分解、后回代"这套流程。

<!-- p.8 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Sparse LU factorization（稀疏 LU 分解）**</span>

<span style="color:#2471a3;">**[algorithm]**</span> 稀疏情形采用 $A = P_1LUP_2$：

- 引入第二个置换矩阵 $P_2$，使 $L$、$U$ 有机会更稀疏（从而分解与求解步骤都更便宜）；
- $P_1$ 与 $P_2$ 按（启发式，heuristically）原则选取，以产生稀疏的 $L$、$U$；
- $P_1$、$P_2$ 的选取依赖于 $A$ 的 sparsity pattern（稀疏模式）与 $A$ 的具体数值；
- 成本通常远低于 $(2/3)n^3$；精确值以复杂的方式依赖于 $n$、$A$ 中零元的个数以及稀疏模式。

<!-- p.9 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Cholesky factorization（Cholesky 分解）**</span>

<span style="color:#2471a3;">**[theorem]**</span> <span style="color:#00838f;">**每个 positive definite（正定）矩阵 $A$ 都可以分解为 $A = LL^T$，其中 $L$ 是下三角矩阵。**</span> 成本：$(1/3)n^3$ 次 flop。

<span style="color:#2471a3;">**[algorithm]**</span> <span style="color:#c0392b;">**Solving linear equations by Cholesky factorization（用 Cholesky 分解求解线性方程组）**</span>：给定线性方程组 $Ax = b$，$A \in \mathbf{S}^n_{++}$（对称正定矩阵全体）。

1. **Cholesky 分解**：把 $A$ 分解为 $A = LL^T$（$(1/3)n^3$ 次 flop）。
2. **前向代入（Forward substitution）**：解 $Lz_1 = b$（$n^2$ 次 flop）。
3. **回代（Backward substitution）**：解 $L^Tx = z_1$（$n^2$ 次 flop）。

成本：$(1/3)n^3 + 2n^2 \approx (1/3)n^3$（$n$ 大时）。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> Cholesky 分解是正定性送来的"结构红利"：成本 $(1/3)n^3$ 恰是 LU 分解 $(2/3)n^3$ 的一半，而且完全不需要置换矩阵——正定保证每个主元都为正，消元天然稳定，无须选主元。你在 Strang 18.06 里判别正定靠"顺序主子式全为正"或"特征值全为正"；数值实践恰恰反过来：直接对 $A$ 做 Cholesky 分解，过程中一旦要对负数开平方，就判定 $A$ 不正定——分解本身就是最快的正定性测试。这解释了本课程后续的一个固定套路：遇到 $\nabla^2 f$ 或 KKT 矩阵时，"能否 Cholesky"几乎是"能否放心使用"的同义词。从逐列计算的角度看，每个 $l_{ij}$ 只依赖已算出的列，这直观解释了它为何省一半功夫；18.065 里 Strang 还把 $A = LL^T$ 与最小二乘（least-squares）正规方程、能量函数 $\|x\|^2$ 联系起来。

<!-- p.10 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Sparse Cholesky factorization（稀疏 Cholesky 分解）**</span>

<span style="color:#2471a3;">**[algorithm]**</span> 稀疏情形采用 $A = PLL^TP^T$：

- 引入置换矩阵 $P$，使 $L$ 有机会更稀疏；
- $P$ 按（启发式）原则选取，以产生稀疏的 $L$；
- 与稀疏 LU 不同，$P$ 的选取只依赖于 $A$ 的稀疏模式（与 $A$ 的具体数值无关）；
- 成本通常远低于 $(1/3)n^3$；精确值以复杂的方式依赖于 $n$、$A$ 中零元的个数以及稀疏模式。

<!-- p.11 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**$LDL^T$ factorization（$LDL^T$ 分解）**</span>

<span style="color:#2471a3;">**[theorem]**</span> <span style="color:#00838f;">**每个非奇异的 symmetric（对称）矩阵 $A$ 都可以分解为 $A = PLDL^TP^T$，其中 $P$ 是置换矩阵，$L$ 是下三角矩阵，$D$ 是 block diagonal（分块对角）矩阵且对角块为 $1\times 1$ 或 $2\times 2$。**</span> 成本：$(1/3)n^3$ 次 flop。

- 用 $LDL^T$ 分解求解对称线性方程组的成本：$(1/3)n^3 + 2n^2 \approx (1/3)n^3$（$n$ 大时）；
- 对稀疏的 $A$，可选取 $P$ 使 $L$ 稀疏；成本 $\ll (1/3)n^3$。

<!-- p.12 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Equations with structured sub-blocks（子块有结构的方程组）**</span>

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">**考察分块方程组（1）**</span>：

```math
\begin{bmatrix} A_{11} & A_{12} \\ A_{21} & A_{22} \end{bmatrix}
\begin{bmatrix} x_1 \\ x_2 \end{bmatrix} =
\begin{bmatrix} b_1 \\ b_2 \end{bmatrix} \qquad \text{(1)}
```

- 变量 $x_1 \in \mathbb{R}^{n_1}$，$x_2 \in \mathbb{R}^{n_2}$；块 $A_{ij} \in \mathbb{R}^{n_i\times n_j}$。
- 若 $A_{11}$ 非奇异，可以消去 $x_1$：$x_1 = A_{11}^{-1}(b_1 - A_{12}x_2)$；于是求 $x_2$ 归结为解

```math
(A_{22} - A_{21}A_{11}^{-1}A_{12})\,x_2 = b_2 - A_{21}A_{11}^{-1}b_1
```

<span style="color:#2471a3;">**[algorithm]**</span> <span style="color:#c0392b;">**Solving linear equations by block elimination（用块消元求解线性方程组）**</span>：给定非奇异的方程组（1），且 $A_{11}$ 非奇异。

1. 计算 $A_{11}^{-1}A_{12}$ 与 $A_{11}^{-1}b_1$；
2. 计算 $S = A_{22} - A_{21}A_{11}^{-1}A_{12}$ 与 $\tilde{b} = b_2 - A_{21}A_{11}^{-1}b_1$；
3. 解 $Sx_2 = \tilde{b}$ 得到 $x_2$；
4. 解 $A_{11}x_1 = b_1 - A_{12}x_2$ 得到 $x_1$。

<!-- p.13 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**块消元 flop 计数中的主导项**</span>

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">**各步骤的主导项（dominant terms in flop count）**</span>：

- 第 1 步：$f + n_2s$（$f$ 是分解 $A_{11}$ 的成本；$s$ 是一次求解步骤的成本）；
- 第 2 步：$2n_2^2n_1$（成本由 $A_{21}$ 与 $A_{11}^{-1}A_{12}$ 的乘积主导）；
- 第 3 步：$(2/3)n_2^3$；
- 总计：$f + n_2s + 2n_2^2n_1 + (2/3)n_2^3$。

<span style="color:#2471a3;">**[example]**</span> 例子：

- 一般的 $A_{11}$（$f = (2/3)n_1^3$，$s = 2n_1^2$）：相对标准方法没有收益——

```math
\#\,\mathrm{flops} = (2/3)n_1^3 + 2n_1^2n_2 + 2n_2^2n_1 + (2/3)n_2^3 = (2/3)(n_1+n_2)^3
```

- 块消元只有当 $A_{11}$ 有结构（$f \ll n_1^3$）时才划算。例如 $A_{11}$ 为对角阵（$f = 0$，$s = n_1$）时：$\#\,\mathrm{flops} \approx 2n_2^2n_1 + (2/3)n_2^3$。

<!-- p.14 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Structured matrix plus low rank term（结构化矩阵加低秩项）**</span>

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">**求解 $(A + BC)x = b$**</span>，其中 $A \in \mathbb{R}^{n\times n}$，$B \in \mathbb{R}^{n\times p}$，$C \in \mathbb{R}^{p\times n}$；假设 $A$ 有结构（即 $Ax = b$ 容易解）。

先把它写成

```math
\begin{bmatrix} A & B \\ C & -I \end{bmatrix}
\begin{bmatrix} x \\ y \end{bmatrix} =
\begin{bmatrix} b \\ 0 \end{bmatrix}
```

再用块消元：先解 $(I + CA^{-1}B)y = CA^{-1}b$，然后解 $Ax = b - By$。

<span style="color:#2471a3;">**[theorem]**</span> 这就证明了 matrix inversion lemma（矩阵求逆引理）：若 $A$ 与 $A + BC$ 都非奇异，则

```math
(A + BC)^{-1} = A^{-1} - A^{-1}B\,(I + CA^{-1}B)^{-1}CA^{-1}
```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这页推导值得逐行咀嚼：引入辅助变量 $y = Cx$，把 $(A+BC)x = b$ 塞进一个更大的分块系统，再用上一页的块消元，"低秩项" $BC$ 就被隔离进小方程 $(I + CA^{-1}B)y = CA^{-1}b$ 里。矩阵求逆引理在 Strang 18.065 里叫 Woodbury 公式（$p = 1$ 时即 Sherman–Morrison 公式），形式上有点唬人，但工程含义一句话就能说清：当 $A$ 结构良好、而 $A + BC$ 只是被秩 $p$ 的扰动破坏了结构时，永远不要显式构造并求逆 $A + BC$，而应围绕 $A$ 解几个结构化的方程组。下一页的数字对比会把这句话变成真金白银：$n$ 大、$p$ 小时，成本从 $O(n^3)$ 降到关于 $n$ 线性。这个引理后续还会出现在递归最小二乘、Kalman 滤波与拟牛顿更新（BFGS）等场合，本质都是"低秩更新，别整体重算"。

<!-- p.15 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Example: A diagonal, B, C dense（例：$A$ 为对角阵，$B$、$C$ 稠密）**</span>

<span style="color:#2471a3;">**[example]**</span> 两种解法对比：

- 方法 1：先算 $D = A + BC$，再解 $Dx = b$。成本：$(2/3)n^3 + 2pn^2$。
- 方法 2（经矩阵求逆引理）：先解方程（2）

```math
(I + CA^{-1}B)\,y = CA^{-1}b \qquad \text{(2)}
```

再计算 $x = A^{-1}b - A^{-1}By$。总成本由（2）主导：$2p^2n + (2/3)p^3$（即关于 $n$ 线性）。

<!-- p.16 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Underdetermined linear equations（欠定线性方程组）**</span>

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">**若 $A \in \mathbb{R}^{p\times n}$，$p < n$，$\operatorname{rank} A = p$，则**</span>

```math
\{\,x \mid Ax = b\,\} = \{\,Fz + \hat{x} \mid z \in \mathbb{R}^{n-p}\,\}
```

- $\hat{x}$ 是（任意）一个 particular solution（特解）；
- $F \in \mathbb{R}^{n\times(n-p)}$ 的列张成 $A$ 的 nullspace（零空间）；
- 计算这样的 $F$ 有若干数值方法（QR factorization（QR 分解）、rectangular LU factorization（矩形 LU 分解）等）。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 解集 $\{Fz + \hat{x}\}$ 正是 Strang 18.06 中"完整解 = 特解 + 齐次通解"的矩阵化重述：$\hat{x}$ 是 $Ax = b$ 的任一特解，$F$ 的列张成零空间 $\mathcal{N}(A)$，两者的线性组合扫出全部解。当年你用行最简形手工找这两个部件，这里换成数值版本——如何稳定、便宜地算出 $F$，答案是用 QR 分解或矩形 LU 等因子化方法，而不是显式求伪逆。维数账与四个基本子空间定理完全一致：$\operatorname{rank}A = p$，零空间维数为 $n - p$，自由参数 $z$ 恰好 $n - p$ 个。这一页也是第 1 章处理欠定 LP、消去等式约束的数值后盾：把通解代入目标函数、化为无约束问题时，用的正是这套表示；而 $F$ 的列在浮点运算下是否真正"数值线性无关"，又回到 LU 一节译者注谈过的稳定性主题。