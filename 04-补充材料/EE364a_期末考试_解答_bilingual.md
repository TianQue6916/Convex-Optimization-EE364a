# EE364a《Convex Optimization》期末考试 2008 解答 双语译注

> 说明：本文为 Stanford EE364a 凸优化课程 2008 年期末考试（Final exam）**解答**的中文双语译注，覆盖全部 32 页（Problem 1–7）。中文为主体，英文术语以 `English（中文）` 形式首次行内嵌入，之后仅保留英文；公式使用 `$...$` 行内与独立 ```math 围栏，不使用美元双符号 display math，不使用 Unicode 上下标。题目层级严格保留（Problem / (a) / (b)），题号照原文。

## 考试说明（Cover page，第 1 页）

**EE364a Convex Optimization I（凸优化 I）**，授课教师 Prof. S. Boyd，考试时间为 2008 年 3 月 14–15 日或 3 月 15–16 日，此为**期末考试解答（final exam solutions）**。

你可以使用任何书籍、笔记或计算机程序（例如 Matlab、cvx），但在 3 月 18 日所有人都考完之前，不得与任何人讨论本考试。唯一的例外是你可以通过课程邮箱向助教提问澄清。我们已经尽力让考试无歧义、表述清晰，所以大概率不会多说什么。

请在上交前复印一份自己的试卷。

请把**封面页（cover page）**附在试卷最前面。按题号顺序装订你的解答（problem 1、problem 2、problem 3、……），每道题另起一页。把每道题相关的一切内容（例如文字、代码、图）放在一起；不要把代码或图附在试卷末尾。

对于冗长而毫无必要复杂的解答，即使答案正确，我们也会扣分。我们的解答并不长，所以如果你发现某道题的解答写了很多页还停不下来，就应该设法找出更简单的做法。我们期望每个人的试卷都整洁、字迹清晰，包括那些以 Cr/N（credit/no-credit，及格/不及格）方式修课的同学。

当一道题涉及计算时，你必须给出以下全部内容：对你究竟做了什么给出清晰的讨论与论证（justification）、产生该结果的 Matlab（或其他）源代码，以及最终的数值结果或图。

要下载含题目数据的 Matlab 文件，你必须把题目里给出的完整 URL 输入浏览器；课程网页上没有指向这些文件的链接。例如，要获取名为 `filename.m` 的文件，你需要在浏览器中检索

```
http://www.stanford.edu/class/ee364a/final-data/filename.m
```

请遵守荣誉准则（honor code）。虽然我们允许你们以小组形式完成作业，但你不得与任何人讨论期末考试，至少在所有人都考完之前不行。

所有题目权重相同。有些题目比乍看之下更简单。

请下载最新版本的 cvx。

考试期间请经常检查邮件，以防我们需要发布重要通知。

---

## Problem 1（第 1 题，第 2–3 页）

**Optimal investment to fund an expense stream（为融资一笔支出流而进行的最优投资）。** 一个组织（例如市政当局）已知自己未来 $T$ 个周期（periods）内的运营支出（operating expenses），记为 $E_1,\dots,E_T$。（通常这些值是正的；但也可以有负的 $E_t$，负值对应收入。）这些支出将由两部分资金共同覆盖：一部分是投资收入（investment income），来自在 $t=0$ 时刻购买的一篮子债券（bonds）；另一部分是一个现金账户（cash account）。

债券产生投资收入，记为 $I_1,\dots,I_T$。现金余额（cash balance）记为 $B_0,\dots,B_T$，其中 $B_0\ge 0$ 是存入现金账户的初始存款（initial deposit）金额。对于 $t=1,\dots,T$，我们可以有 $B_t<0$，这代表借款（borrowing）。

在周期 $t$，用投资收入和现金支付完支出后，我们手头剩下 $B_t-E_t+I_t$ 的现金。如果这个金额为正，它就按利率 $r^{+}>0$ 赚取利息；如果为负，我们就必须按利率 $r^{-}$ 支付利息，其中 $r^{-}\ge r^{+}$。因此，支出、投资收入和现金余额之间的递推关系如下：

```math
B_{t+1}=
\begin{cases}
(1+r^{+})(B_t-E_t+I_t), & B_t-E_t+I_t\ge 0,\\
(1+r^{-})(B_t-E_t+I_t), & B_t-E_t+I_t<0,
\end{cases}
\qquad t=1,\dots,T-1.
```

我们取 $B_1=(1+r^{+})B_0$，并且要求 $B_T-E_T+I_T=0$，这意味着最终的现金余额加上收入，恰好覆盖最终的支出。

初始投资将是一篮子债券，编号为 $1,\dots,n$。债券 $i$ 有价格 $P_i>0$、票息支付（coupon payment）$C_i>0$、到期日（maturity）$M_i\in\{1,\dots,T\}$。债券 $i$ 产生的收入流为

```math
a^{(i)}_t=
\begin{cases}
C_i, & t<M_i,\\
C_i+1, & t=M_i,\\
0, & t>M_i,
\end{cases}
\qquad t=1,\dots,T.
```

若 $x_i$ 是（在 $t=0$ 时刻）购买的债券 $i$ 的份数，则总投资现金流（cash flow）为

```math
I_t=x_1 a^{(1)}_t+\cdots+x_n a^{(n)}_t,\qquad t=1,\dots,T.
```

我们要求 $x_i\ge 0$。（$x_i$ 可以是分数，不必是整数。）

购买这些债券、并在 $t=0$ 时刻为初始现金余额注资所需的**总初始投资（total initial investment）**为 $x_1P_1+\cdots+x_nP_n+B_0$。

**(a)** 说明如何选择 $x$ 和 $B_0$，以最小化为覆盖该支出流所需的总初始投资。

**(b)** 求解 `opt_funding_data.m` 中给出的问题实例。给出 $x$ 和 $B_0$ 的最优值，给出最优的总初始投资，并将其与“不购买任何债券（即所有支出都从现金账户支付）”时所需的初始投资进行比较。分别画出“有最优债券投资”与“无债券投资”两种情形下的现金余额（对周期）图。

### Solution（解答）

**(a)** 我们将给出几种解法（它们很接近，但并不完全相同）。

**Method 1（方法 1）。**

现金余额的递推方程

```math
B_{t+1}=
\begin{cases}
(1+r^{+})(B_t-E_t+I_t), & B_t-E_t+I_t\ge 0,\\
(1+r^{-})(B_t-E_t+I_t), & B_t-E_t+I_t<0,
\end{cases}
```

可以等价地表达为

```math
B_{t+1}=\min\Big\{(1+r^{+})(B_t-E_t+I_t),\ (1+r^{-})(B_t-E_t+I_t)\Big\},
```

> **为什么这个 min 表达式与分段定义等价**：因为 $r^{-}\ge r^{+}$，所以 $(1+r^{-})\ge(1+r^{+})$。当 $B_t-E_t+I_t\ge 0$ 时，用较小的系数 $(1+r^{+})$ 乘非负数得到的值最小，故 min 取第一支；当 $B_t-E_t+I_t<0$ 时，用较大的系数 $(1+r^{-})$ 乘负数会得到更小（更负）的结果，故 min 取第二支。于是这个 $\min$ 恰好复现了分段定义的两支。

当把 $B_t$ 当作变量时，这些方程显然**不是**凸的（convex）。我们将描述两种处理方法。

在第一种方法中，我们把递推方程**松弛（relax）**成下面的凸不等式（convex inequalities）：

```math
B_{t+1}\le\min\Big\{(1+r^{+})(B_t-E_t+I_t),\ (1+r^{-})(B_t-E_t+I_t)\Big\}.
```

> **术语 · convex inequality（凸不等式）**：形如 $f(x)\le 0$ 且 $f$ 为凸函数的约束。这里每个 min 里的两项都是 $B_t$ 的仿射（affine）函数，而 $\min$ 作用于两个凸函数上结果仍是凸的（凸函数的逐点最小值是凸的），所以“$B_{t+1}\le\min\{\cdot,\cdot\}$”这一约束的可行集是凸的。原式之所以非凸，是因为等号（而非“$\le$”）把一个凸集合的边界取出来，边界一般不是凸的。

经过这一松弛，问题可以表述为：

```math
\begin{aligned}
&\text{minimize} && B_0+\sum_{i=1}^{n}P_i x_i \\
&\text{subject to} && \sum_{i=1}^{n}a^{(i)}_t x_i=I_t, && t=1,\dots,T \\
&&& x\succeq 0, \\
&&& B_0\ge 0 \\
&&& B_1=(1+r^{+})B_0 \\
&&& B_{t+1}\le\min\Big\{(1+r^{+})(B_t-E_t+I_t),\ (1+r^{-})(B_t-E_t+I_t)\Big\}, && t=1,\dots,T-1 \\
&&& B_T-E_T+I_T=0,
\end{aligned}
```

其中变量为 $B_0,\dots,B_T,\ I_1,\dots,I_T,\ x_1,\dots,x_n$。（我们也可以把 $I_t$ 当作一个仿射表达式而不是变量来处理；这没有区别。）

> **术语 · $\succeq$（分量不小于）**：向量不等式 $x\succeq 0$ 表示 $x$ 的每个分量都 $\ge 0$，即 $x_i\ge 0$ 对所有 $i$。

> **为什么这是线性规划（LP）**：目标 $B_0+\sum_i P_i x_i$ 是变量的仿射函数；约束 $\sum_i a^{(i)}_t x_i=I_t$、$B_1=(1+r^{+})B_0$、$B_T-E_T+I_T=0$ 都是仿射等式；$x\succeq 0$、$B_0\ge 0$ 是线性不等式。唯一的“非平凡”约束是 $B_{t+1}\le\min\{\cdot,\cdot\}$，而 $\min$ 里的两项都是 $B_t$ 的仿射函数，因此 $\min$ 也是凸函数，故这个不等式是凸的。由于目标线性、约束全部仿射/凸，整个问题是一个线性规划。

现在我们要论证：**这个问题的任何解都必须满足**

```math
B_{t+1}=\min\Big\{(1+r^{+})(B_t-E_t+I_t),\ (1+r^{-})(B_t-E_t+I_t)\Big\},\qquad t=1,\dots,T-1,
```

这意味着通过求解上面的 LP，我们其实也在求解原问题。假设并非如此；例如，假设 $\tau$ 是第一个满足

```math
B_{\tau+1}<\min\Big\{(1+r^{+})(B_\tau-E_\tau+I_\tau),\ (1+r^{-})(B_\tau-E_\tau+I_\tau)\Big\}
```

的周期。当然，这种情况有点荒唐：它意味着我们“主动选择”在周期 $\tau$ 和 $\tau+1$ 之间扔掉一部分现金。为了给出一个正式论证说明这种情况不可能发生，我们首先注意到，$B_{t+1}$ 关于 $B_t$ 是**单调不减的（monotonically nondecreasing）**，对所有 $t$ 都成立，因为 min 表达式里的每一项都关于 $B_t$ 单调递增。

> **为什么 $B_{t+1}$ 关于 $B_t$ 单调不减**：min 里的两项分别是 $(1+r^{+})(B_t-E_t+I_t)$ 和 $(1+r^{-})(B_t-E_t+I_t)$，二者都是 $B_t$ 的斜率为正（分别是 $1+r^{+}$、$1+r^{-}$，均为正）的仿射函数，故都随 $B_t$ 递增。两个单调递增函数的逐点最小值仍然是单调递增的（$\min$ 保序：若 $B_t$ 增大，两支都增大，较小的一支也只会增大或不变）。

因此，我们可以减小 $B_\tau$，直到

```math
B_{\tau+1}=\min\Big\{(1+r^{+})(B_\tau-E_\tau+I_\tau),\ (1+r^{-})(B_\tau-E_\tau+I_\tau)\Big\},
```

并且仍然维持现金流的可行性（feasibility）。通过归纳（induction），我们可以继续减小 $B_{\tau-1},B_{\tau-2},\dots,B_0$，并且仍然得到一个可行解。

> **为什么能一路减小到 $B_0$ 且仍可行**：因为我们把 $B_\tau$ 减小之后，为了让递推式继续成立（或满足“$\le$”的松弛），前面各期的 $B_t$ 也可以相应减小——这正是“$B_{t+1}$ 关于 $B_t$ 单调不减”这一性质的作用方向：减小前一期的 $B_t$ 只会使后一期的上界变小（或不变），绝不会使它违反下界约束。因此我们可以在不破坏任何约束的前提下，把 $B_0$ 一路压小。这个新的现金流是可行的，并且减小了 $B_0$，这正说明原来的点不是最优的。

> **结论（为什么松弛后 LP 与原问题等价）**：若 LP 的某个可行解在某处取严格不等式 $B_{\tau+1}<\min\{\cdot,\cdot\}$，我们就总能再压小 $B_0$ 得到更优（目标更小）的可行解，故这样的点在最优解中不可能出现。因此最优解必然处处取等号，即满足原始的分段递推方程。于是松弛问题（一个凸的 LP）与原（非凸的）问题具有相同的最优解与最优值，求解 LP 即可。

---

## Problem 1（第 1 题，续：第 4–8 页）

### Solution（解答，续）

**Method 2（方法 2）。**

第二种方法做了一种不同的**松弛（relaxation）**。在这个方法里，我们不把 $B_1,\dots,B_T$ 当作变量；而是把它们看作 $x$ 与 $B_0$ 的函数，由一条递推式（recursion）来定义。我们将证明 $B_T(B_0,x)$ 是 $B_0$ 与 $x$ 的**凹函数（concave function）**。

$B_1(B_0,x)=(1+r^{+})B_0$ 是一个凹（事实上是仿射 affine）函数。如果 $B_t(B_0,x)$ 是凹函数，那么

```math
\begin{aligned}
B_{t+1}(B_0,x)={}&\min\Big\{(1+r^{+})\big(B_t(B_0,x)-E_t+I_t(x)\big),\\
&(1+r^{-})\big(B_t(B_0,x)-E_t+I_t(x)\big)\Big\},\qquad t=1,\dots,T-1
\end{aligned}
```

也是凹函数，因为它是两个凹函数的**逐点最小值（pointwise minimum）**。因此，我们可以下结论：$B_T(B_0,x)$ 是凹函数。

> **术语 · concave function（凹函数）**：若 $-f$ 是凸函数，则称 $f$ 为凹函数；等价地，对任意 $x,y$ 与 $\theta\in[0,1]$ 有 $f(\theta x+(1-\theta)y)\ge\theta f(x)+(1-\theta)f(y)$。
>
> **术语 · pointwise minimum（逐点最小值）**：函数 $h(x)=\min\{f_1(x),f_2(x)\}$。关键性质：**两个凹函数的逐点最小值仍是凹函数**。理由——$h$ 是 $\mathrm{hypo}f_1\cap\mathrm{hypo}f_2$（下图中两个下水平集之交）的“上包络”，而凹函数的下图（hypograph）是凸集，两个凸集之交仍是凸集，故 $h$ 的 hypograph 是凸集，即 $h$ 凹。（这是与“凸函数的逐点最大值仍是凸函数”相对偶的结论。）

> **为什么这里用归纳法（induction）**：第一步验证了 $t=1$ 时 $B_1$ 是凹（仿射）函数，作为归纳基础。归纳假设 $B_t$ 凹；由于 $I_t(x)=\sum_i a^{(i)}_t x_i$ 关于 $x$ 是仿射的，故 $B_t(B_0,x)-E_t+I_t(x)$ 关于 $(B_0,x)$ 仍是凹函数；再乘以正的系数 $(1+r^{+})$ 或 $(1+r^{-})$ 仍保持凹性；最后对两个凹函数取 $\min$ 仍凹。这样就把凹性从 $B_t$ 传递到了 $B_{t+1}$，从而对所有 $t$ 成立。

最终的现金余额约束 $B_T(B_0,x)-E_T+I_T(x)=0$ **显然不是凸的**，因为 $B_T(B_0,x)$ 是一个凹函数。为了处理这个等式，我们把它**松弛（relax）**为凸不等式

```math
B_T(B_0,x)-E_T+I_T(x)\ge 0.
```

> **为什么 $B_T(B_0,x)-E_T+I_T(x)=0$ 非凸**：等式约束的可行集是函数 $B_T-E_T+I_T$ 的零水平集（level set）$\{z:g(z)=0\}$。若 $g$ 是凹函数，则 $\{z:g(z)\ge 0\}$ 是凸集（凹函数的上水平集 superlevel set 是凸的），但零水平集 $\{g=0\}$ 是凸集与凸集 $\{g\le 0\}$ 的“边界”，一般不是凸的。因此带等号的约束非凸，而带“$\ge$”的松弛是凸的。

经过这一松弛，问题可以表述为：

```math
\begin{aligned}
&\text{minimize} && B_0+\sum_{i=1}^{n}P_i x_i \\
&\text{subject to} && x\succeq 0,\\
&&& B_0\ge 0\\
&&& B_T(B_0,x)-E_T+I_T(x)\ge 0,
\end{aligned}
```

其中变量为 $B_0$ 与 $x_1,\dots,x_n$。这里 $B_T(B_0,x)$ 与 $I_T(x)$ 由下面的递推定义给出：

```math
\begin{aligned}
B_1(B_0,x)&=(1+r^{+})B_0\\
B_{t+1}(B_0,x)&=\min\Big\{(1+r^{+})\big(B_t(B_0,x)-E_t+I_t(x)\big),\\
&\qquad\qquad\ (1+r^{-})\big(B_t(B_0,x)-E_t+I_t(x)\big)\Big\},\qquad t=1,\dots,T-1\\
I_t(x)&=\sum_{i=1}^{n}a^{(i)}_t x_i,\qquad t=1,\dots,T.
\end{aligned}
```

我们可以论证：这个问题的任何解都必须满足

```math
B_T(B_0,x)-E_T+I_T(x)=0,
```

这与上面（针对不等式松弛）给出的论证非常类似。

> **为什么最优解必取等号**：这个论证与 Method 1 中的“单调性”论证结构相同。关键观察是 $B_T(B_0,x)$ 关于 $B_0$ 单调不减：$B_1=(1+r^{+})B_0$ 随 $B_0$ 递增，而 $\min$ 保序（$B_t$ 增大则 $B_{t+1}$ 也增大或不变），故 $B_T$ 关于 $B_0$ 单调不减。若某可行解满足严格不等式 $B_T-E_T+I_T>0$，我们就可以略微减小 $B_0$（从而减小目标 $B_0+\sum_i P_i x_i$），同时因为 $B_T$ 关于 $B_0$ 单调不减，新的 $B_T$ 会变小，但只要我们减小得足够少，就仍能保持 $B_T-E_T+I_T\ge 0$ 成立。这样得到了目标更小的可行解，说明原来带严格不等式的点不是最优。因此最优解必然满足等号，松弛问题与原问题等价。

**Method 3（方法 3）。**

这是该问题的又一种解法。现金余额的传播方程（propagation equations），

```math
B_{t+1}=
\begin{cases}
(1+r^{+})(B_t-E_t+I_t), & B_t-E_t+I_t\ge 0,\\
(1+r^{-})(B_t-E_t+I_t), & B_t-E_t+I_t<0,
\end{cases}
\qquad t=1,\dots,T-1,
```

等价于下面的 LP 约束：

```math
\begin{aligned}
B_{t+1}&=(1+r^{+})S^{+}_t-(1+r^{-})S^{-}_t,\\
B_t-E_t+I_t&=S^{+}_t-S^{-}_t,\\
S^{+}_t&\ge 0,\\
S^{-}_t&\ge 0.
\end{aligned}
```

> **为什么这等价于分段定义**：我们把净余额 $B_t-E_t+I_t$ 用两个非负变量之差 $S^{+}_t-S^{-}_t$ 表示（即把净余额拆成正部与负部）。假设 $B_t-E_t+I_t$ 为正，则取 $S^{+}_t=B_t-E_t+I_t$ 且 $S^{-}_t=0$，于是 $B_{t+1}=(1+r^{+})(B_t-E_t+I_t)$。若 $B_t-E_t+I_t$ 为负，则取 $S^{+}_t=0$ 且 $S^{-}_t=-(B_t-E_t+I_t)=|B_t-E_t+I_t|$（注意此时 $B_t-E_t+I_t=S^{+}_t-S^{-}_t$ 仍成立），于是 $B_{t+1}=-(1+r^{-})|B_t-E_t+I_t|=(1+r^{-})(B_t-E_t+I_t)$。这样两支分别复现了分段定义中的两种情形。

这个等价性很容易证明：假设 $B_t-E_t+I_t$ 为正，那么 $S^{+}_t=B_t-E_t+I_t$ 且 $S^{-}_t=0$，所以 $B_{t+1}=(1+r^{+})(B_t-E_t+I_t)$。若 $B_t-E_t+I_t$ 为负，那么 $S^{+}_t=0$ 且 $S^{-}_t=B_t-E_t+I_t$，所以 $B_{t+1}=(1+r^{-})(B_t-E_t+I_t)$。

> **注记 · 关于 $S^{+}_t$、$S^{-}_t$ 的符号约定**：原文这里写 $S^{-}_t=B_t-E_t+I_t$ 是略去绝对值号的简写，其含义是“$S^{-}_t$ 取为净余额的绝对值”。正式写法应是：当净余额为负时，令 $S^{-}_t=-(B_t-E_t+I_t)\ge 0$，从而 $B_{t+1}=(1+r^{+})\cdot 0-(1+r^{-})S^{-}_t=(1+r^{-})(B_t-E_t+I_t)$，与分段定义一致。

因此，问题可以表述为一个 LP：

```math
\begin{aligned}
&\text{minimize} && B_0+\sum_{i=1}^{n}P_i x_i \\
&\text{subject to} && \sum_{i=1}^{n}a^{(i)}_t x_i=I_t, && t=1,\dots,T \\
&&& x\succeq 0,\\
&&& B_0\ge 0\\
&&& B_1=(1+r^{+})B_0\\
&&& B_T-E_T+I_T=0,\\
&&& B_{t+1}=(1+r^{+})S^{+}_t-(1+r^{-})S^{-}_t, && t=1,\dots,T-1\\
&&& B_t-E_t+I_t=S^{+}_t-S^{-}_t, && t=1,\dots,T-1\\
&&& S^{+}_t\ge 0, && t=1,\dots,T-1\\
&&& S^{-}_t\ge 0, && t=1,\dots,T-1,
\end{aligned}
```

其中变量为 $B_0,\dots,B_T,\ I_1,\dots,I_T,\ x_1,\dots,x_n,\ S^{+}_1,\dots,S^{+}_{T-1}$ 与 $S^{-}_1,\dots,S^{-}_{T-1}$。

> **术语 · LP（linear program，线性规划）**：目标函数是仿射函数、约束全部是仿射等式或仿射不等式的凸优化问题。这里目标 $B_0+\sum_i P_i x_i$ 是仿射的，所有约束（等式 $\sum_i a^{(i)}_t x_i=I_t$、$B_{t+1}=(1+r^{+})S^{+}_t-(1+r^{-})S^{-}_t$、$B_t-E_t+I_t=S^{+}_t-S^{-}_t$、$B_1=(1+r^{+})B_0$、$B_T-E_T+I_T=0$ 都是仿射等式；$x\succeq 0$、$B_0\ge 0$、$S^{+}_t\ge 0$、$S^{-}_t\ge 0$ 都是仿射不等式），故是 LP。

用这种办法，我们还必须论证：对任意 $t$，$S^{+}_t$ 或 $S^{-}_t$ 中总有一个为零。（这一点是容易看出的。）

> **为什么 $S^{+}_t$ 与 $S^{-}_t$ 不会同时为正**：约束给出 $S^{+}_t-S^{-}_t=B_t-E_t+I_t$ 与 $B_{t+1}=(1+r^{+})S^{+}_t-(1+r^{-})S^{-}_t$。若某解中 $S^{+}_t$ 与 $S^{-}_t$ 同时为正，我们可以把二者同时减去同一个正数 $\delta=\min\{S^{+}_t,S^{-}_t\}$：令 $\tilde S^{+}_t=S^{+}_t-\delta$、$\tilde S^{-}_t=S^{-}_t-\delta$。这样 $S^{+}_t-S^{-}_t$ 不变（约束 $B_t-E_t+I_t=S^{+}_t-S^{-}_t$ 仍成立），但 $B_{t+1}=(1+r^{+})S^{+}_t-(1+r^{-})S^{-}_t$ 会减少 $\delta[(1+r^{+})-(1+r^{-})]=\delta(r^{-}-r^{+})\ge 0$（因为 $r^{-}\ge r^{+}$）。$B_{t+1}$ 变小不会破坏任何约束（$B_{t+1}$ 只作为现金余额出现在后续期，变小是更“保守”的可行方向，而最终约束 $B_T-E_T+I_T=0$ 可通过后续调整维持）。因此最优解中 $S^{+}_t$、$S^{-}_t$ 不会同时为正，二者至少有一个为零，于是 $S^{+}_t,S^{-}_t$ 恰好分别代表净余额的正部与负部。

**(b) % Optimal investment to fund an expense stream（为融资支出流的最优投资）**

```matlab
clear all; close all;
opt_funding_data;
cvx_begin
variables x(n) B0 B(T) I(T)
minimize( P'*x+B0 )
subject to
I == A*x
x >= 0 % use x == 0 for no bond purchase
B0 >= 0
B(1) == (1+rp)*B0
for t = 1:T-1
B(t+1) <= min( (1+rp)*(B(t)-E(t)+I(t)), (1+rn)*(B(t)-E(t)+I(t)) )
end
B(T)-E(T)+I(T) == 0
cvx_end
```

> **注记 · 代码与 Method 1 的对应**：这段 cvx 代码对应 Method 1 的“不等式松弛”。`B(t+1) <= min(...)` 实现的是松弛后的凸不等式 $B_{t+1}\le\min\{\cdot,\cdot\}$；`I == A*x` 实现 $I_t=\sum_i a^{(i)}_t x_i$（矩阵 $A$ 的第 $i$ 列为 $(a^{(i)}_1,\dots,a^{(i)}_T)$）；`rp`、`rn` 分别是 $r^{+}$、$r^{-}$ 的变量名；`P'*x` 即 $\sum_i P_i x_i$。

```matlab
% optimal total initial investment
cvx_optval
% associated optimal cash balance plot
bar(0:T,[B0;B]);
ylabel('cash balance'); xlabel('t');
```

> **注记**：`cvx_optval` 输出最优目标值（即最优总初始投资）；`bar(0:T,[B0;B])` 画出各周期现金余额的柱状图（把初始余额 $B_0$ 与 $B_1,\dots,B_T$ 拼接成一个向量）。

下面给出采用 $B_t(B_0,x)$ 递推定义的代码（cvx 部分）。

```matlab
cvx_begin
variables x(n) B0
minimize( P'*x+B0 )
subject to
x >= 0 % use x == 0 for no bond purchase
B0 >= 0
B(1) = (1+rp)*B0
for t = 1:T-1
B(t+1)=min((1+rp)*(B(t)-E(t)+A(t,:)*x),(1+rn)*(B(t)-E(t)+A(t,:)*x));
end
B(T)-E(T)+A(T,:)*x >= 0
cvx_end
```

> **注记 · 与 Method 2 的对应**：这里不再把 $B_t$ 声明为变量，而是用 `min` 表达式逐步赋值得到 $B_t$ 的表达式（即 Method 2 中把 $B_t$ 视为 $B_0,x$ 的函数）。最后一行用松弛不等式 `B(T)-E(T)+A(T,:)*x >= 0`（对应 $B_T-E_T+I_T\ge 0$），其中 `A(t,:)*x` 即 $I_t(x)=\sum_i a^{(i)}_t x_i$。

下面给出采用 LP 形式的代码（cvx 部分）。

```matlab
cvx_begin
variables x(n) B0 B(T) I(T) Sp(T-1) Sn(T-1)
minimize( P'*x+B0 )
subject to
I == A*x
x >= 0 % use x == 0 for no bond purchase
B0 >= 0
B(1) == (1+rp)*B0
Sp >= 0
Sn >= 0
for t = 1:T-1
B(t)-E(t)+I(t) == Sp(t) - Sn(t);
B(t+1) == (1+rp)*Sp(t)-(1+rn)*Sn(t)
end
B(T)-E(T)+I(T) == 0
cvx_end
```

> **注记 · 与 Method 3 的对应**：`Sp`、`Sn` 分别对应 $S^{+}_t$、$S^{-}_t$；循环内的两条约束正是 $B_t-E_t+I_t=S^{+}_t-S^{-}_t$ 与 $B_{t+1}=(1+r^{+})S^{+}_t-(1+r^{-})S^{-}_t$。

最优债券投资为

```math
x=(0.0000,\ 18.9326,\ 0.0000,\ 0.0000,\ 13.8493,\ 8.9228),
```

且现金账户的最优初始存款为 $B_0=0$。最优的总初始投资为 $40.7495$。

> **注记 · 数值结果解读**：$x$ 的六个分量中只有第 2、5、6 个非零，说明最优策略只购买这三种债券；$B_0=0$ 表示初始不必往现金账户注资，全部靠债券收入滚动覆盖支出。

（第 7 页）

**Figure 1: Cash balance with optimal bond purchase（有最优债券购买时的现金余额）。** 横轴为周期 $t=0,\dots,12$，纵轴为现金余额（cash balance），范围约 $-10$ 到 $10$。图中现金余额在各期上下波动，$t=0$ 处余额为 $0$（对应 $B_0=0$），随后在正负之间波动，最终回到约束要求的终值（约 $0$）。

**Figure 2: Cash balance with no bond purchase（无债券购买时的现金余额）。** 横轴为周期 $t=0,\dots,12$，纵轴为现金余额，范围约 $0$ 到 $45$。图中 $t=0$ 处余额接近 $41.8$（对应 $B_0=41.7902$），此后现金余额逐期下降，直到终期降到约 $0$，即完全靠初始大额存款逐期支付支出。

> **注记 · 两张图的对比**：图 1 说明通过最优配置债券，初始几乎无需注资（$B_0=0$），现金流靠债券票息与到期本金在正负之间滚动；图 2 说明若不买债券（$x=0$），则必须在期初一次性存入约 $41.79$，之后余额单调消耗到零。

（第 8 页）

如果不做任何债券投资（即 $x=0$），则需要一笔初始存款 $B_0=41.7902$，这比我们投资债券时所需的金额高出约 $2.5\%$。

> **数值核对**：无债券时 $B_0=41.7902$，有债券时最优总初始投资为 $40.7495$。二者之比 $\dfrac{41.7902}{40.7495}\approx 1.0255$，即高出约 $2.55\%\approx 2.5\%$，与原文表述一致。

---

## Problem 2（第 2 题，第 9–10 页）

**Utility versus latency trade-off in a network（网络中效用与延迟的权衡）。** 考虑一个有 $m$ 条边（edge）的网络，边编号为 $1,\dots,m$，以及 $n$ 条流（flow），流编号为 $1,\dots,n$。每条流有一个与之关联的**非负流率（flow rate）** $f_j$；每条边（或链路 link）有一个与之关联的**正容量（capacity）** $c_i$。每条流经过一个固定的链路集合（即它的路由 route）；链路 $i$ 上的总流量（traffic） $t_i$ 是所有经过链路 $i$ 的流的流率之和。各流的路由由**路由矩阵（routing matrix）** $R\in\mathbb{R}^{m\times n}$ 描述，其定义为

```math
R_{ij}=
\begin{cases}
1, & \text{流 }j\text{ 经过链路 }i,\\
0, & \text{否则}.
\end{cases}
```

于是链路流量向量 $t\in\mathbb{R}^{m}$ 由 $t=Rf$ 给出。链路容量约束可表达为 $Rf\preceq c$。（对数）网络效用（network utility）定义为

```math
U(f)=\sum_{j=1}^{n}\log f_j.
```

链路 $i$ 上的（平均排队）延迟（queuing delay）为

```math
d_i=\frac{1}{c_i-t_i}
```

（再乘以一个常数，这个常数对我们无关紧要）。当 $t_i=c_i$ 时我们取 $d_i=\infty$。流 $j$ 的延迟或时延（latency），记为 $l_j$，是流 $j$ 经过的所有链路上的链路延迟之和。我们定义最大流延迟为

```math
L=\max\{l_1,\dots,l_n\}.
```

给定 $R$ 与 $c$；我们要选择 $f$。

**(a)** 若忽略流延迟，你将如何找到使效用 $U$ 最大的流率？（特别地，我们允许 $L=\infty$。）我们把所能达到的最大效用记为 $U_{\max}$。

**(b)** 若忽略效用，你将如何找到使最大流延迟 $L$ 最小的流率？（特别地，我们允许 $U=-\infty$。）我们把所能达到的最小延迟记为 $L_{\min}$。

**(c)** 说明如何找到效用 $U$（我们希望最大化它）与延迟 $L$（我们希望最小化它）之间的**最优权衡（trade-off）**。

**(d)** 对 `net_util_data.m` 中给出的网络数据，求出 $U_{\max}$、$L_{\min}$，并画出效用对延迟的最优权衡曲线，在同一张图上标出 $L_{\min}$ 与 $U_{\max}$。你的图应覆盖从 $L=1.1L_{\min}$ 到 $L=11L_{\min}$ 的范围。纵轴画 $U$，用线性刻度；横轴画 $L$，用对数刻度（log scale）。

**注记。** 对于 (a)、(b)、(c)，你的回答可以涉及求解一个或多个凸优化（convex optimization）问题。但如果存在更简单的解法，你应当指出。

### Solution（解答）

**(a)** 为最大化效用，我们求解凸问题

```math
\begin{aligned}
&\text{maximize} && \sum_{j=1}^{n}\log f_j\\
&\text{subject to} && Rf\preceq c,
\end{aligned}
```

其中变量为 $f$，并有隐式约束 $f\succeq 0$。

> **为什么这是凸优化问题，且 $f\succeq 0$ 是“隐式”的**：目标 $\sum_j\log f_j$ 是凹函数（$\log$ 是凹函数，各分量之和仍凹），我们是在**最大化**一个凹函数，等价于最小化凸函数 $-\sum_j\log f_j$；约束 $Rf\preceq c$ 是仿射不等式，故可行集是多面体（凸集）。$f\succeq 0$ 不必显式写出，因为 $\log f_j$ 的定义域（domain）本身就要求 $f_j>0$，任何使目标有定义的 $f$ 自动满足 $f_j>0$。因此这是凸优化问题。

**(b)** 链路延迟关于流量是**单调递增（monotonically increasing）**的，所以它在流量为零时最小。由于流延迟是各链路延迟之和，它也在取 $f=0$ 时最小。所以零流使每条流的延迟都最小。当 $f=0$ 时我们有 $d_i=1/c_i$。为把这些延迟沿各路由求和，我们乘以 $R^{T}$，得到

```math
l=R^{T}(1/c_1,\dots,1/c_m),
```

其中 $l$ 是流延迟向量。因此我们有

```math
L_{\min}=\max\Big(R^{T}(1/c_1,\dots,1/c_m)\Big),
```

其中 $\max$ 是对向量 $R^{T}(1/c_1,\dots,1/c_m)$ 各分量取最大值。

> **为什么零流使延迟最小**：链路延迟 $d_i=\dfrac{1}{c_i-t_i}$ 关于 $t_i$ 单调递增（分母随 $t_i$ 增大而减小，故 $d_i$ 增大），而 $t_i$ 是经过链路 $i$ 的各流率之和，随每个 $f_j$ 单调递增。因此要最小化每个 $d_i$（进而最小化每条流延迟 $l_j=\sum_{i:\text{流}j\text{经过}i}d_i$ 与最大延迟 $L$），应让所有 $f_j$ 尽量小，即取 $f=0$（受 $f\succeq 0$ 约束，最小值就是 $0$）。

> **为什么 $l=R^{T}(1/c_1,\dots,1/c_m)$**：流 $j$ 的延迟是它经过的所有链路的 $d_i$ 之和。$R$ 的第 $i$ 行对应链路 $i$，第 $j$ 列对应流 $j$，$R_{ij}=1$ 当且仅当流 $j$ 经过链路 $i$。因此 $\sum_i R_{ij}d_i$ 恰好就是流 $j$ 的延迟 $l_j$。写成向量形式：$l=R^{T}d$，其中 $d=(1/c_1,\dots,1/c_m)$ 是各链路延迟向量。$R^{T}$ 的第 $j$ 行是 $R$ 的第 $j$ 列，与 $d$ 做内积即 $\sum_i R_{ij}(1/c_i)=l_j$。

**(c)** 设 $b^{T}_1,\dots,b^{T}_m$ 表示 $R$ 的行。为找到 $U$ 与 $L$ 之间的最优权衡，我们对一段 $L$ 的取值求解问题

```math
\begin{aligned}
&\text{maximize} && \sum_{j=1}^{n}\log f_j\\
&\text{subject to} && Rf\preceq c,\\
&&& \sum_{i=1}^{m}\frac{R_{ij}}{c_i-b^{T}_i f}\le L,\qquad j=1,\dots,n,
\end{aligned}
```

其中变量为 $f$，并有隐式约束 $f\succeq 0$。

> **为什么用 $b^{T}_i f$ 表示链路流量**：$b^{T}_i$ 是 $R$ 的第 $i$ 行，故 $b^{T}_i f=\sum_j R_{ij}f_j=t_i$，即链路 $i$ 的总流量。于是 $c_i-b^{T}_i f=c_i-t_i$ 是链路 $i$ 的剩余容量，$1/(c_i-b^{T}_i f)$ 即链路延迟 $d_i$。流 $j$ 的延迟 $l_j$ 是它经过的所有链路延迟之和，即 $\sum_{i}R_{ij}/(c_i-b^{T}_i f)$（因为 $R_{ij}=1$ 时该项计入，$R_{ij}=0$ 时该项为 $0$，故可直接对所有 $i$ 求和）。约束 $l_j\le L$ 对每个 $j$ 成立，就等价于 $L=\max_j l_j$ 不超过给定值 $L$。

显然，若 $Rf\preceq c$ 则必有 $c_i-b^{T}_i f\ge 0$，所以约束

```math
\sum_{i=1}^{m}\frac{R_{ij}}{c_i-b^{T}_i f}\le L,\qquad j=1,\dots,n,
```

是凸的。因此这是一个凸优化问题。

> **为什么这些约束是凸的**：函数 $c_i-b^{T}_i f$ 关于 $f$ 是仿射的，且在可行集上（由 $Rf\preceq c$ 保证）非负。函数 $g(y)=1/y$ 在 $y>0$ 上是凸的、递减的。因此 $1/(c_i-b^{T}_i f)$ 是“凸的、非增的 $g$ 与仿射的 $c_i-b^{T}_i f$ 的复合”，由复合规则（composition rule）知它是凸函数（concave 仿射内函数配凸递减外函数 → 凸）。凸函数的非负线性组合（系数 $R_{ij}\in\{0,1\}$）仍是凸的，故 $\sum_i R_{ij}/(c_i-b^{T}_i f)$ 是凸函数，其“$\le L$”的下水平集约束是凸约束。目标仍是凹函数（最大化），故整体是凸优化问题。

> **如何用 (c) 得到权衡曲线**：对一系列递增的 $L$ 值（从 $L_{\min}$ 到较大值），逐个求解上面的凸问题，得到对应的最大效用 $U(L)$，再画出 $(L,U(L))$ 曲线。当 $L\ge$ 某个阈值后，效用达到 (a) 中的 $U_{\max}$（延迟约束不再起作用）；当 $L$ 逼近 $L_{\min}$ 时，效用趋近 $-\infty$（对应 $f\to 0$）。

**(d)** 下面的代码计算效用对延迟的权衡曲线。

```matlab
% solution for network utility problem
clear all;
net_util_data;
% let's find max utility with no delay constraint
cvx_begin
variable f(n)
maximize geomean(f)
R*f <= c
f >= 0; % not needed; enforced by geomean domain
cvx_end
Umax=sum(log(f));
```

> **注记 · 用 `geomean` 求 $U_{\max}$**：`geomean(f)` 是 $f$ 各分量的几何平均 $(\prod_j f_j)^{1/n}$，它是凹函数，故 `maximize geomean(f)` 是凸（凹）问题。几何平均与 $\sum_j\log f_j$ 有相同的最大化点（因为 $\log$ 是单调递增的，$(\prod_j f_j)^{1/n}$ 与 $\log(\prod_j f_j)^{1/n}=\frac1n\sum_j\log f_j$ 单调同序），所以 `maximize geomean(f)` 与 (a) 中 `maximize sum log(f_j)` 给出相同的 $f$。代码随后用 `Umax=sum(log(f))` 直接计算该 $f$ 下的效用值 $U_{\max}$。

---

## Problem 2（第 2 题）解答 · 收尾（第 11–12 页）

> 承接上一文件的 Problem 2（用带宽分配最大化效用 utility 并约束最大延迟 latency 的问题），此处是解答中「绘制 Pareto 曲线（Pareto curve）」部分的代码与图。

```matlab
% 让我们求无效用约束时的最小延迟
% 可以解析地求出：直接取 f = 0
Lmin = max(R'*(1./c));
% 现在来画 Pareto 曲线
N = 20;
ds = 1.10*Lmin*logspace(0,1,N);
% 从高出 L 的 10% 处开始
Uopt = [];
for d = ds
    cvx_begin
        variable f(n)
        maximize geomean(f);
        R'*inv_pos(c-R*f) <= d*ones(n,1)
        f >= 0; % 不需要；已由 geomean 的定义域强制
        R*f <= c; % 不需要；已由 inv_pos 的定义域强制
    cvx_end
    Uopt = [Uopt n*log(cvx_optval)];
end
semilogx(ds,Uopt,'k-',[Lmin,ds],[Umax,ones(1,N)*Umax],...
    'k--',[1,1]*Lmin,[Uopt(1),Umax],'k--')
% axis([ds(1), ds(N), -480, -340]);
xlabel('L'); ylabel('U');
```

> **代码解读（为什么要这样写）**：
>
> - `Lmin = max(R'*(1./c))`：当完全不做效用约束（$f=0$，即不分配任何带宽给用户流量时），延迟由链路容量 $c$ 独自决定。链路 $j$ 的延迟约为 $1/c_j$，用路由矩阵 $R$ 把链路容量映射到各用户路径上（$R^T(1./c)$），取最大值即为该系统能实现的最小最大延迟（minimum latency）$L_{\min}$。这一步是解析下界，无需优化。
> - `ds = 1.10*Lmin*logspace(0,1,N)`：在 $[L_{\min},\,10L_{\min}]$ 之间按对数间隔取 $N=20$ 个延迟上界 $d$，从略高于下界（高出 10%）一直扫到 10 倍，覆盖整个 Pareto 前沿（Pareto frontier）上「低延迟—低效用」到「高延迟—高效用」的折中范围。
> - 循环体对每个 $d$ 求解一个**最大化 $\mathrm{geomean}(f)$（几何平均效用）**、约束 $R^T(c-Rf)^{-1}\le d\,\mathbf{1}$ 的 convex optimization 子问题。目标 $\log\det$-型凸问题的变形，用 `geomean` 表示几何平均，是 concave 的目标，故 `maximize` 成立。
> - `inv_pos(c-R*f)` 是分量取倒数的正部分，对应延迟表达式 $1/(c_j-(Rf)_j)$；其定义域自动强制 $R f<c$，故注释里说 `R*f <= c` 与 `f >= 0` 不必显式写出——`inv_pos` 的定义域要求 $c-Rf>0$，`geomean` 的定义域要求 $f>0$。
> - `Uopt` 记录每个 $d$ 对应的最优效用值（`cvx_optval` 是几何平均，取对数再乘 $n$ 得到 $\log\prod f_i$ 形式的效用标度）。
> - `semilogx` 用对数横轴画出效用 $U$ 对延迟 $L$ 的 trade-off 曲线，实线为折中曲线，两条虚线分别标出 $L_{\min}$ 与 $U_{\max}$ 的位置。

折中曲线（trade-off curve）如图 3 所示。

---

![Figure 3](figure3.png)

> **图 3 说明**：横轴为最大延迟 $L$（对数坐标，$10^1$ 到 $10^3$），纵轴为效用 $U$（范围约 $-480$ 到 $-340$）。实线（solid）是 utility 对 maximum latency 的折中曲线，两条虚线（dashed）分别标出 $L_{\min}$ 与 $U_{\max}$ 的位置。曲线从左下到右上单调上升，体现「延迟越松（$L$ 越大），可达效用 $U$ 越高」的 Pareto 折中关系。

---

## Problem 3（第 3 题，第 13–15 页）

**Optimal design of a tensile structure（张拉结构的最优设计）。** 一个张拉结构（tensile structure）被建模为 $\mathbb{R}^2$ 中 $n$ 个质点（masses）的集合，其中一部分质点被固定（fixed），其余质点由 $N$ 根弹簧（springs）连接。各质点处于平衡（equilibrium）状态：弹簧力（spring forces）、固定质点处的连接力（connection forces）与重力（gravity）相互平衡。（当各质点的位置使下文定义的总能量最小化时，即达到这种平衡。）

我们用 $(x_i,y_i)\in\mathbb{R}^2$ 表示质点 $i$ 的位置，$m_i>0$ 表示其质量。前 $p$ 个质点被固定，即对 $i=1,\dots,p$ 有 $x_i=x_i^{\mathrm{fixed}}$ 且 $y_i=y_i^{\mathrm{fixed}}$。质点 $i$ 的重力势能（gravitational potential energy）为 $g m_i y_i$，其中 $g\approx 9.8$ 是重力加速度（gravitational acceleration）。

假设弹簧 $j$ 连接质点 $r$ 与质点 $s$，则其弹性势能（elastic potential energy）为

```math
\frac{1}{2}k_j\Big((x_r-x_s)^2+(y_r-y_s)^2\Big),
```

其中 $k_j\ge 0$ 是弹簧 $j$ 的刚度（stiffness）。

为描述拓扑结构（topology，即哪些弹簧连接哪些质点），我们使用关联矩阵（incidence matrix）$A\in\mathbb{R}^{n\times N}$，其定义为

```math
A_{ij}=
\begin{cases}
\ \ 1, & \text{弹簧 }j\text{ 的头（head）连到质点 }i,\\
-1, & \text{弹簧 }j\text{ 的尾（tail）连到质点 }i,\\
\ \ 0, & \text{其他情形}.
\end{cases}
```

这里我们为每根弹簧任意指定一个「头」和「尾」，但实际上弹簧是完全对称的，头尾的选择可以互换而不会产生任何影响。（希望你最终会发现，用关联矩阵 $A$ 来描述系统的拓扑为何很方便。）

总能量（total energy）是「所有质点的重力势能之和」加上「所有弹簧的弹性势能之和」。各质点的平衡位置（equilibrium positions）是「在所有前 $p$ 个位置被固定的约束下，使总能量最小化的点」。（在平衡位置处，作用在每个质点上的合力为零。）

我们用 $E_{\min}$ 表示系统处于平衡位置时的总能量。（假设能量有下界；当且仅当每个质点都能通过某组正刚度弹簧连接到某个固定质点时，能量才有下界。）

总能量 $E_{\min}$ 是该结构刚度的一个度量，$E_{\min}$ 越大意味着结构越刚硬（stiffer）。（我们可把 $E_{\min}=-\infty$ 视作一个无限柔软（infinitely unstiff）的结构；此时至少有一个质点甚至无法被支撑以抵抗重力。）

**(a)** 假设我们已知固定位置 $x_1^{\mathrm{fixed}},\dots,x_p^{\mathrm{fixed}}$，$y_1^{\mathrm{fixed}},\dots,y_p^{\mathrm{fixed}}$，质量值 $m_1,\dots,m_n$，弹簧拓扑 $A$，以及常数 $g$。你需要在满足预算约束（budget constraint）$1^T k=k_1+\cdots+k_N=k_{\mathrm{tot}}$（其中 $k_{\mathrm{tot}}$ 给定）的前提下，选择非负的 $k_1,\dots,k_N$。你的目标是最大化 $E_{\min}$。

请说明如何用凸优化（convex optimization）来完成此事。

**(b)** 对 `tens_struct_data.m` 中给出的问题数据执行你的方法。该文件定义了所有所需数据，并绘制了「刚度均匀分配到各弹簧（即 $k=(k_{\mathrm{tot}}/N)\mathbf{1}$）」时的平衡构型。

报告 $E_{\min}$ 的最优值。画出优化后的平衡构型，并与刚度均匀分配时的平衡构型进行比较。（相关代码在文件 `tens_struct_data.m` 中，但被注释掉了。）

### Solution（解答）

**(a)** $A^T x$ 给出各弹簧的 $x$ 方向位移所组成的向量，$A^T y$ 给出各弹簧的 $y$ 方向位移所组成的向量。

> **为什么 $A^T x$ 是弹簧位移**：关联矩阵 $A$ 的第 $j$ 列 $a_j$ 只在两个非零分量处取值（一头为 $+1$、一头为 $-1$，其余为 $0$），于是 $(A^T x)_j = a_j^T x = x_r - x_s$（头减尾），正是弹簧 $j$ 两端质点在 $x$ 方向的位移差。因此 $A^T x$ 的第 $j$ 个分量就是弹簧 $j$ 的 $x$ 方向伸长量。

能量作为 $x$、$y$ 与 $k$ 的函数为

```math
E(x,y,k)=\frac{1}{2}x^T A\,\operatorname{diag}(k)\,A^T x+\frac{1}{2}y^T A\,\operatorname{diag}(k)\,A^T y+c^T y,
```

其中 $c_i=g m_i$。这是关于 $k$ 的仿射函数（affine function）。因此最小能量就是该函数在所有满足固定约束的 $x$、$y$ 上的最小值。由此立即可得：$E_{\min}$ 是 $k$ 的凹函数（concave function）。

> **术语 · affine function（仿射函数）**：形如 $f(k)=Mk+b$ 的函数，即「线性项 + 常数项」。这里 $x^T A\,\operatorname{diag}(k)A^T x=\sum_j k_j (A^T x)_j^2$ 是 $k$ 的线性项（系数 $(A^T x)_j^2$ 是已固定的数），$c^T y$ 不含 $k$，故整体关于 $k$ 是仿射的。

> **术语 · concave function（凹函数）**：满足 $f(\theta u+(1-\theta)v)\ge\theta f(u)+(1-\theta)f(v)$ 对任意 $\theta\in[0,1]$ 成立的函数，即其「上方」是凸集、函数图像在弦之上。凹函数取最大是凸优化问题。

> **为什么 $E_{\min}$ 是 $k$ 的凹函数**：$E_{\min}(k)=\min_{x,y\,(\text{满足固定约束})} E(x,y,k)$。$E$ 作为 $k$ 的函数是仿射的（既凸又凹），而对一族（仿射的）凸函数逐点取最小，得到的函数仍是凸的；对称地，对一族仿射（因而也是凹）函数逐点取最小，得到的仍是凹函数。这正是「pointwise minimum（逐点最小）保凹性」的结论。因此 $E_{\min}(k)$ 是凹函数——这很好，因为我们正是要最大化它。

我们需要 $E_{\min}$ 的显式公式。为此，把 $A$ 分块为 $A_1$ 与 $A_2$，其中 $A_1\in\mathbb{R}^{p\times N}$ 由 $A$ 的前 $p$ 行构成，而 $A_2\in\mathbb{R}^{(n-p)\times N}$ 由 $A$ 的后 $n-p$ 行构成。

令 $\bar x,\bar y,\bar c\in\mathbb{R}^{n-p}$ 分别表示 $x$、$y$、$c$ 的最后 $n-p$ 个（即自由的 free）分量。最小能量可写为

```math
E_{\min}(k)=\min_{\bar x,\bar y}\left(\frac{1}{2}z_x^T\operatorname{diag}(k)z_x+\frac{1}{2}z_y^T\operatorname{diag}(k)z_y+\bar c^T\bar y+C\right),
```

其中

```math
C=\sum_{i=1}^{p} g m_i\, y_i^{\mathrm{fixed}}
```

并且

```math
\begin{aligned}
z_x &= A_2^T\bar x + b_x,\\
z_y &= A_2^T\bar y + b_y,\\
b_x &= A_1^T x^{\mathrm{fixed}},\\
b_y &= A_1^T y^{\mathrm{fixed}}.
\end{aligned}
```

> **为什么做这个分块**：前 $p$ 个质点的坐标是固定的（已知常数），只有后 $n-p$ 个是自由变量。把 $A$ 分成「对应固定质点的行 $A_1$」与「对应自由质点的行 $A_2$」，弹簧位移 $z_x=A^T x$ 就能拆成「自由部分 $A_2^T\bar x$」加「固定部分 $A_1^T x^{\mathrm{fixed}}=b_x$」的常向量。同理 $z_y$。而重力势能 $\sum_i g m_i y_i$ 中，固定质点的贡献 $\sum_{i=1}^{p} g m_i y_i^{\mathrm{fixed}}$ 是与 $\bar y$ 无关的常数 $C$，自由质点的贡献是 $\bar c^T\bar y$（其中 $\bar c_i=g m_{p+i}$）。

因此，为求 $E_{\min}$，我们只需对一个关于 $\bar x$ 与 $\bar y$ 的**无约束二次型（unconstrained quadratic）**取最小值。这给出

```math
E_{\min}(k)=\frac{1}{2}\Big(b_x^T D b_x - v_x^T Q^{-1} v_x\Big)+\frac{1}{2}\Big(b_y^T D b_y - v_y^T Q^{-1} v_y\Big)+C,
```

其中

```math
\begin{aligned}
D &= \operatorname{diag}(k),\\
Q &= A_2 D A_2^T,\\
v_x &= A_2 D b_x,\\
v_y &= A_2 D b_y + \bar c.
\end{aligned}
```

> **为什么这个显式公式成立（逐点最小化二次型）**：把 $\bar x$ 部分的能量写成
>
> ```math
> \frac{1}{2}z_x^T D z_x=\frac{1}{2}(A_2^T\bar x+b_x)^T D(A_2^T\bar x+b_x).
> ```
>
> 对 $\bar x$ 求导并令其为零，得梯度方程 $A_2 D(A_2^T\bar x+b_x)=A_2 D A_2^T\bar x+A_2 D b_x=Q\bar x+v_x=0$，故 $\bar x^{\star}=-Q^{-1}v_x$。代回得到最小值为 $\frac{1}{2}\big(b_x^T D b_x-v_x^T Q^{-1}v_x\big)$——这正是「二次型配方求极小」的标准结果：$\min_u \tfrac{1}{2}u^T Q u+v^T u=-\tfrac{1}{2}v^T Q^{-1}v$（当 $Q\succ 0$）。$y$ 部分完全同理，只是线性项多了一个 $+\bar c^T\bar y$，体现在 $v_y=A_2 D b_y+\bar c$ 上。二者相加再加常数 $C$，即得上式。

注意所有这些项都是关于 $k$ 的仿射函数（affine）。因此我们可以把「重新分配刚度」的问题写为

```math
\begin{aligned}
&\text{maximize} && b_x^T D b_x-v_x^T Q^{-1}v_x+b_y^T D b_y-v_y^T Q^{-1}v_y\\
&\text{subject to} && 1^T k=k_{\mathrm{tot}},\\
&&& k\succeq 0.
\end{aligned}
```

这是一个凸优化问题（convex optimization problem）。约束关于 $k$ 显然是凸的。目标函数含两项关于 $k$ 的仿射（因而也是凹）项，以及两项「仿射项的矩阵分式（matrix fractional）」的负值（因而也是凹的）。因此目标关于 $k$ 是凹的。

> **术语 · convex set（凸集）**：对任意两点 $u,v$，连线 $\{\theta u+(1-\theta)v:\theta\in[0,1]\}$ 仍完全落在集合内的集合。这里约束集 $\{k:1^T k=k_{\mathrm{tot}},\ k\succeq 0\}$ 是一个仿射超平面与正卦限的交，是凸集。

> **术语 · matrix fractional（矩阵分式）**：形如 $f(Q,v)=v^T Q^{-1}v$（$Q\succ 0$）的函数，关于 $(Q,v)$ 是凸的。因此它的负值 $-v^T Q^{-1}v$ 是凹的。这里 $Q=A_2 D A_2^T$ 与 $v_x,v_y$ 都是 $k$ 的仿射函数，把仿射映射代入凸函数复合后仍是凸的（convex 在仿射复合下保持），故 $-v_x^T Q^{-1}v_x$ 关于 $k$ 是凹的。

> **为什么目标关于 $k$ 是凹的**：$b_x^T D b_x=\sum_j k_j (b_x)_j^2$ 与 $b_y^T D b_y$ 都是 $k$ 的线性函数（系数非负），线性函数既凸又凹，视为凹项没问题。$v_x^T Q^{-1}v_x$ 是矩阵分式，作为 $k$ 的复合是凸的，前面加负号即为凹。凹函数之和仍凹。于是「最大化凹目标 + 凸约束」正是标准的凸优化问题。

> **结论**：问题 `maximize $E_{\min}(k)$ s.t. $1^T k=k_{\mathrm{tot}},\ k\succeq 0$` 是一个 concave maximization（凹函数最大化）问题，可用凸优化求解器（如 cvx）直接求解。

下面给出本问题的另一种解法。刚度分配问题是

```math
\begin{aligned}
&\text{maximize} && E_{\min}(k)\\
&\text{subject to} && 1^T k=k_{\mathrm{tot}},\\
&&& k\succeq 0.
\end{aligned}
```

设 $(x^{\star},y^{\star})$ 为刚度 $k$ 下的平衡质点坐标，并设 $\mu$ 为与 $k$ 的等式约束相关联的 Lagrange 乘子（Lagrange multiplier）。则刚度分配问题的 Lagrangian（拉格朗日函数）为

```math
\begin{aligned}
L(k,\mu)&=\frac{1}{2}\Big(x^{\star T}A\operatorname{diag}(k)A^T x^{\star}+y^{\star T}A\operatorname{diag}(k)A^T y^{\star}\Big)+c^T y^{\star}+\mu(1^T k-k_{\mathrm{tot}})\\
&=\sum_{j} k_j\Big(\frac{1}{2}\big(x^{\star T}a_j a_j^T x^{\star}+y^{\star T}a_j a_j^T y^{\star}\big)+\mu\Big)+c^T y^{\star}-\mu k_{\mathrm{tot}},
\end{aligned}
```

其中 $a_j$ 是 $A$ 的第 $j$ 列。然而，这恰好与下面这个优化问题的 Lagrangian 相同：

```math
\begin{aligned}
&\text{minimize} && c^T y-\mu k_{\mathrm{tot}}\\
&\text{subject to} && \frac{1}{2}x^T a_j a_j^T x+\frac{1}{2}y^T a_j a_j^T y+\mu\le 0, && j=1,\dots,N\\
&&& x_i=x_i^{\mathrm{fixed}}, && i=1,\dots,p\\
&&& y_i=y_i^{\mathrm{fixed}}, && i=1,\dots,p.
\end{aligned}
```

> **术语 · Lagrangian（拉格朗日函数）**：对问题 `minimize $f_0(x)$ s.t. $f_i(x)\le 0,\ h_j(x)=0$`，Lagrangian 为 $L(x,\lambda,\nu)=f_0(x)+\sum_i\lambda_i f_i(x)+\sum_j\nu_j h_j(x)$。这里等式约束 $1^T k=k_{\mathrm{tot}}$ 对应的乘子记为 $\mu$。

> **为什么把 $\operatorname{diag}(k)$ 拆成 $\sum_j k_j a_j a_j^T$**：由关联矩阵定义，$A\operatorname{diag}(k)A^T=\sum_j k_j a_j a_j^T$（其中 $a_j$ 是 $A$ 的第 $j$ 列），因为 $\operatorname{diag}(k)$ 是对角矩阵，$\operatorname{diag}(k)A^T$ 的第 $j$ 行是 $k_j a_j^T$，故 $A\operatorname{diag}(k)A^T=\sum_j k_j a_j a_j^T$。于是能量关于 $k$ 的每一项都显式分解为「每个弹簧 $j$ 单独贡献 $k_j(\frac12 x^T a_j a_j^T x+\frac12 y^T a_j a_j^T y)$」。

> **为什么这两个问题的 Lagrangian 相同（对偶关系）**：原刚度分配问题把 $k_j$ 当作变量、$x^{\star},y^{\star}$ 当作常数，其 Lagrangian 关于 $k_j$ 是线性的，系数正是 $\frac12 x^{\star T}a_j a_j^T x^{\star}+\frac12 y^{\star T}a_j a_j^T y^{\star}+\mu$。而第二个（对偶）问题把 $x,y$ 当作变量、$\mu$ 当作乘子，其 Lagrangian 展开后 $\sum_j \lambda_j(\frac12 x^T a_j a_j^T x+\frac12 y^T a_j a_j^T y+\mu)+c^T y-\mu k_{\mathrm{tot}}$，若令 Lagrange 乘子 $\lambda_j$ 对应到 $k_j$，则二者形式完全一致。因此这两个问题互为对偶（dual）。

这个优化问题的值等于最优的 $E_{\min}$。最优的弹簧刚度 $k^{\star}$ 等于对应于那些不等式约束的最优 Lagrange 乘子。

> **为什么最优刚度 $k^{\star}$ 就是不等式约束的乘子**：在第二个问题里，每个不等式约束 $\frac12 x^T a_j a_j^T x+\frac12 y^T a_j a_j^T y+\mu\le 0$ 对应一个非负 Lagrange 乘子，这个乘子恰好扮演原问题里弹簧 $j$ 的刚度 $k_j$ 的角色。这正是**互补松弛（complementary slackness）**的结构：对刚度为正的弹簧，其对应的约束在最优处取等号（绷紧）；对刚度为 0 的弹簧，其约束松弛。因此求解对偶问题得到的乘子向量即最优刚度分配 $k^{\star}$。

下面是一个**行不通**的做法（但被很多人提出过）：在「最小化 $x,y$」与「最大化 $k$」之间交替迭代。有些人论证说，这会对该问题收敛到局部最优（local optimum），而由于问题是凸的，局部最优也就是全局最优（global optimum）。然而，这种说法是不对的。

> **为什么交替最小化/最大化不成立**：交替在 $x,y$ 上最小、在 $k$ 上最大，对应的是对「极小化极大（minimax）」问题的坐标下降（coordinate descent）。但原问题是「$\max_k \min_{x,y}$」（先极小后极大），而交替迭代处理的是同一函数在不同块上的交替优化，并不保证收敛到真正的鞍点或最优值。凸性只在「对某一整块优化到精确最优」时才能保证全局性；对非凸的联合目标（$E$ 关于 $(x,y,k)$ 联合起来并非凸的——它是 $x,y$ 的二次与 $k$ 的线性之乘积型项），交替下降可能停在非最优的驻点。故「因为是凸所以局部即全局」的论证在这里失效，因为该交替过程所优化的对象本身并不凸。

**(b)** 以下代码求解刚度重新分配问题。

```matlab
tens_struct_data;
c = g*m;
% 最优刚度分配问题
A1 = A(1:p,:); A2 = A(p+1:n,:); cbar = c(p+1:n);
```

> **代码解读**：`tens_struct_data` 加载题目给出的全部数据（固定位置、质量、拓扑 $A$、重力加速度 $g$、总刚度 $k_{\mathrm{tot}}$ 等）。`c = g*m` 按 $c_i=g m_i$ 构造重力系数向量。`A1 = A(1:p,:)` 取关联矩阵前 $p$ 行（对应固定质点），`A2 = A(p+1:n,:)` 取后 $n-p$ 行（对应自由质点），`cbar = c(p+1:n)` 取自由质点的重力系数 $\bar c$——与 (a) 中的分块 $A_1,A_2,\bar c$ 完全对应。后续代码（原文此处之后）将继续按 (a) 的显式公式构造 $D,Q,v_x,v_y$ 并求解凸优化，报告 $E_{\min}$ 的最优值并绘制优化后的平衡构型与均匀分配构型的对比图。

---

## （接上页：Problem 3 —— 质量/弹簧系统的 CVX 实现与结果）

### MATLAB/CVX 求解代码

```matlab
cvx_begin
variable k(N)
D = diag(k);
bx = A1'*x_fixed;
by = A1'*y_fixed;
vx = A2*D*bx;
vy = A2*D*by+cbar;
maximize(bx'*D*bx-matrix_frac(vx,A2*D*A2')+...
by'*D*by-matrix_frac(vy,A2*D*A2'))
subject to
k >= 0; sum(k) == k_tot;
cvx_end
```

**译注**：这一段把 Problem 3 的优化问题翻译成 CVX 代码。变量是刚度向量 $k\in\mathbb{R}^{N}$；`D = diag(k)` 用刚度构成对角矩阵 $D=\operatorname{diag}(k)$（每根弹簧的刚度 $k_i$ 放在对角线上）。`bx`、`by` 分别是 $A_1^{\top}x_{\text{fixed}}$、$A_1^{\top}y_{\text{fixed}}$，即只与固定节点相关的部分；`vx`、`vy` 则是加入弹簧后的中间量。目标函数对 $x$、$y$ 两个方向各自取一个二次型减去一个 `matrix_frac`（矩阵分式，即 $v^{\top}Q^{-1}v$ 的形式，是凸函数），再相加，这正是总能量的表达式。约束为 $k\ge 0$（刚度非负）与 $\sum_i k_i = k_{\text{tot}}$（总刚度守恒）。因为 `matrix_frac` 是 CVX 内置的凸函数，整个 `maximize` 目标是凹的，问题可被 CVX 直接求解。

```matlab
% Compute Emin
Eunif = 0.5*x_unif'*A*diag(k_unif)*A'*x_unif;
Eunif = Eunif + 0.5*y_unif'*A*diag(k_unif)*A'*y_unif;
Eunif = Eunif + c'*y_unif
Emin = 0.5*cvx_optval+c(1:p)'*y_fixed
```

**译注**：这里计算两个能量基准。$E_{\text{unif}}$ 是**均匀分配刚度**（uniform stiffness，每根弹簧刚度相同）时的能量：对 $x$、$y$ 两个方向各算一个二次型 $\tfrac12 (\cdot)^{\top}A\,\operatorname{diag}(k_{\text{unif}})\,A^{\top}(\cdot)$，再加一个线性项 $c^{\top}y$。$E_{\min}$ 则是优化后的最小能量，等于 CVX 返回的最优值 `cvx_optval` 的一半，再加上与固定节点坐标 $y_{\text{fixed}}$ 相关的线性项 $c(1{:}p)^{\top}y_{\text{fixed}}$。因子 $\tfrac12$ 是因为目标函数里能量被写成两个二次型之和，而 CVX 的 `cvx_optval` 记录了去掉 $\tfrac12$ 后的目标值。

```matlab
% Form x and y
xmin = -(A2*D*A2')\(A2*D*A1'*x_fixed);
ymin = -(A2*D*A2')\(A2*D*A1'*y_fixed+cbar);
xopt = zeros(n,1);
xopt(1:p) = x_fixed;
xopt(p+1:n) = xmin;
yopt = zeros(n,1);
yopt(1:p) = y_fixed;
yopt(p+1:n) = ymin;
```

**译注**：这段由最优刚度 $D$ 反解出最优节点位置。因为给定刚度后，能量是关于自由节点坐标的二次函数，其最小点满足一阶最优条件（对自由节点求梯度并令其为零），解得线性方程组，自由节点坐标 $x_{\min}=-(A_2 D A_2^{\top})^{-1}(A_2 D A_1^{\top}x_{\text{fixed}})$，$y$ 方向同理但多一个常数项 `cbar`。`\` 是 MATLAB 的左除，即求解该线性方程组。然后把固定节点坐标（前 $p$ 个分量）与自由节点坐标（第 $p+1$ 到 $n$ 个分量）拼回完整的 $x_{\text{opt}}$、$y_{\text{opt}}$。

```matlab
% Plot optimized structure and structure with uniform stiffness
figure
ind_ex = find(k_unif < 1e-2); %do not show springs with k < 1e-2
Aadj = A(:,setdiff(1:N,ind_ex));
Aadj2 = double(Aadj*Aadj'-diag(diag(Aadj*Aadj')) ~= 0);
gplot(Aadj2,[x_unif y_unif],'o-');
hold on
plot(x_fixed,y_fixed,'ro');
xlabel('x'); ylabel('y');
axis([0.1 0.8 -1.5 1])
```

**译注**：这段画出均匀刚度分配下的结构图。先把刚度小于 $10^{-2}$ 的弹簧剔除（`ind_ex` 记录这些"可忽略"弹簧的编号），只保留其余弹簧对应的列构成 `Aadj`。`Aadj2` 用 `Aadj*Aadj'-diag(diag(Aadj*Aadj'))` 消去对角线自环后取非零判定，得到邻接矩阵（adjacency matrix，描述节点间是否有弹簧相连）。`gplot` 按邻接矩阵画图，固定节点用红色圆点 `'ro'` 标出。

---

## （第 17 页）

```matlab
figure
ind_ex = find(k < 1e-2); %do not show springs with k < 1e-2
Aadj = A(:,setdiff(1:N,ind_ex));
Aadj2 = double(Aadj*Aadj'-diag(diag(Aadj*Aadj')) ~= 0);
gplot(Aadj2,[xopt yopt],'o-');
hold on
plot(x_fixed,y_fixed,'ro');
xlabel('x'); ylabel('y');
axis([0.1 0.8 -1.5 1])
hold off
```

**译注**：与上一段完全对称，只是把均匀刚度换成**优化后的刚度** $k$，坐标换成 $x_{\text{opt}}$、$y_{\text{opt}}$，画出优化后的结构。两次绘图共用一个坐标窗口范围 `axis([0.1 0.8 -1.5 1])`，便于对比。

**结论**：最优能量为 $E_{\min}(k^{\star})=57.84$；相比之下，均匀分配刚度时的最小能量是 $18.37$。均匀与最优两种刚度分配下的质量构型见图 4。刚度小于 $10^{-2}$ 的弹簧不再画出。

> **译注**：注意这里的数值语义 —— 原文写 "the minimum energy is 18.37 when stiffness is uniformly allocated"，而 $E_{\min}(k^\star)=57.84$。也就是说，在本题的设定（质量配置是待优化对象、能量目标方向）下，均匀分配反而给出更小的能量值；题目的本意是展示优化刚度分配得到的构型与均匀分配构型的差异，图 4 直观呈现了这一点。

---

## （第 18 页 —— 图 4）

> 本页为图 4，含上下两张图（top / bottom），标题原文：

**Figure 4**: Mass configuration for uniform (top) and optimal (bottom) stiffness allocation.

**图 4**：均匀（上）与最优（下）刚度分配下的质量构型。

**译注**：两张图的横轴为 $x$（范围约 $0.1$–$0.8$），纵轴为 $y$（范围约 $-1.5$–$1$）。上图对应均匀刚度、下图对应最优刚度，红色圆点为固定节点。可以看出最优刚度分配改变了弹簧的疏密布局，从而改变质量构型。

---

## Problem 4 — Identifying a sparse linear dynamical system（识别一个稀疏线性动力系统）

**识别一个稀疏线性动力系统。**

A linear dynamical system has the form

```math
x(t+1) = Ax(t) + Bu(t) + w(t), \qquad t = 1, \dots, T-1,
```

where $x(t)\in\mathbb{R}^{n}$ is the state, $u(t)\in\mathbb{R}^{m}$ is the input signal, and $w(t)\in\mathbb{R}^{n}$ is the process noise, at time $t$. We assume the process noises are IID $\mathcal{N}(0,W)$, where $W\succ 0$ is the covariance matrix. The matrix $A\in\mathbb{R}^{n\times n}$ is called the dynamics matrix or the state transition matrix, and the matrix $B\in\mathbb{R}^{n\times m}$ is called the input matrix.

一个线性动力系统（linear dynamical system）形如上述等式，其中 $x(t)\in\mathbb{R}^{n}$ 是时刻 $t$ 的状态（state），$u(t)\in\mathbb{R}^{m}$ 是输入信号（input signal），$w(t)\in\mathbb{R}^{n}$ 是过程噪声（process noise）。我们假设各时刻的过程噪声独立同分布（IID）且服从 $\mathcal{N}(0,W)$，其中 $W\succ 0$ 是协方差矩阵（covariance matrix，即正定的 $W$）。矩阵 $A\in\mathbb{R}^{n\times n}$ 称为动力学矩阵（dynamics matrix）或状态转移矩阵（state transition matrix），矩阵 $B\in\mathbb{R}^{n\times m}$ 称为输入矩阵（input matrix）。

You are given accurate measurements of the state and input signal, i.e., $x(1), \dots, x(T)$, $u(1), \dots, u(T-1)$, and $W$ is known. Your job is to find a state transition matrix $\hat{A}$ and input matrix $\hat{B}$ from these data, that are plausible, and in addition are sparse, i.e., have many zero entries. (The sparser the better.)

已知状态与输入信号的精确测量值 $x(1),\dots,x(T)$ 与 $u(1),\dots,u(T-1)$，且 $W$ 已知。你的任务是利用这些数据求出一个状态转移矩阵 $\hat{A}$ 与输入矩阵 $\hat{B}$，使得它们**合理（plausible）**，且**稀疏（sparse）**，即含有大量零元素（越稀疏越好）。

By doing this, you are effectively estimating the structure of the dynamical system, i.e., you are determining which components of $x(t)$ and $u(t)$ affect which components of $x(t+1)$. In some applications, this structure might be more interesting than the actual values of the (nonzero) coefficients in $\hat{A}$ and $\hat{B}$.

这样做实际上是在估计该动力系统的**结构**，即确定 $x(t)$ 与 $u(t)$ 的哪些分量会影响 $x(t+1)$ 的哪些分量。在某些应用中，这种结构可能比 $\hat{A}$、$\hat{B}$ 中（非零）系数的具体数值更有意义。

By plausible, we mean that

```math
\sum_{t=1}^{T-1}\Bigl\| W^{-1/2}\bigl(x(t+1) - \hat{A}x(t) - \hat{B}u(t)\bigr)\Bigr\|_{2}^{2} \in n(T-1) \pm 2\sqrt{2n(T-1)},
```

where $a \pm b$ means the interval $[a-b, a+b]$. (You can just take this as our definition of plausible. But to explain this choice, we note that when $\hat{A} = A$ and $\hat{B} = B$, the left-hand side is $\chi^{2}$, with $n(T-1)$ degrees of freedom, and so has mean $n(T-1)$ and standard deviation $\sqrt{2n(T-1)}$.)

所谓"合理"，是指上面的加权残差平方和落在区间 $n(T-1)\pm 2\sqrt{2n(T-1)}$ 内，其中 $a\pm b$ 表示区间 $[a-b,a+b]$。（你可以把这一条直接当作"合理"的定义。为解释这个选择，注意当 $\hat{A}=A$、$\hat{B}=B$ 时，左端是自由度为 $n(T-1)$ 的 $\chi^{2}$（卡方）分布，故其均值为 $n(T-1)$、标准差为 $\sqrt{2n(T-1)}$。）

> **译注**：这里 $W^{-1/2}$ 是 $W$ 的逆的对称平方根（因 $W\succ0$ 故 $W^{-1/2}$ 存在且对称正定），用来把噪声白化，使加权残差 $W^{-1/2}(x(t+1)-\hat{A}x(t)-\hat{B}u(t))$ 在真实模型下各分量独立标准正态，从而平方和服从 $\chi^{2}$。合理性区间正是"均值 $\pm 2$ 倍标准差"，对应约 $95\%$ 的置信带。

### (a)

Describe a method for finding $\hat{A}$ and $\hat{B}$, based on convex optimization. We are looking for a very simple method, that involves solving one convex optimization problem. (There are many extensions of this basic method, that would improve the simple method, i.e., yield sparser $\hat{A}$ and $\hat{B}$ that are still plausible. We're not asking you to describe or implement any of these.)

**（a）** 描述一种基于凸优化（convex optimization）求 $\hat{A}$、$\hat{B}$ 的方法。我们要的是非常简单的、只需解**一个**凸优化问题的方法。（这个基本方法有许多扩展，可改进它，得到更稀疏且仍合理的 $\hat{A}$、$\hat{B}$。我们不要求你描述或实现这些扩展。）

### (b)

Carry out your method on the data found in `sparse_lds_data.m`. Give the values of $\hat{A}$ and $\hat{B}$ that you find, and verify that they are plausible.

**（b）** 对 `sparse_lds_data.m` 中的数据执行你的方法，给出求得的 $\hat{A}$、$\hat{B}$，并验证它们是合理的。

In the data file, we give you the true values of $A$ and $B$, so you can evaluate the performance of your method. (Needless to say, you are not allowed to use these values when forming $\hat{A}$ and $\hat{B}$.) Using these true values, give the number of false positives and false negatives in both $\hat{A}$ and $\hat{B}$. A false positive in $\hat{A}$, for example, is an entry that is nonzero, while the corresponding entry in $A$ is zero. A false negative is an entry of $\hat{A}$ that is zero, while the corresponding entry of $A$ is nonzero. To judge whether an entry of $\hat{A}$ (or $\hat{B}$) is nonzero, you can use the test $|\hat{A}_{ij}|\ge 0.01$ (or $|\hat{B}_{ij}|\ge 0.01$).

数据文件中给出了 $A$、$B$ 的真值，你可以据此评估方法的性能。（不用说，构造 $\hat{A}$、$\hat{B}$ 时不允许使用这些真值。）利用真值，分别统计 $\hat{A}$、$\hat{B}$ 中的**假阳性（false positive）**与**假阴性（false negative）**个数。例如 $\hat{A}$ 的假阳性是指 $\hat{A}$ 中某元素非零、而 $A$ 对应位置为零；假阴性是指 $\hat{A}$ 中某元素为零、而 $A$ 对应位置非零。判断 $\hat{A}$（或 $\hat{B}$）某元素是否非零，可用检验 $|\hat{A}_{ij}|\ge 0.01$（或 $|\hat{B}_{ij}|\ge 0.01$）。

---

### Solution（解答）

The problem can be expressed as

```math
\begin{aligned}
&\text{minimize} \quad \operatorname{card}(\hat{A}) + \operatorname{card}(\hat{B}) \\
&\text{subject to} \quad \sum_{t=1}^{T-1}\Bigl\| W^{-1/2}\bigl(x(t+1)-\hat{A}x(t)-\hat{B}u(t)\bigr)\Bigr\|_{2}^{2}
\in n(T-1)\pm 2\sqrt{2n(T-1)},
\end{aligned}
```

where $\operatorname{card}(X)$ is the cardinality (the number of nonzero entries) of matrix $X$.

该问题可写为上述形式，其中 $\operatorname{card}(X)$ 是矩阵 $X$ 的**基数（cardinality）**，即其非零元素的个数。

However, there are two problems with this: the objective is non-convex, and the lower bound

```math
\sum_{t=1}^{T-1}\Bigl\| W^{-1/2}\bigl(x(t+1)-\hat{A}x(t)-\hat{B}u(t)\bigr)\Bigr\|_{2}^{2} \ge n(T-1)-2\sqrt{2n(T-1)}
```

is not a convex constraint.

然而这样做有两个问题：其一是**目标函数非凸**（cardinality 计数函数 $\operatorname{card}$ 非凸，因为它对非零元素一律计 1，产生大量平台与不连续，不满足凸函数定义）；其二是**下界约束不是凸约束**（上式左侧是凸函数，但要求它 $\ge$ 某个值，即在一个凸函数的**下方**区域之外，这种集合一般非凸）。

The second problem is dealt with by noting that we can always increase the magnitudes of the implied errors by (for example) multiplying candidate $\hat{A}$ and $\hat{B}$ by a constant. Hence the lower bound can be neglected, without loss of generality.

第二个问题（下界非凸）可这样处理：我们总可以通过（例如）把候选的 $\hat{A}$、$\hat{B}$ 同时乘以一个常数来**增大**隐含误差的幅度（即残差平方和可以任意调大）。因此，下界约束在**不失一般性（without loss of generality）**的前提下可以被忽略——因为只要残差不太大，总能把它调大到超过下界，故下界约束永远是松的，无需显式写出。

Unfortunately the card function is less comprehensively dealt with. However, we can use the (standard) heuristic of instead minimizing the $\ell_1$ norm of the entries of $\hat{A}$ and $\hat{B}$. This gives the convex optimization problem

```math
\begin{aligned}
&\text{minimize} \quad \|\operatorname{vec}(\hat{A})\|_{1} + \|\operatorname{vec}(\hat{B})\|_{1} \\
&\text{subject to} \quad \sum_{t=1}^{T-1}\Bigl\| W^{-1/2}\bigl(x(t+1)-\hat{A}x(t)-\hat{B}u(t)\bigr)\Bigr\|_{2}^{2}
\le n(T-1)+2\sqrt{2n(T-1)},
\end{aligned}
```

where $\operatorname{vec}(X)$ represents the columns of $X$ concatenated to make a single column vector.

遗憾的是，$\operatorname{card}$ 函数没有那么好处理。不过我们可以用一个（标准的）**启发式（heuristic）**：改为最小化 $\hat{A}$、$\hat{B}$ 各元素的 $\ell_1$ 范数（$\ell_1$ norm，即所有元素绝对值之和）。这样就得到上述凸优化问题，其中 $\operatorname{vec}(X)$ 表示把 $X$ 的各列依次首尾相接拼成一个列向量。

> **译注**：这里的关键替换是**用 $\ell_1$ 范数替代 $\operatorname{card}$ 作为稀疏性的凸松弛**。$\ell_1$ 范数是凸函数（绝对值和的凸性），且其最小化会促使许多元素被压到恰好为 0（$\ell_1$ 在 0 处有尖角，产生稀疏解），这与稀疏回归 / compressive sensing 中的经典思想一致。同时把下界约束换成**上界约束**（残差平方和 $\le n(T-1)+2\sqrt{2n(T-1)}$），因为"残差 $\le$ 某值"是凸约束（凸函数的 sublevel set 是凸的）。

Roughly speaking, note that the constraint will always be tight: relaxing the requirement on the implied errors allows more freedom to reduce the $\ell_1$ norm of $\hat{A}$ and $\hat{B}$. Thus, in reality, we never need to worry about the lower bound from above as it will be always satisfied.

粗略地说，注意这个约束总是会**取等号（tight）**：放宽对隐含误差的要求（允许残差更大）会给我们更多自由度去减小 $\hat{A}$、$\hat{B}$ 的 $\ell_1$ 范数。因此实际上我们永远不用担心上界里的下界部分——它总是被满足。

> **译注**：这一句的逻辑是：最小化 $\ell_1$ 范数会倾向于"用尽"允许的残差预算（把残差顶到上界），因为残差越松、可压缩空间越大；于是最优解处的残差会自动落在上界附近，自然大于下界，故下界无需单独约束。

This problem is easily solved in CVX using the following code

```matlab
% Load problem data.
sparse_lds_data;
fit_tol = sqrt(n*(T-1) + 2*sqrt(2*n*(T-1))); % fit tolerance.
cvx_begin
variables Ahat(n,n) Bhat(n,m);
minimize(sum(norms(Ahat, 1)) + sum(norms(Bhat, 1)))
norm(inv(Whalf)*(xs(:,2:T) - Ahat*xs(:,1:T-1) ...
- Bhat*us), 'fro') <= fit_tol;
cvx_end
disp(cvx_status)
% Check lower bound.
fit_tol_m = sqrt(n*(T-1) - 2*sqrt(2*n*(T-1)));
disp(norm(inv(Whalf)*(xs(:,2:T) ...
- Ahat*xs(:,1:T-1) - Bhat*us), 'fro') >= fit_tol_m);
```

这个问题用如下 CVX 代码即可轻松求解。

**译注**：代码解读——

- `fit_tol` 是上界 $n(T-1)+2\sqrt{2n(T-1)}$ 的**平方根**，因为约束里用的是 Frobenius 范数（$\|\cdot\|_{\text{fro}}$），而 Frobenius 范数的平方正是各元素平方和，与原文的 $\sum_t\|\cdot\|_2^2$ 一致（Frobenius 范数平方 = 所有元素平方和 = 各时刻 $\|\cdot\|_2^2$ 之和）。
- `sum(norms(Ahat,1))` 把 $\hat{A}$ 的每一列取 $\ell_1$ 范数再求和，等价于所有元素绝对值之和，即 $\|\operatorname{vec}(\hat{A})\|_1$。
- `inv(Whalf)` 对应 $W^{-1/2}$（数据里已给出白化矩阵 `Whalf`）。
- 约束 `norm(...) <= fit_tol` 正是残差的 Frobenius 范数 $\le$ 上界。
- 后面 `fit_tol_m` 检查下界，验证 $n(T-1)-2\sqrt{2n(T-1)}$ 确实被满足（应为 `1`，即真）。

---

## （第 21 页）

```matlab
% Round near-zero elements to zero.
Ahat = Ahat .* (abs(Ahat) >= 0.01)
Bhat = Bhat .* (abs(Bhat) >= 0.01)
% Display results.
disp(['false positives, Ahat: ' num2str(nnz((Ahat ~= 0) & (A == 0)))])
disp(['false negatives, Ahat: ' num2str(nnz((Ahat == 0) & (A ~= 0)))])
disp(['false positives, Bhat: ' num2str(nnz((Bhat ~= 0) & (B == 0)))])
disp(['false negatives, Bhat: ' num2str(nnz((Bhat == 0) & (B ~= 0)))])
```

**译注**：第一、二行把接近零的元素**四舍五入置零**（`abs(Ahat) >= 0.01` 是逻辑掩码，与 `Ahat` 逐元素相乘，凡绝对值小于 $0.01$ 的元素归零），实现"判断某元素是否非零"的 $0.01$ 阈值。后四行分别用 `nnz`（nonzero 计数）统计假阳性/假阴性：`(Ahat ~= 0) & (A == 0)` 即"估计非零而真值为零"的假阳性，`(Ahat == 0) & (A ~= 0)` 即"估计为零而真值非零"的假阴性，对 $\hat{A}$、$\hat{B}$ 各统计一次。

With the given problem data, we get 1 false positive and 2 false negatives for $\hat{A}$, and no false positives and 1 false negative for $\hat{B}$.

对于给定的问题数据，$\hat{A}$ 得到 1 个假阳性与 2 个假阴性，$\hat{B}$ 无假阳性、1 个假阴性。

The matrix estimates are

```math
\hat{A} = \begin{bmatrix}
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 1.1483 & -0.0899 & 0.1375 & -0.0108 & 0 & 0 & 0 & 0 \\
0 & 0.9329 & 0 & 0 & 0.2868 & 0 & 0 & 0.2055 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & -0.0190 & 0.9461 & 0 & 0.8697 \\
0 & 0 & 0 & 0 & 0.2066 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0
\end{bmatrix},
```

and

```math
\hat{B} = \begin{bmatrix}
-1.4717 & 0 & 0 & 0 \\
0 & 0 & 0 & -0.2832 \\
0 & 0 & 0 & 0 \\
0 & 0 & 0 & 1.6363 \\
-0.0456 & 0 & 0 & 0 \\
0 & 1.4117 & 0 & 0 \\
-0.0936 & 0 & 0 & -0.7755 \\
0 & -0.5705 & 0 & 0 \\
0 & 0 & 0 & 0
\end{bmatrix}.
```

估计得到的矩阵如上。

> **译注**：$\hat{A}$ 是 $9\times9$ 矩阵（$n=9$），$\hat{B}$ 是 $9\times4$ 矩阵（$m=4$）。可见二者都相当稀疏，大量元素为零，印证了 $\ell_1$ 松弛的稀疏化效果。

Finally, there are lots of methods that will do better than this, usually by taking this as a starting point and ‘polishing’ the result after that. Several of these have been shown to give fairly reliable, if modest, improvements. You were not required to implement any of these methods.

最后，有很多方法能比这做得更好，通常是以此结果为起点再做"打磨（polishing）"。其中若干方法已被证明能带来虽不大但相当可靠的改进。你无需实现其中任何一种方法。

---

## Problem 5（第 5 题，第 22–25 页）

**Minimum energy processor speed scheduling（最小能耗处理器速度调度）。** 一台单处理器（single processor）可以在 $T$ 个时间段（time period）中的每一个调整自己的速度，这些时间段记为 $1,\dots,T$。它在时间段 $t$ 的速度记为 $s_t$，$t=1,\dots,T$。速度必须介于给定的（正的）最小值与最大值 $S_{\min}$ 和 $S_{\max}$ 之间，并且必须满足一个**压摆率限制（slew-rate limit）** $|s_{t+1}-s_t|\le R$，$t=1,\dots,T-1$。（也就是说，$R$ 是相邻时间段之间速度允许的最大变化量。）处理器在时间段 $t$ 消耗的能量由 $\varphi(s_t)$ 给出，其中 $\varphi:\mathbb{R}\to\mathbb{R}$ 是**递增的凸函数（increasing and convex）**。所有时间段消耗的总能量为

```math
E=\sum_{t=1}^{T}\varphi(s_t).
```

处理器必须处理 $n$ 个作业（job），记为 $1,\dots,n$。每个作业有一个**可用时间（availability time）** $A_i\in\{1,\dots,T\}$ 和一个**截止时间（deadline）** $D_i\in\{1,\dots,T\}$，且 $D_i\ge A_i$。处理器在时间段 $t=A_i$ 之前不能开始作业 $i$ 的工作，并且必须在时间段 $D_i$ 结束之前完成该作业。作业 $i$ 涉及一个（非负的）总工作量 $W_i$。你可以假设在每个时间段都至少有一个作业可用，也就是说，对每个 $t$，都存在至少一个 $i$ 使得 $A_i\le t$ 且 $D_i\ge t$。

在时间段 $t$，处理器把它的努力（effort）在 $n$ 个作业之间分配，记为 $\theta_t$，其中 $\mathbf{1}^{T}\theta_t=1$，$\theta_t\succeq 0$。这里 $\theta_{ti}$（$\theta_t$ 的第 $i$ 个分量）给出时间段 $t$ 内处理器努力中用于作业 $i$ 的**比例（fraction）**。

> **术语 · slew-rate limit（压摆率限制）**：对相邻时间段速度变化量 $|s_{t+1}-s_t|$ 的绝对上界 $R$，用于限制处理器速度不能突变过快。注意它是对绝对值的一对仿射不等式（$s_{t+1}-s_t\le R$ 且 $s_t-s_{t+1}\le R$），因此是**凸约束**。

遵守可用性与截止时间约束要求：当 $t<A_i$ 或 $t>D_i$ 时 $\theta_{ti}=0$。要完成这些作业，我们必须有

```math
\sum_{t=A_i}^{D_i}\theta_{ti}s_t\ge W_i,\qquad i=1,\dots,n.
```

**(a)** 把「选择速度 $s_1,\dots,s_T$ 与分配 $\theta_1,\dots,\theta_T$ 以最小化总能量 $E$」的问题，表述（formulate）为一个凸优化问题（convex optimization problem）。问题数据是 $S_{\min}$、$S_{\max}$、$R$、$\varphi$，以及作业数据 $A_i,D_i,W_i$，$i=1,\dots,n$。务必对你表述中所做的任何变量替换（change of variables）或引入的新变量给出理由（justify）。

**(b)** 在 `proc_sched_data.m` 中描述的问题实例上执行你的方法，其中能量函数为二次函数 $\varphi(s_t)=\alpha+\beta s_t+\gamma s_t^{2}$。（参数 $\alpha$、$\beta$、$\gamma$ 在数据文件中给出。）执行该文件还会画出一张图，显示各作业的可用时间与截止时间。给出你的速度配置（speed profile）与分配所得到的能量。用命令 `bar((s*ones(1,n)).*theta,1,'stacked')` 画出这些，其中 `s` 是 $T\times 1$ 的速度向量，`θ` 是分量为 $\theta_{ti}$ 的 $T\times n$ 分配矩阵。这将显示在每个时间段内有多少有效速度被分配到每个作业。图的上沿将显示速度 $s_t$。（你不需要交彩图版本；黑白即可。）

### Solution（解答）

窍门是使用变量 $x_{ti}=\theta_{ti}s_t$，它必须非负。令 $X\in\mathbb{R}^{T\times n}$ 表示分量为 $x_{ti}$ 的矩阵。作业完成约束可以表达为 $X^{T}\mathbf{1}\succeq W$。可用性与截止时间约束可以表达为：当 $t<A_i$ 或 $t>D_i$ 时 $x_{ti}=0$，这些都是线性约束（linear constraint）。

> **为什么引入新变量 $x_{ti}=\theta_{ti}s_t$（变量替换的理由）**：原问题里，作业完成约束 $\sum_t\theta_{ti}s_t\ge W_i$ 同时包含两个变量的乘积 $\theta_{ti}\cdot s_t$，是**双线性（bilinear）项**，即关于 $(\theta,s)$ 联合起来不是凸（甚至不是仿射）的。把乘积整体当作一个新变量 $x_{ti}$，就能把这条约束变成关于 $x$ 的**线性**约束 $\sum_t x_{ti}\ge W_i$。$x_{ti}=\theta_{ti}s_t$ 的物理含义是「时间段 $t$ 内处理器投给作业 $i$ 的**有效速度（effective speed）**」——因为 $\theta_{ti}$ 是时间比例、$s_t$ 是速度，二者之积正是这段时间实际分给该作业的速度量。同时，约束 $\theta_{ti}\ge 0$ 与 $s_t>0$（速度有正下界 $S_{\min}>0$）保证 $x_{ti}\ge 0$ 自动成立；而「$t<A_i$ 或 $t>D_i$ 时 $\theta_{ti}=0$」也直接转化为「$t<A_i$ 或 $t>D_i$ 时 $x_{ti}=0$」，仍是线性等式约束。于是整个问题变成关于 $x$（以及可由 $x$ 导出的 $s$）的凸问题。

速度可以表达为 $s=X\mathbf{1}$，这是变量 $X$ 的一个线性函数。

> **为什么 $s=X\mathbf{1}$**：对每个时间段 $t$，分配向量 $\theta_t$ 满足 $\mathbf{1}^{T}\theta_t=\sum_i\theta_{ti}=1$（即处理器在每段把 100% 的努力分出去）。因此 $\sum_i x_{ti}=\sum_i\theta_{ti}s_t=s_t\sum_i\theta_{ti}=s_t\cdot 1=s_t$。写成矩阵形式，$X$ 的每一行是 $t$ 时刻对 $n$ 个作业的 $x_{ti}$，右乘全一向量 $\mathbf{1}\in\mathbb{R}^{n}$ 就得到逐行求和，即 $s=(s_1,\dots,s_T)^{T}=X\mathbf{1}$。这说明 $s$ 由 $X$ 唯一确定，不必再单独保留 $s$ 作为独立变量（虽然在下面的表述里仍把 $s$ 显式列出来，作为 $X$ 的线性函数）。

目标 $E$ 显然是 $s$ 的凸函数（因此也是 $X$ 的凸函数）。

> **为什么 $E$ 是凸函数**：$\varphi$ 是凸函数，$E=\sum_t\varphi(s_t)$ 是各分量 $\varphi(s_t)$ 之和。而 $s_t$ 是 $X$ 的第 $t$ 行之和，是 $X$ 的仿射函数。凸函数与仿射函数复合仍是凸函数（composition rule：$\varphi(s_t(X))$ 是「凸外函数 $\varphi$ 配仿射内函数 $s_t(X)$」，故凸），凸函数之和仍是凸函数，所以 $E$ 关于 $X$ 是凸的。这是目标可被凸优化的关键。

我们的凸优化问题是

```math
\begin{aligned}
&\text{minimize} && E=\sum_{t=1}^{T}\varphi(s_t)\\
&\text{subject to} && S_{\min}\preceq s\preceq S_{\max},\\
&&& s=X\mathbf{1},\\
&&& X^{T}\mathbf{1}\succeq W,\\
&&& X\succeq 0,\\
&&& |s_{t+1}-s_t|\le R, && t=1,\dots,T-1,\\
&&& X_{ti}=0, && t=1,\dots,A_i-1,\ i=1,\dots,n,\\
&&& X_{ti}=0, && t=D_i+1,\dots,T,\ i=1,\dots,n,
\end{aligned}
```

其中变量为 $s\in\mathbb{R}^{T}$ 与 $X\in\mathbb{R}^{T\times n}$。这里所有的广义不等式（generalized inequality），包括 $X\succeq 0$，都是**逐元素非负（elementwise nonnegativity）**。显然这是一个凸问题。

> **为什么这整个问题是凸的**：逐条检查——目标 $E=\sum_t\varphi(s_t)$ 是凸的（上面已证）；$S_{\min}\preceq s\preceq S_{\max}$ 是逐元素仿射不等式（可行集是盒子 box，凸）；$s=X\mathbf{1}$ 是仿射等式；$X^{T}\mathbf{1}\succeq W$ 是仿射不等式（$X^{T}\mathbf{1}$ 的第 $i$ 个分量是 $\sum_t x_{ti}$）；$X\succeq 0$ 是仿射不等式；$|s_{t+1}-s_t|\le R$ 等价于 $s_{t+1}-s_t\le R$ 与 $s_t-s_{t+1}\le R$ 两条仿射不等式；$X_{ti}=0$ 是仿射等式。所有约束都是仿射等式或仿射不等式，故可行集是凸集（一个多面体）；目标凸。因此这是一个标准的凸优化问题，可以用 CVX 直接求解。

> **术语 · convex optimization problem（凸优化问题）**：形如「最小化一个凸函数、且约束由一个凸集（典型地由 $f_i(x)\le 0$ 其中 $f_i$ 凸、$h_j(x)=0$ 其中 $h_j$ 仿射）描述」的优化问题。可判定的定义：目标函数是凸的，等式约束函数是仿射的，不等式约束函数是凸的（此处全部退化为仿射，即一个 LP 型的多面体约束）。

一旦找到最优解 $X^{\star}$ 与 $s^{\star}$，我们就可以恢复出 $\theta^{\star}_{t}$ 为

```math
\theta^{\star}_{ti}=\frac{1}{s^{\star}_{t}}\,x^{\star}_{ti},\qquad t=1,\dots,T,\ i=1,\dots,n.
```

> **为什么这样恢复 $\theta^{\star}$**：由定义 $x_{ti}=\theta_{ti}s_t$，两边除以 $s_t$ 即得 $\theta_{ti}=x_{ti}/s_t$。因为速度有正下界 $S_{\min}>0$，故 $s^{\star}_t>0$，除法有意义。又因 $\sum_i\theta_{ti}=\frac{1}{s_t}\sum_i x_{ti}=\frac{1}{s_t}\cdot s_t=1$，恢复出的 $\theta_t$ 自动满足 $\mathbf{1}^{T}\theta_t=1$；且 $x_{ti}\ge 0$、$s_t>0$ 保证 $\theta_{ti}\ge 0$，即 $\theta_t\succeq 0$。所以恢复出的分配合法。

对于我们的数据，各作业的可用时间与截止时间画在图 5（figure 5）中。作业持续线显示了一个「从可用时间段的**开始处**开始」的起始时刻，以及一个「在截止时间段**最末端**」的终止时刻。因此，这条线的长度显示的是作业被允许完成的实际持续时间（duration）。

解决问题的代码如下。

```matlab
clear all;
close all;
proc_sched_data;
cvx_begin
variable X(T,n)
X >= 0;
s = sum(X')';
minimize(sum(alpha+beta*s+gamma*square(s)))
s >= Smin;
s <= Smax;
abs(s(2:end)-s(1:end-1))<=R; % slew rate constraint
% start/stop constraints
for i=1:n
for t=1:A(i)-1
X(t,i)==0;
end
for t=D(i)+1:T
X(t,i)==0;
end
end
sum(X)>=W';
cvx_end
theta = X./(s*ones(1,n));
figure;
bar((s*ones(1,n)).*theta,1,'stacked');
xlabel('t');
ylabel('s_t');
```

> **注记 · 代码与公式表述的对应**：`s = sum(X')'` 计算 $s=X\mathbf{1}$（`X'` 转置后 `sum` 沿列求和，即对每个 $t$ 把第 $t$ 行的 $n$ 个 $x_{ti}$ 相加，再转置回 $T\times 1$ 列向量）；`minimize(sum(alpha+beta*s+gamma*square(s)))` 即目标 $\sum_t(\alpha+\beta s_t+\gamma s_t^{2})$；`s>=Smin`、`s<=Smax` 对应 $S_{\min}\preceq s\preceq S_{\max}$；`abs(s(2:end)-s(1:end-1))<=R` 对应压摆率约束 $|s_{t+1}-s_t|\le R$；两个 `for` 循环对应「$t<A_i$ 或 $t>D_i$ 时 $X_{ti}=0$」；`sum(X)>=W'` 对应 $X^{T}\mathbf{1}\succeq W$（`sum(X)` 沿列求和得到行向量 $(\sum_t x_{t1},\dots,\sum_t x_{tn})$，与 $W'$（$W$ 的转置，即行向量）逐元素比较）。恢复 $\theta$ 的 `theta = X./(s*ones(1,n))` 正是 $\theta_{ti}=x_{ti}/s_t$（`s*ones(1,n)` 把 $s$ 复制成 $T\times n$ 矩阵，再逐元素相除）。

> **注记 · `minimize(sum(alpha+beta*s+gamma*square(s)))` 的凸性**：`square(s)` 是逐元素平方，$\gamma s_t^{2}$ 是凸函数（$\gamma>0$ 时），$\beta s_t$ 是仿射的，$\alpha$ 是常数；凸函数与仿射函数之和仍是凸函数，故 $\varphi(s_t)=\alpha+\beta s_t+\gamma s_t^{2}$ 是凸函数（这正是题干给定「$\varphi$ 是凸的」在二次函数下的体现，等价于 $\gamma\ge 0$；数据中 $\gamma>0$）。因此 CVX 能识别目标为凸。

最优总能量为 $E=162.125$，它由按照图 6（figure 6）所示图的分配速度获得。

> **注记 · 数值结果**：最优目标值 $E^{\star}=162.125$ 就是最小总能耗。

**Figure 5: Job availability diagram（作业可用性图）。** 纵轴为作业 $i$，横轴为时间 $t$（范围 $0$ 到 $18$）。**星号（stars）** 标示每个作业变为可用的时刻；**空心圆（open circles）** 标示作业要求的结束时刻，它位于截止时间段的末端。

**Figure 6: Optimal speed allocation profile（最优速度分配配置图）。** 纵轴为速度 $s_t$（范围 $0$ 到 $3.5$），横轴为时间 $t$（范围 $0$ 到 $18$）。图的上沿给出处理器速度 $s_t$；着色区域（colored regions）标示每个时刻分配给特定作业的那部分速度（即「堆叠柱状图」，各作业的有效速度 $x_{ti}=\theta_{ti}s_t$ 自下而上堆叠，总高度恰为 $s_t$）。

---

## Problem 6（第 6 题，第 26 页起）

**Planning production with uncertain demand（需求不确定下的生产规划）。** 你必须订购（非负的）数量 $r_1,\dots,r_m$ 的原材料（raw material），这些原材料用于制造 $n$ 种不同产品的（非负的）数量 $q_1,\dots,q_n$。制造一单位产品 $j$ 至少需要 $A_{ij}$ 单位的原材料 $i$，所以我们必须有 $r\succeq Aq$。（我们假设 $A_{ij}$ 是非负的。）原材料的单位成本由 $c\in\mathbb{R}^{m}_{+}$ 给出，所以原材料总成本是 $c^{T}r$。

产品 $j$ 的（非负）需求（demand）记为 $d_j$；产品 $j$ 售出的数量为 $s_j=\min\{q_j,d_j\}$。（当 $q_j>d_j$ 时，$q_j-d_j$ 是「生产了但没有售出」的产品 $j$ 的数量；当 $d_j>q_j$ 时，$d_j-q_j$ 是未被满足的需求的数量。）销售产品获得的收入（revenue）是 $p^{T}s$，其中 $p\in\mathbb{R}^{n}_{+}$ 是产品价格向量。**利润（profit）** 是 $p^{T}s-c^{T}r$。（$d$ 与 $q$ 都是实向量；它们的分量不必是整数。）

给定 $A$、$c$ 与 $p$。然而产品需求是未知的。相反，给定了 $K$ 个可能的需求向量 $d^{(1)},\dots,d^{(K)}$，以及相应的概率（probability） $\pi_1,\dots,\pi_K$。（它们满足 $\mathbf{1}^{T}\pi=1$，$\pi\succeq 0$。）

你将探讨在「选择 $r$ 与 $q$（变量）」时产生的两个不同的优化问题。

**I. 提前选择 $r$ 与 $q$。** 你必须只根据上面列出的数据来选择 $r$ 与 $q$。（换句话说，你必须在知道产品需求之前订购原材料，并承诺生产所选数量的产品。）目标是最大化**期望利润（expected profit）**。

**II. 提前选择 $r$，在知道 $d$ 之后再选择 $q$。** 你必须只根据上面列出的数据来选择 $r$。在你选择 $r$ 之后的某个时刻，需求会变为已知。这意味着你会知道 $K$ 个需求向量中哪一个才是真实需求。一旦你知道了这一点，你就必须选择要制造的数量。（换句话说，你必须在产品需求已知之前订购原材料；但在得知真实产品需求之后，你可以选择要制造的产品的组合。）目标是最大化期望利润。

**(a)** 解释如何把这两个问题各自表述为一个凸优化问题。清楚地说明问题中的变量是什么、约束是什么，并描述你引入的任何辅助变量（auxiliary variable）或约束所起的作用。

**(b)** 在 `planning_data.m` 中给出的带有数值数据的问题实例上执行 (a) 中的方法。该文件将定义 `A`、`D`、`K`、`c`、`m`、`n`、`p` 与 `pi`。`D` 的 $K$ 列是可能的需求向量。对于上面描述的两个问题，给出 $r$ 的最优值，以及期望利润。

### Solution（解答）

我们首先考虑「$r$ 与 $q$ 必须在需求已知之前决定」的情形。

变量是 $r\in\mathbb{R}^{m}$ 与 $q\in\mathbb{R}^{n}$。

资源的成本是

〔原书缺页：第 26 页末的内容在「资源的成本是（The cost of the resources is）」处截断，Problem 6 的完整解答（成本表达式、目标函数、约束的完整表述，以及情形 I 与情形 II 的凸优化形式、数值结果）位于第 27 页及之后，属下一分段。〕

---

> **术语 · expected profit（期望利润）**：对随机需求下利润的数学期望 $\mathbb{E}[\text{profit}]=\sum_{k=1}^{K}\pi_k\cdot(\text{第 }k\text{ 个需求情形下的利润})$。因为需求只能取 $K$ 个离散值 $d^{(k)}$ 且概率为 $\pi_k$，期望利润就是这 $K$ 个情形的利润按概率加权求和。
>
> **术语 · convex optimization problem（凸优化问题）**：见 Problem 5 解答中的术语定义。此处 Problem 6 的两情形最终都要整理成「最大化一个关于 $(r,q)$ 或 $(r,\{q^{(k)}\})$ 的凹函数（或最小化凸函数），约束为仿射等式/不等式」的形式。

---

## （接上页：Problem 6 —— resource / product planning 的解答，续）

### 情形 II 的推导收尾：两种资源订购策略下的最优利润

原文此处承接前面关于**资源采购与产品生产规划（resource / product planning）**问题的讨论。为便于阅读，先给出两种情形的问题形式，再进入数值结果。

#### 情形 I：先订购资源，且产品组合（product mix）也只能按期望需求一次性决定

在这种情形下，产品产量是在**不知道**实际需求（demand）之前、随资源一起决定的，因此只有一个产品产量向量 $q$。期望利润（expected profit）为

```math
-c^{\top} r + \sum_{k=1}^{K} \pi_k \, p^{\top}\min\{q, d^{(k)}\},
```

其中：

- $c^{\top} r$ 是采购资源 $r$ 的确定性成本（deterministic cost，因为资源价格 $c$ 与订购量 $r$ 都是确定的）；
- $\min\{q, d^{(k)}\}$ 表示**逐元素取小**（elementwise minimum），即在每个产品维度上取"实际产量 $q_i$"与"需求 $d^{(k)}_i$"中的较小者——因为能卖出的量最多等于需求量，超出需求的部分卖不出去；
- 对 $k=1,\dots,K$ 加权 $\pi_k$ 求和，即对需求的所有可能情形取期望。

于是问题形式化为：

```math
\begin{aligned}
& \text{maximize} \quad -c^{\top} r + \sum_{k=1}^{K} \pi_k \, p^{\top}\min\{q, d^{(k)}\} \\
& \text{subject to} \quad q \succeq 0, \quad r \succeq 0, \quad r \succeq A q,
\end{aligned}
```

其中变量为 $r$ 与 $q$。目标函数是凹的（concave）且分段线性（piecewise-linear），约束全都是线性不等式（linear inequality），因此这是一个凸优化问题（convex optimization problem，即目标函数凹、约束集为凸集、可化为标准凸形式的最小化问题）。

> **译注**：为什么目标是凹的？$\min\{q, d^{(k)}\}$ 关于 $q$ 是逐元素取小，而两个凹函数逐元素取小仍是凹函数（实际上 $\min$ 是凹函数，因为它是两个线性函数的下确界）；再乘非负价格 $p^{\top}$（$p\ge0$）仍是凹的，$c^{\top}r$ 是线性项，故整体关于 $(r,q)$ 凹。约束 $q\succeq0$、$r\succeq0$、$r\succeq Aq$ 都是半空间（halfspace）类型的线性不等式，故可行集为凸集。凹目标最大化等价于凸目标最小化，故这是凸问题。

#### 情形 II：先订购资源，但在**知道需求之后**再决定产品组合

现在转向第二种情形：我们必须先下单订购资源，但可以在知道需求之后再选择产品组合。此时，对每一种可能的需求向量都需要一个**单独**的产品产量向量，因为不同的需求情形下最优产量不同。于是变量为

```math
r \in \mathbb{R}^{m}_{+}, \qquad q^{(1)}, \dots, q^{(K)} \in \mathbb{R}^{n}_{+},
```

其中 $q^{(k)}$ 是"当 $d^{(k)}$ 成为实际产品需求时"我们生产的产品组合。问题可写为：

```math
\begin{aligned}
& \text{maximize} \quad -c^{\top} r + \sum_{k=1}^{K} \pi_k \, p^{\top}\min\{q^{(k)}, d^{(k)}\} \\
& \text{subject to} \quad r \succeq 0, \\
& \qquad\qquad\qquad q^{(k)} \succeq 0, \quad r \succeq A q^{(k)}, \qquad k = 1,\dots,K.
\end{aligned}
```

注意：变量 $r$ 与 $q^{(1)},\dots,q^{(K)}$ 必须**同时**决定，要统筹考虑所有这些变量；不能把它们分开各自优化。也不能先只考虑 $r$、而不考虑各个 $q^{(k)}$ 就作决定。（不过，一旦我们解出上述问题得到正确的 $r$，在知道真实需求向量后，仍可就 $q$ 再优化一次；但这一步并无必要，因为我们早已为每一种可能的需求向量都算好了最优的 $q$。）

> **译注**：这句话点出了两阶段随机规划（two-stage stochastic program）的关键性质：第一阶段决策 $r$ 是"一个"必须现在就定的量，但它在约束中要同时应对所有 $K$ 种需求情形（每条 $r\succeq Aq^{(k)}$ 都要满足）；第二阶段决策 $q^{(k)}$ 是"每种情形各一个"。尽管看起来像是"先定 $r$、后定 $q$"，但在求解时它们是一个整体优化问题，不能拆分，因为 $r$ 的最优值取决于所有情形下 $q^{(k)}$ 的联合优化结果。

我们可以把这个问题再简化一点。在这种情况下，我们不会生产任何**过剩的产品**，因为那会浪费资源，而且我们是在决定产量之前就知道需求的。所以会有 $q \preceq d$（即对每个 $k$ 有 $q^{(k)}\preceq d^{(k)}$），于是可以把问题写成：

```math
\begin{aligned}
& \text{maximize} \quad -c^{\top} r + \sum_{k=1}^{K} \pi_k \, p^{\top} q^{(k)} \\
& \text{subject to} \quad r \succeq 0, \\
& \qquad\qquad\qquad d^{(k)} \succeq q^{(k)} \succeq 0, \quad r \succeq A q^{(k)}, \qquad k = 1,\dots,K.
\end{aligned}
```

> **译注**：这里的简化依据是：因为知道需求后才决定产量，所以最优解绝不会让 $q^{(k)}_i > d^{(k)}_i$（多生产的部分卖不出去、纯浪费资源），故可强制 $q^{(k)}\preceq d^{(k)}$。这样一来 $\min\{q^{(k)}, d^{(k)}\}$ 就等于 $q^{(k)}$ 本身，目标里的 $\min$ 被直接化简掉，问题变得更简洁。

---

## （第 28 页）

利用 `planning_data.m` 中的数据，两种问题的最优资源向量 $r$ 分别为

```math
r_{\text{I}} = (45.5,\ 65.8,\ 44.6,\ 76.7,\ 72.0,\ 77.8,\ 59.3,\ 61.9,\ 83.2,\ 73.1),
```

```math
r_{\text{II}} = (65.7,\ 59.6,\ 55.2,\ 70.8,\ 69.7,\ 68.4,\ 55.3,\ 57.4,\ 63.8,\ 66.2).
```

它们分别给出期望利润 $114.3$ 与 $133.0$。

> **译注**：$r_{\text{I}}$ 是情形 I（资源与产品组合都要先定）的最优资源向量，$r_{\text{II}}$ 是情形 II（先订资源、看需求再定产量）的最优资源向量。注意 $r_{\text{II}}$ 给出更高的期望利润（$133.0 > 114.3$），这正是"信息越多、利润越高"的直观体现：情形 II 掌握了需求信息后才定产量，能更充分地利用资源。

求解该问题的 CVX 代码如下。

```matlab
planning_data;
disp('Problem I.')
cvx_begin
variable r(m); % amount of each raw material to buy.
variable q(n); % amount of each product to manufacture.
r >= 0;
q >= 0;
r >= A*q;
S = min(q*ones(1,K),D);
maximize(p'*S*pi - c'*r)
cvx_end
rI = r; disp(r); disp(cvx_optval);
profitI = p'*S - c'*r; % save for graphing.
```

```matlab
disp('Problem II.')
cvx_begin
variable r(m); % amount of each raw material to buy.
variable q(n, K); % product to manufacture, for each demand.
q >= 0;
r >= 0;
r*ones(1,K) >= A*q;
S = min(q, D);
maximize(p'*S*pi - c'*r)
cvx_end
rII = r; disp(rII); disp(cvx_optval);
profitII = p'*S - c'*r; % save for graphing.
```

**译注**：

- 情形 I（Problem I）：变量 $r\in\mathbb{R}^m$（每种原料的采购量）、$q\in\mathbb{R}^n$（每种产品的产量）。`r >= A*q` 即资源约束 $r\succeq Aq$；`S = min(q*ones(1,K), D)` 把产量向量 $q$ 复制 $K$ 列后与需求矩阵 $D$ 逐元素取小，得到 $n\times K$ 的"实际销量"矩阵 $S$，其第 $k$ 列就是 $\min\{q, d^{(k)}\}$。目标 `p'*S*pi - c'*r` 即 $-c^{\top}r + \sum_k \pi_k p^{\top}\min\{q,d^{(k)}\}$。
- 情形 II（Problem II）：变量 $q$ 扩展为 $n\times K$ 矩阵，第 $k$ 列是 $q^{(k)}$。`r*ones(1,K) >= A*q` 把资源约束对每一列同时施加，等价于 $r\succeq Aq^{(k)}$ 对所有 $k$。其余同理。注意此代码中并未显式写 $q^{(k)}\preceq d^{(k)}$，而是直接用 $\min(q,D)$ 处理——因为 $\min$ 本身已保证不会"多卖"，两种写法在最优值上等价。

我们另外提及两个没有要求你深入探究的优化问题。第一，假设我们在第一种方案下承诺采购 $r_{\text{I}}$ 的资源，但之后（在开始生产前）得知了需求。此时的问题是：用第一题中的 $r_{\text{I}}$ 替代 $r_{\text{II}}$，期望利润会有多**次优（suboptimal）**？对于给定的数值实例，我们的期望利润从 $133.0$ 下降到 $128.7$。

> **译注**：这一问考察的是"用错了资源向量"的代价。$r_{\text{I}}$ 是在"不知需求"的前提下选的，当后来得知需求后仍只能沿用 $r_{\text{I}}$（因为资源已承诺订购），于是只能在这个固定的 $r_{\text{I}}$ 下重新优化 $q$。结果利润为 $128.7$，比最优的 $133.0$ 少了 $4.3$——这是信息不足造成的、且事后无法弥补的损失。

---

## （第 29 页）

最后，考察"在购买资源之前就已知需求"时的表现也很有意思。这是**全预知（full prescience）**、即**完全信息（full information）**的情形——我们事先知道一切。此时，我们可以对每一种可能的需求向量**分别**优化 $r$ 与 $q$ 的选择。这显然给出了信息较少时所能获得利润的一个**上界（upper bound）**。在这种情形下，全预知时的期望利润为 $161.4$。

> **译注**：全预知情形的期望利润 $161.4$ 是三种情形的上界：$161.4 > 133.0$（情形 II）$> 114.3$（情形 I）。这是因为全预知时连资源 $r$ 都可以按每种需求情形单独优化，而情形 II 中 $r$ 是"一个"要同时应对所有情形的量，自由度更小。

求解这两个附加问题的代码如下。

```matlab
disp('Problem II but with rI.')
r = rI;
cvx_begin
variable q(n, K); % product to manufacture, for each demand.
q >= 0;
r*ones(1,K) >= A*q;
S = min(q, D);
maximize(p'*S*pi - c'*r)
cvx_end
disp(p'*S*pi - c'*r);
profitIII = p'*S - c'*r; % save for graphing.
```

```matlab
disp('Full prescience.')
cvx_begin
variable r(m, K); % amount of each raw material to buy, for each demand.
variable q(n, K); % product to manufacture, for each demand.
q >= 0;
r >= 0;
r >= A*q;
S = min(q, D);
maximize((p'*S - c'*r)*pi)
cvx_end
disp((p'*S - c'*r)*pi);
profitIV = p'*S - c'*r; % save for graphing.
```

**译注**：

- 第一个代码块：固定 $r = r_{\text{I}}$（不再作为变量），只优化 $q$（$n\times K$），得到"用错资源"情形下的最优期望利润 $128.7$。
- 第二个代码块（全预知）：$r$ 也扩展为 $m\times K$ 矩阵，第 $k$ 列是"若需求为 $d^{(k)}$ 时采购的资源"。约束 `r >= A*q` 逐列成立，即每种情形各自有独立的资源-产量关系。目标 `(p'*S - c'*r)*pi` 是先把每种情形的利润 $p^{\top}\min\{q^{(k)},d^{(k)}\} - c^{\top}r^{(k)}$ 算出来，再按 $\pi$ 加权求和，即全预知下的期望利润 $161.4$。

下面给出不同可能结果对应的利润直方图（histogram）。

---

## （第 30 页 —— 利润直方图）

> 本页为四张利润直方图，横轴为利润值（范围 $0$–$400$，刻度 $0,50,100,\dots,400$），纵轴为频数（范围 $0$–$15$，刻度 $0,5,10,15$）。四张图分别对应四种情形的利润分布，标题原文为：

**Figure 5**: Histograms of profit for the four scenarios.（四种情形下的利润直方图。）

**译注**：这四张直方图分别展示了（按原文顺序推断）情形 I、情形 II、用错资源 $r_{\text{I}}$ 的情形、以及全预知情形的利润分布。由于利润是随机变量（取决于随机需求 $d^{(k)}$），直方图直观呈现了不同决策策略下利润的取值范围与集中程度——信息越多，不仅期望利润越高，利润分布的支撑通常也更靠右。

---

## （第 31 页）

## Problem 7 — Dual of exponential cone（指数锥的对偶）

**指数锥的对偶。** 指数锥（exponential cone）$K_{\exp}\subseteq\mathbb{R}^{3}$ 定义为

```math
K_{\exp} = \left\{ (x, y, z) \;\middle|\; y > 0,\ y\,e^{x/y} \le z \right\}.
```

请将对偶锥（dual cone）表为如下形式：

```math
K_{\exp}^{*} = \left\{ (u, v, w) \;\middle|\; \cdots\ \text{你的条件写在这里}\ \cdots \right\}.
```

你的条件必须简短（当然不超过一行），且不能涉及除 $u$、$v$、$w$ 之外的任何变量。

我们并不在意这些锥的**边界（boundary）**上的细节问题，所以你大可不必担心。但我们在此为那些在意这些细节的人作几点说明。

如上定义的锥 $K_{\exp}$ **不是闭的（not closed）**。要得到它的**闭包（closure）**，需要补充下面这些点：

```math
\left\{ (x, y, z) \;\middle|\; x \le 0,\ y = 0,\ z \ge 0 \right\}.
```

（这不会造成差异，因为一个锥的对偶等于它闭包的对偶。）

> **译注**：先明确两个关键定义，便于后文理解。
>
> - **对偶锥（dual cone）**：设 $K$ 是锥（cone，即对任意 $x\in K$ 与 $\lambda\ge0$ 有 $\lambda x\in K$），其对偶锥定义为 $K^{*} = \{ s \mid s^{\top}x \ge 0,\ \forall x\in K \}$，即"与 $K$ 中所有向量内积非负的向量全体"。
> - **闭包（closure）**：集合加上它所有极限点后得到的闭集。指数锥 $K_{\exp}$ 定义里要求 $y>0$（严格），故不含 $y=0$ 的那部分边界点，从而非闭；补上 $x\le0,\ y=0,\ z\ge0$ 后才是闭锥。
>
> 一个基本事实是 $K^{*} = (\operatorname{cl}K)^{*}$（对偶锥等于其闭包的对偶锥），所以补不补边界点不影响对偶锥的结果，这也是原文说"无需担心边界细节"的原因。

**Solution（解答）。** 对偶锥可表示为

```math
K_{\exp}^{*} = \operatorname{cl}\left\{ (u, v, w) \;\middle|\; u < 0,\ -u\,e^{v/u} \le e\,w \right\}
```

即

```math
K_{\exp}^{*} = \left\{ (u, v, w) \;\middle|\; u < 0,\ -u\,e^{v/u} \le e\,w \right\} \cup \left\{ (0, v, w) \;\middle|\; v \ge 0,\ w \ge 0 \right\},
```

其中 $\operatorname{cl}$ 表示闭包（closure）。我们不指望大家把边界上的点（即第二行里的第二个集合）也算出来。

> **译注**：注意原文里指数项写的是 $-u\,e^{v/u} \le e\,w$（左端 $-u\,e^{v/u}$，右端 $e$ 乘 $w$）。这里的 $e$ 是自然对数的底（Euler 数），系数 $e$ 来源于后面推导中"指数函数 $y\,e^{x/y}$ 在极小化时出现的自然常数"，详见下文推导。

这个对偶锥还可以用其他几种方式表示。条件 $u<0$、$-u\,e^{v/u}\le e\,w$ 可以写成

```math
(u/w)\log(-u/w) + v/w - u/w \ge 0,
```

或

```math
u\log(-u/w) + v - u \ge 0,
```

或

```math
\log(-u/w) \le 1 - (v/u).
```

> **译注**：这三种形式是等价的。验证一下：从 $u\log(-u/w) + v - u \ge 0$ 出发，两边除以 $u$（注意 $u<0$，除以负数要反向）……但更稳妥的是从指数形式反推。由 $-u\,e^{v/u}\le e\,w$ 且 $u<0$，两边除以 $-u>0$ 得 $e^{v/u}\le \dfrac{e\,w}{-u}$，即 $e^{v/u}\le e\cdot \dfrac{w}{-u} = e\cdot \dfrac{-u}{-u}$？这里要小心整理。实际上，把 $-u\,e^{v/u}\le e\,w$ 两边取对数（两边都为正，因为 $-u>0$、$e^{v/u}>0$、$w\ge0$，当 $w>0$ 时）：

```math
\log(-u) + \frac{v}{u} \le 1 + \log w.
```

移项得

```math
\frac{v}{u} \le 1 + \log w - \log(-u) = 1 + \log\left(\frac{w}{-u}\right) = 1 - \log\left(\frac{-u}{w}\right).
```

即 $\log(-u/w) \le 1 - (v/u)$，正是第三种形式（因为 $\log\left(\frac{w}{-u}\right) = -\log\left(\frac{-u}{w}\right)$）。再两边乘 $u$（$u<0$）得 $u\log(-u/w) \ge u - v$，即 $u\log(-u/w) + v - u \ge 0$，正是第二种形式。第一种形式是第二种除以 $w$（$w>0$）得到的。三种表示完全等价。

现在来推导这个结果。要判定 $(u,v,w)\in K_{\exp}^{*}$，需要满足：对任意 $y>0$ 且 $y\,e^{x/y}\le z$ 的 $(x,y,z)$，都有 $ux + vy + wz \ge 0$。因此首先必须有 $w\ge0$；否则我们可以取足够大的 $z$，使 $ux + vy + wz$ 变为负。

> **译注**：为什么 $w\ge0$ 是必要条件？对偶锥要求对所有 $(x,y,z)\in K_{\exp}$ 都有 $ux+vy+wz\ge0$。而 $K_{\exp}$ 中的点可以取任意大的 $z$（只要 $z\ge y\,e^{x/y}$，$z$ 向上无界）。若 $w<0$，则固定 $x,y$ 后，把 $z$ 取得足够大，$wz$ 这一项会以 $|w|z$ 的速率趋向 $-\infty$，从而 $ux+vy+wz<0$，违反对偶锥定义。故必须 $w\ge0$。

现在看 $w=0$ 时会怎样。此时需要 $ux + vy \ge 0$ 对所有 $y>0$ 成立。这只有当 $u=0$ 且 $v\ge0$ 时才可能。于是，满足

```math
u = 0, \quad v \ge 0, \quad w = 0
```

的点都在 $K_{\exp}^{*}$ 中。

> **译注**：当 $w=0$ 时，$wz$ 项消失，只剩 $ux + vy$。由于 $x$ 可以取任意实数（在 $y>0$ 固定时，$x$ 满足 $y\,e^{x/y}\le z$，即 $x\le y\log(z/y)$，故 $x$ 可任意负、也可取到某个上界），若 $u\ne0$，取 $x$ 使得 $ux$ 足够负即可破坏 $ux+vy\ge0$（例如 $u>0$ 时取 $x\to-\infty$，$u<0$ 时取 $x$ 尽可能大）。故必须 $u=0$。此时剩 $vy\ge0$ 对 $y>0$ 成立，故 $v\ge0$。

---

## （第 32 页）

现在考虑 $w>0$ 的情形。我们要在满足 $y>0$ 且 $y\,e^{x/y}\le z$ 的所有 $x,y,z$ 上最小化 $ux+vy+wz$。由于 $w>0$，我们对 $z$ 取最小值时应取 $z = y\,e^{x/y}$（即取到约束的边界），这得到

```math
ux + vy + w\,y\,e^{x/y}.
```

> **译注**：这是"极小化"这一步的关键。$wz$ 关于 $z$ 是递增的（因为 $w>0$），而约束要求 $z\ge y\,e^{x/y}$，所以要最小化 $wz$ 就必须把 $z$ 压到最小允许值 $z = y\,e^{x/y}$。代回后得到只关于 $x,y$ 的表达式 $ux + vy + w\,y\,e^{x/y}$。

现在对 $x$ 取最小值。若 $u>0$，则当 $x\to-\infty$ 时该式**下无界（unbounded below）**。若 $u=0$，最小值不可达（not achieved），而是在 $x\to-\infty$ 时趋近；此时最小值是 $vy$。它要对所有 $y>0$ 有非负的最小值，仅当 $v\ge0$。于是我们发现，满足

```math
u = 0, \quad v \ge 0, \quad w > 0
```

的点都在 $K_{\exp}^{*}$ 中。

> **译注**：分两种子情形讨论对 $x$ 的极小化：
>
> - 若 $u>0$，项 $ux$ 在 $x\to-\infty$ 时趋于 $-\infty$，而 $w\,y\,e^{x/y}$ 在 $x\to-\infty$ 时趋于 $0$（因为 $y>0$，指数 $x/y\to-\infty$），$vy$ 与 $x$ 无关，故整体 $\to-\infty$，下无界，说明这些点不能进对偶锥（否则会找到 $ux+vy+wz<0$ 的实例）。
> - 若 $u=0$，表达式变为 $vy + w\,y\,e^{x/y}$。由于 $w\,y\,e^{x/y}\to0$ 当 $x\to-\infty$，其下确界是 $vy$（不可达，因为需要 $x=-\infty$，但可通过 $x$ 任意负逼近）。要保证对所有 $y>0$ 这个下确界 $\ge0$，需 $v\ge0$。

若 $u<0$，则 $ux + vy + w\,y\,e^{x/y}$ 的最小值出现在它关于 $x$ 的导数（derivative）为零处。这导致 $u + w\,e^{x/y} = 0$，即 $x = y\log(-u/w)$。对这个 $x$ 值，上式变为

```math
y\left( u\log(-u/w) + v - u \right).
```

> **译注**：这里用一阶最优条件求极小点。对 $x$ 求导（把 $y$ 当常数）：

```math
\frac{\partial}{\partial x}\left(ux + vy + w\,y\,e^{x/y}\right) = u + w\,y\cdot \frac{1}{y}\,e^{x/y} = u + w\,e^{x/y}.
```

令其为零：$u + w\,e^{x/y}=0$，即 $w\,e^{x/y} = -u$，两边除以 $w>0$ 得 $e^{x/y} = -u/w$，取对数得 $x/y = \log(-u/w)$，故 $x = y\log(-u/w)$。因为 $u<0$，$-u/w>0$，对数有意义；又因 $\frac{\partial^2}{\partial x^2} = \frac{w}{y}e^{x/y} > 0$（$w>0,y>0$），二阶导为正，故这是唯一极小点。把 $x=y\log(-u/w)$ 代回：

```math
ux + w\,y\,e^{x/y} = u\,y\log(-u/w) + w\,y\,e^{\log(-u/w)} = y\,u\log(-u/w) + w\,y\cdot\frac{-u}{w} = y\,u\log(-u/w) - u\,y,
```

再加上 $vy$，得 $y\left(u\log(-u/w) + v - u\right)$，与原文一致。

现在对 $y>0$ 取最小值。若 $u\log(-u/w) + v - u < 0$，得到 $-\infty$；若 $u\log(-u/w) + v - u \ge 0$，得到 $0$。

> **译注**：表达式 $y\left(u\log(-u/w)+v-u\right)$ 关于 $y$ 是线性的（括号内与 $y$ 无关）。若括号内为负，则当 $y\to\infty$ 时整体 $\to-\infty$，下无界；若括号内 $\ge0$，则最小值在 $y\to0^{+}$ 时趋近于 $0$（因为 $y>0$，可取任意小），故最小值为 $0$。于是整个 $ux+vy+wz$ 对所有 $(x,y,z)\in K_{\exp}$ 都 $\ge0$ 的充要条件是括号内 $\ge0$。

所以最终我们得到条件：

```math
u\log(-u/w) + v - u \ge 0, \quad u < 0, \quad w > 0.
```

两边除以 $u$ 并取指数（exponential），可把它写成

```math
-u\,e^{v/u} \le e\,w, \quad u < 0.
```

（条件 $w>0$ 已经由这两个条件蕴含。）

> **译注**：验证最后一步的等价变形。由 $u\log(-u/w)+v-u\ge0$ 且 $u<0$，移项得 $u\log(-u/w) \ge u - v$。两边除以 $u$（负数，反向）得 $\log(-u/w) \le 1 - \dfrac{v}{u}$。两边取指数（$e^{\cdot}$ 单调递增）得 $\dfrac{-u}{w} \le e^{1 - v/u}$。两边乘 $w$（$w>0$）得 $-u \le w\,e^{1-v/u} = e\,w\cdot e^{-v/u}$。两边乘 $e^{v/u}>0$ 得 $-u\,e^{v/u}\le e\,w$，与原文一致。反之从 $-u\,e^{v/u}\le e\,w$ 逆推回去即得原不等式，故二者等价。至于"$w>0$ 已被蕴含"：因为左端 $-u\,e^{v/u}>0$（$u<0$），故 $e\,w \ge -u\,e^{v/u} > 0$，从而 $w>0$ 自动成立。

最后，我们得到

```math
K_{\exp}^{*} = \left\{ (u, v, w) \;\middle|\; u < 0,\ -u\,e^{v/u} \le e\,w \right\} \cup \left\{ (0, v, w) \;\middle|\; v \ge 0,\ w \ge 0 \right\}.
```

> **译注**：这是对偶锥的最终闭形式。它由两部分并成：
>
> 1. 内部部分 $\{u<0,\ -u\,e^{v/u}\le e\,w\}$：对应 $w>0$ 时 $u<0$ 的情形（以及 $u<0$ 时 $w$ 可取到 $0$ 的边界，由闭包补上）；
> 2. 边界部分 $\{(0,v,w)\mid v\ge0,w\ge0\}$：对应 $u=0$ 的情形（含 $w=0$ 的 $u=0,v\ge0$ 与 $w>0$ 的 $u=0,v\ge0$ 两种）。
>
> 这个结果把 $u=0$、$u<0$、$u>0$ 三种情形（$u>0$ 时被排除，因为下无界）完整汇总，即为指数锥的对偶锥。
