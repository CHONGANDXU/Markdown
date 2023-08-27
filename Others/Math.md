**常用带佩亚诺余项的麦克劳林公式**（泰勒）

$$
\begin{align}
e^x&= 1+x+\frac{x^2}{2 !}+\cdots+\frac{x^n}{n !}+o\left(x^n\right) \\
\sin x &= x-\frac{x^3}{3 !}+\frac{x^5}{5 !}+\cdots+(-1)^{n-1} \frac{x^{2 n-1}}{(2 n-1) !}+o\left(x^{2 n}\right) \\
\cos x &= 1-\frac{x^2}{2 !}+\frac{x^4}{4 !}+\cdots+(-1)^n \frac{x^{2 n}}{(2 n) !}+o\left(x^{2 n}\right) \\
\ln (1+x) &= x - \frac{x^2}{2} + \frac{x^3}{3}+\cdots+(-1)^{n-1}\frac{x^n}{n}+o \left(x^n\right) \\
(1+x)^\alpha &= 1+\alpha x+\frac{\alpha(\alpha-1)}{2 !} x^2+\frac{\alpha(\alpha-1)(\alpha-2)}{3 !} x^3+\cdots +\frac{\alpha(\alpha-1) \cdots(\alpha-n+1)}{n !} x^n+o\left(x^n\right)
\end{align}
$$

**常用等价无穷小**
$$
当 x \rightarrow 0 时 \\
\begin{align}
x &\sim \sin x \sim \tan x \sim \arctan x \sim \arcsin x \sim \ln (1+x) \sim e^x-1 \\
(1+x)^\alpha -1 &\sim \alpha x \qquad 1-\cos ^\alpha x \sim \frac{\alpha}{2} x^2 \qquad a^x-1 \sim x \ln a \\
x- \sin x &\sim \frac{x^3}{6} \qquad \tan x - x \sim \frac{x^3}{3} \qquad x - \ln (1+x) \sim \frac{x^2}{2} \\
\arcsin x - x &\sim \frac{x^3}{6} \qquad x - \arctan x \sim \frac{x^3}{3}
\end{align}
$$

**重要结论一**
$$
若 f(x)在 x=0 的某邻域内连续，且当 x\rightarrow 0 时, f(x)是 x 的 m 阶无穷小，\varphi(x) 是 x 的 n 阶无穷小,则当 x\rightarrow0 时,F(x)= \int^{\varphi(x)}_{0} f(t)dt 是 x 的 n(m+1) 阶无穷小) \\
以上结论举例： \\
\alpha = \int^{x}_{0} \cos t^2 dt \qquad \beta = \int^{x^2}_{0} \tan \sqrt{t}dt \qquad \gamma = \int^{\sqrt{x}}_{0} \sin t^3dt \\
1*(0+1)=1 \qquad 2*(\frac{1}{2}+1)=3 \qquad \frac{1}{2}*(3+1)=2
$$
**n阶导数的莱布尼兹公式**
$$
设u=u(x),v=v(x),均n阶可导,则:\\
\begin{align}
[u \pm v]^{(n)}&=u^{(n)} \pm v^{(n)}\\
(uv)^{(n)}&=\complement_{n}^{0}u^{(n)}v^{(0)}+ \complement_{n}^{1}u^{(n-1)}v' + \cdots + \complement_{n}^{k}u^{(n-k)}v^{(k)}+ \cdots + \complement_{n}^{n-1}u'v^{(n-1)} + \complement_{n}^{n}u^{(0)}v^{(n)} \\
&= \sum_{k=0}^{n}\complement_{n}^{k}u^{(n-k)}v^{(k)}
\end{align}
\\
ps: u^{(0)}=u \qquad v^{(0)}=v
$$
