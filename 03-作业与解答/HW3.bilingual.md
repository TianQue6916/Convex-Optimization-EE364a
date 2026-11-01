# EE364a HW3 解答（作业 3 解答）

> Stanford EE364a, Winter 2007-08 · Prof. S. Boyd
> 以下为官方解答的中文译注与完整推导，公式按上下文还原为 LaTeX。约定：中文为主体，关键术语首次以 `English（中文）` 形式出现，之后只用 `English`。

---

## 题目 3.42 近似宽度（Approximation width）

**题干：** Let $f_0,\dots,f_n:\mathbb{R}\to\mathbb{R}$ be given continuous functions. We consider the problem of approximating $f_0$ as a linear combination of $f_1,\dots,f_n$. For $x\in\mathbb{R}^n$, we say that $f=x_1f_1+\cdots+x_nf_n$ approximates $f_0$ with tolerance（容差）$\epsilon>0$ over the interval $[0,T]$ if $|f(t)-f_0(t)|\le\epsilon$ for $0\le t\le T$. Now we choose a fixed tolerance $\epsilon>0$ and define the approximation width（近似宽度） as the largest $T$ such that $f$ approximates $f_0$ over the interval $[0,T]$:

```math
W(x)=\sup\{T \mid |x_1f_1(t)+\cdots+x_nf_n(t)-f_0(t)|\le\epsilon \text{ for } 0\le t\le T\}.
```

Show that $W$ is quasiconcave（拟凹）.

**中文：** 设 $f_0,\dots,f_n:\mathbb{R}\to\mathbb{R}$ 为给定的连续函数。我们考虑把 $f_0$ 用 $f_1,\dots,f_n$ 的线性组合来近似的问题。对 $x\in\mathbb{R}^n$，若在区间 $[0,T]$ 上有 $|f(t)-f_0(t)|\le\epsilon$（其中 $0\le t\le T$），则称 $f=x_1f_1+\cdots+x_nf_n$ 以容差 $\epsilon>0$ 在 $[0,T]$ 上近似 $f_0$。现在固定容差 $\epsilon>0$，定义近似宽度 $W(x)$ 为使得 $f$ 在 $[0,T]$ 上近似 $f_0$ 的最大 $T$。证明 $W$ 是拟凹的。

**解答：** 要证 $W$ 拟凹，只需证明对任意 $\alpha$，上水平集（superlevel set）$\{x\mid W(x)\ge\alpha\}$ 是凸集。

按定义，$W(x)\ge\alpha$ 当且仅当对所有 $t\in[0,\alpha)$ 都成立

```math
-\epsilon \le x_1f_1(t)+\cdots+x_nf_n(t)-f_0(t) \le \epsilon .
```

也就是说，$W(x)\ge\alpha$ 等价于对所有 $t\in[0,\alpha)$，同时满足**两个**关于 $x$ 的线性不等式：

```math
x_1f_1(t)+\cdots+x_nf_n(t) \le f_0(t)+\epsilon,
```

```math
-x_1f_1(t)-\cdots-x_nf_n(t) \le -f_0(t)+\epsilon .
```

对每一个固定的 $t$，上面两条不等式各定义了一个 halfspace（半空间），而半空间是凸集。于是集合 $\{x\mid W(x)\ge\alpha\}$ 是无穷多个半空间（每个 $t$ 对应两个）的交集，因此是凸集。

从而对任意 $\alpha$，上水平集 $\{x\mid W(x)\ge\alpha\}$ 都是凸集，这正说明 $W$ 是 quasiconcave。$\blacksquare$

---

## 题目 3.54 高斯累积分布函数的对数凹性（Log-concavity of Gaussian cumulative distribution function）

**题干：** The cumulative distribution function of a Gaussian random variable,

```math
f(x)=\frac{1}{\sqrt{2\pi}}\int_{-\infty}^{x}e^{-t^2/2}\,dt,
```

is log-concave. This follows from the general result that the convolution of two log-concave functions is log-concave. In this problem we guide you through a simple self-contained proof that $f$ is log-concave. Recall that $f$ is log-concave if and only if $f''(x)f(x)\le f'(x)^2$ for all $x$.

(a) Verify that $f''(x)f(x)\le f'(x)^2$ for $x\ge 0$. That leaves us the hard part, which is to show the inequality for $x<0$.

(b) Verify that for any $t$ and $x$ we have $t^2/2\ge -x^2/2+xt$.

(c) Using part (b) show that $e^{-t^2/2}\le e^{x^2/2-xt}$. Conclude that

```math
\int_{-\infty}^{x}e^{-t^2/2}\,dt \le e^{x^2/2}\int_{-\infty}^{x}e^{-xt}\,dt .
```

(d) Use part (c) to verify that $f''(x)f(x)\le f'(x)^2$ for $x\le 0$.

**中文：** 高斯随机变量的累积分布函数 $f(x)=\frac{1}{\sqrt{2\pi}}\int_{-\infty}^{x}e^{-t^2/2}\,dt$ 是对数凹的。这可由一般结论「两个对数凹函数的卷积仍对数凹」推出。本题引导你给出一个自洽的简单证明。回顾：$f$ 对数凹当且仅当对所有 $x$ 有 $f''(x)f(x)\le f'(x)^2$。

(a) 验证 $x\ge0$ 时 $f''(x)f(x)\le f'(x)^2$；剩下难的部分是证明 $x<0$ 时的不等式。

(b) 验证对任意 $t,x$ 有 $t^2/2\ge -x^2/2+xt$。

(c) 用 (b) 证明 $e^{-t^2/2}\le e^{x^2/2-xt}$，并推出上面那个积分不等式。

(d) 用 (c) 验证 $x\le0$ 时 $f''(x)f(x)\le f'(x)^2$。

**解答：**

首先写出 $f$ 的导数。由微积分基本定理，$f'(x)=\frac{1}{\sqrt{2\pi}}e^{-x^2/2}$，再求导得 $f''(x)=-\frac{1}{\sqrt{2\pi}}xe^{-x^2/2}$。即

```math
f'(x)=\frac{e^{-x^2/2}}{\sqrt{2\pi}},\qquad f''(x)=-\frac{xe^{-x^2/2}}{\sqrt{2\pi}} .
```

**(a)** 当 $x\ge0$ 时，$f''(x)=-\frac{xe^{-x^2/2}}{\sqrt{2\pi}}\le0$。同时 $f(x)>0$（被积函数 $e^{-t^2/2}>0$ 且积分限非空）。于是 $f''(x)f(x)\le0$。而 $f'(x)^2\ge0$，故

```math
f''(x)f(x)\le 0 \le f'(x)^2 .
```

即 $x\ge0$ 时不等式成立。

**(b)** 方法一（利用凸性）：函数 $g(t)=t^2/2$ 是可微凸函数（$g''(t)=1>0$）。对可微凸函数有支撑线不等式（一阶 Taylor 下界）

```math
g(t)\ge g(x)+g'(x)(t-x)=x^2/2+x(t-x)=xt-x^2/2 .
```

这就得到 $t^2/2\ge xt-x^2/2=-x^2/2+xt$。

方法二（配平方，更简单）：注意到

```math
t^2/2+x^2/2-xt=\frac{1}{2}(x-t)^2\ge0 .
```

即 $t^2/2+x^2/2-xt\ge0$，把 $x^2/2-xt$ 移到右边即得 $t^2/2\ge -x^2/2+xt$。

**(c)** 由 (b)，$t^2/2\ge -x^2/2+xt$，两边同乘 $-1$ 得 $-t^2/2\le x^2/2-xt$。指数函数单调递增，故两边取指数得

```math
e^{-t^2/2}\le e^{x^2/2-xt}=e^{x^2/2}e^{-xt}.
```

对上式两边从 $-\infty$ 到 $x$ 积分（注意 $x^2/2$ 与积分变量 $t$ 无关，可提到积分号外）：

```math
\int_{-\infty}^{x}e^{-t^2/2}\,dt \le \int_{-\infty}^{x}e^{x^2/2}e^{-xt}\,dt = e^{x^2/2}\int_{-\infty}^{x}e^{-xt}\,dt .
```

**(d)** 关键不等式 $f''(x)f(x)\le f'(x)^2$ 代入 $f,f',f''$ 的具体表达式，化简如下。左边：

```math
f''(x)f(x)=\left(-\frac{xe^{-x^2/2}}{\sqrt{2\pi}}\right)\left(\frac{1}{\sqrt{2\pi}}\int_{-\infty}^{x}e^{-t^2/2}\,dt\right)
=-\frac{xe^{-x^2/2}}{2\pi}\int_{-\infty}^{x}e^{-t^2/2}\,dt .
```

右边：

```math
f'(x)^2=\frac{e^{-x^2}}{2\pi}.
```

于是不等式 $f''(x)f(x)\le f'(x)^2$ 等价于

```math
-\frac{xe^{-x^2/2}}{2\pi}\int_{-\infty}^{x}e^{-t^2/2}\,dt \le \frac{e^{-x^2}}{2\pi} .
```

当 $x\le0$ 时 $-x\ge0$。为便于处理，把含 $-x$ 的因子移到右边。两边同乘 $-\frac{2\pi}{x}$（注意 $x<0$，乘负数要变号；$x=0$ 时不等式显然成立，见下）：

```math
e^{-x^2/2}\int_{-\infty}^{x}e^{-t^2/2}\,dt \le \frac{e^{-x^2}}{-x},
```

即

```math
\int_{-\infty}^{x}e^{-t^2/2}\,dt \le \frac{e^{-x^2/2}}{-x}.
```

（当 $x=0$ 时 $f''(0)=0$，左边 $f''(0)f(0)=0\le f'(0)^2$ 显然成立，故下面只处理 $x<0$。）

现在用 (c) 的结果来证明上式。在 (c) 中已得

```math
\int_{-\infty}^{x}e^{-t^2/2}\,dt \le e^{x^2/2}\int_{-\infty}^{x}e^{-xt}\,dt .
```

当 $x<0$ 时，$-x>0$，于是 $\int_{-\infty}^{x}e^{-xt}\,dt$ 收敛，直接计算：

```math
\int_{-\infty}^{x}e^{-xt}\,dt=\frac{e^{-xt}}{-x}\Big|_{-\infty}^{x}
=\frac{e^{-x^2}}{-x}-\lim_{t\to-\infty}\frac{e^{-xt}}{-x}.
```

因为 $-x>0$，当 $t\to-\infty$ 时 $-xt\to-\infty$，故 $e^{-xt}\to0$，极限项为 $0$。所以

```math
\int_{-\infty}^{x}e^{-xt}\,dt=\frac{e^{-x^2}}{-x}.
```

代入 (c) 得

```math
\int_{-\infty}^{x}e^{-t^2/2}\,dt \le e^{x^2/2}\cdot\frac{e^{-x^2}}{-x}=\frac{e^{-x^2/2}}{-x}.
```

这正是所需的不等式。故 $x\le0$ 时 $f''(x)f(x)\le f'(x)^2$ 成立。结合 (a) 对 $x\ge0$ 也成立，得 $f$ 对数凹。$\blacksquare$

---

## 题目 3.57 证明 $f(X)=X^{-1}$ 在 $\mathbb{S}_{++}^n$ 上矩阵凸（matrix convex）

**题干：** Show that the function $f(X)=X^{-1}$ is matrix convex on $\mathbb{S}_{++}^n$.

**中文：** 证明函数 $f(X)=X^{-1}$ 在正定矩阵锥 $\mathbb{S}_{++}^n$ 上是矩阵凸的。

**解答：** 要证 $X\mapsto X^{-1}$ 矩阵凸，只需证：对任意向量 $v\in\mathbb{R}^n$，标量函数

```math
g(X)=v^{\mathsf T}X^{-1}v
```

在 $X\in\mathbb{S}_{++}^n$ 上是凸的（关于 $X$）。这是因为矩阵凸的定义（$f(\theta X+(1-\theta)Y)\preceq\theta f(X)+(1-\theta)f(Y)$）等价于沿任意方向 $v$ 的二次型都满足标量凸性。

而 $g(X)=v^{\mathsf T}X^{-1}v$ 的凸性是教材例 3.4（Example 3.4）的标准结论：函数 $X\mapsto v^{\mathsf T}X^{-1}v$ 在正定锥上是凸的。因此 $f(X)=X^{-1}$ 在 $\mathbb{S}_{++}^n$ 上矩阵凸。$\blacksquare$

---

## 题目 4.1 线性分式规划示意与最优解

**题干：** Consider the optimization problem

```math
\begin{aligned}
\text{minimize}\quad & f_0(x_1,x_2) \\
\text{subject to}\quad & 2x_1+x_2\ge 1 \\
& x_1+3x_2\ge 1 \\
& x_1\ge0,\quad x_2\ge0 .
\end{aligned}
```

Make a sketch of the feasible set（可行集）. For each of the following objective functions, give the optimal set（最优集） and the optimal value（最优值）.

(a) $f_0(x_1,x_2)=x_1+x_2$. (b) $f_0(x_1,x_2)=-x_1-x_2$. (c) $f_0(x_1,x_2)=x_1$. (d) $f_0(x_1,x_2)=\max\{x_1,x_2\}$. (e) $f_0(x_1,x_2)=x_1^2+9x_2^2$.

**中文：** 考虑如下优化问题（约束为 $2x_1+x_2\ge1$、$x_1+3x_2\ge1$、$x_1\ge0$、$x_2\ge0$）。画出可行集的草图，并对下列每个目标函数给出最优集与最优值。

**解答：**

可行集由四条半平面交成：$2x_1+x_2\ge1$、$x_1+3x_2\ge1$、$x_1\ge0$、$x_2\ge0$。关键顶点（vertices）：

- 两条约束直线 $2x_1+x_2=1$ 与 $x_1+3x_2=1$ 的交点：联立解 $2x_1+x_2=1$、$x_1+3x_2=1$，得 $x_1=\frac25$、$x_2=\frac15$，即点 $(\frac25,\frac15)$。
- $2x_1+x_2=1$ 与 $x_2=0$ 的交点：$(1,0)$。
- $x_1+3x_2=1$ 与 $x_1=0$ 的交点：$(0,1)$。

可行集是这三个顶点张成的无界凸多边形区域（向右上方向无界延伸）。

[[FIGURE: 可行集示意 —— 顶点为 (1,0)、(2/5,1/5)、(0,1) 的无界凸多边形]]

逐项求解：

**(a)** $f_0=x_1+x_2$。目标等高线为斜率 $-1$ 的直线，沿左下方向减小。最优点在直线 $2x_1+x_2=1$ 与 $x_1+3x_2=1$ 的交点处取得：

```math
x^\star=(2/5,\,1/5),\qquad p^\star = x_1^\star+x_2^\star = 2/5+1/5=3/5 .
```

**(b)** $f_0=-x_1-x_2$。可行集沿 $x_1\to+\infty$ 或 $x_2\to+\infty$ 无界，故 $-x_1-x_2$ 可无限减小，问题无下界（unbounded below），$p^\star=-\infty$。

**(c)** $f_0=x_1$。目标只随 $x_1$ 减小。在可行集中 $x_1$ 的最小可达值为 $0$，此时需满足 $x_1+3x_2\ge1$，即 $x_2\ge1/3$；同时还要满足 $2x_1+x_2=x_2\ge1$，故 $x_2\ge1$ 更强。因此最优集为

```math
X_{\mathrm{opt}}=\{(0,x_2)\mid x_2\ge1\},\qquad p^\star=0 .
```

**(d)** $f_0=\max\{x_1,x_2\}$。最小化 $\max\{x_1,x_2\}$ 相当于最小化一个公共上界 $t$（$x_1\le t,\ x_2\le t$），最优在 $x_1=x_2$ 处达到。令 $x_1=x_2=t$，代入最紧约束 $2x_1+x_2=3t\ge1$ 得 $t\ge1/3$；另一约束 $x_1+3x_2=4t\ge1$ 也满足。故

```math
x^\star=(1/3,\,1/3),\qquad p^\star=\max\{1/3,1/3\}=1/3 .
```

**(e)** $f_0=x_1^2+9x_2^2$。这是严格凸二次函数，最优在唯一点处。求其无约束最小点（梯度为零）：$\nabla f_0=(2x_1,18x_2)$，令其为 $0$ 得原点，但原点不可行，故最优在边界上。约束 $x_1+3x_2=1$ 最紧。题目给出的最优解为

```math
x^\star=(1/2,\,1/6) .
```

验证其最优性：在 $x^\star$ 处有

```math
2x_1+x_2=2\cdot\frac12+\frac16=\frac76>1,\qquad x_1+3x_2=\frac12+3\cdot\frac16=1 .
```

故只有约束 $x_1+3x_2=1$ 是 active（起作用的）。此时梯度为

```math
\nabla f_0(x^\star)=(2x_1,\,18x_2)=(1,\,3).
```

而直线 $x_1+3x_2=1$ 的法向量正是 $(1,3)$，因此 $\nabla f_0(x^\star)$ 与约束边界垂直。这正是 KKT 条件中「梯度等于约束法向量的非负倍数」的体现：存在 $\lambda>0$ 使 $\nabla f_0(x^\star)=\lambda\,(1,3)$（此处 $\lambda=1$），故 $x^\star$ 是最优解。最优值为

```math
p^\star = x_1^{\star2}+9x_2^{\star2}=\frac14+9\cdot\frac1{36}=\frac14+\frac14=\frac12 .
```

---

## 题目 4.4 [P. Parrilo] 对称性与凸优化

**题干：** Suppose $G=\{Q_1,\dots,Q_k\}\subseteq\mathbb{R}^{n\times n}$ is a group（群）, i.e., closed under products and inverse. We say that the function $f:\mathbb{R}^n\to\mathbb{R}$ is $G$-invariant（$G$-不变）, or symmetric with respect to $G$, if $f(Q_ix)=f(x)$ holds for all $x$ and $i=1,\dots,k$. We define $\bar x=(1/k)\sum_{i=1}^{k}Q_ix$, which is the average of $x$ over its $G$-orbit（$G$-轨道）. We define the fixed subspace（不动子空间） of $G$ as

```math
F=\{x\mid Q_ix=x,\ i=1,\dots,k\}.
```

(a) Show that for any $x\in\mathbb{R}^n$, we have $\bar x\in F$.

(b) Show that if $f:\mathbb{R}^n\to\mathbb{R}$ is convex and $G$-invariant, then $f(\bar x)\le f(x)$.

(c) We say the optimization problem $\text{minimize } f_0(x)\text{ subject to } f_i(x)\le0,\ i=1,\dots,m$ is $G$-invariant if the objective $f_0$ is $G$-invariant, and the feasible set is $G$-invariant, which means $f_1(x)\le0,\dots,f_m(x)\le0 \implies f_1(Q_ix)\le0,\dots,f_m(Q_ix)\le0$, for $i=1,\dots,k$. Show that if the problem is convex and $G$-invariant, and there exists an optimal point, then there exists an optimal point in $F$. In other words, we can adjoin the equality constraints $x\in F$ to the problem, without loss of generality.

(d) As an example, suppose $f$ is convex and symmetric, i.e., $f(Px)=f(x)$ for every permutation $P$. Show that if $f$ has a minimizer, then it has a minimizer of the form $\alpha\mathbf 1$. (This means to minimize $f$ over $x\in\mathbb{R}^n$, we can just as well minimize $f(t\mathbf 1)$ over $t\in\mathbb{R}$.)

**中文：** 设 $G=\{Q_1,\dots,Q_k\}\subseteq\mathbb{R}^{n\times n}$ 是一个群（对乘积和逆封闭）。称函数 $f:\mathbb{R}^n\to\mathbb{R}$ 为 $G$-不变（关于 $G$ 对称），若对所有 $x$ 和 $i=1,\dots,k$ 有 $f(Q_ix)=f(x)$。定义 $\bar x=(1/k)\sum_{i=1}^{k}Q_ix$，即 $x$ 在 $G$-轨道上的平均。定义 $G$ 的不动子空间 $F=\{x\mid Q_ix=x,\ i=1,\dots,k\}$。

(a) 证明对任意 $x\in\mathbb{R}^n$ 有 $\bar x\in F$。(b) 证明若 $f$ 凸且 $G$-不变，则 $f(\bar x)\le f(x)$。(c) 若问题凸且 $G$-不变，且存在最优解，则存在 $F$ 中的最优解（即可无损附加等式约束 $x\in F$）。(d) 举例：若 $f$ 凸且对任意置换 $P$ 有 $f(Px)=f(x)$，则若 $f$ 有极小点，必有形如 $\alpha\mathbf1$ 的极小点。

**解答：**

**(a)** 首先观察：用固定的某个 $Q_j$ 去左乘每个 $Q_i$，会得到 $\{Q_i\}$ 的一个置换。即存在置换 $\sigma$，使

```math
Q_jQ_i=Q_{\sigma(i)},\qquad i=1,\dots,k .
```

这是群论的基本事实，这里直接证明：由封闭性，每个 $Q_jQ_i$ 等于某个 $Q_s$。再设 $Q_jQ_i=Q_jQ_\ell=Q_s$，两边右乘 $Q_i^{-1}$（群含逆）得 $Q_j=Q_\ell$，故映射 $i\mapsto s$ 是一一对应，即一个置换 $\sigma$。

于是

```math
Q_j\bar x = Q_j\left(\frac1k\sum_{i=1}^{k}Q_ix\right)
=\frac1k\sum_{i=1}^{k}Q_jQ_ix
=\frac1k\sum_{i=1}^{k}Q_{\sigma(i)}x
=\frac1k\sum_{i=1}^{k}Q_ix=\bar x .
```

（最后一步因为 $\sigma$ 是置换，求和顺序改变不影响结果。）这对任意 $j$ 都成立，故 $\bar x\in F$。

**(b)** 利用 $f$ 的凸性与不变性：

```math
f(\bar x)=f\left(\frac1k\sum_{i=1}^{k}Q_ix\right)
\le \frac1k\sum_{i=1}^{k}f(Q_ix)
=\frac1k\sum_{i=1}^{k}f(x)=f(x) .
```

（第一步不等式用 Jensen 不等式/凸性，第二步等式用 $G$-不变性。）得证。

**(c)** 设 $x^\star$ 是原问题的一个最优解。先证 $\bar x^\star$ 可行：由 $G$-不变性，$f_i(x^\star)\le0\implies f_i(Q_jx^\star)\le0$ 对所有 $i,j$ 成立。又因 $f_i$ 凸（问题凸），且 $\bar x^\star$ 是各 $Q_jx^\star$ 的凸组合，故

```math
f_i(\bar x^\star)=f_i\left(\frac1k\sum_{j=1}^{k}Q_jx^\star\right)
\le\frac1k\sum_{j=1}^{k}f_i(Q_jx^\star)\le0 .
```

所以 $\bar x^\star$ 可行。再由 (b) 的结论（应用于凸且 $G$-不变的目标 $f_0$）：

```math
f_0(\bar x^\star)\le f_0(x^\star).
```

但 $x^\star$ 是最优解，故 $f_0(x^\star)$ 已是最优值，只能有 $f_0(\bar x^\star)=f_0(x^\star)$。又由 (a)，$\bar x^\star\in F$。因此 $\bar x^\star$ 是 $F$ 中的最优解。所以可以无损地附加约束 $x\in F$。

**(d)** 设 $x^\star$ 是 $f$ 的极小点。令

```math
\bar x=\frac{1}{n!}\sum_{P}Px^\star,
```

其中求和遍历所有置换矩阵 $P$。置换矩阵全体构成一个群（对称群 $S_n$ 的表示）。由 (a)，$\bar x$ 在该群的不动子空间中，即对任意置换 $P$ 有 $P\bar x=\bar x$。一个向量对所有置换不变，当且仅当它所有分量相等，故

```math
\bar x=\alpha\mathbf 1 \quad(\text{对某个 }\alpha\in\mathbb{R}).
```

再由 Jensen 不等式与对称性：

```math
f(\bar x)=f\left(\frac1{n!}\sum_P Px^\star\right)
\le\frac1{n!}\sum_P f(Px^\star)
=\frac1{n!}\sum_P f(x^\star)=f(x^\star).
```

故 $f(\bar x)\le f(x^\star)$，而 $x^\star$ 是极小点，所以 $f(\bar x)=f(x^\star)$，即 $\bar x=\alpha\mathbf1$ 也是极小点。这意味着在 $\mathbb{R}^n$ 上最小化 $f$，可以退化为在 $\mathbb{R}$ 上最小化 $f(t\mathbf1)$。$\blacksquare$

---

## 题目 4.8 若干简单线性规划（LPs）

**题干：** Give an explicit solution of each of the following LPs.

**(a)** Minimizing a linear function over an affine set: $\text{minimize } c^{\mathsf T}x \text{ subject to } Ax=b$.

**(b)** Minimizing a linear function over a halfspace: $\text{minimize } c^{\mathsf T}x \text{ subject to } a^{\mathsf T}x\le b$，其中 $a\neq0$。

**(c)** Minimizing a linear function over a rectangle: $\text{minimize } c^{\mathsf T}x \text{ subject to } l\preceq x\preceq u$，其中 $l\preceq u$。

**(d)** Minimizing a linear function over the probability simplex: $\text{minimize } c^{\mathsf T}x \text{ subject to } \mathbf 1^{\mathsf T}x=1,\ x\succeq0$。若把等式约束换成不等式 $\mathbf 1^{\mathsf T}x\le1$ 会怎样？（并给出投资组合解释。）

**(e)** Minimizing a linear function over a unit box with a total budget constraint: $\text{minimize } c^{\mathsf T}x \text{ subject to } \mathbf 1^{\mathsf T}x=\alpha,\ 0\preceq x\preceq1$，其中 $\alpha$ 是 $0$ 到 $n$ 的整数。若 $\alpha$ 非整数（但 $0\le\alpha\le n$）会怎样？若把等式换成 $\mathbf 1^{\mathsf T}x\le\alpha$ 呢？

**中文：** 给出下列各 LP 的显式解。(a) 仿射集上最小化线性函数；(b) 半空间上最小化线性函数；(c) 矩形（盒约束）上最小化线性函数；(d) 概率单纯形上最小化线性函数（含投资组合解释）；(e) 带总预算约束的单位盒上最小化线性函数。

**解答：**

### (a) 仿射集上最小化线性函数

```math
\text{minimize } c^{\mathsf T}x \quad\text{subject to } Ax=b .
```

分三种情形：

- **不可行**：$b\notin \mathcal R(A)$（$b$ 不在 $A$ 的列空间/值域中）。此时可行集为空，最优值 $p^\star=+\infty$（约定）。
- **可行且 $c$ 正交于 $A$ 的零空间（nullspace）**：把 $c$ 分解为 $c=A^{\mathsf T}\lambda+\hat c$，其中 $A\hat c=0$（$\hat c$ 是 $c$ 在零空间上的分量，$A^{\mathsf T}\lambda$ 正交于零空间）。若 $\hat c=0$，则 $c=A^{\mathsf T}\lambda$，在可行集上目标函数退化为常数：

```math
c^{\mathsf T}x=\lambda^{\mathsf T}Ax+\hat c^{\mathsf T}x=\lambda^{\mathsf T}b .
```

  故最优值 $p^\star=\lambda^{\mathsf T}b$，所有可行解都是最优解。
- **可行且 $c\notin\mathcal R(A^{\mathsf T})$（即 $\hat c\neq0$）**：问题无界，$p^\star=-\infty$。验证：取 $x=x_0-t\hat c$。因 $A\hat c=0$，$Ax=A x_0=b$ 对所有 $t$ 成立，故 $x$ 恒可行。而 $c^{\mathsf T}x=c^{\mathsf T}x_0-t\hat c^{\mathsf T}\hat c=c^{\mathsf T}x_0-t\|\hat c\|^2$，随 $t\to+\infty$ 趋于 $-\infty$。

综上：

```math
p^\star=
\begin{cases}
+\infty, & b\notin\mathcal R(A),\\
\lambda^{\mathsf T}b, & c=A^{\mathsf T}\lambda \text{（对某个 }\lambda\text{）},\\
-\infty, & \text{其他}.
\end{cases}
```

### (b) 半空间上最小化线性函数

```math
\text{minimize } c^{\mathsf T}x \quad\text{subject to } a^{\mathsf T}x\le b,\quad a\neq0 .
```

该问题恒可行（例如取 $x$ 沿 $-a$ 方向足够远）。把 $c$ 分解为平行于 $a$ 与正交于 $a$ 的分量：

```math
c=a\lambda+\hat c,\qquad a^{\mathsf T}\hat c=0 .
```

- 若 $\lambda>0$，问题无下界。取 $x=-ta$ 令 $t\to\infty$：$c^{\mathsf T}x=-t\,c^{\mathsf T}a=-t\,\lambda a^{\mathsf T}a\to-\infty$，且 $a^{\mathsf T}x-b=-t\,a^{\mathsf T}a-b\le0$ 对足够大的 $t$ 成立，故 $x$ 可行。直观上沿 $-a$ 方向走得很远，可得任意负的目标值。
- 若 $\hat c\neq0$，问题也无下界。取 $x=ba-t\hat c$（注意取合适的缩放使 $a^{\mathsf T}x$ 满足约束；正交分量 $\hat c$ 不影响 $a^{\mathsf T}x$），沿 $-\hat c$ 方向走远，$c^{\mathsf T}x=b\,c^{\mathsf T}a-t\|\hat c\|^2\to-\infty$。
- 若 $c=a\lambda$ 且 $\lambda\le0$，则最优值 $p^\star=\lambda b$（取 $x=b a/\|a\|^2$ 使约束紧）。

综上：

```math
p^\star=
\begin{cases}
\lambda b, & c=a\lambda \text{ 且 }\lambda\le0,\\
-\infty, & \text{其他}.
\end{cases}
```

### (c) 矩形上最小化线性函数

```math
\text{minimize } c^{\mathsf T}x \quad\text{subject to } l\preceq x\preceq u,\quad l\preceq u .
```

目标与约束都是可分离（separable）的：目标 $c^{\mathsf T}x=\sum_i c_ix_i$ 各项只依赖一个变量，每个约束也只依赖一个变量。故可对每个分量独立最小化：$x_i^\star$ 在 $l_i\le x_i\le u_i$ 下最小化 $c_ix_i$。若 $c_i>0$ 则 $x_i^\star=l_i$；若 $c_i<0$ 则 $x_i^\star=u_i$；若 $c_i=0$ 则区间内任意 $x_i$ 都最优。故最优值为

```math
p^\star=l^{\mathsf T}c^+ + u^{\mathsf T}c^-,
```

其中 $c_i^+=\max\{c_i,0\}$，$c_i^-=\max\{-c_i,0\}$（即 $c=c^+-c^-$）。

### (d) 概率单纯形上最小化线性函数

```math
\text{minimize } c^{\mathsf T}x \quad\text{subject to } \mathbf 1^{\mathsf T}x=1,\ x\succeq0 .
```

设 $c$ 的分量升序排列为 $c_1=c_2=\cdots=c_k\lt c_{k+1}\le\cdots\le c_n$。对任意可行 $x$，

```math
c^{\mathsf T}x\ge c_1(\mathbf 1^{\mathsf T}x)=c_{\min},
```

等号成立当且仅当 $x_1+\cdots+x_k=1$、$x_1,\dots,x_k\ge0$、$x_{k+1}=\cdots=x_n=0$。故最优值 $p^\star=c_1=c_{\min}$。

投资组合解释：$x_i$ 是投入资产 $i$ 的资金比例，每项投资回报固定为 $-c_i$，总回报为 $-c^{\mathsf T}x$（最大化它即最小化 $c^{\mathsf T}x$）。若回报固定已知，则把所有资金投到回报最高的资产（即 $c$ 最小的分量）。

若把等式 $\mathbf 1^{\mathsf T}x=1$ 换成不等式 $\mathbf 1^{\mathsf T}x\le1$，则多了「不投入部分资金」的选项，最优值变为

```math
p^\star=\min\{0,c_{\min}\}.
```

（若 $c_{\min}\le0$，$x$ 的选择同上；否则取 $x=0$。）

### (e) 带总预算约束的单位盒上最小化线性函数

```math
\text{minimize } c^{\mathsf T}x \quad\text{subject to } \mathbf 1^{\mathsf T}x=\alpha,\ 0\preceq x\preceq1,
```

其中 $\alpha$ 是 $0$ 到 $n$ 的整数。先考虑整数 $\alpha$。设

```math
c_1\le\cdots\le c_{i-1}<c_i=\cdots=c_\alpha=\cdots=c_k<c_{k+1}\le\cdots\le c_n .
```

最优值为 $c_1+c_2+\cdots+c_\alpha$，即 $c$ 中最小的 $\alpha$ 个元素之和。$x$ 最优当且仅当

```math
x_1=\cdots=x_{i-1}=1,\qquad x_i+\cdots+x_k=\alpha-i+1,\qquad x_{k+1}=\cdots=x_n=0 .
```

若 $\alpha$ 非整数（但 $0\le\alpha\le n$），则最优值为

```math
p^\star=c_1+c_2+\cdots+c_{\lfloor\alpha\rfloor}+c_{1+\lfloor\alpha\rfloor}\,(\alpha-\lfloor\alpha\rfloor).
```

（即最小的 $\lfloor\alpha\rfloor$ 个元素全取，再加第 $\lfloor\alpha\rfloor+1$ 小元素按剩余分数 $\alpha-\lfloor\alpha\rfloor$ 取一部分。）

若改成不等式约束 $\mathbf 1^{\mathsf T}x\le\alpha$（$\alpha$ 为 $0$ 到 $n$ 的整数），则最优值为 $c$ 中 $\alpha$ 个最小**非正**系数之和（因为负的系数才值得取，正的系数取 $0$ 更好）。

---

## 题目 4.17 最优活动水平（Optimal activity levels）

**题干：** We consider the selection of $n$ nonnegative activity levels $x_1,\dots,x_n$. These activities consume $m$ resources, which are limited. Activity $j$ consumes $A_{ij}x_j$ of resource $i$. The total resource consumption is additive, so $c_i=\sum_{j=1}^{n}A_{ij}x_j$. Each resource consumption is limited: $c_i\le c_i^{\max}$. Each activity generates revenue, a piecewise-linear concave function of the activity level:

```math
r_j(x_j)=
\begin{cases}
p_jx_j, & 0\le x_j\le q_j,\\
p_jq_j+p_j^{\mathrm{disc}}(x_j-q_j), & x_j\ge q_j .
\end{cases}
```

Here $p_j>0$ is the basic price（基础价）, $q_j>0$ is the quantity discount level（数量折扣阈值）, and $p_j^{\mathrm{disc}}$ the quantity discount price（折扣价），满足 $0\lt p_j^{\mathrm{disc}}\lt p_j$。目标是在资源约束下最大化总收益 $\sum_{j=1}^{n}r_j(x_j)$。说明如何把该问题写成 LP。

**中文：** 考虑选择 $n$ 个非负活动水平 $x_1,\dots,x_n$，它们消耗 $m$ 种有限资源。活动 $j$ 消耗资源 $i$ 的量为 $A_{ij}x_j$，总消耗可加，即 $c_i=\sum_{j}A_{ij}x_j$，需满足 $c_i\le c_i^{\max}$。每项活动产生收益，收益是活动水平的分段线性凹函数（见上式，含基础价 $p_j$、数量折扣阈值 $q_j$、折扣价 $p_j^{\mathrm{disc}}$）。目标是在资源约束下最大化总收益。说明如何把该问题建模为 LP。

**解答：**

基本问题可写成

```math
\begin{aligned}
\text{maximize}\quad & \sum_{j=1}^{n}r_j(x_j)\\
\text{subject to}\quad & x\succeq0\\
& Ax\preceq c^{\max}.
\end{aligned}
```

这是凸优化问题：目标 $r_j$ 是凹函数，约束是线性不等式组。

为转化为等价 LP，先把收益函数写成其凹函数表示（两个仿射函数的逐点下确界）：

```math
r_j(x_j)=\min\{p_jx_j,\ p_jq_j+p_j^{\mathrm{disc}}(x_j-q_j)\}.
```

（因 $r_j$ 是两条直线的 $\min$，且 $0\lt p_j^{\mathrm{disc}}\lt p_j$ 保证拐点处两条线相交。）于是 $r_j(x_j)\ge u_j$ 当且仅当

```math
p_jx_j\ge u_j,\qquad p_jq_j+p_j^{\mathrm{disc}}(x_j-q_j)\ge u_j .
```

（即 $u_j$ 不超过 $\min$ 中的任一项。）据此构造 LP：

```math
\begin{aligned}
\text{maximize}\quad & \mathbf 1^{\mathsf T}u\\
\text{subject to}\quad & x\succeq0\\
& Ax\preceq c^{\max}\\
& p_jx_j\ge u_j,\\
& p_jq_j+p_j^{\mathrm{disc}}(x_j-q_j)\ge u_j,\quad j=1,\dots,n,
\end{aligned}
```

变量为 $x$ 和 $u$。

等价性说明：固定 $x$，最后两组约束保证 $u_j\le r_j(x)$，所以任意可行的 $(x,u)$ 都使 LP 目标 $\sum u_j\le\sum r_j(x)$（不超过总收益）。另一方面，总可取 $u_j=r_j(x)$，此时两个目标相等。故 LP 与原问题等价。$\blacksquare$

---

## 附加题 1 最优活动水平数值解

**题干：** Solve the optimal activity level problem of exercise 4.17 for the instance with data

```math
A=\begin{bmatrix}1&2&0&1\\0&0&3&1\\0&3&1&1\\2&1&2&5\\1&0&3&2\end{bmatrix},\quad
c^{\max}=\begin{bmatrix}100\\100\\100\\100\\100\end{bmatrix},\quad
p=\begin{bmatrix}3\\2\\7\\6\end{bmatrix},\quad
p^{\mathrm{disc}}=\begin{bmatrix}2\\1\\4\\2\end{bmatrix},\quad
q=\begin{bmatrix}4\\10\\5\\10\end{bmatrix}.
```

给出最优活动水平、每项收益、总收益，以及每项的平均单价（收益/活动水平，应介于基础价与折扣价之间），并简要解释解。

**中文：** 求解 4.17 的最优活动水平问题实例（数据如上）。给出最优活动水平、每项产生的收益、最优解产生的总收益，以及每项活动水平的平均单价（即收益与活动水平之比，应介于各项基础价与折扣价之间），并简要评述所得解。

**解答：**

用 Matlab/CVX 求解（代码略，核心：`maximize(sum(min(p.*x, p.*q+pdisc.*(x-q))))`，约束 `x>=0; A*x<=cmax`）。结果为

```math
x=\begin{bmatrix}4.0000\\22.5000\\31.0000\\1.5000\end{bmatrix},\qquad
r=\begin{bmatrix}12.0000\\32.5000\\139.0000\\9.0000\end{bmatrix}.
```

总收益

```math
\text{totr}=\sum_j r_j=12+32.5+139+9=192.5000 .
```

平均单价

```math
\text{avgPrice}_j=r_j/x_j=\begin{bmatrix}3.0000\\1.4444\\4.4839\\6.0000\end{bmatrix}.
```

**评述：** 第 3 项活动的活动水平最高（$x_3=31$），也是基础价最高（$p_3=7$）的活动。由于它同时有较高的折扣价（$p_3^{\mathrm{disc}}=4$），其活动水平超过了折扣阈值 $q_3=5$，因而对总收益的贡献最大（$r_3=139$）。第 4 项活动的折扣价（$2$）远低于其基础价（$6$），故其活动水平低于折扣阈值 $q_4=10$；且它消耗大量资源（$A$ 第 4 列较大），故活动水平被压得很低（$x_4=1.5$）。

---

## 附加题 2 在 CVX 中改写约束

**题干：** Each of the following CVX code fragments describes a convex constraint on scalar variables $x,y,z$, but violates the CVX rule set, and so is invalid. Briefly explain why each is invalid, then rewrite it in an equivalent CVX-conforming form.

(a) `norm([x+2*y, x-y]) == 0`
(b) `square(square(x+y)) <= x-y`
(c) `1/x + 1/y <= 1; x>=0; y>=0`
(d) `norm([max(x,1), max(y,2)]) <= 3*x+y`
(e) `x*y >= 1; x>=0; y>=0`
(f) `(x+y)^2 / sqrt(y) <= x-y+5`
(g) `x^3 + y^3 <= 1; x>=0; y>=0`
(h) `x+z <= 1+sqrt(x*y-z^2); x>=0; y>=0`

**中文：** 下列各 CVX 代码片段描述的是标量变量 $x,y,z$ 上的凸约束，但违反了 CVX 规则集（DCP 规则），故无效。简要说明各自无效原因，并改写成等价的符合 CVX 规则的形式。

**解答：**

**(a)** 左侧 `norm(...)` 被正确识别为凸函数，但**等式约束只允许仿射（affine）的左右两侧**。由于向量的范数为零当且仅当向量本身为零，可改写为 `x+2*y == 0; x-y == 0`，或简化为 `x==0; y==0`。

**(b)** 问题在于 `square()` 只能接受仿射参数——它凸但**非单调递增**（在负半轴递减）。改用 `square_pos()`（定义域 $\mathbb R_+$ 上单调递增的凸函数）：

```matlab
square_pos(square(x+y)) <= x-y
```

  也可引入辅助变量 $t$ 分解：

```matlab
square(x+y) <= t
square(t) <= x-y
```

  注意：一般而言引入新变量分解不必然成立，此处成立是因为外层 `square` 在 $\mathbb R_+$ 上凸且单调。

  另一种写法：直接化为多项式 `(x+y)^4 <= x-y`。

**(c)** `1/x` 不是凸函数（除非把定义域限制到 $\mathbb R_{++}$）。改写为 `inv_pos(x) + inv_pos(y) <= 1`。`inv_pos` 的定义域是 $\mathbb R_{++}$，故 $x>0,y>0$ 被隐式包含。

**(d)** `norm()` 只能接受仿射参数（它凸但非单调递增）。引入新变量 $u,v$：

```matlab
norm([u; v]) <= 3*x+y
max(x,1) <= u
max(y,2) <= v
```

  此处分解成立，因为 `norm` 在 $\mathbb R_+^2$ 上凸且单调（特别在 $(1,\infty)\times(2,\infty)$ 上）。

**(e)** $xy$ 不是凹函数，故原写法无效。可改写为 `x >= inv_pos(y)`（$x,y$ 可互换）；或 `geomean([x,y]) >= 1`；也可用 LMI 表示：

```matlab
[x 1; 1 y] == semidefinite(2)
```

  （即 $\begin{bmatrix}x&1\\1&y\end{bmatrix}\succeq0\iff xy\ge1,\ x,y\ge0$。）

**(f)** 失败原因是「凸函数除以凹函数」。改写为：

```matlab
quad_over_lin(x+y, sqrt(y)) <= x-y+5
```

  可行是因为 `quad_over_lin` 对第二个参数单调递减，故能接受凹函数 `sqrt(y)`（而 `sqrt` 是凹的）。

**(g)** 函数 $x^3+y^3$ 在 $x\ge0,y\ge0$ 上凸，但 $x^3$ 在 $x<0$ 时非凸，故 CVX 会拒绝。改写方法一：

```matlab
quad_pos_over_lin(square(x),x) + quad_pos_over_lin(square(y),y) <= 1
```

  （`quad_pos_over_lin` 对第一参数凸且递增，可接受凸函数；而 `quad_over_lin` 对第一参数不递增，不能用。）更简单的方法二：

```matlab
pow_pos(x,3) + pow_pos(y,3) <= 1
```

**(h)** 问题是 $xy$ 非凹，导致 CVX 拒绝。注意到

```math
\sqrt{xy-z^2}=\sqrt{y\,(x-z^2/y)},
```

  可改写为：

```matlab
x+z <= 1 + geomean([x - quad_over_lin(z,y), y])
```

  可行是因为 `geomean` 是凹且对各分量非递减的，故其第一分量可接受凹函数 $x-\text{quad\_over\_lin}(z,y)$。

**验证：** 可写一个（显然不可行的）可行性问题让 CVX 处理以确认无报错，代码如下（把各改写后的约束并排放置即可）。

---

## 附加题 3 照明问题（Illumination problem）

**题干：** This exercise concerns the illumination problem from lecture 1. Take $I_{\text{des}}=1$ and $p^{\max}=1$, so the problem is

```math
\begin{aligned}
\text{minimize}\quad & f_0(p)=\max_{k=1,\dots,n}\big|\log(a_k^{\mathsf T}p)\big|\\
\text{subject to}\quad & 0\le p_j\le1,\quad j=1,\dots,m,
\end{aligned}
\tag{1}
```

with variable $p\in\mathbb{R}^n$. As mentioned in the lecture, the problem is equivalent to

```math
\begin{aligned}
\text{minimize}\quad & \max_{k=1,\dots,n}h(a_k^{\mathsf T}p)\\
\text{subject to}\quad & 0\le p_j\le1,\quad j=1,\dots,m,
\end{aligned}
\tag{2}
```

where $h(u)=\max\{u,1/u\}$ for $u>0$. The function $h$ is nonlinear, nondifferentiable, and convex. Give several approximate solutions and compare to the exact solution for the instance with $m=10$ lamps, $n=20$ patches, $a_{kj}=r_{kj}^{-2}\max\{\cos\theta_{kj},0\}$.

**中文：** 本题涉及第 1 讲（9–11 页）的照明问题。取 $I_{\text{des}}=1$、$p^{\max}=1$，问题化为式 (1)（变量 $p\in\mathbb{R}^n$）。讲座中已说明它等价于式 (2)，其中 $h(u)=\max\{u,1/u\}$（$u>0$），该函数非线性、不可微但凸。对 $m=10$ 盏灯、$n=20$ 个补丁、$a_{kj}=r_{kj}^{-2}\max\{\cos\theta_{kj},0\}$ 的实例，计算若干近似解并与精确解比较。

**解答：**

**等价性说明：** 注意

```math
f_0(p)=\max_k\big|\log(a_k^{\mathsf T}p)\big|
=\max_k\max\{\log(a_k^{\mathsf T}p),\,\log(1/a_k^{\mathsf T}p)\}
=\log\max_k\max\{a_k^{\mathsf T}p,\,1/a_k^{\mathsf T}p\}
=\log\max_k h(a_k^{\mathsf T}p).
```

因为 $\log$ 单调递增，最小化 $f_0$ 等价于最小化 $\max_k h(a_k^{\mathsf T}p)$。

[[FIGURE: 函数 $h(u)=\max\{u,1/u\}$ 的图像（$u>0$，V 形，最低点在 $u=1$）]]

[[FIGURE: 灯/补丁几何布局示意]]

下面给出四种启发式方法加精确解的结果（数据见 `illum_data.m`）：

1. **等功率（Equal lamp powers）**：取 $p_j=\gamma$，在 $[0,1]$ 上扫描 $\gamma$（用 `logspace(-3,0,1000)`），求使 $f_0$ 最小的 $\gamma$。
2. **带饱和的最小二乘（Least-squares with saturation）**：解 $\min\|Ap-\mathbf1\|_2^2$（`A\ones(n,1)`），负值截为 0、大于 1 截为 1。
3. **正则化最小二乘（Regularized least-squares）**：解 $\min\|Ap-\mathbf1\|_2^2+\rho\|p-\tfrac12\mathbf1\|_2^2$，增大 $\rho$ 直到所有 $p_j\in[0,1]$（取满足 $\|p-0.5\|_\infty\le0.5$ 的最小 $\rho$）。
4. **切比雪夫逼近（Chebyshev approximation）**：解 $\min\|Ap-\mathbf1\|_\infty$ 且 $0\le p_j\le1$（用 CVX：`norm(A*p-ones(n,1),inf)`）。这是把 $h(u)$ 用分段线性函数 $|u-1|+1$ 近似（在 $u=1$ 附近是好近似）。

[[FIGURE: $|u-1|+1$ 的图像，在 $u=1$ 附近对 $h(u)$ 近似良好]]

5. **精确解（Exact solution）**：用 CVX 解 $\min\max_k\max(a_k^{\mathsf T}p,\,1/a_k^{\mathsf T}p)$（用 `inv_pos` 表示 $1/a_k^{\mathsf T}p$），约束 $0\le p_j\le1$。

结果汇总如下表（$f_0(p)$ 及各灯功率 $p_1,\dots,p_{10}$）：

| 量 | 方法 1 等功率 | 方法 2 最小二乘 | 方法 3 正则化 | 方法 4 切比雪夫 | 精确解 |
|---|---|---|---|---|---|
| $f_0(p)$ | 0.4693 | 0.8628 | 0.4439 | 0.4198 | **0.3575** |
| $p_1$ | 0.3448 | 1 | 0.5004 | 1 | 1 |
| $p_2$ | 0.3448 | 0 | 0.4777 | 0.1165 | 0.2023 |
| $p_3$ | 0.3448 | 1 | 0.0833 | 0 | 0 |
| $p_4$ | 0.3448 | 0 | 0.0002 | 0 | 0 |
| $p_5$ | 0.3448 | 0 | 0.4561 | 1 | 1 |
| $p_6$ | 0.3448 | 1 | 0.4354 | 0 | 0 |
| $p_7$ | 0.3448 | 0 | 0.4597 | 1 | 1 |
| $p_8$ | 0.3448 | 1 | 0.4307 | 0.0249 | 0.1882 |
| $p_9$ | 0.3448 | 0 | 0.4034 | 0 | 0 |
| $p_{10}$ | 0.3448 | 1 | 0.4526 | 1 | 1 |

**结论：** 精确解给出最小目标值 $f_0=0.3575$，优于所有启发式方法。切比雪夫逼近（0.4198）最接近精确解，其次是正则化最小二乘（0.4439）；等功率（0.4693）与带饱和的最小二乘（0.8628，最差）性能较差。精确解中若干灯满功率（$p=1$）、若干灯关断（$p=0$），呈「开关」式的最优分配结构。
