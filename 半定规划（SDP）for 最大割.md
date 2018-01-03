# 半定规划（SDP）for 最大割

最大割问题:

Instance: given an undirected graph G(V,E), find a bipartition of V into S and T that maximizing the size of the cut E(S,T)=$\{uv\in E|u\in S,v\in T\}$

贪心算法、随机选择、局部查找算法能够达到的approximation ratio 都是$\frac{1}{2}$.

LP for Max-cut：

首先我们可以得到一个非线性的规划：

$max\ \ \ \ \ \sum_{uv\in E}y_{uv}$

$s.t.\ \ \ \ \ \ y_{uv}\le |x_u-x_v|,\ \ \ \ \ \ \forall uv\in E$

​             $x_v\in\{0,1\},\ \ \ \ \ \ \forall v \in V$

$y_{uv}$是对G中所有边进行赋值，如果u，v同属于一个sub-graph，则$|x_u-x_v|=0$, then $y_{uv}=0$, otherwise $y_{uv}=1$.

引入三角关系对：

可以得到一个线性的规划：

$max\ \ \ \ \  \sum_{uv\in E}y_{uv}$

$s.t.\ \ \ \ \ y_{uv}\le y_{uw}+y_{wv}\ \ \ \ \ \forall u,v,w \in V$

​            $y_{uv}+y_{uw}+y_{wv}\le 2\ \ \ \ \ \forall u,v,w\in V$

​            $y_{uv}\in\{0,1\}\ \ \ \ \ \forall u,v\in V$

可以计算出这个线性规划和整数规划的gap=2. 因此用线性规划的方法永远也达不到比$\frac{1}{2}$好的结果。

二次规划quadratic program：

$max \ \ \ \ \ \ \sum_{uv\in E}y_{uv}$

$s.t. \ \ \ \ \ y_{uv}\le \frac{1}{2}(1-x_ux_v)\ \ \ \ \ \forall uv\in E$

​            $x_v\in\{-1,1\}\ \ \ \  \ \forall v\in V$

二次规划是很NP-complete问题，因此可以把它relax to半定规划SDP，通过求出半定规划的最优解，然后rounding出二次规划的可行解，然后求出这个解的ratio。

SDP:

$max \ \ \ \ \ \frac{1}{2}\sum_{uv\in E}(1-\langle\boldsymbol x_u,\boldsymbol x_v\rangle)$

s.t	      $\langle\boldsymbol x_u,\boldsymbol x_v\rangle=1,\ \ \ \ \forall v\in V$

​	      $\boldsymbol x_v\in \boldsymbol R,\ \ \ \ \ \forall v\in V$

假设已经通过SDP的凸优化解法求出了SDP的optimal solution，记作：$\boldsymbol x_v^{*}$

现在需要找一个rounding的策略来求出可行解。

给出一个rounding方法：

随机找单位向量$\boldsymbol u$，使得$\boldsymbol u\in\boldsymbol R^n,\|\boldsymbol u\|_2=1$

然后让sol：$\hat x_v=sgn(\langle\boldsymbol x_v^*,\boldsymbol v\rangle)$

什么意思呢，就是找一个单位向量，然后让这个单位向量和optimal solution求出的向量来进行运算，如果则两个向量之间的家教小于180度，那么让最终的解为1，如果大于180度，置为0.

如何找这个单位向量？

random $\boldsymbol r=(r_1,r_2,\dots,r_n)\in \boldsymbol R^n$, where each $r_i\sim N(0,1)$ i.i.d.

就是把$\boldsymbol u$里面的每一个方向都用二项分布随机找出来。

最后来求approximation ratio。

$E[cut]=\sum_{uv\in E}Pr[sgn(\langle\boldsymbol x_u^*,\boldsymbol r\rangle)\not=sgn(\langle\boldsymbol x_v^*,\boldsymbol r\rangle)]$

​	    $=\sum_{uv\in E}\frac{\theta_{uv}}{\pi}$

​	    $=\sum_{uv\in E}\frac{arccos\langle\boldsymbol x_u^*,\boldsymbol x_v^*\rangle}{\pi}$

在MICHEL X. GOEMANS 1995年的论文inproved approximation  algorithms for maximum cut and satisfiability problems using semidefinite programmming证明了：

$\sum_{uv\in E}\frac{arccos\langle\boldsymbol x_u^*,\boldsymbol x_v^*\rangle}{\pi}\ge\alpha\sum_{uv\in E}\frac{1}{2}(1-\langle\boldsymbol x_u^*,\boldsymbol x_v^*\rangle)$

其中：

$\alpha=inf_{x\in[-1,1]}\frac{2arccos(x)}{\pi(1-x)}=0.87856\dots$

这就证明了利用半定规划能够达到的approximation ratio。



