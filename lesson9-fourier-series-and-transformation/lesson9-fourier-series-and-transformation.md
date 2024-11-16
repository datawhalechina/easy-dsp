# 前言
本章介绍了数字信号处理中的重要数学工具——傅里叶变换。傅里叶变换可以将时域信号转变为频域信号，从而将一些复杂的时域信号拆解为多个频率分量的叠加，使信号变得容易分析。本章首先介绍离散信号从时域到频域的转换工具傅里叶级数，然后推广到连续信号的傅里叶变换，接着介绍了卷积定理并说明了应用领域。
# 傅里叶级数
## 定义推导及系数计算方法
傅里叶级数是一种将周期函数表示为一系列正弦和余弦函数之和的数学工具，它在信号处理、物理学等众多领域有着广泛应用。
假设我们有一个周期为$2L$的函数$x(t)$，定义于区间$-L\leq t\leq L$。我们希望将它表示成正弦和余弦函数的组合形式，这就是傅里叶级数的由来。
首先，我们假设$x(t)$可以表示为：
$$x(t)=\frac{a_0}{2}+\sum_{n = 1}^{\infty}(a_n\cos(\frac{n\pi t}{L})+b_n\sin(\frac{n\pi t}{L}))$$
接下来推导系数$a_n$和$b_n$的计算方法。
为了求出$a_0$，我们对上述等式两边在区间$[-L, L]$上进行积分：
$$
\begin{align*}
\int_{-L}^{L}x(t)dt&=\int_{-L}^{L}\left(\frac{a_0}{2}+\sum_{n = 1}^{\infty}(a_n\cos(\frac{n\pi t}{L})+b_n\sin(\frac{n\pi t}{L}))\right)dt\\
&=\int_{-L}^{L}\frac{a_0}{2}dt+\sum_{n = 1}^{\infty}\left(\int_{-L}^{L}a_n\cos(\frac{n\pi t}{L})dt+\int_{-L}^{L}b_n\sin(\frac{n\pi t}{L})dt\right)
\end{align*}
$$
由于正弦函数和余弦函数在一个周期内的积分有特定的值（例如，$$\int_{-L}^{L}\cos(\frac{n\pi t}{L})dt = 0$$（当$n\neq0$），$$\int_{-L}^{L}\sin(\frac{n\pi t}{L})dt = 0$$），所以除了$$\int_{-L}^{L}\frac{a_0}{2}dt$$这一项，其余项的积分都为$0$。
则$$\int_{-L}^{L}x(t)dt=\int_{-L}^{L}\frac{a_0}{2}dt=a_0L$$
所以$$a_0=\frac{1}{L}\int_{-L}^{L}x(t)dt$$
为了求出$a_n$（$n\neq0$），我们将$x(t)$的表达式两边同时乘以$$\cos(\frac{m\pi t}{L})$$（这里$m$是一个整数且$m\neq0$），然后在区间$[-L, L]$上进行积分：
$$
\begin{align*}
\int_{-L}^{L}x(t)\cos(\frac{m\pi t}{L})dt&=\int_{-L}^{L}\left(\frac{a_0}{2}+\sum_{n = 1}^{\infty}(a_n\cos(\frac{n\pi t}{L})+b_n\sin(\frac{n\pi t}{L}))\right)\cos(\frac{m\pi t}{L})dt\\
&=\int_{-L}^{L}\frac{a_0}{2}\cos(\frac{m\pi t}{L})dt+\sum_{n = 1}^{\infty}\left(\int_{-L}^{L}a_n\cos(\frac{n\pi t}{L})\cos(\frac{m\pi t}{L})dt+\int_{-L}^{L}b_n\sin(\frac{n\pi t}{L})\cos(\frac{m\pi t}{L})dt\right)
\end{align*}
$$
同样，利用三角函数的正交性

例如，$$\int_{-L}^{L}\cos(\frac{n\pi t}{L})\cos(\frac{m\pi t}{L})dt = 0$$（当$n\neq m$），$$\int_{-L}^{L}\sin(\frac{n\pi t}{L})\cos(\frac{m\pi t}{L})dt = 0$$
可以得到：
当$m = n$时，$$\int_{-L}^{L}x(t)\cos(\frac{n\pi t}{L})dt=a_n\int_{-L}^{L}\cos^2(\frac{n\pi t}{L})dt=a_nL$$
因为$$\int_{-L}^{L}\cos^2(\frac{n\pi t}{L})dt = L$$
所以$$a_n=\frac{1}{L}\int_{-L}^{L}x(t)\cos(\frac{n\pi t}{L})dt$$
同理，为了求出$b_n$，我们将$x(t)$的表达式两边同时乘以$$\sin(\frac{m\pi t}{L})$$
然后在区间$[-L, L]$上进行积分，经过类似的推导过程，可以得到$$b_n=\frac{1}{L}\int_{-L}^{L}x(t)\sin(\frac{n\pi t}{L})dt$$
## 示例：计算给定函数（如理想方波）的傅里叶级数
假设我们有一个理想方波函数$x(t)$，周期为$2L$，在区间$-L\leq t\leq L$上定义为：
$$x(t)=\begin{cases}
1, & -L/2\leq t\leq L/2\\
0, & t < -L/2 \text{ 或 } t > L/2
\end{cases}$$
首先计算$a_0$：
$$
\begin{align*}
a_0&=\frac{1}{L}\int_{-L}^{L}x(t)dt\\
&=\frac{1}{L}\int_{-L/2}^{L/2}1dt\\
&=\frac{1}{L}\times L\\
&=1
\end{align*}
$$
接下来计算$a_n$：
$$
\begin{align*}
a_n&=\frac{1}{L}\int_{-L}^{L}x(t)\cos(\frac{n\pi t}{L})dt\\
&=\frac{1}{L}\int_{-L/2}^{L/2}1\times\cos(\frac{n\pi t}{L})dt\\
&=\frac{1}{L}\times\left[\frac{L}{n\pi}\sin(\frac{n\pi t}{L})\right]_{-L/2}^{L/2}\\
&=\frac{1}{n\pi}\left(\sin(\frac{n\pi}{2})-\sin(-\frac{n\pi}{2})\right)\\
&=\frac{2}{n\pi}\sin(\frac{n\pi}{2})
\end{align*}
$$
最后计算$b_n$：
$$
\begin{align*}
b_n&=\frac{1}{L}\int_{-L}^{L}x(t)\sin(\frac{n\pi t}{L})dt\\
&=\frac{1}{L}\int_{-L/2}^{L/2}1\times\sin(\frac{n\pi t}{L})dt\\
&=\frac{1}{L}\times\left[-\frac{L}{n\pi}\cos(\frac{n\pi t}{L})\right]_{-L/2}^{L/2}\\
&=0
\end{align*}
$$
所以，理想方波函数的傅里叶级数为：
$$x(t)=\frac{1}{2}+\sum_{n = 1}^{\infty}\frac{2}{n\pi}\sin(\frac{n\pi}{2})\cos(\frac{n\pi t}{L})$$
分析傅里叶级数展开后的结果：从这个展开式可以看出，理想方波可以由一个直流分量（$\frac{1}{2}$）和一系列不同频率的余弦函数组成。随着$n$的增加，各频率成分的幅度逐渐减小。在信号频谱分析中，这意味着我们可以通过傅里叶级数将时域中的方波信号分解为不同频率的成分，从而了解其频谱特性，比如哪些频率成分占主导地位等，这对于分析信号在不同频率下的传输、处理等情况非常重要。
# 傅里叶变换
## 定义及性质
傅里叶变换是一种将时域函数转换为频域函数的数学变换，它在信号处理领域具有极其重要的地位。
函数$x(t)$的傅里叶变换$X(\omega)$定义为：
$$X(\omega)=\int_{-\infty}^{\infty}x(t)e^{-j\omega t}dt$$
其反傅里叶变换$x(t)$定义为：
$$x(t)=\frac{1}{2\pi}\int_{-\infty}^{\infty}X(\omega)e^{j\omega t}d\omega$$
傅里叶变换具有许多重要的性质，比如线性、时移、频移、卷积定理等，这些性质使得它在分析信号的频域特性时非常方便。
在信号频域分析中的重要性：通过傅里叶变换，我们可以将时域中的复杂信号转换为频域中的函数，从而更直观地了解信号的频率成分、能量分布等特性。例如，对于一个包含多种频率成分的音频信号，通过傅里叶变换可以清晰地看到哪些频率段的能量较强，哪些较弱，这对于音频处理如降噪、均衡等操作具有重要的指导意义。
## 示例：计算弦波、理想脉冲函数等的傅里叶变换
### 弦波：
假设我们有一个弦波函数$x(t)=A\cos(\omega_0 t)$，我们来计算它的傅里叶变换。
根据欧拉公式$$e^{j\theta}=\cos\theta + j\sin\theta$$
我们可以将$x(t)$改写为：
$$x(t)=\frac{A}{2}(e^{j\omega_0 t}+e^{-j\omega_0 t})$$
然后计算其傅里叶变换：
$$
\begin{align*}
X(\omega)&=\int_{-\infty}^{\infty}\frac{A}{2}(e^{j\omega_0 t}+e^{-j\omega_0 t})e^{-j\omega t}dt\\
&=\frac{A}{2}\int_{-\infty}^{\infty}(e^{j(\omega_0-\omega) t}+e^{-j(\omega_0+\omega) t})dt
\end{align*}
$$
当$\omega=\omega_0$时，$$\int_{-\infty}^{\infty}e^{j(\omega_0-\omega) t}dt=\infty$$
当$\omega\neq\omega_0$时，$$\int_{-\infty}^{\infty}e^{j(\omega_0-\omega) t}dt = 0$$
同理对于$$e^{-j(\omega_0+\omega) t}$$
所以，$$X(\omega)=\pi A[\delta(\omega-\omega_0)+\delta(\omega+\omega_0)]$$
这里$\delta(\omega)$是狄拉克函数。
这表明弦波的傅里叶变换在$$\omega=\omega_0$和$\omega = -\omega_0$$处有冲激，即弦波的能量集中在这两个频率点上。

**理想脉冲函数：**

假设我们有一个理想脉冲函数$$\delta(t)$$
其定义为$$\delta(t)=0$$（当$t\neq0$），且$$\int_{-\infty}^{\infty}\delta(t)dt = 1$$
计算其傅里叶变换：
$$
\begin{align*}
X(\omega)&=\int_{-\infty}^{\infty}\delta(t)e^{-j\omega t}dt\\
&=e^{-j\omega\times0}\\
&=1
\end{align*}
$$
这表明理想脉冲函数的傅里叶变换是一个常数$1$，意味着它包含了所有频率成分且能量均匀分布在整个频域。
## 通过图形展示信号在时域和频域的对应关系：
以弦波为例，在时域中，弦波是一个周期性的波形，其频率和幅度可以直观地看到。在频域中，根据上述计算结果，我们可以看到在$\omega=\omega_0$和$\omega = -\omega_0$处有冲激，这就直观地展示了时域中的弦波信号在频域中的频率分布情况，即能量集中在特定的频率点上。
## 傅里叶变换的平移定理
### 第一平移定理
证明：

设$$F\{f(t)\}=F(\omega)=\int_{-\infty}^{\infty}f(t)e^{-j\omega t}dt$$
那么$$F\{f(t - t_0)\}=\int_{-\infty}^{\infty}f(t - t_0)e^{-j\omega t}dt$$
令$$u = t - t_0$$
则$$t = u + t_0$$
且$$dt = du$$
$$
\begin{align*}
F\{f(t - t_0)\}&=\int_{-\infty}^{\infty}f(u)e^{-j\omega (u + t_0)}du\\
&=\int_{-\infty}^{\infty}f(u)e^{-j\omega u}e^{-j\omega t_0}du\\
&=F(\omega)\cdot e^{-j\omega t_0}
\end{align*}
$$
#### 举例说明在信号处理中的应用：
假设我们有一个信号$$f(t)$$
其傅里叶变换为$$F(\omega)$$
现在我们想知道将这个信号在时域中延迟$t_0$后的频域特性。

根据第一平移定理，我们只需将原来的傅里叶变换结果$$F(\omega)$$乘以$$e^{-j\omega t_0}$$即可得到延迟后的信号$$f(t - t_0)$$的傅里叶变换。

这在分析信号传输过程中的延迟对频域特性的影响等方面非常有用。
### 第二平移定理
证明：

设$$F\{f(t)\}=F(\omega)=\int_{-\infty}^{\infty}f(t)e^{-j\omega t}dt$$
$$
\begin{align*}
F\{f(t)\cdot e^{j\omega_0 t}\}&=\int_{-\infty}^{\infty}f(t)\cdot e^{j\omega_0 t}e^{-j\omega t}dt\\
&=\int_{-\infty}^{\infty}f(t)e^{-j(\omega-\omega_0) t}dt\\
&=F(\omega - \omega_0)
\end{align*}
$$
#### 举例说明在信号处理中的应用：
假设我们有一个信号$f(t)$，其傅里叶变换为$F(\omega)$，现在我们想给这个信号在时域中乘以一个频率为$\omega_0$的复指数函数$e^{j\omega_0 t}$，然后了解其频域特性。根据第二平移定理，我们只需将原来的傅里叶变换结果$F(\omega)$的频率变量$\omega$偏移$\omega_0$即可得到$f(t)\cdot e^{j\omega_0 t}$的傅里叶变换。

这在信号调制等方面有着重要的应用，比如在通信系统中，通过乘以不同频率的复指数函数可以实现信号的频率调制。
# 卷积定理
卷积定理是傅里叶分析中的一个重要定理，它表明函数卷积的傅里叶变换是函数傅里叶变换的乘积。反之，函数乘积的傅里叶变换是函数傅里叶变换的卷积。
所以，可以通过卷积定理来求傅里叶变换，也可以通过傅里叶变换来求卷积，从而简化计算。
## 时域卷积定理
设$f_1(t)$和$f_2(t)$是两个时域函数，$F_1(\omega)$和$F_2(\omega)$分别是它们的傅里叶变换。

**时域卷积定理表示为：**

如果$$y(t) = f_1(t)*f_2(t)=\int_{-\infty}^{\infty}f_1(\tau)f_2(t - \tau)d\tau$$
那么$$Y(\omega)=F_1(\omega)F_2(\omega)$$
例如，假设有两个函数$$f_1(t)=e^{-at}u(t)（a>0，u(t)是单位阶跃函数）和f_2(t)=e^{-bt}u(t)$$（$b > 0$）。

首先求$F_1(\omega)$，根据傅里叶变换的公式$$F(\omega)=\int_{-\infty}^{\infty}f(t)e^{-j\omega t}dt$$
对于$f_1(t)=e^{-at}u(t)$，其傅里叶变换$F_1(\omega)=\frac{1}{a + j\omega}$。

同理，$f_2(t)$的傅里叶变换$$F_2(\omega)=\frac{1}{b + j\omega}$$
它们卷积后的函数$y(t)$的傅里叶变换$$Y(\omega)=F_1(\omega)F_2(\omega)=\frac{1}{(a + j\omega)(b + j\omega)}$$
## 频域卷积定理
**频域卷积定理表示为：**

如果$y(t)=f_1(t)f_2(t)$，那么$$Y(\omega)=\frac{1}{2\pi}F_1(\omega)*F_2(\omega)=\frac{1}{2\pi}\int_{-\infty}^{\infty}F_1(\eta)F_2(\omega-\eta)d\eta$$
例如，对于两个矩形脉冲函数$f_1(t)$和$f_2(t)$，它们在时域相乘后的函数$y(t)$的傅里叶变换$Y(\omega)$可以通过频域卷积定理来计算。矩形脉冲函数的傅里叶变换是$sinc$函数，利用频域卷积定理可以求出两个矩形脉冲乘积后的频谱特性。

证明：
设$f(t)$和$g(t)$是两个函数，它们的卷积$f*g$定义为：
$$f*g(t)=\int_{-\infty}^{\infty}f(\tau)g(t-\tau)d\tau$$
其傅里叶变换为：
$$
\begin{align*}
F\{f*g\}&=\int_{-\infty}^{\infty}(f*g)(t)e^{-j\omega t}dt\\
&=\int_{-\infty}^{\infty}\left(\int_{-\infty}^{\infty}f(\tau)g(t-\tau)d\tau\right)e^{-j\omega t}dt\\
\end{align*}
$$
令$u = t-\tau$，则$\tau = t - u$，且$dt = du$
$$
\begin{align*}
F\{f*g\}&=\int_{-\infty}^{\infty}\left(\int_{-\infty}^{\infty}f(t - u)g(u)du\right)e^{-j\omega t}dt\\
&=\int_{-\infty}^{\infty}\left(\int_{-\infty}^{\infty}f(t - u)g(u)du\right)e^{-j\omega (t - u)}e^{-j\omega u}dt\\
&=\int_{-\infty}^{\infty}\left(\int_{-\infty}^{\infty}f(t - u)g(u)du\right)e^{-j\omega t}e^{j\omega u}e^{-j\omega u}dt\\
&=\int_{-\infty}^{\infty}\left(\int_{-\infty}^{\infty}f(t - u)g(u)du\right)e^{-j\omega t}dt\\
&=\int_{-\infty}^{\infty}f(t - u)g(u)du\int_{-\infty}^{\infty}e^{-j\omega t}dt\\
&=F\{f\}\cdot F\{g\}
\end{align*}
$$
## 应用领域
### 信号处理领域：
在滤波操作中，卷积定理可以帮助理解滤波器（其冲激响应函数与输入信号进行卷积）对信号频谱的改变。例如，设计一个低通滤波器，通过卷积定理可以分析它是如何衰减高频分量的。

滤波器可以看作是一个系统，其对输入信号的处理是通过卷积运算实现的。从时域角度看，设输入信号为$x(t)$，滤波器的冲激响应为$h(t)$，那么输出信号$y(t)$可以表示为$$y(t)=x(t)*h(t)=\int_{-\infty}^{\infty}x(\tau)h(t - \tau)d\tau$$

根据卷积定理，时域的卷积对应频域的乘法。如果$X(\omega)$、$H(\omega)$和$Y(\omega)$分别是$x(t)$、$h(t)$和$y(t)$的傅里叶变换，那么$$Y(\omega)=X(\omega)H(\omega)$$
这里的$H(\omega)$就像是一个“频谱修改器”，它会改变输入信号的频谱$X(\omega)$来得到输出信号的频谱$Y(\omega)$。

**以低通滤波器为例**：

低通滤波器的作用是让低频信号通过，衰减高频信号。在频域中，低通滤波器的频率响应$H(\omega)$具有这样的特性：对于低频部分$$\vert\omega\vert < \omega_c$$
$\omega_c$为截止频率，$$H(\omega)\approx1$$
对于高频部分$$\vert\omega\vert\geq\omega_c$$
有$H(\omega)\approx0$。

**对于高通滤波器**：

其频率响应$H(\omega)$在高频部分
$$\vert\omega\vert > \omega_c$$
接近于$1$，在低频部分
$$\vert\omega\vert\leq\omega_c$$接近于$0$。

当输入信号通过高通滤波器时，根据$$Y(\omega)=X(\omega)H(\omega)$$
输入信号频谱中的低频部分会被衰减，高频部分会被保留，输出信号在时域上就会突出高频成分。

**对于带通滤波器**：

带通滤波器的频率响应$H(\omega)$在一个特定的频率区间
$$\omega_1 < \vert\omega\vert < \omega_2$$
接近于$1$，在这个区间之外接近于$0$。通过卷积定理可以理解，输入信号频谱中只有在$$\omega_1 < \vert\omega\vert < \omega_2$$这个区间的频率成分会在输出信号频谱中被保留，其他部分被衰减，从而实现了对特定频率范围信号的提取。

### 图像处理领域：
图像的模糊（比如高斯模糊）其实是图像与一个模糊核进行卷积的过程。利用卷积定理可以在频域中更高效地实现这种模糊操作，并且可以分析模糊对图像频谱的影响，从而更好地进行图像复原等操作。