> <span style="color:#7f8c8d;">Source: 补充材料/Filter Design and Equalization 滤波器设计.pdf · Convex Optimization — Boyd & Vandenberghe, Stanford SEE · 行内双语翻译版</span>

# 凸优化 Convex Optimization（Boyd & Vandenberghe，Stanford EE364A）

<!-- p.1 -->

<span style="color:#2471a3;">**[lecture]**</span>

## <span style="color:#c0392b;">**Filter Design and Equalization（滤波器设计与均衡）**</span>

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">**Filter design（滤波器设计）**</span>

本讲主题导览：

- FIR 滤波器（FIR filters）
- Chebychev 设计（Chebychev design）
- 线性相位滤波器设计（linear phase filter design）
- 均衡器设计（equalizer design）
- 滤波器幅度指标（filter magnitude specifications）

<!-- p.2 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">**FIR filters（FIR 滤波器）**</span>

<span style="color:#2471a3;">**[definition]**</span>

<span style="color:#00838f;">**FIR（finite impulse response，有限冲激响应）滤波器**定义为：</span>

```math
y(t) = \sum_{\tau=0}^{n-1} h_\tau\, u(t-\tau), \qquad t \in \mathbb{Z}
```

- （序列，sequence）$u:\mathbb{Z}\to\mathbb{R}$ 是输入信号（input signal）
- （序列，sequence）$y:\mathbb{Z}\to\mathbb{R}$ 是输出信号（output signal）
- $h_i$ 称为滤波器系数（filter coefficients）
- $n$ 称为滤波器阶数（filter order）或长度（length）

<!-- p.3 -->

<span style="color:#2471a3;">**[definition]**</span>

<span style="color:#00838f;">**频率响应（frequency response）**：$H:\mathbb{R}\to\mathbb{C}$，</span>

```math
H(\omega) = h_0 + h_1 e^{-i\omega} + \cdots + h_{n-1} e^{-i(n-1)\omega}
          = \sum_{t=0}^{n-1} h_t \cos(t\omega) \;+\; i \sum_{t=0}^{n-1} h_t \sin(t\omega)
```

- 电机工程（EE）传统用 $j=\sqrt{-1}$ 代替 $i$
- $H$ 是周期的（periodic）且共轭对称（conjugate symmetric）的，因此只需知道/指定 $0 \le \omega \le \pi$ 上的取值

**FIR 滤波器设计问题（FIR filter design problem）**：选择 $h$，使它以及 $H$ 满足/优化给定指标（specs）。

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 这里的频率响应本质上是一种「变换」：把冲激响应 $h$ 的系数信息编码为 $\omega$ 的复值函数 $H(\omega)=\sum_t h_t e^{-i\omega t}$。你在概率论里已经见过同构的思想——随机变量的特征函数 $\mathbb{E}[e^{itX}]$ 同样把分布信息编码成复指数的加权和；两者共享同一条结构定理：时域卷积对应频域相乘，正如独立随机变量之和的特征函数等于各自特征函数之积。更重要的是凸性的来源：固定 $\omega_k$ 之后，$H(\omega_k)$ 的实部与虚部都是 $h$ 的线性函数，于是形如 $|H(\omega_k)|\le t$ 的约束是二阶锥（second-order cone）约束——这是后面滤波器设计能归入 SOCP 的全部原因。反过来，若改用带反馈的 IIR 滤波器，$H(\omega)$ 会成为系数的有理分式，凸性立即丢失。建议亲手验证一次「$|H(\omega)|^2$ 关于 $h$ 非凸、但关于 $r$ 凸」这一计算，能切实体会「凸性取决于变量的表示方式」这句话的分量。

<!-- p.4 -->

<span style="color:#2471a3;">**[example]**</span>

### <span style="color:#c0392b;">**Example: lowpass FIR filter（例：低通 FIR 滤波器，阶数 $n=21$）**</span>

图为冲激响应（impulse response）$h$ 的图示：横轴 $t$ 从 $0$ 到 $20$，纵轴 $h(t)$ 大致在 $-0.2$ 与 $0.2$ 之间取值。

<!-- p.5 -->

延续上例的两张图：

- 频率响应幅度（即 $|H(\omega)|$）：横轴 $\omega$ 刻度 $0$ 至 $3$（$\pi\approx 3.14$，故曲线终止于 $\pi$），纵轴为对数刻度 $10^{-3}$ 至 $10^{1}$
- 频率响应相位（即 $\angle H(\omega)$）：横轴同上，纵轴约 $-3$ 至 $3$（弧度）

<!-- p.6 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">**Chebychev design（Chebychev 设计）**</span>

<span style="color:#2471a3;">**[algorithm]**</span>

优化问题为

```math
\operatorname{minimize}\quad \max_{\omega\in[0,\pi]}\; \bigl|H(\omega) - H_{\mathrm{des}}(\omega)\bigr|
```

- $h$ 是优化变量（optimization variable）
- $H_{\mathrm{des}}:\mathbb{R}\to\mathbb{C}$ 是（给定的）期望传递函数（desired transfer function）
- 是凸问题（convex problem）
- 可以添加约束，例如 $|h_i|\le 1$

对频率采样（sample / discretize frequency）后：

```math
\operatorname{minimize}\quad \max_{k=1,\dots,m}\; \bigl|H(\omega_k) - H_{\mathrm{des}}(\omega_k)\bigr|
```

- 采样点 $0\le\omega_1<\cdots<\omega_m\le\pi$ 固定（例如 $\omega_k = k\pi/m$）
- $m \gg n$（常用经验法则：$m = 15n$）
- 这给出上述问题的近似（松弛，relaxation）

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 「Chebychev 设计」得名自俄国数学家 Pafnuty Chebyshev（1821–1894）：他在 1850 年代研究多项式最佳逼近时主张以最大偏差（而非平方偏差）衡量优劣，即 minimax 思想；经典的 Chebyshev 交错定理还指出最优逼近的误差应当等波纹地振荡——第 14 页例子的设计结果正是等波纹形状。从凸分析角度看，$\max_k |H(\omega_k)-H_{\mathrm{des}}(\omega_k)|$ 是有限个凸函数的逐点最大值，其上图（epigraph）是一族半空间之交，与 LP 可行域结构同源，因此离散化后能写成 LP/SOCP。采样密度 $m=15n$ 是一条折中：$m$ 太小会在网格点之间留下漏检的波峰波谷，$m$ 太大则徒增约束数量；而且只要 $m$ 有限，它就是原半无限问题（无穷多约束）的松弛——求出的系数是否真正满足连续约束，还需事后在更细的网格上复核。

<!-- p.7 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">**Chebychev design via SOCP（经 SOCP 的 Chebychev 设计）**</span>

<span style="color:#2471a3;">**[algorithm]**</span>

```math
\begin{array}{ll}
\operatorname{minimize} & t \\
\operatorname{subject\ to} & \| A^{(k)} h - b^{(k)} \| \le t, \qquad k = 1, \dots, m
\end{array}
```

其中

```math
A^{(k)} =
\begin{bmatrix}
1 & \cos\omega_k & \cdots & \cos(n-1)\omega_k \\
0 & -\sin\omega_k & \cdots & -\sin(n-1)\omega_k
\end{bmatrix}, \quad
b^{(k)} =
\begin{bmatrix}
\Re H_{\mathrm{des}}(\omega_k) \\
\Im H_{\mathrm{des}}(\omega_k)
\end{bmatrix}, \quad
h =
\begin{bmatrix}
h_0 \\ \vdots \\ h_{n-1}
\end{bmatrix}
```

<!-- p.8 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">**Linear phase filters（线性相位滤波器）**</span>

<span style="color:#2471a3;">**[definition]**</span>

设（suppose）：

- $n = 2N+1$ 为奇数
- 冲激响应关于中点对称（symmetric about midpoint）：$h_t = h_{n-1-t}$，$t = 0, \dots, n-1$

则

```math
H(\omega) = h_0 + h_1 e^{-i\omega} + \cdots + h_{n-1} e^{-i(n-1)\omega}
          = e^{-iN\omega}\bigl(2h_0\cos N\omega + 2h_1\cos(N-1)\omega + \cdots + h_N\bigr)
          \;\triangleq\; e^{-iN\omega}\,\bar H(\omega)
```

<!-- p.9 -->

- 项 $e^{-iN\omega}$ 表示 $N$ 个样本的延迟（delay）
- $\bar H(\omega)$ 是实值的
- $|H(\omega)| = |\bar H(\omega)|$
- 称为线性相位滤波器（linear phase filter）（$\angle H(\omega)$ 除 $\pm\pi$ 的跳变外是线性的）

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 线性相位的价值在于「不色散」：若 $\angle H(\omega)=-N\omega$（差一个常数），则每个频率分量经历相同的 $N$ 样本延迟，输出只是输入波形的整体平移，不发生形状畸变；这对通信、图像等对波形形状敏感的场合至关重要。而「系数对称 $\Rightarrow$ $\bar H(\omega)$ 实值」这条推导，值得与 Strang 线性代数中的现象对照：实对称矩阵的特征值必为实数——共轭对称（Hermitian）结构总是保住「实性」；把 $h_t=h_{n-1-t}$ 看成序列层面的镜像对称，复相位因子被整体剥离为 $e^{-iN\omega}$，剩下的振幅函数就只含余弦项、自然是实的。工程实践中还有一个不对称现象：人耳对相位畸变相对不敏感，所以音频均衡器常可容忍非线性相位；但数据通信的判决环节对相位畸变极其敏感，因此信道均衡器几乎总要设计成（近似）线性相位。这也解释了下一节为何能直接对实值的 $\bar H(\omega_k)$ 施加线性约束。

<!-- p.10 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">**Lowpass filter specifications（低通滤波器指标）**</span>

<span style="color:#2471a3;">**[definition]**</span>

指标图示：横轴 $\omega$ 上标出 $\omega_p$（通带边缘）与 $\omega_s$（阻带边缘）及 $\pi$；幅度方向标出 $\delta_1$ 与 $1/\delta_1$（通带纹波的上下界）以及 $\delta_2$（阻带电平）。

设计思想（idea）：

- 让通带（passband）$[0, \omega_p]$ 内的频率分量通过
- 阻断阻带（stopband）$[\omega_s, \pi]$ 内的频率分量

<!-- p.11 -->

具体指标（specifications）：

- 最大通带纹波（maximum passband ripple）（以 dB 计为 $\pm 20\log_{10}\delta_1$）：

```math
1/\delta_1 \;\le\; |H(\omega)| \;\le\; \delta_1, \qquad 0 \le \omega \le \omega_p
```

- 最小阻带衰减（minimum stopband attenuation）（以 dB 计为 $-20\log_{10}\delta_2$）：

```math
|H(\omega)| \;\le\; \delta_2, \qquad \omega_s \le \omega \le \pi
```

<!-- p.12 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">**Linear phase lowpass filter design（线性相位低通滤波器设计）**</span>

<span style="color:#2471a3;">**[algorithm]**</span>

- 对频率采样（取 $\omega_k$）
- 不失一般性（wlog）可设 $\bar H(0) > 0$，于是纹波指标写成

```math
1/\delta_1 \;\le\; \bar H(\omega_k) \;\le\; \delta_1
```

以最大阻带衰减为目标的设计（design for maximum stopband attenuation）：

```math
\begin{array}{ll}
\operatorname{minimize} & \delta_2 \\
\operatorname{subject\ to} & 1/\delta_1 \le \bar H(\omega_k) \le \delta_1, \qquad 0 \le \omega_k \le \omega_p \\
& -\delta_2 \le \bar H(\omega_k) \le \delta_2, \qquad \omega_s \le \omega_k \le \pi
\end{array}
```

<!-- p.13 -->

- 通带纹波 $\delta_1$ 给定
- 这是关于变量 $h$、$\delta_2$ 的一个 LP
- 自 1960 年代起即已知晓（并被实际使用）
- 可以添加其他约束，例如 $|h_i|\le\alpha$

变化与扩展（variations and extensions）：

- 固定 $\delta_2$，最小化 $\delta_1$（凸，但不是 LP）
- 固定 $\delta_1$ 与 $\delta_2$，最小化 $\omega_s$（拟凸，quasiconvex）
- 固定 $\delta_1$ 与 $\delta_2$，最小化阶数 $n$（拟凸）

<!-- p.14 -->

<span style="color:#2471a3;">**[example]**</span>

### <span style="color:#c0392b;">**Example（例：线性相位低通滤波器）**</span>

- 线性相位滤波器，$n = 21$
- 通带 $[0, 0.12\pi]$；阻带 $[0.24\pi, \pi]$
- 最大纹波 $\delta_1 = 1.012$（$\pm 0.1$ dB）
- 以最大阻带衰减为目标设计

图为冲激响应 $h$：横轴 $t$（$0$–$20$），纵轴约 $-0.2$ 至 $0.2$，系数关于中点 $t=10$ 对称（即 $h_t = h_{20-t}$）。

<!-- p.15 -->

上例的两张图：

- 频率响应幅度 $|H(\omega)|$：横轴刻度 $0$–$3$（终止于 $\pi$），纵轴对数刻度 $10^{-3}$–$10^{1}$；通带内围绕 $1$ 等波纹，进入阻带后迅速下落
- 频率响应相位 $\angle H(\omega)$：除 $\pm\pi$ 跳变外近似线性（斜率约 $-10$，对应 $N=10$ 个样本延迟）

<!-- p.16 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">**Equalizer design（均衡器设计）**</span>

<span style="color:#2471a3;">**[definition]**</span>

幻灯片为串联框图：信号先经过 $G(\omega)$，再经过均衡器 $H(\omega)$。

均衡（equalization）：给定

- $G$（未均衡的频率响应，unequalized frequency response）
- $G_{\mathrm{des}}$（期望的频率响应，desired frequency response）

设计（FIR 均衡器，FIR equalizer）$H$，使得

```math
\tilde G \;\triangleq\; GH \;\approx\; G_{\mathrm{des}}
```

- 常见选择：$G_{\mathrm{des}}(\omega) = e^{-iD\omega}$（纯延迟，delay）
- 也就是说，均衡就是反卷积（deconvolution）（至多差一个延迟）
- 可以对 $H$ 添加约束，例如 $|h_i|$ 的上限或 $\max_\omega |H(\omega)|$ 的上限

<!-- p.17 -->

**Chebychev 均衡器设计（Chebychev equalizer design）：**

```math
\operatorname{minimize}\quad \max_{\omega\in[0,\pi]}\; \bigl|\tilde G(\omega) - G_{\mathrm{des}}(\omega)\bigr|
```

该问题是凸的；对频率采样后为 SOCP。

<!-- p.18 -->

**时域均衡（time-domain equalization）**：直接优化均衡后系统的冲激响应 $\tilde g$。

例如取 $G_{\mathrm{des}}(\omega) = e^{-iD\omega}$，则期望的冲激响应为

```math
g_{\mathrm{des}}(t) =
\begin{cases}
1, & t = D \\
0, & t \ne D
\end{cases}
```

采样设计（sample design）：

```math
\begin{array}{ll}
\operatorname{minimize} & \max_{t\ne D}\; |\tilde g(t)| \\
\operatorname{subject\ to} & \tilde g(D) = 1
\end{array}
```

- 这是一个 LP
- 也可以改用 $\sum_{t\ne D} \tilde g(t)^2$ 或 $\sum_{t\ne D} |\tilde g(t)|$（作为目标函数）

<!-- p.19 -->

扩展（extensions）：

- 可以施加（凸）约束
- 可以混合时域与频域指标
- 可以均衡多个系统，即选择 $H$ 使 $G^{(k)} H \approx G_{\mathrm{des}}$，$k = 1, \dots, K$
- 可以均衡多输入多输出（multi-input multi-output）系统（即 $G$ 与 $H$ 是矩阵）
- 可推广到多维系统，例如图像处理

<!-- p.20 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">**Equalizer design example（均衡器设计例）**</span>

<span style="color:#2471a3;">**[example]**</span>

未均衡系统 $G$ 是 10 阶 FIR：图为 $g(t)$，横轴 $t = 0, \dots, 9$，纵轴约 $-0.4$ 至 $1$。

<!-- p.21 -->

上例的两张图：

- 幅度 $|G(\omega)|$：横轴刻度 $0$–$3$（终止于 $\pi$），纵轴对数刻度 $10^{-1}$–$10^{1}$
- 相位 $\angle G(\omega)$：纵轴约 $-3$–$3$ 弧度

设计任务：构造 30 阶 FIR 均衡器，使得 $\tilde G(\omega) \approx e^{-i10\omega}$。

<!-- p.22 -->

**Chebychev 均衡器设计：**

```math
\operatorname{minimize}\quad \max_{\omega}\; \bigl|\tilde G(\omega) - e^{-i10\omega}\bigr|
```

均衡后系统的冲激响应 $\tilde g$：横轴 $t = 0, \dots, 35$，纵轴约 $-0.2$ 至 $1$，在 $t = 10$ 附近出现主峰。

<!-- p.23 -->

均衡后频率响应的两张图：

- 幅度 $|\tilde G|$：横轴刻度 $0$–$3$（终止于 $\pi$），纵轴对数刻度 $10^{-1}$–$10^{1}$，在感兴趣的频段内贴近 $1$
- 相位 $\angle\tilde G$：除 $\pm\pi$ 跳变外近似线性，对应 10 个样本延迟

<!-- p.24 -->

**时域均衡器设计（time-domain equalizer design）：**

```math
\operatorname{minimize}\quad \max_{t\ne 10}\; |\tilde g(t)|
```

均衡后系统的冲激响应 $\tilde g$：横轴 $t = 0, \dots, 35$，纵轴约 $-0.2$ 至 $1$；按构造，该设计直接压低 $t \ne 10$ 处的所有旁瓣幅度。

<!-- p.25 -->

时域设计的均衡后频率响应两张图：

- 幅度 $|\tilde G|$：横轴刻度 $0$–$3$（终止于 $\pi$），纵轴对数刻度 $10^{-1}$–$10^{1}$
- 相位 $\angle\tilde G$：近似线性

两种设计侧重不同：第 22–23 页在频域上做 Chebychev 极小极大，本页设计则在时域上极小化旁瓣，二者给出的均衡器特性各有所长。

<!-- p.26 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">**Filter magnitude specifications（滤波器幅度指标）**</span>

<span style="color:#2471a3;">**[definition]**</span>

<span style="color:#00838f;">传递函数幅度指标（transfer function magnitude specification）形如：</span>

```math
L(\omega) \;\le\; |H(\omega)| \;\le\; U(\omega), \qquad \omega \in [0, \pi]
```

其中 $L, U:\mathbb{R}\to\mathbb{R}_+$ 给定。

- 下界对滤波器系数 $h$ 不是凸的
- 出现在许多应用中，例如音频（audio）、频谱整形（spectrum shaping）
- 可以通过变量代换（change of variables）改用凸优化求解

<!-- p.27 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">**Autocorrelation coefficients（自相关系数）**</span>

<span style="color:#2471a3;">**[definition]**</span>

<span style="color:#00838f;">与冲激响应 $h = (h_0, \dots, h_{n-1})\in\mathbb{R}^n$ 相关联的自相关系数（autocorrelation coefficients）定义为：</span>

```math
r_t \;=\; \sum_{\tau} h_\tau h_{\tau+t}
```

（约定：当 $k<0$ 或 $k\ge n$ 时取 $h_k = 0$）

- $r_t = r_{-t}$；当 $|t| \ge n$ 时 $r_t = 0$
- 因此只需指定 $r = (r_0, \dots, r_{n-1})\in\mathbb{R}^n$

<!-- p.28 -->

自相关系数的傅里叶变换（Fourier transform）为

```math
R(\omega) \;=\; \sum_{\tau} e^{-i\omega\tau} r_\tau \;=\; r_0 + \sum_{t=1}^{n-1} 2 r_t \cos(\omega t) \;=\; |H(\omega)|^2
```

- 对所有 $\omega$ 总有 $R(\omega) \ge 0$
- 于是幅度指标可以写成

```math
L(\omega)^2 \;\le\; R(\omega) \;\le\; U(\omega)^2, \qquad \omega \in [0, \pi]
```

……它在 $r$ 中是凸的。

<!-- p.29 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">**Spectral factorization（谱分解）**</span>

<span style="color:#2471a3;">**[theorem]**</span>

**问题（question）**：什么时候 $r\in\mathbb{R}^n$ 是某个 $h\in\mathbb{R}^n$ 的自相关系数？

**答案（answer）**：（谱分解定理，spectral factorization theorem）当且仅当对所有 $\omega$ 都有 $R(\omega) \ge 0$。

- 谱分解条件对 $r$ 是凸的
- 存在许多谱分解（spectral factorization）算法，即寻找满足 $R(\omega) = |H(\omega)|^2$ 的 $h$

基于自相关系数的幅度设计（magnitude design via autocorrelation coefficients）：

- 以 $r$ 作为变量（代替 $h$）
- 添加谱分解条件：对所有 $\omega$，$R(\omega) \ge 0$
- 对 $r$ 做优化
- 再用谱分解恢复 $h$

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 谱分解定理在信号处理语境下就是 Fejér–Riesz 定理（Fejér 与 Riesz，1910 年代）：非负三角多项式必可写成某个序列之傅里叶变换的模平方。用矩阵语言看（衔接 Strang 线性代数的半正定概念）：把 $r_0,\dots,r_{n-1}$ 排成 Toeplitz 矩阵，则「对所有 $\omega$ 有 $R(\omega)\ge 0$」恰好等价于该矩阵半正定，而「半正定 ⟺ 特征值全非负」正是你在 18.065 里熟悉的判据；半定规划（SDP）的可行域正是由这类矩阵不等式围成的。概率论这边也有同构：$r_t$ 就是宽平稳随机过程的自协方差序列，「非负定序列 ⟺ 存在谱密度」对应 Herglotz/Bochner 定理，学随机过程时会再遇到同一副骨架。方法论上，本页展示的是凸优化的经典套路：换到使约束凸的坐标系（用 $r$ 而非 $h$）里做优化，最后执行一次非凸但良定义的还原（谱分解）——凸的部分交给求解器，非凸但有显式算法的部分单独处理。

<!-- p.30 -->

<span style="color:#2471a3;">**[section]**</span>

### <span style="color:#c0392b;">**Log-Chebychev magnitude design（对数 Chebychev 幅度设计）**</span>

<span style="color:#2471a3;">**[algorithm]**</span>

选择 $h$ 以最小化

```math
\max_{\omega}\; \bigl|\, 20\log_{10}|H(\omega)| \;-\; 20\log_{10} D(\omega) \,\bigr|
```

- $D$ 是期望的传递函数幅度（对所有 $\omega$，$D(\omega) > 0$）
- 即求极小极大对数（dB）拟合（minimax logarithmic fit）

重新表述（reformulate）为

```math
\begin{array}{ll}
\operatorname{minimize} & t \\
\operatorname{subject\ to} & D(\omega)^2/t \;\le\; R(\omega) \;\le\; t\, D(\omega)^2, \qquad 0 \le \omega \le \pi
\end{array}
```

- 关于变量 $r$、$t$ 是凸的
- 该约束已包含谱分解条件

> <span style="color:#1e8449;">**[note] Note（译者注）:**</span> 为什么用 dB（对数）而不是线性幅度来定指标？生理上，人耳对响度的感知接近对数律，通信链路预算也以 dB 记账；数学上，取对数把「比值」变成「差」：上界 $R \le tD^2$ 化为线性约束，下界 $D^2/t \le R$ 等价于 $tR \ge D^2$（$t, R \ge 0$），是旋转二阶锥（rotated second-order cone）的标准形，仍属凸约束。注意 $D^2/t \le R$ 的左端恒为正，因此这组约束自动蕴含谱分解条件 $R(\omega)\ge 0$——这正是幻灯片说「constraint includes spectral factorization condition」的原因：一组不等式同时完成了幅度拟合与可行性保证。更深的方法论启示与几何规划（geometric programming）同源：凸性不是问题的固有属性，而是「表述方式」的属性——同一个设计问题，变量取 $h$ 时非凸、取 $r$ 时凸、再取对数后连 dB 域的 minimax 也凸了。遇到非凸问题，先问一句「换一组变量能否变凸」，往往比直接跑局部方法划算得多。

<!-- p.31 -->

<span style="color:#2471a3;">**[example]**</span>

### <span style="color:#c0392b;">**Example: 1/f filter（例：$1/f$ 滤波器）**</span>

$1/f$（粉红噪声，pink noise）滤波器示例：取 $D(\omega) = 1/\sqrt{\omega}$，$n = 50$，在 $0.01\pi \le \omega \le \pi$ 上做 log-Chebychev 设计。

图为双对数坐标下的 $|H(\omega)|^2$ 对 $\omega$ 的曲线，紧贴目标 $1/\omega$ 的形状。

最优拟合（optimal fit）：$\pm 0.5$ dB。
