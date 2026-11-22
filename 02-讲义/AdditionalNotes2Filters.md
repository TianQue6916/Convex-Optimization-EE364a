# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

> <span style="color:#7f8c8d;">Source: SEE官方讲义/AdditionalNotes2Filters.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

## <span style="color:#2471a3;">**[lecture]**</span> <span style="color:#c0392b;">**Filter design（滤波器设计）**</span>

<!-- p.1 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Filter design（滤波器设计）：本讲概览**</span>

本讲讨论如何用凸优化做滤波器设计（filter design），主题包括：

- FIR 滤波器（FIR filters）
- Chebychev 设计（Chebychev design）
- 线性相位滤波器设计（linear phase filter design）
- 均衡器设计（equalizer design）
- 滤波器幅度指标（filter magnitude specifications）

<!-- p.2 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**FIR filters（FIR 滤波器）**</span>

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">**有限冲激响应（finite impulse response, FIR）滤波器：**</span>

```math
y(t)=\sum_{\tau=0}^{n-1}h_\tau\,u(t-\tau),\qquad t\in\mathbb{Z}
```

- 序列（sequence）$u:\mathbb{Z}\to\mathbb{R}$ 是输入信号（input signal）
- 序列 $y:\mathbb{Z}\to\mathbb{R}$ 是输出信号（output signal）
- $h_i$ 称为滤波器系数（filter coefficients）
- $n$ 称为滤波器阶数（order）或长度（length）

<!-- p.3 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**滤波器的频率响应（frequency response）**</span>

滤波器的频率响应（frequency response）$H:\mathbb{R}\to\mathbb{C}$：

```math
\begin{aligned}
H(\omega)&=h_0+h_1e^{-j\omega}+\cdots+h_{n-1}e^{-j(n-1)\omega}\\
&=\sum_{t=0}^{n-1}h_t\cos t\omega+j\sum_{t=0}^{n-1}h_t\sin t\omega
\end{aligned}
```

- 这里的 $j$ 表示 $\sqrt{-1}$（EE 传统记法）
- $H$ 是周期的（periodic）且共轭对称（conjugate symmetric），因此只需知道/规定 $0\le\omega\le\pi$ 范围内的行为

FIR 滤波器设计问题（FIR filter design problem）：选择 $h$，使 $h$（系数本身）与 $H$ 满足/优化给定的指标（specs）。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 先从名字拆起：取输入为单位冲激 $u=\delta$（只在 $t=0$ 处为 1），代入定义式立即得到 $y(t)=h_t$——输出恰好就是系数序列本身，"有限冲激响应"说的正是它只有 $n$ 个非零样本，这也是 FIR 与无限冲激响应（IIR）滤波器的分界线。它与 Strang 线代有两条连接路径：其一，把卷积写成矩阵形式 $y=Tu$，$T$ 的第 $t$ 行是 $h$ 右移 $t$ 拍形成的带状 Toeplitz 行，滤波因此是一次结构化矩阵乘法，计算量为 $O(n)$ 而非稠密乘法的 $O(n^2)$；其二，把 $h_0,\dots,h_{n-1}$ 看成多项式系数，$H(\omega)$ 就是该多项式在单位圆上的取值（z 变换的观点），"设计滤波器"于是等同于"设计一个在单位圆上取值受控的 $n-1$ 次多项式"。另有两处 EE 习惯值得记：用 $j$ 而非 $i$ 表示 $\sqrt{-1}$（$i$ 已留给电流），指数里的负号 $e^{-j\tau\omega}$ 刻画"延迟 $\tau$ 拍"。最后核对符号时必须指出：原文上式第二行印作 $+j\sum h_t\sin t\omega$，但按第一行 $e^{-jt\omega}=\cos t\omega-j\sin t\omega$ 展开、并对照后文 SOCP 一节中 $A^{(k)}$ 第二行取 $-\sin$、$b^{(k)}$ 取 $+\Im H_{\rm des}$，此处应为 $-j$，原文当属笔误，理解时请按 $-j$。

<!-- p.4 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**示例：低通（lowpass）FIR 滤波器（$n=21$）的冲激响应**</span>

<span style="color:#2471a3;">**[example]**</span> 示例：低通 FIR 滤波器，阶数 $n=21$。图为冲激响应 $h$ 的取值曲线：横轴为 $t$（$t=0,1,\dots,20$），纵轴为 $h(t)$，取值大致在 $-0.2$ 到 $0.2$ 之间。

<!-- p.5 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**示例（续）：频率响应的幅度与相位**</span>

<span style="color:#2471a3;">**[example]**</span> 两张图，横轴均为 $\omega\in[0,\pi]$（刻度 $0,0.5,\dots,3$）：上图是频率响应幅度（frequency response magnitude），即 $|H(\omega)|$，纵轴为对数刻度（$10^{-3}$ 至 $10^{1}$），曲线呈低通形状；下图是频率响应相位（phase），即 $\angle H(\omega)$，纵轴范围约 $-3$ 至 $3$（弧度）。

<!-- p.6 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Chebychev 设计（Chebychev design）**</span>

<span style="color:#2471a3;">**[definition]**</span> **Chebychev 设计问题：**

```math
\operatorname{minimize}\quad \max_{\omega\in[0,\pi]}\left|H(\omega)-H_{\rm des}(\omega)\right|
```

- $h$ 是优化变量（optimization variable）
- $H_{\rm des}:\mathbb{R}\to\mathbb{C}$ 是（给定的）期望传递函数（desired transfer function）
- 这是凸问题（convex problem）
- 可以附加约束，例如 $|h_i|\le 1$

对频率采样（离散化，sample/discretize）：

```math
\operatorname{minimize}\quad \max_{k=1,\dots,m}\left|H(\omega_k)-H_{\rm des}(\omega_k)\right|
```

- 采样点 $0\le\omega_1<\cdots<\omega_m\le\pi$ 是固定的（例如 $\omega_k=k\pi/m$）
- $m\gg n$（常用经验法则：$m=15n$）
- 得到上述问题的一个近似（松弛，relaxation）

<!-- p.7 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**用 SOCP 求解 Chebychev 设计（Chebychev design via SOCP）**</span>

<span style="color:#2471a3;">**[algorithm]**</span> **Chebychev 设计的二阶锥规划（second-order cone programming, SOCP）形式：**

```math
\begin{aligned}
\operatorname{minimize}\quad & t\\
\text{subject to}\quad & \|A^{(k)}h-b^{(k)}\|\le t,\qquad k=1,\dots,m
\end{aligned}
```

其中

```math
A^{(k)}=\begin{bmatrix}
1 & \cos\omega_k & \cdots & \cos(n-1)\omega_k\\
0 & -\sin\omega_k & \cdots & -\sin(n-1)\omega_k
\end{bmatrix}
```

```math
b^{(k)}=\begin{bmatrix}\Re H_{\rm des}(\omega_k)\\[2pt] \Im H_{\rm des}(\omega_k)\end{bmatrix},\qquad
h=\begin{bmatrix}h_0\\ \vdots\\ h_{n-1}\end{bmatrix}
```

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这一页是全书主线"minimax → 上图化 → 锥约束"在信号处理里的标准落地，值得把三步走看清：第一步，目标 $\max_k|H(\omega_k)-H_{\rm des}(\omega_k)|$ 是 $m$ 个凸函数的最大值，虽凸却不是标准形式；第二步引入辅助变量 $t$ 作上图（epigraph）变量，把目标线性化为 minimize $t$，代价是添上 $m$ 条约束 $|H(\omega_k)-H_{\rm des}(\omega_k)|\le t$；第三步，复数模长 $|a+jb|$ 恰是二维实向量 $(\Re,\Im)$ 的欧氏范数 $\|(a,b)\|_2$（Strang 线代里的老朋友），于是每条约束都是一个二阶锥约束，整题成为 SOCP。与最小二乘对照着记更有感觉：Strang 18.065 的最小二乘最小化误差平方和，照顾"平均表现"；Chebychev 最小化最坏频点误差，是 $\ell_\infty$ 品味，保证"任何频点都不能差"。经验法则 $m=15n$ 意味着约束数约为变量数的 15 倍，对内点法（interior-point method）而言仍是小规模问题。顺带一句历史：这种"最坏频点最小化"的等波纹设计思想早在 1960 年代就由 Remez 交换算法（Parks–McClellan，1972）实现，凸优化的贡献是把它变成可以随意附加约束的通用工具。

<!-- p.8 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**线性相位滤波器（linear phase filters）**</span>

假设（suppose）：

- $n=2N+1$ 为奇数
- 冲激响应关于中点对称（symmetric about midpoint）：

```math
h_t=h_{n-1-t},\qquad t=0,\dots,n-1
```

则

```math
\begin{aligned}
H(\omega)&=h_0+h_1e^{-j\omega}+\cdots+h_{n-1}e^{-j(n-1)\omega}\\
&=e^{-jN\omega}\left(2h_0\cos N\omega+2h_1\cos(N-1)\omega+\cdots+h_N\right)\\
&\triangleq e^{-jN\omega}\tilde H(\omega)
\end{aligned}
```

<!-- p.9 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**线性相位滤波器（续）**</span>

- 项 $e^{-jN\omega}$ 表示 $N$ 个样本的延迟（delay）
- $\tilde H(\omega)$ 是实值的
- $|H(\omega)|=|\tilde H(\omega)|$
- 称为线性相位滤波器（linear phase filter）（$\angle H(\omega)$ 除 $\pm\pi$ 的跳变外是线性的）

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> "对称 ⇒ 线性相位"值得亲手复算一遍，它是 FIR 设计里最优雅的代数事实：利用 $h_t=h_{2N-t}$，把第 $t$ 项与它的镜像第 $2N-t$ 项配对，得 $h_te^{-jt\omega}+h_te^{-j(2N-t)\omega}=2h_t\cos((N-t)\omega)\,e^{-jN\omega}$——正弦（虚部）成对抵消，相位里只剩因子 $e^{-jN\omega}$。相位是 $\omega$ 的线性函数（斜率 $-N$）在物理上意味着群延迟（group delay）恒定：所有频率分量被统一推迟 $N$ 拍，波形只整体平移、不发生色散式失真，这正是音频与图像系统偏爱线性相位滤波器的原因。设计上的实惠同样具体：幅度只涉及实值函数 $\tilde H$，约束对 $h$ 是线性的，后文的 SOCP 因此降级为 LP，自由度也从 $n$ 个减到 $N+1$ 个。相频图中 $\pm\pi$ 的跳变则来自实值因子 $\tilde H(\omega)$ 过零变号：每穿过一个零点，相位瞬时翻转 $\pi$，讲义"除 $\pm\pi$ 跳变外线性"的措辞正是为此留的余地。

<!-- p.10 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**低通滤波器指标（lowpass filter specifications）**</span>

图示：幅度指标的形状，横轴为 $\omega$（从 $0$ 到 $\pi$），图中标出了边界 $\delta_1$、$1/\delta_1$、$\delta_2$ 与转折频率 $\omega_p$、$\omega_s$、$\pi$——通带内幅度被夹在 $1/\delta_1$ 与 $\delta_1$ 两条线之间，阻带内幅度不超过 $\delta_2$。

设计思想（idea）：

- 让通带（passband）$[0,\omega_p]$ 内的频率分量通过
- 让阻带（stopband）$[\omega_s,\pi]$ 内的频率分量被阻挡

<!-- p.11 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**指标（specifications）**</span>

指标（specifications）：

- 最大通带纹波（maximum passband ripple）（即 $\pm 20\log_{10}\delta_1$ dB）：

```math
1/\delta_1\le|H(\omega)|\le\delta_1,\qquad 0\le\omega\le\omega_p
```

- 最小阻带衰减（minimum stopband attenuation）（即 $-20\log_{10}\delta_2$ dB）：

```math
|H(\omega)|\le\delta_2,\qquad \omega_s\le\omega\le\pi
```

<!-- p.12 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**线性相位低通滤波器设计（linear phase lowpass filter design）**</span>

- 对频率采样
- 不失一般性（wlog, without loss of generality）可设 $\tilde H(0)>0$，于是纹波指标为

```math
1/\delta_1\le\tilde H(\omega_k)\le\delta_1
```

按最大阻带衰减（maximum stopband attenuation）设计：

```math
\begin{aligned}
\operatorname{minimize}\quad & \delta_2\\
\text{subject to}\quad & 1/\delta_1\le\tilde H(\omega_k)\le\delta_1,\qquad 0\le\omega_k\le\omega_p\\
& -\delta_2\le\tilde H(\omega_k)\le\delta_2,\qquad \omega_s\le\omega_k\le\pi
\end{aligned}
```

<!-- p.13 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**线性相位低通滤波器设计（续）**</span>

- 通带纹波 $\delta_1$ 是给定的
- 这是关于变量 $h,\delta_2$ 的线性规划（linear program, LP）
- 自 20 世纪 60 年代起就已为人所知（并被实际使用）
- 可以附加其他约束，例如 $|h_i|\le\alpha$

变形与扩展（variations and extensions）：

- 固定 $\delta_2$，最小化 $\delta_1$（凸，但不是 LP）
- 固定 $\delta_1$ 与 $\delta_2$，最小化 $\omega_s$（拟凸，quasiconvex）
- 固定 $\delta_1$ 与 $\delta_2$，最小化阶数 $n$（拟凸）

<!-- p.14 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**示例：线性相位滤波器，$n=21$（冲激响应）**</span>

<span style="color:#2471a3;">**[example]**</span> 设计实例：

- 线性相位滤波器，$n=21$
- 通带 $[0,\,0.12\pi]$；阻带 $[0.24\pi,\,\pi]$
- 最大纹波 $\delta_1=1.012$（即 $\pm 0.1$ dB）
- 按最大阻带衰减设计

图为所得滤波器的冲激响应 $h$：横轴 $t=0,\dots,20$，纵轴 $h(t)$ 约在 $-0.2$ 到 $0.2$ 之间；由线性相位设计可知它关于中点 $t=10$ 对称。

<!-- p.15 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**示例（续）：频率响应的幅度与相位**</span>

<span style="color:#2471a3;">**[example]**</span> 与第 5 页同格式的两张图，横轴均为 $\omega\in[0,\pi]$：上图是频率响应幅度 $|H(\omega)|$（纵轴对数刻度 $10^{-3}$ 至 $10^{1}$），下图是频率响应相位 $\angle H(\omega)$（纵轴约 $-3$ 至 $3$）。

<!-- p.16 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**均衡器设计（equalizer design）**</span>

图示为级联框图：待均衡的系统 $G(\omega)$ 与均衡器 $H(\omega)$ 串联。

均衡（equalization）：给定

- $G$（未均衡的频率响应，unequalized frequency response）
- $G_{\rm des}$（期望的频率响应，desired frequency response）

设计（FIR 均衡器）$H$，使得

```math
\tilde G\triangleq GH\approx G_{\rm des}
```

- 常见选择：$G_{\rm des}(\omega)=e^{-jD\omega}$（纯延迟，delay）；也就是说，均衡就是反卷积（deconvolution）（直到一个延迟的意义上）
- 可以对 $H$ 附加约束，例如限制 $|h_i|$ 或 $\max_\omega|H(\omega)|$

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> "均衡 = 反卷积"背后藏着一个数值计算的经典陷阱，可以用 Strang 的条件数（condition number）语言拆开：想让级联等于理想延迟，最直接的想法是逐频点取 $H(\omega)=e^{-jD\omega}/G(\omega)$——一次"除法式求逆"。但只要信道在某个频点 $\omega_0$ 处 $|G(\omega_0)|$ 很小（多径信道的频率选择性衰落中很常见），$1/G$ 就在该点爆炸，均衡器会竭力放大该频点的噪声：这正是"数学上可逆"与"数值上可逆"的区别——病态求逆把小扰动放大成大误差；再接上 CSAPP 的视角，浮点数只有有限精度，任何被放大的误差最终都会落到机器精度量级以上。所以讲义强调可以对 $H$ 加约束（限制 $|h_i|$ 或 $\max_\omega|H(\omega)|$）：这本质上是给逆问题加正则化（regularization），用"允许残余误差"换"不放大噪声"，与岭回归的思想同源；而 Chebychev 目标把"逐点精确相等"改成"最坏频点误差最小"，天然抑制了在深谷处硬追 $1/G$ 的冲动。

<!-- p.17 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Chebychev 均衡器设计（Chebychev equalizer design）**</span>

<span style="color:#2471a3;">**[definition]**</span> **Chebychev 均衡器设计：**

```math
\operatorname{minimize}\quad \max_{\omega\in[0,\pi]}\left|\tilde G(\omega)-G_{\rm des}(\omega)\right|
```

凸问题；对频率采样后是 SOCP。

<!-- p.18 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**时域均衡（time-domain equalization）**</span>

时域均衡：优化均衡后系统的冲激响应 $\tilde g$。例如取 $G_{\rm des}(\omega)=e^{-jD\omega}$，则

```math
g_{\rm des}(t)=\begin{cases}1 & t=D\\ 0 & t\neq D\end{cases}
```

采样化设计（sampled design）：

```math
\begin{aligned}
\operatorname{minimize}\quad & \max_{t\neq D}|\tilde g(t)|\\
\text{subject to}\quad & \tilde g(D)=1
\end{aligned}
```

- 这是一个 LP
- 也可以改用 $\sum_{t\neq D}\tilde g(t)^2$ 或 $\sum_{t\neq D}|\tilde g(t)|$（作为目标函数）

<!-- p.19 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**均衡的扩展（extensions）**</span>

扩展（extensions）：

- 可以施加（凸）约束
- 可以混合时域与频域指标
- 可以均衡多个系统，即选择 $H$ 使

```math
G^{(k)}H\approx G_{\rm des},\qquad k=1,\dots,K
```

- 可以均衡多输入多输出（multi-input multi-output）系统（即 $G$ 与 $H$ 是矩阵）
- 可推广到多维系统，例如图像处理（image processing）

<!-- p.20 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**均衡器设计示例：未均衡系统 $G$（10 阶 FIR）**</span>

<span style="color:#2471a3;">**[example]**</span> 未均衡系统 $G$ 是 10 阶 FIR 滤波器：图为 $G$ 的冲激响应 $g(t)$，横轴 $t=0,1,\dots,9$，纵轴取值约在 $-0.4$ 到 $1$ 之间。

<!-- p.21 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**均衡器设计示例（续）：$G$ 的频率响应与设计目标**</span>

<span style="color:#2471a3;">**[example]**</span> 两张图，横轴均为 $\omega\in[0,\pi]$：$|G(\omega)|$（纵轴对数刻度 $10^{-1}$ 至 $10^{1}$）与 $\angle G(\omega)$（纵轴约 $-3$ 至 $3$）。

设计一个 30 阶 FIR 均衡器，使得 $\tilde G(\omega)\approx e^{-j10\omega}$。

<!-- p.22 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**Chebychev 均衡器设计与均衡后的冲激响应 $\tilde g$**</span>

Chebychev 均衡器设计：

```math
\operatorname{minimize}\quad \max_\omega\left|\tilde G(\omega)-e^{-j10\omega}\right|
```

图为均衡后系统（equalized system）的冲激响应 $\tilde g$：横轴 $t=0,\dots,35$，纵轴取值约在 $-0.2$ 到 $1$ 之间。

<!-- p.23 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**均衡后系统的频率响应：幅度 $|\tilde G|$ 与相位 $\angle\tilde G$**</span>

<span style="color:#2471a3;">**[example]**</span> 均衡后系统的频率响应幅度（magnitude）$|\tilde G|$（纵轴对数刻度 $10^{-1}$ 至 $10^{1}$）与频率响应相位（phase）$\angle\tilde G$（纵轴约 $-3$ 至 $3$），横轴均为 $\omega\in[0,\pi]$。

<!-- p.24 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**时域均衡器设计与均衡后的冲激响应 $\tilde g$**</span>

时域均衡器设计：

```math
\operatorname{minimize}\quad \max_{t\neq 10}|\tilde g(t)|
```

图为均衡后系统的冲激响应 $\tilde g$：横轴 $t=0,\dots,35$，纵轴取值约在 $-0.2$ 到 $1$ 之间（此设计对应目标延迟 $D=10$）。

<!-- p.25 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**均衡后系统的频率响应（时域设计）：幅度 $|\tilde G|$ 与相位 $\angle\tilde G$**</span>

<span style="color:#2471a3;">**[example]**</span> 均衡后系统的频率响应幅度 $|\tilde G|$（纵轴对数刻度 $10^{-1}$ 至 $10^{1}$）与频率响应相位 $\angle\tilde G$（纵轴约 $-3$ 至 $3$），横轴均为 $\omega\in[0,\pi]$。

<!-- p.26 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**滤波器幅度指标（filter magnitude specifications）**</span>

传递函数（transfer function）的幅度指标（magnitude specification）形如

```math
L(\omega)\le|H(\omega)|\le U(\omega),\qquad \omega\in[0,\pi]
```

其中 $L,U:\mathbb{R}\to\mathbb{R}_+$（非负实数）是给定的函数。

- 下界约束对滤波器系数 $h$ 而言不是凸的
- 出现在许多应用中，例如音频（audio）、频谱整形（spectrum shaping）
- 可以通过换变量（change of variables）用凸优化求解

<!-- p.27 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**自相关系数（autocorrelation coefficients）**</span>

<span style="color:#2471a3;">**[definition]**</span> <span style="color:#00838f;">**与冲激响应 $h=(h_0,\dots,h_{n-1})\in\mathbb{R}^n$ 相联系的自相关系数（autocorrelation coefficients）定义为**</span>

```math
r_t=\sum_\tau h_\tau h_{\tau+t}
```

（约定：当 $k<0$ 或 $k\ge n$ 时取 $h_k=0$）

- $r_t=r_{-t}$；当 $|t|\ge n$ 时 $r_t=0$
- 因此只需指定 $r=(r_0,\dots,r_{n-1})\in\mathbb{R}^n$

<!-- p.28 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**自相关系数的傅里叶变换**</span>

自相关系数的傅里叶变换（Fourier transform）为

```math
R(\omega)=\sum_\tau e^{-j\omega\tau}r_\tau=r_0+\sum_{t=1}^{n-1}2r_t\cos\omega t=|H(\omega)|^2
```

- 对所有 $\omega$ 总有 $R(\omega)\ge 0$
- 可以把幅度指标写成

```math
L(\omega)^2\le R(\omega)\le U(\omega)^2,\qquad \omega\in[0,\pi]
```

……对 $r$ 是凸的（convex in $r$）。

<!-- p.29 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**谱因子分解（spectral factorization）**</span>

问题（question）：$r\in\mathbb{R}^n$ 何时是某个 $h\in\mathbb{R}^n$ 的自相关系数？

<span style="color:#2471a3;">**[theorem]**</span> <span style="color:#00838f;">**答案（谱因子分解定理，spectral factorization theorem）：当且仅当对所有 $\omega$ 都有 $R(\omega)\ge 0$。**</span>

- 谱因子分解条件（spectral factorization condition）对 $r$ 是凸的
- 谱因子分解已有许多算法，即求出满足 $R(\omega)=|H(\omega)|^2$ 的某个 $h$

<span style="color:#2471a3;">**[algorithm]**</span> **基于自相关系数的幅度设计（magnitude design via autocorrelation coefficients）：**

- 以 $r$ 为变量（代替 $h$）
- 加入谱因子分解条件：对所有 $\omega$，$R(\omega)\ge 0$
- 对 $r$ 做优化
- 用谱因子分解恢复 $h$

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 谱因子分解定理是本讲把"非凸"扭成"凸"的枢纽，它与 Strang 线代（正定性 ↔ 半定规划）的联系值得专门记一条：非负三角多项式 $R(\omega)=\sum_\tau r_\tau e^{-j\omega\tau}\ge 0$（对所有 $\omega$）等价于以 $r$ 为系数的 Toeplitz 矩阵半正定（Fejér–Riesz 定理的矩阵表述），而"半正定 ⇔ 存在平方根因子"正是正定矩阵 Cholesky 分解 $A=LL^{\mathsf T}$ 的推广——定理断言的"存在 $h$ 使 $R=|H|^2$"就是把 $R$ 开平方得到的谱因子，堪称多项式世界的 Cholesky 因子。这一换元的威力在于：变量从 $h$ 换成 $r$ 后，$R$ 是 $r$ 的线性函数，幅度指标 $L^2\le R\le U^2$ 变成线性约束，唯一的非凸性被压缩成一条谱因子分解条件。下一页的 log-Chebychev 重述也顺理成章：约束 $D^2/t\le R\le tD^2$ 的左半边是双曲（旋转二阶锥）约束，且 $R(\omega)\ge D(\omega)^2/t>0$ 自动蕴含 $R\ge 0$，一条约束同时完成拟合与可行性。对比直接以 $h$ 为变量时的 $|H|\ge L$（绝对值下界，非凸）：换好变量，问题就凸了一半。

<!-- p.30 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**log-Chebychev 幅度设计（log-Chebychev magnitude design）**</span>

选择 $h$ 以最小化

```math
\max_\omega\left|20\log_{10}|H(\omega)|-20\log_{10}D(\omega)\right|
```

- $D$ 是期望的传递函数幅度（对所有 $\omega$ 有 $D(\omega)>0$）
- 即求最小最大（minimax）对数（dB）拟合

重新表述（reformulate）为：

```math
\begin{aligned}
\operatorname{minimize}\quad & t\\
\text{subject to}\quad & D(\omega)^2/t\le R(\omega)\le t\,D(\omega)^2,\qquad 0\le\omega\le\pi
\end{aligned}
```

- 对变量 $r,t$ 是凸的
- 约束中已包含谱因子分解条件

<!-- p.31 -->

### <span style="color:#2471a3;">**[section]**</span> <span style="color:#c0392b;">**示例：$1/f$（粉红噪声，pink noise）滤波器**</span>

<span style="color:#2471a3;">**[example]**</span> 示例：$1/f$（粉红噪声）滤波器（即 $D(\omega)=1/\sqrt{\omega}$），$n=50$，在 $0.01\pi\le\omega\le\pi$ 上做 log-Chebychev 设计。

图为设计结果：横轴 $\omega$ 与纵轴 $|H(\omega)|^2$ 均取对数刻度；最优拟合达到 $\pm 0.5$ dB。