**泰勒一般展开公式**
$$
f(x)=f(x_0)+f'(x_o)(x-x_0)+\frac{f''(x_0)}{2!}(x-x_0)^2+\cdots+\frac{f^{(n)}(x_0)}{n!}(x-x_0)^n+o[(x-x_0)^n]
$$

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


![](http://i0.hdslb.com/bfs/new_dyn/2dea2d8b6214f5f0c2b0dfc3030aa07619147132.png)

![](http://i0.hdslb.com/bfs/new_dyn/9384cefbacca1651bdbbbe0dad14fec619147132.png)

![](http://i0.hdslb.com/bfs/new_dyn/6b8ca7d74ca96f68cc5a4e032991e70b19147132.png)

![](http://i0.hdslb.com/bfs/new_dyn/5b5de5b34f8e958dec1b77376aad909219147132.png)

![](http://i0.hdslb.com/bfs/new_dyn/acc2245606003efb045abde41dc663eb19147132.png)

![](http://i0.hdslb.com/bfs/new_dyn/62ff69eda4c4aed2bbd47a4538e90a9219147132.png)

![](http://i0.hdslb.com/bfs/new_dyn/32477d68755f3d143e43843390611a3919147132.png)

![](http://i0.hdslb.com/bfs/new_dyn/d06969cda2a7b02532ebe5d33271533b19147132.png)

![](http://i0.hdslb.com/bfs/new_dyn/2f899a2c221f766eb23d3155858ada6819147132.png)
