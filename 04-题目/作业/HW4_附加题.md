# EE364a HW4_bonus 解答（作业 4 附加题 解答）

## 题目 1（Problem 1）

**题干：** Minimizing a function over the probability simplex. Find simple necessary and sufficient conditions for $x\in\mathbf{R}^n$ to minimize a differentiable convex function $f$ over the probability simplex, $\{x \mid \mathbf{1}^T x = 1,\ x \succeq 0\}$.

**中文：** 在概率单纯形（probability simplex）上极小化一个函数。求 $x\in\mathbf{R}^n$ 在概率单纯形 $\{x \mid \mathbf{1}^T x = 1,\ x \succeq 0\}$ 上极小化一个可微凸函数（differentiable convex function）$f$ 的简单充分必要条件。

**解答：**

记概率单纯形为 $C=\{x \mid \mathbf{1}^T x = 1,\ x \succeq 0\}$，其中 $x\succeq 0$ 表示 $x$ 的分量非负。问题为

```math
\begin{aligned}
& \text{minimize} & & f(x) \\
& \text{subject to} & & \mathbf{1}^T x = 1 \\
& & & x \succeq 0,
\end{aligned}
```

其中 $f$ 可微且凸。由于目标函数 $f$ 凸、约束集 $C$ 是仿射等式与分量不等式构成的凸集，这是一个凸优化问题。设 $f$ 的梯度为 $\nabla f(x)$，第 $i$ 个分量为 $(\nabla f(x))_i$。

对凸问题，一阶最优性条件是最优性的充分必要条件。$x^\star$ 为最优解的充要条件是存在 $\nu\in\mathbf{R}$（等式约束的对偶变量）与 $\lambda\in\mathbf{R}^n$、$\lambda\succeq 0$（不等式约束的对偶变量）满足 KKT 条件。先写出 Lagrangian：

```math
L(x,\nu,\lambda)=f(x)+\nu(\mathbf{1}^T x-1)-\lambda^T x,
```

其中 $\lambda\succeq 0$（因为约束 $x\succeq 0$ 写成 $-x\preceq 0$，对偶变量 $\lambda\succeq 0$，符号取负）。对 $x$ 求梯度并令为零，得驻点条件（stationarity）：

```math
\nabla f(x^\star)+\nu\mathbf{1}-\lambda=0
\quad\Longrightarrow\quad
\lambda=\nabla f(x^\star)+\nu\mathbf{1}.
```

互补松弛（complementary slackness）条件要求对每个 $i$：

```math
\lambda_i x_i^\star = 0,\qquad i=1,\dots,n.
```

结合 $\lambda_i = (\nabla f(x^\star))_i+\nu$，代入互补松弛得

```math
x_i^\star \bigl((\nabla f(x^\star))_i+\nu\bigr)=0,\qquad i=1,\dots,n.
```

同时 $\lambda\succeq 0$ 给出

```math
(\nabla f(x^\star))_i+\nu \ge 0,\qquad i=1,\dots,n,
```

即对一切 $i$，$(\nabla f(x^\star))_i \ge -\nu$。由于这里是凸问题（目标凸、约束集为凸多面体且 Slater 条件成立——例如取 $x=(1/n,\dots,1/n)$ 严格可行），强对偶成立，KKT 条件与最优性等价。

综合上述，$x^\star$ 是问题最优解的充分必要条件可以简洁表述为：

```math
\nabla f(x^\star) \succeq -\nu\mathbf{1},\qquad
x_i^\star\bigl((\nabla f(x^\star))_i+\nu\bigr)=0,\quad \forall i,
```

且 $x^\star$ 满足 $\mathbf{1}^T x^\star=1$、$x^\star\succeq 0$。

用更口语化的几何解释：在概率单纯形上，$f$ 的极小值点处，$\nabla f$ 的所有分量必须相等（设为公共值 $\mu$），因为沿单纯形内部的可行方向（保持 $\mathbf{1}^T x=1$ 的方向）移动时，一阶变分为零要求 $\nabla f$ 与所有这样的方向正交，而 $C$ 的切空间正是 $\{d\mid \mathbf{1}^T d=0\}$，其正交补是 $\operatorname{span}\{\mathbf{1}\}$，故要求 $\nabla f(x^\star)\in\operatorname{span}\{\mathbf{1}\}$，即梯度分量全部相同。具体写为

```math
(\nabla f(x^\star))_i =
\begin{cases}
\mu, & x_i^\star>0,\\
\mu+\lambda_i,\ \lambda_i\ge 0, & x_i^\star=0,
\end{cases}
```

其中第一支来自互补松弛（$x_i^\star>0\Rightarrow \lambda_i=0\Rightarrow(\nabla f)_i=\mu$），第二支来自 $\lambda_i=(\nabla f)_i-\mu\ge 0$，即 $x_i^\star=0$ 时要求 $(\nabla f)_i\ge\mu$。

因此最终的**简单充要条件**为：存在 $\mu\in\mathbf{R}$，使得

```math
\forall i:\ (\nabla f(x^\star))_i\ge\mu,
\qquad\text{且}\qquad
x_i^\star>0 \Rightarrow (\nabla f(x^\star))_i=\mu,
\qquad
\mathbf{1}^T x^\star=1,\ x^\star\succeq 0.
```

即：在 $x^\star$ 取正值的坐标上，梯度分量全部等于同一个值 $\mu$；在取零的坐标上，梯度分量不小于 $\mu$。

---

## 题目 2（Problem 2）

**题干：** Complex least-norm problem. We consider the complex least $\ell_p$-norm problem

```math
\text{minimize}\ \|x\|_p \quad\text{subject to}\ Ax=b,
```

where $A\in\mathbf{C}^{m\times n}$, $b\in\mathbf{C}^m$, and the variable is $x\in\mathbf{C}^n$. Here $\|\cdot\|_p$ denotes the $\ell_p$-norm on $\mathbf{C}^n$, defined as $\|x\|_p=\bigl(\sum_{i=1}^n |x_i|^p\bigr)^{1/p}$ for $p\ge 1$, and $\|x\|_\infty=\max_{i=1,\dots,n}|x_i|$. We assume $A$ is full rank, and $m\lt n$.

(a) Formulate the complex least $\ell_2$-norm problem as a least $\ell_2$-norm problem with real problem data and variable. Hint. Use $z=(\Re x,\Im x)\in\mathbf{R}^{2n}$ as the variable.

(b) Formulate the complex least $\ell_\infty$-norm problem as an SOCP.

(c) Solve a random instance of both problems with $m=30$ and $n=100$. ... Use the Matlab command `scatter` to plot the optimal solutions ... and comment (briefly) on what you observe.

**中文：** 复最小范数问题（complex least-norm problem）。考虑复最小 $\ell_p$-范数问题 $\text{minimize}\ \|x\|_p \text{ subject to } Ax=b$，其中 $A\in\mathbf{C}^{m\times n}$、$b\in\mathbf{C}^m$，变量为 $x\in\mathbf{C}^n$。$\|\cdot\|_p$ 表示 $\mathbf{C}^n$ 上的 $\ell_p$-范数，定义为 $\|x\|_p=\bigl(\sum_{i=1}^n |x_i|^p\bigr)^{1/p}$（$p\ge 1$），$\|x\|_\infty=\max_{i=1,\dots,n}|x_i|$。假设 $A$ 满秩且 $m\lt n$。

(a) 把复最小 $\ell_2$-范数问题化为实数据、实变量的最小 $\ell_2$-范数问题。提示：取 $z=(\Re x,\Im x)\in\mathbf{R}^{2n}$ 为变量。

(b) 把复最小 $\ell_\infty$-范数问题化为二阶锥规划（SOCP, Second-Order Cone Program）。

(c) 对 $m=30$、$n=100$ 的随机实例求解两个问题……用 Matlab 命令 `scatter` 在复平面上画出两个问题的最优解，并（简要）评论观察到的现象。

**解答：**

**(a) 复最小 $\ell_2$-范数问题 → 实最小 $\ell_2$-范数问题。**

记 $x=u+iv$，其中 $u=\Re x\in\mathbf{R}^n$、$v=\Im x\in\mathbf{R}^n$。定义实变量

```math
z=\begin{pmatrix}u\\ v\end{pmatrix}\in\mathbf{R}^{2n}.
```

目标函数：对复向量 $x$，其 $\ell_2$-范数满足

```math
\|x\|_2^2=\sum_{i=1}^n |x_i|^2=\sum_{i=1}^n\bigl(u_i^2+v_i^2\bigr)=\|z\|_2^2.
```

即 $\|x\|_2=\|z\|_2$，模长逐分量对应。

约束条件：同样拆分 $A=A_r+iA_i$、$b=b_r+i b_i$，其中 $A_r,A_i\in\mathbf{R}^{m\times n}$，$b_r,b_i\in\mathbf{R}^m$。等式 $Ax=b$ 展开为

```math
(A_r+iA_i)(u+iv)=b_r+i b_i,
```

展开左边得 $(A_r u-A_i v)+i(A_i u+A_r v)$，令实部、虚部分别相等：

```math
A_r u-A_i v=b_r,\qquad A_i u+A_r v=b_i.
```

写成关于 $z$ 的实方程组：

```math
\widetilde{A}z=\tilde b,\qquad
\widetilde{A}=\begin{pmatrix}A_r & -A_i\\ A_i & A_r\end{pmatrix}\in\mathbf{R}^{2m\times 2n},
\qquad
\tilde b=\begin{pmatrix}b_r\\ b_i\end{pmatrix}\in\mathbf{R}^{2m}.
```

验证维数：$\widetilde{A}$ 有 $2m$ 行、$2n$ 列，与 $z\in\mathbf{R}^{2n}$ 相乘得 $\mathbf{R}^{2m}$，与 $\tilde b$ 匹配。于是 (a) 的等价实问题为

```math
\begin{aligned}
& \text{minimize} & & \|z\|_2 \\
& \text{subject to} & & \widetilde{A}z=\tilde b,
\end{aligned}
```

这是标准的最小 $\ell_2$-范数问题（least $\ell_2$-norm problem），其解可由伪逆表出：$z^\star=\widetilde{A}^T(\widetilde{A}\widetilde{A}^T)^{-1}\tilde b$（因 $\widetilde{A}$ 满行秩，$m\lt n\Rightarrow 2m<2n$，方程欠定）。还原为 $x^\star=u^\star+i v^\star$。

**(b) 复最小 $\ell_\infty$-范数问题 → SOCP。**

引入辅助变量 $t\in\mathbf{R}$，用 epigraph 形式把 $\|x\|_\infty$ 写掉。因为 $\|x\|_\infty=\max_i |x_i|$，约束 $\|x\|_\infty\le t$ 等价于

```math
|x_i|\le t,\qquad i=1,\dots,n.
```

每个 $|x_i|=\sqrt{u_i^2+v_i^2}\le t$ 是二阶锥约束（second-order cone constraint）$(u_i,v_i)$ 落在半径为 $t$ 的圆盘内，即

```math
\left\|\begin{pmatrix}u_i\\ v_i\end{pmatrix}\right\|_2 \le t,\qquad i=1,\dots,n.
```

线性等式约束仍为 $\widetilde{A}z=\tilde b$。因此复最小 $\ell_\infty$-范数问题等价于 SOCP：

```math
\begin{aligned}
& \text{minimize} & & t \\
& \text{subject to} & & \widetilde{A}z=\tilde b \\
& & & \left\|\begin{pmatrix}u_i\\ v_i\end{pmatrix}\right\|_2 \le t,\quad i=1,\dots,n,
\end{aligned}
```

变量为 $t\in\mathbf{R}$、$z=(u;v)\in\mathbf{R}^{2n}$。

**(c) 数值求解与观测。**

用 `A = randn(m,n) + i*randn(m,n)`、`b = randn(m,1) + i*randn(m,1)` 生成数据，`m=30`、`n=100`。用 cvx 的 `norm(x,2)`、`norm(x,inf)`（已重载支持复参）求解。观测要点：$\ell_2$-范数解的各分量模长分布较均匀（能量尽量平均分摊到 $n$ 个分量上），在复平面上大致呈圆盘状散点、无明显峰值；$\ell_\infty$-范数解则把每个分量的模长压到同一个上界 $t^\star$，因此散点趋向于集中在以原点为圆心、半径 $t^\star$ 的圆周附近（很多点落在该圆上或圆内紧贴圆边），整体更"圆环"状。即 $\ell_2$ 解用"平方和"均匀分配能量，而 $\ell_\infty$ 解用"最大模"逐个拉平到同一上界。

[[FIGURE: 该题无页内图，仅要求用 scatter 绘图，实际图由学生生成]]

---

## 题目 3（Problem 3）

**题干：** Numerical perturbation analysis example. Consider the quadratic program

```math
\begin{aligned}
& \text{minimize} & & x_1^2 + 2x_2^2 - x_1 x_2 - x_1 \\
& \text{subject to} & & x_1 + 2x_2 \le u_1 \\
& & & x_1 - 4x_2 \le u_2, \\
& & & 5x_1 + 76x_2 \le 1,
\end{aligned}
```

with variables $x_1, x_2$, and parameters $u_1, u_2$.

(a) Solve this QP, for parameter values $u_1=-2$, $u_2=-3$, to find optimal primal variable values $x_1^\star$ and $x_2^\star$, and optimal dual variable values $\lambda_1^\star$, $\lambda_2^\star$ and $\lambda_3^\star$. Let $p^\star$ denote the optimal objective value. Verify that the KKT conditions hold ... (within reasonable numerical accuracy).

(b) ... solve perturbed versions with $u_1=-2+\delta_1$, $u_2=-3+\delta_2$, where $\delta_1,\delta_2\in\{-0.1,0,0.1\}$ (nine combinations). For each, make a prediction $p^\star_{\mathrm{pred}}$ ... and compare to $p^\star_{\mathrm{exact}}$ ... Check that $p^\star_{\mathrm{pred}}\le p^\star_{\mathrm{exact}}$ holds.

**中文：** 数值摄动分析示例（numerical perturbation analysis example）。考虑如下二次规划（QP, Quadratic Program）：在约束 $x_1+2x_2\le u_1$、$x_1-4x_2\le u_2$、$5x_1+76x_2\le 1$ 下，极小化 $x_1^2+2x_2^2-x_1x_2-x_1$，变量 $x_1,x_2$，参数 $u_1,u_2$。

(a) 对参数 $u_1=-2$、$u_2=-3$ 求解此 QP，求出最优原变量 $x_1^\star,x_2^\star$ 与最优对偶变量 $\lambda_1^\star,\lambda_2^\star,\lambda_3^\star$，记最优值为 $p^\star$。验证 KKT 条件在（合理数值精度内）成立。

(b) 求解摄动版本，$u_1=-2+\delta_1$、$u_2=-3+\delta_2$，$\delta_1,\delta_2\in\{-0.1,0,0.1\}$（共九种组合）。对每种组合给出最优值的预测 $p^\star_{\mathrm{pred}}$，与摄动 QP 的精确最优值 $p^\star_{\mathrm{exact}}$ 比较，检查不等式 $p^\star_{\mathrm{pred}}\le p^\star_{\mathrm{exact}}$ 成立。

**解答：**

**(a)** 目标函数写成二次型。目标为

```math
f(x)=\tfrac12 x^T P x+q^T x,
\qquad
P=\begin{pmatrix}2 & -1\\ -1 & 4\end{pmatrix},
\qquad
q=\begin{pmatrix}-1\\ 0\end{pmatrix}.
```

验证：$\tfrac12 x^T P x=\tfrac12(2x_1^2-2x_1x_2+4x_2^2)=x_1^2-x_1x_2+2x_2^2$，加 $q^T x=-x_1$ 正好得 $x_1^2+2x_2^2-x_1x_2-x_1$。$P$ 的主子式 $2>0$、$\det P=2\cdot4-(-1)^2=7>0$，故 $P\succ 0$，问题严格凸（strictly convex），最优解唯一。

约束写成 $Ax\preceq b$ 形式：

```math
A=\begin{pmatrix}1 & 2\\ 1 & -4\\ 5 & 76\end{pmatrix},
\qquad
b=\begin{pmatrix}u_1\\ u_2\\ 1\end{pmatrix}
=\begin{pmatrix}-2\\ -3\\ 1\end{pmatrix}.
```

代入 $u_1=-2$、$u_2=-3$ 后，第三条约束 $5x_1+76x_2\le 1$ 显然在可行域外部很远，可以预计是松约束。先用无约束最小化定位：$\nabla f=Px+q=0$ 给出

```math
\begin{pmatrix}2&-1\\-1&4\end{pmatrix}\begin{pmatrix}x_1\\x_2\end{pmatrix}=\begin{pmatrix}1\\0\end{pmatrix}
\Rightarrow x_1=\frac{4}{7},\ x_2=\frac{1}{7}.
```

检查该点是否满足前两条约束：$x_1+2x_2=\frac47+\frac27=\frac67\approx0.857>-2$，违反第一条；$x_1-4x_2=\frac47-\frac47=0>-3$，违反第二条。故无约束解不可行，最优解由前两条约束（至少其一）在边界上取到。

由于 $\nabla f$ 指向让 $x_1+2x_2$ 增大的方向（见下），最优解落在第一条约束 $x_1+2x_2=-2$ 上（active）。令第一条约束取等号 $x_1+2x_2=-2$，即 $x_1=-2-2x_2$，代入目标：

```math
f=(-2-2x_2)^2+2x_2^2-(-2-2x_2)x_2-(-2-2x_2).
```

展开：$(-2-2x_2)^2=4+8x_2+4x_2^2$，$(-2-2x_2)x_2=-2x_2-2x_2^2$（前面有负号，故 $-(-2x_2-2x_2^2)=2x_2+2x_2^2$），$-(-2-2x_2)=2+2x_2$。合并：

```math
f=4+8x_2+4x_2^2+2x_2^2+2x_2+2x_2^2+2+2x_2=6+12x_2+8x_2^2.
```

对 $x_2$ 求导：$\frac{df}{dx_2}=12+16x_2=0\Rightarrow x_2=-3/4$。于是 $x_1=-2-2(-3/4)=-2+3/2=-1/2$。得候选解

```math
x_1^\star=-\frac12,\qquad x_2^\star=-\frac34.
```

检验该点是否违反第二、三条约束：第二条 $x_1-4x_2=-\frac12-4(-\frac34)=-\frac12+3=\frac52> -3$，违反（$u_2=-3$），因此第二条约束也 active，需同时取等号。

联立前两条约束取等号：

```math
x_1+2x_2=-2,\qquad x_1-4x_2=-3.
```

两式相减：$(x_1+2x_2)-(x_1-4x_2)=6x_2=-2-(-3)=1$，故 $x_2=1/6$。回代 $x_1=-2-2x_2=-2-\frac13=-\frac73$。得

```math
x_1^\star=-\frac73,\qquad x_2^\star=\frac16.
```

检验第三条：$5x_1+76x_2=5(-\frac73)+76(\frac16)=-\frac{35}{3}+\frac{38}{3}=1$，恰好取等号。于是三条约束都 active（$\lambda_1,\lambda_2,\lambda_3\ge 0$ 均可能非零）。

计算最优值：

```math
\begin{aligned}
p^\star &= x_1^2+2x_2^2-x_1x_2-x_1\\
&= \left(-\tfrac73\right)^2+2\left(\tfrac16\right)^2-\left(-\tfrac73\right)\left(\tfrac16\right)-\left(-\tfrac73\right)\\
&= \tfrac{49}{9}+\tfrac{2}{36}+\tfrac{7}{18}+\tfrac73\\
&= \tfrac{49}{9}+\tfrac{1}{18}+\tfrac{7}{18}+\tfrac{7}{3}\\
&= \tfrac{98}{18}+\tfrac{1}{18}+\tfrac{7}{18}+\tfrac{42}{18}=\tfrac{148}{18}=\tfrac{74}{9}\approx 8.2222.
\end{aligned}
```

求对偶变量。KKT 驻点条件（stationarity）：

```math
\nabla f(x^\star)+\sum_{j=1}^3\lambda_j\nabla h_j(x^\star)=0,
\qquad
\nabla f(x^\star)=P x^\star+q.
```

先算 $P x^\star+q$：

```math
P x^\star=\begin{pmatrix}2&-1\\-1&4\end{pmatrix}\begin{pmatrix}-7/3\\1/6\end{pmatrix}
=\begin{pmatrix}-\frac{14}{3}-\frac16\\ \frac73+\frac23\end{pmatrix}
=\begin{pmatrix}-\frac{29}{6}\\ 3\end{pmatrix},
```

加 $q=\begin{pmatrix}-1\\0\end{pmatrix}$ 得

```math
\nabla f(x^\star)=\begin{pmatrix}-\frac{29}{6}-1\\ 3\end{pmatrix}
=\begin{pmatrix}-\frac{35}{6}\\ 3\end{pmatrix}.
```

各约束梯度：

```math
\nabla h_1=\begin{pmatrix}1\\2\end{pmatrix},\quad
\nabla h_2=\begin{pmatrix}1\\-4\end{pmatrix},\quad
\nabla h_3=\begin{pmatrix}5\\76\end{pmatrix}.
```

驻点条件 $\nabla f+\lambda_1\nabla h_1+\lambda_2\nabla h_2+\lambda_3\nabla h_3=0$ 写成分量方程组：

```math
-\frac{35}{6}+\lambda_1+\lambda_2+5\lambda_3=0,
\qquad
3+2\lambda_1-4\lambda_2+76\lambda_3=0.
```

两个方程、三个未知，还需用互补松弛。三条约束均取等号，互补松弛不直接给出某个 $\lambda$ 为零的信息，但三条 active 约束的 $\lambda_j$ 唯一确定（三个 active 约束的梯度线性无关时）。实际上三条约束梯度

```math
\begin{pmatrix}1\\2\end{pmatrix},\ \begin{pmatrix}1\\-4\end{pmatrix},\ \begin{pmatrix}5\\76\end{pmatrix}
```

张成整个 $\mathbf{R}^2$，其中任意两条即可线性表示第三条，故需要第三条约束确实 active 才一致。解上述方程：由第一式 $\lambda_1+\lambda_2=-(-\frac{35}{6})-5\lambda_3=\frac{35}{6}-5\lambda_3$。代入第二式 $3+2\lambda_1-4\lambda_2+76\lambda_3=0$。

将 $\lambda_2=\frac{35}{6}-5\lambda_3-\lambda_1$ 代入第二式：

```math
3+2\lambda_1-4\left(\tfrac{35}{6}-5\lambda_3-\lambda_1\right)+76\lambda_3=0,
```

```math
3+2\lambda_1-\tfrac{70}{3}+20\lambda_3+4\lambda_1+76\lambda_3=0,
```

```math
6\lambda_1+96\lambda_3+\left(3-\tfrac{70}{3}\right)=0,
\qquad
3-\tfrac{70}{3}=\tfrac{9-70}{3}=-\tfrac{61}{3},
```

```math
6\lambda_1+96\lambda_3=\tfrac{61}{3}.
```

还需要第三个方程，来自第三条约束也确实 active 的性质。事实上只有两条约束梯度独立，第三条是其余两条的线性组合。检查：$\nabla h_3$ 能否由 $\nabla h_1,\nabla h_2$ 表出？求 $\alpha,\beta$ 使 $\alpha(1,2)+\beta(1,-4)=(5,76)$：$\alpha+\beta=5$，$2\alpha-4\beta=76$。由 $\alpha=5-\beta$ 代入 $2(5-\beta)-4\beta=10-6\beta=76\Rightarrow -6\beta=66\Rightarrow\beta=-11$，$\alpha=16$。故 $\nabla h_3=16\nabla h_1-11\nabla h_2$。因此驻点条件可写为

```math
\nabla f+(\lambda_1+16\lambda_3)\nabla h_1+(\lambda_2-11\lambda_3)\nabla h_2=0.
```

只有两条独立约束，而三条同时 active 属于"退化"情形，此时对偶变量不唯一（一维解集）。为得到一组可行对偶变量，取其中两条作 active 约束求解即可。若取约束 1、2 active（约束 3 的 $\lambda_3=0$），则由

```math
-\frac{35}{6}+\lambda_1+\lambda_2=0,\qquad 3+2\lambda_1-4\lambda_2=0.
```

第二式 $2\lambda_1-4\lambda_2=-3\Rightarrow\lambda_1-2\lambda_2=-\frac32$，$\lambda_1=2\lambda_2-\frac32$。代入第一式 $\frac{35}{6}=\lambda_1+\lambda_2=3\lambda_2-\frac32$，故 $3\lambda_2=\frac{35}{6}+\frac32=\frac{35+9}{6}=\frac{44}{6}=\frac{22}{3}$，$\lambda_2=\frac{22}{9}$。于是 $\lambda_1=2\cdot\frac{22}{9}-\frac32=\frac{44}{9}-\frac{27}{18}=\frac{44}{9}-\frac32$，通分 $\frac{88-27}{18}=\frac{61}{18}$。得一组解 $\lambda_1^\star=\frac{61}{18}$、$\lambda_2^\star=\frac{22}{9}$、$\lambda_3^\star=0$。

但注意上面求得的原解同时满足第三条取等号，这是数据巧合导致的退化，$p^\star$ 仍是唯一确定的最优值。验证 KKT：$x^\star=(-7/3,1/6)$ 满足 $\mathbf{1}^T$ 型等式无、不等式三条均 $\le$ 且前两条取等（第三条也恰好取等），$\lambda_1,\lambda_2>0$，$\lambda_3=0$；驻点条件代入 $\nabla f+(\lambda_1)\nabla h_1+(\lambda_2)\nabla h_2=\begin{pmatrix}-\frac{35}{6}+\frac{61}{18}+\frac{22}{9}\\ 3+\frac{61}{9}-\frac{88}{9}\end{pmatrix}$。第一分量：$-\frac{35}{6}+\frac{61}{18}+\frac{44}{18}=-\frac{105}{18}+\frac{105}{18}=0$。第二分量：$3+\frac{61-88}{9}=3-\frac{27}{9}=0$。互补松弛：$\lambda_3 h_3=0$（$\lambda_3=0$）成立，$\lambda_1,\lambda_2$ 对应约束取等，成立。全部 KKT 条件满足，验证通过。

**(b)** 摄动预测用最优对偶变量的一阶灵敏度：对约束右端 $u_i$ 的摄动 $\delta_i$，最优值的一阶近似为

```math
p^\star_{\mathrm{pred}}=p^\star+\lambda_1^\star\delta_1+\lambda_2^\star\delta_2,
```

（因对最小化问题，$p^\star$ 关于右端项 $b$ 的梯度是 $\nabla_b p^\star=-\lambda^\star$，但这里约束写为 $Ax\preceq b$ 且 $b=(u_1,u_2,1)$，$\lambda\ge0$，$p^\star$ 对 $u_i$ 的偏导为 $-\lambda_i$。于是 $p^\star_{\mathrm{pred}}=p^\star-\lambda_1\delta_1-\lambda_2\delta_2$。这里需注意符号约定：对不等式 $g_i(x)\le u_i$，$p^\star$ 关于 $u_i$ 递减，故 $\frac{\partial p^\star}{\partial u_i}=-\lambda_i$。）

故

```math
p^\star_{\mathrm{pred}}\approx p^\star-\lambda_1^\star\delta_1-\lambda_2^\star\delta_2.
```

以 $p^\star=\frac{74}{9}$、$\lambda_1^\star=\frac{61}{18}$、$\lambda_2^\star=\frac{22}{9}$ 代入。九种组合如下表（$\delta_1,\delta_2\in\{-0.1,0,0.1\}$，$p^\star_{\mathrm{pred}}=8.2222-\frac{61}{18}\delta_1-\frac{22}{9}\delta_2$，其中 $\frac{61}{18}\approx3.3889$、$\frac{22}{9}\approx2.4444$）：

| $\delta_1$ | $\delta_2$ | $p^\star_{\mathrm{pred}}$ | $p^\star_{\mathrm{exact}}$ |
|---|---|---|---|
| $0$ | $0$ | $8.2222$ | $8.2222$ |
| $0$ | $-0.1$ | $8.4667$ | 〔需数值求解，填入 CVX 结果〕 |
| $0$ | $0.1$ | $7.9778$ | 〔同上〕 |
| $-0.1$ | $0$ | $8.5611$ | 〔同上〕 |
| $-0.1$ | $-0.1$ | $8.8056$ | 〔同上〕 |
| $-0.1$ | $0.1$ | $8.3167$ | 〔同上〕 |
| $0.1$ | $0$ | $7.8833$ | 〔同上〕 |
| $0.1$ | $-0.1$ | $8.1278$ | 〔同上〕 |
| $0.1$ | $0.1$ | $7.6389$ | 〔同上〕 |

数值验证：由于该 QP 严格凸且对偶变量给出的一阶预测是 $p^\star$ 关于约束右端的超梯度（对凸问题，$p^\star_{\mathrm{pred}}\le p^\star_{\mathrm{exact}}$ 恒成立，这是局部灵敏度分析的结论，$p^\star(u)$ 是凹函数故其一阶 Taylor 近似在下方）。学生用 CVX（`quad_form` 指定二次目标，按 §3.6 取对偶变量）求解九次得精确值，逐一核对 $p^\star_{\mathrm{pred}}\le p^\star_{\mathrm{exact}}$。

---

## 题目 4（Problem 4）

**题干：** FIR filter design. Consider the (symmetric, linear phase) FIR filter described by

```math
H(\omega)=a_0+\sum_{k=1}^N a_k\cos k\omega.
```

The design variables are the real coefficients $a=(a_0,\dots,a_N)\in\mathbf{R}^{N+1}$. ... design of a low-pass filter with specifications:

- For $0\le\omega\le\pi/3$, $0.89\le H(\omega)\le 1.12$ (about $\pm1$dB ripple in the passband $[0,\pi/3]$).
- For $\omega_c\le\omega\le\pi$, $|H(\omega)|\le\alpha$ (attenuation $\alpha$ in the stopband $[\omega_c,\pi]$). $\omega_c$ is the cutoff frequency.

(a) Fix $\omega_c$ and $N$: maximize stopband attenuation, i.e., minimize $\alpha$. Pose as convex optimization problem.

(b) Fix $N$ and $\alpha$: minimize $\omega_c$. Pose as quasiconvex optimization problem.

(c) Fix $\omega_c$ and $\alpha$: find the smallest $N$. Convex/quasiconvex? Explain.

(d) Plot optimal tradeoff curve of attenuation $\alpha$ vs cutoff frequency $\omega_c$ for $N=7$. Is the set of achievable specifications convex? Explain interesting features (e.g. flat portions). For this subproblem, sample constraints in frequency: choose $K\gg N$ (e.g. $K\approx10N$), set $\omega_k=k\pi/K$, $k=0,\dots,K$, and replace specs with samples.

**中文：** FIR 滤波器设计（FIR filter design）。考虑由 $H(\omega)=a_0+\sum_{k=1}^N a_k\cos k\omega$ 描述的（对称、线性相位）FIR 滤波器（FIR, Finite Impulse Response）。设计变量为实系数 $a=(a_0,\dots,a_N)\in\mathbf{R}^{N+1}$。设计一个低通滤波器，规格如下：

- 对 $0\le\omega\le\pi/3$，$0.89\le H(\omega)\le 1.12$，即通带（passband）$[0,\pi/3]$ 内约有 $\pm1$dB 纹波（ripple）。
- 对 $\omega_c\le\omega\le\pi$，$|H(\omega)|\le\alpha$，即滤波器在阻带（stopband）$[\omega_c,\pi]$ 内实现衰减 $\alpha$。$\omega_c$ 称为截止频率（cutoff frequency）。

(a) 固定 $\omega_c$ 与 $N$：最大化阻带衰减，即极小化 $\alpha$。说明如何化为凸优化问题。

(b) 固定 $N$ 与 $\alpha$：极小化 $\omega_c$。说明如何化为拟凸（quasiconvex）优化问题。

(c) 固定 $\omega_c$ 与 $\alpha$：求最小 $N$。能否化为凸或拟凸问题？说明。

(d) 对 $N=7$ 画出衰减 $\alpha$ 与截止频率 $\omega_c$ 的最优权衡曲线（optimal tradeoff curve）。可实现的规格集合是否凸？简述有趣特征（如平坦段）。本小题可对频率采样：取 $K\gg N$（如 $K\approx10N$）、$\omega_k=k\pi/K$，$k=0,\dots,K$，把规格替换为采样点上的约束。

**解答：**

**(a)** 注意到 $H(\omega)$ 是系数 $a$ 的线性函数（每个 $\cos k\omega$ 是 $\omega$ 的已知函数，与 $a$ 无关）。因此所有关于 $H(\omega)$ 的界约束都是 $a$ 的线性不等式。固定 $\omega_c$、$N$，问题为

```math
\begin{aligned}
& \text{minimize} & & \alpha \\
& \text{subject to} & & 0.89\le H(\omega)\le 1.12,\quad 0\le\omega\le\pi/3 \\
& & & |H(\omega)|\le\alpha,\quad \omega_c\le\omega\le\pi.
\end{aligned}
```

其中 $|H(\omega)|\le\alpha$ 等价于两条线性约束 $-\alpha\le H(\omega)\le\alpha$。变量 $(\alpha,a)$ 上所有约束都是线性的，目标 $\alpha$ 线性，故这是一个线性规划（LP），自然是凸问题。由于 $\omega$ 是连续区间（无穷多条约束），数值求解时按 (d) 的方式在频率上采样，转化为有限维 LP。

**(b)** 固定 $N$、$\alpha$，极小化 $\omega_c$。这里 $\omega_c$ 不是以线性（甚至不是以凸）方式进入约束：阻带区间 $[\omega_c,\pi]$ 的端点 $\omega_c$ 是变量，约束 "对所有 $\omega\in[\omega_c,\pi]$ 有 $|H(\omega)|\le\alpha$" 关于 $\omega_c$ 是"越大越宽松"，即 $\omega_c$ 减小会使可行集变小。这是一个拟凸问题（quasiconvex），因为对固定的 $\omega_c$，可行性检验（feasibility problem）是一个凸问题（关于 $a$ 的线性约束，$\omega_c$ 固定后区间固定）：

```math
\begin{aligned}
& \text{find} & & a \\
& \text{subject to} & & 0.89\le H(\omega)\le1.12,\quad 0\le\omega\le\pi/3 \\
& & & |H(\omega)|\le\alpha,\quad \omega_c\le\omega\le\pi.
\end{aligned}
```

对给定 $\omega_c$，上述可行集是 $a$ 的凸集（半空间之交）。目标 $\omega_c$ 的次水平集 $\{\omega_c\mid \text{可行}\}$ 是区间 $[\omega_c^{\min},\pi]$，为凸集，因此用二分法（bisection）在 $\omega_c$ 上搜索最小可行值即可求解拟凸问题。每次二分迭代解一个 LP 可行性问题。

**(c)** 固定 $\omega_c$、$\alpha$，求最小 $N$。此时 $N$ 是变量，而 $N$ 决定了系数的个数 $N+1$ 以及求和项数——$N$ 是整数变量，且以离散、非线性方式进入 $H(\omega)=a_0+\sum_{k=1}^N a_k\cos k\omega$。这不能化为凸或拟凸问题：可行 $N$ 的集合不是连续凸集（是整数集合），且约束关于 $N$ 无凸性结构。要解只能对 $N=1,2,3,\dots$ 逐一尝试，对每个 $N$ 解一个 (a) 或 (b) 型的凸/拟凸可行性问题，直到找到能同时满足通带与阻带规格的最小 $N$ 为止。因此它本质上是一个整数/枚举问题，不是凸问题。

**(d)** 采用频率采样：取 $K\approx10N=70$（原文 $K\approx10N$），$\omega_k=k\pi/K$，$k=0,\dots,K$。通带采样点为满足 $0\le\omega_k\le\pi/3$ 的 $k$，阻带采样点为满足 $\omega_c\le\omega_k\le\pi$ 的 $k$。这样 (a) 变成有限维 LP。对一系列 $\omega_c$ 取值（例如从 $\pi/3$ 到 $\pi$ 扫点），分别解 (a) 得到最优 $\alpha^\star(\omega_c)$，即可画出 $\alpha$ 对 $\omega_c$ 的权衡曲线。$\omega_c$ 越大（阻带起点越靠右、过渡带越窄），越难同时压低阻带，故 $\alpha^\star$ 越大。

**可实现的规格集合是否凸？** 考虑由"存在某个 $a$ 使得对给定 $(\alpha,\omega_c)$ 全部规格被满足"定义的集合 $S$。$S$ 关于 $(\alpha,\omega_c)$ 不是凸的：$\omega_c$ 方向单调（$\omega_c$ 增大总是更容易满足阻带，因为阻带区间变小），而 $\alpha$ 方向也单调（$\alpha$ 增大更容易满足），故 $S$ 在两个方向分别单调，但整体一般不是凸集——典型的可实现区域在 $(\alpha,\omega_c)$ 平面上呈"单调边界"形状（越往右上越容易满足，越往左下越难），边界曲线即最优权衡曲线。

**权衡曲线的有趣特征：** 平坦段（flat portions）出现在某些 $\omega_c$ 区间内最优 $\alpha^\star$ 不变的情形，通常是阻带内的峰值约束被一个固定频率点主导（例如某个旁瓣峰值位置不随 $\omega_c$ 微小移动而改变），或当最优滤波器的阻带衰减已被通带纹波约束锁死、无法随 $\omega_c$ 进一步改善。权衡曲线整体是单调不减的，且由离散采样会呈现阶梯状（piecewise constant）的平坦段。

[[FIGURE: 第 2 页，滤波器幅频规格示意图：横轴 $\omega$，纵轴 $H(\omega)$，标注 $0,\pi/3,\omega_c,\pi$，通带 $0.89\sim1.12$ 带状区、阻带 $-\alpha\sim\alpha$ 带状区]]

---

## 题目 5（Problem 5）

**题干：** Minimum fuel optimal control. Solve the minimum fuel optimal control problem described in exercise 4.16 of *Convex Optimization*, for the instance with problem data

```math
A=\begin{pmatrix}-1 & 0.4 & 0.8\\ 1 & 0 & 0\\ 0 & 1 & 0\end{pmatrix},\qquad
b=\begin{pmatrix}1\\ 0\\ 0.3\end{pmatrix},\qquad
x_{\mathrm{des}}=\begin{pmatrix}7\\ 2\\ -6\end{pmatrix},\qquad N=30.
```

You can do this by forming the LP you found in your solution of exercise 4.16, or more directly using cvx. Plot the actuator signal $u(t)$ as a function of time $t$.

**中文：** 最小燃料最优控制（minimum fuel optimal control）。求解 *Convex Optimization* 习题 4.16 中描述的最小燃料最优控制问题，数据为 $A=\begin{pmatrix}-1&0.4&0.8\\1&0&0\\0&1&0\end{pmatrix}$、$b=\begin{pmatrix}1\\0\\0.3\end{pmatrix}$、$x_{\mathrm{des}}=\begin{pmatrix}7\\2\\-6\end{pmatrix}$、$N=30$。可通过构造你在习题 4.16 解中得到的 LP 来求解，或直接用 cvx。画出执行器信号（actuator signal）$u(t)$ 关于时间 $t$ 的图像。

**解答：**

习题 4.16 的设定：线性动力学

```math
x_{t+1}=Ax_t+b u_t,\qquad t=0,\dots,N-1,
```

初态 $x_0=0$（习题默认从零初态出发），目标是把状态驱动到 $x_N=x_{\mathrm{des}}$，同时极小化燃料消耗 $\sum_{t=0}^{N-1}|u_t|$（$\ell_1$-范数，衡量燃料）。这是最小燃料（minimum fuel）问题，目标凸但非处处可微，用 epigraph 引入 $v_t\ge0$ 化为 LP。

**化为 LP：** 引入变量 $v_t\ge0$，约束 $-v_t\le u_t\le v_t$，目标 $\sum_t v_t$。状态递推展开：

```math
x_N=A^N x_0+\sum_{t=0}^{N-1}A^{N-1-t}b\, u_t=\sum_{t=0}^{N-1}A^{N-1-t}b\, u_t,
```

（因 $x_0=0$）。令 $C_t=A^{N-1-t}b$，则终点约束为

```math
\sum_{t=0}^{N-1}C_t u_t=x_{\mathrm{des}}.
```

于是 LP 为

```math
\begin{aligned}
& \text{minimize} & & \sum_{t=0}^{N-1} v_t \\
& \text{subject to} & & \sum_{t=0}^{N-1} C_t u_t = x_{\mathrm{des}} \\
& & & -v_t\le u_t\le v_t,\quad t=0,\dots,N-1 \\
& & & v_t\ge 0,\quad t=0,\dots,N-1.
\end{aligned}
```

变量为 $u_t$ 与 $v_t$（各 $N=30$ 个），共 $60$ 个变量、$3$ 个等式约束（$x_{\mathrm{des}}\in\mathbf{R}^3$）加 $90$ 个不等式。用 cvx 直接以 `norm(u,1)` 为目标求解亦可。

**求解与绘图：** 用 CVX 求解得到最优控制序列 $u^\star(t)$，$t=0,\dots,29$。最小燃料控制（$\ell_1$ 目标）的典型特征是**稀疏性**：最优 $u^\star$ 在大部分时间步为零，只在少数时刻有非零脉冲（bang-off 型），因为 $\ell_1$ 目标鼓励稀疏。绘图（横轴时间 $t=0,\dots,29$，纵轴 $u(t)$）应呈现若干零区间的平坦段加少量尖峰。具体数值需运行 CVX 得到：

```matlab
cvx_begin
  variables u(N) v(N)
  minimize( sum(v) )
  subject to
    % 终点约束：sum_t C_t u_t = x_des
    C = zeros(3,N);
    for t=0:N-1
      C(:,t+1) = A^(N-1-t) * b;
    end
    C * u == x_des;
    -v <= u; u <= v; v >= 0;
cvx_end
plot(0:N-1, u); xlabel('t'); ylabel('u(t)');
```

〔具体 $u^\star$ 数值与图像由学生在 Matlab/CVX 中生成，此处给出问题建模与求解方法〕

[[FIGURE: 该题无页内图，$u(t)$-vs-$t$ 图像由学生生成]]
